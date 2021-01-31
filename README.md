# SparkRDD-SparkDataFrame-SparkProperties
## Phần 1: LÝ THUYẾT
### I: Spark properties
#### 1. Giới thiệu

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Spark Properties kiểm soát hầu hết các cài đặt ứng dụng và được cấu hình riêng cho từng ứng dụng. Các thuộc tính này có thể được cài đặt trực tiếp trên SparkConf được chuyển đến SparkContext. SparkConf cho phép định cấu hình một số thuộc tính, cũng như các cặp key-value thông qua phương thức set().</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Các thuộc tính chỉ định số một số khoảng thời gian với một đơn vị thời gian. Các định dạng sau được Spark chấp nhận:</p>

```note
      25ms (milliseconds)          3h (hours)   
      5s (seconds)                 5d (days)
      10m or 10min (minutes)       1y (years)
 ```
 <p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Các định dạng thuộc tính khích thước byte có trong Spark</p>

```note
      1b (bytes)                                    1g or 1gb (gibibytes = 1024 mebibytes)
      1k or 1kb (kibibytes = 1024 bytes)            1t or 1tb (tebibytes = 1024 gibibytes)
      1m or 1mb (mebibytes = 1024 kibibytes)        1p or 1pb (pebibytes = 1024 tebibytes)
 ```
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; <em><b>Ví dụ</b></em>: Khởi tạo ứng dụng chạy trong ngữ cảnh phân tán với 2 luồng giá trị:</p>

```python
      val conf = new SparkConf()
                 .setMaster("local[2]")
                 .setAppName("CountingSheep")
      val sc = new SparkContext(conf)
 ```

#### 2. Tải động với Spark Properties

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Trong một sô trường hợp, ta có thể tránh việc thiết lập cứng cho các cấu hình mặc định trong một SparkConf. Cụ thể là tạo 1 conf trống trong Spark để chạy ứng dụng với các bản gốc khác nhau hoặc số lượng bộ nhớ khác nhau</p>

```python
      val sc = new SparkContext(new SparkConf())
 ```
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Sau đó, cung cấp cấu hình thời gian chạy:</p>

```python
      ./bin/spark-submit --name "My app" --master local[4] --conf spark.eventLog.enabled=false --
      conf "spark.executor.extraJavaOptions=-XX:+PrintGCDetails -XX:+PrintGCTimeStamps" myApp.jar
```
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; <em>Spark-submit</em>: tải cấu hình tự động, chấp nhận bất kỳ thuộc tính nào nếu dùng cờ <em>--conf/-c</em>, sử dụng các cờ đặc biệt (dùng <em>./bin/spark-submit – help</em> để hiện thị tất cả các tùy chọn) cho các lệnh khởi động <em>spark — master</em>.</p>

#### 3. Các thuộc tính của Spark

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Thuộc tính Spark chia làm 2 loại:</p>
<ul align="justify">
  <li>Liên quan đến triển khai: <b><em>spark.driver.memory, spark.executor.instances</em></b>.</li></br>
  <li>Liên quan đến kiểm soát thời gian chạy Spark: <b><em>spark.task.maxFailures</em></b>.</li>
</ul>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Một số thuộc tính ứng dụng</p>
<ul align="justify">
  <li>spark.app.name: Tên ứng dụng được hiển thị trong giao diện người dùng và trong dữ liệu nhật ký.</li></br>
  <li>spark.driver.cores: Số lõi để sử dụng cho quy trình trình điều khiển, chỉ ở chế độ cụm.</li></br>
  <li>spark.logConf: Ghi lại SparkConf hiệu quả dưới dạng thông tin khi một SparkContext được khởi động.</li></br>
  <li>spark.driver.memoryOverhead: Số lượng bộ nhớ không phải bộ nhớ heap sẽ được phân bổ cho mỗi quá trình điều khiển ở chế độ cụm.</li></br>
  <li>...</li>
