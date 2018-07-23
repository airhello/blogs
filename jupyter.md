# jupyter使用说明

## 安装命令
> apt-get install python3  
> pip3 install jupyter  

## 配置文件
root@ubuntu:~# cat /usr/local/etc/jupyter/jupyter_notebook_config.py  
> c.NotebookApp.notebook_dir = '/home/wudan/jupyter-notebook'  
> c.NotebookApp.ip = '*'  
> c.NotebookApp.open_browser = False  
> c.NotebookApp.token = 'wudan' # 用户名  
> c.NotebookApp.password = 'neteye' # 密码

## 开机启动
root@ubuntu:~# cat /usr/lib/systemd/system/jupyter.service 
> [Unit]  
> Description=jupyter  
>   
> [Service]  
> Type=simple  
> PIDFile=/var/run/jupyter.pid  
> ExecStart=/usr/local/bin/jupyter-notebook --ip=0.0.0.0 --no-browser --config=/usr/local/etc/jupyter/jupyter_notebook_config.py --allow-root  
>   
> [Install]  
> WantedBy=multi-user.target  
>   
>   

 systemctl daemon-reload  
 systemctl enable jupyter  
   
 service jupyter start
