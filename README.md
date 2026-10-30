# Hackintosh-i7-7700HQ-HD630-OpenCore

HP 笔记本黑苹果 OpenCore 引导配置（完美运行 macOS 13.7.8 Ventura）。

## 硬件配置 (Hardware Specs)

| 组件 | 型号 / 规格 |
| :--- | :--- |
| **设备类型** | 笔记本电脑（HP 系主板子系统 `0x103c`） |
| **CPU** | Intel Core i7-7700HQ @ 2.80GHz（4 核 8 线程，Kaby Lake） |
| **核显 (iGPU)** | Intel HD Graphics 630（1536 MB 显存，原生 Metal 3 加速） |
| **独显 (dGPU)** | NVIDIA 独立显卡（通过 SSDT-Disable_GPU_PEG0 屏蔽以节省电量） |
| **声卡 (Audio)** | Realtek ALC295（AppleALC layout-id=3，扬声器/耳机孔/麦克风正常） |
| **无线网卡 (Wi-Fi)** | Realtek RTL8822BE（通过 rtw88.kext 驱动） |
| **蓝牙 (Bluetooth)** | Realtek Bluetooth（通过 BlueToolFixup + RealtekBluetoothFirmware） |
| **有线网卡 (Ethernet)**| Realtek RTL8168H/8111H PCI Express Gigabit Ethernet |
| **存储 (SSD)** | Samsung SSD 960 EVO NVMe 250GB |
| **机型标识 (SMBIOS)** | MacBookPro14,3 |
| **系统版本** | macOS 13.7.8 Ventura (Build 22H730) |

---

## 正常工作项 (Working)

- [x] Intel HD 630 核显硬件加速（Metal 3 支持）
- [x] 声音输出输入（内置扬声器、耳机孔、内置麦克风）
- [x] 键盘与触控板多点手势（VoodooPS2Controller）
- [x] 屏幕亮度调节快捷键（BrightnessKeys）
- [x] 电池电量状态与充电监控（SMCBatteryManager）
- [x] CPU 核心睿频与能耗调度（X86PlatformPlugin）
- [x] 千兆有线网卡与无线网络连接
- [x] 蓝牙功能与固件上传
- [x] 睡眠与唤醒
- [x] 读卡器（Sinetek-rtsx）
- [x] 原生 SIP（System Integrity Protection）开启

---

## 注意事项 (Notes)

1. **序列号保护**：为了防止序列号冲突，上传至公开仓库前建议检查或使用 GenSMBIOS 重新生成一套专属的序列号（MLB, SystemSerialNumber, SystemUUID）。
2. **系统升级建议**：当前 MacBookPro14,3 属于 macOS 13 Ventura 的官方最后支持机型，软硬件高度契合。若需升级到 Sonoma (macOS 14)，需伪装机型并使用 OCLP 修补核显驱动。