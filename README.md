-- Script de menu para ativar/desativar colisão no personagem

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

local userInputService = game:GetService("UserInputService")
local playerGui = player:WaitForChild("PlayerGui")

-- Criação da GUI

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "CollisionMenu"
screenGui.Parent = playerGui

-- Botão para abrir/fechar o menu
local toggleMenuButton = Instance.new("TextButton")
toggleMenuButton.Size = UDim2.new(0, 100, 0, 30)
toggleMenuButton.Position = UDim2.new(0, 10, 0, 10)
toggleMenuButton.Text = "Menu"
toggleMenuButton.Parent = screenGui

-- Frame do menu (inicialmente oculto)
local menuFrame = Instance.new("Frame")
menuFrame.Size = UDim2.new(0, 150, 0, 100)
menuFrame.Position = UDim2.new(0, 10, 0, 50)
menuFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
menuFrame.Visible = false
menuFrame.Parent = screenGui

-- Botão para ativar/desativar colisão
local collisionToggleButton = Instance.new("TextButton")
collisionToggleButton.Size = UDim2.new(1, -20, 0, 40)
collisionToggleButton.Position = UDim2.new(0, 10, 0, 10)
collisionToggleButton.Text = "Desativar Colisão"
collisionToggleButton.Parent = menuFrame

-- Estado do modo fantasma
local ghostMode = false

-- Função para alterar colisão
local function setCollision(state)
    local char = player.Character
    if not char then return end
    for _, part in pairs(char:GetChildren()) do
        if part:IsA("BasePart") then
            part.CanCollide = state
        end
    end
end

-- Toggle função do modo fantasma
local function toggleGhostMode()
    ghostMode = not ghostMode
    setCollision(not ghostMode)
    collisionToggleButton.Text = ghostMode and "Ativar Colisão" or "Desativar Colisão"
end

-- Evento do botão menu (mostrar/ocultar)
toggleMenuButton.MouseButton1Click:Connect(function()
    menuFrame.Visible = not menuFrame.Visible
end)

-- Evento do botão colisão
collisionToggleButton.MouseButton1Click:Connect(function()
    toggleGhostMode()
end)

-- Inicializa com colisão ativada
setCollision(true)
