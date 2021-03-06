
# cmdlist

## NGINX 

```
   nginx 
   nginx -s quit           优雅停止nginx，有连接时会等连接请求完成再杀死worker进程  
   nginx -s reload         优雅重启，并重新载入配置文件nginx.conf
   nginx -s reopen         重新打开日志文件，一般用于切割日志
   nginx -v                查看版本  
   nginx -t                检查nginx的配置文件
   nginx -h                查看帮助信息
 　nginx -V                详细版本信息，包括编译参数 
   nginx  -c filename      指定配置文件
   
   # 直接点击Nginx目录下的   nginx.exe    
   start nginx
   nginx -s stop           stop表示立即停止nginx,不保存相关信息
   nginx -s quit           quit表示正常退出nginx,并保存相关信息
   nginx -s reload
   # 重启(因为改变了配置,需要重启)
```

## windows
#### 查找进程

**查看进程包含字符串'nginx'的进程列表**
```
> tasklist|findstr "nginx*"
```
**查看名称为nginx.exe的进程列表**
```
> tasklist /fi "imagename eq nginx.exe"
```
**查看占用8080端口的进程号**
```
> netstat –aon | findstr “8080”
```
**tasklist 命令详解**
```
TASKLIST [/S system [/U username [/P [password]]]]
         [/M [module] | /SVC | /V] [/FI filter] [/FO format] [/NH]
```

描述:该工具显示在本地或远程机器上当前运行的进程列表。
参数列表:
```
   /S     system           指定连接到的远程系统。
   /U     [domain\]user    指定应该在哪个用户上下文执行这个命令。
   /P     [password]       为提供的用户上下文指定密码。如果省略，则
                           提示输入。
   /M     [module]         列出当前使用所给 exe/dll 名称的所有任务。
                           如果没有指定模块名称，显示所有加载的模块。
   /SVC                    显示每个进程中主持的服务。
   /APPS 显示 Microsoft Store 应用及其关联的进程。
   /V                      显示详细任务信息。
   /FI    filter           显示一系列符合筛选器
                           指定条件的任务。
   /FO    format           指定输出格式。
                           有效值: "TABLE"、"LIST"、"CSV"。
   /NH                     指定列标题不应该
                           在输出中显示。
                           只对 "TABLE" 和 "CSV" 格式有效。
   /?                      显示此帮助消息。
```

筛选器:
```
    筛选器名称     有效运算符           有效值
    -----------     ---------------           --------------------------
    STATUS          eq, ne                    RUNNING | SUSPENDED
                                              NOT RESPONDING | UNKNOWN
    IMAGENAME       eq, ne                    映像名称
    PID             eq, ne, gt, lt, ge, le    PID 值
    SESSION         eq, ne, gt, lt, ge, le    会话编号
    SESSIONNAME     eq, ne                    会话名称
    CPUTIME         eq, ne, gt, lt, ge, le    CPU 时间，格式为
                                              hh:mm:ss。
                                              hh - 小时，
                                              mm - 分钟，ss - 秒
    MEMUSAGE        eq, ne, gt, lt, ge, le    内存使用(以 KB 为单位)
    USERNAME        eq, ne                    用户名，格式为
                                              [域\]用户
    SERVICES        eq, ne                    服务名称
    WINDOWTITLE     eq, ne                    窗口标题
    模块         eq, ne                    DLL 名称
```
注意: 当查询远程计算机时，不支持 "WINDOWTITLE" 和 "STATUS"
      筛选器。
```
Examples:
    TASKLIST
    TASKLIST /M
    TASKLIST /V /FO CSV
    TASKLIST /SVC /FO LIST
    TASKLIST /APPS /FI "STATUS eq RUNNING"
    TASKLIST /M wbem*
    TASKLIST /S system /FO LIST
    TASKLIST /S system /U 域\用户名 /FO CSV /NH
    TASKLIST /S system /U username /P password /FO TABLE /NH
    TASKLIST /FI "USERNAME ne NT AUTHORITY\SYSTEM" /FI "STATUS eq running"
```

