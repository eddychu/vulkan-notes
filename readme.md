# Vulkan: The Big Picture

Vulkan is a low-level API for graphics and compute. It is as close to the hardware as possible, and is designed to provide a low-overhead interface for applications that need to do a lot of intensive graphics work.

While it gives you more control over the hardware, it is also more complex to use than OpenGL. It is not designed to be used by the uninitiated, and you need to really understand what you are doing to get the most out of it.

Because of this, Vulkan is not a good choice for beginners. If you are new to graphics programming, you should start with OpenGL or DirectX 11/12. When you are ready to move on, you can come back to Vulkan.

Before we start, it is better we can construct a simplified mental model of how Vulkan works. This will help us understand the concepts better.

Then entry point of any Vulkan application is an object called an instance. An instance is a connection to the Vulkan library. Think of it as a entry point to the library. It is used to create other objects, and to query information about the system.

A Vulkan **Instance** aggregates a set of **physical devices** (GPUs) that are at your desposal. Most of time, your application won't need to deal with physical devices directly. Instead, You will create a **logical device** from a physical one. 

A logical device is a wrapper around one physical devices. It is used to create objects that are tied to a specific physical device, such as buffers and images. You can create multiple logical devices from the same physical device.

Subsequently, each logical device will have multiple **queues**. Queues are used to submit commands to the GPU. Each queue is associated with a specific type of operation, such as graphics or compute. 

The sole purpose of a queue is to process work your application submits to it. The work in Vulkan's world is abtracted as a **command buffer**. You can think of it as a list of instructions that will be executed by the GPU. To create a command buffer, you need to allocate it from a **command pool**. Picture A command pool as a container for command buffers. It is used to allocate and free command buffers. 

So far, we have created an instance, a logical device, a command pool, and a command buffer. We know we need to record instructions into the command buffer, and submit it to a queue. But this only solves the logistics of our graphics application.

If you are familiar with OpenGL, these terms might scare you away. Think it in this way, Vulkan is just a more verbose/dumped down version of OpenGL. While OpenGL is smart enough to handle all those details for you, Vulkan is not. You need to do initialize all those objects yourself.

To render something, we need run our shader code on the GPU, and display the awesome triangle on the screen. This is where the **pipeline** comes in. There are two kinds of pipelines in Vulkan: Graphics and Compute. A graphics pipeline is used to render graphics, and a compute pipeline, as the name suggests, is only used to run compute shaders. 

You already understand what graphics pieline is if you have some experience with OpenGL. The only difference is that in Vulkan, you need to create a pipeline object, and then bind it to the command buffer, and you get to have different pipelines for different rendering tasks. While in OpenGL, you get a global pipeline state, all your gl* function calls will modify this global (pipeline) state.

By losing the global state, Vulkan gives you more control over the hardware. You can have multiple pipelines, and switch between them as you need. This is especially useful when you need to render different objects with different shaders.

Another key difference between the Vulkan and OpenGL api is that Vulkan is designed to work asynchronously. After you submit your commands to the queue, the function will return immediately without waiting for the GPU to finish executing the work. This begs the question:  how do you know when the GPU is done executing the command buffer? 

This is where the concept of **synchronization** comes in. There are two different aspects of synchronization at work: **host synchronization** and **device synchronization**. Host synchronization is used to synchronize work between the CPU and the GPU, such as waiting for the previous frame to be drawn. Device synchronization is used to synchronize work between multiple command buffers, such as waiting for a compute shader to finish before rendering the result.

To facilitate this, Vulkan api provides us with two different ways to synchronize work: **semaphores** and **fences**. You can use fences to synchronize work between the CPU and the GPU and use semaphores to synchronize work between command buffers.

A great example of this can be found in the [Vulkan Tutorial: Rendering_and_presentation](https://vulkan-tutorial.com/Drawing_a_triangle/Drawing/Rendering_and_presentation#page_Synchronization). Here you create two semaphores, one for the image acquisition, and one for the rendering. and A fence for the CPU-GPU synchronization.

So in simplified terms, a vulkan application will essentially do the following step by step:

1. create an instance

2. create a logical device from a physical device,

3. create a command pool

4. allocate a command buffer from the command pool

5. create a graphics pipeline with all the necessary state

6. for each frame:
 
   1. wait on the fence

   2. acquire an image from the swapchain

   3. record commands into the command buffer (set the pipeline, bind the vertex buffer, draw the triangle, etc.)

   3. submit the command buffer to a queue

   4. present the image to the screen

7. submit the command buffer to a queue


There are a lot of details that I have skipped over, but this is the general idea of how a Vulkan application works. If you work through the aforementioned tutorial, you will find that to draw a simple triangle, you need hundreds of lines of code. You can imagine how much more complex a real-world application will be. 

But don't be too overwhelmed by it. It might seem scary at first, but the only important stuff you need to know is actually in the drawframe function, The rest is just plumbing code that preparing the necessary objects. 

I find that the compiler vs interpreter analogy is very useful here. While OpenGL is an interpreter, Vulkan is a compiler. You need to "compile" first (prepare all the necessary objects), and then "execute" (submit the command buffer to the queue), while in OpenGL, you execute the commands dynamically.

Vulkan is a messy API. I think to build a simple model and understand the big picture will reduce the learning curve a lot. Hope this helps.















