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


    Tabs.General:AddButton({
        Title = "Button",
        Description = "Very important button",
        Callback = function()
            Window:Dialog({
                Title = "Title",
                Content = "This is a dialog",
                Buttons = {
                    {
                        Title = "Confirm",
                        Callback = function()
                            print("Confirmed the dialog.")
                        end
                    },
                    {
                        Title = "Cancel",
                        Callback = function()
                            print("Cancelled the dialog.")
                        end
                    }
                }
            })
        end
    })

    local Toggle = Tabs.General:AddToggle("MyToggle", {Title = "Toggle", Default = false })

    Toggle:OnChanged(function()
        print("Toggle changed:", Options.MyToggle.Value)
    end)

    Options.MyToggle:SetValue(false)



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


local Dropdown = Tabs.TP:AddDropdown("Player", {
    Title = "Player",
    Values = {},
    Multi = false,
    Default = 1,
})

Dropdown:SetValue("None")

Dropdown:OnChanged(function(Value)

    print("Selected Player:", Value)
end)


for _, player in pairs(game.Players:GetPlayers()) do
    table.insert(Dropdown.Values, player.Name)
end


game.Players.PlayerAdded:Connect(function(player)
    table.insert(Dropdown.Values, player.Name)
    Dropdown.Default = #Dropdown.Values
end)


game.Players.PlayerRemoving:Connect(function(player)
    local playerName = player.Name
    for i, name in ipairs(Dropdown.Values) do
        if name == playerName then
            table.remove(Dropdown.Values, i)
            if Dropdown.Default == i then
                Dropdown.Default = 1
            end
            break
        end
    end
end)

Tabs.TP:AddButton({
    Title = "Teleport to Player",
    Description = "Teleport to the selected player",
    Callback = function()
        local selectedPlayerName = Dropdown.Values[Dropdown.Default]
        if selectedPlayerName and selectedPlayerName ~= "None" then
           
            local selectedPlayer = game.Players:FindFirstChild(selectedPlayerName)
            if selectedPlayer then
                game.Players.LocalPlayer.Character:MoveTo(selectedPlayer.Character.HumanoidRootPart.Position)
            end
        else
            print("No player selected")
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
