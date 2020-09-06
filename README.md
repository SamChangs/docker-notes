# Docker解決的問題
1.裝service時因為作業系統、kernel、環境變數設定不正確，造成service無法成功運行，在trial and error上花費太多時間

2.測試code在不同service下能否正常運行，若將service裝在自己的作業系統切換版本時需反覆安裝和卸載，易造成舊安裝檔未卸載完全，導致service無法正常運行

3.開發open sourcr專案，希望有簡單的方法給多人使用，可以把開發完成式包成docker images放到docker hub供人下載，只需打幾行commaand就能執行

# Docker與Vm差別
1.**Vm:**
- 需安裝作業系統
- vm作業系統開機需等待數秒
- 硬體系統資源完全隔離
- 暫硬碟容量較大

2.**Docker:**
- 可直從Docker hub下載作業系統
- 不需開機，啟動速度比vm快
- 底層使用作業系統kernel
- 暫硬碟容量小

# Docker基底技術:
1. Namespace:作環境隔離

2. Cgroup:系統資源管理(cpu,memory,I/O)

 **==兩者並存後，能做到在同個kernel的環境下有下隔離資源使用==**


# Docker系統架構
1. Docker Daemon:
    用來執行Docker內環境 ex:Docker Images, Container

2. Docker Cline:
    使用 Restful API 連到 Docker daemonm,提供 command line 的方式讓使用者可以操作docker

# Docker常見名詞
1. Docker Images:
    為Docker映像檔案且為唯讀，service,mysql,application各別是一塊積木,可向堆疊積木一樣堆積起來
    1-2. Docker images從哪裡來:
   - 從Docker hub pull下來
   - 從另一台電腦export後再import到自己電腦
   - 撰寫Docler file

2. Docker Container:
    是透過docker images執行出來的process，同一個docker images可開啟多個container
    2-1.==docker container的環境是彼此隔離的，不會發生container1開8080port，container2開8080port有衝突到的問題==
    
3. Docker Hub:
    可想像為github，用來存放程式碼的倉庫，可使用網路公開的 Docker Hub，或在自己內部架設私有Docker Registry
    
# 安裝和啟動Docker(CentOS 7.3)
:::info
Windows 需使用docker toolbox
教學網址:https://blog.csdn.net/xbinworld/article/details/78945879

docker hub:https://hub.docker.com/editions/community/docker-ce-desktop-windows
:::

一，安裝準備

1.  Docker 會呼叫到 Linux Kernel 的 Namespace 和 Cgroup，需確認正確版本的 Linux Kerenel，不然 Docker 的 service 會啟動不起

 - 查看作業系統版本
  `$ lsb_release -a`
  - 查看 Kernel 版本
  `$ uname -a`
 2. 安裝docker
 - 使用的指令
  `yum install -y docker`
 - 啟動 docker 
 `systemctl start docker`
 - 查看版本
 ` docker version`
 
二，root 使用者權限

  - 修改 /etc/docker/daemon.json 的檔案，如果沒有此檔案直接建立新的檔案，指令如下：
  `Vi /etc/docker/daemon.json`
  檔案內容如下：
{
"live-restore": true,
"group": "dockerroot"
}
 
 - 使用 root 權限把 user1 指令加入到 dockerroot 的 group
 `usermod -aG dockerroot user1`
 - 重新啟動
 `systemctl restart docker`