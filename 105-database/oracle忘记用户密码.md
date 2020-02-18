### oracle忘记用户密码

cmd打开windows窗口，使用sqlplus操作

```bash
sqlplus /nolog;
conn /as sysdba;
alert user ntkd identified by newpassword;
```