ドキュメント
========

- [原文](https://docs.python.org/3/)
- [日本語版](https://docs.python.jp/3/)


開発環境
========

Pythonの環境について
--------
Pythonは現在、大きく分けてPython2とPython3というバージョンがあります。
Python3が新しいバージョンで、Python2系は2020年にサポートが終了するので、基本的にPython3を利用してください。
この二つには互換性がない部分があり、古いライブラリを利用する場合にPython2系を利用する必要がある場合があります。
その場合は注意してPython2系を利用してください。

上記の理由や開発の利便性により、Pythonを利用する開発者は複数のPythonバージョンを利用したい場合があります。
これを解決するため、以下に紹介する `pyenv` を利用してください。

pyenvのインストール
--------

- Linuxを利用している場合

```sh
$ curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
```

- macOSでHomebrewを利用している場合

```sh
$ brew update
$ brew install pyenv
```

### Pythonのインストール
例えば、Python3.5.1をインストールしたい場合、pyenvを用いて以下のようにインストールすることができます。

```sh
$ pyenv install 3.5.1
```

この段階ではまだインストールしただけで、利用することはできません。
利用できるバージョンは

```sh
$ pyenv install --list
```

で確認することができます。
annacondaとかpypyとかあるよ。

### Pythonバージョンの変更
インストールしたPythonを、以下のコマンドで選択します。

```sh
$ pyenv global 3.5.1
```

これでPythonのバージョンがPython3.5.1になりました。
`python --version` コマンドで確認してみてください。

### pyenvのその他もろもろ
https://github.com/yyuu/pyenv/blob/master/README.md


チュートリアル
========
Pythonは近年のパラダイムも取り入れている、
オブジェクト指向を基本としたマルチパラダイムのスクリプト言語です。
ピンと来なかった人は公式のチュートリアルを実行してください。

https://docs.python.jp/3/tutorial/index.html

とりあえず [3, 4, 5, 6, 8, 9] 章あたりはやっておいてください。


ライブラリの利用
========
Pythonには強力なライブラリがいくつも存在します。それらの利用方法を紹介します。

pip
--------
Pythonには *pip* というマネージャが存在します。

### pipを用いたライブラリのインストール
試しにpipを用いて BeautifulSoup4 というライブラリをインストールしてみましょう。

```sh
$ pip install beautifulsoup4
```

インストールできました。簡単ですね。
利用したいライブラリの Installation を読んでからインストールするようにしましょう。

### pipのインストール先
pipがインストールするのは、pyenvで指定している現在のPythonバージョンのみです。
例えば Python3.5.1 と Python2.7.3 をインストールしていて、 Python3.5.1 を利用している場合、 `pip install beautifulsoup4` を行っても Python2.7.3 の環境にはインストールされません。
各バージョンは隔離されています。

現在の環境にpipでインストールされているライブラリの一覧を見るために以下のコマンドを利用できます。

```sh
$ pip freeze
```
