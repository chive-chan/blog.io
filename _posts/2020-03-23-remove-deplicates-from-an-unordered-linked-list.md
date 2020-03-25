---
layout: post
title: 从无序链表中移除重复项
date: 2020-3-23
categories: blog
tags: [算法,链表]
---

给定一个没有排序的链表， 去掉其重复项， 并保留原顺序。
示例:

原链表: 1->3->1->5->5->7
移除后: 1->3->5->7
思路很简单, 逐一遍历然后看后面的数据有没有重复的, 有就删除, 写一个简单的流程图:
![从无序链表中移除重复项](http://www.plantuml.com/plantuml/png/bPEnJiCm48PtFuN7-1NQWRuA4gaQAQG81o4sL9MY0gcA40omHaAjsb04WrkKFesl3c_1DHukWq7c9jloz_d-dDAHwU5fUdOSith1E5QhAbPGFaBv0EEikXcOmJg0_0csSpDv4y5kcKC-r6fZ5WkjBhMQspkGVK3-QTUxkskOtfldeTsATYq-xqLnoCdLV46hRA-SRGu4ZoGFBa8f4BBLWjK8HbUMGkHxzcwfgjYQia3GwMmx17J5Gu01jBgbaAIaC6Zr3KCLTItfUdH_NgpDYpBBiU1fBqmSWb-_2hND6drJJWimhw1KOImnQzB_HnFCOpRaJuwYBftyDJ6OJiph5-68rgDWJPAUT9D4oRH_mZgunN_W1m00)

流程图看起来比较绕, 直接看代码可能更清晰(MyList的定义参考: [Golang实现一个最简单的链表](http://blog.chive-chan.com/blog/2020/03/18/a-simple-list/)):

```go
func RemoveDeplicatesLikedList(head *Structure.MyList) *Structure.MyList {
	if head == nil || head.Next == nil {
		return nil
	}
	outerCur := head.Next          //用于外层循环, 指向链表的第一个节点
	var innerCur *Structure.MyList //用于内层循环遍历 outerCur 后面的节点
	var innerPre *Structure.MyList //innerCur的前驱节点
	for ; outerCur != nil; outerCur = outerCur.Next {
		for innerCur, innerPre = outerCur.Next, outerCur; innerCur != nil; innerCur = innerCur.Next {
			if outerCur.Elem == innerCur.Elem {
				innerPre.Next = innerCur.Next
			} else {
				innerPre = innerCur
			}
		}
	}
	return head
}
```

运行一下试试:

```go
func main() {
	head := Structure.NewListByDatas(1, 3, 1, 5, 5, 7)
	fmt.Println(head)
	fmt.Println(RemoveDeplicatesLikedList(head))
}
```

输出结果如下:

```go
MyList[len: 6; Elements: 1, 3, 1, 5, 5, 7]
MyList[len: 6; Elements: 1, 3, 5, 7]
```

完成.