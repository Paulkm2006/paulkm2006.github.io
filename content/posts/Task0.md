+++ 
draft = false
date = 2024-10-21T12:50:24+08:00
title = "Task0 Notes"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++

## Process & Thread
### Process

In plain language, a single application. It owns dedicated memory and resource pool. Creation of processes are handled by OS, which takes more time and causes slower termination.

Processes are best used to complete a task that doesn't involve intensive data exchange within processes.
### Thread

Threads are minimal version of processes. They are created, managed and scheduled by a proces. Threads shares the same memory and resources, making data exchanges easy and fast.

We tend to use threads everywhere from simple for loops to handling web requests, as they are easy to create, easy to terminate and have efficient communicating pathways known as channels in Go.

## Concurrency & Parallelism
Both of the terms describes a way to process multiple requests using minimal time. However, their differences are also very obvious.
![img](/images/Task0/abeb21004d0aed99d460ecfbab8aab73.png)
For parallelism, a multi-core processor is required to perform true 'parallel' computing. It is done by distributing different tasks over the processor's cores, achieving lower execution time.
![img](/images/Task0/2190b36b6bb9edb821b825cf45f8bc32.png)
For concurrency, however, the execution time isn't reduced. It just breaks down each task into smaller chunks, and execute the chunks one by one. 

To observers, both methods can achieve simutaneous computing. Parallelism can better increase effitiency while concurrency have less trouble controlling data modification between tasks.

## Lock
In parallel computing, when we need to modify values in a database/array, we need to make sure the operation is exclusive.
The reason is that when multiple operations (eg. read-modify-write) at the same time may cause race condition, where multiple threads might read and write to the same resource simultaneously.
![img](/images/Task0/nolock.png)

To avoid these potential errors, we can implement lock mechanism on variables that might be modified during parallel computing. When one thread is making changes to the variable, we block other threads from modifying the save variables, while being able to perform other operations like read and calculate. This can ensure data integrity while allowing maximize performance.
![img](/images/Task0/lock.png)

## MVC
MVC separates a server into model, view and controller. 
### Model
Model handles everything that involves database and other api providers. 
### View
View is less important in backend development. We can simply apply a json formatter as view.
### Controller
Controller handles the request from frontend, redirect it to proper model and returns the value.


## JWT

In order to help multiple backend servers to identify a user, whether in a cluster or in different sites, the traditional session+cookie method will require an additional identity database, which can cause a lot of troubles when it goes down. So, in order to help simplify auth process and increase expansibility, JWT is introduced.

A typical JWT contains three parts: Header, Payload and Signature, separating by a dot.
### Header
Header is a json object encoded with BaseURL64 (a modified version of base64 that exclude some non-URI characters like = , + and /). It contains information about the algorithm used by signature and the type of the token (which is just "JWT")
### Payload
A standard payload might contain these 7 optional keywords: iss(uer), exp(iration time), sub(ject), aud(ience), nbf(Not before), iat(Issue at) and jti(JWT ID). Additionally, we can specify our own keywords like admin or userGroup.
### Signature
Signature helps to prevent unauthorized modification to the first two parts. It is generated from a secret, using the algorithm designated in header.

## RESTful API

A typical RESTful request should look like this.
![img](/images/Task0/restful_request.png)
Note that not all API endpoints may contain version info.

And a typical RESTful response should be a JSON string containing resources designated by the request method and resource type (e.g. info of a new object for POST, a collection for GET /collection and empty document for DELETE)

## Angular commit message

A well-formed commit message should contain these parts:
```<type>(<scope>): <subject>```
#### type
should be one of the following types:
1. feat(ure), a new feature
2. docs, modification to documents
3. fix, to fix a bug
4. ci/cd, modification to ci/cd script or configuration
5. pref, improve performance
also, style test revert and build are also valid types
#### scope
where the modification is implemented, usually the name of the module
#### subject
what does the modification try to do

