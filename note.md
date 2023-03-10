# Intro

## Scalable

### Data management

- Scalability
  - Able to manage incresingly big volume of data
- Accessibility
  - Able to maintain efficiciency in reading & writing data (I/O) into data storage syss
- Transparency
  - In distributed environment, users should be able to access data over network as easily as if data were stored locally.
  - Users should not have to know physical location of data to access it.
- Availability
  - Fault tolerance
  - no. users, sys failures, or other consequences of distribution shouldn't compromise availability.

### Data ingestion & processing

- Data ingestion
  - Data from different complementing information syss is to be combined to gain a more comprehensive basis to satisfy need
  - How to ingest data efficiently from various, distributed heterogeneous sources?
    - Different data formats
    - Different data models & schemas
    - Security & privacy
- Data processing
  - How to process massive volume of data in a
    - timely fashion?
    - real-time fashion?
  - Traditional parallel, distributed processing (OpenMP, MPI)
    - Big learning curve
    - Scalability is limited
    - Fault tolerence is hard to achive
    - Expensive, high performance computing infrastructure
  - Novel realtime processing architecture
    - Eg. Mini-batch in Spark streaming
    - Eg. Complex event processing in Apache Flink

### Analytic algorithms

- Challenges
  - Big volume
  - Big dimensionality
  - Realtime processing
- Scaling-up Machine Learning algorithms
  - Adapting algorithm to handle Big Data in a single
    machine.
    - Eg. Sub-sampling
    - Eg. Principal component analysis
    - Eg. feature extraction & feature selection
  - Scaling-up algorithms by parallelism
    - Eg. k-nn classification based on MapReduce
    - Eg. scaling-up support vector machines (SVM) by a divide and-conquer approach

## Curse of dimensionality

- required no. samples (to achieve same accuracy) grows exponentionally with no. variables
- In practice: no. training examples is fixed!
  - classifier's performance usually will degrade for a large no. features!
  - After a certain point, increasing dimensionality of problem by adding new features would actually degrade performance of classifier.

## Utilization & interpretability of big data

- Domain expertise to findout problems & interprete analytics results
- Scalable visualization & interpretability of million data points
  - to facilitate interpretability & understanding

# Hadoop ecosys

## Goal

- L??u tr??? data kh??? m???, tin c???y
- Powerful data processing
- Efficient visualization
- Challenges
- Thi???t b??? l??u tr??? t???c ????? ch???m, m??y t??nh thi???u tin c???y, l???p tr??nh song song ph??n t??n ko d??? d??ng

## Intro

- L??u tr??? v?? x??? l?? data kh??? m???, ti???t ki???m chi ph??
  - X??? l?? data ph??n t??n v???i m?? h??nh l???p tr??nh ????n gi???n, th??n thi???n h??n nh?? MapReduce
  - Hadoop thi???t k??? ????? m??? r???ng th??ng qua k??? thu???t scale-out, t??ng s??? l?????ng m??y ch???
  - Thi???t k??? ????? v???n h??nh tr??n ph???n c???ng ph??? th??ng, c?? kh??? n??ng ch???ng ch???u l???i ph???n c???ng
- L???y c???m h???ng t??? ki???n tr??c data c???a Google

## C??c th??nh ph???n ch??nh

- L??u tr??? data: H??? th???ng t???p tin ph??n t??n Hadoop (HDFS)
- X??? l?? data: MapReduce framework
- Ti???n ??ch h??? th???ng:
  - Hadoop Common: C??c ti???n ??ch chung h??? tr??? c??c th??nh ph???n c???a Hadoop.
  - Hadoop YARN: 1 framework qu???n l?? t??i nguy??n v?? l???p l???ch trong c???m Hadoop.

## Gi???i quy???t b??i to??n

### Kh??? m???

- Thi???t k??? h?????ng "ph??n t??n" ngay t??? ?????u
  - m???c ?????nh thi???t k??? ????? tri???n khai tr??n c???m m??y ch???
- C??c m??y ch??? tham gia v??o c???m ??c g???i l?? c??c Nodes
  - M???i node tham gia v??o c??? 2 vai tr?? l??u tr??? v?? t??nh to??n
- Hadoop m??? r???ng b???ng k??? thu???t scale-out
  - C?? th??? t??ng c???m Hadoop l??n h??ng ch???c ng??n nodes

### Ch???u l???i

- V???i vi???c tri???n khai tr??n c???m m??y ch??? ph??? th??ng
  - H???ng h??c ph???n c???ng l?? chuy???n th?????ng ng??y, ko ph???i l?? ngo???i l???
  - Hadoop ch???u l???i th??ng qua k??? thu???t "d?? th???a"
- C??c t???p tin trong HDFS ??c ph??n m???nh, nh??n b???n ra c??c nodes trong c???m
  - N???u 1 node g???p l???i, data ???ng v???i nodes ???? ??c t??i nh??n b???n qua c??c nodes kh??c
- C??ng vi???c x??? l?? data ??c ph??n m???nh th??nh c??c task ?????c l???p
  - M???i task x??? l?? 1 ph???n data ?????u v??o
  - C??c task ??c th???c thi song song v???i c??c task kh??c
  - task l???i s??? ??c t??i l???p l???ch th???c thi tr??n node kh??c
- H??? th???ng Hadoop thi???t k??? sao cho c??c l???i x???y ra trong h??? th???ng ??c x??? l?? t??? ?????ng, ko ???nh h?????ng t???i c??c ???ng d???ng ph??a tr??n

## HDFS

- HDFS cung c???p kh??? n??ng l??u tr??? tin c???y v?? chi ph?? h???p l?? cho kh???i l?????ng data l???n
- T???i ??u cho c??c file k??ch th?????c l???n (t??? v??i tr??m MB t???i v??i TB)
- HDFS c?? ko gian c??y th?? m???c ph??n c???p nh?? UNIX (/hust/soict/hello.txt)
  - H??? tr??? c?? ch??? ph??n quy???n v?? ki???m so??t ng?????i d??ng nh?? c???a UNIX
- Kh??c bi???t so v???i h??? th???ng file tr??n UNIX
  - Ch??? h??? tr??? thao t??c ghi th??m data v??o cu???i t???p (APPEND)
  - Ghi 1 l???n v?? ?????c nhi???u l???n

### Ki???n tr??c

- master: name node
  - Qu???n l?? ko gian t??n v?? si??u data ??nh x??? t???p tin t???i v??? tr?? c??c chunks
  - Gi??m s??t c??c data node
- slave: data node
  - Tr???c ti???p thao t??c I/O c??c chunks

### Nguy??n l?? thi???t k??? c???t l??i

- I/O pattern
  - Ch??? ghi th??m (Append)?? gi???m chi ph?? ??i???u khi???n t????ng tranh
- Ph??n t??n data
  - T???p ??c chia th??nh c??c chunks l???n (64 MB)
    - Gi???m k??ch th?????c metadata
    - Gi???m chi ph?? truy???n data
- Nh??n b???n data
  - M???i chunk th??ng th?????ng ??c sao l??m 3 nh??n b???n
- C?? ch??? ch???u l???i
  - Data node: s??? d???ng c?? ch??? t??i nh??n b???n
  - Name node
    - S??? d???ng Secondary Name Node
    - SNN h???i data nodes khi kh???i ?????ng thay v?? ph???i th???c hi???n c?? ch??? ?????ng b??? ph???c t???p v???i primary NN

## M?? th???c x??? l?? data MapReduce

- MapReduce ko ph???i l?? ng??n ng??? l???p tr??nh, ??c ????? xu???t b???i Google
- ?????c ??i???m c???a MapReduce
  - ????n gi???n (Simplicity)
  - Linh ho???t (Flexibility)
  - Kh??? m??? (Scalability)

### A MR job = {Isolated Tasks}n

