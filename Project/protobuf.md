[[serializable]]
##### 1：protobuf

序列化：把对象、结构数据等转变为二进制内容

解决的问题：

1-方便远程的进程间通信；本地进程间也可以使用

2-方便几十兆M数量级的数据紧凑的存储在文件中.proto

3-对比json等，数据解析快、占空间小

下面默认介绍proto3

| 修饰词      |                                       | type       | to c++     |
| -------- | ------------------------------------- | ---------- | ---------- |
| singular | default                               | double     | double     |
| optional | 类似singular(除了显示确认有没有设置值，如果没有，就不会被序列化) | float      | float      |
| repeated | 使用方法见官网                               | [s]int32   | int32      |
| map      | version3新增                            | [s]int64   | int64      |
|          |                                       | uint32[64] | uint32[64] |
|          |                                       | bool       | bool       |
|          |                                       | string     | string     |
|          |                                       | bytes      | string     |
|          |                                       | enum       |            |

```protobuf
//number 1-15,using one byte; 16-2047 using two bytes
message Test6 {
  map<string, int32> g = 7;
}
//map和下面作用一样
message Test6 {
  message g_Entry {
    optional string key = 1;
    optional int32 value = 2;
  }
  repeated g_Entry g = 7;
}
/////
enum Corpus {
  CORPUS_UNSPECIFIED = 0;//here 0 is value
  CORPUS_UNIVERSAL = 1;
  CORPUS_WEB = 2;
  CORPUS_IMAGES = 3;
}

message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
  Corpus corpus = 4;
}
```
