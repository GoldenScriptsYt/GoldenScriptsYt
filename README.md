-- Load the Orion UI Library
local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()

-- Create the main window
local Window = OrionLib:MakeWindow({Name = "Kaizen (GoldenScripts Battlegrounds)", HidePremium = false, IntroEnabled = false})

-- Variables for toggle and input options
local InfAwaken = false
local NoCooldown = false
local FreeEarlyAccess = false
local AdminPanel = false -- New admin panel toggle
local JumpPower = 200
local Speed = 16

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

-- Admin Panel toggle to enable/disable the "Npc Quest" GUI
MainTab:AddToggle({
    Name = "Admin Panel",
    Default = false,
    Callback = function(value)
        AdminPanel = value
        game:GetService("Players").LocalPlayer.PlayerGui["Npc Quest"].Enabled = value
    end
})

-- Initialize the UI
OrionLib:Init()
