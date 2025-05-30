loadstring([[
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local plr = Players.LocalPlayer
local mouse = plr:GetMouse()

-- == Sons macabros
local function playSound(parent)
    local sound = Instance.new("Sound", parent)
    sound.SoundId = "rbxassetid://4590657391" -- som macabro
    sound.Volume = 1
    sound:Play()
    game.Debris:AddItem(sound, 2)
end

-- == Criar GUI
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "Tr1X_MacabraUI"

local function makeLabel(text, parent)
    local lbl = Instance.new("TextLabel", parent)
    lbl.BackgroundColor3 = Color3.new(0,0,0)
    lbl.TextColor3 = Color3.new(1,1,1)
    lbl.Font = Enum.Font.Arcade
    lbl.TextScaled = true
    lbl.Size = UDim2.new(1,0,0,30)
    lbl.BorderSizePixel = 0
    lbl.Text = text
    return lbl
end

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 420, 0, 320)
frame.Position = UDim2.new(0.5, -210, 0.5, -160)
frame.BackgroundColor3 = Color3.new(0,0,0)
frame.BorderColor3 = Color3.new(1,1,1)
frame.BorderSizePixel = 2

local title = makeLabel("💀 Tr1X MENU MACABRO 💀", frame)
title.Size = UDim2.new(1,0,0,40)
title.BackgroundColor3 = Color3.new(0,0,0)
title.BorderSizePixel = 1
title.BorderColor3 = Color3.new(1,1,1)
title.TextColor3 = Color3.new(1,1,1)

-- Botão minimizar
local miniBtn = Instance.new("TextButton", title)
miniBtn.Size = UDim2.new(0, 30, 1, 0)
miniBtn.Position = UDim2.new(1, -30, 0, 0)
miniBtn.Text = "-"
miniBtn.TextColor3 = Color3.new(1,1,1)
miniBtn.BackgroundColor3 = Color3.new(0,0,0)
miniBtn.Font = Enum.Font.SourceSansBold
miniBtn.TextScaled = true
miniBtn.BorderSizePixel = 1
miniBtn.BorderColor3 = Color3.new(1,1,1)

local tabs = Instance.new("Frame", frame)
tabs.Size = UDim2.new(0, 110, 1, -40)
tabs.Position = UDim2.new(0, 0, 0, 40)
tabs.BackgroundColor3 = Color3.new(0,0,0)
tabs.BorderSizePixel = 1
tabs.BorderColor3 = Color3.new(1,1,1)

local content = Instance.new("Frame", frame)
content.Size = UDim2.new(1, -110, 1, -40)
content.Position = UDim2.new(0, 110, 0, 40)
content.BackgroundColor3 = Color3.new(0,0,0)
content.BorderSizePixel = 1
content.BorderColor3 = Color3.new(1,1,1)

local panels = {}
local buttons = {}

-- Função animar texto "carregando"
local function animateLoading(button)
    local origText = button.Text
    button.Text = "Carregando."
    wait(0.3)
    button.Text = "Carregando.."
    wait(0.3)
    button.Text = "Carregando..."
    wait(0.3)
    button.Text = origText
    playSound(button)
end

local function createTab(name)
    local btn = Instance.new("TextButton", tabs)
    btn.Size = UDim2.new(1,0,0,35)
    btn.Text = name
    btn.TextColor3 = Color3.new(1,1,1)
    btn.Font = Enum.Font.SourceSansBold
    btn.BackgroundColor3 = Color3.new(0,0,0)
    btn.BorderSizePixel = 1
    btn.BorderColor3 = Color3.new(1,1,1)
    btn.TextScaled = true
    
    buttons[name] = btn
    
    local panel = Instance.new("Frame", content)
    panel.Name = name
    panel.Size = UDim2.new(1,0,1,0)
    panel.BackgroundTransparency = 1
    panel.Visible = false
    panels[name] = panel
    
    btn.MouseButton1Click:Connect(function()
        coroutine.wrap(function()
            animateLoading(btn)
            for _,v in pairs(panels) do v.Visible = false end
            panel.Visible = true
        end)()
    end)
end

createTab("Home")
createTab("Risk")
createTab("Fun")

panels["Home"].Visible = true

-- Minimizar controle
local minimized = false
miniBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    playSound(miniBtn)
    if minimized then
        content.Visible = false
        tabs.Visible = false
        frame.Size = UDim2.new(0, 420, 0, 40)
        miniBtn.Text = "+"
    else
        content.Visible = true
        tabs.Visible = true
        frame.Size = UDim2.new(0, 420, 0, 320)
        miniBtn.Text = "-"
    end
end)

-- Arrastar menu
local dragging = false
local dragInput, dragStart, startPos

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

UserInputService.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        local delta = input.Position - dragStart
        frame.Position = UDim2.new(
            startPos.X.Scale,
            math.clamp(startPos.X.Offset + delta.X, 0, workspace.CurrentCamera.ViewportSize.X - frame.AbsoluteSize.X),
            startPos.Y.Scale,
            math.clamp(startPos.Y.Offset + delta.Y, 0, workspace.CurrentCamera.ViewportSize.Y - frame.AbsoluteSize.Y)
        )
    end
end)

