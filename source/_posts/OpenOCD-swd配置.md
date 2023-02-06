objdump -h to check sections in an elf file.

DMA: Direct memory access

DAP: Debug Access Point

TAP: Test Access Ports

STM32F103C8T6:SW-DP TAP-ID:0x1BA01477

![](C:\Users\Administrator\AppData\Roaming\marktext\images\2023-01-30-21-18-34-image.png)

STM32F429IGT6:SW-DP TAP-ID: 0x2BA01477

![](C:\Users\Administrator\AppData\Roaming\marktext\images\2023-01-30-21-34-47-image.png)

F103C8T6:

成功运行后不知道为什么

![](C:\Users\Administrator\AppData\Roaming\marktext\images\2023-01-30-21-33-57-image.png)

提示的id和之前不一样

F429: 

![](C:\Users\Administrator\AppData\Roaming\marktext\images\2023-01-30-21-42-02-image.png)

Cannot identify target as a STM32 family

```bash
Info : device id = 0x00000000
Warn : Cannot identify target as a STM32 family.
Error: auto_probe failed
Error: Connect failed. Consider setting up a gdb-attach event for the target to prepare target for GDB connect, or use 'gdb_memory_map disable'.
Error: attempted 'gdb' connection rejected
```

但是用了Clion之后，貌似能烧了？

![](C:\Users\Administrator\AppData\Roaming\marktext\images\2023-01-30-23-02-14-image.png)

![](C:\Users\Administrator\AppData\Roaming\marktext\images\2023-01-30-23-15-01-image.png)

出现0x00000000 貌似是板子无法reset（不是脚本的问题

真正的原因：Error: ** Unable to reset target **

解决方案：硬重启，重启电源

一个正常的debug(STM32F103):

![](C:\Users\Administrator\AppData\Roaming\marktext\images\2023-01-31-18-42-16-image.png)

问题1:

然而，STM32F429不能halt,

问题2： chibios项目Reading the memory signature of ChibiOS/RT failed

2.1Reading the memory signature of ChibiOS/RT failed

2.2ChibiOS/RT registry check failed, double linked list violation.

问题3：电源问题

![](C:\Users\Administrator\AppData\Roaming\marktext\images\2023-01-31-22-12-06-image.png)

解决方案：在烧录时只接st-link线路

但是不插5v，就只能从system memory 启动 (pc 在 0x1fff---- 循环)

这是插了5v

![](C:\Users\Administrator\AppData\Roaming\marktext\images\2023-02-01-12-20-52-image.png)

貌似时钟配置有问题，卡在0xa0000000



Can't find source: Add -g flag to compile options
