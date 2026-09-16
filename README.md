-- =================================================================
--                     OMINIS HUB - CLEAN EDITION               
--                  Tomate Store & Ominis Framework                
-- Discord: https://discord.gg/q6hYYSSGCz
-- =================================================================

local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local CoreGui = game:GetService("CoreGui")

if CoreGui:FindFirstChild("OminisHubBase") then
    CoreGui.OminisHubBase:Destroy()
end

local Settings = {
    DiscordURL = "https://discord.gg/q6hYYSSGCz",
    KeyValidada = false,
    HubVisivel = true,
    ToggleKey = Enum.KeyCode.N,
    PlanoAtual = "Nenhum"
}

local Theme = {
    BgMain = Color3.fromRGB(12, 10, 18),
    BgHeader = Color3.fromRGB(18, 14, 28),
    BgCard = Color3.fromRGB(22, 18, 34),
    BgCardHover = Color3.fromRGB(32, 26, 48),
    Accent = Color3.fromRGB(138, 43, 226),
    AccentGlow = Color3.fromRGB(175, 82, 255),
    TextPrimary = Color3.fromRGB(245, 240, 255),
    TextSecondary = Color3.fromRGB(140, 130, 160),
    Error = Color3.fromRGB(255, 50, 90),
    Success = Color3.fromRGB(50, 255, 130),
    StrokeColor = Color3.fromRGB(45, 36, 68)
}

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "OminisHubBase"
ScreenGui.Parent = CoreGui
ScreenGui.ResetOnSpawn = false

-- SISTEMA DE NOTIFICAÇÕES
local NotifHolder = Instance.new("Frame")
NotifHolder.Name = "NotifHolder" NotifHolder.Size = UDim2.new(0, 280, 1, -30)
NotifHolder.Position = UDim2.new(1, -290, 0, 15) NotifHolder.BackgroundTransparency = 1 NotifHolder.Parent = ScreenGui

local NotifLayout = Instance.new("UIListLayout")
NotifLayout.VerticalAlignment = Enum.VerticalAlignment.Bottom NotifLayout.Padding = UDim.new(0, 8) NotifLayout.Parent = NotifHolder

local function Notify(title, message, duration)
    duration = duration or 3.5
    local Card = Instance.new("Frame")
    Card.Size = UDim2.new(1, 0, 0, 55) Card.Position = UDim2.new(1, 320, 0, 0)
    Card.BackgroundColor3 = Theme.BgHeader Card.BorderSizePixel = 0 Card.ClipsDescendants = true Card.Parent = NotifHolder

    local Corner = Instance.new("UICorner") Corner.CornerRadius = UDim.new(0, 8) Corner.Parent = Card
    local Stroke = Instance.new("UIStroke") Stroke.Color = Theme.AccentGlow Stroke.Thickness = 1.2 Stroke.Parent = Card

    local TitleLbl = Instance.new("TextLabel")
    TitleLbl.Size = UDim2.new(1, -15, 0, 20) TitleLbl.Position = UDim2.new(0, 12, 0, 6)
    TitleLbl.BackgroundTransparency = 1 TitleLbl.Text = title TitleLbl.TextColor3 = Theme.AccentGlow
    TitleLbl.Font = Enum.Font.GothamBold TitleLbl.TextSize = 13 TitleLbl.TextXAlignment = Enum.TextXAlignment.Left TitleLbl.Parent = Card

    local MsgLbl = Instance.new("TextLabel")
    MsgLbl.Size = UDim2.new(1, -15, 0, 20) MsgLbl.Position = UDim2.new(0, 12, 0, 24)
    MsgLbl.BackgroundTransparency = 1 MsgLbl.Text = message MsgLbl.TextColor3 = Theme.TextPrimary
    MsgLbl.Font = Enum.Font.Gotham MsgLbl.TextSize = 11 MsgLbl.TextXAlignment = Enum.TextXAlignment.Left MsgLbl.Parent = Card

    local ProgressBar = Instance.new("Frame")
    ProgressBar.Size = UDim2.new(1, 0, 0, 3) ProgressBar.Position = UDim2.new(0, 0, 1, -3)
    ProgressBar.BackgroundColor3 = Theme.AccentGlow ProgressBar.BorderSizePixel = 0 ProgressBar.Parent = Card

    TweenService:Create(Card, TweenInfo.new(0.4, Enum.EasingStyle.Quart, Enum.EasingDirection.Out), {Position = UDim2.new(0, 0, 0, 0)}):Play()
    TweenService:Create(ProgressBar, TweenInfo.new(duration, Enum.EasingStyle.Linear), {Size = UDim2.new(0, 0, 0, 3)}):Play()

    task.delay(duration, function()
        local animOut = TweenService:Create(Card, TweenInfo.new(0.4, Enum.EasingStyle.Quart, Enum.EasingDirection.In), {Position = UDim2.new(1, 320, 0, 0)})
        animOut:Play()
        animOut.Completed:Connect(function() Card:Destroy() end)
    end)
