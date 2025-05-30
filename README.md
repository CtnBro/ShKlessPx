loadstring([[
local plr = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "Tr1X_MacabraUI"

local uis = game:GetService("UserInputService")
local runService = game:GetService("RunService")

-- Criar som simples para clique
local sound = Instance.new("Sound", gui)
sound.SoundId = "rbxassetid://4590657391"
sound.Volume = 1

local function showLoading(button)
	button._originalText = button.Text
	button.Text = "Carregando..."
	wait(0.8)
	button.Text = button._originalText
	sound:Play()
end

local function makeLabel(text, parent)
	local l = Instance.new("TextLabel", parent)
	l.BackgroundColor3 = Color3.new(0, 0, 0)
	l.TextColor3 = Color3.new(1, 1, 1)
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
frame.BackgroundColor3 = Color3.new(0, 0, 0)
frame.BorderColor3 = Color3.new(1, 1, 1)
frame.BorderSizePixel = 2

local title = makeLabel("💀 Tr1X MENU MACABRO 💀", frame)
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundColor3 = Color3.new(0, 0, 0)
title.BorderSizePixel = 0
title.TextColor3 = Color3.new(1, 1, 1)

local miniBtn = Instance.new("TextButton", title)
miniBtn.Size = UDim2.new(0, 30, 1, 0)
miniBtn.Position = UDim2.new(1, -30, 0, 0)
miniBtn.Text = "-"
miniBtn.TextColor3 = Color3.new(1, 1, 1)
miniBtn.BackgroundColor3 = Color3.new(0, 0, 0)
miniBtn.Font = Enum.Font.SourceSansBold
miniBtn.TextScaled = true
miniBtn.BorderSizePixel = 1
miniBtn.BorderColor3 = Color3.new(1,1,1)

local tabs = Instance.new("Frame", frame)
tabs.Size = UDim2.new(0, 100, 1, -40)
tabs.Position = UDim2.new(0, 0, 0, 40)
tabs.BackgroundColor3 = Color3.new(0, 0, 0)
tabs.BorderSizePixel = 1
tabs.BorderColor3 = Color3.new(1, 1, 1)

local content = Instance.new("Frame", frame)
content.Size = UDim2.new(1, -100, 1, -40)
content.Position = UDim2.new(0, 100, 0, 40)
content.BackgroundColor3 = Color3.new(0, 0, 0)
content.BorderSizePixel = 1
content.BorderColor3 = Color3.new(1, 1, 1)

local panels = {}
local buttons = {}

local function createTab(name)
	local btn = Instance.new("TextButton", tabs)
	btn.Size = UDim2.new(1, 0, 0, 30)
	btn.Text = name
	btn.TextColor3 = Color3.new(1, 1, 1)
	btn.Font = Enum.Font.SourceSansBold
	btn.BackgroundColor3 = Color3.new(0, 0, 0)
	btn.BorderSizePixel = 1
	btn.BorderColor3 = Color3.new(1, 1, 1)
	btn.TextScaled = true
	
	buttons[name] = btn
	
	local panel = Instance.new("Frame", content)
	panel.Name = name
	panel.Size = UDim2.new(1, 0, 1, 0)
	panel.BackgroundTransparency = 1
	panel.Visible = false
	panels[name] = panel
	
	btn.MouseButton1Click:Connect(function()
		showLoading(btn)
		for _, v in pairs(panels) do v.Visible = false end
		panel.Visible = true
	end)
end

createTab("Home")
createTab("Risk")
createTab("Fun")

panels["Home"].Visible = true

local minimized = false
miniBtn.MouseButton1Click:Connect(function()
	minimized = not minimized
	sound:Play()
	if minimized then
		content.Visible = false
		tabs.Visible = false
		frame.Size = UDim2.new(0, 400, 0, 40)
		miniBtn.Text = "+"
	else
		content.Visible = true
		tabs.Visible = true
		frame.Size = UDim2.new(0, 400, 0, 300)
		miniBtn.Text = "-"
	end
end)

local dragging = false
local dragInput
local dragStart
local startPos

title.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = true
		dragStart = input.Position
		startPos = frame.Position
		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

title.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement then
		dragInput = input
	end
end)

uis.InputChanged:Connect(function(input)
	if input == dragInput and dragging then
		local delta = input.Position - dragStart
		frame.Position = UDim2.new(
			startPos.X.Scale,
			startPos.X.Offset + delta.X,
			startPos.Y.Scale,
			startPos.Y.Offset + delta.Y
		)
	end
end)
]])()
