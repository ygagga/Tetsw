-- Carrega as bibliotecas Fluent, SaveManager e InterfaceManager
local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

-- Criando a Janela da Interface
local Window = Fluent:CreateWindow({
    Title = "Brookhaven RP 🏡 (Troll Hub 🤡)",
    SubTitle = "🔥 Zoando geral! 💀",
    TabWidth = 160,
    Size = UDim2.fromOffset(500, 320),
    Acrylic = true,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl
})

-- Criando Abas
local Tabs = {
    Troll = Window:AddTab({ Title = "🤡 Troll", Icon = "alert" }),
    About = Window:AddTab({ Title = "ℹ️ Sobre", Icon = "info" })
}


-- Adiciona uma seção para controle de jogadores na aba Troll
Tabs.Troll:AddSection("Controle de Jogadores")

local selectedPlayer = ""

-- Campo de entrada para o nome do jogador
Tabs.Troll:AddInput("PlayerName", {
    Title = "Nome do Jogador",
    Default = "",
    Placeholder = "Digite o nome do jogador",
    Callback = function(value)
        selectedPlayer = value
    end
})

-- Função para matar o jogador usando o método do sofá
local function killPlayer(targetUsername)
    local players = game:GetService("Players")
    local localPlayer = players.LocalPlayer
    local targetPlayer = players:FindFirstChild(targetUsername)

    if targetPlayer and targetPlayer.Character and localPlayer.Character then
        local humanoidRootPart = localPlayer.Character:FindFirstChild("HumanoidRootPart")
        local targetHumanoidRootPart = targetPlayer.Character:FindFirstChild("HumanoidRootPart")

        if humanoidRootPart and targetHumanoidRootPart then
            local originalPosition = humanoidRootPart.Position

            -- Teleporta para baixo do jogador
            humanoidRootPart.CFrame = targetHumanoidRootPart.CFrame * CFrame.new(0, -3, 0)

            wait(0.5)

            -- Spawna um sofá e pega o jogador
            local args = {
                [1] = "VehicleSpawn",
                [2] = "Sofa"
            }
            game:GetService("ReplicatedStorage").RE:FindFirstChild("1Avata1rOrigina1l"):FireServer(unpack(args))

            wait(1)

            -- Teleporta o jogador para o void
            targetHumanoidRootPart.CFrame = CFrame.new(0, -500, 0)

            wait(0.5)

            -- Retorna para a posição inicial
            humanoidRootPart.CFrame = CFrame.new(originalPosition)
        end
    end
end

-- Função para teleportar até o jogador
local function teleportToPlayer(targetUsername)
    local players = game:GetService("Players")
    local localPlayer = players.LocalPlayer
    local targetPlayer = players:FindFirstChild(targetUsername)

    if targetPlayer and targetPlayer.Character and localPlayer.Character then
        local humanoidRootPart = localPlayer.Character:FindFirstChild("HumanoidRootPart")
        local targetHumanoidRootPart = targetPlayer.Character:FindFirstChild("HumanoidRootPart")

        if humanoidRootPart and targetHumanoidRootPart then
            humanoidRootPart.CFrame = targetHumanoidRootPart.CFrame
        end
    end
end

-- Função para espectar o jogador
local function spectatePlayer(targetUsername)
    local players = game:GetService("Players")
    local localPlayer = players.LocalPlayer
    local targetPlayer = players:FindFirstChild(targetUsername)

    if targetPlayer and targetPlayer.Character then
        local camera = game.Workspace.CurrentCamera
        camera.CameraSubject = targetPlayer.Character:FindFirstChildOfClass("Humanoid")
    end
end

-- Botão para matar o jogador
Tabs.Troll:AddButton({
    Title = "Matar 💀",
    Description = "Mata o jogador usando o sofá",
    Callback = function()
        if selectedPlayer ~= "" then
            killPlayer(selectedPlayer)
        end
    end
})

-- Botão para teleportar até o jogador
Tabs.Troll:AddButton({
    Title = "Teleportar 🏃",
    Description = "Teleporta até o jogador",
    Callback = function()
        if selectedPlayer ~= "" then
            teleportToPlayer(selectedPlayer)
        end
    end
})

-- Botão para espectar o jogador
Tabs.Troll:AddButton({
    Title = "Espectar 👀",
    Description = "Veja o que o jogador está fazendo",
    Callback = function()
        if selectedPlayer ~= "" then
            spectatePlayer(selectedPlayer)
        end
    end
})
