--// SERVIÇOS
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer

--// REMOVE GUI ANTIGA
pcall(function()
	if game.CoreGui:FindFirstChild("FlyPartHub") then
		game.CoreGui.FlyPartHub:Destroy()
	end
end)

--// GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "FlyPartHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = game.CoreGui

local MainFrame = Instance.new("Frame")
MainFrame.Parent = ScreenGui
MainFrame.Size = UDim2.new(0,300,0,260)
MainFrame.Position = UDim2.new(0.05,0,0.35,0)
MainFrame.BackgroundColor3 = Color3.fromRGB(20,20,20)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true

Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0,10)

local Stroke = Instance.new("UIStroke")
Stroke.Parent = MainFrame
Stroke.Thickness = 2

--// DRAG
local dragging = false
local dragStart
local startPos

MainFrame.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then

		dragging = true
		dragStart = input.Position
		startPos = MainFrame.Position

		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

UserInputService.InputChanged:Connect(function(input)

	if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then

		local delta = input.Position - dragStart

		MainFrame.Position = UDim2.new(
			startPos.X.Scale,
			startPos.X.Offset + delta.X,
			startPos.Y.Scale,
			startPos.Y.Offset + delta.Y
		)
	end
end)

--// TITULO
local Title = Instance.new("TextLabel")
Title.Parent = MainFrame
Title.Size = UDim2.new(1,0,0,35)
Title.BackgroundTransparency = 1
Title.Text = "Fly To Parts Hub"
Title.Font = Enum.Font.GothamBold
Title.TextSize = 17
Title.TextColor3 = Color3.new(1,1,1)

--// FECHAR
local CloseButton = Instance.new("TextButton")
CloseButton.Parent = MainFrame
CloseButton.Size = UDim2.new(0,30,0,30)
CloseButton.Position = UDim2.new(1,-35,0,2)
CloseButton.BackgroundTransparency = 1
CloseButton.Text = "X"
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextSize = 18
CloseButton.TextColor3 = Color3.fromRGB(255,80,80)

CloseButton.MouseButton1Click:Connect(function()
	ScreenGui:Destroy()
end)

--// MINIMIZAR
local MinimizeButton = Instance.new("TextButton")
MinimizeButton.Parent = MainFrame
MinimizeButton.Size = UDim2.new(0,30,0,30)
MinimizeButton.Position = UDim2.new(1,-70,0,2)
MinimizeButton.BackgroundTransparency = 1
MinimizeButton.Text = "-"
MinimizeButton.Font = Enum.Font.GothamBold
MinimizeButton.TextSize = 22
MinimizeButton.TextColor3 = Color3.new(1,1,1)

local Minimized = false

--// BOTÕES
local Create1 = Instance.new("TextButton")
Create1.Parent = MainFrame
Create1.Size = UDim2.new(0,240,0,35)
Create1.Position = UDim2.new(0.5,-120,0.18,0)
Create1.BackgroundColor3 = Color3.fromRGB(35,35,35)
Create1.Text = "CRIAR PART 1"
Create1.Font = Enum.Font.GothamBold
Create1.TextSize = 14
Create1.TextColor3 = Color3.new(1,1,1)

Instance.new("UICorner", Create1)

local Fly1 = Instance.new("TextButton")
Fly1.Parent = MainFrame
Fly1.Size = UDim2.new(0,240,0,35)
Fly1.Position = UDim2.new(0.5,-120,0.35,0)
Fly1.BackgroundColor3 = Color3.fromRGB(35,35,35)
Fly1.Text = "VOAR PARA PART 1"
Fly1.Font = Enum.Font.GothamBold
Fly1.TextSize = 14
Fly1.TextColor3 = Color3.new(1,1,1)

Instance.new("UICorner", Fly1)

local Create2 = Instance.new("TextButton")
Create2.Parent = MainFrame
Create2.Size = UDim2.new(0,240,0,35)
Create2.Position = UDim2.new(0.5,-120,0.55,0)
Create2.BackgroundColor3 = Color3.fromRGB(35,35,35)
Create2.Text = "CRIAR PART 2"
Create2.Font = Enum.Font.GothamBold
Create2.TextSize = 14
Create2.TextColor3 = Color3.new(1,1,1)

Instance.new("UICorner", Create2)

local Fly2 = Instance.new("TextButton")
Fly2.Parent = MainFrame
Fly2.Size = UDim2.new(0,240,0,35)
Fly2.Position = UDim2.new(0.5,-120,0.72,0)
Fly2.BackgroundColor3 = Color3.fromRGB(35,35,35)
Fly2.Text = "VOAR PARA PART 2"
Fly2.Font = Enum.Font.GothamBold
Fly2.TextSize = 14
Fly2.TextColor3 = Color3.new(1,1,1)

