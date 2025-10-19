local player = game.Players.LocalPlayer
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

--[[
Criando a GUI
]]
local ForsakenGuiS = Instance.new("ScreenGui")
ForsakenGuiS.Name = "ForsakenGuiS"
ForsakenGuiS.ResetOnSpawn = false
ForsakenGuiS.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ForsakenGuiS.Parent = player:WaitForChild("PlayerGui")

-- Frame principal
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 150, 0, 450)
frame.Position = UDim2.new(0.03, 0, 0.2, 0)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BackgroundTransparency = 0.4
frame.Visible = false
frame.Parent = ForsakenGuiS

-- Botão abrir/fechar
local Open = Instance.new("TextButton")
Open.Size = UDim2.new(0, 150, 0, 36)
Open.Position = UDim2.new(0.03, 0, 0.9, 0)
Open.BackgroundColor3 = Color3.fromRGB(186, 4, 4)
Open.Text = "Abrir"
Open.TextScaled = true
Open.Parent = ForsakenGuiS

Open.MouseButton1Click:Connect(function()
	frame.Visible = not frame.Visible
end)

-- Função para criar botões
local function makeButton(text, posY)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(0, 120, 0, 30)
	btn.Position = UDim2.new(0.08, 0, posY, 0)
	btn.BackgroundTransparency = 1
	btn.TextColor3 = Color3.fromRGB(255,0,0)
	btn.Text = text
	btn.TextScaled = true
	btn.Parent = frame
	return btn
end

-- Criando botões
local PlayerName = Instance.new("TextBox")
PlayerName.Size = UDim2.new(0, 120, 0, 32)
PlayerName.Position = UDim2.new(0.08, 0, 0.15, 0)
PlayerName.Text = "Nome do Player"
PlayerName.BackgroundTransparency = 1
PlayerName.TextColor3 = Color3.fromRGB(255,0,0)
PlayerName.Parent = frame

local TPButton = makeButton("Tp para jogador", 0.26)
local TPprButton = makeButton("Tp jogador + próximo", 0.11)
local TPLButton = makeButton("Tp lugar alto", 0.32)
local SaveCord = makeButton("Salvar coordenada", 0.42)
local TpCord = makeButton("Teleportar coordenada", 0.48)
local EspButton = makeButton("ESP", 0.05)

local NoNm = Instance.new("TextLabel")
NoNm.Size = UDim2.new(0, 150, 0, 30)
NoNm.Position = UDim2.new(-0.05, 0, -0.07, 0)
NoNm.BackgroundTransparency = 1
NoNm.Text = "Script Forsaken"
NoNm.TextScaled = true
NoNm.Parent = frame

-- Variáveis globais
local savedPos
local savedPart
local espAtivo = false

--[[
Funções dos botões
]]
-- TP para jogador
TPButton.MouseButton1Click:Connect(function()
	local target = Players:FindFirstChild(PlayerName.Text)
	if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
		player.Character:WaitForChild("HumanoidRootPart").CFrame =
			target.Character.HumanoidRootPart.CFrame + Vector3.new(0,2,0)
	end
end)

-- TP para jogador mais próximo
TPprButton.MouseButton1Click:Connect(function()
	local myRoot = player.Character:WaitForChild("HumanoidRootPart")
	local closest, shortest = nil, math.huge

	for _, other in ipairs(Players:GetPlayers()) do
		if other ~= player and other.Character and other.Character:FindFirstChild("HumanoidRootPart") then
			local dist = (myRoot.Position - other.Character.HumanoidRootPart.Position).Magnitude
			if dist < shortest then
				shortest = dist
				closest = other
			end
		end
	end

	if closest then
		myRoot.CFrame = closest.Character.HumanoidRootPart.CFrame + Vector3.new(0,2,0)
	end
end)

-- TP lugar alto com plataforma
TPLButton.MouseButton1Click:Connect(function()
	local root = player.Character:WaitForChild("HumanoidRootPart")
	local highPos = root.Position + Vector3.new(0,150,0)

	local part = Instance.new("Part")
	part.Anchored = true
	part.Size = Vector3.new(10,1,10)
	part.Position = highPos
	part.Material = Enum.Material.Neon
	part.Color = Color3.fromRGB(255,0,0)
	part.Parent = workspace

	root.CFrame = CFrame.new(highPos + Vector3.new(0,3,0))

	task.wait(4)
	part:Destroy()
end)

-- Save coordenada
SaveCord.MouseButton1Click:Connect(function()
	local root = player.Character:WaitForChild("HumanoidRootPart")
	if savedPart then savedPart:Destroy() end
	savedPos = root.Position

	local mark = Instance.new("Part")
	mark.Anchored = true
	mark.Size = Vector3.new(3,1,3)
	mark.Position = savedPos
	mark.Color = Color3.fromRGB(0,255,0)
	mark.Material = Enum.Material.Neon
	mark.Transparency = 0.4
	mark.Parent = workspace
	savedPart = mark
end)

-- TP coordenada salva
TpCord.MouseButton1Click:Connect(function()
	local root = player.Character:WaitForChild("HumanoidRootPart")
	if savedPos then
		root.CFrame = CFrame.new(savedPos + Vector3.new(0,3,0))
	end
end)

-- ESP
EspButton.MouseButton1Click:Connect(function()
	if espAtivo then return end
	espAtivo = true
	local NAME_TEXT_SIZE = 14
	local BILLBOARD_OFFSET = Vector3.new(0,2.5,0)
	local OUTLINE_COLOR = Color3.fromRGB(0,255,0)

	local function createBillboard(char, target)
		local head = char:FindFirstChild("Head") or char:FindFirstChild("HumanoidRootPart")
		if not head then return end

		local billboard = Instance.new("BillboardGui")
		billboard.Adornee = head
		billboard.AlwaysOnTop = true
		billboard.Size = UDim2.new(0,120,0,30)
		billboard.StudsOffset = BILLBOARD_OFFSET
		billboard.Parent = game.CoreGui

		local label = Instance.new("TextLabel")
		label.Size = UDim2.new(1,0,1,0)
		label.BackgroundTransparency = 1
		label.Font = Enum.Font.SourceSansBold
		label.TextSize = NAME_TEXT_SIZE
		label.TextStrokeTransparency = 0.4
		label.TextColor3 = Color3.new(1,1,1)
		label.Text = target.Name
		label.Parent = billboard
		return billboard, label
	end

	local function createHighlight(char)
		local highlight = Instance.new("Highlight")
		highlight.Adornee = char
		highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
		highlight.FillTransparency = 0.8
		highlight.OutlineTransparency = 0.2
		highlight.OutlineColor = OUTLINE_COLOR
		highlight.Parent = char
		return highlight
	end

	local function setupESP(target)
		if target == player then return end
		local function onChar(char)
			if not char then return end
			local highlight = createHighlight(char)
			local billboard, label = createBillboard(char, target)
			RunService.Heartbeat:Connect(function()
				if not char.Parent then
					if highlight then highlight:Destroy() end
					if billboard then billboard:Destroy() end
					return
				end
				local root = char:FindFirstChild("HumanoidRootPart")
				local myRoot = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
				if root and myRoot and label then
					local dist = (root.Position - myRoot.Position).Magnitude
					label.Text = string.format("%s (%.0fm)", target.Name, dist)
				end
			end)
		end
		if target.Character then onChar(target.Character) end
		target.CharacterAdded:Connect(onChar)
	end

	for _, plr in pairs(Players:GetPlayers()) do
		setupESP(plr)
	end
	Players.PlayerAdded:Connect(setupESP)
	print("[ESP] Ativo para todos os jogadores!")
end)
