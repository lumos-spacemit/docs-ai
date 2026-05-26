sidebar_position: 9

# ModelZoo
>
> ModelZoo内模型数据定期更新，测试方法可参考[modelzoo-demo
> ](https://github.com/spacemit-com/model-zoo)

- [ModelZoo](#modelzoo)
  - [基础模型](#基础模型)
    - [测试方式](#测试方式)
    - [resnet](#resnet)
    - [mobilenet](#mobilenet)
    - [efficientnet](#efficientnet)
    - [vit](#vit)
    - [yolov5](#yolov5)
    - [yolov6](#yolov6)
    - [yolov8](#yolov8)
    - [yolov8-seg](#yolov8-seg)
    - [yolov8-pose](#yolov8-pose)
    - [yolov12](#yolov12)
  - [大模型](#大模型)
    - [测试方式](#测试方式-1)
    - [Qwen](#qwen)
    - [HunYuan](#hunyuan)
    - [Llama](#llama)

## 基础模型
- K1
>- 推理引擎版本: spacemit-ort-2.0.2+beta1
>- OS：bianbu-3.0
>- date：2026-2-9

- K3
>- 推理引擎版本: [v2.0.3](https://github.com/spacemit-com/onnxruntime/releases/download/2.0.3/spacemit-ort.riscv64.2.0.3.tar.gz)
>- OS：bianbu-4.0rc3
>- date：2026-5-26

### 测试方式
~~~
# 进入spacemit-ort库路径
# cd {spacemit_ort_lib}/
export LD_LIBRARY_PATH=./lib/

# 调整为自己的${model_path}(模型文件路径)，${num of cores}(选择跑几个核心)
./bin/onnxruntime_perf_test ${model_path} -e spacemit -r 10 -x 1 -S 1 -s -c 1 -i "SPACEMIT_EP_INTRA_THREAD_NUM|${num of cores}" -I

# 输出信息如下
using SpaceMITExecutionProvider
setting SPACEMIT_EP_INTRA_THREAD_NUM : 4
Setting intra_op_num_threads to 1
Session creation time cost: 0.169475 s
First inference time cost: 109 ms
Total inference time cost: 0.0727021 s
Total inference requests: 10
Average inference time cost total: 7.270205 ms
Total inference run time: 0.0727619 s
Number of inferences per second: 137.435
Avg CPU usage: 62 %
Peak working set size: 91336704 bytes
Avg CPU usage:62
Peak working set size:91336704
Runs:10
Min Latency: 0.00720383 s
Max Latency: 0.00730163 s
P50 Latency: 0.00727787 s
P90 Latency: 0.00730163 s
P95 Latency: 0.00730163 s
P99 Latency: 0.00730163 s
P999 Latency: 0.00730163 s

# Average inference time cost total即单帧推理耗时
~~~

### resnet

- K1

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms |
| --- | --- | --- | --- | --- | --- |
| resnet18 | int8 | 224x224 | 39.71 | 22.49 | 13.71 |
| resnet50 | int8 | 224x224 | 93.37 | 53.01 | 32.86 |
| resnet50 | fp16 | 224x224 | 667.55 | 349.34 | 217.27 |

- K3

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms | 8 Core/ms |
| --- | --- | --- | --- | --- | --- | --- |
| resnet18 | int8 | 224x224 | 7.88 | 4.74 | 2.94 | 2.11 |
| resnet50 | int8 | 224x224 | 19.54 | 11.47 | 7.25 | 5.22 |
| resnet50.batch4 | int8 | 224x224 | 73.37 | 40.19 | 23.19 | 15.55 |
| resnet50 | fp16 | 224x224 | 35.38 | 24.00 | 19.27 | 16.68 |

### mobilenet

- K1

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms |
| --- | --- | --- | --- | --- | --- |
| mobilenet_v1 | int8 | 224x224 | 32.10 | 16.56 | 10.72 |
| mobilenet_v2 | int8 | 224x224 | 28.44 | 18.17 | 13.03 |
| mobilenet_v3_small | fp16 | 224x224 | 24.22 | 16.84 | 12.44 |
| mobilenet_v3_large | fp16 | 224x224 | 61.62 | 38.90 | 26.61 |

- K3

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms | 8 Core/ms |
| --- | --- | --- | --- | --- | --- |---|
| mobilenet_v1 | int8 | 224x224 | 12.71 | 7.24 | 3.95 | 2.38 |
| mobilenet_v2 | int8 | 224x224 | 17.35 | 9.80 | 5.14 | 3.29 |
| mobilenet_v3_small | fp16 | 224x224 | 7.62 | 4.71 | 3.13 | 2.82 |
| mobilenet_v3_large | fp16 | 224x224 | 13.68 | 8.32 | 5.28 | 4.14 |

### efficientnet

- K1

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms |
| --- | --- | --- | --- | --- | --- |
| efficientnet_v1_b0 | int8 | 224x224 | 68.81 | 40.65 | 26.30 |
| efficientnet_v1_b1 | int8 | 224x224 | 97.24 | 57.21 | 37.28 |
| efficientnet_v2_s | int8 | 224x224 | 144.81 | 83.11 | 52.66 |
| efficientnet_v1_b0 | fp16 | 224x224 | 121.70 | 71.87 | 46.47 |
| efficientnet_v1_b1 | fp16 | 224x224 | 172.87 | 102.10 | 65.98 |
| efficientnet_v2_s | fp16 | 224x224 | 563.58 | 305.40 | 176.87 |

- K3

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms | 8 Core/ms |
| --- | --- | --- | --- | --- | --- |---|
| efficientnet_v1_b0 | int8 | 224x224 | 33.32 | 18.66 | 10.36 | 7.93 |
| efficientnet_v1_b1 | int8 | 224x224 | 52.32 | 28.79 | 16.12 | 12.07 |
| efficientnet_v2_s | int8 | 224x224 | 43.06 | 24.64 | 15.19 | 10.65 |
| efficientnet_v1_b0 | fp16 | 224x224 | 34.16 | 19.70 | 12.82 | 9.86 |
| efficientnet_v1_b1 | fp16 | 224x224 | 50.25 | 29.40 | 18.94 | 14.44 |
| efficientnet_v2_s | fp16 | 224x224 | 55.02 | 32.48 | 20.85 | 14.25 |

### vit

- K1

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms |
| --- | --- | --- | --- | --- | --- |
| vit_b_16 | int8 | 224x224 | 527.78 | 356.00 | 200.91 |
| vit_b_16 | fp16 | 224x224 | 2557.03 | 1425.90 | 774.00 |

- K3

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms | 8 Core/ms |
| --- | --- | --- | --- | --- | --- | --- |
| vit_b_16 | int8 | 224x224 | 104.25 | 58.93 | 37.39 | 25.01 |
| vit_b_16 | fp16 | 224x224 | 206.15 | 122.17 | 82.56 | 62.04 |

### yolov5

- K1

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms |
| --- | --- | --- | --- | --- | --- |
| yolov5n | int8 | 640x640 | 233.24 | 149.24 | 111.18 |
| yolov5s | int8 | 640x640 | 450.00 | 238.84 | 140.92 |
| yolov5m | int8 | 640x640 | 996.12 | 483.86 | 269.41 |

- K3

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms | 8 Core/ms |
| --- | --- | --- | --- | --- | --- | --- |
| yolov5n | int8 | 640x640 | 44.72 | 24.56 | 14.51 | 9.80 |
| yolov5s | int8 | 640x640 | 74.38 | 40.77 | 24.27 | 15.96 |
| yolov5m | int8 | 640x640 | 153.58 | 82.73 | 46.53 | 29.65 |

### yolov6

- K1

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms |
| --- | --- | --- | --- | --- | --- |
| yolov6n | int8 | 640x640 | 177.65 | 100.04 | 62.43 |
| yolov6s | int8 | 640x640 | 462.12 | 237.01 | 132.61 |

- K3

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms | 8 Core/ms |
| --- | --- | --- | --- | --- | --- | --- |
| yolov6n | int8 | 640x640 | 32.93 | 18.59 | 11.11 | 7.72 |
| yolov6s | int8 | 640x640 | 66.36 | 36.61 | 21.56 | 13.60 |

### yolov8
- K1

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms |
| --- | --- | --- | --- | --- | --- |
| yolov8n | int8 | 640x640 | 211.49 | 118.88 | 76.18 |
| yolov8s | int8 | 640x640 | 463.19 | 240.62 | 142.38 |
| yolov8m | int8 | 640x640 | 994.91 | 510.06 | 284.39 |

- K3

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms | 8 Core/ms |
| --- | --- | --- | --- | --- | --- | --- |
| [yolov8n](https://archive.spacemit.com/spacemit-ai/model_zoo/vision/yolov8/yolov8n.q.onnx) | int8 | 640x640 | 43.05 | 23.91 | 14.23 | 9.82 |
| [yolov8s](https://archive.spacemit.com/spacemit-ai/model_zoo/vision/yolov8/yolov8s.q.onnx) | int8 | 640x640 | 76.96 | 42.41 | 25.52 | 17.14 |
| yolov8m | int8 | 640x640 | 163.62 | 88.08 | 49.67 | 32.66 |

### yolov8-seg

- K3

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms | 8 Core/ms |
| --- | --- | --- | --- | --- | --- | --- |
| yolov8n-seg | int8 | 640x640 | 68.70 | 37.34 | 21.44 | 13.97 |
| yolov8s-seg | int8 | 640x640 | 111.86 | 60.74 | 35.67 | 23.22 |
| yolov8m-seg | int8 | 640x640 | 216.20 | 115.77 | 64.78 | 41.56 |

### yolov8-pose

- K3

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms | 8 Core/ms |
| --- | --- | --- | --- | --- | --- | --- |
| yolov8n-pose | int8 | 640x640 | 47.14 | 26.73 | 16.46 | 11.44 |
| yolov8s-pose | int8 | 640x640 | 83.19 | 46.34 | 28.31 | 19.15 |
| yolov8m-pose | int8 | 640x640 | 170.45 | 92.66 | 52.62 | 34.96 |

### yolov12

- K1

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms |
| --- | --- | --- | --- | --- | --- |
| yolo12n | int8 | 640x640 | 405.57 | 238.88 | 161.90 |
| yolo12s | int8 | 640x640 | 912.32 | 533.02 | 312.74 |
| yolo12m | int8 | 640x640 | 2050.74 | 1096.84 | 661.23 |

- K3

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms | 8 Core/ms |
| --- | --- | --- | --- | --- | --- | --- |
| yolo12n | int8 | 640x640 | 119.64 | 64.88 | 38.08 | 27.60 |
| yolo12s | int8 | 640x640 | 218.19 | 117.37 | 68.71 | 48.16 |
| yolo12m | int8 | 640x640 | 428.03 | 228.18 | 130.62 | 89.44 |

## 大模型

- K3
>- llama.cpp版本：[0.1.1](https://github.com/spacemit-com/llama.cpp/releases/download/spacemit-llama.cpp.riscv64.0.1.1/spacemit-llama.cpp.riscv64.0.1.1.tar.gz)
>- OS：bianbu-4.0rc3
>- date：2026-5-26

### 测试方式
~~~
# 进入spacemit-llama.cpp库路径
# cd {spacemit-llama.cpp}/
export LD_LIBRARY_PATH=./lib/

# 调整为自己的${model_path}(模型文件路径)，${num of cores}(选择跑几个核心)
./bin/llama-bench -m ${model_path} -t ${num of cores} -p 128 -n 128 -mmp 0 -fa 1 -ub 128

# 输出信息如下
CPU_RISCV64_SPACEMIT: tcm is available, blk_size: 393216, blk_num: 8, is_fake_tcm: 0
CPU_RISCV64_SPACEMIT: num_cores: 16, num_perfer_cores: 8, perfer_core_arch_id: a064, exclude_main_thread: 0, use_ime1: 0, use_ime2: 1, mem_backend: HPAGE, cpu_mask: ff00, aicpu_id_offset: 8
CPU_RISCV64_SPACEMIT: alloc_chunk: open(/dev/tcm_sync_mem) failed, errno=2
CPU_RISCV64_SPACEMIT: failed to allocate init_barrier from shared mem, falling back to heap
| model                          |       size |     params | backend    | threads | n_ubatch | fa | mmap |            test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | ------: | -------: | -: | ---: | --------------: | -------------------: |
| qwen3 0.6B Q4_0                | 358.78 MiB |   596.05 M | CPU        |       8 |      128 |  1 |    0 |           pp128 |        486.08 ± 0.43 |
| qwen3 0.6B Q4_0                | 358.78 MiB |   596.05 M | CPU        |       8 |      128 |  1 |    0 |           tg128 |         47.72 ± 0.01 |
~~~

### Qwen

- K3

| 模型名 | 量化类型 | PP128 (token/s) | TG128 (token/s) | PP1280 (token/s) | TG1280 (token/s) |
| --- | --- | --- | --- | --- | --- |
| [qwen3-0.6B](https://www.modelscope.cn/models/unsloth/Qwen3-0.6B-GGUF/file/view/master/Qwen3-0.6B-Q4_0.gguf?status=2) | Q4_0 | 486.08 | 47.72 | - | - |
| [qwen3-1.7B](https://www.modelscope.cn/models/unsloth/Qwen3-1.7B-GGUF/file/view/master/Qwen3-1.7B-Q4_0.gguf?status=2) | Q4_0 | 223.11 | 20.52 | - | - |
| qwen3-4B | Q4_0 | 80.05 | 9.57 | - | - |
| qwen3-moe-30B-A3B | Q4_0 | 57.41 | 11.89 | 44.30 | 10.70
| qwen3.5-0.8B | Q4_0 | 59.64 | 24.30 | - | - |
| qwen3.5-2B | Q4_1 | 46.30 | 13.75 | - | - |
| HY-MT1.5-1.8B | Q4_K_M | 169.17 | 15.36 | - | - |
| llama2-7B | Q4_0 | 49.45 | 6.90 | - | - |

### HunYuan

- K3

| 模型名 | 量化类型 | PP128 (token/s) | TG128 (token/s) | PP1280 (token/s) | TG1280 (token/s) |
| --- | --- | --- | --- | --- | --- |
| HY-MT1.5-1.8B | Q4_K_M | 169.17 | 15.36 | - | - |

### Llama

- K3

| 模型名 | 量化类型 | PP128 (token/s) | TG128 (token/s) | PP1280 (token/s) | TG1280 (token/s) |
| --- | --- | --- | --- | --- | --- |
| llama2-7B | Q4_0 | 49.45 | 6.90 | - | - |
