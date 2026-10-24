# Lab de compilation et d’architecture des ordinateurs : gcc, flex, bison et assembleur RISC-V dans une VM Ubuntu

[English](README.md) | [Français](README.fr.md) | [日本語](README.ja.md) | [Español](README.es.md)

[![check](https://github.com/mitsurutech/lab-compilers-arch/actions/workflows/check.yml/badge.svg)](https://github.com/mitsurutech/lab-compilers-arch/actions/workflows/check.yml)

Un **lab de compilation** et d’**architecture des ordinateurs** prêt à l’emploi pour la classe, issu de la [plateforme d’apprentissage MitsuruTech](https://mitsurutech.co.uk/fr/learning), où les écoles, les établissements et les universités donnent à chaque étudiant sa propre machine Ubuntu et un terminal dans le navigateur. Il contient une vraie chaîne C et, à côté, une chaîne RISC-V : **suivre une compilation étape par étape** (`gcc -E`, `gcc -S`, `gcc -c`, édition de liens), **lire les erreurs et les avertissements du compilateur** (`gcc -Wall`), **construire un analyseur lexical et un analyseur syntaxique** (`flex`, `bison`, `make`), **assembler et lancer du RISC-V** sur un émulateur (`riscv64-linux-gnu-as`, `qemu-riscv64`, `echo $?`), **lire du code machine** (`objdump -d`) et **chronométrer un cache** avec une boucle. Rien n’est installé sur l’ordinateur de l’étudiant.

## Ce que contient la machine

| Quoi | Où | Pour |
| --- | --- | --- |
| Un petit programme C avec une macro et une fonction | `~/compiler/hello.c` | `gcc -E`, `gcc -S`, `gcc -c`, édition de liens |
| Un programme avec une variable inutilisée et une autre utilisée hors de sa portée | `~/compiler/scope.c` | `gcc -Wall` : un avertissement et une erreur |
| Un analyseur flex, un analyseur bison et un Makefile pour une calculatrice à quatre opérations | `~/compiler/calc.l`, `calc.y`, `Makefile` | `make`, puis `echo "2 + 3 * 4" \| ./calc` |
| Cinq lignes de RISC-V qui additionnent deux nombres et sortent avec le résultat | `~/arch/add.s` | `riscv64-linux-gnu-as`, `-ld`, `qemu-riscv64`, `echo $?`, `objdump -d` |
| Un programme qui additionne un tableau de 64 Mo ligne par ligne puis colonne par colonne | `~/arch/loop.c` | `gcc -O2`, mesurer un cache, avancer pas à pas dans `gdb` |

La VM n’est pas en RISC-V : `qemu-riscv64` exécute le programme RISC-V sur le processeur de la VM, une instruction après l’autre. Le lab n’a ni vérification ni tâche. Les deux leçons de la plateforme, Compilers Basics et Computer Architecture Basics, sont l’activité, et un enseignant qui veut une note prépare lui-même un rendu.

## Ce que contient le fichier

`compilers-arch.lab.yaml` est une **définition complète de lab VM**, exactement ce que la plateforme exécute :

| Clé | Ce que c’est |
| --- | --- |
| `cpus`, `memory`, `disk` | la taille de la VM de chaque étudiant : 1 processeur, 1024 Mo et 10 Go, largement assez pour des programmes de quelques lignes |
| `open` | les ports ouverts dans le navigateur : aucun, le lab est le terminal |
| `setup` | un script lancé une fois, en root, au premier démarrage de la VM de chaque étudiant : il installe gcc, make, binutils, gdb, flex, bison, python3, file, le compilateur croisé RISC-V et qemu-user depuis l’archive d’Ubuntu, et écrit les programmes ci-dessus et un README dans le dossier personnel |

## L’utiliser sur la plateforme

1. Dans votre module, ajoutez une tâche et choisissez la carte **Lab VM Ubuntu**.
2. Appuyez sur l’exemple, ou collez le contenu de `compilers-arch.lab.yaml`. Utiliser un exemple remplace le texte, et Annuler rétablit le vôtre.
3. Publiez et essayez d’abord votre propre copie. Chaque étudiant a sa propre VM, conservée d’une séance à l’autre.
4. Attribuez-le comme n’importe quelle tâche, ou ouvrez les leçons dans la bibliothèque et appuyez sur Exécuter à côté d’une commande.

La [page de ce lab](https://mitsurutech.co.uk/fr/help/lab-compilers-arch) dans le manuel, son [guide des labs VM Ubuntu](https://mitsurutech.co.uk/fr/help/labs) et sa [page des labs d’exemple](https://mitsurutech.co.uk/fr/help/sample-labs) disent le reste. Pas encore de compte pour votre établissement ? [La première année est gratuite](https://mitsurutech.co.uk/fr/pricing) jusqu’à 50 étudiants, et [l’inscription](https://mitsurutech.co.uk/fr/signup) ne demande qu’une adresse e-mail.

## Vérifier le fichier

À chaque push, `.github/workflows/check.yml` vérifie que le fichier est un YAML valide avec les clés ci-dessus et que `setup` reste sous la limite de 16 Ko de la plateforme. Il ne démarre pas de VM : cela se fait sur les machines de la plateforme.

## Ce qui ne marche pas dans un lab

La VM atteint l’archive des paquets d’Ubuntu, PyPI, npm et GitHub, et rien d’autre : un lab ne peut donc pas viser un hôte sur internet. Tout ce dont ce lab a besoin s’installe depuis l’archive d’Ubuntu.

## À propos

Réalisé par [Mitsuru Technologies](https://mitsurutech.co.uk) pour la [plateforme d’apprentissage MitsuruTech](https://mitsurutech.co.uk/fr/learning) : Python, Java, Go, Node, SQL et Linux dans le navigateur pour les écoles et les établissements, avec des labs en conteneur, des labs VM, des applications web déployées depuis GitHub et des quiz qui se corrigent tout seuls. Essayez du code sans compte dans le [bac à sable](https://mitsurutech.co.uk/fr/playground).

D’autres labs de la même source : [lab-linux-everyday](https://github.com/mitsurutech/lab-linux-everyday), [lab-linux-admin](https://github.com/mitsurutech/lab-linux-admin), [lab-netlab](https://github.com/mitsurutech/lab-netlab), [lab-git-gitea](https://github.com/mitsurutech/lab-git-gitea) et [lab-sample](https://github.com/mitsurutech/lab-sample).

## Licence

Cette œuvre est sous licence [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.fr). Vous pouvez la partager et l’adapter pour apprendre et enseigner, à condition de citer Mitsuru Technologies Ltd, de ne pas en faire un usage commercial et de partager ce que vous en tirez sous la même licence. Le texte complet est dans le fichier `LICENSE`.

`gcc`, `make`, `binutils`, `gdb`, `flex`, `bison`, `python3`, `file`, `gcc-riscv64-linux-gnu` et `qemu-user` sont installés depuis l’archive d’Ubuntu au démarrage d’une VM ; rien d’eux n’est distribué ici.
