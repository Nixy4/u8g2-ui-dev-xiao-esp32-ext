# XIAO ESP32S3 Sense — 摄像头接口 (Camera)

> 来源：https://wiki.seeedstudio.com/xiao_esp32s3_camera_usage/  
> 来源：https://wiki.seeedstudio.com/xiao_esp32s3_getting_started/  
> 采集日期：2026-06-26

---

## 摄像头型号

| 型号 | 状态 | 分辨率 | 接口 | 备注 |
|------|------|--------|------|------|
| OV3660 | ✅ 当前出货型号 | 2048×1536 (3MP) | DVP 并口 + I2C | 出厂默认 |
| OV5640 | ✅ 可选替换 | 2592×1944 (5MP) | DVP 并口 + I2C | 支持自动对焦 (AF) |
| OV2640 | ⚠️ 已停产 | 1600×1200 (2MP) | DVP 并口 + I2C | 旧版出货，代码仍兼容 |

> Wiki 中所有摄像头示例代码均兼容 OV3660 / OV5640 / OV2640。

---

## 摄像头 GPIO 占用（扩展板内部，共 14 个 GPIO）

| `esp_camera` 配置字段 | GPIO 编号 | DVP 信号名 | 备注 |
|----------------------|---------|-----------|------|
| `pin_xclk` (XCLK)    | GPIO10  | XMCLK     | 主时钟输出给摄像头 |
| `pin_d7` (Y9)        | GPIO48  | DVP_Y9    | 数据位 9（最高位） |
| `pin_d6` (Y8)        | GPIO11  | DVP_Y8    | 数据位 8 |
| `pin_d5` (Y7)        | GPIO12  | DVP_Y7    | 数据位 7 |
| `pin_pclk` (PCLK)    | GPIO13  | DVP_PCLK  | 像素时钟 |
| `pin_d4` (Y6)        | GPIO14  | DVP_Y6    | 数据位 6 |
| `pin_d0` (Y2)        | GPIO15  | DVP_Y2    | 数据位 2（最低位） |
| `pin_d3` (Y5)        | GPIO16  | DVP_Y5    | 数据位 5 |
| `pin_d1` (Y3)        | GPIO17  | DVP_Y3    | 数据位 3 |
| `pin_d2` (Y4)        | GPIO18  | DVP_Y4    | 数据位 4 |
| `pin_vsync` (VSYNC)  | GPIO38  | DVP_VSYNC | 垂直同步 |
| `pin_sccb_scl` (SIOC)| GPIO39  | CAM_SCL   | SCCB/I2C 时钟（寄存器配置） |
| `pin_sccb_sda` (SIOD)| GPIO40  | CAM_SDA   | SCCB/I2C 数据（寄存器配置） |
| `pin_href` (HREF)    | GPIO47  | DVP_HREF  | 水平参考信号 |
| `pin_pwdn`           | -1      | -         | 无独立 PWDN 引脚（设为 -1） |
| `pin_reset`          | -1      | -         | 无独立 RESET 引脚（设为 -1） |

> 共 14 路 GPIO，其中 GPIO39/40 与 JTAG TCK/TDO 复用。

---

## `camera_config_t` 标准配置（ESP-IDF / Arduino）

```c
// 使用宏定义时，在 camera_pins.h 中定义 CAMERA_MODEL_XIAO_ESP32S3
#define CAMERA_MODEL_XIAO_ESP32S3 // Has PSRAM

// 对应的宏定义值：
#define PWDN_GPIO_NUM   -1
#define RESET_GPIO_NUM  -1
#define XCLK_GPIO_NUM   10
#define SIOD_GPIO_NUM   40
#define SIOC_GPIO_NUM   39
#define Y9_GPIO_NUM     48
#define Y8_GPIO_NUM     11
#define Y7_GPIO_NUM     12
#define Y6_GPIO_NUM     14
#define Y5_GPIO_NUM     16
#define Y4_GPIO_NUM     18
#define Y3_GPIO_NUM     17
#define Y2_GPIO_NUM     15
#define VSYNC_GPIO_NUM  38
#define HREF_GPIO_NUM   47
#define PCLK_GPIO_NUM   13
```

---

## 摄像头初始化参数说明

| 参数 | 典型值 | 说明 |
|------|--------|------|
| `xclk_freq_hz` | 20000000 | XCLK 频率：20MHz |
| `fb_location` | `CAMERA_FB_IN_PSRAM` | 帧缓冲存储在 PSRAM（需开启 PSRAM） |
| `pixel_format` | `PIXFORMAT_JPEG` | 流媒体格式（RGB565 用于人脸识别） |
| `frame_size` | `FRAMESIZE_UXGA` | 分辨率（有 PSRAM 时可用 UXGA=1600×1200） |
| `jpeg_quality` | 10~12 | JPEG 质量（0-63，值越小质量越高） |
| `fb_count` | 2 | 有 PSRAM 时用 2，无时用 1 |
| `grab_mode` | `CAMERA_GRAB_LATEST` | 有 PSRAM 时用 LATEST；无时用 WHEN_EMPTY |

**重要：** 使用摄像头必须在 Arduino IDE / IDF 中开启 PSRAM 选项。

---

## 支持的分辨率（FRAMESIZE 枚举）

| FRAMESIZE 枚举 | 分辨率 |
|----------------|--------|
| `FRAMESIZE_QVGA` | 320×240 |
| `FRAMESIZE_CIF`  | 400×296 |
| `FRAMESIZE_VGA`  | 640×480 |
| `FRAMESIZE_SVGA` | 800×600 |
| `FRAMESIZE_XGA`  | 1024×768 |
| `FRAMESIZE_SXGA` | 1280×1024 |
| `FRAMESIZE_UXGA` | 1600×1200 |
| `FRAMESIZE_240X240` | 240×240 |

---

## Sense 扩展板 J3 焊盘说明

Sense 扩展板的 J3 焊盘控制 MicroSD SPI 上拉电阻（R4~R6）：

| J3 状态 | 效果 |
|---------|------|
| **默认连通**（出厂）| MicroSD 卡启用；D8/D9/D10 引脚被 SD 卡占用 |
| **切断 J3** | MicroSD 卡禁用；D8/D9/D10 恢复为普通 SPI |

> 使用 Round Display for XIAO + Sense 同时使用时，必须切断 J3（两块板都有上拉电阻，冲突）。

---

## 摄像头功耗参考

| 摄像头 | 工作场景（640×480） | 平均功耗 | 峰值功耗 |
|--------|---------------------|---------|---------|
| OV3660 | Active (640×480)    | ~0.12A  | ~0.6A   |
| OV2640 | Active (640×480)    | ~0.24A  | ~0.65A  |
| 整机（Sense+扩展板，5V） | Webcam 应用 | ~140mA | ~347mA |

---

## 相关代码库

| 资源 | 链接 |
|------|------|
| 示例代码（Arduino）| https://github.com/limengdu/SeeedStudio-XIAO-ESP32S3-Sense-camera |
| OV5640 自动对焦库 | https://github.com/0015/ESP32-OV5640-AF |
| OV5640 数据手册 | https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32S3/res/OV5640_datasheet.pdf |
| OV3660 数据手册 | https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32S3/res/OV3660_datasheet.pdf |
| OV2640 数据手册 | https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32S3/res/OV2640_datasheet.pdf |
