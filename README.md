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


local playerService = game:GetService('Players')
local runService = game:GetService('RunService')

local player = playerService.LocalPlayer
local selectedPlayer = nil
local isSpectating = false
local camera = workspace.CurrentCamera



-- Função para encontrar jogadores pelo nome
local function gplr(String)
    local strl = String:lower()
    for _, v in pairs(playerService:GetPlayers()) do
        if v.Name:lower():sub(1, #strl) == strl then
            return v
        end
    end
    return nil
end

-- Função para teleportar o jogador para o void ao sentar
local function teleportToVoid()
    player.Character.HumanoidRootPart.CFrame = CFrame.new(Vector3.new(-47, -469, -44))
end

-- Função para seguir o jogador e teleportá-lo ao void ao sentar
local function followAndTeleport(targetPlayer)
    if not targetPlayer or not targetPlayer.Character then return end

    local connection
    connection = runService.Heartbeat:Connect(function()
        if targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local targetCFrame = targetPlayer.Character.HumanoidRootPart.CFrame
            player.Character.HumanoidRootPart.CFrame = targetCFrame + Vector3.new(0, 1, 0)

            if targetPlayer.Character:FindFirstChild("Humanoid") and targetPlayer.Character.Humanoid.Sit then
                teleportToVoid()
                connection:Disconnect()
            end
        else
            connection:Disconnect()
        end
    end)
end

-- Função para teleportar até o jogador
local function teleportToPlayer(targetPlayer)
    if targetPlayer and targetPlayer.Character and player.Character then
        player.Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
    end
end

-- Função para espectar o jogador
local function spectatePlayer(targetPlayer)
    if targetPlayer and targetPlayer.Character then
        isSpectating = true
        camera.CameraSubject = targetPlayer.Character:FindFirstChildOfClass("Humanoid")
    end
end

-- Função para parar de espectar
local function stopSpectating()
    isSpectating = false
    camera.CameraSubject = player.Character:FindFirstChildOfClass("Humanoid")
end

-- Adiciona uma seção para controle de jogadores na aba Troll
Tabs.Troll:AddSection("Controle de Jogadores")

-- Campo de entrada para o nome do jogador
Tabs.Troll:AddInput("PlayerName", {
    Title = "Nome do Jogador",
    Default = "",
    Placeholder = "Digite o nome do jogador",
    Callback = function(value)
        selectedPlayer = gplr(value)
        if selectedPlayer then
            Window:Notify({ Title = "Jogador Encontrado", Description = selectedPlayer.Name, Duration = 3 })
        else
            Window:Notify({ Title = "Erro", Description = "Nenhum jogador encontrado!", Duration = 3 })
        end
    end
})

-- Botão para matar (teletransportar ao void)
Tabs.Troll:AddButton({
    Title = "Matar 💀",
    Description = "Mata o jogador selecionado.",
    Callback = function()
        if selectedPlayer then
            followAndTeleport(selectedPlayer)
        else
            Window:Notify({ Title = "Erro", Description = "Nenhum jogador selecionado!", Duration = 3 })
        end
    end
})

-- Botão para teleportar até o jogador
Tabs.Troll:AddButton({
    Title = "Teleportar 🏃",
    Description = "Teleporta até o jogador.",
    Callback = function()
        if selectedPlayer then
            teleportToPlayer(selectedPlayer)
        else
            Window:Notify({ Title = "Erro", Description = "Nenhum jogador selecionado!", Duration = 3 })
        end
    end
})

-- Botão para espectar o jogador
Tabs.Troll:AddButton({
    Title = "Espectar 👀",
    Description = "Veja o que o jogador está fazendo.",
    Callback = function()
        if selectedPlayer then
            spectatePlayer(selectedPlayer)
        else
            Window:Notify({ Title = "Erro", Description = "Nenhum jogador selecionado!", Duration = 3 })
        end
    end
})

-- Botão para parar de espectar
Tabs.Troll:AddButton({
    Title = "Parar de Espectar ❌",
    Description = "Retorna sua visão para você mesmo.",
    Callback = function()
        stopSpectating()
        Window:Notify({ Title = "Visualização",
        


-----------------------------------------------------------
-- 🎶 Música
-----------------------------------------------------------

Tabs.Music:AddSection("Reproduzir Música para Todos")

local musicId = ""  -- ID da música a ser tocada
local loopMusic = false  -- Controle de loop da música
local musicPlaying = nil  -- Armazena o som que está tocando

-- Função para tocar música em loop para todos os jogadores
local function playMusicForAll(id, loop)
    -- Checa se já existe uma música tocando, e se sim, para ela
    if musicPlaying then
        musicPlaying:Stop()
        musicPlaying:Destroy()
    end

    -- Cria um novo objeto de som no Workspace
    musicPlaying = Instance.new("Sound")
    musicPlaying.SoundId = "rbxassetid://" .. id
    musicPlaying.Looped = loop
    musicPlaying.Volume = 1  -- Volume máximo
    musicPlaying.Parent = game:GetService("Workspace")  -- Coloca o som no Workspace, assim todos podem ouvir

    musicPlaying:Play()
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
        end
    end
})
