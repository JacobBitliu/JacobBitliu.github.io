---
layout:     default
title:      MarkDown Syntax
category: blog
description: Mac 10.12 established javaweb development enviroument
---

#Tools

  Before begin javaweb develop , we should get the tools what we need first.
  The tools we need can be devide into two group . First is server the second is ide we decide to use.
  In this tutorial , you can learn how to establish Tomcat server in your mac , and how to use these tools with Intellj idea .
 
##1. Get the JDK

###[JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

  Clicking the title ,and jumping to download site of jdk . Chose the one you want and download it .
  I introduce you to download the dmg file to install jdk as after completing the install , you will see the java preference is already show in your System Preference Setting . Just like the figure next .

![]({{site.url}}/assets/SystemPreference.png)

  It is convenient that you can do many things through java preference like auto-launch when your computer start.
  
  After intall the dmg file , use "java -version" and "javac" command to test if you hava install it will .
  
  If you want to know where is your jdk after install , use the command next.
```
/usr/libexec/java_home
```

  After using this command , you can get the jdk path (which also the JAVA_HOME environment variables). You can also use this command to set the path in your ide what ever Eclipse or Intellj . 
  
  

##2. Prepare the Server

###[Tomcat](http://tomcat.apache.org)
 
  Clicking the title above and jumping to Official Website of Tomcat then chose the tomcat server you want to download. Then you have finished our first step . 
  ![]({{site.url}}/assets/DownloadTomcat.png)
  ![]({{site.url}}/assets/TomcatFile.png)
  Download these files like the figure above. These include doc for Tomcat and Tomcat server.
  
  Then move the tomcat server to "/usr/local" and try start server.
  
  (You can find the mothed about how to start the server from next figure.)
  
  ![]({{site.url}}/assets/StartTomcat.png)
  
  If you successfully start the tomcat, just begin next step.
 
###[MySql](https://www.mysql.com)

  Clicking the title above and jumping to Official Website of MySql
then download the dmg file you need .
  I introduce dmg file , too. The reason just the same as the jdk.
  
  In the first picture you can see the MySql preference , but for your convenient , I put it here too.
  
![]({{site.url}}/assets/SystemPreference.png)

  After download it you will hava to test it .
  
```
sudo /usr/local/mysql/support-files/mysql.server start
```
  
  Please remember the "sudo" , it's very important.
 
```
sudo /usr/local/mysql/support-files/mysql.server stop
```
  Use the command above to stop mysql .
  
  Here I can show you the path to mysql.server file.
  
  ![]({{site.url}}/assets/mysqlserver.png)
  
  If you face a error which say something about pid , which usually appear , you have to know the key always lies in you don's the privilege. So , be sure you use the "sudo" or you already change to root .
  
  After you see "SUCCESS" , we go next step.
  
  Runing the command next :
  
```
/usr/local/mysql/bin/mysql
```

  If you see
> Access denied for user 'Dantrsey'@'localhost' (using password: NO)

  use the "sudo" , and may be you need use the command downstairs :
```
/usr/local/mysql/bin/mysql -u root -p
```
  then entry the default password which you get affter install the dmg. If you forget it , run the next command :
  
```
sudo /usr/local/mysql/bin/mysqld_safe --skip-grant-tables
```
  then run mysql and do as next :
```
UPDATE mysql.user SET authentication_string=PASSWORD('root') WHERE User='root';
FLUSH PRIVILEGES;
```
  
  and reboot mysql , test it with :

```
show databases;
```
  if you see:
> ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
  
  entry :
```
SET PASSWORD = PASSWORD('root');
```

  and you have made it .(~_~)




  
  
  
  
