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
<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; <em>Spark-submit</em>: tải cấu hình tự động, chấp nhận bất kỳ thuộc tính nào nếu dùng cờ <em>--conf/-c</em>, sử dụng các cờ đặc biệt (dùng <em>./bin/spark-submit –help</em> để hiện thị tất cả các tùy chọn) cho các lệnh khởi động <em>spark—master</em>.</p>

#### 3. Các thuộc tính của Spark

