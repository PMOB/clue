# Hello, C language

ここでは各言語で言う`Hello, World!`を終えることが目標となります。

# 開発環境の構築

Windowsの方は「コマンドプロンプト」
MacまたはLinuxの方は「ターミナル」
について基本的なコマンドについての理解が必要です。

IDEを使用する場合はとりあえず #question に投げてください。

## Windows

まだない。

## Mac or Linux

1. ターミナルを開きます。
1. ```gcc```コマンドが使用できることを確認してください。

お し ま い。

## IDE(統合開発環境)を使用する場合

- Xcode
- Visual Studio
- Clion

などがあります。詳しくはググって(Googleで調べて)。

# プログラムを書く

```C
#include <stdio.h>

int main() {
  printf("Hello, World!\n");
}
```

上記の内容を`hello.c`というファイルを書き、
任意のフォルダに保存します。

# コンパイル & 実行

ターミナル(またはコマンドプロンプト)で先程の保存したファイルのあるフォルダまで移動し、
以下のコマンドでコンパイルします。

```
$ gcc hello.c -o hello
```

すると、`hello`という実行できるプログラムが出来ているので実行

```
$ ./hello
Hello, World!
```

おしまい。
