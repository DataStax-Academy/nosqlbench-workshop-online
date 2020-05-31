
![OK](https://github.com/DataStax-Academy/nosqlbech-workshop-online/blob/master/materials/images/title-page.png?raw=true)

# Custom Workloads
This section starts getting into a slightly more advanced topic, but once you understand the pattern you'll be on your way to creating workloads and scenarios for your own data models. This won't be an exhaustive walkthrough, but should give you enough to get going.

# Step 1: Listing Workloads and Named Scenarios
The benchmarking workloads we have run in previous scenarios we all pre-packaged. Now, we'll take a look at how these are built so we can start creating our own.

### 1a. Using --list-workloads
It's usually easiest to modify an existing workload. We can list the pre-packaged workloads with this command.

![Windows](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/windows32.png?raw=true)  ![osx](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/mac32.png?raw=true): To run on Windows or OSX use the jar.

📘 **Command to execute**
```
java -jar nb.jar --list-workloads
```

![linux](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/linux32.png?raw=true) : To run on linux use the following command.

📘 **Command to execute**
```bash
./nb --list-workloads
```

📗 **Expected output**
```
activities/baselines/cql-iot-dse.yaml
# description:
An IOT workload which more optimal DSE settings

activities/baselines/cql-iot.yaml
# description:
This workload emulates a time-series data model and access patterns.
...
```

### 1b. Using --list-scenarios
Named scenarios are found within workloads and allow you to create multiple variations of scenarios. For example, maybe for the *default* scenario I perform queries as I would expect, but I also might want an *allow-filtering* scenario to test out what might happen if I use **ALLOW FILTERING** in my read queries. I can add both to a workload to make it easy to switch between them, reuse the same bindings, or even run them as part of a single benchmark for comparison.

![Windows](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/windows32.png?raw=true)  ![osx](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/mac32.png?raw=true): To run on Windows or OSX use the jar.

📘 **Command to execute**
```
java -jar nb.jar --list-scenarios
```

![linux](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/linux32.png?raw=true) : To run on linux use the following command.

📘 **Command to execute**
```bash
./nb --list-scenarios
```

📗 **Expected output**
```
# workload in activities/baselines/cql-iot.yaml
# description:
This workload emulates a time-series data model and access patterns.
    # scenarios:
    nb activities/baselines/cql-iot default
        # defaults
        compression = LZ4Compressor
        expiry_minutes = 60
        instrument = false
        instrument-reads = false
        instrument-writes = false
        keyspace = baselines
        limit = 10
...
```
Notice how the above example displays the **default** scenario within the **cql-iot** workload. Not only that, but this output will also display all of the default parameters being used.

# Step 2: Copying Workloads
Ok, so we listed some workloads and scenarios, but all of the pre-packaged ones are stored in the nb binary or jar. Instead of trying to create one the first time from scratch it's best to simply copy an existing one and go from there. Luckily, there's a very simple command to do just that.

### 2a. Using --copy
Since we've been using the cql-iot workload throughout this workshop let's work with that. Just use the **--copy** option and pass the name of the workload. That's it.

![Windows](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/windows32.png?raw=true)  ![osx](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/mac32.png?raw=true): To run on Windows or OSX use the jar.

📘 **Command to execute**
```
java -jar nb.jar --copy cql-iot
```

![linux](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/linux32.png?raw=true) : To run on linux use the following command.

📘 **Command to execute**
```bash
./nb --copy cql-iot
```

📗 **Expected output**
```
There is no log output, but cql-iot.yaml should be sitting in the directory where you ran the above command.
```

### 2b. Reading the yaml
Briefly, inspect the extracted file with your favorite editor. Like an unfamiliar yaml file, these can be a bit overwhelming at first glance, but we'll walk you through it. By the end of this scenario, you'll be comfortable with the basics!

📘 **Command to execute**
```
Using your favorite text/yaml editor open cql-iot.yaml and inspect the contents
```

*Don't go too crazy trying to understand everything in the file at this point. We'll build up to that. For now a quick once-over is fine.*

# Step 3: Building Your First Workload
Since this is our first introduction to workload configuration, we created some stripped-down files to emphsize the important parts. We'll use these stripped-down files instead of the one we extracted, but when we are done, you should be able to make your way through the file you extracted.

## Alrighty. I think at this point you can call yourself dangerous and start executing NoSQLBench against your own data models. Good luck and happy benchmarking!

![OK](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/welldone.jpg?raw=true)
