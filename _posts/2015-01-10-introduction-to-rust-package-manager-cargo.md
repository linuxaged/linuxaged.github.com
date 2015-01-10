---
layout: post
title: "Introduction to Rust Package Manager Cargo"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Cargo.lock 会记录所有依赖 repo 的版本号，例如：
	
	[[package]]
	name = "render"
	version = "0.1.0"
	source = "git+http://github.com/gfx-rs/gfx-rs#e8563cda1ed1167b4d1a8581000961fac2a38320"
	dependencies = [
 	"device 0.1.0 (git+http://github.com/gfx-rs/gfx-rs)",
 	"log 0.1.9 (registry+https://github.com/rust-lang/crates.io-index)",
	]
	
你可以通过 `cargo update` 命令来更新依赖 repo 的版本。