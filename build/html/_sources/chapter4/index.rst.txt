4 Compiling of quantum program
==============================

4.1 Conversion of QASM by a quantum program
-------------------------------------------

You may parse the quantum program created by QPanda2 with this function
module, and extract the qubit information and quantum logic gate
operation information contained therein, to obtain the QASM instruction
set saved in a fixed form.

4.1.1 QASM introduction
~~~~~~~~~~~~~~~~~~~~~~~

Quantum Assembly Language is put forward by IBM, and is similar to the
syntax rules in the QRunes introduction;
a QASM code segment is as follows:

::

    OPENQASM 2.0;
    include "qelib1.inc";
    qreg q[10];
    creg c[10];

    x q[0];
    h q[1];
    tdg q[2];
    sdg q[2];
    cx q[0],q[2];
    cx q[1],q[4];
    u1(pi) q[0];
    u2(pi,pi) q[1];
    u3(pi,pi,pi) q[2];
    cz q[2],q[5];
    ccx q[3],q[4],q[6];
    cu3(pi,pi,pi) q[0],q[1];
    measure q[2] -> c[2];
    measure q[0] -> c[0];

It should be noted that the syntax formats of QASM and QRunes are alike
but different, with the following differences:

• For quantum logic gates and quantum circuits requiring the transposed conjugate, QRunes needs to place the target between DAGGER and ENDAG-GER sentences; QASM does the conversion directly.

• QRunes supports the control operation of the quantum logic gates and quantum circuits, but QASM doesn't. Before the conversion of QASM by a quantum program, the control operation contained therein will be decomposed.

Refer to `IBM Q Experience quantum cloud platform for more details on
the introduction, use, and experience of
QASM <https://quantumexperience.ng.bluemix.net/qx/editor>`__.

QPanda2 provides the QASM conversion tool interface
``convert_qprog_to_qasm``, which is easy to use. Reference can be made to
the following example program.

4.1.2 Example
~~~~~~~~~~~~~

The following routine demonstrates the process of converting QASM
instruction set by a quantum program through a simple interface call.

::

    from pyqpanda import *

    if __name__ == "__main__":
        qvm = init_quantum_machine(QMachineType.CPU)
        q = qvm.qAlloc_many(6)
        c = qvm.cAlloc_many(6)
        prog = QProg()
        cir = QCircuit()
        cir << T(q[0]) << S(q[1]) << CNOT(q[1], q[0])
        prog << cir
        prog << X(q[0]) << Y(q[1]) << CU(1.2345, 3, 4, 5, q[5], q[2])\
            << H(q[2]) << RX(q[3], 3.14)\
            << Measure(q[0], c[0])

        qasm = convert_qprog_to_qasm(prog, qvm)
        print(qasm)
        qvm.finalize()

The specific steps are as follows:

• Firstly, initialize a quantum simulator object with ``init_quantum_machine`` in the main program, in order to manage a series of subsequent behaviors

• Then, initialize the number of qubits and classical registers with ``qAlloc_many`` and ``cAlloc_many``.

• Next, call ``QProg`` to create the quantum program.

• Finally, the interface ``convert_qprog_to_qasm`` is called to output the QASM instruction set; ``finalize ()`` is used to release system resources

The running results are as follows:

::

    OPENQASM 2.0;
    include "qelib1.inc";
    qreg q[6];
    creg c[6];
    u3(0,0.78539816339744828,0) q[0];
    u3(0,1.5707963267948966,0) q[1];
    cx q[1],q[0];
    u3(3.1415926535897931,0,3.1415926535897931) q[0];
    u3(3.1415926535897931,0,0) q[1];
    u3(0,-0.33629632679489674,0) q[5];
    u3(0,-0.67259265358979359,0) q[2];
    cx q[5],q[2];
    u3(0,0.33629632679489674,0) q[2];
    cx q[5],q[2];
    u3(1.1415926535897929,3.1415926535897931,2.8672963267948974) q[2];
    u3(0,1.5707963267948963,0) q[5];
    cx q[5],q[2];
    u3(1.1415926535897929,-1.1947036732051033,0) q[2];
    cx q[5],q[2];
    u3(1.5707963267949034,0,-1.3362963267948968) q[2];
    u3(3.1400000000000001,-1.5707963267948966,1.5707963267948966) q[3];  
    measure q[0] -> c[0];

