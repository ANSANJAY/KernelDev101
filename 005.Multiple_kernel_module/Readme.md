## Building a Kernel Module with Multiple Source Files using Makefile

If you have multiple source files, such as `hello.c` and `func.c`, and you want them to be compiled and linked together to form a single kernel module, you can achieve this with the following `Makefile`:

```makefile
obj-m := hello1.o
hello1-objs := func.o hello.o

all:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```
or

If you want to generate two kernel modules, say `module1.ko` and `module2.ko`, from two different source files, `module1.c` and `module2.c`, you would adjust your Makefile to include both modules.

Here's a sample Makefile to achieve this:

```makefile
obj-m := module1.o module2.o

all:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

----



