git remote
==========

### リモートリポジトリに対する接続名を変更する。

    git remote rename <old_name> <new_name>

例) 新たにリポートリポジトリ https://github.com/tknpow22/sample.git を rtest として追加し、utest に変更する。

    $ git remote -v
    origin  https://github.com/tknpow22/sample.git (fetch)
    origin  https://github.com/tknpow22/sample.git (push)
    $ git remote add rtest https://github.com/tknpow22/sample.git
    $ git remote -v
    origin  https://github.com/tknpow22/sample.git (fetch)
    origin  https://github.com/tknpow22/sample.git (push)
    rtest   https://github.com/tknpow22/sample.git (fetch)
    rtest   https://github.com/tknpow22/sample.git (push)
    
    $ git remote rename rtest utest
    
    $ git remote -v
    origin  https://github.com/tknpow22/sample.git (fetch)
    origin  https://github.com/tknpow22/sample.git (push)
    utest   https://github.com/tknpow22/sample.git (fetch)
    utest   https://github.com/tknpow22/sample.git (push)

### 指定のリモートリポジトリの情報を表示する。

    git remote show <name>

例) リポートリポジトリ utest の情報を表示する。

    $ git remote show utest
    * remote utest
      Fetch URL: https://github.com/tknpow22/sample.git
      Push  URL: https://github.com/tknpow22/sample.git
      HEAD branch: master
      Remote branches:
        iss5   new (next fetch will store in remotes/utest)
        master new (next fetch will store in remotes/utest)
      Local ref configured for 'git push':
        master pushes to master (up to date)

### リモートリポジトリに対する接続を削除する。

    git remote remove <name>

例) リポートリポジトリ utest に対する接続を削除する。

    $ git remote -v
    origin  https://github.com/tknpow22/sample.git (fetch)
    origin  https://github.com/tknpow22/sample.git (push)
    utest   https://github.com/tknpow22/sample.git (fetch)
    utest   https://github.com/tknpow22/sample.git (push)
    
    $ git remote remove utest
    
    $ git remote -v
    origin  https://github.com/tknpow22/sample.git (fetch)
    origin  https://github.com/tknpow22/sample.git (push)

