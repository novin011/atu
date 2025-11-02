local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local UserInputService = game:GetService("UserInputService")

local Window = Rayfield:CreateWindow({
    Name = "AWOL SCRIPTS",
    LoadingTitle = "AWOL SCRIPTS",
    LoadingSubtitle = "by novin011",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "AWOL_SCRIPTS"
    },
    Discord = { Enabled = false },
    KeySystem = false
})

local MainTab = Window:CreateTab("Principal", 4483362458)
local PainelTab = Window:CreateTab("Painel Jogador", 4483362458)

MainTab:CreateButton({
    Name = "Ativar ESP (AWOL)",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/novin011/awol-scprt/refs/heads/main/README.md"))()
        Rayfield:Notify("AWOL SCRIPTS", "ESP ativada!", 2)
    end,
})

MainTab:CreateButton({
    Name = "Ativar Anti AFK (AWOL)",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/novin011/scrptawol/refs/heads/main/README.md"))()
        Rayfield:Notify("AWOL SCRIPTS", "Anti AFK ativado!", 2)
    end,
})

local viewingPlayer = nil

PainelTab:CreateInput({
    Name = "Digite nick ou display do jogador",
    PlaceholderText = "Exemplo: novin",
    RemoveTextAfterFocusLost = false,
    Callback = function(nome)
        PainelTab:CreateSection("Funções para o jogador encontrado:")
        local match = nil
        for _,plr in pairs(Players:GetPlayers()) do
            if string.lower(plr.Name):find(string.lower(nome), 1, true) or (plr.DisplayName and string.lower(plr.DisplayName):find(string.lower(nome), 1, true)) then
                match = plr
                break
            end
        end

        if match then
            PainelTab:CreateParagraph({
                Title = "@"..match.Name,
                Content = "UserID: "..match.UserId.."\nDisplay: "..(match.DisplayName or match.Name)
            })
            PainelTab:CreateButton({
                Name = "Teleporte para "..match.Name,
                Callback = function()
                    if match.Character and match.Character:FindFirstChild("HumanoidRootPart") and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                        LocalPlayer.Character.HumanoidRootPart.CFrame = match.Character.HumanoidRootPart.CFrame + Vector3.new(2,0,0)
                        Rayfield:Notify("Painel Jogador", "Teleportado para: " .. match.Name, 2)
                    else
                        Rayfield:Notify("Painel Jogador", "Jogador não encontrado!", 2)
                    end
                end,
            })
            PainelTab:CreateButton({
                Name = "View (Ver tela de "..match.Name..") [Pressione H para sair]",
                Callback = function()
                    if match.Character and match.Character:FindFirstChild("Head") then
                        workspace.CurrentCamera.CameraSubject = match.Character.Head
                        viewingPlayer = true
                        Rayfield:Notify("Painel Jogador", "Agora vendo a tela de: " .. match.Name.."\nPressione [H] para sair do view.", 2)
                    else
                        Rayfield:Notify("Painel Jogador", "Jogador não encontrado!", 2)
                    end
                end,
            })
        else
            PainelTab:CreateParagraph({
                Title = "Nenhum jogador encontrado",
                Content = "Digite corretamente o nick/display!"
            })
        end
    end,
})

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.H and viewingPlayer then
        -- Sair do view e voltar para si mesmo
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Head") then
            workspace.CurrentCamera.CameraSubject = LocalPlayer.Character.Head
            Rayfield:Notify("Painel Jogador", "Saiu do view! Agora está vendo sua própria tela.", 2)
            viewingPlayer = false
        end
    end
end)

Rayfield:Notify("AWOL SCRIPTS", "Script carregado! Use as abas e funções. Pressione [H] para sair do view.", 5)
