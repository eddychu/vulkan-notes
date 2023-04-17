# Pipeline

There are two types of pipelines in Vulkan: graphics pipelines and compute pipelines.

## Graphics Pipeline

the pipeline begins at input assembler, where the input vertex data is assembled based on the primitive topology.

the vertex shader, which transforms the vertex data into clip space.

then the vertex data are tessellated by the tessellation control shader and tessellation evaluation shader.

then geometry shader is invoked, which can generate new primitives from the input primtives.

next, primtiive assembler takes all the transformed vertices and arrange them into primitive format. 

rasterization is the process of transforming the primitives into fragments.

the fragment shader is invoked for each fragment, which can generate a color for the fragment. these fragments finally become a part of the framebuffer, which is subjected to a number of tests (depth testing, stenciling, fragment blending) before being written to the framebuffer.

## Compute Pipeline

commonly used in the filed of image processing and physics computation.


## Three concepts of pipeline

three related concepts of pipeline are:  pipeline state objects, pipeline cache objects and pipeline layout objects.



