# 📚 MacLib — Documentação Completa

> UI Library para Roblox com suporte a blur acrílico, sistema de configs, tabs, e muito mais.

---

## 📦 Instalação

```lua
local MacLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/NickolasFrutuoso/MacUI/refs/heads/main/UI"))()
```

---

## 🚀 Uso Rápido com CreateHUB

O método `CreateHUB` cria uma janela completa já com as abas **Settings** e **Player** prontas, sistema de configs, keybind de menu, e muito mais.

```lua
local HUB = MacLib:CreateHUB({
    Title    = "Noliar HUB",
    Subtitle = game:GetService("MarketplaceService"):GetProductInfo(game.PlaceId).Name,
    OnTabGroup = function(TabGroup, Window)
        -- Adicione seus tabs customizados aqui
        local CombatTab = TabGroup:Tab({ Name = "Combat", Image = "rbxassetid://XXXXX" })
    end,
})

-- Referências retornadas pelo HUB:
local Window          = HUB.Window
local TabGroup        = HUB.TabGroup
local SettingsTab     = HUB.SettingsTab
local PlayerTab       = HUB.PlayerTab
local FeatureRegistry = HUB.FeatureRegistry
```

### Parâmetros do CreateHUB

| Parâmetro    | Tipo       | Descrição                                              |
|--------------|------------|--------------------------------------------------------|
| `Title`      | `string`   | Título exibido no canto superior esquerdo              |
| `Subtitle`   | `string`   | Subtítulo abaixo do título (ex: nome do jogo)          |
| `Size`       | `UDim2`    | Tamanho inicial da janela (padrão: 868x650)            |
| `Keybind`    | `KeyCode`  | Tecla para abrir/fechar o menu (padrão: RightControl)  |
| `OnTabGroup` | `function` | Callback para adicionar tabs customizados acima do divider |

---

## 🪟 MacLib:Window()

Cria uma janela manualmente com controle total.

```lua
local Window = MacLib:Window({
    Title                  = "Meu HUB",
    Subtitle               = "v1.0",
    Size                   = UDim2.fromOffset(868, 650),
    DragStyle              = 1,       -- 1 = arrastar pelo ícone | 2 = arrastar pela janela inteira
    AcrylicBlur            = true,    -- Ativa o blur acrílico de fundo
    Keybind                = Enum.KeyCode.RightControl,
    ShowUserInfo           = true,    -- Exibe foto/nome do jogador no canto inferior
    DisabledWindowControls = {},      -- Lista de controles desabilitados: "Exit", "Minimize"
})
```

### Métodos da Window

| Método | Descrição |
|--------|-----------|
| `Window:UpdateTitle(str)` | Altera o título da janela |
| `Window:UpdateSubtitle(str)` | Altera o subtítulo |
| `Window:SetState(bool)` | Mostra ou esconde a janela |
| `Window:GetState()` | Retorna se a janela está visível |
| `Window:SetKeybind(KeyCode)` | Muda a tecla de toggle |
| `Window:SetAcrylicBlurState(bool)` | Ativa/desativa o blur acrílico |
| `Window:GetAcrylicBlurState()` | Retorna o estado do blur |
| `Window:SetUserInfoState(bool)` | Mostra/oculta as informações do usuário |
| `Window:SetSize(UDim2)` | Redefine o tamanho da janela |
| `Window:GetSize()` | Retorna o tamanho atual |
| `Window:SetScale(number)` | Escala toda a UI (ex: 0.8 = 80%) |
| `Window:GetScale()` | Retorna a escala atual |
| `Window:SetNotificationsState(bool)` | Ativa/desativa notificações |
| `Window:Unload()` | Destroi toda a UI |
| `Window.onUnloaded(callback)` | Callback executado ao fechar/descarregar |

---

## 🔔 Window:Notify()

Exibe uma notificação no canto inferior direito.

```lua
Window:Notify({
    Title       = "Aviso",
    Description = "Isso é uma notificação.",
    Lifetime    = 3,       -- Duração em segundos (0 = permanente)
    Style       = "None",  -- "None" | "Confirm" | "Cancel"
    SizeX       = 250,     -- Largura da notificação
    Scale       = 1,       -- Escala da notificação
    Callback    = function() -- Executado ao clicar no botão (Confirm/Cancel)
        print("Clicou!")
    end,
})
```

### Métodos da Notificação

| Método | Descrição |
|--------|-----------|
| `notif:UpdateTitle(str)` | Altera o título |
| `notif:UpdateDescription(str)` | Altera a descrição |
| `notif:Resize(number)` | Altera a largura |
| `notif:Cancel()` | Fecha a notificação manualmente |

---

## 💬 Window:Dialog()

