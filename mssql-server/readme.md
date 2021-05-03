mssql server建立

第一步 尋找image : docker search
docker search mssql

第二步 pull image : docker pull
docker pull microsoft/mssql-server-linux

第三步 查看image：docker images
docker images

第四步 建立Container：docker run
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=密碼" -p 1433:1433 --name sql-server -d microsoft/mssql-server-linux

-e ACCEPT_EULA=Y : 設定環境中授權條款同意書 ( Y )
-e "MSSQL_SA_PASSWORD
-p 1433:1433 ：設定連線阜號，第一個是你本機的，另外一個是Container的
-d microsoft/mssql-server-linux：-d 是run container background 好了之後show container ID，後面的是IMAGE名稱

第五步 查看Container：docker ps
docker ps

CONTAINER ID   IMAGE                          COMMAND                  CREATED         STATUS         PORTS                              NAMES
eadc97201da6   microsoft/mssql-server-linux   "/opt/mssql/bin/sqls…"   2 minutes ago   Up 2 minutes   1433/tcp, 0.0.0.0:1533->1533/tcp   sql-server

第六步 執行Command在一個執行中的Container：docker exec
docker exec -it 你的Containder名稱 bash >> 【docker exec -it eadc97201da6 bash】
-i : 開啟互動介面
-t：建立一個偽終端機
bash 是因為在linux系統 如果要在windows 應該是cmd

第七步 新增sqlcmd到環境變數(PATH)中
新增sqlcmd到一個需要登入的session到Path中 
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
新增sqlcmd到一個不需要登入的session到Path中
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
二擇一，我也不知道可以幹嘛

第8步 使用sqlcmd連線到sqlserver
sqlcmd -S localhost -U SA -P '<YourPassword>' >>【sqlcmd -S localhost -U SA -P 'password'】

錯誤排除
1.確保未使用您的端口，請轉至資源監視器以進行驗證。現在檢查端口是否已保留。打開命令提示符，然後輸入
docker: Error response from daemon: 
Ports are not available: listen tcp 0.0.0.0:1433: 
bind: An attempt was made to access a socket in a way forbidden by its access permissions.

查看port號
netsh int ipv4 show excludedportrange protocol=tcp

此處列出的端口由hyper-v管理，此處刪除端口1433的唯一方法是禁用hyper-v，保留端口1433，以便hyper-v不會將其保留回去。
dism.exe /Online /Disable-Feature:Microsoft-Hyper-V
hyper-v會吃到1433，先關掉hyper-v，保留Port號再啟動hyper-v讓他配號
windows版本不一樣Hyper-V的名稱不同
dism /Online  /Get-Features  查看可用功能。
比如我的功能就叫
HypervisorPlatform && VirtualMachinePlatform
【dism.exe /Online /Disable-Feature:HypervisorPlatform】【dism.exe /Online /Disable-Feature:VirtualMachinePlatform】
或者win+R打開開啟指令optionalfeatures，進去把這兩個功能關掉
 

保留端口1433
netsh int ipv4 add excludedportrange protocol=tcp startport=1433 numberofports=1

重新啟用hyper-v
dism.exe /Online /Enable-Feature:Microsoft-Hyper-V /All

docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=password" -p 1433:1433 --name sql-server -d microsoft/mssql-server-linux