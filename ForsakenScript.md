-- LocalScript único para ForsakenGuiS (substitui os LocalScripts individuais)
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local screenGui = script.Parent
local openBtn = screenGui:WaitForChild("Open")
local frame = screenGui:WaitForChild("Frame")
local espBtn = frame:WaitForChild("EspButton")
local tpPrBtn = frame:WaitForChild("TPprButton")
local tpBtn = frame:WaitForChild("TPButton")
local playerNameBox = frame:WaitForChild("PlayerName")
local tplBtn = frame:WaitForChild("TPLButton")
local saveCordBtn = frame:WaitForChild("SaveCord")
local tpCordBtn = frame:WaitForChild("TpCord")
local credFrame = screenGui:WaitForChild("Cred")
local openCredBtn = screenGui:WaitForChild("Opencred")

-- Ensure frame starts closed
frame.Visible = false
credFrame.Visible = false

openBtn.MouseButton1Click:Connect(function()
	frame.Visible = not frame.Visible
end)

openCredBtn.MouseButton1Click:Connect(function()
	credFrame.Visible = not credFrame.Visible
end)

-- ===== ESP =====
-- Armazenamento por jogador para limpar quando character remove
local espData = {} -- espData[player] = { highlight = Highlight, billboard = BillboardGui, heartbeatConn = RBXScriptConnection }

local function createBillboardForPlayer(targetPlayer)
	local char = targetPlayer.Character
	if not char then return end
	local head = char:FindFirstChild("Head") or char:FindFirstChild("HumanoidRootPart")
	if not head then return end

	-- BillboardGui parented ao PlayerGui do jogador local para ser visível somente a ele
	local billboard = Instance.new("BillboardGui")
	billboard.Adornee = head
	billboard.AlwaysOnTop = true
	billboard.Size = UDim2.new(0, 120, 0, 30)
	billboard.StudsOffset = Vector3.new(0, 2.5, 0)
	billboard.Parent = playerGui

	local label = Instance.new("TextLabel")
	label.Size = UDim2.new(1, 0, 1, 0)
	label.BackgroundTransparency = 1
	label.Font = Enum.Font.SourceSansBold
	label.TextSize = 14
	label.TextStrokeTransparency = 0.4
	label.TextColor3 = Color3.new(1,1,1)
	label.Text = targetPlayer.Name
	label.Parent = billboard

	return billboard, label
end

local function createHighlightForCharacter(character)
	-- Highlight funciona se for parentado no workspace ou no próprio character
	local highlight = Instance.new("Highlight")
	highlight.Adornee = character
	highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
	highlight.FillTransparency = 0.8
	highlight.OutlineTransparency = 0.2
	highlight.Parent = workspace
	return highlight
end

local function cleanupESPForPlayer(p)
	local data = espData[p]
	if not data then return end
	if data.heartbeatConn then
		data.heartbeatConn:Disconnect()
		data.heartbeatConn = nil
	end
	if data.billboard and data.billboard.Parent then
		data.billboard:Destroy()
	end
	if data.highlight and data.highlight.Parent then
		data.highlight:Destroy()
	end
	espData[p] = nil
end

local function setupESPForPlayer(targetPlayer)
	if targetPlayer == player then return end
	-- evita duplicar
	if espData[targetPlayer] then return end

	local function onCharacterLoaded(char)
		cleanupESPForPlayer(targetPlayer)

		-- cria highlight + billboard
		local highlight = createHighlightForCharacter(char)
		local billboard, label = createBillboardForPlayer(targetPlayer)
		if not billboard then
			-- falha, limpa highlight
			if highlight and highlight.Parent then highlight:Destroy() end
			return
		end

		-- Heartbeat atualiza distância e checa vida do character
		local conn
		conn = RunService.Heartbeat:Connect(function()
			if not char.Parent or not char:FindFirstChild("HumanoidRootPart") or not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") then
				-- personagem saiu -> limpa e desconecta
				cleanupESPForPlayer(targetPlayer)
				if conn then conn:Disconnect() end
				return
			end

			local myRoot = player.Character:FindFirstChild("HumanoidRootPart")
			local theirRoot = char:FindFirstChild("HumanoidRootPart")
			if myRoot and theirRoot and label then
				local dist = (theirRoot.Position - myRoot.Position).Magnitude
				label.Text = string.format("%s (%.0fm)", targetPlayer.Name, dist)
			end
		end)

		espData[targetPlayer] = {
			highlight = highlight;
			billboard = billboard;
			heartbeatConn = conn;
		}
	end

	-- se já tiver character, liga; senão espera carregar
	if targetPlayer.Character then
		onCharacterLoaded(targetPlayer.Character)
	end
	targetPlayer.CharacterAdded:Connect(onCharacterLoaded)
	-- também limpa quando o jogador sair do jogo
	targetPlayer.AncestryChanged:Connect(function()
		if not targetPlayer:IsDescendantOf(game) then
			cleanupESPForPlayer(targetPlayer)
		end
	end)
end

