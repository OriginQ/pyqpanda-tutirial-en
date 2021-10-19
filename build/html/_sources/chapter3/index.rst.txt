3 Quantum program information
=============================


3.1 NodeIter
------------

NodeIter is QProg or QCircuit iterator externally provided by pyQPanda.
We can control our quantum programs conveniently through NodeIter.

3.1.1 Interface introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

At present, NodeIter has the main operations below:

obtaining the next node   

::

    iter = iter.get_next()

obtaining the previous node

::

    iter = iter.get_pre()

obtaining the node type

::

    type = iter.get_node_type()

configuring QProg through iterator

::

    type = iter.get_node_type()
    if pq.NodeType.PROG_NODE == type:
        prog = pq.QProg(iter)

configuring the quantum circuit QCircuit through iterator

::

    type = iter.get_node_type()
    if pq.NodeType.CIRCUIT_NODE == type:
        cir = pq.QCircuit(iter)

configuring QGate through iterator

::

    type = iter.get_node_type()
    if pq.NodeType.GATE_NODE == type:
        gate = pq.QGate(iter)

configuring QIfProg through iterator

::

    type = iter.get_node_type()
    if pq.NodeType.QIF_START_NODE == type:
        if_prog = pq.QIfProg(iter)

configuring QWhileProg through iterator

::

    type = iter.get_node_type()
    if pq.NodeType.WHILE_START_NODE == type:
        while_prog = pq.QWhileProg(iter)

configuring QMeasure through iterator

::

    type = iter.get_node_type()
    if pq.NodeType.MEASURE_GATE == type:
        measure_gate = pq.QMeasure(iter)

3.1.2 Example
~~~~~~~~~~~~~

The following example program has the function to browse one QProg
through NodeIter and output the types of node logic gates:

::

    import pyqpanda.pyQPanda as pq
    import math

    machine = pq.init_quantum_machine(pq.QMachineType.CPU)
    q = machine.qAlloc_many(8)
    c = machine.cAlloc_many(8)
    prog = pq.QProg()

    prog << pq.H(q[0]) << pq.S(q[2]) << pq.CNOT(q[0], q[1])\
     << pq.CZ(q[1], q[2]) << pq.CR(q[1], q[2], math.pi/2)
    iter = prog.begin()
    iter_end = prog.end()
    while  iter != iter_end:
        if pq.NodeType.GATE_NODE == iter.get_node_type():
            gate = pq.QGate(iter)
            print(gate.gate_type())
        iter = iter.get_next()
    else:
        print('Traversal End.\n')

    pq.destroy_quantum_machine(machine)

Reverse browsing:

::

    import pyqpanda.pyQPanda as pq
    import math

    machine = pq.init_quantum_machine(pq.QMachineType.CPU)
    q = machine.qAlloc_many(8)
    c = machine.cAlloc_many(8)
    prog = pq.QProg()

    prog << pq.H(q[0]) << pq.S(q[2]) << pq.CNOT(q[0], q[1])\
     << pq.CZ(q[1], q[2]) << pq.CR(q[1], q[2], math.pi/2)
    iter_head = prog.head()
    iter = prog.last()
    while  iter != iter_head:
        if pq.NodeType.GATE_NODE == iter.get_node_type():
            gate = pq.QGate(iter)
            print(gate.gate_type())
        iter = iter.get_pre()
    else:
        print('Traversal End.\n')

3.2 Logic gate statistics
-------------------------

3.2.1 Introduction
~~~~~~~~~~~~~~~~~~

Logic gate statistics refers to a method for counting the number of the
quantum logic gates (including measurement gates) in quantum programs,
quantum circuits, quantum cycle control or quantum condition control.

3.2.2 Interface introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We create one quantum program firstly through pyqpanda:

::

    prog = QProg()
    prog << X(qubits[0]) << Y(qubits[1])\
         << H(qubits[0]) << RX(qubits[0], 3.14)\
         << Measure(qubits[0], cbits[0])

invoke ``get_qgate_num`` to count the number of quantum logic gates;

::

    number = get_qgate_num(prog)

.. admonition:: Note

    The method of counting the number of quantum logic gates in ``QCircuit``, ``QWhileProg`` and ``QIfProg`` is similar to the method of counting ``QProg``.



3.2.3 Example
~~~~~~~~~~~~~