- M???i ch????ng tr??nh MapReduce l?? 1 job ??c ph??n r?? l??m nhi???u task v?? c??c task n??y ??c ph??n t??n tr??n c??c nodes kh??c nhau c???a c???m ????? th???c thi
- M???i task ??c th???c thi ?????c l???p v???i c??c task kh??c ????? ?????t ??c t??nh kh??? m???
  - Gi???m truy???n th??ng gi???a c??c node m??y ch???
  - Tr??nh ph???i th???c hi???n c?? ch??? ?????ng b??? gi???a c??c task

### Data cho MapReduce

- MapReduce trong m??i tr?????ng Hadoop th?????ng l??m vi???c v???i data c?? s???n tr??n HDFS
- Khi th???c thi, m?? ch????ng tr??nh MapReduce ??c g???i t???i c??c node ???? c?? data t????ng ???ng

### Ch????ng tr??nh MapReduce

- L???p tr??nh v???i MapReduce c???n c??i ?????t 2 h??m Map v?? Reduce
- 2 h??m n??y ??c th???c thi b???i c??c ti???n tr??nh Mapper v?? Reducer t????ng ???ng.
- Data ??c nh??n nh???n nh?? l?? c??c c???p key - value
- Nh???n ?????u v??o v?? tr??? v??? ?????u ra c??c c???p key - value
- V?? d???
  - ?????u v??o: t???p v??n b???n ch???a th??ng tin v??? order ID, employee name, & sale amount
  - ?????u ra : Doanh s??? b??n (sales) theo t???ng nh??n vi??n (employee)

### B?????c Map

- data ?????u v??o ??c x??? l?? b???i nhi???u task Mapping ?????c l???p
  - S??? task Mapping ??c x??c ?????nh theo l?????ng data ?????u v??o (~ s??? chunks)
  - M???i task Mapping x??? l?? 1 ph???n data (chunk) c???a kh???i data ban ?????u
- V???i m???i task Mapping, Mapper x??? l?? l???n l?????t t???ng b???n ghi ?????u v??o
  - V???i m???i b???n ghi ?????u v??o (key-value)
    - Mapper ????a ra 0 ho???c nhi???u b???n ghi ?????u ra (key - value trung gian)
- Trong v?? d???, task Mapping ????n gi???n ?????c t???ng d??ng v??n b???n v?? ????a ra t??n nh??n vi??n v?? doanh s??? t????ng ???ng Map phase

### B?????c shuffle & sort

- T??? ?????ng s???p x???p v?? g???p ?????u ra c???a c??c Mappers theo c??c partitions
- M???i partitions l?? ?????u v??o cho 1 Reducer Shuffle & sort phase

### B?????c Reduce

- Reducer nh???n data ?????u v??o t??? b?????c shuffle & sort
  - T???t c??? c??c b???n ghi key - value t????ng ???ng v???i 1 key ??c x??? l?? b???i 1 Reducer duy nh???t
  - Gi???ng b?????c Map, Reducer x??? l?? l???n l?????t t???ng key, m???i l???n v???i to??n b??? c??c values t????ng ???ng
- Trong v?? d???, h??m reduce ????n gi???n l?? t??nh t???ng doanh s??? cho t???ng nh??n vi??n, ?????u ra l?? c??c c???p key - value t????ng ???ng v???i t??n nh??n vi??n - doanh s??? t???ng

## C??c th??nh ph???n kh??c trong h??? sinh th??i Hadoop

- Th??nh ph???n kh??c ph???c v???
  - Ph??n t??ch data
  - T??ch h???p data
  - Qu???n l?? lu???ng
- Ko ph???i 'core Hadoop' nh??ng l?? 1 ph???n c???a h??? sinh th??i Hadoop
  - H???u h???t l?? m?? ngu???n m??? tr??n Apache

### Pig

- Cung c???p giao di???n x??? l?? data m???c cao
  - Pig ?????c bi???t t???t cho c??c ph??p to??n Join v?? Transformation
- Tr??nh bi??n d???ch c???a Pig ch???y tr??n m??y client
  - Bi???n ?????i PigLatin script th??nh c??c jobs c???a MapReduce
  - ????? tr??nh c??c c??ng vi???c n??y l??n c???m t??nh to??n

### Hive

- C??ng l?? 1 l???p tr???u t?????ng m???c cao c???a MapReduce
  - Gi???m th???i gian ph??t tri???n
  - Cung c???p ng??n ng??? HiveQL: SQL-like language
- Tr??nh bi??n d???ch Hive ch???y tr??n m??y client
  - Chuy???n HiveQL script th??nh MapReduce jobs
  - ????? tr??nh c??c c??ng vi???c n??y l??n c???m t??nh to??n

### Hbase

- CSDL c???t m??? r???ng ph??n t??n, l??u tr??? data tr??n HDFS
  - h??? qu???n tr??? CSDL c???a Hadoop
- data ??c t??? ch???c v??? m???t logic l?? c??c b???ng, bao g???m r???t nhi???u d??ng v?? c???t
  - K??ch th?????c b???ng c?? th??? l??n ?????n h??ng Terabyte, Petabyte
  - B???ng c?? th??? c?? h??ng ng??n c???t
- C?? t??nh kh??? m??? cao, ????p ???ng b??ng th??ng ghi data t???c ????? cao
  - H??? tr??? h??ng tr??m ng??n thao t??c INSERT m???i gi??y (/s)
- Tuy nhi??n v??? c??c ch???c n??ng r???t h???n ch??? khi so s??nh v???i h??? QTCSDL truy???n th???ng
  - L?? NoSQL: ko c?? ng??n ng??? truy v???n m???c cao nh?? SQL
  - Ph???i s??? d???ng API ????? scan/ put/ get/ data theo kh??a

### Sqoop

- Sqoop l?? 1 c??ng c??? cho ph??p trung chuy???n data theo kh???i t??? Apache Hadoop v?? c??c CSDL c?? c???u tr??c nh?? CSDL quan h???
- H??? tr??? import t???t c??? c??c b???ng, 1 b???ng hay 1 ph???n c???a b???ng v??o HDFS
  - Th??ng qua Map only ho???c MapReduce job
  - K???t qu??? l?? 1 th?? m???c trong HDFS ch??? c??c file v??n b???n ph??n t??ch c??c tr?????ng theo k?? t??? ph??n t??ch (vd. , ho???c \t)
- H??? tr??? export data ng?????c tr??? l???i t??? Hadoop ra b??n ngo??i

### Kafka

- Producers ko c???n bi???t Consumers
  - ?????m b???o s??? linh ho???t v?? tin c???y trong qu?? tr??nh trung chuy???n data gi???a c??c b??n
- cho ph??p ph??n t??ch m???ch l???c c??c th??nh ph???n tham gia v??o lu???ng data

### Oozie

- H??? th???ng l???p l???ch lu???ng c??ng vi???c ????? qu???n l?? c??c c??ng vi???c th???c thi tr??n c???m Hadoop
- Lu???ng workflow c???a Oozie l?? ????? th??? v??ng c?? h?????ng (Directed Acyclical Graphs (DAGs)) c???a c??c kh???i c??ng vi???c
- Oozie h??? tr??? ??a d???ng c??c lo???i c??ng vi???c
  - Th???c thi
    - MapReduce jobs
    - Pig hay Hive scripts
    - c??c ch????ng tr??nh Java ho???c Shell
  - T????ng t??c v???i data tr??n HDFS
  - Ch???y ch????ng tr??nh t??? xa qua SSH
  - G???i nh???n email

### Zookeeper

- D???ch v??? cung c???p c??c ch???c n??ng ph???i h???p ph??n t??n ????? tin c???y cao
  - Qu???n l?? th??nh vi??n trong nh??m m??y ch???
  - B???u c??? leader
  - Qu???n l?? th??ng tin c???u h??nh ?????ng
  - Gi??m s??t tr???ng th??i h??? th???ng
- ????y l?? c??c service l??i, t???i quan tr???ng trong c??c h??? th???ng ph??n t??n

### YARN - Yet Another Resource Negotiator

- Nodes c?? t??i nguy??n l?? - b??? nh??? v?? CPU cores
- ????ng vai tr?? c???p ph??t l?????ng t??i nguy??n ph?? h???p cho c??c ???ng d???ng khi c?? y??u c???u
- ??c ????a ra t??? Hadoop 2.0
  - Cho ph??p MapReduce v?? non MapReduce c??ng ch???y tr??n 1 c???m Hadoop
  - V???i MapReduce job, vai tr?? c???a job tracker ??c th???c hi???n b???i application tracker

