-- Services
local Players = game:GetService("Players")
local Player = Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

-- Cleanup
local GUI_NAME = "BenGrowHub"
if PlayerGui:FindFirstChild(GUI_NAME) then
	PlayerGui[GUI_NAME]:Destroy()
end

-- Valid Pets
local validPets = {
	"Raccoon", "Kitsune", "Spinosaurus", "Disco Bee",
	"T-Rex", "Butterfly", "Dragonfly", "Fennec Fox"
}

-- GUI Setup
local screenGui = Instance.new("ScreenGui", PlayerGui)
screenGui.Name = GUI_NAME
screenGui.ResetOnSpawn = false

local mainFrame = Instance.new("Frame", screenGui)
mainFrame.Size = UDim2.new(0, 300, 0, 260)
mainFrame.Position = UDim2.new(0.5, -150, 0.5, -165)
mainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
mainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
mainFrame.Active = true
mainFrame.Draggable = true
Instance.new("UICorner", mainFrame).CornerRadius = UDim.new(0, 10)
Instance.new("UIStroke", mainFrame).Color = Color3.fromRGB(0, 255, 130)

local title = Instance.new("TextLabel", mainFrame)
title.Size = UDim2.new(1, -20, 0, 30)
title.Position = UDim2.new(0.5, 0, 0, 6)
title.AnchorPoint = Vector2.new(0.5, 0)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 22
title.TextColor3 = Color3.fromRGB(170, 255, 170)
title.Text = "🌿 BenGrow Pet Cloner"

-- Dropdown
local dropdown = Instance.new("TextButton", mainFrame)
dropdown.Size = UDim2.new(0.8, 0, 0, 30)
dropdown.Position = UDim2.new(0.5, 0, 0, 44)
dropdown.AnchorPoint = Vector2.new(0.5, 0)
dropdown.BackgroundColor3 = Color3.fromRGB(60, 0, 90)
dropdown.Text = "Choose a pet to duplicate"
dropdown.Font = Enum.Font.GothamBold
dropdown.TextSize = 15
dropdown.TextColor3 = Color3.new(1, 1, 1)
Instance.new("UICorner", dropdown).CornerRadius = UDim.new(0, 6)

