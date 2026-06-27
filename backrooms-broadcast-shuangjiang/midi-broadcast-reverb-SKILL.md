---
name: midi-broadcast-reverb
description: 当用户要把 MIDI / 简谱旋律做成 Backrooms 式、空城广播、机器喇叭、巨大混响、远处公共广播音、旋律必须严格来自 MIDI 的音频时使用。尤其适用于用户抱怨 text_to_music 没按 MIDI 旋律生成、要求“不要让 AI 猜旋律”、要保留《让我们荡起双桨》或其他 MIDI 主旋律并转换成机器广播音 / 空旷城市混响风格。这个 skill 会指导 Claude 先解析 MIDI，再确定性合成旋律，最后加广播音色与大混响，而不是用纯文本音乐生成模型自由作曲。
---

# MIDI Broadcast Reverb 音频生成

这个 skill 用于把 MIDI 旋律确定性转换成“机器广播音 + 巨大空城混响”的音频。核心原则是：**旋律必须来自 MIDI 音符，不要让 text-to-music 模型猜旋律**。

## 何时使用

用户出现以下需求时使用：

- “这个 MIDI 旋律要变成 Backrooms 风格”
- “像空旷城市里喇叭播放的机器音”
- “只有主旋律和混响”
- “混响很大，强度随歌曲变化”
- “不要生成崩，旋律必须按 MIDI”
- “text_to_music 没按旋律，帮我确定性生成”
- “把简谱 / MIDI 做成机器广播音”

## 不要做的事

- 不要直接调用 `text_to_music` 让模型自由生成旋律。
- 不要只把简谱写进 prompt 后期待模型准确复现。
- 不要把旋律做成普通钢琴、八音盒、流行编曲、合唱或完整乐队。
- 不要加鼓组、贝斯线、复杂和弦伴奏，除非用户明确要求。

原因：纯文本音乐模型通常不能严格执行 MIDI / 简谱旋律；如果旋律必须准确，应采用 MIDI 解析 + 程序化合成。

## 标准工作流

1. 确认输入 MIDI 文件。
   - 有些文件可能后缀写错，例如 `.mid.ogg`，但文件头是 `MThd`。
   - 用 `file`、`ffprobe` 或读取前 4 字节确认。

2. 解析 MIDI。
   - 读取 tempo、ticks per beat、note on/off。
   - 找出主旋律通道。
   - 如果无法自动判断，优先选择：
     - 非鼓通道；
     - 音域较高；
     - 音符数量适中；
     - 呈现完整主题旋律的通道。

3. 确定性合成主旋律。
   - 使用 monophonic synthetic lead。
   - 音色应像老式公共广播喇叭 / 机器播音音符。
   - 使用窄频、中频、轻微失谐、轻微 vibrato。
   - 保留 MIDI 音高与相对时值；可以整体放慢或拉长 release，但不要改变旋律顺序。

4. 添加空间处理。
   - 大混响是核心。
   - 让旋律像在空旷城市、混凝土广场、长走廊、废弃楼群中回荡。
   - 混响强度应随段落变化：开始中等，长音处变宽，后段再次膨胀，结尾留长尾音。

5. 添加极少量背景纹理。
   - speaker noise
   - tape hiss
   - electrical hum
   - faint room tone
   - 不要使用明显鼓点、贝斯线、和弦伴奏。

6. 导出音频。
   - 推荐输出 `.ogg`，中间可保留 `.wav`。
   - 输出后汇报：路径、时长、主旋律通道、前若干 MIDI 音符，证明旋律来源。

## 可复用 text-only prompt

如果用户仍要求 text-only prompt，可提供以下提示词，但要说明：**它不能保证严格复现 MIDI 旋律**。

