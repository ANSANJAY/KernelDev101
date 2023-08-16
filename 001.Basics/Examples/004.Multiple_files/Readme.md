Including Additional Source Files in Kernel Module Compilation:
---------------------------------------------------------------

When building a kernel module that involves multiple source files, you'll need to adjust the `Makefile` to consider these additional files.

Given our previous example where we have `hello.c` as our main source file and now we're introducing `func.c`, here's how to adjust the `Makefile`:

```makefile
obj-m := linux.o
linux-objs := hello.o func.o

all:
	make -C /lib/modules/`uname -r`/build M=$(PWD) modules

clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```
## Explanation:

`linux-objs := hello.o func.o`: This line is updated to include `func.o`. This tells the kernel build system to compile both `hello.c` and `func.c` and then link the resulting object files (`hello.o` and `func.o`) into our final module, `linux.ko`.

When you run `make`, the kernel build system will compile both `hello.c` and `func.c` into their respective object files. These object files will then be linked together to produce the `linux.ko` module.

> **Note:** Ensure that any header files or dependencies shared between `hello.c` and `func.c` are properly included and available in your module's directory.
