3. Docker のインストール
WSL 2 上で systemd を使って Docker を管理するために、以下の手順で Docker をインストールします。

リポジトリと GPG 鍵の追加:

sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
Docker リポジトリの設定:

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
Docker のインストール:

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
4. Docker を sudo なしで実行できるように設定
docker グループを作成する:

sudo groupadd docker
現在のユーザーを docker グループに追加する:

sudo usermod -aG docker $USER
変更を反映するために、WSL セッションを再起動する:

exit
その後、再度 WSL にログインします。

5. Docker サービスの開始と有効化
Docker デーモンを起動し、自動起動を設定します。

Docker の起動:

sudo systemctl start docker
Docker の自動起動設定:

sudo systemctl enable docker
6. 動作確認
Docker コマンドの実行 (sudo なし):

docker --version
docker run hello-world
