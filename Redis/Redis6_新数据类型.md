# 新数据类型

## Bitmaps

- 底层也是String，但对比String在某些情况下比较节省空间
- 可以将 Bitmap 看成是一个 bit 为单位的数组，数组的每个单元只能存储 0 或者 1，数组的下标在 Bitmap 中叫做 offset 偏移量。

### 基本命令

- `setbit <key> <index> <0|1>` : 设置key中索引为index的位为1
- `getbit <key> <index>` : 得到key中索引位的值
- `bitcount <key> <start> <stop>` : 得到key中从start到stop位置（单位为Byte）中位为1的个数
- `bitpos <key> <bit> <start> <stop>` : 得到在start到stop中第一位的bit出现的位置
- `bitop [AND | OR | NOT | XOR] <distkey> [key...]` : 对多个key按位进行逻辑运算