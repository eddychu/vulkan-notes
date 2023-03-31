# Resource

Resource can be viewed as just a chunk of data. In Vulkan we are mainly dealing with two kinds of resources: buffers and images.

You can create a buffer by using vkCreateBuffer, and create an image by using vkCreateImage.

A vulkan application doesn't use the buffer and image objects directly, instead, you create a buffer view or image view to access the buffer or image.

Like the name suggests, a buffer view is a view of a buffer. and an image view is a view of an image. Think the raw buffer and image objects as meaningless raw data chunks, the buffer view and image view objects are there to help you define the format and layout of the data in the buffer or image.

