-- MT SCRIPT - Versão Mobile
-- Script para Roblox com controles otimizados para celular

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local rootPart = character:WaitForChild("HumanoidRootPart")

-- Variáveis de controle
local speedMultiplier = 1
local jumpPower = 50
local isMinimized = false

-- Criar ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "MTScriptGui"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Painel Principal (Preto com bordas vermelhas)
local mainPanel = Instance.new("Frame")
mainPanel.Name = "MainPanel"
mainPanel.Size = UDim2.new(0, 280, 0, 320)
mainPanel.Position = UDim2.new(0, 10, 0, 50)
mainPanel.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
mainPanel.BorderColor3 = Color3.fromRGB(255, 0, 0)
mainPanel.BorderSizePixel = 3
mainPanel.Parent = screenGui

-- Título
local titleLabel = Instance.new("TextLabel")
titleLabel.Name = "Title"
titleLabel.Size = UDim2.new(1, 0, 0, 40)
titleLabel.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextSize = 24
titleLabel.Font = Enum.Font.GothamBold
titleLabel.Text = "MT SCRIPT"
titleLabel.Parent = mainPanel

-- Botão Minimizar (canto superior direito)
local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Name = "MinimizeBtn"
minimizeBtn.Size = UDim2.new(0, 40, 0, 40)
minimizeBtn.Position = UDim2.new(1, -40, 0, 0)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
minimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
minimizeBtn.TextSize = 20
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.Text = "−"
minimizeBtn.Parent = mainPanel

-- Container dos Sliders
local container = Instance.new("Frame")
container.Name = "Container"
container.Size = UDim2.new(1, -10, 1, -50)
container.Position = UDim2.new(0, 5, 0, 45)
container.BackgroundTransparency = 1
container.Parent = mainPanel

-- Label Velocidade
local speedLabel = Instance.new("TextLabel")
speedLabel.Name = "SpeedLabel"
speedLabel.Size = UDim2.new(1, 0, 0, 25)
speedLabel.Position = UDim2.new(0, 0, 0, 10)
speedLabel.BackgroundTransparency = 1
speedLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
speedLabel.TextSize = 14
speedLabel.Font = Enum.Font.Gotham
speedLabel.Text = "🚀 VELOCIDADE: 1.0x"
speedLabel.TextXAlignment = Enum.TextXAlignment.Left
speedLabel.Parent = container

-- Slider Velocidade
local speedSlider = Instance.new("Frame")
speedSlider.Name = "SpeedSlider"
speedSlider.Size = UDim2.new(1, -10, 0, 8)
speedSlider.Position = UDim2.new(0, 5, 0, 40)
speedSlider.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
speedSlider.BorderColor3 = Color3.fromRGB(255, 0, 0)
speedSlider.BorderSizePixel = 1
speedSlider.Parent = container

local speedFill = Instance.new("Frame")
speedFill.Name = "Fill"
speedFill.Size = UDim2.new(0.2, 0, 1, 0)
speedFill.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
speedFill.BorderSizePixel = 0
speedFill.Parent = speedSlider

-- Label Pulo
local jumpLabel = Instance.new("TextLabel")
jumpLabel.Name = "JumpLabel"
jumpLabel.Size = UDim2.new(1, 0, 0, 25)
jumpLabel.Position = UDim2.new(0, 0, 0, 80)
jumpLabel.BackgroundTransparency = 1
jumpLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
jumpLabel.TextSize = 14
jumpLabel.Font = Enum.Font.Gotham
jumpLabel.Text = "⬆️ PULO: 50"
jumpLabel.TextXAlignment = Enum.TextXAlignment.Left
jumpLabel.Parent = container

-- Slider Pulo
local jumpSlider = Instance.new("Frame")
jumpSlider.Name = "JumpSlider"
jumpSlider.Size = UDim2.new(1, -10, 0, 8)
jumpSlider.Position = UDim2.new(0, 5, 0, 110)
jumpSlider.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
jumpSlider.BorderColor3 = Color3.fromRGB(255, 0, 0)
jumpSlider.BorderSizePixel = 1
jumpSlider.Parent = container

local jumpFill = Instance.new("Frame")
jumpFill.Name = "Fill"
jumpFill.Size = UDim2.new(0.25, 0, 1, 0)
jumpFill.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
jumpFill.BorderSizePixel = 0
jumpFill.Parent = jumpSlider

-- Label Status
local statusLabel = Instance.new("TextLabel")
statusLabel.Name = "Status"
statusLabel.Size = UDim2.new(1, 0, 0, 20)
statusLabel.Position = UDim2.new(0, 0, 0, 160)
statusLabel.BackgroundTransparency = 1
statusLabel.TextColor3 = Color3.fromRGB(100, 100, 100)
statusLabel.TextSize = 12
statusLabel.Font = Enum.Font.Gotham
statusLabel.Text = "Status: Ativo"
statusLabel.TextXAlignment = Enum.TextXAlignment.Left
statusLabel.Parent = container

