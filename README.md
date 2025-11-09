-- GRAPS HUB - Brainrot Edition Premium (Atualizado)
-- Conforme solicitado: visual moderno, status de on/off, bordas, efeitos UI.

if game.CoreGui:FindFirstChild("GrapsHub") then
\tgame.CoreGui:FindFirstChild("GrapsHub"):Destroy()
end

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:FindFirstChildOfClass("Humanoid") or character:WaitForChild("Humanoid",5)
local hrp = character:FindFirstChild("HumanoidRootPart") or character:FindFirstChild("LowerTorso")

local function refreshCharacter()
\tcharacter = player.Character or player.CharacterAdded:Wait()
\thumanoid = character:FindFirstChildOfClass("Humanoid") or character:WaitForChild("Humanoid",5)
\thrp = character:FindFirstChild("HumanoidRootPart") or character:FindFirstChild("LowerTorso")
end

player.CharacterAdded:Connect(function()
\ttask.wait(0.5)
\trefreshCharacter()
end)

-- GUI
local gui = Instance.new("ScreenGui")
gui.Name = "GrapsHub"
gui.ResetOnSpawn = false
gui.Parent = game.CoreGui

local main = Instance.new("Frame", gui)
main.Size = UDim2.new(0,420,0,680)
main.Position = UDim2.new(0.5,-210,0.5,-340)
main.BackgroundColor3 = Color3.fromRGB(20,20,20)
main.Visible = false
main.Active = true
main.Draggable = true
main.BackgroundTransparency = 0.0

local UIStroke = Instance.new("UIStroke", main)
UIStroke.Color = Color3.fromRGB(0,170,255)
UIStroke.Thickness = 2

local UICorner = Instance.new("UICorner", main)
UICorner.CornerRadius = UDim.new(0,14)

-- Título
local title = Instance.new("TextLabel", main)
title.Text = "⚡ GRAPS HUB - Brainrot V2 Premium"
title.Size = UDim2.new(1,0,0,54)
title.Position = UDim2.new(0,0,0,0)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextColor3 = Color3.fromRGB(0,255,255)
title.TextSize = 20
title.TextScaled = true
title.TextXAlignment = Enum.TextXAlignment.Left
title.PaddingLeft = 12

-- Subtítulo com status
local status = Instance.new("TextLabel", main)
status.Text = "Sistema pronto."
status.Size = UDim2.new(1,0,0,26)
status.Position = UDim2.new(0,0,0,54)
status.BackgroundTransparency = 1
status.Font = Enum.Font.Gotham
status.TextColor3 = Color3.fromRGB(200,200,200)
status.TextSize = 14
status.TextScaled = true
status.TextXAlignment = Enum.TextXAlignment.Left
status.PaddingLeft = 12

-- UI de Ativação Global
local function createToggleLabel(parent, label, initialOn)
\tlocal frame = Instance.new("Frame", parent)
\tframe.Size = UDim2.new(1,0,0,54)
\tframe.BackgroundColor3 = Color3.fromRGB(25,25,25)
\tframe.BorderSizePixel = 0
\tlocal fCorner = Instance.new("UICorner", frame)
\tfCorner.CornerRadius = UDim.new(0,12)

\tlocal labelObj = Instance.new("TextLabel", frame)
\tlabelObj.Text = label
\tlabelObj.TextScaled = true
\tlabelObj.Font = Enum.Font.GothamBold
\tlabelObj.TextColor3 = Color3.fromRGB(230,230,230)
\tlabelObj.BackgroundTransparency = 1
\tlabelObj.Position = UDim2.new(0,12,0,0)
\tlabelObj.Size = UDim2.new(0.7,0,1,0)

\tlocal toggle = Instance.new("TextButton", frame)
\ttoggle.Size = UDim2.new(0, 78, 1, 0)
\ttoggle.Position = UDim2.new(0.78, -78, 0, 0)
\ttoggle.BackgroundColor3 = initialOn and Color3.fromRGB(0,255,0) or Color3.fromRGB(255,0,0)
\ttoggle.Text = initialOn and "ON" or "OFF"
\ttoggle.TextColor3 = Color3.fromRGB(0,0,0)
\ttoggle.Font = Enum.Font.GothamBold
\ttoggle.TextScaled = true
\ttoggle.BorderSizePixel = 0
\tlocal tBtn = Instance.new("UICorner", toggle)
\ttBtn.CornerRadius = UDim.new(0,8)

