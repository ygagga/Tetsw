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

-- Variáveis de serviços
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

local jogadorSelecionado = nil
local playerNameInput = nil

-- Função para buscar jogadores pelo nome
local function gplr(String)
    local strl = String:lower()
    for _, v in pairs(Players:GetPlayers()) do
        if v.Name:lower():sub(1, #strl) == strl then
            return v
        end
    end
    return nil
end

-- Função para visualizar o jogador
local function spectatePlayer(player)
    local camera = workspace.CurrentCamera
    if player and player.Character then
        camera.CameraSubject = player.Character
        Window:Notify({
            Title = "Visualizando Jogador",
            Description = "Você está visualizando " .. player.Name,
            Duration = 5
        })
    else
        camera.CameraSubject = LocalPlayer.Character
        Window:Notify({
            Title = "Visualização Desativada",
            Description = "Você voltou a visualizar seu personagem",
            Duration = 5
        })
    end
end

-- Função para matar jogador usando o sofá
local function killPlayer(target)
    if not target or not target.Character then
        Window:Notify({
            Title = "Erro",
            Description = "Jogador inválido!",
            Duration = 5
        })
        return
    end

    -- Spawnar sofá na mão do jogador
    local args = {
        [1] = "VehicleSpawn",
        [2] = "HouseSofa",
        [3] = LocalPlayer
    }
    game:GetService("ReplicatedStorage").RE:FindFirstChild("1Car"):FireServer(unpack(args))

    wait(1)  -- Tempo para garantir que o sofá spawnou

    -- Pegar o jogador com o sofá e teleportar para o void
    LocalPlayer.Character.HumanoidRootPart.CFrame = target.Character.HumanoidRootPart.CFrame
    wait(0.5)
    target.Character.HumanoidRootPart.CFrame = CFrame.new(0, -500, 0) -- Void

    wait(1)

    -- Voltar para posição original
    LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(0, 10, 0)

    Window:Notify({
        Title = "Jogador Eliminado!",
        Description = "O jogador " .. target.Name .. " foi enviado para o void!",
        Duration = 5
    })
end

-- Input para selecionar o jogador
TrollTab:AddInput("player_select", {
    Title = "Nome do Jogador",
    Description = "Digite pelo menos 3 letras",
    Placeholder = "Nome",
    Numeric = false,
    Finished = true,
    Callback = function(value)
        jogadorSelecionado = gplr(value)
        if jogadorSelecionado then
            Window:Notify({
                Title = "Jogador Selecionado",
                Description = "Você selecionou: " .. jogadorSelecionado.Name,
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

-- Botão para visualizar o jogador
TrollTab:AddToggle({
    Title = "Visualizar Jogador",
    Description = "Foca a câmera no jogador selecionado",
    Callback = function(state)
        spectatePlayer(state and jogadorSelecionado or nil)
    end
})

-- Botão para matar o jogador
TrollTab:AddButton({
    Title = "Matar Jogador",
    Description = "Manda o jogador selecionado para o void!",
    Callback = function()
        if jogadorSelecionado then
            killPlayer(jogadorSelecionado)
        else
            Window:Notify({
                Title = "Erro",
                Description = "Nenhum jogador selecionado!",
                Duration = 5
            })
        end
    end
})
