# intro_docker

## Getting Started with Docker

- wslのセットアップがまだなら[wslのセットアップ][install_wsl]から
- Dockerのインストールの詳細は[Dockerのセットアップ][install_docker]へ

### 1. wslのアップデート
<!-- microsoft storeで`Windows Subsystem for Linux`と検索 -->
<!-- <img src="./docs/images/windows_subsystem_for_linux.png" width="320px"> -->
PowerShellにて以下のコマンドを実行

```pwsh
wsl --update
```

### 2. docker ceのインストール
以下をwsl内で実行
```sh
sudo sh -c "$(curl -fsSL https://get.docker.com)"
sudo usermod -aG docker $USER
sudo sh -c 'echo "[boot]\nsystemd=true" > /etc/wsl.conf'
```

その後、wslのウィンドウを閉じ、PowerShellにて以下のコマンドを実行
```sh
wsl --shutdown
```

### 3. dockerの動作確認
再度wslを起動し、以下のコマンドを実行
```sh
docker run --rm hello-world
```
画像のような出力が出てくればDockerの導入は成功
![hello-world](docs/images/wsl_4.png)

## Containers using Dev Containers with vscode

基本的には以下のようなディレクトリの構成で運用

```
.
├── .devcontainer/
│   ├── devcontainer.json
│   └── Dockerfile
├── project_directory/
│   ├── lib/
│   └── main.py
└── README.py
```

### devcontainers.json

```json
{
    "name": "python 3.11",
    "build": { "dockerfile": "Dockerfile" },
    "runArgs": ["--init", "--name", "py_workbench"],
    "customizations": {
        "vscode": {
            "settings": {
                "diffEditor.ignoreTrimWhitespace": false,
                "files.insertFinalNewline": true,
                "files.trimTrailingWhitespace": true,
                "markdown-preview-enhanced.scrollSync": false
                // "notebook.lineNumbers": "on"
            },
            "extensions": [
                "oderwat.indent-rainbow",
                "yzhang.markdown-all-in-one",
                "shd101wyy.markdown-preview-enhanced",
                "donjayamanne.python-extension-pack"
                // "ms-toolsai.jupyter",
            ]
          }
    }
}
```

### Dockerfile

```Dockerfile
FROM python:3.11-bullseye
USER root

RUN DEBIAN_FRONTEND=noninteractive \
    apt-get update && \
    apt-get purge -y imagemagick imagemagick-6-common && \
    apt-get install -y --no-install-recommends \
    python3-tk && \
    apt-get -y clean && \
    rm -rf /var/lib/apt/lists/*

RUN python -m pip install --upgrade pip
RUN python -m pip install numpy scikit-learn matplotlib
```


<!-- path list -->
[install_wsl]: docs/install_wsl_and_docker/install_wsl.md
[install_docker]: docs/install_wsl_and_docker/install_docker.md
