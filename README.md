# Compilers and computer architecture lab: gcc, flex, bison and RISC-V assembly in one Ubuntu VM

[English](README.md) | [Français](README.fr.md) | [日本語](README.ja.md) | [Español](README.es.md)

[![check](https://github.com/mitsurutech/lab-compilers-arch/actions/workflows/check.yml/badge.svg)](https://github.com/mitsurutech/lab-compilers-arch/actions/workflows/check.yml)

A ready-made **compilers lab** and **computer architecture lab** for classrooms, from the [MitsuruTech Learning Platform](https://mitsurutech.co.uk/learning), where schools, colleges and universities give every student their own Ubuntu machine and a terminal in the browser. It has a real C toolchain and a RISC-V one beside it: **watch a build stage by stage** (`gcc -E`, `gcc -S`, `gcc -c`, linking), **read compiler errors and warnings** (`gcc -Wall`), **build a lexer and a parser** (`flex`, `bison`, `make`), **assemble and run RISC-V** on an emulator (`riscv64-linux-gnu-as`, `qemu-riscv64`, `echo $?`), **read machine code** (`objdump -d`) and **time a cache** with a loop. Nothing is installed on the student's computer.

## What is on the machine

| What | Where | Used for |
| --- | --- | --- |
| A small C program with a macro and a function | `~/compiler/hello.c` | `gcc -E`, `gcc -S`, `gcc -c`, linking |
| A program with an unused variable and one used out of its scope | `~/compiler/scope.c` | `gcc -Wall`: a warning and an error |
| A flex lexer, a bison parser and a Makefile for a four-operation calculator | `~/compiler/calc.l`, `calc.y`, `Makefile` | `make`, then `echo "2 + 3 * 4" \| ./calc` |
| Five lines of RISC-V that add two numbers and exit with the answer | `~/arch/add.s` | `riscv64-linux-gnu-as`, `-ld`, `qemu-riscv64`, `echo $?`, `objdump -d` |
| A program that sums a 64 MB table row by row and column by column | `~/arch/loop.c` | `gcc -O2`, timing a cache, stepping with `gdb` |

The VM is not RISC-V: `qemu-riscv64` runs the RISC-V program on whatever processor the VM has, one instruction at a time. The lab has no check and no task. The two lessons on the platform, Compilers Basics and Computer Architecture Basics, are the activity, and a teacher who wants a mark sets a hand-in of their own.

## What is in the file

`compilers-arch.lab.yaml` is a whole **VM lab definition**, exactly what the platform runs:

| Key | What it is |
| --- | --- |
| `cpus`, `memory`, `disk` | the size of each student's VM: 1 CPU, 1024 MB and 10 GB, plenty for programs of a few lines |
| `open` | the ports that open in the browser: none, the lab is the terminal |
| `setup` | a script run once, as root, the first time each student's VM starts: it installs gcc, make, binutils, gdb, flex, bison, python3, file, the RISC-V cross compiler and qemu-user from Ubuntu's archive, writes the programs above and a README in the home folder |

## Using it on the platform

1. In your module, add a task and choose the **Ubuntu VM lab** card.
2. Press the sample, or paste the contents of `compilers-arch.lab.yaml`. Using a sample replaces the text, and Undo brings yours back.
3. Publish and try your own copy first. Every student gets a VM of their own, kept between sessions.
4. Assign it like any task, or open the lessons in the library and press Run beside a command.

The help manual's [page for this lab](https://mitsurutech.co.uk/help/lab-compilers-arch), its [guide to Ubuntu VM labs](https://mitsurutech.co.uk/help/labs) and its [sample labs page](https://mitsurutech.co.uk/help/sample-labs) cover the rest. No college account yet? [Your first year is free](https://mitsurutech.co.uk/pricing) for up to 50 students, and [signing up](https://mitsurutech.co.uk/signup) takes an email address.

## Checking the file

On every push, `.github/workflows/check.yml` checks that the file is valid YAML with the keys above and that `setup` is inside the platform's 16 KB limit. It does not boot a VM: that is done on the platform's own machines.

## What does not work in a lab

The VM reaches Ubuntu's package archive, PyPI, npm and GitHub, and nothing else, so a lab cannot be pointed at a host on the internet. Everything this lab needs is installed from Ubuntu's archive.

## About

Made by [Mitsuru Technologies](https://mitsurutech.co.uk) for the [MitsuruTech Learning Platform](https://mitsurutech.co.uk/learning): Python, Java, Go, Node, SQL and Linux in the browser for schools and colleges, with container labs, VM labs, web apps deployed from GitHub, and quizzes that mark themselves. Try code with no account in the [playground](https://mitsurutech.co.uk/playground).

More labs from the same place: [lab-linux-everyday](https://github.com/mitsurutech/lab-linux-everyday), [lab-linux-admin](https://github.com/mitsurutech/lab-linux-admin), [lab-netlab](https://github.com/mitsurutech/lab-netlab), [lab-git-gitea](https://github.com/mitsurutech/lab-git-gitea) and [lab-sample](https://github.com/mitsurutech/lab-sample).

## Licence

This work is licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/). You may share it and adapt it for learning and teaching, if you credit Mitsuru Technologies Ltd, do not use it commercially, and share anything you make from it under the same licence. The full text is in the `LICENSE` file.

`gcc`, `make`, `binutils`, `gdb`, `flex`, `bison`, `python3`, `file`, `gcc-riscv64-linux-gnu` and `qemu-user` are installed from Ubuntu's archive when a VM starts; nothing of them is distributed here.