Exibe uma janela de diálogo modal com botões customizáveis.

```lua
Window:Dialog({
    Title       = "Confirmação",
    Description = "Tem certeza que deseja sair?",
    Buttons = {
        {
            Name = "Confirmar",
            Callback = function()
                Window:Unload()
            end,
        },
        {
            Name = "Cancelar",
            -- sem Callback = fecha o dialog sem fazer nada
        },
    },
})
```

### Métodos do Dialog

| Método | Descrição |
|--------|-----------|
| `dialog:UpdateTitle(str)` | Altera o título |
| `dialog:UpdateDescription(str)` | Altera a descrição |
| `dialog:Cancel()` | Fecha o dialog manualmente |

---

## ⚙️ Window:GlobalSetting()

Adiciona um item no menu global (ícone de globo no topo da sidebar).

```lua
Window:GlobalSetting({
    Name     = "Opção Global",
    Default  = false,
    Callback = function(state)
        print("Global setting:", state)
    end,
})
```

### Métodos do GlobalSetting

| Método | Descrição |
|--------|-----------|
| `gs:UpdateName(str)` | Altera o nome exibido |
| `gs:UpdateState(bool)` | Define o estado (ligado/desligado) |

---

## 📂 Window:TabGroup()

Cria um grupo de tabs na sidebar.

```lua
local TabGroup = Window:TabGroup()
```

### TabGroup:Tab()

Cria uma aba dentro do grupo.

```lua
local MeuTab = TabGroup:Tab({
    Name  = "Combat",
    Image = "rbxassetid://XXXXX", -- ícone da aba (opcional)
})
```

### TabGroup:Divider()

Adiciona uma linha divisória entre tabs na sidebar.

```lua
TabGroup:Divider()
```

### Métodos do Tab

| Método | Descrição |
|--------|-----------|
| `Tab:Select()` | Seleciona/ativa esse tab |
| `Tab:Section(Settings)` | Cria uma seção dentro do tab |

---

## 📋 Tab:Section()

Cria uma seção (card) dentro de um tab.

```lua
local Secao = MeuTab:Section({
    Side = "Left",  -- "Left" ou "Right" (coluna da seção)
})
```

---

## 🧩 Elementos de uma Section

### 🔘 Toggle

```lua
local toggle = Secao:Toggle({
    Name     = "Meu Toggle",
    Default  = false,
    Callback = function(state)
        print("Toggle:", state)
    end,
}, "FlagDoToggle") -- Flag é opcional, registra em MacLib.Options
```

| Método | Descrição |
|--------|-----------|
| `toggle:UpdateState(bool)` | Define o estado |
| `toggle:GetState()` | Retorna o estado atual |
| `toggle:UpdateName(str)` | Altera o nome |
| `toggle:SetVisibility(bool)` | Mostra/oculta o elemento |
| `toggle:Toggle()` | Inverte o estado atual |

---

### 🎚️ Slider

```lua
local slider = Secao:Slider({
    Name          = "Velocidade",
    Minimum       = 0,
    Maximum       = 100,
    Default       = 16,
    Precision     = 0,           -- casas decimais (0 = inteiro)
    DisplayMethod = "Value",     -- "Value" | "Percent" | "Degrees" | "Round" | "Tenths" | "Hundredths"
    Prefix        = "",          -- texto antes do valor (ex: "$")
    Suffix        = "",          -- texto após o valor (ex: "km/h")
    Callback      = function(value)
        print("Slider:", value)
    end,
    onInputComplete = function(value) -- chamado ao soltar o slider
        print("Final:", value)
    end,
}, "FlagDoSlider")
```

| Método | Descrição |
|--------|-----------|
| `slider:UpdateValue(number)` | Define o valor sem chamar callback |
| `slider:GetValue()` | Retorna o valor atual |
| `slider:UpdateName(str)` | Altera o nome |
| `slider:SetVisibility(bool)` | Mostra/oculta o elemento |

---

### 🖊️ Input (Caixa de texto)

```lua
local input = Secao:Input({
    Name               = "Nome",
    Default            = "",
    Placeholder        = "Digite aqui...",
    AcceptedCharacters = "All", -- "All" | "Numeric" | "Alphabetic" | "AlphaNumeric" | function(str)
    CharacterLimit     = 20,    -- limite de caracteres (opcional)
    Callback           = function(text)  -- chamado ao perder foco
        print("Input:", text)
    end,
    onChanged          = function(text)  -- chamado a cada tecla
        print("Mudou:", text)
    end,
}, "FlagDoInput")
```

