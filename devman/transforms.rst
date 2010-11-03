Transformation Plugins
======================
Data transformation plugins provide functionality to transform input 
data from one scale/space to another. The most basic example is 
a logarithmic transform which converts linear data to a logarithmic 
scale. The registration method for these plugins looks like the following::

   def transform_register():
         return (transformMethod, MethodTransformScaleClass)
         
The ``transformMethod`` reference performs the actual transformation of 
user data. The ``MethodTransformScaleClass`` is an instance of the 
``ScaleBase`` class found in the ``matplotlib.scale`` module. It 
is not required, so the register method can simply place ``None`` in the 
second slot of the tuple. However, if you wish to provide a plugin 
that is also automatically applicable to graphs/plots, then you will 
need to provide a subclass of ``ScaleBase``. An 
`example <http://matplotlib.sourceforge.net/examples/api/custom_scale_example.html>`_ 
of creating your own scale for plots is available at the matplotlib website. 
If you provide this class, FIND will automatically register your scale with 
the matplotlib engine, and it will be available to specify for any matplotlib 
plot that accepts scale requests.

The ``transformMethod`` method signature and doc string should look like 
the following::

   def transformMethod(data, **kwargs):
      """
      string-ID; transformMethod name; Method description string
      """
      ...
      ...
      return transformed_data

The ``data`` parameter is (as with other plugins) an ``m x n`` array 
(numpy ``ndarray``) with ``m`` data points (events), and ``n`` dimensions 
(channels). The ``**kwargs`` parameter is a dictionary containing options 
for use by the transform method. For example, FIND's built-in log transform 
accepts ``base`` and ``min_clip`` parameters indicating, respectively, the 
base of the log transform (2, 10, ``e``) and the lower end the data should 
be clipped to when negative values are encountered (default: 10e-5).

Finally, as mentioned in the section on Graphing Plugins, the internal 
transforms package provides within its ``methods`` module, the means 
for any FIND code to use registered transforms. Specifically, the module 
provides a ``getMethod(strID)`` method that returns the method 
specified by the string identifier of the transformation method/plugin. 
So to apply a log transform to your data with the built-in log method 
you might use the following lines::

   import transforms.methods as tm
   logData = tm.getMethod('log')(data, base=10, min_clip=10e-8)