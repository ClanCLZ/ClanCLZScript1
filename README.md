--[[
    Clan CLZ Script | By: MITO - v2 (auto-detect remotes)
    Corrigido: encontra MuscleEvent / RebirthRemote automaticamente
]]

local Players = game:GetService("Players")
local RS      = game:GetService("ReplicatedStorage")
local UIS     = game:GetService("UserInputService")
local VUser   = game:GetService("VirtualUser")
local LP      = Players.LocalPlayer

repeat task.wait() until LP.Character and LP.Character:FindFirstChild("HumanoidRootPart")
local Char = LP.Character
local HRP  = Char:WaitForChild("HumanoidRootPart")
local Hum  = Char:WaitForChild("Humanoid")

-- =====================================================
-- AUTO-DETECT REMOTES
-- =====================================================
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
                        if n:find(kw) then
                            return v
                        end
                    end
                end
            end
        end
    end
    return nil
end

local MuscleEvent  = findRemote({"muscle", "punch", "hit", "attack", "remote"})
local RebirthRemote = findRemote({"rebirth"})

print("✅ MuscleEvent encontrado em:", MuscleEvent and MuscleEvent:GetFullName() or "NÃO ENCONTRADO")
print("✅ RebirthRemote encontrado em:", RebirthRemote and RebirthRemote:GetFullName() or "NÃO ENCONTRADO")

-- Se não achou nada, para aqui
if not MuscleEvent then
    warn("⚠️ Nenhum remote de soco encontrado! Rode o diagnóstico.")
end

-- =====================================================
-- ESTADOS
-- =====================================================
local S = {
    AutoSoco    = false,
    AutoFlexao  = false,
    AutoPeso    = false,
    AutoRebirth = false,
    AutoComprar = false,
    AutoColetar = false,
    KillAura    = false,
    AntiAFK     = false,
    SkyFarm     = false,
    ESP         = false,
    Speed       = false,
    Teleport    = false,
}

-- =====================================================
-- GUI
-- =====================================================
local gui = Instance.new("ScreenGui")
gui.Name = "ClanCLZScript"
gui.ResetOnSpawn = false
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
gui.Parent = LP:WaitForChild("PlayerGui")

local main = Instance.new("Frame")
main.Size = UDim2.new(0, 270, 0, 440)
main.Position = UDim2.new(0.02, 0, 0.15, 0)
main.BackgroundColor3 = Color3.fromRGB(18, 18, 22)
main.BorderSizePixel = 0
main.Active = true
main.Parent = gui
Instance.new("UICorner", main).CornerRadius = UDim.new(0, 10)

local stroke = Instance.new("UIStroke", main)
stroke.Color = Color3.fromRGB(180, 30, 30)
stroke.Thickness = 1.5
stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

local top = Instance.new("Frame")
top.Size = UDim2.new(1, 0, 0, 36)
top.BackgroundColor3 = Color3.fromRGB(180, 30, 30)
top.BorderSizePixel = 0
top.Parent = main
Instance.new("UICorner", top).CornerRadius = UDim.new(0, 10)

local topFix = Instance.new("Frame")
topFix.Size = UDim2.new(1, 0, 0, 12)
topFix.Position = UDim2.new(0, 0, 1, -12)
topFix.BackgroundColor3 = Color3.fromRGB(180, 30, 30)
topFix.BorderSizePixel = 0
topFix.Parent = top

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -70, 1, 0)
title.Position = UDim2.new(0, 10, 0, 0)
title.BackgroundTransparency = 1
title.Text = "Clan CLZ Script | By: MITO"
title.TextColor3 = Color3.new(1, 1, 1)
title.Font = Enum.Font.GothamBold
title.TextSize = 13
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = top

local minimize = Instance.new("TextButton")
minimize.Size = UDim2.new(0, 26, 0, 26)
minimize.Position = UDim2.new(1, -60, 0, 5)
minimize.BackgroundColor3 = Color3.fromRGB(255, 200, 0)
minimize.Text = "-"
minimize.TextColor3 = Color3.fromRGB(30, 30, 30)
minimize.Font = Enum.Font.GothamBold
minimize.TextSize = 18
minimize.BorderSizePixel = 0
minimize.Parent = top
Instance.new("UICorner", minimize).CornerRadius = UDim.new(0, 5)

