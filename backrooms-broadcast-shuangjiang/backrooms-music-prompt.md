# Backrooms 音频风格提示词与旋律简谱生成可行性自评

## 1. 音频来源

分析对象：`/workspace/assets/audio/Backrooms.ogg`

该文件扩展名为 `.ogg`，但实际音频编码检测为 MP3。分析结果仅基于本地信号特征与可推断听感，不等同于人工完整扒谱。

## 2. Backrooms.ogg 风格摘要

该音频更接近低频 drone / 暗环境声景，而不是有清晰主旋律的传统歌曲。核心特征如下：

- 低频和低中频主导，主要能量集中在 20-500 Hz。
- 高频含量很少，整体听感闷、暗、像被墙体或空间过滤。
- 没有稳定、可唱的主旋律线。
- 音高中心大致围绕 F / C / G 附近漂移。
- 调性模糊，不形成明确大小调和弦进行。
- 靠响度、低频厚度、泛音粗糙度和频谱重心变化推动情绪。
- 后半段频谱重心上升，带来更明显的不安和逼近感。
- 结尾快速衰减至接近静音。

## 3. 可验证音频特征

基础信息：

```text
时长：约 155.1 秒
采样率：44100 Hz
声道：双声道
实际编码：MP3
mean volume：约 -12.2 dB
Integrated loudness：约 -10.3 LUFS
Loudness range：约 11 LU
约 149.9 秒后进入明显静音 / 衰减
```

频段能量分布：

```text
20-60Hz      23.3%
60-120Hz     19.0%
120-250Hz    42.2%
250-500Hz    10.9%
500-1000Hz    3.2%
1000-2000Hz   1.0%
2000Hz 以上   极少
```

常见主导频率：

```text
43 Hz
86 Hz
172 Hz
258 Hz
355 Hz
441 Hz
527 Hz
699 Hz
786 Hz
882 Hz
1055 Hz
```

粗略音高中心：

```text
F / C / G / A / D 附近
```

粗略调性感候选：

```text
F major
D minor
F minor
C major
```

注意：这些候选仅表示频谱和 chroma 能量分布接近，并不表示该音频是传统意义上的 F 大调或 D 小调歌曲。

## 4. 中文 AI 音乐提示词

```text
一首低频主导的暗环境音乐，整体没有清晰主旋律，围绕 F / C / G 附近的低音中心缓慢漂移。音乐以持续的低频 drone、低中频共振、闷响的房间噪声和模糊泛音构成，几乎没有鼓点和明确节奏。整体频谱偏暗，高频被明显削弱，像隔着墙听见地下空间或空旷走廊里的机器低鸣。

开头用压低的低频铺底，缓慢建立空间压迫感；中段低频墙逐渐变厚，音量轻微起伏但不形成传统高潮；后半段加入轻微失真、噪声纹理和更高的泛音，让频谱重心慢慢上移，制造逼近和不安；最后快速衰减到近乎静音。

氛围关键词：幽闭、空旷、无人、低频压迫、调性模糊、失真泛音、旧录音质感、低通滤波、缓慢推进、liminal space、backrooms、dark ambient、drone。
```

## 5. 英文 AI 音乐提示词

```text
Create a dark ambient drone piece with no clear singable melody. The track should be dominated by sub-bass and low-mid resonance, centered loosely around F, C and G, but with ambiguous tonality and no traditional chord progression. Use a thick sustained low-frequency drone, muffled room tone, distant mechanical hum, low-pass filtered texture, subtle tape noise, and blurred harmonic overtones.

The energy should evolve very slowly: start with a dark low drone and enclosed room resonance, then gradually thicken the low-frequency wall. Avoid drums, avoid obvious rhythm, avoid bright leads, avoid melodic hooks. In the second half, introduce slight distortion, unstable overtones, faint noise layers and a slowly rising spectral brightness, creating the feeling of an empty underground corridor becoming more oppressive. End with a sudden fade into near silence.

Mood: liminal space, backrooms, claustrophobic, empty corridors, abandoned building, oppressive low frequencies, detuned resonance, eerie nostalgia, dark ambient, drone, muffled, low-pass filtered, cinematic horror ambience.
```

