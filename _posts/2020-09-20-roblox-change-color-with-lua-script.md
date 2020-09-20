---
layout: post
title: Roblox Change Color with lua script
date: 2020-09-20 09:24 -0700
tags: ['Roblox']
---


<iframe width="560" height="315" src="https://www.youtube.com/embed/8nqwN8ZZ710" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

```lua
local part = script.Parent

part.BrickColor = BrickColor.new("Burlap")

while true do
	wait(3)
	part.BrickColor = BrickColor.new("Burlap")
	wait(3)
	part.BrickColor = BrickColor.new("Bright orange")
	wait(3)
	part.BrickColor = BrickColor.new(Color3.fromRGB(53, 50, 240))
end

```