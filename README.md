Steps:

1. Download Apache Jmeter. 
2. Copy the *jar files from /usr/hdp/current/hive-server2-hive2/lib (your relevant dir) into jmeter/lib and jmeter/lib/ext directories.
3. Download hive-jdbc-2.1.0-SNAPSHOT-standalone.jar (or hive-jdbc-3.1.3000.7.2.0.0-237-standalone.jar). Save it in $JMETER_HOME/lib
4. Change the following in the jmx file based on your setup

	Add your jdbc url to this property
 	<stringProp name="dbUrl">jdbc:hive2://localhost:10007/tpcds_bin_partitioned_orc_10000?hive.exec.orc.split.strategy=BI</stringProp> 
5. Set Jmeter heapsize to prevent jmeter crashing on queries returning large resultsets
export _JAVA_OPTIONS="-Djava.awt.headless=true -Xmx8192m"
6. Run jmeter -n -t llap-test.jmx. Look at jmeter.log for any errors reported.
7. Run "python report_2.py  raw_llap_1468573258.xml > results.csv" to get a csv format of queries and 4 runs of runtimes.

For kerberos cluster:


Edit $JMETER_HOME/bin/system.properties and uncomment/add the following lines.

java.security.krb5.conf=/etc/krb5.conf

java.security.auth.login.config=jaas.conf

jaas.conf should already be present in default installation and its content would look like the following.

JMeter {
    com.sun.security.auth.module.Krb5LoginModule required
    doNotPrompt=false
    useKeyTab=false
    storeKey=false;
};

