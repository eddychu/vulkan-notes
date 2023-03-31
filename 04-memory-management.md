## Memory Management 

** this is a draft **

Memory are rudimentally divided into two types: device memory and host memory. Lossely speaking, Device memory is the memory that is visible to the GPU. Host memory is the memory that is visible to the CPU. 

## Host Memory

Vulkan utilizes the host memory to store API's internal data structures. Vulkan provides a mechanism to allocate and free memory from the host memory for the application to use incase you don't explicitly specify a memory allocator.


## Device Memory

Device memory is the memory that is visible to the GPU. Image objects, buffer objects, and uniform buffers are all allocated and stored in device memory.

Device memory can be allocated in Vulkan using the vkAllocateMemory function. The memory type index is used to specify the type of memory to allocate. The memory type index is obtained by calling the vkGetPhysicalDeviceMemoryProperties function.

The allocated device memory using the vkAllocateMemory function is only visible to the device. The host can only access the those device memory regions that are mappable. that means the memory type index must have the VK_MEMORY_PROPERTY_HOST_VISIBLE_BIT bit set.

For device memory with the VK_MEMORY_PROPERTY_HOST_VISIBLE_BIT bit set, the host can access the memory using the vkMapMemory function. The host can then read and write to the memory using the returned pointer. When you are done updating the memory, you must call the vkUnmapMemory function to unmap the memory.





There a different types of memory in Vulkan.

## Device Local Memory

Device local memory is the memory that is visible to the GPU. This is the memory that you will use to store your vertex data, index data, uniform data, etc. 

This is where you put resources written and read frequently by the GPU. and infrequently by the CPU, for example, static mesh data that you can upload once and then read from the GPU.

## Host Visible Memory

This is typically the ram on you computer. If you access this memory from the GPU, it will go across the PCI bus, typically used as staging memory to upload data to the GPU.




There are two types of memory in Vulkan: device memory and host memory. Device memory is the memory that is visible to the GPU. Host memory is the memory that is visible to the CPU.

Vulkan requires you to manage the memory yourself. To do this, you will typically have a memory management system in your application to be responsible for allocating and freeing memory.

The memory management part is done "magically" in OpenGL by the drivers, and you don't have to worry about it. But in Vulkan, you have to do it yourself.

When you create a vulkan instance, there is a vkAllocationCallbacks structure that you can pass in to specify the memory allocation callbacks. 

This callback is how you specify your custom memory allocation functions. 

An example of a custom memory allocator class can be found in the book Vulkan Programming Guide Chapter 2.

Two resources in Vulkan you can find in almost any vulkan application are buffers and images.

Buffers are used to store vertex data, index data, uniform data, etc. Images are used to store textures, render targets, etc.

You can create a buffer by using vkCreateBuffer, and create an image by using vkCreateImage.







### Reference

Vulkan Programming Guide Chapter 2, Memory and Resources.

[Vulkan Memory Management](https://www.youtube.com/watch?v=gM93bbKQ0P8&ab_channel=Vulkan)