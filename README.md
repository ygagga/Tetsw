-- Definindo a posição de matar (local onde o jogador vai ser teleportado)
local killLocation = CFrame.new(-212, -499, -627)

-- Variável para armazenar o nome do jogador selecionado
local selectedPlayerName = nil

-- Função para matar o jogador com o sofá
local function killPlayerWithSofa(targetPlayer)
    local character = game.Players.LocalPlayer.Character
    if not character or not targetPlayer.Character then return end

    -- Spawn do sofá na mão do jogador
    local sofa = Instance.new("Part")
    sofa.Size = Vector3.new(4, 1, 4)  -- Tamanho do sofá
    sofa.Position = character.HumanoidRootPart.Position
    sofa.Anchored = false
    sofa.CanCollide = true
    sofa.BrickColor = BrickColor.new("Bright red")
    sofa.Parent = game.Workspace

    -- Criando um Weld para conectar o sofá à mão do jogador
    local weld = Instance.new("Weld")
    weld.Part0 = character:WaitForChild("RightHand")  -- Mão direita do jogador
    weld.Part1 = sofa
    weld.C0 = CFrame.new(0, -1, 0)  -- Ajuste a posição conforme necessário
    weld.Parent = character.RightHand

    -- Pegando o alvo (jogador a ser morto)
    local targetHumanoidRootPart = targetPlayer.Character:WaitForChild("HumanoidRootPart")

    -- Esperando até o sofá ser pego e mover o jogador para o void
    wait(1) -- Tempo para garantir que o jogador está pronto para ser pego
    character.HumanoidRootPart.CFrame = targetHumanoidRootPart.CFrame  -- Teleporta o jogador para você

    -- Teleportando o jogador para o Void
    targetPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(0, -1000, 0)

    -- Retornando ao local original após matar o jogador
    wait(1)  -- Tempo para a animação do teleporte
    character.HumanoidRootPart.CFrame = killLocation
end

-- Função para atualizar a seleção de jogador
local function updateSelectedPlayer(playerName)
    selectedPlayerName = playerName
end

-- Criando a interface do script
local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

-- Criando a janela da interface
local Window = Fluent:CreateWindow({
    Title = "Brookhaven RP 🏡 (Troll Hub 🤡)",
    SubTitle = "🔥 Zoando geral! 💀",
    TabWidth = 160,
    Size = UDim2.fromOffset(500, 320),
    Acrylic = true,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl
})

-- Criando as abas
local Tabs = {
    Avatar = Window:AddTab({ Title = "👤 Avatar", Icon = "shirt" }),
    Troll = Window:AddTab({ Title = "🤡 Troll", Icon = "alert" }),
    Hacks = Window:AddTab({ Title = "⚡ Hacks", Icon = "zap" }),
    About = Window:AddTab({ Title = "ℹ️ Sobre", Icon = "info" })
}

-----------------------------------------------------------
-- 👤 Avatar
-----------------------------------------------------------
Tabs.Avatar:AddSection("Trocar Cabeça")

Tabs.Avatar:AddInput("Head ID", {
    Title = "Digite o ID da Cabeça",
    Default = "",
    Placeholder = "ID",
    Numeric = true,
    Finished = true,
    Callback = function(s)
        changeAvatar(tonumber(s), "Carregando...")
        wait(1)
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Pronto!",
            Text = "Cabeça alterada com sucesso ✅",
            Duration = 3
        })
    end
})

Tabs.Avatar:AddButton({
    Title = "Headless Horseman",
    Description = "Cabeça invisível!",
    Callback = function()
        changeAvatar(134082579, "Headless Horseman Aplicado!")
    end
})

Tabs.Avatar:AddButton({
    Title = "Blaze Burner",
    Description = "Cabeça flamejante 🔥",
    Callback = function()
        changeAvatar(3210773801, "Blaze Burner Aplicado!")
    end
})

-----------------------------------------------------------
-- 🤡 Troll (ESP + KillPlayer com Sofá)
-----------------------------------------------------------
Tabs.Troll:AddSection("Trollando no servidor!")

-- Função para selecionar o jogador
Tabs.Troll:AddDropdown({
    Title = "Selecione o Jogador",
    List = function()
        local players = {}
        for _, player in ipairs(game.Players:GetPlayers()) do
            if player.Name ~= game.Players.LocalPlayer.Name then
                table.insert(players, player.Name)
            end
        end
        return players
    end,
    Callback = function(playerName)
        updateSelectedPlayer(playerName)
    end
})

-- Matar o jogador selecionado
Tabs.Troll:AddButton({
    Title = "Matar Jogador Selecionado com Sofá",
    Description = "Spawnar um sofá e teleportar o jogador para o void.",
    Callback = function()
        local selectedPlayer = game.Players:FindFirstChild(selectedPlayerName)
        if selectedPlayer then
            killPlayerWithSofa(selectedPlayer)
        else
            game:GetService("StarterGui"):SetCore("SendNotification", {
                Title = "Erro",
                Text = "Nenhum jogador selecionado!",
                Duration = 3
            })
        end
    end
})

-----------------------------------------------------------
-- ⚡ Hacks
-----------------------------------------------------------
Tabs.Hacks:AddSection("Superpoderes!")

-- Ativar Velocidade
Tabs.Hacks:AddButton({
    Title = "Ativar Super Velocidade ⚡",
    Description = "Corre mais rápido que o Flash!",
    Callback = function()
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 100
    end
})

-- Pulo infinito
Tabs.Hacks:AddButton({
    Title = "Ativar Pulo Infinito 🦘",
    Description = "Pule o quanto quiser sem limites!",
    Callback = function()
        game:GetService("UserInputService").JumpRequest:Connect(function()
            game.Players.LocalPlayer.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end)
    end
})

-----------------------------------------------------------
-- ℹ️ Sobre
-----------------------------------------------------------
Tabs.About:AddSection("Sobre o Script")
Tabs.About:AddParagraph("Criado por Troll Hub para bagunçar no Brookhaven RP!")
Tabs.About:AddParagraph("Aproveite e divirta-se, mas sem exagerar! 😆")
