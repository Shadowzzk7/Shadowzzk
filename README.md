-- SHADOW HUB V17 - PLAGUE DOCTOR EDITION (TEMA FOTO) - ATUALIZADO (FIXED ANTI-FLING)
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Lighting = game:GetService("Lighting")
local player = Players.LocalPlayer
local camera = workspace.CurrentCamera

-- ================= CONFIGURAÇÃO DE ACESSO PERMANENTE =================
local NOME_ARQUIVO = "shadow_token.txt"
local SENHA_CORRETA = "Atomic"

local function salvarAcesso()
    if writefile then
        writefile(NOME_ARQUIVO, "autenticado")
    end
end

local function jaAutenticado()
    if isfile and isfile(NOME_ARQUIVO) then
        return true
    end
    return false
end

-- ================= DESIGN E CORES (TEMA DA FOTO) =================
local Cores = {
    Fundo = Color3.fromRGB(10, 10, 10),      -- Preto Profundo
    Sidebar = Color3.fromRGB(5, 5, 5),       -- Preto Quase Total
    Detalhe = Color3.fromRGB(255, 255, 255), -- Branco (Cor da Máscara/Asa)
    Texto = Color3.fromRGB(240, 240, 240),   -- Branco acinzentado para leitura
    Botao = Color3.fromRGB(25, 25, 25),      -- Cinza muito escuro
    ToggleOff = Color3.fromRGB(35, 35, 35),  -- Desativado
    ToggleOn = Color3.fromRGB(255, 255, 255),-- Ativado (Branco Brilhante)
    AcaoOn = Color3.fromRGB(255, 255, 255),  -- Seleção
    AcaoOff = Color3.fromRGB(20, 20, 20),    -- Não selecionado
    SliderFundo = Color3.fromRGB(20, 20, 20),
    Discord = Color3.fromRGB(40, 40, 40)     -- Discord discreto
}

local discordLink = "https://discord.gg/shadowhub"

local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "ShadowHub_PlagueTheme"
gui.ResetOnSpawn = false
gui.DisplayOrder = 999999

-- ================= FUNÇÃO AUXILIAR DISCORD =================
local function createDiscordButton(parent, pos)
    local btn = Instance.new("TextButton", parent)
    btn.Size = UDim2.new(0.8, 0, 0, 35); btn.Position = pos; btn.BackgroundColor3 = Cores.Discord; btn.Text = "      Discord"; btn.TextColor3 = Color3.new(1,1,1); btn.Font = Enum.Font.GothamBold; btn.TextSize = 14; Instance.new("UICorner", btn)
    local icon = Instance.new("ImageLabel", btn); icon.Size = UDim2.new(0, 25, 0, 25); icon.Position = UDim2.new(0, 10, 0.5, -12); icon.BackgroundTransparency = 1; icon.Image = "rbxassetid://11743781226"
    btn.MouseButton1Click:Connect(function() setclipboard(discordLink); local old = btn.Text; btn.Text = "COPIADO!"; task.wait(2); btn.Text = old end)
    return btn
end

-- ================= BOTÃO S PRINCIPAL =================
local btnS = Instance.new("TextButton", gui)
btnS.Size = UDim2.new(0, 70, 0, 70); btnS.Position = UDim2.new(0, 250, 0, 50); btnS.BackgroundColor3 = Cores.Fundo; btnS.Text = "S"; btnS.TextColor3 = Cores.Detalhe; btnS.Font = Enum.Font.GothamBlack; btnS.TextSize = 40; btnS.Draggable = true
Instance.new("UICorner", btnS).CornerRadius = UDim.new(1,0); 
local strokeS = Instance.new("UIStroke", btnS); strokeS.Thickness = 3; strokeS.Color = Cores.Detalhe

-- ================= PAINEL PRINCIPAL =================
local mainFrame = Instance.new("Frame", gui)
mainFrame.Size = UDim2.new(0, 480, 0, 400); mainFrame.BackgroundColor3 = Cores.Fundo; mainFrame.Visible = false; mainFrame.ClipsDescendants = true; Instance.new("UICorner", mainFrame)
local mainStroke = Instance.new("UIStroke", mainFrame); mainStroke.Color = Cores.Detalhe; mainStroke.Thickness = 1

