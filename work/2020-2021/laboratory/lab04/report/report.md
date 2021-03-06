---
# Front matter
lang: ru-RU
title: "Отчёта по лабораторной работе №4"
subtitle: "Модель гармонических колебаний"
author: "Шувалов Николай Константинович"

# Formatting
toc-title: "Содержание"
toc: true # Table of contents
toc_depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4paper
documentclass: scrreprt
polyglossia-lang: russian
polyglossia-otherlangs: english
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase
indent: true
pdf-engine: lualatex
header-includes:
  - \linepenalty=10 # the penalty added to the badness of each line within a paragraph (no associated penalty node) Increasing the value makes tex try to have fewer lines in the paragraph.
  - \interlinepenalty=0 # value of the penalty (node) added after each line of a paragraph.
  - \hyphenpenalty=50 # the penalty for line breaking at an automatically inserted hyphen
  - \exhyphenpenalty=50 # the penalty for line breaking at an explicit hyphen
  - \binoppenalty=700 # the penalty for breaking a line at a binary operator
  - \relpenalty=500 # the penalty for breaking a line at a relation
  - \clubpenalty=150 # extra penalty for breaking after first line of a paragraph
  - \widowpenalty=150 # extra penalty for breaking before last line of a paragraph
  - \displaywidowpenalty=50 # extra penalty for breaking before last line before a display math
  - \brokenpenalty=100 # extra penalty for page breaking after a hyphenated line
  - \predisplaypenalty=10000 # penalty for breaking before a display
  - \postdisplaypenalty=0 # penalty for breaking after a display
  - \floatingpenalty = 20000 # penalty for splitting an insertion (can only be split footnote in standard LaTeX)
  - \raggedbottom # or \flushbottom
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Познакомиться с моделью гармонических колебаний.

# Задание

1. Построить решение уравнения гармонического осциллятора без затухания 

2. Записать уравнение свободных колебаний гармонического осциллятора с
затуханием, построить его решение. Построить фазовый портрет гармонических
колебаний с затуханием.

3. Записать уравнение колебаний гармонического осциллятора, если на систему
действует внешняя сила, построить его решение. Построить фазовый портрет
колебаний с действием внешней силы.


# Теоретическая справка

Движение грузика на пружинке, маятника, заряда в электрическом контуре, а также эволюция во 
времени многих систем в физике, химии, биологии и других науках при определенных предположениях 
можно описать одним и тем же дифференциальным уравнением, которое в теории колебаний 
выступает в качестве основной модели. Эта модель называется линейным гармоническим 
осциллятором. Уравнение свободных колебаний гармонического осциллятора имеет следующий вид: 
$$\ddot{x}+2\gamma\dot{x}+\omega_0^2=0$$

где $x$ - переменная, описывающая состояние системы (смещение грузика, заряд конденсатора и т.д.), 
$\gamma$ - параметр, характеризующий потери энергии (трение в механической системе, 
сопротивление в контуре), $\omega_0$ - собственная частота колебаний. Это уравнение есть линейное 
однородное дифференциальное уравнение второго порядка и оно является примером линейной 
динамической системы.

При отсутствии потерь в системе ( $\gamma=0$ ) получаем уравнение консервативного осциллятора 
энергия колебания которого сохраняется во времени. $$\ddot{x}+\omega_0^2x=0$$

Для однозначной разрешимости уравнения второго порядка необходимо задать два начальных 
условия вида

$$ \begin{cases} x(t_0)=x_0 \
\dot{x(t_0)}=y_0 \end{cases} $$

Уравнение второго порядка можно представить в виде системы двух уравнений первого порядка: 
$$ \begin{cases} x=y \
y=-\omega_0^2x \end{cases} $$

Начальные условия для системы примут вид: $$ \begin{cases} x(t_0)=x_0 \
y(t_0)=y_0 \end{cases} $$

Независимые переменные $x, y$ определяют пространство, в котором «движется» решение. Это 
фазовое пространство системы, поскольку оно двумерно будем называть его фазовой плоскостью. 
Значение фазовых координат $x, y$ в любой момент времени полностью определяет состояние 
системы. Решению уравнения движения как функции времени отвечает гладкая кривая в фазовой 
плоскости. Она называется фазовой траекторией. Если множество различных решений 
(соответствующих различным начальным условиям) изобразить на одной фазовой плоскости, возникает 
общая картина поведения системы. Такую картину, образованную набором фазовых траекторий, 
называют фазовым портретом.


# Выполнение лабораторной работы

Условие задачи (рис. -@fig:001)

![Условие](image/усл.png){ #fig:001 width=70% }

Написал код:
```
import numpy as np
import math
import matplotlib.pyplot as plt
from scipy.integrate import odeint

w = 4.3
tmax = 61
step = 0.05
y0 = [-0.3, 1.3]
def W(y, t):
    y1, y2 = y
    return[y2, -w*y1]
t = np.arange(0, tmax, step)
w1 = odeint(W, y0, t)
y11 = w1[:,0]
y21 = w1[:,1]

fig = plt.figure()
plt.plot(t, y11)
plt.xlabel("t")
plt.ylabel("x")
plt.grid(True)
plt.show()
fig1 = plt.figure()
plt.plot(y11,y21)
plt.xlabel("x")
plt.ylabel("y")
plt.grid(True)
plt.show()

w = 20
g = 1
def W(y,t):
    y1,y2 = y
    return[y2, -w*w*y1 -g*y2]
t = np.arange(0, tmax, step)
w1 = odeint(W, y0, t)
y11 = w1[:,0]
y21 = w1[:,1]

fig = plt.figure()
plt.plot(t,y11)
plt.xlabel("t")
plt.ylabel("x")
plt.grid(True)
plt.show()
fig1 = plt.figure()
plt.plot(y11,y21)
plt.xlabel("x")
plt.ylabel("y")
plt.grid(True)
plt.show()

w = 8.8
g = 1
def f(t):
    f =0.7*math.sin(3*t)
    return f
def W(y,t):
    y1,y2 = y
    return [y2, -w*w*y1-g*y2+f(t)]
t = np.arange(0, tmax, step)
w1 = odeint(W, y0, t)
y11 = w1[:,0]
y21 = w1[:,1]

fig = plt.figure()
plt.plot(t,y11)
plt.xlabel("t")
plt.ylabel("x")
plt.grid(True)
plt.show()
fig1 = plt.figure()
plt.plot(y11,y21)
plt.xlabel("x")
plt.ylabel("y")
plt.grid(True)
plt.show()                          
```
![График решения для случая 1](image/1.png){ #fig:002 width=70% }

![Фазовый портрет для случая 1](image/ф1.png){ #fig:003 width=70% }

![График решения для случая 2](image/2.png){ #fig:004 width=70% }

![Фазовый портрет для случая 2](image/ф2.png){ #fig:005 width=70% }

![График решения для случая 3](image/3.png){ #fig:006 width=70% }

![Фазовый портрет для случая 3](image/ф3.png){ #fig:007 width=70% }

# Выводы

Познакомились с моделью гармонических колебаний.

