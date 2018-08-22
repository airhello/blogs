# 通过堆栈回溯调用的过程

案例堆栈
```
Exception stack(0xffffffc01b5fb490 to 0xffffffc01b5fb5c0)
b480: 10bc2100 ffffffc0 00000080 00000000
b4a0: 1b5fb5e0 ffffffc0 fd4d45d8 ffffffbf 1aa50380 ffffffc0 ffffffbffd3c5ac8 
b4c0: 10bc2100 ffffffc0 34b40d5c ffffffc0 0000009f 00000000 00000ee8 00000000
b4e0: 1aa58000 ffffffc0 1aa58ed0 ffffffc0 00000100 00000000 00000018 00000000
b500: 00000018 00000000 00000000 00000000 fd4d3c70 ffffffbf 00000000 00000000
b520: 00000001 00000000 00000000 00000000 00154ea0 ffffffc0 99050568 0000007f
b540: 0000000a 00000000 10bc2100 ffffffc0 1b579180 ffffffc0 1d60d280 ffffffc0
b560: 00000007 00000000 0fd2c780 ffffffc0 34b40d5c ffffffc0 fd3c5ac8 ffffffbf
b580: 3933019c ffffffc0 00000001 00000000 1b5fbc28 ffffffc0 1b5fb5e0 ffffffc0
b5a0: fd3c67e8 ffffffbf 1b5fb5e0 ffffffc0 fd4d45d8 ffffffbf 80000105 00000000
```

## 找出栈上函数地址
内核模块的代码段地址范围一般是0xffffffbfXXXXXXXX, 先在栈上找到这类地址

## 升级到相同版本, 确认模块的加载初始地址
```
~ # lsmod | grep wl
wl 9022719 0 - Live 0xffffffbffd16c000 (O)
```

## 根据初始地址计算栈上函数地址与初始地址偏移
```
ffffffbffd16c000 (O)
ffffffbffd4d45d8 - ffffffbffd16c000 = 0x3685D8
```

## 通过nw命令来根据偏移查找函数(一般尽量grep前面4个数字, 要考虑地址可能是函数内部的语句)
```
root@ubuntu:~/ap860-i/images/rootfs/sbin# nm wl.ko | grep 3685
0000000000368508 t $d
0000000000368510 t $d
0000000000368518 t $d
0000000000368520 t $d
0000000000368528 t $d
0000000000368530 t $d
0000000000368538 t $d
0000000000368540 t $d
0000000000368548 t $d
0000000000368550 t $d
0000000000368558 t $d
00000000000a6090 r __FUNCTION__.73685
0000000000368560 T wlc_pcb_fn_move
00000000003685a0 T wlc_pcb_fn_register
0000000000368560 t $x
```

## 找到又小于最接近的函数符号, 并计算实际地址与函数符号的偏移, 得到函数内部偏移
```
3685D8 - 3685a0 = 0x38
ffffffbffd4d45d8 = wlc_pcb_fn_register+0x38
```

## TODO: 脚本化, 函数变量定位(估计需要结合寄存器/反汇编)
