--[[
    ██████╗██╗      █████╗ ███╗   ██╗     ██████╗██╗     ███████╗
   ██╔════╝██║     ██╔══██╗████╗  ██║    ██╔════╝██║     ╚══███╔╝
   ██║     ██║     ███████║██╔██╗ ██║    ██║     ██║       ███╔╝
   ██║     ██║     ██╔══██║██║╚██╗██║    ██║     ██║      ███╔╝
   ╚██████╗███████╗██║  ██║██║ ╚████║    ╚██████╗███████╗███████╗
    ╚═════╝╚══════╝╚═╝  ╚═╝╚═╝  ╚═══╝     ╚═════╝╚══════╝╚══════╝

               Clan CLZ Script | By: MITO
               Jogo: Muscle Legends
               Executor: Xeno | Delta | Wave | Solara
]]

-- ==== VERIFICAÇÃO DE JOGO ====
if game.PlaceId ~= 155615604 then
    return
end

-- ==== CARREGA REDZLIB ====
local redzlib = loadstring(game:HttpGet("https://raw.githubusercontent.com/REDzHUB/RedzLibV5/main/Source.Lua"))()

-- ==== JANELA PRINCIPAL ====
local Window = redzlib:MakeWindow({
    Title    = "Clan CLZ Script | By: MITO",
    SubTitle = "Muscle Legends — OP Edition",
    SaveFolder = "ClanCLZConfig"
})

-- ==== SERVIÇOS ====
local Players = game:GetService("Players")
local RS      = game:GetService("ReplicatedStorage")
local VUser   = game:GetService("VirtualUser")
local LP      = Players.LocalPlayer

repeat task.wait() until LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")
local Char = LP.Character
local HRP  = Char:WaitForChild("HumanoidRootPart")
local Hum  = Char:WaitForChild("Humanoid")

-- ==== AUTO-DETECT DE REMOTES ====
local function findRemote(keywords)
    local places = {
        LP,
        LP.Character,
        RS,
        RS:FindFirstChild("rEvents"),
    }
    for _, c in ipairs(places) do
        if c then
            for _, v in ipairs(c:GetDescendants()) do
                if v:IsA("RemoteEvent") or v:IsA("RemoteFunction") then
                    local n = v.Name:lower()
                    for _, kw in ipairs(keywords) do
                        if n:find(kw) then return v end
                    end
                end
            end
        end
    end
    return nil
end

local MuscleEvent   = findRemote({"muscle", "punch", "hit", "attack"})
local RebirthRemote = findRemote({"rebirth"})

print("✅ Clan CLZ Script carregado!")
print("MuscleEvent:", MuscleEvent and MuscleEvent:GetFullName() or "❌ não achou")
print("RebirthRemote:", RebirthRemote and RebirthRemote:GetFullName() or "❌ não achou")

-- Tenta vários formatos de soco (pra cobrir todas as versões do jogo)
local function tryPunch()
    if not MuscleEvent then return end
    pcall(function() MuscleEvent:FireServer("punch", "leftHand")  end)
    pcall(function() MuscleEvent:FireServer("punch", "rightHand") end)
    pcall(function() MuscleEvent:FireServer("punch")               end)
    pcall(function() MuscleEvent:FireServer()                      end)
end

-- ═══════════════════════════════════════════════════════
--  ABA 1: ⚡ AUTO FARM
-- ═══════════════════════════════════════════════════════
local FarmTab = Window:MakeTab({"⚡ Auto Farm", "home"})

FarmTab:AddToggle({
    Name = "🥊 Auto Soco (Força Ultra Rápida)",
    Default = false,
    Callback = function(Value)
        _G.AutoSoco = Value
        task.spawn(function()
            while _G.AutoSoco do
                tryPunch()
                task.wait(0.0001)
            end
        end)
    end
})

FarmTab:AddToggle({
    Name = "💪 Auto Flexão",
    Default = false,
    Callback = function(Value)
        _G.AutoFlexao = Value
        task.spawn(function()
            while _G.AutoFlexao do
                if MuscleEvent then
                    pcall(function()
                        MuscleEvent:FireServer("flex")
                        MuscleEvent:FireServer("situps")
                        MuscleEvent:FireServer("pushups")
                        MuscleEvent:FireServer("squats")
                    end)
                end
                task.wait(0.05)
            end
        end)
    end
})