</ul>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Một số thuộc tính xáo trộn</p>
<ul align="justify">
  <li>spark.shuffle.compress: Có nén các map output file hay không.</li></br>
  <li>spark.shuffle.io.retryWait: (Chỉ mạng) Thời gian chờ giữa các lần tìm nạp lại. Theo mặc định, Độ trễ tối đa do thử lại là 15 giây.</li></br>
  <li>spark.shuffle.service.port: Cổng mà dịch vụ shuffle ngoài sẽ chạy, mặc định port 7337.</li></br>
  <li>...</li>
</ul>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Giao diện người dùng</p>
<ul align="justify">
  <li>spark.eventLog.enabled: Có ghi lại các sự kiện Spark hay không, hữu ích trong việc tạo lại giao diện người dùng Web sau khi ứng dụng hoàn tất.</li></br>
  <li>spark.eventLog.logBlockUpdates.enabled: Có ghi lại các sự kiện cho mỗi lần cập nhật khối hay không, nếu spark.eventLog.enabled là true. Cảnh báo: Điều này sẽ làm tăng đáng kể kích thước của nhật ký sự kiện.</li></br>
  <li>spark.eventLog.compress: Có nén các sự kiện đã ghi nếu            spark.eventLog.enabled = true.</li></br>
  <li>spark.eventLog.overwrite: Có ghi đè lên bất kỳ tệp hiện có nào không.</li></br>
  <li>spark.ui.enabled: Có chạy giao diện người dùng web (User interface) cho ứng dụng Spark hay không.</li></br>
  <li>...</li>
</ul>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Nén và tuần tự hóa: spark.rdd.compress - Có nén các phân vùng tuần tự</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Một số thuộc tính khác</p>
<ul align="justify">
  <li>Môi trường thực thi.</li></br>
  <li>Quản lý bộ nhớ.</li></br>
  <li>Hành vi thực thi.</li></br>
  <li>Chỉ số thực thi.</li></br>
  <li>Kết nối mạng.</li></br>
  <li>Lập lịch.</li>
</ul>

### II: Spark RDD
#### 1. Giới thiệu
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; RDD (Resilient Distributed Datasets) là một cấu trúc dữ liệu cơ bản của Spark, là một tập hợp bất biến phân tán của một đối tượng.</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Mỗi dataset trong RDD được chia thành nhiều phần vùng logical, có thể được tính toán trên các nút khác nhau.</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; RDD có thể chứa bất kì kiểu dữ liệu của Python, Java hoặc Scala bao gồm các lớp do người dùng định nghĩa.</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Về hình thức, RDD là một tập hợp các bản ghi được phân vùng và chỉ cho phép đọc. RDD có thể được tạo thông qua các hoạt động xác định trên dữ liệu trên bộ lưu trữ ổn định hoặc các RDD khác. RDD là một tập hợp các phần tử chịu được lỗi có thể hoạt động song song.</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Có 2 cách để tạo RDDs:</p>
<ul align="justify">
  <li>Tạo từ một tập hợp dữ liệu có sẵn trong ngôn ngữ sử dụng như Java, Python, Scala.</li></br>
  <li>Lấy từ dataset hệ thống lưu trữ bên ngoài như HDFS, Hbase hoặc các cơ sở dữ liệu quan hệ.</li>
</ul>

#### 2. Thực thi trên Spark RDD
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Dữ liệu trong MapReduce chia sẻ chậm do sao chép, tuần tự hóa và tốc độ I/O của ổ đĩa. Hầu hết các ứng dụng Hadoop, cần dành hơn 90% thời gian để thực hiện các thao tác đọc-ghi HDFS.</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Để khắc phục được vấn đề trên, các nhà nghiên cứu đã phát triển một framework chuyên biệt gọi là Apache Spark. </p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Ý tưởng chính của Spark là Resilient Distributed Datasets (RDD), nó hỗ trợ tính toán xử lý trong bộ nhớ. Điều này có nghĩa, nó lưu trữ trạng thái của bộ nhớ dưới dạng một đối tượng trên các công việc và đối tượng có thể chia sẻ giữa các công việc đó. Việc xử lý dữ liệu trong bộ nhớ nhanh hơn 10 đến 100 lần so với network và disk.</p>