# FSs

## Goals

### Network (Access) Transparency

- Users should be able to access files over a network as easily as if files were stored locally.
- Users should not have to know physical location of a file to access it.
- Transparency can be addressed through naming & file mounting mechanisms
  - Location Transparency
    - file name doesn't specify physical location
  - Location Independence
    - files can be moved to new physical location, no need to change references to them.
      - A name is independent of its addresses
    - ??? location transparency, but reverse is not necessarily true.

### Availability

- Files should be easily & quickly accessible.
- no. users, sys failures, or other consequences of distribution shouldn't compromise availability.
- Addressed mainly through replication.

## Architectures

- Client-Server
  - Sun Microsys Network FS (NFS), Google FS (GFS)
  - Architecture
    - 1 or more machines (file servers) manage FS.
    - Files are stored on disks at servers
    - Requests for file operations are made from clients to servers.
    - Client-server syss centralize storage & management; P2P syss decentralize it.
- Symmetric
  - Fully decentralized; based on peer-to-peer technology
  - Ivy (uses a Chord DHT approach)
  - IPFS & webtorrent

## Design issues

### Naming & name resolution

- A name space
  - collection of names
- Name resolution
  - mapping a name to an object
- 3 traditional ways
  - Concatenate host name to names of files stored on that host
  - Mount remote directories onto local directories
  - Provide a single global directory

### File Sharing Semantics

- Problem
  - When dealing with distributed FSs, we need to take into account ordering of concurrent read/write operations & expected semantics (=consistency)
- Assume open; reads/writes; close
  - UNIX semantics
    - value read is value stored by last write
    - Writes to an open file are visible immediately to others that have this file opened at same time
    - Easy to implement if 1 server & no cache.
  - Session semantics
    - Writes to an open file by a user is not visible immediately by other users that have files opened already.
    - Once a file is closed
      - changes made by it are visible by sessions started later.
  - Immutable-Shared-Files semantics
    - A sharable file cannot be modified.
    - File names cannot be reused & its contents may not be altered.
    - Simple to implement.
  - Transactions
    - All changes have all-or-nothing property
    - W1,R1,R2,W2 not allowed where P1 = W1;W2 & P2 = R1;R2

### Caching

- Server caching: in main memory
  - cache management issue, how much to cache, replacement strategy
  - still slow due to network delay
  - Used in high-performance web-search engine servers
- Client caching in main memory
  - can be used by diskless workstation
  - faster to access from main memory than disk
  - compete with virtual memory sys for physical memory space
- Client-cache on a local disk
  - large files can be cached
  - virtual memory management is simpler
  - a workstation can func even when it is disconnected from network
- tradeoffs
  - Reduces remote accesses
    - reduces network traffic & server load
  - Total network overhead is lower for big chunks of data (caching) than a series of responses to specific requests
  - Disk access can be optimized better for large requests than random disk blocks
  - Cache-consistency problem is major drawback
    - If there are frequent writes, overhead due to consistency problem is significant.

### Replication

- File data is replicated to multiple storage servers
- Goals
  - Increase reliability
  - improve availability
  - balance servers workload
- How to
  - make replication transparent?
  - keep replicas consistent?
    - a replica is not updated due to its server failure
    - network partitioned

# HDFS

### Overview

- Provides inexpensive & reliable storage for massive amounts of data
- Designed for
  - Big files (100 MB to several TBs file sizes)
  - Write once, read many times (Appending only)
  - Running on commodity hardware
- Hierarchical UNIX style FSs
  - (e.g., /hust/soict/hello.txt)
  - UNIX style file ownership & permissions

## main design principles

- I/O pattern
  - Append only
    - reduce synchronization
- Data distribution
  - File is splitted in big chunks (64 MB)
    - reduce metadata size
    - reduce network communication
- Data replication
  - Each chunk is usually replicated in 3 different nodes
- Fault tolerance
  - Data node: re-replication
  - Name node
    - Secondary Namenode
    - Standby, Active Namenodes

## Architecture

- Master/slave architecture
- master: Namenode
  - Manage namespace & metadata
  - Monitor Datanode
- slaves: Datanodes
  - Handle read/write actual data {chunks}
  - Chunks are local files in local FSs

### Namenode

- Manages FS Namespace
  - Maps a file name to a set of blocks
  - Maps a block to Datanodes where it resides
- Cluster Configuration Management
- Replication Engine for Blocks
- metadata
  - Metadata in memory
    - entire metadata is in main memory
    - No demand paging of metadata
  - Types of metadata
    - List of files
    - List of Blocks for each file
    - List of Datanodes for each block
    - File attributes, e.g. creation time, replication factor
  - A Transaction Log
    - Records file creations, file deletions etc

### Datanode

- A Block Server
  - Stores data in local FS (e.g. ext3)
  - Stores metadata of a block (e.g. CRC)
  - Serves data & metadata to Clients
- Block Report
  - Periodically sends a report of all existing blocks to Namenode
- Facilitates Pipelining of Data
  - Forwards data to other specified Datanodes
- Heartbeat
  - Datanodes send heartbeat to Namenode
    - Once every 3 seconds
  - Namenode uses heartbeats to detect Datanode failure

## Data

### Replication

- Chunk placement
  - Current Strategy
    - 1 replica on local node
    - Second replica on a remote rack
    - Third replica on same remote rack
    - Additional replicas are randomly placed
  - Clients read from nearest replicas
- Namenode detects Datanode failures
  - Chooses new Datanodes for new replicas
  - Balances disk usage
  - Balances communication traffic to Datanodes

### Rebalance

- Goal: % disk full on Datanodes should be similar
  - Usually run when new Datanodes are added
  - Cluster is online when Rebalancer is active
  - Rebalancer is throttled to avoid network congestion
  - Command line tool

### Correctness

- Use Checksums to validate data
  - Use CRC32
- File Creation
  - Client computes checksum per 512 bytes
  - Datanode stores checksum
- File access
  - Client retrieves data & checksum from Datanode
  - If Validation fails, Client tries other replicas

### Pipelining

- Client retrieves a list of Datanodes on which to place replicas of a block
- Client writes block to first Datanode
- first Datanode forwards data to next node in Pipeline
- When all replicas are written, Client moves on to write next block in file

## Secondary Name node

- Namenode is a single point of failure
- Secondary Namenode
  - Checkpointing latest copy of FsImage & Transaction Log files.
  - Copies FsImage & Transaction Log from Namenode to a temporary directory
- When Namenode restarted
  - Merges FSImage & Transaction Log into a new FSImage in temporary directory
  - Uploads new FSImage to Namenode
    - Transaction Log on Namenode is purged

## HDFS data format

### Text file

- CSV, TSV, Json records
- Convenient format to use to exchange between applications or scripts
- Human readable & parsable
- Do not support block compression
- Not as efficient to query
- Good for beginning, but not good enough for real life.

### Sequence file

- Provides a persistent data structure for binary key-value pairs
- Commonly used to transfer data between Map Reduce jobs
- Can be used as an archive to pack small files in Hadoop
- Row-based
- Compression
- Splittable
  - Support splitting even when data is compressed

### Avro

- Row based
- Supports (object) compression & splitting
- Flexible data scheme
  - Schema (JSON) included to file
- Data types
  - primitive: null, boolean, int, long, ...
  - complex: records, arrays, maps, ...
- Binary & JSON data serialization
- Data corruption detection

### Parquet

- Column-oriented binary file format
- Efficient in terms of disk I/O when specific columns need to be queried
- Supports (page) compression & splitting
- Supports nested columns (Dremel encoding)

### Optimized row columnar (ORC)

- RCFile
  - Every column is compressed individually within row group
- ORC File
  - Block-mode compression
  - Data type support
  - Ordered data store (within 1 stripe)