4.2 Conversion into a quantum program by QASM
---------------------------------------------

You may parse the QASM text file with this function module, and extract
the quantum logic gate operation information therein, to obtain the
quantum program that is operable in QPanda 2.

4.2.1 QASM introduction
~~~~~~~~~~~~~~~~~~~~~~~

Reference can be made to the `QASM
introduction <https://qpanda-tutorial.readthedocs.io/zh/latest/QASMToQProg.html#id1>`__
in the module of converting QASM by a quantum program for the writing
format specifications and routines of QASM.

QPanda 2 provides the QASM file conversion tool interface
``convert_qasm_to_qprog()``, which is easy to use. Reference can be made
to the following example program.

4.2.2 Example
~~~~~~~~~~~~~

The following demonstrates the process of converting the quantum program
by QASM instruction set through a simple interface call.

::

    from pyqpanda import *

    if __name__=="__main__":
    machine = init_quantum_machine(QMachineType.CPU)

    # Write QASM files
    f = open('testfile.txt', mode='w',encoding='utf-8')
    f.write("""// test QASM file
    OPENQASM 2.0;
    include "qelib1.inc";
    qreg q[3];
    creg c[3];
    x q[0];
    x q[1];
    z q[2];
    h q[0];
    tdg q[1];
    measure q[0] -> c[0];
    """)
    f.close()

    # QASM converts quantum programs and returns quantum programs, quantum bits,
    # and classical registers
    prog_trans, qv, cv = convert_qasm_to_qprog("testfile.txt", machine)

    # Quantum program conversion QASM
    qasm = convert_qprog_to_qasm(prog_trans,machine)

    # Print and compare the conversion results
    print(qasm)
    destroy_quantum_machine(machine)

The specific steps are as follows:

• Firstly, compile QASM and save it in a designated file

• Then, initialize a quantum simulator object with ``init_quantum_machine`` in the main program, in order to manage a series of subsequent behaviors

• Next, call the ``convert_qasm_to_qprog`` interface to convert QASM into a quantum program

• Finally, call the ``convert_qprog_to_qasm`` interface to convert the quantum program into QASM, decide whether QASM is converted into the quantum program correctly by comparing the execution results of the quantum program, and release system resources by ``destroy_quantum_machine``

The running results are as follows:

::

    OPENQASM 2.0;
    include "qelib1.inc";
    qreg q[3];
    creg c[3];
    u3(1.5707963267949037,3.1415926535897931,3.1415926535897931) q[0];
    u3(3.1415926535897931,2.3561944901923386,0) q[1];
    u3(0,3.1415926535897931,0) q[2];
    measure q[0] -> c[0];

.. admonition:: Note

    In the above example, since QASM supports U3 gate, the quantum circuit is optimized when QProg is converted to QASM, and only U3 gate is in the output QASM, which can effectively reduce the quantum circuit depth. For the types that are not supported temporarily, errors may occur during the process of converting QASM into a quantum program.



4.3 Conversion into Quil by a quantum program
---------------------------------------------

4.3.1 Introduction
~~~~~~~~~~~~~~~~~~

Quil can directly describe quantum programs and quantum algorithms from
a very low level, which is similar to the hardware description language
or assembly language in classical computers. Quil basically adopts the
design method of "instruction + parameter list". An example of a simple
quantum program is as follows:

::

    X 0
    Y 1
    CNOT 0 1
    H 0
    RX(-3.141593) 0
    MEASURE 1 [0]

● The purpose of ``X`` is to perform ``Pauli-X`` gate operations on target qubits.
Similar keywords include ``Y``, ``Z``, ``H``, etc.

● The purpose of ``Y`` is to perform ``Pauli-Y`` gate operations on target
qubits.

● The purpose of ``CNOT`` is to perform ``CNOT`` operations on two qubits. The
input parameters are the control qubit serial number and target qubit
serial number.

● The purpose of ``H`` is to perform ``Hadamard`` gate operations on target
qubits.

● The purpose of ``MEASURE`` is to measure target qubits and save the
measuring results in a classical register. The input parameters are
serial numbers of target qubits and classical registers where the
measuring results are saved.

The above is just a small part of Quil instruction set syntax. Reference
can be made to
`pyQuil <https://pyquil.readthedocs.io/en/stable/compiler.html>`__ for
more details.

4.3.2 Interface introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We create a quantum program with pyqpanda at first:

