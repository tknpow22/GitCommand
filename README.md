Git コマンドサンプル
====================

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

### ワークツリー、ステージングエリア、追跡対象外のファイルの状態を一覧表示する。

    git status

[git status]

### 現在のワークツリーを一時的に保存する。
スタックとして働くので先頭(stash@{0})にプッシュされる。

    git stash
    git stash save

[git stash]

### 一時的に保存されている内容から先頭のものをワークツリーに反映する。

    git stash apply

[git stash]

### 一時的に保存されている内容を見る。
スタックとして働くので一覧のうち先頭(stash@{0})が最後に保存(プッシュ)した内容。

    git stash list

[git stash]

### 一時的に保存されている内容から先頭のものを削除する。

    git stash drop

[git stash]

### ステージングされた内容とワークツリーの内容を比較する。

    git diff

[git diff]

### コミットされた内容とステージングされている内容を比較する。

    git diff --cached
    git diff --staged

[git diff]

### 編集中のファイルの内容を破棄し、直前にステージングした内容に戻す。
ステージングした内容がない場合、直前にコミットした内容に戻す

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

### 直前のコミットを打ち消しすための新たなコミットを作成する。
ひとつのコミットのみを元に戻す打ち消しであり、取り消しでないことに注意すること。
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
    # エディタが起動し、メッセージを入力する
    $  git log --oneline --decorate
    4372be1 (HEAD, master) Revert "add line3"
    d68cda0 add line3
    1c7205c add line2
    82f7743 add test.txt
    $ cat test.txt
    line1
    line2

[git revert]

### 編集中のファイルの内容を破棄し、直前にコミットした内容に戻す。
ステージングした内容がある場合、当該ファイルのステージング内容も破棄される。

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


### HEAD が指しているコミットに軽量タグを作成する。

    git tag <tag>

[git tag]

### HEAD が指しているコミットに注釈付きタグを作成する。

    git tag -a -m "message." <tag>

[git tag]

### ステージング済みのファイルを移動/リネームする。

    git mv <old_file> <new_file>

[git mv]

### 誤ってステージングしてしまったファイルをステージングエリアから削除する。
ワークツリーのファイルは変更されない。

    git rm --cached <file>
    git rm --cached -- <file>

[git rm]

### ワークツリーから追跡対象外のファイルを削除する。

    git clean -f

[git clean]

### ワークツリーから追跡対象外のファイルおよびフォルダを削除する。

    git clean -df

[git clean]

### ステージングエリアにある内容をコミットする。

    git commit -m "message."

[git commit]

### 追跡対象になっているファイルをステージングエリアに追加してから、コミットする。
git add を省略するときに使う。

    git commit -a -m "message."

[git commit]

### ステージングエリアにある内容で直前のコミットを上書きする。

リモートリポジトリに push した後は使用してはいけない。

    git commit --amend -m "message."

[git commit]

### コミット履歴をハッシュ(短縮版)、件名、タグ等を一行で表示する。

    git log --oneline --decorate

[git log]

### リモートブランチのコミット履歴を表示する。

    git log <remote_name>/<branch>

[git log]

### リモートリポジトリからフェッチし、マージする。

    git pull <remote_name>

[git pull]

### リモートリポジトリからフェッチし、リベースしてマージする。

    git pull --rebase <remote_name>

[git pull]

### リモートリポジトリの指定のブランチからフェッチし、マージする。

    git pull <remote_name> <branch>

[git pull]

### ローカルリポジトリからリポートリポジトリに送信する。

    git push <remote_name> <branch>

[git push]

### すべてのタグをリポートリポジトリの指定のブランチに送信する。

    git push <remote_name> <branch> --tags

[git push]

### 作成した新しいブランチをリモートリポジトリに登録する

    git push origin <branch>

[git push]

### ローカルリポジトリで削除したブランチをリモートリポジトリでも削除する。

    git push origin :<branch>

[git push]

### ローカルリポジトリのすべてのブランチを表示する。

    git branch

[git branch]

### ローカルのリモートブランチを表示する

    git branch -r

[git branch]

### すべてのブランチを表示する

    git branch -a

[git branch]

### 新しいブランチをローカルリポジトリに作成する。

    git branch <new_branch>

[git branch]

### ローカルリポジトリのブランチを削除する。

    git branch -d <branch>

[git branch]

### 新しいブランチをローカルリポジトリに作成し、作成したブランチに切り替える。

    git checkout -b <new_branch>

[git checkout]

### 指定のブランチを元に新しいブランチをローカルリポジトリに作成し、作成したブランチに切り替える。

    git checkout -b <new_branch> <existing_branch>

[git checkout]

### ブランチを切り替える

    git checkout <branch>

[git checkout]

### リモートリポジトリで削除されたブランチのローカルキャッシュを削除する。

    git fetch --prune origin

[git fetch]

### リモートリポジトリからすべてのブランチをフェッチする。

    git fetch <remote_name>

[git fetch]

### リモートリポジトリの指定のブランチをフェッチする。

    git fetch <remote_name> <branch>

[git fetch]

### 指定したブランチを現在のブランチにマージする。

    git merge <branch>

[git-merge]

### 指定したブランチを現在のブランチにマージする(ファストフォワードしない)。

    git merge --no-ff <branch>

[git-merge]

### リモートブランチの内容をマージする

    git merge <remote_name>/<branch>

[git-merge]

### 現在のブランチを指定のブランチにリベースする。
リモートリポジトリに push した後は使用してはいけない。

    git rebase <branch>

[git-rebase]

### ローカルリポジトリの reflog を表示する。

    git reflog

[git reflog]

### リモート接続の一覧を URL を含めて表示する。

    git remote -v

[git remote]

### リモートリポジトリに対する新規接続を作成する。

    git remote add <name> <remote_url>

[git remote]

例) 新たなローカルリポジトリを作成し、それにリモートリポジトリを追加する。

    $ cd /repobase
    $ mkdir sample
    $ cd sample
    $ git init
    $ git remote add origin https://github.com/tknpow22/sample.git

### リモートリポジトリに対する接続名を変更する。

    git remote rename <old_name> <new_name>

[git remote]

### 指定のリモートリポジトリの情報を表示する。

    git remote show <name>

[git remote]

### リモートリポジトリに対する接続を削除する。

    git remote rm <name>

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
