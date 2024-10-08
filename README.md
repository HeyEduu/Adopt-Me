-- Script para gerenciar a adoção de pets

-- Criar uma tabela para armazenar os pets adotados por cada jogador
local playerPets = {}

-- Função para criar um pet
function createPet(petName)
    local pet = Instance.new("Model")
    pet.Name = petName
    
    -- Crie uma parte simples para representar o pet
    local petPart = Instance.new("Part")
    petPart.Size = Vector3.new(2, 2, 2) -- Tamanho do pet
    petPart.Anchored = true -- Não cairá
    petPart.Parent = pet -- Adiciona a parte ao pet
    
    return pet
end

-- Função que será chamada quando o jogador adotar um pet
local function adoptPet(player, petName)
    if not playerPets[player.Name] then
        playerPets[player.Name] = {}
    end
    
    -- Crie o pet
    local pet = createPet(petName)
    
    -- Posicione o pet na frente do jogador
    pet:SetPrimaryPartCFrame(player.Character.HumanoidRootPart.CFrame * CFrame.new(5, 0, 0))
    pet.Parent = game.Workspace
    
    -- Adicionar o pet à lista do jogador
    table.insert(playerPets[player.Name], pet)
    print(player.Name .. " adotou um " .. petName)
end

-- Evento para quando um jogador entra no jogo
game.Players.PlayerAdded:Connect(function(player)
    playerPets[player.Name] = {} -- Iniciar lista vazia de pets para o jogador
    
    -- Comando simples para o jogador adotar um pet
    player.Chatted:Connect(function(message)
        local petName = string.match(message, "^/adopt (.+)$")
        if petName then
            adoptPet(player, petName)
        end
    end)
end)

-- Evento para quando o jogador sair do jogo
game.Players.PlayerRemoving:Connect(function(player)
    playerPets[player.Name] = nil -- Limpar dados de pets quando o jogador sair
end)
