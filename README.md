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

-- Função para matar o jogador selecionado com o sofá
local function killPlayer(player)
    local character = player.Character
    if character and character:FindFirstChild("HumanoidRootPart") then
        -- Spawn do sofá na mão do jogador
        local sofa = Instance.new("Part")
        sofa.Size = Vector3.new(5, 1, 5)
        sofa.Anchored = false
        sofa.CanCollide = false
        sofa.Position = game.Players.LocalPlayer.Character.HumanoidRootPart.Position
        sofa.Parent = game.Workspace

        -- Criar um motor de corpo (BodyPosition) para manter o sofá na mão
        local bodyPosition = Instance.new("BodyPosition")
        bodyPosition.MaxForce = Vector3.new(100000, 100000, 100000)
        bodyPosition.P = 10000
        bodyPosition.D = 1000
        bodyPosition.Parent = sofa

        -- Ajeita o sofá para a posição da mão
        local hand = game.Players.LocalPlayer.Character:WaitForChild("RightHand")
        bodyPosition.Position = hand.Position + Vector3.new(0, 2, 0)

        -- Espera até que o sofá seja pego
        wait(0.5)

        -- Teleportar o jogador selecionado para o void
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
        humanoidRootPart.CFrame = CFrame.new(-5000, -5000, -5000)

        -- Matar o jogador
        humanoidRootPart.CFrame = CFrame.new(-5000, -5000, -5000)

        -- Voltar o jogador que fez a ação para o local inicial
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-212, -499, -627)
        
        -- Esperar um pouco e destruir o sofá
        wait(1)
        sofa:Destroy()
    end
end

-- Criando Abas
local Tabs = {
    Troll = Window:AddTab({ Title = "🤡 Troll", Icon = "alert" })
}

-----------------------------------------------------------
-- 🤡 Troll
-----------------------------------------------------------
Tabs.Troll:AddSection("Matar Jogador com Sofá")

-- Selecionar jogador e matar
Tabs.Troll:AddInput("Jogador para matar", {
    Title = "Digite o nome do jogador",
    Default = "",
    Placeholder = "Nome do jogador",
    Numeric = false,
    Finished = true,
    Callback = function(playerName)
        local player = game.Players:FindFirstChild(playerName)
        if player then
            killPlayer(player)
        else
            game:GetService("StarterGui"):SetCore("SendNotification", {
                Title = "Erro!",
                Text = "Jogador não encontrado!",
                Duration = 3
            })
        end
    end
})
