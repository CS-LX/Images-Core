# Backrooms Broadcast Shuangjiang

本目录保存“让我们荡起双桨 MIDI 旋律 + Backrooms/空城机器广播混响风格”的确定性合成结果与复用提示词。

## 文件

- `city_loudspeaker_shuangjiang.wav`：确定性合成的 WAV 版本。
- `city_loudspeaker_shuangjiang.ogg`：确定性合成的 OGG Vorbis 版本。
- `backrooms-music-prompt.md`：风格分析、text-only prompt、自评与生成记录。
- `midi-broadcast-reverb-SKILL.md`：后续复用的 skill 说明。

## 关键方法

不要使用 text-to-music 模型自由生成旋律。该方法会导致旋律不按 MIDI。

正确流程：

1. 读取 MIDI 文件，确认文件头为 `MThd`。
2. 解析 MIDI note on/off 事件。
3. 选择主旋律通道。
4. 按 MIDI 音高和时值确定性合成机器广播音。
5. 添加空旷城市尺度的大混响、speaker noise、tape hiss、电流 hum。
6. 导出 WAV / OGG。

## 本次成功参数

- 主旋律通道：`2`
- 旋律音符数：`63`
- BPM：`88`
- 放慢倍率：`1.55`
- 混响强度：`0.72`
- 开头旋律：`B3 D4 E4 F#4 A4 F#4 D4 E4 B3 D4 E4 F#4 A4 A4 B4 E4`
