"Glass" UI application
==================================

Glass views eObject tree, and can open windows to visualize the objects. How an object is viewed is is based on data it contains, class, and metadata within object. 

The egui/class works on top of eobjects data abstraction library, and inherits it's functionality: High level "ecom" protocol connects multiple processes together as one eobjects tree. IO device network communication, "iocom", is also readily visible from the egui.

The egui/glass look and feel, and and application user interface structure is intended to be customizable. Idea is to adopt to applications requiremenets, physical device like PC/tablet/phone, and to fashion trends. The egui library is based on Dear ImGui, which renders directly to OpenGL, DirectX or Vulcan graphics pipeline. 

.. figure:: pics/210215-glass-transparent-windows-on-top-of-camera-view.png

   "Early glass prototyping".

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   210215-glass-introduction

