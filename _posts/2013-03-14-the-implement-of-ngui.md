---
layout: post
title: "the implement of NGUI"
description: ""
category: 
tags: []
---
{% include JB/setup %}
#参考
http://www.tasharen.com/?page_id=160


NGUI 是基于组件设计的,所有的UI单元都要组织在 UIPanel 下面,然后用一个 UICamera 来渲染 UI
#NGUI 组成
1.数据结构
BetterList

2.GUI组件
UINode
UIPanel
UIWidget
#NGUI 事件传递
UICamera 绑定的相机绘制的对象(绑定了collider)添加以下事件:
	void OnHover() 
	void OnPress() 
	void OnClick()
	void OnDoubleClick()
	void OnSelect()
	void OnDrag()
	void OnDrop()
	void OnInput()
	void OnTooltip()
	void OnScroll()
	void OnKey()