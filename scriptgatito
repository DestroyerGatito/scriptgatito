local ProximityPromptService = game:GetService("ProximityPromptService")
local Player = game.Players.LocalPlayer

-- Função para simular a compra/interação
local function autoTrigger(promptObject, player)
    -- **ATENÇÃO:** Nesta linha estaria a lógica de compra real do jogo.
    -- Em um exploit, você tentaria encontrar e disparar o RemoteEvent
    -- que o jogo usa para processar a compra no servidor,
    -- passando os parâmetros necessários.
    
    -- Exemplo de uso do Triggered, que é o evento normal:
    -- promptObject.Triggered:Fire(player) -- Não funciona assim no cliente, mas ilustra o conceito.
    
    print("ProximityPrompt detectado e ativado: " .. promptObject.Parent.Name)
end

-- Itera sobre todos os objetos no jogo para encontrar prompts existentes
for _, prompt in pairs(workspace:GetDescendants()) do
    if prompt:IsA("ProximityPrompt") then
        -- Desativa o HoldDuration no CLIENTE (pode ser ignorado pelo servidor)
        prompt.HoldDuration = 0
        
        -- Conecta a função ao evento para ativação instantânea no cliente
        prompt.Triggered:Connect(autoTrigger)
    end
end

-- Monitora novos prompts (exemplo simplificado)
workspace.DescendantAdded:Connect(function(newObject)
    if newObject:IsA("ProximityPrompt") then
        newObject.HoldDuration = 0
        newObject.Triggered:Connect(autoTrigger)
    end
end)
