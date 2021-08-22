# GSOC'21 Final Report | Chapel
## Project Details

* **Student:** Divye Nayyar
* **Project:** Go-Style Channels
* **Organisation:** [Chapel](https://chapel-lang.org/)
* **Mentors:** [Michael Ferguson](https://github.com/mppf), [Aniket Mathur](https://github.com/Aniket21mathur)

## Abstract
Chapel is a programming language designed for productive parallel computing at scale. It is designed to be highly portable and can run on multiple architectures. This allows chapel to be used in a highly parallel environment and implement complex algorithms. It would require the chapel users to specify synchronization and communication patterns to their codes in addition to the problem logic. This would make the code less readable and lead to inefficiency, unwanted race conditions, and incorrect results. Channels will solve this problem by effectively taking care of communication and synchronization between various tasks and thus, would enable the user to write more efficient and clean code.

## Introduction
### What are Go Channels?
Channel is an important built-in feature in Go. They provide a mechanism for concurrently executing functions to communicate by sending and receiving values of a specified element type.  Channels make goroutines share memory by communicating. We can view a channel as an internal FIFO (first in, first out) queue within a program. Some goroutines send values to the queue (the channel) and some other goroutines receive values from the queue. Please see the official documentation of [Go Channels](https://gobyexample.com/channels) to learn more.

### The Project
The aim of the project was to add a communication mechanism to Chapel similar to Go channels. The main challenges while implementing the project were to maximise thread efficiency, and detect the situations in which race conditions and deadlocks can occur and figure out how to prevent them. 

My implementation of this project was divided into three notable milestones :

#### 1) Implement basic channel operations
This step involved designing and implementing Waiter class which are used to store the information about suspended tasks. I implemented a Doubly-Ended Queue to handle multiple waiters in a First-In-First-Out manner. The next task was to implement a class that would support the channel send, receive and close operations. Each channel instance has independent waiter queues to keep track of suspended operations on it. The last thing was to add an iterator that would receive values from the channel until it is closed.

#### 2) Select statements
The select statement is used to choose from multiple send/receive channel operations. The select statement blocks until one of the send/receive operations is ready. Each case of the select statement is a channel send or receive operation or it is a default case which is used to make a select call non-blocking. I implemented the select runtime function which avoids deadlock and starvation with other concurrently running select calls. The syntax to be used for the select statements in chapel is yet to be decided. 

#### 3) Performance Testing
The final phase of the project included testing the performance of Chapel channels and comparing it with the benchmarks written in Golang.

## Interface of the module
![example!](https://user-images.githubusercontent.com/55523247/130367586-12962c00-52c4-459f-bd2d-726ddd8d78b0.png)

## Contributions to the main repository
| Pull Request | Description |
|----|----|
| [#17954: [Design] Go-Style Channels API review](https://github.com/chapel-lang/chapel/pull/17954)  | <ul> <li> It consists of the document used for the API review of the channels module </li> <li> It contains information about the user facing functions for channels </li> <li> It also contains the information about select statements and other imporatant design proposals </li></ul> | 
| [#18168: [GSOC] Go-Style Channels](https://github.com/chapel-lang/chapel/pull/18168) | <ul><li> This PR includes the implementation of the channel class and select statements. </li> <li> It also contains the tests written for examining correctness and performance of various channel operations. </li></ul> |

## Further Work
* Work with my mentors to merge the open Pull Requests to the main repository
* Add performance tests and fix anything that impacts performance unexpectedly.
* Continue to look for improvements for the channel module.
