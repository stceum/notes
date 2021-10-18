# windows 常用指令

## 获取当前的所有环境变量

```powershell
Get-ChildItem env:
```

## 重定向文件流

```powershell
Get-ChildItem env: >> backup_filename.txt
```

## 符号链接

Windows 10(以及通常的Powershell 5.0)允许您通过New-Item cmdlet创建符号链接.

用法:

```powershell
New-Item -Path C:\LinkDir -ItemType SymbolicLink -Value F:\RealDir
```

```powershell
function make-link ($target, $link) {
    New-Item -Path $link -ItemType SymbolicLink -Value $target
}
```

[参考](https://qa.1r1g.com/sf/ask/62610131/)