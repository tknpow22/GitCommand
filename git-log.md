git log
=======

### コミット履歴を表示する。

    git log

### コミット履歴を表示する(各コミットの diff をワード単位で表示する)。

    git log -p --word-diff

### コミット履歴を表示する(直近の n エントリのみ表示する)。

    git log -<number>
    git log -n <number>

### コミット履歴を表示する(各コミットの diff を表示する)。

    git log -p

### 例) コミット履歴をハッシュ(短縮版)と件名とでグラフィカルに表示する。

    git log --pretty=format:"%h %s" --graph

