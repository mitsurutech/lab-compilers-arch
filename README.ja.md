# コンパイラとコンピュータアーキテクチャのラボ：gcc、flex、bison、RISC-V アセンブリを1台の Ubuntu VM で

[English](README.md) | [Français](README.fr.md) | [日本語](README.ja.md) | [Español](README.es.md)

[![check](https://github.com/mitsurutech/lab-compilers-arch/actions/workflows/check.yml/badge.svg)](https://github.com/mitsurutech/lab-compilers-arch/actions/workflows/check.yml)

授業ですぐ使える**コンパイラのラボ**、そして**コンピュータアーキテクチャのラボ**です。[MitsuruTech 学習プラットフォーム](https://mitsurutech.co.uk/ja/learning)のもので、学校や大学が学生一人ひとりに Ubuntu マシンとブラウザーのターミナルを用意します。本物の C ツールチェーンと、その横に RISC-V のツールチェーンがあります。**ビルドを段階ごとに見る**（`gcc -E`、`gcc -S`、`gcc -c`、リンク）、**コンパイラのエラーと警告を読む**（`gcc -Wall`）、**字句解析器と構文解析器を作る**（`flex`、`bison`、`make`）、**RISC-V をアセンブルしてエミュレータで動かす**（`riscv64-linux-gnu-as`、`qemu-riscv64`、`echo $?`）、**機械語を読む**（`objdump -d`）、そして**ループでキャッシュを計測する**ことができます。学生のパソコンには何もインストールしません。

## マシンにあるもの

| 何 | どこ | 使い道 |
| --- | --- | --- |
| マクロと関数がある小さな C のプログラム | `~/compiler/hello.c` | `gcc -E`、`gcc -S`、`gcc -c`、リンク |
| 使われない変数と、スコープの外で使われる変数があるプログラム | `~/compiler/scope.c` | `gcc -Wall`：警告とエラー |
| 四則演算の電卓用の flex の字句解析器、bison の構文解析器、Makefile | `~/compiler/calc.l`、`calc.y`、`Makefile` | `make`、続けて `echo "2 + 3 * 4" \| ./calc` |
| 2つの数を足し、答えを終了ステータスにする5行の RISC-V | `~/arch/add.s` | `riscv64-linux-gnu-as`、`-ld`、`qemu-riscv64`、`echo $?`、`objdump -d` |
| 64 MB の表を行ごとと列ごとに足すプログラム | `~/arch/loop.c` | `gcc -O2`、キャッシュの計測、`gdb` での1命令ずつの実行 |

VM は RISC-V ではありません。`qemu-riscv64` が、VM のプロセッサの上で RISC-V のプログラムを命令1つずつ実行します。このラボには検査もタスクもありません。プラットフォームの2つのレッスン、Compilers Basics と Computer Architecture Basics が活動そのものです。成績が必要な先生は、独自の提出物を出してください。

## ファイルの中身

`compilers-arch.lab.yaml` は **VM ラボの定義のすべて**で、プラットフォームが動かすものそのものです。

| キー | 内容 |
| --- | --- |
| `cpus`、`memory`、`disk` | 学生ごとの VM の大きさ。CPU 1つ、1024 MB、10 GB。数行のプログラムには十分です |
| `open` | ブラウザーで開くポート。なし。ラボはターミナルです |
| `setup` | 学生の VM が初めて起動したとき、root で1回だけ動くスクリプト。Ubuntu のアーカイブから gcc、make、binutils、gdb、flex、bison、python3、file、RISC-V のクロスコンパイラ、qemu-user を入れ、上のプログラムと README をホームフォルダーに書きます |

## プラットフォームで使う

1. モジュールでタスクを追加し、**Ubuntu VM ラボ**のカードを選びます。
2. サンプルを押すか、`compilers-arch.lab.yaml` の中身を貼り付けます。サンプルを使うと本文が置き換わり、元に戻すで自分の本文に戻せます。
3. 公開して、まず自分のコピーで試します。学生はそれぞれ自分の VM を持ち、それは授業をまたいで残ります。
4. ほかのタスクと同じように割り当てるか、ライブラリでレッスンを開き、コマンドの横の実行を押します。

ヘルプマニュアルの[このラボのページ](https://mitsurutech.co.uk/ja/help/lab-compilers-arch)、[Ubuntu VM ラボのガイド](https://mitsurutech.co.uk/ja/help/labs)、[サンプルラボのページ](https://mitsurutech.co.uk/ja/help/sample-labs)に、そのほかのことが書いてあります。学校のアカウントがまだですか。学生 50 人までなら[最初の1年は無料](https://mitsurutech.co.uk/ja/pricing)で、[登録](https://mitsurutech.co.uk/ja/signup)にはメールアドレスだけが必要です。

## ファイルの検査

push のたびに、`.github/workflows/check.yml` がファイルが正しい YAML で上のキーを持つこと、`setup` がプラットフォームの 16 KB の上限に収まることを確かめます。VM は起動しません。それはプラットフォーム自身のマシンで行います。

## ラボでできないこと

VM が届くのは、Ubuntu のパッケージアーカイブ、PyPI、npm、GitHub だけです。そのため、ラボからインターネット上のホストには接続できません。このラボに必要なものは、すべて Ubuntu のアーカイブから入ります。

## このリポジトリについて

[Mitsuru Technologies](https://mitsurutech.co.uk) が [MitsuruTech 学習プラットフォーム](https://mitsurutech.co.uk/ja/learning)のために作りました。学校向けに、Python、Java、Go、Node、SQL、Linux をブラウザーで使え、コンテナラボ、VM ラボ、GitHub からデプロイする Web アプリ、自動で採点されるクイズがあります。アカウントなしで[プレイグラウンド](https://mitsurutech.co.uk/ja/playground)でコードを試せます。

同じところのほかのラボ：[lab-linux-everyday](https://github.com/mitsurutech/lab-linux-everyday)、[lab-linux-admin](https://github.com/mitsurutech/lab-linux-admin)、[lab-netlab](https://github.com/mitsurutech/lab-netlab)、[lab-git-gitea](https://github.com/mitsurutech/lab-git-gitea)、[lab-sample](https://github.com/mitsurutech/lab-sample)。

## ライセンス

この作品は [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.ja) で提供しています。Mitsuru Technologies Ltd の名前を示し、商用に使わず、作ったものを同じライセンスで共有するなら、学習と授業のために共有し、改変できます。全文は `LICENSE` ファイルにあります。

`gcc`、`make`、`binutils`、`gdb`、`flex`、`bison`、`python3`、`file`、`gcc-riscv64-linux-gnu`、`qemu-user` は、VM の起動時に Ubuntu のアーカイブから入ります。ここではそれらを一切配布していません。
