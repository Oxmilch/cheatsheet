
## **ファイル操作** 
### ファイル名を変更する 
```shell
git mv "変更前のファイル名" "変更後のファイル名"
```
### ファイルを移動する  
```shell
git mv "移動する前のファイルパスとファイル名" "移動した後のファイルパスとファイル名"
```

## **ブランチ操作**
### チェックアウトしているブランチに、別のブランチをマージする  
```shell
git merge "マージするブランチ名"
```

## **初期設定**
### 設定値確認  
```shell
git config --global -l
```
### ユーザー名変更  
```shell
git config --global user.name "ユーザー名"
```
### メールアドレス名変更  
```shell
git config --global user.email "メールアドレス"
```

## githubと連携する
### SSHキー作成
[GitHubDocs - 新しい SSH キーを生成して ssh-agent に追加する](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
```shell
# ED25519 (推奨)
ssh-keygen -t ed25519 -C "your_email@example.com"

# RSA (使用可、ED25519に非対応端末の場合)
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

### SSHキーをgithubに登録
[GitHubDocs - GitHub アカウントへの新しい SSH キーの追加](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
