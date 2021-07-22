# schema

#### 优化原则
+ 更小的通常更好
+ 简单就好，整型比字符串代价低，内建时间类型>字符串，整型存储ip
+ 尽量避免null，因为索引计算会变得复杂，但提升较小

#### 整数类型
+ TINYINT(8) [unsigned]
+ SMALLINT(16) [unsigned]
+ MEDIUMINT(24) [unsigned]
+ INT(32) [unsigned]
+ BIGINT(64) [unsigned]

#### 实数类型
+ float(4)
+ double(8)
+ decimal(P,D) P有效位数，D小数点后
+ 只对小数进行精确计算时才使用decimal，或者数据量较大时，可以考虑用BIGINT代替DECIMAL，以避免精确计算的高代价

#### 字符串
