> **免责声明**
>  
>   *"Open QWear" 是一个独立的开源项目，与 阿里巴巴、千问、夸克 或其各自组织无关联、未获其背书，也非官方合作项目。*  
>   *"Qwen" 名称、图标 <img src="https://img.shields.io/badge/-6A5CFF?logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGZpbGw9IndoaXRlIiB2aWV3Qm94PSIwIDAgMTAyNCAxMDI0Ij48cGF0aCBkPSJNNDA2LjUgMTYwLjljLTEuMS41LTIuNyAxLjctMy42IDIuNy00LjIgNC43LTg3IDE1MS4yLTg3LjUgMTU0LjktLjQgMi41IDIuMSA3LjUgMTUuNyAzMC41IDguOSAxNS4xIDE4LjkgMzIuMSAyMi4yIDM3LjcgNC41IDcuNiA2LjkgMTAuNiA5LjMgMTEuOCAyLjggMS4zIDI0LjUgMS41IDE5Mi4xIDEuNSAxODEuOSAwIDE4OC45LS4xIDE5MS44LTEuOSA0LjMtMi42IDQ1LjUtNzQuNCA0NS41LTc5LjMgMC0zLjMtMTEuNi0yNC41LTM5LjQtNzEuNi0xLjctMy00LjMtNi01LjgtNi44LTIuMy0xLjItMTgtMS40LTk3LjYtMS40LTg5LjEgMC05NS0uMS05Ni41LTEuOC0xLjItMS40LTI1LjItNDIuMy0zOC40LTY1LjctMS45LTMuMy00LjUtNy4xLTUuOS04LjVsLTIuNC0yLjUtNDguOC0uMmMtMjYuOC0uMS00OS42LjItNTAuNy42TTIxMi40IDM2MC40Yy0zLjMgMS41LTQuNSAzLjQtMzMgNTMuNi0xNi40IDI4LjktMTYuMyAyOC41LTkuMyA0MCA0LjkgOC4xIDIyLjIgMzcuMiAzNi40IDYxLjUgNC45IDguMiAxOC4yIDMwLjggMjkuNyA1MC4yIDEyLjggMjEuNiAyMC44IDM2LjIgMjAuOCAzNy44IDAgMy4zLS44IDQuOS0yMSA0MC41LTIzLjkgNDIuNC0yNCA0Mi41LTI0IDQ1LjcgMCAyLjggNi4yIDE0LjEgMzIuMiA1OC4zIDkuNCAxNS45IDEyLjEgMTkuOCAxNC45IDIxLjIgMy4yIDEuNyA5LjYgMS44IDkyLjIgMS44IDk0LjkgMCA5My4zLjEgOTYuMy00LjdDNDU3LjEgNzUxIDQ5MCA2OTEuOCA0OTAgNjkwYzAtMS4zLTMuMS03LjgtNi45LTE0LjQtMTcuNC0zMC40LTQwLjUtNzAuNC00OC42LTg0LjEtNC44LTguMy0xNy0yOS4yLTI3LTQ2LjUtMTkuOS0zNC40LTY4LjEtMTE3LjQtODcuMi0xNTAtNi42LTExLjMtMTMuNC0yMy4xLTE1LjMtMjYuMy01LjctMTAuMS0zLjQtOS43LTQ5LjUtOS43LTMxLjIuMS00MC43LjQtNDMuMSAxLjRtNDY2LjQgNzQuMWMtMy4zIDEuOC0yLjcuOS00MiA3MC41LTg0LjcgMTQ5LjktMTUxLjIgMjY4LjctMTUyLjEgMjcxLjctLjggMi43LS43IDQuMS43IDcuMSA0IDguNSA0My4yIDc0LjkgNDUuNyA3Ny41bDIuNyAyLjdoOTEuOGwzLjItMi44YzEuOS0xLjYgOS40LTEzLjYgMTguOC0zMC4yIDI0LjEtNDIuMyA3MC4zLTEyMy45IDczLjctMTMwIDEuNi0zIDQuMS03IDUuNC04LjhsMi40LTMuMmg0OC43YzQ0LjQgMCA0OS0uMiA1Mi4yLTEuOCAyLjktMS40IDUuMy00LjkgMTQuNy0yMS4yIDMwLjgtNTMuMSAzNC4zLTU5LjYgMzQuMy02M3MtMy41LTkuOC0yNy41LTUwLjVjLTYuMi0xMC41LTIzLjUtNDAuMi0zOC41LTY2LTE1LTI1LjktMjguNS00OC40LTMwLjEtNTBsLTIuOS0zLTQ5LjMtLjJjLTQxLjYtLjItNDkuNiAwLTUxLjkgMS4yIi8+PC9zdmc+" align="absmiddle"> 及相关品牌元素是 Qwen /千问团队及各自所有者的商标或版权材料。本项目仅出于标识与兼容性目的使用这些元素。*  
>   **本项目仅供学习、研究与兼容性测试用途，不得用于商业冒充、侵权或未经授权的服务分发。**

