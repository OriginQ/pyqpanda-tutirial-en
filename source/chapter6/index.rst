6 Component
===========

6.1 Class of Pauli operators
----------------------------

The Pauli operators are a set of three 2 × 2 complex matrices which are
Hermitian and unitary, also called unitary matrices. The matrices are
usually indicated by the Greek letter σ (sigma), denoted as

.. |X| image:: /images/6.1.png
   :width: 50px
   :height: 50px

.. |Y| image:: /images/6.2.png
   :width: 50px
   :height: 50px
   
.. |Z| image:: /images/6.3.png
   :width: 50px
   :height: 50px

====================== ======================= ==========================================================================
| |X|                       | :math:`\sigma_x`                 | :math:`\begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}\quad`
| |Y|                       | :math:`\sigma_y`                 | :math:`\begin{bmatrix} 0 & -i \\ i & 0 \end{bmatrix}\quad`
| |Z|                       | :math:`\sigma_z`                 | :math:`\begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}\quad`
====================== ======================= ==========================================================================

The operational rules of Pauli operators are as below:

1. The Pauli operators are multiplied by themselves to get an identity
   matrix.

.. math::


   \begin{aligned}
   &\sigma_{x} \sigma_{x}=I \\
   &\sigma_{y} \sigma_{y}=I \\
   &\sigma_{z} \sigma_{z}=I
   \end{aligned}

2. The Pauli operators remain unchanged when multiplied (either
   pre-multiplied or post-multiplied) by the identity matrix.

   .. math::


         \begin{aligned}
         &\sigma_{x} \mathrm{I}=\mathrm{I} \sigma_{x}=\sigma_{x} \\
         &\sigma_{y} \mathrm{I}=\mathrm{I} \sigma_{y}=\sigma_{y} \\
         &\sigma_{z} \mathrm{I}=\mathrm{I} \sigma_{z}=\sigma_{z}
         \end{aligned}
         

3. Two Pauli operators are multiplied in sequence to be i times as large
   as the operators not involved in the computing.

   .. math::


         \begin{aligned}
         &\sigma_{x} \sigma_{y}=\mathrm{i} \sigma_{z} \\
         &\sigma_{y} \sigma_{z}=\mathrm{i} \sigma_{x} \\
         &\sigma_{z} \sigma_{x}=\mathrm{i} \sigma_{y}
         \end{aligned}
         

4. Two Pauli operators are multiplied in inverted sequence to be −i
   times as large as the operators not involved in the computing.

   .. math::


         \begin{aligned}
         \sigma_{y} \sigma_{x} &=-\mathrm{i} \sigma_{z} \\
         \sigma_{z} \sigma_{y} &=-\mathrm{i} \sigma_{x} \\
         \sigma_{x} \sigma_{z} &=-\mathrm{i} \sigma_{y}
         \end{aligned}
         

6.1.1 Interface introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

According to the above properties of the Pauli operators, we implement
``PauliOperator`` in ``pyQPanda``. We can construct the class of Pauli operators
easily, for example

::

    from pyqpanda import *

    if __name__=="__main__":
        # Construct an empty PauliOperator class
        p1 = PauliOperator()

        # Two times the tensor product of Pauli Z0 Pauli Z1
        p2 = PauliOperator("Z0 Z1", 2)

    #2 times the Pauli Z0 tensor product Pauli Z1 plus 3 times the Pauli X1
    # tensor product Pauli Y2
        p3 = PauliOperator({"Z0 Z1": 2, "X1 Y2": 3})

    # Construct an identity matrix with coefficients of 2, equivalent to p4
    # = PauliOperator("", 2)
        p4 = PauliOperator(2)

Where PauliOperator p2("Z0 Z1", 2) represents

.. math::  2 \sigma_{0}^{Z} \otimes \sigma_{1}^{Z}

.. admonition:: Note
  For construction of the class of Pauli operators, the string therein can contain only one or more of the following characters: space, X, Y and Z. Inclusion of any other character will throw exceptions. In addition, the qubit index of the same Pauli operator in the same string shall not be the same. For example, PauliOperator("Z0 Z0", 2) will throw exceptions.



Pauli operators can be added, subtracted, multiplied or subjected to
other operations, and the computed return result still belongs to the
class of Pauli operators.

::

    a = PauliOperator("Z0 Z1", 2)
    b = PauliOperator("X5 Y6", 3)

    plus = a + b
    minus = a - b
    muliply = a * b

The class of Pauli operators supports printing, and we can print the
class of Pauli operators on the screen to view their values.

