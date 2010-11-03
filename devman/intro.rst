Introduction
============
Developers are encouraged to create additional functionality for FIND 
through the available plugin architecture. Specifically, FIND allows for 
plugins in the following categories: Analysis, Clustering, Graphing, IO, 
and Transforms. Each of these plugin types are described in detail with 
code examples in their respective sections later in this documentation.

Beyond the API provided within FIND (which will be discussed where appropriate
per plugin type), the following external Python libraries 
are available to developers:

* Numpy (`http://numpy.scipy.org <http://numpy.scipy.org>`_)
* Scipy (`http://scipy.org <http://scipy.org>`_)
* Matplotlib (`http://matplotlib.sourceforge.net/ <http://matplotlib.sourceforge.net/>`_)
* wxPython (`http://wxpython.org/ <http://wxpython.org/>`_)
* Pycluster (`http://bonsai.hgc.jp/~mdehoon/software/cluster/software.htm <http://bonsai.hgc.jp/~mdehoon/software/cluster/software.htm>`_)

General Plugin Architecture
---------------------------
In order for a plugin to be recognized, the main module file containing 
it must be placed within the appropriate folder in the Plugins directory 
that must accompany the FIND executable. For example, included with the 
FIND distribution is a clustering plugin called **Unicluster**. The plugin 
consists of one file ``unicluster.py`` which is placed within the ``cluster`` 
folder. Internally, module files must provide two items, indicating to FIND 
the existence of plugin functionality. The first is a registration method 
which in all code examples follow the naming convention ``method_register()``. 
This method returns a ``tuple`` that provides FIND with references to necessary 
methods or information specific to the plugin type. Given that the register 
method can take any name, modules must indicate its existence to find by placing 
it in the ``__all__`` magic list variable provided by Python. The FIND plugin 
architecture will use this list and the corresponding methods to internally link 
to the provided plugin functionality. Plugins may consist of multiple modules, 
but only modules with the ``__all__`` variable defined will be examined for plugin 
functionality. The specifics of what each registration method must provide are 
discussed in the appropriate section of the manual. 