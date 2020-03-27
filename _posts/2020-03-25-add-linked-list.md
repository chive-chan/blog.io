---
layout: post
title: 从无序链表中移除重复项
date: 2020-3-23
categories: blog
tags: [算法,链表]
---

给定两个单链表， 链表的每个节点代表一位数, 计算两个数的和。
示例:

输入链表: 3->1->5和5->9->2
输出链表: 8->0->8(513+295=808)

注意: 个位数在链表头.

我们使用链表相加法, 对链表中的节点直接进行相加操作, 把相加的和存储到新的链表中对应的结点中, 同时还要记录结点相加后的进位.

使用这种方法需要注意:

1. 每组结点进行相加后需要记录其是否有进位;
2. 如果两个链表H1和H2的长度不同(分别为L1和L2且L1<L2), 当对链表第L1位计算完成后, 接下来只需要考虑链表H2剩余结点的值(需要考虑进位).
3. 对链表所有结点都计算完成后, 还需要考虑此时是否还有进位, 如果有则需要增加新的结点, 此结点的数据域为1.

算法流程图如下:

![链表相加法计算两个链表的和](http://www.plantuml.com/plantuml/png/lLFDRjD063pRJ-4BKYeXmTgt-bBkl0GYhcoHXXLsXRX1eL5JQXUH7rHSMdgesWXX-HGIMtPUnj-k_HPysatYM201Gkh5ilQRdMzcDBlZUazUUFjx7UrHk_tOwJwpyEjwgZXMbMqMNBFXj1havTKjd1wmKMoJ-vgjcwhYx6ejcnIpX4yITZGkNn_QX7z-1xr3YERoU4lpRAkhjLvPxfQgADlbOWZXXRYjkDvpgYfMnxK4aTuWql-EY6mGTTlEBWg6eozmynvEnkKYB38V1JC_UeeiLJWrnJo4D67ZM7gYV7XR944PnP2DU3fW5-SrQWb1mrINkEgD3q5l7S5Bs-bOBKgaQeeU_9LUnwtLylICilDl4EViHmh7Nt4x-iDj_61k2ZlH5Oy-uIAgGpQ0lYzdKZm2YlGQJeyRZf00Ffwu-tk20E8UHLPv3KSI19UNPPJpB49HYBn77gx03QfDp8CQ8tt3Xu3VfAO8MDXLbqFSJlKoVrkD5ijwz2T5ThOsXf4Y9JNgc8p_ZCVuVJQpuAw3CNu9HRw6fexlVImCaStz1xD_LapCLTr5zNVm7n_mJm00)

实现代码如下(MyList的定义参考: [Golang实现一个最简单的链表](http://blog.chive-chan.com/blog/2020/03/18/a-simple-list/)):

```go
func AddLikedList(h1 *Structure.MyList, h2 *Structure.MyList) *Structure.MyList {
	if h1 == nil || h1.Next == nil {
		return h2
	}
	if h2 == nil || h2.Next == nil {
		return h1
	}
	c := 0                            // 记录进位
	sum := 0                          // 记录两个结点相加的值
	p1 := h1.Next                     // 遍历h1
	p2 := h2.Next                     // 遍历h2
	resultHead := Structure.NewList() // 相加后链表头结点
	p := resultHead                   // 指向链表resultHead最后一个结点
	for ; p1 != nil && p2 != nil; p1, p2 = p1.Next, p2.Next {
		p.Next = &Structure.MyList{} // 指向新创建的存储相加和的结点
		sum = p1.Elem.(int) + p2.Elem.(int) + c
		p.Next.Elem = sum % 10                      // 两结点相加和
		resultHead.Elem = resultHead.Elem.(int) + 1 // 由于我们直接改变了MyList的结点而不是使用Add方法, 所以需要手动修改链表长度.
		c = sum / 10                                // 进位
		p = p.Next
	}
	// 用p3记录p1或者p2剩余的结点
	p3 := &Structure.MyList{}
	if p1 == nil {
		p3 = p2
	} else {
		p3 = p1
	}
	for ; p3 != nil; p3 = p3.Next {
		p.Next = &Structure.MyList{} // 指向新创建的存储相加和的结点
		sum = p3.Elem.(int) + c
		p.Next.Elem = sum % 10 // 两结点相加和
		resultHead.Elem = resultHead.Elem.(int) + 1
		c = sum / 10
		p = p.Next
	}
	if c == 1 {
		p.Next = &Structure.MyList{}
		p.Next.Elem = 1
		resultHead.Elem = resultHead.Elem.(int) + 1
	}
	return resultHead
}
```

运行一下试试:

```go
func main() {
	l1 := Structure.NewListByDatas(3, 1, 7)
	l2 := Structure.NewListByDatas(5, 9, 2)
	fmt.Println(l1)
	fmt.Println(l2)
	fmt.Println(AddLikedList(l1, l2))
}
```

输出结果如下:

```go
MyList[len: 3; Elements: 3, 1, 7]
MyList[len: 3; Elements: 5, 9, 2]
MyList[len: 4; Elements: 8, 0, 0, 1]
```

完成.