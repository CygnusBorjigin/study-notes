# Compute

## Elastic Compute Cloud (EC2)

This is a service that allow user to rent virtual computers. When configuring the virtual machines (instances) we can customize the CPU, RAM, and Storages as well as optimizations.

One of the main features of EC2 is it can be paid on demand. 

Therefore, say we have a task that requires one EC2 instance $24$ hours to complete. Since each instance is paid on demand, we can have $24$ instances running together for one hour. This allows us to complete the task, paid the same amount, and save $23$ hours. 

One other main feature is that each instance is resizable. This is also call on demand sizing.

## Containers

When writing codes, it is very common to use libraries. However, if a library receives an update, that will mean multiple version of that library exists on the internet. 

To avoid the problem of a version mismatch, containers was created. A container packages the application and the libraries it depends on into a image that can be run on any computer. 

## Lambda

AWS Lambda is a service for server-less computing. Instead of having a server, operating system, language, and packages doing the work, it runs the code in respond to events. Therefore, there will no server to configure, maintain and pay for. 

The Lambda service is charged by the millisecond and the price depends on the amount of RAM used. 