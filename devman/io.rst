Input/Output Plugins
====================
IO plugins are slightly different from the other plugin types. 
The ``__all__`` variable must still be populated with the 
registration method, but it must provide a string identifier 
and a reference to an ``IOPlugin`` (available from the 
pluginbase module) subclass. The base class provides structure 
for what is required of an IO plugin::

   class IOPlugin(Plugin):
      """
      All IOPlugins are expected to provide methods for opening and/or saving 
      data files.
      """
      def __init__(self, filename=None, fcData=None, window=None):
         self.filename = filename
         self.fcData = fcData
         self.window = window
        
      def register(self):
	     """
	     Returns a dictionary keyed on the FILE_INPUT and 
	     FILE_OUTPUT IDs (found in data.io) to indicate which (if any) methods
	     provide input and output functionality.
	     """
	     pass
    
      def fileType(self):
         """
         Returns a string used for identifying the file type(s) this plugin is 
         capable of reading/saving.
        
         ex: 'Comma Separated Values (*.csv)|*.csv' 
         """
         pass
    
      def read(self):
         """
         Given the specified path of a data file, input the data and return
         it along with the column labels, and any annotations or analysis.
        
         :@rtype: tuple
         :@return: (labels, data, annotations, analysis) 
         """
         pass
        
      def save(self):
         """
         Write the FC data to the specified file.
         """
         pass
         
As is specified above, the IOPlugin subclass must provide a second registration to 
the FIND plugin system, indicating whether it can read files, write files, or both. 
This is done by having the register method return a dictionary as specified above 
with the keys for reading and writing methods coming from::

   from data.io import FILE_INPUT, FILE_OUTPUT
   
Thus, having the register method return a ``dict`` with only the ``FILE_INPUT`` 
key and the reading method will cause FIND to only use the class for file 
input (available as an option in File..Open). If the ``FILE_OUTPUT`` method 
is also specified, FIND will place a menu item in File..Export with the 
string identifier given in the module register method. The final parameter 
to the class initializer is ``window``. This provides a reference to the FIND 
window class so that plugins can show dialog boxes (or other window subclasses) 
with options for reading or writing. For an example of this, see the CSV plugin.






