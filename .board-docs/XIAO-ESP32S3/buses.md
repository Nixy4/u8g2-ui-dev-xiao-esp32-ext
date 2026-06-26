# XIAO ESP32S3 — 总线 & 接口映射表 (Buses)

> 来源：https://wiki.seeedstudio.com/xiao_esp32s3_getting_started/  
> 来源：https://wiki.seeedstudio.com/xiao_esp32s3_pin_multiplexing/  
> 采集日期：2026-06-26

---

## I2C

| 信号 | Arduino 标号 | GPIO 编号 | 备注 |
|------|------------|---------|------|
| SDA  | D4 / A4    | GPIO5   | 默认 Wire 总线 |
| SCL  | D5 / A5    | GPIO6   | 默认 Wire 总线 |

**使用说明：**
- Arduino 中使用 `Wire.begin()` 即自动使用上述引脚
- 最高支持 400kHz（Fast-mode）
- 用于 OLED（SSD1306 等）、传感器等设备

**Sense 版摄像头 I2C（内部占用）：**

| 信号 | GPIO 编号 | 备注 |
|------|---------|------|
| Camera SCL | GPIO39 (MTCK) | 仅 Sense 扩展板内部 |
| Camera SDA | GPIO40 (MTDO) | 仅 Sense 扩展板内部 |

---

## SPI

| 信号 | Arduino 标号 | GPIO 编号 | 备注 |
|------|------------|---------|------|
| SCK  | D8 / A8    | GPIO7   | 默认 SPI 时钟 |
| MISO | D9 / A9    | GPIO8   | 默认 SPI 数据输入 |
| MOSI | D10 / A10  | GPIO9   | 默认 SPI 数据输出 |
| CS   | 用户自定义  | 任意 GPIO | 需在代码中手动指定 |

**Sense 版说明：**
- Sense 扩展板的 MicroSD 卡默认占用 GPIO7/8/9/21
- 扩展板上的 **J3 焊盘**控制 SPI/SD 切换：
  - 切断 J3 → 禁用 SD 卡，恢复 SPI 引脚（D8/D9/D10）为普通 SPI
  - 保留 J3 → SD 卡可用，SPI 引脚被占用

**MicroSD SPI 映射（Sense 版内部）：**

| 信号 | GPIO 编号 | 备注 |
|------|---------|------|
| SD_SCK  | GPIO7  | 与 D8 共用 |
| SD_MISO | GPIO8  | 与 D9 共用 |
| SD_MOSI | GPIO9  | 与 D10 共用 |
| SD_CS   | GPIO21 | 与 USER_LED 共用 |

---

## UART

ESP32-S3 共有 3 个 UART（UART0/1/2），引脚可重映射到任意 GPIO。

### UART0（默认 / USB 调试串口）

| 信号 | Arduino 标号 | GPIO 编号 | 备注 |
|------|------------|---------|------|
| TX   | D6         | GPIO43  | `Serial` / `Serial1` 默认 TX |
| RX   | D7         | GPIO44  | `Serial` / `Serial1` 默认 RX |

- Arduino 中 `Serial.begin(115200)` 使用 USB CDC（不占用 D6/D7）
- `Serial1.begin(BAUD, SERIAL_8N1, D7, D6)` 使用硬件 UART（D6=TX, D7=RX）

### UART1/2（可重映射）

| 信号 | 示例映射 | 备注 |
|------|---------|------|
| TX   | D10 (GPIO9) | `HardwareSerial MySerial(1); MySerial.begin(115200, SERIAL_8N1, D9, D10)` |
| RX   | D9 (GPIO8)  | 同上 |

> 任意未占用的 GPIO 均可作为 UART 引脚，通过 `HardwareSerial.begin(baud, config, RX, TX)` 指定。

---

## ADC（模拟输入）

ESP32-S3 的 ADC 分辨率最高 12-bit（0-4095），参考电压 3.3V。

