-- Exemplo funcional direto pra testar
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "VirusUI"

local Main = Instance.new("Frame", ScreenGui)
Main.Size = UDim2.new(0, 400, 0, 300)
Main.Position = UDim2.new(0.5, -200, 0.5, -150)
Main.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Main.BorderSizePixel = 0

local Title = Instance.new("TextLabel", Main)
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 1
Title.Text = "☠️ TR1X VIRUS MENU ☠️"
Title.TextColor3 = Color3.fromRGB(255, 0, 0)
Title.Font = Enum.Font.GothamBlack
Title.TextSize = 24

local minimize = Instance.new("TextButton", Main)
minimize.Text = "-"
minimize.Size = UDim2.new(0, 30, 0, 30)
minimize.Position = UDim2.new(1, -35, 0, 5)
minimize.BackgroundColor3 = Color3.fromRGB(50, 0, 0)
minimize.TextColor3 = Color3.fromRGB(255, 255, 255)

local tabs = Instance.new("Frame", Main)
tabs.Position = UDim2.new(0, 0, 0, 40)
tabs.Size = UDim2.new(1, 0, 1, -40)
tabs.BackgroundColor3 = Color3.fromRGB(10, 10, 10)

local Risk = Instance.new("TextButton", tabs)
Risk.Text = "Risk"
Risk.Size = UDim2.new(0, 100, 0, 30)
Risk.Position = UDim2.new(0, 10, 0, 10)
Risk.BackgroundColor3 = Color3.fromRGB(70, 0, 0)
Risk.TextColor3 = Color3.fromRGB(255, 255, 255)

local Looping = false
local LoopBtn = Instance.new("TextButton", tabs)
LoopBtn.Text = "Sou Preguiçoso"
LoopBtn.Size = UDim2.new(0, 200, 0, 40)
LoopBtn.Position = UDim2.new(0, 10, 0, 60)
LoopBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
LoopBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
LoopBtn.TextSize = 18

local function GiveMoney()
	while Looping do
		local plr = game.Players.LocalPlayer
		if plr and plr.leaderstats and plr.leaderstats:FindFirstChild("Sheckles") then
			plr.leaderstats.Sheckles.Value += 5000000
			print("💰 5M de Sheckles adicionados!")
		end
		wait(2)
	end
end

LoopBtn.MouseButton1Click:Connect(function()
	Looping = not Looping
	if Looping then
		LoopBtn.Text = "Preguiça Ativada"
		task.spawn(GiveMoney)
		game.StarterGui:SetCore("SendNotification", {
			Title = "TR1X VIRUS",
			Text = "Pronto seu liso fudido, vai roubar, trabalhar não dá futuro",
			Duration = 5
		})
	else
		LoopBtn.Text = "Sou Preguiçoso"
	end
end)

minimize.MouseButton1Click:Connect(function()
	Main.Visible = not Main.Visible
end)
