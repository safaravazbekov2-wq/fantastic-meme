local P, R, C, S = game:GetService("Players"), game:GetService("RunService"), game:GetService("CoreGui"), game:GetService("Stats")
local LP = P.LocalPlayer

if C:FindFirstChild("PulseHUD") then C.PulseHUD:Destroy() end
local SG = Instance.new("ScreenGui", C) SG.Name = "PulseHUD"
local HUD = Instance.new("Frame", SG) HUD.Size = UDim2.new(0, 180, 0, 35) HUD.Position = UDim2.new(0.5, -90, 0.5, -17) HUD.BackgroundColor3 = Color3.fromRGB(15, 10, 20) HUD.Active = true HUD.Draggable = true
Instance.new("UICorner", HUD).CornerRadius = UDim.new(0, 8)
local ST = Instance.new("UIStroke", HUD) ST.Color = Color3.fromRGB(170, 0, 255) ST.Thickness = 1.2
local Txt = Instance.new("TextLabel", HUD) Txt.Size = UDim2.new(1, 0, 1, 0) Txt.BackgroundTransparency = 1 Txt.Font = Enum.Font.GothamBold Txt.TextSize = 12 Txt.TextColor3 = Color3.fromRGB(230, 200, 255)

local Btn = Instance.new("TextButton", SG) Btn.Size = UDim2.new(0, 45, 0, 45) Btn.Position = UDim2.new(0.05, 0, 0.2, 0) Btn.BackgroundColor3 = Color3.fromRGB(25, 15, 35) Btn.Text = "P" Btn.TextColor3 = Color3.fromRGB(170, 0, 255) Btn.Font = Enum.Font.GothamBold Btn.TextSize = 20 Btn.Draggable = true
Instance.new("UICorner", Btn).CornerRadius = UDim.new(0, 10)
Instance.new("UIStroke", Btn).Color = Color3.fromRGB(170, 0, 255)

local Menu = Instance.new("Frame", SG) Menu.Size = UDim2.new(0, 350, 0, 220) Menu.Position = UDim2.new(0.3, 0, 0.2, 0) Menu.BackgroundColor3 = Color3.fromRGB(14, 10, 18) Menu.Visible = false Menu.Draggable = true
Instance.new("UICorner", Menu).CornerRadius = UDim.new(0, 12)
Instance.new("UIStroke", Menu).Color = Color3.fromRGB(60, 30, 80)

local Hd = Instance.new("Frame", Menu) Hd.Size = UDim2.new(1, 0, 0, 40) Hd.BackgroundColor3 = Color3.fromRGB(24, 15, 32)
Instance.new("UICorner", Hd).CornerRadius = UDim.new(0, 12)
local HT = Instance.new("TextLabel", Hd) HT.Text = " PULSE VISUALS — AGGRESSIVE BOT" HT.Font = Enum.Font.GothamBold HT.TextSize = 13 HT.TextColor3 = Color3.fromRGB(240, 220, 255) HT.Size = UDim2.new(1, 0, 1, 0) HT.Position = UDim2.new(0, 12, 0, 0) HT.TextXAlignment = Enum.TextXAlignment.Left HT.BackgroundTransparency = 1

Btn.MouseButton1Click:Connect(function() Menu.Visible = not Menu.Visible end)

local Sc = Instance.new("ScrollingFrame", Menu) Sc.Size = UDim2.new(1, -16, 1, -50) Sc.Position = UDim2.new(0, 8, 0, 46) Sc.BackgroundTransparency = 1 Sc.CanvasSize = UDim2.new(0, 0, 0, 150) Sc.ScrollBarThickness = 3
local LL = Instance.new("UIListLayout", Sc) LL.SortOrder = Enum.SortOrder.LayoutOrder LL.Padding = UDim.new(0, 6)

local function Add(txt, cb)
    local state = false
    local Bl = Instance.new("Frame", Sc) Bl.Size = UDim2.new(1, 0, 0, 38) Bl.BackgroundColor3 = Color3.fromRGB(24, 18, 30)
    Instance.new("UICorner", Bl).CornerRadius = UDim.new(0, 6)
    local Lb = Instance.new("TextLabel", Bl) Lb.Text = txt Lb.Font = Enum.Font.Gotham Lb.TextSize = 13 Lb.TextColor3 = Color3.fromRGB(230, 210, 240) Lb.Position = UDim2.new(0, 10, 0, 0) Lb.Size = UDim2.new(0, 200, 1, 0) Lb.TextXAlignment = Enum.TextXAlignment.Left Lb.BackgroundTransparency = 1
    local B = Instance.new("TextButton", Bl) B.Size = UDim2.new(0, 40, 0, 20) B.Position = UDim2.new(1, -50, 0, 9) B.BackgroundColor3 = Color3.fromRGB(55, 45, 60) B.Text = ""
    Instance.new("UICorner", B).CornerRadius = UDim.new(0, 10)
    B.MouseButton1Click:Connect(function() state = not state cb(state) B.BackgroundColor3 = state and Color3.fromRGB(170, 0, 255) or Color3.fromRGB(55, 45, 60) end)
end

local lastT, frames = os.clock(), 0
R.RenderStepped:Connect(function()
    frames = frames + 1
    if os.clock() - lastT >= 1 then
        Txt.Text = string.format("👑 PULSE | FPS: %d | %dms", frames, math.floor(S.Network.ServerStatsItem["Data Ping"]:GetValue()))
        frames, lastT = 0, os.clock()
    end
end)

local chams, rageBot = false, false
Add("👻 MM2 Chams (🔴/🔵 ФИКС)", function(v) chams = v end)
Add("🎮 Агрессивный ИИ Бот", function(v) rageBot = v end)

