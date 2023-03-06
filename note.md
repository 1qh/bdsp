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

- Lưu trữ data khả mở, tin cậy
- Powerful data processing
- Efficient visualization
- Challenges
- Thiết bị lưu trữ tốc độ chậm, máy tính thiếu tin cậy, lập trình song song phân tán ko dễ dàng

## Intro

- Lưu trữ và xử lý data khả mở, tiết kiệm chi phí
  - Xử lý data phân tán với mô hình lập trình đơn giản, thân thiện hơn như MapReduce
  - Hadoop thiết kế để mở rộng thông qua kỹ thuật scale-out, tăng số lượng máy chủ
  - Thiết kế để vận hành trên phần cứng phổ thông, có khả năng chống chịu lỗi phần cứng
- Lấy cảm hứng từ kiến trúc data của Google

## Các thành phần chính

- Lưu trữ data: Hệ thống tệp tin phân tán Hadoop (HDFS)
- Xử lý data: MapReduce framework
- Tiện ích hệ thống:
  - Hadoop Common: Các tiện ích chung hỗ trợ các thành phần của Hadoop.
  - Hadoop YARN: 1 framework quản lý tài nguyên và lập lịch trong cụm Hadoop.

## Giải quyết bài toán

### Khả mở

- Thiết kế hướng "phân tán" ngay từ đầu
  - mặc định thiết kế để triển khai trên cụm máy chủ
- Các máy chủ tham gia vào cụm đc gọi là các Nodes
  - Mỗi node tham gia vào cả 2 vai trò lưu trữ và tính toán
- Hadoop mở rộng bằng kỹ thuật scale-out
  - Có thể tăng cụm Hadoop lên hàng chục ngàn nodes

### Chịu lỗi

- Với việc triển khai trên cụm máy chủ phổ thông
  - Hỏng hóc phần cứng là chuyện thường ngày, ko phải là ngoại lệ
  - Hadoop chịu lỗi thông qua kỹ thuật "dư thừa"
- Các tệp tin trong HDFS đc phân mảnh, nhân bản ra các nodes trong cụm
  - Nếu 1 node gặp lỗi, data ứng với nodes đó đc tái nhân bản qua các nodes khác
- Công việc xử lý data đc phân mảnh thành các task độc lập
  - Mỗi task xử lý 1 phần data đầu vào
  - Các task đc thực thi song song với các task khác
  - task lỗi sẽ đc tái lập lịch thực thi trên node khác
- Hệ thống Hadoop thiết kế sao cho các lỗi xảy ra trong hệ thống đc xử lý tự động, ko ảnh hưởng tới các ứng dụng phía trên

## HDFS

- HDFS cung cấp khả năng lưu trữ tin cậy và chi phí hợp lý cho khối lượng data lớn
- Tối ưu cho các file kích thước lớn (từ vài trăm MB tới vài TB)
- HDFS có ko gian cây thư mục phân cấp như UNIX (/hust/soict/hello.txt)
  - Hỗ trợ cơ chế phân quyền và kiểm soát người dùng như của UNIX
- Khác biệt so với hệ thống file trên UNIX
  - Chỉ hỗ trợ thao tác ghi thêm data vào cuối tệp (APPEND)
  - Ghi 1 lần và đọc nhiều lần

### Kiến trúc

- master: name node
  - Quản lý ko gian tên và siêu data ánh xạ tệp tin tới vị trí các chunks
  - Giám sát các data node
- slave: data node
  - Trực tiếp thao tác I/O các chunks

### Nguyên lý thiết kế cốt lõi

- I/O pattern
  - Chỉ ghi thêm (Append)à giảm chi phí điều khiển tương tranh
- Phân tán data
  - Tệp đc chia thành các chunks lớn (64 MB)
    - Giảm kích thước metadata
    - Giảm chi phí truyền data
- Nhân bản data
  - Mỗi chunk thông thường đc sao làm 3 nhân bản
- Cơ chế chịu lỗi
  - Data node: sử dụng cơ chế tái nhân bản
  - Name node
    - Sử dụng Secondary Name Node
    - SNN hỏi data nodes khi khởi động thay vì phải thực hiện cơ chế đồng bộ phức tạp với primary NN

## Mô thức xử lý data MapReduce

- MapReduce ko phải là ngôn ngữ lập trình, đc đề xuất bởi Google
- Đặc điểm của MapReduce
  - Đơn giản (Simplicity)
  - Linh hoạt (Flexibility)
  - Khả mở (Scalability)

### A MR job = {Isolated Tasks}n

- Mỗi chương trình MapReduce là 1 job đc phân rã làm nhiều task và các task này đc phân tán trên các nodes khác nhau của cụm để thực thi
- Mỗi task đc thực thi độc lập với các task khác để đạt đc tính khả mở
  - Giảm truyền thông giữa các node máy chủ
  - Tránh phải thực hiện cơ chế đồng bộ giữa các task

### Data cho MapReduce

- MapReduce trong môi trường Hadoop thường làm việc với data có sẵn trên HDFS
- Khi thực thi, mã chương trình MapReduce đc gửi tới các node đã có data tương ứng

### Chương trình MapReduce

- Lập trình với MapReduce cần cài đặt 2 hàm Map và Reduce
- 2 hàm này đc thực thi bởi các tiến trình Mapper và Reducer tương ứng.
- Data đc nhìn nhận như là các cặp key - value
- Nhận đầu vào và trả về đầu ra các cặp key - value
- Ví dụ
  - Đầu vào: tệp văn bản chứa thông tin về order ID, employee name, & sale amount
  - Đầu ra : Doanh số bán (sales) theo từng nhân viên (employee)

### Bước Map

- data đầu vào đc xử lý bởi nhiều task Mapping độc lập
  - Số task Mapping đc xác định theo lượng data đầu vào (~ số chunks)
  - Mỗi task Mapping xử lý 1 phần data (chunk) của khối data ban đầu
- Với mỗi task Mapping, Mapper xử lý lần lượt từng bản ghi đầu vào
  - Với mỗi bản ghi đầu vào (key-value)
    - Mapper đưa ra 0 hoặc nhiều bản ghi đầu ra (key - value trung gian)
- Trong ví dụ, task Mapping đơn giản đọc từng dòng văn bản và đưa ra tên nhân viên và doanh số tương ứng Map phase

### Bước shuffle & sort

- Tự động sắp xếp và gộp đầu ra của các Mappers theo các partitions
- Mỗi partitions là đầu vào cho 1 Reducer Shuffle & sort phase

### Bước Reduce

- Reducer nhận data đầu vào từ bước shuffle & sort
  - Tất cả các bản ghi key - value tương ứng với 1 key đc xử lý bởi 1 Reducer duy nhất
  - Giống bước Map, Reducer xử lý lần lượt từng key, mỗi lần với toàn bộ các values tương ứng
- Trong ví dụ, hàm reduce đơn giản là tính tổng doanh số cho từng nhân viên, đầu ra là các cặp key - value tương ứng với tên nhân viên - doanh số tổng

