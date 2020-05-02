# 开发人员指南(CN)
## 引言
这是为开发Meishi Keyboard的用户提供的指南。
我们将以教程格式说明创建"名片"键盘（例如" mkbd / pcb"）的过程。

## PCB 設計
### 开始一个新项目

启动KiCad并从"文件">"新建">"工程"中创建一个新项目。

![new_project](./images/new_project/new_project.png)

确定项目名称，然后单击[保存]。 

![save_new_project](./images/new_project/save_new_project.png)

界面如下

![global_menu](./images/new_project/global_menu.png)

### 电路设计

首先，设计键盘电路。
从全局菜单中，打开原理图布局编辑器。

![open_eeschema](./images/eeschema/open_eeschema.png)


这是"原理图布局编辑器"（以下称为Eeschema）的工作屏幕。

![start_eeschema](./images/eeschema/start_eeschema.png)

现在，如果您想系统地学习，则应打开"帮助">"KiCad入门"，看看是否只有设计所需的最少说明。
推荐这样做是因为您可以详细了解如何使用KiCad。

![kotohajime](./images/eeschema/kotohajime.png)

在开始设计之前，请先获取键盘所需的零件（库）。
从GitHub获取所需的库，如下所示。

```
$ git clone https://github.com/foostan/kbd
```

或访问https://github.com/foostan/kbd进行下载。

接下来，从"首选项>管理符号库..."中加载获得的库。

![manage_lib](./images/eeschema/manage_lib.png)

从[浏览库...]打开 `kbd/library/kbd.lib`

![open_lib](./images/eeschema/open_lib.png)

现在您可以设计电路了。
在Eeschema工作屏幕中按快捷方式"a"以打开符号选择窗口。
在搜索表单中输入"promicro_r"，选择"ProMicro_r"，然后单击"确定"。

![add_promicro_r](./images/eeschema/add_promicro_r.png)

适当排版。

![set_promicro_r](./images/eeschema/set_promicro_r.png)

如果您手边有一个真正的"Pro Micro"，比较一下。
您会看到零件未安装在表面上。
在电路设计中，没有必要将形状和方向与真实物体相匹配，但是对于初学者而言，如果可以看到真实物体，则更容易理解，
如果使用外部库，则最好选择一个易于查看的库。

![promicro](./images/eeschema/promicro.png)

接下来，我们将安排键轴。
这次我们将设计具有6个按键的键盘。

在Eeschema工作屏幕中按快捷方式"a"以打开符号选择窗口。

在搜索表单中输入" sw_push"，选择" SW_PUSH"，然后单击[确定]。

![add_sw_push](./images/eeschema/add_sw_push.png)

把它们排版一下，使它们易于理解。
要移动符号，请将光标放在符号上，然后按快捷方式"m"将其移动到所需位置。

![set_sw_push](./images/eeschema/set_sw_push.png)

接下来，将键轴连接到ProMicro。
按下快捷方式"w"，并参考下图进行连接。
如果要使接线更整洁，还有一种使用"标签"的方法，因此请搜索"KiCad入门"等方法。

![wiring_sw_push](./images/eeschema/wiring_sw_push.png)

然后将键轴的另一脚放到GND。
在Eeschema工作屏幕中按快捷方式" a"以打开符号选择窗口。
在搜索表单中输入"gnd"，选择" GND"，然后单击"确定"。

![add_gnd](./images/eeschema/add_gnd.png)

参考下图添加GND。
如果您对要调整元件方向，请尝试调整快捷方式"g"或快捷方式"r"等。（请检查每个快捷方式的行为）。

![set_gnd](./images/eeschema/set_gnd.png)

接下来，我将安装一个rst开关。
在Eeschema工作屏幕中按快捷方式"a"以打开符号选择窗口。
在搜索表单中输入" sw_push"，选择" SW_Push"，然后单击[确定]。
它与键轴在原理图上图标是一样的，但是我对其进行了更改，因为如果看起来不同，则更容易理解。

![add_sw_push_reset](./images/eeschema/add_sw_push_reset.png)

接线和GND的安装方法与键轴相同。

![set_sw_push_reset](./images/eeschema/set_sw_push_reset.png)

将VCC和GND连接到ProMicro_r。
在Eeschema工作屏幕中按快捷方式"a"以打开符号选择窗口。
在搜索表单中输入"vcc"，选择"VCC"，然后单击"确定"。

![set_vcc_and_gnd](./images/eeschema/set_vcc_and_gnd.png)

现在，我们已经连接了所有必需的部件，我们将在ProMicro_r上未使用的端口上添加"未连接的标志"。


![set_quit_flag](./images/eeschema/set_quit_flag.png)


最后，将PWR_FLAG连接到VCC和GND。
在Eeschema工作屏幕中按快捷方式" a"以打开符号选择窗口。
在搜索表单中输入"pwr_flag"，选择"PWR_FLAG"，然后单击"确定"。

