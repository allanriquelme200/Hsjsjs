-- Carregar a biblioteca Fluent (interface)
local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/raw/main/Fluent.lua"))()

-- Criar janela
local Window = Fluent.CreateWindow({
    Title = "DK Aimbot Mobile",
    Size = UDim2.fromOffset(580, 460),
    Theme = "Dark"
})

-- Abas
local Tabs = {
    Main = Window:AddTab({ Title = "Aimbot" }),
}

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")

-- Função para encontrar o jogador mais próximo
function GetClosestPlayer()
    local Closest
    local ShortestDistance = math.huge

    for _, Player in ipairs(Players:GetPlayers()) do
        if Player ~= LocalPlayer and Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
            local Distance = (Player.Character.HumanoidRootPart.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
            if Distance < ShortestDistance then
                Closest = Player
                ShortestDistance = Distance
            end
        end
    end

    return Closest
end

-- Atualiza a mira a cada frame
RunService.RenderStepped:Connect(function()
    if not LocalPlayer.Character or not LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then return end

    local Target = GetClosestPlayer()

    if Target and Target.Character and Target.Character:FindFirstChild("HumanoidRootPart") then
        local Root = LocalPlayer.Character.HumanoidRootPart
        local TargetPos = Target.Character.HumanoidRootPart.Position

        -- Rotaciona a câmera em direção ao alvo
        workspace.CurrentCamera.CFrame = CFrame.lookAt(
            workspace.CurrentCamera.CFrame.Position,
            TargetPos
        )
    end
end)
