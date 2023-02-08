---
title: "使用 Golang 实现 worker pool"
date: 2023-02-08T16:00:00+08:00
draft: false
categories: [Concurrency,Channel]
tags: [Golang]
---

## 介绍
Golang 中的 Channel 可以用来处理并发，下面我们就使用 Channel 来实现一个并发异步任务 worker pool。maxWorkers 是最大并发数，JobQueue 为待执行 job 队列。

## 实现
```
package main

import (
	"fmt"
	"io"
	"log"
	"net/http"
	"strconv"
	"time"
)

func jobHandler(w http.ResponseWriter, req *http.Request) {
	id, _ := strconv.Atoi(req.URL.Query().Get("id"))

	// handle async logic
	job := Job{
		Handle: jobRunHandler,
		ID:     id,
	}
	JobQueue <- job

	io.WriteString(w, "do job ok!\n")
}

func jobRunHandler(params HandleParams) {
	time.Sleep(time.Second * 3)
	fmt.Printf("job %d executed \n\n", params.ID)
}

func main() {
	// init worker pool
	Setup()

	http.HandleFunc("/do-job", jobHandler)
	log.Fatal(http.ListenAndServe(":8081", nil))
}

var (
	maxWorkers   = 3 // The number of workers (currency number)
	maxQueueSize = 5 // The size of job queue

	JobQueue chan Job
)

func Setup() {
	// Create the job queue.
	JobQueue = make(chan Job, maxQueueSize)

	// Start the dispatcher.
	dispatcher := NewDispatcher(JobQueue, maxWorkers)
	dispatcher.run()
}

type HandleParams struct {
	ID int
}

// Job holds the attributes needed to perform unit of work.
type Job struct {
	Handle func(HandleParams)
	ID     int
}

// NewWorker creates takes a numeric id and a channel w/ worker pool.
func NewWorker(id int, workerPool chan chan Job) Worker {
	return Worker{
		id:         id,
		jobQueue:   make(chan Job),
		workerPool: workerPool,
		quitChan:   make(chan bool),
	}
}

type Worker struct {
	id         int
	jobQueue   chan Job
	workerPool chan chan Job
	quitChan   chan bool
}

func (w Worker) start() {
	go func() {
		for {
			// Add jobQueue to the worker pool.
			w.workerPool <- w.jobQueue

			select {
			case job := <-w.jobQueue:
				// Dispatcher has added a job to jobQueue.
				log.Printf("worker%d: started job_id %d\n", w.id, job.ID)

				// excute handler
				job.Handle(HandleParams{
					ID: job.ID,
				})

				log.Printf("worker%d: completed job_id %d!\n", w.id, job.ID)
			case <-w.quitChan:
				// We have been asked to stop.
				log.Printf("worker%d stopping\n", w.id)
				return
			}
		}
	}()
}

func (w Worker) stop() {
	go func() {
		w.quitChan <- true
	}()
}

// NewDispatcher creates, and returns a new Dispatcher object.
func NewDispatcher(jobQueue chan Job, maxWorkers int) *Dispatcher {
	workerPool := make(chan chan Job, maxWorkers)

	return &Dispatcher{
		jobQueue:   jobQueue,
		maxWorkers: maxWorkers,
		workerPool: workerPool,
	}
}

type Dispatcher struct {
	workerPool chan chan Job
	maxWorkers int
	jobQueue   chan Job
}

func (d *Dispatcher) run() {
	for i := 0; i < d.maxWorkers; i++ {
		worker := NewWorker(i+1, d.workerPool)
		worker.start()
	}

	go d.dispatch()
}

func (d *Dispatcher) dispatch() {
	for job := range d.jobQueue {
		go func(job Job) {
			log.Printf("fetching workerJobQueue for job %d\n", job.ID)
			workerJobQueue := <-d.workerPool
			log.Printf("adding job %d to workerJobQueue\n", job.ID)
			workerJobQueue <- job
		}(job)
	}
}
```

## 命令行并发测试
```
for i in {1..10}; do curl http://127.0.0.1:8081/do-job?id=$i ; done
```


## Ref
- [Go by Example: Worker Pools](https://gobyexample.com/worker-pools)
- [Worker pool to control concurrency and collect results](https://gist.github.com/harlow/49318d54f45d29f1a77cc641faf14054)
- [Job queues in Golang](https://gist.github.com/harlow/dbcd639cf8d396a2ab73)
