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
    Avatar = Window:AddTab({ Title = "👤 Avatar", Icon = "shirt" }),
    Troll = Window:AddTab({ Title = "🤡 Troll", Icon = "alert" }),
    Hacks = Window:AddTab({ Title = "⚡ Hacks", Icon = "zap" }),
    About = Window:AddTab({ Title = "ℹ️ Sobre", Icon = "info" })
}

-----------------------------------------------------------
-- 🤡 Troll (Teleport)
-----------------------------------------------------------
Tabs.Troll:AddSection("Zoando geral! 💀")

-- Input para escolher o jogador a teleportar
local TargetPlayer = ""

Tabs.Troll:AddInput("TargetPlayer", {
    Title = "Nome do Jogador",
    Default = "",
    Placeholder = "Digite o nome...",
    Callback = function(value)
        TargetPlayer = value
    end
})

-- Botão para Teleportar ao Jogador
Tabs.Troll:AddButton({
    Title = "Teleportar para Jogador",
    Description = "Leva você até o jogador selecionado!",
    Callback = function()
        local players = game:GetService("Players")
        local target = players:FindFirstChild(TargetPlayer)
        if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
            players.LocalPlayer.Character:MoveTo(target.Character.HumanoidRootPart.Position)
        else
            Fluent:Notify({
                Title = "Erro!",
                Content = "Jogador não encontrado ou sem personagem!",
                Duration = 3
            })
        end
    end
})

-----------------------------------------------------------
-- ℹ️ Sobre
-----------------------------------------------------------
Tabs.About:AddSection("Criado por Shelby, user discord: snobodj")
Tabs.About:AddParagraph("Criado por Troll Hub para bagunçar no Brookhaven RP!")
Tabs.About:AddParagraph("Aproveite e divirta-se, mas sem exagerar! 😆")

-- Inicializa a Interface
Window:SelectTab(1)