end

-- TELA DE KEY (SISTEMA DE LICENÇA)
local KeyFrame = Instance.new("Frame")
KeyFrame.Name = "KeyFrame" KeyFrame.Size = UDim2.new(0, 380, 0, 240)
KeyFrame.Position = UDim2.new(0.5, -190, 0.5, -120) KeyFrame.BackgroundColor3 = Theme.BgMain KeyFrame.Parent = ScreenGui
local KeyCorner = Instance.new("UICorner") KeyCorner.CornerRadius = UDim.new(0, 12) KeyCorner.Parent = KeyFrame
local KeyStroke = Instance.new("UIStroke") KeyStroke.Color = Theme.Accent KeyStroke.Thickness = 1.5 KeyStroke.Parent = KeyFrame

local KeyHeader = Instance.new("TextLabel")
KeyHeader.Size = UDim2.new(1, 0, 0, 45) KeyHeader.BackgroundTransparency = 1
KeyHeader.Text = "TOMATE STORE | KEY SYSTEM" KeyHeader.TextColor3 = Theme.AccentGlow
KeyHeader.Font = Enum.Font.GothamBold KeyHeader.TextSize = 16 KeyHeader.Parent = KeyFrame

local KeySubHeader = Instance.new("TextLabel")
KeySubHeader.Size = UDim2.new(1, 0, 0, 20) KeySubHeader.Position = UDim2.new(0, 0, 0, 38)
KeySubHeader.BackgroundTransparency = 1 KeySubHeader.Text = "Insira sua licença para continuar"
KeySubHeader.TextColor3 = Theme.TextSecondary KeySubHeader.Font = Enum.Font.GothamSemibold KeySubHeader.TextSize = 11 KeySubHeader.Parent = KeyFrame

local KeyBox = Instance.new("TextBox")
KeyBox.Size = UDim2.new(0.86, 0, 0, 40) KeyBox.Position = UDim2.new(0.07, 0, 0.38, 0)
KeyBox.BackgroundColor3 = Theme.BgCard KeyBox.Text = "" KeyBox.PlaceholderText = "Cole sua Key aqui..."
KeyBox.TextColor3 = Theme.TextPrimary KeyBox.PlaceholderColor3 = Theme.TextSecondary KeyBox.Font = Enum.Font.GothamSemibold KeyBox.TextSize = 12 KeyBox.Parent = KeyFrame
local KeyBoxCorner = Instance.new("UICorner") KeyBoxCorner.CornerRadius = UDim.new(0, 8) KeyBoxCorner.Parent = KeyBox
local KeyBoxStroke = Instance.new("UIStroke") KeyBoxStroke.Color = Theme.StrokeColor KeyBoxStroke.Parent = KeyBox

local VerifyBtn = Instance.new("TextButton")
VerifyBtn.Size = UDim2.new(0.41, 0, 0, 38) VerifyBtn.Position = UDim2.new(0.07, 0, 0.68, 0)
VerifyBtn.BackgroundColor3 = Theme.Accent VerifyBtn.Text = "ENTRAR"
VerifyBtn.TextColor3 = Theme.TextPrimary VerifyBtn.Font = Enum.Font.GothamBold VerifyBtn.TextSize = 12 VerifyBtn.Parent = KeyFrame
local VerifyCorner = Instance.new("UICorner") VerifyCorner.CornerRadius = UDim.new(0, 8) VerifyCorner.Parent = VerifyBtn

