local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Aumentar a hitbox do próprio jogador para 1 e deixá-la visível
for _, v in pairs(character:GetChildren()) do
    if v:IsA("BasePart") then
        v.Size = Vector3.new(1, 1, 1)
        v.Transparency = 0 -- Torna a hitbox visível para o próprio jogador
    end
end

-- Definir hitbox dos outros jogadores como 1000 e invisível para eles
for _, otherPlayer in pairs(game.Players:GetPlayers()) do
    if otherPlayer ~= player then
        local otherCharacter = otherPlayer.Character
        if otherCharacter then
            for _, v in pairs(otherCharacter:GetChildren()) do
                if v:IsA("BasePart") then
                    v.Size = Vector3.new(1000, 1000, 1000)
                    v.Transparency = 1 -- Torna a hitbox invisível para os outros jogadores
                end
            end
        end
    end
end

-- Remover tempo de recarga das habilidades e definir para 0,0
local function removeCooldown()
    for _, v in pairs(getgc(true)) do
        if type(v) == "function" and islclosure(v) then
            local constants = debug.getconstants(v)
            for _, c in pairs(constants) do
                if type(c) == "number" and c > 0 then -- Verifica se há tempos de recarga
                    debug.setupvalue(v, 1, 0) -- Define o cooldown como zero
                end
            end
        end
    end
end

removeCooldown()

-- Criar notificação na tela
game.StarterGui:SetCore("SendNotification", {
    Title = "Aviso";
    Text = "Se inscreva no canal do @pedroanjoyt!";
    Duration = 60; -- Duração da notificação em segundos
})

print("Script ativado! Sua hitbox está visível e definida para 1, e a hitbox dos outros jogadores está invisível e definida para 1000. Cooldown removido (definido para 0,0).")

