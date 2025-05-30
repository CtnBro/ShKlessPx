-- Tr1X Menu Macabro FINAL 💀
-- Criado para o jogo Grow a Garden
-- https://www.roblox.com/games/13451391903/

local plr = game.Players.LocalPlayer
local mouse = plr:GetMouse()
local uis = game:GetService("UserInputService")
local runService = game:GetService("RunService")
local rs = game:GetService("ReplicatedStorage")

local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "Tr1X_VirusGUI"

local dragging, dragInput, dragStart, startPos

-- Som macabro
local sound = Instance.new("Sound", gui)
sound.SoundId = "rbxassetid://4590657391"
sound.Volume = 1

-- Função de animação texto
local function animate(btn, txt)
	btn.Text = "Carregando..."
	task.wait(0.7)
	btn.Text = txt
end

-- Criar base da UI
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 420, 0, 320)
frame.Position = UDim2.new(0.5, -210, 0.5, -160)
frame.BackgroundColor3 = Color3.new(0, 0, 0)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundColor3 = Color3.new(1, 1, 1)
title.TextColor3 = Color3.new(0, 0, 0)
title.Font = Enum.Font.Code
title.TextScaled = true
title.Text = "💀 Tr1X MENU MACABRO 💀"

-- Minimizar
local mini = Instance.new("TextButton", title)
mini.Size = UDim2.new(0, 30, 1, 0)
mini.Position = UDim2.new(1, -30, 0, 0)
mini.Text = "-"
mini.BackgroundColor3 = Color3.new(0, 0, 0)
mini.TextColor3 = Color3.new(1, 1, 1)
mini.Font = Enum.Font.Code
mini.TextScaled = true

local tabs = Instance.new("Frame", frame)
tabs.Size = UDim2.new(0, 100, 1, -40)
tabs.Position = UDim2.new(0, 0, 0, 40)
tabs.BackgroundColor3 = Color3.fromRGB(20, 20, 20)

local content = Instance.new("Frame", frame)
content.Size = UDim2.new(1, -100, 1, -40)
content.Position = UDim2.new(0, 100, 0, 40)
content.BackgroundColor3 = Color3.fromRGB(10, 10, 10)

local panels = {}

local function createTab(name)
	local btn = Instance.new("TextButton", tabs)
	btn.Size = UDim2.new(1, 0, 0, 30)
	btn.Text = name
	btn.Font = Enum.Font.Code
	btn.TextColor3 = Color3.new(1,1,1)
	btn.BackgroundColor3 = Color3.fromRGB(30,30,30)
	btn.TextScaled = true

	local panel = Instance.new("Frame", content)
	panel.Name = name
	panel.Size = UDim2.new(1, 0, 1, 0)
	panel.BackgroundTransparency = 1
	panel.Visible = false
	panels[name] = panel

	btn.MouseButton1Click:Connect(function()
		for _, p in pairs(panels) do p.Visible = false end
		panel.Visible = true
		sound:Play()
	end)
end

createTab("Home")
createTab("Risk")
createTab("Fun")

-- Minimizar funcionamento
mini.MouseButton1Click:Connect(function()
	content.Visible = not content.Visible
	sound:Play()
end)

-- Aba Home
local home = panels["Home"]
local l1 = Instance.new("TextLabel", home)
l1.Size = UDim2.new(1, 0, 0, 60)
l1.BackgroundTransparency = 1
l1.TextColor3 = Color3.new(1, 1, 1)
l1.Font = Enum.Font.Code
l1.TextScaled = true
l1.Text = "Seja bem-vindo ao inferno digital"

