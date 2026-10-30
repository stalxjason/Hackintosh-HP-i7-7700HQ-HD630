# Hackintosh-i7-7700HQ-HD630-OpenCore

HP 笔记本黑苹果 OpenCore 引导配置（完美运行 macOS 13.7.8 Ventura）。

## 硬件配置 (Hardware Specs)

| 组件 | 型号 / 规格 | 状态与说明 |
| :--- | :--- | :--- |
| **设备类型** | 笔记本电脑（HP 系主板子系统 `0x103c`） | 内建屏幕 / 键盘 / 触控板等 |
| **CPU** | Intel Core i7-7700HQ @ 2.80GHz（4 核 8 线程，Kaby Lake） | 原生睿频与能耗调度 (X86PlatformPlugin) |
| **核显 (iGPU)** | Intel HD Graphics 630（1536 MB 显存） | 原生 Metal 3 硬件加速 |
| **独显 (dGPU)** | NVIDIA 独立显卡 | 通过 `SSDT-Disable_GPU_PEG0.aml` 屏蔽省电 |
| **声卡 (Audio)** | Realtek ALC295（layout-id=3） | 原生 AppleHDA + AppleALC，扬声器/耳机孔/麦克风正常 |
| **有线网卡 (Ethernet)**| Realtek RTL8168H/8111H PCI Express Gigabit Ethernet | 原生千兆连接 (en1) |
| **无线网卡 (Wi-Fi)** | **Realtek RTL8822BE** | **非原生免驱**。通过开源逆向驱动 `rtw88.kext` 驱动（虚拟为以太网 `en0`，不支持原生 AirDrop/接力） |
| **蓝牙 (Bluetooth)** | Realtek Bluetooth | 通过 `BlueToolFixup` + `RealtekBluetoothFirmware` 上传固件 |
| **存储 (SSD)** | Samsung SSD 960 EVO NVMe 250GB | 运行稳定，已启用 NVMeFix 优化 |
| **机型标识 (SMBIOS)** | MacBookPro14,3 | 官方原生对应机型 |
| **系统版本** | macOS 13.7.8 Ventura (Build 22H730) | 当前运行稳定 |

---

## 正常工作项 (Working)

- [x] Intel HD 630 核显硬件加速（Metal 3 支持）
- [x] 声音输出输入（内置扬声器、耳机孔、内置麦克风正常工作）
- [x] 键盘与触控板多点手势（VoodooPS2Controller）
- [x] 屏幕亮度调节快捷键（BrightnessKeys）
- [x] 电池电量状态与充电监控（SMCBatteryManager）
- [x] CPU 核心睿频与能耗调度（X86PlatformPlugin）
- [x] 千兆有线网卡工作正常
- [x] 蓝牙功能与固件上传（可连接蓝牙设备）
- [x] 睡眠与唤醒
- [x] 读卡器（Sinetek-rtsx）
- [x] 原生 SIP（System Integrity Protection）开启

---

## 局限与已知问题 (Limitations)

1. **Wi-Fi（非原生无线体验）**：
   * 本机板载无线网卡为 **Realtek RTL8822BE**，苹果系统未原生收录该芯片驱动。
   * 当前是依靠社区逆向的内核扩展插件 **`rtw88.kext`** 驱动，在系统层面上**被虚拟成了一个有线以太网设备（`en0 - Ethernet Internal`）**。
   * **功能受限**：由于不是走苹果的原生无线协议栈（CoreWLAN），因此**无法使用 macOS 菜单栏的原生 Wi-Fi 搜索面板**，同时**不支持隔空投送（AirDrop）、隔空播放（AirPlay）、随航（Sidecar）、接力（HandOff）等生态连续互通功能**。
   * *完美方案*：如需体验原生 Wi-Fi 与隔空投送，建议更换为拆机免驱网卡（如博通 BCM94360CS2/CD 加转接卡，或兼容 Intel 无线卡搭配 AirportItlwm）。

---

## 注意事项 (Notes)

1. **序列号保护**：为了防止序列号冲突，建议根据个人需求使用 GenSMBIOS 重新生成一套专属的序列号（MLB, SystemSerialNumber, SystemUUID）。
2. **系统升级建议**：当前 MacBookPro14,3 属于 macOS 13 Ventura 的官方最后支持机型，软硬件高度契合。若需升级到 Sonoma (macOS 14)，需伪装机型并使用 OCLP 修补核显驱动；**不建议升级至 macOS 26 (Tahoe)**（AppleHDA 已被苹果移除，会导致内置声卡彻底失效）。