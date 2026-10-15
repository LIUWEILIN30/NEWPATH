# NEWPATH

**中文** | [English](README_EN.md)

面向大幅面颗粒3D打印的Grasshopper参数化路径组件库。**1.0.4 · 26个组件 · Rhino 8 / Python 3**。

从几何切片、路径编辑和连续化，到料条预览与G-code生成，在Grasshopper中构建可检查、可调整的打印流程。组件名称与说明为中文，端口标识与分类保留英文。

## 下载与安装

前往 [GitHub Releases](https://github.com/LIUWEILIN30/NEWPATH/releases)，选择对应版本，下载 `NEWPATH_1.0.4.zip` 安装附件。附件由维护者上传；若尚未显示，请等待该版本发布。

解压后保存工作、关闭全部Rhino窗口，运行 `安装.ps1`。脚本校验插件、备份匹配的旧NEWPATH组件，并将 `NEWPATH.gha` 安装到 `%APPDATA%/Grasshopper/Libraries`。重启Rhino及Grasshopper后，从NEWPATH分类放置组件。

手动安装：将 `NEWPATH.gha` 放入Grasshopper的 Libraries Folder，先移走旧版NEWPATH的ghuser及 `NEWPATH.Toolbar.gha`，避免重复入口。图标和算法均已内嵌，不依赖外置源码。已有画布实例不会自动升级，需放置新版并重新连接。

## 1.0.4 更新

保留1.0.3全部26项，改为单个GHA提供独立注册组件。包含连续路径、开放连续路径、UV方向切片、生成brim、G-code生成、上传及公共运行时更新。

104个输入端口均有中文用途说明，50个固定默认值写入端口；动态默认值按提示由算法计算。G-code生成与导出直接放置编译组件，不再自动展开ghuser输入组。默认参数遵循算法定义，使用前须按设备核对。

## 使用与环境

典型流程：智能切片 → 路径分组 → 按需编辑路径 → 连续路径 → 可选PATH转G1或G2/G3 → G-code生成 → 导出或上传。使用Path数据端口连接；Preview用于观察。开放连续路径和UV方向切片按各自输入条件选择。

已在Windows Rhino 8.20、.NET 8、Grasshopper及Rhino Python 3环境验证。未验证Mac或Rhino的.NET Framework启动模式。内嵌算法仍使用Python 3执行，不提供源码保密保证。

PATH数据应由上游重算，不应Internalise跨会话保存；连续路径保存重开后需重新触发Run。上传默认为False，不会因安装或放置组件启动打印。示例见安装包内“示例”目录。

设备测试范围仅为金石FGF 1800大幅面颗粒打印机，未验证机械臂及桌面级高速打印机。生成指令前核对毫米单位、设备行程、速度、温度、挤出参数及控制器圆弧支持。

## 验证与许可

1.0.4交付记录：100项离线回归通过，21个PATH组件结构检查零错误；26个组件通过注册、新建实例、图标、源码、端口和默认值检查。中文说明在求解及归档回读后保留，代表链路完成保存重开与开发目录隔离验证。

本版未执行网络上传、机器打印或操作系统重启后的安装验收。上述记录对应交付时的测试条件，不代表其他设备或任意画布均已验证。

本仓库维护图标和双语说明；安装附件通过GitHub Releases分发。采用 [MIT License](LICENSE)。
