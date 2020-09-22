---
layout: post
title: roblox Obyy
date: 2020-09-21 11:06 -0700
---



[Game](https://www.roblox.com/games/5717257383/Roblox-Obby-Youtube)


## Video 1 Code

This is the code that moves the player

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

## Part 4 Code
This is code the disappearing bridge.

```lua
local bridge1 = script.Parent.Bridge1
local bridge2 = script.Parent.Bridge2
local bridge3 = script.Parent.Bridge3

function addTouchEvent(bridgePart)
	local function touchedPart(hitPart)
		bridgePart.BrickColor = BrickColor.new("Bright red")
		bridgePart.Material = Enum.Material.Neon
		wait(.2)
		bridgePart.Transparency = 1
		bridgePart.CanCollide = false
	end
	
	bridgePart.Touched:Connect(touchedPart)
end

addTouchEvent(bridge1)
addTouchEvent(bridge2)
addTouchEvent(bridge3)
```