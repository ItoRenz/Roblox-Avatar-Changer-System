# 🎭 Avatar Changer System

A comprehensive avatar transformation system for Roblox with stunning visual effects, caching system, and cross-platform support.

![Version](https://img.shields.io/badge/version-2.0.0-blue.svg)
![Roblox](https://img.shields.io/badge/platform-Roblox-red.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

## ✨ Features

### 🎯 Core Features
- **Avatar Selection System** - Browse and apply custom avatars from predefined lists
- **Epic Transformation Effects** - Cinematic transformation with particles, lightning, and camera shake
- **Smart Caching System** - Preload avatars for instant switching
- **Movement Lock** - Character is frozen during transformation for cinematic effect
- **Auto-Close UI** - Panel automatically closes after avatar application
- **Respawn Persistence** - Avatars persist through character respawns

### 🎨 Visual Effects
- Multi-layer aura system with pulsing animations
- Ground cracks and debris with physics
- Lightning bolts from sky to ground
- Energy rings expansion
- Particle tornado effects
- Screen flash for local player
- Dynamic camera shake

### 🔧 Technical Features
- **R6/R15 Compatible** - Automatic rig type detection and adjustment
- **Cross-Platform** - Works on PC, Mobile, Console, and VR
- **Performance Optimized** - Efficient caching and preloading
- **Clean Logging** - Minimal console output (debug mode available)
- **Error Handling** - Robust retry logic and fallback systems

## 📦 Installation

### Prerequisites
- Roblox Studio
- Basic knowledge of Roblox scripting
- ServerScriptService and StarterPlayerScripts enabled

### Setup Steps

1. **Avatar Data Loader** (ServerScriptService)
   ```lua
   -- Copy Script 1: AvatarDataLoader.lua
   -- Place in: ServerScriptService
   ```

2. **Avatar Changer Server** (ServerScriptService)
   ```lua
   -- Copy Script 2: AvatarChangerServer.lua
   -- Place in: ServerScriptService
   ```

3. **Avatar Transformation Effect** (StarterPlayerScripts)
   ```lua
   -- Copy Script 3: AvatarTransformationEffect.lua
   -- Place in: StarterPlayer > StarterPlayerScripts
   ```

4. **Avatar Changer Client** (StarterPlayerScripts)
   ```lua
   -- Copy Script 4: AvatarChangerClient.lua
   -- Place in: StarterPlayer > StarterPlayerScripts
   ```

## 🎮 Usage

### Player Usage
1. Click the **👤 button** in the top-left corner to open the avatar selector
2. Switch between **BOY** and **GIRL** tabs
3. Use **← PREV** and **NEXT →** to browse avatars
4. Click **APPLY AVATAR** to transform
5. Click **RESET** to return to original avatar

### Configuration

#### Avatar Data (Script 1)
```lua
local AvatarData = {
    Boy = {
        "Username1",
        "Username2",
        -- Add more usernames
    },
    Girl = {
        "Username1",
        "Username2",
        -- Add more usernames
    }
}
```

#### Debug Mode (All Scripts)
```lua
local DEBUG_MODE = false  -- Set to true for detailed logging
```

#### Server Configuration (Script 2)
```lua
local COOLDOWN_TIME = 2              -- Seconds between avatar changes
local ENABLE_PRELOADING = true       -- Enable avatar caching
local MAX_RETRY_ATTEMPTS = 3         -- API retry attempts
```

#### Effect Configuration (Script 3)
```lua
local CONFIG = {
    TotalDuration = 6,               -- Total transformation duration
    ChargeTime = 1.5,                -- Initial charge phase
    BuildupTime = 2.0,               -- Buildup phase
    PeakTime = 0.8,                  -- Peak explosion phase
    FadeTime = 1.7,                  -- Fade out phase
    PrimaryColor = Color3.fromRGB(100, 200, 255),
    -- More customization options...
}
```

#### Client Configuration (Script 4)
```lua
local AUTO_CLOSE_ENABLED = true     -- Auto-close panel after apply
local AUTO_CLOSE_DELAY = 0.5        -- Delay before auto-close
```

## 🏗️ Architecture

### Script Flow
```
┌─────────────────────────────────────────────────────────────┐
│                    INITIALIZATION PHASE                      │
├─────────────────────────────────────────────────────────────┤
│  1. AvatarDataLoader                                         │
│     ↓ Converts usernames to UserIds                          │
│     ↓ Stores in ReplicatedStorage                            │
│  2. AvatarChangerServer                                      │
│     ↓ Waits for data                                         │
│     ↓ Preloads avatar descriptions                           │
│     ↓ Caches assets                                          │
│  3. AvatarChangerClient                                      │
│     ↓ Loads data from server                                 │
│     ↓ Creates UI                                             │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                    RUNTIME PHASE                             │
├─────────────────────────────────────────────────────────────┤
│  Player clicks "Apply"                                       │
│     ↓                                                         │
│  Client → Server (RemoteEvent)                               │
│     ↓                                                         │
│  Server validates & applies avatar                           │
│     ↓                                                         │
│  Server → All Clients (RefreshTitle)                         │
│     ↓                                                         │
│  Client plays transformation effect                          │
└─────────────────────────────────────────────────────────────┘
```

### Data Flow
```
AvatarStorage (Folder)
├── Boy (Folder)
│   ├── Username1 (IntValue) → UserId
│   ├── Username2 (IntValue) → UserId
│   └── ...
├── Girl (Folder)
│   ├── Username1 (IntValue) → UserId
│   └── ...
└── DataReady (BoolValue) → true
```

## 🔧 API Reference

### Server-Side Functions

#### AvatarChanger Global (_G.AvatarChanger)
```lua
-- Clear cooldown for specific player
_G.AvatarChanger.ClearCooldown(playerId: number) → boolean

-- Clear all cooldowns
_G.AvatarChanger.ClearAllCooldowns() → void

-- Reset saved avatar for player
_G.AvatarChanger.ResetSavedAvatar(playerId: number) → boolean

-- Get all cooldowns
_G.AvatarChanger.GetCooldowns() → table

-- Get saved avatars
_G.AvatarChanger.GetSavedAvatars() → table

-- Get cache status
_G.AvatarChanger.GetCacheStatus() → table

-- Clear cache
_G.AvatarChanger.ClearCache() → void

-- Reload cache
_G.AvatarChanger.ReloadCache() → void

-- Set debug mode
_G.AvatarChanger.SetDebugMode(enabled: boolean) → void
```

### Remote Events

#### ChangeAvatar (Client → Server)
```lua
-- Request avatar change
RemoteEvent:FireServer(userId: number)

-- Parameters:
--   userId: Target avatar UserId (use Player.UserId for reset)
```

#### AvatarEffectTrigger (Client → Server → All Clients)
```lua
-- Trigger transformation effect
RemoteEvent:FireServer()
```

#### RefreshTitle (Server → All Clients)
```lua
-- Refresh player's overhead title/nametag
RemoteEvent:FireAllClients(playerId: number)
```

## 🎨 Customization

### Changing Effect Colors
```lua
-- In AvatarTransformationEffect.lua
local CONFIG = {
    PrimaryColor = Color3.fromRGB(100, 200, 255),    -- Main aura color
    SecondaryColor = Color3.fromRGB(255, 255, 255),  -- Secondary color
    AuraColor = Color3.fromRGB(150, 220, 255),       -- Particle color
    LightningColor = Color3.fromRGB(200, 230, 255),  -- Lightning color
    ExplosionColor = Color3.fromRGB(255, 255, 255),  -- Flash color
}
```

### Changing UI Colors
```lua
-- In AvatarChangerClient.lua
local TAB_COLORS = {
    Boy = Color3.fromRGB(100, 150, 255),   -- Boy tab color
    Girl = Color3.fromRGB(255, 100, 180)   -- Girl tab color
}
```

### Adjusting Effect Intensity
```lua
-- In AvatarTransformationEffect.lua
local CONFIG = {
    AuraLayers = 4,              -- Number of aura layers
    CrackCount = 16,             -- Ground cracks
    DebrisCount = 20,            -- Flying debris
    EnergyRingCount = 5,         -- Energy rings
    LightningBoltCount = 30,     -- Lightning strikes
    ShakeMagnitude = 1.5,        -- Camera shake intensity
}
```

### Changing Panel Size
```lua
-- In AvatarChangerClient.lua (createUI function)
local panelWidth = isMobile and 230 or 256   -- Width in pixels
local panelHeight = isMobile and 242 or 268  -- Height in pixels
```

## 🐛 Troubleshooting

### Common Issues

#### Avatars Not Loading
- **Check Script Order**: Ensure AvatarDataLoader runs before AvatarChangerServer
- **Wait for Data**: DataReady flag must be true before other scripts proceed
- **Verify Usernames**: Check that all usernames in AvatarData are valid
- **Enable Debug**: Set `DEBUG_MODE = true` to see detailed logs

#### Transformation Effect Not Playing
- **Check RemoteEvent**: Ensure AvatarEffectTrigger exists in ReplicatedStorage
- **Verify Script Location**: AvatarTransformationEffect must be in StarterPlayerScripts
- **Check Character**: Player must have a valid character with HumanoidRootPart

#### Movement Not Unlocking
- **Check Duration**: Ensure transformation completes (6 seconds default)
- **Verify Lock/Unlock**: Check console for lock/unlock messages with debug enabled
- **Character Reset**: Try resetting character if stuck

#### UI Not Appearing
- **Script Location**: Ensure AvatarChangerClient is in StarterPlayerScripts
- **Check PlayerGui**: Verify ScreenGui is created in PlayerGui
- **Mobile Detection**: UI adapts to mobile/PC automatically

### Debug Commands

Enable debug mode and use these commands in console:

```lua
-- Check cache status
print(_G.AvatarChanger.GetCacheStatus())

-- Check cooldowns
print(_G.AvatarChanger.GetCooldowns())

-- Clear all cooldowns
_G.AvatarChanger.ClearAllCooldowns()

-- Reload cache
_G.AvatarChanger.ReloadCache()
```

## 📊 Performance

### Optimization Features
- **Avatar Caching**: Descriptions cached after first load
- **Asset Preloading**: Assets preloaded during initialization
- **Cooldown System**: Prevents spam and reduces server load
- **Efficient Effects**: Effects use instance pooling and cleanup
- **Mobile Optimization**: Smaller UI and reduced effects on mobile

### Performance Metrics
- **Initial Load**: ~10-30 seconds (depending on avatar count)
- **Cached Load**: <100ms instant switching
- **Effect Duration**: 6 seconds with automatic cleanup
- **Memory Usage**: ~2-5MB for 100 cached avatars

## 🔒 Security

### Built-in Protection
- **Whitelist System**: Only predefined avatars can be applied
- **Cooldown System**: 2-second cooldown between changes
- **Validation**: UserId validation before API calls
- **Authorization**: Server-side authorization checks
- **Rate Limiting**: Retry limits on API calls

### Security Best Practices
1. Keep avatar lists updated and verified
2. Monitor for unusual avatar change patterns
3. Use appropriate cooldown times
4. Enable only necessary debug features in production

## 📝 Changelog

### Version 2.0.0 (Current)
- ✨ Added clean logging system (minimal console spam)
- ✨ Implemented movement lock during transformation
- ✨ Added auto-close panel feature
- ✨ Cross-platform support (PC, Mobile, Console, VR)
- ✨ R6/R15 automatic detection and support
- 🔧 Optimized caching system
- 🔧 Improved error handling
- 🐛 Fixed various bugs and edge cases

### Version 1.0.0
- Initial release
- Basic avatar changing functionality
- Transformation effects
- UI system

## 👥 Credits

- **Author**: ItoRenz00
- **System Design**: Avatar Changer with Effects
- **Supported Platforms**: Roblox PC, Mobile, Console, VR

## 📄 License

MIT License

Copyright (c) 2024 ItoRenz00

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit pull requests or open issues.

### Development Setup
1. Clone/fork the repository
2. Test changes in Roblox Studio
3. Ensure all scripts work together
4. Submit pull request with detailed description

## 📞 Support

For issues, questions, or suggestions:
- Open an issue on GitHub
- Contact: ItoRenz00
- Check troubleshooting section above

---

**Made with ❤️ for the Roblox community**
