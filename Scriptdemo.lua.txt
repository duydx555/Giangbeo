loadstring(game:HttpGet(("https://raw.githubusercontent.com/daucobonhi/Ui-Redz-V2/refs/heads/main/UiREDzV2.lua")))()

       local Window = MakeWindow({
         Hub = {
         Title = "SicMaboy Giang Béo",
         Animation = "Anh Giang Quá Gầy Bị SayGed"
         },
        Key = {
        KeySystem = false,
        Title = "Key System",
        Description = "",
        KeyLink = "",
        Keys = {"1234"},
        Notifi = {
        Notifications = true,
        CorrectKey = "Running the Script...",
       Incorrectkey = "The key is incorrect",
       CopyKeyLink = "Copied to Clipboard"
      }
    }
  })

       MinimizeButton({
       Image = "http://www.roblox.com/asset/?id=83190276951914",
       Size = {60, 60},
       Color = Color3.fromRGB(10, 10, 10),
       Corner = true,
       Stroke = false,
       StrokeColor = Color3.fromRGB(255, 0, 0)
      })
      
------ Tab
     local Tab1o = MakeTab({Name = "Đét Geo"})
     
------- BUTTON
    
    AddButton(Tab1o, {
     Name = "AimBot Giang Béo",
    Callback = function()
	  --dead rails wall hack aimbot.
local Players = game:GetService("Players")
local player = Players.LocalPlayer
player.CameraMode = Enum.CameraMode.Classic
local runService = game:GetService("RunService")
local StarterGui = game:GetService("StarterGui")
local camera = workspace.CurrentCamera
 
StarterGui:SetCore("SendNotification", {
    Title = "Script by Duydz", -- dont skid ðŸ™ðŸ’”
    Text = "Đét Geo",
    Duration = 3
})
 
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "GIANG_BÉO"
screenGui.Parent = game:GetService("CoreGui")
 
local button = Instance.new("TextButton")
button.Name = "Me may beo+Ba may gay: Mở/Khóa"
button.Size = UDim2.new(0, 150, 0, 50)
button.Position = UDim2.new(0.5, -75, 0.9, -25)
button.BackgroundColor3 = Color3.new(0, 0, 0)
button.TextColor3 = Color3.new(1, 1, 1)
button.Text = "NPC Lock: OFF"
button.Font = Enum.Font.Fantasy
button.TextScaled = true
button.TextSize = 20
button.Parent = screenGui
 
local uicorner = Instance.new("UICorner")
uicorner.CornerRadius = UDim.new(0, 12)
uicorner.Parent = button
 
local dragging = false
local dragInput, dragStart, startPos
 
local function update(input)
    local delta = input.Position - dragStart
    button.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end
 
button.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = button.Position
 
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)
 
button.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)
 
game:GetService("UserInputService").InputChanged:Connect(function(input)
    if dragging and input == dragInput then
        update(input)
    end
end)
 
local npcLock = false
local lastTarget = nil
local toggleLoop
 
local function addPlayerHighlight()
    if player.Character then
        local highlight = player.Character:FindFirstChild("PlayerHighlightESP")
        if not highlight then
            highlight = Instance.new("Highlight")
            highlight.Name = "PlayerHighlightESP"
            highlight.FillColor = Color3.new(1, 1, 1)
            highlight.OutlineColor = Color3.new(1, 1, 1)
            highlight.FillTransparency = 0.5
            highlight.OutlineTransparency = 0
            highlight.Parent = player.Character
        end
    end
end
 
local function removePlayerHighlight()
    if player.Character and player.Character:FindFirstChild("PlayerHighlightESP") then
        player.Character.PlayerHighlightESP:Destroy()
    end
end
 
local function getClosestNPC()
    local closestNPC = nil
    local closestDistance = math.huge
 
    for _, object in ipairs(workspace:GetDescendants()) do
        if object:IsA("Model") then
            local humanoid = object:FindFirstChild("Humanoid") or object:FindFirstChildWhichIsA("Humanoid")
            local hrp = object:FindFirstChild("HumanoidRootPart") or object.PrimaryPart
            if humanoid and hrp and humanoid.Health > 0 and object.Name ~= "Horse" then
                local isPlayer = false
                for _, pl in ipairs(Players:GetPlayers()) do
                    if pl.Character == object then
                        isPlayer = true
                        break
                    end
                end
                if not isPlayer then
                    local distance = (hrp.Position - player.Character.HumanoidRootPart.Position).Magnitude
                    if distance < closestDistance then
                        closestDistance = distance
                        closestNPC = object
                    end
                end
            end
        end
    end
 
    return closestNPC
end
 
button.MouseButton1Click:Connect(function()
    npcLock = not npcLock
    if npcLock then
        button.Text = "Me may beo: Mở"
        toggleLoop = runService.RenderStepped:Connect(function()
            local npc = getClosestNPC()
            if npc and npc:FindFirstChild("Humanoid") then
                local npcHumanoid = npc:FindFirstChild("Humanoid")
                if npcHumanoid.Health > 0 then
                    camera.CameraSubject = npcHumanoid
                    lastTarget = npc
                    addPlayerHighlight()
                else
                    StarterGui:SetCore("SendNotification", {
                        Title = "Killed NPC",
                        Text = npc.Name,
                        Duration = 0.4
                    })
                    lastTarget = nil
                    removePlayerHighlight()
                    if player.Character and player.Character:FindFirstChild("Humanoid") then
                        camera.CameraSubject = player.Character:FindFirstChild("Humanoid")
                    end
                end
            else
                if player.Character and player.Character:FindFirstChild("Humanoid") then
                    camera.CameraSubject = player.Character:FindFirstChild("Humanoid")
                end
                lastTarget = nil
                removePlayerHighlight()
            end
        end)
    else
        button.Text = "Ba may gay: khóa"
        if toggleLoop then
            toggleLoop:Disconnect()
            toggleLoop = nil
        end
        removePlayerHighlight()
        if player.Character and player.Character:FindFirstChild("Humanoid") then
            camera.CameraSubject = player.Character:FindFirstChild("Humanoid")
        end
    end
end)
  })