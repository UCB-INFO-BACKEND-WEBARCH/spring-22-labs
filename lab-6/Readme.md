# Lab 6 - Asynchronous Task Queues

## Intro to Async Queues
Whatever we have learnt till now in this course has revolved around synchronous execution of tasks. However, most of the real-world processes around us are actually asynchronous. 
Think about you ordering food and the time it takes for it to arrive at your doorstep.

In the world of APIs too, a lot of processes are asynchronous. Whenever we execute a piece of code which takes some time, the execution will most likely be asynchronous.
Consider the following example:
You are building a Data Visualization platform where the flow is something like:
    1. You upload a dataset
    2. You select a few features and parameters
    3. The platform creates a visualization for you and presents it as a dashboard
Sounds cool right?

But we know that datasets are big (especially in the age we live, data is available for chump change). So it is highly likely that the data upload will take some time. Moreover, once it is uploaded, it will take some time for the data to be processed for the platform to showcase relevant features/column names etc and then there will be more time required to actually create the visualization.
So what do we do when all these processes are taking place in the background? Do we stop the user from doing anything on the platform and make them see a loader for an hour? Doesn't sound right!

This is where Asynchronous Queus come in. Asynchoronous Queues are used to store and process tasks that will take up some time while the system can move on to doing other things rather than waiting for the previous process to finish executing. Once the task has finished executing, these queues usually have a `callback` functionality which notify that the processor that the task has been executed and then the processor can do what it was supposed to do with it.

Let's think about this through another example. When you go to a restaurant to order something, does the waiter/waitress stop by your table and wait while you look at the menu and decide what you want to eat? Not unless you have questions! They will go around and serve other tables while you decide. Once you have decided, you signal or call the waiter/waitress and convey your order.

The waiter/waitress in this case is an Async Queue while you and all the other guests at the table are tasks which are executing asynchronously.

## So why do we actually need Async Queues?
There are three main reasons:
<ol>
<li> Speed: When we’re talking to a third party API we have to face reality; unless that third party is physically located next to our infrastructure, there’s going to be latency involved. All it would take is the addition of a few API calls and we could easily end up doubling or tripling our response time, leading to a sluggish site and unhappy users. However if we push these API calls into our queue instead, we can return a response to our users immediately while our queues take as long as they like to talk to the API.</li>

<li>Reliability: We don’t live in a world of 100% uptime, services do go down, and when they do it’s important that our users aren’t the ones that suffer. If we were to make our API calls directly in the users requests we wouldn’t have any good options in the event of a failure. We could retry the call right away in the hope that it was just a momentary glitch, but more than likely we’ll either have to show the user an error, or silently discard whatever we were trying to do. Queues neatly get around this problem since they can happily continue retrying over and over in the background, and all the while our users never need to know anything is wrong.</li>

<li>Scalability. If we had a surge in requests that involved something CPU intensive like resizing images, we might have a problem if all of our apps were responsible for this. Not only would the increased CPU load slow down other image resize requests, it could very well slow down requests across the entire site. What we need to do is isolate this workload from the user’s experience, so that it doesn’t matter if it happens quickly or slowly. This is where queues shine. Even if our queues become overloaded, the rest of the site will remain responsive.</li>