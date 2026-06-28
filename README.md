# Quantum Computing and Option Pricing

A project completed for the Quantum Computing and Option Pricing course taught by Mnacho Echenim at Ensimag (2026).

This repository contains a Qiskit implementation that prices a European call option using the Quantum Amplitude Estimation (QAE) algorithm, built entirely from the postulates of quantum mechanics introduced in the lectures. The notebook is benchmarked against the classical Black-Scholes formula and a classical Monte Carlo simulation.

---

### Course Overview

The course is split into four lectures. Each lecture builds directly on the previous one:

**Lecture 1**: Postulates of quantum mechanics

**Lecture 2**: Quantum computing

**Lecture 3**:Quantum pricing (I)

**Lecture 4**: Quantum pricing (II)

---

### The project

The notebook ```quantum_pricing.ipynb``` implements the full pipeline described in lectures 3 and 4, structured as follows:


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
