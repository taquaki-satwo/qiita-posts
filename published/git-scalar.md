Git Scalarで巨大リポジトリを高速cloneする

この記事は [ZOZO Advent Calendar 2022 #2 24日目](https://qiita.com/advent-calendar/2022/zozo) の記事です。

# Scalarとは

今年の10月にリリースされたgit 2.38に内蔵された大規模リポジトリ向けツールです。
Scalar自体はMicrosoftが以前から開発を進めていたものになります。

- [Highlights from Git 2.38 | The GitHub Blog](https://github.blog/2022-10-03-highlights-from-git-2-38/)
- [microsoft/scalar: Scalar: A set of tools and extensions for Git to allow very large monorepos to run on Git without a virtualization layer](https://github.com/microsoft/scalar/)

# 何ができるのか

Scalarは以下の機能を提供します。

- Partial clone
  - cloneするときに、すべてのGitオブジェクトをダウンロードしない。それによって取得時間を短縮する。
  - 参考) [Get up to speed with partial clone and shallow clone | The GitHub Blog](https://github.blog/2020-12-21-get-up-to-speed-with-partial-clone-and-shallow-clone/#partial-clone)
- Background prefetch
  - 1時間ごとにリモートリポジトリからGitオブジェクトをダウンロードし、git fetchの実行時間を短縮する。
  - 参考) [Highlights from Git 2.31 | The GitHub Blog](https://github.blog/2021-03-15-highlights-from-git-2-31/#introducing-git-maintenance)
- Sparse-checkout
  - 作業ディレクトリのサイズを制限する。
  - 参考) [Bring your monorepo down to size with sparse-checkout | The GitHub Blog](https://github.blog/2020-01-17-bring-your-monorepo-down-to-size-with-sparse-checkout/#cone-mode-restricted-patterns-improve-performance)
- File system monitor
  - 最近の更新ファイルを追跡し、ワークツリー全てをスキャンさせないようにする。
  - 参考) [Improve Git monorepo performance with a file system monitor | The GitHub Blog](https://github.blog/2022-06-29-improve-git-monorepo-performance-with-a-file-system-monitor/)
- Commit-graph
  - コミット・ウォークと到達可能性計算を高速化しgit logなどのコマンドを高速化する。
  - 参考) [Git's database internals II: commit history queries | The GitHub Blog](https://github.blog/2022-08-30-gits-database-internals-ii-commit-history-queries/#gits-commit-graph-file)
- Multi-pack-index
  - 複数のパックファイル（複数のGitオブジェクトをまとめて圧縮したバイナリファイル）からオブジェクトを高速に検索できるようになる。
  - 参考） [Scaling monorepo maintenance | The GitHub Blog](https://github.blog/2021-04-29-scaling-monorepo-maintenance/)
- Incremental repack
  - Multi-pack-indexを使用し、実行中のコマンドを中断させずに、パックされたGitデータをより少ないパックファイルにリパックする。

# はじめかた

バージョン2.38以降のGitをインストールすればscalarコマンドが使えます。  
リモートリポジトリからcloneする際にScalarの機能を有効化する場合は、下記のコマンドを実行します。

```
scalar clone /path/to/repo
```

既存のローカルリポジトリに対しては、下記のコマンドを実行します。

```
cd /path/to/repo
scalar register
```

# 比較してみる

試しにcommit数10万を超える [Visual Studio Code](https://github.com/microsoft/vscode) のリポジトリを使って比較したいと思います。
ちなみに、マシンスペックは下記です。

```
system_profiler SPHardwareDataType
Hardware:

    Hardware Overview:

      Model Name: MacBook Pro
      Model Identifier: MacBookPro18,1
      Chip: Apple M1 Pro
      Total Number of Cores: 10 (8 performance and 2 efficiency)
      Memory: 32 GB
```

## cloneとstatus

まずは、通常のclone。

```
time git clone git@github.com:microsoft/vscode.git
Cloning into 'vscode'...
remote: Enumerating objects: 1358701, done.
remote: Counting objects: 100% (51/51), done.
remote: Compressing objects: 100% (42/42), done.
remote: Total 1358701 (delta 16), reused 26 (delta 9), pack-reused 1358650
Receiving objects: 100% (1358701/1358701), 821.71 MiB | 5.73 MiB/s, done.
Resolving deltas: 100% (841866/841866), done.
Updating files: 100% (6382/6382), done.
git clone git@github.com:microsoft/vscode.git  53.98s user 21.04s system 48% cpu 2:35.57 total
```

git statusを実行。

```
time git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
git status  0.02s user 0.15s system 69% cpu 0.248 total
```

次にScalarを使ったclone。

```
time scalar clone git@github.com:microsoft/vscode.git
Initialized empty Git repository in /Users/hoge
warning: fetch normally indicates which branches had a forced update,
but that check has been disabled; to re-enable, use '--show-forced-updates'
flag or run 'git config fetch.showForcedUpdates true'
warning: fetch normally indicates which branches had a forced update,
but that check has been disabled; to re-enable, use '--show-forced-updates'
flag or run 'git config fetch.showForcedUpdates true'
remote: Enumerating objects: 23, done.
remote: Counting objects: 100% (18/18), done.
remote: Compressing objects: 100% (17/17), done.
remote: Total 23 (delta 1), reused 1 (delta 1), pack-reused 5
Receiving objects: 100% (23/23), 261.01 KiB | 569.00 KiB/s, done.
Resolving deltas: 100% (1/1), done.
warning: fetch normally indicates which branches had a forced update,
but that check has been disabled; to re-enable, use '--show-forced-updates'
flag or run 'git config fetch.showForcedUpdates true'
Updating files: 100% (23/23), done.
branch 'main' set up to track 'origin/main'.
Already on 'main'
Your branch is up to date with 'origin/main'.
scalar clone git@github.com:microsoft/vscode.git  9.42s user 4.78s system 38% cpu 36.465 total
```

git statusを実行。

```
time git status
On branch main
Your branch is up to date with 'origin/main'.

You are in a sparse checkout with 1% of tracked files present.

nothing to commit, working tree clean
git status  0.01s user 0.02s system 12% cpu 0.180 total
```

Enumerating objectsが非常に少なくなり、clone時間が1/5以下に短縮されました。またstatusも早くなっているのが確認できました。

## config

.git/configを比較します。  
まずは、通常のcloneで取得したリポジトリのconfig。

```
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
	ignorecase = true
	precomposeunicode = true
[remote "origin"]
	url = git@github.com:microsoft/vscode.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "main"]
	remote = origin
	merge = refs/heads/main
```

次にScalarを使ったcloneで取得したリポジトリのconfig。

```
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
	ignorecase = true
	precomposeunicode = true
	FSCache = true
	multiPackIndex = true
	preloadIndex = true
	untrackedCache = true
	autoCRLF = false
	safeCRLF = false
	fsmonitor = true
[remote "origin"]
	url = git@github.com:microsoft/vscode.git
	fetch = +refs/heads/*:refs/remotes/origin/*
	promisor = true
	partialCloneFilter = blob:none
[extensions]
	worktreeConfig = true
[am]
	keepCR = true
[credential "https://dev.azure.com"]
	useHttpPath = true
[credential]
	validate = false
[gc]
	auto = 0
[gui]
	GCWarning = false
[index]
	threads = true
	version = 4
[merge]
	stat = false
	renames = true
[pack]
	useBitmaps = false
	useSparse = true
[receive]
	autoGC = false
[feature]
	manyFiles = false
	experimental = false
[fetch]
	unpackLimit = 1
	writeCommitGraph = false
	showForcedUpdates = false
[status]
	aheadBehind = false
[commitGraph]
	generationVersion = 1
[log]
	excludeDecoration = refs/prefetch/*
[branch "main"]
	remote = origin
	merge = refs/heads/main
[maintenance]
	auto = false
	strategy = incremental
```

Scalarによって、Partial clone（partialCloneFilter）やFile system monitor（fsmonitor）、Multi-pack-index（multiPackIndex）などの設定が追加されているのがわかります。
また、先ほどcloneした際に出ていたwarningはScalarがshowForcedUpdatesオプションをfalseにしたためということがわかりました。

## ファイル

ダウンロードしたファイルを比較してみます。
まずは、通常のcloneでダウンロードしたファイル。

```
ls -laf
total 1736
drwxr-xr-x  38 takaaki.sato  staff    1216 12 24 21:25 .
drwxr-xr-x   4 takaaki.sato  staff     128 12 24 21:23 ..
-rw-r--r--   1 takaaki.sato  staff  181356 12 24 21:25 ThirdPartyNotices.txt
-rw-r--r--   1 takaaki.sato  staff     101 12 24 21:25 .yarnrc
drwxr-xr-x  11 takaaki.sato  staff     352 12 24 21:25 test
drwxr-xr-x   7 takaaki.sato  staff     224 12 24 21:25 resources
-rw-r--r--   1 takaaki.sato  staff     116 12 24 21:25 .lsifrc.json
drwxr-xr-x   9 takaaki.sato  staff     288 12 24 21:25 cli
drwxr-xr-x   6 takaaki.sato  staff     192 12 24 21:25 .devcontainer
-rw-r--r--   1 takaaki.sato  staff     365 12 24 21:25 .editorconfig
drwxr-xr-x  98 takaaki.sato  staff    3136 12 24 21:25 extensions
-rw-r--r--   1 takaaki.sato  staff    6861 12 24 21:25 README.md
-rw-r--r--   1 takaaki.sato  staff     153 12 24 21:25 .mention-bot
-rw-r--r--   1 takaaki.sato  staff  509411 12 24 21:25 yarn.lock
-rw-r--r--   1 takaaki.sato  staff     746 12 24 21:25 tsfmt.json
-rw-r--r--   1 takaaki.sato  staff     227 12 24 21:25 .gitignore
-rw-r--r--   1 takaaki.sato  staff    9462 12 24 21:25 package.json
-rw-r--r--   1 takaaki.sato  staff    5696 12 24 21:25 CONTRIBUTING.md
-rw-r--r--   1 takaaki.sato  staff       6 12 24 21:25 .nvmrc
drwxr-xr-x  29 takaaki.sato  staff     928 12 24 21:25 scripts
drwxr-xr-x  14 takaaki.sato  staff     448 12 24 21:25 .github
-rw-r--r--   1 takaaki.sato  staff     167 12 24 21:25 .gitattributes
-rw-r--r--   1 takaaki.sato  staff     382 12 24 21:25 gulpfile.js
-rw-r--r--   1 takaaki.sato  staff    1306 12 24 21:25 .eslintignore
drwxr-xr-x  34 takaaki.sato  staff    1088 12 24 21:25 build
-rw-r--r--   1 takaaki.sato  staff    2758 12 24 21:25 product.json
drwxr-xr-x  24 takaaki.sato  staff     768 12 24 21:25 .eslintplugin
-rw-r--r--   1 takaaki.sato  staff     265 12 24 21:25 .mailmap
-rw-r--r--   1 takaaki.sato  staff     699 12 24 21:25 .git-blame-ignore
drwxr-xr-x  13 takaaki.sato  staff     416 12 24 22:44 .git
-rw-r--r--   1 takaaki.sato  staff    1109 12 24 21:25 LICENSE.txt
drwxr-xr-x  10 takaaki.sato  staff     320 12 24 21:25 .vscode
-rw-r--r--   1 takaaki.sato  staff   60677 12 24 21:25 cgmanifest.json
-rw-r--r--   1 takaaki.sato  staff   15092 12 24 21:25 .eslintrc.json
-rw-r--r--   1 takaaki.sato  staff    2780 12 24 21:25 SECURITY.md
-rw-r--r--   1 takaaki.sato  staff   20522 12 24 21:25 cglicenses.json
drwxr-xr-x  22 takaaki.sato  staff     704 12 24 21:25 src
drwxr-xr-x   6 takaaki.sato  staff     192 12 24 21:25 remote
```

次にScalarを使ったcloneでダウンロードしたファイル。

```
cd src
ls -laf
total 1736
drwxr-xr-x  26 takaaki.sato  staff     832 12 24 21:38 .
drwxr-xr-x   3 takaaki.sato  staff      96 12 24 21:37 ..
-rw-r--r--   1 takaaki.sato  staff  181356 12 24 21:38 ThirdPartyNotices.txt
-rw-r--r--   1 takaaki.sato  staff     101 12 24 21:38 .yarnrc
-rw-r--r--   1 takaaki.sato  staff     116 12 24 21:38 .lsifrc.json
-rw-r--r--   1 takaaki.sato  staff     365 12 24 21:38 .editorconfig
-rw-r--r--   1 takaaki.sato  staff    6861 12 24 21:38 README.md
-rw-r--r--   1 takaaki.sato  staff     153 12 24 21:38 .mention-bot
-rw-r--r--   1 takaaki.sato  staff  509411 12 24 21:38 yarn.lock
-rw-r--r--   1 takaaki.sato  staff     746 12 24 21:38 tsfmt.json
-rw-r--r--   1 takaaki.sato  staff     227 12 24 21:38 .gitignore
-rw-r--r--   1 takaaki.sato  staff    9462 12 24 21:38 package.json
-rw-r--r--   1 takaaki.sato  staff    5696 12 24 21:38 CONTRIBUTING.md
-rw-r--r--   1 takaaki.sato  staff       6 12 24 21:38 .nvmrc
-rw-r--r--   1 takaaki.sato  staff     167 12 24 21:38 .gitattributes
-rw-r--r--   1 takaaki.sato  staff     382 12 24 21:38 gulpfile.js
-rw-r--r--   1 takaaki.sato  staff    1306 12 24 21:38 .eslintignore
-rw-r--r--   1 takaaki.sato  staff    2758 12 24 21:38 product.json
-rw-r--r--   1 takaaki.sato  staff     265 12 24 21:38 .mailmap
-rw-r--r--   1 takaaki.sato  staff     699 12 24 21:38 .git-blame-ignore
drwxr-xr-x  15 takaaki.sato  staff     480 12 24 22:45 .git
-rw-r--r--   1 takaaki.sato  staff    1109 12 24 21:38 LICENSE.txt
-rw-r--r--   1 takaaki.sato  staff   60677 12 24 21:38 cgmanifest.json
-rw-r--r--   1 takaaki.sato  staff   15092 12 24 21:38 .eslintrc.json
-rw-r--r--   1 takaaki.sato  staff    2780 12 24 21:38 SECURITY.md
-rw-r--r--   1 takaaki.sato  staff   20522 12 24 21:38 cglicenses.json
```

Scalarの特徴の1つですが、リポジトリにsrcディレクトリが自動で作成され、その下にファイルがダウンロードされます。
さらに、ファイルのみでディレクトリが無いことが確認できます。これはPartial cloneとSparse-checkoutによって、ダウンロードするオブジェクトを制限しているためです。これによって高速なcloneができるようです。
ちなみに `partialCloneFilter = blob:none` とは最新のcommitにおけるblobオブジェクト（ファイルを圧縮したGitオブジェクト）を除き、過去のblobオブジェクトをcloneしない。という設定になります。これを `tree:0` にすると、blobオブジェクトに加えて過去のtreeオブジェクトもダウンロードしない設定になります。  
開発時には `blob:none` を設定し、ビルド環境などで最新のcommitのみを使用するだけならば `tree:0` を設定するのが良さそうです。

# Sparse-checkoutについて

特定のディレクトリをダウンロードするには `git sparse-checkout` を実行します。

```
git sparse-checkout set src
remote: Enumerating objects: 3917, done.
remote: Counting objects: 100% (1906/1906), done.
remote: Compressing objects: 100% (1835/1835), done.
remote: Total 3917 (delta 238), reused 71 (delta 71), pack-reused 2011
Receiving objects: 100% (3917/3917), 10.15 MiB | 2.65 MiB/s, done.
Resolving deltas: 100% (440/440), done.
warning: fetch normally indicates which branches had a forced update,
but that check has been disabled; to re-enable, use '--show-forced-updates'
flag or run 'git config fetch.showForcedUpdates true'
Updating files: 100% (3963/3963), done.
```

このように余計なファイルはダウンロードせず、リポジトリのサイズを少ないままにできます。


# まとめ

ZOZOTOWNがサービスを開始してから18年が経ち、弊社にも巨大なリポジトリが複数存在します。Scalarによって普段の開発スピードやCI/CDの効率化が期待できそうです。
まだまだ実験的な機能も含まれているそうなので、今後も注目していきたいと思います。

25日の大トリは、 [@gold-kou](https://qiita.com/gold-kou) さんの記事です。