::

    from pyqpanda import *

    if __name__ == "__main__":
        qvm = init_quantum_machine(QMachineType.CPU)
        qubits = qvm.qAlloc_many(2)
        cbits = qvm.cAlloc_many(2)

        prog = QProg()

        # Building quantum programs
        prog << X(qubits[0]) << Y(qubits[1])\
            << H(qubits[0]) << RX(qubits[0], 3.14)\
            << Measure(qubits[0], cbits[0])

        # Statistics the number of logical gates
        number = get_qgate_num(prog)
        print("QGate number: " + str(number))

        qvm.finalize()

Output results are as follows:

::

    QGate number: 4

.. Warning:: 

    The interface name in the new version is changed, and the old interface ``count_gate`` is replaced by ``get_qgate_num``. Please note that ``count_gate`` will be removed in the next version.

3.3 Counting quantum program clock cycle
----------------------------------------

3.3.1 Introduction
~~~~~~~~~~~~~~~~~~

Estimate the running time of a quantum program under the known running
time of the quantum logic gates. The time of the quantum logic gates is
set to the item metadata configuration file ``QPandaConfig.xml``; a default
is given if such time is not set, where the time of single quantum gate
is defaulted as 2, and that of the double quantum gates is defaulted as
5.

The configuration file can be set according to the following example:

::

    "QGate": {
        "SingleGate":{
            "U3":{"time":1}
        },
        "DoubleGate":{
            "CNOT":{"time":2},
            "CZ":{"time":2}
        }
    }

3.3.2 Interface introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We create one quantum program firstly through pyqpanda:

::

    prog = QProg()
    prog << H(qubits[0]) << CNOT(qubits[0], qubits[1])\
         << iSWAP(qubits[1], qubits[2]) << RX(qubits[3], np.pi / 4)

invoke ``get_qprog_clock_cycle`` interface to get the clock cycle of the
quantum program

::

    clock_cycle = get_qprog_clock_cycle(qvm, prog)

3.3.3 Example
~~~~~~~~~~~~~

::

    from pyqpanda import *
    import numpy as np

    if __name__ == "__main__":
        qvm = init_quantum_machine(QMachineType.CPU)
        qubits = qvm.qAlloc_many(4)
        cbits = qvm.cAlloc_many(4)

        # Building quantum programs
        prog = QProg()
        prog << H(qubits[0]) << CNOT(qubits[0], qubits[1])\
             << iSWAP(qubits[1], qubits[2]) << RX(qubits[3], np.pi / 4)

        # Statistical quantum program clock cycle
        clock_cycle = get_qprog_clock_cycle(prog, qvm)

        print(clock_cycle)
        destroy_quantum_machine(qvm)

Output results are as follows:

::

    5

.. Warning:: 

    The interface name in the new version is changed, and the old interface ``get_clock_cycle`` will be replaced by ``get_qprog_clock_cycle``. Please note that ``get_clock_cycle`` will be removed in the next version.


3.4 Get the corresponding matrix of the quantum circuit
-------------------------------------------------------

The ``get_matrix`` interface can get the corresponding matrix of the input
circuit and has three output parameters, where one parameter is quantum
circuit QCircuit (or Qprog), and other two parameters are optional
parameters: the starting and ending positions of the iterator are used
for designating one circuit interval to get corresponding matrix
information; if the parameters are null, the matrix information of the
entire quantum circuit is obtained.

.. admonition:: Note

    It should be noted in use of ``get_matrix`` that the quantum circuit does not involve measurement.


3.4.1 Example
~~~~~~~~~~~~~

::

    import pyqpanda.pyQPanda as pq
    import math

    class InitQMachine:
    def __init__(self, quBitCnt, cBitCnt,\
     machineType = pq.QMachineType.CPU):
            self.m_machine = pq.init_quantum_machine(machineType)
            self.m_qlist = self.m_machine.qAlloc_many(quBitCnt)
            self.m_clist = self.m_machine.cAlloc_many(cBitCnt)
            self.m_prog = pq.QProg()

        def __del__(self):
            pq.destroy_quantum_machine(self.m_machine)

    def test_get_matrix(q, c):
        prog = pq.QProg()

        # Building quantum programs
        prog << pq.H(q[0]) \
            << pq.S(q[2]) \
            << pq.CNOT(q[0], q[1]) \
            << pq.CZ(q[1], q[2]) \
            << pq.CR(q[1], q[2], math.pi/2)

        # Obtain the line corresponding matrix
        result_mat = pq.get_matrix(prog)

        # Print matrix information
        pq.print_matrix(result_mat)

    if __name__=="__main__":
        init_machine = InitQMachine(16, 16)
        qlist = init_machine.m_qlist
        clist = init_machine.m_clist
        machine = init_machine.m_machine

        test_get_matrix(qlist, clist)
        print("Test over.")

