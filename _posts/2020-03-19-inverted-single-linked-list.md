---
layout: post
title: 头插法反转链表
date: 2020-3-19
categories: blog
tags: [算法,链表]
---

本文介绍Ｇolang使用头插法反转一个包含头节点的单链表.
示例:

输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
先写一个大致的流程图:
![头插反转链表法流程图](http://www.plantuml.com/plantuml/png/SoWkIImgAStDuG8pk1I0mEh5kWrFzqvzkdVoqyxU5rrDpvjsQZnRk-JfafOdEtOzNxFcoOw69pjMGIIUzcv-kcGBXFsif_tfX8dFPxL0k9hM4DEUxrttVEKkk2gW_Fizwr8hIY3IHQa5gOaGUp7iXca0sjiDiVJvxib04Mk4ygSRsc1g4Ku0seMf5IY2eHNDwN2JyDhnT4BVy-MxaLslK9wHcPEge86iwicENc1e0JrkhwcGMQoWKPbQh0aCu_m2BeVKl1HWW0C0)

流程图出来了代码实现就很简单了:

```go
func reverseList(head *Structure.MyList) *Structure.MyList {
	if head == nil || head.Next == nil {
		return nil
	}

	var pCur *Structure.MyList  // 当前节点
	var pNext *Structure.MyList // 后继结点

	pCur = head.Next	// 记录当前节点
	head.Next = nil		// 置空头节点的Ｎext
	for {
		pNext = pCur.Next		// 记录当前节点的下一个节点
		pCur.Next = head.Next	// 将当前节点指向头节点的下一个节点
		head.Next = pCur		// 将头节点指向当前节点
		pCur = pNext			// 将当前节点后移
		if pCur == nil {		// 如果当前节点为空表示已经插入完成
			break
		}
	}
	return head
}
```

我们测试一下:

```go
func main() {
	l := Structure.NewListByDatas(1, 2, 3, 4, 5)
	fmt.Println(l)
	fmt.Println(reverseList(l))
}
```

输出结果如下:

```go
MyList[len: 5; Elements: 1, 2, 3, 4, 5]
MyList[len: 5; Elements: 5, 4, 3, 2, 1]
```

反转完成.