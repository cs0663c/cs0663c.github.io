#wireguard客户端先安装
#可选安装Tunnel Service服务的形式运行
命令如下：配置文件和路径自行修改
```shell
wireguard /installtunnelservice C:\Users\HSSL\Documents\WireGuard\B75.conf```
##******以下是动态域名的小技巧******
首先前提时wireguard服务时部署在家里的动态公网主机上使用动态域名连接对端
否则下面这些纯属白瞎搞 有固定公网的服务器无此烦恼
因为现在的客户端配置中第一次运行配置文件会解析对端域名换成公网ip 并不会实时监控ip是否变化
一旦服务端ip变更便失联
现在来个小技巧
###先运行一次配置文件确保连接正常
```shell
wireguard /installtunnelservice C:\Users\HSSL\Documents\WireGuard\B75.conf```
运行服务services.msc找到服务WireGuard Tunnel: B75" 复制下来
我们需要设置的就时让这任务定时重启 重启时它会重新解析ip 这样就曲线解决了
新建一个bat
```shell
@echo off
net stop "WireGuard Tunnel: B75"
net start "WireGuard Tunnel: B75"```
就一个关和开的命令 这需要管理员权限 记得
然后你要设置计划任务让其定时运行或者开机运行,这都是后话了,
如果你有更好更简单的方式欢迎指教

下面的是我自己的bat,花里胡哨仅供参考
```shell
@echo off
setlocal enabledelayedexpansion

@echo %date% %time% - 服务重启流程开始  >> "%USERPROFILE%\Documents\WireGuard\restart_service_log.txt"
@echo %date% %time% - 尝试停止服务 "WireGuard Tunnel: H510HP"  >> "%USERPROFILE%\Documents\WireGuard\restart_service_log.txt"
net stop "WireGuard Tunnel: H510HP" 2>> "%USERPROFILE%\Documents\WireGuard\restart_service_log.txt"
if %errorlevel% neq 0 (
     @echo %date% %time% - 停止服务失败，错误代码为 %errorlevel%  >> "%USERPROFILE%\Documents\WireGuard\restart_service_log.txt"
) else (
     @echo %date% %time% - 服务成功停止  >> "%USERPROFILE%\Documents\WireGuard\restart_service_log.txt"
     @echo %date% %time% - 尝试启动服务 "WireGuard Tunnel: H510HP"  >> "%USERPROFILE%\Documents\WireGuard\restart_service_log.txt"
     net start "WireGuard Tunnel: H510HP" 2>> "%USERPROFILE%\Documents\WireGuard\restart_service_log.txt"
     if %errorlevel% neq 0 (
          @echo %date% %time% - 启动服务失败，错误代码为 %errorlevel%  >> "%USERPROFILE%\Documents\WireGuard\restart_service_log.txt"
     ) else (
          @echo %date% %time% - 服务成功启动  >> "%USERPROFILE%\Documents\WireGuard\restart_service_log.txt"
     )
)

@echo %date% %time% - 尝试查询服务状态  >> "%USERPROFILE%\Documents\WireGuard\restart_service_log.txt"
net start "WireGuard Tunnel: H510HP" 2>> "%USERPROFILE%\Documents\WireGuard\restart_service_log.txt"

@echo %date% %time% - 服务重启操作完成  >> "%USERPROFILE%\Documents\WireGuard\restart_service_log.txt"
```