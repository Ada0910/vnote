# 1. 基本参数
```
-Xmx：最大堆大小
-Xms：初始堆大小
-Xmn:年轻代大小
-XXSurvivorRatio：年轻代中Eden区与Survivor区的大小比值
```
例如：
-Xmx10240m -Xms10240m -Xmn5120m -XXSurvivorRatio=3
,其最小内存值和Survivor区总大小分别是（）

解：年轻代5120m， Eden：Survivor=3，Survivor区大小=1024m（Survivor区有两个，即将年轻代分为5份，每个Survivor区占一份），总大小为2048m。即 ，两块Survivor；    计算Survivor大小， Eden:Survivor = 3，总大小为5120,3x+x+x=5120  x=1024
-Xms初始堆大小即最小内存值为10240m