-- Info Mobile
local infoLabel = Instance.new("TextLabel")
infoLabel.Name = "Info"
infoLabel.Size = UDim2.new(1, 0, 0, 80)
infoLabel.Position = UDim2.new(0, 0, 0, 200)
infoLabel.BackgroundTransparency = 1
infoLabel.TextColor3 = Color3.fromRGB(150, 150, 150)
infoLabel.TextSize = 11
infoLabel.Font = Enum.Font.Gotham
infoLabel.TextWrapped = true
infoLabel.Text = "📱 Deslize os controles\n\n🔘 Toque para minimizar\n\n⌨️ Pressione K para toggle"
infoLabel.TextXAlignment = Enum.TextXAlignment.Center
infoLabel.TextYAlignment = Enum.TextYAlignment.Top
infoLabel.Parent = container

-- Função para atualizar velocidade
local function updateSpeed(value)
	speedMultiplier = math.clamp(value, 1, 6)
	speedLabel.Text = "🚀 VELOCIDADE: " .. string.format("%.1f", speedMultiplier) .. "x"
	speedFill.Size = UDim2.new(((speedMultiplier - 1) / 5), 0, 1, 0)
end

-- Função para atualizar pulo
local function updateJump(value)
	jumpPower = math.clamp(value, 50, 200)
	jumpLabel.Text = "⬆️ PULO: " .. tostring(math.floor(jumpPower))
	jumpFill.Size = UDim2.new(((jumpPower - 50) / 150), 0, 1, 0)
end

-- Função para minimizar/abrir
local function toggleMinimize()
	isMinimized = not isMinimized
	if isMinimized then
		container.Visible = false
		minimizeBtn.Text = "+"
		mainPanel.Size = UDim2.new(0, 280, 0, 40)
	else
		container.Visible = true
		minimizeBtn.Text = "−"
		mainPanel.Size = UDim2.new(0, 280, 0, 320)
	end
end

-- Controles de toque para Sliders (Mobile)
local speedTouching = false
local jumpTouching = false

speedSlider.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.UserInputType == Enum.UserInputType.Touch then
		speedTouching = true
	end
end)

speedSlider.InputEnded:Connect(function(input, gameProcessed)
	if input.UserInputType == Enum.UserInputType.Touch then
		speedTouching = false
	end
end)

jumpSlider.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.UserInputType == Enum.UserInputType.Touch then
		jumpTouching = true
	end
end)

jumpSlider.InputEnded:Connect(function(input, gameProcessed)
	if input.UserInputType == Enum.UserInputType.Touch then
		jumpTouching = false
	end
end)

-- Atualizar posição ao deslizar (Mobile)
UserInputService.InputChanged:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	
	if input.UserInputType == Enum.UserInputType.Touch then
		local touchPosition = input.Position
		
		if speedTouching then
			local relativeX = touchPosition.X - speedSlider.AbsolutePosition.X
			local percentage = math.clamp(relativeX / speedSlider.AbsoluteSize.X, 0, 1)
			updateSpeed(1 + (percentage * 5))
		end
		
		if jumpTouching then
			local relativeX = touchPosition.X - jumpSlider.AbsolutePosition.X
			local percentage = math.clamp(relativeX / jumpSlider.AbsoluteSize.X, 0, 1)
			updateJump(50 + (percentage * 150))
		end
	end
end)

-- Botão minimizar
minimizeBtn.MouseButton1Click:Connect(toggleMinimize)

-- Atalho K
UserInputService.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.K then
		toggleMinimize()
	end
end)

-- Aplicar velocidade
RunService.RenderStepped:Connect(function()
	if character and humanoid.Health > 0 then
		local moveDirection = Vector3.new(0, 0, 0)
		
		if UserInputService:IsKeyDown(Enum.KeyCode.W) then
			moveDirection = moveDirection + (character.HumanoidRootPart.CFrame.LookVector * speedMultiplier)
		end
		if UserInputService:IsKeyDown(Enum.KeyCode.A) then
			moveDirection = moveDirection - (character.HumanoidRootPart.CFrame.RightVector * speedMultiplier)
		end
		if UserInputService:IsKeyDown(Enum.KeyCode.S) then
			moveDirection = moveDirection - (character.HumanoidRootPart.CFrame.LookVector * speedMultiplier)
		end
		if UserInputService:IsKeyDown(Enum.KeyCode.D) then
			moveDirection = moveDirection + (character.HumanoidRootPart.CFrame.RightVector * speedMultiplier)
		end
		
		if moveDirection.Magnitude > 0 then
			character.HumanoidRootPart.Velocity = Vector3.new(
				moveDirection.X,
				character.HumanoidRootPart.Velocity.Y,
				moveDirection.Z
			)
		end
	end
end)

-- Aplicar pulo
humanoid.Jumping:Connect(function()
	if character then
		character.HumanoidRootPart.Velocity = Vector3.new(
			character.HumanoidRootPart.Velocity.X,
			jumpPower,
			character.HumanoidRootPart.Velocity.Z
		)
	end
end)

-- Atualizar ao respawnar
player.CharacterAdded:Connect(function(newCharacter)
	character = newCharacter
	humanoid = character:WaitForChild("Humanoid")
	rootPart = character:WaitForChild("HumanoidRootPart")
	statusLabel.Text = "Status: Ativo (Respawned)"
end)

print("✅ MT SCRIPT - Versão Mobile carregado com sucesso!")