## Các thành phần khác trong hệ sinh thái Hadoop

- Thành phần khác phục vụ
  - Phân tích data
  - Tích hợp data
  - Quản lý luồng
- Ko phải 'core Hadoop' nhưng là 1 phần của hệ sinh thái Hadoop
  - Hầu hết là mã nguồn mở trên Apache

### Pig

- Cung cấp giao diện xử lý data mức cao
  - Pig đặc biệt tốt cho các phép toán Join và Transformation
- Trình biên dịch của Pig chạy trên máy client
  - Biến đổi PigLatin script thành các jobs của MapReduce
  - Đệ trình các công việc này lên cụm tính toán

### Hive

- Cũng là 1 lớp trừu tượng mức cao của MapReduce
  - Giảm thời gian phát triển
  - Cung cấp ngôn ngữ HiveQL: SQL-like language
- Trình biên dịch Hive chạy trên máy client
  - Chuyển HiveQL script thành MapReduce jobs
  - Đệ trình các công việc này lên cụm tính toán

### Hbase

- CSDL cột mở rộng phân tán, lưu trữ data trên HDFS
  - hệ quản trị CSDL của Hadoop
- data đc tổ chức về mặt logic là các bảng, bao gồm rất nhiều dòng và cột
  - Kích thước bảng có thể lên đến hàng Terabyte, Petabyte
  - Bảng có thể có hàng ngàn cột
- Có tính khả mở cao, đáp ứng băng thông ghi data tốc độ cao
  - Hỗ trợ hàng trăm ngàn thao tác INSERT mỗi giây (/s)
- Tuy nhiên về các chức năng rất hạn chế khi so sánh với hệ QTCSDL truyền thống
  - Là NoSQL: ko có ngôn ngữ truy vấn mức cao như SQL
  - Phải sự dụng API để scan/ put/ get/ data theo khóa

### Sqoop

- Sqoop là 1 công cụ cho phép trung chuyển data theo khối từ Apache Hadoop và các CSDL có cấu trúc như CSDL quan hệ
- Hỗ trợ import tất cả các bảng, 1 bảng hay 1 phần của bảng vào HDFS
  - Thông qua Map only hoặc MapReduce job
  - Kết quả là 1 thư mục trong HDFS chứ các file văn bản phân tách các trường theo ký tự phân tách (vd. , hoặc \t)
- Hỗ trợ export data ngược trở lại từ Hadoop ra bên ngoài

### Kafka

- Producers ko cần biết Consumers
  - Đảm bảo sự linh hoạt và tin cậy trong quá trình trung chuyển data giữa các bên
- cho phép phân tách mạch lạc các thành phần tham gia vào luồng data

### Oozie

- Hệ thống lập lịch luồng công việc để quản lý các công việc thực thi trên cụm Hadoop
- Luồng workflow của Oozie là đồ thị vòng có hướng (Directed Acyclical Graphs (DAGs)) của các khối công việc
- Oozie hỗ trợ đa dạng các loại công việc
  - Thực thi
    - MapReduce jobs
    - Pig hay Hive scripts
    - các chương trình Java hoặc Shell
  - Tương tác với data trên HDFS
  - Chạy chương trình từ xa qua SSH
  - Gửi nhận email

### Zookeeper

- Dịch vụ cung cấp các chức năng phối hợp phân tán độ tin cậy cao
  - Quản lý thành viên trong nhóm máy chủ
  - Bầu cử leader
  - Quản lý thông tin cấu hình động
  - Giám sát trạng thái hệ thống
- Đây là các service lõi, tối quan trọng trong các hệ thống phân tán

### YARN - Yet Another Resource Negotiator

- Nodes có tài nguyên là - bộ nhớ và CPU cores
- đóng vai trò cấp phát lượng tài nguyên phù hợp cho các ứng dụng khi có yêu cầu
- đc đưa ra từ Hadoop 2.0
  - Cho phép MapReduce và non MapReduce cùng chạy trên 1 cụm Hadoop
  - Với MapReduce job, vai trò của job tracker đc thực hiện bởi application tracker

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
    - → location transparency, but reverse is not necessarily true.

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
  - 8-16 cores, 32 GB RAM, 8×1.5 TB disks

## MapReduce Programming Model

- Inspired from map & reduce operations commonly used in funcal programming languages like Lisp.
- Have multiple map tasks & reduce tasks
- Users implement interface of 2 primary methods:
  - Map: (key1, val1) → (key2, val2)
  - Reduce: (key2, [val2]) → [val3]
- Example
  - Given a file
    - A file may be divided into multiple parts (splits).
  - Each record (line) is processed by a Map func,
    - written by user,
    - takes an input key/value pair
    - produces a set of intermediate key/value pairs.
    - e.g. (doc—id, doc-content)
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
    - Input: (docno, doc_content), …
    - Output: (docno, [class, class, …]), …
  - No reduce tasks

### Filtering & Accumulation

- Counting total enrollments of 2 given student classes
- Map selects records & outputs initial counts
  - In: (Jamie, 11741), (Tom, 11493), …
  - Out: (11741, 1), (11493, 1), …
- Shuffle/Partition by class_id
- Sort
  - In: (11741, 1), (11493, 1), (11741, 1), …
  - Out: (11493, 1), …, (11741, 1), (11741, 1), …
- Reduce accumulates counts
  - In: (11493, [1, 1, …]), (11741, [1, 1, …])
  - Sum & Output: (11493, 16), (11741, 35)

### Database Join

- A JOIN is a means for combining fields from 2 tables by using values common to each.
- For each employee, find department he works in
- Problem: Massive lookups
  - Given 2 large lists: (URL, ID) & (URL, doc_content) pairs
  - Produce (URL, ID, doc_content) or (ID, doc_content)
