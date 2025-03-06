-- Carregar bibliotecas Fluent
local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

-- Criar janela Fluent
local Window = Fluent:CreateWindow({
    Title = "Script de Kill - Brookhaven",
    SubTitle = "by ChatGPT",
    TabWidth = 160,
    Size = UDim2.new(0, 500, 0, 400),
    Acrylic = true,
    Theme = "Dark"
})

-- Criar aba Trool
local Main = Window:AddTab({ Title = "Troll", Icon = "rbxassetid://4483345998" })

-- Variáveis de jogo
local playerService = game:GetService("Players")
local runService = game:GetService("RunService")
local player = playerService.LocalPlayer
local jogadorSelecionado = nil
local listaJogadores = {}

-- Função para atualizar a lista de jogadores
local function atualizarLista()
    listaJogadores = {}
    for _, v in pairs(playerService:GetPlayers()) do
        table.insert(listaJogadores, v.Name)
    end
    return listaJogadores
end

-- Função para teleportar jogador para o void
local function teleportToVoid()
    if jogadorSelecionado and jogadorSelecionado.Character then
        local targetHRP = jogadorSelecionado.Character:FindFirstChild("HumanoidRootPart")
        if targetHRP then
            targetHRP.CFrame = CFrame.new(-212, -499, -627) -- Local do void
        end
    end
end

-- Criar dropdown de seleção de jogador
Window:AddDropdown({
    Title = "Selecione um Jogador",
    Values = atualizarLista(),
    MultiSelection = false,
    Tab = Main,
    Callback = function(value)
        jogadorSelecionado = playerService:FindFirstChild(value)
        if jogadorSelecionado then
            Window:Notify({
                Title = "Jogador Selecionado",
                Description = "Você escolheu: " .. jogadorSelecionado.Name,
                Duration = 5
            })
        end
    end
})

-- Atualizar a lista automaticamente quando um jogador entra ou sai
playerService.PlayerAdded:Connect(function()
    Window:UpdateDropdown({
        Title = "Selecione um Jogador",
        Values = atualizarLista(),
    })
end)

playerService.PlayerRemoving:Connect(function()
    Window:UpdateDropdown({

       
