.. ==================================================
.. FOR YOUR INFORMATION
.. --------------------------------------------------
.. -*- coding: utf-8 -*- with BOM.

.. include:: ../../Includes.txt

f:render
========

Partials in Fluid are like FCEs in TemplaVoilà. In short: re-usable template segments. They are highly useful if the 
code contained in the Partial is used more than once. Each partial takes around 5ms to load, so take this into account 
when using them. A table layout containing 700 cells and 15 columns, where each cell loads a Partial, would take 
700 * 15 * 5 = 52,500 milliseconds… so nearly a minute!

Properties
-------------

.. t3-field-list-table::
 :header-rows: 1

 - :Property,20:    Property
   :Datatype,20:    Variable type
   :Description,40: Description
   :Standard,10:    Standard
   :Mandatory,10:   Mandatory

 - :Property:    section
   :Datatype:    String
   :Description: The name of the section to be rendered.
   :Standard:    NULL
   :Mandatory:   Yes

 - :Property:    partial
   :Datatype:    String
   :Description: The path and filename (without .html suffix), starting from the Partials root folder.
   :Standard:    NULL
   :Mandatory:   Yes

 - :Property:    arguments
   :Datatype:    Array
   :Description: Which variables should be passed into the Partial or into the Layout.
   :Standard:    Empty array
   :Mandatory:   Yes

 - :Property:    optional
   :Datatype:    Boolean
   :Description: You'll normally see an error message if a section can't be found. Setting this property to TRUE will stop an error from being displayed.
   :Standard:    FALSE
   :Mandatory:   Yes

The Partial files are always stored in pre-defined folders. The usual path within an extension is typo3conf/ext/[ExtensionKey]/Resources/Private/Partials/. 
If you're working with FLUIDTEMPLATE, you can re-define this path by over-riding the TypoScript property "partialRootPath" (with a trailing slash).

Sections don't have their own folder: they are defined within the current template file. Exceptions are managed using Layouts.

Example of a Partial
--------------------

In our Fluid template:

::

 <f:alias map="{employees: {0: {first_name: 'Stefan', city: 'Lindlar'},1: {first_name: 'Petra', city: 'Lindlar'},2: {first_name: 'Sascha', city: 'Remscheid'},3: {first_name: 'Patrick', city: 'Bonn'},4: {first_name: 'Sven', city: 'Gummersbach'},5: {first_name: 'Andrea', city: 'Wuppertal'}}}">
   <table>
     <f:for each="{employees}" as="employee">
       <f:render partial="TableRow" arguments="{employee: employee}"/>
     </f:for>
   </table>
 </f:alias>

In the Partial file Resources/Private/Partials/TableRow.html:

::

 <tr>
   <td>{employee.first_name}</td>
   <td>{employee.city}</td>
 </tr>
 
Where it will be more efficient to work with a Section (see explanation above):

::

 <f:section name="Main">
   <f:alias map="{employees: {0: {first_name: 'Stefan', city: 'Lindlar'},1: {first_name: 'Petra', city: 'Lindlar'},2: {first_name: 'Sascha', city: 'Remscheid'},3: {first_name: 'Patrick', city: 'Bonn'},4: {first_name: 'Sven', city: 'Gummersbach'},5: {first_name: 'Andrea', city: 'Wuppertal'}}}">
     <table>
       <f:for each="{employees}" as="employee">
         <f:render section="TableRow" arguments="{employee: employee}"/>
       </f:for>
     </table>
   </f:alias>
 </f:section>

 <f:section name="TableRow">
   <tr>
      <td>{employee.first_name}</td>
      <td>{employee.city}</td>
   </tr>
 </f:section>
 