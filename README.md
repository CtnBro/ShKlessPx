-- Tr1X Menu Macabro vFinalizado 💀
local plr = game.Players.LocalPlayer
local sheckles = plr:FindFirstChild("Sheckles") or plr:WaitForChild("Sheckles")
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "Tr1X_MacabraUI"
local uis = game:GetService("UserInputService")
local runService = game:GetService("RunService")
local sound = Instance.new("Sound", gui)
sound.SoundId = "rbxassetid://4590657391"
sound.Volume = 1

-- ESTILO SUJO
local function makeLabel(text, parent)
	local l = Instance.new("TextLabel", parent)
	l.BackgroundColor3 = Color3.fromRGB(30, 0, 30)
	l.TextColor3 = Color3.fromRGB(255, 0, 255)
	l.Font = Enum.Font.Arcade
	l.TextScaled = true
	l.Size = UDim2.new(1, 0, 0, 30)
	l.BorderSizePixel = 0
	l.Text = text
	return l
end

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 400, 0, 300)
frame.Position = UDim2.new(0.5, -200, 0.5, -150)
frame.BackgroundColor3 = Color3.fromRGB(15, 0, 15)
frame.BorderSizePixel = 0

local title = makeLabel("💀 Tr1X MENU MACABRO 💀", frame)
title.Size = UDim2.new(1, 0, 0, 40)

local tabs = Instance.new("Frame", frame)
tabs.Size = UDim2.new(0, 100, 1, -40)
tabs.Position = UDim2.new(0, 0, 0, 40)
tabs.BackgroundColor3 = Color3.fromRGB(40, 0, 40)

local content = Instance.new("Frame", frame)
content.Size = UDim2.new(1, -100, 1, -40)
content.Position = UDim2.new(0, 100, 0, 40)
content.BackgroundColor3 = Color3.fromRGB(20, 0, 20)

-- Minimizar botão
local miniBtn = Instance.new("TextButton", title)
miniBtn.Size = UDim2.new(0, 30, 1, 0)
miniBtn.Position = UDim2.new(1, -30, 0, 0)
miniBtn.Text = "-"
miniBtn.TextColor3 = Color3.new(1, 0, 1)
miniBtn.BackgroundColor3 = Color3.fromRGB(50, 0, 50)
miniBtn.Font = Enum.Font.SourceSansBold
miniBtn.TextScaled = true
miniBtn.MouseButton1Click:Connect(function()
	content.Visible = not content.Visible
	sound:Play()
end)

-- Cria abas e painéis
local panels = {}
local function createTab(name)
	local btn = Instance.new("TextButton", tabs)
	btn.Size = UDim2.new(1, 0, 0, 30)
	btn.Text = name
	btn.TextColor3 = Color3.new(1, 0, 1)
	btn.Font = Enum.Font.SourceSansBold
	btn.BackgroundColor3 = Color3.fromRGB(50, 0, 50)
	btn.TextScaled = true

	local panel = Instance.new("Frame", content)
	panel.Name = name
	panel.Size = UDim2.new(1, 0, 1, 0)
	panel.BackgroundTransparency = 1
	panel.Visible = false
	panels[name] = panel

	btn.MouseButton1Click:Connect(function()
		for k, v in pairs(panels) do v.Visible = false end
		panel.Visible = true
		sound:Play()
	end)
end

createTab("Home")
createTab("Risk")

-- Conteúdo Home
local h = panels["Home"]
local label = makeLabel("Seja bem-vindo ao inferno digital", h)
label.Size = UDim2.new(1, 0, 0, 50)

local insult = makeLabel("Seu liso fudido, vai farmar", h)
insult.Position = UDim2.new(0, 0, 0, 60)
insult.Size = UDim2.new(1, 0, 0, 50)

-- Conteúdo Risk
local r = panels["Risk"]
local lazyBtn = Instance.new("TextButton", r)
lazyBtn.Size = UDim2.new(1, -20, 0, 50)
lazyBtn.Position = UDim2.new(0, 10, 0, 10)
lazyBtn.Text = "Sou Preguiçoso (Ativar)"
lazyBtn.TextColor3 = Color3.new(1, 0, 1)
lazyBtn.Font = Enum.Font.SourceSansBold
lazyBtn.TextScaled = true
lazyBtn.BackgroundColor3 = Color3.fromRGB(50, 0, 50)

-- Sistema de farm nojento
local loopActive = false
local loop

lazyBtn.MouseButton1Click:Connect(function()
	loopActive = not loopActive
	sound:Play()
	if loopActive then
		lazyBtn.Text = "Sou Preguiçoso (Desativar)"
		game.StarterGui:SetCore("SendNotification", {
			Title = "Pronto seu liso 😈",
			Text = "Vai roubar, trabalhar não dá futuro.",
			Duration = 4
		})
		loop = runService.RenderStepped:Connect(function(dt)
			if tick() % 2 < 0.03 then
				if sheckles and sheckles.Value then
					sheckles.Value = sheckles.Value + 5000000
				end
			end
		end)
	else
		lazyBtn.Text = "Sou Preguiçoso (Ativar)"
		if loop then loop:Disconnect() end
	end
end)

-- Mostra aba inicial
panels["Home"].Visible = true
