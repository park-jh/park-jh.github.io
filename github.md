# githubを使用してみよう。

## 目的
rails templateファイルをgithubに登録して使って見たい。テンプレートを共有したい。

## gitリポジトリを生成する。
1. rails template関連ファイルが集まれているディレクトリへ移動します。
2. そして、ローカルリポジトリを生成します。
3. インデックスに追加し、変更点全てを一括してカレントブレンチに適用します。
  ```
  cd ~/documents/study/temp
  git init .
  git add .
  git commit
  ```
4. このリポジトリで使う固有のユーザ名、メールアドレスを設定する。
  ```
  git config user.email 'park-jh@gameon.co.jp'
  git config user.name 'park-jh'
  git config -l
  ```
5. 参考
  * http://transitive.info/article/git/command/config/

## githubでリモートリポジトリを生成する。
1. githubにログインする。githubアカウントがない場合は作成する。
2. New repositoryボタンをクリックする。  
<img src="file://localhost/Users/devnote/Library/Mobile%20Documents/com~apple~CloudDocs/notes/git/github01.png" alt="setting vpn" width="500" height="200" />
3. Repository nameを入力して、Create repositoryボタンをクリックする。  
   Descriptionはオプションなので、入力しなくても構わない。（しかし、簡単なことでも書いたほうがいいと思う。）
<img src="file://localhost/Users/devnote/Library/Mobile%20Documents/com~apple~CloudDocs/notes/git/github02.png" alt="setting vpn" width="640" height="480" />

## 鍵を生成して登録する。
1. 鍵を生成する。
  ```
  [devnote@hooni:~/documents/study/temp] % cd ~/.ssh
  total 56
  drwx------   9 devnote  staff   306B  5 25 15:39 ./
  drwxr-xr-x+ 71 devnote  staff   2.4K  9 10 17:51 ../
  -rw-------   1 devnote  staff   389B 12 24  2014 authorized_keys
  -rw-r--r--   1 devnote  staff   829B 12 29  2014 config
  -rw-------   1 devnote  staff   1.6K  5 25 15:39 id_rsa
  -rw-r--r--   1 devnote  staff   401B  5 25 15:39 id_rsa.pub
  -rw-r--r--   1 devnote  staff   1.6K  9 10 17:46 known_hosts
  [devnote@hooni:~/.ssh] % ssh-keygen -t rsa -C 'park-jh@gameon.co.jp'
  Generating public/private rsa key pair.
  Enter file in which to save the key (/Users/devnote/.ssh/id_rsa): id_gameon
  Enter passphrase (empty for no passphrase):
  Enter same passphrase again:
  Your identification has been saved in id_gameon.
  Your public key has been saved in id_gameon.pub.
  The key fingerprint is:
  18:cd:ea:87:be:05:4f:f7:2c:a9:48:92:66:e4:80:83 park-jh@gameon.co.jp
  The key's randomart image is:
  +--[ RSA 2048]----+
  |                 |
  |       o         |
  |      . o        |
  |..     +         |
  |E . . + S .      |
  | . + o = . +     |
  |    * + + o o    |
  |   o + + . .     |
  |      +..        |
  +-----------------+
  [devnote@hooni:~/.ssh] % ll
  total 72
  drwx------  11 devnote  staff   374B  9 10 17:55 ./
  drwxr-xr-x+ 71 devnote  staff   2.4K  9 10 17:51 ../
  -rw-------   1 devnote  staff   389B 12 24  2014 authorized_keys
  -rw-r--r--   1 devnote  staff   829B 12 29  2014 config
  -rw-------   1 devnote  staff   1.6K  9 10 17:55 id_gameon
  -rw-r--r--   1 devnote  staff   402B  9 10 17:55 id_gameon.pub
  -rw-------   1 devnote  staff   1.6K  5 25 15:39 id_rsa
  -rw-r--r--   1 devnote  staff   401B  5 25 15:39 id_rsa.pub
  -rw-r--r--   1 devnote  staff   1.6K  9 10 17:46 known_hosts
  ```
2. githubに公開鍵を登録する。
macだったら次のコマンドでclipboardにコピーする。
  ```
  [devnote@hooni:~/.ssh] % pbcopy < id_gameon.pub
  ```
githubサイトから上段の右側にあるアイコンをクリックして、settingを選択する。  
<img src="file://localhost/Users/devnote/Library/Mobile%20Documents/com~apple~CloudDocs/notes/git/github03.png" alt="setting vpn" width="300" height="300" />  
SSH keysを選択してAdd SSH keyというボタンをクリックする。  
入力欄に先ほど生成した公開鍵をコピーペする。  
3. 秘密鍵を追加する。
  ```
  [devnote@hooni:~/documents/study/temp] % ssh-add ~/.ssh/id_gameon
  Identity added: /Users/devnote/.ssh/id_gameon (/Users/devnote/.ssh/id_gameon)
  [devnote@hooni:~/documents/study/temp] % ssh-add -l
  2048 18:cd:ea:87:be:05:4f:f7:2c:a9:48:92:66:e4:80:83 /Users/devnote/.ssh/id_gameon (RSA)
  ```
4. リモートリポジトリへpushする。
  ```
  [devnote@hooni:~/documents/study/temp] % git remote add origin git@github.com:park-jh/templates.git    
  [devnote@hooni:~/documents/study/temp] % git push -u origin master
  ```
