## Copy to host visible buffer
| Vulkan | FrameGraph |
|---|---|
| vkMapMemory | FrameGraph::UpdateHostBuffer or FrameGraph::MapBufferRange |


## Copy data between resources
| Vulkan | FrameGraph |
|---|---|
| vkCmdCopyBuffer | CopyBuffer task |
| vkCmdCopyBufferToImage | CopyBufferToImage task |
| vkCmdCopyImageToBuffer | CopyImageToBuffer task |
| vkCmdCopyImage | CopyImage task |
| vkCmdBlitImage | BlitImage task |
| vkResolveImage | ResolveImage task |
| vkCmdCopyBuffer from host to device | UpdateBuffer task |
| vkCmdCopyImage from host to device | UpdateImage task |
| vkCmdCopyBuffer from device to host | ReadBuffer task |
| vkCmdCopyImage from device to host | ReadImage task |


## Pipelines
| Vulkan | FrameGraph |
|---|---|
| vkCreateGraphicsPipelines | FrameGraph::CreatePipeline with GraphicsPipelineDesc that contains shaders and reflection, RenderPassDesc contains all other render states, draw tasks contains frequently changed render states and dynamic states |
| vkCreateComputePipelines | FrameGraph::CreatePipeline with ComputePipelineDesc, compute tasks may override local group size |
| vkCreateGraphicsPipelines with mesh shader | FrameGraph::CreatePipeline with MeshPipelineDesc |
| vkCreateRayTracingPipelinesNV | FrameGraph::CreatePipeline with RayTracingPipelineDesc |
| vkCreatePipelineLayout | PipelineDescription::PipelineLayout when created pipeline |
| vkCreateDescriptorSetLayout | PipelineDescription::DescriptorSet when created pipeline |


## Descriptor set
| Vulkan | FrameGraph |
|---|---|
| VkDescriptorBufferInfo | PipelineResources::BindBuffer, PipelineResources::SetBufferBase to set base offset if used dynamic buffer offset |
| VkDescriptorImageInfo with sampler | PipelineResources::BindSampler |
| VkDescriptorImageInfo with image | PipelineResources::BindImage |
| VkDescriptorImageInfo with image and sampler | PipelineResources::BindTexture |
| VkAccelerationStructureNV | PipelineResources::BindRayTracingScene |


## Drawing
| Vulkan | FrameGraph |
|---|---|
| vkCmdDraw | DrawVertices task |
| vkCmdDrawIndexed | DrawIndexed task |
| vkCmdDrawIndirect | DrawVerticesIndirect task |
| vkCmdDrawIndexedIndirect | DrawIndexedIndirect task |
| vkCmdBindVertexBuffers | DrawTask::AddBuffer |
| vkCmdBindIndexBuffer | DrawIndexed::SetIndexBuffer or DrawIndexedIndirect::SetIndexBuffer |
| vkCmdDrawMeshTasksNV | DrawMeshes task |
| vkCmdDrawMeshTasksIndirectNV | DrawMeshesIndirect task |
| vkCmdSetScissor | DrawTask::AddScissor |
| vkCmdPushConstants | DrawTask::AddPushConstant |
| vkCmdSetStencilCompareMask | DrawTask::SetStencilCompareMask |
| vkCmdSetStencilReference | DrawTask::SetStencilReference |
| vkCmdSetStencilWriteMask | DrawTask::SetStencilWriteMask |
| vkCmdSetViewport | RenderPassDesc::AddViewport |
| vkCmdBeginRenderPass, vkCmdNextSubpass, vkCmdEndRenderPass | SubmitRenderPass task |
| vkCmdClearAttachments | ClearAttachments task |
| vkCmdBindPipeline with VK_PIPELINE_BIND_POINT_GRAPHICS | DrawTask::SetPipeline |
| vkCmdBindDescriptorSets  with VK_PIPELINE_BIND_POINT_GRAPHICS| DrawTask::AddResources and RenderPassDesc::AddResources |

`DrawTask` - may be one of DrawVertices, DrawIndexed, DrawVerticesIndirect, DrawIndexedIndirect, DrawMeshes, DrawMeshesIndirect.


## Computing
| Vulkan | FrameGraph |
|---|---|
| vkCmdDispatch | DispatchCompute task |
| vkCmdDispatchIndirect | DispatchComputeIndirect task |
| vkCmdPushConstants | ComputeTask::AddPushConstant |
| vkCmdBindPipeline with VK_PIPELINE_BIND_POINT_COMPUTE | ComputeTask::SetPipeline |
| vkCmdBindDescriptorSets with VK_PIPELINE_BIND_POINT_COMPUTE | ComputeTask::AddResources |

`ComputeTask` - may be one of DispatchCompute or DispatchComputeIndirect.


## Ray tracing
| Vulkan | FrameGraph |
|---|---|
| vkGetRayTracingShaderGroupHandlesNV | UpdateRayTracingShaderTable task |
| vkCmdBuildAccelerationStructureNV for BLAS | BuildRayTracingGeometry task |
| vkCmdBuildAccelerationStructureNV for TLAS | BuildRayTracingScene task |
| vkCmdTraceRaysNV | TraceRays task |
| vkCmdPushConstants | TraceRays::AddPushConstant |
| vkCmdBindPipeline with VK_PIPELINE_BIND_POINT_RAY_TRACING_NV | ComputeTask::SetPipeline |
| vkCmdBindDescriptorSets with VK_PIPELINE_BIND_POINT_RAY_TRACING_NV | ComputeTask::AddResources |


## Synchronizations
| Vulkan | FrameGraph |
|---|---|
| vkWaitForFences | FrameGraph::Wait for command buffers |
| semaphores | add dependency between command buffers by specializing `dependsOn` parameter in FrameGraph::Begin or by calling CommandBuffer::AddDependency |
| vkDeviceWaitIdle | FrameGraph::WaitIdle |
