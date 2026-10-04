-- LocalScript đặt trong StarterPlayer > StarterPlayerScripts
-- Tên: Sonhub (test)

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local rootPart = character:WaitForChild("HumanoidRootPart")

local flying = false
local speed = 50 -- tốc độ bay
local bodyVelocity = nil
local moveDirection = Vector3.zero

-- ========== Tạo GUI ==========
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "Sonhub (test)"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Nút Bật/Tắt Bay
local toggleBtn = Instance.new("TextButton")
toggleBtn.Size = UDim2.new(0, 130, 0, 50)
toggleBtn.Position = UDim2.new(1, -150, 1, -180)
toggleBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
toggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleBtn.Text = "Sonhub: TẮT"
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.TextSize = 16
toggleBtn.Parent = screenGui

local corner1 = Instance.new("UICorner")
corner1.CornerRadius = UDim.new(0, 12)
corner1.Parent = toggleBtn

-- Nút Lên
local upBtn = Instance.new("TextButton")
upBtn.Size = UDim2.new(0, 70, 0, 70)
upBtn.Position = UDim2.new(1, -90, 1, -280)
upBtn.BackgroundColor3 = Color3.fromRGB(50, 120, 50)
upBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
upBtn.Text = "↑"
upBtn.Font = Enum.Font.GothamBold
upBtn.TextSize = 32
upBtn.Visible = false
upBtn.Parent = screenGui

local corner2 = Instance.new("UICorner")
corner2.CornerRadius = UDim.new(0, 12)
corner2.Parent = upBtn

-- Nút Xuống
local downBtn = Instance.new("TextButton")
downBtn.Size = UDim2.new(0, 70, 0, 70)
downBtn.Position = UDim2.new(1, -90, 1, -360)
downBtn.BackgroundColor3 = Color3.fromRGB(120, 50, 50)
downBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
downBtn.Text = "↓"
downBtn.Font = Enum.Font.GothamBold
downBtn.TextSize = 32
downBtn.Visible = false
downBtn.Parent = screenGui

local corner3 = Instance.new("UICorner")
corner3.CornerRadius = UDim.new(0, 12)
corner3.Parent = downBtn

-- ========== Hàm bay ==========
local function startFly()
	if flying then return end
	flying = true
	humanoid.PlatformStand = true
	
	bodyVelocity = Instance.new("BodyVelocity")
	bodyVelocity.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
	bodyVelocity.Velocity = Vector3.zero
	bodyVelocity.Parent = rootPart
	
	toggleBtn.Text = "Sonhub: BẬT"
	toggleBtn.BackgroundColor3 = Color3.fromRGB(30, 140, 30)
	upBtn.Visible = true
	downBtn.Visible = true
end

local function stopFly()
	if not flying then return end
	flying = false
	humanoid.PlatformStand = false
	if bodyVelocity then
		bodyVelocity:Destroy()
		bodyVelocity = nil
	end
	
	toggleBtn.Text = "Sonhub: TẮT"
	toggleBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	upBtn.Visible = false
	downBtn.Visible = false
	moveDirection = Vector3.zero
end

-- Bấm nút Bật/Tắt
toggleBtn.MouseButton1Click:Connect(function()
	if flying then
		stopFly()
	else
		startFly()
	end
end)

-- Giữ nút Lên / Xuống
upBtn.MouseButton1Down:Connect(function()
	moveDirection = moveDirection + Vector3.new(0, 1, 0)
end)
upBtn.MouseButton1Up:Connect(function()
	moveDirection = moveDirection - Vector3.new(0, 1, 0)
end)

downBtn.MouseButton1Down:Connect(function()
	moveDirection = moveDirection + Vector3.new(0, -1, 0)
end)
downBtn.MouseButton1Up:Connect(function()
	moveDirection = moveDirection - Vector3.new(0, -1, 0)
end)

-- Hỗ trợ phím F (máy tính)
UserInputService.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.F then
		if flying then
			stopFly()
		else
			start