| Método | Descrição |
|--------|-----------|
| `input:GetInput()` | Retorna o texto atual |
| `input:UpdateText(str)` | Define o texto |
| `input:UpdatePlaceholder(str)` | Altera o placeholder |
| `input:UpdateName(str)` | Altera o nome |
| `input:SetVisibility(bool)` | Mostra/oculta o elemento |

---

### ⌨️ Keybind

```lua
local keybind = Secao:Keybind({
    Name      = "Ativar",
    Default   = Enum.KeyCode.F,
    Blacklist = {},  -- lista de KeyCodes proibidos (opcional)
    Callback  = function(bind)  -- chamado ao pressionar a tecla bindada
        print("Tecla pressionada:", bind)
    end,
    onBinded  = function(bind)  -- chamado ao definir um novo bind
        print("Novo bind:", bind)
    end,
    onBindHeld = function(holding, bind)  -- chamado ao segurar/soltar
        print("Segurando:", holding)
    end,
}, "FlagDoKeybind")
```

| Método | Descrição |
|--------|-----------|
| `keybind:Bind(KeyCode)` | Define o bind programaticamente |
| `keybind:Unbind()` | Remove o bind |
| `keybind:GetBind()` | Retorna o bind atual |
| `keybind:UpdateName(str)` | Altera o nome |
| `keybind:SetVisibility(bool)` | Mostra/oculta o elemento |

---

### 🔽 Dropdown

```lua
local dropdown = Secao:Dropdown({
    Name     = "Modo",
    Options  = {"Opção 1", "Opção 2", "Opção 3"},
    Default  = 1,       -- índice padrão (single) ou tabela (multi)
    Multi    = false,   -- true = seleção múltipla
    Required = false,   -- true = sempre deve ter ao menos 1 selecionado
    Search   = false,   -- true = exibe campo de busca
    Callback = function(value)
        print("Selecionado:", value)
    end,
}, "FlagDoDropdown")
```

| Método | Descrição |
|--------|-----------|
| `dropdown:UpdateSelection(value)` | Define a seleção (string, número ou tabela) |
| `dropdown:InsertOptions(table)` | Adiciona novas opções |
| `dropdown:ClearOptions()` | Remove todas as opções |
| `dropdown:RemoveOptions(table)` | Remove opções específicas |
| `dropdown:GetOptions()` | Retorna tabela com status de cada opção |
| `dropdown:IsOption(str)` | Verifica se uma opção existe |
| `dropdown:UpdateName(str)` | Altera o nome |
| `dropdown:SetVisibility(bool)` | Mostra/oculta o elemento |

---

### 🎨 Colorpicker

```lua
local colorpicker = Secao:Colorpicker({
    Name     = "Cor do ESP",
    Default  = Color3.fromRGB(255, 0, 0),
    Alpha    = 0,    -- transparência inicial (0 = opaco, 1 = invisível). Omitir desativa alpha.
    Callback = function(color, alpha)
        print("Cor:", color, "Alpha:", alpha)
    end,
}, "FlagDoCor")
```

| Método | Descrição |
|--------|-----------|
| `cp:SetColor(Color3)` | Define a cor programaticamente |
| `cp:SetAlpha(number)` | Define o alpha (0–1) |
| `cp:UpdateName(str)` | Altera o nome |
| `cp:SetVisibility(bool)` | Mostra/oculta o elemento |

---

### 🔲 Button

```lua
local button = Secao:Button({
    Name     = "Executar",
    Callback = function()
        print("Clicado!")
    end,
}, "FlagDoBotao")
```

| Método | Descrição |
|--------|-----------|
| `button:UpdateName(str)` | Altera o nome |
| `button:SetVisibility(bool)` | Mostra/oculta o elemento |

---

### 📝 Label

```lua
Secao:Label({
    Text = "Texto informativo aqui.",
}, "FlagLabel")
```

| Método | Descrição |
|--------|-----------|
| `label:UpdateName(str)` | Altera o texto |
| `label:SetVisibility(bool)` | Mostra/oculta |

---

### 🔤 SubLabel

```lua
Secao:SubLabel({
    Text = "Texto menor e mais transparente.",
})
```

---

### 🏷️ Header

```lua
Secao:Header({
    Name = "Título da seção",
})
```

---

### 📄 Paragraph

```lua
local p = Secao:Paragraph({
    Header = "Título",
    Body   = "Texto do parágrafo com mais conteúdo explicativo.",
})
```

| Método | Descrição |
|--------|-----------|
| `p:UpdateHeader(str)` | Altera o título |
| `p:UpdateBody(str)` | Altera o corpo |
| `p:SetVisibility(bool)` | Mostra/oculta |

---

### ➖ Divider (dentro de Section)

```lua
local div = Secao:Divider()
```

