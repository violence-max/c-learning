> 根据数据在容器中排列的特性，可以分为序列式容器和关联式容器
>  
标准的STL关联式容器分为set和map两大类，以及两大类的衍生体multiset和multimap。这些容器的底层机制均由红黑树完成，红黑树本身也是一个容器，但是不对外开放  
sgi stl还提供了一个不在标准规格之列的关联式容器hash table（之所以不在c++标准规格的原因是提案太迟了，笑死我了），以及以此为底层机制而完成的hash set，hash map，hash multiset和hash multimap  
- 至此，可以将stl中常见的容器统一描述：  
stl中的容器之间的关系是衍生而非继承，所谓衍生，指的是某一容器的底层机制是另一容器，如：heap的底层是vector，priority_queue的底层是heap，stack和queue的底层是deque；set/map/multiset/multimap的底层是RB-tree，hash_x的底层是hash table  
- 一般而言，关联式容器的内部结构是一个二叉平衡树，以便获得良好的搜索效率，而二叉平衡树又有许多种类型，如：AVL-tree，RB-tree，AA-tree。运用得最广泛得是RB-tree  
* [树的导览](/chpter5/tree.md)
* [关联式容器](/chpter5/assotiatedcontainers.md)  