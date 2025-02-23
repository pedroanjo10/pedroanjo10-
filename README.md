local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

local espEnabled = false

-- Função para ativar/desativar o ESP
local function toggleESP()
    espEnabled = not espEnabled

    -- Caso o ESP esteja ativado
    if espEnabled then
        for _, otherPlayer in pairs(game.Players:GetPlayers()) do
            if otherPlayer ~= player and otherPlayer.Character then
                local otherCharacter = otherPlayer.Character
                local humanoidRootPart = otherCharacter:FindFirstChild("HumanoidRootPart")
                if humanoidRootPart then
                    -- Criar uma linha que conecta o jogador com o outro
                    local line = Instance.new("LineHandleAdornment")
                    line.Parent = otherCharacter
                    line.Adornee = humanoidRootPart
                    line.Color3 = Color3.fromRGB(255, 0, 0) -- Cor da linha
                    line.Thickness = 0.2 -- Espessura da linha
                    line.ZIndex = 10 -- Garante que a linha fique acima do personagem
                    line.Visible = true
                    line.Name = "ESP_Line"
                    otherCharacter:SetAttribute("ESP_Line", line) -- Armazenar a linha no atributo
                end
            end
        end
    else
        -- Desativar o ESP, removendo as linhas
        for _, otherPlayer in pairs(game.Players:GetPlayers()) do
            if otherPlayer ~= player and otherPlayer.Character then
                local otherCharacter = otherPlayer.Character
                local espLine = otherCharacter:GetAttribute("ESP_Line")
                if espLine then
                    espLine:Destroy()
                    otherCharacter:SetAttribute("ESP_Line", nil)
                end
            end
        end
    end
end

-- Função para ativar a modificação das hitboxes
local function modifyHitboxes()
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
end

-- Função para remover cooldown das habilidades
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

-- Função para ativar voo
local flying = false
local bodyVelocity = nil

local function toggleFly()
    if flying then
        -- Desligar o voo
        flying = false
        if bodyVelocity then
            bodyVelocity:Destroy()
            bodyVelocity = nil
        end
        character:FindFirstChild("Humanoid").PlatformStand = false
    else
        -- Ativar o voo
        flying = true
        local humanoid = character:FindFirstChild("Humanoid")
        humanoid.PlatformStand = true -- Desativa a física do personagem para o voo

        -- Criar o BodyVelocity para movimentação enquanto voando
        bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(100000, 100000, 100000)
        bodyVelocity.Velocity = Vector3.new(0, 50, 0) -- Força para o voo
        bodyVelocity.Parent = character:WaitForChild("HumanoidRootPart")
    end
end

-- Função para alterar a vida do jogador para 1000000
local function setHealthToMax()
    local humanoid = character:FindFirstChild("Humanoid")
    if humanoid then
        humanoid.Health = 1000000 -- Define a vida para 1.000.000
    end
end

-- Função para dar 100 de dano em todos os outros jogadores
local function dealDamageToAll()
    for _, otherPlayer in pairs(game.Players:GetPlayers()) do
        if otherPlayer ~= player and otherPlayer.Character then
            local otherCharacter = otherPlayer.Character
            local humanoid = otherCharacter:FindFirstChild("Humanoid")
            if humanoid then
                humanoid:TakeDamage(100) -- Dá 100 de dano
            end
        end
    end
end

-- Criar notificação
local function showNotification()
    game.StarterGui:SetCore("SendNotification", {
        Title = "Aviso";
        Text = "Se inscreva no canal do @pedroanjoyt!";
        Duration = 60; -- Duração da notificação em segundos
    })
end

-- Criar UI com botões
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player:WaitForChild("PlayerGui")

local buttonFrame = Instance.new("Frame")
buttonFrame.Size = UDim2.new(0, 300, 0, 350)
buttonFrame.Position = UDim2.new(0.5, -150, 0.5, -175)
buttonFrame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
buttonFrame.Parent = screenGui

local modifyButton = Instance.new("TextButton")
modifyButton.Size = UDim2.new(0, 250, 0, 50)
modifyButton.Position = UDim2.new(0, 25, 0, 25)
modifyButton.Text = "Ativar Modificação de Hitbox"
modifyButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
modifyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
modifyButton.Parent = buttonFrame
modifyButton.MouseButton1Click:Connect(modifyHitboxes)

local cooldownButton = Instance.new("TextButton")
cooldownButton.Size = UDim2.new(0, 250, 0, 50)
cooldownButton.Position = UDim2.new(0, 25, 0, 85)
cooldownButton.Text = "Remover Cooldown"
cooldownButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
cooldownButton.TextColor3 = Color3.fromRGB(255, 255, 255)
cooldownButton.Parent = buttonFrame
cooldownButton.MouseButton1Click:Connect(function()
    removeCooldown()
    showNotification()
end)

local flyButton = Instance.new("TextButton")
flyButton.Size = UDim2.new(0, 250, 0, 50)
flyButton.Position = UDim2.new(0, 25, 0, 145)
flyButton.Text = "Ativar/Desativar Voo"
flyButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
flyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
flyButton.Parent = buttonFrame
flyButton.MouseButton1Click:Connect(toggleFly)

local espButton = Instance.new("TextButton")
espButton.Size = UDim2.new(0, 250, 0, 50)
espButton.Position = UDim2.new(0, 25, 0, 205)
espButton.Text = "Ativar/Desativar ESP"
espButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
espButton.TextColor3 = Color3.fromRGB(255, 255, 255)
espButton.Parent = buttonFrame
espButton.MouseButton1Click:Connect(toggleESP)

local healthButton = Instance.new("TextButton")
healthButton.Size = UDim2.new(0, 250, 0, 50)
healthButton.Position = UDim2.new(0, 25, 0, 265)
healthButton.Text = "Alterar Vida para 1.000.000"
healthButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
healthButton.TextColor3 = Color3.fromRGB(255, 255, 255)
healthButton.Parent = buttonFrame
healthButton.MouseButton1Click:Connect(setHealthToMax)

local damageButton = Instance.new("TextButton")
damageButton.Size = UDim2.new(0, 250, 0, 50)
damageButton.Position = UDim2.new(0, 25, 0, 325)
damageButton.Text = "Dar 100 de Dano em Todos"
damageButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
damageButton.TextColor3 = Color3.fromRGB(255, 255, 255)
damageButton.Parent = buttonFrame
damageButton.MouseButton1Click:Connect(dealDamageToAll)

-- Mensagem inicial no console
print("Interface carregada. Clique nos botões para ativar as funções.")