-- Aba Risk - farm de Sheckles
local risk = panels["Risk"]
local sheckBtn = Instance.new("TextButton", risk)
sheckBtn.Size = UDim2.new(1, -20, 0, 50)
sheckBtn.Position = UDim2.new(0, 10, 0, 10)
sheckBtn.Text = "Sou Preguiçoso (Farm)"
sheckBtn.TextColor3 = Color3.new(1,1,1)
sheckBtn.BackgroundColor3 = Color3.fromRGB(20,20,20)
sheckBtn.Font = Enum.Font.Code
sheckBtn.TextScaled = true

local activeFarm = false
local remote = rs:FindFirstChild("Spawn")

sheckBtn.MouseButton1Click:Connect(function()
	if not remote then return end
	activeFarm = not activeFarm
	sound:Play()
	animate(sheckBtn, "Sou Preguiçoso (Farm)")
	if activeFarm then
		sheckBtn.Text = "Farm ON"
		task.spawn(function()
			while activeFarm do
				remote:FireServer("Sheckles", 5000000)
				task.wait(2)
			end
		end)
	else
		sheckBtn.Text = "Sou Preguiçoso (Farm)"
	end
end)

-- Aba Fun
local fun = panels["Fun"]
-- Voo
local flying = false
local flyBtn = Instance.new("TextButton", fun)
flyBtn.Size = UDim2.new(1, -20, 0, 50)
flyBtn.Position = UDim2.new(0, 10, 0, 10)
flyBtn.Text = "Ativar Voo (CTRL)"
flyBtn.TextColor3 = Color3.new(1,1,1)
flyBtn.BackgroundColor3 = Color3.fromRGB(20,20,20)
flyBtn.Font = Enum.Font.Code
flyBtn.TextScaled = true

local bp, bg

uis.InputBegan:Connect(function(input)
	if input.KeyCode == Enum.KeyCode.LeftControl and flying then
		flying = false
		if bp then bp:Destroy() end
		if bg then bg:Destroy() end
	else
		if input.KeyCode == Enum.KeyCode.LeftControl and not flying then
			flying = true
			bp = Instance.new("BodyPosition", plr.Character.HumanoidRootPart)
			bg = Instance.new("BodyGyro", plr.Character.HumanoidRootPart)
			bp.MaxForce = Vector3.new(1e9, 1e9, 1e9)
			bg.MaxTorque = Vector3.new(1e9, 1e9, 1e9)
			runService.RenderStepped:Connect(function()
				if flying then
					bp.Position = plr.Character.HumanoidRootPart.Position + Vector3.new(
						(uis:IsKeyDown(Enum.KeyCode.D) and 5 or 0) - (uis:IsKeyDown(Enum.KeyCode.A) and 5 or 0),
						(uis:IsKeyDown(Enum.KeyCode.Space) and 5 or 0),
						(uis:IsKeyDown(Enum.KeyCode.S) and 5 or 0) - (uis:IsKeyDown(Enum.KeyCode.W) and 5 or 0)
					)
					bg.CFrame = plr.Character.HumanoidRootPart.CFrame
				end
			end)
		end
	end
end)

-- Duplicar fruta na mão
local dupBtn = Instance.new("TextButton", fun)
dupBtn.Size = UDim2.new(1, -20, 0, 50)
dupBtn.Position = UDim2.new(0, 10, 0, 70)
dupBtn.Text = "Duplicar Item na Mão"
dupBtn.TextColor3 = Color3.new(1,1,1)
dupBtn.BackgroundColor3 = Color3.fromRGB(20,20,20)
dupBtn.Font = Enum.Font.Code
dupBtn.TextScaled = true

dupBtn.MouseButton1Click:Connect(function()
	local item = plr.Character:FindFirstChildOfClass("Tool")
	if not item then return end
	local id = tonumber(item.Name:match("%d+")) or 0
	local newID = id + 1
	local clone = item:Clone()
	clone.Name = item.Name:gsub(tostring(id), tostring(newID))
	clone.Parent = plr.Backpack
	animate(dupBtn, "Duplicar Item na Mão")
end)

-- Mostrar a aba Home por padrão
panels["Home"].Visible = true
