# xf-webasm
Port and modernisation of the XForms 1.1 from mozilla to an xhtml5 / Web assembly

This project takes the mozilla xforms engine that was written in C++ and converts it to a web assembly.  That is the easy part!

To make it work in a modern XHTML5 environment this project also will have an XSLT/JS library that will take an XHTML5 page with xforms
1.1 markup and call-back to the web-assembly to do the model, instance, event, submission that is the complex bits.  Leaving the decorated
html5 code for rendering and UI.

The XSLT/JS library will be heavily influenced by XSLTForms which we currently use commercially for our projects.  This engine is intended 
to replace the MIP/Event/XPath engine with a modern C++ implementation.

If you are interested, please just follow this page, I will let you know when the heavy lifting engine is at least alpha and starting
to pass the MIP tests from XForms 1.1 the rest of the compliance tests will be focused on the XSLT/JS library.
