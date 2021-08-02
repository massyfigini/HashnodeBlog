## Very Basic Hadoop

Open Virtualbox and import Cloudera  
click on terminal (black icon)  
go on folder (cd folder, ecc.)  
to copy on HDFS:  
```
hadoop fs -copyFromLocal filename.extension
``` 

if error is because it's in "safe mode" so exec:
``` 
hdfs dfsadmin -safemode leave
``` 

verify if copied:
``` 
hadoop fs -ls
``` 

another copy:
``` 
hadoop fs -cp file.est file2.est
``` 

copy to local:
``` 
hadoop fs -copyToLocal file2.est
``` 

remove a file from HDFS:
``` 
hadoop fs -rm file2.est
``` 

### MapReduce 
See example program on MapReduce:
``` 
hadoop jar /usr/jars/hadoop-examples.jar
``` 

see arguments of WordCount program:
``` 
hadoop jar /usr/jars/hadoop-examples.jar wordcount
``` 

launch WordCount on the file words.txt (must be copied on HDFS):
``` 
hadoop jar /usr/jars/hadoop-examples.jar wordcount words.txt out
``` 

enter the folder "out":
``` 
hadoop fs -ls out
``` 

copy in local the output created (part-r-00000 is the name):
``` 
hadoop fs –copyToLocal out/part-r-00000 local.txt
``` 

Open copied file:
``` 
more local.txt
``` 