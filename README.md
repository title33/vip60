local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/title33/SaveManager/main/README.md"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "Xylo Hub",
    SubTitle = "by Sky",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true, -- The blur may be detectable, setting this to false disables blur entirely
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl -- Used when theres no MinimizeKeybind
})

--Fluent provides Lucide Icons https://lucide.dev/icons/ for the tabs, icons are optional
local Tabs = {
    General = Window:AddTab({ Title = "General", Icon = "home" }),
    Skil = Window:AddTab({ Title = "Skil", Icon = "database-zap" }),
    TP = Window:AddTab({ Title = "Tp", Icon = "fast-forward" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}

local Options = Fluent.Options

MONS = {}

local Weaponlist = {}
local Weapon = nil



for i,v in pairs(game:GetService("Players").LocalPlayer.Backpack:GetChildren()) do
    table.insert(Weaponlist,v.Name)
end

for i, v in pairs(game.Workspace.Lives:GetChildren()) do
    if not game.Players:FindFirstChild(v.Name) then
        MONS[v.Name] = true
    end
end


    local Dropdown = Tabs.General:AddDropdown("Select weapon", {
        Title = "Select weapon",
        Values = ,
        Multi = false,
        Default = 1,
    })

    Dropdown:SetValue("None")

    Dropdown:OnChanged(function(currentOption)
         Weapon = currentOption
    end)


local Toggle = Tabs.General:AddToggle("", {Title = "Auto Auto Equip", Default = false })

Toggle:OnChanged(function(e)
AutoEquiped = a
end)




local Dropdown = Tabs.General:AddDropdown("SelectWeapon", {
    Title = "SelectWeapon",
    Values = MONS,
    Multi = false,
    Default = 1,
})

Dropdown:SetValue("None")

Dropdown:OnChanged(function(currentOption)
    Select = currentOption
end)

local Toggle = Tabs.General:AddToggle("", {Title = "Auto Farm Mon", Default = false })

Toggle:OnChanged(function(e)
    _G.AutoFarm = e
    while _G.AutoFarm do
        wait()
        if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local target = game.Workspace.Lives[Select]
            if target and target:FindFirstChild("HumanoidRootPart") then
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = target.HumanoidRootPart.CFrame * CFrame.new(0, 0, 10)
            end
        end
    end
end)




local Button = Tabs.General:AddButton({
    Title = "Farm",
    Description = "Start auto farming",
    Callback = function()
        _G.AutoFarm = true
    end
})

local Button = Tabs.General:AddButton({
    Title = "Stop",
    Description = "Stop auto farming",
    Callback = function()
        _G.AutoFarm = false
    end
})

spawn(function()
while wait() do
if AutoEquiped then
pcall(function()
game.Players.LocalPlayer.Character.Humanoid:EquipTool(game:GetService("Players").LocalPlayer.Backpack:FindFirstChild(Weapon))
end)
end
end
end)


Options.MyToggle:SetValue(false)

InterfaceManager:BuildInterfaceSection(Tabs.Settings)
SaveManager:BuildConfigSection(Tabs.Settings)

Window:SelectTab(1)

-- You can use the SaveManager:LoadAutoloadConfig() to load a config
-- which has been marked to be one that auto loads!
SaveManager:LoadAutoloadConfig()

