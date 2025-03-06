-- Carregar bibliotecas
local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

-- Criar interface
local Window = Fluent:CreateWindow({
    Title = "Brookhaven RP - Troll Hub 🤡",
    SubTitle = "🔥 Zoando geral!",
    TabWidth = 160,
    Size = UDim2.fromOffset(500, 320),
    Acrylic = true,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl
})

local TrollTab = Window:AddTab({ Title = "🤡 Troll", Icon = "alert" })

local LocalPlayer = game.Players.LocalPlayer
local selectedPlayer = nil

-- Função para encontrar um jogador pelo nome
local function encontrarJogador(nome)
    for _, jogador in ipairs(game.Players:GetPlayers()) do
        if string.lower(jogador.Name) == string.lower(nome) then
            return jogador
        end
    end
    return nil
end

-- Função para copiar a skin do jogador
local function copiarSkin(jogadorAlvo)
    local targetCharacter = jogadorAlvo.Character
    local localCharacter = LocalPlayer.Character

    if not targetCharacter or not localCharacter then
        Window:Notify({
            Title = "Erro",
            Description = "Personagem não encontrado!",
            Duration = 5
        })
        return
    end

    -- Remove as roupas atuais do jogador local
    for _, item in ipairs(localCharacter:GetChildren()) do
        if item:IsA("Accessory") or item:IsA("Shirt") or item:IsA("Pants") or item:IsA("ShirtGraphic") then
            item:Destroy()
        end
    end

    -- Copia as roupas e acessórios do jogador alvo
    for _, item in ipairs(targetCharacter:GetChildren()) do
        if item:IsA("Accessory") or item:IsA("Shirt") or item:IsA("Pants") or item:IsA("ShirtGraphic") then
            local clone = item:Clone()
            clone.Parent = localCharacter
        end
    end

    -- Copia a cor do corpo (skin)
    local targetBodyColors = targetCharacter:FindFirstChild("Body Colors")
    local localBodyColors = localCharacter:FindFirstChild("Body Colors")

    if targetBodyColors and localBodyColors then
        localBodyColors.HeadColor = targetBodyColors.HeadColor
        localBodyColors.LeftArmColor = targetBodyColors.LeftArmColor
        localBodyColors.RightArmColor = targetBodyColors.RightArmColor
        localBodyColors.LeftLegColor = targetBodyColors.LeftLegColor
        localBodyColors.RightLegColor = targetBodyColors.RightLegColor
        localBodyColors.TorsoColor = targetBodyColors.TorsoColor
    end

    Window:Notify({
        Title = "Sucesso!",
        Description = "Skin copiada de " .. jogadorAlvo.Name .. "!",
        Duration = 5
    })
end

-- Função para matar o jogador (teleportando para o void)
local function matarJogador()
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-212, -499, -627)
        Window:Notify({
            Title = "Jogador Eliminado!",
            Description = "Você teleportou o jogador para o void.",
            Duration = 5
        })
    else
        Window:Notify({
            Title = "Erro!",
            Description = "Não foi possível matar o jogador.",
            Duration = 5
        })
    end
end

-- Adicionar input para selecionar jogador
Window:AddInput({
    Title = "Nome do Jogador",
    Description = "Digite o nome exato",
    Tab = TrollTab,
    Callback = function(value)
        selectedPlayer = encontrarJogador(value)
        if selectedPlayer then
            Window:Notify({
                Title = "Jogador Selecionado",
                Description = "Jogador encontrado: " .. selectedPlayer.Name,
                Duration = 5
            })
        else
            Window:Notify({
                Title = "Erro",
                Description = "Nenhum jogador encontrado!",
                Duration = 5
            })
        end
    end
})

-- Adicionar botão para copiar a skin
Window:AddButton({
    Title = "Copiar Skin",
    Description = "Rouba a skin do jogador selecionado",
    Tab = TrollTab,
    Callback = function()
        if selectedPlayer then
            copiarSkin(selectedPlayer)
        else
            Window:Notify({
                Title = "Erro",
                Description = "Nenhum jogador selecionado!",
                Duration = 5
            })
        end
    end
})

-- Adicionar botão para matar o jogador
Window:AddButton({
    Title = "Matar Jogador",
    Description = "Teleporta o jogador para o void",
    Tab = TrollTab,
    Callback = function()
        matarJogador()
    end
})
