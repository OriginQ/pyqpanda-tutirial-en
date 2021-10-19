5 Utility tool
==============

5.1 Quantum circuit query and replacement
-----------------------------------------

In quantum computing, there are some quantum logic gates or quantum
circuits which are interchangeable, including the substitution process
below:

H(0)->CNOT(1,0)->H(0) can be substituted by CZ(1,0).

5.1.1 Introduction to interface used
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There may be multiple sub-quantum circuits with the same structure or
multiple quantum logic gates with the same structure in the quantum
program. The function of querying and substituting the quantum circuits
with the specified structure in the quantum program is to find these
sub-quantum circuits with the same structure and substitute them with
the target quantum circuits.

The ``circuit_optimizer`` provides a unified quantum circuit optimization
interface, which can realize query and replacement of various quantum
circuits. The corresponding interface parameters are as follows:
Parameter 1: QProg parameter 2 of the original quantum program to be
optimized: Vector sublines query replacement queues, and each queue
element contains the target search line and the corresponding
replacement line.

.. Warning:: 
    
    The ``graph_query_replace`` interface is deprecated.

5.1.2 Example
~~~~~~~~~~~~~

::

    from pyqpanda import *
    if __name__=="__main__":
        machine = init_quantum_machine(QMachineType.CPU)
        q = machine.qAlloc_many(4)
        c = machine.cAlloc_many(4)
        # Building quantum programs
        prog = QProg()
        prog << H(q[0])\
            << H(q[2])\
            << H(q[3])\
            << CNOT(q[1], q[0])\
            << H(q[0])\
            << CNOT(q[1], q[2])\
            << H(q[2])\
            << CNOT(q[2], q[3])\
            << H(q[3])
        # Build a query line
        query_cir = QCircuit()
        query_cir << H(q[0])\
                << CNOT(q[1], q[0])\
                << H(q[0])
        # Build alternate lines
        replace_cir = QCircuit()
        replace_cir << CZ(q[1], q[0])
        print("Query before replacement：")
        print(convert_qprog_to_originir(prog,machine))
    # Search the query line in the quantum program and replace it with a
    # replacement line
        update_prog = circuit_optimizer(prog, [[query_cir, replace_cir]])
        print("Query after replacement：")
        print(convert_qprog_to_originir(update_prog,machine))

The running results are as follows:

Query before replacement：

::

    QINIT 4
    CREG 4
    H q[0]
    H q[2]
    H q[3]
    CNOT q[1],q[0]
    H q[0]
    CNOT q[1],q[2]
    H q[2]
    CNOT q[2],q[3]
    H q[3]

Query after replacement：

::

    QINIT 4
    CREG 4
    CZ q[1],q[0]
    CZ q[1],q[2]
    CZ q[2],q[3]

.. Warning:: 
    
    1. The qubits controlled by the queried quantum circuit and the substituted quantum circuit must be one-to-one corresponding.
    
    2.  The directed acyclic graph corresponding to the queried quantum circuit and the substituted quantum circuit must be connected.

5.2 Filling QProg by gate I
---------------------------

The interface ``fill_qprog_by_I`` realizes the function of filling QProg
(quantum program) by I gate.

5.2.1 Example
~~~~~~~~~~~~~

::

    import pyqpanda.pyQPanda as pq
    import math
    from pyqpanda.Visualization.circuit_draw import *
    import numpy as np
    class InitQMachine:
        def __init__(self, quBitCnt, cBitCnt, machineType = pq.QMachineType.CPU):
            self.m_machine = pq.init_quantum_machine(machineType)
            self.m_qlist = self.m_machine.qAlloc_many(quBitCnt)
            self.m_clist = self.m_machine.cAlloc_many(cBitCnt)
        def __del__(self):
            pq.destroy_quantum_machine(self.m_machine)
    def test_fill_I(q, c):
        # Building quantum programs
        prog = pq.QCircuit()
    prog << pq.CU(1, 2, 3, 4, q[0], q[5]) << pq.H(q[0]) << pq.S(q[2])\
     << pq.CNOT(q[0], q[1]) << pq.CZ(q[1], q[2])\
     << pq.CR(q[2], q[1], math.pi/2)
        prog.set_dagger(True)
        # Output the original quantum program
        print('source prog:')
        draw_qprog(prog, 'text',console_encode_type='gbk')
        """
        console_encode_type='utf8' or 'gbk'(默认'utf8')
        """
        # Quantum program filling I gate
        prog = pq.fill_qprog_by_I(prog)
        # Output fill I gate quantum program
        print('The prog after fill_qprog_by_I:')
        draw_qprog(prog, 'text',console_encode_type='gbk')
        draw_qprog(prog, 'pic', filename='D:/test_cir_draw.png')
    if __name__=="__main__":
        init_machine = InitQMachine(16, 16)
        qlist = init_machine.m_qlist
        clist = init_machine.m_clist
        machine = init_machine.m_machine
        test_fill_I(qlist, clist)

The above example program demonstrates how to use the ``fill_qprog_by_I``
interface. We can see that we only need input a parameter of QProg type.
The interface returns a new filled QProg, and the input QProg remains
unchanged. The character drawing of the example program above shows the
output results as below:

.. figure::  /images/5.1.png
   :alt:

5.3 Unitary matrix decomposition
--------------------------------

Currently, a quantum computing algorithm is usually represented by a
quantum circuit which includes quantum logic gate operations. A
continuous quantum circuit generally includes dozens or hundreds or even
thousands of quantum logic gate operations. The larger the number of
quantum logic gates or the number of qubits operated by a single quantum
logic gate, the more complex the computing process is, thus resulting in
low simulation efficiency of quantum circuits and great occupancy of
hardware resources.

