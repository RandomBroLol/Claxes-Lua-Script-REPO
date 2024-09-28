# Claxes-Lua-Script-REPO

Too lazy waiting for chatgpt to generate your response well here is an repository that has basic scripts to copy! (Credits to TwoToned Studios On roblox and Purple_Lua For these codes!)
Some Scripts are modified!

## /Sit Command
```lua
game:GetService("Players").PlayerAdded:Connect(function(player)
    player.Chatted:Connect(function(msg)
        if string.lower(msg) == "/sit" then
            if not player.Character then return end

            player.Character:WaitForChild("Humanoid").Sit = true
        end
    end)
end)
```

## Badge Door
```lua
local badge_id = 0 -- replace 0 with your badge id
script.Parent.Touched:Connect(function(hit)
	local player = game:GetService("Players"):GetPlayerFromCharacter(hit.Parent)
	if not player then return end
	if not game:GetService("BadgeService"):UserHasBadgeAsync(player.UserId, badge_id) then
		local s, r = pcall(function()
			hit.Parent:WaitForChild("Humanoid").Health = 0
		end)
	end
end)
```

## Basic Radgoll After Death
```lua
game:GetService("Players").PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function(character)
        character:WaitForChild("Humanoid").BreakJointsOnDeath = false
        character:WaitForChild("Humanoid").Died:Connect(function()
            for i, v in pairs(character:GetDescendants()) do
                if v:IsA("Motor6D") then
                    local attachment0, attachment1 = Instance.new("Attachment"), Instance.new("Attachment")

                    attachment0.CFrame = v.C0
                    attachment1.CFrame = v.C1

                    attachment0.Parent = v.Part0
                    attachment1.Parent = v.Part1

                    local constraint = Instance.new("BallSocketConstraint")

                    constraint.Attachment0 = attachment0
                    constraint.Attachment1 = attachment1

                    constraint.Parent = v.Part0

                    v:Destroy()
                end
            end

            character:WaitForChild("HumanoidRootPart").CanCollide = false

        end)
    end)
end)
```
## Basic lua Ban System
```lua
local bannedPlayers = {"bannedusername1", "writeUsernamehere"} -- banned users here you can always add more if you would like!

game:GetService("Players").PlayerAdded:Connect(function(player)
    for i, v in pairs(bannedPlayers) do
        if player.Name == v then
            player:Kick("You are banned!") --your ban message here
        end
    end
end)
```
## Change Camera FOV
```lua
game:GetService("Workspace"):WaitForChild("Camera").FieldOfView = 120 -- change the FOV here
```

