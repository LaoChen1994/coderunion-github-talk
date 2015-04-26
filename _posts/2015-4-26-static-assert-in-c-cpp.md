---
layout: post
title: "C/C++中的静态断言"
date: 2015-04-26 10:50:00
tag: 
- cpp
categories: cpp
---

* content
{:toc}

原文地址:[http://codefalling.github.io/2015/01/15/static-assert-in-cpp/](http://codefalling.github.io/2015/01/15/static-assert-in-cpp/)

# 断言

断言（assert）是c语言标准库（`assert.h`）中一个使用的 *宏* ，作为一种先验条件判断程序是否如预期执行，断言在判断失败时 *立即结束程序并返回错误地点* ，其作用是 **让错误尽可能早的暴露** 。C标准库的 `assert` 是在运行时进行的，事实上大多断言也的确应该在运行时进行以保证运行如期望的那样进行。

而这篇文章介绍的是编译器进行的静态断言（static assert），它可以借助编译器在编译阶段对某些 *常量* 进行检查，例如系统常量或者是用户自定义常量。在 C++ 的模板中也可用于 **检查模板参数的有效性**。

# 尽可能早的发现错误

当有错误（这里指逻辑错误）的时候，我们应该 **尽可能早的暴露其存在** ，而不是在程序原理错误现场后才发现一个完全摸不着头脑的问题，而这极其不利于调试。将这条守则贯彻的最彻底的当为 [契约式设计](http://zh.wikipedia.org/wiki/%E5%A5%91%E7%BA%A6%E5%BC%8F%E8%AE%BE%E8%AE%A1)。

# 静态断言（Static Assertion）

静态断言是`C++11` 开始引入的特性，语法为 `static_assert (bool_constexpr,message)`

## 举例

``` c++
static_assert(sizeof(void *) == 4, "64-bit code generation is not supported.");
```

使用64位的 `clang++` 编译，得到下面的错误信息（注意开启 `--std=c++11` ）

``` bash
static_assert.cpp:7:5: error: static_assert failed "64-bit code generation is not supported."
    static_assert(sizeof(void *) == 4, "64-bit code generation is not supported.");
        ^             ~~~~~~~~~~~~~~~~~~~
        1 error generated.
```

# static_assrt 的其他实现

在没有支持C++11的地方，我们可以借助编译器的其他特性实现`static_assrt`

## 利用数组大小不能为 **负数**

只要简单的一个宏就能实现

``` cpp
#define STATIC_ASSERT(expr) {char unnamed[(expr)?1:-1];}
int main(){
    STATIC_ASSERT(sizeof(void *) == 4); /*64-bit code is not supported*/
    return 0;
}
```

使用64位clang++编译得，

``` bash
static_assert.c:7:5: error: 'unname' declared as an array with a negative size
    STATIC_ASSERT(sizeof(void *) == 4); /*64-bit code is not supported*/
        ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        static_assert.c:3:42: note: expanded from macro 'STATIC_ASSERT'
        #define STATIC_ASSERT(expr) {char unname[(expr)?1:-1];}
                                                 ^~~~~~~~~~~
                                                 1 error generated.
```

可以看到 `STATIC_ASSERT` 宏也能起到类似的作用，但是不能传递我们自定义的错误信息，只能写在同一行的注释里。

**注意**

《Modern C++ Design》里也曾提到这个方法，但是使用的数组长度是0，然而我尝试各个C和C++编译器 `char unnamed[0]` 都能合法编译通过，即使使用参数 `-pedantic` 也只能得到一个 warring 而不是 error。所以建议 **不要使用声明长度为0的数组来产生错误**，虽然 ISO C 规定其为非法，但大多编译器上它仍然能毫无障碍的编译通过。

参考[GCC-DOC](https://gcc.gnu.org/onlinedocs/gcc-4.1.2/gcc/Zero-Length.html)

## 利用模板参数暴露错误

``` cpp
template<bool>
struct StaticAsserter
{
    StaticAsserter(...);
};

template<>
struct StaticAsserter<false> {};

#define STATIC_ASSERT_CPP(expr,msg)\
{\
    class STATIC_ASSERT_FAILED_##msg{};\
    sizeof((StaticAsserter<(expr)>(STATIC_ASSERT_FAILED_##msg())));\
}
int main(int argc, char const* argv[])
{
    STATIC_ASSERT_CPP(sizeof(void *) == 4, 64BIT_NOT_SUPPORT);
    return 0;
}
```

这样的实现一定程度上提供了自定义错误信息的能力，`clang` 的报错为

``` bash
error: no matching conversion for functional-style cast from
      'STATIC_ASSERT_FAILED_64BIT_NOT_SUPPORT' to 'StaticAsserter<(sizeof(void *) == 4)>'
```

因为`sizeof`属于编译期行为，它只会对表达式进行编译期求值，并不会造成运行时损耗。

