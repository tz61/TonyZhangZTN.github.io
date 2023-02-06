# Why to use GLAD on windows in order to use OpenGL?

> Authors suggest there to use GLAD library, because this library provides creation of GL context (which does not fit me)

No, GLAD does not create or manage GL contexts in any way, and the website https://learnopengl.com/Getting-started/Creating-a-window never claims otherwise. They use GLFW for context and window management.

> Does compiler really cannot get addresses of Opengl functions from GL-related .obj files which I specify in Linker Settings of Visual Studio?

**No.**

First of all, you do not specify OpenGL-related `.obj` files for the linker, but on Windows, you might use `opengl32.lib`, which is the import library file for `opengl32.dll` which comes with every windows version since Windows 95.

However, this DLL does not contain the OpenGL implementation you are typically using, but it contains Microsoft's OpenGL 1.1 GDI software rasterizer. The actual OpenGL implementation on windows is provided by an *Installable Client Driver* (ICD) which comes with your graphics driver. For OpenGL 1.0 and 1.1 functions, `opengl32.dll` will act as a [trampoline](https://en.wikipedia.org/wiki/Trampoline_(computing)) and will forward the calls to the actual ICD DLL.

**If you want to call any OpenGL function beyond OpenGL 1.1** (and that one is from 1997), you have to use the OpenGL extension mechanism in every case, as **`opengl32.dll` does not provide these entry points at all**, and the compiler/linker will of course not find them.

Source: [c++ - Why use &#39;glad&#39; library for opengl initialization? - Stack Overflow](https://stackoverflow.com/questions/55267854/why-use-glad-library-for-opengl-initialization#:~:text=In%20simple%20words%2C%20GLAD%20manages,the%20specific%20graphics%20card%20supports.)
