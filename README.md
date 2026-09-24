# 26 电控组赛道工程样例

完整工程以 ZIP 形式存放，包含 64 个文件，保留源码、配置、中文文档、合成日志和结果图的原始目录结构。

**[下载完整工程 control_track_sample.zip](./control_track_sample.zip?raw=true)**

解压后先阅读 `control_track_sample/START_HERE.md`。校验值见 [SHA256SUMS.txt](./SHA256SUMS.txt)。

## 快速运行

进入解压后的 `control_track_sample` 文件夹，需要 Python 3.10+ 和 GCC/Clang：

```bash
python -m pip install -r task1_imu/requirements.txt
python run.py
```

仅编译与测试 C：

```bash
python run.py --check-only
```

倒立摆：在 MATLAB 中进入 `task2_pendulum`，执行：

```matlab
metrics = run_sample
```

## 工程结构

| 部分 | 内容 |
|---|---|
| task1_imu/core | C 六面标定、四元数误差状态 EKF |
| task1_imu/drivers | MPU6050 驱动与 SI 单位换算 |
| task1_imu/app | 初始化、掉线恢复、频闪状态机 |
| task1_imu/protocol | VOFA+ JustFloat 遥测 |
| task1_imu/ports | FreeRTOS 调度与 STM32F4 HAL 适配 |
| task1_imu/config | 标定、噪声和任务配置 |
| task1_imu/tools | 真实数据采集、合成演示与绘图 |
| task1_imu/docs | 架构、算法、接线、协议和验收说明 |
| task2_pendulum | MATLAB 模型生成、LQR、限幅、随机扰动和指标统计 |

## 验证边界

C 核心与应用状态机已通过严格编译、模块测试、ASan/UBSan 检查（未做泄漏检测）和合成数据回放。
FreeRTOS 文件仅经过声明桩语法检查，STM32 适配尚未烧录实测；MATLAB/Simulink 脚本尚未实机运行。
所有 SYNTHETIC 数据和 reference_check 结果仅用于样例验证，不能替代赛道要求的真实硬件数据或 Simulink 验收。
默认板端标定标志无效，需要先自行采集六面数据并生成真实标定参数。
