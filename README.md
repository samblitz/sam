local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local hrp = character:WaitForChild("HumanoidRootPart")

-- GUI principal
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "SamblitzHub"
gui.ResetOnSpawn = false

-- Painel base
local panel = Instance.new("Frame", gui)
panel.Size = UDim2.new(0, 400, 0, 300)
panel.Position = UDim2.new(0.5, -200, 0.5, -150)
panel.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
panel.BorderSizePixel = 0
panel.Active = true
panel.Draggable = true -- 🔄 Tornar arrastável

-- Título
local title = Instance.new("TextLabel", panel)
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
title.Text = "SamblitzHub"
title.TextColor3 = Color3.new(1, 1, 1)
title.Font = Enum.Font.SourceSansBold
title.TextSize = 22

-- Botão Fechar
local closeBtn = Instance.new("TextButton", panel)
closeBtn.Size = UDim2.new(0, 60, 0, 30)
closeBtn.Position = UDim2.new(1, -65, 0, 5)
closeBtn.Text = "Fechar"
closeBtn.BackgroundColor3 = Color3.fromRGB(255, 60, 60)
closeBtn.TextColor3 = Color3.new(1, 1, 1)
closeBtn.Font = Enum.Font.SourceSansBold
closeBtn.TextSize = 16
closeBtn.MouseButton1Click:Connect(function()
	panel.Visible = false
end)

-- Tabs
local tabNames = {"Main", "Auto Farm"}
local tabFrames = {}

local function createTabButton(name, index)
	local btn = Instance.new("TextButton", panel)
	btn.Size = UDim2.new(0, 100, 0, 30)
	btn.Position = UDim2.new(0, 10 + (index * 105), 0, 50)
	btn.Text = name
	btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	btn.TextColor3 = Color3.new(1, 1, 1)
	btn.Font = Enum.Font.SourceSansBold
	btn.TextSize = 16
	btn.MouseButton1Click:Connect(function()
		for i, frame in pairs(tabFrames) do
			frame.Visible = i == name
		end
	end)
end

-- Criar abas
for i, name in ipairs(tabNames) do
	local frame = Instance.new("Frame", panel)
	frame.Size = UDim2.new(1, -20, 1, -90)
	frame.Position = UDim2.new(0, 10, 0, 90)
	frame.BackgroundTransparency = 1
	frame.Visible = i == 1
	tabFrames[name] = frame
	createTabButton(name, i - 1)
end

-- === MAIN TAB ===
local mainTab = tabFrames["Main"]

local function createInput(labelText, defaultValue, posY, onChange)
	local label = Instance.new("TextLabel", mainTab)
	label.Position = UDim2.new(0, 0, 0, posY)
	label.Size = UDim2.new(0, 180, 0, 30)
	label.Text = labelText
	label.TextColor3 = Color3.new(1, 1, 1)
	label.BackgroundTransparency = 1
	label.Font = Enum.Font.SourceSans
	label.TextSize = 16

	local box = Instance.new("TextBox", mainTab)
	box.Position = UDim2.new(0, 190, 0, posY)
	box.Size = UDim2.new(0, 100, 0, 30)
	box.Text = tostring(defaultValue)
	box.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
	box.TextColor3 = Color3.new(1, 1, 1)
	box.Font = Enum.Font.SourceSansBold
	box.TextSize = 16
	box.FocusLost:Connect(function()
		local value = tonumber(box.Text)
		if value then onChange(value) end
	end)
end

-- Velocidade e Pulo
createInput("Velocidade:", humanoid.WalkSpeed, 0, function(v)
	humanoid.WalkSpeed = v
end)

createInput("Pulo:", humanoid.JumpPower, 40, function(v)
	humanoid.JumpPower = v
end)

-- Anti-reset
local noReset = Instance.new("TextButton", mainTab)
noReset.Position = UDim2.new(0, 0, 0, 80)
noReset.Size = UDim2.new(0, 290, 0, 30)
noReset.Text = "Impedir reset após comer"
noReset.BackgroundColor3 = Color3.fromRGB(120, 120, 120)
noReset.TextColor3 = Color3.new(1, 1, 1)
noReset.Font = Enum.Font.SourceSansBold
noReset.TextSize = 16

noReset.MouseButton1Click:Connect(function()
	player.CharacterRemoving:Connect(function(char)
		local clone = char:Clone()
		wait()
		player.Character = clone
	end)
	noReset.Text = "Proteção ativada ✅"
	noReset.BackgroundColor3 = Color3.fromRGB(80, 200, 80)
end)

-- === AUTO FARM TAB ===
local farmTab = tabFrames["Auto Farm"]

local teleportBtn = Instance.new("TextButton", farmTab)
teleportBtn.Position = UDim2.new(0, 0, 0, 0)
teleportBtn.Size = UDim2.new(0, 300, 0, 40)
teleportBtn.Text = "Teleportar para Auto Farm"
teleportBtn.BackgroundColor3 = Color3.fromRGB(100, 100, 200)
teleportBtn.TextColor3 = Color3.new(1, 1, 1)
teleportBtn.Font = Enum.Font.SourceSansBold
teleportBtn.TextSize = 18

-- Coordenadas do auto-farm
local autoFarmPos = Vector3.new(0, 200, 0)

teleportBtn.MouseButton1Click:Connect(function()
	hrp.CFrame = CFrame.new(autoFarmPos)
end)
