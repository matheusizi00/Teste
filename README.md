local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local rootPart = character:WaitForChild("HumanoidRootPart")

-- Variáveis
local speedMultiplier = 1
local jumpMultiplier = 1
local isMinimized = false
local bodyVelocity

-- Criar BodyVelocity para controlar velocidade
local function createBodyVelocity()
    if bodyVelocity then bodyVelocity:Destroy() end
    bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.Velocity = Vector3.new(0, 0, 0)
    bodyVelocity.MaxForce = Vector3.new(math.huge, 0, math.huge)
    bodyVelocity.Damping = 0
    bodyVelocity.Parent = rootPart
end

createBodyVelocity()

-- Criar GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "MTScript"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Frame Principal
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 300, 0, 250)
mainFrame.Position = UDim2.new(0, 10, 0, 10)
mainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
mainFrame.BorderColor3 = Color3.fromRGB(255, 0, 0)
mainFrame.BorderSizePixel = 3
mainFrame.Parent = screenGui

-- Título
local title = Instance.new("TextLabel")
title.Name = "Title"
title.Size = UDim2.new(1, 0, 0, 40)
title.Position = UDim2.new(0, 0, 0, 0)
title.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
title.TextColor3 = Color3.fromRGB(255, 0, 0)
title.TextSize = 18
title.Font = Enum.Font.GothamBold
title.Text = "🔴 MT SCRIPT 🔴"
title.Parent = mainFrame

-- Botão Minimizar
local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Name = "MinimizeBtn"
minimizeBtn.Size = UDim2.new(0, 30, 0, 30)
minimizeBtn.Position = UDim2.new(1, -35, 0, 5)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
minimizeBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
minimizeBtn.TextSize = 16
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.Text = "−"
minimizeBtn.Parent = mainFrame

-- Label Velocidade
local speedLabel = Instance.new("TextLabel")
speedLabel.Size = UDim2.new(1, 0, 0, 25)
speedLabel.Position = UDim2.new(0, 10, 0, 50)
speedLabel.BackgroundTransparency = 1
speedLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
speedLabel.TextSize = 14
speedLabel.Font = Enum.Font.Gotham
speedLabel.TextXAlignment = Enum.TextXAlignment.Left
speedLabel.Text = "🚀 Velocidade: 1x"
speedLabel.Parent = mainFrame

-- Slider Velocidade
local speedSlider = Instance.new("Frame")
speedSlider.Size = UDim2.new(0, 250, 0, 15)
speedSlider.Position = UDim2.new(0, 25, 0, 75)
speedSlider.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
speedSlider.BorderColor3 = Color3.fromRGB(255, 0, 0)
speedSlider.BorderSizePixel = 2
speedSlider.Parent = mainFrame

local speedButton = Instance.new("TextButton")
speedButton.Size = UDim2.new(0, 15, 0, 15)
speedButton.Position = UDim2.new(0, 0, 0, 0)
speedButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
speedButton.BorderSizePixel = 0
speedButton.Text = ""
speedButton.Parent = speedSlider

-- Label Pulo
local jumpLabel = Instance.new("TextLabel")
jumpLabel.Size = UDim2.new(1, 0, 0, 25)
jumpLabel.Position = UDim2.new(0, 10, 0, 110)
jumpLabel.BackgroundTransparency = 1
jumpLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
jumpLabel.TextSize = 14
jumpLabel.Font = Enum.Font.Gotham
jumpLabel.TextXAlignment = Enum.TextXAlignment.Left
jumpLabel.Text = "⬆️ Pulo: 1x"
jumpLabel.Parent = mainFrame

-- Slider Pulo
local jumpSlider = Instance.new("Frame")
jumpSlider.Size = UDim2.new(0, 250, 0, 15)
jumpSlider.Position = UDim2.new(0, 25, 0, 135)
jumpSlider.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
jumpSlider.BorderColor3 = Color3.fromRGB(255, 0, 0)
jumpSlider.BorderSizePixel = 2
jumpSlider.Parent = mainFrame

local jumpButton = Instance.new("TextButton")
jumpButton.Size = UDim2.new(0, 15, 0, 15)
jumpButton.Position = UDim2.new(0, 0, 0, 0)
jumpButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
jumpButton.BorderSizePixel = 0
jumpButton.Text = ""
jumpButton.Parent = jumpSlider

