--[[
=========================================================
                    CLAN CLZ
                  Criador: MITO
=========================================================
Muscle Legends - Script Completo com GUI
--]]

if game.PlaceId ~= 155615604 then 
    return 
end

local redzlib = loadstring(game:HttpGet("https://github.com/MITORYAN/scriptmusclelegendstest1/blob/main/README.md"))()

local Window = redzlib:MakeWindow({
    Title = "CLAN CLZ",
    SubTitle = "Criador: MITO | Muscle Legends",
    SaveFolder = "CLANCLZConfig"
})

local LP = game:GetService("Players").LocalPlayer
local RS = game:GetService("ReplicatedStorage")

local MuscleEvent = LP:FindFirstChild("MuscleEvent") 
    or (LP.Character and LP.Character:FindFirstChild("MuscleEvent"))

if not MuscleEvent then
    warn("[CLAN CLZ] MuscleEvent não encontrado.")
    return
end

-- =========================================================
-- ABA PRINCIPAL
-- =========================================================

local MainTab = Window:MakeTab({
    {"⚡ Auto Farm", "home"}
})

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
                pcall(function()
                    RS.rEvents.rebirthRemote:InvokeServer("rebirthRequest")
                end)

                task.wait(5)
            end
        end)
    end
})

-- =========================================================
-- ABA DE COMBATE
-- =========================================================

local CombatTab = Window:MakeTab({
    {"⚔️ Combate", "swords"}
})

CombatTab:AddToggle({
    Name = "Kill Aura (Raio 25)",
    Default = false,

    Callback = function(Value)
        _G.KillAura = Value

        task.spawn(function()
            while _G.KillAura do

                for _, v in pairs(game.Players:GetPlayers()) do

                    if v ~= LP 
                    and v.Character 
                    and v.Character:FindFirstChild("HumanoidRootPart")
                    and LP.Character
                    and LP.Character:FindFirstChild("HumanoidRootPart") then

                        local dist =
                            (LP.Character.HumanoidRootPart.Position
                            - v.Character.HumanoidRootPart.Position).Magnitude

                        if dist < 25 then
                            pcall(function()
                                MuscleEvent:FireServer("punch", "leftHand")
                                MuscleEvent:FireServer("punch", "rightHand")
                            end)
                        end
                    end
                end

                task.wait(0.1)
            end
        end)
    end
})

-- =========================================================
-- ABA DE SEGURANÇA
-- =========================================================

local StealthTab = Window:MakeTab({
    {"🛡️ Segurança", "shield"}
})

StealthTab:AddToggle({
    Name = "Sky Farm",
    Default = false,

    Callback = function(Value)
        _G.SkyFarm = Value

        task.spawn(function()
            while _G.SkyFarm do

                pcall(function()
                    if LP.Character
                    and LP.Character:FindFirstChild("HumanoidRootPart") then

                        local Root = LP.Character.HumanoidRootPart
                        local Position = Root.Position

                        Root.CFrame = CFrame.new(
                            Position.X,
                            5000,
                            Position.Z
                        )
                    end
                end)

                task.wait(1)
            end
        end)
    end
})

-- =========================================================
-- INFORMAÇÕES
-- =========================================================

print("==========================================")
print("              CLAN CLZ")
print("            Criador: MITO")
print("==========================================")
print("Muscle Legends Hub carregado!")
print("==========================================")