::

    prog = QProg()
    prog << X(qubits[0]) << Y(qubits[1])\
        << H(qubits[2]) << RX(qubits[3], 3.14)\
        << Measure(qubits[0], cbits[0])

Then ,call the ``convert_qprog_to_quil`` for the transformation

::

    quil = convert_qprog_to_quil(prog, qvm)

4.3.3 Example
~~~~~~~~~~~~~

::

    from pyqpanda import *

    if __name__ == "__main__":
        qvm = init_quantum_machine(QMachineType.CPU)
        qubits = qvm.qAlloc_many(4)
        cbits = qvm.cAlloc_many(4)
        prog = QProg()

        # Building quantum programs
        prog << X(qubits[0]) << Y(qubits[1])\
            << H(qubits[2]) << RX(qubits[3], 3.14)\
            << Measure(qubits[0], cbits[0])

        # The quantum program converts the Quil and prints the Quil
        quil = convert_qprog_to_quil(prog, qvm)
        print(quil)

        qvm.finalize()

Running results:

::

    X 0
    Y 1
    H 2
    RX(3.140000) 3
    MEASURE 0 [0]

.. Warning:: 

    The new interface ``convert_qprog_to_quil()`` is provided with the same function as the old one ``transform_qprog_to_quil()``.


4.4 Serialization of quantum programs
-------------------------------------

4.4.1 Introduction
~~~~~~~~~~~~~~~~~~

A protocol is defined to serialize quantum programs to binary data for
the convenience of saving and transmitting quantum programs.

4.4.2 Interface introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We create a quantum program with pyqpanda at first:

::

    prog = QProg()
    prog << H(qubits[0]) \
        << CNOT(qubits[0], qubits[1]) \
        << CNOT(qubits[1], qubits[2]) \
        << CNOT(qubits[2], qubits[3])

Next, call the ``convert_qprog_to_binary`` interface for serialization.

::

    prog_str = convert_qprog_to_binary(prog, qvm)



.. admonition:: Note

    Quantum program serialization is a two-step process that first serializes a quantum program into a binary and then converts the binary into a string encoded in base 64 format.



4.4.3 Example
~~~~~~~~~~~~~

::

    from pyqpanda import *
    import base64

    if __name__ == "__main__":
        qvm = init_quantum_machine(QMachineType.CPU)
        qubits = qvm.qAlloc_many(4)
        cbits = qvm.cAlloc_many(4)

        # Building quantum programs
        prog = QProg()
        prog << H(qubits[0]) \
            << CNOT(qubits[0], qubits[1]) \
            << CNOT(qubits[1], qubits[2]) \
            << CNOT(qubits[2], qubits[3])

        # Quantum program serialization
        binary_data = convert_qprog_to_binary(prog, qvm)

        # The resulting binary data is encoded in Base64 and printed
        str_base64_data =  base64.encodebytes(bytes(binary_data))
        print(str_base64_data)

        destroy_quantum_machine(qvm)

Running results:

::

    b'AAAAAAQAAAAEAAAABAAAAA4AAQAAAAAAJAACAAAAAQAkAAMAAQACACQABAACAAMA\n'

.. admonition:: Note

    The binary data cannot be output directly and should be subject to base64 encoding, to obtain corresponding character strings.

.. Warning:: 

    The new interface ``convert_qprog_to_binary()`` is provided with the same function as the old one ``transform_qprog_to_binary()``.



4.5 Parse quantum program binary files
--------------------------------------

4.5.1 Introduction
~~~~~~~~~~~~~~~~~~

Parse quantum programs that are serialized by the quantum program
serialization method.

4.5.2 Interface introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We create a quantum program with pyqpanda at first:

::

    prog = QProg()
    prog << H(qubits[0]) \
        << CNOT(qubits[0], qubits[1]) \
        << CNOT(qubits[1], qubits[2]) \
        << CNOT(qubits[2], qubits[3])

After the serialization and base64 encoding, the result is (refer to the
quantum program serialization for specific serialized method).

::

    b'AAAAAAQAAAAEAAAABAAAAA4AAQAAAAAAJAACAAAAAQAkAAMAAQACACQABAACAAMA\n'

Now, deserialized this result. Firstly, decode the base64 result into
binary data:

