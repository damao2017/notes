# windows 右键 此处打开CMD.reg

```ini
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\Background\shell\在此处打开 CMD]
"Icon"="cmd.exe"

[HKEY_CLASSES_ROOT\Directory\Background\shell\在此处打开 CMD\command]
@="cmd.exe /s /k pushd \"%V\""
```

