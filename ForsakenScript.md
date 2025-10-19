-- LocalScript único para criar ForsakenGuiS no PlayerGui
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- ======= CRIAÇÃO DA GUI =======
local ForsakenGuiS = Instance.new("ScreenGui")
ForsakenGuiS.Name = "ForsakenGuiS"
ForsakenGuiS.Parent = playerGui
ForsakenGuiS.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- Botão abrir/fechar
local Open = Instance.new("TextButton")
Open.Name = "Open"
Open.Parent = ForsakenGuiS
Open.BackgroundColor3 = Color3.fromRGB(186,4,4)
Open.BorderSizePixel = 0
Open.Position = UDim2.new(0.17,0,0.92,0)
Open.Size = UDim2.new(0,133,0,36)
Open.Font = Enum.Font.Unknown
Open.Text = "#$!??$!&%"
Open.TextColor3 = Color3.new(0,0,0)
Open.TextScaled = true

-- Frame principal
local Frame = Instance.new("Frame")
Frame.Name = "Frame"
Frame.Parent = ForsakenGuiS
Frame.BackgroundColor3 = Color3.new(0,0,0)
Frame.BackgroundTransparency = 0.4
Frame.BorderSizePixel = 0
Frame.Position = UDim2.new(0.08,0,0.20,0)
Frame.Size = UDim2.new(0,449,0,421)
Frame.Visible = false

-- Botões dentro do frame
local function createButton(name, parent, pos, size, text)
    local btn = Instance.new("TextButton")
    btn.Name = name
    btn.Parent = parent
    btn.BackgroundTransparency = 1
    btn.BorderSizePixel = 0
    btn.Position = pos
    btn.Size = size
    btn.Font = Enum.Font.SourceSans
    btn.Text = text
    btn.TextColor3 = Color3.fromRGB(255,0,0)
    btn.TextScaled = true
    btn.TextWrapped = true
    return btn
end

local EspButton = createButton("EspButton", Frame, UDim2.new(0.03,0,0.028,0), UDim2.new(0,110,0,27), "Esp")
local TPprButton = createButton("TPprButton", Frame, UDim2.new(0.29,0,0.028,0), UDim2.new(0,110,0,27), "Tp Jogador Mais Proximo")
local TPButton = createButton("TPButton", Frame, UDim2.new(0.31,0,0.20,0), UDim2.new(0,110,0,27), "Tp para jogador")
local TPLButton = createButton("TPLButton", Frame, UDim2.new(0.049,0,0.118,0), UDim2.new(0,110,0,27), "Tp Lugar Alto")
local SaveCord = createButton("SaveCord", Frame, UDim2.new(0.03,0,0.204,0), UDim2.new(0,110,0,27), "Salvar Cordenadas")
local TpCord = createButton("TpCord", Frame, UDim2.new(0.03,0,0.294,0), UDim2.new(0,110,0,27), "Teleportar Pra Cordenada")

-- TextBox para nome do jogador
local PlayerName = Instance.new("TextBox")
PlayerName.Name = "PlayerName"
PlayerName.Parent = Frame
PlayerName.BackgroundTransparency = 1
PlayerName.BorderSizePixel = 0
PlayerName.Position = UDim2.new(0.29,0,0.106,0)
PlayerName.Size = UDim2.new(0,120,0,27)
PlayerName.Font = Enum.Font.SourceSans
PlayerName.Text = "Nome Do Jogador"
PlayerName.TextColor3 = Color3.fromRGB(255,0,0)
PlayerName.TextSize = 14

-- Label principal
local NoNm = Instance.new("TextLabel")
NoNm.Name = "NoNm"
NoNm.Parent = Frame
NoNm.BackgroundTransparency = 1
NoNm.Position = UDim2.new(0.22,0,-0.092,0)
NoNm.Size = UDim2.new(0,175,0,32)
NoNm.Font = Enum.Font.Unknown
NoNm.Text = "Script Forsaken"
NoNm.TextColor3 = Color3.new(0,0,0)
NoNm.TextScaled = true
NoNm.TextWrapped = true

-- Frame de créditos
local Cred = Instance.new("Frame")
Cred.Name = "Cred"
Cred.Parent = ForsakenGuiS
Cred.BackgroundColor3 = Color3.new(0,0,0)
Cred.BackgroundTransparency = 0.4
Cred.BorderSizePixel = 0
Cred.Position = UDim2.new(0.63,0,0.22,0)
Cred.Size = UDim2.new(0,449,0,421)
Cred.Visible = false

local function createLabel(name, parent, pos, size, text)
    local lbl = Instance.new("TextLabel")
    lbl.Name = name
    lbl.Parent = parent
    lbl.BackgroundTransparency = 1
    lbl.Position = pos
    lbl.Size = size
    lbl.Font = Enum.Font.SourceSans
    lbl.Text = text
    lbl.TextColor3 = Color3.fromRGB(182,182,182)
    lbl.TextScaled = true
    lbl.TextWrapped = true
    return lbl
end

createLabel("Ide", Cred, UDim2.new(0,0,0,0), UDim2.new(0,232,0,50), "Ideia: Tubers83hacker")
createLabel("Scri", Cred, UDim2.new(0,0,0.11,0), UDim2.new(0,232,0,50), "Scripter: Chat GPT ESP Tps Cordenadas")
createLabel("Ui", Cred, UDim2.new(0,0,0.26,0), UDim2.new(0,232,0,48), "Ui: ???")
createLabel("Discord", Cred, UDim2.new(0,0,0.92,0), UDim2.new(0,176,0,31), "Discord Tubers: flowers_1_0")
createLabel("Discord2", Cred, UDim2.new(0.42,0,0.92,0), UDim2.new(0,176,0,31), "Discord ?? : vl4dky")

