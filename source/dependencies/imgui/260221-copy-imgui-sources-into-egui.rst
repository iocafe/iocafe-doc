Copy imgui source files into egui library code
==============================================

HOW TO SWITCH TO A NEW IMGUI VERSION: 
Imgui source code is downloaded as /coderoot/bluetree/egui/dependencies/downloads/imgui/imgui-1.92.6-docking.
Selected imgui source code files are copied into egui project. 

- Copy source files from /coderoot/bluetree/egui/dependencies/downloads/imgui/imgui-1.92.6-docking directory 
  into /coderoot/bluetree/egui/code/imgui directory.

- For version imgui-1.92.6-docking these files are imconfig.h, imgui.cpp, imgui.h, imgui_demo.cpp, imgui_draw.cpp, 
  imgui_internal.h, imgui_tables.cpp, imgui_widgets.cpp, imstb_rectpack.h, imstb_textedit.h and imstb_truetype.h.
  File list and location has been consistent between diggerent imgui versions.

- Copy GLF/Opengl implementation files from /coderoot/bluetree/egui/dependencies/downloads/imgui/imgui-1.92.6-docking/backends 
  to /coderoot/bluetree/egui/glfw_opengl3/imgui.
  These files are imgui_impl_glfw.cpp, imgui_impl_glfw.h, imgui_impl_opengl3.cpp, imgui_impl_opengl3.h 
  and imgui_impl_opengl3_loader.h.

- Some problem with egui, assert for table fails. I needed to comment an assert away to be able to run.
  The assert is in file imgui.cpp, line 3429. Marked with string "PEKKA, I HAVE SOME ERROR HERE". 

260221, updated 25.2.2026/pekka
