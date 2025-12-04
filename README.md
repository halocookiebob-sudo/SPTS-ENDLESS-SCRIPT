local AREAS_PARENT = workspace.Map.Training_Collisions

local CATEGORIES = {
    ["FS FARM"] = "FistStrength",
    ["MS FARM"] = "MovementSpeed",
    ["BT FARM"] = "BodyToughness",
    ["PP FARM"] = "PsychicPower",
    ["JP FARM"] = "JumpForce",
}

local AREA_REQUIREMENTS = {
    ["BT FARM"] = {
        FireBathTouchPart = "10k",
        Water = "100",
        IcePart = "100K",
        LavaPart = "10M",
        TornadoTouchPart = "1M",
        GreenFirePart = "1B",
        AcidPart = "100B",
        LavaPart2 = "10T",
        AFK_BT_1 = "1Qa",
        AFK_BT_2 = "50Qa",
        AFK_BT_3 = "5Qi",
        AFK_BT_4 = "450Qi",
        AFK_BT_5 = "40Sx",
        AFK_BT_6 = "3Sp",
        AFK_BT_7 = "250Sp",
        AFK_BT_8 = "20Oc",
        AFK_BT_9 = "2No",
        AFK_BT_10 = "200No",
        AFK_BT_11 = "15Dc",
        AFK_BT_12 = "1Ud",
        AFK_BT_14 = "10Dd",
        AFK_BT_13 = "250Ud",
        AFK_BT_15 = "1Td",
    },
    ["FS FARM"] = {
        TrainingArea_2 = "0",
        AFK_FS_1 = "1Qa",
        TrainingArea_3 = "1M",
        StarFSTraining1 = "1B",
        StarFSTraining2 = "100B",
        StarFSTraining3 = "10T",
        AFK_FS_2 = "100Qa",
        AFK_FS_3 = "15Qi",
        AFK_FS_4 = "2.5Sx",
        AFK_FS_5 = "1Sp",
        AFK_FS_6 = "500Sp",
        AFK_FS_7 = "250Oc",
        AFK_FS_8 = "150No",
        AFK_FS_9 = "55Dc",
        AFK_FS_10 = "30Ud",
        AFK_FS_11 = "11Dd",
    },
    ["MS FARM"] = {
        AFK_MS_1 = "1Qa",
        AFK_MS_2 = "2.22Qa",
        AFK_MS_3 = "60Qa",
        AFK_MS_4 = "1.5Qi",
        AFK_MS_5 = "40Qi",
        AFK_MS_6 = "1Sx",
        AFK_MS_7 = "25Sx",
        AFK_MS_8 = "750Sx",
        AFK_MS_9 = "15.5Sp",
        AFK_MS_10 = "400Sp",
        AFK_MS_11 = "10Oc",
    },
    ["PP FARM"] = {
        PPTrainingPart1 = "1M",
        PPTrainingPart2 = "1B",
        PPTrainingPart3 = "1T",
        PPTrainingPart4 = "1Qa",
        AFK_PP_1 = "333Qa",
        AFK_PP_2 = "111Qi",
        AFK_PP_3 = "33.3Sx",
        AFK_PP_4 = "11.1Sp",
        AFK_PP_5 = "3.36Oc",
        AFK_PP_6 = "1.11No",
        AFK_PP_7 = "444No",
        AFK_PP_8 = "111Dc",
        AFK_PP_9 = "55.5Ud",
        AFK_PP_10 = "22.2Dd",
    },
    ["JP FARM"] = {
        AFK_JF_1 = "100T",
        AFK_JF_2 = "5Qa",
        AFK_JF_3 = "150Qa",
        AFK_JF_4 = "5Qi",
        AFK_JF_5 = "200Qi",
        AFK_JF_6 = "10Sx",
        AFK_JF_7 = "300Sx",
        AFK_JF_8 = "15Sp",
        AFK_JF_9 = "400Sp",
    },
}

local UNIT_ORDER = {
    k = 1, M = 2, B = 3, T = 4, Qa = 5, Qi = 6, Sx = 7, Sp = 8, Oc = 9, No = 10, Dc = 11, Ud = 12, Dd = 13, Td = 14
}

local function parseRequirement(str)
    local num, unit = str:match("([%d%.]+)(%a+)")
    num = tonumber(num) or 0
    local unitOrder = UNIT_ORDER[unit] or 0
    return unitOrder, num
end

local Players = game:GetService("Players")
local player = Players.LocalPlayer

if player.PlayerGui:FindFirstChild("FarmUI") then
    player.PlayerGui.FarmUI:Destroy()
end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "FarmUI"
ScreenGui.IgnoreGuiInset = true
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = player.PlayerGui

local mainFrame = Instance.new("Frame", ScreenGui)
mainFrame.Size = UDim2.new(0, 180, 0, 250)
mainFrame.Position = UDim2.new(0, 20, 0.3, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)

