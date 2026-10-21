鼠标电量  MouseBatteryTray  v1.0.6
==========================================

由 LogiBatteryTray 改写。

支持的数据源
  1. 罗技 HID++ 2.0（Unifying / Bolt / Lightspeed 接收器、直连、蓝牙）
  2. ATK / VXE / Compx 私有协议（部分国产 2.4G 鼠标，373B:1216 已实测）
  3. 蓝牙 GATT 电量服务 0x180F / 0x2A19
  4. 通用 HID 标准电量字段（0x06:0x20、0x84:0x29）

使用
  双击 MouseBatteryTray.exe，托盘出现电池图标。
  左键点击：显示所有设备的电量概览
  右键菜单：设备切换 / 立即刷新 / 刷新间隔 / 低电量提醒 /
            图标显示百分比 / 开机自动启动 / 打开日志 / 关于 / 退出

★ 首次运行提示"未知发布者"/"Windows 已保护你的电脑"？
  这是没有代码签名证书导致的，不是病毒。任选其一：
  1) 右键 exe → 属性 → 勾选"解除锁定" → 确定，然后再运行；
  2) 或 PowerShell 执行：Unblock-File .\MouseBatteryTray.exe
  3) 提示里点"更多信息 → 仍要运行"也可以，只是下次还会问。
  本程序不需要管理员权限（清单里是 asInvoker），所以不应出现 UAC 提权框；
  如果出现了，说明快捷方式勾了"以管理员身份运行"，取消即可。

★ 看不到托盘图标？按顺序检查
  1) 不要通过"沙盒 / 受限环境"启动（例如在 AI 助手的文件卡片里点"运行"）。
     低完整性进程会被 Windows 拒绝添加托盘图标，并且无法写设置和日志，
     表现为"程序在跑，但通知区域什么都没有"。
     → 请直接在资源管理器中双击本 exe，或按 Win+R 粘贴完整路径后回车。
     → 自检命令：MouseBatteryTray.exe --traycheck
       正常应输出 "托盘图标: 可以添加" 和 "可写=是"。
  2) Windows 11 默认把新出现的图标收进右下角的 ^ 里。
     点一下 ^ 就能看到，把它拖到任务栏即可长期固定；
     也可以到 设置 → 个性化 → 任务栏 → 其他系统托盘图标 里打开"鼠标电量"。

命令行
  MouseBatteryTray.exe --traycheck  检查托盘/数据目录是否正常（排查看不到图标）
  MouseBatteryTray.exe --watch 30   采集 30 秒原始 HID 报文（排查"读不到电量"）
  MouseBatteryTray.exe --dump       设备与协议诊断（并写入 dump.txt）
  MouseBatteryTray.exe --once       读取一次电量
  MouseBatteryTray.exe --selftest   自检（约 5 秒后退出，退出码 0 正常）
  MouseBatteryTray.exe --help       查看全部参数

数据位置
  设置：%APPDATA%\MouseBatteryTray\settings.json
  日志：%LOCALAPPDATA%\MouseBatteryTray\log.txt
  便携模式：exe 同目录放一个 portable.txt，改存到 exe\data\

依赖
  .NET Framework 4.8（Windows 10 1903+ / Windows 11 系统自带）

限制
  鼠标休眠时不响应私有协议查询，界面显示"休眠中"，移动鼠标唤醒即可。
  Windows 不允许应用读写打开鼠标/键盘顶层集合，通用 HID 电量仅部分设备可读。
  雷蛇 / 赛睿 / 罗技 G 系列 2.4G 等品牌私有协议尚未实现。
