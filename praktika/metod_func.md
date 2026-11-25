# Метод Ньютона и метод секущих

## 1 Задача


```
import math

from bokeh.layouts import row


def newton(f, df, x0, eps=1e-12, max_iter=100):
    """
    Метод Ньютона (касательных).
    Возвращает: (корень, журнал, статус)
    """
    journal = []
    x = x0
    for k in range(1, max_iter + 1):
        fx = f(x)
        dfx = df(x)
        if dfx == 0:
            return x, journal, "zero_derivative"
        x_new = x - fx / dfx
        journal.append({
            "iter": k,
            "x": x,
            "f(x)": fx,
            "f'(x)": dfx,
            "x_new": x_new,
            "step": abs(x_new - x),
        })
        if abs(x_new - x) < eps or abs(fx) < eps:
            return x_new, journal, "ok"
        x = x_new
    return x, journal, "max_iter"


def secant(f, x0, x1, eps=1e-12, max_iter=100):
    """
    Метод секущих.
    Возвращает: (корень, журнал, статус)
    """
    journal = []
    x_prev, x_curr = x0, x1
    f_prev, f_curr = f(x_prev), f(x_curr)

    for k in range(1, max_iter + 1):
        if f_curr - f_prev == 0:
            return x_curr, journal, "zero_denominator"
        x_new = x_curr - f_curr * (x_curr - x_prev) / (f_curr - f_prev)
        journal.append({
            "iter": k,
            "x_prev": x_prev,
            "x_curr": x_curr,
            "f_curr": f_curr,
            "x_new": x_new,
            "step": abs(x_new - x_curr),
        })
        if abs(x_new - x_curr) < eps or abs(f_curr) < eps:
            return x_new, journal, "ok"
        x_prev, f_prev = x_curr, f_curr
        x_curr = x_new
        f_curr = f(x_curr)
    return x_curr, journal, "max_iter"


def print_journal_newton(journal, title=""):
    if title:
        print(f"\n=== {title} ===")
    print(f"{'k':>3} | {'x':>14} | {'f(x)':>14} | {'f\'(x)':>14} | {'шаг':>10}")
    print("-" * 70)
    for row in journal:
        print(f"{row['iter']:>3} | {row['x']:>14.10f} | {row['f(x)']:>14.3e} | "
              f"{row['f\'(x)']:>14.3e} | {row['step']:>10.2e}")


def print_journal_secant(journal, title=""):
    if title:
        print(f"\n=== {title} ===")
    print(f"{'k':>3} | {'x_prev':>14} | {'x_curr':>14} | {'f_curr':>14} | {'шаг':>10}")
    print("-" * 70)
    for row in journal:
        print(f"{row['iter']:>3} | {row['x_prev']:>14.10f} | {row['x_curr']:>14.3e} | "
              f"{row['f_curr']:>14.3e} | {row['step']:>10.2e}")

# Функция 1:
f1 = lambda x: x**3 - x - 1
df1 = lambda x: 3*x**2 - 1

root_n, jn, _ = newton(f1, df1, x0=1.5, eps=1e-12)
root_s, js, _ = secant(f1, x0=1.0, x1=1.5, eps=1e-12)

print_journal_newton(jn, "Ньютон: f(x) = x³ - x - 1,  x₀ = 1.5")
print(f"корень = {root_n:.12f},  итераций: {len(jn)}")

print_journal_secant(js, "Секущие: f(x) = x³ - x - 1,  x₀ = 1.0, x₁ = 1.5")
print(f"корень = {root_s:.12f},  итераций: {len(js)}")


# Функция 2:
f2 = lambda x: math.exp(x) - x - 2
df2 = lambda x: math.exp(x) - 1

root_n2, jn2, _ = newton(f2, df2, x0=1.0, eps=1e-12)
root_s2, js2, _ = secant(f2, x0=0.5, x1=1.5, eps=1e-12)

print_journal_newton(jn2, "Ньютон: f(x) = eˣ - x - 2,  x₀ = 1.0")
print(f"корень = {root_n2:.12f},  итераций: {len(jn2)}")


# Функция 3:
f3 = lambda x: math.sin(x) - 0.5*x
df3 = lambda x: math.cos(x) - 0.5

root_n3, jn3, _ = newton(f3, df3, x0=2.0, eps=1e-12)
print_journal_newton(jn3, "Ньютон: f(x) = sin(x) - 0.5x,  x₀ = 2.0")
print(f"корень = {root_n3:.12f},  итераций: {len(jn3)}")
```