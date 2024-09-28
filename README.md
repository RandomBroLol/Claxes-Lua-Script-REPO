# Claxes-Lua-Script-REPO


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