-- ================= TELA DE SENHA =================
local loginFrame = Instance.new("Frame", gui)
loginFrame.Size = UDim2.new(0, 300, 0, 230); loginFrame.Position = UDim2.new(0.5, -150, 0.5, -115); loginFrame.BackgroundColor3 = Cores.Fundo; loginFrame.Visible = false; Instance.new("UICorner", loginFrame); Instance.new("UIStroke", loginFrame).Color = Cores.Detalhe

local passInput = Instance.new("TextBox", loginFrame)
passInput.Size = UDim2.new(0.8, 0, 0, 40); passInput.Position = UDim2.new(0.1, 0, 0.15, 0); passInput.BackgroundColor3 = Cores.Botao; passInput.PlaceholderText = "Digite a Senha..."; passInput.Text = ""; passInput.TextColor3 = Color3.new(1,1,1); passInput.Font = Enum.Font.Gotham; Instance.new("UICorner", passInput)

local loginBtn = Instance.new("TextButton", loginFrame)
loginBtn.Size = UDim2.new(0.8, 0, 0, 40); loginBtn.Position = UDim2.new(0.1, 0, 0.4, 0); loginBtn.BackgroundColor3 = Cores.Detalhe; loginBtn.Text = "DESBLOQUEAR"; loginBtn.TextColor3 = Color3.new(0,0,0); loginBtn.Font = Enum.Font.GothamBold; Instance.new("UICorner", loginBtn)

createDiscordButton(loginFrame, UDim2.new(0.1, 0, 0.7, 0))

-- ================= CONTROLE DE ACESSO (MODIFICADO) =================
local logado = jaAutenticado()

if logado then loginFrame:Destroy() end

btnS.MouseButton1Click:Connect(function()
    if not logado then 
        loginFrame.Visible = not loginFrame.Visible 
    else
        mainFrame.Visible = not mainFrame.Visible
        mainFrame.Position = UDim2.new(0, btnS.AbsolutePosition.X + 80, 0, btnS.AbsolutePosition.Y)
    end
end)

loginBtn.MouseButton1Click:Connect(function()
    if passInput.Text == SENHA_CORRETA then 
        logado = true; salvarAcesso(); loginFrame:Destroy(); mainFrame.Visible = true; 
        mainFrame.Position = UDim2.new(0, btnS.AbsolutePosition.X + 80, 0, btnS.AbsolutePosition.Y)
    else 
        passInput.Text = ""; passInput.PlaceholderText = "SENHA INCORRETA!"; task.wait(1); passInput.PlaceholderText = "Digite a Senha..." 
    end
end)

-- ================= DESIGN DA SIDEBAR =================
local sidebar = Instance.new("Frame", mainFrame); sidebar.Size = UDim2.new(0, 130, 1, 0); sidebar.BackgroundColor3 = Cores.Sidebar; Instance.new("UICorner", sidebar)
local logo = Instance.new("TextLabel", sidebar); logo.Size = UDim2.new(1, 0, 0, 60); logo.Text = "SHADOW HUB"; logo.TextColor3 = Cores.Detalhe; logo.Font = Enum.Font.GothamBold; logo.TextSize = 18; logo.BackgroundTransparency = 1

local antiLagOn = false
local alBtn = Instance.new("TextButton", sidebar)
alBtn.Size = UDim2.new(0.85, 0, 0, 22); alBtn.Position = UDim2.new(0.07, 0, 1, -85); alBtn.BackgroundColor3 = Cores.ToggleOff; alBtn.Text = "Anti-Lag: OFF"; alBtn.TextColor3 = Color3.new(1,1,1); alBtn.Font = Enum.Font.GothamBold; alBtn.TextSize = 10; Instance.new("UICorner", alBtn)

local function clean(obj)
    if not antiLagOn then return end
    if obj:IsA("Part") or obj:IsA("MeshPart") then 
        obj.Material = Enum.Material.SmoothPlastic
        obj.Reflectance = 0
    elseif obj:IsA("Decal") or obj:IsA("Texture") then 
        obj:Destroy() 
    elseif obj:IsA("ParticleEmitter") or obj:IsA("Trail") then
        obj.Enabled = false
    end
end

game.DescendantAdded:Connect(function(descendant) if antiLagOn then clean(descendant) end end)

