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

**a fun ui library i might update in the future but yk**

[![Lua](https://img.shields.io/badge/Lua-5.1-blue?style=flat-square&logo=lua)](https://lua.org) [![Roblox](https://img.shields.io/badge/Roblox-executor-red?style=flat-square)](https://roblox.com) [![Version](https://img.shields.io/badge/version-2.0.0-indigo?style=flat-square)](https://github.com/gallocatechol/guiframe)

---

```lua
local NexusUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/gallocatechol/guiframe/refs/heads/main/NexusUI.lua"))()
```

---

## whats included

| feature           | details                                                                                    |
| ----------------- | ------------------------------------------------------------------------------------------ |
| 🪟 windows         | draggable (screen-clamped), minimizable, closeable, show/hide without destroying           |
| 📑 tabs            | sidebar with icons, hide individual tabs, each has its own scroll area                     |
| 🎛️ elements       | button, toggle, slider, dropdown, textbox, keybind, label, separator, color picker, progress bar |
| 🔔 notifications   | toast popups with icon, progress bar, typed colors, click to dismiss                       |
| ✨ animations      | smooth tweens on basically everything                                                      |
| 🎨 theme           | dark by default, swappable accent color that live-updates all elements mid-session         |
| 🧹 cleanup         | `Window:Destroy()` disconnects all connections — no leaks                                  |

---

## making a window

```lua
local Window = NexusUI:CreateWindow({
    Title    = "my script",
    Subtitle = "by me",   -- optional
})
```

> you can also pass `Size` and `Position` if you want to control where it spawns. defaults to centered at 540×420.

the titlebar is draggable and clamped to the screen edges. `─` minimizes it to just the bar. `✕` closes it with an animation and cleans everything up.

**window methods**

```lua
Window:SetTitle("new title")       -- update the title text at runtime
Window:SetSubtitle("new sub")      -- update the subtitle text at runtime
Window:Toggle()                    -- show/hide without destroying
Window:Destroy()                   -- fully close and clean up all connections
```

---

## tabs

```lua
local Main   = Window:AddTab({ Name = "Main",   Icon = "⚡" })
local Combat = Window:AddTab({ Name = "Combat", Icon = "⚔️" })
local Misc   = Window:AddTab({ Name = "Misc",   Icon = "🔧" })
```

> `Icon` is optional. first tab added gets selected automatically.

```lua
Main:SetVisible(false)   -- hide a tab from the sidebar at runtime
Main:SetVisible(true)    -- bring it back
```

---

## elements

### `AddSection`

a small label to group things visually.

```lua
Main:AddSection("movement")
```

---

### `AddButton`

```lua
Main:AddButton({
    Name        = "do the thing",
    Description = "optional subtitle line",   -- shows smaller text below the name
    Variant     = "default",                  -- "default" | "accent" | "danger" | "ghost"
    Callback    = function()
        print("clicked!")
    end
})
```

`Variant` controls the look:

| variant     | style                                          |
| ----------- | ---------------------------------------------- |
| `"default"` | surface bg, turns accent on hover (default)    |
| `"accent"`  | solid accent color, white text                 |
| `"danger"`  | solid red, white text                          |
| `"ghost"`   | transparent, subtle hover fill                 |

---

### `AddToggle`

```lua
local myToggle = Main:AddToggle({
    Name        = "god mode",
    Description = "optional subtitle",   -- shows smaller text below the name
    Default     = false,
    Callback    = function(val)
        -- runs on change
    end
})
```

```lua
myToggle:Set(true)
myToggle:OnChanged(function(val) print(val) end)
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
    Suffix   = "",     -- appended to the value label, e.g. " px", "%", " ms"
    Callback = function(val)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = val
    end
})
```

> float steps work correctly — `Step = 0.1` won't produce `0.30000000004`.
> `Suffix` just adds a label, it doesn't affect the value.

```lua
mySlider:Set(100)
mySlider:OnChanged(function(val) print(val) end)
print(mySlider.Value)
```

---

### `AddTextBox`

```lua
local myInput = Main:AddTextBox({
    Name         = "teleport to player",
    Placeholder  = "username...",
    Default      = "",
    ClearOnFocus = true,
    OnFocus      = function()
        -- fires when the box is clicked into
    end,
    OnChanged    = function(text)
        -- fires live on every keystroke
    end,
    Callback     = function(text, pressedEnter)
        -- fires on focus lost (click away or enter)
    end
})
```

> the input border glows accent color while focused.

```lua
myInput:Get()
myInput:Set("hello")
myInput.Box   -- the raw TextBox instance
```

---

### `AddDropdown`

```lua
local myDropdown = Main:AddDropdown({
    Name     = "team",
    Options  = { "red", "blue", "green", "spectator" },
    Default  = "red",
    Callback = function(selected)
        print("picked:", selected)
    end
})
```

> lists with more than 5 options scroll. a search bar filters options as you type — no config needed, it's always there.

```lua
myDropdown:Set("blue")
myDropdown:Refresh({ "option a", "option b" })   -- swap the whole list
myDropdown:OnChanged(function(val) print(val) end)
print(myDropdown.Value)
```

---

### `AddColorPicker`

opens an HSV picker panel with a saturation/value square and a hue bar. shows a live hex readout.

```lua
local myColor = Main:AddColorPicker({
    Name     = "beam color",
    Default  = Color3.fromRGB(99, 102, 241),
    Callback = function(color)
        -- fires whenever the color changes while dragging
    end
})
```

```lua
myColor:Set(Color3.fromRGB(255, 0, 0))
myColor:OnChanged(function(color) print(color) end)
print(myColor.Value)   -- current Color3
```

---

### `AddProgressBar`

a read-only bar you control from code. useful for loading states, timers, health displays, etc.

```lua
local myBar = Main:AddProgressBar({
    Name    = "loading",
    Default = 0,    -- 0 to 1
})
```

```lua
myBar:Set(0.75)          -- 0 to 1
myBar:SetLabel("done")   -- update the name label
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

a thin horizontal line.

```lua
Main:AddSeparator()
```

---

### `AddKeybind`

```lua
local myKeybind = Main:AddKeybind({
    Name    = "toggle menu",
    Default = Enum.KeyCode.RightShift,
    OnPress = function()
        -- fires on keypress (not while rebinding)
    end,
    Callback = function(newKey)
        -- fires when the user picks a new key
    end
})
```

```lua
myKeybind:OnChanged(function(key) print(key.Name) end)
print(myKeybind.Value)
```

---

## notifications

```lua
NexusUI:Notify({
    Title    = "done!",
    Message  = "the thing ran successfully.",
    Duration = 4,
    Type     = "success"
})
```

| type        | color    | icon |
| ----------- | -------- | ---- |
| `"info"`    | 🟣 indigo | ℹ    |
| `"success"` | 🟢 green  | ✓    |
| `"warning"` | 🟡 yellow | ⚠    |
| `"error"`   | 🔴 red    | ✕    |

click the notification to dismiss it early.

---

## changing the accent color

`SetAccent` live-updates every element that uses the accent color — tab highlights, slider fills, toggle tracks, section labels, keybind buttons, dropdown text, progress bars — all of it, no recreating anything.

```lua
NexusUI:SetAccent(Color3.fromRGB(239, 68,  68))   -- red
NexusUI:SetAccent(Color3.fromRGB(34,  197, 94))   -- green
NexusUI:SetAccent(Color3.fromRGB(234, 179, 8))    -- yellow
NexusUI:SetAccent(Color3.fromRGB(99,  102, 241))  -- back to indigo
```

---

## cleanup

```lua
Window:Destroy()       -- closes one window, disconnects all its signal connections
NexusUI:DestroyAll()   -- tears down every window and removes the ScreenGui entirely
```

> `Window:Destroy()` is called automatically when the user clicks `✕`. you only need to call it manually if you want to close it from code.

---

## full example

<details>
<summary>click to expand</summary>

```lua
local NexusUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/gallocatechol/guiframe/refs/heads/main/NexusUI.lua"))()

local Win    = NexusUI:CreateWindow({ Title = "nexus script", Subtitle = "v2.0" })
local Main   = Win:AddTab({ Name = "Main",     Icon = "🏠" })
local Visual = Win:AddTab({ Name = "Visuals",  Icon = "🎨" })
local Misc   = Win:AddTab({ Name = "Misc",     Icon = "🔧" })

-- ── main ────────────────────────────────────────
Main:AddSection("player")

Main:AddSlider({
    Name = "walk speed", Min = 16, Max = 250, Step = 1, Default = 16,
    Suffix = " wu",
    Callback = function(v)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = v
    end
})

Main:AddSlider({
    Name = "jump power", Min = 7, Max = 200, Step = 1, Default = 7,
    Callback = function(v)
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = v
    end
})

local loading = Main:AddProgressBar({ Name = "esp loading", Default = 0 })

Main:AddSeparator()
Main:AddSection("combat")

Main:AddToggle({
    Name        = "big hitbox",
    Description = "expands the hitbox for easier hits",
    Default     = false,
    Callback    = function(val)
        -- hitbox logic
    end
})

Main:AddButton({
    Name     = "run something",
    Variant  = "accent",
    Callback = function()
        for i = 1, 10 do
            task.wait(0.1)
            loading:Set(i / 10)
        end
        NexusUI:Notify({ Title = "done", Message = "ran successfully.", Type = "success" })
    end
})

-- ── visuals ─────────────────────────────────────
Visual:AddSection("colors")

local beamColor = Visual:AddColorPicker({
    Name     = "beam color",
    Default  = Color3.fromRGB(99, 102, 241),
    Callback = function(c)
        -- apply color somewhere
    end
})

Visual:AddDropdown({
    Name    = "accent preset",
    Options = { "indigo", "red", "green", "yellow" },
    Default = "indigo",
    Callback = function(val)
        local colors = {
            indigo = Color3.fromRGB(99,  102, 241),
            red    = Color3.fromRGB(239, 68,  68),
            green  = Color3.fromRGB(34,  197, 94),
            yellow = Color3.fromRGB(234, 179, 8),
        }
        NexusUI:SetAccent(colors[val])
    end
})

-- ── misc ─────────────────────────────────────────
Misc:AddSection("settings")

Misc:AddKeybind({
    Name    = "ui toggle",
    Default = Enum.KeyCode.RightShift,
    OnPress = function()
        Win:Toggle()
    end
})

Misc:AddTextBox({
    Name        = "teleport to player",
    Placeholder = "username...",
    Callback    = function(text, enter)
        if enter and text ~= "" then
            print("tp to:", text)
        end
    end
})

Misc:AddButton({
    Name     = "destroy ui",
    Variant  = "danger",
    Callback = function()
        NexusUI:DestroyAll()
    end
})
```

</details>

---

## tips

> [!TIP]
> **organize with sections** — `AddSection` before groups of related elements makes the ui way easier to read. don't skip it.

> [!TIP]
> **keep references to return objects** — toggles, sliders, dropdowns, color pickers, and progress bars all return objects with `.Value`, `:Set()`, and `:OnChanged()`. store them in a variable if you need to interact with them from other callbacks.

> [!NOTE]
> **Callback vs OnChanged** — `Callback` in the config table and `:OnChanged(fn)` on the return object both work. use `Callback` for the main logic, `:OnChanged()` if you need to attach extra listeners later.

> [!NOTE]
> **keybind edge case is handled** — `OnPress` won't fire while the user is actively rebinding. you don't need to guard against that.

> [!TIP]
> **notify liberally** — a quick `NexusUI:Notify(...)` after anything that isn't instant gives users confidence the script is actually doing something.

> [!TIP]
> **live accent** — `SetAccent` updates every existing element. drop it in a dropdown callback for an in-ui theme switcher with zero extra work.
