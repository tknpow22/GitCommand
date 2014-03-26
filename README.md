Git コマンドサンプル
====================

### 既にリモートリポジトリがあり、それを使ってローカルリポジトリを作成(クローン)する。
以下の例ではrepobase下にリポジトリ名でフォルダが作成される。

    cd /repobase
    git clone <remote_url>

### 既にリモートリポジトリがあり、それを使ってローカルリポジトリを作成(クローン)する。
ローカルのワークツリーのフォルダ名を指定する場合に使用する。

    cd /repobase
    git clone <remote_url> <repodir>

### 既にリモートリポジトリがあり、それを使ってローカルリポジトリを作成する。

    mkdir /repobase/repodir
    cd /repobase/repodir
    git init
    git remote add origin <remote_url>

### 新しいリモートリポジトリがあり、そこにローカルリポジトリを反映させる。

    git remote add origin <remote_url>
    git push -u origin --all
    git push -u origin --tags

### パスワードを平文で保存する
このコマンドを実行後、認証が必要なコマンド(git remote show origin など)を発行した際に
入力したパスワードが $HOME/.git-credentials に保存される。

    git config credential.helper store

### ローカルのワークツリーの内容をステージングエリアに追加する。
ワイルドカードも使用できる。また、*追加*といいながら SVN とは違い、
ファイルを改変したことを追加するものであり、ファイルの追加、更新毎に行う必要がある。

    git add <file>
    git add <directory>

### ステージされた内容とワークツリーの内容を比較する。

    git diff

### コミットされた内容とステージされた内容を比較する。

    git diff --cached
    git diff --staged

### ステージ済みのファイルを移動/リネームする。

    git mv <old_file> <new_file>

### ワークツリーの特定のファイルの変更を取り消す
ワークツリーでの変更はすべて消えてしまうので、注意すること。

    git checkout -- <file>

### ステージングエリアにある内容(スナップショット)をローカルリポジトリにコミットする。

    git commit -m "message."

### 追跡対象になっているファイルをステージし、ローカルリポジトリにコミットする。

    git commit -a -m "message."

### ステージングエリアにある内容(スナップショット)で直前のコミットを修正して、ローカルリポジトリにコミットする。
リモートリポジトリに push した後は使用してはいけない。

    git commit --amend -m "message."

### ワークツリー、ステージングエリア、追跡対象外のファイルを一覧表示する。

    git status

### コミット履歴を表示する。

    git log

### コミット履歴を表示する(直近の n エントリのみ表示する)。

    git log -<number>
    git log -n <number>

### コミット履歴を表示する(各コミットの diff を表示する)。

    git log -p

### コミット履歴を表示する(各コミットの diff をワード単位で表示する)。

    git log -p --word-diff

### 例) コミット履歴をハッシュ(短縮版)と件名とでグラフィカルに表示する

    git log --pretty=format:"%h %s" --graph

### ファイルの過去のリビジョンをチェックアウトする。
ワークツリーの指定したファイルは置き換えられるので注意すること。

    git checkout <commit> <file>

### ワークツリーのすべてのファイルを指定したリビジョンに置き換える
このコマンドを実行すると、"detached HEAD" 状態になるので注意すること。
なお、ワークツリーにステージングしていない変更がある場合、警告が表示され、置き換えられない。

    git checkout <commit>

### コミット済みのスナップショットを元に戻す(打ち消す)。
ひとつのコミットのみを元に戻す打ち消しであり、取り消しでないことに注意すること。
そのため、打ち消したコミットも履歴に残る。

    git revert <commit>

### コミット済みのスナップショットを元に戻す(取り消す)。
履歴をも取り消してしまうため、ローカルリポジトリにのみ使用すること。
リモートリポジトリに push した後は使用してはいけない。

    git reset <commit>

### ワークツリーから追跡対象外のファイルを削除する。

    git clean -f

### ワークツリーから追跡対象外のファイルおよびフォルダを削除する。

    git clean -df

### ワークツリーから削除したステージング済みのファイルをステージングエリアから削除する。

    git rm <file>

### ワークツリーにファイルは残しつつステージングエリアに追加済みのファイルを削除する。

    git rm --cached <file>

### ローカルリポジトリのすべてのブランチを表示する。

    git branch

### ローカルのリモートブランチを表示する

    git branch -r

### すべてのブランチを表示する

    git branch -a

### 新しいブランチをローカルリポジトリに作成する。

    git branch <new_branch>

### 新しいブランチをローカルリポジトリに作成し、作成したブランチに切り替える。

    git checkout -b <new_branch>

### 指定のブランチを元に新しいブランチをローカルリポジトリに作成し、作成したブランチに切り替える。

    git checkout -b <new_branch> <existing_branch>

### ブランチを切り替える

    git checkout <branch>

### 作成した新しいブランチをリモートリポジトリに登録する

    git push origin <branch>

### ローカルリポジトリのブランチを削除する。

    git branch -d <branch>

### リモートリポジトリで削除されたブランチのローカルキャッシュを削除する。

    git fetch --prune origin

### ローカルリポジトリで削除したブランチをリモートリポジトリでも削除する。

    git push origin :<branch>

### 指定したブランチを現在のブランチにマージする。

    git merge <branch>

### 指定したブランチを現在のブランチにマージする(ファストフォワードしない)。

    git merge --no-ff <branch>

### 現在のブランチを指定のブランチにリベースする。
リモートリポジトリに push した後は使用してはいけない。

    git rebase <branch>

### ローカルリポジトリの reflog を表示する。

    git reflog

### リモート接続の一覧を URL を含めて表示する。

    git remote -v

### リモートリポジトリに対する新規接続を作成する。

    git remote add <name> <remote_url>

### リモートリポジトリに対する接続名を変更する。

    git remote rename <old_name> <new_name>

### 指定のリモートリポジトリの情報を表示する。

    git remote show <name>

### リモートリポジトリに対する接続を削除する。

    git remote rm <name>

### リモートリポジトリからすべてのブランチをフェッチする。

    git fetch <remote_name>

### リモートリポジトリの指定のブランチをフェッチする。

    git fetch <remote_name> <branch>

### リモートブランチのコミット履歴を表示する。

    git log <remote_name>/<branch>

### リモートブランチの内容をマージする

    git merge <remote_name>/<branch>

### リモートリポジトリからフェッチし、マージする。

    git pull <remote_name>

### リモートリポジトリからフェッチし、リベースしてマージする。

    git pull --rebase <remote_name>

### リモートリポジトリの指定のブランチからフェッチし、マージする。

    git pull <remote_name> <branch>

### ローカルリポジトリからリポートリポジトリに送信する。

    git push <remote_name> <branch>

### タグをリポートリポジトリに送信する。

    git push <remote_name> <branch> --tags

### 例) master のワークツリーの作業内容をリモートリポジトリの内容を取得、マージ(リベース)してコミットを整理後、リモートリポジトリに送信する。

    git checkout master
    git fetch origin master
    git rebase origin/master
    # マージ作業を行う
    git push origin master

