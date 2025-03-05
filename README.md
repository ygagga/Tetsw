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
        -- Spawn do sofá
        local sofa = Instance.new("Part")
        sofa.Size = Vector3.new(5, 1, 5)
        sofa.Anchored = true
        sofa.CanCollide = false
        sofa.Position = game.Players.LocalPlayer.Character.HumanoidRootPart.Position
        sofa.Parent = game.Workspace

        -- Conectar função de matar com o sofá
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

        -- Teleporta o player selecionado para o void
        sofa.Touched:Connect(function(hit)
            local humanoid = hit.Parent:FindFirstChild("Humanoid")
            if humanoid then
                humanoid.Health = 0
            end
        end)

        -- Teleportar para o local do sofá
        sofa.CFrame = humanoidRootPart.CFrame

        -- Voltar o jogador para o local inicial
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-212, -499, -627)
        
        -- Depois que o jogador for teleportado e morto, remove o sofá
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