alBtn.MouseButton1Click:Connect(function()
    antiLagOn = not antiLagOn
    alBtn.Text = antiLagOn and "Anti-Lag: ON" or "Anti-Lag: OFF"
    alBtn.BackgroundColor3 = antiLagOn and Cores.ToggleOn or Cores.ToggleOff
    alBtn.TextColor3 = antiLagOn and Color3.new(0,0,0) or Color3.new(1,1,1)
    if antiLagOn then for _, v in pairs(game:GetDescendants()) do clean(v) end end
end)

local profileFrame = Instance.new("Frame", sidebar); profileFrame.Size = UDim2.new(1, 0, 0, 50); profileFrame.Position = UDim2.new(0, 0, 1, -55); profileFrame.BackgroundTransparency = 1
local pImg = Instance.new("ImageLabel", profileFrame); pImg.Size = UDim2.new(0, 35, 0, 35); pImg.Position = UDim2.new(0, 10, 0.5, -17); pImg.Image = "rbxthumb://type=AvatarHeadShot&id="..player.UserId.."&w=150&h=150"; Instance.new("UICorner", pImg).CornerRadius = UDim.new(1,0)
local pName = Instance.new("TextLabel", profileFrame); pName.Size = UDim2.new(1, -55, 1, 0); pName.Position = UDim2.new(0, 50, 0, 0); pName.Text = player.DisplayName; pName.TextColor3 = Cores.Texto; pName.Font = Enum.Font.GothamBold; pName.TextSize = 10; pName.BackgroundTransparency = 1; pName.TextXAlignment = Enum.TextXAlignment.Left

local tabContainer = Instance.new("Frame", sidebar); tabContainer.Size = UDim2.new(1, 0, 1, -210); tabContainer.Position = UDim2.new(0, 0, 0, 70); tabContainer.BackgroundTransparency = 1
local function createTabBtn(name, pos)
    local btn = Instance.new("TextButton", tabContainer); btn.Size = UDim2.new(0.9, 0, 0, 32); btn.Position = UDim2.new(0.05, 0, 0, pos); btn.BackgroundColor3 = Cores.Botao; btn.Text = name; btn.TextColor3 = Color3.new(1, 1, 1); btn.Font = Enum.Font.GothamBold; btn.TextSize = 13; Instance.new("UICorner", btn); return btn
end

local btnMain = createTabBtn("🏠 Main", 0); local btnMM2 = createTabBtn("🔪 MM2", 37); local btnSelect = createTabBtn("📍 Selecionar", 74); local btnConfig = createTabBtn("⚙️ CONFIG", 111)

local function createPage()
    local p = Instance.new("ScrollingFrame", mainFrame); p.Position = UDim2.new(0, 140, 0, 10); p.Size = UDim2.new(1, -150, 1, -20); p.BackgroundTransparency = 1; p.ScrollBarThickness = 2; p.AutomaticCanvasSize = Enum.AutomaticSize.Y; p.Visible = false
    local l = Instance.new("UIListLayout", p); l.Padding = UDim.new(0, 8); l.SortOrder = Enum.SortOrder.LayoutOrder; return p
end

local mainPage = createPage(); local mm2Page = createPage(); local selectPage = createPage(); local configPage = createPage()
mainPage.Visible = true; btnMain.BackgroundColor3 = Cores.Detalhe; btnMain.TextColor3 = Color3.new(0,0,0)

local function switchTab(tab, btn)
    mainPage.Visible = false; mm2Page.Visible = false; selectPage.Visible = false; configPage.Visible = false
    btnMain.BackgroundColor3 = Cores.Botao; btnMM2.BackgroundColor3 = Cores.Botao; btnSelect.BackgroundColor3 = Cores.Botao; btnConfig.BackgroundColor3 = Cores.Botao
    btnMain.TextColor3 = Color3.new(1,1,1); btnMM2.TextColor3 = Color3.new(1,1,1); btnSelect.TextColor3 = Color3.new(1,1,1); btnConfig.TextColor3 = Color3.new(1,1,1)
    tab.Visible = true; btn.BackgroundColor3 = Cores.Detalhe; btn.TextColor3 = Color3.new(0,0,0)
