building-G2B-document-
======================

编译G2B心得体会


编译的具体步骤：

1.选择一款可以编译的手机非常重要，[《Firefox OS build prerequisites》](https://developer.mozilla.org/en-US/docs/Mozilla/Firefox_OS/Firefox_OS_build_prerequisites) 这篇文章告诉大家，有三类设备可以在上面包firefox os.

我选用的设备是：google nexus 7 II (2013/10/18 最新机型)

硬件设备：lenovo thinkpad E430c 

系统：ubuntu 12.04 64位

依照[《Firefox OS build prerequisites》](https://developer.mozilla.org/en-US/docs/Mozilla/Firefox_OS/Firefox_OS_build_prerequisites) 把该安装的都安装上。


2.然后看[《Preparing for your first B2G build》](https://developer.mozilla.org/en-US/docs/Mozilla/Firefox_OS/Preparing_for_your_first_B2G_build)这篇文档。

   步骤1：clone B2G repository
         
         复制B2G到自己的ubuntu目录下

         git clone git://github.com/mozilla-b2g/B2G.git

         然后进入

         cd B2G

         (你的机器设备保证是Android 4.0.1以上)

          然后开始下源代码到机器上

         ./config.sh
         
         这时候会出现所有可以编译机型（如果没有你的机型，你也可以花时间移植你的B2G，我试过，好多东西根本不会。也没有时间干这事，所以还是买个好点的手机靠普点。）

         会出现如下：

         Usage: ./config.sh [-cdflnq] (device name)

         Flags are passed through to |./repo sync|.

         Valid devices to configure are:
         - galaxy-s2
         - galaxy-nexus
         - nexus-4
         - nexus-s
         - nexus-s-4g
         - otoro
         - unagi
         - inari
         - keon
         - peak
         - leo
         - hamachi
         - helix
         - wasabi
         - tara
         - pandaboard
         - emulator
         - emulator-jb
         - emulator-x86
         - emulator-x86-jb
         
         然后选择你的机型

         我的是 google nexus 7 II :flo（如果是google nexus 7 I应该选择 nexus-s）

         ./config.sh flo
         
         然后就是漫长的代码下载了，一般也要1-2天，我下载的时候网速很慢，老下不成功，查了一下 
         
         加了这样一句代码：
         
         将git缓冲增加到500

         git config http.postBuffer 5242888000

         经过一晚上，终于下载完成了。

3.然后就是编译了，也要几个小时。[《Building Firefox OS》](https://developer.mozilla.org/en-US/docs/Mozilla/Firefox_OS/Building)

         首先更新一下代码
         
         git pull
         
         ./repo sync
         
         更新一下gaia层。

         ./repo sync gaia
         
         然后可以编译了
         
         cd B2G

         ./build.sh

         如果你没有Android环境，还有java环境都要先设置一下

         --java环境

         1.查看操作系统位数

         getconf LONG_BIT  //64
         
         看看操作系统信息
     
         lsb_release -a


         下载 java-jdk

         http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html

         压缩 jdk-7-linux-i586.tar.gz

         将它解压并移动到/usr/lib/jvm 目录下

         mkdir /usr/lib/jvm

         将改好名的 jdk-7-sun 移动到 /usr/lib/jvm
         
         mv /usr/lib/jvm/jdk-7-sun /usr/lib/jvm/java-1-sum

         修改局部变量
         
         sudo apt-get install vim(先安装这个工具)
         
         配置环境：

         vim ~/.bashrc

         export JAVA_HOME=/usr/lib/jvm/jdk-7-sun
         
         exprot JRE_HOME=${JAVA_HOME}/jre

         export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib

         export PATH=${JAVA_HOME}/bin:$PATH

         加载末尾 保存退出 source ~/.bashrc

         配置默认JDK版本
         
         sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk-7-sun/bin/java 300 
         sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk-7-sun/bin/javac 300 
         sudo update-alternatives --install /usr/bin/jar jar /usr/lib/jvm/jdk-7-sun/bin/jar 300

         检查
         
         sudo update-alternatives --config java

         查看版本号

         java -version

         列出


         java version "1.7.0_01"
		 Java(TM) SE Runtime Environment (build 1.7.0_01-b08)
		 Java HotSpot(TM) Client VM (build 21.1-b02, mixed mode)
			        

         2安装Android所用的工具
         
         $ sudo apt-get install git gnupg flex bison gperf build-essential \
           zip curl libc6-dev libncurses5-dev:i386 x11proto-core-dev \
           libx11-dev:i386 libreadline6-dev:i386 libgl1-mesa-glx:i386 \
           libgl1-mesa-dev g++-multilib mingw32 tofrodos \
           python-markdown libxml2-utils xsltproc zlib1g-dev:i386
         $ sudo ln -s /usr/lib/i386-linux-gnu/mesa/libGL.so.1 /usr/lib/i386-linux-gnu/libGL.so


现在可以编译了。又是好几个小时，呵呵。用了17天，终于搞定了。参考文档 [https://developer.mozilla.org/en-US/docs/Mozilla/Firefox_OS/Building](https://developer.mozilla.org/en-US/docs/Mozilla/Firefox_OS/Building)

         

        

         

         
         
         
         

         

        

         



         
         

         