\t-- animate color change
\tlocal function setState(on)
\t\ttoggle.BackgroundColor3 = on and Color3.fromRGB(0,255,0) or Color3.fromRGB(255,0,0)
\t\ttoggle.Text = on and "ON" or "OFF"
\tend

\treturn frame, toggle, setState
end

-- Container de controles
local content = Instance.new("Frame", main)
content.Size = UDim2.new(1, -20, 1, -80)
content.Position = UDim2.new(0,10,0,78)
content.BackgroundTransparency = 1
local contentLayout = Instance.new("UIListLayout", content)
contentLayout.SortOrder = Enum.SortOrder.LayoutOrder
contentLayout.Padding = UDim.new(0,8)

-- Botões com estilo premium
local function criarBotao(text)
\tlocal btn = Instance.new("TextButton", content)
\tbtn.Size = UDim2.new(1,0,0,48)
\tbtn.BackgroundColor3 = Color3.fromRGB(28,28,28)
\tbtn.TextColor3 = Color3.fromRGB(255,255,255)
\tbtn.Font = Enum.Font.GothamBold
\tbtn.TextSize = 15
\tbtn.Text = text
\tbtn.AutoButtonColor = true
\tlocal c = Instance.new("UICorner", btn)
\tc.CornerRadius = UDim.new(0,12)
\tlocal s = Instance.new("UIStroke", btn)
\ts.Color = Color3.fromRGB(0,170,255)
\ts.Thickness = 1
\tbtn.BackgroundTransparency = 0.0
\treturn btn
end

-- Seção de status rápido (off/on com cor dinâmica)
local premiumPanel = Instance.new("Frame", main)
premiumPanel.Size = UDim2.new(1,0,0,60)
premiumPanel.Position = UDim2.new(0,0,0,0)
premiumPanel.BackgroundTransparency = 1
local premiumLayout = Instance.new("UIListLayout", premiumPanel)
premiumLayout.FillDirection = Enum.FillDirection.Horizontal
premiumLayout.Padding = UDim.new(0,8)

local function createTag(label, color)
\tlocal t = Instance.new("TextLabel", premiumPanel)
\tt.Text = label
\tt.Font = Enum.Font.GothamBold
\tt.TextColor3 = Color3.fromRGB(255,255,255)
\tt.BackgroundColor3 = color
\tt.TextScaled = true
\tt.Size = UDim2.new(0, 140, 1, 0)
\tlocal cc = Instance.new("UICorner", t)
\tcc.CornerRadius = UDim.new(0,8)
\treturn t
end

local tagOn = createTag("Premium Active", Color3.fromRGB(0, 180, 0))
local tagVersion = createTag("Brainrot V2", Color3.fromRGB(0, 120, 255))

-- Controles com estados on/off
local toggleFrames = {}

-- Salvar posição
local btnSalvar = criarBotao("💾 Salvar Posição")
btnSalvar.Parent = content

-- Teleportar
local btnTP = criarBotao("⚡ Teleportar")
btnTP.Parent = content

-- Teleguiado V2
local btnGuiado = criarBotao("🚀 Teleguiado V2")
btnGuiado.Parent = content

-- Parar teleguiado (desativar)
local btnCancelarGuiado = criarBotao("❌ Parar Teleguiado")
btnCancelarGuiado.Parent = content

-- ESP
local btnESP = criarBotao("👁️ ESP Player")
btnESP.Parent = content

-- Fly
local btnFly = criarBotao("🕊️ Fly")
btnFly.Parent = content

-- Noclip
local btnNoclip = criarBotao("👻 Noclip")
btnNoclip.Parent = content

-- Invis
local btnInvis = criarBotao("🕶️ Invisível")
btnInvis.Parent = content

-- Speed
local btnSpeed = criarBotao("⚡ Speed")
btnSpeed.Parent = content

-- Anti-Hit
local btnAntiHit = criarBotao("🛡️ Anti-Hit")
btnAntiHit.Parent = content

-- Hover Pad
local btnChao = criarBotao("🟦 Hover Pad")
btnChao.Parent = content

-- Atacar todos
local btnAttackAll = criarBotao("⚔️ Atacar Todos Jogadores")
btnAttackAll.Parent = content

-- Serviço PV
local btnPV = criarBotao("🔒 Servidor PV")
btnPV.Parent = content

-- Controle de estados globais
local posSalva, teleguiadoActive = nil, false
local espPlayers = {}
local flyActive, noclipActive, invisActive, speedActive, antiHitActive, chaoActive = false, false, false, false, false, false
local chaoPart, flySpeed = nil, 50
local chaoOffset = Vector3.new(0,-3,0)

