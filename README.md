# NEWPATH

**中文** | [English](README_EN.md)

面向大幅面颗粒 3D 打印的 Grasshopper 参数化路径组件库。**1.2.7 · 37 个组件 · Rhino 8 / Python 3**

NEWPATH 将几何切片、路径编辑、打印顺序组织、料条预览与 G-code 输出连接在同一张 Grasshopper 画布中。你可以从模型或手绘层线出发，按设计需要组合组件，逐步检查和调整打印路径。

## 核心价值

- **让路径携带工艺信息**：PATH 数据包统一传递曲线、层号、逐点高度、料条宽度及运动信息，使切片、编辑、预览和输出共享同一套数据。
- **按构造需求设计路径**：组合双层墙、结构肋、端部承托、brim 和填充，调整局部构造；通过分组、连续路径或逐层打印组织执行顺序。
- **输出前看清打印过程**：查看路径曲线、逐点运动与料条网格，并用 G-code 运动模拟检查指令对应的打印头运动。
- **直接在 Grasshopper 使用**：单个 `NEWPATH.gha` 提供 37 个独立组件，算法与图标已内嵌。组件名称和说明为中文，端口标识保留英文，分类采用七个中文分组。

## 下载与安装

运行环境：**Windows、Rhino 8.20 或更新版本、Grasshopper 与 Rhino Python 3**。本版将全部 37 个组件整合为同一份 net48 GHA，供 Framework 和 Core 模式使用；实际验收范围见下文。Mac 尚未验证。

