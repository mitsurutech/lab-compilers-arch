# Laboratorio de compiladores y arquitectura de computadores: gcc, flex, bison y ensamblador RISC-V en una VM Ubuntu

[English](README.md) | [Français](README.fr.md) | [日本語](README.ja.md) | [Español](README.es.md)

[![check](https://github.com/mitsurutech/lab-compilers-arch/actions/workflows/check.yml/badge.svg)](https://github.com/mitsurutech/lab-compilers-arch/actions/workflows/check.yml)

Un **laboratorio de compiladores** y de **arquitectura de computadores** listo para clase, de la [plataforma de aprendizaje MitsuruTech](https://mitsurutech.co.uk/es/learning), donde centros educativos y universidades dan a cada estudiante su propia máquina Ubuntu y un terminal en el navegador. Tiene una cadena de herramientas de C real y, al lado, una de RISC-V: **seguir una compilación etapa por etapa** (`gcc -E`, `gcc -S`, `gcc -c`, enlazado), **leer los errores y avisos del compilador** (`gcc -Wall`), **construir un analizador léxico y uno sintáctico** (`flex`, `bison`, `make`), **ensamblar y ejecutar RISC-V** en un emulador (`riscv64-linux-gnu-as`, `qemu-riscv64`, `echo $?`), **leer código máquina** (`objdump -d`) y **medir una caché** con un bucle. No se instala nada en el ordenador del estudiante.

## Qué hay en la máquina

| Qué | Dónde | Para qué |
| --- | --- | --- |
| Un programa pequeño en C con una macro y una función | `~/compiler/hello.c` | `gcc -E`, `gcc -S`, `gcc -c`, enlazado |
| Un programa con una variable sin usar y otra usada fuera de su ámbito | `~/compiler/scope.c` | `gcc -Wall`: un aviso y un error |
| Un analizador de flex, uno de bison y un Makefile para una calculadora de cuatro operaciones | `~/compiler/calc.l`, `calc.y`, `Makefile` | `make` y después `echo "2 + 3 * 4" \| ./calc` |
| Cinco líneas de RISC-V que suman dos números y salen con el resultado | `~/arch/add.s` | `riscv64-linux-gnu-as`, `-ld`, `qemu-riscv64`, `echo $?`, `objdump -d` |
| Un programa que suma una tabla de 64 MB fila a fila y columna a columna | `~/arch/loop.c` | `gcc -O2`, medir una caché, avanzar paso a paso con `gdb` |

La VM no es RISC-V: `qemu-riscv64` ejecuta el programa RISC-V en el procesador que tenga la VM, instrucción a instrucción. El laboratorio no tiene comprobación ni tarea. Las dos lecciones de la plataforma, Compilers Basics y Computer Architecture Basics, son la actividad, y el docente que quiera una nota pide una entrega propia.

## Qué hay en el archivo

`compilers-arch.lab.yaml` es una **definición completa de laboratorio con VM**, exactamente lo que ejecuta la plataforma:

| Clave | Qué es |
| --- | --- |
| `cpus`, `memory`, `disk` | el tamaño de la VM de cada estudiante: 1 CPU, 1024 MB y 10 GB, de sobra para programas de pocas líneas |
| `open` | los puertos que se abren en el navegador: ninguno, el laboratorio es el terminal |
| `setup` | un script que se ejecuta una vez, como root, la primera vez que arranca la VM de cada estudiante: instala gcc, make, binutils, gdb, flex, bison, python3, file, el compilador cruzado de RISC-V y qemu-user desde el archivo de Ubuntu, y escribe los programas de arriba y un README en la carpeta personal |

## Usarlo en la plataforma

1. En tu módulo, añade una tarea y elige la tarjeta **laboratorio con VM Ubuntu**.
2. Pulsa el ejemplo o pega el contenido de `compilers-arch.lab.yaml`. Usar un ejemplo sustituye el texto, y Deshacer recupera el tuyo.
3. Publica y prueba primero tu propia copia. Cada estudiante tiene su propia VM, que se conserva entre sesiones.
4. Asígnalo como cualquier tarea, o abre las lecciones en la biblioteca y pulsa Ejecutar junto a un comando.

La [página de este laboratorio](https://mitsurutech.co.uk/es/help/lab-compilers-arch) en el manual, su [guía de laboratorios con VM Ubuntu](https://mitsurutech.co.uk/es/help/labs) y su [página de laboratorios de ejemplo](https://mitsurutech.co.uk/es/help/sample-labs) cuentan el resto. ¿Aún no tienes cuenta para tu centro? [El primer año es gratis](https://mitsurutech.co.uk/es/pricing) hasta 50 estudiantes, y para [registrarte](https://mitsurutech.co.uk/es/signup) basta una dirección de correo.

## Comprobar el archivo

En cada push, `.github/workflows/check.yml` comprueba que el archivo es YAML válido con las claves de arriba y que `setup` cabe en el límite de 16 KB de la plataforma. No arranca ninguna VM: eso se hace en las máquinas de la plataforma.

## Qué no funciona en un laboratorio

La VM llega al archivo de paquetes de Ubuntu, a PyPI, a npm y a GitHub, y a nada más, así que un laboratorio no puede apuntar a un servidor de internet. Todo lo que necesita este laboratorio se instala desde el archivo de Ubuntu.

## Acerca de

Hecho por [Mitsuru Technologies](https://mitsurutech.co.uk) para la [plataforma de aprendizaje MitsuruTech](https://mitsurutech.co.uk/es/learning): Python, Java, Go, Node, SQL y Linux en el navegador para centros educativos, con laboratorios en contenedor, laboratorios con VM, aplicaciones web desplegadas desde GitHub y cuestionarios que se corrigen solos. Prueba código sin cuenta en el [playground](https://mitsurutech.co.uk/es/playground).

Más laboratorios del mismo sitio: [lab-linux-everyday](https://github.com/mitsurutech/lab-linux-everyday), [lab-linux-admin](https://github.com/mitsurutech/lab-linux-admin), [lab-netlab](https://github.com/mitsurutech/lab-netlab), [lab-git-gitea](https://github.com/mitsurutech/lab-git-gitea) y [lab-sample](https://github.com/mitsurutech/lab-sample).

## Licencia

Esta obra tiene licencia [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es). Puedes compartirla y adaptarla para aprender y enseñar, siempre que cites a Mitsuru Technologies Ltd, no la uses con fines comerciales y compartas lo que hagas a partir de ella con la misma licencia. El texto completo está en el archivo `LICENSE`.

`gcc`, `make`, `binutils`, `gdb`, `flex`, `bison`, `python3`, `file`, `gcc-riscv64-linux-gnu` y `qemu-user` se instalan desde el archivo de Ubuntu al arrancar una VM; aquí no se distribuye nada de ellos.
