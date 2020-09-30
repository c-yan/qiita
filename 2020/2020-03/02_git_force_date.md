# Git のコミットの日時を指定のものにする

GitHub の草の関係で指定したい場合とか.

```sh
$ git commit --amend --date="Sun Mar 1 23:59:59 2020 +0900"
$ git rebase HEAD~1 --committer-date-is-author-date
```
