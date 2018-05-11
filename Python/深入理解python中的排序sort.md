> * 基本排序 Sorting Basics
> * key函数Key Functions
> * operator库函数自定义排序（ Operator Module Functions）
> * 升序和降序Ascending and Descending
> * 排序的稳定性和复杂排序 （Sort Stability and Complex Sorts）
> * 传统的DSU(Decorate-Sort-Undecorate)的排序方法
> * 利用cmp方法进行排序的原始方式
> * 其他

# 基本排序 Sorting Basics
进行一个简单的升序排列直接调用sorted()函数，函数将会返回一个排序后的列表：
```python
>>> sorted([5, 2, 3, 1, 4])
[1, 2, 3, 4, 5]
```
**sorted函数不会改变原有的list，而是返回一个新的排好序的list**
```python
>>> list = [1,3,2,4,5,3,2]
>>> sorted(list)
[1, 2, 2, 3, 3, 4, 5]
>>> list
[1, 3, 2, 4, 5, 3, 2]
```

如果你想使用就地排序，也就是改变原list的内容，那么可以使用list.sort()的方法，这个方法的返回值是None。
```python
>>> a = [5, 2, 3, 1, 4]
>>> a.sort()
>>> a
[1, 2, 3, 4, 5]
```
另一个区别是，list.sort()方法只是list也就是列表类型的方法，只可以在列表类型上调用。而sorted方法则是可以接受任何可迭代对象。
```python
>>> sorted({1: 'D', 2: 'B', 3: 'B', 4: 'E', 5: 'A'})
[1, 2, 3, 4, 5]
```

# key函数Key Functions

list.sort()和sorted()函数都有一个key参数，可以用来指定一个函数来确定排序的一个优先级。比如，这个例子就是根据大小写的优先级进行排序：
```python
>>> sorted("This is a test string from Andrew".split(), key=str.lower)
['a', 'Andrew', 'from', 'is', 'string', 'test', 'This']
```

key参数的值应该是一个函数，这个函数接受一个参数然后返回以一个key，这个key就被用作进行排序。这个方法很高效，因为对于每一个输入的记录只需要调用一次key函数。
一个常用的场景就是当我们需要对一个复杂对象的某些属性进行排序时：
```python
>>> student_tuples = [
... ('john', 'A', 15),
... ('jane', 'B', 12),
... ('dave', 'B', 10),
... ]
>>> sorted(student_tuples, key=lambda student: student[2]) # sort by age
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```
再如：
```python
>>> class Student:
... def __init__(self, name, grade, age):
... self.name = name
... self.grade = grade
... self.age = age
... def __repr__(self):
... return repr((self.name, self.grade, self.age))
```

```python
>>> student_objects = [
... Student('john', 'A', 15),
... Student('jane', 'B', 12),
... Student('dave', 'B', 10),
... ]
>>> sorted(student_objects, key=lambda student: student.age) # sort by age
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```

# operator库函数自定义排序（ Operator Module Functions）

前面我们看到的利用key-function来自定义排序，同时Python也可以通过operator库来自定义排序，而且通常这种方法更好理解并且效率更高。
operator库提供了 itemgetter(), attrgetter(), and a methodcaller()三个函数

```python
>>> from operator import itemgetter, attrgetter
```

```python
>>> sorted(student_tuples, key=itemgetter(2))
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```

```python
>>> sorted(student_objects, key=attrgetter('age'))
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```

同时还支持多层排序

```python
>>> sorted(student_tuples, key=itemgetter(1,2))
[('john', 'A', 15), ('dave', 'B', 10), ('jane', 'B', 12)]
```

```python
>>> sorted(student_objects, key=attrgetter('grade', 'age'))
[('john', 'A', 15), ('dave', 'B', 10), ('jane', 'B', 12)]
```

# 升序和降序Ascending and Descending

list.sort()和sorted()都有一个boolean类型的reverse参数，可以用来指定升序和降序排列，默认为false，也就是升序排序，如果需要降序排列，则需将reverse参数指定为true。

```python
>>> sorted(student_tuples, key=itemgetter(2), reverse=True)
[('john', 'A', 15), ('jane', 'B', 12), ('dave', 'B', 10)]
```

