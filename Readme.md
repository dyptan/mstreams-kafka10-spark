You need to create Streams with specific name:

`maprcli stream create -path /user/mapr/pump -produceperm u:mapr -consumeperm u:mapr -topicperm u:mapr
maprcli stream topic create -path /user/mapr/pump -topic topic0 -partitions 1`

Build project with SBT:
    
`sbt package`

Or compile with unmanaged Spark deps:

`scalac -classpath $(echo *.jar /opt/mapr/spark/spark-2.4.4/jars/*.jar | tr ' ' ':'):`mapr classpath` DStream.scala -d Streams.jar`

submit the jar to your cluster with spark scripts:

`/opt/mapr/spark/spark-2.4.4/bin/spark-submit --class DStream Streams.jar`

or as standalone app:

```java -cp '/opt/mapr/spark/spark-2.4.4/jars/*':`mapr classpath`:../stream-mapr-spark_2.11-1.0.jar DStream "/user/mapr/pump:topic0"```

populate the stream with some data:

`mapr perfproducer -ntopics 1 -path /user/mapr/pump -nmsgs 50 -npart 1 -rr`

check current offset for consumer:

`maprcli stream cursor list -path /user/mapr/pump -topic topic0`
    
