# Recursive Functions

## 1. 定义

A function is called *recursive* if the body of the function calls the function itself, either directly or indirectly.

示例代码：

```python
def sum_digits(n)：
	"""Return the sum of the digits of positive integer n."""
    if n < 10:
        return n
    else:
        all_but_last, last = n // 10, n % 10
        return sum_digits(all_but_last) + last

sum_digits(738)
```

Environment Diagram:

<img src="https://s1.ax1x.com/2020/07/22/UbWsqx.png" alt="Environment Diagrams" style="zoom: 50%;" />

## 2. 递归函数的分析

> A common pattern can be found in the body of many recursive functions. 
>
> The body begins with a **base case**, a conditional statement that defines the behavior of the function for the inputs that are simplest to process. 
>
> The base cases are then followed by one or more **recursive calls**.  Recursive calls always have a certain character: they simplify the original problem.

递归函数  = 递归基 + 递归调用

递归基处理平凡情况，递归调用的作用是简化原问题（分治或减治）。

## 3. Recursive leap of faith

>While we can unwind the recursion using our model of computation, it is often clearer to **think about recursive calls as functional abstractions**. That is, we should not care about how `fact(n-1)` is implemented in the body of `fact`; we should simply trust that it computes the factorial of`n-1`. Treating a recursive call as a functional abstraction has been called a **recursive leap of faith**. 

以计算n的阶乘为例：

```python
def fact(n):
	if n == 1:
        return 1
    else:
        return n * fact(n-1)
```

在计算fact(n)的时候，我们不要尝试去理解fact(n-1)是如何计算出来，而是应当将fact(n-1)看作一个抽象的函数（或者看作一个“黑箱”？），我们仅仅需要相信它能计算出n-1的阶乘。

## 4. Mutual Recursion

When a recursive procedure is divided among two functions that call each other, the functions are said to be *mutually recursive*.

示例：

判断一个数是奇数还是偶数：

* 0是偶数
* 如果n-1是奇数，则n是偶数
* 如果n-1是偶数，则n是奇数

代码：

```python
def is_even(n):
    if n == 0:
        return True
    else:
        return is_odd(n-1)

def is_odd(n):
    if n == 0:
        return False
    else:
        return is_even(n-1)
result = is_even(4)
```

Environment Diagram:

<img src="https://s1.ax1x.com/2020/07/22/UbbJ1K.png" alt="UbbJ1K.png" style="zoom: 50%;" />

可以看到mutual recursion的environment diagram的特点是两个函数的frame交替出现。

> Mutually recursive functions can be turned into a single recursive function by breaking the abstraction boundary between the two functions.

Mutually recursive functions可以转化为单一的递归函数。

```python
def is_even(n):
    if n == 0:
        return True
    else:
        if (n-1) == 0:
            return False
        return is_even((n-1)-1)
```

所以如何打破两个相互调用递归函数之间的抽象层？

在本例中，将is_even(n)中else语句块里面的is_odd(n-1)使用is_odd函数具体实现进行替换。

## 5. Tree Recursive

Another common pattern of computation is called tree recursion, in which a function **calls itself more than once**.

示例程序：

```python
def fib(n):
    if n == 1:
        return 0
    elif n == 2:
        return 1
    else:
        return fib(n-1) + fib(n-2)
result = fib(6)
```

递归调用树：

<img src="https://s1.ax1x.com/2020/07/22/UbLGLD.png" alt="UbLGLD.png" style="zoom: 50%;" />

