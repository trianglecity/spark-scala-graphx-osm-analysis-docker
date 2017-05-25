
##
##	Spark Graphx OpenStreetMap Analysis on Docker
##

NOTICE 1: The source code is based on 

		[1] http://raazesh-sainudiin.github.io/scalable-data-science/db/studentProjects/01_DillonGeorge/039_OSMMap2GraphX.html

		[2] https://www.manning.com/books/spark-graphx-in-action

		[3] https://jaceklaskowski.gitbooks.io/mastering-apache-spark/content/spark-broadcast.html

		[4] http://note.yuhc.me/2015/03/graphx-pregel-shortest-path/

		[5] https://github.com/apache/spark/blob/master/graphx/src/main/scala/org/apache/spark/graphx/lib/ShortestPaths.scala
		
NOTICE 2: The implemented dijkstra algorithm has the GC overhead limit exceeded error. 

##

[1] git clone this-source-code-folder

[2] cd downloaded-source-code-folder

[3] sudo docker build -t spark-scala-graphx-analysis:01 .

	wait ... wait ... wait ... wait ...

[4] sudo docker run --rm --privileged  -i --name graphxosm -v $PWD:/home/spark_graphx  -t "spark-scala-graphx-analysis:01"  bash

[5] root@e98bc0a5eb06:/# cd /home/spark_graphx/

[6] root@e98bc0a5eb06:/home/spark_graphx#   cd example/graphx

[7] root@e98bc0a5eb06:/home/spark_graphx/example/graphx# /hadoop/hadoop-dist/target/hadoop-2.7.3/bin/hadoop fs -mkdir /temp

[8] root@e98bc0a5eb06:/home/spark_graphx/example/graphx# /hadoop/hadoop-dist/target/hadoop-2.7.3/bin/hadoop fs -put ./faroe-islands-latest.osm.pbf /temp

[9] root@e98bc0a5eb06:/home/spark_graphx/example/graphx# sbt clean compile

[10] root@e98bc0a5eb06:/home/spark_graphx/example/graphx# sbt clean package


[11] root@e98bc0a5eb06:/home/spark_graphx/example/graphx# /spark/bin/spark-submit --jars /root/.ivy2/cache/org.openstreetmap.osmosis/osmosis-core/jars/osmosis-core-0.43.1.jar,/root/.ivy2/cache/org.openstreetmap.osmosis/osmosis-pbf/jars/osmosis-pbf-0.43.1.jar,/root/.ivy2/cache/org.openstreetmap.osmosis/osmosis-osm-binary/jars/osmosis-osm-binary-0.43.1.jar,/root/.ivy2/cache/com.google.protobuf/protobuf-java/bundles/protobuf-java-2.5.0.jar,/root/.ivy2/cache/com.esri.geometry/esri-geometry-api/jars/esri-geometry-api-1.2.1.jar --executor-memory 4g --driver-memory 5g --class "Analysis" ./target/scala-2.11/graphanalysis_2.11-1.0.jar