| 可用引脚 | Arduino 模拟标号 | GPIO 编号 | 备注 |
|---------|----------------|---------|------|
| A0      | A0             | GPIO1   | ✓ ADC |
| A1      | A1             | GPIO2   | ✓ ADC |
| A2      | A2             | GPIO3   | ✓ ADC |
| A3      | A3             | GPIO4   | ✓ ADC |
| A4      | A4             | GPIO5   | ✓ ADC（I2C SDA 复用） |
| A5      | A5             | GPIO6   | ✓ ADC（I2C SCL 复用） |
| A8      | A8             | GPIO7   | ✓ ADC（SPI SCK 复用） |
| A9      | A9             | GPIO8   | ✓ ADC（SPI MISO 复用） |
| A10     | A10            | GPIO9   | ✓ ADC（SPI MOSI 复用） |
| A11     | A11            | GPIO42  | ⚠️ **不支持 ADC**（仅 Sense 版） |
| A12     | A12            | GPIO41  | ⚠️ **不支持 ADC**（仅 Sense 版） |

> ⚠️ D6（GPIO43）和 D7（GPIO44）**不支持 ADC**。

---

## PWM

- **全部 GPIO 引脚均支持 PWM 输出**（通过 LEDC 外设）
- Arduino 使用 `analogWrite(pin, value)` 即可
- 分辨率：8-bit（0-255）默认

---

## Touch（电容触摸）

| 引脚 | Arduino 标号 | GPIO 编号 | Touch 通道 |
|------|------------|---------|-----------|
| D0   | A0         | GPIO1   | TOUCH1 |
| D1   | A1         | GPIO2   | TOUCH2 |
| D2   | A2         | GPIO3   | TOUCH3 |
| D3   | A3         | GPIO4   | TOUCH4 |
| D4   | A4         | GPIO5   | TOUCH5 |
| D5   | A5         | GPIO6   | TOUCH6 |
| D8   | A8         | GPIO7   | TOUCH7 |
| D9   | A9         | GPIO8   | TOUCH8 |
| D10  | A10        | GPIO9   | TOUCH9 |
| D11  | -          | GPIO42  | TOUCH12（Sense 版） |
| D12  | -          | GPIO41  | TOUCH13（Sense 版） |

> 使用 `analogRead(pin)` 读取触摸值，接触时数值明显增大。

---

## I2S（IIS）

ESP32-S3 支持 I2S 接口（仅 Sense/Plus 版规格表中列出）：
- 引脚可灵活重映射，无固定默认引脚
- Sense 版 PDM 麦克风使用 GPIO41（DATA）/ GPIO42（CLK）

---

## JTAG

| 信号 | GPIO 编号 | Arduino 标号 | 备注 |
|------|---------|------------|------|
| JTAG TCK | GPIO39 | - | 与 Sense Camera SCL 共用 |
| JTAG TDO | GPIO40 | - | 与 Sense Camera SDA 共用 |
| JTAG TDI | GPIO41 | D12（Sense） | - |
| JTAG TMS | GPIO42 | D11（Sense） | - |

---

## USB

| 信号 | 说明 |
|------|------|
| USB D+ | 差分正，高速数据线（内部，不引出） |
| USB D- | 差分负，高速数据线（内部，不引出） |
| USB Type-C | 唯一外部 USB 接口，支持 CDC 虚拟串口 + 供电 |

---

## 电源

| 供电方式 | 电压范围 | 说明 |
|---------|---------|------|
| USB Type-C | 5V | 最常用供电方式；5V 引脚可输出 |
| BAT 引脚   | 3.7V 锂电池 | 电池供电时 5V 引脚无输出 |
| 3V3 引脚输出 | 3.3V（最大 700mA） | 由板载 LDO 提供 |
| 充电电流（标准版） | 50mA 快充 / 3.8mA 涓流 | - |
| 充电电流（Plus 版） | 100mA 快充 / 0.9mA 涓流 | - |
| 工作温度 | -20°C ~ 65°C | - |