#### 3. Hoạt động tương tác 
<p align="center"><img src ="https://user-images.githubusercontent.com/77887833/106388105-87c42d80-640f-11eb-96ff-199a7b3293e3.jpg" width="90%"/></p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Hình minh họa này cho thấy các hoạt động tương tác trên Spark RDD. Nếu các truy vấn khác nhau được chạy lặp lại trên cùng một tập dữ liệu, thì dữ liệu cụ thể này có thể được lưu trong bộ nhớ để có thời gian thực thi tốt hơn.</p>

#### 4. Hoạt động lặp
<p align="center"><img src ="https://user-images.githubusercontent.com/77887833/106388107-88f55a80-640f-11eb-97cf-e6032dbba201.jpg" width="90%"/></p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Hình minh họa dưới đây cho thấy các hoạt động lặp lại trên Spark RDD. Nó sẽ lưu trữ các kết quả trung gian trong một bộ nhớ phân tán thay vì Ổ lưu trữ ổn định (Disk) và làm cho hệ thống nhanh hơn.</p>

#### 5. Các loại RDD
<p align="center"><img src ="https://user-images.githubusercontent.com/77887833/106388108-898df100-640f-11eb-8feb-8e4fdbf2d43c.jpg" width="90%"/></p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Các RDD biểu diễn một tập hợp cố định, dã được phân vùng các record để có thể xử lý song song.</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Các record trong RĐ có thể là đối tượng Java, Scala hay Python.</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; RDD đã từng là API chính được sử dụng trong series Spark 1x và vẫn có thể sử dụng trong version 2x.</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; RDD API có thể được sử dụng trong Java, Scala hay Python:</p>
<ul align="justify">
  <li>Scala và Java: performance tương đương trên hầu hết mọi phần.</li></br>
  <li>Python: mất một lượng performance, chủ yếu là cho việc serialization giữa tiến trình Python và JVM.</li>
</ul>

#### 6. Các transformation và action với RDD
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; RDD cung cấp các transformation và action hoạt động giống như DataFrame lẫn DataSets. Transformation xử lý các thao tác lazily và Action xử lý thao tác cần xử lý tức thời.</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Một số transformation:</p>
<ul align="justify">
  <li>Distinct: loại bỏ trùng lắp trong RDD</li></br>
  <li>Filter: tương đương với việc sủ dụng where trong SQL – tìm các record trong RDD xem những phần tử nào thỏa điều kiện. Có thể cung cấp một hàm phức tạp sử dụng để filter các record cần thiết – như trong Python, có thể sử dụng hàm lambda để truyền vào filter.</li></br>
  <li>Map: thực hiện  một công việc nào đó trên toàn bộ RDD. Trong Python sử dụng lambda với từng phần tử để truyền vào map.</li></br>
  <li>flatMap: cung cấp một hàm đơn giản hơn hàm map.</li></br>
  <li>sortBy: mô tả một hàm để trích xuất dữ liệu từ các object của RDD và thực hiện sort được từ đó.</li></br>
  <li>randomSplit: nhận một mảng trọng số và tạo một random seed, tách các RDD thành một mảng các RDD có số lượng chia theo trọng số.</li>
</ul>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Một số action:</p>
<ul align="justify">
  <li>Reduce: thực hiện hàm reduce trên RDD để thu về 1 giá trị duy nhất.</li></br>
  <li>Count: đếm số dòng trong RDD.</li></br>
  <li>countApprox: phiên bản đếm xấp xỉ của count nhưng phải cung cấp timeout vì có thể không nhận được kết quả.</li></br>
  <li>countByValue: đếm số giá trị của RDD.</li></br>
  <li>countApproxDistinct: đếm xấp xỉ các giá trị khác nhau.</li></br>
  <li>countByValueApprox: đếm xấp xỉ các giá trị.</li></br>
  <li>First: lấy giá trị đầu tiên của dataset.</li></br>
  <li>Max và Min: lần lượt lấy giá trị lớn nhất và nhỏ nhất của dataset.</li></br>
  <li>Take và các method tương tự: lấy một lượng giá trị từ trong RDD. Take trước hết scan qua một partition và sử dụng kết quả để dự đoán số lượng partition cần phải lấy thêm để thỏa mãn số lượng lấy.</li></br>
  <li>Top và takeOrdered: top sẽ hiệu quả hơn takeOrdered vì top lấy các giá trị đầu tiên được sắp xếp ngầm trong RDD.</li></br>
  <li>takeSamples: lấy một lượng giá trị ngẫu nhiên trong RDD.</li>
