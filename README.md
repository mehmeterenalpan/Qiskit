
## Quantum "Hello World" Example: Bell State and n-Qubit GHZ State

This code demonstrates the creation and measurement of quantum states using **Qiskit**, specifically:

1. **2-qubit Bell State** (a fundamental entangled quantum state)
2. **n-qubit GHZ State** (generalized entanglement across multiple qubits)

---

## Part I: Bell State

### Step 1: Map the problem to circuits and operators
- **Quantum Circuit**: A simple 2-qubit circuit is created.
    - A Hadamard gate (`H`) is applied to the first qubit to create a superposition.
    - A controlled-X gate (`CX`) entangles the first and second qubits.

```python
from qiskit import QuantumCircuit

qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)

qc.draw(output='mpl')
```
- The resulting **Bell state** is:
    \[
    |\text{Bell}\rangle = \frac{1}{\sqrt{2}} (|00\rangle + |11\rangle)
    \]

- **Observables**: Six Pauli operators (`ZZ`, `ZI`, `IZ`, `XX`, `XI`, `IX`) are defined to analyze the circuit.

```python
from qiskit.quantum_info import Pauli
ZZ = Pauli('ZZ')
ZI = Pauli('ZI')
IZ = Pauli('IZ')
XX = Pauli('XX')
XI = Pauli('XI')
IX = Pauli('IX')
observables = [ZZ, ZI, IZ, XX, XI, IX]
```

---

### Step 2 & 3: Optimize and Execute
- The circuit is executed using Qiskit's **Estimator**, which computes expectation values for the defined observables.

```python
from qiskit_aer.primitives import Estimator

estimator = Estimator()
job = estimator.run([qc] * len(observables), observables)
job.result()
```

---

### Step 4: Post-Processing and Plotting
- The measured expectation values are plotted for visualization.

```python
import matplotlib.pyplot as plt

data = ['XX', 'ZI', 'IZ', 'ZZ', 'XI', 'IX']
values = job.result().values

plt.plot(data, values, '-o')
plt.xlabel('Observable')
plt.ylabel('Expectation Value')
plt.show()
```

---

## Part II: n-Qubit GHZ State

### Step 1: Map the Problem to Circuits and Operators
- **n-qubit GHZ State**: A generalization of the Bell state to `n` qubits:
    - A Hadamard gate is applied to the first qubit.
    - A chain of controlled-X gates (`CX`) entangles all subsequent qubits.

```python
def get_qc_for_n_qubit_GHZ_state(n):
    qc = QuantumCircuit(n)
    qc.h(0)
    for i in range(n-1):
        qc.cx(i, i+1)
    return qc

n = 10
gc = get_qc_for_n_qubit_GHZ_state(n)
gc.draw(output='mpl')
```

- **Operators**: A series of `ZZ` operators are constructed to measure pairwise correlations between qubits.

```python
from qiskit.quantum_info import SparsePauliOp

operator_strings = ['Z' + 'I' * i + 'Z' + 'I' * (n-2-i) for i in range(n-1)]
operators = [SparsePauliOp(operator_strings) for operator_strings in operator_strings]
```

---

### Step 2: Optimize the Problem for Quantum Execution
- The circuit is **transpiled** for an IBM Quantum backend to improve execution efficiency.

```python
from qiskit_ibm_runtime import QiskitRuntimeService
from qiskit.transpiler.preset_passmanagers import generate_preset_pass_manager

service = QiskitRuntimeService()
backend_name = "ibm_brisbane"
backend = service.backend(backend_name)
pm = generate_preset_pass_manager(backend=backend, optimization_level=1)

qc_transpiled = pm.run(gc)
operator_transpiled_list = [op.apply_layout(qc_transpiled.layout) for op in operators]
```

---

### Step 3: Execute on the Backend
- The **EstimatorV2** executes the transpiled circuit and measures the defined operators.

```python
from qiskit_ibm_runtime import EstimatorV2 as Estimator, EstimatorOptions

options = EstimatorOptions()
estimator = Estimator(backend, options=options)

job = estimator.run([(qc_transpiled, operator_transpiled_list)])
job_id = job.job_id()
print(job_id)
```
- The `job_id` allows you to monitor progress on your IBM Quantum dashboard.

---

### Step 4: Post-Processing and Plotting
- Correlations between qubits are normalized and plotted.

```python
import matplotlib.pyplot as plt

job_id = 'cxgwbcd6t010008cr7yg'
job = service.job(job_id)

data = list(range(1, len(operators)+1))
result = job.result()[0]
values = result.data.evs
values = [v / values[0] for v in values]

plt.scatter(data, values, marker='o', label='100-qubit GHZ state')
plt.xlabel('Distance between qubits $i$')
plt.ylabel(r'$\langle Z_0 Z_i \rangle / \langle Z_0 Z_1 \rangle$')
plt.legend()
plt.show()
```

---

## Explanation
1. **Part I** introduces the concept of entanglement via the simple Bell state, where expectation values for various observables are computed and visualized.
2. **Part II** generalizes this to the **n-qubit GHZ state**, demonstrating entanglement across multiple qubits and pairwise correlations.

---

### Results:
- The Bell state demonstrates quantum entanglement between two qubits.
- The GHZ state extends this concept to multiple qubits, showcasing quantum correlations at increasing distances.

---

Feel free to clone, extend, and execute this code on your preferred IBM Quantum backend! 🚀

---
