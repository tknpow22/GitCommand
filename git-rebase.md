git rebase
==========

### git rebase -i を使ってコミットをまとめる。

以下のような状態で bbb と ccc を bbb&ccc にまとめたい。

```
$ git log
commit d8a7fb0c9cee54001ca67b0f61a118874d7e6d07
Author: tknpow22 <tknpow22@koutou-software.net>
Date:   Fri Apr 7 22:20:34 2017 +0900

    ccc

commit 24c2732dc84303d57fe766dd052e1e82757e10dd
Author: tknpow22 <tknpow22@koutou-software.net>
Date:   Fri Apr 7 22:20:23 2017 +0900

    bbb

commit 92ba29d415feec7aed86ee9ee27814f1f0014ad2
Author: tknpow22 <tknpow22@koutou-software.net>
Date:   Fri Apr 7 22:20:07 2017 +0900

    aaa
```

aaa の commit を指定する。

    git rebase -i 92ba29d415feec7aed86ee9ee27814f1f0014ad2

エディタが起動するので、新しい方に squash を指定する

```
pick 24c2732 bbb
pick d8a7fb0 ccc
```
↓
```
pick 24c2732 bbb
s d8a7fb0 ccc
```

再度エディタが起動するので、コメントをひとつにまとめる。

```
# This is a combination of 2 commits.
# The first commit's message is:

bbb

# This is the 2nd commit message:

ccc

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
```
↓
```
# This is a combination of 2 commits.
# The first commit's message is:

bbb&ccc

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
```

完了。

```
$ git log
commit d0fe034393e391dd694be87fc7338875702aef0c
Author: tknpow22 <tknpow22@koutou-software.net>
Date:   Fri Apr 7 22:20:23 2017 +0900

    bbb&ccc

commit 92ba29d415feec7aed86ee9ee27814f1f0014ad2
Author: tknpow22 <tknpow22@koutou-software.net>
Date:   Fri Apr 7 22:20:07 2017 +0900

    aaa

```
