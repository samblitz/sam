-- Script de menu com botão menor, arrastável e imagem de fundo no botão Menu

local player = game.Players.LocalPlayer
local userInputService = game:GetService("UserInputService")
local playerGui = player:WaitForChild("PlayerGui")

-- Criação da GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "CollisionMenu"
screenGui.Parent = playerGui

-- Frame para o botão Menu (para colocar imagem de fundo e texto)
local menuButtonFrame = Instance.new("Frame")
menuButtonFrame.Size = UDim2.new(0, 80, 0, 30) -- botão menor
menuButtonFrame.Position = UDim2.new(0, 10, 0, 10)
menuButtonFrame.BackgroundTransparency = 1
menuButtonFrame.Parent = screenGui
menuButtonFrame.Active = true
menuButtonFrame.Draggable = true -- para ser arrastável

-- Imagem de fundo do botão Menu
local menuImage = Instance.new("ImageLabel")
menuImage.Size = UDim2.new(1, 0, 1, 0)
menuImage.Position = UDim2.new(0, 0, 0, 0)
menuImage.BackgroundTransparency = 1
menuImage.Image = "rbxassetid://12806098155" -- imagem espaço com planetas e estrelas (exemplo)
menuImage.Parent = menuButtonFrame

-- Botão Menu (texto sobre a imagem)
local toggleMenuButton = Instance.new("TextButton")
toggleMenuButton.Size = UDim2.new(1, 0, 1, 0)
toggleMenuButton.BackgroundTransparency = 1
toggleMenuButton.Text = "Menu"
toggleMenuButton.TextColor3 = Color3.new(1, 1, 1)
toggleMenuButton.Font = Enum.Font.GothamBold
toggleMenuButton.TextScaled = true
toggleMenuButton.Parent = menuButtonFrame

-- Frame do menu (inicialmente oculto)
local menuFrame = Instance.new("Frame")
menuFrame.Size = UDim2.new(0, 150, 0, 100)
menuFrame.Position = UDim2.new(0, 10, 0, 50)
menuFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
menuFrame.Visible = false
menuFrame.Parent = screenGui
menuFrame.ClipsDescendants = true
menuFrame.Active = true
menuFrame.Draggable = true -- para poder mover o menu também

-- Botão para ativar/desativar colisão
local collisionToggleButton = Instance.new("TextButton")
collisionToggleButton.Size = UDim2.new(1, -20, 0, 40)
collisionToggleButton.Position = UDim2.new(0, 10, 0, 10)
collisionToggleButton.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
collisionToggleButton.TextColor3 = Color3.new(1, 1, 1)
collisionToggleButton.Font = Enum.Font.GothamBold
collisionToggleButton.TextScaled = true
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
