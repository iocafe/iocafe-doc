Dear ImGui notes
==================================

Fonts
***********

ImGui works fine with Unicode fonts, I believe it works for at least all languages which can be written from left to right. Not so sure about Arabic and Hebrew.

Google's NOTO fonts can be downloaded from https://www.google.com/get/noto/help/install/ as one big zip file.
Here only one of the files within zip is needed, I picked up NotoSansDisplay-Regular.ttf.

Use something like io.Fonts->AddFontFromFileTTF("/coderoot/bluetree/egui/fonts/NotoSansDisplay-Regular.ttf", 20.0f, NULL, io.Fonts->GetGlyphRangesCyrillic()); to load the Cyrillic glyphs to ImGui (latin ones are loaded by default).

If Russian string literals are needed within C/C++ code, portable way is to code these as escape sequences:
const char myrussiantext[] = "\xd0\xbc\xd0\xb0\xd1\x82\xd0\xb5\xd1\x80\xd0\xb8\xd0\xbd\xd1\x81\xd0\xba\xd0\xb0\xd1\x8f\x20\xd0\xbd\xd0\xbe\xd1\x87\xd1\x8c";

Tool to convert Unicode text (Chinese, Russian, etc.) to C escape sequences: https://onlineunicodetools.com/escape-unicode
Select "use bytes", UTF8 and no separator. The escaped unicode can be copy/pasted into code.

.. figure:: pics/210417-russian-text-in-egui.png.png

   "Russian text by Dear ImGui".


210417, updated 17.3.2021/pekka
