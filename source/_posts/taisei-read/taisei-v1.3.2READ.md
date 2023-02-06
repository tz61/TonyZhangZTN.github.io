![](C:\Users\Administrator\AppData\Roaming\marktext\images\2022-12-25-22-44-34-image.png)

如果直接meson setup build

meson compile -C build

这样全用本地的submodule，会出现链接问题SDL_main unresolved reference

meson脚本里写的是如果pkgconfig找不到才会用本地的subprojects(利用wrap下的文件安装的，具体是的`check-submodules.py` 检查的，具体的项目的构建树以后再去看，脚本太多了)

然后用pacman -S 安装SDL2后就不报错了，但是不能运行（打开后闪退，没有原因

然后我把所有依赖用pacman装了才能运行

具体以后再探究