```text
Create an instrumental reinterpretation of the given MIDI melody using a Backrooms-like loudspeaker broadcast style. The melody must be clearly recognizable and placed in the foreground. Use the exact melodic contour of the MIDI as the main lead, but make it sound as if a machine is playing the notes through a distant public-address speaker in a vast empty city.

Core sound: a single mechanical broadcast tone plays the melody, like an old municipal loudspeaker or automated announcement system. The lead should be monophonic, synthetic, slightly detuned, narrow-band, mid-frequency, and emotionless, but the note lengths should be stretched, lingering and melancholic. Avoid piano, avoid music box, avoid human voice. The lead should feel like a machine playing notes, not a singer.

Atmosphere: huge reverb is essential. The melody should echo through empty streets, concrete plazas, abandoned buildings and long urban corridors. The reverb tail is very large, wet, and spacious. The reverb intensity changes with the song: at the beginning it is distant and moderate, then it grows wider and more overwhelming during longer notes, then pulls back slightly before swelling again near the end. The reverb bed itself should feel alive and moving.

Arrangement: only main melody plus reverb ambience. Very minimal background. No drums. No bassline. No chordal accompaniment. No full band. Use only faint room tone, speaker noise, tape hiss, subtle electrical hum, and the huge evolving reverb field. The melody should remain identifiable despite the space.

Mood: nostalgic but uncanny, lonely, empty-city broadcast, liminal space, melancholic, slow, prolonged, suspended, mechanical, spacious, haunting but not aggressive.

Structure: short empty-air intro with distant speaker noise, then the full recognizable main melody played slowly by the mechanical broadcast tone. Let long notes hang and bloom into reverb. End with the melody disappearing into a long city-scale echo tail.
```

### Style 字段

```text
instrumental, mechanical loudspeaker melody, empty city ambience, huge evolving reverb, liminal space, Backrooms-inspired, monophonic synthetic lead, old public-address speaker, slow melancholic melody, sparse arrangement, no drums, no vocals, distant broadcast, tape hiss, electrical hum
```

### Negative prompt

```text
vocals, lyrics, singing, choir, cheerful children song, bright pop, dance beat, EDM drop, rock drums, acoustic guitar, piano ballad, orchestral march, brass band, complex harmony, fast rhythm, heavy percussion, upbeat arrangement, happy nursery rhyme, lush strings, full band, music box, clean piano
```

## 推荐脚本

本 skill 附带脚本：

```text
scripts/midi_broadcast_reverb.py
```

用法：

```bash
python3 /workspace/skills/midi-broadcast-reverb/scripts/midi_broadcast_reverb.py \
  --input /workspace/assets/audio/让我们荡起双桨.mid.ogg \
  --output /workspace/assets/audio/city_loudspeaker_shuangjiang.ogg
```

可选参数：

```bash
--melody-channel 2     # 指定 MIDI 主旋律通道，默认自动选择
--slow 1.55            # 整体放慢倍率
--reverb 0.72          # 混响湿声强度
--title NAME           # 仅用于日志标记
```

## 输出汇报模板

完成后用这个格式回复用户：

```text
已按 MIDI 确定性合成，不再使用 text_to_music 猜旋律。

输出文件：<path>
时长：<duration>
MIDI 主旋律通道：<channel>
使用音符数：<count>
开头旋律音符：<note names>

这版旋律来自 MIDI note on/off 事件；风格处理为机器广播音 + 空城大混响。
```

## 经验参数

上次成功案例使用的有效方向：

- 主旋律通道：`2`
- 开头 MIDI 音符：`B3 D4 E4 F#4 A4 F#4 D4 E4 ...`
- 音色：窄频机器广播合成音，轻微失谐和 vibrato
- 空间：8 秒级大混响，早期反射 + 长尾噪声卷积
- 背景：speaker noise + 50Hz/100Hz electrical hum
- 输出：`/workspace/assets/audio/city_loudspeaker_shuangjiang.ogg`

记住：用户要的是“旋律正确 + 风格正确”。旋律正确优先于生成模型的自由发挥。
