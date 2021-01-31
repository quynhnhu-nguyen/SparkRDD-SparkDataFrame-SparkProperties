# SparkRDD-SparkDataFrame-SparkProperties
## Phần 1: LÝ THUYẾT
### I: Spark properties
#### 1. Giới thiệu

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Spark Properties kiểm soát hầu hết các cài đặt ứng dụng và được cấu hình riêng cho từng ứng dụng. Các thuộc tính này có thể được cài đặt trực tiếp trên SparkConf được chuyển đến SparkContext. SparkConf cho phép định cấu hình một số thuộc tính, cũng như các cặp key-value thông qua phương thức set().</p>

<p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Các thuộc tính chỉ định số một số khoảng thời gian với một đơn vị thời gian. Các định dạng sau được Spark chấp nhận:</p>

```python
      val conf = new SparkConf()
                 .setMaster("local[2]")
                 .setAppName("CountingSheep")
      val sc = new SparkContext(conf)
 ```
 <p align="justify"> &nbsp;&nbsp;&nbsp;&nbsp; Các định dạng thuộc tính khích thước byte có trong Spark</p>
 
 ```note
      1b (bytes)                                    1g or 1gb (gibibytes = 1024 mebibytes)
      1k or 1kb (kibibytes = 1024 bytes)            1t or 1tb (tebibytes = 1024 gibibytes)
      1m or 1mb (mebibytes = 1024 kibibytes)        1p or 1pb (pebibytes = 1024 tebibytes)
```