end
btnMain.MouseButton1Click:Connect(function() switchTab(mainPage, btnMain) end)
btnMM2.MouseButton1Click:Connect(function() switchTab(mm2Page, btnMM2) end)
btnSelect.MouseButton1Click:Connect(function() switchTab(selectPage, btnSelect) end)
btnConfig.MouseButton1Click:Connect(function() switchTab(configPage, btnConfig) end)

-- ================= UTILITÁRIOS =================
local function createToggle(parent, text, order, callback)
    local state = false
    local frame = Instance.new("Frame", parent); frame.Size = UDim2.new(1, 0, 0, 35); frame.BackgroundTransparency = 1; frame.LayoutOrder = order
    local label = Instance.new("TextLabel", frame); label.Size = UDim2.new(0.7, 0, 1, 0); label.Text = text; label.TextColor3 = Cores.Texto; label.Font = Enum.Font.GothamBold; label.TextSize = 11; label.BackgroundTransparency = 1; label.TextXAlignment = Enum.TextXAlignment.Left
    local switch = Instance.new("TextButton", frame); switch.Size = UDim2.new(0, 35, 0, 18); switch.Position = UDim2.new(1, -40, 0.5, -9); switch.BackgroundColor3 = Cores.ToggleOff; switch.Text = ""; Instance.new("UICorner", switch).CornerRadius = UDim.new(1, 0)
    local circle = Instance.new("Frame", switch); circle.Size = UDim2.new(0, 12, 0, 12); circle.Position = UDim2.new(0, 3, 0.5, -6); circle.BackgroundColor3 = state and Color3.new(0,0,0) or Color3.new(1, 1, 1); Instance.new("UICorner", circle).CornerRadius = UDim.new(1, 0)
    switch.MouseButton1Click:Connect(function() 
        state = not state
        TweenService:Create(circle, TweenInfo.new(0.2), {Position = state and UDim2.new(0, 20, 0.5, -6) or UDim2.new(0, 3, 0.5, -6), BackgroundColor3 = state and Color3.new(0,0,0) or Color3.new(1,1,1)}):Play()
        TweenService:Create(switch, TweenInfo.new(0.2), {BackgroundColor3 = state and Cores.ToggleOn or Cores.ToggleOff}):Play()
        callback(state) 
    end)
end

local function createSlider(parent, text, min, max, default, order, callback)
    local frame = Instance.new("Frame", parent); frame.Size = UDim2.new(1, 0, 0, 45); frame.BackgroundTransparency = 1; frame.LayoutOrder = order
    local label = Instance.new("TextLabel", frame); label.Size = UDim2.new(1, 0, 0, 20); label.Text = text .. ": " .. default; label.TextColor3 = Cores.Texto; label.Font = Enum.Font.GothamBold; label.TextSize = 11; label.BackgroundTransparency = 1; label.TextXAlignment = Enum.TextXAlignment.Left
    local sliderBar = Instance.new("TextButton", frame); sliderBar.Size = UDim2.new(1, -10, 0, 6); sliderBar.Position = UDim2.new(0, 0, 0, 30); sliderBar.BackgroundColor3 = Cores.SliderFundo; sliderBar.Text = ""; Instance.new("UICorner", sliderBar)
    local fill = Instance.new("Frame", sliderBar); fill.Size = UDim2.new((default - min) / (max - min), 0, 1, 0); fill.BackgroundColor3 = Cores.Detalhe; Instance.new("UICorner", fill)
    local knob = Instance.new("Frame", sliderBar); knob.Size = UDim2.new(0, 12, 0, 12); knob.Position = UDim2.new((default - min) / (max - min), -6, 0.5, -6); knob.BackgroundColor3 = Cores.Detalhe; Instance.new("UICorner", knob)
    local dragging = false
    local function update(input)
        local pos = math.clamp((input.Position.X - sliderBar.AbsolutePosition.X) / sliderBar.AbsoluteSize.X, 0, 1)
        local val = math.floor(min + (max - min) * pos)
        fill.Size = UDim2.new(pos, 0, 1, 0); knob.Position = UDim2.new(pos, -6, 0.5, -6); label.Text = text .. ": " .. val; callback(val)
    end
    knob.InputBegan:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = true end end)
    UserInputService.InputEnded:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = false end end)
    UserInputService.InputChanged:Connect(function(input) if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then update(input) end end)
end

