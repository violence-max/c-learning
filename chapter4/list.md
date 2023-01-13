> list的本质是一个双向链表，插入和删除的操作的时间复杂度都是常数级别的。相较于vector，list复杂了许多，但是其对于空间的运用是绝对精确的，一点也不浪费
> 
- list和list的节点的结构是分开设计的，list的节点是具有pre节点和next节点的双向链表节点
- list不像vector一样以普通指针作为迭代器，因为其节点不保证在存储空间中连续存在
- list其实不仅是一个双向链表还是一个环状双向链表，其内部设计具有一个node节点，这个node节点为双向链表的最后一个节点，为了符合STL设计规范之“前闭后开”的原则，node.next作为begin()，node作为end()  
**list的一些基本操作：**  
1. insert(iterator,x)：在iterrator位置之前插入值为x的节点并且返回该节点
2. erase(iterator)：删除iterator位置上的节点并且返回原iterator位置的下一个节点
3. remove(x)：移除所有值为x的节点
4. unique()：移除数值相同的连续元素，只有连续而相同的元素才会被移除只剩一个
5. list内置了一个transfer操作，接受参数为pos，first，last，意为将一条链表的first到last之间的节点（满足左闭右开的原则）移动到另一条链表的pos位置之前，其实就是操作两个链表之间的指针而已，不难。书上说可以对同一个链表进行transfer操作，经过我在纸上的演算，发现如果pos在first和last之间，结果会很丑陋，而且感觉没有什么规律  
    transfer操作为splice操作、sort操作和merge操作奠定了良好的基础  
6. splice(iterator position,list &x)：将链表x接合到pos之前，x必须不同于链表list本身  
    重载：  
    splice(iterator position,list&,iterator i)：将i所指元素移动到pos位置之前，可以是同一个链表（如果位置i和pos相同或者位置i刚好在pos之前则不做改变）  
    splice(iterator position,list&,iterator first,iterator last)：将first到last之间的节点（左闭右开）移动到pos位置之前，可以是同一个链表（但是pos不能位于first和last之间，算是对前面第5点的解释了，不过没有看到代码相关的判断部分）  
7. merge(list<T,Alloc>& x)：将x和*this（链表list本身）按从小到达的顺序进行合并的操作，但是前提条件是x和list本身都是递增的，即提前排序过的
8. reverse()：将list的内容逆向重置（感觉可以出一个双向链表方向的算法题了，这里简直就是transfer思路的完美呈现，记录一下：
    1. 如果链表没有节点或者只有一个节点，即size() == 0 或者 size() == 1则无需操作，直接返回。在stl里面有比用等于符号判断更加快速的操作，因为一个链表的node默认是end()之后的节点，因此直接用node→next == next 和 （node→next）→next == next来判断即可
    2. 另first迭代器等于begin()，然后向前移动first，在first不等于last迭代器的时候进行while循环：  
        使用old迭代器记录first，向前移动first，调用transfer(begin(),old,first)来将old位置上的节点移动到开始节点之前  
    ）  
9. sort()：使用的不是algorithm下的sort()算法而是自己的sort算法，但是没看懂  