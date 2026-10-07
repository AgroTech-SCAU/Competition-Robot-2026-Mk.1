<div align="center">

# SteerWheel Mk.1

</div>

> AgroTech 协会中型轮式机器人初代原型机（仓库：`Competition-Robot-2026-Mk.1`）

> 本仓库由原集合仓库 `Steering-Wheel-Chassis`（一车一目录）拆分而来，当前只维护 **SteerWheel Mk.1** 这一台机器人

---

## 1. 当前状态

- **状态：** 开发中（原型机验证阶段）
- **开发计划：** [`docs/plan.md`](docs/plan.md)
- **整车详细说明：** [`SteerWheel Mk.1/README.md`](SteerWheel%20Mk.1/README.md)

> 首次形成可复现的稳定版本后，再创建 Git Tag + GitHub Release，并将本 README 更新为该稳定版本的完整使用说明

---

## 2. 参与本项目开发

推荐流程：

**Issue → Branch → Commit → Push → Pull Request → 项目负责人 Merge**

- 开始开发前，原则上先创建或认领 Issue
- 请勿直接在 `main` 开发或 Push
- 如果已经误在 `main` 上产生了有用 Commit，**不要先 `reset --hard`**，先按协作指南把提交保存到新分支

完整流程与常见问题：[`.github/CONTRIBUTING.md`](.github/CONTRIBUTING.md)

---

## 3. 项目简介

`SteerWheel Mk.1` 是 AgroTech 协会中型轮式机器人平台的初代原型机，用于验证四舵轮底盘的机械结构与运动学控制、底盘与五自由度机械臂的一体化集成，以及自研 STM32H723 控制板的软件分层与设备驱动架构，是后续 `Atlas`、`Hephaestus` 等轮式平台的技术试验床与资产来源。

核心特性：

- 四模块舵轮底盘（转向与驱动双总线分离控制）
- 五轴五自由度机械臂（支持整体 / 单关节 / 零位 / IK 位姿控制）
- 自研 `STM32H723VGT6` 控制板，CubeMX 工程，500 Hz 控制节拍
- BMI088 IMU 姿态采集、iBus 遥控链路、WS2812 RGB 状态指示
- 机械臂 `URDF` / `mesh` / `launch` 开发资源（`arm_description/`）

---

## 4. 环境要求

### 软件

- `STM32CubeMX` / `STM32CubeIDE`（或 EIDE / `arm-none-eabi-gcc`）
- 机械臂描述资源验证可选用 ROS 环境
- SolidWorks（如需查看机械资料）

### 硬件

- 主控：`STM32H723VGT6`
- 底盘：四舵轮模块（转向 `DM-G6220` + 驱动 `M3508`，双 CAN 1 Mbps）
- 机械臂：五轴五自由度总线舵机
- 传感器：`BMI088` IMU、`FS-iA10B / iBus` 遥控接收
- 指示：WS2812 RGB

---

## 5. 快速开始

### 5.1 固件

1. 使用 `STM32CubeMX` / `STM32CubeIDE` 打开 `SteerWheel Mk.1/chassis_control_code/robot.ioc`
2. 检查本地 STM32H7 固件包版本是否兼容
3. 生成或刷新工程，编译并下载到 `STM32H723VGT6`
4. 按实际接线确认 CAN、IMU、遥控接收器、总线舵机、RGB 灯工作正常

### 5.2 固件入口与装配

```c
// Core/Src/main.c
entry_init();
while (1) {
    entry_loop();
}
```

装配顺序：`delay → log → rgb → imu → chassis → remote → TIM6 500Hz → arm`

### 5.3 机械臂描述资源

- `SteerWheel Mk.1/chassis_control_code/arm_description/`：`urdf/`、`meshes/*.STL`、`launch/`、`config/`

---

## 6. 目录结构

```text
Competition-Robot-2026-Mk.1/
├── SteerWheel Mk.1/
│   └── chassis_control_code/          # STM32H723 底盘 + 机械臂嵌入式控制工程
│       ├── Core/                      # STM32Cube 生成入口与底层初始化
│       ├── src/
│       │   ├── app/                   # 应用层（遥控、系统入口）
│       │   ├── service/               # 服务层（底盘 / 机械臂 / IMU / RGB 装配）
│       │   ├── device/                # 设备抽象层（电机、舵机、IMU、RGB、遥控）
│       │   ├── domain/                # 领域层（舵轮底盘与串联机械臂运动学）
│       │   ├── infra/                 # 基础设施（日志、PID、矩阵、协议、HFSM）
│       │   └── platform/              # STM32 HAL 适配层
│       ├── arm_description/           # 机械臂 URDF / mesh / launch 资源
│       └── robot.ioc                  # STM32CubeMX 工程
├── docs/
│   └── plan.md                        # 开发计划
└── README.md
```

---

## 7. 文档

- [`SteerWheel Mk.1/README.md`](SteerWheel%20Mk.1/README.md)：机械系统、电控系统、软件架构、启动流程、控制循环、遥控逻辑、编译与使用建议
- `docs/plan.md`：开发计划（必须）

---

## 8. 维护者

- 项目负责人：`@<GitHub-ID>`（待填写）
