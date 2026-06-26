# XIAO ESP32S3 — 引脚映射表 (Pinout)

> 来源：https://wiki.seeedstudio.com/xiao_esp32s3_getting_started/  
> 来源：https://wiki.seeedstudio.com/xiao_esp32s3_pin_multiplexing/  
> 采集日期：2026-06-26

引脚图（正面）：  
`https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32S3/img/XIAO_ESP32-S3_front_pinout.png`

引脚图（背面）：  
`https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32S3/img/XIAO_ESP32-S3_back_pinout.png`

---

## 主引脚映射表（Header Pins）

| 板载标号 | Arduino 数字 | Arduino 模拟 | GPIO 编号 | Touch 通道 | 主要功能 | 备注 |
|---------|------------|------------|---------|-----------|---------|------|
| 5V      | -          | -          | VBUS    | -         | 电源输入/输出 (5V) | USB供电时有效，电池供电时无5V |
| GND     | -          | -          | GND     | -         | 地       | - |
| 3V3     | -          | -          | 3V3_OUT | -         | 3.3V 输出 | 最大 700mA |
| D0      | D0         | A0         | GPIO1   | TOUCH1    | GPIO / ADC / Touch | - |
| D1      | D1         | A1         | GPIO2   | TOUCH2    | GPIO / ADC / Touch | - |
| D2      | D2         | A2         | GPIO3   | TOUCH3    | GPIO / ADC / Touch | - |
| D3      | D3         | A3         | GPIO4   | TOUCH4    | GPIO / ADC / Touch | - |
| D4      | D4         | A4         | GPIO5   | TOUCH5    | GPIO / ADC / I2C SDA | 默认 I2C SDA |
| D5      | D5         | A5         | GPIO6   | TOUCH6    | GPIO / ADC / I2C SCL | 默认 I2C SCL |
| D6      | D6         | -          | GPIO43  | -         | GPIO / UART0 TX     | 不支持 ADC |
| D7      | D7         | -          | GPIO44  | -         | GPIO / UART0 RX     | 不支持 ADC |
| D8      | D8         | A8         | GPIO7   | TOUCH7    | GPIO / ADC / SPI SCK | 默认 SPI SCK |
| D9      | D9         | A9         | GPIO8   | TOUCH8    | GPIO / ADC / SPI MISO | 默认 SPI MISO |
| D10     | D10        | A10        | GPIO9   | TOUCH9    | GPIO / ADC / SPI MOSI | 默认 SPI MOSI |

---

## Sense 扩展板额外引脚（B2B Connector）

> 仅 XIAO ESP32S3 Sense 版本

| 板载标号 | Arduino | GPIO 编号 | 主要功能 | 备注 |
|---------|---------|---------|---------|------|
| D11     | D11     | GPIO42  | GPIO / Touch12 | **不支持 ADC**；默认用于 PDM 麦克风 CLK；切断 J1/J2 后可作 GPIO |
| D12     | D12     | GPIO41  | GPIO / Touch13 | **不支持 ADC**；默认用于 PDM 麦克风 DATA；切断 J1/J2 后可作 GPIO |

> ⚠️ GPIO41/42 虽映射到 A11/A12，但 ESP32-S3 芯片限制导致这两个引脚**不支持 ADC 功能**。

---

## 内部/特殊引脚（不在 Header 上）

| 标号           | GPIO 编号 | 功能描述       | 备注 |
|----------------|---------|--------------|------|
| USER_LED       | GPIO21  | 用户橙色 LED   | 低电平点亮 |
| CHARGE_LED     | -       | 充电红色指示灯 | 充电时闪烁，充满熄灭 |
| Boot           | GPIO0   | 进入 Bootloader 模式 | 上电时下拉进 Boot |
| Reset          | CHIP_PU | 芯片复位        | - |
| U.FL           | LNA_IN  | 外置 WiFi/BT 天线接口 | - |
| MTDO (JTAG)    | GPIO40  | JTAG TDO      | 也是 Sense 扩展板 Camera SDA |
| MTDI (JTAG)    | GPIO41  | JTAG TDI      | 也是 Sense 扩展板 PDM DATA / D12 |
| MTCK (JTAG)    | GPIO39  | JTAG TCK      | 也是 Sense 扩展板 Camera SCL |
| MTMS (JTAG)    | GPIO42  | JTAG TMS      | 也是 Sense 扩展板 PDM CLK / D11 |

---

## Strapping 引脚（上电配置）

| GPIO  | 功能                     | 默认状态（内部上拉/下拉） |
|-------|--------------------------|------------------------|
| GPIO0 | Chip boot mode           | 内部弱上拉 (SPI Boot)   |
| GPIO3 | JTAG signal source       | -                       |
| GPIO45| VDD_SPI 电压选择          | 内部弱下拉              |
| GPIO46| Boot mode + ROM 串口输出  | 内部弱下拉              |

---

## Sense 扩展板内部引脚占用

> 以下 GPIO 被 Sense 扩展板板载外设占用，**不可直接作普通 GPIO 使用**

| GPIO  | 占用外设        | 说明 |
|-------|----------------|------|
| GPIO7 | MicroSD SPI SCK  | J3 控制；切断 J3 可恢复为普通 SPI |
| GPIO8 | MicroSD SPI MISO | 同上 |
| GPIO9 | MicroSD SPI MOSI | 同上 |
| GPIO21| MicroSD SPI CS   | - |
| GPIO10| Camera MCLK (XMCLK) | - |
| GPIO11| Camera DVP_Y8    | - |
| GPIO12| Camera DVP_Y7    | - |
| GPIO13| Camera DVP_PCLK  | - |
| GPIO14| Camera DVP_Y6    | - |
| GPIO15| Camera DVP_Y2    | - |
| GPIO16| Camera DVP_Y5    | - |
| GPIO17| Camera DVP_Y3    | - |
| GPIO18| Camera DVP_Y4    | - |
| GPIO38| Camera DVP_VSYNC | - |
| GPIO39| Camera I2C SCL   | - |
| GPIO40| Camera I2C SDA   | - |
| GPIO41| PDM Mic DATA (D12)| 切断 J1/J2 后可作 GPIO |
| GPIO42| PDM Mic CLK (D11) | 切断 J1/J2 后可作 GPIO |
| GPIO47| Camera DVP_HREF  | - |
| GPIO48| Camera DVP_Y9    | - |
