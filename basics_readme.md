
# Quantum Circuit Basics: Building and Extending Circuits with Qiskit

This example walks through fundamental quantum operations and circuit construction using **Qiskit**. The code demonstrates operations on single qubits, multi-qubit systems, barrier operations, measurements, and how to extend circuits dynamically.

---

## Step-by-Step Guide

### Step 1: Initialize Quantum and Classical Registers
- A **Quantum Register** (`qr`) with 2 qubits and a **Classical Register** (`cr`) with 2 classical bits are initialized.
- These registers are then combined into a `QuantumCircuit`.

```python
from qiskit import QuantumCircuit, QuantumRegister, ClassicalRegister

qr = QuantumRegister(2, 'qr')  # Quantum Register with 2 qubits
cr = ClassicalRegister(2, 'c') # Classical Register with 2 bits

circuit = QuantumCircuit(qr, cr) # Combining into a Quantum Circuit
```

---

### Step 2: Single Qubit Operations
- Apply single-qubit gates to individual qubits:
  - **Hadamard Gate** (`H`) on the first qubit to create a superposition.
  - **Pauli-X Gate** (`X`) on the second qubit to flip its state.

```python
circuit.h(qr[0]) # Hadamard Gate
circuit.x(qr[1]) # Pauli-X Gate
```

---

### Step 3: Multi-Qubit Operations
- Use a **CNOT Gate** (`CX`) to create entanglement between the first and second qubits.

```python
circuit.cx(qr[0], qr[1]) # Controlled-NOT Gate
```

---

### Step 4: Barrier Operation
- Insert a **barrier** to separate operations visually and logically in the circuit. This step does not affect computation but is useful for clarity.

```python
circuit.barrier()
```

---

### Step 5: Measurement
- Measure the quantum states of the qubits and store the results in the classical register.

```python
circuit.measure(qr, cr)
```

---

### Step 6: Return the Circuit Depth
- The **depth** of a quantum circuit represents the number of layers of gates. This metric is crucial for assessing the complexity and execution time of a circuit.

```python
print(f'Circuit Depth: {circuit.depth()}')
```

---

### Step 7: Extend the Circuit Dynamically
- Add another quantum register (`qr2`) with one qubit to the existing circuit.
- Apply a Hadamard gate (`H`) to the new qubit.

```python
qr2 = QuantumRegister(1, 'q2') # Adding a new Quantum Register
circuit.add_register(qr2)      # Extending the circuit
circuit.h(qr2[0])             # Applying a Hadamard Gate
```

---

### Step 8: Visualize the Circuit
- Use `draw()` to view the final circuit structure.

```python
print(circuit.draw())
```

The resulting circuit looks like this:

```
      ┌───┐     ░ ┌─┐      
qr_0: ┤ H ├──■──░─┤M├──────
      └───┘┌─┴─┐ ░ └╥┘┌───┐
qr_1: ──X──┤ X ├─░──╫─┤ H ├
           └───┘ ░  ║ └───┘
q2_0: ───────────░──╫──────
                 ░  ║      
 c: 2/══════════════╩══════
                    0      
```

---

## Summary of Operations
1. **Single-Qubit Gates**: Introduce superposition and state flipping.
2. **Multi-Qubit Gates**: Create entanglement between qubits.
3. **Barrier**: Logical separation for better understanding.
4. **Measurement**: Extract classical information from quantum states.
5. **Circuit Depth**: Evaluate circuit complexity.
6. **Circuit Extension**: Dynamically add new qubits and operations.


--- 

Feel free to adapt this example for your quantum experiments! 🚀
