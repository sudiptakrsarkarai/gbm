# Brownian Motion Models for Stochastic Processes

![Simulation](https://i.imgur.com/gA3qH4P.png) 

This repository provides a mathematical explanation and Python-based simulation of **Simple (Arithmetic) Brownian Motion** and **Geometric Brownian Motion**. The concepts are demonstrated using a non-traditional example—modeling a cricket score progression—to make the underlying principles of stochastic calculus more intuitive.

## Table of Contents
1.  [Mathematical Foundations](#mathematical-foundations)
    * [The Wiener Process](#the-wiener-process)
    * [Ito's Calculus & Ito's Lemma](#itos-calculus--itos-lemma)
2.  [Simple Brownian Motion (SBM)](#simple-brownian-motion-sbm)
    * [SDE and Derivation](#sde-and-derivation)
3.  [Geometric Brownian Motion (GBM)](#geometric-brownian-motion-gbm)
    * [SDE and Derivation](#sde-and-derivation-1)
4.  [Simulation Code](#simulation-code)
5.  [How to Use](#how-to-use)
6.  [Additional Notes](#additional-notes)

---

## Mathematical Foundations

To understand how we model these processes, we first need to understand their mathematical building blocks.

### The Wiener Process

The foundation of our models is the **Standard Wiener Process** (or Standard Brownian Motion), $W_t$. It formalizes the idea of a continuous-time random walk.

A process $W_t$ is a Wiener Process if:
1.  $W_0 = 0$.
2.  It has independent increments: the change in one time interval is independent of the change in any other non-overlapping interval.
3.  Its increments $W_t - W_s$ are normally distributed with mean 0 and variance $t-s$, i.e., $W_t - W_s \sim N(0, t-s)$.
4.  Its paths are continuous but nowhere differentiable.

### Ito's Calculus & Ito's Lemma

Because the Wiener process is not differentiable, standard calculus rules don't apply. We must use **Ito's Calculus**.

**Ito's Lemma** is the chain rule for stochastic processes. If a process $X_t$ follows the Stochastic Differential Equation (SDE) $dX_t = a dt + b dW_t$, then for any twice-differentiable function $f(t, x)$, the process $Y_t = f(t, X_t)$ is given by:

$$ df = \left( \frac{\partial f}{\partial t} + a \frac{\partial f}{\partial x} + \frac{1}{2} b^2 \frac{\partial^2 f}{\partial x^2} \right) dt + b \frac{\partial f}{\partial x} dW_t $$

The extra term, $\frac{1}{2} b^2 \frac{\partial^2 f}{\partial x^2}$, arises because the variance of the Wiener process is proportional to time, leading to the heuristic rule $(dW_t)^2 = dt$.

---

## Simple Brownian Motion (SBM)

SBM (also called Arithmetic Brownian Motion) models processes where the change is additive and independent of the current level.

### SDE and Derivation

The SDE for SBM is:
$$ dS_t = \mu dt + \sigma dW_t $$
Where:
* $S_t$ is the value of the process at time $t$.
* $\mu$ is the constant drift.
* $\sigma$ is the constant volatility.

**Derivation:**
Since the coefficients $\mu$ and $\sigma$ are constants, we can solve this SDE by direct integration from $0$ to $T$:
$$ \int_0^T dS_t = \int_0^T \mu dt + \int_0^T \sigma dW_t $$
$$ S_T - S_0 = \mu \int_0^T dt + \sigma \int_0^T dW_t $$
The final form of the process is:
$$ S_T = S_0 + \mu T + \sigma W_T $$
This shows that $S_T$ is normally distributed. While simple, its main drawback is that it can model values becoming negative, which is unrealistic for quantities like scores or prices.

---

## Geometric Brownian Motion (GBM)

GBM is a more realistic model for quantities that are non-negative and grow proportionally. The change in the process is a percentage of its current level.

### SDE and Derivation

The SDE for GBM is:
$$ dS_t = \mu S_t dt + \sigma S_t dW_t $$
Where:
* $\mu$ is the proportional drift (percentage growth rate).
* $\sigma$ is the proportional volatility.

**Derivation:**
We cannot integrate this directly. We use Ito's Lemma with the function $f(S_t) = \ln(S_t)$ to simplify the SDE. Let $Y_t = \ln(S_t)$.

The partial derivatives of $f(S_t)$ are:
$$ \frac{\partial f}{\partial S_t} = \frac{1}{S_t}, \quad \frac{\partial^2 f}{\partial S_t^2} = -\frac{1}{S_t^2}, \quad \frac{\partial f}{\partial t} = 0 $$

In our SDE, $a = \mu S_t$ and $b = \sigma S_t$. Plugging these into Ito's Lemma:
$$ d(\ln S_t) = \left( 0 + (\mu S_t) \left(\frac{1}{S_t}\right) + \frac{1}{2} (\sigma S_t)^2 \left(-\frac{1}{S_t^2}\right) \right) dt + (\sigma S_t) \left(\frac{1}{S_t}\right) dW_t $$
This simplifies to a process with constant coefficients:
$$ d(\ln S_t) = \left( \mu - \frac{1}{2} \sigma^2 \right) dt + \sigma dW_t $$
Now we can integrate this from $0$ to $T$:
$$ \ln S_T - \ln S_0 = \left( \mu - \frac{1}{2} \sigma^2 \right) T + \sigma W_T $$
Finally, exponentiating both sides gives the solution for $S_T$:
$$ S_T = S_0 \exp\left( \left(\mu - \frac{1}{2} \sigma^2\right)T + \sigma W_T \right) $$
This guarantees that $S_T$ will always be positive if $S_0$ is positive.

---

## Simulation Code

This repository includes a Jupyter Notebook (`Brownian_Motion_Cricket.ipynb`) that implements Monte Carlo simulations for both SBM and GBM. The code:
* Sets up parameters for a hypothetical T20 cricket match.
* Runs thousands of simulations for a team's score progression.
* Visualizes the random paths generated by the models.
* Calculates the probability of reaching the target score based on the simulation outcomes.

---

## How to Use

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/sudiptakrsarkarai/gbm.git](https://github.com/sudiptakrsarkarai/gbm.git)
    cd gbm
    ```

2.  **Install dependencies:**
    ```bash
    pip install numpy matplotlib
    ```

3.  **Run the notebook:**
    Open and run the `Brownian_Motion_Cricket.ipynb` file in Jupyter Notebook or JupyterLab.

---

## Additional Notes

For more detailed mathematical notes, derivations, and background reading, please refer to the documents in the Google Drive folder linked below.

[**>> Google Drive for Additional Notes <<**](https://drive.google.com/file/d/1CWFWpqo71dDu0K53Xi_QUO2tpay50RgB/view?usp=sharing)
