# uMod Plugins Dataset - Code Abstractions (Continued)

Chunk 6 - Generated: 2025-07-06 20:25:18

## MagicRainOfFirePanel by MJSU - Displays when the rain of fire event is active in Magic Panel

```csharp
using System;
using System.ComponentModel;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Magic Rain Of Fire Panel", "MJSU", "1.0.2")]
[Description("Displays if the rain of fire event is active")]
public class MagicRainOfFirePanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private bool _isRainOfFireActive;
    private Timer _endTimer;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void OnEntitySpawned(TimedExplosive explosive);
    private Hash<string, object> GetPanel();
    private bool CanShowPanel(TimedExplosive explosive);
    private class PluginConfig
    {
        [DefaultValue("#00FF00FF")]
        [JsonProperty(PropertyName = "Active Color")]
        public string ActiveColor { get; set; }
        [DefaultValue("#FFFFFF1A")]
        [JsonProperty(PropertyName = "Inactive Color")]
        public string InactiveColor { get; set; }
        [DefaultValue(5f)]
        [JsonProperty(PropertyName = "Event ends after last rocket (Seconds)")]
        public float EventEnd { get; set; }
        [JsonProperty(PropertyName = "Panel Settings")]
        public PanelRegistration PanelSettings { get; set; }
        [JsonProperty(PropertyName = "Panel Layout")]
        public Panel Panel { get; set; }
    }

    private class PanelRegistration
    {
        public string Dock { get; set; }
        public float Width { get; set; }
        public int Order { get; set; }
        public string BackgroundColor { get; set; }
    }

    private class Panel
    {
        public PanelImage Image { get; set; }
        public Hash<string, object> ToHash();
    }

    private abstract class PanelType
    {
        public bool Enabled { get; set; }
        public string Color { get; set; }
        public int Order { get; set; }
        public float Width { get; set; }
        public TypePadding Padding { get; set; }
        public virtual Hash<string, object> ToHash();
    }

    private class PanelImage : PanelType
    {
        public string Url { get; set; }
        public override Hash<string, object> ToHash();
    }

    private class TypePadding
    {
        public float Left { get; set; }
        public float Right { get; set; }
        public float Top { get; set; }
        public float Bottom { get; set; }
        public TypePadding(float left, float right, float top, float bottom);
        public Hash<string, object> ToHash();
    }

}

private class PluginConfig
{
    [DefaultValue("#00FF00FF")]
    [JsonProperty(PropertyName = "Active Color")]
    public string ActiveColor { get; set; }
    [DefaultValue("#FFFFFF1A")]
    [JsonProperty(PropertyName = "Inactive Color")]
    public string InactiveColor { get; set; }
    [DefaultValue(5f)]
    [JsonProperty(PropertyName = "Event ends after last rocket (Seconds)")]
    public float EventEnd { get; set; }
    [JsonProperty(PropertyName = "Panel Settings")]
    public PanelRegistration PanelSettings { get; set; }
    [JsonProperty(PropertyName = "Panel Layout")]
    public Panel Panel { get; set; }
}

private class PanelRegistration
{
    public string Dock { get; set; }
    public float Width { get; set; }
    public int Order { get; set; }
    public string BackgroundColor { get; set; }
}

private class Panel
{
    public PanelImage Image { get; set; }
    public Hash<string, object> ToHash();
}

private abstract class PanelType
{
    public bool Enabled { get; set; }
    public string Color { get; set; }
    public int Order { get; set; }
    public float Width { get; set; }
    public TypePadding Padding { get; set; }
    public virtual Hash<string, object> ToHash();
}

private class PanelImage : PanelType
{
    public string Url { get; set; }
    public override Hash<string, object> ToHash();
}

private class TypePadding
{
    public float Left { get; set; }
    public float Right { get; set; }
    public float Top { get; set; }
    public float Bottom { get; set; }
    public TypePadding(float left, float right, float top, float bottom);
    public Hash<string, object> ToHash();
}


```

---

## MagicSantaPanel by MJSU - Displays when the Santa event is active in Magic Panel

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Magic Santa Panel", "MJSU", "1.0.2")]
[Description("Displays if the santa event is active")]
public class MagicSantaPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private List<SantaSleigh> _activeSleighs;
    private bool _init;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void CheckEvent();
    private void UnsubscribeAll();
    private void OnEntitySpawned(SantaSleigh santa);
    private void OnEntityKill(SantaSleigh santa);
    private Hash<string, object> GetPanel();
    private bool CanShowPanel(SantaSleigh santa);
    private class PluginConfig
    {
        [DefaultValue("#DC3545FF")]
        [JsonProperty(PropertyName = "Active Color")]
        public string ActiveColor { get; set; }
        [DefaultValue("#FFFFFF1A")]
        [JsonProperty(PropertyName = "Inactive Color")]
        public string InactiveColor { get; set; }
        [JsonProperty(PropertyName = "Panel Settings")]
        public PanelRegistration PanelSettings { get; set; }
        [JsonProperty(PropertyName = "Panel Layout")]
        public Panel Panel { get; set; }
    }

    private class PanelRegistration
    {
        public string Dock { get; set; }
        public float Width { get; set; }
        public int Order { get; set; }
        public string BackgroundColor { get; set; }
    }

    private class Panel
    {
        public PanelImage Image { get; set; }
        public Hash<string, object> ToHash();
    }

    private abstract class PanelType
    {
        public bool Enabled { get; set; }
        public string Color { get; set; }
        public int Order { get; set; }
        public float Width { get; set; }
        public TypePadding Padding { get; set; }
        public virtual Hash<string, object> ToHash();
    }

    private class PanelImage : PanelType
    {
        public string Url { get; set; }
        public override Hash<string, object> ToHash();
    }

    private class TypePadding
    {
        public float Left { get; set; }
        public float Right { get; set; }
        public float Top { get; set; }
        public float Bottom { get; set; }
        public TypePadding(float left, float right, float top, float bottom);
        public Hash<string, object> ToHash();
    }

}

private class PluginConfig
{
    [DefaultValue("#DC3545FF")]
    [JsonProperty(PropertyName = "Active Color")]
    public string ActiveColor { get; set; }
    [DefaultValue("#FFFFFF1A")]
    [JsonProperty(PropertyName = "Inactive Color")]
    public string InactiveColor { get; set; }
    [JsonProperty(PropertyName = "Panel Settings")]
    public PanelRegistration PanelSettings { get; set; }
    [JsonProperty(PropertyName = "Panel Layout")]
    public Panel Panel { get; set; }
}

private class PanelRegistration
{
    public string Dock { get; set; }
    public float Width { get; set; }
    public int Order { get; set; }
    public string BackgroundColor { get; set; }
}

private class Panel
{
    public PanelImage Image { get; set; }
    public Hash<string, object> ToHash();
}

private abstract class PanelType
{
    public bool Enabled { get; set; }
    public string Color { get; set; }
    public int Order { get; set; }
    public float Width { get; set; }
    public TypePadding Padding { get; set; }
    public virtual Hash<string, object> ToHash();
}

private class PanelImage : PanelType
{
    public string Url { get; set; }
    public override Hash<string, object> ToHash();
}

private class TypePadding
{
    public float Left { get; set; }
    public float Right { get; set; }
    public float Top { get; set; }
    public float Bottom { get; set; }
    public TypePadding(float left, float right, float top, float bottom);
    public Hash<string, object> ToHash();
}


```

---

## MagicServerRewardsPanel by MJSU - Displays player server rewards data in Magic Panel

```csharp
using System;
using System.Collections;
using System.ComponentModel;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Magic Server Rewards Panel", "MJSU", "1.0.7")]
[Description("Displays player server rewards data in MagicPanel")]
public class MagicServerRewardsPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private readonly Plugin ServerRewards;
    private PluginConfig _pluginConfig;
    private string _textFormat;
    private readonly Hash<ulong, int> _playerRewards;
    private Coroutine _updateRoutine;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void Unload();
    private void UnsubscribeAll();
    private void UpdatePlayerRewards();
    private IEnumerator HandleUpdateServerRewards();
    private Hash<string, object> GetPanel(BasePlayer player);
    private int GetServerRewards(ulong userId);
    private class PluginConfig
    {
        [DefaultValue(5f)]
        [JsonProperty(PropertyName = "Panel Update Rate (Seconds)")]
        public float UpdateRate { get; set; }
        [JsonProperty(PropertyName = "Panel Settings")]
        public PanelRegistration PanelSettings { get; set; }
        [JsonProperty(PropertyName = "Panel Layout")]
        public Panel Panel { get; set; }
    }

    private class PanelRegistration
    {
        public string Dock { get; set; }
        public float Width { get; set; }
        public int Order { get; set; }
        public string BackgroundColor { get; set; }
    }

    private class Panel
    {
        public PanelImage Image { get; set; }
        public PanelText Text { get; set; }
        public Hash<string, object> ToHash();
    }

    private abstract class PanelType
    {
        public bool Enabled { get; set; }
        public string Color { get; set; }
        public int Order { get; set; }
        public float Width { get; set; }
        public TypePadding Padding { get; set; }
        public virtual Hash<string, object> ToHash();
    }

    private class PanelImage : PanelType
    {
        public string Url { get; set; }
        public override Hash<string, object> ToHash();
    }

    private class PanelText : PanelType
    {
        public string Text { get; set; }
        public int FontSize { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        public TextAnchor TextAnchor { get; set; }
        public override Hash<string, object> ToHash();
    }

    private class TypePadding
    {
        public float Left { get; set; }
        public float Right { get; set; }
        public float Top { get; set; }
        public float Bottom { get; set; }
        public TypePadding(float left, float right, float top, float bottom);
        public Hash<string, object> ToHash();
    }

}

private class PluginConfig
{
    [DefaultValue(5f)]
    [JsonProperty(PropertyName = "Panel Update Rate (Seconds)")]
    public float UpdateRate { get; set; }
    [JsonProperty(PropertyName = "Panel Settings")]
    public PanelRegistration PanelSettings { get; set; }
    [JsonProperty(PropertyName = "Panel Layout")]
    public Panel Panel { get; set; }
}

private class PanelRegistration
{
    public string Dock { get; set; }
    public float Width { get; set; }
    public int Order { get; set; }
    public string BackgroundColor { get; set; }
}

private class Panel
{
    public PanelImage Image { get; set; }
    public PanelText Text { get; set; }
    public Hash<string, object> ToHash();
}

private abstract class PanelType
{
    public bool Enabled { get; set; }
    public string Color { get; set; }
    public int Order { get; set; }
    public float Width { get; set; }
    public TypePadding Padding { get; set; }
    public virtual Hash<string, object> ToHash();
}

private class PanelImage : PanelType
{
    public string Url { get; set; }
    public override Hash<string, object> ToHash();
}

private class PanelText : PanelType
{
    public string Text { get; set; }
    public int FontSize { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    public TextAnchor TextAnchor { get; set; }
    public override Hash<string, object> ToHash();
}

private class TypePadding
{
    public float Left { get; set; }
    public float Right { get; set; }
    public float Top { get; set; }
    public float Bottom { get; set; }
    public TypePadding(float left, float right, float top, float bottom);
    public Hash<string, object> ToHash();
}


```

---

## MagicServerTimePanel by MJSU - Displays the local server time in Magic Panel

```csharp
using System;
using System.ComponentModel;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Magic Server Time Panel", "MJSU", "1.0.6")]
[Description("Displays the servers local time in magic panel")]
public class MagicServerTimePanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private string _textFormat;
    private string _previousTime;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void UpdateTime();
    private Hash<string, object> GetPanel();
    private class PluginConfig
    {
        [DefaultValue(60f)]
        [JsonProperty(PropertyName = "Update Rate (Seconds)")]
        public float UpdateRate { get; set; }
        [DefaultValue(0)]
        [JsonProperty(PropertyName = "Offset time (Minutes)")]
        public int MinuteOffset { get; set; }
        [JsonProperty(PropertyName = "Panel Settings")]
        public PanelRegistration PanelSettings { get; set; }
        [JsonProperty(PropertyName = "Panel Layout")]
        public Panel Panel { get; set; }
    }

    private class PanelRegistration
    {
        public string Dock { get; set; }
        public float Width { get; set; }
        public int Order { get; set; }
        public string BackgroundColor { get; set; }
    }

    private class Panel
    {
        public PanelImage Image { get; set; }
        public PanelText Text { get; set; }
        public Hash<string, object> ToHash();
    }

    private abstract class PanelType
    {
        public bool Enabled { get; set; }
        public string Color { get; set; }
        public int Order { get; set; }
        public float Width { get; set; }
        public TypePadding Padding { get; set; }
        public virtual Hash<string, object> ToHash();
    }

    private class PanelImage : PanelType
    {
        public string Url { get; set; }
        public override Hash<string, object> ToHash();
    }

    private class PanelText : PanelType
    {
        public string Text { get; set; }
        public int FontSize { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        public TextAnchor TextAnchor { get; set; }
        public override Hash<string, object> ToHash();
    }

    private class TypePadding
    {
        public float Left { get; set; }
        public float Right { get; set; }
        public float Top { get; set; }
        public float Bottom { get; set; }
        public TypePadding(float left, float right, float top, float bottom);
        public Hash<string, object> ToHash();
    }

}

private class PluginConfig
{
    [DefaultValue(60f)]
    [JsonProperty(PropertyName = "Update Rate (Seconds)")]
    public float UpdateRate { get; set; }
    [DefaultValue(0)]
    [JsonProperty(PropertyName = "Offset time (Minutes)")]
    public int MinuteOffset { get; set; }
    [JsonProperty(PropertyName = "Panel Settings")]
    public PanelRegistration PanelSettings { get; set; }
    [JsonProperty(PropertyName = "Panel Layout")]
    public Panel Panel { get; set; }
}

private class PanelRegistration
{
    public string Dock { get; set; }
    public float Width { get; set; }
    public int Order { get; set; }
    public string BackgroundColor { get; set; }
}

private class Panel
{
    public PanelImage Image { get; set; }
    public PanelText Text { get; set; }
    public Hash<string, object> ToHash();
}

private abstract class PanelType
{
    public bool Enabled { get; set; }
    public string Color { get; set; }
    public int Order { get; set; }
    public float Width { get; set; }
    public TypePadding Padding { get; set; }
    public virtual Hash<string, object> ToHash();
}

private class PanelImage : PanelType
{
    public string Url { get; set; }
    public override Hash<string, object> ToHash();
}

private class PanelText : PanelType
{
    public string Text { get; set; }
    public int FontSize { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    public TextAnchor TextAnchor { get; set; }
    public override Hash<string, object> ToHash();
}

private class TypePadding
{
    public float Left { get; set; }
    public float Right { get; set; }
    public float Top { get; set; }
    public float Bottom { get; set; }
    public TypePadding(float left, float right, float top, float bottom);
    public Hash<string, object> ToHash();
}


```

---

## MagicSets by  - Allows users to store and create custom skinned gearsets with one command

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using System.Linq;

Oxide.Plugins
[Info("Magic Sets", "Norn", "0.2.0")]
[Description("Allows users to store and create custom gearsets with one command")]
 class MagicSets : RustPlugin
{
     string Lang(string key, string id, object[] args);
    private const string PERMISSION_DEFAULT;
    private const string PERMISSION_VIP;
    private List<SetInfo> Sets;
     class SetInfo
    {
        public int ID;
        public ulong OwnerID;
        public string Name;
        public bool Public;
        public int TimesUsed;
        public long LastUsed;
        public List<InternalItemInfo> Contents;
        public SetInfo();
    }

     class InternalItemInfo
    {
        public string Shortname;
        public ulong SkinID;
        public InternalItemInfo();
    }

     void Init();
     void Unload();
    private long UnixTimestamp();
    private DateTime UnixToDateTime(long unixTime);
    private long DateTimeToUnixTimestamp(DateTime dateTime);
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    private List<ItemAmount> ReturnIngredients(ItemDefinition item);
     void LoadData();
     void SaveData();
     void RemoveOldEntries();
    private bool HasItem(BasePlayer player, string item, int amount);
    private bool TakeItem(BasePlayer player, string item, int amount);
    private bool GiveSetItem(BasePlayer player, int itemid, ulong skinid, int amount, bool wear);
    [ChatCommand("sets")]
    private void SetsCommand(BasePlayer player, string command, string[] args);
    private void SetsCommandHandler(BasePlayer player, string[] args);
    private void Reskin(BasePlayer player, int id);
    private void ReskinFromArgs(BasePlayer player, string[] args);
    private void CreateFromArgs(BasePlayer player, string[] args);
    private void PublicStatusFromArgs(BasePlayer player, string[] args);
    private void RemoveFromArgs(BasePlayer player, string[] args);
    private void AddFromArgs(BasePlayer player, string[] args);
    private void UpdatePublicStatus(BasePlayer player, string args);
    private void CreateSet(BasePlayer player, int id);
    private SetInfo SetFromName(BasePlayer player, string setname);
    private SetInfo SetFromID(int id);
    private bool SetExistsFromName(BasePlayer player, string setname);
    private bool SetExistsFromID(int id);
    private int SetIDFromName(BasePlayer player, string setname);
    private IEnumerable<SetInfo> ReturnSets(BasePlayer player);
    private string FormatPublicString(bool status);
    private bool HasSets(BasePlayer player);
    private int SetsNextID();
    private int SetCount(BasePlayer player);
    private int RemoveAllSets(BasePlayer player);
    private void SetList(BasePlayer player);
    private void RemoveSet(BasePlayer player, string arg);
    private void AddSet(BasePlayer player, string arg);
    private void ClearSets(BasePlayer player);
}

 class SetInfo
{
    public int ID;
    public ulong OwnerID;
    public string Name;
    public bool Public;
    public int TimesUsed;
    public long LastUsed;
    public List<InternalItemInfo> Contents;
    public SetInfo();
}

 class InternalItemInfo
{
    public string Shortname;
    public ulong SkinID;
    public InternalItemInfo();
}


```

---

## MagicSleepersPanel by MJSU - Displays the sleeping player count in Magic Panel

```csharp
using System;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Magic Sleepers Panel", "MJSU", "1.0.3")]
[Description("Displays the sleeping players in magic panel")]
public class MagicSleepersPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private string _textFormat;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void OnPlayerSleepEnded(BasePlayer player);
    private void OnPlayerSleep(BasePlayer player);
    private void UpdatePanel();
    private Hash<string, object> GetPanel();
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Panel Settings")]
        public PanelRegistration PanelSettings { get; set; }
        [JsonProperty(PropertyName = "Panel Layout")]
        public Panel Panel { get; set; }
    }

    private class PanelRegistration
    {
        public string Dock { get; set; }
        public float Width { get; set; }
        public int Order { get; set; }
        public string BackgroundColor { get; set; }
    }

    private class Panel
    {
        public PanelImage Image { get; set; }
        public PanelText Text { get; set; }
        public Hash<string, object> ToHash();
    }

    private abstract class PanelType
    {
        public bool Enabled { get; set; }
        public string Color { get; set; }
        public int Order { get; set; }
        public float Width { get; set; }
        public TypePadding Padding { get; set; }
        public virtual Hash<string, object> ToHash();
    }

    private class PanelImage : PanelType
    {
        public string Url { get; set; }
        public override Hash<string, object> ToHash();
    }

    private class PanelText : PanelType
    {
        public string Text { get; set; }
        public int FontSize { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        public TextAnchor TextAnchor { get; set; }
        public override Hash<string, object> ToHash();
    }

    private class TypePadding
    {
        public float Left { get; set; }
        public float Right { get; set; }
        public float Top { get; set; }
        public float Bottom { get; set; }
        public TypePadding(float left, float right, float top, float bottom);
        public Hash<string, object> ToHash();
    }

}

private class PluginConfig
{
    [JsonProperty(PropertyName = "Panel Settings")]
    public PanelRegistration PanelSettings { get; set; }
    [JsonProperty(PropertyName = "Panel Layout")]
    public Panel Panel { get; set; }
}

private class PanelRegistration
{
    public string Dock { get; set; }
    public float Width { get; set; }
    public int Order { get; set; }
    public string BackgroundColor { get; set; }
}

private class Panel
{
    public PanelImage Image { get; set; }
    public PanelText Text { get; set; }
    public Hash<string, object> ToHash();
}

private abstract class PanelType
{
    public bool Enabled { get; set; }
    public string Color { get; set; }
    public int Order { get; set; }
    public float Width { get; set; }
    public TypePadding Padding { get; set; }
    public virtual Hash<string, object> ToHash();
}

private class PanelImage : PanelType
{
    public string Url { get; set; }
    public override Hash<string, object> ToHash();
}

private class PanelText : PanelType
{
    public string Text { get; set; }
    public int FontSize { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    public TextAnchor TextAnchor { get; set; }
    public override Hash<string, object> ToHash();
}

private class TypePadding
{
    public float Left { get; set; }
    public float Right { get; set; }
    public float Top { get; set; }
    public float Bottom { get; set; }
    public TypePadding(float left, float right, float top, float bottom);
    public Hash<string, object> ToHash();
}


```

---

## MagicTeleportation by  - Magical home/teleport system.

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using UnityEngine;
using Oxide.Core.Plugins;
using Rust;

Oxide.Plugins
[Info("MagicTeleportation", "Norn", "1.1.3", ResourceId = 1404)]
[Description("Teleportation system.")]
public class MagicTeleportation : RustPlugin
{
    [PluginReference]
     Plugin PopupNotifications;
    [PluginReference]
     Plugin BuildingOwners;
     class StoredData
    {
        public Dictionary<ulong, PlayerData> PlayerData;
        public Dictionary<string, HomeEntities> Entities;
        public Dictionary<int, TeleportInfo> Teleports;
        public StoredData();
    }

    public class TeleportInfo
    {
        public int iID;
        public string tTitle;
        public string tDescription;
        public float fX;
        public float fY;
        public float fZ;
        public bool uEnabled;
        public int iAuthLevel;
        public bool uSleepGod;
        public int iCount;
        public TeleportInfo();
    }

     class HomesLocs
    {
        public float fX;
        public float fY;
        public float fZ;
        public float fHealth;
        public int iEID;
        public string tBagName;
        public HomesLocs();
    }

     class PlayerData
    {
        public ulong uUserID;
        public bool uCooldownEnabled;
        public Dictionary<int, HomesLocs> HomesLocs;
        public PlayerData();
    }

    public class HomeEntities
    {
        public string tPrefab_name;
        public string tShortname;
        public bool bEnabled;
        public HomeEntities();
    }

     Dictionary<string, string> DEFAULT_HomeEntities;
     StoredData MTData;
    private void Loaded();
    public static Int32 UnixTimeStampUTC();
    public static int GetRandomNumber(int min, int max);
    private int CreateTeleport(string title, string description, float X, float Y, float Z, bool sleepgod, int authlevel, bool enabled);
    private bool PlayerExists(BasePlayer player);
    private int ResetCooldownForAll();
    private int PlayerHomeCount(BasePlayer player);
    private bool InitPlayer(BasePlayer player);
    private bool EntityPlayerCheck(uint netid, BasePlayer attacker);
    private bool EntityExists(BaseCombatEntity entity);
    private void OnHomeEntityDeath(BaseCombatEntity entity, HitInfo info);
     List<ulong> SLEEPING_TELEPORTERS;
     Dictionary<ulong, Timer> TELEPORT_QUEUE;
    private HitInfo OnEntityTakeDamage(BaseCombatEntity vic, HitInfo hitInfo);
     bool CancelTeleport(BasePlayer player);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private static Dictionary<string, string> displaynameToShortname;
    private static Dictionary<string, int> deployedToItem;
    private void InitializeTable();
    private void RefundHomeEntity(BasePlayer player, BaseEntity entity, int amount);
    private void OnEntityBuilt(Planner planner, GameObject gameObject);
     Dictionary<string, object> GetSleepingBagData(SleepingBag bag);
     List<Dictionary<string, object>> FindSleepingBags(ulong userid);
    private bool UnfreezePlayer(ulong steamid);
    private void CommandMessage(BasePlayer player);
    [ChatCommand("t")]
     void cmdHome(BasePlayer player, string cmd, string[] args);
    private bool InitTeleport(BasePlayer player, float init_x, float init_y, float init_z, bool type, bool printtoplayer, string title, string description, int count, int seconds);
    private void TeleportPlayerPosition(BasePlayer player, Vector3 pos);
     void Unload();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     Timer SyncHomeData;
     void OnServerInitialized();
     void InitializeConfig();
     void SyncHomesEx(BasePlayer player);
     void SyncHomes();
     void SaveData();
    private string IsTeleportationCapable(BasePlayer player);
    private string FindPlayerName(ulong userId);
    private void GetDeployedItemOwner(BasePlayer player, SleepingBag ditem);
    private object FindOwnerBlock(BuildingBlock block);
    protected override void LoadDefaultConfig();
     int PopulateEntityData();
    private void PrintToChatEx(BasePlayer player, string result, string tcolour);
}

 class StoredData
{
    public Dictionary<ulong, PlayerData> PlayerData;
    public Dictionary<string, HomeEntities> Entities;
    public Dictionary<int, TeleportInfo> Teleports;
    public StoredData();
}

public class TeleportInfo
{
    public int iID;
    public string tTitle;
    public string tDescription;
    public float fX;
    public float fY;
    public float fZ;
    public bool uEnabled;
    public int iAuthLevel;
    public bool uSleepGod;
    public int iCount;
    public TeleportInfo();
}

 class HomesLocs
{
    public float fX;
    public float fY;
    public float fZ;
    public float fHealth;
    public int iEID;
    public string tBagName;
    public HomesLocs();
}

 class PlayerData
{
    public ulong uUserID;
    public bool uCooldownEnabled;
    public Dictionary<int, HomesLocs> HomesLocs;
    public PlayerData();
}

public class HomeEntities
{
    public string tPrefab_name;
    public string tShortname;
    public bool bEnabled;
    public HomeEntities();
}


```

---

## MagicTools by Mevent - Automatically smelts mined resources and can get a Bonushit multiplier

```csharp
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Rust;

Oxide.Plugins
[Info("Magic Tools", "Krungh Crow", "3.0.0")]
[Description("Automatically smelts Mined resources and bonuses")]
 class MagicTools : CovalencePlugin
{
    const string Use_Perm;
    const string Default_Perm;
    const string Vip_Perm;
     void Init();
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Multiplier on Bonus hit (rounded numbers only(ea 1 not 1.2)")]
        public SettingsBonus Settings;
    }

     class SettingsBonus
    {
        [JsonProperty(PropertyName = "Default multiplier on Bonus")]
        public int MultiDefault;
        [JsonProperty(PropertyName = "Vip multiplier on Bonus")]
        public int MultiVip;
    }

    private bool LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConf();
    private object OnDispenserGather(ResourceDispenser dispenser, BasePlayer player, Item item);
    private object OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Multiplier on Bonus hit (rounded numbers only(ea 1 not 1.2)")]
    public SettingsBonus Settings;
}

 class SettingsBonus
{
    [JsonProperty(PropertyName = "Default multiplier on Bonus")]
    public int MultiDefault;
    [JsonProperty(PropertyName = "Vip multiplier on Bonus")]
    public int MultiVip;
}


```

---

## MagicVariables by  - Simple static variable API for plugin developers

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Rust;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("MagicVariables", "Norn", 0.1, ResourceId = 1419)]
[Description("Simple static variable system.")]
public class MagicVariables : RustPlugin
{
    public Int32 UnixTimeStampUTC();
    static readonly DateTime UnixEpoch;
    static readonly double MaxUnixSeconds;
    public static DateTime UnixTimeStampToDateTime(double unixTimeStamp);
    public class StaticVariables
    {
        public int iResourceID;
        public Dictionary<string, object> Variables;
        public Hash<ulong, Dictionary<string, object>> UserVariables;
        public Dictionary<ulong, PlayersInit> Users;
        public StaticVariables();
    }

    public class PlayersInit
    {
        public ulong uUserID;
        public string tDisplayName;
        public PlayersInit();
    }

    public class PluginInfo
    {
        public Dictionary<Plugin, StaticVariables> Plugins;
        public PluginInfo();
    }

     PluginInfo Data;
    private bool PluginExists(Plugin plugin);
    private bool SetStaticVariable(Plugin plugin, string variable, string value, bool debug);
    private string GetStaticVariable(Plugin plugin, string variable, bool debug);
    private bool RemoveStaticVariable(Plugin plugin, string variable, bool debug);
    private bool DestroyPlugin(Plugin plugin, bool debug);
    private bool InitPlugin(Plugin plugin, bool debug);
     void OnServerInitialized();
    private bool RemoveStaticPlayerVariable(Plugin plugin, BasePlayer player, string variable, bool debug);
    private string GetStaticPlayerVariable(Plugin plugin, BasePlayer player, string variable, bool debug);
    private void SetStaticPlayerVariable(Plugin plugin, BasePlayer player, string variable, string value, bool debug);
    private int PlayerHasVariables(Plugin plugin, BasePlayer player);
    private bool InitPlayer(Plugin plugin, BasePlayer player, bool debug);
    private bool RemovePlayer(Plugin plugin, BasePlayer player, bool debug);
    private bool PlayerExists(Plugin plugin, BasePlayer player);
    protected override void LoadDefaultConfig();
}

public class StaticVariables
{
    public int iResourceID;
    public Dictionary<string, object> Variables;
    public Hash<ulong, Dictionary<string, object>> UserVariables;
    public Dictionary<ulong, PlayersInit> Users;
    public StaticVariables();
}

public class PlayersInit
{
    public ulong uUserID;
    public string tDisplayName;
    public PlayersInit();
}

public class PluginInfo
{
    public Dictionary<Plugin, StaticVariables> Plugins;
    public PluginInfo();
}


```

---

## MagicWipePanel by MJSU - Displays days till server wipe in Magic Panel

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Magic Wipe Panel", "MJSU", "1.0.5")]
[Description("Displays days to wipe in magic panel")]
public class MagicWipePanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private readonly Plugin WipeInfoApi;
    private PluginConfig _pluginConfig;
    private int _daysTillWipe;
    private string _textFormat;
    private void Init();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void OnWipeCalculated();
    private void MagicPanelRegisterPanels();
    private Hash<string, object> GetPanel();
    private string Lang(string key, object[] args);
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Panel Settings")]
        public PanelRegistration PanelSettings { get; set; }
        [JsonProperty(PropertyName = "Panel Layout")]
        public Panel Panel { get; set; }
    }

    private class LangKeys
    {
        public const string Today;
        public const string OneDay;
        public const string Days;
    }

    private class PanelRegistration
    {
        public string Dock { get; set; }
        public float Width { get; set; }
        public int Order { get; set; }
        public string BackgroundColor { get; set; }
    }

    private class Panel
    {
        public PanelImage Image { get; set; }
        public PanelText Text { get; set; }
        public Hash<string, object> ToHash();
    }

    private abstract class PanelType
    {
        public bool Enabled { get; set; }
        public string Color { get; set; }
        public int Order { get; set; }
        public float Width { get; set; }
        public TypePadding Padding { get; set; }
        public virtual Hash<string, object> ToHash();
    }

    private class PanelImage : PanelType
    {
        public string Url { get; set; }
        public override Hash<string, object> ToHash();
    }

    private class PanelText : PanelType
    {
        public string Text { get; set; }
        public int FontSize { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        public TextAnchor TextAnchor { get; set; }
        public override Hash<string, object> ToHash();
    }

    private class TypePadding
    {
        public float Left { get; set; }
        public float Right { get; set; }
        public float Top { get; set; }
        public float Bottom { get; set; }
        public TypePadding(float left, float right, float top, float bottom);
        public Hash<string, object> ToHash();
    }

}

private class PluginConfig
{
    [JsonProperty(PropertyName = "Panel Settings")]
    public PanelRegistration PanelSettings { get; set; }
    [JsonProperty(PropertyName = "Panel Layout")]
    public Panel Panel { get; set; }
}

private class LangKeys
{
    public const string Today;
    public const string OneDay;
    public const string Days;
}

private class PanelRegistration
{
    public string Dock { get; set; }
    public float Width { get; set; }
    public int Order { get; set; }
    public string BackgroundColor { get; set; }
}

private class Panel
{
    public PanelImage Image { get; set; }
    public PanelText Text { get; set; }
    public Hash<string, object> ToHash();
}

private abstract class PanelType
{
    public bool Enabled { get; set; }
    public string Color { get; set; }
    public int Order { get; set; }
    public float Width { get; set; }
    public TypePadding Padding { get; set; }
    public virtual Hash<string, object> ToHash();
}

private class PanelImage : PanelType
{
    public string Url { get; set; }
    public override Hash<string, object> ToHash();
}

private class PanelText : PanelType
{
    public string Text { get; set; }
    public int FontSize { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    public TextAnchor TextAnchor { get; set; }
    public override Hash<string, object> ToHash();
}

private class TypePadding
{
    public float Left { get; set; }
    public float Right { get; set; }
    public float Top { get; set; }
    public float Bottom { get; set; }
    public TypePadding(float left, float right, float top, float bottom);
    public Hash<string, object> ToHash();
}


```

---

## ManageMini by DMB7 - Spawn a minicopter, own it, manage who can pilot it or not, see details, add upgrades, etc.

```csharp
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Newtonsoft.Json;
using System.Security;
using System.CodeDom.Compiler;

Oxide.Plugins
[Info("Manage Mini", "DMB7", "0.3.3"), Description("Manage, customize and own your minicopter!")]
 class ManageMini : RustPlugin
{
    private MiniData miniData;
    private ConfigFile configFile;
    private List<string> miniChatCommands;
    private System.Random rand;
    private int batteryMaxDrain;
    private string miniHelpPerm;
    private string makeMiniPerm;
    private string takeMiniPerm;
    private string getMiniPerm;
    private string ownMiniPerm;
    private string authPilotPerm;
    private string unauthPilotPerm;
    private string miniDetailsPerm;
    private string upgradeMiniPerm;
    private string upgradeTurretPerm;
    private string unlimitedFuel;
    private string randomFuelRate;
    private void Init();
     void Unload();
     void OnServerSave();
     void onEntityKill(MiniCopter mini);
     object CanMountEntity(BasePlayer player, BaseMountable mini);
     void OnPlayerConnected(BasePlayer player);
     void OnEntityDismounted(BaseMountable mini, BasePlayer player);
    [ChatCommand("minihelp")]
    private void miniHelp(BasePlayer player, string command, string[] args);
    [ChatCommand("makemini")]
    private void makeMini(BasePlayer player, string command, string[] args);
    [ChatCommand("takemini")]
    private void takeMini(BasePlayer player, string command, string[] args);
    [ChatCommand("getmini")]
    private void getMini(BasePlayer player, string command, string[] args);
    [ChatCommand("ownMini")]
    private void ownMini(BasePlayer player, string command, string[] args);
    [ChatCommand("authPilot")]
    private void authPilot(BasePlayer player, string command, string[] args);
    [ChatCommand("unauthPilot")]
    private void unauthPilot(BasePlayer player, string command, string[] args);
    [ChatCommand("miniDetails")]
    private void miniDetails(BasePlayer player, string command, string[] args);
    [ChatCommand("upgradeMini")]
    private void upgradeMini(BasePlayer player, string command, string[] args);
    private void registerPerms();
    private void registerWaitTimes();
    private bool checkPermissions(BasePlayer player, string perm);
    private void setChatHelp();
    private void createMini(BasePlayer player);
    private void setFuelRate(MiniCopter mini, float fuelPerSec);
    private void resetFuelRate(BasePlayer player, float fuelPerSec);
    private void takeOwnershipMini(BasePlayer player, MiniCopter mini);
    private bool checkWaitList(BasePlayer player);
    private MiniCopter getMinicopter(BasePlayer player);
    private bool setMiniLocation(BasePlayer player);
    private void takeBackMini(BasePlayer player);
    private BasePlayer getPlayer(ulong id);
    private BasePlayer getPlayer(string name);
    private void chatMessageArray(BasePlayer player, string msgType, string[] obj);
    private void chatMessage(BasePlayer player, string msgType, string message);
    private string chatAuthPilotList(BasePlayer player);
    private string chatMinicopterAge(BasePlayer player);
    private string[] getTimeDHMS(double var);
    private void setMiniData(BasePlayer player, uint num, int index);
    private uint getMiniData(BasePlayer player, int index);
    private List<string> getPilotsList(BasePlayer player);
    private bool addAuthPilot(BasePlayer player, string name);
    private bool removeAuthPilot(BasePlayer player, string name);
    private bool setFuelPerSec(BasePlayer player, float rate);
    private void upgradeElectricMini(BasePlayer player, BaseVehicle mini);
    private void removeElectricMini(BasePlayer player);
    private void addBatteriesMini(BasePlayer player, BaseVehicle mini);
    private void removeBatteriesMini(BasePlayer player);
    private void removeTurretMini(BasePlayer player);
    private void removeUpgrade(uint num);
    private uint addBatteries(BaseVehicle mini, float pos_x, float pos_y, float pos_z, float rot_x, float rot_y, float rot_z);
    private uint setElectricInput(BaseVehicle mini, float pos_x, float pos_y, float pos_z, float rot_x, float rot_y, float rot_z);
    private uint addTurretMini(BaseVehicle mini);
    private void setTurretRange(BasePlayer player, float range);
    private void setMiniRotation(BaseVehicle mini);
    private uint setItemUpgrade(string prefab, BaseVehicle mini, Vector3 location, float pos_x, float pos_y, float pos_z, float rot_x, float rot_y, float rot_z);
    private List<ElectricBattery> getBatteries(BasePlayer player);
    private int getTotalPower(List<ElectricBattery> batteries);
    private void setTotalPower(BasePlayer player, int power);
    private void setWireScheme(BasePlayer player);
    private void setWire(IOEntity output, int pin_O, IOEntity input, int pin_I);
    private void disableWire(BasePlayer player);
    private void disableGroundReq(BaseEntity entity);
    private int getFuelAmount(BaseVehicle mini);
    private void setFuelAmount(BaseVehicle mini, int amount);
    private void lockFuelTank(StorageContainer fuelTank, bool state);
    private void resetBatteriesDrain(BasePlayer player);
    private void fixUpgradeMini(BasePlayer player);
    private class MiniData
    {
        public Dictionary<ulong, OwnerMiniData> ownerMini;
        public Dictionary<ulong, string> errorLog;
    }

    [Serializable]
    private class OwnerMiniData
    {
        [JsonProperty]
        private uint miniEntityID;
        [JsonProperty]
        private DateTime manufactureData;
        [JsonProperty]
        private uint batteryLeftID;
        [JsonProperty]
        private uint batteryRightID;
        [JsonProperty]
        private uint batteryEngineID;
        [JsonProperty]
        private uint branchID;
        [JsonProperty]
        private uint autoTurretID;
        [JsonProperty]
        private float fuelPerSec;
        [JsonProperty]
        private List<string> authPilots;
        public uint getMiniEntityID();
        public DateTime getManufactureData();
        public uint getBranchID();
        public uint getBatteryLeftID();
        public uint getBatteryRightID();
        public uint getBatteryEngineID();
        public uint getAutoTurretID();
        public float getFuelPerSec();
        public List<string> getAuthPilots();
        public void setMiniEntityID(uint entityID);
        public void setManufactureData(DateTime data);
        public void setBranchID(uint entityID);
        public void setBatteryLeftID(uint entityID);
        public void setBatteryRightID(uint entityID);
        public void setBatteryEngineID(uint entityID);
        public void setAutoTurretID(uint entityID);
        public void setFuelPerSec(float rate);
        public void setAuthPilots(List<string> pilots);
        public bool hasMiniCopter();
        public bool hasBatteries();
        public bool hasElectricEngine();
        public bool hasAutoTurret();
    }

    private class ConfigFile
    {
        [JsonProperty("miniSpawnDistance")]
        public float miniSpawnDistance { get; set; }
        [JsonProperty("miniStartHealth")]
        public float miniStartHealth { get; set; }
        [JsonProperty("AssetPrefab")]
        public string assetPrefab { get; set; }
        [JsonProperty("CanSpawnBuildingBlocked")]
        public bool canSpawnBuildingBlocked { get; set; }
        [JsonProperty("WaitTimes")]
        public Dictionary<string, float> waitTimes { get; set; }
        [JsonProperty("FuelRate")]
        public float fuelPerSec { get; set; }
        [JsonProperty("FuelRateUnlimited")]
        public float fuelPerSecUnlimited { get; set; }
        [JsonProperty("FuelAmountUnlimited")]
        public int fuelAmountUnlimited { get; set; }
    }

    private ConfigFile GetDefaults();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
}

private class MiniData
{
    public Dictionary<ulong, OwnerMiniData> ownerMini;
    public Dictionary<ulong, string> errorLog;
}

[Serializable]
private class OwnerMiniData
{
    [JsonProperty]
    private uint miniEntityID;
    [JsonProperty]
    private DateTime manufactureData;
    [JsonProperty]
    private uint batteryLeftID;
    [JsonProperty]
    private uint batteryRightID;
    [JsonProperty]
    private uint batteryEngineID;
    [JsonProperty]
    private uint branchID;
    [JsonProperty]
    private uint autoTurretID;
    [JsonProperty]
    private float fuelPerSec;
    [JsonProperty]
    private List<string> authPilots;
    public uint getMiniEntityID();
    public DateTime getManufactureData();
    public uint getBranchID();
    public uint getBatteryLeftID();
    public uint getBatteryRightID();
    public uint getBatteryEngineID();
    public uint getAutoTurretID();
    public float getFuelPerSec();
    public List<string> getAuthPilots();
    public void setMiniEntityID(uint entityID);
    public void setManufactureData(DateTime data);
    public void setBranchID(uint entityID);
    public void setBatteryLeftID(uint entityID);
    public void setBatteryRightID(uint entityID);
    public void setBatteryEngineID(uint entityID);
    public void setAutoTurretID(uint entityID);
    public void setFuelPerSec(float rate);
    public void setAuthPilots(List<string> pilots);
    public bool hasMiniCopter();
    public bool hasBatteries();
    public bool hasElectricEngine();
    public bool hasAutoTurret();
}

private class ConfigFile
{
    [JsonProperty("miniSpawnDistance")]
    public float miniSpawnDistance { get; set; }
    [JsonProperty("miniStartHealth")]
    public float miniStartHealth { get; set; }
    [JsonProperty("AssetPrefab")]
    public string assetPrefab { get; set; }
    [JsonProperty("CanSpawnBuildingBlocked")]
    public bool canSpawnBuildingBlocked { get; set; }
    [JsonProperty("WaitTimes")]
    public Dictionary<string, float> waitTimes { get; set; }
    [JsonProperty("FuelRate")]
    public float fuelPerSec { get; set; }
    [JsonProperty("FuelRateUnlimited")]
    public float fuelPerSecUnlimited { get; set; }
    [JsonProperty("FuelAmountUnlimited")]
    public int fuelAmountUnlimited { get; set; }
}


```

---

## MapBlock by Jacob - Creates a black marker over the map

```csharp
using UnityEngine;

Oxide.Plugins
[Info("Map Block", "Jacob", "1.0.0")]
internal class MapBlock : RustPlugin
{
    private MapMarkerGenericRadius _marker;
    private void OnPlayerInit(BasePlayer player);
    private void OnServerInitialized();
    private void Unload();
    private void UpdateMarker();
}


```

---

## MapMyAirDrop by ColdUnwanted - Displays a popup on Cargo spawn/drop with distance, and a marker on in-game map at drop position

```csharp
using System.Collections.Generic;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Oxide.Core.Plugins;
using System.Linq;
using System;

Oxide.Plugins
[Info("Map My AirDrop", "ColdUnwanted", "0.1.0")]
[Description("Display a pop-up on Cargo Plane spawn, and a marker on in-game map at drop position.")]
public class MapMyAirDrop : RustPlugin
{
    private bool ConfigChanged;
    private float mapmarkerradius;
    private float mapMarkerAlpha;
    private string mapMarkerColor;
    private float mapMarkerLootedAlpha;
    private string mapMarkerLootedColor;
    private bool displayHudForAll;
    private bool displayGuiForAll;
    private int bannerHUDLimit;
    private Dictionary<string, string> bannerGUIData;
    private Dictionary<string, string> bannerHUDData;
    private Color markerColor;
    private Color markerLootedColor;
    private Dictionary<BaseEntity, MapMarkerGenericRadius> dropradius;
    private Dictionary<BaseEntity, bool> lootedornot;
    private Dictionary<BaseEntity, SupplyDrop> entsupply;
    private Dictionary<BaseEntity, Vector3> dropposition;
    private List<Vector3> supplySignalPosition;
    private const string MapMyAirdropHUD;
    private const string MapMyAirdropBanner;
    private Dictionary<BasePlayer, List<string>> HUDlist;
    private string hudAnchorMin;
    private string hudAnchorMax;
    private string hudOffsetMin;
    private string hudOffsetMax;
    private bool alreadyUpdating;
    private Dictionary<BasePlayer, List<string>> bannerlist;
    private string bannerAnchorMin;
    private string bannerAnchorMax;
    private string bannerOffsetMin;
    private string bannerOffsetMax;
    private static Dictionary<string, object> defaultBannerGUI();
    private static Dictionary<string, object> defaultBannerHUD();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private object GetConfig(string menu, string datavalue, object defaultValue);
    protected override void LoadDefaultMessages();
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void DestroyOneBanner(BasePlayer player);
     void DestroyOneHUD(BasePlayer player);
     void OnPlayerConnected(BasePlayer player);
     void Unload();
     void DestoyAllUi();
     void DestroyAllHUD();
     void DestroyAllBanner();
     void Init();
    private void FindAllDrops();
    private void GenerateMarkers();
    private void MarkerDisplayingDelete(BaseEntity Entity);
    private void DisplayDropHUD(string reason);
    private void OnSupplyDropLanded(SupplyDrop drop);
    private void OnExplosiveDropped(BasePlayer player, BaseEntity entity);
    private void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
    private void OnEntitySpawned(BaseEntity Entity);
     void OnEntityKill(BaseNetworkable entity);
     void OnLootEntity(BasePlayer player, BaseEntity entity);
     void DisplayBannerToAll(string reason);
}


```

---

## MapMyDeath by  - Displays a mapmarker and a popup with distance on player death

```csharp
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Convert = System.Convert;
using System;

Oxide.Plugins
[Info("Map My Death", "BuzZ[PHOQUE]", "0.0.4")]
[Description("Displays a mapmarker and a popup with distance on player death")]
public class MapMyDeath : RustPlugin
{
     class StoredData
    {
        public Dictionary<ulong, DeathInfo> playerdeath;
        public StoredData();
    }

     class DeathInfo
    {
        public float playerx;
        public float playery;
        public float playerz;
    }

    private StoredData storedData;
    public Dictionary<ulong, MapMarkerGenericRadius> MapMarker;
     bool debug;
     float markertime;
     bool ConfigChanged;
     string markertimeint;
     int meters;
     float daradius;
     void Init();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private object GetConfig(string menu, string datavalue, object defaultValue);
     void Unload();
     void OnPlayerDeath(BasePlayer player, HitInfo info);
     object CanNetworkTo(BaseNetworkable entity, BasePlayer target);
     void UpdatePos(BasePlayer player);
     void GetPos(BasePlayer player);
    private void OnPlayerSleepEnded(BasePlayer player);
}

 class StoredData
{
    public Dictionary<ulong, DeathInfo> playerdeath;
    public StoredData();
}

 class DeathInfo
{
    public float playerx;
    public float playery;
    public float playerz;
}


```

---

## MapMyPatrol by  - Adds red marker(s) on in-game map for Heli Patrol(s), HUD with details, and refresh on timer

```csharp
using System.Collections.Generic;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Oxide.Core.Plugins;
using Convert = System.Convert;
using System.Linq;

Oxide.Plugins
[Info("Map My Patrol", "BuzZ[PHOQUE]", "0.0.5")]
[Description("Adds red marker(s) on ingame map for Heli Patrol(s) position, HUD with details, and refresh on timer.")]
public class MapMyPatrol : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
     bool debug;
     int anim;
     string icondapatrol;
    private bool ConfigChanged;
    const string MapMyPatrolHUD;
     bool sound;
     bool bannerdisplay;
     string bannercolor;
     string HUDcolor;
     int HUDfontsize;
    public Dictionary<BaseHelicopter, MapMarkerGenericRadius> baseradius;
    public Dictionary<BaseHelicopter, Vector3> baseposition;
    public Dictionary<BaseHelicopter, string> basemessage;
    public Dictionary<BaseHelicopter, PatrolHelicopterAI> baseai;
    public Dictionary<BaseHelicopter, BaseCombatEntity> basecombat;
    public Dictionary<BaseHelicopter, BaseEntity> baseent;
    public Dictionary<string, BasePlayer> HUDlist;
    private Timer patrolrefresh;
     void Init();
     void Loaded();
     void Unload();
     void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private object GetConfig(string menu, string datavalue, object defaultValue);
     void MarkerDisplayingDelete(BaseHelicopter delpatrol);
     void GenerateMarkers();
    private void PatrolHUD();
    private void DestroyAllUiPlease();
    private void OnEntitySpawned(BaseEntity Entity);
     void AfterSpawn();
     void OnEntityKill(BaseCombatEntity Entity, HitInfo info);
     void DisplayBannerToAll(string reason);
}


```

---

## MapMyPlayers by 1AK1 - Displays all players positions with colored/texted markers on in-game map

```csharp
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Convert = System.Convert;
using CompanionServer.Handlers;

Oxide.Plugins
[Info("Map My Players", "1AK1", "1.1.1")]
[Description("Display all players positions with texted markers on ingame map (name/steamID/activ/sleepers) with autorefresh")]
public class MapMyPlayers : RustPlugin
{
     string Prefix;
     string PrefixColor;
     ulong SteamIDIcon;
     bool debug;
     float refreshrate;
     float MarkerRadius;
     bool ShowSteamID;
    const string MMPAdmin;
     bool ConfigChanged;
    private Timer mmptimer;
    public List<MapMarkerGenericRadius> PublicRadMarker;
    public List<VendingMachineMapMarker> PublicVendMarker;
    public Dictionary<ulong, string> activplayers;
    public Dictionary<ulong, string> sleepplayers;
    public Dictionary<ulong, Vector3> playerspos;
    private void Init();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private object GetConfig(string menu, string datavalue, object defaultValue);
     void Unload();
     object CanNetworkTo(MapMarkerGenericRadius marker, BasePlayer player);
     object CanNetworkTo(VendingMachineMapMarker marker, BasePlayer player);
    protected override void LoadDefaultMessages();
     void MarkerDisplayingDelete(BasePlayer player, string command, string[] args);
    private void ListPlayers();
    [ChatCommand("mmp_stop")]
    private void MapMyCommandStop(BasePlayer player, string command, string[] args);
    [ChatCommand("mmp_show")]
    private void MapMyCommand(BasePlayer player, string command, string[] args);
     void GenerateMarkers();
}


```

---

## MapNameChanger by Ryz0r - Custom Map Name allows you to change the "Custom Map" field to have predefined text in place of it!

```csharp
using System;
using System.Runtime.InteropServices;
using Newtonsoft.Json;
using Steamworks;

Oxide.Plugins
[Info("Custom Map Name", "Ryz0r", "1.0.2")]
[Description("Allows you to edit the Custom Map Name field without using a custom map.")]
public class CustomMapName : RustPlugin
{
    private Configuration _config;
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private string _cachedMapName;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Map Name Displayed")]
        public string MapNameDisplayed;
        [JsonProperty(PropertyName = "Cycle Map Names")]
        public bool CycleMapNames;
        [JsonProperty(PropertyName = "Map Names to Cycle", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public string[] MapNamesToCycle;
        [JsonProperty(PropertyName = "Name Refresh Interval")]
        public float NameRefreshInterval;
    }

    protected override void LoadConfig();
    private void Init();
    private void OnServerInformationUpdated();
    private static bool ContainsOfficial(string name);
    private static bool TooShort(string name);
    private bool NoNames();
}

private class Configuration
{
    [JsonProperty(PropertyName = "Map Name Displayed")]
    public string MapNameDisplayed;
    [JsonProperty(PropertyName = "Cycle Map Names")]
    public bool CycleMapNames;
    [JsonProperty(PropertyName = "Map Names to Cycle", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public string[] MapNamesToCycle;
    [JsonProperty(PropertyName = "Name Refresh Interval")]
    public float NameRefreshInterval;
}


```

---

## MapNoteTeleport by MONaH - Teleports player to marker on map when placed

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core;
using ProtoBuf;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("MapNote Teleport", "MON@H", "2.2.1")]
[Description("Teleports player to marker on map when placed.")]
public class MapNoteTeleport : CovalencePlugin
{
    private const string PermissionUse;
    private readonly List<ulong> _playersOnCooldown;
    private readonly List<ulong> _playersWithGodMode;
    private void Init();
    private void OnServerInitialized();
    private void OnServerSave();
    private void Unload();
    private ConfigData _configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Global settings")]
        public GlobalConfiguration GlobalSettings;
        [JsonProperty(PropertyName = "Chat settings")]
        public ChatConfiguration ChatSettings;
        public class GlobalConfiguration
        {
            [JsonProperty(PropertyName = "Use permissions")]
            public bool UsePermission;
            [JsonProperty(PropertyName = "Allow admins to use without permission")]
            public bool AdminsAllowed;
            [JsonProperty(PropertyName = "Default Enabled")]
            public bool DefaultEnabled;
            [JsonProperty(PropertyName = "Default Cooldown")]
            public float DefaultCooldown;
            [JsonProperty(PropertyName = "Maximum Cooldown")]
            public float MaximumCooldown;
            [JsonProperty(PropertyName = "Minimum Cooldown")]
            public float MinimumCooldown;
            [JsonProperty(PropertyName = "GodMode Cooldown")]
            public float GodModeCooldown;
            [JsonProperty(PropertyName = "Commands list")]
            public string[] Commands;
        }

        public class ChatConfiguration
        {
            [JsonProperty(PropertyName = "Chat steamID icon")]
            public ulong SteamIDIcon;
            [JsonProperty(PropertyName = "Notifications Enabled by default")]
            public bool DefaultNotification;
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private StoredData _storedData;
    private class StoredData
    {
        public readonly Dictionary<ulong, PlayerData> PlayerData;
    }

    public class PlayerData
    {
        public bool Enabled;
        public float Cooldown;
        public bool Notification;
    }

    private PlayerData GetPlayerData(ulong playerId, bool addToStored);
    private void LoadData();
    private void SaveData();
    private void ClearData();
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
    private void CmdMapNoteTeleport(IPlayer player, string command, string[] args);
    private void TeleportToNote(BasePlayer player, MapNote note);
    private string CheckPlayer(BasePlayer player);
    private void OnMapMarkerAdded(BasePlayer player, MapNote note);
    static float GetGroundPosition(Vector3 pos);
    private object OnEntityTakeDamage(BasePlayer player, HitInfo info);
    private void Print(IPlayer player, string message);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Global settings")]
    public GlobalConfiguration GlobalSettings;
    [JsonProperty(PropertyName = "Chat settings")]
    public ChatConfiguration ChatSettings;
    public class GlobalConfiguration
    {
        [JsonProperty(PropertyName = "Use permissions")]
        public bool UsePermission;
        [JsonProperty(PropertyName = "Allow admins to use without permission")]
        public bool AdminsAllowed;
        [JsonProperty(PropertyName = "Default Enabled")]
        public bool DefaultEnabled;
        [JsonProperty(PropertyName = "Default Cooldown")]
        public float DefaultCooldown;
        [JsonProperty(PropertyName = "Maximum Cooldown")]
        public float MaximumCooldown;
        [JsonProperty(PropertyName = "Minimum Cooldown")]
        public float MinimumCooldown;
        [JsonProperty(PropertyName = "GodMode Cooldown")]
        public float GodModeCooldown;
        [JsonProperty(PropertyName = "Commands list")]
        public string[] Commands;
    }

    public class ChatConfiguration
    {
        [JsonProperty(PropertyName = "Chat steamID icon")]
        public ulong SteamIDIcon;
        [JsonProperty(PropertyName = "Notifications Enabled by default")]
        public bool DefaultNotification;
    }

}

public class GlobalConfiguration
{
    [JsonProperty(PropertyName = "Use permissions")]
    public bool UsePermission;
    [JsonProperty(PropertyName = "Allow admins to use without permission")]
    public bool AdminsAllowed;
    [JsonProperty(PropertyName = "Default Enabled")]
    public bool DefaultEnabled;
    [JsonProperty(PropertyName = "Default Cooldown")]
    public float DefaultCooldown;
    [JsonProperty(PropertyName = "Maximum Cooldown")]
    public float MaximumCooldown;
    [JsonProperty(PropertyName = "Minimum Cooldown")]
    public float MinimumCooldown;
    [JsonProperty(PropertyName = "GodMode Cooldown")]
    public float GodModeCooldown;
    [JsonProperty(PropertyName = "Commands list")]
    public string[] Commands;
}

public class ChatConfiguration
{
    [JsonProperty(PropertyName = "Chat steamID icon")]
    public ulong SteamIDIcon;
    [JsonProperty(PropertyName = "Notifications Enabled by default")]
    public bool DefaultNotification;
}

private class StoredData
{
    public readonly Dictionary<ulong, PlayerData> PlayerData;
}

public class PlayerData
{
    public bool Enabled;
    public float Cooldown;
    public bool Notification;
}


```

---

## MarkerDistance by Notchu - Show distance to placed markers

```csharp
using System;
using System.Collections.Generic;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using ProtoBuf;
using UnityEngine;

[Info("Marker Distance", "Notchu", "1.2.1")]
[Description("Shows distance to placed marker")]
internal class MarkerDistance : RustPlugin
{
    private class Configuration
    {
        [JsonProperty("Text Color")]
        public string TextColor;
        [JsonProperty("Font Size")]
        public int FontSize;
        [JsonProperty("Background Color")]
        public string BackgroundColor;
        [JsonProperty("Anchor Min")]
        public string AnchorMin;
        [JsonProperty("Anchor Max")]
        public string AnchorMax;
        [JsonProperty("Refresh Interval")]
        public float Interval;
        [JsonProperty("Offset Min")]
        public string OffsetMin;
        [JsonProperty("Offset Max")]
        public string OffsetMax;
    }

    private Configuration _config;
    private Timer _distanceTimer;
    private HashSet<BasePlayer> _users;
    private readonly Dictionary<int, string> _colorCodes;
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
     void Init();
     void Unload();
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnMapMarkerAdded(BasePlayer player, MapNote note);
    private void ShowDistance();
    private bool DoesHavePermission(string id);
}

private class Configuration
{
    [JsonProperty("Text Color")]
    public string TextColor;
    [JsonProperty("Font Size")]
    public int FontSize;
    [JsonProperty("Background Color")]
    public string BackgroundColor;
    [JsonProperty("Anchor Min")]
    public string AnchorMin;
    [JsonProperty("Anchor Max")]
    public string AnchorMax;
    [JsonProperty("Refresh Interval")]
    public float Interval;
    [JsonProperty("Offset Min")]
    public string OffsetMin;
    [JsonProperty("Offset Max")]
    public string OffsetMax;
}


```

---

## MarkerManager by DezLife - Allows to place markers on in-game map

```csharp
using ConVar;
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Text;
using UnityEngine;
using VLB;
using Color = UnityEngine.Color;

Oxide.Plugins
[Info("Marker Manager", "DezLife", "3.0.3")]
public class MarkerManager : RustPlugin
{
    public static StringBuilder StringBuilderInstance;
    private const string genericPrefab;
    private const string vendingPrefab;
    private const string permUse;
    private const string chatCommand;
    private readonly List<CustomMapMarker> mapMarkers;
    private void Init();
    private void OnPlayerConnected(BasePlayer player);
    private void OnServerInitialized();
    private void Unload();
    private void CreateMarker(Vector3 position, int duration, float refreshRate, string name, string displayName, float radius, float alpha, string colorMarker, string colorOutline);
    private void CreateMarker(BaseEntity entity, int duration, float refreshRate, string name, string displayName, float radius, float alpha, string colorMarker, string colorOutline);
    private void RemoveMarker(string name);
    private void RemoveMarkers();
    private void RemoveCustomMarker(string name, BasePlayer player);
    private void CreateCustomMarker(CachedMarker def, BasePlayer player);
    private void SaveCustomMarker(CachedMarker def);
    private void LoadCustomMarkers();
    private void RemoveSavedMarker(string name);
    private void cmdMarkerChat(BasePlayer player, string command, string[] args);
    private const string filename;
    private static List<CachedMarker> data;
    private class CachedMarker
    {
        public float radius;
        public float alpha;
        public string color1;
        public string color2;
        public string displayName;
        public string name;
        public float refreshRate;
        public Vector3 position;
        public int duration;
    }

    private void LoadData();
    private void SaveData();
    private void API_CreateMarker(Vector3 position, string name, int duration, float refreshRate, float radius, string displayName, string colorMarker, string colorOutline, float alpha);
    private void API_CreateMarker(BaseEntity entity, string name, int duration, float refreshRate, float radius, string displayName, string colorMarker, string colorOutline, float alpha);
    private void API_RemoveMarker(string name);
    protected override void LoadDefaultMessages();
    public string GetLang(string LangKey, string userID, object[] args);
    private void Message(BasePlayer player, string messageKey, object[] args);
    private class CustomMapMarker : MonoBehaviour
    {
        private VendingMachineMapMarker vending;
        private MapMarkerGenericRadius generic;
        public BaseEntity parent;
        private bool asChild;
        public new string name;
        public float radius;
        public float alpha;
        public Color color1;
        public Color color2;
        public string displayName;
        public float refreshRate;
        public Vector3 position;
        public int duration;
        public bool placedByPlayer;
        private void Start();
        private void CreateMarkers();
        private void UpdatePosition();
        public void UpdateMarkers();
        private void DestroyMakers();
        private void OnDestroy();
    }

}

private class CachedMarker
{
    public float radius;
    public float alpha;
    public string color1;
    public string color2;
    public string displayName;
    public string name;
    public float refreshRate;
    public Vector3 position;
    public int duration;
}

private class CustomMapMarker : MonoBehaviour
{
    private VendingMachineMapMarker vending;
    private MapMarkerGenericRadius generic;
    public BaseEntity parent;
    private bool asChild;
    public new string name;
    public float radius;
    public float alpha;
    public Color color1;
    public Color color2;
    public string displayName;
    public float refreshRate;
    public Vector3 position;
    public int duration;
    public bool placedByPlayer;
    private void Start();
    private void CreateMarkers();
    private void UpdatePosition();
    public void UpdateMarkers();
    private void DestroyMakers();
    private void OnDestroy();
}


```

---

## Martyrdom by k1lly0u - Drops a grenade/beancan grenade/timed explosive on death

```csharp
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Martyrdom", "k1lly0u", "0.2.1")]
[Description("Much like the Martyrdom perk in COD, allows players with permission to drop various explosives when they are killed")]
 class Martyrdom : RustPlugin
{
    private Dictionary<ulong, ExplosiveType> registeredMartyrs;
    private Dictionary<ExplosiveType, ExplosiveInfo> explosiveInfo;
    private void Init();
    private void OnServerInitialized();
    private void OnEntityDeath(BaseEntity entity, HitInfo info);
    private void TryDropExplosive(BasePlayer player);
    private void CreateExplosive(ExplosiveType type, BasePlayer player);
    private bool HasEnoughRes(BasePlayer player, int itemid, int amount);
    private void TakeResources(BasePlayer player, int itemid, int amount);
    private bool HasPerm(BasePlayer player, ExplosiveType type);
    private bool HasAnyPerm(BasePlayer player);
    private void SetExplosiveInfo();
    private class ExplosiveInfo
    {
        public int ItemID;
        public string PrefabName;
        public float Damage;
        public float Radius;
        public float Fuse;
    }

    [ChatCommand("m")]
    private void cmdM(BasePlayer player, string command, string[] args);
    private ConfigData configData;
    private class ConfigData
    {
        public ExpType Grenade { get; set; }
        public ExpType Beancan { get; set; }
        public ExpType Explosive { get; set; }
        public class ExpType
        {
            public bool Activated { get; set; }
            public float Damage { get; set; }
            public float Radius { get; set; }
            public float Fuse { get; set; }
        }

    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void SaveConfig(ConfigData config);
    private string msg(string key, string id);
    private Dictionary<string, string> messages;
}

private class ExplosiveInfo
{
    public int ItemID;
    public string PrefabName;
    public float Damage;
    public float Radius;
    public float Fuse;
}

private class ConfigData
{
    public ExpType Grenade { get; set; }
    public ExpType Beancan { get; set; }
    public ExpType Explosive { get; set; }
    public class ExpType
    {
        public bool Activated { get; set; }
        public float Damage { get; set; }
        public float Radius { get; set; }
        public float Fuse { get; set; }
    }

}

public class ExpType
{
    public bool Activated { get; set; }
    public float Damage { get; set; }
    public float Radius { get; set; }
    public float Fuse { get; set; }
}


```

---

## MassGenocide by Ryz0r - Mass Genocide is a plugin that allows users with permission to kill every player on the server.

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Mass Genocide", "Ryz0r", "1.0.2")]
[Description("Allows players with permission to kill all players at once.")]
public class MassGenocide : RustPlugin
{
    private const string NoKillPerm;
    private const string UsePerm;
    private void Init();
    protected override void LoadDefaultMessages();
    private void GenocideCommand(IPlayer player, string command, string[] args);
}


```

---

## MasterKey by FastBurst - Gain access and/or authorization to any locked object with permissions.

```csharp
using System;
using System.Collections.Generic;
using ProtoBuf;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Master Key", "FastBurst", "0.7.8")]
[Description("Gain access and/or authorization to any locked object.")]
public class MasterKey : CovalencePlugin
{
    [PluginReference]
     Plugin LockMaster;
    private readonly DynamicConfigFile dataFile;
    private readonly string[] lockableTypes;
    private Dictionary<string, bool> playerPrefs;
    private const string permCom;
    private bool logUsage;
    private bool showMessages;
    private new void LoadDefaultConfig();
    private void Init();
    private new void LoadDefaultMessages();
    [Command("masterkey", "mkey", "mk")]
    private void ChatCommand(IPlayer player, string command, string[] args);
    private object CanUseLockedEntity(BasePlayer player, BaseLock @lock);
    private object CanUnlock(BasePlayer player, BaseLock @lock);
    private object CanLock(BasePlayer player, BaseLock @lock);
    private object CanHackCrate(BasePlayer player, HackableLockedCrate crate);
     void OnPlayerInput(BasePlayer player, InputState input);
    private PlayerNameID GrabPlayer(BasePlayer player);
    private BaseEntity GetEntity(BasePlayer player);
    private string PlayerName(BasePlayer player);
    private T GetConfig(string name, T value);
    private string Lang(string key, string id, object[] args);
    private void Log(string text);
}


```

---

## MasterLock by S0N0FBISCUIT - Gives players the ability to control all code locks in their base from the tool cupboard

```csharp
using Oxide.Core;
using Oxide.Core.Plugins;
using System.Collections.Generic;

Oxide.Plugins
[Info("MasterLock", "S0N_0F_BISCUIT", "1.0.2")]
[Description("Control all locks in a base with the tool cupboard.")]
 class MasterLock : RustPlugin
{
    [PluginReference]
     Plugin GameTipAPI;
     class StoredData
    {
        public uint seed;
        public Dictionary<uint, bool> buildings { get; set; }
    }

    private StoredData data;
    private bool initialized;
    private new void LoadDefaultMessages();
    private void Init();
     void OnServerInitialized();
    protected override void LoadDefaultConfig();
    private object ConfigValue(string value);
    private void LoadData();
    private void SaveData();
    private void FindBuildingPrivileges();
    [ChatCommand("masterlock")]
     void ToggleMasterLock(BasePlayer player, string command, string[] args);
    [ChatCommand("opendoors")]
     void OpenConnectedDoors(BasePlayer player, string command, string[] args);
    [ChatCommand("closedoors")]
     void CloseConnectedDoors(BasePlayer player, string command, string[] args);
     void OnEntitySpawned(BaseNetworkable entity);
     object OnCupboardAuthorize(BuildingPrivlidge privilege, BasePlayer player);
     object OnCupboardClearList(BuildingPrivlidge privilege, BasePlayer player);
     object OnCupboardDeauthorize(BuildingPrivlidge privilege, BasePlayer player);
     void CanChangeCode(CodeLock codeLock, BasePlayer player, string newCode, bool isGuestCode);
    private string Lang(string key, string userId, object[] args);
    private void InitializeMasterLock(BuildingPrivlidge privilege);
     void AddAuthorization(BuildingPrivlidge privilege, BasePlayer player, BasePlayer caller);
     void RemoveAuthorization(BuildingPrivlidge privilege, BasePlayer player);
     void ClearAuthorizations(BuildingPrivlidge privilege);
    private uint UpdateCode(BuildingPrivlidge privilege, string newCode);
    private void OpenDoors(BuildingPrivlidge privilege, BasePlayer player);
    private void CloseDoors(BuildingPrivlidge privilege, BasePlayer player);
    private void ShowGameTip(BasePlayer player, string tip);
}

 class StoredData
{
    public uint seed;
    public Dictionary<uint, bool> buildings { get; set; }
}


```

---

## MasterSwitch by Lincoln - Toggle stuff with a single command

```csharp
using CompanionServer;
using Facepunch;
using System;
using System.Collections.Generic;
using UnityEngine;
using WebSocketSharp;
using static BaseEntity;

Oxide.Plugins
[Info("Master Switch", "Lincoln", "1.1.0")]
[Description("Toggle things on or off with a command.")]
public class MasterSwitch : RustPlugin
{
    private const float MaxValue;
    private const float MinValue;
    private const string PermUse;
    private float radius;
    private int requiredPower;
    private readonly List<Timer> doorTimerList;
    private void Init();
    private bool HasPermission(BasePlayer player);
    private bool HasArgs(BasePlayer player, string[] args);
    private bool HasRadius(BasePlayer player, float radius);
    protected override void LoadDefaultMessages();
    private List<BaseEntity> FindBaseEntity(Vector3 pos, float radius);
    [ChatCommand("ms")]
    private void ToggleCommand(BasePlayer player, string command, string[] args);
    private void ToggleDoors(List<BaseEntity> entities, List<string> entityCount, bool open);
    private void ToggleLightsAndDevices(List<BaseEntity> entities, List<string> entityCount, bool on);
    private void ToggleTurrets(List<BaseEntity> entities, List<string> entityCount, bool on);
    private void ToggleBearTraps(List<BaseEntity> entities, List<string> entityCount, bool arm);
    private void IgniteEntities(List<BaseEntity> entities, List<string> entityCount);
    private void ExplodeEntities(List<BaseEntity> entities, List<string> entityCount);
    private void ToggleMachines(List<BaseEntity> entities, List<string> entityCount, bool on);
}


```

---

## MaxCupboardAuths by redBDGR - Limits how many tool cupboards each player can authorize to

```csharp
using System;
using System.Linq;
using System.Reflection;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Facepunch;

Oxide.Plugins
[Info("MaxCupboardAuths", "Krungh Crow", "2.0.1")]
[Description("Limit cupboard max authing")]
 class MaxCupboardAuths : RustPlugin
{
     bool debug;
    const ulong chaticon;
    const string prefix;
    const string Max_Perm;
    const string MaxTC_Perm;
    const string MaxTCDef_Perm;
    const string MaxTCVip_Perm;
    const string Bypass_Perm;
     void Init();
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Put Debug to console")]
        public bool UseDebug;
        [JsonProperty(PropertyName = "Settings ToolCupboard")]
        public SettingsTCAuth TCAuths;
        [JsonProperty(PropertyName = "Settings Players")]
        public SettingsPlayerAuth PlayerAuths;
    }

     class SettingsTCAuth
    {
        [JsonProperty(PropertyName = "Max total Auths for each TC")]
        public int MaxTCAuth;
    }

     class SettingsPlayerAuth
    {
        [JsonProperty(PropertyName = "Max Auths for default Player")]
        public int MaxAuth;
        [JsonProperty(PropertyName = "Max Auths for vip Player")]
        public int MaxAuthVip;
    }

    private bool LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConf();
    protected override void LoadDefaultMessages();
     void OnEntitySpawned(BaseEntity entity, UnityEngine.GameObject gameObject);
     object OnCupboardAuthorize(BuildingPrivlidge privilege, BasePlayer player);
    private int TCcount(BasePlayer player);
    private string msg(string key, string id);
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Put Debug to console")]
    public bool UseDebug;
    [JsonProperty(PropertyName = "Settings ToolCupboard")]
    public SettingsTCAuth TCAuths;
    [JsonProperty(PropertyName = "Settings Players")]
    public SettingsPlayerAuth PlayerAuths;
}

 class SettingsTCAuth
{
    [JsonProperty(PropertyName = "Max total Auths for each TC")]
    public int MaxTCAuth;
}

 class SettingsPlayerAuth
{
    [JsonProperty(PropertyName = "Max Auths for default Player")]
    public int MaxAuth;
    [JsonProperty(PropertyName = "Max Auths for vip Player")]
    public int MaxAuthVip;
}


```

---

## MazeGen by Razor - Creates mazes via commands

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("MazeGen", "Default/Razor", "0.2.7")]
[Description("Creates mazes of various sizes for your players to solve.")]
 class MazeGen : RustPlugin
{
    private System.Random random;
    private const string gen;
    private static MazeGen instance;
    private List<Block> Grid;
    private float BlockSize;
    private float FoundationSize;
    private float BuldingHeight;
    private Vector3 RealGoal;
    private bool MakeRoof;
    private void Init();
    private void LoadMessages();
    [ChatCommand("maze.delete")]
     void dmaze(BasePlayer player, string command, string[] args);
    [ChatCommand("maze")]
     void GenerateMaze(BasePlayer player, string command, string[] args);
    private void BuildGrid(BasePlayer player);
    private void GeneratePath();
    private void SpawnMaze();
    private void FindNext(Block block, bool New, Vector3 Goal, int RougCounter);
     List<Block> LastVisited;
    private void OpenWall(Block from, Block to);
    private float ManhattanDistance(Vector3 ToPosition, Vector3 Goal, Vector3 FromPosition);
    private List<Block> GetNeighbours(Block block, bool IsVisited);
    public class Block
    {
        public Vector3 Position { get; set; }
        public Vector3 BasePosition { get; set; }
        public Quaternion Rotation { get; set; }
        public BaseEntity Entity { get; set; }
        public int EntityId { get; set; }
        public List<Block> Walls { get; set; }
        public Block Roof { get; set; }
        public Block Stair { get; set; }
        public Block FloorFrame { get; set; }
        public Block LadderHatch { get; set; }
        public int Weight { get; set; }
        public float foundationSize { get; set; }
        public float buildingHeight { get; set; }
        public bool DisplayBlock { get; set; }
        public bool visited { get; set; }
        public Block(Vector3 posion, Vector3 basePosition, Quaternion rotation, float FoundationSize, float BuildingHeight, Prefab prefab, int randomNumber, bool MakeRoof);
        public bool IsTheEnd(Vector3 Goal);
        public void Destroye();
        public void SpawnEntitys();
        public void AddStair();
        private void AddWalls();
        private void CreateEntity(Prefab prefab);
        private void AddEntity(BaseEntity entity);
    }

}

public class Block
{
    public Vector3 Position { get; set; }
    public Vector3 BasePosition { get; set; }
    public Quaternion Rotation { get; set; }
    public BaseEntity Entity { get; set; }
    public int EntityId { get; set; }
    public List<Block> Walls { get; set; }
    public Block Roof { get; set; }
    public Block Stair { get; set; }
    public Block FloorFrame { get; set; }
    public Block LadderHatch { get; set; }
    public int Weight { get; set; }
    public float foundationSize { get; set; }
    public float buildingHeight { get; set; }
    public bool DisplayBlock { get; set; }
    public bool visited { get; set; }
    public Block(Vector3 posion, Vector3 basePosition, Quaternion rotation, float FoundationSize, float BuildingHeight, Prefab prefab, int randomNumber, bool MakeRoof);
    public bool IsTheEnd(Vector3 Goal);
    public void Destroye();
    public void SpawnEntitys();
    public void AddStair();
    private void AddWalls();
    private void CreateEntity(Prefab prefab);
    private void AddEntity(BaseEntity entity);
}


```

---

## MedicalToolTweaker by  - Modifies medical tools attributes (syringe/bandage)

```csharp
using System.Collections.Generic;
using Convert = System.Convert;

Oxide.Plugins
[Info("Medical Tool Tweaker", "BuzZ", "0.0.1")]
[Description("Modify medical tools variables")]
public class MedicalToolTweaker : RustPlugin
{
     bool debug;
    private bool ConfigChanged;
    const string MedicalTweaker;
     float maxdistanceother;
     float healdurationother;
     bool canuseonother;
     float healdurationself;
     bool canrevive;
     void Init();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private object GetConfig(string menu, string datavalue, object defaultValue);
     void OnHealingItemUse(MedicalTool tool, BasePlayer player);
}


```

---

## MedicRevive by k1lly0u - Pick players up from being wounded by injecting them with a medical syringe

```csharp

Oxide.Plugins
[Info("MedicRevive", "k1lly0u", "0.1.0", ResourceId = 0)]
 class MedicRevive : RustPlugin
{
     void Loaded();
     void OnHealingItemUse(MedicalTool tool, BasePlayer player);
}


```

---

## MegaDrones by WhiteThunder - Allows players to spawn large versatile drones with computer stations attached to them

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Rust;
using System.Collections.Generic;
using System;
using System.Linq;
using System.Text;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("Mega Drones", "WhiteThunder", "0.2.10")]
[Description("Allows players to spawn large drones with computer stations attached to them.")]
internal class MegaDrones : CovalencePlugin
{
    [PluginReference]
    private readonly Plugin DroneScaleManager;
    private readonly Plugin EntityScaleManager;
    private readonly Plugin VehicleDeployedLocks;
    private Configuration _config;
    private StoredData _data;
    private const string PermissionSpawn;
    private const string PermissionFetch;
    private const string PermissionDestroy;
    private const string PermissionGive;
    private const string PermissionCooldownPrefix;
    private const string CommandName_MegaDrone;
    private const string CommandName_GiveMegaDrone;
    private const string SubCommandName_Help;
    private const string SubCommandName_Fetch;
    private const string SubCommandName_Destroy;
    private const float MegaDroneScale;
    private const int DroneItemId;
    private const int ComputerStationItemId;
    private const int CCTVItemId;
    private const BaseEntity.Slot MegaDroneSlot;
    private const string DronePrefab;
    private const string ComputerStationPrefab;
    private const string ComputerStationDeployEffectPrefab;
    private const string CCTVPrefab;
    private const string CCTVDeployEffectPrefab;
    private static readonly Vector3 ComputerStationLocalPosition;
    private static readonly Quaternion ComputerStationLocalRotation;
    private static readonly Vector3 CameraLocalPosition;
    private static readonly Quaternion CameraLocalRotation;
    private static readonly Vector3 LockPosition;
    private static readonly Quaternion LockRotation;
    private static readonly Vector3 DroneExtents;
    private readonly object True;
    private readonly object False;
    private const int BoxcastLayers;
    private DynamicHookHashSet<NetworkableId> _megaDroneTracker;
    private DynamicHookHashSet<ulong> _droneMounteeTracker;
    private DynamicHookHashSet<ulong> _droneControllerTracker;
    public MegaDrones();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnServerSave();
    private void OnNewSave();
    private void OnPluginLoaded(Plugin plugin);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnServerCommand(ConsoleSystem.Arg arg);
    private object OnEntityTakeDamage(ComputerStation station, HitInfo hitInfo);
    private object OnEntityTakeDamage(CCTV_RC camera, HitInfo hitInfo);
    private object HandleOnEntityTakeDamage(BaseEntity entity, HitInfo hitInfo);
    private object canRemove(BasePlayer player, Drone drone);
    private object canRemove(BasePlayer player, ComputerStation station);
    private object canRemove(BasePlayer player, CCTV_RC camera);
    private object HandleCanRemove(BaseEntity entity);
    private void OnEntityKill(Drone drone);
    private void OnBookmarkControlStarted(ComputerStation station, BasePlayer player, string bookmarkName, Drone drone);
    private void OnBookmarkControlStarted(ComputerStation station, BasePlayer player, string bookmarkName, CCTV_RC camera);
    private void OnBookmarkControlEnded(ComputerStation station, BasePlayer player, Drone drone);
    private void OnBookmarkControlEnded(ComputerStation station, BasePlayer player, CCTV_RC camera);
    private void OnEntityMounted(ComputerStation station, BasePlayer player);
    private void OnEntityDismounted(ComputerStation station, BasePlayer player);
    private void OnPlayerDismountFailed(BasePlayer player, ComputerStation station);
    private object OnCCTVDirectionChange(CCTV_RC camera);
    private void OnVehicleLockDeployed(ComputerStation computerStation, BaseLock baseLock);
    private string OnDroneTypeDetermine(Drone drone);
    private object OnCCTVMovableBecome(CCTV_RC camera);
    private object OnDroneRangeLimit(Drone drone);
    [HookMethod(nameof(API_SpawnMegaDrone))]
    public Drone API_SpawnMegaDrone(BasePlayer player);
    [Command(CommandName_MegaDrone)]
    private void CommandMegaDrone(IPlayer player, string cmd, string[] args);
    private void SubCommand_Help(IPlayer player, string cmd);
    private void SubCommand_Spawn(IPlayer player, string cmd);
    private void FetchInternal(IPlayer player, Drone drone);
    private void SubCommand_Fetch(IPlayer player);
    private void SubCommand_Destroy(IPlayer player);
    [Command(CommandName_GiveMegaDrone)]
    private void CommandGiveMegaDrone(IPlayer player, string cmd, string[] args);
    private static class RCUtils
    {
        public static bool HasController(IRemoteControllable controllable, BasePlayer player);
        public static bool HasRealController(IRemoteControllable controllable);
        public static bool CanControl(BasePlayer player, IRemoteControllable controllable);
        public static void RemoveController(IRemoteControllable controllable);
        public static bool AddViewer(IRemoteControllable controllable, BasePlayer player);
    }

    private bool VerifyPermission(IPlayer player, string perm);
    private bool VerifyHasNoDrone(IPlayer player, string cmd);
    private bool VerifyHasDrone(IPlayer player, Drone drone);
    private bool VerifyCanBuild(IPlayer player);
    private bool VerifyDroneNotOccupied(IPlayer player, Drone drone);
    private bool VerifyOffCooldown(IPlayer player, CooldownType cooldownType);
    private bool VerifySufficientSpace(IPlayer player, Vector3 determinedPosition, Quaternion determinedRotation);
    private bool VerifyCanInteract(IPlayer player);
    private bool VerifyNotMounted(IPlayer player);
    public static void LogInfo(string message);
    public static void LogError(string message);
    public static void LogWarning(string message);
    private bool VerifyDependencies();
    private void RegisterWithVehicleDeployedLocks();
    private static bool SpawnMegaDroneWasBlocked(BasePlayer player);
    private static bool FetchMegaDroneWasBlocked(BasePlayer player, Drone drone);
    private static bool DestroyMegaDroneWasBlocked(BasePlayer player, Drone drone);
    private BaseEntity GetRootEntity(Drone drone);
    private static string GetCooldownPermission(string permissionSuffix);
    public bool IsMegaDrone(Drone drone);
    public bool IsMegaDrone(Drone drone, string userIdString);
    public bool IsPlayerMegaDrone(Drone drone);
    private Drone GetParentMegaDrone(BaseEntity entity, BaseEntity rootEntity);
    private Drone GetParentMegaDrone(BaseEntity entity);
    private bool ParentEntityToDrone(Drone drone, BaseEntity entity);
    private static T GetChildOfType(BaseEntity entity);
    private ComputerStation GetComputerStation(Drone drone);
    private CCTV_RC GetCamera(Drone drone);
    private static void StartControlling(BasePlayer player, ComputerStation station, IRemoteControllable controllable);
    private bool TryMountPlayer(Drone drone, BasePlayer player);
    private static bool IsDroneEligible(Drone drone);
    private static void HitNotify(BaseEntity entity, HitInfo info);
    private static void SetupComputerStation(Drone drone, ComputerStation station);
    private ComputerStation DeployComputerStation(Drone drone, BasePlayer player);
    private static void SetupCamera(Drone drone, CCTV_RC camera);
    private CCTV_RC DeployCamera(Drone drone, BasePlayer player, int idNumber);
    private static Quaternion GetPlayerWorldRotation(BasePlayer player);
    private static Vector3 GetPlayerForwardPosition(BasePlayer player);
    private static Vector3 GetPlayerRelativeSpawnPosition(BasePlayer player);
    private static Quaternion GetPlayerRelativeSpawnRotation(BasePlayer player);
    private void SetupDrone(Drone drone);
    private static int SetRandomIdentifier(IRemoteControllable controllable, string prefix);
    private static void RegisterIdentifier(ComputerStation station, IRemoteControllable controllable);
    private Drone SpawnMegaDrone(BasePlayer player, bool shouldTrack);
    private static void RemoveGroundWatch(BaseEntity entity);
    private static void RunOnEntityBuilt(Item item, BaseEntity entity);
    private static void RunOnEntityBuilt(BasePlayer basePlayer, BaseEntity entity, int itemid);
    private static string FormatTime(double seconds);
    private void DismountAllPlayersFromDrone(Drone drone);
    private Drone FindPlayerDrone(string userId);
    private BasePlayer GetMountedPlayer(Drone drone);
    private bool HasChildPlayer(Drone drone);
    private void RefreshMegaDrone(Drone drone);
    private void RefreshAllMegaDrones();
    private string DetermineSubCommand(string argLower);
    private long GetRemainingCooldownSeconds(string userId, CooldownType cooldownType);
    private void StartCooldown(string userId, CooldownType cooldownType);
    private class HookCollection
    {
        public bool IsSubscribed { get; set; }
        private readonly MegaDrones _plugin;
        private readonly string[] _hookNames;
        private readonly Func<bool> _shouldSubscribe;
        public HookCollection(MegaDrones plugin, Func<bool> shouldSubscribe, string[] hookNames);
        public void Subscribe();
        public void Unsubscribe();
        public void Refresh();
    }

    private class DynamicHookHashSet : HashSet<T>
    {
        private readonly HookCollection _hookCollection;
        public DynamicHookHashSet(MegaDrones plugin, string[] hookNames);
        public new bool Add(T item);
        public new bool Remove(T item);
        public void Unsubscribe();
    }

    private class CameraMovement : EntityComponent<BasePlayer>
    {
        public static CameraMovement AddToPlayer(BasePlayer player, Drone drone);
        public static void RemoveFromPlayer(BasePlayer player);
        private Drone _drone;
        private CameraMovement SetDrone(Drone drone);
        private void Update();
    }

    private class StoredData
    {
        [JsonProperty("PlayerDrones")]
        public Dictionary<string, ulong> PlayerDrones;
        [JsonProperty("OtherDrones")]
        public HashSet<ulong> OtherDrones;
        [JsonProperty("Cooldowns")]
        public CooldownManager Cooldowns;
        public static StoredData Load();
        public static StoredData Reset();
        public StoredData Save();
        public HashSet<ulong> GetAllMegaDroneIds();
        public bool IsMegaDrone(Drone drone, string userIdString);
        public bool IsMegaDrone(Drone drone);
        public void RegisterPlayerDrone(string userId, Drone drone);
        public void UnregisterPlayerDrone(string userId);
        public void RegisterOtherDrone(Drone drone);
        public void UnregisterOtherDrone(Drone drone);
    }

    private class CooldownManager
    {
        [JsonProperty("Spawn")]
        private Dictionary<string, long> Spawn;
        [JsonProperty("Fetch")]
        private Dictionary<string, long> Fetch;
        public Dictionary<string, long> GetCooldownMap(CooldownType cooldownType);
        public void ClearAll();
    }

    private class Configuration : BaseConfiguration
    {
        [JsonProperty("AutoMount")]
        public bool AutoMount;
        [JsonProperty("AutoFetch")]
        public bool AutoFetch;
        [JsonProperty("DroneIdentifierPrefix")]
        public string DroneIdentifierPrefix;
        [JsonProperty("CamIdentifierPrefix")]
        public string CamIdentifierPrefix;
        [JsonProperty("CanSpawnWhileBuildingBlocked")]
        public bool CanSpawnBuildingBlocked;
        [JsonProperty("CanFetchWhileBuildingBlocked")]
        public bool CanFetchBuildingBlocked;
        [JsonProperty("CanFetchWhileOccupied")]
        public bool CanFetchOccupied;
        [JsonProperty("CanDestroyWhileOccupied")]
        public bool CanDestroyWhileOccupied;
        [JsonProperty("DismountPlayersOnFetch")]
        public bool DismountPlayersOnFetch;
        [JsonProperty("DestroyOnDisconnect")]
        public bool DestroyOnDisconnect;
        [JsonProperty("DefaultCooldowns")]
        public CooldownConfig DefaultCooldowns;
        [JsonProperty("CooldownsRequiringPermission")]
        public CooldownConfig[] CooldownsRequiringPermission;
        [JsonProperty("CommandAliases")]
        public Dictionary<string, string[]> CommandAliases;
        [JsonProperty("SubcommandAliases")]
        public Dictionary<string, string[]> SubcommandAliases;
        public void Init(MegaDrones pluginInstance);
        public CooldownConfig GetCooldownConfigForPlayer(MegaDrones plugin, string userId);
    }

    private class CooldownConfig
    {
        [JsonProperty("PermissionSuffix", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string PermissionSuffix;
        [JsonProperty("SpawnSeconds")]
        public long SpawnSeconds;
        [JsonProperty("FetchSeconds")]
        public long FetchSeconds;
        [JsonIgnore]
        public string Permission;
        public void Init(MegaDrones plugin);
        public long GetSeconds(CooldownType cooldownType);
    }

    private Configuration GetDefaultConfig();
    private class BaseConfiguration
    {
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    private static class JsonHelper
    {
        public static object Deserialize(string json);
        private static object ToObject(JToken token);
    }

    private bool MaybeUpdateConfig(BaseConfiguration config);
    private bool MaybeUpdateConfigDict(Dictionary<string, object> currentWithDefaults, Dictionary<string, object> currentRaw);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private string GetMessage(string playerId, string messageName, object[] args);
    private string GetMessage(IPlayer player, string messageName, object[] args);
    private void ReplyToPlayer(IPlayer player, string messageName, object[] args);
    private void ChatMessage(BasePlayer player, string messageName, object[] args);
    private class Lang
    {
        public const string ErrorNoPermission;
        public const string ErrorBuildingBlocked;
        public const string ErrorDroneNotFound;
        public const string ErrorDroneOccupied;
        public const string ErrorCooldown;
        public const string ErrorGenericRestricted;
        public const string ErrorUnknownCommand;
        public const string ErrorMounted;
        public const string ErrorInsufficientSpace;
        public const string SpawnSuccess;
        public const string SpawnErrorDroneAlreadyExists;
        public const string SpawnErrorDroneAlreadyExistsHelp;
        public const string GiveErrorSyntax;
        public const string GiveErrorPlayerNotFound;
        public const string GiveSuccess;
        public const string InfoDroneDestroyed;
        public const string Help;
        public const string HelpSpawn;
        public const string HelpFetch;
        public const string HelpDestroy;
        public const string HelpRemainingCooldown;
    }

    protected override void LoadDefaultMessages();
}

private static class RCUtils
{
    public static bool HasController(IRemoteControllable controllable, BasePlayer player);
    public static bool HasRealController(IRemoteControllable controllable);
    public static bool CanControl(BasePlayer player, IRemoteControllable controllable);
    public static void RemoveController(IRemoteControllable controllable);
    public static bool AddViewer(IRemoteControllable controllable, BasePlayer player);
}

private class HookCollection
{
    public bool IsSubscribed { get; set; }
    private readonly MegaDrones _plugin;
    private readonly string[] _hookNames;
    private readonly Func<bool> _shouldSubscribe;
    public HookCollection(MegaDrones plugin, Func<bool> shouldSubscribe, string[] hookNames);
    public void Subscribe();
    public void Unsubscribe();
    public void Refresh();
}

private class DynamicHookHashSet : HashSet<T>
{
    private readonly HookCollection _hookCollection;
    public DynamicHookHashSet(MegaDrones plugin, string[] hookNames);
    public new bool Add(T item);
    public new bool Remove(T item);
    public void Unsubscribe();
}

private class CameraMovement : EntityComponent<BasePlayer>
{
    public static CameraMovement AddToPlayer(BasePlayer player, Drone drone);
    public static void RemoveFromPlayer(BasePlayer player);
    private Drone _drone;
    private CameraMovement SetDrone(Drone drone);
    private void Update();
}

private class StoredData
{
    [JsonProperty("PlayerDrones")]
    public Dictionary<string, ulong> PlayerDrones;
    [JsonProperty("OtherDrones")]
    public HashSet<ulong> OtherDrones;
    [JsonProperty("Cooldowns")]
    public CooldownManager Cooldowns;
    public static StoredData Load();
    public static StoredData Reset();
    public StoredData Save();
    public HashSet<ulong> GetAllMegaDroneIds();
    public bool IsMegaDrone(Drone drone, string userIdString);
    public bool IsMegaDrone(Drone drone);
    public void RegisterPlayerDrone(string userId, Drone drone);
    public void UnregisterPlayerDrone(string userId);
    public void RegisterOtherDrone(Drone drone);
    public void UnregisterOtherDrone(Drone drone);
}

private class CooldownManager
{
    [JsonProperty("Spawn")]
    private Dictionary<string, long> Spawn;
    [JsonProperty("Fetch")]
    private Dictionary<string, long> Fetch;
    public Dictionary<string, long> GetCooldownMap(CooldownType cooldownType);
    public void ClearAll();
}

private class Configuration : BaseConfiguration
{
    [JsonProperty("AutoMount")]
    public bool AutoMount;
    [JsonProperty("AutoFetch")]
    public bool AutoFetch;
    [JsonProperty("DroneIdentifierPrefix")]
    public string DroneIdentifierPrefix;
    [JsonProperty("CamIdentifierPrefix")]
    public string CamIdentifierPrefix;
    [JsonProperty("CanSpawnWhileBuildingBlocked")]
    public bool CanSpawnBuildingBlocked;
    [JsonProperty("CanFetchWhileBuildingBlocked")]
    public bool CanFetchBuildingBlocked;
    [JsonProperty("CanFetchWhileOccupied")]
    public bool CanFetchOccupied;
    [JsonProperty("CanDestroyWhileOccupied")]
    public bool CanDestroyWhileOccupied;
    [JsonProperty("DismountPlayersOnFetch")]
    public bool DismountPlayersOnFetch;
    [JsonProperty("DestroyOnDisconnect")]
    public bool DestroyOnDisconnect;
    [JsonProperty("DefaultCooldowns")]
    public CooldownConfig DefaultCooldowns;
    [JsonProperty("CooldownsRequiringPermission")]
    public CooldownConfig[] CooldownsRequiringPermission;
    [JsonProperty("CommandAliases")]
    public Dictionary<string, string[]> CommandAliases;
    [JsonProperty("SubcommandAliases")]
    public Dictionary<string, string[]> SubcommandAliases;
    public void Init(MegaDrones pluginInstance);
    public CooldownConfig GetCooldownConfigForPlayer(MegaDrones plugin, string userId);
}

private class CooldownConfig
{
    [JsonProperty("PermissionSuffix", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string PermissionSuffix;
    [JsonProperty("SpawnSeconds")]
    public long SpawnSeconds;
    [JsonProperty("FetchSeconds")]
    public long FetchSeconds;
    [JsonIgnore]
    public string Permission;
    public void Init(MegaDrones plugin);
    public long GetSeconds(CooldownType cooldownType);
}

private class BaseConfiguration
{
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private static class JsonHelper
{
    public static object Deserialize(string json);
    private static object ToObject(JToken token);
}

private class Lang
{
    public const string ErrorNoPermission;
    public const string ErrorBuildingBlocked;
    public const string ErrorDroneNotFound;
    public const string ErrorDroneOccupied;
    public const string ErrorCooldown;
    public const string ErrorGenericRestricted;
    public const string ErrorUnknownCommand;
    public const string ErrorMounted;
    public const string ErrorInsufficientSpace;
    public const string SpawnSuccess;
    public const string SpawnErrorDroneAlreadyExists;
    public const string SpawnErrorDroneAlreadyExistsHelp;
    public const string GiveErrorSyntax;
    public const string GiveErrorPlayerNotFound;
    public const string GiveSuccess;
    public const string InfoDroneDestroyed;
    public const string Help;
    public const string HelpSpawn;
    public const string HelpFetch;
    public const string HelpDestroy;
    public const string HelpRemainingCooldown;
}


```

---

## MemoryCache by austinv900 - Plugin API that allows other plugins to store data in-memory and retrieve it later

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Memory Cache", "austinv900", "1.0.6")]
[Description("Provides api for in-memory storage")]
internal class MemoryCache : CovalencePlugin
{
    public sealed class CacheItemOptions
    {
        public DateTimeOffset? AbsoluteExpiration { get; set; }
        public TimeSpan? AbsoluteExpirationRelativeToNow { get; set; }
        public Action<object> ExpirationCallback { get; set; }
        public TimeSpan? SlidingExpiration { get; set; }
    }

    private class CacheItem
    {
        public object StoredObject { get; set; }
        public CacheItemOptions Options { get; set; }
        public DateTimeOffset LastAccess { get; set; }
    }

    private Dictionary<string, CacheItem> _memoryCache;
    private void Init();
    private void Unload();
    private void OnServerSave();
    private void RunCacheCleanup();
    private bool IsExpired(CacheItem item, CacheItemOptions options, DateTimeOffset current);
    private void ProcessCacheCleanupItem(string key, CacheItem item, CacheItemOptions options, DateTimeOffset currentTimestamp);
    private void ExpireItem(string key, CacheItem item);
    public bool Add(string key, object item, CacheItemOptions options);
    private bool Add(string key, object item, Action<object> expireCallback, DateTimeOffset? absoluteExpire, TimeSpan? slidingExpire);
    private bool Add(string key, object item, Action<object> expireCallback, TimeSpan? slidingExpire);
    private bool Add(string key, object item, Action<object> expireCallback, DateTimeOffset? absoluteExpire);
    private bool Add(string key, object item, TimeSpan? slidingExpire);
    private bool Add(string key, object item, DateTimeOffset? absoluteExpire);
    private bool Add(string key, object item);
    private object Get(string key);
    public TCacheItem Get(string key);
    private bool Remove(string key);
}

public sealed class CacheItemOptions
{
    public DateTimeOffset? AbsoluteExpiration { get; set; }
    public TimeSpan? AbsoluteExpirationRelativeToNow { get; set; }
    public Action<object> ExpirationCallback { get; set; }
    public TimeSpan? SlidingExpiration { get; set; }
}

private class CacheItem
{
    public object StoredObject { get; set; }
    public CacheItemOptions Options { get; set; }
    public DateTimeOffset LastAccess { get; set; }
}


```

---

## Metabolism by TheFriendlyChap - Modify or disable player metabolism stats

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Metabolism", "The Friendly Chap", "1.0.2")]
[Description("Modify or disable player metabolism stats")]
public class Metabolism : RustPlugin
{
    private void Init();
    private void OnPlayerRespawned(BasePlayer player);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Permission -> Settings")]
        public Dictionary<string, MetabolismSettings> permissions;
    }

    private class MetabolismSettings
    {
        [JsonProperty(PropertyName = "Water on respawn")]
        public float hydration;
        [JsonProperty(PropertyName = "Calories on respawn")]
        public float calories;
        [JsonProperty(PropertyName = "Health on respawn")]
        public float health;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void ShowLogo();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Permission -> Settings")]
    public Dictionary<string, MetabolismSettings> permissions;
}

private class MetabolismSettings
{
    [JsonProperty(PropertyName = "Water on respawn")]
    public float hydration;
    [JsonProperty(PropertyName = "Calories on respawn")]
    public float calories;
    [JsonProperty(PropertyName = "Health on respawn")]
    public float health;
}


```

---

## MinicopterDCProtect by NooBlet - Protects player minicopters from crashing if they disconnect

```csharp
using UnityEngine;

Oxide.Plugins
[Info("Mini Copter DC Protect", "NooBlet", "0.1.9")]
[Description("Protects player minicopters from crashing if they disconnect")]
public class MinicopterDCProtect : CovalencePlugin
{
    private static int _layerMask;
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void TPplayerandCopter(BasePlayer player, BaseHelicopter copter);
    private void Teleport(BasePlayer player, Vector3 target);
    private Vector3 FindLowestTpPoint(Vector3 loc);
     Vector3 FindclosestLand(Vector3 loc);
}


```

---

## MiniCopterEject by Ryz0r - Allows the driver of a Mini Copter to eject or self-eject the passenger using a command, or button

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Mini Copter Eject", "Ryz0r", "1.0.2")]
[Description("Allows the pilot to eject the passenger from a MiniCopter.")]
public class MiniCopterEject : RustPlugin
{
    private const string EjectPerm;
    private Configuration _config;
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private class Configuration
    {
        [JsonProperty(PropertyName = "Self Eject If No Passenger")]
        public bool SelfEject;
        [JsonProperty(PropertyName = "Send Messages on Eject")]
        public bool SendOnEject;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultMessages();
    private void Init();
    private void DoEject(BaseVehicle bv, BasePlayer bp, bool selfEject);
    private static void SpawnButton(Component entity);
    private void OnEntitySpawned(BaseNetworkable entity);
    private object OnButtonPress(BaseNetworkable button, BasePlayer player);
    private void EjectCommand(IPlayer player, string command, string[] args);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Self Eject If No Passenger")]
    public bool SelfEject;
    [JsonProperty(PropertyName = "Send Messages on Eject")]
    public bool SendOnEject;
}


```

---

## MiniCopterLock by Thisha - Players can add a key lock to a minicopter

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("MiniCopter Lock", "Thisha", "0.1.7")]
[Description("Gives players the ability to lock a minicopter")]
 class MiniCopterLock : RustPlugin
{
    private const string keyLockPrefab;
    private const string codeLockPrefab;
    private const string effectDenied;
    private const string effectDeployed;
    private const string keylockpermissionName;
    private const string codelockpermissionName;
    private const string kickpermissionName;
    private const int doorkeyItemID;
    private const int keylockItemID;
    private const int codelockItemID;
    private CooldownManager cooldownManager;
    protected override void LoadDefaultMessages();
    private ConfigData config;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Sound Effects")]
        public bool SoundEffects;
        [JsonProperty(PropertyName = "Locks Are Free")]
        public bool LocksAreFree;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [ChatCommand("lockmini.key")]
    private void LockWithkeyLock1(BasePlayer player, string command, string[] args);
    [ChatCommand("lockmini.code")]
    private void LockWithCodeLock1(BasePlayer player, string command, string[] args);
    [ChatCommand("lockit.key")]
    private void LockWithkeyLock2(BasePlayer player, string command, string[] args);
    [ChatCommand("lockit.code")]
    private void LockWithCodeLock2(BasePlayer player, string command, string[] args);
    [ConsoleCommand("heli.kick")]
    private void KickPassenger(ConsoleSystem.Arg arg);
    private void Init();
     object CanMountEntity(BasePlayer player, BaseMountable entity);
     void OnEntityDismounted(BaseMountable entity, BasePlayer player);
     object CanLock(BasePlayer player, KeyLock keyLock);
     object CanLock(BasePlayer player, CodeLock codeLock);
     object CanUnlock(BasePlayer player, KeyLock keyLock);
     object CanChangeCode(BasePlayer player, CodeLock codeLock, string newCode, bool isGuestCode);
     object CanPickupLock(BasePlayer player, BaseLock baseLock);
     object CanLootEntity(BasePlayer player, StorageContainer container);
     object OnVehiclePush(BaseVehicle vehicle, BasePlayer player);
    private void LockIt(BasePlayer player, LockType lockType);
    private LockType HasLock(PlayerHelicopter heli);
    private bool CanAffordLock(BasePlayer player, LockType lockType, PayType payType);
    private void PayForlock(BasePlayer player, LockType lockType, PayType payType);
    private void AddKeylock(PlayerHelicopter heli, BasePlayer player);
    private void AddCodelock(PlayerHelicopter heli, BasePlayer player, bool isfree);
    private object CheckLock(BasePlayer player, KeyLock keyLock, bool forLocking);
    private bool HasAnyAuthorizedMounted(PlayerHelicopter heli, BasePlayer dismounted, bool kick, bool hardkick);
    private bool PlayerIsAuthorized(BasePlayer player, PlayerHelicopter heli);
    private bool PlayerHasTheKey(BasePlayer player, int keyCode);
    private bool IsMatchingKey(Item item, int keyCode);
    private void DismountPlayers(PlayerHelicopter heli);
    internal class CooldownManager
    {
        private readonly Dictionary<string, CooldownInfo> Cooldowns;
        public CooldownManager();
        private class CooldownInfo
        {
            public float CraftTime;
            public float CoolDown;
            public CooldownInfo(float craftTime, float duration);
        }

        public void UpdateLastUsedForPlayer(string userID, LockType lockType);
        public float GetSecondsRemaining(string userID, LockType lockType);
    }

    private bool PlayerHasCooldown(string userID, LockType lockType, float secondsRemaining);
    private static BasePlayer FindBasePlayer(Vector3 pos);
    private string Lang(string key, string userId, object[] args);
    private LockType GetLockType(string allowedLockType);
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Sound Effects")]
    public bool SoundEffects;
    [JsonProperty(PropertyName = "Locks Are Free")]
    public bool LocksAreFree;
}

internal class CooldownManager
{
    private readonly Dictionary<string, CooldownInfo> Cooldowns;
    public CooldownManager();
    private class CooldownInfo
    {
        public float CraftTime;
        public float CoolDown;
        public CooldownInfo(float craftTime, float duration);
    }

    public void UpdateLastUsedForPlayer(string userID, LockType lockType);
    public float GetSecondsRemaining(string userID, LockType lockType);
}

private class CooldownInfo
{
    public float CraftTime;
    public float CoolDown;
    public CooldownInfo(float craftTime, float duration);
}


```

---

## MiniCopterOptions by Pho3niX90 - Options like fuel consumption, movement speed, storage, turrets, land on cargo & fix flyhack

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Mini-Copter Options", "Pho3niX90", "2.5.1")]
[Description("Provide a number of additional options for Mini-Copters, including storage and seats.")]
internal class MiniCopterOptions : CovalencePlugin
{
    private readonly string minicopterPrefab;
    private readonly string storagePrefab;
    private readonly string storageLargePrefab;
    private readonly string autoturretPrefab;
    private readonly string switchPrefab;
    private readonly string searchLightPrefab;
    private readonly string flasherBluePrefab;
    private readonly string spherePrefab;
    private const string resizableLootPanelName;
    private const int MinStorageCapacity;
    private const int MaxStorageCapacity;
    private static readonly Vector3 TurretSwitchPosition;
    private readonly object False;
    private Configuration config;
    private MiniCopterDefaults copterDefaults;
    private bool lastRanAtNight;
    private int setupTimeHooksAttempts;
    private TOD_Sky time;
    private float sunrise;
    private float sunset;
    private float lastNightCheck;
    private float[] stageFuelPerSec;
    private float[] stageLiftFraction;
    private void Init();
    private void OnServerInitialized(bool init);
    private void Unload();
    private void OnEntitySpawned(Minicopter copter);
    private void OnEntityKill(BaseNetworkable entity);
    private void OnSwitchToggled(ElectricSwitch electricSwitch, BasePlayer player);
    private object OnTurretTarget(AutoTurret turret, BaseCombatEntity target);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private void OnEntityDismounted(BaseNetworkable entity, BasePlayer player);
    private void OnItemDeployed(Deployer deployer, StorageContainer container, BaseLock baseLock);
    private void OnMinicopterFuelPerSecChange(Plugin plugin, Minicopter copter, float[] fuelPerSec);
    private void OnMinicopterLiftFractionChange(Plugin plugin, Minicopter copter, float[] liftFraction);
    private static class ExposedHooks
    {
        public static object OnMiniCopterOptions(Minicopter copter);
        public static object OnMinicopterFuelPerSecChange(Plugin plugin, Minicopter copter, float[] fuelPerSec);
        public static object OnMinicopterLiftFractionChange(Plugin plugin, Minicopter copter, float[] liftFraction);
    }

    private bool IsNight();
    private void OnHour();
    private void SetupTimeHooks();
    private StorageContainer[] GetStorage(Minicopter copter);
    private void AddLargeStorageBox(Minicopter copter);
    private void AddRearStorageBox(Minicopter copter);
    private void AddSideStorageBoxes(Minicopter copter);
    private void AddStorageBox(Minicopter copter, string prefab, Vector3 position);
    private void SetupStorage(StorageContainer box);
    private void AddStorageBox(Minicopter copter, string prefab, Vector3 position, Quaternion rotation);
    private void SetupInvincibility(BaseCombatEntity entity);
    private void SetupTailLight(FlasherLight tailLight);
    private void AddTailLight(Minicopter copter);
    private void SetupSphereEntity(SphereEntity sphereEntity);
    private void SetupSearchLight(SearchLight searchLight);
    private void AddSearchLight(Minicopter copter);
    private void SetupAutoTurret(AutoTurret turret);
    private void AddTurret(Minicopter copter);
    private void SetupSwitch(ElectricSwitch electricSwitch);
    private void AddSwitch(AutoTurret turret);
    private void DestroyGroundComp(BaseEntity ent);
    private void DestroyMeshCollider(BaseEntity ent);
    private void RestoreMiniCopter(Minicopter copter, bool removeStorage);
    private void ModifyMiniCopter(Minicopter copter);
    private bool CanModifyMiniCopter(Minicopter copter);
    private bool CanModifyFuelPerSec(Minicopter copter, float proposedFuelPerSec);
    private bool CanModifyLiftFraction(Minicopter copter, float proposedLiftFraction);
    private void ScheduleModifyMiniCopter(Minicopter copter);
    private void StoreMiniCopterDefaults();
    private class MiniCopterDefaults
    {
        public float fuelPerSec;
        public float liftFraction;
        public Vector3 torqueScale;
    }

    private class Configuration : SerializableConfiguration
    {
        [JsonProperty("Fuel per Second")]
        public float fuelPerSec;
        [JsonProperty("Lift Fraction")]
        public float liftFraction;
        [JsonProperty("Pitch Torque Scale")]
        public float torqueScalePitch;
        [JsonProperty("Yaw Torque Scale")]
        public float torqueScaleYaw;
        [JsonProperty("Roll Torque Scale")]
        public float torqueScaleRoll;
        [JsonProperty("Storage Containers")]
        public int storageContainers;
        [JsonProperty("Large Storage Containers")]
        public int storageLargeContainers;
        [JsonProperty("Restore Defaults")]
        public bool restoreDefaults;
        [JsonProperty("Reload Storage")]
        public bool reloadStorage;
        [JsonProperty("Drop Storage Loot On Death")]
        public bool dropStorage;
        [JsonProperty("Large Storage Lockable")]
        public bool largeStorageLockable;
        [JsonProperty("Large Storage Size (Max 48)")]
        public int largeStorageSize;
        [JsonProperty("Large Storage Size (Max 42)")]
        public int largeStorageSizeDeprecated { get; set; }
        [JsonProperty("Seconds to pause flyhack when dismount from heli.")]
        public int flyHackPause;
        [JsonProperty("Add auto turret to heli")]
        public bool autoturret;
        [JsonProperty("Auto turret targets players")]
        public bool autoTurretTargetsPlayers;
        [JsonProperty("Auto turret targets NPCs")]
        public bool autoTurretTargetsNPCs;
        [JsonProperty("Auto turret targets animals")]
        public bool autoTurretTargetsAnimals;
        [JsonProperty("Mini Turret Range (Default 30)")]
        public float turretRange;
        [JsonProperty("Light: Add Searchlight to heli")]
        public bool addSearchLight;
        [JsonProperty("Light: Add Nightitme Tail Light")]
        public bool lightTail;
    }

    private Configuration GetDefaultConfig();
    private class SerializableConfiguration
    {
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    private static class JsonHelper
    {
        public static object Deserialize(string json);
        private static object ToObject(JToken token);
    }

    private bool MaybeUpdateConfig(SerializableConfiguration config);
    private bool MaybeUpdateConfigDict(Dictionary<string, object> currentWithDefaults, Dictionary<string, object> currentRaw);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
}

private static class ExposedHooks
{
    public static object OnMiniCopterOptions(Minicopter copter);
    public static object OnMinicopterFuelPerSecChange(Plugin plugin, Minicopter copter, float[] fuelPerSec);
    public static object OnMinicopterLiftFractionChange(Plugin plugin, Minicopter copter, float[] liftFraction);
}

private class MiniCopterDefaults
{
    public float fuelPerSec;
    public float liftFraction;
    public Vector3 torqueScale;
}

private class Configuration : SerializableConfiguration
{
    [JsonProperty("Fuel per Second")]
    public float fuelPerSec;
    [JsonProperty("Lift Fraction")]
    public float liftFraction;
    [JsonProperty("Pitch Torque Scale")]
    public float torqueScalePitch;
    [JsonProperty("Yaw Torque Scale")]
    public float torqueScaleYaw;
    [JsonProperty("Roll Torque Scale")]
    public float torqueScaleRoll;
    [JsonProperty("Storage Containers")]
    public int storageContainers;
    [JsonProperty("Large Storage Containers")]
    public int storageLargeContainers;
    [JsonProperty("Restore Defaults")]
    public bool restoreDefaults;
    [JsonProperty("Reload Storage")]
    public bool reloadStorage;
    [JsonProperty("Drop Storage Loot On Death")]
    public bool dropStorage;
    [JsonProperty("Large Storage Lockable")]
    public bool largeStorageLockable;
    [JsonProperty("Large Storage Size (Max 48)")]
    public int largeStorageSize;
    [JsonProperty("Large Storage Size (Max 42)")]
    public int largeStorageSizeDeprecated { get; set; }
    [JsonProperty("Seconds to pause flyhack when dismount from heli.")]
    public int flyHackPause;
    [JsonProperty("Add auto turret to heli")]
    public bool autoturret;
    [JsonProperty("Auto turret targets players")]
    public bool autoTurretTargetsPlayers;
    [JsonProperty("Auto turret targets NPCs")]
    public bool autoTurretTargetsNPCs;
    [JsonProperty("Auto turret targets animals")]
    public bool autoTurretTargetsAnimals;
    [JsonProperty("Mini Turret Range (Default 30)")]
    public float turretRange;
    [JsonProperty("Light: Add Searchlight to heli")]
    public bool addSearchLight;
    [JsonProperty("Light: Add Nightitme Tail Light")]
    public bool lightTail;
}

private class SerializableConfiguration
{
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private static class JsonHelper
{
    public static object Deserialize(string json);
    private static object ToObject(JToken token);
}


```

---

## MinicopterSeating by Bazz3l - Adds 2 extra seats to the minicopter (total of 4)

```csharp
using UnityEngine;

Oxide.Plugins
[Info("Minicopter Seating", "Bazz3l", "1.1.7")]
[Description("Spawn extra seats on each side of the minicopter.")]
internal class MinicopterSeating : RustPlugin
{
    private readonly GameObjectRef _gameObjectSeat;
    private readonly Vector3 _seat1;
    private readonly Vector3 _seat2;
    private void OnServerInitialized();
    private void Init();
    private void OnEntitySpawned(Minicopter copter);
    private void SetupSeating(BaseVehicle vehicle);
    private BaseVehicle.MountPointInfo CreateMount(BaseVehicle.MountPointInfo mountPoint, Vector3 position);
}


```

---

## MiniPlate by TheFriendlyChap - Adds a wooden "Licence Plate" to the minicopter for identification.

```csharp
using Newtonsoft.Json;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Minicopter Licence Plate", "The Friendly Chap", "1.1.6")]
[Description("Spawn a licence plate (Small Wooden Board) at the back of the minicopter, with optional permissions.")]
 class MiniPlate : RustPlugin
{
    const string PlatePrefab;
    private static readonly Vector3 PlatePosition;
    private static readonly Quaternion PlateRotation;
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Use Permissions")]
        public bool UsePermissions;
        [JsonProperty(PropertyName = "Debug")]
        public bool Debug;
    }

     void Init();
    protected override void LoadDefaultConfig();
    private bool LoadConfigVariables();
     void SaveConfig(ConfigData config);
    private void OnServerInitialized();
     void FindMinicopters();
     void OnEntitySpawned(Minicopter mini);
     void AttachLicensePlate(BaseVehicle vehicle);
     void CreateLicensePlate(BaseVehicle vehicle, Vector3 position);
     void DestroyUnnecessaryComponents(BaseEntity entity);
    private void ShowLogo();
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Use Permissions")]
    public bool UsePermissions;
    [JsonProperty(PropertyName = "Debug")]
    public bool Debug;
}


```

---

## MiniTrails by Salsa - Adds a trail onto the back of players' minis.

```csharp
using System.Collections.Generic;
using System.Collections;
using Oxide.Game.Rust.Libraries;
using Newtonsoft.Json;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Plugins;
using System;
using UnityEngine;

Oxide.Plugins
[Info("Mini Trails", "Salsa", "2.1.4")]
[Description("Draws trails behind minicopters")]
 class MiniTrails : RustPlugin
{
    private static Data data;
    private Dictionary<ulong, MiniT> MountedMinis;
    private const string SnowballGunEnt;
    private const string AnchorEnt;
    private const string FlameEnt;
    private const string WooshSound;
    private const string SupplyEnt;
    private const string FireworkEnt;
    private const string PermUse;
    private class Preferences
    {
        public bool active;
        public int count;
        public trailType type;
        public Preferences();
    }

    private class Data
    {
        public Dictionary<ulong, Preferences> preferences;
        public Data();
    }

    private void OnEntityMounted(BaseMountable mountable, BasePlayer player);
    private void OnEntityDismounted(BaseMountable mountable, BasePlayer player);
    private void OnEntityKill(Minicopter mini);
    private void OnServerSave();
    private void Unload();
    private void Init();
    private class MiniT
    {
        public BaseVehicle mini;
        public BasePlayer driver;
        public List<BaseEntity> anchors;
        public List<BaseEntity> trails;
        private void spawnFlame(string prefab, Vector3 localPos, Quaternion localRotation);
        private void spawnSupplySignal(string prefab, Vector3 localPos, Quaternion localRotation);
        private void spawnFirework(string prefab, Vector3 localPos, Quaternion localRotation);
        public void spawnTrail();
        public void clearTrail();
        public MiniT(BaseVehicle _mini, BasePlayer _player);
    }

    [ChatCommand("trail")]
    private void cmdTrail(BasePlayer player, string command, string[] args);
    protected override void LoadDefaultMessages();
     void SendChatMessage(BasePlayer player, string msg);
}

private class Preferences
{
    public bool active;
    public int count;
    public trailType type;
    public Preferences();
}

private class Data
{
    public Dictionary<ulong, Preferences> preferences;
    public Data();
}

private class MiniT
{
    public BaseVehicle mini;
    public BasePlayer driver;
    public List<BaseEntity> anchors;
    public List<BaseEntity> trails;
    private void spawnFlame(string prefab, Vector3 localPos, Quaternion localRotation);
    private void spawnSupplySignal(string prefab, Vector3 localPos, Quaternion localRotation);
    private void spawnFirework(string prefab, Vector3 localPos, Quaternion localRotation);
    public void spawnTrail();
    public void clearTrail();
    public MiniT(BaseVehicle _mini, BasePlayer _player);
}


```

---

## Minstrel by  - Binds 1-8 to notes

```csharp
using Oxide.Core;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Reflection;
using System.Linq;
using UnityEngine;
using System.Text;

Oxide.Plugins
[Info("Minstrel", "4seti [Lunatiq] for Rust Planet", "0.0.7", ResourceId = 981)]
public class Minstrel : RustPlugin
{
    private void Log(string message);
    private void Warn(string message);
    private void Error(string message);
    private T GetConfig(string name, T defaultValue);
     void Loaded();
    private static FieldInfo serverinput;
     Dictionary<string, List<TuneNote>> tuneDict;
     Dictionary<ulong, List<TuneNote>> tuneRecTemp;
     List<string> cfgTunes;
    protected override void LoadDefaultConfig();
     void LoadVariables();
     void Init();
     void Unload();
    private static void DestroyAll();
    [ChatCommand("ms")]
     void cmdToggleMS(BasePlayer player, string cmd, string[] args);
    private bool LoadTune(string tuneName, List<TuneNote> list);
     void SaveTune(string playerName, string tuneName, ulong userID);
    private string BuildNoteList(float adjust, float start);
    [ChatCommand("ms_tune")]
     void cmdTune(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ms_rec")]
     void cmdRec(BasePlayer player, string cmd, string[] args);
     void playTune(BasePlayer player, string tuneName);
    private bool checkComponent(BasePlayer player);
     List<object> getTune(string tuneName);
    [ChatCommand("ms_adj")]
     void cmdMSAdjust(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ms_list")]
     void cmdMSList(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ms_rel")]
     void cmdMSReload(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ms_del")]
     void cmdMSDelete(BasePlayer player, string cmd, string[] args);
    [ChatCommand("msp")]
     void cmdMSPluck(BasePlayer player, string cmd, string[] args);
    public class KeyboardGuitar : MonoBehaviour
    {
        private static float noteTime;
        public float adjust;
        public float start;
        public float NextTimeToPress;
        public float noteToPlay;
        private InputState input;
        public bool Recording;
        public BasePlayer owner;
        public List<TuneNote> recTune;
        public float nextNoteTime;
         Effect effectP;
         Effect effectS;
         void Awake();
         void FixedUpdate();
        public void PlayNote(float scale);
        public List<TuneNote> curTune;
        private Stack<TuneNote> tuneToPlay;
        public bool playingTune;
        private TuneNote nextNote;
        public void PlayTune();
        private void PlayTuneStack();
    }

    public class TuneNote
    {
        public float NoteScale;
        public float Delay;
        public bool Pluck;
        public TuneNote(float note, float delay, bool pluck);
    }

}

public class KeyboardGuitar : MonoBehaviour
{
    private static float noteTime;
    public float adjust;
    public float start;
    public float NextTimeToPress;
    public float noteToPlay;
    private InputState input;
    public bool Recording;
    public BasePlayer owner;
    public List<TuneNote> recTune;
    public float nextNoteTime;
     Effect effectP;
     Effect effectS;
     void Awake();
     void FixedUpdate();
    public void PlayNote(float scale);
    public List<TuneNote> curTune;
    private Stack<TuneNote> tuneToPlay;
    public bool playingTune;
    private TuneNote nextNote;
    public void PlayTune();
    private void PlayTuneStack();
}

public class TuneNote
{
    public float NoteScale;
    public float Delay;
    public bool Pluck;
    public TuneNote(float note, float delay, bool pluck);
}


```

---

## MLRSFixer by Aspectdev - Provides commands to reset the timer and fix all MLRS vehicles on your server.

```csharp
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("MLRS Fixer", "Aspect.dev", "0.1.3")]
[Description("Provides commands to fix all MLRS on your server.")]
internal class MLRSFixer : CovalencePlugin
{
    public const string fixPermission;
    protected override void LoadDefaultMessages();
     void Init();
    [Command("fixmlrs")]
     void FixMLRS(IPlayer sender, string command, string[] args);
}


```

---

## MLRSHellfire by Timm3D - Manage MLRS and/or call MLRS missiles to a specific point, in different ways

```csharp
using UnityEngine;
using System.Linq;
using Newtonsoft.Json;
using System.Collections.Generic;
using System.Text;
using System;

Oxide.Plugins
[Info("MLRS Hellfire", "Timm3D", "1.0.8")]
[Description("Manage MLRS and/or call MLRS missiles to a specific point, in different ways.")]
internal class MLRSHellfire : RustPlugin
{
    private const string MLRSHellfire_Admin;
    private const string MLRSHellfire_MLRSMountBypass;
    private const string MLRSHellfire_UseRemoteMLRS;
    private const string MLRSHellfire_RemoteMLRS_AllowTargetingPlayer;
    private const string MLRSHellfire_RemoteMLRS_MLRSBrokenBypass;
    private PluginConfig mConfig;
    private void Init();
    protected override void LoadConfig();
    private PluginConfig GetDefaultConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     class PluginConfig
    {
        [JsonProperty]
        public ulong PluginPicture { get; set; }
        [JsonProperty]
        public string PluginPrefix { get; set; }
        [JsonProperty]
        public float HellfireRocketDamageModifier { get; set; }
        [JsonProperty]
        public float HellfireRocketExplosiveRadiusModifier { get; set; }
        [JsonProperty("MLRSFireInterval in seconds")]
        public float MLRSFireInterval { get; set; }
        [JsonProperty("RemoteMLRSFireInterval in seconds")]
        public float RemoteMLRSFireInterval { get; set; }
        [JsonProperty("HellfireFireInterval in seconds")]
        public float HellfireFireInterval { get; set; }
        [JsonProperty("Allow using of MLRS for all players like in vanilla rust")]
        public bool AllowUsingOfMLRSForAllPlayers { get; set; }
        [JsonProperty("Max amount of rockets which can spawn when using hellfire command with custom missle amount")]
        public uint HellFireMaxRocketAmountToSpawn { get; set; }
    }

    protected override void LoadDefaultMessages();
    private const string Note_MLRS_StartedAttack;
    private const string Note_MLRS_FiringIn10Sec;
    private const string Note_MLRS_Fixed;
    private const string Note_All_MLRS_Fixed;
    private const string Note_MLRS_Enabled;
    private const string Note_MLRS_Disabled;
    private const string Note_MLRS_Status_Enabled;
    private const string Note_MLRS_Status_Disabled;
    private const string Error_MLRS_NoMLRS;
    private const string Error_MLRS_CantFindPlayer;
    private const string Error_MLRS_NoMapMark;
    private const string Error_MLRS_Disabled;
    private const string Error_MLRS_IsBusy;
    private const string Note_HellFire_StartedAttack;
    private const string Error_HellFire_NoMapMark;
    private const string Error_HellFire_InvalidAmount;
    private const string Error_HellFire_CantFindPlayer;
    private const string Error_FoundMultiplePlayer;
    private const string Error_RemoteMLRS_NoMissles;
    private const string Error_RemoteMLRS_InvalidAmount;
    private const string Error_RemoteMLRS_NoMapMark;
    private const string Error_RemoteMLRS_MRLSBroken;
    private const string Error_NoPermission;
    private const string Syntax_MLRS;
    private const string Syntax_HellFire;
    private const string Syntax_RemoteMLRS;
     object CanMountEntity(BasePlayer player, BaseMountable entity);
    private void ChatReply(BasePlayer player, string message);
    private const string MLRSRocketPrefab;
    private static List<CustomMLRS> allActiveMRLSOnServer;
    private static TimedExplosive mlrsRocketTimedExplosive;
    private static CustomMLRS mainMLRS;
    private const string MapNoteTargetName;
    [ChatCommand("mlrs")]
    private void Mlrs(BasePlayer player, string command, string[] args);
    [ChatCommand("mlrsfire")]
    private void MlrsFire(BasePlayer player, string command, string[] args);
    [ChatCommand("mlrsfireall")]
    private void MlrsFireAll(BasePlayer player, string command, string[] args);
    [ChatCommand("mlrsfix")]
    private void MLRSFix(BasePlayer player, string command, string[] args);
    [ChatCommand("mlrsfixall")]
    private void MLRSFixAll(BasePlayer player, string command, string[] args);
    [ChatCommand("remotemlrs")]
    private void RemoteMlrs(BasePlayer player, string command, string[] args);
    [ChatCommand("hellfire")]
    private void Hellfire(BasePlayer player, string command, string[] args);
    private void ExecuteFireOperation(BasePlayer player, Vector3 targetPosition);
    private Vector3 GetAimToTarget(Vector3 startPosition, Vector3 targetPos, float baseGravity);
    private bool CreateAndSpawnRocket(Vector3 firingPos, Vector3 firingDir, ServerProjectile mlrsRocketProjectile);
    private float ProjectileDistToGravity(float x, float y, float θ, float v);
    private void ApplyMLRSRocketModfications(MLRSRocket mlrsRocket);
    private BasePlayer SearchAndGetTargetPlayer(string targetPlayername, BasePlayer callingPlayer);
    private bool IsValidHellfireMissleAmount(string amount, int rocketsToSpawn);
    private bool IsValidMLRSMissleAmount(string amount, int rocketsToSpawn);
    private static CustomMLRS GetActiveMainMLRS();
    private static List<CustomMLRS> GetAllActiveMLRS();
    private static List<CustomMLRS> _GetAllActiveMLRS();
    private TimedExplosive GetDamageOfHellfireRocket(MLRSRocket mlrsRocket);
    private Vector3 GetTargetMapNotePosition(BasePlayer player);
     class CustomMLRS
    {
        public CustomMLRS(MLRS mlrs);
        public readonly MLRS instance;
        public bool isBusy;
        public readonly int mlrsId;
    }

}

 class PluginConfig
{
    [JsonProperty]
    public ulong PluginPicture { get; set; }
    [JsonProperty]
    public string PluginPrefix { get; set; }
    [JsonProperty]
    public float HellfireRocketDamageModifier { get; set; }
    [JsonProperty]
    public float HellfireRocketExplosiveRadiusModifier { get; set; }
    [JsonProperty("MLRSFireInterval in seconds")]
    public float MLRSFireInterval { get; set; }
    [JsonProperty("RemoteMLRSFireInterval in seconds")]
    public float RemoteMLRSFireInterval { get; set; }
    [JsonProperty("HellfireFireInterval in seconds")]
    public float HellfireFireInterval { get; set; }
    [JsonProperty("Allow using of MLRS for all players like in vanilla rust")]
    public bool AllowUsingOfMLRSForAllPlayers { get; set; }
    [JsonProperty("Max amount of rockets which can spawn when using hellfire command with custom missle amount")]
    public uint HellFireMaxRocketAmountToSpawn { get; set; }
}

 class CustomMLRS
{
    public CustomMLRS(MLRS mlrs);
    public readonly MLRS instance;
    public bool isBusy;
    public readonly int mlrsId;
}


```

---

## MlrsRocketTracker by MadKingCraig - Tracks who shot the MLRS and its target

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("MLRS Rocket Tracker", "MadKingCraig", "1.1.0")]
[Description("Track where the MLRS is fired at")]
 class MlrsRocketTracker : RustPlugin
{
    [PluginReference]
    private Plugin DiscordMessages;
    private void Init();
    private void OnMlrsFired(MLRS mlrs, BasePlayer player);
    private string CalculateGridPosition(Vector3 position);
    private void SendDiscordMessage(string name, string playerId, string text);
    private PluginConfig _config;
    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Log to Console")]
        public bool LogToConsole;
        [JsonProperty(PropertyName = "Use Discord Webhook")]
        public bool UseDiscord;
        [JsonProperty(PropertyName = "Discord Webhook URL")]
        public string WebhookUrl;
    }

    protected override void LoadDefaultMessages();
}

private class PluginConfig
{
    [JsonProperty(PropertyName = "Log to Console")]
    public bool LogToConsole;
    [JsonProperty(PropertyName = "Use Discord Webhook")]
    public bool UseDiscord;
    [JsonProperty(PropertyName = "Discord Webhook URL")]
    public string WebhookUrl;
}


```

---

## MLRSWipeBlock by Malmo - Allows blocking players from mounting the MLRS vehicle, if it's wipe blocked

```csharp
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System.Collections.Generic;

Oxide.Plugins
[Info("MLRS Wipe Block", "Malmo", "1.0.1")]
[Description("Blocks use of MLRS while wipe block")]
internal class MLRSWipeBlock : RustPlugin
{
    [PluginReference]
    private Plugin WipeBlock;
    private void OnServerInitialized();
     object CanMountEntity(BasePlayer player, MLRS entity);
    private int GetTimeLeft();
    private string GetTimeString(int time);
    private void ChatMessage(BasePlayer player, string messageName, object[] args);
    private string GetMessage(IPlayer player, string messageName, object[] args);
    private string GetMessage(string playerId, string messageName, object[] args);
    protected override void LoadDefaultMessages();
}


```

---

## ModdedWorkCarts by Kopter - Allows making modifications to Work Carts, such as adding an Auto Turret, Storage Boxes, ...

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Modded Work Carts", "Kopter", "1.0.2")]
[Description("Allows making modifications to Work Carts, such as adding Auto Turret, Storage")]
public class ModdedWorkCarts : RustPlugin
{
     int AutoTurretID;
     List<ulong> PlayersOnCoolDown;
    private const string StoragePrefab;
    private const string ChairPrefab;
    private const string InvisibleChairPrefab;
    private const string AutoTurretPrefab;
    private const string ElectricSwitchPrefab;
    private const string AutoTurretSound;
    private const string TurretPermission;
    public List<Vector3> ChairPositions;
    public List<Vector3> StoragePositions;
     Vector3 AutoTurretPosition;
     Vector3 ElectricSwitchPosition;
     void Init();
     void OnEntitySpawned(TrainCar WorkCart);
     object OnSwitchToggled(ElectricSwitch ElectricSwitch);
     object OnEntityTakeDamage(ElectricSwitch ElectricSwitch);
     void OnEntityKill(StorageContainer Storage);
    [ChatCommand("workcartturret")]
     void WorkCartTurretCommand(BasePlayer player, string command, string[] args);
     void SpawnChairs(TrainCar WorkCart);
     void SpawnStorage(TrainCar WorkCart);
     object SpawnAutoTurretAndSwitch(TrainCar WorkCart);
     void RemoveColliderProtection(BaseEntity Entity);
    private bool VerifyWorkCartFound(BasePlayer player, TrainCar WorkCart);
    private static AutoTurret GetWorkCartAutoTurret(TrainCar WorkCart);
    private ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Add a Auto Turret on top of the driver cabin")]
        public bool AutoTurret;
        [JsonProperty(PropertyName = "Add Storage Boxes on top of the fuel deposit")]
        public bool Storage;
        [JsonProperty(PropertyName = "Add chairs at the back of the Work Cart")]
        public bool Chairs;
        [JsonProperty(PropertyName = "Turret Command Cooldown in Minutes (If 0 there will be none)")]
        public float CommandCooldown;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
     string Lang(string key, string id, object[] args);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Add a Auto Turret on top of the driver cabin")]
    public bool AutoTurret;
    [JsonProperty(PropertyName = "Add Storage Boxes on top of the fuel deposit")]
    public bool Storage;
    [JsonProperty(PropertyName = "Add chairs at the back of the Work Cart")]
    public bool Chairs;
    [JsonProperty(PropertyName = "Turret Command Cooldown in Minutes (If 0 there will be none)")]
    public float CommandCooldown;
}


```

---

## MoneyTime by MrBlue - Pays players with Economics or Server Rewards money for playing

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Money Time", "Wulf", "2.3.0")]
[Description("Pays players with Economics money for playing")]
public class MoneyTime : CovalencePlugin
{
    private const string Perm;
    private Configuration _config;
    public class Configuration
    {
        [JsonProperty("Enable Economics as default currency")]
        public bool Economics;
        [JsonProperty("Enable Server Rewards as default currency")]
        public bool ServerRewards;
        [JsonProperty("Enable AFK API plugin support")]
        public bool AfkApi;
        [JsonProperty("Base payout amount")]
        public int BasePayout;
        [JsonProperty("Payout interval (seconds)")]
        public int PayoutInterval;
        [JsonProperty("Time alive bonus")]
        public bool TimeAliveBonus;
        [JsonProperty("Time alive multiplier")]
        public float TimeAliveMultiplier;
        [JsonProperty("Allow Permission-based Multipliers to stack")]
        public bool StackMultipliers;
        [JsonProperty("New player welcome bonus")]
        public float WelcomeBonus;
        [JsonProperty("Permission-based mulitipliers", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public SortedDictionary<string, float> PermissionMulitipliers;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private StoredData _storedData;
    private class StoredData
    {
        public Dictionary<string, PlayerInfo> Players;
        public StoredData();
    }

    private class PlayerInfo
    {
        public DateTime LastTimeAlive;
        public bool WelcomeBonus;
        public PlayerInfo();
    }

    private void SaveData();
    private void OnServerSave();
    [PluginReference]
    private Plugin Economics;
    private Plugin ServerRewards;
    private Plugin AFKAPI;
    private Dictionary<string, Values> _payOut;
    private Dictionary<string, double> _perms;
    private class Values
    {
        public double amount;
        public float time;
    }

    private void Init();
    private void InitializePlayer(IPlayer player, float current);
    private void OnServerInitialized();
    private void Unload();
    private void OnGroupPermissionGranted(string name, string perm);
    private void OnGroupPermissionRevoked(string name, string perm);
    private void OnUserPermissionGranted(string id, string perm);
    private void OnUserPermissionRevoked(string id, string perm);
    private void Edit(bool all, bool mine, string user);
    private void Payout(IPlayer player, double amount, string message);
    private void OnUserConnected(IPlayer player);
    private void OnUserDisconnected(IPlayer player);
    private void OnUserRespawn(IPlayer player);
    private string GetLang(string langKey, string playerId);
    private void Message(IPlayer player, string textOrLang, object[] args);
}

public class Configuration
{
    [JsonProperty("Enable Economics as default currency")]
    public bool Economics;
    [JsonProperty("Enable Server Rewards as default currency")]
    public bool ServerRewards;
    [JsonProperty("Enable AFK API plugin support")]
    public bool AfkApi;
    [JsonProperty("Base payout amount")]
    public int BasePayout;
    [JsonProperty("Payout interval (seconds)")]
    public int PayoutInterval;
    [JsonProperty("Time alive bonus")]
    public bool TimeAliveBonus;
    [JsonProperty("Time alive multiplier")]
    public float TimeAliveMultiplier;
    [JsonProperty("Allow Permission-based Multipliers to stack")]
    public bool StackMultipliers;
    [JsonProperty("New player welcome bonus")]
    public float WelcomeBonus;
    [JsonProperty("Permission-based mulitipliers", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public SortedDictionary<string, float> PermissionMulitipliers;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private class StoredData
{
    public Dictionary<string, PlayerInfo> Players;
    public StoredData();
}

private class PlayerInfo
{
    public DateTime LastTimeAlive;
    public bool WelcomeBonus;
    public PlayerInfo();
}

private class Values
{
    public double amount;
    public float time;
}


```

---

## MonumentAddons by WhiteThunder - Add entities, spawn points and more to monuments

```csharp
using Facepunch;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Plugins.MonumentAddonsExtensions;
using System;
using System.Collections;
using System.Collections.Generic;
using System.ComponentModel;
using System.Globalization;
using System.Linq;
using System.Reflection;
using System.Runtime.Serialization;
using System.Text.RegularExpressions;
using System.Text;
using UnityEngine;
using static IOEntity;
using static WireTool;
using Component = UnityEngine.Component;
using HumanNPCGlobal = global::HumanNPC;
using SkullTrophyGlobal = global::SkullTrophy;
using CustomInitializeCallback = System.Func<BasePlayer, string[], object>;
using CustomInitializeCallbackV2 = System.Func<BasePlayer, string[], System.ValueTuple<bool, object>>;
using CustomEditCallback = System.Func<BasePlayer, string[], UnityEngine.Component, Newtonsoft.Json.Linq.JObject, System.ValueTuple<bool, object>>;
using CustomSpawnCallback = System.Func<UnityEngine.Vector3, UnityEngine.Quaternion, Newtonsoft.Json.Linq.JObject, UnityEngine.Component>;
using CustomSpawnCallbackV2 = System.Func<System.Guid, UnityEngine.Component, UnityEngine.Vector3, UnityEngine.Quaternion, Newtonsoft.Json.Linq.JObject, UnityEngine.Component>;
using CustomCheckSpaceCallback = System.Func<UnityEngine.Vector3, UnityEngine.Quaternion, Newtonsoft.Json.Linq.JObject, bool>;
using CustomKillCallback = System.Action<UnityEngine.Component>;
using CustomUnloadCallback = System.Action<UnityEngine.Component>;
using CustomUpdateCallback = System.Action<UnityEngine.Component, Newtonsoft.Json.Linq.JObject>;
using CustomUpdateCallbackV2 = System.Func<UnityEngine.Component, Newtonsoft.Json.Linq.JObject, UnityEngine.Component>;
using CustomDisplayCallback = System.Action<UnityEngine.Component, Newtonsoft.Json.Linq.JObject, System.Text.StringBuilder>;
using CustomDisplayCallbackV2 = System.Action<UnityEngine.Component, Newtonsoft.Json.Linq.JObject, BasePlayer, System.Text.StringBuilder, float>;
using CustomSetDataCallback = System.Action<UnityEngine.Component, object>;
using Tuple1 = System.ValueTuple<object>;
using Tuple2 = System.ValueTuple<object, object>;
using Tuple3 = System.ValueTuple<object, object, object>;
using Tuple4 = System.ValueTuple<object, object, object, object>;

Oxide.Plugins
[Info("Monument Addons", "WhiteThunder", "0.18.4")]
[Description("Allows adding entities, spawn points and more to monuments.")]
internal class MonumentAddons : CovalencePlugin
{
    [PluginReference]
    private Plugin CopyPaste;
    private Plugin CustomVendingSetup;
    private Plugin EntityScaleManager;
    private Plugin MonumentFinder;
    private Plugin SignArtist;
    private MonumentAddons _plugin;
    private Configuration _config;
    private StoredData _data;
    private ProfileStateData _profileStateData;
    private const float MaxRaycastDistance;
    private const float TerrainProximityTolerance;
    private const float MaxFindDistanceSquared;
    private const float ShowVanillaDuration;
    private const string PermissionAdmin;
    private const string WireToolPlugEffect;
    private const string CargoShipPrefab;
    private const string CargoShipShortName;
    private const string DefaultProfileName;
    private const string DefaultUrlPattern;
    private static readonly int HitLayers;
    private static readonly Dictionary<string, string> JsonRequestHeaders;
    private static readonly Dictionary<string, string> OfficialProfileNames;
    private readonly HookCollection _dynamicMonumentHooks;
    private readonly ProfileStore _profileStore;
    private readonly OriginalProfileStore _originalProfileStore;
    private readonly ProfileManager _profileManager;
    private readonly CoroutineManager _coroutineManager;
    private readonly AddonComponentTracker _componentTracker;
    private readonly AdapterListenerManager _adapterListenerManager;
    private readonly ControllerFactory _controllerFactory;
    private readonly CustomAddonManager _customAddonManager;
    private readonly CustomMonumentManager _customMonumentManager;
    private readonly UniqueNameRegistry _uniqueNameRegistry;
    private readonly AdapterDisplayManager _adapterDisplayManager;
    private readonly MonumentHelper _monumentHelper;
    private readonly WireToolManager _wireToolManager;
    private readonly IOManager _ioManager;
    private readonly UndoManager _undoManager;
    private readonly ValueRotator<Color> _colorRotator;
    private readonly object True;
    private readonly object False;
    private ItemDefinition _waterDefinition;
    private ProtectionProperties _immortalProtection;
    private ActionDebounced _saveProfileStateDebounced;
    private StringBuilder _sb;
    private HashSet<string> _deployablePrefabs;
    private BaseEntity[] _entityBuffer;
    private Coroutine _startupCoroutine;
    private bool _serverInitialized;
    private bool _isLoaded;
    public MonumentAddons();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnNewSave();
    private void OnPluginLoaded(Plugin plugin);
    private void OnPluginUnloaded(Plugin plugin);
    private void OnEntitySpawned(BaseEntity entity);
    private object canRemove(BasePlayer player, BaseEntity entity);
    private object CanChangeGrade(BasePlayer player, BuildingBlock block, BuildingGrade.Enum grade);
    private object CanUpdateSign(BasePlayer player, ISignage signage);
    private void OnSignUpdated(ISignage signage, BasePlayer player);
    private void OnImagePost(BasePlayer player, string url, bool raw, ISignage signage, uint textureIndex);
    private object OnSprayRemove(SprayCanSpray spray, BasePlayer player);
    private void OnEntityScaled(BaseEntity entity, float scale);
    private Component OnTelekinesisFindFailed(BasePlayer player);
    private Tuple<Component, Component> OnTelekinesisStart(BasePlayer player, BaseEntity entity);
    private object CanStartTelekinesis(BasePlayer player, Component moveComponent, Component rotateComponent);
    private void OnTelekinesisStarted(BasePlayer player, Component moveComponent, Component rotateComponent);
    private void OnTelekinesisStopped(BasePlayer player, Component moveComponent, Component rotateComponent);
    private Dictionary<string, object> OnCustomVendingSetupDataProvider(NPCVendingMachine vendingMachine);
    private bool CheckDependencies();
    private float GetEntityScale(BaseEntity entity);
    private bool TryScaleEntity(BaseEntity entity, float scale);
    private void SkinSign(ISignage signage, SignArtistImage[] signArtistImages);
    private JObject MigrateVendingProfile(NPCVendingMachine vendingMachine);
    private void RefreshVendingProfile(NPCVendingMachine vendingMachine);
    private static class PasteUtils
    {
        private static readonly string[] CopyPasteArgs;
        private static VersionNumber _requiredVersion;
        public static bool IsCopyPasteCompatible(Plugin copyPaste);
        public static bool DoesPasteExist(string filename);
        public static Action PasteWithCancelCallback(Plugin copyPaste, PasteData pasteData, Vector3 position, float yRotation, Action<BaseEntity> onEntityPasted, Action onPasteCompleted);
    }

    [HookMethod(nameof(API_IsMonumentEntity))]
    public object API_IsMonumentEntity(BaseEntity entity);
    [HookMethod(nameof(API_GetMonumentEntityGuid))]
    public object API_GetMonumentEntityGuid(BaseEntity entity);
    private static class ExposedHooks
    {
        public static void OnMonumentAddonsInitialized();
        public static void OnMonumentEntitySpawned(BaseEntity entity, Component monument, Guid guid);
        public static void OnMonumentPrefabCreated(GameObject gameObject, Component monument, Guid guid);
        public static object OnDynamicMonument(BaseEntity entity);
    }

    [Command("maspawn")]
    private void CommandSpawn(IPlayer player, string cmd, string[] args);
    [Command("maedit")]
    private void CommandEdit(IPlayer player, string cmd, string[] args);
    [Command("maprefab")]
    private void CommandPrefab(IPlayer player, string cmd, string[] args);
    [Command("masave")]
    private void CommandSave(IPlayer player, string cmd, string[] args);
    [Command("makill")]
    private void CommandKill(IPlayer player, string cmd, string[] args);
    [Command("maundo")]
    private void CommandUndo(IPlayer player, string cmd, string[] args);
    [Command("masetid")]
    private void CommandSetId(IPlayer player, string cmd, string[] args);
    [Command("masetdir")]
    private void CommandSetDirection(IPlayer player, string cmd, string[] args);
    [Command("maskull")]
    private void CommandSkull(IPlayer player, string cmd, string[] args);
    [Command("matrophy")]
    private void CommandTrophy(IPlayer player, string cmd, string[] args);
    [Command("maskin")]
    private void CommandSkin(IPlayer player, string cmd, string[] args);
    [Command("maflag")]
    private void CommandFlag(IPlayer player, string cmd, string[] args);
    private void AddProfileDescription(StringBuilder sb, IPlayer player, ProfileController profileController);
    [Command("macardlevel")]
    private void CommandLevel(IPlayer player, string cmd, string[] args);
    [Command("mapuzzle")]
    private void CommandPuzzle(IPlayer player, string cmd, string[] args);
    private void SubCommandPuzzleHelp(IPlayer player, string cmd);
    [Command("maprofile")]
    private void CommandProfile(IPlayer player, string cmd, string[] args);
    private void SubCommandProfileHelp(IPlayer player);
    [Command("mainstall")]
    private void CommandInstallProfile(IPlayer player, string cmd, string[] args);
    private void SharedCommandInstallProfile(IPlayer player, string[] args);
    [Command("mashow")]
    private void CommandShow(IPlayer player, string cmd, string[] args);
    [Command("maspawngroup", "masg")]
    private void CommandSpawnGroup(IPlayer player, string cmd, string[] args);
    private void SubCommandSpawnGroupHelp(IPlayer player, string cmd);
    [Command("maspawnpoint", "masp")]
    private void CommandSpawnPoint(IPlayer player, string cmd, string[] args);
    private void SubCommandSpawnPointHelp(IPlayer player, string cmd);
    [Command("mapaste")]
    private void CommandPaste(IPlayer player, string cmd, string[] args);
    private void AddSpawnGroupInfo(IPlayer player, StringBuilder sb, SpawnGroup spawnGroup, int spawnPointCount);
    [Command("mashowvanilla")]
    private void CommandShowVanillaSpawns(IPlayer player, string cmd, string[] args);
    [Command("magenerate")]
    private void CommandGenerateSpawnPointProfile(IPlayer player, string cmd, string[] args);
    [Command("mawire")]
    private void CommandWire(IPlayer player, string cmd, string[] args);
    [Command("mahelp")]
    private void CommandHelp(IPlayer player, string cmd, string[] args);
    [HookMethod(nameof(API_RegisterCustomAddon))]
    public Dictionary<string, object> API_RegisterCustomAddon(Plugin plugin, string addonName, Dictionary<string, object> addonSpec);
    [HookMethod(nameof(API_RegisterCustomMonument))]
    public object API_RegisterCustomMonument(Plugin plugin, string monumentName, Component component, Bounds bounds);
    [HookMethod(nameof(API_UnregisterCustomMonument))]
    public object API_UnregisterCustomMonument(Plugin plugin, Component component);
    private static class ObjectCache
    {
        private static readonly object True;
        private static readonly object False;
        private static class StaticObjectCache
        {
            private static readonly Dictionary<T, object> _cacheByValue;
            public static object Get(T value);
        }

        public static object Get(T value);
        public static object Get(bool value);
    }

    private bool VerifyPlayer(IPlayer player, BasePlayer basePlayer);
    private bool VerifyHasPermission(IPlayer player, string perm);
    private bool VerifyValidInt(IPlayer player, string arg, int value, T errorFormatter);
    private bool VerifyValidFloat(IPlayer player, string arg, float value, T errorFormatter);
    private bool VerifyValidBool(IPlayer player, string arg, bool value, T errorFormatter);
    private bool VerifyValidEnumValue(IPlayer player, string arg, T enumValue);
    private bool VerifyMonumentFinderLoaded(IPlayer player);
    private bool VerifyProfileSelected(IPlayer player, ProfileController profileController);
    private bool VerifyHitPosition(IPlayer player, Vector3 position);
    private bool VerifyAtMonument(IPlayer player, Vector3 position, BaseMonument closestMonument);
    private bool VerifyLookingAtMonumentPosition(IPlayer player, Vector3 position, BaseMonument closestMonument);
    private bool VerifyValidModderPrefab(IPlayer player, string[] args, string prefabPath);
    private bool VerifyValidEntityPrefab(IPlayer player, string prefabArg, string prefabPath);
    private bool VerifyValidEntityPrefabOrCustomAddon(IPlayer player, string prefabArg, string prefabPath, CustomAddonDefinition addonDefinition);
    private bool VerifyValidEntityPrefabOrDeployable(IPlayer player, string[] args, string prefabPath, CustomAddonDefinition addonDefinition, ulong skinId);
    private bool VerifySpawnGroupPrefabOrCustomAddon(IPlayer player, SpawnGroupData spawnGroupData, string prefabArg, WeightedPrefabData weightedPrefabData);
    private bool VerifyLookingAtAdapter(IPlayer player, AdapterFindResult<TAdapter, TController> findResult, TFormatter errorFormatter);
    private bool VerifyLookingAtAdapter(IPlayer player, TAdapter adapter, TController controller, TFormatter errorFormatter);
    private bool VerifyLookingAtAdapter(IPlayer player, TController controller, TFormatter errorFormatter);
    private bool VerifySpawnGroupFound(IPlayer player, string partialGroupName, BaseMonument monument, SpawnGroupController spawnGroupController);
    private bool VerifyProfileNameAvailable(IPlayer player, string profileName);
    private bool VerifyProfileExists(IPlayer player, string profileName, ProfileController controller);
    private bool VerifyProfile(IPlayer player, string[] args, ProfileController controller, LangEntry0 syntaxLangEntry);
    private bool VerifySpawnGroupNameAvailable(IPlayer player, Profile profile, BaseMonument monument, string spawnGroupName, SpawnGroupController spawnGroupController);
    private bool VerifyEntityComponent(IPlayer player, BaseEntity entity, T component, LangEntry0 errorLangEntry);
    private bool VerifyCanLoadProfile(IPlayer player, string profileName, Profile profile);
    private AdapterFindResult<TAdapter, TController> FindHitAdapter(BasePlayer basePlayer, RaycastHit hit);
    private AdapterFindResult<TAdapter, TController> FindClosestNearbyAdapter(Vector3 position);
    private AdapterFindResult<TAdapter, TController> FindAdapter(BasePlayer basePlayer);
    private AdapterFindResult<TAdapter, BaseController> FindAdapter(BasePlayer basePlayer);
    private AdapterFindResult<TransformAdapter, BaseController> FindAdapter(BasePlayer basePlayer);
    private IEnumerable<SpawnGroupController> FindSpawnGroups(string partialGroupName, string monumentIdentifier, Profile profile, bool partialMatch);
    private PuzzleReset FindConnectedPuzzleReset(IOSlot[] slotList, HashSet<IOEntity> visited);
    private PuzzleReset FindConnectedPuzzleReset(IOEntity ioEntity, HashSet<IOEntity> visited);
    public static void LogInfo(string message);
    public static void LogError(string message);
    public static void LogWarning(string message);
    private static bool RenameDictKey(Dictionary<string, TValue> dict, string oldName, string newName);
    private static HashSet<string> DetermineAllDeployablePrefabs();
    private static bool IsKeyBindArg(string arg);
    private static bool TryRaycast(BasePlayer player, RaycastHit hit, float maxDistance);
    private static bool TrySphereCast(BasePlayer player, RaycastHit hit, int layerMask, float radius, float maxDistance);
    private static bool TryGetHitPosition(BasePlayer player, Vector3 position);
    public static T GetLookEntitySphereCast(BasePlayer player, int layerMask, float radius, float maxDistance);
    private static void SendEffect(BasePlayer player, string effectPrefab);
    private static bool IsOnTerrain(Vector3 position);
    private static string GetShortName(string prefabName);
    private static void DetermineLocalTransformData(Vector3 position, BasePlayer basePlayer, BaseMonument monument, Vector3 localPosition, Vector3 localRotationAngles, bool isOnTerrain, bool flipRotation);
    private static void DestroyProblemComponents(BaseEntity entity);
    private static bool HasRigidBody(BaseEntity entity);
    private static bool IsRedirectSkin(ulong skinId, string alternativeShortName);
    private static T FindPrefabComponent(string prefabName);
    private static BaseEntity FindPrefabBaseEntity(string prefabName);
    private static string FormatTime(double seconds);
    private static void BroadcastEntityTransformChange(BaseEntity entity);
    private static void EnableSavingRecursive(BaseEntity entity, bool enableSaving);
    private static IEnumerator WaitWhileWithTimeout(Func<bool> predicate, float timeoutSeconds);
    private static bool TryParseEnum(string arg, T enumValue);
    private static float GetTimeToNextSpawn(SpawnGroup spawnGroup);
    private static float GetTimeToNextSpawn(IndividualSpawner spawner);
    private static List<MonumentTier> GetTierList(MonumentTier tier);
    private static MonumentTier GetMonumentTierMask(Vector3 position);
    private static BaseEntity FindValidEntity(ulong entityId);
    private static IEnumerator KillEntitiesRoutine(ICollection<BaseEntity> entityList);
    private static CustomSpawnPoint GetSpawnPoint(BaseEntity entity);
    private static bool IsSpawnPointEntity(BaseEntity entity, SpawnPointAdapter adapter);
    private static bool IsSpawnPointEntity(BaseEntity entity);
    private bool IsTransformAddon(Component component, TransformAdapter adapter, BaseController controller);
    private bool IsTransformAddon(Component component, BaseController controller);
    private bool HasAdminPermission(string userId);
    private bool HasAdminPermission(BasePlayer player);
    private bool IsDynamicMonument(BaseEntity entity);
    private bool IsPlayerParentedToDynamicMonument(BasePlayer player, Vector3 position, BaseMonument monument);
    private BaseMonument GetClosestMonument(BasePlayer player, Vector3 position);
    private List<BaseMonument> GetDynamicMonumentInstances(uint prefabId);
    private List<BaseMonument> GetMonumentsByIdentifier(string monumentIdentifier);
    private IEnumerator SpawnAllProfilesRoutine();
    private void StartupRoutine();
    private void DownloadProfile(IPlayer player, string url, Action<Profile> successCallback, Action<string> errorCallback);
    private string DeterminePrefabFromPlayerActiveDeployable(BasePlayer basePlayer, ulong skinId);
    private bool ShouldRecommendSpawnPoints(string prefabName);
    private static class StringUtils
    {
        public static bool EqualsCaseInsensitive(string a, string b);
        public static bool Contains(string haystack, string needle);
    }

    private static class SearchUtils
    {
        public static List<string> FindModderPrefabMatches(string partialName);
        public static List<T> FindPrefabMatches(IEnumerable<T> sourceList, Func<T, string> selector, string partialName, UniqueNameRegistry uniqueNameRegistry);
        public static List<string> FindEntityPrefabMatches(string partialName, UniqueNameRegistry uniqueNameRegistry);
        public static List<T> FindCustomAddonMatches(IEnumerable<T> sourceList, Func<T, string> selector, string partialName);
        public static List<CustomAddonDefinition> FindCustomAddonMatches(string partialName, IEnumerable<CustomAddonDefinition> customAddons);
        public static List<T> FindMatches(IEnumerable<T> sourceList, Func<T, bool>[] predicateList);
    }

    private static class EntityUtils
    {
        public static T GetNearbyEntity(BaseEntity originEntity, float maxDistance, int layerMask, string filterShortPrefabName);
        public static T GetClosestNearbyEntity(Vector3 position, float maxDistance, int layerMask, Func<T, bool> predicate);
        public static T GetClosestNearbyComponent(Vector3 position, float maxDistance, int layerMask, Func<T, bool> predicate);
        public static void ConnectNearbyVehicleSpawner(VehicleVendor vehicleVendor);
        public static void ConnectNearbyVehicleVendor(VehicleSpawner vehicleSpawner);
        public static void ConnectNearbyDoor(DoorManipulator doorManipulator);
        private static T GetClosestComponent(Vector3 position, List<T> componentList, Func<T, bool> predicate);
    }

    private static class EntitySetupUtils
    {
        public static bool ShouldBeImmortal(BaseEntity entity);
        public static void PreSpawnShared(BaseEntity entity);
        public static void PostSpawnShared(MonumentAddons plugin, BaseEntity entity, bool enableSaving);
    }

    private static class BooleanParser
    {
        private static string[] _booleanYesValues;
        private static string[] _booleanNoValues;
        public static bool TryParse(string arg, bool value);
    }

    private class ValueRotator
    {
        private T[] _values;
        private int _index;
        public ValueRotator(T[] values);
        public T GetNext();
        public void Reset();
    }

    private class HookCollection
    {
        private MonumentAddons _plugin;
        private string[] _hookNames;
        private Func<bool> _shouldSubscribe;
        private bool _isSubscribed;
        public HookCollection(MonumentAddons plugin, string[] hookNames, Func<bool> shouldSubscribe);
        public void Refresh();
        public void Subscribe();
        public void Unsubscribe();
    }

    private class UniqueNameRegistry
    {
        private Dictionary<string, string> _uniqueNameByPrefabPath;
        public void OnServerInitialized();
        public string GetUniqueShortName(string prefabPath);
        private string[] GetSegments(string prefabPath);
        private string GetPartialPath(string[] segments, int numSegments);
        private void BuildIndex();
    }

    private class EmptyMonoBehavior : MonoBehaviour
    {
    }

    private class CoroutineManager
    {
        public static Coroutine StartGlobalCoroutine(IEnumerator enumerator);
        private static IEnumerator CallbackRoutine(Coroutine dependency, Action action);
        private MonoBehaviour _coroutineComponent;
        public Coroutine StartCoroutine(IEnumerator enumerator);
        public Coroutine StartCallbackRoutine(Coroutine coroutine, Action callback);
        public void StopAll();
        public void Destroy();
    }

    private class WireToolManager
    {
        private const float DrawDuration;
        private const float DrawSlotRadius;
        private const float PreviewDotRadius;
        private const float InputCooldown;
        private const float HoldToClearDuration;
        private const float DrawIntervalDuration;
        private const float DrawIntervalWithBuffer;
        private const float MinAngleDot;
        private class WireSession
        {
            public BasePlayer Player { get; set; }
            public WireColour? WireColor;
            public IOType WireType;
            public EntityAdapter Adapter;
            public IOSlot StartSlot;
            public int StartSlotIndex;
            public bool IsSource;
            public float LastInput;
            public float SecondaryPressedTime;
            public float LastDrawTime;
            public List<Vector3> Points;
            public WireSession(BasePlayer player);
            public void Reset(bool refreshPoints);
            public void StartConnection(EntityAdapter adapter, IOSlot startSlot, int slotIndex, bool isSource);
            public void AddPoint(Vector3 position);
            public void RemoveLast();
            public bool IsOnInputCooldown();
            public bool IsOnDrawCooldown();
            public void RefreshPoints();
        }

        private MonumentAddons _plugin;
        private ProfileStore _profileStore;
        private AddonComponentTracker _componentTracker { get; set; }
        private List<WireSession> _playerSessions;
        private Timer _timer;
        public WireToolManager(MonumentAddons plugin, ProfileStore profileStore);
        public bool HasPlayer(BasePlayer player);
        public void StartOrUpdateSession(BasePlayer player, WireColour? wireColor);
        public void StopSession(BasePlayer player);
        public void Unload();
        private WireSession GetPlayerSession(BasePlayer player, int index);
        private WireSession GetPlayerSession(BasePlayer player);
        private void DestroySession(WireSession session, int index);
        private EntityAdapter GetLookIOAdapter(BasePlayer player, IOEntity ioEntity);
        private IOSlot GetClosestIOSlot(IOEntity ioEntity, Ray ray, float minDot, int index, float highestDot, bool wantsSourceSlot, bool? wantsOccupiedSlots);
        private IOSlot GetClosestIOSlot(IOEntity ioEntity, Ray ray, float minDot, int index, bool isSourceSlot, bool? wantsOccupiedSlots);
        private bool CanPlayerUseTool(BasePlayer player);
        private Color DetermineSlotColor(IOSlot slot);
        private void ShowSlots(BasePlayer player, IOEntity ioEntity, bool showSourceSlots);
        private void DrawSessionState(WireSession session);
        private void MaybeStartWire(WireSession session, EntityAdapter adapter);
        private void MaybeEndWire(WireSession session, EntityAdapter adapter);
        private void MaybeClearWire(WireSession session);
        private void ProcessPlayers();
    }

    private abstract class DictionaryKeyConverter : JsonConverter
    {
        public virtual string KeyToString(TKey key);
        public abstract TKey KeyFromString(string key);
        public override bool CanConvert(Type objectType);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    }

    private class HtmlColorConverter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    }

    private class ActionDebounced
    {
        private PluginTimers _pluginTimers;
        private float _seconds;
        private Action _action;
        private Timer _timer;
        public ActionDebounced(PluginTimers pluginTimers, float seconds, Action action);
        public void Schedule();
        public void Flush();
    }

    private static bool HasDeepItems(Dictionary<TKey, TValue> dict);
    private class MonumentHelper
    {
        private MonumentAddons _plugin;
        private Plugin _monumentFinder { get; set; }
        private Dictionary<DungeonGridInfo, MonumentInfo> _entranceToMonument;
        public MonumentHelper(MonumentAddons plugin);
        public void OnServerInitialized();
        public List<BaseMonument> FindMonumentsByAlias(string alias);
        public List<BaseMonument> FindMonumentsByShortName(string shortName);
        public MonumentAdapter GetClosestMonumentAdapter(Vector3 position);
        public bool IsMonumentUnique(string shortName);
        private List<BaseMonument> WrapFindMonumentResults(List<Dictionary<string, object>> dictList);
        public MonumentInfo GetMonumentFromTunnel(DungeonGridCell dungeonGridCell);
        private MonumentInfo GetMonumentFromEntrance(DungeonGridInfo dungeonGridInfo);
    }

    private static class PhoneUtils
    {
        public const string Tunnel;
        public const string UnderwaterLab;
        public const string CargoShip;
        private static readonly Regex SplitCamelCaseRegex;
        private static readonly string[] PolymorphicMonumentVariants;
        public static void NameTelephone(Telephone telephone, BaseMonument monument, Vector3 position, MonumentHelper monumentHelper);
        private static string GetFTLCorridorPhoneName(string tunnelName, Vector3 position);
        private static string GetFTLPhoneName(string tunnelAlias, DungeonGridCell dungeonGridCell, BaseMonument monument, Vector3 position, MonumentHelper monumentHelper);
        private static string GetFTLTrainStationPhoneName(MonumentInfo attachedMonument, string tunnelName, Vector3 position, MonumentHelper monumentHelper);
        private static string GetUnderwaterLabPhoneName(DungeonBaseLink link, Vector3 position);
        private static string GetDynamicMonumentPhoneName(DynamicMonument monument, Telephone phone);
        private static bool ShouldAppendCoordinate(string monumentShortName, MonumentHelper monumentHelper);
        private static string SplitCamelCase(string camelCase);
    }

    private abstract class BaseMonument
    {
        public Component Object { get; set; }
        public virtual string PrefabName { get; set; }
        public virtual string ShortName { get; set; }
        public virtual string Alias { get; set; }
        public virtual string UniqueDisplayName { get; set; }
        public virtual string UniqueName { get; set; }
        public virtual Vector3 Position { get; set; }
        public virtual Quaternion Rotation { get; set; }
        public virtual bool IsValid { get; set; }
        protected BaseMonument(Component component);
        public virtual Vector3 TransformPoint(Vector3 localPosition);
        public virtual Vector3 InverseTransformPoint(Vector3 worldPosition);
        public abstract Vector3 ClosestPointOnBounds(Vector3 position);
        public abstract bool IsInBounds(Vector3 position);
        public virtual bool IsEquivalentTo(BaseMonument other);
    }

    private class MonumentAdapter : BaseMonument
    {
        public override string PrefabName { get; set; }
        public override string ShortName { get; set; }
        public override string Alias { get; set; }
        public override string UniqueDisplayName { get; set; }
        public override Vector3 Position { get; set; }
        public override Quaternion Rotation { get; set; }
        private Dictionary<string, object> _monumentInfo;
        public MonumentAdapter(Dictionary<string, object> monumentInfo);
        public override Vector3 TransformPoint(Vector3 localPosition);
        public override Vector3 InverseTransformPoint(Vector3 worldPosition);
        public override Vector3 ClosestPointOnBounds(Vector3 position);
        public override bool IsInBounds(Vector3 position);
    }

    private class DynamicMonument : BaseMonument, IDynamicMonument, IEntityMonument
    {
        public BaseEntity RootEntity { get; set; }
        public bool IsMobile { get; set; }
        public override string PrefabName { get; set; }
        public override string ShortName { get; set; }
        public override string UniqueName { get; set; }
        public override string UniqueDisplayName { get; set; }
        public override bool IsValid { get; set; }
        public NetworkableId EntityId { get; set; }
        protected OBB BoundingBox { get; set; }
        public DynamicMonument(BaseEntity entity, bool isMobile);
        public DynamicMonument(BaseEntity entity);
        public override Vector3 ClosestPointOnBounds(Vector3 position);
        public override bool IsInBounds(Vector3 position);
        public override bool IsEquivalentTo(BaseMonument other);
    }

    private class CustomMonument : BaseMonument
    {
        public readonly Plugin OwnerPlugin;
        public readonly Component Component;
        public Bounds Bounds;
        private readonly string _monumentName;
        private Vector3 _position;
        public override string PrefabName { get; set; }
        public override string ShortName { get; set; }
        public override string UniqueName { get; set; }
        public override string UniqueDisplayName { get; set; }
        public override Vector3 Position { get; set; }
        public OBB BoundingBox { get; set; }
        public CustomMonument(Plugin ownerPlugin, Component component, string monumentName, Bounds bounds);
        public override Vector3 ClosestPointOnBounds(Vector3 position);
        public override bool IsInBounds(Vector3 position);
        public override bool IsEquivalentTo(BaseMonument other);
    }

    private class CustomEntityMonument : CustomMonument, IEntityMonument
    {
        public BaseEntity RootEntity { get; set; }
        public NetworkableId EntityId { get; set; }
        public bool IsMobile { get; set; }
        public CustomEntityMonument(Plugin ownerPlugin, BaseEntity entity, string monumentName, Bounds bounds);
    }

    private class CustomMonumentComponent : FacepunchBehaviour
    {
        public static CustomMonumentComponent AddToMonument(CustomMonumentManager manager, CustomMonument monument);
        public CustomMonument Monument { get; set; }
        private CustomMonumentManager _manager;
        private void OnDestroy();
    }

    private class CustomMonumentManager
    {
        private readonly MonumentAddons _plugin;
        public readonly List<CustomMonument> MonumentList;
        public CustomMonumentManager(MonumentAddons plugin);
        public void Register(CustomMonument monument);
        public void Unregister(CustomMonument monument);
        public void UnregisterAllForPlugin(Plugin plugin);
        public CustomMonument FindByComponent(Component component);
        public CustomMonument FindByPosition(Vector3 position);
        public int CountMonumentByName(string name);
        public List<BaseMonument> FindMonumentsByName(string name);
        private IEnumerator KillRoutine(List<BaseAdapter> adapterList);
    }

    private abstract class BaseUndo
    {
        private const float ExpireAfterSeconds;
        public virtual bool IsValid { get; set; }
        protected readonly MonumentAddons _plugin;
        protected readonly ProfileController _profileController;
        protected readonly string _monumentIdentifier;
        private readonly float _undoTime;
        protected Profile Profile { get; set; }
        private bool IsExpired { get; set; }
        private bool ProfileExists { get; set; }
        protected BaseUndo(MonumentAddons plugin, ProfileController profileController, string monumentIdentifier);
        public abstract void Undo(BasePlayer player);
    }

    private class UndoKill : BaseUndo
    {
        protected readonly BaseData _data;
        public UndoKill(MonumentAddons plugin, ProfileController profileController, string monumentIdentifier, BaseData data);
        public override void Undo(BasePlayer player);
    }

    private class UndoKillSpawnPoint : UndoKill
    {
        private readonly SpawnGroupData _spawnGroupData;
        private readonly SpawnPointData _spawnPointData;
        public UndoKillSpawnPoint(MonumentAddons plugin, ProfileController profileController, string monumentIdentifier, SpawnGroupData spawnGroupData, SpawnPointData spawnPointData);
        public override void Undo(BasePlayer player);
    }

    private class UndoManager
    {
        private static void CleanStack(Stack<BaseUndo> stack);
        private readonly Dictionary<ulong, Stack<BaseUndo>> _undoStackByPlayer;
        public bool TryUndo(BasePlayer player);
        public void AddUndo(BasePlayer player, BaseUndo undoAction);
        private Stack<BaseUndo> EnsureUndoStack(BasePlayer player);
        private Stack<BaseUndo> GetUndoStack(BasePlayer player);
    }

    private class AddonComponent : FacepunchBehaviour, IAddonComponent
    {
        public static AddonComponent AddToComponent(AddonComponentTracker componentTracker, Component hostComponent, TransformAdapter adapter);
        public static void RemoveFromComponent(Component component);
        public static AddonComponent GetForComponent(BaseEntity entity);
        public TransformAdapter Adapter { get; set; }
        private Component _hostComponent;
        private AddonComponentTracker _componentTracker;
        private void OnDestroy();
    }

    private class AddonComponentTracker
    {
        private static bool IsComponentValid(Component component);
        private HashSet<Component> _trackedComponents;
        public void RegisterComponent(Component component);
        public void UnregisterComponent(Component component);
        public bool IsAddonComponent(Component component);
        public bool IsAddonComponent(Component component, TAdapter adapter, TController controller);
        public bool IsAddonComponent(Component component, TController controller);
    }

    private abstract class BaseAdapter
    {
        public abstract bool IsValid { get; set; }
        public BaseData Data { get; set; }
        public BaseController Controller { get; set; }
        public BaseMonument Monument { get; set; }
        public MonumentAddons Plugin { get; set; }
        public ProfileController ProfileController { get; set; }
        public Profile Profile { get; set; }
        protected Configuration _config { get; set; }
        protected ProfileStateData _profileStateData { get; set; }
        protected IOManager _ioManager { get; set; }
        public IEnumerator WaitInstruction { get; set; }
        protected BaseAdapter(BaseData data, BaseController controller, BaseMonument monument);
        public abstract void Spawn();
        public abstract void Kill();
        public abstract void OnComponentDestroyed(Component component);
        public virtual void DetachSavedEntities();
        public virtual void PreUnload();
    }

    private abstract class TransformAdapter : BaseAdapter
    {
        public BaseTransformData TransformData { get; set; }
        public abstract Component Component { get; set; }
        public abstract Transform Transform { get; set; }
        public abstract Vector3 Position { get; set; }
        public abstract Quaternion Rotation { get; set; }
        public Vector3 LocalPosition { get; set; }
        public Quaternion LocalRotation { get; set; }
        public Vector3 IntendedPosition { get; set; }
        public Quaternion IntendedRotation { get; set; }
        public virtual bool IsAtIntendedPosition { get; set; }
        protected TransformAdapter(BaseTransformData transformData, BaseController controller, BaseMonument monument);
        public virtual bool TryRecordUpdates(Transform moveTransform, Transform rotateTransform);
    }

    private abstract class BaseController
    {
        public ProfileController ProfileController { get; set; }
        public BaseData Data { get; set; }
        public List<BaseAdapter> Adapters { get; set; }
        public MonumentAddons Plugin { get; set; }
        public Profile Profile { get; set; }
        private bool _enabled;
        protected BaseController(ProfileController profileController, BaseData data);
        public abstract BaseAdapter CreateAdapter(BaseMonument monument);
        public virtual void OnAdapterSpawned(BaseAdapter adapter);
        public virtual void OnAdapterKilled(BaseAdapter adapter);
        public virtual BaseAdapter SpawnAtMonument(BaseMonument monument);
        public virtual IEnumerator SpawnAtMonumentsRoutine(IEnumerable<BaseMonument> monumentList);
        public virtual Coroutine Kill(BaseData data);
        public bool HasAdapterForMonument(BaseMonument monument);
        public Coroutine Kill();
        public void PreUnload();
        public IEnumerator KillRoutine();
        public void DetachSavedEntities();
        public BaseAdapter FindAdapterForMonument(BaseMonument monument);
    }

    private class PrefabAdapter : TransformAdapter
    {
        public GameObject GameObject { get; set; }
        public PrefabData PrefabData { get; set; }
        public override Component Component { get; set; }
        public override Transform Transform { get; set; }
        public override Vector3 Position { get; set; }
        public override Quaternion Rotation { get; set; }
        public override bool IsValid { get; set; }
        private Transform _transform;
        public PrefabAdapter(BaseController controller, PrefabData prefabData, BaseMonument monument);
        public override void Spawn();
        public override void Kill();
        public void HandleChanges();
        public override void OnComponentDestroyed(Component component);
    }

    private class PrefabController : BaseController, IUpdateableController
    {
        public PrefabData PrefabData { get; set; }
        public PrefabController(ProfileController profileController, PrefabData prefabData);
        public override BaseAdapter CreateAdapter(BaseMonument monument);
        public Coroutine StartUpdateRoutine();
        private IEnumerator UpdateRoutine();
    }

    private class EntityAdapter : TransformAdapter
    {
        private class IOEntityOverrideInfo
        {
            public Dictionary<int, Vector3> Inputs;
            public Dictionary<int, Vector3> Outputs;
        }

        private static readonly Dictionary<string, IOEntityOverrideInfo> IOOverridesByEntity;
        public BaseEntity Entity { get; set; }
        public EntityData EntityData { get; set; }
        public virtual bool IsDestroyed { get; set; }
        public override Component Component { get; set; }
        public override Transform Transform { get; set; }
        public override Vector3 Position { get; set; }
        public override Quaternion Rotation { get; set; }
        public override bool IsValid { get; set; }
        private Transform _transform;
        private PuzzleResetHandler _puzzleResetHandler;
        private BuildingGrade.Enum IntendedBuildingGrade { get; set; }
        public EntityAdapter(BaseController controller, EntityData entityData, BaseMonument monument);
        public override void Spawn();
        public override void Kill();
        public override void OnComponentDestroyed(Component component);
        public void UpdateScale();
        public void UpdateSkin();
        public override bool TryRecordUpdates(Transform moveTransform, Transform rotateTransform);
        public virtual void HandleChanges();
        public void UpdateSkullName();
        public void UpdateHuntingTrophy();
        public void UpdateIOConnections();
        public void MaybeProvidePower();
        public override void PreUnload();
        public void HandlePuzzleReset();
        protected virtual void PreEntitySpawn();
        protected virtual void PostEntitySpawn();
        protected virtual void PreEntityKill();
        public override void DetachSavedEntities();
        protected BaseEntity CreateEntity(Vector3 position, Quaternion rotation);
        private bool ShouldEnableSaving(BaseEntity entity);
        private void UpdatePosition();
        private BaseEntity FindAndCleanupDuplicateEntities(BaseEntity ignoreEntity);
        private List<CCTV_RC> GetNearbyStaticCameras();
        private void GatherStaticCameras(ComputerStation computerStation);
        private BaseEntity GetEntityToMove();
        private void UpdateBuildingGrade();
        private void UpdatePuzzle();
        private void UpdateCardReaderLevel();
        private void DisableFlags();
        private void EnableFlags();
        private void UpdateIOEntitySlotPositions(IOEntity ioEntity);
        private void SetupIOSlot(IOSlot slot, IOEntity otherEntity, int otherSlotIndex, WireColour color);
        private void ClearIOSlot(IOEntity ioEntity, IOSlot slot);
    }

    private class EntityController : BaseController, IUpdateableController
    {
        public EntityData EntityData { get; set; }
        private Dictionary<string, object> _vendingDataProvider;
        public EntityController(ProfileController profileController, EntityData entityData);
        public override BaseAdapter CreateAdapter(BaseMonument monument);
        public override void OnAdapterSpawned(BaseAdapter adapter);
        public override void OnAdapterKilled(BaseAdapter adapter);
        public Coroutine StartUpdateRoutine();
        public Dictionary<string, object> GetVendingDataProvider();
        private IEnumerator HandleChangesRoutine();
    }

    private class SignAdapter : EntityAdapter
    {
        public SignAdapter(BaseController controller, EntityData entityData, BaseMonument monument);
        public override void PreUnload();
        public uint[] GetTextureIds();
        public void SetTextureIds(uint[] textureIds);
        public void SkinSign();
        protected override void PreEntitySpawn();
        protected override void PreEntityKill();
        private void DeleteSignFiles();
    }

    private class SignController : EntityController
    {
        public SignController(ProfileController profileController, EntityData data);
        protected SignAdapter _primaryAdapter;
        public override BaseAdapter CreateAdapter(BaseMonument monument);
        public override void OnAdapterSpawned(BaseAdapter adapter);
        public override void OnAdapterKilled(BaseAdapter adapter);
        public void UpdateSign(uint[] textureIds);
    }

    private class CCTVEntityAdapter : EntityAdapter
    {
        private int _idSuffix;
        private string _cachedIdentifier;
        private string _savedIdentifier { get; set; }
        public CCTVEntityAdapter(BaseController controller, EntityData entityData, BaseMonument monument, int idSuffix);
        public override void PreUnload();
        protected override void PreEntitySpawn();
        protected override void PostEntitySpawn();
        public override void OnComponentDestroyed(Component component);
        public override void HandleChanges();
        public void UpdateIdentifier();
        public void UpdateDirection();
        public string GetIdentifier();
        private void SetIdentifier(string id);
        private List<ComputerStation> GetNearbyStaticComputerStations();
    }

    private class CCTVController : EntityController
    {
        private int _nextId;
        public CCTVController(ProfileController profileController, EntityData data);
        public override BaseAdapter CreateAdapter(BaseMonument monument);
    }

    private abstract class AdapterListenerBase
    {
        public virtual void Init();
        public virtual void OnServerInitialized();
        public abstract bool InterestedInAdapter(BaseAdapter adapter);
        public abstract void OnAdapterSpawned(BaseAdapter adapter);
        public abstract void OnAdapterKilled(BaseAdapter adapter);
        protected static bool IsEntityAdapter(BaseAdapter adapter);
    }

    private abstract class DynamicHookListener : AdapterListenerBase
    {
        private MonumentAddons _plugin;
        protected string[] _dynamicHookNames;
        private HashSet<BaseAdapter> _adapters;
        protected DynamicHookListener(MonumentAddons plugin);
        public override void Init();
        public override void OnAdapterSpawned(BaseAdapter adapter);
        public override void OnAdapterKilled(BaseAdapter adapter);
        private void SubscribeHooks();
        private void UnsubscribeHooks();
    }

    private class SignEntityListener : DynamicHookListener
    {
        public SignEntityListener(MonumentAddons plugin);
        public override bool InterestedInAdapter(BaseAdapter adapter);
    }

    private class BuildingBlockEntityListener : DynamicHookListener
    {
        public BuildingBlockEntityListener(MonumentAddons plugin);
        public override bool InterestedInAdapter(BaseAdapter adapter);
    }

    private class SprayDecalListener : DynamicHookListener
    {
        public SprayDecalListener(MonumentAddons plugin);
        public override bool InterestedInAdapter(BaseAdapter adapter);
    }

    private class AdapterListenerManager
    {
        private AdapterListenerBase[] _listeners;
        public AdapterListenerManager(MonumentAddons plugin);
        public void Init();
        public void OnServerInitialized();
        public void OnAdapterSpawned(BaseAdapter entityAdapter);
        public void OnAdapterKilled(BaseAdapter entityAdapter);
    }

    private class PuzzleResetHandler : FacepunchBehaviour
    {
        public static PuzzleResetHandler Create(EntityAdapter adapter, PuzzleReset puzzleReset);
        private EntityAdapter _adapter;
        private void OnPuzzleReset();
        public void Destroy();
    }

    private class SpawnedVehicleComponent : FacepunchBehaviour
    {
        public static void AddToVehicle(MonumentAddons plugin, GameObject gameObject);
        private const float MaxDistanceSquared;
        private MonumentAddons _plugin;
        private Vector3 _originalPosition;
        private Transform _transform;
        public void Awake();
        public void CheckPositionTracked();
        private void CheckPosition();
    }

    private class CustomAddonSpawnPointInstance : SpawnPointInstance
    {
        public static CustomAddonSpawnPointInstance AddToComponent(AddonComponentTracker componentTracker, Component hostComponent, CustomAddonDefinition customAddonDefinition);
        public CustomAddonDefinition CustomAddonDefinition { get; set; }
        private Component _hostComponent;
        private AddonComponentTracker _componentTracker;
        public void Kill();
        public new void OnDestroy();
    }

    private class CustomSpawnPoint : BaseSpawnPoint, IAddonComponent
    {
        private const int TrainCarLayerMask;
        private static readonly Dictionary<string, int> CustomBoundsCheckMask;
        public static CustomSpawnPoint AddToGameObject(AddonComponentTracker componentTracker, GameObject gameObject, SpawnPointAdapter adapter, SpawnPointData spawnPointData);
        public TransformAdapter Adapter { get; set; }
        private AddonComponentTracker _componentTracker;
        private SpawnPointAdapter _spawnPointAdapter;
        private SpawnPointData _spawnPointData;
        private Transform _transform;
        private BaseEntity _parentEntity;
        private List<SpawnPointInstance> _instances;
        public void PreUnload();
        private void Awake();
        public override void GetLocation(Vector3 position, Quaternion rotation);
        public override void ObjectSpawned(SpawnPointInstance instance);
        public override void ObjectRetired(SpawnPointInstance instance);
        public bool IsAvailableTo(CustomSpawnGroup.SpawnEntry spawnEntry);
        public override bool IsAvailableTo(GameObject prefab);
        public override bool HasPlayersIntersecting();
        public void KillSpawnedInstances(WeightedPrefabData weightedPrefabData);
        private bool IsVehicle(BaseEntity entity);
        private void DisableVehicleDecay(BaseEntity vehicle);
        public void MoveSpawnedInstances();
        private void OnDestroy();
    }

    private class CustomSpawnGroup : SpawnGroup
    {
        public new class SpawnEntry
        {
            public readonly CustomAddonDefinition CustomAddonDefinition;
            public readonly GameObjectRef Prefab;
            public int Weight;
            public bool IsValid { get; set; }
            public SpawnEntry(CustomAddonDefinition customAddonDefinition, int weight);
            public SpawnEntry(GameObjectRef prefab, int weight);
        }

        private static AIInformationZone FindVirtualInfoZone(Vector3 position);
        public SpawnGroupAdapter SpawnGroupAdapter { get; set; }
        public List<SpawnEntry> SpawnEntries { get; set; }
        private AIInformationZone _cachedInfoZone;
        private bool _didLookForInfoZone;
        private SpawnGroupData SpawnGroupData { get; set; }
        public void Init(SpawnGroupAdapter spawnGroupAdapter);
        public void UpdateSpawnClock();
        public void HandleObjectRetired();
        public void OnPuzzleReset();
        protected override void Spawn(int numToSpawn);
        protected override void PostSpawnProcess(BaseEntity entity, BaseSpawnPoint spawnPoint);
        private bool HasSpawned(SpawnEntry spawnEntry);
        private BaseSpawnPoint GetRandomSpawnPoint(SpawnEntry spawnEntry, Vector3 position, Quaternion rotation);
        private float DetermineSpawnEntryWeight(SpawnEntry spawnEntry);
        private SpawnEntry GetRandomSpawnEntry();
        private AIInformationZone GetVirtualInfoZone();
        private void ResetSpawnClock();
        private new void OnDestroy();
    }

    private class SpawnPointAdapter : TransformAdapter
    {
        public SpawnPointData SpawnPointData { get; set; }
        public SpawnGroupAdapter SpawnGroupAdapter { get; set; }
        public CustomSpawnPoint SpawnPoint { get; set; }
        public override Component Component { get; set; }
        public override Transform Transform { get; set; }
        public override Vector3 Position { get; set; }
        public override Quaternion Rotation { get; set; }
        public override bool IsValid { get; set; }
        private Transform _transform;
        public SpawnPointAdapter(SpawnPointData spawnPointData, SpawnGroupAdapter spawnGroupAdapter, BaseController controller, BaseMonument monument);
        public override void Spawn();
        public override void Kill();
        public override void OnComponentDestroyed(Component component);
        public override void PreUnload();
        public void KillSpawnedInstances(WeightedPrefabData weightedPrefabData);
        public void UpdatePosition();
        public override bool TryRecordUpdates(Transform moveTransform, Transform rotateTransform);
    }

    private class SpawnGroupAdapter : BaseAdapter
    {
        public SpawnGroupData SpawnGroupData { get; set; }
        public List<SpawnPointAdapter> SpawnPointAdapters { get; set; }
        public CustomSpawnGroup SpawnGroup { get; set; }
        public PuzzleReset AssociatedPuzzleReset { get; set; }
        public override bool IsValid { get; set; }
        public SpawnGroupAdapter(SpawnGroupData spawnGroupData, BaseController controller, BaseMonument monument);
        public override void Spawn();
        public override void Kill();
        public override void OnComponentDestroyed(Component component);
        public override void PreUnload();
        public void OnSpawnPointAdapterKilled(SpawnPointAdapter spawnPointAdapter);
        public void OnSpawnGroupKilled();
        public void UpdateCustomAddons(CustomAddonDefinition customAddonDefinition);
        private Vector3 GetMidpoint();
        private PuzzleReset FindClosestVanillaPuzzleReset(float maxDistance);
        private void RegisterWithPuzzleReset(PuzzleReset puzzleReset);
        private void UnregisterWithPuzzleReset(PuzzleReset puzzleReset);
        private bool IsRegisteredWithPuzzleReset(PuzzleReset puzzleReset);
        private void UpdateProperties();
        private void UpdatePrefabEntries();
        private void UpdateSpawnPointReferences();
        private void UpdateSpawnPointPositions();
        private void UpdatePuzzleResetAssociation();
        public void UpdateSpawnGroup();
        public void SpawnTick();
        public void KillSpawnedInstances(WeightedPrefabData weightedPrefabData);
        public void CreateSpawnPoint(SpawnPointData spawnPointData);
        public void KillSpawnPoint(SpawnPointData spawnPointData);
        private SpawnPointAdapter FindSpawnPoint(SpawnPointData spawnPointData);
    }

    private class SpawnGroupController : BaseController, IUpdateableController
    {
        public SpawnGroupData SpawnGroupData { get; set; }
        public IEnumerable<SpawnGroupAdapter> SpawnGroupAdapters { get; set; }
        public SpawnGroupController(ProfileController profileController, SpawnGroupData spawnGroupData);
        public override BaseAdapter CreateAdapter(BaseMonument monument);
        public override Coroutine Kill(BaseData data);
        public void CreateSpawnPoint(SpawnPointData spawnPointData);
        public void KillSpawnPoint(SpawnPointData spawnPointData);
        public void UpdateSpawnGroups();
        public Coroutine StartUpdateRoutine();
        public void StartSpawnRoutine();
        public void StartKillSpawnedInstancesRoutine(WeightedPrefabData weightedPrefabData);
        public void StartRespawnRoutine();
        private IEnumerator UpdateRoutine();
        private IEnumerator SpawnTickRoutine();
        private IEnumerator KillSpawnedInstancesRoutine(WeightedPrefabData weightedPrefabData);
        private IEnumerator RespawnRoutine();
    }

    private class PasteAdapter : TransformAdapter
    {
        private const float CopyPasteMagicRotationNumber;
        private const float MaxWaitSeconds;
        private const float KillBatchSize;
        public PasteData PasteData { get; set; }
        public OBB Bounds { get; set; }
        public override Component Component { get; set; }
        public override Transform Transform { get; set; }
        public override Vector3 Position { get; set; }
        public override Quaternion Rotation { get; set; }
        public override bool IsValid { get; set; }
        public override bool IsAtIntendedPosition { get; set; }
        private Transform _transform;
        private Vector3 _spawnedPosition;
        private Quaternion _spawnedRotation;
        private Bounds _bounds;
        private bool _isWorking;
        private Action _cancelPaste;
        private List<BaseEntity> _pastedEntities;
        public PasteAdapter(PasteData pasteData, BaseController controller, BaseMonument monument);
        public override void Spawn();
        public override void Kill();
        public override void OnComponentDestroyed(Component component);
        public void HandleChanges();
        private void SpawnPaste(Vector3 position, Quaternion rotation);
        private IEnumerator SpawnPasteRoutine(Vector3 position, Quaternion rotation);
        private IEnumerator RespawnPasteRoutine(Vector3 position, Quaternion rotation);
        private Coroutine KillPaste();
        private IEnumerator KillRoutine();
        private void OnEntityPasted(BaseEntity entity);
        private void OnPasteComplete();
        private Bounds GetBounds();
    }

    private class PasteController : BaseController, IUpdateableController
    {
        public PasteData PasteData { get; set; }
        public PasteController(ProfileController profileController, PasteData pasteData);
        public override BaseAdapter CreateAdapter(BaseMonument monument);
        public override IEnumerator SpawnAtMonumentsRoutine(IEnumerable<BaseMonument> monumentList);
        public Coroutine StartUpdateRoutine();
        private IEnumerator UpdateRoutine();
    }

    private class CustomAddonDefinition
    {
        public static CustomAddonDefinition FromDictionary(string addonName, Plugin plugin, Dictionary<string, object> addonSpec);
        public string AddonName;
        public Plugin OwnerPlugin;
        private CustomInitializeCallback Initialize;
        private CustomInitializeCallbackV2 InitializeV2;
        private CustomEditCallback Edit;
        private CustomSpawnCallback Spawn;
        private CustomSpawnCallbackV2 SpawnV2;
        public CustomCheckSpaceCallback CheckSpace;
        public CustomKillCallback Kill;
        public CustomUnloadCallback Unload;
        private CustomUpdateCallback Update;
        private CustomUpdateCallbackV2 UpdateV2;
        private CustomDisplayCallback Display;
        private CustomDisplayCallbackV2 DisplayV2;
        public bool IsValid;
        public List<CustomAddonAdapter> AdapterUsers;
        public List<CustomAddonSpawnPointInstance> SpawnPointInstances;
        public bool SupportsEditing { get; set; }
        public Dictionary<string, object> ToApiResult(ProfileStore profileStore);
        public bool Validate();
        public bool TryInitialize(BasePlayer player, string[] args, object data);
        public bool TryEdit(BasePlayer player, string[] args, Component component, JObject data, object newData);
        public Component DoSpawn(Guid guid, Component monument, Vector3 position, Quaternion rotation, JObject jObject);
        public Component DoUpdate(Component component, JObject data);
        public void SetData(ProfileStore profileStore, CustomAddonController controller, object data);
        public void SetData(ProfileStore profileStore, Component component, object data);
        public void DoDisplay(Component component, JObject data, BasePlayer player, StringBuilder sb, float duration);
    }

    private class CustomAddonManager
    {
        private MonumentAddons _plugin;
        private Dictionary<string, CustomAddonDefinition> _customAddonsByName;
        private Dictionary<string, List<CustomAddonDefinition>> _customAddonsByPlugin;
        public IEnumerable<CustomAddonDefinition> GetAllAddons();
        public CustomAddonManager(MonumentAddons plugin);
        public bool IsRegistered(string addonName, Plugin otherPlugin);
        public void RegisterAddon(CustomAddonDefinition addonDefinition);
        public void UnregisterAllForPlugin(Plugin plugin);
        public CustomAddonDefinition GetAddon(string addonName);
        private List<CustomAddonDefinition> GetAddonsForPlugin(Plugin plugin);
        private IEnumerator DestroyControllersRoutine(ICollection<CustomAddonController> controllerList);
    }

    private class CustomAddonAdapter : TransformAdapter
    {
        public CustomAddonData CustomAddonData { get; set; }
        public CustomAddonDefinition AddonDefinition { get; set; }
        public override Component Component { get; set; }
        public override Transform Transform { get; set; }
        public override Vector3 Position { get; set; }
        public override Quaternion Rotation { get; set; }
        public override bool IsValid { get; set; }
        private Component _component;
        private Transform _transform;
        private bool _wasKilled;
        public CustomAddonAdapter(CustomAddonData customAddonData, BaseController controller, BaseMonument monument, CustomAddonDefinition addonDefinition);
        public override void Spawn();
        public override void PreUnload();
        public override void Kill();
        public override void OnComponentDestroyed(Component component);
        public void SetupComponent(Component component);
        public void HandleChanges();
        private void UpdateViaOwnerPlugin();
        private void UpdatePosition();
    }

    private class CustomAddonController : BaseController, IUpdateableController
    {
        public CustomAddonData CustomAddonData { get; set; }
        private CustomAddonDefinition _addonDefinition;
        public CustomAddonController(ProfileController profileController, CustomAddonData customAddonData, CustomAddonDefinition addonDefinition);
        public override BaseAdapter CreateAdapter(BaseMonument monument);
        public Coroutine StartUpdateRoutine();
        private IEnumerator UpdateRoutine();
    }

    private class EntityControllerFactory
    {
        public virtual bool AppliesToEntity(BaseEntity entity);
        public virtual EntityController CreateController(ProfileController controller, EntityData entityData);
    }

    private class SignControllerFactory : EntityControllerFactory
    {
        public override bool AppliesToEntity(BaseEntity entity);
        public override EntityController CreateController(ProfileController controller, EntityData entityData);
    }

    private class CCTVControllerFactory : EntityControllerFactory
    {
        public override bool AppliesToEntity(BaseEntity entity);
        public override EntityController CreateController(ProfileController controller, EntityData entityData);
    }

    private class ControllerFactory
    {
        private MonumentAddons _plugin;
        public ControllerFactory(MonumentAddons plugin);
        private EntityControllerFactory[] _entityFactories;
        public BaseController CreateController(ProfileController profileController, BaseData data);
        private EntityControllerFactory ResolveEntityFactory(EntityData entityData);
    }

    private class IOManager
    {
        private const int FreePowerAmount;
        private static bool HasInput(IOEntity ioEntity, IOType ioType);
        private static bool HasConnectedInput(IOEntity ioEntity, int inputSlot);
        private readonly int[] _defaultInputSlots;
        private readonly Type[] _dontPowerPrefabsOfType;
        private readonly Dictionary<string, int[]> _inputSlotsByPrefabName;
        private readonly Dictionary<uint, int[]> _inputSlotsByPrefabId;
        private List<uint> _dontPowerPrefabIds;
        public void OnServerInitialized();
        public bool MaybeProvidePower(IOEntity ioEntity);
        private int[] DeterminePowerInputSlots(IOEntity ioEntity);
    }

    private class AdapterDisplayManager
    {
        private MonumentAddons _plugin;
        private UniqueNameRegistry _uniqueNameRegistry;
        private Configuration _config { get; set; }
        public const int HeaderSize;
        public static readonly string Divider;
        public static readonly Vector3 ArrowVerticalOffeset;
        private const float DisplayIntervalDurationFast;
        private const float DisplayIntervalDuration;
        private class PlayerInfo
        {
            public Timer Timer;
            public ProfileController ProfileController;
            public TransformAdapter MovingAdapter;
            public CustomMonument MovingCustomMonument;
            public RealTimeSince RealTimeSinceShown;
            public float DisplayDurationSlow { get; set; }
            public float DisplayDurationFast { get; set; }
            public float GetDisplayDuration(BaseAdapter adapter);
            public float GetDisplayDuration(CustomMonument monument);
        }

        private float DefaultDisplayDuration { get; set; }
        private float DisplayDistanceSquared { get; set; }
        private float DisplayDistanceAbbreviatedSquared { get; set; }
        private StringBuilder _sb;
        private Dictionary<ulong, PlayerInfo> _playerInfo;
        public AdapterDisplayManager(MonumentAddons plugin, UniqueNameRegistry uniqueNameRegistry);
        public void SetPlayerProfile(BasePlayer player, ProfileController profileController);
        public void SetPlayerMovingAdapter(BasePlayer player, TransformAdapter adapter);
        public void ShowAllRepeatedly(BasePlayer player, float? duration, bool immediate);
        private Color DetermineColor(BaseAdapter adapter, PlayerInfo playerInfo, ProfileController profileController);
        private void AddCommonInfo(BasePlayer player, ProfileController profileController, BaseController controller, BaseAdapter adapter);
        private void ShowPuzzleInfo(BasePlayer player, EntityAdapter entityAdapter, PuzzleReset puzzleReset, Vector3 playerPosition, PlayerInfo playerInfo);
        private Ddraw CreateDrawer(BasePlayer player, BaseAdapter adapter, PlayerInfo playerInfo);
        private void ShowEntityInfo(Ddraw drawer, BasePlayer player, EntityAdapter adapter, Vector3 playerPosition, PlayerInfo playerInfo);
        private void ShowPrefabInfo(Ddraw drawer, BasePlayer player, PrefabAdapter adapter, PlayerInfo playerInfo);
        private void ShowSpawnPointInfo(BasePlayer player, SpawnPointAdapter adapter, SpawnGroupAdapter spawnGroupAdapter, PlayerInfo playerInfo, bool showGroupInfo);
        private void ShowPasteInfo(Ddraw drawer, BasePlayer player, PasteAdapter adapter);
        private void ShowCustomAddonInfo(Ddraw drawer, BasePlayer player, CustomAddonAdapter adapter);
        private SpawnPointAdapter FindClosestSpawnPointAdapter(SpawnGroupAdapter spawnGroupAdapter, Vector3 origin);
        private SpawnPointAdapter FindClosestSpawnPointAdapterToRay(SpawnGroupAdapter spawnGroupAdapter, Ray ray);
        private Vector3 GetClosestAdapterPosition(BaseAdapter adapter, Ray ray, BaseAdapter closestAdapter);
        private static bool IsWithinDistanceSquared(Vector3 position1, Vector3 position2, float distanceSquared);
        private static bool IsWithinDistanceSquared(TransformAdapter adapter, Vector3 position, float distanceSquared);
        private static void DrawAbbreviation(Ddraw drawer, TransformAdapter adapter);
        private void DisplayMonument(BasePlayer player, PlayerInfo playerInfo, CustomMonument monument);
        private void ShowNearbyCustomMonuments(BasePlayer player, Vector3 playerPosition, PlayerInfo playerInfo);
        private void DisplayAdapter(BasePlayer player, Vector3 playerPosition, PlayerInfo playerInfo, BaseAdapter adapter, BaseAdapter closestAdapter, float distanceSquared, int remainingToShow);
        private void ShowNearbyAdapters(BasePlayer player, Vector3 playerPosition, PlayerInfo playerInfo);
        private PlayerInfo GetOrCreatePlayerInfo(BasePlayer player);
    }

    private class ProfileController
    {
        public MonumentAddons Plugin { get; set; }
        public Profile Profile { get; set; }
        public ProfileStatus ProfileStatus { get; set; }
        public WaitUntil WaitUntilLoaded;
        public WaitUntil WaitUntilUnloaded;
        private Configuration _config { get; set; }
        private StoredData _pluginData { get; set; }
        private ProfileManager _profileManager { get; set; }
        private ProfileStateData _profileStateData { get; set; }
        private CoroutineManager _coroutineManager;
        private Dictionary<BaseData, BaseController> _controllersByData;
        private Queue<SpawnQueueItem> _spawnQueue;
        public bool IsEnabled { get; set; }
        public ProfileController(MonumentAddons plugin, Profile profile, bool startLoaded);
        public void OnControllerKilled(BaseController controller);
        public Coroutine StartCoroutine(IEnumerator enumerator);
        public Coroutine StartCallbackRoutine(Coroutine coroutine, Action callback);
        public IEnumerable<T> GetControllers();
        public IEnumerable<T> GetAdapters();
        public void Load(ProfileCounts profileCounts);
        public void PreUnload();
        public void DetachSavedEntities();
        public void Unload(IEnumerator cleanupRoutine);
        public void Reload(Profile newProfileData);
        public IEnumerator PartialLoadForLateMonument(ICollection<BaseData> dataList, BaseMonument monument);
        public void SpawnNewData(BaseData data, ICollection<BaseMonument> monumentList);
        public void Rename(string newName);
        public void Enable(Profile newProfileData);
        public void Disable();
        public void Clear();
        public BaseController FindControllerById(Guid guid);
        public BaseAdapter FindAdapter(Guid guid, BaseMonument monument);
        public T FindEntity(Guid guid, BaseMonument monument);
        public void SetupIO();
        private void Interrupt();
        private BaseController GetController(BaseData data);
        private BaseController EnsureController(BaseData data);
        private void Enqueue(SpawnQueueItem queueItem);
        private void EnqueueAll(ProfileCounts profileCounts);
        private void SetProfileStatus(ProfileStatus newStatus, bool broadcast);
        private IEnumerator ProcessSpawnQueue();
        private IEnumerator UnloadRoutine(IEnumerator cleanupRoutine);
        private IEnumerator ReloadRoutine(Profile newProfileData);
        private IEnumerator ClearRoutine();
        private IEnumerator CleanEntitiesRoutine(ProfileState profileState, List<BaseEntity> entitiesToKill);
        private void CleanOrphanedEntities();
    }

    private class ProfileCounts
    {
        public int EntityCount;
        public int PrefabCount;
        public int SpawnPointCount;
        public int PasteCount;
    }

    private class ProfileInfo
    {
        public static List<ProfileInfo> GetList(StoredData pluginData, ProfileManager profileManager);
        public string Name;
        public bool Enabled;
        public Profile Profile;
    }

    private class ProfileManager
    {
        private readonly MonumentAddons _plugin;
        private OriginalProfileStore _originalProfileStore;
        private readonly ProfileStore _profileStore;
        private List<ProfileController> _profileControllers;
        private Configuration _config { get; set; }
        private StoredData _pluginData { get; set; }
        public bool HasAnyEnabledDynamicMonuments { get; set; }
        public ProfileManager(MonumentAddons plugin, OriginalProfileStore originalProfileStore, ProfileStore profileStore);
        public void BroadcastProfileStateChanged(ProfileController profileController, ProfileStatus status, ProfileStatus previousStatus);
        public bool HasDynamicMonument(BaseEntity entity);
        public IEnumerator LoadAllProfilesRoutine();
        public void UnloadAllProfiles();
        public IEnumerator PartialLoadForLateMonumentRoutine(BaseMonument monument);
        public ProfileController GetCachedProfileController(string profileName);
        public ProfileController GetProfileController(string profileName);
        public ProfileController GetPlayerProfileController(string userId);
        public ProfileController GetPlayerProfileControllerOrDefault(string userId);
        public bool ProfileExists(string profileName);
        public ProfileController CreateProfile(string profileName, string authorName);
        public void DisableProfile(ProfileController profileController);
        public void DeleteProfile(ProfileController profileController);
        public IEnumerable<ProfileController> GetEnabledProfileControllers();
        public IEnumerable<T> GetEnabledControllers();
        public IEnumerable<T> GetEnabledAdapters();
        public IEnumerable<T> GetEnabledAdaptersForMonument(BaseMonument monument);
        private IEnumerator UnloadAllProfilesRoutine();
    }

    private abstract class BaseData
    {
        [JsonProperty("Id", Order = -10)]
        public Guid Id;
    }

    private abstract class BaseTransformData : BaseData
    {
        [JsonProperty("SnapToTerrain", Order = -4, DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool SnapToTerrain;
        [JsonProperty("Position", Order = -3)]
        public Vector3 Position;
        [JsonProperty("RotationAngle", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public float DeprecatedRotationAngle { get; set; }
        [JsonProperty("RotationAngles", Order = -2, DefaultValueHandling = DefaultValueHandling.Ignore)]
        public Vector3 RotationAngles;
        [JsonProperty("OnTerrain")]
        public bool DepredcatedOnTerrain { get; set; }
    }

    private class PrefabData : BaseTransformData
    {
        [JsonProperty("PrefabName", Order = -5)]
        public string PrefabName;
    }

    private class BuildingBlockInfo
    {
        [JsonProperty("Grade")]
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(BuildingGrade.Enum.None)]
        public BuildingGrade.Enum Grade;
    }

    private class CCTVInfo
    {
        [JsonProperty("RCIdentifier", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string RCIdentifier;
        [JsonProperty("Pitch", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public float Pitch;
        [JsonProperty("Yaw", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public float Yaw;
    }

    private class SignArtistImage
    {
        [JsonProperty("Url")]
        public string Url;
        [JsonProperty("Raw", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool Raw;
    }

    private class IOConnectionData
    {
        [JsonProperty("ConnectedToId")]
        public Guid ConnectedToId;
        [JsonProperty("Slot")]
        public int Slot;
        [JsonProperty("ConnectedToSlot")]
        public int ConnectedToSlot;
        [JsonProperty("ShowWire")]
        public bool ShowWire;
        [JsonProperty("Color", DefaultValueHandling = DefaultValueHandling.Ignore)]
        [JsonConverter(typeof(StringEnumConverter))]
        public WireColour Color;
        [JsonProperty("Points")]
        public Vector3[] Points;
    }

    private class IOEntityData
    {
        [JsonProperty("Outputs")]
        public List<IOConnectionData> Outputs;
        public IOConnectionData FindConnection(int slot);
    }

    private class PuzzleData
    {
        [JsonProperty("PlayersBlockReset")]
        public bool PlayersBlockReset;
        [JsonProperty("PlayerDetectionRadius")]
        public float PlayerDetectionRadius;
        [JsonProperty("SecondsBetweenResets")]
        public float SecondsBetweenResets;
        [JsonProperty("SpawnGroupIds", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public List<Guid> SpawnGroupIds;
        public bool ShouldSerializeSpawnGroupIds();
        public bool HasSpawnGroupId(Guid spawnGroupId);
        public void AddSpawnGroupId(Guid spawnGroupId);
        public void RemoveSpawnGroupId(Guid spawnGroupId);
    }

    private class HeadData
    {
        private FieldInfo CurrentTrophyDataField;
        public static HeadData FromHeadEntity(HeadEntity headEntity);
        public class BasicItemData
        {
            public readonly int ItemId;
            public BasicItemData(int itemId);
        }

        [JsonProperty("EntitySource")]
        public uint EntitySource;
        [JsonProperty("HorseBreed", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public int HorseBreed;
        [JsonProperty("PlayerId", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public ulong PlayerId;
        [JsonProperty("PlayerName", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string PlayerName;
        [JsonProperty("Clothing", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public BasicItemData[] Clothing;
        public void ApplyToHuntingTrophy(HuntingTrophy huntingTrophy);
    }

    private class EntityData : BaseTransformData
    {
        [JsonProperty("PrefabName", Order = -5)]
        public string PrefabName;
        [JsonProperty("Skin", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public ulong Skin;
        [JsonProperty("EnabledFlags", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public BaseEntity.Flags EnabledFlags;
        [JsonProperty("DisabledFlags", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public BaseEntity.Flags DisabledFlags;
        [JsonProperty("Puzzle", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public PuzzleData Puzzle;
        [JsonProperty("Scale", DefaultValueHandling = DefaultValueHandling.Ignore)]
        [DefaultValue(1f)]
        public float Scale;
        [JsonProperty("CardReaderLevel", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public ushort CardReaderLevel;
        [JsonProperty("BuildingBlock", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public BuildingBlockInfo BuildingBlock;
        [JsonProperty("CCTV", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public CCTVInfo CCTV;
        [JsonProperty("SignArtistImages", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public SignArtistImage[] SignArtistImages;
        [JsonProperty("VendingProfile", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public object VendingProfile;
        [JsonProperty("IOEntity", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public IOEntityData IOEntityData;
        [JsonProperty("SkullName", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string SkullName;
        [JsonProperty("HeadData", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public HeadData HeadData;
        public void SetFlag(BaseEntity.Flags flag, bool? value);
        public bool? HasFlag(BaseEntity.Flags flag);
        public void RemoveIOConnection(int slot);
        public void AddIOConnection(IOConnectionData connectionData);
        public PuzzleData EnsurePuzzleData(PuzzleReset puzzleReset);
    }

    private class SpawnPointData : BaseTransformData
    {
        [JsonProperty("Exclusive", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool Exclusive;
        [JsonProperty("SnapToGround", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool SnapToGround;
        [JsonProperty("DropToGround")]
        public bool DeprecatedDropToGround { get; set; }
        [JsonProperty("CheckSpace", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool CheckSpace;
        [JsonProperty("RandomRotation", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool RandomRotation;
        [JsonProperty("RandomRadius", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public float RandomRadius;
        [JsonProperty("PlayerDetectionRadius", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public float PlayerDetectionRadius;
    }

    private class WeightedPrefabData
    {
        [JsonProperty("PrefabName", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string PrefabName;
        [JsonProperty("CustomAddonName", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string CustomAddonName;
        [JsonProperty("Weight")]
        public int Weight;
    }

    private class SpawnGroupData : BaseData
    {
        [JsonProperty("Name")]
        public string Name;
        [JsonProperty("Color", DefaultValueHandling = DefaultValueHandling.Ignore)]
        [JsonConverter(typeof(HtmlColorConverter))]
        public Color? Color;
        [JsonProperty("MaxPopulation")]
        public int MaxPopulation;
        [JsonProperty("SpawnPerTickMin")]
        public int SpawnPerTickMin;
        [JsonProperty("SpawnPerTickMax")]
        public int SpawnPerTickMax;
        [JsonProperty("RespawnDelayMin")]
        public float RespawnDelayMin;
        [JsonProperty("RespawnDelayMax")]
        public float RespawnDelayMax;
        [JsonProperty("InitialSpawn")]
        public bool InitialSpawn;
        [JsonProperty("PreventDuplicates", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool PreventDuplicates;
        [JsonProperty("PauseScheduleWhileFull", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool PauseScheduleWhileFull;
        [JsonProperty("RespawnWhenNearestPuzzleResets", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool RespawnWhenNearestPuzzleResets;
        [JsonProperty("Prefabs")]
        public List<WeightedPrefabData> Prefabs;
        [JsonProperty("SpawnPoints")]
        public List<SpawnPointData> SpawnPoints;
        [JsonIgnore]
        public int TotalWeight { get; set; }
        public List<WeightedPrefabData> FindCustomAddonMatches(string partialName);
        public List<WeightedPrefabData> FindPrefabMatches(string partialName, UniqueNameRegistry uniqueNameRegistry);
    }

    private class PasteData : BaseTransformData
    {
        [JsonProperty("Filename")]
        public string Filename;
        [JsonProperty("Args")]
        public string[] Args;
    }

    private class CustomAddonData : BaseTransformData
    {
        [JsonProperty("AddonName")]
        public string AddonName;
        [JsonProperty("PluginData", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private object PluginData;
        public JObject GetSerializedData();
        public CustomAddonData SetData(object data);
    }

    private class ProfileSummaryEntry
    {
        public string MonumentName;
        public string AddonType;
        public string AddonName;
        public int Count;
    }

    private List<ProfileSummaryEntry> GetProfileSummary(IPlayer player, Profile profile);
    private class MonumentData
    {
        [JsonProperty("Entities")]
        public List<EntityData> Entities;
        public bool ShouldSerializeEntities();
        [JsonProperty("Prefabs")]
        public List<PrefabData> Prefabs;
        public bool ShouldSerializePrefabs();
        [JsonProperty("SpawnGroups")]
        public List<SpawnGroupData> SpawnGroups;
        public bool ShouldSerializeSpawnGroups();
        [JsonProperty("Pastes")]
        public List<PasteData> Pastes;
        public bool ShouldSerializePastes();
        [JsonProperty("CustomAddons")]
        public List<CustomAddonData> CustomAddons;
        public bool ShouldSerializeCustomAddons();
        [JsonIgnore]
        public int NumSpawnables { get; set; }
        [JsonIgnore]
        public int NumSpawnPoints { get; set; }
        public IEnumerable<BaseData> GetSpawnablesLazy();
        public ICollection<BaseData> GetSpawnables();
        public bool HasEntity(Guid guid);
        public bool HasSpawnGroup(Guid guid);
        public void AddData(BaseData data);
        public bool RemoveData(BaseData data);
        private void CleanSpawnGroupReferences(Guid id);
    }

    private static class ProfileDataMigration
    {
        private static readonly Dictionary<string, string> _monumentNameCorrections;
        private static readonly Dictionary<string, string> _prefabCorrections;
        private static string GetPrefabCorrectionIfExists(string prefabName);
        public static bool MigrateToLatest(T data);
        public static bool MigrateV0ToV1(T data);
        public static bool MigrateV1ToV2(T data);
        public static bool MigrateIncorrectPrefabs(T data);
        public static bool MigrateIncorrectMonuments(T data);
    }

    private class FileStore
    {
        protected string _directoryPath;
        public FileStore(string directoryPath);
        public virtual bool Exists(string filename);
        public virtual T Load(string filename);
        public T LoadIfExists(string filename);
        public void Save(string filename, T data);
        public void Delete(string filename);
        protected virtual string GetFilepath(string filename);
    }

    private class OriginalProfileStore : FileStore<Profile>
    {
        public const string OriginalSuffix;
        public static bool IsOriginalProfile(string profileName);
        public OriginalProfileStore();
        protected override string GetFilepath(string profileName);
        public void Save(Profile profile);
        public void MoveTo(Profile profile, string newName);
    }

    private class ProfileStore : FileStore<Profile>
    {
        public static string[] GetProfileNames();
        public ProfileStore();
        public override bool Exists(string profileName);
        public override Profile Load(string profileName);
        public bool TryLoad(string profileName, Profile profile, string errorMessage);
        public void Save(Profile profile);
        public Profile Create(string profileName, string authorName);
        public void MoveTo(Profile profile, string newName);
        public void EnsureDefaultProfile();
        private string GetCaseSensitiveFileName(string profileName);
    }

    private class Profile
    {
        [JsonProperty("Name")]
        public string Name;
        [JsonProperty("Author", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string Author;
        [JsonProperty("SchemaVersion", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public float SchemaVersion;
        [JsonProperty("Url", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string Url;
        [JsonProperty("Monuments", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public Dictionary<string, List<EntityData>> DeprecatedMonumentMap;
        [JsonProperty("MonumentData")]
        public Dictionary<string, MonumentData> MonumentDataMap;
        private HashSet<uint> _dynamicMonumentPrefabIds;
        private bool _hasDeterminedDynamicMonuments;
        [JsonIgnore]
        public bool HasAnyDynamicMonuments { get; set; }
        [OnDeserialized]
        private void OnDeserialized(StreamingContext context);
        public bool IsEmpty();
        public bool HasEntity(string monumentUniqueName, Guid guid);
        public bool HasEntity(string monumentUniqueName, EntityData entityData);
        public bool HasSpawnGroup(string monumentUniqueName, Guid guid);
        public void AddData(string monumentUniqueName, BaseData data);
        public bool RemoveData(BaseData data, string monumentUniqueName);
        public bool HasDynamicMonument(BaseEntity entity);
        private MonumentData EnsureMonumentData(string monumentUniqueName);
        private void DetermineDynamicMonuments();
    }

    private static class DataFileUtils
    {
        public static bool Exists(string filepath);
        public static T Load(string filepath);
        public static T LoadIfExists(string filepath);
        public static T LoadOrNew(string filepath);
        public static void Save(string filepath, T data);
    }

    private class BaseDataFile
    {
        private string _filepath;
        public BaseDataFile(string filepath);
        public void Save();
    }

    private class MonumentState : IDeepCollection
    {
        [JsonProperty("Entities")]
        public Dictionary<Guid, ulong> Entities;
        public bool HasItems();
        public bool HasEntity(Guid guid, NetworkableId entityId);
        public BaseEntity FindEntity(Guid guid);
        public void AddEntity(Guid guid, NetworkableId entityId);
        public bool RemoveEntity(Guid guid);
        public IEnumerable<ValueTuple<Guid, BaseEntity>> FindValidEntities();
        public int CleanStaleEntityRecords();
    }

    private class MonumentStateMapConverter : DictionaryKeyConverter<Vector3, MonumentState>
    {
        public override string KeyToString(Vector3 v);
        public override Vector3 KeyFromString(string key);
    }

    private class Vector3EqualityComparer : IEqualityComparer<Vector3>
    {
        public bool Equals(Vector3 a, Vector3 b);
        public int GetHashCode(Vector3 vector);
    }

    private class MonumentStateMap : IDeepCollection
    {
        [JsonProperty("ByLocation")]
        [JsonConverter(typeof(MonumentStateMapConverter))]
        private Dictionary<Vector3, MonumentState> ByLocation;
        public bool ShouldSerializeByLocation();
        [JsonProperty("ByEntity")]
        private Dictionary<ulong, MonumentState> ByEntity;
        public bool ShouldSerializeByEntity();
        public bool HasItems();
        public IEnumerable<ValueTuple<Guid, BaseEntity>> FindValidEntities();
        public int CleanStaleEntityRecords();
        public MonumentState GetMonumentState(BaseMonument monument);
        public MonumentState GetOrCreateMonumentState(BaseMonument monument);
    }

    private class ProfileState : Dictionary<string, MonumentStateMap>, IDeepCollection
    {
        [OnDeserialized]
        private void OnDeserialized(StreamingContext context);
        public bool HasItems();
        public int CleanStaleEntityRecords();
        public IEnumerable<MonumentEntityEntry> FindValidEntities();
    }

    private class ProfileStateMap : Dictionary<string, ProfileState>, IDeepCollection
    {
        public ProfileStateMap();
        public bool HasItems();
    }

    private class ProfileStateData : BaseDataFile
    {
        private static string Filepath { get; set; }
        public static ProfileStateData Load(StoredData pluginData);
        private StoredData _pluginData;
        public ProfileStateData();
        [JsonProperty("ProfileState", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private ProfileStateMap ProfileStateMap;
        public bool ShouldSerializeProfileStateMap();
        public ProfileState GetProfileState(string profileName);
        public bool HasEntity(string profileName, BaseMonument monument, Guid guid, NetworkableId entityId);
        public BaseEntity FindEntity(string profileName, BaseMonument monument, Guid guid);
        public void AddEntity(string profileName, BaseMonument monument, Guid guid, NetworkableId entityId);
        public bool RemoveEntity(string profileName, BaseMonument monument, Guid guid);
        public List<BaseEntity> FindAndRemoveValidEntities(string profileName);
        public List<BaseEntity> CleanDisabledProfileState();
        public void Reset();
    }

    private static class StoredDataMigration
    {
        private static readonly Dictionary<string, string> MigrateMonumentNames;
        public static bool MigrateToLatest(ProfileStore profileStore, StoredData data);
        public static bool MigrateV0ToV1(StoredData data);
        public static bool MigrateV1ToV2(ProfileStore profileStore, StoredData data);
    }

    private class StoredData
    {
        public static StoredData Load(ProfileStore profileStore);
        [JsonProperty("DataFileVersion", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public float DataFileVersion;
        [JsonProperty("EnabledProfiles")]
        public HashSet<string> EnabledProfiles;
        [JsonProperty("SelectedProfiles")]
        public Dictionary<string, string> SelectedProfiles;
        [JsonProperty("Monuments", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public Dictionary<string, List<EntityData>> DeprecatedMonumentMap;
        public void Save();
        public bool IsProfileEnabled(string profileName);
        public void SetProfileEnabled(string profileName);
        public void SetProfileDisabled(string profileName);
        public void RenameProfileReferences(string oldName, string newName);
        public string GetSelectedProfileName(string userId);
        public void SetProfileSelected(string userId, string profileName);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class DebugDisplaySettings
    {
        [JsonProperty("Default display duration (seconds)")]
        public float DefaultDisplayDuration;
        [JsonProperty("Display distance")]
        public float DisplayDistance;
        [JsonProperty("Display distance abbreviated")]
        public float DisplayDistanceAbbreviated;
        [JsonProperty("Max addons to show unabbreviated")]
        public int MaxAddonsToShowUnabbreviated;
        [JsonProperty("Entity color")]
        [JsonConverter(typeof(HtmlColorConverter))]
        public Color EntityColor;
        [JsonProperty("Spawn point color")]
        [JsonConverter(typeof(HtmlColorConverter))]
        public Color SpawnPointColor;
        [JsonProperty("Paste color")]
        [JsonConverter(typeof(HtmlColorConverter))]
        public Color PasteColor;
        [JsonProperty("Custom addon color")]
        [JsonConverter(typeof(HtmlColorConverter))]
        public Color CustomAddonColor;
        [JsonProperty("Custom monument color")]
        [JsonConverter(typeof(HtmlColorConverter))]
        public Color CustomMonumentColor;
        [JsonProperty("Inactive profile color")]
        [JsonConverter(typeof(HtmlColorConverter))]
        public Color InactiveProfileColor;
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class EntitySaveSettings
    {
        [JsonProperty("Enable saving for storage entities")]
        public bool EnabledForStorageEntities;
        [JsonProperty("Enable saving for non-storage entities")]
        public bool EnabledForNonStorageEntities;
        [JsonProperty("Override saving enabled by prefab")]
        private Dictionary<string, bool> OverrideEnabledByPrefab;
        private Dictionary<uint, bool> _overrideEnabledByPrefabId;
        public void Init();
        public bool ShouldEnableSaving(BaseEntity entity);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class DynamicMonumentSettings
    {
        [JsonProperty("Entity prefabs to consider as monuments")]
        public string[] DynamicMonumentPrefabs;
        [JsonIgnore]
        private uint[] _dynamicMonumentPrefabIds;
        public void Init();
        public bool IsConfiguredAsDynamicMonument(BaseEntity entity);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class SpawnGroupDefaults
    {
        [JsonProperty(nameof(SpawnGroupOption.MaxPopulation))]
        private int MaxPopulation;
        [JsonProperty(nameof(SpawnGroupOption.SpawnPerTickMin))]
        private int SpawnPerTickMin;
        [JsonProperty(nameof(SpawnGroupOption.SpawnPerTickMax))]
        private int SpawnPerTickMax;
        [JsonProperty(nameof(SpawnGroupOption.RespawnDelayMin))]
        private float RespawnDelayMin;
        [JsonProperty(nameof(SpawnGroupOption.RespawnDelayMax))]
        private float RespawnDelayMax;
        [JsonProperty(nameof(SpawnGroupOption.InitialSpawn))]
        private bool InitialSpawn;
        [JsonProperty(nameof(SpawnGroupOption.PreventDuplicates))]
        private bool PreventDuplicates;
        [JsonProperty(nameof(SpawnGroupOption.PauseScheduleWhileFull))]
        private bool PauseScheduleWhileFull;
        [JsonProperty(nameof(SpawnGroupOption.RespawnWhenNearestPuzzleResets))]
        private bool RespawnWhenNearestPuzzleResets;
        public SpawnGroupData ApplyTo(SpawnGroupData spawnGroupData);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class SpawnPointDefaults
    {
        [JsonProperty(nameof(SpawnPointOption.Exclusive))]
        private bool Exclusive;
        [JsonProperty(nameof(SpawnPointOption.SnapToGround))]
        private bool SnapToGround;
        [JsonProperty(nameof(SpawnPointOption.CheckSpace))]
        private bool CheckSpace;
        [JsonProperty(nameof(SpawnPointOption.RandomRotation))]
        private bool RandomRotation;
        [JsonProperty(nameof(SpawnPointOption.RandomRadius))]
        private float RandomRadius;
        [JsonProperty(nameof(SpawnPointOption.PlayerDetectionRadius))]
        private float PlayerDetectionRadius;
        public SpawnPointData ApplyTo(SpawnPointData spawnPointData);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class PuzzleDefaults
    {
        [JsonProperty(nameof(PuzzleOption.PlayersBlockReset))]
        public bool PlayersBlockReset;
        [JsonProperty(nameof(PuzzleOption.PlayerDetectionRadius))]
        public float PlayerDetectionRadius;
        [JsonProperty(nameof(PuzzleOption.SecondsBetweenResets))]
        public float SecondsBetweenResets;
        public PuzzleData ApplyTo(PuzzleData puzzleData);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class AddonDefaults
    {
        [JsonProperty("Spawn group defaults")]
        public SpawnGroupDefaults SpawnGroups;
        [JsonProperty("Spawn point defaults")]
        public SpawnPointDefaults SpawnPoints;
        [JsonProperty("Puzzle defaults")]
        public PuzzleDefaults Puzzles;
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class Configuration : BaseConfiguration
    {
        [JsonProperty("Debug", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool Debug;
        [JsonProperty("Debug display settings")]
        public DebugDisplaySettings DebugDisplaySettings;
        [JsonProperty("Debug display distance")]
        private float DeprecatedDebugDisplayDistance { get; set; }
        [JsonProperty("Save entities between restarts/reloads to preserve their state throughout a wipe")]
        public EntitySaveSettings EntitySaveSettings;
        [JsonProperty("Persist entities while the plugin is unloaded")]
        private bool DeprecatedEnableEntitySaving { get; set; }
        [JsonProperty("Dynamic monuments")]
        public DynamicMonumentSettings DynamicMonuments;
        [JsonProperty("Addon defaults")]
        public AddonDefaults AddonDefaults;
        [JsonProperty("Deployable overrides")]
        public Dictionary<string, string> DeployableOverrides;
        [JsonProperty("Xmas tree decorations (item shortnames)")]
        public string[] XmasTreeDecorations;
        public void Init();
    }

    private class BaseConfiguration
    {
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    private static class JsonHelper
    {
        public static object Deserialize(string json);
        private static object ToObject(JToken token);
    }

    private bool MaybeUpdateConfig(BaseConfiguration config);
    private bool MaybeUpdateConfigDict(Dictionary<string, object> currentWithDefaults, Dictionary<string, object> currentRaw);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private class LangEntry
    {
        public static List<LangEntry> AllLangEntries;
        public static readonly LangEntry0 NotApplicable;
        public static readonly LangEntry0 ErrorNoPermission;
        public static readonly LangEntry0 ErrorMonumentFinderNotLoaded;
        public static readonly LangEntry0 ErrorNoMonuments;
        public static readonly LangEntry2 ErrorNotAtMonument;
        public static readonly LangEntry0 ErrorNoSuitableAddonFound;
        public static readonly LangEntry0 ErrorNoCustomAddonFound;
        public static readonly LangEntry0 ErrorEntityNotEligible;
        public static readonly LangEntry0 ErrorNoSpawnPointFound;
        public static readonly LangEntry2 ErrorSetSyntaxGeneric;
        public static readonly LangEntry2 ErrorSetSyntax;
        public static readonly LangEntry1 ErrorSetUnknownOption;
        public static readonly LangEntry0 WarningRecommendSpawnPoint;
        public static readonly LangEntry0 SpawnErrorSyntax;
        public static readonly LangEntry0 SpawnErrorNoProfileSelected;
        public static readonly LangEntry1 SpawnErrorEntityNotFound;
        public static readonly LangEntry1 SpawnErrorEntityOrAddonNotFound;
        public static readonly LangEntry0 SpawnErrorMultipleMatches;
        public static readonly LangEntry0 ErrorNoSurface;
        public static readonly LangEntry3 SpawnSuccess;
        public static readonly LangEntry3 KillSuccess;
        public static readonly LangEntry0 SaveNothingToDo;
        public static readonly LangEntry2 SaveSuccess;
        public static readonly LangEntry0 PrefabErrorSyntax;
        public static readonly LangEntry1 PrefabErrorIsEntity;
        public static readonly LangEntry1 PrefabErrorNotFound;
        public static readonly LangEntry3 PrefabSuccess;
        public static readonly LangEntry0 UndoNotFound;
        public static readonly LangEntry3 UndoKillSuccess;
        public static readonly LangEntry0 EditSynax;
        public static readonly LangEntry1 EditErrorNoMatch;
        public static readonly LangEntry1 EditErrorNotEditable;
        public static readonly LangEntry1 EditSuccess;
        public static readonly LangEntry0 PasteNotCompatible;
        public static readonly LangEntry0 PasteSyntax;
        public static readonly LangEntry1 PasteNotFound;
        public static readonly LangEntry4 PasteSuccess;
        public static readonly LangEntry0 AddonTypeUnknown;
        public static readonly LangEntry0 AddonTypeEntity;
        public static readonly LangEntry0 AddonTypePrefab;
        public static readonly LangEntry0 AddonTypeSpawnPoint;
        public static readonly LangEntry0 AddonTypePaste;
        public static readonly LangEntry0 AddonTypeCustom;
        public static readonly LangEntry1 SpawnGroupCreateSyntax;
        public static readonly LangEntry3 SpawnGroupCreateNameInUse;
        public static readonly LangEntry1 SpawnGroupCreateSucces;
        public static readonly LangEntry3 SpawnGroupSetSuccess;
        public static readonly LangEntry1 SpawnGroupAddSyntax;
        public static readonly LangEntry3 SpawnGroupAddSuccess;
        public static readonly LangEntry1 SpawnGroupRemoveSyntax;
        public static readonly LangEntry2 SpawnGroupRemoveMultipleMatches;
        public static readonly LangEntry2 SpawnGroupRemoveNoMatch;
        public static readonly LangEntry2 SpawnGroupRemoveSuccess;
        public static readonly LangEntry1 SpawnGroupNotFound;
        public static readonly LangEntry1 SpawnGroupMultipeMatches;
        public static readonly LangEntry1 SpawnPointCreateSyntax;
        public static readonly LangEntry1 SpawnPointCreateSuccess;
        public static readonly LangEntry2 SpawnPointSetSuccess;
        public static readonly LangEntry2 SpawnPointSetAllSuccess;
        public static readonly LangEntry0 SpawnGroupHelpHeader;
        public static readonly LangEntry1 SpawnGroupHelpCreate;
        public static readonly LangEntry1 SpawnGroupHelpSet;
        public static readonly LangEntry1 SpawnGroupHelpAdd;
        public static readonly LangEntry1 SpawnGroupHelpRemove;
        public static readonly LangEntry1 SpawnGroupHelpSpawn;
        public static readonly LangEntry1 SpawnGroupHelpRespawn;
        public static readonly LangEntry0 SpawnPointHelpHeader;
        public static readonly LangEntry1 SpawnPointHelpCreate;
        public static readonly LangEntry1 SpawnPointHelpSet;
        public static readonly LangEntry0 SpawnGroupSetHelpName;
        public static readonly LangEntry0 SpawnGroupSetHelpColor;
        public static readonly LangEntry0 SpawnGroupSetHelpMaxPopulation;
        public static readonly LangEntry0 SpawnGroupSetHelpRespawnDelayMin;
        public static readonly LangEntry0 SpawnGroupSetHelpRespawnDelayMax;
        public static readonly LangEntry0 SpawnGroupSetHelpSpawnPerTickMin;
        public static readonly LangEntry0 SpawnGroupSetHelpSpawnPerTickMax;
        public static readonly LangEntry0 SpawnGroupSetHelpInitialSpawn;
        public static readonly LangEntry0 SpawnGroupSetHelpPreventDuplicates;
        public static readonly LangEntry0 SpawnGroupSetHelpPauseScheduleWhileFull;
        public static readonly LangEntry0 SpawnGroupSetHelpRespawnWhenNearestPuzzleResets;
        public static readonly LangEntry0 SpawnPointSetHelpExclusive;
        public static readonly LangEntry0 SpawnPointSetHelpSnapToGround;
        public static readonly LangEntry0 SpawnPointSetHelpCheckSpace;
        public static readonly LangEntry0 SpawnPointSetHelpRandomRotation;
        public static readonly LangEntry0 SpawnPointSetHelpRandomRadius;
        public static readonly LangEntry0 SpawnPointSetHelpPlayerDetectionRadius;
        public static readonly LangEntry1 PuzzleAddSpawnGroupSyntax;
        public static readonly LangEntry1 PuzzleAddSpawnGroupSuccess;
        public static readonly LangEntry1 PuzzleRemoveSpawnGroupSyntax;
        public static readonly LangEntry1 PuzzleRemoveSpawnGroupSuccess;
        public static readonly LangEntry0 PuzzleNotPresent;
        public static readonly LangEntry1 PuzzleNotConnected;
        public static readonly LangEntry0 PuzzleResetSuccess;
        public static readonly LangEntry2 PuzzleSetSuccess;
        public static readonly LangEntry0 PuzzleHelpHeader;
        public static readonly LangEntry1 PuzzleHelpReset;
        public static readonly LangEntry1 PuzzleHelpSet;
        public static readonly LangEntry1 PuzzleHelpAdd;
        public static readonly LangEntry1 PuzzleHelpRemove;
        public static readonly LangEntry0 PuzzleSetHelpMaxPlayersBlockReset;
        public static readonly LangEntry0 PuzzleSetHelpPlayerDetectionRadius;
        public static readonly LangEntry0 PuzzleSetHelpSecondsBetweenResets;
        public static readonly LangEntry1 ShowVanillaNoSpawnPoints;
        public static readonly LangEntry1 GenerateSuccess;
        public static readonly LangEntry1 ShowSuccess;
        public static readonly LangEntry1 ShowLabelPlugin;
        public static readonly LangEntry1 ShowLabelProfile;
        public static readonly LangEntry2 ShowLabelCustomMonument;
        public static readonly LangEntry2 ShowLabelMonument;
        public static readonly LangEntry3 ShowLabelMonumentWithTier;
        public static readonly LangEntry1 ShowLabelSkin;
        public static readonly LangEntry1 ShowLabelScale;
        public static readonly LangEntry1 ShowLabelRCIdentifier;
        public static readonly LangEntry1 ShowHeaderEntity;
        public static readonly LangEntry1 ShowHeaderPrefab;
        public static readonly LangEntry0 ShowHeaderPuzzle;
        public static readonly LangEntry1 ShowHeaderSpawnGroup;
        public static readonly LangEntry1 ShowHeaderVanillaSpawnGroup;
        public static readonly LangEntry1 ShowHeaderSpawnPoint;
        public static readonly LangEntry1 ShowHeaderVanillaSpawnPoint;
        public static readonly LangEntry1 ShowHeaderVanillaIndividualSpawnPoint;
        public static readonly LangEntry1 ShowHeaderPaste;
        public static readonly LangEntry1 ShowHeaderCustom;
        public static readonly LangEntry1 ShowLabelFlags;
        public static readonly LangEntry0 ShowLabelSpawnPointExclusive;
        public static readonly LangEntry0 ShowLabelSpawnPointRandomRotation;
        public static readonly LangEntry0 ShowLabelSpawnPointSnapToGround;
        public static readonly LangEntry0 ShowLabelSpawnPointCheckSpace;
        public static readonly LangEntry1 ShowLabelSpawnPointRandomRadius;
        public static readonly LangEntry1 ShowLabelSpawnPoints;
        public static readonly LangEntry1 ShowLabelTiers;
        public static readonly LangEntry0 ShowLabelSpawnWhenParentSpawns;
        public static readonly LangEntry0 ShowLabelSpawnOnServerStart;
        public static readonly LangEntry0 ShowLabelSpawnOnMapWipe;
        public static readonly LangEntry0 ShowLabelInitialSpawn;
        public static readonly LangEntry0 ShowLabelPreventDuplicates;
        public static readonly LangEntry0 ShowLabelPauseScheduleWhileFull;
        public static readonly LangEntry0 ShowLabelRespawnWhenNearestPuzzleResets;
        public static readonly LangEntry2 ShowLabelPopulation;
        public static readonly LangEntry2 ShowLabelRespawnPerTick;
        public static readonly LangEntry2 ShowLabelRespawnDelay;
        public static readonly LangEntry1 ShowLabelNextSpawn;
        public static readonly LangEntry0 ShowLabelNextSpawnQueued;
        public static readonly LangEntry0 ShowLabelNextSpawnPaused;
        public static readonly LangEntry0 ShowLabelEntities;
        public static readonly LangEntry3 ShowLabelEntityDetail;
        public static readonly LangEntry0 ShowLabelNoEntities;
        public static readonly LangEntry1 ShowLabelPlayerDetectionRadius;
        public static readonly LangEntry0 ShowLabelPlayerDetectedInRadius;
        public static readonly LangEntry1 ShowLabelPuzzlePlayersBlockReset;
        public static readonly LangEntry1 ShowLabelPuzzleTimeBetweenResets;
        public static readonly LangEntry1 ShowLabelPuzzleNextReset;
        public static readonly LangEntry0 ShowLabelPuzzleNextResetOverdue;
        public static readonly LangEntry1 ShowLabelPuzzleSpawnGroups;
        public static readonly LangEntry2 SkinGet;
        public static readonly LangEntry1 SkinSetSyntax;
        public static readonly LangEntry3 SkinSetSuccess;
        public static readonly LangEntry2 SkinErrorRedirect;
        public static readonly LangEntry3 FlagsGet;
        public static readonly LangEntry1 FlagsSetSyntax;
        public static readonly LangEntry1 FlagsEnableSuccess;
        public static readonly LangEntry1 FlagsDisableSuccess;
        public static readonly LangEntry1 FlagsUnsetSuccess;
        public static readonly LangEntry1 CCTVSetIdSyntax;
        public static readonly LangEntry3 CCTVSetIdSuccess;
        public static readonly LangEntry2 CCTVSetDirectionSuccess;
        public static readonly LangEntry1 SkullNameSyntax;
        public static readonly LangEntry3 SkullNameSetSuccess;
        public static readonly LangEntry0 SetHeadNoHeadItem;
        public static readonly LangEntry0 SetHeadMismatch;
        public static readonly LangEntry2 SetHeadSuccess;
        public static readonly LangEntry1 CardReaderSetLevelSyntax;
        public static readonly LangEntry1 CardReaderSetLevelSuccess;
        public static readonly LangEntry0 ProfileListEmpty;
        public static readonly LangEntry0 ProfileListHeader;
        public static readonly LangEntry2 ProfileListItemEnabled;
        public static readonly LangEntry2 ProfileListItemDisabled;
        public static readonly LangEntry2 ProfileListItemSelected;
        public static readonly LangEntry1 ProfileByAuthor;
        public static readonly LangEntry0 ProfileInstallSyntax;
        public static readonly LangEntry0 ProfileInstallShorthandSyntax;
        public static readonly LangEntry1 ProfileUrlInvalid;
        public static readonly LangEntry1 ProfileAlreadyExistsNotEmpty;
        public static readonly LangEntry2 ProfileInstallSuccess;
        public static readonly LangEntry1 ProfileInstallError;
        public static readonly LangEntry2 ProfileDownloadError;
        public static readonly LangEntry2 ProfileParseError;
        public static readonly LangEntry0 ProfileDescribeSyntax;
        public static readonly LangEntry1 ProfileNotFound;
        public static readonly LangEntry1 ProfileEmpty;
        public static readonly LangEntry1 ProfileDescribeHeader;
        public static readonly LangEntry4 ProfileDescribeItem;
        public static readonly LangEntry0 ProfileSelectSyntax;
        public static readonly LangEntry1 ProfileSelectSuccess;
        public static readonly LangEntry1 ProfileSelectEnableSuccess;
        public static readonly LangEntry0 ProfileEnableSyntax;
        public static readonly LangEntry1 ProfileAlreadyEnabled;
        public static readonly LangEntry1 ProfileEnableSuccess;
        public static readonly LangEntry0 ProfileDisableSyntax;
        public static readonly LangEntry1 ProfileAlreadyDisabled;
        public static readonly LangEntry1 ProfileDisableSuccess;
        public static readonly LangEntry0 ProfileReloadSyntax;
        public static readonly LangEntry1 ProfileNotEnabled;
        public static readonly LangEntry1 ProfileReloadSuccess;
        public static readonly LangEntry0 ProfileCreateSyntax;
        public static readonly LangEntry1 ProfileAlreadyExists;
        public static readonly LangEntry1 ProfileCreateSuccess;
        public static readonly LangEntry0 ProfileRenameSyntax;
        public static readonly LangEntry2 ProfileRenameSuccess;
        public static readonly LangEntry0 ProfileClearSyntax;
        public static readonly LangEntry1 ProfileClearSuccess;
        public static readonly LangEntry0 ProfileDeleteSyntax;
        public static readonly LangEntry1 ProfileDeleteBlocked;
        public static readonly LangEntry1 ProfileDeleteSuccess;
        public static readonly LangEntry0 ProfileMoveToSyntax;
        public static readonly LangEntry2 ProfileMoveToAlreadyPresent;
        public static readonly LangEntry3 ProfileMoveToSuccess;
        public static readonly LangEntry0 ProfileHelpHeader;
        public static readonly LangEntry0 ProfileHelpList;
        public static readonly LangEntry0 ProfileHelpDescribe;
        public static readonly LangEntry0 ProfileHelpEnable;
        public static readonly LangEntry0 ProfileHelpDisable;
        public static readonly LangEntry0 ProfileHelpReload;
        public static readonly LangEntry0 ProfileHelpSelect;
        public static readonly LangEntry0 ProfileHelpCreate;
        public static readonly LangEntry0 ProfileHelpRename;
        public static readonly LangEntry0 ProfileHelpClear;
        public static readonly LangEntry0 ProfileHelpDelete;
        public static readonly LangEntry0 ProfileHelpMoveTo;
        public static readonly LangEntry0 ProfileHelpInstall;
        public static readonly LangEntry0 WireToolInvisible;
        public static readonly LangEntry1 WireToolInvalidColor;
        public static readonly LangEntry0 WireToolNotEquipped;
        public static readonly LangEntry1 WireToolActivated;
        public static readonly LangEntry0 WireToolDeactivated;
        public static readonly LangEntry2 WireToolTypeMismatch;
        public static readonly LangEntry2 WireToolProfileMismatch;
        public static readonly LangEntry0 WireToolMonumentMismatch;
        public static readonly LangEntry0 HelpHeader;
        public static readonly LangEntry0 HelpSpawn;
        public static readonly LangEntry0 HelpPrefab;
        public static readonly LangEntry0 HelpKill;
        public static readonly LangEntry0 HelpUndo;
        public static readonly LangEntry0 HelpSave;
        public static readonly LangEntry0 HelpFlag;
        public static readonly LangEntry0 HelpSkin;
        public static readonly LangEntry0 HelpSetId;
        public static readonly LangEntry0 HelpSetDir;
        public static readonly LangEntry0 HelpSkull;
        public static readonly LangEntry0 HelpTrophy;
        public static readonly LangEntry0 HelpCardReaderLevel;
        public static readonly LangEntry0 HelpPuzzle;
        public static readonly LangEntry0 HelpSpawnGroup;
        public static readonly LangEntry0 HelpSpawnPoint;
        public static readonly LangEntry0 HelpPaste;
        public static readonly LangEntry0 HelpEdit;
        public static readonly LangEntry0 HelpShow;
        public static readonly LangEntry0 HelpShowVanilla;
        public static readonly LangEntry0 HelpProfile;
        public string Name;
        public string English;
        protected LangEntry(string name, string english);
    }

    private class LangEntry0 : LangEntry, IMessageFormatter
    {
        public LangEntry0(string name, string english);
        public string Format(TemplateProvider templateProvider);
    }

    private class LangEntry1 : LangEntry
    {
        public LangEntry1(string name, string english);
        public Formatter Bind(Tuple1 args);
        public Formatter Bind(object arg1);
    }

    private class LangEntry2 : LangEntry
    {
        public LangEntry2(string name, string english);
        public Formatter Bind(Tuple2 args);
        public Formatter Bind(object arg1, object arg2);
    }

    private class LangEntry3 : LangEntry
    {
        public LangEntry3(string name, string english);
        public Formatter Bind(Tuple3 args);
        public Formatter Bind(object arg1, object arg2, object arg3);
    }

    private class LangEntry4 : LangEntry
    {
        public LangEntry4(string name, string english);
        public Formatter Bind(Tuple4 args);
        public Formatter Bind(object arg1, object arg2, object arg3, object arg4);
    }

    private string GetMessage(string playerId, T formatter);
    private string GetMessage(string playerId, LangEntry1 langEntry, object arg1);
    private string GetMessage(string playerId, LangEntry2 langEntry, object arg1, object arg2);
    private string GetMessage(string playerId, LangEntry3 langEntry, object arg1, object arg2, object arg3);
    private string GetMessage(string playerId, LangEntry4 langEntry, object arg1, object arg2, object arg3, object arg4);
    private void ReplyToPlayer(IPlayer player, T formatter);
    private void ReplyToPlayer(IPlayer player, LangEntry1 langEntry, object arg1);
    private void ReplyToPlayer(IPlayer player, LangEntry2 langEntry, object arg1, object arg2);
    private void ReplyToPlayer(IPlayer player, LangEntry3 langEntry, object arg1, object arg2, object arg3);
    private void ReplyToPlayer(IPlayer player, LangEntry4 langEntry, object arg1, object arg2, object arg3, object arg4);
    private void ChatMessage(BasePlayer player, T formatter);
    private void ChatMessage(BasePlayer player, LangEntry1 langEntry, object arg1);
    private void ChatMessage(BasePlayer player, LangEntry2 langEntry, object arg1, object arg2);
    private void ChatMessage(BasePlayer player, LangEntry3 langEntry, object arg1, object arg2, object arg3);
    private void ChatMessage(BasePlayer player, LangEntry4 langEntry, object arg1, object arg2, object arg3, object arg4);
    private string GetAuthorSuffix(IPlayer player, string author);
    private string GetAddonName(IPlayer player, BaseData data);
    protected override void LoadDefaultMessages();
}

private static class PasteUtils
{
    private static readonly string[] CopyPasteArgs;
    private static VersionNumber _requiredVersion;
    public static bool IsCopyPasteCompatible(Plugin copyPaste);
    public static bool DoesPasteExist(string filename);
    public static Action PasteWithCancelCallback(Plugin copyPaste, PasteData pasteData, Vector3 position, float yRotation, Action<BaseEntity> onEntityPasted, Action onPasteCompleted);
}

private static class ExposedHooks
{
    public static void OnMonumentAddonsInitialized();
    public static void OnMonumentEntitySpawned(BaseEntity entity, Component monument, Guid guid);
    public static void OnMonumentPrefabCreated(GameObject gameObject, Component monument, Guid guid);
    public static object OnDynamicMonument(BaseEntity entity);
}

private static class ObjectCache
{
    private static readonly object True;
    private static readonly object False;
    private static class StaticObjectCache
    {
        private static readonly Dictionary<T, object> _cacheByValue;
        public static object Get(T value);
    }

    public static object Get(T value);
    public static object Get(bool value);
}

private static class StaticObjectCache
{
    private static readonly Dictionary<T, object> _cacheByValue;
    public static object Get(T value);
}

private static class StringUtils
{
    public static bool EqualsCaseInsensitive(string a, string b);
    public static bool Contains(string haystack, string needle);
}

private static class SearchUtils
{
    public static List<string> FindModderPrefabMatches(string partialName);
    public static List<T> FindPrefabMatches(IEnumerable<T> sourceList, Func<T, string> selector, string partialName, UniqueNameRegistry uniqueNameRegistry);
    public static List<string> FindEntityPrefabMatches(string partialName, UniqueNameRegistry uniqueNameRegistry);
    public static List<T> FindCustomAddonMatches(IEnumerable<T> sourceList, Func<T, string> selector, string partialName);
    public static List<CustomAddonDefinition> FindCustomAddonMatches(string partialName, IEnumerable<CustomAddonDefinition> customAddons);
    public static List<T> FindMatches(IEnumerable<T> sourceList, Func<T, bool>[] predicateList);
}

private static class EntityUtils
{
    public static T GetNearbyEntity(BaseEntity originEntity, float maxDistance, int layerMask, string filterShortPrefabName);
    public static T GetClosestNearbyEntity(Vector3 position, float maxDistance, int layerMask, Func<T, bool> predicate);
    public static T GetClosestNearbyComponent(Vector3 position, float maxDistance, int layerMask, Func<T, bool> predicate);
    public static void ConnectNearbyVehicleSpawner(VehicleVendor vehicleVendor);
    public static void ConnectNearbyVehicleVendor(VehicleSpawner vehicleSpawner);
    public static void ConnectNearbyDoor(DoorManipulator doorManipulator);
    private static T GetClosestComponent(Vector3 position, List<T> componentList, Func<T, bool> predicate);
}

private static class EntitySetupUtils
{
    public static bool ShouldBeImmortal(BaseEntity entity);
    public static void PreSpawnShared(BaseEntity entity);
    public static void PostSpawnShared(MonumentAddons plugin, BaseEntity entity, bool enableSaving);
}

private static class BooleanParser
{
    private static string[] _booleanYesValues;
    private static string[] _booleanNoValues;
    public static bool TryParse(string arg, bool value);
}

private class ValueRotator
{
    private T[] _values;
    private int _index;
    public ValueRotator(T[] values);
    public T GetNext();
    public void Reset();
}

private class HookCollection
{
    private MonumentAddons _plugin;
    private string[] _hookNames;
    private Func<bool> _shouldSubscribe;
    private bool _isSubscribed;
    public HookCollection(MonumentAddons plugin, string[] hookNames, Func<bool> shouldSubscribe);
    public void Refresh();
    public void Subscribe();
    public void Unsubscribe();
}

private class UniqueNameRegistry
{
    private Dictionary<string, string> _uniqueNameByPrefabPath;
    public void OnServerInitialized();
    public string GetUniqueShortName(string prefabPath);
    private string[] GetSegments(string prefabPath);
    private string GetPartialPath(string[] segments, int numSegments);
    private void BuildIndex();
}

private class EmptyMonoBehavior : MonoBehaviour
{
}

private class CoroutineManager
{
    public static Coroutine StartGlobalCoroutine(IEnumerator enumerator);
    private static IEnumerator CallbackRoutine(Coroutine dependency, Action action);
    private MonoBehaviour _coroutineComponent;
    public Coroutine StartCoroutine(IEnumerator enumerator);
    public Coroutine StartCallbackRoutine(Coroutine coroutine, Action callback);
    public void StopAll();
    public void Destroy();
}

private class WireToolManager
{
    private const float DrawDuration;
    private const float DrawSlotRadius;
    private const float PreviewDotRadius;
    private const float InputCooldown;
    private const float HoldToClearDuration;
    private const float DrawIntervalDuration;
    private const float DrawIntervalWithBuffer;
    private const float MinAngleDot;
    private class WireSession
    {
        public BasePlayer Player { get; set; }
        public WireColour? WireColor;
        public IOType WireType;
        public EntityAdapter Adapter;
        public IOSlot StartSlot;
        public int StartSlotIndex;
        public bool IsSource;
        public float LastInput;
        public float SecondaryPressedTime;
        public float LastDrawTime;
        public List<Vector3> Points;
        public WireSession(BasePlayer player);
        public void Reset(bool refreshPoints);
        public void StartConnection(EntityAdapter adapter, IOSlot startSlot, int slotIndex, bool isSource);
        public void AddPoint(Vector3 position);
        public void RemoveLast();
        public bool IsOnInputCooldown();
        public bool IsOnDrawCooldown();
        public void RefreshPoints();
    }

    private MonumentAddons _plugin;
    private ProfileStore _profileStore;
    private AddonComponentTracker _componentTracker { get; set; }
    private List<WireSession> _playerSessions;
    private Timer _timer;
    public WireToolManager(MonumentAddons plugin, ProfileStore profileStore);
    public bool HasPlayer(BasePlayer player);
    public void StartOrUpdateSession(BasePlayer player, WireColour? wireColor);
    public void StopSession(BasePlayer player);
    public void Unload();
    private WireSession GetPlayerSession(BasePlayer player, int index);
    private WireSession GetPlayerSession(BasePlayer player);
    private void DestroySession(WireSession session, int index);
    private EntityAdapter GetLookIOAdapter(BasePlayer player, IOEntity ioEntity);
    private IOSlot GetClosestIOSlot(IOEntity ioEntity, Ray ray, float minDot, int index, float highestDot, bool wantsSourceSlot, bool? wantsOccupiedSlots);
    private IOSlot GetClosestIOSlot(IOEntity ioEntity, Ray ray, float minDot, int index, bool isSourceSlot, bool? wantsOccupiedSlots);
    private bool CanPlayerUseTool(BasePlayer player);
    private Color DetermineSlotColor(IOSlot slot);
    private void ShowSlots(BasePlayer player, IOEntity ioEntity, bool showSourceSlots);
    private void DrawSessionState(WireSession session);
    private void MaybeStartWire(WireSession session, EntityAdapter adapter);
    private void MaybeEndWire(WireSession session, EntityAdapter adapter);
    private void MaybeClearWire(WireSession session);
    private void ProcessPlayers();
}

private class WireSession
{
    public BasePlayer Player { get; set; }
    public WireColour? WireColor;
    public IOType WireType;
    public EntityAdapter Adapter;
    public IOSlot StartSlot;
    public int StartSlotIndex;
    public bool IsSource;
    public float LastInput;
    public float SecondaryPressedTime;
    public float LastDrawTime;
    public List<Vector3> Points;
    public WireSession(BasePlayer player);
    public void Reset(bool refreshPoints);
    public void StartConnection(EntityAdapter adapter, IOSlot startSlot, int slotIndex, bool isSource);
    public void AddPoint(Vector3 position);
    public void RemoveLast();
    public bool IsOnInputCooldown();
    public bool IsOnDrawCooldown();
    public void RefreshPoints();
}

private abstract class DictionaryKeyConverter : JsonConverter
{
    public virtual string KeyToString(TKey key);
    public abstract TKey KeyFromString(string key);
    public override bool CanConvert(Type objectType);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
}

private class HtmlColorConverter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
}

private class ActionDebounced
{
    private PluginTimers _pluginTimers;
    private float _seconds;
    private Action _action;
    private Timer _timer;
    public ActionDebounced(PluginTimers pluginTimers, float seconds, Action action);
    public void Schedule();
    public void Flush();
}

private class MonumentHelper
{
    private MonumentAddons _plugin;
    private Plugin _monumentFinder { get; set; }
    private Dictionary<DungeonGridInfo, MonumentInfo> _entranceToMonument;
    public MonumentHelper(MonumentAddons plugin);
    public void OnServerInitialized();
    public List<BaseMonument> FindMonumentsByAlias(string alias);
    public List<BaseMonument> FindMonumentsByShortName(string shortName);
    public MonumentAdapter GetClosestMonumentAdapter(Vector3 position);
    public bool IsMonumentUnique(string shortName);
    private List<BaseMonument> WrapFindMonumentResults(List<Dictionary<string, object>> dictList);
    public MonumentInfo GetMonumentFromTunnel(DungeonGridCell dungeonGridCell);
    private MonumentInfo GetMonumentFromEntrance(DungeonGridInfo dungeonGridInfo);
}

private static class PhoneUtils
{
    public const string Tunnel;
    public const string UnderwaterLab;
    public const string CargoShip;
    private static readonly Regex SplitCamelCaseRegex;
    private static readonly string[] PolymorphicMonumentVariants;
    public static void NameTelephone(Telephone telephone, BaseMonument monument, Vector3 position, MonumentHelper monumentHelper);
    private static string GetFTLCorridorPhoneName(string tunnelName, Vector3 position);
    private static string GetFTLPhoneName(string tunnelAlias, DungeonGridCell dungeonGridCell, BaseMonument monument, Vector3 position, MonumentHelper monumentHelper);
    private static string GetFTLTrainStationPhoneName(MonumentInfo attachedMonument, string tunnelName, Vector3 position, MonumentHelper monumentHelper);
    private static string GetUnderwaterLabPhoneName(DungeonBaseLink link, Vector3 position);
    private static string GetDynamicMonumentPhoneName(DynamicMonument monument, Telephone phone);
    private static bool ShouldAppendCoordinate(string monumentShortName, MonumentHelper monumentHelper);
    private static string SplitCamelCase(string camelCase);
}

private abstract class BaseMonument
{
    public Component Object { get; set; }
    public virtual string PrefabName { get; set; }
    public virtual string ShortName { get; set; }
    public virtual string Alias { get; set; }
    public virtual string UniqueDisplayName { get; set; }
    public virtual string UniqueName { get; set; }
    public virtual Vector3 Position { get; set; }
    public virtual Quaternion Rotation { get; set; }
    public virtual bool IsValid { get; set; }
    protected BaseMonument(Component component);
    public virtual Vector3 TransformPoint(Vector3 localPosition);
    public virtual Vector3 InverseTransformPoint(Vector3 worldPosition);
    public abstract Vector3 ClosestPointOnBounds(Vector3 position);
    public abstract bool IsInBounds(Vector3 position);
    public virtual bool IsEquivalentTo(BaseMonument other);
}

private class MonumentAdapter : BaseMonument
{
    public override string PrefabName { get; set; }
    public override string ShortName { get; set; }
    public override string Alias { get; set; }
    public override string UniqueDisplayName { get; set; }
    public override Vector3 Position { get; set; }
    public override Quaternion Rotation { get; set; }
    private Dictionary<string, object> _monumentInfo;
    public MonumentAdapter(Dictionary<string, object> monumentInfo);
    public override Vector3 TransformPoint(Vector3 localPosition);
    public override Vector3 InverseTransformPoint(Vector3 worldPosition);
    public override Vector3 ClosestPointOnBounds(Vector3 position);
    public override bool IsInBounds(Vector3 position);
}

private class DynamicMonument : BaseMonument, IDynamicMonument, IEntityMonument
{
    public BaseEntity RootEntity { get; set; }
    public bool IsMobile { get; set; }
    public override string PrefabName { get; set; }
    public override string ShortName { get; set; }
    public override string UniqueName { get; set; }
    public override string UniqueDisplayName { get; set; }
    public override bool IsValid { get; set; }
    public NetworkableId EntityId { get; set; }
    protected OBB BoundingBox { get; set; }
    public DynamicMonument(BaseEntity entity, bool isMobile);
    public DynamicMonument(BaseEntity entity);
    public override Vector3 ClosestPointOnBounds(Vector3 position);
    public override bool IsInBounds(Vector3 position);
    public override bool IsEquivalentTo(BaseMonument other);
}

private class CustomMonument : BaseMonument
{
    public readonly Plugin OwnerPlugin;
    public readonly Component Component;
    public Bounds Bounds;
    private readonly string _monumentName;
    private Vector3 _position;
    public override string PrefabName { get; set; }
    public override string ShortName { get; set; }
    public override string UniqueName { get; set; }
    public override string UniqueDisplayName { get; set; }
    public override Vector3 Position { get; set; }
    public OBB BoundingBox { get; set; }
    public CustomMonument(Plugin ownerPlugin, Component component, string monumentName, Bounds bounds);
    public override Vector3 ClosestPointOnBounds(Vector3 position);
    public override bool IsInBounds(Vector3 position);
    public override bool IsEquivalentTo(BaseMonument other);
}

private class CustomEntityMonument : CustomMonument, IEntityMonument
{
    public BaseEntity RootEntity { get; set; }
    public NetworkableId EntityId { get; set; }
    public bool IsMobile { get; set; }
    public CustomEntityMonument(Plugin ownerPlugin, BaseEntity entity, string monumentName, Bounds bounds);
}

private class CustomMonumentComponent : FacepunchBehaviour
{
    public static CustomMonumentComponent AddToMonument(CustomMonumentManager manager, CustomMonument monument);
    public CustomMonument Monument { get; set; }
    private CustomMonumentManager _manager;
    private void OnDestroy();
}

private class CustomMonumentManager
{
    private readonly MonumentAddons _plugin;
    public readonly List<CustomMonument> MonumentList;
    public CustomMonumentManager(MonumentAddons plugin);
    public void Register(CustomMonument monument);
    public void Unregister(CustomMonument monument);
    public void UnregisterAllForPlugin(Plugin plugin);
    public CustomMonument FindByComponent(Component component);
    public CustomMonument FindByPosition(Vector3 position);
    public int CountMonumentByName(string name);
    public List<BaseMonument> FindMonumentsByName(string name);
    private IEnumerator KillRoutine(List<BaseAdapter> adapterList);
}

private abstract class BaseUndo
{
    private const float ExpireAfterSeconds;
    public virtual bool IsValid { get; set; }
    protected readonly MonumentAddons _plugin;
    protected readonly ProfileController _profileController;
    protected readonly string _monumentIdentifier;
    private readonly float _undoTime;
    protected Profile Profile { get; set; }
    private bool IsExpired { get; set; }
    private bool ProfileExists { get; set; }
    protected BaseUndo(MonumentAddons plugin, ProfileController profileController, string monumentIdentifier);
    public abstract void Undo(BasePlayer player);
}

private class UndoKill : BaseUndo
{
    protected readonly BaseData _data;
    public UndoKill(MonumentAddons plugin, ProfileController profileController, string monumentIdentifier, BaseData data);
    public override void Undo(BasePlayer player);
}

private class UndoKillSpawnPoint : UndoKill
{
    private readonly SpawnGroupData _spawnGroupData;
    private readonly SpawnPointData _spawnPointData;
    public UndoKillSpawnPoint(MonumentAddons plugin, ProfileController profileController, string monumentIdentifier, SpawnGroupData spawnGroupData, SpawnPointData spawnPointData);
    public override void Undo(BasePlayer player);
}

private class UndoManager
{
    private static void CleanStack(Stack<BaseUndo> stack);
    private readonly Dictionary<ulong, Stack<BaseUndo>> _undoStackByPlayer;
    public bool TryUndo(BasePlayer player);
    public void AddUndo(BasePlayer player, BaseUndo undoAction);
    private Stack<BaseUndo> EnsureUndoStack(BasePlayer player);
    private Stack<BaseUndo> GetUndoStack(BasePlayer player);
}

private class AddonComponent : FacepunchBehaviour, IAddonComponent
{
    public static AddonComponent AddToComponent(AddonComponentTracker componentTracker, Component hostComponent, TransformAdapter adapter);
    public static void RemoveFromComponent(Component component);
    public static AddonComponent GetForComponent(BaseEntity entity);
    public TransformAdapter Adapter { get; set; }
    private Component _hostComponent;
    private AddonComponentTracker _componentTracker;
    private void OnDestroy();
}

private class AddonComponentTracker
{
    private static bool IsComponentValid(Component component);
    private HashSet<Component> _trackedComponents;
    public void RegisterComponent(Component component);
    public void UnregisterComponent(Component component);
    public bool IsAddonComponent(Component component);
    public bool IsAddonComponent(Component component, TAdapter adapter, TController controller);
    public bool IsAddonComponent(Component component, TController controller);
}

private abstract class BaseAdapter
{
    public abstract bool IsValid { get; set; }
    public BaseData Data { get; set; }
    public BaseController Controller { get; set; }
    public BaseMonument Monument { get; set; }
    public MonumentAddons Plugin { get; set; }
    public ProfileController ProfileController { get; set; }
    public Profile Profile { get; set; }
    protected Configuration _config { get; set; }
    protected ProfileStateData _profileStateData { get; set; }
    protected IOManager _ioManager { get; set; }
    public IEnumerator WaitInstruction { get; set; }
    protected BaseAdapter(BaseData data, BaseController controller, BaseMonument monument);
    public abstract void Spawn();
    public abstract void Kill();
    public abstract void OnComponentDestroyed(Component component);
    public virtual void DetachSavedEntities();
    public virtual void PreUnload();
}

private abstract class TransformAdapter : BaseAdapter
{
    public BaseTransformData TransformData { get; set; }
    public abstract Component Component { get; set; }
    public abstract Transform Transform { get; set; }
    public abstract Vector3 Position { get; set; }
    public abstract Quaternion Rotation { get; set; }
    public Vector3 LocalPosition { get; set; }
    public Quaternion LocalRotation { get; set; }
    public Vector3 IntendedPosition { get; set; }
    public Quaternion IntendedRotation { get; set; }
    public virtual bool IsAtIntendedPosition { get; set; }
    protected TransformAdapter(BaseTransformData transformData, BaseController controller, BaseMonument monument);
    public virtual bool TryRecordUpdates(Transform moveTransform, Transform rotateTransform);
}

private abstract class BaseController
{
    public ProfileController ProfileController { get; set; }
    public BaseData Data { get; set; }
    public List<BaseAdapter> Adapters { get; set; }
    public MonumentAddons Plugin { get; set; }
    public Profile Profile { get; set; }
    private bool _enabled;
    protected BaseController(ProfileController profileController, BaseData data);
    public abstract BaseAdapter CreateAdapter(BaseMonument monument);
    public virtual void OnAdapterSpawned(BaseAdapter adapter);
    public virtual void OnAdapterKilled(BaseAdapter adapter);
    public virtual BaseAdapter SpawnAtMonument(BaseMonument monument);
    public virtual IEnumerator SpawnAtMonumentsRoutine(IEnumerable<BaseMonument> monumentList);
    public virtual Coroutine Kill(BaseData data);
    public bool HasAdapterForMonument(BaseMonument monument);
    public Coroutine Kill();
    public void PreUnload();
    public IEnumerator KillRoutine();
    public void DetachSavedEntities();
    public BaseAdapter FindAdapterForMonument(BaseMonument monument);
}

private class PrefabAdapter : TransformAdapter
{
    public GameObject GameObject { get; set; }
    public PrefabData PrefabData { get; set; }
    public override Component Component { get; set; }
    public override Transform Transform { get; set; }
    public override Vector3 Position { get; set; }
    public override Quaternion Rotation { get; set; }
    public override bool IsValid { get; set; }
    private Transform _transform;
    public PrefabAdapter(BaseController controller, PrefabData prefabData, BaseMonument monument);
    public override void Spawn();
    public override void Kill();
    public void HandleChanges();
    public override void OnComponentDestroyed(Component component);
}

private class PrefabController : BaseController, IUpdateableController
{
    public PrefabData PrefabData { get; set; }
    public PrefabController(ProfileController profileController, PrefabData prefabData);
    public override BaseAdapter CreateAdapter(BaseMonument monument);
    public Coroutine StartUpdateRoutine();
    private IEnumerator UpdateRoutine();
}

private class EntityAdapter : TransformAdapter
{
    private class IOEntityOverrideInfo
    {
        public Dictionary<int, Vector3> Inputs;
        public Dictionary<int, Vector3> Outputs;
    }

    private static readonly Dictionary<string, IOEntityOverrideInfo> IOOverridesByEntity;
    public BaseEntity Entity { get; set; }
    public EntityData EntityData { get; set; }
    public virtual bool IsDestroyed { get; set; }
    public override Component Component { get; set; }
    public override Transform Transform { get; set; }
    public override Vector3 Position { get; set; }
    public override Quaternion Rotation { get; set; }
    public override bool IsValid { get; set; }
    private Transform _transform;
    private PuzzleResetHandler _puzzleResetHandler;
    private BuildingGrade.Enum IntendedBuildingGrade { get; set; }
    public EntityAdapter(BaseController controller, EntityData entityData, BaseMonument monument);
    public override void Spawn();
    public override void Kill();
    public override void OnComponentDestroyed(Component component);
    public void UpdateScale();
    public void UpdateSkin();
    public override bool TryRecordUpdates(Transform moveTransform, Transform rotateTransform);
    public virtual void HandleChanges();
    public void UpdateSkullName();
    public void UpdateHuntingTrophy();
    public void UpdateIOConnections();
    public void MaybeProvidePower();
    public override void PreUnload();
    public void HandlePuzzleReset();
    protected virtual void PreEntitySpawn();
    protected virtual void PostEntitySpawn();
    protected virtual void PreEntityKill();
    public override void DetachSavedEntities();
    protected BaseEntity CreateEntity(Vector3 position, Quaternion rotation);
    private bool ShouldEnableSaving(BaseEntity entity);
    private void UpdatePosition();
    private BaseEntity FindAndCleanupDuplicateEntities(BaseEntity ignoreEntity);
    private List<CCTV_RC> GetNearbyStaticCameras();
    private void GatherStaticCameras(ComputerStation computerStation);
    private BaseEntity GetEntityToMove();
    private void UpdateBuildingGrade();
    private void UpdatePuzzle();
    private void UpdateCardReaderLevel();
    private void DisableFlags();
    private void EnableFlags();
    private void UpdateIOEntitySlotPositions(IOEntity ioEntity);
    private void SetupIOSlot(IOSlot slot, IOEntity otherEntity, int otherSlotIndex, WireColour color);
    private void ClearIOSlot(IOEntity ioEntity, IOSlot slot);
}

private class IOEntityOverrideInfo
{
    public Dictionary<int, Vector3> Inputs;
    public Dictionary<int, Vector3> Outputs;
}

private class EntityController : BaseController, IUpdateableController
{
    public EntityData EntityData { get; set; }
    private Dictionary<string, object> _vendingDataProvider;
    public EntityController(ProfileController profileController, EntityData entityData);
    public override BaseAdapter CreateAdapter(BaseMonument monument);
    public override void OnAdapterSpawned(BaseAdapter adapter);
    public override void OnAdapterKilled(BaseAdapter adapter);
    public Coroutine StartUpdateRoutine();
    public Dictionary<string, object> GetVendingDataProvider();
    private IEnumerator HandleChangesRoutine();
}

private class SignAdapter : EntityAdapter
{
    public SignAdapter(BaseController controller, EntityData entityData, BaseMonument monument);
    public override void PreUnload();
    public uint[] GetTextureIds();
    public void SetTextureIds(uint[] textureIds);
    public void SkinSign();
    protected override void PreEntitySpawn();
    protected override void PreEntityKill();
    private void DeleteSignFiles();
}

private class SignController : EntityController
{
    public SignController(ProfileController profileController, EntityData data);
    protected SignAdapter _primaryAdapter;
    public override BaseAdapter CreateAdapter(BaseMonument monument);
    public override void OnAdapterSpawned(BaseAdapter adapter);
    public override void OnAdapterKilled(BaseAdapter adapter);
    public void UpdateSign(uint[] textureIds);
}

private class CCTVEntityAdapter : EntityAdapter
{
    private int _idSuffix;
    private string _cachedIdentifier;
    private string _savedIdentifier { get; set; }
    public CCTVEntityAdapter(BaseController controller, EntityData entityData, BaseMonument monument, int idSuffix);
    public override void PreUnload();
    protected override void PreEntitySpawn();
    protected override void PostEntitySpawn();
    public override void OnComponentDestroyed(Component component);
    public override void HandleChanges();
    public void UpdateIdentifier();
    public void UpdateDirection();
    public string GetIdentifier();
    private void SetIdentifier(string id);
    private List<ComputerStation> GetNearbyStaticComputerStations();
}

private class CCTVController : EntityController
{
    private int _nextId;
    public CCTVController(ProfileController profileController, EntityData data);
    public override BaseAdapter CreateAdapter(BaseMonument monument);
}

private abstract class AdapterListenerBase
{
    public virtual void Init();
    public virtual void OnServerInitialized();
    public abstract bool InterestedInAdapter(BaseAdapter adapter);
    public abstract void OnAdapterSpawned(BaseAdapter adapter);
    public abstract void OnAdapterKilled(BaseAdapter adapter);
    protected static bool IsEntityAdapter(BaseAdapter adapter);
}

private abstract class DynamicHookListener : AdapterListenerBase
{
    private MonumentAddons _plugin;
    protected string[] _dynamicHookNames;
    private HashSet<BaseAdapter> _adapters;
    protected DynamicHookListener(MonumentAddons plugin);
    public override void Init();
    public override void OnAdapterSpawned(BaseAdapter adapter);
    public override void OnAdapterKilled(BaseAdapter adapter);
    private void SubscribeHooks();
    private void UnsubscribeHooks();
}

private class SignEntityListener : DynamicHookListener
{
    public SignEntityListener(MonumentAddons plugin);
    public override bool InterestedInAdapter(BaseAdapter adapter);
}

private class BuildingBlockEntityListener : DynamicHookListener
{
    public BuildingBlockEntityListener(MonumentAddons plugin);
    public override bool InterestedInAdapter(BaseAdapter adapter);
}

private class SprayDecalListener : DynamicHookListener
{
    public SprayDecalListener(MonumentAddons plugin);
    public override bool InterestedInAdapter(BaseAdapter adapter);
}

private class AdapterListenerManager
{
    private AdapterListenerBase[] _listeners;
    public AdapterListenerManager(MonumentAddons plugin);
    public void Init();
    public void OnServerInitialized();
    public void OnAdapterSpawned(BaseAdapter entityAdapter);
    public void OnAdapterKilled(BaseAdapter entityAdapter);
}

private class PuzzleResetHandler : FacepunchBehaviour
{
    public static PuzzleResetHandler Create(EntityAdapter adapter, PuzzleReset puzzleReset);
    private EntityAdapter _adapter;
    private void OnPuzzleReset();
    public void Destroy();
}

private class SpawnedVehicleComponent : FacepunchBehaviour
{
    public static void AddToVehicle(MonumentAddons plugin, GameObject gameObject);
    private const float MaxDistanceSquared;
    private MonumentAddons _plugin;
    private Vector3 _originalPosition;
    private Transform _transform;
    public void Awake();
    public void CheckPositionTracked();
    private void CheckPosition();
}

private class CustomAddonSpawnPointInstance : SpawnPointInstance
{
    public static CustomAddonSpawnPointInstance AddToComponent(AddonComponentTracker componentTracker, Component hostComponent, CustomAddonDefinition customAddonDefinition);
    public CustomAddonDefinition CustomAddonDefinition { get; set; }
    private Component _hostComponent;
    private AddonComponentTracker _componentTracker;
    public void Kill();
    public new void OnDestroy();
}

private class CustomSpawnPoint : BaseSpawnPoint, IAddonComponent
{
    private const int TrainCarLayerMask;
    private static readonly Dictionary<string, int> CustomBoundsCheckMask;
    public static CustomSpawnPoint AddToGameObject(AddonComponentTracker componentTracker, GameObject gameObject, SpawnPointAdapter adapter, SpawnPointData spawnPointData);
    public TransformAdapter Adapter { get; set; }
    private AddonComponentTracker _componentTracker;
    private SpawnPointAdapter _spawnPointAdapter;
    private SpawnPointData _spawnPointData;
    private Transform _transform;
    private BaseEntity _parentEntity;
    private List<SpawnPointInstance> _instances;
    public void PreUnload();
    private void Awake();
    public override void GetLocation(Vector3 position, Quaternion rotation);
    public override void ObjectSpawned(SpawnPointInstance instance);
    public override void ObjectRetired(SpawnPointInstance instance);
    public bool IsAvailableTo(CustomSpawnGroup.SpawnEntry spawnEntry);
    public override bool IsAvailableTo(GameObject prefab);
    public override bool HasPlayersIntersecting();
    public void KillSpawnedInstances(WeightedPrefabData weightedPrefabData);
    private bool IsVehicle(BaseEntity entity);
    private void DisableVehicleDecay(BaseEntity vehicle);
    public void MoveSpawnedInstances();
    private void OnDestroy();
}

private class CustomSpawnGroup : SpawnGroup
{
    public new class SpawnEntry
    {
        public readonly CustomAddonDefinition CustomAddonDefinition;
        public readonly GameObjectRef Prefab;
        public int Weight;
        public bool IsValid { get; set; }
        public SpawnEntry(CustomAddonDefinition customAddonDefinition, int weight);
        public SpawnEntry(GameObjectRef prefab, int weight);
    }

    private static AIInformationZone FindVirtualInfoZone(Vector3 position);
    public SpawnGroupAdapter SpawnGroupAdapter { get; set; }
    public List<SpawnEntry> SpawnEntries { get; set; }
    private AIInformationZone _cachedInfoZone;
    private bool _didLookForInfoZone;
    private SpawnGroupData SpawnGroupData { get; set; }
    public void Init(SpawnGroupAdapter spawnGroupAdapter);
    public void UpdateSpawnClock();
    public void HandleObjectRetired();
    public void OnPuzzleReset();
    protected override void Spawn(int numToSpawn);
    protected override void PostSpawnProcess(BaseEntity entity, BaseSpawnPoint spawnPoint);
    private bool HasSpawned(SpawnEntry spawnEntry);
    private BaseSpawnPoint GetRandomSpawnPoint(SpawnEntry spawnEntry, Vector3 position, Quaternion rotation);
    private float DetermineSpawnEntryWeight(SpawnEntry spawnEntry);
    private SpawnEntry GetRandomSpawnEntry();
    private AIInformationZone GetVirtualInfoZone();
    private void ResetSpawnClock();
    private new void OnDestroy();
}

public new class SpawnEntry
{
    public readonly CustomAddonDefinition CustomAddonDefinition;
    public readonly GameObjectRef Prefab;
    public int Weight;
    public bool IsValid { get; set; }
    public SpawnEntry(CustomAddonDefinition customAddonDefinition, int weight);
    public SpawnEntry(GameObjectRef prefab, int weight);
}

private class SpawnPointAdapter : TransformAdapter
{
    public SpawnPointData SpawnPointData { get; set; }
    public SpawnGroupAdapter SpawnGroupAdapter { get; set; }
    public CustomSpawnPoint SpawnPoint { get; set; }
    public override Component Component { get; set; }
    public override Transform Transform { get; set; }
    public override Vector3 Position { get; set; }
    public override Quaternion Rotation { get; set; }
    public override bool IsValid { get; set; }
    private Transform _transform;
    public SpawnPointAdapter(SpawnPointData spawnPointData, SpawnGroupAdapter spawnGroupAdapter, BaseController controller, BaseMonument monument);
    public override void Spawn();
    public override void Kill();
    public override void OnComponentDestroyed(Component component);
    public override void PreUnload();
    public void KillSpawnedInstances(WeightedPrefabData weightedPrefabData);
    public void UpdatePosition();
    public override bool TryRecordUpdates(Transform moveTransform, Transform rotateTransform);
}

private class SpawnGroupAdapter : BaseAdapter
{
    public SpawnGroupData SpawnGroupData { get; set; }
    public List<SpawnPointAdapter> SpawnPointAdapters { get; set; }
    public CustomSpawnGroup SpawnGroup { get; set; }
    public PuzzleReset AssociatedPuzzleReset { get; set; }
    public override bool IsValid { get; set; }
    public SpawnGroupAdapter(SpawnGroupData spawnGroupData, BaseController controller, BaseMonument monument);
    public override void Spawn();
    public override void Kill();
    public override void OnComponentDestroyed(Component component);
    public override void PreUnload();
    public void OnSpawnPointAdapterKilled(SpawnPointAdapter spawnPointAdapter);
    public void OnSpawnGroupKilled();
    public void UpdateCustomAddons(CustomAddonDefinition customAddonDefinition);
    private Vector3 GetMidpoint();
    private PuzzleReset FindClosestVanillaPuzzleReset(float maxDistance);
    private void RegisterWithPuzzleReset(PuzzleReset puzzleReset);
    private void UnregisterWithPuzzleReset(PuzzleReset puzzleReset);
    private bool IsRegisteredWithPuzzleReset(PuzzleReset puzzleReset);
    private void UpdateProperties();
    private void UpdatePrefabEntries();
    private void UpdateSpawnPointReferences();
    private void UpdateSpawnPointPositions();
    private void UpdatePuzzleResetAssociation();
    public void UpdateSpawnGroup();
    public void SpawnTick();
    public void KillSpawnedInstances(WeightedPrefabData weightedPrefabData);
    public void CreateSpawnPoint(SpawnPointData spawnPointData);
    public void KillSpawnPoint(SpawnPointData spawnPointData);
    private SpawnPointAdapter FindSpawnPoint(SpawnPointData spawnPointData);
}

private class SpawnGroupController : BaseController, IUpdateableController
{
    public SpawnGroupData SpawnGroupData { get; set; }
    public IEnumerable<SpawnGroupAdapter> SpawnGroupAdapters { get; set; }
    public SpawnGroupController(ProfileController profileController, SpawnGroupData spawnGroupData);
    public override BaseAdapter CreateAdapter(BaseMonument monument);
    public override Coroutine Kill(BaseData data);
    public void CreateSpawnPoint(SpawnPointData spawnPointData);
    public void KillSpawnPoint(SpawnPointData spawnPointData);
    public void UpdateSpawnGroups();
    public Coroutine StartUpdateRoutine();
    public void StartSpawnRoutine();
    public void StartKillSpawnedInstancesRoutine(WeightedPrefabData weightedPrefabData);
    public void StartRespawnRoutine();
    private IEnumerator UpdateRoutine();
    private IEnumerator SpawnTickRoutine();
    private IEnumerator KillSpawnedInstancesRoutine(WeightedPrefabData weightedPrefabData);
    private IEnumerator RespawnRoutine();
}

private class PasteAdapter : TransformAdapter
{
    private const float CopyPasteMagicRotationNumber;
    private const float MaxWaitSeconds;
    private const float KillBatchSize;
    public PasteData PasteData { get; set; }
    public OBB Bounds { get; set; }
    public override Component Component { get; set; }
    public override Transform Transform { get; set; }
    public override Vector3 Position { get; set; }
    public override Quaternion Rotation { get; set; }
    public override bool IsValid { get; set; }
    public override bool IsAtIntendedPosition { get; set; }
    private Transform _transform;
    private Vector3 _spawnedPosition;
    private Quaternion _spawnedRotation;
    private Bounds _bounds;
    private bool _isWorking;
    private Action _cancelPaste;
    private List<BaseEntity> _pastedEntities;
    public PasteAdapter(PasteData pasteData, BaseController controller, BaseMonument monument);
    public override void Spawn();
    public override void Kill();
    public override void OnComponentDestroyed(Component component);
    public void HandleChanges();
    private void SpawnPaste(Vector3 position, Quaternion rotation);
    private IEnumerator SpawnPasteRoutine(Vector3 position, Quaternion rotation);
    private IEnumerator RespawnPasteRoutine(Vector3 position, Quaternion rotation);
    private Coroutine KillPaste();
    private IEnumerator KillRoutine();
    private void OnEntityPasted(BaseEntity entity);
    private void OnPasteComplete();
    private Bounds GetBounds();
}

private class PasteController : BaseController, IUpdateableController
{
    public PasteData PasteData { get; set; }
    public PasteController(ProfileController profileController, PasteData pasteData);
    public override BaseAdapter CreateAdapter(BaseMonument monument);
    public override IEnumerator SpawnAtMonumentsRoutine(IEnumerable<BaseMonument> monumentList);
    public Coroutine StartUpdateRoutine();
    private IEnumerator UpdateRoutine();
}

private class CustomAddonDefinition
{
    public static CustomAddonDefinition FromDictionary(string addonName, Plugin plugin, Dictionary<string, object> addonSpec);
    public string AddonName;
    public Plugin OwnerPlugin;
    private CustomInitializeCallback Initialize;
    private CustomInitializeCallbackV2 InitializeV2;
    private CustomEditCallback Edit;
    private CustomSpawnCallback Spawn;
    private CustomSpawnCallbackV2 SpawnV2;
    public CustomCheckSpaceCallback CheckSpace;
    public CustomKillCallback Kill;
    public CustomUnloadCallback Unload;
    private CustomUpdateCallback Update;
    private CustomUpdateCallbackV2 UpdateV2;
    private CustomDisplayCallback Display;
    private CustomDisplayCallbackV2 DisplayV2;
    public bool IsValid;
    public List<CustomAddonAdapter> AdapterUsers;
    public List<CustomAddonSpawnPointInstance> SpawnPointInstances;
    public bool SupportsEditing { get; set; }
    public Dictionary<string, object> ToApiResult(ProfileStore profileStore);
    public bool Validate();
    public bool TryInitialize(BasePlayer player, string[] args, object data);
    public bool TryEdit(BasePlayer player, string[] args, Component component, JObject data, object newData);
    public Component DoSpawn(Guid guid, Component monument, Vector3 position, Quaternion rotation, JObject jObject);
    public Component DoUpdate(Component component, JObject data);
    public void SetData(ProfileStore profileStore, CustomAddonController controller, object data);
    public void SetData(ProfileStore profileStore, Component component, object data);
    public void DoDisplay(Component component, JObject data, BasePlayer player, StringBuilder sb, float duration);
}

private class CustomAddonManager
{
    private MonumentAddons _plugin;
    private Dictionary<string, CustomAddonDefinition> _customAddonsByName;
    private Dictionary<string, List<CustomAddonDefinition>> _customAddonsByPlugin;
    public IEnumerable<CustomAddonDefinition> GetAllAddons();
    public CustomAddonManager(MonumentAddons plugin);
    public bool IsRegistered(string addonName, Plugin otherPlugin);
    public void RegisterAddon(CustomAddonDefinition addonDefinition);
    public void UnregisterAllForPlugin(Plugin plugin);
    public CustomAddonDefinition GetAddon(string addonName);
    private List<CustomAddonDefinition> GetAddonsForPlugin(Plugin plugin);
    private IEnumerator DestroyControllersRoutine(ICollection<CustomAddonController> controllerList);
}

private class CustomAddonAdapter : TransformAdapter
{
    public CustomAddonData CustomAddonData { get; set; }
    public CustomAddonDefinition AddonDefinition { get; set; }
    public override Component Component { get; set; }
    public override Transform Transform { get; set; }
    public override Vector3 Position { get; set; }
    public override Quaternion Rotation { get; set; }
    public override bool IsValid { get; set; }
    private Component _component;
    private Transform _transform;
    private bool _wasKilled;
    public CustomAddonAdapter(CustomAddonData customAddonData, BaseController controller, BaseMonument monument, CustomAddonDefinition addonDefinition);
    public override void Spawn();
    public override void PreUnload();
    public override void Kill();
    public override void OnComponentDestroyed(Component component);
    public void SetupComponent(Component component);
    public void HandleChanges();
    private void UpdateViaOwnerPlugin();
    private void UpdatePosition();
}

private class CustomAddonController : BaseController, IUpdateableController
{
    public CustomAddonData CustomAddonData { get; set; }
    private CustomAddonDefinition _addonDefinition;
    public CustomAddonController(ProfileController profileController, CustomAddonData customAddonData, CustomAddonDefinition addonDefinition);
    public override BaseAdapter CreateAdapter(BaseMonument monument);
    public Coroutine StartUpdateRoutine();
    private IEnumerator UpdateRoutine();
}

private class EntityControllerFactory
{
    public virtual bool AppliesToEntity(BaseEntity entity);
    public virtual EntityController CreateController(ProfileController controller, EntityData entityData);
}

private class SignControllerFactory : EntityControllerFactory
{
    public override bool AppliesToEntity(BaseEntity entity);
    public override EntityController CreateController(ProfileController controller, EntityData entityData);
}

private class CCTVControllerFactory : EntityControllerFactory
{
    public override bool AppliesToEntity(BaseEntity entity);
    public override EntityController CreateController(ProfileController controller, EntityData entityData);
}

private class ControllerFactory
{
    private MonumentAddons _plugin;
    public ControllerFactory(MonumentAddons plugin);
    private EntityControllerFactory[] _entityFactories;
    public BaseController CreateController(ProfileController profileController, BaseData data);
    private EntityControllerFactory ResolveEntityFactory(EntityData entityData);
}

private class IOManager
{
    private const int FreePowerAmount;
    private static bool HasInput(IOEntity ioEntity, IOType ioType);
    private static bool HasConnectedInput(IOEntity ioEntity, int inputSlot);
    private readonly int[] _defaultInputSlots;
    private readonly Type[] _dontPowerPrefabsOfType;
    private readonly Dictionary<string, int[]> _inputSlotsByPrefabName;
    private readonly Dictionary<uint, int[]> _inputSlotsByPrefabId;
    private List<uint> _dontPowerPrefabIds;
    public void OnServerInitialized();
    public bool MaybeProvidePower(IOEntity ioEntity);
    private int[] DeterminePowerInputSlots(IOEntity ioEntity);
}

private class AdapterDisplayManager
{
    private MonumentAddons _plugin;
    private UniqueNameRegistry _uniqueNameRegistry;
    private Configuration _config { get; set; }
    public const int HeaderSize;
    public static readonly string Divider;
    public static readonly Vector3 ArrowVerticalOffeset;
    private const float DisplayIntervalDurationFast;
    private const float DisplayIntervalDuration;
    private class PlayerInfo
    {
        public Timer Timer;
        public ProfileController ProfileController;
        public TransformAdapter MovingAdapter;
        public CustomMonument MovingCustomMonument;
        public RealTimeSince RealTimeSinceShown;
        public float DisplayDurationSlow { get; set; }
        public float DisplayDurationFast { get; set; }
        public float GetDisplayDuration(BaseAdapter adapter);
        public float GetDisplayDuration(CustomMonument monument);
    }

    private float DefaultDisplayDuration { get; set; }
    private float DisplayDistanceSquared { get; set; }
    private float DisplayDistanceAbbreviatedSquared { get; set; }
    private StringBuilder _sb;
    private Dictionary<ulong, PlayerInfo> _playerInfo;
    public AdapterDisplayManager(MonumentAddons plugin, UniqueNameRegistry uniqueNameRegistry);
    public void SetPlayerProfile(BasePlayer player, ProfileController profileController);
    public void SetPlayerMovingAdapter(BasePlayer player, TransformAdapter adapter);
    public void ShowAllRepeatedly(BasePlayer player, float? duration, bool immediate);
    private Color DetermineColor(BaseAdapter adapter, PlayerInfo playerInfo, ProfileController profileController);
    private void AddCommonInfo(BasePlayer player, ProfileController profileController, BaseController controller, BaseAdapter adapter);
    private void ShowPuzzleInfo(BasePlayer player, EntityAdapter entityAdapter, PuzzleReset puzzleReset, Vector3 playerPosition, PlayerInfo playerInfo);
    private Ddraw CreateDrawer(BasePlayer player, BaseAdapter adapter, PlayerInfo playerInfo);
    private void ShowEntityInfo(Ddraw drawer, BasePlayer player, EntityAdapter adapter, Vector3 playerPosition, PlayerInfo playerInfo);
    private void ShowPrefabInfo(Ddraw drawer, BasePlayer player, PrefabAdapter adapter, PlayerInfo playerInfo);
    private void ShowSpawnPointInfo(BasePlayer player, SpawnPointAdapter adapter, SpawnGroupAdapter spawnGroupAdapter, PlayerInfo playerInfo, bool showGroupInfo);
    private void ShowPasteInfo(Ddraw drawer, BasePlayer player, PasteAdapter adapter);
    private void ShowCustomAddonInfo(Ddraw drawer, BasePlayer player, CustomAddonAdapter adapter);
    private SpawnPointAdapter FindClosestSpawnPointAdapter(SpawnGroupAdapter spawnGroupAdapter, Vector3 origin);
    private SpawnPointAdapter FindClosestSpawnPointAdapterToRay(SpawnGroupAdapter spawnGroupAdapter, Ray ray);
    private Vector3 GetClosestAdapterPosition(BaseAdapter adapter, Ray ray, BaseAdapter closestAdapter);
    private static bool IsWithinDistanceSquared(Vector3 position1, Vector3 position2, float distanceSquared);
    private static bool IsWithinDistanceSquared(TransformAdapter adapter, Vector3 position, float distanceSquared);
    private static void DrawAbbreviation(Ddraw drawer, TransformAdapter adapter);
    private void DisplayMonument(BasePlayer player, PlayerInfo playerInfo, CustomMonument monument);
    private void ShowNearbyCustomMonuments(BasePlayer player, Vector3 playerPosition, PlayerInfo playerInfo);
    private void DisplayAdapter(BasePlayer player, Vector3 playerPosition, PlayerInfo playerInfo, BaseAdapter adapter, BaseAdapter closestAdapter, float distanceSquared, int remainingToShow);
    private void ShowNearbyAdapters(BasePlayer player, Vector3 playerPosition, PlayerInfo playerInfo);
    private PlayerInfo GetOrCreatePlayerInfo(BasePlayer player);
}

private class PlayerInfo
{
    public Timer Timer;
    public ProfileController ProfileController;
    public TransformAdapter MovingAdapter;
    public CustomMonument MovingCustomMonument;
    public RealTimeSince RealTimeSinceShown;
    public float DisplayDurationSlow { get; set; }
    public float DisplayDurationFast { get; set; }
    public float GetDisplayDuration(BaseAdapter adapter);
    public float GetDisplayDuration(CustomMonument monument);
}

private class ProfileController
{
    public MonumentAddons Plugin { get; set; }
    public Profile Profile { get; set; }
    public ProfileStatus ProfileStatus { get; set; }
    public WaitUntil WaitUntilLoaded;
    public WaitUntil WaitUntilUnloaded;
    private Configuration _config { get; set; }
    private StoredData _pluginData { get; set; }
    private ProfileManager _profileManager { get; set; }
    private ProfileStateData _profileStateData { get; set; }
    private CoroutineManager _coroutineManager;
    private Dictionary<BaseData, BaseController> _controllersByData;
    private Queue<SpawnQueueItem> _spawnQueue;
    public bool IsEnabled { get; set; }
    public ProfileController(MonumentAddons plugin, Profile profile, bool startLoaded);
    public void OnControllerKilled(BaseController controller);
    public Coroutine StartCoroutine(IEnumerator enumerator);
    public Coroutine StartCallbackRoutine(Coroutine coroutine, Action callback);
    public IEnumerable<T> GetControllers();
    public IEnumerable<T> GetAdapters();
    public void Load(ProfileCounts profileCounts);
    public void PreUnload();
    public void DetachSavedEntities();
    public void Unload(IEnumerator cleanupRoutine);
    public void Reload(Profile newProfileData);
    public IEnumerator PartialLoadForLateMonument(ICollection<BaseData> dataList, BaseMonument monument);
    public void SpawnNewData(BaseData data, ICollection<BaseMonument> monumentList);
    public void Rename(string newName);
    public void Enable(Profile newProfileData);
    public void Disable();
    public void Clear();
    public BaseController FindControllerById(Guid guid);
    public BaseAdapter FindAdapter(Guid guid, BaseMonument monument);
    public T FindEntity(Guid guid, BaseMonument monument);
    public void SetupIO();
    private void Interrupt();
    private BaseController GetController(BaseData data);
    private BaseController EnsureController(BaseData data);
    private void Enqueue(SpawnQueueItem queueItem);
    private void EnqueueAll(ProfileCounts profileCounts);
    private void SetProfileStatus(ProfileStatus newStatus, bool broadcast);
    private IEnumerator ProcessSpawnQueue();
    private IEnumerator UnloadRoutine(IEnumerator cleanupRoutine);
    private IEnumerator ReloadRoutine(Profile newProfileData);
    private IEnumerator ClearRoutine();
    private IEnumerator CleanEntitiesRoutine(ProfileState profileState, List<BaseEntity> entitiesToKill);
    private void CleanOrphanedEntities();
}

private class ProfileCounts
{
    public int EntityCount;
    public int PrefabCount;
    public int SpawnPointCount;
    public int PasteCount;
}

private class ProfileInfo
{
    public static List<ProfileInfo> GetList(StoredData pluginData, ProfileManager profileManager);
    public string Name;
    public bool Enabled;
    public Profile Profile;
}

private class ProfileManager
{
    private readonly MonumentAddons _plugin;
    private OriginalProfileStore _originalProfileStore;
    private readonly ProfileStore _profileStore;
    private List<ProfileController> _profileControllers;
    private Configuration _config { get; set; }
    private StoredData _pluginData { get; set; }
    public bool HasAnyEnabledDynamicMonuments { get; set; }
    public ProfileManager(MonumentAddons plugin, OriginalProfileStore originalProfileStore, ProfileStore profileStore);
    public void BroadcastProfileStateChanged(ProfileController profileController, ProfileStatus status, ProfileStatus previousStatus);
    public bool HasDynamicMonument(BaseEntity entity);
    public IEnumerator LoadAllProfilesRoutine();
    public void UnloadAllProfiles();
    public IEnumerator PartialLoadForLateMonumentRoutine(BaseMonument monument);
    public ProfileController GetCachedProfileController(string profileName);
    public ProfileController GetProfileController(string profileName);
    public ProfileController GetPlayerProfileController(string userId);
    public ProfileController GetPlayerProfileControllerOrDefault(string userId);
    public bool ProfileExists(string profileName);
    public ProfileController CreateProfile(string profileName, string authorName);
    public void DisableProfile(ProfileController profileController);
    public void DeleteProfile(ProfileController profileController);
    public IEnumerable<ProfileController> GetEnabledProfileControllers();
    public IEnumerable<T> GetEnabledControllers();
    public IEnumerable<T> GetEnabledAdapters();
    public IEnumerable<T> GetEnabledAdaptersForMonument(BaseMonument monument);
    private IEnumerator UnloadAllProfilesRoutine();
}

private abstract class BaseData
{
    [JsonProperty("Id", Order = -10)]
    public Guid Id;
}

private abstract class BaseTransformData : BaseData
{
    [JsonProperty("SnapToTerrain", Order = -4, DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool SnapToTerrain;
    [JsonProperty("Position", Order = -3)]
    public Vector3 Position;
    [JsonProperty("RotationAngle", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public float DeprecatedRotationAngle { get; set; }
    [JsonProperty("RotationAngles", Order = -2, DefaultValueHandling = DefaultValueHandling.Ignore)]
    public Vector3 RotationAngles;
    [JsonProperty("OnTerrain")]
    public bool DepredcatedOnTerrain { get; set; }
}

private class PrefabData : BaseTransformData
{
    [JsonProperty("PrefabName", Order = -5)]
    public string PrefabName;
}

private class BuildingBlockInfo
{
    [JsonProperty("Grade")]
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(BuildingGrade.Enum.None)]
    public BuildingGrade.Enum Grade;
}

private class CCTVInfo
{
    [JsonProperty("RCIdentifier", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string RCIdentifier;
    [JsonProperty("Pitch", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public float Pitch;
    [JsonProperty("Yaw", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public float Yaw;
}

private class SignArtistImage
{
    [JsonProperty("Url")]
    public string Url;
    [JsonProperty("Raw", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool Raw;
}

private class IOConnectionData
{
    [JsonProperty("ConnectedToId")]
    public Guid ConnectedToId;
    [JsonProperty("Slot")]
    public int Slot;
    [JsonProperty("ConnectedToSlot")]
    public int ConnectedToSlot;
    [JsonProperty("ShowWire")]
    public bool ShowWire;
    [JsonProperty("Color", DefaultValueHandling = DefaultValueHandling.Ignore)]
    [JsonConverter(typeof(StringEnumConverter))]
    public WireColour Color;
    [JsonProperty("Points")]
    public Vector3[] Points;
}

private class IOEntityData
{
    [JsonProperty("Outputs")]
    public List<IOConnectionData> Outputs;
    public IOConnectionData FindConnection(int slot);
}

private class PuzzleData
{
    [JsonProperty("PlayersBlockReset")]
    public bool PlayersBlockReset;
    [JsonProperty("PlayerDetectionRadius")]
    public float PlayerDetectionRadius;
    [JsonProperty("SecondsBetweenResets")]
    public float SecondsBetweenResets;
    [JsonProperty("SpawnGroupIds", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public List<Guid> SpawnGroupIds;
    public bool ShouldSerializeSpawnGroupIds();
    public bool HasSpawnGroupId(Guid spawnGroupId);
    public void AddSpawnGroupId(Guid spawnGroupId);
    public void RemoveSpawnGroupId(Guid spawnGroupId);
}

private class HeadData
{
    private FieldInfo CurrentTrophyDataField;
    public static HeadData FromHeadEntity(HeadEntity headEntity);
    public class BasicItemData
    {
        public readonly int ItemId;
        public BasicItemData(int itemId);
    }

    [JsonProperty("EntitySource")]
    public uint EntitySource;
    [JsonProperty("HorseBreed", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public int HorseBreed;
    [JsonProperty("PlayerId", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public ulong PlayerId;
    [JsonProperty("PlayerName", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string PlayerName;
    [JsonProperty("Clothing", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public BasicItemData[] Clothing;
    public void ApplyToHuntingTrophy(HuntingTrophy huntingTrophy);
}

public class BasicItemData
{
    public readonly int ItemId;
    public BasicItemData(int itemId);
}

private class EntityData : BaseTransformData
{
    [JsonProperty("PrefabName", Order = -5)]
    public string PrefabName;
    [JsonProperty("Skin", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public ulong Skin;
    [JsonProperty("EnabledFlags", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public BaseEntity.Flags EnabledFlags;
    [JsonProperty("DisabledFlags", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public BaseEntity.Flags DisabledFlags;
    [JsonProperty("Puzzle", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public PuzzleData Puzzle;
    [JsonProperty("Scale", DefaultValueHandling = DefaultValueHandling.Ignore)]
    [DefaultValue(1f)]
    public float Scale;
    [JsonProperty("CardReaderLevel", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public ushort CardReaderLevel;
    [JsonProperty("BuildingBlock", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public BuildingBlockInfo BuildingBlock;
    [JsonProperty("CCTV", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public CCTVInfo CCTV;
    [JsonProperty("SignArtistImages", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public SignArtistImage[] SignArtistImages;
    [JsonProperty("VendingProfile", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public object VendingProfile;
    [JsonProperty("IOEntity", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public IOEntityData IOEntityData;
    [JsonProperty("SkullName", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string SkullName;
    [JsonProperty("HeadData", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public HeadData HeadData;
    public void SetFlag(BaseEntity.Flags flag, bool? value);
    public bool? HasFlag(BaseEntity.Flags flag);
    public void RemoveIOConnection(int slot);
    public void AddIOConnection(IOConnectionData connectionData);
    public PuzzleData EnsurePuzzleData(PuzzleReset puzzleReset);
}

private class SpawnPointData : BaseTransformData
{
    [JsonProperty("Exclusive", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool Exclusive;
    [JsonProperty("SnapToGround", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool SnapToGround;
    [JsonProperty("DropToGround")]
    public bool DeprecatedDropToGround { get; set; }
    [JsonProperty("CheckSpace", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool CheckSpace;
    [JsonProperty("RandomRotation", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool RandomRotation;
    [JsonProperty("RandomRadius", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public float RandomRadius;
    [JsonProperty("PlayerDetectionRadius", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public float PlayerDetectionRadius;
}

private class WeightedPrefabData
{
    [JsonProperty("PrefabName", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string PrefabName;
    [JsonProperty("CustomAddonName", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string CustomAddonName;
    [JsonProperty("Weight")]
    public int Weight;
}

private class SpawnGroupData : BaseData
{
    [JsonProperty("Name")]
    public string Name;
    [JsonProperty("Color", DefaultValueHandling = DefaultValueHandling.Ignore)]
    [JsonConverter(typeof(HtmlColorConverter))]
    public Color? Color;
    [JsonProperty("MaxPopulation")]
    public int MaxPopulation;
    [JsonProperty("SpawnPerTickMin")]
    public int SpawnPerTickMin;
    [JsonProperty("SpawnPerTickMax")]
    public int SpawnPerTickMax;
    [JsonProperty("RespawnDelayMin")]
    public float RespawnDelayMin;
    [JsonProperty("RespawnDelayMax")]
    public float RespawnDelayMax;
    [JsonProperty("InitialSpawn")]
    public bool InitialSpawn;
    [JsonProperty("PreventDuplicates", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool PreventDuplicates;
    [JsonProperty("PauseScheduleWhileFull", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool PauseScheduleWhileFull;
    [JsonProperty("RespawnWhenNearestPuzzleResets", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool RespawnWhenNearestPuzzleResets;
    [JsonProperty("Prefabs")]
    public List<WeightedPrefabData> Prefabs;
    [JsonProperty("SpawnPoints")]
    public List<SpawnPointData> SpawnPoints;
    [JsonIgnore]
    public int TotalWeight { get; set; }
    public List<WeightedPrefabData> FindCustomAddonMatches(string partialName);
    public List<WeightedPrefabData> FindPrefabMatches(string partialName, UniqueNameRegistry uniqueNameRegistry);
}

private class PasteData : BaseTransformData
{
    [JsonProperty("Filename")]
    public string Filename;
    [JsonProperty("Args")]
    public string[] Args;
}

private class CustomAddonData : BaseTransformData
{
    [JsonProperty("AddonName")]
    public string AddonName;
    [JsonProperty("PluginData", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private object PluginData;
    public JObject GetSerializedData();
    public CustomAddonData SetData(object data);
}

private class ProfileSummaryEntry
{
    public string MonumentName;
    public string AddonType;
    public string AddonName;
    public int Count;
}

private class MonumentData
{
    [JsonProperty("Entities")]
    public List<EntityData> Entities;
    public bool ShouldSerializeEntities();
    [JsonProperty("Prefabs")]
    public List<PrefabData> Prefabs;
    public bool ShouldSerializePrefabs();
    [JsonProperty("SpawnGroups")]
    public List<SpawnGroupData> SpawnGroups;
    public bool ShouldSerializeSpawnGroups();
    [JsonProperty("Pastes")]
    public List<PasteData> Pastes;
    public bool ShouldSerializePastes();
    [JsonProperty("CustomAddons")]
    public List<CustomAddonData> CustomAddons;
    public bool ShouldSerializeCustomAddons();
    [JsonIgnore]
    public int NumSpawnables { get; set; }
    [JsonIgnore]
    public int NumSpawnPoints { get; set; }
    public IEnumerable<BaseData> GetSpawnablesLazy();
    public ICollection<BaseData> GetSpawnables();
    public bool HasEntity(Guid guid);
    public bool HasSpawnGroup(Guid guid);
    public void AddData(BaseData data);
    public bool RemoveData(BaseData data);
    private void CleanSpawnGroupReferences(Guid id);
}

private static class ProfileDataMigration
{
    private static readonly Dictionary<string, string> _monumentNameCorrections;
    private static readonly Dictionary<string, string> _prefabCorrections;
    private static string GetPrefabCorrectionIfExists(string prefabName);
    public static bool MigrateToLatest(T data);
    public static bool MigrateV0ToV1(T data);
    public static bool MigrateV1ToV2(T data);
    public static bool MigrateIncorrectPrefabs(T data);
    public static bool MigrateIncorrectMonuments(T data);
}

private class FileStore
{
    protected string _directoryPath;
    public FileStore(string directoryPath);
    public virtual bool Exists(string filename);
    public virtual T Load(string filename);
    public T LoadIfExists(string filename);
    public void Save(string filename, T data);
    public void Delete(string filename);
    protected virtual string GetFilepath(string filename);
}

private class OriginalProfileStore : FileStore<Profile>
{
    public const string OriginalSuffix;
    public static bool IsOriginalProfile(string profileName);
    public OriginalProfileStore();
    protected override string GetFilepath(string profileName);
    public void Save(Profile profile);
    public void MoveTo(Profile profile, string newName);
}

private class ProfileStore : FileStore<Profile>
{
    public static string[] GetProfileNames();
    public ProfileStore();
    public override bool Exists(string profileName);
    public override Profile Load(string profileName);
    public bool TryLoad(string profileName, Profile profile, string errorMessage);
    public void Save(Profile profile);
    public Profile Create(string profileName, string authorName);
    public void MoveTo(Profile profile, string newName);
    public void EnsureDefaultProfile();
    private string GetCaseSensitiveFileName(string profileName);
}

private class Profile
{
    [JsonProperty("Name")]
    public string Name;
    [JsonProperty("Author", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string Author;
    [JsonProperty("SchemaVersion", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public float SchemaVersion;
    [JsonProperty("Url", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string Url;
    [JsonProperty("Monuments", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public Dictionary<string, List<EntityData>> DeprecatedMonumentMap;
    [JsonProperty("MonumentData")]
    public Dictionary<string, MonumentData> MonumentDataMap;
    private HashSet<uint> _dynamicMonumentPrefabIds;
    private bool _hasDeterminedDynamicMonuments;
    [JsonIgnore]
    public bool HasAnyDynamicMonuments { get; set; }
    [OnDeserialized]
    private void OnDeserialized(StreamingContext context);
    public bool IsEmpty();
    public bool HasEntity(string monumentUniqueName, Guid guid);
    public bool HasEntity(string monumentUniqueName, EntityData entityData);
    public bool HasSpawnGroup(string monumentUniqueName, Guid guid);
    public void AddData(string monumentUniqueName, BaseData data);
    public bool RemoveData(BaseData data, string monumentUniqueName);
    public bool HasDynamicMonument(BaseEntity entity);
    private MonumentData EnsureMonumentData(string monumentUniqueName);
    private void DetermineDynamicMonuments();
}

private static class DataFileUtils
{
    public static bool Exists(string filepath);
    public static T Load(string filepath);
    public static T LoadIfExists(string filepath);
    public static T LoadOrNew(string filepath);
    public static void Save(string filepath, T data);
}

private class BaseDataFile
{
    private string _filepath;
    public BaseDataFile(string filepath);
    public void Save();
}

private class MonumentState : IDeepCollection
{
    [JsonProperty("Entities")]
    public Dictionary<Guid, ulong> Entities;
    public bool HasItems();
    public bool HasEntity(Guid guid, NetworkableId entityId);
    public BaseEntity FindEntity(Guid guid);
    public void AddEntity(Guid guid, NetworkableId entityId);
    public bool RemoveEntity(Guid guid);
    public IEnumerable<ValueTuple<Guid, BaseEntity>> FindValidEntities();
    public int CleanStaleEntityRecords();
}

private class MonumentStateMapConverter : DictionaryKeyConverter<Vector3, MonumentState>
{
    public override string KeyToString(Vector3 v);
    public override Vector3 KeyFromString(string key);
}

private class Vector3EqualityComparer : IEqualityComparer<Vector3>
{
    public bool Equals(Vector3 a, Vector3 b);
    public int GetHashCode(Vector3 vector);
}

private class MonumentStateMap : IDeepCollection
{
    [JsonProperty("ByLocation")]
    [JsonConverter(typeof(MonumentStateMapConverter))]
    private Dictionary<Vector3, MonumentState> ByLocation;
    public bool ShouldSerializeByLocation();
    [JsonProperty("ByEntity")]
    private Dictionary<ulong, MonumentState> ByEntity;
    public bool ShouldSerializeByEntity();
    public bool HasItems();
    public IEnumerable<ValueTuple<Guid, BaseEntity>> FindValidEntities();
    public int CleanStaleEntityRecords();
    public MonumentState GetMonumentState(BaseMonument monument);
    public MonumentState GetOrCreateMonumentState(BaseMonument monument);
}

private class ProfileState : Dictionary<string, MonumentStateMap>, IDeepCollection
{
    [OnDeserialized]
    private void OnDeserialized(StreamingContext context);
    public bool HasItems();
    public int CleanStaleEntityRecords();
    public IEnumerable<MonumentEntityEntry> FindValidEntities();
}

private class ProfileStateMap : Dictionary<string, ProfileState>, IDeepCollection
{
    public ProfileStateMap();
    public bool HasItems();
}

private class ProfileStateData : BaseDataFile
{
    private static string Filepath { get; set; }
    public static ProfileStateData Load(StoredData pluginData);
    private StoredData _pluginData;
    public ProfileStateData();
    [JsonProperty("ProfileState", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private ProfileStateMap ProfileStateMap;
    public bool ShouldSerializeProfileStateMap();
    public ProfileState GetProfileState(string profileName);
    public bool HasEntity(string profileName, BaseMonument monument, Guid guid, NetworkableId entityId);
    public BaseEntity FindEntity(string profileName, BaseMonument monument, Guid guid);
    public void AddEntity(string profileName, BaseMonument monument, Guid guid, NetworkableId entityId);
    public bool RemoveEntity(string profileName, BaseMonument monument, Guid guid);
    public List<BaseEntity> FindAndRemoveValidEntities(string profileName);
    public List<BaseEntity> CleanDisabledProfileState();
    public void Reset();
}

private static class StoredDataMigration
{
    private static readonly Dictionary<string, string> MigrateMonumentNames;
    public static bool MigrateToLatest(ProfileStore profileStore, StoredData data);
    public static bool MigrateV0ToV1(StoredData data);
    public static bool MigrateV1ToV2(ProfileStore profileStore, StoredData data);
}

private class StoredData
{
    public static StoredData Load(ProfileStore profileStore);
    [JsonProperty("DataFileVersion", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public float DataFileVersion;
    [JsonProperty("EnabledProfiles")]
    public HashSet<string> EnabledProfiles;
    [JsonProperty("SelectedProfiles")]
    public Dictionary<string, string> SelectedProfiles;
    [JsonProperty("Monuments", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public Dictionary<string, List<EntityData>> DeprecatedMonumentMap;
    public void Save();
    public bool IsProfileEnabled(string profileName);
    public void SetProfileEnabled(string profileName);
    public void SetProfileDisabled(string profileName);
    public void RenameProfileReferences(string oldName, string newName);
    public string GetSelectedProfileName(string userId);
    public void SetProfileSelected(string userId, string profileName);
}

[JsonObject(MemberSerialization.OptIn)]
private class DebugDisplaySettings
{
    [JsonProperty("Default display duration (seconds)")]
    public float DefaultDisplayDuration;
    [JsonProperty("Display distance")]
    public float DisplayDistance;
    [JsonProperty("Display distance abbreviated")]
    public float DisplayDistanceAbbreviated;
    [JsonProperty("Max addons to show unabbreviated")]
    public int MaxAddonsToShowUnabbreviated;
    [JsonProperty("Entity color")]
    [JsonConverter(typeof(HtmlColorConverter))]
    public Color EntityColor;
    [JsonProperty("Spawn point color")]
    [JsonConverter(typeof(HtmlColorConverter))]
    public Color SpawnPointColor;
    [JsonProperty("Paste color")]
    [JsonConverter(typeof(HtmlColorConverter))]
    public Color PasteColor;
    [JsonProperty("Custom addon color")]
    [JsonConverter(typeof(HtmlColorConverter))]
    public Color CustomAddonColor;
    [JsonProperty("Custom monument color")]
    [JsonConverter(typeof(HtmlColorConverter))]
    public Color CustomMonumentColor;
    [JsonProperty("Inactive profile color")]
    [JsonConverter(typeof(HtmlColorConverter))]
    public Color InactiveProfileColor;
}

[JsonObject(MemberSerialization.OptIn)]
private class EntitySaveSettings
{
    [JsonProperty("Enable saving for storage entities")]
    public bool EnabledForStorageEntities;
    [JsonProperty("Enable saving for non-storage entities")]
    public bool EnabledForNonStorageEntities;
    [JsonProperty("Override saving enabled by prefab")]
    private Dictionary<string, bool> OverrideEnabledByPrefab;
    private Dictionary<uint, bool> _overrideEnabledByPrefabId;
    public void Init();
    public bool ShouldEnableSaving(BaseEntity entity);
}

[JsonObject(MemberSerialization.OptIn)]
private class DynamicMonumentSettings
{
    [JsonProperty("Entity prefabs to consider as monuments")]
    public string[] DynamicMonumentPrefabs;
    [JsonIgnore]
    private uint[] _dynamicMonumentPrefabIds;
    public void Init();
    public bool IsConfiguredAsDynamicMonument(BaseEntity entity);
}

[JsonObject(MemberSerialization.OptIn)]
private class SpawnGroupDefaults
{
    [JsonProperty(nameof(SpawnGroupOption.MaxPopulation))]
    private int MaxPopulation;
    [JsonProperty(nameof(SpawnGroupOption.SpawnPerTickMin))]
    private int SpawnPerTickMin;
    [JsonProperty(nameof(SpawnGroupOption.SpawnPerTickMax))]
    private int SpawnPerTickMax;
    [JsonProperty(nameof(SpawnGroupOption.RespawnDelayMin))]
    private float RespawnDelayMin;
    [JsonProperty(nameof(SpawnGroupOption.RespawnDelayMax))]
    private float RespawnDelayMax;
    [JsonProperty(nameof(SpawnGroupOption.InitialSpawn))]
    private bool InitialSpawn;
    [JsonProperty(nameof(SpawnGroupOption.PreventDuplicates))]
    private bool PreventDuplicates;
    [JsonProperty(nameof(SpawnGroupOption.PauseScheduleWhileFull))]
    private bool PauseScheduleWhileFull;
    [JsonProperty(nameof(SpawnGroupOption.RespawnWhenNearestPuzzleResets))]
    private bool RespawnWhenNearestPuzzleResets;
    public SpawnGroupData ApplyTo(SpawnGroupData spawnGroupData);
}

[JsonObject(MemberSerialization.OptIn)]
private class SpawnPointDefaults
{
    [JsonProperty(nameof(SpawnPointOption.Exclusive))]
    private bool Exclusive;
    [JsonProperty(nameof(SpawnPointOption.SnapToGround))]
    private bool SnapToGround;
    [JsonProperty(nameof(SpawnPointOption.CheckSpace))]
    private bool CheckSpace;
    [JsonProperty(nameof(SpawnPointOption.RandomRotation))]
    private bool RandomRotation;
    [JsonProperty(nameof(SpawnPointOption.RandomRadius))]
    private float RandomRadius;
    [JsonProperty(nameof(SpawnPointOption.PlayerDetectionRadius))]
    private float PlayerDetectionRadius;
    public SpawnPointData ApplyTo(SpawnPointData spawnPointData);
}

[JsonObject(MemberSerialization.OptIn)]
private class PuzzleDefaults
{
    [JsonProperty(nameof(PuzzleOption.PlayersBlockReset))]
    public bool PlayersBlockReset;
    [JsonProperty(nameof(PuzzleOption.PlayerDetectionRadius))]
    public float PlayerDetectionRadius;
    [JsonProperty(nameof(PuzzleOption.SecondsBetweenResets))]
    public float SecondsBetweenResets;
    public PuzzleData ApplyTo(PuzzleData puzzleData);
}

[JsonObject(MemberSerialization.OptIn)]
private class AddonDefaults
{
    [JsonProperty("Spawn group defaults")]
    public SpawnGroupDefaults SpawnGroups;
    [JsonProperty("Spawn point defaults")]
    public SpawnPointDefaults SpawnPoints;
    [JsonProperty("Puzzle defaults")]
    public PuzzleDefaults Puzzles;
}

[JsonObject(MemberSerialization.OptIn)]
private class Configuration : BaseConfiguration
{
    [JsonProperty("Debug", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool Debug;
    [JsonProperty("Debug display settings")]
    public DebugDisplaySettings DebugDisplaySettings;
    [JsonProperty("Debug display distance")]
    private float DeprecatedDebugDisplayDistance { get; set; }
    [JsonProperty("Save entities between restarts/reloads to preserve their state throughout a wipe")]
    public EntitySaveSettings EntitySaveSettings;
    [JsonProperty("Persist entities while the plugin is unloaded")]
    private bool DeprecatedEnableEntitySaving { get; set; }
    [JsonProperty("Dynamic monuments")]
    public DynamicMonumentSettings DynamicMonuments;
    [JsonProperty("Addon defaults")]
    public AddonDefaults AddonDefaults;
    [JsonProperty("Deployable overrides")]
    public Dictionary<string, string> DeployableOverrides;
    [JsonProperty("Xmas tree decorations (item shortnames)")]
    public string[] XmasTreeDecorations;
    public void Init();
}

private class BaseConfiguration
{
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private static class JsonHelper
{
    public static object Deserialize(string json);
    private static object ToObject(JToken token);
}

private class LangEntry
{
    public static List<LangEntry> AllLangEntries;
    public static readonly LangEntry0 NotApplicable;
    public static readonly LangEntry0 ErrorNoPermission;
    public static readonly LangEntry0 ErrorMonumentFinderNotLoaded;
    public static readonly LangEntry0 ErrorNoMonuments;
    public static readonly LangEntry2 ErrorNotAtMonument;
    public static readonly LangEntry0 ErrorNoSuitableAddonFound;
    public static readonly LangEntry0 ErrorNoCustomAddonFound;
    public static readonly LangEntry0 ErrorEntityNotEligible;
    public static readonly LangEntry0 ErrorNoSpawnPointFound;
    public static readonly LangEntry2 ErrorSetSyntaxGeneric;
    public static readonly LangEntry2 ErrorSetSyntax;
    public static readonly LangEntry1 ErrorSetUnknownOption;
    public static readonly LangEntry0 WarningRecommendSpawnPoint;
    public static readonly LangEntry0 SpawnErrorSyntax;
    public static readonly LangEntry0 SpawnErrorNoProfileSelected;
    public static readonly LangEntry1 SpawnErrorEntityNotFound;
    public static readonly LangEntry1 SpawnErrorEntityOrAddonNotFound;
    public static readonly LangEntry0 SpawnErrorMultipleMatches;
    public static readonly LangEntry0 ErrorNoSurface;
    public static readonly LangEntry3 SpawnSuccess;
    public static readonly LangEntry3 KillSuccess;
    public static readonly LangEntry0 SaveNothingToDo;
    public static readonly LangEntry2 SaveSuccess;
    public static readonly LangEntry0 PrefabErrorSyntax;
    public static readonly LangEntry1 PrefabErrorIsEntity;
    public static readonly LangEntry1 PrefabErrorNotFound;
    public static readonly LangEntry3 PrefabSuccess;
    public static readonly LangEntry0 UndoNotFound;
    public static readonly LangEntry3 UndoKillSuccess;
    public static readonly LangEntry0 EditSynax;
    public static readonly LangEntry1 EditErrorNoMatch;
    public static readonly LangEntry1 EditErrorNotEditable;
    public static readonly LangEntry1 EditSuccess;
    public static readonly LangEntry0 PasteNotCompatible;
    public static readonly LangEntry0 PasteSyntax;
    public static readonly LangEntry1 PasteNotFound;
    public static readonly LangEntry4 PasteSuccess;
    public static readonly LangEntry0 AddonTypeUnknown;
    public static readonly LangEntry0 AddonTypeEntity;
    public static readonly LangEntry0 AddonTypePrefab;
    public static readonly LangEntry0 AddonTypeSpawnPoint;
    public static readonly LangEntry0 AddonTypePaste;
    public static readonly LangEntry0 AddonTypeCustom;
    public static readonly LangEntry1 SpawnGroupCreateSyntax;
    public static readonly LangEntry3 SpawnGroupCreateNameInUse;
    public static readonly LangEntry1 SpawnGroupCreateSucces;
    public static readonly LangEntry3 SpawnGroupSetSuccess;
    public static readonly LangEntry1 SpawnGroupAddSyntax;
    public static readonly LangEntry3 SpawnGroupAddSuccess;
    public static readonly LangEntry1 SpawnGroupRemoveSyntax;
    public static readonly LangEntry2 SpawnGroupRemoveMultipleMatches;
    public static readonly LangEntry2 SpawnGroupRemoveNoMatch;
    public static readonly LangEntry2 SpawnGroupRemoveSuccess;
    public static readonly LangEntry1 SpawnGroupNotFound;
    public static readonly LangEntry1 SpawnGroupMultipeMatches;
    public static readonly LangEntry1 SpawnPointCreateSyntax;
    public static readonly LangEntry1 SpawnPointCreateSuccess;
    public static readonly LangEntry2 SpawnPointSetSuccess;
    public static readonly LangEntry2 SpawnPointSetAllSuccess;
    public static readonly LangEntry0 SpawnGroupHelpHeader;
    public static readonly LangEntry1 SpawnGroupHelpCreate;
    public static readonly LangEntry1 SpawnGroupHelpSet;
    public static readonly LangEntry1 SpawnGroupHelpAdd;
    public static readonly LangEntry1 SpawnGroupHelpRemove;
    public static readonly LangEntry1 SpawnGroupHelpSpawn;
    public static readonly LangEntry1 SpawnGroupHelpRespawn;
    public static readonly LangEntry0 SpawnPointHelpHeader;
    public static readonly LangEntry1 SpawnPointHelpCreate;
    public static readonly LangEntry1 SpawnPointHelpSet;
    public static readonly LangEntry0 SpawnGroupSetHelpName;
    public static readonly LangEntry0 SpawnGroupSetHelpColor;
    public static readonly LangEntry0 SpawnGroupSetHelpMaxPopulation;
    public static readonly LangEntry0 SpawnGroupSetHelpRespawnDelayMin;
    public static readonly LangEntry0 SpawnGroupSetHelpRespawnDelayMax;
    public static readonly LangEntry0 SpawnGroupSetHelpSpawnPerTickMin;
    public static readonly LangEntry0 SpawnGroupSetHelpSpawnPerTickMax;
    public static readonly LangEntry0 SpawnGroupSetHelpInitialSpawn;
    public static readonly LangEntry0 SpawnGroupSetHelpPreventDuplicates;
    public static readonly LangEntry0 SpawnGroupSetHelpPauseScheduleWhileFull;
    public static readonly LangEntry0 SpawnGroupSetHelpRespawnWhenNearestPuzzleResets;
    public static readonly LangEntry0 SpawnPointSetHelpExclusive;
    public static readonly LangEntry0 SpawnPointSetHelpSnapToGround;
    public static readonly LangEntry0 SpawnPointSetHelpCheckSpace;
    public static readonly LangEntry0 SpawnPointSetHelpRandomRotation;
    public static readonly LangEntry0 SpawnPointSetHelpRandomRadius;
    public static readonly LangEntry0 SpawnPointSetHelpPlayerDetectionRadius;
    public static readonly LangEntry1 PuzzleAddSpawnGroupSyntax;
    public static readonly LangEntry1 PuzzleAddSpawnGroupSuccess;
    public static readonly LangEntry1 PuzzleRemoveSpawnGroupSyntax;
    public static readonly LangEntry1 PuzzleRemoveSpawnGroupSuccess;
    public static readonly LangEntry0 PuzzleNotPresent;
    public static readonly LangEntry1 PuzzleNotConnected;
    public static readonly LangEntry0 PuzzleResetSuccess;
    public static readonly LangEntry2 PuzzleSetSuccess;
    public static readonly LangEntry0 PuzzleHelpHeader;
    public static readonly LangEntry1 PuzzleHelpReset;
    public static readonly LangEntry1 PuzzleHelpSet;
    public static readonly LangEntry1 PuzzleHelpAdd;
    public static readonly LangEntry1 PuzzleHelpRemove;
    public static readonly LangEntry0 PuzzleSetHelpMaxPlayersBlockReset;
    public static readonly LangEntry0 PuzzleSetHelpPlayerDetectionRadius;
    public static readonly LangEntry0 PuzzleSetHelpSecondsBetweenResets;
    public static readonly LangEntry1 ShowVanillaNoSpawnPoints;
    public static readonly LangEntry1 GenerateSuccess;
    public static readonly LangEntry1 ShowSuccess;
    public static readonly LangEntry1 ShowLabelPlugin;
    public static readonly LangEntry1 ShowLabelProfile;
    public static readonly LangEntry2 ShowLabelCustomMonument;
    public static readonly LangEntry2 ShowLabelMonument;
    public static readonly LangEntry3 ShowLabelMonumentWithTier;
    public static readonly LangEntry1 ShowLabelSkin;
    public static readonly LangEntry1 ShowLabelScale;
    public static readonly LangEntry1 ShowLabelRCIdentifier;
    public static readonly LangEntry1 ShowHeaderEntity;
    public static readonly LangEntry1 ShowHeaderPrefab;
    public static readonly LangEntry0 ShowHeaderPuzzle;
    public static readonly LangEntry1 ShowHeaderSpawnGroup;
    public static readonly LangEntry1 ShowHeaderVanillaSpawnGroup;
    public static readonly LangEntry1 ShowHeaderSpawnPoint;
    public static readonly LangEntry1 ShowHeaderVanillaSpawnPoint;
    public static readonly LangEntry1 ShowHeaderVanillaIndividualSpawnPoint;
    public static readonly LangEntry1 ShowHeaderPaste;
    public static readonly LangEntry1 ShowHeaderCustom;
    public static readonly LangEntry1 ShowLabelFlags;
    public static readonly LangEntry0 ShowLabelSpawnPointExclusive;
    public static readonly LangEntry0 ShowLabelSpawnPointRandomRotation;
    public static readonly LangEntry0 ShowLabelSpawnPointSnapToGround;
    public static readonly LangEntry0 ShowLabelSpawnPointCheckSpace;
    public static readonly LangEntry1 ShowLabelSpawnPointRandomRadius;
    public static readonly LangEntry1 ShowLabelSpawnPoints;
    public static readonly LangEntry1 ShowLabelTiers;
    public static readonly LangEntry0 ShowLabelSpawnWhenParentSpawns;
    public static readonly LangEntry0 ShowLabelSpawnOnServerStart;
    public static readonly LangEntry0 ShowLabelSpawnOnMapWipe;
    public static readonly LangEntry0 ShowLabelInitialSpawn;
    public static readonly LangEntry0 ShowLabelPreventDuplicates;
    public static readonly LangEntry0 ShowLabelPauseScheduleWhileFull;
    public static readonly LangEntry0 ShowLabelRespawnWhenNearestPuzzleResets;
    public static readonly LangEntry2 ShowLabelPopulation;
    public static readonly LangEntry2 ShowLabelRespawnPerTick;
    public static readonly LangEntry2 ShowLabelRespawnDelay;
    public static readonly LangEntry1 ShowLabelNextSpawn;
    public static readonly LangEntry0 ShowLabelNextSpawnQueued;
    public static readonly LangEntry0 ShowLabelNextSpawnPaused;
    public static readonly LangEntry0 ShowLabelEntities;
    public static readonly LangEntry3 ShowLabelEntityDetail;
    public static readonly LangEntry0 ShowLabelNoEntities;
    public static readonly LangEntry1 ShowLabelPlayerDetectionRadius;
    public static readonly LangEntry0 ShowLabelPlayerDetectedInRadius;
    public static readonly LangEntry1 ShowLabelPuzzlePlayersBlockReset;
    public static readonly LangEntry1 ShowLabelPuzzleTimeBetweenResets;
    public static readonly LangEntry1 ShowLabelPuzzleNextReset;
    public static readonly LangEntry0 ShowLabelPuzzleNextResetOverdue;
    public static readonly LangEntry1 ShowLabelPuzzleSpawnGroups;
    public static readonly LangEntry2 SkinGet;
    public static readonly LangEntry1 SkinSetSyntax;
    public static readonly LangEntry3 SkinSetSuccess;
    public static readonly LangEntry2 SkinErrorRedirect;
    public static readonly LangEntry3 FlagsGet;
    public static readonly LangEntry1 FlagsSetSyntax;
    public static readonly LangEntry1 FlagsEnableSuccess;
    public static readonly LangEntry1 FlagsDisableSuccess;
    public static readonly LangEntry1 FlagsUnsetSuccess;
    public static readonly LangEntry1 CCTVSetIdSyntax;
    public static readonly LangEntry3 CCTVSetIdSuccess;
    public static readonly LangEntry2 CCTVSetDirectionSuccess;
    public static readonly LangEntry1 SkullNameSyntax;
    public static readonly LangEntry3 SkullNameSetSuccess;
    public static readonly LangEntry0 SetHeadNoHeadItem;
    public static readonly LangEntry0 SetHeadMismatch;
    public static readonly LangEntry2 SetHeadSuccess;
    public static readonly LangEntry1 CardReaderSetLevelSyntax;
    public static readonly LangEntry1 CardReaderSetLevelSuccess;
    public static readonly LangEntry0 ProfileListEmpty;
    public static readonly LangEntry0 ProfileListHeader;
    public static readonly LangEntry2 ProfileListItemEnabled;
    public static readonly LangEntry2 ProfileListItemDisabled;
    public static readonly LangEntry2 ProfileListItemSelected;
    public static readonly LangEntry1 ProfileByAuthor;
    public static readonly LangEntry0 ProfileInstallSyntax;
    public static readonly LangEntry0 ProfileInstallShorthandSyntax;
    public static readonly LangEntry1 ProfileUrlInvalid;
    public static readonly LangEntry1 ProfileAlreadyExistsNotEmpty;
    public static readonly LangEntry2 ProfileInstallSuccess;
    public static readonly LangEntry1 ProfileInstallError;
    public static readonly LangEntry2 ProfileDownloadError;
    public static readonly LangEntry2 ProfileParseError;
    public static readonly LangEntry0 ProfileDescribeSyntax;
    public static readonly LangEntry1 ProfileNotFound;
    public static readonly LangEntry1 ProfileEmpty;
    public static readonly LangEntry1 ProfileDescribeHeader;
    public static readonly LangEntry4 ProfileDescribeItem;
    public static readonly LangEntry0 ProfileSelectSyntax;
    public static readonly LangEntry1 ProfileSelectSuccess;
    public static readonly LangEntry1 ProfileSelectEnableSuccess;
    public static readonly LangEntry0 ProfileEnableSyntax;
    public static readonly LangEntry1 ProfileAlreadyEnabled;
    public static readonly LangEntry1 ProfileEnableSuccess;
    public static readonly LangEntry0 ProfileDisableSyntax;
    public static readonly LangEntry1 ProfileAlreadyDisabled;
    public static readonly LangEntry1 ProfileDisableSuccess;
    public static readonly LangEntry0 ProfileReloadSyntax;
    public static readonly LangEntry1 ProfileNotEnabled;
    public static readonly LangEntry1 ProfileReloadSuccess;
    public static readonly LangEntry0 ProfileCreateSyntax;
    public static readonly LangEntry1 ProfileAlreadyExists;
    public static readonly LangEntry1 ProfileCreateSuccess;
    public static readonly LangEntry0 ProfileRenameSyntax;
    public static readonly LangEntry2 ProfileRenameSuccess;
    public static readonly LangEntry0 ProfileClearSyntax;
    public static readonly LangEntry1 ProfileClearSuccess;
    public static readonly LangEntry0 ProfileDeleteSyntax;
    public static readonly LangEntry1 ProfileDeleteBlocked;
    public static readonly LangEntry1 ProfileDeleteSuccess;
    public static readonly LangEntry0 ProfileMoveToSyntax;
    public static readonly LangEntry2 ProfileMoveToAlreadyPresent;
    public static readonly LangEntry3 ProfileMoveToSuccess;
    public static readonly LangEntry0 ProfileHelpHeader;
    public static readonly LangEntry0 ProfileHelpList;
    public static readonly LangEntry0 ProfileHelpDescribe;
    public static readonly LangEntry0 ProfileHelpEnable;
    public static readonly LangEntry0 ProfileHelpDisable;
    public static readonly LangEntry0 ProfileHelpReload;
    public static readonly LangEntry0 ProfileHelpSelect;
    public static readonly LangEntry0 ProfileHelpCreate;
    public static readonly LangEntry0 ProfileHelpRename;
    public static readonly LangEntry0 ProfileHelpClear;
    public static readonly LangEntry0 ProfileHelpDelete;
    public static readonly LangEntry0 ProfileHelpMoveTo;
    public static readonly LangEntry0 ProfileHelpInstall;
    public static readonly LangEntry0 WireToolInvisible;
    public static readonly LangEntry1 WireToolInvalidColor;
    public static readonly LangEntry0 WireToolNotEquipped;
    public static readonly LangEntry1 WireToolActivated;
    public static readonly LangEntry0 WireToolDeactivated;
    public static readonly LangEntry2 WireToolTypeMismatch;
    public static readonly LangEntry2 WireToolProfileMismatch;
    public static readonly LangEntry0 WireToolMonumentMismatch;
    public static readonly LangEntry0 HelpHeader;
    public static readonly LangEntry0 HelpSpawn;
    public static readonly LangEntry0 HelpPrefab;
    public static readonly LangEntry0 HelpKill;
    public static readonly LangEntry0 HelpUndo;
    public static readonly LangEntry0 HelpSave;
    public static readonly LangEntry0 HelpFlag;
    public static readonly LangEntry0 HelpSkin;
    public static readonly LangEntry0 HelpSetId;
    public static readonly LangEntry0 HelpSetDir;
    public static readonly LangEntry0 HelpSkull;
    public static readonly LangEntry0 HelpTrophy;
    public static readonly LangEntry0 HelpCardReaderLevel;
    public static readonly LangEntry0 HelpPuzzle;
    public static readonly LangEntry0 HelpSpawnGroup;
    public static readonly LangEntry0 HelpSpawnPoint;
    public static readonly LangEntry0 HelpPaste;
    public static readonly LangEntry0 HelpEdit;
    public static readonly LangEntry0 HelpShow;
    public static readonly LangEntry0 HelpShowVanilla;
    public static readonly LangEntry0 HelpProfile;
    public string Name;
    public string English;
    protected LangEntry(string name, string english);
}

private class LangEntry0 : LangEntry, IMessageFormatter
{
    public LangEntry0(string name, string english);
    public string Format(TemplateProvider templateProvider);
}

private class LangEntry1 : LangEntry
{
    public LangEntry1(string name, string english);
    public Formatter Bind(Tuple1 args);
    public Formatter Bind(object arg1);
}

private class LangEntry2 : LangEntry
{
    public LangEntry2(string name, string english);
    public Formatter Bind(Tuple2 args);
    public Formatter Bind(object arg1, object arg2);
}

private class LangEntry3 : LangEntry
{
    public LangEntry3(string name, string english);
    public Formatter Bind(Tuple3 args);
    public Formatter Bind(object arg1, object arg2, object arg3);
}

private class LangEntry4 : LangEntry
{
    public LangEntry4(string name, string english);
    public Formatter Bind(Tuple4 args);
    public Formatter Bind(object arg1, object arg2, object arg3, object arg4);
}

Oxide.Plugins.MonumentAddonsExtensions
public static class DictionaryExtensions
{
    public static TValue GetOrCreate(Dictionary<TKey, TValue> dict, TKey key);
}


```

---

## MonumentFinder by WhiteThunder - Provides an API for other plugins to find monuments and determine their bounds

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Reflection;
using System.Text;
using UnityEngine;

Oxide.Plugins
[Info("Monument Finder", "WhiteThunder", "3.1.4")]
[Description("Find monuments with commands or API.")]
internal class MonumentFinder : CovalencePlugin
{
    private static MonumentFinder _pluginInstance;
    private static Configuration _pluginConfig;
    private const string PermissionFind;
    private const float DrawDuration;
    private readonly FieldInfo DungeonBaseLinksFieldInfo;
    private Dictionary<MonumentInfo, NormalMonumentAdapter> _normalMonuments;
    private Dictionary<DungeonGridCell, TrainTunnelAdapter> _trainTunnels;
    private Dictionary<DungeonBaseLink, UnderwaterLabLinkAdapter> _labModules;
    private Dictionary<MonoBehaviour, BaseMonumentAdapter> _allMonuments;
    private Collider[] _colliderBuffer;
    private void Init();
    private void Unload();
    private void OnServerInitialized();
    private Dictionary<string, object> API_GetClosest(Vector3 position);
    private Dictionary<string, object> API_GetClosestMonument(Vector3 position);
    private Dictionary<string, object> API_GetClosestTrainTunnel(Vector3 position);
    private Dictionary<string, object> API_GetClosestUnderwaterLabModules(Vector3 position);
    private List<Dictionary<string, object>> API_FindMonuments(string filter);
    private List<Dictionary<string, object>> API_FindTrainTunnels(string filter);
    private List<Dictionary<string, object>> API_FindUnderwaterLabModules(string filter);
    private List<Dictionary<string, object>> API_Find(string filter);
    private List<Dictionary<string, object>> API_FindByShortName(string shortName);
    private List<Dictionary<string, object>> API_FindByAlias(string alias);
    private List<MonumentInfo> FindMonuments(string filter);
    private void CommandFind(IPlayer player, string command, string[] args);
    private void SubcommandHelp(IPlayer player, string command);
    private void SubcommandList(IPlayer player, string cmd, string[] args);
    private void SubcommandShow(IPlayer player, string command, string[] args);
    private void SubcommandClosest(IPlayer player, string command, string[] args);
    private static void LogError(string message);
    private static void LogWarning(string message);
    private static bool IsCustomMonument(MonumentInfo monumentInfo);
    private static Collider FindPreventBuildingVolume(Vector3 position);
    private static T GetClosestMonument(IEnumerable<T> monumentList, Vector3 position);
    private static Dictionary<string, object> GetClosestMonumentForAPI(IEnumerable<BaseMonumentAdapter> monumentList, Vector3 position);
    private static List<T> FilterMonuments(IEnumerable<T> monumentList, string filter, string shortName, string alias);
    private static List<Dictionary<string, object>> FilterMonumentsForAPI(IEnumerable<BaseMonumentAdapter> monumentList, string filter, string shortName, string alias);
    private void PrintMonumentList(IPlayer player, IEnumerable<BaseMonumentAdapter> monuments);
    private static void ShowMonumentName(BasePlayer player, BaseMonumentAdapter monument);
    private abstract class BaseMonumentAdapter
    {
        protected static string GetShortName(string prefabName);
        public MonoBehaviour Object { get; set; }
        public string PrefabName { get; set; }
        public string ShortName { get; set; }
        public string Alias { get; set; }
        public Vector3 Position { get; set; }
        protected Quaternion Rotation { get; set; }
        public OBB[] BoundingBoxes { get; set; }
        protected BaseMonumentAdapter(MonoBehaviour behavior);
        public Vector3 TransformPoint(Vector3 localPosition);
        public Vector3 InverseTransformPoint(Vector3 worldPosition);
        public bool IsInBounds(Vector3 position);
        public Vector3 ClosestPointOnBounds(Vector3 position);
        public virtual bool MatchesFilter(string filter, string shortName, string alias);
        private Dictionary<string, object> _cachedAPIResult;
        public Dictionary<string, object> APIResult { get; set; }
    }

    private class NormalMonumentAdapter : BaseMonumentAdapter
    {
        public static Dictionary<string, Bounds> MonumentBounds;
        public MonumentInfo MonumentInfo { get; set; }
        public NormalMonumentAdapter(MonumentInfo monumentInfo);
        public override bool MatchesFilter(string filter, string shortName, string alias);
    }

    private class TrainTunnelAdapter : BaseMonumentAdapter
    {
        public static readonly string[] IgnoredPrefabs;
        private abstract class BaseTunnelInfo
        {
            public Quaternion Rotation;
            public virtual Bounds Bounds { get; set; }
            public virtual string Alias { get; set; }
        }

        private class TrainStation : BaseTunnelInfo
        {
            public override Bounds Bounds { get; set; }
            public override string Alias { get; set; }
        }

        private class BarricadeTunnel : BaseTunnelInfo
        {
            public override Bounds Bounds { get; set; }
            public override string Alias { get; set; }
        }

        private class LootTunnel : BaseTunnelInfo
        {
            public override Bounds Bounds { get; set; }
            public override string Alias { get; set; }
        }

        private class SplitTunnel : BaseTunnelInfo
        {
            public override Bounds Bounds { get; set; }
            public override string Alias { get; set; }
        }

        private class Intersection : BaseTunnelInfo
        {
            public override Bounds Bounds { get; set; }
            public override string Alias { get; set; }
        }

        private class LargeIntersection : BaseTunnelInfo
        {
            public override Bounds Bounds { get; set; }
            public override string Alias { get; set; }
        }

        private class VerticalIntersection : BaseTunnelInfo
        {
            public override Bounds Bounds { get; set; }
            public override string Alias { get; set; }
        }

        private class CornerTunnel : BaseTunnelInfo
        {
            public override Bounds Bounds { get; set; }
            public override string Alias { get; set; }
        }

        private static readonly Dictionary<string, BaseTunnelInfo> PrefabToTunnelInfo;
        private static BaseTunnelInfo GetTunnelInfo(string shortName);
        private BaseTunnelInfo _tunnelInfo;
        public TrainTunnelAdapter(DungeonGridCell dungeonCell);
    }

    private class UnderwaterLabLinkAdapter : BaseMonumentAdapter
    {
        public UnderwaterLabLinkAdapter(DungeonBaseLink dungeonLink);
    }

    private static class Ddraw
    {
        public static void Sphere(BasePlayer player, Vector3 origin, float radius, Color color, float duration);
        public static void Line(BasePlayer player, Vector3 origin, Vector3 target, Color color, float duration);
        public static void Text(BasePlayer player, Vector3 origin, string text, Color color, float duration);
        public static void Segments(BasePlayer player, Vector3 origin, Vector3 target, Color color, float duration);
        public static void Box(BasePlayer player, Vector3 center, Quaternion rotation, Vector3 halfExtents, Color color, float duration, bool showBoxDetails);
        public static void Box(BasePlayer player, OBB boundingBox, Color color, float duration, bool showBoxDetails);
    }

    private class CustomBounds
    {
        [JsonProperty("Size")]
        public Vector3 Size;
        [JsonProperty("Center adjustment")]
        public Vector3 CenterOffset;
        [JsonProperty("Center")]
        private Vector3 DeprecatedCenter { get; set; }
        public Bounds ToBounds();
        public CustomBounds Copy();
    }

    private class BaseDetectionSettings
    {
        [JsonProperty("Auto determine from monument marker", Order = -3)]
        public bool UseMonumentMarker;
        [JsonProperty("Auto determine from prevent building volume", Order = -2)]
        public bool UsePreventBuildingVolume;
        public BaseDetectionSettings Copy();
    }

    private class BoundSettings : BaseDetectionSettings
    {
        [JsonProperty("Use custom bounds")]
        public bool UseCustomBounds;
        [JsonProperty("Custom bounds")]
        public CustomBounds CustomBounds;
        public new BoundSettings Copy();
    }

    private class MonumentSettings
    {
        [JsonProperty("Position")]
        public BaseDetectionSettings Position;
        [JsonProperty("Rotation")]
        public BaseDetectionSettings Rotation;
        [JsonProperty("Bounds")]
        public BoundSettings Bounds;
        public MonumentSettings Copy();
    }

    private class Configuration : SerializableConfiguration
    {
        [JsonProperty(PropertyName = "Command")]
        public string Command;
        [JsonProperty("Default custom monument settings")]
        public MonumentSettings DefaultCustomMonumentSettings;
        [JsonProperty("Monuments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        private Dictionary<string, MonumentSettings> MonumentSettingsMap;
        [JsonProperty("OverrideMonumentBounds")]
        private Dictionary<string, CustomBounds> DeprecatedOverrideMonumentBounds { get; set; }
        public MonumentSettings GetMonumentSettings(string monumentName);
        public bool AddMonument(string aliasOrShortName, BaseMonumentAdapter monument);
    }

    private Configuration GetDefaultConfig();
    private class SerializableConfiguration
    {
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    private static class JsonHelper
    {
        public static object Deserialize(string json);
        private static object ToObject(JToken token);
    }

    private bool MaybeUpdateConfig(SerializableConfiguration config);
    private bool MaybeUpdateConfigDict(Dictionary<string, object> currentWithDefaults, Dictionary<string, object> currentRaw);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private string GetMessage(string playerId, string messageName, object[] args);
    private string GetMessage(IPlayer player, string messageName, object[] args);
    private void ReplyToPlayer(IPlayer player, string messageName, object[] args);
    private static class Lang
    {
        public const string ErrorNoPermission;
        public const string NoMonumentsFound;
        public const string ListHeader;
        public const string AtMonument;
        public const string ClosestMonument;
        public const string ClosestConfigSuccess;
        public const string ClosestConfigAlreadyPresent;
        public const string HelpHeader;
        public const string HelpList;
        public const string HelpShow;
        public const string HelpClosest;
        public const string HelpClosestConfig;
    }

    protected override void LoadDefaultMessages();
}

private abstract class BaseMonumentAdapter
{
    protected static string GetShortName(string prefabName);
    public MonoBehaviour Object { get; set; }
    public string PrefabName { get; set; }
    public string ShortName { get; set; }
    public string Alias { get; set; }
    public Vector3 Position { get; set; }
    protected Quaternion Rotation { get; set; }
    public OBB[] BoundingBoxes { get; set; }
    protected BaseMonumentAdapter(MonoBehaviour behavior);
    public Vector3 TransformPoint(Vector3 localPosition);
    public Vector3 InverseTransformPoint(Vector3 worldPosition);
    public bool IsInBounds(Vector3 position);
    public Vector3 ClosestPointOnBounds(Vector3 position);
    public virtual bool MatchesFilter(string filter, string shortName, string alias);
    private Dictionary<string, object> _cachedAPIResult;
    public Dictionary<string, object> APIResult { get; set; }
}

private class NormalMonumentAdapter : BaseMonumentAdapter
{
    public static Dictionary<string, Bounds> MonumentBounds;
    public MonumentInfo MonumentInfo { get; set; }
    public NormalMonumentAdapter(MonumentInfo monumentInfo);
    public override bool MatchesFilter(string filter, string shortName, string alias);
}

private class TrainTunnelAdapter : BaseMonumentAdapter
{
    public static readonly string[] IgnoredPrefabs;
    private abstract class BaseTunnelInfo
    {
        public Quaternion Rotation;
        public virtual Bounds Bounds { get; set; }
        public virtual string Alias { get; set; }
    }

    private class TrainStation : BaseTunnelInfo
    {
        public override Bounds Bounds { get; set; }
        public override string Alias { get; set; }
    }

    private class BarricadeTunnel : BaseTunnelInfo
    {
        public override Bounds Bounds { get; set; }
        public override string Alias { get; set; }
    }

    private class LootTunnel : BaseTunnelInfo
    {
        public override Bounds Bounds { get; set; }
        public override string Alias { get; set; }
    }

    private class SplitTunnel : BaseTunnelInfo
    {
        public override Bounds Bounds { get; set; }
        public override string Alias { get; set; }
    }

    private class Intersection : BaseTunnelInfo
    {
        public override Bounds Bounds { get; set; }
        public override string Alias { get; set; }
    }

    private class LargeIntersection : BaseTunnelInfo
    {
        public override Bounds Bounds { get; set; }
        public override string Alias { get; set; }
    }

    private class VerticalIntersection : BaseTunnelInfo
    {
        public override Bounds Bounds { get; set; }
        public override string Alias { get; set; }
    }

    private class CornerTunnel : BaseTunnelInfo
    {
        public override Bounds Bounds { get; set; }
        public override string Alias { get; set; }
    }

    private static readonly Dictionary<string, BaseTunnelInfo> PrefabToTunnelInfo;
    private static BaseTunnelInfo GetTunnelInfo(string shortName);
    private BaseTunnelInfo _tunnelInfo;
    public TrainTunnelAdapter(DungeonGridCell dungeonCell);
}

private abstract class BaseTunnelInfo
{
    public Quaternion Rotation;
    public virtual Bounds Bounds { get; set; }
    public virtual string Alias { get; set; }
}

private class TrainStation : BaseTunnelInfo
{
    public override Bounds Bounds { get; set; }
    public override string Alias { get; set; }
}

private class BarricadeTunnel : BaseTunnelInfo
{
    public override Bounds Bounds { get; set; }
    public override string Alias { get; set; }
}

private class LootTunnel : BaseTunnelInfo
{
    public override Bounds Bounds { get; set; }
    public override string Alias { get; set; }
}

private class SplitTunnel : BaseTunnelInfo
{
    public override Bounds Bounds { get; set; }
    public override string Alias { get; set; }
}

private class Intersection : BaseTunnelInfo
{
    public override Bounds Bounds { get; set; }
    public override string Alias { get; set; }
}

private class LargeIntersection : BaseTunnelInfo
{
    public override Bounds Bounds { get; set; }
    public override string Alias { get; set; }
}

private class VerticalIntersection : BaseTunnelInfo
{
    public override Bounds Bounds { get; set; }
    public override string Alias { get; set; }
}

private class CornerTunnel : BaseTunnelInfo
{
    public override Bounds Bounds { get; set; }
    public override string Alias { get; set; }
}

private class UnderwaterLabLinkAdapter : BaseMonumentAdapter
{
    public UnderwaterLabLinkAdapter(DungeonBaseLink dungeonLink);
}

private static class Ddraw
{
    public static void Sphere(BasePlayer player, Vector3 origin, float radius, Color color, float duration);
    public static void Line(BasePlayer player, Vector3 origin, Vector3 target, Color color, float duration);
    public static void Text(BasePlayer player, Vector3 origin, string text, Color color, float duration);
    public static void Segments(BasePlayer player, Vector3 origin, Vector3 target, Color color, float duration);
    public static void Box(BasePlayer player, Vector3 center, Quaternion rotation, Vector3 halfExtents, Color color, float duration, bool showBoxDetails);
    public static void Box(BasePlayer player, OBB boundingBox, Color color, float duration, bool showBoxDetails);
}

private class CustomBounds
{
    [JsonProperty("Size")]
    public Vector3 Size;
    [JsonProperty("Center adjustment")]
    public Vector3 CenterOffset;
    [JsonProperty("Center")]
    private Vector3 DeprecatedCenter { get; set; }
    public Bounds ToBounds();
    public CustomBounds Copy();
}

private class BaseDetectionSettings
{
    [JsonProperty("Auto determine from monument marker", Order = -3)]
    public bool UseMonumentMarker;
    [JsonProperty("Auto determine from prevent building volume", Order = -2)]
    public bool UsePreventBuildingVolume;
    public BaseDetectionSettings Copy();
}

private class BoundSettings : BaseDetectionSettings
{
    [JsonProperty("Use custom bounds")]
    public bool UseCustomBounds;
    [JsonProperty("Custom bounds")]
    public CustomBounds CustomBounds;
    public new BoundSettings Copy();
}

private class MonumentSettings
{
    [JsonProperty("Position")]
    public BaseDetectionSettings Position;
    [JsonProperty("Rotation")]
    public BaseDetectionSettings Rotation;
    [JsonProperty("Bounds")]
    public BoundSettings Bounds;
    public MonumentSettings Copy();
}

private class Configuration : SerializableConfiguration
{
    [JsonProperty(PropertyName = "Command")]
    public string Command;
    [JsonProperty("Default custom monument settings")]
    public MonumentSettings DefaultCustomMonumentSettings;
    [JsonProperty("Monuments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    private Dictionary<string, MonumentSettings> MonumentSettingsMap;
    [JsonProperty("OverrideMonumentBounds")]
    private Dictionary<string, CustomBounds> DeprecatedOverrideMonumentBounds { get; set; }
    public MonumentSettings GetMonumentSettings(string monumentName);
    public bool AddMonument(string aliasOrShortName, BaseMonumentAdapter monument);
}

private class SerializableConfiguration
{
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private static class JsonHelper
{
    public static object Deserialize(string json);
    private static object ToObject(JToken token);
}

private static class Lang
{
    public const string ErrorNoPermission;
    public const string NoMonumentsFound;
    public const string ListHeader;
    public const string AtMonument;
    public const string ClosestMonument;
    public const string ClosestConfigSuccess;
    public const string ClosestConfigAlreadyPresent;
    public const string HelpHeader;
    public const string HelpList;
    public const string HelpShow;
    public const string HelpClosest;
    public const string HelpClosestConfig;
}


```

---

## MonumentLifts by Mevent - Adds lifts to monuments

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Monument Lifts", "Mevent", "1.0.3")]
[Description("Adds lifts to monuments")]
public class MonumentLifts : RustPlugin
{
    private readonly List<ModularCarGarage> _lifts;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Prefab")]
        public string Prefab;
        [JsonProperty(PropertyName = "Spawn Setting (name - pos correction)",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, Vector3> Monuments;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    private void Unload();
    private void OnEntityTakeDamage(ModularCarGarage entity, HitInfo info);
    private void SpawnEntity(Vector3 pos, Quaternion rot);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Prefab")]
    public string Prefab;
    [JsonProperty(PropertyName = "Spawn Setting (name - pos correction)",
				ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, Vector3> Monuments;
}


```

---

## MonumentPlayerSettings by YaMang - Block certain actions, commands, etc. within the monument. (to teleport)

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using UnityEngine;

Oxide.Plugins
[Info("Monument Player Settings", "YaMang-w-", "1.0.3")]
[Description("Block certain actions, commands, etc. within the monument. (to teleport)")]
 class MonumentPlayerSettings : RustPlugin
{
    [PluginReference]
    private Plugin NoEscape;
    private const string OutPostPrefab;
    private const string BanditTownPrefab;
    private const string MainUI;
    private const string UsePermission;
     Dictionary<string, Timer> teleportTimer;
     Dictionary<string, Vector3> monumentLists;
     void OnServerInitialized();
     void Unload();
     void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
     object OnPlayerCommand(BasePlayer player, string command, string[] args);
     bool CanPickupEntity(BasePlayer player, BaseEntity entity);
    private object OnSprayCreate(SprayCan spray, Vector3 position, Quaternion rotation);
    private void MonumentTPCMD(BasePlayer player, string command, string[] args);
    [ConsoleCommand("teleport.monument")]
    private void MonumentTeleport(ConsoleSystem.Arg arg);
    private ConfigData _config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "General Settings")]
        public GeneralSettings generalSettings { get; set; }
        [JsonProperty(PropertyName = "Teleport Settings")]
        public TPMonumentSetting tpMonumentSetting { get; set; }
        [JsonProperty(PropertyName = "Monument Settings")]
        public MonumentSetting monumentSetting { get; set; }
        public Oxide.Core.VersionNumber Version { get; set; }
    }

    public class GeneralSettings
    {
        [JsonProperty(PropertyName = "Prefix", Order = 1)]
        public string Prefix { get; set; }
        [JsonProperty(PropertyName = "SteamID", Order = 2)]
        public ulong SteamID { get; set; }
        [JsonProperty(PropertyName = "Commands", Order = 3)]
        public string Commands { get; set; }
    }

    public class TPMonumentSetting
    {
        [JsonProperty(PropertyName = "Use teleport Outpost?")]
        public bool useOutPost { get; set; }
        [JsonProperty(PropertyName = "teleport cooltime Outpost")]
        public float cooltimeOutPost { get; set; }
        [JsonProperty(PropertyName = "Use teleport Bandit?")]
        public bool useBandit { get; set; }
        [JsonProperty(PropertyName = "teleport cooltime Bandit")]
        public float cooltimeBandit { get; set; }
    }

    public class MonumentSetting
    {
        [JsonProperty(PropertyName = "Block commands in monuments")]
        public List<string> blockInMCommands { get; set; }
        [JsonProperty(PropertyName = "Block Spary in monuments")]
        public bool blockInMSpray { get; set; }
        [JsonProperty(PropertyName = "Block Active Item in monuments")]
        public List<string> blockInMActiveItems { get; set; }
        [JsonProperty(PropertyName = "Block Pickup in monuments")]
        public bool blockInMPickup { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    protected override void LoadDefaultMessages();
    private string Lang(string key, object[] args);
    private void Messages(BasePlayer player, string text);
    private string SetFlags(BasePlayer player);
    private bool CheckFlags(BasePlayer player);
    private bool InMonumentPosition(BasePlayer player);
    private Vector3 FindMonumentPosition(string monumentPrefab);
    private bool TeleportPlayer(BasePlayer player, Vector3 pos, string text);
    private static string HexToCuiColor(string HEX, float Alpha);
    private void MonumentTeleportUI(BasePlayer player);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "General Settings")]
    public GeneralSettings generalSettings { get; set; }
    [JsonProperty(PropertyName = "Teleport Settings")]
    public TPMonumentSetting tpMonumentSetting { get; set; }
    [JsonProperty(PropertyName = "Monument Settings")]
    public MonumentSetting monumentSetting { get; set; }
    public Oxide.Core.VersionNumber Version { get; set; }
}

public class GeneralSettings
{
    [JsonProperty(PropertyName = "Prefix", Order = 1)]
    public string Prefix { get; set; }
    [JsonProperty(PropertyName = "SteamID", Order = 2)]
    public ulong SteamID { get; set; }
    [JsonProperty(PropertyName = "Commands", Order = 3)]
    public string Commands { get; set; }
}

public class TPMonumentSetting
{
    [JsonProperty(PropertyName = "Use teleport Outpost?")]
    public bool useOutPost { get; set; }
    [JsonProperty(PropertyName = "teleport cooltime Outpost")]
    public float cooltimeOutPost { get; set; }
    [JsonProperty(PropertyName = "Use teleport Bandit?")]
    public bool useBandit { get; set; }
    [JsonProperty(PropertyName = "teleport cooltime Bandit")]
    public float cooltimeBandit { get; set; }
}

public class MonumentSetting
{
    [JsonProperty(PropertyName = "Block commands in monuments")]
    public List<string> blockInMCommands { get; set; }
    [JsonProperty(PropertyName = "Block Spary in monuments")]
    public bool blockInMSpray { get; set; }
    [JsonProperty(PropertyName = "Block Active Item in monuments")]
    public List<string> blockInMActiveItems { get; set; }
    [JsonProperty(PropertyName = "Block Pickup in monuments")]
    public bool blockInMPickup { get; set; }
}


```

---

## MonumentPlus by Razor - Auto spawn prefabs at monuments

```csharp
using System.Collections.Generic;
using Oxide.Core.Configuration;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using UnityEngine;
using System.Linq;
using Oxide.Core;
using System;
using Oxide.Game.Rust.Cui;
using System.Text.RegularExpressions;
using Color = UnityEngine.Color;
using VLB;
using Rust;

Oxide.Plugins
[Info("Monument Plus Lite", "Ts3Hosting", "1.1.0")]
[Description("Auto spawn prefabs at monuments")]
public class MonumentPlus : RustPlugin
{
    public static MonumentPlus _;
    public List<NetworkableId> spawnedEntitys;
    private const int DamageTypeMax;
    private new void LoadDefaultMessages();
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Settings")]
        public Settings settings { get; set; }
        [JsonProperty(PropertyName = "Spawn Addons At")]
        public Entity entity { get; set; }
        public class Settings
        {
            public bool AutoSpawnOnBoot { get; set; }
        }

        public class Entity
        {
            public Dictionary<string, itemSpawning> Information { get; set; }
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
     class itemSpawning
    {
        public bool enabled;
        public string MonumentName;
        public string prefab;
        public Vector3 pos;
        public float rotate;
    }

    protected override void SaveConfig();
    private void UpdateConfigValues();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    [ChatCommand("monumentinfo")]
    private void GetLocations(BasePlayer player, string command, string[] args);
    public string GetMonumentName(MonumentInfo monument);
    private void spawningAddons(bool unLoading);
    private void RemoveGroundWatch(BaseEntity entity);
    private void OnEntityTakeDamage(BaseEntity entity, HitInfo hitInfo);
    private static Dictionary<string, string> MonumentToName { get; set; }
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Settings")]
    public Settings settings { get; set; }
    [JsonProperty(PropertyName = "Spawn Addons At")]
    public Entity entity { get; set; }
    public class Settings
    {
        public bool AutoSpawnOnBoot { get; set; }
    }

    public class Entity
    {
        public Dictionary<string, itemSpawning> Information { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class Settings
{
    public bool AutoSpawnOnBoot { get; set; }
}

public class Entity
{
    public Dictionary<string, itemSpawning> Information { get; set; }
}

 class itemSpawning
{
    public bool enabled;
    public string MonumentName;
    public string prefab;
    public Vector3 pos;
    public float rotate;
}


```

---

## MonumentRadiation by k1lly0u - Creates radiation zones at designated monuments

```csharp
using System.Collections.Generic;
using Network;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("MonumentRadiation", "k1lly0u", "0.2.58")]
[Description("Create radiation zones at designated monuments")]
 class MonumentRadiation : RustPlugin
{
    private static MonumentRadiation Instance { get; set; }
    private List<RadiationZone> radiationZones;
    private bool radsOn;
    private int offTimer;
    private int onTimer;
    private const int PLAYER_MASK;
    private void OnServerInitialized();
    private void Unload();
    private void DestroyAllComponents();
    private void FindMonuments();
    private void CreateHapis();
    private void ConfirmCreation();
    private void CreateZone(ConfigData.RadZones.MonumentSettings zone, Vector3 pos);
    private void StartRadTimers();
    private int GetRandom(int min, int max);
    private void SendEchoConsole(Network.Connection cn, string msg);
    private bool IsAdmin(BasePlayer player);
    private bool IsAuthed(ConsoleSystem.Arg arg);
    [ConsoleCommand("mr_list")]
    private void ccmdRadZoneList(ConsoleSystem.Arg arg);
    [ChatCommand("mr_list")]
    private void cmdRadZoneList(BasePlayer player, string command, string[] args);
    [ChatCommand("mr")]
    private void cmdCheckTimers(BasePlayer player, string command, string[] args);
    [ChatCommand("mr_show")]
    private void cmdShowZones(BasePlayer player, string command, string[] args);
    private class RadiationZone : MonoBehaviour
    {
        private TriggerRadiation triggerRadiation;
        public float radius;
        public float amount;
        private void Awake();
        private void OnDestroy();
        private void OnTriggerEnter(Collider obj);
        private void OnTriggerExit(Collider obj);
        public void InitializeRadiationZone(string type, Vector3 position, float radius, float amount);
        public void Deactivate();
        public void Reactivate();
    }

    private Dictionary<string, HapisIslandMonuments> HIMon;
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Messaging Settings")]
        public Messaging Messages { get; set; }
        public RadiationTimers Timers { get; set; }
        public Options Settings { get; set; }
        [JsonProperty(PropertyName = "Zone Settings")]
        public RadZones Zones { get; set; }
        public class Messaging
        {
            [JsonProperty(PropertyName = "Display message to player when they enter a radiation zone")]
            public bool Enter { get; set; }
            [JsonProperty(PropertyName = "Display message to player when they leave a radiation zone")]
            public bool Exit { get; set; }
        }

        public class RadiationTimers
        {
            [JsonProperty(PropertyName = "Random on time (minimum minutes)")]
            public int ROnMin { get; set; }
            [JsonProperty(PropertyName = "Random on time (maximum minutes)")]
            public int ROnmax { get; set; }
            [JsonProperty(PropertyName = "Random off time (minimum minutes)")]
            public int ROffMin { get; set; }
            [JsonProperty(PropertyName = "Random off time (maximum minutes)")]
            public int ROffMax { get; set; }
            [JsonProperty(PropertyName = "Forced off time (minutes)")]
            public int StaticOff { get; set; }
            [JsonProperty(PropertyName = "Forced on time (minutes)")]
            public int StaticOn { get; set; }
        }

        public class RadZones
        {
            public MonumentSettings Airfield { get; set; }
            public MonumentSettings Dome { get; set; }
            public MonumentSettings Junkyard { get; set; }
            public MonumentSettings Lighthouse { get; set; }
            public MonumentSettings LargeHarbor { get; set; }
            public MonumentSettings GasStation { get; set; }
            public MonumentSettings Powerplant { get; set; }
            [JsonProperty(PropertyName = "Stone Quarry")]
            public MonumentSettings Quarry_Stone { get; set; }
            [JsonProperty(PropertyName = "Sulfur Quarry")]
            public MonumentSettings Quarry_Sulfur { get; set; }
            [JsonProperty(PropertyName = "HQM Quarry")]
            public MonumentSettings Quarry_HQM { get; set; }
            public MonumentSettings Radtown { get; set; }
            public MonumentSettings RocketFactory { get; set; }
            public MonumentSettings Satellite { get; set; }
            public MonumentSettings SmallHarbor { get; set; }
            public MonumentSettings Supermarket { get; set; }
            public MonumentSettings Trainyard { get; set; }
            public MonumentSettings Tunnels { get; set; }
            public MonumentSettings Warehouse { get; set; }
            public MonumentSettings WaterTreatment { get; set; }
            public class MonumentSettings
            {
                [JsonProperty(PropertyName = "Enable radiation at this monument")]
                public bool Activate;
                [JsonProperty(PropertyName = "Monument name (internal use)")]
                public string Name;
                [JsonProperty(PropertyName = "Radius of radiation")]
                public float Radius;
                [JsonProperty(PropertyName = "Radiation amount")]
                public float Radiation;
            }

        }

        public class Options
        {
            [JsonProperty(PropertyName = "Broadcast radiation status changes")]
            public bool ShowTimers { get; set; }
            [JsonProperty(PropertyName = "Using Hapis Island map")]
            public bool IsHapis { get; set; }
            [JsonProperty(PropertyName = "Enable InfoPanel integration")]
            public bool Infopanel { get; set; }
            [JsonProperty(PropertyName = "Use radiation toggle timers")]
            public bool UseTimers { get; set; }
            [JsonProperty(PropertyName = "Randomise radiation timers")]
            public bool UseRandomTimers { get; set; }
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private string msg(string key, ulong playerId);
    private Dictionary<string, string> Messages;
}

private class RadiationZone : MonoBehaviour
{
    private TriggerRadiation triggerRadiation;
    public float radius;
    public float amount;
    private void Awake();
    private void OnDestroy();
    private void OnTriggerEnter(Collider obj);
    private void OnTriggerExit(Collider obj);
    public void InitializeRadiationZone(string type, Vector3 position, float radius, float amount);
    public void Deactivate();
    public void Reactivate();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Messaging Settings")]
    public Messaging Messages { get; set; }
    public RadiationTimers Timers { get; set; }
    public Options Settings { get; set; }
    [JsonProperty(PropertyName = "Zone Settings")]
    public RadZones Zones { get; set; }
    public class Messaging
    {
        [JsonProperty(PropertyName = "Display message to player when they enter a radiation zone")]
        public bool Enter { get; set; }
        [JsonProperty(PropertyName = "Display message to player when they leave a radiation zone")]
        public bool Exit { get; set; }
    }

    public class RadiationTimers
    {
        [JsonProperty(PropertyName = "Random on time (minimum minutes)")]
        public int ROnMin { get; set; }
        [JsonProperty(PropertyName = "Random on time (maximum minutes)")]
        public int ROnmax { get; set; }
        [JsonProperty(PropertyName = "Random off time (minimum minutes)")]
        public int ROffMin { get; set; }
        [JsonProperty(PropertyName = "Random off time (maximum minutes)")]
        public int ROffMax { get; set; }
        [JsonProperty(PropertyName = "Forced off time (minutes)")]
        public int StaticOff { get; set; }
        [JsonProperty(PropertyName = "Forced on time (minutes)")]
        public int StaticOn { get; set; }
    }

    public class RadZones
    {
        public MonumentSettings Airfield { get; set; }
        public MonumentSettings Dome { get; set; }
        public MonumentSettings Junkyard { get; set; }
        public MonumentSettings Lighthouse { get; set; }
        public MonumentSettings LargeHarbor { get; set; }
        public MonumentSettings GasStation { get; set; }
        public MonumentSettings Powerplant { get; set; }
        [JsonProperty(PropertyName = "Stone Quarry")]
        public MonumentSettings Quarry_Stone { get; set; }
        [JsonProperty(PropertyName = "Sulfur Quarry")]
        public MonumentSettings Quarry_Sulfur { get; set; }
        [JsonProperty(PropertyName = "HQM Quarry")]
        public MonumentSettings Quarry_HQM { get; set; }
        public MonumentSettings Radtown { get; set; }
        public MonumentSettings RocketFactory { get; set; }
        public MonumentSettings Satellite { get; set; }
        public MonumentSettings SmallHarbor { get; set; }
        public MonumentSettings Supermarket { get; set; }
        public MonumentSettings Trainyard { get; set; }
        public MonumentSettings Tunnels { get; set; }
        public MonumentSettings Warehouse { get; set; }
        public MonumentSettings WaterTreatment { get; set; }
        public class MonumentSettings
        {
            [JsonProperty(PropertyName = "Enable radiation at this monument")]
            public bool Activate;
            [JsonProperty(PropertyName = "Monument name (internal use)")]
            public string Name;
            [JsonProperty(PropertyName = "Radius of radiation")]
            public float Radius;
            [JsonProperty(PropertyName = "Radiation amount")]
            public float Radiation;
        }

    }

    public class Options
    {
        [JsonProperty(PropertyName = "Broadcast radiation status changes")]
        public bool ShowTimers { get; set; }
        [JsonProperty(PropertyName = "Using Hapis Island map")]
        public bool IsHapis { get; set; }
        [JsonProperty(PropertyName = "Enable InfoPanel integration")]
        public bool Infopanel { get; set; }
        [JsonProperty(PropertyName = "Use radiation toggle timers")]
        public bool UseTimers { get; set; }
        [JsonProperty(PropertyName = "Randomise radiation timers")]
        public bool UseRandomTimers { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class Messaging
{
    [JsonProperty(PropertyName = "Display message to player when they enter a radiation zone")]
    public bool Enter { get; set; }
    [JsonProperty(PropertyName = "Display message to player when they leave a radiation zone")]
    public bool Exit { get; set; }
}

public class RadiationTimers
{
    [JsonProperty(PropertyName = "Random on time (minimum minutes)")]
    public int ROnMin { get; set; }
    [JsonProperty(PropertyName = "Random on time (maximum minutes)")]
    public int ROnmax { get; set; }
    [JsonProperty(PropertyName = "Random off time (minimum minutes)")]
    public int ROffMin { get; set; }
    [JsonProperty(PropertyName = "Random off time (maximum minutes)")]
    public int ROffMax { get; set; }
    [JsonProperty(PropertyName = "Forced off time (minutes)")]
    public int StaticOff { get; set; }
    [JsonProperty(PropertyName = "Forced on time (minutes)")]
    public int StaticOn { get; set; }
}

public class RadZones
{
    public MonumentSettings Airfield { get; set; }
    public MonumentSettings Dome { get; set; }
    public MonumentSettings Junkyard { get; set; }
    public MonumentSettings Lighthouse { get; set; }
    public MonumentSettings LargeHarbor { get; set; }
    public MonumentSettings GasStation { get; set; }
    public MonumentSettings Powerplant { get; set; }
    [JsonProperty(PropertyName = "Stone Quarry")]
    public MonumentSettings Quarry_Stone { get; set; }
    [JsonProperty(PropertyName = "Sulfur Quarry")]
    public MonumentSettings Quarry_Sulfur { get; set; }
    [JsonProperty(PropertyName = "HQM Quarry")]
    public MonumentSettings Quarry_HQM { get; set; }
    public MonumentSettings Radtown { get; set; }
    public MonumentSettings RocketFactory { get; set; }
    public MonumentSettings Satellite { get; set; }
    public MonumentSettings SmallHarbor { get; set; }
    public MonumentSettings Supermarket { get; set; }
    public MonumentSettings Trainyard { get; set; }
    public MonumentSettings Tunnels { get; set; }
    public MonumentSettings Warehouse { get; set; }
    public MonumentSettings WaterTreatment { get; set; }
    public class MonumentSettings
    {
        [JsonProperty(PropertyName = "Enable radiation at this monument")]
        public bool Activate;
        [JsonProperty(PropertyName = "Monument name (internal use)")]
        public string Name;
        [JsonProperty(PropertyName = "Radius of radiation")]
        public float Radius;
        [JsonProperty(PropertyName = "Radiation amount")]
        public float Radiation;
    }

}

public class MonumentSettings
{
    [JsonProperty(PropertyName = "Enable radiation at this monument")]
    public bool Activate;
    [JsonProperty(PropertyName = "Monument name (internal use)")]
    public string Name;
    [JsonProperty(PropertyName = "Radius of radiation")]
    public float Radius;
    [JsonProperty(PropertyName = "Radiation amount")]
    public float Radiation;
}

public class Options
{
    [JsonProperty(PropertyName = "Broadcast radiation status changes")]
    public bool ShowTimers { get; set; }
    [JsonProperty(PropertyName = "Using Hapis Island map")]
    public bool IsHapis { get; set; }
    [JsonProperty(PropertyName = "Enable InfoPanel integration")]
    public bool Infopanel { get; set; }
    [JsonProperty(PropertyName = "Use radiation toggle timers")]
    public bool UseTimers { get; set; }
    [JsonProperty(PropertyName = "Randomise radiation timers")]
    public bool UseRandomTimers { get; set; }
}


```

---

## MonumentsRecycler by VisEntities - Adds recyclers to monuments, including the Cargo Ship

```csharp
using UnityEngine;
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Linq;
using Rust;

Oxide.Plugins
[Info("Monuments Recycler", "Dana", "0.2.6")]
[Description("Adds recyclers to monuments including the cargo ship.")]
internal class MonumentsRecycler : RustPlugin
{
    private bool _serverInitialized;
    private const string RecyclerPrefab;
    private const string SmallOilRigKey;
    private const string LargeOilRigKey;
    private const string DomeKey;
    const string FishingVillageLargePrefab;
    const string FishingVillageSmallBPrefab;
    const string FishingVillageSmallAPrefab;
    private List<BaseEntity> _recyclers;
    private SpawnData _domeSpawnData;
    private readonly Dictionary<string, SpawnData> _smallOilRigRecyclerPositions;
    private readonly Dictionary<string, SpawnData> _largeOilRigRecyclerPositions;
    private readonly Dictionary<string, SpawnData> _fishingVillageRecyclerPositions;
    private Configuration _config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Cargo Ship - Recycler Position - Front")]
        public bool RecyclerPositionFront;
        [JsonProperty(PropertyName = "Cargo Ship - Recycler Position - Back")]
        public bool RecyclerPositionBack;
        [JsonProperty(PropertyName = "Cargo Ship - Recycler Position - Bottom")]
        public bool RecyclerPositionBottom;
        [JsonProperty(PropertyName = "Large Oil Rig - Recycler Position - Level 4")]
        public bool LargeOilRigRecyclerPositionLevel4;
        [JsonProperty(PropertyName = "Large Oil Rig - Recycler Position - Level 6")]
        public bool LargeOilRigRecyclerPositionLevel6;
        [JsonProperty(PropertyName = "Small Oil Rig - Recycler Position - Level 3")]
        public bool SmallOilRigRecyclerPositionLevel3;
        [JsonProperty(PropertyName = "Small Oil Rig - Recycler Position - Level 4")]
        public bool SmallOilRigRecyclerPositionLevel4;
        [JsonProperty(PropertyName = "Fishing Village - Recycler - Large")]
        public bool FishingVillageRecyclerLarge { get; set; }
        [JsonProperty(PropertyName = "Fishing Village - Recycler - Small A")]
        public bool FishingVillageRecyclerSmallA { get; set; }
        [JsonProperty(PropertyName = "Fishing Village - Recycler - Small B")]
        public bool FishingVillageRecyclerSmallB { get; set; }
        [JsonProperty(PropertyName = "Dome - Recycler")]
        public bool DomeRecycler;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     Tuple<Vector3, Quaternion> GetCargoFrontPosition(Vector3 position, Quaternion rotation);
     Tuple<Vector3, Quaternion> GetCargoBackPosition(Vector3 position, Quaternion rotation);
     Tuple<Vector3, Quaternion> GetCargoBottomPosition(Vector3 position, Quaternion rotation);
    private void OnEntitySpawned(CargoShip ship);
    private void OnServerInitialized();
    private void ShowRecyclers();
    private void SpawnFishingVillageRecycler(Transform objTransform, Vector3 spawnOffset, Vector3 rotationOffset);
    private void SpawnRecycler(Transform objTransform, Vector3 spawnOffset, Vector3 rotationOffset);
    private void SpawnRecycler(CargoShip ship, Tuple<Vector3, Quaternion> selectedTransform);
    private void Unload();
    private class SpawnData
    {
        public SpawnData(Vector3 position, Vector3 rotation);
        public Vector3 Position { get; set; }
        public Vector3 Rotation { get; set; }
    }

}

public class Configuration
{
    [JsonProperty(PropertyName = "Cargo Ship - Recycler Position - Front")]
    public bool RecyclerPositionFront;
    [JsonProperty(PropertyName = "Cargo Ship - Recycler Position - Back")]
    public bool RecyclerPositionBack;
    [JsonProperty(PropertyName = "Cargo Ship - Recycler Position - Bottom")]
    public bool RecyclerPositionBottom;
    [JsonProperty(PropertyName = "Large Oil Rig - Recycler Position - Level 4")]
    public bool LargeOilRigRecyclerPositionLevel4;
    [JsonProperty(PropertyName = "Large Oil Rig - Recycler Position - Level 6")]
    public bool LargeOilRigRecyclerPositionLevel6;
    [JsonProperty(PropertyName = "Small Oil Rig - Recycler Position - Level 3")]
    public bool SmallOilRigRecyclerPositionLevel3;
    [JsonProperty(PropertyName = "Small Oil Rig - Recycler Position - Level 4")]
    public bool SmallOilRigRecyclerPositionLevel4;
    [JsonProperty(PropertyName = "Fishing Village - Recycler - Large")]
    public bool FishingVillageRecyclerLarge { get; set; }
    [JsonProperty(PropertyName = "Fishing Village - Recycler - Small A")]
    public bool FishingVillageRecyclerSmallA { get; set; }
    [JsonProperty(PropertyName = "Fishing Village - Recycler - Small B")]
    public bool FishingVillageRecyclerSmallB { get; set; }
    [JsonProperty(PropertyName = "Dome - Recycler")]
    public bool DomeRecycler;
}

private class SpawnData
{
    public SpawnData(Vector3 position, Vector3 rotation);
    public Vector3 Position { get; set; }
    public Vector3 Rotation { get; set; }
}


```

---

## MonumentTeleporter by misticos - Teleport to monuments found with Monument Finder

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Monument Teleporter", "Iv Misticos", "1.0.1")]
[Description("Teleport to monuments found with Monument Finder")]
 class MonumentTeleporter : CovalencePlugin
{
    private const string PermissionUse;
    [PluginReference]
    private Plugin MonumentFinder;
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Command")]
        public string Command;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void Init();
    private void CommandTeleport(IPlayer player, string command, string[] args);
    private string GetMsg(string key, string userId);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Command")]
    public string Command;
}


```

---

## MoreResourcesTeam by  - Get more resources when farming close to a team member

```csharp
using UnityEngine;
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core;

Oxide.Plugins
[Info("More Resources Team", "Krungh Crow", "1.1.1")]
[Description("Get more resources when you're farming close to a team member")]
 class MoreResourcesTeam : CovalencePlugin
{
    const string Use_Perm;
    const string Vip_Perm;
     void Init();
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty("Distance between you and your mates (in feet)")]
        public float distanceMate;
        [JsonProperty("Bonus percentage Default")]
        public int bonusMate;
        [JsonProperty("Bonus percentage Vip")]
        public int bonusMateVip;
    }

    private bool LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConf();
    protected override void LoadDefaultMessages();
     string GetMessage(string key, string steamId);
    private void notifyPlayer(IPlayer player, string text);
     void OnDispenserGather(ResourceDispenser dispenser, BasePlayer player, Item item);
}

 class ConfigData
{
    [JsonProperty("Distance between you and your mates (in feet)")]
    public float distanceMate;
    [JsonProperty("Bonus percentage Default")]
    public int bonusMate;
    [JsonProperty("Bonus percentage Vip")]
    public int bonusMateVip;
}


```

---

## MountComputerStation by YaMang - View computer stations with commands

```csharp
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Mount Computer Station", "YaMang -w-", "1.1.2")]
[Description("View computer stations with commands!")]
public class MountComputerStation : RustPlugin
{
     Dictionary<BasePlayer, Vector3> SetLocation;
     Dictionary<BasePlayer, BaseEntity> _station;
    private string UsePermission;
    private void OnServerInitialized();
    private void Unload();
    private void ChatStation(BasePlayer player, string command, string[] args);
    private void ConsoleStation(ConsoleSystem.Arg arg);
    private void SetComputerStation(BasePlayer player);
    [ConsoleCommand("scsdismount")]
    private void ScsDismount(ConsoleSystem.Arg arg);
    private void OnEntityDismounted(BaseMountable entity, BasePlayer player);
    private void ScsDestory(BaseMountable entity, BasePlayer player);
    private void Messages(BasePlayer player, string text);
    private ConfigData _config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Prefix")]
        public string Prefix { get; set; }
        [JsonProperty(PropertyName = "SteamID")]
        public string SteamID { get; set; }
        [JsonProperty(PropertyName = "Commands")]
        public List<string> commands { get; set; }
        [JsonProperty(PropertyName = "Location Station")]
        public LocationStation locationStation { get; set; }
        [JsonProperty(PropertyName = "Auto Add Station CCTVs")]
        public List<string> addcctvs { get; set; }
        [JsonProperty(PropertyName = "UI Settings")]
        public UISettings uiSettings { get; set; }
        public Oxide.Core.VersionNumber Version { get; set; }
    }

    public class UISettings
    {
        public string oMin { get; set; }
        public string oMax { get; set; }
        public string Color { get; set; }
    }

    public class LocationStation
    {
        [JsonProperty(PropertyName = "Summoned from player position (If false, use location)")]
        public bool PlayerLocation;
        public Vector3 location;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    protected override void LoadDefaultMessages();
    private string Lang(string key, object[] args);
    private void ExitUI(BasePlayer player);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Prefix")]
    public string Prefix { get; set; }
    [JsonProperty(PropertyName = "SteamID")]
    public string SteamID { get; set; }
    [JsonProperty(PropertyName = "Commands")]
    public List<string> commands { get; set; }
    [JsonProperty(PropertyName = "Location Station")]
    public LocationStation locationStation { get; set; }
    [JsonProperty(PropertyName = "Auto Add Station CCTVs")]
    public List<string> addcctvs { get; set; }
    [JsonProperty(PropertyName = "UI Settings")]
    public UISettings uiSettings { get; set; }
    public Oxide.Core.VersionNumber Version { get; set; }
}

public class UISettings
{
    public string oMin { get; set; }
    public string oMax { get; set; }
    public string Color { get; set; }
}

public class LocationStation
{
    [JsonProperty(PropertyName = "Summoned from player position (If false, use location)")]
    public bool PlayerLocation;
    public Vector3 location;
}


```

---

## MountLogger by BaronVonFinchus - Logs to a file when players mount or dismount an entity

```csharp
using Oxide.Core;
using System;
using System.Linq;
using System.Collections.Generic;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Mount Logger", "BaronVonFinchus", "1.6")]
[Description("A simple plugin to track the location of mounted entities. Logs player name, ID and position to a file. Useful for PvE servers.")]
public class MountLogger : RustPlugin
{
    private PluginConfig _config;
     void Init();
     void OnServerInitialized();
    private void Unload();
    private void PrintToConsole(string message);
    private void PrintToFile(string message);
     string getGrid(Vector3 pos);
     void OnEntityMounted(BaseMountable entity, BasePlayer player);
     void OnEntityDismounted(BaseMountable entity, BasePlayer player);
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    private void LogKey(string key, object[] args);
    private void LogMessage(string message);
    private class DefaultLocalization : Dictionary<string, string>
    {
        public DefaultLocalization();
    }

    private class RuLocalization : Dictionary<string, string>
    {
        public RuLocalization();
    }

    private class PluginConfig
    {
        [JsonProperty("Print logs to console")]
        public bool PrintToConsole { get; set; }
        [JsonProperty("Print logs to file")]
        public bool PrintToFile { get; set; }
    }

    private class PluginData
    {
        public bool FirstStart { get; set; }
    }

}

private class DefaultLocalization : Dictionary<string, string>
{
    public DefaultLocalization();
}

private class RuLocalization : Dictionary<string, string>
{
    public RuLocalization();
}

private class PluginConfig
{
    [JsonProperty("Print logs to console")]
    public bool PrintToConsole { get; set; }
    [JsonProperty("Print logs to file")]
    public bool PrintToFile { get; set; }
}

private class PluginData
{
    public bool FirstStart { get; set; }
}


```

---

## MountRestrictions by jhawins - Restricts equipped gear when mounting entities

```csharp
using System;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Mount Restrictions", "Jhawins", "1.0.2")]
[Description("Restricts equipment when mounting entities based on configuration")]
 class MountRestrictions : CovalencePlugin
{
    protected override void LoadDefaultMessages();
    private string GetMessage(string key, string id);
    private Configuration config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "RestrictionSets", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<RestrictionSet> RestrictionSets { get; set; }
    }

    private Configuration GetDefaultConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
     bool? CanMountEntity(BasePlayer player, BaseMountable entity);
     bool? CanWearItem(PlayerInventory inventory, Item item);
    private int ConfigExceptions;
     bool CheckAnyRestrictionsMatched(List<Item> items, BasePlayer player, BaseMountable mountedEntity, bool alreadyMounted);
}

private class Configuration
{
    [JsonProperty(PropertyName = "RestrictionSets", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<RestrictionSet> RestrictionSets { get; set; }
}


```

---

## MovableCCTV by Bazz3l - Control CCTV cameras using WASD keys when mounted at computer station

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("Movable CCTV", "Bazz3l", "1.1.2")]
[Description("Allow players to control placed cctv cameras using WASD")]
internal class MovableCCTV : CovalencePlugin
{
    private const string PERM_USE;
    private static MovableCCTV _plugin;
    private PluginConfig _config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private class PluginConfig
    {
        public float RotateSpeed;
        public string TextColor;
        public int TextSize;
        public string AnchorMin;
        public string AnchorMax;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private void Init();
    private void Unload();
    private void OnEntityBuilt(Planner plan, GameObject go);
    private void OnBookmarkControlStarted(ComputerStation computerStation, BasePlayer player, string bookmarkName, IRemoteControllable entity);
    private void OnBookmarkControlEnded(ComputerStation station, BasePlayer player, CCTV_RC cctvRc);
    private void CheckCctv();
    private class CctvCamMover : MonoBehaviour
    {
        public static void RemoveAll();
        private ComputerStation _station;
        private BasePlayer _player;
        private void Awake();
        private void FixedUpdate();
        public void DestroyMe();
        public void DestroyImmediate();
        private CCTV_RC GetControlledCctv(ComputerStation computerStation);
    }

    private static class UI
    {
        private const string PANEL_NAME;
        public static void CreateUI(BasePlayer player, string description);
        public static void RemoveUI(BasePlayer player);
        public static void RemoveAll();
    }

    private string Lang(string key, string id, object[] args);
    private bool HasPermission(BasePlayer player);
    private bool BecomeMovableWasBlocked(CCTV_RC cctvRc, BasePlayer player);
}

private class PluginConfig
{
    public float RotateSpeed;
    public string TextColor;
    public int TextSize;
    public string AnchorMin;
    public string AnchorMax;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private class CctvCamMover : MonoBehaviour
{
    public static void RemoveAll();
    private ComputerStation _station;
    private BasePlayer _player;
    private void Awake();
    private void FixedUpdate();
    public void DestroyMe();
    public void DestroyImmediate();
    private CCTV_RC GetControlledCctv(ComputerStation computerStation);
}

private static class UI
{
    private const string PANEL_NAME;
    public static void CreateUI(BasePlayer player, string description);
    public static void RemoveUI(BasePlayer player);
    public static void RemoveAll();
}


```

---

## Murderers by KrunghCrow - Spawn murderers at selected spawn points

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AI;
using Oxide.Core;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Facepunch;
using Rust;
using Rust.Ai;

Oxide.Plugins
[Info("Murderers", "Krungh Crow", "1.1.3")]
[Description("Murderers Reborn")]
 class Murderers : RustPlugin
{
    [PluginReference]
     Plugin Kits;
     ulong chaticon;
     string prefix;
     bool Debug;
    public string Kit;
    public bool Turret_Safe;
    public bool Animal_Safe;
    public bool SpawnScarecrow;
    private const string _PermKill;
    private const string _PermAddLoc;
    private const string _PermSpawn;
    private Dictionary<string, int> murdersPoint;
    private Dictionary<ulong, string> npcCreated;
    private string npcPrefab;
     int npcCoolDown;
     void Init();
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Main config")]
        public SettingsPlugin PlugCFG;
        [JsonProperty(PropertyName = "NPC config")]
        public SettingsNPC NPCCFG;
    }

     class SettingsPlugin
    {
        [JsonProperty(PropertyName = "Debug")]
        public bool Debug;
        [JsonProperty(PropertyName = "Chat Steam64ID")]
        public ulong Chaticon;
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string Prefix;
    }

     class SettingsNPC
    {
        [JsonProperty(PropertyName = "Spawn as Scarecrow (displayname only)")]
        public bool Scarecrow;
        [JsonProperty(PropertyName = "Health (HP)")]
        public int NPCHealth;
        [JsonProperty(PropertyName = "Respawn time (seconds)")]
        public int RespawnTime;
        [JsonProperty(PropertyName = "AnimalSafe")]
        public bool _AnimalSafe;
        [JsonProperty(PropertyName = "TurretSafe")]
        public bool _TurretSafe;
        [JsonProperty(PropertyName = "Kit ID")]
        public List<string> KitName;
    }

    private bool LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConf();
    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private void Unload();
    private class Zombies : FacepunchBehaviour
    {
        private ScarecrowNPC npc;
        public bool ReturningToHome;
        public bool isRoaming;
        public Vector3 SpawnPoint;
        private void Awake();
        private void Attack();
        public void _UseBrain();
         void GoHome();
        private void SettargetDestination(Vector3 position);
         void WipeMemory();
         void OnDestroy();
    }

     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     object CanBeTargeted(ScarecrowNPC target, MonoBehaviour turret);
     object CanNpcAttack(BaseNpc npc, BaseEntity target);
     object OnNpcKits(BasePlayer player);
     void startSpawn(string position);
     void Kill();
    private void Reload();
    [ConsoleCommand("m.spawn")]
     void CmdSpawn(ConsoleSystem.Arg arg);
    [ConsoleCommand("m.kill")]
     void CmdBotKill(ConsoleSystem.Arg arg);
    [ChatCommand("m.spawn")]
     void npcSpawn(BasePlayer player);
    [ChatCommand("m.point")]
     void npcMain(BasePlayer player, string command, string[] args);
    [ChatCommand("m.kill")]
     void npcKill(BasePlayer player, string command, string[] args);
    private string msg(string key, string id);
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Main config")]
    public SettingsPlugin PlugCFG;
    [JsonProperty(PropertyName = "NPC config")]
    public SettingsNPC NPCCFG;
}

 class SettingsPlugin
{
    [JsonProperty(PropertyName = "Debug")]
    public bool Debug;
    [JsonProperty(PropertyName = "Chat Steam64ID")]
    public ulong Chaticon;
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string Prefix;
}

 class SettingsNPC
{
    [JsonProperty(PropertyName = "Spawn as Scarecrow (displayname only)")]
    public bool Scarecrow;
    [JsonProperty(PropertyName = "Health (HP)")]
    public int NPCHealth;
    [JsonProperty(PropertyName = "Respawn time (seconds)")]
    public int RespawnTime;
    [JsonProperty(PropertyName = "AnimalSafe")]
    public bool _AnimalSafe;
    [JsonProperty(PropertyName = "TurretSafe")]
    public bool _TurretSafe;
    [JsonProperty(PropertyName = "Kit ID")]
    public List<string> KitName;
}

private class Zombies : FacepunchBehaviour
{
    private ScarecrowNPC npc;
    public bool ReturningToHome;
    public bool isRoaming;
    public Vector3 SpawnPoint;
    private void Awake();
    private void Attack();
    public void _UseBrain();
     void GoHome();
    private void SettargetDestination(Vector3 position);
     void WipeMemory();
     void OnDestroy();
}


```

---

## MushroomEffects by supreme - Displays colorful/shaking/blur effects to players when eating mushrooms

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("Mushroom Effects", "supreme", "1.1.2")]
[Description("Make mushroom eating fun and add effects")]
public class MushroomEffects : RustPlugin
{
    private PluginConfig _pluginConfig;
    private readonly Hash<string, Effect> _cachedEffects;
    private readonly Hash<ulong, List<Timer>> _cachedTimers;
    private readonly Hash<ulong, int> _cachedUsedMushrooms;
    private readonly System.Random _random;
    private const string UiBlur;
    private const string UiEffects;
    private const string HardShakeEffectPrefab;
    private const string SoftShakeEffectPrefab;
    private const string VomitEffetPrefab;
    private const string BreatheEffectPrefab;
    private const string UsePermission;
    private const int MushroomItemId;
    private string _cachedUi;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnItemUse(Item item, int amount);
    private void OnPlayerDeath(BasePlayer player, HitInfo hitInfo);
    private void OnPlayerDisconnected(BasePlayer player);
    private void DisplayUiBlur(BasePlayer player);
    private void DisplayUiEffects(BasePlayer player);
    private List<Timer> GetCachedTimers(ulong playerId);
    private void CacheUiBlur();
    private void SendEffectTo(string effectPrefab, BasePlayer player);
    private void DestroyMushroomEffects(BasePlayer player);
    private void DestroyTimers(ulong playerId);
    private string GetRandomColor();
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Duration of the effects")]
        public float EffectsDuration { get; set; }
        [JsonProperty(PropertyName = "Minimum Amount Of Mushrooms required to trigger the effect")]
        public int MinUsed { get; set; }
        [JsonProperty(PropertyName = "Maximum Amount Of Mushrooms required to trigger the effect")]
        public int MaxUsed { get; set; }
        [JsonProperty(PropertyName = "Opacity of the colors")]
        public float ColorOpacity { get; set; }
        [JsonProperty(PropertyName = "Enable vomit effect")]
        public bool EnableVomitEffect { get; set; }
        [JsonProperty(PropertyName = "Enable breath effect")]
        public bool EnableBreathEffect { get; set; }
        [JsonProperty(PropertyName = "Enable shake effect")]
        public bool EnableShakeEffect { get; set; }
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
}

private class PluginConfig
{
    [JsonProperty(PropertyName = "Duration of the effects")]
    public float EffectsDuration { get; set; }
    [JsonProperty(PropertyName = "Minimum Amount Of Mushrooms required to trigger the effect")]
    public int MinUsed { get; set; }
    [JsonProperty(PropertyName = "Maximum Amount Of Mushrooms required to trigger the effect")]
    public int MaxUsed { get; set; }
    [JsonProperty(PropertyName = "Opacity of the colors")]
    public float ColorOpacity { get; set; }
    [JsonProperty(PropertyName = "Enable vomit effect")]
    public bool EnableVomitEffect { get; set; }
    [JsonProperty(PropertyName = "Enable breath effect")]
    public bool EnableBreathEffect { get; set; }
    [JsonProperty(PropertyName = "Enable shake effect")]
    public bool EnableShakeEffect { get; set; }
}


```

---

## MuteExplosions by 1AK1 - Mutes explosion sounds for everyone but the initiator

```csharp
using System.Collections.Generic;
using System.ComponentModel;
using Facepunch;
using Network;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Mute Explosions", "1AK1/MJSU", "1.0.3")]
[Description("Mutes explosion sounds for everyone but the initiator")]
internal class MuteExplosions : CovalencePlugin
{
    private PluginConfig _pluginConfig;
    private const string UsePermission;
    private const string BypassPermission;
    private readonly Hash<uint, string> _explosionEffect;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    public PluginConfig AdditionalConfig(PluginConfig config);
    private void OnEntitySpawned(TimedExplosive explosive);
    private void OnEntityKill(TimedExplosive explosive);
    private void SendEffect(Effect effect, List<Connection> connections);
    private bool HasPermission(BasePlayer player, string perm);
    public class PluginConfig
    {
        [DefaultValue(true)]
        [JsonProperty(PropertyName = "Send to explosive owner")]
        public bool SendToOwner { get; set; }
        [DefaultValue(0)]
        [JsonProperty(PropertyName = "Send to in range (Meters)")]
        public float SendToRange { get; set; }
    }

}

public class PluginConfig
{
    [DefaultValue(true)]
    [JsonProperty(PropertyName = "Send to explosive owner")]
    public bool SendToOwner { get; set; }
    [DefaultValue(0)]
    [JsonProperty(PropertyName = "Send to in range (Meters)")]
    public float SendToRange { get; set; }
}


```

---

## MyBirthdayCake by KrunghCrow - Throw a birthday cake bomb

```csharp
using UnityEngine;
using System.Collections.Generic;
using System.Linq;
using System;
using Convert = System.Convert;
using Oxide.Core;

Oxide.Plugins
[Info("My Birthday Cake", "Krungh Crow", "1.1.0")]
[Description("Throw a Birthday Cake Bomb")]
public class MyBirthdayCake : RustPlugin
{
     bool debug;
    private bool ConfigChanged;
     string Prefix;
     ulong ChatIcon;
     float flamingtime;
     string cakename;
    const string Cake_Perm;
     class StoredData
    {
        public Dictionary<ulong, Cake> Cakes;
        public StoredData();
    }

    private StoredData storedData;
     class Cake
    {
        public ulong playerownerID;
        public bool explosion;
        public bool fire;
        public bool oilflames;
        public bool napalmflames;
        public bool flameonthrow;
    }

     void Init();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private object GetConfig(string menu, string datavalue, object defaultValue);
    [ChatCommand("cake")]
    private void Cake_Permer(BasePlayer player, string command, string[] args);
     void Unload();
     void OnMeleeThrown(BasePlayer player, Item item);
     void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
     void DelaydCakeTrigger(ulong cakeid, Item item);
     void CakeRunTrigger(Item item, string birthdaytype, BaseEntity entity);
    private void CreateCake(BasePlayer player, string[] args);
}

 class StoredData
{
    public Dictionary<ulong, Cake> Cakes;
    public StoredData();
}

 class Cake
{
    public ulong playerownerID;
    public bool explosion;
    public bool fire;
    public bool oilflames;
    public bool napalmflames;
    public bool flameonthrow;
}


```

---

## MyCH47 by FastBurst - Allows players to spawn a CH47 helicopter

```csharp
using UnityEngine;
using System.Collections.Generic;
using Oxide.Core;
using Convert = System.Convert;
using System.Linq;
using Newtonsoft.Json;

Oxide.Plugins
[Info("My CH47", "FastBurst", "1.0.2")]
[Description("Spawn a CH47 Helicopter")]
public class MyCH47 : RustPlugin
{
    private const string PREFAB_CH47;
    private const string CH47spawn;
    private const string CH47cooldown;
    private bool normalch47kill;
    private float trigger;
    private Timer clock;
    public Dictionary<BasePlayer, BaseVehicle> baseplayerch47;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnEntitySpawned(BaseNetworkable entity);
    private void OnEntityKill(BaseNetworkable entity);
    private void ChatPlayerOnline(ulong ailldi, string message);
    private void LetsClock();
    [ChatCommand("mych47")]
    private void SpawnMyCH47(BasePlayer player, string command, string[] args);
    [ChatCommand("noch47")]
    private void KillMyCH47(BasePlayer player, string command, string[] args);
    private static ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Chat Settings")]
        public ChatOptions ChatSettings { get; set; }
        [JsonProperty(PropertyName = "Cooldown (on permissions)")]
        public CooldownOptions CoolSettings { get; set; }
        [JsonProperty(PropertyName = "Debris Settings")]
        public DebrisOptions DebrisSettings { get; set; }
        [JsonProperty(PropertyName = "Debug Option")]
        public DebugOptions DebugSettings { get; set; }
        public class ChatOptions
        {
            [JsonProperty(PropertyName = "ChatColor")]
            public string ChatColor { get; set; }
            [JsonProperty(PropertyName = "Prefix")]
            public string Prefix { get; set; }
            [JsonProperty(PropertyName = "Prefix Color")]
            public string PrefixColor { get; set; }
            [JsonProperty(PropertyName = "Chat Icon")]
            public ulong SteamIDIcon { get; set; }
        }

        public class CooldownOptions
        {
            [JsonProperty(PropertyName = "Value in minutes")]
            public float cooldownmin { get; set; }
        }

        public class DebrisOptions
        {
            [JsonProperty(PropertyName = "Remove debris")]
            public bool withoutdebris { get; set; }
        }

        public class DebugOptions
        {
            [JsonProperty(PropertyName = "Enable debug Messages in console")]
            public bool debug { get; set; }
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
     T GetConfig(string name, T defaultValue);
    protected override void LoadDefaultMessages();
     class StoredData
    {
        public Dictionary<ulong, ulong> playerch47;
        public Dictionary<ulong, float> playercounter;
        public StoredData();
    }

    private StoredData storedData;
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Chat Settings")]
    public ChatOptions ChatSettings { get; set; }
    [JsonProperty(PropertyName = "Cooldown (on permissions)")]
    public CooldownOptions CoolSettings { get; set; }
    [JsonProperty(PropertyName = "Debris Settings")]
    public DebrisOptions DebrisSettings { get; set; }
    [JsonProperty(PropertyName = "Debug Option")]
    public DebugOptions DebugSettings { get; set; }
    public class ChatOptions
    {
        [JsonProperty(PropertyName = "ChatColor")]
        public string ChatColor { get; set; }
        [JsonProperty(PropertyName = "Prefix")]
        public string Prefix { get; set; }
        [JsonProperty(PropertyName = "Prefix Color")]
        public string PrefixColor { get; set; }
        [JsonProperty(PropertyName = "Chat Icon")]
        public ulong SteamIDIcon { get; set; }
    }

    public class CooldownOptions
    {
        [JsonProperty(PropertyName = "Value in minutes")]
        public float cooldownmin { get; set; }
    }

    public class DebrisOptions
    {
        [JsonProperty(PropertyName = "Remove debris")]
        public bool withoutdebris { get; set; }
    }

    public class DebugOptions
    {
        [JsonProperty(PropertyName = "Enable debug Messages in console")]
        public bool debug { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class ChatOptions
{
    [JsonProperty(PropertyName = "ChatColor")]
    public string ChatColor { get; set; }
    [JsonProperty(PropertyName = "Prefix")]
    public string Prefix { get; set; }
    [JsonProperty(PropertyName = "Prefix Color")]
    public string PrefixColor { get; set; }
    [JsonProperty(PropertyName = "Chat Icon")]
    public ulong SteamIDIcon { get; set; }
}

public class CooldownOptions
{
    [JsonProperty(PropertyName = "Value in minutes")]
    public float cooldownmin { get; set; }
}

public class DebrisOptions
{
    [JsonProperty(PropertyName = "Remove debris")]
    public bool withoutdebris { get; set; }
}

public class DebugOptions
{
    [JsonProperty(PropertyName = "Enable debug Messages in console")]
    public bool debug { get; set; }
}

 class StoredData
{
    public Dictionary<ulong, ulong> playerch47;
    public Dictionary<ulong, float> playercounter;
    public StoredData();
}


```

---

## MyDigicode by  - Set a unique code for all codelocks, with autolock and remote options

```csharp
using System.Collections.Generic;
using System;
using Oxide.Core;
using System.Linq;

Oxide.Plugins
[Info("My Digicode", "BuzZ", "1.0.1")]
[Description("Set a unique code for all codelocks, with autolock and remote options")]
public class MyDigicode : RustPlugin
{
     bool debug;
    const string Digicoder;
    const string AutoRemote;
    const string LockRemote;
    private bool ConfigChanged;
     bool loaded;
     string Prefix;
     string PrefixColor;
     ulong SteamIDIcon;
     bool destroy_info;
     class StoredData
    {
        public Dictionary<ulong, Digicode> playerdigicode;
        public StoredData();
    }

    private StoredData storedData;
    public class Digicode
    {
        public string digicode;
        public bool autolock;
        public List<uint> uniques;
    }

     void Init();
     void OnServerInitialized();
     void Unload();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private object GetConfig(string menu, string datavalue, object defaultValue);
     void TheWorldIsLocked();
     void ResetAllCounters();
     void MyWorldIsLocked(ulong playerID, string reason);
     void ResetMyCounter(ulong playerID);
    private string CodeRandomizor();
     void OnEntitySpawned(BaseNetworkable entity);
     void OneDigicodeAnalyze(CodeLock dacodelock, BaseEntity parent, string reason);
    [ChatCommand("digi")]
     void MyDigicodeChatCommand(BasePlayer player, string command, string[] args);
     void OnEntityKill(BaseNetworkable Entity);
}

 class StoredData
{
    public Dictionary<ulong, Digicode> playerdigicode;
    public StoredData();
}

public class Digicode
{
    public string digicode;
    public bool autolock;
    public List<uint> uniques;
}


```

---

## MyHotAirBalloon by  - Spawns a hot air balloon for yourself, HUD for status, speed setting and more

```csharp
using UnityEngine;
using System.Collections.Generic;
using Oxide.Core;
using Convert = System.Convert;
using System.Linq;
using Oxide.Game.Rust.Cui;
using System;

Oxide.Plugins
[Info("My Hot Air Balloon", "BuzZ[PHOQUE]", "0.1.3")]
[Description("Spawn a Hot Air Balloon")]
public class MyHotAirBalloon : RustPlugin
{
     bool debug;
    private string DaBalloonBar;
    private string DaBalloonBarHealth;
    private string DaBalloonBarWind;
    private string DaBalloonBarInflation;
    private string DaBalloonBarLift;
    private string DaBalloonBarFuel;
    private string DaBalloonBarLock;
    private string DaBalloonBarupboost;
    private string DaBalloonNavigatorBar;
    private string DaBalloonNavigatorBarWest;
    private string DaBalloonNavigatorBarEast;
    private string DaBalloonNavigatorBarNorth;
    private string DaBalloonNavigatorBarSouth;
    private string DaBalloonBombBar;
     string Prefix;
     string PrefixColor;
     string ChatColor;
     ulong SteamIDIcon;
     float fuelrate;
     double barbottom;
    const string prefab;
     string lightprefab;
     string stockprefab;
    private bool ConfigChanged;
    const string Ballooner;
    const string UnBallooner;
    const string GodBalloon;
    const string FuelRateBalloon;
    const string BalloonTP;
    const string Navigator;
    const string Bomber;
    public Dictionary<BasePlayer, HotAirBalloon > baseplayerballoon;
    public Dictionary<HotAirBalloon, Timer > balloontimer;
    public List<BasePlayer> NavigateMode;
    public Dictionary<HotAirBalloon, Vector3 > balloonposition;
    public List<BasePlayer> BalloonWest;
    public List<BasePlayer> BalloonEast;
    public List<BasePlayer> BalloonNorth;
    public List<BasePlayer> BalloonSouth;
     class StoredData
    {
        public Dictionary<ulong, uint> playerballoon;
        public StoredData();
    }

    private StoredData storedData;
     void Init();
     void OnServerInitialized();
     void Unload();
     void PluginReload();
     void OnPlayerSleepEnded(BasePlayer player);
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private object GetConfig(string menu, string datavalue, object defaultValue);
     BaseEntity SpawnEntity(string prefab, bool active, int angx, int angy, int angz, float posx, float posy, float posz, BaseEntity parent, ulong skinid);
     void RefreshPosition(BaseEntity entity);
    [ChatCommand("balloon_tp")]
    private void TeleportTo(BasePlayer player, string command, string[] args);
    [ChatCommand("balloon_invite")]
    private void InviteAFriend(BasePlayer player, string command, string[] args);
    [ChatCommand("balloon_lock")]
    private void LockMyBalloon(BasePlayer player, string command, string[] args);
     void MyBallonBombHUD(HotAirBalloon Balloon, BaseEntity Loloon, BasePlayer player);
    [ConsoleCommand("DropABombFromMyBalloon")]
    private void MyBallonBombDrop(ConsoleSystem.Arg arg);
    private void NavigateMyBalloon(HotAirBalloon Balloon, BaseEntity Loloon, BasePlayer player);
     void NavigatorBarHUD(HotAirBalloon Balloon, BaseEntity Loloon, BasePlayer player);
    [ConsoleCommand("NavigateMyHotAirBalloon")]
    private void NavigateMyHotAirBalloon(ConsoleSystem.Arg arg);
     float RandomAFloatBuddy();
    [ConsoleCommand("SpawnMyHotAirBalloon")]
    private void SpawnMyHotAirBalloonConsole(ConsoleSystem.Arg arg);
    [ChatCommand("balloon")]
    private void SpawnMyBalloonChatCommand(BasePlayer player, string command, string[] args);
    private void SpawnMyBalloon(BasePlayer player, string command, string[] args);
    [ConsoleCommand("DespawnMyHotAirBalloon")]
    private void DespawnMyHotAirBalloonConsole(ConsoleSystem.Arg arg);
    [ChatCommand("noballoon")]
    private void KillMyBalloonChatCommand(BasePlayer player, string command, string[] args);
    private void KillMyBalloon(BasePlayer player, string command, string[] args);
    private void ChatPlayerOnline(ulong ailldi, string message);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     void NullMHABDamage(BaseCombatEntity entity, HitInfo hitInfo);
     void BalloonBar(BasePlayer player);
    private void BalloonBarButtons(BasePlayer player, float fuelpersec, float inflation, float lift, float windforce, float entityhealth);
    [ConsoleCommand("BalloonMinus")]
    private void BalloonMinus(ConsoleSystem.Arg arg);
    [ConsoleCommand("BalloonPlus")]
    private void BalloonPlus(ConsoleSystem.Arg arg);
    [ConsoleCommand("BalloonBoost")]
    private void BalloonUpBoost(ConsoleSystem.Arg arg);
     void OnPlayerDisconnected(BasePlayer player, string reason);
    private void BalloonTimer(HotAirBalloon Balloon, BaseEntity Loloon, BasePlayer player);
     void OnEntityKill(BaseNetworkable entity);
}

 class StoredData
{
    public Dictionary<ulong, uint> playerballoon;
    public StoredData();
}


```

---

## MyNPC by Lincoln - Spawn your own player NPCs and access their inventories

```csharp
using System.Collections.Generic;
using Oxide.Core;
using System.Linq;
using Newtonsoft.Json;
using UnityEngine;
using Oxide.Core.Libraries.Covalence;
using Rust;

Oxide.Plugins
[Info("My NPC", "Lincoln", "1.1.0")]
[Description("Spawn your own player NPCs and access their inventories!")]
 class MyNPC : RustPlugin
{
    private readonly Hash<string, float> cooldowns;
     float cooldownTime;
    private const string npcEntity;
     Dictionary<string, ulong> npcList;
    private static NPCPlayer baseNPC;
    private const string permUse;
    private const string permBypassCooldown;
    private const string permMultipleNPC;
    private void Init();
    private void OnPlayerConnected();
    private bool hasPermission(BasePlayer player);
    private void Unload();
    private void DestroyNPC(BasePlayer player, NPCPlayer npc);
     object GetActiveNPC(BasePlayer player);
    private void SpawnNPC(BasePlayer player);
     void RecallNPC(BasePlayer player, NPCPlayer npc);
     void GoToNPC(BasePlayer player, NPCPlayer npc);
     void HealNPC(BasePlayer player, NPCPlayer npc);
     void NPCStatus(BasePlayer player, NPCPlayer npc);
     bool OnCoolDown(BasePlayer player);
    private void ViewInventory(BasePlayer player, BasePlayer targetplayer);
    private void StartLooting(BasePlayer player, BasePlayer targetplayer, LootableCorpse corpse);
    private class PluginConfig
    {
        public int Cooldown;
    }

    private PluginConfig config;
    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
    [ChatCommand("npc.list")]
    private void ListCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("npc")]
    private void NPCCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("npc.help")]
    private void HelpCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("npc.add")]
    private void NPCAddCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("npc.kill")]
    private void KillNPCCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("npc.heal")]
    private void HealNPCCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("npc.status")]
    private void CheckNPCCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("npc.inv")]
    private void ViewInvCmd(BasePlayer player, string command, string[] args);
    [ChatCommand("npc.recall")]
    private void RecallNPCCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("npc.goto")]
    private void TeleportNPCCommand(BasePlayer player, string command, string[] args);
     void OnEntityKill(BaseEntity entity);
     void OnEntityTakeDamage(NPCPlayer npc, HitInfo info);
    private bool PositionIsInWater(Vector3 position);
     bool ValidationChecks(BasePlayer player);
    private void ChatMessage(BasePlayer player, string messageName, object[] args);
    private string GetMessage(IPlayer player, string messageName, object[] args);
    private string GetMessage(string playerId, string messageName, object[] args);
    protected override void LoadDefaultMessages();
}

private class PluginConfig
{
    public int Cooldown;
}


```

---

## MySnowmobile by MasterSplinter - Let players purchase Tomaha or skin to via command, also allows for Snowmobile to go fast anywhere!

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Libraries;
using Newtonsoft.Json;
using Newtonsoft.Json.Serialization;
using UnityEngine;

Oxide.Plugins
[Info("My Snowmobile", "MasterSplinter", "1.0.3")]
[Description("Let players purchase Tomaha or skin to via command, also allows for Snowmobile to go fast anywhere!")]
 class MySnowmobile : RustPlugin
{
    private bool debug;
    private string debugversion;
     string Prefix;
     string PrefixColor;
     ulong SteamIDIcon;
    private readonly string UseSkinPerm;
    private readonly string UseBuyPerm;
    private List<VehicleCache> vehicles;
    private List<BasePlayer> cooldown;
    public class VehicleCache
    {
        public BaseEntity entity;
    }

    private class StoredData
    {
        public Dictionary<ulong, int> CurrentFuel;
        public StoredData();
    }

     StoredData storedData;
    private void SaveData();
    private class ConfigFile
    {
        [JsonProperty(PropertyName = "Main Settings")]
        public MainSettings Main;
        [JsonProperty(PropertyName = "Advance Settings")]
        public AdvanceSettings Advance;
    }

    public class MainSettings
    {
        [JsonProperty("Allow Snowmobile's to goes fast on all terrain types")]
        public bool AllTerrain;
        [JsonProperty("Allow Snowmobile's to have more power")]
        public bool IncreasedPower;
        [JsonProperty("The amount of power to give in KW (59KW is default)")]
        public int PowerKW;
        [JsonProperty("The amount of Scrap to charge for a Snowmobile")]
        public int BuyCost;
        [JsonProperty("The amount of fuel to give on purchase")]
        public int FuelOnBuy;
        [JsonProperty("The amount of seconds in between purchase (0 = Instant)")]
        public int CooldownTime;
    }

    public class AdvanceSettings
    {
        [JsonProperty("Tomaha Location")]
        public string Tomaha;
        [JsonProperty("Spray Cloud Location")]
        public string SprayCloudEffect;
        [JsonProperty("Spray Sound Location")]
        public string SpraySoundEffect;
    }

    private ConfigFile config;
    protected override void LoadDefaultConfig();
    private void OnServerInitialized(bool isStartup);
    private void RegisterCommands();
    private void RegisterPermissions();
    private void Init();
    private void Unload();
    protected override void LoadDefaultMessages();
    private void OnEntitySpawned(BaseMountable entity);
    public int GetFuelAmount(VehicleCache vehicle);
    private void LogFuelAmount(BasePlayer player, VehicleCache vehicle);
    private void RemoveLoggedFuel(BasePlayer player, VehicleCache vehicle);
    private void AddFuel(Snowmobile snowmobile, string userId);
    private void AddFuelOnBuy(Snowmobile snowmobile);
    private BaseEntity GetLookAtEntity(BasePlayer player, float maxDist);
    private void SkinSnowmobile(IPlayer p, VehicleCache vehicle, Vector3 customPosition, bool useCustomPosition);
    private void BuyTomaha(IPlayer p, Vector3 customPosition, bool useCustomPosition);
     void SprayEffect(BasePlayer player, Vector3 customPosition, bool useCustomPosition);
    private bool IsDamagedEntity(BaseEntity entity);
    private Vector3 GetPosition(BasePlayer player);
    private Quaternion GetRotation(BasePlayer player);
    private void IncreasedPower(Snowmobile snowmobile);
    private void AllTerrain(Snowmobile snowmobile);
    private void RevertToStock(Snowmobile snowmobile);
    private void ProcessExistingSnowmobile();
    private void RevertExistingSnowmobile();
    private Dictionary<string, string> Commands;
    private void SkinSnowmobileCmd(IPlayer p, string command, string[] args);
    private void BuySnowmobileCmd(IPlayer p, string command, string[] args);
}

public class VehicleCache
{
    public BaseEntity entity;
}

private class StoredData
{
    public Dictionary<ulong, int> CurrentFuel;
    public StoredData();
}

private class ConfigFile
{
    [JsonProperty(PropertyName = "Main Settings")]
    public MainSettings Main;
    [JsonProperty(PropertyName = "Advance Settings")]
    public AdvanceSettings Advance;
}

public class MainSettings
{
    [JsonProperty("Allow Snowmobile's to goes fast on all terrain types")]
    public bool AllTerrain;
    [JsonProperty("Allow Snowmobile's to have more power")]
    public bool IncreasedPower;
    [JsonProperty("The amount of power to give in KW (59KW is default)")]
    public int PowerKW;
    [JsonProperty("The amount of Scrap to charge for a Snowmobile")]
    public int BuyCost;
    [JsonProperty("The amount of fuel to give on purchase")]
    public int FuelOnBuy;
    [JsonProperty("The amount of seconds in between purchase (0 = Instant)")]
    public int CooldownTime;
}

public class AdvanceSettings
{
    [JsonProperty("Tomaha Location")]
    public string Tomaha;
    [JsonProperty("Spray Cloud Location")]
    public string SprayCloudEffect;
    [JsonProperty("Spray Sound Location")]
    public string SpraySoundEffect;
}


```

---

## MySQLWhitelist by rostov114 - Restricts server access to only those whitelisted in a database

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Newtonsoft.Json;
using System.Linq;

Oxide.Plugins
[Info("MySQL Whitelist", "Sonny-Boi", "1.0.0")]
[Description("Restricts server access to only those whitelisted in a database")]
internal class MySQLWhitelist : RustPlugin
{
    private Configuration config;
     class Configuration
    {
        [JsonProperty("host")]
        public string host;
        [JsonProperty("port")]
        public ulong port;
        [JsonProperty("username")]
        public string username;
        [JsonProperty("password")]
        public string password;
        [JsonProperty("database")]
        public string database;
        public static Configuration DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void Init();
    private readonly Core.MySql.Libraries.MySql _mySql;
    private Core.Database.Connection _mySqlConnection;
    public List<string> whitelistedPlayers;
    private const string SelectData;
    private const string CreateQuery;
    private void OnServerInitialized();
    private bool isWhitelisted(string id);
    private void CheckIfWhitelistedOrKick(BasePlayer player);
    private void OnServerSave();
     object CanUserLogin(string name, string id);
    private new void LoadDefaultMessages();
    private string Lang(string key, string userId, object[] args);
}

 class Configuration
{
    [JsonProperty("host")]
    public string host;
    [JsonProperty("port")]
    public ulong port;
    [JsonProperty("username")]
    public string username;
    [JsonProperty("password")]
    public string password;
    [JsonProperty("database")]
    public string database;
    public static Configuration DefaultConfig();
}


```

---

## MyTransit by Lincoln - Spawn trains and work carts and change the speed of them!

```csharp
using Facepunch;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("My Transit", "Lincoln & Unboxing", "1.5.4")]
[Description("Spawns work carts and makes them go fast")]
 class MyTransit : RustPlugin
{
    private readonly Dictionary<ulong, string> _playerTransit;
    private const string PermUse;
    private const string PermAdmin;
    private const string WorkcartEntity;
    private const string TrainEntity;
    private const string TrainEntity2;
    private const string TrainEntity3;
    private const string TrainEntity4;
    private const int ItemFuelID;
    private List<string> allTrains;
    private Timer _speedTimer;
    private PluginConfig _config;
    private class PluginConfig
    {
        public int forwardSpeed;
        public int reverseSpeed;
        public bool useFuel;
    }

    private void Init();
    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
    private void OnServerInitialized();
    private void OnEntityKill(BaseEntity entity);
    private void Unload();
    private void SpawnTransit(BasePlayer player, string command, string[] args);
    private bool PositionIsInWater(Vector3 position);
    private void RecallTransit(BasePlayer player, BaseEntity entity);
    private void ChangeSpeed(BasePlayer player, int currentSpeed);
    private TrainCar GetActiveTransit(BasePlayer player);
    public bool TryFindTrackNearby(Vector3 pos, float maxDist, TrainTrackSpline splineResult, float distResult);
    private bool ValidationChecks(BasePlayer player);
    [ChatCommand("mtlist")]
    private void ListCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("mtcheck")]
    private void CheckCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("mtspeed")]
    private void SpeedCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("mtspeed.off")]
    private void OffCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("mthelp")]
    private void HelpCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("mtspawn")]
    private void SpawnCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("mtkill")]
    private void KillCommand(BasePlayer player, string command, string[] args);
    private void OnEntityTakeDamage(TrainCar trainCar, HitInfo info);
    private void DestroyTransit(BasePlayer player, TrainCar transit);
    private string GetLang(string langKey, string playerId);
    protected override void LoadDefaultMessages();
}

private class PluginConfig
{
    public int forwardSpeed;
    public int reverseSpeed;
    public bool useFuel;
}


```

---

## NameChanger by birthdates - Listen for profile name changes

```csharp
using System;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Name Changer", "birthdates", "1.0.1")]
[Description("Listen for profile name changes")]
public class NameChanger : CovalencePlugin
{
    private void OnServerInitialized();
    private const string ProfileURL;
    private Regex NameRegex { get; set; }
    private void CheckProfiles();
    private void CheckProfile(IPlayer player);
    private void HandleProfile(IPlayer player, int code, string response);
    private ConfigFile _config;
    protected override void LoadConfig();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class ConfigFile
    {
        [JsonProperty("Check Interval (seconds)")]
        public float CheckInterval { get; set; }
        public static ConfigFile DefaultConfig();
    }

}

private class ConfigFile
{
    [JsonProperty("Check Interval (seconds)")]
    public float CheckInterval { get; set; }
    public static ConfigFile DefaultConfig();
}


```

---

## NameManager by Ankawi - Easily manage player names on your server

```csharp
using Oxide.Core;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("NameManager", "Ankawi", "1.0.1")]
[Description("Manage names on your server")]
 class NameManager : CovalencePlugin
{
    new void LoadConfig();
    protected override void LoadDefaultConfig();
     void Loaded();
     object CanUserLogin(string name, string id, string ip);
     void LoadDefaultMessages();
     void SetConfig(object[] args);
     string GetMsg(string key, object id);
}


```

---

## NameRewards by misticos - Assigns players a group based on phrases found in their names

```csharp
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("NameRewards", "Kappasaurus", "1.0.0", ResourceId = 0)]
[Description("Adds players to a group based on phrases in their name")]
 class NameRewards : CovalencePlugin
{
     ConfigData config;
     class ConfigData
    {
        public string Group { get; set; }
        public string[] Phrases { get; set; }
    }

    protected override void LoadDefaultConfig();
     void Init();
     void OnUserConnected(IPlayer player);
}

 class ConfigData
{
    public string Group { get; set; }
    public string[] Phrases { get; set; }
}


```

---

## NavMeshErrorFix by Ryz0r - Fixes the Nav Mesh error that has been plaguing and spamming servers

```csharp
using System.Text.RegularExpressions;
using System.Collections.Generic;
using Rust;
using UnityEngine;
using Application = UnityEngine.Application;
using Server = ConVar.Server;

Oxide.Plugins
[Info("Nav Mesh Error Fix", "Ryz0r", "1.1.1")]
[Description("Fixes the dreaded NavMesh Error Spam.")]
 class NavMeshErrorFix : CovalencePlugin
{
    private void Init();
    private void Unload();
    private void HandleLog(string message, string stackTrace, LogType type);
}


```

---

## NeedForSpeed by SwenenzY - Changes speed and mechanics of the minicopter and scrap copter

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Need For Speed", "SwenenzY", "1.0.5")]
[Description("Changes Speed / Mechanical of minicopter, scrap copter ")]
public class NeedForSpeed : RustPlugin
{
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Permission -> Settings")]
        public Dictionary<string, PluginSettings> permissions;
    }

    private class PluginSettings
    {
        [JsonProperty(PropertyName = "MiniCopter: Torque Scale Pitch")]
        public float mTSP;
        [JsonProperty(PropertyName = "MiniCopter: Torque Scale Yaw")]
        public float mTSY;
        [JsonProperty(PropertyName = "MiniCopter: Torque Scale Roll")]
        public float mTSR;
        [JsonProperty(PropertyName = "MiniCopter: Maximum Rotor Speed")]
        public float mMRS;
        [JsonProperty(PropertyName = "MiniCopter: Time Until Maximum Rotor Speed")]
        public float mTUMRS;
        [JsonProperty(PropertyName = "MiniCopter: Rotor Blur Threshold")]
        public float mRBT;
        [JsonProperty(PropertyName = "MiniCopter: Motor Force Constant")]
        public float mMFC;
        [JsonProperty(PropertyName = "MiniCopter: Brake Force Constant")]
        public float mBFC;
        [JsonProperty(PropertyName = "MiniCopter: Fuel Per Second")]
        public float mFPS;
        [JsonProperty(PropertyName = "MiniCopter: Thwop Gain Min")]
        public float mTGMi;
        [JsonProperty(PropertyName = "MiniCopter: Thwop Gain Max")]
        public float mTGMa;
        [JsonProperty(PropertyName = "MiniCopter: Lift Fraction")]
        public float mLF;
        [JsonProperty(PropertyName = "ScrapCopter: Torque Scale Pitch")]
        public float cTSP;
        [JsonProperty(PropertyName = "ScrapCopter: Torque Scale Yaw")]
        public float cTSY;
        [JsonProperty(PropertyName = "ScrapCopter: Torque Scale Roll")]
        public float cTSR;
        [JsonProperty(PropertyName = "ScrapCopter: Maximum Rotor Speed")]
        public float cMRS;
        [JsonProperty(PropertyName = "ScrapCopter: Time Until Maximum Rotor Speed")]
        public float cTUMRS;
        [JsonProperty(PropertyName = "ScrapCopter: Rotor Blur Threshold")]
        public float cRBT;
        [JsonProperty(PropertyName = "ScrapCopter: Motor Force Constant")]
        public float cMFC;
        [JsonProperty(PropertyName = "ScrapCopter: Brake Force Constant")]
        public float cBFC;
        [JsonProperty(PropertyName = "ScrapCopter: Fuel Per Second")]
        public float cFPS;
        [JsonProperty(PropertyName = "ScrapCopter: Thwop Gain Min")]
        public float cTGMi;
        [JsonProperty(PropertyName = "ScrapCopter: Thwop Gain Max")]
        public float cTGMa;
        [JsonProperty(PropertyName = "ScrapCopter: Lift Fraction")]
        public float cLF;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Init();
    private void OnEntityMounted(BaseMountable entity, BasePlayer player);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Permission -> Settings")]
    public Dictionary<string, PluginSettings> permissions;
}

private class PluginSettings
{
    [JsonProperty(PropertyName = "MiniCopter: Torque Scale Pitch")]
    public float mTSP;
    [JsonProperty(PropertyName = "MiniCopter: Torque Scale Yaw")]
    public float mTSY;
    [JsonProperty(PropertyName = "MiniCopter: Torque Scale Roll")]
    public float mTSR;
    [JsonProperty(PropertyName = "MiniCopter: Maximum Rotor Speed")]
    public float mMRS;
    [JsonProperty(PropertyName = "MiniCopter: Time Until Maximum Rotor Speed")]
    public float mTUMRS;
    [JsonProperty(PropertyName = "MiniCopter: Rotor Blur Threshold")]
    public float mRBT;
    [JsonProperty(PropertyName = "MiniCopter: Motor Force Constant")]
    public float mMFC;
    [JsonProperty(PropertyName = "MiniCopter: Brake Force Constant")]
    public float mBFC;
    [JsonProperty(PropertyName = "MiniCopter: Fuel Per Second")]
    public float mFPS;
    [JsonProperty(PropertyName = "MiniCopter: Thwop Gain Min")]
    public float mTGMi;
    [JsonProperty(PropertyName = "MiniCopter: Thwop Gain Max")]
    public float mTGMa;
    [JsonProperty(PropertyName = "MiniCopter: Lift Fraction")]
    public float mLF;
    [JsonProperty(PropertyName = "ScrapCopter: Torque Scale Pitch")]
    public float cTSP;
    [JsonProperty(PropertyName = "ScrapCopter: Torque Scale Yaw")]
    public float cTSY;
    [JsonProperty(PropertyName = "ScrapCopter: Torque Scale Roll")]
    public float cTSR;
    [JsonProperty(PropertyName = "ScrapCopter: Maximum Rotor Speed")]
    public float cMRS;
    [JsonProperty(PropertyName = "ScrapCopter: Time Until Maximum Rotor Speed")]
    public float cTUMRS;
    [JsonProperty(PropertyName = "ScrapCopter: Rotor Blur Threshold")]
    public float cRBT;
    [JsonProperty(PropertyName = "ScrapCopter: Motor Force Constant")]
    public float cMFC;
    [JsonProperty(PropertyName = "ScrapCopter: Brake Force Constant")]
    public float cBFC;
    [JsonProperty(PropertyName = "ScrapCopter: Fuel Per Second")]
    public float cFPS;
    [JsonProperty(PropertyName = "ScrapCopter: Thwop Gain Min")]
    public float cTGMi;
    [JsonProperty(PropertyName = "ScrapCopter: Thwop Gain Max")]
    public float cTGMa;
    [JsonProperty(PropertyName = "ScrapCopter: Lift Fraction")]
    public float cLF;
}


```

---

## NeutralNPCs by 0x89A - NPCs only attack if they are attacked first

```csharp
using System.Collections.Generic;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Neutral NPCs", "0x89A", "2.0.1")]
[Description("NPCs only attack if they are attacked first")]
 class NeutralNPCs : RustPlugin
{
    private const string _usePerm;
    private void Init();
    private object OnNpcTarget(BaseCombatEntity entity, BasePlayer target);
    private object OnNpcTarget(BaseAnimalNPC animal, BasePlayer target);
    private bool CanBradleyApcTarget(BradleyAPC entity, BasePlayer target);
    private bool CanHelicopterTarget(PatrolHelicopterAI entity, BasePlayer target);
    private bool CanTarget(BaseCombatEntity entity, BasePlayer target);
    private bool HasForgotten(float lastAttackedTime);
    private bool HasPermission(BasePlayer player);
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty("Use Permission")]
        public bool usePermission;
        [JsonProperty(PropertyName = "Forget time")]
        public float forgetTime;
        [JsonProperty(PropertyName = "Only animals")]
        public bool onlyAnimals;
        [JsonProperty(PropertyName = "Affect only selected")]
        public bool onlySelected;
        [JsonProperty(PropertyName = "Selected entities")]
        public List<string> selected;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class Configuration
{
    [JsonProperty("Use Permission")]
    public bool usePermission;
    [JsonProperty(PropertyName = "Forget time")]
    public float forgetTime;
    [JsonProperty(PropertyName = "Only animals")]
    public bool onlyAnimals;
    [JsonProperty(PropertyName = "Affect only selected")]
    public bool onlySelected;
    [JsonProperty(PropertyName = "Selected entities")]
    public List<string> selected;
}


```

---

## NeverTeamless by VisEntities - No one goes without a team, even solo players

```csharp
using Newtonsoft.Json;
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Never Teamless", "Dana", "1.0.0")]
[Description("No one goes without a team, even if it's a team of one.")]
public class NeverTeamless : RustPlugin
{
    private Coroutine _teamFormationCoroutine;
    private static Configuration _config;
    private Timer _scanTimer;
    private class Configuration
    {
        [JsonProperty("Version")]
        public string Version { get; set; }
        [JsonProperty("Scan Interval Seconds")]
        public float ScanIntervalSeconds { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfig();
    private Configuration GetDefaultConfig();
    private void OnServerInitialized();
    private void Unload();
    private IEnumerator FormTeams();
    private void StartTeamFormationProcess();
    private void StopTeamFormationProcess();
}

private class Configuration
{
    [JsonProperty("Version")]
    public string Version { get; set; }
    [JsonProperty("Scan Interval Seconds")]
    public float ScanIntervalSeconds { get; set; }
}


```

---

## NeverWear by rostov114 - Stops all items from losing condition

```csharp
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[Info("NeverWear", "k1lly0u / rostov114", "0.2.0")]
 class NeverWear : RustPlugin
{
    private Configuration _config;
    public class Configuration
    {
        public bool useWeapons;
        public bool useTools;
        public bool useAttire;
        public bool useWhiteList;
        public List<string> WhitelistedItems;
        public bool useBlackList;
        public List<string> BlacklistedItems;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Loaded();
    private void OnLoseCondition(Item item, float amount);
    public bool HasPerm(BasePlayer player, string perm);
    public BasePlayer GetPlayer(Item item);
}

public class Configuration
{
    public bool useWeapons;
    public bool useTools;
    public bool useAttire;
    public bool useWhiteList;
    public List<string> WhitelistedItems;
    public bool useBlackList;
    public List<string> BlacklistedItems;
}


```

---

## NightDoor by Slydelix - Allows opening of selected door(s) only during set time period

```csharp
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Night Door", "Slydelix", "1.7.2", ResourceId = 2684)]
 class NightDoor : RustPlugin
{
    private bool hideTime;
    private bool BypassAdmin;
    private bool BypassPerm;
    private bool UseRealTime;
    private bool AutoDoor;
    private bool useMulti;
    private bool intialized;
    private string startTime;
    private string endTime;
    private const string usePerm;
    private const string createIntervalPerm;
    private const string bypassPerm;
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private T GetConfig(string name, T defaultValue);
    private class StoredData
    {
        public Dictionary<uint, string> IDlist;
        public HashSet<TimeInfo> TimeEntries;
        public StoredData();
    }

    private class TimeInfo
    {
        public string name;
        public string start;
        public string end;
        public TimeInfo(string nameIn, string startInput, string endInput);
    }

    private StoredData storedData;
    private void OnNewSave(string filename);
    private void OnServerInitialized();
    private void Init();
    private void SaveFile();
    private void Unload();
    private void OnServerSave();
    private void OnEntityKill(BaseNetworkable entity);
    private void OnDoorOpened(Door door, BasePlayer player);
    private void OnItemDeployed(Deployer deployer, BaseEntity entity);
    private object CanPickupEntity(BaseCombatEntity entity, BasePlayer player);
    private string GetNameFromEntity(BaseEntity entity);
    private void Repeat();
    private void DoDefaultT();
    private void CheckDoors();
    private bool CanOpen(BasePlayer player, uint ID, float now);
    private void CheckAllEntites();
    private float GetFloat(uint ID, bool start);
    private float GetFloat(string in2);
    private DateTime GetDateTime(uint ID, bool start);
    private DateTime GetDateTime(string input);
    private BaseEntity GetLookAtEntity(BasePlayer player, float maxDist, int coll);
    [ConsoleCommand("wipedoordata")]
    private void nightdoorwipeccmd(ConsoleSystem.Arg arg);
    [ChatCommand("timeperiod")]
    private void timeCmd(BasePlayer player, string command, string[] args);
    [ChatCommand("nd")]
    private void nightdoorcmd(BasePlayer player, string command, string[] args);
}

private class StoredData
{
    public Dictionary<uint, string> IDlist;
    public HashSet<TimeInfo> TimeEntries;
    public StoredData();
}

private class TimeInfo
{
    public string name;
    public string start;
    public string end;
    public TimeInfo(string nameIn, string startInput, string endInput);
}


```

---

## NightLantern by k1lly0u - Automatically turns ON and OFF lanterns after sunset and sunrise

```csharp
using HarmonyLib;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("Night Lantern", "k1lly0u", "2.1.1")]
[Description("Automatically turns ON and OFF lanterns after sunset and sunrise")]
 class NightLantern : RustPlugin
{
    [PluginReference]
    private Plugin NoFuelRequirements;
    private static Hash<ulong, Dictionary<EntityType, bool>> _toggleList;
    private readonly HashSet<LightController> _lightControllers;
    private static readonly Hash<BaseOven, LightController> OvenControllers;
    private bool _lightsOn;
    private bool _globalToggle;
    private Timer _timeCheck;
    private static Func<string, ulong, object> _ignoreFuelConsumptionFunction;
    private void Init();
    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private object OnFuelConsume(BaseOven baseOven, Item fuel, ItemModBurnable burnable);
    private void OnOvenToggle(BaseOven baseOven, BasePlayer player);
    private void OnEntitySpawned(BaseEntity entity);
    private void OnEntityKill(BaseNetworkable entity);
    private void OnServerSave();
    private void Unload();
    private IEnumerator CreateAllLights(IEnumerable<BaseNetworkable> entities);
    private void InitializeLightController(BaseEntity entity);
    private void CheckCurrentTime();
    private static IEnumerator ToggleAllLights(IEnumerable<LightController> lights, bool status);
    private static EntityType StringToType(string name);
    private static EntityType ParseType(string type);
    private static bool ConsumeTypeEnabled(ulong playerId, EntityType entityType);
    private object NoFuelRequirementsIgnoreFuelConsumption(string shortname, ulong playerId);
    private class LightController : MonoBehaviour
    {
        private BaseEntity _entity;
        private ConfigData.LightSettings _config;
        private bool _isSearchlight;
        private bool _ignoreFuelConsumption;
        private bool _automaticallyToggled;
        public EntityType entityType;
        public bool ShouldIgnoreFuelConsumption { get; set; }
        private void Awake();
        private void OnEnable();
        private void OnDisable();
        public void ToggleLight(bool status);
        public void OnOvenToggled();
        public object OnConsumeFuel();
        public bool IsOwner(ulong playerId);
    }

    [AutoPatch]
    [HarmonyPatch(typeof(BaseOven))]
    [HarmonyPatch(nameof(BaseOven.CanRunWithNoFuel), MethodType.Getter)]
    private static class BaseOven_CanRunWithNoFuelPatch
    {
        [HarmonyPrefix]
        private static bool Prefix(BaseOven __instance, bool __result);
    }

    private StringBuilder _stringBuilder;
    [ChatCommand("lantern")]
    private void cmdLantern(BasePlayer player, string command, string[] args);
    private static ConfigData _configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Light Settings")]
        public Dictionary<EntityType, LightSettings> Types { get; set; }
        [JsonProperty(PropertyName = "Time autolights are disabled")]
        public float Sunrise { get; set; }
        [JsonProperty(PropertyName = "Time autolights are enabled")]
        public float Sunset { get; set; }
        public class LightSettings
        {
            [JsonProperty(PropertyName = "This type is enabled")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "This type consumes fuel")]
            public bool ConsumeFuel { get; set; }
            [JsonProperty(PropertyName = "This type consumes fuel when toggled by a player")]
            public bool ConsumeFuelWhenToggled { get; set; }
            [JsonProperty(PropertyName = "This type starts cooking items when toggled by plugin")]
            public bool Cook { get; set; }
            [JsonProperty(PropertyName = "This type can be toggled by the owner")]
            public bool Owner { get; set; }
            [JsonProperty(PropertyName = "This type requires permission to be toggled by the owner")]
            public bool Permission { get; set; }
        }

        public VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private void SaveData();
    private void LoadData();
    private string GetMessage(string key, ulong playerId);
    private readonly Dictionary<string, string> _messages;
}

private class LightController : MonoBehaviour
{
    private BaseEntity _entity;
    private ConfigData.LightSettings _config;
    private bool _isSearchlight;
    private bool _ignoreFuelConsumption;
    private bool _automaticallyToggled;
    public EntityType entityType;
    public bool ShouldIgnoreFuelConsumption { get; set; }
    private void Awake();
    private void OnEnable();
    private void OnDisable();
    public void ToggleLight(bool status);
    public void OnOvenToggled();
    public object OnConsumeFuel();
    public bool IsOwner(ulong playerId);
}

[AutoPatch]
[HarmonyPatch(typeof(BaseOven))]
[HarmonyPatch(nameof(BaseOven.CanRunWithNoFuel), MethodType.Getter)]
private static class BaseOven_CanRunWithNoFuelPatch
{
    [HarmonyPrefix]
    private static bool Prefix(BaseOven __instance, bool __result);
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Light Settings")]
    public Dictionary<EntityType, LightSettings> Types { get; set; }
    [JsonProperty(PropertyName = "Time autolights are disabled")]
    public float Sunrise { get; set; }
    [JsonProperty(PropertyName = "Time autolights are enabled")]
    public float Sunset { get; set; }
    public class LightSettings
    {
        [JsonProperty(PropertyName = "This type is enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "This type consumes fuel")]
        public bool ConsumeFuel { get; set; }
        [JsonProperty(PropertyName = "This type consumes fuel when toggled by a player")]
        public bool ConsumeFuelWhenToggled { get; set; }
        [JsonProperty(PropertyName = "This type starts cooking items when toggled by plugin")]
        public bool Cook { get; set; }
        [JsonProperty(PropertyName = "This type can be toggled by the owner")]
        public bool Owner { get; set; }
        [JsonProperty(PropertyName = "This type requires permission to be toggled by the owner")]
        public bool Permission { get; set; }
    }

    public VersionNumber Version { get; set; }
}

public class LightSettings
{
    [JsonProperty(PropertyName = "This type is enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "This type consumes fuel")]
    public bool ConsumeFuel { get; set; }
    [JsonProperty(PropertyName = "This type consumes fuel when toggled by a player")]
    public bool ConsumeFuelWhenToggled { get; set; }
    [JsonProperty(PropertyName = "This type starts cooking items when toggled by plugin")]
    public bool Cook { get; set; }
    [JsonProperty(PropertyName = "This type can be toggled by the owner")]
    public bool Owner { get; set; }
    [JsonProperty(PropertyName = "This type requires permission to be toggled by the owner")]
    public bool Permission { get; set; }
}


```

---

## NightMessage by Mordeus - Broadcasts a custom message at dawn and/or dusk

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("NightMessage", "Mordeus", "1.0.1")]
[Description("Universal Day/Night Message")]
public class NightMessage : CovalencePlugin
{
    public bool NightMessageSent;
    public bool DayMessageSent;
    public string ChatTitle;
    public bool UseNightMessage;
    public bool UseDayMessage;
    public float DawnTime;
    public float DuskTime;
    private new void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    private new void LoadConfig();
    private void OnServerInitialized();
    private void Init();
    private void CheckTime();
     bool IsNight { get; set; }
    private void SendMessage(bool night, bool day);
    private string Message(string key, string id, object[] args);
    private T GetConfig(object[] pathAndValue);
    private void Broadcast(string key);
}


```

---

## NightPVP by  - Allows PvP only during night

```csharp
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Collections.Generic;
using Convert = System.Convert;
using System.Linq;
using System;
using Oxide.Core;

Oxide.Plugins
[Info("Night PVP", "BuzZ[PHOQUE]", "0.1.7")]
[Description("PVP only during night and PVE no damage during day")]
public class NightPVP : RustPlugin
{
     string Prefix;
     string PrefixColor;
     string ChatColor;
     ulong SteamIDIcon;
     float starthour;
     float stophour;
     float rate;
    private static string ONpvpHUD;
    private static string ONpveHUD;
     bool debug;
    private bool ConfigChanged;
     double leftmin;
     double bottom;
     int HUDtxtsize;
     double HUDwidth;
     double HUDheigth;
     string HUDpvecolor;
     string HUDpveopacity;
     string HUDpvpcolor;
     string HUDpvpopacity;
    public List<BasePlayer> hasreceived;
     void Init();
     void OnServerInitialized();
     void insidetimer();
     void Loaded();
     void Unload();
     class StoredData
    {
        public bool NightPvpOn;
        public StoredData();
    }

    private StoredData storedData;
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private void NightIsOn();
    private void NightIsOff();
    private void DisplayPVE(BasePlayer player);
    private void DisplayPVP(BasePlayer player);
    private void KillHUD(BasePlayer player);
     void OnEntityTakeDamage(BaseEntity entity, HitInfo info);
    public bool IsReal(BasePlayer check);
    public bool IsRaid(BaseEntity entity, BasePlayer badguy, HitInfo info);
    private void AntiSpamage(BasePlayer player);
    private void ChatPlayerOnline(string status);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
}

 class StoredData
{
    public bool NightPvpOn;
    public StoredData();
}


```

---

## NightVision by Clearshot - Allows players to see at night

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Network;
using Oxide.Core;

Oxide.Plugins
[Info("NightVision", "Clearshot", "2.4.1")]
[Description("Allows players to see at night")]
 class NightVision : CovalencePlugin
{
    private PluginConfig _config;
    private Game.Rust.Libraries.Player _rustPlayer;
    private EnvSync _envSync;
    private Dictionary<ulong, NVPlayerData> _playerData;
    private Dictionary<ulong, float> _playerTimes;
    private DateTime _nvDate;
    private List<ulong> _connected;
    private string PERM_ALLOWED;
    private string PERM_UNLIMITEDNVG;
    private string PERM_AUTO;
    private bool API_blockEnvUpdates;
    private void SendChatMsg(BasePlayer pl, string msg, string prefix);
    private void Init();
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer pl, string reason);
    private void OnPlayerSleepEnded(BasePlayer pl);
    private void Unload();
    private void SaveData();
    [Command("nightvision", "nv", "unlimitednvg", "unvg")]
    private void NightVisionCommand(IPlayer player, string command, string[] args);
    private object CanWearItem(PlayerInventory inventory, Item item, int targetSlot);
    private void OnItemDropped(Item item, BaseEntity entity);
    private NVPlayerData GetNVPlayerData(BasePlayer pl);
    [HookMethod("LockPlayerTime")]
     void LockPlayerTime_PluginAPI(BasePlayer player, float time);
    [HookMethod("UnlockPlayerTime")]
     void UnlockPlayerTime_PluginAPI(BasePlayer player);
    [HookMethod("IsPlayerTimeLocked")]
     bool IsPlayerTimeLocked_PluginAPI(BasePlayer player);
    [HookMethod("BlockEnvUpdates")]
     void BlockEnvUpdates_PluginAPI(bool blockEnv);
    private DateTime _defaultDate;
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
    protected override void LoadConfig();
    private class PluginConfig
    {
        public string chatIconID;
        public string date;
        public float time;
    }

    private class NVPlayerData
    {
        public bool timeLocked;
        public float time;
    }

}

private class PluginConfig
{
    public string chatIconID;
    public string date;
    public float time;
}

private class NVPlayerData
{
    public bool timeLocked;
    public float time;
}


```

---

## NightZombies by 0x89A - Spawns murderers and scarecrows at night, kills them at sunrise

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Random = UnityEngine.Random;
using Physics = UnityEngine.Physics;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using ConVar;
using Pool = Facepunch.Pool;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Night Zombies", "0x89A", "3.4.2")]
[Description("Spawns and kills zombies at set times")]
 class NightZombies : RustPlugin
{
    private const string DeathSound;
    private const string RemoveMeMethodName;
    private const int GrenadeItemId;
    private static NightZombies _instance;
    private static Configuration _config;
    private DynamicConfigFile _dataFile;
    [PluginReference("Kits")]
    private Plugin _kits;
    [PluginReference("Vanish")]
    private Plugin _vanish;
    private SpawnController _spawnController;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnDay();
    private object OnNpcTarget(ScarecrowNPC npc, BaseEntity target);
    private object OnTurretTarget(NPCAutoTurret turret, ScarecrowNPC entity);
    private object OnPlayerDeath(ScarecrowNPC scarecrow, HitInfo info);
    private BaseCorpse OnCorpsePopulate(ScarecrowNPC npcPlayer, NPCPlayerCorpse corpse);
    private void OnEntitySpawned(NPCPlayerCorpse corpse);
    private void OnEntitySpawned(DroppedItemContainer container);
    private object CanAttack(BaseEntity target);
    private bool HumanNPCCheck(BaseEntity target);
    private class SpawnController
    {
        private const string _zombiePrefab;
        private readonly Configuration.SpawnSettings _spawnConfig;
        private readonly Configuration.SpawnSettings.ZombieSettings _zombiesConfig;
        private readonly int _spawnLayerMask;
        private readonly WaitForSeconds _waitTenthSecond;
        private bool IsSpawnTime { get; set; }
        private bool IsDestroyTime { get; set; }
        public int DaysSinceLastSpawn;
        private readonly float _spawnTime;
        private readonly float _destroyTime;
        private readonly bool _leaveCorpse;
        private bool _spawned;
        private Coroutine _currentCoroutine;
        private readonly List<ScarecrowNPC> _zombies;
        public SpawnController();
        private IEnumerator SpawnZombies();
        private IEnumerator RemoveZombies(bool shuttingDown);
        public void TimeTick();
        public void ZombieDied(ScarecrowNPC zombie);
        private void SpawnZombie();
        private bool GetRandomPlayer(BasePlayer player);
        private Vector3 GetRandomPosition();
        private Vector3 GetRandomPositionAroundPlayer(BasePlayer player);
        private bool CanSpawn();
        private bool IsInObject(Vector3 position);
        private bool IsInOcean(Vector3 position);
        private void Broadcast(string key, object[] values);
        public void Shutdown();
    }

    private class Configuration
    {
        [JsonProperty("Spawn Settings")]
        public SpawnSettings Spawn;
        [JsonProperty("Destroy Settings")]
        public DestroySettings Destroy;
        [JsonProperty("Behaviour Settings")]
        public BehaviourSettings Behaviour;
        [JsonProperty("Broadcast Settings")]
        public ChatSettings Broadcast;
        public class SpawnSettings
        {
            [JsonProperty("Always Spawned")]
            public bool AlwaysSpawned;
            [JsonProperty("Spawn Time")]
            public float SpawnTime;
            [JsonProperty("Destroy Time")]
            public float DestroyTime;
            [JsonProperty("Spawn near players")]
            public bool SpawnNearPlayers;
            [JsonProperty("Min pop for near player spawn")]
            public int MinNearPlayers;
            [JsonProperty("Min distance from player")]
            public float MinDistance;
            [JsonProperty("Max distance from player")]
            public float MaxDistance;
            [JsonProperty("Zombie Settings")]
            public ZombieSettings Zombies;
            public class ZombieSettings
            {
                [JsonProperty("Display Name")]
                public string DisplayName;
                [JsonProperty("Scarecrow Population (total amount)")]
                public int Population;
                [JsonProperty("Scarecrow Health")]
                public float Health;
                [JsonProperty("Scarecrow Kits")]
                public List<string> Kits;
            }

            [JsonProperty("Chance Settings")]
            public ChanceSetings Chance;
            public class ChanceSetings
            {
                [JsonProperty("Chance per cycle")]
                public float Chance;
                [JsonProperty("Days betewen spawn")]
                public int Days;
            }

        }

        public class DestroySettings
        {
            [JsonProperty("Leave Corpse, when destroyed")]
            public bool LeaveCorpse;
            [JsonProperty("Leave Corpse, when killed by player")]
            public bool LeaveCorpseKilled;
            [JsonProperty("Spawn Loot")]
            public bool SpawnLoot;
            [JsonProperty("Half bodybag despawn time")]
            public bool HalfBodybagDespawn;
        }

        public class BehaviourSettings
        {
            [JsonProperty("Attack sleeping players")]
            public bool AttackSleepers;
            [JsonProperty("Zombies attacked by outpost sentries")]
            public bool SentriesAttackZombies;
            [JsonProperty("Throw Grenades")]
            public bool ThrowGrenades;
            [JsonProperty("Ignore Human NPCs")]
            public bool IgnoreHumanNpc;
            [JsonProperty("Ignored entities (full entity shortname)")]
            public List<string> Ignored;
        }

        public class ChatSettings
        {
            [JsonProperty("Broadcast spawn amount")]
            public bool DoBroadcast;
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private string GetMessage(string key, string userId);
    protected override void LoadDefaultMessages();
}

private class SpawnController
{
    private const string _zombiePrefab;
    private readonly Configuration.SpawnSettings _spawnConfig;
    private readonly Configuration.SpawnSettings.ZombieSettings _zombiesConfig;
    private readonly int _spawnLayerMask;
    private readonly WaitForSeconds _waitTenthSecond;
    private bool IsSpawnTime { get; set; }
    private bool IsDestroyTime { get; set; }
    public int DaysSinceLastSpawn;
    private readonly float _spawnTime;
    private readonly float _destroyTime;
    private readonly bool _leaveCorpse;
    private bool _spawned;
    private Coroutine _currentCoroutine;
    private readonly List<ScarecrowNPC> _zombies;
    public SpawnController();
    private IEnumerator SpawnZombies();
    private IEnumerator RemoveZombies(bool shuttingDown);
    public void TimeTick();
    public void ZombieDied(ScarecrowNPC zombie);
    private void SpawnZombie();
    private bool GetRandomPlayer(BasePlayer player);
    private Vector3 GetRandomPosition();
    private Vector3 GetRandomPositionAroundPlayer(BasePlayer player);
    private bool CanSpawn();
    private bool IsInObject(Vector3 position);
    private bool IsInOcean(Vector3 position);
    private void Broadcast(string key, object[] values);
    public void Shutdown();
}

private class Configuration
{
    [JsonProperty("Spawn Settings")]
    public SpawnSettings Spawn;
    [JsonProperty("Destroy Settings")]
    public DestroySettings Destroy;
    [JsonProperty("Behaviour Settings")]
    public BehaviourSettings Behaviour;
    [JsonProperty("Broadcast Settings")]
    public ChatSettings Broadcast;
    public class SpawnSettings
    {
        [JsonProperty("Always Spawned")]
        public bool AlwaysSpawned;
        [JsonProperty("Spawn Time")]
        public float SpawnTime;
        [JsonProperty("Destroy Time")]
        public float DestroyTime;
        [JsonProperty("Spawn near players")]
        public bool SpawnNearPlayers;
        [JsonProperty("Min pop for near player spawn")]
        public int MinNearPlayers;
        [JsonProperty("Min distance from player")]
        public float MinDistance;
        [JsonProperty("Max distance from player")]
        public float MaxDistance;
        [JsonProperty("Zombie Settings")]
        public ZombieSettings Zombies;
        public class ZombieSettings
        {
            [JsonProperty("Display Name")]
            public string DisplayName;
            [JsonProperty("Scarecrow Population (total amount)")]
            public int Population;
            [JsonProperty("Scarecrow Health")]
            public float Health;
            [JsonProperty("Scarecrow Kits")]
            public List<string> Kits;
        }

        [JsonProperty("Chance Settings")]
        public ChanceSetings Chance;
        public class ChanceSetings
        {
            [JsonProperty("Chance per cycle")]
            public float Chance;
            [JsonProperty("Days betewen spawn")]
            public int Days;
        }

    }

    public class DestroySettings
    {
        [JsonProperty("Leave Corpse, when destroyed")]
        public bool LeaveCorpse;
        [JsonProperty("Leave Corpse, when killed by player")]
        public bool LeaveCorpseKilled;
        [JsonProperty("Spawn Loot")]
        public bool SpawnLoot;
        [JsonProperty("Half bodybag despawn time")]
        public bool HalfBodybagDespawn;
    }

    public class BehaviourSettings
    {
        [JsonProperty("Attack sleeping players")]
        public bool AttackSleepers;
        [JsonProperty("Zombies attacked by outpost sentries")]
        public bool SentriesAttackZombies;
        [JsonProperty("Throw Grenades")]
        public bool ThrowGrenades;
        [JsonProperty("Ignore Human NPCs")]
        public bool IgnoreHumanNpc;
        [JsonProperty("Ignored entities (full entity shortname)")]
        public List<string> Ignored;
    }

    public class ChatSettings
    {
        [JsonProperty("Broadcast spawn amount")]
        public bool DoBroadcast;
    }

}

public class SpawnSettings
{
    [JsonProperty("Always Spawned")]
    public bool AlwaysSpawned;
    [JsonProperty("Spawn Time")]
    public float SpawnTime;
    [JsonProperty("Destroy Time")]
    public float DestroyTime;
    [JsonProperty("Spawn near players")]
    public bool SpawnNearPlayers;
    [JsonProperty("Min pop for near player spawn")]
    public int MinNearPlayers;
    [JsonProperty("Min distance from player")]
    public float MinDistance;
    [JsonProperty("Max distance from player")]
    public float MaxDistance;
    [JsonProperty("Zombie Settings")]
    public ZombieSettings Zombies;
    public class ZombieSettings
    {
        [JsonProperty("Display Name")]
        public string DisplayName;
        [JsonProperty("Scarecrow Population (total amount)")]
        public int Population;
        [JsonProperty("Scarecrow Health")]
        public float Health;
        [JsonProperty("Scarecrow Kits")]
        public List<string> Kits;
    }

    [JsonProperty("Chance Settings")]
    public ChanceSetings Chance;
    public class ChanceSetings
    {
        [JsonProperty("Chance per cycle")]
        public float Chance;
        [JsonProperty("Days betewen spawn")]
        public int Days;
    }

}

public class ZombieSettings
{
    [JsonProperty("Display Name")]
    public string DisplayName;
    [JsonProperty("Scarecrow Population (total amount)")]
    public int Population;
    [JsonProperty("Scarecrow Health")]
    public float Health;
    [JsonProperty("Scarecrow Kits")]
    public List<string> Kits;
}

public class ChanceSetings
{
    [JsonProperty("Chance per cycle")]
    public float Chance;
    [JsonProperty("Days betewen spawn")]
    public int Days;
}

public class DestroySettings
{
    [JsonProperty("Leave Corpse, when destroyed")]
    public bool LeaveCorpse;
    [JsonProperty("Leave Corpse, when killed by player")]
    public bool LeaveCorpseKilled;
    [JsonProperty("Spawn Loot")]
    public bool SpawnLoot;
    [JsonProperty("Half bodybag despawn time")]
    public bool HalfBodybagDespawn;
}

public class BehaviourSettings
{
    [JsonProperty("Attack sleeping players")]
    public bool AttackSleepers;
    [JsonProperty("Zombies attacked by outpost sentries")]
    public bool SentriesAttackZombies;
    [JsonProperty("Throw Grenades")]
    public bool ThrowGrenades;
    [JsonProperty("Ignore Human NPCs")]
    public bool IgnoreHumanNpc;
    [JsonProperty("Ignored entities (full entity shortname)")]
    public List<string> Ignored;
}

public class ChatSettings
{
    [JsonProperty("Broadcast spawn amount")]
    public bool DoBroadcast;
}


```

---

## NoAmmoConsumption by KibbeWater - Prevents ammunition from being consumed when reloading

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("No Ammo Consumtion", "KibbeWater", "0.1.6")]
[Description("Blocks from consuming ammunition from your inventory")]
 class NoAmmoConsumption : RustPlugin
{
    private PluginConfig _config;
    private string permissionUse;
    private Dictionary<BaseProjectile, Timer> reloadingWeapons;
    private List<int> segmentLoadedWeapons;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private class PermissionInfo
    {
        public string permissionName;
        public List<int> ammoList;
    }

    private class PluginConfig
    {
        public List<PermissionInfo> allowedAmmoGroups;
        public static PluginConfig DefaultConfig();
    }

    public bool isAllowed(BasePlayer player, int ammoID);
    private bool IsSegmentedLoaded(BaseProjectile weapon);
    private bool IsSegmentedLoaded(int weponId);
    private void Init();
     object OnReloadWeapon(BasePlayer player, BaseProjectile projectile);
     object OnAmmoUnload(BaseProjectile projectile, Item item, BasePlayer player);
     object OnSwitchAmmo(BasePlayer player, BaseProjectile projectile);
     void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
    private void Unload();
}

private class PermissionInfo
{
    public string permissionName;
    public List<int> ammoList;
}

private class PluginConfig
{
    public List<PermissionInfo> allowedAmmoGroups;
    public static PluginConfig DefaultConfig();
}


```

---

## NoBackpacks by collectvood - Removes backpacks after the configured amount of time

```csharp
using System;
using System.Linq;

Oxide.Plugins
[Info("No Backpacks", "hoppel", "1.0.4")]
[Description("Removes backpacks after the configured amount of time")]
public class NoBackpacks : RustPlugin
{
    private void OnEntitySpawned(BaseNetworkable entity);
    private int despawnTimer;
    private new void LoadConfig();
    private void Init();
    private void GetConfig(T variable, string[] path);
    protected override void LoadDefaultConfig();
}


```

---

## NoBlood by birthdates - Removes bleeding from rust

```csharp
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("No Blood", "birthdates", "1.0.1")]
[Description("Removes bleeding from rust")]
public class NoBlood : RustPlugin
{
    private string Perm;
    private void Init();
     void OnRunPlayerMetabolism(PlayerMetabolism metabolism);
}


```

---

## NoBuildOnRoad by supreme - Disallows building on roads

```csharp
using UnityEngine;
using System;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("No Build On Road", "supreme", "1.0.4")]
[Description("Disallow building over road")]
public class NoBuildOnRoad : RustPlugin
{
    const string NoBuildOnRoadIgnore;
    protected override void LoadDefaultMessages();
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Prefix")]
        public string usedPrefix;
        [JsonProperty(PropertyName = "Prefix Color")]
        public string usedPrefixColor;
        [JsonProperty(PropertyName = "Chat Icon [SteamID64]")]
        public ulong chatIcon;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
     void OnEntityBuilt(Planner plan, GameObject go);
     void GiveRefund(BaseEntity entity, BasePlayer player);
     void Init();
     bool CheckRoad(Vector3 Position);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Prefix")]
    public string usedPrefix;
    [JsonProperty(PropertyName = "Prefix Color")]
    public string usedPrefixColor;
    [JsonProperty(PropertyName = "Chat Icon [SteamID64]")]
    public ulong chatIcon;
}


```

---

## NoClipRestrictions by 2CHEVSKII - Blocks usage of certain commands and items as well as dealing and receiving damage while in noclip

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("NoClip Restrictions", "2CHEVSKII", "0.1.1")]
[Description("Blocks usage of certain commands and items as well as dealing and receiving damage while in noclip mode")]
internal class NoClipRestrictions : CovalencePlugin
{
    private const string PERMISSIONBYPASS;
    private bool check_running;
    private PluginSettings Settings { get; set; }
    private class PluginSettings
    {
        [JsonProperty("Blocked items (player cannot set them active while nocliping)")]
        public List<string> BlockedItems { get; set; }
        [JsonProperty("Blocked commands (player cannot execute them while nocliping)")]
        public List<string> BlockedCommands { get; set; }
        [JsonProperty("Block outgoing damage")]
        public bool BlockOutDamage { get; set; }
        [JsonProperty("Block incoming damage")]
        public bool BlockInDamage { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private readonly Dictionary<string, string> defaultmessages_en;
    private const string mprefix;
    private const string mblockeditem;
    private const string mblockedcommand;
    private const string mblockedoutdmg;
    private const string mblockedindmg;
    private const string mblockedusageitem;
    private const string mblockedusageentity;
    protected override void LoadDefaultMessages();
    private string GetLocalizedString(IPlayer player, string key, object[] args);
    private void SendMessageRaw(IPlayer player, string message);
    private void SendMessage(IPlayer player, string key, object[] args);
    private void SendMessage(BasePlayer player, string key, object[] args);
    private object OnPlayerAttack(BasePlayer attacker, HitInfo info);
    private void OnPlayerActiveItemChanged(BasePlayer player);
    private void OnServerInitialized();
     object OnPlayerCommand(ConsoleSystem.Arg arg);
    private IEnumerator CheckRoutine();
    private void CheckHeld(BasePlayer player, bool message);
}

private class PluginSettings
{
    [JsonProperty("Blocked items (player cannot set them active while nocliping)")]
    public List<string> BlockedItems { get; set; }
    [JsonProperty("Blocked commands (player cannot execute them while nocliping)")]
    public List<string> BlockedCommands { get; set; }
    [JsonProperty("Block outgoing damage")]
    public bool BlockOutDamage { get; set; }
    [JsonProperty("Block incoming damage")]
    public bool BlockInDamage { get; set; }
}


```

---

## NoCoilDrain by Lincoln - Prevents the Tesla coil from damaging itself while in use

```csharp
using UnityEngine;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("No Coil Drain", "Lincoln/Orange", "1.0.5")]
[Description("Prevent Tesla coils from damaging themselves while in use.")]
public class NoCoilDrain : RustPlugin
{
    private const string PermissionNoCoilDrainUnlimited;
    private void OnServerInitialized();
    private void OnEntitySpawned(TeslaCoil entity);
    private void Unload();
}


```

---

## NoCompound by VisEntities - Removes compound (outpost) and bandit camp components (turrets, bots, safe zones)

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("No Compound", "Orange", "1.0.1")]
[Description("Removing compound (outpost) and bandit camp components (turrets, bots, safe zones)")]
public class NoCompound : RustPlugin
{
    private const float distance;
    private List<Vector3> banditTown;
    private List<Vector3> compound;
    private bool nearCompound(Vector3 pos);
    private bool nearBanditTown(Vector3 pos);
    private void Init();
    private void OnServerInitialized();
    private void OnEntitySpawned(NPCPlayer player);
    private object OnEntityMarkHostile(BaseCombatEntity entity, float duration);
    private void CheckObjects();
    private void CheckPlayer(NPCPlayer player);
    private void CheckSafeZone(TriggerSafeZone trigger);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Remove NPC Turrets")]
        public bool removeTurrets;
        [JsonProperty(PropertyName = "Remove vehicle vendors")]
        public bool removeVehicleVendor;
        [JsonProperty(PropertyName = "Remove Compound safe zone")]
        public bool removeCompoundCampSZ;
        [JsonProperty(PropertyName = "Remove Compound npc-s")]
        public bool removeNPCCompound;
        [JsonProperty(PropertyName = "Remove Bandit Camp safe zone")]
        public bool removeBanditCampSZ;
        [JsonProperty(PropertyName = "Remove Bandit Camp npc-s")]
        public bool removeNPCBanditTown;
        [JsonProperty(PropertyName = "Remove hostile timer")]
        public bool removeHostileTimer;
    }

    protected override void LoadConfig();
    private void ValidateConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Remove NPC Turrets")]
    public bool removeTurrets;
    [JsonProperty(PropertyName = "Remove vehicle vendors")]
    public bool removeVehicleVendor;
    [JsonProperty(PropertyName = "Remove Compound safe zone")]
    public bool removeCompoundCampSZ;
    [JsonProperty(PropertyName = "Remove Compound npc-s")]
    public bool removeNPCCompound;
    [JsonProperty(PropertyName = "Remove Bandit Camp safe zone")]
    public bool removeBanditCampSZ;
    [JsonProperty(PropertyName = "Remove Bandit Camp npc-s")]
    public bool removeNPCBanditTown;
    [JsonProperty(PropertyName = "Remove hostile timer")]
    public bool removeHostileTimer;
}


```

---

## NoCorpse by ErikAleksander - Simple plugin that will allow players with set permission to not spawn their corpse when they die.

```csharp

Oxide.Plugins
[Info("No Corpse", "Erik", "1.0.2")]
[Description("Simple plugin that will allow players with set permission to not spawn their corpse when they die.")]
public class NoCorpse : RustPlugin
{
    private const string permNoCorpse;
    private void Init();
     object OnPlayerCorpseSpawn(BasePlayer player);
}


```

---

## NoCraft by Ryan - Blacklist craft items or disable crafting altogether

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("No Craft", "Ryan", "1.0.0")]
[Description("Blacklist craft items or disable crafting altogether")]
 class NoCraft : RustPlugin
{
    private ConfigFile CFile;
    private const string Perm;
    private class ConfigFile
    {
        public Dictionary<string, bool> BlockedItems { get; set; }
        public BlockAll BlockAll { get; set; }
        public ConfigFile();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void Regenerate();
    private class Msg
    {
        public const string CantCraft;
        public const string CraftingDisabled;
    }

    private new void LoadDefaultMessages();
    private class BlockAll
    {
        public bool Enabled { get; set; }
        public bool SendMessage { get; set; }
    }

    private string Lang(string key, string id, object[] args);
    private void Init();
    private object CanCraft(ItemCrafter itemCrafter, ItemBlueprint bp, int amount);
}

private class ConfigFile
{
    public Dictionary<string, bool> BlockedItems { get; set; }
    public BlockAll BlockAll { get; set; }
    public ConfigFile();
}

private class Msg
{
    public const string CantCraft;
    public const string CraftingDisabled;
}

private class BlockAll
{
    public bool Enabled { get; set; }
    public bool SendMessage { get; set; }
}


```

---

## NoCrashFlyingVehicles by MONaH - Prevents flying vehicles from crashing

```csharp

Oxide.Plugins
[Info("No Crash Flying Vehicles", "MON@H", "2.0.0")]
[Description("Prevents flying vehicles from crashing.")]
public class NoCrashFlyingVehicles : RustPlugin
{
    private static readonly object _true;
    private void Init();
    private void OnServerInitialized();
    private object OnEntityTakeDamage(BaseHelicopter entity, HitInfo info);
}


```

---

## NoCrawl by bushhy - A lightweight way to remove the crawling state.

```csharp

Oxide.Plugins
[Info("No Crawl", "Bushhy", "1.0.1")]
[Description("A simple plugin to disable the crawling state.")]
public class NoCrawl : RustPlugin
{
     object OnPlayerWound(BasePlayer player, HitInfo hitInfo);
}


```

---

## NoDeathScreen by Paulsimik - Disables the death screen by automatically respawning players

```csharp
using System.Collections.Generic;
using Oxide.Core;
using System.Linq;
using System;
using Newtonsoft.Json;

Oxide.Plugins
[Info("No Death Screen", "Paulsimik", "1.0.2")]
[Description("Disables the death screen by automatically respawning players")]
public class NoDeathScreen : RustPlugin
{
    private const string permUse;
    private const string permToggle;
    private const string fileName;
    private Configuration config;
    private List<string> playerData;
    private void Init();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnEntityDeath(BasePlayer player);
    private void ProcessRespawn(BasePlayer player);
    private bool HasSleepingBag(ulong playerID);
    private bool HasPermRespawn(string playerID);
    private void chatCmdNoDeathScreen(BasePlayer player, string command, string[] args);
    private class Configuration
    {
        [JsonProperty(PropertyName = "No respawn if the player has a sleeping bag or bed")]
        public bool sleepingBagBypass;
        [JsonProperty(PropertyName = "End sleeping after respawned")]
        public bool endSleeping;
        [JsonProperty(PropertyName = "Custom chat commands")]
        public string[] chatCommands;
        public VersionNumber version;
    }

    private void SaveData();
    private void LoadData();
    private Configuration GetDefaultConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
    private string GetLang(string key, string playerID);
    protected override void LoadDefaultMessages();
}

private class Configuration
{
    [JsonProperty(PropertyName = "No respawn if the player has a sleeping bag or bed")]
    public bool sleepingBagBypass;
    [JsonProperty(PropertyName = "End sleeping after respawned")]
    public bool endSleeping;
    [JsonProperty(PropertyName = "Custom chat commands")]
    public string[] chatCommands;
    public VersionNumber version;
}


```

---

## NoDeauth by KrunghCrow - Simple DeAuthing blocker with perm and global config

```csharp
using System.Collections.Generic;
using System.Text;
using Oxide.Core;
using UnityEngine;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

Oxide.Plugins
[Info("No Deauth", "Krungh Crow", "1.0.1")]
[Description("Prevent Deauthing from TC's")]
 class NoDeauth : RustPlugin
{
    const string Bypass_Perm;
     void Init();
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Global")]
        public SettingsGlobal CFG;
    }

     class SettingsGlobal
    {
        [JsonProperty(PropertyName = "Prevent deauthing")]
        public bool GlobalDA;
    }

    private bool LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConf();
    protected override void LoadDefaultMessages();
     object OnCupboardDeauthorize(BuildingPrivlidge privilege, BasePlayer player);
     object OnCupboardClearList(BuildingPrivlidge privilege, BasePlayer player);
     string msg(string key, string id);
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Global")]
    public SettingsGlobal CFG;
}

 class SettingsGlobal
{
    [JsonProperty(PropertyName = "Prevent deauthing")]
    public bool GlobalDA;
}


```

---

## NoDecay by 0x89A - Scales or disables decay of items, and deployables

```csharp
using System;
using System.Collections.Concurrent;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
using Facepunch;
using Rust;
using Newtonsoft.Json;
using Oxide.Core;
using ProtoBuf;
using Environment = System.Environment;

Oxide.Plugins
[Info("No Decay", "0x89A", "1.5.0")]
[Description("Scales or disables decay of items and deployables")]
 class NoDecay : RustPlugin
{
    private Configuration _config;
    private readonly Dictionary<ulong, BuildingPrivlidge> _cachedToolCupboards;
    private const int WoodItemId;
    private const int StoneItemId;
    private const int MetalFragmentsItemId;
    private const int HqMetalItemId;
    private readonly uint _toolCupboardPrefabId;
    private readonly uint _retroToolCupboardPrefabId;
    private readonly uint _shockByteToolCupboardPrefabId;
    private readonly ConcurrentQueue<string> _logQueue;
    private bool _logQueueRunning;
    private string _logFileName;
    private void Init();
    private void Output(string text);
    private void LogWorker();
    private object OnEntityTakeDamage(DecayEntity entity, HitInfo info);
    private object CanMoveItem(Item item, PlayerInventory playerLoot, ItemContainerId targetContainer, int targetSlot, int amount);
    private bool AnyToolCupboards(BaseEntity entity);
    private bool CheckAllToolCupboardPlayers(BuildingPrivlidge toolCupboard);
    private bool IsOfType(BaseEntity entity, string matchingKey);
    private bool IsToolCupboard(BaseEntity entity);
    private class Configuration
    {
        [JsonProperty(PropertyName = "General")]
        public GeneralSettings General;
        [JsonProperty(PropertyName = "Building grade multipliers")]
        public TierMultipliers BuildingTiers;
        [JsonProperty(PropertyName = "Other multipliers")]
        public Dictionary<string, float> multipliers;
        public class GeneralSettings
        {
            [JsonProperty(PropertyName = "Disable decay for all entities")]
            public bool disableAll;
            [JsonProperty(PropertyName = "Exclude \"Other Multipliers\"")]
            public bool excludeOthers;
            [JsonProperty(PropertyName = "Use permission")]
            public bool usePermission;
            [JsonProperty(PropertyName = "Decay if there is no owner (and perms enabled)")]
            public bool decayNoOwner;
            [JsonProperty(PropertyName = "Permission")]
            public string permission;
            [JsonProperty(PropertyName = "Output")]
            public OutputClass Output;
            [JsonProperty(PropertyName = "Cupboard Settings")]
            public CupboardSettingsClass CupboardSettings;
            public class OutputClass
            {
                [JsonProperty(PropertyName = "Output to server console")]
                public bool rconOutput;
                [JsonProperty(PropertyName = "Log to file")]
                public bool logToFile;
                [JsonProperty(PropertyName = "Log file name")]
                public string logFileName;
            }

            public class CupboardSettingsClass
            {
                [JsonProperty(PropertyName = "Disable No Decay if resources placed in TC")]
                public bool disableOnResources;
                [JsonProperty(PropertyName = "Require Tool Cupboard")]
                public bool requireTC;
                [JsonProperty(PropertyName = "Any authed on TC")]
                public bool anyAuthed;
                [JsonProperty(PropertyName = "Cupboard Range")]
                public float cupboardRange;
                [JsonProperty(PropertyName = "Block cupboard wood")]
                public bool blockWood;
                [JsonProperty(PropertyName = "Block cupboard stone")]
                public bool blockStone;
                [JsonProperty(PropertyName = "Block cupbard metal")]
                public bool blockMetal;
                [JsonProperty(PropertyName = "Block cupboard high quality")]
                public bool blockHighQ;
            }

        }

        public class TierMultipliers
        {
            [JsonProperty(PropertyName = "Twig multiplier")]
            public float twig;
            [JsonProperty(PropertyName = "Wood multiplier")]
            public float wood;
            [JsonProperty(PropertyName = "Stone multiplier")]
            public float stone;
            [JsonProperty(PropertyName = "Sheet Metal multiplier")]
            public float metal;
            [JsonProperty(PropertyName = "Armoured multiplier")]
            public float armoured;
        }

        [JsonIgnore]
        public float[] buildingMultipliers;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
}

private class Configuration
{
    [JsonProperty(PropertyName = "General")]
    public GeneralSettings General;
    [JsonProperty(PropertyName = "Building grade multipliers")]
    public TierMultipliers BuildingTiers;
    [JsonProperty(PropertyName = "Other multipliers")]
    public Dictionary<string, float> multipliers;
    public class GeneralSettings
    {
        [JsonProperty(PropertyName = "Disable decay for all entities")]
        public bool disableAll;
        [JsonProperty(PropertyName = "Exclude \"Other Multipliers\"")]
        public bool excludeOthers;
        [JsonProperty(PropertyName = "Use permission")]
        public bool usePermission;
        [JsonProperty(PropertyName = "Decay if there is no owner (and perms enabled)")]
        public bool decayNoOwner;
        [JsonProperty(PropertyName = "Permission")]
        public string permission;
        [JsonProperty(PropertyName = "Output")]
        public OutputClass Output;
        [JsonProperty(PropertyName = "Cupboard Settings")]
        public CupboardSettingsClass CupboardSettings;
        public class OutputClass
        {
            [JsonProperty(PropertyName = "Output to server console")]
            public bool rconOutput;
            [JsonProperty(PropertyName = "Log to file")]
            public bool logToFile;
            [JsonProperty(PropertyName = "Log file name")]
            public string logFileName;
        }

        public class CupboardSettingsClass
        {
            [JsonProperty(PropertyName = "Disable No Decay if resources placed in TC")]
            public bool disableOnResources;
            [JsonProperty(PropertyName = "Require Tool Cupboard")]
            public bool requireTC;
            [JsonProperty(PropertyName = "Any authed on TC")]
            public bool anyAuthed;
            [JsonProperty(PropertyName = "Cupboard Range")]
            public float cupboardRange;
            [JsonProperty(PropertyName = "Block cupboard wood")]
            public bool blockWood;
            [JsonProperty(PropertyName = "Block cupboard stone")]
            public bool blockStone;
            [JsonProperty(PropertyName = "Block cupbard metal")]
            public bool blockMetal;
            [JsonProperty(PropertyName = "Block cupboard high quality")]
            public bool blockHighQ;
        }

    }

    public class TierMultipliers
    {
        [JsonProperty(PropertyName = "Twig multiplier")]
        public float twig;
        [JsonProperty(PropertyName = "Wood multiplier")]
        public float wood;
        [JsonProperty(PropertyName = "Stone multiplier")]
        public float stone;
        [JsonProperty(PropertyName = "Sheet Metal multiplier")]
        public float metal;
        [JsonProperty(PropertyName = "Armoured multiplier")]
        public float armoured;
    }

    [JsonIgnore]
    public float[] buildingMultipliers;
}

public class GeneralSettings
{
    [JsonProperty(PropertyName = "Disable decay for all entities")]
    public bool disableAll;
    [JsonProperty(PropertyName = "Exclude \"Other Multipliers\"")]
    public bool excludeOthers;
    [JsonProperty(PropertyName = "Use permission")]
    public bool usePermission;
    [JsonProperty(PropertyName = "Decay if there is no owner (and perms enabled)")]
    public bool decayNoOwner;
    [JsonProperty(PropertyName = "Permission")]
    public string permission;
    [JsonProperty(PropertyName = "Output")]
    public OutputClass Output;
    [JsonProperty(PropertyName = "Cupboard Settings")]
    public CupboardSettingsClass CupboardSettings;
    public class OutputClass
    {
        [JsonProperty(PropertyName = "Output to server console")]
        public bool rconOutput;
        [JsonProperty(PropertyName = "Log to file")]
        public bool logToFile;
        [JsonProperty(PropertyName = "Log file name")]
        public string logFileName;
    }

    public class CupboardSettingsClass
    {
        [JsonProperty(PropertyName = "Disable No Decay if resources placed in TC")]
        public bool disableOnResources;
        [JsonProperty(PropertyName = "Require Tool Cupboard")]
        public bool requireTC;
        [JsonProperty(PropertyName = "Any authed on TC")]
        public bool anyAuthed;
        [JsonProperty(PropertyName = "Cupboard Range")]
        public float cupboardRange;
        [JsonProperty(PropertyName = "Block cupboard wood")]
        public bool blockWood;
        [JsonProperty(PropertyName = "Block cupboard stone")]
        public bool blockStone;
        [JsonProperty(PropertyName = "Block cupbard metal")]
        public bool blockMetal;
        [JsonProperty(PropertyName = "Block cupboard high quality")]
        public bool blockHighQ;
    }

}

public class OutputClass
{
    [JsonProperty(PropertyName = "Output to server console")]
    public bool rconOutput;
    [JsonProperty(PropertyName = "Log to file")]
    public bool logToFile;
    [JsonProperty(PropertyName = "Log file name")]
    public string logFileName;
}

public class CupboardSettingsClass
{
    [JsonProperty(PropertyName = "Disable No Decay if resources placed in TC")]
    public bool disableOnResources;
    [JsonProperty(PropertyName = "Require Tool Cupboard")]
    public bool requireTC;
    [JsonProperty(PropertyName = "Any authed on TC")]
    public bool anyAuthed;
    [JsonProperty(PropertyName = "Cupboard Range")]
    public float cupboardRange;
    [JsonProperty(PropertyName = "Block cupboard wood")]
    public bool blockWood;
    [JsonProperty(PropertyName = "Block cupboard stone")]
    public bool blockStone;
    [JsonProperty(PropertyName = "Block cupbard metal")]
    public bool blockMetal;
    [JsonProperty(PropertyName = "Block cupboard high quality")]
    public bool blockHighQ;
}

public class TierMultipliers
{
    [JsonProperty(PropertyName = "Twig multiplier")]
    public float twig;
    [JsonProperty(PropertyName = "Wood multiplier")]
    public float wood;
    [JsonProperty(PropertyName = "Stone multiplier")]
    public float stone;
    [JsonProperty(PropertyName = "Sheet Metal multiplier")]
    public float metal;
    [JsonProperty(PropertyName = "Armoured multiplier")]
    public float armoured;
}


```

---

## NoDroneSway by WhiteThunder - Drones no longer sway in the wind, if they have attachments

```csharp
using Network;

Oxide.Plugins
[Info("No Drone Sway", "WhiteThunder", "1.0.4")]
[Description("Drones no longer sway in the wind, if they have attachments.")]
internal class NoDroneSway : CovalencePlugin
{
    private const int DroneThrottleUpFlag;
    private const int DroneFlyingFlag;
    private readonly object False;
    private void OnEntitySaved(Drone drone, BaseNetworkable.SaveInfo saveInfo);
    private object OnEntityFlagsNetworkUpdate(Drone drone);
    private static class RCUtils
    {
        public static T GetControlledEntity(BasePlayer player);
    }

    private static bool IsControllingDrone(Connection connection, Drone drone);
    private static void SendFlagsUpdate(BaseEntity entity, int flags, SendInfo sendInfo);
    private static int ModifyDroneFlags(BaseEntity drone);
    private static bool ShouldSway(Drone drone);
}

private static class RCUtils
{
    public static T GetControlledEntity(BasePlayer player);
}


```

---

## NoDuds by alibali - Prevents explosives (satchels, beancans) from becoming dud

```csharp
using Newtonsoft.Json;

Oxide.Plugins
[Info("No Duds", "bearr", 1.2)]
[Description("Prevents explosives from becoming dud")]
 class NoDuds : RustPlugin
{
    private GameConfig config;
    protected override void LoadConfig();
    private void Init();
     object OnExplosiveDud(DudTimedExplosive explosive);
    private class GameConfig
    {
        [JsonProperty("Satchel Charge Dud")]
        public bool satcheldud { get; set; }
        [JsonProperty("Beancan Dud")]
        public bool beancandud { get; set; }
    }

    private GameConfig GetDefaultConfig();
    protected override void LoadDefaultConfig();
}

private class GameConfig
{
    [JsonProperty("Satchel Charge Dud")]
    public bool satcheldud { get; set; }
    [JsonProperty("Beancan Dud")]
    public bool beancandud { get; set; }
}


```

---

## NoDurability by  - Allows players with permission to have no durability on every item

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("No Durability", "Wulf/lukespragg/Arainrr", "2.2.5", ResourceId = 1061)]
public class NoDurability : RustPlugin
{
    [PluginReference]
    private readonly Plugin ZoneManager;
    private readonly Plugin DynamicPVP;
    private const string PERMISSION_USE;
    private void Init();
    private void OnLoseCondition(Item item, float amount);
    private bool IsDynamicPVPZone(string zoneID);
    private bool IsPlayerInZone(string zoneID, BasePlayer player);
    private string[] GetPlayerZoneIDs(BasePlayer player);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Use ZoneManager")]
        public bool useZoneManager;
        [JsonProperty(PropertyName = "Keep Max Durability")]
        public bool keepMaxDurability;
        [JsonProperty(PropertyName = "Exclude all zone")]
        public bool excludeAllZone;
        [JsonProperty(PropertyName = "Exclude dynamic pvp zone")]
        public bool excludeDynPVPZone;
        [JsonProperty(PropertyName = "Zone exclude list (Zone ID)")]
        public HashSet<string> zoneList;
        [JsonProperty(PropertyName = "Item list (Item short name)")]
        public HashSet<string> itemList;
        [JsonProperty(PropertyName = "Item list is a blacklist? (If false, it's is a whitelist)")]
        public bool itemListIsBlackList;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Use ZoneManager")]
    public bool useZoneManager;
    [JsonProperty(PropertyName = "Keep Max Durability")]
    public bool keepMaxDurability;
    [JsonProperty(PropertyName = "Exclude all zone")]
    public bool excludeAllZone;
    [JsonProperty(PropertyName = "Exclude dynamic pvp zone")]
    public bool excludeDynPVPZone;
    [JsonProperty(PropertyName = "Zone exclude list (Zone ID)")]
    public HashSet<string> zoneList;
    [JsonProperty(PropertyName = "Item list (Item short name)")]
    public HashSet<string> itemList;
    [JsonProperty(PropertyName = "Item list is a blacklist? (If false, it's is a whitelist)")]
    public bool itemListIsBlackList;
}


```

---

## NoEngineParts by WhiteThunder - Allows modular cars to be driven without engine parts

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Serialization;
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Linq;
using Rust.Modular;

Oxide.Plugins
[Info("No Engine Parts", "WhiteThunder", "1.0.1")]
[Description("Allows modular cars to be driven without engine parts.")]
internal class NoEngineParts : CovalencePlugin
{
    private const string PermissionPresetPrefix;
    private Configuration pluginConfig;
    private void Init();
    private void OnEntityMounted(ModularCarSeat seat);
    private void OnEngineStarted(ModularCar car);
    private object OnEngineLoadoutRefresh(EngineStorage engineStorage);
    private bool OverrideLoadoutWasBlocked(EngineStorage engineStorage);
    private string GetPresetPermission(string presetName);
    private void RefreshCarEngineLoadouts(ModularCar car);
    private bool TryRefreshEngineLoadout(EngineStorage engineStorage, EnginePreset enginePreset);
    private EnginePreset DetermineEnginePresetForStorage(EngineStorage engineStorage);
    private EnginePreset DetermineEnginePresetForOwner(ulong ownerId);
    private Configuration GetDefaultConfig();
    internal class Configuration : SerializableConfiguration
    {
        [JsonProperty("DefaultPreset")]
        public EnginePreset defaultPreset;
        [JsonProperty("PresetsRequiringPermission")]
        public EnginePreset[] presetsRequiringPermission;
    }

    internal class EnginePreset
    {
        [JsonProperty("Name", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string name;
        [JsonProperty("Acceleration")]
        public float acceleration;
        [JsonProperty("TopSpeed")]
        public float topSpeed;
        [JsonProperty("FuelEconomy")]
        public float fuelEconomy;
    }

    internal class SerializableConfiguration
    {
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    internal static class JsonHelper
    {
        public static object Deserialize(string json);
        private static object ToObject(JToken token);
    }

    private bool MaybeUpdateConfig(SerializableConfiguration config);
    private bool MaybeUpdateConfigDict(Dictionary<string, object> currentWithDefaults, Dictionary<string, object> currentRaw);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
}

internal class Configuration : SerializableConfiguration
{
    [JsonProperty("DefaultPreset")]
    public EnginePreset defaultPreset;
    [JsonProperty("PresetsRequiringPermission")]
    public EnginePreset[] presetsRequiringPermission;
}

internal class EnginePreset
{
    [JsonProperty("Name", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string name;
    [JsonProperty("Acceleration")]
    public float acceleration;
    [JsonProperty("TopSpeed")]
    public float topSpeed;
    [JsonProperty("FuelEconomy")]
    public float fuelEconomy;
}

internal class SerializableConfiguration
{
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

internal static class JsonHelper
{
    public static object Deserialize(string json);
    private static object ToObject(JToken token);
}


```

---

## NoEscape by Calytic - Prevent "teleportation" (and more) while in raid or combat

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json.Linq;
using UnityEngine;
using Facepunch;
using Rust;

Oxide.Plugins
[Info ("No Escape", "Calytic", "2.1.35")]
[Description ("Prevent commands/actions while raid and/or combat is occuring")]
 class NoEscape : RustPlugin
{
     List<string> blockTypes;
     bool combatBlock;
    static float combatDuration;
     bool combatOnHitPlayer;
     float combatOnHitPlayerMinCondition;
     float combatOnHitPlayerMinDamage;
     bool combatOnTakeDamage;
     float combatOnTakeDamageMinCondition;
     float combatOnTakeDamageMinDamage;
     bool combatOnHitNPC;
     bool combatOnTakeDamageNPC;
     bool raidBlock;
    static float raidDuration;
     float raidDistance;
     bool blockOnDamage;
     float blockOnDamageMinCondition;
     bool blockOnDestroy;
     bool ownerCheck;
     bool blockUnowned;
     bool blockAll;
     bool ownerBlock;
     bool cupboardShare;
     bool friendShare;
     bool clanShare;
     bool clanCheck;
     bool friendCheck;
     bool raiderBlock;
     List<string> raidDamageTypes;
     List<string> raidDeathTypes;
     List<string> combatDamageTypes;
     bool raidUnblockOnDeath;
     bool raidUnblockOnWakeup;
     bool raidUnblockOnRespawn;
     bool combatUnblockOnDeath;
     bool combatUnblockOnWakeup;
     bool combatUnblockOnRespawn;
     float cacheTimer;
     bool raidBlockNotify;
     bool combatBlockNotify;
     bool useZoneManager;
     bool zoneEnter;
     bool zoneLeave;
     bool useRaidableBases;
     bool raidableZoneEnter;
     bool raidableZoneLeave;
     bool sendUINotification;
     bool sendChatNotification;
     bool sendGUIAnnouncementsNotification;
     bool sendLustyMapNotification;
     string GUIAnnouncementTintColor;
     string GUIAnnouncementTextColor;
     string LustyMapIcon;
     float LustyMapDuration;
     Dictionary<string, RaidZone> zones;
     Dictionary<string, List<string>> memberCache;
     Dictionary<string, string> clanCache;
     Dictionary<string, List<string>> friendCache;
     Dictionary<string, DateTime> lastClanCheck;
     Dictionary<string, DateTime> lastCheck;
     Dictionary<string, DateTime> lastFriendCheck;
     Dictionary<string, bool> prefabBlockCache;
    internal Dictionary<ulong, BlockBehavior> blockBehaviors;
    public static NoEscape plugin;
    [PluginReference]
     Plugin Clans;
     Plugin Friends;
     Plugin ZoneManager;
     Plugin GUIAnnouncements;
     Plugin LustyMap;
    readonly int cupboardMask;
    readonly int blockLayer;
     Dictionary<string, bool> _cachedExcludedWeapons;
     List<string> blockedPrefabs;
     List<string> exceptionPrefabs;
     List<string> exceptionWeapons;
    private List<string> GetDefaultRaidDamageTypes();
    private List<string> GetDefaultCombatDamageTypes();
     Dictionary<string, object> blockWhenRaidDamageDefault;
     Dictionary<string, object> blockWhenCombatDamageDefault;
    static Regex _htmlRegex;
    protected override void LoadDefaultConfig();
     void Loaded();
     void Unload();
     void LoadMessages();
     void CheckConfig();
    protected void ReloadConfig();
     void OnServerInitialized();
     void UnsubscribeHooks();
    public class RaidZone
    {
        public string zoneid;
        public Vector3 position;
        public Timer timer;
        public RaidZone(string zoneid, Vector3 position);
        public float Distance(RaidZone zone);
        public float Distance(Vector3 pos);
        public RaidZone ResetTimer();
    }

    public abstract class BlockBehavior : MonoBehaviour
    {
        protected BasePlayer player;
        public DateTime lastBlock;
        public DateTime lastNotification;
        internal DateTime lastUINotification;
        internal Timer timer;
        internal Action notifyCallback;
        internal string iconUID;
        internal bool moved;
        public void CopyFrom(BlockBehavior behavior);
        internal abstract float Duration { get; set; }
        internal abstract CuiRectTransformComponent NotificationWindow { get; set; }
        internal abstract string notifyMessage { get; set; }
        internal string BlockName { get; set; }
        public bool Active { get; set; }
         void Awake();
         void Destroy();
         void Update();
        public void Stop();
        public void Notify(Action callback);
        private string FormatTime(TimeSpan ts);
         void SendGUI();
    }

    public class CombatBlock : BlockBehavior
    {
        internal override float Duration { get; set; }
        internal override string notifyMessage { get; set; }
         CuiRectTransformComponent _notificationWindow;
        internal override CuiRectTransformComponent NotificationWindow { get; set; }
    }

    public class RaidBlock : BlockBehavior
    {
        internal override float Duration { get; set; }
        internal override string notifyMessage { get; set; }
        private CuiRectTransformComponent _notificationWindow;
        internal override CuiRectTransformComponent NotificationWindow { get; set; }
    }

     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerAttack(BasePlayer attacker, HitInfo hitInfo);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo hitInfo);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnPlayerRespawned(BasePlayer player);
     object CanLootEntity(BasePlayer player, StorageContainer container);
     void StructureAttack(BaseEntity targetEntity, BaseEntity sourceEntity, string weapon, Vector3 hitPosition);
     float GetHealthPercent(BaseEntity entity, float damage);
     void BlockAll(BasePlayer source, BaseEntity targetEntity, List<string> sourceMembers);
     void OwnerBlock(BasePlayer source, BaseEntity sourceEntity, ulong target, Vector3 position, List<string> sourceMembers);
     List<string> CupboardShare(string owner, Vector3 position, BaseEntity sourceEntity, List<string> sourceMembers);
     void RaiderBlock(BasePlayer source, ulong target, Vector3 position, List<string> sourceMembers);
     bool IsBlocked(string target);
     bool IsBlocked(BasePlayer target);
    public bool IsBlocked(BasePlayer target);
     bool IsRaidBlocked(BasePlayer target);
     bool IsCombatBlocked(BasePlayer target);
     bool IsEscapeBlocked(string target);
     bool IsRaidBlocked(string target);
     bool IsCombatBlocked(string target);
     bool ShouldBlockEscape(ulong target, ulong source, List<string> sourceMembers);
     void StartRaidBlocking(BasePlayer target, bool createZone);
     void StartRaidBlocking(BasePlayer target, Vector3 position, bool createZone);
     void StartCombatBlocking(BasePlayer target);
     void StopBlocking(BasePlayer target);
    public void StopBlocking(BasePlayer target);
     void ClearRaidBlockingS(string target);
     void StopRaidBlocking(BasePlayer player);
     void StopRaidBlocking(string target);
     void StopCombatBlocking(BasePlayer player);
     void StopCombatBlocking(string target);
     void ClearCombatBlocking(string target);
     void EraseZone(string zoneid);
     void ResetZoneTimer(RaidZone zone);
     void CreateRaidZone(Vector3 position);
    [HookMethod ("OnEnterZone")]
     void OnEnterZone(string zoneid, BasePlayer player);
    [HookMethod ("OnExitZone")]
     void OnExitZone(string zoneid, BasePlayer player);
    [HookMethod ("OnPlayerEnteredRaidableBase")]
     void OnPlayerEnteredRaidableBase(BasePlayer player, Vector3 raidPos, bool allowPVP);
    [HookMethod ("OnPlayerExitedRaidableBase")]
     void OnPlayerExitedRaidableBase(BasePlayer player, Vector3 raidPos, bool allowPVP);
    public List<string> getFriends(string player);
    public List<string> getFriendList(string player);
    public List<string> getClanMembers(string player);
     JObject GetClan(string tag);
     List<string> CacheClan(JObject clan);
    [HookMethod ("OnClanCreate")]
     void OnClanCreate(string tag);
    [HookMethod ("OnClanUpdate")]
     void OnClanUpdate(string tag);
    [HookMethod ("OnClanDestroy")]
     void OnClanDestroy(string tag);
     bool HasPerm(string userid, string perm);
     bool CanRaidCommand(BasePlayer player, string command);
     bool CanRaidCommand(string playerID, string command);
     bool CanCombatCommand(BasePlayer player, string command);
     bool CanCombatCommand(string playerID, string command);
     object CanDo(string command, BasePlayer player);
     object OnStructureRepair(BaseCombatEntity entity, BasePlayer player);
     object OnStructureUpgrade(BuildingBlock block, BasePlayer player, BuildingGrade.Enum grade);
     object CanRedeemKit(BasePlayer player);
     object CanUseMailbox(BasePlayer player, Mailbox mailbox);
     object CanUseVending(VendingMachine machine, BasePlayer player);
     object CanBuild(Planner plan, Construction prefab);
     object CanAssignBed(SleepingBag bag, BasePlayer player, ulong targetPlayerId);
     object CanOpenBackpack(BasePlayer player, ulong backpackOwnerID);
     object CanBank(BasePlayer player);
     object CanTrade(BasePlayer player);
     object canRemove(BasePlayer player);
     object canShop(BasePlayer player);
     object CanShop(BasePlayer player);
     object CanTeleport(BasePlayer player);
     object canTeleport(BasePlayer player);
     object CanGridTeleport(BasePlayer player);
     object CanCraft(ItemCrafter itemCrafter, ItemBlueprint bp, int amount);
     object CanRecycleCommand(BasePlayer player);
     object CanBGrade(BasePlayer player, int grade, BuildingBlock buildingBlock, Planner planner);
     void SendBlockMessage(BasePlayer target, BlockBehavior blocker, string langMessage, string completeMessage);
     string GetCooldownTime(float f, string userID);
    public string GetMessage(BasePlayer player);
    public string GetPrefix(string player);
    public string GetMessage(BasePlayer player, string blockMsg, float duration);
     bool TryGetBlocker(BasePlayer player, T blocker);
    public bool isEntityException(string prefabName);
    public bool IsEntityBlocked(BaseCombatEntity entity);
     bool IsRaidDamage(DamageType dt);
     bool IsDeathDamage(DamageType dt);
     bool IsRaidDamage(DamageTypeList dtList);
     bool IsDeathDamage(DamageTypeList dtList);
     bool IsExcludedWeapon(string name);
     bool IsCombatDamage(DamageType dt);
     bool IsCombatDamage(DamageTypeList dtList);
     T GetConfig(string name, string name2, string name3, T defaultValue);
     T GetConfig(string name, string name2, T defaultValue);
     T GetConfig(string name, T defaultValue);
     T ParseValue(object val, T defaultValue);
    static string GetMsg(string key, object user);
}

public class RaidZone
{
    public string zoneid;
    public Vector3 position;
    public Timer timer;
    public RaidZone(string zoneid, Vector3 position);
    public float Distance(RaidZone zone);
    public float Distance(Vector3 pos);
    public RaidZone ResetTimer();
}

public abstract class BlockBehavior : MonoBehaviour
{
    protected BasePlayer player;
    public DateTime lastBlock;
    public DateTime lastNotification;
    internal DateTime lastUINotification;
    internal Timer timer;
    internal Action notifyCallback;
    internal string iconUID;
    internal bool moved;
    public void CopyFrom(BlockBehavior behavior);
    internal abstract float Duration { get; set; }
    internal abstract CuiRectTransformComponent NotificationWindow { get; set; }
    internal abstract string notifyMessage { get; set; }
    internal string BlockName { get; set; }
    public bool Active { get; set; }
     void Awake();
     void Destroy();
     void Update();
    public void Stop();
    public void Notify(Action callback);
    private string FormatTime(TimeSpan ts);
     void SendGUI();
}

public class CombatBlock : BlockBehavior
{
    internal override float Duration { get; set; }
    internal override string notifyMessage { get; set; }
     CuiRectTransformComponent _notificationWindow;
    internal override CuiRectTransformComponent NotificationWindow { get; set; }
}

public class RaidBlock : BlockBehavior
{
    internal override float Duration { get; set; }
    internal override string notifyMessage { get; set; }
    private CuiRectTransformComponent _notificationWindow;
    internal override CuiRectTransformComponent NotificationWindow { get; set; }
}


```

---

## NoExperiments by  - Blocks experiments on workbench

```csharp
using Newtonsoft.Json;

Oxide.Plugins
[Info("No Experiments", "Orange", "1.0.1")]
[Description("Plugin disables experiments in workbenches")]
public class NoExperiments : RustPlugin
{
    private object CanExperiment(BasePlayer player, Workbench workbench);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Block on lvl 1")]
        public bool block1;
        [JsonProperty(PropertyName = "Block on lvl 2")]
        public bool block2;
        [JsonProperty(PropertyName = "Block on lvl 3")]
        public bool block3;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Block on lvl 1")]
    public bool block1;
    [JsonProperty(PropertyName = "Block on lvl 2")]
    public bool block2;
    [JsonProperty(PropertyName = "Block on lvl 3")]
    public bool block3;
}


```

---

## NoFireBallDamage by Lincoln - Prevents fireballs and splash fire from damaging entities

```csharp
using Rust;

Oxide.Plugins
[Info("No FireBall Damage", "Lincoln", "1.0.2")]
[Description("Prevent fireballs from damaging things.")]
public class NoFireBallDamage : CovalencePlugin
{
     void OnFireBallDamage(FireBall fire, BaseCombatEntity entity, HitInfo info);
}


```

---

## NoFireworksOnTerrain by 0x89A - Restricts/prevents placement of fireworks on terrain

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("No Fireworks On Terrain", "0x89A", "1.0.4")]
[Description("Restricts placement of fireworks")]
public class NoFireworksOnTerrain : RustPlugin
{
    const string bypassPermission;
    private HashSet<uint> fireworkPrefabIds;
     void Init();
    protected override void LoadDefaultMessages();
    private object CanBuild(Planner planner, Construction prefab, Construction.Target target);
}


```

---

## NoFlykick by August - Allows players with permission to override flyhack kicks

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("No Flykick", "August", "1.3.4")]
[Description("Allows players with permission to bypass flyhack kicks.")]
 class NoFlykick : RustPlugin
{
    private const string permUse;
    private const string permManage;
    private bool IsEnabled;
    private void Init();
    protected override void LoadDefaultMessages();
    private void GetStatus(BasePlayer player);
    private void Toggle(BasePlayer player);
    private string Lang(string key, string id, object[] args);
    [ChatCommand("nokick")]
    private void NoKickChatCommand(BasePlayer player, string cmd, string[] args);
     object OnPlayerViolation(BasePlayer player, AntiHackType type, float amount);
}


```

---

## NoFoundationObjects by mvrb - Blocks objects under foundations

```csharp
using System.Collections.Generic;
using UnityEngine;
using System.Linq;
using Newtonsoft.Json;

Oxide.Plugins
[Info("NoFoundationObjects", "mvrb", "1.0.0")]
[Description("Blocks objects under foundations")]
 class NoFoundationObjects : RustPlugin
{
    private Dictionary<string, string> deployables;
    private void LoadDefaultMessages();
    private void OnServerInitialized();
    private void OnEntityBuilt(Planner planner, GameObject obj);
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Prevent building above Small Stashes")]
        public bool PreventBuildAboveStash { get; set; }
        [JsonProperty(PropertyName = "List of blocked objects")]
        public List<string> BlackList { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
    private string Lang(string key, string id, object[] args);
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Prevent building above Small Stashes")]
    public bool PreventBuildAboveStash { get; set; }
    [JsonProperty(PropertyName = "List of blocked objects")]
    public List<string> BlackList { get; set; }
}


```

---

## NoFuelRequirements by k1lly0u - Allows players to choose which deployables do not use fuel

```csharp
using Oxide.Core;
using Newtonsoft.Json;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("NoFuelRequirements", "k1lly0u", "1.8.0")]
 class NoFuelRequirements : RustPlugin
{
    private readonly string[] ValidFuelTypes;
    private readonly ConsumeType[] ObsoleteConsumeTypes;
    private void Loaded();
    private void OnFuelConsume(BaseOven baseOven, Item fuelItem, ItemModBurnable itemModBurnable);
    private void OnItemUse(Item item, int amount);
    private object IgnoreFuelConsumption(string consumeTypeStr, ulong ownerId);
    private bool HasPermission(string userId, ConsumeType consumeType);
    private bool HasPermission(ulong userId, ConsumeType consumeType);
    private bool IsConsumeTypeEnabled(ConsumeType consumeType);
    private ConsumeType ShortNameToConsumeType(string shortname);
    private ConsumeType ParseType(string type);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Entities that ignore fuel consumption")]
        public Dictionary<ConsumeType, bool> AffectedTypes { get; set; }
        [JsonProperty(PropertyName = "Require permission to ignore fuel consumption")]
        public bool UsePermissions { get; set; }
        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Entities that ignore fuel consumption")]
    public Dictionary<ConsumeType, bool> AffectedTypes { get; set; }
    [JsonProperty(PropertyName = "Require permission to ignore fuel consumption")]
    public bool UsePermissions { get; set; }
    public Oxide.Core.VersionNumber Version { get; set; }
}


```

---

## NoGiveGametip by noname - Removes the gametip when you create an item with the F1 console

```csharp
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("No Give Gametip", "noname", "1.0.1")]
[Description("Clears the GameTip from using the giveid command.")]
 class NoGiveGametip : CovalencePlugin
{
    private void OnServerCommand(ConsoleSystem.Arg arg);
}


```

---

## NoGiveNotices by MrBlue - Prevents F1 item giving notices from showing in the chat

```csharp

Oxide.Plugins
[Info("No Give Notices", "Wulf", "0.3.0")]
[Description("Prevents F1 item giving notices from showing in the chat")]
 class NoGiveNotices : RustPlugin
{
    private object OnServerMessage(string message, string name);
}


```

---

## NoGreen by misticos - Remove admins' green names

```csharp
using System;
using CompanionServer;
using ConVar;
using Facepunch;
using Facepunch.Math;
using UnityEngine;

Oxide.Plugins
[Info("No Green", "Iv Misticos", "1.3.10")]
[Description("Remove admins' green names")]
 class NoGreen : RustPlugin
{
    private bool OnPlayerChat(BasePlayer player, string message, Chat.ChatChannel channel);
}


```

---

## NoHeliFire by Tryhard - Removes the explosion sound, gibs and fire effect from mini-, attack- and scrap helicopters

```csharp
using Newtonsoft.Json;

Oxide.Plugins
[Info("No Heli Fire", "Tryhard", "1.2.6")]
[Description("Optionally removes the explosion sound, gibs and fire effect from mini- and scrap helicopters")]
public class NoHeliFire : RustPlugin
{
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Disable minicopter gibs")]
        public bool mGibs;
        [JsonProperty(PropertyName = "Disable minicopter fire")]
        public bool mFire;
        [JsonProperty(PropertyName = "Disable minicopter explosion sound")]
        public bool mExplo;
        [JsonProperty(PropertyName = "Disable scraphelicopter gibs")]
        public bool sGibs;
        [JsonProperty(PropertyName = "Disable scraphelicopter explosion sound")]
        public bool sExplo;
        [JsonProperty(PropertyName = "Disable scraphelicopter fire ")]
        public bool sFire;
        [JsonProperty(PropertyName = "Disable attackhelicopter gibs")]
        public bool aGibs;
        [JsonProperty(PropertyName = "Disable attackhelicopter explosion sound")]
        public bool aExplo;
        [JsonProperty(PropertyName = "Disable attackhelicopter fire ")]
        public bool aFire;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnEntitySpawned(ScrapTransportHelicopter entity);
    private void OnEntitySpawned(Minicopter entity);
    private void OnEntitySpawned(AttackHelicopter entity);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Disable minicopter gibs")]
    public bool mGibs;
    [JsonProperty(PropertyName = "Disable minicopter fire")]
    public bool mFire;
    [JsonProperty(PropertyName = "Disable minicopter explosion sound")]
    public bool mExplo;
    [JsonProperty(PropertyName = "Disable scraphelicopter gibs")]
    public bool sGibs;
    [JsonProperty(PropertyName = "Disable scraphelicopter explosion sound")]
    public bool sExplo;
    [JsonProperty(PropertyName = "Disable scraphelicopter fire ")]
    public bool sFire;
    [JsonProperty(PropertyName = "Disable attackhelicopter gibs")]
    public bool aGibs;
    [JsonProperty(PropertyName = "Disable attackhelicopter explosion sound")]
    public bool aExplo;
    [JsonProperty(PropertyName = "Disable attackhelicopter fire ")]
    public bool aFire;
}


```

---

## NoHeliFlyhack by DoobySkoo - Prevents players getting kicked for flyhacking after dismounting helicopters

```csharp

Oxide.Plugins
[Info("No Heli Flyhack", "Dooby Skoo", "1.2.1")]
[Description("Prevents players getting kicked for flyhacking after dismounting helicopters.")]
public class NoHeliFlyhack : RustPlugin
{
    private const string FHPerm;
    private void Init();
    private void OnEntityDismounted(BaseNetworkable entity, BasePlayer player);
}


```

---

## NoIgniterDrain by Lincoln - Prevents the igniter from damaging itself while in use

```csharp

Oxide.Plugins
[Info("No Igniter Drain", "Lincoln", "1.0.3")]
[Description("Prevent Igniters from damaging themselves while in use.")]
public class NoIgniterDrain : RustPlugin
{
    private void OnServerInitialized();
    private void OnEntitySpawned(Igniter entity);
    private void Unload();
}


```

---

## NoLocks by VisEntities - Blocks the deployment of key and code locks on certain entities

```csharp
using Newtonsoft.Json;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("No Locks", "VisEntities", "3.0.0")]
[Description("Blocks the deployment of key and code locks on certain entities.")]
public class NoLocks : RustPlugin
{
    private static NoLocks _plugin;
    private static Configuration _config;
    private const int ITEM_ID_KEY_LOCK;
    private const int ITEM_ID_CODE_LOCK;
    private class Configuration
    {
        [JsonProperty("Version")]
        public string Version { get; set; }
        [JsonProperty("Remove Locks On Startup")]
        public bool RemoveLocksOnStartup { get; set; }
        [JsonProperty("Entity Groups")]
        public List<LockConfig> EntityGroups { get; set; }
    }

    private class LockConfig
    {
        [JsonProperty("Prefab Short Names")]
        public List<string> PrefabShortNames { get; set; }
        [JsonProperty("Allow Code Lock Deployment")]
        public bool AllowCodeLockDeployment { get; set; }
        [JsonProperty("Allow Key Lock Deployment")]
        public bool AllowKeyLockDeployment { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfig();
    private Configuration GetDefaultConfig();
    private void Init();
    private void Unload();
    private void OnServerInitialized(bool isStartup);
    private object CanDeployItem(BasePlayer player, Deployer deployerItem, NetworkableId targetEntityId);
    private IEnumerator RemoveLocksCoroutine();
    private LockConfig GetLockConfigForPrefab(string prefabName);
    private static BaseLock GetEntityLock(BaseEntity entity);
    public static BasePlayer FindPlayerById(ulong playerId);
    private static class CoroutineUtil
    {
        private static readonly Dictionary<string, Coroutine> _activeCoroutines;
        public static void StartCoroutine(string coroutineName, IEnumerator coroutineFunction);
        public static void StopCoroutine(string coroutineName);
        public static void StopAllCoroutines();
    }

    private static class PermissionUtil
    {
        public const string BYPASS;
        private static readonly List<string> _permissions;
        public static void RegisterPermissions();
        public static bool HasPermission(BasePlayer player, string permissionName);
    }

    private class Lang
    {
        public const string DeployCodeLockBlocked;
        public const string DeployKeyLockBlocked;
    }

    protected override void LoadDefaultMessages();
    private void SendMessage(BasePlayer player, string messageKey, object[] args);
}

private class Configuration
{
    [JsonProperty("Version")]
    public string Version { get; set; }
    [JsonProperty("Remove Locks On Startup")]
    public bool RemoveLocksOnStartup { get; set; }
    [JsonProperty("Entity Groups")]
    public List<LockConfig> EntityGroups { get; set; }
}

private class LockConfig
{
    [JsonProperty("Prefab Short Names")]
    public List<string> PrefabShortNames { get; set; }
    [JsonProperty("Allow Code Lock Deployment")]
    public bool AllowCodeLockDeployment { get; set; }
    [JsonProperty("Allow Key Lock Deployment")]
    public bool AllowKeyLockDeployment { get; set; }
}

private static class CoroutineUtil
{
    private static readonly Dictionary<string, Coroutine> _activeCoroutines;
    public static void StartCoroutine(string coroutineName, IEnumerator coroutineFunction);
    public static void StopCoroutine(string coroutineName);
    public static void StopAllCoroutines();
}

private static class PermissionUtil
{
    public const string BYPASS;
    private static readonly List<string> _permissions;
    public static void RegisterPermissions();
    public static bool HasPermission(BasePlayer player, string permissionName);
}

private class Lang
{
    public const string DeployCodeLockBlocked;
    public const string DeployKeyLockBlocked;
}


```

---

## NoLoot by MrBlue - Removes all loot containers and prevents them from spawning

```csharp

Oxide.Plugins
[Info("No Loot", "Wulf", "1.1.0")]
[Description("Removes all loot containers and prevents them from spawning")]
 class NoLoot : CovalencePlugin
{
    private void Init();
    private void OnServerInitialized();
    private bool ProcessEntity(BaseEntity entity);
    private void OnEntitySpawned(JunkPile junkPile);
    private void OnEntitySpawned(LootContainer container);
}


```

---

## NoMedicalHealing by  - Simple plugin to null medical healing on player with permission

```csharp

Oxide.Plugins
[Info("No Medical Healing", "BuzZ", "0.0.1")]
[Description("Null medical healing")]
public class NoMedicalHealing : RustPlugin
{
     bool debug;
    const string CanNotUseMedical;
     void Init();
     object OnHealingItemUse(MedicalTool tool, BasePlayer player);
}


```

---

## NoMetabolism by MONaH - Allows the player to have a consistent metabolism

```csharp
using Newtonsoft.Json;

Oxide.Plugins
[Info("No Metabolism", "MON@H", "1.1.3")]
[Description("Allows the player to have a consistent metabolism.")]
public class NoMetabolism : CovalencePlugin
{
    private const string PermissionUse;
    private void Init();
    private ConfigData _configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Activate for all players without permission")]
        public bool AllAllowed;
        [JsonProperty(PropertyName = "Activate for admins without permission")]
        public bool AdminsAllowed;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     object OnRunPlayerMetabolism(PlayerMetabolism metabolism, BasePlayer player);
    private object HandlePlayerMetabolism(PlayerMetabolism metabolism, BasePlayer player);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Activate for all players without permission")]
    public bool AllAllowed;
    [JsonProperty(PropertyName = "Activate for admins without permission")]
    public bool AdminsAllowed;
}


```

---

## NoMini by Sche1sseHund - Prevents players from using mini or scrap helicopters or prevent them from spawning altogether

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("No Mini", "Sche1sseHund", 0.9)]
[Description("Prevents users from using/buying mini and scrap transport helicopters")]
 class NoMini : RustPlugin
{
    private ConfigFile CfgFile;
    private const string PERMISSION_ALLOWMINI;
    private const string PERMISSION_ALLOWATTACK;
    private const string PERMISSION_ALLOWSCRAPPY;
    private const string PERMISSION_ALLOWAIRWOLFVEND;
    private const string PREFAB_MINICOPTER;
    private const string PREFAB_SCRAPTRANSPORT;
    private const string PREFAB_ATTACKCOPTER;
    private const string PREFAB_BANDITNPC1;
    private const string PREFAB_BANDITNPC1_SHORT;
     void Init();
     object OnEngineStart(BaseVehicle vehicle, BasePlayer driver);
     void OnEntitySpawned(BaseVehicle entity);
     void KillMini(BaseVehicle entity);
     object OnNpcConversationStart(NPCTalking npcTalking, BasePlayer player, ConversationData conversationData);
    public class ConfigFile
    {
        public bool KillMiniOnSpawn { get; set; }
        public bool KillAttackOnSpawn { get; set; }
        public bool KillScrappyOnSpawn { get; set; }
        public bool ExplodeOnKill { get; set; }
        public bool DisableBanditVendor { get; set; }
        public ConfigFile();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void CreateNewConfig();
    protected override void LoadDefaultMessages();
}

public class ConfigFile
{
    public bool KillMiniOnSpawn { get; set; }
    public bool KillAttackOnSpawn { get; set; }
    public bool KillScrappyOnSpawn { get; set; }
    public bool ExplodeOnKill { get; set; }
    public bool DisableBanditVendor { get; set; }
    public ConfigFile();
}


```

---

## NoMlgWater by Chernikov - Applies fall damage when falling into the water

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("No MLG Water", "Polybro", "1.0.2")]
[Description("Adds fall damage when falling into the water")]
public class NoMlgWater : CovalencePlugin
{
    private static ConfigData config;
     List<ulong> pool;
     object OnPlayerTick(BasePlayer player, PlayerTick msg, bool wasPlayerStalled);
    private void Unload();
    [Command("nmw.reload")]
    [Permission("nomlgwater.admin")]
    private void ReloadConfig(IPlayer iplayer, string command, string[] args);
    private void ApplyFallDamage(BasePlayer player);
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    public class ConfigData
    {
        [JsonProperty(PropertyName = "Damage Exposure")]
        public float DamageExposure { get; set; }
        [JsonProperty(PropertyName = "Minimum damage speed (>= 0)")]
        public float MinimumSpeed { get; set; }
        [JsonProperty(PropertyName = "Decrease enter damage (change only if minimum speed >= 15.0)")]
        public bool DecreaseEnterDamage { get; set; }
        [JsonProperty(PropertyName = "Allow wounded state after hitting the water too hard")]
        public bool EnableWounded { get; set; }
    }

    protected override void LoadDefaultMessages();
    private void Respond(BasePlayer Player, string key);
}

public class ConfigData
{
    [JsonProperty(PropertyName = "Damage Exposure")]
    public float DamageExposure { get; set; }
    [JsonProperty(PropertyName = "Minimum damage speed (>= 0)")]
    public float MinimumSpeed { get; set; }
    [JsonProperty(PropertyName = "Decrease enter damage (change only if minimum speed >= 15.0)")]
    public bool DecreaseEnterDamage { get; set; }
    [JsonProperty(PropertyName = "Allow wounded state after hitting the water too hard")]
    public bool EnableWounded { get; set; }
}


```

---

## NoMLRSMount by Lincoln - Prevent players without permission from using the MLRS

```csharp
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core;
using System.Linq;
using Newtonsoft.Json;
using UnityEngine;
using Rust;

Oxide.Plugins
[Info("No MLRS Mount", "Lincoln", "1.0.4")]
[Description("Disallows people from using the MLRS.")]
 class NoMLRSMount : RustPlugin
{
    private const string permBypass;
    private void Init();
     object CanMountEntity(BasePlayer player, MLRS entity);
    private void ChatMessage(BasePlayer player, string messageName, object[] args);
    private string GetMessage(IPlayer player, string messageName, object[] args);
    private string GetMessage(string playerId, string messageName, object[] args);
    protected override void LoadDefaultMessages();
}


```

---

## NoNudity by MrBlue - Forces players to wear clothing, and gives them some if they do not have any

```csharp
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("NoNudity", "Absolut", "1.0.3", ResourceId = 2394)]
 class NoNudity : RustPlugin
{
     void OnServerInitialized();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerRespawned(BasePlayer player);
     void GivePants(BasePlayer player);
     object CanMoveItem(Item item, PlayerInventory playerLoot, uint targetContainer, int targetSlot);
     void MovePants(Item item, ItemContainer cont);
     object OnItemAction(Item item, string cmd);
    private List<string> Pants;
    private ConfigData configData;
     class ConfigData
    {
        public string TypeOfPants_Shortname { get; set; }
        public ulong PantsSkin { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
}

 class ConfigData
{
    public string TypeOfPants_Shortname { get; set; }
    public ulong PantsSkin { get; set; }
}


```

---

## NoobGroup by MrBlue - Adds new players to a temporary group, and permanent group on return

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;

Oxide.Plugins
[Info("Noob Group", "Wulf", "2.0.0")]
[Description("Adds new players to a temporary group, and permanent group on return")]
public class NoobGroup : CovalencePlugin
{
    private Configuration config;
    private class CustomGroup
    {
        public string Name;
        public string Title;
        public int Rank;
        public CustomGroup(string name, string title, int rank);
    }

    private class Configuration
    {
        [JsonProperty("Noob group")]
        public CustomGroup NoobGroup;
        [JsonProperty("Returning group")]
        public CustomGroup ReturningGroup;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private const string permReset;
    private void Init();
    private void OnUserConnected(IPlayer player);
    private void GroupSetup(bool reset);
    private void CommandReset(IPlayer player, string command);
    private void AddLocalizedCommand(string command);
    private string CleanId(string playerId);
    private string GetLang(string langKey, string playerId, object[] args);
    private void Message(IPlayer player, string textOrLang, object[] args);
}

private class CustomGroup
{
    public string Name;
    public string Title;
    public int Rank;
    public CustomGroup(string name, string title, int rank);
}

private class Configuration
{
    [JsonProperty("Noob group")]
    public CustomGroup NoobGroup;
    [JsonProperty("Returning group")]
    public CustomGroup ReturningGroup;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## NoobMessages by FastBurst - Displays a message when a player joins the server for the first time

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Newtonsoft.Json;

Oxide.Plugins
[Info("NoobMessages", "FastBurst", "2.0.3")]
[Description("Displays a message when a player joins the server for the first time")]
 class NoobMessages : RustPlugin
{
     List<ulong> allplayers;
     List<BasePlayer> unAwake;
    private static NoobMessages ins { get; set; }
    private void Init();
    private void Loaded();
    private void OnServerSave();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerSleepEnded(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void Unload();
    private static ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "General Settings")]
        public GeneralOptions General { get; set; }
        public class GeneralOptions
        {
            [JsonProperty(PropertyName = "Announce to all players")]
            public bool Announce { get; set; }
            [JsonProperty(PropertyName = "Display a welcome message for new player")]
            public bool Welcome { get; set; }
            [JsonProperty(PropertyName = "Enable Chat Prefix")]
            public bool enablePrefix { get; set; }
            [JsonProperty(PropertyName = "Chat Prefix")]
            public string Prefix { get; set; }
            [JsonProperty(PropertyName = "Chat Icon (example 7656110000000000)")]
            public ulong ChatIcon { get; set; }
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
     T GetConfig(string name, T defaultValue);
    private void SaveData();
    private void LoadData();
    private void SendMessage(BasePlayer player, string key, object[] args);
    private void SendChatMessage(string key, object[] args);
    private static string msg(string key, string playerId);
    private Dictionary<string, string> Messages;
}

private class ConfigData
{
    [JsonProperty(PropertyName = "General Settings")]
    public GeneralOptions General { get; set; }
    public class GeneralOptions
    {
        [JsonProperty(PropertyName = "Announce to all players")]
        public bool Announce { get; set; }
        [JsonProperty(PropertyName = "Display a welcome message for new player")]
        public bool Welcome { get; set; }
        [JsonProperty(PropertyName = "Enable Chat Prefix")]
        public bool enablePrefix { get; set; }
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string Prefix { get; set; }
        [JsonProperty(PropertyName = "Chat Icon (example 7656110000000000)")]
        public ulong ChatIcon { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class GeneralOptions
{
    [JsonProperty(PropertyName = "Announce to all players")]
    public bool Announce { get; set; }
    [JsonProperty(PropertyName = "Display a welcome message for new player")]
    public bool Welcome { get; set; }
    [JsonProperty(PropertyName = "Enable Chat Prefix")]
    public bool enablePrefix { get; set; }
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string Prefix { get; set; }
    [JsonProperty(PropertyName = "Chat Icon (example 7656110000000000)")]
    public ulong ChatIcon { get; set; }
}


```

---

## NoobQueueBypass by Ryan - Allows new players to skip the queue

```csharp
using System;
using Oxide.Core;

Oxide.Plugins
[Info("Noob Queue Bypass", "Ryan", "1.0.01")]
[Description("Allows new players to skip the queue")]
 class NoobQueueBypass : RustPlugin
{
    private string perm;
     void Init();
     object CanBypassQueue(Network.Connection connection);
     void OnPlayerConnected(BasePlayer player);
}


```

---

## NoPickupPenalty by VisEntities - Disables condition loss for deployables when picked up

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;

Oxide.Plugins
[Info("No Pickup Penalty", "VisEntities", "2.0.0")]
[Description("Disables condition loss for deployables when picked up.")]
public class NoPickupPenalty : RustPlugin
{
    private static NoPickupPenalty _plugin;
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty("Version")]
        public string Version { get; set; }
        [JsonProperty("Unbreakable Entities")]
        public List<string> UnbreakableEntities { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfig();
    private Configuration GetDefaultConfig();
    private void Init();
    private void Unload();
    private object CanPickupEntity(BasePlayer player, BaseCombatEntity entity);
    private static class PermissionUtil
    {
        public const string USE;
        private static readonly List<string> _permissions;
        public static void RegisterPermissions();
        public static bool HasPermission(BasePlayer player, string permissionName);
    }

}

private class Configuration
{
    [JsonProperty("Version")]
    public string Version { get; set; }
    [JsonProperty("Unbreakable Entities")]
    public List<string> UnbreakableEntities { get; set; }
}

private static class PermissionUtil
{
    public const string USE;
    private static readonly List<string> _permissions;
    public static void RegisterPermissions();
    public static bool HasPermission(BasePlayer player, string permissionName);
}


```

---

## NoRaid by Ryan - Prevents players from destroying buildings they're not associated with

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("NoRaid", "Ryan", "1.3.22", ResourceId = 2530)]
[Description("Prevents players destroying buildings of those they're not associated with")]
 class NoRaid : RustPlugin
{
    private string Lang(string key, string id, object[] args);
    [PluginReference]
    private Plugin Friends;
    private Plugin Clans;
    private static ConfigFile cFile;
    private static NoRaid _instance;
    private bool _canWipeRaid;
    private bool _canRaid;
    private float _startMinutes;
    private Timer _uiTimer;
    private Timer _startTimer;
    private Timer _wipeCheckTimer;
    private Timer _startUiTimer;
    private DateTime _cachedWipeTime;
    private DateTime _cachedRaidTime;
    private HashSet<ulong> _uiPlayers;
    private CuiElementContainer _cachedContainer;
    private const string _uiParent;
    private const string _timerParent;
    private const string _perm;
    private class ConfigFile
    {
        public FriendBypass FriendBypass;
        public bool StopAllRaiding;
        public WipeRaiding WipeRaiding;
        public NoRaidCommand NoRaidCommand;
        public UiSettings Ui;
        public static ConfigFile DefaultConfig();
    }

    private class FriendBypass
    {
        public bool Enabled { get; set; }
        public FriendsAPI FriendsApi { get; set; }
        public RustIOClans RustIoClans { get; set; }
        public PlayerOwner PlayerOwner { get; set; }
    }

    private class FriendsAPI
    {
        public bool Enabled { get; set; }
    }

    private class RustIOClans
    {
        public bool Enabled { get; set; }
    }

    private class PlayerOwner
    {
        public bool Enabled { get; set; }
    }

    private class WipeRaiding
    {
        public bool Enabled { get; set; }
        [JsonProperty("Amount of time from wipe people can raid (minutes)")]
        public float MinsFromWipe { get; set; }
        [JsonProperty("Amount of seconds to check if players can raid")]
        public float CheckInterval { get; set; }
    }

    private class NoRaidCommand
    {
        public bool Enabled { get; set; }
        public float DefaultMin { get; set; }
        public float CheckInterval { get; set; }
    }

    private class UiSettings
    {
        public bool Enabled { get; set; }
        public float RefreshInterval { get; set; }
        public Rgba PrimaryColor { get; set; }
        public Rgba DarkColor { get; set; }
        public Anchor AnchorMin { get; set; }
        public Anchor AnchorMax { get; set; }
        public Rgba TextColor { get; set; }
    }

    private class Rgba
    {
        public float R { get; set; }
        public float G { get; set; }
        public float B { get; set; }
        public float A { get; set; }
        public Rgba();
        public Rgba(float r, float g, float b, float a);
        public static string Format(Rgba rgba);
    }

    private class Anchor
    {
        public float X { get; set; }
        public float Y { get; set; }
        public Anchor();
        public Anchor(float x, float y);
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private string GetFormattedTime(double time);
    private void StartPeriod();
    private void StopPeriod();
    private class UI
    {
        public static CuiElementContainer Container(string name, string bgColor, Anchor Min, Anchor Max, string parent, float fadeOut, float fadeIn);
        public static void Text(string name, string parent, CuiElementContainer container, TextAnchor anchor, string color, int fontSize, string text, Anchor Min, Anchor Max, string font, float fadeOut, float fadeIn);
        public static void Element(string name, string parent, CuiElementContainer container, Anchor Min, Anchor Max, string bgColor, float fadeOut, float fadeIn);
        public static void Image(string name, string parent, CuiElementContainer container, Anchor Min, Anchor Max, string img, string color);
        public static bool ShouldDestroy(DateTime time, float minutes);
        public static void ConstructCachedUi();
        public static CuiElementContainer ConstructTimer(DateTime time, float minutes);
    }

    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    [ChatCommand("canraid")]
    private void RaidCmd(BasePlayer player, string command, string[] args);
    [ChatCommand("noraid")]
    private void NoRaidCmd(BasePlayer player, string command, string[] args);
    [ConsoleCommand("noraid.start")]
    private void ConsoleStartCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("noraid.stop")]
    private void ConsoleStopCommand(ConsoleSystem.Arg arg);
}

private class ConfigFile
{
    public FriendBypass FriendBypass;
    public bool StopAllRaiding;
    public WipeRaiding WipeRaiding;
    public NoRaidCommand NoRaidCommand;
    public UiSettings Ui;
    public static ConfigFile DefaultConfig();
}

private class FriendBypass
{
    public bool Enabled { get; set; }
    public FriendsAPI FriendsApi { get; set; }
    public RustIOClans RustIoClans { get; set; }
    public PlayerOwner PlayerOwner { get; set; }
}

private class FriendsAPI
{
    public bool Enabled { get; set; }
}

private class RustIOClans
{
    public bool Enabled { get; set; }
}

private class PlayerOwner
{
    public bool Enabled { get; set; }
}

private class WipeRaiding
{
    public bool Enabled { get; set; }
    [JsonProperty("Amount of time from wipe people can raid (minutes)")]
    public float MinsFromWipe { get; set; }
    [JsonProperty("Amount of seconds to check if players can raid")]
    public float CheckInterval { get; set; }
}

private class NoRaidCommand
{
    public bool Enabled { get; set; }
    public float DefaultMin { get; set; }
    public float CheckInterval { get; set; }
}

private class UiSettings
{
    public bool Enabled { get; set; }
    public float RefreshInterval { get; set; }
    public Rgba PrimaryColor { get; set; }
    public Rgba DarkColor { get; set; }
    public Anchor AnchorMin { get; set; }
    public Anchor AnchorMax { get; set; }
    public Rgba TextColor { get; set; }
}

private class Rgba
{
    public float R { get; set; }
    public float G { get; set; }
    public float B { get; set; }
    public float A { get; set; }
    public Rgba();
    public Rgba(float r, float g, float b, float a);
    public static string Format(Rgba rgba);
}

private class Anchor
{
    public float X { get; set; }
    public float Y { get; set; }
    public Anchor();
    public Anchor(float x, float y);
}

private class UI
{
    public static CuiElementContainer Container(string name, string bgColor, Anchor Min, Anchor Max, string parent, float fadeOut, float fadeIn);
    public static void Text(string name, string parent, CuiElementContainer container, TextAnchor anchor, string color, int fontSize, string text, Anchor Min, Anchor Max, string font, float fadeOut, float fadeIn);
    public static void Element(string name, string parent, CuiElementContainer container, Anchor Min, Anchor Max, string bgColor, float fadeOut, float fadeIn);
    public static void Image(string name, string parent, CuiElementContainer container, Anchor Min, Anchor Max, string img, string color);
    public static bool ShouldDestroy(DateTime time, float minutes);
    public static void ConstructCachedUi();
    public static CuiElementContainer ConstructTimer(DateTime time, float minutes);
}


```

---

## NoRaidRepair by Ryan - Prevents the player from repairing their base whilst raidblocked.

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("NoRaidRepair", "Ryan", "1.0.0")]
[Description("Prevents the player from repairing their base whilst raidblocked.")]
 class NoRaidRepair : RustPlugin
{
    [PluginReference]
     RustPlugin NoEscape;
     void LoadDefaultMessages();
     bool isRaidBlocked(BasePlayer player);
     void OnHammerHit(BasePlayer player, HitInfo info);
     object OnStructureRepair(BaseCombatEntity entity, BasePlayer player);
     string Lang(string key, string id, object[] args);
}


```

---

## NoRecoil by Jacob - Assists in detecting no recoil violations and scripts such as AutoHotkey

```csharp
using System;
using System.Collections.Generic;
using ProtoBuf;
using System.Linq;

Oxide.Plugins
[Info("NoRecoil", "Kappasaurus", "1.0.6")]
 class NoRecoil : RustPlugin
{
    private bool banEnabled;
    private bool kickEnabled;
    private float recoilTimer;
    private readonly Dictionary<ulong, NoRecoilData> data;
    private readonly Dictionary<ulong, Timer> detections;
    private readonly int detectionDiscardSeconds;
    private readonly int violationProbability;
    private readonly int maximumViolations;
    private readonly Dictionary<string, int> probabilityModifiers;
    private readonly List<string> blacklistedAttachments;
    public class NoRecoilData
    {
        public int Ticks;
        public int Count;
        public int Violations;
    }

     void Init();
    private void OnWeaponFired(BaseProjectile projectile, BasePlayer player, ItemModProjectile mod, ProjectileShoot projectileShoot);
    private new void LoadConfig();
    protected override void LoadDefaultConfig();
    private void GetConfig(T variable, string[] path);
    private void ProcessRecoil(BaseProjectile projectile, BasePlayer player, ItemModProjectile mod, ProjectileShoot projectileShoot, NoRecoilData info, int probModifier, UnityEngine.Vector3 eyesDirection);
}

public class NoRecoilData
{
    public int Ticks;
    public int Count;
    public int Violations;
}


```

---

## NoRespawnCooldowns by MrBlue - Disables respawn cooldown for players with permission

```csharp

Oxide.Plugins
[Info("No Respawn Cooldowns", "Absolut", "1.0.3")]
[Description("Disables respawn cooldown for players with permission")]
 class NoRespawnCooldowns : RustPlugin
{
    private const string permAllow;
    private void Init();
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
    private void ResetSpawnTargets(BasePlayer player);
}


```

---

## NoSafeZoneSleepers by NooBlet - Automatically creates a zone to remove sleeping players from Outpost and Bandit Camp

```csharp
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("No Safe Zone Sleepers", "NooBlet", "1.9")]
[Description("Automaticly Creates a Zone to remove sleeping players from Outpost and Bandit Camp")]
public class NoSafeZoneSleepers : CovalencePlugin
{
     List<string> createdZones;
     int number;
    private bool UseCustomZone;
    private bool KillSleepers;
    private int TimeToKill;
    private int OupostZoneRadius;
    private int BanditCampZoneRadius;
    private int OtherZones;
    private bool UseMessages;
    private string EnterMessage;
    private string ExitMessage;
    private string MessageColor;
    private List<object> CustomZones;
    protected override void LoadDefaultConfig();
    private void LoadConfiguration();
    private void CheckCfg(string Key, T var);
    [PluginReference]
    private Plugin ZoneManager;
    private void OnServerInitialized();
     void Unload();
     void OnPlayerDisconnected(BasePlayer player, string reason);
     bool isInPriv(BasePlayer player);
     string ZoneName(string message, string zonename);
    private bool CheckCustomZone(BasePlayer player);
    private string GetPlayerZone(BasePlayer player);
    private void RemoveSleeper(BasePlayer sleeper);
    private void Addflag(string zone, string flag);
    private void ClearZones();
    private void AddZones();
}


```

---

## NoSash by MrBlue - Blocks sashes from showing on players

```csharp
using System.Linq;

Oxide.Plugins
[Info("No Sash", "Wulf", "1.0.2")]
[Description("Blocks sashes from showing on players")]
public class NoSash : CovalencePlugin
{
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerSpawn(BasePlayer player);
    private void OnItemAddedRemoved(Item item, bool added);
}


```

---

## NoServerCCTV by NooBlet - Renames server cameras identifiers to stop players from using them

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("No Server CCTV Camaras", "NooBlet", "0.1.6")]
[Description("Renames default Server CCTV camara identifiers")]
public class NoServerCCTV : CovalencePlugin
{
     Dictionary<string, String> identifiers;
     void OnServerInitialized();
     void Unload();
     CCTV_RC[] GetCCTVs();
     void SetServerCCTVs();
     void ResetServerCCTVnames();
}


```

---

## NoShelvesBugRaiding by wazzzup - No bug raiding with shelves

```csharp
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("NoShelvesBugRaiding", "wazzzup", "1.1.0")]
[Description("No bug raiding with shelfs")]
public class NoShelvesBugRaiding : RustPlugin
{
     bool OnShelves(BaseEntity entity);
     void OnEntityBuilt(Planner plan, GameObject obj);
}


```

---

## NoSleepers by collectvood - Prevents players from sleeping and optionally removes player corpses and bags

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using UnityEngine;
using Newtonsoft.Json;
using Oxide.Core;

Oxide.Plugins
[Info("NoSleepers", "collect_vood", "0.5.7")]
[Description("Prevents players from sleeping and optionally removes player corpses and bags")]
 class NoSleepers : CovalencePlugin
{
    private ConfigurationFile Configuration;
    private class ConfigurationFile
    {
        [JsonProperty(PropertyName = "Kill existing")]
        public bool KillExisting;
        [JsonProperty(PropertyName = "Remove corpses")]
        public bool RemoveCorpses;
        [JsonProperty(PropertyName = "Remove bags")]
        public bool RemoveBags;
        [JsonProperty(PropertyName = "Save last position")]
        public bool SaveLastPosition;
        [JsonProperty(PropertyName = "Save last inventory")]
        public bool SaveLastInventory;
        [JsonProperty(PropertyName = "Exclude Permission")]
        public string PermExclude;
        [JsonProperty(PropertyName = "Player Inactivity Data Removal (hours)")]
        public int InactivityRemovalTime;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private StoredData AllStoredData;
    private Dictionary<string, PlayerData> AllPlayerData { get; set; }
    private class StoredData
    {
        [JsonProperty("All Player Data", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, PlayerData> AllPlayerData { get; set; }
    }

    public class PlayerData
    {
        [JsonProperty("Player Items")]
        public List<PlayerItem> PlayerItems;
        [JsonProperty("Last position")]
        public Vector3 LastPosition;
        [JsonProperty("Last active")]
        public long LastActive;
        public PlayerData();
    }

    public class PlayerItem
    {
        [JsonProperty("Name")]
        public string Shortname;
        [JsonProperty("Amount")]
        public int Amount;
        [JsonProperty("SkinId")]
        public ulong SkinId;
        [JsonProperty("Position")]
        public int Position;
        [JsonProperty("Condition")]
        public float Condition;
        [JsonProperty("Magazine")]
        public int Magazine;
        [JsonProperty("Container")]
        public ContainerType Container;
        [JsonProperty("Mods")]
        public List<int> Mods;
        public PlayerItem(string shortName, int amount, ulong skinId, int position, float condition, int magazine, ContainerType container, List<int> mods);
    }

    private void SaveData();
    private void LoadData();
    private void OnServerSave();
    private void Unload();
    private void ClearUpData();
     void OnServerInitialized();
     void OnPlayerRespawned(BasePlayer player);
     object OnPlayerRespawn(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     object OnServerCommand(ConsoleSystem.Arg arg);
     void OnEntitySpawned(BaseNetworkable entity);
     List<PlayerItem> GetAllItems(PlayerInventory inventory);
     void GivePlayerItems(BasePlayer player, List<PlayerItem> playerItems);
}

private class ConfigurationFile
{
    [JsonProperty(PropertyName = "Kill existing")]
    public bool KillExisting;
    [JsonProperty(PropertyName = "Remove corpses")]
    public bool RemoveCorpses;
    [JsonProperty(PropertyName = "Remove bags")]
    public bool RemoveBags;
    [JsonProperty(PropertyName = "Save last position")]
    public bool SaveLastPosition;
    [JsonProperty(PropertyName = "Save last inventory")]
    public bool SaveLastInventory;
    [JsonProperty(PropertyName = "Exclude Permission")]
    public string PermExclude;
    [JsonProperty(PropertyName = "Player Inactivity Data Removal (hours)")]
    public int InactivityRemovalTime;
}

private class StoredData
{
    [JsonProperty("All Player Data", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, PlayerData> AllPlayerData { get; set; }
}

public class PlayerData
{
    [JsonProperty("Player Items")]
    public List<PlayerItem> PlayerItems;
    [JsonProperty("Last position")]
    public Vector3 LastPosition;
    [JsonProperty("Last active")]
    public long LastActive;
    public PlayerData();
}

public class PlayerItem
{
    [JsonProperty("Name")]
    public string Shortname;
    [JsonProperty("Amount")]
    public int Amount;
    [JsonProperty("SkinId")]
    public ulong SkinId;
    [JsonProperty("Position")]
    public int Position;
    [JsonProperty("Condition")]
    public float Condition;
    [JsonProperty("Magazine")]
    public int Magazine;
    [JsonProperty("Container")]
    public ContainerType Container;
    [JsonProperty("Mods")]
    public List<int> Mods;
    public PlayerItem(string shortName, int amount, ulong skinId, int position, float condition, int magazine, ContainerType container, List<int> mods);
}


```

---

## NoSuicide by MrBlue - Stops players from suiciding/killing themselves

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("No Suicide", "Wulf/lukespragg", "0.1.5")]
[Description("Stops players from suiciding/killing themselves")]
public class NoSuicide : CovalencePlugin
{
    private const string permExclude;
    private void Init();
    private bool CanSuicide(string id);
}


```

---

## NoSunGlare by Tryhard - Disables sun glare or removes the sun

```csharp
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("No Sun Glare", "Tryhard", "2.0.0")]
[Description("Removes sun or sun glare")]
public class NoSunGlare : RustPlugin
{
    private WeatherConfig _config;
    public class WeatherConfig
    {
        [JsonProperty(PropertyName = "Clouds")]
        public float Clouds { get; set; }
        [JsonProperty(PropertyName = "Cloud Opacity")]
        public float CloudOpacity { get; set; }
        [JsonProperty(PropertyName = "Cloud Brightness")]
        public float CloudBrightness { get; set; }
        [JsonProperty(PropertyName = "Cloud Coloring")]
        public int CloudColoring { get; set; }
        [JsonProperty(PropertyName = "Cloud Saturation")]
        public int CloudSaturation { get; set; }
        [JsonProperty(PropertyName = "Cloud Scattering")]
        public int CloudScattering { get; set; }
        [JsonProperty(PropertyName = "Cloud Sharpness")]
        public int CloudSharpness { get; set; }
        [JsonProperty(PropertyName = "Cloud Size")]
        public int CloudSize { get; set; }
        [JsonProperty(PropertyName = "Cloud Coverage")]
        public int CloudCoverage { get; set; }
        [JsonProperty(PropertyName = "Cloud Attenuation")]
        public int CloudAttenuation { get; set; }
        [JsonProperty(PropertyName = "Wind")]
        public float Wind { get; set; }
        [JsonProperty(PropertyName = "Rain")]
        public float Rain { get; set; }
        [JsonProperty(PropertyName = "Fog")]
        public float Fog { get; set; }
        [JsonProperty(PropertyName = "Fogginess")]
        public float Fogginess { get; set; }
        [JsonProperty(PropertyName = "Dust Chance")]
        public float DustChance { get; set; }
        [JsonProperty(PropertyName = "Fog Chance")]
        public float FogChance { get; set; }
        [JsonProperty(PropertyName = "Overcast Chance")]
        public float OvercastChance { get; set; }
        [JsonProperty(PropertyName = "Storm Chance")]
        public float StormChance { get; set; }
        [JsonProperty(PropertyName = "Clear Chance")]
        public float ClearChance { get; set; }
        [JsonProperty(PropertyName = "Atmosphere Contrast")]
        public float AtmosphereContrast { get; set; }
        [JsonProperty(PropertyName = "Atmosphere Directionality")]
        public float AtmosphereDirectionality { get; set; }
        [JsonProperty(PropertyName = "Atmosphere Mie")]
        public float AtmosphereMie { get; set; }
        [JsonProperty(PropertyName = "Atmosphere Rayleigh")]
        public float AtmosphereRayleigh { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private void SetWeatherParameters(float clouds, float cloudOpacity, float cloudBrightness, int cloudColoring, int cloudSaturation, int cloudScattering, int cloudSharpness, int cloudSize, int cloudCoverage, int cloudAttenuation, float wind, float rain, float fog, float fogginess, float dustChance, float fogChance, float overcastChance, float stormChance, float clearChance, float atmosphereContrast, float atmosphereDirectionality, float atmosphereMie, float atmosphereRayleigh);
}

public class WeatherConfig
{
    [JsonProperty(PropertyName = "Clouds")]
    public float Clouds { get; set; }
    [JsonProperty(PropertyName = "Cloud Opacity")]
    public float CloudOpacity { get; set; }
    [JsonProperty(PropertyName = "Cloud Brightness")]
    public float CloudBrightness { get; set; }
    [JsonProperty(PropertyName = "Cloud Coloring")]
    public int CloudColoring { get; set; }
    [JsonProperty(PropertyName = "Cloud Saturation")]
    public int CloudSaturation { get; set; }
    [JsonProperty(PropertyName = "Cloud Scattering")]
    public int CloudScattering { get; set; }
    [JsonProperty(PropertyName = "Cloud Sharpness")]
    public int CloudSharpness { get; set; }
    [JsonProperty(PropertyName = "Cloud Size")]
    public int CloudSize { get; set; }
    [JsonProperty(PropertyName = "Cloud Coverage")]
    public int CloudCoverage { get; set; }
    [JsonProperty(PropertyName = "Cloud Attenuation")]
    public int CloudAttenuation { get; set; }
    [JsonProperty(PropertyName = "Wind")]
    public float Wind { get; set; }
    [JsonProperty(PropertyName = "Rain")]
    public float Rain { get; set; }
    [JsonProperty(PropertyName = "Fog")]
    public float Fog { get; set; }
    [JsonProperty(PropertyName = "Fogginess")]
    public float Fogginess { get; set; }
    [JsonProperty(PropertyName = "Dust Chance")]
    public float DustChance { get; set; }
    [JsonProperty(PropertyName = "Fog Chance")]
    public float FogChance { get; set; }
    [JsonProperty(PropertyName = "Overcast Chance")]
    public float OvercastChance { get; set; }
    [JsonProperty(PropertyName = "Storm Chance")]
    public float StormChance { get; set; }
    [JsonProperty(PropertyName = "Clear Chance")]
    public float ClearChance { get; set; }
    [JsonProperty(PropertyName = "Atmosphere Contrast")]
    public float AtmosphereContrast { get; set; }
    [JsonProperty(PropertyName = "Atmosphere Directionality")]
    public float AtmosphereDirectionality { get; set; }
    [JsonProperty(PropertyName = "Atmosphere Mie")]
    public float AtmosphereMie { get; set; }
    [JsonProperty(PropertyName = "Atmosphere Rayleigh")]
    public float AtmosphereRayleigh { get; set; }
}


```

---

## NoSupplySignal by Whispers88 - Prevents supply drops triggering from supply signals

```csharp

Oxide.Plugins
[Info("NoSupplySignal", "Wulf/lukespragg, Whispers88", 0.2, ResourceId = 2375)]
[Description("Prevents supply drops triggering from supply signals")]
 class NoSupplySignal : CovalencePlugin
{
     void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
     void OnExplosiveDropped(BasePlayer player, BaseEntity entity, ThrownWeapon item);
}


```

---

## NoTeams by OfficerJAKE - Players cannot create teams at all

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;

Oxide.Plugins
[Info("No Teams", "OfficerJAKE", "1.0.4")]
[Description("Players cannot create teams at all")]
public class NoTeams : RustPlugin
{
    private const string BypassPerm;
    private Configuration config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Settings")]
        public Settings settings;
        public class Settings
        {
            [JsonProperty(PropertyName = "Enable Chat Reply")]
            public bool EnableChatReply;
            [JsonProperty(PropertyName = "Enable Console Logs")]
            public bool EnableConsoleLogs;
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private void Init();
    private object OnTeamCreate(BasePlayer player);
    private void SendChatMessage(BasePlayer player, string key);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Settings")]
    public Settings settings;
    public class Settings
    {
        [JsonProperty(PropertyName = "Enable Chat Reply")]
        public bool EnableChatReply;
        [JsonProperty(PropertyName = "Enable Console Logs")]
        public bool EnableConsoleLogs;
    }

}

public class Settings
{
    [JsonProperty(PropertyName = "Enable Chat Reply")]
    public bool EnableChatReply;
    [JsonProperty(PropertyName = "Enable Console Logs")]
    public bool EnableConsoleLogs;
}


```

---

## NoTechTree by Sche1sseHund - Disables the use of the tech tree

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("No Tech Tree", "Sche1sseHund", 1.2)]
[Description("Allows server owner to disable the use of the tech tree")]
 class NoTechTree : RustPlugin
{
    private const string PERMISSION_ALLOWBLOCKBYPASS;
    private void Init();
    private object CanUnlockTechTreeNode(BasePlayer player, TechTreeData.NodeInstance node, TechTreeData techTree);
    protected override void LoadDefaultMessages();
}


```

---

## NoteLogger by unboxingman - Tracks the who, when, what of note edit is, and logs to file

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Facepunch.Extend;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Note Logger", "un-boxing-man", "2.0.2")]
[Description("Logs notes to a file.")]
 class NoteLogger : RustPlugin
{
    const string permAdmin;
     class NoteInfo
    {
        public string Text;
        public string DisplayName;
        public ulong UserID;
        public ItemId NoteID;
        public DateTime TimeStamp;
        public NoteInfo();
        public NoteInfo(BasePlayer player, string Text, ItemId NoteID);
    }

     List<NoteInfo> NoteList;
     void Init();
     void OnServerSave();
     ConfigFile config;
     class ConfigFile
    {
        [JsonProperty(PropertyName = "Log Notes To Console")]
        public bool LogToConsole;
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [ChatCommand("note")]
     void NoteInfoCommand(BasePlayer player, string command, string[] args);
    [ConsoleCommand("note.update")]
     void NoteUpdateCommand(ConsoleSystem.Arg arg);
     void LogToConsole(BasePlayer player, string text, uint uid);
     void logTofile(BasePlayer player, string text, uint uid);
     void Teleport(BasePlayer player, Vector3 position);
     void StartSleeping(BasePlayer player);
     string GetMessage(string key, BasePlayer player);
     bool HasPerm(BasePlayer player);
     void SaveData();
     void ReadData();
}

 class NoteInfo
{
    public string Text;
    public string DisplayName;
    public ulong UserID;
    public ItemId NoteID;
    public DateTime TimeStamp;
    public NoteInfo();
    public NoteInfo(BasePlayer player, string Text, ItemId NoteID);
}

 class ConfigFile
{
    [JsonProperty(PropertyName = "Log Notes To Console")]
    public bool LogToConsole;
    public static ConfigFile DefaultConfig();
}


```

---

## NotesMonitor by MrBlue - Send a message to discord with the content of a note set by the player

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using UnityEngine;
using UnityEngine.Networking;

Oxide.Plugins
[Info( "Notes Monitor", "Mr. Blue", "1.0.3" )]
[Description( "Send a message to discord with the content of a note set by the user" )]
public class NotesMonitor : RustPlugin
{
    private const string WEBHOOK_INTRO;
    private static int? _embedColor;
    private class PluginConfiguration
    {
        [JsonProperty( "Discord Webhook" )]
        public string DiscordWebhook;
        [JsonProperty( "Embed Color" )]
        public string EmbedColor;
    }

    private PluginConfiguration _config;
    private void Init();
    protected override void LoadDefaultConfig();
    private PluginConfiguration GetDefaultConfig();
    protected override void LoadDefaultMessages();
    private string FormatMessage(string key, BasePlayer player, string prevText, string newText);
    private const string NOTE_UPDATE_COMMAND;
    private void OnServerCommand(ConsoleSystem.Arg arg);
    private void SendDiscordEmbed(BasePlayer player, string prevText, string newText);
    private IEnumerator HandleUpload(string url, WWWForm data);
    private static int? FromHex(string value);
}

private class PluginConfiguration
{
    [JsonProperty( "Discord Webhook" )]
    public string DiscordWebhook;
    [JsonProperty( "Embed Color" )]
    public string EmbedColor;
}


```

---

## NoteWriteControl by Gachl - Controls how players can write notes (create new, append to, or edit all)

```csharp
using System;
using UnityEngine;

Oxide.Plugins
[Info("Note Write Control", "Gachl", "1.0.1")]
[Description("Control how players can write notes (create new, append to, or edit all)")]
 class NoteWriteControl : RustPlugin
{
    private static readonly string PERMISSION_WRITE;
    private static readonly string PERMISSION_APPEND;
    private static readonly string PERMISSION_EDIT;
    private void Init();
    [ConsoleCommand("note.update")]
     void NoteUpdateCommand(ConsoleSystem.Arg arg);
}


```

---

## Notice by LaserHydra - Notice players anonymously

```csharp
using System.Collections.Generic;
using System.Reflection;
using System;
using System.Data;
using UnityEngine;
using Oxide.Core;

Oxide.Plugins
[Info("Notice", "LaserHydra", "1.0.1", ResourceId = 1193)]
[Description("Notice players anonymously")]
 class Notice : RustPlugin
{
     void Loaded();
    protected override void LoadDefaultConfig();
    [ChatCommand("notice")]
     void cmdNotice(BasePlayer player, string cmd, string[] args);
     BasePlayer GetPlayer(string searchedPlayer, BasePlayer executer, string prefix);
     string ArrayToString(string[] array, int first);
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}


```

---

## NotifyToDiscord by  - Sends server/player notifications to a Discord Channel

```csharp
using Oxide.Core;
using Oxide.Core.Plugins;
using Rust;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using UnityEngine;
using Oxide.Core.Libraries.Covalence;
using Newtonsoft.Json;
using System.Net;

Oxide.Plugins
[Info("Notify To Discord", "BuzZ", "0.0.12")]
[Description("Choose from a large range of server/player notifications to message to a Discord channel")]
public class NotifyToDiscord : RustPlugin
{
    [PluginReference]
     Plugin BetterChat;
     Plugin HooksExtended;
     Plugin DangerousTreasures;
     Plugin HumanNPC;
     Plugin DiscordMessages;
     bool debug;
     string Prefix;
     string PrefixColor;
     string ChatColor;
     ulong SteamIDIcon;
     bool countonline;
     string WebHookURL;
     bool WebHook;
    public List<string> queue;
    private static string BaseURLTemplate;
     bool discording;
     bool addvendingoffer;
     bool airdrop;
     bool apchunt;
     bool onattack;
     bool chatmessage;
     bool codelockchange;
     bool candemolish;
     bool canunlock;
     bool canmail;
     bool canmount;
     bool candismount;
     bool CH47attacked;
     bool CH47killed;
     bool codeenter;
     bool cratedrop;
     bool cratehack;
     bool cratehackend;
     bool dispenserbonus;
     bool doorknocked;
     bool dismounted;
     bool groupgrant;
     bool grouprevoke;
     bool lootitem;
     bool npcposition;
     bool onvoice;
     bool onoven;
     bool playerkick;
     bool playerban;
     bool playerunban;
     bool playerdie;
     bool playerdisconnect;
     bool playerrespawn;
     bool playerrespawned;
     bool playersleep;
     bool playersleepend;
     bool playerspawn;
     bool playerspectate;
     bool playerspectateend;
     bool playerviolation;
     bool pluginloaded;
     bool pluginunloaded;
     bool playerconnect;
     bool lootplayer;
     bool lootentity;
     bool mounted;
     bool rconnection;
     bool rconcommand;
     bool serversave;
     bool shopcomplete;
     bool signupdate;
     bool servermessage;
     bool usernameupdate;
     bool usergrant;
     bool userrevoke;
     bool wounded;
    public string BotToken;
    public ulong ChannelID;
    private bool ConfigChanged;
    private void Init();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private void OnServerInitialized();
    protected override void LoadDefaultMessages();
     void UnsubscribeMeSir();
     void SendWithWebHook(string MessageText);
     void SendMessage(string MessageText);
     void PostCallBack(int code, string response);
     void NotifyDiscord(string todiscord);
     class DiscordPayload
    {
        [JsonProperty("content")]
        public string MessageText { get; set; }
    }

     void OnBetterChat(Dictionary<string, object> dict);
     void OnPlayerChat(BasePlayer player, string message);
     string playername;
     string GetPlayerName(BasePlayer player);
     void OnGroupPermissionGranted(string name, string perm);
     void OnGroupPermissionRevoked(string name, string perm);
     void OnLootPlayer(BasePlayer player, BasePlayer target);
     void OnLootEntity(BasePlayer player, BaseEntity entity);
     void OnPlayerBanned(string name, ulong id, string address, string reason);
     void OnPlayerUnbanned(string name, ulong id, string address);
     void OnPlayerDeath(BasePlayer player, HitInfo info);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnPlayerRespawn(BasePlayer player);
     void OnPlayerRespawned(BasePlayer player);
     void OnPlayerSleep(BasePlayer player);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnPlayerSpawn(BasePlayer player);
     void OnPlayerSpectate(BasePlayer player, string spectateFilter);
     void OnPlayerSpectateEnd(BasePlayer player, string spectateFilter);
     void OnPlayerViolation(BasePlayer player, AntiHackType type, float amount);
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerKicked(BasePlayer player, string reason);
     void OnPluginLoaded(Plugin name);
     void OnPluginUnloaded(Plugin name);
     void OnServerSave();
     void OnServerMessage(string message, string name, string color, ulong id);
     void OnUserNameUpdated(string id, string oldName, string newName);
     void CanChangeCode(CodeLock codeLock, BasePlayer player, string newCode, bool isGuestCode);
     void CanUnlock(BasePlayer player, BaseLock baseLock);
     void OnCodeEntered(CodeLock codeLock, BasePlayer player, string code);
     void CanBeWounded(BasePlayer player, HitInfo info);
     void OnUserPermissionGranted(string id, string perm);
     void OnUserPermissionRevoked(string id, string perm);
     void CanMountEntity(BasePlayer player, BaseMountable entity);
     void OnEntityMounted(BaseMountable entity, BasePlayer player);
     void CanDismountEntity(BasePlayer player, BaseMountable entity);
     void OnEntityDismounted(BaseMountable entity, BasePlayer player);
     void OnLootItem(BasePlayer player, Item item);
     void OnRconCommand(IPAddress ip, string command, string[] args);
     void OnRconConnection(IPAddress ip);
     void OnAddVendingOffer(VendingMachine machine, ProtoBuf.VendingMachine.SellOrder sellOrder);
     void OnShopCompleteTrade(ShopFront shop, BasePlayer customer);
     void OnAirdrop(CargoPlane plane, Vector3 dropPosition);
     void OnCrateDropped(HackableLockedCrate crate);
     void OnCrateHack(HackableLockedCrate crate);
     void OnCrateHackEnd(HackableLockedCrate crate);
     void OnHelicopterAttacked(CH47HelicopterAIController heli);
     void OnHelicopterKilled(CH47HelicopterAIController heli);
     void OnBradleyApcHunt(BradleyAPC apc);
     void OnDoorKnocked(Door door, BasePlayer player);
     void CanUseMailbox(BasePlayer player, Mailbox mailbox);
     void OnOvenToggle(BaseOven oven, BasePlayer player);
     void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     void OnSignUpdated(Signage sign, BasePlayer player, string text);
     void OnPlayerAttack(BasePlayer attacker, HitInfo info);
     void OnPlayerVoice(BasePlayer player, Byte[] data);
     void CanDemolish(BasePlayer player, BuildingBlock block, BuildingGrade.Enum grade);
     bool extreceivedsnap;
     bool extnpcattack;
     bool extusesleepbag;
     bool exthelispawned;
     bool extusecar;
     bool extchinookspawned;
     void OnReceivedSnapshot(BasePlayer player);
     void OnNPCAttack(BaseCombatEntity entity, NPCPlayer npc, HitInfo info);
     void OnUseSleepingBag(BasePlayer source, SleepingBag bag);
     void OnHelicopterSpawned(BaseHelicopter helicopter);
     void OnChinookSpawned(CH47Helicopter helicopter);
     void OnUseCar(BasePlayer source, BasicCar target);
     bool dangerousmessage;
     void OnDangerousMessage(BasePlayer player, Vector3 eventPos, string message);
}

 class DiscordPayload
{
    [JsonProperty("content")]
    public string MessageText { get; set; }
}


```

---

## NoVehicleFuel by birthdates - No fuel needed for boats and RHIBs

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("No Vehicle Fuel", "birthdates", "2.0.3")]
[Description("No fuel needed for boats and rhibs.")]
public class NoVehicleFuel : RustPlugin
{
    private const string permission_boat;
    private const string permission_copter;
    public static NoVehicleFuel Ins;
    private readonly List<FuelVehicle> cachedVehicles;
    private readonly Dictionary<string, string> PrefabToPermission;
    private class FuelVehicle : MonoBehaviour
    {
        private BaseEntity Vehicle;
        private StorageContainer FuelTank;
        private void Awake();
        public void AddFuel(int amount);
        public void End(bool remove);
    }

    private void Init();
    private void OnServerInitialized();
    private void SetupMounted();
    private void Unload();
    private void OnEntityMounted(BaseMountable entity, BasePlayer player);
    private void OnEntityDismounted(BaseMountable entity, BasePlayer player);
    private object CanLootEntity(BasePlayer player, StorageContainer container);
    private void OnItemRemovedFromContainer(ItemContainer container, Item item);
    public ConfigFile _config;
    public class ConfigFile
    {
        [JsonProperty("Disabled Vehicles (Prefab)")]
        public List<string> Disabled;
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class FuelVehicle : MonoBehaviour
{
    private BaseEntity Vehicle;
    private StorageContainer FuelTank;
    private void Awake();
    public void AddFuel(int amount);
    public void End(bool remove);
}

public class ConfigFile
{
    [JsonProperty("Disabled Vehicles (Prefab)")]
    public List<string> Disabled;
    public static ConfigFile DefaultConfig();
}


```

---

## NoVood by SapnuPuas - alow players with Permission to use furnaces and oil refineries without the need of wood

```csharp
using System;
using Oxide.Core;
using Newtonsoft.Json;
using System.Collections.Generic;

Oxide.Plugins
[Info("NoVood", "Sapnu Puas#3696", "1.0.4")]
[Description("use furnaces with no wood and quick smelt options, by permission")]
public class NoVood : RustPlugin
{
    private void Init();
    private void OnServerInitialized();
    private object OnFindBurnable(BaseOven oven);
     void OnOvenToggle(BaseOven oven, BasePlayer player);
    private Configuration config;
    protected override void LoadDefaultConfig();
    public class Configuration
    {
        [JsonProperty("Permissions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, Ovens> Permissions;
        [JsonProperty("Allowed Resources (Item Shortnames) items must be in this list to use without wood")]
        public HashSet<string> AllowedRes;
    }

    public class Ovens
    {
        [JsonProperty("base oven short prefab name and settings")]
        public Dictionary<string, FurnaceSetup> furnaceSettings { get; set; }
    }

    public class FurnaceSetup
    {
        [JsonProperty("Uses no fuel ?")]
        public bool UseNoWood { get; set; }
        [JsonProperty("Require raw resources to start when using no fuel ?")]
        public bool NeedRes { get; set; }
        [JsonProperty("Quicksmelt speed (1 is default speed)")]
        public int Speed { get; set; }
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
}

public class Configuration
{
    [JsonProperty("Permissions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, Ovens> Permissions;
    [JsonProperty("Allowed Resources (Item Shortnames) items must be in this list to use without wood")]
    public HashSet<string> AllowedRes;
}

public class Ovens
{
    [JsonProperty("base oven short prefab name and settings")]
    public Dictionary<string, FurnaceSetup> furnaceSettings { get; set; }
}

public class FurnaceSetup
{
    [JsonProperty("Uses no fuel ?")]
    public bool UseNoWood { get; set; }
    [JsonProperty("Require raw resources to start when using no fuel ?")]
    public bool NeedRes { get; set; }
    [JsonProperty("Quicksmelt speed (1 is default speed)")]
    public int Speed { get; set; }
}


```

---

## NoWeaponDrop by  - Prevents dropping of active weapon when players start to die

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("No Weapon Drop", "Fujikura", "1.2.0")]
[Description("Prevents dropping of active weapon when players start to die")]
 class NoWeaponDrop : CovalencePlugin
{
    [PluginReference]
     Plugin RestoreUponDeath;
    private const string permissionName;
    private bool Changed;
    private bool usePermission;
    private object GetConfig(string menu, string datavalue, object defaultValue);
     void LoadVariables();
    protected override void LoadDefaultConfig();
     void Init();
     object CanDropActiveItem(BasePlayer player);
}


```

---

## NoWorkbench by k1lly0u - Eliminates the requirement of being near a bench to craft

```csharp
using Newtonsoft.Json;
using System.Linq;
using System.Collections.Generic;

Oxide.Plugins
[Info("NoWorkbench", "k1lly0u", "0.1.51")]
[Description("Eliminates the requirement of being near a bench to craft")]
 class NoWorkbench : RustPlugin
{
    private Dictionary<int, int> defaultBlueprints;
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void UnlockAllBlueprints(BasePlayer player);
    private void Unload();
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Disable the need for blueprints")]
        public bool NoBlueprints { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void SaveConfig(ConfigData config);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Disable the need for blueprints")]
    public bool NoBlueprints { get; set; }
}


```

---

## NoWoundedSuicide by  - Blocks suicide when player is wounded

```csharp
using UnityEngine;

Oxide.Plugins
[Info("No Wounded Suicide", "Orange", "1.0.1")]
[Description("Blocks suicide when player is wounded")]
public class NoWoundedSuicide : RustPlugin
{
    private object OnServerCommand(ConsoleSystem.Arg arg);
}


```

---

## NPCDropGun by 2CHEVSKII - Forces NPCs to drop their weapon and other loot after death

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Facepunch;
using Newtonsoft.Json;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("NPC Drop Gun", "2CHEVSKII", "2.0.8")]
[Description("Forces NPC to drop used gun and other items after death")]
public class NPCDropGun : RustPlugin
{
     Settings settings;
     Dictionary<BasePlayer, List<Item>> delayedItems;
     void Init();
     void OnServerInitialized();
     void OnEntityDeath(BasePlayer player);
     void OnCorpsePopulate(BasePlayer player, PlayerCorpse corpse);
     void DoSpawns(BasePlayer player);
     Item SpawnMeds();
     Item SpawnAmmo(BaseProjectile weapon);
     Item SetAttachments(Item item);
     BaseEntity DropNearPosition(Item item, Vector3 pos);
     BaseEntity ApplyVelocity(BaseEntity entity);
     ulong GetRandomSkin(ItemDefinition idef);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     class Settings
    {
        public GunSettings Guns { get; set; }
        public OtherSettings Ammo { get; set; }
        public OtherSettings Meds { get; set; }
        [JsonProperty("Put weapon into corpse")]
        public bool GunIntoCorpse { get; set; }
        [JsonProperty("Remove default loot from corpse")]
        public bool RemoveDefault { get; set; }
        [JsonProperty("Drop spawned items near corpse (otherwise just delete them)")]
        public bool DropNearFull { get; set; }
        public static Settings Default { get; set; }
        public class GunSettings : DropChanceSettings
        {
            [JsonProperty("Gun condition")]
            public RangeSettings Condition { get; set; }
            [JsonProperty("Attachment count")]
            public RangeSettings AttachmentCount { get; set; }
            [JsonProperty("Attachment list")]
            public string[] Attachments { get; set; }
            [JsonProperty("Assign random skin")]
            public bool RandomSkin { get; set; }
        }

        public class DropChanceSettings
        {
            [JsonProperty("Drop chance")]
            public float DropChance { get; set; }
        }

        public class OtherSettings : DropChanceSettings
        {
            [JsonProperty("Amount to drop")]
            public RangeSettings Amount { get; set; }
        }

        public class RangeSettings
        {
            public uint Min { get; set; }
            public uint Max { get; set; }
        }

    }

}

 class Settings
{
    public GunSettings Guns { get; set; }
    public OtherSettings Ammo { get; set; }
    public OtherSettings Meds { get; set; }
    [JsonProperty("Put weapon into corpse")]
    public bool GunIntoCorpse { get; set; }
    [JsonProperty("Remove default loot from corpse")]
    public bool RemoveDefault { get; set; }
    [JsonProperty("Drop spawned items near corpse (otherwise just delete them)")]
    public bool DropNearFull { get; set; }
    public static Settings Default { get; set; }
    public class GunSettings : DropChanceSettings
    {
        [JsonProperty("Gun condition")]
        public RangeSettings Condition { get; set; }
        [JsonProperty("Attachment count")]
        public RangeSettings AttachmentCount { get; set; }
        [JsonProperty("Attachment list")]
        public string[] Attachments { get; set; }
        [JsonProperty("Assign random skin")]
        public bool RandomSkin { get; set; }
    }

    public class DropChanceSettings
    {
        [JsonProperty("Drop chance")]
        public float DropChance { get; set; }
    }

    public class OtherSettings : DropChanceSettings
    {
        [JsonProperty("Amount to drop")]
        public RangeSettings Amount { get; set; }
    }

    public class RangeSettings
    {
        public uint Min { get; set; }
        public uint Max { get; set; }
    }

}

public class GunSettings : DropChanceSettings
{
    [JsonProperty("Gun condition")]
    public RangeSettings Condition { get; set; }
    [JsonProperty("Attachment count")]
    public RangeSettings AttachmentCount { get; set; }
    [JsonProperty("Attachment list")]
    public string[] Attachments { get; set; }
    [JsonProperty("Assign random skin")]
    public bool RandomSkin { get; set; }
}

public class DropChanceSettings
{
    [JsonProperty("Drop chance")]
    public float DropChance { get; set; }
}

public class OtherSettings : DropChanceSettings
{
    [JsonProperty("Amount to drop")]
    public RangeSettings Amount { get; set; }
}

public class RangeSettings
{
    public uint Min { get; set; }
    public uint Max { get; set; }
}


```

---

## NPCHealth by Rustoholics - Boost or decrease the health of all NPC players by scaling damage dealt to them

```csharp
using System;
using Newtonsoft.Json;

Oxide.Plugins
[Info("NPC Health", "Rustoholics", "0.1.1")]
[Description("Boost or decrease the health of all NPC players by scaling damage dealt to them")]
public class NPCHealth : CovalencePlugin
{
    private Configuration _config;
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private class Configuration
    {
        [JsonProperty("How strong are NPCs (1.0 = normal, 0.5 = half as strong, 2 = twice as strong, 10 = epic")]
        public double NPCHealthMultiplier;
    }

    protected override void LoadConfig();
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
}

private class Configuration
{
    [JsonProperty("How strong are NPCs (1.0 = normal, 0.5 = half as strong, 2 = twice as strong, 10 = epic")]
    public double NPCHealthMultiplier;
}


```

---

## NPCLoadouts by VisEntities - Equip npcs with custom loadouts

```csharp
using Facepunch;
using Newtonsoft.Json;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("NPC Loadouts", "VisEntities", "2.0.1")]
[Description("Equip npcs with custom loadouts.")]
public class NPCLoadouts : RustPlugin
{
    private static NPCLoadouts _plugin;
    private static Configuration _config;
    private Coroutine _coroutine;
    private class Configuration
    {
        [JsonProperty("Version")]
        public string Version { get; set; }
        [JsonProperty("NPC Groups")]
        public List<NPCGroupConfig> NPCGroups { get; set; }
        public void BuildLoadouts();
    }

    private class NPCGroupConfig
    {
        [JsonProperty("Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty("NPC Short Prefab Names")]
        public string[] NPCShortPrefabNames { get; set; }
        [JsonProperty("Loadout")]
        public LoadoutConfig Loadout { get; set; }
    }

    private class LoadoutConfig
    {
        [JsonProperty("Randomize Active Weapon")]
        public bool RandomizeActiveWeapon { get; set; }
        [JsonProperty("Belt")]
        public List<ItemInfo> Belt { get; set; }
        [JsonProperty("Wear")]
        public List<ItemInfo> Wear { get; set; }
        [JsonIgnore]
        public PlayerInventoryProperties InventoryProperties { get; set; }
        public void BuildLoadout();
    }

    public class ItemInfo
    {
        [JsonProperty("Shortname")]
        public string Shortname { get; set; }
        [JsonProperty("Skin Id")]
        public ulong SkinId { get; set; }
        [JsonProperty("Amount")]
        public int Amount { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfig();
    private Configuration GetDefaultConfig();
    private void StartCoroutine();
    private void StopCoroutine();
    private void Init();
    private void Unload();
    private void OnServerInitialized();
    private void OnEntitySpawned(NPCPlayer npc);
    private IEnumerator EquipLoadoutForAll();
    private void EquipLoadout(NPCPlayer npc, LoadoutConfig loadout);
    public static void StripInventory(BasePlayer player);
}

private class Configuration
{
    [JsonProperty("Version")]
    public string Version { get; set; }
    [JsonProperty("NPC Groups")]
    public List<NPCGroupConfig> NPCGroups { get; set; }
    public void BuildLoadouts();
}

private class NPCGroupConfig
{
    [JsonProperty("Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty("NPC Short Prefab Names")]
    public string[] NPCShortPrefabNames { get; set; }
    [JsonProperty("Loadout")]
    public LoadoutConfig Loadout { get; set; }
}

private class LoadoutConfig
{
    [JsonProperty("Randomize Active Weapon")]
    public bool RandomizeActiveWeapon { get; set; }
    [JsonProperty("Belt")]
    public List<ItemInfo> Belt { get; set; }
    [JsonProperty("Wear")]
    public List<ItemInfo> Wear { get; set; }
    [JsonIgnore]
    public PlayerInventoryProperties InventoryProperties { get; set; }
    public void BuildLoadout();
}

public class ItemInfo
{
    [JsonProperty("Shortname")]
    public string Shortname { get; set; }
    [JsonProperty("Skin Id")]
    public ulong SkinId { get; set; }
    [JsonProperty("Amount")]
    public int Amount { get; set; }
}


```

---

## NPCMapMarkers by 1AK1 - Shows custom map markers for NPCs and animals on the server

```csharp
using System.Collections.Generic;
using UnityEngine;
using CompanionServer.Handlers;
using Newtonsoft.Json;

Oxide.Plugins
[Info("NPC Map Markers", "AK", "1.0.0")]
[Description("Shows custom map markers for NPCs and animals on the server")]
internal class NPCMapMarkers : CovalencePlugin
{
    private const string permAdmin;
    public List<MapMarkerGenericRadius> npcMarkers;
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "NPC Map Marker Options")]
        public NPCMapMarkerOptions NPCMapMarker { get; set; }
        [JsonProperty(PropertyName = "Human NPC Markers Options")]
        public NPCMarkersOptions NPCMarkers { get; set; }
        [JsonProperty(PropertyName = "Animal Markers Options")]
        public AnimalMarkersOptions AnimalMarkers { get; set; }
        public class NPCMapMarkerOptions
        {
            [JsonProperty(PropertyName = "Prefab Path")]
            public string PREFAB_MARKER { get; set; }
            [JsonProperty(PropertyName = "Update frequency")]
            public float updateFreq { get; set; }
            [JsonProperty(PropertyName = "Show to all? (true/false)")]
            public bool visibleToAll { get; set; }
            [JsonProperty(PropertyName = "Show Human NPC on map? (true/false)")]
            public bool showNPC { get; set; }
            [JsonProperty(PropertyName = "Show Animals on map? (true/false)")]
            public bool showAnimals { get; set; }
        }

        public class NPCMarkersOptions
        {
            [JsonProperty(PropertyName = "Color1 (hex)")]
            public string NPCColor1 { get; set; }
            [JsonProperty(PropertyName = "Color2 (hex)")]
            public string NPCColor2 { get; set; }
            [JsonProperty(PropertyName = "Alpha")]
            public float NPCAlpha { get; set; }
            [JsonProperty(PropertyName = "Radius")]
            public float NPCRadius { get; set; }
        }

        public class AnimalMarkersOptions
        {
            [JsonProperty(PropertyName = "Color1 (hex)")]
            public string AnimalColor1 { get; set; }
            [JsonProperty(PropertyName = "Color2 (hex)")]
            public string AnimalColor2 { get; set; }
            [JsonProperty(PropertyName = "Alpha")]
            public float AnimalAlpha { get; set; }
            [JsonProperty(PropertyName = "Radius")]
            public float AnimalRadius { get; set; }
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void Init();
    private void OnServerInitialized(bool initial);
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
     object CanNetworkTo(MapMarkerGenericRadius marker, BasePlayer player);
    private void UpdateMarkers();
    private void LoadNPCMapMarkers();
    private void RemoveNPCMapMarkers();
    private void CreateNPCMapMarker(BaseNetworkable entity, string type, string color1, string color2, float alpha, float radius);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "NPC Map Marker Options")]
    public NPCMapMarkerOptions NPCMapMarker { get; set; }
    [JsonProperty(PropertyName = "Human NPC Markers Options")]
    public NPCMarkersOptions NPCMarkers { get; set; }
    [JsonProperty(PropertyName = "Animal Markers Options")]
    public AnimalMarkersOptions AnimalMarkers { get; set; }
    public class NPCMapMarkerOptions
    {
        [JsonProperty(PropertyName = "Prefab Path")]
        public string PREFAB_MARKER { get; set; }
        [JsonProperty(PropertyName = "Update frequency")]
        public float updateFreq { get; set; }
        [JsonProperty(PropertyName = "Show to all? (true/false)")]
        public bool visibleToAll { get; set; }
        [JsonProperty(PropertyName = "Show Human NPC on map? (true/false)")]
        public bool showNPC { get; set; }
        [JsonProperty(PropertyName = "Show Animals on map? (true/false)")]
        public bool showAnimals { get; set; }
    }

    public class NPCMarkersOptions
    {
        [JsonProperty(PropertyName = "Color1 (hex)")]
        public string NPCColor1 { get; set; }
        [JsonProperty(PropertyName = "Color2 (hex)")]
        public string NPCColor2 { get; set; }
        [JsonProperty(PropertyName = "Alpha")]
        public float NPCAlpha { get; set; }
        [JsonProperty(PropertyName = "Radius")]
        public float NPCRadius { get; set; }
    }

    public class AnimalMarkersOptions
    {
        [JsonProperty(PropertyName = "Color1 (hex)")]
        public string AnimalColor1 { get; set; }
        [JsonProperty(PropertyName = "Color2 (hex)")]
        public string AnimalColor2 { get; set; }
        [JsonProperty(PropertyName = "Alpha")]
        public float AnimalAlpha { get; set; }
        [JsonProperty(PropertyName = "Radius")]
        public float AnimalRadius { get; set; }
    }

}

public class NPCMapMarkerOptions
{
    [JsonProperty(PropertyName = "Prefab Path")]
    public string PREFAB_MARKER { get; set; }
    [JsonProperty(PropertyName = "Update frequency")]
    public float updateFreq { get; set; }
    [JsonProperty(PropertyName = "Show to all? (true/false)")]
    public bool visibleToAll { get; set; }
    [JsonProperty(PropertyName = "Show Human NPC on map? (true/false)")]
    public bool showNPC { get; set; }
    [JsonProperty(PropertyName = "Show Animals on map? (true/false)")]
    public bool showAnimals { get; set; }
}

public class NPCMarkersOptions
{
    [JsonProperty(PropertyName = "Color1 (hex)")]
    public string NPCColor1 { get; set; }
    [JsonProperty(PropertyName = "Color2 (hex)")]
    public string NPCColor2 { get; set; }
    [JsonProperty(PropertyName = "Alpha")]
    public float NPCAlpha { get; set; }
    [JsonProperty(PropertyName = "Radius")]
    public float NPCRadius { get; set; }
}

public class AnimalMarkersOptions
{
    [JsonProperty(PropertyName = "Color1 (hex)")]
    public string AnimalColor1 { get; set; }
    [JsonProperty(PropertyName = "Color2 (hex)")]
    public string AnimalColor2 { get; set; }
    [JsonProperty(PropertyName = "Alpha")]
    public float AnimalAlpha { get; set; }
    [JsonProperty(PropertyName = "Radius")]
    public float AnimalRadius { get; set; }
}


```

---

## NpcTarget by misticos - Prevent NPCs from targeting each other

```csharp

Oxide.Plugins
[Info("NPC Target", "misticos", "1.0.4")]
[Description("Prevent NPCs from targeting each other")]
 class NpcTarget : RustPlugin
{
    private object OnNpcTarget(BaseEntity attacker, BaseEntity entity);
    private object CanBradleyApcTarget(BradleyAPC attacker, BaseEntity entity);
    private bool IsNpc(BaseEntity entity);
}


```

---

## Npctp by Razor - Creates NPCs that teleport players anywhere, run commands, or open doors

```csharp
using System.Collections.Generic;
using System;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using UnityEngine;
using Newtonsoft.Json.Linq;
using System.Linq;
using Oxide.Core.Libraries.Covalence;
using static UnityEngine.Vector3;
using Oxide.Game.Rust.Cui;
using System.Globalization;
using System.Reflection;
using Facepunch;

Oxide.Plugins
[Info("Npctp", "Razor", "2.5.1")]
[Description("Some NPC Controle Thanks Wulf and k1lly0u")]
 class Npctp : RustPlugin
{
    [PluginReference]
     Plugin Spawns;
     Plugin Economics;
     Plugin ServerRewards;
     Plugin Vanish;
     Plugin HumanNPC;
     Plugin Jail;
     PlayerCooldown pcdData;
     NPCTPDATA npcData;
    private DynamicConfigFile PCDDATA;
    private DynamicConfigFile NPCDATA;
    private FieldInfo serverinput;
    private bool backroundimage;
    private string backroundimageurl;
    private static int cooldownTime;
    private static int auth;
    private bool Changed;
    private float Cost;
    private float DoorLocX;
    private float DoorLocY;
    private float DoorLocZ;
    private ulong DoorId;
    private static bool useEconomics;
    private static bool useRewards;
    private static bool useJail;
    private static bool useSpawns;
    private static bool AutoCloseDoors;
    private static int AutoCloseTime;
    private static int SleepKillDamage;
    private static bool DoorOpenMessage;
    private static bool FollowDead;
    private static bool UseGunToKill;
    private static bool SetOnFire;
    private static int OnFireTime;
    private static int OnFireDamage;
    private string NpcName;
    private string IDNPC;
    private string msg;
    private string SpawnFiles;
     Dictionary<string, string> messages;
     void Loaded();
    private void OnServerInitialized();
     void Init();
     void Unload();
     object GetConfig(string menu, string datavalue, object defaultValue);
    private void RegisterPermissions();
    private void CheckDependencies();
     void LoadVariables();
    protected override void LoadDefaultConfig();
     void SaveNpcTpData();
     class NPCTPDATA
    {
        public Dictionary<string, NPCInfo> NpcTP;
        public NPCTPDATA();
    }

     class NPCInfo
    {
        public string NpcName;
        public string SpawnFile;
        public int Cooldown;
        public bool CanUse;
        public bool useUI;
        public float Cost;
        public string permission;
        public bool useItem;
        public bool UseCommand;
        public bool useMessage;
        public string MessageNpc;
        public bool EnableDead;
        public bool DeadOnPlayer;
        public string DeadCmd;
        public string DeadArgs;
        public bool OpenDoor;
        public float DoorLocX;
        public float DoorLocY;
        public float DoorLocZ;
        public ulong DoorId;
        public bool NoPermKill;
        public bool KillSleep;
        public Dictionary<string, NPCcommands> Commands;
        public Dictionary<string, NPCitems> Items;
    }

     class PlayerCooldown
    {
        public Dictionary<ulong, PCDInfo> pCooldown;
        public PlayerCooldown();
    }

     class PCDInfo
    {
        public Dictionary<string, long> npcCooldowns;
        public PCDInfo();
        public PCDInfo(long cd);
    }

     class NPCcommands
    {
        public string Command;
        public string Arrangements;
        public bool OnPlayer;
    }

     class NPCitems
    {
        public string ItemShortName;
        public int Amount;
    }

     void SaveData();
     void LoadData();
    static double GrabCurrentTime();
    [ChatCommand("npctp_door")]
     void SetDoor(BasePlayer player, string command, string[] args);
    private int GetAmount(BasePlayer player, string shortname);
    private bool TakeResources(BasePlayer player, string shortname, int iAmount);
    private int TakeResourcesFrom(BasePlayer player, List<Item> container, string shortname, int iAmount);
     object CastRay(Vector3 Pos, Vector3 Aim);
    private object ProcessRay(RaycastHit hitInfo);
     RaycastHit RayPosition(Vector3 sourcePos);
    private void TeleportPlayerPosition1(BasePlayer player, Vector3 destination);
    private void StartSleeping(BasePlayer player);
     object GetRandomVector(Vector3 position, float max, bool failed);
    [ChatCommand("npctp_add")]
     void cmdNpcAdD(BasePlayer player, string command, string[] args);
    [ChatCommand("npctp")]
     void cmdChatNPCEdit(BasePlayer player, string command, string[] args);
     void OnKillNPC(BasePlayer npc, HitInfo hinfo);
    [ConsoleCommand("hardestcommandtoeverguessnpctp")]
     void cmdRun(ConsoleSystem.Arg arg);
    readonly Dictionary<ulong, Timer> timers;
     void OnEnterNPC(BasePlayer npc, BasePlayer player);
     void OnLeaveNPC(BasePlayer npc, BasePlayer player);
     void StartNpcAttack(BasePlayer npc, BasePlayer player);
     void StartFire(BasePlayer player);
     void applyBlastDamage(BasePlayer player);
     void applySleepDamage(BasePlayer player);
     void OnUseNPC(BasePlayer npc, BasePlayer player, Vector3 destination);
}

 class NPCTPDATA
{
    public Dictionary<string, NPCInfo> NpcTP;
    public NPCTPDATA();
}

 class NPCInfo
{
    public string NpcName;
    public string SpawnFile;
    public int Cooldown;
    public bool CanUse;
    public bool useUI;
    public float Cost;
    public string permission;
    public bool useItem;
    public bool UseCommand;
    public bool useMessage;
    public string MessageNpc;
    public bool EnableDead;
    public bool DeadOnPlayer;
    public string DeadCmd;
    public string DeadArgs;
    public bool OpenDoor;
    public float DoorLocX;
    public float DoorLocY;
    public float DoorLocZ;
    public ulong DoorId;
    public bool NoPermKill;
    public bool KillSleep;
    public Dictionary<string, NPCcommands> Commands;
    public Dictionary<string, NPCitems> Items;
}

 class PlayerCooldown
{
    public Dictionary<ulong, PCDInfo> pCooldown;
    public PlayerCooldown();
}

 class PCDInfo
{
    public Dictionary<string, long> npcCooldowns;
    public PCDInfo();
    public PCDInfo(long cd);
}

 class NPCcommands
{
    public string Command;
    public string Arrangements;
    public bool OnPlayer;
}

 class NPCitems
{
    public string ItemShortName;
    public int Amount;
}


```

---

## NPCVenderModifier by  - Changes the multiplier of items purchased at NPC vending machines.

```csharp
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("NPC Vender Modifier", "Default", "1.0.1")]
[Description("Allows changing the multiplier of the NPC shops")]
public class NPCVenderModifier : RustPlugin
{
     int shopMulti;
     bool Changed;
    private const string permissionName;
     object OnGiveSoldItem(NPCVendingMachine vending, Item soldItem, BasePlayer buyer);
     void Init();
     object GetConfig(string menu, string datavalue, object defaultValue);
     void LoadVariables();
    protected override void LoadDefaultConfig();
}


```

---

## NPCVendingMapMarker by PinguinNordpol - Adds in-game vending map markers to NPCs

```csharp
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("NPC Vending Map Marker", "PinguinNordpol", "0.1.0")]
[Description("Adds in-game vending map markers to NPC's")]
 class NPCVendingMapMarker : CovalencePlugin
{
    [PluginReference]
    private Plugin HumanNPC;
    private NPCVendingMapMarkerConfig config_data;
    private Dictionary<ulong, VendingMachineMapMarker> map_markers;
    private List<TempVmm> TempVmms;
    private class MarkerData
    {
        public string shop_name;
        public UnityEngine.Vector3 position;
        public MarkerData(string _shop_name, UnityEngine.Vector3 _position);
    }

    private class NPCVendingMapMarkerConfig
    {
        public Dictionary<ulong, MarkerData> configured_markers;
        public string currency_sign;
        public bool debug;
    }

    protected override void LoadDefaultConfig();
    private NPCVendingMapMarkerConfig GetDefaultConfig();
    private class SR_NPCData
    {
        public Dictionary<string, SR_NPCInfo> npcInfo;
        public class SR_NPCInfo
        {
            public string name;
            public float x;
            public float z;
            public bool useCustom;
            public bool sellItems;
            public bool sellKits;
            public bool sellCommands;
            public bool canTransfer;
            public bool canSell;
            public bool canExchange;
            public List<string> items;
            public List<string> kits;
            public List<string> commands;
        }

    }

    private class SR_RewardData
    {
        public Dictionary<string, SR_RewardItem> items;
        public SortedDictionary<string, SR_RewardKit> kits;
        public SortedDictionary<string, SR_RewardCommand> commands;
        public class SR_RewardItem : SR_Reward
        {
            public string shortname;
            public string customIcon;
            public int amount;
            public ulong skinId;
            public bool isBp;
            public SR_Category category;
        }

        public class SR_RewardKit : SR_Reward
        {
            public string kitName;
            public string description;
            public string iconName;
        }

        public class SR_RewardCommand : SR_Reward
        {
            public string description;
            public string iconName;
            public List<string> commands;
        }

        public class SR_Reward
        {
            public string displayName;
            public int cost;
            public int cooldown;
        }

    }

    private SR_NPCData sr_npcdata;
    private SR_RewardData sr_rewarddata;
    private bool sr_enabled;
    private void ReadServerRewardsData();
    private string GetServerRewardsData(string _npc_id);
     void Loaded();
     void Unload();
     void Init();
     void OnNPCRespawn(BasePlayer npc);
     void OnKillNPC(BasePlayer npc, HitInfo hitinfo);
     void OnUseNPC(BasePlayer npc, BasePlayer player);
    protected override void LoadDefaultMessages();
    [Command("npcvmm")]
     void CmdInfo(IPlayer player, string command, string[] args);
    [Command("npcvmm.debug"), Permission("npcvendingmapmarker.admin")]
     void CmdDebug(IPlayer player, string command, string[] args);
    [Command("npcvmm.add"), Permission("npcvendingmapmarker.admin")]
     void CmdAdd(IPlayer player, string command, string[] args);
    [Command("npcvmm.reset"), Permission("npcvendingmapmarker.admin")]
     void CmdReset(IPlayer player, string command, string[] args);
    [Command("npcvmm.list"), Permission("npcvendingmapmarker.admin")]
     void CmdList(IPlayer player, string command, string[] args);
    [Command("npcvmm.del"), Permission("npcvendingmapmarker.admin")]
     void CmdDel(IPlayer player, string command, string[] args);
    [Command("npcvmm.clear"), Permission("npcvendingmapmarker.admin")]
     void CmdClear(IPlayer player, string command, string[] args);
    [Command("npcvmm.refresh"), Permission("npcvendingmapmarker.admin")]
     void CmdRefresh(IPlayer player, string command, string[] args);
    private bool AddMapMarker(ulong _npc_id, MarkerData _marker_data);
    private bool RemoveMapMarker(ulong _npc_id);
    private bool HasMapMarker(ulong _npc_id);
    private void ClearVendingMapMarkers();
    private void ReplyToPlayer(IPlayer player, string msg);
    private void Log(string msg);
    private void LogDebug(string msg);
}

private class MarkerData
{
    public string shop_name;
    public UnityEngine.Vector3 position;
    public MarkerData(string _shop_name, UnityEngine.Vector3 _position);
}

private class NPCVendingMapMarkerConfig
{
    public Dictionary<ulong, MarkerData> configured_markers;
    public string currency_sign;
    public bool debug;
}

private class SR_NPCData
{
    public Dictionary<string, SR_NPCInfo> npcInfo;
    public class SR_NPCInfo
    {
        public string name;
        public float x;
        public float z;
        public bool useCustom;
        public bool sellItems;
        public bool sellKits;
        public bool sellCommands;
        public bool canTransfer;
        public bool canSell;
        public bool canExchange;
        public List<string> items;
        public List<string> kits;
        public List<string> commands;
    }

}

public class SR_NPCInfo
{
    public string name;
    public float x;
    public float z;
    public bool useCustom;
    public bool sellItems;
    public bool sellKits;
    public bool sellCommands;
    public bool canTransfer;
    public bool canSell;
    public bool canExchange;
    public List<string> items;
    public List<string> kits;
    public List<string> commands;
}

private class SR_RewardData
{
    public Dictionary<string, SR_RewardItem> items;
    public SortedDictionary<string, SR_RewardKit> kits;
    public SortedDictionary<string, SR_RewardCommand> commands;
    public class SR_RewardItem : SR_Reward
    {
        public string shortname;
        public string customIcon;
        public int amount;
        public ulong skinId;
        public bool isBp;
        public SR_Category category;
    }

    public class SR_RewardKit : SR_Reward
    {
        public string kitName;
        public string description;
        public string iconName;
    }

    public class SR_RewardCommand : SR_Reward
    {
        public string description;
        public string iconName;
        public List<string> commands;
    }

    public class SR_Reward
    {
        public string displayName;
        public int cost;
        public int cooldown;
    }

}

public class SR_RewardItem : SR_Reward
{
    public string shortname;
    public string customIcon;
    public int amount;
    public ulong skinId;
    public bool isBp;
    public SR_Category category;
}

public class SR_RewardKit : SR_Reward
{
    public string kitName;
    public string description;
    public string iconName;
}

public class SR_RewardCommand : SR_Reward
{
    public string description;
    public string iconName;
    public List<string> commands;
}

public class SR_Reward
{
    public string displayName;
    public int cost;
    public int cooldown;
}


```

---

## NPCVendingReset by Whispers88 - Resets NPC vending machines via console or chat command

```csharp
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;

Oxide.Plugins
[Info("NPC Vending Reset", "Whispers88", "1.0.9")]
[Description("Reset all NPC vending machines")]
public class NPCVendingReset : CovalencePlugin
{
    private void vendingreset();
    [Command("vendingreset"), Permission("npcvendingreset.allowed")]
    private void Resetvendingmachines(IPlayer player, string command, string[] args);
    protected override void LoadDefaultMessages();
}


```

---

## NTeleportation by nivex - Multiple teleportation systems for admin and players

```csharp
using Facepunch;
using Network;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust;
using Oxide.Game.Rust.Cui;
using Oxide.Plugins.NTeleportationExtensionMethods;
using Rust;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Reflection;
using UnityEngine;

Oxide.Plugins
[Info("NTeleportation", "nivex", "1.8.9")]
[Description("Multiple teleportation systems for admin and players")]
 class NTeleportation : RustPlugin
{
    [PluginReference]
    private Plugin Clans;
    private Plugin Economics;
    private Plugin IQEconomic;
    private Plugin ServerRewards;
    private Plugin Friends;
    private Plugin CompoundTeleport;
    private Plugin ZoneManager;
    private Plugin NoEscape;
    private Plugin RaidBlock;
    private Plugin PopupNotifications;
    private Plugin BlockUsers;
    private Dictionary<string, BasePlayer> _codeToPlayer;
    private Dictionary<string, string> _playerToCode;
    private bool newSave;
    private const string NewLine;
    private const string TPA;
    private static readonly string[] nullArg;
    private const string PermAdmin;
    private const string PermRestrictions;
    private const string ConfigDefaultPermVip;
    private const string PermHome;
    private const string PermWipeHomes;
    private const string PermCraftHome;
    private const string PermCaveHome;
    private const string PermDeleteHome;
    private const string PermHomeHomes;
    private const string PermImportHomes;
    private const string PermRadiusHome;
    private const string PermCraftTpR;
    private const string PermCaveTpR;
    private const string PermTpR;
    private const string PermTpA;
    private const string PermTp;
    private const string PermDisallowTpToMe;
    private const string PermTpT;
    private const string PermTpB;
    private const string PermTpN;
    private const string PermTpL;
    private const string PermTpConsole;
    private const string PermTpRemove;
    private const string PermTpSave;
    private const string PermExempt;
    private const string PermFoundationCheck;
    private const string PermTpMarker;
    private DynamicConfigFile dataConvert;
    private DynamicConfigFile dataDisabled;
    private DynamicConfigFile dataAdmin;
    private DynamicConfigFile dataHome;
    private DynamicConfigFile dataTPR;
    private DynamicConfigFile dataTPT;
    private Dictionary<ulong, AdminData> _Admin;
    private Dictionary<ulong, HomeData> _Home;
    private Dictionary<ulong, TeleportData> _TPR;
    private Dictionary<string, List<string>> TPT;
    private bool changedAdmin;
    private bool changedHome;
    private bool changedTPR;
    private bool changedTPT;
    private float boundary;
    private readonly Dictionary<ulong, float> TeleportCooldowns;
    private readonly Dictionary<ulong, TeleportTimer> TeleportTimers;
    private readonly Dictionary<ulong, Timer> PendingRequests;
    private readonly Dictionary<ulong, BasePlayer> PlayersRequests;
    private readonly Dictionary<int, string> ReverseBlockedItems;
    private readonly Dictionary<ulong, Vector3> teleporting;
    private SortedDictionary<string, Vector3> caves;
    private List<PrefabInfo> monuments;
    private bool outpostEnabled;
    private bool banditEnabled;
    private class PrefabInfo
    {
        public Quaternion rotation;
        public Vector2 positionXZ;
        public Vector3 position;
        public Vector3 extents;
        public string name;
        public string prefab;
        public bool sphere;
        public PrefabInfo();
        public PrefabInfo(Vector3 position, Quaternion rotation, Vector3 extents, float extra, string name, string prefab, bool sphere);
        public bool IsInBounds(Vector3 a);
    }

    private static Configuration config;
    public class InterruptSettings
    {
        [JsonProperty(PropertyName = "Interrupt Teleport At Specific Monuments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Monuments { get; set; }
        [JsonProperty(PropertyName = "Above Water")]
        public bool AboveWater { get; set; }
        [JsonProperty(PropertyName = "Under Water")]
        public bool UnderWater { get; set; }
        [JsonProperty(PropertyName = "Balloon")]
        public bool Balloon { get; set; }
        [JsonProperty(PropertyName = "Boats")]
        public bool Boats { get; set; }
        [JsonProperty(PropertyName = "Cargo Ship")]
        public bool Cargo { get; set; }
        [JsonProperty(PropertyName = "Cold")]
        public bool Cold { get; set; }
        [JsonProperty(PropertyName = "Excavator")]
        public bool Excavator { get; set; }
        [JsonProperty(PropertyName = "Hot")]
        public bool Hot { get; set; }
        [JsonProperty(PropertyName = "Hostile")]
        public bool Hostile { get; set; }
        [JsonProperty(PropertyName = "Hostile Includes Towns")]
        public bool IncludeHostileTown { get; set; }
        [JsonProperty(PropertyName = "Hurt")]
        public bool Hurt { get; set; }
        [JsonProperty(PropertyName = "Junkpiles")]
        public bool Junkpiles { get; set; }
        [JsonProperty(PropertyName = "Lift")]
        public bool Lift { get; set; }
        [JsonProperty(PropertyName = "Monument")]
        public bool Monument { get; set; }
        [JsonProperty(PropertyName = "Ignore Monument Marker Prefab")]
        public bool BypassMonumentMarker { get; set; }
        [JsonProperty(PropertyName = "Mounted")]
        public bool Mounted { get; set; }
        [JsonProperty(PropertyName = "Oil Rig")]
        public bool Oilrig { get; set; }
        [JsonProperty(PropertyName = "Safe Zone")]
        public bool Safe { get; set; }
        [JsonProperty(PropertyName = "Swimming")]
        public bool Swimming { get; set; }
    }

    private void OnPlayerSleep(BasePlayer player);
    private List<(Vector3 position, float sqrDistance)> safeZones;
    private bool IsSafeZone(Vector3 a, float extra);
    public bool IsAuthed(BasePlayer player, BuildingPrivlidge priv);
    private List<ulong> delayedTeleports;
    public void DelayedTeleportHome(BasePlayer player);
    private object OnPlayerRespawn(BasePlayer player);
    public class PluginSettings
    {
        [JsonProperty(PropertyName = "Delay Saving Data On Server Save")]
        public double SaveDelay { get; set; }
        [JsonProperty("TPB")]
        public TPBSettings TPB;
        [JsonProperty(PropertyName = "Interrupt TP")]
        public InterruptSettings Interrupt { get; set; }
        [JsonProperty(PropertyName = "Auto Wake Up After Teleport")]
        public bool AutoWakeUp { get; set; }
        [JsonProperty(PropertyName = "Seconds Until Teleporting Home Offline Players Within SafeZones")]
        public float TeleportHomeSafeZone { get; set; }
        [JsonProperty(PropertyName = "Respawn Players At Outpost")]
        public bool RespawnOutpost { get; set; }
        [JsonProperty(PropertyName = "Block Teleport (NoEscape)")]
        public bool BlockNoEscape { get; set; }
        [JsonProperty(PropertyName = "Block Teleport (RaidBlock)")]
        public bool RaidBlock { get; set; }
        [JsonProperty(PropertyName = "Block Teleport (ZoneManager)")]
        public bool BlockZoneFlag { get; set; }
        [JsonProperty(PropertyName = "Block Map Marker Teleport (AbandonedBases)")]
        public bool BlockAbandoned { get; set; }
        [JsonProperty(PropertyName = "Block Map Marker Teleport (RaidableBases)")]
        public bool BlockRaidable { get; set; }
        [JsonProperty(PropertyName = "Chat Name")]
        public string ChatName { get; set; }
        [JsonProperty(PropertyName = "Chat Steam64ID")]
        public ulong ChatID { get; set; }
        [JsonProperty(PropertyName = "Check Boundaries On Teleport X Y Z")]
        public bool CheckBoundaries { get; set; }
        [JsonProperty(PropertyName = "Check Boundaries Min Height")]
        public float BoundaryMin { get; set; }
        [JsonProperty(PropertyName = "Check Boundaries Max Height")]
        public float BoundaryMax { get; set; }
        [JsonProperty(PropertyName = "Check If Inside Rock")]
        public bool Rock { get; set; }
        [JsonProperty(PropertyName = "Height To Prevent Teleporting To/From (0 = disabled)")]
        public float ForcedBoundary { get; set; }
        [JsonProperty(PropertyName = "Data File Directory (Blank = Default)")]
        public string DataFileFolder { get; set; }
        [JsonProperty(PropertyName = "Draw Sphere On Set Home")]
        public bool DrawHomeSphere { get; set; }
        [JsonProperty(PropertyName = "Homes Enabled")]
        public bool HomesEnabled { get; set; }
        [JsonProperty(PropertyName = "TPR Enabled")]
        public bool TPREnabled { get; set; }
        [JsonProperty(PropertyName = "Strict Foundation Check")]
        public bool StrictFoundationCheck { get; set; }
        [JsonProperty(PropertyName = "Minimum Temp")]
        public float MinimumTemp { get; set; }
        [JsonProperty(PropertyName = "Maximum Temp")]
        public float MaximumTemp { get; set; }
        [JsonProperty(PropertyName = "Blocked Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, string> BlockedItems { get; set; }
        [JsonProperty(PropertyName = "Blocked Town Prefabs", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> BlockedPrefabs { get; set; }
        [JsonProperty(PropertyName = "Bypass CMD")]
        public string BypassCMD { get; set; }
        [JsonProperty(PropertyName = "Use Economics")]
        public bool UseEconomics { get; set; }
        [JsonProperty(PropertyName = "Use Server Rewards")]
        public bool UseServerRewards { get; set; }
        [JsonProperty(PropertyName = "Wipe On Upgrade Or Change")]
        public bool WipeOnUpgradeOrChange { get; set; }
        [JsonProperty(PropertyName = "Auto Generate Outpost Location")]
        public bool AutoGenOutpost { get; set; }
        [JsonProperty(PropertyName = "Outpost Map Prefab", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Outpost { get; set; }
        [JsonProperty(PropertyName = "Auto Generate Bandit Location")]
        public bool AutoGenBandit { get; set; }
        [JsonProperty(PropertyName = "Bandit Map Prefab", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Bandit { get; set; }
        [JsonProperty(PropertyName = "Show Time As Seconds Instead")]
        public bool UseSeconds { get; set; }
        [JsonProperty(PropertyName = "Use Quick Teleport")]
        public bool Quick { get; set; }
        [JsonProperty(PropertyName = "Chat Command Color")]
        public string ChatCommandColor;
        [JsonProperty(PropertyName = "Chat Command Argument Color")]
        public string ChatCommandArgumentColor;
        [JsonProperty("Enable Popup Support")]
        public bool UsePopup;
        [JsonProperty("Send Messages To Player")]
        public bool SendMessages;
        [JsonProperty("Block All Teleporting From Inside Authorized Base")]
        public bool BlockAuthorizedTeleporting;
        [JsonProperty("Global Teleport Cooldown")]
        public float Global;
        [JsonProperty("Global VIP Teleport Cooldown")]
        public float GlobalVIP;
        [JsonProperty("Play Sounds Before Teleport")]
        public bool PlaySoundsBeforeTeleport;
        [JsonProperty("Sound Effects Before Teleport", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> DisappearEffects;
        [JsonProperty("Play Sounds After Teleport")]
        public bool PlaySoundsAfterTeleport;
        [JsonProperty("Sound Effects After Teleport", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> ReappearEffects;
    }

    public class TPBSettings
    {
        [JsonProperty(PropertyName = "VIP Countdowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> Countdowns { get; set; }
        [JsonProperty("Countdown")]
        public int Countdown;
        [JsonProperty("Available After X Seconds")]
        public int Time;
    }

    public class AdminSettings
    {
        [JsonProperty(PropertyName = "Announce Teleport To Target")]
        public bool AnnounceTeleportToTarget { get; set; }
        [JsonProperty(PropertyName = "Usable By Admins")]
        public bool UseableByAdmins { get; set; }
        [JsonProperty(PropertyName = "Usable By Moderators")]
        public bool UseableByModerators { get; set; }
        [JsonProperty(PropertyName = "Location Radius")]
        public int LocationRadius { get; set; }
        [JsonProperty(PropertyName = "Teleport Near Default Distance")]
        public int TeleportNearDefaultDistance { get; set; }
        [JsonProperty(PropertyName = "Extra Distance To Block Monument Teleporting")]
        public int ExtraMonumentDistance { get; set; }
    }

    public class HomesSettings
    {
        [JsonProperty(PropertyName = "Homes Limit")]
        public int HomesLimit { get; set; }
        [JsonProperty(PropertyName = "VIP Homes Limits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPHomesLimits { get; set; }
        [JsonProperty(PropertyName = "Allow Sethome At Specific Monuments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> AllowedMonuments { get; set; }
        [JsonProperty(PropertyName = "Allow Sethome At All Monuments")]
        public bool AllowAtAllMonuments { get; set; }
        [JsonProperty(PropertyName = "Allow Sethome On Tugboats")]
        public bool AllowTugboats { get; set; }
        [JsonProperty(PropertyName = "Allow TPB")]
        public bool AllowTPB { get; set; }
        [JsonProperty(PropertyName = "Cooldown")]
        public int Cooldown { get; set; }
        [JsonProperty(PropertyName = "Countdown")]
        public int Countdown { get; set; }
        [JsonProperty(PropertyName = "Daily Limit")]
        public int DailyLimit { get; set; }
        [JsonProperty(PropertyName = "VIP Daily Limits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPDailyLimits { get; set; }
        [JsonProperty(PropertyName = "VIP Cooldowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPCooldowns { get; set; }
        [JsonProperty(PropertyName = "VIP Countdowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPCountdowns { get; set; }
        [JsonProperty(PropertyName = "Location Radius")]
        public int LocationRadius { get; set; }
        [JsonProperty(PropertyName = "Force On Top Of Foundation")]
        public bool ForceOnTopOfFoundation { get; set; }
        [JsonProperty(PropertyName = "Check Foundation For Owner")]
        public bool CheckFoundationForOwner { get; set; }
        [JsonProperty(PropertyName = "Use Friends")]
        public bool UseFriends { get; set; }
        [JsonProperty(PropertyName = "Use Clans")]
        public bool UseClans { get; set; }
        [JsonProperty(PropertyName = "Use Teams")]
        public bool UseTeams { get; set; }
        [JsonProperty(PropertyName = "Usable Out Of Building Blocked")]
        public bool UsableOutOfBuildingBlocked { get; set; }
        [JsonProperty(PropertyName = "Usable Into Building Blocked")]
        public bool UsableIntoBuildingBlocked { get; set; }
        [JsonProperty(PropertyName = "Usable From Safe Zone Only")]
        public bool UsableFromSafeZoneOnly { get; set; }
        [JsonProperty(PropertyName = "Allow Cupboard Owner When Building Blocked")]
        public bool CupOwnerAllowOnBuildingBlocked { get; set; }
        [JsonProperty(PropertyName = "Allow Iceberg")]
        public bool AllowIceberg { get; set; }
        [JsonProperty(PropertyName = "Allow Cave")]
        public bool AllowCave { get; set; }
        [JsonProperty(PropertyName = "Allow Crafting")]
        public bool AllowCraft { get; set; }
        [JsonProperty(PropertyName = "Allow Above Foundation")]
        public bool AllowAboveFoundation { get; set; }
        [JsonProperty(PropertyName = "Check If Home Is Valid On Listhomes")]
        public bool CheckValidOnList { get; set; }
        [JsonProperty(PropertyName = "Pay")]
        public int Pay { get; set; }
        [JsonProperty(PropertyName = "Bypass")]
        public int Bypass { get; set; }
        [JsonProperty(PropertyName = "Hours Before Useable After Wipe")]
        public double Hours { get; set; }
    }

    public class TPTSettings
    {
        [JsonProperty(PropertyName = "Use Friends")]
        public bool UseFriends { get; set; }
        [JsonProperty(PropertyName = "Use Clans")]
        public bool UseClans { get; set; }
        [JsonProperty(PropertyName = "Use Teams")]
        public bool UseTeams { get; set; }
        [JsonProperty(PropertyName = "Allow Cave")]
        public bool AllowCave { get; set; }
        [JsonProperty(PropertyName = "Enabled Color")]
        public string EnabledColor { get; set; }
        [JsonProperty(PropertyName = "Disabled Color")]
        public string DisabledColor { get; set; }
    }

    public class DiscordSettings
    {
        [JsonProperty(PropertyName = "Webhook URL")]
        public string Webhook;
        [JsonProperty(PropertyName = "Log When TPA To Non-Ally Players More Than X Times")]
        public int TPA;
    }

    public class TPRSettings
    {
        [JsonProperty(PropertyName = "Discord")]
        public DiscordSettings Discord { get; set; }
        [JsonProperty("Play Sounds To Request Target")]
        public bool PlaySoundsToRequestTarget;
        [JsonProperty("Teleport Request Sound Effects", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> TeleportRequestEffects;
        [JsonProperty("Play Sounds When Target Accepts")]
        public bool PlaySoundsWhenTargetAccepts;
        [JsonProperty("Teleport Accept Sound Effects", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> TeleportAcceptEffects;
        [JsonProperty(PropertyName = "Require Player To Be Friend, Clan Mate, Or Team Mate")]
        public bool UseClans_Friends_Teams { get; set; }
        [JsonProperty(PropertyName = "Require nteleportation.tpa to accept TPR requests")]
        public bool RequireTPAPermission { get; set; }
        [JsonProperty(PropertyName = "Allow Cave")]
        public bool AllowCave { get; set; }
        [JsonProperty(PropertyName = "Allow TPB")]
        public bool AllowTPB { get; set; }
        [JsonProperty(PropertyName = "Use Blocked Users")]
        public bool UseBlockedUsers { get; set; }
        [JsonProperty(PropertyName = "Cooldown")]
        public int Cooldown { get; set; }
        [JsonProperty(PropertyName = "Countdown")]
        public int Countdown { get; set; }
        [JsonProperty(PropertyName = "Daily Limit")]
        public int DailyLimit { get; set; }
        [JsonProperty(PropertyName = "VIP Daily Limits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPDailyLimits { get; set; }
        [JsonProperty(PropertyName = "VIP Cooldowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPCooldowns { get; set; }
        [JsonProperty(PropertyName = "VIP Countdowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPCountdowns { get; set; }
        [JsonProperty(PropertyName = "Enable Request UI")]
        public bool UI { get; set; }
        [JsonProperty(PropertyName = "Request Duration")]
        public int RequestDuration { get; set; }
        [JsonProperty(PropertyName = "Block TPA On Ceiling")]
        public bool BlockTPAOnCeiling { get; set; }
        [JsonProperty(PropertyName = "Usable Out Of Building Blocked")]
        public bool UsableOutOfBuildingBlocked { get; set; }
        [JsonProperty(PropertyName = "Usable Into Building Blocked")]
        public bool UsableIntoBuildingBlocked { get; set; }
        [JsonProperty(PropertyName = "Allow Cupboard Owner When Building Blocked")]
        public bool CupOwnerAllowOnBuildingBlocked { get; set; }
        [JsonProperty(PropertyName = "Allow Crafting")]
        public bool AllowCraft { get; set; }
        [JsonProperty(PropertyName = "Pay")]
        public int Pay { get; set; }
        [JsonProperty(PropertyName = "Bypass")]
        public int Bypass { get; set; }
        [JsonProperty(PropertyName = "Hours Before Useable After Wipe")]
        public double Hours { get; set; }
    }

    public class TownSettings
    {
        [JsonProperty(PropertyName = "Command Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Set Position From Monument Marker Name")]
        public string MonumentMarkerName { get; set; }
        [JsonProperty(PropertyName = "Set Position From Monument Marker Name Offset")]
        public string MonumentMarkerNameOffset { get; set; }
        [JsonProperty(PropertyName = "Allow TPB")]
        public bool AllowTPB { get; set; }
        [JsonProperty(PropertyName = "Allow Cave")]
        public bool AllowCave { get; set; }
        [JsonProperty(PropertyName = "Cooldown")]
        public int Cooldown { get; set; }
        [JsonProperty(PropertyName = "Countdown")]
        public int Countdown { get; set; }
        [JsonProperty(PropertyName = "Daily Limit")]
        public int DailyLimit { get; set; }
        [JsonProperty(PropertyName = "VIP Daily Limits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPDailyLimits { get; set; }
        [JsonProperty(PropertyName = "VIP Cooldowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPCooldowns { get; set; }
        [JsonProperty(PropertyName = "VIP Countdowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VIPCountdowns { get; set; }
        [JsonProperty(PropertyName = "Location")]
        public Vector3 Location { get; set; }
        [JsonProperty(PropertyName = "Locations", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<Vector3> Locations { get; set; }
        [JsonProperty(PropertyName = "Teleport To Random Location")]
        public bool Random { get; set; }
        [JsonProperty(PropertyName = "Usable Out Of Building Blocked")]
        public bool UsableOutOfBuildingBlocked { get; set; }
        [JsonProperty(PropertyName = "Allow Crafting")]
        public bool AllowCraft { get; set; }
        [JsonProperty(PropertyName = "Pay")]
        public int Pay { get; set; }
        [JsonProperty(PropertyName = "Bypass")]
        public int Bypass { get; set; }
        [JsonProperty(PropertyName = "Hours Before Useable After Wipe")]
        public double Hours { get; set; }
        public bool CanCraft(BasePlayer player, string command);
        public bool CanCave(BasePlayer player, string command);
        [JsonIgnore]
        public StoredData Teleports;
        [JsonIgnore]
        public string Command { get; set; }
    }

    private class Configuration
    {
        [JsonProperty(PropertyName = "Settings")]
        public PluginSettings Settings;
        [JsonProperty(PropertyName = "Admin")]
        public AdminSettings Admin;
        [JsonProperty(PropertyName = "Home")]
        public HomesSettings Home;
        [JsonProperty(PropertyName = "TPT")]
        public TPTSettings TPT;
        [JsonProperty(PropertyName = "TPR")]
        public TPRSettings TPR;
        [JsonProperty(PropertyName = "Dynamic Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, TownSettings> DynamicCommands { get; set; }
    }

    private static Dictionary<string, TownSettings> DefaultCommands;
    public void InitializeDynamicCommands();
    protected override void LoadConfig();
    private bool canSaveConfig;
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private class DisabledData
    {
        [JsonProperty("List of disabled commands")]
        public List<string> DisabledCommands;
        public DisabledData();
    }

    private DisabledData DisabledCommandData;
    private class AdminData
    {
        [JsonProperty("t")]
        public bool Town { get; set; }
        [JsonProperty("u")]
        public ulong UserID { get; set; }
        [JsonProperty("d")]
        public string Home { get; set; }
        [JsonProperty("pl")]
        public Vector3 PreviousLocation { get; set; }
        [JsonProperty("b")]
        public bool BuildingBlocked { get; set; }
        [JsonProperty("c")]
        public bool AllowCrafting { get; set; }
        [JsonProperty("cv")]
        public bool AllowCave { get; set; }
        [JsonProperty("l")]
        public Dictionary<string, Vector3> Locations { get; set; }
    }

    private class HomeData
    {
        public class Entry
        {
            public Vector3 Position;
            public BaseNetworkable Entity;
            public bool isEntity { get; set; }
            public bool wasEntity;
            public Entry();
            public Entry(Vector3 Position);
            public Vector3 Get();
        }

        public class Boat
        {
            public ulong Value;
            public Vector3 Offset;
            public Boat();
            public Boat(Entry entry);
        }

        [JsonProperty("l")]
        public Dictionary<string, Vector3> buildings { get; set; }
        [JsonProperty("b")]
        public Dictionary<string, Boat> boats { get; set; }
        [JsonProperty("t")]
        public TeleportData Teleports { get; set; }
        [JsonIgnore]
        private Dictionary<string, Entry> Cache;
        [JsonIgnore]
        public Dictionary<string, Entry> Locations { get; set; }
        private void InitializeBuildings();
        private void InitializeBoats();
        public bool TryGetValue(string key, Entry homeEntry);
        public void Set(string key, Entry homeEntry);
        public bool Remove(string key);
    }

    public class TeleportData
    {
        [JsonProperty("a")]
        public int Amount { get; set; }
        [JsonProperty("d")]
        public string Date { get; set; }
        [JsonProperty("t")]
        public int Timestamp { get; set; }
    }

    private class TeleportTimer
    {
        public string Town { get; set; }
        public ulong UserID { get; set; }
        public string Home { get; set; }
        public Timer Timer { get; set; }
        public BasePlayer OriginPlayer { get; set; }
        public BasePlayer TargetPlayer { get; set; }
    }

    private List<string> GetMonumentMessages();
    protected override void LoadDefaultMessages();
    private void Init();
    private void OnPlayerConnected(BasePlayer player);
    private string GetPlayerCode(BasePlayer player);
    private Dictionary<string, StoredData> _DynamicData;
    public class StoredData
    {
        public Dictionary<ulong, TeleportData> TPData;
        public bool Changed;
    }

    private void LoadDataAndPerms();
    private bool CanBypassRestrictions(string userid);
    private void RegisterCommand(string command, string callback, string perm);
    private void UnregisterCommand(string command);
    private void RegisterCommand(string key, TownSettings settings, bool justCreated);
    private DynamicConfigFile GetFile(string name);
    private void SetGlobalCooldown(BasePlayer player);
    private float GetGlobalCooldown(BasePlayer player);
    private bool IsEmptyMap();
    private void CheckNewSave();
     bool TrySetNewTownPosition(TownSettings town);
     void OnServerInitialized();
    private void AddCovalenceCommands();
     void OnNewSave(string strFilename);
     void OnServerSave();
     void SaveAllInstant();
     void OnServerShutdown();
     void Unload();
     void OnEntityTakeDamage(BasePlayer player, HitInfo hitInfo);
    private void SendInterruptMessage(TeleportTimer teleportTimer, BasePlayer player, string key);
     object OnPlayerViolation(BasePlayer player, AntiHackType type, float amount);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
    private void SaveTeleportsAdmin();
    private void SaveTeleportsHome();
    private void SaveTeleportsTPR();
    private void SaveTeleportsTPT();
    private void SaveTeleportsTown();
    private void SaveLocation(BasePlayer player, Vector3 position, string home, ulong uid, bool town, bool build, bool craft, bool cave);
    private void RemoveLocation(BasePlayer player);
    private Coroutine _cmc;
    private bool _cmcCompleted;
    private List<PrefabInfo> PrefabVolumeInfo;
    public bool GetCustomMapPrefabRadius(Vector2 v, float radius);
    public bool GetCustomMapPrefabName(ProtoBuf.PrefabData prefab, string prefabName);
    private void SetupVolumeOrTrigger(ProtoBuf.PrefabData prefab, string prefabName);
    private IEnumerator SetupMonuments();
    private void RemoveNearBuildingBlocks(List<BaseEntity> ents);
    private IEnumerator SetupOutpost(PrefabInfo mi);
    private IEnumerator SetupOutpost(MonumentInfo monument);
    private IEnumerator SetupBandit(PrefabInfo mi);
    private IEnumerator SetupBandit(MonumentInfo monument);
    private T GetColliderFrom(T expected, Vector3 a, string text);
    public IEnumerator CalculateMonumentSize(Vector3 from, Quaternion rot, Bounds b, string text, string prefab);
    public static List<Vector3> GetCardinalPositions(Vector3 center, Quaternion rotation, float radius);
    public void CalculateFurthestDistances(string text, List<Vector3> positions, Vector3 center, Quaternion rot, float x, float z);
    private static void DrawText(BasePlayer player, float duration, Color color, Vector3 from, object text);
    private static void DrawLine(BasePlayer player, float duration, Color color, Vector3 from, Vector3 to);
    private static void DrawSphere(BasePlayer player, float duration, Color color, Vector3 from, float radius);
    private bool TeleportInForcedBoundary(BasePlayer[] players);
    private void TeleportRequestUI(BasePlayer player, string displayName);
    public void DestroyTeleportRequestCUI(BasePlayer player);
    [ConsoleCommand("ntp.accept")]
    private void ccmdAccept(ConsoleSystem.Arg arg);
    [ConsoleCommand("ntp.reject")]
    private void ccmdReject(ConsoleSystem.Arg arg);
    private bool OutOfRange(MonumentInfo m, Vector3 a, bool checkHeight);
    private bool OutOfRange(Vector3 m, Vector3 a, float r, bool checkHeight);
    private void CommandToggle(IPlayer user, string command, string[] args);
    private void CommandTeleport(IPlayer user, string command, string[] args);
    private Vector3 GetAdminLocation(string value);
    private void CommandTeleportNear(IPlayer user, string command, string[] args);
    private void CommandTeleportLocation(IPlayer user, string command, string[] args);
    private void CommandSaveTeleportLocation(IPlayer user, string command, string[] args);
    private void CommandRemoveTeleportLocation(IPlayer user, string command, string[] args);
    private void CommandTeleportBack(IPlayer user, string command, string[] args);
    private void TeleportBack(BasePlayer player, AdminData adminData, int countdown);
    private void CommandSetHome(IPlayer user, string command, string[] args);
    private void CommandRemoveHome(IPlayer user, string command, string[] args);
    private void CommandHome(IPlayer user, string command, string[] args);
    private void CommandHomeRadius(IPlayer user, string command, string[] args);
    private void SendHomeError(BasePlayer player, List<string> toRemove, string err, string homeName, Vector3 position, bool wasEntity, bool send);
    private void CommandHomeDelete(IPlayer user, string command, string[] args);
    private void CommandHomeAdminTP(IPlayer user, string command, string[] args);
    private bool UseEconomy();
    private bool CheckEconomy(BasePlayer player, double bypass, bool withdraw, bool deposit);
    private void cmdChatHomeTP(BasePlayer player, string command, string[] args);
    private void CommandListHomes(IPlayer user, string command, string[] args);
    private void ValidateHomes(BasePlayer player, HomeData homeData, bool showRemoved, bool showLoc);
    private void CommandHomeHomes(IPlayer user, string command, string[] args);
    private void CommandTeleportAcceptToggle(IPlayer user, string command, string[] args);
    public bool IsOnSameTeam(ulong playerId, ulong targetId);
    private bool AreFriends(string playerId, string targetId);
    private bool IsFriend(string playerId, string targetId);
    private bool IsInSameClan(string playerId, string targetId);
    private bool InstantTeleportAccept(BasePlayer target, BasePlayer player);
    private bool IsEnabled(string targetId, string value);
    private void ToggleTPTEnabled(BasePlayer target, string command, string[] args);
    private string GetMultiplePlayers(List<BasePlayer> players);
    private double GetUseableTime(BasePlayer player, double hours);
    private void CommandTeleportRequest(IPlayer user, string command, string[] args);
    private void CommandTeleportAccept(IPlayer user, string command, string[] args);
    private void CommandWipeHomes(IPlayer user, string command, string[] args);
    private void CommandTeleportHelp(IPlayer user, string command, string[] args);
    private List<string> _tpid;
    private void CommandTeleportInfo(IPlayer user, string command, string[] args);
    private void CommandTeleportCancel(IPlayer user, string command, string[] args);
    private void CommandDynamic(IPlayer user, string command, string[] args);
    private void CommandCustom(IPlayer user, string command, string[] args);
    private TownSettings GetSettings(string command, ulong userid);
    private bool IsServerCommand(IPlayer user, string command, string[] args);
    private void CommandTown(IPlayer user, string command, string[] args);
    private double SecondsUntilTomorrow();
    private void Interrupt(BasePlayer player, bool paidmoney, double bypass);
    private void CommandTeleportII(IPlayer user, string command, string[] args);
    private void CommandSphereMonuments(IPlayer user, string command, string[] args);
    private void CommandDrawChecks(BasePlayer player, Vector3 a, float diameter);
    private void CommandDrawTopology(BasePlayer player, Vector3 a, float diameter);
    private bool IsMonument(Vector3 v);
    private bool HasMonumentTopology(Vector3 v);
    private bool IsCave(Vector3 v);
    private bool HasPreventBuildingCollider(Vector3 v);
    private void CommandImportHomes(IPlayer user, string command, string[] args);
    private void RequestTimedOut(BasePlayer player, BasePlayer target);
    private void CommandPluginInfo(IPlayer user, string command, string[] args);
    private readonly System.Text.StringBuilder _sb;
    private string FormatTime(BasePlayer player, double seconds);
    public void Teleport(BasePlayer player, BasePlayer target, bool build, bool craft, bool cave);
    public void Teleport(BasePlayer player, float x, float y, float z, bool build, bool craft, bool cave);
    [HookMethod("Teleport")]
    public void Teleport(BasePlayer player, Vector3 newPosition, string home, ulong uid, bool town, bool allowTPB, bool build, bool craft, bool cave);
    private bool CanWake(BasePlayer player);
    public void RemoveProtections(ulong userid);
    public void StartSleeping(BasePlayer player);
    private void EndSleeping(BasePlayer player);
    [PluginReference]
     Plugin RaidableBases;
     Plugin AbandonedBases;
    private List<string> blockMapMarker;
    private void CommandBlockMapMarker(IPlayer user, string command, string[] args);
    private void OnMapMarkerAdded(BasePlayer player, ProtoBuf.MapNote note);
    private bool EventTerritory(Vector3 worldPosition);
    private string CanPlayerTeleport(BasePlayer player, Vector3[] vectors);
    private string CanPlayerTeleportHome(BasePlayer player, Vector3 homePos);
    private bool CanCraftHome(BasePlayer player);
    private bool CanCaveHome(BasePlayer player);
    private bool CanCraftTPR(BasePlayer player);
    private bool CanCaveTPR(BasePlayer player);
    private List<string> monumentExceptions;
    private bool IsInAllowedMonument(Vector3 target, string mode);
    private string NearMonument(Vector3 target, bool check, string mode);
    private string CheckPlayer(BasePlayer player, bool build, bool craft, bool origin, string mode, bool allowcave);
    private bool IsWaterBlockedAbove(BasePlayer player, BaseEntity entity);
    private string CheckTargetLocation(BasePlayer player, Vector3 targetLocation, bool usableIntoBuildingBlocked, bool cupOwnerAllowOnBuildingBlocked);
    private bool CheckCupboardBlock(BuildingBlock block, BasePlayer player, bool cupOwnerAllowOnBuildingBlocked);
    private string CheckItems(BasePlayer player);
    private static List<T> FindEntitiesOfType(Vector3 a, float n, int m);
    private bool IsInsideEntity(Vector3 a);
    private string IsInsideEntity(Vector3 targetLocation, ulong userid, string mode);
    private bool UnderneathFoundation(Vector3 a);
    private string CheckFoundation(ulong userid, Vector3 position, string mode);
    private bool IsInTrainTunnels(Vector3 a);
    private bool IsUnderground(Vector3 a);
    private bool IsUnderwaterLab(Vector3 a);
    private bool IsInBounds(BuildingBlock block, Vector3 a);
    private bool IsBlockedOnIceberg(Vector3 position);
    private BuildingBlock GetFoundationOwned(Vector3 position, ulong userID);
    private bool IsAlly(ulong playerId, ulong targetId, bool useTeams, bool useClans, bool useFriends);
     bool IsBlockedUser(ulong playerid, ulong targetid);
    private bool PassesStrictCheck(BaseEntity entity, Vector3 position);
    private bool IsExternalWallOverlapped(Vector3 center, Vector3 position);
    private T FindEntity(BaseEntity entity);
    private T GetStandingOnEntity(BasePlayer player, float distance, int layerMask);
    private T GetStandingOnEntity(Vector3 a, float distance, int layerMask);
    private bool IsStandingOnEntity(Vector3 a, float distance, int layerMask, BaseEntity entity, string[] prefabs);
    private bool IsCeiling(DecayEntity entity);
    private bool CheckBoundaries(float x, float y, float z);
    private Vector3 GetGroundBuilding(Vector3 a);
    public bool AboveWater(Vector3 a);
    private static bool ContainsTopology(TerrainTopology.Enum mask, Vector3 position, float radius);
    private bool IsInCave(Vector3 a);
    private bool GetLift(Vector3 position);
    private bool IsOnJunkPile(BasePlayer player);
    private bool IsAllowed(BasePlayer player, string perm);
    private bool IsAllowedMsg(BasePlayer player, string perm);
    private Effect reusableSoundEffectInstance;
    private void SendEffect(BasePlayer player, List<string> effects);
    private int GetHigher(BasePlayer player, Dictionary<string, int> limits, int limit, bool unlimited);
    private int GetLower(BasePlayer player, Dictionary<string, int> times, int time);
    private void CheckPerms(Dictionary<string, int> limits);
    protected new static void Puts(string format, object[] args);
    private string _(string msgId, BasePlayer player, object[] args);
    private void PrintMsgL(IPlayer user, string msgId, object[] args);
    private void PrintMsgL(BasePlayer player, string msgId, object[] args);
    private void PrintMsgL(ulong userid, string msgId, object[] args);
    private void PrintMsg(BasePlayer player, string message);
    private void SendDiscordMessage(BasePlayer player, BasePlayer target);
    private void CheckDiscordMessages();
    private List<string> discordMessages;
    private Dictionary<(ulong, ulong), int> teleportCounts;
    public class DiscordMessage
    {
        public DiscordMessage(string content);
        [JsonProperty("content")]
        public string Content { get; set; }
        public string ToJson();
    }

     void DrawMonument(BasePlayer player, Vector3 center, Vector3 extents, Quaternion rotation, Color color, float duration);
    private ulong FindPlayersSingleId(string nameOrIdOrIp, BasePlayer player);
    private BasePlayer FindPlayersSingle(string value, BasePlayer player);
    private List<BasePlayer> FindPlayers(string arg, bool all);
    private bool API_HavePendingRequest(BasePlayer player);
    private bool API_HaveAvailableHomes(BasePlayer player);
    [HookMethod("API_GetHomes")]
    public Dictionary<string, Vector3> GetPlayerHomes(BasePlayer player);
    private List<Vector3> API_GetLocations(string command);
    private Dictionary<string, List<Vector3>> API_GetAllLocations();
    private int GetLimitRemaining(BasePlayer player, string type);
    private int GetCooldownRemaining(BasePlayer player, string type);
    private int GetCountdownRemaining(BasePlayer player, string type);
    private class UnityVector3Converter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

    private class CustomComparerDictionaryCreationConverter : CustomCreationConverter<IDictionary>
    {
        private readonly IEqualityComparer<T> comparer;
        public CustomComparerDictionaryCreationConverter(IEqualityComparer<T> comparer);
        public override bool CanConvert(Type objectType);
        private static bool HasCompatibleInterface(Type objectType);
        private static bool HasGenericTypeDefinition(Type objectType, Type typeDefinition);
        private static bool HasCompatibleConstructor(Type objectType);
        public override IDictionary Create(Type objectType);
    }

    public class Exploits
    {
        public static bool TestInsideRock(Vector3 a);
        private static bool IsRockFaceDownwards(Vector3 a);
        private static bool IsRockFaceUpwards(Vector3 point);
        private static bool IsRock(string name);
    }

    [HookMethod("SendHelpText")]
    private void SendHelpText(BasePlayer player);
}

private class PrefabInfo
{
    public Quaternion rotation;
    public Vector2 positionXZ;
    public Vector3 position;
    public Vector3 extents;
    public string name;
    public string prefab;
    public bool sphere;
    public PrefabInfo();
    public PrefabInfo(Vector3 position, Quaternion rotation, Vector3 extents, float extra, string name, string prefab, bool sphere);
    public bool IsInBounds(Vector3 a);
}

public class InterruptSettings
{
    [JsonProperty(PropertyName = "Interrupt Teleport At Specific Monuments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Monuments { get; set; }
    [JsonProperty(PropertyName = "Above Water")]
    public bool AboveWater { get; set; }
    [JsonProperty(PropertyName = "Under Water")]
    public bool UnderWater { get; set; }
    [JsonProperty(PropertyName = "Balloon")]
    public bool Balloon { get; set; }
    [JsonProperty(PropertyName = "Boats")]
    public bool Boats { get; set; }
    [JsonProperty(PropertyName = "Cargo Ship")]
    public bool Cargo { get; set; }
    [JsonProperty(PropertyName = "Cold")]
    public bool Cold { get; set; }
    [JsonProperty(PropertyName = "Excavator")]
    public bool Excavator { get; set; }
    [JsonProperty(PropertyName = "Hot")]
    public bool Hot { get; set; }
    [JsonProperty(PropertyName = "Hostile")]
    public bool Hostile { get; set; }
    [JsonProperty(PropertyName = "Hostile Includes Towns")]
    public bool IncludeHostileTown { get; set; }
    [JsonProperty(PropertyName = "Hurt")]
    public bool Hurt { get; set; }
    [JsonProperty(PropertyName = "Junkpiles")]
    public bool Junkpiles { get; set; }
    [JsonProperty(PropertyName = "Lift")]
    public bool Lift { get; set; }
    [JsonProperty(PropertyName = "Monument")]
    public bool Monument { get; set; }
    [JsonProperty(PropertyName = "Ignore Monument Marker Prefab")]
    public bool BypassMonumentMarker { get; set; }
    [JsonProperty(PropertyName = "Mounted")]
    public bool Mounted { get; set; }
    [JsonProperty(PropertyName = "Oil Rig")]
    public bool Oilrig { get; set; }
    [JsonProperty(PropertyName = "Safe Zone")]
    public bool Safe { get; set; }
    [JsonProperty(PropertyName = "Swimming")]
    public bool Swimming { get; set; }
}

public class PluginSettings
{
    [JsonProperty(PropertyName = "Delay Saving Data On Server Save")]
    public double SaveDelay { get; set; }
    [JsonProperty("TPB")]
    public TPBSettings TPB;
    [JsonProperty(PropertyName = "Interrupt TP")]
    public InterruptSettings Interrupt { get; set; }
    [JsonProperty(PropertyName = "Auto Wake Up After Teleport")]
    public bool AutoWakeUp { get; set; }
    [JsonProperty(PropertyName = "Seconds Until Teleporting Home Offline Players Within SafeZones")]
    public float TeleportHomeSafeZone { get; set; }
    [JsonProperty(PropertyName = "Respawn Players At Outpost")]
    public bool RespawnOutpost { get; set; }
    [JsonProperty(PropertyName = "Block Teleport (NoEscape)")]
    public bool BlockNoEscape { get; set; }
    [JsonProperty(PropertyName = "Block Teleport (RaidBlock)")]
    public bool RaidBlock { get; set; }
    [JsonProperty(PropertyName = "Block Teleport (ZoneManager)")]
    public bool BlockZoneFlag { get; set; }
    [JsonProperty(PropertyName = "Block Map Marker Teleport (AbandonedBases)")]
    public bool BlockAbandoned { get; set; }
    [JsonProperty(PropertyName = "Block Map Marker Teleport (RaidableBases)")]
    public bool BlockRaidable { get; set; }
    [JsonProperty(PropertyName = "Chat Name")]
    public string ChatName { get; set; }
    [JsonProperty(PropertyName = "Chat Steam64ID")]
    public ulong ChatID { get; set; }
    [JsonProperty(PropertyName = "Check Boundaries On Teleport X Y Z")]
    public bool CheckBoundaries { get; set; }
    [JsonProperty(PropertyName = "Check Boundaries Min Height")]
    public float BoundaryMin { get; set; }
    [JsonProperty(PropertyName = "Check Boundaries Max Height")]
    public float BoundaryMax { get; set; }
    [JsonProperty(PropertyName = "Check If Inside Rock")]
    public bool Rock { get; set; }
    [JsonProperty(PropertyName = "Height To Prevent Teleporting To/From (0 = disabled)")]
    public float ForcedBoundary { get; set; }
    [JsonProperty(PropertyName = "Data File Directory (Blank = Default)")]
    public string DataFileFolder { get; set; }
    [JsonProperty(PropertyName = "Draw Sphere On Set Home")]
    public bool DrawHomeSphere { get; set; }
    [JsonProperty(PropertyName = "Homes Enabled")]
    public bool HomesEnabled { get; set; }
    [JsonProperty(PropertyName = "TPR Enabled")]
    public bool TPREnabled { get; set; }
    [JsonProperty(PropertyName = "Strict Foundation Check")]
    public bool StrictFoundationCheck { get; set; }
    [JsonProperty(PropertyName = "Minimum Temp")]
    public float MinimumTemp { get; set; }
    [JsonProperty(PropertyName = "Maximum Temp")]
    public float MaximumTemp { get; set; }
    [JsonProperty(PropertyName = "Blocked Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, string> BlockedItems { get; set; }
    [JsonProperty(PropertyName = "Blocked Town Prefabs", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> BlockedPrefabs { get; set; }
    [JsonProperty(PropertyName = "Bypass CMD")]
    public string BypassCMD { get; set; }
    [JsonProperty(PropertyName = "Use Economics")]
    public bool UseEconomics { get; set; }
    [JsonProperty(PropertyName = "Use Server Rewards")]
    public bool UseServerRewards { get; set; }
    [JsonProperty(PropertyName = "Wipe On Upgrade Or Change")]
    public bool WipeOnUpgradeOrChange { get; set; }
    [JsonProperty(PropertyName = "Auto Generate Outpost Location")]
    public bool AutoGenOutpost { get; set; }
    [JsonProperty(PropertyName = "Outpost Map Prefab", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Outpost { get; set; }
    [JsonProperty(PropertyName = "Auto Generate Bandit Location")]
    public bool AutoGenBandit { get; set; }
    [JsonProperty(PropertyName = "Bandit Map Prefab", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Bandit { get; set; }
    [JsonProperty(PropertyName = "Show Time As Seconds Instead")]
    public bool UseSeconds { get; set; }
    [JsonProperty(PropertyName = "Use Quick Teleport")]
    public bool Quick { get; set; }
    [JsonProperty(PropertyName = "Chat Command Color")]
    public string ChatCommandColor;
    [JsonProperty(PropertyName = "Chat Command Argument Color")]
    public string ChatCommandArgumentColor;
    [JsonProperty("Enable Popup Support")]
    public bool UsePopup;
    [JsonProperty("Send Messages To Player")]
    public bool SendMessages;
    [JsonProperty("Block All Teleporting From Inside Authorized Base")]
    public bool BlockAuthorizedTeleporting;
    [JsonProperty("Global Teleport Cooldown")]
    public float Global;
    [JsonProperty("Global VIP Teleport Cooldown")]
    public float GlobalVIP;
    [JsonProperty("Play Sounds Before Teleport")]
    public bool PlaySoundsBeforeTeleport;
    [JsonProperty("Sound Effects Before Teleport", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> DisappearEffects;
    [JsonProperty("Play Sounds After Teleport")]
    public bool PlaySoundsAfterTeleport;
    [JsonProperty("Sound Effects After Teleport", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> ReappearEffects;
}

public class TPBSettings
{
    [JsonProperty(PropertyName = "VIP Countdowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> Countdowns { get; set; }
    [JsonProperty("Countdown")]
    public int Countdown;
    [JsonProperty("Available After X Seconds")]
    public int Time;
}

public class AdminSettings
{
    [JsonProperty(PropertyName = "Announce Teleport To Target")]
    public bool AnnounceTeleportToTarget { get; set; }
    [JsonProperty(PropertyName = "Usable By Admins")]
    public bool UseableByAdmins { get; set; }
    [JsonProperty(PropertyName = "Usable By Moderators")]
    public bool UseableByModerators { get; set; }
    [JsonProperty(PropertyName = "Location Radius")]
    public int LocationRadius { get; set; }
    [JsonProperty(PropertyName = "Teleport Near Default Distance")]
    public int TeleportNearDefaultDistance { get; set; }
    [JsonProperty(PropertyName = "Extra Distance To Block Monument Teleporting")]
    public int ExtraMonumentDistance { get; set; }
}

public class HomesSettings
{
    [JsonProperty(PropertyName = "Homes Limit")]
    public int HomesLimit { get; set; }
    [JsonProperty(PropertyName = "VIP Homes Limits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPHomesLimits { get; set; }
    [JsonProperty(PropertyName = "Allow Sethome At Specific Monuments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> AllowedMonuments { get; set; }
    [JsonProperty(PropertyName = "Allow Sethome At All Monuments")]
    public bool AllowAtAllMonuments { get; set; }
    [JsonProperty(PropertyName = "Allow Sethome On Tugboats")]
    public bool AllowTugboats { get; set; }
    [JsonProperty(PropertyName = "Allow TPB")]
    public bool AllowTPB { get; set; }
    [JsonProperty(PropertyName = "Cooldown")]
    public int Cooldown { get; set; }
    [JsonProperty(PropertyName = "Countdown")]
    public int Countdown { get; set; }
    [JsonProperty(PropertyName = "Daily Limit")]
    public int DailyLimit { get; set; }
    [JsonProperty(PropertyName = "VIP Daily Limits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPDailyLimits { get; set; }
    [JsonProperty(PropertyName = "VIP Cooldowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPCooldowns { get; set; }
    [JsonProperty(PropertyName = "VIP Countdowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPCountdowns { get; set; }
    [JsonProperty(PropertyName = "Location Radius")]
    public int LocationRadius { get; set; }
    [JsonProperty(PropertyName = "Force On Top Of Foundation")]
    public bool ForceOnTopOfFoundation { get; set; }
    [JsonProperty(PropertyName = "Check Foundation For Owner")]
    public bool CheckFoundationForOwner { get; set; }
    [JsonProperty(PropertyName = "Use Friends")]
    public bool UseFriends { get; set; }
    [JsonProperty(PropertyName = "Use Clans")]
    public bool UseClans { get; set; }
    [JsonProperty(PropertyName = "Use Teams")]
    public bool UseTeams { get; set; }
    [JsonProperty(PropertyName = "Usable Out Of Building Blocked")]
    public bool UsableOutOfBuildingBlocked { get; set; }
    [JsonProperty(PropertyName = "Usable Into Building Blocked")]
    public bool UsableIntoBuildingBlocked { get; set; }
    [JsonProperty(PropertyName = "Usable From Safe Zone Only")]
    public bool UsableFromSafeZoneOnly { get; set; }
    [JsonProperty(PropertyName = "Allow Cupboard Owner When Building Blocked")]
    public bool CupOwnerAllowOnBuildingBlocked { get; set; }
    [JsonProperty(PropertyName = "Allow Iceberg")]
    public bool AllowIceberg { get; set; }
    [JsonProperty(PropertyName = "Allow Cave")]
    public bool AllowCave { get; set; }
    [JsonProperty(PropertyName = "Allow Crafting")]
    public bool AllowCraft { get; set; }
    [JsonProperty(PropertyName = "Allow Above Foundation")]
    public bool AllowAboveFoundation { get; set; }
    [JsonProperty(PropertyName = "Check If Home Is Valid On Listhomes")]
    public bool CheckValidOnList { get; set; }
    [JsonProperty(PropertyName = "Pay")]
    public int Pay { get; set; }
    [JsonProperty(PropertyName = "Bypass")]
    public int Bypass { get; set; }
    [JsonProperty(PropertyName = "Hours Before Useable After Wipe")]
    public double Hours { get; set; }
}

public class TPTSettings
{
    [JsonProperty(PropertyName = "Use Friends")]
    public bool UseFriends { get; set; }
    [JsonProperty(PropertyName = "Use Clans")]
    public bool UseClans { get; set; }
    [JsonProperty(PropertyName = "Use Teams")]
    public bool UseTeams { get; set; }
    [JsonProperty(PropertyName = "Allow Cave")]
    public bool AllowCave { get; set; }
    [JsonProperty(PropertyName = "Enabled Color")]
    public string EnabledColor { get; set; }
    [JsonProperty(PropertyName = "Disabled Color")]
    public string DisabledColor { get; set; }
}

public class DiscordSettings
{
    [JsonProperty(PropertyName = "Webhook URL")]
    public string Webhook;
    [JsonProperty(PropertyName = "Log When TPA To Non-Ally Players More Than X Times")]
    public int TPA;
}

public class TPRSettings
{
    [JsonProperty(PropertyName = "Discord")]
    public DiscordSettings Discord { get; set; }
    [JsonProperty("Play Sounds To Request Target")]
    public bool PlaySoundsToRequestTarget;
    [JsonProperty("Teleport Request Sound Effects", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> TeleportRequestEffects;
    [JsonProperty("Play Sounds When Target Accepts")]
    public bool PlaySoundsWhenTargetAccepts;
    [JsonProperty("Teleport Accept Sound Effects", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> TeleportAcceptEffects;
    [JsonProperty(PropertyName = "Require Player To Be Friend, Clan Mate, Or Team Mate")]
    public bool UseClans_Friends_Teams { get; set; }
    [JsonProperty(PropertyName = "Require nteleportation.tpa to accept TPR requests")]
    public bool RequireTPAPermission { get; set; }
    [JsonProperty(PropertyName = "Allow Cave")]
    public bool AllowCave { get; set; }
    [JsonProperty(PropertyName = "Allow TPB")]
    public bool AllowTPB { get; set; }
    [JsonProperty(PropertyName = "Use Blocked Users")]
    public bool UseBlockedUsers { get; set; }
    [JsonProperty(PropertyName = "Cooldown")]
    public int Cooldown { get; set; }
    [JsonProperty(PropertyName = "Countdown")]
    public int Countdown { get; set; }
    [JsonProperty(PropertyName = "Daily Limit")]
    public int DailyLimit { get; set; }
    [JsonProperty(PropertyName = "VIP Daily Limits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPDailyLimits { get; set; }
    [JsonProperty(PropertyName = "VIP Cooldowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPCooldowns { get; set; }
    [JsonProperty(PropertyName = "VIP Countdowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPCountdowns { get; set; }
    [JsonProperty(PropertyName = "Enable Request UI")]
    public bool UI { get; set; }
    [JsonProperty(PropertyName = "Request Duration")]
    public int RequestDuration { get; set; }
    [JsonProperty(PropertyName = "Block TPA On Ceiling")]
    public bool BlockTPAOnCeiling { get; set; }
    [JsonProperty(PropertyName = "Usable Out Of Building Blocked")]
    public bool UsableOutOfBuildingBlocked { get; set; }
    [JsonProperty(PropertyName = "Usable Into Building Blocked")]
    public bool UsableIntoBuildingBlocked { get; set; }
    [JsonProperty(PropertyName = "Allow Cupboard Owner When Building Blocked")]
    public bool CupOwnerAllowOnBuildingBlocked { get; set; }
    [JsonProperty(PropertyName = "Allow Crafting")]
    public bool AllowCraft { get; set; }
    [JsonProperty(PropertyName = "Pay")]
    public int Pay { get; set; }
    [JsonProperty(PropertyName = "Bypass")]
    public int Bypass { get; set; }
    [JsonProperty(PropertyName = "Hours Before Useable After Wipe")]
    public double Hours { get; set; }
}

public class TownSettings
{
    [JsonProperty(PropertyName = "Command Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Set Position From Monument Marker Name")]
    public string MonumentMarkerName { get; set; }
    [JsonProperty(PropertyName = "Set Position From Monument Marker Name Offset")]
    public string MonumentMarkerNameOffset { get; set; }
    [JsonProperty(PropertyName = "Allow TPB")]
    public bool AllowTPB { get; set; }
    [JsonProperty(PropertyName = "Allow Cave")]
    public bool AllowCave { get; set; }
    [JsonProperty(PropertyName = "Cooldown")]
    public int Cooldown { get; set; }
    [JsonProperty(PropertyName = "Countdown")]
    public int Countdown { get; set; }
    [JsonProperty(PropertyName = "Daily Limit")]
    public int DailyLimit { get; set; }
    [JsonProperty(PropertyName = "VIP Daily Limits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPDailyLimits { get; set; }
    [JsonProperty(PropertyName = "VIP Cooldowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPCooldowns { get; set; }
    [JsonProperty(PropertyName = "VIP Countdowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VIPCountdowns { get; set; }
    [JsonProperty(PropertyName = "Location")]
    public Vector3 Location { get; set; }
    [JsonProperty(PropertyName = "Locations", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<Vector3> Locations { get; set; }
    [JsonProperty(PropertyName = "Teleport To Random Location")]
    public bool Random { get; set; }
    [JsonProperty(PropertyName = "Usable Out Of Building Blocked")]
    public bool UsableOutOfBuildingBlocked { get; set; }
    [JsonProperty(PropertyName = "Allow Crafting")]
    public bool AllowCraft { get; set; }
    [JsonProperty(PropertyName = "Pay")]
    public int Pay { get; set; }
    [JsonProperty(PropertyName = "Bypass")]
    public int Bypass { get; set; }
    [JsonProperty(PropertyName = "Hours Before Useable After Wipe")]
    public double Hours { get; set; }
    public bool CanCraft(BasePlayer player, string command);
    public bool CanCave(BasePlayer player, string command);
    [JsonIgnore]
    public StoredData Teleports;
    [JsonIgnore]
    public string Command { get; set; }
}

private class Configuration
{
    [JsonProperty(PropertyName = "Settings")]
    public PluginSettings Settings;
    [JsonProperty(PropertyName = "Admin")]
    public AdminSettings Admin;
    [JsonProperty(PropertyName = "Home")]
    public HomesSettings Home;
    [JsonProperty(PropertyName = "TPT")]
    public TPTSettings TPT;
    [JsonProperty(PropertyName = "TPR")]
    public TPRSettings TPR;
    [JsonProperty(PropertyName = "Dynamic Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, TownSettings> DynamicCommands { get; set; }
}

private class DisabledData
{
    [JsonProperty("List of disabled commands")]
    public List<string> DisabledCommands;
    public DisabledData();
}

private class AdminData
{
    [JsonProperty("t")]
    public bool Town { get; set; }
    [JsonProperty("u")]
    public ulong UserID { get; set; }
    [JsonProperty("d")]
    public string Home { get; set; }
    [JsonProperty("pl")]
    public Vector3 PreviousLocation { get; set; }
    [JsonProperty("b")]
    public bool BuildingBlocked { get; set; }
    [JsonProperty("c")]
    public bool AllowCrafting { get; set; }
    [JsonProperty("cv")]
    public bool AllowCave { get; set; }
    [JsonProperty("l")]
    public Dictionary<string, Vector3> Locations { get; set; }
}

private class HomeData
{
    public class Entry
    {
        public Vector3 Position;
        public BaseNetworkable Entity;
        public bool isEntity { get; set; }
        public bool wasEntity;
        public Entry();
        public Entry(Vector3 Position);
        public Vector3 Get();
    }

    public class Boat
    {
        public ulong Value;
        public Vector3 Offset;
        public Boat();
        public Boat(Entry entry);
    }

    [JsonProperty("l")]
    public Dictionary<string, Vector3> buildings { get; set; }
    [JsonProperty("b")]
    public Dictionary<string, Boat> boats { get; set; }
    [JsonProperty("t")]
    public TeleportData Teleports { get; set; }
    [JsonIgnore]
    private Dictionary<string, Entry> Cache;
    [JsonIgnore]
    public Dictionary<string, Entry> Locations { get; set; }
    private void InitializeBuildings();
    private void InitializeBoats();
    public bool TryGetValue(string key, Entry homeEntry);
    public void Set(string key, Entry homeEntry);
    public bool Remove(string key);
}

public class Entry
{
    public Vector3 Position;
    public BaseNetworkable Entity;
    public bool isEntity { get; set; }
    public bool wasEntity;
    public Entry();
    public Entry(Vector3 Position);
    public Vector3 Get();
}

public class Boat
{
    public ulong Value;
    public Vector3 Offset;
    public Boat();
    public Boat(Entry entry);
}

public class TeleportData
{
    [JsonProperty("a")]
    public int Amount { get; set; }
    [JsonProperty("d")]
    public string Date { get; set; }
    [JsonProperty("t")]
    public int Timestamp { get; set; }
}

private class TeleportTimer
{
    public string Town { get; set; }
    public ulong UserID { get; set; }
    public string Home { get; set; }
    public Timer Timer { get; set; }
    public BasePlayer OriginPlayer { get; set; }
    public BasePlayer TargetPlayer { get; set; }
}

public class StoredData
{
    public Dictionary<ulong, TeleportData> TPData;
    public bool Changed;
}

public class DiscordMessage
{
    public DiscordMessage(string content);
    [JsonProperty("content")]
    public string Content { get; set; }
    public string ToJson();
}

private class UnityVector3Converter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}

private class CustomComparerDictionaryCreationConverter : CustomCreationConverter<IDictionary>
{
    private readonly IEqualityComparer<T> comparer;
    public CustomComparerDictionaryCreationConverter(IEqualityComparer<T> comparer);
    public override bool CanConvert(Type objectType);
    private static bool HasCompatibleInterface(Type objectType);
    private static bool HasGenericTypeDefinition(Type objectType, Type typeDefinition);
    private static bool HasCompatibleConstructor(Type objectType);
    public override IDictionary Create(Type objectType);
}

public class Exploits
{
    public static bool TestInsideRock(Vector3 a);
    private static bool IsRockFaceDownwards(Vector3 a);
    private static bool IsRockFaceUpwards(Vector3 point);
    private static bool IsRock(string name);
}

Oxide.Plugins.NTeleportationExtensionMethods
public static class ExtensionMethods
{
    public static bool All(IEnumerable<T> a, Func<T, bool> b);
    public static T FirstOrDefault(IEnumerable<T> a, Func<T, bool> b);
    public static IEnumerable<V> Select(IEnumerable<T> a, Func<T, V> b);
    public static string[] Skip(string[] a, int b);
    public static List<T> Take(IList<T> a, int b);
    public static Dictionary<T, V> ToDictionary(IEnumerable<S> a, Func<S, T> b, Func<S, V> c);
    public static List<T> ToList(IEnumerable<T> a);
    public static List<T> Where(IEnumerable<T> a, Func<T, bool> b);
    public static List<T> OrderByDescending(IEnumerable<T> a, Func<T, TKey> s);
    public static int Count(IEnumerable<T> a, Func<T, bool> b);
    public static bool IsKilled(BaseNetworkable a);
    public static void ResetToPool(List<T> obj);
}


```

---

## NudistHeli by Panduck - Configurable restricted belt items, hostile time, and max clothing count

```csharp
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Nudist Heli", "Panduck", "0.1.1")]
[Description("Configurable helicopter engagement behaviour.")]
public class NudistHeli : RustPlugin
{
    private NudistHeliSettings _settings;
    private Dictionary<BasePlayer, double> _hostilePlayers;
    private static double CurrentTime { get; set; }
    private static NudistHeliSettings GetDefaultConfig();
    public class NudistHeliSettings
    {
        public int MaxClothingCount { get; set; }
        public float HostileTime { get; set; }
        public HashSet<string> RestrictedWeapons { get; set; }
        public bool OnlyEngageOnWeaponHeld { get; set; }
    }

    protected override void LoadDefaultConfig();
    private void Init();
    private void OnEntityKill(BaseNetworkable entity);
    private bool CanHelicopterTarget(PatrolHelicopterAI heli, BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
    private object OnThreatLevelUpdate(BasePlayer player);
    private bool IsHostile(BasePlayer player);
    private void SetHostile(BasePlayer player, float duration);
    private void RemoveHostile(BasePlayer player);
    private void ClearHostiles();
}

public class NudistHeliSettings
{
    public int MaxClothingCount { get; set; }
    public float HostileTime { get; set; }
    public HashSet<string> RestrictedWeapons { get; set; }
    public bool OnlyEngageOnWeaponHeld { get; set; }
}


```

---

## NukeWeapons by k1lly0u - Creates nuclear ammo for a bunch of different ammo types, full GUI crafting menu and ammo gauge

```csharp
using Facepunch;
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;
using System.Linq;
using System.IO;
using Rust;

Oxide.Plugins
[Info("NukeWeapons", "k1lly0u", "0.1.69")]
[Description("Create nuclear ammo for a bunch of different ammo types, full GUI crafting menu and ammo gauge")]
 class NukeWeapons : RustPlugin
{
    [PluginReference]
     Plugin LustyMap;
     Plugin ImageLibrary;
    private NukeData _nukeData;
    private ItemNames _itemNames;
    private DynamicConfigFile _data;
    private DynamicConfigFile item_names;
    private string dataDirectory;
    private readonly List<ZoneList> _radiationZones;
    private readonly Hash<ulong, NukeType> _activeUsers;
    private Hash<ulong, Hash<NukeType, int>> _cachedAmmo;
    private readonly Hash<ulong, Hash<NukeType, double>> _craftingTimers;
    private readonly List<Timer> _timers;
    private Dictionary<string, ItemDefinition> _itemDefs;
    private Hash<string, string> _displayNames;
    private const int PlayerMask;
    private void Loaded();
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnEntityDeath(BasePlayer player, HitInfo hitInfo);
    private void OnRocketLaunched(BasePlayer player, BaseEntity entity);
    private void OnEntitySpawned(Landmine landmine);
    private void OnEntityDeath(Landmine landmine, HitInfo info);
    private void OnPlayerAttack(BasePlayer attacker, HitInfo info);
    private void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
    private bool HasEnoughResources(BasePlayer player, int itemid, int amount);
    private void TakeResources(BasePlayer player, int itemid, int amount);
    private double CurrentTime();
    private bool IsSelectedType(BasePlayer player, NukeType type);
    private T ParseType(string type);
    private void FindAllMines();
    private bool CanCraftWeapon(BasePlayer player, NukeType type);
    private bool IsCrafting(BasePlayer player, NukeType type);
    private string CraftTimeClock(BasePlayer player, double time);
    private void StartCrafting(BasePlayer player, NukeType type);
    private void FinishCraftingItems(BasePlayer player, NukeType type);
    private bool FinishedCrafting(BasePlayer player);
    private void CloseMap(BasePlayer player);
    private void OpenMap(BasePlayer player);
    private class NWUI
    {
        public static CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor, string parent);
        public static void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        public static void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align, float fadein);
        public static void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, float fadein);
        public static void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
        public static string CreateTextOverlay(CuiElementContainer container, string panelName, string textcolor, string text, int size, string distance, string olcolor, string aMin, string aMax, TextAnchor align);
    }

    private readonly Dictionary<string, string> _uiColors;
    private const string UIMain;
    private const string UIPanel;
    private const string UIEntry;
    private const string UIIcon;
    private void OpenCraftingMenu(BasePlayer player);
    private void CraftingElement(BasePlayer player, NukeType type);
    private void CreateCraftTimer(BasePlayer player);
    private void CreateIngredientEntry(CuiElementContainer container, string panel, string name, int amountreq, int plyrhas, int number);
    private void CreateAmmoIcons(BasePlayer player);
    private void AmmoIcon(BasePlayer player, NukeType type, int number);
    private void AddButtons(BasePlayer player, int number);
    private void CreateMenuButton(CuiElementContainer container, string panelName, string buttonname, string command, int number);
    [ConsoleCommand("NWUI_Craft")]
    private void cmdNWCraft(ConsoleSystem.Arg arg);
    [ConsoleCommand("NWUI_DeactivateMenu")]
    private void cmdNWDeActivate(ConsoleSystem.Arg arg);
    [ConsoleCommand("NWUI_DeactivateButton")]
    private void cmdNWDeActivateButton(ConsoleSystem.Arg arg);
    [ConsoleCommand("NWUI_DeactivateIcons")]
    private void cmdNWDeactivateIcons(ConsoleSystem.Arg arg);
    [ConsoleCommand("NWUI_OpenMenu")]
    private void cmdNWOpenMenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("NWUI_Activate")]
    private void cmdNWActivate(ConsoleSystem.Arg arg);
    [ConsoleCommand("NWUI_ChangeElement")]
    private void cmdNWChangeElement(ConsoleSystem.Arg arg);
    [ConsoleCommand("NWUI_DestroyAll")]
    private void cmdNWDestroyAll(ConsoleSystem.Arg arg);
    private void DestroyCraftUI(BasePlayer player);
    private void DestroyIconUI(BasePlayer player);
    private void InitializePlugin();
    private bool HasAmmo(ulong player, NukeType type);
    private Dictionary<string, int> GetCraftingComponents(NukeType type);
    private ConfigData.NWType GetConfigFromType(NukeType type);
    private void CheckPlayerEntry(BasePlayer player);
    private void InitializeZone(Vector3 location, float intensity, float duration, float radius, bool explosionType);
    private void DestroyZone(ZoneList zone);
    private class Nuke : MonoBehaviour
    {
        private NukeWeapons instance;
        public NukeType type;
        public ConfigData.NWType.RadiationStats stats;
        private void OnDestroy();
        public void InitializeComponent(NukeWeapons ins, NukeType typ, ConfigData.NWType.RadiationStats sta);
    }

    public class ZoneList
    {
        public RadiationZone zone;
        public Timer time;
    }

    internal class RadiationZone : MonoBehaviour
    {
        private void Awake();
        public void Activate(Vector3 pos, float radius, float amount);
    }

    [ChatCommand("nw")]
    private void cmdNukes(BasePlayer player, string command, string[] args);
    private bool HasPermission(BasePlayer player, NukeType type);
    private bool HasUnlimitedAmmo(BasePlayer player);
    private ConfigData configData;
    private class ConfigData
    {
        public NWType Mines { get; set; }
        public NWType Rockets { get; set; }
        public NWType Bullets { get; set; }
        public NWType Grenades { get; set; }
        public NWType Explosives { get; set; }
        public Option Options { get; set; }
        public Dictionary<string, string> URL_IconList { get; set; }
        public class NWType
        {
            public bool Enabled { get; set; }
            public int MaxAllowed { get; set; }
            public int CraftTime { get; set; }
            public int CraftAmount { get; set; }
            public Dictionary<string, int> CraftingCosts { get; set; }
            public RadiationStats RadiationProperties { get; set; }
            public class RadiationStats
            {
                public float Intensity { get; set; }
                public float Duration { get; set; }
                public float Radius { get; set; }
            }

        }

        public class Option
        {
            public string MSG_MainColor { get; set; }
            public string MSG_SecondaryColor { get; set; }
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private void SaveData();
    private void SaveDisplayNames();
    private void LoadData();
    private class NukeData
    {
        public Hash<ulong, Hash<NukeType, int>> ammo;
        public List<ulong> Mines;
    }

    private class ItemNames
    {
        public Hash<string, string> displayNames;
    }

    private class PlayerAmmo
    {
        public int Rockets;
        public int Mines;
        public int Bullets;
        public int Explosives;
        public int Grenades;
    }

    [ConsoleCommand("nukeicons")]
    private void cmdNukeIcons(ConsoleSystem.Arg arg);
    public void AddImage(string imageName, string fileName);
    private string GetImage(string name);
    private void SendMSG(BasePlayer player, string message, string message2);
    private string MSG(string key, string playerid);
    private Dictionary<string, string> Messages;
}

private class NWUI
{
    public static CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor, string parent);
    public static void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    public static void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align, float fadein);
    public static void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, float fadein);
    public static void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
    public static string CreateTextOverlay(CuiElementContainer container, string panelName, string textcolor, string text, int size, string distance, string olcolor, string aMin, string aMax, TextAnchor align);
}

private class Nuke : MonoBehaviour
{
    private NukeWeapons instance;
    public NukeType type;
    public ConfigData.NWType.RadiationStats stats;
    private void OnDestroy();
    public void InitializeComponent(NukeWeapons ins, NukeType typ, ConfigData.NWType.RadiationStats sta);
}

public class ZoneList
{
    public RadiationZone zone;
    public Timer time;
}

internal class RadiationZone : MonoBehaviour
{
    private void Awake();
    public void Activate(Vector3 pos, float radius, float amount);
}

private class ConfigData
{
    public NWType Mines { get; set; }
    public NWType Rockets { get; set; }
    public NWType Bullets { get; set; }
    public NWType Grenades { get; set; }
    public NWType Explosives { get; set; }
    public Option Options { get; set; }
    public Dictionary<string, string> URL_IconList { get; set; }
    public class NWType
    {
        public bool Enabled { get; set; }
        public int MaxAllowed { get; set; }
        public int CraftTime { get; set; }
        public int CraftAmount { get; set; }
        public Dictionary<string, int> CraftingCosts { get; set; }
        public RadiationStats RadiationProperties { get; set; }
        public class RadiationStats
        {
            public float Intensity { get; set; }
            public float Duration { get; set; }
            public float Radius { get; set; }
        }

    }

    public class Option
    {
        public string MSG_MainColor { get; set; }
        public string MSG_SecondaryColor { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class NWType
{
    public bool Enabled { get; set; }
    public int MaxAllowed { get; set; }
    public int CraftTime { get; set; }
    public int CraftAmount { get; set; }
    public Dictionary<string, int> CraftingCosts { get; set; }
    public RadiationStats RadiationProperties { get; set; }
    public class RadiationStats
    {
        public float Intensity { get; set; }
        public float Duration { get; set; }
        public float Radius { get; set; }
    }

}

public class RadiationStats
{
    public float Intensity { get; set; }
    public float Duration { get; set; }
    public float Radius { get; set; }
}

public class Option
{
    public string MSG_MainColor { get; set; }
    public string MSG_SecondaryColor { get; set; }
}

private class NukeData
{
    public Hash<ulong, Hash<NukeType, int>> ammo;
    public List<ulong> Mines;
}

private class ItemNames
{
    public Hash<string, string> displayNames;
}

private class PlayerAmmo
{
    public int Rockets;
    public int Mines;
    public int Bullets;
    public int Explosives;
    public int Grenades;
}


```

---

## NukeWipe by Razor - Wipe with style and explosion

```csharp
using System.Collections.Generic;
using UnityEngine;
using System.Linq;
using Facepunch;
using System;

Oxide.Plugins
[Info("NukeWipe", "LaserHydra/Razor", "1.0.1", ResourceId = 0)]
[Description("Wipe with style - and propably lag lol")]
 class NukeWipe : RustPlugin
{
     Type[] DestroyableTypes;
     void Loaded();
     void LoadConfig();
     void LoadMessages();
    protected override void LoadDefaultConfig();
    [ChatCommand("nuke")]
     void cmdNuke(BasePlayer player);
     void Nuke();
     void Explode(BasePlayer player);
     bool ShouldBeDestroyed(BaseEntity entity);
     List<BaseEntity> GetEntities(Vector3 position, int radius);
     List<BaseCorpse> GetCorps(Vector3 position, int radius);
     string ListToString(List<BaseEntity> list, int first, string seperator);
     List<T> CopyList(List<T> list);
     void SetConfig(object[] args);
     T GetConfig(T defaultVal, object[] args);
     void LoadData(T data, string filename);
     void SaveData(T data, string filename);
     string GetMsg(string key, object userID);
     void RegisterPerm(string[] permArray);
     bool HasPerm(object uid, string[] permArray);
     string PermissionPrefix { get; set; }
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}


```

---

## ObjectRemover by misticos - Removes furnaces, lanterns, campfires, buildings, etc. on command

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;

Oxide.Plugins
[Info("Object Remover", "Iv Misticos", "3.0.5")]
[Description("Removes furnaces, lanterns, campfires, buildings etc. on command")]
 class ObjectRemover : RustPlugin
{
    private const string ShortnameCupboard;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Object Command Permission")]
        public string PermissionObject;
        [JsonProperty(PropertyName = "Statistics Command Permission")]
        public string PermissionStatistics;
        [JsonProperty(PropertyName = "Object Command")]
        public string CommandObject;
        [JsonProperty(PropertyName = "Statistics Command")]
        public string CommandStatistics;
        public string Prefix;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private void CommandObject(IPlayer player, string command, string[] args);
    private void CommandStatistics(IPlayer player, string command, string[] args);
    private class RemoveOptions
    {
        public float Radius;
        public bool Count;
        public bool OutsideCupboard;
        public bool InsideCupboard;
        public void Parse(string[] args);
    }

    private List<BaseEntity> FindObjects(Vector3 startPos, string entity, RemoveOptions options);
    private bool IsNeededObject(BaseEntity entity, string shortname, bool isAll, RemoveOptions options);
    private string GetMsg(string key, string userId);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Object Command Permission")]
    public string PermissionObject;
    [JsonProperty(PropertyName = "Statistics Command Permission")]
    public string PermissionStatistics;
    [JsonProperty(PropertyName = "Object Command")]
    public string CommandObject;
    [JsonProperty(PropertyName = "Statistics Command")]
    public string CommandStatistics;
    public string Prefix;
}

private class RemoveOptions
{
    public float Radius;
    public bool Count;
    public bool OutsideCupboard;
    public bool InsideCupboard;
    public void Parse(string[] args);
}


```

---

## OfflineDoors by Slydelix - Automagically closes all doors owned by player when he disconnects

```csharp
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Offline Doors", "Slydelix", "1.1.2", ResourceId = 2782)]
 class OfflineDoors : RustPlugin
{
    [PluginReference]
     Plugin Clans;
    protected override void LoadDefaultMessages();
    private const string perm;
    private bool usePerms;
    private bool useClans;
    protected override void LoadDefaultConfig();
    private T GetConfig(string name, T defaultValue);
    private class StoredData
    {
        public Dictionary<ulong, bool> players;
        public StoredData();
    }

    private StoredData storedData;
    private void SaveData();
    private void OnUserConnected(IPlayer player);
    private void Init();
    private void Loaded();
    private void Unload();
    private void OnServerSave();
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private List<ulong> GetOnlineClanMembers(string clanName);
    private void ClosePlayerDoors(BasePlayer player);
    [ChatCommand("ofd")]
    private void offlinedoorscmd(BasePlayer player, string command, string[] args);
}

private class StoredData
{
    public Dictionary<ulong, bool> players;
    public StoredData();
}


```

---

## OfflineLadderBlock by Chepzz - Blocks players from placing ladders on your base if your offline

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using System.Linq;
using Oxide.Core.Plugins;
using UnityEngine;
using Facepunch;

Oxide.Plugins
[Info("Offline Ladder Block", "Chepzz", "1.0.1")]
[Description("Prevents ladders being placed if the entity owner is offline.")]
public class OfflineLadderBlock : RustPlugin
{
    const string bypassPerm;
     Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "OfflineLadderBlock Options")]
        public PluginOptions POptions;
    }

    public class PluginOptions
    {
        [JsonProperty(PropertyName = "Chat Icon (Steam64ID)")]
        public ulong chatIcon;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private new void LoadDefaultMessages();
    private void Loaded();
     object CanBuild(Planner planner, Construction prefab, Construction.Target target);
    private void PrintMsg(BasePlayer player, string message);
     string Lang(string key, string id, object[] args);
}

public class Configuration
{
    [JsonProperty(PropertyName = "OfflineLadderBlock Options")]
    public PluginOptions POptions;
}

public class PluginOptions
{
    [JsonProperty(PropertyName = "Chat Icon (Steam64ID)")]
    public ulong chatIcon;
}


```

---

## OfflineMail by redBDGR - Send messages to a player so they receive them on their next login

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Offline Mail", "redBDGR", "1.0.2")]
[Description("Send messages to a player so they receive them on their next login")]
public class OfflineMail : CovalencePlugin
{
    private DynamicConfigFile offlineMessageData;
     StoredData storedData;
     class StoredData
    {
        public Dictionary<string, MessageInfo> offlineMessageInformation;
    }

     bool Changed;
    public const string permissionName;
    public const string permissionNameADMIN;
    public string consoleName;
    public float waitTime;
     Dictionary<string, MessageInfo> playerMessages;
     class MessageInfo
    {
        public string senderName;
        public List<string> messages;
    }

     void SaveData();
     void LoadData();
     void Unload();
     void OnServerSave();
     void Init();
     void LoadVariables();
    protected override void LoadDefaultConfig();
    private new void LoadDefaultMessages();
     void OnUserConnected(IPlayer player);
    [Command("mail")]
     void sendomCMD(IPlayer player, string command, string[] args);
    [Command("clearmail")]
     void omclearCMD(IPlayer player, string command, string[] args);
     object GetConfig(string menu, string datavalue, object defaultValue);
     string msg(string key, string id);
}

 class StoredData
{
    public Dictionary<string, MessageInfo> offlineMessageInformation;
}

 class MessageInfo
{
    public string senderName;
    public List<string> messages;
}


```

---

## OilCrate by  - Brings back the pump jack to Rust.

```csharp
using Facepunch;
using System;
using System.Collections.Generic;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Oil Crate", "Default", "0.7.5")]
[Description("Brings the pump jack back into the game")]
 class OilCrate : RustPlugin
{
    private const string oilCrateAllow;
    private readonly List<Timer> timers;
    private readonly List<string> duplicate;
    private bool quarryLiquid;
    private bool pumpjackSolid;
    private bool effectStaticPumpjacks;
    private bool missingPermission;
    private float oilCrateChance;
    private float oilCrateDespawn;
    private bool dryOnly;
    private float oilGatherWorkNeededMin;
    private float oilGatherWorkNeededMax;
    private bool dropFreePumpjack;
    private bool craftViaPickaxe;
    private Dictionary<string, object> pumpJackResources;
    private Dictionary<string, object> pumpJackResourcesDefault();
    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void Init();
    private void OnResourceDepositCreated(ResourceDepositManager.ResourceDeposit resourceDeposit);
    private void OnSurveyGather(SurveyCharge survey, Item item);
    private void OnEntitySpawned(BaseNetworkable entity);
    private void OnPlayerAttack(BasePlayer attacker, HitInfo info);
    [ConsoleCommand("oilcrate")]
    private void ccmdOilCrate(ConsoleSystem.Arg arg);
    protected override void LoadDefaultMessages();
    [ChatCommand("oilcrate")]
    private void cmdOilCrate(BasePlayer player, string command, string[] args);
    private void RefreshQuarries(string todo);
    private void Spawn(string prefab, Vector3 position);
    private void DeSpawn(string prefab, Vector3 position, float time);
    private void GetLocation(Vector3 startPos, Vector3 pos, Quaternion rot);
    private T GetConfig(string name, string name2, T defaultValue);
    private bool IsAllowed(BasePlayer player, bool perm);
}


```

---

## OilRigDiscordNotify by VisEntities - Notifies when the hackable locked crate in both oil rigs gets activated

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Oil Rig Discord Notify", "Dana", "0.0.6")]
[Description("Notifies when the hackable locked crate in both Oil Rigs gets activated.")]
internal class OilRigDiscordNotify : RustPlugin
{
    private Configuration config;
    private Timer LoginCheck;
    const float calgon;
    public Dictionary<string, Vector3> GridInfo;
     Vector3 SmallOilRigPos;
     Vector3 BigOilRigPos;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Plugin - Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Discord - Webhook URL")]
        public string DiscordWebHookUrl { get; set; }
        [JsonProperty(PropertyName = "Discord - Mention Roles (Roles IDs)")]
        public List<string> MentionRoles { get; set; }
        [JsonProperty(PropertyName = "Discord - Message - Text")]
        public string MessageText { get; set; }
        [JsonProperty(PropertyName = "Discord - Embed - Image - URLs")]
        public List<string> EmbedImages { get; set; }
        [JsonProperty(PropertyName = "Discord - Embed - Image - Randomly Select (false = use the first, true = random)")]
        public bool SelectRandomImage { get; set; }
        [JsonProperty(PropertyName = "Discord - Embed - Color")]
        public int EmbedColor { get; set; }
        [JsonProperty(PropertyName = "Discord - Embed - Field 1 Title")]
        public string EmbedField1Title { get; set; }
        [JsonProperty(PropertyName = "Discord - Embed - Field 2 Title")]
        public string EmbedField2Title { get; set; }
    }

    private void Init();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
     Direction GetDirection(Vector3 location);
     bool isOnRig(Vector3 rigVector, Vector3 pos);
     void CanHackCrate(BasePlayer player, HackableLockedCrate crate);
     void AccountForRoles(bool bigRig, BasePlayer hacker);
     void SendToDiscordBot(bool bigRig, BasePlayer hacker, string mentions);
    private class WebHookEmbedBody
    {
        [JsonProperty(PropertyName = "embeds")]
        public WebHookEmbed[] Embeds;
    }

    private class WebHookContentBody
    {
        [JsonProperty(PropertyName = "content")]
        public string Content;
    }

    private class WebHookEmbed
    {
        [JsonProperty(PropertyName = "title")]
        public string Title;
        [JsonProperty(PropertyName = "type")]
        public string Type;
        [JsonProperty(PropertyName = "description")]
        public string Description;
        [JsonProperty(PropertyName = "color")]
        public int Color;
        [JsonProperty(PropertyName = "author")]
        public WebHookAuthor Author;
        [JsonProperty(PropertyName = "image")]
        public WebHookImage Image;
        [JsonProperty(PropertyName = "fields")]
        public List<WebHookField> Fields;
        [JsonProperty(PropertyName = "footer")]
        public WebHookFooter Footer;
    }

    private class WebHookAuthor
    {
        [JsonProperty(PropertyName = "name")]
        public string Name;
        [JsonProperty(PropertyName = "url")]
        public string AuthorUrl;
        [JsonProperty(PropertyName = "icon_url")]
        public string AuthorIconUrl;
    }

    private class WebHookImage
    {
        [JsonProperty(PropertyName = "proxy_url")]
        public string ProxyUrl;
        [JsonProperty(PropertyName = "url")]
        public string Url;
        [JsonProperty(PropertyName = "height")]
        public int? Height;
        [JsonProperty(PropertyName = "width")]
        public int? Width;
    }

    private class WebHookField
    {
        [JsonProperty(PropertyName = "name")]
        public string Name;
        [JsonProperty(PropertyName = "value")]
        public string Value;
        [JsonProperty(PropertyName = "inline")]
        public bool Inline;
    }

    private class WebHookFooter
    {
        [JsonProperty(PropertyName = "text")]
        public string Text;
        [JsonProperty(PropertyName = "icon_url")]
        public string IconUrl;
        [JsonProperty(PropertyName = "proxy_icon_url")]
        public string ProxyIconUrl;
    }

}

public class Configuration
{
    [JsonProperty(PropertyName = "Plugin - Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Discord - Webhook URL")]
    public string DiscordWebHookUrl { get; set; }
    [JsonProperty(PropertyName = "Discord - Mention Roles (Roles IDs)")]
    public List<string> MentionRoles { get; set; }
    [JsonProperty(PropertyName = "Discord - Message - Text")]
    public string MessageText { get; set; }
    [JsonProperty(PropertyName = "Discord - Embed - Image - URLs")]
    public List<string> EmbedImages { get; set; }
    [JsonProperty(PropertyName = "Discord - Embed - Image - Randomly Select (false = use the first, true = random)")]
    public bool SelectRandomImage { get; set; }
    [JsonProperty(PropertyName = "Discord - Embed - Color")]
    public int EmbedColor { get; set; }
    [JsonProperty(PropertyName = "Discord - Embed - Field 1 Title")]
    public string EmbedField1Title { get; set; }
    [JsonProperty(PropertyName = "Discord - Embed - Field 2 Title")]
    public string EmbedField2Title { get; set; }
}

private class WebHookEmbedBody
{
    [JsonProperty(PropertyName = "embeds")]
    public WebHookEmbed[] Embeds;
}

private class WebHookContentBody
{
    [JsonProperty(PropertyName = "content")]
    public string Content;
}

private class WebHookEmbed
{
    [JsonProperty(PropertyName = "title")]
    public string Title;
    [JsonProperty(PropertyName = "type")]
    public string Type;
    [JsonProperty(PropertyName = "description")]
    public string Description;
    [JsonProperty(PropertyName = "color")]
    public int Color;
    [JsonProperty(PropertyName = "author")]
    public WebHookAuthor Author;
    [JsonProperty(PropertyName = "image")]
    public WebHookImage Image;
    [JsonProperty(PropertyName = "fields")]
    public List<WebHookField> Fields;
    [JsonProperty(PropertyName = "footer")]
    public WebHookFooter Footer;
}

private class WebHookAuthor
{
    [JsonProperty(PropertyName = "name")]
    public string Name;
    [JsonProperty(PropertyName = "url")]
    public string AuthorUrl;
    [JsonProperty(PropertyName = "icon_url")]
    public string AuthorIconUrl;
}

private class WebHookImage
{
    [JsonProperty(PropertyName = "proxy_url")]
    public string ProxyUrl;
    [JsonProperty(PropertyName = "url")]
    public string Url;
    [JsonProperty(PropertyName = "height")]
    public int? Height;
    [JsonProperty(PropertyName = "width")]
    public int? Width;
}

private class WebHookField
{
    [JsonProperty(PropertyName = "name")]
    public string Name;
    [JsonProperty(PropertyName = "value")]
    public string Value;
    [JsonProperty(PropertyName = "inline")]
    public bool Inline;
}

private class WebHookFooter
{
    [JsonProperty(PropertyName = "text")]
    public string Text;
    [JsonProperty(PropertyName = "icon_url")]
    public string IconUrl;
    [JsonProperty(PropertyName = "proxy_icon_url")]
    public string ProxyIconUrl;
}


```

---

## OilRigDoorsFix by MONaH - Fix for glitch when doors on Oil Rigs are always open

```csharp
using System;
using System.Collections.Generic;
using Facepunch;

Oxide.Plugins
[Info("OilRigDoorsFix", "MON@H", "1.0.2")]
[Description("Fix for always open doors on Oil Rigs")]
public class OilRigDoorsFix : RustPlugin
{
    private uint _prefabIDCrate;
    private readonly uint[] _prefabIDs;
    private void Init();
    private void OnServerInitialized();
    private void OnEntitySpawned(HackableLockedCrate crate);
}


```

---

## OilRigWipeProtection by Strrobez - Block players from activating oil rig X amount of minutes after a map wipe

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Oil Rig Wipe Protection", "Strrobez", "1.0.2")]
[Description("Block people from activating oil rig X amount of minutes after a map wipe.")]
 class OilRigWipeProtection : RustPlugin
{
    private const string CratePrefab;
    private const string permEnabled;
    private const string permBypass;
    private DateTime _cachedWipeTime;
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void Init();
    private void OnServerInitialized();
    private object CanHackCrate(BasePlayer player, HackableLockedCrate crate);
}


```

---

## OldSchoolQuarries by S0N0FBISCUIT - Makes resource output from quarries better

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("OldSchoolQuarries", "S0N_0F_BISCUIT", "1.0.8")]
[Description("Makes resource output from quarries better")]
 class OldSchoolQuarries : RustPlugin
{
     class CustomItem
    {
        public string shortname;
        public int amount;
        public double chance;
        public bool valid;
    }

     class ConfigData
    {
        [JsonProperty(PropertyName = "Display Detailed Analysis")]
        public bool Detailed_Analysis;
        [JsonProperty(PropertyName = "Custom Items")]
        public List<CustomItem> Custom_Items;
    }

    static class StandardOutput
    {
        public static DepositEntry stone;
        public static DepositEntry hqm;
        public static DepositEntry metal;
        public static DepositEntry sulfur;
    }

     class DepositEntry
    {
        public OreType type;
        public int amount;
        public float workNeeded;
    }

     class Deposit
    {
        public Origin origin;
        public List<DepositEntry> entries;
    }

     class Origin
    {
        public float x;
        public float y;
        public float z;
        public Origin();
        public Origin(Vector3 vector);
    }

     class StoredData
    {
        public List<Deposit> changedDeposits;
    }

    private static readonly System.Random rng;
    private ConfigData config;
    private StoredData data;
    private new void LoadDefaultMessages();
    private static class Permissions
    {
        public static string probe;
        public static string customloot;
        public static string standardoutput;
    }

    private void Init();
     void OnServerInitialized();
    private void OnNewSave(string filename);
    protected override void LoadDefaultConfig();
    private void LoadConfigData();
    private void ValidateConfig();
    private void LoadData();
    private void SaveData();
    private void ClearData();
     void OnEntitySpawned(BaseNetworkable entity);
     void OnItemUse(Item item, int amountToUse);
    [ChatCommand("getdeposit")]
     void GetDeposit(BasePlayer player, string command, string[] args);
    [ConsoleCommand("getdeposit")]
     void GetDepositConsole(ConsoleSystem.Arg arg);
    [ChatCommand("osq.reloadconfig")]
     void ReloadConfig(BasePlayer player, string command, string[] args);
    [ConsoleCommand("osq.cleardata")]
     void ClearData(ConsoleSystem.Arg arg);
    private string Lang(string key, string userId, object[] args);
    private Vector3 GetVector3(Origin origin);
    private Origin GetOrigin(Vector3 vector);
}

 class CustomItem
{
    public string shortname;
    public int amount;
    public double chance;
    public bool valid;
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Display Detailed Analysis")]
    public bool Detailed_Analysis;
    [JsonProperty(PropertyName = "Custom Items")]
    public List<CustomItem> Custom_Items;
}

static class StandardOutput
{
    public static DepositEntry stone;
    public static DepositEntry hqm;
    public static DepositEntry metal;
    public static DepositEntry sulfur;
}

 class DepositEntry
{
    public OreType type;
    public int amount;
    public float workNeeded;
}

 class Deposit
{
    public Origin origin;
    public List<DepositEntry> entries;
}

 class Origin
{
    public float x;
    public float y;
    public float z;
    public Origin();
    public Origin(Vector3 vector);
}

 class StoredData
{
    public List<Deposit> changedDeposits;
}

private static class Permissions
{
    public static string probe;
    public static string customloot;
    public static string standardoutput;
}


```

---

## OnlinePlayers by MACHIN3 - Shows list of online players and sleepers in separate UIs

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Online Players", "MACHIN3", "1.1.8")]
[Description("Shows list of online players and sleepers in separate UIs")]
public class OnlinePlayers : RustPlugin
{
    [PluginReference]
    private readonly Plugin KillRecords;
    private readonly Plugin XPerience;
    private readonly Plugin NTeleportation;
    private readonly Plugin DiscordReport;
    private readonly Plugin ImageLibrary;
    private Configuration config;
    private Timer _playerdata;
    private const string OnlinePlayersUI;
    private const string OnlinePlayersUIInner;
    private const string OnlinePlayersUIPages;
    private const string SleeperPlayersUI;
    private const string SleeperPlayersUIInner;
    private const string SleeperPlayersUIPages;
    private const string OnlinePlayersHUD;
    private readonly Hash<ulong, int> _onlineUIPage;
    private readonly Hash<ulong, string> onlinePlayers;
    private readonly Hash<ulong, int> _sleeperPlayersUIPage;
    private readonly Hash<ulong, string> sleeperPlayers;
    private class Configuration
    {
        [JsonProperty("Hide Admins")]
        public bool HideAdmins;
        [JsonProperty("Show Online Player Count")]
        public bool ShowOnlineCount;
        [JsonProperty("Show Sleeper Count")]
        public bool ShowSleeperCount;
        [JsonProperty("Show Player Avatars (requires ImageLibrary and Store player avatars = true)")]
        public bool playeravatars;
        [JsonProperty("Show KillRecords Icon (Requires Kill Records Plugin)")]
        public bool ShowKRIcon;
        [JsonProperty("Show XPerience Icon (Requires XPerience Plugin)")]
        public bool ShowXPIcon;
        [JsonProperty("Show XPerience Rank Sig (Requires XPerience Plugin)")]
        public bool ShowRankSig;
        [JsonProperty("Show XPerience Level (Requires XPerience Plugin)")]
        public bool ShowLevel;
        [JsonProperty("Show TPR Icon (Requires NTeleportation Plugin and tpr permission)")]
        public bool ShowTPIcon;
        [JsonProperty("Show Discord Report Icon (Requires DiscordReport Plugin)")]
        public bool ShowDRIcon;
        [JsonProperty("UI Location (distance from left 0 - 0.80)")]
        public double UILeft;
        [JsonProperty("UI Location (distance from bottom 0.45 - 1.0)")]
        public double UITop;
        [JsonProperty("Chat Command (Online Players)")]
        public string chatplayers;
        [JsonProperty("Chat Command (Sleepers)")]
        public string chatsleepers;
        [JsonProperty("Show Online HUD")]
        public bool OnlineHUD;
        [JsonProperty("Online HUD Chat Command")]
        public string chathud;
        [JsonProperty("HUD Location From Left")]
        public double HUDLeft;
        [JsonProperty("HUD Location From Top")]
        public double HUDTop;
        [JsonProperty("HUD Width")]
        public double HUDWidth;
        [JsonProperty("HUD Height")]
        public double HUDHeight;
        [JsonProperty("Max Players On HUD")]
        public int HUDplayercount;
        [JsonProperty("HUD Transparency 0.0 - 1.0")]
        public double HUDTransparency;
        [JsonProperty("HUD Refresh Rate (seconds)")]
        public float HUDrefreshrate;
        [JsonProperty("HUD Font Size")]
        public int HUDfontsize;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private void Loaded();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private static BasePlayer FindPlayer(string playerid);
    private void Chatplayers(BasePlayer player, string command, string[] args);
    private void Chatsleepers(BasePlayer player, string command, string[] args);
    private void ChatHUD(BasePlayer player, string command, string[] args);
    [ConsoleCommand("op.playerprofiles")]
    private void Cmdopplayerprofiles(ConsoleSystem.Arg arg);
    [ConsoleCommand("op.sleeperprofiles")]
    private void Cmdopsleeperprofiles(ConsoleSystem.Arg arg);
    [ConsoleCommand("op.refreshplayers")]
    private void Cmdrefreshplayers(ConsoleSystem.Arg arg);
    [ConsoleCommand("op.refreshsleepers")]
    private void Cmdrefreshsleepers(ConsoleSystem.Arg arg);
    [ConsoleCommand("op.playerpages")]
    private void Cmdopplayerpages(ConsoleSystem.Arg arg);
    [ConsoleCommand("op.sleeperpages")]
    private void Cmdopsleeperpages(ConsoleSystem.Arg arg);
    [ConsoleCommand("op.nteleportation")]
    private void Cmdnteleportation(ConsoleSystem.Arg arg);
    private CuiPanel DefaultUIPanel(string anchorMin, string anchorMax, string color);
    private CuiLabel DefaultUILabel(string text, double i, float height, TextAnchor align, int fontSize, string xMin, string xMax, string color);
    private CuiButton DefaultUIButton(string command, double i, float rowHeight, int fontSize, string color, string content, string xMin, string xMax, TextAnchor align, string fcolor);
    private CuiElement DefaultUIImage(string parent, string image, double i, float imgheight, string xMin, string xMax);
    private CuiElement DefaultHUDUIImage(string parent, string image, double i, float imgheight, string xMin, string xMax);
    private CuiLabel DefaultHUDUILabel(string text, double i, float height, TextAnchor align, int fontSize, string xMin, string xMax, string color);
    private void OnlineHUD(BasePlayer player);
    private void PlayerOnlineUI(BasePlayer player);
    private void PlayerOnlineUIList(BasePlayer player, int from);
    private void PlayerOnlineUIPages(BasePlayer player, int next, int back, int from);
    private void HUDTimer(BasePlayer player, bool active);
    private void SleeperPlayerUI(BasePlayer player);
    private void SleeperPlayerUIList(BasePlayer player, int from);
    private void SleeperPlayerUIPages(BasePlayer player, int next, int back, int from);
    private void DestroyUi(BasePlayer player, string name);
    private void ClearPlayerUIs(BasePlayer player, bool hud);
    protected override void LoadDefaultMessages();
    private string OPLang(string key, string id, object[] args);
}

private class Configuration
{
    [JsonProperty("Hide Admins")]
    public bool HideAdmins;
    [JsonProperty("Show Online Player Count")]
    public bool ShowOnlineCount;
    [JsonProperty("Show Sleeper Count")]
    public bool ShowSleeperCount;
    [JsonProperty("Show Player Avatars (requires ImageLibrary and Store player avatars = true)")]
    public bool playeravatars;
    [JsonProperty("Show KillRecords Icon (Requires Kill Records Plugin)")]
    public bool ShowKRIcon;
    [JsonProperty("Show XPerience Icon (Requires XPerience Plugin)")]
    public bool ShowXPIcon;
    [JsonProperty("Show XPerience Rank Sig (Requires XPerience Plugin)")]
    public bool ShowRankSig;
    [JsonProperty("Show XPerience Level (Requires XPerience Plugin)")]
    public bool ShowLevel;
    [JsonProperty("Show TPR Icon (Requires NTeleportation Plugin and tpr permission)")]
    public bool ShowTPIcon;
    [JsonProperty("Show Discord Report Icon (Requires DiscordReport Plugin)")]
    public bool ShowDRIcon;
    [JsonProperty("UI Location (distance from left 0 - 0.80)")]
    public double UILeft;
    [JsonProperty("UI Location (distance from bottom 0.45 - 1.0)")]
    public double UITop;
    [JsonProperty("Chat Command (Online Players)")]
    public string chatplayers;
    [JsonProperty("Chat Command (Sleepers)")]
    public string chatsleepers;
    [JsonProperty("Show Online HUD")]
    public bool OnlineHUD;
    [JsonProperty("Online HUD Chat Command")]
    public string chathud;
    [JsonProperty("HUD Location From Left")]
    public double HUDLeft;
    [JsonProperty("HUD Location From Top")]
    public double HUDTop;
    [JsonProperty("HUD Width")]
    public double HUDWidth;
    [JsonProperty("HUD Height")]
    public double HUDHeight;
    [JsonProperty("Max Players On HUD")]
    public int HUDplayercount;
    [JsonProperty("HUD Transparency 0.0 - 1.0")]
    public double HUDTransparency;
    [JsonProperty("HUD Refresh Rate (seconds)")]
    public float HUDrefreshrate;
    [JsonProperty("HUD Font Size")]
    public int HUDfontsize;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## OnlineQuarries by  - Automatically disable players' quarries when offline

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Online Quarries", "mvrb/Arainrr", "1.2.8", ResourceId = 2216)]
[Description("Automatically disable players' quarries when offline")]
public class OnlineQuarries : RustPlugin
{
    [PluginReference]
    private Plugin Friends;
    private Plugin Clans;
    private readonly Dictionary<ulong, Timer> _stopEngineTimer;
    private readonly HashSet<MiningQuarry> _miningQuarries;
    private void Init();
    private void OnServerInitialized();
    private void OnEntitySpawned(MiningQuarry miningQuarry);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private object OnEntityDistanceCheck(EngineSwitch engineSwitch, BasePlayer player, uint id, string debugName, float maximumDistance);
    private void CheckQuarries(BasePlayer player, bool isOn);
    private bool AnyOnlineFriends(ulong playerId);
    private bool AreFriends(ulong playerID, ulong friendID);
    private bool SameTeam(ulong playerID, ulong friendID);
    private bool HasFriend(ulong playerID, ulong friendID);
    private bool SameClan(ulong playerID, ulong friendID);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Use team")]
        public readonly bool useTeam;
        [JsonProperty(PropertyName = "Use clans")]
        public readonly bool useClans;
        [JsonProperty(PropertyName = "Use friends")]
        public readonly bool useFriends;
        [JsonProperty(PropertyName = "Prevent other players from turning the quarry on or off")]
        public readonly bool preventOther;
        [JsonProperty(PropertyName = "Automatically disable the delay of quarry (seconds)")]
        public readonly float offlineTime;
        [JsonProperty(PropertyName = "Quarry automatically starts after players are online")]
        public readonly bool autoStart;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Use team")]
    public readonly bool useTeam;
    [JsonProperty(PropertyName = "Use clans")]
    public readonly bool useClans;
    [JsonProperty(PropertyName = "Use friends")]
    public readonly bool useFriends;
    [JsonProperty(PropertyName = "Prevent other players from turning the quarry on or off")]
    public readonly bool preventOther;
    [JsonProperty(PropertyName = "Automatically disable the delay of quarry (seconds)")]
    public readonly float offlineTime;
    [JsonProperty(PropertyName = "Quarry automatically starts after players are online")]
    public readonly bool autoStart;
}


```

---

## OnScreenLogo by misticos - Allows you to put your own logo on the players screen

```csharp
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections;
using System.IO;
using UnityEngine;

Oxide.Plugins
[Info("OnScreenLogo", "Vlad-00003", "1.1.5", ResourceId = 2601)]
[Description("Displays the button on the player screen.")]
 class OnScreenLogo : RustPlugin
{
    private string PanelName;
    private string Image;
    static OnScreenLogo instance;
    private string Amax;
    private string Amin;
    private string ImageAddress;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void LoadData();
    private void SaveData();
     void OnServerInitialized();
     void Unload();
    [ConsoleCommand("osl.refresh")]
    private void cmdRedownload(ConsoleSystem.Arg arg);
    private void DownloadImage();
     IEnumerator DownloadImage(string url);
     void OnPlayerSleepEnded(BasePlayer player);
    private void CreateButton(BasePlayer player);
    private void GetConfig(string Key, T var);
}


```

---

## OptimalBurn by Thisha - Furnace splitter according to "The Ultimate Furnace Guide".

```csharp
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Optimal Burn", "Thisha", "0.9.6")]
[Description("Splitter according to ultimate furnace guide")]
public class OptimalBurn : RustPlugin
{
    private const string permUse;
    private const int sulfurOre;
    private const int metalOre;
    private const int hqOre;
    private const int wood;
    private const string largefurnace;
    private const string smallfurnace;
    private const int sulfurLarge1K;
    private const int sulfurLarge2K;
    private const int sulfurLarge2HK;
    private const int hqmLarge1K;
    private const int metalLarge1K;
    private const int metalLarge2K;
    private const int metalLarge3K;
    private const int metalLarge4K;
    private const int metalLarge5K;
    private const int metalSmall;
    private const int sulfurSmall;
    private const int hqmSmallLow;
    private const int hqmSmallHigh;
    private List<Item> items;
    private readonly Dictionary<ulong, string> openUis;
    private Dictionary<ulong, PlayerData> playerData;
    private class PlayerData
    {
        public int sulfurQtyFurnace;
        public int metalQtyFurnace;
        public int hqQtyFurnace;
        public int woodQtyFurnace;
        public int oreQtyFurnace;
        public int sulfurQtyPlayer;
        public int metalQtyPlayer;
        public int hqQtyPlayer;
        public int oreQtyPlayer;
        public int woodQQtyPlayer;
        public int ore;
        public int oreQty;
        public int woodQty;
        public int woodToAdd;
        public int oreToAdd;
        public bool divideit;
        public bool autoSplit;
        public furnaceType currFurnace;
    }

    private new void LoadDefaultMessages();
    private void Init();
    private void Unload();
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private void OnLootEntityEnd(BasePlayer player, BaseCombatEntity entity);
    private void OnEntityKill(BaseNetworkable networkable);
     object CanPickupEntity(BasePlayer player, BaseEntity entity);
    [ConsoleCommand("optimalburn.split")]
    private void OptimizeLargefurnace(ConsoleSystem.Arg arg);
     bool HasInvalidItems(BaseOven furnace);
     bool BurnableDefined(BasePlayer player);
     void DefineQuantitesToUseLarge(BasePlayer player);
     void DefineQuantitesToUseSmall(BasePlayer player);
     void Optimizefurnace(BasePlayer player, BaseOven furnace);
     void Handleremainings(BasePlayer player);
     void ReturnUnstackedToPlayer(BasePlayer player, int itemID, int Qty);
     void Filllargefurnace(BaseOven furnace, BasePlayer player);
     void Fillsmallfurnace(BaseOven furnace, BasePlayer player);
    private CuiElementContainer CreateUi(BasePlayer player, BaseOven oven);
    private void DestroyUI(BasePlayer player);
    private void DestroyOvenUI(BaseOven oven);
    private bool HasPermission(BasePlayer player);
    private string Lang(string key, string userId, object[] args);
}

private class PlayerData
{
    public int sulfurQtyFurnace;
    public int metalQtyFurnace;
    public int hqQtyFurnace;
    public int woodQtyFurnace;
    public int oreQtyFurnace;
    public int sulfurQtyPlayer;
    public int metalQtyPlayer;
    public int hqQtyPlayer;
    public int oreQtyPlayer;
    public int woodQQtyPlayer;
    public int ore;
    public int oreQty;
    public int woodQty;
    public int woodToAdd;
    public int oreToAdd;
    public bool divideit;
    public bool autoSplit;
    public furnaceType currFurnace;
}


```

---

## OrangeLift by  - Allows you to stop lifts at any level

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Orange Lift", "Orange", "1.0.2")]
[Description("Allows you to stop lifts at any level")]
public class OrangeLift : RustPlugin
{
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private object OnLiftUse(ProceduralLift entity, BasePlayer player);
    private void OnEntitySpawned(ProceduralLift entity);
    private void cmdControlChat(BasePlayer player, string commands, string[] args);
    private void cmdControlConsole(ConsoleSystem.Arg arg);
    private void LoadLifts();
    private void DestroyLifts();
    private static ProceduralLift GetClosestLift(BasePlayer player);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Lifts")]
        public List<LiftDefinition> lifts;
    }

    private class LiftDefinition
    {
        [JsonProperty(PropertyName = "Name")]
        public string name;
        [JsonProperty(PropertyName = "Position")]
        public Vector3 position;
        [JsonProperty(PropertyName = "Return delay time")]
        public float returnDelay;
        [JsonProperty(PropertyName = "Floors")]
        public List<float> floors;
        public bool Match(BaseEntity entity);
    }

    protected override void LoadConfig();
    private void ValidateConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private Dictionary<object, string> langMessages;
    protected override void LoadDefaultMessages();
    private string GetMessage(MessageType key, string playerID, object[] args);
    private static Dictionary<string, object> OrganizeArgs(object[] args);
    private static string ReplaceArgs(string message, Dictionary<string, object> args);
    private void SendMessage(object receiver, MessageType key, object[] args);
    private void SendMessage(object receiver, string message);
    private class LiftExtended : MonoBehaviour
    {
        public ProceduralLift entity;
        public int lastIndex;
        public Vector3 movePosition;
        public LiftDefinition definition;
        public int maxFloors { get; set; }
        public BasePlayer lastPassenger;
        public bool paused;
        private void Awake();
        private void OnDestroy();
        public void SetLastPassenger(BasePlayer player);
        public void MoveToNextFloor();
        public void SelectFloor(int floor);
        public Vector3 GetFloor(int index);
        public void MoveToStart();
        public void Update();
        private void OnStartedMoving();
        private void OnFinishedMoving();
    }

}

private class ConfigData
{
    [JsonProperty(PropertyName = "Lifts")]
    public List<LiftDefinition> lifts;
}

private class LiftDefinition
{
    [JsonProperty(PropertyName = "Name")]
    public string name;
    [JsonProperty(PropertyName = "Position")]
    public Vector3 position;
    [JsonProperty(PropertyName = "Return delay time")]
    public float returnDelay;
    [JsonProperty(PropertyName = "Floors")]
    public List<float> floors;
    public bool Match(BaseEntity entity);
}

private class LiftExtended : MonoBehaviour
{
    public ProceduralLift entity;
    public int lastIndex;
    public Vector3 movePosition;
    public LiftDefinition definition;
    public int maxFloors { get; set; }
    public BasePlayer lastPassenger;
    public bool paused;
    private void Awake();
    private void OnDestroy();
    public void SetLastPassenger(BasePlayer player);
    public void MoveToNextFloor();
    public void SelectFloor(int floor);
    public Vector3 GetFloor(int index);
    public void MoveToStart();
    public void Update();
    private void OnStartedMoving();
    private void OnFinishedMoving();
}


```

---

## OreBonusRadiusModifier by Ryz0r - Ore Bonus Radius Modifier allows you to modify the radius of the hot spot (bonus) to make it easier.

```csharp
using System;
using System.Linq;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Ore Bonus Radius Modifier", "Ryz0r", "1.0.1")]
[Description("Allows you to modify the size of an Ore Bonus (hotspot) radius")]
public class OreBonusRadiusModifier : RustPlugin
{
    private static Configuration _config;
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private class Configuration
    {
        [JsonProperty(PropertyName = "Bonus Radius (Default 0.15)")]
        public float BonusRadius;
    }

    protected override void LoadConfig();
    private void OnEntitySpawned(OreHotSpot spot);
    private void OnServerInitialized();
    private void Unload();
}

private class Configuration
{
    [JsonProperty(PropertyName = "Bonus Radius (Default 0.15)")]
    public float BonusRadius;
}


```

---

## OtherBuildingHealth by Judess69er - Configurable health multiplier for many different building blocks that are missed in BuildingHeath

```csharp
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System;
using System.Linq;

Oxide.Plugins
[Info("Other Building Health", "Judess69er", "1.6.3")]
[Description("Multiply the health of other building materials")]
 class OtherBuildingHealth : RustPlugin
{
    [PluginReference]
     Plugin RaidableBases;
    private const uint HIGH_EXTERNAL_ICE_WALL;
    private const uint DISCO_FLOOR;
    private const uint DISCO_FLOOR_LARGE;
    private ConfigData config;
    private class ConfigData
    {
        [JsonProperty("Health Multiplier")]
        public float Multiplier { get; set; }
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void Init();
    private void OnServerInitialized(bool isStartup);
    private void Unload();
    private void OnEntitySpawned(Door door);
    private void OnEntitySpawned(SimpleBuildingBlock block);
    private void OnEntitySpawned(Barricade barricade);
    private void OnEntitySpawned(StorageContainer container);
    private void OnEntitySpawned(IceFence fence);
    private bool isViable(BaseEntity entity);
}

private class ConfigData
{
    [JsonProperty("Health Multiplier")]
    public float Multiplier { get; set; }
}


```

---

## OurServers by rostov114 - Shows players information on your other server(s) with name, description, current player count, etc.

```csharp
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Our Servers", "Hougan / rostov114", "0.0.8")]
[Description("Show information about other servers (with fetching current online)")]
public class OurServers : RustPlugin
{
    private class Server
    {
        [JsonProperty("Name of your server in interface")]
        public string DisplayName;
        [JsonProperty("Description of your server in interface")]
        public string Description;
        [JsonProperty("IP:Port of server (ex: 127.0.0.1:12000)")]
        public string IPAndPort;
        [JsonProperty("Interface width (increase it, if your description is very length)")]
        public int ServerWidth;
        [JsonIgnore]
        public int CurrentOnline;
        [JsonIgnore]
        public int MaxOnline;
        [JsonIgnore]
        public bool Status;
        public string IP();
        public int Port();
        public int GetOnline();
        public void UpdateStatus(bool status);
    }

    private class Configuration
    {
        [JsonProperty("Servers configure")]
        public List<Server> Servers;
        [JsonProperty("Command to open interface")]
        public string Command;
        [JsonProperty("Use chat instead of UI")]
        public bool UseChat;
        [JsonProperty("Information update interval in seconds (should be > 30)")]
        public int Interval;
        [JsonProperty("Which API to use? (Possible options: steampowered or battlemetrics) (Default: battlemetrics)")]
        public string API;
        [JsonProperty("Steampowered API key")]
        public string SteamWebApiKey;
        public static Configuration Generate();
    }

    public class BattlemetricsApiResponse
    {
        [JsonProperty("data")]
        public List<ServerData> data;
        public class ServerData
        {
            [JsonProperty("id")]
            public string id;
            [JsonProperty("type")]
            public string type;
            [JsonProperty("attributes")]
            public Attributes attributes;
            public class Attributes
            {
                [JsonProperty("id")]
                public string id;
                [JsonProperty("name")]
                public string name;
                [JsonProperty("ip")]
                public string ip;
                [JsonProperty("port")]
                public int port;
                [JsonProperty("portQuery")]
                public int portQuery;
                [JsonProperty("players")]
                public int players;
                [JsonProperty("maxPlayers")]
                public int maxPlayers;
                [JsonProperty("status")]
                public string status;
            }

        }

    }

    public class SteampoweredApi
    {
        [JsonProperty("response")]
        public Response response;
        public class Response
        {
            [JsonProperty("servers")]
            public List<ServerData> servers;
            public class ServerData
            {
                [JsonProperty("addr")]
                public string addr;
                [JsonProperty("gameport")]
                public int gameport;
                [JsonProperty("steamid")]
                public string steamid;
                [JsonProperty("name")]
                public string name;
                [JsonProperty("appid")]
                public int appid;
                [JsonProperty("gamedir")]
                public string gamedir;
                [JsonProperty("version")]
                public string version;
                [JsonProperty("product")]
                public string product;
                [JsonProperty("region")]
                public int region;
                [JsonProperty("players")]
                public int players;
                [JsonProperty("max_players")]
                public int max_players;
                [JsonProperty("bots")]
                public int bots;
                [JsonProperty("map")]
                public string map;
                [JsonProperty("secure")]
                public bool secure;
                [JsonProperty("dedicated")]
                public bool dedicated;
                [JsonProperty("os")]
                public string os;
                [JsonProperty("gametype")]
                public string gametype;
            }

        }

    }

    private Coroutine UpdateAction;
    private Configuration Settings;
    private bool Initialized;
    private bool Broken;
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    private void Unload();
    private IEnumerator UpdateOnline();
    [ConsoleCommand("UI_OurServersHandler")]
    private void CmdConsoleHandler(ConsoleSystem.Arg args);
    private void CmdShowServers(BasePlayer player, string command, string[] args);
    private const string Layer;
    private void UI_DrawInterface(BasePlayer player, int index);
}

private class Server
{
    [JsonProperty("Name of your server in interface")]
    public string DisplayName;
    [JsonProperty("Description of your server in interface")]
    public string Description;
    [JsonProperty("IP:Port of server (ex: 127.0.0.1:12000)")]
    public string IPAndPort;
    [JsonProperty("Interface width (increase it, if your description is very length)")]
    public int ServerWidth;
    [JsonIgnore]
    public int CurrentOnline;
    [JsonIgnore]
    public int MaxOnline;
    [JsonIgnore]
    public bool Status;
    public string IP();
    public int Port();
    public int GetOnline();
    public void UpdateStatus(bool status);
}

private class Configuration
{
    [JsonProperty("Servers configure")]
    public List<Server> Servers;
    [JsonProperty("Command to open interface")]
    public string Command;
    [JsonProperty("Use chat instead of UI")]
    public bool UseChat;
    [JsonProperty("Information update interval in seconds (should be > 30)")]
    public int Interval;
    [JsonProperty("Which API to use? (Possible options: steampowered or battlemetrics) (Default: battlemetrics)")]
    public string API;
    [JsonProperty("Steampowered API key")]
    public string SteamWebApiKey;
    public static Configuration Generate();
}

public class BattlemetricsApiResponse
{
    [JsonProperty("data")]
    public List<ServerData> data;
    public class ServerData
    {
        [JsonProperty("id")]
        public string id;
        [JsonProperty("type")]
        public string type;
        [JsonProperty("attributes")]
        public Attributes attributes;
        public class Attributes
        {
            [JsonProperty("id")]
            public string id;
            [JsonProperty("name")]
            public string name;
            [JsonProperty("ip")]
            public string ip;
            [JsonProperty("port")]
            public int port;
            [JsonProperty("portQuery")]
            public int portQuery;
            [JsonProperty("players")]
            public int players;
            [JsonProperty("maxPlayers")]
            public int maxPlayers;
            [JsonProperty("status")]
            public string status;
        }

    }

}

public class ServerData
{
    [JsonProperty("id")]
    public string id;
    [JsonProperty("type")]
    public string type;
    [JsonProperty("attributes")]
    public Attributes attributes;
    public class Attributes
    {
        [JsonProperty("id")]
        public string id;
        [JsonProperty("name")]
        public string name;
        [JsonProperty("ip")]
        public string ip;
        [JsonProperty("port")]
        public int port;
        [JsonProperty("portQuery")]
        public int portQuery;
        [JsonProperty("players")]
        public int players;
        [JsonProperty("maxPlayers")]
        public int maxPlayers;
        [JsonProperty("status")]
        public string status;
    }

}

public class Attributes
{
    [JsonProperty("id")]
    public string id;
    [JsonProperty("name")]
    public string name;
    [JsonProperty("ip")]
    public string ip;
    [JsonProperty("port")]
    public int port;
    [JsonProperty("portQuery")]
    public int portQuery;
    [JsonProperty("players")]
    public int players;
    [JsonProperty("maxPlayers")]
    public int maxPlayers;
    [JsonProperty("status")]
    public string status;
}

public class SteampoweredApi
{
    [JsonProperty("response")]
    public Response response;
    public class Response
    {
        [JsonProperty("servers")]
        public List<ServerData> servers;
        public class ServerData
        {
            [JsonProperty("addr")]
            public string addr;
            [JsonProperty("gameport")]
            public int gameport;
            [JsonProperty("steamid")]
            public string steamid;
            [JsonProperty("name")]
            public string name;
            [JsonProperty("appid")]
            public int appid;
            [JsonProperty("gamedir")]
            public string gamedir;
            [JsonProperty("version")]
            public string version;
            [JsonProperty("product")]
            public string product;
            [JsonProperty("region")]
            public int region;
            [JsonProperty("players")]
            public int players;
            [JsonProperty("max_players")]
            public int max_players;
            [JsonProperty("bots")]
            public int bots;
            [JsonProperty("map")]
            public string map;
            [JsonProperty("secure")]
            public bool secure;
            [JsonProperty("dedicated")]
            public bool dedicated;
            [JsonProperty("os")]
            public string os;
            [JsonProperty("gametype")]
            public string gametype;
        }

    }

}

public class Response
{
    [JsonProperty("servers")]
    public List<ServerData> servers;
    public class ServerData
    {
        [JsonProperty("addr")]
        public string addr;
        [JsonProperty("gameport")]
        public int gameport;
        [JsonProperty("steamid")]
        public string steamid;
        [JsonProperty("name")]
        public string name;
        [JsonProperty("appid")]
        public int appid;
        [JsonProperty("gamedir")]
        public string gamedir;
        [JsonProperty("version")]
        public string version;
        [JsonProperty("product")]
        public string product;
        [JsonProperty("region")]
        public int region;
        [JsonProperty("players")]
        public int players;
        [JsonProperty("max_players")]
        public int max_players;
        [JsonProperty("bots")]
        public int bots;
        [JsonProperty("map")]
        public string map;
        [JsonProperty("secure")]
        public bool secure;
        [JsonProperty("dedicated")]
        public bool dedicated;
        [JsonProperty("os")]
        public string os;
        [JsonProperty("gametype")]
        public string gametype;
    }

}

public class ServerData
{
    [JsonProperty("addr")]
    public string addr;
    [JsonProperty("gameport")]
    public int gameport;
    [JsonProperty("steamid")]
    public string steamid;
    [JsonProperty("name")]
    public string name;
    [JsonProperty("appid")]
    public int appid;
    [JsonProperty("gamedir")]
    public string gamedir;
    [JsonProperty("version")]
    public string version;
    [JsonProperty("product")]
    public string product;
    [JsonProperty("region")]
    public int region;
    [JsonProperty("players")]
    public int players;
    [JsonProperty("max_players")]
    public int max_players;
    [JsonProperty("bots")]
    public int bots;
    [JsonProperty("map")]
    public string map;
    [JsonProperty("secure")]
    public bool secure;
    [JsonProperty("dedicated")]
    public bool dedicated;
    [JsonProperty("os")]
    public string os;
    [JsonProperty("gametype")]
    public string gametype;
}


```

---

## OwnCasino by NooBlet - Create your own BigWheel Event

```csharp
using Oxide.Core.Libraries.Covalence;
using UnityEngine;
using System.Collections.Generic;
using Physics = UnityEngine.Physics;

Oxide.Plugins
[Info("Own Casino", "NooBlet", "1.0.6")]
[Description("Make your own Casino Free version")]
public class OwnCasino : CovalencePlugin
{
    private const ulong skinIDwheel;
    private const ulong skinIDterminal;
    private const string wheelprefab;
    private const string terminalprefab;
    private const string chairprefab;
    private static OwnCasino plugin;
    static List<string> effects;
    private void OnEntityBuilt(Planner plan, GameObject go);
    private void RunChecks(BaseEntity baseEntity);
    private void OnHammerHit(BasePlayer player, HitInfo info);
    private void Loaded();
    private void CheckchildClass();
    private void CheckDeployWheel(BaseEntity entity);
    private bool IsWheel(ulong skin);
    private void SpawnWheel(Vector3 position, Quaternion rotation, ulong ownerID);
    private void CheckHit(BasePlayer player, BaseEntity entity);
    private void CheckDeployTermanal(BaseEntity entity);
    private bool IsTermanal(ulong skin);
    private void SpawnTerminal(Vector3 position, Quaternion rotation, ulong ownerID);
    [Command("casino")]
    private void casinoCommand(IPlayer iplayer, string command, string[] args);
    private void GiveItems(BasePlayer player);
    private Item[] CreateItems();
    private class WheelComponent : MonoBehaviour
    {
        private BigWheelGame wheel;
        private void Awake();
        private void CheckGround();
         bool isGroundMissing(BaseEntity entity);
        private void GroundMissing();
        public void TryPickup(BasePlayer player);
        public void DoDestroy();
    }

    private string GetLang(string key, string id);
    protected override void LoadDefaultMessages();
}

private class WheelComponent : MonoBehaviour
{
    private BigWheelGame wheel;
    private void Awake();
    private void CheckGround();
     bool isGroundMissing(BaseEntity entity);
    private void GroundMissing();
    public void TryPickup(BasePlayer player);
    public void DoDestroy();
}


```

---

## Parachute by Rustoholics - Gives players a wearable parachute to prevent fall damage

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("Parachute", "Rustoholics", "0.2.2")]
[Description("Give players a parachute")]
public class Parachute : CovalencePlugin
{
    [PluginReference]
    private Plugin Chute;
    private double _fallSpeed;
    private float _fallTimer;
    private int _coolDown;
    private float _packCheckTimer;
    private string _backpackPrefab;
    private Dictionary<string, Timer> _fallTimers;
    private Configuration _config;
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private class Configuration
    {
        public bool ShowBackpack;
        public ulong ParachuteSkinId;
        public string ParachuteName;
        public string ParachuteShortname;
    }

    protected override void LoadConfig();
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void WearSetupAndTimer(BasePlayer player);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnItemAddedToContainer(ItemContainer container, Item item);
    private void OnItemRemovedFromContainer(ItemContainer container, Item item);
    private void MonitorPlayerFall(BasePlayer player);
    private bool IsWearingParachute(BasePlayer player);
    private bool ItemIsParachute(Item item);
    private void WearParachutePack(BasePlayer player);
    private void UnwearParachutePack(BasePlayer player);
    private bool? OnEntityKill(BaseNetworkable entity);
    private void DoPackWearing(BasePlayer player, Item item);
    private void GiveParachute(BasePlayer player);
    [Command("parachute.give"), Permission("parachute.admin")]
    private void GivePackCommand(IPlayer iplayer, string command, string[] args);
}

private class Configuration
{
    public bool ShowBackpack;
    public ulong ParachuteSkinId;
    public string ParachuteName;
    public string ParachuteShortname;
}


```

---

## ParentedEntityRenderFix by WhiteThunder - Fixes issue where some parented entities do not render except near map origin

```csharp
using Network;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Serialization;
using Oxide.Core;
using System.Collections.Generic;
using System.IO;
using System.Linq;

Oxide.Plugins
[Info("Parented Entity Render Fix", "WhiteThunder", "0.1.2")]
[Description("Fixes bug where some parented entities do not render except near map origin.")]
internal class ParentedEntityRenderFix : CovalencePlugin
{
    private Configuration _pluginConfig;
    private void Unload();
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnEntityKill(BaseEntity entity);
    private void OnEntitySnapshot(BaseEntity entity, Connection connection);
    private void OnNetworkGroupLeft(BasePlayer player, Network.Visibility.Group group);
    private abstract class BaseNetworkCacheManager
    {
        private readonly Dictionary<NetworkableId, MemoryStream> _networkCache;
        public void Clear();
        public void InvalidateForEntity(NetworkableId entityId);
        public void SendModifiedSnapshot(BaseEntity entity, Connection connection);
        private void ToStream(BaseEntity entity, Stream stream, BaseNetworkable.SaveInfo saveInfo);
        private Stream ToStreamForNetwork(BaseEntity entity, Stream stream, BaseNetworkable.SaveInfo saveInfo);
        protected abstract void HandleOnEntitySaved(BaseEntity entity, BaseNetworkable.SaveInfo saveInfo);
    }

    private class NetworkCacheManager : BaseNetworkCacheManager
    {
        private static NetworkCacheManager _instance;
        public static NetworkCacheManager Instance { get; set; }
        protected override void HandleOnEntitySaved(BaseEntity entity, BaseNetworkable.SaveInfo saveInfo);
    }

    private class EntitySubscriptionManager
    {
        private static EntitySubscriptionManager _instance;
        public static EntitySubscriptionManager Instance { get; set; }
        private readonly Dictionary<NetworkableId, HashSet<ulong>> _entitySubscibers;
        public void Clear();
        public bool AddEntitySubscription(NetworkableId entityId, ulong userId);
        public void RemoveEntitySubscription(NetworkableId entityId, ulong userId);
        public void RemoveSubscriber(ulong userId);
        public void RemoveEntity(NetworkableId entityId);
    }

    private class Configuration : SerializableConfiguration
    {
        [JsonProperty("EnabledEntities")]
        public HashSet<string> EnabledEntities;
    }

    private Configuration GetDefaultConfig();
    private class SerializableConfiguration
    {
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    private static class JsonHelper
    {
        public static object Deserialize(string json);
        private static object ToObject(JToken token);
    }

    private bool MaybeUpdateConfig(SerializableConfiguration config);
    private bool MaybeUpdateConfigDict(Dictionary<string, object> currentWithDefaults, Dictionary<string, object> currentRaw);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
}

private abstract class BaseNetworkCacheManager
{
    private readonly Dictionary<NetworkableId, MemoryStream> _networkCache;
    public void Clear();
    public void InvalidateForEntity(NetworkableId entityId);
    public void SendModifiedSnapshot(BaseEntity entity, Connection connection);
    private void ToStream(BaseEntity entity, Stream stream, BaseNetworkable.SaveInfo saveInfo);
    private Stream ToStreamForNetwork(BaseEntity entity, Stream stream, BaseNetworkable.SaveInfo saveInfo);
    protected abstract void HandleOnEntitySaved(BaseEntity entity, BaseNetworkable.SaveInfo saveInfo);
}

private class NetworkCacheManager : BaseNetworkCacheManager
{
    private static NetworkCacheManager _instance;
    public static NetworkCacheManager Instance { get; set; }
    protected override void HandleOnEntitySaved(BaseEntity entity, BaseNetworkable.SaveInfo saveInfo);
}

private class EntitySubscriptionManager
{
    private static EntitySubscriptionManager _instance;
    public static EntitySubscriptionManager Instance { get; set; }
    private readonly Dictionary<NetworkableId, HashSet<ulong>> _entitySubscibers;
    public void Clear();
    public bool AddEntitySubscription(NetworkableId entityId, ulong userId);
    public void RemoveEntitySubscription(NetworkableId entityId, ulong userId);
    public void RemoveSubscriber(ulong userId);
    public void RemoveEntity(NetworkableId entityId);
}

private class Configuration : SerializableConfiguration
{
    [JsonProperty("EnabledEntities")]
    public HashSet<string> EnabledEntities;
}

private class SerializableConfiguration
{
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private static class JsonHelper
{
    public static object Deserialize(string json);
    private static object ToObject(JToken token);
}


```

---

## Password by MrBlue - Provides name and chat command password protection for the server

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Password", "Wulf", "3.0.2")]
[Description("Provides name and chat command password protection for the server")]
 class Password : CovalencePlugin
{
    private Configuration config;
     class Configuration
    {
        [JsonProperty("Server password")]
        public string ServerPassword;
        [JsonProperty("Maxium password attempts")]
        public int MaxAttempts;
        [JsonProperty("Grace period (seconds)")]
        public int GracePeriod;
        [JsonProperty("Always check for password on join")]
        public bool AlwaysCheck;
        [JsonProperty("Ask for password in chat")]
        public bool PromptInChat;
        [JsonProperty("Check for password in names")]
        public bool CheckNames;
        [JsonProperty("Freeze unauthorized players")]
        public bool FreezeUnauthorized;
        [JsonProperty("Mute unauthorized players")]
        public bool MuteUnauthorized;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private readonly Dictionary<string, Timer> freezeTimers;
    private readonly Dictionary<string, int> attempts;
    private readonly HashSet<string> authorized;
    private const string permBypass;
    private void Init();
    private void OnServerInitialized();
    private object CanUserLogin(string playerName, string playerId);
    private void OnUserConnected(IPlayer player);
    private void OnUserDisconnected(IPlayer player);
    private object OnUserChat(IPlayer player);
    private object OnBetterChat(Dictionary<string, object> data);
    private void CommandPassword(IPlayer player, string command, string[] args);
    private void CommandServerPassword(IPlayer player, string command, string[] args);
    private void AddLocalizedCommand(string command);
    private string GetLang(string langKey, string playerId, object[] args);
    private void Message(IPlayer player, string textOrLang, object[] args);
}

 class Configuration
{
    [JsonProperty("Server password")]
    public string ServerPassword;
    [JsonProperty("Maxium password attempts")]
    public int MaxAttempts;
    [JsonProperty("Grace period (seconds)")]
    public int GracePeriod;
    [JsonProperty("Always check for password on join")]
    public bool AlwaysCheck;
    [JsonProperty("Ask for password in chat")]
    public bool PromptInChat;
    [JsonProperty("Check for password in names")]
    public bool CheckNames;
    [JsonProperty("Freeze unauthorized players")]
    public bool FreezeUnauthorized;
    [JsonProperty("Mute unauthorized players")]
    public bool MuteUnauthorized;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## PathFinding by Razor - Path finding API, used by other plugins only

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using UnityEngine;
using static UnityEngine.Vector3;

Oxide.Plugins
[Info("PathFinding", "Reneb / Nogrod", "1.1.3", ResourceId = 868)]
[Description("Path finding API, used by other plugins only")]
public class PathFinding : RustPlugin
{
    private static readonly Vector3 Up;
    public sealed class PathFinder
    {
        private static readonly sbyte[,] Direction;
        public static readonly Vector3 EyesPosition;
        public static readonly uint Size;
        public static PriorityQueue OpenList;
        public static PathFindNode[,] Grid;
        static PathFinder();
        public List<Vector3> FindPath(Vector3 sourcePos, Vector3 targetPos);
        private static void Clear();
        private static List<Vector3> RetracePath(PathFindNode startNode, PathFindNode endNode);
        private static int GetDistance(PathFindNode nodeA, PathFindNode nodeB);
        private static PathFindNode FindPathNodeOrCreate(int x, int z, float y);
    }

    public sealed class PriorityQueue
    {
        private readonly PathFindNode[] nodes;
        private int numNodes;
        public PriorityQueue(int maxNodes);
        public int Count { get; set; }
        public int MaxSize { get; set; }
        public void Clear();
        public bool Contains(PathFindNode node);
        public void Update(PathFindNode node);
        public void Enqueue(PathFindNode node);
        private void Swap(PathFindNode node1, PathFindNode node2);
        private void SortUp(PathFindNode node);
        private void SortDown(PathFindNode node);
        public PathFindNode Dequeue();
        public void Remove(PathFindNode node);
        private static int CompareTo(PathFindNode node, PathFindNode other);
    }

    public sealed class PathFindNode
    {
        public readonly int X;
        public readonly int Z;
        public int QueueIndex;
        public float H;
        public float G;
        public float F;
        public PathFindNode Parent;
        public Vector3 Position;
        public bool Walkable;
        public PathFindNode(Vector3 position);
        public override int GetHashCode();
    }

    public static bool FindRawGroundPosition(Vector3 sourcePos, Vector3 groundPos);
    private class PathFollower : MonoBehaviour
    {
        public List<Vector3> Paths;
        public float secondsTaken;
        public float secondsToTake;
        public float waypointDone;
        public float speed;
        public Vector3 StartPos;
        public Vector3 EndPos;
        public Vector3 nextPos;
        public BaseEntity entity;
        public BasePlayer player;
        private void Awake();
        private void Move();
        private void Execute_Move();
        private void FindNextWaypoint();
        public void SetMovementPoint(Vector3 endpos, float s);
        private void SetViewAngle(BasePlayer player, Quaternion ViewAngles);
        private void FixedUpdate();
    }

    public static Vector3 jumpPosition;
    public static int groundLayer;
    public static int blockLayer;
    private static int MaxDepth;
    protected override void LoadDefaultConfig();
    private void CheckCfg(string Key, T var);
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private bool FindAndFollowPath(BaseEntity entity, Vector3 sourcePosition, Vector3 targetPosition);
    private void FollowPath(BaseEntity entity, List<Vector3> pathpoints);
    [HookMethod("FindBestPath")]
    public List<Vector3> FindBestPath(Vector3 sourcePosition, Vector3 targetPosition);
    public List<Vector3> Go(Vector3 source, Vector3 target);
    private List<Vector3> FindPath(Vector3 sourcePosition, Vector3 targetPosition);
    private List<Vector3> FindLinePath(Vector3 sourcePosition, Vector3 targetPosition);
    private void ResetPathFollowers();
    [ChatCommand("path")]
    private void cmdChatPath(BasePlayer player, string command, string[] args);
    private bool TryGetPlayerView(BasePlayer player, Quaternion viewAngle);
    private bool TryGetClosestRayPoint(Vector3 sourcePos, Quaternion sourceDir, object closestEnt, Vector3 closestHitpoint);
}

public sealed class PathFinder
{
    private static readonly sbyte[,] Direction;
    public static readonly Vector3 EyesPosition;
    public static readonly uint Size;
    public static PriorityQueue OpenList;
    public static PathFindNode[,] Grid;
    static PathFinder();
    public List<Vector3> FindPath(Vector3 sourcePos, Vector3 targetPos);
    private static void Clear();
    private static List<Vector3> RetracePath(PathFindNode startNode, PathFindNode endNode);
    private static int GetDistance(PathFindNode nodeA, PathFindNode nodeB);
    private static PathFindNode FindPathNodeOrCreate(int x, int z, float y);
}

public sealed class PriorityQueue
{
    private readonly PathFindNode[] nodes;
    private int numNodes;
    public PriorityQueue(int maxNodes);
    public int Count { get; set; }
    public int MaxSize { get; set; }
    public void Clear();
    public bool Contains(PathFindNode node);
    public void Update(PathFindNode node);
    public void Enqueue(PathFindNode node);
    private void Swap(PathFindNode node1, PathFindNode node2);
    private void SortUp(PathFindNode node);
    private void SortDown(PathFindNode node);
    public PathFindNode Dequeue();
    public void Remove(PathFindNode node);
    private static int CompareTo(PathFindNode node, PathFindNode other);
}

public sealed class PathFindNode
{
    public readonly int X;
    public readonly int Z;
    public int QueueIndex;
    public float H;
    public float G;
    public float F;
    public PathFindNode Parent;
    public Vector3 Position;
    public bool Walkable;
    public PathFindNode(Vector3 position);
    public override int GetHashCode();
}

private class PathFollower : MonoBehaviour
{
    public List<Vector3> Paths;
    public float secondsTaken;
    public float secondsToTake;
    public float waypointDone;
    public float speed;
    public Vector3 StartPos;
    public Vector3 EndPos;
    public Vector3 nextPos;
    public BaseEntity entity;
    public BasePlayer player;
    private void Awake();
    private void Move();
    private void Execute_Move();
    private void FindNextWaypoint();
    public void SetMovementPoint(Vector3 endpos, float s);
    private void SetViewAngle(BasePlayer player, Quaternion ViewAngles);
    private void FixedUpdate();
}


```

---

## PayForElectricity by mr01sam - Electric generators cost money (or scrap) to provide power.

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Pay for Electricity", "mr01sam", "1.0.4")]
[Description("Electric generators cost money (or scrap) to provide power")]
public class PayForElectricity : CovalencePlugin
{
    private static PayForElectricity PLUGIN;
    private const string PermissionUse;
    [PluginReference]
    private readonly Plugin Economics;
    private readonly string prefab;
    private const int PAY_PERIOD;
     void Init();
     void OnServerInitialized(bool initial);
     void OnServerSave();
     void Unload();
     void OnEntitySpawned(ElectricGenerator entity);
     void OnEntityKill(ItemBasedFlowRestrictor entity);
     void OnLootEntity(BasePlayer player, ItemBasedFlowRestrictor entity);
     ItemContainer.CanAcceptResult? CanAcceptItem(ItemContainer container, Item item, int targetPos);
    private void CheckIfLooting(BasePlayer player);
    private string FormatTime(BasePlayer player, double hours);
    private void RemoveScrap(BasePlayer player, int amount);
    private double GetBalance(BasePlayer player);
    private void SubBalance(BasePlayer player, double amount);
    private void PlayEffect(BaseEntity entity, string effectString);
    private class PaidGenerator
    {
        public static readonly Dictionary<ulong, PaidGenerator> Generators;
        public NetworkableId boxId { get; set; }
        public NetworkableId generatorId { get; set; }
        public double powerOutput { get; set; }
        public double costPerHour { get; set; }
        public double balance { get; set; }
        public PaidGenerator(NetworkableId generatorId, NetworkableId boxId);
        public void SetPowerOutput(double output);
        public ElectricGenerator GetElectricGenerator(NetworkableId entityId);
        public double AddBalance(double amount);
        public void CollectPayment(int seconds);
        public double GetHoursRemaining();
        public double CostOfHours(int hours);
        private void PlayShutdownEffect();
        public static void LoadAll(Dictionary<ulong, PaidGenerator> existing);
        public static void Destroy(NetworkableId boxId);
        public static PaidGenerator Create(NetworkableId generatorId, NetworkableId boxId);
        public static PaidGenerator FindById(NetworkableId boxId);
    }

    [Command("setpower")]
    private void cmd_setpower(IPlayer player, string command, string[] args);
    [Command("addhours")]
    private void cmd_addhours(IPlayer player, string command, string[] args);
    private Configuration config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Price per watt (hourly)")]
        public float PricePerWatt;
        [JsonProperty(PropertyName = "Min power")]
        public float MinPower;
        [JsonProperty(PropertyName = "Max power")]
        public float MaxPower;
        [JsonProperty(PropertyName = "Power increments")]
        public float PowerIncrements;
        [JsonProperty(PropertyName = "Use economics balance (requires plugin)")]
        public bool UseEconomics;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
     void ShowUI(BasePlayer player, PaidGenerator generator);
    protected override void LoadDefaultMessages();
    private string Lang(string key, string id, object[] args);
}

private class PaidGenerator
{
    public static readonly Dictionary<ulong, PaidGenerator> Generators;
    public NetworkableId boxId { get; set; }
    public NetworkableId generatorId { get; set; }
    public double powerOutput { get; set; }
    public double costPerHour { get; set; }
    public double balance { get; set; }
    public PaidGenerator(NetworkableId generatorId, NetworkableId boxId);
    public void SetPowerOutput(double output);
    public ElectricGenerator GetElectricGenerator(NetworkableId entityId);
    public double AddBalance(double amount);
    public void CollectPayment(int seconds);
    public double GetHoursRemaining();
    public double CostOfHours(int hours);
    private void PlayShutdownEffect();
    public static void LoadAll(Dictionary<ulong, PaidGenerator> existing);
    public static void Destroy(NetworkableId boxId);
    public static PaidGenerator Create(NetworkableId generatorId, NetworkableId boxId);
    public static PaidGenerator FindById(NetworkableId boxId);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Price per watt (hourly)")]
    public float PricePerWatt;
    [JsonProperty(PropertyName = "Min power")]
    public float MinPower;
    [JsonProperty(PropertyName = "Max power")]
    public float MaxPower;
    [JsonProperty(PropertyName = "Power increments")]
    public float PowerIncrements;
    [JsonProperty(PropertyName = "Use economics balance (requires plugin)")]
    public bool UseEconomics;
}


```

---

## PayNow by PayNow - Monetize your server with PayNow.gg: Fees as low as 2.5%, no chargebacks, in USD/GBP/EUR.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System.Text;

Oxide.Plugins
[Info("PayNow", "PayNow Services Inc", "0.0.10")]
[Description("Official plugin for the PayNow.gg store integration.")]
internal class PayNow : CovalencePlugin
{
    const string COMMAND_QUEUE_URL;
    const string GS_LINK_URL;
     PluginConfig _config;
    readonly Dictionary<string, string> _headers;
    readonly CommandHistory _executedCommands;
    readonly StringBuilder _cachedStringBuilder;
    readonly List<string> _successfulCommandsList;
     Timer _pendingCommandsTimer;
    [HookMethod("Loaded")]
     void Loaded();
    [HookMethod("OnServerInitialized")]
     void OnServerInitialized();
    [Command("paynow.token")]
     void CommandToken(IPlayer player, string command, string[] args);
     void ValidateToken();
     void StartPendingCommandsLoop();
     void StopPendingCommandsLoop();
     void LinkGameServer(Action continuationCallback);
     void HandleLinkGameServerResponse(int code, string responseString, Action continuationCallback);
     void GetPendingCommands();
     void HandlePendingCommands(int code, string response);
     void AcknowledgeCommands(List<string> commandsIds);
     void HandleAcknowledgeCommands(int code, string response);
     void ProcessPendingCommands(QueuedCommand[] queuedCommands);
     bool ExecuteCommand(string command);
    [Serializable]
    public class QueuedCommand
    {
        [JsonProperty("attempt_id")]
        public string AttemptId;
        [JsonProperty("steam_id")]
        public string SteamId;
        [JsonProperty("command")]
        public string Command;
        [JsonProperty("online_only")]
        public bool OnlineOnly;
        [JsonProperty("queued_at")]
        public string QueuedAt;
    }

    [Serializable]
    public class LinkGameServerRequest
    {
        [JsonProperty("ip")]
        public string Ip;
        [JsonProperty("hostname")]
        public string Hostname;
        [JsonProperty("platform")]
        public string Platform;
        [JsonProperty("version")]
        public string Version;
    }

    [Serializable]
    public class LinkGameServerResponse
    {
        [JsonProperty("update_available")]
        public bool UpdateAvailable { get; set; }
        [JsonProperty("latest_version")]
        public string LatestVersion { get; set; }
        [JsonProperty("previously_linked")]
        public PreviouslyLinkedData PreviouslyLinked { get; set; }
        [JsonProperty("gameserver")]
        public GameServerData GameServer { get; set; }
        public class PreviouslyLinkedData
        {
            [JsonProperty("ip")]
            public string IP { get; set; }
            [JsonProperty("host_name")]
            public string HostName { get; set; }
            [JsonProperty("last_linked_at")]
            public DateTime LastLinkedAt { get; set; }
        }

        public class GameServerData
        {
            [JsonProperty("id")]
            public long Id { get; set; }
            [JsonProperty("store_id")]
            public long StoreId { get; set; }
            [JsonProperty("name")]
            public string Name { get; set; }
            [JsonProperty("created_at")]
            public DateTime? CreatedAt { get; set; }
            [JsonProperty("updated_at")]
            public DateTime? UpdatedAt { get; set; }
        }

    }

    [Serializable]
     class PluginConfig
    {
        [JsonProperty("API token")]
        public string ApiToken;
        [JsonProperty("Time between API checks in seconds")]
        public float ApiCheckIntervalSeconds;
        [JsonProperty("Log command executions")]
        public bool LogCommandExecutions;
        [JsonProperty("ApiToken")]
        public string OldApiToken { get; set; }
        [JsonProperty("ApiCheckIntervalSeconds")]
        public float OldApiCheckIntervalSeconds { get; set; }
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     void UpdateHeaders();
     string BuildAcknowledgeJson(List<string> orderIds);
     string BuildOnlinePlayersJson();
     class CommandHistory
    {
        readonly Queue<string> _queue;
        readonly int _capacity;
        public CommandHistory(int capacity);
        public void Add(string command);
        public bool Contains(string command);
    }

     void PrintException(string message, Exception ex);
}

[Serializable]
public class QueuedCommand
{
    [JsonProperty("attempt_id")]
    public string AttemptId;
    [JsonProperty("steam_id")]
    public string SteamId;
    [JsonProperty("command")]
    public string Command;
    [JsonProperty("online_only")]
    public bool OnlineOnly;
    [JsonProperty("queued_at")]
    public string QueuedAt;
}

[Serializable]
public class LinkGameServerRequest
{
    [JsonProperty("ip")]
    public string Ip;
    [JsonProperty("hostname")]
    public string Hostname;
    [JsonProperty("platform")]
    public string Platform;
    [JsonProperty("version")]
    public string Version;
}

[Serializable]
public class LinkGameServerResponse
{
    [JsonProperty("update_available")]
    public bool UpdateAvailable { get; set; }
    [JsonProperty("latest_version")]
    public string LatestVersion { get; set; }
    [JsonProperty("previously_linked")]
    public PreviouslyLinkedData PreviouslyLinked { get; set; }
    [JsonProperty("gameserver")]
    public GameServerData GameServer { get; set; }
    public class PreviouslyLinkedData
    {
        [JsonProperty("ip")]
        public string IP { get; set; }
        [JsonProperty("host_name")]
        public string HostName { get; set; }
        [JsonProperty("last_linked_at")]
        public DateTime LastLinkedAt { get; set; }
    }

    public class GameServerData
    {
        [JsonProperty("id")]
        public long Id { get; set; }
        [JsonProperty("store_id")]
        public long StoreId { get; set; }
        [JsonProperty("name")]
        public string Name { get; set; }
        [JsonProperty("created_at")]
        public DateTime? CreatedAt { get; set; }
        [JsonProperty("updated_at")]
        public DateTime? UpdatedAt { get; set; }
    }

}

public class PreviouslyLinkedData
{
    [JsonProperty("ip")]
    public string IP { get; set; }
    [JsonProperty("host_name")]
    public string HostName { get; set; }
    [JsonProperty("last_linked_at")]
    public DateTime LastLinkedAt { get; set; }
}

public class GameServerData
{
    [JsonProperty("id")]
    public long Id { get; set; }
    [JsonProperty("store_id")]
    public long StoreId { get; set; }
    [JsonProperty("name")]
    public string Name { get; set; }
    [JsonProperty("created_at")]
    public DateTime? CreatedAt { get; set; }
    [JsonProperty("updated_at")]
    public DateTime? UpdatedAt { get; set; }
}

[Serializable]
 class PluginConfig
{
    [JsonProperty("API token")]
    public string ApiToken;
    [JsonProperty("Time between API checks in seconds")]
    public float ApiCheckIntervalSeconds;
    [JsonProperty("Log command executions")]
    public bool LogCommandExecutions;
    [JsonProperty("ApiToken")]
    public string OldApiToken { get; set; }
    [JsonProperty("ApiCheckIntervalSeconds")]
    public float OldApiCheckIntervalSeconds { get; set; }
}

 class CommandHistory
{
    readonly Queue<string> _queue;
    readonly int _capacity;
    public CommandHistory(int capacity);
    public void Add(string command);
    public bool Contains(string command);
}


```

---

## PerfectRepair by TheFriendlyChap - Items will be fully repaired, removing the permanent penalty (red bar)

```csharp
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Perfect Repair", "The Friendly Chap", "1.1.2")]
[Description("Items will be fully repaired, removing the permanent penalty (red bar)")]
public class PerfectRepair : RustPlugin
{
    private void OnServerInitialized();
    private IEnumerator CheckContainers();
    private void ShowLogo();
}


```

---

## PerformanceMonitor by ViolationHandler - Tool for collecting information about server performance

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Performance Monitor", "Orange", "1.2.7")]
[Description("Tool for collecting information about server performance")]
public class PerformanceMonitor : RustPlugin
{
    private const string commandString;
    private const string commandString2;
    private PerformanceDump currentReport;
    private void Init();
    private void OnServerInitialized();
    private void cmdCompleteNow(ConsoleSystem.Arg arg);
    private void CreateReport();
    private IEnumerator CreateActualReport();
    private void CompletePluginsReport();
    private void ProcessPlugins(Plugin plugin, List<string> list);
    private IEnumerator CompleteEntitiesReport();
    private void SaveReport(PerformanceDump dump);
    private string Time();
    private static string FormatBytes(long bytes);
    private static string FormatBytes(double bytes);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Save file dates in European Format 27-06-2023 (other format is North American 06-27-2023)")]
        public bool EuropeanTimeSave;
        [JsonProperty(PropertyName = "Create reports every (seconds)")]
        public int checkTime;
        [JsonProperty(PropertyName = "Create plugins report")]
        public bool runPluginsReport;
        [JsonProperty(PropertyName = "Sort Plugins By Hook Time (If set false, sorts by Memory Usage)")]
        public bool sortByHookTime;
        [JsonProperty(PropertyName = "Create entities report")]
        public bool runEntitiesReport;
        [JsonProperty(PropertyName = "Excluded entities")]
        public string[] excludedEntities;
        [JsonProperty(PropertyName = "Excluded plugins")]
        public string[] excludedPlugins;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class PerformanceDump
    {
        [JsonProperty(PropertyName = "Online Players")]
        public int onlinePlayers;
        [JsonProperty(PropertyName = "Offline Players")]
        public int offlinePlayers;
        [JsonProperty(PropertyName = "Entities Report")]
        public EntitiesReport entities;
        [JsonProperty(PropertyName = "Plugins Report")]
        public string[] plugins;
        [JsonProperty(PropertyName = "Performance Report")]
        public Performance.Tick performance;
        [JsonIgnore]
        public int statusBar;
        [JsonIgnore]
        public int entitiesChecked;
        [JsonIgnore]
        public int entitiesTotal;
    }

    private class EntitiesReport
    {
        [JsonProperty(PropertyName = "Total")]
        public int countGlobal;
        [JsonProperty(PropertyName = "Owned")]
        public int countOwned;
        [JsonProperty(PropertyName = "Unowned")]
        public int countUnowned;
        [JsonProperty(PropertyName = "List")]
        public Dictionary<string, EntityInfo> list;
        [JsonIgnore]
        public bool completed;
    }

    private class EntityInfo
    {
        [JsonProperty(PropertyName = "Total")]
        public int countGlobal;
        [JsonProperty(PropertyName = "Owned")]
        public int countOwned;
        [JsonProperty(PropertyName = "Unowned")]
        public int countUnowned;
    }

}

private class ConfigData
{
    [JsonProperty(PropertyName = "Save file dates in European Format 27-06-2023 (other format is North American 06-27-2023)")]
    public bool EuropeanTimeSave;
    [JsonProperty(PropertyName = "Create reports every (seconds)")]
    public int checkTime;
    [JsonProperty(PropertyName = "Create plugins report")]
    public bool runPluginsReport;
    [JsonProperty(PropertyName = "Sort Plugins By Hook Time (If set false, sorts by Memory Usage)")]
    public bool sortByHookTime;
    [JsonProperty(PropertyName = "Create entities report")]
    public bool runEntitiesReport;
    [JsonProperty(PropertyName = "Excluded entities")]
    public string[] excludedEntities;
    [JsonProperty(PropertyName = "Excluded plugins")]
    public string[] excludedPlugins;
}

private class PerformanceDump
{
    [JsonProperty(PropertyName = "Online Players")]
    public int onlinePlayers;
    [JsonProperty(PropertyName = "Offline Players")]
    public int offlinePlayers;
    [JsonProperty(PropertyName = "Entities Report")]
    public EntitiesReport entities;
    [JsonProperty(PropertyName = "Plugins Report")]
    public string[] plugins;
    [JsonProperty(PropertyName = "Performance Report")]
    public Performance.Tick performance;
    [JsonIgnore]
    public int statusBar;
    [JsonIgnore]
    public int entitiesChecked;
    [JsonIgnore]
    public int entitiesTotal;
}

private class EntitiesReport
{
    [JsonProperty(PropertyName = "Total")]
    public int countGlobal;
    [JsonProperty(PropertyName = "Owned")]
    public int countOwned;
    [JsonProperty(PropertyName = "Unowned")]
    public int countUnowned;
    [JsonProperty(PropertyName = "List")]
    public Dictionary<string, EntityInfo> list;
    [JsonIgnore]
    public bool completed;
}

private class EntityInfo
{
    [JsonProperty(PropertyName = "Total")]
    public int countGlobal;
    [JsonProperty(PropertyName = "Owned")]
    public int countOwned;
    [JsonProperty(PropertyName = "Unowned")]
    public int countUnowned;
}


```

---

## PerformanceUI by 2CHEVSKII - Displays server FPS and tickrate in real-time via GUI

```csharp
using System.Collections.Generic;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Oxide.Core.Plugins;
using System;

Oxide.Plugins
[Info("Performance UI", "2CHEVSKII", "1.0.7")]
[Description("Shows information about server performance in a user-friendly way")]
 class PerformanceUI : RustPlugin
{
    protected override void LoadDefaultConfig();
    private void LoadCFG();
    private void CheckConfig(string key, T value);
    private void CheckConfigFloat(string key, float value);
    protected override void LoadDefaultMessages();
    [PluginReference]
    private Plugin ImageLibrary;
    private Dictionary<BasePlayer, bool> guiUsersOpened;
    private Dictionary<BasePlayer, bool> guiUsersCollapsed;
    private int nominalTickrate;
    private int nominalFPS;
    private int tickrate;
    private int ticks;
    private bool isTimerRunning;
    private bool usePermissions;
    private const string permUse;
    private const string permUseGUI;
    private float UI_X;
    private float UI_Y;
    private void OnTick();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerInit(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    [ChatCommand("performance")]
    private void CmdChatPerformance(BasePlayer player, string command, string[] args);
    [ConsoleCommand("performance.size")]
    private void CmdConsoleSize(ConsoleSystem.Arg argument);
    private void OpenCloseUI(BasePlayer player);
    private void ExpandCollapseUI(BasePlayer player);
    private void RefreshUI();
    private int GetFPS();
    private float GetFrametime();
    private int GetTickrate();
    private int GetUserPing(BasePlayer player);
    private CuiElementContainer expandedContainer;
    private CuiElementContainer collapsedContainer;
    private const string mainCanvas;
    private const string ecButton;
    private const string tickrateBG;
    private const string tickrateValue;
    private const string tickrateIcon;
    private const string tickrateURL;
    private string tickColor;
    private string frameColor;
    private string bigUIAMin;
    private string bigUIAMax;
    private string smallUIAMin;
    private string smallUIAMax;
    private const string fpsBG;
    private const string fpsFTValue;
    private const string fpsFTIcon;
    private const string fpsURL;
    private void IconSize();
    private void DownloadIcons();
    private void ColorSwitcher(string tickColor, string frameColor);
    private void BuildUI();
    private void BuildSmallUI();
}


```

---

## PerfTimings by Rockioux - Performance timer allowing to get min, max, cumul and average processing time for a block of code.

```csharp
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;

Oxide.Plugins
[Info("Perf Timings", "Rockioux", "1.0.0")]
[Description("Rudimentary performance timer allowing to get min, max, cumul and average processing time for a block of code. Meant to be used by plugin developers.")]
public class PerfTimings : CovalencePlugin
{
    public class PerfTimer : IDisposable
    {
        private readonly Stopwatch _timer;
        private readonly PerfTimings _plugin;
        private readonly string _key;
        public PerfTimer(PerfTimings plugin, string key);
        public void Dispose();
    }

    private class PerfMetrics
    {
        public double Cumul { get; set; }
        public double Min { get; set; }
        public double Max { get; set; }
        public int TotalCalls { get; set; }
        public double Avg { get; set; }
        public void AddValue(double value);
    }

    private readonly Dictionary<string, PerfMetrics> _perfMetricsByTag;
    private PerfMetrics _framePerfMetrics;
    private double _captureDurationMs;
    private int _maxKeyStrLen;
    private bool _isCurrentlyCapturing;
    private const double kDefaultCaptureDurationMs;
    private const int kColumnAlignment;
    public PerfTimer CreatePerfTimer(string[] tags);
    [Command("perfcapture"), Permission("perftimings.capture")]
    private void PerfTimingsCapture(IPlayer player, string command, string[] args);
    private void OnFrame(float delta);
    private void AddPerfMetrics(string key, double value);
    private void Reset();
    private void PerfTimingsCapture_Internal(int duration);
    private void PrintStats();
}

public class PerfTimer : IDisposable
{
    private readonly Stopwatch _timer;
    private readonly PerfTimings _plugin;
    private readonly string _key;
    public PerfTimer(PerfTimings plugin, string key);
    public void Dispose();
}

private class PerfMetrics
{
    public double Cumul { get; set; }
    public double Min { get; set; }
    public double Max { get; set; }
    public int TotalCalls { get; set; }
    public double Avg { get; set; }
    public void AddValue(double value);
}


```

---

## PermaDeath by  - Deletes all of a player's entities after a set amount of deaths

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;

Oxide.Plugins
[Info("Perma Death", "Kappasaurus", "1.0.1")]
[Description("Deletes all of a player's entities after a set amount of deaths")]
 class PermaDeath : RustPlugin
{
    private int maximumDeaths;
    private bool includeSuicide;
    static PermaDeath Instance;
     class StoredData
    {
        public List<PlayerInfo> Players;
        public PlayerInfo GetInfo(BasePlayer player);
    }

     StoredData storedData;
     class PlayerInfo
    {
        public ulong SteamID;
        public int Deaths;
        public PlayerInfo();
        public PlayerInfo(BasePlayer player);
    }

    private void SaveData();
    private void ReadData();
    private void Init();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnNewSave();
    [ChatCommand("permadeath")]
    private void PermadeathCommand(BasePlayer player, string command, string[] args);
    private new void LoadConfig();
    protected override void LoadDefaultConfig();
    private void GetConfig(T variable, string[] path);
    protected override void LoadDefaultMessages();
}

 class StoredData
{
    public List<PlayerInfo> Players;
    public PlayerInfo GetInfo(BasePlayer player);
}

 class PlayerInfo
{
    public ulong SteamID;
    public int Deaths;
    public PlayerInfo();
    public PlayerInfo(BasePlayer player);
}


```

---

## PermaMap by MrBlue - Allows players to always have a map to use in their inventory

```csharp
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Perma Map", "Wulf", "1.0.8")]
[Description("Makes sure that players always have access to a map")]
public class PermaMap : CovalencePlugin
{
    protected override void LoadDefaultMessages();
    private const float checkTime;
    private const string permUse;
    private void Init();
    private object CanCraft(ItemCrafter itemCrafter, ItemBlueprint blueprint);
    private void AddMap(BasePlayer player);
    private void RemoveMap(BasePlayer player);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDeath(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnItemRemovedFromContainer(ItemContainer container, Item item);
    private void JoinedEvent(BasePlayer player);
    private void LeftEvent(BasePlayer player);
    private string GetLang(string langKey, string playerId, object[] args);
}


```

---

## PermissionEffects by  - add a detailed time limit to "permission" and provides a GUI for it

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Permission Effects", "noname", "1.1.1")]
[Description("Enhance the permission system by abstracting Oxide permission system")]
 class PermissionEffects : CovalencePlugin
{
    [PluginReference]
    private Plugin PlaytimeTracker;
    private Plugin UIScaleManager;
    public static PermissionEffects Plugin;
    public static string permissioneffects_admin_perm_admin_perm;
     Dictionary<string, Timer> PlayersViewingUi;
     string CursorUIPanel;
     string UIBaseInvPanel;
     string UIBasePanel;
     string BackGroundPanel;
     string UIBaseBtnPanel;
     string UIPMBtn;
     string ESBtn;
    private new void LoadDefaultConfig();
    private void Init();
     void OnServerSave();
     void Unload();
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private class BasePlayerEffectThread : IEquatable<BasePlayerEffectThread>
    {
        public PlayerThreadType PlayerThreadType { get; set; }
        public string ThreadName { get; set; }
        public string ThreadColor { get; set; }
        public List<string> Permissions { get; set; }
        public DateTime DisconnectAutoExpireDate { get; set; }
        public TimeSpan? DisconnectAutoExpireTime { get; set; }
        public DateTime ExpireDate { get; set; }
        public TimeSpan ExpireTime { get; set; }
        public BasePlayerEffectThread();
        public bool Equals(BasePlayerEffectThread other);
        public override bool Equals(object obj);
        public override int GetHashCode();
        public void GrantUserPermissions(string id);
        public void GrantGroupPermissions(string groupName);
        public void RevokeUserPermissions(string id, Dictionary<string, BasePlayerEffectThread> PlayerEffectThreads);
        public void RevokeUserPermissions(string id);
        public void RevokeGroupPermissions(string groupName, Dictionary<string, BasePlayerEffectThread> PlayerEffectThreads);
        public void RevokeGroupPermissions(string groupName);
    }

    private class BaseDataEffectThread
    {
        public DataThreadType DataThreadType { get; set; }
        public string ThreadName { get; set; }
        public string ThreadColor { get; set; }
        public List<string> Permissions { get; set; }
        public TimeSpan? DisconnectAutoExpireTime { get; set; }
        public TimeSpan ExpireTime { get; set; }
        public BaseDataEffectThread();
        public BasePlayerEffectThread ToPlayerEffectThread(string PlayerID);
    }

    private class EffectThread : BasePlayerEffectThread
    {
        public EffectThread();
        public EffectThread(string threadName, string threadColor, List<string> permissions, TimeSpan? AutoExpireTIme);
    }

    private class RealtimeEffectThread : EffectThread
    {
        public RealtimeEffectThread();
        public RealtimeEffectThread(string threadName, string threadColor, List<string> permissions, TimeSpan? AutoExpireTIme, TimeSpan expireTime);
    }

    private class PlaytimeEffectThread : EffectThread
    {
        public PlaytimeEffectThread();
        public PlaytimeEffectThread(string threadName, string threadColor, List<string> permissions, TimeSpan? AutoExpireTIme, TimeSpan expireTime, string PlayerID);
    }

    private class EffectThreadData : BaseDataEffectThread
    {
        public EffectThreadData();
        public EffectThreadData(string threadName, string threadColor, List<string> permissions, TimeSpan? AutoExpireTIme);
    }

    private class RealtimeEffectThreadData : EffectThreadData
    {
        public RealtimeEffectThreadData();
        public RealtimeEffectThreadData(string threadName, string threadColor, List<string> permissions, TimeSpan? AutoExpireTIme, TimeSpan expireTime);
    }

    private class PlaytimeEffectThreadData : EffectThreadData
    {
        public PlaytimeEffectThreadData();
        public PlaytimeEffectThreadData(string threadName, string threadColor, List<string> permissions, TimeSpan? AutoExpireTIme, TimeSpan expireTime);
    }

    private PluginConfig config;
    private new void LoadConfig();
    private void VersionUpdate(PluginConfig config);
    private class PluginConfig
    {
        public bool use_BroadCast;
        public bool use_Message;
        public bool use_ConsoleMessage;
        public int UIUpdateTimerInterval;
        public int TimeLimitTimerInterval;
        public int DisconnectDetectTimerInterval;
        public string EPSCommand;
        public string EffectCommand;
        public Dictionary<int, Posion> UIPosions;
        public class Posion
        {
            public float YAnchorMax;
            public float YAnchorMin;
            public float XAnchorMax;
            public float XAnchorMin;
        }

        public VersionNumber ConfigVersion;
    }

    private PluginConfig GetDefaultConfig();
    private Dictionary<int, PluginConfig.Posion> GetUIPosionConfig();
     DynamicConfigFile playersdataFile;
     PlayersData playersData;
    private void LoadPlayersData();
    private void SavePlayersData();
    private class PlayersData
    {
        public Dictionary<string, PlayerInfo> Players { get; set; }
        public PlayersData();
        public void ResetData(IPlayer player);
        public void UpdatePlayers();
        public void AddEffectToPlayer(IPlayer target, IPlayer player, string EffectThreadName);
        public void DeleteEffectFromPlayer(IPlayer target, IPlayer player, string EffectThreadName);
        public void DeleteEffectFromPlayer(string target, IPlayer player, string EffectThreadName);
        public void DeleteAllEffectFromPlayer(IPlayer target, IPlayer player);
        public bool PlayerHasEffect(string playerId, string effectThreadName);
        public TimeSpan? GetPlayerEffectLeftTime(string playerId, string effectThreadName);
    }

    private class PlayerInfo
    {
        public string Id { get; set; }
        public string Name { get; set; }
        public Dictionary<string, BasePlayerEffectThread> PlayerEffectThreads { get; set; }
        public PlayerInfo();
        public PlayerInfo(IPlayer player);
        public void UpdateEffects();
        public void UpdateEffect(string EffectName);
        public void AddEffect(string EffectThreadName, IPlayer player);
        public void DeleteEffect(string EffectThreadName, IPlayer player);
        public void DeleteAllEffect(IPlayer player);
    }

     DynamicConfigFile groupsdataFile;
     GroupsData groupsData;
    private void LoadGroupsData();
    private void SaveGroupsData();
    private class GroupsData
    {
        public Dictionary<string, GroupInfo> Groups { get; set; }
        public GroupsData();
        public void ResetData(IPlayer player);
        public void UpdateGroup();
        public void AddEffectToGroup(string GroupName, IPlayer player, string EffectThreadName);
        public void DeleteEffectFromGroup(string GroupName, IPlayer player, string EffectThreadName);
        public void DeleteAllEffectFromGroup(string GroupName, IPlayer player);
        public bool GroupHasEffect(string groupName, string effectThreadName);
        public TimeSpan? GetGroupEffectLeftTime(string groupName, string effectThreadName);
    }

    private class GroupInfo
    {
        public string GroupName { get; set; }
        public Dictionary<string, BasePlayerEffectThread> PlayerEffectThreads { get; set; }
        public GroupInfo();
        public GroupInfo(string groupName);
        public void UpdateEffects();
        public void UpdateEffect(string EffectName);
        public void AddEffect(string EffectThreadName, IPlayer player);
        public void DeleteEffect(string EffectThreadName, IPlayer player);
        public void DeleteAllEffect(IPlayer player);
    }

     DynamicConfigFile threadsdataFile;
     RegisteredThreads registeredThreads;
    private void LoadThreadsData();
    private void SaveThreadsData();
    private class RegisteredThreads
    {
        public Dictionary<string, BaseDataEffectThread> DataEffectThreads { get; set; }
        public RegisteredThreads();
        public void RegisterThread(BaseDataEffectThread dataEffectThread, IPlayer player);
        public void UnRegisterThread(string effectThreadName, IPlayer player);
        public bool ThreadIsExist(string ThreadName);
    }

     DynamicConfigFile playersuidataFile;
     PlayersGUIData playersUIData;
    private void LoadPlayersGUIData();
    private void SavePlayersGUIData();
    private class PlayersGUIData
    {
        public Dictionary<string, PlayerGUIInfo> Players;
        public PlayersGUIData();
        public void ResetPlayerData(BasePlayer player);
        public void ResetOnlinePlayersData();
    }

    private class PlayerGUIInfo
    {
        public string Id;
        public string Name;
        public bool useUI;
        public int EffectPage;
        public PlayerGUIInfo();
        public PlayerGUIInfo(IPlayer player);
    }

    protected override void LoadDefaultMessages();
    private string Lang(string key, string id, object[] args);
    private void RegisterPermissions();
    private void RegisterTestEffects();
    private EffectThreadData GetTestEffectRegThread();
    private PlaytimeEffectThreadData GetTestEffect2RegThread();
    private RealtimeEffectThreadData GetTestEffect3RegThread();
    private void StartTimer();
    private void TimeLimitTimer_Tick();
    private void UIUpdateTimer_Tick();
    private void DisconnectDetectTimer_Tick();
    private void UpdatePlayerGUI(string Id);
    private void UpdatePlayerGUI(BasePlayer player);
    private void UpdatePlayersGUI();
    private void UpdateUIInfo();
    private void UpdatePagePlayerUIInfo(BasePlayer player);
    private List<BasePlayerEffectThread> GetPlayerGUIData(string Id);
    private string GetRegisteredThread(IPlayer player);
    private string GetPlayersThread(IPlayer player);
    private string GetPlayerThread(IPlayer player, IPlayer target);
    private string GetGroupThread(IPlayer player);
     void BasicCommand(IPlayer player, string command, string[] args);
     void CheckEffectCommand(IPlayer player, string command, string[] args);
     void RegisterCommands();
    [Command("EffecterUI.set")]
     void SetEffectUIPage(IPlayer player, string command, string[] args);
    [Command("eui")]
     void SetUICommand(IPlayer player, string command, string[] args);
    private void OnUIScaleChanged(IPlayer player);
    private void PlayerAddCursor(BasePlayer player);
    private void PlayerAddBaseinvUI(BasePlayer player);
    private void PlayersAddBaseinvUI();
    private void PlayersRemoveBaseinvUI();
    private void UpdateBaseUI(BasePlayer player, List<BasePlayerEffectThread> effectthreadUIs, bool useUpPM, bool useDownPM);
    private CuiElementContainer SingleEffectUI(BasePlayer player, int index, BasePlayerEffectThread effectthreadUI);
    private CuiElementContainer EndBarUI(int index);
     double IndexGap;
    private string GetAnchorMin(BasePlayer player, int index);
    private string GetAnchorMax(BasePlayer player, int index);
    private string GetLeftTImeAnchorMax(BasePlayer player, int index);
    private string GetEndAnchorMax(int index);
    private string GetSingleAnchorMin(int index);
    private string GetSingleAnchorMax(int index);
    private string GetBtnAnchorMax(int index);
    private void API_AddEffectToPlayer(IPlayer player, string EffectThreadName);
    private void API_DeleteEffectFromPlayer(IPlayer player, string EffectThreadName);
    private void API_AddEffectToGroup(string GroupName, string EffectThreadName);
    private void API_DeleteEffectFromGroup(string GroupName, string EffectThreadName);
    private void API_DeleteAllEffectFromGroup(string GroupName);
    private void API_RegisterThread(int ThreadType, string ThreadName, string HexThreadColor, string[] Permissions, int AutoExpireTime, int EffectTime);
    private void API_UnRegisterThread(string ThreadName);
    private bool API_ThreadIsExist(string ThreadName);
    private bool API_PlayerHasEffect(string playerId, string effectThreadName);
    private TimeSpan? API_GetPlayerEffectLeftTime(IPlayer player, string effectThreadName);
    private float GetUISize(string playerID);
    private string GetLeftTimeString(TimeSpan lefttime);
    private TimeSpan? GetPlayerPlaytime(string playerID);
    private string GetHexColorFromFormat(string color);
    private string GetHexColor(Color color);
    private Color GetColorFromString(string str);
    private IPlayer GetPlayer(string nameOrID, IPlayer player);
    private IPlayer GetPlayer(string nameOrID);
    private BasePlayer GetPlayerFromID(string ID);
    private bool PlayerIsOnline(string ID);
    private void SendChatMessage(IPlayer player, string message);
    private void SendReplyMessage(IPlayer player, string message);
    private void SendBroadcastMessage(string message);
    private void puts(string msg);
    private void printWarning(string msg);
}

private class BasePlayerEffectThread : IEquatable<BasePlayerEffectThread>
{
    public PlayerThreadType PlayerThreadType { get; set; }
    public string ThreadName { get; set; }
    public string ThreadColor { get; set; }
    public List<string> Permissions { get; set; }
    public DateTime DisconnectAutoExpireDate { get; set; }
    public TimeSpan? DisconnectAutoExpireTime { get; set; }
    public DateTime ExpireDate { get; set; }
    public TimeSpan ExpireTime { get; set; }
    public BasePlayerEffectThread();
    public bool Equals(BasePlayerEffectThread other);
    public override bool Equals(object obj);
    public override int GetHashCode();
    public void GrantUserPermissions(string id);
    public void GrantGroupPermissions(string groupName);
    public void RevokeUserPermissions(string id, Dictionary<string, BasePlayerEffectThread> PlayerEffectThreads);
    public void RevokeUserPermissions(string id);
    public void RevokeGroupPermissions(string groupName, Dictionary<string, BasePlayerEffectThread> PlayerEffectThreads);
    public void RevokeGroupPermissions(string groupName);
}

private class BaseDataEffectThread
{
    public DataThreadType DataThreadType { get; set; }
    public string ThreadName { get; set; }
    public string ThreadColor { get; set; }
    public List<string> Permissions { get; set; }
    public TimeSpan? DisconnectAutoExpireTime { get; set; }
    public TimeSpan ExpireTime { get; set; }
    public BaseDataEffectThread();
    public BasePlayerEffectThread ToPlayerEffectThread(string PlayerID);
}

private class EffectThread : BasePlayerEffectThread
{
    public EffectThread();
    public EffectThread(string threadName, string threadColor, List<string> permissions, TimeSpan? AutoExpireTIme);
}

private class RealtimeEffectThread : EffectThread
{
    public RealtimeEffectThread();
    public RealtimeEffectThread(string threadName, string threadColor, List<string> permissions, TimeSpan? AutoExpireTIme, TimeSpan expireTime);
}

private class PlaytimeEffectThread : EffectThread
{
    public PlaytimeEffectThread();
    public PlaytimeEffectThread(string threadName, string threadColor, List<string> permissions, TimeSpan? AutoExpireTIme, TimeSpan expireTime, string PlayerID);
}

private class EffectThreadData : BaseDataEffectThread
{
    public EffectThreadData();
    public EffectThreadData(string threadName, string threadColor, List<string> permissions, TimeSpan? AutoExpireTIme);
}

private class RealtimeEffectThreadData : EffectThreadData
{
    public RealtimeEffectThreadData();
    public RealtimeEffectThreadData(string threadName, string threadColor, List<string> permissions, TimeSpan? AutoExpireTIme, TimeSpan expireTime);
}

private class PlaytimeEffectThreadData : EffectThreadData
{
    public PlaytimeEffectThreadData();
    public PlaytimeEffectThreadData(string threadName, string threadColor, List<string> permissions, TimeSpan? AutoExpireTIme, TimeSpan expireTime);
}

private class PluginConfig
{
    public bool use_BroadCast;
    public bool use_Message;
    public bool use_ConsoleMessage;
    public int UIUpdateTimerInterval;
    public int TimeLimitTimerInterval;
    public int DisconnectDetectTimerInterval;
    public string EPSCommand;
    public string EffectCommand;
    public Dictionary<int, Posion> UIPosions;
    public class Posion
    {
        public float YAnchorMax;
        public float YAnchorMin;
        public float XAnchorMax;
        public float XAnchorMin;
    }

    public VersionNumber ConfigVersion;
}

public class Posion
{
    public float YAnchorMax;
    public float YAnchorMin;
    public float XAnchorMax;
    public float XAnchorMin;
}

private class PlayersData
{
    public Dictionary<string, PlayerInfo> Players { get; set; }
    public PlayersData();
    public void ResetData(IPlayer player);
    public void UpdatePlayers();
    public void AddEffectToPlayer(IPlayer target, IPlayer player, string EffectThreadName);
    public void DeleteEffectFromPlayer(IPlayer target, IPlayer player, string EffectThreadName);
    public void DeleteEffectFromPlayer(string target, IPlayer player, string EffectThreadName);
    public void DeleteAllEffectFromPlayer(IPlayer target, IPlayer player);
    public bool PlayerHasEffect(string playerId, string effectThreadName);
    public TimeSpan? GetPlayerEffectLeftTime(string playerId, string effectThreadName);
}

private class PlayerInfo
{
    public string Id { get; set; }
    public string Name { get; set; }
    public Dictionary<string, BasePlayerEffectThread> PlayerEffectThreads { get; set; }
    public PlayerInfo();
    public PlayerInfo(IPlayer player);
    public void UpdateEffects();
    public void UpdateEffect(string EffectName);
    public void AddEffect(string EffectThreadName, IPlayer player);
    public void DeleteEffect(string EffectThreadName, IPlayer player);
    public void DeleteAllEffect(IPlayer player);
}

private class GroupsData
{
    public Dictionary<string, GroupInfo> Groups { get; set; }
    public GroupsData();
    public void ResetData(IPlayer player);
    public void UpdateGroup();
    public void AddEffectToGroup(string GroupName, IPlayer player, string EffectThreadName);
    public void DeleteEffectFromGroup(string GroupName, IPlayer player, string EffectThreadName);
    public void DeleteAllEffectFromGroup(string GroupName, IPlayer player);
    public bool GroupHasEffect(string groupName, string effectThreadName);
    public TimeSpan? GetGroupEffectLeftTime(string groupName, string effectThreadName);
}

private class GroupInfo
{
    public string GroupName { get; set; }
    public Dictionary<string, BasePlayerEffectThread> PlayerEffectThreads { get; set; }
    public GroupInfo();
    public GroupInfo(string groupName);
    public void UpdateEffects();
    public void UpdateEffect(string EffectName);
    public void AddEffect(string EffectThreadName, IPlayer player);
    public void DeleteEffect(string EffectThreadName, IPlayer player);
    public void DeleteAllEffect(IPlayer player);
}

private class RegisteredThreads
{
    public Dictionary<string, BaseDataEffectThread> DataEffectThreads { get; set; }
    public RegisteredThreads();
    public void RegisterThread(BaseDataEffectThread dataEffectThread, IPlayer player);
    public void UnRegisterThread(string effectThreadName, IPlayer player);
    public bool ThreadIsExist(string ThreadName);
}

private class PlayersGUIData
{
    public Dictionary<string, PlayerGUIInfo> Players;
    public PlayersGUIData();
    public void ResetPlayerData(BasePlayer player);
    public void ResetOnlinePlayersData();
}

private class PlayerGUIInfo
{
    public string Id;
    public string Name;
    public bool useUI;
    public int EffectPage;
    public PlayerGUIInfo();
    public PlayerGUIInfo(IPlayer player);
}


```

---

## PermissionGroupSync by SenorCerveza - Synchronizes sets of groups and permissions across servers

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;
using Oxide.Core.Database;

Oxide.Plugins
[Info("Permission Group Sync", "Apec", "2.1.1")]
[Description("Synchronizes sets of Groups and Permissions with customizable Commands across multiple Servers")]
public class PermissionGroupSync : CovalencePlugin
{
    private const string PermissionGroupSyncPerm;
    private const string PermissionGroupSyncPermAdmin;
    private const string sql_table;
    private const string ServerIdAll;
    private readonly Core.MySql.Libraries.MySql _mySql;
    private Connection _mySqlConnection;
    private bool PluginLoaded;
     Configuration _cfg;
    private void InitializeConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private new void LoadDefaultMessages();
    private void Init();
     void OnServerInitialized();
    private void InitializeDatabase();
    private void RunTimer();
    private void Command_PermissionGroupSync(IPlayer player, string command, string[] args);
    private string Lang(string key, string userId);
    public void SyncPermissions();
    public bool SetUserPermissionRust(ulong steamIdNumber, int permissionRust);
    public bool CheckUserPermissionRust(ulong steamIdNumber, int permissionNumber);
    public bool CheckGroupExclude(GroupPermission groupPermission, string steamId);
    public ulong CheckUserSteamId(string steamId);
    public HashSet<string> GetCurrentUserIds(string groupNameSearch);
    public HashSet<string> GetCurrentUserGroups(string steamIdSearch);
    public void InitializeGroup(string groupName, HashSet<string> groupPermissions);
    public HashSet<string> RemoveGroups(GroupPermission groupPermission, string steamId);
    public void executeQuery(string query, object[] data);
    public void executeQuery(string query);
    private class Configuration
    {
        [JsonProperty(PropertyName = "ServerId")]
        public string ServerId { get; set; }
        [JsonProperty(PropertyName = "PollIntervalSeconds")]
        public int PollIntervalSeconds { get; set; }
        [JsonProperty(PropertyName = "DatabaseConfiguration")]
        public DatabaseConfiguration DatabaseConfiguration { get; set; }
        [JsonProperty(PropertyName = "GroupPermissions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<GroupPermission> GroupPermissions { get; set; }
    }

    public class DatabaseConfiguration
    {
        [JsonProperty(PropertyName = "Host")]
        public string Host { get; set; }
        [JsonProperty(PropertyName = "Port")]
        public int Port { get; set; }
        [JsonProperty(PropertyName = "Username")]
        public string Username { get; set; }
        [JsonProperty(PropertyName = "Password")]
        public string Password { get; set; }
        [JsonProperty(PropertyName = "Database")]
        public string Database { get; set; }
    }

    public class GroupPermission
    {
        [JsonProperty(PropertyName = "CommandName")]
        public string CommandName { get; set; }
        [JsonProperty(PropertyName = "GroupName")]
        public string GroupName { get; set; }
        [JsonProperty(PropertyName = "ExtendedPermissionHandling")]
        public bool ExtendedPermissionHandling { get; set; }
        [JsonProperty(PropertyName = "PermissionUse")]
        public bool PermissionUse { get; set; }
        [JsonProperty(PropertyName = "ProtectedGroup")]
        public bool ProtectedGroup { get; set; }
        [JsonProperty(PropertyName = "OverrideServerIdCheck")]
        public bool OverrideServerIdCheck { get; set; }
        [JsonProperty(PropertyName = "GroupsCheckExcludedSync")]
        public HashSet<string> GroupsCheckExcludedSync { get; set; }
        [JsonProperty(PropertyName = "GroupsRemove")]
        public HashSet<string> GroupsRemove { get; set; }
        [JsonProperty(PropertyName = "PermissionsRust")]
        public int PermissionRust { get; set; }
        [JsonProperty(PropertyName = "PermissionsOxide")]
        public HashSet<string> PermissionsOxide { get; set; }
        [JsonProperty(PropertyName = "AdditionalCommands")]
        public HashSet<string> AdditionalCommands { get; set; }
    }

    public class GroupPermissionDb
    {
        [JsonProperty(PropertyName = "SteamId")]
        public ulong SteamId { get; set; }
        [JsonProperty(PropertyName = "GroupName")]
        public string GroupName { get; set; }
        [JsonProperty(PropertyName = "ServerId")]
        public string ServerId { get; set; }
    }

    public class GroupPermissionRustDb
    {
        [JsonProperty(PropertyName = "SteamId")]
        public ulong SteamId { get; set; }
        [JsonProperty(PropertyName = "PermissionsRust")]
        public int PermissionRust { get; set; }
    }

}

private class Configuration
{
    [JsonProperty(PropertyName = "ServerId")]
    public string ServerId { get; set; }
    [JsonProperty(PropertyName = "PollIntervalSeconds")]
    public int PollIntervalSeconds { get; set; }
    [JsonProperty(PropertyName = "DatabaseConfiguration")]
    public DatabaseConfiguration DatabaseConfiguration { get; set; }
    [JsonProperty(PropertyName = "GroupPermissions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<GroupPermission> GroupPermissions { get; set; }
}

public class DatabaseConfiguration
{
    [JsonProperty(PropertyName = "Host")]
    public string Host { get; set; }
    [JsonProperty(PropertyName = "Port")]
    public int Port { get; set; }
    [JsonProperty(PropertyName = "Username")]
    public string Username { get; set; }
    [JsonProperty(PropertyName = "Password")]
    public string Password { get; set; }
    [JsonProperty(PropertyName = "Database")]
    public string Database { get; set; }
}

public class GroupPermission
{
    [JsonProperty(PropertyName = "CommandName")]
    public string CommandName { get; set; }
    [JsonProperty(PropertyName = "GroupName")]
    public string GroupName { get; set; }
    [JsonProperty(PropertyName = "ExtendedPermissionHandling")]
    public bool ExtendedPermissionHandling { get; set; }
    [JsonProperty(PropertyName = "PermissionUse")]
    public bool PermissionUse { get; set; }
    [JsonProperty(PropertyName = "ProtectedGroup")]
    public bool ProtectedGroup { get; set; }
    [JsonProperty(PropertyName = "OverrideServerIdCheck")]
    public bool OverrideServerIdCheck { get; set; }
    [JsonProperty(PropertyName = "GroupsCheckExcludedSync")]
    public HashSet<string> GroupsCheckExcludedSync { get; set; }
    [JsonProperty(PropertyName = "GroupsRemove")]
    public HashSet<string> GroupsRemove { get; set; }
    [JsonProperty(PropertyName = "PermissionsRust")]
    public int PermissionRust { get; set; }
    [JsonProperty(PropertyName = "PermissionsOxide")]
    public HashSet<string> PermissionsOxide { get; set; }
    [JsonProperty(PropertyName = "AdditionalCommands")]
    public HashSet<string> AdditionalCommands { get; set; }
}

public class GroupPermissionDb
{
    [JsonProperty(PropertyName = "SteamId")]
    public ulong SteamId { get; set; }
    [JsonProperty(PropertyName = "GroupName")]
    public string GroupName { get; set; }
    [JsonProperty(PropertyName = "ServerId")]
    public string ServerId { get; set; }
}

public class GroupPermissionRustDb
{
    [JsonProperty(PropertyName = "SteamId")]
    public ulong SteamId { get; set; }
    [JsonProperty(PropertyName = "PermissionsRust")]
    public int PermissionRust { get; set; }
}


```

---

## PermissionNotifications by klauz24 - Allows you to notify players when they get or lose some permission or group.

```csharp
using System.Linq;
using Newtonsoft.Json;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Permission Notifications", "klauz24", "1.0.5"), Description("Allows you to notify players when they get or lose some permission.")]
internal class PermissionNotifications : CovalencePlugin
{
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Granted (permission or group, lang key)")]
        public Dictionary<string, string> Granted;
        [JsonProperty(PropertyName = "Revoked (permission or group, lang key)")]
        public Dictionary<string, string> Revoked;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private void OnUserPermissionGranted(string id, string permName);
    private void OnUserPermissionRevoked(string id, string permName);
    private void OnUserGroupAdded(string id, string groupName);
    private void OnUserGroupRemoved(string id, string groupName);
    private void NotifyPlayer(Dictionary<string, string> dict, string id, string str, bool isPerm);
    private string GetLang(IPlayer player, string langKey);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Granted (permission or group, lang key)")]
    public Dictionary<string, string> Granted;
    [JsonProperty(PropertyName = "Revoked (permission or group, lang key)")]
    public Dictionary<string, string> Revoked;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## PermRewards by cymug - Gives players a kit-like reward if they're in a group

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Perm Rewards", "Ryan", "1.0.2")]
[Description("Gives players a kit-like reward if they're in an Oxide group")]
internal class PermRewards : RustPlugin
{
    private string Lang(string key, string id, object[] args);
    private new void LoadDefaultMessages();
    private static Cooldowns cooldowns;
    public class Cooldowns
    {
        public Dictionary<ulong, DateTime> PlayerCooldowns;
    }

    public class Data
    {
        public static void Add(BasePlayer player);
        public static bool Exists(BasePlayer player);
        public static void Remove(BasePlayer player);
        public static void Clear();
        public static void Save();
    }

    private ConfigFile _Config;
    public class Items
    {
        public string ItemName { get; set; }
        public int Amount { get; set; }
    }

    public class ConfigFile
    {
        [JsonProperty(PropertyName = "Item names and their amounts")]
        public List<Items> ItemInfo;
        [JsonProperty(PropertyName = "Command to use the reward")]
        public string Command;
        [JsonProperty(PropertyName = "Reward cooldown (seconds)")]
        public float Cooldown;
        [JsonProperty(PropertyName = "Enable the cooldown (true/false)")]
        public bool UseCooldown;
        [JsonProperty(PropertyName = "Oxide permission group")]
        public string OxideGroup;
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void GiveItem(BasePlayer player);
    public double GetCooldown(BasePlayer player);
    private string GetFormattedMsg(double cooldown);
    private void OnNewSave(string filename);
    private void Loaded();
    private bool CanUse(BasePlayer player);
    private void chatCmd(BasePlayer player, string command, string[] args);
}

public class Cooldowns
{
    public Dictionary<ulong, DateTime> PlayerCooldowns;
}

public class Data
{
    public static void Add(BasePlayer player);
    public static bool Exists(BasePlayer player);
    public static void Remove(BasePlayer player);
    public static void Clear();
    public static void Save();
}

public class Items
{
    public string ItemName { get; set; }
    public int Amount { get; set; }
}

public class ConfigFile
{
    [JsonProperty(PropertyName = "Item names and their amounts")]
    public List<Items> ItemInfo;
    [JsonProperty(PropertyName = "Command to use the reward")]
    public string Command;
    [JsonProperty(PropertyName = "Reward cooldown (seconds)")]
    public float Cooldown;
    [JsonProperty(PropertyName = "Enable the cooldown (true/false)")]
    public bool UseCooldown;
    [JsonProperty(PropertyName = "Oxide permission group")]
    public string OxideGroup;
    public static ConfigFile DefaultConfig();
}


```

---

## PermsUI by Camoec - Useful for managing permissions.

```csharp
using UnityEngine;
using System.Collections.Generic;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("Perms UI", "Camoec", 1.3)]
[Description("GUI for managing plugin permissions in-game")]
public class PermsUI : RustPlugin
{
    private const string UsePerm;
    private void Init();
    private void MainUI(BasePlayer player, string userID, bool user, int page);
    [ConsoleCommand("perms")]
    private void GivePerm(ConsoleSystem.Arg arg);
    [ChatCommand("perms")]
    private void permsCommand(BasePlayer player, string command, string[] args);
    protected override void LoadDefaultMessages();
    private string Lang(string key, string userid);
}


```

---

## PersonalBeacon by  - Displays a beacon at a marked location for easier navigation

```csharp
using System.Collections.Generic;
using System;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Configuration;
using System.Text;

Oxide.Plugins
[Info("PersonalBeacon", "redBDGR", "2.0.4")]
[Description("Displays a beacon at a marked location for easier navigation")]
 class PersonalBeacon : RustPlugin
{
    private DynamicConfigFile exampleData;
     WaypointDataStorage storedData;
     void SaveData();
     void LoadData();
     void LoadPlayerWaypoints();
     void LoadGlobalWaypoints();
    protected override void LoadDefaultConfig();
     void LoadVariables();
     bool Changed;
    private const string permissionName;
    private const string permissionNameADMIN;
     Dictionary<string, List<WaypointData>> playerWaypointsCache;
     List<WaypointData> globalWaypointCache;
     List<TimerData> timers;
    public float arrowHeight;
    public float arrowLevitation;
    public float arrowHeadSize;
    public float playerWaypointDisplaytime;
    public float globalRefreshTime;
    public int maxWaypoints;
     class WaypointDataStorage
    {
        public Dictionary<string, List<WaypointData>> playerWaypoints;
        public List<WaypointData> globalWaypoints;
    }

     class WaypointData
    {
        public string name;
        public float x;
        public float y;
        public float z;
    }

     class TimerData
    {
        public string name;
        public Timer timer;
        public WaypointData data;
    }

     void Loaded();
     void OnServerInitialized();
     void Unload();
     void OnServerSave();
     void Init();
    [ChatCommand("setwp")]
     void setwpCMD(BasePlayer player, string command, string[] args);
    [ChatCommand("wp")]
     void wpCMD(BasePlayer player, string command, string[] args);
    [ChatCommand("removewp")]
     void removewp(BasePlayer player, string command, string[] args);
    [ChatCommand("wplist")]
     void wplistCMD(BasePlayer player, string command, string[] args);
    [ChatCommand("setglobalwp")]
     void globalwpCMD(BasePlayer player, string command, string[] args);
    [ChatCommand("removeglobalwp")]
    private void RemoveGlobalWPCMD(BasePlayer player, string command, string[] args);
    [ChatCommand("hideglobalwp")]
     void hideglobalwpCMD(BasePlayer player, string command, string[] args);
    [ChatCommand("showglobalwp")]
     void showglobalwpCMD(BasePlayer player, string command, string[] args);
    [ChatCommand("wpshowall")]
     void cmdShowAllBeacons(BasePlayer player, string command, string[] args);
     void InitGlobalWaypoint(WaypointData data);
     void DrawWaypoint(BasePlayer player, Vector3 pos, float time, bool isGlobal, string name);
     object GetConfig(string menu, string datavalue, object defaultValue);
     string msg(string key, string id);
}

 class WaypointDataStorage
{
    public Dictionary<string, List<WaypointData>> playerWaypoints;
    public List<WaypointData> globalWaypoints;
}

 class WaypointData
{
    public string name;
    public float x;
    public float y;
    public float z;
}

 class TimerData
{
    public string name;
    public Timer timer;
    public WaypointData data;
}


```

---

## PersonalHeli by rostov114 - Calls heli for player and his friends/team and attacks only them, loot can be taken only by them

```csharp
using System.Collections.Generic;
using System;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;
using System.Text;
using System.Linq;
using Facepunch;
using Rust;

Oxide.Plugins
[Info("Personal Heli", "Egor Blagov", "1.1.11")]
[Description("Calls heli to player and his team, with loot/damage and minig lock")]
 class PersonalHeli : RustPlugin
{
    const string permUse;
    const string permConsole;
    const float HelicopterEntitySpawnRadius;
    [PluginReference]
     Plugin Friends;
     Plugin Clans;
     class PluginConfig
    {
        public bool UseFriends;
        public bool UseTeams;
        public bool UseClans;
        public int CooldownSeconds;
        public string ChatCommand;
        public bool ResetCooldownsOnWipe;
        public bool MemorizeTeamOnCall;
        public bool RetireOnAllTeamDead;
        public bool DenyCratesLooting;
        public bool DenyGibsMining;
        public bool RemoveFireFromCrates;
    }

    private PluginConfig config;
     class StoredData
    {
        public Dictionary<ulong, CallData> CallDatas;
        public class CallData
        {
            public DateTime LastCall;
            public bool CanCallNow(int cooldown);
            public int SecondsToWait(int cooldown);
            public void OnCall();
        }

        public CallData GetForPlayer(BasePlayer player);
    }

    private void SaveData();
    private void LoadData();
    private StoredData storedData;
    protected override void LoadDefaultMessages();
    private string _(string key, string userId, object[] args);
    private void Init();
    private void Unload();
    protected override void LoadDefaultConfig();
    private void OnNewSave();
    private void OnServerSave();
    private void OnEntityKill(BaseEntity entity);
    private object CanLootEntity(BasePlayer player, StorageContainer container);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private object OnHelicopterTarget(HelicopterTurret turret, BaseCombatEntity entity);
    private object OnPlayerAttack(BasePlayer attacker, HitInfo info);
    private object CanHelicopterStrafeTarget(PatrolHelicopterAI heliAi, BasePlayer target);
    private object CanHelicopterTarget(PatrolHelicopterAI heliAi, BasePlayer player);
    private bool CallHeliForPlayer(BasePlayer player);
    private bool IsPersonal(PatrolHelicopter heli);
    [ConsoleCommand("personalheli.call")]
    private void CmdCallHeliConsole(ConsoleSystem.Arg arg);
    private void CmdCallHeli(BasePlayer player, string cmd, string[] argv);
    private string GetPlayerOwnerDescription(BasePlayer player, BasePlayer playerOwner);
    private T InvokePersonal(GameObject obj, Func<C, T> action);
    private void InvokePersonal(GameObject obj, Action<C> action);
    abstract class PersonalComponent : FacepunchBehaviour
    {
        protected PersonalHeli Plugin;
        protected PluginConfig Config { get; set; }
        public List<BasePlayer> SavedTeam;
        public BasePlayer Player;
        public void Init(PersonalHeli plugin, BasePlayer player);
        protected virtual void OnInitChild();
        public virtual bool CanInterractWith(BaseEntity target);
        protected bool AreSameClan(BasePlayer basePlayer);
        protected bool AreSameTeam(BasePlayer otherPlayer);
        protected bool AreFriends(BasePlayer otherPlayer);
        private void OnDestroy();
        protected virtual void OnDestroyChild();
    }

     class PersonalCrateComponent : PersonalComponent
    {
        private StorageContainer Crate;
        private void Awake();
        protected override void OnDestroyChild();
    }

     class PersonalGibComponent : PersonalComponent
    {
        private HelicopterDebris Gib;
        private void Awake();
        protected override void OnDestroyChild();
    }

     class PersonalHeliComponent : PersonalComponent
    {
        private const int MaxHeliDistanceToPlayer;
        public static List<PersonalHeliComponent> ActiveHelis;
        private PatrolHelicopter Heli;
        private PatrolHelicopterAI HeliAi { get; set; }
        private void Awake();
        protected override void OnInitChild();
        private void UpdateTargets();
        protected override void OnDestroyChild();
        private List<BasePlayer> GetAllPlayersInTeam();
        public void OnKill();
        public void OnPlayerDied(BasePlayer player);
    }

}

 class PluginConfig
{
    public bool UseFriends;
    public bool UseTeams;
    public bool UseClans;
    public int CooldownSeconds;
    public string ChatCommand;
    public bool ResetCooldownsOnWipe;
    public bool MemorizeTeamOnCall;
    public bool RetireOnAllTeamDead;
    public bool DenyCratesLooting;
    public bool DenyGibsMining;
    public bool RemoveFireFromCrates;
}

 class StoredData
{
    public Dictionary<ulong, CallData> CallDatas;
    public class CallData
    {
        public DateTime LastCall;
        public bool CanCallNow(int cooldown);
        public int SecondsToWait(int cooldown);
        public void OnCall();
    }

    public CallData GetForPlayer(BasePlayer player);
}

public class CallData
{
    public DateTime LastCall;
    public bool CanCallNow(int cooldown);
    public int SecondsToWait(int cooldown);
    public void OnCall();
}

abstract class PersonalComponent : FacepunchBehaviour
{
    protected PersonalHeli Plugin;
    protected PluginConfig Config { get; set; }
    public List<BasePlayer> SavedTeam;
    public BasePlayer Player;
    public void Init(PersonalHeli plugin, BasePlayer player);
    protected virtual void OnInitChild();
    public virtual bool CanInterractWith(BaseEntity target);
    protected bool AreSameClan(BasePlayer basePlayer);
    protected bool AreSameTeam(BasePlayer otherPlayer);
    protected bool AreFriends(BasePlayer otherPlayer);
    private void OnDestroy();
    protected virtual void OnDestroyChild();
}

 class PersonalCrateComponent : PersonalComponent
{
    private StorageContainer Crate;
    private void Awake();
    protected override void OnDestroyChild();
}

 class PersonalGibComponent : PersonalComponent
{
    private HelicopterDebris Gib;
    private void Awake();
    protected override void OnDestroyChild();
}

 class PersonalHeliComponent : PersonalComponent
{
    private const int MaxHeliDistanceToPlayer;
    public static List<PersonalHeliComponent> ActiveHelis;
    private PatrolHelicopter Heli;
    private PatrolHelicopterAI HeliAi { get; set; }
    private void Awake();
    protected override void OnInitChild();
    private void UpdateTargets();
    protected override void OnDestroyChild();
    private List<BasePlayer> GetAllPlayersInTeam();
    public void OnKill();
    public void OnPlayerDied(BasePlayer player);
}


```

---

## Pets by k1lly0u - Gives players the ability to tame and have pets

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;
using UnityEngine.SceneManagement;

Oxide.Plugins
[Info("Pets", "Nogrod/k1lly0u", "0.6.5"), Description("Gives players the ability to tame and have pets")]
 class Pets : RustPlugin
{
    private static Pets ins;
    private BUTTON Main;
    private BUTTON Secondary;
    private Dictionary<ulong, PetData> npcSaveList;
    private void OnServerInitialized();
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo hitInfo);
    private void OnPlayerConnected(BasePlayer player);
    private object OnNpcAttack(BaseNpc entity, BaseEntity target);
    private object OnNpcTarget(BaseNpc entity, BaseEntity target);
    private object CanNpcEat(BaseNpc entity, BaseEntity target);
    private void OnServerSave();
    private void Unload();
    private object CanAnimalAttack(BaseNpc entity, BasePlayer player);
    private object CanNPCEat(BaseNpc entity, BaseEntity target);
    private void RegisterPermissions();
    private BUTTON ConvertToButton(string button);
    private void DestroyAll();
    private string FormatTime(double time);
    private BaseEntity InstantiateEntity(string type, Vector3 position, Quaternion rotation);
    private class NPCController : MonoBehaviour
    {
        public BasePlayer player;
        public NpcAI npcAi;
        private float nextPressTime;
        private float nextDrawTime;
        private float nextControlTime;
        private float lootDistance;
        private float tameTimer;
        private float lastTpTime;
        private bool usePermissions;
        internal bool drawEnabled;
        private void Awake();
        private void Update();
        private void UpdateDraw();
        private void UpdateAction();
        private void OpenPetInventory();
        private void ChangeFollowAction();
        internal void TeleportToPlayer();
        private void TryGetNewPet(BaseNpc npcPet);
    }

    private class NpcAI : MonoBehaviour
    {
        internal Action action;
        internal Vector3 targetPos;
        internal BaseCombatEntity targetEnt;
        internal bool stopFollow;
        public NPCController owner;
        public ItemContainer inventory;
        public BaseNpc entity;
        private double attackRange;
        private float targetIgnoreDistance;
        private float healthModifier;
        private float speedModifier;
        private float attackModifier;
        private float nextMessageTime;
        private void Awake();
        private void OnDestroy();
        internal void OnAttacked(HitInfo info);
        internal void OnDeath();
        private void Update();
        private void UpdateDestination(Vector3 position, bool run);
        private void StopMoving();
        private void SetBehaviour(BaseNpc.Behaviour behaviour);
        internal void Attack(BaseCombatEntity targetEnt, Action action);
    }

    [ChatCommand("pet")]
    private void pet(BasePlayer player, string command, string[] args);
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Pet statistic modifiers")]
        public Modifiers NPCMods { get; set; }
        [JsonProperty(PropertyName = "Control buttons")]
        public Controls UserControl { get; set; }
        public OtherOptions Options { get; set; }
        public class Modifiers
        {
            [JsonProperty(PropertyName = "Attack damage")]
            public float Attack { get; set; }
            public float Health { get; set; }
            public float Speed { get; set; }
        }

        public class Controls
        {
            [JsonProperty(PropertyName = "Npc command control button")]
            public string Main { get; set; }
            [JsonProperty(PropertyName = "Follow player toggle button")]
            public string Secondary { get; set; }
        }

        public class OtherOptions
        {
            [JsonProperty(PropertyName = "Maximum distance to tame an pet")]
            public float TameDistance { get; set; }
            [JsonProperty(PropertyName = "Teleport to player cooldown (seconds)")]
            public int TpCooldown { get; set; }
            [JsonProperty(PropertyName = "Time between taming pets")]
            public float TameTimer { get; set; }
            [JsonProperty(PropertyName = "Maximum distance to open your pets inventory")]
            public float LootDistance { get; set; }
            [JsonProperty(PropertyName = "Maximum distance before your pet will ignore a target")]
            public float AttackDistance { get; set; }
            [JsonProperty(PropertyName = "Use the permission system")]
            public bool UsePermissions { get; set; }
            [JsonProperty(PropertyName = "Use the Ddraw system")]
            public bool UseDrawSystem { get; set; }
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
     void SaveData();
     void LoadData();
     class PetData
    {
        public uint prefabID;
        public float x;
        public float y;
        public float z;
        public byte[] inventory;
        internal bool NeedToSpawn;
        public PetData();
        public PetData(NpcAI pet);
    }

    static void UserMessage(BasePlayer player, string key, string[] args);
     Dictionary<string, string> Messages;
}

private class NPCController : MonoBehaviour
{
    public BasePlayer player;
    public NpcAI npcAi;
    private float nextPressTime;
    private float nextDrawTime;
    private float nextControlTime;
    private float lootDistance;
    private float tameTimer;
    private float lastTpTime;
    private bool usePermissions;
    internal bool drawEnabled;
    private void Awake();
    private void Update();
    private void UpdateDraw();
    private void UpdateAction();
    private void OpenPetInventory();
    private void ChangeFollowAction();
    internal void TeleportToPlayer();
    private void TryGetNewPet(BaseNpc npcPet);
}

private class NpcAI : MonoBehaviour
{
    internal Action action;
    internal Vector3 targetPos;
    internal BaseCombatEntity targetEnt;
    internal bool stopFollow;
    public NPCController owner;
    public ItemContainer inventory;
    public BaseNpc entity;
    private double attackRange;
    private float targetIgnoreDistance;
    private float healthModifier;
    private float speedModifier;
    private float attackModifier;
    private float nextMessageTime;
    private void Awake();
    private void OnDestroy();
    internal void OnAttacked(HitInfo info);
    internal void OnDeath();
    private void Update();
    private void UpdateDestination(Vector3 position, bool run);
    private void StopMoving();
    private void SetBehaviour(BaseNpc.Behaviour behaviour);
    internal void Attack(BaseCombatEntity targetEnt, Action action);
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Pet statistic modifiers")]
    public Modifiers NPCMods { get; set; }
    [JsonProperty(PropertyName = "Control buttons")]
    public Controls UserControl { get; set; }
    public OtherOptions Options { get; set; }
    public class Modifiers
    {
        [JsonProperty(PropertyName = "Attack damage")]
        public float Attack { get; set; }
        public float Health { get; set; }
        public float Speed { get; set; }
    }

    public class Controls
    {
        [JsonProperty(PropertyName = "Npc command control button")]
        public string Main { get; set; }
        [JsonProperty(PropertyName = "Follow player toggle button")]
        public string Secondary { get; set; }
    }

    public class OtherOptions
    {
        [JsonProperty(PropertyName = "Maximum distance to tame an pet")]
        public float TameDistance { get; set; }
        [JsonProperty(PropertyName = "Teleport to player cooldown (seconds)")]
        public int TpCooldown { get; set; }
        [JsonProperty(PropertyName = "Time between taming pets")]
        public float TameTimer { get; set; }
        [JsonProperty(PropertyName = "Maximum distance to open your pets inventory")]
        public float LootDistance { get; set; }
        [JsonProperty(PropertyName = "Maximum distance before your pet will ignore a target")]
        public float AttackDistance { get; set; }
        [JsonProperty(PropertyName = "Use the permission system")]
        public bool UsePermissions { get; set; }
        [JsonProperty(PropertyName = "Use the Ddraw system")]
        public bool UseDrawSystem { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class Modifiers
{
    [JsonProperty(PropertyName = "Attack damage")]
    public float Attack { get; set; }
    public float Health { get; set; }
    public float Speed { get; set; }
}

public class Controls
{
    [JsonProperty(PropertyName = "Npc command control button")]
    public string Main { get; set; }
    [JsonProperty(PropertyName = "Follow player toggle button")]
    public string Secondary { get; set; }
}

public class OtherOptions
{
    [JsonProperty(PropertyName = "Maximum distance to tame an pet")]
    public float TameDistance { get; set; }
    [JsonProperty(PropertyName = "Teleport to player cooldown (seconds)")]
    public int TpCooldown { get; set; }
    [JsonProperty(PropertyName = "Time between taming pets")]
    public float TameTimer { get; set; }
    [JsonProperty(PropertyName = "Maximum distance to open your pets inventory")]
    public float LootDistance { get; set; }
    [JsonProperty(PropertyName = "Maximum distance before your pet will ignore a target")]
    public float AttackDistance { get; set; }
    [JsonProperty(PropertyName = "Use the permission system")]
    public bool UsePermissions { get; set; }
    [JsonProperty(PropertyName = "Use the Ddraw system")]
    public bool UseDrawSystem { get; set; }
}

 class PetData
{
    public uint prefabID;
    public float x;
    public float y;
    public float z;
    public byte[] inventory;
    internal bool NeedToSpawn;
    public PetData();
    public PetData(NpcAI pet);
}


```

---

## PhantomSleepers by nivex - Create phantom sleepers to use as bait against suspected cheaters

```csharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Reflection;
using UnityEngine;

Oxide.Plugins
[Info("Phantom Sleepers", "nivex", "1.0.0")]
[Description("Create phantom sleepers to use as bait against suspected cheaters.")]
 class PhantomSleepers : RustPlugin
{
    private const string playerPrefab;
    private const string permissionName;
    private const ulong phantomId;
    private Dictionary<ulong, Phantom> phantoms;
    private class Phantom : BasePlayer
    {
        internal BasePlayer phantom;
        internal BasePlayer sleeper;
        internal PhantomSleepers _instance;
        internal List<Item> items;
        internal Vector3 spawnPos;
        internal bool Invisibility;
        internal bool NoLooting;
        internal bool NoCorpse;
        internal bool NoCorpseLoot;
        internal ulong networkId;
        internal float nextUpdateCheck;
        private void Awake();
        private void OnDestroy();
        private void FixedUpdate();
        public void EntityDestroyOnClient(BasePlayer target);
        public void SendShouldNetworkToUpdateImmediate();
        public override bool ShouldDropActiveItem();
        public override bool CanBeLooted(BasePlayer player);
        public override void OnDied(HitInfo info);
        public override BaseCorpse CreateCorpse(PlayerFlags flagsOnDeath, Vector3 posOnDeath, Quaternion rotOnDeath, List<TriggerBase> triggersOnDeath, bool forceServerSide);
    }

    private void ccmdCreatePhantom(ConsoleSystem.Arg arg);
    private void OnServerInitialized();
    private void Unload();
    private void Equip(Phantom phantom);
    private void EquipItem(Phantom phantom, List<string> itemNames);
    private Configuration config;
    private class Configuration
    {
        [JsonProperty("Invisibility")]
        public InvisibilityConfig Invisibility { get; set; }
        [JsonProperty("Settings")]
        public SettingsConfig Settings { get; set; }
        [JsonProperty("Gear")]
        public GearConfig Gear { get; set; }
    }

    private class InvisibilityConfig
    {
        [JsonProperty("Hide Real Sleepers")]
        public bool HideRealSleepers { get; set; }
    }

    private class SettingsConfig
    {
        [JsonProperty("Random Names", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> RandomNames { get; set; }
        [JsonProperty("Strip Phantoms On Death")]
        public bool StripPhantomsOnDeath { get; set; }
        [JsonProperty("Destroy Phantom Corpses")]
        public bool DestroyPhantomCorpses { get; set; }
        [JsonProperty("Prevent Phantom Looting")]
        public bool PreventPhantomLooting { get; set; }
        [JsonProperty("Console Command")]
        public string ConsoleCommand { get; set; }
        [JsonProperty("Max Raycast Distance")]
        public float MaxRaycastDistance { get; set; }
        [JsonProperty("Min Distance From Real Sleeper")]
        public float MinDistanceFromRealSleeper { get; set; }
        [JsonProperty("Default Name If No Sleepers")]
        public string DefaultNameIfNoSleepers { get; set; }
        [JsonProperty("Phantoms Spawn Sleeping")]
        public bool PhantomsSpawnSleeping { get; set; }
        [JsonProperty("Random Starting Health")]
        public bool RandomHealth { get; set; }
    }

    private class GearConfig
    {
        [JsonProperty("Shirts", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Shirts { get; set; }
        [JsonProperty("Pants", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Pants { get; set; }
        [JsonProperty("Helms", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Helms { get; set; }
        [JsonProperty("Vests", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Vests { get; set; }
        [JsonProperty("Gloves", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Gloves { get; set; }
        [JsonProperty("Boots", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Boots { get; set; }
        [JsonProperty("Weapons", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Weapons { get; set; }
    }

    protected override void LoadConfig();
    private bool canSaveConfig;
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private string LangAPI(string key, string id, object[] args);
}

private class Phantom : BasePlayer
{
    internal BasePlayer phantom;
    internal BasePlayer sleeper;
    internal PhantomSleepers _instance;
    internal List<Item> items;
    internal Vector3 spawnPos;
    internal bool Invisibility;
    internal bool NoLooting;
    internal bool NoCorpse;
    internal bool NoCorpseLoot;
    internal ulong networkId;
    internal float nextUpdateCheck;
    private void Awake();
    private void OnDestroy();
    private void FixedUpdate();
    public void EntityDestroyOnClient(BasePlayer target);
    public void SendShouldNetworkToUpdateImmediate();
    public override bool ShouldDropActiveItem();
    public override bool CanBeLooted(BasePlayer player);
    public override void OnDied(HitInfo info);
    public override BaseCorpse CreateCorpse(PlayerFlags flagsOnDeath, Vector3 posOnDeath, Quaternion rotOnDeath, List<TriggerBase> triggersOnDeath, bool forceServerSide);
}

private class Configuration
{
    [JsonProperty("Invisibility")]
    public InvisibilityConfig Invisibility { get; set; }
    [JsonProperty("Settings")]
    public SettingsConfig Settings { get; set; }
    [JsonProperty("Gear")]
    public GearConfig Gear { get; set; }
}

private class InvisibilityConfig
{
    [JsonProperty("Hide Real Sleepers")]
    public bool HideRealSleepers { get; set; }
}

private class SettingsConfig
{
    [JsonProperty("Random Names", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> RandomNames { get; set; }
    [JsonProperty("Strip Phantoms On Death")]
    public bool StripPhantomsOnDeath { get; set; }
    [JsonProperty("Destroy Phantom Corpses")]
    public bool DestroyPhantomCorpses { get; set; }
    [JsonProperty("Prevent Phantom Looting")]
    public bool PreventPhantomLooting { get; set; }
    [JsonProperty("Console Command")]
    public string ConsoleCommand { get; set; }
    [JsonProperty("Max Raycast Distance")]
    public float MaxRaycastDistance { get; set; }
    [JsonProperty("Min Distance From Real Sleeper")]
    public float MinDistanceFromRealSleeper { get; set; }
    [JsonProperty("Default Name If No Sleepers")]
    public string DefaultNameIfNoSleepers { get; set; }
    [JsonProperty("Phantoms Spawn Sleeping")]
    public bool PhantomsSpawnSleeping { get; set; }
    [JsonProperty("Random Starting Health")]
    public bool RandomHealth { get; set; }
}

private class GearConfig
{
    [JsonProperty("Shirts", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Shirts { get; set; }
    [JsonProperty("Pants", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Pants { get; set; }
    [JsonProperty("Helms", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Helms { get; set; }
    [JsonProperty("Vests", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Vests { get; set; }
    [JsonProperty("Gloves", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Gloves { get; set; }
    [JsonProperty("Boots", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Boots { get; set; }
    [JsonProperty("Weapons", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Weapons { get; set; }
}


```

---

## PhoneCommands by marcuzz - Bind commands to phone numbers

```csharp
using Facepunch;
using Oxide.Core;
using ProtoBuf;
using System;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using UnityEngine;

Oxide.Plugins
[Info("Phone Commands", "marcuzz", "0.0.10")]
[Description("Bind commands to phone numbers")]
public class PhoneCommands : CovalencePlugin
{
    private static PluginConfig _config;
    private static ListDictionary<int, PhoneCommand> _phoneCommands;
    private static StoredData _storedData;
    private static Regex _chatCommandRegex;
    private static Dictionary<string, int> _blockedChatCommands;
    private static Dictionary<ulong, int> _allowedCommands;
     void Init();
     void OnServerInitialized();
     void Unload();
     object OnPlayerCommand(BasePlayer player, string command, string[] args);
     object OnPhoneDial(PhoneController callerPhone, PhoneController receiverPhone, BasePlayer player);
     void OnEntitySpawned(MobilePhone phone);
     void OnEntitySpawned(Telephone phone);
     void OnNewSave();
    private void RegisterPhoneComands();
    private void RegisterCommand(PhoneCommandConfig command, Telephone telephone, Dictionary<int, PhoneController> phones);
    private void SetBlockedCommands(PhoneCommandConfig command);
    private static bool SpawnPhone(Telephone telephone);
    private static bool IsConfigurationValid(PhoneCommandConfig command);
    private void AddCommandsToContactList(PhoneController phone);
    private static void AddCommandToContactList(PhoneController controller, PhoneCommandConfig command);
    private void RestrictedDevice(BasePlayer player);
    private bool CheckCooldown(PhoneCommand command, BasePlayer player, string message);
    private bool CheckPermission(PhoneCommand command, BasePlayer player);
    private void RunCommand(BasePlayer player, PhoneCommand command);
    private void PlayEffect(BasePlayer player, bool failed);
    private static class CommandActionType
    {
        public const string ServerCommand;
        public const string PlayerChat;
        public const string Broadcast;
        public const string Reply;
    }

    private static PluginConfig GetDefaultConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void UpdateConfig();
    private class PluginConfig
    {
        public bool EffectEnabled { get; set; }
        public string EffectSuccess { get; set; }
        public string EffectFailed { get; set; }
        public bool AddCommandsToContactList { get; set; }
        public bool BlockChatCommandsBoundToPhoneNumber { get; set; }
        public List<PhoneCommandConfig> PhoneCommands { get; set; }
    }

    private class StoredData
    {
        public List<int> UsedNumbers;
        public Dictionary<ulong, Dictionary<int, long>> PlayerCooldowns;
    }

    private void SaveData();
    protected override void LoadDefaultMessages();
}

private static class CommandActionType
{
    public const string ServerCommand;
    public const string PlayerChat;
    public const string Broadcast;
    public const string Reply;
}

private class PluginConfig
{
    public bool EffectEnabled { get; set; }
    public string EffectSuccess { get; set; }
    public string EffectFailed { get; set; }
    public bool AddCommandsToContactList { get; set; }
    public bool BlockChatCommandsBoundToPhoneNumber { get; set; }
    public List<PhoneCommandConfig> PhoneCommands { get; set; }
}

private class StoredData
{
    public List<int> UsedNumbers;
    public Dictionary<ulong, Dictionary<int, long>> PlayerCooldowns;
}


```

---

## PhoneRename by Bazz3l - Ability to rename naughty named phones and log changes to Discord

```csharp
using System.Text.RegularExpressions;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using Oxide.Core.Libraries;

Oxide.Plugins
[Info("Phone Rename", "Bazz3l", "0.0.4")]
[Description("Ability to rename naughty named phones and changes to discord.")]
public class PhoneRename : CovalencePlugin
{
    [PluginReference]
    private Plugin DiscordMessages;
    private const string PermUse;
    private readonly Dictionary<string, string> _headers;
    private PluginConfig _pluginConfig;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private class PluginConfig
    {
        [JsonProperty("DiscordWebhook (discord webhook url here)")]
        public string DiscordWebhook;
        [JsonProperty("DiscordColor (discord embed color)")]
        public int DiscordColor;
        [JsonProperty("DiscordAuthor (discord embed author name)")]
        public string DiscordAuthor;
        [JsonProperty("DiscordAuthorImageUrl (discord embed author image url)")]
        public string DiscordAuthorImageUrl;
        [JsonProperty("DiscordAuthorUrl (discord embed author url)")]
        public string DiscordAuthorUrl;
        [JsonProperty("LogToDiscord (log updated phone names to a discord channel)")]
        public bool LogToDiscord;
        [JsonProperty("WordList (list of ad words)")]
        public List<string> WordList;
    }

    protected override void LoadDefaultMessages();
    private void Init();
    private object OnPhoneNameUpdate(PhoneController phoneController, string phoneName, BasePlayer player);
    private object UpdatePhoneName(IPlayer player, PhoneController phoneController, string phoneName);
    private string FilterWord(string phoneName);
    private Telephone FindByPhoneNumber(int phoneNumber);
    [Command("renamephone")]
    private void RenamePhoneCommand(IPlayer player, string command, string[] args);
    private void SendDiscordMessage(IPlayer player, string phoneName, string phoneNumber);
    private class DiscordMessage
    {
        [JsonProperty("content")]
        public string Content { get; set; }
        [JsonProperty("embeds")]
        public List<Embed> Embeds { get; set; }
        public DiscordMessage(string content, List<Embed> embeds);
        public string ToJson();
    }

    private class Embed
    {
        [JsonProperty("author")]
        public Author Author;
        [JsonProperty("title")]
        public string Title { get; set; }
        [JsonProperty("description")]
        public string Description { get; set; }
        [JsonProperty("color")]
        public int Color { get; set; }
        [JsonProperty("fields")]
        public List<Field> Fields { get; set; }
    }

    private class Author
    {
        [JsonProperty("icon_url")]
        public string IconUrl;
        [JsonProperty("name")]
        public string Name;
        [JsonProperty("url")]
        public string Url;
    }

    private class Field
    {
        public Field(string name, string value, bool inline);
        [JsonProperty("name")]
        public string Name { get; set; }
        [JsonProperty("value")]
        public string Value { get; set; }
        [JsonProperty("inline")]
        public bool Inline { get; set; }
    }

    private string Lang(string key, string id, object[] args);
}

private class PluginConfig
{
    [JsonProperty("DiscordWebhook (discord webhook url here)")]
    public string DiscordWebhook;
    [JsonProperty("DiscordColor (discord embed color)")]
    public int DiscordColor;
    [JsonProperty("DiscordAuthor (discord embed author name)")]
    public string DiscordAuthor;
    [JsonProperty("DiscordAuthorImageUrl (discord embed author image url)")]
    public string DiscordAuthorImageUrl;
    [JsonProperty("DiscordAuthorUrl (discord embed author url)")]
    public string DiscordAuthorUrl;
    [JsonProperty("LogToDiscord (log updated phone names to a discord channel)")]
    public bool LogToDiscord;
    [JsonProperty("WordList (list of ad words)")]
    public List<string> WordList;
}

private class DiscordMessage
{
    [JsonProperty("content")]
    public string Content { get; set; }
    [JsonProperty("embeds")]
    public List<Embed> Embeds { get; set; }
    public DiscordMessage(string content, List<Embed> embeds);
    public string ToJson();
}

private class Embed
{
    [JsonProperty("author")]
    public Author Author;
    [JsonProperty("title")]
    public string Title { get; set; }
    [JsonProperty("description")]
    public string Description { get; set; }
    [JsonProperty("color")]
    public int Color { get; set; }
    [JsonProperty("fields")]
    public List<Field> Fields { get; set; }
}

private class Author
{
    [JsonProperty("icon_url")]
    public string IconUrl;
    [JsonProperty("name")]
    public string Name;
    [JsonProperty("url")]
    public string Url;
}

private class Field
{
    public Field(string name, string value, bool inline);
    [JsonProperty("name")]
    public string Name { get; set; }
    [JsonProperty("value")]
    public string Value { get; set; }
    [JsonProperty("inline")]
    public bool Inline { get; set; }
}


```

---

## PhonesPlus by mr01sam - Adds notifications, messages, caller ID and more features to phones

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Phones Plus", "mr01sam", "1.0.2")]
[Description("Adds notifications, messages, caller ID and more features to phones.")]
public class PhonesPlus : CovalencePlugin
{
    private static PhonesPlus PLUGIN;
    private const string PermissionUse;
    private const string PermissionAdmin;
    private const string PermissionTemp;
    [PluginReference]
     Plugin ImageLibrary;
     PhoneManager phoneManager;
    protected override void LoadDefaultMessages();
    private Configuration config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Enable incoming call notifications (true/false)")]
        public bool EnableIncomingCallNotifications { get; set; }
        [JsonProperty(PropertyName = "Hourly message limit")]
        public int HourlyMessageLimit { get; set; }
        [JsonProperty(PropertyName = "HUD Notification")]
        public NotificationHUD HudNotification;
        [JsonProperty(PropertyName = "HUD Call Indicator")]
        public IndicatorHUD HudCallIndicator;
        [JsonProperty(PropertyName = "HUD Message Indicator")]
        public IndicatorHUD HudMessageIndicator;
        public class NotificationHUD
        {
            [JsonProperty(PropertyName = "Anchor min")]
            public string AnchorMin;
            [JsonProperty(PropertyName = "Anchor max")]
            public string AnchorMax;
            [JsonProperty(PropertyName = "Background color")]
            public string BackgroundColor;
        }

        public class IndicatorHUD
        {
            [JsonProperty(PropertyName = "Anchor min")]
            public string AnchorMin;
            [JsonProperty(PropertyName = "Anchor max")]
            public string AnchorMax;
            [JsonProperty(PropertyName = "Image color")]
            public string ImageColor;
        }

        public class HistoryUI
        {
            [JsonProperty(PropertyName = "Entry background color")]
            public string EntryBackgroundColor;
            [JsonProperty(PropertyName = "Font color")]
            public string FontColor;
        }

        public class MessagesUI
        {
            [JsonProperty(PropertyName = "Entry background color")]
            public string EntryBackgroundColor;
        }

    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private Dictionary<string, ChatCmd> commands;
     void InitCommands();
     class ChatCmd
    {
        public string prefix;
        public List<string> usages;
        public List<string> perms;
        public Action<IPlayer, string, string[]> function;
        public bool HasPerms(string id, PhonesPlus plugin);
        public string Usage(string prefix);
    }

    [Command("phone")]
    private void cmd_phone(IPlayer player, string command, string[] args);
    [Command("phone.set"), Permission(PermissionUse)]
    private void cmd_phone_set(IPlayer player, string command, string[] args);
    [Command("phone.reset"), Permission(PermissionUse)]
    private void cmd_phone_reset(IPlayer player, string command, string[] args);
    [Command("phone.help"), Permission(PermissionUse)]
    private void cmd_phone_help(IPlayer player, string command, string[] args);
    [Command("phone.message"), Permission(PermissionTemp)]
    private void cmd_phone_message(IPlayer player, string command, string[] args);
    [Command("phone.track"), Permission(PermissionTemp)]
    private void cmd_phone_track(IPlayer player, string command, string[] args);
    [Command("phone.untrack"), Permission(PermissionTemp)]
    private void cmd_phone_untrack(IPlayer player, string command, string[] args);
    [Command("phone.clear"), Permission(PermissionTemp)]
    private void cmd_phone_clear(IPlayer player, string command, string[] args);
    [Command("phone.uiclose"), Permission(PermissionTemp)]
    private void cmd_uiclose(IPlayer player, string command, string[] args);
    [Command("phone.history"), Permission(PermissionAdmin)]
    private void cmd_phone_history(IPlayer player, string command, string[] args);
     void UsePhone(BasePlayer player, RegisteredPhone phone);
     void UsePhoneEnd(BasePlayer player);
     void ActiveCall(BasePlayer player, RegisteredPhone phone);
     void ActiveCallEnd(BasePlayer player, RegisteredPhone phone);
     void Init();
     void OnServerInitialized();
     void OnPhoneAnswered(PhoneController receiverPhone, PhoneController callerPhone);
     object OnPhoneDial(PhoneController callerPhone, PhoneController receiverPhone, BasePlayer player);
     void OnEntitySpawned(Telephone entity);
     void OnEntityDeath(Telephone entity, HitInfo info);
     bool CanPickupEntity(BasePlayer player, Telephone entity);
     void OnPlayerConnected(BasePlayer player);
     void OnUserPermissionGranted(string id, string permName);
     object OnPhoneDialFail(PhoneController callerPhone, Telephone.DialFailReason reason, BasePlayer player);
     void OnServerSave();
    private string FormatTxt(string text, string color, string size);
    private void LoadImages();
    private void CheckIfUsingPhone(BasePlayer player);
    private void ResetMessages();
    private void InitPlayer(BasePlayer player);
    private string ElapsedTimeString(BasePlayer player, long seconds);
     void NotifyIncoming(uint receiverPhoneID, uint callerPhoneID);
     class PhoneMessage
    {
        public readonly int senderNumber;
        public readonly int receiverNumber;
        public readonly string text;
        public bool unread;
        public long time;
        public PhoneMessage(int senderNumber, int receiverNumber, string text, long time);
        public long GetTimeElapsed();
        public string SenderID(PhoneManager manager);
        public string ReceiverID(PhoneManager manager);
    }

     class PhoneMessageList
    {
        public List<PhoneMessage> messages;
        public PhoneMessageList();
        public List<PhoneMessage> RecentMessages(int amount);
        public int MarkAllAsRead();
    }

     class PhoneRecord
    {
        public readonly int callerNumber;
        public readonly int receiverNumber;
        public bool unread;
        public readonly string status;
        public long time;
        public PhoneRecord(string status, int callerNumber, int receiverNumber, long time);
        public long GetTimeElapsed();
        public string CallerID(PhoneManager manager);
        public string ReceiverID(PhoneManager manager);
        public string ID(PhoneManager manager);
        public int Number(PhoneManager manager);
    }

     class PhoneRecordList
    {
        public List<PhoneRecord> records;
        [NonSerialized]
        public PhoneController phoneItem;
        public PhoneRecordList(PhoneController phone);
        public PhoneRecordList();
        public List<PhoneRecord> RecentRecords(int amount);
        public void RecordAnswered(PhoneController thisPhone, PhoneController callerPhone, PhoneManager manager);
        public void RecordMissed(PhoneController thisPhone, PhoneController callerPhone, PhoneManager manager);
        public void RecordOutgoing(PhoneController thisPhone, PhoneController receiverPhone, PhoneManager manager);
        public int MarkAllAsRead();
    }

     class RegisteredPhone
    {
        public readonly BaseEntity baseEntity;
        public readonly PhoneController phoneController;
        public RegisteredPhone(PhoneController phoneController);
        public uint GetID();
        public int GetNumber();
        public string GetName();
        public bool HasName();
        public string GetIdentity();
        public bool UserHasAuth(BasePlayer player);
    }

     class PhoneManager
    {
        public List<uint> registeredPhoneIds;
        public List<RegisteredPhone> registeredPhones;
        private Dictionary<uint, RegisteredPhone> id2phone;
        private Dictionary<int, RegisteredPhone> number2phone;
        private Dictionary<ulong, List<uint>> userTrackedPhones;
        private Dictionary<uint, List<ulong>> trackersOfPhone;
        private Dictionary<ulong, Dictionary<string, object>> userPreferences;
        public Dictionary<int, PhoneRecordList> phoneRecords;
        private Dictionary<ulong, int> phoneRecordsCount;
        public Dictionary<int, PhoneMessageList> phoneMessages;
        private Dictionary<ulong, int> phoneMessageCount;
        private Dictionary<ulong, int> hourlyMessageCount;
        private Dictionary<ulong, int> callState;
        public readonly string UNREGISTERED;
        public PhoneManager();
        public void LeaveMessage(int senderNumber, int targetNumber, string text);
        public void InitPreferences(ulong userID);
        public void SetPreference(ulong userID, string key, object value);
        public T GetPreference(ulong userID, string key);
        public void RegisterPhone(PhoneController phone);
        public void UnregisterPhone(Telephone baseEntity);
        public bool TrackPhone(BasePlayer player, int phoneNumber);
        public bool UntrackPhone(BasePlayer player, int phoneNumber);
        public void ClearTrackers(RegisteredPhone phone);
        public void ClearMessages(RegisteredPhone phone);
        public void ClearHistory(RegisteredPhone phone);
        public void ResetHourlyMessageLimits();
        public void IncrementMessageUse(ulong userId);
        public bool ExceededMessageLimit(ulong userId);
        public RegisteredPhone GetNearbyPhone(BasePlayer entity);
        public RegisteredPhone FindPhoneByNumber(int phoneNumber);
        public RegisteredPhone FindPhoneByID(uint baseEntityId);
        public List<ulong> GetPhoneTrackers(RegisteredPhone phone);
        public List<uint> GetTrackedPhones(ulong basePlayerId);
        public bool IsTrackedBy(RegisteredPhone phone, ulong userId);
        public PhoneRecordList GetPhoneRecords(int phoneNumber);
        public int GetPhoneRecordsCount(ulong userId);
        public void UpdatePhoneRecordsCount(ulong userId, int newValue);
        public PhoneMessageList GetPhoneMessages(int phoneNumber);
        public int GetPhoneMessageCount(ulong userId);
        public void UpdatePhoneMessageCount(ulong userId, int newValue);
        public void SetState(BasePlayer player, int value);
        public bool IsStateEqualTo(BasePlayer player, int value);
        public void RegisterExistingPhones();
        public void SaveTrackedPhones();
        public void LoadTrackedPhones();
        public void SavePhoneRecords();
        public void LoadPhoneRecords();
        public void SavePhoneMessages();
        public void LoadPhoneMessages();
        public void SavePreferences();
        public void LoadPreferences();
        private void SaveFile(string fileName, T data);
        private T LoadFromFile(string fileName, T defaultValue);
    }

     CuiElementContainer CreateHistoryPanel(BasePlayer player, RegisteredPhone phone);
     void CreateHistoryEntry(CuiElementContainer container, BasePlayer player, PhoneRecord record, int index, double mod, double bottomS, double topS);
     CuiElementContainer CreateTrackButton(BasePlayer player, RegisteredPhone phone);
     CuiElementContainer CreateMessagesPanel(BasePlayer player, RegisteredPhone phone, bool centered);
     void CreateMessageEntry(CuiElementContainer container, BasePlayer player, PhoneMessage message, int index, double mod, double bottomS, double topS);
     CuiElementContainer CreateInputPanel(BasePlayer player, int callerNumber, int receiverNumber);
     CuiElementContainer CreateNotificationPanel(BasePlayer player, string message, RegisteredPhone phone);
     CuiElementContainer CreateUnreadIndicator(BasePlayer player, int messages);
     CuiElementContainer CreateCallIndicator(BasePlayer player, int calls);
     void ShowPhoneUI(BasePlayer player, RegisteredPhone phone);
     void ShowMessagesUI(BasePlayer player, RegisteredPhone phone);
     void ShowNotificationHUD(BasePlayer player, string message, RegisteredPhone phone);
     void HideNotificationHUD(BasePlayer player);
     void ShowInputUI(BasePlayer player, int senderNumber, int receiverNumber);
     void ShowUnreadIndicator(BasePlayer player);
     void ShowCallIndicator(BasePlayer player);
     void HideAllUI(BasePlayer player);
    private readonly ColorUI COLOR_RUST_BODY;
    private readonly ColorUI COLOR_RUST_HEADER;
    private readonly ColorUI COLOR_PP_UI;
    private readonly ColorUI COLOR_PP_WARN;
    private readonly ColorUI COLOR_PP_SUBMIT;
    private readonly ColorUI COLOR_PP_ENTRY_BODY;
    private readonly ColorUI COLOR_PP_ENTRY_HEADER;
    private readonly ColorUI COLOR_PP_ENTRY_TITLE_FONT;
    private readonly ColorUI COLOR_PP_ENTRY_BODY_FONT;
     class ColorUI
    {
        public float r;
        public float g;
        public float b;
        public float a;
        public ColorUI(float r, float g, float b, float a);
        public string RGBA();
        public string RGBA(float a);
    }

}

private class Configuration
{
    [JsonProperty(PropertyName = "Enable incoming call notifications (true/false)")]
    public bool EnableIncomingCallNotifications { get; set; }
    [JsonProperty(PropertyName = "Hourly message limit")]
    public int HourlyMessageLimit { get; set; }
    [JsonProperty(PropertyName = "HUD Notification")]
    public NotificationHUD HudNotification;
    [JsonProperty(PropertyName = "HUD Call Indicator")]
    public IndicatorHUD HudCallIndicator;
    [JsonProperty(PropertyName = "HUD Message Indicator")]
    public IndicatorHUD HudMessageIndicator;
    public class NotificationHUD
    {
        [JsonProperty(PropertyName = "Anchor min")]
        public string AnchorMin;
        [JsonProperty(PropertyName = "Anchor max")]
        public string AnchorMax;
        [JsonProperty(PropertyName = "Background color")]
        public string BackgroundColor;
    }

    public class IndicatorHUD
    {
        [JsonProperty(PropertyName = "Anchor min")]
        public string AnchorMin;
        [JsonProperty(PropertyName = "Anchor max")]
        public string AnchorMax;
        [JsonProperty(PropertyName = "Image color")]
        public string ImageColor;
    }

    public class HistoryUI
    {
        [JsonProperty(PropertyName = "Entry background color")]
        public string EntryBackgroundColor;
        [JsonProperty(PropertyName = "Font color")]
        public string FontColor;
    }

    public class MessagesUI
    {
        [JsonProperty(PropertyName = "Entry background color")]
        public string EntryBackgroundColor;
    }

}

public class NotificationHUD
{
    [JsonProperty(PropertyName = "Anchor min")]
    public string AnchorMin;
    [JsonProperty(PropertyName = "Anchor max")]
    public string AnchorMax;
    [JsonProperty(PropertyName = "Background color")]
    public string BackgroundColor;
}

public class IndicatorHUD
{
    [JsonProperty(PropertyName = "Anchor min")]
    public string AnchorMin;
    [JsonProperty(PropertyName = "Anchor max")]
    public string AnchorMax;
    [JsonProperty(PropertyName = "Image color")]
    public string ImageColor;
}

public class HistoryUI
{
    [JsonProperty(PropertyName = "Entry background color")]
    public string EntryBackgroundColor;
    [JsonProperty(PropertyName = "Font color")]
    public string FontColor;
}

public class MessagesUI
{
    [JsonProperty(PropertyName = "Entry background color")]
    public string EntryBackgroundColor;
}

 class ChatCmd
{
    public string prefix;
    public List<string> usages;
    public List<string> perms;
    public Action<IPlayer, string, string[]> function;
    public bool HasPerms(string id, PhonesPlus plugin);
    public string Usage(string prefix);
}

 class PhoneMessage
{
    public readonly int senderNumber;
    public readonly int receiverNumber;
    public readonly string text;
    public bool unread;
    public long time;
    public PhoneMessage(int senderNumber, int receiverNumber, string text, long time);
    public long GetTimeElapsed();
    public string SenderID(PhoneManager manager);
    public string ReceiverID(PhoneManager manager);
}

 class PhoneMessageList
{
    public List<PhoneMessage> messages;
    public PhoneMessageList();
    public List<PhoneMessage> RecentMessages(int amount);
    public int MarkAllAsRead();
}

 class PhoneRecord
{
    public readonly int callerNumber;
    public readonly int receiverNumber;
    public bool unread;
    public readonly string status;
    public long time;
    public PhoneRecord(string status, int callerNumber, int receiverNumber, long time);
    public long GetTimeElapsed();
    public string CallerID(PhoneManager manager);
    public string ReceiverID(PhoneManager manager);
    public string ID(PhoneManager manager);
    public int Number(PhoneManager manager);
}

 class PhoneRecordList
{
    public List<PhoneRecord> records;
    [NonSerialized]
    public PhoneController phoneItem;
    public PhoneRecordList(PhoneController phone);
    public PhoneRecordList();
    public List<PhoneRecord> RecentRecords(int amount);
    public void RecordAnswered(PhoneController thisPhone, PhoneController callerPhone, PhoneManager manager);
    public void RecordMissed(PhoneController thisPhone, PhoneController callerPhone, PhoneManager manager);
    public void RecordOutgoing(PhoneController thisPhone, PhoneController receiverPhone, PhoneManager manager);
    public int MarkAllAsRead();
}

 class RegisteredPhone
{
    public readonly BaseEntity baseEntity;
    public readonly PhoneController phoneController;
    public RegisteredPhone(PhoneController phoneController);
    public uint GetID();
    public int GetNumber();
    public string GetName();
    public bool HasName();
    public string GetIdentity();
    public bool UserHasAuth(BasePlayer player);
}

 class PhoneManager
{
    public List<uint> registeredPhoneIds;
    public List<RegisteredPhone> registeredPhones;
    private Dictionary<uint, RegisteredPhone> id2phone;
    private Dictionary<int, RegisteredPhone> number2phone;
    private Dictionary<ulong, List<uint>> userTrackedPhones;
    private Dictionary<uint, List<ulong>> trackersOfPhone;
    private Dictionary<ulong, Dictionary<string, object>> userPreferences;
    public Dictionary<int, PhoneRecordList> phoneRecords;
    private Dictionary<ulong, int> phoneRecordsCount;
    public Dictionary<int, PhoneMessageList> phoneMessages;
    private Dictionary<ulong, int> phoneMessageCount;
    private Dictionary<ulong, int> hourlyMessageCount;
    private Dictionary<ulong, int> callState;
    public readonly string UNREGISTERED;
    public PhoneManager();
    public void LeaveMessage(int senderNumber, int targetNumber, string text);
    public void InitPreferences(ulong userID);
    public void SetPreference(ulong userID, string key, object value);
    public T GetPreference(ulong userID, string key);
    public void RegisterPhone(PhoneController phone);
    public void UnregisterPhone(Telephone baseEntity);
    public bool TrackPhone(BasePlayer player, int phoneNumber);
    public bool UntrackPhone(BasePlayer player, int phoneNumber);
    public void ClearTrackers(RegisteredPhone phone);
    public void ClearMessages(RegisteredPhone phone);
    public void ClearHistory(RegisteredPhone phone);
    public void ResetHourlyMessageLimits();
    public void IncrementMessageUse(ulong userId);
    public bool ExceededMessageLimit(ulong userId);
    public RegisteredPhone GetNearbyPhone(BasePlayer entity);
    public RegisteredPhone FindPhoneByNumber(int phoneNumber);
    public RegisteredPhone FindPhoneByID(uint baseEntityId);
    public List<ulong> GetPhoneTrackers(RegisteredPhone phone);
    public List<uint> GetTrackedPhones(ulong basePlayerId);
    public bool IsTrackedBy(RegisteredPhone phone, ulong userId);
    public PhoneRecordList GetPhoneRecords(int phoneNumber);
    public int GetPhoneRecordsCount(ulong userId);
    public void UpdatePhoneRecordsCount(ulong userId, int newValue);
    public PhoneMessageList GetPhoneMessages(int phoneNumber);
    public int GetPhoneMessageCount(ulong userId);
    public void UpdatePhoneMessageCount(ulong userId, int newValue);
    public void SetState(BasePlayer player, int value);
    public bool IsStateEqualTo(BasePlayer player, int value);
    public void RegisterExistingPhones();
    public void SaveTrackedPhones();
    public void LoadTrackedPhones();
    public void SavePhoneRecords();
    public void LoadPhoneRecords();
    public void SavePhoneMessages();
    public void LoadPhoneMessages();
    public void SavePreferences();
    public void LoadPreferences();
    private void SaveFile(string fileName, T data);
    private T LoadFromFile(string fileName, T defaultValue);
}

 class ColorUI
{
    public float r;
    public float g;
    public float b;
    public float a;
    public ColorUI(float r, float g, float b, float a);
    public string RGBA();
    public string RGBA(float a);
}


```

---

## PickupLines by  - Sends lovely pickup lines to yourself or other players

```csharp
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("PickupLines", "Spicy", "1.0.2")]
[Description("Send lovely pickup lines to yourself or other players.")]
 class PickupLines : CovalencePlugin
{
     void Init();
     void SetupLanguage();
    [Command("pickupline")]
     void cmdPickupLine(IPlayer player, string command, string[] args);
     string GetPickupLine(string pageResponse);
}


```

---

## PillsHere by MrBlue - Recovers health, hunger, and/or hydration by set amounts on item use

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Pills Here", "Wulf", "3.2.1")]
[Description("Recovers health, hunger, and/or hydration by set amounts on item use")]
 class PillsHere : CovalencePlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty("Calories amount (0.0 - 500.0)")]
        public float CaloriesAmount;
        [JsonProperty("Health amount (0.0 - 100.0)")]
        public float HealthAmount;
        [JsonProperty("Hydration amount (0.0 - 250.0)")]
        public float HydrationAmount;
        [JsonProperty("Item ID or short name to use")]
        public string ItemIdOrShortName;
        [JsonProperty("Use permission system")]
        public bool UsePermissions;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private const string permUse;
    private void Init();
    private void OnItemUse(Item item);
}

public class Configuration
{
    [JsonProperty("Calories amount (0.0 - 500.0)")]
    public float CaloriesAmount;
    [JsonProperty("Health amount (0.0 - 100.0)")]
    public float HealthAmount;
    [JsonProperty("Hydration amount (0.0 - 250.0)")]
    public float HydrationAmount;
    [JsonProperty("Item ID or short name to use")]
    public string ItemIdOrShortName;
    [JsonProperty("Use permission system")]
    public bool UsePermissions;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## Ping by MrBlue - Ping checking on command and automatic kicking of players with high pings

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Ping", "Wulf/lukespragg", "1.8.0", ResourceId = 1921)]
[Description("Ping chekcing on command and automatic kicking of players with high pings")]
 class Ping : CovalencePlugin
{
    const string permBypass;
    const string permCheck;
     bool highPingKick;
     bool kickNotices;
     bool repeatChecking;
     bool warnBeforeKick;
     int highPingLimit;
     int kickGracePeriod;
    protected override void LoadDefaultConfig();
     void LoadDefaultMessages();
     void Init();
     void OnServerInitialized();
     void OnServerSave();
     void OnUserConnected(IPlayer player);
     void PingCheck(IPlayer player, bool warned);
     void PingKick(IPlayer player, string ping);
    [Command("ping", "pong")]
     void PingCommand(IPlayer player, string command, string[] args);
     T GetConfig(string name, T value);
     string Lang(string key, string id, object[] args);
}


```

---

## PirateInsults by BaronVonFinchus - Hurl pirate themed insults at yourself or another person.

```csharp
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using System.Linq;
using System.Runtime.Serialization;
using System.Runtime.Serialization.Json;
using Newtonsoft.Json.Linq;

Oxide.Plugins
[Info("PirateInsults", "Baron Von Finchus", "1.0.5")]
[Description("Send insults to yourself or other players.")]
 class PirateInsults : CovalencePlugin
{
     void Init();
     void SetupLanguage();
    [Command("pirateinsult")]
     void cmdInsult(IPlayer player, string command, string[] args);
}


```

---

## PlaceholderAPI by misticos - Centralized location to query data from other plugins. Streamlined, convenient, and performant

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Net;
using System.Text;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

[Info("Placeholder API", "misticos", "2.2.5")]
[Description("Centralized location to query data from other plugins. Streamlined, convenient, and performant.")]
internal class PlaceholderAPI : CovalencePlugin
{
    private Dictionary<string, Placeholder> _placeholdersByName;
    private string _serverAddress;
    private static PlaceholderAPI _ins;
    private const string PermissionList;
    private const string CommandNameList;
    private const string PermissionTest;
    private const string CommandNameTest;
    private const string HookNameReady;
    private const string HookNameAddressDataRetrieved;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Placeholders", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, string> Placeholders;
        [JsonProperty(PropertyName = "Culture Ietf Tag")]
        public string DefaultCulture;
        [JsonProperty(PropertyName = "Local Time Offset")]
        public TimeSpan LocalTimeOffset;
        [JsonProperty(PropertyName = "Request Address Data (ip-api.com)")]
        public bool RequestAddressData;
        [JsonIgnore]
        public CultureInfo Culture;
        public class WipeSchedule
        {
            [JsonConverter(typeof(StringEnumConverter))]
            [JsonProperty(PropertyName = "Every First Month Day")]
            public DayOfWeek? EveryFirstDay;
            [JsonConverter(typeof(StringEnumConverter))]
            [JsonProperty(PropertyName = "Every N Day")]
            public DayOfWeek? EveryDay;
            [JsonProperty(PropertyName = "Every")]
            public TimeSpan? Every;
            [JsonProperty(PropertyName = "Time")]
            public TimeSpan? Time;
            public DateTime GetNextWipeDate(DateTime lastWipe);
            private DateTime GetFirstMonthDayOfWeek(DateTime date, DayOfWeek day);
            private DateTime GetNextDayOfWeek(DateTime date, DayOfWeek day);
        }

    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void CommandList(IPlayer player, string command, string[] args);
    private class Options
    {
        public IPlayer Target;
        public bool IgnoreCache;
        public int Parsed;
        public Options(IPlayer caller, string[] args);
    }

    private void CommandTest(IPlayer player, string command, string[] args);
    public static class AddressHandler
    {
        public static Queue<KeyValuePair<IPlayer, string>> QueuedPlayers;
        public static Dictionary<string, AddressResponse> Cache;
        public static Dictionary<string, JObject> CachedJson;
        public const float FrequencyRequest;
        public const float FrequencyBulkRequest;
        private const string AddressDataRequest;
        private const string AddressDataBulkRequest;
        public static bool Enqueue(IPlayer player, string address);
        public static void Clear();
        public static void RunTimers();
        private static void ProcessQueue();
        private static void Request(IPlayer player, string address);
        private static void ProcessQueueBulk();
        private static void RequestBulk(IPlayer[] players, string[] addresses, string body);
    }

    public class AddressResponse
    {
        [JsonProperty(PropertyName = "status")]
        public string Status;
        [JsonIgnore]
        public bool IsSuccess { get; set; }
        [JsonProperty(PropertyName = "continent")]
        public string Continent;
        [JsonProperty(PropertyName = "continentCode")]
        public string ContinentCode;
        [JsonProperty(PropertyName = "country")]
        public string Country;
        [JsonProperty(PropertyName = "countryCode")]
        public string CountryCode;
        [JsonProperty(PropertyName = "regionName")]
        public string Region;
        [JsonProperty(PropertyName = "region")]
        public string RegionCode;
        [JsonProperty(PropertyName = "city")]
        public string City;
        [JsonProperty(PropertyName = "district")]
        public string District;
        [JsonProperty(PropertyName = "zip")]
        public string ZipCode;
        [JsonProperty(PropertyName = "lat")]
        public float Latitude;
        [JsonProperty(PropertyName = "lon")]
        public float Longitude;
        [JsonProperty(PropertyName = "timezone")]
        public string Timezone;
        [JsonProperty(PropertyName = "offset")]
        public int TimezoneOffset;
        [JsonProperty(PropertyName = "currency")]
        public string Currency;
        [JsonProperty(PropertyName = "isp")]
        public string ISP;
        [JsonProperty(PropertyName = "org")]
        public string Organization;
        [JsonProperty(PropertyName = "as", NullValueHandling = NullValueHandling.Ignore)]
        public string AS;
        [JsonProperty(PropertyName = "asname", NullValueHandling = NullValueHandling.Ignore)]
        public string ASName;
        [JsonProperty(PropertyName = "mobile")]
        public bool Mobile;
        [JsonProperty(PropertyName = "proxy")]
        public bool Proxy;
        [JsonProperty(PropertyName = "hosting")]
        public bool Hosting;
    }

    public class Placeholder
    {
        public string Name;
        public string Description;
        public Plugin Owner;
        public Func<IPlayer, string, object> Action;
        public Dictionary<string, Dictionary<string, KeyValuePair<long, object>>> Cache;
        public long CacheTTLTicks;
        public bool CachePerPlayer;
        public Placeholder(Plugin owner, string name, string description, Func<IPlayer, string, object> action, double cacheTTL, bool cachePerPlayer);
        public bool HasCache();
        public bool HasCacheExpiration();
        public object Evaluate(long timestamp, IPlayer player, string option, bool ignoreCache);
        private static readonly Regex InputRegex;
        private static int GetNestedLevel(string input);
        public static void Run(IPlayer player, StringBuilder builder, bool ignoreCache);
        private static string Format(object value, string format, string option);
    }

    protected override void LoadDefaultMessages();
    private void Init();
    private void Loaded();
    private void OnPluginUnloaded(Plugin plugin);
    private void OnPluginLoaded(Plugin plugin);
    private void OnServerInitialized(bool isServer);
    private void Unload();
    private void OnUserApproved(string username, string id, string address);
    private void OnUserApprovedInternal(IPlayer player, string address);
    [HookMethod(nameof(ProcessPlaceholders))]
    private void ProcessPlaceholders(IPlayer player, StringBuilder builder, bool ignoreCache);
    [HookMethod(nameof(GetProcessPlaceholders))]
    private object GetProcessPlaceholders(int version);
    [HookMethod(nameof(EvaluatePlaceholder))]
    private object EvaluatePlaceholder(IPlayer player, string name, string option, bool ignoreCache);
    [HookMethod(nameof(GetEvaluatePlaceholder))]
    private object GetEvaluatePlaceholder(int version);
    [HookMethod(nameof(ExistsPlaceholder))]
    private bool ExistsPlaceholder(string name);
    [HookMethod(nameof(GetAddressData))]
    private JObject GetAddressData(string address);
    private static readonly Regex PlaceholderValidNameRegex;
    [HookMethod(nameof(AddPlaceholder))]
    private bool AddPlaceholder(Plugin plugin, string name, Func<IPlayer, string, object> action, string description, double cacheTTL, bool cachePerPlayer);
    [HookMethod(nameof(RemovePlaceholder))]
    private void RemovePlaceholder(string name);
    private void CallReady(Plugin plugin);
    private string GetMsg(string key, string userId);
    private void RegisterInbuiltPlaceholders();
    private IPAddress GetServerAddress();
}

private class Configuration
{
    [JsonProperty(PropertyName = "Placeholders", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, string> Placeholders;
    [JsonProperty(PropertyName = "Culture Ietf Tag")]
    public string DefaultCulture;
    [JsonProperty(PropertyName = "Local Time Offset")]
    public TimeSpan LocalTimeOffset;
    [JsonProperty(PropertyName = "Request Address Data (ip-api.com)")]
    public bool RequestAddressData;
    [JsonIgnore]
    public CultureInfo Culture;
    public class WipeSchedule
    {
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty(PropertyName = "Every First Month Day")]
        public DayOfWeek? EveryFirstDay;
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty(PropertyName = "Every N Day")]
        public DayOfWeek? EveryDay;
        [JsonProperty(PropertyName = "Every")]
        public TimeSpan? Every;
        [JsonProperty(PropertyName = "Time")]
        public TimeSpan? Time;
        public DateTime GetNextWipeDate(DateTime lastWipe);
        private DateTime GetFirstMonthDayOfWeek(DateTime date, DayOfWeek day);
        private DateTime GetNextDayOfWeek(DateTime date, DayOfWeek day);
    }

}

public class WipeSchedule
{
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty(PropertyName = "Every First Month Day")]
    public DayOfWeek? EveryFirstDay;
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty(PropertyName = "Every N Day")]
    public DayOfWeek? EveryDay;
    [JsonProperty(PropertyName = "Every")]
    public TimeSpan? Every;
    [JsonProperty(PropertyName = "Time")]
    public TimeSpan? Time;
    public DateTime GetNextWipeDate(DateTime lastWipe);
    private DateTime GetFirstMonthDayOfWeek(DateTime date, DayOfWeek day);
    private DateTime GetNextDayOfWeek(DateTime date, DayOfWeek day);
}

private class Options
{
    public IPlayer Target;
    public bool IgnoreCache;
    public int Parsed;
    public Options(IPlayer caller, string[] args);
}

public static class AddressHandler
{
    public static Queue<KeyValuePair<IPlayer, string>> QueuedPlayers;
    public static Dictionary<string, AddressResponse> Cache;
    public static Dictionary<string, JObject> CachedJson;
    public const float FrequencyRequest;
    public const float FrequencyBulkRequest;
    private const string AddressDataRequest;
    private const string AddressDataBulkRequest;
    public static bool Enqueue(IPlayer player, string address);
    public static void Clear();
    public static void RunTimers();
    private static void ProcessQueue();
    private static void Request(IPlayer player, string address);
    private static void ProcessQueueBulk();
    private static void RequestBulk(IPlayer[] players, string[] addresses, string body);
}

public class AddressResponse
{
    [JsonProperty(PropertyName = "status")]
    public string Status;
    [JsonIgnore]
    public bool IsSuccess { get; set; }
    [JsonProperty(PropertyName = "continent")]
    public string Continent;
    [JsonProperty(PropertyName = "continentCode")]
    public string ContinentCode;
    [JsonProperty(PropertyName = "country")]
    public string Country;
    [JsonProperty(PropertyName = "countryCode")]
    public string CountryCode;
    [JsonProperty(PropertyName = "regionName")]
    public string Region;
    [JsonProperty(PropertyName = "region")]
    public string RegionCode;
    [JsonProperty(PropertyName = "city")]
    public string City;
    [JsonProperty(PropertyName = "district")]
    public string District;
    [JsonProperty(PropertyName = "zip")]
    public string ZipCode;
    [JsonProperty(PropertyName = "lat")]
    public float Latitude;
    [JsonProperty(PropertyName = "lon")]
    public float Longitude;
    [JsonProperty(PropertyName = "timezone")]
    public string Timezone;
    [JsonProperty(PropertyName = "offset")]
    public int TimezoneOffset;
    [JsonProperty(PropertyName = "currency")]
    public string Currency;
    [JsonProperty(PropertyName = "isp")]
    public string ISP;
    [JsonProperty(PropertyName = "org")]
    public string Organization;
    [JsonProperty(PropertyName = "as", NullValueHandling = NullValueHandling.Ignore)]
    public string AS;
    [JsonProperty(PropertyName = "asname", NullValueHandling = NullValueHandling.Ignore)]
    public string ASName;
    [JsonProperty(PropertyName = "mobile")]
    public bool Mobile;
    [JsonProperty(PropertyName = "proxy")]
    public bool Proxy;
    [JsonProperty(PropertyName = "hosting")]
    public bool Hosting;
}

public class Placeholder
{
    public string Name;
    public string Description;
    public Plugin Owner;
    public Func<IPlayer, string, object> Action;
    public Dictionary<string, Dictionary<string, KeyValuePair<long, object>>> Cache;
    public long CacheTTLTicks;
    public bool CachePerPlayer;
    public Placeholder(Plugin owner, string name, string description, Func<IPlayer, string, object> action, double cacheTTL, bool cachePerPlayer);
    public bool HasCache();
    public bool HasCacheExpiration();
    public object Evaluate(long timestamp, IPlayer player, string option, bool ignoreCache);
    private static readonly Regex InputRegex;
    private static int GetNestedLevel(string input);
    public static void Run(IPlayer player, StringBuilder builder, bool ignoreCache);
    private static string Format(object value, string format, string option);
}


```

---

## Plagued by  - A plague has decimated the world's population, avoid others players to stay alive

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.SQLite;
using UnityEngine;

Oxide.Plugins
[Info("Plagued", "Psi|ocybin", "0.3.6", ResourceId = 1991)]
[Description("Everyone is infected")]
 class Plagued : RustPlugin
{
    static int plagueRange;
    static int plagueIncreaseRate;
    static int plagueDecreaseRate;
    static int plagueMinAffinity;
    static int affinityIncRate;
    static int affinityDecRate;
    static int maxKin;
    static int maxKinChanges;
    static int playerLayer;
    static bool disableSleeperAffinity;
     Dictionary<ulong, PlayerState> playerStates;
    protected override void LoadDefaultConfig();
     void Unload();
     void OnServerInitialized();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void OnRunPlayerMetabolism(PlayerMetabolism metabolism);
     void OnPlayerProximity(BasePlayer player, List<BasePlayer> players);
     void OnPlayerAlone(BasePlayer player);
    [ChatCommand("plagued")]
     void cmdPlagued(BasePlayer player, string command, string[] args);
     void cmdAddKin(BasePlayer player);
     bool cmdDelKin(BasePlayer player);
     bool cmdDelKin(BasePlayer player, int id);
     void cmdListKin(BasePlayer player);
     void cmdListAssociates(BasePlayer player);
     bool cmdInfo(BasePlayer player);
    public static void MsgPlayer(BasePlayer player, string format, object[] args);
    public void displayList(BasePlayer player, string listName, List<string> stringList);
     bool getPlayerLookedAt(BasePlayer player, BasePlayer targetPlayer);
     bool TryGetClosestRayPoint(Vector3 sourcePos, Quaternion sourceDir, object closestEnt, Vector3 closestHitpoint);
     bool TryGetPlayerView(BasePlayer player, Quaternion viewAngle);
    public class PlayerState
    {
        static readonly Core.SQLite.Libraries.SQLite sqlite;
        static Core.Database.Connection sqlConnection;
         BasePlayer player;
         int id;
         int plagueLevel;
         int kinChangesCount;
         bool pristine;
         Dictionary<ulong, Association> associations;
         Dictionary<ulong, Kin> kins;
         List<ulong> kinRequests;
        const string UpdateAssociation;
        const string InsertAssociation;
        const string CheckAssociationExists;
        const string DeleteAssociation;
        const string InsertPlayer;
        const string SelectPlayer;
        const string UpdatePlayerPlagueLevel;
        const string SelectAssociations;
        const string SelectKinList;
        const string InsertKin;
        const string DeleteKin;
        const string SelectKinRequestList;
        public PlayerState(BasePlayer newPlayer, Func<PlayerState, bool> callback);
        public static void setupDatabase(RustPlugin plugin);
        public static void closeDatabase();
         Association increaseAssociateAffinity(BasePlayer associate);
        public void increasePlaguePenalty(List<BasePlayer> associates);
        public void decreasePlaguePenalty();
        public void increasePlagueLevel(int contagionVectorCount);
        public void decreasePlagueLevel();
        public void decreaseAssociationsLevel();
        public bool isKinByUserID(ulong userID);
        public bool hasKinRequest(ulong kinID);
        public bool addKinRequest(ulong kinID);
        public bool addKin(ulong kinUserID);
        public bool removeKinById(int id);
        public bool removeKin(ulong kinUserID);
        public bool forceRemoveKin(ulong kinUserID);
        public List<string> getKinList();
        public List<string> getAssociatesList();
        public int getPlagueLevel();
        public int getId();
        public bool getPristine();
         Kin createKin(ulong kinUserId);
         void createAssociation(ulong associate_user_id, Func<Association, bool> callback);
         void syncPlagueLevel();
         void loadAssociations();
         void loadKinList();
         void loadKinRequestList();
         class Association
        {
            public int id;
            public int player_id;
            public int associate_id;
            public ulong associate_user_id;
            public string associate_name;
            public int level;
            public void create();
            public void load(Dictionary<string, object> association);
            public string getAffinityLabel();
        }

         class Kin
        {
            public int self_id;
            public int kin_id;
            public ulong kin_user_id;
            public string kin_name;
            public int player_one_id;
            public int player_two_id;
             Kin();
            public Kin(int p_self_id);
            public void create();
            public void load(Dictionary<string, object> kin);
        }

    }

    public class ProximityDetector : MonoBehaviour
    {
        public BasePlayer player;
        public void disableProximityCheck();
         void Awake();
         void OnDestroy();
         void CheckProximity();
         void notifyPlayerProximity(List<BasePlayer> players);
         void notifyPlayerAlone();
    }

}

public class PlayerState
{
    static readonly Core.SQLite.Libraries.SQLite sqlite;
    static Core.Database.Connection sqlConnection;
     BasePlayer player;
     int id;
     int plagueLevel;
     int kinChangesCount;
     bool pristine;
     Dictionary<ulong, Association> associations;
     Dictionary<ulong, Kin> kins;
     List<ulong> kinRequests;
    const string UpdateAssociation;
    const string InsertAssociation;
    const string CheckAssociationExists;
    const string DeleteAssociation;
    const string InsertPlayer;
    const string SelectPlayer;
    const string UpdatePlayerPlagueLevel;
    const string SelectAssociations;
    const string SelectKinList;
    const string InsertKin;
    const string DeleteKin;
    const string SelectKinRequestList;
    public PlayerState(BasePlayer newPlayer, Func<PlayerState, bool> callback);
    public static void setupDatabase(RustPlugin plugin);
    public static void closeDatabase();
     Association increaseAssociateAffinity(BasePlayer associate);
    public void increasePlaguePenalty(List<BasePlayer> associates);
    public void decreasePlaguePenalty();
    public void increasePlagueLevel(int contagionVectorCount);
    public void decreasePlagueLevel();
    public void decreaseAssociationsLevel();
    public bool isKinByUserID(ulong userID);
    public bool hasKinRequest(ulong kinID);
    public bool addKinRequest(ulong kinID);
    public bool addKin(ulong kinUserID);
    public bool removeKinById(int id);
    public bool removeKin(ulong kinUserID);
    public bool forceRemoveKin(ulong kinUserID);
    public List<string> getKinList();
    public List<string> getAssociatesList();
    public int getPlagueLevel();
    public int getId();
    public bool getPristine();
     Kin createKin(ulong kinUserId);
     void createAssociation(ulong associate_user_id, Func<Association, bool> callback);
     void syncPlagueLevel();
     void loadAssociations();
     void loadKinList();
     void loadKinRequestList();
     class Association
    {
        public int id;
        public int player_id;
        public int associate_id;
        public ulong associate_user_id;
        public string associate_name;
        public int level;
        public void create();
        public void load(Dictionary<string, object> association);
        public string getAffinityLabel();
    }

     class Kin
    {
        public int self_id;
        public int kin_id;
        public ulong kin_user_id;
        public string kin_name;
        public int player_one_id;
        public int player_two_id;
         Kin();
        public Kin(int p_self_id);
        public void create();
        public void load(Dictionary<string, object> kin);
    }

}

 class Association
{
    public int id;
    public int player_id;
    public int associate_id;
    public ulong associate_user_id;
    public string associate_name;
    public int level;
    public void create();
    public void load(Dictionary<string, object> association);
    public string getAffinityLabel();
}

 class Kin
{
    public int self_id;
    public int kin_id;
    public ulong kin_user_id;
    public string kin_name;
    public int player_one_id;
    public int player_two_id;
     Kin();
    public Kin(int p_self_id);
    public void create();
    public void load(Dictionary<string, object> kin);
}

public class ProximityDetector : MonoBehaviour
{
    public BasePlayer player;
    public void disableProximityCheck();
     void Awake();
     void OnDestroy();
     void CheckProximity();
     void notifyPlayerProximity(List<BasePlayer> players);
     void notifyPlayerAlone();
}


```

---

