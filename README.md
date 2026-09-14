--[[
=========================================================
                     CLAN CLZ
                  CRIADOR: MITO
=========================================================
        Muscle Legends - CLAN CLZ HUB
        Baseado na estrutura enviada pelo usuário
=========================================================
]]

-- =========================================================
-- VERIFICAÇÃO DO JOGO
-- =========================================================

if game.PlaceId ~= 155615604 then
    return
end

-- =========================================================
-- CARREGAMENTO DA INTERFACE
-- =========================================================

local redzlib = loadstring(game:HttpGet(
    "https://raw.githubusercontent.com/2581235867/21/refs/heads/main/By%20Tokattk"
))()

local Window = redzlib:MakeWindow({
    Title = "CLAN CLZ",
    SubTitle = "Criador: MITO | Muscle Legends",
    SaveFolder = "CLANCLZConfig"
})

-- =========================================================
-- SERVIÇOS
-- =========================================================

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local LP = Players.LocalPlayer

repeat
    task.wait()
until LP.Character

local Character = LP.Character

local MuscleEvent =
    LP:FindFirstChild("MuscleEvent")
    or Character:FindFirstChild("MuscleEvent")

if not MuscleEvent then
    warn("[CLAN CLZ] MuscleEvent não encontrado.")
    return
end

-- =========================================================
-- AUTO FARM
-- =========================================================

local MainTab = Window:MakeTab({
    {"⚡ Auto Farm", "home"}
})

MainTab:AddToggle({
    Name = "Auto Força",
    Default = false,

    Callback = function(Value)

        _G.CLZAutoStrength = Value

        task.spawn(function()

            while _G.CLZAutoStrength do

                pcall(function()
                    MuscleEvent:FireServer("punch", "leftHand")
                    MuscleEvent:FireServer("punch", "rightHand")
                end)

                task.wait(0.0001)
            end

        end)
    end
})

-- =========================================================
-- AUTO REBIRTH
-- =========================================================

MainTab:AddToggle({
    Name = "Auto Rebirth",
    Default = false,

    Callback = function(Value)

        _G.CLZAutoRebirth = Value

        task.spawn(function()

            while _G.CLZAutoRebirth do

                pcall(function()
                    ReplicatedStorage.rEvents.rebirthRemote:InvokeServer(
                        "rebirthRequest"
                    )
                end)

                task.wait(5)
            end

        end)
    end
})

-- =========================================================
-- COMBATE
-- =========================================================

local CombatTab = Window:MakeTab({
    {"⚔️ Combate", "swords"}
})

CombatTab:AddToggle({
    Name = "Kill Aura (Raio 25)",
    Default = false,

    Callback = function(Value)

        _G.CLZKillAura = Value

        task.spawn(function()

            while _G.CLZKillAura do

                local MyCharacter = LP.Character

                if MyCharacter
                and MyCharacter:FindFirstChild("HumanoidRootPart") then

                    local MyRoot = MyCharacter.HumanoidRootPart

                    for _, Player in pairs(Players:GetPlayers()) do

                        if Player ~= LP
                        and Player.Character
                        and Player.Character:FindFirstChild("HumanoidRootPart") then

                            local TargetRoot =
                                Player.Character.HumanoidRootPart

                            local Distance =
                                (MyRoot.Position - TargetRoot.Position).Magnitude

                            if Distance < 25 then

                                pcall(function()
                                    MuscleEvent:FireServer(
                                        "punch",
                                        "leftHand"
                                    )

                                    MuscleEvent:FireServer(
                                        "punch",
                                        "rightHand"
                                    )
                                end)

                            end
                        end
                    end
                end

                task.wait(0.1)
            end

        end)
    end
})

-- =========================================================
-- SEGURANÇA
-- =========================================================

local StealthTab = Window:MakeTab({
    {"🛡️ Segurança", "shield"}
})

StealthTab:AddToggle({
    Name = "Sky Farm",
    Default = false,

    Callback = function(Value)

        _G.CLZSkyFarm = Value

        task.spawn(function()

            while _G.CLZSkyFarm do

                pcall(function()

                    local CurrentCharacter = LP.Character

                    if CurrentCharacter
                    and CurrentCharacter:FindFirstChild("HumanoidRootPart") then

                        local Root =
                            CurrentCharacter.HumanoidRootPart

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

local InfoTab = Window:MakeTab({
    {"👑 CLZ", "info"}
})

InfoTab:AddParagraph({
    "CLAN CLZ",
    "Hub do Clan CLZ"
})

InfoTab:AddParagraph({
    "Criador",
    "MITO"
})

InfoTab:AddParagraph({
    "Jogo",
    "Muscle Legends"
})

-- =========================================================
-- FINALIZAÇÃO
-- =========================================================

print("========================================")
print("             CLAN CLZ")
print("          Criador: MITO")
print("       Muscle Legends Hub")
print("========================================")
print("[CLAN CLZ] Script carregado com sucesso!")
