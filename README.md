## やってくれること
* bash_profileの基本形を配置
* bash.dディレクトリ作成（rbenvやnodejsの環境パス設定用）
* gitインストール
* ruby, rbenvインストール
* rails配置用ディレクトリ生成（パス：~/rails）
* unicorn（アプリケーションサーバ）インストール  
（nginxは手動インストール）
* postgresqlインストール
* nodejs 10.6.0インストール
* imagemagickインストール

## Usage

### ec2-user(sudoパスワードなし)を用いてansibleを起動する場合
* ローカルにanbible 2.6.1をインストール
* .ssh/configにec2へのssh接続情報追記

```bash
Host my-server
      Hostname ***.amazonaws.com
      IdentityFile ~/.ssh/****.pem
      User ec2-user
```

* 下記、ansible起動コマンドを実行

```bash
$ ansible-playbook -i hosts ec2-server.yml
```

### 別ユーザ（sudoパスワードあり）を用いてansibleを起動する場合
※上記の1.2を行った上で
* プロジェクトルートパス上にprivate.ymlを作成
    * 以下のようにsudoパスワードを追記
    ```yml
    ansible_sudo_pass: ****（sudoパスワード）
    ```
* 以下コマンドでprivate.ymlを暗号化
```bash
$ ansible-vault encrypt private.yml

# （入力後、暗号化用パスワード入力）
```
* 下記、ansible起動コマンドを実行
```bash
ansible-playbook -i hosts ec2-server.yml --extra-vars="@private.yml" --ask-vault-pass
```
