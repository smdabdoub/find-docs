Analysis Plugins
================
This category of plugins attempts to provide a more generic means of 
including additional functionality within FIND. Currently, analysis 
plugins can fill two roles: Provide some visual analysis results when 
a user clicks on the analysis plugin from within the plugins menu, or 
return numeric analysis results to a piece of code calling it from 
within FIND. Analysis plugins must at least provide numeric results 
to callers, but the plugin can also indicate that it has the ability 
to display some results to the user through a dialog or other window 
object. This is possible through the method registration function which 
should be similar to the following::

   def analysisMethod_register():
      return (analysisMethod, True)
      
Here, the first item in the tuple is a reference to the provided 
analysis method, while the second item is a boolean indicating 
(True in this case) that this method is callable by the user and 
will display some results visually. A True in this slot will cause 
FIND to create an enabled menu item in the Plugins>>Analysis menu. 
A False will cause FIND to place a disabled menu item to simply inform 
the user the plugin was loaded.

The analysis method signature and doc string are as follows::

   def analysisMethod(data, **kwargs):
      """
      string-ID; method-name; Method description string
      """
      ...
      ...
      return (result, message)
      
The ``data`` parameter is an ``m x n`` array (numpy ``ndarray``) with 
``m`` data points (events), and ``n`` dimensions (channels). The ``**kwargs`` 
parameter is a dictionary containing options for the analysis algorithm. 
These could be either generic options provided by FIND or specific options 
you specify and publish for others to use. Currently FIND provides one 
option in the args dictionary: ``'parentWindow'``. This provides a reference 
to the FIND main window, and allows the analysis method to show a dialog or 
other window class for the purpose of displaying results to the user.

The first line of the doc string must be semicolon-separated into three 
fields as seen above. The 'string-ID' field is what other plugin authors 
will give to the analysis module in order to access the analysis method 
from within their own code. The 'method-name' is a short name that FIND will 
use for the menu item placed in Plugins>>Analysis. The final field will 
appear in the program status bar when a user moves the mouse over the 
menu item for the analysis plugin.

The return of the analysis method consists of a 2-tuple. The first 
element of the tuple is some result (or None) if it is being called from 
code and not by user interaction with the Plugin menu. The second element is 
a string message that will be displayed to the user in the program status 
bar once the method has completed running.

Plugin authors will be able to make use of analysis plugin methods by passing 
the string identifier in the following manner to the Analysis module::

   import analysis.methods as am
   result = am.getMethod('pca')(data)