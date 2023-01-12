> vector与array非常相似，array是静态空间，一旦配置了就不能改变，而vector是动态空间，随着元素的加入，它的内部机制会自信扩充空间以容纳新的元素
>   
vector实现于更底层的<stl_vector.h>，引用于<vector>  
vector采取的数据结构为线性连续空间  
vector的扩充空间包括三个步骤：重新配置、移动数据、释放原空间。vector的动态扩充空间的操作可能造成记忆体的重新配置，导致原有的迭代器全部失效  
- **vector的insert操作**：  
由于vector是我平常使用最多的容器，基本上其实现细节都有一些了解，而且其实现细节也没有那么多tricky的地方，只有这个insert的流程是稍微有点复杂的，因为涉及到了一点点内存配置的问题，这里记录一下其实现细节：  
insert(pos,n,x)：在pos位置之前插入n个x  
1. 只有当n不等于0时才进行以下操作：
2. 如果备用空间大于等于新增元素个数：
    1. 计算插入点之后的现有元素个数elems_after，用old_finish记录原来的使用空间的尾
    2. 如果插入点之后的现有元素个数大于新增元素的个数：
        1. 在finish位置上调用uninitialized_copy将finish位置之前的n个位置上的元素从朋友到备用空间上并且移动finish
        2. 调用copy_backward(position,old_finish-n,old_finish)将插入位置到已经进行copy的区间内的元素的头位置之间的元素移动到尾部为old_finish位置之前
        3. 在position位置上调用fill函数来填充n个位置的值
    3. 如果插入点之后的现有元素个数小于或者等于新增元素的个数：
        1. 调用uninitialized_fill_n来将finish之后的elems_after个位置填充元素x并且移动finish到尾部
        2. 调用uninitialized_copy来讲position到old_finish位置上的元素复制到finish之后的那段区间并且移动finish到末尾
        3. 调用fill函数来填充position到old_finish位置上的元素为x
3. 如果备用空间小于新增元素个数：
    1. 首先决定新长度：len = old_size + max(old_size + n)
    2. 调用allocate(len)配置新的vector空间
    3. 尝试：
        1. 将插入点之前的元素用uninitialized_copy复制到新空间
        2. 调用uninitialized_fill_n将插入点之后的n个位置填充x
        3. 调用uninitialized_copy将插入点之后的元素复制到新空间
    4. 如果有异常发生：
        1. 根据”*commit or roll back*”语意，销毁新配置空间上的对象并且释放这段空间
    5. 清楚并释放旧的vector，调整start，finish和end_of_storage迭代器