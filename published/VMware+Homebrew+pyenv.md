## ある日

```bash
brew doctor
env: Fusion.app/Contents/Public:...中略... No such file or directory
```

Homebrew が死んだ。

ググる。

`/private/etc/paths.d/com.vmware.fusion.public` に書かれているVMwareのパス `/Applications/VMware Fusion.app/Contents/Public` にスペースが入ってるためシェルに読み込まれる時におかしくなるとのこと。

参考: [macos - Getting `No such file or directory` when executing any terminal command (caused by VMware Fusion) - Stack Overflow](https://stackoverflow.com/questions/49722724/getting-no-such-file-or-directory-when-executing-any-terminal-command-caused)


パーミッションを変更する。

```bash
sudo chmod 644 /private/etc/paths.d/com.vmware.fusion.public
```

管理者権限で何らかのエディタを使い下記のように修正する。

```:com.vmware.fusion.public
- /Applications/VMware Fusion.app/Contents/Public
+ /Applications/VMware\ Fusion.app/Contents/Public
```

シェルを再起動し確認する。

```bash
exec $SHELL -l
brew doctor
env: Fusion.app/Contents/Public:...中略... No such file or directory
```

まだ死んでいる。
パスを確認する。

```bash
# 改行して見やすくする
echo $PATH | tr ':' '\n'
...中略...
/Applications/VMware\ Fusion.app/Contents/Public
...中略...
/Applications/VMware Fusion.app/Contents/Public
```

パスが重複して読み込まれている。
ターミナルを開き直してみる。

```bash
echo $PATH | tr ':' '\n'
...中略...
/Applications/VMware\ Fusion.app/Contents/Public
```

重複が消えた。
`exec $SHELL -l` ではだめだということがわかった。

```bash
brew doctor
Your system is ready to brew.
```

生き返った。

だがしかし。

VMware を起動すると `com.vmware.fusion.public` が元に戻ってしまった。
また Homebrew が死んだ。

ググる。

答えに行き着く。

`.zshrc` に書いていた `alias brew="env PATH=${PATH/~\/\.anyenv\/envs\/pyenv\/shims:/} brew"` に `/Applications/VMware Fusion.app/Contents/Public` が読み込まれたときにスペースをエスケープすれば良いとのこと。

そもそもこの alias は何かというと pyenv を使って Python をインストールすると `brew doctor` した時に以下のような Warning が出る。

```bash
brew doctor
Warning: "config" scripts exist outside your system or Homebrew directories.
`./configure` scripts often look for *-config scripts to determine if
software packages are installed, and which additional flags to use when
compiling and linking.

Having additional scripts in your path can confuse software installed via
Homebrew if the config script overrides a system or Homebrew-provided
script of the same name. We found the following "config" scripts:
  /Users/<USER NAME>/.anyenv/envs/pyenv/shims/python3.7-config
  /Users/<USER NAME>/.anyenv/envs/pyenv/shims/python3.7m-config
  /Users/<USER NAME>/.anyenv/envs/pyenv/shims/python-config
  /Users/<USER NAME>/.anyenv/envs/pyenv/shims/python3-config
```

意味は Homebrew の管轄外（pyenv以下）に `config` ファイルがあると混乱するよ。といった感じ。

これを避けるために `brew` コマンドを実行したときに `env PATH=${PATH/~\/\.anyenv\/envs\/pyenv\/shims:/}` でパスから `/pyenv/shims` を削除し何も見なかったことにしてもらう。

で、この処理をする時にスペースが入ったパスが展開されるとエラーを引き起こす。
なので下記の様に修正。

```bash:.zshrc
- alias brew="env PATH=${PATH/~\/\.anyenv\/envs\/pyenv\/shims:/} brew"
+ alias brew="env PATH=\"${PATH/~\/\.anyenv\/envs\/pyenv\/shims:/}\" brew"
```

ターミナルを開き直して確認する。

```bash
brew doctor
Your system is ready to brew.
```

生き返った。
えらい回り道をしてしまった。

参考というか答えそのもの: [【Tips】VMware Fusion 10を使用するとパスがおかしくなる問題の解決法【完全版】 | ソフトアンテナブログ](https://www.softantenna.com/wp/tips/vmware-fusion-10-path-fix/)

## まとめ

* VMWare を使い pyenv を使い `brew doctor` の Warning を回避する alias を使っている時に brew コマンドを実行すると Homebrew は死ぬ。
* alias でパスが展開されるところでスペースをエスケープしてやれば Homebrew 生き返る
* `exec $SHELL -l` はパスが重複して読み込まれちゃう
* はじめからしっかりググろう