![set_pwr_flag](./images/eeschema/set_pwr_flag.png)

接下来，编号已安装的符号。
从上面的菜单中选择"注释原理图符号"

![annotation](./images/eeschema/annotation.png)

[Annotation]保留原样。

![set_annotation](./images/eeschema/set_annotation.png)

接下来，检查设计的电路是否有缺陷。
从上面的菜单中选择"检查电气规则"

![erc](./images/eeschema/erc.png)

单击执行。 如果"消息"中没有警告显示并且仅显示"结束"，则可以。

![run_erc](./images/eeschema/run_erc.png)

接下来，将符号连接到实际的电路板组件（封装）。
从上面的菜单中，选择"分布PCB封装到原理符号图"

![run_erc](./images/eeschema/relate_to_footprint.png)

选择编辑封装库表，

![run_erc](./images/eeschema/footprint_lib.png)

从[浏览库...]中选择`kbd / kbd.pretty`，然后单击[确定]。

![run_erc](./images/eeschema/select_kbd_pretty.png)


现在可以使用键轴的封装了。

![start_relation](./images/eeschema/start_relation.png)

按以下方式从读取的kbd中进行分配。
注意，在此示例中，SW1是复位开关。
如果将注释更改为"RSW"等，可能会更容易理解，从而可以更轻松地事先理解。
（您可以通过将光标置于符号上并使用快捷方式"e"来更改注释）。

![finish_relate](./images/eeschema/finish_relate.png)

另外，如果要检查封装，可以从右框架中选择它，然后单击"查看选定的封装"。

![show_footprint](./images/eeschema/show_footprint.png)
![show_selected_footprint](./images/eeschema/show_selected_footprint.png)


最后，从上面的菜单中选择" 生成网表"，
![output_netlist](./images/eeschema/output_netlist.png)

从[生成网表]中生成称为网表的电路信息作为文件输出。
![output_netlist_to_file](./images/eeschema/output_netlist_to_file.png)

电路设计现已完成。


### 制作PCB
图示-->单位-->mm

返回到全局菜单，然后选择" Board Layout Editor"。
![open_pcbnew](./images/pcbnew/open_pcbnew.png)

这是PCB布局编辑器（Pcbnew）的工作屏幕。
![pcbnew](./images/pcbnew/pcbnew.png)

首先，加载此处先前创建的电路数据。
从上面的菜单中选择"加载网表"
![open_netlist](./images/pcbnew/open_netlist.png)

选择加载当前网表。
![load_netlist](./images/pcbnew/load_netlist.png)

如下图所示加载的每个封装。
![loaded_netlist](./images/pcbnew/loaded_netlist.png)

首先，我将制作名片框架。
顺便说一下，名片的大小是91mm x 55mm。

从上面的菜单中将要工作的图层更改为"Edge.Cuts"，
![select_edge_cut](./images/pcbnew/select_edge_cut.png)

从右侧菜单中选择"添加图形线条"。
![add_line](./images/pcbnew/add_line.png)

首先，制作一个粗糙的矩形。
![draw_lines](./images/pcbnew/draw_lines.png)


接下来，将光标移至该行，并使用快捷方式"e"显示"接线段属性"。
**译者注：这个很重要，从起点x=0，y=0开始，以后安排键轴就可以有参考了。变换网格就可以方便自己排放各种封装。**  
**译者注：这个很重要，从起点x=0，y=0开始，以后安排键轴就可以有参考了。变换网格就可以方便自己排放各种封装。**  
**译者注：这个很重要，从起点x=0，y=0开始，以后安排键轴就可以有参考了。变换网格就可以方便自己排放各种封装。**  
直线的位置由起点X，起点Y，终点X和终点Y确定。``
![edit_xy](./images/pcbnew/edit_xy.png)

我认为有4条主线，所以

1
- 始点 X: 0
- 始点 Y: 0
- 終点 X: 91
- 終点 Y: 0

2
- 始点 X: 91
- 始点 Y: 0
- 終点 X: 91
- 終点 Y: 55

3
- 始点 X: 91
- 始点 Y: 55
- 終点 X: 0
- 終点 Y: 55

4
- 始点 X: 0
- 始点 Y: 55
- 終点 X: 0
- 終点 Y: 0

确定

![edge_cut](./images/pcbnew/edge_cut.png)

如果是这样，拐角处会很锋利并且很疼，所以把它弄圆。

首先，设置网格，以便您可以轻松地工作。
我认为0.1mm就足够了。
![select_0.1mm](./images/pcbnew/select_0.1mm.png)

选择 "添加圆弧" 以圆角化。

![add_enko](./images/pcbnew/add_enko.png)

![set_enko](./images/pcbnew/set_enko.png)

接下来，布置键轴的占地面积。
对于Cherry MX系列，按键开关的间距通常为19.05 mm。

