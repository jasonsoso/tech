---
layout: note
title: Git
---
## List deleted files

`git log --diff-filter=D --summary`

See [Restore a deleted file in a Git repo](http://stackoverflow.com/questions/953481/restore-a-deleted-file-in-a-git-repo)

If you don't want all the information about which commit they were removed in, you can just add a `grep delete` in there.

`git log --diff-filter=D --summary | grep delete`

[via](http://stackoverflow.com/a/6018043/100031)
