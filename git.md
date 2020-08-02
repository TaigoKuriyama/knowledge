# Git

## 基本

- ファイルの状態
	- tracked
		- unmodified
		- modified
		- staged
	- untracked

### git init - リポジトリ初期化

```sh
$ mkdir git
$ cd git/
$ git init
Initialized empty Git repository in /Users/taigokuriyama/projects/git/.git/
$ ls .git/
HEAD		description	info		refs
config		hooks		objects
```

### git config

- git config
	- /etc/gitconfig ファイル: システム上の全てのユーザーと全てのリポジトリに対する設定値を保持
	- ~/.gitconfig か ~/.config/git/config ファイル: 特定のユーザーに対する設定値を保持
		- --global オプションを指定することで、Gitに、明確にこのファイルに読み書きを行なわせることができる
	- 現在使っているリポジトリのGitディレクトリにある`config`ファイル(`.git/config`のことです): 特定の 単一リポジトリに対する設定

```sh
$ git config --global user.name "xxxxxxx"
$ git config --global user.email xxxxxxx@xxxxx.xxx
$ git config --global core.editor vim
```

### git status - リポジトリ の状態を確認

- master ブランチにいて、コミット対象が存在しない

```sh
$ git status
On branch master

Initial commit

nothing to commit (create/copy files and use "git add" to track)
```

- master ブランチにて、ファイルは存在するが追跡されていない
   - ワーキングツリーでファイルを作成しただけでは、Gitリポジトリのバージョン管理の対象としてファイルは登録されていない

```sh
$ ls
$ touch README.md
$ git status
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	README.md

nothing added to commit but untracked files present (use "git add" to track)
```

### git add

- いくつかの機能がある
   - 新しいファイルの追跡開始
   - ステージ領域へファイルを追加
      - コマンドを実行した時点の状態のファイルをステージする
   - マージ時に衝突が発生したファイルに対する「解決済み」マーク付け

- ファイルを Gitリポジトリの管理対象とするために`git add`コマンドを利用してステージ領域と呼ばれる場所にファイルを登録
   - `Changes to be committed:`以下がステージ領域にされているファイル

```sh
$ git add README.md
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   README.md
```

### git diff - 変更したけどまだステージしていない変更を確認

```sh
$ git diff
```

- ステージされている変更と直近のコミットの内容を比較する場合

```sh
$ git diff --staged
```

- tool で確認する

```sh
$ git difftool
```

### git commit - リポジトリの歴史を記録

- `git commit`コマンドはステージ領域に登録されている時点のファイル群を実際にリポジトリの歴史として記録する

```sh
$ git commit -m "First Commit"
[master (root-commit) 9b7066c] First Commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md
```

### git rm - ファイルの削除をステージする

- ファイルの削除をステージする

```sh
$ git rm PROJECTS.md
rm 'PROJECTS.md'
$ git status
On branch master
Changes to be committed:
(use "git reset HEAD <file>..." to unstage) deleted: PROJECTS.md
```

普通に rm すると、not updated” (つまり ステージされていない) 欄に表示される

```sh
 $ rm PROJECTS.md
$ git status
On branch master
Your branch is up-to-date with 'origin/master'. Changes not staged for commit:
(use "git add/rm <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)
deleted: PROJECTS.md
no changes added to commit (use "git add" and/or "git commit -a")
```

- ファイル自体は作業ツリーに残しつ つステージングエリアからの削除だけを行う

```sh
$ git rm --cached README
```

### git mv

- ファイル名を変更してステージする

```sh
$ git mv README.md README
$ git status
On branch master
Changes to be committed:
(use "git reset HEAD <file>..." to unstage) renamed: README.md -> README
```

### git log - コミットログを確認

- リポジトリにコミットされたログを確認

```sh
$ git log
commit 6e8a1e7b11668f66c60b45c5ce955ccb165cf761 (HEAD -> master)
Author: xxxxxxxxx <xxxxxxxxx@xxxxxx.xxx>
Date:   Mon May 11 08:06:14 2020 +0900
    test
```

- `--graph`でブランチの分岐を視覚的に表現する

