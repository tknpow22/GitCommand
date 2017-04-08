Git コマンドサンプル
====================

### 新しいリポジトリを作成する。

    git init

```
$ git init
Initialized empty Git repository in C:/Usr/Work/Temporary/work1/.git/
```

### 新しいベアリポジトリを作成する。

    git init --bare

```
$ git init --bare
Initialized empty Git repository in C:/Usr/Work/Temporary/work1/
```

### リモートリポジトリをローカルにクローンする。

    git clone <remote_url>

例) https://github.com/tknpow22/sample.git を /repbase/sample にクローンする

    $ cd /repobase
    $ git clone https://github.com/tknpow22/sample.git
    $ ls
    sample

[git clone]

### パスワードを平文で保存する。
このコマンドを実行後、認証が必要なコマンド(git remote show origin など)を発行した際に
入力したパスワードが $HOME/.git-credentials に保存される。

    git config credential.helper store

[git config]

### ファイルの追加・変更をステージングする。
ワイルドカードも使用できる。また、*追加*といいながら SVN とは違い、
ファイルを改変したことを追加するものであり、ファイルの追加、更新毎に行う必要がある。

    git add <file>
    git add <directory>

例) test.txt を追加・変更しステージングする。

    #### test.txt の追加をステージングする
    $ echo 'line1' > test.txt
    $ git add test.txt
    
    #### test.txt の変更をステージングする
    $ echo 'line2' >> test.txt
    $ git add test.txt

[git add]

### すべてのファイルの追加・変更をステージングする。

    git add --all

[git add]

### ステージングエリア、ワークツリー、追跡対象外のファイルの状態を一覧表示する。

    git status

例) test.txt の追加・変更と test2.txt の追加の状態を表示する。

    $ echo 'line1' > test.txt
    $ git add test.txt
    $ echo 'line2' >> test.txt
    $ echo 'line0' > test2.txt
    $ git status
    On branch master
    
    Initial commit
    
    Changes to be committed:
      (use "git rm --cached <file>..." to unstage)
    
            new file:   test.txt
    
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    
            modified:   test.txt
    
    Untracked files:
      (use "git add <file>..." to include in what will be committed)
    
            test2.txt

[git status]

### 現在のワークツリーの状態を一時領域(stash)に保存する。
stash はスタックとして働き、保存した内容は stash の先頭(stash@{0})にプッシュされる。
なお、コミットがまだ一度もされていない場合、保存できないので注意すること。

    git stash
    git stash save

例) ワークツリーの状態を保存する。

    $ git stash
    Saved working directory and index state WIP on master: 6d79282 initail commit
    HEAD is now at 6d79282 initail commit

[git stash]

### 現在のワークツリーの状態を Untracked なファイルも含め、一時領域(stash)に保存する。

    git stash -u

    $ git stash -u
    Saved working directory and index state WIP on branch1: 8ef382f init
    HEAD is now at 8ef382f init

[git stash]

### 一時領域(stash)の一覧を見る。
最後に保存(プッシュ)した内容は一覧の先頭(stash@{0})に保存されている。

    git stash list

例) stash の一覧を見る。

    $ git stash list
    stash@{0}: WIP on master: 6d79282 initail commit

[git stash]

### 一時領域(stash)の先頭の内容をワークツリーに反映する。
ワークツリーに反映後も stash の内容は破棄されないので、必要であれば git stash drop 等で stash から削除すること。

    git stash apply

例) stash の先頭の内容をワークツリーに反映する。

    $ git stash apply
    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    
            modified:   test.txt
    
    Untracked files:
      (use "git add <file>..." to include in what will be committed)
    
            test2.txt
    
    no changes added to commit (use "git add" and/or "git commit -a")

[git stash]

### 一時領域(stash)の先頭の内容を削除する。

    git stash drop

例) stash の先頭の内容を削除する。

    $ git stash drop
    Dropped refs/stash@{0} (9042f98e5126009e4133962035f049028236a055)