The specific steps are as follows:

1. Initialize a quantum simulator object with ``init_quantum_machine`` (pq.QMachineType.CPU) in order to manage a series of subsequent behaviors

2. Initialize the number of qubits and classical registers with ``machine.qAlloc_many()`` and  ``machine.cAlloc_many()``, where respective 8 qubits and classical register are applied.

3. Create prog

4. Determine the circuit interval of computing matrix information, namely setting of ``iter_start`` and ``iter_end``

5. Invoke ``get_matrix`` interface to output the corresponding matrix of the quantum circuit, and print obtained matrix information through ``print_mat`` interface.


3.5 Judge whether quantum logic gate matches with the quantum topology
----------------------------------------------------------------------

Each quantum chip has its own particular qubit topology, for example,
``ibmq_16_melbourne`` provided by IBMQ:

.. figure::  /images/3.1.png
   :alt:

It can be seen from the Figure that the qubits in the quantum chip are
not connected in pairs, and those not interconnected cannot directly
execute multi-gate operations. Therefore, it is necessary to judge
whether the double-gate (multi-gate) operation in the quantum program is
in conformity with the qubit topology prior to the execution of the
quantum program.

3.5.1 Interface introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``is_match_topology`` is to judge whether the quantum logic gate is in
conformity with the qubit topology. The first input parameter is the
target quantum logic gate QGate, the second input parameter is the qubit
topology, and the returned value is Boolean value. This indicates
whether the target quantum logic gate is in conformity with the qubit
topology. True means that the target quantum logic gate is in conformity
with the qubit topology. False means that the target quantum logic gate
is not in conformity with the qubit topology.

::

    import pyqpanda.pyQPanda as pq
    import math

    class InitQMachine:
    def __init__(self, quBitCnt, cBitCnt,\
    machineType = pq.QMachineType.CPU):
            self.m_machine = pq.init_quantum_machine(machineType)
            self.m_qlist = self.m_machine.qAlloc_many(quBitCnt)
            self.m_clist = self.m_machine.cAlloc_many(cBitCnt)
            self.m_prog = pq.QProg()

        def __del__(self):
            pq.destroy_quantum_machine(self.m_machine)

    def test_is_match_topology(q, c):
        cx = pq.CNOT(q[1], q[3])

        # Building a Topology
        qubits_topology = \
    [[0,1,0,0,0],[1,0,1,1,0],[0,1,0,0,0],[0,1,0,0,1],[0,0,0,1,0]]

        # Determine whether the logic gate conforms to the quantum topology
        if (pq.is_match_topology(cx,qubits_topology)) == True:
            print('Match !\n')
        else:
            print('Not match.')

    if __name__=="__main__":
        init_machine = InitQMachine(16, 16)
        qlist = init_machine.m_qlist
        clist = init_machine.m_clist
        machine = init_machine.m_machine
        test_is_match_topology(qlist, clist)
        print("Test over.")

Prior to the use of ``is_match_topology``, the adjacency matrix
``qubits_topology`` of the qubit topology of the designated quantum chip
should be created.

It can be seen from the above example, ``qubits_topology`` includes 5
qubits. The qubit topological diagram is shown below:

.. figure::  /images/3.2.png
   :alt: 

CNOT logic gate operates qubits 1# and 3#. However, it can be seen from
the topological diagram that qubits 1# and 3# are interconnected, and
therefore the result should be true.

3.6 Get adjacent quantum logic gates at the designated position.
----------------------------------------------------------------

The ``get_adjacent_qgate_type`` interface can get designated adjacent
quantum logic gates in the quantum program. The first input parameter is
the target quantum program QProg, the second input parameter is the
iterator of the target quantum logic gate in the quantum program, and
the returned result is a set of iterators of adjacent target quantum
logic gates.

3.6.1 Example
~~~~~~~~~~~~~

