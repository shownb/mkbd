# 开发人员指南(CN)
## 引言
这是为开发Meishi Keyboard的用户提供的指南。
我们将以教程格式说明创建“名片”键盘（例如“ mkbd / pcb”）的过程。

## PCB 設計
### 开始一个新项目

启动KiCad并从“文件”>“新建”>“工程”中创建一个新项目。

![new_project](./images/new_project/new_project.png)

确定项目名称，然后单击[保存]。 

![save_new_project](./images/new_project/save_new_project.png)

界面如下

![global_menu](./images/new_project/global_menu.png)

### 电路设计

首先，设计键盘电路。
从全局菜单中，打开原理图布局编辑器。

![open_eeschema](./images/eeschema/open_eeschema.png)


这是“原理图布局编辑器”（以下称为Eeschema）的工作屏幕。

![start_eeschema](./images/eeschema/start_eeschema.png)

现在，如果您想系统地学习，则应打开“帮助”>“ KiCad入门”，看看是否只有设计所需的最少说明。
推荐这样做是因为您可以详细了解如何使用KiCad。

![kotohajime](./images/eeschema/kotohajime.png)

在开始设计之前，请先获取键盘所需的零件（库）。
从GitHub获取所需的库，如下所示。

```
$ git clone https://github.com/foostan/kbd
```

或访问https://github.com/foostan/kbd进行下载。

接下来，从“设置>管理符号库...”中加载获得的库。

![manage_lib](./images/eeschema/manage_lib.png)

从[浏览库...]打开 `kbd/library/kbd.lib`

![open_lib](./images/eeschema/open_lib.png)

现在您可以设计电路了。
在Eeschema工作屏幕中按快捷方式“ a”以打开符号选择窗口。
在搜索表单中输入“ promicro_r”，选择“ ProMicro_r”，然后单击“确定”。

![add_promicro_r](./images/eeschema/add_promicro_r.png)

适当安排。

![set_promicro_r](./images/eeschema/set_promicro_r.png)

如果您手边有一个真正的“ Pro Micro”，让我们对其进行比较。
您会看到零件未安装在表面上。
在电路设计中，没有必要将形状和方向与真实物体相匹配，但是对于初学者而言，如果可以想象真实物体，则更容易理解，
如果使用外部库，则最好选择一个易于查看的库。

![promicro](./images/eeschema/promicro.png)

接下来，我们将安排键轴。
这次我们将设计具有6个按键的键盘。

在Eeschema工作屏幕中按快捷方式“ a”以打开符号选择窗口。

在搜索表单中输入“ sw_push”，选择“ SW_PUSH”，然后单击[确定]。

![add_sw_push](./images/eeschema/add_sw_push.png)

安排它们，使它们易于理解。
要移动符号，请将光标放在符号上，然后按快捷方式“ m”将其移动到所需位置。

![set_sw_push](./images/eeschema/set_sw_push.png)

接下来，将键轴连接到ProMicro。
按下快捷方式“ w”，并参考下图进行连接。
如果要使接线更整洁，还有一种使用“标签”的方法，因此请搜索“KiCad入门”等方法。

![wiring_sw_push](./images/eeschema/wiring_sw_push.png)

然后将键轴的另一脚放到GND。
在Eeschema工作屏幕中按快捷方式“ a”以打开符号选择窗口。
在搜索表单中输入“ gnd”，选择“ GND”，然后单击“确定”。

![add_gnd](./images/eeschema/add_gnd.png)

参考下图添加GND。
如果您对外观感兴趣，请尝试调整快捷方式“ g”或快捷方式“ r”等。（请检查每个快捷方式的行为）。

![set_gnd](./images/eeschema/set_gnd.png)

接下来，我将安装一个rst开关。
在Eeschema工作屏幕中按快捷方式“ a”以打开符号选择窗口。
在搜索表单中输入“ sw_push”，选择“ SW_Push”，然后单击[确定]。
它与键开关相同是没有问题的，但是我对其进行了更改，因为如果看起来不同，则更容易理解。

