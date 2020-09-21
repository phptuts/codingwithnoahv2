---
layout: post
title: roblox obby part 1 make a part that can move the player
date: 2020-09-21 11:06 -0700
---

<iframe width="560" height="315" src="https://www.youtube.com/embed/-03TRSMqwYU" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


[Game](https://www.roblox.com/games/5717257383/Roblox-Obby-Youtube)


## Code

```lua
local moverPart = script.Parent
local direction = "right"

moverPart.BodyVelocity.Velocity = Vector3.new(3, 0, 0)

while true do
	if direction == "right" and moverPart.Position.X > 27 then
		direction = "left"
		moverPart.BodyVelocity.Velocity = Vector3.new(-3, 0, 0)
	end

	if direction == "left" and moverPart.Position.X < 13 then
		direction = "right"
		moverPart.BodyVelocity.Velocity = Vector3.new(3, 0, 0)
	end

	wait(.001)
end
```