1. 前往[GitHub Releases](https://github.com/LIUWEILIN30/NEWPATH/releases)下载 `NEWPATH_1.2.7.gha`；附件未出现表示尚未上传。不再提供安装ZIP。
2. 保存工作并关闭全部Rhino窗口，将旧版NEWPATH GHA及独立的 `NEWPATH.DeviceSettings.gha` / `NEWPATH.Toolbar.gha`移到Libraries以外备份，再复制唯一一份当前GHA到Libraries。
3. 重启Rhino与Grasshopper，从七个中文分类中放置组件；机器人组设 primary、secondary、tertiary 三栏。

手动安装时，在 Grasshopper 中打开 **File → Special Folders → Libraries Folder**，放入 `NEWPATH_1.2.7.gha`；先移走旧版 NEWPATH 的 ghuser、`NEWPATH.Toolbar.gha` 和独立的 `NEWPATH.DeviceSettings.gha`，避免重复入口。无需另装图标或外置 Python 源码，算法执行仍依赖 Rhino Python 3。

切换运行时可执行 `SetDotNetRuntime` 后重启 Rhino，无需更换 GHA。同版本更新也需关闭全部 Rhino、替换文件并重启。旧 ghuser/Python 实例保留自身代码，已有 GHA 实例按原 GUID 解析；配套输入更新需从新版工具栏新放置组件并重新连接。输入端口可悬停查看中文用途与默认行为；接线后使用外部输入。

## 使用流程

根据目标选择打印组织方式：

- **连续打印**：智能切片 → 路径分组 → 按需编辑路径 → 连续路径 → 可选 PATH转G1 / PATH转G2 G3 → Path G-code生成 → G-code导出或上传。
- **多物体逐层打印**：切片 → 按需编辑路径 → 可选 PATH转G1 / PATH转G2 G3 → 逐层打印 → Path G-code生成 → G-code导出或上传。输入模型须使用共同层号基准；同层按 brim、填充、外壁排序。逐层打印接收尚未连续化的路径，其后直接进入 G-code 生成，避免再次分组或连续化改变顺序。

开放轮廓使用“开放连续路径”；需要沿曲面参数方向生成层线时使用“UV方向切片”；已有手绘平行层线可用“打包Path”进入流程。各组件有对应的输入条件，详细参数见端口提示。

处理节点之间连接 **Path** 数据端口；`Preview` 用于观察几何，不能替代携带工艺属性的数据包。PATH 数据应由上游重算，不应通过 Internalise 跨会话保存；连续路径保存重开后需重新触发 `Run`。

生成 G-code 前，核对毫米单位、设备行程、温度、速度和挤出参数；圆弧输出须确认控制器支持 XY 平面 G2/G3。上传组件使用 Moonraker，需要填写自己的设备地址及所需密钥，`Upload` 默认关闭，上传不会启动打印。历史设备测试范围为金石 FGF 1800 大幅面颗粒打印机，其他设备需自行适配；**1.1.0 尚未进行实机打印**。

## 全部组件

37 个组件按工具栏中的七个分类排列。图标以 80×80 显示；仓库中提供组件图标的 512×512 透明 PNG，模板素材库图标为嵌入位图（1\.2\.3 起）。全部组件通过同一个 GHA 安装。

### 1 切片 · 切片

| 图标 | 组件 | 功能 |
|---|---|---|
| <img src="%E5%9B%BE%E6%A0%87/uv_slicer_path.png" width="80" height="80" alt="UV方向切片"> | **UV方向切片** | 沿曲面的指定参数方向生成保形切片，并记录逐点高度和生长方向。 |
| <img src="%E5%9B%BE%E6%A0%87/smart_slicer_path.png" width="80" height="80" alt="智能切片"> | **智能切片** | 按层高与端部平面切片几何，输出携带料条宽度和逐点属性的路径。 |
| <img src="%E5%9B%BE%E6%A0%87/pack_path.png" width="80" height="80" alt="打包Path"> | **打包Path** | 把手绘平行层线打包为保留实际层高和来源的 Path 数据。 |

### 2 Path编辑 · 路径编辑

| 图标 | 组件 | 功能 |
|---|---|---|
| <img src="%E5%9B%BE%E6%A0%87/double_wall_path.png" width="80" height="80" alt="双层墙路径"> | **双层墙路径** | 向闭合墙线内侧偏移，并在指定连接点生成双层墙路径。 |
| <img src="%E5%9B%BE%E6%A0%87/surface_rib_path.png" width="80" height="80" alt="双道结构肋"> | **双道结构肋** | 按定位点、层范围和悬垂角，在墙体内侧生成双道往返结构肋。 |
| <img src="%E5%9B%BE%E6%A0%87/brim_skirt_path.png" width="80" height="80" alt="生成brim"> | **生成brim** | 沿落地外轮廓向外生成多圈贴边裙边，并保留原模型路径。 |
| <img src="%E5%9B%BE%E6%A0%87/skirt_path.png" width="80" height="80" alt="生成包围brim"> | **生成包围brim** | 在全部模型底部生成闭合的包围 brim。 |
| <img src="%E5%9B%BE%E6%A0%87/end_offset_path.png" width="80" height="80" alt="端部偏移"> | **端部偏移** | 按指定层范围逐层向内或向外偏移墙体端部。 |
| <img src="%E5%9B%BE%E6%A0%87/end_bearing_path.png" width="80" height="80" alt="端部承托"> | **端部承托** | 在指定端部层范围内逐层加厚双层墙，形成承托面。 |
| <img src="%E5%9B%BE%E6%A0%87/bottom_hatch_fill.png" width="80" height="80" alt="蛇形填充"> | **蛇形填充** | 按指定层范围和扫描间距，为切片路径添加蛇形底部填充。 |
| <img src="%E5%9B%BE%E6%A0%87/spiral_infill_path.png" width="80" height="80" alt="螺旋填充"> | **螺旋填充** | 在首个模型组的指定层添加螺旋填充，并保留全部原路径。 |

### 3 Path组织 · 路径组织

| 图标 | 组件 | 功能 |
|---|---|---|
| <img src="%E5%9B%BE%E6%A0%87/open_continuous_path.png" width="80" height="80" alt="开放连续路径"> | **开放连续路径** | 将同组逐层开放曲线交替反向连接为往返打印路径。 |
| <img src="%E5%9B%BE%E6%A0%87/path_grouping.png" width="80" height="80" alt="路径分组"> | **路径分组** | 按指定高度重新划分打印块，并可按块显示不同颜色。 |
| <img src="%E5%9B%BE%E6%A0%87/continuous_path.png" width="80" height="80" alt="连续路径"> | **连续路径** | 按引导点调整闭环接缝，将同组切片连接为连续挤出路径。 |
| <img src="%E5%9B%BE%E6%A0%87/curve_arc_classifier.png" width="80" height="80" alt="PATH转G2 G3"> | **PATH转G2 G3** | 将路径拟合为直线和圆弧，并按公差及段长限制重建路径。 |
| <img src="%E5%9B%BE%E6%A0%87/path_to_g1.png" width="80" height="80" alt="PATH转G1"> | **PATH转G1** | 按长度或特征采样将 Path 曲线离散为 G1 路径，保留逐点属性。 |
| <img src="%E5%9B%BE%E6%A0%87/layer_print_order.png" width="80" height="80" alt="逐层打印"> | **逐层打印** | 按真实层号逐层组织打印，同层依次排列brim、填充和外壁。 |

### 4 导出 · 输出

| 图标 | 组件 | 功能 |
|---|---|---|
| <img src="%E5%9B%BE%E6%A0%87/gcode_upload.png" width="80" height="80" alt="G-code上传"> | **G-code上传** | 将 G-code 保存到本地并上传至指定打印设备。 |
| <img src="%E5%9B%BE%E6%A0%87/gcode_export.png" width="80" height="80" alt="G-code导出"> | **G-code导出** | 分析打印时间和材料用量，并按按钮将指令保存到指定目录。 |
| <img src="%E5%9B%BE%E6%A0%87/path_gcode.png" width="80" height="80" alt="Path G-code生成"> | **Path G-code生成** | 根据连续路径及速度、温度和挤出参数生成打印指令。 |

### 5 分析 · 分析

| 图标 | 组件 | 功能 |
|---|---|---|
| <img src="%E5%9B%BE%E6%A0%87/gcode_motion.png" width="80" height="80" alt="Path逐点运动"> | **Path逐点运动** | 读取路径原始点列，输出逐点坐标、索引和入边运动类型。 |
| <img src="%E5%9B%BE%E6%A0%87/unpack_path.png" width="80" height="80" alt="解包Path"> | **解包Path** | 将路径数据包拆为层平面、曲线、名义层高和原始点列。 |
| <img src="%E5%9B%BE%E6%A0%87/rounded_mesh_pipe.png" width="80" height="80" alt="路径预览"> | **路径预览** | 按 Path 的逐点宽度和高度预览打印料条网格。 |
| <img src="%E5%9B%BE%E6%A0%87/Gcode%E8%BF%90%E5%8A%A8%E6%A8%A1%E6%8B%9F_gcode_simulator.png" width="80" height="80" alt="Gcode运动模拟"> | **Gcode运动模拟** | 解析 G-code 并按速度播放打印头运动，显示已走路径。 |

### 6 工具 · 辅助工具

| 图标 | 组件 | 功能 |
|---|---|---|
| <img src="%E5%9B%BE%E6%A0%87/print_bed.png" width="80" height="80" alt="打印床"> | **打印床** | 按设备名称和长宽显示打印床，并输出世界水平基准面。 |
| <img src="%E5%9B%BE%E6%A0%87/%E6%B0%B4%E5%B9%B3%E5%81%8F%E7%A7%BB%E6%9B%B2%E9%9D%A2_horizontal_offset_surface.png" width="80" height="80" alt="水平偏移曲面"> | **水平偏移曲面** | 按水平距离偏移曲面，可封闭为墙体。 |

### 7 机器人 · 机器人

机器人组按 primary、secondary、tertiary 分为三栏。

| 图标 | 组件 | 功能 |
|---|---|---|
| <img src="图标/base_inverse.png" width="80" height="80" alt="基座坐标逆变换"> | **基座坐标逆变换** | 单线 RS 输入与输出，对示教器基座位姿取逆，其余系统数据透传。 |
| <img src="图标/tool_setup.png" width="80" height="80" alt="Tool与Base设置"> | **Tool与Base设置** | 将工具 TCP、模型、负载、打印姿态及 TOOL/BASE 编号保存为独立配置包。 |
| <img src="图标/joint_pose.png" width="80" height="80" alt="Joint Pose"> | **设备六轴姿态** | 编辑、切换并保存设备六轴姿态预设。 |
| <img src="图标/path_robim_targets.png" width="80" height="80" alt="机器人目标生成"> | **Path机器人目标生成** | 从 Path 和工具基座配置包生成起始、打印、返回目标序列。 |
| <img src="图标/robim_playback.png" width="80" height="80" alt="播放控制"> | **RoBIM播放控制** | 按程序时长提供暂停、定位、倍速及归零的模拟时间控制。 |
| <img src="图标/robot_program_export.png" width="80" height="80" alt="程序导出"> | **机器人程序导出** | 校验名称，追加纯数字迭代后缀，统一程序声明、调用和文件名后导出。 |
| <img src="图标/load_robot_system.png" width="80" height="80" alt="加载机器人系统"> | **加载机器人系统** | 加载指定型号的机器人系统，供目标生成、程序计算与仿真使用。 |
| <img src="图标/degrees_to_radians.png" width="80" height="80" alt="角度转弧度"> | **角度转弧度** | 将六轴角度转换为可连接机器人目标生成的弧度数据。 |
| <img src="图标/create_program.png" width="80" height="80" alt="创建程序"> | **创建程序** | 根据机器人系统和目标序列计算运动并生成 KUKA KRL 程序。 |
| <img src="图标/program_simulation.png" width="80" height="80" alt="程序模拟"> | **程序模拟** | 按程序时间输出机器人网格、TCP 平面与六轴角度。 |
| <img src="图标/template_library.png" width="80" height="80" alt="模板素材库"> | **模板素材库** | 管理、导入和导出 tem 模板与 GH 画布素材。 |

加载机器人系统、角度转弧度、创建程序和程序模拟是本库自研组件。Path机器人目标生成的 `RobotMode` 默认为 `NEWPATH`，输出项目目标数据供 NEWPATH 创建程序使用；选择 `RoBIM` 时输出原生目标，连接兼容版本的 RoBIM 原生创建程序。两套机器人组件按整套链路使用；RoBIM 模式需要安装兼容插件，缺插件时会报错并清空输出。

新放置组件的 `PtpSpeed` 配套滑块默认 10%，仍由用户控制；超过 50% 时提示警告。返回点 `EndTarget` 必须高于所有目标。PTP 百分比不代表 TCP 毫米每秒，默认值不等于碰撞安全或真机验收。

程序导出的 Name 接在导出组件，Create program 可保留内置占位名称。迭代名例如 `TEST001`、`TEST002`，KUKA 模块名称同时检查 24 字符限制。

## 版本变化

**1.2.7（2026-11-04）** 新增 `RobotMode` 双模式与机器人配套输入；新放置的 PTP 速度默认 10%，StartPause 使用滑块。组件总数仍为 37。

**1.1.0（2026-10-20）** 将已验收的路径更新及 6 个机器人组件整合为单个 GHA，共 32 个组件；新增第七分类及两栏，保留原六组分栏。单螺旋填充实验不包含在本版，原有“螺旋填充”仍保留。

- 目标生成、播放控制、程序导出新放置时分别携带 11、4、3 个输入对象。
- G-code生成、导出、上传的配套输入和新增三组件统一按实际端口对齐；共 40 个输入。对齐只在新建及首次求解时完成，不持续限制拖动；保存重开和复制不重复创建输入。
- Tool与Base设置 只需一根配置包连接；程序导出统一配置名称，迭代后缀只有数字，并修复网络共享根目录临时文件清理误报。
- 机器人组件配齐透明图标；打包Path的 Role 使用当前整数选项（0–3）。

**1.0.5（2026-10-15，更新至 2026-10-16）** 修复 .NET Framework 模式加载问题，保留全部 26 个组件的 GUID 和端口，并增加以下更新：

- 从工具栏新放置 G-code生成、导出、上传时，分别自动生成并连接 14、4、4 个配套输入对象，保留数值、范围、精度、文本和选项；Path/GCode 数据端口仍需接线。保存重开和复制已有组件不重复生成输入。
- G-code生成按实际连线端点校正配套输入：每条线两端同高，输入对象的输出端同列。放置和首次求解后校正，不持续约束后续拖动，也不重排已有画布。
- 智能切片的 `Planes` 输入支持 .NET `IList`。
- 六个工具栏分类各分为两栏，组件身份和端口保持不变。

**1.0.4（2026-10-15）** 保留 1.0.3 的全部 26 项功能，改为单个 GHA 中的独立注册组件，并更新连续路径、开放连续路径、UV方向切片、生成brim、G-code生成与上传及公共运行时。相较 1.0.2 的 25 个组件，现版包含“逐层打印”，用于按真实层号组织多物体的打印顺序。

此前记录的 131 个输入端口提供用途说明，既有固定默认值随当前接口更新并写入端口；动态默认值按提示由算法计算。默认值以当前算法定义为准，旧示例中的自定义参数不会自动迁移。

## 验证与许可

本仓库提供双语说明与图标，安装附件通过 [GitHub Releases](https://github.com/LIUWEILIN30/NEWPATH/releases) 分发。采用 [MIT License](LICENSE)。GHA 内嵌运行所需的 Python 算法，不提供源码保密保证。
