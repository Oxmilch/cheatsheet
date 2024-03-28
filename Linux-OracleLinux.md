## アップデート
[Oracle Help Center - Oracle LinuxでDNFを使用 - パッケージのインストール、ダウンロードおよび再インストール
](https://docs.oracle.com/ja/learn/use_dnf_on_oracle_8/#install-download-and-reinstall-packages)
```bash
sudo dnf upgrade
```
## Dockerのインストール
[OracleBase - Docker : Oracle Linux 8 (OL8)へのDockerのインストール](https://oracle-base.com/articles/linux/docker-install-docker-on-oracle-linux-ol8)  

※WSL2 の systemdがPID=1で実行する設定が完了していること
### Dockerに必要なモジュールのインストール
```bash
sudo dnf install -y dnf-utils zip unzip
```
### Dockerリポジトリの追加
```bash
sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
```
### Dockerパッケージのインストール
```bash
sudo dnf install -y docker-ce --nobest
```

### Dockerサービスの登録
```bash
sudo systemctl start docker
sudo systemctl start docker.service
```
### Dockerサービスの常駐
```bash
sudo systemctl enable docker
```
### プロキシサーバー環境がある場合の追加設定
[GODAI BLOG - 【Docker】社内プロキシ環境下でDocker Hubからイメージをpullしようとすると失敗する](https://www.godaiblog.com/entry/2018/12/17/【Docker】社内プロキシ環境下でDocker_Hubからイメージをpullし)
```bash
# プロセスに対して環境変数を設定
# ディレクトリの作成
sudo mkdir /etc/systemd/system/docker.service.d

# 下記コマンドを実行し、帰ってくるIPアドレスをメモ
cat /etc/resolv.conf | grep nameserver | awk '{print $2}'

# ファイルを新規作成
sudi vi /etc/systemd/system/docker.service.d/http-proxy.conf
```
http-proxy.conf ファイルの内容は下記の通り
```bash
[service]
Environment="HTTP_PROXY=http://{実行して帰ってくるIPアドレス}:プロキシサーバーのポート番号"
Environment="HTTPS_PROXY=http://{実行して帰ってくるIPアドレス}:プロキシサーバーのポート番号"
```
保存して WSL2 を再起動する