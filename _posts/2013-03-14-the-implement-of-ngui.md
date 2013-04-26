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

#NGUI ScrollView

以官方 example 为例,先看一下层次结构

	-UIRoot
		-UIPanel(dragable)
			-UIGrid
				-Item
					-Label
			
			
			
##制作步骤
1.新建 UI, Panel 的 scale 只能是整数

2.给panel绑定component UIDragablePanel,调整参数  scale , x=1,y=0,z=0 限制只能在x方向

3.在 panel 下空的创建子节点,重命名为 UIGrid 并且绑定 UIGrid 和 UICenterOnChirld 脚本,调整 UIGrid 中 Cell width 和 height

4.在 UIGrid 下创建新的子节点 Item,绑定 UIDragablePanelContents 并且添加 box collider

5.最后在 Item 下创建需要的 widget