::

    import pyqpanda.pyQPanda as pq
    import math

    class InitQMachine:
        def __init__(self, quBitCnt, cBitCnt, machineType = pq.QMachineType.CPU):
            self.m_machine = pq.init_quantum_machine(machineType)
            self.m_qlist = self.m_machine.qAlloc_many(quBitCnt)
            self.m_clist = self.m_machine.cAlloc_many(cBitCnt)
            self.m_prog = pq.QProg()

        def __del__(self):
            pq.destroy_quantum_machine(self.m_machine)

    def test_get_adjacent_qgate_type(qlist, clist):
        prog = pq.QProg()

        # Building quantum programs
        prog << pq.T(qlist[0]) \
            << pq.CNOT(qlist[1], qlist[2]) \
            << pq.Reset(qlist[1]) \
            << pq.H(qlist[3]) \
            << pq.H(qlist[4])

        iter = prog.begin()
        iter = iter.get_next()
        type =iter.get_node_type()
        if pq.NodeType.GATE_NODE == type:
            gate = pq.QGate(iter)
            print(gate.gate_type())

        # Gets the types of logical gates before and after the specified location
        list = pq.get_adjacent_qgate_type(prog,iter)
        print(len(list))
        print(len(list[0].m_target_qubits))
        print(list[1].m_is_dagger)

        node_type = list[0].m_node_type
        print(node_type)
        if node_type == pq.NodeType.GATE_NODE:
            gateFront = pq.QGate(list[0].m_iter)
            print(gateFront.gate_type())

        node_type = list[1].m_node_type
        print(node_type)
        if node_type == pq.NodeType.GATE_NODE:
            gateBack = pq.QGate(list[1].m_iter)
            print(gateBack.gate_type())

    if __name__=="__main__":
        init_machine = InitQMachine(16, 16)
        qlist = init_machine.m_qlist
        clist = init_machine.m_clist
        machine = init_machine.m_machine
        test_get_adjacent_qgate_type(qlist, clist)
        print("Test over.")

The above example shows how to use ``get_adjacent_qgate_type`` interface:

1. Create one quantum program prog;

2. Designate position information, namely setting of iter

3. Invoke ``get_adjacent_qgate_type`` interface to get a set of iterators
   of adjacent iter logic gates. The types of obtained logic gates are
   respectively printed in the last four rows of the example code

When using ``get_adjacent_qgate_type`` interface, we need to note the
following points:

1. A set of the iterators of adjacent target quantum logic gates always
   includes two elements; The first element is an iterator of the
   previous quantum logic gate, and the second element is an iterator of
   the next logic gate.

2. If the target quantum logic gate is the first node of the quantum
   program, only the iterator of the following target quantum logic gate
   can be obtained in a set of iterators of adjacent target quantum
   logic gates (as the output parameter), and the first element in the
   set is the null iterator.

3. If the target quantum logic gate is the last quantum logic gate in
   the quantum program, only the iterator of the previous target quantum
   logic gate can be obtained in a set of iterators of adjacent target
   quantum logic gates (as the output parameter), and the second element
   in the set is the null iterator.

4. If the previous node of the target quantum logic gate is QIf or
   QWhile, only the iterator of the following target quantum logic gate
   can be obtained in a set of iterators of adjacent target quantum
   logic gates (as the output parameter), and the first element in the
   set is the null iterator.

5. If the following node of the target quantum logic gate is QIf or
   QWhile, only the iterator of the previous target quantum logic gate
   can be obtained in a set of iterators of adjacent target quantum
   logic gates (as the output parameter), and the second element in the
   set is the null iterator.

6. If the target quantum logic gate is the first quantum logic gate of
   QWhile, only the iterator of the following target quantum logic gate
   can be obtained in a set of iterators of adjacent target quantum
   logic gates (as the output parameter), and the first element in the
   set is the null iterator.

7. If the target quantum logic gate is the last quantum logic gate of
   QWhile, only the iterator of the previous target quantum logic gate
   can be obtained in a set of iterators of adjacent target quantum
   logic gates (as the output parameter), and the second element in the
   set is the null iterator.

3.7 Judge whether two quantum logic gates are interchanged for their positions
------------------------------------------------------------------------------

The ``is_swappable`` interface can judge whether two designated quantum
logic gates in the quantum program are interchanged for their positions.
The first input parameter is the quantum program QProg; the second and
third input parameters are the iterators of two quantum logic gates to
be judged. The returned value is Boolean value; True represents the
interchangeable position; and False represents the non-interchangeable
position.

3.7.1 Example
~~~~~~~~~~~~~

The following example demonstrates how to use ``is_swappable`` interface.

