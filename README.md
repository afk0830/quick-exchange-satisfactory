    loadstring([[
    -- PROTEÇÃO ANTI-DUPLICAÇÃO
    if getgenv().Spam12Loaded then return end
    getgenv().Spam12Loaded = true

    -- CONFIG
    local DELAY = 0.05

    -- SERVIÇOS
    local Players = game:GetService("Players")
    local ContextActionService = game:GetService("ContextActionService")
    local VirtualInputManager = game:GetService("VirtualInputManager")
    local RunService = game:GetService("RunService")
    local TweenService = game:GetService("TweenService")

    local player = Players.LocalPlayer
    local PlayerGui = player:WaitForChild("PlayerGui")

    local ENABLED = false
    local PANEL_OPEN = true

    -- FUNÇÃO PARA SIMULAR TECLAS
    local function pressKey(keycode)
    VirtualInputManager:SendKeyEvent(true, keycode, false, game)
    task.wait(0.01)
    VirtualInputManager:SendKeyEvent(false, keycode, false, game)
    end
   
    -- CRIAR PAINEL
    local function createPanel()
    if PlayerGui:FindFirstChild("SpamPanel") then return PlayerGui.SpamPanel end
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "SpamPanel"
    ScreenGui.ResetOnSpawn = false
    ScreenGui.Parent = PlayerGui

    -- FRAME PRINCIPAL
    local Frame = Instance.new("Frame")
    Frame.Size = UDim2.new(0,220,0,120)
    Frame.Position = UDim2.new(0.5,-110,0.1,0)
    Frame.BackgroundColor3 = Color3.fromRGB(40,40,40)
    Frame.BorderSizePixel = 0
    Frame.Parent = ScreenGui

    -- ARRASTAR PAINEL
    local dragging, dragInput, dragStart, startPos
    local function update(input)
        local delta = input.Position - dragStart
        Frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
    Frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = Frame.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)
    Frame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement then
            dragInput = input
        end
    end)
    RunService.RenderStepped:Connect(function()
        if dragging and dragInput then
            update(dragInput)
        end
    end)

    -- QUADRADO MINIMIZADO
    local MiniFrame = Instance.new("Frame")
    MiniFrame.Size = UDim2.new(0,60,0,60)
    MiniFrame.Position = UDim2.new(0,10,0.1,0)
    MiniFrame.BackgroundColor3 = Color3.fromRGB(50,50,50)
    MiniFrame.Visible = false
    MiniFrame.BorderSizePixel = 3
    MiniFrame.Parent = ScreenGui

    -- TEXTO DENTRO DO QUADRADO
    local MiniLabel = Instance.new("TextLabel")
    MiniLabel.Size = UDim2.new(1,0,1,0)
    MiniLabel.BackgroundTransparency = 1
    MiniLabel.Text = "by afk0830"
    MiniLabel.Font = Enum.Font.GothamBold
    MiniLabel.TextScaled = true
    MiniLabel.Parent = MiniFrame

    -- PALETAS RAINBOW
    local RainbowColors = {
        Color3.fromRGB(255,0,0),
        Color3.fromRGB(255,127,0),
        Color3.fromRGB(255,255,0),
        Color3.fromRGB(0,255,0),
        Color3.fromRGB(0,0,255),
        Color3.fromRGB(75,0,130),
        Color3.fromRGB(148,0,211)
    }

    -- ANIMAÇÃO RAINBOW + PULSO + TEXTO
    local counter = 0
    RunService.RenderStepped:Connect(function()
        counter = counter + 1
        -- Borda rainbow
        MiniFrame.BorderColor3 = RainbowColors[(math.floor(counter/5) % #RainbowColors)+1]
        -- Pulso do quadrado
        local scale = 1 + 0.07*math.sin(counter/10)
        MiniFrame.Size = UDim2.new(0,60*scale,0,60*scale)
        -- Texto rainbow
        MiniLabel.TextColor3 = RainbowColors[(math.floor(counter/3) % #RainbowColors)+1]
    end)

    -- TEXTO CHAMATIVO NO PAINEL
    local LabelBy = Instance.new("TextLabel")
    LabelBy.Size = UDim2.new(1,0,0.2,0)
    LabelBy.Position = UDim2.new(0,0,0.8,0)
    LabelBy.Text = "by afk0830"
    LabelBy.TextColor3 = Color3.fromRGB(255,100,0)
    LabelBy.Font = Enum.Font.GothamBold
    LabelBy.TextScaled = true
    LabelBy.BackgroundTransparency = 1
    LabelBy.Parent = Frame

    -- CONTEÚDO INTERNO
    local Content = Instance.new("Frame")
    Content.Size = UDim2.new(1,0,0.75,0)
    Content.Position = UDim2.new(0,0,0,0)
    Content.BackgroundTransparency = 1
    Content.Parent = Frame

    -- STATUS
    local StatusLabel = Instance.new("TextLabel")
    StatusLabel.Size = UDim2.new(1,0,0.5,0)
    StatusLabel.BackgroundTransparency = 1
    StatusLabel.Text = "OFF"
    StatusLabel.TextColor3 = Color3.fromRGB(255,0,0)
    StatusLabel.Font = Enum.Font.SourceSansBold
    StatusLabel.TextScaled = true
    StatusLabel.Parent = Content

    -- TOGGLE BUTTON
    local ToggleButton = Instance.new("TextButton")
    ToggleButton.Size = UDim2.new(1,0,0.25,0)
    ToggleButton.Position = UDim2.new(0,0,0.5,0)
    ToggleButton.BackgroundColor3 = Color3.fromRGB(70,70,70)
    ToggleButton.Text = "TOGGLE"
    ToggleButton.TextColor3 = Color3.fromRGB(255,255,255)
    ToggleButton.Font = Enum.Font.SourceSansBold
    ToggleButton.TextScaled = true
    ToggleButton.Parent = Content

    -- MINIMIZAR
    local MinButton = Instance.new("TextButton")
    MinButton.Size = UDim2.new(0.5,0,0.25,0)
    MinButton.Position = UDim2.new(0,0,0.75,0)
    MinButton.BackgroundColor3 = Color3.fromRGB(100,100,100)
    MinButton.Text = "_"
    MinButton.TextColor3 = Color3.fromRGB(255,255,255)
    MinButton.Font = Enum.Font.SourceSansBold
    MinButton.TextScaled = true
    MinButton.Parent = Frame

    -- FECHAR
    local CloseButton = Instance.new("TextButton")
    CloseButton.Size = UDim2.new(0.5,0,0.25,0)
    CloseButton.Position = UDim2.new(0.5,0,0.75,0)
    CloseButton.BackgroundColor3 = Color3.fromRGB(200,50,50)
    CloseButton.Text = "X"
    CloseButton.TextColor3 = Color3.fromRGB(255,255,255)
    CloseButton.Font = Enum.Font.SourceSansBold
    CloseButton.TextScaled = true
    CloseButton.Parent = Frame

    -- ATUALIZA UI
    local function updateUI()
        if ENABLED then
            StatusLabel.Text = "ON"
            StatusLabel.TextColor3 = Color3.fromRGB(0,255,0)
        else
            StatusLabel.Text = "OFF"
            StatusLabel.TextColor3 = Color3.fromRGB(255,0,0)
        end
    end

    -- TOGGLE CLICK
    ToggleButton.MouseButton1Click:Connect(function()
        ENABLED = not ENABLED
        updateUI()
    end)

    -- MINIMIZAR CLICK
    MinButton.MouseButton1Click:Connect(function()
        PANEL_OPEN = false
        Frame.Visible = false
        MiniFrame.Visible = true
    end)

    -- CLIQUE NO QUADRADO
    MiniFrame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            PANEL_OPEN = true
            Frame.Visible = true
            MiniFrame.Visible = false
        end
    end)

    -- FECHAR CLICK
    CloseButton.MouseButton1Click:Connect(function()
        Frame:Destroy()
        MiniFrame:Destroy()
        getgenv().Spam12Loaded = false
        ENABLED = false
    end)

    return Frame, updateUI
    end

    local Frame, updateUI = createPanel()

    -- LOOP SPAM
    task.spawn(function()
    while true do
        if ENABLED then
            pressKey(Enum.KeyCode.One)
            pressKey(Enum.KeyCode.Two)
        end
        task.wait(DELAY)
    end
    end)

    -- TOGGLE COM TECLA E
    ContextActionService:BindAction(
    "ToggleSpam12",
    function(_, state)
        if state == Enum.UserInputState.Begin then
            ENABLED = not ENABLED
            updateUI()
        end
    end,
    false,
    Enum.KeyCode.E
    )
    ]])()