- Stores collections of rows & within collection row data is stored in columnar format
- Introduces a lightweight indexing that enables skipping of irrelevant blocks of rows
- Splittable: allows parallel processing of row collections
- Indices with column-level aggregated values (min, max, sum & count)

# Parallel Programming with Hadoop/MapReduce

- Typical Hadoop Cluster
- 40 nodes/rack, 1000-4000 nodes in cluster
- 1 Gbps bandwidth in rack, 8 Gbps out of rack
- Node specs :
  - 8-16 cores, 32 GB RAM, 8??1.5 TB disks

## MapReduce Programming Model

- Inspired from map & reduce operations commonly used in funcal programming languages like Lisp.
- Have multiple map tasks & reduce tasks
- Users implement interface of 2 primary methods:
  - Map: (key1, val1) ??? (key2, val2)
  - Reduce: (key2, [val2]) ??? [val3]
- Example
  - Given a file
    - A file may be divided into multiple parts (splits).
  - Each record (line) is processed by a Map func,
    - written by user,
    - takes an input key/value pair
    - produces a set of intermediate key/value pairs.
    - e.g. (doc???id, doc-content)
  - Draw an analogy to SQL group-by clause

## Distributed Filesyss

- interface is same as a single-machine FS
  - create()
  - open()
  - read()
  - write()
  - close()
- Distribute file data to a no. machines (storage units).
  - Support replication
- Support concurrent data access
  - Fetch content from remote servers. Local caching
- Different implementations sit in different places on complexity/feature scale
  - Google FS & Hadoop HDFS
    - Highly scalable for large data-intensive applications.
    - Provides redundant storage of massive amounts of data on cheap & unreliable computers

### Assumptions

- High component failure rates
  - Inexpensive commodity components fail all time
- "Modest" no. HUGE files
  - Just a few million
  - Each is 100MB or larger; multi-GB files typical
- Files are write-once, mostly appended to
  - Perhaps concurrently
- Large streaming reads
- High sustained throughput favored over low latency

### GFS

- Files are broken into chunks (typically 64 MB) & serve in chunk servers
- Master manages metadata, but clients may cache meta data obtained.
  - Data transfers happen directly between clients/chunk-servers
- Reliability through replication
  - Each chunk replicated across 3+ chunk-servers

### HDFS

- Files split into 128MB blocks
- Blocks replicated across several datanodes (often 3)
- Namenode stores metadata (file names, locations, etc)
- Optimized for large files, sequential reads
- Files are append-only

## MapReduce Execution

### Overview

- Master Server distributes M map tasks to machines & monitors their progress.
- Map task reads allocated data, saves map results in local buffer.
- Shuffle phase assigns reducers to these buffers, which are remotely read & processed by reducers.
- Reducers output result on stable storage.

### Detail

- Input reader
  - Divide input into splits, assign each split to a Map task
- Map task
  - Apply Map func to each record in split
  - Each Map func returns a list of (key, value) pairs
- Shuffle/Partition & Sort
  - Shuffle distributes sorting & aggregation to many reducers
  - All records for key k are directed to same reduce processor
  - Sort groups same keys together, & prepares for aggregation
- Reduce task
  - Apply Reduce func to each key
  - result of Reduce func is a list of (key, value) pairs

### Fault Tolerance

- Handled via re-execution of tasks.
  - Task completion committed through master
- Mappers save outputs to local disk before serving to reducers
  - Allows recovery if a reducer crashes
  - Allows running more reducers than # of nodes
- If a task crashes:
  - Retry on another node
    - OK for a map because it had no dependencies
    - OK for reduce because map outputs are on disk
  - If same task repeatedly fails, fail job or ignore that input block
  - For fault tolerance to work, user tasks must be deterministic & side-effect-free
- If a node crashes:
  - Relaunch its current tasks on other nodes
  - Relaunch any maps node previously ran
    - Necessary because their output files were lost along with crashed node

### Locality Optimization

- Leverage distributed FS to schedule a map task on a machine that contains a replica of corresponding input data.
- Thousands of machines read input at local disk speed
- Without this, rack switches limit read rate

### Redundant Execution

- Slow workers are source of bottleneck, may delay completion time.
- Near end of phase, spawn backup tasks, 1 to finish first wins.
- Effectively utilizes computing power, reducing job completion time by a factor.

### Skipping Bad Records

- Map/Reduce funcs sometimes fail for particular inputs.
- Fixing Bug might not be possible
  - Third Party Libraries.
- On Error
  - Worker sends signal to Master
  - If multiple error on same record, skip record

### Miscellaneous Refinements

- Combiner func at a map task
- Sorting Guarantees within each reduce partition.
- Local execution for debugging/testing
- User-defined counters

### Combining Phase

- Run on map machines after map phase
- "Mini-reduce," only on local map output
- Used to save bandwidth before sending data to full reduce tasks
- Reduce tasks can be combiner if commutative & associative

### Applications

- Web Applications
  - Distributed Grep.
  - Count of URL Access Frequency.
  - Clustering (K-means)
  - Graph Algorithms.
  - Indexing syss
- Map Only processing
- Filtering & accumulation
- Database join
- Reversing graph edges
- Producing inverted index for web search
- PageRank graph processing

## Hadoop & Tools

- Various Linux Hadoop clusters around
  - Cluster +Hadoop
  - Amazon EC2
- Windows & other platforms
  - NetBeans plugin simulates Hadoop
  - workflow view works on Windows
- Hadoop-based tools
  - For Developing in Java, NetBeans plugin
- Pig Latin, a SQL-like high level data processing script language
- Hive, Data warehouse, SQL
- Mahout, Machine Learning algorithms on Hadoop
- HBase, Distributed data store as a large table

## Use Case

### Map Only

- Data distributive tasks - Map Only
  - Classify individual documents
  - Map does everything
    - Input: (docno, doc_content), ???
    - Output: (docno, [class, class, ???]), ???
  - No reduce tasks

### Filtering & Accumulation

- Counting total enrollments of 2 given student classes
- Map selects records & outputs initial counts
  - In: (Jamie, 11741), (Tom, 11493), ???
  - Out: (11741, 1), (11493, 1), ???
- Shuffle/Partition by class_id
- Sort
  - In: (11741, 1), (11493, 1), (11741, 1), ???
  - Out: (11493, 1), ???, (11741, 1), (11741, 1), ???
- Reduce accumulates counts
  - In: (11493, [1, 1, ???]), (11741, [1, 1, ???])
  - Sum & Output: (11493, 16), (11741, 35)

### Database Join

- A JOIN is a means for combining fields from 2 tables by using values common to each.
- For each employee, find department he works in
- Problem: Massive lookups
  - Given 2 large lists: (URL, ID) & (URL, doc_content) pairs
  - Produce (URL, ID, doc_content) or (ID, doc_content)
- Solution:
  - Input stream: both (URL, ID) & (URL, doc_content) lists
    - (a/post, 0), (b/submit, 1), ???
    - (a/post, <html0>), (b/submit, <html1>), ???
  - Map simply passes input along,
  - Shuffle & Sort on URL (group ID & doc_content for same URL together)
    - Out
      - (a/post, 0), (a/post, <html0>), (b/submit, <html1>), (b/submit, 1), ???
  - Reduce outputs result stream of (ID, doc_content) pairs
    - In
      - (a/post, [0, html0]), (b/submit, [html1, 1]), ???
    - Out
      - (0, <html0>), (1, <html1>), ???

### Reverse graph edge directions & output in node order

- Input example: adjacency list of graph (3 nodes & 4 edges)
  - in
    - (3, [1, 2])
    - (1, [2, 3])
  - out
    - (1, [3])
    - (2, [1, 3])
    - (3, [1])
- node_ids in output values are also sorted.
  - But Hadoop only sorts on keys!