-- ================= SISTEMAS E VARIÁVEIS =================
local running = true
local flying, noclipOn, godEnabled, antiFling = false, false, false, false
local espOn, killAura = false, false
local killAuraRange = 50 
local bringOn, freezeOn, selectedLB, selectedFZ = false, false, {}, {}
local walkSpeedVal, jumpPowerVal = 16, 50
local bv, bg, flyConn

-- SISTEMA DE MOVIMENTO (FIXED ANTI-FLING LOGIC)
RunService.Heartbeat:Connect(function()
    if not running then return end
    local char = player.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    
    if hum then
        hum.WalkSpeed = walkSpeedVal
        hum.JumpPower = jumpPowerVal
        hum.UseJumpPower = true
    end
    
    if antiFling and hrp then
        -- Estabiliza velocidade exagerada do seu HRP
        if hrp.Velocity.Magnitude > 100 or hrp.RotVelocity.Magnitude > 100 then
            hrp.Velocity = Vector3.new(0, 0, 0)
            hrp.RotVelocity = Vector3.new(0, 0, 0)
        end
        -- Desativa colisão apenas com OUTROS jogadores (Evita bugar itens equipados)
        for _, other in pairs(Players:GetPlayers()) do
            if other ~= player and other.Character then
                for _, part in pairs(other.Character:GetDescendants()) do
                    if part:IsA("BasePart") then 
                        part.CanCollide = false 
                    end
                end
            end
        end
    end
end)

-- ABA MAIN
createToggle(mainPage, "Vôo (Fly Original)", 1, function(s) 
    flying = s 
    if s then 
        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            player.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Physics)
            bv = Instance.new("BodyVelocity", hrp); bv.MaxForce = Vector3.new(1e9, 1e9, 1e9)
            bg = Instance.new("BodyGyro", hrp); bg.MaxTorque = Vector3.new(1e9, 1e9, 1e9)
            flyConn = RunService.RenderStepped:Connect(function()
                if not flying or not bv then return end
                local moveDir = player.Character.Humanoid.MoveDirection
                bv.Velocity = moveDir.Magnitude > 0 and (camera.CFrame.LookVector * moveDir:Dot(camera.CFrame.LookVector) + camera.CFrame.RightVector * moveDir:Dot(camera.CFrame.RightVector)).Unit * 70 or Vector3.zero
                bg.CFrame = camera.CFrame
            end)
        end
    else 
        if flyConn then flyConn:Disconnect() end; if bv then bv:Destroy() end; if bg then bg:Destroy() end; 
        if player.Character and player.Character:FindFirstChild("Humanoid") then player.Character.Humanoid:ChangeState(8) end 
    end 
end)
createToggle(mainPage, "Noclip", 2, function(s) noclipOn = s end)
local function runGod() task.spawn(function() while godEnabled and running do local hum = player.Character and player.Character:FindFirstChildOfClass("Humanoid") if hum then if hum.Health < 100 and hum.Health > 0 then hum.Health = 100 end hum:SetStateEnabled(Enum.HumanoidStateType.Dead, false) end task.wait(0.1) end end) end
createToggle(mainPage, "God Mode", 3, function(s) godEnabled = s if s then runGod() end end)
createToggle(mainPage, "Anti-Fling ULTIMATE (Fixed)", 4, function(s) antiFling = s end)

-- ABA MM2
createSlider(mm2Page, "Kill Aura Range", 5, 500, 50, 1, function(v) killAuraRange = v end)
createToggle(mm2Page, "Kill Aura Ativado", 2, function(s) killAura = s end)
createToggle(mm2Page, "Visão ESP MM2 (TEMA FOTO)", 3, function(s) espOn = s end)

-- ABA SELECIONAR
createToggle(selectPage, "ATIVAR LOOP BRING", 1, function(s) bringOn = s end)
createToggle(selectPage, "ATIVAR FREEZE LOCAL", 2, function(s) freezeOn = s end)
local listContainer = Instance.new("Frame", selectPage); listContainer.Size = UDim2.new(1, 0, 0, 0); listContainer.AutomaticSize = Enum.AutomaticSize.Y; listContainer.BackgroundTransparency = 1; Instance.new("UIListLayout", listContainer).Padding = UDim.new(0, 5)