1. Create a quantum program prog. Here, a slightly complex quantum
   program with nested nodes is enumerated;

2. Get iterators at two designated positions of the nested node cir:
   iter\_first and iter\_second;

3. Invoke ``is_swappable`` interface to judge whether two designated logic
   gates are interchanged for their positions, and output changeable
   judgement results on the console.

::

    import math

    class InitQMachine:
    def __init__(self, quBitCnt, cBitCnt,\
     machineType = pq.QMachineType.CPU):
            self.m_machine = pq.init_quantum_machine(machineType)
            self.m_qlist = self.m_machine.qAlloc_many(quBitCnt)
            self.m_clist = self.m_machine.cAlloc_many(cBitCnt)
            self.m_prog = pq.QProg()

        def __del__(self):
            pq.destroy_quantum_machine(self.m_machine)

    # Test interface: judge whether two designated logic gates are interchanged
    # for their positions
    def test_is_swappable(q, c):
        prog = pq.QProg()
        cir = pq.QCircuit()
        cir2 = pq.QCircuit()
    cir2 << pq.H(q[3]) << pq.RX(q[1], math.pi/2) << pq.T(q[2])\
    << pq.RY(q[3], math.pi/2) << pq.RZ(q[2], math.pi/2)
        cir2.set_dagger(True)
        cir << pq.H(q[1]) << cir2 << pq.CR(q[1], q[2], math.pi/2)
        prog << pq.H(q[0]) << pq.S(q[2]) \
        << cir\
        << pq.CNOT(q[0], q[1]) << pq.CZ(q[1], q[2]) << pq.measure_all(q,c)

        iter_first = cir.begin()

        iter_second = cir2.begin()
        #iter_second = iter_second.get_next()
        #iter_second = iter_second.get_next()
        #iter_second = iter_second.get_next()

        type =iter_first.get_node_type()
        if pq.NodeType.GATE_NODE == type:
            gate = pq.QGate(iter_first)
            print(gate.gate_type())

        type =iter_second.get_node_type()
        if pq.NodeType.GATE_NODE == type:
            gate = pq.QGate(iter_second)
            print(gate.gate_type())

        if (pq.is_swappable(prog, iter_first, iter_second)) == True:
            print('Could be swapped !\n')
        else:
            print('Could NOT be swapped.')

    if __name__=="__main__":
        init_machine = InitQMachine(16, 16)
        qlist = init_machine.m_qlist
        clist = init_machine.m_clist
        machine = init_machine.m_machine

        test_is_swappable(qlist, clist)
        print("Test over.")

3.8 Judge whether the logic gates are of a set of quantum logic gates supported by the quantum chip
---------------------------------------------------------------------------------------------------

A set of quantum logic gates supported by the quantum chip can be
configured in the metadata configuration file QPandaConfig.xml. If we do
not set the configuration files, QPanda sets a default set of quantum
logic gates.

The default collection is as follows:

::

    single_gates.push_back("RX");
    single_gates.push_back("RY");
    single_gates.push_back("RZ");
    single_gates.push_back("X1");
    single_gates.push_back("H");
    single_gates.push_back("S");

    double_gates.push_back("CNOT");
    double_gates.push_back("CZ");
    double_gates.push_back("ISWAP");

The configuration file can be set according to the following example:

::

    "QGate": {
        "SingleGate":{
            "U3":{"time":1}
        },
        "DoubleGate":{
            "CNOT":{"time":2},
            "CZ":{"time":2}
        }
    }

It can be seen from the above examples that the quantum chip supports
RX, RY, RZ, S, H, X1, CNOT, CZ and ISWAP gates. We may call the
interface ``is_supported_qgate_type`` upon the configuration of the
configuration file, to determine whether the logic gate falls into the
quantum logic gate set that the quantum chip supports. The interface
``is_supported_qgate_type`` only has one parameter: target quantum logic
gate.

