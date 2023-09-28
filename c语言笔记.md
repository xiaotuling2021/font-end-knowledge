# c语言笔记

1.gcc编译过程

```
预处理：gcc -E hello.c -o hello.i
编译：gcc -S hello.i -o hello.s
汇编：gcc -c hello.s -o hello.o
链接：gcc hello.o -o hello
```

