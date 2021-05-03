# **docker create ms sql-server**

- 尋找image: 
```
docker search mssql
```
- 下載image(這邊使用linux mssql-server):
```
docker pull microsoft/mssql-server-linux
```
- 查看image
```
docker images
```

- 建立Container：
```
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=密碼" -p 1433:1433 --name sql-server -d microsoft/mssql-server-linux
```
	 -e ACCEPT_EULA=Y : 設定環境中授權條款同意書(Y)
	 -e MSSQL_SA_PASSWORD : 密碼
	 -p 1433:1433 ：設定連線阜號，第一個是你本機的，另外一個是Container的
	 -d microsoft/mssql-server-linux：-d 是run container background 好了之後show container ID，後面的是IMAGE名稱

- 查看Container：
```
docker ps
```

- 執行Command在一個執行中的Container：
```
docker exec -it eadc97201da6 bash
```
	-i : 開啟互動介面
	-t：建立一個偽終端機
	bash 是因為在linux系統 如果要在windows 應該是cmd

- 新增sqlcmd到環境變數(PATH)中
```
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
```

- 使用sqlcmd連線到sqlserver
```
sqlcmd -S localhost -U SA -P 'password'
```

___

# **錯誤排除**
```
docker: Error response from daemon: 
Ports are not available: listen tcp 0.0.0.0:1433: 
bind: An attempt was made to access a socket in a way forbidden by its access permissions.
```

## hyper-v會吃到1433，先關掉hyper-v，保留Port號再啟動hyper-v讓他配號

- 查看port號指令
```
netsh int ipv4 show excludedportrange protocol=tcp
```

######查看可用功能。
```
dism /Online  /Get-Features 
```
###### windows版本不一樣Hyper-V的名稱不同，比如我的功能就叫 HypervisorPlatform && VirtualMachinePlatform
```
dism.exe /Online /Disable-Feature:HypervisorPlatform
dism.exe /Online /Disable-Feature:VirtualMachinePlatform
```
###### 或者win+R打開開啟指令optionalfeatures，進去把這兩個功能關掉
 

- 保留端口1433
```
netsh int ipv4 add excludedportrange protocol=tcp startport=1433 numberofports=1
```

- 重新啟用hyper-v
```
dism.exe /Online /Enable-Feature:Microsoft-Hyper-V /All
```