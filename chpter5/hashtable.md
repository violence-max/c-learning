> hashtable，即散列表。在元素的插入、删除和搜寻等操作上具有常数平均时间的表现，而且这种表现是以统计为基础，而不需要像二叉搜索树一样依赖于输入的随机性
>   
- 负载系数：元素个数除以表格大小  
几种常见的哈希存数方法：  
1. 线性探测：平均插入成本的成长幅度远高于负载系数的成长幅度，这被称为主集团
2. 二次探测：主要解决主集团的问题。解决碰撞的方程式为F(i) = i ^ 2
    1. 若保证表格大小是一个质数且负载系数始终在0.5一下，每插入一个元素所需要的探测次数不多于2
    2. 二次探测可以消除主集团的问题却可能造成次集团的问题：两个元素经过hash func计算后得到的位置一致。消除的办法有复式散列等，这里不做详述
3. 开链：每一个表格元素维护一个list，在list上进行元素的插入、删除和查询操作  
**stl的hash table就是采取开链的手法**  
hash table设计架构：  
1. _hashtable_node：具有指向下一个节点的指针和实值
2. _hashtable_iterator：具有目前指向的节点和容器ht（保持和容器的连结关系）还有一些基本操作符
    1. operator++()：若迭代器指向的节点的下一个节点仍在当前的list上，则获取下一个迭代器很容易；若当前节点为list的最后一个节点则需要找到下一个非空的桶的第一个节点
3. hashtable：用vector实现buckets，其中装的是指向node节点的指针
    1. stl以质数大小的表格来设计hashtable，并且先提前计算出大致呈现两倍关系的28个质数以备随时访问。并且以函数_stl_next_prime(unsigned long n)找出最接近并且大于等于n的质数（lower_bound()辅助完成）
    2. 重建表格原则：总元素个数大于buckets的size，由此可见，每个list的最大容量即为buckets.size()
    3. btk_num()函数：用于判断元素应该落到哪个bucket中，有两个重载，分别是btk_num(key,n)和btk_num(key)，分别以n和buckets.size()为除数使用hash func对键值key计算出来的结果对其进行取模
    4. hash functions：针对char，int，long等整数型别，这里什么也没做只是返回原值，而对于const char*，即字符指针，单独设计了一个转换函数h(i) = 5 * h(i-1) + *s。stl的hashtable无法处理除了这些型别以外的任何型别，用户必须为另外的型别自定义hashfunc
hash_set，hash_map，multi_hash_set，multi_hash_map可以与set和map对称  