```sh
$ git log --graph
*   commit b1ef120598526f44d60c618edfcb29532905aaed (HEAD -> master)
|\  Merge: 6e8a1e7 30ae9ad
| | Author: xxxxxxxxx <xxxxxxxxx@xxxxxxxxx.xxx>
| | Date:   Tue May 12 07:53:47 2020 +0900
| |
| |     Merge branch 'test'
| |
| * commit 30ae9adf05acda9f9be221eb61104daa89ea8708 (test)
|/  Author: xxxxxxxxx <xxxxxxxxx@xxxxxxxxx.xxx>
|   Date:   Tue May 12 07:44:35 2020 +0900
|
|       add test to test
|
* commit 6e8a1e7b11668f66c60b45c5ce955ccb165cf761
| Author: xxxxxxxxx <xxxxxxxxx@xxxxxxxxx.xxx>
| Date:   Mon May 11 08:06:14 2020 +0900
|
|     test
|
```

- ` --stat`：各コミットエントリに続けて変更されたファイルの一覧と変更されたフ ァイルの数、追加・削除された行数が表示

```sh
 $ git log --stat
commit ca82a6dff817ec66f44342007202690a93763949 Author: Scott Chacon <schacon@gee-mail.com> Date: Mon Mar 17 21:52:11 2008 -0700
      changed the version number
Rakefile | 2 +-
1 file changed, 1 insertion(+), 1 deletion(-)
commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 Author: Scott Chacon <schacon@gee-mail.com> Date: Sat Mar 15 16:40:33 2008 -0700
      removed unnecessary test
lib/simplegit.rb | 5 -----
1 file changed, 5 deletions(-)
commit a11bef06a3f659402fe7563abf99ad00de2209e6 Author: Scott Chacon <schacon@gee-mail.com> Date: Sat Mar 15 10:31:28 2008 -0700
first commit
README | 6 ++++++
Rakefile | 23 +++++++++++++++++++++++ lib/simplegit.rb | 25 +++++++++++++++++++++++++ 3 files changed, 54 insertions(+)
```

- `--pretty`：独自のログ出力フォーマットを指定

```sh
 $ git log --pretty=format:"%h - %an, %ar : %s"
ca82a6d - Scott Chacon, 6 years ago : changed the version number 085bb3b - Scott Chacon, 6 years ago : removed unnecessary test a11bef0 - Scott Chacon, 6 years ago : first commit
```

- `--since`：時間制限のオプション

```sh
$ git log --since=2.weeks
```

- `-S`：任意の文字列を引数にでき、その文字列が追加・削除されたコミットのみを抜き出す

```sh
$ git log -Sfunction_name
```

### git remote - リモート Git リポジトリにアクセスしやすいような名前をつけて追加

- 新しいリモート Git リポジトリにアクセスしやすいような名前をつけて追加する
  - どういう時に使うのかはわからん

```sh
$ git remote
origin
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote -v
origin https://github.com/schacon/ticgit (fetch) origin https://github.com/schacon/ticgit (push)
pb https://github.com/paulboone/ticgit (fetch)
pb https://github.com/paulboone/ticgit (push)
```

### git tag - 歴史上の重要なポイントに印をつける

- 既存のタグの一覧を表示

```sh
$ git tag
v0.1
v1.3
```

- タグの作成
  - タグには軽量 (lightweight) 版と注釈付き (annotated) 版の二通りがある

- 現在のHEADが指しているコミットに注釈付きタグを付与する

```sh
$ git tag -a v1.4 -m "my version 1.4"
$ git show v1.4
tag v1.4
Tagger: Ben Straub <ben@straub.cc> Date: Sat May 3 20:19:12 2014 -0700
my version 1.4
commit ca82a6dff817ec66f44342007202690a93763949 Author: Scott Chacon <schacon@gee-mail.com> Date: Mon Mar 17 21:52:11 2008 -0700
      changed the version number
```

- 現在のHEADが指しているコミットに軽量タグを付与する

```sh
$ git tag v1.4-lw $ git tag
v0.1
v1.3
v1.4 v1.4-lw v1.5
$ git show v1.4-lw
commit ca82a6dff817ec66f44342007202690a93763949 Author: Scott Chacon <schacon@gee-mail.com> Date: Mon Mar 17 21:52:11 2008 -0700
      changed the version number

```


