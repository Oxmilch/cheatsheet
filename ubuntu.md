https://learn.microsoft.com/ja-jp/windows/wsl/  
https://docs.docker.com/engine/install/ubuntu/  

アップデート
```bash
sudo apt update && sudo apt upgrade && sudo apt-get update
```
旧バージョンのDockerアンインストール
```bash
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
Docker公式のGPGキーの取得
```bash
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
Dockerリポジトリの登録
```bash
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
Dockerパッケージのインストール
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
Dockerインストールの確認
```bash
docker --version
```
SSHキー作成
```bash
ssh-keygen
```