## 6. 用于旋律风格迁移的短提示词

当需要把外部旋律套入 Backrooms.ogg 的风格时，推荐使用以下提示词：

```text
Arrange the given melody as a dark ambient Backrooms-style piece. Keep the melody very slow, distant and barely exposed, played by a muffled thin synth or detuned music box buried deep in reverb. Under it, build a heavy low-frequency drone centered around F/C/G, with no clear chord progression. Use low-pass filtering, room hum, tape noise, subtle distortion, blurred overtones, and long decays. Avoid drums, avoid bright pop arrangement, avoid strong rhythm. The melody should feel like it is coming from another room through thick walls in an empty corridor.
```

负面提示词：

```text
bright pop, cheerful, upbeat, clear vocals, dance beat, EDM drop, rock drums, acoustic guitar strumming, clean piano ballad, orchestral fanfare, happy children song, strong percussion, catchy hook, high-frequency sparkle
```

## 7. 通过旋律简谱创建音频的可行性自评

### 7.1 目标定义

目标是输入一段旋律简谱，例如：

```text
3 3 5 5 6 5 | 3 2 1 2 | 3 5 6 -
```

然后生成一段音频，使其同时满足：

1. 主旋律可辨认，严格或基本遵循简谱。
2. 编曲风格接近 Backrooms.ogg。
3. 整体听感不是普通儿歌、流行歌或钢琴翻奏，而是暗环境 / drone / 低频空间声景。

### 7.2 仅使用文本生音乐模型的可行性

结论：可行性低到中等，不适合要求严格旋律还原的任务。

原因：

- 文本生音乐模型通常不擅长精确执行数字简谱。
- 即使在 prompt 中写入完整简谱，模型也可能只把它当作风格或情绪提示，而不是严格旋律约束。
- 对于 Backrooms.ogg 这种“旋律缺席”的风格，模型可能进一步弱化旋律，导致生成结果没有可辨认主题。
- 如果同时要求“暗环境风格”和“清晰旋律”，两者存在张力：暗环境强调模糊、持续、无节奏；简谱旋律强调清晰音高、节奏和句法。

预期风险：

```text
高风险：旋律不可辨认
高风险：模型自由改写旋律
中风险：风格变成普通恐怖 BGM，而不是低频 drone
中风险：为了突出旋律，模型生成成 music box / piano cover，失去 Backrooms.ogg 的低频压迫感
```

### 7.3 使用 MIDI / guide melody 的可行性

结论：可行性高，推荐。

更可靠流程：

1. 将简谱转换为明确的 MIDI 音高和时值。
2. 单独导出一条 guide melody。
3. 使用该旋律作为强约束，再进行风格化编曲。
4. 将旋律音色处理为：低通、远距离、大混响、轻微失谐、低音量嵌入空间。
5. 在底层增加 Backrooms.ogg 风格的低频 drone、机械 hum、房间噪声、泛音摩擦和缓慢动态变化。

优势：

- 主旋律可控。
- 节奏可控。
- 可单独调整旋律与氛围层的比例。
- 可以避免文本模型忽略简谱。

### 7.4 推荐制作方案

最推荐的制作方式不是“一步文本生成”，而是分层制作：

```text
旋律层：由简谱转换 MIDI，使用 muffled sine / detuned music box / filtered synth lead
氛围层：低频 F/C/G drone，低通滤波，房间 hum，机械噪声
纹理层：tape hiss、轻微失真、反向 swell、模糊泛音
空间层：长混响、窄动态、远距离感、墙后感
混音层：旋律音量低，埋在空间中，但音高轮廓仍可辨认
```

### 7.5 旋律与 Backrooms 风格的冲突处理

如果旋律必须清晰：