5.3.1 Objective of algorithm
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For the above problem, equivalent transformation is necessary for the
quantum circuit and the number of logic gates in the quantum circuit
shall be reduced. Meanwhile, on this basis, we shall ensure that the
unitary matrix corresponding to the whole quantum circuit before the
transformation is exactly the same as that after the transformation.

5.3.2 Overview of algorithm
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The algorithm introduced herein is to decompose a unitary matrix of
order N into no more than r = N(N−1)/2 single-quantum logic gate
sequences with a few controls where N=2^ N, and the decomposed products
satisfy the equation relations below:

.. math::


   U_{r} U_{r-1} \ldots U_{3} U_{2} U_{1} U=I_{N}

Thus, we can obtain the decomposition result of the original matrix U

.. math::


   U=U_{1}^{\dagger} U_{2}^{\dagger} U_{2}^{\dagger} \ldots U_{r-1}^{\dagger} U_{r}^{\dagger}

5.3.3 Instructions
~~~~~~~~~~~~~~~~~~

The pyqpanda is designed with ``matrix_decompose`` interface which is used
for unitary matrix decomposition and requires two parameters: the first
parameter is all the qubits used and the second is the unitary matrix to
be decomposed. The output of this function is the quantum circuit after
transformation.

5.3.4 Example
~~~~~~~~~~~~~

The following example shows how to use the partial-amplitude quantum
simulator.

::

    import pyqpanda as pq
    import numpy as np

    if __name__=="__main__":

        machine = pq.init_quantum_machine(pq.QMachineType.CPU)
        q = machine.qAlloc_many(2)
        c = machine.cAlloc_many(2)

    source_matrix = [(0.6477054522122977+0.1195417767870219j),\ 
    (-0.16162176706189357-0.4020495632468249j),\
    (-0.19991615329121998-0.3764618308248643j),\
    (-0.2599957197928922-0.35935248873007863j),
                        (-0.16162176706189363-0.40204956324682495j), (0.7303014482204584-0.4215172444390785j),\
    (-0.15199187936216693+0.09733585496768032j),\
    (-0.22248203136345918-0.1383600597660744j),
                        (-0.19991615329122003-0.3764618308248644j),\
     (-0.15199187936216688+0.09733585496768032j),\
     (0.6826630277354306-0.37517063774206166j),\
     (-0.3078966462928956-0.2900897445133085j),
                        (-0.2599957197928923-0.3593524887300787j),\
     (-0.22248203136345912-0.1383600597660744j),\
     (-0.30789664629289554-0.2900897445133085j),\
     (0.6640994547408099-0.338593803336005j)]

        print("source matrix : ")
        print(source_matrix)

        out_cir = pq.matrix_decompose(q, source_matrix)
        circuit_matrix = pq.get_matrix(out_cir)

        print("the decomposed matrix : ")
        print(circuit_matrix)

        source_matrix = np.round(np.array(source_matrix),3)
        circuit_matrix = np.round(np.array(circuit_matrix),3)

        if np.all(source_matrix == circuit_matrix):
            print('matrix decompose ok !')
        else:
            print('matrix decompose false !')

The results are as below:

::

    source matrix :
    [(0.6477054522122977+0.1195417767870219j), (-0.16162176706189357-0.4020495632468249j),
    (-0.19991615329121998-0.3764618308248643j), (-0.2599957197928922-0.35935248873007863j),
    (-0.16162176706189363-0.40204956324682495j), (0.7303014482204584-0.4215172444390785j),
    (-0.15199187936216693+0.09733585496768032j), (-0.22248203136345918-0.1383600597660744j),
    (-0.19991615329122003-0.3764618308248644j), (-0.15199187936216688+0.09733585496768032j),
    (0.6826630277354306-0.37517063774206166j), (-0.3078966462928956-0.2900897445133085j),
    (-0.2599957197928923-0.3593524887300787j), (-0.22248203136345912-0.1383600597660744j),
    (-0.30789664629289554-0.2900897445133085j), (0.6640994547408099-0.338593803336005j)]

    the decomposed matrix :
    [(0.6477054522122979+0.11954177678702192j), (-0.16162176706189357-0.402049563246825j),
    (-0.19991615329122003-0.37646183082486445j), (-0.2599957197928924-0.3593524887300788j),
    (-0.16162176706189368-0.40204956324682506j), (0.7303014482204584-0.4215172444390785j),
    (-0.1519918793621669+0.09733585496768038j), (-0.22248203136345918-0.13836005976607446j),
    (-0.19991615329122003-0.3764618308248644j), (-0.151991879362167+0.09733585496768042j),
    (0.6826630277354307-0.37517063774206155j), (-0.30789664629289576-0.2900897445133086j),
    (-0.2599957197928924-0.35935248873007875j), (-0.22248203136345918-0.13836005976607443j),
    (-0.30789664629289576-0.2900897445133086j), (0.6640994547408103-0.3385938033360052j)]

    matrix decompose ok !

Based on the output results, the matrix before transformation is exactly
the same as that after transformation. For a quantum system where the
number of qubits is determined, the interface can control the complexity
of the decomposed quantum circuit within a reasonable range as not
affected by the complexity of the pre-decomposed quantum circuit despite
that the pre-decomposed quantum circuit contains thousands of quantum
logic gates.

.. admonition:: Note
    
    1. The input parameter of the interface must be a unitary matrix.
    
    2. Limiting the number of decomposition results to a limited range effectively reduces the number of quantum logic gates in the quantum circuit and significantly improves the simulation efficiency of the quantum algorithm.
   
    3. In the example program, the ``get_matrix interface`` is used to acquire the corresponding matrix of a quantum circuit.