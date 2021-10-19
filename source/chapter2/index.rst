2 Advanced Quantum programming
===============

2.1 Quantum logic gate
----------------------

In classical computing, bit is the most basic unit and logic gate is the
most basic control mode. We can control the circuit through combination
of logic gates. Similarly, qubit is processed by the quantum logic gate.
We consciously evolve quantum states by using quantum logic gates.
Therefore, the quantum logic gate is the basis of the quantum algorithm.

The quantum logic gate is denoted with unitary matrix. The most common
quantum logic gates operate on one or two qubits, just as the common
classical logic gates operate on one or two bits.

2.1.1 Matrix form of common quantum logic gates
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

2.1.1.1 Single-qubit quantum logic gate
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. |I| image::  /images/2.1.png
   :width: 50px
   :height: 50px

.. |H| image:: /images/2.2.png
   :width: 50px
   :height: 50px

.. |T| image:: /images/2.3.png
   :width: 50px
   :height: 50px

.. |S| image:: /images/2.4.png
   :width: 50px
   :height: 50px

.. |X| image:: /images/2.5.png
   :width: 50px
   :height: 50px

.. |Y| image:: /images/2.6.png
   :width: 50px
   :height: 50px
   
.. |Z| image:: /images/2.7.png
   :width: 50px
   :height: 50px

.. |X1| image:: /images/2.8.png
   :width: 50px
   :height: 50px

.. |Y1| image:: /images/2.9.png
   :width: 50px
   :height: 50px
   
.. |Z1| image:: /images/2.10.png
   :width: 50px
   :height: 50px

.. |RX| image:: /images/2.11.png
   :width: 50px
   :height: 50px

.. |RY| image:: /images/2.12.png
   :width: 50px
   :height: 50px

.. |RZ| image:: /images/2.13.png
   :width: 50px
   :height: 50px

.. |U1| image:: /images/2.14.png
   :width: 50px
   :height: 50px

.. |U2| image:: /images/2.15.png
   :width: 50px
   :height: 50px

.. |U3| image:: /images/2.16.png
   :width: 50px
   :height: 50px

.. |U4| image:: /images/2.17.png
   :width: 50px
   :height: 50px

.. |CNOT| image:: /images/2.18.png
   :width: 50px
   :height: 50px

.. |CR| image:: /images/2.19.png
   :width: 50px
   :height: 50px

.. |iSWAP| image:: /images/2.20.png
   :width: 50px
   :height: 50px

.. |SWAP| image:: /images/2.21.png
   :width: 50px
   :height: 50px

.. |CZ| image:: /images/2.22.png
   :width: 50px
   :height: 50px

.. |CU| image:: /images/2.23.png
   :width: 50px
   :height: 50px

.. |Toffoli| image:: /images/2.24.png
   :width: 50px
   :height: 50px


======================================================== ======================= =========================================================================================================================================================================
| |I|                                                     | ``I``                     | :math:`\begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}\quad`
| |H|                                                     | ``Hadamard``              | :math:`\begin{bmatrix} 1/\sqrt {2} & 1/\sqrt {2} \\ 1/\sqrt {2} & -1/\sqrt {2} \end{bmatrix}\quad`
| |T|                                                     | ``T``                     | :math:`\begin{bmatrix} 1 & 0 \\ 0 & \exp(i\pi / 4) \end{bmatrix}\quad`
| |S|                                                     | ``S``                     | :math:`\begin{bmatrix} 1 & 0 \\ 0 & 1i \end{bmatrix}\quad`
| |X|                                                     | ``Pauli-X``               | :math:`\begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}\quad`
| |Y|                                                     | ``Pauli-Y``               | :math:`\begin{bmatrix} 0 & -1i \\ 1i & 0 \end{bmatrix}\quad`
| |Z|                                                     | ``Pauli-Z``               | :math:`\begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}\quad`
| |X1|                                                    | ``X1``                    | :math:`\begin{bmatrix} 1/\sqrt {2} & -1i/\sqrt {2} \\ -1i/\sqrt {2} & 1/\sqrt {2} \end{bmatrix}\quad`
| |Y1|                                                    | ``Y1``                    | :math:`\begin{bmatrix} 1/\sqrt {2} & -1/\sqrt {2} \\ 1/\sqrt {2} & 1/\sqrt {2} \end{bmatrix}\quad`
| |Z1|                                                    | ``Z1``                    | :math:`\begin{bmatrix} \exp(-i\pi/4) & 0 \\ 0 & \exp(i\pi/4) \end{bmatrix}\quad`
| |RX|                                                    | ``RX``                    | :math:`\begin{bmatrix} \cos(\theta/2) & -1i×\sin(\theta/2) \\ -1i×\sin(\theta/2) & \cos(\theta/2) \end{bmatrix}\quad`
| |RY|                                                    | ``RY``                    | :math:`\begin{bmatrix} \cos(\theta/2) & -\sin(\theta/2) \\ \sin(\theta/2) & \cos(\theta/2) \end{bmatrix}\quad`
| |RZ|                                                    | ``RZ``                    | :math:`\begin{bmatrix} \exp(-i\theta/2) & 0 \\ 0 & \exp(i\theta/2) \end{bmatrix}\quad`
| |U1|                                                    | ``U1``                    | :math:`\begin{bmatrix} 1 & 0 \\ 0 & \exp(i\theta) \end{bmatrix}\quad`
| |U2|                                                    | ``U2``                    | :math:`\begin{bmatrix} 1/\sqrt {2} & -\exp(i\lambda)/\sqrt {2} \\ \exp(i\phi)/\sqrt {2} & \exp(i\lambda+i\phi)/\sqrt {2} \end{bmatrix}\quad`
| |U3|                                                    | ``U3``                    | :math:`\begin{bmatrix} \cos(\theta/2) & -\exp(i\lambda)×\sin(\theta/2) \\ \exp(i\phi)×\sin(\theta/2) & \exp(i\lambda+i\phi)×\cos(\theta/2) \end{bmatrix}\quad`
| |U4|                                                    | ``U4``                    | :math:`\begin{bmatrix} u0 & u1 \\ u2 & u3 \end{bmatrix}\quad`
======================================================== ======================= =========================================================================================================================================================================


2.1.1.2 Multi-qubit quantum logic gates
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

============================================================ =========================== ========================================================================================================
| |CNOT|                                                      | ``CNOT``                  | :math:`\begin{bmatrix} 1 & 0 & 0 & 0  \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & 1 & 0 \end{bmatrix}\quad`
| |CR|                                                        | ``CR``                    | :math:`\begin{bmatrix} 1 & 0 & 0 & 0  \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & \exp(i\theta) \end{bmatrix}\quad`
| |iSWAP|                                                     | ``iSWAP``                 | :math:`\begin{bmatrix} 1 & 0 & 0 & 0  \\ 0 & \cos(\theta) & -i×\sin(\theta) & 0 \\ 0 & -i×\sin(\theta) & \cos(\theta) & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}\quad`
| |SWAP|                                                      | ``SWAP``                  | :math:`\begin{bmatrix} 1 & 0 & 0 & 0  \\ 0 & 0 & 1 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}\quad`
| |CZ|                                                        | ``CZ``                    | :math:`\begin{bmatrix} 1 & 0 & 0 & 0  \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & -1 \end{bmatrix}\quad`
| |CU|                                                        | ``CU``                    | :math:`\begin{bmatrix} 1 & 0 & 0 & 0  \\ 0 & 1 & 0 & 0 \\ 0 & 0 & u0 & u1 \\ 0 & 0 & u2 & u3 \end{bmatrix}\quad`
| |Toffoli|                                                   | ``Toffoli``               | :math:`\begin{bmatrix} 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 \\ 0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 \\ 0 & 0 & 0 & 0 & 1 & 0 & 0 & 0  \\ 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1  \\ 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 \\ \end{bmatrix}\quad`
============================================================ =========================== ========================================================================================================

PyQPanda encapsulates all quantum logic gates as API for the use by
users, and can obtain the return value of QGate type. For example, if
using Hadamard gate, you can obtain it by the following ways:

::

    from pyqpanda import *
    import numpy as np
    init(QMachineType.CPU)
    qubits = qAlloc_many(4)
    h = H(qubits[0])

The parameter is the target qubit and the return value is the quantum
logic gate.

Single-gate without any angle, supported by pyqpanda includes: ``I``, ``H``, ``T``,
``S``, ``X``, ``Y``, ``Z``, ``X1``, ``Y1``, and ``Z1`` .

The application for qubit will be introduced in the part of Quantum
simulator.

Single-gate includes logic gate with a rotation angle, and RX gate is
taken as an example:

::

    rx = RX(qubits[0]，np.pi/3)

The first parameter is target qubit and the second parameter is rotation
angle.

Single-gate supported by pyqpanda includes logic gate with a rotation
angle: ``RX``, ``RY``, ``RZ``, ``U1`` and ``P`` .

``U2``, ``U3`` and ``U4`` gates are supported by pyqpanda and used as follows:

::

    # U2(qubit, phi, lambda)  There are two angles
    u2 = U2(qubits[0]，np.pi, np.pi/2)

    # U3(qubit, theta, phi, lambda) There are three angles
    u3 = U3(qubits[0]，np.pi, np.pi/2, np.pi/4)

    # U4(qubit, alpha, beta, gamma, delta) There are four angles
    u4 = U4(qubits[0]，np.pi, np.pi/2, np.pi/4, np.pi/2)

Except that input parameters are different, two-qubits quantum logic
gate and single-qubit quantum logic gate are used by the same measure,
and CNOT gate is taken as an example:

::

    cnot = CNOT(qubits[0]，qubits[1])

The first parameter is control qubit and the second parameter is target
qubit.

.. admonition:: Note

    two qubits are different. 

The double logic gate not provided with angle and supported by pyqpanda
includes ``CNOT``, ``CZ``, ``SWAP``, ``iSWAP``, and ``SqiSWAP``.

