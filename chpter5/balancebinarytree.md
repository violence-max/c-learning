> 二叉搜索树的节点放置规则为：任何节点的键值一定大于其左子树中节点的键值且小于其右子树中节点的键值
>   
特性：  
1. 可提供对数时间的元素插入和访问操作
2. 从根节点分别一直向左走和向右走就可以得到最小值和最大值  
操作：  
1. 插入操作：从根节点开始，遇到键值较大的节点就向左走，遇到键值较小的就向右走，最终插入到尾端
2. 删除操作：如果待删除节点只有一个子节点，则将该子节点直接与父节点相连即可；如果待删除节点有两个子节点，则向右寻找最小节点作为新的节点进行替换