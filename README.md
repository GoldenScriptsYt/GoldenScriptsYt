-- Load the Orion UI Library
local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()

-- Create the main window
local Window = OrionLib:MakeWindow({Name = "Kaizen (GoldenScripts Battlegrounds)", HidePremium = false, IntroEnabled = false})

-- Variables for toggle and input options
local InfAwaken = false
local NoCooldown = false
local FreeEarlyAccess = false
local JumpPower = 200
local Speed = 16

-- Players list for admin kick
local players = game:GetService("Players")
local kickPlayerDropdown -- reference to the dropdown

-- Main tab
local MainTab = Window:MakeTab({
    Name = "Main",
    Icon = "rbxassetid://4483345998", 
    PremiumOnly = false
})

-- Infinite Awaken toggle
MainTab:AddToggle({
    Name = "Infinite Awaken",
    Default = false,
    Callback = function(value)
        InfAwaken = value
        game.Players.LocalPlayer.InfAwaken.Value = value
    end    
})

-- No Cooldown toggle
MainTab:AddToggle({
    Name = "No Cooldown",
    Default = false,
    Callback = function(value)
        NoCooldown = value
        game.Players.LocalPlayer.NoCD.Value = value
    end    
})

-- Free Early Access toggle
MainTab:AddToggle({
    Name = "Free Early Access",
    Default = false,
    Callback = function(value)
        FreeEarlyAccess = value
        game.Players.LocalPlayer.EarlyAccess.Value = value
    end    
})

-- Jump Power input
MainTab:AddTextbox({
    Name = "Jump Power",
    Default = "200",
    TextDisappear = true,
    Callback = function(value)
        JumpPower = tonumber(value)
        if JumpPower then
            game.Players.LocalPlayer.JumpPower.Value = JumpPower
        else
            warn("Please enter a valid number for Jump Power")
        end
    end    
})

-- Speed input
MainTab:AddTextbox({
    Name = "Speed",
    Default = "16",
    TextDisappear = true,
    Callback = function(value)
        Speed = tonumber(value)
        if Speed then
            game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = Speed
        else
            warn("Please enter a valid number for Speed")
        end
    end    
})

-- Function to update the dropdown with the list of players
local function updatePlayersDropdown()
    local playerNames = {}
    for _, player in pairs(players:GetPlayers()) do
        table.insert(playerNames, player.Name)
    end

    -- Update the dropdown options dynamically
    kickPlayerDropdown:Refresh(playerNames, true)
end

-- Admin Kick dropdown
kickPlayerDropdown = MainTab:AddDropdown({
    Name = "Kick Player",
    Default = "",
    Options = {}, -- Will be updated dynamically
    Callback = function(selectedPlayer)
        local playerToKick = players:FindFirstChild(selectedPlayer)
        if playerToKick then
            playerToKick:Kick("YouHaveBeenKickedByGoldenScriptLol")
        else
            warn("Player not found.")
        end
    end
})

-- Update dropdown when a player joins or leaves
players.PlayerAdded:Connect(updatePlayersDropdown)
players.PlayerRemoving:Connect(updatePlayersDropdown)

-- Initialize the UI
OrionLib:Init()

-- Call this to populate the dropdown at the start
updatePlayersDropdown()
