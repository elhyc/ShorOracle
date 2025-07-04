# An implementation of the Shor oracle
This repository contains a qiskit implementation of the oracle required for the quantum phase estimation part of Shor's algorithm (which is the "quantum part" of Shor's algorithm). It is based on the paper https://arxiv.org/abs/quant-ph/0205095. The circuit construction uses a modest number of ancilla qubits, at the cost of possibly having longer circuit depth.

Given two inputs, $a,N \in \mathbb{Z}_{+}$, such that $a < N$, we construct a quantum circuit which implements the oracle:

$$
U | x \rangle_{1} | y \rangle_{n}  = \begin{cases}  | x \rangle_{1} | (a y) \text{ mod } N \rangle_{n} \text{, if } x = 1 \text{ and } y < N \\ 

|x \rangle_{1} |  y \rangle_{n} \text{, otherwise }

\end{cases}
$$

We note that this oracle should not have any effect if $y \geq  N$. With this extra condition, we will need to use an extra ancilla qubit in our construction to help keep track of the combination of the two conditions $x = 1$ <b> and </b> $ y < N$. That is, if both $x = 1$ and $y < N$ are true, then this extra ancilla will be in the $ | 1 \rangle$ state. Otherwise, the exta ancilla qubit will remain $| 0 \rangle$.


If we pass in a state $| x \rangle_{1} | y \rangle_{n}$, such that $x = 1$ and $y < N$, then the output of our quantum circuit before cleaning ancillas, will be of the following form:

$$
| x \rangle_{1} \otimes | 0 \rangle_{1}  \otimes | \text{ extra ancilla } \rangle_{1} \otimes |   (a y) \text{ mod } N \rangle_{n} \otimes | 0 \rangle_{n}  =  | x \rangle_{1} \otimes | 0 \rangle_{1}  \otimes | 1 \rangle_{1} \otimes |   (a y) \text{ mod } N \rangle_{n} \otimes | 0 \rangle_{n}
$$

If either $x \neq 1$ or $y \geq  N$, then the output of our quantum circuit will be of the form:

$$
| x \rangle_{1} \otimes | 0 \rangle_{1}  \otimes | \text{ extra ancilla } \rangle_{1} \otimes |   y \rangle_{n} \otimes | 0 \rangle_{n}  =  | x \rangle_{1} \otimes | 0 \rangle_{1}  \otimes | 0 \rangle_{1} \otimes |  y \rangle_{n} \otimes | 0 \rangle_{n}
$$

In the end, we will be able to produce a construction with "clean ancillas". That is, the final output will be of the form:

$$ |x \rangle_{1} | y \rangle_{n} \mapsto 
 \begin{cases}  | x \rangle_{1}  \otimes | 0 \rangle_{1} \otimes | 0 \rangle_{1} \otimes  | (a y) \text{ mod } N \rangle_{n} \otimes | 0 \rangle_{n} \text{, if } x = 1 \text{ and } y < N \\ 

| x \rangle_{1}  \otimes | 0 \rangle_{1} \otimes | 0 \rangle_{1} \otimes  | y \rangle_{n} \otimes | 0 \rangle_{n}  \text{, otherwise }
\end{cases}
$$

The construction presented in the [notebook](ShorOracleMiniProject.ipynb) works for arbitrary $N$

