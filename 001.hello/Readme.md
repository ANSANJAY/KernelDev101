# Hello World Kernel Module

In traditional C/C++ programming, `main()` serves both as the entry and exit points. Contrarily, kernel modules require at least two distinct functions:
- A "start" or initialization function, invoked when the module is loaded.
    - return value is int
- An "end" or cleanup function, invoked just before the module's removal.
    - return value is void
These are achieved using the `module_init()` and `module_exit()` macros.

## Licensing

It's imperative for a module to declare its licensing type, accomplished using the `MODULE_LICENSE()` macro. Below are some common licenses:

- "GPL" - Represents the GNU Public License v2 or later versions.
- "GPL v2" - Specifically for the GNU Public License v2.
- "GPL and additional rights" - Offers rights of GNU Public License v2 and potentially more.
- "Dual BSD/GPL" - Provides an option between the GNU Public License v2 and the BSD license.
- "Dual MIT/GPL" - Gives a choice between the GNU Public License v2 and the MIT license.
- "Dual MPL/GPL" - An option to pick between the GNU Public License v2 and the Mozilla license.
- "Proprietary" - Used for non-free products.

## Header Files

Every kernel module mandates the inclusion of `linux/module.h`. This inclusion aids in the macro expansion of `module_init` and `module_exit`. The `linux/kernel.h` is exclusively required for the macro expansion associated with the `printk()` log level.


## How to write a Makefile for a kernel module: 

A `Makefile` is a simple way to manage project build processes in a structured and efficient manner, especially when building more complex projects. In the context of kernel module development, a `Makefile` becomes nearly essential because kernel module compilation involves special flags, includes, and processes that are different from normal user-space programs.

## How to write a Makefile for a kernel module:

Given a kernel module source file named `hello.c`, here's a basic `Makefile` that would compile it:

```makefile
obj-m += hello.o

all:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

## Explanation:


### What is `obj-m`?

In the context of kernel module compilation, the variable `obj-m` specifies which object files are to be built as loadable kernel modules. When you set `obj-m += hello.o`, you're telling the kernel build system that `hello.c` (which compiles to `hello.o`) should be built as a module, not as a part of the main kernel.

- `obj-m += hello.o`: The `obj-m` directive tells the kernel make system that you want to create a module. The `obj-m` stands for "object module", which means the output should be a module (as opposed to a built-in piece of the kernel). The source file `hello.c` gets compiled into an object file named `hello.o`, which then gets linked to produce the kernel module.

- `all`: This is the default rule. When you run `make` in your directory, this rule is what gets executed. 

  - `-C /lib/modules/$(shell uname -r)/build`: This changes the directory to the kernel build directory for the currently running kernel. This is where the kernel source and configuration for the currently running kernel is found on most systems.

  **Section 1: Explain the technical concept 📘**

In the context of the Linux kernel's Makefile and many other Makefiles, the `-C` option is used with `make` to change to a specified directory before reading the makefiles or performing any build actions.

For instance:
```
make -C /path/to/directory
```

When using the `-C` option, `make` will first change to the specified directory (`/path/to/directory` in the example) and then it will look for the Makefile in that directory to execute it.

In the context of the Linux kernel, you might see commands like:
```
make -C /lib/modules/$(uname -r)/build M=$(PWD) modules
```
This command essentially tells `make` to change to the kernel build directory (often where the kernel headers and configuration for the running kernel are stored) and then execute the build from there using the current directory (`$(PWD)`) as the module source.

**Section 2: Technical interview questions about this topic and answers 🤓**

1. **Question**: What does the `-C` option do in the `make` command?
   **Answer**: The `-C` option in the `make` command instructs `make` to change to a specified directory before reading the makefiles or executing any build actions.

2. **Question**: When building a Linux kernel module, why might one use the `-C` option followed by `/lib/modules/$(uname -r)/build`?
   **Answer**: This is used to point the `make` command to the directory where the build environment of the currently running kernel resides. It ensures that the module is being built against the correct kernel headers and configuration of the running kernel.

3. **Question**: If you're already in a directory containing a Makefile, do you need to use the `-C` option to run `make`?
   **Answer**: No, if you're already in the directory containing the Makefile, you can just run `make` directly. The `-C` option is useful when you want to invoke a build from a different directory without changing your current working directory.

**Section 3: Explain the concept in simple words 🌟**

Imagine you're a chef with many recipe books stored in different rooms of your house. Instead of always going to a specific room to check a recipe, you can call an assistant and say, "Hey, bring me the recipe from the kitchen room!" This is similar to what the `-C` option does with `make`. It's like telling `make`, "Hey, execute the build instructions (or recipe) from this specific folder!"

  - `M=$(PWD)`: This tells the kernel make system to go back to your module's directory (the current directory) to build the module.
  - `M`: This is a make variable used by the kernel's build system. When building external modules, you set M to the path of the directory containing your module's source files.
  - `$(PWD)`: This is a variable that evaluates to the current working directory. PWD stands for "present working directory". In a Makefile, the (PWD) syntax is used to retrieve the value of the PWD variable. Essentially, it gives the full path to the directory where the Makefile is located.
  When you pass `M=$(PWD)` as an argument to make, you're telling the kernel's build system to build the module(s) located in the current directory. This allows you to build external kernel modules without modifying the kernel's own source tree.



  - `modules`: This is the actual target for building modules. When you specify "modules" as a target, the kernel build system knows that you intend to build a module, not another kind of target.

![](../images/Screenshot%20from%202023-08-16%2001-03-39.png)


-----

![](../images/Screenshot%20from%202023-08-16%2001-06-09.png)


note : not an  intree module



## Why use a Makefile:

1. **Simplification**: By using a `Makefile`, you encapsulate the details of the build process in one place. This makes it easy to compile the module by just running `make` instead of remembering and typing out the full compilation command.

2. **Consistency**: It ensures that the module is always built the same way, preventing inconsistencies and errors that can arise from manually typing in the build commands.

3. **Flexibility**: As your module grows, you might add more source files, dependencies, or even additional modules. A `Makefile` can easily handle such complexities, ensuring all parts of your project are properly built.

4. **Integration with kernel build system**: Kernel modules need to be built against the kernel source and configuration that they're intended to be loaded into. The kernel's build system provides a lot of functionality for building modules, and using a `Makefile` is the easiest way to take advantage of this functionality.

In summary, a `Makefile` greatly simplifies and standardizes the process of building kernel modules, making development quicker and less error-prone.

## Load the kernel module in the kernel 

### Syntax : 
```bash
insmod ./<module name>
```
### Usage 
```
insmod ./hello.ko
```

```bash 
lsmod
```

Use `lsmod` to check the module has been loaded to the kernel or not.

![](../images/Screenshot%20from%202023-08-16%2001-10-33.png)

Navigate to `/sys/module` , a module named hello should have been added.

![](../images/Screenshot%20from%202023-08-16%2001-15-58.png)

## Check kernel message 


We used printk int he program as follows 

```C
    printk(KERN_INFO"%s : In init\n",__func__);
