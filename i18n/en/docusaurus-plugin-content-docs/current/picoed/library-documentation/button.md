# 按键
Pico:ed包含A、B两个按键。
## 属性
### `button_a`
类[Button](#w8UmM)的实例，代表左侧的按键。
### `button_b`
类[Button](#w8UmM)的实例，代表右侧的按键。
## 类
### `class Button(pin)`
用来表示一个按键。

- **unit -** 按键引脚

> `**is_pressed()**`

如果按钮被按住，则返回 True，否则返回 False。

> `**was_pressed()**`

如果按钮被按下，且在下一次按下之前只返回一次 True。
## 示例
1.按键状态改变LED状态
```python
# 导入程序所需要的模块
from picoed import *

# 循环检测是否按下A键以及对LED的状态改变
while True:
    if button_a.is_pressed():
        led.on()
    else:
        led.off()

```

2.按键次数计数
```python
from picoed import *

times = 0
display.show(times)

while True:
    # 按一次A键，计数减1
    if button_a.was_pressed():
        times -= 1
        display.show(times)
    # 按一次B键，计数加1
    if button_b.was_pressed():
        times += 1
        display.show(times)

```