local function refreshList()
    for _, c in pairs(listContainer:GetChildren()) do if c:IsA("Frame") then c:Destroy() end end
    for _, p in pairs(Players:GetPlayers()) do
        local row = Instance.new("Frame", listContainer); row.Size = UDim2.new(1, 0, 0, 35); row.BackgroundColor3 = Color3.fromRGB(15, 15, 15); Instance.new("UICorner", row)
        local name = Instance.new("TextLabel", row); name.Size = UDim2.new(0.4, 0, 1, 0); name.Position = UDim2.new(0, 10, 0, 0); name.Text = p.DisplayName; name.TextColor3 = Color3.new(1,1,1); name.Font = Enum.Font.GothamBold; name.TextSize = 10; name.BackgroundTransparency = 1; name.TextXAlignment = Enum.TextXAlignment.Left
        local btns = Instance.new("Frame", row); btns.Size = UDim2.new(0.6, 0, 1, 0); btns.Position = UDim2.new(0.4, 0, 0, 0); btns.BackgroundTransparency = 1; Instance.new("UIListLayout", btns).FillDirection = Enum.FillDirection.Horizontal; btns.UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Right; btns.UIListLayout.VerticalAlignment = Enum.VerticalAlignment.Center; btns.UIListLayout.Padding = UDim.new(0, 5)
        local tp = Instance.new("TextButton", btns); tp.Size = UDim2.new(0, 32, 0, 22); tp.Text = "TP"; tp.BackgroundColor3 = Cores.Botao; tp.TextColor3 = Color3.new(1,1,1); Instance.new("UICorner", tp)
        tp.MouseButton1Click:Connect(function() if p.Character and p.Character:FindFirstChild("HumanoidRootPart") then player.Character:MoveTo(p.Character.HumanoidRootPart.Position) end end)
        local fz = Instance.new("TextButton", btns); fz.Size = UDim2.new(0, 32, 0, 22); fz.Text = "FZ"; fz.BackgroundColor3 = selectedFZ[p] and Cores.AcaoOn or Cores.AcaoOff; fz.TextColor3 = selectedFZ[p] and Color3.new(0,0,0) or Color3.new(1,1,1); Instance.new("UICorner", fz)
        fz.MouseButton1Click:Connect(function() selectedFZ[p] = not selectedFZ[p] refreshList() end)
        if p ~= player then local lb = Instance.new("TextButton", btns); lb.Size = UDim2.new(0, 32, 0, 22); lb.Text = "LB"; lb.BackgroundColor3 = selectedLB[p] and Cores.AcaoOn or Cores.AcaoOff; lb.TextColor3 = selectedLB[p] and Color3.new(0,0,0) or Color3.new(1,1,1); Instance.new("UICorner", lb); lb.MouseButton1Click:Connect(function() selectedLB[p] = not selectedLB[p] refreshList() end) end
    end
end
refreshList(); Players.PlayerAdded:Connect(refreshList); Players.PlayerRemoving:Connect(refreshList)

-- ABA CONFIG
createSlider(configPage, "Velocidade", 16, 250, 16, 1, function(v) walkSpeedVal = v end)
createSlider(configPage, "Pulo", 50, 300, 50, 2, function(v) jumpPowerVal = v end)
local discBtn = createDiscordButton(configPage, UDim2.new(0, 0, 0, 0)); discBtn.Size = UDim2.new(1, 0, 0, 35); discBtn.LayoutOrder = 3
local delBtn = Instance.new("TextButton", configPage); delBtn.Size = UDim2.new(1, 0, 0, 35); delBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 0); delBtn.Text = "FECHAR SCRIPT"; delBtn.TextColor3 = Color3.new(1,1,1); delBtn.Font = Enum.Font.GothamBold; delBtn.LayoutOrder = 10; Instance.new("UICorner", delBtn)
delBtn.MouseButton1Click:Connect(function() running = false; killAura = false; espOn = false; flying = false; antiFling = false; if flyConn then flyConn:Disconnect() end; if bv then bv:Destroy() end; if bg then bg:Destroy() end; if player.Character and player.Character:FindFirstChild("Humanoid") then player.Character.Humanoid.WalkSpeed = 16; player.Character.Humanoid.JumpPower = 50; player.Character.Humanoid:ChangeState(8) end; gui:Destroy() end)

