# Queues and Commands

Today I will try to figure out what is going on under the hood when we call `vkQueueSubmit`. We will start by looking at the concept of queues and commands, and then we will look at how to use them.

Each device in Vulkan has one or more queues, and they are grouped into **queue families** by their capabilities.

You can query the properties of each physical device's queue families and select the one that supports the operation you need. For example, if you want to render graphics, you need to find a queue family that supports graphics operations.

You need to specify the queue create info when you create a logical device. You don't create queues directly, but when you created a logical device, you can get the queues from the device by using **vkGetDeviceQueue**.

Work (rendering, compute, transfer) is represented as a sequence of commands that are recorded into **command buffers**, and subsequently submitted to queues by using **vkQueueSubmit**. 

Command buffers are allocated from a **command pool**, which is created by **vkCreateCommandPool**. 

After creating the command buffers and command pool, you can start recording commands into the command buffers. You can record commands into a command buffer by calling **vkBeginCommandBuffer** and **vkEndCommandBuffer**.

Please Be warned that you need to ensure there is no other thread recording commands into the same command buffer at the same time. 

For this reason, it is recommended to use a **single command buffer per thread**, and because command buffers are allocated from a command pool, it is better use a **single command pool per thread**.

We will not go into the details of how to record commands into a command buffer, and treat it as a black box for now. We will look at it in the future notes.

After you are done with a command buffer, you can free it by calling **vkFreeCommandBuffers**, which will give the command buffer back to the command pool for reuse.

But because the nature of graphics rendering is that you are basically doing the same set of operations over and over again, so instead of free and allocate command buffers every frame, you can use **vkResetCommandBuffer** to reset the command buffer to the initial state, and then record commands into it again.

You can also use **vkResetCommandPool** to reset all the command buffers in a command pool to the initial state. it is typically used at the end of a frame instead of reset individual command buffers.

To execute the commands in a command buffer, you need to submit it to a queue by calling **vkQueueSubmit**. 


You can specify a set of sempahores upon which to wait before executing the command buffer, and a set of semaphores to be signaled when the command buffer finishes execution. 

Note that the queue is expecting an array of command buffers, so you can submit multiple command buffers at the same time.

Also note that vkQueueSubmit is a non-blocking call, so it will return immediately, and the command buffer will be executed in the background.

To wait for the completion of the commands in this submission, you can specify a fence object to be signaled when the commands are finished. You can use **vkWaitForFences** to wait for the fence to be signaled.

You can also use **vkQueueWaitIdle** to blindly wait for all the commands in the queue to finish execution. Think of it as a fence that is signaled when the queue is empty.

But you should avoid using **vkQueueWaitIdle** as they are very expensive operations, and you should use fences instead. A suitable senario for using **vkQueueWaitIdle** is when you are shutting down the application, and you want to make sure all the commands in the queue are finished before you destroy the resources.
