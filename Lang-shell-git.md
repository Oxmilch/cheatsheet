
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