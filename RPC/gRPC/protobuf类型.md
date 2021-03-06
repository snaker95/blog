# protobuf类型

[TOC]

## protobuf消息定义

消息由至少一个字段组合而成，类似于C语言中的结构。每个字段都有一定的格式。

字段格式：限定修饰符① | 数据类型② | 字段名称③ | = | 字段编码值④ | [字段默认值⑤]

* 限定修饰符包含 required\optional\repeated

  * Required: 表示是一个必须字段，必须相对于发送方，在发送消息之前必须设置该字段的值，对于接收方，必须能够识别该字段的意思。发送之前没有设置required字段或者无法识别required字段都会引发编解码异常，导致消息被丢弃。
  * Optional：表示是一个可选字段，可选对于发送方，在发送消息时，可以有选择性的设置或者不设置该字段的值。对于接收方，如果能够识别可选字段就进行相应的处理，如果无法识别，则忽略该字段，消息中的其它字段正常处理。---因为optional字段的特性，很多接口在升级版本中都把后来添加的字段都统一的设置为optional字段，这样老的版本无需升级程序也可以正常的与新的软件进行通信，只不过新的字段无法识别而已，因为并不是每个节点都需要新的功能，因此可以做到按需升级和平滑过渡。
  * Repeated：表示该字段可以包含0~N个元素。其特性和optional一样，但是每一次可以包含多个值。可以看作是在传递一个数组的值。

* 数据类型

  * | .proto类型 | Java 类型  | C++类型 | 备注                                                         |
    | ---------- | ---------- | ------- | ------------------------------------------------------------ |
    | double     | double     | double  |                                                              |
    | float      | float      | float   |                                                              |
    | int32      | int        | int32   | 使用可变长编码方式。编码负数时不够高效——如果你的字段可能含有负数，那么请使用sint32。 |
    | int64      | long       | int64   | 使用可变长编码方式。编码负数时不够高效——如果你的字段可能含有负数，那么请使用sint64。 |
    | uint32     | int[1]     | uint32  | Uses variable-length encoding.                               |
    | uint64     | long[1]    | uint64  | Uses variable-length encoding.                               |
    | sint32     | int        | int32   | 使用可变长编码方式。有符号的整型值。编码时比通常的int32高效。 |
    | sint64     | long       | int64   | 使用可变长编码方式。有符号的整型值。编码时比通常的int64高效。 |
    | fixed32    | int[1]     | uint32  | 总是4个字节。如果数值总是比总是比228大的话，这个类型会比uint32高效。 |
    | fixed64    | long[1]    | uint64  | 总是8个字节。如果数值总是比总是比256大的话，这个类型会比uint64高效。 |
    | sfixed32   | int        | int32   | 总是4个字节。                                                |
    | sfixed64   | long       | int64   | 总是8个字节。                                                |
    | bool       | boolean    | bool    |                                                              |
    | string     | String     | string  | 一个字符串必须是UTF-8编码或者7-bit ASCII编码的文本。         |
    | bytes      | ByteString | string  | 可能包含任意顺序的字节数据。                                 |

* 字段名称

  * protobuf建议字段的命名采用以下划线分割的驼峰式

* 字段编码值

  * 编码值的取值范围为 1~2^32（4294967296）; 其中 1~15的编码时间和空间效率都是最高的
  * 1900~2000编码值为Google protobuf 系统内部保留值，建议不要在自己的项目中使用

* 默认值。

  * 传递数据时，required数据类型，如果用户没有设置值，则使用默认值传递到对端
  * 接受数据时, optional字段，如果没有接收到optional字段，则设置为默认值



# *Message* 结构

 

1. 键值型结构（Key-Value）
2. 第一部分为Key值，Varint 结构
3. Key值的后三位表示规则类型的Type值，其他部分和为类型的数字编号
4. 后面紧跟value，value的值依据规则类型不同而不同

```protobuf
required int32 a = 1; // 当a值为150时

// Key：0000 1000,类型为0000，数字编号为0001

// Value（Varint类型）：1001 0110  0000 0001

// 值解码： 000 0001 + 001 0110 = 10010110 = 150

```

