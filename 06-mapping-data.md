# 06 Sending Data to Shaders

There are 5 ways to connect data from your application to SPIR-V shaders

### 1. Input Attributes

This is only possible with vertex shaders. It involves declaring the input attributes in the shader and creating the input slots when creating VkPipeline objects, and before drawing, binding the VkBuffer objects that contain the data to the input slots.

### 2. Descriptors

A resource descriptor can be used to map data such as uniform buffers, storage buffers, and samplers to any shader stage.



Descriptors are grouped together into descriptor sets, which are then bound to the pipeline. The descriptor sets are bound to the pipeline before drawing, and the data is then available to the shader.

### 3. Push Constants

A push constant is a small bank of values accessible in shaders. Push constants allow the application to set values used in shaders without creating buffers or modifying and binding descriptor sets for each update.

### 4. Specialization Constants


### 5. Physical Storage Buffer
