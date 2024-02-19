local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/title33/SaveManager/main/README.md"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "Xylo Hub",
    SubTitle = "by Sky",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl
})

local Tabs = {
    General = Window:AddTab({ Title = "General", Icon = "home" }),
    Skil = Window:AddTab({ Title = "Skil", Icon = "battery-charging" }),
    TP = Window:AddTab({ Title = "Tp", Icon = "chevrons-right" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}

local Options = Fluent.Options

-- ... (Your existing code)

local DropdownNPC = Tabs.TP:AddDropdown("NPC", {
    Title = "NPC",
    Values = {},  
    Multi = false,
    Default = 1,
})

for _, npc in pairs(game:GetService("Workspace").NPC:GetChildren()) do
    table.insert(DropdownNPC.Values, npc.Name)
end

DropdownNPC:SetValue("None")

DropdownNPC:OnChanged(function(Value)
    print("Dropdown changed:", Value)
end)

Tabs.TP:AddButton({
    Title = "Teleport to NPC",
    Description = "Teleport to the selected NPC",
    Callback = function()
        local selectedNPCName = DropdownNPC.Values[DropdownNPC.Default]
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



InterfaceManager:SetFolder("FluentScriptHub")
SaveManager:SetFolder("FluentScriptHub/specific-game")

InterfaceManager:BuildInterfaceSection(Tabs.Settings)
SaveManager:BuildConfigSection(Tabs.Settings)

Window:SelectTab(1)

SaveManager:LoadAutoloadConfig()
