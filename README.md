loadstring([[
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local plr = Players.LocalPlayer
local sheckles = plr:WaitForChild("Sheckles", 10)
local AddShecklesEvent = ReplicatedStorage:WaitForChild("AddSheckles", 10)

-- Criar GUI
local gui = Instance.new("ScreenGui")
gui.Name = "Tr1X_MacabraUI"
gui.Parent = game.CoreGui

-- Sons macabros
local soundClick = Instance.new("Sound", gui)
soundClick.SoundId = "rbxassetid://4590657391"
soundClick.Volume = 0.8

-- Estilo sujo macabro
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

local function makeButton(text, parent)
	local b = Instance.new("TextButton", parent)
	b.Size = UDim2.new(1, -20, 0, 40)
	b.Position = UDim2.new(0, 10, 0, 0)
	b.BackgroundColor3 = Color3.fromRGB(60, 0, 60)
	b.TextColor3 = Color3.new(1, 0, 1)
	b.Font = Enum.Font.SourceSansBold
	b.TextScaled = true
	b.Text = text
	return b
end

-- Janela principal
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 450, 0, 350)
frame.Position = UDim2.new(0.5, -225, 0.5, -175)
frame.BackgroundColor3 = Color3.fromRGB(15, 0, 15)
frame.BorderSizePixel = 0

local title = makeLabel("💀 Tr1X MENU MACABRO 💀", frame)
title.Size = UDim2.new(1, 0, 0, 40)

-- Minimizar botão
local miniBtn = Instance.new("TextButton", title)
miniBtn.Size = UDim2.new(0, 30, 1, 0)
miniBtn.Position = UDim2.new(1, -30, 0, 0)
miniBtn.Text = "-"
miniBtn.TextColor3 = Color3.new(1, 0, 1)
miniBtn.BackgroundColor3 = Color3.fromRGB(50, 0, 50)
miniBtn.Font = Enum.Font.SourceSansBold
miniBtn.TextScaled = true

-- Tabs
local tabs = Instance.new("Frame", frame)
tabs.Size = UDim2.new(0, 110, 1, -40)
tabs.Position = UDim2.new(0, 0, 0, 40)
tabs.BackgroundColor3 = Color3.fromRGB(40, 0, 40)

local content = Instance.new("Frame", frame)
content.Size = UDim2.new(1, -110, 1, -40)
content.Position = UDim2.new(0, 110, 0, 40)
content.BackgroundColor3 = Color3.fromRGB(20, 0, 20)

local panels = {}
local function createTab(name)
	local btn = Instance.new("TextButton", tabs)
	btn.Size = UDim2.new(1, 0, 0, 40)
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
		for _, v in pairs(panels) do v.Visible = false end
		panel.Visible = true
		soundClick:Play()
	end)
end

createTab("Home")
createTab("Risk")
createTab("Outfit")
createTab("Fun")

-- Minimizar ação
miniBtn.MouseButton1Click:Connect(function()
	content.Visible = not content.Visible
	soundClick:Play()
end)

-- Conteúdo Home
do
	local h = panels["Home"]
	local label1 = makeLabel("Seja bem-vindo ao inferno digital", h)
	label1.Size = UDim2.new(1, 0, 0, 50)
	local label2 = makeLabel("Seu liso fudido, vai farmar", h)
	label2.Position = UDim2.new(0, 0, 0, 60)
	label2.Size = UDim2.new(1, 0, 0, 50)
end

-- Função para notificação
local function notify(title, text)
	game.StarterGui:SetCore("SendNotification", {
		Title = title,
		Text = text,
		Duration = 4
	})
end

-- Conteúdo Risk (farm sheckles)
do
	local r = panels["Risk"]
	local lazyBtn = makeButton("Sou Preguiçoso (Ativar)", r)
	lazyBtn.Position = UDim2.new(0, 10, 0, 10)

	local loopActive = false
	local lastTick = 0

	lazyBtn.MouseButton1Click:Connect(function()
		loopActive = not loopActive
		soundClick:Play()
		if loopActive then
			lazyBtn.Text = "Sou Preguiçoso (Desativar)"
			notify("Pronto seu liso 😈", "Vai roubar, trabalhar não dá futuro.")
		else
			lazyBtn.Text = "Sou Preguiçoso (Ativar)"
		end
	end)

	RunService.RenderStepped:Connect(function()
		if loopActive then
			if tick() - lastTick > 2 then
				if AddShecklesEvent and sheckles then
					AddShecklesEvent:FireServer(5000000)
				end
				lastTick = tick()
			end
		end
	end)
end

-- Conteúdo Outfit (ESP)
do
	local o = panels["Outfit"]

	local espBtn = makeButton("Ativar ESP", o)
	espBtn.Position = UDim2.new(0, 10, 0, 10)

	local espActive = false
	local espBoxes = {}

	local function createBox(player)
		local box = Instance.new("BoxHandleAdornment")
		box.Adornee = nil
		box.AlwaysOnTop = true
		box.ZIndex = 10
		box.Size = Vector3.new(4, 6, 4)
		box.Color3 = Color3.fromRGB(255, 0, 255)
		box.Transparency = 0.5
		box.Parent = game.CoreGui
		return box
	end

	espBtn.MouseButton1Click:Connect(function()
		espActive = not espActive
		soundClick:Play()
		if espActive then
			espBtn.Text = "Desativar ESP"
		else
			espBtn.Text = "Ativar ESP"
			-- Remove todos
			for _, box in pairs(espBoxes) do
				if box then box:Destroy() end
			end
			espBoxes = {}
		end
	end)

	RunService.RenderStepped:Connect(function()
		if espActive then
			for _, player in pairs(Players:GetPlayers()) do
				if player ~= plr and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
					if not espBoxes[player] then
						espBoxes[player] = createBox(player)
					end
					espBoxes[player].Adornee = player.Character.HumanoidRootPart
				end
			end
		end
	end)
end

-- Conteúdo Fun (Duplicar item na mão)
do
	local f = panels["Fun"]

	local dupBtn = makeButton("Duplicar Item na Mão", f)
	dupBtn.Position = UDim2.new(0, 10, 0, 10)

	local function duplicateItem()
		local char = plr.Character
		if not char then notify("Erro", "Personagem não carregado") return end

		local tool = char:FindFirstChildOfClass("Tool")
		if not tool then
			notify("Erro", "Segure um item para duplicar!")
			return
		end

		local newTool = tool:Clone()

		-- Assumindo que o ID está em um atributo ou propriedade chamada "ItemID" (ajuste conforme seu jogo)
		local id = newTool:GetAttribute("ItemID") or tonumber(newTool.Name:match("%d+")) or 0
		if id == 0 then
			notify("Erro", "Não foi possível identificar ID do item.")
			return
		end

		local newId = id + 1
		newTool:SetAttribute("ItemID", newId)

		-- Renomear item para refletir novo ID, se quiser
		newTool.Name = "Item_" .. newId

		newTool.Parent = plr.Backpack
		notify("Sucesso", "Item duplicado com ID " .. newId)
	end

	dupBtn.MouseButton1Click:Connect(function()
		soundClick:Play()
		duplicateItem()
	end)
end

-- Voo
do
	local flying = false
	local speed = 50
	local bodyVelocity

	UserInputService.InputBegan:Connect(function(input, gameProcessed)
		if gameProcessed then return end
		if input.KeyCode == Enum.KeyCode.LeftControl then
			flying = not flying
			soundClick:Play()
			notify("Voo", flying and "Voo ativado" or "Voo desativado")

			local char = plr.Character
			local hrp = char and char:FindFirstChild("HumanoidRootPart")

			if flying and hrp then
				bodyVelocity = Instance.new("BodyVelocity")
				bodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
				bodyVelocity.Velocity = Vector3.new(0, 0, 0)
				bodyVelocity.Parent = hrp
			elseif bodyVelocity then
				bodyVelocity:Destroy()
				bodyVelocity = nil
			end
		end
	end)

	RunService.RenderStepped:Connect(function()
		if flying and bodyVelocity then
			local char = plr.Character
			local hrp = char and char:FindFirstChild("HumanoidRootPart")
			if hrp then
				local moveDir = Vector3.new(0, 0, 0)
				if UserInputService:IsKeyDown(Enum.KeyCode.W) then moveDir = moveDir + workspace.CurrentCamera.CFrame.LookVector end
				if UserInputService:IsKeyDown(Enum.KeyCode.S) then moveDir = moveDir - workspace.CurrentCamera.CFrame.LookVector end
				if UserInputService:IsKeyDown(Enum.KeyCode.A) then moveDir = moveDir - workspace.CurrentCamera.CFrame.RightVector end
				if UserInputService:IsKeyDown(Enum.KeyCode.D) then moveDir = moveDir + workspace.CurrentCamera.CFrame.RightVector end
				if UserInputService:IsKeyDown(Enum.KeyCode.Space) then moveDir = moveDir + Vector3.new(0, 1, 0) end
				if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then moveDir = moveDir - Vector3.new(0, 1, 0) end

				moveDir = Vector3.new(moveDir.X, moveDir.Y, moveDir.Z).Unit
				if moveDir ~= moveDir then moveDir = Vector3.new(0,0,0) end -- evitar NaN

				bodyVelocity.Velocity = moveDir * speed
			end
		end
	end)
end

-- Mostrar aba Home por padrão
panels["Home"].Visible = true

]])()