espBtn.MouseButton1Click:Connect(function()
	-- evita ativar várias vezes
	if _G.ESPAtivo then
		warn("[ESP] Já está ativo")
		return
	end
	_G.ESPAtivo = true
	for _, plr in ipairs(Players:GetPlayers()) do
		setupESPForPlayer(plr)
	end
	Players.PlayerAdded:Connect(function(plr) setupESPForPlayer(plr) end)
	Players.PlayerRemoving:Connect(function(plr) cleanupESPForPlayer(plr) end)
	print("[ESP] Ativo")
end)

-- ===== Teleports =====

-- Teleporta perto do jogador mais próximo
tpPrBtn.MouseButton1Click:Connect(function()
	local character = player.Character or player.CharacterAdded:Wait()
	local root = character:FindFirstChild("HumanoidRootPart")
	if not root then return end

	local closestPlayer
	local closestDist = math.huge
	for _, plr in pairs(Players:GetPlayers()) do
		if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
			local otherRoot = plr.Character.HumanoidRootPart
			local dist = (otherRoot.Position - root.Position).Magnitude
			if dist < closestDist then
				closestDist = dist
				closestPlayer = plr
			end
		end
	end

	if closestPlayer and closestPlayer.Character and closestPlayer.Character:FindFirstChild("HumanoidRootPart") then
		local targetRoot = closestPlayer.Character.HumanoidRootPart
		local direction = (root.Position - targetRoot.Position)
		if direction.Magnitude ~= 0 then
			direction = direction.Unit
		else
			direction = Vector3.new(0,0,1)
		end
		local newPos = targetRoot.Position + direction * 3
		root.CFrame = CFrame.new(newPos)
		print("[TPprButton] Teleportado perto de:", closestPlayer.Name)
	else
		warn("[TPprButton] Nenhum jogador próximo encontrado!")
	end
end)

-- Teleporta para o jogador pelo nome digitado
tpBtn.MouseButton1Click:Connect(function()
	local targetName = playerNameBox.Text or ""
	if targetName == "" then
		warn("[TPButton] Digite o nome de um jogador!")
		return
	end

	local targetPlayer
	for _, plr in pairs(Players:GetPlayers()) do
		if string.lower(plr.Name) == string.lower(targetName) then
			targetPlayer = plr
			break
		end
	end

	if not targetPlayer then
		warn("[TPButton] Jogador não encontrado no servidor.")
		return
	end

	local character = player.Character or player.CharacterAdded:Wait()
	local root = character:FindFirstChild("HumanoidRootPart")
	if not root then return end

	if targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
		local targetRoot = targetPlayer.Character.HumanoidRootPart
		root.CFrame = targetRoot.CFrame + Vector3.new(0, 3, 0)
		print("[TPButton] Teleportado até:", targetPlayer.Name)
	else
		warn("[TPButton] Jogador existe, mas character não carregou.")
	end
end)

-- Cria plataforma alta (visível globalmente; highlight faz destaque pra cliente)
tplBtn.MouseButton1Click:Connect(function()
	local character = player.Character or player.CharacterAdded:Wait()
	local root = character and character:FindFirstChild("HumanoidRootPart")
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

	-- highlight client-side (só pra você): parentado no workspace mas adornee aponta pra part
	local highlight = Instance.new("Highlight")
	highlight.Adornee = part
	highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
	highlight.FillTransparency = 0.8
	highlight.OutlineColor = Color3.fromRGB(255,0,0)
	highlight.Parent = workspace

	root.CFrame = part.CFrame + Vector3.new(0,3,0)

	print("[TPLButton] Teleportado para o alto!")

	task.delay(4, function()
		if part and part.Parent then part:Destroy() end
		if highlight and highlight.Parent then highlight:Destroy() end
		print("[TPLButton] Plataforma removida.")
	end)
end)

-- ===== Salvar / Teleportar Coordenada =====
-- Recomendo usar Attributes ou ModuleScript para persistência; aqui uso _G pra simplicidade cliente
_G.SavedCFrame = _G.SavedCFrame or nil

saveCordBtn.MouseButton1Click:Connect(function()
	local character = player.Character or player.CharacterAdded:Wait()
	local root = character:FindFirstChild("HumanoidRootPart")
	if not root then return end
	_G.SavedCFrame = root.CFrame
	print(string.format("[SaveCord] Coordenada salva em: X=%.2f, Y=%.2f, Z=%.2f", root.Position.X, root.Position.Y, root.Position.Z))
end)

tpCordBtn.MouseButton1Click:Connect(function()
	if not _G.SavedCFrame then
		warn("[TpCord] Nenhuma coordenada salva! Use 'Salvar Cordenadas' primeiro.")
		return
	end
	local character = player.Character or player.CharacterAdded:Wait()
	local root = character:FindFirstChild("HumanoidRootPart")
	if not root then return end
	root.CFrame = _G.SavedCFrame + Vector3.new(0,3,0)
	print("[TpCord] Teleportado para coordenada salva.")
end)
