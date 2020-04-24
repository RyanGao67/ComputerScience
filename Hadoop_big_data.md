### Get started
* Install VirtualBox
* Go to cloudera.com/downloads.html => downloads hortonworks Sandbox  (Google the steps to setup hortonworks HDP sandbox)
Sandbox HDP Virtualbox Downloads => 2.6.5 (this edition is good for learning 2020.4.22)

### Mapreduce
u.data this is the example data file
```
22      377     1       878887116
244     51      2       880606923
166     346     1       886397596
298     474     4       884182806
115     265     2       881171488
253     465     5       891628467
0       50      5       881250949
0       172     5       881250949
0       133     1       881250949
196     242     3       881250949
186     302     3       891717742
22      377     1       878887116
244     51      2       880606923
166     346     1       886397596
298     474     4       884182806
```
The following is the mapreduce job written in python, as the mapreduce is outdated these days, we just need to read this. and be able to run this. 
```py
from mrjob.job import MRJob
from mrjob.step import MRStep

class RatingsBreakdown(MRJob):
    def steps(self):
        return [
            MRStep(
                mapper=self.mapper_get_ratings,
                reducer=self.reducer_count_ratings)
        ]

    def mapper_get_ratings(self, _, line):
        (userID, movieID, rating, timestamp) = line.split('\t')
        yield rating, 1

    def reducer_count_ratings(self, key, values):
        yield key, sum(values)

if __name__ == '__main__':
    RatingsBreakdown.run()
```  

to run the map reduce job

```
[maria_dev@sandbox-hdp ~]$ python RatingsBreakdown.py -r hadoop --hadoop-streaming-jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar u.data
No configs found; falling back on auto-configuration
Looking for hadoop binary in $PATH...
Found hadoop binary: /usr/bin/hadoop
Using Hadoop version 2.7.3.2.6.5.0
Creating temp directory /tmp/RatingsBreakdown.maria_dev.20200423.190527.764509
Copying local files to hdfs:///user/maria_dev/tmp/mrjob/RatingsBreakdown.maria_d                                                                                                             ev.20200423.190527.764509/files/...
Running step 1 of 1...
  packageJobJar: [] [/usr/hdp/2.6.5.0-292/hadoop-mapreduce/hadoop-streaming-2.7.3.2.6.5.0-292.jar] /tmp/streamjob5085523190627013476.jar tmpDir=null
  Connecting to ResourceManager at sandbox-hdp.hortonworks.com/172.18.0.2:8032
  Connecting to Application History server at sandbox-hdp.hortonworks.com/172.18.0.2:10200
  Connecting to ResourceManager at sandbox-hdp.hortonworks.com/172.18.0.2:8032
  Connecting to Application History server at sandbox-hdp.hortonworks.com/172.18.0.2:10200
  Total input paths to process : 1
  number of splits:2
  Submitting tokens for job: job_1587645571920_0001
  Submitted application application_1587645571920_0001
  The url to track the job: http://sandbox-hdp.hortonworks.com:8088/proxy/application_1587645571920_0001/
  Running job: job_1587645571920_0001
  Job job_1587645571920_0001 running in uber mode : false
   map 0% reduce 0%
   map 100% reduce 0%
   map 100% reduce 100%
  Job job_1587645571920_0001 completed successfully
  Output directory: hdfs:///user/maria_dev/tmp/mrjob/RatingsBreakdown.maria_dev.20200423.190527.764509/output
Counters: 49
        File Input Format Counters
                Bytes Read=2088191
        File Output Format Counters
                Bytes Written=49
        File System Counters
                FILE: Number of bytes read=800030
                FILE: Number of bytes written=2072886
                FILE: Number of large read operations=0
                FILE: Number of read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=2088549
                HDFS: Number of bytes written=49
                HDFS: Number of large read operations=0
                HDFS: Number of read operations=9
                HDFS: Number of write operations=2
        Job Counters
                Data-local map tasks=2
                Launched map tasks=2
                Launched reduce tasks=1
                Total megabyte-milliseconds taken by all map tasks=4925000
                Total megabyte-milliseconds taken by all reduce tasks=1723750
                Total time spent by all map tasks (ms)=19700
                Total time spent by all maps in occupied slots (ms)=19700
                Total time spent by all reduce tasks (ms)=6895
                Total time spent by all reduces in occupied slots (ms)=6895
                Total vcore-milliseconds taken by all map tasks=19700
                Total vcore-milliseconds taken by all reduce tasks=6895
        Map-Reduce Framework
                CPU time spent (ms)=4030
                Combine input records=0
                Combine output records=0
                Failed Shuffles=0
                GC time elapsed (ms)=457
                Input split bytes=358
                Map input records=100003
                Map output bytes=600018
                Map output materialized bytes=800036
                Map output records=100003
                Merged Map outputs=2
                Physical memory (bytes) snapshot=497594368
                Reduce input groups=5
                Reduce input records=100003
                Reduce output records=5
                Reduce shuffle bytes=800036
                Shuffled Maps =2
                Spilled Records=200006
                Total committed heap usage (bytes)=241172480
                Virtual memory (bytes) snapshot=5838700544
        Shuffle Errors
                BAD_ID=0
                CONNECTION=0
                IO_ERROR=0
                WRONG_LENGTH=0
                WRONG_MAP=0
                WRONG_REDUCE=0
Streaming final output from hdfs:///user/maria_dev/tmp/mrjob/RatingsBreakdown.maria_dev.20200423.190527.764509/output...
"1"     6111
"2"     11370
"3"     27145
"4"     34174
"5"     21203
Removing HDFS temp directory hdfs:///user/maria_dev/tmp/mrjob/RatingsBreakdown.maria_dev.20200423.190527.764509...
Removing temp directory /tmp/RatingsBreakdown.maria_dev.20200423.190527.764509...

```

