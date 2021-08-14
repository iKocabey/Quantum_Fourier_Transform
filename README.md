# Quantum_Fourier_Transform
We apply quantum fourier transform to qubits

from qiskit.circuit.library import QFT
from qiskit.quantum_info import Statevector
from qiskit import QuantumCircuit
from qiskit.visualization import plot_bloch_multivector
import warnings
import numpy as np
warnings.filterwarnings('ignore')


pi=np.pi
def myQFT(nqubits):
    myQFT_circuit=QuantumCircuit(nqubits)
    for qubit in range(nqubits):
        myQFT_circuit.h(qubit)
        for otherqubit in range(qubit+1,nqubits):
            myQFT_circuit.cu1(pi/(2**(otherqubit-qubit)),otherqubit,qubit)
    return myQFT_circuit
    
    
display(myQFT(4).draw())
display(QFT(4).draw())


state='001'
mycircuit=QuantumCircuit(len(state))
mycircuit.initialize(Statevector.from_label(state).data,mycircuit.qubits[::-1])
print('==Computational basis==')
display(plot_bloch_multivector(Statevector.from_instruction(mycircuit).data))
print('==Fourier basis==')
mycircuit.append(myQFT(len(state)),mycircuit.qubits)
display(plot_bloch_multivector(Statevector.from_instruction(mycircuit).data))  
