---
layout: post
title: Golang实现一个最简单的链表 
date: 2020-3-18
categories: blog
tags: [Golang,链表]
---

为了后续方便, 本文实现一个最简单的链表结构, 基本上只包含新建(New)和添加(Add)两个方法. 此结构不考虑安全性易用性等, 仅为了后续实现链表相关算法时方便创建新的链表.



新建一个文件夹Structure, 然后新建文件mylist.go.

先定义链表数据类型:

``` go
package Structure

import "fmt"

type MyList struct {
	Elem interface{}
	Next *MyList
}
```

定义New方法:

```go
func NewList() *MyList {
	head := new(MyList)
	head.Elem = 0
	head.Next = nil
	return head
}
```

可见New方法很简单, 就是返回一个头节点. 头节点的元素表示链表长度.

给MyList定义Add方法:

```go
func (l *MyList) Add(elem interface{}) *MyList {
	if l == nil {
		_ = fmt.Errorf("%s\n", "You need to create a linked list before adding elements. Call the New method to get an empty linked list containing the head node.")
		return nil
	}
	a := new(MyList)
	a.Elem = elem
	a.Next = l.Next
	l.Next = a
	if v, ok := l.Elem.(int); ok == true {
		l.Elem = v + 1
		return l
	} else {
		_ = fmt.Errorf("%s\n", "The head node element is wrong, please make sure that the element of the head node is of type int.")
		return nil
	}
}
```

这个Add方法也很简单, 新建一个MyList数据, 将元素设置为参数elem, 然后将这个新加的链表节点插入到头节点后面.

一个一个的Add感觉还是挺麻烦的, 为了方便, 我们再定义一个New方法:

```go
func NewListByDatas(elems ...interface{}) *MyList {
	head := NewList()
	for i := len(elems) - 1; i >= 0; i-- {
		head.Add(elems[i])
	}
	return head
}
```

这个New方法就是遍历参数数据, 然后逐一插入到链表中去. 由于我们的Add方法是将数据插入到头节点后, 所以为了保持链表顺序跟参数顺序一致, 我们这里Add的顺序是跟参数顺序相反的.

最后, 我们定义一个String方法, 这样可以很方便地使用fmt包输出我们的数据:

```go
func (l *MyList) String() string {
	s := "MyList[len: "
	s = s + fmt.Sprintf("%d", l.Elem.(int)) + "; Elements: "
	for e := l.Next; e != nil; e = e.Next {
		s = s + fmt.Sprintf("%v", e.Elem) + ", "
	}
	s = s[:len(s)-2] + "]"
	return s
}
```

我们输出链表的长度以及各个元素的值.

测试一下:

```go
package main

import (
	"fmt"
	"github.com/chive-chan/Go/Structure"
)

func main() {
	l1 := Structure.NewList()
	l1.Add(44)
	l1.Add(89)
	l1.Add(9)
	fmt.Println(l1)

	l := Structure.NewListByDatas(1, 2, 3)
	l.Add(5)
	fmt.Println(l)
}
```

保存并运行main输出如下:

```shell
MyList[len: 3; Elements: 9, 89, 44]
MyList[len: 4; Elements: 5, 1, 2, 3]
```

可见我们的New和Add都能正常运行.

因为我们的链表元素定义为interface类型, 所以我们甚至能Add其它类型的数据, 将main改为如下测试一下:

```go
func main() {
	l := Structure.NewListByDatas(1, 2.8, "string", []int{1, 2, 3}, []string{"A", "B", "C"})
	fmt.Println(l)
}
```

输出如下:

```shell
MyList[len: 5; Elements: 1, 2.8, string, [1 2 3], [A B C]]
```

OK~ 偷懒完成, 以后需要链表时就可以直接调用它了~