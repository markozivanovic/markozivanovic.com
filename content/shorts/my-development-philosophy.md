+++
author = "Marko Zivanovic"
title = "My development philosophy"
date = "2021-08-18"
tags = [
    "development", "philosophy"
]
categories = [
    "shorts"
]
+++

I was reading a book by Ashley Davis on bootstrapping microservices*, and at the beginning of the book, the author sums up his philosophy of development as:

- Iterate
- Keep it working
- Move from simple to complex

Fair enough! That sounds like a reasonable dev philosophy. Even if I found something in there I dislike, I can't argue with the man. It's his philosophy, and it's his right for it to be whatever he likes it to be!

Reading those three bullet points made me realize that I don't have a development philosophy, and I've been developing for over a decade! It's time for me to sum up my dev philosophy!

My development philosophy summed up, v0.0.1:

- Explore and prepare
- Break down the complexity to micro implementable chunks
- Develop with end-users in mind
- Optimize and iterate
- Keep shipping

### 1) Explore and prepare

I'm not the one to jump into the codebase without reading the manual first. Based on my anecdotes, that makes me a minority. People usually jump right into the code and figure out the bigger picture afterward. I prefer to have the big picture (if possible) before I start changing things.

### 2) Break down the complexity to micro implementable chunks

Decomposition is my key to handling complex systems. It's a no-brainer, but it's fascinating how often people forget about it when they take on a problem. I always try to break things down to the smallest possible unit of work when I'm working on a project.

### 3) Develop with end-users in mind

It's easy to forget when writing software that actual people will use it. Often, you want to finish the damn thing as quickly as you can and continue with your life. In the end, it's all about your fellow human beings. That mantra makes me a better developer, or I'd like to think so.

### 4) Optimize and iterate

I nearly didn't add this one because it's so obvious and implied for most developers. I did choose to add it because it's one of the biggest and arguably the most important parts of the development process.

### 5) Keep shipping

Unless it's something done for the pure enjoyment of doing it, software should be shipped and used. Perfect is the enemy of good. Waiting for all the stars to align to publish your code is rarely a good thing to do. Ship early, ship often.

What's your philosophy, if you have one?

\* <a href="https://www.manning.com/books/bootstrapping-microservices-with-docker-kubernetes-and-terraform" target="_blank">Bootstrapping Microservices with Docker, Kubernetes, and Terraform by Ashley Davis</a>