::

    str_base64_data =b'AAAAAAQAAAAEAAAABAAAAA4AAQAAAAAAJAACAAAAAQAkAAMAAQACACQABAACAAMA\n'
    data = [int(x) for x in bytes(base64.decodebytes(str_base64_data))]

We may use one interface encapsulated by QPanda2:

::

    convert_binary_data_to_qprog(qvm, data, qubits_parse, cbits_parse, parseProg);

4.5.3 Example
~~~~~~~~~~~~~

::

    from pyqpanda import *
    import base64

    if __name__ == "__main__":
        qvm = init_quantum_machine(QMachineType.CPU)

        # Base64 decoding of the way to get binary data
        str_base64_data =\ b'AAAAAAQAAAAEAAAABAAAAA4AAQAAAAAAJAACAAAAAQAkAAMAAQACACQABAACAAMA\n';
        data = [int(x) for x in bytes(base64.decodebytes(str_base64_data))]

        # Parse binary data, get quantum programs
        parseProg = QProg()
        parseProg = convert_binary_data_to_qprog(qvm, data)

        # The quantum program converts OriginIR and prints it
        print(convert_qprog_to_originir(parseProg,qvm))

        destroy_quantum_machine(qvm)

Running results:

::

    QINIT 4
    CREG 4
    H q[0]
    CNOT q[0],q[1]
    CNOT q[1],q[2]
    CNOT q[2],q[3]

.. admonition:: Note

    If the running results are correct, serialized quantum programs can beparsed correctly


.. Warning:: 

    The new interface ``convert_binary_data_to_qprog()`` is provided with the same function as the old one ``transform_binary_data_to_qprog()`` .



4.6 Conversion into a quantum program by OriginIR
-------------------------------------------------

You may parse the OriginIR text file with this function module, and
extract the quantum logic gate operation information therein, to obtain
the quantum program that is operable in QPanda 2.

4.6.1 OriginIR
~~~~~~~~~~~~~~

Reference can be made to the OriginIR introduction in the module of
converting OriginIR by a quantum program for the writing format
specifications and routines of OriginIR.

QPanda 2 provides the OriginIR file conversion tool interface
``convert_originir_to_qprog()`` ,which is easy to use. Reference can be
made to the following example program.

4.6.2 Example
~~~~~~~~~~~~~

The following demonstrates the process of converting the quantum program
by OriginIR through a simple interface call.

::

    from pyqpanda import *
    if __name__=="__main__":
        machine = init_quantum_machine(QMachineType.CPU)

        # Write OriginIR files
        f = open('testfile.txt', mode='w',encoding='utf-8')
        f.write("""QINIT 4
            CREG 4
            DAGGER
            X q[1]
            X q[2]
            CONTROL q[1], q[2]
            RY q[0], (1.047198)
            ENDCONTROL
            ENDDAGGER
            MEASURE q[0], c[0]
            QIF c[0]
            H q[1]
            H q[2]
            RZ q[2], (2.356194)
            CU q[2], q[3], (3.141593, 4.712389, 1.570796, -1.570796)
            CNOT q[2], q[1]
            ENDQIF
            """)

        f.close()

    # OriginIR converts a quantum program and returns the converted quantum 
    # program, the quantum bits used by the quantum program, and the classical
    # registers
    prog, qv, cv = convert_originir_to_qprog("testfile.txt", machine)

    # The quantum program converts OriginIR, prints and compares the conversion
    # results
        print(convert_qprog_to_originir(prog,machine))

        destroy_quantum_machine(machine)

The specific steps are as follows:

● Firstly, compile OriginIR and save it in a designated file

● Then, initialize a quantum simulator object with
``init_quantum_machine`` in the main program in order to manage a series
of subsequent behaviors

● Next, call ``convert_originir_to_qprog()`` for the conversion

● Finally, call the ``convert_qprog_to_originir()`` to convert the
quantum program into OriginIR, decide whether OriginIR is converted into
the quantum program correctly by comparing whether the QASMs input and
generated are the same, and release system resources by
``destroy_quantum_machine``

The running results are as follows:

::

    QINIT 4
    CREG 4
    DAGGER
    X q[1]
    X q[2]
    CONTROL q[1],q[2]
    RY q[0],(1.047198)
    ENCONTROL
    ENDDAGGER
    MEASURE q[0],c[0]
    QIF c[0]
    H q[1]
    H q[2]
    RZ q[2],(2.356194)
    CU q[2],q[3],(3.141593,4.712389,1.570796,-1.570796)
    CNOT q[2],q[1]
    ENDQIF