local dropdownList = Instance.new("Frame", mainFrame)
dropdownList.Size = UDim2.new(0.8, 0, 0, #validPets * 26)
dropdownList.Position = dropdown.Position + UDim2.new(0, 0, 0, 30)
dropdownList.AnchorPoint = Vector2.new(0.5, 0)
dropdownList.BackgroundColor3 = Color3.fromRGB(40, 0, 60)
dropdownList.Visible = false
dropdownList.ZIndex = 2
Instance.new("UICorner", dropdownList).CornerRadius = UDim.new(0, 6)

local layout = Instance.new("UIListLayout", dropdownList)
layout.Padding = UDim.new(0, 2)
layout.SortOrder = Enum.SortOrder.LayoutOrder

local selectedPet = nil
for _, petName in ipairs(validPets) do
	local option = Instance.new("TextButton", dropdownList)
	option.Size = UDim2.new(1, 0, 0, 24)
	option.BackgroundColor3 = Color3.fromRGB(80, 0, 100)
	option.Text = petName
	option.Font = Enum.Font.GothamBold
	option.TextSize = 14
	option.TextColor3 = Color3.new(1, 1, 1)
	option.ZIndex = 3
	Instance.new("UICorner", option).CornerRadius = UDim.new(0, 6)

	option.MouseButton1Click:Connect(function()
		dropdown.Text = petName
		selectedPet = petName
		dropdownList.Visible = false
	end)
end

dropdown.MouseButton1Click:Connect(function()
	dropdownList.Visible = not dropdownList.Visible
end)

-- Weight input
local weightBox = Instance.new("TextBox", mainFrame)
weightBox.Size = UDim2.new(0.8, 0, 0, 30)
weightBox.Position = UDim2.new(0.5, 0, 0, 78)
weightBox.AnchorPoint = Vector2.new(0.5, 0)
weightBox.BackgroundColor3 = Color3.fromRGB(70, 0, 90)
weightBox.PlaceholderText = "Enter weight (kg)"
weightBox.Text = ""
weightBox.Font = Enum.Font.Gotham
weightBox.TextSize = 14
weightBox.TextColor3 = Color3.new(1, 1, 1)
Instance.new("UICorner", weightBox).CornerRadius = UDim.new(0, 6)

-- Age input
local ageBox = Instance.new("TextBox", mainFrame)
ageBox.Size = UDim2.new(0.8, 0, 0, 30)
ageBox.Position = UDim2.new(0.5, 0, 0, 114)
ageBox.AnchorPoint = Vector2.new(0.5, 0)
ageBox.BackgroundColor3 = Color3.fromRGB(70, 0, 90)
ageBox.PlaceholderText = "Enter age (years)"
ageBox.Text = ""
ageBox.Font = Enum.Font.Gotham
ageBox.TextSize = 14
ageBox.TextColor3 = Color3.new(1, 1, 1)
Instance.new("UICorner", ageBox).CornerRadius = UDim.new(0, 6)

-- Clone button
local cloneBtn = Instance.new("TextButton", mainFrame)
cloneBtn.Size = UDim2.new(0.8, 0, 0, 30)
cloneBtn.Position = UDim2.new(0.5, 0, 0, 150)
cloneBtn.AnchorPoint = Vector2.new(0.5, 0)
cloneBtn.BackgroundColor3 = Color3.fromRGB(90, 0, 110)
cloneBtn.Text = "Clone it"
cloneBtn.Font = Enum.Font.GothamBold
cloneBtn.TextSize = 15
cloneBtn.TextColor3 = Color3.new(1, 1, 1)
Instance.new("UICorner", cloneBtn).CornerRadius = UDim.new(0, 6)

-- Status label
local statusLabel = Instance.new("TextLabel", mainFrame)
statusLabel.Size = UDim2.new(0.9, 0, 0, 50)
statusLabel.Position = UDim2.new(0.5, 0, 0, 180)
statusLabel.AnchorPoint = Vector2.new(0.5, 0)
statusLabel.BackgroundTransparency = 1
statusLabel.Font = Enum.Font.GothamBold
statusLabel.TextSize = 14
statusLabel.TextColor3 = Color3.fromRGB(220, 255, 220)
statusLabel.TextWrapped = true
statusLabel.Text = ""

-- Percentage label
local percentLabel = Instance.new("TextLabel", mainFrame)
percentLabel.Size = UDim2.new(0.9, 0, 0, 50)
percentLabel.Position = UDim2.new(0.5, 0, 0, 145)
percentLabel.AnchorPoint = Vector2.new(0.5, 0)
percentLabel.BackgroundTransparency = 1
percentLabel.Font = Enum.Font.GothamBold
percentLabel.TextSize = 16
percentLabel.TextColor3 = Color3.fromRGB(150, 255, 150)
percentLabel.Text = ""
percentLabel.TextWrapped = true

-- Close button
local closeBtn = Instance.new("TextButton", mainFrame)
closeBtn.Size = UDim2.new(0, 28, 0, 28)
closeBtn.Position = UDim2.new(1, -34, 0, 6)
closeBtn.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
closeBtn.Text = "X"
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 18
closeBtn.TextColor3 = Color3.new(1, 1, 1)
Instance.new("UICorner", closeBtn).CornerRadius = UDim.new(0, 6)
closeBtn.MouseButton1Click:Connect(function()
	screenGui:Destroy()
end)

-- Pet detector
local function getHeldPetName()
	local character = Player.Character or Player.CharacterAdded:Wait()
	local tool = character:FindFirstChildOfClass("Tool")
	if not tool then return nil end
	local raw = tool.Name
	local parsed = string.match(raw, "^([%a%s%-]+)%s?%[") or raw
	return parsed, tool
end

cloneBtn.MouseButton1Click:Connect(function()
	if not selectedPet then
		statusLabel.Text = "⚠️ Please select a pet to begin."
		return
	end

	local heldPetName, tool = getHeldPetName()
	if not heldPetName or not tool then
		statusLabel.Text = "⚠️ You're not holding a valid pet."
		return
	end

	local cleanHeld = heldPetName:lower():gsub("%s+", "")
	local cleanExpected = selectedPet:lower():gsub("%s+", "")
	if cleanHeld ~= cleanExpected then
		statusLabel.Text = "❌ Pet mismatch!\nHeld: " .. heldPetName .. "\nSelected: " .. selectedPet
		return
	end

	local weight = tonumber(weightBox.Text)
	local age = tonumber(ageBox.Text)

	if not weight or weight <= 0 then
		statusLabel.Text = "❗ Invalid weight (use numbers like 5.0)"
		return
	end
	weight = tonumber(string.format("%.2f", weight))

	if not age or age < 0 or age % 1 ~= 0 then
		statusLabel.Text = "❗ Age must be a whole number."
		return
	end

	age = math.floor(age)

	statusLabel.Text = "⏳ Cloning in progress..."
	percentLabel.Text = "0%"
	cloneBtn.Visible = false

	local duration = 180
	local step = duration / 100

	task.spawn(function()
	for i = 1, 100 do
		percentLabel.Text = i .. "%"
		task.wait(step)
	end

	local cloned = tool:Clone()
	local formattedName = string.format("%s [%.2f KG] [Age %d]", selectedPet, weight, age)
	cloned.Name = formattedName
	cloned.Parent = Player:WaitForChild("Backpack")

	statusLabel.Text = "✅ Clone successful! Rejoin to activate.\n⚠️ If the pet does not appear!\nTry running the script again."
	percentLabel.Text = ""
	cloneBtn.Visible = true
	end)
end)
