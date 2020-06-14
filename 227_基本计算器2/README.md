# [227. 基本计算器 II](https://leetcode-cn.com/problems/basic-calculator-ii/)

实现一个基本的计算器来计算一个简单的字符串表达式的值。

字符串表达式仅包含非负整数，+， - ，*，/ 四种运算符和空格  。 整数除法仅保留整数部分。

示例 1:

输入: "3+2*2"
输出: 7
示例 2:

输入: " 3/2 "
输出: 1
示例 3:

输入: " 3+5 / 2 "
输出: 5
说明：

你可以假设所给定的表达式都是有效的。
请不要使用内置的库函数 eval。**~~return eval(s)直接可以A~~C**

**模拟：我写的不太好**

- 对表达式中所有的'*' '/'算法直接运算，运算后的值加入到列表中
- 遇到数字添加到stacknum中，遇到+，-添加的stackflag中
- 从前往后计算stacknum和stackflag

```python
class Solution(object):
    def calculate(self, s):
        """
        :type s: str
        :rtype: int
        """
        s = s.replace(' ','')
        stacknum = []
        stackflag = []
        i = 0
        num = 0
        mi = 10
        while i<len(s): 
            if s[i].isdigit():
                num = num*mi + int(s[i])
                if i+1>=len(s) or not s[i+1].isdigit():
                    stacknum.append(num)
                    num = 0
                    mi = 10
            elif s[i] in ['/','*']:
                numa = stacknum.pop()
                numb = 0
                j=i
                while j+1<len(s) and s[j+1].isdigit():
                    numb = numb*10 + int(s[j+1])
                    j+=1
                if s[i] == '/':
                    stacknum.append(int(numa*1.0//numb))
                else:
                    stacknum.append(numa*numb)
                i=j
            else:
                stackflag.append(s[i])
            i+=1
        num = stacknum.pop(0)
        while len(stackflag)>0 and len(stacknum)>0:
            numa = stacknum.pop()
            flag = stackflag.pop()
            if flag =='-':
                num-=numa
            else:
                num+=numa
        return num
```

**题解其中的方法**

```python
class Solution:
    def calculate(self, s: str) -> int:
        num = 0
        stack = list()
        op = '+'
        for i, c in enumerate(s):
            if c.isnumeric():
                num = num*10+int(c)
            if c in '+-*/' or i == len(s)-1:
                if op == '+':
                    stack.append(num)
                if op == '-':
                    stack.append(-num)
                if op == '*':
                    stack.append(stack.pop()*num)
                if op == '/':
                    stack.append(int(stack.pop()/num))
                op = c
                num = 0
        return sum(stack) 

作者：a-bai-152
链接：https://leetcode-cn.com/problems/basic-calculator-ii/solution/pythonde-dan-zhan-jie-fa-by-a-bai-152/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

