local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local runService = game:GetService("RunService")
local isInvisible = false

-- Interface
local screenGui = Instance.new("ScreenGui")
local invButton = Instance.new("TextButton")
local uiCorner = Instance.new("UICorner")

screenGui.Name = "InvisGui"
screenGui.Parent = player:WaitForChild("PlayerGui")
screenGui.ResetOnSpawn = false

invButton.Size = UDim2.new(0, 150, 0, 50)
invButton.Position = UDim2.new(0.5, -75, 0.3, 0)
invButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
invButton.Text = "Invisibilidade: OFF"
invButton.TextColor3 = Color3.fromRGB(255, 255, 255)
invButton.Font = Enum.Font.SourceSansBold
invButton.TextSize = 16
invButton.Draggable = true 
invButton.Parent = screenGui

uiCorner.Parent = invButton

-- Função para ocultar/mostrar partes do personagem
local function setCharacterVisibility(visible)
    if not character then return end
    
    for _, descendant in pairs(character:GetDescendants()) do
        if descendant:IsA("BasePart") then
            descendant.Transparency = visible and 0 or 1
        elseif descendant:IsA("Decal") or descendant:IsA("Texture") then
            descendant.Transparency = visible and 0 or 1
        elseif descendant:IsA("Accessory") or descendant:IsA("Hat") then
            if not visible then
                descendant:Destroy()
            end
        end
    end
    
    local head = character:FindFirstChild("Head")
    if head then
        local billboard = head:FindFirstChild("BillboardGui")
        if billboard then
            billboard.Enabled = visible
        end
    end
end

-- Função para tornar invisível
local function toggleInvisibility()
    isInvisible = not isInvisible
    character = player.Character
    if not character then return end
    
    if isInvisible then
        invButton.Text = "Invisibilidade: ON"
        invButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
        setCharacterVisibility(false)
    else
        invButton.Text = "Invisibilidade: OFF"
        invButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
        humanoid.Health = 0
    end
end

invButton.MouseButton1Click:Connect(toggleInvisibility)

player.CharacterAdded:Connect(function(newChar)
    character = newChar
    humanoid = newChar:WaitForChild("Humanoid")
    if isInvisible then
        task.wait(1)
        setCharacterVisibility(false)
    end
end)


