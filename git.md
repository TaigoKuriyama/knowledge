# Git

## 基本

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

### git commit - リポジトリの歴史を記録

- `git commit`コマンドはステージ領域に登録されている時点のファイル群を実際にリポジトリの歴史として記録する

```sh
$ git commit -m "First Commit"
[master (root-commit) 9b7066c] First Commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md
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

### フロー

![スクリーンショット 2020-05-09 9 05 30](https://user-images.githubusercontent.com/20186020/81458244-64e45000-91d4-11ea-87cd-dd01599efbd2.png)

- [2.2 Git の基本 - 変更内容のリポジトリへの記録](https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E5%9F%BA%E6%9C%AC-%E5%A4%89%E6%9B%B4%E5%86%85%E5%AE%B9%E3%81%AE%E3%83%AA%E3%83%9D%E3%82%B8%E3%83%88%E3%83%AA%E3%81%B8%E3%81%AE%E8%A8%98%E9%8C%B2)


## 応用

### コミットメッセージの変更

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
```

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
  
## 参考 URL

- https://git-scm.com/
