# Поворот и отражение шестиугольника

## Задача 1 

```import numpy as np
import matplotlib.pyplot as plt

k = np.arange(7)
angles = 2 * np.pi * k / 6
hexagon = np.array([np.cos(angles), np.sin(angles)]).T

theta_45 = np.deg2rad(45)
rotate_45 = np.array([
    [np.cos(theta_45), -np.sin(theta_45)],
    [np.sin(theta_45), np.cos(theta_45)],
])

reflect_yx = np.array([
    [0.0, 1.0],
    [1.0, 0.0],
])

rotated_hex = hexagon @ rotate_45.T
reflected_hex = hexagon @ reflect_yx.T

fig, ax = plt.subplots(figsize=(7, 7))
ax.plot(hexagon[:, 0], hexagon[:, 1], 'o-', color='gray', alpha=0.6, lw=2, label='исходный')
ax.plot(rotated_hex[:, 0], rotated_hex[:, 1], 'o-', color='steelblue', lw=2, label='поворот на 45°')
ax.plot(reflected_hex[:, 0], reflected_hex[:, 1], 's--', color='crimson', lw=2, label='отражение y = x')
ax.axhline(0, color='black', lw=0.5)
ax.axvline(0, color='black', lw=0.5)
ax.set_aspect('equal')
ax.grid(True, alpha=0.3)
ax.legend(loc='upper left')
ax.set_title('Задача 1: Поворот и отражение шестиугольника')
plt.tight_layout()
fig.savefig('task1_hexagon.png', dpi=150, bbox_inches='tight')
plt.show()
```

### Задача 1. Ответ на вопрос:
* Матрица поворота на 90° имеет вид [[0, -1], [1, 0]], а матрица отражения относительно y = x — [[0, 1], [1, 0]]. Они совпадают с точностью до знака элементов. Поскольку правильный шестиугольник симметричен, отражение меняет ориентацию, но визуально фигура накладывается на себя так же, как при повороте на 90° (или 30° с учётом симметрии).