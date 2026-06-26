# Seeed Studio XIAO ESP32S3 — 开发板资料

> **厂商：** Seeed Studio  
> **主控：** ESP32-S3R8（Xtensa LX7 双核，240 MHz，8MB PSRAM）  
> **采集日期：** 2026-06-26

---

## 版本说明

本文档覆盖以下三个版本，差异见 [peripherals.md](peripherals.md)：

| 版本 | 摄像头 | 麦克风 | MicroSD | Flash |
|------|--------|--------|---------|-------|
| XIAO ESP32S3         | ✗ | ✗ | ✗ | 8MB |
| XIAO ESP32S3 Sense   | OV3660 (3MP) | PDM | ✓ | 8MB |
| XIAO ESP32S3 Plus    | ✗ | ✗ | ✗ | 16MB |

---

## 文件目录

| 文件 | 内容 |
|------|------|
| [pinout.md](pinout.md)           | 完整引脚映射表（Header 引脚 + 内部引脚 + Sense 扩展板引脚） |
| [peripherals.md](peripherals.md) | 板载外设型号表（MCU、存储、摄像头、麦克风、LED、电源等） |
| [buses.md](buses.md)             | 总线映射汇总（I2C / SPI / UART / ADC / PWM / Touch / JTAG） |

---

## 快速参考

### 关键引脚速查

| 功能 | Arduino 标号 | GPIO |
|------|------------|------|
| I2C SDA | D4 | GPIO5 |
| I2C SCL | D5 | GPIO6 |
| SPI SCK | D8 | GPIO7 |
| SPI MISO | D9 | GPIO8 |
| SPI MOSI | D10 | GPIO9 |
| UART TX | D6 | GPIO43 |
| UART RX | D7 | GPIO44 |
| User LED | - | GPIO21（低电平点亮）|

### ADC 可用引脚

`A0(GPIO1)` `A1(GPIO2)` `A2(GPIO3)` `A3(GPIO4)` `A4(GPIO5)` `A5(GPIO6)` `A8(GPIO7)` `A9(GPIO8)` `A10(GPIO9)`

> ⚠️ A11(GPIO42) 和 A12(GPIO41) **不支持 ADC**，仅 Sense 版有此引脚。

### PWM

全部 GPIO 引脚均支持 PWM。

---

## 来源 URL

| 页面 | URL |
|------|-----|
| Getting Started | https://wiki.seeedstudio.com/xiao_esp32s3_getting_started/ |
| Pin Multiplexing | https://wiki.seeedstudio.com/xiao_esp32s3_pin_multiplexing/ |
| 引脚图（正面） | https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32S3/img/XIAO_ESP32-S3_front_pinout.png |
| 引脚图（背面） | https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32S3/img/XIAO_ESP32-S3_back_pinout.png |
| 原理图（标准版 v1.4） | https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32S3/new-res/202003751_XIAO%20ESP32S3_v1.4_SCH_260226.pdf.pdf |
| 原理图（Sense 版 v1.5） | https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32S3/new-res/202003753_XIAO%20ESP32S3%20Sense_v1.5_SCH_260226.pdf.pdf |
| 原理图（Plus 版 v1.1） | https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32S3/res/XIAO_ESP32S3_Plus_V1.1_SCH_260115.pdf |
| Pinout Sheet (xlsx) | https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32S3/res/XIAO_ESP32S3_Sense_Pinout.xlsx |
| ESP32-S3 数据手册 | https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32S3/res/esp32-s3_datasheet.pdf |