</ul>

#### 7. Một số kỹ thuật đối với RDD

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Lưu trữ file</p>
<ul align="justify">
  <li>Thực hiện ghi vào các file plain-text.</li></br>
  <li>Có thể sử dụng các code nén từ thư viện Hadoop.</li></br>
  <li>Lưu trữ vào các database bên ngoài yêu cầu ta phải lặp qua tất cả partition của RDD – công việc được thực hiện ngầm trong các high-level API.</li></br>
  <li>sequenceFile là một flat file chưa các cặp key-value thường được sử dụng làm định dạng inout/output của MapReduce. Spark có thể ghi ác sequencefile bằng cách ghi lại các cặp key-value.</li></br>
  <li>Spark cũng hỗ trợ ghi nhiều dạng file khác nhau cho phép define các class, định dạng output, config và compression scheme của Hadoop.</li>
</ul>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Caching: tăng tốc độ xử lý bằng cache</p>
<ul align="justify">
  <li>Caching với RDD, Dataset hay DataFrame có nguyên lý như nhau.</li></br>
  <li>Chúng ta có thể lựa chọn cache hay persist một RDD và mặc định chỉ xử lý dữ liệu trong bộ nhớ.</li>
</ul>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Checkpointing: lưu trữ lại các bước xử lý để phục hồi</p>
<ul align="justify">
  <li>Checkpointing lưu RDD vào đĩa cứng để các tiến trình khác để thể sử dụng lại RDD point này làm partition trung gian thay vì tính toán lại RDD từ các nguồn dữ liệu gốc.</li></br>
  <li>Checkpointing cũng tương tự như cache, chỉ khác nhau là lưu trữ vào đĩa cứng và không dùng được trong API của DataFrame.</li></br>
  <li>Cần sử dụng nhiều để tối ưu hóa.</li>
</ul>

### III: Spark DataFrame
#### 1. Giới thiệu

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Spark DataFrame là một tập hợp dữ liệu phân tán được tổ chức thành các cột được đặt tên và cũng được sử dụng để cung cấp các hoạt động như lọc, tính toán tổng hợp, phân nhóm và cũng có thể được sử dụng với Spark SQL.</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Khung dữ liệu có thể được tạo bằng cách sử dụng các tệp dữ liệu có cấu trúc, cùng với các RDD hiện có, cơ sở dữ liệu bên ngoài và bảng Hive. </p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Về cơ bản, nó được gọi là một lớp trừu tượng được xây dựng trên RDD và cũng được theo sau bởi API tập dữ liệu đã được giới thiệu trong các phiên bản sau của Spark (2.0 +). </p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Hơn nữa, các bộ dữ liệu không được giới thiệu trong Pyspark mà chỉ ở Scala với Spark nhưng đây không phải là trường hợp của Dataframe. </p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Khung dữ liệu phổ biến được gọi là DF là định dạng cột hợp lý giúp làm việc với RDD dễ dàng và thuận tiện hơn, cũng sử dụng các chức năng tương tự như RDD theo cách tương tự. Nếu nói nhiều hơn ở mức độ khái niệm thì nó tương đương với các bảng quan hệ cùng với các tính năng và kỹ thuật tối ưu hóa tốt.</p>

#### 2. Lợi ích mà DataFrame mang lại

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Xử lý dữ liệu có cấu trúc và bán cấu trúc </p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Slicing và Dicing</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Hỗ trợ nhiều ngôn ngữ</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Nguồn dữ liệu</p>

