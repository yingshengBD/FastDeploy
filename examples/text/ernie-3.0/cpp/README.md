# ERNIE 3.0 模型C++部署示例

在部署前，需确认以下两个步骤

- 1. 软硬件环境满足要求，参考[FastDeploy环境要求](../../../../docs/cn/build_and_install/download_prebuilt_libraries.md)
- 2. 根据开发环境，下载预编译部署库和samples代码，参考[FastDeploy预编译库](../../../../docs/cn/build_and_install/download_prebuilt_libraries.md)

本目录下提供`seq_cls_infer.cc`快速完成在CPU/GPU的文本分类任务的C++部署示例。


## 文本分类任务

### 快速开始

以下示例展示如何基于FastDeploy库完成ERNIE 3.0 Medium模型在CLUE Benchmark的[AFQMC数据集](https://bj.bcebos.com/paddlenlp/datasets/afqmc_public.zip)上进行文本分类任务的C++预测部署。

```bash
# 下载SDK，编译模型examples代码（SDK中包含了examples代码）
wget https://bj.bcebos.com/fastdeploy/release/cpp/fastdeploy-linux-x64-gpu-0.4.0.tgz
tar xvf fastdeploy-linux-x64-gpu-0.4.0.tgz

cd fastdeploy-linux-x64-gpu-0.4.0/examples/text/ernie-3.0/cpp
mkdir build
cd build
# 执行cmake，需要指定FASTDEPLOY_INSTALL_DIR为FastDeploy SDK的目录。
cmake .. -DFASTDEPLOY_INSTALL_DIR=${PWD}/../../../../../../fastdeploy-linux-x64-gpu-0.4.0
make -j

# 下载AFQMC数据集的微调后的ERNIE 3.0模型以及词表
wget https://bj.bcebos.com/fastdeploy/models/ernie-3.0/ernie-3.0-medium-zh-afqmc.tgz
tar xvfz ernie-3.0-medium-zh-afqmc.tgz

# CPU 推理
./seq_cls_infer_demo --device cpu --model_dir ernie-3.0-medium-zh-afqmc

# GPU 推理
./seq_cls_infer_demo --device gpu --model_dir ernie-3.0-medium-zh-afqmc

```

运行完成后返回的结果如下：
```bash
[INFO] /paddle/FastDeploy/examples/text/ernie-3.0/cpp/seq_cls_infer.cc(93)::CreateRuntimeOption	model_path = ernie-3.0-medium-zh-afqmc/infer.pdmodel, param_path = ernie-3.0-medium-zh-afqmc/infer.pdiparams
[INFO] fastdeploy/runtime.cc(469)::Init	Runtime initialized with Backend::ORT in Device::CPU.
Batch id: 0, example id: 0, sentence 1: 花呗收款额度限制, sentence 2: 收钱码，对花呗支付的金额有限制吗, label: 1, confidence:  0.581852
Batch id: 1, example id: 0, sentence 1: 花呗支持高铁票支付吗, sentence 2: 为什么友付宝不支持花呗付款, label: 0, confidence:  0.997921
```



### 参数说明

`seq_cls_infer_demo` 除了以上示例的命令行参数，还支持更多命令行参数的设置。以下为各命令行参数的说明。

| 参数 |参数说明 |
|----------|--------------|
|--model_dir | 指定部署模型的目录， |
|--batch_size |最大可测的 batch size，默认为 1|
|--max_length |最大序列长度，默认为 128|
|--device | 运行的设备，可选范围: ['cpu', 'gpu']，默认为'cpu' |
|--backend | 支持的推理后端，可选范围: ['onnx_runtime', 'paddle', 'openvino', 'tensorrt', 'paddle_tensorrt']，默认为'onnx_runtime' |
|--use_fp16 | 是否使用FP16模式进行推理。使用tensorrt和paddle_tensorrt后端时可开启，默认为False |

## 相关文档

[ERNIE 3.0模型详细介绍](https://github.com/PaddlePaddle/PaddleNLP/tree/release/2.4/model_zoo/ernie-3.0)

[ERNIE 3.0模型导出方法](https://github.com/PaddlePaddle/PaddleNLP/tree/release/2.4/model_zoo/ernie-3.0)

[ERNIE 3.0模型Python部署方法](../python/README.md)
