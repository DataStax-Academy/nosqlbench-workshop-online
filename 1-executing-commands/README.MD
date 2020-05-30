
![OK](https://github.com/DataStax-Academy/nosqlbech-workshop-online/blob/master/materials/images/title-page.png?raw=true)

# Executing Commands

# Step 1: Initial Run

### 1a. Running NoSQLBench commands
At this point you should have downloaded either the binary or jar and are ready to start executing commands with NoSQLBench. We will run a quick scenario just to ensure things are working. Execute the following commands to get going.

![Windows](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/windows32.png?raw=true)  ![osx](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/mac32.png?raw=true): Notice that when using the jar you will to prepend your commands with "java -jar nb.jar", but other than that ALL commands will work exactly the same as the binary. 

📘 **Command to execute**
```
java -jar nb.jar cql-iot
```

![linux](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/linux32.png?raw=true) : To run on linux use the following command.

📘 **Command to execute**
```bash
# Let's run a simple scenario (just ctrl-C to kill it once it starts up)
./nb cql-iot
```

### 1b. Some initial results
Wether you're using the binary or jar you should see output similar the following log statement and initial reporting. Feel free to kill the scenario with ctrl-C at this point.

In the previous "nb cql-iot" command we just ran a pre-packaged scenario. Don't worry about the details of that just yet, we'll get there. What's key is you see something similar to the output below which means everything is hooked up and running.

📗 **Expected output**
```bash
Logging to logs/scenario_20200530_110840_609.log
cql-iot_default_001: 3.45%/Running (details: min=0 cycle=345214 max=10000000)
cql-iot_default_000: 100.00%/Finished (details: min=0 cycle=3 max=3) (last report)
```

### 1c. Let's clean up
The bechmark we just ran created a keyspace. Let's delete it just to keep things clean. Again, the following instruction assumes you are using the Docker setup. If you are using your own Cassandra then you'll need to use your own cqlsh as well. :)

📘 **Command to execute**
```
docker exec -it my-cassandra cqlsh -e "DROP KEYSPACE baselines;"
```

## Step 2: Create a Test Schema

### 2a. Create the schema
In the previous step, we ran a pre-packaged benchmark. In the remainder of this scenario, we'll show you how to un-package and customize tests.

![Windows](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/windows32.png?raw=true)  ![osx](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/mac32.png?raw=true): To run on Windows or OSX use the jar.

📘 **Command to execute**
```
java -jar nb.jar run driver=cql workload=cql-keyvalue tags=phase:schema
```

![linux](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/linux32.png?raw=true) : To run on linux use the following command.

📘 **Command to execute**
```bash
./nb run driver=cql workload=cql-keyvalue tags=phase:schema
```

Here's what the command parameters mean:

- **run** tells **nb** to execute the sequence of operations synchronously
- **driver=cql** tells **nb** to talk to a Cassandra database using cql
- **workload=cql-keyvalue** identifies the workload description file, which in this case is **cql-keyvalue**
- **tags=phase:schema** identifies the section of the workload file containing the operations to create the schema

## 2b. View the results
In the last command we created a schema within our database for the cql-keyvalue workload. Let's take a look at what we just created.

📘 **Command to execute**
```
docker exec -it my-cassandra cqlsh -e "DESCRIBE KEYSPACE baselines;"
```

Look at the query output. You should see we created a keyspace named **baselines** and a table named **keyvalue**.

## Step 3: Write Some Test Data
Now that we have our newly created table, let's load some data, but before we do this we might want to review the activity **nb** will generate. We can do this by setting **driver=stdout**. Let's do it.

### 3a. Test activity with stdout

![Windows](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/windows32.png?raw=true)  ![osx](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/mac32.png?raw=true): To run on Windows or OSX use the jar.

📘 **Command to execute**
```
java -jar nb.jar start driver=stdout workload=cql-keyvalue tags=phase:rampup cycles=10
```

![linux](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/linux32.png?raw=true) : To run on linux use the following command.

📘 **Command to execute**
```bash
./nb run start driver=stdout workload=cql-keyvalue tags=phase:rampup cycles=10
```

Let's review the parameters for this command:

- **start** is like **run**, but is asynchronous, where as **run** is synchronous
- **driver=stdout** only prints CQL statements that NoSQLBench generates, but applies no statements to the database
- **workload=cql-keyvalue** sets the name of the workload configuration file to cql-keyvalue
- **tags=phase:rampup** is the target within the workload file
- **cycles=10** tells **nb** how many times to run the target within the workload

**NOTE:** *NoSQLBench generates data values deterministically so that the values will be the same from run to run. So, the output you see with **driver=stdout** will be the same as the values actually inserted with **driver=cql**.*

📗 **Expected output**
```
 Logging to logs/scenario_20200530_134442_399.log
insert into baselines.keyvalue
(key, value)
values (0,382062539);
insert into baselines.keyvalue
(key, value)
values (1,774912474);
insert into baselines.keyvalue
(key, value)
values (2,949364593);
insert into baselines.keyvalue
(key, value)
values (3,352527683);
insert into baselines.keyvalue
(key, value)
values (4,351686621);
...
```

### 3b. This time let's insert the data into our table
The command in this case will be almost exactly the same with one caveat. Notice the value for the **driver** parameter.

![Windows](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/windows32.png?raw=true)  ![osx](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/mac32.png?raw=true): To run on Windows or OSX use the jar.

📘 **Command to execute**
```
java -jar nb.jar start driver=cql workload=cql-keyvalue tags=phase:rampup cycles=10
```

![linux](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/linux32.png?raw=true) : To run on linux use the following command.

📘 **Command to execute**
```bash
./nb run start driver=cql workload=cql-keyvalue tags=phase:rampup cycles=10
```

### 3c. View the results
Our last command used **driver=cql** instead of **driver=stdout** which means it used the CQL driver to talk to Cassandra and insert the rampup data as compared to printing it to the terminal. Let's verify.

📘 **Command to execute**
```
docker exec -it my-cassandra cqlsh -e "SELECT * FROM baselines.keyvalue;"
```

If you compare the data from our earlier command using **driver=stdout** the data in the table should match our previous output.

📗 **Expected output**
```
 key | value
-----+-----------
   6 | 439790106
   7 | 564330072
   9 |  97405552
   4 | 351686621
   3 | 352527683
   5 | 114304900
   0 | 382062539
   8 | 296173906
   2 | 949364593
   1 | 774912474
```

## Congratulations, you've executed a set of NoSQLBench commands. Hopefully you are starting to get a feel for how it works.

![OK](https://github.com/DataStax-Academy/nosqlbench-workshop-online/blob/master/materials/images/welldone.jpg?raw=true)
