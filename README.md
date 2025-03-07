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
    Avatar = Window:AddTab({ Title = "👤 Avatar", Icon = "shirt" }),
    Troll = Window:AddTab({ Title = "🤡 Troll", Icon = "alert" }),
    Hacks = Window:AddTab({ Title = "⚡ Hacks", Icon = "zap" }),
    local ScriptsTab = Window:AddTab({ Title = "☢️ Scripts Universais", Icon = "zap" })
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
-- 🤡 Troll (ESP)
-----------------------------------------------------------
Tabs.Troll:AddSection("Trollando no servidor!")

-- ESP (Nome do Jogador e Distância)
local espActive = false
local espElements = {}  -- Tabela para armazenar os elementos de ESP criados

Tabs.Troll:AddButton({
    Title = "Ativar ESP 🔍",
    Description = "Veja o nome e distância dos jogadores!",
    Callback = function()
        espActive = true
        for _, player in pairs(game.Players:GetPlayers()) do
            if player.Character and player ~= game.Players.LocalPlayer then
                local billboardGui = Instance.new("BillboardGui")
                billboardGui.Parent = player.Character.Head
                billboardGui.Adornee = player.Character.Head
                billboardGui.Size = UDim2.new(0, 100, 0, 50)
                billboardGui.StudsOffset = Vector3.new(0, 2, 0)
                billboardGui.AlwaysOnTop = true

                local textLabel = Instance.new("TextLabel")
                textLabel.Parent = billboardGui
                textLabel.Size = UDim2.new(1, 0, 1, 0)
                textLabel.BackgroundTransparency = 1
                textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
                textLabel.TextStrokeTransparency = 0.8
                textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
                textLabel.TextSize = 14
                textLabel.Text = player.Name .. "\n" .. tostring((game.Players.LocalPlayer.Character.HumanoidRootPart.Position - player.Character.HumanoidRootPart.Position).Magnitude) .. " studs"

                -- Atualizar a distância dinamicamente
                spawn(function()
                    while espActive do
                        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                            local distance = (game.Players.LocalPlayer.Character.HumanoidRootPart.Position - player.Character.HumanoidRootPart.Position).Magnitude
                            textLabel.Text = player.Name .. "\n" .. math.floor(distance) .. " studs"
                        end
                        wait(0.1)  -- Atualiza a cada 0.1 segundos
                    end
                end)

                table.insert(espElements, {player = player, billboardGui = billboardGui})
            end
        end
    end
})

-- Desativar ESP
Tabs.Troll:AddButton({
    Title = "Desativar ESP ❌",
    Description = "Desativa o ESP!",
    Callback = function()
        espActive = false
        for _, element in ipairs(espElements) do
            if element.billboardGui then
                element.billboardGui:Destroy()
            end
        end
        espElements = {}  -- Limpa a tabela de elementos de ESP
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

-- Adicionando o script de voo
Tabs.Hacks:AddButton({
    Title = "Ativar Voo 🕊️",
    Description = "Voar pelo mapa!",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/mikeexc/Dsc-Mike-Fly-Gui/main/Fly%20Gui"))()
    end
})


-----------------------------------------------------------
-- ☢️ Scipts universais
-----------------------------------------------------------
ScriptsTab:AddSection("Scripts Universais abaixo ☟")

ScriptsTab:AddButton({
    Title = "Fly gui V3🕊️",
    Description = "Voar pelo mapa!",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/XNEOFF/FlyGuiV3/main/FlyGuiV3.txt"))()
    end
})

ScriptsTab:AddButton({
    Title = "Rael hub",
    Description = "Mandrakes👍",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/Laelmano24/Rael-Hub/refs/heads/main/Universal/script.txt"))()
    end
})

ScriptsTab:AddButton({
    Title = "Mango hub",
    Description = "🥭",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/rogelioajax/lua/main/MangoHub", true))()
    end
})




-----------------------------------------------------------
-- ℹ️ Sobre
-----------------------------------------------------------
Tabs.About:AddSection("Criado por Shelby, user discord: snobodj")
Tabs.About:AddParagraph("Criado por Troll Hub para bagunçar no Brookhaven RP!")
Tabs.About:AddParagraph("Aproveite e divirta-se, mas sem exagerar! 😆")
