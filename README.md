
-- Muscle Legends - Script Completo com GUI
-- Baseado no script "KING SUPREMACY" (adaptado)

if game.PlaceId ~= 155615604 then 
    return 
end

local redzlib = loadstring(game:HttpGet("https://raw.githubusercontent.com/2581235867/21/refs/heads/main/By%20Tokattk"))()
local Window = redzlib:MakeWindow({
    Title = "Muscle Legends - Auto Farm",
    SubTitle = "Auto Força | OP Edition",
    SaveFolder = "MLAutoFarmConfig"
})

local LP = game:GetService("Players").LocalPlayer
local RS = game:GetService("ReplicatedStorage")
local MuscleEvent = LP:FindFirstChild("MuscleEvent") or LP.Character:FindFirstChild("MuscleEvent")

-- Aba Principal
local MainTab = Window:MakeTab({"⚡ Auto Farm", "home"})

MainTab:AddToggle({
    Name = "Auto Força (Ultra Fast Rep)",
    Default = false,
    Callback = function(Value)
        _G.FastRep = Value
        task.spawn(function()
            while _G.FastRep do
                pcall(function()
                    MuscleEvent:FireServer("punch", "leftHand")
                    MuscleEvent:FireServer("punch", "rightHand")
                end)
                task.wait(0.0001)
            end
        end)
    end
})

MainTab:AddToggle({
    Name = "Auto Rebirth",
    Default = false,
    Callback = function(Value)
        _G.Rebirth = Value
        task.spawn(function()
            while _G.Rebirth do
                RS.rEvents.rebirthRemote:InvokeServer("rebirthRequest")
                task.wait(5)
            end
        end)
    end
})

-- Aba de Combate
local CombatTab = Window:MakeTab({"⚔️ Combate", "swords"})

CombatTab:AddToggle({
    Name = "Kill Aura (Raio 25)",
    Default = false,
    Callback = function(Value)
        _G.KillAura = Value
        task.spawn(function()
            while _G.KillAura do
                for _, v in pairs(game.Players:GetPlayers()) do
                    if v ~= LP and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                        local dist = (LP.Character.HumanoidRootPart.Position - v.Character.HumanoidRootPart.Position).Magnitude
                        if dist < 25 then
                            MuscleEvent:FireServer("punch", "leftHand")
                            MuscleEvent:FireServer("punch", "rightHand")
                        end
                    end
                end
                task.wait(0.1)
            end
        end)
    end
})

-- Aba de Segurança
local StealthTab = Window:MakeTab({"🛡️ Segurança", "shield"})

StealthTab:AddToggle({
    Name = "Sky Farm (Anti-Report)",
    Default = false,
    Callback = function(Value)
        _G.SkyFarm = Value
        task.spawn(function()
            while _G.SkyFarm do
                pcall(function()
                    LP.Character.HumanoidRootPart.CFrame = CFrame.new(
                        LP.Character.HumanoidRootPart.Position.X, 
                        5000, 
                        LP.Character.HumanoidRootPart.Position.Z
                    )
                end)
                task.wait(1)
            end
        end)
    end
})

print("[ML Auto Farm] Script carregado com sucesso!")
