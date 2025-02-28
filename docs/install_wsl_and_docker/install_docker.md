# やや丁寧なDockerのインストール

## 1. 前準備

前準備としてwslの更新を行っておく

Windows PowerShellにて以下のコマンドを実行
```pwsh
wsl --update
```

## 2. Docker Engineのインストール

> [!NOTE]
> wslと言えばLINUXのシステムを指すが、これ以降ではUbuntu自体をwslと呼んでいる

以下の5行のコマンドをwsl内で実行

1. まずパッケージの更新を行っておく
    ```sh
    sudo apt update && sudo apt upgrade -y
    ```

2. Docker Engineのインストール
    ```sh
    sudo sh -c "$(curl -fsSL https://get.docker.com)"
    ```
    - `https://get.docker.com` には[docker github](https://github.com/docker/docker-install/blob/master/install.sh)のシェルスクリプトが置いてあり、curlコマンドによってGETリクエストを送信している。
    - レスポンスとしてシェルスクリプトが `$(  )` 内で展開され、root権限でdockerのインストールが実行される。
    ![](../images/wsl_2.png)

<!-- get.docker.comからインストールスクリプトを直接実行している位の注釈を入れるか？ -->
<br>

3. ルート以外のユーザでもdockerコマンドを使用可能にする
    ```sh
    sudo usermod -aG docker $USER
    ```

4. systemdを有効化させる
    - wsl.confに `systemd=true` を リダイレクトで `/etc/wsl.conf` に書き込み
    - ~~このコマンドを実行しなくても元から存在する場合もありそうではあるが~~
    ```sh
    sudo sh -c 'echo "[boot]\nsystemd=true" > /etc/wsl.conf'
    ```
![](../images/wsl_3.png)

5. その後、wslのウィンドウを閉じ、PowerShellにて以下のコマンドを実行
    - wslを再起動して設定を反映させる
    ```sh
    wsl --shutdown
    ```

## 3. dockerの動作確認
再度wslを起動し、以下のコマンドを実行
```sh
docker run --rm hello-world
```
画像のような出力が出てくればDockerの導入は成功
![hello-world](../images/wsl_4.png)