```
To monitor the output , monitor `dmesg`

### When insmod is excuted 

![](../images/Screenshot%20from%202023-08-16%2001-22-44.png)


### While rmmod is excuted 

![](../images/Screenshot%20from%202023-08-16%2001-26-16.png)

How to Cross Compile
====================

Cross-compiling refers to the process of building code for one platform (the target) on a different platform (the host). This is particularly useful when the target platform has limited resources. For instance, if you're developing for an embedded system, it might not have the necessary resources or tools to compile the code. In such cases, you'd use a more powerful host system (like a PC) to compile the code, and then transfer the compiled binary to the target system.

To cross-compile, you typically need:
1. A cross-compiler: This is a compiler that generates binaries for the target architecture. These tools usually have a prefix indicating the target architecture, like `arm-linux-gnueabi-gcc` for ARM.
2. Libraries and headers for the target system: These might be different from the libraries and headers on the host system.
3. A way to transfer the compiled binary to the target system, such as `scp`, `rsync`, or physical media.

How to Make Changes in the Above Makefile to Cross Compile
=========================================================

When cross-compiling, you might need to adjust the `Makefile` to use the cross-compiler and ensure the appropriate flags and paths are set. Here's an example of how you could modify the aforementioned `Makefile` to support cross-compilation:

```makefile
CROSS_COMPILE ?= arm-linux-gnueabi-
CC := $(CROSS_COMPILE)gcc
ARCH ?= arm

obj-m += hello.o

all:
	make ARCH=$(ARCH) CROSS_COMPILE=$(CROSS_COMPILE) -C /path/to/target/kernel/source M=$(PWD) modules

clean:
	make ARCH=$(ARCH) CROSS_COMPILE=$(CROSS_COMPILE) -C /path/to/target/kernel/source M=$(PWD) clean
```

`printf` vs `printk`
====================

Both `printf` and `printk` are functions used to display messages, but they have distinct purposes and are used in different contexts.

### `printf`:

- **Definition**: `printf()` is a function found in the C Standard Library.
- **Purpose**: Used in user-space programs to display formatted strings.
- **Output**: Writes to the standard output, typically the console or terminal where the program runs.
  
### `printk`:

- **Definition**: `printk()` is a kernel-level function used in the Linux kernel.
- **Purpose**: Provides logging and debugging capabilities for kernel space. Especially useful because the kernel does not have access to standard C library functions.
- **Output**: Writes messages to the kernel log buffer. They can later be fetched using commands like `dmesg`.
- **Syntax**: Unlike `printf`, `printk` is typically called with an additional argument representing the log priority:
  
  ```c
  printk(KERN_log_priority "formatted string\n");
  ```

  ## Log Priorities
---
These are predefined in `linux/kernel.h` and determine the importance/severity of the message. They are:

- `KERN_EMERG`
- `KERN_ALERT`
- `KERN_CRIT`
- `KERN_ERR`
- `KERN_WARNING`
- `KERN_NOTICE`
- `KERN_INFO`
- `KERN_DEBUG`

Each log priority value helps in categorizing kernel messages. For instance, error messages can be distinguished from general informational messages.

## Key Difference
---
- `printk()` writes messages to the kernel's ring buffer, which is a cyclical buffer in memory for storing log messages. On the other hand, `printf()` writes messages to the standard output.

- `printk()` allows for setting a log priority which helps in filtering and organizing kernel messages. `printf()` lacks this capability.

Utilizing these functions appropriately is crucial for effective debugging and logging, whether you're in user-space or kernel-space development.

------

