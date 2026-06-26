# Seeed Studio XIAO ESP32S3 Sense — 扩展板资料

> **厂商：** Seeed Studio  
> **硬件：** XIAO ESP32S3 主板 + Sense 扩展板（B2B 连接）  
> **采集日期：** 2026-06-26

---

## Sense 扩展板专属硬件

| 外设 | 型号 | GPIO 占用 | 文档 |
|------|------|-----------|------|
| 摄像头 | OV3660（出货）/ 兼容 OV5640 | GPIO10-18, 38-40, 47-48（共 14 路） | [camera.md](camera.md) |
| PDM 数字麦克风 | 板载 PDM | GPIO41（DATA）/ GPIO42（CLK） | [microphone.md](microphone.md) |
| MicroSD 卡槽 | SPI，≤32GB FAT32 | GPIO21（CS）/ GPIO7/8/9（SPI） | [sdcard.md](sdcard.md) |

---

## 文件目录

| 文件 | 内容 |
|------|------|
| [camera.md](camera.md)       | 摄像头 GPIO 占用表、初始化配置、`camera_config_t` 宏定义、支持分辨率 |
| [microphone.md](microphone.md) | PDM 麦克风引脚、I2S 初始化、录音 WAV 代码片段 |
| [sdcard.md](sdcard.md)       | MicroSD SPI 引脚、J3 焊盘控制、文件操作 API |

**通用引脚文档（主板）请参见：** [../XIAO-ESP32S3/](../XIAO-ESP32S3/README.md)

---

## Sense 扩展板 GPIO 占用总览

下表汇总 Sense 扩展板对主板 GPIO 的占用情况：

| GPIO | 占用功能 | 是否可释放 | 释放方式 |
|------|---------|-----------|---------|
| GPIO7  | MicroSD SCK  | ✅ | 切断 J3 焊盘 |
| GPIO8  | MicroSD MISO | ✅ | 切断 J3 焊盘 |
| GPIO9  | MicroSD MOSI | ✅ | 切断 J3 焊盘 |
| GPIO21 | MicroSD CS   | ❌ | - |
| GPIO10 | Camera MCLK  | ❌ | - |
| GPIO11 | Camera DVP_Y8 | ❌ | - |
| GPIO12 | Camera DVP_Y7 | ❌ | - |
| GPIO13 | Camera PCLK  | ❌ | - |
| GPIO14 | Camera DVP_Y6 | ❌ | - |
| GPIO15 | Camera DVP_Y2 | ❌ | - |
| GPIO16 | Camera DVP_Y5 | ❌ | - |
| GPIO17 | Camera DVP_Y3 | ❌ | - |
| GPIO18 | Camera DVP_Y4 | ❌ | - |
| GPIO38 | Camera VSYNC | ❌ | - |
| GPIO39 | Camera SCL   | ❌ | - |
| GPIO40 | Camera SDA   | ❌ | - |
| GPIO41 | PDM Mic DATA (D12) | ✅ | 切断 J1/J2 焊盘 |
| GPIO42 | PDM Mic CLK (D11)  | ✅ | 切断 J1/J2 焊盘 |
| GPIO47 | Camera HREF  | ❌ | - |
| GPIO48 | Camera DVP_Y9 | ❌ | - |

> Sense 扩展板共占用 **20 个 GPIO**（含 2 个可释放的 PDM 引脚）。  
> 主板 Header 上剩余可自由使用的 GPIO：**D0~D5（GPIO1-6）**，共 6 路。

---

## 焊盘控制说明

| 焊盘 | 控制功能 | 默认状态 |
|------|---------|---------|
| J1/J2 | PDM 麦克风 DATA/CLK 连接 | 连通（麦克风可用） |
| J3    | MicroSD SPI 上拉电阻（R4~R6）| 连通（SD 卡可用）  |

---

## 来源 URL

| 页面 | URL |
|------|-----|
| Camera 使用指南 | https://wiki.seeedstudio.com/xiao_esp32s3_camera_usage/ |
| 文件系统 / MicroSD | https://wiki.seeedstudio.com/xiao_esp32s3_sense_filesystem/ |
| 麦克风使用指南 | https://wiki.seeedstudio.com/xiao_esp32s3_sense_mic/ |
| Getting Started（主板） | https://wiki.seeedstudio.com/xiao_esp32s3_getting_started/ |
| Sense 扩展板原理图（v1.5） | https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32S3/new-res/202003753_XIAO%20ESP32S3%20Sense_v1.5_SCH_260226.pdf.pdf |
| 扩展板底板原理图 | https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32S3/res/XIAO_ESP32S3_ExpBoard_v1.0_SCH.pdf |
| OV3660 规格书 | https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32S3/new-res/OV3660_Camera_Module_Specification.pdf |
| OV5640 规格书 | https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32S3/new-res/OV5640_Camera_Module_Specification.pdf |