-- Conteúdo aba Home
do
    local h = panels["Home"]
    local welcome = makeLabel("Bem-vindo ao inferno digital", h)
    welcome.Size = UDim2.new(1,0,0,50)
    
    local insult = makeLabel("Seu liso fudido, vai farmar", h)
    insult.Position = UDim2.new(0,0,0,60)
    insult.Size = UDim2.new(1,0,0,50)
end

-- Variáveis para Sheckles (usa RemoteEvent para comunicar com o servidor)
local remoteAddSheckles = ReplicatedStorage:FindFirstChild("AddSheckles") or Instance.new("RemoteEvent", ReplicatedStorage)
remoteAddSheckles.Name = "AddSheckles"

-- Loop farm Sheckles
local farmActive = false
local farmConnection

-- Aba Risk
do
    local r = panels["Risk"]
    local lazyBtn = Instance.new("TextButton", r)
    lazyBtn.Size = UDim2.new(1, -20, 0, 50)
    lazyBtn.Position = UDim2.new(0, 10, 0, 10)
    lazyBtn.Text = "Sou Preguiçoso (Ativar)"
    lazyBtn.TextColor3 = Color3.new(1,1,1)
    lazyBtn.Font = Enum.Font.SourceSansBold
    lazyBtn.TextScaled = true
    lazyBtn.BackgroundColor3 = Color3.new(0,0,0)
    lazyBtn.BorderSizePixel = 1
    lazyBtn.BorderColor3 = Color3.new(1,1,1)
    
    lazyBtn.MouseButton1Click:Connect(function()
        farmActive = not farmActive
        playSound(lazyBtn)
        if farmActive then
            lazyBtn.Text = "Sou Preguiçoso (Desativar)"
            game.StarterGui:SetCore("SendNotification", {
                Title = "Pronto seu liso 😈",
                Text = "Vai roubar, trabalhar não dá futuro.",
                Duration = 4
            })
            farmConnection = RunService.Heartbeat:Connect(function()
                if tick() % 2 < 0.03 then
                    -- envia pedido pro server adicionar 5M sheckles
                    remoteAddSheckles:FireServer(5000000)
                end
            end)
        else
            lazyBtn.Text = "Sou Preguiçoso (Ativar)"
            if farmConnection then farmConnection:Disconnect() end
        end
    end)
end

-- Funções do Fun
local Fun = panels["Fun"]

-- Voo
local flying = false
local speed = 60
local bodyVelocity = nil
local bodyGyro = nil

local function startFly()
    local character = plr.Character
    if not character or not character:FindFirstChild("HumanoidRootPart") then return end
    local hrp = character.HumanoidRootPart

    bodyVelocity = Instance.new("BodyVelocity", hrp)
    bodyVelocity.MaxForce = Vector3.new(9e4, 9e4, 9e4)
    bodyVelocity.Velocity = Vector3.new(0,0,0)

    bodyGyro = Instance.new("BodyGyro", hrp)
    bodyGyro.MaxTorque = Vector3.new(9e4,9e4,9e4)
    bodyGyro.CFrame = hrp.CFrame
end

local function stopFly()
    if bodyVelocity then bodyVelocity:Destroy() bodyVelocity = nil end
    if bodyGyro then bodyGyro:Destroy() bodyGyro = nil end
end

local keys = {W=false, A=false, S=false, D=false, SPACE=false, SHIFT=false}

UserInputService.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    local key = input.KeyCode
    if key == Enum.KeyCode.LeftControl then
        flying = not flying
        playSound(frame)
        if flying then
            startFly()
        else
            stopFly()
        end
    elseif keys[key.Name] ~= nil then
        keys[key.Name] = true
    end
end)

UserInputService.InputEnded:Connect(function(input, gpe)
    if gpe then return end
    local key = input.KeyCode
    if keys[key.Name] ~= nil then
        keys[key.Name] = false
    end
end)

