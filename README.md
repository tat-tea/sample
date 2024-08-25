WSL 2 上で、`sudo` コマンドを使わずに Docker を使用できるようにするための手順を以下に整理しました。

### 1. WSL 2 の確認と有効化
まず、WSL 2 がインストールされていることを確認し、WSL 2 を使用していることを確認します。

```bash
wsl -l -v
```

### 2. Systemd の有効化
WSL 2 で systemd を有効にします。

```bash
sudo nano /etc/wsl.conf
```

以下の内容を追加して保存します:
```ini
[boot]
systemd=true
```

次に、WSL を再起動します。
```bash
wsl --shutdown
```

### 3. Docker のインストール
WSL 2 上で systemd を使って Docker を管理するために、以下の手順で Docker をインストールします。

**リポジトリと GPG 鍵の追加:**
```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

**Docker リポジトリの設定:**
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

**Docker のインストール:**
```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

### 4. Docker を `sudo` なしで実行できるように設定

1. **`docker` グループを作成する:**
   ```bash
   sudo groupadd docker
   ```

2. **現在のユーザーを `docker` グループに追加する:**
   ```bash
   sudo usermod -aG docker $USER
   ```

3. **変更を反映するために、WSL セッションを再起動する:**
   ```bash
   exit
   ```
   その後、再度 WSL にログインします。

### 5. Docker サービスの開始と有効化

Docker デーモンを起動し、自動起動を設定します。

**Docker の起動:**
```bash
sudo systemctl start docker
```

**Docker の自動起動設定:**
```bash
sudo systemctl enable docker
```

### 6. 動作確認

**Docker コマンドの実行 (sudo なし):**
```bash
docker --version
```
```bash
docker run hello-world
```

これで、`sudo` なしで Docker コマンドを実行できるようになります。もし `docker` グループに追加した後に問題が発生した場合、再起動やセッションの再ログインを試みてください。
