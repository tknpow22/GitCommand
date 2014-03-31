git push
========

### 新しいリモートリポジトリを追加し、ローカルリポジトリの内容を反映させる。

    git remote add <remote_name> <remote_url>
    git push -u <remote_name> --all
    git push -u <remote_name> --tags

### 作成した新しいブランチをリモートリポジトリに登録する

    git push <remote_name> <branch>

### ローカルリポジトリで削除したブランチをリモートリポジトリでも削除する。

    git push <remote_name> :<branch>