[git stash]

### ステージングされた内容とワークツリーの内容を比較する。

    git diff

例) test.txt を作成・ステージングした後、変更し、比較してみる。

    $ echo 'line1' > test.txt
    $ git add test.txt
    $ echo 'line2' >> test.txt
    $ git diff
    diff --git a/test.txt b/test.txt
    index a29bdeb..c0d0fb4 100644
    --- a/test.txt
    +++ b/test.txt
    @@ -1 +1,2 @@
     line1
    +line2

[git diff]

### コミットされた内容とステージングエリアの内容を比較する。

    git diff --cached
    git diff --staged

例) test.txt を作成・コミット後、変更・ステージングし、比較してみる。

    $ echo 'line1' > test.txt
    $ git add test.txt
    $ git commit -m 'add test.txt'
    $ echo 'line2' >> test.txt
    $ git add test.txt
    $ git diff --staged
    diff --git a/test.txt b/test.txt
    index a29bdeb..c0d0fb4 100644
    --- a/test.txt
    +++ b/test.txt
    @@ -1 +1,2 @@
     line1
    +line2

[git diff]

### 編集中のファイルの内容を破棄し、直前にステージングした内容に戻す。
ステージングした内容がない場合、直前にコミットした内容に戻る。

    git checkout <file>
    git checkout -- <file>

例) test.txt を編集・ステージングし、さらに編集した後、編集内容を破棄して、直前にステージングした内容に戻す。

    $ echo 'line1' > test.txt
    $ cat test.txt
    line1
    $ git add test.txt
    $ echo 'line2' >> test.txt
    $ cat test.txt
    line1
    line2
    
    $ git checkout -- test.txt
    
    $ cat test.txt
    line1

[git checkout]

### 編集中のファイルの内容を破棄し、直前にコミットした内容に戻す。
当該ファイルにステージングした内容がある場合、それも破棄される。

    git checkout <commit> <file>
    git checkout <commit> -- <file>

例) test.txt を編集・ステージングし、さらに編集した後、編集内容を破棄して、直前にコミットした内容に戻す。

    $ echo 'line1' > test.txt
    $ cat test.txt
    line1
    $ git add test.txt
    $ git commit -m 'add test.txt'
    $ echo 'line2' >> test.txt
    $ cat test.txt
    line1
    line2
    $ git add test.txt
    $ echo 'line3' >> test.txt
    $ cat test.txt
    line1
    line2
    line3
    
    $ git checkout HEAD -- test.txt
    
    $ cat test.txt
    line1

[git checkout]

### 編集中の内容をすべて破棄し、直前にコミットした内容に戻す。
ワークツリーでの編集内容はすべて破棄されるので注意すること。

    git reset --hard

例) testA.txt、testB.txt を編集・コミットし、さらに編集した後、すべてのファイルの編集内容を破棄して、直前コミットした内容に戻す。

    $ echo 'lineA1' > testA.txt
    $ echo 'lineB1' > testB.txt
    $ cat testA.txt
    lineA1
    $ cat testB.txt
    lineB1
    $ git add *.txt
    $ git commit -m 'add testA.txt, testB.txt'
    $ echo 'lineA2' >> testA.txt
    $ echo 'lineB2' >> testB.txt
    $ cat testA.txt
    lineA1
    lineA2
    $ cat testB.txt
    lineB1
    lineB2
    
    $ git reset --hard
    HEAD is now at 7402f89 add testA.txt, testB.txt
    
    $ cat testA.txt
    lineA1
    $ cat testB.txt
    lineB1

[git reset]

### 直前のコミットの内容を取り消す

以下のような状態の時に、ワークツリーの状態はそのままで、直前のコミット bbb を取り消すには

     git reset --soft HEAD^

