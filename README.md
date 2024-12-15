# Boris GUI
This is a Roblox script that includes:
- Setting players on fire
- Turning avatars red
- Playing a random song
- Wall walking
- Red sky and black textures

## How to Use
Copy the script from the repository and paste it into your Roblox game.

## Author
Created by 88HATRED.


--[[ 
    Boris GUI Script
    FE-Compatible Features:
    - Catch fire
    - Turn avatars red
    - Play random song
    - Wall walking
    - Backdoor finder
    - Red sky and black textures
    - Black and red-themed GUI
]]

local Players = game:GetService("Players")
local Lighting = game:GetService("Lighting")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")
local SoundService = game:GetService("SoundService")

-- Prevent multiple GUIs
if game.Players.LocalPlayer:FindFirstChild("BorisGUI") then
    return
end

-- GUI Creation
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "BorisGUI"
ScreenGui.Parent = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0.3, 0, 0.5, 0)
Frame.Position = UDim2.new(0.35, 0, 0.25, 0)
Frame.BackgroundColor3 = Color3.new(0, 0, 0)
Frame.BorderColor3 = Color3.new(1, 0, 0)
Frame.Parent = ScreenGui

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0.2, 0)
Title.Text = "BORIS GUI"
Title.TextColor3 = Color3.new(1, 0, 0)
Title.BackgroundTransparency = 1
Title.Font = Enum.Font.SciFi
Title.TextScaled = true
Title.Parent = Frame

local ButtonFire = Instance.new("TextButton")
ButtonFire.Text = "Set Players on Fire"
ButtonFire.Size = UDim2.new(1, 0, 0.15, 0)
ButtonFire.Position = UDim2.new(0, 0, 0.2, 0)
ButtonFire.BackgroundColor3 = Color3.new(0.2, 0, 0)
ButtonFire.TextColor3 = Color3.new(1, 0, 0)
ButtonFire.Parent = Frame

local ButtonRed = Instance.new("TextButton")
ButtonRed.Text = "Turn Players Red"
ButtonRed.Size = UDim2.new(1, 0, 0.15, 0)
ButtonRed.Position = UDim2.new(0, 0, 0.4, 0)
ButtonRed.BackgroundColor3 = Color3.new(0.2, 0, 0)
ButtonRed.TextColor3 = Color3.new(1, 0, 0)
ButtonRed.Parent = Frame

local ButtonMusic = Instance.new("TextButton")
ButtonMusic.Text = "Play Random Song"
ButtonMusic.Size = UDim2.new(1, 0, 0.15, 0)
ButtonMusic.Position = UDim2.new(0, 0, 0.6, 0)
ButtonMusic.BackgroundColor3 = Color3.new(0.2, 0, 0)
ButtonMusic.TextColor3 = Color3.new(1, 0, 0)
ButtonMusic.Parent = Frame

local ButtonWallWalk = Instance.new("TextButton")
ButtonWallWalk.Text = "Enable Wall Walk"
ButtonWallWalk.Size = UDim2.new(1, 0, 0.15, 0)
ButtonWallWalk.Position = UDim2.new(0, 0, 0.8, 0)
ButtonWallWalk.BackgroundColor3 = Color3.new(0.2, 0, 0)
ButtonWallWalk.TextColor3 = Color3.new(1, 0, 0)
ButtonWallWalk.Parent = Frame

-- Features
local function setPlayersOnFire()
    for _, player in ipairs(Players:GetPlayers()) do
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            -- Remove existing fire before adding new one
            local existingFire = player.Character.HumanoidRootPart:FindFirstChildOfClass("Fire")
            if existingFire then
                existingFire:Destroy()
            end
            
            local fire = Instance.new("Fire")
            fire.Size = 5  -- Adjust fire size
            fire.Heat = 9  -- Adjust heat intensity
            fire.Parent = player.Character.HumanoidRootPart
        end
    end
end

local function turnPlayersRed()
    for _, player in ipairs(Players:GetPlayers()) do
        if player.Character then
            for _, part in ipairs(player.Character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.Color = Color3.fromRGB(255, 0, 0)  -- More precise color setting
                end
            end
        end
    end
end

local function playRandomSong()
    local songs = {
        "rbxassetid://183763515", 
        "rbxassetid://184785309",
        "rbxassetid://903417231"
    }
    
    -- Stop any currently playing sounds
    for _, sound in ipairs(SoundService:GetChildren()) do
        if sound:IsA("Sound") then
            sound:Stop()
            sound:Destroy()
        end
    end
    
    local sound = Instance.new("Sound")
    sound.SoundId = songs[math.random(1, #songs)]
    sound.Volume = 0.5  -- Reduced volume to prevent ear damage
    sound.Looped = false
    sound.Parent = SoundService
    sound:Play()
end

local function enableWallWalking()
    local function onStepped()
        local player = Players.LocalPlayer
        if player and player.Character and player.Character:FindFirstChild("Humanoid") then
            local humanoid = player.Character.Humanoid
            humanoid:ChangeState(Enum.HumanoidStateType.Climbing)
        end
    end
    RunService.Stepped:Connect(onStepped)
end

local function setRedSkyAndBlackTextures()
    -- Remove existing sky
    for _, skybox in ipairs(Lighting:GetChildren()) do
        if skybox:IsA("Sky") then
            skybox:Destroy()
        end
    end

    local sky = Instance.new("Sky")
    sky.SkyboxBk = "rbxassetid://0"
    sky.SkyboxDn = "rbxassetid://0"
    sky.SkyboxFt = "rbxassetid://0"
    sky.SkyboxLf = "rbxassetid://0"
    sky.SkyboxRt = "rbxassetid://0"
    sky.SkyboxUp = "rbxassetid://0"
    sky.Parent = Lighting
    
    Lighting.Ambient = Color3.fromRGB(26, 0, 0)  -- More subtle red
    
    for _, obj in ipairs(Workspace:GetDescendants()) do
        if obj:IsA("BasePart") and not obj:IsA("Terrain") then
            obj.Color = Color3.new(0, 0, 0)
        end
    end
end

local function findBackdoors()
    warn("Backdoor Scanner Started")
    local backdoorsFound = 0
    for _, v in ipairs(Workspace:GetDescendants()) do
        if (v:IsA("Script") or v:IsA("LocalScript")) and not v:IsA("ModuleScript") then
            local source = v.Source
            if string.find(source, "require") or string.find(source, "getgenv") or 
               string.find(source, "setreadonly") or string.find(source, "hookfunction") then
                warn("Possible Backdoor Found in: " .. v:GetFullName())
                backdoorsFound = backdoorsFound + 1
            end
        end
    end
    
    if backdoorsFound == 0 then
        print("No suspicious backdoors detected.")
    end
end

-- Button Connections
ButtonFire.MouseButton1Click:Connect(setPlayersOnFire)
ButtonRed.MouseButton1Click:Connect(turnPlayersRed)
ButtonMusic.MouseButton1Click:Connect(playRandomSong)
ButtonWallWalk.MouseButton1Click:Connect(enableWallWalking)

-- Automatic Background Changes
pcall(setRedSkyAndBlackTextures)
pcall(findBackdoors)