local close = Instance.new("TextButton")
close.Size = UDim2.new(0, 26, 0, 26)
close.Position = UDim2.new(1, -30, 0, 5)
close.BackgroundColor3 = Color3.fromRGB(220, 60, 60)
close.Text = "X"
close.TextColor3 = Color3.new(1, 1, 1)
close.Font = Enum.Font.GothamBold
close.TextSize = 13
close.BorderSizePixel = 0
close.Parent = top
Instance.new("UICorner", close).CornerRadius = UDim.new(0, 5)

local footer = Instance.new("TextLabel")
footer.Size = UDim2.new(1, 0, 0, 20)
footer.Position = UDim2.new(0, 0, 1, -22)
footer.BackgroundTransparency = 1
footer.Text = "By: MITO  •  Xeno Edition"
footer.TextColor3 = Color3.fromRGB(140, 140, 140)
footer.Font = Enum.Font.Gotham
footer.TextSize = 11
footer.Parent = main

local scroll = Instance.new("ScrollingFrame")
scroll.Size = UDim2.new(1, -16, 1, -80)
scroll.Position = UDim2.new(0, 8, 0, 44)
scroll.BackgroundTransparency = 1
scroll.BorderSizePixel = 0
scroll.ScrollBarThickness = 4
scroll.ScrollBarImageColor3 = Color3.fromRGB(180, 30, 30)
scroll.CanvasSize = UDim2.new(0, 0, 0, 0)
scroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
scroll.Parent = main

local list = Instance.new("UIListLayout")
list.Padding = UDim.new(0, 6)
list.SortOrder = Enum.SortOrder.LayoutOrder
list.Parent = scroll

local function makeToggle(name, key, onChange)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -6, 0, 34)
    btn.BackgroundColor3 = Color3.fromRGB(38, 38, 46)
    btn.BorderSizePixel = 0
    btn.Text = name .. ": OFF"
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 12
    btn.TextXAlignment = Enum.TextXAlignment.Left
    btn.Parent = scroll
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
    local pad = Instance.new("UIPadding", btn)
    pad.PaddingLeft = UDim.new(0, 10)

    btn.MouseButton1Click:Connect(function()
        S[key] = not S[key]
        btn.Text = name .. ": " .. (S[key] and "ON" or "OFF")
        btn.BackgroundColor3 = S[key] and Color3.fromRGB(0, 150, 60)
                                      or Color3.fromRGB(38, 38, 46)
        if onChange then onChange(S[key]) end
    end)
end

-- drag
local dragging, dragStart, startPos
top.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
    or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = main.Position
    end
end)
UIS.InputChanged:Connect(function(input)
    if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement
    or input.UserInputType == Enum.UserInputType.Touch) then
        local d = input.Position - dragStart
        main.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + d.X,
                                  startPos.Y.Scale, startPos.Y.Offset + d.Y)
    end
end)
UIS.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
    or input.UserInputType == Enum.UserInputType.Touch then
        dragging = false
    end
end)

minimize.MouseButton1Click:Connect(function()
    scroll.Visible = not scroll.Visible
    footer.Visible = not footer.Visible
    main.Size = scroll.Visible and UDim2.new(0, 270, 0, 440)
                            or UDim2.new(0, 270, 0, 36)
end)

close.MouseButton1Click:Connect(function() gui:Destroy() end)

-- =====================================================
-- TOGGLES
-- =====================================================
makeToggle("🥊 Auto Soco (Força)",       "AutoSoco")
makeToggle("💪 Auto Flexão",             "AutoFlexao")
makeToggle("🏋️ Auto Peso (Academia)",   "AutoPeso")
makeToggle("🔄 Auto Rebirth",            "AutoRebirth")
makeToggle("🛒 Auto Comprar",            "AutoComprar")
makeToggle("💰 Auto Coletar",            "AutoColetar")
makeToggle("⚔️ Kill Aura",               "KillAura")
makeToggle("🛡️ Anti-AFK",                "AntiAFK")
makeToggle("☁️ Sky Farm (Anti-Report)",  "SkyFarm")
makeToggle("👁️ ESP (Ver Players)",       "ESP")
makeToggle("💨 Speed (Velocidade)",      "Speed", function(v)
    if Hum then Hum.WalkSpeed = v and 100 or 16 end
end)
makeToggle("🌀 Teleport (segue mouse)","Teleport")

-- =====================================================
-- FUNÇÕES
-- =====================================================

