Bản chất DStream:

- **_là một chuỗi liên tục RDD_**
- Là một chuỗi liên tục DataFrame
- Là một chuỗi liên tục DataSet
- ko có đáp án đúng

Trong hệ sinh thái của Spark không có công cụ hay thành phần nào sau đây:

- MLib
- GraphX
- **_Sqoop_**
- Cluster Managers

Hadoop giải quyết bài toán khả mở bằng cách nào? Chọn đáp án sai

- Thiết kế phân tán ngay từ đầu, mặc định triển khai trên cụm máy chủ
- **_Các node tham gia vào cụm Hadoop được gán vai trò hoặc là node tính toán hoặc là node lưu trữ dữ liệu_**
- Các node tham gia vào cụm đóng cả 2 vai trò tính toán và lưu trữ
- Các node thêm vào cụm có thể có cấu hình, độ tin cậy cao

Hadoop giải quyết bài toán chịu lỗi thông qua kỹ thuật gì? Chọn đáp án SAI

- Kỹ thuật dư thừa
- Các tệp tin được phân mảnh, các mảnh được nhân bản ra các node khác trên cụm
- **_Các tệp tin được phân mảnh, các mảnh được lưu trữ tin cậy trên ổ cứng theo cơ chế RAID_**
- các công việc cần tính toán được phân mảnh thành các tác vụ độc lập

Các đặc trưng của HDFS. Chọn đáp án SAI

- Tối ưu cho các tệp tin có kích thước lớn
- **_Hỗ trợ thao tác đọc ghi tương tranh tại chunk (phân mảnh) trên tệp tin_**
- Hỗ trợ nén dữ liệu để tiết kiệm chi phí
- hỗ trợ cơ chế phân quyền và kiểm soát người dùng của UNIX

Spark Streaming trừu tượng hóa cũng như thao tác với các dòng dữ liệu (data stream) dựa trên khái niệm nào:

- shared variable
- RDD
- **_DStream_**
- DataFrame

Spark hỗ trợ các cluster manager nào

- Standalone Cluster manager
- MESOS
- YARN
- **_tất cả đáp án trên_**

Đáp án nào không phải là một “output operation” khi thao tác với DStream

- saveAsTextFile
- foreachRDD
- saveasHadoopFile
- **_reduceByKeyAndWindow_**

Đáp án nào không phải là một “Transformation” khi thao tác với DStream

- reduceByWindow
- window
- foreachWindow
- **_countByWindow_**

Mục đích của sử dụng sparkML là gì

- chạy MapReduce
- chạy các thuật toán dự đoán
- tính toán phân tán
- **_cả b và c_**

Mục đích của lệnh sau đây là gì:

```
(trainingData, testData) = dataset.randomSplit([0.8, 0.2], seed=100)
```

- **_chia dữ liệu học và dữ liệu kiểm tra_**
- chạy chương trình học
- tạo dữ liệu ngẫu nhiên cho dữ liệu học và dữ liệu kiểm tra
- chạy chương trình dự đoán

Label và feature của câu lệnh bên dưới có nghĩa là gì

```
LogisticRegression(labelCol = “label” , featuresCol = “features”, maxIter = 10)
```

- dữ liệu đầu vào được gán là feature và dự đoán được gán vào label
- dữ liệu đầu vào được gán là label và kết quả của dữ liệu đầu vào đó được gán vào feature
- **_dữ liệu đầu vào được gán là feature và kết quả của dữ liệu đầu vào được gán vào label_**
- dữ liệu đầu vào được gán là lebl và kết quả dự đoán được gán vào feature

Đâu là lệnh lưu trữ dữ liệu ra ngoài chương trình Spark:

- **_input.saveAsTextFile(‘file:///usr/momoinu/mon_loz/hihi.txt’)_**
- input.saveAsTextFile(‘/usr/momoinu/mon_loz/hihi.txt’)
- input.saveAs (‘file:///usr/momoinu/mon_loz/hihi.txt’)
- input.saveAsTextFile: ‘file:///usr/momoinu/mon_loz/hihi.txt’

Đâu là cách submit đúng 1 job lên Spark cluster hoặc chế độ local

- **_./spark-submit wordcount.py README.md_**
- ./spark-submit README.md wordcount.py
- spark-submit README.md wordcount.py
- phương án a và c

Câu lệnh MapReduce trong Spark dưới đây, chia mỗi dòng thành từ dựa vào delimiter nào

```
input.flatMap( lambda x: x.split(“\t”) ).map(lambda x: (x, 1)).reduceByKey(add)
```

- **_Tab_**
- Dấu cách
- Dấu hai chấm
- Dấu phẩy

Cơ chế chịu lỗi của datanode trong HDFS

- dử dụng ZooKeeper để quản lý các thành viên datanode trong cụm
- **_sử dụng cơ chế heartbeat, định kỳ các datanode thông báo về trạng thái cho Namenode_**
- sử dụng cơ chế heartbeat, Namenode định kỳ hỏi các datanode về trạng thái tồn tại của datanode

Cơ chế tổ chức dữ liệu của Datanode trong HDFS

- **_các chunk là các tệp tin trong hệ thống tệp tin cục bộ của máy chủ datanode_**
- các chunk là các vùng dữ liệu liên tục trên ổ cứng của máy chủ data node
- các chunk được lưu trữ tin cậy trên datanode theo cơ chế RAID

Cơ chế nhân bản dữ liệu trong HDFS

- **_Namenode quyết định vị trí các nhân bản của các chunk trên các datanode_**
- Datanode là primary quyết định vị trí các nhân bản của cac chunk tại các secondary datanode
- Client quyết định vị trí lưu trữ các nhân bản với từng chunk

HDFS giải quyết bài toán single-point-of-failure cho Namenode bằng cách nào

- sử dụng thêm secondary namenode theo cơ chế active-active. Cả Namenode và Secondary Namenode cùng online trong hệ thống
- **_Sử dụng Secondary namenode theo cơ chế active-passive. Secondary namenode chỉ hoạt động khi có vấn đề với namenode_**

Các mục tiêu chính của Apache Hadoop

- lưu trữ dữ liệu khả mở
- xử lý dữ liệu lớn mạnh mẽ
- trực quan hóa dữ liệu hiệu quả
- **_lưu trữ dữ liệu khả mở và xử lý dữ liệu lớn mạnh mẽ_**
- lưu trữ dữ liệu khả mở, xử lý dữ liệu lớn mạnh mẽ và trực quan hóa dữ liệu hiệu quả

Phát biểu nào không đúng vè Apache Hadoop

- xử lý dữ liệu phân tán với mô hình lập trình đơn giản, than thiện hơn như MapReduce
- Hadoop thiết kế để mở rộng thông qua kỹ thuật scale-outm tăng số lượng máy chủ
- thiết kế để vận hàn trên phần cứng phổ thống, có khả năng chống chịu lỗi phần cứng
- **_thiết kế đẻ vận hành trên siêu máy tính, cấu hình mạnh, độ tin cậy cao_**

Thành phần nào không thuộc thành phần lõi của Hadoop

- Hệ thống tập tin phân tán HDFS
- MapReduce Framework
- YARN: yet another resource negotiator
- Apache ZooKeeper
- **_Apache Hbase_**

Spark có thể chạy ở chế độ nào khi chạy trên nhiều máy

- chạy trên YARN
- chạy trên ZooKeeper
- phương án a và b đều sai
- **_phương án a và b đều đúng_**

phát biểu nào sau đây sai về kafka

- partition được nhân bản ra nhiều brokers
- **_message sau khi được tiêu thụ (consume) thì không bị xóa._**
- các topic gồm nhiều partitions
- Kafka đảm bảo thứ tự của các message với mỗi topics

Mô tả cach thức một client đọc dữ liệu trên HDFS

- client thông báo tới namenode để bắt đầu quá trình đọc sau đó client chạy truy vấn các datanode để trực tiếp đọc các chunks
- client truy vấn Namenodê để biết được vị trí các chunks. Nếu namenode không biết thì namenode sẽ hỏi các datanode., Sau đó namenode gửi lại thông tin vị trí các chunk cho client. client kết nối song song tới các Datanode để đọc các chunk
- client truy vấn namenode để đưa thông tin về thao tác đọc, Namenode kết nối song song tới các datanode để lấy dữ liệu, sau đó trả về cho client
- **_client truy vấn namenode để biết được vị trí các chunks. Namenode trả về vị trí các chunks. Client kết nối song song tới các datanode để đọc các chunks_**

Đâu không phải là tính năng mà NoSQL nào cũng đáp ứng

- tính sẵn sàng cao
- khả năng mở rộng linh hoạt
- **_phù hợp với dữ liệu lớn_**

Hệ thống nào cho phép đọc ghi dữ liệu tại vị trí ngẫu nhiên, thời gian thực tới hàng terabyte dữ liệu

- **_Hbase_**
- Flume
- Pig
- HDFS

Phát biểu nào đúng về Quorum trong Amazon DynamoDB

- với N là tổng số nhân bản, R là số nhân bản cần đọc trong 1 thao tác đọc. W là số nhân bản cần ghi trong 1 thao tác ghi. N > R + W
- **_với N là tổng số nhân bản, R là số nhân bản cần đọc trong 1 thao tác đọc. W là số nhân bản cần ghi trong 1 thao tác ghi. N < R + W_**
- với N là tổng số nhân bản, R là số nhân bản cần đọc trong 1 thao tác đọc. W là số nhân bản cần ghi trong 1 thao tác ghi. N = R + W

Phát biểu nào sau đây sai về Kafka

- nhiều consumer có thể cùng đọc 1 topic
- 1 message có thể được đọc bới nhiểu consumer khác nhau
- **_số lượng consumer phải ít hơn hoặc bằng số lượng partitions_**
- 1 message chỉ có thể được đọc bởi 1 consumer trong 1 consumer group

Đâu là một dạng của NoSQL

- MySQL
- JSON
- **_Key-value store_**
- OLAP.

phát biểu nào sai về Presto

- presto là một engine truy vấn SQL hiệu năng cao, phân tán cho dữ liệu lớn.
- presto thích hợp với các công cụ Business Intelligence
- presto được quản lý bởi presto software foundation
- **_presto được quản lý bởi apache software foundation_**

Phát biểu nào đúng về Presto

- **_các stage được thực thi theo cơ chế pipeline, không có thời gian chờ giữa các stage như Map Reduce_**
- Presto cho phép xử lý kết tập dữ liệu mà kích thước lớn hơn kích thước bộ nhớ trong
- Presto có cơ chế chịu lỗi khi thực thi truy vấn

Chọn phát biểu đúng khi nói về MongoDB

- MongoDB có các trình điều khiển driver cho nhiều ngôn ngữ lập trình khác nhau.
- các văn bản có thể chứa nhiều cặp key-value hoặc key-array, hoặc các văn bản lồng (nested documents)
- **_tất cả các phương án trên_**
- MongoDB hay các NoSQL có khả năng khả mở tốt hơn các CSDL quan hệ truyền thống

Giữa Pig và Hive, công cụ nào có giao diện truy vấn gắn với ANSI SQL hơn

- Pig
- không phải 2 đáp án trên
- **_Hive_**

Công ty nào đã phát triển Apache Cassandra giai đoạn đầu tiên

- Google
- twitter
- linkedin
- **_facebook_**

Phát biểu về định lý CAP

- The limitations of distributed databases can be described in the so called the CAP theorem

  - Consistency: every node always sees the same data at any given instance (i.e., strict consistency)
  - Availability: the system continues to operate, even if nodes in a cluster crash, or some hardware or software parts are down due to upgrades
  - Partition Tolerance: the system continues to operate in the presence of network partitions

Cho đoạn chương trình sau:

```
rdd = sc.parallelize([“hello”, “world”, “good”, “hello”], 2)
rdd = rdd.map(lambda w : (w, 1))
```

Đưa ra kết quả của rdd.glom().collect()

```
from pyspark.context import SparkContext
from pyspark.sql.session import SparkSession
from pyspark.sql import SQLContext
sc = SparkContext('local')
spark = SparkSession(sc)
sqlContext = SQLContext(sc)

rdd = sc.parallelize(["hello", "world", "good", "hello"], 2)
rdd = rdd.map(lambda w : (w, 1))
result = rdd.glom().collect()

print(result)
```

```
[[('hello', 1), ('world', 1)], [('good', 1), ('hello', 1)]]
```

Giả sử một textfile kích thước lớn được đặt ở đường dẫn hdfs://user/bigfile.txt.
Viết chương trình Spark đếm số lần xuất hiện của chuỗi “big data processing” trong file text này.

```
linesRDD = sc.textFile("hdfs://user/bigfile.txt")

df = spark.read.format("json").load("../data/flight-data/json/2015-summary.json")

df = (spark.read.format("com.databricks.spark.csv")
    .option("header", "true")
    #.option("inferSchema","true")
    .schema(schema)
    .load("hdfs://192.168.56.10:9000/user/output/data10/\*.csv"))

df.printSchema()
df.createOrReplaceTempView("dfTable")
print("Số bản ghi:")
print(df.count())
df.show(20, False)
```