![add_sw_push_reset](./images/eeschema/add_sw_push_reset.png)

接线和GND的安装方法与键轴相同。

![set_sw_push_reset](./images/eeschema/set_sw_push_reset.png)

将VCC和GND连接到ProMicro_r。
在Eeschema工作屏幕中按快捷方式“ a”以打开符号选择窗口。
在搜索表单中输入“ vcc”，选择“ VCC”，然后单击“确定”。

![set_vcc_and_gnd](./images/eeschema/set_vcc_and_gnd.png)

现在，我们已经连接了所有必需的部件，我们将在ProMicro_r上未使用的端口上添加“未连接的标志”。


![set_quit_flag](./images/eeschema/set_quit_flag.png)


最后，将PWR_FLAG连接到VCC和GND。
在Eeschema工作屏幕中按快捷方式“ a”以打开符号选择窗口。
在搜索表单中输入“ pwr_flag”，选择“ PWR_FLAG”，然后单击“确定”。

![set_pwr_flag](./images/eeschema/set_pwr_flag.png)

接下来，编号已安装的符号。
从上面的菜单中选择“注释原理图符号”

![annotation](./images/eeschema/annotation.png)

[Annotation]保留原样。

![set_annotation](./images/eeschema/set_annotation.png)

接下来，检查设计的电路是否有缺陷。
从上面的菜单中选择“检查电气规则”

![erc](./images/eeschema/erc.png)

单击执行。 如果“消息”中没有警告显示并且仅显示“结束”，则可以。

![run_erc](./images/eeschema/run_erc.png)

接下来，将符号连接到实际的电路板组件（封装）。
从上面的菜单中，选择“分布PCB封装到原理符号图”

![run_erc](./images/eeschema/relate_to_footprint.png)

选择编辑封装库表，

![run_erc](./images/eeschema/footprint_lib.png)

从[浏览库...]中选择`kbd / kbd.pretty`，然后单击[确定]。

![run_erc](./images/eeschema/select_kbd_pretty.png)


现在可以使用键轴的封装了。

![start_relation](./images/eeschema/start_relation.png)

按以下方式从读取的kbd中进行分配。
注意，在此示例中，SW1是复位开关。
如果将注释更改为“ RSW”等，可能会更容易理解，从而可以更轻松地事先理解。
（您可以通过将光标置于符号上并使用快捷方式“ e”来更改注释）。

![finish_relate](./images/eeschema/finish_relate.png)

另外，如果要检查封装，可以从右框架中选择它，然后单击“查看选定的封装”。

![show_footprint](./images/eeschema/show_footprint.png)
![show_selected_footprint](./images/eeschema/show_selected_footprint.png)


最后，从上面的菜单中选择“ 生成网表”，
![output_netlist](./images/eeschema/output_netlist.png)

从[生成网表]中生成称为网表的电路信息作为文件输出。
![output_netlist_to_file](./images/eeschema/output_netlist_to_file.png)

电路设计现已完成。


### PCBの作成

グローバルメニューまで戻り、「基板レイアウト エディター」を選択します。
![open_pcbnew](./images/pcbnew/open_pcbnew.png)

これが基板レイアウト エディター(以降 Pcbnew) の作業画面です。
![pcbnew](./images/pcbnew/pcbnew.png)

まずはここに先程作成した回路データを読み込みます。
上記メニューの「ネットリストの読み込み」を選択し
![open_netlist](./images/pcbnew/open_netlist.png)

[現在のネットリストを読み込む]を選択します。
![load_netlist](./images/pcbnew/load_netlist.png)

下記のように各フットプリントが読み込まれていればOKです。
![loaded_netlist](./images/pcbnew/loaded_netlist.png)

まずは名刺の枠を作っていきます。
ちなみに名刺のサイズは 91mm x 55mm です。