Except that input parameters are different, two-qubits quantum logic
gate and single-qubit quantum logic gate are used by the same measure,
and CNOT gate is taken as an example:

::

    cnot = CNOT(control_qubit, target_qubit)

CNOT gate receives two parameters, in which the first parameter is
control qubit and the second parameter is target qubit.

Double logic gate supported by pyqpanda and provided with rotation angle
includes CR, CU and CP.

Double-gate with rotation angle, for example, CR gate:

::

    cr = CR(qubits[0]，qubits[1]，np.pi)

The first parameter is control qubit, the second parameter is target
qubit, and the third parameter is the rotation angle.

The CU gate is supported and used by the following measure:

::

    # CU(control, target, alpha, beta, gamma, delta) There are four angles
    cu = CU(qubits[0]，qubits[1]，np.pi,np.pi/2,np.pi/3,np.pi/4)

Three-qubit quantum logic gate ``Toffoli`` is obtained by the measure:

::

    toffoli = Toffoli(qubits[0], qubits[1], qubits[2]) 

In practice, three-qubit quantum logic gate Toffoli is CCNOT gate, and
the first two parameters are control qubits and the final parameter is
target qubit.

Pyqpanda also supports to add the qubit array into the quantum logic
gate, that is, all qubits in the array are computed by endowing the same
logic gate, and single-gate H is taken as an example:

::

    # Returns a quantum circuit
    circuit = H(Qvec);

Here, Qvec is the array to store qubits. When multiple gates are added
with arrays, multiple corresponding arrays are correspondingly imported
and the logic gates are computed according to the subscript sequence of
the arrays.

2.1.2 Interface introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

As mentioned at the beginning of this chapter, all quantum logic gates
are of the unitary matrix, so you can also perform transposed conjugate
on quantum logic gates and obtain a quantum logic gate following the
quantum logic gate ``dagger`` by using the following measure:

::

    rx_dagger = RX(qubits[0], np.pi).dagger()

Or:

::

    rx_dagger = RX(qubits[0], np.pi)
    rx_dagger.set_dagger(true)

The quantum logic gate can be added with control qubit, where the
quantum logic gate controlled by a quantum logic gate may be added by
the following measures:

::

    qvec = [qubits[0], qubits[1]]
    rx_control = RX(qubits[2], np.pi).control(qvec)

Or:

::

    qvec = [qubits[0], qubits[1]]
    rx_control = RX(qubits[2], np.pi)
    rx_control.set_control(qvec)

pyqpanda also encapsulates some convenient interfaces to simply the
operations of some quantum logic gates.

::

    cir = apply_QGate(qubits, H)

All qubits are added with H gate

2.1.3 Example
~~~~~~~~~~~~~

The following example mainly demonstrates how to use QGate interface

::

    from pyqpanda import *

    if __name__ == "__main__":
       init(QMachineType.CPU)
       qubits = qAlloc_many(3)
       control_qubits = [qubits[0], qubits[1]]
       prog = create_empty_qprog()

       # Building quantum programs
       prog  << H(qubits) \
             << H(qubits[0]).dagger() \
             << X(qubits[2]).control(control_qubits)

       # Perform probability measurements on quantum programs
       result = prob_run_dict(prog, qubits, -1)

       # Print measurement results
       print(result)
       finalize()

The computing results are as follows:

::

    {'000': 0.24999999999999295, '001': 0.0, '010': 0.24999999999999295, '011': 0.0, '100': 0.24999999999999295, '101': 0.0, '110': 0.24999999999999295, '111': 0.0}

2.2 Quantum circuit
-------------------

Quantum circuit, also called as quantum logic circuit, is the most
common quantum computing model, which represents a circuit to operate
qubits in an abstract concept. The quantum circuit is composed of
qubits, circuits (timelines), and logic gates. Quantum measurement
results often require to be read out.

Distinguished from a traditional circuit that is connected by metal
wires to transmit voltage or current signals, the quantum circuit is
connected by timeline, that is, the natural evolution in qubit state
occurs along with time, and follows the instructions of Hamiltonian
operators until they are operated by logic gates.

Each quantum logic gate to constitute a quantum circuit is a ``unitary
operator``, and therefore the entire quantum circuit is also a large
unitary operator.

2.2.1 Quantum algorithm circuit diagram
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In the current theoretical study of quantum computing, the quantum
algorithms are commonly indicated by quantum circuits, such as the
quantum circuit diagram of ``HHL algorithm`` listed below. 

.. figure::  /images/2.25.png
   :alt:

2.2.2 Interface introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In pyQPanda, QCircuit is a container type and only carries quantum logic
gates. Besides, it is also a type of QNode. One QCircuit object can be
initialized by the following two measures:

::

    cir = QCircuit()

Or:

::

    cir = create_empty_circuit()

You can populate nodes into the end of QCircuit with the following ways,
in which pyqpanda reloads << operators as a way to insert the quantum
circuit.

::

    cir << node

Node may be either QGate or QCircuit.

We can also obtion the quantum circuit after the transposed conjugate of
QCircuit by the following way:

::

    cir_dagger = cir.dagger()

If you want to replicate the current quantum circuit and add control
qubits to the replicated quantum circuit, the following ways can be
adopted:

::

    qvec = [qubits[0], qubits[1]]
    cir_control = cir.control(qvec)


.. admonition:: Note

    ●  No error is reported when QPorg, QIf and Measure are inserted into QCircuit, but unexpected errors may occur during the operation.
    
    ●  One constituted QCircuit cannot directly conduct quantum computing and simulation, which requires to be further constituted into QProg.

2.2.3 Example
~~~~~~~~~~~~~

::

    from pyqpanda import *

    if __name__ == "__main__":

        init(QMachineType.CPU)
        qubits = qAlloc_many(4)
        cbits = cAlloc_many(4)

        # Building quantum programs
        prog = QProg()
        circuit = create_empty_circuit()

        circuit << H(qubits[0]) \
                << CNOT(qubits[0], qubits[1]) \
                << CNOT(qubits[1], qubits[2]) \
                << CNOT(qubits[2], qubits[3])

        prog << circuit << Measure(qubits[0], cbits[0])

        # The quantum program runs 1000 times and returns the measurement
        result = run_with_configuration(prog, cbits, 1000)

        # The number of times the printed quantum state appears in the result 
    # of multiple runs of the quantum program
        print(result)
        finalize()

Running results:

::

    {'0000': 486, '0001': 514}

2.3 QWhile
----------

The operations are controlled circularly by the quantum program; the
input parameters are considered as the conditional judgment expression,
with the function to execute the while loop operation.

2.3.1 Interface introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In pyQPanda, QWhileProg is used to execute the while loop operation of
the quantum program, and also regarded as a type of QNode; one
QWhileProg object can be initialized by the following two measures:

::

    qwile = QWhileProg(ClassicalCondition, QNode)

Or

::

    qwile = create_while_prog(ClassicalCondition, QNode)

The abovementioned functions need to have two types of parameters,
namely quantum expression of ClassicalCondition and QNode, where
passable QNode includes QProg, QCircuit, QGate, QWhileProg, QIfProg, and
QMeasure.

2.3.2 Example
~~~~~~~~~~~~~

::

    from pyqpanda import *

    if __name__ == "__main__":

        init(QMachineType.CPU)
        qubits = qAlloc_many(3)
        cbits = cAlloc_many(3)
        cbits[0].set_val(0)
        cbits[1].set_val(1)

        prog = QProg()
        prog_while = QProg()

        # Build the loop branch of QWhile
        prog_while << H(qubits[0]) << H(qubits[1])<< H(qubits[2])\
                << assign(cbits[0], cbits[0] + 1)\
    << Measure(qubits[1], cbits[1])

        # Build a QWhile
        qwhile = create_while_prog(cbits[1], prog_while)

        # QWhile is inserted into the quantum program
        prog << qwhile

        # Run and print the measurement results
        result = directly_run(prog)
        print(result)
        print(cbits[0].get_val())
        finalize()

Running results:

::

    2
    {'c1': False}

2.4 QIf
-------

QIf refers to the conditional judgment of quantum program; the input
parameters are considered as the conditional judgment expression, with
the function to execute the conditional judgment.

2.4.1 Interface introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In pyQPanda, QIfProg is used to execute the conditional judgment of
quantum program, and also regarded as a type of QNode; one QIfProg
object can be initialized by the following two measures:

::

    qif = QIfProg(ClassicalCondition, QNode)
    qif = QIfProg(ClassicalCondition, QNode, QNode)

Or

::

    qif = create_if_prog(ClassicalCondition, QNode)
    qif = create_if_prog(ClassicalCondition, QNode, QNode)

The mentioned function needs to have two types of parameters, namely
quantum expression of ClassicalCondition and QNode. When one QNode
parameter passes, QNode is correct branch node. When two QNode
parameters pass, the first parameter is a correct branch node and the
second parameter is a wrong branch node. Passable QNode includes QProg,
QCircuit, QGate, QWhileProg, QIfProg and QMeasure.

2.4.2 Example
~~~~~~~~~~~~~

::

    from pyqpanda import *

    if __name__ == "__main__":

        init(QMachineType.CPU)
        qubits = qAlloc_many(3)
        cbits = cAlloc_many(3)
        cbits[0].set_val(0)
        cbits[1].set_val(3)

        prog = QProg()
        branch_true = QProg()
        branch_false = QProg()

        # Build the right and wrong branches of QIf
        branch_true << H(qubits[0])<< H(qubits[1]) << H(qubits[2])
    branch_false << H(qubits[0]) << CNOT(qubits[0], qubits[1])\
     << CNOT(qubits[1], qubits[2])

        # Build QIf
        qif = create_if_prog(cbits[0] > cbits[1], branch_true, branch_false)

        # QIf is inserted into the quantum program
        prog << qif

    # Probability measurement, and returns the probability measurement
    # result of the target qubit, subscript decimal
        result = prob_run_tuple_list(prog, qubits, -1)

        # Print the probability measurement results
        print(result)
        finalize()

