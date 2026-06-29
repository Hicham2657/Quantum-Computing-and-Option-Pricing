# Quantum Computing and Option Pricing

A project completed for the Quantum Computing and Option Pricing course taught by Mnacho Echenim at Ensimag (2026).

This repository contains a Qiskit implementation that prices a European call option using the Quantum Amplitude Estimation (QAE) algorithm, built entirely from the postulates of quantum mechanics introduced in the lectures. The notebook is benchmarked against the classical Black-Scholes formula and a classical Monte Carlo simulation.

---

### Course Overview


The goal of this class is twofold:

- First, present some of the fundamental notions of quantum computing, including the postulates of quantum mechanics, the circuit representation of quantum algorithms and some results on quantum advantages.
- Second, investigate how quantum computing can be applied to solve a concrete problem, specifically the pricing of a financial option.
At the end of the class, students should be familiar with core concepts of quantum computing as well as the **Qiskit** software development kit, and have a clearer view of the potential and current limits of quantum computing.

The course is split into four lectures. Each lecture builds directly on the previous one:

[**Lecture 1**](./lecture1.pdf): Postulates of quantum mechanics

[**Lecture 2**](./lecture2.pdf): Quantum computing

[**Lecture 3**:](./lecture3.pdf) Quantum pricing I

[**Lecture 4**:](./lecture4.pdf) Quantum pricing II

---

### The project

The goal of this project is to design a quantum circuit that leverages the Quantum Amplitude Estimation algorithm to approximate the price of a call option. More precisely, given a call option with strike $K$ and maturity $T$ on an underlying $S$ with volatility $\sigma$, the goal of this project is to design a circuit that constructs a state of the form $$\sqrt{1-a}\left(|\Psi_0\rangle_n \otimes |1\rangle\right) + \sqrt{a}\left(|\Psi_1\rangle_n \otimes |1\rangle\right)$$
such that $a$ is an approximation of $\mathbb{E}\left[(S_T - K)_+\right]$. The Quantum Estimation Algorithm is then used to estimate the value of $a$, which represents the compounded price of the call option.

The notebook [```quantum_pricing.ipynb```](./quantum_pricing.ipynb) implements the full pipeline described in lectures 3 and 4.


**1. Classical baseline**

- Closed-form Black-Scholes price of a European call
- Classical Monte Carlo estimator of the (compounded) price, for comparison


**2. Encoding a probability distribution**

- The log-normal distribution of the terminal asset price $S_T$ is discretized onto a grid of $2^n$ points
- A circuit of cascaded controlled Ry rotations exactly mirroring the recursive construction from Lecture 3


**3. Encoding the payoff**

- The call payoff $(S_T − K)_+$ is rewritten as a piecewise linear function and approximated through a sine squared encoding (Lecture 4)
- A comparator circuit flags whether the discretized price index is above or below the strike


**4. Putting it all together**

- The distribution circuit, the comparator, and the two affine-function circuits are composed into a single circuit (```qc_pricer```)
- This estimate is rescaled to recover the expected (undiscounted) payoff, and compared against the classical Black-Scholes value


### Result

With only 7 iterations of amplitude estimation, the pipeline reaches a relative error under 0.3%: a level of accuracy that would require several thousand classical Monte Carlo samples to match, concretely illustrating the quadratic speedup of QAE over classical sampling