FarmTab:AddToggle({
    Name = "🏋️ Auto Peso (Academia)",
    Default = false,
    Callback = function(Value)
        _G.AutoPeso = Value
        task.spawn(function()
            while _G.AutoPeso do
                pcall(function()
                    for _, v in ipairs(workspace:GetDescendants()) do
                        if v:IsA("ProximityPrompt") and v.Enabled and v.Parent
                        and v.Parent:IsA("BasePart") then
                            local d = (HRP.Position - v.Parent.Position).Magnitude
                            if d < 30 then fireproximityprompt(v) end
                        end
                    end
                end)
                task.wait(0.3)
            end
        end)
    end
})

FarmTab:AddToggle({
    Name = "🔄 Auto Rebirth",
    Default = false,
    Callback = function(Value)
        _G.AutoRebirth = Value
        task.spawn(function()
            while _G.AutoRebirth do
                if RebirthRemote then
                    pcall(function()
                        if RebirthRemote:IsA("RemoteFunction") then
                            RebirthRemote:InvokeServer("rebirthRequest")
                        else
                            RebirthRemote:FireServer("rebirthRequest")
                        end
                    end)
                end
                task.wait(3)
            end
        end)
    end
})

FarmTab:AddToggle({
    Name = "🛒 Auto Comprar",
    Default = false,
    Callback = function(Value)
        _G.AutoComprar = Value
        task.spawn(function()
            while _G.AutoComprar do
                pcall(function()
                    for _, v in ipairs(workspace:GetDescendants()) do
                        if v:IsA("ProximityPrompt") and v.Enabled and v.Parent
                        and v.Parent:IsA("BasePart") then
                            local n = (v.Parent.Name .. v.ActionText):lower()
                            if n:find("buy") or n:find("shop") or n:find("comprar") then
                                local d = (HRP.Position - v.Parent.Position).Magnitude
                                if d < 30 then fireproximityprompt(v) end
                            end
                        end
                    end
                end)
                task.wait(1)
            end
        end)
    end
})

FarmTab:AddToggle({
    Name = "💰 Auto Coletar (Itens no chão)",
    Default = false,
    Callback = function(Value)
        _G.AutoColetar = Value
        task.spawn(function()
            while _G.AutoColetar do
                pcall(function()
                    for _, v in ipairs(workspace:GetDescendants()) do
                        if v:IsA("BasePart") and v.CanTouch then
                            local n = v.Name:lower()
                            if n:find("coin") or n:find("cash")
                            or n:find("gem")  or n:find("drop") then
                                local d = (HRP.Position - v.Position).Magnitude
                                if d < 20 then
                                    firetouchinterest(HRP, v, 0)
                                    firetouchinterest(HRP, v, 1)
                                end
                            end
                        end
                    end
                end)
                task.wait(0.3)
            end
        end)
    end
})

-- ═══════════════════════════════════════════════════════
--  ABA 2: ⚔️ COMBATE
-- ═══════════════════════════════════════════════════════
local CombatTab = Window:MakeTab({"⚔️ Combate", "swords"})

CombatTab:AddToggle({
    Name = "⚔️ Kill Aura (Raio 25)",
    Default = false,
    Callback = function(Value)
        _G.KillAura = Value
        task.spawn(function()
            while _G.KillAura do
                for _, p in ipairs(Players:GetPlayers()) do
                    if p ~= LP and p.Character then
                        local tH = p.Character:FindFirstChild("HumanoidRootPart")
                        if tH and (HRP.Position - tH.Position).Magnitude < 25 then
                            tryPunch()
                        end
                    end
                end
                task.wait(0.05)
            end
        end)
    end
})

CombatTab:AddToggle({
    Name = "👁️ ESP (Ver Players)",
    Default = false,
    Callback = function(Value)
        _G.ESP = Value
        task.spawn(function()
            local cache = _G.ESPCache or {}
            _G.ESPCache = cache
            while _G.ESP do
                for _, p in ipairs(Players:GetPlayers()) do
                    if p ~= LP then
                        if p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                            if not cache[p] or not cache[p].Parent then
                                local hl = Instance.new("Highlight")
                                hl.FillColor = Color3.fromRGB(255, 40, 40)
                                hl.OutlineColor = Color3.new(1, 1, 1)
                                hl.FillTransparency = 0.5
                                hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
                                hl.Parent = p.Character.HumanoidRootPart
                                cache[p] = hl
                            end
                        elseif cache[p] then
                            cache[p]:Destroy()
                            cache[p] = nil
                        end
                    end
                end
                task.wait(0.3)
            end
            for p, hl in pairs(cache) do
                if hl then hl:Destroy() end
                cache[p] = nil
            end
        end)
    end
})

