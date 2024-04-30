
## システム要件
- Windows 10 Enterprise、Pro、または Education
- 第 2 レベルのアドレス変換 (SLAT) の 64 ビット プロセッサ
- VM モニター モード拡張機能 (Intel CPU 上の VT-c) の CPU サポート。
- 最小 4 GB のメモリ。
## 対応CPUか確認
コマンドプロンプトまたは、PowerShell上で以下コマンドを実行
```powershell
systeminfo
```

## 
```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```