.. admonition:: Note

    For the types that are not supported temporarily, errors may occur during the process of converting OriginIR into a quantum program.


.. Warning:: 

   The new interface ``convert_originir_to_qprog()`` is provided with the same function as the old one ``transform_originir_to_QProg()``.


4.7 Conversion of OriginIR by a quantum program
-----------------------------------------------

You may parse the quantum program created by pyqpanda with this function
module, and extract the qubit information and quantum logic gate
operation information contained therein, to obtain the OriginIR saved in
a fixed form.

4.7.1 OriginIR introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~

OriginIR is an intermediate representation of quantum programs based on
QPanda, and plays an important role in supporting properties of QPanda.
OriginIR not only indicates most of the quantum logic gate types,
indicates dagger operation for quantum circuits, and adds control qubits
for quantum circuits, but also supports Qif and QWhile unique to QPanda
and implements classical programs where quantum programs are embedded.

OriginIR mainly includes qubit, classical register, quantum logic gate,
transposed conjugate operation, adding control qubit operation, QIf,
QWhile, and classical expression.

4.7.1.1 Qubit
^^^^^^^^^^^^^

OriginIR applies for qubits with QINIT. The format is QINIT followed by
space + the total number of qubits, like QINIT 6. It should be noted
that QINIT must be at the first row of the OriginIR program, except
notes. When qubits are used, OriginIR uses q[i] to indicate a certain
specific qubit; i is the No. of the qubit, and can be a unsigned
numerical constant or a variable; it can also use an alternate
expression comprising c[i], like q[1], q[c[0]], q[c[1]+c[2]+c[3]].

4.7.1.2 Classical register
^^^^^^^^^^^^^^^^^^^^^^^^^^

OriginIR applies for classical registers with CREG. The format is CREG
followed by space + the total number of classical registers, like CREG
6; when the classical registers are used, OriginIR uses c[i] to indicate
a certain specific classical register; i is the No. of the classical
register, and must be an unsigned numeric constant, like c[1].

4.7.1.3 Quantum logic gate
^^^^^^^^^^^^^^^^^^^^^^^^^^

OriginIR divides quantum logic gates into the following categories:
keywords of the single-gate without parameters, single-gate with one
parameter, single-gate with two parameters, single-gate with three
parameters, single-gate with four parameters, double-gate without
parameters, double-gate with one parameter, double-gate with four
parameters, and tri-gate without parameters. It should be noted that for
all single-gate operations, the target qubit can be the entire qubit
array or a single qubit. In case of the entire qubit array, for example:

::

    H q

When the qubit array size is 3, it is equivalent to:

::

    H q[0]
    H q[1]
    H q[2]

1.Keywords of the single-gate without parameters: H, T, S, X, Y, Z, X1,
Y1, Z1, and I; indicating the single-quantum logic gate without
parameters; the format is quantum logic gate keyword + space + target
qubit. Example:

::

    H q[0]

2.Keywords of the single-gate with one parameter: RX, RY, RZ, and U1;
indicating the single-quantum logic gate with one parameter; the format
is quantum logic gate keyword + space + target qubit + comma +
(deflection angle). Example:

::

    RX q[0],(1.570796)

3.Keywords of the single-gate with two parameters: U2 and RPhi;
indicating the single-quantum logic gate with two parameters; the format
is quantum logic gate keyword + space + target qubit + comma + (two
deflection angles). Example:

::

    U2 q[0],(1.570796,-3.141593)

4.Keyword of the single-gate with three parameters: U3; indicating the
single-quantum logic gate with three parameters; the format is quantum
logic gate keyword + space + target qubit + comma + (three deflection
angles). Example:

::

    U3 q[0],(1.570796,4.712389,1.570796)

5.Keyword of the single-gate with four parameters: U4; indicating the
single-quantum logic gate with four parameters; the format is quantum
logic gate keyword + space + target qubit + comma + (four deflection
angles). Example:

::

    U4 q[1],(3.141593,4.712389,1.570796,-3.141593)

6.Keywords of the double-gate without parameters: CNOT, CZ, ISWAP,
SQISWAP, and SWAP; indicating the double-quantum logic gate without
parameters; the format is quantum logic gate keyword + space + control
qubit + comma + target qubit. Example:

::

    CNOT q[0],q[1]

