# 第五人格 MIDI 演奏器 v1.0

基于 [sabihoshi/GenshinLyreMidiPlayer](https://github.com/sabihoshi/GenshinLyreMidiPlayer) 修改，原作者 [sabihoshi](https://github.com/sabihoshi)，MIT 许可。
原项目：https://github.com/sabihoshi/GenshinLyreMidiPlayer
保留 LICENSE.md 和 THIRD-PARTY-NOTICES.md；本修改版非官方软件。

## 使用

完整解压便携 ZIP，运行 GenshinLyreMidiPlayer.WPF.exe（沿用原管理员权限清单）。
打开设置，开启全局模式，关闭试听模式，点击设置顶部的“启用黑键”开关。
游戏中保持黑键开启。按游戏实际键位核对下面的预设：

| 音区 | 白键 do re mi fa sol la si | 黑键 升do 升re 升fa 升sol 升la |
|---|---|---|
| 低音 C3～B3 | , . / I O P [ | L ; 9 0 ] |
| 中音 C4～B4 | Z X C V B N M | S D G H J |
| 高音 C5～B5 | Q W E R T Y U | 2 3 5 6 7 |

符号使用英文半角，按键之间用空格分隔。每行白键7个、黑键5个，36个键不可重复。
键位及黑键开关可编辑，点击“保存键位”后生效；保存会停止播放并释放按键。
没有点击截图预设前，保留旧版21键配置，黑键默认关闭。
关闭黑键再保存可使用21白键模式。原神模式不受黑键设置影响。

导入 MIDI 并选择音轨，切到游戏后 F9 开始、F8 暂停/继续、F10 停止。
点击界面播放按钮有3秒切换窗口时间。全局模式发送给当前前台窗口。
启用黑键后，C3～B5范围内的36个半音直接映射；超出音域仍需要跳过或调整。
如旧曲目设置了转调，请核对曲目调音设置。扬声器试听不发送键盘输入。
若快捷键占用提示出现，关闭其他播放器实例或占用程序后重启，也可用界面按钮。

## 构建与验证

本机 .NET SDK 8.0.425，Windows x64 自包含发布成功。
运行 Tests/Tests.csproj：59项断言全部通过，含36个真实播放映射、符号键、非法配置和21键兼容性。
测试不会发送真实按键。Windows界面交互、游戏内响应、快捷键和停止释放行为尚未实测。
保留原有 InputSimulator/SQLite/Fody 编译警告，详见 BUILD-STATUS.txt。
源码已直接在用户提供的修改版目录中更新，没有从原仓库覆盖。
重新构建：安装 .NET 8 SDK 后运行 build.bat。
## Apple Music 风格界面

原生 WPF 界面采用红色强调、侧栏导航、曲目卡片与底部播放栏，播放速度位于播放栏右侧。
支持浅色、深色与原有主题设置。面板保留圆角和基础半透明样式；已移除折射、玻璃边缘、高光、面板背景采样与模糊渲染，以降低滚动和调整窗口时的开销。

设置顶部仅保留一个“启用黑键”开关，切换后立即保存。
庄园钢琴封面为本次生成的默认装饰图，不表示 MIDI 自带封面。
最小窗口尺寸为 900×720，默认 1120×850。已删除的设置项目没有恢复。
修改前的界面相关文件位于 ui-backup-before-glass；原生渲染示例位于 ui-preview。
检查命令：dotnet run --project UiChecks/UiChecks.csproj -c Release -- <ui-preview 的绝对路径>。
UiChecks 使用示例曲目和真实音轨模型，离屏检查速度、音轨、进度绑定及深浅色/紧凑布局，不发送游戏按键。
点击播放器封面上的“更换封面”可选择 PNG、JPG、BMP 或 GIF 图片，路径自动保存；右键封面可恢复默认。如果原图片被移动或删除，下次启动显示默认封面。GIF 使用静态首帧。播放列表的红色“添加 MIDI 文件”按钮支持原有多文件选择。