local UIList = Instance.new("UIListLayout", mainFrame)
UIList.Padding = UDim.new(0, 5)

local listFrame = Instance.new("Frame", ScreenGui)
listFrame.Size = UDim2.new(0, 260, 0, 300)
listFrame.Position = UDim2.new(0, 220, 0.3, 0)
listFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
listFrame.Visible = false

local listTitle = Instance.new("TextLabel", listFrame)
listTitle.Size = UDim2.new(1, 0, 0, 40)
listTitle.Text = "Área: Nenhuma"
listTitle.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
listTitle.TextColor3 = Color3.new(1, 1, 1)
listTitle.Font = Enum.Font.FredokaOne
listTitle.TextScaled = true

local closeButton = Instance.new("TextButton", listFrame)
closeButton.Size = UDim2.new(0, 40, 0, 40)
closeButton.Position = UDim2.new(1, -45, 0, 0)
closeButton.BackgroundColor3 = Color3.fromRGB(150, 50, 50)
closeButton.TextColor3 = Color3.new(1, 1, 1)
closeButton.TextScaled = true
closeButton.Font = Enum.Font.FredokaOne
closeButton.Text = "X"
closeButton.ZIndex = 5

closeButton.MouseButton1Click:Connect(function()
    listFrame.Visible = false
end)

local autoFarmBtn = Instance.new("TextButton", listFrame)
autoFarmBtn.Size = UDim2.new(1, 0, 0, 40)
autoFarmBtn.Position = UDim2.new(0, 0, 0, 40)
autoFarmBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
autoFarmBtn.TextColor3 = Color3.new(1, 1, 1)
autoFarmBtn.Text = "AutoFarm: OFF"
autoFarmBtn.Font = Enum.Font.FredokaOne
autoFarmBtn.TextScaled = true

local scroll = Instance.new("ScrollingFrame", listFrame)
scroll.Size = UDim2.new(1, 0, 1, -90)
scroll.Position = UDim2.new(0, 0, 0, 80)
scroll.CanvasSize = UDim2.new(0, 0, 0, 0)

local scrollList = Instance.new("UIListLayout", scroll)
scrollList.Padding = UDim.new(0, 5)

local selectedArea = nil
local autoFarm = false

local function updateCharacter()
    local char = player.Character or player.CharacterAdded:Wait()
    task.spawn(function()
        while char.Parent do
            task.wait(0.2)
            if autoFarm and selectedArea then
                if selectedArea:FindFirstChild("CFrame") then
                    char:PivotTo(selectedArea.CFrame.Value)
                elseif selectedArea:IsA("BasePart") then
                    char:PivotTo(selectedArea.CFrame)
                end
            end
        end
    end)
end

player.CharacterAdded:Connect(updateCharacter)
updateCharacter()

autoFarmBtn.MouseButton1Click:Connect(function()
    autoFarm = not autoFarm
    autoFarmBtn.Text = "AutoFarm: " .. (autoFarm and "ON" or "OFF")
end)

