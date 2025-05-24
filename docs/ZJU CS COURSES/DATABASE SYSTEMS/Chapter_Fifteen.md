## Review
* 按照列来存放：适合分析型（catch hit，data compress, parallism）
* query->parser and translator->relational algebra expression->optimizer（statistics about data）-> evaluation->result
* 语意检查
* 关系代数表达式
* 利用逻辑转换规则：（先筛选再 join）
* 物理优化：用什么算法实现什么算子
* pipeline
* 如何计算查询 cost：
> [!note]
> ![[image-1.png]]
* extra buffer space
* worst case estimation
## File Scan
* linear search ()

## Hash Join Algorithm

1. Partition the relation s using hashing function h. When partitioning a relation, one block of memory is reserved as the output buffer for each partition. Partition r similarly.

2. For each i:
* (A): Load si into memory and build an in-memory
hash index on it using the join attribute. This hash index uses a different hash function than the earlier one h.

* Read the tuples in ri from the disk one by one.
For each tuple tr locate each matching tuple ts in si using the in-memory hash index. Output the concatenation of their attributes.

**Relation s is called the build input and r is called the probe input.**


* The value n and the hash function h is chosen such that each si should fit in memory.

* Typically n is chosen as $n = \lceil \frac{b_s}{M} \rceil \mul f$ where f is a “fudge factor”, typically around 1.2

* The probe relation partitions ri need not fit in memory


* Recursive partitioning required if number of partitions n is greater than
number of pages M of memory.




