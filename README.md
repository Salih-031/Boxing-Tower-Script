-- GUI Oluşturma
local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local Button = Instance.new("TextButton")
local UIS = game:GetService("UserInputService")

ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

Frame.Size = UDim2.new(0, 200, 0, 100)
Frame.Position = UDim2.new(0.5, -100, 0.5, -50)
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Frame.Active = true
Frame.Draggable = true
Frame.Parent = ScreenGui

Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
Title.Text = "Boxing Tower Script"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 18
Title.Parent = Frame

Button.Size = UDim2.new(1, 0, 0, 50)
Button.Position = UDim2.new(0, 0, 0.4, 0)
Button.BackgroundColor3 = Color3.fromRGB(70, 130, 180)
Button.Text = "Troll Başlat"
Button.TextColor3 = Color3.fromRGB(255, 255, 255)
Button.Font = Enum.Font.SourceSansBold
Button.TextSize = 18
Button.Parent = Frame

-- Troll Fonksiyon
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")

Button.MouseButton1Click:Connect(function()
    local target = nil
    
    -- En yakındaki oyuncuyu bul
    local shortestDistance = math.huge
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local distance = (player.Character.HumanoidRootPart.Position - LocalPlayer.Character.HumanoidRootPart.Position).magnitude
            if distance < shortestDistance then
                shortestDistance = distance
                target = player
            end
        end
    end
    
    -- Arkasına ışınlan
    if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            local behindPosition = target.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 2) -- yakın mesafe
            hrp.CFrame = behindPosition
            
            -- Orbit etkisi (1 saniye)
            local conn
            conn = RunService.Heartbeat:Connect(function()
                if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
                    hrp.CFrame = target.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 2)
                else
                    conn:Disconnect()
                end
            end)
            
            wait(1) -- burada 2'ydi, 1 saniyeye düşürüldü
            conn:Disconnect()
        end
    end
end)
