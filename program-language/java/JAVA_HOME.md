[Licensed under the CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)

在此感谢 [@oracle](https://www.oracle.com/index.html)

文档不定时更新，如有疑问请及时联系本文作者，当前文档版本1.0.0

[jdk-8u221-linux-x64.tar.gz](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

~~~
linux
vi /etc/profile
export JAVA_HOME=/usr/java/jdk1.8.0_221
export JRE_HOME=$JAVA_HOME/jre
export JAVA=$JAVA_HOME/bin/java                                                       # 可选
export CLASSPATH=.:$JRE_HOME/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
source /etc/profile
java
javac
java -version
echo $JAVA_HOME
echo $JRE_HOME
echo $CLASSPATH
echo $PATH

windows
set JAVA_HOME=D:\Program Files\Java\jdk1.8.0_221
set JRE_HOME=%JAVA_HOME%\jre
set JAVA=%JAVA_HOME%\bin\java.exe                                                      # 可选
set CLASSPATH=.;%JRE_HOME%\lib\rt.jar;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar
set PATH=%JAVA_HOME%\bin;%JRE_HOME%\bin;%PATH%
java
javac
java -version
echo %JAVA_HOME%
echo %JRE_HOME%
echo %CLASSPATH%
echo %PATH%
~~~