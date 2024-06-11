# Menu-TweenService
-- Verion 1,0 du Gui TweenService

-- Ce script est un Gui de TweenService pour se téléporter au joueur dans la map 

-- Ce script est la propriéter de maxproglitcher et il est Copyright!!!

-- Mes oui le code est Open Source pour prouver le script est pas un Script Malware...

-- Ce script risque d'être mise a jour vue de la documentation .Lua qui évolue a force des années..

game:GetService("StarterGui"):SetCore("SendNotification",{
  Title = "Tp au joueurs", 
  Text = "Script a été executer", 
  Icon = "rbxassetid://14682078456",
  Duration = 15
})

game:GetService("StarterGui"):SetCore("SendNotification",{
  Title = "Version 1.0", 
  Text = "Script a été executer", 
  Icon = "rbxassetid://14682078456",
  Duration = 15
})
 
-- Code pour le Bruit de ouverture du Gui...

task.spawn(function()
  local beep = Instance.new("Sound");
  beep.Volume = 1; -- de 0 à 1, vous pouvez en faire plus mais ce sera bruyant
  beep.SoundId = "rbxassetid://232127604";
  beep.Parent = game:GetService("CoreGui");
  beep:Play();
  beep.Ended:Wait();
  beep:Destroy();
end)
 
print ("Le menu a été activer , tout les joueurs de la game se mettras a jour tout le long de la game ")
 
print ("Merci de utiliser mon script!!!!")
 
local player = game.Players.LocalPlayer
local TweenService = game:GetService("TweenService")
 
-- Créer un ScreenGui pour contenir la liste des lecteurs personnalisés
local playerListGui = Instance.new("ScreenGui")
playerListGui.Parent = game.CoreGui
playerListGui.Enabled = true -- Initialement, l'interface graphique est activée
 
-- Créer un cadre pour l'arrière-plan
local backgroundFrame = Instance.new("Frame")
backgroundFrame.Parent = playerListGui
backgroundFrame.Size = UDim2.new(0, 200, 0, 400)
backgroundFrame.Position = UDim2.new(1, -200, 0, 0) -- Coin supérieur droit
backgroundFrame.BackgroundColor3 = Color3.new(0, 0, 0)
backgroundFrame.BackgroundTransparency = 0.5 -- 50% la transparence
backgroundFrame.BorderSizePixel = 0
 
-- Créer un TextLabel pour le nombre de joueurs
local playerCountLabel = Instance.new("TextLabel")
playerCountLabel.Parent = backgroundFrame
playerCountLabel.Size = UDim2.new(1, 0, 0, 30)
playerCountLabel.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
playerCountLabel.BorderSizePixel = 0
playerCountLabel.Text = "Players: " .. #game.Players:GetPlayers()
playerCountLabel.TextSize = 18
playerCountLabel.TextColor3 = Color3.new(1, 1, 1)
 
-- Créer un ScrollingFrame pour contenir la liste des joueurs
local playerListFrame = Instance.new("ScrollingFrame")
playerListFrame.Parent = backgroundFrame
playerListFrame.Size = UDim2.new(1, 0, 1, -30)
playerListFrame.Position = UDim2.new(0, 0, 0, 30)
playerListFrame.BackgroundTransparency = 1 -- La rendre totalement transparente
playerListFrame.ScrollBarThickness = 5
 
-- Créer un UIListLayout pour organiser les étiquettes verticalement
local listLayout = Instance.new("UIListLayout")
listLayout.Parent = playerListFrame
listLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
listLayout.VerticalAlignment = Enum.VerticalAlignment.Top
listLayout.SortOrder = Enum.SortOrder.LayoutOrder
 
