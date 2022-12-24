# 音乐
## 属性
### `music`
类[Music](#NNZvx)的实例，用来控制Pico:ed板载蜂鸣器播放旋律。
## 类
### `class Music(pin, ticks=4, bpm=120)`
可以使用 Music 类通过Pico:ed板载的蜂鸣器播放旋律。

- **pin -** 蜂鸣器引脚
- **ticks -** 许多刻度构成一个节拍。默认值为 4。
- **bpm -** 每分钟节拍次数。默认值为 120。

> `**set_tempo(ticks=4, bpm=120)**`

设置播放的近似速度。

- **ticks -** 许多刻度构成一个节拍。默认值为 4。
- **bpm -** 每分钟节拍次数。默认值为 120。

> `**get_tempo()**`

获取当前速度作为整数元组：（ticks，bpm）。

> `**play(music)**`

播放旋律。

- **music -** 乐谱DSL。

> `**play_async(music)**`

异步播放旋律。

- **music -** 乐谱DSL。

> `**pitch(frequency, duration=-1)**`

以给定的整数频率播放指定毫秒数的音高。

- **frequency -** 指定的频率。
- **duration -** 延时毫秒数。

> `**pitch_async(frequency, duration=-1)**`

以给定的整数频率异步播放指定毫秒数的音高。

- **frequency -** 指定的频率。
- **duration -** 延时毫秒数。

> `**stop()**`

停止音乐播放。事实上，只适用于`**play_async(music)**`**。**

> `**reset()**`

重置为默认状态。
## 乐谱DSL
单个音符如下：
```python
NOTE[octave][:duration]
```
例如，`A1:4` refers to the note “A” in octave 1 that lasts for four ticks (a tick is an arbitrary length of time defined by a tempo setting function - see below). If the note name `R` is used then it is treated as a rest (silence).
Accidentals (flats and sharps) are denoted by the `b` (flat - a lower case b) and `#` (sharp - a hash symbol). For example, `Ab` is A-flat and `C#` is C-sharp.
**注意名称不区分大小写。**
The `octave` and `duration` parameters are states that carry over to subsequent notes until re-specified. The default states are `octave = 4` (containing middle C) and `duration = 4` (a crotchet, given the default tempo settings - see below).
For example, if 4 ticks is a crotchet, the following list is crotchet, quaver, quaver, crotchet based arpeggio:
```python
['c1:4', 'e:2', 'g', 'c2:4']
```
The opening of Beethoven’s 5th Symphony would be encoded thus:
```python
['r4:2', 'g', 'g', 'g', 'eb:8', 'r:2', 'f', 'f', 'f', 'd:8']
```
The definition and scope of an octave conforms to the table listed [on this page about scientific pitch notation](https://en.wikipedia.org/wiki/Scientific_pitch_notation#Table_of_note_frequencies). For example, middle “C” is `'c4'` and concert “A” (440) is `'a4'`. Octaves start on the note “C”.
The library has quite a lot of built-in melodies. Here’s a complete list:

- DADADADUM
- ENTERTAINER
- PRELUDE
- ODE
- NYAN
- RINGTONE
- FUNK
- BLUES
- BIRTHDAY
- WEDDING
- FUNERAL
- PUNCHLINE
- PYTHON
- BADDY
- CHASE
- BA_DING
- WAWAWAWAA
- JUMP_UP
- JUMP_DOWN
- POWER_UP
- POWER_DOWN
## 示例
1.播放内置音乐
```python
from picoed import music

music.play(music.DADADADUM)
```

2.同步播放
```python
from picoed import music

# play Prelude in C.
notes = [
    'c4:1', 'e', 'g', 'c5', 'e5', 'g4', 'c5', 'e5', 'c4', 'e', 'g', 'c5', 'e5', 'g4', 'c5', 'e5',
    'c4', 'd', 'a', 'd5', 'f5', 'a4', 'd5', 'f5', 'c4', 'd', 'a', 'd5', 'f5', 'a4', 'd5', 'f5',
    'b3', 'd4', 'g', 'd5', 'f5', 'g4', 'd5', 'f5', 'b3', 'd4', 'g', 'd5', 'f5', 'g4', 'd5', 'f5',
    'c4', 'e', 'g', 'c5', 'e5', 'g4', 'c5', 'e5', 'c4', 'e', 'g', 'c5', 'e5', 'g4', 'c5', 'e5',
    'c4', 'e', 'a', 'e5', 'a5', 'a4', 'e5', 'a5', 'c4', 'e', 'a', 'e5', 'a5', 'a4', 'e5', 'a5',
    'c4', 'd', 'f#', 'a', 'd5', 'f#4', 'a', 'd5', 'c4', 'd', 'f#', 'a', 'd5', 'f#4', 'a', 'd5',
    'b3', 'd4', 'g', 'd5', 'g5', 'g4', 'd5', 'g5', 'b3', 'd4', 'g', 'd5', 'g5', 'g4', 'd5', 'g5',
    'b3', 'c4', 'e', 'g', 'c5', 'e4', 'g', 'c5', 'b3', 'c4', 'e', 'g', 'c5', 'e4', 'g', 'c5',
    'a3', 'c4', 'e', 'g', 'c5', 'e4', 'g', 'c5', 'a3', 'c4', 'e', 'g', 'c5', 'e4', 'g', 'c5',
    'd3', 'a', 'd4', 'f#', 'c5', 'd4', 'f#', 'c5', 'd3', 'a', 'd4', 'f#', 'c5', 'd4', 'f#', 'c5',
    'g3', 'b', 'd4', 'g', 'b', 'd', 'g', 'b', 'g3', 'b3', 'd4', 'g', 'b', 'd', 'g', 'b'
]

music.play(notes)
```

3.异步播放
```python
import asyncio
from picoed import music, led

# play Prelude in C.
notes = [
    'c4:1', 'e', 'g', 'c5', 'e5', 'g4', 'c5', 'e5', 'c4', 'e', 'g', 'c5', 'e5', 'g4', 'c5', 'e5',
    'c4', 'd', 'a', 'd5', 'f5', 'a4', 'd5', 'f5', 'c4', 'd', 'a', 'd5', 'f5', 'a4', 'd5', 'f5',
    'b3', 'd4', 'g', 'd5', 'f5', 'g4', 'd5', 'f5', 'b3', 'd4', 'g', 'd5', 'f5', 'g4', 'd5', 'f5',
    'c4', 'e', 'g', 'c5', 'e5', 'g4', 'c5', 'e5', 'c4', 'e', 'g', 'c5', 'e5', 'g4', 'c5', 'e5',
    'c4', 'e', 'a', 'e5', 'a5', 'a4', 'e5', 'a5', 'c4', 'e', 'a', 'e5', 'a5', 'a4', 'e5', 'a5',
    'c4', 'd', 'f#', 'a', 'd5', 'f#4', 'a', 'd5', 'c4', 'd', 'f#', 'a', 'd5', 'f#4', 'a', 'd5',
    'b3', 'd4', 'g', 'd5', 'g5', 'g4', 'd5', 'g5', 'b3', 'd4', 'g', 'd5', 'g5', 'g4', 'd5', 'g5',
    'b3', 'c4', 'e', 'g', 'c5', 'e4', 'g', 'c5', 'b3', 'c4', 'e', 'g', 'c5', 'e4', 'g', 'c5',
    'a3', 'c4', 'e', 'g', 'c5', 'e4', 'g', 'c5', 'a3', 'c4', 'e', 'g', 'c5', 'e4', 'g', 'c5',
    'd3', 'a', 'd4', 'f#', 'c5', 'd4', 'f#', 'c5', 'd3', 'a', 'd4', 'f#', 'c5', 'd4', 'f#', 'c5',
    'g3', 'b', 'd4', 'g', 'b', 'd', 'g', 'b', 'g3', 'b3', 'd4', 'g', 'b', 'd', 'g', 'b'
]

async def blink(interval):
    while True:
        led.on()
        await asyncio.sleep(interval)
        led.off()
        await asyncio.sleep(interval)

async def main():
    player = asyncio.create_task(music.play_async(notes))
    light = asyncio.create_task(blink(0.1))
    await asyncio.gather(player)
    await asyncio.gather(light)

asyncio.run(main())

```

## 