#### 3. Các tính năng của DataFrame và nguồn dữ liệu Pyspark

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Các tính năng</p>
<ul align="justify">
  <li>DataFrame được phân phối trong tự nhiên, làm cho nó trở thành một cấu trúc dữ liệu có khả năng chịu lỗi và có tính khả dụng cao.</li></br>
  <li>Đánh giá lười biếng là một chiến lược đánh giá giữa việc đánh giá một biểu thức cho đến khi giá trị của nó là cần thiết. Nó tránh đánh giá lặp lại. Đánh giá lười biếng trong Spark có nghĩa là quá trình thực thi sẽ không bắt đầu cho đến khi một hành động được kích hoạt. Trong Spark, bức tranh về sự lười biếng xuất hiện khi các phép biến đổi Spark xảy ra.</li></br>
  <li>DataFrame là bất biến trong tự nhiên. Bởi bất biến, ý tôi là nó là một đối tượng có trạng thái không thể sửa đổi sau khi nó được tạo. Nhưng chúng ta có thể biến đổi các giá trị của nó bằng cách áp dụng một phép biến đổi nhất định, như trong RDD.</li>
</ul>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Nguồn dữ liệu Pyspark</p>
<ul align="justify">
  <li>Đọc dữ liệu.</li></br>
  <li>Hiển thị dữ liệu.</li></br>
  <li>Sử dụng phương pháp printSchema.</li></br>
  <li>Sử dụng phương pháp select</li></br>
  <li>Sử dụng bộ lọc tuổi.</li></br>
  <li>Sử dụng phương pháp groupBy.</li></br>
  <li>Sử dụng hàm SQL trên SparkSession.</li></br>
  <li>Sử dụng hàm SQL trên một phiên Spark cho chế độ xem tạm thời toàn cầu.</li>
</ul>

## Phần 2: CODE MINH HỌA

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Tạo một Pyspark RDD:</p>
<p align="center"><img src ="https://user-images.githubusercontent.com/77887833/106389800-f0afa380-6417-11eb-8317-e9b271378199.jpg" width="90%"/></p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Chạy một vài thao tác cơ bản bằng Pyspark</p>
<p align="center"><img src ="https://user-images.githubusercontent.com/77887833/106389803-f1e0d080-6417-11eb-85d1-5eebdf58087e.jpg" width="90%"/></p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Hàm <b>Count()</b></p>
<p align="center"><img src ="https://user-images.githubusercontent.com/77887833/106389804-f2796700-6417-11eb-8321-14edf5029a92.jpg" width="90%"/></p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Hàm <b>Collect()</b></p>
<p align="center"><img src ="https://user-images.githubusercontent.com/77887833/106389807-f311fd80-6417-11eb-9c09-74e614329903.jpg" width="90%"/></p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Hàm <b>foreach(f)</b></p>
<p align="center"><img src ="https://user-images.githubusercontent.com/77887833/106389808-f311fd80-6417-11eb-8536-12df949c10be.jpg" width="90%"/></p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Hàm <b>filler(f)</b></p>
<p align="center"><img src ="https://user-images.githubusercontent.com/77887833/106389810-f3aa9400-6417-11eb-83f7-3da817d02619.jpg" width="90%"/></p>

## Phần 3: TÀI LIỆU THAM KHẢO
 
&nbsp;&nbsp;&nbsp;&nbsp; 1. https://laptrinh.vn/books/apache-spark/page/apache-spark-rdd#bkmrk-resilient-distribute
&nbsp;&nbsp;&nbsp;&nbsp; 2. https://spark.apache.org/docs/latest/configuration.html
&nbsp;&nbsp;&nbsp;&nbsp; 3. http://itechseeker.com/tutorials/apache-spark/lap-trinh-spark-voi-scala/spark-sql-dataset-va-dataframes/
&nbsp;&nbsp;&nbsp;&nbsp; 4. https://dzone.com/articles/pyspark-dataframe-tutorial-introduction-to-datafra
&nbsp;&nbsp;&nbsp;&nbsp; 5. https://www.tutorialspoint.com/pyspark/pyspark_rdd.htm
