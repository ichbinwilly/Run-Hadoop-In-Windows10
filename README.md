# Run-Hadoop-In-Windows10
Environment
 - Windows 10
 - Hadoop 2.9.2 (https://hadoop.apache.org/releases.html)
 - myJar.jar
# Configuration
## core-site.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!--
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License. See accompanying LICENSE file.
-->

<!-- Put site-specific property overrides in this file. -->

<configuration>
	<property>
		<name>fs.defaultFS</name>
		<value>hdfs://localhost:9000</value>
	</property>
</configuration>
```
## mapred-site.xml
```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!--
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License. See accompanying LICENSE file.
-->

<!-- Put site-specific property overrides in this file. -->

<configuration>
	<property>
		<name>mapreduce.framework.name</name>
		<value>yarn</value>
	</property>
</configuration>
```
## hdfs-site.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!--
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License. See accompanying LICENSE file.
-->

<!-- Put site-specific property overrides in this file. -->

<configuration>
	<property>
		<name>dfs.replication</name>
		<value>1</value>
	</property>
	<property>
		<name>dfs.namenode.name.dir</name>
		<value>C:\hadoop-2.9.2\data\namenode</value>
	</property>
	<property>
		<name>dfs.datanode.data.dir</name>
		<value>C:\hadoop-2.9.2\data\datanode</value>
	</property>
</configuration>
```

## yarn-site.xml
```xml
<?xml version="1.0"?>
<!--
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License. See accompanying LICENSE file.
-->

<!--
<configuration>
	<property>
		<name>yarn.nodemanager.aux-services</name>
		<value>mapreduce_shuffle</value>
	</property>
	<property>
		<name>yarn.nodemanager.auxservices.mapreduce_shuffle.class</name>
		<value>org.apache.hadoop.mapred.ShuffleHandler</value>
	</property>

</configuration>
-->
<configuration>
<property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
</property>
<property>
    <name>yarn.nodemanager.aux-services.mapreduce_shuffle.class</name>
    <value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
<property>
    <name>yarn.resourcemanager.resource-tracker.address</name>
    <value>localhost:8025</value>
</property>
<property>
    <name>yarn.resourcemanager.scheduler.address</name>
    <value>localhost:8030</value>
</property>
<property>
    <name>yarn.resourcemanager.address</name>
    <value>localhost:8050</value>
</property>
<property>
   <name>yarn.application.classpath</name>
   <value>
		%HADOOP_HOME%\etc\hadoop,
		%HADOOP_HOME%\share\hadoop\common\*,
		%HADOOP_HOME%\share\hadoop\common\lib\*,
		%HADOOP_HOME%\share\hadoop\hdfs\*,
		%HADOOP_HOME%\share\hadoop\hdfs\lib\*,
		%HADOOP_HOME%\share\hadoop\mapreduce\*,
		%HADOOP_HOME%\share\hadoop\mapreduce\lib\*,
		%HADOOP_HOME%\share\hadoop\yarn\*,
		%HADOOP_HOME%\share\hadoop\yarn\lib\*
   </value>
</property>
</configuration>
```
## hadoop-env.cmd
In line 25, modify the jdk source
```
set JAVA_HOME=C:\PROGRA~1\Java\jdk1.8.0_191
```
# Web Interface
1. Namenode Information (http://localhost:50070 )
2. Hadoop Cluster (http://localhost:8088/cluster)

#Trouble Shooting

1. Exception message "FileAlreadyExistsException"

```
C:\hadoop-2.9.2\sbin\javatest>hadoop jar myJar.jar WordCount /input_dir /output_dir
19/01/20 20:13:56 INFO client.RMProxy: Connecting to ResourceManager at localhost/127.0.0.1:8050
Exception in thread "main" org.apache.hadoop.mapred.FileAlreadyExistsException: Output directory hdfs://localhost:9000/output_dir already exists
        at org.apache.hadoop.mapreduce.lib.output.FileOutputFormat.checkOutputSpecs(FileOutputFormat.java:146)
        at org.apache.hadoop.mapreduce.JobSubmitter.checkSpecs(JobSubmitter.java:279)
        at org.apache.hadoop.mapreduce.JobSubmitter.submitJobInternal(JobSubmitter.java:145)
        at org.apache.hadoop.mapreduce.Job$11.run(Job.java:1570)
        at org.apache.hadoop.mapreduce.Job$11.run(Job.java:1567)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:422)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1893)
        at org.apache.hadoop.mapreduce.Job.submit(Job.java:1567)
        at org.apache.hadoop.mapreduce.Job.waitForCompletion(Job.java:1588)
        at WordCount.main(WordCount.java:59)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.apache.hadoop.util.RunJar.run(RunJar.java:244)
        at org.apache.hadoop.util.RunJar.main(RunJar.java:158)
```
Try to remove output_dir because it might be existing already
```
C:\hadoop-2.9.2\sbin\javatest>hdfs dfs -rm -r -f /output_dir
Deleted /output_dir
```

2. CreateSymbolicLink error
```
C:\hadoop-2.9.2\sbin\javatest>hadoop jar myJar.jar WordCount /input_dir /output_dir
19/01/20 20:17:49 INFO client.RMProxy: Connecting to ResourceManager at localhost/127.0.0.1:8050
19/01/20 20:17:51 WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
19/01/20 20:17:52 INFO input.FileInputFormat: Total input files to process : 1
19/01/20 20:17:52 INFO mapreduce.JobSubmitter: number of splits:1
19/01/20 20:17:53 INFO Configuration.deprecation: yarn.resourcemanager.system-metrics-publisher.enabled is deprecated. Instead, use yarn.system-metrics-publisher.enabled
19/01/20 20:17:53 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1547986415722_0001
19/01/20 20:17:54 INFO impl.YarnClientImpl: Submitted application application_1547986415722_0001
19/01/20 20:17:54 INFO mapreduce.Job: The url to track the job: http://willyhsu-PC:8088/proxy/application_1547986415722_0001/
19/01/20 20:17:54 INFO mapreduce.Job: Running job: job_1547986415722_0001
19/01/20 20:18:02 INFO mapreduce.Job: Job job_1547986415722_0001 running in uber mode : false
19/01/20 20:18:02 INFO mapreduce.Job:  map 0% reduce 0%
19/01/20 20:18:03 INFO mapreduce.Job: Job job_1547986415722_0001 failed with state FAILED due to: Application application_1547986415722_0001 failed 2 times due to AM Container for appattempt_1547986415722_0001_000002 exited with  exitCode: 1
Failing this attempt.Diagnostics: [2019-01-20 20:18:02.818]Exception from container-launch.
Container id: container_1547986415722_0001_02_000001
Exit code: 1
Exception message: CreateSymbolicLink error (1314):
```
- This issue can be solved by runing the CMD in administrative mode
 1. Run Command Prompt in admin mode
 2. Run your job (hadoop jar myJar.jar WordCount /input_dir /output_dir)
 
```
C:\hadoop-2.9.2\sbin\javatest>hadoop jar myJar.jar WordCount /input_dir /output_dir
19/01/20 20:36:52 INFO client.RMProxy: Connecting to ResourceManager at localhost/127.0.0.1:8050
19/01/20 20:36:53 WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
19/01/20 20:36:54 INFO input.FileInputFormat: Total input files to process : 1
19/01/20 20:36:54 INFO mapreduce.JobSubmitter: number of splits:1
19/01/20 20:36:55 INFO Configuration.deprecation: yarn.resourcemanager.system-metrics-publisher.enabled is deprecated. Instead, use yarn.system-metrics-publisher.enabled
19/01/20 20:36:55 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1547987747988_0002
19/01/20 20:36:56 INFO impl.YarnClientImpl: Submitted application application_1547987747988_0002
19/01/20 20:36:56 INFO mapreduce.Job: The url to track the job: http://willyhsu-PC:8088/proxy/application_1547987747988_0002/
19/01/20 20:36:56 INFO mapreduce.Job: Running job: job_1547987747988_0002
19/01/20 20:37:16 INFO mapreduce.Job: Job job_1547987747988_0002 running in uber mode : false
19/01/20 20:37:16 INFO mapreduce.Job:  map 0% reduce 0%
19/01/20 20:37:25 INFO mapreduce.Job:  map 100% reduce 0%
19/01/20 20:37:35 INFO mapreduce.Job:  map 100% reduce 100%
19/01/20 20:37:41 INFO mapreduce.Job: Job job_1547987747988_0002 completed successfully
19/01/20 20:37:41 INFO mapreduce.Job: Counters: 49
        File System Counters
                FILE: Number of bytes read=110
                FILE: Number of bytes written=399685
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=199
                HDFS: Number of bytes written=68
                HDFS: Number of read operations=6
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=2
        Job Counters
                Launched map tasks=1
                Launched reduce tasks=1
                Data-local map tasks=1
                Total time spent by all maps in occupied slots (ms)=6000
                Total time spent by all reduces in occupied slots (ms)=6988
                Total time spent by all map tasks (ms)=6000
                Total time spent by all reduce tasks (ms)=6988
                Total vcore-milliseconds taken by all map tasks=6000
                Total vcore-milliseconds taken by all reduce tasks=6988
                Total megabyte-milliseconds taken by all map tasks=6144000
                Total megabyte-milliseconds taken by all reduce tasks=7155712
        Map-Reduce Framework
                Map input records=15
                Map output records=15
                Map output bytes=140
                Map output materialized bytes=110
                Input split bytes=106
                Combine input records=15
                Combine output records=9
                Reduce input groups=9
                Reduce shuffle bytes=110
                Reduce input records=9
                Reduce output records=9
                Spilled Records=18
                Shuffled Maps =1
                Failed Shuffles=0
                Merged Map outputs=1
                GC time elapsed (ms)=184
                CPU time spent (ms)=1887
                Physical memory (bytes) snapshot=504889344
                Virtual memory (bytes) snapshot=597946368
                Total committed heap usage (bytes)=327680000
        Shuffle Errors
                BAD_ID=0
                CONNECTION=0
                IO_ERROR=0
                WRONG_LENGTH=0
                WRONG_MAP=0
                WRONG_REDUCE=0
        File Input Format Counters
                Bytes Read=93
        File Output Format Counters
                Bytes Written=68
```

3. Compile WordCount.java error
```
WordCount.java:4: error: package org.apache.hadoop.conf does not exist
import org.apache.hadoop.conf.Configuration;
                             ^
WordCount.java:5: error: package org.apache.hadoop.fs does not exist
import org.apache.hadoop.fs.Path;
                           ^
WordCount.java:6: error: package org.apache.hadoop.io does not exist
import org.apache.hadoop.io.IntWritable;
                           ^
WordCount.java:7: error: package org.apache.hadoop.io does not exist
import org.apache.hadoop.io.Text;
                           ^
WordCount.java:8: error: package org.apache.hadoop.mapreduce does not exist
import org.apache.hadoop.mapreduce.Job;
                                  ^
WordCount.java:9: error: package org.apache.hadoop.mapreduce does not exist
import org.apache.hadoop.mapreduce.Mapper;
                                  ^
WordCount.java:10: error: package org.apache.hadoop.mapreduce does not exist
import org.apache.hadoop.mapreduce.Reducer;
                                  ^
WordCount.java:11: error: package org.apache.hadoop.mapreduce.lib.input does not exist
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
                                            ^
WordCount.java:12: error: package org.apache.hadoop.mapreduce.lib.output does not exist
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
                                             ^
WordCount.java:17: error: cannot find symbol
       extends Mapper<Object, Text, Text, IntWritable>{
               ^
  symbol:   class Mapper
  location: class WordCount
WordCount.java:17: error: cannot find symbol
       extends Mapper<Object, Text, Text, IntWritable>{
                              ^
  symbol:   class Text
  location: class WordCount
WordCount.java:17: error: cannot find symbol
       extends Mapper<Object, Text, Text, IntWritable>{
```
 - enter "hadoop classpath" get the CLASSPATH information
 
 ```
 C:\hadoop-2.9.2\etc\hadoop;C:\hadoop-2.9.2\share\hadoop\common\lib\*;C:\hadoop-2.9.2\share\hadoop\common\*;C:\hadoop-2.9.2\share\hadoop\hdfs;C:\hadoop-2.9.2\share\hadoop\hdfs\lib\*;C:\hadoop-2.9.2\share\hadoop\hdfs\*;C:\hadoop-2.9.2\share\hadoop\yarn;C:\hadoop-2.9.2\share\hadoop\yarn\lib\*;C:\hadoop-2.9.2\share\hadoop\yarn\*;C:\hadoop-2.9.2\share\hadoop\mapreduce\lib\*;C:\hadoop-2.9.2\share\hadoop\mapreduce\*
```

enter "javac WordCount.java -cp {CLASSPATH}", where {CLASSPATH} is derived from "hadoop classpath" command,
once you finished the job, you will get the following files
 - WordCount$IntSumReducer.class
 - WordCount$TokenizerMapper.class
 - WordCount.class
 
# Run your first MapReduce program
You can download the WordCount.java (https://www.dropbox.com/s/yp9i7nwmgzr3nkx/WordCount.java?dl=0) and compile it. Make a jar file after compiling the java file.
1. javac WordCount.java -cp {CLASSPATH}
  > {CLASSPATH} : Hadoop provides the utility to gt the class information by entering "hadoop classpath" 
2. jar -cf WordCount.jar WordCount*
3. hadoop jar WordCount.jar WordCount /input_dir /output_dir
4. Print out the result "hdfs dfs -cat /output_dir2/*"
	```
	alex    2
	nicki   3
	nicky   1
	q       2
	queenb  1
	test    2
	william 1
	willy   2
	wily    1
	```
