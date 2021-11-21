# docker_laravel_mailhog
mailhogを使えるLaravel環境を作るためのdocker-compose

## 概要
docker-composeを使って最小限のmailhog付きLaravel(LEMP)環境を構築することができます。

## 前提条件
(まず https://github.com/Asano-Naoki/docker_laravel_minimum を試すことをおすすめします。)

このリポジトリを使う前に、メール機能のあるLaravelプロジェクトディレクトリを用意してください。

新しいLaravelプロジェクトディレクトリを作る場合は、以下の手順に従ってください。
1. まっさらなLaravelプロジェクトディレクトリを作ります。
```
docker run --rm -v $PWD:/app composer create-project laravel/laravel example-mailhog-app
```
2. .envファイルを編集します。
```
-DB_HOST=127.0.0.1
+DB_HOST=mysql
-DB_USERNAME=root
-DB_PASSWORD=
+DB_USERNAME=test_user
+DB_PASSWORD=test_pass
-MAIL_FROM_ADDRESS=null
+MAIL_FROM_ADDRESS=test@test
```
3. このリポジトリ内のファイルとディレクトリをすべて、gitコマンド（fetchとmerge）か手動で、Laravelプロジェクトディレクトリにコピーします。
```
git init
git add .
git commit -m "first commit"
git remote add origin git@github.com:Asano-Naoki/docker_laravel_mailhog.git
git fetch
git merge origin/main --allow-unrelated-histories
```
4. docker-composeを開始します。
```
docker-compose up -d
```
5. usersテーブルを作ります。
```
docker-compose exec php sh
...
(after entering sh of php)
...
php artisan migrate
```
注:
最初のセットアップ時には、mysqlファイルがすべて作られていないために、php artisan migrateコマンドが失敗するかもしれません。その場合は数分待ってください。

6. Laravel Breezeをインストールします。
```
docker run --rm -v $PWD:/app composer require laravel/breeze
```
7. 認証ファイルを発行します。
```
docker-compose exec php sh
...
(after entering sh of php)
...
php artisan breeze:install
```
8. assetsをコンパイルします。
```
docker run --rm  -v $PWD:/usr/src/app -w /usr/src/app node npm install && npm run dev
```
9. 新しいユーザーを登録します。

    (1) トップページに行きます(http://localhost/)。 

    (2) 画面右上の"Register"リンクをクリックします(http://localhost/register)。

    (3) 空欄を適切に埋めてRegisterボタンを押します。

    (4) 先ほど作成したユーザー名をクリックし、"Log Out"をクリックします。

    (5) 画面右上の"Log in"をクリックします(http://localhost/login)。

    (6) "Forgot your password?"リンクをクリックします。

    (7) (3)で埋めたメールアドレスを入力し、EMAIL PASSWORD RESET LINKボタンをクリックします。

エラーについては https://github.com/Asano-Naoki/docker_laravel_minimum も参照してください。



## 使い方
Laravelアプリケーションでメールを送ってください。そうしたら自動的にmailhogで補足されます。http://localhost:8025/ でそのメールを確認できます。

注:
"Cannot send message without a sender address"エラーが発生しないようにするためには、.envファイルでMAIL_FROM_ADDRESSにメールアドレスを表す文字列を設定する必要があります。


## 著者
[浅野直樹](https://asanonaoki.com/blog/)


## ライセンス
MITライセンスの元にライセンスされています。詳細は[LICENSE](/LICENSE)をご覧ください。



