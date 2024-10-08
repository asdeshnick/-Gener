# Генерация лабиринта 

<img src="https://i.imgur.com/fTDcjB3.png" width="450" height="450">


#### Из чего состоит лабиринт?
Лабиринт состоит из препятсвий: <span style="color:rgb(255,127,0)">ящиков</span> и <span  style="color:rgb(255,50,0)">стенок</span>.
Также слева находится <span style="color:rgb(0,255,0)">вход</span> в лабиринт,
справа <span style="color:rgb(0,127,255)">выход</span>

#### Настройка окружения

1. Создайте удобным для вас способ окружение (venv)
2. В активированном окружении запустите в терминале
`pip install numpy opencv-python matplotlib`

#### Импорт необходимых библиотек

```
import numpy as np
import cv2
import random
import json
import matplotlib.pyplot as plt
```

### Функции:


### Разрез прямоугольника 

<img src="https://i.imgur.com/26djpRo.png" width="650" height="450">

Пусть исходный прямоугольник (ящик) задан кординатами углов: 

Цель - "разрезать" ящик, добавивь проход и стенки на нем 

#### Возвращает
- кортеж, содержащий:
  - список кортежей с координатами новых прямоугольников
  - список кортежей с информацией о перегородках
  - возможную точку входа в лабиринт, если разрез горизонтальный и исходный ящик касается левой стенки поля, в противном случае None
  - возможную точку выхода из лабиринта, если разрез горизонтальный и исходный ящик касается правой стенки поля, в противном случае None

#### Примечание
- стенки выбираются следущим образом: 
  1. Создается список стенок через каждые `d` пикселей 
  2. Случайно отбирается 10%
 

### Функция: create_maze

Эта функция генерирует лабиринт, рекурсивно разрезая коробки на меньшие.

#### Алгоритм:

1. Если `n` равно 0, то возвращает текущее состояние `box_list`, `wall_list`, `entry_point_list` и `exit_point_list`.
2. Для каждой коробки в `box_list`:
   - Вызывает функцию `cut_pram` с параметрами разреза.
  

### Функция: `show_maze`.

Визуализирует лабиринт


#### Задание параметров

```
maze_size = 1000    # размер стороны лабиринта в пикселях 
d = 15  # минимальное расстояние между стенками 
c = 20  # ширина проходов
min_portion = 0.35  # минимальный процент ширины или высоты прямоугольника, в пределах которого может быть сделан разрез 
n = 8   # количество рекурсивных вызовов разреза всех коробок.
window = False  # ключ для визуализации в отдельном окне 
seed = 345  # сид для постоянства random
```

#### Запуск программы 

```
random.seed(seed)
wall_list = []
box_list = [(0,0,maze_size,maze_size)]
entry_point_list = []
exit_point_list = []

box_list, wall_list, entry_point_list, exit_point_list = create_maze(
    box_list,
    wall_list,
    n,
    min_portion,
    d, c,
    maze_size,
    entry_point_list,
    exit_point_list
    )

entry_point = random.sample(entry_point_list, 1)[0]
exit_point = random.sample(exit_point_list, 1)[0]

maze_img = show_maze(box_list, wall_list, entry_point, exit_point, window, maze_size)
```


```
plt.figure(figsize=(9,9))
plt.imshow(cv2.cvtColor(maze_img, cv2.COLOR_BGR2RGB))
plt.axis('off')
plt.show()
```
