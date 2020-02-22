<!-- sectionTitle: Connect to GitHub -->

## GitとGithubを接続する

Gitでは､コードを共有するためになんらかの方法で通信する必要がある

<br/>

#### どんな通信方法(プロトコル)があるのか

- [Gitの公式ドキュメント](https://git-scm.com/book/ja/v2/Git%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC-%E3%83%97%E3%83%AD%E3%83%88%E3%82%B3%E3%83%AB)によると､Gitが対応しているのは､
  - Local / HTTP / Secure Shell (SSH) / Git
- [GitHubの公式ドキュメント](https://help.github.com/ja/github/using-git/which-remote-url-should-i-use)によると､GitHubが対応しているのは､
  - HTTPS / SSH

---

## GitとGithubを接続する

Gitでは､コードを共有するためになんらかの方法で通信する必要がある

<br/>

#### どんな通信方法(プロトコル)があるのか

- [Gitの公式ドキュメント](https://git-scm.com/book/ja/v2/Git%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC-%E3%83%97%E3%83%AD%E3%83%88%E3%82%B3%E3%83%AB)によると､Gitが対応しているのは､
  - Local / HTTP / Secure Shell (SSH) / Git
- [GitHubの公式ドキュメント](https://help.github.com/ja/github/using-git/which-remote-url-should-i-use)によると､GitHubが対応しているのは､
  - HTTPS / SSH

<br/>

## つまり､**HTTP**か**SSH**を使えばOK
(GitHubの公式ドキュメントにはとくに書いてないっぽいけど､Gitプロトコルも使いますが､意識しなくてok)

---

## Q. HTTPとSSH､どっちを使えばいいの?

---

<!-- note
ここで分岐する
-->

## A. どちらでも良いですが､今回はSSHを使います｡

- [GitHub公式](https://help.github.com/ja/github/using-git/which-remote-url-should-i-use)はHTTPSを使えといっている
- が､その理由はどうやら､SSHよりもHTTPSのほうが簡単でサポートが少なくなるからっぽい ([ソース](https://stackoverflow.com/questions/11041729/why-does-github-recommend-https-over-ssh))
- 今回はサポートするのは僕だし､エンジニアならどうせSSH使うので､今のうちに触っておこう

---

## 通信内容が外部に漏れた場合､いろいろやばい (語彙力)

---

<!-- note
ここが､GitHubがサポート面倒臭がっている部分
-->

ので､**通信相手がなりすましでない**こと､  
**通信内容が通信相手にしかわからない**こと  
**通信したことをバックレられない**こと  
が保証できるように工夫します  

---

ので､**通信相手がなりすましでない**こと､  
**通信内容が通信相手にしかわからない**こと  
**通信したことをバックレられない**こと  
が保証できるように工夫します  

### これに対応したのがSSH (すごい!)

---

<!-- note
興味ある人はなんかやってるんやなってことさえ知っていれば
勝手に調べるし､直接Gitに関係ないからとばしてもよい
というか､正しい情報を伝える難易度が高すぎて､正確な解説はインターネット上に
出回っていない｡(英語でも同様)
下手に解説しないようが良いかも
-->

## SSH (Secure Shell) とは

---

## SSHとはハイブリッド暗号通信プロトコル

<br/>

- 共通鍵暗号で使用するセッション鍵の生成: 鍵交換アルゴリズム
- 通信の暗号化: 共通鍵暗号
- ホスト認証やユーザ認証: 公開鍵暗号 / パスワード認証 / ワンタイムパスワードなど

<br/>

##### [RFC4250](https://tools.ietf.org/html/rfc4250)や[RFC4251](https://tools.ietf.org/html/rfc4251)などとして制定されている

---

<!-- note
この辺で一応詳しく解説したい
-->

## SSHで使う暗号鍵ペア (公開鍵と秘密鍵) をつくる

```shell
# 鍵をつくるコマンド
$ ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa -C 'your@email.com'  # GitHubに登録したメールアドレス
Enter passphrase (empty for no passphrase):                       # <-- パスフレーズを入力(任意)
Enter same passphrase again:                                      # <-- パスフレーズを確認(任意)
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.

# 鍵を置いた場所の内容を確認する
$ ls ~/.ssh
id_rsa  id_rsa.pub # <-- この2つのファイルがあればok
```

---

## 自分の公開鍵をGitHubに教える

```shell
# 公開鍵を表示してクリップボードにコピーする
$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCbErJTyTcqp/hWh8hMowDXWKHRLIWRe2AZ+vMK
j8Sh2VcRihHm+A8WtCWXary8tkFhYjtVzgD2mTpeThr/i8ZZuMOz2gZ0iXQeOQ6fjA4sHUwCzBmI
EqcUa+dfSQI8KrY6fNMp/GFLWn2fayLPpgYoaDtEvkiVJfHnhhyNzsmX7JtzBdX3s2mwTEW0JVwh
wyf1HxBoX07E/IP0m0/K/86fptzJE1AclC8MAPPA6Zgg1dN7HjjNSqeKWd+EqBFoallDq+LEpG94
3SJIjTVuybVDRoSKyzG1eAMpagXHCe54wmWDj+zqdH1Uum+w4e23gGev76Wua0prfQMXJg4Q+udU
d1JgHclu/g+yrMnm10fKRCJ3HbcVwaGPp4N1IbszhBA27wsZR87Qu5UrkVoWg194Kxs97f+z9CsF
Atj37RQ7903ejWhsr/JI68jPgNqXz/9cZv2MJ4CxP3zMO0iLsfqHPl7RPz4bOHnInwNDUCC4Hmdl
7yt2aC30szoEehSVJZa1cNyupmRVMuin9aXlJ11x1BkL5V4JW+7ki+3JKlTmhGdT9RLOpYetNlzQ
j166ZePpKpeL6cBbDhiCCd0urOXz1z8nazHsfIe0I570Zwrd8ygik/aXNZKZreQCLjFsQ/qJ6I0k
4d6NjF+Tmajp8zTae/Tlj2t24SeDNT9iLRk7Kw== your@email.com
```

---

<!-- classes: fullscreen -->

## 自分の公開鍵をGitHubに教える

GitHubの[SSH and GPG keys](https://github.com/settings/keys)設定ページにアクセスして**New SSH key**をクリック
<img src="https://github.com/anoriqq/anoriqq/blob/images/2020-02-06T13-53-51_GQ0o3BSQ3phLTLjAyAY5JWWM.jpg?raw=true"/>

---

<!-- classes: fullscreen -->

## 自分の公開鍵をGitHubに教える

- **Title**にこのSSH Keyを識別する名前を入力する `ユーザー名@OSやPC名`など
- **Key**に先程コピーしたSSH Keyをペーストする
- **Add SSH key**をクリックして､パスワードを求められたら入力する

<img src="https://github.com/anoriqq/anoriqq/blob/images/2020-02-06T14-09-41_pODmy4GrODBS9MnD9oHDmNYH.jpg?raw=true"/>

---

## SSH keysが追加されたことを確認

<img src="https://github.com/anoriqq/anoriqq/blob/images/2020-02-06T14-16-26_g8k5RWKmARRwSotsTwRpaxzu.jpg?raw=true"/>

---

<!-- note
リポジトリとは､コードを保存しておく場所
これから作るのは特にリモートリポジトリと呼ばれるもの
写真とかをクラウドにあげるように､コードをクラウドにあげる場所をつくるイメージ
-->

## リモートリポジトリをつくる

---

<!-- classes: fullscreen -->

<img src="https://github.com/anoriqq/anoriqq/blob/images/ITTShkAUKer2EMryNPPT9eo03adNbO.png?raw=true"/>

---

<!-- note
今回はgit-studyというリポジトリ名にしました
-->

<!-- classes: fullscreen -->

## Repository nameを入力してCreate repositoryボタンを押す

<img src="https://github.com/anoriqq/anoriqq/blob/images/SNfF3G5lA0kVlqMEO9kDPovPtfUzab.png?raw=true"/>

---

<!-- note
リポジトリが作成できた
-->

<!-- classes: fullscreen -->

<img src="https://github.com/anoriqq/anoriqq/blob/images/sgjLUKekHXIR7KfUJypqaTM9fTJdLJ.png?raw=true"/>

---

<!-- classes: fullscreen -->

## ローカルリポジトリにリモートリポジトリの場所を設定する

<img src="https://github.com/anoriqq/anoriqq/blob/images/sgjLUKekHXIR7KfUJypqaTM9fTJdLJ_LI.jpg?raw=true"/>

---

## ローカルリポジトリにリモートリポジトリの場所を設定する

```shell
$ git remote add origin git@github.com:anoriqq/git-study.git # <-- コピーしてきたURL
```

---

## ローカルリポジトリの内容をリモートリポジトリにプッシュする

```shell
$ git push -u origin master
```

---

## 無事､リモートにプッシュした内容が反映されているか確認する
