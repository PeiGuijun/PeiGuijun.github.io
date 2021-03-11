# PeiGuijun.github.io

## 单元测试

参考文档: https://docs.python.org/zh-cn/3/library/unittest.html

### 示例
```
import unittest

# 将要被测试的排序函数
def sort(arr):
    l = len(arr)
    for i in range(0, l):
        for j in range(i + 1, l):
            if arr[i] >= arr[j]:
                tmp = arr[i]
                arr[i] = arr[j]
                arr[j] = tmp


# 编写子类继承unittest.TestCase
# 通常使用 assertEqual()、assertTrue()、assertFalse() 和 assertRaise() 等 assert 语句对结果进行验证

class TestSort(unittest.TestCase):

   # 以test开头的函数将会被测试
   def test_sort(self):
        arr = [3, 4, 1, 5, 6]
        sort(arr)
        # assert 结果跟我们期待的一样
        self.assertEqual(arr, [1, 3, 4, 5, 6])

if __name__ == '__main__':
    ## 如果在Jupyter下，请用如下方式运行单元测试
    unittest.main(argv=['first-arg-is-ignored'], exit=False)
```
## mock
通过 mock，来创建一些虚假的数据库层、文件系统层、网络层对象，以便可以简单地对核心后端逻辑单元进行测试。
```
import unittest
from unittest.mock import MagicMock

class A(unittest.TestCase):
    def m1(self):
        val = self.m2()
        self.m3(val)

    def m2(self):
        pass

    def m3(self, val):
        pass

    def test_m1(self):
        a = A()
        a.m2 = MagicMock(return_value="custom_val")
        a.m3 = MagicMock()
        a.m1()
        self.assertTrue(a.m2.called) #验证m2被call过
        a.m3.assert_called_with("custom_val") #验证m3被指定参数call过
        
if __name__ == '__main__':
    unittest.main(argv=['first-arg-is-ignored'], exit=False)
```
## Mock Side Effect
mock 的函数，属性是可以根据不同的输入，返回不同的数值，而不只是一个 return_value
```
from unittest.mock import MagicMock

def side_effect(arg):
    if arg < 0:
        return 1
    else:
        return 2
mock = MagicMock()
mock.side_effect = side_effect
```
## patch
```
from unittest.mock import patch

@patch('sort')
def test_sort(self, mock_sort):
    ...
    ...
```
