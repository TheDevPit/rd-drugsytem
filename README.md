# 💊 Advanced Drug Selling Script - Complete Overview

## 📖 Description
A comprehensive ESX drug selling system with rank progression, NPC theft mechanics, and dynamic risk/reward gameplay. Players can sell drugs to randomly spawned NPCs while managing risks like rejections, thefts, and police alerts.

---

## 🎮 Core Features

### **1. Selling Mode System**
- **Command**: `/dealer` - Activates continuous selling mode
- **Command**: `/stopdealing` - Deactivates selling mode
- **Auto-Search**: Automatically finds new clients every 10 seconds
- **Burner Phone Required**: Players need a burner phone item to contact clients
- **No Delays**: Instant client search when starting

### **2. Trap Rank Progression** ⭐
- **5 Ranks**: Street Hustler → Block Runner → Trap Star → Kingpin → Cartel Boss
- **XP System**: 
  - Earn 10 XP per successful sale
  - Earn 5 XP per rejected sale (still trying counts)
- **Drug Restrictions**: Higher-tier drugs require higher ranks
- **Persistent**: Ranks saved to database per player
- **Command**: `/trapxp` - View rank, XP, and available drugs

### **3. NPC Theft Mechanic** 🏃
- **15% Chance**: NPCs can steal drugs and flee (configurable)
- **Chase System**: Kill the thief to recover stolen goods
- **Search Mechanic**: 3-second progress bar with kneeling animation
- **Pause/Resume**: Selling pauses until drugs are recovered
- **Risk/Reward**: Adds tension and excitement to sales

### **4. Sale Outcomes**
- **Success (55%)**: Drug sold, money received, XP gained
- **Rejection (30%)**: NPC refuses, may attack player, small XP gained
- **Theft (15%)**: NPC steals and runs, chase to recover

### **5. Police System** 🚔
- **Random Alerts**: 10% chance police get alerted even on successful sales
- **Bad Sale Alerts**: Police always alerted on rejections
- **Dispatch Notifications**: Police receive location and street name
- **Blip System**: 90-second blip on police map

---

## 📊 Configuration Settings

### **Risk Percentages** (Realistic Balanced)
```lua
rejectchance = 30        -- 30% chance NPC rejects sale
npcStealChance = 15      -- 15% chance NPC steals drugs
randomsuccesschance = 10 -- 10% police alert on success
```

### **Rank System**
```lua
enableRankSystem = true  -- Enable/disable ranks
xpPerSale = 10          -- XP per successful sale
xpPerReject = 5         -- XP per rejected sale
```

### **Requirements**
```lua
requiredCops = 0              -- Minimum cops online
requireBurnerPhone = true     -- Need burner phone
burnerPhoneItem = 'burnerphone'
account = 'black_money'       -- Payment account
```

---

## 🏆 Rank System Details

| Rank | Name | XP Required | Unlocks |
|------|------|-------------|---------|
| 1 | Street Hustler | 0 | Crack |
| 2 | Block Runner | 100 | Weed Pooch |
| 3 | Trap Star | 300 | (Add more drugs) |
| 4 | Kingpin | 600 | (Add more drugs) |
| 5 | Cartel Boss | 1000 | (Add more drugs) |

**Progression Example**:
- 10 successful sales = 100 XP = Rank 2
- 30 successful sales = 300 XP = Rank 3
- 60 successful sales = 600 XP = Rank 4
- 100 successful sales = 1000 XP = Rank 5

---

## 🎯 Gameplay Flow

### **Starting to Sell**
1. Obtain burner phone
2. Get drugs in inventory
3. Type `/dealer`
4. Script searches for client (2-4 seconds)
5. NPC spawns and approaches player

### **During Sale**
1. NPC walks to player showing wanted amount
2. Player presses **E** to sell
3. **Random outcome**:
   - **Success**: Animation plays, money received, XP gained
   - **Rejection**: NPC refuses, may attack, police alerted
   - **Theft**: NPC steals drugs and flees

### **If Stolen**
1. NPC runs away with drugs
2. Selling mode **pauses**
3. Chase and kill the thief
4. Search corpse (3-second progress bar)
5. Recover drugs
6. Press **E** to resume selling

### **Stopping**
- Type `/stopdealing` to exit selling mode
- Clears all NPCs and resets state

---

## 💻 Commands

| Command | Description | Permission |
|---------|-------------|------------|
| `/dealer` | Start selling drugs | All players |
| `/stopdealing` | Stop selling mode | All players |
| `/trapxp` | View rank & XP menu | All players |

---

## 📱 Notifications

### **Success Messages**
- ✅ "You've sold x[amount] of [drug] for $[price]"
- ⭐ "+10 XP earned! (100/300)"
- 🎉 "Rank Up! You are now a Block Runner!"

### **Warning Messages**
- ⚠️ "This stuff is horrible!" (Rejection)
- 🏃 "The client stole your drugs and ran!" (Theft)
- 🚫 "Your trap rank is too low to sell [drug]"

### **Info Messages**
- 🔍 "Searching for clients for [drug]"
- 💀 "You killed the thief! Search their body."
- 💊 "You recovered x[amount] [drug] from the body!"
- ▶️ "Press [E] to continue selling"

---

## 🗄️ Database

