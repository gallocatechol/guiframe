<div align="center">

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

![Lua](https://img.shields.io/badge/Lua-5.1-blue?style=flat-square&logo=lua)
![Roblox](https://img.shields.io/badge/Roblox-executor-red?style=flat-square)
![Version](https://img.shields.io/badge/version-1.0.0-indigo?style=flat-square)

</div>

---

```lua
local NexusUI = loadstring(game:HttpGet("RAW_URL_HERE"))()
```

---

## whats included

| feature | details |
|---|---|
| 🪟 windows | draggable, minimizable, closeable |
| 📑 tabs | sidebar with icons, each has its own scroll area |
| 🎛️ elements | button, toggle, slider, dropdown, textbox, keybind, label, separator |
| 🔔 notifications | toast popups with progress bar and typed colors |
| ✨ animations | smooth tweens on basically everything |
| 🎨 theme | dark by default, swappable accent color |

---

## making a window

```lua
local Window = NexusUI:CreateWindow({
    Title    = "my script",
    Subtitle = "by me",   -- optional, shows up smaller under the title
})
```

> you can also pass `Size` and `Position` if you want to control where it spawns, but the defaults (centered, 540×420) are fine for most scripts.

the titlebar is draggable. the `─` button minimizes it down to just the bar. the `✕` closes it with a little animation.

---

## tabs

every window has a sidebar. you add tabs to it, and each tab has its own scrollable content area.

```lua
local Main   = Window:AddTab({ Name = "Main",   Icon = "⚡" })
local Combat = Window:AddTab({ Name = "Combat", Icon = "⚔️" })
local Misc   = Window:AddTab({ Name = "Misc",   Icon = "🔧" })
```

> `Icon` is optional — just leave it out if you don't want one. first tab added gets selected automatically.

from here, everything gets added to a tab — `Main:AddButton(...)`, `Combat:AddToggle(...)`, etc.

---

## elements

### `AddSection`

a small label to group things visually. doesn't do anything functional, just helps organize.

```lua
Main:AddSection("movement")
Main:AddSection("combat stuff")
```

---

### `AddButton`

```lua
Main:AddButton({
    Name     = "do the thing",
    Callback = function()
        -- runs when clicked
        print("clicked!")
    end
})
```

has a hover effect and a little click animation so its chill yk.

---

### `AddToggle`

on/off switch. returns an object so you can read or change the value later.

```lua
local myToggle = Main:AddToggle({
    Name     = "god mode",
    Default  = false,        -- starts off by default
    Callback = function(val)
        if val then
            -- turn it on
        else
            -- turn it off
        end
    end
})
```

<details>
<summary><b>using the return object</b></summary>

```lua
myToggle:Set(true)            -- force a value from code

myToggle:OnChanged(function(val)
    print("toggle is now", val)
end)

print(myToggle.Value)         -- read current state
```

</details>

---

### `AddSlider`

a draggable bar between a min and max. great for speeds, sizes, anything numeric.

```lua
local mySlider = Main:AddSlider({
    Name     = "walk speed",
    Min      = 16,
    Max      = 250,
    Step     = 1,            -- how much it increments per tick
    Default  = 16,
    Callback = function(val)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = val
    end
})
```

> `Step` controls precision — use `1` for whole numbers, `0.1` for decimals, etc.

<details>
<summary><b>using the return object</b></summary>

```lua
mySlider:Set(100)             -- jump to a value programmatically

mySlider:OnChanged(function(val)
    print("slider moved to", val)
end)

print(mySlider.Value)         -- current value
```

</details>

---

### `AddTextBox`

an input field. fires the callback when the user clicks away or presses enter.

```lua
local myInput = Main:AddTextBox({
    Name         = "teleport to player",
    Placeholder  = "username...",
    Default      = "",            -- starting text, optional
    ClearOnFocus = true,          -- clears when you click in (default: true)
    Callback     = function(text, pressedEnter)
        -- text is whatever they typed
        -- pressedEnter is true if they hit enter, false if they clicked away
        print("got:", text)
    end
})
```

<details>
<summary><b>using the return object</b></summary>

```lua
myInput:Get()          -- returns the current text
myInput:Set("hello")   -- sets the text from code
myInput.Box            -- the actual TextBox instance if you need it directly
```

</details>

---

### `AddDropdown`

a menu that opens and lets you pick one option from a list.

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

<details>
<summary><b>using the return object</b></summary>

```lua
myDropdown:Set("blue")        -- set selected option from code

myDropdown:Refresh({ "option a", "option b", "option c" })  -- swap the list entirely

myDropdown:OnChanged(function(val)
    print("now selected:", val)
end)

print(myDropdown.Value)       -- currently selected option
```

</details>

---

### `AddLabel`

just a line of text. useful for status, values, info — anything you want to display but not interact with.

```lua
local myLabel = Main:AddLabel("status: idle")

myLabel:Set("status: running")  -- update it later from anywhere
```

pass a `Color3` as the second arg for a custom color:

```lua
Main:AddLabel("something went wrong", Color3.fromRGB(248, 113, 113))
```

---

### `AddSeparator`

a thin horizontal line. use it to break up content when `AddSection` is too heavy.

```lua
Main:AddSeparator()
```

---

### `AddKeybind`

lets the user set their own hotkey. clicking the button in the ui puts it in listen mode — next key they press becomes the bind.

```lua
local myKeybind = Main:AddKeybind({
    Name    = "toggle menu",
    Default = Enum.KeyCode.RightShift,
    OnPress = function()
        -- fires every time the bound key is pressed (not while rebinding)
        print("key pressed!")
    end,
    Callback = function(newKey)
        -- fires when the user picks a new key
        print("rebound to:", newKey.Name)
    end
})
```

<details>
<summary><b>using the return object</b></summary>

```lua
myKeybind:OnChanged(function(key)
    print("new key:", key.Name)
end)

print(myKeybind.Value)   -- current KeyCode
```

</details>

---

## notifications

pops a toast in the bottom-right corner. good for giving feedback so the user knows something actually happened.

```lua
NexusUI:Notify({
    Title    = "done!",
    Message  = "the thing ran successfully.",
    Duration = 4,         -- seconds before it disappears, default is 4
    Type     = "success"  -- changes the accent color
})
```

| type | color | when to use |
|---|---|---|
| `"info"` | 🟣 indigo | general updates, default |
| `"success"` | 🟢 green | something worked |
| `"warning"` | 🟡 yellow | heads up, non-critical |
| `"error"` | 🔴 red | something failed |

there's a progress bar at the bottom that drains in real time so the user knows how long it'll stay up.

---

## changing the accent color

the default accent is indigo. swap it out before you build your window and everything inherits the new color automatically.

```lua
NexusUI:SetAccent(Color3.fromRGB(239, 68,  68))   -- red
NexusUI:SetAccent(Color3.fromRGB(34,  197, 94))   -- green
NexusUI:SetAccent(Color3.fromRGB(234, 179, 8))    -- yellow
NexusUI:SetAccent(Color3.fromRGB(99,  102, 241))  -- back to default indigo
```

---

## full example

<details>
<summary><b>click to expand</b></summary>

```lua
local NexusUI = loadstring(game:HttpGet("RAW_URL_HERE"))()

local Win  = NexusUI:CreateWindow({ Title = "nexus script", Subtitle = "v1.0" })
local Main = Win:AddTab({ Name = "Main", Icon = "🏠" })
local Misc = Win:AddTab({ Name = "Misc", Icon = "🔧" })

-- main tab
Main:AddSection("player")

Main:AddSlider({
    Name = "walk speed", Min = 16, Max = 250, Step = 1, Default = 16,
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

Main:AddSeparator()
Main:AddSection("combat")

Main:AddToggle({
    Name = "big hitbox", Default = false,
    Callback = function(val)
        -- hitbox logic here
    end
})

Main:AddButton({
    Name = "run something",
    Callback = function()
        -- your logic here
        NexusUI:Notify({ Title = "done", Message = "ran successfully.", Type = "success" })
    end
})

-- misc tab
Misc:AddSection("settings")

Misc:AddKeybind({
    Name    = "ui toggle",
    Default = Enum.KeyCode.RightShift,
    OnPress = function()
        -- toggle ui visibility
    end
})

Misc:AddDropdown({
    Name    = "accent color",
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
```

</details>

---

## tips

> [!TIP]
> **organize with sections** — `AddSection` before groups of related elements makes the ui way easier to read at a glance. don't skip it.

> [!TIP]
> **keep references to return objects** — toggles, sliders, and dropdowns all return objects with `.Value`, `:Set()`, and `:OnChanged()`. if you need to read or change them from other callbacks, store them in a variable up top.

> [!NOTE]
> **Callback vs OnChanged** — `Callback` in the config table and `:OnChanged(fn)` on the return object do the same thing. use `Callback` for the main logic and `:OnChanged()` if you need to hook in extra listeners later.

> [!NOTE]
> **keybind edge case is handled** — `OnPress` won't fire while the user is actively rebinding the key. you don't need to guard against that yourself.

> [!TIP]
> **notify liberally** — a quick `NexusUI:Notify(...)` after anything that isn't instant gives users confidence that the script is actually doing something. one line, worth it.

> [!NOTE]
> **step on sliders matters** — `Step = 1` gives whole numbers. `Step = 0.5` gives halves. the display value snaps to the step automatically.