### git checkout -b - ブランチを作成し切り替える

```sh
$ git branch
* master
$ git checkout -b test
Switched to a new branch 'test'
$ git branch
  master
* test
```

### git branch -D - ブランチを削除

```sh
$ git branch -D test
```

### git merge - ブランチをマージ

- `--no-ff`でマージコミットメッセージを記入するためのエディタが立ち上がる

```sh
$ git checkout master
Switched to branch 'master'* master
$ git merge --no-ff test
Merge made by the 'recursive' strategy.
 test | 4 ++++
 1 file changed, 4 insertions(+)
```

### git reset --hard コミットハッシュ - コミットを元に戻す

```sh
$ git log
commit 90db068cbac100ba6249e9e6f49045128536fe23 (HEAD -> master)
Author: xxxxxxxxx <xxxxxxxxx@xxxxxxxxx.xxx>
Date:   Tue May 12 08:03:55 2020 +0900

    add test

commit 9b7066ce13139b01c4b0062da33eb0ae85c0bc64
Author: xxxxxxxxx <xxxxxxxxx@xxxxxxxxx.xxx>
Date:   Sat May 9 08:57:05 2020 +0900

    First Commit
$ git reset --hard 9b7066ce13139b01c4b0062da33eb0ae85c0bc64
HEAD is now at 9b7066c First Commit
$ git log
commit 9b7066ce13139b01c4b0062da33eb0ae85c0bc64 (HEAD -> master)
Author: xxxxxxxxx <xxxxxxxxx@xxxxxxxxx.xxx>
Date:   Sat May 9 08:57:05 2020 +0900

    First Commit
```

### git refrog - リポジトリで行われた作業のログを確認

- `git log` だと今の状態から過去のログしか見れない

```sh
 git reflog
9b7066c (HEAD -> master) HEAD@{0}: reset: moving to 9b7066ce13139b01c4b0062da33eb0ae85c0bc64
90db068 HEAD@{1}: commit: add test
9b7066c (HEAD -> master) HEAD@{2}: reset: moving to 9b7066ce13139b01c4b0062da33eb0ae85c0bc64
b1ef120 HEAD@{3}: checkout: moving from test to master
30ae9ad (test) HEAD@{4}: checkout: moving from master to test
b1ef120 HEAD@{5}: checkout: moving from master to master
b1ef120 HEAD@{6}: merge test: Merge made by the 'recursive' strategy.
6e8a1e7 HEAD@{7}: checkout: moving from test to master
30ae9ad (test) HEAD@{8}: commit: add test to test
6e8a1e7 HEAD@{9}: checkout: moving from master to test
6e8a1e7 HEAD@{10}: commit: test
3fad247 HEAD@{11}: commit: test
4b420e0 HEAD@{12}: commit: test
9b7066c (HEAD -> master) HEAD@{13}: commit (initial): First Commit
```

### git rebase -i - 過去のコミットを改変する

- 最新のコミットを１つ前のコミットに含める
   - １つ前のコミットに間違えが見つかった時に、修正したコミットを使って改竄するイメージ

```sh
$ git rebase -i HEAD-2
```

### git remote add - リモートリポジトリを登録

- origin という識別子で`https://github.com/TaigoKuriyama/git-test.git`を指すようになる

```sh
$ git remote add origin https://github.com/TaigoKuriyama/git-test.git
$ cat .git/config
[remote "origin"]
	url = https://github.com/TaigoKuriyama/git-test.git
	fetch = +refs/heads/*:refs/remotes/origin/*
```

### git push - リモートリポジトリへ送信

- 初めてブランチを Push するときに`-u`オプションをつけることを推奨
- origin の master ブランチに現在のブランチの内容を送信
  - git push [remote-name] [branch-name]

```sh
$ git push -u origin master
```

### git clone - リモートリポジトリを取得

- git clone 直後は master ブランチにいる