## 目录

1. [架构概览](#1-架构概览)
2. [GMA 协议规范](#2-gma-协议规范)
3. [Hook 技术方案](#3-hook-技术方案)
4. [TCP Bridge 设计](#4-tcp-bridge-设计)
5. [复现指南](#5-复现指南)
6. [附录](#6-附录)

---

## 1. 架构概览

### 1.1 通信链路

```
眼镜 (BLE GATT) ←→ 配套 App (GMA 协议层) ←→ [Xposed Hook 拦截点] ←→ TCP Bridge
```

眼镜与手机通过 BLE 通信，配套 App 内部使用 GMA 协议封装命令和数据。Xposed 模块在 GMA 层和 BLE 回调层插入 Hook，截获所有通信数据，并通过 TCP Bridge 对外暴露。

### 1.2 拦截层次

```
┌─────────────────────────────────────
│  Layer 1: BLE GATT 回调
│  BluetoothGattCallback
│  ├─ onCharacteristicChanged (RX)
│  ├─ onConnectionStateChange (状态)
│  └─ writeCharacteristic (TX)
├─────────────────────────────────────
│  Layer 2: GMA 客户端
│  ├─ sendCommand(msg, callback)
│  ├─ addObserver(listener)
│  └─ addStateListener(listener)
├─────────────────────────────────────
│  Layer 3: GMA 数据回调
│  ├─ onDeviceResponse(type, data)
│  ├─ onReceiveCommandData(msg)
│  └─ onStateChanged(state)
└─────────────────────────────────────
```

建议在 **Layer 2 (GMA 客户端层)** 进行主要拦截，因为该层已解析 GMA 帧头，可直接获取 namespace/command/payload。Layer 1 作为补充，用于捕获连接状态变化和原始 PDU。

---

## 2. GMA 协议规范

### 2.1 PDU 帧结构

每个 GMA PDU 由 5 字节固定头 + 可变长 payload 组成：

```
字节 0: channelId     (通道 ID)
字节 1: requestType   (0x00 = REQUEST)
字节 2: namespace     (功能域, 0-41)
字节 3: command       (操作码)
字节 4: dataType      (0x01=JSON, 0x02=CBOR, 0x03=RAW)
字节 5+: payload      (数据载荷)
```

### 2.2 命名空间定义

| NS | 标识 | 功能 |
|-|-|-|
| 0 | GMA | 设备基础信息查询 |
| 1 | VOICE | 语音通信 |
| 2 | WIFI | WiFi 文件传输控制 |
| 3 | MUSIC | 音乐播放 |
| 4 | NAVI | 导航 |
| 5 | TEL | 电话 |
| 6 | SET_WIFI | WiFi 配置 |
| 7 | CAMERA | 相机/拍照 |
| 8 | SETTINGS | **设备设置** |
| 9 | TRANS | 翻译 |
| 14 | AGDS_TRANS | AGDS 翻译 |
| 15 | CY_APP | 应用事件 |
| 16 | GW_REQ | 设备状态广播 |
| 17 | NOTIFICATION | 通知推送 |
| 18 | OTA | 固件升级 |
| 19 | PAY | 支付 SDK |
| 23 | SYS_UT | 系统工具 (文件/照片传输) |
| 25 | GALLERY | 相册 |
| 28 | FACTORY_TEST | 工厂测试 |
| 33 | ANT_AI | AI 语音助手 |
| 36 | GMA_COMMON | 设备属性查询 |
| 37 | DESKTOP_ICON | 桌面图标管理 |

### 2.3 设置操作协议 (NS=8)

所有设备设置的读写使用统一的 JSON 格式，通过 `identifier` 字段区分具体设置项。

**通用请求格式**:
```json
{
  "device": [{
    "identifier": "<设置标识符>",
    "value": "<JSON 编码字符串>"
  }]
}
```

**命令对 (cmd)**:
| 操作 | 发送 cmd | 接收 cmd | 说明 |
|------|---------|---------|------|
| 查询全部 | 9 | 4 | 返回所有设备设置 |
| 设置(离散) | 1 | 3 | 下拉/开关/枚举 |
| 设置(连续) | 6 | 8 | 滑块/实时调节 |

**已知设置标识符**:
| identifier | 值格式 | 值范围 |
|-----------|--------|--------|
| `soundEffect` | `{"value":"quiet"}` | quiet/standard/energetic |
| `speaker` | `{"volume":38,"isMute":false}` | volume: 0-100 |
| `screenConfig` | 复合对象 (18 字段) | 见 2.3.1 |
| `wakeupConfig` | `{"enable":true,"wakeWord":"..."}` | boolean |
| `wearDetection` | `{"enable":true}` | boolean |
| `battery` | `{"batteryCapacityLevel":85,...}` | 0-100 |
| `wear` | `{"status":true}` | boolean |
| `cameraConfig` | 复合对象 | — |
| `translateConfig` | 复合对象 | — |
| `navigatorConfig` | 复合对象 | — |
| `musicConfig` | 复合对象 | — |
| `customeDesktopApp` | `{"appList":[...]}` | 应用排序数组 |

**设置修改要点**:
1. 发送时 `value` 字段是 JSON **字符串**（二次编码），接收确认时是已解析的 JSON **对象**
2. 修改复合对象中的任意字段，需发送**完整**对象
3. 设置变更后，眼镜通过 NS=16 cmd=1 广播状态变化事件

#### 2.3.1 screenConfig

```json
{
  "screenHeight": 142,
  "screenOffTime": 60000,
  "systemDistance": 300,
  "autoBrightness": true,
  "autoScreenOff": true,
  "distanceMode": "manual",
  "headLiftSensitivity": "high",
  "wakeOnHeadLift": true,
  "ttsDisplay": true,
  "scrollDirection": "normal",
  "promptShowStyle": "none",
  "amapDistance": 900,
  "amapDistanceForSmart": 900,
  "systemDistanceForSmart": 400,
  "teleprompterDistance": 200,
  "teleprompterDistanceForSmart": 400,
  "translateDistance": 200,
  "translateDistanceForSmart": 200
}
```

| 字段 | 类型 | 值范围 | 单位 |
|------|------|--------|------|
| `screenHeight` | int | 0/47/95/142/190 (5 档) | 值越小越高 |
| `screenOffTime` | int | 5000-60000 | 毫秒 |
| `systemDistance` | int | 200-300 | 厘米 |
| `autoBrightness` | bool | — | — |
| `autoScreenOff` | bool | — | — |
| `headLiftSensitivity` | string | high/normal | — |
| `wakeOnHeadLift` | bool | — | — |
| `distanceMode` | string | manual | — |
| `ttsDisplay` | bool | — | — |
| `scrollDirection` | string | normal | — |
| `*Distance` 系列 | int | 200-900 | 厘米 |
| `*DistanceForSmart` 系列 | int | 200-900 | 厘米 |

### 2.4 文件传输 (NS=23)

- 眼镜拍照后自动推送照片数据，以 200-400 字节的二进制分片形式传输
- 无手机端请求命令，纯被动接收
- 手动导入照片时通过 NS=2 (WIFI) 启动 WiFi Direct 传输，NS=15 (CY_APP) 报告传输状态

### 2.5 AI 语音事件 (NS=23, NS=33)

AI 语音助手通信使用 NS=23 cmd=1 (数据) 和 NS=33 cmd=2 (响应)，payload 为 JSON 格式，包含语音会话状态、ASR 识别结果、TTS 合成数据元信息等。

### 2.6 其他命名空间

| NS | cmd | 格式示例 | 说明 |
|----|-----|---------|------|
| 7 | 29 | `{"messageId":"...","phoneType":1,"supportHeicDecode":1}` | 相机 HEIC 解码协商 |
| 15 | 10 | `{"code":"Amap-Navigation"}` | 应用注册 |
| 15 | 6 | `{"code":"FileTransfer","status":"Exited"}` | 文件传输状态 |
| 25 | 6 | `{"action":"requestThumbnail","traceId":"..."}` | 相册缩略图请求 |
| 36 | 1 | `{"props":["ro.product.model","ro.product.brand"]}` | 设备属性查询 |
| 37 | 2 | `{}` | 桌面图标切换 |

---

## 3. Hook 技术方案

### 3.1 环境要求

- LSPosed / Xposed 框架 (API 100+)
- 目标 App 已安装并运行
- Xposed 模块需在目标 App 的**主进程**中加载（GMA 通信在主进程）

### 3.2 Hook 点 1: GMA 客户端

**目标类特征**: 单例模式，包含 `getInstance`、`sendCommand` 以及多个 `addXxxObserver`/`addXxxListener` 注册方法。

**实现要点**:

```kotlin
// 1. 通过 ClassLoader 加载目标类
val clientClass = classLoader.loadClass("<GMA客户端完整类名>")

// 2. Hook getInstance() — 捕获单例
hook(getInstance).intercept { chain ->
    val result = chain.proceed()
    if (result != null) captureSingleton(result)
    result
}

// 3. Hook sendCommand(msg, callback) — 拦截发送
hook(sendCommand).intercept { chain ->
    val msg = chain.getArg(0)
    extractPdu(msg, outgoing = true)  // 在 proceed 之前提取
    val result = chain.proceed()
    hookCallback(chain.getArg(1))      // hook 回调以拦截接收
    result
}

// 4. Hook addObserver / addListener — 拦截回调注册
hook(addObserver).intercept { chain ->
    val result = chain.proceed()
    hookCallback(chain.getArg(1))      // hook 回调对象的所有方法
    result
}
```

**PDU 字段提取** (通过反射访问消息对象):
- `msg.nameSpace.value` → 命名空间 (int, 取低 8 位)
- `msg.commandId` → 命令 ID (int, 取低 8 位)
- `msg.commandData` → 载荷 (ByteArray 或 String)

### 3.3 Hook 点 2: GMA 回调对象

**目标**: 拦截所有注册到 GMA 客户端的回调对象的方法调用。

**实现要点**:
```kotlin
// 对回调对象的所有声明方法进行 hook
for (method in callback.javaClass.declaredMethods) {
    if (method.name in skipMethods) continue  // 跳过 Object 方法
    hook(method).intercept { chain ->
        // 收集参数 (在 proceed 前，但处理在 proceed 后)
        val args = (0 until method.parameterTypes.size).map { chain.getArg(i) }
        val result = chain.proceed()
        // 异步处理: 提取 ByteArray → 解析 PDU
        // 提取复杂对象 → 反射获取 nameSpace/commandId/commandData
        result
    }
}
```

**状态监听识别**: 回调对象中若含 `onStateChanged` 方法，其参数为连接状态枚举 (CONNECTING/CONNECTED/DISCONNECTED)，提取状态名用于连接检测。

### 3.4 Hook 点 3: BLE GATT 回调

**目标类特征**: 继承 `android.bluetooth.BluetoothGattCallback`，通常有主线程封装子类。

**实现要点**:

```kotlin
// 1. onConnectionStateChange — 连接状态
hook(onConnectionStateChange).intercept { chain ->
    val result = chain.proceed()
    val gatt = chain.getArg(0) as BluetoothGatt
    val newState = chain.getArg(2) as Int
    if (gatt.device.address == TARGET_MAC) {
        isConnected = (newState == 2)  // STATE_CONNECTED
    }
    result
}

// 2. onCharacteristicChanged — 接收通知
hook(onCharacteristicChanged).intercept { chain ->
    val result = chain.proceed()
    val characteristic = chain.getArg(1) as BluetoothGattCharacteristic
    if (matchesTarget(characteristic)) {
        val pdu = parsePdu(characteristic.value)  // 解析为 GMA PDU
        forwardToBridge(pdu)
    }
    result
}

// 3. onServicesDiscovered — 捕获特性对象
hook(onServicesDiscovered).intercept { chain ->
    val result = chain.proceed()
    // 查找目标 Service/Characteristic UUID，保存引用
    captureCharacteristics(gatt, SVC_UUID, WRITE_UUID, NOTIFY_UUID)
    result
}
```

### 3.5 Hook 点 4: BLE 写入拦截

```kotlin
// BluetoothGatt.writeCharacteristic
hook(writeCharacteristic).intercept { chain ->
    val characteristic = chain.thisObject as BluetoothGattCharacteristic
    val result = chain.proceed()
    if (matchesTarget(characteristic)) {
        val pdu = parsePdu(characteristic.value)
        forwardToBridge(pdu, outgoing = true)
    }
    result
}
```

### 3.6 Hook 点 5: BLE 连接入口

```kotlin
// BluetoothDevice.connectGatt — 捕获返回的 BluetoothGatt 实例
hook(connectGatt).intercept { chain ->
    val result = chain.proceed()
    if (device.address == TARGET_MAC) {
        captureGatt(result as BluetoothGatt)
    }
    result
}
```

### 3.7 主动发送命令

通过捕获的 GMA 客户端单例，反射调用其 `sendCommand` 方法向眼镜发送命令：

```kotlin
// 1. 获取 GMA 客户端的 ClassLoader 和相关类
// 2. 通过反射构造消息对象:
//    - 设置 namespace (通过枚举 from(byte) 方法)
//    - 设置 command (Byte)
//    - 设置 payload (String)
//    - 设置 requestType (枚举)
// 3. 调用 sendCommand(message, callback)
//    - callback 可为 null (fire-and-forget)
//    - 或提供回调以等待响应
```

---

## 4. TCP Bridge 设计

### 4.1 概述

在目标 App 进程内启动 TCP Server，将拦截到的 PDU 数据和设备信息通过 JSON-RPC 协议对外暴露。通过 TCP 连接访问这些数据。

### 4.2 数据结构

**PDU 历史缓冲**:
- 每条包含: `ns`, `cmd`, `hex` (完整 PDU hex), `timestamp`
- 累计计数器: `totalSentCount`, `totalRecvCount` (AtomicLong)

**设备信息缓存**:
- `ConcurrentHashMap<String, String>` — 存储设备设置项
- 通过解析 NS=255 的 `onDeviceResponse` 回调自动累积更新

### 4.3 JSON-RPC 命令列表

| 命令 | 说明 | 关键返回字段 |
|------|------|------------|
| `ping` | 心跳 | `{"type":"pong"}` |
| `pdu_history` | 获取 PDU 历史 | `sent[]`, `received[]`, `totalSent`, `totalRecv` |
| `diagnostics` | 完整诊断 | `gatt`, `gmaState`, `glassesActive`, `lastPduSec`, `hooks{}` |
| `status` | 快速状态 | `bridge`, `gmaActive`, `sentCount`, `recvCount` |
| `get_devices` | 设备信息缓存 | `count`, `devices{}` |
| `device_info` | 查询/强制刷新设备信息 | `force:true` 跳过缓存 |
| `send` | 发送 GMA 命令 | `ns`, `command`, `payload`, `dataType`, `wait` |
| `enable_adb` | 启用眼镜 ADB WiFi | — |
| `hook_status` | 各 Hook 安装状态 | 每个 Hook 的 `installed`, `connected` |
| `clear_logs` | 清空 PDU 缓冲 | `cleared:true` |

### 4.4 发送 GMA 命令格式

```json
{
  "type": "send",
  "ns": 8,
  "command": 1,
  "payload": "{\"device\":[{\"identifier\":\"speaker\",\"value\":\"{\\\"volume\\\":50}\"}]}",
  "dataType": "json",
  "wait": true
}
```

- `payload` 应为**已在外部 JSON 转义**的字符串
- `wait=true` 会阻塞等待眼镜响应，`wait=false` 仅发送
- `dataType`: `"json"` | `"cbor"` | `"raw"`

### 4.5 连接状态检测

连接状态综合以下两个信号判断：

| 信号 | 来源 | 说明 |
|------|------|------|
| GMA 状态 | `onStateChanged` 回调 | 最可靠，直接反映眼镜 BLE 连接 |
| BLE GATT | `onConnectionStateChange` | 底层蓝牙连接 |

---

## 5. 复现指南

### 5.1 环境准备

- Android 设备 (已 root，安装 LSPosed 等注入框架)
- 配套 App
- JDK 17+
- jadx

### 5.2 构建 Xposed 模块

```bash
# 1. 创建项目，配置 build.gradle.kts
#    - compileSdk 34+
#    - 依赖: libxposed API 101+, Compose BOM 2024.10+
#    - 参考: app/build.gradle.kts

# 2. 实现核心 Hook (参考第 3 节)
#    - GmaClientHooks: 拦截 GMA 客户端 + 回调
#    - GattCallbackHooks: 拦截 BLE GATT 回调
#    - GattWriteHooks: 拦截 BLE 写入

# 3. 实现 TCP Bridge (参考第 4 节)
#    - TcpBridgeServer: TCP 服务器
#    - BridgeProtocol: JSON-RPC 命令分发

# 4. 编译安装
./gradlew :app:assembleDebug
adb install app/build/outputs/apk/debug/app-debug.apk

# 5. LSPosed 管理器 → 启用模块 → 勾选目标 App → 重启目标 App
```

### 5.3 验证 Hook 生效

```
# 查看 LSPosed 日志
adb logcat | grep "LSPosedLogDaemon.*GmaClientHooks"
# 预期输出:
#   GMAClient found
#   getInstance hooked
#   sendCommand hooked
#   SEND ns=8 cmd=1 payload=...
#   RECV ns=16 cmd=1 payload=...

# 验证 TCP Bridge
adb shell "echo '{"type":"ping"}' | nc 127.0.0.1 37373"
# 预期: {"status":"ok","type":"pong"}
```

---

## 6. 附录

### 6.1 GMA 命名空间完整列表

```
 0: GMA                14: AGDS_TRANS        28: FACTORY_TEST
 1: VOICE              15: CY_APP            29: LOCAL_TIPS
 2: WIFI               16: GW_REQ            30: WAFT
 3: MUSIC              17: NOTIFICATION      31: BT_PROXY
 4: NAVI               18: OTA               32: ANT_AI
 5: TEL                19: PAY               33: LIVE_STREAM
 6: SET_WIFI           20: LOW_POWER         34: SCREENSHOT
 7: CAMERA             21: SHUTDOWN          35: TIMER
 8: SETTINGS           22: LONG_REC          36: GMA_COMMON
 9: TRANS              23: SYS_UT            37: DESKTOP_ICON
10: LISTEN_REC         24: SENSOR            38: TRACK_AR1
11: GET_WORD           25: GALLERY           39: LIVE_TALK
12: QUICK_RE           26: LOG_REPORT        40: BT_MULTI
13: AGDS_TRANS         27: XS_TEACH
```

### 6.2 设置标识符完整列表

```
actionFeedbackSound    calendarSync          cameraConfig
cameraHybridRaw        characterId           connection
fullDuplexDialog2      gmaOtaSettings        idleBackup
immersiveDialog        messageSync           musicConfig
navigator              navigatorConfig       phoneSetSync
postureConfig          power                 powerOnSetting
productCharacter       productTtsConfig      recordConfig
screen                 screenConfig          screenDisplay
shortcut_wakeup        soundEffect           speaker
vadConfig              wakeupConfig          wear
wearDetection          translateConfig       translateLang
aiTalkCameraConfig     bluetoothDualConfig   deviceMirrorSetting
glassAiTutorial        asrOnGW               asrRecordTime
lyricsDisplay          spatialEffect         eou
audioRecord            promptVolThre         ampsys
battery                customUiMode          customeDesktopApp
```

### 6.3 BLE UUID 参考

智能眼镜 BLE 通信使用的标准 UUID 模式：

```
Service:    0000FEB3-0000-1000-8000-00805F9B34FB
Write Char: 0000FED7-0000-1000-8000-00805F9B34FB
Notify Char: 0000FED8-0000-1000-8000-00805F9B34FB
```

不同厂商的眼镜可能使用不同的 UUID，可通过 BLE Scanner 工具在连接时捕获。

### 6.4 PDU 解析示例

Hex: `ff000801017b22646576696365223a5b5d7d`

```
ff          = channelId 255
00          = requestType 0 (REQUEST)
08          = namespace 8 (SETTINGS)
01          = command 1 (Set)
01          = dataType 1 (JSON)
7b22...7d   = payload: {"device":[]}
```
