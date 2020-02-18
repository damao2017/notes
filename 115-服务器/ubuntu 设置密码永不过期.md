# ubuntu 设置密码永不过期

```bash
Usage: chage [options] [LOGIN]

Options:
  -d, --lastday LAST_DAY        set date of last password change to LAST_DAY
  -E, --expiredate EXPIRE_DATE  set account expiration date to EXPIRE_DATE
  -h, --help                    display this help message and exit
  -I, --inactive INACTIVE       set password inactive after expiration
                                to INACTIVE
  -l, --list                    show account aging information
  -m, --mindays MIN_DAYS        set minimum number of days before password
                                change to MIN_DAYS
  -M, --maxdays MAX_DAYS        set maximim number of days before password
                                change to MAX_DAYS
  -W, --warndays WARN_DAYS      set expiration warning days to WARN_DAYS
  
  
root@JYHPT:~# chage -l ntkd
Last password change			            		: Aug 22, 2019
Password expires				                	: Nov 20, 2019
Password inactive				                  	: never
Account expires					                 	: never
Minimum number of days between password change		: 6
Maximum number of days between password change		: 90
Number of days of warning before password expires	: 30

#修改
root@JYHPT:~# chage ntkd -M -1


root@JYHPT:~# chage -l ntkd
Last password change				            	: Aug 22, 2019
Password expires					                : never
Password inactive				                	: never
Account expires					                	: never
Minimum number of days between password change		: 6
Maximum number of days between password change		: -1
Number of days of warning before password expires	: 30
```