Running results:

::

    [(0, 0.4999999999999999), (7, 0.4999999999999999), (1, 0.0), (2, 0.0), (3, 0.0), (4, 0.0), (5, 0.0), (6, 0.0)]

2.5 Quantum program
-------------------

For the compilation and construction of the quantum program, the quantum
program is designed. Generally, it can be understood as an operation
sequence. As the quantum algorithm also includes classical computing, so
the industrial assumption is that the quantum computer in the immediate
future is a hybrid structure and composed of two parts: one is the
classical computer for classical computing and control; and the other is
the quantum equipment for quantum computing. pyQPanda considers the
programming procedure of quantum program as a part of classical program
running, and the entire peripheral host program must contain the part of
quantum program creation.

2.5.1 Interface introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In pyQPanda, QProg is a container type of quantum programming and the
highest unit of one quantum program. It is also a type of QNode; and one
QProg object is initialized by the following measures:

::

    prog = QProg()

Or

::

    prog = create_empty_qprog()

Quantum programs can also be constructed through the existing QNode, for
example:

::

    qubit = qAlloc()
    gate = H(qubit)
    prog = QProg(gate)  

The quantum programs of QCircuit, QGate, QWhileProg, QIfProg,
ClassicalCondition, and QMeasure may be constructed by the similar
measure.

You can populate nodes into the end of QProg with the following ways, in
which pyqpanda reloads << operators as a way to insert the quantum
circuit.

::

    prog << node

QNode includes QGate, QPorg, QIf, Measure and the like; and QProg
supports insertion of all types of QNode.

2.5.2 Example
~~~~~~~~~~~~~

::

    from pyqpanda import *

    if __name__ == "__main__":

        init(QMachineType.CPU)
        qubits = qAlloc_many(4)
        cbits = cAlloc_many(4)
        prog = QProg()

        # Building quantum programs
        prog << H(qubits[0]) \
             << X(qubits[1]) \
             << iSWAP(qubits[0], qubits[1]) \
             << CNOT(qubits[1], qubits[2]) \
             << H(qubits[3]) \
             << measure_all(qubits, cbits)

        # The quantum program runs 1000 times and returns the measurement
        result = run_with_configuration(prog, cbits, 1000)

        # The number of times the printed quantum state appears in the result 
    # of multiple runs of the quantum program
        print(result)
        finalize()

Running results:

::

    {'0001': 255, '0111': 253, '1001': 258, '1111': 247}

2.6 Quantum simulator
---------------------

Before an actual quantum computer is not constituted, the quantum
simulators are used to undertake the verification of quantum algorithms
and quantum applications. pyQPanda supports full-amplitude quantum
simulator, single-amplitude quantum simulator, partial-amplitude quantum
simulator and noise inclusive quantum simulator.

2.6.1 Full-amplitude quantum simulator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The full-amplitude quantum simulator can simulate all amplitudes of the
quantum state at one time, supports the computing ways such as CPU,
single-circuit computing and GPU, and performs configurations during the
initialization by the same way, but has different computing efficiency.

2.6.1.1 Interface introduction
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Type of full-amplitude quantum simulator:

::

    class QMachineType(__pybind11_builtins.pybind11_object):
    """
        Members:

        CPU

        GPU

        CPU_SINGLE_THREAD

        NOISE
    """

In pyQPanda, the quantum simulator is constructed by the following ways:

::

    init(QMachineType.CPU)  
    # With init, qvm is not returned and a global qvm is generated in the code
    auto qvm = init_quantum_machine(QMachineType.CPU) 
    # Get the quantum machine object through the interface
    qvm = CPUQVM() 
    # Create a new Quantum Machine object


.. admonition:: Note

   The functions of ``init`` and ``init_quantum_machine`` functions are not thread-safe and not suitable for multi-threaded programming, and the maximum number of qubits and the number of classical registers are both 25 by default.
  
The quantum simulator needs to be initialized after configuration:

::

    qvm.init_qvm()

.. admonition:: Note

   No initialization is required when ``init`` and ``init_quantum_machine``interfaces are invoked.
   

Now, we need to apply for qubits and classical registers.

Setting of the maximum number of qubits

::

    # Set the maximum number of qubits and the maximum number of classical
    # registers
    qvm.set_configure(30, 30)

.. admonition:: Note

  If this parameter not set, the maximum number of qubits is 29 by default.
   

For example, we apply for 4 qubits:

::

    qubits = qvm.qAlloc_many(4)

This interface can be used when a qubit is applied:

::

    qubit = qvm.qAlloc()

The classical register is also applied by the interface similar to that
to apply for the qubit; and the application method is identical to the
qubit application method, for example, the method of applying four
classical registers:

::

    cbits = qvm.cAlloc_many(4)

Such interface may be used when one classical register is applied:

::

    cbit = qvm.cAlloc()

In a quantum simulator, qubits or classical registers are applied
several times, and therefore we want to know how many qubits or
classical registers are applied by the following methods:

::

    num_qubit = qvm.get_allocate_qubit_num() # Number of qubits applied
    num_cbit = qvm.get_allocate_cmem_num() # Number of classical registers 
    # applied

How should we use simulator to execute quantum programs? The following
methods can be adopted:

::

    prog = QProg()
    prog << H(qubits[0]) << CNOT(qubits[0], qubits[1])\ 
    << Measure(qubits[0], cbits[0])
    result = qvm.directly_run(prog) # Execution of quantum program

If you want to run a quantum program and obtain the results of each
quantum program, we also provide an interface ``run_with_configuration`` ,
in addition to the recursive call of ``directly_run`` ; such interface has
two reloading methods as follows:

::

    result = qvm.run_with_configuration(prog, cbits, shots)

In a method, ``prog`` is a quantum program; ``cbits`` is ClassicalCondition
list; ``shots`` as reshaped data are the running frequency of the quantum
program.

::

    result = qvm.run_with_configuration(prog, cbits, config)

In another method, ``prog`` is a quantum program; ``cbits`` is
ClassicalCondition list; ``config`` is dictionary date as follows:

::

    config = {'shots': 1000}

If you want to obtain the amplitudes of various quantum states after the
quantum program runs, ``get_qstate`` function can be invoked:

::

    stat = qvm.get_qstate()

The measurement and probability usage of the quantum simulator is the
same as those introduced in quantum measurement and probability
measurement, which will not be described here.

2.6.1.2 Example 1
^^^^^^^^^^^^^^^^^

::

    from pyqpanda import *

    if __name__ == "__main__":
        qvm = CPUQVM()
        qvm.init_qvm()

        qvm.set_configure(29, 29)
        qubits = qvm.qAlloc_many(4)
        cbits = qvm.cAlloc_many(4)

        # Building a quantum program
        prog = QProg()
    prog << H(qubits[0]) << CNOT(qubits[0], qubits[1])\
    << Measure(qubits[0], cbits[0])

        # The quantum program runs 1000 times and returns the measurement
        result = qvm.run_with_configuration(prog, cbits, 1000)

    # Print the number of times the quantum state appears in the results of
    # multiple runs of the quantum program
        print(result)
        qvm.finalize()

Running results:

::

    {'0000': 481, '0001': 519}

.. admonition:: Note

   The computational results of the quantum program are indefinite, but the corresponding values of ``0000`` and ``0001`` should both be approximately 500.

   
For the ease of use, pyqpanda also encapsulates some process-oriented
interfaces, which have basically the same names and using methods as
those described above. We modify the above example into a
process-oriented interface as follows:

2.6.1.3 Example 2
^^^^^^^^^^^^^^^^^

::

    from pyqpanda import *

    if __name__ == "__main__":
        init(QMachineType.CPU)
        qubits = qAlloc_many(4)
        cbits = cAlloc_many(4)

        # Building a quantum program
        prog = QProg()
    prog << H(qubits[0]) << CNOT(qubits[0], qubits[1])\
     << Measure(qubits[0], cbits[0])

        # The quantum program runs 1000 times and returns the measurement
        result = run_with_configuration(prog, cbits, 1000)

    # Print the number of times the quantum state appears in the results of
    # multiple runs of the quantum program

        print(result)
        finalize()

Running results:

::

    {'0000': 484, '0001': 516}

2.6.2 Noise inclusive quantum simulator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Restricted by the physical characteristics of the qubits, the actual
quantum computer usually has the inevitable computing error. To better
simulate such error in the quantum simulator, pyQPanda provides a noise
inclusive quantum simulator on the basis of the quantum simulator. The
noise inclusive quantum simulator is closer to the actual quantum
computer in terms of simulation. We can customize the types of logic
gates supported and the noise model supported by logic gates. Through
the custom forms, the quantum programs developed by pyQPanda will be
widely applied in practice.

2.6.2.1 Introduction to noise models
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

(1) DAMPING\_KRAUS\_OPERATOR
''''''''''''''''''''''''''''''''''

DAMPING\_KRAUS\_OPERATOR is the relaxation process noise model of qubit.
Its kraus operator and representation are as shown below:

.. math::


   K_{1}=\left[\begin{array}{cc}
   1 & 0 \\
   0 & \sqrt{1-p}
   \end{array}\right], K_{2}=\left[\begin{array}{cc}
   0 & \sqrt{p} \\
   0 & 0
   \end{array}\right]

A noise parameter is required.

(2) DEPHASING\_KRAUS\_OPERATOR
''''''''''''''''''''''''''''''''''''

DEPHASING\_KRAUS\_OPERATOR is the dephasing process noise model of
qubit. Its kraus operator and representation are as shown below:

.. math::


   K_{1}=\left[\begin{array}{cc}
   \sqrt{1-p} & 0 \\
   0 & \sqrt{1-p}
   \end{array}\right], K_{2}=\left[\begin{array}{cc}
   \sqrt{p} & 0 \\
   0 & -\sqrt{p}
   \end{array}\right]

A noise parameter is required.

(3) DECOHERENCE\_KRAUS\_OPERATOR
''''''''''''''''''''''''''''''''''''''

DECOHERENCE\_KRAUS\_OPERATOR is the decoherence noise model and a
combination of the above two noise models, and their relation is as
shown below:

:math:`P_{\text {damping }}=1-e^{-\frac{t_{\text {gate }}}{T_{1}}}, P_{\text {dephasing }}=0.5 \times\left(1-e^{-\left(\frac{t_{\text {gate }}}{T_{2}}-\frac{\text { tgate }}{2 T_{1}}\right)}\right)`
:math:`K_{1}=K_{1 \text { damping }} K_{1 \text { dephasing }} K_{2}=K_{1 \text { damping }} K_{2 \text { dephasing }}`
:math:`K_{3}=K_{2 \text { dampin }} K_{1 \text { dephasing }} K_{4}=K_{2 \text { dampin }} K_{2 \text { dephasing }}`

Three noise parameters are required.

(4) DEPOLARIZING\_KRAUS\_OPERATOR
'''''''''''''''''''''''''''''''''''''''

