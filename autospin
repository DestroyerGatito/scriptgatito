local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local LocalPlayer = Players.LocalPlayer

-- ################ CONFIGURAÇÃO ################

-- Caminho do ModuleScript "Net" para obter o RemoteEvent.
local NET_MODULE = ReplicatedStorage:WaitForChild("Packages"):WaitForChild("Net") 

-- Caminho do ModuleScript "Synchronizer" para ler o Player Data.
local SYNCHRONIZER_MODULE = ReplicatedStorage:WaitForChild("Packages"):WaitForChild("Synchronizer") 

-- Nome do RemoteEvent de giro corrigido:
local REMOTE_EVENT_NAME = "RadioactiveEventService/Spin" 

-- Chaves de dados corrigidas para ler o status de giros:
local LAST_CLAIMED_KEY = "RadioactiveSpinWheel.LastFreeClaimed"
local BOUGHT_SPINS_KEY = "RadioactiveSpinWheel.Spins"

-- Nome do Atributo no ReplicatedStorage para verificar o evento ativo:
local EVENT_ATTRIBUTE_NAME = "RadioactiveEvent"
local EVENT_LAST_TIME_ATTRIBUTE = "RadioactiveEventLastTime"

-- Intervalo de tempo (em segundos) para verificar se o giro está disponível.
local CHECK_INTERVAL = 5 

-- ##############################################

local NetModule = require(NET_MODULE)
local SynchronizerModule = require(SYNCHRONIZER_MODULE)
local SpinRemoteEvent = NetModule:RemoteEvent(REMOTE_EVENT_NAME)
local PlayerData = SynchronizerModule:Wait(LocalPlayer)
local REPLICATED_STORAGE = ReplicatedStorage

if not SpinRemoteEvent or not SpinRemoteEvent:IsA("RemoteEvent") then
    warn("🚨 Falha ao obter o RemoteEvent. Verifique se o módulo 'Net' e o nome do evento estão corretos.")
    return
end

print("✅ Automação inteligente da roleta 'Radioactive' iniciada.")

-- Função principal de verificação e giro
local function CheckAndSpin()
    local freeSpinsCount = 0
    
    -- 1. Verifica se há giro GRATUITO disponível
    local isEventActive = REPLICATED_STORAGE:GetAttribute(EVENT_ATTRIBUTE_NAME)
    
    if isEventActive then
        local lastClaimed = PlayerData:Get(LAST_CLAIMED_KEY)
        local eventLastTime = REPLICATED_STORAGE:GetAttribute(EVENT_LAST_TIME_ATTRIBUTE)
        
        -- Se o último giro gratuito foi antes do último horário do evento (ou seja, está liberado)
        if lastClaimed ~= eventLastTime then
            freeSpinsCount = 1
        end
    end
    
    -- 2. Obtém Giros Comprados
    local boughtSpins = PlayerData:Get(BOUGHT_SPINS_KEY) or 0
    
    -- 3. Calcula o total de giros disponíveis
    local totalSpins = freeSpinsCount + boughtSpins
    
    if totalSpins > 0 then
        print("🎉 Giro 'Radioactive' disponível detectado! Total:", totalSpins)
        -- Tenta girar a roleta
        SpinRemoteEvent:FireServer() 
        print("🔥 Disparado um giro. Esperando", CHECK_INTERVAL, "segundos para a próxima verificação.")
    else
        -- O console fica mais limpo se não spammar a mensagem de "não disponível"
        -- print("😴 Nenhum giro disponível. Verificando novamente em", CHECK_INTERVAL, "segundos.")
    end
end

-- Loop de rotação automática
while true do
    -- Verifica a cada 5 segundos
    task.wait(CHECK_INTERVAL) 
    
    if LocalPlayer and LocalPlayer.Parent then
        CheckAndSpin()
    else
        break
    end
end
