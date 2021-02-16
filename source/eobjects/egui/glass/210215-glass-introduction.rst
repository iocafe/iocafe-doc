"Glass" UI application
==================================

Glass views eobjects tree, and can open windows to visualize the objects. How an object is viewed is is based on data it contains, class, and metadata within object. 

The egui/class works on top of eobjects data abstraction, and "inherit" it's functionality: High level "ecom" protocol connects multiple processes (in different computers) connect togerther as one eobjects tree. IO device network communication, "iocom", is readily visible.

The egui/glass look and feel, and and application user interface structure is intended to be customizable. Idea is to  allow it to adopt to applications requiremenets, device on which it runs (pc/tablet/phone) and to fashion trends in UI makeup. The egui library is based on Dear ImGui, which renders directly OpenGL, DirectX or Vulcan graphics pipeline. 

.. figure:: pics/210215-glass-transparent-windows-on-top-of-camera-view.png

   "Early glass prototyping".

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   210215-glass-introduction