- Solution:
  - Input stream: both (URL, ID) & (URL, doc_content) lists
    - (a/post, 0), (b/submit, 1), …
    - (a/post, <html0>), (b/submit, <html1>), …
  - Map simply passes input along,
  - Shuffle & Sort on URL (group ID & doc_content for same URL together)
    - Out
      - (a/post, 0), (a/post, <html0>), (b/submit, <html1>), (b/submit, 1), …
  - Reduce outputs result stream of (ID, doc_content) pairs
    - In
      - (a/post, [0, html0]), (b/submit, [html1, 1]), …
    - Out
      - (0, <html0>), (1, <html1>), …

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
- Output: (term, [docid, docid, …])
  - E.g., (apple, [1, 23, 49, 127, …])
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
  - What happens if a server fails or joins (k -> k±1)?
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
    if (id Î (n, successor])
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
where("age > 21")
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
messages = errors.map(_.split(‘\t')(2))
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

- Common Transformation & Actions
- Persistence (Caching)
  53
  53
  RDD Operations
- 2 types of operations
- Transformations: operations that return a new
  RDDs e.g., map(), filter()
- Actions: operations that return a result to driver
  program or write it to storage such as count(),
  first()
- Treated differently by Spark
- Transformation: lazy evaluation
- Action: execution at any time
  54
  54
  27
  05/12/2022
  Transformation
- Example 1. Use filter()
- Python
  inputRDD = sc.textFile("log.txt")
  errorsRDD = inputRDD.filter(lambda x: "error" in x)
- Scala
  val inputRDD = sc.textFile("log.txt")
  val errorsRDD = inputRDD.filter(line =>
  line.contains("error"))
- Java
  JavaRDD<String> inputRDD = sc.textFile("log.txt");
  JavaRDD<String> errorsRDD = inputRDD.filter(
  new func<String, Boolean>() {
  public Boolean call(String x) {
  return x.contains("error"); }}
  });
  55
  55
  Transformation
- filter()
- does not change existing inputRDD
- returns a pointer to an entirely new RDD
- inputRDD still can be reused
- union()
  errorsRDD = inputRDD.filter(lambda x: "error" in x)
  warningsRDD=inputRDD.filter(lambda x: "warning" in x)
  badLinesRDD = errorsRDD.union(warningsRDD)
- transformations can operate on any no.
  input RDDs
  56
  56
  28
  05/12/2022
  Transformation
- Spark keeps track dependencies between
  RDDs, called lineage graph
- Allow recovering lost data
  57
  57
  Actions
- Example. count no. errors
- Python
  print "Input had " + badLinesRDD.count() + " concerning lines"
  print "Here are 10 examples:"
  for line in badLinesRDD.take(10):
  print line
- Scala
  println("Input had " + badLinesRDD.count() + " concerning
  lines")
  println("Here are 10 examples:")
  badLinesRDD.take(10).foreach(println)
- Java
  sys.out.println("Input had " + badLinesRDD.count() + "
  concerning lines")
  sys.out.println("Here are 10 examples:")
  for (String line: badLinesRDD.take(10)) {
  sys.out.println(line);
  58
  }
  58
  29
  05/12/2022
- RDD Basics
- Creating RDDs
  Resilient
  Distributed
  Dataset -
  RDD
- RDD Operations
- Common Transformation & Actions
- Persistence (Caching)
  59
  59
  RDD Basics
  Transformations
  map
  flatMap
  filter
  sample
  union
  groupByKey
  reduceByKey
  join
  cache
  …
  Actions
  reduce
  collect
  count
  save
  lookupKey
  …
  Email: info@pti.edu.vn | Website: pti.edu.vn
  60
  30
  05/12/2022
  Transformations
  61
  Transformations
  62
  31
  05/12/2022
  Actions
  63
  Actions
  64
  32
  05/12/2022
  Resilient
  Distributed
  Dataset -
  RDD
- RDD Basics
- Creating RDDs
- RDD Operations
- Common Transformation & Actions
- Persistence (Caching)
  65
  65
  Persistence levels
  66
  66
  33
  05/12/2022
  Persistence
- Example
  val result = input.map(x => x \* x)
  result.persist(StorageLevel.DISK_ONLY)
  println(result.count())
  println(result.collect().mkString(","))
  67
  67
  Books:
- Holden Karau, Andy Konwinski,
  Patrick Wendell & Matei Zaharia.
  Learning Spark. Oreilly
- TutorialsPoint. Spark Core
  Programming
  Acknowledgement
  & References
  Slides:
- Paco Nathan. Intro to Apache
  Spark
- Harold Liu. Berkely Data
  Analytics Stack
- DataBricks. Intro to Spark
  Development
  68
  68
  34
  19/12/2022
  Lecture 2: Load & Inspect Data in Spark
  1
  IT4043E
  Lưu trữ và phân tích data lớn
  IT4043E
  12/2022
  Thanh-Chung Dao Ph.D.
  1
  Agenda
  Zeppelin notebook
  Load, inspect, & save data
  What & why we need it?
  Loading data from difference sources
  Installation using Docker
  Simple inspecting commands
  Usage
  Saving data
  2
  2
  1
  19/12/2022
  Zeppelin notebook
- A web-based interface for interactive data
  analytics
- Easy to write & access your code
- Support many programming languages
- Scala (with Apache Spark), Python (with Apache Spark),
  SparkSQL, Hive, Markdown, Angular, & Shell
- Data visualization
- Monitoring Spark jobs
  3
  3
  Installation using Docker
- Install Docker & login
- https://docs.docker.com/docker-for-windows/install/
- https://docs.docker.com/docker-for-mac/install/
- Download lecture's git repository
- https://github.com/bk-blockchain/big-data-class
- Run Zeppelin using docker-composer
- docker-compose up -d --build spark_master
- http://localhost
  4
  4
  2
  19/12/2022
  Zeppelin usage
- Run first node: "About this Build"
- Check Spark version
- Check Spark running mode
- http://localhost:4040
- Need to start Spark first by running first note
- Run second node: "Tutorial/Basic Features
  (Spark)"
- Load data into table
- SQL example
  5
  5
  Useful Docker commands
- Login to a container
- docker ps (get any container id)
- docker exec -it container_id bash
- List all containers: docker ps -a
- Stop a container: docker stop container_id
- Start a stopped container: docker start
  container_id
  6
  6
  3
  19/12/2022
  Load, inspect, & save data
- Data is always huge that does not fit on a
  single machine
- Data is distributed on many storage nodes
- Data scientists can likely focus on format
  that their data is already in
- Engineers may wish to explore more output formats
- Spark supports a wide range of input & output sources
  7
  7
  Data sources
- File formats & filesyss
- Local or distributed filesys, such as NFS, HDFS,
  or Amazon S3
- File formats including text, JSON, SequenceFiles,
  & protocol buffers
- Structured data sources through Spark SQL
- Apache Hive
- Parquet
- JSON
- From RDDs
- Databases & key/value stores
- Cassandra, HBase, Elasticsearch, & JDBC dbs
  8
  8
  4
  19/12/2022
  File Formats
- Formats range from unstructured, like text, to
  semistructured, like JSON, to structured, like
  SequenceFiles
  From Learning Spark [1]
  9
  9
  Lab: loading, inspecting, & saving data
- On Zeppelin notebook
- http://localhost:8080/#/notebook/2EAMFFAH7
  10
  10
  5
  19/12/2022
  References
- [1] Karau, Holden, et al. Learning spark:
  lightning-fast big data analysis. " O'Reilly
  Media, Inc.", 2015.
  11
  11
  6
  12/19/22
  Lecture 3: Workinng with Pair RDDs & DataFrame
  1
  IT4043E
  Tích hợp và xử lý data lớn
  IT4043E
  12/2022
  Thanh-Chung Dao Ph.D.
  1
  From where to learn Spark ?
  http://spark.apache.org/
  http://shop.oreilly.com/product/0636920028512.do
  2
  1
  12/19/22
  Spark architecture
  3
  Easy ways to run Spark ?
  ★ your IDE (ex. Eclipse or IDEA)
  ★ Standalone Deploy Mode: simplest way to deploy Spark
  on a single machine
  ★ Docker & Zeppelin
  ★ EMR
  ★ Hadoop vendors (Cloudera, Hortonworks)
  Digital Ocean (Kuberneste cluster)
  4
  2
  12/19/22
  Supported languages
  5
  RDD
  An RDD is simply an immutable distributed collection of
  objects!
  a
  b
  c
  d
  e
  f
  g
  h
  i
  j
  k
  l
  m
  n
  o
  p
  q
  6
  3
  12/19/22
  RDD (Resilient Distributed Dataset)
  RDD (Resilient Distributed Dataset)
  - Resilient: If data in memory is lost, it can be recreated
  - Distributed: Processed across cluster
  - Dataset: Initial data can come from a source such as a
    file, or it can be created programmatically
- RDDs are fundamental unit of data in Spark
- Most Spark programming consists of performing
  operations on RDDs
  7
  Creating RDD (I)
  Python
  lines = sc.parallelize(["workshop", "spark"])
  Scala
  val lines = sc.parallelize(List("workshop", "spark"))
  Java
  JavaRDD<String> lines = sc.parallelize(Arrays.asList("workshop", "spark"))
  8
  4
  12/19/22
  Creating RDD (II)
  Python
  lines = sc.textFile("/path/to/file.txt")
  Scala
  val lines = sc.textFile("/path/to/file.txt")
  Java
  JavaRDD<String> lines = sc.textFile("/path/to/file.txt")
  9
  RDD persistence
  MEMORY_ONLY
  MEMORY_AND_DISK
  MEMORY_ONLY_SER
  MEMORY_AND_DISK_SER
  DISK_ONLY
  MEMORY_ONLY_2
  MEMORY_AND_DISK_2
  OFF_HEAP
  10
  5
  12/19/22
  Working with RDDs
  11
  RDDs
  RDDs can hold any serializable type of element
  -Primitive types such as integers, characters, & booleans
  -Sequence types such as strings, lists, arrays, tuples,
  & dicts (including nested data types)
  -Scala/Java Objects (if serializable)
  -Mixed types
  § Some RDDs are specialized & have additional
  funcality
  -Pair RDDs
  -RDDs consisting of key-value pairs
  -Double RDDs
  -RDDs consisting of numeric data
  12
  6
  12/19/22
  Creating RDDs from Collections
  You can create RDDs from collections instead of files
  -sc.parallelize(collection)
  myData = ["Alice","Carlos","Frank","Barbara"]
  > myRdd = sc.parallelize(myData)
  > myRdd.take(2) ['Alice', 'Carlos']
  > 13
  > Creating RDDs from Text Files (1)
  > For file-based RDDs, use SparkContext.textFile
  >
  > - Accepts a single file, a directory of files, a wildcard list of
  >   files, or a comma-separated list of files. Examples:
  >   -sc.textFile("myfile.txt")
  >   -sc.textFile("mydata/")
  >   -sc.textFile("mydata/\*.log")
  >   -sc.textFile("myfile1.txt,myfile2.txt")
  >   -Each line in each file is a separate record in RDD
  >   Files are referenced by absolute or relative URI
  >   -Absolute URI:
  >   -file:/home/training/myfile.txt
  >   -hdfs://nnhost/loudacre/myfile.txt
  >   14
  >   7
  >   12/19/22
  >   Examples: Multi-RDD Transformations (1)
  >   15
  >   Examples: Multi-RDD Transformations (2)
  >   16
  >   8
  >   12/19/22
  >   Some Other General RDD Operations
  >   Other RDD operations
  >   -first returns first element of RDD
  >   -foreach applies a func to each element in an RDD
  >   -top(n) returns largest n elements using natural ordering
  >   Sampling operations
  >   -sample creates a new RDD with a sampling of elements
  >   -take Sample returns an array of sampled elements
  >   17
  >   Other data structures in Spark
  >   ★ Paired RDD
  >   ★ DataFrame
  >   ★ DataSet
  >   18
  >   9
  >   12/19/22
  >   Paired RDD
  >   Paired RDD = an RDD of key/value pairs
  >   user1
  >   id1/user1
  >   user2
  >   id2/user2
  >   user3
  >   user4
  >   id3/user3
  >   id4/user4
  >   user5
  >   id5/user5
  >   19
  >   Pair RDDs
  >   20
  >   10
  >   12/19/22
  >   Pair RDDs
  >   § Pair RDDs are a special form of RDD
  >   -Each element must be a keyvalue pair (a two-element tuple)
  >   -Keys & values can be any type
  >   § Why?
  >   -Use with map-reduce algorithms
  >   -Many additional funcs are
  >   available for common data
  >   processing needs
  >   -Such as sorting, joining, grouping,
  >   & counting
  >   21
  >   Creating Pair RDDs
  >   The first step in most workflows is to get data into
  >   key/value form
  >   -What should RDD should be keyed on?
  >   -What is value?
  >   Commonly used funcs to create pair RDDs
  >   -map
  >   -flatMap / flatMapValues
  >   -keyBy
  >   22
  >   11
  >   12/19/22
  >   Example: A Simple Pair RDD
  >   Example: Create a pair RDD from a tab-separated file
  >   23
  >   Example: Keying Web Logs by User ID
  >   24
  >   12
  >   12/19/22
  >   Mapping Single Rows to Multiple Pairs
  >   25
  >   Answer : Mapping Single Rows to
  >   Multiple Pairs
  >   26
  >   13
  >   12/19/22
  >   Map-Reduce
  >   § Map-reduce is a common programming model
  >   -Easily applicable to distributed processing of large
  >   data sets
  >   § Hadoop MapReduce is major implementation
  >   -Somewhat limited
  >   -Each job has 1 map phase, 1 reduce phase
  >   -Job output is saved to files
  >   § Spark implements map-reduce with much greater
  >   flexibility
  >   -Map & reduce funcs can be interspersed
  >   -Results can be stored in memory
  >   -Operations can easily be chained
  >   27
  >   Map-Reduce in Spark
  >   § Map-reduce in Spark works on pair RDDs
  >   § Map phase
  >   -Operates on 1 record at a time
  >   -"Maps" each record to zero or more new records
  >   -Examples: map, flatMap, filter, keyBy
  >   § Reduce phase
  >   -Works on map output
  >   -Consolidates multiple records
  >   -Examples: reduceByKey, sortByKey, mean
  >   28
  >   14
  >   12/19/22
  >   Example: Word Count
  >   29
  >   reduceByKey
  >   The func passed to reduceByKey combines values
  >   from 2 keys
  > - func must be binary
  >   30
  >   15
  >   12/19/22
  >   val counts = sc.textFile (£i1e) . flat.Map
  >   (line => line.sp lit (' ')) . map (word => (word
  >   ,l)) . reduceByKey ((vl ,v2 ) => vl+v2)
  >   OR
  >   ,,
  >   val counts = sc.textFile (£i1e) . flat.Map
  >   (_.split (' 1 ) ) -
  >   map ((_ ,1)) .
  >   reduceByKey(_+_ )
  >   31
  >   Pair RDD Operations
  >   § In addition to map & reduceByKey operations, Spark
  >   has several operations specific to pair RDDs
  >   § Examples
  >   -countByKey returns a map with count of
  >   occurrences
  >   of each key
  >   -groupByKey groups all values for each key in an
  >   RDD
  >   -sortByKey sorts in ascending or descending order
  >   -join returns an RDD containing all pairs with matching
  >   keys from 2 RDD
  >   32
  >   16
  >   12/19/22
  >   Example: Pair RDD Operations
  >   33
  >   Example: Joining by Key
  >   34
  >   17
  >   12/19/22
  >   Other Pair Operations
  >   § Some other pair operations
  >   -keys returns an RDD of just keys, without values
  >   -values returns an RDD of just values, without keys
  >   -lookup(key) returns value(s) for a key
  >   -leftOuterJoin, rightOuterJoin , fullOuterJoin join 2 RDDs,
  >   including keys defined in left, right or either RDD
  >   respectively
  >   -mapValues, flatMapValues execute a func on just > values,
  >   keeping key same
  >   35
  >   DataFrames & Apache Spark SQL
  >   36
  >   18
  >   12/19/22
  >   What is Spark SQL?
  >   §
  >   What is Spark SQL?
  >   -Spark module for structured data processing
  >   -Replaces Shark (a prior Spark module, now deprecated)
  >   -Built on top of core Spark
  >   § What does Spark SQL provide?
  >   -The DataFrame API—a library for working with data as
  >   tables
  >   -Defines DataFrames containing rows & columns
  >   -DataFrames are focus of this chapter!
  >   -Catalyst Optimizer—an extensible optimization framework
  >   -A SQL engine & command line interface
  >   37
  >   SQL Context
  >   § main Spark SQL entry point is a SQL context object
  >   -Requires a SparkContext object
  >   -The SQL context in Spark SQL is similar to Spark context in
  >   core Spark
  >   § There are 2 implementations
  >   -SQLContext
  >   -Basic implementation
  >   -HiveContext
  >   -Reads & writes Hive/HCatalog tables directly
  >   -Supports full HiveQL language
  >   -Requires Spark application be linked with Hive libraries
  >   -Cloudera recommends using HiveContext
  >   38
  >   19
  >   12/19/22
  >   Creating a SQL Context
  >   §
  >   The Spark shell creates a HiveContext instance automatically
  >   -Call sqlContext
  >   -You will need to create 1 when writing a Spark
  >   application
  >   -Having multiple SQL context objects is allowed
  >   § A SQL context object is created based on Spark context
  >   39
  >   DataFrames
  >   § DataFrames are main abstraction in Spark SQL
  >   -Analogous to RDDs in core Spark
  >   -A distributed collection of structured data organized
  >   into Named columns
  >   -Built on a base RDD containing Row objects
  >   40
  >   20
  >   12/19/22
  >   Creating a DataFrame from a Data
  >   Source
  >   §
  >   sqlContext.read returns a DataFrameReader object
  >   § DataFrameReader provides funcality to load data into
  >   a DataFrame
  >   § Convenience funcs
  >   -json(filename)
  >   -parquet(filename)
  >   -orc(filename)
  >   -table(hive-tablename)
  >   -jdbc(url,table,options)
  >   41
  >   Example: Creating a DataFrame from a
  >   JSON File
  >   42
  >   21
  >   12/19/22
  >   Example: Creating a DataFrame from a
  >   Hive/Impala Table
  >   43
  >   Loading from a Data Source Manually
  >   § You can specify settings for DataFrameReader
  >   -format: Specify a data source type
  >   -option: A key/value setting for underlying data source
  >   -schema: Specify a schema instead of inferring from data
  >   source
  >   § Then call generic base func load
  >   44
  >   22
  >   12/19/22
  >   Data Sources
  >   Spark SQL 1.6 built-in data source types
  >   -table
  >   -json
  >   -parquet
  >   -jdbc
  >   -orc
  >   § You can also use third party data source libraries, such as
  >   -Avro (included in CDH)
  >   -HBase
  >   -CSV
  >   -MySQL
  >   -and more being added all time
  >   §
  >   45
  >   DataFrame Basic Operations
  >   Basic operations deal with DataFrame metadata (rather than
  >   its data)
- § Some examples
- -schema returns a schema object describing data
- -printSchema displays schema as a visual tree
- -cache / persist persists DataFrame to disk or memory
- §
- -columns returns an array containing names of columns
- -dtypes returns an array of (column name,type) pairs
- -explain prints debug information about DataFrame to
  console
  46
  23
  12/19/22
  DataFrame Basic Operations
  47
  DataFrame Actions
  §
  Some DataFrame actions
  -collect returns all rows as an array of Row
  objects
  -take(n) returns first n rows as an array
  of Row objects
  -count returns no. rows
  -show(n)displays first n rows
  (default=20)
  48
  24
  12/19/22
  DataFrame Queries
  DataFrame query methods return new DataFrames
  - Queries can be chained like transformations
    § Some query methods
    -distinct returns a new DataFrame with distinct elements of
    this DF
    -join joins this DataFrame with a second DataFrame
  - Variants for inside, outside, left, & right joins
    -limit returns a new DataFrame with first n rows of this DF
    -select returns a new DataFrame with data from 1 or
    more columns of base DataFrame
    -where returns a new DataFrame with rows meeting
    specified query criteria (alias for filter)
    §
    49
    DataFrame Query Strings
    50
    25
    12/19/22
    Querying DataFrames using Columns
    § Columns can be referenced in multiple ways
    51
    Joining DataFrames
    §
    A basic inner join when join column is in both DataFrames
    52
    26
    12/19/22
    Joining DataFrames
    53
    SQL Queries
    § When using HiveContext, you can query Hive/Impala
    tables using HiveQL
  - Returns a DataFrame
    54
    27
    12/19/22
    Saving DataFrames
    Data in DataFrames can be saved to a data source
    § Use DataFrame.write to create a DataFrameWriter
    § DataFrameWriter provides convenience funcs to
    externally save
    the data represented by a DataFrame
    -jdbc inserts into a new or existing table in a database
    -json saves as a JSON file
    -parquet saves as a Parquet file
    -orc saves as an ORC file
    -text saves as a text file (string data in a single column only)
    -saveAsTable saves as a Hive/Impala table (HiveContext only)
    §
    55
    Options for Saving DataFrames
    § DataFrameWriter option methods
    -format specifies a data source type
    -mode determines behavior if file or
    table already exists:
    overwrite, append, ignore or error (default
    is error)
    -partitionBy stores data in partitioned
    directories in form
    column=value (as with Hive/Impala
    partitioning)
    -options specifies properties for target
    data source
    -save is generic base func to write
    the data
    56
    28
    12/19/22
    DataFrames & RDDs
    § DataFrames are built on RDDs
    -Base RDDs contain Row objects
    -Use rdd to get underlying RDD
    57
    DataFrames & RDDs
    § Row RDDs have all standard Spark actions & transformations
    -Actions: collect, take, count, & so on
    -Transformations: map, flatMap, filter, & so on
    § Row RDDs can be transformed into pair RDDs to use
    mapreduce methods
    § DataFrames also provide convenience methods (such as
    map, flatMap,and foreach)for converting to RDDs
    58
    29
    12/19/22
    Working with Row Objects
    -Use Array-like syntax to return values with type Any
    -row(n) returns element in nth column
    -row.fieldIndex("age")returns index of age column
    -Use methods to get correctly typed values
    -row.getAs[Long]("age")
    -Use type-specific get methods to return typed values
    -row.getString(n) returns nth column as a string
    -row.getInt(n) returns nth column as an integer
    -And so on
    59
    Example: Extracting Data from Row
    Objects
    60
    30
    12/19/22
    Converting RDDs to DataFrames
    §
    You can also create a DF from an RDD using createDataFrame
    61
    Working with
    Spark RDDs, Pair-RDDs
    © 2019 Binh Minh
    Nguyen
    Hanoi University of Science & Technology
    62
    62
    31
    12/19/22
    RDD Operations
    Transformations
    map()
    flatMap()
    filter()
    union()
    intersection()
    distinct()
    groupByKey()
    reduceByKey()
    sortByKey()
    join()
    …
    Actions
    count()
    collect()
    first(), top(n)
    take(n), takeOrdered(n)
    countByValue()
    reduce()
    foreach()
    …
    © 2019 Binh Minh
    Nguyen
    Hanoi University of Science & Technology
    63
    63
    Lambda Expression
    PySpark WordCount example:
    input_file = sc.textFile("/path/to/text/file")
    map = input_file.flatMap(lambda line: line.split(" ")) \
    .map(lambda word: (word, 1))
    counts = map.reduceByKey(lambda a, b: a + b)
    counts.saveAsTextFile("/path/to/output/")
    lambda arguments: expression
    © 2019 Binh Minh
    Nguyen
    Hanoi University of Science & Technology
    64
    64
    32
    12/19/22
    PySpark RDD API
    https://spark.apache.org/docs/latest/api/python/pyspark.htm
    l#pyspark.RDD
    © 2019 Binh Minh
    Nguyen
    Hanoi University of Science & Technology
    65
    65
    Practice with flight data (1)
    Data: airports.dat (https://openflights.org/data.html)
    [Airport ID, Name, City, Country, IATA, ICAO, Latitude, Longitude, Altitude,
    Timezone, DST, Tz database, Type, Source]
    Try to do somethings:
- Create RDD from textfile
- Count no. airports
- Filter by country
- Group by country
- Count no. airports in each country
  © 2019 Binh Minh
  Nguyen
  Hanoi University of Science & Technology
  66
  66
  33
  12/19/22
  Practice with flight data (2)
  - Data: airports.dat (https://openflights.org/data.html)
    [Airport ID, Name, City, Country, IATA, ICAO, Latitude, Longitude, Altitude,
    Timezone, DST, Tz database, Type, Source]
  - Data: routes.dat
    [Airline, Airline ID, Source airport, Source airport ID, Destination airport,
    Destination airport ID, Codeshare, Stops, Equipment]
    Try to do somethings:
- Join 2 RDD
- Count no. flights arriving in each country
  © 2019 Binh Minh
  Nguyen
  Hanoi University of Science & Technology
  67
  67
  Working with
  DataFrame & Spark SQL
  © 2019 Binh Minh
  Nguyen
  Hanoi University of Science & Technology
  68
  68
  34
  12/19/22
  Creating a DataFrame(1)
  © 2019 Binh Minh
  Nguyen
  Hanoi University of Science & Technology
  69
  69
  Creating a DataFrame
  From CSV file:
  From RDD:
  © 2019 Binh Minh
  Nguyen
  Hanoi University of Science & Technology
  70
  70
  35
  12/19/22
  DataFrame APIs
- DataFrame: show(), collect(), createOrReplaceTempView(),
  distinct(), filter(), select(), count(), groupBy(), join()…
- Column: like()
- Row: row.key, row[key]
- GroupedData: count(), max(), min(), sum(), …
  https://spark.apache.org/docs/latest/api/python/pyspark.sql.html
  © 2019 Binh Minh
  Nguyen
  Hanoi University of Science & Technology
  71
  71
  Spark SQL
- Create a temporary view
- Query using SQL syntax
  © 2019 Binh Minh
  Nguyen
  Hanoi University of Science & Technology
  72
  72
  36
  26/12/2022
  Lecture 4: Build simple Spark applications
  1
  IT4043E
  Tích hợp và xử lý data lớn
  IT4043E
  12/2022
  Thanh-Chung Dao Ph.D.
  1
  Spark running mode
- Local
- Clustered
- Spark Standalone
- Spark on Apache Mesos
- Spark on Hadoop YARN
  2
  2
  1
  26/12/2022
  Hello World: Word-Count
  Figure from [1]
  3
  3
  Run using command line
- Turn on docker bash
- spark-submit wordcount.py README.md
- Result will be shown as follows
  4
  4
  2
  26/12/2022
  Lab: Word-Count
- Lab on Zeppelin notebook
- Github source code
- https://github.com/bk-blockchain/big-data-class
  5
  5
  Flight data:
- Analyzing flight data from United States
  Bureau of Transportation statistics
- Lab on Zeppelin notebook
- Github source code
- https://github.com/bk-blockchain/big-data-class
  6
  6
  3
  26/12/2022
  References
- [1]
  https://datamize.wordpress.com/2015/02/08/vi
  sualizing-basic-rdd-operations-throughwordcount-in-pyspark/
  7
  7
  4
  09/01/2023
  Lecture 5: Spark Streaming
  1
  Big Data Processing
  1/2023
  Thanh-Chung Dao Ph.D.
  1
  Agenda
- What is Spark Streaming
- Operation on DStreams
  2
  2
  1
  09/01/2023
  What is Spark Streaming
  3
  Email: info@pti.edu.vn | Website: pti.edu.vn
  3
  Spark Streaming
- Scalable, fault-tolerant stream processing
  sys
- Receive data streams from input sources,
  process them in a cluster, push out to
  databases/dashboards
  4
  4
  2
  09/01/2023
  How does it work?
- stream is treated as a series of very small,
  deterministic batches of data
- Spark treats each batch of data as RDDs & processes them using RDD operations
- Processed results are pushed out in batches
  5
  5
  Discretized Stream (DStream)
- Sequence of RDDs representing a stream of
  data
  6
  6
  3
  09/01/2023
  Discretized Stream (DStream)
- Any operation applied on a DStream translates
  to operations on underlying RDDs
  7
  7
  StreamingContext
- main entry point of all Spark Streaming
  funcality
  val conf = new
  SparkConf().setAppName(appName).setMaster(master)
  val ssc = new StreamingContext(conf, batchinterval)
- appname: name of application
- master: a Spark, Mesos, or YARN cluster URL
- batchinternval: time interval (in second) of
  each batch
  8
  8
  4
  09/01/2023
  Operation on DStreams
  9
  Email: info@pti.edu.vn | Website: pti.edu.vn
  9
  Operation on DStreams
- 3 categories
- Input operation
- Transformation operation
- Output operation
  10
  10
  5
  09/01/2023
  Input Operations
- Every input DStream is associated with a
  Receiver object
- 2 built-in categories of streaming sources:
- Basic sources, e.g., FSs, socket connection
- Advanced sources, e.g., Twitter, Kafka
  11
  11
  Input Operations
- Basic sources
- Socket connection
  // Create a DStream that will connect to hostname:port
  ssc.socketTextStream("localhost", 9999)
- File stream
  streamingContext.fileStream[…](dataDirectory)
- Advanced sources
  val ssc = new StreamingContext(sparkContext, Seconds(1))
  val tweets = TwitterUtils.createStream(ssc, auth)
  12
  12
  6
  09/01/2023
  Transformation
  13
  13
  Transformation
  Transformation
  Meaning
  map (func)
  Return a new DStream by passing each element of
  the source DStream through a func func
  flatmap(func)
  Similar to map, but each input item can be mapped
  to 0 or more output items
  filter(func)
  Return a new DStream by selecting only records
  of source DStream on which func returns true
  14
  14
  7
  09/01/2023
  Transformation
  Transformation
  Meaning
  count
  Return a new DStream of single-element RDDs by
  counting no. elements in each RDD of
  the source DStream
  countbyValue
  Returns a new DStream of (K, Long) pairs where
  the value of each key is its frequency in each RDD
  of source DStream.
  reduce(func)
  Return a new DStream of single-element RDDs by
  aggregating elements in each RDD of source DStream using a func func (which takes
  2 arguments & returns one).
  reducebyKey(func)
  When called on a DStream of (K, V) pairs, return a
  new DStream of (K, V) pairs where values for
  each key are aggregated using given reduce
  func
  15
  15
  Transformation
  Transformation
  Meaning
  union(otherStream) Return a new DStream that contains union of
  the elements in source DStream & otherDStream.
  join(otherStream)
  When called on 2 DStreams of (K, V) & (K, W)
  pairs, return a new DStream of (K, (V, W)) pairs
  with all pairs of elements for each key.
  16
  16
  8
  09/01/2023
  Window Operations
- Spark provides a set of transformations that
  apply to a sliding window of data
- A window is defined by: window length & siding interval
  17
  17
  Window Operations
- window(windowLength, slideInterval)
- Returns a new DStream which is computed based on
  windowed batches
- countByWindow(windowLength, slideInterval)
- Returns a sliding window count of elements in stream.
- reduceByWindow(func, windowLength,
  slideInterval)
- Returns a new single-element DStream, created by
  aggregating elements in stream over a sliding
  interval using func.
  18
  18
  9
  09/01/2023
  Output Operation
- Push out DStream's data to external syss,
  e.g., a database or a FS
  Operation
  Meaning
  print
  Prints first ten elements of every batch of data
  in a DStream on driver node running application
  saveAsTextFiles
  Save this DStream's contents as text files
  saveAsHadoopFiles
  Save this DStream's contents as Hadoop files.
  foreachRDD(func)
  Applies a func, func, to each RDD generated
  from stream
  19
  19
  Example
  Word Count
  val context = new StreamingContext(conf, Seconds(1))
  val lines = context.socketTextStream(...)
  val words = lines.flatMap(_.split(" "))
  val wordCounts = words.map(x => (x, 1)).reduceByKey(_+\_)
  wordCounts.print()
  context.start()
  Print DStream contents on screen
  Start streaming job
  20
  20
  10
  09/01/2023
  Lifecycle of a streaming app
  21
  21
  Execution in any Spark Application
  22
  22
  11
  09/01/2023
  Execution in Spark Streaming: Receiving data
  23
  23
  Execution in Spark Streaming: Processing data
  24
  24
  12
  09/01/2023
  End-to-end view
  DStreamGraph
  Input
  DStreams
  DAG of RDDs
  every interval
  T
  B
  B
  T
  U
  t.saveAsHadoopFiles(…)
  t.map(…).foreach(…)
  t.filter(…).foreach(…)
  Stage 2
  Stage 2
  Stage 2
  M
  M
  M
  F
  M
  M
  F
  FE
  FE
  M
  F
  M
  Output
  operations
  YOU
  write this
  B
  U
  M
  FE
  Stage 1
  Stage 1
  Stage 1
  B
  B
  U
  U
  t = t1.union(t2).map(…)
  Tasks
  every interval
  BlockRDDs
  B
  t1 = ssc.socketStream("…")
  t2 = ssc.socketStream("…")
  DAG of stages
  every interval
  Executors
  Streaming app
  F
  Stage 3
  Stage 3
  Stage 3
  RDD Actions /
  Spark Jobs
  Spark Streaming
  JobScheduler + JobGenerator
  Spark
  DAGScheduler
  Spark
  TaskScheduler
  16
  25
  25
  Dynamic Load Balancing
  26
  26
  13
  09/01/2023
  Fast failure & Straggler recovery
  27
  27
  Books:
- Holden Karau, Andy Konwinski,
  Patrick Wendell & Matei Zaharia.
  Learning Spark. Oreilly
- James A. Scott. Getting started with
  Apache Spark. MapR Technologies
  Acknowledgement
  & References
  Slides:
- Amir H. Payberah. Scalable Stream
  Processing - Spark Streaming & Flink
- Matteo Nardelli. Spark Streaming:
  Hands on Session
- DataBricks. Spark Streaming
- DataBricks: Spark Streaming: Best
  Practices
  28
  28
  14
  30/01/2023
  L6: Use Spark ML to do basic Machine learning algorithm
  1
  Big Data Processing
  1/2023
  Thanh-Chung Dao Ph.D.
  1
  Machine learning
  From [1]
  2
  2
  1
  30/01/2023
  Machine Learning Lifecycle
- 2 major phases
- Training Set
- You have complete training dataset
- You can extract features & train to fit a model.
- Testing Set
- Once model is obtained, you can predict using model obtained on training set
  From [2]
  3
  3
  Spark ML & PySpark
- Spark ML is a machine-learning library
- Classification: logistic regression, naive Bayes
- Regression: generalized linear regression, survival regression
- Decision trees, random forests, & gradient-boosted trees
- Recommendation: alternating least squares (ALS)
- Clustering: K-means, Gaussian mixtures (GMMs)
- Topic modeling: latent Dirichlet allocation (LDA)
- Frequent item sets, association rules, & sequential pattern mining
- PySpark is an interface for using Python
  From [2]
  4
  4
  2
  30/01/2023
  Binary Classification Example [3]
- Binary Classification is task of predicting
  a binary label
- Is an email spam or not spam?
- Should I show this ad to this user or not?
- Will it rain tomorrow or not?
- Adult dataset
- https://archive.ics.uci.edu/ml/datasets/Adult
- 48842 individuals & their annual income
- We will use this information to predict if an
  individual earns <=50K or >50k a year
  5
  5
  Dataset Information
- Attribute Information:
  - age: continuous
  -
  - workclass: Private,Self-emp-not-inc, Self-emp-inc, Federal-gov, Local-gov, State-gov, Without-pay,
    Never-worked
    fnlwgt: continuous
  -
  - education: Bachelors, Some-college, 11th, HS-grad, Prof-school, Assoc-acdm, Assoc-voc...
    education-num: continuous
  - marital-status: Married-civ-spouse, Divorced, Never-married, Separated, Widowed, Married-spouseabsent...
    occupation: Tech-support, Craft-repair, Other-service, Sales, Exec-managerial, Prof-specialty, Handlerscleaners...
  -
  -
  - relationship: Wife, Own-child, Husband, Not-in-family, Other-relative, Unmarried
    race: White, Asian-Pac-Islander, Amer-Indian-Eskimo, Other, Black
  -
  - sex: Female, Male
    capital-gain: continuous
  -
  -
  - capital-loss: continuous
    hours-per-week: continuous
    native-country: United-States, Cambodia, England, Puerto-Rico, Canada, Germany...
- Target/Label: - <=50K, >50K
  6
  6
  3
  30/01/2023
  Analyzing Flow
- Load data
- Preprocess Data
- Fit & Evaluate Models
- Logistic Regression
- Decision Trees
- Random Forest
- Make Classification
  7
  7
  Lab: Running Binary Classification
  on Zeppelin
- Get prepared notebook
- Run & try to understand algorithms
  8
  8
  4
  30/01/2023
  References
- [1]
  https://blogs.oracle.com/bigdata/difference-aimachine-learning-deep-learning
- [2] https://www.edureka.co/blog/pysparkmllib-tutorial/
- [3]
  https://docs.databricks.com/spark/latest/mllib/
  binary-classification-mllib-pipelines.html
  9
  9
  5
  30/01/2023
  L7: Use Spark ML to Predict Flight Delays
  1
  Big Data Processing
  1/2023
  Thanh-Chung Dao Ph.D.
  1
  Spark ML
- Spark ML is a machine-learning library
- Classification: logistic regression, naive Bayes
- Regression: generalized linear regression, survival regression
- Decision trees, random forests, & gradient-boosted trees
- Recommendation: alternating least squares (ALS)
- Clustering: K-means, Gaussian mixtures (GMMs)
- Topic modeling: latent Dirichlet allocation (LDA)
- Frequent item sets, association rules, & sequential pattern mining
  2
  2
  1
  30/01/2023
  Classification vs Prediction
- Classification models predict categorical class
  labels [2]
- Binary classification
- Prediction models predict continuous valued
  funcs
- Regression analysis is a statistical methodology that
  is most often used for numeric prediction
  3
  3
  Predicting arrival delay of
  commercial flights [1]
- Problem
- We want to be able to predict, based on historical data
- arrival delay of a flight using only information
  available before flight takes off
- Dataset
- http://stat-computing.org/dataexpo/2009/the-data.html
- data used was published by US Department of
  Transportation
- It compromises almost 23 years worth of data
- Approach
- Using a regression algorithm
  4
  4
  2
  30/01/2023
  Dataset Information
  Name
  Year
  Description
  1
  2
  Month
  1-12
  3
  DayofMonth
  1-31
  4
  DayOfWeek
  1 (Monday) - 7 (Sunday)
  5
  DepTime
  actual departure time (local, hhmm)
  6
  CRSDepTime
  scheduled departure time (local, hhmm)
  7
  ArrTime
  actual arrival time (local, hhmm)
  8
  CRSArrTime
  scheduled arrival time (local, hhmm)
  9
  UniqueCarrier
  unique carrier code
  10
  FlightNum
  flight number
  11
  12
  TailNum
  ActualElapsedTime
  plane tail number
  in minutes
  13
  CRSElapsedTime
  in minutes
  14
  AirTime
  in minutes
  15
  ArrDelay
  arrival delay, in minutes
  16
  DepDelay
  departure delay, in minutes
  1987-2008
  5
  5
  Analyzing Flow
- Load data
- Preprocess Data
- Train data & obtain a model
- Evaluate resulting model
- Make Predictions
  6
  6
  3
  30/01/2023
  Lab: Running Prediction of Flight Delay
  on Zeppelin
- Write code using PySpark
- Get prepared notebook
- Run & try to understand algorithms
- original source code (in Scala)
- https://github.com/pedroduartecosta/SparkPredictFlightDelay
  7
  7
  References
- [1] https://medium.com/@pedrodc/building-abig-data-machine-learning-spark-applicationfor-flight-delay-prediction-4f9507cdb010
- [2]
  https://www.tutorialspoint.com/data_mining/d
  m_classification_prediction.htm
  8
  8
  4
  06/02/2023
  L8: Spark GraphX
  1
  IT5427
  Tích hợp và xử lý data lớn
  IT5427
  01/2023
  Thanh-Chung Dao Ph.D.
  1
  GraphX
  ¨ Apache Spark's API for graphs & graph-parallel
  computation
  ¨ GraphX unifies ETL (Extract, Transform & Load)
  process
  ¨ Exploratory analysis & iterative graph
  computation within a single sys
  2
  1
  06/02/2023
  Use cases
  ¨ Facebook's friends, LinkedIn's connections
  ¨ Internet's routers
  ¨ Relationships between galaxies & stars in
  astrophysics & Google's Maps
  ¨ Disaster detection, banking, stock market
  3
  RDD on GraphX
  ¨ GraphX extends Spark RDD with a Resilient
  Distributed Property Graph
  ¨ property graph is a directed multigraph which
  can have multiple edges in parallel
  ¨ parallel edges allow multiple relationships
  between same vertices
  4
  2
  06/02/2023
  Spark GraphX Features
  ¨ Flexibility
  ¤ Spark GraphX works with both graphs & computations
  ¤ GraphX unifies ETL (Extract, Transform & Load),
  exploratory analysis & iterative graph computation
  ¨ Speed
  ¤ fastest specialized graph processing syss
  ¨ Growing Algorithm Library
  ¤ Page rank, connected components, label propagation,
  SVD++, strongly connected components & triangle
  count
  5
  GraphX with Examples
  ¨ graph here represents Twitter users & whom they follow on Twitter. For e.g. Bob follows
  Davide & Alice on Twitter
  ¨ Looking at graph, we can extract information
  about people (vertices) & relations
  between them (edges)
  6
  3
  06/02/2023
  Source code
  7
  More source code
  8
  4
  06/02/2023
  Other example in PySpark
  9
  Spark Knowledge Graph
  ¨ Example: https://github.com/spoddutur/graph-
  knowledge-browser
  10
  5
  06/02/2023
  Books:
  Acknowledgement
  & References
  Slides:
- https://www.edureka.co/blog/sparkgraphx/
  11
  6