-- Status
local statusLabel = Instance.new("TextLabel")
statusLabel.Size = UDim2.new(1, -20, 0, 30)
statusLabel.Position = UDim2.new(0, 10, 1, -35)
statusLabel.BackgroundTransparency = 1
statusLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
statusLabel.TextSize = 12
statusLabel.Font = Enum.Font.Gotham
statusLabel.Text = "✓ Ativo"
statusLabel.Parent = mainFrame

-- Funções Slider
local function updateSpeedSlider()
    local percentage = (speedMultiplier - 1) / 5
    speedButton.Position = UDim2.new(percentage, -7.5, 0, 0)
    speedLabel.Text = "🚀 Velocidade: " .. string.format("%.1f", speedMultiplier) .. "x"
end

local function updateJumpSlider()
    local percentage = (jumpMultiplier - 1) / 4
    jumpButton.Position = UDim2.new(percentage, -7.5, 0, 0)
    jumpLabel.Text = "⬆️ Pulo: " .. string.format("%.1f", jumpMultiplier) .. "x"
end

-- Eventos dos Sliders
local draggingSpeed = false
local draggingJump = false

speedButton.MouseButton1Down:Connect(function()
    draggingSpeed = true
end)

jumpButton.MouseButton1Down:Connect(function()
    draggingJump = true
end)

UserInputService.InputEnded:Connect(function(input, gameProcessed)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        draggingSpeed = false
        draggingJump = false
    end
end)

UserInputService.InputChanged:Connect(function(input, gameProcessed)
    if draggingSpeed then
        local sliderPos = speedSlider.AbsolutePosition.X
        local sliderSize = speedSlider.AbsoluteSize.X
        local mousePos = UserInputService:GetMouseLocation().X
        local percentage = math.clamp((mousePos - sliderPos) / sliderSize, 0, 1)
        speedMultiplier = 1 + (percentage * 5)
        updateSpeedSlider()
    elseif draggingJump then
        local sliderPos = jumpSlider.AbsolutePosition.X
        local sliderSize = jumpSlider.AbsoluteSize.X
        local mousePos = UserInputService:GetMouseLocation().X
        local percentage = math.clamp((mousePos - sliderPos) / sliderSize, 0, 1)
        jumpMultiplier = 1 + (percentage * 4)
        updateJumpSlider()
    end
end)

-- Botão Minimizar
minimizeBtn.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    if isMinimized then
        mainFrame.Size = UDim2.new(0, 300, 0, 40)
        minimizeBtn.Text = "+"
    else
        mainFrame.Size = UDim2.new(0, 300, 0, 250)
        minimizeBtn.Text = "−"
    end
end)

-- Atalho Teclado K
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.K then
        isMinimized = not isMinimized
        if isMinimized then
            mainFrame.Size = UDim2.new(0, 300, 0, 40)
            minimizeBtn.Text = "+"
        else
            mainFrame.Size = UDim2.new(0, 300, 0, 250)
            minimizeBtn.Text = "−"
        end
    end
end)

-- ⭐ VELOCIDADE E PULO FUNCIONANDO PERFEITAMENTE ⭐
RunService.RenderStepped:Connect(function()
    if not character.Parent then return end
    
    -- SUPER PULO - Aplicar JumpHeight com multiplicador
    humanoid.JumpHeight = 7.2 * jumpMultiplier
    
    -- SUPER VELOCIDADE - Aplicar velocidade constante
    local moveDirection = humanoid.MoveDirection
    
    if moveDirection.Magnitude > 0 then
        local constantSpeed = 16 * speedMultiplier
        bodyVelocity.Velocity = moveDirection * constantSpeed
    else
        bodyVelocity.Velocity = Vector3.new(0, 0, 0)
    end
end)

-- Recriar ao spawnar
player.CharacterAdded:Connect(function(newCharacter)
    character = newCharacter
    humanoid = character:WaitForChild("Humanoid")
    rootPart = character:WaitForChild("HumanoidRootPart")
    createBodyVelocity()
end)

updateSpeedSlider()
updateJumpSlider()
