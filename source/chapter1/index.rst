1 Basic Introduction
====================

For the sake of compatibility efficiency and convenience, QPanda2 is
provided with two versions: C++ and Python. This document mainly
introduces how to use the Python version. To learn about the use of C++
version, please go to QPanda2.

We use the pybind11 tools to encapsulate the functions and classes in
QPanda2 in a direct and concise way, and provide the almost perfect
mapping function. The code of the encapsulated part will generate a
dynamic library during Qpanda2 compilation, so the encapsulated part can
be imported as a Python package.

1.1 System configuration
------------------------

Pyqpanda takes C++ as the host language, with requirements for the
system environment as follows:

+------------+-----------+
| software   | version   |
+============+===========+
| GCC        | >=5.4.0   |
+------------+-----------+
| Python     | >=3.6.0   |
+------------+-----------+

1.2 Download pyqpanda
---------------------

If you have installed the python environment and pip tool, please enter
the following commands at the terminal or console:

::

    pip install pyqpanda

.. admonition:: Note

    In case of permission problems under linux, add ``sudo`` .