Instance.new("UICorner", Fly2)

--// MINIMIZAR
MinimizeButton.MouseButton1Click:Connect(function()

	Minimized = not Minimized

	TweenService:Create(MainFrame,TweenInfo.new(0.25),{
		Size = Minimized and UDim2.new(0,300,0,35) or UDim2.new(0,300,0,260)
	}):Play()

	Create1.Visible = not Minimized
	Fly1.Visible = not Minimized
	Create2.Visible = not Minimized
	Fly2.Visible = not Minimized

	MinimizeButton.Text = Minimized and "+" or "-"
end)

--// PARTS
local Part1
local Part2

--// CRIAR PART 1
Create1.MouseButton1Click:Connect(function()

	if Part1 then
		Part1:Destroy()
	end

	local Character = LocalPlayer.Character
	if not Character then return end

	local HRP = Character:FindFirstChild("HumanoidRootPart")
	if not HRP then return end

	Part1 = Instance.new("Part")
	Part1.Name = "Part1"
	Part1.Size = Vector3.new(8,1,8)
	Part1.Anchored = true
	Part1.Material = Enum.Material.Neon
	Part1.Color = Color3.fromRGB(0,255,100)

	-- CRIA A PART AONDE O PLAYER ESTÁ
	Part1.Position = HRP.Position - Vector3.new(0,3,0)

	Part1.Parent = workspace
end)

--// CRIAR PART 2
Create2.MouseButton1Click:Connect(function()

	if Part2 then
		Part2:Destroy()
	end

	local Character = LocalPlayer.Character
	if not Character then return end

	local HRP = Character:FindFirstChild("HumanoidRootPart")
	if not HRP then return end

	Part2 = Instance.new("Part")
	Part2.Name = "Part2"
	Part2.Size = Vector3.new(8,1,8)
	Part2.Anchored = true
	Part2.Material = Enum.Material.Neon
	Part2.Color = Color3.fromRGB(255,0,0)

	-- CRIA A PART AONDE O PLAYER ESTÁ
	Part2.Position = HRP.Position - Vector3.new(0,3,0)

	Part2.Parent = workspace
end)

--// NOCLIP
local NoclipConnection

local function EnableNoclip()

	NoclipConnection = RunService.Stepped:Connect(function()

		local Character = LocalPlayer.Character

		if Character then
			for _,v in pairs(Character:GetDescendants()) do
				if v:IsA("BasePart") then
					v.CanCollide = false
				end
			end
		end
	end)
end

local function DisableNoclip()

	if NoclipConnection then
		NoclipConnection:Disconnect()
		NoclipConnection = nil
	end
end

--// SISTEMA DE VOO
local Flying = false

local function FlyTo(TargetPart, Button)

	if not TargetPart or not TargetPart.Parent then
		return
	end

	local Character = LocalPlayer.Character
	if not Character then return end

	local HRP = Character:FindFirstChild("HumanoidRootPart")
	if not HRP then return end

	-- PARAR VOO
	if Flying then
		Flying = false
		Button.Text = "VOAR"
		Button.BackgroundColor3 = Color3.fromRGB(35,35,35)
		return
	end

	-- COMEÇAR VOO
	Flying = true
	Button.Text = "PARAR"
	Button.BackgroundColor3 = Color3.fromRGB(255,70,70)

	EnableNoclip()

	while Flying and TargetPart and TargetPart.Parent do

		local Direction = (TargetPart.Position - HRP.Position)
		local Distance = Direction.Magnitude

		-- CHEGOU
		if Distance <= 4 then
			break
		end

		-- MOVE
		HRP.CFrame = HRP.CFrame + (Direction.Unit * 2.5)

		RunService.Heartbeat:Wait()
	end

	Flying = false

	Button.Text = "VOAR"
	Button.BackgroundColor3 = Color3.fromRGB(35,35,35)

	DisableNoclip()
end

--// BOTÃO PART 1
Fly1.MouseButton1Click:Connect(function()

	task.spawn(function()
		FlyTo(Part1, Fly1)
	end)
end)

--// BOTÃO PART 2
Fly2.MouseButton1Click:Connect(function()

	task.spawn(function()
		FlyTo(Part2, Fly2)
	end)
end)

--// RGB
RunService.RenderStepped:Connect(function()

	local Color = Color3.fromHSV((tick()%5)/5,1,1)

	Stroke.Color = Color
	Title.TextColor3 = Color
end)