#### 关闭进程

```
taskkill /f /t /im               进程名称
taskkill /f /t /im /javaw.exe

```

#### taskkill help
```
TASKKILL [/S system [/U username [/P [password]]]]
         { [/FI filter] [/PID processid | /IM imagename] } [/T] [/F]

描述:
    使用该工具按照进程 ID (PID) 或映像名称终止任务。

参数列表:
    /S    system           指定要连接的远程系统。

    /U    [domain\]user    指定应该在哪个用户上下文执行这个命令。

    /P    [password]       为提供的用户上下文指定密码。如果忽略，提示
                           输入。

    /FI   filter           应用筛选器以选择一组任务。
                           允许使用 "*"。例如，映像名称 eq acme*

    /PID  processid        指定要终止的进程的 PID。
                           使用 TaskList 取得 PID。

    /IM   imagename        指定要终止的进程的映像名称。通配符 '*'可用来
                           指定所有任务或映像名称。

    /T                     终止指定的进程和由它启用的子进程。

    /F                     指定强制终止进程。

    /?                     显示帮助消息。

筛选器:
    筛选器名      有效运算符                有效值
    -----------   ---------------           -------------------------
    STATUS        eq, ne                    RUNNING |
                                            NOT RESPONDING | UNKNOWN
    IMAGENAME     eq, ne                    映像名称
    PID           eq, ne, gt, lt, ge, le    PID 值
    SESSION       eq, ne, gt, lt, ge, le    会话编号。
    CPUTIME       eq, ne, gt, lt, ge, le    CPU 时间，格式为
                                            hh:mm:ss。
                                            hh - 时，
                                            mm - 分，ss - 秒
    MEMUSAGE      eq, ne, gt, lt, ge, le    内存使用量，单位为 KB
    USERNAME      eq, ne                    用户名，格式为 [domain\]user
    MODULES       eq, ne                    DLL 名称
    SERVICES      eq, ne                    服务名称
    WINDOWTITLE   eq, ne                    窗口标题

    说明
    ----
    1) 只有在应用筛选器的情况下，/IM 切换才能使用通配符 '*'。
    2) 远程进程总是要强行 (/F) 终止。
    3) 当指定远程机器时，不支持 "WINDOWTITLE" 和 "STATUS" 筛选器。

例如:
    TASKKILL /IM notepad.exe
    TASKKILL /PID 1230 /PID 1241 /PID 1253 /T
    TASKKILL /F /IM cmd.exe /T
    TASKKILL /F /FI "PID ge 1000" /FI "WINDOWTITLE ne untitle*"
    TASKKILL /F /FI "USERNAME eq NT AUTHORITY\SYSTEM" /IM notepad.exe
    TASKKILL /S system /U 域\用户名 /FI "用户名 ne NT*" /IM *
    TASKKILL /S system /U username /P password /FI "IMAGENAME eq note*"
```

```
TSKILL processid | processname [/SERVER:servername] [/ID:sessionid | /A] [/V]

  processid           要结束的进程的 Process ID。
  processname         要结束的进程名称。
  /SERVER:servername  含有 processID 的服务器(默认值是当前值)。
                         使用进程名和 /SERVER 时，必须指定 /ID
                         或 /A
  /ID:sessionid       结束在指定会话下运行的进程。
  /A                  结束在所有会话下运行的进程。
  /V                  显示正在执行的操作的信息。
```

## linux

### 查找进程
```
ps -ef |grep nig

Usage: ps [-aefls] [-u UID] [-p PID]

Report process status

 -a, --all       show processes of all users
 -e, --everyone  show processes of all users
 -f, --full      show process uids, ppids
 -h, --help      output usage information and exit
 -l, --long      show process uids, ppids, pgids, winpids
 -p, --process   show information for specified PID
 -s, --summary   show process summary
 -u, --user      list processes owned by UID
 -V, --version   output version information and exit
 -W, --windows   show windows as well as cygwin processes

With no options, ps outputs the long format by default
```