::

    import pyqpanda.pyQPanda as pq
    import math

    class InitQMachine:
    def __init__(self, quBitCnt, cBitCnt,\
     machineType = pq.QMachineType.CPU):
            self.m_machine = pq.init_quantum_machine(machineType)
            self.m_qlist = self.m_machine.qAlloc_many(quBitCnt)
            self.m_clist = self.m_machine.cAlloc_many(cBitCnt)
            self.m_prog = pq.QProg()

        def __del__(self):
            pq.destroy_quantum_machine(self.m_machine)

    def test_support_qgate_type():
        machine = pq.init_quantum_machine(pq.QMachineType.CPU)
        q = machine.qAlloc_many(8)
        c = machine.cAlloc_many(8)

        prog = pq.QProg()
        prog << pq.H(q[1])
        result = pq.is_supported_qgate_type(prog.begin())
        if result == True:
            print('Support !\n')
        else:
            print('Unsupport !')

    if __name__=="__main__":
        init_machine = InitQMachine(16, 16)
        qlist = init_machine.m_qlist
        clist = init_machine.m_clist
        machine = init_machine.m_machine

        test_support_qgate_type()
        print("Test over.")

.. admonition:: Note

    The user may access the default configuration file QPandaConfig.json through the following link, and place it under the same directory as the executive program, which will automatically parse the file.

3.9 Character drawing of quantum circuit
----------------------------------------

At present, PyQPanda provides three visualization methods of quantum
circuit. Please refer to the following examples for specific usage
methods.

3.9.1 Example
~~~~~~~~~~~~~

::

    import pyqpanda.pyQPanda as pq
    from pyqpanda.Visualization.circuit_draw import *
    import math
    class InitQMachine:
        def __init__(self, quBitCnt, cBitCnt, machineType = pq.QMachineType.CPU):
            self.m_machine = pq.init_quantum_machine(machineType)
            self.m_qlist = self.m_machine.qAlloc_many(quBitCnt)
            self.m_clist = self.m_machine.cAlloc_many(cBitCnt)

        def __del__(self):
            pq.destroy_quantum_machine(self.m_machine)

    def test_print_qcircuit(q, c):
        # Building quantum programs
        prog = pq.QCircuit()
        prog << pq.CU(1, 2, 3, 4, q[0], q[5]) << pq.H(q[0]) << pq.S(q[2])\
             << pq.CNOT(q[0], q[1]) << pq.CZ(q[1], q[2])\
     << pq.CR(q[2], q[1], math.pi/2)
        prog.set_dagger(True)

        print('draw_qprog:')

    # Through print directly outputs the character drawing of quantum
    # circuit. This method will output the quantum circuit in the console,
    # and the output format is uft8 encoding. Therefore, in the console 
    # with non-uft8 encoding, the character drawing of output will be 
    # garbled.

    # At the same time, this method will save the current quantum circuit
    #character drawing information to the file named  QCircuitTextPic.txt
    # the file is encoded with uft8 and saved under the face path.
    # Therefore, users can also view quantum circuit information through 
    # this file. Note that the file must be opened in uft8 format, 
    # otherwise garbled characters will appear.
        print(prog)
    # Output quantum circuit character drawing through draw_qprog 
    # interface. The function of this method is the same as print method,
    # but the difference is that this interface can specify the console 
    # encoding type to ensure that the quantum circuit character drawing 
    # output on the console can be displayed normally.
    # The "console_encode_type" parameter is used to specify the console 
    # type. Currently, two encoding modes are supported: utf8 and gbk. The
    # default is utf8
        draw_qprog(prog, 'text', console_encode_type='gbk')
    # The draw_qprog interface can also save the quantum circuit as a 
    # picture, called as follows. The “filename” parameter is used to 
    # specify a filename to save.
        draw_qprog(prog, 'pic', filename='D:/test_cir_draw.png')

    if __name__=="__main__":
        init_machine = InitQMachine(16, 16)
        qlist = init_machine.m_qlist
        clist = init_machine.m_clist
        machine = init_machine.m_machine

        test_print_qcircuit(qlist, clist)

The above example demonstrates the ``draw_qprog`` interface, respectively.
The output from the above code is as follows:

.. figure::  /images/3.3.png
   :alt:

The output quantum circuit picture effect is as follows:

.. figure::  /images/3.6.png
   :alt:


The parameters of ``draw_qprog()`` are described as follows:

::

     """Draw a quantum circuit to different formats (set by output parameter):

    **text**: ASCII art TextDrawing that can be printed in the console.

    **pic**: images with color rendered purely in Python.

    Args:
        prog : the quantum circuit to draw
        scale (float): scale of image to draw (shrink if < 1). Only used by the ``pic`` outputs.
        filename (str): file path to save image to
        NodeIter_first: circuit printing start position.
        NodeIter_second: circuit printing end position.
        console_encode_type(str): Target console encoding type.
            Mismatching of encoding types may result in character confusion, 'utf8' and 'gbk' are supported.
            Only used by the ``pic`` outputs.
        line_length (int): Sets the length of the lines generated by `text` output type.

    Returns: no return

    """
    def draw_qprog(prog, output=None, scale=0.7, filename=None, line_length=None, NodeIter_first=None, \
    NodeIter_second=None, console_encode_type = 'utf8'):

