生成日志

   -Xloggc:./tmp/gc.log -XX:+PrintGCDetails -XX:+PrintGCDateStamps

 

-XX:+PrintGC 输出GC日志
-XX:+PrintGCDetails 输出GC的详细日志
-XX:+PrintGCTimeStamps 输出GC的时间戳（以基准时间的形式）
-XX:+PrintGCDateStamps 输出GC的时间戳（以日期的形式，如 2013-05-04T21:53:59.234+0800）
-XX:+PrintHeapAtGC 在进行GC的前后打印出堆的信息
-Xloggc:../logs/gc.log 日志文件的输出路径

**实时查看**
`jstat -gc pid 2000 3`

S0: 新生代中Survivor space 0区已使用空间的百分比

S1: 新生代中Survivor space 1区已使用空间的百分比
E: 新生代已使用空间的百分比
O: 老年代已使用空间的百分比
M: 元空间已使用空间的百分比

CCS: Compressed class space utilization as a percentage
YGC: 从应用程序启动到当前，发生Yang GC 的次数

YGCT: 从应用程序启动到当前，Yang GC所用的时间【单位秒】
FGC: 从应用程序启动到当前，发生Full GC的次数
FGCT: 从应用程序启动到当前，Full GC所用的时间
GCT: 从应用程序启动到当前，用于垃圾回收的总时间【单位秒】