- MapReduce format
  - Input: (3, [1, 2]), (1, [2, 3]).
  - Intermediate: (1, [3]), (2, [3]), (2, [1]), (3, [1]). (reverse edge direction)
  - Out: (1,[3]) (2, [1, 3]) (3, [[1]).

### Inverted Indexing Preliminaries

- Construction of inverted lists for document search
- Input: documents: (docid, [term, term..]), (docid, [term, ..]), ..
- Output: (term, [docid, docid, ???])
  - E.g., (apple, [1, 23, 49, 127, ???])
- A document id is an internal document id, a unique integer
  - Not an external document id such as a url

### PageRank

- Start with seed PageRank values
- Each page distributes PageRank "credit" to all pages it points to.
- Each target page adds up "credit" from multiple inbound links to compute PRi+1
  - Effects at each iteration is local. i+1th iteration depends only on ith iteration
  - At iteration i, PageRank for individual nodes can be computed independently
- Map
  - distribute PageRank "credit" to link targets
- Reduce
  - gather up PageRank "credit" from

1 PageRank iteration:

- Input:
  - (id1, [score1(t), out11, out12, ..]), (id2, [score2(t), out21, out22, ..]) ..
- Output:
  - (id1, [score1(t+1), out11, out12, ..]), (id2, [score2(t+1), out21, out22,..]) ..
- MapReduce elements
  - Score distribution & accumulation
  - Database join
- Score Distribution & Accumulation
  - Map
    - In
      - (id1, [score1(t), out11, out12, ..]), (id2, [score2(t), out21, out22, ..]) ..
    - Out
      - (out11, score1(t)/n1), (out12, score1(t)/n1) .., (out21, score2(t)/n2), ..
  - Shuffle & Sort by node_id
    - In
      - (id2, score1), (id1, score2), (id1, score1), ..
    - Out
      - (id1, score1), (id1, score2), .., (id2, score1), ..
  - Reduce
    - In
      - (id1, [score1, score2, ..]), (id2, [score1, ..]), ..
    - Out
      - (id1, score1(t+1)), (id2, score2(t+1)), ..
- Database Join to associate outlinks with score
  - Map
    - In & Out
      - (id1, score1(t+1)), (id2, score2(t+1)), .., (id1, [out11, out12, ..]), (id2, [out21, out22, ..]) ..
  - Shuffle & Sort by node_id
    - Out
      - (id1, score1(t+1)), (id1, [out11, out12, ..]), (id2, [out21, out22, ..]), (id2, score2(t+1)), ..
  - Reduce
    - In
      - (id1, [score1(t+1), out11, out12, ..]), (id2, [out21, out22, .., score2(t+1)]), ..
    - Out
      - (id1, [score1(t+1), out11, out12, ..]), (id2, [score2(t+1), out21, out22, ..]) ..

# NoSQL

## Web applications

- have different needs
  - Horizontal scalability - lowers cost
  - Geographically distributed
  - Elasticity
  - Schema less, flexible schema for semi-structured data
  - Easier for developers
  - Heterogeneous data storage
  - High Availability/Disaster Recovery
- do not always need
  - Transaction
  - Strong consistency
  - Complex queries

## SQL vs NoSQL

- SQL
  - Gigabytes to Terabytes
  - Centralized
  - Structured
  - Structured Query Language
  - Stable Data Model
  - Complex Relationships
  - ACID Property
  - Transaction is priority
  - Joins Tables
- NoSQL
  - Petabytes(1kTB) to Exabytes(1kPB) to Zetabytes(1kEB)
  - Distributed
  - Semi structured & Unstructured
  - No declarative query language
  - Schema less
  - Less complex relationships
  - Eventual Consistency
  - High Availability, High Scalability
  - Embedded structures

## Use cases

- Massive data volume at scale (Big volume)
  - Google, Amazon, Yahoo, Facebook - 10-100K servers
- Extreme query workload (Big velocity)
- High availability
- Flexible, schema evolution

## Data model

### Relational

- Data is usually stored in row by row manner (row store)
- Standardized query language (SQL)
- Data model defined before you add data
- Joins merge data from multiple tables
- Results are tables
- Pros: Mature ACID transactions with fine-grain security controls, widely used
- Cons: Requires up front data modeling, does not scale well

### Key/value

- Simple key/value interface
  - GET, PUT, DELETE
- Value can contain any kind of data
- Super fast & easy to scale (no joins)

### Key/value vs. table

- A table with 2 columns & a simple interface
  - Add a key-value
  - For this key, give me value
  - Delete a key

## Tools

### Memcached

- Open source in-memory key-value caching sys
- Make effective use of RAM on many distributed web servers
- Designed to speed up dynamic web applications by alleviating database load
  - Simple interface for highly distributed RAM caches
  - 30ms read times typical
- Designed for quick deployment, ease of development
- APIs in many languages

### Redis

- Open source in-memory key-value store with optional durability
- Focus on high speed reads & writes of common data structures to RAM
- Allows simple lists, sets & hashes to be stored within value & manipulated
- Many features that developers like expiration, transactions, pub/sub, partitioning

### Amazon DynamoDB

- Scalable key-value store
- Fastest growing product in Amazon's history
- Focus on throughput on storage & predictable read & write times
- Strong integration with S3 & Elastic MapReduce

### Bigtable

- Fault-tolerant, persistent
- Scalable
  - Thousands of servers
  - Terabytes of in-memory data
  - Petabyte of disk-based data
  - Millions of reads/writes per second, efficient scans
- Self-managing
  - Servers can be added/removed dynamically
  - Servers adjust to load imbalance

### Apache Hbase

- Open-source Bigtable, written in JAVA
- Part of Apache Hadoop project

### Apache Cassandra

- Apache open source column family database
- Peer-to-peer distribution model
- Strong reputation for linear scale out (millions of writes/second)
- Written in Java & works well with HDFS & MapReduce

## Column family store

- Dynamic schema, column-oriented data model
- Sparse, distributed persistent multi-dimensional sorted map
- (row, column (family), timestamp) -> cell contents

### Column families

- Group columns into "Column families"
- Group column families into "Super-Columns"
- Be able to query all columns with a family or super family
- Similar data grouped together to improve speed

### Column family data model vs. relational

- Sparse matrix, preserve table structure
  - 1 row could have millions of columns but can be very sparse
- Hybrid row/column stores
- no. columns is extendible
  - New columns to be inserted without doing an "alter table"

## Graph data model

- Core abstractions
  - Nodes
  - Relationships
  - Properties on both

### Graph database store

- A database stored data in an explicitly graph structure
- Each node knows its adjacent nodes
- Queries are really graph traversals

### Neo4j

- Graph database designed to be easy to use by Java developers
- Disk-based (not just RAM)
- Full ACID
- High Availability (with Enterprise Edition)
- 32 Billion Nodes, 32 Billion Relationships, 64 Billion Properties
- Embedded java library
- REST API

## Document store

- Documents, not value, not tables
- JSON or XML formats
- Document is identified by ID
- Allow indexing on properties

### MongoDB

- Open Source JSON data store created by 10gen
- Master-slave scale out model
- Strong developer community
- Sharding built-in, automatic
- Implemented in C++ with many APIs (C++, JavaScript, Java, Perl, Python)
- architecture
  - Replica set
    - Copies of data on each node
    - Data safety
    - High availability
    - Disaster recovery
    - Maintenance
    - Read scaling
  - Sharding
    - "Partitions" of data
    - Horizontal scale

### Apache CouchDB

- Open source JSON data store
- Written in ERLANG
- RESTful JSON API
- B-Tree based indexing, shadowing b-tree versioning
- ACID fully supported
- View model
- Data compaction
- Security

# Elasticsearch & Kibana

## Elasticsearch

- Full-text search engine
- Based on Lucene library
- HTTP web interface & schema-free JSON documents

## Kibana aggregations

- 2 types
  - Bucket aggregations groups documents together in 1 bucket according to your logic & requirements
  - Metric aggregations are used to calculate a value for each bucket based on documents inside bucket

# CAP theorem

## Scaling Traditional Databases

- Traditional RDBMSs can be either scaled:
  - Vertically (or Up)
    - Can be achieved by hardware upgrades (e.g., faster CPU, more memory, or larger disk)
    - Limited by amount of CPU, RAM & disk that can be configured on a single machine
  - Horizontally (or Out)
    - Can be achieved by adding more machines
    - Requires database sharding & probably replication
    - Limited by Read-to-Write ratio & communication overhead
- Data sharding
  - Data is typically sharded (or striped) to allow for concurrent/parallel accesses
  - Will it scale for complex query processing?
- Data replicating
  - Replicating data across servers helps in:
    - Avoiding performance bottlenecks
    - Avoiding single point of failures
    - Enhancing scalability & availability
- Consistency Challenge
  - In an e-commerce application, bank database has been replicated across 2 servers
  - Maintaining consistency of replicated data is a challenge
- Two-Phase Commit Protocol
  - 2PC can be used to ensure atomicity & consistency

## CAP Theorem

- limitations of distributed databases
  - Consistency
    - every node always sees same data at any given instance (strict consistency)
  - Availability
    - sys continues to operate, even if nodes in a cluster crash, or some hardware or software parts are down due to upgrades
  - Partition Tolerance
    - sys continues to operate in presence of network partitions
- CAP theorem
  - any distributed database with shared data, can have at most 2 of 3 desirable properties, C, A or P
  - trade-offs involved in distributed sys

### Scalability of relational databases

- Relational Database is built on principle of ACID
  - Atomicity
  - Consistency
  - Isolation
  - Durability
- A truly distributed relational database should have availability, consistency & partition tolerance.
  - Which unfortunately is impossible

### Large-Scale Databases

- When companies such as Google & Amazon were designing large-scale databases, 24/7 Availability was a key
  - A few minutes of downtime means lost revenue
- When horizontally scaling databases to 1000s of machines, likelihood of a node or a network failure increases tremendously
  - to have strong guarantees on Availability & Partition Tolerance, they had to sacrifice "strict" Consistency (implied by CAP theorem)

### Trading-Off Consistency

- Maintaining consistency should balance between strictness of consistency versus availability/scalability
  - Good-enough consistency depends on your application

### BASE Properties

- Impossible to guarantee strict Consistency & Availability while being able to tolerate network partitions
- Databases with relaxed ACID guarantees
- Apply BASE properties:
  - Basically Available: sys guarantees Availability
  - Soft-State: state of sys may change over time
  - Eventual Consistency: sys will eventually become consistent

### Eventual Consistency

- A database is termed as Eventually Consistent if:
- All replicas will gradually become consistent in absence of new updates

# Distributed hash table (DHT)

## Hashing

### Hash map

- A data structure implements an associative array that can map keys to values.
  - searching & insertions are 0(1) in worse case
- Uses a hash func to compute an index into an array of buckets or slots from which correct value can be found.
- index = f(key, array_size)

### Hash funcs

- Crucial for good hash table performance
- Can be difficult to achieve
  - WANTED: uniform distribution of hash values
  - A non-uniform distribution increases no. collisions & cost of resolving them

### Hashing for partitioning usecase

- Objective
  - Given document X, choose 1 of k servers to use
- Eg. using modulo hashing
  - Number servers 1..k
  - Place X on server i = (X mod k)
    - Problem? Data may not be uniformly distributed
  - Place X on server i = hash (X) mod k
- Problem?
  - What happens if a server fails or joins (k -> k??1)?
  - What is different clients has different estimate of k?
    - Answer: All entries get remapped to new nodes!

## Distributed hash table (DHT)

- Similar to hash table but spread across many hosts
- Interface
  - insert(key, value)
  - lookup(key)
- Every DHT node supports a single operation:
  - Given key as input; route messages to node holding key

### How to design

- State Assignment
  - What "(key, value) tables" does a node store?
- Network Topology
  - How does a node select its neighbors?
- Routing Algorithm:
  - Which neighbor to pick while routing to a destination?
- Various DHT algorithms make different choices

## Chord

- A scalable peer-to-peer look-up protocol for internet applications

### What is Chord?

- a peer-to-peer lookup sys
- Given a key (data item), it maps key onto a node (peer).
- Uses consistent hashing to assign keys to nodes .
- Solves problem of locating key in a collection of distributed nodes.
- Maintains routing information with frequent node arrivals & departures

### Consistent hashing

- Consistent hash func assigns each node & key an m-bit identifier.
- SHA-1 is used as a base hash func.
- A node's identifier is defined by hashing node's IP address.
- A key identifier is produced by hashing key (chord doesn't define this. Depends on application).
  - ID(node) = hash(IP, Port)
  - ID(key) = hash(key)
- In an m-bit identifier space, there are 2m identifiers.
- Identifiers are ordered on an identifier circle modulo 2m.
- identifier ring is called Chord ring.
- Key k is assigned to first node whose identifier is equal to or follows (the identifier of) k in identifier space.
- This node is successor node of key k, denoted by successor(k).
- Join & departure
  - When a node n joins network, certain keys previously assigned to n's successor now become assigned to n.
  - When node n leaves network, all of its assigned keys are reassigned to n's successor.

### A Simple key lookup

- If each node knows only how to contact its current successor node on identifier circle, all node can be visited in linear order.
- Queries for a given identifier could be passed around circle via these successor pointers until they encounter node that contains key.
- Pseudo code for finding successor:

```
  // ask node n to find successor of id
  n.find_successor(id)
    if (id ?? (n, successor])
      return successor;
    else
      // forward query around circle
      return successor.find_successor(id);
```

### Scalable key location

- To accelerate lookups, Chord maintains additional routing information.
- This additional information is not essential for correctness, which is achieved as long as each node knows its correct successor.
- Finger tables
  - Each node n' maintains a routing table with up to m entries (which is in fact no. bits in identifiers), called finger table.
  - ith entry in table at node n contains identity of first node s that succeeds n by at least 2^i-1 on identifier circle.
  - s = successor(n+2^i-1).
  - s is called ith finger of node n, denoted by n.finger(i)
  - A finger table entry includes both Chord identifier & IP address (and port number) of relevant node.
  - first finger of n is immediate successor of n on circle.

### Applications: Chord-based DNS

- DNS provides a lookup service
  - keys: host names values: IP adresses
- Chord could hash each host name to a key
- Chord-based DNS:
  - no special root servers
  - no manual management of routing information
  - no naming structure
  - can find objects not tied to particular machines

### Addressed problems

- Load balance
  - chord acts as a distributed hash func, spreading keys evenly over nodes
- Decentralization
  - chord is fully distributed, no node is more important than any other, improves robustness
- Scalability
  - logarithmic growth of lookup costs with no. nodes in network, even very large syss are feasible
- Availability
  - chord automatically adjusts its internal tables to ensure that node responsible for a key can always be found
- Flexible naming
  - chord places no constraints on structure of keys it looks up.

### Summary

- Simple, powerful protocol
- Only operation: map a key to responsible node
- Each node maintains information about O(log N) other nodes
- Lookups via O(log N) messages
- Scales well with no. nodes
- Continues to func correctly despite even major changes of sys

# Spark

### Support Interactive & Streaming Comp.

- Aggressive use of memory
  1. Memory transfer rates: disk or SSDs
  1. Many datasets already fit into memory
     - Inputs of over 90% of jobs in Facebook, Yahoo!, & Bing clusters fit into memory
     - e.g., 1TB = 1 billion records @ 1KB each
  1. Memory density (still) grows with Moore's law
     - RAM/SSD hybrid memories at horizon
- Increase parallelism
  - Reduce work per node -> improve latency
  - Techniques:
    - Low latency parallel scheduler that achieve high locality
    - Optimized parallel communication patterns (shuffle, broadcast)
    - Efficient recovery from failures & straggler mitigation

## What is Spark

- Unified analytics engine for large-scale data processing
- Speed: run workloads 100x faster
  - High performance for both batch & streaming data
  - Computations run in memory
- Ease of Use: write applications quickly
  - Over 80 high-level operators
  - Use interactively form Scala, Python, R, & SQL

```
df = spark.read.json("logs.json")df.
where("age
21")
select("name.first").show()
```

- Generality: combine SQL, Streaming, & complex analytics
  - Provide libraries including SQL & DataFrames, Spark Streaming, MLib, GraphX,
  - Wide range of workloads: batch applications, interactive algorithms, interactive queries, streaming
- Run Everywhere:
  - run on Hadoop, Apache Mesos, Kubernetes, standalone or in cloud.
  - access data in HDFS, Aluxio, Apache Cassandra, Apache Hbase, Apache Hive

## Stack

- Spark Core:
  - contain basic funcality of Spark including task scheduling, memory management, fault recovery
  - provide APIs for building & manipulating RDDs
- SparkSQL
  - querying structured data via SQL, Hive Query Language
  - combining SQL queries & data manipulations in Python, Java, Scala
- Spark Streaming
  - enables processing of live streams of data via APIs
- Mlib
  - contain common machine language funcality
  - provide multiple types of algorithms: classification, regression, clustering
- GraphX
  - library for manipulating graphs & performing graph-parallel computations
  - extend Spark RDD API
- Cluster Managers
  - Hadoop Yarn
  - Apache Mesos
  - Standalone Schedular (simple manager in Spark).

## Resilient Distributed Dataset - RDD

### Basics

- Immutable distributed collection of objects
- Split into multiple partitions => can be computed on different nodes
- All work in Spark is expressed as
  - creating new RDDs
  - transforming existing RDDs
  - calling actions on RDDs
- Example
  - Load error messages from a log into memory
  - Interactively search for various patterns

```
lines = spark.textFile("hdfs://...")
errors = lines.filter(_.startsWith("ERROR"))
messages = errors.map(_.split(???\t')(2))
cachedMsgs = messages.cache()

cachedMsgs.filter(\_.contains("foo")).count
cachedMsgs.filter(\_.contains("bar")).count
```

- 2 types of operations
  - Transformations
    - construct a new RDD from a previous 1
      - filter data
  - Actions
    - compute a result base on an RDD
      - count elements
      - get first element

#### Transformations

- Create new RDDs from existing RDDs
- Lazy evaluation
  - See whole chain of transformations
  - Compute just data needed
- Persist contents:
  - persist an RDD in memory to reuse it in future
  - persist RDDs on disk is possible

#### Typical works of a Spark program

1. Create some input RDDs form external data
1. Transform them to define new RDDs using transformations like filter()
1. Ask Spark to persist() any intermediate RDDs that will need to be reused
1. Launch actions such as count(), first() to kick off a parallel computation

### Creating RDDs

1. Parallelizing a collection: uses parallelize()

   `lines = sc.parallelize(["pandas", "i like pandas"])`

1. Loading data from external storage
  
   `lines = sc.textFile("/path/to/README.md")`

### Operations

- 2 types of operations
- Transformations
  - return a new RDDs
    - map()
    - filter()
- Actions
  - return a result to
    - driver program
    - write it to storage
      - count(), first()
- Treated differently by Spark
  - Transformation: lazy evaluation
  - Action: execution at any time
- Transformation
  - Can operate on any no. input RDDs
  - Spark keeps track dependencies between RDDs, called lineage graph
  - Allow recovering lost data
  - filter()
    - not change existing inputRDD
    - returns a pointer to an entirely new RDD
    - inputRDD still can be reused
    ```
    inputRDD = sc.textFile("log.txt")
    errorsRDD = inputRDD.filter(lambda x: "error" in x)
    ```
  - union()
    ```
    errorsRDD = inputRDD.filter(lambda x: "error" in x)
    warningsRDD=inputRDD.filter(lambda x: "warning" in x)
    badLinesRDD = errorsRDD.union(warningsRDD)
    ```
- Count no. errors

```
print("Input had " + badLinesRDD.count() + " concerning lines")
print("Here are 10 examples:")
for line in badLinesRDD.take(10):
  print(line)
```

## Load & Inspect Data in Spark

### Zeppelin notebook

- A web-based interface for interactive data analytics
  - Easy to write & access your code
  - Support many programming languages
    - Scala (with Apache Spark), Python (with Apache Spark), SparkSQL, Hive, Markdown, Angular, & Shell
  - Data visualization
- Monitoring Spark jobs

### Load, inspect & save data

- Data is always huge that does not fit on a single machine
  - Data is distributed on many storage nodes
- Data scientists can likely focus on format that their data is already in
  - Engineers may wish to explore more output formats
- Spark supports a wide range of input & output sources

### Data sources

- File formats & filesyss
  - Local or distributed filesys, such as NFS, HDFS or Amazon S3
  - File formats including text, JSON, SequenceFiles & protocol buffers
- Structured data sources through Spark SQL
  - Apache Hive
  - Parquet
  - JSON
  - From RDDs
- Databases & key/value stores
  - Cassandra, HBase, Elasticsearch, & JDBC dbs

### File Formats

- unstructured
  - text
- semistructured
  - JSON
- structured
  - SequenceFiles

## Pair RDDs & DataFrame

### RDD (Resilient Distributed Dataset)

- Immutable distributed collection of objects

  - Resilient: If data in memory is lost, it can be recreated
  - Distributed: Processed across cluster
  - Dataset: Initial data can come from a source such as a file, or it can be created programmatically

- can hold any serializable type of element
  - Primitive types such as integers, characters, & booleans
  - Sequence types such as strings, lists, arrays, tuples, & dicts (including nested data types)
  - Scala/Java Objects (if serializable)
  - Mixed types
- Some RDDs are specialized & have additional funcality
  - Pair RDDs
  - RDDs consisting of key-value pairs
  - Double RDDs
  - RDDs consisting of numeric data
- operations
  - first
    - returns first element of RDD
  - foreach
    - applies a func to each element in an RDD
  - top(n)
    - returns largest n elements using natural ordering
  - Sampling operations
    - sample
      - creates a new RDD with a sampling of elements
    - take
      - Sample returns an array of sampled elements

### Paired RDD

- RDD of key/value pairs
- special form of RDD
  - Each element must be a keyvalue pair (a two-element tuple)
  - Keys & values can be any type
- Why?
  - Use with map-reduce algorithms
  - Many additional funcs are available for common data processing needs
    - sorting, joining, grouping & counting
- Creating Pair RDDs
  - Get data into key/value form
    - map
    - flatMap / flatMapValues
    - keyBy

## Map-Reduce

- Easily applicable to distributed processing of large data sets
- Hadoop MapReduce is major implementation
  - limited
  - Each job has 1 map phase, 1 reduce phase
  - Job output is saved to files
- Spark implements map-reduce with greater flexibility
  - Map & reduce funcs can be interspersed
  - Results can be stored in memory
  - Operations can easily be chained

### Map-Reduce in Spark

- Work on pair RDDs
- Map phase
  - Operates on 1 record at a time
  - "Maps" each record to zero or more new records
    - map
    - flatMap
    - filter
    - keyBy
- Reduce phase
  - Works on map output
  - Consolidates multiple records
    - reduceByKey
    - sortByKey
    - mean

### Pair RDD Operations

- `countByKey`
  - returns a map with count of occurrences of each key
- `groupByKey`
  - groups all values for each key in an RDD
- `sortByKey`
  - sorts in ascending or descending order
- `join`
  - returns an RDD containing all pairs with matching keys from 2 RDD
- other operations
  - `keys`
    - returns an RDD of just keys, without values
  - `values`
    - returns an RDD of just values, without keys
  - `lookup(key)`
    - returns value(s) for a key
  - `leftOuterJoin, rightOuterJoin, fullOuterJoin`
    - join 2 RDDs, including keys defined in left, right or either RDD respectively
  - `mapValues, flatMapValues`
    - execute a func on just values, keeping key same

## DataFrames & Apache Spark SQL

### Spark SQL

- Spark module for structured data processing
- Built on top of core Spark
  - DataFrame API
    - a library for working with data as tables
  - Defines DataFrames containing rows & columns
  - DataFrames are focus of this chapter!
  - Catalyst Optimizer
    - an extensible optimization framework
  - A SQL engine & command line interface

### SQL Context

- main Spark SQL entry point is a SQL context object
  - Requires a SparkContext object
  - Similar to Spark context in core Spark
- 2 implementations
  - SQLContext
    - Basic implementation
  - HiveContext
    - Reads & writes Hive/HCatalog tables directly
    - Supports full HiveQL language
    - Requires Spark application be linked with Hive libraries
    - Cloudera recommends using HiveContext
- Creating a SQL Context
  - The Spark shell creates a HiveContext instance automatically
    - Call sqlContext
    - Create 1 when writing a Spark application
    - Having multiple SQL context objects is allowed
  - SQL context object is created based on Spark context

### DataFrames

- Main abstraction in Spark SQL
  - Analogous to RDDs in core Spark
  - A distributed collection of structured data organized into Named columns
  - Built on a base RDD containing Row objects
- Load data
  - `json(filename)`
  - `parquet(filename)`
  - `orc(filename)`
  - `table(hive`
  - `tablename)`
  - `jdbc(url,table,options)`
- Load from a Data Source Manually
- Specify settings for DataFrameReader
  - format: Specify a data source type
  - option: A key/value setting for underlying data source
  - schema: Specify a schema instead of inferring from data source
- Then call generic base func load

### Data Sources

- table
- json
- parquet
- jdbc
- orc
- third party data source libraries
  - Avro (included in CDH)
  - HBase
  - CSV
  - MySQL

### DataFrame Basic Operations

- Deal with metadata (rather than its data)
  - `schema`
    - returns a schema object describing data
  - `printSchema`
    - displays schema as a visual tree
  - `cache / persist`
    - persists DataFrame to disk or memory
  - `columns`
    - returns an array containing names of columns
  - `dtypes`
    - returns an array of (column name,type) pairs
  - `explain`
    - prints debug information about DataFrame to console

### DataFrame Actions

- `collect`
  - returns all rows as an array of Row objects
- `take(n)`
  - returns first n rows as an array of Row objects
- `count`
  - returns no. rows
- `show(n)`
  - displays first n rows (default=20)

### DataFrame Queries

- Return new DataFrames
- Can be chained like transformations
  - `distinct`
    - returns a new DataFrame with distinct elements of this DF
  - `join`
    - joins this DataFrame with a second DataFrame
    - Variants for inside, outside, left, & right joins
  - `limit`
    - returns a new DataFrame with first n rows of this DF
  - `select`
    - returns a new DataFrame with data from 1 or more columns of base DataFrame
  - `where`
    - returns a new DataFrame with rows meeting specified query criteria (alias for filter)

### Saving DataFrames

- DataFrame.write to create a DataFrameWriter
- provides convenience funcs to externally save data represented by a DataFrame
  - `jdbc`
    - inserts into a new or existing table in a database
  - `json`
    - saves as a JSON file
  - `parquet`
    - saves as a Parquet file
  - `orc`
    - saves as an ORC file
  - `text`
    - saves as a text file (string data in a single column only)
  - `saveAsTable`
    - saves as a Hive/Impala table (HiveContext only)
- Options for Saving DataFrames
  - `format`
    - specifies a data source type
  - `mode`
    - determines behavior if file or table already exists: overwrite, append, ignore or error (default is error)
  - `partitionBy`
    - stores data in partitioned directories in form column=value (as with Hive/Impala partitioning)
  - `options`
    - specifies properties for target data source
  - `save`
    - generic base func to write data

### DataFrames & RDDs

- DataFrames are built on RDDs
  - Base RDDs contain Row objects
  - Use rdd to get underlying RDD
- Row RDDs have all standard Spark actions & transformations
  - Actions: collect, take, count
  - Transformations: map, flatMap, filter
- Row RDDs can be transformed into pair RDDs to use mapreduce methods
- DataFrames also provide convenience methods (such as map, flatMap,and foreach) for converting to RDDs

### Working with Row Objects

- Array-like syntax to return values with type Any
  - `row(n)`
    - returns element in nth column
  - `row.fieldIndex("age")`
    - returns index of age column
- methods to get correctly typed values
  - `row.getAs[Long]("age")`
- type-specific get methods to return typed values
  - `row.getString(n)`
    - returns nth column as a string
  - `row.getInt(n)`
    - returns nth column as an integer

## Spark Streaming

- Scalable, fault-tolerant stream processing sys
  - Receive data streams from input sources
  - process them in a cluster
  - push out to databases/dashboards

### How does it work?

- stream is treated as a series of very small, deterministic batches of data
- Spark treats each batch of data as RDDs & processes them using RDD operations
- Processed results are pushed out in batches
- Discretized Stream (DStream)
  - Sequence of RDDs representing a stream of data
  - Any operation applied on a DStream translates to operations on underlying RDDs
- StreamingContext
  - main entry point of all Spark Streaming

### Operation on DStreams

#### Input Operations

- Every input DStream is associated with a Receiver object
- 2 built-in categories of streaming sources:
  - Basic sources
    - FSs, socket connection
  - Advanced sources
    - Twitter, Kafka

#### Transformation

- `map(func)`
  - Return a new DStream by passing each element of the source DStream through a func func
- `flatmap(func)`
  - Similar to map, but each input item can be mapped to 0 or more output items
- `filter(func)`
  - Return a new DStream by selecting only records of source DStream on which func returns true
- `count`
  - Return a new DStream of single-element RDDs by counting no. elements in each RDD of source DStream
- `countbyValue`
  - Returns a new DStream of (K, Long) pairs where value of each key is its frequency in each RDD of source DStream.
- `reduce(func)`
  - Return a new DStream of single-element RDDs by aggregating elements in each RDD of source DStream using a func func (which takes 2 arguments & returns one).
- `reducebyKey(func)`
  - When called on a DStream of (K, V) pairs, return a new DStream of (K, V) pairs where values for each key are aggregated using given reduce func
- `union(otherStream)`
  - Return a new DStream that contains union of elements in source DStream & otherDStream.
- `join(otherStream)`
  - When called on 2 DStreams of (K, V) & (K, W) pairs, return a new DStream of (K, (V, W)) pairs with all pairs of elements for each key.

#### Window Operations

- apply to a sliding window of data
- A window is defined by: window length & siding interval
  - `window(windowLength, slideInterval)`
    - Returns a new DStream which is computed based on windowed batches
  - `countByWindow(windowLength, slideInterval)`
    - Returns a sliding window count of elements in stream.
  - `reduceByWindow(func, windowLength, slideInterval)`
    - Returns a new single-element DStream, created by aggregating elements in stream over a sliding interval using func.

#### Output Operation

- Push out DStream's data to external syss, a database or a FS
- `print`
  - Prints first ten elements of every batch of data in a DStream on driver node running application
- `saveAsTextFiles`
  - Save this DStream's contents as text files
- `saveAsHadoopFiles`
  - Save this DStream's contents as Hadoop files.
- `foreachRDD(func)`
  - Applies a func, func, to each RDD generated from stream

## Spark ML

- Classification: logistic regression, naive Bayes
- Regression: generalized linear regression, survival regression
- Decision trees, random forests, & gradient-boosted trees
- Recommendation: alternating least squares (ALS)
- Clustering: K-means, Gaussian mixtures (GMMs)
- Topic modeling: latent Dirichlet allocation (LDA)
- Frequent item sets, association rules, & sequential pattern mining

## Spark GraphX

- Apache Spark's API for graphs & graph-parallel computation
- GraphX unifies ETL (Extract, Transform & Load) process
- Exploratory analysis & iterative graph computation within a single sys

### Use cases

- Facebook's friends, LinkedIn's connections
- Internet's routers
- Relationships between galaxies & stars in astrophysics & Google's Maps
- Disaster detection, banking, stock market

### RDD on GraphX

- GraphX extends Spark RDD with a Resilient Distributed Property Graph
- property graph is a directed multigraph which can have multiple edges in parallel
- parallel edges allow multiple relationships between same vertices

### Features

- Flexibility
  - Spark GraphX works with both graphs & computations
  - GraphX unifies ETL (Extract, Transform & Load), exploratory analysis & iterative graph computation
- Speed
  - fastest specialized graph processing syss
- Growing Algorithm Library
  - Page rank
  - connected components
  - label propagation
  - SVD++
  - strongly connected components & triangle count
