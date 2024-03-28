# 目次
- [WSL2のインストールと有効化](#wsl2のインストールと有効化)   
　[1. Linux用 Windowsサブシステムを有効にする](#１-linux用-windowsサブシステムを有効にする)  
　[2. 仮想マシンの機能を有効にする](#２-仮想マシンの機能を有効にする)  
　[3. Linuxカーネル更新プログラムパッケージをインストールする](#３-linuxカーネル更新プログラムパッケージをインストールする)  
　[4. WSL2を既定のバージョンとして設定する](#４-wsl2を既定のバージョンとして設定する)  
　[5. インストール可能な一覧](#5-インストール可能な一覧)  
　[6. Linuxのインストール(Debianの場合)](#6-linuxのインストールdebianの場合)  
　[7. WSLの仮想環境設定](#7-wslの仮想環境設定)  
　　[- wslconfigパラメータ説明](#wslconfigパラメータ説明)  
　　[- wslconfigパラメータ内容](#wslconfigパラメータ内容)  
- [WSLコマンド](#wslコマンド)  
- [WSL2のアンインストールと無効化](#wsl2のアンインストールと無効化)

# WSL2のインストールと有効化
- [Microsoft Docs - WSLの手動インストール手順 Win10 v2004以降, win11](https://learn.microsoft.com/ja-jp/windows/wsl/install)  
- [Microsoft Docs - WSLの手動インストール手順 旧バージョン](https://docs.microsoft.com/ja-jp/windows/wsl/install-manual)  
> ※PowerShellを管理者で実行すること

Windows 10 v2004以降、またはWindows 11 の場合は、手順 5から実施でOK。

## １. Linux用 Windowsサブシステムを有効にする  
```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

## ２. 仮想マシンの機能を有効にする  
```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

## ３. Linuxカーネル更新プログラムパッケージをインストールする  
[Microsoft Docs - x64マシン用WSL2Linux カーネル更新プログラム（wsl_update_x64.msi）](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)  

## ４. WSL2を既定のバージョンとして設定する  
```powershell
wsl --set-default-version 2
```

## 5. インストール可能な一覧
```powershell
wsl --list --online
```
※ 2024年 3月現在
| NAME | FRENDLY NAME |
|-|-|
|Ubuntu|Ubuntu|
|Debian|Debian GNU/Linux|
|kali-linux|kali Linux Rolling|
|Ubuntu-18.04|Ubuntu 18.04 LTS|
|Ubuntu-20.04|Ubuntu 20.04 LTS|
|Ubuntu-22.04|Ubuntu 22.04 LTS|
|OracleLinux_7_9|OracleLinux 7.9|
|OracleLinux_8_7|OracleLinux 8.7|
|OracleLinux_9_1|OracleLinux 9.1|
|openSUSE-Leap-15.5|openSUSE Leap 15.5|
|SUSE-Linux-Enterprise-Server-15-SP4|SUSE Linux Enterprise Server 15 SP4|
|SUSE-Linux-Enterprise-15-SP5|SUSE Linux Enterprise 15 SP5|
|openSUSE-Tumbleweed|openSUSE Tumbleweed|

## 6. Linuxのインストール(Debianの場合)  
１～４までの手順を飛ばした場合は、こちらのコマンドから実行する
```powershell
wsl --install
```
ディストリビュージョンのインストール
```powershell
wsl --install -d Debian
```

## 7. WSLの仮想環境設定  
[MicroSoft Docs - WSLでの詳細設定の構成](https://docs.microsoft.com/ja-jp/windows/wsl/wsl-config)  

WSL2 で実行されているインストール済みディストリビューション全体で設定をグローバルに構成する場合は、.wslconfig ファイルを %USERPROFILE% に作成し、各パラメータを記述する。  
> ファイルパス：%USERPROFILE%.wslconfig  
### wslconfigパラメータ説明
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

### wslconfigパラメータ内容
```bash
[wsl2]
# memory=4GB
# processors=4
# kernel=
# kernelCommandLine =
# swap=2GB
# swapfile=%USERPROFILE%\\AppData\\Local\\Temp\\swap.vhdx
# pageReporting=true
# localhostforwarding=true
# nestedVirtualization=false
# Disables nested virtualization
# nestedVirtualization=false
# debugConsole=false
# vmIdleTimeout=60000
```

# WSLコマンド
[Microsoft Docs - WSLの基本的なコマンド](https://docs.microsoft.com/ja-jp/windows/wsl/basic-commands)  

# WSL2のアンインストールと無効化
正常にインストールできない場合は次の手順を踏んでアンインストールする。
- 削除するアプリケーション
  - Windowsの設定 -> アプリ -> インストールされているアプリ
    - Linux用Windowsサブシステム
    - インストールしたLinuxのディストリビュージョン

- 無効化する機能
  - スタート -> Windowsのアプリ一覧 -> Windowsツール -> コントロールパネル -> プログラムと機能 -> Windowsの機能の有効化または無効化
    - Linux用Windowsサブシステム
    - 仮想マシンプラットフォーム

# WSL2でsystemctlを有効化する
[Qiita - WSL2でsystemctlを使う方法](https://qiita.com/curacao/items/fb9adaf1c097b1acd6a8)  
標準でsystemdがPID=1で起動されないため、WSL起動時にPID=1になるように設定する  
以下ファイルを開く
```bash
sudo vi /etc/wsl.conf
```
wsl.confの内容は以下の通りにする
```bash
[boot]
systemd=ture
```
変更が終わったら、WSL2を再起動する