for name, folderName in pairs(CATEGORIES) do
    local btn = Instance.new("TextButton", mainFrame)
    btn.Size = UDim2.new(1, -10, 0, 40)
    btn.Text = name
    btn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.Font = Enum.Font.FredokaOne
    btn.TextScaled = true

    btn.MouseButton1Click:Connect(function()
        listFrame.Visible = false
        task.wait()
        listFrame.Visible = true

        for _, item in ipairs(scroll:GetChildren()) do
            if item:IsA("TextButton") then item:Destroy() end
        end

        listTitle.Text = "Área: Nenhuma"

        local categoryFolder = AREAS_PARENT:FindFirstChild(folderName)
        if not categoryFolder then return end

        local reqList = {}
        for _, area in pairs(categoryFolder:GetChildren()) do
            local req = AREA_REQUIREMENTS[name][area.Name]
            if req then
                table.insert(reqList, {instance = area, requirement = req})
            end
        end

        table.sort(reqList, function(a, b)
            local unitA, numA = parseRequirement(a.requirement)
            local unitB, numB = parseRequirement(b.requirement)
            if unitA == unitB then
                return numA < numB
            else
                return unitA < unitB
            end
        end)

        for _, data in ipairs(reqList) do
            local areaBtn = Instance.new("TextButton", scroll)
            areaBtn.Size = UDim2.new(1, -10, 0, 40)
            areaBtn.Text = data.requirement
            areaBtn.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
            areaBtn.TextColor3 = Color3.new(1, 1, 1)
            areaBtn.TextScaled = true

            areaBtn.MouseButton1Click:Connect(function()
                selectedArea = data.instance
                listTitle.Text = "Área: " .. data.requirement
            end)
        end

        scroll.CanvasSize = UDim2.new(0, 0, 0, #reqList * 45)
    end)
end
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local player = Players.LocalPlayer
local RemoteEvent = ReplicatedStorage:WaitForChild("RemoteEvent")

local weightValue = 1
local trainInterval = 0.1
local autoJPMS = false -- controle do Auto Jump/Speed

-- Funções principais
local function equipWeight()
    local args = {{"EquipWeight_Request", weightValue}}
    RemoteEvent:FireServer(unpack(args))
end

local function trainJump()
    local args = {{"Add_JF_Request"}}
    RemoteEvent:FireServer(unpack(args))
end

local function trainSpeed()
    local args = {{"Add_MS_Request"}}
    RemoteEvent:FireServer(unpack(args))
end

-- Equipar weight 5s após respawn e reativar farm JP/MS
local function onCharacterAdded()
    delay(5, function()
        equipWeight()
        if weightValue > 0 then
            autoJPMS = true
        else
            autoJPMS = false
        end
    end)
end

player.CharacterAdded:Connect(onCharacterAdded)
if player.Character then
    onCharacterAdded()
end


spawn(function()
    while true do
        if autoJPMS then
            trainJump()
            trainSpeed()
        end
        wait(trainInterval)
    end
end)


local playerGui = player:WaitForChild("PlayerGui")


local screenGui = Instance.new("ScreenGui")
screenGui.Name = "WeightGUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui


local mainButton = Instance.new("TextButton")
mainButton.Size = UDim2.new(0, 120, 0, 50)
mainButton.Position = UDim2.new(0, 20, 0, 20)
mainButton.Text = "Weight"
mainButton.Parent = screenGui


local weightFrame = Instance.new("Frame")
weightFrame.Size = UDim2.new(0, 600, 0, 300)
weightFrame.Position = UDim2.new(0, 20, 0, 80)
weightFrame.Visible = false
weightFrame.BackgroundColor3 = Color3.fromRGB(50,50,50)
weightFrame.Parent = screenGui


local weights = {
    {name="Unequip", value=0, color=Color3.fromRGB(100,100,100)},
    {name="100 LB", value=1, color=Color3.fromRGB(100,100,100)},
    {name="1 TON", value=2, color=Color3.fromRGB(100,100,100)},
    {name="10 TON", value=3, color=Color3.fromRGB(100,100,100)},
    {name="100 TON", value=4, color=Color3.fromRGB(100,100,100)},
    {name="1K TON", value=5, color=Color3.fromRGB(255, 255, 0)},
    {name="10K TON", value=6, color=Color3.fromRGB(255, 255, 0)},
    {name="100K TON", value=7, color=Color3.fromRGB(255, 255, 0)},
    {name="1M TON", value=8, color=Color3.fromRGB(255, 255, 0)},
    {name="10M TON", value=9, color=Color3.fromRGB(255, 255, 0)},
    {name="1B TON", value=10, color=Color3.fromRGB(128, 0, 128)},
    {name="100B TON", value=11, color=Color3.fromRGB(128, 0, 128)},
    {name="10T TON", value=12, color=Color3.fromRGB(128, 0, 128)},
    {name="1Qa TON", value=13, color=Color3.fromRGB(128, 0, 128)},
    {name="100Qa TON", value=14, color=Color3.fromRGB(128, 0, 128)},
    {name="10Qi TON", value=15, color=Color3.fromRGB(255,0,0)},
    {name="1Sx TON", value=16, color=Color3.fromRGB(255,0,0)},
    {name="100Sx TON", value=17, color=Color3.fromRGB(255,0,0)},
    {name="10Sp TON", value=18, color=Color3.fromRGB(255,0,0)},
    {name="100Oc TON", value=19, color=Color3.fromRGB(255,0,0)},
    {name="10No TON", value=20, color=Color3.fromRGB(255,0,0)},
    {name="10Dc TON", value=21, color=Color3.fromRGB(255,0,0)},
    {name="10Td TON", value=22, color=Color3.fromRGB(255,0,0)},
    {name="10Qad TON", value=23, color=Color3.fromRGB(255,0,0)},
    {name="10Qid TON", value=24, color=Color3.fromRGB(255,0,0)},
}


for i, info in ipairs(weights) do
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0, 100, 0, 50)
    btn.Position = UDim2.new(0, ((i-1)%5)*110, 0, math.floor((i-1)/5)*60)
    btn.Text = info.name
    btn.BackgroundColor3 = info.color
    btn.Parent = weightFrame

   btn.MouseButton1Click:Connect(function()
        weightValue = info.value
        equipWeight()

      
 if weightValue > 0 then
            autoJPMS = true
        else
            autoJPMS = false
        end

  print("Weight selecionado:", info.name, "Valor interno:", info.value)
    end)
end


mainButton.MouseButton1Click:Connect(function()
    weightFrame.Visible = not weightFrame.Visible
end)
loadstring(game:HttpGet("https://raw.githubusercontent.com/Genesisuii/Genesisuii/refs/heads/main/Anti%20AFK%20script", true))()
loadstring(game:HttpGet("https://raw.githubusercontent.com/ShockerLL22/spts-classic-main/refs/heads/main/void.lua"))()
