
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
```bash
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

### 3a. Create a test schema
The first thing a workload wants to do is create the necessary keyspace and tables for the benchmark. Here's an example workload to create the IOT keyspace and tables.

```yaml
# nb run driver=cql workload=cql-iot-basic-schema.yaml threads=auto cycles=3
description: |
  This workload emulates a time-series data model schema creation.
blocks:
  - tags:
      phase: schema
    params:
      prepared: false
    statements:
     - create-keyspace: |
        create keyspace if not exists baselines
        WITH replication = {'class': 'SimpleStrategy', 'replication_factor': '1'}
        AND durable_writes = true;
     - create-table : |
        create table if not exists baselines.iot (
        machine_id UUID,     // source machine
        sensor_name text,    // sensor name
        time timestamp,      // timestamp of collection
        sensor_value double, //
        station_id UUID,     // source location
        data text,
        PRIMARY KEY ((machine_id, sensor_name), time)
        ) WITH CLUSTERING ORDER BY (time DESC)
         AND compression = { 'sstable_compression' : 'LZ4Compressor' }
         AND compaction = {
         'class': 'TimeWindowCompactionStrategy',
         'compaction_window_size': 60,
         'compaction_window_unit': 'MINUTES'
        };
     - truncate-table: |
         truncate table baselines.iot;
```

We'll put a comment at the top of the file that shows how to run the workload as well as a description of the workload.
```yaml
# nb run driver=cql workload=cql-iot-basic-schema.yaml threads=auto cycles=3
description: |
  This workload emulates a time-series data model schema creation.
```

The remainder of this file is a list named blocks containing a single block. Each block has tags. We use these tags to indicate the phase of the workload. In this example, the phase is **schema**, which is short for schema creation. Remember back in the **Executing Commands** section when we executed the command "run driver=cql workload=cql-keyvalue tags=phase:schema"? This is the *Schema* phase we were referring to in the **cql-keyvalue** workload. When called, it will process everything in the **schema** phase.
```yaml
blocks:
  - tags:
      phase: schema
```

Ok, so let's take a look at the rest of the file. 
- **prepared: false** is telling nb none of the following statements needs to be prepared. Since these are used only once for schema creation they do not need to be prepared
- **statements:** sets the statements that will be executed within this phase
- **- create-keyspace:** sets the CQL DDL statement to use for creating the keyspace
- **- create-table :** sets the CQL DDL statement(s) to use for creating and tables
- **- truncate-table:** is truncating the table in case it already exists to ensure data is clean

Notice within each **statement** type are the CQL statements to execute. These are exactly the same as the statements you might execute within cqlsh or from within your application.
```yaml
params:
      prepared: false
    statements:
     - create-keyspace: |
        create keyspace if not exists baselines
        WITH replication = {'class': 'SimpleStrategy', 'replication_factor': '1'}
        AND durable_writes = true;
     - create-table : |
        create table if not exists baselines.iot (
        machine_id UUID,     // source machine
        sensor_name text,    // sensor name
        time timestamp,      // timestamp of collection
        sensor_value double, //
        station_id UUID,     // source location
        data text,
        PRIMARY KEY ((machine_id, sensor_name), time)
        ) WITH CLUSTERING ORDER BY (time DESC)
         AND compression = { 'sstable_compression' : 'LZ4Compressor' }
         AND compaction = {
         'class': 'TimeWindowCompactionStrategy',
         'compaction_window_size': 60,
         'compaction_window_unit': 'MINUTES'
        };
     - truncate-table: |
         truncate table baselines.iot;
```

*BTW, all of the above sections are optional. Obviously, you would need to create a keyspace if you want any tables, but you technically don't have to include it. I just included this set of examples to give a "full" picture of what you might need for a schema.*

### 3b. Insert initial seed data
When benchmarking you're attempting to emulate what your what your system performance looks like under realistic conditions. This includes access patterns, data density, and factoring in headroom that any highly available system requires. The **rampup** phase is meant to bring a system under test to a realistic density with a realistic data set.

*Response, throughput, and data density are always connected. Every database performs differently at higher density for read operations, thus it is imperative that you qualify your results with all three parameters: throughput, latency, and density.*

```yaml
# nb run driver=cql workload=cql-iot-basic-rampup.yaml threads=auto cycles=100000
description: |
  This workload emulates a time-series data model and ramps up data after schema creation.
blocks:
  - tags:
      phase: rampup
    bindings:
      machine_id: Mod(10000); ToHashedUUID() -> java.util.UUID
      sensor_name: HashedLineToString('data/variable_words.txt')
      time: Mul(100); Div(10000L); ToDate()
      cell_timestamp: Mul(100L); Div(10000L); Mul(1000L)
      sensor_value: Normal(0.0,5.0); Add(100.0) -> double
      station_id: Div(10000);Mod(100); ToHashedUUID() -> java.util.UUID
      data: HashedFileExtractToString('data/lorem_ipsum_full.txt',800,1200)
    statements:
     - insert-rampup: |
        insert into baselines.iot
        (machine_id, sensor_name, time, sensor_value, station_id, data)
        values ({machine_id}, {sensor_name}, {time}, {sensor_value}, {station_id}, {data})
        using timestamp {cell_timestamp}
       idempotent: true
       prepared: true
       cl: LOCAL_QUORUM
```

Notice the name of our **phase** tag. Again, remember back to the **Executing Commands** section when we executed the command "start driver=stdout workload=cql-keyvalue **tags=phase:rampup** cycles=10"? This was executing the **rampup** phase which inserts the initial dataset into our data model and primes us for our benchmark.
```yaml
blocks:
  - tags:
      phase: rampup
```

I think at this point you will recognize the **insert** statement within the **statements** section to be the insert statement responsible for our data load. Again, this is the same DML statement would you execute in cqlsh or from your application. 
Notice the following paramters, again, all optional.
- **idempotent: true** sets idempotency to true
- **prepared: true** sets prepared to true since this query will run many times
- **cl: LOCAL_QUORUM** sets our consistency level to LOCAL_QUORUM

The last part I want to call out is the **bindings** section. This is the section where our "generated" data comes from. I put generated in quotes because data is not randomly generated or anything like that. It comes from initial seed data and functions used to calculate values. The key here is that this data is deterministic which means subsequent runs will generate the same data. If you want a deeper dive take a look [here](http://docs.nosqlbench.io/#/docs/bindings).

For now, I want you to see the relationship between bindings and how they are applied to the insert statement.
Notice the following 3 bindings.
```yaml
machine_id: Mod(10000); ToHashedUUID() -> java.util.UUID
sensor_name: HashedLineToString('data/variable_words.txt')
time: Mul(100); Div(10000L); ToDate()
```

Now notice our insert statement. See the curly braces {} around **machine_id**, **sensor_name**, and **time**? These are mapping the respective bindings to the insert statement. That's it. Create your bindings, then apply to your queries.
```yaml
 insert into baselines.iot
        (machine_id, sensor_name, time, sensor_value, station_id, data)
        values ({machine_id}, {sensor_name}, {time}, {sensor_value}, {station_id}, {data})
```


## Alrighty. I think at this point you can call yourself dangerous and start executing NoSQLBench against your own data models. Good luck and happy benchmarking!

![OK](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/welldone.jpg?raw=true)
