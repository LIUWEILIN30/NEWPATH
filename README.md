# NEWPATH

**中文** | [English](README_EN.md)

面向大幅面颗粒 3D 打印的 Grasshopper 参数化路径组件库。**1.0.5 · 26 个组件 · Rhino 8 / Python 3**

NEWPATH 将几何切片、路径编辑、打印顺序组织、料条预览与 G-code 输出连接在同一张 Grasshopper 画布中。你可以从模型或手绘层线出发，按设计需要组合组件，逐步检查和调整打印路径。

## 核心价值

- **让路径携带工艺信息**：PATH 数据包统一传递曲线、层号、逐点高度、料条宽度及运动信息，使切片、编辑、预览和输出共享同一套数据。
- **按构造需求设计路径**：组合双层墙、结构肋、端部承托、brim 和填充，调整局部构造；通过分组、连续路径或逐层打印组织执行顺序。
- **输出前看清打印过程**：查看路径曲线、逐点运动与料条网格，并用 G-code 运动模拟检查指令对应的打印头运动。
- **直接在 Grasshopper 使用**：单个 `NEWPATH.gha` 提供 26 个独立组件，算法与图标已内嵌。组件名称和说明为中文，端口标识与分类保留英文。

## 下载与安装

运行环境：**Windows、Rhino 8、Grasshopper 与 Rhino Python 3**。最低宿主依赖为 Rhino 8.20，尚未实现全部 Rhino 8 版本兼容。1.0.5 以 net48 编译，同一个 GHA 用于 .NET Framework 和 .NET Core；早期构建已在 Rhino 8.20 / Framework 4.8 与 .NET 8.0.14 两种独立进程验证。最新构建的验收边界见下文；Mac 尚未验证。