DEPOLARIZING\_KRAUS\_OPERATOR depolarization noise model means that
single qubit is replaced by a completely mixed state I/2 under specific
probability. Its kraus operator and representation are as shown below:

.. math::


   \begin{gathered}
   K_{1}=\sqrt{1-3 p / 4} \times \mathrm{I}, K_{2}=\sqrt{p} / 2 \times \mathrm{X} \\
   K_{3}=\sqrt{p} / 2 \times \mathrm{Y}, K_{4}=\sqrt{p} / 2 \times \mathrm{Z}
   \end{gathered}

I, X, Y and Z respectively indicate that the matrix corresponding to
their quantum logic gates

A noise parameter is required.

(5) BITFLIP\_KRAUS\_OPERATOR
''''''''''''''''''''''''''''''''''

BITFLIP\_KRAUS\_OPERATOR is the qubit inversion noise model. Its kraus
operator and representation are as shown below:

.. math::


   K_{1}=\left[\begin{array}{cc}
   \sqrt{1-p} & 0 \\
   0 & \sqrt{1-p}
   \end{array}\right], K_{2}=\left[\begin{array}{cc}
   0 & \sqrt{p} \\
   \sqrt{p} & 0
   \end{array}\right]

A noise parameter is required.

(6) BIT\_PHASE\_FLIP\_OPRATOR
'''''''''''''''''''''''''''''''''''

BIT\_PHASE\_FLIP\_OPRATOR is the qubit-phase inversion noise model. Its
kraus operator and representation are as shown below:

.. math::


   K_{1}=\left[\begin{array}{cc}
   \sqrt{1-p} & 0 \\
   0 & \sqrt{1-p}
   \end{array}\right], K_{2}=\left[\begin{array}{cc}
   0 & -i \times \sqrt{p} \\
   i \times \sqrt{p} & 0
   \end{array}\right]

(7) PHASE\_DAMPING\_OPRATOR
'''''''''''''''''''''''''''''''''

PHASE\_DAMPING\_OPRATOR is the phase damping noise model. Its kraus
operator and representation are as shown below:

.. math::


   K_{1}=\left[\begin{array}{cc}
   1 & 0 \\
   0 & \sqrt{1-p}
   \end{array}\right], K_{2}=\left[\begin{array}{cc}
   0 & 0 \\
   0 & \sqrt{p}
   \end{array}\right]

A noise parameter is required.

(8) DOULE-GATE NOISE MODEL
'''''''''''''''''''''''''''''''''

Similarly, the double-gate noise model is also divided into the
above-mentioned types: DAMPING\_KRAUS\_OPERATOR,

DEPHASING\_KRAUS\_OPERATOR,

DECOHERENCE\_KRAUS\_OPERATOR,

DEPOLARIZING\_KRAUS\_OPERATOR,

BITFLIP\_KRAUS\_OPERATOR,

BIT\_PHASE\_FLIP\_OPRATOR,

and PHASE\_DAMPING\_OPRATOR.

They have the same input parameters as the single-gate noise model;
their kraus operator and representation correspond to those of the
single-gate noise model: if the single-gate noise model is {K1, K2}, the
corresponding double-gate noise model is {K1⊗K1, K1⊗K2, K2⊗K1, K2⊗K2}.

2.6.2.2 Interface introduction
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Noise model supported currently by pyqpanda