上記メニューから作業するレイヤーを 「Edge.Cuts」 に変更し、
![select_edge_cut](./images/pcbnew/select_edge_cut.png)

右メニューから「図形ラインを追加」を選択します。
![add_line](./images/pcbnew/add_line.png)

まずは大まかに四角形を作ります。
![draw_lines](./images/pcbnew/draw_lines.png)


次に線にカーソルを合わせてショートカット `e` で「配線セグメントのプロパティ」を表示します。
この始点 X、始点 Y、終点 X、終点 Yで線の位置を決定していきます。``
![edit_xy](./images/pcbnew/edit_xy.png)

4本線があると思うのでそれぞれ、

1本目
- 始点 X: 0
- 始点 Y: 0
- 終点 X: 91
- 終点 Y: 0

2本目
- 始点 X: 91
- 始点 Y: 0
- 終点 X: 91
- 終点 Y: 55

3本目
- 始点 X: 91
- 始点 Y: 55
- 終点 X: 0
- 終点 Y: 55

4本目
- 始点 X: 0
- 始点 Y: 55
- 終点 X: 0
- 終点 Y: 0

を指定します。

![edge_cut](./images/pcbnew/edge_cut.png)

これだと角が尖っていて痛いので丸めます。

まずは作業がしやすいように、グリッドの設定をします。
0.1mm でいいと思います。
![select_0.1mm](./images/pcbnew/select_0.1mm.png)

「次に円弧を追加」を選択して角を丸めます。

![add_enko](./images/pcbnew/add_enko.png)

![set_enko](./images/pcbnew/set_enko.png)

次にキースイッチのフットプリントを並べていきます。
キースイッチのピッチはCherryMX系の場合は一般的には19.05mmです。

まずはカスタムのグリッド設定を行うため、「表示  > グリッド設定」を選び、サイズ X およびサイズ Y を 19.05 にします。

![grid_setting](./images/pcbnew/grid_setting.png)

その後上記メニューのグリッド設定を「カスタム ユーザー グリッド」にすると、19.05mm 単位のグリッドが表示されます。
あとはこのグリッドに沿ってキースイッチを並べます。
なお、フットプリントの移動はフットプリントにカーソルを合わせて、ショートカットキー `m` でできます。

![set_key_sw](./images/pcbnew/set_key_sw.png)


次にグリッド設定を0.1mm等に戻して、下記のようにProMicroとリセットスイッチを配置します。
基本的には好みで構わないのですが、ProMicroについてはUSB側が外形に重なるように配置します
(もっと内側に設置してしまうと、USBケーブルが干渉してさせなくなります)。

![set_promicro_and_reset](./images/pcbnew/set_promicro_and_reset.png)

次に配線をしていきます。ラッツネスト(ホールやパッドをつなぐ細長い白い線)が、配線すべき箇所を表しています。
きれいに配線するにはこのラッツネストを参考にして、どう配線していくか計画をたてるとうまくいくと思います。

配線はまず、上記メニューから 「F.Cu」レイヤに切り替えて 
![select_fcu](./images/pcbnew/select_fcu.png)

右メニューの「配線」から行います。

![line](./images/pcbnew/line.png)


なお GND は別途処理をするのでそれ以外の配線を行います。
配線にカーソルを合わせて、ショートカットキー `d` や、
フットプリントにカーソルを合わせて、ショートカットキー `r` などの操作を行い、
なるべくシンプルな配線になるように心がけます。

配線の例はこちらです。

![finished_line](./images/pcbnew/finished_line.png)

次に GND の処理をします。右メニューから「塗りつぶしゾーンを追加」を選択し、外形を覆うように配置します。

![beta](./images/pcbnew/beta.png)

設定は以下のとおりです。レイヤーを 「F.Cu」にし、ネットを「GND」にしてあとはデフォルトのままです。

![set_zone](./images/pcbnew/set_zone.png)

正しくゾーンが設置できると以下のようになります。
未接続だった GND が全てつながっているはずです。