7.Keywords of the double-gate with one parameter: ISWAPTHETA and CR;
indicating the single-quantum logic gate with one parameter; the format
is quantum logic gate keyword + space + control qubit + comma + target
qubit + comma + (deflection angle). Example:

::

    CR q[0],q[1],(1.570796)

8.Keyword of the double-gate with four parameters: CU; indicating the
single-quantum logic gate with four parameters; the format is quantum
logic gate keyword + space + control qubit + comma + target qubit +
comma + (four deflection angles). Example:

::

    CU q[1],q[3],(3.141593,4.712389,1.570796,-3.141593)

9.Keyword of the tri-gate without parameters: TOFFOLI; indicating the
tri-quantum logic gate without parameters; the format is quantum logic
gate keyword + space + control qubit 1 + comma + control qubit 2 + comma
+ target qubit. Example:

::

    TOFFOLI  q[0],q[1],q[2]

4.7.1.4 Transposed conjugate operation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The transposed conjugate operation can be performed for one or more
quantum logic gates in OriginIR, which defines the range of transposed
conjugate operation with keywords, i.e. DAGGER and ENDDAGGER. One DAGGER
must match with one ENDDAGGER; for example:

::

    DAGGER
    H q[0]
    CNOT q[0],q[1]
    ENDDAGGER

4.7.1.5 Adding control qubit operation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The control qubits can be added for one or more quantum logic gates in
OriginIR, which defines the range of adding control qubits with
keywords, i.e. CONTROL and ENDCONTROL. CONTROL is followed by space +
control qubit list; Example:

::

    CONTROL q[2],q[3]
    H q[0]
    CNOT q[0],q[1]
    ENDCONTROL

4.7.1.6 QIF
^^^^^^^^^^^

The judgment programs for quantum conditions can be indicated in
OriginIR, which determines the range of different ranches of judgment
programs for quantum conditions by QIF, ELSE, and ENDIF. QIF must match
with one ENDIF; if QIF has two branches, ELSE is required; if QIF only
has one branch, ELSE is not required; QIF is followed by space +
judgment expression. Example:

::

    1、QIF There's only one conditional branch
    QIF c[0]==c[1]
    H q[0]
    CNOT q[0],q[1]
    ENDIF

    2、QIF There are two conditional branches
    QIF c[0]+c[1]<5
    H q[0]
    CNOT q[0],q[1]
    ELSE
    H q[0]
    X q[1]
    ENDIF

4.7.1.7 QWHILE
^^^^^^^^^^^^^^

The judgment programs for quantum loops can be indicated in OriginIR,
which determines the range of judgment programs for loops by QIF, ELSE,
and ENDIF; QWHILE is followed by space + judgment expression. Example:

::

    QWHILE c[0]<5
    H q[c[0]]
    c[0]=c[0]+1
    ENDQWHILE

4.7.1.8 Classical expression
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

OriginIR can embed a classical expression in the quantum program, like
c[0]==c[1]+c[2]; Example:

::

    QWHILE c[0]<5
    H q[c[0]]
    c[0]=c[0]+1
    ENDQWHILE

The expression indicates the H gate operation performed for q[0]~q[4]
qubit; the classical expression must be comprised of classical registers
and constants; the operators of the classical expression include.

::

    {PLUS , "+"},
    {MINUS, "-"},
    {MUL, "*"},
    {DIV, "/"},
    {EQUAL, "==" },
    { NE, "!=" },
    { GT, ">" },
    { EGT, ">=" },
    { LT, "<" },
    { ELT, "<=" },
    {AND, "&&"},
    {OR, "||"},
    {NOT, "!"},
    {ASSIGN, "=" }

4.7.1.9 MEASURE operation
^^^^^^^^^^^^^^^^^^^^^^^^^

MEASURE indicates the measurement operation performed for designated
qubits, and saving the results to the designated classical register.
MEASURE is followed by space + target qubit + ',' + target classical
register. Example:

::

    MEASURE q[0],c[0]

If the number of qubits applied is the same as that of the classical
registers, q can be used to indicate all qubits, and c indicates all
classical bits. Example:

::

    MEAUSRE q,c

If the number of qubits and the number of classical bits are 3
respectively, it is equivalent to

::

    MEAUSRE q[0],c[0]
    MEAUSRE q[1],c[1]
    MEAUSRE q[2],c[2]