-- Função universal de soco: tenta VÁRIOS formatos diferentes
local function tryPunch()
    if not MuscleEvent then return end
    pcall(function() MuscleEvent:FireServer("punch", "leftHand") end)
    pcall(function() MuscleEvent:FireServer("punch", "rightHand") end)
    pcall(function() MuscleEvent:FireServer("punch") end)
    pcall(function() MuscleEvent:FireServer() end)
end

-- AUTO SOCO
task.spawn(function()
    while task.wait() do
        if S.AutoSoco then tryPunch() end
    end
end)

-- AUTO FLEXÃO
task.spawn(function()
    while task.wait(0.05) do
        if S.AutoFlexao and MuscleEvent then
            pcall(function()
                MuscleEvent:FireServer("flex")
                MuscleEvent:FireServer("situps")
                MuscleEvent:FireServer("pushups")
                MuscleEvent:FireServer("squats")
            end)
        end
    end
end)

-- AUTO PESO
task.spawn(function()
    while task.wait(0.3) do
        if S.AutoPeso then
            pcall(function()
                for _, v in ipairs(workspace:GetDescendants()) do
                    if v:IsA("ProximityPrompt") and v.Enabled and v.Parent
                    and v.Parent:IsA("BasePart") then
                        local d = (HRP.Position - v.Parent.Position).Magnitude
                        if d < 30 then fireproximityprompt(v) end
                    end
                end
            end)
        end
    end
end)

-- AUTO REBIRTH
task.spawn(function()
    while task.wait(3) do
        if S.AutoRebirth and RebirthRemote then
            pcall(function()
                if RebirthRemote:IsA("RemoteFunction") then
                    RebirthRemote:InvokeServer("rebirthRequest")
                else
                    RebirthRemote:FireServer("rebirthRequest")
                end
            end)
        end
    end
end)

-- AUTO COMPRAR
task.spawn(function()
    while task.wait(1) do
        if S.AutoComprar then
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
        end
    end
end)

-- AUTO COLETAR
task.spawn(function()
    while task.wait(0.3) do
        if S.AutoColetar then
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
        end
    end
end)

-- KILL AURA
task.spawn(function()
    while task.wait(0.05) do
        if S.KillAura then
            for _, p in ipairs(Players:GetPlayers()) do
                if p ~= LP and p.Character then
                    local tH = p.Character:FindFirstChild("HumanoidRootPart")
                    if tH and (HRP.Position - tH.Position).Magnitude < 25 then
                        tryPunch()
                    end
                end
            end
        end
    end
end)

-- ANTI-AFK
LP.Idled:Connect(function()
    if S.AntiAFK then
        VUser:CaptureController()
        VUser:ClickButton2(Vector2.new())
    end
end)

-- SKY FARM
task.spawn(function()
    while task.wait(1) do
        if S.SkyFarm and HRP then
            pcall(function() HRP.CFrame = CFrame.new(HRP.Position.X, 5000, HRP.Position.Z) end)
        end
    end
end)

-- ESP
local espCache = {}
task.spawn(function()
    while task.wait(0.3) do
        for _, p in ipairs(Players:GetPlayers()) do
            if p ~= LP then
                if S.ESP and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                    if not espCache[p] or not espCache[p].Parent then
                        local hl = Instance.new("Highlight")
                        hl.FillColor = Color3.fromRGB(255, 40, 40)
                        hl.OutlineColor = Color3.new(1,1,1)
                        hl.FillTransparency = 0.5
                        hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
                        hl.Parent = p.Character.HumanoidRootPart
                        espCache[p] = hl
                    end
                elseif espCache[p] then
                    espCache[p]:Destroy()
                    espCache[p] = nil
                end
            end
        end
    end
end)

-- TELEPORT
task.spawn(function()
    while task.wait(0.15) do
        if S.Teleport and HRP then
            pcall(function()
                local m = LP:GetMouse()
                if m and m.Hit then
                    HRP.CFrame = CFrame.new(m.Hit.Position + Vector3.new(0, 3, 0))
                end
            end)
        end
    end
end)

-- RESPAWN
LP.CharacterAdded:Connect(function(c)
    Char = c
    HRP  = c:WaitForChild("HumanoidRootPart")
    Hum  = c:WaitForChild("Humanoid")
    task.wait(1)
    MuscleEvent = findRemote({"muscle", "punch", "hit", "attack", "remote"})
    if S.Speed and Hum then Hum.WalkSpeed = 100 end
end)

print("✅ Clan CLZ Script | By: MITO v2 carregado!")
print("MuscleEvent:", MuscleEvent and MuscleEvent:GetFullName() or "❌ NÃO ACHOU")