As a demonstration, we change the ``test_print_qcircuit()`` interface
implementation from the example code above to the following:

::

    prog = pq.QCircuit()
    prog << pq.CU(1, 2, 3, 4, q[0], q[5]) << pq.H(q[0]) << pq.S(q[2]) << pq.CNOT(q[0], q[1]) << pq.CZ(q[1], q[2]) << pq.CR(q[2], q[1], math.pi/2)
    iter_start = prog.begin()
    iter_end = iter_start.get_next()
    iter_end = iter_end.get_next()
    iter_end = iter_end.get_next()
    prog.set_dagger(True)
    draw_qprog(prog, 'text', NodeIter_first=iter_start, NodeIter_second=iter_end, console_encode_type='gbk')
    draw_qprog(prog, 'pic', NodeIter_first=iter_start, NodeIter_second=iter_end, filename='D:/test_cir_draw.jpg')

The above example code will output only the first 4 logical gate nodes
of prog, users can replace the above code to the previous example
program, run to view the results, not repeated here.

3.10 Quantum volume
-------------------

3.10.1 Introduction
~~~~~~~~~~~~~~~~~~~

Quantum volume is a protocol used to evaluate the performance of the
quantum computing system. It represents the random circuit with the
maximum equal width depth that can be performed on the system. The
higher the operating fidelity of the quantum computing system is, the
higher the correlation is; the larger the gate operation set calibrated
is, the higher the quantum volume is. Quantum volume relates to the
overall performance of the system, including the overall error rate of
the system, potential physical qubit correlation, and gate operation
parallelism. Generally speaking, quantum volume is practical method for
making an overall evaluation of the quantum computing system in the near
future; the higher the value is, the lower the overall error rate of the
system is, the better the performance is.

To measure the quantum volume in a standard way is to perform random
circuit operations for the system with specified quantum circuit model,
entangle qubits as far as possible, and compare the experimental results
with the simulated results. The statistical results are analyzed as
required.

Quantum volume is defined in the exponential form:

.. math::


   V_{Q}=2^{n}

Where n is the maximum logical depth of system operation under the given
number of qubits m (m is greater than n) and the completion of computing
tasks; if the maximum executive logical depth of the chip n is greater
than the number of qubits m, the quantum volume of the system is

.. math:: 2^M

3.10.2 Interface description
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The input parameters of ``calculate_quantum_volume`` are the noise
simulator or quantum cloud machine, qubit to be measured, number of
random iterations, and number of measurements. The output is size\_t,
which is the quantum volume size.

3.10.3 Example
~~~~~~~~~~~~~~

::

    from pyqpanda import *

    if __name__=="__main__":
        # Create a noise simulator and set noise parameters
        qvm = NoiseQVM()
        qvm.init_qvm()
        qvm.set_noise_model(NoiseModel.DEPOLARIZING_KRAUS_OPERATOR, GateType.CZ_GATE, 0.005)

    # You can also apply for cloud computing machines (using real chips), using real chips
    # to consider the chip structure
        #qvm = QCloud()
        #qvm.init_qvm("898D47CF515A48CEAA9F2326394B85C6")

    # Construct the qubit combination to be measured, where the qubit combination is two 
    # groups; the qubit 3 and 4 are a group, qubits 2, 3, and 5 are a group.
    qubit_lists = [[3,4], [2,3,5]]

        # Set the number of random iterations
        ntrials = 100

        # Set the measurement times, namely real chip or noise simulator shots value
        shots = 2000
        qv_result = calculate_quantum_volume(qvm, qubit_lists, ntrials, shots)
        print("Quantum Volume : ", qv_result)
        qvm.finalize()

Running results:

::

    Quantum Volume ： 8

3.11 Random benchmark
---------------------

3.11.1 Introduction
~~~~~~~~~~~~~~~~~~~