4.7.1.10 RESET operation
^^^^^^^^^^^^^^^^^^^^^^^^

RESET operation is to reset the quantum state of qubits operated to
zero. The format is RESET + space + target qubit where the target qubit
can be the entire qubit array or a single qubit. Example:

::

    RESET q

    RESET q[1]

4.7.1.11 BARRIER operation
^^^^^^^^^^^^^^^^^^^^^^^^^^

BARRIER operation is to block the qubits operated, so as to prevent the
optimization and execution on the circuit. The format is BARRIER + space
+ target qubit where the target qubit can be the entire qubit array or a
single qubit or multiple qubits. Example:

::

    BARRIER q
    BARRIER q[0]
    BARRIER q[0],q[1],q[2]

4.7.1.12 QGATE operation
^^^^^^^^^^^^^^^^^^^^^^^^

QGATE is user-defined logic gate operation, during which multiple logic
gates can be combined into a new one. The scope of user-defined logic
gates is framed through QGATE and ENDQGATE. It should be noted that the
parameter name of the user-defined logic gate can not conflict with any
of the related keywords as described above.

The rules for declaration of user-defined logic gates are as below:

::

    QGATE UserDefinedeGateName BitParameter,(angle)
    //UserDefinedeGateName, user-defined logical gate name，string
    //BitParameter, user-defined logical gate parameter information，string
    //angle, angle information，string
    // Other relevant information[","、"(" etc.]It must be written in the defined format
    // Among them ",” and subsequent information can be blank, the angle information can be null

A simplified example is shown below:

::

    QGATE new_H a
    H a
    X a
    ENDQGATE


    QGATE new_RX a,(b)
    RX a,(PI/2+b)
    CONTROL q[0]
    RX a,(-3.141593)
    DAGGER
    H a
    ENDDAGGER
    ENDCONTROL
    DAGGER
    H a
    DAGGER
    H a
    ENDDAGGER
    ENDDAGGER
    ENDQGATE

The user, after applying for qubits and classical registers, can call
user-defined logic gates in the following format:

::

    UserDefinedeGateName  argue,(angle)
    //UserDefinedeGateName, user-defined logical gate name，string, be consistent with the definition section above
    //BitParameter, user-defined logical gate parameter information，string，It must be q[x], and x needs to be less than the number of qubits requested
    //angle, angle information，string，it can be a number, or an expression related to PI

A simplified example is shown below:

::

    new_H q[0]
    new_RX q[1],(PI/4)

4.7.1.13 OriginIR program example
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

OPE algorithm

::

    QINIT 3
    CREG 2
    H q[2]
    H q[0]
    H q[1]
    CONTROL q[1]
    RX q[2],(-3.141593)
    ENCONTROL
    CONTROL q[0]
    RX q[2],(-3.141593)
    RX q[2],(-3.141593)
    ENCONTROL
    DAGGER
    H q[1]
    CR q[0],q[1],(1.570796)
    H q[0]
    ENDDAGGER
    MEASURE q[0],c[0]
    MEASURE q[1],c[1]

