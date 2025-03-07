-- Carrega a biblioteca OrionLib
local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()

-- Cria a Janela Principal
local Window = OrionLib:MakeWindow({
    Name = "Brookhaven RP 🏡 (Troll Hub 🤡)",
    HidePremium = false,
    SaveConfig = false,
    ConfigFolder = "TrollHub"
})

-- Criando a Aba de Scripts Universais
local ScriptsTab = Window:MakeTab({
    Name = "☢️ Scripts Universais",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

-- Adicionando seção para Scripts Universais
ScriptsTab:AddSection("Scripts Universais abaixo ☟")

-- Botão para Fly GUI V3
ScriptsTab:AddButton({
    Name = "Fly gui V3🕊️",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/XNEOFF/FlyGuiV3/main/FlyGuiV3.txt"))()
    end
})

-- Botão para Rael Hub
ScriptsTab:AddButton({
    Name = "Rael hub",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/Laelmano24/Rael-Hub/refs/heads/main/Universal/script.txt"))()
    end
})

-- Botão para Mango Hub
ScriptsTab:AddButton({
    Name = "Mango hub 🥭",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/rogelioajax/lua/main/MangoHub", true))()
    end
})

-- Faz a interface aparecer
OrionLib:Init()