```python
>>> sorted(student_objects, key=attrgetter('age'), reverse=True)
[('john', 'A', 15), ('jane', 'B', 12), ('dave', 'B', 10)]
```

# 排序的稳定性和复杂排序 （Sort Stability and Complex Sorts）

排序的稳定性指，有相同key值的多个记录进行排序之后，原始的前后关系保持不变

```python
>>> data = [('red', 1), ('blue', 1), ('red', 2), ('blue', 2)]
>>> sorted(data, key=itemgetter(0))
[('blue', 1), ('blue', 2), ('red', 1), ('red', 2)]
```
我们可以看到python中的排序是稳定的。

我们可以利用这个稳定的特性来进行一些复杂的排序步骤，比如，我们将学生的数据先按成绩降序然后年龄升序。当排序是稳定的时候，我们可以先将年龄升序，再将成绩降序会得到相同的结果。

```python
>>> s = sorted(student_objects, key=attrgetter('age')) # sort on secondary key
>>> sorted(s, key=attrgetter('grade'), reverse=True) # now sort on primary key, descending
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```

# 传统的DSU(Decorate-Sort-Undecorate)的排序方法

传统的DSU(Decorate-Sort-Undecorate)的排序方法主要有三个步骤：
* 给list添加一个新的值，这个值一般是用来控制排序的顺序（Decorate）
* 排序
* 将添加的值去掉，也就是Undecorate
具体可以看下面的例子：
```python
>>> decorated = [(student.grade, i, student) for i, student in enumerate(student_objects)]
>>> decorated.sort()
>>> [student for grade, i, student in decorated] # undecorate
[('john', 'A', 15), ('jane', 'B', 12), ('dave', 'B', 10)]
```
因为元组是按字典序比较的，比较完grade之后，会继续比较i。
添加index的i值不是必须的，但是添加i值有以下好处：
- 可以保证排序的稳定性，如果key值相同，就可以利用i来维持原有的顺序
- 原始对象的item不用进行比较，因为通过key和i的比较就能将数组排序好

现在python3提供了key-function，所以DSU方法已经不常用了

# 利用cmp方法进行排序的原始方式
python2.x版本中，是利用cmp参数自定义排序。
python3.x已经将这个方法移除了，但是我们还是有必要了解一下cmp参数
cmp参数的使用方法就是指定一个函数，自定义排序的规则，和java等其他语言很类似
```python
>>> def numeric_compare(x, y):
... return x - y
>>> sorted([5, 2, 4, 1, 3], cmp=numeric_compare)
[1, 2, 3, 4, 5]
```
也可以反序排列
```python
>>> def reverse_numeric(x, y):
... return y - x
>>> sorted([5, 2, 4, 1, 3], cmp=reverse_numeric)
[5, 4, 3, 2, 1]
```
python3.x中可以用如下方式：
```python
def cmp_to_key(mycmp):
'Convert a cmp= function into a key= function'
class K:
def __init__(self, obj, *args):
self.obj = obj
def __lt__(self, other):
return mycmp(self.obj, other.obj) < 0
def __gt__(self, other):
return mycmp(self.obj, other.obj) > 0
def __eq__(self, other):
return mycmp(self.obj, other.obj) == 0
def __le__(self, other):
return mycmp(self.obj, other.obj) <= 0
def __ge__(self, other):
return mycmp(self.obj, other.obj) >= 0
def __ne__(self, other):
return mycmp(self.obj, other.obj) != 0
return K
```

```python
>>> sorted([5, 2, 4, 1, 3], key=cmp_to_key(reverse_numeric))
[5, 4, 3, 2, 1]
```

# 其他

- 可以通过以下方式定义__lt__函数来指定两个对象比较的方式
```python
>>> Student.__lt__ = lambda self, other: self.age < other.age
>>> sorted(student_objects)
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```

- 排序的ley-function参数不仅仅可以依赖于排序的对象，也可以依赖于外部的对象.
如下例，成绩姓名分开存储。
```python
>>> students = ['dave', 'john', 'jane']
>>> newgrades = {'john': 'F', 'jane':'A', 'dave': 'C'}
>>> sorted(students, key=newgrades.__getitem__)
['jane', 'dave', 'john']
```