local GetKeyBtn = Instance.new("TextButton")
GetKeyBtn.Size = UDim2.new(0.41, 0, 0, 38) GetKeyBtn.Position = UDim2.new(0.52, 0, 0.68, 0)
GetKeyBtn.BackgroundColor3 = Theme.BgCard GetKeyBtn.Text = "PEGAR KEY"
GetKeyBtn.TextColor3 = Theme.TextSecondary GetKeyBtn.Font = Enum.Font.GothamBold GetKeyBtn.TextSize = 11 GetKeyBtn.Parent = KeyFrame
local GetKeyCorner = Instance.new("UICorner") GetKeyCorner.CornerRadius = UDim.new(0, 8) GetKeyCorner.Parent = GetKeyBtn
local GetKeyStroke = Instance.new("UIStroke") GetKeyStroke.Color = Theme.StrokeColor GetKeyStroke.Parent = GetKeyBtn

GetKeyBtn.MouseButton1Click:Connect(function()
    setclipboard(Settings.DiscordURL)
    Notify("Tomate Store", "Link do Discord copiado!", 3)
end)

-- PAINEL PRINCIPAL (LIMPO)
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame" MainFrame.Size = UDim2.new(0, 550, 0, 350)
MainFrame.Position = UDim2.new(0.5, -275, 0.5, -175) MainFrame.BackgroundColor3 = Theme.BgMain
MainFrame.ClipsDescendants = true MainFrame.Visible = false MainFrame.Parent = ScreenGui

local MainCorner = Instance.new("UICorner") MainCorner.CornerRadius = UDim.new(0, 12) MainCorner.Parent = MainFrame
local MainStroke = Instance.new("UIStroke") MainStroke.Color = Theme.Accent MainStroke.Thickness = 1.5 MainStroke.Parent = MainFrame

-- BARRA SUPERIOR
local TopBar = Instance.new("Frame")
TopBar.Name = "TopBar" TopBar.Size = UDim2.new(1, 0, 0, 45)
TopBar.BackgroundColor3 = Theme.BgHeader TopBar.BorderSizePixel = 0 TopBar.Parent = MainFrame

local TitleLbl = Instance.new("TextLabel")
TitleLbl.Size = UDim2.new(0, 220, 1, 0) TitleLbl.Position = UDim2.new(0, 15, 0, 0)
TitleLbl.BackgroundTransparency = 1 TitleLbl.Text = "OMINIS HUB | TOMATE STORE"
TitleLbl.TextColor3 = Theme.AccentGlow TitleLbl.Font = Enum.Font.GothamBold TitleLbl.TextSize = 13 TitleLbl.TextXAlignment = Enum.TextXAlignment.Left TitleLbl.Parent = TopBar

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 30, 0, 30) CloseBtn.Position = UDim2.new(1, -35, 0, 7)
CloseBtn.BackgroundTransparency = 1 CloseBtn.Text = "✕" CloseBtn.TextColor3 = Theme.TextSecondary
CloseBtn.Font = Enum.Font.GothamBold CloseBtn.TextSize = 14 CloseBtn.Parent = TopBar
CloseBtn.MouseButton1Click:Connect(function() ScreenGui:Destroy() end)

-- ÁREA DE STATUS DO USUÁRIO
local StatusCard = Instance.new("Frame")
StatusCard.Size = UDim2.new(0.92, 0, 0, 60) StatusCard.Position = UDim2.new(0.04, 0, 0.18, 0)
StatusCard.BackgroundColor3 = Theme.BgCard StatusCard.Parent = MainFrame
local StatusCorner = Instance.new("UICorner") StatusCorner.CornerRadius = UDim.new(0, 8) StatusCorner.Parent = StatusCard
local StatusStroke = Instance.new("UIStroke") StatusStroke.Color = Theme.StrokeColor StatusStroke.Parent = StatusCard

local StatusTitle = Instance.new("TextLabel")
StatusTitle.Size = UDim2.new(1, -20, 0, 22) StatusTitle.Position = UDim2.new(0, 12, 0, 8)
StatusTitle.BackgroundTransparency = 1 StatusTitle.Text = "STATUS DA LICENÇA"
StatusTitle.TextColor3 = Theme.TextSecondary StatusTitle.Font = Enum.Font.GothamBold StatusTitle.TextSize = 10 StatusTitle.TextXAlignment = Enum.TextXAlignment.Left StatusTitle.Parent = StatusCard

