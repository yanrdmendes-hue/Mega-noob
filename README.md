local players = game:GetService("Players")
local replicatedStorage = game:GetService("ReplicatedStorage")

local lp = players.LocalPlayer
local punch = replicatedStorage:WaitForChild("Remotes"):WaitForChild("Punch")
local areas = workspace:WaitForChild("Areas")

_G.AuraEnabled = true

local function getTarget()
    local char = lp.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") then return end
    
    local target, dist = nil, 25 --- u can change it whatever u want 
    
    for _, area in pairs(areas:GetChildren()) do
        local holder = area:FindFirstChild("EnemyHolder")
        if holder then
            for _, enemy in pairs(holder:GetChildren()) do
                local root = enemy:FindFirstChild("HumanoidRootPart") or enemy:FindFirstChild("Head")
                if root then
                    local mag = (char.HumanoidRootPart.Position - root.Position).Magnitude
                    if mag < dist then
                        dist = mag
                        target = enemy
                    end
                end
            end
        end
    end
    return target
end

task.spawn(function()
    while task.wait(0.1) do
        if _G.AuraEnabled then
            local enemy = getTarget()
            if enemy then
                punch:FireServer(enemy)
            end
        end
    end
end)