RunService.Heartbeat:Connect(function()
    if flying and bodyVelocity and bodyGyro then
        local character = plr.Character
        if character and character:FindFirstChild("HumanoidRootPart") and character:FindFirstChildOfClass("Humanoid") then
            local hrp = character.HumanoidRootPart
            local cam = workspace.CurrentCamera
            local vel = Vector3.new(0,0,0)
            local humanoid = character:FindFirstChildOfClass("Humanoid")
            local speedMod = speed
            
            if keys.W then vel = vel + cam.CFrame.LookVector end
            if keys.S then vel = vel - cam.CFrame.LookVector end
            if keys.A then vel = vel - cam.CFrame.RightVector end
            if keys.D then vel = vel + cam.CFrame.RightVector end
            if keys.SPACE then vel = vel + Vector3.new(0,1,0) end
            if keys.SHIFT then vel = vel - Vector3.new(0,1,0) end
            
            bodyVelocity.Velocity = vel.Unit * speedMod
            bodyGyro.CFrame = cam.CFrame
            humanoid.PlatformStand = true
        end
    elseif plr.Character and plr.Character:FindFirstChildOfClass("Humanoid") then
        plr.Character.Humanoid.PlatformStand = false
    end
end)

-- ESP simples
local ESP_Enabled = false
local espLabels = {}

local function toggleESP()
    ESP_Enabled = not ESP_Enabled
    playSound(Fun)
    if ESP_Enabled then
        for _,p in pairs(Players:GetPlayers()) do
            if p ~= plr and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                local bill = Instance.new("BillboardGui", p.Character.HumanoidRootPart)
                bill.Name = "ESP"
                bill.Adornee = p.Character.HumanoidRootPart
                bill.Size = UDim2.new(0,100,0,40)
                bill.StudsOffset = Vector3.new(0, 2, 0)
                bill.AlwaysOnTop = true
                
                local label = Instance.new("TextLabel", bill)
                label.Size = UDim2.new(1,0,1,0)
                label.BackgroundTransparency = 1
                label.TextColor3 = Color3.new(1,1,1)
                label.Font = Enum.Font.Arcade
                label.TextScaled = true
                label.Text = p.Name
                espLabels[p] = bill
            end
        end
    else
        for p,bill in pairs(espLabels) do
            if bill and bill.Parent then
                bill:Destroy()
            end
        end
        espLabels = {}
    end
end

local espBtn = Instance.new("TextButton", Fun)
espBtn.Size = UDim2.new(1, -20, 0, 50)
espBtn.Position = UDim2.new(0, 10, 0, 10)
espBtn.Text = "ESP (Ligar/Desligar)"
espBtn.TextColor3 = Color3.new(1,1,1)
espBtn.Font = Enum.Font.SourceSansBold
espBtn.TextScaled = true
espBtn.BackgroundColor3 = Color3.new(0,0,0)
espBtn.BorderSizePixel = 1
espBtn.BorderColor3 = Color3.new(1,1,1)
espBtn.MouseButton1Click:Connect(toggleESP)

-- Duplicar item na mão (simples: adiciona 1 ao ID e dá ao jogador)
local function duplicateItem()
    local character = plr.Character
    if not character then return end
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if not humanoid then return end

    local tool = plr.Character and plr.Character:FindFirstChildOfClass("Tool")
    if not tool then
        game.StarterGui:SetCore("SendNotification", {
            Title = "Erro",
            Text = "Segure uma ferramenta para duplicar",
            Duration = 3
        })
        return
    end

    playSound(Fun)
    local toolClone = tool:Clone()
    toolClone.Parent = plr.Backpack
end

local dupBtn = Instance.new("TextButton", Fun)
dupBtn.Size = UDim2.new(1, -20, 0, 50)
dupBtn.Position = UDim2.new(0, 10, 0, 70)
dupBtn.Text = "Duplicar item na mão"
dupBtn.TextColor3 = Color3.new(1,1,1)
dupBtn.Font = Enum.Font.SourceSansBold
dupBtn.TextScaled = true
dupBtn.BackgroundColor3 = Color3.new(0,0,0)
dupBtn.BorderSizePixel = 1
dupBtn.BorderColor3 = Color3.new(1,1,1)
dupBtn.MouseButton1Click:Connect(duplicateItem)

-- Aviso de ativação do voo
local function notify(msg)
    game.StarterGui:SetCore("SendNotification", {
        Title = "Tr1X Menu",
        Text = msg,
        Duration = 3
    })
end

UserInputService.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == Enum.KeyCode.LeftControl then
        flying = not flying
        playSound(frame)
        if flying then
            startFly()
            notify("Voo ativado")
        else
            stopFly()
            notify("Voo desativado")
        end
    end
end)

-- Limpar ao fechar o menu (opcional)
gui.Destroying:Connect(function()
    if farmConnection then farmConnection:Disconnect() end
    if flying then stopFly() end
    for _,bill in pairs(espLabels) do
        if bill and bill.Parent then bill:Destroy() end
    end
end)

]])()
