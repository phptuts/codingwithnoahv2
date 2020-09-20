---
layout: post
title: Roblox leaderboard with rotating coin
date: 2020-09-19 18:42 -0700
tags: ['Roblox']
---

<iframe width="560" height="315" src="https://www.youtube.com/embed/Jqjdy3GHSrY" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


## Leaderboard script
```lua
function onPlayerJoin(player)
	local leaderboard = Instance.new("Folder")
	leaderboard.Name = "leaderstats"
	leaderboard.Parent = player

	local coins = Instance.new("IntValue")
	coins.Name = "Coins"
	coins.Value = 0
	coins.Parent = leaderboard
end

game.Players.PlayerAdded:Connect(onPlayerJoin)
```
## Player touches coin script

```lua
local coin = script.Parent

function touchedCoin(part)
	local player = game.Players:GetPlayerFromCharacter(part.Parent)
	if player then
		coin:Destroy()
		player.leaderstats.Coins.Value = player.leaderstats.Coins.Value + 1
	end
end

coin.Touched:Connect(touchedCoin)
```

## Coin rotating script

```lua
local coin = script.Parent

while true do
	coin.Orientation = coin.Orientation + Vector3.new(0, 30, 0)
	wait(.1)
end
```
