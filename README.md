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
    Music = Window:AddTab({ Title = "🎶 Música", Icon = "music" }),
    About = Window:AddTab({ Title = "ℹ️ Sobre", Icon = "info" })
}


-- Adiciona uma seção para controle de jogadores na aba Troll
Tabs.Troll:AddSection("Controle de Jogadores")

local selectedPlayer = ""
local isSpectating = false  -- Variável para controlar o espectar

-- Campo de entrada para o nome do jogador
Tabs.Troll:AddInput("PlayerName", {
    Title = "Nome do Jogador",
    Default = "",
    Placeholder = "Digite o nome do jogador",
    Callback = function(value)
        selectedPlayer = value
    end
})

-- Função para teleportar todos os jogadores para o local do jogador que executou o comando
local function teleportAllPlayers()
    local players = game:GetService("Players")
    local localPlayer = players.LocalPlayer
    local localHumanoidRootPart = localPlayer.Character:FindFirstChild("HumanoidRootPart")

    if localHumanoidRootPart then
        -- Teleportando todos os jogadores para a posição do jogador atual
        for _, targetPlayer in pairs(players:GetPlayers()) do
            if targetPlayer.Character and targetPlayer ~= localPlayer then
                local targetHumanoidRootPart = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
                if targetHumanoidRootPart then
                    targetHumanoidRootPart.CFrame = localHumanoidRootPart.CFrame
                end
            end
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
        isSpectating = true
    end
end

-- Função para despectar (retornar a câmera para o jogador original)
local function despectatePlayer()
    local players = game:GetService("Players")
    local localPlayer = players.LocalPlayer
    local camera = game.Workspace.CurrentCamera
    camera.CameraSubject = localPlayer.Character:FindFirstChildOfClass("Humanoid")
    isSpectating = false
end

-- Botão para teleportar todos os jogadores para o jogador
Tabs.Troll:AddButton({
    Title = "Teleportar Todos 🏃‍♂️",
    Description = "Teleporta todos os jogadores para você!",
    Callback = function()
        teleportAllPlayers()
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

-- Botão para despectar (voltar para o jogador original)
Tabs.Troll:AddButton({
    Title = "Despectar 🚶‍♂️",
    Description = "Volte para o seu personagem!",
    Callback = function()
        if isSpectating then
            despectatePlayer()
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


-----------------------------------------------------------
-- 🎶 Música
-----------------------------------------------------------

Tabs.Music:AddSection("Reproduzir Música para Todos")

local musicId = ""  -- ID da música a ser tocada
local loopMusic = false  -- Controle de loop da música

-- Função para tocar música em loop para todos os jogadores
local function playMusicForAll(id, loop)
    local players = game:GetService("Players")
    
    for _, player in pairs(players:GetPlayers()) do
        local character = player.Character
        if character then
            -- Cria um objeto de som
            local sound = Instance.new("Sound")
            sound.SoundId = "rbxassetid://" .. id
            sound.Looped = loop
            sound.Volume = 0.5  -- Define o volume (ajuste conforme necessário)
            sound.Parent = character:FindFirstChild("HumanoidRootPart")  -- Toca no "HumanoidRootPart"
            sound:Play()
        end
    end
end

-- Campo de entrada para o ID da música
Tabs.Music:AddInput("MusicID", {
    Title = "ID da Música",
    Default = "",
    Placeholder = "Digite o ID da música",
    Numeric = true,
    Finished = true,
    Callback = function(value)
        musicId = value
    end
})

-- Campo de seleção para o loop da música
Tabs.Music:AddToggle("LoopMusic", {
    Title = "Loop",
    Default = false,
    Callback = function(value)
        loopMusic = value
    end
})

-- Botão para iniciar a música para todos os jogadores
Tabs.Music:AddButton({
    Title = "Reproduzir Música 🎶",
    Description = "Reproduza a música para todos os jogadores.",
    Callback = function()
        if musicId ~= "" then
            playMusicForAll(musicId, loopMusic)
        else
            game:GetService("StarterGui"):SetCore("SendNotification", {
                Title = "Erro",
                Text = "ID da música não fornecido.",
                Duration = 3
            })
        end
    end
})


