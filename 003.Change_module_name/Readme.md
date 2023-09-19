Renaming the Output Kernel Module
=================================

If you have a source code file named `hello.c` but you want the output module to be `linux.ko`, you can achieve this by defining the desired module name and then mapping it to the appropriate object file.

Here's a `Makefile` example that accomplishes this:

```makefile
obj-m := linux.o
linux-objs := hello.o

all:
	make -C /lib/modules/`uname -r`/build M=$(PWD) modules

clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```
Explanation:
------------

- `obj-m := linux.o`: This line informs the kernel build system that you're aiming to produce a module named `linux.ko`.

- `linux-objs := hello.o`: This specifies that the `linux.ko` module should be built using the `hello.o` object file, which comes from compiling `hello.c`.

When you execute `make`, the kernel build system will first compile `hello.c` to get `hello.o`. Then, instead of creating a module named `hello.ko` (the default behavior), it will link `hello.o` into a module named `linux.ko`.