local PlanoLbl = Instance.new("TextLabel")
PlanoLbl.Name = "PlanoLbl" PlanoLbl.Size = UDim2.new(1, -20, 0, 22) PlanoLbl.Position = UDim2.new(0, 12, 0, 28)
PlanoLbl.BackgroundTransparency = 1 PlanoLbl.Text = "Plano: Carregando..."
PlanoLbl.TextColor3 = Theme.Success PlanoLbl.Font = Enum.Font.GothamBold PlanoLbl.TextSize = 13 PlanoLbl.TextXAlignment = Enum.TextXAlignment.Left PlanoLbl.Parent = StatusCard

-- CONTÊINER PARA ADICIONAR SEUS SCRIPTS FUTUROS
local ContentArea = Instance.new("Frame")
ContentArea.Name = "ContentArea" ContentArea.Size = UDim2.new(0.92, 0, 0.58, 0)
ContentArea.Position = UDim2.new(0.04, 0, 0.38, 0) ContentArea.BackgroundColor3 = Theme.BgCard ContentArea.Parent = MainFrame
local ContentCorner = Instance.new("UICorner") ContentCorner.CornerRadius = UDim.new(0, 8) ContentCorner.Parent = ContentArea
local ContentStroke = Instance.new("UIStroke") ContentStroke.Color = Theme.StrokeColor ContentStroke.Parent = ContentArea

local InfoPlaceholder = Instance.new("TextLabel")
InfoPlaceholder.Size = UDim2.new(1, -20, 1, 0) InfoPlaceholder.Position = UDim2.new(0, 10, 0, 0)
InfoPlaceholder.BackgroundTransparency = 1 InfoPlaceholder.Text = "Espaço reservado para as suas funções.\nAdicione seus botões e scripts dentro de 'ContentArea'."
InfoPlaceholder.TextColor3 = Theme.TextSecondary InfoPlaceholder.Font = Enum.Font.GothamSemibold InfoPlaceholder.TextSize = 12 InfoPlaceholder.Parent = ContentArea

-- VALIDADOR DE KEY E IDENTIFICADOR DE PLANO
local function ValidarEntrada(inputKey)
    local key = string.upper(inputKey)
    
    if key == "NOG" then
        Settings.PlanoAtual = "Owner / Developer (Acesso Total)"
        return true
    elseif string.find(key, "OMINIS-DAILY-") then
        Settings.PlanoAtual = "Plano Diário (24 Horas)"
        return true
    elseif string.find(key, "OMINIS-WEEKLY-") then
        Settings.PlanoAtual = "Plano Semanal (7 Dias)"
        return true
    elseif string.find(key, "OMINIS-MONTHLY-") then
        Settings.PlanoAtual = "Plano Mensal (30 Dias)"
        return true
    elseif string.find(key, "OMINIS-LIFE-") then
        Settings.PlanoAtual = "Plano Lifetime (Vitalício)"
        return true
    end
    
    return false
end

VerifyBtn.MouseButton1Click:Connect(function()
    if ValidarEntrada(KeyBox.Text) then
        Settings.KeyValidada = true
        PlanoLbl.Text = "Plano Ativo: " .. Settings.PlanoAtual
        
        TweenService:Create(KeyFrame, TweenInfo.new(0.35, Enum.EasingStyle.Quart, Enum.EasingDirection.In), {Position = UDim2.new(0.5, -190, 0, -300)}):Play()
        task.wait(0.35)
        KeyFrame:Destroy()
        MainFrame.Visible = true
        Notify("Tomate Store", "Licença aprovada!", 4)
    else
        KeyBox.Text = "" KeyBox.PlaceholderText = "Key Inválida! Adquira no Discord."
        TweenService:Create(KeyBoxStroke, TweenInfo.new(0.2), {Color = Theme.Error}):Play()
        task.wait(1)
        TweenService:Create(KeyBoxStroke, TweenInfo.new(0.2), {Color = Theme.StrokeColor}):Play()
    end
end)

-- ATALHO DE TECLA (MINIMIZAR COM N)
UserInputService.InputBegan:Connect(function(input, gpe)
    if not gpe and Settings.KeyValidada and input.KeyCode == Settings.ToggleKey then
        Settings.HubVisivel = not Settings.HubVisivel
        MainFrame.Visible = Settings.HubVisivel
    end
end)