-- Fonction permettant de créer un TextButton pour chaque joueur avec des animations de survol et de clic.
local function createPlayerButton(playerName)
    local button = Instance.new("TextButton")
    button.Parent = playerListFrame
    button.Size = UDim2.new(1, 0, 0, 30)
    button.BackgroundColor3 = Color3.new(197, 0, 0) -- Couleur de fond bleue
    button.BackgroundTransparency = 0.5 -- 50% de transparence
    button.BorderSizePixel = 0
    button.Text = playerName
    button.TextSize = 18
    button.TextColor3 = Color3.new(1, 1, 1)
 
    -- Effet de survol
    local defaultColor = button.BackgroundColor3
    local hoverColor = Color3.new(0.15, 0.6, 1) -- Bleu plus clair au survol
 
    button.MouseEnter:Connect(function()
        button:TweenSize(UDim2.new(1, 0, 0, 40), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.2, true)
        button.BackgroundColor3 = hoverColor
    end)
 
    button.MouseLeave:Connect(function()
        button:TweenSize(UDim2.new(1, 0, 0, 30), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.2, true)
        button.BackgroundColor3 = defaultColor
    end)
 
    -- Téléportation du lecteur local vers le lecteur sélectionné à l'aide d'un tweening
    button.MouseButton1Click:Connect(function()
        local targetPlayer = game.Players:FindFirstChild(playerName)
        if targetPlayer then
            local targetCharacter = targetPlayer.Character
            if targetCharacter then
                local targetHumanoidRootPart = targetCharacter:FindFirstChild("HumanoidRootPart")
                if targetHumanoidRootPart then
                    local teleportDestination = targetHumanoidRootPart.Position + Vector3.new(0, 5, 0)
                    local playerCharacter = player.Character
                    if playerCharacter then
                        local playerHumanoidRootPart = playerCharacter:FindFirstChild("HumanoidRootPart")
                        if playerHumanoidRootPart then
                            local tweenInfo = TweenInfo.new(
                                1, -- Duration
                                Enum.EasingStyle.Quad, -- Un style apaisé
                                Enum.EasingDirection.Out -- Assouplir la direction
                            )
                            local targetCFrame = CFrame.new(teleportDestination)
                            local tween = TweenService:Create(playerHumanoidRootPart, tweenInfo, {CFrame = targetCFrame})
                            tween:Play()
                        end
                    end
                end
            end
        end
    end)
end
 
-- Fonction permettant de mettre à jour la liste des joueurs et d'ajuster la taille de l'interface graphique
local function updatePlayerList()
    local players = game.Players:GetPlayers()
    local numPlayers = #players
    local guiHeight = math.max(numPlayers * 40, 400) -- Hauteur minimale de 400
 
    -- Mise à jour de l'étiquette du nombre de joueurs
    playerCountLabel.Text = "Players: " .. numPlayers
 
    -- Effacer la liste des joueurs
    for _, child in pairs(playerListFrame:GetChildren()) do
        if child:IsA("TextButton") then
            child:Destroy()
        end
    end
 
    --Créer un TextButton pour chaque joueur du serveur
    for _, playerObj in ipairs(players) do
        if playerObj ~= player then
            createPlayerButton(playerObj.Name)
        end
    end
 
  --  Ajuster la taille de l'arrière-plan à la liste mise à jour
    backgroundFrame.Size = UDim2.new(0, 200, 0, guiHeight + 30)
end
 
-- Première mise à jour de la liste des joueurs
updatePlayerList()
 
-- Se connecter aux événements d'ajout et de départ de joueurs pour mettre à jour la liste des joueurs.
game.Players.PlayerAdded:Connect(updatePlayerList)
game.Players.PlayerRemoving:Connect(updatePlayerList)
 
--  Fonction permettant d'activer et de désactiver la visibilité de l'interface graphique à l'aide de la touche "T
local function toggleGUIVisibility()
    playerListGui.Enabled = not playerListGui.Enabled
end
 
-- Écouter la pression de la touche "T
game:GetService("UserInputService").InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.T then
        toggleGUIVisibility()
    end
end)
 
warn ("Si vous rencontrez des problème avec mon script aller m'écricre en privé sur Discord: maxproglitcher ")
