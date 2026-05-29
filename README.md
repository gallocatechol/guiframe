```
 ,----..                ,--,    ,--,  
/   /   \             ,--.'|  ,--.'|  
|   :     :            |  | :  |  | :     ,---.  
.   |  ;. /            :  : '  :  : '    '   ,'\  
.   ; /--`    ,--.--.  |  ' |  |  ' |   /   /   |
;   | ;  __  /       \ '  | |  '  | |  .   ; ,. :
|   : |.' .'.--.  .-. ||  | :  |  | :  '   | |: :
.   | '_.' : \__\/: . .'  : |__'  : |__'   | .; :
'   ; : \  | ," .--.; ||  | '.'|  | '.'|   :    |
'   | '/  .'/  /  ,.  |;  :    ;  :    ;\   \  /  
|   :    / ;  :   .'   \  ,   /|  ,   /  `----'  
\   \ .'  |  ,     .-./---`-'  ---`-'  
 `---`     `--`---'
```

**V3 is fucked ngl**

[![Lua](https://img.shields.io/badge/Lua-5.1-blue?style=flat-square&logo=lua)](https://lua.org) [![Roblox](https://img.shields.io/badge/Roblox-executor-red?style=flat-square)](https://roblox.com) [![Version](https://img.shields.io/badge/version-3.0.0-indigo?style=flat-square)](https://github.com/gallocatechol/guiframe)

---

```lua
local NexusUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/gallocatechol/guiframe/refs/heads/main/NexusUI.lua"))()
```

---

## whats included

| feature | details |
| --- | --- |
| 🪟 windows | draggable (screen-clamped, touch support), minimizable, closeable, fade toggle |
| 📑 tabs | sidebar with icons + search bar, badges, hide individual tabs |
| 🎛️ elements | button, toggle, slider, dropdown, multi-dropdown, textbox, numeric input, keybind, color picker, progress bar, table, image, label, separator |
| 💾 config | save/load all element state to disk with one call |
| 🔔 notifications | typed toasts with queue, icon, progress bar, click to dismiss |
| 🔒 enable/disable | any element can be grayed out and blocked at runtime |
| 💬 tooltips | hover popups on any element |
| ✨ animations | smooth tweens on everything, spawn animation, tab slide-in |
| 🎨 theme | full dark theme, live accent + bulk theme overrides |
| 🧹 cleanup | `Window:Destroy()` disconnects all connections, no leaks |

---

## making a window

```lua
local Window = NexusUI:CreateWindow({
    Title    = "my script",
    Subtitle = "v3.0",   -- 
})
```

the window grows in on spawn. the titlebar is draggable and screen-clamped — works on both mouse and touch. `─` minimizes, `✕` closes with an animation and cleans up all connections.

**window methods**

```lua
Window:SetTitle("new title")
Window:SetSubtitle("new sub")
Window:Toggle()           -- fade in/out without destroying
Window:Minimize()         -- programmatic minimize
Window:Expand()           -- programmatic expand
Window:SetKeybind(Enum.KeyCode.RightShift)  -- dedicated toggle hotkey
Window:Destroy()          -- close + disconnect everything
Window:SaveConfig("main") -- write element state to nexusui_main.json
Window:LoadConfig("main") -- restore element state from file
```

> `SaveConfig` / `LoadConfig` only work in executors that have `writefile` / `readfile`. they serialize toggles, sliders, dropdowns, textboxes, keybinds, and numeric inputs automatically — no extra setup needed.

---

## tabs

```lua
local Main   = Window:AddTab({ Name = "Main",    Icon = "🏠" })
local Visual = Window:AddTab({ Name = "Visuals", Icon = "🎨" })
local Misc   = Window:AddTab({ Name = "Misc",    Icon = "🔧" })
```

> `Icon` is optional. the first tab is auto-selected. switching tabs plays a slide-in animation on the content.

**tab methods**

```lua
Main:SetVisible(false)   -- hide from sidebar
Main:SetBadge("3")       -- show a badge count on the tab button
Main:SetBadge(nil)       -- clear the badge
```

the sidebar also has a **search bar** — typing filters tabs by name and by the names of elements within each tab.

---

## elements

every element that takes a config table accepts an optional `Tooltip` field — hovering the element shows a small popup with that text.

every element return object has a `:SetEnabled(bool)` method that grays out the row with a 🔒 icon and blocks interaction.

---

### `AddSection`

```lua
Main:AddSection("movement")
```

---

### `AddButton`

```lua
Main:AddButton({
    Name        = "teleport",
    Description = "optional subtitle line",
    Variant     = "default",   -- "default" | "accent" | "danger" | "ghost"
    Cooldown    = 3,           -- disables for 3s after click, shows countdown + drain bar
    Tooltip     = "tp to spawn",
    Callback    = function() end,
})
```

| variant | look |
| --- | --- |
| `"default"` | surface bg, accent on hover |
| `"accent"` | solid accent, white text |
| `"danger"` | solid red, white text |
| `"ghost"` | transparent, subtle hover |

```lua
local btn = Main:AddButton({ ... })
btn:SetEnabled(false)
```

---

### `AddToggle`

```lua
local myToggle = Main:AddToggle({
    Name        = "god mode",
    Description = "optional subtitle",
    Default     = false,
    Tooltip     = "makes you invincible",
    Callback    = function(val) end,
})
```

```lua
myToggle:Set(true)
myToggle:OnChanged(function(val) end)
myToggle:SetEnabled(false)
print(myToggle.Value)
```

---

### `AddSlider`

```lua
local mySlider = Main:AddSlider({
    Name     = "walk speed",
    Min      = 16,
    Max      = 250,
    Step     = 1,
    Default  = 16,
    Suffix   = " wu",   -- appended to the value label
    Tooltip  = "right-click to type an exact value",
    Callback = function(val) end,
})
```

> right-clicking the slider opens an inline textbox so you can type an exact number instead of dragging.
> float steps work correctly — `Step = 0.1` won't produce `0.30000000004`.

```lua
mySlider:Set(100)
mySlider:OnChanged(function(val) end)
mySlider:SetEnabled(false)
print(mySlider.Value)
```

---

### `AddTextBox`

```lua
local myInput = Main:AddTextBox({
    Name         = "player name",
    Placeholder  = "username...",
    Default      = "",
    ClearOnFocus = true,
    Tooltip      = "enter a player to target",
    OnFocus      = function() end,
    OnChanged    = function(text) end,   -- fires live on every keystroke
    Callback     = function(text, pressedEnter) end,
})
```

> the input border glows accent color while focused.

```lua
myInput:Get()
myInput:Set("hello")
myInput:SetEnabled(false)
myInput.Box   -- the raw TextBox instance
```

---

### `AddNumericInput`

a number field with `−` / `+` stepper buttons. cleaner than a slider when you want exact values without dragging.

```lua
local myNum = Main:AddNumericInput({
    Name     = "damage",
    Min      = 0,
    Max      = 999,
    Step     = 1,
    Default  = 50,
    Tooltip  = "base damage per hit",
    Callback = function(val) end,
})
```

```lua
myNum:Set(100)
myNum:OnChanged(function(val) end)
myNum:SetEnabled(false)
print(myNum.Value)
```

---

### `AddDropdown`

```lua
local myDropdown = Main:AddDropdown({
    Name     = "team",
    Options  = { "red", "blue", "green", "spectator" },
    Default  = "red",
    Tooltip  = "pick your team",
    Callback = function(selected) end,
})
```

> lists with more than 5 options scroll. a search bar filters as you type.

```lua
myDropdown:Set("blue")
myDropdown:Refresh({ "a", "b", "c" })   -- swap the entire list
myDropdown:OnChanged(function(val) end)
myDropdown:SetEnabled(false)
print(myDropdown.Value)
```

---

### `AddMultiDropdown`

same look as dropdown but lets you select multiple options at once. shows a summary in the header (`"red +2"`). each option has a checkbox.

```lua
local myMulti = Main:AddMultiDropdown({
    Name     = "effects",
    Options  = { "blur", "bloom", "vignette", "chromatic" },
    Default  = { "blur" },
    Tooltip  = "pick any combination",
    Callback = function(selected) end,   -- selected is a sorted table of strings
})
```

```lua
myMulti:Set({ "blur", "bloom" })
myMulti:Get()   -- returns table of currently selected options
myMulti:OnChanged(function(t) end)
```

---

### `AddColorPicker`

HSV picker — saturation/value square + hue bar + live hex readout. opens in a panel below the row.

```lua
local myColor = Main:AddColorPicker({
    Name     = "beam color",
    Default  = Color3.fromRGB(99, 102, 241),
    Tooltip  = "pick a color",
    Callback = function(color) end,   -- fires while dragging
})
```

```lua
myColor:Set(Color3.fromRGB(255, 0, 0))
myColor:OnChanged(function(color) end)
print(myColor.Value)
```

---

### `AddProgressBar`

a read-only bar you drive from code. useful for loading states, timers, health displays.

```lua
local myBar = Main:AddProgressBar({
    Name    = "loading assets",
    Default = 0,   -- 0 to 1
})
```

```lua
myBar:Set(0.75)           -- animates smoothly
myBar:SetLabel("done!")   -- update the name label at runtime
```

---

### `AddTable`

a scrollable data grid with an optional header row. shows up to 6 rows before scrolling. alternating row colors for readability.

```lua
local myTable = Main:AddTable({
    Name    = "players",
    Headers = { "Name", "Team", "Kills" },
    Data    = {
        { "player1", "red",  "12" },
        { "player2", "blue", "7"  },
    },
})
```

```lua
myTable:SetData({
    { "player1", "red", "15" },
})
myTable:GetData()   -- returns the current data table
```

---

### `AddImage`

displays an rbxassetid image inline in the tab.

```lua
local myImg = Main:AddImage({
    Name      = "logo",                  -- optional label above the image
    Image     = "rbxassetid://0",
    Height    = 120,
    ScaleType = Enum.ScaleType.Fit,      -- optional
})
```

```lua
myImg:SetImage("rbxassetid://12345")
myImg:SetHeight(80)
```

---

### `AddLabel`

```lua
local myLabel = Main:AddLabel("status: idle")

myLabel:Set("status: running")
myLabel:Set("error", Color3.fromRGB(248, 113, 113))   -- optional color arg
```

---

### `AddSeparator`

```lua
Main:AddSeparator()
```

---

### `AddKeybind`

```lua
local myKeybind = Misc:AddKeybind({
    Name     = "toggle ui",
    Default  = Enum.KeyCode.RightShift,
    Tooltip  = "press to show/hide",
    OnPress  = function()
        -- fires on keypress (blocked while rebinding)
    end,
    Callback = function(newKey)
        -- fires when user picks a new key
    end,
})
```

> click the key button to enter rebind mode — it shows `···` and waits for a keypress. `OnPress` is automatically blocked while rebinding.

```lua
myKeybind:OnChanged(function(key) end)
myKeybind:SetEnabled(false)
print(myKeybind.Value)
```

---

## notifications

```lua
NexusUI:Notify({
    Title    = "done!",
    Message  = "operation completed successfully.",
    Duration = 4,
    Type     = "success",
})
```

| type | color | icon |
| --- | --- | --- |
| `"info"` | 🟣 indigo | ℹ |
| `"success"` | 🟢 green | ✓ |
| `"warning"` | 🟡 yellow | ⚠ |
| `"error"` | 🔴 red | ✕ |

- click a notification to dismiss it early
- max 5 visible at once — extras queue and appear as older ones clear
- `NexusUI:DismissAll()` clears everything instantly

---

## theme

### accent color

```lua
NexusUI:SetAccent(Color3.fromRGB(239, 68,  68))   -- red
NexusUI:SetAccent(Color3.fromRGB(34,  197, 94))   -- green
NexusUI:SetAccent(Color3.fromRGB(234, 179, 8))    -- yellow
NexusUI:SetAccent(Color3.fromRGB(168, 85,  247))  -- purple
NexusUI:SetAccent(Color3.fromRGB(99,  102, 241))  -- default indigo
```

live-updates every element that uses the accent color — tab highlights, slider fills, toggle tracks, section labels, keybind buttons, dropdown text, progress bars — all at once.

### bulk override

```lua
NexusUI:SetTheme({
    Background   = Color3.fromRGB(10, 10, 14),
    Surface      = Color3.fromRGB(20, 20, 28),
    Accent       = Color3.fromRGB(168, 85, 247),
})
```

pass any subset of keys — only what you include gets changed.

**all theme keys**

| key | default | used for |
| --- | --- | --- |
| `Background` | `18, 18, 24` | window fill |
| `Surface` | `26, 26, 36` | titlebar, sidebar |
| `SurfaceLight` | `34, 34, 48` | element rows |
| `SurfaceDark` | `14, 14, 20` | tooltip bg |
| `Accent` | `99, 102, 241` | highlights, fills |
| `AccentHover` | `118, 120, 255` | button hover |
| `AccentDim` | `60, 62, 160` | button press |
| `Text` | `230, 230, 240` | primary text |
| `TextMuted` | `140, 140, 160` | labels, subtitles |
| `TextDim` | `90, 90, 110` | placeholders |
| `Border` | `50, 50, 70` | element outlines |
| `Success` | `52, 211, 153` | success toast |
| `Warning` | `251, 191, 36` | warning toast, rebind state |
| `Danger` | `248, 113, 113` | close button, error toast |
| `ToggleOn` | `99, 102, 241` | toggle on-state |
| `ToggleOff` | `55, 55, 75` | toggle off-state |
| `ScrollBar` | `60, 60, 85` | scroll thumb |
| `Disabled` | `45, 45, 60` | disabled overlay |
| `DisabledText` | `80, 80, 100` | 🔒 icon color |

---

## events

```lua
NexusUI:On("windowCreated", function(window)
    print("window opened")
end)

NexusUI:On("windowDestroyed", function(window)
    print("window closed")
end)

NexusUI:On("tabChanged", function(window, tab)
    print("switched to:", tab.Label.Text)
end)

NexusUI:Off("tabChanged", myHandler)   -- remove a listener
```

---

## config save / load

all element types register their setters automatically. just call save/load on the window.

```lua
-- save to nexusui_main.json
Window:SaveConfig("main")

-- restore from nexusui_main.json (call after building the UI)
Window:LoadConfig("main")
```

> requires `writefile` / `readfile` in your executor. silently does nothing if unavailable.

---

## cleanup

```lua
Window:Destroy()       -- close one window + disconnect all connections
NexusUI:DestroyAll()   -- destroy every window + the ScreenGui
NexusUI:DismissAll()   -- clear all active and queued notifications
```

---

## full example

<details>
<summary>click to expand</summary>

```lua
local NexusUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/gallocatechol/guiframe/refs/heads/main/NexusUI.lua"))()

local Win  = NexusUI:CreateWindow({ Title = "nexus script", Subtitle = "v3.0" })
local Main = Win:AddTab({ Name = "Main",    Icon = "🏠" })
local Vis  = Win:AddTab({ Name = "Visuals", Icon = "🎨" })
local Misc = Win:AddTab({ Name = "Misc",    Icon = "🔧" })

Win:SetKeybind(Enum.KeyCode.RightShift)

-- ── main ─────────────────────────────────────────────────────
Main:AddSection("player")

Main:AddSlider({
    Name = "walk speed", Min = 16, Max = 250, Step = 1, Default = 16, Suffix = " wu",
    Callback = function(v)
        local char = game.Players.LocalPlayer.Character
        if char then char.Humanoid.WalkSpeed = v end
    end,
})

Main:AddNumericInput({
    Name = "jump power", Min = 7, Max = 500, Step = 1, Default = 50,
    Callback = function(v)
        local char = game.Players.LocalPlayer.Character
        if char then char.Humanoid.JumpPower = v end
    end,
})

Main:AddSeparator()
Main:AddSection("combat")

Main:AddToggle({
    Name = "big hitbox", Description = "expands hitbox for easier hits",
    Default = false, Tooltip = "may be detected by some anti-cheats",
    Callback = function(val) end,
})

local loading = Main:AddProgressBar({ Name = "esp loading", Default = 0 })

Main:AddButton({
    Name = "run esp", Variant = "accent", Cooldown = 5,
    Callback = function()
        for i = 1, 10 do task.wait(0.1) loading:Set(i / 10) end
        NexusUI:Notify({ Title = "esp active", Message = "loaded successfully.", Type = "success" })
    end,
})

Main:AddButton({
    Name = "kill all", Variant = "danger", Tooltip = "use with caution",
    Callback = function()
        NexusUI:Notify({ Title = "warning", Message = "this might get you banned.", Type = "warning" })
    end,
})

-- ── visuals ───────────────────────────────────────────────────
Vis:AddSection("colors")

Vis:AddColorPicker({
    Name = "beam color", Default = Color3.fromRGB(99, 102, 241),
    Callback = function(c) end,
})

Vis:AddDropdown({
    Name = "accent preset",
    Options = { "indigo", "red", "green", "yellow", "purple" },
    Default = "indigo",
    Callback = function(val)
        local colors = {
            indigo = Color3.fromRGB(99,  102, 241),
            red    = Color3.fromRGB(239, 68,  68),
            green  = Color3.fromRGB(34,  197, 94),
            yellow = Color3.fromRGB(234, 179, 8),
            purple = Color3.fromRGB(168, 85,  247),
        }
        NexusUI:SetAccent(colors[val])
    end,
})

Vis:AddMultiDropdown({
    Name = "active effects",
    Options = { "blur", "bloom", "vignette", "sun rays" },
    Default = { "bloom" },
    Callback = function(selected) end,
})

Vis:AddSection("player list")

local playerTable = Vis:AddTable({
    Name = "nearby players",
    Headers = { "Name", "Dist", "Team" },
    Data = {},
})

task.spawn(function()
    while task.wait(2) do
        local rows = {}
        for _, p in ipairs(game.Players:GetPlayers()) do
            if p ~= game.Players.LocalPlayer then
                table.insert(rows, { p.Name, "??", p.Team and p.Team.Name or "none" })
            end
        end
        playerTable:SetData(rows)
        Vis:SetBadge(tostring(#rows))
    end
end)

-- ── misc ──────────────────────────────────────────────────────
Misc:AddSection("settings")

Misc:AddKeybind({
    Name = "toggle ui", Default = Enum.KeyCode.RightShift,
    OnPress = function() Win:Toggle() end,
})

Misc:AddTextBox({
    Name = "teleport to player", Placeholder = "username...",
    Callback = function(text, enter)
        if enter and text ~= "" then print("tp to:", text) end
    end,
})

Misc:AddButton({
    Name = "save config",
    Callback = function()
        Win:SaveConfig("main")
        NexusUI:Notify({ Title = "saved", Message = "config written to disk.", Type = "success", Duration = 2 })
    end,
})

Misc:AddButton({
    Name = "destroy ui", Variant = "danger",
    Callback = function() NexusUI:DestroyAll() end,
})

Win:LoadConfig("main")
```

</details>

---

## tips

> [!TIP]
> **use sections** — grouping related elements with `AddSection` makes the UI way easier to navigate.

> [!TIP]
> **right-click sliders** — instead of dragging to a precise value, right-click the slider to type a number directly into an inline box.

> [!TIP]
> **call LoadConfig last** — call `Win:LoadConfig()` at the very end of your script after all elements are added, so every setter is registered before the values are applied.

> [!TIP]
> **SetAccent in a dropdown** — drop `SetAccent` in a dropdown callback for a full in-UI theme switcher with basically zero effort.

> [!TIP]
> **tab badges for live data** — use `Tab:SetBadge()` to surface live counts (nearby players, active warnings, etc.) without making the user open the tab to check.

> [!NOTE]
> **Callback vs OnChanged** — `Callback` in the config fires on the primary interaction (focus lost for textboxes, click for buttons). `:OnChanged(fn)` on the return object lets you attach additional listeners after the element is built.

> [!NOTE]
> **SetEnabled grays, not hides** — `element:SetEnabled(false)` overlays a dim tint and a 🔒 icon. the element stays visible and communicates that it's locked, rather than disappearing.