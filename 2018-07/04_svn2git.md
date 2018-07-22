# 古い subversion のレポジトリを git に変換した

Windows で TortoiseSVN で管理していたローカル svn レポジトリを git に変換した.
もう変換作業はないと思うが、一応記録しておく.

前提条件:

1. レポジトリは trunk すらなく、/ の直下に各プロジェクトのディレクトリが存在し、このディレクトリごとに git レポジトリを作成する
2. IaaS で CentOS 7.5 のまっさらな VM を作業用に用意(使い捨て)
3. Windows 上の fsfs な svn レポジトリを zip で固めて scp で 2. の Linux に転送
4. 作業が終わったら固めて、scp で Windows に戻す

以下、作業ログ

```
$ sudo su -
# yum install mod_dav_svn git-svn unzip
# cd /etc/httpd/conf
# cp -p httpd.conf httpd.conf.bak
# vi httpd.conf
# diff -u httpd.conf.bak httpd.conf
--- httpd.conf.bak      2018-06-26 18:07:11.000000000 +0000
+++ httpd.conf  2018-07-22 13:08:00.058609793 +0000
@@ -39,7 +39,7 @@
 # prevent Apache from glomming onto all bound IP addresses.
 #
 #Listen 12.34.56.78:80
-Listen 80
+Listen 127.0.0.1:8080

 #
 # Dynamic Shared Object (DSO) Support
@@ -351,3 +351,8 @@
 #
 # Load config files in the "/etc/httpd/conf.d" directory, if any.
 IncludeOptional conf.d/*.conf
+
+<Location /svn>
+    DAV svn
+    SVNPath /var/www/repository
+</Location>
# cd /var/www/
# unzip /home/c-yan/repository.zip
# apachectl configtest
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 10.0.1.4. Set the 'ServerName' directive globally to suppress this message
Syntax OK
# apachectl start
# cd
# svn co http://127.0.0.1:8080/svn/ svn
# mkdir git
# cd git
# ls ../svn | while read i; do
> mkdir $i
> cd $i
> git svn init -T /$i http://127.0.0.1:8080/svn/
> git svn fetch
> cd ..
> done
# for i in *; do \rm -r $i/.git/svn; done
# for i in *; do \rm $i/.git/refs/remotes/trunk; done
# cd ..
# tar cf - git > /home/c-yan/git.tar
# chown c-yan /home/c-yan/git.tar
```

`git svn clone`はエラーでコケることが多いので、`git svn init`と`git svn fetch`にバラして実行したほうが良い. また、svn は ignore がファイルではないので、`svn propget svn:ignore`で拾って`.gitignore`を作成する必要があるが、この作業には含まれていない.
