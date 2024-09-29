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

## Badge Door (Modified to work)
```lua
local BadgeService = game:GetService("BadgeService")
local door = script.Parent  -- The door part
local badgeId = 12345678  -- Replace with your Badge ID

-- Function to check if a player has the badge
local function playerHasBadge(player)
    local success, hasBadge = pcall(function()
        return BadgeService:UserHasBadgeAsync(player.UserId, badgeId)
    end)

    if success then
        return hasBadge
    else
        warn("Could not check badge: ", hasBadge)
        return false
    end
end

-- Function to handle when a player touches the door
local function onTouched(hit)
    local player = game.Players:GetPlayerFromCharacter(hit.Parent)
    
    if player then
        if playerHasBadge(player) then
            -- Open the door (transparency and can collide off)
            door.Transparency = 0.5
            door.CanCollide = false
            wait(3)  -- Keep door open for 3 seconds
            door.Transparency = 0
            door.CanCollide = true
        else
            -- Player doesn't have the badge, so kill them
            local character = player.Character
            if character and character:FindFirstChild("Humanoid") then
                character.Humanoid.Health = 0  -- Kills the player
            end
        end
    end
end

-- Connect the touched event to the function
door.Touched:Connect(onTouched)
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
## Gamepass Door (Modified)
```lua
local door = script.Parent
local playerService = game:GetService("Players")
local doorId = 123456789 -- Replace with your Game Pass ID

-- Function to check if the player owns the game pass
local function playerHasGamePass(player)
    local success, result = pcall(function()
        return player:HasPass(doorId)
    end)
    return success and result
end

-- Function to handle player touch
local function onTouch(other)
    local player = playerService:GetPlayerFromCharacter(other.Parent)
    if player and playerHasGamePass(player) then
        door.Transparency = 0.5 -- Makes the door transparent
        door.CanCollide = false   -- Allows players to pass through
        wait(3) -- Time the door stays open
        door.Transparency = 0 -- Resets transparency
        door.CanCollide = true  -- Restores collision
    else
        other.Parent:BreakJoints() -- Kills the player if they don't own the Game Pass
    end
end

door.Touched:Connect(onTouch)
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
## Landmine (Modified to Actually be useful)
```lua
local respawnTime = 30 -- How many seconds before spawning again
local isActive = true -- Flag to track if the landmine is active

script.Parent.Anchored = true -- Ensure the landmine is anchored

-- Create the sound instance
local sound = Instance.new("Sound")
sound.SoundId = "rbxassetid://423041300" -- Set the sound asset ID
sound.Parent = script.Parent -- Parent the sound to the landmine part

script.Parent.Touched:Connect(function(hit)
	if not isActive then return end -- If not active, do nothing

	-- Check if the object hit is part of a player's character
	local humanoid = hit.Parent:FindFirstChildOfClass("Humanoid")
	if humanoid then
		humanoid.Health = 0 -- Kills the player

		-- Play the sound
		sound:Play() -- Play the sound when hit

		-- Set the object transparency to 1 and make it non-collidable and non-touchable
		script.Parent.Transparency = 1
		script.Parent.CanCollide = false -- Disable collisions

		local explosion = Instance.new("Explosion")
		explosion.Position = script.Parent.Position
		explosion.Parent = game.Workspace -- Parent the explosion to Workspace
		explosion.BlastRadius = 10 -- Set the explosion radius (adjust as needed)
		explosion.BlastPressure = 5000 -- Set the explosion pressure (adjust as needed)
		explosion.ExplosionType = Enum.ExplosionType.NoCraters -- Prevent craters if desired

		-- Add the explosion to Debris for cleanup
		game:GetService("Debris"):AddItem(explosion, 2)

		-- Set the landmine to inactive
		isActive = false

		-- Wait for respawn time and reset transparency and collision
		wait(respawnTime)
		script.Parent.Transparency = 0
		script.Parent.CanCollide = true -- Enable collisions again
		isActive = true -- Set the landmine back to active
	end
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