::

    class NoiseModel(__pybind11_builtins.pybind11_object):
        """
        Members:

        DAMPING_KRAUS_OPERATOR

        DECOHERENCE_KRAUS_OPERATOR

        DEPHASING_KRAUS_OPERATOR

        PAULI_KRAUS_MAP

        DECOHERENCE_KRAUS_OPERATOR_P1_P2

        BITFLIP_KRAUS_OPERATOR

        DEPOLARIZING_KRAUS_OPERATOR

        BIT_PHASE_FLIP_OPRATOR

        PHASE_DAMPING_OPRATOR

A noise parameter is set by the following method:

::

    from pyqpanda import *
    import numpy as np

    qvm = NoiseQVM()
    qvm.init_qvm()
    q = qvm.qAlloc_many(4)
    c = qvm.cAlloc_many(4)

    # If no function qubit is specified, all qubits are valid
    qvm.set_noise_model(NoiseModel.BITFLIP_KRAUS_OPERATOR, GateType.PAULI_X_GATE, 0.1)
    # When specifying a qubit, only the specified qubit takes effect
    qvm.set_noise_model(NoiseModel.BITFLIP_KRAUS_OPERATOR, GateType.RY_GATE, 0.1,\ [q[0], q[1]])
    # When you specify a qubit for a double gate, you need to specify two qubits at
    # the same time and are sensitive to the sequence of the qubits
    qvm.set_noise_model(NoiseModel.DAMPING_KRAUS_OPERATOR, GateType.CNOT_GATE, 0.1,\ [[q[0], q[1]],[q[1], q[2]]])
    # Noise can be added to all types in the circuit
    qvm.set_noise_model(NoiseModel.BITFLIP_KRAUS_OPERATOR, types, 0.1)
    qvm.set_noise_model(NoiseModel.DECOHERENCE_KRAUS_OPERATOR, types, 0.1, 0.2, 0.3)
    qvm.set_noise_model(NoiseModel.DAMPING_KRAUS_OPERATOR, GateType.CNOT_GATE, 0.1, q)

The first paramenter is the type of the noise modle; the second
parameter is the type of the quantum logic gate; and the third parameter
is required for the noise modle.

Three noise parameters are set by the following method:

::

    # If no function qubit is specified, all qubits are valid
    qvm.set_noise_model(NoiseModel.DECOHERENCE_KRAUS_OPERATOR, GateType.PAULI_Y_GATE,\
    5, 2, 0.01)
    # When specifying a qubit, only the specified qubit takes effect
    qvm.set_noise_model(NoiseModel.DECOHERENCE_KRAUS_OPERATOR, GateType.Y_HALF_PI,\
    5, 2, 0.01, [q[0], q[1]])
    # When you specify a qubit for a double gate, you need to specify two qubits at the same
    # time and are sensitive to the sequence of the qubits
    qvm.set_noise_model(NoiseModel.DECOHERENCE_KRAUS_OPERATOR, GateType.CZ_GATE,\
    5, 2, 0.01, [[q[0], q[1]], [q[1], q[0]]])
    # Noise can be added to all Gatetype in the circuit
    qvm.set_noise_model(NoiseModel.BITFLIP_KRAUS_OPERATOR, types, 0.1)
    qvm.set_noise_model(NoiseModel.DECOHERENCE_KRAUS_OPERATOR, types, 0.1, 0.2, 0.3)
    qvm.set_noise_model(NoiseModel.DAMPING_KRAUS_OPERATOR, GateType.CNOT_GATE, 0.1, q)

The noise inclusive simulator also supports the error of the rotation
angle of the quantum logic gate with an angle, and its interface usage
is as shown below:

::

    qvm.set_rotation_error(0.05)

Namely, the error of the rotation angle is set to 0.05.

The measurement error is set by the method similar to the above setting
method, except that the type of the quantum logic gate is not
designated.

::

    qvm.set_measure_error(NoiseModel.DEPOLARIZING_KRAUS_OPERATOR, 0.1)

Setting of reset noise:

::

    p0 = 0.9
    p1 = 0.05
    qvm.set_reset_error(p0, p1)

p0 is the probability to reset noise to \|0⟩; p1 is the probability to
reset noise to \|1⟩; and the probability not to reset noise is 1-p0-p1.

Setting of read error:

::

    f0 = 0.9
    f1 = 0.85
    qvm.set_readout_error([[f0, 1 - f0], [1 - f1, f1]])

When reading q0, the probability to read 0 as 0 is 0.9, the probability
to read 0 as 1 is 1 - f0, the probability to read 1 as 1 is 0.85, and
the probability to read 0 as 1 is 1 - f1.

2.6.2.3 Example
^^^^^^^^^^^^^^^

::

    from pyqpanda import *
    import numpy as np

    if __name__ == "__main__":
        qvm = NoiseQVM()
        qvm.init_qvm()
        q = qvm.qAlloc_many(4)
        c = qvm.cAlloc_many(4)

        qvm.set_noise_model(NoiseModel.BITFLIP_KRAUS_OPERATOR,\ GateType.PAULI_X_GATE, 0.1)
        qv0 = [q[0], q[1]]
    qvm.set_noise_model(NoiseModel.DEPHASING_KRAUS_OPERATOR,\
    GateType.HADAMARD_GATE, 0.1, qv0)
        qves = [[q[0], q[1]], [q[1], q[2]]]
        qvm.set_noise_model(NoiseModel.DAMPING_KRAUS_OPERATOR,\ GateType.CNOT_GATE, 0.1, qves)

        f0 = 0.9
        f1 = 0.85
        qvm.set_readout_error([[f0, 1 - f0], [1 - f1, f1]])
        qvm.set_rotation_error(0.05)

        prog = QProg()
        prog << X(q[0]) << H(q[0]) \
             << CNOT(q[0], q[1]) \
             << CNOT(q[1], q[2]) \
             << CNOT(q[2], q[3]) \
             << measure_all(q, c)

        result = qvm.run_with_configuration(prog, c, 1000)
        print(result)

Running results:

::

    {'0000': 347, '0001': 55, '0010': 50, '0011': 43, '0100': 41, '0101': 18, '0110': 16, '0111': 34, '1000': 50, '1001': 18, '1010': 18, '1011': 37, '1100': 15, '1101': 49, '1110': 42, '1111': 167}

2.6.3 Single-amplitude quantum simulator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

At present, we can use the classical computer to simulate quantum
simulator based on relevant quantum computing theories. In terms of
simulation, the quantum simulator is mainly divided into full amplitude
and single amplitude. Their difference mainly lies in that: all
amplitudes of the quantum state can be worked out through one-time
full-amplitude simulation computing, and one of \ :math:`2^n`\ amplitudes only can be worked out through one single-amplitude
simulation computing.

However, the full-amplitude simulation quantum computing requires
relatively long time, and therefore the computing quantity increases
along the number of qubits; under the existing hardware, when the number
of qubits is beyond 49, no simulation is allowed. When the number of
qubits is beyond 49, the simulation can be achieved by the
single-amplitude quantum simulator; the simulation speed is greatly
improved, and the computing quantity of the algorithm does not increase
along with the number of qubits.

2.6.3.1 Instructions
^^^^^^^^^^^^^^^^^^^^

Its usage is very similar to the usage of the quantum simulator module
described above. The main interfaces are as follows:

``run``: input parameters include executed quantum programs, applied qubits,
maximum RANK, and maximum running time to optimize quickBB

``pmeasure_bin_index``: input parameters are binary index strings, and
output parameters are the quantum state under the index. Before use, run
interface requires to be invoked, such as
pmeasure\_bin\_index("0000000000"); meanwhile, it is ensured that the
string length is the same as the number of qubits measured.

``pmeasure_dec_index``: input parameters are decimal index strings, and
output parameters are the quantum state under the index. Before use, run
interface requires to be invoked, such as pmeasure\_dec\_index("1");
meanwhile, it is ensured that the index is not beyond 2 to the power of
n (n is the number of qubits).

``get_prob_dict``: input parameters include quantum program to be executed
and qubits to be measured. Output parameters are all state results of
the corresponding qubits. Before use, run interface requires to be
invoked, It should be noted that the interface can be used when the
number of qubits is within 30.

``prob_run_dict``: input parameters include quantum program to be executed
and qubits to be measured. Output parameters are all state results of
the corresponding qubits. It should be noted that the interface can be
used when the number of qubits is within 30.

Firstly, initialize a single-amplitude quantum simulator object through
``SingleAmpQVM`` in order to manage a series of subsequent behaviors.

::

    from pyqpanda import *
    from numpy import pi

    qvm = SingleAmpQVM()

Secondly, initialize, construct and carry the quantum programs:

::

    qvm.init_qvm()

    qv = qvm.qAlloc_many(10)
    cv = qvm.cAlloc_many(10)

    prog = QProg()

    # Building quantum programs
    prog << CZ(qv[1], qv[5])\
        << CZ(qv[3], qv[5])\
        << CZ(qv[2], qv[4])\
        << CZ(qv[3], qv[7])\
        << CZ(qv[0], qv[4])\
        << RY(qv[7], pi / 2)\
        << RX(qv[8], pi / 2)\
        << RX(qv[9], pi / 2)\
        << CR(qv[0], qv[1], pi)\
        << CR(qv[2], qv[3], pi)\
        << RY(qv[4], pi / 2)\
        << RZ(qv[5], pi / 4)\
        << RX(qv[6], pi / 2)\
        << RZ(qv[7], pi / 4)\
        << CR(qv[8], qv[9], pi)\
        << CR(qv[1], qv[2], pi)\
        << RY(qv[3], pi / 2)\
        << RX(qv[4], pi / 2)\
        << RX(qv[5], pi / 2)\
        << CR(qv[9], qv[1], pi)\
        << RY(qv[1], pi / 2)\
        << RY(qv[2], pi / 2)\
        << RZ(qv[3], pi / 4)\
        << CR(qv[7], qv[8], pi)

The interface is used as follows:

``pmeasure_bin_index`` is combined with ``run`` method in use. Example:

::

    qvm.run(prog, qv)
    dec_result = qvm.pmeasure_bin_index("0001000000")
    print("0001000000 : ",dec_result)

Output results are as follows:

::

    0001000000 :  0.001953123603016138

``pmeasure_dec_index`` is combined with ``run`` method in use. Example:

::

    qvm.run(prog, qv)
    dec_result = qvm.pmeasure_dec_index("2")
    print("2 : ",dec_result)

Output results are as follows:

::

    2 :  0.001953123603016138

``get_prob_dict`` is combined with ``run`` method in use. Example:

::

    qvm.run(prog, qv)
    res = qvm.get_prob_dict(qv)

``prob_run_dict`` interface is the encapsulation of ``get_prob_dict`` and
``run``, with the example as follows:

::

    res_1 = qvm.prob_run_dict(prog, qv)

2.6.4 Partial-amplitude quantum simulator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

At present, the classical computer simulates the quantum simulator
mainly by full amplitude and single amplitude. Besides, the
partial-amplitude quantum simulator can realize higher simulation
efficiency under the lower hardware conditions.

2.6.4.1 Instructions
^^^^^^^^^^^^^^^^^^^^

The usage of the partial-amplitude quantum simulator is very similar to
that of the quantum simulator module described above. Firstly,
initialize a partial-amplitude quantum simulator object through
``PartialAmpQVM`` in order to manage a series of subsequent behaviors.

::

    from pyqpanda import *
    from numpy import pi
    machine = PartialAmpQVM()

Secondly, initialize, construct and carry the quantum programs, where
the quantum programs are demonstrated in the partial-amplitude example
program of ref: of pyQPanda.

::

    machine.init_qvm()

    q = machine.qAlloc_many(10)
    c = machine.cAlloc_many(10)

    # Building quantum programs
    prog = QProg()
    prog << hadamard_circuit(q)\
         << CZ(q[1], q[5])\
         << CZ(q[3], q[7])\
         << CZ(q[0], q[4])\
         << RZ(q[7], pi / 4)\
         << RX(q[5], pi / 4)\
         << RX(q[4], pi / 4)\
         << RY(q[3], pi / 4)\
         << CZ(q[2], q[6])\
         << RZ(q[3], pi / 4)\
         << RZ(q[8], pi / 4)\
         << CZ(q[9], q[5])\
         << RY(q[2], pi / 4)\
         << RZ(q[9], pi / 4)\
         << CZ(q[2], q[3])

    machine.run(prog)

Partial interfaces are used as follows:

● ``pmeasure_bin_index(string)``, example

::

    result = machine.pmeasure_bin_index("0000000000")
    print(result)

Output results are as follows:

::

    (-0.00647208746522665-0.006472080945968628j)

● ``pmeasure_dec_index(string)``, example

::

    result = machine.pmeasure_dec_index("1")
    print(result)

Output results are as follows:

::

    (-6.068964220062867e-10-0.009152906015515327j)

● ``pmeasure_subset(state_index)``, example

::

    state_index = ["0", "1", "2"]
    result = machine.pmeasure_subset(state_index)
    print(result)

Output results are as follows:

::

    {'0': (-0.00647208746522665-0.006472080945968628j),
     '1': (-6.068964220062867e-10-0.009152906015515327j),
     '2': (-6.984919309616089e-10-0.009152908809483051j)}


.. Caution:: 
    Partial old interfaces, including ``get_qstate()``, ``pmeasure(string)``, ``pmeasure(string)`` and ``get_prob_dict(qvec,string)``, have been obsoleted.

2.6.5 Tensor network quantum simulator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For a spin system with \ :math:`N`\  qubits, its dimension of Hilbert space is \ :math:`2^N`\

With respect to the state evolution of the complex system, the
traditional full-amplitude simulator considers it as a one-dimensional
vector with \ :math:`2^N`\ elements.

Viewed from the tensor network, the coefficient of the entire system
quantum state corresponds to \ :math:`2^N`\ dimension tensor (namely, N-order tensor with N indicators respectively
taking 2 as dimension); the coefficient of the quantum operator is \ :math:`2^{2N}`\ dimension tensor (namely, 2N-order tensor with 2N indicators respectively
taking 2 as dimension); the quantum state can be denoted by the
following graphs:

.. figure::  /images/2.26.png
   :alt: 

As the number of spins of the quantum system increases, the number of
quantum state coefficients increases exponentially, which is considered
as the exponential wall. The exponential wall limits the maximum number
of simulation spins and performance of the traditional full-amplitude
simulator.

However, the exponential wall can be addressed by the tensor network, so
as to avoid its influence. In the tensor network, our simulation of
quantum system, including operation and measurement of quantum logic
gate, can be realized through the contraction and decomposition of
tensors. The matrix product state is the most common representation in
the tensor network and taken as TT (tensor-train) in the multi-linear
algebra, as illustrated below. 

.. figure::  /images/2.27.png
   :alt:

The quantum state is decomposed as the representation form (namely, the
right side of the equation); for some quantum logic gates in the quantum
circuit, the global problems can be transformed into a local tensor
problem, thereby effectively reducing the time complexity and space
complexity.

2.6.5.1 Applications
^^^^^^^^^^^^^^^^^^^^

In the simulation method of quantum circuit, it is very important to
select the appropriate simulation backend. Different quantum circuit
simulators have the application places as follows:

``Full-amplitude quantum simulator``: the full-amplitude quantum simulator
can simultaneously simulate and store all the amplitudes of the quantum
state. However, due to the restrictions of the machine memory, the limit
of qubit is 50 bits and suitable for quantum circuits with low qubits
and high depth, such as Google random quantum circuits with low bits and
scenarios required for the acquisition of all simulation results.

``Partial-amplitude quantum simulator``: the partial-amplitude quantum
simulator can simulate a higher number of qubits depending upon the
low-bit quantum circuit amplitude simulation results provided by other
simulators, but its simulation depth is reduced. Therefore, such
simulator is usually used to obtain partial subset simulation results of
quantum state amplitude.

``Single-amplitude quantum simulator``: the single-amplitude quantum
simulator can simulate a higher qubit circuit diagram, have higher
simulation performance, but will not increase exponentially as the
number of qubits. With the increasing circuit depth, simulation
performance decreases sharply; at the same time, the difficulty in
simulation of more control gates also becomes its drawback, such
simulator is suitable for the simulation of the high-bit low-depth
quantum circuit, and usually suitable for the quick simulation to obtain
single quantum state amplitude.

``Tensor network quantum simulator``: The tensor network simulator is
similar to the single-amplitude quantum simulator; compared with the
single-amplitude quantum simulator, it can simulate more control gates
and have the performance advantage in higher-depth circuit simulation.

``Quantum cloud simulator``: the quantum cloud simulator can submit tasks to
remote high-performance computing clusters for running, thereby breaking
through the restrictions of the local hardware performance and
supporting the quantum algorithm on the real quantum chip.

2.6.5.2 Instructions
^^^^^^^^^^^^^^^^^^^^

In pyqpanda，the quantum circuit can be simulated through the tensor
network through ``MPSQVM``. Like the application methods of many other
simulators, the tensor network simulator is provided with the same
quantum simulator interface, for example, the simple example code is
given below:

::

    from numpy import pi
    from pyqpanda import *

    # Build a quantum virtual machine
    qvm = MPSQVM()

    # Initialization operation
    qvm.set_configure(64, 64)
    qvm.init_qvm()

    q = qvm.qAlloc_many(10)
    c = qvm.cAlloc_many(10)

    # Building quantum programs
    prog = QProg()
    prog << hadamard_circuit(q)\
        << CZ(q[2], q[4])\
        << CZ(q[3], q[7])\
        << CNOT(q[0], q[1])\
        << Measure(q[0], c[0])\
        << Measure(q[1], c[1])\
        << Measure(q[2], c[2])\
        << Measure(q[3], c[3])

    # The quantum program runs 100 times and returns the measurement
    result = qvm.run_with_configuration(prog, c, 100)

    # The number of times the printed quantum state appears in the result of
    # multiple runs of the quantum program
    print(result)

    qvm.finalize()

2.6.5.3 Complete example code
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following example shows how to use the computing interfaces of the
tensor network simulator.

::

    from numpy import pi
    from pyqpanda import *

    qvm = MPSQVM()
    qvm.set_configure(64, 64)
    qvm.init_qvm()

    q = qvm.qAlloc_many(10)
    c = qvm.cAlloc_many(10)

    prog = QProg()
    prog << hadamard_circuit(q)\
        << CZ(q[2], q[4])\
        << CZ(q[3], q[7])\
        << CNOT(q[0], q[1])\
        << CZ(q[3], q[7])\
        << CZ(q[0], q[4])\
        << RY(q[7], pi / 2)\
        << RX(q[8], pi / 2)\
        << RX(q[9], pi / 2)\
        << CR(q[0], q[1], pi)\
        << CR(q[2], q[3], pi)\
        << RY(q[4], pi / 2)\
        << RZ(q[5], pi / 4)\
        << Measure(q[0], c[0])\
        << Measure(q[1], c[1])\
        << Measure(q[2], c[2])

    # Monte Carlo sampling simulation interface
    result0 = qvm.run_with_configuration(prog, c, 100)

    # Probability measurement interface
    result1 = qvm.prob_run_dict(prog, [q[0], q[1], q[2]], -1)

    print(result0)
    print(result1)

    qvm.finalize()

In the above code, “run\_with\_configuration” and ``prob_run_dict``
interfaces are respectively used for the sampling simulation and
probability measurement of Monte Carlo, and output the simulation
sampling results and the probability corresponding to amplitude, and the
above program has the following computing results:

::

    # Monte Carlo sampling simulation results
    {'0000000000': 7,
     '0000000001': 12,
     '0000000010': 13,
     '0000000011': 10,
     '0000000100': 16,
     '0000000101': 14,
     '0000000110': 12,
     '0000000111': 16}

    # Probability measurement results
    {'000': 0.12499999999999194,
     '001': 0.12499999999999185,
     '010': 0.12499999999999194,
     '011': 0.124999999999992,
     '100': 0.12499999999999198,
     '101': 0.12499999999999194,
     '110': 0.12499999999999198,
     '111': 0.12499999999999208}

2.7 Qubit pool
--------------

2.7.1 Introduction
~~~~~~~~~~~~~~~~~~

In previous QPanda, the application, management and control of qubits
and classical registers are done by the simulator. A method independent
to the simulator is provided, that is, qubits and classical registers
are not managed by the simulator, and can be directly applied and
released by the qubit pool provided. In order to better use qubits and
classical registers, we further support physical addresses in
substitution of the corresponding qubits.

2.7.2 Interface description
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Qubit pool:

``OriginQubitPool`` is to get single qubit pool, and the qubits are released
by applying the pool object

``get_capacity`` is to get the maximum capacity

``set_capacity`` sets the capacity

``get_qubit_by_addr`` is to get the qubits through the physical address

Classical register pool:

``OriginCMem`` is to get single classical register and the classical
register is released by applying the pool object

``get_capacity`` is to get the maximum capacity

``set_capacity`` sets the capacity

``get_cbit_by_addr`` is to get the qubit through the physical address

The qubit pool has the same application and release methods as the
simulator. A detailed description is given in `quantum
simulator <https://pyqpanda-toturial.readthedocs.io/zh/latest/QuantumMachine.html#quantummachine>`__.
Meanwhile, in use of the qubits and the classical registers, the
parameters can be directly transmitted through the address corresponding
to qubit.

For example: ``H(1)`` can be understood that H gate performs its function on
the qubit taking 1 as the physical address. ``Measure(1, 1)`` can be
understood that measurement is exerted to the qubit taking 1 as the
physical address, and the results are stored into the classical register
taking 1 as the address.

2.7.3 Example
~~~~~~~~~~~~~

::

    from pyqpanda import *
    from numpy import pi
    if __name__=="__main__":
    # The qubit can be separated from the virtual machine and obtain the singleton of the
    # corresponding pool. Different from QPanda, the object constructed here is the singleton
    # pool
        qpool = OriginQubitPool()
        qpool_1 = OriginQubitPool()
        cmem = OriginCMem()
        # Get the qubit pool capacity
        print("get_capacity : ", qpool.get_capacity())
        # Set the qubit pool capacity
        qpool.set_capacity(20)
        print("qpool get_capacity : ", qpool.get_capacity())
    #Since the qubit pool is a singleton object, set the capacity to 20 above, where qooL_1 
    # will also get the capacity to 20
        print("qpool_1 get_capacity : ", qpool_1.get_capacity())
    # Apply for qubits from a qubit pool. In the singleton mode, ensure that the number of 
    # applied qubits does not exceed the maximum capacity
        qv = qpool.qAlloc_many(6)
        cv = cmem.cAlloc_many(6)
        # Creating a simulator
        qvm = CPUQVM()
        qvm.init_qvm()
        prog = QProg()
        # Use the physical address directly as the qubit information input parameter
        prog << H(0)\
            << H(1)\
            << H(2)\
            << H(4)\
            << X(5)\
            << X1(2)\
            << CZ(2, 3)\
            << RX(3, pi / 4)\
            << CR(4, 5, pi / 2)\
            << SWAP(3, 5)\
            << CU(1, 3, pi / 2, pi / 3, pi / 4, pi / 5)\
            << U4(4, 2.1, 2.2, 2.3, 2.4)\
            << BARRIER([0, 1,2,3,4,5])\
            << BARRIER(0)
        #print(prog)
        # Measurement methods can also use bit physical addresses
        res_0 = qvm.prob_run_dict(prog, [ 0,1,2,3,4,5 ])
        #res_1 = qvm.prob_run_dict(prog, qv)  #同等上述方法
        #print(res_0)
        # The same classical bit address can also be used as a classical bit information input
        prog << Measure(0, 0)\
            << Measure(1, 1)\
            << Measure(2, 2)\
            << Measure(3, 3)\
            << Measure(4, 4)\
            << Measure(5, 5)
        # Use the classical bit address to enter the parameter
        res_2 = qvm.run_with_configuration(prog, [ 0,1,2,3,4,5 ], 5000)
        # res_3 = qvm.run_with_configuration(prog, cv, 5000) # Same as above
        #print(res_2)
        qvm.finalize()
    # At the same time, we can also use the QV applied here again to avoid the problem of 
    # applying bits for multiple times using virtual machines
    qvm_noise = NoiseQVM()
        qvm_noise.init_qvm()
        res_4 = qvm_noise.run_with_configuration(prog, [ 0,1,2,3,4,5 ], 5000)
        qvm_noise.finalize()

Output results are as follows:

::

    get_capacity :  29
    qpool get_capacity :  20
    qpool_1 get_capacity :  20

2.8 Quantum measurement
-----------------------

Quantum measurement is to obtain necessary information by externally
interfering with the quantum system, and the measurement gate is
measured by the Monte Carlo method. It is represented by the following
icons in the quantum circuit:

.. figure::  /images/2.28.png
   :alt:

2.8.1 Interface introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Chapter mainly introduces obtained quantum measurement objects,
quantum programs containing quantum measurement according to run
configuration, as well as quick measure.

In the quantum program, we need to measure a specific qubit and store
the measurement results in the classical register. Besides, a
measurement object can be obtained by the following way:

::

    measure = Measure(qubit, cbit);

It can be seen that two parameters access to Measure, where the first
parameter is the measurement qubit and the second parameter is the
classical register.

If you want to measure all the qubits and store them in the
corresponding classical register, the following operations can be
conducted:

::

    measureprog = measure_all(qubits， cbits);

qubits is classified as ``QVec``; and cbits is classified as
``ClassicalCondition list``.

.. admonition:: Note

    The ``measure_all`` returned values are classified as ``QProg``.



After getting the program containing quantum measurement, we can invoke
``directly_run`` or ``run_with_configuration`` to get the measurement results
of the quantum program.

``directly_run`` function is to run the quantum programs and return the run
results, and its usage is as follows:

::

    prog = QProg()
    prog << H(qubits[0])\
         << CNOT(qubits[0], qubits[1])\
         << CNOT(qubits[1], qubits[2])\
         << CNOT(qubits[2], qubits[3])\
         << Measure(qubits[0], cbits[0])

    result = directly_run(prog)

``run_with_configuration`` function is to count the multi-run measurement
results of the quantum program, and its usage is as follows:

::

    prog = QProg()
    prog << H(qubits[0])\
         << H(qubits[0])\
         << H(qubits[1])\
         << H(qubits[2])\
         << measure_all(qubits, cbits)

    result = run_with_configuration(prog, cbits, 1000)

The first parameter is the quantum program, the second parameter is
ClassicalCondition list, and the third parameter is the running
frequency.

2.8.2 Example
~~~~~~~~~~~~~

::

    from pyqpanda import *

    if __name__ == "__main__":
        init(QMachineType.CPU)
        qubits = qAlloc_many(4)
        cbits = cAlloc_many(4)

        # Building quantum programs
        prog = QProg()
        prog << H(qubits[0])\
             << H(qubits[1])\
             << H(qubits[2])\
             << H(qubits[3])\
             << measure_all(qubits, cbits)

        # The quantum program runs 1000 times and returns the measurement
        result = run_with_configuration(prog, cbits, 1000)

        # Print measurement results
        print(result)
        finalize()

Output results are as follows:

::

    {'0000': 59, '0001': 69, '0010': 52, '0011': 62,
    '0100': 63, '0101': 67, '0110': 79, '0111': 47,
    '1000': 73, '1001': 59, '1010': 72, '1011': 60,
    '1100': 61, '1101': 71, '1110': 50, '1111': 56}

2.9 Probability measurement
---------------------------

Probability measurement is to obtain the amplitude of target qubit,
where the target qubit can be single qubit or a set of qubits. In
pyQPanda, probability measurement is also named as PMeasure. Probability
measurement and `quantum
measure <https://pyqpanda-toturial.readthedocs.io/zh/latest/Measure.html#measure>`__\ ment
are completely different; Measure is to perform a measurement, return a
definite 0/1 result, and change the quantum state.

2.9.1 Interface introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

PyQPanda provides three ways to obtain PMeasure results, including
``prob_run_list``, ``prob_run_tuple_list``, and ``prob_run_dict``.

 ● ``prob_run_list`` is to get the list of probability measurement results of the target qubits.

 ● ``prob_run_tuple_list`` is to get the probability measurement results of the target qubit and belongs to a dictionary; its corresponding index is of the decimal system.

 ● ``prob_run_dict`` is to get the probability measurement results of the target qubit and belongs to a dictionary; its corresponding index is of the binary system.

The above-mentioned three functions have the same usage; and the following describes ``prob_run_dict`` as an example, and its usage is given below:

::

    prog = QProg()
    prog << H(qubits[0])\
         << CNOT(qubits[0], qubits[1])\
         << CNOT(qubits[1], qubits[2])\
         << CNOT(qubits[2], qubits[3])

    result = prob_run_dict(prog, qubits, 3)

The first parameter is the quantum program; and the second parameter is
``QVec`` and defines the qubits concerned by us. When the third parameter is
-1, all probability measurement results are obtained; when it is more
than zero, the largest top several numbers are obtained.

2.9.2 Example
~~~~~~~~~~~~~

::

    from pyqpanda import *

    if __name__ == "__main__":
        init(QMachineType.CPU)
        qubits = qAlloc_many(2)
        cbits = cAlloc_many(2)

        prog = QProg()
        prog << H(qubits[0])\
            << CNOT(qubits[0], qubits[1])

        print("prob_run_dict: ")
        result1 = prob_run_dict(prog, qubits, -1)
        print(result1)

        print("prob_run_tuple_list: ")
        result2 = prob_run_tuple_list(prog, qubits, -1)
        print(result2)

        print("prob_run_list: ")
        result3 = prob_run_list(prog, qubits, -1)
        print(result3)

        finalize()

Output results are as follows:

::

    prob_run_dict:
    {'00': 0.4999999999999999, '01': 0.0, '10': 0.0, '11': 0.4999999999999999}
    prob_run_tuple_list:
    [(0, 0.4999999999999999), (3, 0.4999999999999999), (1, 0.0), (2, 0.0)]
    prob_run_list:
    [0.4999999999999999, 0.0, 0.0, 0.4999999999999999]

.. admonition:: Note

   ``Probability measurement`` is not applicable to the noise simulator.


2.10 OriginQ Cloud service
--------------------------

In complex quantum circuit simulation, there is a need to use the
high-performance computer cluster or the real quantum computer to
replace local computing with cloud computing, which will result in
reducing the user's computing cost to a certain extent and obtaining the
better computing experience.

Origin quantum cloud platform submits tasks to the remote quantum
computer or computing cluster through the scheduling server and receives
returned results, as shown in Figure below. 

.. figure::  /images/2.29.png
   :alt:


As pyqpanda encapsulates the quantum cloud simulator, it can send
computing instructions to the computing server cluster of the origin
quantum or the real quantum chip, and obtain computing results. Before
using the simulators described below, you need to ensure that the
corresponding simulators have been launched. 

.. figure::  /images/2.30.png
   :alt:

.. figure::  /images/2.31.png
   :alt:

2.10.1 Real chip computing service
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

2.10.1.1 Origin Wuyuan superconducting chip
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``Origin Wuyuan`` is the superconducting quantum computer developed independently by Origin Quantum on September 12, 2020 (which carries 6-qubit superconducting quantum processor KF C6-130). Benefited from the Origin superconducting quantum computing cloud platform, the quantum computer can escape from the laboratory, provide many potential
industries with basic conditions to explore quantum computing, and
promote the implementation and engineering development of quantum
computing industry, thereby truly serving the human society.

As a bridge to link the user with the quantum computing system, the
superconducting quantum computing cloud platform plays a crucial
coordination and transfer role in the process in which the user
initiates computing tasks to the quantum system and the quantum system
completes the tasks and returns the computing results.

``The chip topology structure`` of Origin Wuyuan is shown below:

.. figure::  /images/2.32.png
   :alt:

``The corresponding chip parameters`` are shown in the figure below:

.. figure::  /images/2.33.png
   :alt:

The interface is described as follows:

    ● 1.Monte Carlo measurement interface: ``real_chip_measure``, with the example as follows:

::

    from pyqpanda import *
    PI = 3.1416
    # Create a quantum cloud simulator machine through QCloud()
    QCM = QCloud()
    # Initialization by passing in the current user's token
    QCM.init_qvm("E02BB115D5294012AA88D4BE82603984", True)
    q = QCM.qAlloc_many(6)
    c = QCM.cAlloc_many(6)
    # Building quantum programs
    prog = QProg()
    prog << hadamard_circuit(q)\
        << RX(q[1], PI / 4)\
        << RX(q[2], PI / 4)\
        << RX(q[1], PI / 4)\
        << CZ(q[0], q[1])\
        << CZ(q[1], q[2])\
        << Measure(q[0], c[0])\
        << Measure(q[1], c[1])
    # To invoke the real chip computing interface, at least two parameters, 
    # quantum program and measurement times, are required. The next three 
    # default parameters are chip type and whether line mapping and line 
    # optimization functions are enabled.
    result = QCM.real_chip_measure(prog, 1000, real_chip_type.origin_wuyuan_d4)
    print(result)
    QCM.finalize()

It should be noted in the above process that ``init`` requires a subscriber
to import the subscriber validation ``token`` of the quantum cloud platform,
and obtains it from the personal information of the OriginQ Cloud
platform, and its details are shown in the following screenshot.

.. figure::  /images/2.34.png
   :alt:

The output results are given below: the binary representation of the
quantum state on the left and the corresponding probability of the
number of measurements on the right:

    ● 2. Get quantum state (qst) tomography result interface: ``get_state_tomography_density``，with the example shown below:

::

    from pyqpanda import *

    PI = 3.1416

    # Create a quantum cloud simulator machine through QCloud()
    QCM = QCloud()

    # Initialization by passing in the current user's token
    QCM.init_qvm("E02BB115D5294012AA88D4BE82603984", True)

    q = QCM.qAlloc_many(6)
    c = QCM.cAlloc_many(6)

    # Building quantum programs
    prog = QProg()
    prog << hadamard_circuit(q)\
        << RX(q[1], PI / 4)\
        << RX(q[2], PI / 4)\
        << RX(q[1], PI / 4)\
        << CZ(q[0], q[1])\
        << CZ(q[1], q[2])\
        << Measure(q[0], c[0])\
        << Measure(q[1], c[1])

    # To invoke the real chip computing interface, at least two parameters, 
    # quantum program and measurement times, are required. The next three
    # default parameters are chip type and whether line mapping and line 
    # optimization functions are enabled.
    result = QCM.get_state_tomography_density( prog, 1000, real_chip_type.origin_wuyuan_d4)
    print(result)

    QCM.finalize()

Output results are as follows:

::

    [[(0.26001013684744045+0j), (0.23492143943233657+0.000760263558033436j), (0.01267105930055755+0.002280790674100364j), (-0.003547896604156095-0.003294475418144968j)],
    [(0.23492143943233657-0.000760263558033436j), (0.250886974151039+0j), (0.00937658388241254+0.003547896604156081j), (0.009883426254434847-0.0025342118601114905j)],
    [(0.01267105930055755-0.002280790674100364j), (0.00937658388241254-0.003547896604156081j), (0.2412569690826153+0j), (-0.2240243284338571-0.009123162696401413j)],
    [(-0.003547896604156095+0.003294475418144968j), (0.009883426254434847+0.0025342118601114905j), (-0.2240243284338571+0.009123162696401413j), (0.24784591991890528+0j)]]

    ● 3. Get quantum state fidelity interface: ``get\_state\_fidelity``,with the example shown below:

::

    from pyqpanda import *

    PI = 3.1416

    # Create a quantum cloud simulator machine through QCloud()
    QCM = QCloud()

    # Initialization by passing in the current user's token
    QCM.init_qvm("E02BB115D5294012AA88D4BE82603984", True)

    q = QCM.qAlloc_many(6)
    c = QCM.cAlloc_many(6)

    # Building quantum programs
    prog = QProg()
    prog << hadamard_circuit(q)\
        << RX(q[1], PI / 4)\
        << RX(q[2], PI / 4)\
        << RX(q[1], PI / 4)\
        << CZ(q[0], q[1])\
        << CZ(q[1], q[2])\
        << Measure(q[0], c[0])\
        << Measure(q[1], c[1])

    # To invoke the real chip computing interface, at least two parameters, quantum
    # program and measurement times, are required. The next three default 
    # parameters are chip type and whether line mapping and line optimization 
    # functions are enabled.
    result = QCM.get_state_fidelity(prog, 1000, real_chip_type.origin_wuyuan_d4)
    print(result)

    QCM.finalize()

Output results are as follows:

::

    0.942748

When applying the Origin Wuyuan real chip for measurement, you often
encounter various errors; the following introduces some error
information, you can find out them according to given error exception
information.

    ● ``server connection failed`` refers to server downtime or server
connection failed

    ● ``api key error`` indicates that the API-Key parameter of the subscriber
is abnormal, please confirm the personnel information on the official
website.

    ● ``un-activate products or lack of computing power`` indicates that the
subscriber does not activate products or is lack of computing power.

    ● ``build system error`` refers to the run error of the compiling system

    ● ``exceeding maximum timing sequence`` refers to the relatively long quantum program timing

    ● ``unknown task status`` refers to the abnormalities of other task states

.. admonition:: Note

    ● Before using the corresponding computing interface, there is a need to ensure that the current subscriber has enabled the product.Otherwise, it is possible not to successfully submit computing tasks.

    ● In the noise simulation, there are respective three single-gate and double-gate parameters of decoherence, which are different from other noise.

    ● The measurement frequency supported by Origin Wuyuan measurement is between 1000 and 10000, and currently only supports the simulation of the quantum circuit of 6 or below qubits. Other quantum chips will be added in the future, please look forward to it.

    ●  If you experience any problems in use, please give user feedback to us. We will solve your problems as soon as possible.


2.10.2 Origin high-performance computing cluster cloud service
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The high-performance computing cluster of Origin Quantum provides
various simulator computing backends with powerful functions, and is
applicable to the simulation requirements for quantum circuits in
different conditions, and the following introduces the complete example
program:

::

    from pyqpanda import *
    import numpy as np

    # Create a quantum cloud simulator machine through QCloud()
    QCM = QCloud()

    # Initialization by passing in the current user's token
    QCM.init_qvm("3B1AC640AAC248C6A7EE4E8D8537370D")

    qlist = QCM.qAlloc_many(6)
    clist = QCM.cAlloc_many(6)

    # Building quantum program. It can be entered manually, from OriginIR or QASM syntax
    # files, etc.
    measure_prog = QProg()
    measure_prog << hadamard_circuit(qlist)\
                 << CZ(qlist[1], qlist[5])\
                 << Measure(qlist[0], clist[0])\
                 << Measure(qlist[1], clist[1])

    pmeasure_prog = QProg()
    pmeasure_prog << hadamard_circuit(qlist)\
                  << CZ(qlist[1], qlist[5])\
                  << RX(qlist[2], np.pi / 4)\
                  << RX(qlist[1], np.pi / 4)\

    # To call the calculation interface of full-amplitude Monte Carlo measurement 
    # operation, two parameters are required: quantum program and measurement times
    result = QCM.full_amplitude_measure(measure_prog, 1000)
    print(result)

2.10.2.1 Full-amplitude simulation cloud computing
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The interface is described as follows:

● ``full_amplitude_measure (full-amplitude Monte Carlo measurement)``:

::

    result0 = QCM.full_amplitude_measure(measure_prog, 100)
    print(result0)

    The second parameter required for input is the number of measurements,
and the output results are given below: the binary representation of the
quantum state on the left and the corresponding probability of the
number of measurements on the right:

::

    {'00': 0.25,
     '01': 0.28,
     '10': 0.22,
     '11': 0.25}


● ``full_amplitude_pmeasure (full-amplitude probability measurement)``:

::

    result1 = QCM.full_amplitude_pmeasure(pmeasure_prog, [0, 1, 2])
    print(result1)

The second parameter required for input is the measurement qubit, and
the output results are given below: the binary representation of the
quantum state on the left and the corresponding probability of the
number of measurements on the right:

::

    {'000': 0.125,
     '001': 0.125,
     '010': 0.125,
     '011': 0.125,
     '100': 0.125,
     '110': 0.125,
     '111': 0.125}

2.10.2.2 Partial-amplitude simulation cloud computing
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

● ``partial_amplitude_pmeasure (partial-amplitude probability measurement)`` :

::

    result2 = QCM.partial_amplitude_pmeasure(pmeasure_prog, ["0", "1", "2"])
    print(result2)

The second parameter required for input is the decimal representation of
the measured quantum state amplitude, and the output results are given
below: the decimal representation of the quantum state on the left and
the plural amplitudes on the right:

::

    {'0': (0.08838832192122936-0.08838833495974541j),
     '1': (0.08838832192122936-0.08838833495974541j),
     '2': (0.08838832192122936-0.08838833495974541j }

2.10.2.3 Single-amplitude cloud computing
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

● ``single_amplitude_pmeasure (single-amplitude probability measurement)``:

::

    result3 = QCM.single_amplitude_pmeasure(pmeasure_prog, "0")
    print(result3)

The second parameter required for input is the measured amplitude
(decimal representation), and the output results are given below: only
the plural amplitude corresponding to one quantum state is output:

::

    (0.08838833056846361-0.08838833850593952j)

2.10.2.4 Noise simulation cloud computing
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

● ``noise_measure (noise simulator measurement)``:

::

    QCM.set_noise_model(NoiseModel.BIT_PHASE_FLIP_OPRATOR, [0.01], [0.02])
    result4 = QCM.noise_measure(measure_prog, 100)
    print(result4)

The noise parameters are set through ``set_noise_model``; the first
parameter is the noise model, and the subsequent parameters are
respectively the single-gate noise parameter and the double-gate noise
parameter, where the noise model is defined as follows:

::

    enum NOISE_MODEL
    {
        DAMPING_KRAUS_OPERATOR,
        DEPHASING_KRAUS_OPERATOR,
        DECOHERENCE_KRAUS_OPERATOR_P1_P2,
        BITFLIP_KRAUS_OPERATOR,
        DEPOLARIZING_KRAUS_OPERATOR,
        BIT_PHASE_FLIP_OPRATOR,
        PHASE_DAMPING_OPRATOR,
        DECOHERENCE_KRAUS_OPERATOR,
        PAULI_KRAUS_MAP,
        KRAUS_MATRIX_OPRATOR,
        MIXED_UNITARY_OPRATOR,
    };

It can be obtained through pyqpanda enumerated ``NoiseModel``; the interface
output results are given below: the binary representation of the quantum
state on the left and the corresponding probability of the number of
measurements on the right:

::

    {'00': 0.26,
     '01': 0.21,
     '10': 0.29,
     '11': 0.24}

2.10.2.5 Get the quantum state tomography results
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    from pyqpanda import *

    qm = QCloud()

    qm.init_qvm("E02BB115D5294012AA88D4BE82603984")

    qlist = qm.qAlloc_many(6)
    clist = qm.cAlloc_many(6)

    prog = QProg()
    prog << hadamard_circuit(qlist)\
        << CZ(qlist[1], qlist[5])\
        << Measure(qlist[0], clist[0])\
        << Measure(qlist[1], clist[1])

    result = qm.get_state_tomography_density(prog, 1000)
    print(result)
    qm.finalize()

Apply the way similar to Monte Carlo measurement, with the output
results as follows:

::

    [[(0.2587544156749868-8.004934191929294e-19j), (0.251211804846972+0.001414451655940455j), (-0.008943457002333129+0.0014876032160007612j), (-0.0040247742512866495+0.007632530135083866j)],
    [(0.2512118048469719-0.001414451655940456j), (0.25003193002089275-6.776263578034404e-19j), (0.0026098957997104447-0.0145657172180014j), (0.001739623577306608+0.003430686695967179j)],
    [(-0.008943457002333132-0.001487603216000763j), (0.002609895799710438+0.0145657172180014j), (0.24548904782784528+2.1684043449710093e-19j), (0.2290859282493824+0.000791060320984212j)],
    [(-0.0040247742512866495-0.007632530135083866j), (0.001739623577306601-0.0034306866959671776j), (0.2290859282493824-0.0007910603209842113j), (0.2457246064762752-2.710505431213761e-20j)]]

.. admonition:: Note

    ●   Before using the corresponding computing interface, there is a need to ensure that the current subscriber has enabled the product. Otherwise, it is possible not to successfully submit computing tasks.

    ●  In the noise simulation, there are respective three single-gate and double-gate parameters of decoherence, which are different from other noise.

    ●  The measurement frequency supported by Origin Wuyuan measurement is between 1000 and 10000, and currently only supports the simulation of the quantum circuit of 6 or below qubits. Other quantum chips will be added in the future, please look forward to it.

    ●  If you experience any problems in use, please give user feedback to us. We will solve your problems as soon as possible.
