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

- masterブランチにいて、コミットが存在しない

```sh
$ git status
On branch master

Initial commit

nothing to commit (create/copy files and use "git add" to track)
```

- master ブランチにて、ファイルは存在するとが追跡されていない
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

### git add - ステージ領域へファイルを追加

- ファイルを Gitリポジトリの管理対象とするために`git add`コマンドを利用してステージ領域と呼ばれる場所にファイルを登録

```sh
$ git add README.md
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   README.md
```

### git commit リポジトリの歴史を記録

- `git commit`コマンドはステージ領域に登録されている時点のファイル群を実際にリポジトリの歴史として記録する

```sh
$ git commit -m "First Commit"
[master (root-commit) 9b7066c] First Commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md
```

### フロー

![スクリーンショット 2020-05-09 9 05 30](https://user-images.githubusercontent.com/20186020/81458244-64e45000-91d4-11ea-87cd-dd01599efbd2.png)


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
  ファイルやディレクトリの状態を記録する場所で、保存された状態は、内容の変更履歴として格納される
- リポジトリデータ
  - `.git` ディレクトリに存在するワークツリー以下を管理するファイルたち
- ワークツリー(ワーキングツリー)
  - `git init`コマンドを実行したディレクトリ以下のこと 
- コミット
  - ワークツリーにある全てのファイルのその時点の状態を記録すること
- ステージ領域
  - コミットをする前の一時領域
  
## 参考 URL

- https://git-scm.com/
