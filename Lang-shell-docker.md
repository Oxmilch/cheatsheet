## **環境設定**
### Windows10 HomeEdition
１. WSL2 のインストールと有効化  
[WSL の手動インストール手順](https://docs.microsoft.com/ja-jp/windows/wsl/install-manual)  
> ※PowerShellを管理者で実行すること

１-１. Linux 用 Windows サブシステムを有効にする  
```shell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

１-２. 仮想マシンの機能を有効にする  
```shell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

１-３. Linuxカーネル更新プログラムパッケージをインストールする  
[x64マシン用WSL2Linux カーネル更新プログラム（wsl_update_x64.msi）](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)  

１-４. WSL2を既定のバージョンとして設定する  
```shell
wsl --set-default-version 2
```

１-５. Linuxのインストール(Debian)  
```shell
wsl --install -d Debian
```
２. WSLの仮想環境設定  
[WSLでの詳細設定の構成](https://docs.microsoft.com/ja-jp/windows/wsl/wsl-config)  

WSL2 で実行されているインストール済みディストリビューション全体で設定をグローバルに構成する場合は、.wslconfig ファイルを %USERPROFILE% に作成し、各パラメータを記述する。  
> ファイルパス：%USERPROFILE%.wslconfig  

| 設定 | 規定値| 説明 |
| - | :-: | - |
| memory | Windowsの物理メモリの50%or8GBで少ない方 | WSL2に割り当てるメモリ量。MB,GBで指定。 |
| processors | WindowsのCPU数 | WSL2に割り当てるCPU数 |
| kernel |  | Linuxカーネルへの絶対パス(Windows上)。 |
| kernelCommandLine | 空白 | カーネルコマンドライン。 |
| swap | Windowsの物理メモリの25% | WSL2のスワップ領域。 |
| swapfile | %USERPROFILE%\AppData\Local\Temp\swap.vhdx | スワップファイルの生成ディレクトリ。 |
| pageReporting | true | WSL2に割り当てられた未使用メモリの再利用可否。 |
| localhostforwarding | true | WSL2でlocalhostにバインドされたポートに、localhost:port経由でホストから接続できるかどうかを指定。 |
| nestedVirtualization | true | Windows11のみ。入れ子になった仮想化で他の入れ子になったWSL2の実行可否。 |
| debugConsole | false | Windows11のみ。WSL2のインスタンスの開始時にdmesgの内容を表示する出力コンソールウィンドウの有無。 |
| vmIdleTimeout | 60000 | Windows11のみ。WSL2ががアイドル状態になってからシャットダウンされるまでのミリ秒数。 |

```config
[wsl2]
# memory=4GB
# processors=4
# kernel=C:\\temp\\myCustomKernel
# kernelCommandLine =
# swap=2GB
# swapfile=%USERPROFILE%\AppData\Local\Temp\swap.vhdx
# pageReporting=true
# localhostforwarding=true
# nestedVirtualization=false
# Disables nested virtualization
# nestedVirtualization=false
# debugConsole=false
# vmIdleTimeout=60000
```

３. Docker Desktop for Windows のインストール  

### macOS IntelCPU
１. Docker Desktop for Mac のインストール  
[MacでDockerをインストールした後に必要な作業](https://qiita.com/butada/items/4044c5efd03341c8afef)  


## **初期設定**  
### DockerHubログイン  
```shell
docker login
Username: "DockerHubのログインユーザーID名"
Password: "DockerHubのログインパスワード"
```