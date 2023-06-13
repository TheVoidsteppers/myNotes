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

## Navicate

Navicate MongoDB V16 无限试用期

```powershell
@echo off

echo Delete HKEY_CURRENT_USER\Software\PremiumSoft\NavicatMONGODB\Registration16XCS
for /f %%i in ('"REG QUERY "HKEY_CURRENT_USER\Software\PremiumSoft\NavicatMONGODB" /s | findstr /L Registration"') do (
    reg delete %%i /va /f
)
echo.

echo Delete Info folder under HKEY_CURRENT_USER\Software\Classes\CLSID
for /f %%i in ('"REG QUERY "HKEY_CURRENT_USER\Software\Classes\CLSID" /s | findstr /E Info"') do (
    reg delete %%i /va /f
)
echo.

echo Finish

pause
```