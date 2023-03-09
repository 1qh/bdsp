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