5. 参考
  * http://qiita.com/tsumekoara/items/6c3e96c394bb0f753ea0
  * http://qiita.com/isaoshimizu/items/84ac5a0b1d42b9d355cf


## トラブル
1. リモートリポジトリにPushするときエラーが発生する。
  * 現象
      ```
      [devnote@hooni:~/documents/study/temp] % git push -u origin master
      error: src refspec master does not match any.
      error: failed to push some refs to 'git@github.com:park-jh/templates.git'
      ```
  * 原因  
      インデックスに追加してカレントブレンチに適用しない状態で、リモートリポジトリへpushした。
  * 対応
      ```
      git init .
      git add .
      git commit
      git push -u origin master
      ```
2. リモートリポジトリにpushするときpermission deniedエラーが発生する。
  * 現象
      ```
      [devnote@hooni:~/documents/study/temp] % git push -u origin master
      Permission denied (publickey).
      fatal: Could not read from remote repository.

      Please make sure you have the correct access rights
      and the repository exists.
      ```
  * 原因  
      一緒に生成した秘密鍵が登録されていなかったので発生した。
  * 対応
      ```
      [devnote@hooni:~/documents/study/temp] % ssh-add ~/.ssh/id_gameon
      Identity added: /Users/devnote/.ssh/id_gameon (/Users/devnote/.ssh/id_gameon)
      [devnote@hooni:~/documents/study/temp] % git push -u origin master
      Warning: Permanently added the RSA host key for IP address '192.30.252.129' to the list of known hosts.
      Counting objects: 30, done.
      Delta compression using up to 8 threads.
      Compressing objects: 100% (25/25), done.
      Writing objects: 100% (30/30), 12.82 KiB | 0 bytes/s, done.
      Total 30 (delta 4), reused 0 (delta 0)
      To git@github.com:park-jh/templates.git
       * [new branch]      master -> master
      Branch master set up to track remote branch master from origin.
      ```
3. リモートリポジトリへpushしたらrejectされた。
  * 現象
    ```
    devnote@hooni:~/documents/study/temp] % git push -u origin master                                                                               (git)-[master]
    To git@github.com:park-jh/templates.git
     ! [rejected]        master -> master (non-fast-forward)
    error: failed to push some refs to 'git@github.com:park-jh/templates.git'
    hint: Updates were rejected because the tip of your current branch is behind
    hint: its remote counterpart. Integrate the remote changes (e.g.

      1 Merge branch 'temp'
    hint: 'git pull ...') before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.
    ```
  * 原因  
    わからない。
  * 対応
    ```
    #任意のbranchを作成してmasterから切り替える。
    [devnote@hooni:~/documents/study/temp] % git branch temp                                                                                         (git)-[master]
    [devnote@hooni:~/documents/study/temp] % git checkout temp                                                                                       (git)-[master]
    Switched to branch 'temp'
    [devnote@hooni:~/documents/study/temp] % git pull origin +master:temp                                                                              (git)-[temp]
    From github.com:park-jh/templates
     + 5a75b02...95069f4 master     -> temp  (forced update)
    Warning: fetch updated the current branch head.
    Warning: fast-forwarding your working tree from
    Warning: commit 5a75b02d1ecbf144bb8775668b945b8bb159e8cf.
    Already up-to-date.

    # tempとmasterの差分を確認する。
    [devnote@hooni:~/documents/study/temp] % git diff temp master                                                                                      (git)-[temp]
    diff --git a/src/config/postgresql.yml b/src/config/postgresql.yml
    index c0df804..43aeab3 100644
    --- a/src/config/postgresql.yml
    +++ b/src/config/postgresql.yml
    @@ -2,8 +2,9 @@ default: &default
       adapter: postgresql
       encoding: unicode
       pool: 5
    -  username: devnote
    -  template: template1
    +  username: vagrant
    +  password: vagrant
    +  template: template2

     development:
       <<: *default
    @@ -16,11 +17,9 @@ test:
     production:
       <<: *default
       database: APPNAME
    -  username: devnote
    -  password: devnote
       host: 192.168.33.30
       port: 9999
    -  template: template1
    +  template: template2

     ora:
       adapter: oracle_enhanced

    # master branchに切り替える。
    [devnote@hooni:~/documents/study/temp] % git checkout master                                                                                       (git)-[temp]
    Switched to branch 'master'
    Your branch and 'origin/master' have diverged,
    and have 2 and 1 different commit each, respectively.
      (use "git pull" to merge the remote branch into yours)

    # tempをmergeする。
    [devnote@hooni:~/documents/study/temp] % git merge temp                                                                                          (git)-[master]
    Merge made by the 'recursive' strategy.

    # リモートリポジトリへpushする。
    [devnote@hooni:~/documents/study/temp] % git push -u origin master                                                                               (git)-[master]
    Counting objects: 7, done.
    Delta compression using up to 8 threads.
    Compressing objects: 100% (7/7), done.
    Writing objects: 100% (7/7), 841 bytes | 0 bytes/s, done.
    Total 7 (delta 3), reused 0 (delta 0)
    To git@github.com:park-jh/templates.git
       95069f4..b90464d  master -> master
    Branch master set up to track remote branch master from origin. 
    ```
    もっと簡単にできるみたい。  
    ```
    git fetch && git merge origin/master
    ```
    確認はしていない。
  * 参考  
    http://d.hatena.ne.jp/snaka72/20100602/1275496817