-- Funcionalidades com estados visuais
local function setStatus(text)
\tstatus.Text = text
end

-- Salvar posição
btnSalvar.MouseButton1Click:Connect(function()
\tif hrp then
\t\tposSalva = hrp.Position
\t\tsetStatus("📍 Posição salva!")
\telse
\t\trefreshCharacter()
\t\tsetStatus("❌ HRP não encontrado!")
\tend
end)

-- Teleportar
btnTP.MouseButton1Click:Connect(function()
\tif posSalva and hrp then
\t\thrp.CFrame = CFrame.new(posSalva + Vector3.new(0,2,0))
\t\tsetStatus("⚡ Teleportado!")
\telse
\t\tsetStatus("❌ Nenhuma posição salva!")
\tend
end)

-- ESP
btnESP.MouseButton1Click:Connect(function()
\tlocal anyOn = false
\tfor _,v in pairs(espPlayers) do if v then anyOn = true break end end
\tif anyOn then
\t\tespPlayers = {}
\t\tfor _,plr in pairs(Players:GetPlayers()) do
\t\t\tif plr.Character and plr.Character:FindFirstChild("ESPGraps") then
\t\t\t\tplr.Character.ESPGraps:Destroy()
\t\t\tend
\t\tend
\t\tsetStatus("👁️ ESP OFF")
\telse
\t\tfor _,plr in pairs(Players:GetPlayers()) do
\t\t\tif plr ~= player then espPlayers[plr]=true end
\t\tend
\t\tfor plr,_ in pairs(espPlayers) do
\t\t\tlocal ch = plr.Character
\t\t\tif ch then
\t\t\t\tif not ch:FindFirstChild("ESPGraps") then
\t\t\t\t\tlocal hl = Instance.new("Highlight")
\t\t\t\t\thl.Name = "ESPGraps"
\t\t\t\t\thl.FillTransparency = 0.6
\t\t\t\t\thl.OutlineTransparency = 0
\t\t\t\t\thl.FillColor = Color3.fromRGB(0,255,255)
\t\t\t\t\thl.OutlineColor = Color3.fromRGB(255,255,255)
\t\t\t\t\thl.Parent = ch
\t\t\t\tend
\t\t\tend
\t\tend
\t\tsetStatus("👁️ ESP ON")
\tend
end)

-- Fly
btnFly.MouseButton1Click:Connect(function()
\tflyActive = not flyActive
\tsetStatus(flyActive and "🕊️ Fly ON" or "🕊️ Fly OFF")
end)

-- Noclip
btnNoclip.MouseButton1Click:Connect(function()
\tnoclipActive = not noclipActive
\tsetStatus(noclipActive and "👻 Noclip ON" or "👻 Noclip OFF")
end)

-- Invis
btnInvis.MouseButton1Click:Connect(function()
\tinvisActive = not invisActive
\tif character then
\t\tfor _,p in pairs(character:GetDescendants()) do
\t\t\tif p:IsA("BasePart") or p:IsA("MeshPart") then
\t\t\t\tp.Transparency = invisActive and 1 or 0
\t\t\telseif p:IsA("Decal") then
\t\t\t\tp.Transparency = invisActive and 1 or 0
\t\t\tend
\t\tend
\tend
\tsetStatus(invisActive and "🕶️ Invisível ON" or "🕶️ Invisível OFF")
end)

-- Speed
btnSpeed.MouseButton1Click:Connect(function()
\tspeedActive = not speedActive
\thumanoid.WalkSpeed = speedActive and 100 or 16
\tsetStatus(speedActive and "⚡ Speed ON" or "⚡ Speed OFF")
end)

-- Anti-Hit
btnAntiHit.MouseButton1Click:Connect(function()
\tantiHitActive = not antiHitActive
\tsetStatus(antiHitActive and "🛡️ Anti-Hit ON" or "🛡️ Anti-Hit OFF")
end)

-- Hover Pad
btnChao.MouseButton1Click:Connect(function()
\tchaoActive = not chaoActive
\tif chaoActive then
\t\tif not chaoPart then
\t\t\tchaoPart = Instance.new("Part",Workspace)
\t\t\tchaoPart.Size = Vector3.new(8,1,8)
\t\t\tchaoPart.Anchored = true
\t\t\tchaoPart.CanCollide = true
\t\t\tchaoPart.Material = Enum.Material.Neon
\t\t\tchaoPart.Color = Color3.fromRGB(0,255,255)
\t\t\tchaoPart.Name = "HoverPad"
\t\tend
\t\tsetStatus("🟦 Hover Pad ATIVO")
\telse
\t\tsetStatus("🟦 Hover Pad DESATIVADO")
\tend
end)

