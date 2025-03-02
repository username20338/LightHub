# LightHub                                                                                                                                                                                                                                                    local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/shlexware/Orion/main/source"))()
local Window = OrionLib:MakeWindow({Name = "BrazilianHub", HidePremium = false, SaveConfig = true, ConfigFolder = "BrazilianHubConfig"})

-- Criando uma aba principal
local MainTab = Window:MakeTab({Name = "Principal", Icon = "rbxassetid://4483345998", PremiumOnly = false})

-- Função para teleportar automaticamente para missões
local function TeleportToQuest()
    local player = game.Players.LocalPlayer
    local humanoidRootPart = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    local questNPCs = game:GetService("Workspace"):FindFirstChild("QuestNPCs")
    
    if questNPCs and humanoidRootPart then
        for _, npc in pairs(questNPCs:GetChildren()) do
            if npc:FindFirstChild("HumanoidRootPart") then
                humanoidRootPart.CFrame = npc.HumanoidRootPart.CFrame * CFrame.new(0, 3, 0)
                break -- Teleporta para o primeiro NPC de missão encontrado
            end
        end
    end
end

-- Função para coletar itens automaticamente
local function AutoCollectItems()
    local player = game.Players.LocalPlayer
    local humanoidRootPart = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    local items = game:GetService("Workspace"):GetChildren()
    
    if humanoidRootPart then
        for _, item in pairs(items) do
            if item:IsA("Tool") or item.Name:lower():find("chest") or item.Name:lower():find("fruit") then
                if item:FindFirstChild("Handle") then
                    humanoidRootPart.CFrame = item.Handle.CFrame * CFrame.new(0, 3, 0)
                    wait(0.5) -- Pequeno delay para evitar falhas na coleta
                end
            end
        end
    end
end

-- Função para ativar o Haki automaticamente
local function AutoHaki()
    local player = game.Players.LocalPlayer
    local character = player.Character
    if character and not character:FindFirstChild("HakiActive") then
        game:GetService("ReplicatedStorage").Remotes:FindFirstChild("ActivateHaki"):FireServer()
    end
end

-- Função para distribuir pontos automaticamente
local function AutoStats()
    local player = game.Players.LocalPlayer
    local stats = {"Melee", "Defense", "Sword", "Gun", "Fruit"}
    for _, stat in pairs(stats) do
        game:GetService("ReplicatedStorage").Remotes:FindFirstChild("AddStat"):FireServer(stat, 1)
        wait(0.2) -- Pequeno delay para evitar excesso de requisições
    end
end

-- Safe Mode para evitar bans
local function SafeMode()
    local player = game.Players.LocalPlayer
    local character = player.Character
    if character then
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                part.CanCollide = false -- Evita detecção de colisão suspeita
            end
        end
        game:GetService("RunService").Stepped:Connect(function()
            player.ReplicationFocus = workspace -- Minimiza anomalias de movimentação
        end)
    end
end

-- Função para ativar ESP de jogadores e frutas
local function ESP()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer then
            local highlight = Instance.new("Highlight", player.Character or player.CharacterAdded:Wait())
            highlight.FillColor = Color3.fromRGB(255, 0, 0) -- Vermelho para jogadores
        end
    end
end

-- Função para Auto Raid
local function AutoRaid()
    while _G.AutoRaid do
        local raidNPC = game:GetService("Workspace"):FindFirstChild("RaidNPC")
        if raidNPC and raidNPC:FindFirstChild("HumanoidRootPart") then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = raidNPC.HumanoidRootPart.CFrame
            wait(1)
    …
