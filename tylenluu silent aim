local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()
local Window = OrionLib:MakeWindow({Name = "Universal Mobile Script", HidePremium = false, SaveConfig = true, ConfigFolder = "OrionMobile"})

-- // Variables
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local RunService = game:GetService("RunService")

-- // State Management
getgenv().SilentAim = false
getgenv().TeamCheck = false
getgenv().FOVRadius = 100
getgenv().FOVColor = Color3.fromRGB(255, 255, 255)
getgenv().ESPEnabled = false
getgenv().ESPColor = Color3.fromRGB(255, 0, 0)

-- // Drawing FOV Circle
local FOVCircle = Drawing.new("Circle")
FOVCircle.Thickness = 2
FOVCircle.NumSides = 60
FOVCircle.Filled = false
FOVCircle.Transparency = 1

-- // Helper Functions
local function GetCenter()
    return Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
end

local function GetClosestPlayer()
    local Target = nil
    local Distance = getgenv().FOVRadius
    local Center = GetCenter()

    for _, v in pairs(Players:GetPlayers()) do
        if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
            if getgenv().TeamCheck and v.Team == LocalPlayer.Team then continue end
            if v.Character:FindFirstChild("Humanoid") and v.Character.Humanoid.Health <= 0 then continue end

            local ScreenPos, OnScreen = Camera:WorldToViewportPoint(v.Character.HumanoidRootPart.Position)
            if OnScreen then
                local dist = (Center - Vector2.new(ScreenPos.X, ScreenPos.Y)).Magnitude
                if dist < Distance then
                    Target = v
                    Distance = dist
                end
            end
        end
    end
    return Target
end

-- // Main Tabs
local AimTab = Window:MakeTab({Name = "Combat", Icon = "rbxassetid://4483345998"})
local VisualTab = Window:MakeTab({Name = "Visuals", Icon = "rbxassetid://4483345998"})

-- // Combat Section
AimTab:AddSection({Name = "Silent Aim"})

AimTab:AddToggle({
	Name = "Enable Silent Aim",
	Default = false,
	Callback = function(Value) getgenv().SilentAim = Value end    
})

AimTab:AddToggle({
	Name = "Team Check",
	Default = false,
	Callback = function(Value) getgenv().TeamCheck = Value end    
})

AimTab:AddSlider({
	Name = "FOV Size",
	Min = 10, Max = 500, Default = 100,
	Color = Color3.fromRGB(255,255,255),
	Increment = 5, ValueName = "Pixels",
	Callback = function(Value) getgenv().FOVRadius = Value end    
})

AimTab:AddColorpicker({
	Name = "FOV Color",
	Default = Color3.fromRGB(255, 255, 255),
	Callback = function(Value) getgenv().FOVColor = Value end	  
})

-- // Visuals Section
VisualTab:AddSection({Name = "ESP Settings"})

VisualTab:AddToggle({
	Name = "Enable ESP",
	Default = false,
	Callback = function(Value) getgenv().ESPEnabled = Value end    
})

VisualTab:AddColorpicker({
	Name = "Enemy ESP Color",
	Default = Color3.fromRGB(255, 0, 0),
	Callback = function(Value) getgenv().ESPColor = Value end	  
})

-- // RunService Loop
RunService.RenderStepped:Connect(function()
    -- FOV Update
    FOVCircle.Visible = true -- Set to false if you want it hidden
    FOVCircle.Radius = getgenv().FOVRadius
    FOVCircle.Color = getgenv().FOVColor
    FOVCircle.Position = GetCenter()

    -- ESP & Aim Logic
    for _, v in pairs(Players:GetPlayers()) do
        if v ~= LocalPlayer and v.Character then
            local Highlight = v.Character:FindFirstChild("ESPHighlight")
            
            if getgenv().ESPEnabled then
                if not Highlight then
                    Highlight = Instance.new("Highlight", v.Character)
                    Highlight.Name = "ESPHighlight"
                end
                -- Color teammates green if teamcheck is on
                Highlight.FillColor = (getgenv().TeamCheck and v.Team == LocalPlayer.Team) and Color3.new(0,1,0) or getgenv().ESPColor
            else
                if Highlight then Highlight:Destroy() end
            end
        end
    end
end)

OrionLib:Init()
