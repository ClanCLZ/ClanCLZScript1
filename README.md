--[[
    =========================================================
                    CLAN CLZ
                  Criador: MITO
    =========================================================
    
    Muscle Legends - Auto Farm de Força (Punch)
]]

local Players = game:GetService("Players")
local LP = Players.LocalPlayer

-- =========================================================
-- CONFIGURAÇÃO DO CLAN
-- =========================================================

local HUB_NAME = "CLAN CLZ"
local CREATOR = "MITO"

-- =========================================================
-- AGUARDA O PERSONAGEM CARREGAR
-- =========================================================

repeat
    task.wait()
until LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")

-- =========================================================
-- ENCONTRA O MUSCLE EVENT
-- =========================================================

local MuscleEvent = LP:FindFirstChild("MuscleEvent")
    or LP.Character:FindFirstChild("MuscleEvent")

if not MuscleEvent then
    warn("[" .. HUB_NAME .. "] MuscleEvent não encontrado.")
    warn("[" .. HUB_NAME .. "] Certifique-se de estar no Muscle Legends.")
    return
end

-- =========================================================
-- CONFIGURAÇÕES
-- =========================================================

local DELAY = 0.0001

_G.AutoFarmStrength = true

-- =========================================================
-- AUTO FARM DE FORÇA
-- =========================================================

task.spawn(function()
    while _G.AutoFarmStrength do
        pcall(function()
            MuscleEvent:FireServer("punch", "leftHand")
            MuscleEvent:FireServer("punch", "rightHand")
        end)

        task.wait(DELAY)
    end
end)

-- =========================================================
-- INFORMAÇÕES
-- =========================================================

print("========================================")
print("              " .. HUB_NAME)
print("           Criador: " .. CREATOR)
print("========================================")
print("Auto Farm de Força: ATIVADO")
print("Delay: " .. DELAY)
print("========================================")

-- Para desativar:
-- _G.AutoFarmStrength = false
