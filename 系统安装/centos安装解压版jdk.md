# centos安装解压版jdk

## 参考

https://blog.csdn.net/youngking520/article/details/104615256





## 使用xftp软件先将jdk传输到linux主机中

```
su
cp  jdk-8u291-linux-x64.tar.gz  /usr/local
tar -zxvf   /usr/local/jdk-8u291-linux-x64.tar.gz 





gedit /etc/profile
#在文件末尾输入
export JAVA_HOME=/usr/local/jdk1.8.0_291
export JRE_HOME=${JAVA_HOME}/jre
export JAVA_PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin
export PATH=$PATH:${JAVA_PATH}
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar






export JAVA_HOME=/home/sen/jdk1.8.0_291
export JRE_HOME=${JAVA_HOME}/jre
export JAVA_PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin
export PATH=$PATH:${JAVA_PATH}
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar



#配置文件生效
source /etc/profile

# 查看path
echo $PATH

chmod +x /home/sen/jdk1.8.0_291/bin/*
chmod +x /home/sen/jdk1.8.0_291/jre/bin/*
```

