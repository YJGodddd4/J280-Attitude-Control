# J280 姿态控制系统学习与开发

## 1. 项目简介

本项目围绕 J280 姿态控制系统开发套件展开学习与开发。

核心器件为高云 GW2A-55 系列 FPGA，主要学习内容包括：

- FPGA 基本结构与工作原理
- Verilog HDL
- I/O Bank 与管脚约束
- 时钟与 PLL
- PWM 与电机控制
- 编码器采集
- ADC 与角位移传感器
- 姿态控制算法
- FPGA 系统综合设计

本仓库同时用于记录团队学习过程、实验结果、问题解决过程以及双周组会汇报内容。

---

## 2. 项目目录

```text
J280-Attitude-Control/
│
├─ .vscode/          VS Code 工程配置
│
├─ docs/             FPGA 与 J280 学习笔记
│
├─ fpga_lab/         FPGA 实验工程
│
├─ hardware/         硬件、接口、管脚等资料
│
├─ images/           实物照片、波形、结构图
│
├─ logs/             学习与开发日志
│
├─ reports/          双周组会总结
│
├─ .gitignore
└─ README.md