1. 前往 [GitHub Releases](https://github.com/LIUWEILIN30/NEWPATH/releases)，下载 `NEWPATH_1.0.5_安装包.zip` 安装附件；若列表中尚无该附件，表示尚未上传。
2. 解压，保存工作并关闭全部 Rhino 窗口，运行包内 `安装.ps1`。脚本校验插件，备份匹配的旧 NEWPATH 组件，并安装 `NEWPATH.gha`。
3. 重启 Rhino 与 Grasshopper，从 **NEWPATH** 工具栏的六个分类中放置组件。六个分类各使用两栏显示。

手动安装时，在 Grasshopper 中打开 **File → Special Folders → Libraries Folder**，放入 `NEWPATH.gha`；先移走旧版 NEWPATH 的 ghuser 和 `NEWPATH.Toolbar.gha`，避免重复入口。无需另装图标或外置 Python 源码，算法执行仍依赖 Rhino Python 3。

切换运行时可执行 `SetDotNetRuntime` 后重启 Rhino，无需更换 GHA。同版本更新也需关闭全部 Rhino、替换文件并重启。旧 ghuser/Python 实例保留自身代码，已有 GHA 实例按原 GUID 解析；配套输入更新需从新版工具栏新放置组件并重新连接。输入端口可悬停查看中文用途与默认行为；接线后使用外部输入。

## 使用流程

根据目标选择打印组织方式：

- **连续打印**：智能切片 → 路径分组 → 按需编辑路径 → 连续路径 → 可选 PATH转G1 / PATH转G2 G3 → Path G-code生成 → G-code导出或上传。
- **多物体逐层打印**：切片 → 按需编辑路径 → 可选 PATH转G1 / PATH转G2 G3 → 逐层打印 → Path G-code生成 → G-code导出或上传。输入模型须使用共同层号基准；同层按 brim、填充、外壁排序。逐层打印接收尚未连续化的路径，其后直接进入 G-code 生成，避免再次分组或连续化改变顺序。

开放轮廓使用“开放连续路径”；需要沿曲面参数方向生成层线时使用“UV方向切片”；已有手绘平行层线可用“打包Path”进入流程。各组件有对应的输入条件，详细参数见端口提示。

处理节点之间连接 **Path** 数据端口；`Preview` 用于观察几何，不能替代携带工艺属性的数据包。PATH 数据应由上游重算，不应通过 Internalise 跨会话保存；连续路径保存重开后需重新触发 `Run`。

生成 G-code 前，核对毫米单位、设备行程、温度、速度和挤出参数；圆弧输出须确认控制器支持 XY 平面 G2/G3。上传组件使用 Moonraker，需要填写自己的设备地址及所需密钥，`Upload` 默认关闭，上传不会启动打印。历史设备测试范围为金石 FGF 1800 大幅面颗粒打印机，其他设备需自行适配；**1.0.5 尚未进行实机打印**。

## 全部组件

26 个组件按工具栏中的六个分类排列。图标以 80×80 显示，仓库中提供 512×512 透明 PNG；全部组件通过同一个 GHA 安装。

### 1 Slice · 切片

| 图标 | 组件 | 功能 |
|---|---|---|
| <img src="%E5%9B%BE%E6%A0%87/uv_slicer_path.png" width="80" height="80" alt="UV方向切片"> | **UV方向切片** | 沿曲面的指定参数方向生成保形切片，并记录逐点高度和生长方向。 |
| <img src="%E5%9B%BE%E6%A0%87/smart_slicer_path.png" width="80" height="80" alt="智能切片"> | **智能切片** | 按层高与端部平面切片几何，输出携带料条宽度和逐点属性的路径。 |
| <img src="%E5%9B%BE%E6%A0%87/pack_path.png" width="80" height="80" alt="打包Path"> | **打包Path** | 把手绘平行层线打包为保留实际层高和来源的 Path 数据。 |

### 2 Path Edit · 路径编辑

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

### 3 Path Assembly · 路径组织

| 图标 | 组件 | 功能 |
|---|---|---|
| <img src="%E5%9B%BE%E6%A0%87/open_continuous_path.png" width="80" height="80" alt="开放连续路径"> | **开放连续路径** | 将同组逐层开放曲线交替反向连接为往返打印路径。 |
| <img src="%E5%9B%BE%E6%A0%87/path_grouping.png" width="80" height="80" alt="路径分组"> | **路径分组** | 按指定高度重新划分打印块，并可按块显示不同颜色。 |
| <img src="%E5%9B%BE%E6%A0%87/continuous_path.png" width="80" height="80" alt="连续路径"> | **连续路径** | 按引导点调整闭环接缝，将同组切片连接为连续挤出路径。 |
| <img src="%E5%9B%BE%E6%A0%87/curve_arc_classifier.png" width="80" height="80" alt="PATH转G2 G3"> | **PATH转G2 G3** | 将路径拟合为直线和圆弧，并按公差及段长限制重建路径。 |
| <img src="%E5%9B%BE%E6%A0%87/path_to_g1.png" width="80" height="80" alt="PATH转G1"> | **PATH转G1** | 按长度或特征采样将 Path 曲线离散为 G1 路径，保留逐点属性。 |
| <img src="%E5%9B%BE%E6%A0%87/layer_print_order.png" width="80" height="80" alt="逐层打印"> | **逐层打印** | 按真实层号逐层组织打印，同层依次排列brim、填充和外壁。 |

### 4 Export · 输出

| 图标 | 组件 | 功能 |
|---|---|---|
| <img src="%E5%9B%BE%E6%A0%87/gcode_upload.png" width="80" height="80" alt="G-code上传"> | **G-code上传** | 将 G-code 保存到本地并上传至指定打印设备。 |
| <img src="%E5%9B%BE%E6%A0%87/gcode_export.png" width="80" height="80" alt="G-code导出"> | **G-code导出** | 分析打印时间和材料用量，并按按钮将指令保存到指定目录。 |
| <img src="%E5%9B%BE%E6%A0%87/path_gcode.png" width="80" height="80" alt="Path G-code生成"> | **Path G-code生成** | 根据连续路径及速度、温度和挤出参数生成打印指令。 |

### 5 Analyze · 分析

| 图标 | 组件 | 功能 |
|---|---|---|
| <img src="%E5%9B%BE%E6%A0%87/gcode_motion.png" width="80" height="80" alt="Path逐点运动"> | **Path逐点运动** | 读取路径原始点列，输出逐点坐标、索引和入边运动类型。 |
| <img src="%E5%9B%BE%E6%A0%87/unpack_path.png" width="80" height="80" alt="解包Path"> | **解包Path** | 将路径数据包拆为层平面、曲线、名义层高和原始点列。 |
| <img src="%E5%9B%BE%E6%A0%87/rounded_mesh_pipe.png" width="80" height="80" alt="路径预览"> | **路径预览** | 按 Path 的逐点宽度和高度预览打印料条网格。 |
| <img src="%E5%9B%BE%E6%A0%87/Gcode%E8%BF%90%E5%8A%A8%E6%A8%A1%E6%8B%9F_gcode_simulator.png" width="80" height="80" alt="Gcode运动模拟"> | **Gcode运动模拟** | 解析 G-code 并按速度播放打印头运动，显示已走路径。 |

### 6 Utilities · 辅助工具

| 图标 | 组件 | 功能 |
|---|---|---|
| <img src="%E5%9B%BE%E6%A0%87/print_bed.png" width="80" height="80" alt="打印床"> | **打印床** | 按设备名称和长宽显示打印床，并输出世界水平基准面。 |
| <img src="%E5%9B%BE%E6%A0%87/%E6%B0%B4%E5%B9%B3%E5%81%8F%E7%A7%BB%E6%9B%B2%E9%9D%A2_horizontal_offset_surface.png" width="80" height="80" alt="水平偏移曲面"> | **水平偏移曲面** | 按水平距离偏移曲面，可封闭为墙体。 |

## 版本变化

**1.0.5（2026-10-15，更新至 2026-10-16）** 修复 .NET Framework 模式加载问题，保留全部 26 个组件的 GUID 和端口，并增加以下更新：

- 从工具栏新放置 G-code生成、导出、上传时，分别自动生成并连接 14、4、4 个配套输入对象，保留数值、范围、精度、文本和选项；Path/GCode 数据端口仍需接线。保存重开和复制已有组件不重复生成输入。
- G-code生成按实际连线端点校正配套输入：每条线两端同高，输入对象的输出端同列。放置和首次求解后校正，不持续约束后续拖动，也不重排已有画布。
- 智能切片的 `Planes` 输入支持 .NET `IList`。
- 六个工具栏分类各分为两栏，组件身份和端口保持不变。

**1.0.4（2026-10-15）** 保留 1.0.3 的全部 26 项功能，改为单个 GHA 中的独立注册组件，并更新连续路径、开放连续路径、UV方向切片、生成brim、G-code生成与上传及公共运行时。相较 1.0.2 的 25 个组件，现版包含“逐层打印”，用于按真实层号组织多物体的打印顺序。

104 个输入端口均提供中文用途说明，50 个固定默认值写入端口；动态默认值按提示由算法计算。默认值以当前算法定义为准，旧示例中的自定义参数不会自动迁移。

## 验证与许可

1.0.4 交付时完成了 100 项离线回归、21 个 PATH 组件的结构检查，以及全部 26 个组件的注册、新建实例、图标、源码、端口和默认值检查。代表链路覆盖双物体逐层打印、brim 与填充后的连续路径、UV 切片后的开放连续路径，并完成保存重开及开发目录隔离验证。

1.0.5 早期兼容性构建在 Rhino 8.20 的 Framework 4.8 与 .NET 8.0.14 独立进程中，均通过全部 26 个组件注册、端口与默认值检查，以及打包 → G1转换 → 逐层打印 → G-code生成 → 导出分析链路和保存重开。.NET 8 另测 brim、填充、连续路径及 UV 开放路径。随后配套输入构建在 .NET 8 中验证了全部 22 个输入的生成、设置、实例身份与归档回读。

后续智能切片与输入对齐修正通过独立 .NET Framework 进程的真实 GH 属性检查，覆盖 26 个组件元数据、3 个位置下全部 14 个输入对齐、首滑块偏移恢复及重复校正。最新工具栏分栏更新通过独立进程回读检查，尚未完成 Rhino 实时工具栏视觉验收；最新构建尚未重新完成双运行时完整求解、保存重开及安装后的重启验收。此前实测不等于最新二进制已完成同等验收。

1.0.5 尚未执行网络上传、实机打印及操作系统重启后的安装验收。上述验证对应各次构建的环境与测试输入，不代表所有设备和画布组合均已验证。

本仓库提供双语说明与图标，安装附件通过 [GitHub Releases](https://github.com/LIUWEILIN30/NEWPATH/releases) 分发。采用 [MIT License](LICENSE)。GHA 内嵌运行所需的 Python 算法，不提供源码保密保证。