- 旋律音色不要太亮。
- 使用薄的 sine lead 或失谐 music box。
- 截掉高频，只保留中频轮廓。
- 让旋律像从墙后传来，而不是站在前景。
- 节奏可以放慢，但不要破坏原本音符关系。

如果氛围必须极像 Backrooms.ogg：

- 旋律只能非常弱、远、断续。
- 允许部分音符被混响和 drone 掩盖。
- 不要使用完整流行编曲。
- 不要加入鼓组或明显和弦伴奏。

### 7.6 自评结论

```text
仅靠文本 prompt：不可靠，不建议用于严格旋律复现。
文本 prompt + 明确简谱：可尝试，但旋律保持度不可控。
MIDI / guide melody + prompt：推荐，旋律保持度和风格迁移都更可控。
分层制作 / DAW / 程序化音频：最可靠。
```

如果后续要继续制作“Backrooms 风格 + 指定简谱旋律”的音频，建议先把简谱转换为 MIDI 或至少转换为明确的音名、时值、BPM、小节结构，再进入音乐生成或编曲阶段。

## 8. 根据用户补充听感修订的 text-only prompt

用户补充后的目标不再强调低频 drone，而是强调：主旋律、机器广播音、长音缠绵、巨大空城混响，以及混响强度随歌曲变化。

### 8.1 英文 text-only prompt

```text
Create an instrumental reinterpretation of the melody of the Chinese song "让我们荡起双桨" using a Backrooms-like loudspeaker broadcast style. The melody must be clearly recognizable and placed in the foreground. Use the exact melodic contour of the song as the main lead, but make it sound as if a machine is playing the notes through a distant public-address speaker in a vast empty city.

Core sound: a single mechanical broadcast tone plays the melody, like an old municipal loudspeaker or automated announcement system. The lead should be monophonic, synthetic, slightly detuned, narrow-band, mid-frequency, and emotionless, but the note lengths should be stretched, lingering and melancholic. Avoid piano, avoid music box, avoid human voice. The lead should feel like a machine playing notes, not a singer.

Atmosphere: huge reverb is essential. The melody should echo through empty streets, concrete plazas, abandoned buildings and long urban corridors. The reverb tail is very large, wet, and spacious. The reverb intensity changes with the song: at the beginning it is distant and moderate, then it grows wider and more overwhelming during longer notes, then pulls back slightly before swelling again near the end. The reverb bed itself should feel alive and moving.

Arrangement: only main melody plus reverb ambience. Very minimal background. No drums. No bassline. No chordal accompaniment. No full band. Use only faint room tone, speaker noise, tape hiss, subtle electrical hum, and the huge evolving reverb field. The melody should remain identifiable despite the space.

Mood: nostalgic but uncanny, lonely, empty-city broadcast, liminal space, melancholic, slow, prolonged, suspended, mechanical, spacious, haunting but not aggressive.

Structure: short empty-air intro with distant speaker noise, then the full recognizable main melody of "让我们荡起双桨" played slowly by the mechanical broadcast tone. Let long notes hang and bloom into reverb. End with the melody disappearing into a long city-scale echo tail.
```

### 8.2 风格字段

```text
instrumental, mechanical loudspeaker melody, empty city ambience, huge evolving reverb, liminal space, Backrooms-inspired, monophonic synthetic lead, old public-address speaker, slow melancholic melody, sparse arrangement, no drums, no vocals, distant broadcast, tape hiss, electrical hum
```

### 8.3 负面提示词

```text
vocals, lyrics, singing, choir, cheerful children song, bright pop, dance beat, EDM drop, rock drums, acoustic guitar, piano ballad, orchestral march, brass band, complex harmony, fast rhythm, heavy percussion, upbeat arrangement, happy nursery rhyme, lush strings, full band
```

### 8.4 本次生成结果

```text
标题：空城双桨
输出文件：/workspace/assets/audio/music_1782561310011.ogg
时长：约 99.8 秒
生成方式：text-only prompt
```
