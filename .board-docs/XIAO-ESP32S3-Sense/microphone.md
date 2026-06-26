# XIAO ESP32S3 Sense — 数字麦克风 (Microphone)

> 来源：https://wiki.seeedstudio.com/xiao_esp32s3_sense_mic/  
> 采集日期：2026-06-26

---

## 麦克风规格

| 属性 | 规格 |
|------|------|
| 类型 | PDM 数字麦克风（Pulse Density Modulation） |
| 接口 | PDM（通过 I2S 外设驱动） |
| 通道 | 单声道（MONO） |
| 采样率 | 推荐 16kHz（稳定），可调 |
| 采样位宽 | 16-bit（ESP32-S3 当前仅支持 16-bit PDM） |
| 位置 | Sense 扩展板板载 |

---

## 麦克风 GPIO 引脚

| 信号 | GPIO 编号 | 板载标号 | 备注 |
|------|---------|---------|------|
| PDM DATA | GPIO41 | D12（Sense B2B 引脚） | I2S DATA IN |
| PDM CLK  | GPIO42 | D11（Sense B2B 引脚） | I2S Word Select / CLK |

> ⚠️ 默认情况下 GPIO41/42 通过 J1/J2 焊盘连接至麦克风。  
> 切断 J1/J2 后，麦克风不可用，GPIO41/42 可作普通 GPIO 使用。

---

## I2S 初始化配置

### Arduino esp32 2.0.x（使用 I2S.h）

```cpp
#include <I2S.h>

// PDM CLK = GPIO42, PDM DATA = GPIO41
// 参数顺序：bck_pin, ws_pin, data_in_pin, data_out_pin, ch_pin
I2S.setAllPins(-1, 42, 41, -1, -1);

// 初始化：PDM 单声道，16kHz，16-bit
I2S.begin(PDM_MONO_MODE, 16000, 16);
```

### Arduino esp32 3.0.x（使用 ESP_I2S.h）

```cpp
#include <ESP_I2S.h>
I2SClass I2S;

// 设置 PDM 引脚：CLK=42, DATA=41
I2S.setPinsPdmRx(42, 41);

// 初始化
I2S.begin(I2S_MODE_PDM_RX, 16000, I2S_DATA_BIT_WIDTH_16BIT, I2S_SLOT_MODE_MONO);
```

---

## 读取音频数据

```cpp
// 读取单个采样值（过滤无效值 0, -1, 1）
int sample = I2S.read();
if (sample && sample != -1 && sample != 1) {
    Serial.println(sample);
}
```

---

## 录音保存为 WAV 文件

录音需要配合 MicroSD 卡使用（SD CS = GPIO21），并开启 **PSRAM**（用于 `ps_malloc` 缓冲区）。

**WAV 文件规格：**

| 参数 | 值 |
|------|---|
| 格式 | PCM WAV |
| 采样率 | 16000 Hz |
| 位深度 | 16-bit |
| 声道数 | 1（MONO） |
| 文件扩展名 | `.wav` |

**关键代码片段（esp32 2.0.x）：**

```cpp
#define RECORD_TIME   20        // 录音秒数
#define SAMPLE_RATE   16000U
#define SAMPLE_BITS   16
#define VOLUME_GAIN   2         // 音量增益

// PSRAM 分配缓冲区
uint32_t record_size = (SAMPLE_RATE * SAMPLE_BITS / 8) * RECORD_TIME;
uint8_t *rec_buffer = (uint8_t *)ps_malloc(record_size);

// 读取 I2S 数据到缓冲区
esp_i2s::i2s_read(esp_i2s::I2S_NUM_0, rec_buffer, record_size, &sample_size, portMAX_DELAY);

// 写入 SD 卡
SD.begin(21);   // SD CS = GPIO21
```

---

## 注意事项

| 事项 | 说明 |
|------|------|
| PSRAM 必须开启 | 录音缓冲区使用 `ps_malloc()` 分配到 PSRAM |
| 仅支持 PDM_MONO_MODE | 当前 ESP32-S3 芯片限制，不支持立体声 |
| 推荐采样率 16kHz | 更高采样率可能不稳定 |
| J1/J2 焊盘 | 默认连通（麦克风可用）；切断后 GPIO41/42 恢复为通用 IO |
| 与摄像头共用 GPIO | GPIO41/42 同时也是 JTAG TDI/TMS，使用时注意调试器冲突 |
