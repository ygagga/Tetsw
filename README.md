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
    Troll = Window:AddTab({ Title = "🤡 Troll", Icon = "alert" }),
    About = Window:AddTab({ Title = "ℹ️ Sobre", Icon = "info" })
}

-----------------------------------------------------------
-- 🤡 Troll (Teleport, Spectar e Matar com Sofá)
-----------------------------------------------------------
Tabs.Troll:AddSection("Zoando geral! 💀")

-- Input para escolher o jogador
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

-- Botão para Spectar o Jogador
Tabs.Troll:AddButton({
    Title = "Spectar Jogador",
    Description = "Veja tudo o que o jogador está fazendo!",
    Callback = function()
        local players = game:GetService("Players")
        local target = players:FindFirstChild(TargetPlayer)
        if target and target.Character then
            workspace.CurrentCamera.CameraSubject = target.Character:FindFirstChildOfClass("Humanoid")
            Fluent:Notify({
                Title = "Spectando...",
                Content = "Pressione qualquer tecla para sair do modo espectador.",
                Duration = 5
            })

            -- Resetar a câmera quando o jogador pressionar uma tecla
            game:GetService("UserInputService").InputBegan:Connect(function()
                workspace.CurrentCamera.CameraSubject = players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            end)
        else
            Fluent:Notify({
                Title = "Erro!",
                Content = "Jogador não encontrado!",
                Duration = 3
            })
        end
    end
})

-- Botão para Matar com Sofá
Tabs.Troll:AddButton({
    Title = "Matar com Sofá 💀",
    Description = "Usa um sofá para mandar o jogador pro void!",
    Callback = function()
        local players = game:GetService("Players")
        local target = players:FindFirstChild(TargetPlayer)
        local localPlayer = players.LocalPlayer
        local character = localPlayer.Character

        if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
            -- Salvar posição original
            local originalPosition = character.HumanoidRootPart.Position

            -- Criar sofá
            local seat = Instance.new("Seat")
            seat.Parent = workspace
            seat.Anchored = false
            seat.Position = target.Character.HumanoidRootPart.Position + Vector3.new(0, 3, 0)

            -- Loop para prender o jogador no sofá
            local killing = true
            task.spawn(function()
                while killing do
                    if target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
                        character:MoveTo(target.Character.HumanoidRootPart.Position)
                        seat.Position = target.Character.HumanoidRootPart.Position + Vector3.new(0, 3, 0)
                    end
                    task.wait(0.1)
                end
            end)

            -- Esperar o jogador sentar
            seat.ChildAdded:Connect(function(child)
                if child:IsA("Weld") and child.Part1 and child.Part1.Parent == target.Character then
                    killing = false -- Parar o loop de teleport

                    -- Jogar no void
                    seat.Position = Vector3.new(0, -500, 0)
                    task.wait(1)

                    -- Voltar para posição original
                    character:MoveTo(originalPosition)

                    -- Remover sofá
                    seat:Destroy()
                    Fluent:Notify({
                        Title = "Morto!",
                        Content = TargetPlayer .. " foi eliminado no void! 💀",
                        Duration = 3
                    })
                end
            end)
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
