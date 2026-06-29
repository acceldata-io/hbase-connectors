<!---
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

# Apache HBase&trade; Spark Connector

## Spark, Scala and Configurable Options

### Building for Spark 3.x (Default)

The default build is configured for Spark 3.x with Scala 2.12. To build:

```
$ mvn clean install
```

To generate an artifact for a different [Spark version](https://mvnrepository.com/artifact/org.apache.spark/spark-core) and/or [Scala version](https://www.scala-lang.org/download/all.html),
[Hadoop version](https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-core), or [HBase version](https://mvnrepository.com/artifact/org.apache.hbase/hbase), pass command-line options as follows (changing version numbers appropriately):

```
$ mvn -Dspark.version=3.5.5 -Dscala.version=2.12.18 -Dhadoop-three.version=3.3.6 -Dscala.binary.version=2.12 -Dhbase.version=2.6.2 clean install
```

### Building for Spark 4.x

To build for Spark 4.x, use the `spark-4` profile. This profile automatically configures:
- Scala 2.13.17
- Jackson 2.20.0
- Jakarta Servlet API (instead of javax.servlet)
- JDK 17 target

```
$ mvn -Pspark-4 clean install
```

**Requirements for Spark 4.x:**
- JDK 17 or later is required
- Spark 4.x uses Jakarta Servlet API instead of javax.servlet

You can also override specific versions with the spark-4 profile:

```
$ mvn -Pspark-4 -Dspark.version=4.1.1.3.3.6.4-1000 -Dhadoop-three.version=3.3.6.3.3.6.4-1000 -Dhbase.version=2.6.2.3.3.6.4-1000 clean install
```

## Configuration and Installation
**Client-side** (Spark) configuration:
- The HBase configuration file `hbase-site.xml` should be made available to Spark, it can be copied to `$SPARK_CONF_DIR` (default is $SPARK_HOME/conf`)

**Server-side** (HBase region servers) configuration:
- The following jars need to be in the CLASSPATH of the HBase region servers:
  - scala-library, hbase-spark, and hbase-spark-protocol-shaded.
- The server-side configuration is needed for column filter pushdown
  - if you cannot perform the server-side configuration, consider using `.option("hbase.spark.pushdown.columnfilter", false)`
- The Scala library version must match the Scala version used for compiling the connector:
  - Spark 3.x: Scala 2.12
  - Spark 4.x: Scala 2.13