```sh
$ git clone https://github.com/TaigoKuriyama/git-test.git
Cloning into 'git-test'...
remote: Enumerating objects: 21, done.
remote: Counting objects: 100% (21/21), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 21 (delta 5), reused 19 (delta 3), pack-reused 0
Unpacking objects: 100% (21/21), done.
$ cd git-test/
$ git branch
* master
```

### git branch - 現在のブランチを表示

- `-a` オプションでリモートリポジトリのブランチも表示

```sh
$ git branch
* master
$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/test
```

### リモートリポジトリのブランチをチェックアウト

```sh
$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/test
$ git checkout -b test origin/test
Branch rebase set up to track remote branch rebase from origin.
Switched to a new branch 'test'
(base) TK2:git-test taigokuriyama$ git branch
  master
* test
```

### git fetch - リモートリポジトリの最新の履歴の取得だけを行う

- リモートリポジトリの最新の履歴の取得だけを行う
  - remote-name とは `git remote -v` で表示される名前でデフォルトは `origin`

```sh
$ git fetch [remote-name]
```



### git pull - 

- リモートリポジトリの内容を取得し、マージする
  - git fetch + merge

```sh

```

- [Git 再入門: 引数がない git pull のデフォルトの挙動 (アップストリーム, トラッキングブランチについて)](https://www.yunabe.jp/docs/relearning_git_pull_default.html)



### pull request の流れ

- fork する
- fork したリポジトリを clone する
- clone したリポジトリでトピックブランチを作成
- コードを修正
- git add, commit
- 元のリポジトリに pull request を送る

### フロー

![スクリーンショット 2020-05-09 9 05 30](https://user-images.githubusercontent.com/20186020/81458244-64e45000-91d4-11ea-87cd-dd01599efbd2.png)

- [2.2 Git の基本 - 変更内容のリポジトリへの記録](https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E5%9F%BA%E6%9C%AC-%E5%A4%89%E6%9B%B4%E5%86%85%E5%AE%B9%E3%81%AE%E3%83%AA%E3%83%9D%E3%82%B8%E3%83%88%E3%83%AA%E3%81%B8%E3%81%AE%E8%A8%98%E9%8C%B2)

### git flow

- master ブランチは常にデプロイできる状態とする
- 新しい作業をするときは、masterブランチから記述的な名前のブランチを作成する
  - 記述的とはブランチの特性を明確に著した
- 同名のブランチを GitHub のリポジトリに定期的に push する
- Pull Request を送り、レビューをしてもらう
- master ブランチにマージし、直ちにデプロイする

### コミットのやり直し

- ステージングエリアの内容をコミットに使用する
  - 直近のコミット以降に何も変更をし ていない場合 (たとえば、コミットの直後にこのコマンドを実行したような場合)、 スナップショットの内容 はまったく同じでありコミットメッセージを変更することになる
- 最終的にできあがるのはひとつのコミット。二番目のコミットが、最初のコミットの結果を上書きする

```sh
$ git commit --amend
```

### コンフリクト時

```sh
$ git pull
error: Pulling is not possible because you have unmerged files.
hint: Fix them up in the work tree, and then use 'git add/rm <file>'
hint: as appropriate to mark resolution and make a commit.
fatal: Exiting because of an unresolved conflict.
$ vim conflict_file
$ git add conflict_file
$ git commit -m "resolve conflict"
```

### .gitignore

- [Python.gitignore](https://github.com/github/gitignore/blob/master/Python.gitignore)

## 用語

- リポジトリ
  - ファイルやディレクトリの状態を記録する場所で、保存された状態は、内容の変更履歴として格納される
- リポジトリデータ
  - `.git` ディレクトリに存在するワークツリー以下を管理するファイルたち
- ワークツリー(ワーキングツリー)
  - `git init`コマンドを実行したディレクトリ以下のこと 
- コミット
  - ワークツリーにある全てのファイルのその時点の状態を記録すること
- ステージ(インデックス)エリア
  - コミットをする前の一時領域
- トピックブランチ
  - １つのトピックに集中して他の作業は一切行わないブランチ
- 上流ブランチ
  - あるローカルブランチが、履歴を追跡するように設定したリモートブランチの事
  
## 参考 URL

- https://git-scm.com/