Random benchmark (RB) is a benchmark test for quantum gates with a
randomization method. Since the complete process tomography doesn't work
for the large system, more attention is paid to the extensible method,
to characterize noise that affects the quantum system. An extensible and
robust algorithm (a system comprising n qubits) is put forward in
`[1] <https://arxiv.org/pdf/1009.3639>`__, i.e. benchmark test is
performed for the whole Clifford gate by a single parameter with the
randomizing technique.

3.11.2 Interface description
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The input parameters of ``single_qubit_rb`` are the noise simulator or
quantum cloud machine, qubit to be measured, a different number of
combinations of random circuit clifford gate sets, number of random
circuits, number of measurements, verification of basic logic gate
(default: none), output being ``std::map`` data, key value being the number
of clifford gate sets, and value corresponding to the expected
probability.

The input parameters of ``double_qubit_rb`` are the noise simulator or
quantum cloud machine, qubit 0 to be measured, qubit 1 to be measured, a
different number of combinations of random circuit clifford gate sets,
number of random circuits, number of measurements, verification of basic
logic gate (default: none), output being ``std::map`` data, key value being
the number of clifford gate sets, and value corresponding to the
expected probability.

::

    from pyqpanda import *

    if __name__=="__main__":
        # Build a noise simulator and adjust the noise to simulate the real chip
        qvm = NoiseQVM()
        qvm.init_qvm()
        qvm.set_noise_model(NoiseModel.DEPOLARIZING_KRAUS_OPERATOR, GateType.CZ_GATE, 0.005)
    qvm.set_noise_model(NoiseModel.DEPOLARIZING_KRAUS_OPERATOR, GateType.PAULI_Y_GATE,\ 0.005)
        qv = qvm.qAlloc_many(4)

        # You can also apply for cloud computing machines (with real chips)
        # qvm =  QCloud()
        # qvm.init_qvm("898D47CF515A48CEAA9F2326394B85C6")

        # Set the number of Clifford gates in a random line
        range = [ 5,10,15 ]

        # Measuring a single qubit random reference
        res = single_qubit_rb(qvm, qv[0], range, 10, 1000)

        # It is also possible to measure two-qubit random datum
        #res = double_qubit_rb(qvm, qv[0], qv[1], range, 10, 1000)

    # With the influence of noise, the larger the noise value is, the smaller the result
    # is. And with the increase of Clifford gate set, the smaller the result is.
        print(res)

        qvm.finalize()

3.11.3 Example
~~~~~~~~~~~~~~

Running results:

::

    {5: 0.9996, 10: 0.9999, 15: 0.9993000000000001}

3.12 Cross-entropy benchmark
----------------------------

3.12.1 Introduction
~~~~~~~~~~~~~~~~~~~

Cross-entropy benchmark (xeb) is a method used to evaluate the gate
performance by applying the random circuits and measuring the
cross-entropy between the observed values of bit strings and the
expected probability of such bit strings obtained from the simulation.

3.12.2 Interface description
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The input parameters of ``double_gate_xeb`` are the noise simulator or
quantum cloud machine, qubit 0 to be measured, qubit 1 to be measured,
different layers of circuits, number of random circuits, number of
measurements, verification of double-gate type (default: CZ gate),
output being std::map data, key value being the number of circuit
layers, and value corresponding to the expected probability.

3.12.3 Example
~~~~~~~~~~~~~~

::

    from pyqpanda import *

    if __name__=="__main__":

        # Build a noise simulator and adjust the noise to simulate the real chip
        qvm = NoiseQVM()
        qvm.init_qvm()
        qv = qvm.qAlloc_many(4)

        # Setting noise Parameters
        qvm.set_noise_model(NoiseModel.DEPOLARIZING_KRAUS_OPERATOR, GateType.CZ_GATE, 0.1)

        # You can also apply for cloud computing machines (with real chips)
        # qvm =  QCloud()
        # qvm.init_qvm("898D47CF515A48CEAA9F2326394B85C6")

        # Set different layer combinations
        range = [2,4,6,8,10]
        # Currently, the main testable dual-gate types are CZ CNOT SWAP ISWAP SQISWAP
        res = double_gate_xeb(qvm, qv[0], qv[1], range, 10, 1000, GateType.CZ_GATE)
    # With the influence of noise, the larger the noise value is, the smaller the result
    # will be; and the number of layer increases, the smaller the result will be.

        print(res)

        qvm.finalize()

Running results:

::

    {2: 0.9922736287117004, 4: 0.9303175806999207, 6: 0.7203856110572815, 8: 0.7342230677604675, 10: 0.7967881560325623}
