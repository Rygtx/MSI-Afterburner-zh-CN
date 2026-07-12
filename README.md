# MSI Afterburner 简体中文本地化包

## 项目简介

本项目为 MSI Afterburner 提供简体中文界面翻译,基于 RTMUI(RivaTuner 用户可扩展本地化引擎)实现,无需修改应用程序本体即可完成界面、菜单、提示与帮助文档的本地化。

## 适配软件版本

| 项目 | 信息 |
|---|---|
| 软件名称 | MSI Afterburner |
| 软件版本 | 4.6.7.17352 |
| 文件版本 | 4.6.7.17352 |
| 版权年份 | © 2009-2025 |
| 本地化包标识 | CHN |
| 显示语言名 | Simplified Chinese |
| 代码页 | 936(GBK) |

> 本地化包仅适配上述版本的软件。若软件升级,请按 `SDK\Doc\Localization reference.md` 第 6 节的方法重新比对并更新。

## 项目结构

```
Localization\CHN\
├── Description                      本地化包描述文件(语言名/图标/创建者)
├── zhcn.ico                         语言图标
├── README.md                        本说明文档
├── Help\                            上下文帮助文件(234 个)
│   ├── BUTTON_*.txt                 主界面按钮提示(20 个)
│   ├── SLIDER_*.txt / TEXT_*.txt    滑块与文本提示(15 个)
│   ├── PLACEHOLDER_MON_WND          监控窗口占位说明
│   ├── Info\                        信息窗口帮助(5 个)
│   ├── Plugins\Monitoring\          监控插件帮助(8 个)
│   └── Properties\                  高级属性帮助
│       ├── Benchmark\               基准测试(6 个)
│       ├── Fan\                     风扇控制(8 个)
│       ├── General\                 通用设置(24 个)
│       ├── Monitoring\              监控设置(含 LayoutEditor、PluginsSettings、AlarmSettings 子目录)
│       ├── On-Screen Display\       屏幕显示(10 个)
│       ├── Profiles\                配置文件(10 个)
│       ├── Screen capture\          屏幕捕获(6 个)
│       ├── User interface\          用户界面(11 个)
│       └── Video capture\           视频捕获(28 个)
└── Translation\                     翻译数据库(35 个)
    ├── Localization\                语言下拉框中各语言名称的翻译(16 个)
    │   └── <LANG>\Description       每种语言对应一个 Description 文件
    ├── MSIAfterburner.exe\          主程序翻译
    │   ├── Dialogs                  对话框文本
    │   ├── Internal                 内部字符串
    │   ├── Menus                    菜单文本
    │   └── StringTable              字符串表
    ├── MSIAfterburner.cfg\          配置文件中的本地化文本
    ├── MSIAfterburner.dat\          数据文件中的本地化文本
    ├── MSIAfterburner.oem\          OEM 预设中的本地化文本
    ├── Plugins\Monitoring\          监控插件本地化文本
    │   ├── AIDA64.dll\
    │   ├── GPU.dll\
    │   ├── HwInfo.dll\
    │   ├── PerfCounter.dll\
    │   ├── Ping.dll\
    │   ├── PSU.dll\
    │   └── SMART.dll\
    └── Graphics\LCD\Fonts           LCD 字体配置
```

## 技术规范

### 编码与行结束符

- 所有文本文件统一使用 **GBK(代码页 936)** 编码
- 行结束符统一为 **CRLF**(`\r\n`)
- 无 BOM(字节顺序标记)
- 温度单位 `°C` 中的 `°` 符号使用 GBK 编码字节 `0xA1A3`

### 翻译数据库格式

翻译数据库文件使用以下令牌:

| 令牌 | 说明 |
|---|---|
| `#src <文本>` | 源文本(英文原文) |
| `#dst <文本>` | 译文(简体中文) |
| `#end` | 结束当前条目 |
| `#dlu <宽> [<高>]` | 可选,调整控件尺寸(对话框单位) |
| `#hst <类> [<ID>]` | 可选,限定翻译仅作用于指定控件类或控件 ID |

### 翻译原则

- 逐字逐句人工翻译,不使用机器翻译
- 保留 `%s`、`%d`、`%I64u` 等格式说明符及其顺序
- 保留 `%PRODUCTNAME%`、`%SERVERPRODUCTNAME%` 等变量占位符
- 保留单位与缩写(如 `FPS`、`OSD`、`LCD`、`BIOS`、`GUID` 等)
- 游戏与硬件术语对照官方中文资料翻译

## 安装与使用

### 安装

本本地化包已位于 `Localization\CHN\` 目录,无需额外安装。

### 选择简体中文

1. 启动 MSI Afterburner
2. 点击齿轮图标打开设置
3. 切换到 `<User interface>`(用户界面)选项卡
4. 在 `Language`(语言)下拉框中选择 `Simplified Chinese`
5. 点击 `Apply`(应用)并重启程序

## 验证方法

### 比较模式验证

按 `SDK\Doc\Localization reference.md` 第 6 节,使用比较模式验证本地化包完整性:

1. 在 `MSIAfterburner.cfg` 的 `[Settings]` 节中添加 `Language = CHN`
2. 在程序根目录执行:
   ```
   MSIAfterburner.exe /CL RUS
   ```
3. 程序将生成 `Localization\LocalizationDifferences.log` 差异报告
4. 分析报告中的差异项并处理
5. 验证完成后恢复配置文件(移除 `Language = CHN` 行)

### 编码验证

使用以下 PowerShell 脚本验证所有文件编码:

```powershell
$gbk = [System.Text.Encoding]::GetEncoding(936)
$chnRoot = "d:\Portable\MSI Afterburner\Localization\CHN"
$files = Get-ChildItem -Path $chnRoot -Recurse -File
# 检查 BOM、非 GBK 字节、LF-only 行结束符、乱码(??? / 锟斤拷)
# 期望结果:全部为 0
```

### 已知差异说明

与 RUS 参考包比较时,以下差异属正常现象:

- 图标文件不同(`zhcn.ico` 与 `RUS.ico`)
- `°C` 编码差异(GBK `°`=0xA1A3,Windows-1251 `°`=0xB0)导致比较工具假阳性
- RUS 包含的创建者信息条目

## 本地化调试

在 `MSIAfterburner.cfg` 中设置 `LocalizationDebugFlags` 可启用调试日志:

| 位 | 说明 |
|---|---|
| `0x00000001` | 启用日志系统,写入 `Localization\Localization.log` |
| `0x00000002` | 不记录正确条目 |
| `0x00000004` | 不记录跳过条目 |
| `0x00000008` | 不记录成功请求 |
| `0x00000010` | 不记录成功请求的源文件信息 |
| `0x00000020` | 不记录失败请求 |
| `0x00000040` | 高亮被 `#dlu` 调整尺寸的控件 |
| `0x00000080` | 在悬浮提示中显示控件信息(类、ID、尺寸) |

## 参考文档

- `SDK\Doc\Localization reference.md` - RTMUI 本地化参考指南 v1.2