-- Teleguiado V2
btnGuiado.MouseButton1Click:Connect(function()
\tif not posSalva then setStatus("❌ Nenhuma posição salva!") return end
\tif teleguiadoActive then setStatus("⚠ Teleguiado já ativo!") return end
\tteleguiadoActive = true
\tsetStatus("🚀 Teleguiado V2 iniciado...")
\ttask.spawn(function()
\t\twhile teleguiadoActive and hrp do
\t\t\ttask.wait(0.03)
\t\t\tlocal targetPos = posSalva + Vector3.new(0,50,0)
\t\t\tlocal dir = (targetPos-hrp.Position).Unit
\t\t\thrp.CFrame = CFrame.lookAt(hrp.Position + dir*80*0.03,targetPos)
\t\t\thumanoid.PlatformStand=true
\t\t\tif (hrp.Position-targetPos).Magnitude<4 then
\t\t\t\tteleguiadoActive=false
\t\t\t\thumanoid.PlatformStand=false
\t\t\t\tsetStatus("✅ Chegou ao destino!")
\t\t\tend
\t\tend
\tend)
end)

btnCancelarGuiado.MouseButton1Click:Connect(function()
\tteleguiadoActive=false
\tif humanoid then humanoid.PlatformStand=false end
\tsetStatus("✖ Teleguiado cancelado")
end)

-- Atacar todos jogadores
btnAttackAll.MouseButton1Click:Connect(function()
\tfor _,plr in pairs(Players:GetPlayers()) do
\t\tif plr~=player and plr.Character and plr.Character:FindFirstChildOfClass("Humanoid") then
\t\t\tplr.Character.Humanoid.Health = 0
\t\tend
\tend
\tsetStatus("⚔️ Todos jogadores atacados!")
end)

-- Noclip & Fly Loop
RunService.Stepped:Connect(function()
\tif noclipActive and character then
\t\tfor _,p in pairs(character:GetDescendants()) do
\t\t\tif p:IsA("BasePart") then p.CanCollide=false end
\t\tend
\tend
\tif antiHitActive and character then
\t\tfor _,tool in pairs(Workspace:GetDescendants()) do
\t\t\tif tool:IsA("Tool") and tool.Parent~=character then tool.Parent=Workspace end
\t\tend
\tend
\t-- Hover pad
\tif chaoActive and hrp and chaoPart then
\t\tlocal pos = hrp.Position+chaoOffset
\t\tif humanoid:GetState()==Enum.HumanoidStateType.Jumping or humanoid:GetState()==Enum.HumanoidStateType.Freefall then
\t\t\tpos=pos+Vector3.new(0,0.5,0)
\t\tend
\t\tlocal vel=hrp.Velocity
\t\tpos=pos+Vector3.new(vel.X,0,vel.Z)*0.03
\t\tchaoPart.CFrame=CFrame.new(pos)
\tend
end)

-- Botão abrir/fechar menu B
local toggleBtn = Instance.new("TextButton",gui)
toggleBtn.Size=UDim2.new(0,55,0,55)
toggleBtn.Position=UDim2.new(0.93,0,0.18,0)
toggleBtn.BackgroundColor3=Color3.fromRGB(20,20,20)
toggleBtn.Text="B"
toggleBtn.Font=Enum.Font.GothamBold
toggleBtn.TextColor3=Color3.fromRGB(0,255,255)
toggleBtn.TextSize=30
toggleBtn.AutoButtonColor=true
local btnCorner=Instance.new("UICorner",toggleBtn); btnCorner.CornerRadius=UDim.new(0,12)
local btnStroke=Instance.new("UIStroke",toggleBtn); btnStroke.Color=Color3.fromRGB(0,255,255); btnStroke.Thickness=1.5

toggleBtn.MouseButton1Click:Connect(function()
\tmain.Visible=not main.Visible
\tstatus.Text=main.Visible and "📂 Menu Aberto" or "📁 Menu Fechado"
end)

-- Inicialização de status final
tagOn.Parent = premiumPanel
tagVersion.Parent = premiumPanel
setStatus="Pronto para Brainrot — GRAPS HUB V2 Premium ativo."
