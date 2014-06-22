---
layout: post
title:  "Storm with stateful-ness"
date:   2014-06-22 14:34:10
author: Abhishek Bhattacharjee
categories: jekyll update
---
I came across [Storm](https://storm.incubator.apache.org/) while I was trying to find ideas to work on for my final year engineering project.
Storm, is basically a real-time distributed system which doesn't store data for processing it. It processes data on the go.
Flowing data(real-time data) is caught with the help of a component called spout and processed through a chain of bolts.
Spout and bolts are terms native to the world of Storm. Spout is basically a logical component to fetch data and bolt is 
the componenet that works on that data.

As the title mentions **_state_**, those acquainted with the concept of **Distributed System** would know what it means :-)
Anyways, Storm doesn't maintain state and rightly so ! As it is a real-time distributed system which is meant for on the fly processing, storing state
would slow down the execution of tasks as it creates another overhead (i.e strong state) for the system.

So, as a result if a task fails in-between, Storm starts it from the scratch and not resumes it from where it stopped. Now consider a task which has
just started and there's some failure, it is not a big deal to restart the task as it had just started so much work wasn't done. Athough, if the task 
fails at the midpoint of its execution or just when it was about to finish then the cost incurred in re-doing the task could prove to be costlier
than maintaining the overhead of state.

So, we(me and my group-mates) thought we could improve the situation if we have some information, which we can use to resume a task from the point 
it stopped,stored. In other words, we thought of storing the state if the system or rather the processing units of the system, bolts. 
This idea of stateful bolts was already proposed in the issues list in Storm's github page.
Although, there was not much work in that area because saving state of a real-time distributed system was not very appealing ;-)

Anyways, we decided to go ahead with this idea and pursued it as our final year project. We used two two other technologies Kafka which was 
used at the spout end and redis as the database to store the state. Using an in-memory database was necessary as we needed fast retrieval 
in case of failures. More about Kafka and Redis can be read on their respective websites.

We wrote our own client for making storm talk to kafka using the kafka apis. Although, there already exists a library for kafka-storm interaction.
But we preferred to use our own. We used redis's jedis api for saving state using storm. We needed to divide the data we receive into batches so that
we can commit the batches and not every single data packet(or tuple). The batch size can be defined by the developer so one batch could have 
several no of tuples or even a single tuple. To facilitate this feature we used transactional topologies which storm provides.
Transactional topologies have now been replaced by trident topologies. Transactional topologies uses the concept of ACID properties.
In a transactional topology, tasks can be run parallely but commit happens serially. Here, commit refers to completion of a task and sending a
signal of its comletion to the parent spout. Commit is generally handled by a separate bolt called an ICommiter bolt. We don't need to go into much
detail thought. We also use this bolt to store the state. 

We wrote two topologies, one with stateful-ness and the other as stateless. We made them fail at equal rate and we observed the rate of processing data 
in both cases. We found that our method was faster compared to the de-facto method of processing data without saving states.
Thus, we got what we expected from our project. We were able to provide some improvement in storms performance in case of failures.

We learned a lot while doing this project and also got some awards :-). It feels good when your work is appreciated. 

###And we lived happily ever after !###

