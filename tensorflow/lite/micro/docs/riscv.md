<!--ts-->
* [RISC-V support in Tensorflow Lite for Microcontrollers (TFLM)](#riscv)
   * [Building for RISCV64 target](#riscv64)
<!--te-->

# RISC-V support in Tensorflow Lite for Microcontrollers (TFLM)

This doc contains instructions about how to build TFLM with RISCV64 target.

## Building for RISCV64 target

### Limited support for ARCH, ABI and CODE_MODEL

The building instructions for "riscv64_generic" target were tested for `RISCV_ARCH=rv64gc`, `RISCV_ABI=lp64d`, `RISCV_CODE_MODEL=medany`, as can be seen in [riscv64_generic_makefile.inc](tensorflow/lite/micro/tools/make/targets/riscv64_generic_makefile.inc). If you have different setup, your mileage may vary.

### Prerequisites

* Toolchain

Unlike `riscv32_reneric_makefile.inc`, no attempt is made to download a "default" toolchain, which means that you MUST have your own toolchain installed before trying to build TFLM.

On Ubuntu, the toolchain can be installed with:

    $ apt install gcc-riscv64-linux-gnu g++-riscv64-linux-gnu libc6-dev-riscv64-cross

Make sure to point `TARGET_TOOLCHAIN_ROOT` to "/usr/bin/" later, when running "make".

* Python (numpy and Pillow)

In addition to the bare-minimum Python, a couple of packages must be installed.
First, make sure to create and activate a virtual environment in the project root:

    $ python3 -m venv venv
	$ source ./venv/bin/activate

then install the required packages:

    $ pip install numpy Pillow

* QEMU user mode, optional

You need QEMU only if you are going to run tests for the built library.

Do not confuse "qemu-user-\*" with "qemu-system-\*" packages! On Ubuntu, install the user-mode QEMU with:

    $ apt install qemu-user


### Running Make

Ensure that your python virtual environment is active:

    $ source ./venv/bin/activate

then run the "make" incantation:

    $ make -f tensorflow/lite/micro/tools/make/Makefile TARGET=riscv64_generic TARGET_ARCH=riscv64 TARGET_TOOLCHAIN_ROOT=/usr/bin/


### QEMU

Running tests with QEMU may require specifying "-L /usr/riscv64-linux-gnu" option for qemu. It can be achived by setting "QEMU_EXTRA_ARGS". For example:

    $ make -f tensorflow/lite/micro/tools/make/Makefile test TARGET=riscv64_generic test TARGET_ARCH=riscv64 TARGET_TOOLCHAIN_ROOT=/usr/bin/ QEMU_EXTRA_ARGS="-L /usr/riscv64-linux-gnu"