### **Table**: `drug_ranks`
```sql
CREATE TABLE IF NOT EXISTS `drug_ranks` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `identifier` varchar(60) NOT NULL,
  `xp` int(11) NOT NULL DEFAULT 0,
  `rank` int(11) NOT NULL DEFAULT 1,
  PRIMARY KEY (`id`),
  UNIQUE KEY `identifier` (`identifier`)
);
```

**Installation**: Run `drug_ranks.sql` before first use

---

## 📦 Dependencies

- **ESX Legacy** - Core framework
- **oxmysql** - Database queries
- **ox_lib** - Notifications, progress bars, menus, TextUI

---

## 📁 File Structure

```
rd-drugsells/
├── fxmanifest.lua          # Resource manifest
├── config.lua              # All configuration settings
├── server.lua              # Server-side logic
├── client.lua              # Client-side logic
├── drug_ranks.sql          # Database table
├── SCRIPT_OVERVIEW.md      # This file
├── TRAP_RANK_README.md     # Rank system guide
└── STEAL_FEATURE_README.md # Theft mechanic guide
```

---

## 🎨 UI/UX Features

### **ox_lib Integration**
- **Notifications**: Styled, color-coded alerts
- **Progress Bars**: Searching corpse animation
- **Menus**: `/trapxp` rank overview
- **TextUI**: Interactive prompts

### **Visual Feedback**
- 3D text above NPCs showing wanted drugs
- Kneeling search animation
- Hand-off animations during sales
- Color-coded notifications (green=success, red=error, orange=theft)

---

## ⚙️ Customization

### **Adding New Drugs**
```lua
drugs = {
    ["cocaine"] = {
        name = "cocaine",
        label = "Cocaine",
        minamount = 1,
        maxamount = 3,
        minprice = 300,
        maxprice = 500,
        requiredRank = 3,  -- Requires Trap Star rank
    },
}
```

### **Adding New Ranks**
```lua
ranks = {
    [6] = {
        name = "Drug Lord", 
        xpRequired = 2000, 
        icon = "skull-crossbones", 
        color = "#9400D3"
    },
}
```

### **Adjusting Difficulty**
```lua
-- Easy Mode
rejectchance = 20
npcStealChance = 5
xpPerSale = 20

-- Hard Mode
rejectchance = 50
npcStealChance = 30
xpPerSale = 5
```

---

## 🔧 Technical Details

### **Performance**
- Optimized Wait() loops
- Proper entity cleanup
- Thread management for paused states
- Efficient event system

### **Security**
- Server-side validation
- Anti-exploit checks
- Proper permission handling
- Database sanitization

### **Compatibility**
- ESX Legacy compatible
- Works with all inventory systems
- Compatible with police job systems
- Supports multiple languages (edit Config.notify)

---

## 🎯 Recommended Server Settings

### **Balanced Gameplay**
```lua
requiredCops = 2           -- Need 2 cops online
rejectchance = 30          -- 30% rejection
npcStealChance = 15        -- 15% theft
randomsuccesschance = 10   -- 10% alert on success
xpPerSale = 10            -- Standard XP
```

### **High Risk / High Reward**
```lua
requiredCops = 4
rejectchance = 40
npcStealChance = 25
randomsuccesschance = 20
xpPerSale = 15
```

### **Casual / Beginner Friendly**
```lua
requiredCops = 0
rejectchance = 20
npcStealChance = 10
randomsuccesschance = 5
xpPerSale = 15
```

---

## 📈 Expected Player Experience

### **Per 100 Sales** (with default settings)
- **55 Successful Sales**: $5,500 - $15,000 earned, 550 XP
- **30 Rejections**: 150 XP, potential combat
- **15 Thefts**: Chase encounters, drugs recoverable
- **~6 Police Alerts**: Police response on successful sales

### **Time to Max Rank** (Rank 5 - 1000 XP)
- **Best Case**: ~90 successful sales
- **Average**: ~120-150 sales (including rejections)
- **Playtime**: 3-5 hours of active selling

---

## 🐛 Troubleshooting

### **"Client has resigned from the order"**
- You're in an interior or invalid location
- Move to an open outdoor area (street, parking lot)

### **No XP gained**
- Check if rank system is enabled: `Config.enableRankSystem = true`
- Verify database table exists: Run `drug_ranks.sql`

### **TextUI stuck on screen**
- Type `/stopdealing` to reset
- Restart resource: `/restart rd-drugsells`

### **NPC not spawning**
- Check console for errors
- Verify you have drugs in inventory
- Ensure burner phone is in inventory (if required)

---

## 📝 Credits

**Original Script**: DevPit  
**Enhanced Version**: Advanced features including rank system, theft mechanic, pause/resume system  
**Framework**: ESX Legacy  
**Libraries**: ox_lib, oxmysql  

---

## 🚀 Quick Start Guide

1. **Install**: Place in `resources/[rd]/rd-drugsells`
2. **Database**: Run `drug_ranks.sql` in your database
3. **Configure**: Edit `config.lua` to your preferences
4. **Start**: Add `ensure rd-drugsells` to server.cfg
5. **Play**: Get burner phone + drugs, type `/dealer`

---

## 📞 Support

For issues or questions:
- Check `TRAP_RANK_README.md` for rank system help
- Check `STEAL_FEATURE_README.md` for theft mechanic help
- Review config comments for setting explanations
- Check F8 console for error messages

---

**Version**: 2.0  
**Last Updated**: 2025-10-02  
**Status**: Production Ready ✅
