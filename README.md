# pysparknote


  Agenda (7 sessions * 4 hours each)
  -----------------------------------
   1. Spark - Basics & Architecture
   
# Spark Basics:
Apache Spark is a distributed computing framework used for large-scale data processing.
Spark is designed to process data in memory and can be up to 100x faster than Hadoop MapReduce for certain workloads.
Spark provides APIs for various programming languages, including Scala, Java, Python, and R.
Spark has several components, including Spark Core, Spark SQL, Spark Streaming, MLlib, and GraphX, which provide support for various data processing tasks.

# Spark Architecture:
Spark architecture is based on a master-slave architecture, where the master node is called the Spark Driver and the slave nodes are called the Spark Executors.
The Spark Driver program creates and manages the Spark Context, which is the entry point for Spark applications and coordinates the distribution of tasks across the cluster.
The Spark Executors are responsible for executing tasks and storing data in memory or on disk.
Spark provides a distributed storage system called RDD (Resilient Distributed Dataset) that is partitioned across the cluster and can be processed in parallel across multiple nodes.
Spark uses a DAG (Directed Acyclic Graph) execution engine to execute tasks in parallel and optimize the execution plan based on the dependencies between tasks.
Spark can run on various cluster managers, such as Apache Mesos, Hadoop YARN, and Kubernetes, and can also be run in standalone mode.
Overall, Spark's architecture is designed to provide efficient and scalable data processing using a distributed computing model that processes data in memory and enables parallel execution of tasks across a cluster of machines.

   2. Spark Core API
	-> RDD Transformations & Actions
	-> Shaed Variables
   3. Spark SQL 
   4. Spark MLlib (& Machine Learning)
   5. Spark Streaming (Introduction)


   Materials
   ---------
	-> PDF versions of the presentations
	-> Lot of Code Modules
        -> Class Notes


   Accesssing your vLab
   --------------------
	-> Login and CentOS 7 icon on the desktop
	-> Enter your userid and password - (check readme.txt file for details)
		-> This opens the VM where your lab is there.

	-> Open a Terminal and type
		$ jupyter notebook --allow-root
		-> This opens the Jupyter Notebook

	
	
	
   Cluster
   -------	
      Is a unified entity comprising of multiple nodes whose cumulative resources can
      be used to distribute your storage and processing. 
  
   Spark
   -----
      => Is a unified in-memory distributed computing framework	
      => Written in Scala

      => In-memory distributed computing
	    -> The intermediate results (partitions) can be saved in-memory and subsequent tasks
               can be aluncjed on them. 
       
      => Unified Framework
	    -> Spark comes with a consistent set of API running on the same execution engine to
	       process different types of analytical work loads. 
		
		Batch Processing of unstructurted data	=> Spark Core
		Batch Processing of structurted data	=> Spark SQL
		Stream Processing			=> Spark Streaming
		Predictive analytics (ML)		=> Spark MLlib
		Graph Parallel Computations		=> Spark GraphX

      => Spark is a polyglot
	    -> We can write Spark application using Scala, Python, Java and R

      => Spark Layered Architecture

	  => Programming Languages:	Scala, Python, Java, R & SQL  
	  => Spark High Level APIs:	Spark SQL, Spark Streaming, Spatk MLlib, Spark GraphX
	  => Spark Low Level API:	Spark Core (RDD API) 
	  => Resource Managers : 	Spark Standalone, YARN, Mesos, Kubernetes
	  => Storage Layer:   		Linux, HDFS, S3, Kafka, RDBMS, NoSQL

    	
    Spark Architecture
    ------------------

	1. Cluster Manager
		-> Jobs are submitted to CM
		-> Spark programs can be submitted to Spark Standalone, YARN, Mesos, Kubernetes
		-> CM will allocate executors to the application. 

	2. Driver
		-> Master Process
		-> Contains the SparkContext
		-> Maintain all the metadata of the user program and of RDD
		-> Send tasks to the executors

		Deploy Modes:
		 client  -> default, the driver process runs on the client machine.
		 cluster -> driver runs in one of the node in the cluster

	3. Executors
		-> Executes the tasks sent by the driver
		-> All tasks do the same processing, but on different partitions of the data
		-> They report the status of the tasks to the driver.

	4. SparkContext
		-> Starting point of execution 
		-> Created inside a driver
		-> Represents an application
		-> Link betweeb the driver and several tasks running in the executors


     Getting started with Spark
     --------------------------
	1. Working with Spark Shell

		-> Make sure you have Java (jdk 1.8.x) running
		-> Download Spark binaries from https://spark.apache.org/downloads.html
		-> Extract it to a suitable location.

		-> Add the necessary environment variables:	
			SPARK_HOME   -> D:\spark-3.2.0-bin-hadoop2.7
			HADOOP_HOME  -> D:\spark-3.2.0-bin-hadoop2.7
			PYTHONPATH   -> %SPARK_HOME%/python;%SPARK_HOME%/python/lib/py4j-0.10.9-src.zip;%PYTHONPATH%
			Add SPARK_HOME to the PATH Environment variable

                -> Launch PySpark shell.

	2. Install an IDE

		-> Install Anaconda Navigator (https://www.anaconda.com/products/individual)
		-> Follow the instructions given in the GitHub to setup PySpark with Jupytor Notebook / Spyder

        3. Signup to Databrick Community Edition (Free Cloud Account)
		URL: https://databricks.com/try-databricks

		-> Read the "Quick Start Tutorial"  (Explore the Quickstart Tutorial link)


     RDD ( Resilient Distributed Dataset)
     -------------------------------------

	-> A distributed collection of in-memory partitions
		-> Each partition is a collection of objects

	-> RDDs are immutable (you can not change the content of RDDs)

	-> RDD has two components:
		1. Lineage DAG  : meta data info about the logical plan on how to create the RDD
		2. Data		: in-memory partitions

	-> RDDs follow lazy evaluation.
		-> Transformations does not cause execution. 
		-> Only action commands cause execution.
		
    
    How to create RDDs ?
    ---------------------

	Three ways:

	1. Create an RDD from some external text file

		rdd1 = sc.textFile( <filePath> )

		-> The default number of partitions is decided by "sc.defaultMinPartitions", whose
		   value is 2 if you have atleast two cores.

		rddFile = sc.textFile( file, 4 )   // 4 partitions are created.

	2.  Create an RDD from programmatic data. 
	
		rdd1 = sc.parallelize( range(1, 101) )

		-> The default number of partitions is decided by "sc.defaultParallelism", whose
		   value is equal to the number of cores allocated to your application.

		rdd1 = sc.parallelize( range(1, 101), 3 ) // 3 partitions are created.

	3. By applying transformation on existing RDDs we can create new RDD.

		rdd2 = rdd1.map( ... )


    What can you do with RDDs ?
   ----------------------------

	Only Two things:

	1. Transformations
	    -> Transformations does not cause execution 
	    -> Transformations cause only the lineage DAG to be created at the driver side.

	2. Actions
	    -> Triggers the actual execution of the RDDs and some output is generated
	    -> Causes the driver the convert the logical plan to a physical execution ad several
	       tasks are sent to the executor.	

    RDD Lineage DAG
    ----------------  
    
	NOTE: Use rdd1.getNumPartitions() to see the partition count

	-> Is a logical plan maintained by the driver which contains all the dependencies (parent RDDs)
           that caused the creation of this RDD all the way from the very first RDD.

	rdd1 = sc.textFile(filePath, 4)
		Lineage DAG: rdd1 -> sc.textFile	

    	rdd2 = rdd1.map(lambda x: x.upper())
		Lineage DAG: rdd2 -> rdd1.map -> sc.textFile

	rdd3 = rdd2.flatMap(lambda x: x.split(" "))
   		Lineage DAG: rdd3 -> rdd2.flatMap -> rdd1.map -> sc.textFile

	rdd4 = rdd3.map(lambda x: (x, 1))
		Lineage DAG: rdd4 ->  rdd3.map -> rdd2.flatMap -> rdd1.map -> sc.textFile

    
    RDD Persistence
    ---------------	
	rdd1 = sc.textFile( ..., 10 )
	rdd2 = rdd1.t2(..)
	rdd3 = rdd1.t3(..)
	rdd4 = rdd3.t4(..)
	rdd5 = rdd3.t5(..)
	rdd6 = rdd5.t6(..)  
	rdd6.persist(StorageLevel.MEMORY_ONLY)    --> instruction to spark to persist the RDD partitions.
	rdd7 = rdd6.t7(..)

	rdd6.collect()
	Lineage of rdd6 :  rdd6 -> rdd5.t6 -> rdd3.t5 -> rdd1.t3 -> sc.textFile
	sc.textFile (rdd1) -> t3 (rdd3) -> t5 (rdd5) -> t6 (rdd6) --> collect()

	rdd7.collect()
	Lineage of rdd7 :  rdd7 -> rdd6.t7
	rdd6 -> t7 (rdd7) -> collect()
	
	Commands
	--------
		rdd1.persist()  // default storage level
		rdd1.persist( StorageLevel.MEMORY_AND_DISK )
		rdd1.cache()
		
		rdd1.unpersist()

	Storage levels
        --------------
	1. MEMORY_ONLY		-> deserialised format, memory only
	2. MEMORY_AND_DISK	-> deserialised format
	3. DISK_ONLY
	4. MEMORY_ONLY_SER	-> serialized format
	5. MEMORY_AND_DISK_SER
	6. MEMORY_ONLY_2
	7. MEMORY_AND_DISK_2


    Executor Memory Structure
    --------------------------
	Reference URL: https://spark.apache.org/docs/latest/configuration.html

	Let us assume, we request executors with 10 GB RAM.
	
	-> Cluster Manager allocates exectors with 10.3 GB RAM

	1. Reserved Memory  : 300 MB
		-> Spark's internal usage. 

	2. Spark Memory (spark.memory.fraction: 0.6) => 6 GB
		-> Used for RDD execution and storage

		2.1 Execution Memory
			-> Used for execution of RDD tasks and creating RDD partitions. 

		2.2 Storage Memory (spark.memory.storageFraction = 0.5)  => 3 GB
			-> Used for RDD persistence and storing broadcast variables.

            -> Storage memory can not evict execution memory even if execution memory is
               using more than its 3 GB limit. It has to wait until more memory becomes 
	       available.

	    -> Execution memory can evict some partitions from storage, if it requires more
	       memory. But, it can evict only excess portion that is used by storage beyond its
 	       3 GB limit. 

	3. User Memory (1 - spark.memory.fraction = 0.4) => 4 GB
		-> Used for user code (Python/Scala/Java etc)

   Types of Transformations
   ------------------------

	1. Narrow Transformations
	    -> Does not cause shuffling of the data from one partition to other partitions
	    -> Partition to partition transformations
	    -> The output RDD will have the same number of partitions as input RDD

	2. Wide Transformation
	    -> Causes shuffling of the data
	    -> One output partition may need data from multiple input partitions
	    -> The output RDD may have different number of partitions than input RDD

   RDD Transformations
   -------------------
      
      => A transformation returns 'RDD'

    1. map			P: U -> V
				Transform each elements of the RDD by applying the function
				Element -> Element transformation
				input RDD: N objects, output RDD: N objects

    2. filter			P: U -> Boolean
				Only those elements that return Truw will be in the output RDD
				input RDD: N objects, output RDD: <= N objects

    3. glom			P: None
				Will return an list with all the elements of each partition.
	
		rdd1			rdd2 = rdd1.glom()
			
		P0 :  3,2,1,4,2 -> glom() -> P0 : [3,2,1,4,2]
		P1 :  5,2,4,3,0 -> glom() -> P1 : [5,2,4,3,0]
		P2 :  6,2,4,1,8 -> glom() -> P2 : [6,2,4,1,8]
		
		rdd1.count : 15 (int)	  rdd2.count(): 3 (array)

    4. flatMap			P: U -> Iterable[V]	(function should return some collection)
				Flattens the iterables produced by the functions
				Input RDD: N objects, output RDD: >= N objects
				
		rddWords = rddFile.flatMap(lambda x: x.split(" "))


    5. mapPartitions		P: Iterator[U] -> Iterator[V]
				Applies a function to entire partitions.


		rdd1			rdd2 = rdd1.mapPartitions( lambda x:  )
			
		P0 :  3,2,1,4,2 -> mapPartitions() -> P0 : 12
		P1 :  5,2,4,3,0 -> mapPartitions() -> P1 : 14
		P2 :  6,2,4,1,8 -> mapPartitions() -> P2 : 21

		rdd1.mapPartitions(lambda x: [sum(x)]).collect()
		rdd1.mapPartitions(lambda x: map(lambda a: a*10, x)).collect()


    6. mapPartititonsWithIndex		P: Int, Iterator[U] -> Iterator[V]
					Applies a function to entire partitions. 
					We get the partition-id as an additional function input.

		rdd1.mapPartitionsWithIndex(lambda pid, pdata: [(pid, sum(pdata))]).collect()
		rdd1.mapPartitionsWithIndex(lambda id, x: map(lambda a: (id, a*10), x)).collect()


   7. distinct			P: None, Optional: numPartitions
				Returns distict elements of the RDD.

		rddWords.distinct().collect()
		rddWords.distinct(3).collect()

    8. sortBy			P: U -> V
				The objects of the RDD are sorted based on the value of the function output
				Input RDD: N objects, output RDD: >= N objects

		rdd1.sortBy(lambda x: x%3).collect()
		rdd1.sortBy(lambda x: x%3, False).collect()
		rdd1.sortBy(lambda x: x%9, True, 5)
		

    Types of RDDs
    -------------
	-> Generic RDDs:   RDD[U]
	-> Pair RDDs:      RDD[(U, V)]			

    9. mapValues		P: U -> V
				Applied only to Pair RDDs
				Transforms the 'value' part of the (k, v) pairs by applying the function.

   10. groupBy			P: U -> V
				RDD elements are grouped by function output.
				Returns a pair RDD where:
				   key: unique-values of the function output
				   value: ResultIterable object containing all objects of the RDD that produced
					  the key.
				
		rddWordcount = rdd1.flatMap(lambda x: x.split(" ")) \
                   		.groupBy(lambda x: x) \
                   		.mapValues(len) \
                   		.sortBy(lambda x: x[1], False, 1)

		rdd1.groupBy(lambda x: x%5)
		rdd1.groupBy(lambda x: x%5, 4)	


    11. randomSplit		P: Array of ratios (ec: [0.6, 0.4])
				Returns an array of RDDs randomly split from the input RDD in the
				given ratios.

		rddArr = rdd1.randomSplit([0.4, 0.3, 0.3])
		rddArr = rdd1.randomSplit([0.5, 0.5], 456)


    12. repartition		P: # of partitions
				Used to increase or decrease number of partitions of the output RDD. 
				Results in global shuffle

		rdd2 = rdd1.repartition(10)   -> can increase or decrease

    13. coalesce		P: # of partitions
				Used to only decrease number of partitions of the output RDD.
				Results in partition merging

		rdd2 = rdd1.coalesce(5)   -> can only decrease  			


    recommendations & best practices
    ---------------------------------
	-> The size of each partition should be around 128 MB  (100MB to 150MB)
	-> Have atleast/minimum as many partitions as there are CPU cores
		-> You can go to 1x to 4x times the number of cores.
		-> Make sure your number of partitions is a multiple of number of cores.
	-> The number of partitions is close to 2000, bump it upto 2000.	
	-> The number of cores in each executor should be 5.


    14. partitionBy		P: # of partitions, optional: partitioning function
				Applied only to pair RDDs
				Used to control which elements goes to which partitions based on the key. 

		rdd2 = rdd1.partitionBy(4)   # default Hash partitioning is used.

		rdd2 = rdd1.partitionBy(3, custom_partitioner)   
		rdd2.glom().collect()


    15. union, intersection, subtract, cartesian

	 let us say rdd1 has M partitions and rdd2 has N partitions

	 command			# partitions
	--------------------------------------------
	 rdd1.union(rdd2)		M + N, narrow
	 rdd1.intersection(rdd2)	M + N, wide
	 rdd1.subtract(rdd2)		M + N, wide
	 rdd1.cartesian(rdd2)		M * N, wide

	
     ..ByKey transformations
	
	-> Are all wide transformations
	-> All are applied only on Pair RDD
	-> They do some operation based on the key.


     16. sortByKey			P: None, Optional: Ascending (True/False), numPartitions
					Sorts the elements of the RDD by Key

		rddPairs.sortByKey()
		rddPairs.sortByKey(False)
		rddPairs.sortByKey(True, 8)
		rddPairs.sortByKey(numPartitions = 8)

     17. groupByKey			P: None, Optional: numPartitions
					Groups the RDD by key, so that the output RDD has unique keys
					and grouped values.
					RDD[(U, V)].groupByKey() => RDD[(U, ResultIterable[V])]
					Avaiod this if possible
					
		
		rddWordcount = rdd1.flatMap(lambda x: x.split(" ")) \
				.map(lambda x: (x, 1))
                   		.groupByKey() \
                   		.mapValues(sum)

		rddPairs.groupByKey()
		rddPairs.groupByKey(3)  // 3 output partitions


      18. reduceByKey			P: (U, U) -> U, Optional: numPartitions
					Reduces all the different values of each unique key by 
					iterativly applying the reduce function.

		rddWordcount = rdd1.flatMap(lambda x: x.split(" ")) \
				.map(lambda x: (x, 1))
                   		.reduceByKey(lambda x, y: x + y)


      19. aggregateByKey		Returns an RDD where all values each each unique key are aggregated
					to a value which is of different type than that ofRDD element's values


		rdd2 = student_rdd.map(lambda x: (x[0], x[2])) \
        			.aggregateByKey( (0,0),
                         			lambda z, v: (z[0] + v, z[1] + 1),
                         			lambda a, b: (a[0] + b[0], a[1] + b[1])) \
        			.mapValues(lambda x: x[0]/x[1])



      20. Joins	   		-> join, leftOuterJoin, rightOuterJoin, fullOuterJoin
				-> RDD[(U, V)].join( RDD[(U, W)] ) -> RDD[(U, (V, W))]

		join = names1.join(names2)   #inner Join
		leftOuterJoin = names1.leftOuterJoin(names2)
		rightOuterJoin = names1.rightOuterJoin(names2)
		fullOuterJoin = names1.fullOuterJoin(names2)

      21. cogroup		Is used to join RDDs with duplicate keys.
				=> groupByKey + fullOuterJoin 

	   
  RDD Actions
  ------------

	1. collect

	2. count

	3. saveAsTextFile 

	4. reduce		P: (U, U) -> U
				Reduces the entire into one final value.

		P0: 6, 2, 3, 5, 4, 9, 8		=> 9 => 9
		P1: 4, 6, 1, 0, 6, 0, 2		=> 6
		P2: 0, 4, 1, 0, 2, 3, 4, 5	=> 5

		rdd1.reduce(lambda x, y : x if  (x > y) else y)
		rdd1.reduce(lambda x, y : x + y)

	5. aggregate		Reduces the entire RDD to one value of a type different from the
				elements of the RDD.

	6. take(n)       -> returns a list with first n elements of the RDD

	7. takeOrdered	 -> returns a list with first n elements of the RDD sorted by the value or based 
			    ona custom sorting function.

			rdd1.takeOrdered(10)
			rdd1.takeOrdered(10, lambda x: x%2)

	8. takeSample

			rdd1.takeSample(True, 8)	// with replacement sampling
			rdd1.takeSample(True, 8, 44)    // where 44 is a seed
			rdd1.takeSample(False, 8)	// without replacement sampling

	9. countByValue

	10. countByKey

	11. first

	12. foreach	-> Does not return anything
			-> Runs some function on all the elements of the RDD.


	13. saveAsSequenceFile

 
   Use-Case
   --------
	From cars.tsv dataset. find the averahe weight of each make from American Cars
	Arrange the data in the DESC order of average weight
	Save the output in one text file. 

	def seq_fun(zv, el) :
    		return (zv[0] + el, zv[1] + 1)

	def comb_fun(a, b) :
    		return (a[0] + b[0], a[1] + b[1])
    
	output = sc.textFile(carsFile, 4) \
            .map(lambda s: s.split("\t")) \
            .filter(lambda cars: cars[9] == "American") \
            .map(lambda arr: (arr[0], int(arr[6]))) \
            .aggregateByKey( (0,0), seq_fun, comb_fun) \
            .mapValues(lambda x: x[0]/x[1]) \
            .sortBy(lambda t: t[1], False, 1)

	output.saveAsTextFile("E:\\PySpark\\output\\cars")	


  Closures
  ---------
	Closure is those variables and methods which must be visible for the executor to performs its
	computations on the RDD.

	This closure is serialized and sent to each executor.
	
		rdd1 = sc.parallelize( range(1, 40000), 4 )

		counter = 0

		def isPrimeNumber( n ):
			returns 0 of n is NOT prime
			return 1 if n IS prime.

		def f1(n) :
			global counter
			if ( isPrimeNumber(n) == 1 ) counter += 1
			return n*10;

		rdd2 = rdd1.map( f1 )

		rdd2.collect()

		print(counter)    // print 0

	IMP: We can not use "local variables" to capture the global state of RDD. Hence we can not 
	implement any counter using local varibles.
		-> use "Accumulators" to solve the above problem.


  Shared Variables
  ----------------

	Sharaed Variables:

	1. Accumulators       

		rdd1 = sc.parallelize( range(1, 40000), 4 )

		counter = sc.accumulator(0)

		def isPrimeNumber( n ):
			returns 0 of n is NOT prime
			return 1 if n IS prime.

		def f1(n) :
			global counter
			if ( isPrimeNumber(n) == 1 ) counter.add(1)
			return n*10;

		rdd2 = rdd1.map( f1 )

		rdd2.collect()

		print(counter)    // print 0


	Broadcast Variable
        ------------------
	-> A large immutable collection (such as a large list or dictionary) can be converted
	   into a broadcast variable.
	
	-> A broadcast variable is not a part of function closure. Only one copy of the variable
	   is stored in storage-memory and all task will lookup from this one copy.
                 -> This will save a lot of execution memory.	


		d = sc.broadcast({1: a, 2: b, 3: c, 4: d, .....})       

		def f1(n) :
			global d
			return d.value[n]

		rdd1 = sc.parallelize([1,2,3,4, ...], 4)
		rdd2 = rdd1.map( f1 )

   spark-submit
   ------------
      -> Is a single command that is used to submit any spark program (pyspark, scala, java, R) 
	 to any cluster manager (local, spark standalone, yarn, mesos, kubernetus).

	spark-submit --master yarn
		--deploy-mode cluster
		--driver-memory 2G
		--executor-memory 10G
		--driver-cores 4
		--executor-cores 5
		--num-executors 20
		wordcount.py <command-line-args>

	spark-submit --master local[2] E:\spaspark-core\worcount.py wordcount.txt wcout

 
   ----------------
     Spark SQL
   ----------------

      -> Structured data processing API

	      Structured file formats:  Parquet (default), ORC, JSON, CSV (delimited text file)
	      JDBC data source: RDBMS Databases, NoSQL databases
	      Hive (data warehousing platform on Hadoop)

     -> SparkSession
              -> Starting point of execution.
	      -> represents a user-session inside an application (represented by a SparkContext)
	      -> Within a SparkContext, we can have multiple SparkSessions.
		
 		spark = SparkSession \
        		.builder \
        		.appName("Dataframe Operations") \
        		.config("spark.master", "local[*]") \
        		.getOrCreate() 

		spark.conf.set("spark.sql.shuffle.partitions", "10")

     -> DataFrame
	     -> Data abstraction of SparkSQL	     
	     -> Collection of distributed in-memory partitions that are immutable & lazy-evaluated. 
	     -> DataFrame is a collection of "Row" objects

	     -> DataFrame operations are very efficient compared to plain RDD
		-> This is because SparkSQL engine can apply a lot of cpu & memory optimizations
		   before creating the physical execution plan. 
		-> Optimizers:  Catalyst, Tungston.

	     -> DataFrame
		    -> Data   : Collection of Row objects
		    -> Schema : Strcuture of a row   (StructType object)

			StructType(
			     List(
				StructField(age,LongType,true),
				StructField(gender,StringType,true),
				StructField(name,StringType,true),
				StructField(phone,StringType,true),
				StructField(userid,LongType,true)
			     )
			)

     
  
   Steps in a Spark SQL Application:
   --------------------------------

	1. Read/load the data from some external data source (file, database. or programmatic data etc.)
	   into  a DataFrame.

		df1 = spark.read.format("json").load(inputFilePath)
		df1 = spark.read.json(inputFilePath)

	2. Apply transformations on the Dataframe using DataFrame API or by using SQL.

		DataFrame API:

			df2 = df1.select("userid", "name", "gender", "age") \
         			.where("age is not null") \
         			.groupBy("age").count() \
         			.orderBy("count", "age") \
         			.limit(4)

		Using SQL:

			df1.createOrReplaceTempView("users")

			qry = """select age, count(*) as count
         			from users
         			where age is not null
         			group by age
         			order by count, age
         			limit 4"""

			df3 = spark.sql(qry)
		
	
	3. Write/save the dataframe in some structured file format or into a structured data store. 
	
		df2.write.format("json").save(outputDirPath)
		df2.write.json(outputDirPath)


   LocalTempViews & GlobalTempViews
   ---------------------------------
	df1.createOrReplaceTempView("users")

        // global temp views
	df1.createGlobalTempView("gusers")

	qry = """select age, count(*) as count 
        	 from global_temp.gusers 
         	where age is not null
         	group by age
         	order by count
         	limit 4"""
         
	df3 = spark.sql(qry)
	df3.show()


	Local Temp View :
		-> Created in SparkSession's local catalog.
			df1.createOrReplaceTempView("users")
		-> Accessible only from with in that sparksession.

	Global Temp View:
		-> Created at application scope
			df1.createGlobalTempView("gusers")
		-> Accessible from any sparksession in that application.

	
   DataFrame Transformations
   -------------------------

   1. select
		df2 = df1.select("userid", "name", "gender", "age")

		df2 = df1.select( col("DEST_COUNTRY_NAME").alias("destination"),
                  	column("ORIGIN_COUNTRY_NAME").alias("origin"),
                  	expr("count"),
                  	expr("count + 10 as newCount"),
                  	expr("count > 200 as highFrequency"),
                  	expr("DEST_COUNTRY_NAME = ORIGIN_COUNTRY_NAME as domestic")
                 )

   2. where / filter

		df3 = df2.where("age is not null")
		df3 = df2.filter("age is not null")

   3. groupBy (with aggregation methods)
 
		df3 = df2.groupBy("domestic", "highFrequency").count()
		df3 = df2.groupBy("domestic", "highFrequency").sum("count")
		df3 = df2.groupBy("domestic", "highFrequency").max("count")
		df3 = df2.groupBy("domestic", "highFrequency").avg("count")

		df3 = df2.groupBy("domestic", "highFrequency") \
        		.agg( count("count").alias("count"),
             			sum("count").alias("sum"),
             			max("count").alias("max"),
             			avg("count").alias("avg"))


   4. orderBy / sort

		df3 = df1.orderBy("count", "age")
		df3 = df2.orderBy(asc("count"), desc("destination"))

   5. limit
	
		df1.limit(4)

   6. selectExpr

		df2 = df1.selectExpr( "DEST_COUNTRY_NAME as destination",
                  "ORIGIN_COUNTRY_NAME as origin",
                  "count",
                  "count + 10 as newCount",
                  "count > 200 as highFrequency",
                  "DEST_COUNTRY_NAME = ORIGIN_COUNTRY_NAME as domestic"
                 )

   7. withColumn  

	df3 = df1.withColumn( "count2",  col("count") + 10 ) \
        	.withColumn( "highFrequency",  col("count") > 20) \
        	.withColumn( "domestic",  expr("DEST_COUNTRY_NAME = ORIGIN_COUNTRY_NAME") )


   8. withColumnRenamed
	df4 = df3.withColumnRenamed("DEST_COUNTRY_NAME", "destination") \
        	.withColumnRenamed("ORIGIN_COUNTRY_NAME", "origin")


   9. drop
   	df4 = df3.drop("count2", "highFrequency")

   10. union
	df2 = df1.where("count > 1000")
	df3 = df1.where("count > 1100")

	df4 = df2.union(df3)


   11. distinct
	df5 = df1.select("ORIGIN_COUNTRY_NAME").distinct()


   12. randomSplit
	df10, df11 = df1.randomSplit([0.7, 0.3], 64)
	df10.count()
	df11.count()

   13. sample
	df10 = df1.sample(True, 0.6)
	df10 = df1.sample(True, 1.6, 54)
	df10 = df1.sample(False, 0.6)
	df10 = df1.sample(False, 1.6) // this is invalis as the fraction range should be [0,1]

   14. repartition
	df2 = df1.repartition(10)
	df2 = df1.repartition(4)
	df2 = df1.repartition(col("DEST_COUNTRY_NAME"))   // hash partitioned by given col.
	// number of partitions is governed by "sparl.sql.shuffle.partitions" whose default value is 200.
        // spark.conf.set("spark.sql.shuffle.partitions", "20")
	df2 = df1.repartition(8, col("DEST_COUNTRY_NAME"))

   15. coalesce	
	df5 = df4.coalesce(4)

    16. join
	   -> discussed separatly.


  show comand:
  -----------
		df1.show()		-> shows the first 20 rows
		df1.show(50)		-> shows the first 50 rows
		df1.show(50, False)     -> shows the first 50 rows, columns not truncated.

   Working with different file formats
   -----------------------------------

	JSON:

		read:
			df1 = spark.read.format("json").load( dataPath )
			df1 = spark.read.json( dataPath )

		write:
			df1.write.format("json").save( outputDirPath )
			df1.write.json( outputDirPath )

	Parquet:

		read:
			df1 = spark.read.format("parquet").load( dataPath )
			df1 = spark.read.parquet( dataPath )

		write:
			df1.write.format("parquet").save( outputDirPath )
			df1.write.parquet( outputDirPath )

	ORC:

		read:
			df1 = spark.read.format("orc").load( dataPath )
			df1 = spark.read.orc( dataPath )

		write:
			df1.write.format("orc").save( outputDirPath )
			df1.write.orc( outputDirPath )

	CSV:
		read:
			df1 = spark.read.csv(inputFilePath, header=True, inferSchema=True)
			df1 = spark.read.csv(inputFilePath, header=True, inferSchema=True, sep="|")

		writing
			df2.write.mode("overwrite").csv(outputDirPath)
			df2.write.mode("overwrite").csv(outputDirPath, header=True)
			df2.write.mode("overwrite").csv(outputDirPath, header=True, sep="|")
	

  
   SaveModes
   ---------
	-> defines the behaviour when writing to an existing directory.

	1. errorIfExists   (default)
	2. ignore
	3. append
	4. overwrite

	=> df2.write.mode("overwrite").json(outputDirPath)


   Converting a DataFrame to RDD
   -----------------------------
	rdd1 = df1.rdd


    
   Creating DataFrames from Python collections
   -------------------------------------------
	listUsers = [(1, "Raju", 45),
             (2, "Ramesh", 35),
             (3, "Raghu", 25),
             (4, "Ravi", 35),
             (5, "Radhika", 40),
             (6, "Rajeev", 25)]

	df2 = spark.createDataFrame(listUsers).toDF("id", "name", "age")


   Creating DataFrames from RDD
   -----------------------------
	rdd1 = spark.sparkContext.parallelize(listUsers)
	df2 = spark.createDataFrame(rdd1).toDF("id", "name", "age")
       

   Creating DataFrames from RDD with custom schema
   ------------------------------------------------
	rddRows = rdd1.map(lambda t: Row(t[0], t[1], t[2]))
	rddRows.collect()

	mySchema = StructType([
            StructField("id", IntegerType(), True),
            StructField("name", StringType(), True),
            StructField("age", IntegerType(), True)
        ])

	df2 = spark.createDataFrame(rddRows, mySchema)


   Programmatically applying schema while creating a DF from source file
   ----------------------------------------------------------------------
	inputFilePath = "E:\\PySpark\\data\\flight-data\\json\\2015-summary.json"

	mySchema = StructType([
            StructField("ORIGIN_COUNTRY_NAME", StringType(), True),
            StructField("DEST_COUNTRY_NAME", StringType(), True),
            StructField("count", IntegerType(), True)
        ])

	df1 = spark.read.schema(mySchema).json(inputFilePath)	
	df1 = spark.read.json(inputFilePath, schema=mySchema)


  Joins
  ------

	Supoprted Joins: inner (default), left_outer, right_outer, full_outer, left_semi, left_anti

	left-Semi Join
        --------------
	=> Is like inner join, but data only comes from left-side table. 
	=> Is like the following subquery:
		select * from emp where deptid IN (select id from dept)

	left-Anti Join
        --------------
	=> Is like the following subquery:
		select * from emp where deptid NOT IN (select id from dept)



   
	data:
       -------

employee = spark.createDataFrame([
    (1, "Raju", 25, 101),
    (2, "Ramesh", 26, 101),
    (3, "Amrita", 30, 102),
    (4, "Madhu", 32, 102),
    (5, "Aditya", 28, 102),
    (6, "Pranav", 28, 100)])\
  .toDF("id", "name", "age", "deptid")
    
department = spark.createDataFrame([
    (101, "IT", 1),
    (102, "ITES", 1),
    (103, "Opearation", 1),
    (104, "HRD", 2)])\
  .toDF("id", "deptname", "locationid")  



	SQL way:    
	--------

	employee.createOrReplaceTempView("emp")
	department.createOrReplaceTempView("dept")

	spark.catalog.listTables()

	qry = """select emp.*, dept.*
         	from emp join dept
         	on emp.deptid = dept.id"""
         
	joinedDf = spark.sql(qry)

	joinedDf.show()


	DF API way
        ----------

	joinCol = employee["deptid"] == department["id"]

	joinedDf = employee.join(department, joinCol, "left_outer")
	joinedDf.show()
	
	=> Supoprted Joins: inner (default), left_outer, right_outer, full_outer, left_semi, left_anti


   Working with JDBC Sources - MySQL
   ---------------------------------
       
import os
import sys

# setup the environment for the IDE
os.environ['SPARK_HOME'] = '/home/kanak/spark-2.4.7-bin-hadoop2.7'
os.environ['PYSPARK_PYTHON'] = '/usr/bin/python'

sys.path.append('/home/kanak/spark-2.4.7-bin-hadoop2.7/python')
sys.path.append('/home/kanak/spark-2.4.7-bin-hadoop2.7/python/lib/py4j-0.10.7-src.zip')

from pyspark.sql import SparkSession

spark = SparkSession.builder \
            .appName("JDBC_MySQL") \
            .config('spark.master', 'local') \
            .getOrCreate()
  
qry = "(select emp.*, dept.deptname from emp left outer join dept on emp.deptid = dept.deptid) emp"
                  
df_mysql = spark.read \
            .format("jdbc") \
            .option("url", "jdbc:mysql://localhost/sparkdb?autoReConnect=true&useSSL=false") \
            .option("driver", "com.mysql.jdbc.Driver") \
            .option("dbtable", qry)  \
            .option("user", "root") \
            .option("password", "kanakaraju") \
            .load()
                
df_mysql.show()  

spark.catalog.listTables()

df2 = df_mysql.filter("age > 30").select("id", "name", "age", "deptid")

df2.show()   

df2.printSchema()    

df2.write.format("jdbc") \
    .option("url", "jdbc:mysql://localhost/sparkdb?autoReConnect=true&useSSL=false") \
    .option("driver", "com.mysql.jdbc.Driver") \
    .option("dbtable", "emp2")  \
    .option("user", "root") \
    .option("password", "kanakaraju") \
    .mode("overwrite") \
    .save()      
                                     
spark.stop()

      
      Working with Hive
      =================

      Hive is a Data Warehousing platform of Hadoop
    
      -> Hive Warehouse => is a directory where Hive stores the data files of all its databases and tables.
      -> Hive metastore => is (usually) an external RDBMS (such as MySQL) where Hive stores all ite metadata.


# -*- coding: utf-8 -*-
import findspark
findspark.init()

from os.path import abspath
from pyspark.sql import SparkSession
from pyspark.sql.functions import desc, avg, count

warehouse_location = abspath('spark-warehouse')

spark = SparkSession \
    .builder \
    .appName("Datasorces") \
    .config("spark.master", "local") \
    .config("spark.sql.warehouse.dir", warehouse_location) \
    .enableHiveSupport() \
    .getOrCreate()
    
spark.catalog.listTables()    

spark.sql("show databases").show()

spark.sql("drop database if exists sparkdemo cascade")
spark.sql("create database if not exists sparkdemo")
spark.sql("use sparkdemo")

spark.catalog.listTables()

spark.sql("DROP TABLE IF EXISTS movies")
spark.sql("DROP TABLE IF EXISTS ratings")
spark.sql("DROP TABLE IF EXISTS topRatedMovies")
    
createMovies = """CREATE TABLE IF NOT EXISTS 
         movies (movieId INT, title STRING, genres STRING) 
         ROW FORMAT DELIMITED 
         FIELDS TERMINATED BY ','"""
    
loadMovies = """LOAD DATA LOCAL INPATH 'E:/PySpark/data/movielens/moviesNoHeader.csv' 
         OVERWRITE INTO TABLE movies"""
    
createRatings = """CREATE TABLE IF NOT EXISTS 
         ratings (userId INT, movieId INT, rating DOUBLE, timestamp LONG) 
         ROW FORMAT DELIMITED 
         FIELDS TERMINATED BY ','"""
    
loadRatings = """LOAD DATA LOCAL INPATH 'E:/PySpark/data/movielens/ratingsNoHeader.csv' 
         OVERWRITE INTO TABLE ratings"""
         
spark.sql(createMovies)
spark.sql(loadMovies)
spark.sql(createRatings)
spark.sql(loadRatings)
    
spark.catalog.listTables()
     
#Queries are expressed in HiveQL

moviesDF = spark.sql("SELECT * FROM movies")
ratingsDF = spark.sql("SELECT * FROM ratings")

moviesDF.show()
ratingsDF.show()
           
summaryDf = ratingsDF \
            .groupBy("movieId") \
            .agg(count("rating").alias("ratingCount"), avg("rating").alias("ratingAvg")) \
            .filter("ratingCount > 25") \
            .orderBy(desc("ratingAvg")) \
            .limit(10)
              
summaryDf.show()
    
joinStr = summaryDf["movieId"] == moviesDF["movieId"]
    
summaryDf2 = summaryDf.join(moviesDF, joinStr) \
                .drop(summaryDf["movieId"]) \
                .select("movieId", "title", "ratingCount", "ratingAvg") \
                .orderBy(desc("ratingAvg")) \
                .coalesce(1)
    
summaryDf2.show()
  
summaryDf2.write.mode("overwrite").format("hive").saveAsTable("topRatedMovies")

summaryDf2.printSchema()

spark.catalog.listTables()
        
topRatedMovies = spark.sql("SELECT * FROM topRatedMovies")
topRatedMovies.show() 
    
spark.catalog.listFunctions()  
  
spark.stop()


   Use-case:
   --------- 
   From movies.csv and ratings.csv datasets fetch the top 20 movies with highest average ratings:	
   -> Consider only those movies which have atleast 30 ratings. 
   -> Data Required:  movieId, title, totalNumberOfRatings, averageRating	
   -> Arrange the data in the DESC order of averageRating	
   -> Save the output as a single pipe-separated text file.


moviesFile = "E:\\PySpark\\data\\movielens\\movies.csv"
ratingsFile = "E:\\PySpark\\data\\movielens\\ratings.csv"

moviesDf = spark.read.csv(moviesFile, header=True, inferSchema=True)
ratingsDf = spark.read.csv(ratingsFile, header=True, inferSchema=True)

moviesDf.show(5)
ratingsDf.show(5)

summaryDf = ratingsDf.groupBy("movieId") \
            .agg( count("rating").alias("totalNumberOfRatings"),
                  avg("rating").alias("averageRating")
             ) \
            .where("totalNumberOfRatings >= 30") \
            .orderBy( desc("averageRating") ) \
            .limit(20)

summaryDf.show()

joinCol = summaryDf["movieId"] == moviesDf["movieId"]

outputDf = summaryDf.join(moviesDf, joinCol) \
            .drop(moviesDf["movieId"]) \
            .select("movieId", "title", "totalNumberOfRatings", "averageRating") \
            .orderBy( desc("averageRating") ) \
            .coalesce(1)

outputDf.show()

outputDir = "E:\\PySpark\\output\\movies"

outputDf.rdd.getNumPartitions()

outputDf.write.csv(outputDir, header=True, sep="|")

===========================================
  Machine Learning & Spark MLlib
===========================================

   ML Model  => Is a learned/trained entity that is trained from historic data.
		-> Based on its learning, a model can:
			-> predict outputs on unseen data
			-> find patterns in the data
			- forecasting, projections, recommendation etc.

		-> Algorithm is an iterative mathematical computation to establish a relation between
		   the output and inputs with a goal to minimize the loss.                      
			
	            -------------------------------------
		    model = algorithm( <training data> ) 
                    --------------------------------------
		
        x (ip)		y(ip)		z (output)     prediction	error
	---------------------------------------------------------------------
	1000		500		2450		2500		-50
	1200		600		3100		3000		100
	2000		200		4250		4200		50
	1500		1000		3900		4000		-100
	1100		300		???   
	---------------------------------------------------------------------
					(Loss function)	Loss:   300/4 = 75
 
       algorithm   ==>   z = 2x + y	     Loss: 75
			 z = 1.9x + 1.2y     Loss: 70
			 z = 1.85x + 1.2y    Loss: 65
			 z = 1.8x + 1.2y     Loss: 69
			 z = 1.84x + 1.3y    Loss: 64

    Terminology
    -----------

    1. Features	 :	Inputs, independent variables, dimensions.

    2. Label	 :	Output, dependent variable

    3. Model	 :       Model has a relation inbuilt between the label and features.  
			Learned Entity, Output of algorithmic training.

    4. Algorithm :	Iterative mathematic computation that creates a model

    5. Error	 :	Difference between an actual data point and the prediction

    6. Loss	 :       Cumulative error of the entire data. 


   House Price Prediction
   ----------------------

     feature		label
     ---------------------------------------
      area (sqft)	price (lakhs of INR)
      ---------------------------------------
	1000		50
	1100		55
	1200		56
	2000		80
	1600		70
	1500		75
	1100		65
    
  
   Steps in an ML Project
   ----------------------
     1. Data Collection

     2. Data Preparation   ( > 60% time )
 
           -> prepare the data into a format that is suitable to be fit to an algorithm. 
	   -> goal: create a "feature vector" 

	   1. EDA (exploratory data analytics)
	   2. FE (Feature engineer)

	   -> All data must be numeric
	   -> There should be no nulls/empty strings etc.	   
	
    3. Train the model (using one or more algorithms)	

	   -> Take the prepared data (which contains label) and split into 70% (train) & 30% (validation) 
	      dataset 
	   -> Train the model using train dataset (70%)	

    4. Evaluate the model

	   -> Have the model predict the output on the validation dataset
	   -> By comparing the prediction with the actual output, you can evaluate the model. 

    5. Deploy the model	
		

    Types of Machine Learning
    --------------------------

     1. Supervised Learning
		
	=> Training data is labelled data (contains both label and features)

	1.1 Classification
	     => Label is one few fixed values
	        -> 1/0, True/False, [1,2,3,4,5]
		-> Survival Prediction, Email Spam Prediction, ..

	1.2 Regression
	    => Label is a continuous value
	       -> House Price Prediction. 		

     2. Unsupervised Learning

	=> Training data does not contain label.

	2.1 Clustering
	     -> Group the data into different clusters based on the patterns in the data

	2.2 Dimensionality Reduction	
	

     3. Reinforcement Learning	
	
	  => Semi Supervised Learning


   Spark MLlib
   -----------
         Machine Learning -> Spark MLlib, SciKitLearn (SKLearn), SAS, PyTorch
	 Deep Learning    -> TensorFlow, Keras, PyTorch
   
   	 Two libraries:		
		=> pyspark.mllib : legacy (based on RDDs)
		=> pyspark.ml    : current (based on DataFrames)


    Spark MLlib Contents
    ---------------------	
	1. Feature Tools : Feature Extractors, Feature Transformers, Feature Selectors.  
	2. ML Algorithms : Classification, Regression, Clustering, Collaborative Filtering etc.
	3. Pipeline	 : An approach to model building
	4. Model Selection Tools : TrainValidationSplit, CrossValidations
	5. Persistence
	6. Utilities


    Spark MLLib Building Blocks
    ---------------------------

	1. Feature Vector
	
		1.1  Dense Vector	-> Vectors.dense(0,0,0,0,9,3,5,0,0,0,0,6,0,0,3,0)
		1.2  Sparse Vector	-> Vectors.sparse(16, [4,5,6,11,14], [9,3,5,6,3])

	2. Estimator
		input: dataframe
		output: model
		method: fit

		<model> = <estimator>.fit( <dataframe> )

		Ex: All ML algorithms, Some Feature tools - StringIndexer, OneHotEncoder, RFormula, ...

	3. Transformer
		input: dataframe
		output: dataframe
		method: transform

		<outputDf> = <transformer>.transform( <inputDf> )

		Ex: All models, Lot of Feature Transformers such as tokenizer, HashingTF

	4. Pipeline

		pl = Pipeline(stages=[T1, T2, T3, E1])	
		plModel = pl.fit( df1 )		
		df1 -> T1 -> df2 -> T2 -> df3 -> T3 -> df4 -> E1 -> plModel


   Mini Project    https://www.kaggle.com/c/titanic/data
   ------------	
  
	PassengerId,Survived,Pclass,Name,Sex,Age,SibSp,Parch,Ticket,Fare,Cabin,Embarked
	1,0,3,Harris",male,22,1,0,A/5 21171,7.25,,S
	2,1,1,"Cumings, Mrs.Braund, Mr. Owen  John Bradley (Florence Briggs Thayer)",female,38,1,0,PC 17599,71.2833,C85,C
	3,1,3,"Heikkinen, Miss. Laina",female,26,0,0,STON/O2. 3101282,7.925,,S
	4,1,1,"Futrelle, Mrs. Jacques Heath (Lily May Peel)",female,35,1,0,113803,53.1,C123,S
	5,0,3,"Allen, Mr. William Henry",male,35,0,0,373450,8.05,,S
	6,0,3,"Moran, Mr. James",male,,0,0,330877,8.4583,,Q
	7,0,1,"McCarthy, Mr. Timothy J",male,54,0,0,17463,51.8625,E46,S

    	Label: Survived
	Features: Sex,Embarked	

		Numerical   : Pclass,Age,SibSp,Parch,Fare
		Categorical : Sex,Embarked
			=> StringIndexer & OneHotEncoder

 ========================================================================
   Spark Streaming
 ========================================================================
 	
       -> Spark Streaming fecilitates real-time data analytics.
	
		Two libraries:
		1. Spark Streaming  		(based on RDDs)
		2. Structured Streaming		(based on DataFrames)

        -> Spark Streaming API
		-> Micro-batch based processing that provides seconds scale latency
		-> Near-Real-Time processing	
	
        -> Nativly Supported Streaming Sources:
		=> Socket Streaming
		=> File Streaming
    