-- ═══════════════════════════════════════════════════════
--  ABA 3: 🛡️ SEGURANÇA
-- ═══════════════════════════════════════════════════════
local StealthTab = Window:MakeTab({"🛡️ Segurança", "shield"})

StealthTab:AddToggle({
    Name = "🛡️ Anti-AFK",
    Default = false,
    Callback = function(Value)
        _G.AntiAFK = Value
    end
})

LP.Idled:Connect(function()
    if _G.AntiAFK then
        VUser:CaptureController()
        VUser:ClickButton2(Vector2.new())
    end
end)

StealthTab:AddToggle({
    Name = "☁️ Sky Farm (Anti-Report)",
    Default = false,
    Callback = function(Value)
        _G.SkyFarm = Value
        task.spawn(function()
            while _G.SkyFarm do
                pcall(function()
                    HRP.CFrame = CFrame.new(HRP.Position.X, 5000, HRP.Position.Z)
                end)
                task.wait(1)
            end
        end)
    end
})

-- ═══════════════════════════════════════════════════════
--  ABA 4: 🎮 MISC
-- ═══════════════════════════════════════════════════════
local MiscTab = Window:MakeTab({"🎮 Misc", "users"})

MiscTab:AddSlider({
    Name = "💨 Velocidade (Speed)",
    Min = 16,
    Max = 200,
    Default = 16,
    Color = Color3.fromRGB(180, 30, 30),
    Increment = 1,
    ValueName = "studs",
    Callback = function(Value)
        if Hum then Hum.WalkSpeed = Value end
    end
})

MiscTab:AddSlider({
    Name = "🦘 Pulo (JumpPower)",
    Min = 50,
    Max = 300,
    Default = 50,
    Color = Color3.fromRGB(180, 30, 30),
    Increment = 5,
    ValueName = "power",
    Callback = function(Value)
        if Hum then Hum.JumpPower = Value end
    end
})

MiscTab:AddToggle({
    Name = "🌀 Teleport (segue mouse)",
    Default = false,
    Callback = function(Value)
        _G.Teleport = Value
        task.spawn(function()
            while _G.Teleport do
                pcall(function()
                    local m = LP:GetMouse()
                    if m and m.Hit then
                        HRP.CFrame = CFrame.new(m.Hit.Position + Vector3.new(0, 3, 0))
                    end
                end)
                task.wait(0.15)
            end
        end)
    end
})

-- ═══════════════════════════════════════════════════════
--  ABA 5: ℹ️ INFO
-- ═══════════════════════════════════════════════════════
local InfoTab = Window:MakeTab({"ℹ️ Info", "info"})

InfoTab:AddParagraph({
    Title = "Clan CLZ Script",
    Content = "Criado por: MITO\nVersão: 1.0\nExecutor: Xeno / Delta / Wave / Solara"
})

InfoTab:AddParagraph({
    Title = "Status dos Remotes",
    Content = "MuscleEvent: " .. (MuscleEvent and "✅ OK" or "❌ FALHOU")
        .. "\nRebirthRemote: " .. (RebirthRemote and "✅ OK" or "❌ FALHOU")
})

InfoTab:AddButton({
    Name = "🔔 Notificação de teste",
    Callback = function()
        redzlib:MakeNotification({
            Name = "Clan CLZ Script",
            Content = "Tudo funcionando! By: MITO 🔥",
            Time = 4
        })
    end
})

-- ═══════════════════════════════════════════════════════
--  RESPAWN HANDLER
-- ═══════════════════════════════════════════════════════
LP.CharacterAdded:Connect(function(c)
    Char = c
    HRP  = c:WaitForChild("HumanoidRootPart")
    Hum  = c:WaitForChild("Humanoid")
    task.wait(1)
    MuscleEvent = findRemote({"muscle", "punch", "hit", "attack"})
    RebirthRemote = findRemote({"rebirth"})
end)

print("✅ Clan CLZ Script | By: MITO carregado com sucesso!")
