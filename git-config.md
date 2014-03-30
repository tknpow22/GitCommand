git config
==========

### デフォルトのユーザ、メールアドレスを設定する。
設定した内容は $HOME/.gitconfig に保存される。

    git config --global user.name "name"
    git config --global user.email "email"

### コミットする際に改行を自動変換しないよう設定する。
設定した内容は $HOME/.gitconfig に保存される。

    git config --global core.autocrlf false

### カレントブランチと同名のリモートブランチが存在する場合のみ、カレントブランチをプッシュするよう設定する。
設定した内容は $HOME/.gitconfig に保存される。

    git config --global push.default simple

