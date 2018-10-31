# windows

## 获取整个内存
1. dd命令，无法在winServer2003以上版本使用  Helix提供图形界面
2. nigilant程序 tool菜单栏 
3. 远程获取  ProDiscoverIP(远程系统需要运行servlet程序) OnlineDFS/liveWire
4. windows较新版本（winServer2003后）无法直接之访问/Device/PhysicalMemory, 需要编写定制的内核驱动程序保证内存获取工具正常访问物理内存

## 时间相关工具
1. date /t
2. time /t
3. 编程实现

## 系统信息
1. hostname 目标系统名称
2. whoami 当前系统用户信息
3. ver 获得操作系统环境
4. ipconfig 获取网络信息 常用选项/all /displaydns
5. iplist显示网络接口信息
6. dumpwin

## 网络配置
1. Promiscdetect  promqry 需要.net框架支持 显示网卡的运行模式
2. URLprotocolView 识别系统中被激活的协议
3. 检查网络连接情况（端口，协议，地址），DNS请求，目标系统的NetBIOS名字表,ARP缓存，内部路由表等
4. netstat 常用选项 -ano
5. nbtstat 显示协议统计和当前使用 NBI 的 TCP/IP 连接 常用选项-S检查NetBIOS缓存
6. net 需要管理员权限
7. arp 显示arp协议使用的物理地址到ip地址转换表

## 登陆用户
1. psloggedon
2. quser 在windows server中提供 显示用户名，登陆时间、地点，session类别状态
3. netuser目标系统中本地登陆账户的详细信息
4. logonsession 需要管理员权限，

## 进程
1. 需要搜集信息： 进程名和PID，临时上下文，内存消耗，可执行程序镜像的进程，用户镜像的进程，子进程，调用库和依赖库，用来创建进程的命令行参数，相关联的句柄，进程的内存内容，与系统状态和残留的环境相关的上下文环境
2. tlist window debugging工具 收集活动进程的简单信息
3. PRCView 显示当前进程信息 附带pv。pv -e获取进程对应文件位置
4. pslist 识别程序运行时间，文件名，对应PID等
5. query 系列命令 在较新的win server可以使用
6. tasklist 显示所有运行进程的相关信息 -v选项获得与进程所属用户信息
7. currProcess输出当前进程的文件的详细信息，用法CurrProcess.exe /stext path
8. cmdline（diamondCS）获取进程启动时获取的命令行参数 
9. 获取进程打开的句柄 handles， open handle（都没有找到）process explorer
10. 进程打开的dll listdll, PrcView,tasklist等工具
11. 检查进程所关联的可执行文件，及确定