![finished_zone](./images/pcbnew/finished_zone.png)

今回の場合ゾーンは F.Cu レイヤだけで動作的には問題ありませんが、
裏表の仕上がりに差が出てしまうので B.Cu レイヤにもゾーンを配置します。

![finished_line_and_zone](./images/pcbnew/finished_line_and_zone.png)

次に「名刺」として機能するようにシルクで名前を入れていきます。
好みのドローソフトやペイントソフトを使用して名刺画像を作成します。

今回は PhotoShop を使います。
幅 91mm x 高さ 55mm で 300 ppi で新規作成します。 

![new_meishi](./images/pcbnew/new_meishi.png)

今回作成したものはこちらです。
背景を黒にして2色で作成します。

![mkbd](./images/pcbnew/mkbd.png)

一旦グローバルメニューにもどり「インポート ビットマップ」を選択します。

![import_bitmap](./images/pcbnew/import_bitmap.png)

ここで先ほど作成した画像を読み込みます。
設定はデフォルトのままで大丈夫だと思いますが、一応サイズと解像度は確認しておきましょう。

![import_meishi](./images/pcbnew/import_meishi.png)


[エクスポート] で Pcbnew で読み込めるシルクデータとして保存します。
なお注意としてはかならず「*.pretty」というフォルダを新規に作成してその中に保存してください。

今回の場合は「mkbd.pretty/meishi.kicad_mod」として保存しています。

![export](./images/pcbnew/export.png)

Pcbnewの画面に戻り、「設定 > フットプリント ライブラリーを管理」を選択します。
[ライブラリーを参照] から先程作成した「mkbd.pretty」を選択します。

![load_lib](./images/pcbnew/load_lib.png)

これで作成した画像がシルクとして使えるようになりました。
「配置 > フットプリント」から配置してみます。
下記のように選択できるようになっているはずです。

![set_meishi_footprint](./images/pcbnew/set_meishi_footprint.png)

あとは好みに配置して完成です。
今回は部品を実装しない裏側に設定しました。
なおフットプリントを裏返す場合は、フットプリントにカーソルを合わせてショートカット `e` から編集ウインドウを開き、
「基板側::表側」となっているのを「裏側」に変更します。

![finished](./images/pcbnew/finished.png)

以上で完成です。
最後にミスがないかチェックします。

上記メニューの「デザインルール チェックを実行」から

![drc](./images/pcbnew/drc.png)

[DRCの開始] を押してチェックします。問題や未配線がなければOKです。

![run_drc](./images/pcbnew/run_drc.png)

完成したPCBを「表示 > 3Dビューアー」から確認してみます。

![3d](./images/pcbnew/3d.png)

![3d_back](./images/pcbnew/3d_back.png)

### PCBの発注

完成したPCBのデータを業者へ発注する過程を説明します。

「ファイル > プロット」から製造用ファイルを生成します。

「含まれるレイヤー」にて、

- F.Cu
- B.Cu
- F.SilkS
- B.SilkS
- F.Mask
- B.Mask
- Edge.Cuts

を選択します。
あとはデフォルトのままです。
出力ディレクトリーをしていして [製造ファイル出力] をします。

![plot](./images/order/plot.png)

また、ドリルデータも必要なので、[ドリル ファイルを生成...] から

![drill](./images/order/drill.png)

[ドリル ファイルを生成] をします。
また生成した製造ファイル(ガーバーデータ)は zip 等で圧縮しておきます。


今回発注には Seeed の FusionPCB を利用します。
https://www.fusionpcb.jp/prototype-pcb-sale.html

こちらではオプションの制約はありますが、100mm x 100mm 以下のPCBの発注を $7.9 で行えるプランが有り、
またOCSを選択しても $12.90 と格安です(少し前までは更に安くて$10を切っていた)。

なお同じ構成で Elecrow に頼むと $18.13 でした。

![fusionpcb](./images/order/fusionpcb.png)

あとは [カートに追加] し、注文すれば完了です。
