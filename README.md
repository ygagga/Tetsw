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

-- Função para trocar a cabeça do avatar
local function changeAvatar(id, notificationTitle)
    local argsTable = (type(id) == "table") and id or {1, 1, 1, 1, 1, id}

    local args = {
        [1] = "CharacterChange",
        [2] = argsTable,
        [3] = "🔥 Troll Hub 💀"
    }

    local replicatedStorage = game:GetService("ReplicatedStorage")
    local starterGui = game:GetService("StarterGui")

    if replicatedStorage and starterGui then
        local remote = replicatedStorage.RE:FindFirstChild("1Avata1rOrigina1l")
        if remote then
            remote:FireServer(unpack(args))
            starterGui:SetCore("SendNotification", {
                Title = notificationTitle,
                Text = "Aguarde 1-10 segundos...",
                Duration = 5
            })
        end
    end
end

-- Criando Abas
local Tabs = {
    
    Troll = Window:AddTab({ Title = "🤡 Troll", Icon = "alert" }),
    Music = Window:AddTab({ Title = "🎶 Música", Icon = "music" }),
    Hacks = Window:AddTab({ Title = "⚡ Hacks", Icon = "zap" }),
    Scripts = Window:AddTab({ Title = "🧑‍💻 Scripts", Icon = "code" }),
    About = Window:AddTab({ Title = "ℹ️ Sobre", Icon = "info" })
}


-----------------------------------------------------------
-- 🤡 Troll (ESP)
-----------------------------------------------------------
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



-- Botão para matar o jogador usando o Kill Brick na interface de seleção
Window:AddButton({
    Title = "Matar 💀",
    Description = "Mata o jogador colocando um Kill Brick no seu local.",
    Tab = Tabs.Troll,
    Callback = function()
        -- Verificando se o jogador foi selecionado
        if jogadorSelecionado then
            -- Criar o Kill Brick no local do jogador selecionado
            spawnKillBrick(jogadorSelecionado.Name)
            Window:Notify({
                Title = "Matar 💀",
                Description = "Kill Brick spawnado para " .. jogadorSelecionado.Name,
                Duration = 5
            })
        else
            Window:Notify({
                Title = "Erro",
                Description = "Nenhum jogador selecionado!",
                Duration = 5
            })
        end
    end
})

-- Função para criar o Kill Brick no local do jogador
local function spawnKillBrick(targetPlayer)
    local target = playerService:FindFirstChild(targetPlayer)

    if target and target.Character then
        local targetHumanoidRootPart = target.Character:FindFirstChild("HumanoidRootPart")

        if targetHumanoidRootPart then
            -- Criando o Kill Brick
            local killBrick = Instance.new("Part")
            killBrick.Size = Vector3.new(5, 1, 5) -- Tamanho do Kill Brick
            killBrick.Position = targetHumanoidRootPart.Position -- Posicionando no local do jogador
            killBrick.Anchored = true
            killBrick.CanCollide = true
            killBrick.BrickColor = BrickColor.new("Bright red") -- Cor do Kill Brick
            killBrick.Parent = game.Workspace

            -- Detectando se o jogador colide com o Kill Brick
            killBrick.Touched:Connect(function(hit)
                if hit.Parent == target.Character then
                    local humanoid = hit.Parent:FindFirstChildOfClass("Humanoid")
                    if humanoid then
                        humanoid.Health = 0 -- Mata o jogador
                    end
                end
            end)

            -- Remover o Kill Brick após 2 segundos
            wait(2)
            killBrick:Destroy()
        end
    end
end


-----------------------------------------------------------
-- 🎶 Música
-------------------------------------------------------------

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



-----------------------------------------------------------
-- ⚡ Hacks (Velocidade + Pulo Infinito + Atravessar Paredes)
-----------------------------------------------------------
local speedActive = false
local jumpActive = false
local wallWalkActive = false

Tabs.Hacks:AddSection("Superpoderes!")

-- Velocidade infinita
Tabs.Hacks:AddButton({
    Title = "Ativar Super Velocidade ⚡",
    Description = "Corre mais rápido que o Flash!",
    Callback = function()
        speedActive = true
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 100
    end
})

-- Desativar Velocidade
Tabs.Hacks:AddButton({
    Title = "Desativar Velocidade ❌",
    Description = "Desativa a Super Velocidade!",
    Callback = function()
        speedActive = false
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 16
    end
})

-- Pulo infinito
Tabs.Hacks:AddButton({
    Title = "Ativar Pulo Infinito 🦘",
    Description = "Pule o quanto quiser sem limites!",
    Callback = function()
        jumpActive = true
        game:GetService("UserInputService").JumpRequest:Connect(function()
            game.Players.LocalPlayer.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end)
    end
})

-- Desativar Pulo Infinito
Tabs.Hacks:AddButton({
    Title = "Desativar Pulo Infinito ❌",
    Description = "Desativa o Pulo Infinito!",
    Callback = function()
        jumpActive = false
        -- Desconectar a função de pulo infinito
        game:GetService("UserInputService").JumpRequest:Disconnect()
    end
})

-- Atravessar Paredes
Tabs.Hacks:AddButton({
    Title = "Ativar Atravessar Paredes 🚪",
    Description = "Agora você pode atravessar as paredes!",
    Callback = function()
        wallWalkActive = true
        local player = game.Players.LocalPlayer
        local character = player.Character
        local humanoid = character:WaitForChild("Humanoid")

        local function enableWallWalk()
            for _, part in pairs(character:GetChildren()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
        end

        enableWallWalk()
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Poder Ativado!",
            Text = "Você agora pode atravessar paredes! 🚪",
            Duration = 5
        })
    end
})

-- Desativar Atravessar Paredes
Tabs.Hacks:AddButton({
    Title = "Desativar Atravessar Paredes ❌",
    Description = "Desativa o poder de atravessar paredes!",
    Callback = function()
        wallWalkActive = false
        local player = game.Players.LocalPlayer
        local character = player.Character

        local function disableWallWalk()
            for _, part in pairs(character:GetChildren()) do
                if part:IsA("BasePart") then
                    part.CanCollide = true
                end
            end
        end

        disableWallWalk()
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Poder Desativado",
            Text = "Você não pode mais atravessar paredes.",
            Duration = 5
        })
    end
})



-- Adiciona os scripts na aba Scripts
Tabs.Scripts:AddSection("Carregar Scripts")

-- Botões para carregar os scripts
Tabs.Scripts:AddButton({
    Title = "Fly Script ✈️",
    Description = "Ativa o Fly script.",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/XNEOFF/FlyGuiV3/main/FlyGuiV3.txt"))()
    end
})

Tabs.Scripts:AddButton({
    Title = "RAEL Hub 🔧",
    Description = "Carrega o RAEL Hub.",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/Laelmano24/Rael-Hub/main/main.txt"))()
    end
})

Tabs.Scripts:AddButton({
    Title = "Sander X 🛸",
    Description = "Carrega o Sander X.",
    Callback = function()
        loadstring(game:HttpGet('https://raw.githubusercontent.com/sXPiterXs1111/Sanderxv3.30/main/sanderx3.30'))()
    end
})



-----------------------------------------------------------
-- ℹ️ Sobre
-----------------------------------------------------------
Tabs.About:AddSection("Criado por Shelby,user discord:snobodj")
Tabs.About:AddParagraph("Criado por Troll Hub para bagunçar no Brookhaven RP!")
Tabs.About:AddParagraph("Aproveite e divirta-se, mas sem exagerar! 😆")

