# tensorRT基本使用

1：模型转换onnx>trt

```
trtexec --onnx=resnet50/model.onnx --saveEngine=resnet_engine.trt  --explicitBatch
```

*其他框架转换成onnx，需要明确bachsize，在tensorrt模型转换会进一步优化。因此添加参数--explicitBatch

涉及：算子优化、特定硬件加速优化、参数量化

2：验证转换好的trt模型能否成功运行

```
trtexec --shapes=input:1x3x224x224 --loadEngine=resnet_engine.trt
输出类似： &&&& PASSED TensorRT.trtexec
```