# Windows11 右键恢复原始菜单

### Win11切换经典右键菜单

右下角搜索窗口搜索CMD，以管理员身份运行CMD

输入以下命令：

```cmd
reg add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve
taskkill /f /im explorer.exe & start explorer.exe
```

### Win11恢复回新右键菜单

右下角搜索窗口搜索CMD，以管理员身份运行CMD

输入以下命令：

```
reg delete "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f
taskkill /f /im explorer.exe & start explorer.exe 
```