local function checkRole(p)
    if not p or p == LP then return "Innocent" end
    local bp, ch = p:FindFirstChild("Backpack"), p.Character
    if (bp and bp:FindFirstChild("Knife")) or (ch and ch:FindFirstChild("Knife")) then return "Murderer" end
    if (bp and bp:FindFirstChild("Gun")) or (ch and ch:FindFirstChild("Gun")) then return "Sheriff" end
    return "Innocent"
end

local function applyVampireAnims(c)
    local anims = {idle = "rbxassetid://10834458315", walk = "rbxassetid://10834710185", run = "rbxassetid://10834575850", jump = "rbxassetid://10834538890", fall = "rbxassetid://10834493390"}
    local am = c:WaitForChild("Animate", 5)
    if am then
        for name, id in pairs(anims) do
            local track = am:FindFirstChild(name)
            if track and track:FindFirstChildOfClass("Animation") then track:FindFirstChildOfClass("Animation").AnimationId = id end
        end
    end
end
if LP.Character then applyVampireAnims(LP.Character) end
LP.CharacterAdded:Connect(applyVampireAnims)

R.RenderStepped:Connect(function()
    for _, p in pairs(P:GetPlayers()) do
        if p ~= LP and p.Character then
            local hl = p.Character:FindFirstChild("PulseChams")
            if chams then
                if not hl then hl = Instance.new("Highlight", p.Character) hl.Name = "PulseChams" hl.FillTransparency = 0.3 hl.OutlineColor = Color3.fromRGB(255, 255, 255) end
                local role = checkRole(p)
                hl.FillColor = (role == "Murderer" and Color3.fromRGB(255, 0, 0)) or (role == "Sheriff" and Color3.fromRGB(0, 100, 255)) or Color3.fromRGB(170, 0, 255)
                hl.Enabled = true
            elseif hl then hl.Enabled = false end
        end
    end
end)

local function getTargetPlayer()
    local target, minDist = nil, math.huge
    for _, p in pairs(P:GetPlayers()) do
        if p ~= LP and p.Character and p.Character:FindFirstChild("HumanoidRootPart") and p.Character:FindFirstChild("Humanoid") and p.Character.Humanoid.Health > 0 then
            local dist = (LP.Character.HumanoidRootPart.Position - p.Character.HumanoidRootPart.Position).Magnitude
            if dist < minDist then minDist, target = dist, p.Character.HumanoidRootPart end
        end
    end
    return target
end

local function findCoins(root)
    local bestCoin, minDist = nil, math.huge
    for _, obj in pairs(workspace:GetDescendants()) do
        if obj.Name:lower():find("coin") or obj.Name == "CoinContainer" then
            local part = obj:IsA("Part") and obj or obj:FindFirstChildOfClass("Part")
            if part then local dist = (root.Position - part.Position).Magnitude if dist < minDist and dist < 120 then minDist, bestCoin = dist, part end end
        end
    end
    return bestCoin
end

task.spawn(function()
    while task.wait(0.01) do
        if rageBot and LP.Character and LP.Character:FindFirstChild("Humanoid") and LP.Character:FindFirstChild("HumanoidRootPart") then
            local hum, root = LP.Character.Humanoid, LP.Character.HumanoidRootPart
            local myRole, marder, knife = checkRole(LP), nil, nil
            for _, p in pairs(P:GetPlayers()) do
                if checkRole(p) == "Murderer" and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then marder = p.Character.HumanoidRootPart knife = p.Character:FindFirstChild("Knife") break end
            end
            local mainGui = LP.PlayerGui:FindFirstChild("MainGui")
            local gameTimer = mainGui and mainGui:FindFirstChild("Game") and mainGui.Game:FindFirstChild("Timer")
            if not (gameTimer and gameTimer.Visible) then
                local victim = getTargetPlayer()
                if victim then hum:MoveTo(victim.Position) if (root.Position - victim.Position).Magnitude < 5 then hum.Jump = true end
                else hum:Move(Vector3.new(math.sin(tick() * 5) * 15, 0, math.cos(tick() * 5) * 15)) end
                local voting = mainGui and mainGui:FindFirstChild("Voting")
                if voting and voting.Visible then local btn = voting:FindFirstChild("MapOne") or voting:FindFirstChild("MapTwo") if btn then game:GetService("VirtualUser"):ClickButton1(Vector2.new(btn.AbsolutePosition.X + 5, btn.AbsolutePosition.Y + 5)) end end
            else
                if myRole == "Murderer" then
                    local victim = getTargetPlayer()
                    if victim then hum:MoveTo(victim.Position) local k = LP.Character:FindFirstChild("Knife") or LP.Backpack:FindFirstChild("Knife") if k then k.Parent = LP.Character if (root.Position - victim.Position).Magnitude <= 15 then k:Activate() end end end
                elseif myRole == "Sheriff" then
                    if marder then hum:MoveTo(marder.Position) local g = LP.Character:FindFirstChild("Gun") or LP.Backpack:FindFirstChild("Gun") if g then g.Parent = LP.Character if (root.Position - marder.Position).Magnitude <= 40 then g:Activate() end end end
                else
                    local coin = findCoins(root)
                    if marder then
                        local dist = (root.Position - marder.Position).Magnitude
                        if dist < 45 or knife then hum:MoveTo(root.Position + (root.Position - marder.Position).Unit * 30 + Vector3.new(math.random(-15, 15), 0, math.random(-15, 15))) hum.Jump = true
                        elseif coin then hum:MoveTo(coin.Position) else local victim = getTargetPlayer() if victim then hum:MoveTo(victim.Position) end end
                    elseif coin then hum:MoveTo(coin.Position) else local victim = getTargetPlayer() if victim then hum:MoveTo(victim.Position) end end
                end
            end
        end
    end
end)

