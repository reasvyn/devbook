# Differential Equations

## Description

Differential equations describe how quantities change — the language of physics, engineering, biology, and economics. Every physics engine in games is built on ODE solvers. Population models, circuit simulation, chemical kinetics, and epidemiology all use differential equations. Understanding ODEs gives you the tools to model and simulate dynamic systems.

## Prerequisites

- [Integration](integration.md) — solving ODEs requires integration techniques
- [Multivariate Calculus](multivariate-calculus.md) — systems of ODEs involve multiple dependent variables

## Table of Contents

- [What is an ODE?](#what-is-an-ode)
- [Order and Linearity](#order-and-linearity)
- [Separation of Variables](#separation-of-variables)
- [Integrating Factors](#integrating-factors)
- [Second-Order Linear ODEs](#second-order-linear-odes)
- [Systems of ODEs](#systems-of-odes)
- [Numerical Methods](#numerical-methods)
- [Applications](#applications)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What is an ODE?

An ordinary differential equation (ODE) is an equation involving a function and its derivatives.

$$
\frac{dy}{dt} = f(t, y)
$$

The goal is to find $y(t)$ that satisfies the equation.

**Examples of ODEs in the real world:**

- Newton's cooling: $\frac{dT}{dt} = -k(T - T_{\text{env}})$
- Population growth: $\frac{dP}{dt} = rP$
- Spring motion: $m\frac{d^2x}{dt^2} = -kx$
- RL circuit: $L\frac{dI}{dt} + RI = V(t)$

```python
import numpy as np
import matplotlib.pyplot as plt

def exponential_growth(y0, r, t_max, dt=0.01):
    """Simulate dy/dt = r*y using Euler's method."""
    n_steps = int(t_max / dt)
    t = np.zeros(n_steps)
    y = np.zeros(n_steps)
    t[0], y[0] = 0, y0

    for i in range(1, n_steps):
        t[i] = i * dt
        y[i] = y[i-1] + r * y[i-1] * dt  # Euler step

    return t, y

t, y = exponential_growth(1.0, 0.5, 5)
print(f"At t={t[-1]:.2f}: y={y[-1]:.4f} (exact: {np.exp(0.5*5):.4f})")
```

### Order and Linearity

**Order:** The highest derivative in the equation.

- First-order: $\frac{dy}{dt} = ky$
- Second-order: $\frac{d^2y}{dt^2} + \omega^2 y = 0$
- Third-order: $\frac{d^3y}{dt^3} - \frac{dy}{dt} = 0$

**Linearity:** An ODE is linear if $y$ and its derivatives appear only to the first power, not multiplied together, and not inside nonlinear functions.

- Linear: $y'' + p(t)y' + q(t)y = g(t)$
- Nonlinear: $y'' + \sin(y) = 0$ (pendulum), $y' = y^2$

```python
import sympy as sp

t = sp.Symbol('t')
y = sp.Function('y')

# Check order of an ODE
ode1 = sp.diff(y(t), t) - sp.sin(t)
ode2 = sp.diff(y(t), t, 2) + 3*sp.diff(y(t), t) + 2*y(t) - sp.exp(t)

def ode_order(ode):
    """Determine the order of an ODE."""
    from sympy import oo
    max_order = 0
    for arg in sp.preorder_traversal(ode):
        if isinstance(arg, sp.Derivative):
            if len(arg.args[1:]) > max_order:
                max_order = len(arg.args[1:])
    return max_order

print(f"Order of y' - sin(t) = 0: {ode_order(ode1)}")
print(f"Order of y'' + 3y' + 2y = e^t: {ode_order(ode2)}")
```

### Separation of Variables

For ODEs of the form $\frac{dy}{dt} = g(t)h(y)$:

1. Separate: $\frac{dy}{h(y)} = g(t) \, dt$
2. Integrate both sides: $\int \frac{dy}{h(y)} = \int g(t) \, dt$
3. Solve for $y$ if possible.

**Example:** $\frac{dy}{dt} = ky$ (exponential growth)

$$
\frac{dy}{y} = k \, dt \implies \ln|y| = kt + C \implies y = Ce^{kt}
$$

```python
import sympy as sp

t = sp.Symbol('t')
y = sp.Function('y')
ode = sp.Eq(sp.diff(y(t), t), 2 * y(t))

sol = sp.dsolve(ode)
print(f"y' = 2y: {sol}")

# With initial condition y(0) = 3
sol_ic = sp.dsolve(ode, ics={y(0): 3})
print(f"With y(0)=3: {sol_ic}")
```

**Example:** Logistic growth $\frac{dP}{dt} = rP\left(1 - \frac{P}{K}\right)$

```python
def logistic_growth(P0, r, K, t_max, dt=0.01):
    """Simulate logistic growth."""
    n_steps = int(t_max / dt)
    t = np.zeros(n_steps)
    P = np.zeros(n_steps)
    t[0], P[0] = 0, P0

    for i in range(1, n_steps):
        t[i] = i * dt
        P[i] = P[i-1] + r * P[i-1] * (1 - P[i-1]/K) * dt

    return t, P

t, P = logistic_growth(10, 0.5, 100, 20)
print(f"At t={t[-1]:.1f}: P={P[-1]:.2f} (approaching K=100)")
```

### Integrating Factors

For first-order linear ODEs: $\frac{dy}{dt} + p(t)y = g(t)$

Multiply by the integrating factor $\mu(t) = e^{\int p(t) \, dt}$:

$$
\frac{d}{dt}[\mu(t) y] = \mu(t) g(t)
$$

Then integrate:

$$
y = \frac{1}{\mu(t)} \int \mu(t) g(t) \, dt
$$

**Example:** $y' + 2ty = t$ with $y(0) = 1$

Here $p(t) = 2t$, so $\mu(t) = e^{\int 2t \, dt} = e^{t^2}$.

$$
\frac{d}{dt}[e^{t^2} y] = t e^{t^2} \implies y = e^{-t^2} \left( \frac{1}{2} e^{t^2} + C \right) = \frac{1}{2} + Ce^{-t^2}
$$

With $y(0) = 1$, $C = \frac{1}{2}$, so $y = \frac{1}{2}(1 + e^{-t^2})$.

```python
import sympy as sp

t = sp.Symbol('t')
y = sp.Function('y')

ode = sp.Eq(sp.diff(y(t), t) + 2*t*y(t), t)
sol = sp.dsolve(ode, ics={y(0): 1})
print(f"y' + 2ty = t, y(0)=1: {sol}")
```

```python
import numpy as np

def integrating_factor_numerical(p_func, g_func, y0, t_span, dt=0.01):
    """Solve y' + p(t)y = g(t) using integrating factor method numerically."""
    t = np.arange(t_span[0], t_span[1], dt)
    n = len(t)
    y = np.zeros(n)
    y[0] = y0

    for i in range(1, n):
        dt_i = t[i] - t[i-1]
        mu = np.exp(np.trapz(p_func(t[:i+1]), t[:i+1]))
        y[i] = (1/mu) * (y0 + np.trapz(mu * g_func(t[:i+1]), t[:i+1]))

    return t, y

t, y = integrating_factor_numerical(
    lambda t: 2*t,
    lambda t: t,
    1.0, (0, 3), dt=0.01
)
print(f"y(3) ≈ {y[-1]:.6f} (exact: {0.5*(1+np.exp(-9)):.6f})")
```

### Second-Order Linear ODEs

General form: $a y'' + b y' + c y = f(t)$

**Homogeneous ($f(t) = 0$):** Assume $y = e^{rt}$. The characteristic equation is $ar^2 + br + c = 0$.

- Distinct real roots: $y = C_1 e^{r_1 t} + C_2 e^{r_2 t}$
- Repeated root: $y = (C_1 + C_2 t)e^{rt}$
- Complex roots $r = \alpha \pm i\beta$: $y = e^{\alpha t}(C_1 \cos \beta t + C_2 \sin \beta t)$

```python
import sympy as sp

t = sp.Symbol('t')
y = sp.Function('y')

# y'' - 3y' + 2y = 0 (distinct real roots: r=1, r=2)
ode1 = sp.Eq(sp.diff(y(t), t, 2) - 3*sp.diff(y(t), t) + 2*y(t), 0)
sol1 = sp.dsolve(ode1)
print(f"y'' - 3y' + 2y = 0: {sol1}")

# y'' + 2y' + y = 0 (repeated root: r=-1)
ode2 = sp.Eq(sp.diff(y(t), t, 2) + 2*sp.diff(y(t), t) + y(t), 0)
sol2 = sp.dsolve(ode2)
print(f"y'' + 2y' + y = 0: {sol2}")

# y'' + 4y = 0 (complex roots: r = ±2i)
ode3 = sp.Eq(sp.diff(y(t), t, 2) + 4*y(t), 0)
sol3 = sp.dsolve(ode3)
print(f"y'' + 4y = 0: {sol3}")
```

**Harmonic oscillator:** $m\ddot{x} + kx = 0$ (no damping)

The solution is $x(t) = A\cos(\omega t) + B\sin(\omega t)$ where $\omega = \sqrt{k/m}$.

```python
import numpy as np

def harmonic_oscillator(A, B, omega, t):
    """Analytic solution of undamped harmonic oscillator."""
    return A * np.cos(omega * t) + B * np.sin(omega * t)

def energy(x, v, m=1, k=1):
    """Compute total energy of harmonic oscillator."""
    return 0.5 * m * v**2 + 0.5 * k * x**2

omega = 2.0
t_vals = np.linspace(0, 10, 100)
x_vals = harmonic_oscillator(1.0, 0.0, omega, t_vals)
v_vals = -1.0 * omega * np.sin(omega * t_vals) + 0.0 * omega * np.cos(omega * t_vals)
E = energy(x_vals, v_vals)
print(f"Energy conservation: max={E.max():.4e}, min={E.min():.4e}, range={E.max()-E.min():.2e}")
```

### Systems of ODEs

Many real systems require multiple coupled ODEs.

**Lotka-Volterra predator-prey model:**

$$
\frac{dx}{dt} = \alpha x - \beta xy
$$

$$
\frac{dy}{dt} = \delta xy - \gamma y
$$

```python
import numpy as np

def lotka_volterra(x0, y0, alpha, beta, delta, gamma, t_max, dt=0.01):
    """Simulate predator-prey system using Euler's method."""
    n_steps = int(t_max / dt)
    t = np.zeros(n_steps)
    x = np.zeros(n_steps)
    y = np.zeros(n_steps)
    t[0], x[0], y[0] = 0, x0, y0

    for i in range(1, n_steps):
        t[i] = i * dt
        dx = alpha * x[i-1] - beta * x[i-1] * y[i-1]
        dy = delta * x[i-1] * y[i-1] - gamma * y[i-1]
        x[i] = x[i-1] + dx * dt
        y[i] = y[i-1] + dy * dt

    return t, x, y

t, x, y = lotka_volterra(10, 5, 1.0, 0.1, 0.075, 1.5, 50)
print(f"Final: prey={x[-1]:.2f}, predator={y[-1]:.2f}")
```

**SIR model for epidemiology:**

$$
\frac{dS}{dt} = -\beta SI,\quad
\frac{dI}{dt} = \beta SI - \gamma I,\quad
\frac{dR}{dt} = \gamma I
$$

```python
import numpy as np

def sir_model(S0, I0, beta, gamma, t_max, dt=0.01):
    """Simulate SIR epidemic model."""
    n_steps = int(t_max / dt)
    t = np.zeros(n_steps)
    S = np.zeros(n_steps)
    I = np.zeros(n_steps)
    R = np.zeros(n_steps)
    t[0], S[0], I[0], R[0] = 0, S0, I0, 0

    N = S0 + I0  # total population

    for i in range(1, n_steps):
        t[i] = i * dt
        dS = -beta * S[i-1] * I[i-1] / N
        dI = beta * S[i-1] * I[i-1] / N - gamma * I[i-1]
        dR = gamma * I[i-1]
        S[i] = S[i-1] + dS * dt
        I[i] = I[i-1] + dI * dt
        R[i] = R[i-1] + dR * dt

    return t, S, I, R

t, S, I, R = sir_model(990, 10, 0.3, 0.1, 100)
print(f"At t={t[-1]:.0f}: S={S[-1]:.0f}, I={I[-1]:.0f}, R={R[-1]:.0f}")
print(f"Total infected: {R[-1]:.0f} ({(R[-1]/990)*100:.1f}%)")
```

### Numerical Methods

Most ODEs cannot be solved analytically. Numerical methods approximate solutions.

**Euler's method:**

$$
y_{n+1} = y_n + h f(t_n, y_n)
$$

Simple but not very accurate. Error per step is $O(h^2)$.

```python
import numpy as np

def euler(f, y0, t_span, dt=0.01):
    """Euler's method for ODE y' = f(t, y)."""
    t = np.arange(t_span[0], t_span[1], dt)
    n = len(t)
    y = np.zeros(n)
    y[0] = y0

    for i in range(1, n):
        y[i] = y[i-1] + dt * f(t[i-1], y[i-1])

    return t, y

def f_cooling(t, T):
    return -0.1 * (T - 20)  # Newton's cooling, env temp = 20

t, T = euler(f_cooling, 100, (0, 100), dt=0.5)
print(f"Euler: T(100) = {T[-1]:.4f} (exact: {20 + 80*np.exp(-0.1*100):.4f})")
```

**Runge-Kutta (RK4):** Fourth-order accurate — error per step is $O(h^4)$.

$$
k_1 = f(t_n, y_n)
$$

$$
k_2 = f(t_n + \frac{h}{2}, y_n + \frac{h}{2}k_1)
$$

$$
k_3 = f(t_n + \frac{h}{2}, y_n + \frac{h}{2}k_2)
$$

$$
k_4 = f(t_n + h, y_n + h k_3)
$$

$$
y_{n+1} = y_n + \frac{h}{6}(k_1 + 2k_2 + 2k_3 + k_4)
$$

```python
import numpy as np

def rk4(f, y0, t_span, dt=0.01):
    """Classical fourth-order Runge-Kutta method."""
    t = np.arange(t_span[0], t_span[1], dt)
    n = len(t)
    y = np.zeros(n)
    y[0] = y0

    for i in range(1, n):
        h = t[i] - t[i-1]
        k1 = f(t[i-1], y[i-1])
        k2 = f(t[i-1] + h/2, y[i-1] + h/2 * k1)
        k3 = f(t[i-1] + h/2, y[i-1] + h/2 * k2)
        k4 = f(t[i-1] + h, y[i-1] + h * k3)
        y[i] = y[i-1] + h/6 * (k1 + 2*k2 + 2*k3 + k4)

    return t, y

def f_decay(t, y):
    return -y  # dy/dt = -y

t_euler, y_euler = euler(f_decay, 1.0, (0, 5), dt=0.1)
t_rk4, y_rk4 = rk4(f_decay, 1.0, (0, 5), dt=0.1)
exact = np.exp(-t_rk4)
print(f"At t=5: Euler={y_euler[-1]:.6f}, RK4={y_rk4[-1]:.6f}, exact={exact[-1]:.6f}")
print(f"Euler error: {abs(y_euler[-1] - exact[-1]):.2e}")
print(f"RK4 error:   {abs(y_rk4[-1] - exact[-1]):.2e}")
```

**Using scipy.integrate.odeint (now solve_ivp):**

```python
from scipy.integrate import solve_ivp
import numpy as np

def f_system(t, y):
    """dy/dt = -y, dz/dt = y - z (coupled system)."""
    return [-y[0], y[0] - y[1]]

sol = solve_ivp(f_system, [0, 5], [1.0, 0.0], method='RK45', dense_output=True)
t = sol.t
y = sol.y
print(f"Time points: {t}")
print(f"y solutions: {y[0, -1]:.6f}, {y[1, -1]:.6f}")
```

### Applications

**Physics engine — spring with damping:**

$$
m\ddot{x} + c\dot{x} + kx = 0
$$

This is a second-order ODE. Convert to a system of first-order ODEs:

$$
\dot{x} = v, \quad \dot{v} = -\frac{c}{m}v - \frac{k}{m}x
$$

```python
import numpy as np
from scipy.integrate import solve_ivp

def damped_spring(t, state, m=1.0, c=0.5, k=10.0):
    x, v = state
    dxdt = v
    dvdt = -c/m * v - k/m * x
    return [dxdt, dvdt]

sol = solve_ivp(damped_spring, [0, 20], [1.0, 0.0],
                args=(1.0, 0.5, 10.0), max_step=0.01)

print(f"At t=20: x={sol.y[0,-1]:.4f}, v={sol.y[1,-1]:.4f}")
print(f"Oscillations decay to zero (damped)")
```

**Circuit simulation — RC circuit:**

$$
RC \frac{dV_c}{dt} + V_c = V_{in}(t)
$$

```python
import numpy as np
from scipy.integrate import solve_ivp

def rc_circuit(t, Vc, R=1e3, C=1e-6, V_in=5.0):
    """RC circuit charging."""
    return (V_in - Vc) / (R * C)

sol = solve_ivp(rc_circuit, [0, 0.01], [0.0], args=(1e3, 1e-6, 5.0), max_step=1e-5)
print(f"At t={sol.t[-1]*1000:.2f}ms: Vc={sol.y[0,-1]:.4f}V (approaching 5V)")
```

**Epidemiology — SEIR model:**

The SEIR model adds an exposed (E) compartment to SIR.

$$
\frac{dS}{dt} = -\beta SI, \quad
\frac{dE}{dt} = \beta SI - \sigma E, \quad
\frac{dI}{dt} = \sigma E - \gamma I, \quad
\frac{dR}{dt} = \gamma I
$$

```python
import numpy as np
from scipy.integrate import solve_ivp

def seir_model(t, y, beta=0.5, sigma=0.2, gamma=0.1):
    S, E, I, R = y
    N = S + E + I + R
    dS = -beta * S * I / N
    dE = beta * S * I / N - sigma * E
    dI = sigma * E - gamma * I
    dR = gamma * I
    return [dS, dE, dI, dR]

sol = solve_ivp(seir_model, [0, 200], [999, 1, 0, 0],
                args=(0.5, 0.2, 0.1), max_step=0.5)
S, E, I, R = sol.y
print(f"Peak infections: {I.max():.0f} at t={sol.t[np.argmax(I)]:.1f}")
print(f"Final recovered: {R[-1]:.0f}")
```

## Glossary

| Term | Definition |
|------|------------|
| ODE | Ordinary differential equation — equation involving derivatives of a function |
| Order | The highest derivative in an ODE |
| Linear ODE | ODE where the function and its derivatives appear linearly |
| Nonlinear ODE | ODE involving products or nonlinear functions of the function or its derivatives |
| Separation of variables | Method for ODEs where variables can be separated |
| Integrating factor | Function used to solve first-order linear ODEs |
| Characteristic equation | Polynomial equation for solving linear ODEs with constant coefficients |
| Homogeneous | ODE with zero right-hand side |
| Particular solution | A specific solution to a non-homogeneous ODE |
| Euler's method | First-order numerical ODE solver |
| Runge-Kutta | Family of higher-order numerical ODE solvers (RK4 is most common) |
| Stiff ODE | ODE requiring implicit methods due to widely separated time scales |
| Phase portrait | Plot of trajectories in state space |
| BVP | Boundary value problem — conditions specified at boundaries |
| IVP | Initial value problem — conditions specified at a starting point |
| Lyapunov exponent | Measure of sensitivity to initial conditions (chaos) |
| BDF | Backward differentiation formula — implicit solver for stiff ODEs |
| Chaotic system | System exhibiting sensitive dependence on initial conditions |

## Quick References

- [Scipy Integrate Documentation](https://docs.scipy.org/doc/scipy/reference/integrate.html) — solve_ivp, odeint, solve_bvp
- [SymPy ODE Documentation](https://docs.sympy.org/latest/modules/solvers/ode.html) — symbolic ODE solving
- [Paul's Online Math Notes — Differential Equations](https://tutorial.math.lamar.edu/Classes/DE/DE.aspx) — comprehensive reference
- [Lorenz Attractor — Wikipedia](https://en.wikipedia.org/wiki/Lorenz_system) — the classic chaotic system

## Next Steps

This is the final module in the calculus sequence. You have covered limits, derivatives, integration, multivariate calculus, series, and differential equations — the essential toolkit for scientific and machine learning computing.