```
$ git log
commit ab765ad89a8d383f8f7b7108d59faf6f9e42684e
Author: tknpow22 <tknpow22@koutou-software.net>
Date:   Sat Apr 8 12:15:39 2017 +0900

    bbb

commit 9bafa973e89f2e1c5a623e410f1d8fba8013a7be
Author: tknpow22 <tknpow22@koutou-software.net>
Date:   Sat Apr 8 12:05:04 2017 +0900

    aaa

Bono@ZEO MINGW64 /C/Usr/Work/Temporary/hoge (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   hoge.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

```
Bono@ZEO MINGW64 /C/Usr/Work/Temporary/hoge (master)
$ git reset --soft HEAD^

Bono@ZEO MINGW64 /C/Usr/Work/Temporary/hoge (master)
$ git log
commit 9bafa973e89f2e1c5a623e410f1d8fba8013a7be
Author: tknpow22 <tknpow22@koutou-software.net>
Date:   Sat Apr 8 12:05:04 2017 +0900

    aaa

Bono@ZEO MINGW64 /C/Usr/Work/Temporary/hoge (master)
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   hoge.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   hoge.txt
```


[git reset]

### 直前のコミットを打ち消しすための新たなコミットを作成する。
ひとつのコミットのみを元に戻す*打ち消し*であり、*取り消し*でないことに注意すること。
そのため、打ち消したコミットも履歴に残る。

    git revert <commit>

例) 一つ前のコミットの状態に戻し、それを履歴にも残す。

    $ echo 'line1' > test.txt
    $ git add test.txt
    $ git commit -m 'add test.txt'
    $ echo 'line2' >> test.txt
    $ git commit -a -m 'add line2'
    $ echo 'line3' >> test.txt
    $ git commit -a -m 'add line3'
    $ git log --oneline --decorate
    d68cda0 (HEAD, master) add line3
    1c7205c add line2
    82f7743 add test.txt
    $ cat test.txt
    line1
    line2
    line3
    
    $ git revert HEAD
    #### エディタが起動するので、必要であればメッセージを入力すること。
    
    $  git log --oneline --decorate
    4372be1 (HEAD, master) Revert "add line3"
    d68cda0 add line3
    1c7205c add line2
    82f7743 add test.txt
    $ cat test.txt
    line1
    line2

[git revert]

### HEAD が指しているコミットに軽量タグを作成する。

    git tag <tag>

例) タグ v1.0 を作成する。

    $ git log --oneline --decorate
    86331ad (HEAD, master) add line3
    c4b38db add line2
    16ee40c test.txt
    
    $ git tag v1.0
    
    $ git log --oneline --decorate
    86331ad (HEAD, tag: v1.0, master) add line3
    c4b38db add line2
    16ee40c test.txt

[git tag]

### HEAD が指しているコミットに注釈付きタグを作成する。

    git tag -a -m "message." <tag>

例) 注釈付きタグ v1.0 を作成する。

    $ git tag -a -m 'v1.0-message' v1.0
    $ git show v1.0
    tag v1.0
    Tagger: tknpow22 <tknpow22@koutou-software.co.jp>
    Date:   Sun Mar 30 13:20:41 2014 +0900
    
    v1.0-message
    
    commit 810b9ec905bc142687621e60726a05ae945af480
    Author: tknpow22 <tknpow22@koutou-software.co.jp>
    Date:   Sun Mar 30 13:19:44 2014 +0900
    
        add line3
    
    diff --git a/test.txt b/test.txt
    index c0d0fb4..83db48f 100644
    --- a/test.txt
    +++ b/test.txt
    @@ -1,2 +1,3 @@
     line1
     line2
    +line3

[git tag]

### ファイルを移動/リネームする。

    git mv <old_file> <new_file>

例) test.txt を test2.txt にリネームする

    $ git mv test.txt test2.txt
    
    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)
    
            renamed:    test.txt -> test2.txt

[git mv]

### 誤ってステージングしてしまったファイルをステージングエリアから削除する。
ワークツリーのファイルは変更されない。

    git rm --cached <file>
    git rm --cached -- <file>

例) 誤って追加してしまった test.bak をステージングエリアから削除する。

    $ echo 'line0' >> test.bak
    $ git add test.bak
    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)
    
            new file:   test.bak
    
    $ git rm --cached test.bak
    
    $ git status
    On branch master
    Untracked files:
      (use "git add <file>..." to include in what will be committed)
    
            test.bak
    
    nothing added to commit but untracked files present (use "git add" to track)
    
    $ cat test.bak
    line0

[git rm]

### ワークツリーから追跡対象外のファイルを削除する。

    git clean -f

例) 追跡対象外のファイルを削除する。

    $ git status
    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    
            modified:   test.txt
    
    Untracked files:
      (use "git add <file>..." to include in what will be committed)
    
            test.bak
    
    no changes added to commit (use "git add" and/or "git commit -a")
    
    $ git clean -f
    Removing test.bak
    
    $ git status
    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    
            modified:   test.txt
    
    no changes added to commit (use "git add" and/or "git commit -a")

[git clean]

### ワークツリーから追跡対象外のファイルおよびフォルダを削除する。

    git clean -df

例) 追跡対象外のファイルおよびフォルダを削除する。

    $ git status
    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    
            modified:   test.txt
    
    Untracked files:
      (use "git add <file>..." to include in what will be committed)
    
            test.bak
            tmpdir/
    
    no changes added to commit (use "git add" and/or "git commit -a")
    
    $ git clean -df
    Removing test.bak
    Removing tmpdir/
    
    $ git status
    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    
            modified:   test.txt
    
    no changes added to commit (use "git add" and/or "git commit -a")

[git clean]

### ステージングエリアにある内容をコミットする。

    git commit -m "message."

例) ステージング済みの test.txt への変更をコミットする。

    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)
    
            modified:   test.txt
    
    $ git commit -m 'add line4'
    [master 5142000] add line4
     1 file changed, 1 insertion(+)
    
    $ git status
    On branch master
    nothing to commit, working directory clean

[git commit]

### 追跡対象になっているファイルをステージングエリアに追加してから、コミットする。
git add を省略するときに使う。

    git commit -a -m "message."

例) test.txt への変更をステージングしてから、コミットする。

    $ git status
    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    
            modified:   test.txt
    
    no changes added to commit (use "git add" and/or "git commit -a")
    
    $ git commit -a -m 'add line5'
    [master 259d707] add line5
     1 file changed, 1 insertion(+)
    
    $ git status
    On branch master
    nothing to commit, working directory clean
    
[git commit]

### ステージングエリアにある内容で直前のコミットを上書きする。

リモートリポジトリに push した後は使用してはいけない。

    git commit --amend -m "message."

例) test.txt を変更し、直前のコミットを上書きしてコミットする。

    $ git log --oneline --decorate
    75016fc (HEAD, master) add line3
    e7504fc add line2
    9c124fb add test.txt
    
    $ echo 'line4' >> test.txt
    $ git add test.txt
    $ git commit --amend -m 'add line3 & line4'
    [master 7b06182] add line3 & line4
     1 file changed, 2 insertions(+)
    
    $ git log --oneline --decorate
    7b06182 (HEAD, master) add line3 & line4
    e7504fc add line2
    9c124fb add test.txt

[git commit]

### コミット履歴をハッシュ(短縮版)、件名、タグ等を一行で表示する。

    git log --oneline --decorate

例) コミット履歴を表示する。

    $ git log --oneline --decorate
    75016fc (HEAD, tag: v1.0, master) add line3
    e7504fc add line2
    9c124fb add test.txt

[git log]

### リモートブランチのコミット履歴を表示する。

    git log <remote_name>/<branch>

例) コミット履歴を表示する。

    $ git log origin/master --oneline --decorate
    9621bef (HEAD, origin/master, master) add line3
    e4b00fa add line2
    fa14b13 add test.txt

[git log]

### リモートリポジトリの指定のブランチからフェッチする。

    git fetch <remote_name> <branch>

例) リモートリポジトリ origin のブランチ master からフェッチする。

    $ git fetch origin master
    From https://github.com/tknpow22/sample
     * branch            master     -> FETCH_HEAD

[git fetch]

### リモートリポジトリの指定のブランチからフェッチし、マージする。

    git pull <remote_name> <branch>

例) リモートリポジトリ origin のブランチ master からフェッチし、マージする。

    $ git pull origin master
    remote: Counting objects: 5, done.
    remote: Total 3 (delta 0), reused 3 (delta 0)
    Unpacking objects: 100% (3/3), done.
    From https://github.com/tknpow22/sample
     * branch            master     -> FETCH_HEAD
       cabe6f8..86ae952  master     -> origin/master
    Updating cabe6f8..86ae952
    Fast-forward
     test.txt | 1 +
     1 file changed, 1 insertion(+)

[git pull]

### リモートリポジトリの指定のブランチにプッシュする。

    git push <remote_name> <branch>

例) リモートリポジトリ origin のブランチ master にプッシュする。

    $ git push origin master
    Counting objects: 5, done.
    Writing objects: 100% (3/3), 266 bytes | 0 bytes/s, done.
    Total 3 (delta 0), reused 0 (delta 0)
    To https://github.com/tknpow22/sample.git
       86ae952..8d1e696  master -> master

[git push]

### すべてのタグをリポートリポジトリの指定のブランチにプッシュする。

    git push <remote_name> <branch> --tags

例) 全てのタグをリモートリポジトリ origin のブランチ master にプッシュする。

    $ git push origin master --tags
    Total 0 (delta 0), reused 0 (delta 0)
    To https://github.com/tknpow22/sample.git
     * [new tag]         v1.0 -> v1.0

[git push]

### ローカルリポジトリのすべてのブランチを表示する。

    git branch

例) ローカルリポジトリのすべてのブランチを表示する。

    $ git branch
      iss1
      iss5
    * master

[git branch]

### すべてのブランチを表示する。

    git branch -a

例) すべてのブランチを表示する。

    $ git branch -a
      iss1
      iss5
    * master
      remotes/origin/iss5
      remotes/origin/master

[git branch]

### 新しいブランチをローカルリポジトリに作成する。

    git branch <new_branch>

例) 新しいブランチ iss7 を作成する。

    $ git branch iss7
    $ git branch
      iss1
      iss5
      iss7
    * master

[git branch]

### ローカルリポジトリのブランチを削除する。

    git branch -d <branch>

例) ブランチ iss7 を削除する。

    $ git branch -d iss7
    Deleted branch iss7 (was 8d1e696).
    $ git branch
      iss1
      iss5
    * master

[git branch]

### 新しいブランチをローカルリポジトリに作成し、作成したブランチに切り替える。

    git checkout -b <new_branch>

例) 新しいブランチ iss8 を作成し、切り替える。

    $ git checkout -b iss8
    Switched to a new branch 'iss8'
    
    $ git branch
      iss1
      iss5
    * iss8
      master

[git checkout]

### 指定のブランチを元に新しいブランチをローカルリポジトリに作成し、作成したブランチに切り替える。

    git checkout -b <new_branch> <existing_branch>

例) ブランチ iss5 を元にブランチ iss9 を作成し、切り替える。

    $ git checkout -b iss9 iss5
    Switched to a new branch 'iss9'
    
    $ git branch
      iss1
      iss5
      iss8
    * iss9
      master

[git checkout]

### ブランチを切り替える

    git checkout <branch>

例) ブランチ iss1 に切り替える。

    $ git checkout iss1
    Switched to branch 'iss1'
    
    $ git branch
    * iss1
      iss5
      iss8
      master

[git checkout]

### 指定したブランチを現在のブランチにマージする。

    git merge <branch>

例) ブランチ iss1 での変更をブランチ master にマージする。

    $ git checkout -b iss1
    Switched to a new branch 'iss1'
    $ echo 'line4' >> test.txt
    $ git commit -am 'add line4'
    [iss1 7999167] add line4
     1 file changed, 1 insertion(+)
    $ git checkout master
    Switched to branch 'master'
    
    $ git merge iss1
    Updating 91e7c4a..7999167
    Fast-forward
     test.txt | 1 +
     1 file changed, 1 insertion(+)
    
    $ git log --oneline --decorate
    7999167 (HEAD, master, iss1) add line4
    91e7c4a add line3
    7e2bc02 add line2
    5fd4039 add test.txt

[git-merge]

### 指定したブランチを現在のブランチにマージする(ファストフォワードしない)。

    git merge --no-ff <branch>

例) ブランチ iss2 での変更をブランチ master にファストフォワードしないでマージする。

    $ git checkout -b iss2
    Switched to a new branch 'iss2'
    $ echo 'line5' >> test.txt
    $ git commit -am 'add line5'
    [iss2 832479b] add line5
     1 file changed, 1 insertion(+)
    $ git checkout master
    Switched to branch 'master'
    
    $ git merge --no-ff iss2
    Merge made by the 'recursive' strategy.
     test.txt | 1 +
     1 file changed, 1 insertion(+)
    
    $ git log --oneline --decorate
    537f055 (HEAD, master) Merge branch 'iss2'
    832479b (iss2) add line5
    7999167 (iss1) add line4
    91e7c4a add line3
    7e2bc02 add line2
    5fd4039 add test.txt

[git-merge]

### 指定したリモートブランチを現在のブランチにマージする

    git merge <remote_name>/<branch>

例) リモートブランチ origin/master を現在のブランチ master にマージする

    $ git fetch origin master
    remote: Counting objects: 5, done.
    remote: Total 3 (delta 0), reused 3 (delta 0)
    Unpacking objects: 100% (3/3), done.
    From https://github.com/tknpow22/sample
     * branch            master     -> FETCH_HEAD
       8d1e696..517a528  master     -> origin/master
    
    $ git merge origin/master
    Updating 8d1e696..517a528
    Fast-forward
     test.txt | 1 +
     1 file changed, 1 insertion(+)

[git-merge]

### 指定のブランチを現在のブランチにリベースする。
リモートリポジトリに push した後は使用してはいけない。

    git rebase <branch>

例) ブランチ iss1 にブランチ master をリベースする。

    $ echo 'line1' > test.txt
    $ git add test.txt
    $ git commit -m 'add test.txt'
    #### ここでブランチ iss1 を作成しておく
    $ git branch iss1
    #### さらにブランチ master で変更する
    $ echo 'line2' >> test.txt
    $ git commit -am 'add line2'
    #### ブランチ iss1 で変更する
    $ git checkout iss1
    Switched to branch 'iss1'
    $ cat test.txt
    line1
    $ echo 'lineA' >> test.txt
    $ git commit -am 'add lineA'

    #### ブランチ iss1 にブランチ master をリベースする
    $ git rebase master
    First, rewinding head to replay your work on top of it...
    Applying: add lineA
    Using index info to reconstruct a base tree...
    M       test.txt
    Falling back to patching base and 3-way merge...
    Auto-merging test.txt
    CONFLICT (content): Merge conflict in test.txt
    Failed to merge in the changes.
    Patch failed at 0001 add lineA
    The copy of the patch that failed is found in:
       c:/usr/Git_LocalRepositories/sample/.git/rebase-apply/patch
    
    When you have resolved this problem, run "git rebase --continue".
    If you prefer to skip this patch, run "git rebase --skip" instead.
    To check out the original branch and stop rebasing, run "git rebase --abort".
    
    #### ここでエディタで競合を解決すること

    #### 解決したらステージングし、リベースを続ける
    $ git add test.txt
    $ git rebase --continue
    Applying: add lineA

[git-rebase]

### ローカルリポジトリの reflog を表示する。

    git reflog

例) reflog を表示する。

    $ git reflog
    b9a76fe HEAD@{0}: checkout: moving from iss1 to master
    b715d09 HEAD@{1}: rebase finished: returning to refs/heads/iss1
    b715d09 HEAD@{2}: rebase: add lineA
    b9a76fe HEAD@{3}: rebase: checkout master
    43203c3 HEAD@{4}: commit: add lineA
    f5f7aad HEAD@{5}: checkout: moving from master to iss1
    b9a76fe HEAD@{6}: commit: add line2
    f5f7aad HEAD@{7}: commit (initial): add test.txt
    
    $ git checkout iss1
    Switched to branch 'iss1'
    
    $ git reflog
    b715d09 HEAD@{0}: checkout: moving from master to iss1
    b9a76fe HEAD@{1}: checkout: moving from iss1 to master
    b715d09 HEAD@{2}: rebase finished: returning to refs/heads/iss1
    b715d09 HEAD@{3}: rebase: add lineA
    b9a76fe HEAD@{4}: rebase: checkout master
    43203c3 HEAD@{5}: commit: add lineA
    f5f7aad HEAD@{6}: checkout: moving from master to iss1
    b9a76fe HEAD@{7}: commit: add line2
    f5f7aad HEAD@{8}: commit (initial): add test.txt

[git reflog]

### リモート接続の一覧を URL を含めて表示する。

    git remote -v

例) リモート接続の一覧を表示する。

    $ git remote -v
    origin  https://github.com/tknpow22/sample.git (fetch)
    origin  https://github.com/tknpow22/sample.git (push)

[git remote]

### リモートリポジトリに対する新規接続を作成する。

    git remote add <name> <remote_url>

例) 新たなローカルリポジトリを作成し、リモートリポジトリを追加する。

    $ cd /repobase
    $ mkdir sample
    $ cd sample
    $ git init
    $ git remote add origin https://github.com/tknpow22/sample.git

[git remote]

### master から新しいブランチ bugfix を作成し、ブランチ bugfix で作業後 master をマージ(リベース)した後、master で bugfix をマージ後、ブランチ bugfix を削除する

    #### ファイル test.txt をコミットする
    $ echo 'line1' > test.txt
    $ git add test.txt
    $ git commit -m 'add test.txt'
    
    #### ブランチ bugfix を作成し、切り替える
    $ git checkout -b bugfix
    #### ブランチ bugfix で作業しコミットする
    $ echo 'bugfix' >> test.txt
    $ git add test.txt
    $ git commit -m 'bugfix'
    
    #### bugfix で作業している間に master ブランチでも test.txt に変更が加えられコミットされたものとする
    
    #### ブランチ bugfix に master をマージ(リベース)する
    $ git rebase master
    
    #### 競合が発生するのでエディタ等で修正する
    $ vi test.txt
    
    #### 修正をコミットし、リベースを続ける
    $ git add test.txt
    $ git rebase --continue
    
    #### master に切り替える
    $ git checkout master
    #### master にブランチ bugfix をマージする
    $ git merge bugfix
    #### 不要になったブランチ bugfix を削除する
    $ git branch -d bugfix

  [git add]: git-add.md
  [git branch]: git-branch.md
  [git checkout]: git-checkout.md
  [git clean]: git-clean.md
  [git clone]: git-clone.md
  [git commit]: git-commit.md
  [git config]: git-config.md
  [git diff]: git-diff.md
  [git fetch]: git-fetch.md
  [git log]: git-log.md
  [git-merge]: git-merge.md
  [git mv]: git-mv.md
  [git pull]: git-pull.md
  [git push]: git-push.md
  [git-rebase]: git-rebase.md
  [git reflog]: git-reflog.md
  [git remote]: git-remote.md
  [git reset]: git-reset.md
  [git revert]: git-revert.md
  [git rm]: git-rm.md
  [git stash]: git-stash.md
  [git status]: git-status.md
  [git tag]: git-tag.md