-- ================= LOOP DO ESP E SISTEMAS =================
RunService.Stepped:Connect(function() if noclipOn and player.Character then for _, v in pairs(player.Character:GetDescendants()) do if v:IsA("BasePart") then v.CanCollide = false end end end end)
RunService.Heartbeat:Connect(function()
    if bringOn and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then for p, val in pairs(selectedLB) do if val and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then p.Character.HumanoidRootPart.CFrame = player.Character.HumanoidRootPart.CFrame * CFrame.new(0,0,-3) end end end
    for p, val in pairs(selectedFZ) do if p.Character and p.Character:FindFirstChild("HumanoidRootPart") then p.Character.HumanoidRootPart.Anchored = (freezeOn and val) end end
end)

task.spawn(function()
    while true do
        if not running then break end
        if killAura and player.Character and player.Character:FindFirstChild("Knife") then
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= player and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                    if (player.Character.HumanoidRootPart.Position - p.Character.HumanoidRootPart.Position).Magnitude < killAuraRange then
                        firetouchinterest(p.Character.HumanoidRootPart, player.Character.Knife.Handle, 0); firetouchinterest(p.Character.HumanoidRootPart, player.Character.Knife.Handle, 1)
                    end
                end
            end
        end
        if espOn then
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= player and p.Character and p.Character:FindFirstChild("HumanoidRootPart") and p.Character:FindFirstChild("Humanoid") then
                    pcall(function()
                        local char = p.Character
                        local hrp = char.HumanoidRootPart
                        local hum = char.Humanoid
                        local hl = char:FindFirstChild("SH_HL") or Instance.new("Highlight", char)
                        hl.Name = "SH_HL"
                        
                        local isSheriff = p.Backpack:FindFirstChild("Gun") or char:FindFirstChild("Gun")
                        local isMurder = p.Backpack:FindFirstChild("Knife") or char:FindFirstChild("Knife")
                        
                        local isScripting = false
                        local ray = Ray.new(hrp.Position, Vector3.new(0, -15, 0))
                        local hit = workspace:FindPartOnRay(ray, char)
                        
                        if hrp.Velocity.Magnitude > 100 or (not hit and hrp.Velocity.Y > 2 and hum:GetState() ~= Enum.HumanoidStateType.Jumping) then
                            isScripting = true
                        end

                        local espColor = Color3.fromRGB(150, 150, 150)
                        local statusText = ""

                        if isScripting then
                            espColor = Color3.fromRGB(0, 255, 0)
                            statusText = " [HACKER]"
                        elseif isMurder then
                            espColor = Color3.fromRGB(255, 0, 0)
                            statusText = " [ASSASSINO]"
                        elseif isSheriff then
                            espColor = Color3.fromRGB(255, 255, 255)
                            statusText = " [XERIFE]"
                        end
                        
                        hl.FillColor = espColor; hl.OutlineColor = Color3.new(1,1,1); hl.Enabled = true
                        
                        local bill = char:FindFirstChild("SH_Bill") or Instance.new("BillboardGui", char); bill.Name = "SH_Bill"; bill.AlwaysOnTop = true; bill.Size = UDim2.new(0, 120, 0, 50); bill.ExtentsOffset = Vector3.new(0, 2.5, 0)
                        local txt = bill:FindFirstChild("Info") or Instance.new("TextLabel", bill); txt.Name = "Info"; txt.BackgroundTransparency = 1; txt.Size = UDim2.new(1, 0, 1, 0); txt.Font = Enum.Font.GothamBold; txt.TextSize = 10; txt.TextColor3 = espColor; txt.TextStrokeTransparency = 0
                        local dist = math.floor((player.Character.HumanoidRootPart.Position - hrp.Position).Magnitude)
                        txt.Text = string.format("%s%s\n%d m", p.DisplayName, statusText, dist)
                    end)
                end
            end
        else
            for _, p in pairs(Players:GetPlayers()) do pcall(function() if p.Character and p.Character:FindFirstChild("SH_HL") then p.Character.SH_HL:Destroy() end if p.Character and p.Character:FindFirstChild("SH_Bill") then p.Character.SH_Bill:Destroy() end end) end
        end
        task.wait(0.1)
    end
end)

player.CharacterAdded:Connect(function() task.wait(1.5) if godEnabled and running then runGod() end end)
