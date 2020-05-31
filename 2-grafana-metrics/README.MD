
![OK](https://github.com/DataStax-Academy/nosqlbech-workshop-online/blob/master/materials/images/title-page.png?raw=true)

# Understanding Metrics

# Step 1: Starting a benchmark with Grafana metrics enabled

### 1a. Using --docker-metrics
Even though there are [multiple ways](http://docs.nosqlbench.io/#/docs/getting_started%2F03_reading_metrics) to get metrics from NoSQLBench, by far the easiest and quickest way is to use the **--docker-metrics** parameter. This option tells NoSQLBench to export statistical data so that we can view it in Grafana. It will launch all of the needed components and hook them all up in Docker for you. Let's try it out.


![Windows](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/windows32.png?raw=true)  ![osx](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/mac32.png?raw=true): To run on Windows or OSX use the jar.

📘 **Command to execute**
```
java -jar nb.jar cql-iot cyclerate=100 --progress console:15s --docker-metrics
```

![linux](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/linux32.png?raw=true) : To run on linux use the following command.

📘 **Command to execute**
```bash
./nb cql-iot cyclerate=100 --progress console:15s --docker-metrics
```
On your first run it might take a moment for all of the Grafana images to download into Docker. Once everything is up and running you should see output like the following.

📗 **Expected output**
```
11:11:06.974 [main] WARN  io.nosqlbench.engine.cli.NBCLI - Docker Containers are started, for grafana and prometheus, hit these urls in your browser: http://<host>:3000 and http://<host>:9090
```

This is letting you know that Grafana is now available at a **default** of http://localhost:3000 and Prometheus at a **default** of http://localhost:9090.

# Step 2. View Grafana
At this point you should be able to open http://localhost:3000 in your browser. Let's do that.

### 2a. Launch Grafana in your browser and login
📘 **Command to execute**
<pre>
Ctrl-click on <a href="http://localhost:3000" target="_blank">http://localhost:3000</a> to launch Grafana in a new tab.
</pre>

The first screen you are presented should look something like:
![OK](https://github.com/DataStax-Academy/nosqlbech-workshop-online/blob/master/materials/images/SkipChangePW.png?raw=true)

The credentials are
- username: *admin*
- password: *admin*

Normally, you should change your password when you login the first time, but for this training scenario, just skip it.

### 2b. Navigate to the *Analysis* dashboard
📘 **Command to execute**
<pre>
Next, click on the dropdown arrow by the Home icon.
</pre>
![OK](https://github.com/DataStax-Academy/nosqlbech-workshop-online/blob/master/materials/images/HomeButton.png?raw=true)

📘 **Command to execute**
```
Then, click on Analysis.
```
![OK](https://github.com/DataStax-Academy/nosqlbech-workshop-online/blob/master/materials/images/AnalysisLink.png?raw=true)

The benchmark will need to run for a minute or so to allow statistics to accumulate. Then you will start to see results in the various metrics' graphs.

# Step 3. View Various Metrics

## Woot! You've now got a taste of how to quickly visualize metrics with NoSQLBench. Now, let's move on to creating workloads to fit YOUR data model.

![OK](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/welldone.jpg?raw=true)