-- Botão abrir créditos
local Opencred = Instance.new("TextButton")
Opencred.Name = "Opencred"
Opencred.Parent = ForsakenGuiS
Opencred.BackgroundColor3 = Color3.fromRGB(186,4,4)
Opencred.BorderSizePixel = 0
Opencred.Position = UDim2.new(0.74,0,0.92,0)
Opencred.Size = UDim2.new(0,133,0,36)
Opencred.Font = Enum.Font.Unknown
Opencred.Text = "Creditos"
Opencred.TextColor3 = Color3.new(0,0,0)
Opencred.TextScaled = true

-- ======= FUNCIONALIDADES =======
-- Toggle frame principal
Open.MouseButton1Click:Connect(function()
    Frame.Visible = not Frame.Visible
end)

-- Toggle frame créditos
Opencred.MouseButton1Click:Connect(function()
    Cred.Visible = not Cred.Visible
end)

-- ====== ESP ======
local espData = {}
EspButton.MouseButton1Click:Connect(function()
    if _G.ESPAtivo then return end
    _G.ESPAtivo = true

    for _, plr in ipairs(Players:GetPlayers()) do
        if plr ~= player then
            local function setupCharacter(char)
                if not char then return end
                local highlight = Instance.new("Highlight")
                highlight.Adornee = char
                highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
                highlight.FillTransparency = 0.8
                highlight.OutlineTransparency = 0.2
                highlight.Parent = workspace

                local head = char:FindFirstChild("Head") or char:FindFirstChild("HumanoidRootPart")
                if head then
                    local billboard = Instance.new("BillboardGui")
                    billboard.Adornee = head
                    billboard.AlwaysOnTop = true
                    billboard.Size = UDim2.new(0,120,0,30)
                    billboard.StudsOffset = Vector3.new(0,2.5,0)
                    billboard.Parent = playerGui

                    local label = Instance.new("TextLabel")
                    label.Size = UDim2.new(1,0,1,0)
                    label.BackgroundTransparency = 1
                    label.Font = Enum.Font.SourceSansBold
                    label.TextSize = 14
                    label.TextStrokeTransparency = 0.4
                    label.TextColor3 = Color3.new(1,1,1)
                    label.Text = plr.Name
                    label.Parent = billboard

                    RunService.Heartbeat:Connect(function()
                        if not char.Parent then
                            highlight:Destroy()
                            billboard:Destroy()
                            return
                        end
                        local myRoot = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
                        local theirRoot = char:FindFirstChild("HumanoidRootPart")
                        if myRoot and theirRoot then
                            local dist = (theirRoot.Position - myRoot.Position).Magnitude
                            label.Text = string.format("%s (%.0fm)", plr.Name, dist)
                        end
                    end)
                end
            end

            if plr.Character then
                setupCharacter(plr.Character)
            end
            plr.CharacterAdded:Connect(setupCharacter)
        end
    end

    Players.PlayerAdded:Connect(function(plr)
        if plr ~= player then
            plr.CharacterAdded:Connect(function(char) 
                if char then
                    local highlight = Instance.new("Highlight")
                    highlight.Adornee = char
                    highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
                    highlight.FillTransparency = 0.8
                    highlight.OutlineTransparency = 0.2
                    highlight.Parent = workspace
                end
            end)
        end
    end)
end)

-- ===== TELEPORTS =====
TPprButton.MouseButton1Click:Connect(function()
    local char = player.Character or player.CharacterAdded:Wait()
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    local closest, dist = nil, math.huge
    for _, plr in pairs(Players:GetPlayers()) do
        if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            local d = (plr.Character.HumanoidRootPart.Position - root.Position).Magnitude
            if d < dist then dist, closest = d, plr end
        end
    end
    if closest then
        local dir = (root.Position - closest.Character.HumanoidRootPart.Position).Unit
        root.CFrame = CFrame.new(closest.Character.HumanoidRootPart.Position + dir*3)
        print("Teleportado perto de:", closest.Name)
    end
end)

TPButton.MouseButton1Click:Connect(function()
    local targetName = PlayerName.Text
    if targetName == "" then return end
    local targetPlayer
    for _, plr in pairs(Players:GetPlayers()) do
        if string.lower(plr.Name) == string.lower(targetName) then
            targetPlayer = plr
            break
        end
    end
    if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local root = player.Character.HumanoidRootPart
        root.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame + Vector3.new(0,3,0)
    end
end)

TPLButton.MouseButton1Click:Connect(function()
    local char = player.Character or player.CharacterAdded:Wait()
    local root = char:FindFirstChild("HumanoidRootPart")
    if not root then return end
    local part = Instance.new("Part")
    part.Size = Vector3.new(6,1,6)
    part.Anchored = true
    part.CanCollide = true
    part.Material = Enum.Material.Neon
    part.Color = Color3.fromRGB(255,0,0)
    part.Transparency = 0.2
    part.CFrame = CFrame.new(root.Position + Vector3.new(0,50,0))
    part.Parent = workspace
    root.CFrame = part.CFrame + Vector3.new(0,3,0)
    task.delay(4,function()
        part:Destroy()
    end)
end)

-- ===== SALVAR / TELEPORTAR COORD =====
_G.SavedCFrame = nil
SaveCord.MouseButton1Click:Connect(function()
    local root = player.Character.HumanoidRootPart
    _G.SavedCFrame = root.CFrame
    print("Coordenada salva!")
end)

TpCord.MouseButton1Click:Connect(function()
    if _G.SavedCFrame then
        local root = player.Character.HumanoidRootPart
        root.CFrame = _G.SavedCFrame + Vector3.new(0,3,0)
        print("Teleportado para coordenada salva!")
    end
end)