[12] the ouput looks something like this

	
		+--------+------------------+-------------------+--------------------+
		|  nodeId|          latitude|          longitude|                tags|
		+--------+------------------+-------------------+--------------------+
		|29023724|61.455746000000005|         -6.7590335|[Akrar, Sumba (Su...|
		|29023725|62.254764300000005|         -6.5781653|[?nirnar, Klaksv?...|
		|29023727|            62.255|             -6.533|[?rnafj?r?ur, Kla...|
		|29023728|62.086051600000005|         -7.3692636|[B?ur, B?ggjar Ko...|
		|29023729|61.784322100000004|-6.6785038000000005|[Dalur, H?sav?k (...|
		|29023730|62.286638800000006|         -6.5299448|[Depil, Hvannasun...|
		|29023731|        61.6849993|         -6.7558718|[D?mun, Sk?voy (S...|
		|29023732|62.300426400000006|-7.0894368000000005|[Ei?i, Eysturoy, ...|
		|29023733|62.281417700000006|          -6.911594|[Elduv?k, Elduv?k...|
		|29023734|         61.524861| -6.878636800000001|[F?mjin, F?mjin (...|
		|29023735|61.547000000000004| -6.771000000000001|[Fro?ba, Tv?royri...|
		|29023736|62.244022900000004|         -6.8141971|[Fuglafj?r?ur, Fu...|
		|29023737|62.237829100000006|          -6.929513|[Funningsfj?r?ur,...|
		|29023738|        62.2869145|         -6.9680448|[Funningur, Funni...|
		|29023739|62.110486800000004|         -7.4370516|[G?sadalur, B?ggj...|
		|29023740|62.130804700000006|          -6.725463|[Glyvrar, Eysturo...|
		|29023741| 62.32486170000001| -6.948243700000001|[Gj?gv, Gj?gv (Ey...|
		|29023742|        62.1918816| -6.748710300000001|[G?tugj?gv, G?tu ...|
		|29023743|62.175498000000005| -6.768989400000001|[G?tuei?i, G?tu K...|
		|29023744|62.270046300000004|         -6.6026952|[Haraldssund, Kun...|
		+--------+------------------+-------------------+--------------------+


		+-------+--------------------+--------------------+
		|  wayId|                tags|               nodes|
		+-------+--------------------+--------------------+
		|4965671|[Vagli?, resident...|[564832208, 41719...|
		|4965672|[Koparg?ta, yes, ...|[564832205, 61261...|
		|4965675|[Var?ag?ta, no, r...|[32884979, 329474...|
		|4965713|[Havnarg?ta, resi...|[32885555, 328976...|
		|4965800|[V?gsbotnur, resi...|[32885827, 328858...|
		|4965804|[Gr?ms Kambans g?...|[32885937, 328859...|
		|4967426|[Sk?latr??, resid...|[32897683, 328976...|
		|4967428|[Havnarg?ta, yes,...|[32897700, 539127...|
		|4967429|[Yviri vi? Strond...|[32897704, 303582...|
		|4967430|   [residential, 50]|[32897701, 328977...|
		|4967432|[Yviri vi? Strond...|[32897703, 328977...|
		|4967433|[Hv?tanesvegur, s...|[32897795, 328977...|
		|4967435|[Hv?tanesvegur, y...|[32897941, 566451...|
		|4967437|[10, Hv?tanesvegu...|[32897939, 369792...|
		|4967439|[Vegurin Langi, n...|[32897941, 328980...|
		|4967440|[Sundsvegur, no, ...|[2184401620, 4710...|
		|4967441|[V.U. Hammershaim...|[32898218, 328982...|
		|4972589|[B?kjarabrekka, n...|[32946858, 329468...|
		|4972590|[R.C. Effers?es g...|[32946858, 329468...|
		|4972595|[Egholmstr??, no,...|[363849135, 36384...|
		+-------+--------------------+--------------------+
		
		+---------+--------+
		|    value|count(1)|
		+---------+--------+
		| 32898027|       1|
		| 33253241|       1|
		| 33253250|       1|
		|302771931|       3|
		|302771941|       1|
		|394860517|       1|
		|305452274|       1|
		|305452307|       1|
		|307696638|       1|
		|307702309|       1|
		|307705173|       1|
		|307705458|       1|
		|569830699|       2|
		|310014716|       1|
		|316536686|       2|
		|316536768|       1|
		|793070637|       1|
		|466326950|       1|
		|331546657|       1|
		|331546684|       1|
		+---------+--------+
		only showing top 20 row	

		+----------+
		|     value|
		+----------+
		| 302771931|
		| 569830699|
		| 316536686|
		| 563848141|
		|3452032799|
		| 438222804|
		| 567217965|
		| 567231100|
		| 432697135|
		| 564832208|
		| 305452188|
		| 392519975|
		| 410039023|
		| 475339609|
		| 485658054|
		| 559478134|
		| 566650007|
		| 566811424|
		| 316463628|
		| 331545676|
		+----------+
		only showing top 20 rows

		+--------+------------------+-------------------+----+
		|  nodeId|          latitude|          longitude|tags|
		+--------+------------------+-------------------+----+
		|32884410|        62.0101718|-6.7717803000000005|  []|
		|32884411|62.010621500000006| -6.772577600000001|  []|
		|32884412| 62.01116880000001| -6.773545800000001|  []|
		|32884414|         62.012163|         -6.7747314|  []|
		|32884935| 62.01234530000001|             -6.775|  []|
		|32884936|         62.012447|         -6.7752521|  []|
		|32884937|62.012517800000005|         -6.7755269|  []|
		|32884938| 62.01255510000001|         -6.7757439|  []|
		|32884939|62.012570200000006|         -6.7758368|  []|
		|32884940|        62.0126135|         -6.7759818|  []|
		|32884941|         62.012689|         -6.7761408|  []|
		|32884943| 62.01018070000001|          -6.771522|  []|
		|32884944| 62.01014060000001|         -6.7711926|  []|
		|32884945|        62.0100783| -6.770889800000001|  []|
		|32884972|62.009958100000006|         -6.7706049|  []|
		|32884973|        62.0098245|         -6.7702755|  []|
		|32884974|62.009625500000006|         -6.7699334|  []|
		|32884975|        62.0094963|         -6.7697532|  []|
		|32884976| 62.00925110000001| -6.769325500000001|  []|
		|32884977|        62.0095668|-6.7695525000000005|  []|
		+--------+------------------+-------------------+----+

		
		+-------+--------------------+
		|  wayId|        labeledNodes|
		+-------+--------------------+
		|4965671|[[564832208,true]...|
		|4965672|[[564832205,true]...|
		|4965675|[[32884979,true],...|
		|4965713|[[32885555,true],...|
		|4965800|[[32885827,true],...|
		|4965804|[[32885937,true],...|
		|4967426|[[32897683,true],...|
		|4967428|[[32897700,true],...|
		|4967429|[[32897704,true],...|
		|4967430|[[32897701,true],...|
		|4967432|[[32897703,true],...|
		|4967433|[[32897795,true],...|
		|4967435|[[32897941,true],...|
		|4967437|[[32897939,true],...|
		|4967439|[[32897941,true],...|
		|4967440|[[2184401620,true...|
		|4967441|[[32898218,true],...|
		|4972589|[[32946858,true],...|
		|4972590|[[32946858,true],...|
		|4972595|[[363849135,true]...|
		+-------+--------------------+
		only showing top 20 rows
		
		+-------+---+
		|     _1| _2|
		+-------+---+
		|4965671|  2|
		|4965672|  3|
		|4965675|  6|
		|4965713|  2|
		|4965800|  2|
		|4965804|  2|
		|4967426|  2|
		|4967428|  4|
		|4967429|  5|
		|4967430|  2|
		|4967432|  2|
		|4967433|  3|
		|4967435|  4|
		|4967437|  2|
		|4967439|  3|
		|4967440|  4|
		|4967441|  3|
		|4972589|  2|
		|4972590|  4|
		|4972595|  2|
		+-------+---+
		only showing top 20 rows


		+-------+--------------------+
		|     _1|                  _2|
		+-------+--------------------+
		|4965671|[[564832208,Wrapp...|
		|4965672|[[564832205,Wrapp...|
		|4965675|[[32884979,Wrappe...|
		|4965713|[[32885555,Wrappe...|
		|4965800|[[32885827,Wrappe...|
		|4965804|[[32885937,Wrappe...|
		|4967426|[[32897683,Wrappe...|
		|4967428|[[32897700,Wrappe...|
		|4967429|[[32897704,Wrappe...|
		|4967430|[[32897701,Wrappe...|
		|4967432|[[32897703,Wrappe...|
		|4967433|[[32897795,Wrappe...|
		|4967435|[[32897941,Wrappe...|
		|4967437|[[32897939,Wrappe...|
		|4967439|[[32897941,Wrappe...|
		|4967440|[[2184401620,Wrap...|
		|4967441|[[32898218,Wrappe...|
		|4972589|[[32946858,Wrappe...|
		|4972590|[[32946858,Wrappe...|
		|4972595|[[363849135,Wrapp...|
		+-------+--------------------+

		// root
 		// |-- _1: long (nullable = true)
		// |-- _2: array (nullable = true)
		// |    |-- element: struct (containsNull = true)
		// |    |    |-- _1: long (nullable = true)
		// |    |    |-- _2: array (nullable = true)
		// |    |    |    |-- element: long (containsNull = false)
		// |    |    |-- _3: array (nullable = true)
		// |    |    |    |-- element: long (containsNull = false)
		// 

		// for each (intersection_nodeId, inBuf, outBuf) => (wayId, IntersectionNode(intersection_nodeId, inBuf, outBuf))

		//+-------+--------------------+
		//|     _1|                  _2|
		//+-------+--------------------+
		//|4965671|[564832208,Wrappe...|
		//|4965671|[32884943,Wrapped...|

		//|4965672|[564832205,Wrappe...|
		//|4965672|[612617149,Wrappe...|
		//|4965672|[612617145,Wrappe...|

		//|4965675|[32884979,Wrapped...|
		//|4965675|[331930182,Wrappe...|
		//|4965675|[331930287,Wrappe...|
		//|4965675|[331930301,Wrappe...|
		//|4965675|[331930330,Wrappe...|
		//|4965675|[306613927,Wrappe...|

		//|4965713|[32885555,Wrapped...|
		//|4965713|[32885559,Wrapped...|

		//|4965800|[32885827,Wrapped...|
		//|4965800|[32885831,Wrapped...|

		//|4965804|[32885937,Wrapped...|
		//|4965804|[32885844,Wrapped...|

		//|4967426|[32897683,Wrapped...|
		//|4967426|[32898222,Wrapped...|
		//|4967428|[32897700,Wrapped...|
		//+-------+--------------------+
		

		
		vertices
		id:324467343 has attr: Map(29439275 -> ([J@5e4a068f,[J@24112ec4), 29439280 -> ([J@6a6082b3,[J@1d591f58))
		id:3906819976 has attr: Map(387380592 -> ([J@13a91c02,[J@3e6367bd))
		id:444505024 has attr: Map(37877995 -> ([J@50f80fd8,[J@58e6940))
		id:394663688 has attr: Map(34378120 -> ([J@5e1c2cff,[J@7b67e60e), 34378146 -> ([J@6bf1b075,[J@1d430022))
		id:563805636 has attr: Map(44361935 -> ([J@2207bca1,[J@66946979))


		// +---------+---------+-------+
		// |    srcId|    dstId|   attr|
		// +---------+---------+-------+
		// |564832208| 32884943|4965671|
		// | 32884943|564832208|4965671|

		// |564832205|612617149|4965672|
		// |612617149|564832205|4965672|

		// |612617149|612617145|4965672|
		// |612617145|612617149|4965672|

		// | 32884979|331930182|4965675|
		// |331930182| 32884979|4965675|

		// |331930182|331930287|4965675|
		// |331930287|331930182|4965675|

		// |331930287|331930301|4965675|
		// |331930301|331930287|4965675|

		// |331930301|331930330|4965675|
		// |331930330|331930301|4965675|

		// |331930330|306613927|4965675|
		// |306613927|331930330|4965675|

		// | 32885555| 32885559|4965713|
		// | 32885559| 32885555|4965713|

		// | 32885827| 32885831|4965800|
		// | 32885831| 32885827|4965800|
		// +---------+---------+-------+

	
		intersection_nodeId, attr( wayId, memebr_node)
		324467343 (29439280 324467351)
		324467343 (29439280 324467347)

		394663688 (34378120 394663129)
		394663688 (34378120 394663130)
		394663688 (34378120 394663131)
		394663688 (34378120 394663132)
		394663688 (34378120 394663133)
		394663688 (34378120 394663134)
		394663688 (34378120 394663135)
		394663688 (34378120 394663136)
		394663688 (34378120 394663137)
		394663688 (34378120 394663138)
		394663688 (34378146 394663678)
		394663688 (34378146 394663679)
		394663688 (34378146 394663680)
		394663688 (34378146 394682596)
		394663688 (34378146 394663681)
		394663688 (34378146 394663682)
		394663688 (34378146 394683886)
		394663688 (34378146 394663683)
		394663688 (34378146 394663684)
		394663688 (34378146 394663685)
		394663688 (34378146 394663686)
		394663688 (34378146 394663687)

		triangle count
		(559477985,1)
		(391665674,1)
		(567231045,1)
		(313360160,1)
		(305451969,1)
		(464687933,1)
		(1685216083,1)
		(392811948,1)
		(32897701,1)
		(32884941,1)

		connected components
		//(324467343,32884411)
		//(3906819976,3906819960)
		//(444505024,32884411)
		//(394663688,32884411)
		//(563805636,563805341)
		//(559477985,32884411)
		//(559552998,32884411)
		//(563597273,316463374)
		//(559771433,32884411)
		//(559617764,32884411)