首先，要设置自定义网格设置，请选择"视图>网格设置"，并将"大小X"和"大小Y"设置为19.05。

![grid_setting](./images/pcbnew/grid_setting.png)

此后，如果将以上菜单中的网格设置设置为"自定义用户网格"，将显示19.05毫米单位的网格。
之后，沿着该网格排列键轴封装。
您可以通过将光标移动到封装并使用快捷键"m"来移动足迹。

![set_key_sw](./images/pcbnew/set_key_sw.png)


接下来，将栅格设置重新设置为0.1mm等，然后如下所示放置ProMicro和复位开关。
最好USB侧与轮廓重叠
（如果将其安装在更深的内部，则USB电缆插不到。）。

![set_promicro_and_reset](./images/pcbnew/set_promicro_and_reset.png)

接下来，我们将进行连线。 飞线（连接孔和垫的长白线）表示要布线的点。

对于布线，首先从上面的菜单切换到" F.Cu"层。
![select_fcu](./images/pcbnew/select_fcu.png)

从右侧菜单中的"接线"执行。

![line](./images/pcbnew/line.png)

由于GND是单独处理的，因此需要其他布线。
将光标移至接线并按快捷键"d"或
将光标移至足迹并执行快捷键"r"等，
尝试使接线尽可能简单。

这是接线示例。

![finished_line](./images/pcbnew/finished_line.png)

接下来，处理GND。 从右侧菜单中选择"添加填充区域"并将其放置，使其覆盖轮廓。

![beta](./images/pcbnew/beta.png)

设置如下。 将层设置为" F.Cu"，将网络设置为" GND"，并保留默认设置。

![set_zone](./images/pcbnew/set_zone.png)

如果可以正确设置区域，则如下所示。
所有未连接的GND都应连接。

![finished_zone](./images/pcbnew/finished_zone.png)

在这种情况下，该区域仅是F.Cu层，操作中没有问题，但是
由于正面和背面的光洁度会有差异，因此也应在B.Cu层中放置区域。

![finished_line_and_zone](./images/pcbnew/finished_line_and_zone.png)

接下来，在丝绸上添加名称，以便将其用作"名片"。
使用您喜欢的绘图软件或绘画软件创建名片图像。

这次我使用PhotoShop。
创建一个宽度为91mm x高度为55mm且高度为300 ppi的新产品。

![new_meishi](./images/pcbnew/new_meishi.png)

这是我这次所做的。 创建具有两种颜色的黑色背景。

![mkbd](./images/pcbnew/mkbd.png)

返回到全局菜单，然后选择“导入位图”。

![import_bitmap](./images/pcbnew/import_bitmap.png)

加载刚刚创建的图像。
默认设置就可以，但是检查一下大小和分辨率。

![import_meishi](./images/pcbnew/import_meishi.png)


另存为水印数据，Pcbnew可以使用[导出]读取该数据。
确保创建一个新文件夹“ * .pretty”并将其保存在该文件夹中。

在这种情况下，它将另存为“ mkbd.pretty / meishi.kicad_mod”。

![export](./images/pcbnew/export.png)

返回到“ Pcbnew”屏幕，然后选择“设置>管理封装库”。
从[浏览库]中选择之前创建的[mkbd.pretty]。

![load_lib](./images/pcbnew/load_lib.png)

以此创建的图像现在可以用作水印。
尝试从“放置>封装”放置它。
您应该可以选择以下内容。

![set_meishi_footprint](./images/pcbnew/set_meishi_footprint.png)

之后，根据需要安排它并完成。
这次，我将其放在没有安装任何部件的背面。
如果您要翻转封装，请将光标移至封装，然后从快捷方式“ e”打开编辑窗口，
将“板侧::正面”更改为“背面”。

![finished](./images/pcbnew/finished.png)

就是这样！
最后检查是否有任何错误。

从上面菜单中的“运行设计规则检查”

![drc](./images/pcbnew/drc.png)

按启动DRC进行检查。 如果没有问题或未连接，则可以。

![run_drc](./images/pcbnew/run_drc.png)

让我们从“ View> 3D Viewer”中检查完成的PCB。

![3d](./images/pcbnew/3d.png)

![3d_back](./images/pcbnew/3d_back.png)

### PCB打板

我将解释从供应商订购完整的PCB数据的过程。

从“文件”>“绘制”生成制造文件。

在“包含的图层”中，选择。

- F.Cu
- B.Cu
- F.SilkS
- B.SilkS
- F.Mask
- B.Mask
- Edge.Cuts

其余为默认值。
使用[输出制造文件]更改输出目录。

![plot](./images/order/plot.png)

您还需要钻取数据，因此选择生成钻孔文件...

![drill](./images/order/drill.png)

选择生成钻取文件。
另外，生成的制造文件（Gerber数据）用zip等压缩。


然后找网站放gerber文件，打印。就这样吧。