| Método | Descrição |
|--------|-----------|
| `div:Remove()` | Remove o divider |
| `div:SetVisibility(bool)` | Mostra/oculta |

---

### 📏 Spacer

```lua
local spacer = Secao:Spacer()
```

| Método | Descrição |
|--------|-----------|
| `spacer:Remove()` | Remove o spacer |
| `spacer:SetVisibility(bool)` | Mostra/oculta |

---

## 💾 Sistema de Configs

O MacLib possui um sistema de configs embutido para salvar/carregar configurações em JSON.

### Configurar pasta

```lua
MacLib:SetFolder("NomeDoScript")
```

### Salvar config

```lua
local ok, err = MacLib:SaveConfig("nomeDaConfig")
```

### Carregar config

```lua
local ok, err = MacLib:LoadConfig("nomeDaConfig")
```

### Listar configs disponíveis

```lua
local lista = MacLib:RefreshConfigList()
```

### Carregar config de autoload

```lua
MacLib:LoadAutoLoadConfig()
```

> **Flags:** Para que um elemento seja salvo/carregado, ele precisa ter um **Flag** (segundo argumento). Elementos com `IgnoreConfig = true` são ignorados.

---

## 🏋️ FeatureRegistry (apenas no CreateHUB)

Permite registrar hooks customizados que são executados ao carregar uma config.

```lua
HUB.FeatureRegistry:Register("FlagDoToggle", function(value)
    -- lógica extra ao carregar esse valor de config
    if value then
        ativarFeature()
    end
end)
```

---

## 🎮 Aba Player (embutida no CreateHUB)

A aba **Player** já vem com as seguintes funcionalidades prontas:

| Feature | Flag | Descrição |
|---------|------|-----------|
| WalkSpeed Toggle | `PlayerWalkSpeedToggle` | Ativa velocidade customizada |
| WalkSpeed Value | `PlayerWalkSpeedValue` | Define o valor da velocidade (16–100) |
| JumpPower Toggle | `PlayerJumpPowerToggle` | Ativa pulo customizado |
| JumpPower Value | `PlayerJumpPowerValue` | Define o valor do pulo (50–300) |
| Fly Toggle | `PlayerFlyToggle` | Ativa/desativa o voo |
| Fly Keybind | `PlayerFlyKeybind` | Tecla para alternar o voo (padrão: F3) |
| Fly Speed | `PlayerFlySpeed` | Velocidade do voo (0–300) |
| Anti-AFK | `PlayerAntiAFK` | Previne kick por inatividade |
| FPS Boost | `PlayerFPSBoost` | Remove efeitos visuais para melhorar FPS |
| Auto Hide UI | `PlayerAutoHideUI` | Esconde chat, lista de jogadores, etc. |

---

## ⚙️ Aba Settings (embutida no CreateHUB)

A aba **Settings** já vem com:

- **Menu:** keybind para abrir/fechar (salvo em arquivo separado)
- **Config:** criar, carregar, sobrescrever e deletar configs
- **Autoload:** definir config para carregar automaticamente (normal ou por conta)
- **Account Exclusive:** salva config com o UserId do jogador no nome
- **Account Autoload:** autoload exclusivo para a conta atual

---

## 🔧 MacLib.Options

Todos os elementos com Flag ficam acessíveis globalmente via:

```lua
MacLib.Options["MinhaFlag"]:UpdateState(true)
MacLib.Options["MinhaFlag"]:GetState()
```

---

## 📌 Exemplo Completo

```lua
local MacLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/NickolasFrutuoso/MacUI/refs/heads/main/UI"))()

local HUB = MacLib:CreateHUB({
    Title    = "Noliar HUB",
    Subtitle = game:GetService("MarketplaceService"):GetProductInfo(game.PlaceId).Name,
    OnTabGroup = function(TabGroup, Window)
        local CombatTab = TabGroup:Tab({ Name = "Combat", Image = "rbxassetid://10734950309" })

        local EspSection = CombatTab:Section({ Side = "Left" })
        EspSection:Header({ Name = "ESP" })

        EspSection:Toggle({
            Name     = "Player ESP",
            Default  = false,
            Callback = function(state)
                print("ESP:", state)
            end,
        }, "CombatESP")

        EspSection:Colorpicker({
            Name     = "Cor do ESP",
            Default  = Color3.fromRGB(255, 0, 0),
            Callback = function(color)
                print("Cor:", color)
            end,
        }, "CombatESPColor")

        EspSection:Slider({
            Name     = "Distância",
            Minimum  = 10,
            Maximum  = 1000,
            Default  = 500,
            Callback = function(value)
                print("Distância:", value)
            end,
        }, "CombatESPDistance")
    end,
})
```