## Click to toggle visibility on an block (you need to have an click detector inside the part)
```lua
script.Parent:WaitForChild("ClickDetector").MouseClick:Connect(function(player)
	if script.Parent.Transparency ~= 1 then
		script.Parent.Transparency = 1
	else
		script.Parent.Transparency = 0
	end
end)
```
## Block changes color every second random colors
```lua
while true do
    script.Parent.BrickColor = BrickColor.Random()
    wait(1)
end
```
## When touched damages the humanoid (Player or NPC)
```lua
script.Parent.Touched:Connect(function(hit)
	if hit.Parent:FindFirstChildOfClass("Humanoid") then
		hit.Parent:FindFirstChildOfClass("Humanoid"):TakeDamage(2) -- how much damage he will take
	end
end)
```
## Day-Night Cycle
```lua
while true do
    game:GetService("Lighting").ClockTime = game:GetService("Lighting").ClockTime + 0.1
    wait(60)
end
```
## Delete everytool from everyplayers inventory
```lua
for _, player in pairs(game:GetService("Players"):GetPlayers()) do
    for i, tool in pairs(player:WaitForChild("Backpack"):GetChildren()) do
        if tool:IsA("Tool") then tool:Destroy() end
    end
end
```
## Developer Title  (Modified to display the Developer Text)
```lua
local developers = {"Roblox", "RandomUsername1"} -- Put usernames in here

game:GetService("Players").PlayerAdded:Connect(function(player)
	local valid = false
	for _, dev in ipairs(developers) do
		if string.lower(dev) == string.lower(player.Name) then
			valid = true
			break -- No need to keep checking once a match is found
		end
	end
	if not valid then return end

	player.CharacterAdded:Connect(function(character)
		local head = character:WaitForChild("Head", 5) -- Timeout to avoid indefinite waiting
		if head then
			-- Create a new BillboardGui
			local billboard = Instance.new("BillboardGui")
			billboard.Name = "DeveloperTag"
			billboard.Size = UDim2.new(0, 100, 0, 50) -- Adjust size as needed
			billboard.StudsOffset = Vector3.new(0, 2, 0) -- Position above the head
			billboard.AlwaysOnTop = true -- Ensures it renders on top of other 3D elements
			billboard.Parent = head

			-- Create a new TextLabel inside the BillboardGui
			local textLabel = Instance.new("TextLabel")
			textLabel.Size = UDim2.new(1, 0, 1, 0) -- Takes up the full size of the BillboardGui
			textLabel.BackgroundTransparency = 1 -- Makes background transparent
			textLabel.Text = "Developer" -- Customize this text
			textLabel.TextColor3 = Color3.new(1, 1, 1) -- White text
			textLabel.TextScaled = true -- Automatically scales the text to fit the label
			textLabel.Font = Enum.Font.SourceSansBold -- Customize font
			textLabel.Parent = billboard
		else
			warn("Head not found for " .. player.Name)
		end
	end)
end)
```
## Disable Chat
```lua
game:GetService("StarterGui"):SetCoreGuiEnabled(Enum.CoreGuiType.Chat, false)
```
## First Person Lock
```lua
game:GetService("Players").LocalPlayer.CameraMode = Enum.CameraMode.LockFirstPerson
```
## Gamepass Door
```lua
local gamepass_id = 0
script.Parent.Touched:Connect(function(hit)
local player = game:GetService("Players"):GetPlayerFromCharacter(hit.Parent)
if not player then return end
if not game:GetService("MarketplaceService"):UserOwnsGamePassAsync(player.UserId, gamepass_id) then
local s, r = pcall(function()
hit.Parent:WaitForChild("Humanoid").Health = 0
end)
end
end)
```
## Group Team (Puts Players on a team if they are in  a group)
```lua
local groupId = 00000 --Enter your group ID
local team = game:GetService("Teams"):FindFirstChild("Admin") --Change to your team (You can replace Admin with your teams name and it should autofill)

game:GetService("Players").PlayerAdded:Connect(function(player)
	if player:IsInGroup(groupId) then
		player.Team = team
	end
end)
```
## Player Respawns Instantly after death
```lua
game:GetService("Players").PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function(character)
        character:WaitForChild("Humanoid").Died:Connect(function()
            player:LoadCharacter()
        end)
    end)
end)
```
## Kicks Player From game after dying
```lua
game:GetService("Players").PlayerAdded:Connect(function(player)
	player.CharacterAdded:Connect(function(character)
		character:WaitForChild("Humanoid").Died:Connect(function()
			player:Kick("You died!") -- your kick message
		end)
	end)
end)
```
## Kill Brick
```lua
script.Parent.Touched:Connect(function(hit)
    if hit.Parent:FindFirstChildOfClass("Humanoid") then
        hit.Parent:FindFirstChildOfClass("Humanoid").Health = 0
    end
end)
```
## Landmine (Modified to kill you)
```lua
local respawnTime = 30 -- How many seconds before spawning again

script.Parent.Touched:Connect(function(hit)
	-- Check if the object hit is part of a player's character
	local humanoid = hit.Parent:FindFirstChildOfClass("Humanoid")
	if humanoid then
		humanoid.Health = 0 -- Kills the player
	end
	
	-- If the object is already transparent, return
	if script.Parent.Transparency == 1 then return end
	
	-- Set the object transparency to 1 and create an explosion
	script.Parent.Transparency = 1
	local explosion = Instance.new("Explosion", script.Parent)
	explosion.Position = script.Parent.Position
	game:GetService("Debris"):AddItem(explosion, 2)
	
	-- Wait for respawn time and reset transparency
	wait(respawnTime)
	script.Parent.Transparency = 0
end)
```
## Phrase Teleport
```lua
local phrase = "secret game" -- Set to your term that will cause them to be teleported
local placeId = 00000 -- Set to your place ID

game:GetService("Players").PlayerAdded:Connect(function(player)
	player.Chatted:Connect(function(msg)
		if string.lower(msg) == string.lower(phrase) then game:GetService("TeleportService"):Teleport(placeId, player) end
	end)
end)
```
/respawn Command
```lua
game:GetService("Players").PlayerAdded:Connect(function(player)
    player.Chatted:Connect(function(msg)
        if string.lower(msg) == "/respawn" then
            player:LoadCharacter()
        end
    end)
end)
```
## Press Shift To Sprint
```lua
local UIS = game:GetService("UserInputService")
local Humanoid = script.Parent:WaitForChild("Humanoid")
local runningSpeed = 25
local walkingSpeed = 16
UIS.InputBegan:Connect(function(key)
    if key.KeyCode == Enum.KeyCode.LeftShift then
        Humanoid.WalkSpeed = runningSpeed
    end
end)

UIS.InputEnded:Connect(function(key)
    if key.KeyCode == Enum.KeyCode.LeftShift then
        Humanoid.WalkSpeed = walkingSpeed
    end
end)
```
## Slippery Part (Makes the character Sit when touched)
```lua
script.Parent.Touched:Connect(function(hit)
    if hit.Parent:FindFirstChildOfClass("Humanoid") then
        hit.Parent:FindFirstChildOfClass("Humanoid").Sit = true
    end
end)
```
## Spinning Part
```lua
while true do
script.Parent.CFrame = script.Parent.CFrame * CFrame.new(0, 0, 0) * CFrame.fromEulerAnglesXYZ(0, 0.1, 0)
wait()
end
```
## Welcome Badge
```lua
local service = game:GetService("BadgeService")
local badgeID = 0 -- BadgeID Here

game:GetService("Players").PlayerAdded:Connect(function(player)
    if not service:UserHasBadgeAsync(player.UserId, badgeID) then
        service:AwardBadge(player.UserId, badgeID)
    end
end)
```
## Whitelist
```lua
local allowedPlayers = {"Username1", "Username2", "Username3"} -- add more users if needed

game:GetService("Players").PlayerAdded:Connect(function(player)
    local allowed = false

    for i, name in ipairs(allowedPlayers) do
        if name == player.Name then allowed = true end
    end

    if not allowed then player:Kick("You are not allowed in this game!") end -- your kick message
end)
```
## The End Of the list for now. Im not gonna be adding the Tier Scripts Or Modules because they cost robux In ScriptBlox So im gonna make my own!
