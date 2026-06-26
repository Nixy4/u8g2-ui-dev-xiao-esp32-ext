# XIAO ESP32S3 Sense — MicroSD 卡接口 (SD Card)

> 来源：https://wiki.seeedstudio.com/xiao_esp32s3_sense_filesystem/  
> 来源：https://wiki.seeedstudio.com/xiao_esp32s3_camera_usage/  
> 采集日期：2026-06-26

---

## MicroSD 卡规格

| 属性 | 规格 |
|------|------|
| 接口协议 | SPI |
| 最大容量 | 32GB |
| 文件系统 | FAT32（必须格式化为 FAT32） |
| 插卡方向 | 金手指朝内插入 |
| 位置 | Sense 扩展板板载卡槽 |

---

## MicroSD SPI GPIO 引脚

| SPI 信号 | GPIO 编号 | Arduino 标号 | 备注 |
|---------|---------|------------|------|
| CS      | GPIO21  | -           | SD 片选，**固定 GPIO21** |
| SCK     | GPIO7   | D8 / A8    | 与 Header SPI SCK 共用 |
| MISO    | GPIO8   | D9 / A9    | 与 Header SPI MISO 共用 |
| MOSI    | GPIO9   | D10 / A10  | 与 Header SPI MOSI 共用 |

> ⚠️ GPIO21 同时是板载 USER_LED，SD 卡使用时 LED 功能冲突。

---

## 初始化代码

```cpp
#include "FS.h"
#include "SD.h"
#include "SPI.h"

// CS 引脚固定为 GPIO21
if (!SD.begin(21)) {
    Serial.println("Card Mount Failed");
    return;
}
```

> ⚠️ 必须传入 CS 引脚号 `21`，不能使用默认的 `SD.begin()` 无参调用。

---

## J3 焊盘控制（SPI 与 SD 卡切换）

| J3 状态 | MicroSD 卡 | D8/D9/D10（SPI Header）|
|---------|-----------|----------------------|
| **默认连通**（出厂）| ✅ 可用 | ❌ 被 SD 卡占用（上拉电阻 R4~R6 连通）|
| **切断 J3** | ❌ 不可用 | ✅ 恢复为普通 SPI / GPIO |

> 切断 J3 实际是断开上拉电阻 R4~R6，使 SD 卡的 SPI 时序无法正常工作。

---

## 与 Round Display for XIAO 同时使用

| 情况 | 处理方式 |
|------|---------|
| 同时使用 Round Display 的 SD 卡槽 | 必须切断 J3（两板均有上拉电阻，会冲突）；使用 Round Display 上的 SD 卡，CS = D2 |
| 仅使用 Sense 扩展板 SD 卡 | 保持 J3 连通，CS = GPIO21 |
| 软件方法（无需切断 J3）| 参考 Mjrovai 方案：https://github.com/Mjrovai/XIAO-ESP32S3-Sense/tree/main/camera_round_display_save_jpeg |

---

## 常用文件操作

```cpp
// 列出目录
void listDir(fs::FS &fs, const char *dirname, uint8_t levels);

// 读取文件
void readFile(fs::FS &fs, const char *path);

// 写入文件（覆盖）
void writeFile(fs::FS &fs, const char *path, const char *message);

// 追加内容
void appendFile(fs::FS &fs, const char *path, const char *message);

// 删除文件
SD.remove("/foo.txt");

// 获取卡容量
uint64_t cardSize = SD.cardSize() / (1024 * 1024);  // MB
```

---

## 支持的卡类型

| 类型 | 说明 |
|------|------|
| CARD_MMC  | MMC 卡 |
| CARD_SD   | SDSC（标准 SD） |
| CARD_SDHC | SDHC（高容量，最常见）|
| CARD_NONE | 未插卡 |

---

## 文件系统方案对比

| 方案 | 适用场景 | 备注 |
|------|---------|------|
| MicroSD（FAT32）| 大文件存储，图片/音频/日志 | 需 Sense 扩展板；最大 32GB |
| SPIFFS（片上 Flash）| 小文件，配置文件，Web 资源 | 不支持目录，最大受限于 Flash 大小（~8MB）|
| Preferences.h | 键值对，配置参数持久化 | 存储到 NVS 分区 |
| EEPROM | 小量数据，兼容 Arduino 风格 | 存储到 Flash |

---

## 深度睡眠 + SD 卡数据记录模式

```cpp
// 将变量保存到 RTC 内存（深度睡眠后保留）
RTC_DATA_ATTR int readingID = 0;

// 设置唤醒时间（秒）
uint64_t TIME_TO_SLEEP = 600;  // 10 分钟
esp_sleep_enable_timer_wakeup(TIME_TO_SLEEP * 1000000);
esp_deep_sleep_start();
```

---

## 注意事项

| 事项 | 说明 |
|------|------|
| 格式化要求 | 必须 FAT32；若出现挂载失败，用 SD Card Formatter 工具重新格式化 |
| 插卡方向 | 金手指朝内（朝向 B2B 连接器方向） |
| J3 切断不可逆 | 切断后可重新焊接 J1/J2 恢复，但需焊接技巧 |
| CS 引脚固定 | Sense 扩展板 CS 固定为 GPIO21，不可更改 |
| SPI 独占 | 使用 SD 卡时，D8/D9/D10 不可用于其他 SPI 设备（J3 连通状态下）|
