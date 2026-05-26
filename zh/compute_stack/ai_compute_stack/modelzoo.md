sidebar_position: 9

# ModelZoo
>
> ModelZoo内模型数据定期更新，测试方法可参考[modelzoo-demo
> ](https://github.com/spacemit-com/model-zoo)

- [ModelZoo](#modelzoo)
  - [基础模型](#基础模型)
    - [resnet](#resnet)
    - [mobilenet](#mobilenet)
    - [efficientnet](#efficientnet)
    - [vit](#vit)
    - [yolov5](#yolov5)
    - [yolov6](#yolov6)
    - [yolov8](#yolov8)
    - [yolov12](#yolov12)
  - [大模型](#大模型)
    - [Qwen](#qwen)
    - [HunYuan](#hunyuan)
    - [Llama](#llama)

## 基础模型
- K1
>- 推理引擎版本: spacemit-ort-2.0.2+beta1
>- OS：bianbu-3.0
>- date：2026-2-9

- K3
>- 推理引擎版本: spacemit-ort-2.0.2+rc2
>- OS：bianbu-4.0rc1
>- date：2026-4-16
>- 6400DDR，A100@1.8GHz

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
| resnet18 | int8 | 224x224 | 7.66 | 4.60 | 2.92 | 2.07 |
| resnet50 | int8 | 224x224 | 18.91 | 11.17 | 6.98 | 5.22 |
| resnet50.batch4 | int8 | 224x224 | 73.37 | 40.19 | 23.19 | 15.55 |
| resnet50 | fp16 | 224x224 | 34.22 | 23.18 | 18.37 | 16.46 |

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
| mobilenet_v1 | int8 | 224x224 | 12.73 | 7.2 | 3.9 | 2.30 |
| mobilenet_v2 | int8 | 224x224 | 17.48 | 9.85 | 5.19 | 3.25 |
| mobilenet_v3_small | fp16 | 224x224 | 6.75 | 4.31 | 2.90 | 2.70 |
| mobilenet_v3_large | fp16 | 224x224 | 13.51 | 8.14 | 5.16 | 4.07 |

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
| efficientnet_v1_b0 | int8 | 224x224 | 28.30 | 16.33 | 10.46 | 7.94 |
| efficientnet_v1_b1 | int8 | 224x224 | 43.66 | 25.50 | 16.57 | 12.62 |
| efficientnet_v2_s | int8 | 224x224 | 42.52 | 24.34 | 15.20 | 11.97 |
| efficientnet_v1_b0 | fp16 | 224x224 | 30.51 | 18.41 | 12.33 | 9.95 |
| efficientnet_v1_b1 | fp16 | 224x224 | 44.64 | 27.09 | 18.22 | 14.45 |
| efficientnet_v2_s | fp16 | 224x224 | 54.56 | 31.69 | 20.53 | 14.55 |

### vit

- K1

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms |
| --- | --- | --- | --- | --- | --- |
| vit_b_16 | int8 | 224x224 | 527.78 | 356.00 | 200.91 |
| vit_b_16 | fp16 | 224x224 | 2557.03 | 1425.90 | 774.00 |

- K3

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms | 8 Core/ms |
| --- | --- | --- | --- | --- | --- | --- |
| vit_b_16 | int8 | 224x224 | 112.43 | 62.83 | 38.78 | 25.83 |
| vit_b_16 | fp16 | 224x224 | 147.27 | 91.96 | 66.03 | 54.53 |

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
| yolov5n | int8 | 640x640 | 52.15 | 28.36 | 16.64 | 10.9 |
| yolov5s | int8 | 640x640 | 79.81 | 43.43 | 25.77 | 16.66 |
| yolov5m | int8 | 640x640 | 156.78 | 84.41 | 47.46 | 30.42 |

### yolov6

- K1

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms |
| --- | --- | --- | --- | --- | --- |
| yolov6n | int8 | 640x640 | 177.65 | 100.04 | 62.43 |
| yolov6s | int8 | 640x640 | 462.12 | 237.01 | 132.61 |

- K3

| 模型名 | type | shape | 1 Core/ms | 2 Core/ms | 4 Core/ms | 8 Core/ms |
| --- | --- | --- | --- | --- | --- | --- |
| yolov6n | int8 | 640x640 | 39.42 | 21.91 | 13.19 | 8.81 |
| yolov6s | int8 | 640x640 | 71.0 | 38.98 | 23.05 | 14.42 |

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
| yolov8n | int8 | 640x640 | 50.67 | 27.81 | 16.39 | 11.04 |
| yolov8s | int8 | 640x640 | 82.39 | 45.06 | 26.96 | 18.08 |
| yolov8m | int8 | 640x640 | 165.84 | 89.342 | 50.522 | 33.96 |

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
| yolo12n | int8 | 640x640 | 122.262 | 65.914 | 38.624 | 28.20 |
| yolo12s | int8 | 640x640 | 213.505 | 113.878 | 66.6732 | 47.99 |
| yolo12m | int8 | 640x640 | 410.327 | 217.392 | 124.668 | 87.35 |

## 大模型

- K3
>- llama.cpp版本：0.1.1
>- OS：bianbu-4.0rc1
>- date：2026-5-26
>- 6400DDR，A100@1.8GHz
>- bash: llama-bench -m model.gguf -t 8 -p 128 -n 128 -mmp 0 -fa 1 -ub 128

### Qwen

- K3

| 模型名 | 量化类型 | PP128 (token/s) | TG128 (token/s) | PP1280 (token/s) | TG1280 (token/s) |
| --- | --- | --- | --- | --- | --- |
| qwen3-0.6B | Q4_0 | 499.75 | 53.35 | - | - |
| qwen3-1.7B | Q4_0 | 229.79 | 23.11 | - | - |
| qwen3-4B | Q4_0 | 76.44 | 11.03 | - | - |
| qwen3-moe-30B-A3B | Q4_0 | 55.54 | 13.33 | 43.86 | 11.93
| qwen3.5-0.8B | Q4_0 | 182.69 | 29.33 | - | - |
| qwen3.5-2B | Q4_1 | 112.22 | 16.15 | - | - |

### HunYuan

- K3

| 模型名 | 量化类型 | PP128 (token/s) | TG128 (token/s) | PP1280 (token/s) | TG1280 (token/s) |
| --- | --- | --- | --- | --- | --- |
| HY-MT1.5-1.8B | Q4_K_M | 157.81 | 20.15 | - | - |

### Llama

- K3

| 模型名 | 量化类型 | PP128 (token/s) | TG128 (token/s) | PP1280 (token/s) | TG1280 (token/s) |
| --- | --- | --- | --- | --- | --- |
| llama2-7B | Q4_0 | 50.40 | 7.07 | - | - |

### Download

| 模型名 | 下载地址 |
| --- | --- |
| qwen3-0.6B | https://www.modelscope.cn/models/aliworkq/Qwen3-0.6B-Q4_0.GGUF/files |
| qwen3-1.7B | https://huggingface.co/miku552/Qwen3-1.7B-Q4_0-GGUF/tree/main |
| qwen3-4B | https://huggingface.co/miku552/Qwen3-4B-Q4_0-GGUF/tree/main |
| qwen3-moe-30B-A3B | https://huggingface.co/fernandoruiz/Qwen3-30B-A3B-Q4_0-GGUF/tree/main |
| qwen3.5-0.8B | https://www.modelscope.cn/models/cola9936/Qwen3.5-0.8B-Q4_0/files |
| qwen3.5-2B | https://www.modelscope.cn/models/Manojb/Qwen3.5-2B-Q4_1.gguf/files |
| HY-MT1.5-1.8B | https://www.modelscope.cn/models/Tencent-Hunyuan/HY-MT1.5-1.8B-GGUF/files |
| llama2-7B | https://www.modelscope.cn/models/TheBloke/Llama-2-7B-GGUF/files |