::

    a = PauliOperator("Z0 Z1", 2)
    print(a)

In actual applications, we often need to know how many qubits are
operated by the class of Pauli operators, which can be obtained by
calling the getMaxIndex interface of the class of Pauli operators. If an
empty class of Pauli operators calls the getMaxIndex interface, the
result returned will be 0. Otherwise, the result will be the maximum
subscript index value plus 1.

::

    a = PauliOperator("Z0 Z1", 2)
    b = PauliOperator("X5 Y6", 3)
    # The output value is 2
    print(a.getMaxIndex())
    # The output value is 7
    print(b.getMaxIndex())

We construct a class of Pauli operators where the subscript index of the
Pauli operator is not allocated from 0. For example, the number of
qubits returned and used by the getMaxIndex interface called by
PauliOperator b("X5 Y6", 3) is 7, but actually only 2 qubits are used.
How can we return the number of qubits actually used? We can call the
remapQubitIndex interface in the class of Pauli operators which is used
to allocate and map the indexes in the Pauli operator from 0 qubit and
returns the new class of Pauli operator. This interface requires input
of a map to save the mapping relationship between the subscript before
saving the that after saving.

::

    b = PauliOperator("X5 Y6", 3)
    index_map = []
    c = b.remapQubitIndex(index_map)

    # The output value is 7
    print(b.getMaxIndex())
    # The output value is 2
    print(a.getMaxIndex())

6.1.2 Example
~~~~~~~~~~~~~

The following example mainly demonstrate how to use the ``PauliOperator``
interface.

::

    from pyqpanda import *
    if __name__=="__main__":
        a = PauliOperator("Z0 Z1", 2)
        b = PauliOperator("X5 Y6", 3)
        plus = a + b
        minus = a - b
        muliply = a * b
        print("a + b = ", plus)
        print("a - b = ", minus)
        print("a * b = ", muliply)
        print("Index : ", muliply.getMaxIndex())
        index_map = {}
        remap_pauli = muliply.remapQubitIndex(index_map)
        print("remap_pauli : ", remap_pauli)
        print("Index : ", remap_pauli.getMaxIndex())

The results are as below:

::

    a + b =  {      
    X5 Y6 : 3.000000
    Z0 Z1 : 2.000000
    }
    a - b =  {
    X5 Y6 : -3.000000
    Z0 Z1 : 2.000000
    }
    a * b =  {
    Z0 Z1 X5 Y6 : 6.000000
    }
    Index :  7
    remap_pauli :  {
    Z0 Z1 X2 Y3 : 6.000000
    }
    Index :  4

6.2 Class of Fermionic operators
--------------------------------

We use the following notations to indicate the two forms of fermions, 
annihilation: X denotes :math:`a_{x}` , creation :math:`X_{+}` denotes ::math:`a_{x}^{\dagger}`. For example, "1+ 3 5+ 1" denotes :math:`a_{1}^{\dagger} a_{3} a_{5}^{\dagger} a_{1}`.

The rules are organized as below:

1. Different numbers

   .. math::


         \begin{array}{r}
         " 1 \quad 2 "=-1 * " 2 \quad 1 " \\
         " 1+2+"=-1 * " 2+1+" \\
         " 1+2 "=-1 * " 2 \quad 1+"
         \end{array}
         

2. Same number

.. math::


   \begin{array}{r}
   " 1 \quad 1+"=1-" 1+1 " \\
   " 1+1+"=0 \\
   \quad " 1\quad1 "=0
   \end{array}

Similar with ``PauliOperator``, the class of ``FermionOperator`` provides basic
operations of Fermionic operators including addition, subtraction and
multiplication. An ordered list of results can be obtained through
organization.

6.2.1 Example
~~~~~~~~~~~~~

::

    from pyqpanda import *

    if __name__=="__main__":

        a = FermionOperator("0 1+", 2)
        b = FermionOperator("2+ 3", 3)

        plus = a + b
        minus = a - b
        muliply = a * b

        print("a + b = ", plus)
        print("a - b = ", minus)
        print("a * b = ", muliply)

        print("normal_ordered(a + b) = ", plus.normal_ordered())
        print("normal_ordered(a - b) = ", minus.normal_ordered())
        print("normal_ordered(a * b) = ", muliply.normal_ordered())

The results are as below:

::

    a + b =  {     
    0 1+ : 2.000000
    2+ 3 : 3.000000
    }
    a - b =  {
    0 1+ : 2.000000     
    2+ 3  : -3.000000
    }
    a * b =  {
    0 1+ 2+ 3 : 6.000000
    }
    normal_ordered(a + b) =  {
    1+ 0 : -2.000000
    2+ 3 : 3.000000
    }
    normal_ordered(a - b) =  {
    1+ 0 : -2.000000
    2+ 3  : -3.000000
    }
    normal_ordered(a * b) =  {
    2+ 1+ 3 0 : 6.000000
    }

6.3 Optimization algorithm (direct search)
------------------------------------------

This chapter will explain the application of optimization algorithms,
including the ``Nelder-Mead`` algorithm and the ``Powell`` algorithm which are
direct search ones. In ``QPanda``, we have implemented ``OriginNelderMead`` and
``OriginPowell`` which are both inherited from ``AbstractOptimizer``.

6.3.1 Interface introduction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

An optimizer of a specified type can be generated through the optimizer
factory. For instance, the type of the optimizer can be specified as
Nelder-mead.

::

    optimizer = OptimizerFactory.makeOptimizer(OptimizerType.NELDER_MEAD)
    #optimizer = OptimizerFactory.makeOptimizer('NELDER_MEAD')

We need to register with the optimizer a function used to compute the
loss value and the parameters to be optimized.

::

    init_para = [0, 0]
    optimizer.registerFunc(lossFunc, init_para)

Then, we can set the ending condition. Moreover, we can set the
convergence thresholds of variables and function values, the maximum
number of function calls and the number of iterations in an
optimization. The optimization ends as long as the above conditions are
met.

::

    optimizer.setXatol(1e-6)
    optimizer.setFatol(1e-6)
    optimizer.setMaxFCalls(200)
    optimizer.setMaxIter(200)

Then, we can conduct optimization through the exec interface, and obtain
the optimized results through the getResult interface.

::

    optimizer.exec()

    result = optimizer.getResult()
    print(result.message)
    print(" Current function value: ", result.fun_val)
    print(" Iterations: ", result.iters)
    print(" Function evaluations: ", result.fcalls)
    print(" Optimized para: W: ", result.para[0], " b: ", result.para[1])

6.3.2 Example
~~~~~~~~~~~~~

Given with some hash points, we fit a line where the sum of the
distances between the hash points and the line is minimized. The
expression of the function which defines the line is ``y = w\*x + b``. Next,
we will get the optimization values of w and b by using the optimization
algorithm. First, we define the function to obtain expectation.

::

    from pyqpanda import *
    import numpy as np

    x = np.array([3.3, 4.4, 5.5, 6.71, 6.93, 4.168, 9.779, 6.182, 7.59,\
                 2.167, 7.042, 10.791, 5.313, 7.997, 5.654, 9.27,3.1])
    y = np.array([1.7, 2.76, 2.09, 3.19, 1.694, 1.573, 3.366, 2.596, 2.53,\
                 1.221, 2.827, 3.465, 1.65, 2.904, 2.42, 2.94,1.3])

    def lossFunc(para,grad,inter,fcall):
        y_ = np.zeros(len(y))

        for i in range(len(y)):
            y_[i] = para[0] * x[i] + para[1]

        loss = 0
        for i in range(len(y)):
            loss += (y_[i] - y[i])**2/len(y)

        return ("", loss)

We use the ``Nelder-Mead`` algorithm for optimization.

::

    optimizer = OptimizerFactory.makeOptimizer('NELDER_MEAD')

    init_para = [0, 0]
    optimizer.registerFunc(lossFunc, init_para)
    optimizer.setXatol(1e-6)
    optimizer.setFatol(1e-6)
    optimizer.setMaxIter(200)
    optimizer.exec()

    result = optimizer.getResult()
    print(result.message)
    print(" Current function value: ", result.fun_val)
    print(" Iterations: ", result.iters)
    print(" Function evaluations: ", result.fcalls)
    print(" Optimized para: W: ", result.para[0], " b: ", result.para[1])

::

    Optimization terminated successfully.
     Current function value: 0.1538576740419577
     Iterations: 84
     Function evaluations: 161
     Optimized para: W：0.2516349997921341  b：0.7988010530189995

We plot a graph with the hash points and the fitted lines.

::

    import matplotlib.pyplot as plt

    w = result.para[0]
    b = result.para[1]

    plt.plot(x, y, 'o', label = 'Training data')
    plt.plot(x, w*x + b, 'r', label = 'Fitted line')
    plt.legend()
    plt.show()

.. figure::  /images/6.4.png
   :alt: