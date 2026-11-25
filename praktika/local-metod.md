# Локализация корня и метод половинного деления

## 1 Задача


```
import math

def bisection(f, a, b, eps=1e-9, max_iter=200):
    """
    Метод половинного деления с журналом итераций.

    Возвращает: (корень, журнал, статус)
      статус = 'ok'       — достигнута точность по длине интервала
      статус = 'f_zero'   — f(c) == 0 (точное попадание)
      статус = 'max_iter' — исчерпан лимит итераций
    """
    fa, fb = f(a), f(b)
    if fa * fb > 0:
        raise ValueError(
            f"Нет смены знака на [{a}, {b}]: f(a)={fa}, f(b)={fb}"
        )

    journal = []

    for k in range(1, max_iter + 1):
        c = (a + b) / 2
        fc = f(c)
        length = b - a

        journal.append({
            "iter": k,
            "a": a,
            "b": b,
            "c": c,
            "f(c)": fc,
            "length": length,
        })

        if fc == 0:
            return c, journal, "f_zero"
        if length < eps:
            return c, journal, "ok"

        if fa * fc < 0:
            b = c
            fb = fc
        else:
            a = c
            fa = fc

    return c, journal, "max_iter"


def print_journal(journal, title=""):
    if title:
        print(f"\n=== {title} ===")
    print(f"{'k':>3} | {'a':>12} | {'b':>12} | {'c':>12} | "
          f"{'f(c)':>14} | {'b-a':>10}")
    print("-" * 80)
    for row in journal:
        print(f"{row['iter']:>3} | {row['a']:>12.6f} | {row['b']:>12.6f} | "
              f"{row['c']:>12.6f} | {row['f(c)']:>14.3e} | "
              f"{row['length']:>10.2e}")


def f1(x):
    return x**3 - x - 1

# Локализация: f(1) = -1, f(2) = 5 → корень на [1, 2]
root1, j1, s1 = bisection(f1, 1, 2, eps=1e-9)
print_journal(j1, "f(x) = x³ - x - 1, [1, 2]")
print(f"корень: {root1:.12f}, f(root) = {f1(root1):.3e}, статус: {s1}")
print(f"итераций: {len(j1)}")


def f2(x):
    return math.exp(x) - x - 2

# f(0) = 1 - 0 - 2 = -1; f(2) = e² - 2 - 2 ≈ 3.389 → корень на [0, 2]
root2, j2, s2 = bisection(f2, 0, 2, eps=1e-9)
print_journal(j2, "f(x) = eˣ - x - 2, [0, 2]")
print(f"корень: {root2:.12f}, f(root) = {f2(root2):.3e}, статус: {s2}")

```