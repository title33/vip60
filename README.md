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
    Skil = Window:AddTab({ Title = "Skil", Icon = "battery-charging" }),
    TP = Window:AddTab({ Title = "Tp", Icon = "chevrons-right" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}

local Options = Fluent.Options


local Weaponlist = {}
local Weapon = nil

for i,v in pairs(game:GetService("Players").LocalPlayer.Backpack:GetChildren()) do
    table.insert(Weaponlist,v.Name)
end



local Dropdown = Tabs.General:AddDropdown("Boss", {
    Title = "Boss",
    Values = {"Shadow", "Gojo", "Kashimo", "Sukuna", "Snow Bandit Leader", "Shank", "Monkey King", "Sand Man", "Bomb Man", "Bandit Leader", "Artoria", "Uraume", "Gojo [Unleashed]", "Sukuna [Half Power]", "Rimuru", "Killua"},
    Multi = false,
    Default = 1,
})

Dropdown:SetValue("None")

Dropdown:OnChanged(function(Value)
    SelectedBoss = Value
end)

local Toggle = Tabs.General:AddToggle("MyToggle", {Title = "Auto Farm Boss", Default = false })

Toggle:OnChanged(function(Value)
    _G.AutoFarm = Value

    while _G.AutoFarm do
        wait()
        if SelectedBoss ~= "None" then
            local BossCFrame = game:GetService("Workspace").Lives[SelectedBoss].HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = BossCFrame
        end
    end
end)

Options.MyToggle:SetValue(false)


local Dropdown = Tabs.General:AddDropdown("Select weapon", {
    Title = "Select weapon",
    Values = Weaponlist,
    Multi = false,
    Default = 1,
})

Dropdown:SetValue("None")

Dropdown:OnChanged(function(currentOption)
     Weapon = currentOption
end)

    local Toggle = Tabs.General:AddToggle("MyToggle", {Title = "Auto Equip", Default = false })

    Toggle:OnChanged(function(a)
       AutoEquiped = a
    end)

    Options.MyToggle:SetValue(false)

spawn(function()
while wait() do
if AutoEquiped then
pcall(function()
game.Players.LocalPlayer.Character.Humanoid:EquipTool(game:GetService("Players").LocalPlayer.Backpack:FindFirstChild(Weapon))
end)
end
end
end)

    local Toggle = Tabs.General:AddToggle("MyToggle", {Title = "Auto Attack", Default = false })

    Toggle:OnChanged(function(o)
       Autoattack = o
    end)

    Options.MyToggle:SetValue(false)

spawn(function()
    while wait() do
        if Autoattack then
            game:GetService("RunService").RenderStepped:Connect(function()
                pcall(function()
                    game:GetService('VirtualUser'):CaptureController()
                    game:GetService('VirtualUser'):Button1Down(Vector2.new(1280, 672))
                end)
            end)
        end
    end
end)






    local Toggle = Tabs.Skil:AddToggle("Auto Skil Z", {Title = "Auto Skil Z", Default = false })

    Toggle:OnChanged(function(G)
       _G.AutoZ = G

spawn(function()
while wait(.1) do
    pcall(function()
if _G.AutoZ then
game:GetService("VirtualInputManager"):SendKeyEvent(true,"Z",false,game)
                end
        end)
   end
end)

    end)

    Options.MyToggle:SetValue(false)
    


    local Toggle = Tabs.Skil:AddToggle("Auto Skil X", {Title = "Auto Skil X", Default = false })

    Toggle:OnChanged(function(G)
       _G.AutoX = G

spawn(function()
while wait(.1) do
    pcall(function()
if _G.AutoX then
game:GetService("VirtualInputManager"):SendKeyEvent(true,"X",false,game)
                end
        end)
   end
end)

    end)

    Options.MyToggle:SetValue(false)


    local Toggle = Tabs.Skil:AddToggle("Auto Skil C", {Title = "Auto Skil C", Default = false })

    Toggle:OnChanged(function(G)
       _G.AutoC = G

spawn(function()
while wait(.1) do
    pcall(function()
if _G.AutoC then
game:GetService("VirtualInputManager"):SendKeyEvent(true,"C",false,game)
                end
        end)
   end
end)

    end)

    Options.MyToggle:SetValue(false)
 


    local Toggle = Tabs.Skil:AddToggle("Auto Skil V", {Title = "Auto Skil V", Default = false })

    Toggle:OnChanged(function(G)
       _G.AutoV = G

spawn(function()
while wait(.1) do
    pcall(function()
if _G.AutoV then
game:GetService("VirtualInputManager"):SendKeyEvent(true,"V",false,game)
                end
        end)
   end
end)

    end)

    Options.MyToggle:SetValue(false)

players = {}
 
for i,v in pairs(game:GetService("Players"):GetChildren()) do
   table.insert(players,v.Name)
end


local Dropdown = Tabs.TP:AddDropdown("Player", {
    Title = "Player",
    Values = players,
    Multi = false,
    Default = 1,
})

Dropdown:SetValue("None")

Dropdown:OnChanged(function(V)
    Select = v
end)

    Tabs.TP:AddButton({
        Title = "Refresh",
        Description = "Refresh",
        Callback = function()
            Window:Dialog({
                Title = "Refresh",
                Content = "Refresh",
                Buttons = {
                    {
                        Title = "Confirm",
                        Callback = function()
                               table.clear(players)
for i,v in pairs(game:GetService("Players"):GetChildren()) do
   table.insert(players,v.Name)
end
                        end
                    },
                    {
                        Title = "Cancel",
                        Callback = function()
                            print("no")
                        end
                    }
                }
            })
        end
    })

    Tabs.TP:AddButton({
        Title = "Teleport to Player",
        Description = "Teleport to the selected player",
        Callback = function()
            Window:Dialog({
                Title = "Teleport to Player",
                Content = "Teleport to the selected player",
                Buttons = {
                    {
                        Title = "Confirm",
                        Callback = function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game.Players[Select].Character.HumanoidRootPart.CFrame
                        end
                    },
                    {
                        Title = "Cancel",
                        Callback = function()
                            print("no")
                        end
                    }
                }
            })
        end
    })

local Dropdown = Tabs.TP:AddDropdown("Dropdown", {
    Title = "Dropdown",
    Values = {},  
    Multi = false,
    Default = 1,
})


for _, npc in pairs(game:GetService("Workspace").NPC:GetChildren()) do
    table.insert(Dropdown.Values, npc.Name)
end

Dropdown:SetValue("None")

Dropdown:OnChanged(function(Value)
    print("Dropdown changed:", Value)
end)

Tabs.TP:AddButton({
    Title = "Teleport to NPC",
    Description = "Teleport to the selected NPC",
    Callback = function()
        local selectedNPCName = Dropdown.Values[Dropdown.Default]
        if selectedNPCName and selectedNPCName ~= "None" then
            
            local selectedNPC = game:GetService("Workspace").NPC:FindFirstChild(selectedNPCName)
            if selectedNPC then
                game.Players.LocalPlayer.Character:MoveTo(selectedNPC.Position)
                print("Teleported to NPC:", selectedNPCName)
            else
                print("NPC not found.")
            end
        else
            print("No NPC selected.")
        end
    end
})




-- Addons:
-- SaveManager (Allows you to have a configuration system)
-- InterfaceManager (Allows you to have a interface managment system)

-- Hand the library over to our managers
SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)

-- Ignore keys that are used by ThemeManager.
-- (we dont want configs to save themes, do we?)
SaveManager:IgnoreThemeSettings()

-- You can add indexes of elements the save manager should ignore
SaveManager:SetIgnoreIndexes({})

-- use case for doing it this way:
-- a script hub could have themes in a global folder
-- and game configs in a separate folder per game
InterfaceManager:SetFolder("FluentScriptHub")
SaveManager:SetFolder("FluentScriptHub/specific-game")

InterfaceManager:BuildInterfaceSection(Tabs.Settings)
SaveManager:BuildConfigSection(Tabs.Settings)


Window:SelectTab(1)


-- You can use the SaveManager:LoadAutoloadConfig() to load a config
-- which has been marked to be one that auto loads!
SaveManager:LoadAutoloadConfig()
