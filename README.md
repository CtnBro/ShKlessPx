loadstring([[
local plr = game.Players.LocalPlayer
local sheckles = plr:WaitForChild("Sheckles")
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "Tr1X_MacabraUI"

local uis = game:GetService("UserInputService")
local runService = game:GetService("RunService")

-- Som de clique
local sound = Instance.new("Sound", gui)
sound.SoundId = "rbxassetid://4590657391"
sound.Volume = 1

-- Função para animação "carregando"
local function showLoading(button)
	button.Text = "Carregando..."
	wait(0.8)
	button.Text = button._originalText or button.Text
	sound:Play()
end

-- Função para criar label estilizado (preto e branco)
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

-- Criando a frame principal
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 400, 0, 300)
frame.Position = UDim2.new(0.5, -200, 0.5, -150)
frame.BackgroundColor3 = Color3.new(0, 0, 0)
frame.BorderColor3 = Color3.new(1, 1, 1)
frame.BorderSizePixel = 2

-- Barra de título (para arrastar)
local title = makeLabel("💀 Tr1X MENU MACABRO 💀", frame)
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundColor3 = Color3.new(0, 0, 0)
title.BorderSizePixel = 0
title.TextColor3 = Color3.new(1, 1, 1)

-- Botão minimizar
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

-- Container abas (tabs)
local tabs = Instance.new("Frame", frame)
tabs.Size = UDim2.new(0, 100, 1, -40)
tabs.Position = UDim2.new(0, 0, 0, 40)
tabs.BackgroundColor3 = Color3.new(0, 0, 0)
tabs.BorderSizePixel = 1
tabs.BorderColor3 = Color3.new(1, 1, 1)

-- Container conteúdo das abas
local content = Instance.new("Frame", frame)
content.Size = UDim2.new(1, -100, 1, -40)
content.Position = UDim2.new(0, 100, 0, 40)
content.BackgroundColor3 = Color3.new(0, 0, 0)
content.BorderSizePixel = 1
content.BorderColor3 = Color3.new(1, 1, 1)

-- Tabela para guardar painéis e botões
local panels = {}
local buttons = {}

-- Função criar aba e botão
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

-- Criar abas
createTab("Home")
createTab("Risk")
createTab("Fun")

-- Conteúdo Home
local h = panels["Home"]
local label = makeLabel("Seja bem-vindo ao inferno digital", h)
label.Size = UDim2.new(1, 0, 0, 50)

local insult = makeLabel("Seu liso fudido, vai farmar", h)
insult.Position = UDim2.new(0, 0, 0, 60)
insult.Size = UDim2.new(1, 0, 0, 50)

-- Conteúdo Risk (farming sheckles)
local r = panels["Risk"]
local lazyBtn = Instance.new("TextButton", r)
lazyBtn.Size = UDim2.new(1, -20, 0, 50)
lazyBtn.Position = UDim2.new(0, 10, 0, 10)
lazyBtn.Text = "Sou Preguiçoso (Ativar)"
lazyBtn.TextColor3 = Color3.new(1, 1, 1)
lazyBtn.Font = Enum.Font.SourceSansBold
lazyBtn.TextScaled = true
lazyBtn.BackgroundColor3 = Color3.new(0, 0, 0)
lazyBtn.BorderSizePixel = 1
lazyBtn.BorderColor3 = Color3.new(1, 1, 1)
lazyBtn._originalText = lazyBtn.Text

local loopActive = false
local loop

lazyBtn.MouseButton1Click:Connect(function()
	loopActive = not loopActive
	showLoading(lazyBtn)
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
					-- Para alterar o valor do servidor, deve-se usar RemoteEvent, aqui só altera local. 
					-- Então abaixo só simula a interface local.
					sheckles.Value = sheckles.Value + 5000000
				end
			end
		end)
	else
		lazyBtn.Text = "Sou Preguiçoso (Ativar)"
		if loop then loop:Disconnect() end
	end
end)

-- Conteúdo Fun (voar, esp, duplicar)
local f = panels["Fun"]

-- Voo
local flyBtn = Instance.new("TextButton", f)
flyBtn.Size = UDim2.new(1, -20, 0, 50)
flyBtn.Position = UDim2.new(0, 10, 0, 10)
flyBtn.Text = "Voo (Desativado)"
flyBtn.TextColor3 = Color3.new(1, 1, 1)
flyBtn.Font = Enum.Font.SourceSansBold
flyBtn.TextScaled = true
flyBtn.BackgroundColor3 = Color3.new(0, 0, 0)
flyBtn.BorderSizePixel = 1
flyBtn.BorderColor3 = Color3.new(1, 1, 1)
flyBtn._originalText = flyBtn.Text

local flying = false
local speed = 50
local bodyVelocity

flyBtn.MouseButton1Click:Connect(function()
	flying = not flying
	showLoading(flyBtn)
	if flying then
		flyBtn.Text = "Voo (Ativado)"
		bodyVelocity = Instance.new("BodyVelocity")
		bodyVelocity.Velocity = Vector3.new(0, 0, 0)
		bodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
		bodyVelocity.Parent = plr.Character.HumanoidRootPart
		
		local connection
		connection = runService.Heartbeat:Connect(function()
			if not flying then
				connection:Disconnect()
				bodyVelocity:Destroy()
				return
			end
			local direction = Vector3.new(0, 0, 0)
			if uis:IsKeyDown(Enum.KeyCode.W) then direction = direction + workspace.CurrentCamera.CFrame.LookVector end
			if uis:IsKeyDown(Enum.KeyCode.S) then direction = direction - workspace.CurrentCamera.CFrame.LookVector end
			if uis:IsKeyDown(Enum.KeyCode.A) then direction = direction - workspace.CurrentCamera.CFrame.RightVector end
			if uis:IsKeyDown(Enum.KeyCode.D) then direction = direction + workspace.CurrentCamera.CFrame.RightVector end
			if uis:IsKeyDown(Enum.KeyCode.Space) then direction = direction + Vector3.new(0, 1, 0) end
			if uis:IsKeyDown(Enum.KeyCode.LeftControl) then direction = direction - Vector3.new(0, 1, 0) end
			
			bodyVelocity.Velocity = direction.Unit * speed
		end)
	else
		flyBtn.Text = "Voo (Desativado)"
		if bodyVelocity then bodyVelocity:Destroy() end
	end
end)

-- ESP simples (exibir nome dos jogadores)
local espBtn = Instance.new("TextButton", f)
espBtn.Size = UDim2.new(1, -20, 0, 50)
espBtn.Position = UDim2.new(0, 10, 0, 70)
espBtn.Text = "ESP (Desativado)"
espBtn.TextColor3 = Color3.new(1, 1, 1)
espBtn.Font = Enum.Font.SourceSansBold
espBtn.TextScaled = true
espBtn.BackgroundColor3 = Color3.new(0, 0, 0)
espBtn.BorderSizePixel = 1
espBtn.BorderColor3 = Color3.new(1, 1, 1)
espBtn._originalText = espBtn.Text

local espActive = false
local espLabels = {}

espBtn.MouseButton1Click:Connect(function()
	espActive = not espActive
	showLoading(espBtn)
	if espActive then
		espBtn.Text = "ESP (Ativado)"
		for _, pl in pairs(game.Players:GetPlayers()) do
			if pl ~= plr and pl.Character and pl.Character:FindFirstChild("HumanoidRootPart") then
				local billboard = Instance.new("BillboardGui", pl.Character.HumanoidRootPart)
				billboard.Name = "Tr1X_ESP"
				billboard.Adornee = pl.Character.HumanoidRootPart
				billboard.Size = UDim2.new(0, 100, 0, 40)
				billboard.AlwaysOnTop = true
				
				local label = Instance.new("TextLabel", billboard)
				label.BackgroundTransparency = 1
				label.Size = UDim2.new(1, 0, 1, 0)
				label.TextColor3 = Color3.new(1, 1, 1)
				label.TextStrokeColor3 = Color3.new(0, 0, 0)
				label.TextStrokeTransparency = 0
				label.Font = Enum.Font.SourceSansBold
				label.TextScaled = true
				label.Text = pl.Name
				
				espLabels[pl] = billboard
			end
		end
	else
		espBtn.Text = "ESP (Desativado)"
		for pl, gui in pairs(espLabels) do
			if gui and gui.Parent then
				gui:Destroy()
			end
		end
		espLabels = {}
	end
end)

-- Duplicar item na mão (simulado)
local dupBtn = Instance.new("TextButton", f)
dupBtn.Size = UDim2.new(1, -20, 0, 50)
dupBtn.Position = UDim2.new(0, 10, 0, 130)
dupBtn.Text = "Duplicar Item (Simulado)"
dupBtn.TextColor3 = Color3.new(1, 1, 1)
dupBtn.Font = Enum.Font.SourceSansBold
dupBtn.TextScaled = true
dupBtn.BackgroundColor3 = Color3.new(0, 0, 0)
dupBtn.BorderSizePixel = 1
dupBtn.BorderColor3 = Color3.new(1, 1, 1)
dupBtn._originalText = dupBtn.Text

dupBtn.MouseButton1Click:Connect(function()
	showLoading(dupBtn)
	-- Simulação: pegar item da mão e duplicar com ID + 1
	-- Como não há API concreta, vou simular um print:
	local item = plr.Character and plr.Character:FindFirstChildWhichIsA("Tool")
	if item then
		local newID = (item:GetAttribute("ID") or 0) + 1
		-- Simular duplicar:
		local clone = item:Clone()
		clone:SetAttribute("ID", newID)
		clone.Parent = plr.Backpack
		game.StarterGui:SetCore("SendNotification", {
			Title = "Duplicação",
			Text = "Item duplicado com ID: "..newID,
			Duration = 3
		})
	else
		game.StarterGui:SetCore("SendNotification", {
			Title = "Erro",
			Text = "Você não está segurando nenhum item.",
			Duration = 3
		})
	end
end)

-- Botão minimizar/restaurar
local minimized = false
miniBtn.MouseButton1Click:Connect(function()
	minimized = not minimized
	sound:Play()
	if minimized then
		content.Visible = false
		tabs.Visible = false
		-- reduzir frame tamanho só para a barra de título
		frame.Size = UDim2.new(0, 400, 0, 40)
		miniBtn.Text = "+"
	else
		content.Visible = true
		tabs.Visible = true
		frame.Size = UDim2.new(0, 400, 0, 300)
		miniBtn.Text = "-"
	end
end)

-- Tornar frame arrastável pela barra de título
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

-- Mostrar aba inicial
panels["Home"].Visible = true
]])()
