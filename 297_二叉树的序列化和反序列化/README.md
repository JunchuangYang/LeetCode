# [297. 二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。

请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。

```
示例: 

你可以将以下二叉树：

    1
   / \
  2   3
     / \
    4   5

序列化为 "[1,2,3,null,null,4,5]"
```

**层次遍历+层次遍历构建二叉树**

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Codec:

    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
        if not root:
            return []
        res = []
        queue = [root]
        while len(queue)>0:
            length = len(queue)
            for i in range(length):
                node = queue.pop(0)
                if not node:
                    res.append('null')
                else:
                    res.append(str(node.val))
                    queue.append(node.left)
                    queue.append(node.right)
        #print(res)
        return  ','.join(res)
    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """
        if len(data) == 0:
            return None
        data = data.split(',')
        root = TreeNode(int(data[0]))
        queue = [root]
        index = 0
        while len(queue)>0:
            length = len(queue)
            for i in range(length):
                node = queue.pop(0)
                if node:
                    if data[index+1]!='null':
                        nodeleft = TreeNode(int(data[index+1]))
                        node.left = nodeleft
                        queue.append(nodeleft)
                    index+=1
                    if data[index+1]!='null':
                        noderight = TreeNode(int(data[index+1]))
                        node.right = noderight
                        queue.append(noderight)
                    index+=1
        return root

# Your Codec object will be instantiated and called as such:
# codec = Codec()
# codec.deserialize(codec.serialize(root))
```

