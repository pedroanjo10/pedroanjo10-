<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Script para The T-Titans Battlegrounds</title>
    <style>
        body {
            background-color: cyan;
            text-align: center;
            font-family: Arial, sans-serif;
        }
        h1 {
            color: white;
        }
        button {
            background-color: white;
            color: black;
            padding: 15px 30px;
            font-size: 18px;
            border: none;
            cursor: pointer;
            margin-top: 20px;
        }
        button:hover {
            background-color: gray;
            color: white;
        }
    </style>
</head>
<body>
    <h1>Baixe o Script para The T-Titans Battlegrounds</h1>
    <button onclick="downloadScript()">Baixar Script</button>

    <script>
        function downloadScript() {
            const scriptContent = `local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Aumentar a hitbox (corpo do jogador)
for _, v in pairs(character:GetChildren()) do
    if v:IsA("BasePart") then
        v.Size = Vector3.new(100, 100, 100)
        v.Transparency = 0.7 -- Deixa semi-transparente para evitar suspeitas
    end
end

-- Remover tempo de recarga das habilidades
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

print("Script ativado! Hitbox definida para 100, cooldown removido e notificação exibida por 60 segundos.")`;

            const blob = new Blob([scriptContent], { type: "text/plain" });
            const a = document.createElement("a");
            a.href = URL.createObjectURL(blob);
            a.download = "roblox_script.lua";
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
        }
    </script>
</body>
</html>
