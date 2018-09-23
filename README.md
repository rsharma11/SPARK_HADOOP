# Installing Hadoop on Mac

# Change ownership of "/usr/local/share/man/man7"
sudo chown -R $(whoami) /usr/local/share/man/man7

## Brew Hadoop 
brew install hadoop

## Go to cd /usr/local/Cellar/hadoop/3.1.1/libexec/etc/hadoop/, then open hadoop-env.sh

export HADOOP_OPTS="$HADOOP_OPTS -Djava.net.preferIPv4Stack=true"

##change to

export HADOOP_OPTS="$HADOOP_OPTS -Djava.net.preferIPv4Stack=true -Djava.security.krb5.realm= -Djava.security.krb5.kdc="
export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.7.0_79.jdk/Contents/Home"



Then configure HDFS address and port number, open core-site.xml, input following content in <configuration></configuration> tag

<!-- Put site-specific property overrides in this file. -->
 <configuration>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/usr/local/Cellar/hadoop/hdfs/tmp</value>
        <description>A base for other temporary directories.</description>
    </property>
    <property>
        <name>fs.default.name</name>
        <value>hdfs://localhost:8020</value>
    </property>
</configuration>

Configure jobtracker address and port number in map-reduce, first sudo cp mapred-site.xml.template mapred-site.xml to make a copy of mapred-site.xml, and open mapred-site.xml, add

<configuration>
    <property>
        <name>mapred.job.tracker</name>
        <value>localhost:8021</value>
    </property>
</configuration>

Set HDFS default backup, the default value is 3, we should change to 1, open hdfs-site.xml, add

<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>

Before running background program, we should format the installed HDFS first, executing command hdfs namenode -format, when terminal returns a long inforamtion like:

17/07/02 16:11:05 INFO namenode.NameNode: STARTUP_MSG: 
/************************************************************
......
17/07/02 16:11:07 INFO namenode.NameNode: SHUTDOWN_MSG: 
/************************************************************
SHUTDOWN_MSG: Shutting down NameNode at haodemacbook-pro.local/192.168.1.4
************************************************************/

It means that we finish HDFS configuration, and Hadoop is ready to launch. Besides, maybe you will get a warning

$ ... WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable

It happens since you are running on 64-bit system but Hadoop native library is based on 32-bit. This is not a big issue. If it appears, you can fixed by refering this link: here.
Launch Hadoop

Go to /usr/local/Cellar/hadoop/2.8.0/sbin, execute:

$ ./start-dfs.sh # start HDFS service
$ ./stop-dfs.sh # stop HDFS service