QPanda2 provides an OriginIR conversion interface (std::string
``convert_qprog_to_originir()`` which is easy to use. See the following
example program for details.

4.7.2 Example
~~~~~~~~~~~~~

The following example demonstrates the process of converting OriginIR by
a quantum program through a simple interface call.

::

    from pyqpanda import *

    if __name__ == "__main__":
        machine = init_quantum_machine(QMachineType.CPU)
        qlist = machine.qAlloc_many(4)
        clist = machine.cAlloc_many(4)
        prog = create_empty_qprog()
        prog_cir = create_empty_circuit()

        # Building quantum circuits
        prog_cir << Y(qlist[2]) << H(qlist[2]) << CNOT(qlist[0],qlist[1])

        # Build a QWhile that uses quantum circuitry for loop branching
        qwhile = create_while_prog(clist[1], prog_cir)

        # Build the quantum program and insert QWhile into the quantum program
        prog << H(qlist[2]) << Measure(qlist[1],clist[1]) << qwhile

        # The quantum program converts the QriginIR and prints the OriginI
        print(convert_qprog_to_originir(prog,machine))

        destroy_quantum_machine(machine)

The specific steps are as follows:

● Firstly, initialize a quantum simulator object with
``init_quantum_machine`` in the main program in order to manage a series
of subsequent behaviors.

● Then, initialize the number of qubits and classical registers with
``qAlloc_many`` and ``cAlloc_many``.

● Next, call ``create_empty_qprog`` to create the quantum program.

● Finally, Call the ``convert_qprog_to_originir`` interface to output the
OriginIR，string and use ``destroy_quantum_machine`` to release system
resources.

The running results are as follows:

::

    QINIT 4
    CREG 4
    H q[2]
    MEASURE q[1],c[1]
    QWHILE c[1]
    Y q[2]
    H q[2]
    CNOT q[0],q[1]
    ENDQWHILE


.. admonition:: Note

    For operations which are not currently supported, OriginIR will display UnSupported XXXNode where XXX indicates the type of nodes.


.. Warning:: 

   The new interface ``convert_qprog_to_originir()`` is provided with the same function as the old one ``transform_qprog_to_originIR()``.


4.8 Quantum program matching topology
-------------------------------------

A quantum computing device has finite connections between qubits,
allowing only two qubit gates to be applied to finite pairs of qubits.
When the quantum program is applied to the target device, the original
quantum program must be converted to adapt to the hardware restriction
to allow the two qubits in the double qubit gates in compliance with the
physical topology, thus to ensure normal operation of the double qubit
gates. Most of the existing solutions require inserting additional SWAP
operations between two qubits failing to interact in order to "move" the
logic qubits to a position where they can interact. This solution is
called quantum program matching topology.

4.8.1 Interface description
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Two matching topology methods are available in the current version:

``Interface topology_match``

By adopting circuit layering and A\* search algorithms, the number of
inserted SWAP operations is approximately minimized in the process of
matching thus to minimize the overall approximate consumption of the
algorithms. The interface requires input of 5 parameters: the quantum
program constructed, the set of qubits used, the simulator pointer
initialized, the mode of SWAP operation used, and the type of topology.
And also the interface can return to the mapped quantum program.

4.8.2 Example
~~~~~~~~~~~~~

::

    from pyqpanda import *

    if __name__=="__main__":
        qvm = CPUQVM()
        qvm.init_qvm()
        qv = qvm.qAlloc_many(16)
        c = qvm.cAlloc_many(16)
        src_prog = QProg()
        # Building quantum programs
        src_prog << CNOT(qv[0], qv[3]) \
                << CNOT(qv[0], qv[2]) \
                << CNOT(qv[1], qv[3]) \
                << CZ(qv[1], qv[2]) \
                << CZ(qv[0], qv[2]) \
                << T(qv[1])  \
                << S(qv[2])  \
                << H(qv[3])

        # The probability measurement of src_prog results in results_1
        qvm.directly_run(src_prog)
        results_1 = qvm.pmeasure_no_index(qv)

    # The quantum program out_prog matching IBM_QX5_ARCH topology is obtained
    # by topology matching src_prog
        out_prog, out_qv = topology_match(src_prog, qv, qvm, CNOT_GATE_METHOD,\ IBM_QX5_ARCH)

        # The probability of out_prog is measured and the result is results_2
        qvm.directly_run(out_prog)
        results_2 = qvm.pmeasure_no_index(out_qv)

    # Compare the probability measurement results_1 and Results_2, and print
    # the same result
        len = min(len(results_1), len(results_2))
        for index in range(len):
            if abs(results_1[index] - results_2[index]) < 1.0e-6 :
                print(results_1[index])
            else:
                print("The results are different")
        destroy_quantum_machine(qvm)

The specific steps are as follows:

● Firstly, create a quantum simulator, a quantum register and a
classical register.

● Then, write the quantum program ``src_prog``, and measure the probability
of the program to obtain the ``result_1``.

● Next, call ``topology_match()`` to map ``src_prog`` to a circuit in
compliance with the specific physical structure thus to get the quantum
program ``out_prog`` matching with the specific physical structure.

● Finally, measure the probability of the program ``out_prog`` to obtain
the ``result_2``.

● The mapping of quantum program, as an additional ``SWAP`` operation to the
original circuit to adapt to the physical topology, does not affect the
structure of the circuit. Therefore, the mapping of the circuit is
correct if ``result_1`` and ``result_2`` are the same.

The results are as below:

::

    0.49999999999999645
    The results are different
    0.0
    0.0
    0.0
    0.0
    0.0
    0.0
    The results are different
    0.0
    0.0
    0.0
    0.0
    0.0
    0.0
    0.0
