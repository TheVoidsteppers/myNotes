# windows 命令

## 软链接文件夹

```powershell
mklink /D node_modules "D:\git-hub\xxx\node_modules"
# 将当前 `D:\git-hub\xxx\` 下的 node_modules 文件夹映射到 pwd 的 node_modules
# /D 创建目录符号链接而不是文件符号链接（默认为文件符号链接）
```
## autoHotKey

```powershell
# 更改上/下一桌面
LWin & PgUp::
Send {LWin Down}{Ctrl Down}{Left}{Ctrl Up}{LWin Up}
return
LWin & PgDn::
Send {LWin Down}{Ctrl Down}{Right}{Ctrl Up}{LWin Up}
return
```