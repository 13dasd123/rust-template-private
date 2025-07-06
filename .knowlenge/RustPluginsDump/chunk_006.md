# uMod Plugins Dataset - Code Abstractions (Continued)

Chunk 6 - Generated: 2025-07-06 20:39:06

## Payback

```csharp
using Oxide.Core;
using System.Collections.Generic;
using UnityEngine;
using System.Collections;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using System;
using System.Linq;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using System.Text.RegularExpressions;
using Oxide.Core.Libraries.Covalence;
using Rust;
using Rust.Ai;
using Network;

Oxide.Plugins
[Info("Payback", "Sempai#3239", "1.9.7")]
[Description("Специальные команды администратора для борьбы с читерами")]
 class Payback : RustPlugin
{
     Dictionary<Card, string> descriptions;
     Dictionary<string, Card> cardAliases;
    public void GiveCard(ulong userID, Card card, string[] args, BasePlayer admin);
     bool silentPacifism;
     void DoShark(BasePlayer player, string[] args, BasePlayer admin);
     IEnumerator SharkCo2(BasePlayer player, string[] args, BasePlayer admin);
     string sfx_bloodHit;
     string sfx_watersplash;
     IEnumerator MoveSharkCo(SimpleShark shark, BasePlayer player);
     void DoEmote(BasePlayer player, string[] args, BasePlayer admin);
     bool? CanUseGesture(BasePlayer player, GestureConfig gesture);
     void ResolveConflictingCommands(BasePlayer player, BasePlayer admin);
     Dictionary<ulong, BaseEntity> coilMap;
     void DoShocker(BasePlayer player, string[] args, BasePlayer admin);
     void DoBagSearch(ulong userID, string[] args, BasePlayer admin);
     IEnumerator BagSearchCo(ulong userID, bool logToDiscord, BasePlayer admin);
     bool flag_kill_no_loot;
     void GiveAdminHammer(BasePlayer admin);
     object OnStructureRepair(BaseCombatEntity entity, BasePlayer player);
     IEnumerator DeleteByCo(ulong steamid, Vector3 position, BasePlayer admin);
     HashSet<ulong> currentlyScreamingPlayers;
    public const string sound_scream;
    public const string effect_onfire;
     void GivePlayerFlamethrower(BasePlayer player);
    public void Line(BasePlayer player, Vector3 from, Vector3 to, Color color, float duration);
     void AdminSpawnChickens(BasePlayer player, BasePlayer target, string[] args);
     HashSet<BaseCombatEntity> chickens;
     void SpawnChickens(BasePlayer player, Vector3 spawnposition, string[] args);
     IEnumerator AnimalAttackCo(BasePlayer player, Vector3 spawnposition, string[] args);
     void OnPlayerRespawned(BasePlayer player);
     void OnEntityKill(BaseNetworkable entity, HitInfo info);
     HashSet<BaseNetworkable> entitiesWatchingForKilledMounts;
     void RocketManTarget(BasePlayer player);
     IEnumerator AccelerateRocketOverTime(ServerProjectile projectile);
    public const string invisibleChairPrefab;
     HashSet<BaseMountable> chairsPreventingDismount;
     BaseEntity InvisibleSit(BasePlayer targetPlayer);
    public List<string> sounds_kill_quad;
     void DoPinyataEffect(BasePlayer player);
     void DoDana(BasePlayer player, BasePlayer admin);
     void GiveItemOrDrop(BasePlayer player, Item item, bool stack);
     HashSet<ulong> thirstyPlayers;
     Dictionary<ulong, BasePlayer> basePlayerMap;
     Coroutine thirstyCoroutine;
     void DoThirsty(BasePlayer player);
     IEnumerator DoThirstyCo();
     void DoHigherGround(BasePlayer player);
     void DoCamomoCommand(BasePlayer player);
     void DoNakedCommand(BasePlayer targetPlayer);
     IEnumerator NakedOverTime(BasePlayer targetPlayer);
     string chairPrefab;
     Dictionary<ulong, BaseEntity> sitChairMap;
     void DoSitCommand(BasePlayer targetPlayer, BasePlayer adminPlayer);
     IEnumerator SitCo(BasePlayer player);
     object CanDismountEntity(BasePlayer player, BaseMountable entity);
     string guid_BSOD;
     string url_bsod;
     void DoBSOD(BasePlayer player, bool playPublic);
     string sound_bsod;
     string sound_fall;
     IEnumerator PlayBSODSounds(BasePlayer player, bool playPublic);
    [ConsoleCommand("uipaybackcommand")]
     void CommandUICommand(ConsoleSystem.Arg arg);
    [ChatCommand("setdroppercent")]
     void Command_SetDropPercent(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("setdroppercent")]
     void Console_CommandSetDropPercent(ConsoleSystem.Arg arg);
     void SetDropPercent(BasePlayer player, string[] args);
     Dictionary<ulong, float> playerMessageTimestamps;
     void SendPlayerLimitedMessage(ulong userID, string message, float rate);
     void AdminCommandToggleCard(BasePlayer admin, Card card, string[] args);
     void AdminToggleCard(BasePlayer admin, BasePlayer targetPlayer, Card card, string[] args);
    [ConsoleCommand("payback")]
     void Console_Payback(ConsoleSystem.Arg arg);
    [ChatCommand("payback")]
     void ChatCommandPayback(BasePlayer player, string cmd, string[] args);
     void CommandPayback(BasePlayer player, string cmd, string[] args);
     void DoPaybackPrintout(BasePlayer player, string[] args);
     Dictionary<ulong, HashSet<Card>> cardMap;
    public bool HasAnyCard(ulong userID);
    public bool HasCard(ulong userID, Card card);
    public void TakeCard(BasePlayer player, Card card, string[] args, BasePlayer admin);
    public void TakeCard(ulong userID, Card card, string[] args, BasePlayer admin);
     HashSet<ulong> recentPlayerVoices;
     Dictionary<ulong, float> recentPlayerVoiceTimestamps;
     Dictionary<ulong, HashSet<ulong>> listenedPlayersMap;
     Timer listenTimer;
     bool isListening;
    [ConsoleCommand("listen")]
     void ConsoleCommandListenNext(ConsoleSystem.Arg arg);
    [ChatCommand("listen")]
     void CommandListenNext(BasePlayer player);
    private object OnPlayerViolation(BasePlayer player, AntiHackType type);
     object OnPlayerDeath(BasePlayer player, HitInfo hitinfo);
     float Random();
     object OnPlayerVoice(BasePlayer player, Byte[] data);
     object OnHealingItemUse(MedicalTool tool, BasePlayer player);
     object OnPlayerHealthChange(BasePlayer player, float oldValue, float newValue);
    [ChatCommand("TestBanned")]
     void CommandTestBanned(BasePlayer player, string cmd, string[] args);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnPlayerBanned(Network.Connection connection, string reason);
     void OnPlayerBanned(string name, ulong id, string address, string reason);
     void OnPlayerKicked(BasePlayer player, string reason);
     List<string> banReasons;
     void CheckPublisherBan(string name, ulong id, string address, string reason);
     string TryGetDisplayName(ulong userID);
    [ConsoleCommand("bancheckexception")]
     void CommandBanCheckException(ConsoleSystem.Arg arg);
     bool test_connect;
    [ChatCommand("testconnect")]
     void CommandTestConnect(BasePlayer player);
     void OnPlayerConnected(BasePlayer player);
     void SendToDiscordWebhook(Dictionary<string, string> messageData, string title);
    private void SendWebhook(string WebhookUrl, string title, object fields);
    private class SendEmbedMessage
    {
        public SendEmbedMessage(int EmbedColour, string discordMessage, object _fields);
        [JsonProperty("embeds")]
        public object Embeds { get; set; }
        public string ToJson();
    }

    private void OnEntityTakeDamage(BaseEntity entity, HitInfo hitinfo);
     void DoScreaming(BasePlayer player, BasePlayer attacker, bool fire, bool screamSourceIsTarget);
     HashSet<BaseEntity> protectedStashes;
     void OnStashExposed(StashContainer stash, BasePlayer player);
    public BasePlayer GetPlayerWithName(string displayName);
     BaseEntity RaycastFirstEntity(Ray ray, float distance);
     void Initialize();
     void GenericChatCommand(BasePlayer player, string cmd, string[] args);
     void GenericConsoleCommand(ConsoleSystem.Arg arg);
     void OnServerInitialized(bool serverIsNOTinitialized);
    private static List<string> _viewInventoryHooks;
     void ViewTargetPlayerInventory(BasePlayer target, BasePlayer admin);
    private void ViewInvCmd(IPlayer iplayer, string command, string[] args);
    private List<LootableCorpse> _viewingcorpse;
    private void ViewInventory(BasePlayer player, BasePlayer targetplayer);
     LootableCorpse GetLootableCorpse(string title);
    private void StartLooting(BasePlayer player, BasePlayer targetplayer, LootableCorpse corpse);
    private void StartLootingContainer(BasePlayer player, ItemContainer container, LootableCorpse corpse);
    private void OnLootEntityEnd(BasePlayer player, LootableCorpse corpse);
     void OnEntityDeath(LootableCorpse corpse, HitInfo info);
    private IPlayer FindPlayer(string nameOrId);
    private bool HasPerm(string id, string perm);
    private string GetLang(string langKey, string playerId, object[] args);
    private void ChatMessage(IPlayer player, string langKey, object[] args);
    private void UnSubscribeFromHooks();
    private void SubscribeToHooks();
     string filename_data { get; set; }
     DynamicConfigFile file_payback_data;
    public PaybackData paybackData;
    public class PaybackData
    {
        public float percent_butterfingers_dropchance;
        public HashSet<ulong> bancheck_exceptions;
    }

     void Unload();
    private void SaveData();
    private void LoadData();
     void ReadDataIntoDynamicConfigFiles();
     void LoadFromDynamicConfigFiles();
    public const string permission_admin;
    public bool IsAdmin(BasePlayer player);
     void SetDespawnDuration(DroppedItem dropped, float seconds);
     void DestroyGroundCheck(BaseEntity entity);
    [ChatCommand("sound")]
     void SoundCommand(BasePlayer player, string cmd, string[] args);
     void PrintToPlayer(BasePlayer player, string text);
    public HashSet<ulong> GetPlayerTeam(ulong userID);
    public void PlaySound(List<string> effects, BasePlayer player, Vector3 worldPosition, bool playlocal);
    public void PlaySound(List<string> effects, BasePlayer player, bool playlocal);
    public void PlaySound(string effect, ListHashSet<BasePlayer> players, bool playlocal);
     bool test;
    public void PlaySound(string effect, BasePlayer player, bool playlocal, Vector3 posLocal);
    public void PlayGesture(BasePlayer target, string gestureName, bool canCancel);
    public class Worker : MonoBehaviour
    {
        public static Worker GetSingleton();
        static Worker _singleton;
        public static Coroutine StaticStartCoroutine(IEnumerator c);
    }

    private void Init();
    private PluginConfig config;
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private class PluginConfig
    {
        [JsonProperty("Check temporary game bans and notify via Discord Webhook")]
        public bool enabled_nexus_gamebancheck;
        [JsonProperty("Only report temp bans younger than days")]
        public int nexus_ban_days;
        [JsonProperty("Notify Game Ban + Team")]
        public bool notify_game_ban;
        [JsonProperty("Only notify ban if has team")]
        public bool notify_only_if_has_team;
        [JsonProperty("Include bm links in ban notification")]
        public bool notify_ban_include_bm;
        [JsonProperty("These discord webhooks will get notified")]
        public List<string> webhooks;
        [JsonProperty("notify player being attacked by cheater")]
        public bool notifyCheaterAttacking;
    }

    public class UI2
    {
        public static Vector4 vectorFullscreen;
        public static string ColorText(string input, string color);
        public static void ClearUI(BasePlayer player);
        public static Dictionary<ulong, HashSet<string>> dirtyMap;
        public static HashSet<string> GetDirtyBitsForPlayer(BasePlayer player);
        public class Layout
        {
            public Vector2 startPosition;
            public Vector4 cellBounds;
            public Vector2 padding;
            public Vector4 cursor;
            public int maxRows;
            public int row;
            public int col;
            public void Init(Vector2 _startPosition, Vector4 _cellBounds, int _maxRows, Vector2 _padding);
            public void NextCell(System.Action<Vector4, int, int> populateAction);
            public void Reset();
        }

        public static string ColorToHex(Color color);
        public static string HexToRGBAString(string hex);
        public static Vector4 GetOffsetVector4(Vector2 offset);
        public static Vector4 GetOffsetVector4(float x, float y);
        public static Vector4 SubtractPadding(Vector4 input, float padding);
        public static float GetSquareFromWidth(float width, float aspect);
        public static float GetSquareFromHeight(float height, float aspect);
        public static Vector4 MakeSquareFromWidth(Vector4 bounds, float aspect);
        public static Vector4 MakeSquareFromHeight(Vector4 bounds, float aspect);
        public static Vector4 MakeRectFromWidth(Vector4 bounds, float ratio, float aspect);
        public static Vector4 MakeRectFromHeight(Vector4 bounds, float ratio, float aspect);
        public static HashSet<string> guids;
        public static string GetMinUI(Vector4 panelPosition);
        public static string GetMaxUI(Vector4 panelPosition);
        public static string GetColorString(Vector4 color);
        public static CuiElement CreateInputField(CuiElementContainer container, string parent, string panelName, string message, int textSize, string color, Vector4 bounds, string command);
        public static void CreateOutlineLabel(CuiElementContainer container, string parent, string panelName, string message, string color, int size, Vector4 bounds, TextAnchor textAlignment, float fadeOut, float fadeIn, string outlineColor, string outlineDistance);
        public static void CreateLabel(CuiElementContainer container, string parent, string panelName, string message, string color, int size, string aMin, string aMax, TextAnchor textAlignment, float fadeIn, float fadeOut);
        public static CuiButton CreateButton(CuiElementContainer container, string parent, string panelName, string color, string text, int size, Vector4 bounds, string command, TextAnchor align, string textColor);
        public static CuiPanel CreatePanel(CuiElementContainer container, string parent, string panelName, string color, Vector4 bounds, string imageUrl, bool cursor, float fadeOut, float fadeIn, bool png, bool blur, bool outline);
    }

}

private class SendEmbedMessage
{
    public SendEmbedMessage(int EmbedColour, string discordMessage, object _fields);
    [JsonProperty("embeds")]
    public object Embeds { get; set; }
    public string ToJson();
}

public class PaybackData
{
    public float percent_butterfingers_dropchance;
    public HashSet<ulong> bancheck_exceptions;
}

public class Worker : MonoBehaviour
{
    public static Worker GetSingleton();
    static Worker _singleton;
    public static Coroutine StaticStartCoroutine(IEnumerator c);
}

private class PluginConfig
{
    [JsonProperty("Check temporary game bans and notify via Discord Webhook")]
    public bool enabled_nexus_gamebancheck;
    [JsonProperty("Only report temp bans younger than days")]
    public int nexus_ban_days;
    [JsonProperty("Notify Game Ban + Team")]
    public bool notify_game_ban;
    [JsonProperty("Only notify ban if has team")]
    public bool notify_only_if_has_team;
    [JsonProperty("Include bm links in ban notification")]
    public bool notify_ban_include_bm;
    [JsonProperty("These discord webhooks will get notified")]
    public List<string> webhooks;
    [JsonProperty("notify player being attacked by cheater")]
    public bool notifyCheaterAttacking;
}

public class UI2
{
    public static Vector4 vectorFullscreen;
    public static string ColorText(string input, string color);
    public static void ClearUI(BasePlayer player);
    public static Dictionary<ulong, HashSet<string>> dirtyMap;
    public static HashSet<string> GetDirtyBitsForPlayer(BasePlayer player);
    public class Layout
    {
        public Vector2 startPosition;
        public Vector4 cellBounds;
        public Vector2 padding;
        public Vector4 cursor;
        public int maxRows;
        public int row;
        public int col;
        public void Init(Vector2 _startPosition, Vector4 _cellBounds, int _maxRows, Vector2 _padding);
        public void NextCell(System.Action<Vector4, int, int> populateAction);
        public void Reset();
    }

    public static string ColorToHex(Color color);
    public static string HexToRGBAString(string hex);
    public static Vector4 GetOffsetVector4(Vector2 offset);
    public static Vector4 GetOffsetVector4(float x, float y);
    public static Vector4 SubtractPadding(Vector4 input, float padding);
    public static float GetSquareFromWidth(float width, float aspect);
    public static float GetSquareFromHeight(float height, float aspect);
    public static Vector4 MakeSquareFromWidth(Vector4 bounds, float aspect);
    public static Vector4 MakeSquareFromHeight(Vector4 bounds, float aspect);
    public static Vector4 MakeRectFromWidth(Vector4 bounds, float ratio, float aspect);
    public static Vector4 MakeRectFromHeight(Vector4 bounds, float ratio, float aspect);
    public static HashSet<string> guids;
    public static string GetMinUI(Vector4 panelPosition);
    public static string GetMaxUI(Vector4 panelPosition);
    public static string GetColorString(Vector4 color);
    public static CuiElement CreateInputField(CuiElementContainer container, string parent, string panelName, string message, int textSize, string color, Vector4 bounds, string command);
    public static void CreateOutlineLabel(CuiElementContainer container, string parent, string panelName, string message, string color, int size, Vector4 bounds, TextAnchor textAlignment, float fadeOut, float fadeIn, string outlineColor, string outlineDistance);
    public static void CreateLabel(CuiElementContainer container, string parent, string panelName, string message, string color, int size, string aMin, string aMax, TextAnchor textAlignment, float fadeIn, float fadeOut);
    public static CuiButton CreateButton(CuiElementContainer container, string parent, string panelName, string color, string text, int size, Vector4 bounds, string command, TextAnchor align, string textColor);
    public static CuiPanel CreatePanel(CuiElementContainer container, string parent, string panelName, string color, Vector4 bounds, string imageUrl, bool cursor, float fadeOut, float fadeIn, bool png, bool blur, bool outline);
}

public class Layout
{
    public Vector2 startPosition;
    public Vector4 cellBounds;
    public Vector2 padding;
    public Vector4 cursor;
    public int maxRows;
    public int row;
    public int col;
    public void Init(Vector2 _startPosition, Vector4 _cellBounds, int _maxRows, Vector2 _padding);
    public void NextCell(System.Action<Vector4, int, int> populateAction);
    public void Reset();
}


```

---

## Payback2

```csharp
using Oxide.Core;
using System.Collections.Generic;
using UnityEngine;
using System.Collections;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using System;
using System.Linq;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using System.Text.RegularExpressions;
using Oxide.Core.Libraries.Covalence;
using Rust;
using Rust.Ai;
using Network;

Oxide.Plugins
[Info("Payback2", "discord.gg/9vyTXsJyKR", "1.3.7")]
[Description("Special Admin Commands To Mess With Cheaters")]
 class Payback2 : RustPlugin
{
    [PluginReference]
     Plugin Payback;
    [PluginReference]
    private Plugin ImageLibrary;
     Dictionary<Card, string> descriptions;
     Dictionary<string, Card> cardAliases;
    public void GiveCard(ulong userID, Card card, string[] args, BasePlayer admin);
     bool silentPacifism;
    public void Line(BasePlayer player, Vector3 from, Vector3 to, Color color, float duration);
    public Dictionary<ulong, HashSet<BaseEntity>> roastEntities;
     void DoSpitroastCommand(BasePlayer targetPlayer, BasePlayer adminPlayer);
     IEnumerator RoastCo(BasePlayer targetPlayer, BasePlayer adminPlayer, RaycastHit hitinfo);
     string interrogate_open_url;
     string interrogate_closed_url;
     string guid_interrogate;
     Dictionary<ulong, bool> interrogationState;
     Dictionary<ulong, Item> interrogationMasks;
     Dictionary<ulong, HashSet<ulong>> interrogationSpectators;
     void DoInterrogate(BasePlayer player, BasePlayer admin, string[] args, bool removeCard, bool open);
     bool GetInterrogationState(ulong userid);
     void UpdateInterrogateUI(BasePlayer player, bool open, bool remove);
     object OnPlayerSpectate(BasePlayer spectator, string targetDisplayName);
     object OnPlayerSpectateEnd(BasePlayer spectator, string targetDisplayName);
     object OnPlayerRecover(BasePlayer player);
     Dictionary<ulong, Coroutine> interrogationCooldowns;
     object OnPlayerVoice(BasePlayer player, Byte[] data);
     IEnumerator InterrogationCo(BasePlayer player);
     object CanLootPlayer(BasePlayer target, BasePlayer looter);
     Dictionary<ulong, List<BaseEntity>> cowboynetworkables;
     void DoHog(BasePlayer player, BasePlayer admin, string[] args, bool removeCard);
     IEnumerator HogSFX(BasePlayer player, BaseEntity boarEntity);
     IEnumerator HogGameTipCo(BasePlayer player, BaseMountable mount);
     IEnumerator UpdateSeatedPlayerCo(BaseMountable mount);
    public void CreateGameTip(string text, BasePlayer player, float length, bool redColor);
     IEnumerator DropFollowPlayer(BasePlayer player, DroppedItem item);
     IEnumerator ChairFacingCo(BasePlayer player, BaseEntity chair);
     IEnumerator ContinuousTP(BasePlayer player, BaseEntity entity);
    public void SendToPlayer(BasePlayer player, ulong netid, byte[] data);
     IEnumerator AlwaysKill(BaseEntity entity, BasePlayer player);
     string guid_potato;
     Dictionary<ulong, int> currentFrameMap;
     Dictionary<ulong, Coroutine> potatoCoRoutines;
     void DoPotato(BasePlayer player, BasePlayer admin, string[] args, bool doRemove);
     IEnumerator DoPotatoCo(BasePlayer player, string[] args);
     Dictionary<ulong, float> woodsTimestamps;
     HashSet<ulong> woodsHasLandmines;
     void DoWoods(BasePlayer target);
     HashSet<BaseEntity> animals;
     HashSet<BaseEntity> noDamageEntities;
     IEnumerator AnimalAttackCo(BasePlayer player, Vector3 spawnposition, string[] args);
    private object OnPlayerViolation(BasePlayer player, AntiHackType type);
     object OnPlayerDeath(BasePlayer player, HitInfo hitinfo);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnPlayerBanned(Network.Connection connection, string reason);
     void OnPlayerBanned(string name, ulong id, string address, string reason);
     void OnPlayerKicked(BasePlayer player, string reason);
    private void OnEntityTakeDamage(BaseEntity entity, HitInfo hitinfo);
     Dictionary<ulong, float> playerMessageTimestamps;
     void SendPlayerLimitedMessage(ulong userID, string message, float rate);
     void AdminCommandToggleCard(BasePlayer admin, Card card, string[] args);
     void AdminToggleCard(BasePlayer admin, BasePlayer targetPlayer, Card card, string[] args);
    [ConsoleCommand("payback2")]
     void Console_Payback(ConsoleSystem.Arg arg);
    [ChatCommand("payback2")]
     void ChatCommandPayback(BasePlayer player, string cmd, string[] args);
     void CommandPayback(BasePlayer player, string cmd, string[] args);
    const string PAYBACK_VERSION;
     void DoPaybackPrintout(BasePlayer player, string[] args);
     Dictionary<ulong, HashSet<Card>> cardMap;
    public bool HasAnyCard(ulong userID);
    public bool HasCard(ulong userID, Card card);
    public void TakeCard(BasePlayer player, Card card, string[] args, BasePlayer admin);
    public void TakeCard(ulong userID, Card card, string[] args, BasePlayer admin);
     void SendToDiscordWebhook(Dictionary<string, string> messageData, string title);
    private void SendWebhook(string WebhookUrl, string title, object fields);
    private class SendEmbedMessage
    {
        public SendEmbedMessage(int EmbedColour, string discordMessage, object _fields);
        [JsonProperty("embeds")]
        public object Embeds { get; set; }
        public string ToJson();
    }

     void Initialize();
     void GenericChatCommand(BasePlayer player, string cmd, string[] args);
     void GenericConsoleCommand(ConsoleSystem.Arg arg);
     void OnServerInitialized(bool serverIsNOTinitialized);
    private static List<string> _viewInventoryHooks;
     void ViewTargetPlayerInventory(BasePlayer target, BasePlayer admin);
    private void ViewInvCmd(IPlayer iplayer, string command, string[] args);
    private List<LootableCorpse> _viewingcorpse;
    private void ViewInventory(BasePlayer player, BasePlayer targetplayer);
     LootableCorpse GetLootableCorpse(string title);
    private void StartLooting(BasePlayer player, BasePlayer targetplayer, LootableCorpse corpse);
    private void StartLootingContainer(BasePlayer player, ItemContainer container, LootableCorpse corpse);
    private void OnLootEntityEnd(BasePlayer player, LootableCorpse corpse);
     void OnEntityDeath(LootableCorpse corpse, HitInfo info);
    private IPlayer FindPlayer(string nameOrId);
    private bool HasPerm(string id, string perm);
    private string GetLang(string langKey, string playerId, object[] args);
    private void ChatMessage(IPlayer player, string langKey, object[] args);
    private void UnSubscribeFromHooks();
    private void SubscribeToHooks();
     void AddImage(string url);
    private string GetImage(string url);
     float Random();
     string TryGetDisplayName(ulong userID);
    public BasePlayer GetPlayerWithName(string displayName);
     BaseEntity RaycastFirstEntity(Ray ray, float distance);
     void SetDespawnDuration(DroppedItem dropped, float seconds);
     void DestroyGroundCheck(BaseEntity entity);
    [ChatCommand("sound")]
     void SoundCommand(BasePlayer player, string cmd, string[] args);
     void PrintToPlayer(BasePlayer player, string text);
    public HashSet<ulong> GetPlayerTeam(ulong userID);
    public void PlaySound(List<string> effects, BasePlayer player, Vector3 worldPosition, bool playlocal);
    public void PlaySound(List<string> effects, BasePlayer player, bool playlocal);
    public void PlaySound(string effect, ListHashSet<BasePlayer> players, bool playlocal);
     bool test;
    public void PlaySound(string effect, BasePlayer player, bool playlocal, Vector3 posLocal);
    public void PlayGesture(BasePlayer target, string gestureName, bool canCancel);
    public class Worker : MonoBehaviour
    {
        public static Worker GetSingleton();
        static Worker _singleton;
        public static Coroutine StaticStartCoroutine(IEnumerator c);
    }

     HashSet<BaseNetworkable> entitiesWatchingForKilledMounts;
    public const string chairPrefab2;
    public const string invisibleChairPrefab;
     HashSet<BaseMountable> chairsPreventingDismount;
     string chairPrefab;
     Dictionary<ulong, BaseEntity> sitChairMap;
     BaseEntity InvisibleSit(BasePlayer targetPlayer);
     void DoBagSearch(ulong userID, string[] args, BasePlayer admin);
     IEnumerator BagSearchCo(ulong userID, bool logToDiscord, BasePlayer admin);
     bool flag_kill_no_loot;
     void GiveAdminHammer(BasePlayer admin);
     object OnStructureRepair(BaseCombatEntity entity, BasePlayer player);
     IEnumerator DeleteByCo(ulong steamid, Vector3 position, BasePlayer admin);
     void ResolveConflictingCommands(BasePlayer player, BasePlayer admin);
     void OnPlayerRespawned(BasePlayer player);
     void OnEntityKill(BaseNetworkable entity, HitInfo info);
     void GiveItemOrDrop(BasePlayer player, Item item, bool stack);
     void DoHigherGround(BasePlayer player);
     void DoSitCommand(BasePlayer targetPlayer, BasePlayer adminPlayer);
     IEnumerator SitCo(BasePlayer player);
     object CanDismountEntity(BasePlayer player, BaseMountable entity);
    public HashSet<Card> cardsInPayback1;
    private void Init();
    private PluginConfig config;
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private class PluginConfig
    {
        [JsonProperty("These discord webhooks will get notified. Dont forget the [\"\"] Format: \"webhooks\" : [\"hook\"],")]
        public List<string> webhooks;
        [JsonProperty("Notify player when attacked by cheater")]
        public bool notifyCheaterAttacking;
    }

     string filename_data { get; set; }
     DynamicConfigFile file_payback_data;
    public PaybackData paybackData;
    public class PaybackData
    {
    }

     void Unload();
    private void SaveData();
    private void LoadData();
     void ReadDataIntoDynamicConfigFiles();
     void LoadFromDynamicConfigFiles();
    public const string permission_admin;
    public bool IsAdmin(BasePlayer player);
    public class UI2
    {
        public static Vector4 vectorFullscreen;
        public static string ColorText(string input, string color);
        public static void ClearUI(BasePlayer player);
        public static Dictionary<ulong, HashSet<string>> dirtyMap;
        public static HashSet<string> GetDirtyBitsForPlayer(BasePlayer player);
        public class Layout
        {
            public Vector2 startPosition;
            public Vector4 cellBounds;
            public Vector2 padding;
            public Vector4 cursor;
            public int maxRows;
            public int row;
            public int col;
            public void Init(Vector2 _startPosition, Vector4 _cellBounds, int _maxRows, Vector2 _padding);
            public void NextCell(System.Action<Vector4, int, int> populateAction);
            public void Reset();
        }

        public static string ColorToHex(Color color);
        public static string HexToRGBAString(string hex);
        public static Vector4 GetOffsetVector4(Vector2 offset);
        public static Vector4 GetOffsetVector4(float x, float y);
        public static Vector4 SubtractPadding(Vector4 input, float padding);
        public static float GetSquareFromWidth(float width, float aspect);
        public static float GetSquareFromHeight(float height, float aspect);
        public static Vector4 MakeSquareFromWidth(Vector4 bounds, float aspect);
        public static Vector4 MakeSquareFromHeight(Vector4 bounds, float aspect);
        public static Vector4 MakeRectFromWidth(Vector4 bounds, float ratio, float aspect);
        public static Vector4 MakeRectFromHeight(Vector4 bounds, float ratio, float aspect);
        public static HashSet<string> guids;
        public static string GetMinUI(Vector4 panelPosition);
        public static string GetMaxUI(Vector4 panelPosition);
        public static string GetColorString(Vector4 color);
        public static CuiElement CreateInputField(CuiElementContainer container, string parent, string panelName, string message, int textSize, string color, Vector4 bounds, string command);
        public static void CreateOutlineLabel(CuiElementContainer container, string parent, string panelName, string message, string color, int size, Vector4 bounds, TextAnchor textAlignment, float fadeOut, float fadeIn, string outlineColor, string outlineDistance);
        public static void CreateLabel(CuiElementContainer container, string parent, string panelName, string message, string color, int size, string aMin, string aMax, TextAnchor textAlignment, float fadeIn, float fadeOut);
        public static CuiButton CreateButton(CuiElementContainer container, string parent, string panelName, string color, string text, int size, Vector4 bounds, string command, TextAnchor align, string textColor);
        public static CuiPanel CreatePanel(CuiElementContainer container, string parent, string panelName, string color, Vector4 bounds, string imageUrl, bool cursor, float fadeOut, float fadeIn, bool png, bool blur, bool outline);
    }

}

private class SendEmbedMessage
{
    public SendEmbedMessage(int EmbedColour, string discordMessage, object _fields);
    [JsonProperty("embeds")]
    public object Embeds { get; set; }
    public string ToJson();
}

public class Worker : MonoBehaviour
{
    public static Worker GetSingleton();
    static Worker _singleton;
    public static Coroutine StaticStartCoroutine(IEnumerator c);
}

private class PluginConfig
{
    [JsonProperty("These discord webhooks will get notified. Dont forget the [\"\"] Format: \"webhooks\" : [\"hook\"],")]
    public List<string> webhooks;
    [JsonProperty("Notify player when attacked by cheater")]
    public bool notifyCheaterAttacking;
}

public class PaybackData
{
}

public class UI2
{
    public static Vector4 vectorFullscreen;
    public static string ColorText(string input, string color);
    public static void ClearUI(BasePlayer player);
    public static Dictionary<ulong, HashSet<string>> dirtyMap;
    public static HashSet<string> GetDirtyBitsForPlayer(BasePlayer player);
    public class Layout
    {
        public Vector2 startPosition;
        public Vector4 cellBounds;
        public Vector2 padding;
        public Vector4 cursor;
        public int maxRows;
        public int row;
        public int col;
        public void Init(Vector2 _startPosition, Vector4 _cellBounds, int _maxRows, Vector2 _padding);
        public void NextCell(System.Action<Vector4, int, int> populateAction);
        public void Reset();
    }

    public static string ColorToHex(Color color);
    public static string HexToRGBAString(string hex);
    public static Vector4 GetOffsetVector4(Vector2 offset);
    public static Vector4 GetOffsetVector4(float x, float y);
    public static Vector4 SubtractPadding(Vector4 input, float padding);
    public static float GetSquareFromWidth(float width, float aspect);
    public static float GetSquareFromHeight(float height, float aspect);
    public static Vector4 MakeSquareFromWidth(Vector4 bounds, float aspect);
    public static Vector4 MakeSquareFromHeight(Vector4 bounds, float aspect);
    public static Vector4 MakeRectFromWidth(Vector4 bounds, float ratio, float aspect);
    public static Vector4 MakeRectFromHeight(Vector4 bounds, float ratio, float aspect);
    public static HashSet<string> guids;
    public static string GetMinUI(Vector4 panelPosition);
    public static string GetMaxUI(Vector4 panelPosition);
    public static string GetColorString(Vector4 color);
    public static CuiElement CreateInputField(CuiElementContainer container, string parent, string panelName, string message, int textSize, string color, Vector4 bounds, string command);
    public static void CreateOutlineLabel(CuiElementContainer container, string parent, string panelName, string message, string color, int size, Vector4 bounds, TextAnchor textAlignment, float fadeOut, float fadeIn, string outlineColor, string outlineDistance);
    public static void CreateLabel(CuiElementContainer container, string parent, string panelName, string message, string color, int size, string aMin, string aMax, TextAnchor textAlignment, float fadeIn, float fadeOut);
    public static CuiButton CreateButton(CuiElementContainer container, string parent, string panelName, string color, string text, int size, Vector4 bounds, string command, TextAnchor align, string textColor);
    public static CuiPanel CreatePanel(CuiElementContainer container, string parent, string panelName, string color, Vector4 bounds, string imageUrl, bool cursor, float fadeOut, float fadeIn, bool png, bool blur, bool outline);
}

public class Layout
{
    public Vector2 startPosition;
    public Vector4 cellBounds;
    public Vector2 padding;
    public Vector4 cursor;
    public int maxRows;
    public int row;
    public int col;
    public void Init(Vector2 _startPosition, Vector4 _cellBounds, int _maxRows, Vector2 _padding);
    public void NextCell(System.Action<Vector4, int, int> populateAction);
    public void Reset();
}


```

---

## PerformanceMonitor

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

## PermanentBPs

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using System.Text;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("Permanent Blueprints", "Skipcast", "1.0.1")]
[Description("Saves blueprints between BP wipes.")]
public class PermanentBPs : RustPlugin
{
    const string PermissionName;
    static class Translations
    {
        public class Translation
        {
            public static Plugin plugin;
            public static Lang lang;
            public readonly string Key;
            public readonly string DefaultValue;
            public Translation(string key, string defaultValue);
            public string Get(BasePlayer player);
        }

        public static readonly Translation NoItemWithName;
        public static readonly Translation ItemAlreadyBlacklisted;
        public static readonly Translation ItemNotBlacklisted;
        public static readonly Translation AddedToBlacklist;
        public static readonly Translation RemovedFromBlacklist;
        public static readonly Translation NoPermission;
        public static readonly Translation BlueprintsLearnedNotice;
        public static readonly Translation BlacklistedBlueprintsNotice;
    }

     class PlayerData
    {
        public List<string> Blueprints;
    }

     class StoredData
    {
        public Dictionary<ulong, PlayerData> PlayerData;
    }

     class PluginConfig
    {
        public List<string> BlacklistedBlueprints;
    }

    private StoredData data;
    private List<string> blacklistedBlueprints;
     void Init();
    protected override void LoadDefaultConfig();
     void LoadDefaultMessages();
     void Loaded();
     void OnServerInitialized();
     void OnServerSave();
    private void SaveBPs();
     void OnPlayerInit(BasePlayer player);
    [ChatCommand("blacklistbp")]
     void ChatCmd_BlacklistBlueprint(BasePlayer player, string command, string[] args);
    [ChatCommand("whitelistbp")]
     void ChatCmd_WhitelistBlueprint(BasePlayer player, string command, string[] args);
    private static ItemDefinition GetItemDefinitionFromDisplayName(string[] args);
    private void RelearnBlueprints(BasePlayer player);
}

static class Translations
{
    public class Translation
    {
        public static Plugin plugin;
        public static Lang lang;
        public readonly string Key;
        public readonly string DefaultValue;
        public Translation(string key, string defaultValue);
        public string Get(BasePlayer player);
    }

    public static readonly Translation NoItemWithName;
    public static readonly Translation ItemAlreadyBlacklisted;
    public static readonly Translation ItemNotBlacklisted;
    public static readonly Translation AddedToBlacklist;
    public static readonly Translation RemovedFromBlacklist;
    public static readonly Translation NoPermission;
    public static readonly Translation BlueprintsLearnedNotice;
    public static readonly Translation BlacklistedBlueprintsNotice;
}

public class Translation
{
    public static Plugin plugin;
    public static Lang lang;
    public readonly string Key;
    public readonly string DefaultValue;
    public Translation(string key, string defaultValue);
    public string Get(BasePlayer player);
}

 class PlayerData
{
    public List<string> Blueprints;
}

 class StoredData
{
    public Dictionary<ulong, PlayerData> PlayerData;
}

 class PluginConfig
{
    public List<string> BlacklistedBlueprints;
}


```

---

## PermissionsManager

```csharp
using System;
using System.Linq;
using System.Globalization;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json;

Oxide.Plugins
[Info("PermissionsManager", "Steenamaroo", "2.0.4", ResourceId = 0)]
 class PermissionsManager : RustPlugin
{
     List<string> PlugList;
     Dictionary<int, string> numberedPerms;
     List<ulong> MenuOpen;
     bool HasPermission(string id, string perm);
    const string permAllowed;
     Dictionary<ulong, Info> ActiveAdmins;
    public class Info
    {
        public string inheritedcheck;
        public int noOfPlugs;
        public int pluginPage;
        public int PPage;
        public int GPage;
        public string subjectGroup;
        public BasePlayer subject;
    }

     string ButtonColour1;
     string ButtonColour2;
     void OnPluginLoaded(Plugin plugin);
     void OnPluginUnloaded(Plugin plugin);
     void Loaded();
     void OnServerInitialized();
     void Unload();
     void OnPlayerDisconnected(BasePlayer player);
     void OnPlayerDeath(BasePlayer player, HitInfo info);
     void DestroyMenu(BasePlayer player, bool all);
     void Wipe();
     void GetPlugs(BasePlayer player);
     bool IsAuth(BasePlayer player);
     void SetButtons(bool on);
     object[] PermsCheck(BasePlayer player, string group, string info);
     void PMBgUI(BasePlayer player);
     void PMMainUI(BasePlayer player, bool group, int page);
     void PlugsUI(BasePlayer player, string msg, string group, int page);
     void PMPermsUI(BasePlayer player, string msg, int PlugNumber, string group, int page);
     void ViewPlayersUI(BasePlayer player, string msg, int page);
     void ViewGroupsUI(BasePlayer player, string msg, int page);
    [ConsoleCommand("PMToMain")]
    private void PMToMain(ConsoleSystem.Arg arg);
    [ConsoleCommand("PMTogglePlayerGroup")]
    private void PMTogglePlayerGroup(ConsoleSystem.Arg arg, bool group, int page);
    [ConsoleCommand("ShowInherited")]
    private void ShowInherited(ConsoleSystem.Arg arg);
    [ConsoleCommand("PMEmptyGroup")]
    private void PMEmptyGroup(ConsoleSystem.Arg arg);
    [ConsoleCommand("EmptyGroup")]
    private void EmptyGroup(ConsoleSystem.Arg arg);
    [ConsoleCommand("Empty")]
    private void Empty(ConsoleSystem.Arg arg);
     string RemoveSpaces(string input);
     string RemoveDashes(string input);
    [ConsoleCommand("GroupAddRemove")]
    private void GroupAddRemove(ConsoleSystem.Arg arg);
    [ConsoleCommand("Groups")]
    private void GroupsPM(ConsoleSystem.Arg arg);
    [ConsoleCommand("PlayersIn")]
    private void PlayersPM(ConsoleSystem.Arg arg);
    [ConsoleCommand("ClosePM")]
    private void ClosePM(ConsoleSystem.Arg arg);
    [ConsoleCommand("Navigate")]
    private void Navigate(ConsoleSystem.Arg arg);
    [ConsoleCommand("PMSelected")]
    private void PMSelected(ConsoleSystem.Arg arg);
    [ConsoleCommand("PermsList")]
    private void PermsList(ConsoleSystem.Arg arg, int plugNumber);
    [ChatCommand("perms")]
     void CmdPerms(BasePlayer player, string command, string[] args);
     List<BasePlayer> GetAllPlayers();
     BasePlayer FindPlayer(ulong ID);
     BasePlayer FindPlayerByName(string name);
    public ConfigData config;
    public class ConfigData
    {
        [JsonProperty(PropertyName = "Options - GUI Transparency 0-1")]
        public double guitransparency;
        [JsonProperty(PropertyName = "Chat - Title colour")]
        public string TitleColour;
        [JsonProperty(PropertyName = "Chat - Message colour")]
        public string MessageColour;
        [JsonProperty(PropertyName = "Options - Plugin BlockList")]
        public string BlockList;
        [JsonProperty(PropertyName = "GUI - Label colour")]
        public string ButtonColour;
        [JsonProperty(PropertyName = "GUI - On colour")]
        public string OnColour;
        [JsonProperty(PropertyName = "GUI - Off colour")]
        public string OffColour;
        [JsonProperty(PropertyName = "GUI - All = per page")]
        public bool AllPerPage;
        [JsonProperty(PropertyName = "GUI - Inherited colour")]
        public string InheritedColour;
    }

    private bool LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConfig(ConfigData config);
    readonly Dictionary<string, string> messages;
}

public class Info
{
    public string inheritedcheck;
    public int noOfPlugs;
    public int pluginPage;
    public int PPage;
    public int GPage;
    public string subjectGroup;
    public BasePlayer subject;
}

public class ConfigData
{
    [JsonProperty(PropertyName = "Options - GUI Transparency 0-1")]
    public double guitransparency;
    [JsonProperty(PropertyName = "Chat - Title colour")]
    public string TitleColour;
    [JsonProperty(PropertyName = "Chat - Message colour")]
    public string MessageColour;
    [JsonProperty(PropertyName = "Options - Plugin BlockList")]
    public string BlockList;
    [JsonProperty(PropertyName = "GUI - Label colour")]
    public string ButtonColour;
    [JsonProperty(PropertyName = "GUI - On colour")]
    public string OnColour;
    [JsonProperty(PropertyName = "GUI - Off colour")]
    public string OffColour;
    [JsonProperty(PropertyName = "GUI - All = per page")]
    public bool AllPerPage;
    [JsonProperty(PropertyName = "GUI - Inherited colour")]
    public string InheritedColour;
}


```

---

## PersonalBeacon

```csharp
using System.Collections.Generic;
using System;
using System.Reflection;
using System.Data;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("PersonalBeacon", "Mordenak", "1.0.7", ResourceId = 1000)]
 class PersonalBeacon : RustPlugin
{
    static int beaconHeight;
    static int arrowSize;
    static float beaconRefresh;
    static Core.Configuration.DynamicConfigFile BeaconData;
    static Dictionary<string, bool> userBeacons;
    static Dictionary<string, Oxide.Plugins.Timer> userBeaconTimers;
    static Dictionary<string, bool> adminBeacons;
    static Dictionary<string, Oxide.Plugins.Timer> adminTimers;
    static Dictionary<string, bool> adminBeaconIsOn;
    static Dictionary<string, Oxide.Plugins.Timer> adminBeaconTimers;
     void Loaded();
     void Unload();
     void OnServerSave();
    private void SaveBeaconData();
    private void LoadBeaconData();
     void DisplayBeacon(BasePlayer player);
    [ChatCommand("setwp")]
     void cmdSetBeacon(BasePlayer player, string command, string[] args);
    [ChatCommand("wp")]
     void cmdBeacon(BasePlayer player, string command, string[] args);
    [ChatCommand("wpadmin")]
     void cmdAdminWaypoint(BasePlayer player, string command, string[] args);
    [ChatCommand("wpcount")]
     void cmdCountBeacons(BasePlayer player, string command, string[] args);
    [ChatCommand("wpshowall")]
     void cmdShowAllBeacons(BasePlayer player, string command, string[] args);
    [HookMethod("SendHelpText")]
    private void SendHelpText(BasePlayer player);
}


```

---

## Pets

```csharp
using System.Collections.Generic;
using System;
using System.Reflection;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Pets", "Bombardir", "0.5.5", ResourceId = 851)]
 class Pets : RustPlugin
{
    static Pets PluginInstance;
    static BUTTON MainButton;
    static BUTTON SecondButton;
    static Dictionary<ulong, PetInfo> SaveNpcList;
    public class NpcControl : MonoBehaviour
    {
        private readonly FieldInfo serverinput;
        private float ButtonReload;
        private float DrawReload;
        internal static float LootDistance;
        internal static float ReloadControl;
        internal static float MaxControlDistance;
        internal bool DrawEnabled;
         InputState input;
         float NextTimeToPress;
         float NextTimeToControl;
         float NextTimeToDraw;
        public NpcAI npc;
        public BasePlayer owner;
         void Awake();
         void OnAttacked(HitInfo info);
         void FixedUpdate();
         void UpdateDraw();
         void UpdateAction();
         void OpenPetInventory();
         void ChangeFollowAction();
         void TryGetNewPet(BaseNPC npcPet);
    }

    public class NpcAI : MonoBehaviour
    {
        private readonly MethodInfo SetDeltaTimeMethod;
        internal static float IgnoreTargetDistance;
        internal static float HealthModificator;
        internal static float AttackModificator;
        internal static float SpeedModificator;
        private float PointMoveDistance;
        private float TargetMoveDistance;
         float lastTick;
         float hungerLose;
         float thristyLose;
         float sleepLose;
         double attackrange;
        internal Act action;
        internal Vector3 targetpoint;
        internal BaseCombatEntity targetentity;
        public NpcControl owner;
        public ItemContainer inventory;
        public BaseNPC Base;
        public NPCAI RustAI;
        public NPCMetabolism RustMetabolism;
         void Awake();
         void OnDestroy();
        internal void OnAttacked(HitInfo info);
         void FixedUpdate();
         void Sleep();
         void Move(Vector3 point);
        internal void Attack(BaseCombatEntity ent, Act act);
    }

    public class PetInfo
    {
        public uint prefabID;
        public float x;
        public float y;
        public float z;
        public byte[] inventory;
        internal bool NeedToSpawn;
        public PetInfo();
        public PetInfo(NpcAI pet);
    }

    static bool UsePermission;
    static bool GlobalDraw;
    static string CfgButton;
    static string CfgSecButton;
    static string OpenInvMsg;
    static string ReloadMsg;
    static string NewPetMsg;
    static string CloserMsg;
    static string NoPermPetMsg;
    static string FollowMsg;
    static string UnFollowMsg;
    static string SleepMsg;
    static string AttackMsg;
    static string NoPermMsg;
    static string ActivatedMsg;
    static string DeactivatedMsg;
    static string NotNpc;
    static string NpcFree;
    static string NoOwn;
    static string EatMsg;
    static string DrawEn;
    static string DrawDis;
    static string DrawSysDis;
    static string InfoMsg;
    protected override void LoadDefaultConfig();
     void Init();
     void Unload();
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     void OnPlayerInit(BasePlayer player);
     void OnServerSave();
    [ChatCommand("pet")]
     void pet(BasePlayer player, string command, string[] args);
     bool HasPermission(BasePlayer player, string perm);
    static void DestroyAll();
     void CheckCfg(string Key, T var);
    static BUTTON ConvertStringToButton(string button);
}

public class NpcControl : MonoBehaviour
{
    private readonly FieldInfo serverinput;
    private float ButtonReload;
    private float DrawReload;
    internal static float LootDistance;
    internal static float ReloadControl;
    internal static float MaxControlDistance;
    internal bool DrawEnabled;
     InputState input;
     float NextTimeToPress;
     float NextTimeToControl;
     float NextTimeToDraw;
    public NpcAI npc;
    public BasePlayer owner;
     void Awake();
     void OnAttacked(HitInfo info);
     void FixedUpdate();
     void UpdateDraw();
     void UpdateAction();
     void OpenPetInventory();
     void ChangeFollowAction();
     void TryGetNewPet(BaseNPC npcPet);
}

public class NpcAI : MonoBehaviour
{
    private readonly MethodInfo SetDeltaTimeMethod;
    internal static float IgnoreTargetDistance;
    internal static float HealthModificator;
    internal static float AttackModificator;
    internal static float SpeedModificator;
    private float PointMoveDistance;
    private float TargetMoveDistance;
     float lastTick;
     float hungerLose;
     float thristyLose;
     float sleepLose;
     double attackrange;
    internal Act action;
    internal Vector3 targetpoint;
    internal BaseCombatEntity targetentity;
    public NpcControl owner;
    public ItemContainer inventory;
    public BaseNPC Base;
    public NPCAI RustAI;
    public NPCMetabolism RustMetabolism;
     void Awake();
     void OnDestroy();
    internal void OnAttacked(HitInfo info);
     void FixedUpdate();
     void Sleep();
     void Move(Vector3 point);
    internal void Attack(BaseCombatEntity ent, Act act);
}

public class PetInfo
{
    public uint prefabID;
    public float x;
    public float y;
    public float z;
    public byte[] inventory;
    internal bool NeedToSpawn;
    public PetInfo();
    public PetInfo(NpcAI pet);
}


```

---

## Picklock

```csharp
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Picklock", "TopPlugin.ru", "1.0.2")]
public class Picklock : RustPlugin
{
    private PluginConfig _config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    public class CrateConfig
    {
        [JsonProperty("Prefab")]
        public string Prefab;
        [JsonProperty("Минимальное кол-во")]
        public int MinAmount;
        [JsonProperty("Максимальное кол-во")]
        public int MaxAmount;
        [JsonProperty("Шанс выпадения предмета [0.0-100.0]")]
        public float Chance;
    }

    private class PluginConfig
    {
        [JsonProperty("Время открытия замка [sec.]")]
        public float TimeUnlock;
        [JsonProperty("Вероятность открытия замка [%]")]
        public float ChanceUnlock;
        [JsonProperty("Расстояние от игрока до замка [m]")]
        public float DistanceUnlock;
        [JsonProperty("Открывать при помощи отмычки дверной замок? [true/false]")]
        public bool IsKeyLock;
        [JsonProperty("Открывать при помощи отмычки кодовый замок? [true/false]")]
        public bool IsCodeLock;
        [JsonProperty("Открывать только замки плагина Raidable Bases? [true/false]")]
        public bool OnlyRaidableBases;
        [JsonProperty("Открывать дверь после взлома замка? [true/false]")]
        public bool IsOpenDoor;
        [JsonProperty("Настройка появления отмычек в ящиках")]
        public List<CrateConfig> Crates;
        public static PluginConfig DefaultConfig();
    }

    private static Picklock ins;
     void Init();
     void OnServerInitialized();
     void Unload();
     void OnPlayerInput(BasePlayer player, InputState input);
     void OnLootSpawn(LootContainer container);
     object CanStackItem(Item item, Item targetItem);
     object CanCombineDroppedItem(DroppedItem drItem, DroppedItem anotherDrItem);
     Item OnItemSplit(Item item, int amount);
    [PluginReference]
     Plugin RaidableBases;
     List<ulong> UnlockPlayers;
     void Unlock(BasePlayer player, BaseLock key, float time);
     BaseEntity RaycastAll(Ray ray);
     Item GetPicklock(int amount);
    private bool isRaidableBases(BaseEntity entity);
     void UnlockGUI(BasePlayer player, float time);
    protected override void LoadDefaultMessages();
     string GetMessage(string langKey, ulong UID);
     string GetMessage(string langKey, ulong UID, object[] args);
     void MoveItem(BasePlayer player, Item item);
     int GetSpaceCountItem(BasePlayer player, string shortname, int stack, ulong skinID);
     void MoveInventoryItem(BasePlayer player, Item item);
     void MoveOutItem(BasePlayer player, Item item);
    [ConsoleCommand("givepicklock")]
     void ConsoleGivePicklock(ConsoleSystem.Arg arg);
}

public class CrateConfig
{
    [JsonProperty("Prefab")]
    public string Prefab;
    [JsonProperty("Минимальное кол-во")]
    public int MinAmount;
    [JsonProperty("Максимальное кол-во")]
    public int MaxAmount;
    [JsonProperty("Шанс выпадения предмета [0.0-100.0]")]
    public float Chance;
}

private class PluginConfig
{
    [JsonProperty("Время открытия замка [sec.]")]
    public float TimeUnlock;
    [JsonProperty("Вероятность открытия замка [%]")]
    public float ChanceUnlock;
    [JsonProperty("Расстояние от игрока до замка [m]")]
    public float DistanceUnlock;
    [JsonProperty("Открывать при помощи отмычки дверной замок? [true/false]")]
    public bool IsKeyLock;
    [JsonProperty("Открывать при помощи отмычки кодовый замок? [true/false]")]
    public bool IsCodeLock;
    [JsonProperty("Открывать только замки плагина Raidable Bases? [true/false]")]
    public bool OnlyRaidableBases;
    [JsonProperty("Открывать дверь после взлома замка? [true/false]")]
    public bool IsOpenDoor;
    [JsonProperty("Настройка появления отмычек в ящиках")]
    public List<CrateConfig> Crates;
    public static PluginConfig DefaultConfig();
}


```

---

## PillsHere

```csharp
using System;

Oxide.Plugins
[Info("PillsHere", "Wulf/lukespragg", "3.0.1", ResourceId = 1723)]
[Description("Recovers health, hunger, and thirst by set amounts when using rad pills")]
 class PillsHere : RustPlugin
{
    const string permUse;
     float healAmount;
     float hungerAmount;
     float thirstAmount;
    protected override void LoadDefaultConfig();
     void Init();
     void OnConsumableUse(Item item);
     T GetConfig(string name, T value);
}


```

---

## PilotEject

```csharp
using Facepunch;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core.Plugins;
using Rust;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("PilotEject", "k1lly0u", "3.1.2")]
[Description("A mini event where a helicopter malfunctions and the pilot has to eject")]
 class PilotEject : RustPlugin
{
    [PluginReference]
     Plugin CustomNPC;
     Plugin Kits;
     Plugin HeliRefuel;
    private List<ScientistNPC> scientistNPCs;
    private Hash<ulong, ItemContainer[]> scientistInventory;
    public static PilotEject Instance { get; set; }
    private const string ADMIN_PERM;
    private const string HELICOPTER_PREFAB;
    private const string PARACHUTE_PREFAB;
    private const string SMOKE_EFFECT;
    private const int LAND_LAYERS;
    private void Loaded();
    private void OnServerInitialized();
    private void OnEntitySpawned(BaseHelicopter baseHelicopter);
    private void OnEntityTakeDamage(BaseHelicopter baseHelicopter, HitInfo hitInfo);
    private void OnEntityKill(BaseHelicopter baseHelicopter);
    private void OnEntityKill(ScientistNPC scientist);
    private void OnHelicopterRetire(PatrolHelicopterAI patrolHelicopterAI);
    private object CanBeTargeted(BaseCombatEntity player, MonoBehaviour behaviour);
    private void OnEntityDeath(ScientistNPC scientistNpc, HitInfo hitInfo);
    private object CanPopulateLoot(ScientistNPC scientistNpc, NPCPlayerCorpse corpse);
    private object OnCorpsePopulate(ScientistNPC scientistNpc, NPCPlayerCorpse corpse);
    private object CanLootPlayer(ScientistNPC scientistNpc, BasePlayer player);
    private object OnPlayerAssist(ScientistNPC scientistNpc, BasePlayer player);
    private void Unload();
    private void RunAutomatedEvent();
    private EjectionComponent SpawnEntity();
    private static string GetGridString(Vector3 position);
    private static string NumberToString(int number);
    private static void StripInventory(BasePlayer player);
    private static void ClearContainer(ItemContainer container);
    private static void PopulateLoot(ItemContainer container, ConfigData.LootContainer loot);
    private void StoreInventory(ScientistNPC scientistNpc);
    private void MoveInventoryTo(ulong scientistId, LootableCorpse corpse);
    private class EjectionComponent : MonoBehaviour
    {
        internal static List<EjectionComponent> allHelicopters;
        internal BaseHelicopter Helicopter { get; set; }
        internal PatrolHelicopterAI AI { get; set; }
        private float actualHealth;
        internal bool ejectOverride;
        internal bool hasEjected;
        private void Awake();
        private void OnDestroy();
        internal void OnTakeDamage(HitInfo hitInfo);
        private void TryAutoEjection();
        internal void EjectPilot();
        private IEnumerator SpawnNPCs(BaseHelicopter baseHelicopter);
        private void DropLoot();
    }

    internal class ParachutePhysics : MonoBehaviour
    {
        internal static List<ParachutePhysics> _allParachutes;
        internal ScientistNPC Entity { get; set; }
        internal BaseHelicopter Helicopter { get; set; }
        private Transform tr;
        private Rigidbody rb;
        private BaseEntity parachute;
        private Vector3 currentWindVector;
        internal Vector3 crashSite;
        private bool isFalling;
        private bool wasWounded;
        private Vector3 DirectionTowardsCrash2D { get; set; }
        private void Awake();
        private void OnDestroy();
        private void Update();
        private void FixedUpdate();
        private void InitializeVelocity();
        private void DeployParachute();
        private Vector3 GetWindAtCurrentPos();
        private void OnParachuteLand();
    }

    [ChatCommand("pe")]
    private void cmdPilotEject(BasePlayer player, string command, string[] args);
    [ConsoleCommand("pe")]
    private void ccmdPilotEject(ConsoleSystem.Arg arg);
    private static ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Event Automation")]
        public AutomationOptions Automation { get; set; }
        [JsonProperty(PropertyName = "Ejection Options")]
        public EjectionOptions Ejection { get; set; }
        [JsonProperty(PropertyName = "Helicopter Options")]
        public HelicopterOptions Helicopter { get; set; }
        [JsonProperty(PropertyName = "Notification Options")]
        public NotificationOptions Notifications { get; set; }
        [JsonProperty(PropertyName = "NPC Options")]
        public NPCOptions NPC { get; set; }
        [JsonProperty(PropertyName = "Loot Container Options")]
        public Loot LootBoxes { get; set; }
        public class AutomationOptions
        {
            [JsonProperty(PropertyName = "Automatically spawn helicopters on a timer")]
            public bool AutoSpawn { get; set; }
            [JsonProperty(PropertyName = "Auto-spawn time minimum (seconds)")]
            public float Min { get; set; }
            [JsonProperty(PropertyName = "Auto-spawn time maximum (seconds)")]
            public float Max { get; set; }
            [JsonProperty(PropertyName = "Minimum amount of online players to trigger the event")]
            public int RequiredPlayers { get; set; }
            [JsonProperty(PropertyName = "Chance of game spawned helicopter becoming a PilotEject helicopter (x / 100)")]
            public float Chance { get; set; }
        }

        public class EjectionOptions
        {
            [JsonProperty(PropertyName = "Eject the pilot when the helicopter has been shot down")]
            public bool EjectOnKilled { get; set; }
            [JsonProperty(PropertyName = "Eject the pilot when the helicopter has been destroyed mid-air")]
            public bool EjectOnDeath { get; set; }
            [JsonProperty(PropertyName = "Eject the pilot randomly")]
            public bool EjectRandom { get; set; }
            [JsonProperty(PropertyName = "Show smoke when parachuting")]
            public bool ShowSmoke { get; set; }
            [JsonProperty(PropertyName = "Random ejection time minimum (seconds)")]
            public float Min { get; set; }
            [JsonProperty(PropertyName = "Random ejection time maximum (seconds)")]
            public float Max { get; set; }
            [JsonProperty(PropertyName = "Parachute drag force")]
            public float Drag { get; set; }
            [JsonProperty(PropertyName = "Wind force")]
            public float Wind { get; set; }
        }

        public class HelicopterOptions
        {
            [JsonProperty(PropertyName = "Helicopter spawns with tail rotor on fire")]
            public bool DamageEffects { get; set; }
            [JsonProperty(PropertyName = "Start health")]
            public float Health { get; set; }
        }

        public class NotificationOptions
        {
            [JsonProperty(PropertyName = "Show notification when helicopter has been shot down")]
            public bool Death { get; set; }
            [JsonProperty(PropertyName = "Show notification when helicopter malfunctions")]
            public bool Malfunction { get; set; }
            [JsonProperty(PropertyName = "Show notification when a NPC has been killed")]
            public bool NPCDeath { get; set; }
        }

        public class NPCOptions
        {
            [JsonProperty(PropertyName = "Amount of NPCs to spawn")]
            public int Amount { get; set; }
            [JsonProperty(PropertyName = "NPC display name (chosen at random)")]
            public string[] Names { get; set; }
            [JsonProperty(PropertyName = "NPC kit (chosen at random)")]
            public string[] Kits { get; set; }
            [JsonProperty(PropertyName = "NPC health")]
            public int Health { get; set; }
            [JsonProperty(PropertyName = "Chance of being wounded when landing (x / 100)")]
            public int WoundedChance { get; set; }
            [JsonProperty(PropertyName = "Chance of recovery from being wounded(x / 100)")]
            public int RecoveryChance { get; set; }
            [JsonProperty(PropertyName = "Amount of time the NPCs will be alive before suiciding (seconds) (0 = disabled)")]
            public int Lifetime { get; set; }
            [JsonProperty(PropertyName = "Roam distance from landing position")]
            public float RoamDistance { get; set; }
            [JsonProperty(PropertyName = "Sight range")]
            public float SightRange { get; set; }
            [JsonProperty(PropertyName = "Loot type (Default, Inventory, Random)")]
            public string LootType { get; set; }
            [JsonProperty(PropertyName = "Random loot items")]
            public LootContainer RandomItems { get; set; }
            [JsonProperty(PropertyName = "Can be targeted by turrets")]
            public bool TargetedByTurrets { get; set; }
        }

        public class Loot
        {
            [JsonProperty(PropertyName = "Amount of loot boxes to drop when pilot ejects")]
            public int Amount { get; set; }
            [JsonProperty(PropertyName = "Loot container items")]
            public LootContainer RandomItems { get; set; }
        }

        public class LootContainer
        {
            [JsonProperty(PropertyName = "Minimum amount of items")]
            public int Minimum { get; set; }
            [JsonProperty(PropertyName = "Maximum amount of items")]
            public int Maximum { get; set; }
            [JsonProperty(PropertyName = "Items")]
            public LootItem[] Items { get; set; }
        }

        public class LootItem
        {
            [JsonProperty(PropertyName = "Item shortname")]
            public string Name { get; set; }
            [JsonProperty(PropertyName = "Item skin ID")]
            public ulong Skin { get; set; }
            [JsonProperty(PropertyName = "Minimum amount of item")]
            public int Minimum { get; set; }
            [JsonProperty(PropertyName = "Maximum amount of item")]
            public int Maximum { get; set; }
            [JsonProperty(PropertyName = "Item weight (a larger number has more chance of being selected)")]
            public int Weight { get; set; }
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private static void Broadcast(string key, object[] args);
    private string Message(string key, ulong playerId);
    private Dictionary<string, string> Messages;
}

private class EjectionComponent : MonoBehaviour
{
    internal static List<EjectionComponent> allHelicopters;
    internal BaseHelicopter Helicopter { get; set; }
    internal PatrolHelicopterAI AI { get; set; }
    private float actualHealth;
    internal bool ejectOverride;
    internal bool hasEjected;
    private void Awake();
    private void OnDestroy();
    internal void OnTakeDamage(HitInfo hitInfo);
    private void TryAutoEjection();
    internal void EjectPilot();
    private IEnumerator SpawnNPCs(BaseHelicopter baseHelicopter);
    private void DropLoot();
}

internal class ParachutePhysics : MonoBehaviour
{
    internal static List<ParachutePhysics> _allParachutes;
    internal ScientistNPC Entity { get; set; }
    internal BaseHelicopter Helicopter { get; set; }
    private Transform tr;
    private Rigidbody rb;
    private BaseEntity parachute;
    private Vector3 currentWindVector;
    internal Vector3 crashSite;
    private bool isFalling;
    private bool wasWounded;
    private Vector3 DirectionTowardsCrash2D { get; set; }
    private void Awake();
    private void OnDestroy();
    private void Update();
    private void FixedUpdate();
    private void InitializeVelocity();
    private void DeployParachute();
    private Vector3 GetWindAtCurrentPos();
    private void OnParachuteLand();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Event Automation")]
    public AutomationOptions Automation { get; set; }
    [JsonProperty(PropertyName = "Ejection Options")]
    public EjectionOptions Ejection { get; set; }
    [JsonProperty(PropertyName = "Helicopter Options")]
    public HelicopterOptions Helicopter { get; set; }
    [JsonProperty(PropertyName = "Notification Options")]
    public NotificationOptions Notifications { get; set; }
    [JsonProperty(PropertyName = "NPC Options")]
    public NPCOptions NPC { get; set; }
    [JsonProperty(PropertyName = "Loot Container Options")]
    public Loot LootBoxes { get; set; }
    public class AutomationOptions
    {
        [JsonProperty(PropertyName = "Automatically spawn helicopters on a timer")]
        public bool AutoSpawn { get; set; }
        [JsonProperty(PropertyName = "Auto-spawn time minimum (seconds)")]
        public float Min { get; set; }
        [JsonProperty(PropertyName = "Auto-spawn time maximum (seconds)")]
        public float Max { get; set; }
        [JsonProperty(PropertyName = "Minimum amount of online players to trigger the event")]
        public int RequiredPlayers { get; set; }
        [JsonProperty(PropertyName = "Chance of game spawned helicopter becoming a PilotEject helicopter (x / 100)")]
        public float Chance { get; set; }
    }

    public class EjectionOptions
    {
        [JsonProperty(PropertyName = "Eject the pilot when the helicopter has been shot down")]
        public bool EjectOnKilled { get; set; }
        [JsonProperty(PropertyName = "Eject the pilot when the helicopter has been destroyed mid-air")]
        public bool EjectOnDeath { get; set; }
        [JsonProperty(PropertyName = "Eject the pilot randomly")]
        public bool EjectRandom { get; set; }
        [JsonProperty(PropertyName = "Show smoke when parachuting")]
        public bool ShowSmoke { get; set; }
        [JsonProperty(PropertyName = "Random ejection time minimum (seconds)")]
        public float Min { get; set; }
        [JsonProperty(PropertyName = "Random ejection time maximum (seconds)")]
        public float Max { get; set; }
        [JsonProperty(PropertyName = "Parachute drag force")]
        public float Drag { get; set; }
        [JsonProperty(PropertyName = "Wind force")]
        public float Wind { get; set; }
    }

    public class HelicopterOptions
    {
        [JsonProperty(PropertyName = "Helicopter spawns with tail rotor on fire")]
        public bool DamageEffects { get; set; }
        [JsonProperty(PropertyName = "Start health")]
        public float Health { get; set; }
    }

    public class NotificationOptions
    {
        [JsonProperty(PropertyName = "Show notification when helicopter has been shot down")]
        public bool Death { get; set; }
        [JsonProperty(PropertyName = "Show notification when helicopter malfunctions")]
        public bool Malfunction { get; set; }
        [JsonProperty(PropertyName = "Show notification when a NPC has been killed")]
        public bool NPCDeath { get; set; }
    }

    public class NPCOptions
    {
        [JsonProperty(PropertyName = "Amount of NPCs to spawn")]
        public int Amount { get; set; }
        [JsonProperty(PropertyName = "NPC display name (chosen at random)")]
        public string[] Names { get; set; }
        [JsonProperty(PropertyName = "NPC kit (chosen at random)")]
        public string[] Kits { get; set; }
        [JsonProperty(PropertyName = "NPC health")]
        public int Health { get; set; }
        [JsonProperty(PropertyName = "Chance of being wounded when landing (x / 100)")]
        public int WoundedChance { get; set; }
        [JsonProperty(PropertyName = "Chance of recovery from being wounded(x / 100)")]
        public int RecoveryChance { get; set; }
        [JsonProperty(PropertyName = "Amount of time the NPCs will be alive before suiciding (seconds) (0 = disabled)")]
        public int Lifetime { get; set; }
        [JsonProperty(PropertyName = "Roam distance from landing position")]
        public float RoamDistance { get; set; }
        [JsonProperty(PropertyName = "Sight range")]
        public float SightRange { get; set; }
        [JsonProperty(PropertyName = "Loot type (Default, Inventory, Random)")]
        public string LootType { get; set; }
        [JsonProperty(PropertyName = "Random loot items")]
        public LootContainer RandomItems { get; set; }
        [JsonProperty(PropertyName = "Can be targeted by turrets")]
        public bool TargetedByTurrets { get; set; }
    }

    public class Loot
    {
        [JsonProperty(PropertyName = "Amount of loot boxes to drop when pilot ejects")]
        public int Amount { get; set; }
        [JsonProperty(PropertyName = "Loot container items")]
        public LootContainer RandomItems { get; set; }
    }

    public class LootContainer
    {
        [JsonProperty(PropertyName = "Minimum amount of items")]
        public int Minimum { get; set; }
        [JsonProperty(PropertyName = "Maximum amount of items")]
        public int Maximum { get; set; }
        [JsonProperty(PropertyName = "Items")]
        public LootItem[] Items { get; set; }
    }

    public class LootItem
    {
        [JsonProperty(PropertyName = "Item shortname")]
        public string Name { get; set; }
        [JsonProperty(PropertyName = "Item skin ID")]
        public ulong Skin { get; set; }
        [JsonProperty(PropertyName = "Minimum amount of item")]
        public int Minimum { get; set; }
        [JsonProperty(PropertyName = "Maximum amount of item")]
        public int Maximum { get; set; }
        [JsonProperty(PropertyName = "Item weight (a larger number has more chance of being selected)")]
        public int Weight { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class AutomationOptions
{
    [JsonProperty(PropertyName = "Automatically spawn helicopters on a timer")]
    public bool AutoSpawn { get; set; }
    [JsonProperty(PropertyName = "Auto-spawn time minimum (seconds)")]
    public float Min { get; set; }
    [JsonProperty(PropertyName = "Auto-spawn time maximum (seconds)")]
    public float Max { get; set; }
    [JsonProperty(PropertyName = "Minimum amount of online players to trigger the event")]
    public int RequiredPlayers { get; set; }
    [JsonProperty(PropertyName = "Chance of game spawned helicopter becoming a PilotEject helicopter (x / 100)")]
    public float Chance { get; set; }
}

public class EjectionOptions
{
    [JsonProperty(PropertyName = "Eject the pilot when the helicopter has been shot down")]
    public bool EjectOnKilled { get; set; }
    [JsonProperty(PropertyName = "Eject the pilot when the helicopter has been destroyed mid-air")]
    public bool EjectOnDeath { get; set; }
    [JsonProperty(PropertyName = "Eject the pilot randomly")]
    public bool EjectRandom { get; set; }
    [JsonProperty(PropertyName = "Show smoke when parachuting")]
    public bool ShowSmoke { get; set; }
    [JsonProperty(PropertyName = "Random ejection time minimum (seconds)")]
    public float Min { get; set; }
    [JsonProperty(PropertyName = "Random ejection time maximum (seconds)")]
    public float Max { get; set; }
    [JsonProperty(PropertyName = "Parachute drag force")]
    public float Drag { get; set; }
    [JsonProperty(PropertyName = "Wind force")]
    public float Wind { get; set; }
}

public class HelicopterOptions
{
    [JsonProperty(PropertyName = "Helicopter spawns with tail rotor on fire")]
    public bool DamageEffects { get; set; }
    [JsonProperty(PropertyName = "Start health")]
    public float Health { get; set; }
}

public class NotificationOptions
{
    [JsonProperty(PropertyName = "Show notification when helicopter has been shot down")]
    public bool Death { get; set; }
    [JsonProperty(PropertyName = "Show notification when helicopter malfunctions")]
    public bool Malfunction { get; set; }
    [JsonProperty(PropertyName = "Show notification when a NPC has been killed")]
    public bool NPCDeath { get; set; }
}

public class NPCOptions
{
    [JsonProperty(PropertyName = "Amount of NPCs to spawn")]
    public int Amount { get; set; }
    [JsonProperty(PropertyName = "NPC display name (chosen at random)")]
    public string[] Names { get; set; }
    [JsonProperty(PropertyName = "NPC kit (chosen at random)")]
    public string[] Kits { get; set; }
    [JsonProperty(PropertyName = "NPC health")]
    public int Health { get; set; }
    [JsonProperty(PropertyName = "Chance of being wounded when landing (x / 100)")]
    public int WoundedChance { get; set; }
    [JsonProperty(PropertyName = "Chance of recovery from being wounded(x / 100)")]
    public int RecoveryChance { get; set; }
    [JsonProperty(PropertyName = "Amount of time the NPCs will be alive before suiciding (seconds) (0 = disabled)")]
    public int Lifetime { get; set; }
    [JsonProperty(PropertyName = "Roam distance from landing position")]
    public float RoamDistance { get; set; }
    [JsonProperty(PropertyName = "Sight range")]
    public float SightRange { get; set; }
    [JsonProperty(PropertyName = "Loot type (Default, Inventory, Random)")]
    public string LootType { get; set; }
    [JsonProperty(PropertyName = "Random loot items")]
    public LootContainer RandomItems { get; set; }
    [JsonProperty(PropertyName = "Can be targeted by turrets")]
    public bool TargetedByTurrets { get; set; }
}

public class Loot
{
    [JsonProperty(PropertyName = "Amount of loot boxes to drop when pilot ejects")]
    public int Amount { get; set; }
    [JsonProperty(PropertyName = "Loot container items")]
    public LootContainer RandomItems { get; set; }
}

public class LootContainer
{
    [JsonProperty(PropertyName = "Minimum amount of items")]
    public int Minimum { get; set; }
    [JsonProperty(PropertyName = "Maximum amount of items")]
    public int Maximum { get; set; }
    [JsonProperty(PropertyName = "Items")]
    public LootItem[] Items { get; set; }
}

public class LootItem
{
    [JsonProperty(PropertyName = "Item shortname")]
    public string Name { get; set; }
    [JsonProperty(PropertyName = "Item skin ID")]
    public ulong Skin { get; set; }
    [JsonProperty(PropertyName = "Minimum amount of item")]
    public int Minimum { get; set; }
    [JsonProperty(PropertyName = "Maximum amount of item")]
    public int Maximum { get; set; }
    [JsonProperty(PropertyName = "Item weight (a larger number has more chance of being selected)")]
    public int Weight { get; set; }
}


```

---

## PlaceAnything

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Globalization;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;
using Oxide.Game.Rust.Cui;
using VLB;

Oxide.Plugins
[Info("PlaceAnything", "David", "1.0.5")]
[Description("Place any entity you want.")]
public class PlaceAnything : RustPlugin
{
    [PluginReference]
    private Plugin CopyPaste;
    private Plugin EntityScaleManager;
    static PlaceAnything plugin;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnHammerHit(BasePlayer player, HitInfo info);
     void OnEntityBuilt(Planner plan, GameObject go);
    [ChatCommand("gimme")]
    private void gimme(BasePlayer player);
    private void RefundItem(BasePlayer player, string itemName);
    private void AddCompToAll();
    private void KillAllComps();
    private class Mono : MonoBehaviour
    {
         BasePlayer player;
         float progress;
         BaseEntity entity;
         string item;
         void Awake();
         void BarProg();
        public void RunPickUp(BasePlayer player, BaseEntity _entity, string itemName);
    }

    private void CreatePickUpBar(BasePlayer player);
    private void DestroyBar(BasePlayer player);
    private void CreateProgressBar(BasePlayer player, float progress);
    private void SaveData();
    private Dictionary<string, Placeable> placeable;
    private class Placeable
    {
        public string BaseItem;
        public ulong SkinID;
        public string Prefab;
        public bool NeedsTCAuth;
        public bool CanBePickedUp;
        public float AdjustHeight;
    }

    private void LoadData();
    private void CreateExamples();
    public class CUIClass
    {
        public static CuiElementContainer CreateOverlay(string _name, string _color, string _anchorMin, string _anchorMax, bool _cursorOn, float _fade, string _mat);
        public static void CreatePanel(CuiElementContainer _container, string _name, string _parent, string _color, string _anchorMin, string _anchorMax, bool _cursorOn, float _fadeIn, float _fadeOut, string _mat2, string _OffsetMin, string _OffsetMax);
        public static void CreateImage(CuiElementContainer _container, string _name, string _parent, string _image, string _anchorMin, string _anchorMax, float _fadeIn, float _fadeOut, string _OffsetMin, string _OffsetMax);
        public static void PullFromAssets(CuiElementContainer _container, string _name, string _parent, string _color, string _sprite, float _fadeIn, float _fadeOut, string _anchorMin, string _anchorMax, string _material);
        public static void CreateInput(CuiElementContainer _container, string _name, string _parent, string _color, int _size, string _anchorMin, string _anchorMax, string _font, string _command, TextAnchor _align);
        public static void CreateText(CuiElementContainer _container, string _name, string _parent, string _color, string _text, int _size, string _anchorMin, string _anchorMax, TextAnchor _align, string _font, float _fadeIn, float _fadeOut, string _outlineColor, string _outlineScale);
        public static void CreateButton(CuiElementContainer _container, string _name, string _parent, string _color, string _text, int _size, string _anchorMin, string _anchorMax, string _command, string _close, string _textColor, float _fade, TextAnchor _align, string _font, string _material);
    }

    private Configuration config;
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     class Configuration
    {
        [JsonProperty(PropertyName = "Main Settings")]
        public Main main { get; set; }
        public class Main
        {
            [JsonProperty("Resize Entities - ('item name':'scale 0 to 1') ")]
            public Dictionary<string, float> resize { get; set; }
            [JsonProperty("Pick up on first hit")]
            public bool pickup { get; set; }
        }

        public static Configuration CreateConfig();
    }

}

private class Mono : MonoBehaviour
{
     BasePlayer player;
     float progress;
     BaseEntity entity;
     string item;
     void Awake();
     void BarProg();
    public void RunPickUp(BasePlayer player, BaseEntity _entity, string itemName);
}

private class Placeable
{
    public string BaseItem;
    public ulong SkinID;
    public string Prefab;
    public bool NeedsTCAuth;
    public bool CanBePickedUp;
    public float AdjustHeight;
}

public class CUIClass
{
    public static CuiElementContainer CreateOverlay(string _name, string _color, string _anchorMin, string _anchorMax, bool _cursorOn, float _fade, string _mat);
    public static void CreatePanel(CuiElementContainer _container, string _name, string _parent, string _color, string _anchorMin, string _anchorMax, bool _cursorOn, float _fadeIn, float _fadeOut, string _mat2, string _OffsetMin, string _OffsetMax);
    public static void CreateImage(CuiElementContainer _container, string _name, string _parent, string _image, string _anchorMin, string _anchorMax, float _fadeIn, float _fadeOut, string _OffsetMin, string _OffsetMax);
    public static void PullFromAssets(CuiElementContainer _container, string _name, string _parent, string _color, string _sprite, float _fadeIn, float _fadeOut, string _anchorMin, string _anchorMax, string _material);
    public static void CreateInput(CuiElementContainer _container, string _name, string _parent, string _color, int _size, string _anchorMin, string _anchorMax, string _font, string _command, TextAnchor _align);
    public static void CreateText(CuiElementContainer _container, string _name, string _parent, string _color, string _text, int _size, string _anchorMin, string _anchorMax, TextAnchor _align, string _font, float _fadeIn, float _fadeOut, string _outlineColor, string _outlineScale);
    public static void CreateButton(CuiElementContainer _container, string _name, string _parent, string _color, string _text, int _size, string _anchorMin, string _anchorMax, string _command, string _close, string _textColor, float _fade, TextAnchor _align, string _font, string _material);
}

 class Configuration
{
    [JsonProperty(PropertyName = "Main Settings")]
    public Main main { get; set; }
    public class Main
    {
        [JsonProperty("Resize Entities - ('item name':'scale 0 to 1') ")]
        public Dictionary<string, float> resize { get; set; }
        [JsonProperty("Pick up on first hit")]
        public bool pickup { get; set; }
    }

    public static Configuration CreateConfig();
}

public class Main
{
    [JsonProperty("Resize Entities - ('item name':'scale 0 to 1') ")]
    public Dictionary<string, float> resize { get; set; }
    [JsonProperty("Pick up on first hit")]
    public bool pickup { get; set; }
}


```

---

## Plagued

```csharp
using System.Reflection;
using System.Collections.Generic;
using System;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Ext.SQLite;
using UnityEngine;

Oxide.Plugins
[Info("Plagued", "Wernesgruner", "0.3.3")]
[Description("Everyone is infected.")]
 class Plagued : RustPlugin
{
    private static int plagueRange;
    private static int plagueIncreaseRate;
    private static int plagueDecreaseRate;
    private static int plagueMinAffinity;
    private static int affinityIncRate;
    private static int affinityDecRate;
    private static int maxKin;
    private static int maxKinChanges;
    private static int playerLayer;
    private static bool disableSleeperAffinity;
    private readonly FieldInfo serverinput;
    private static readonly Collider[] colBuffer;
    private Dictionary<ulong, PlayerState> playerStates;
    protected override void LoadDefaultConfig();
     void Unload();
     void OnServerInitialized();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void OnRunPlayerMetabolism(PlayerMetabolism metabolism);
     void OnPlayerProximity(BasePlayer player, BasePlayer[] players);
     void OnPlayerAlone(BasePlayer player);
    [ChatCommand("plagued")]
     void cmdPlagued(BasePlayer player, string command, string[] args);
    private void cmdAddKin(BasePlayer player);
    private bool cmdDelKin(BasePlayer player);
    private bool cmdDelKin(BasePlayer player, int id);
    private void cmdListKin(BasePlayer player);
    private void cmdListAssociates(BasePlayer player);
    private bool cmdInfo(BasePlayer player);
    public static void MsgPlayer(BasePlayer player, string format, object[] args);
    public void displayList(BasePlayer player, string listName, List<string> stringList);
    private bool getPlayerLookedAt(BasePlayer player, BasePlayer targetPlayer);
    private bool TryGetClosestRayPoint(Vector3 sourcePos, Quaternion sourceDir, object closestEnt, Vector3 closestHitpoint);
    private bool TryGetPlayerView(BasePlayer player, Quaternion viewAngle);
    public class PlayerState
    {
        private static readonly Oxide.Ext.SQLite.Libraries.SQLite sqlite;
        private static Core.Database.Connection sqlConnection;
        private BasePlayer player;
        private int id;
        private int plagueLevel;
        private int kinChangesCount;
        private bool pristine;
        private Dictionary<ulong, Association> associations;
        private Dictionary<ulong, Kin> kins;
        private List<ulong> kinRequests;
        private const string UpdateAssociation;
        private const string InsertAssociation;
        private const string CheckAssociationExists;
        private const string DeleteAssociation;
        private const string InsertPlayer;
        private const string SelectPlayer;
        private const string UpdatePlayerPlagueLevel;
        private const string SelectAssociations;
        private const string SelectKinList;
        private const string InsertKin;
        private const string DeleteKin;
        private const string SelectKinRequestList;
        public PlayerState(BasePlayer newPlayer, Func<PlayerState,bool> callback);
        public static void setupDatabase(RustPlugin plugin);
        public static void closeDatabase();
        private Association increaseAssociateAffinity(BasePlayer associate);
        public void increasePlaguePenalty(BasePlayer[] associates);
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
        private Kin createKin(ulong kinUserId);
        private void createAssociation(ulong associate_user_id, Func<Association, bool> callback);
        private void syncPlagueLevel();
        private void loadAssociations();
        private void loadKinList();
        private void loadKinRequestList();
        private class Association
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

        private class Kin
        {
            public int self_id;
            public int kin_id;
            public ulong kin_user_id;
            public string kin_name;
            public int player_one_id;
            public int player_two_id;
            private Kin();
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
         void notifyPlayerProximity(BasePlayer[] players);
         void notifyPlayerAlone();
    }

}

public class PlayerState
{
    private static readonly Oxide.Ext.SQLite.Libraries.SQLite sqlite;
    private static Core.Database.Connection sqlConnection;
    private BasePlayer player;
    private int id;
    private int plagueLevel;
    private int kinChangesCount;
    private bool pristine;
    private Dictionary<ulong, Association> associations;
    private Dictionary<ulong, Kin> kins;
    private List<ulong> kinRequests;
    private const string UpdateAssociation;
    private const string InsertAssociation;
    private const string CheckAssociationExists;
    private const string DeleteAssociation;
    private const string InsertPlayer;
    private const string SelectPlayer;
    private const string UpdatePlayerPlagueLevel;
    private const string SelectAssociations;
    private const string SelectKinList;
    private const string InsertKin;
    private const string DeleteKin;
    private const string SelectKinRequestList;
    public PlayerState(BasePlayer newPlayer, Func<PlayerState,bool> callback);
    public static void setupDatabase(RustPlugin plugin);
    public static void closeDatabase();
    private Association increaseAssociateAffinity(BasePlayer associate);
    public void increasePlaguePenalty(BasePlayer[] associates);
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
    private Kin createKin(ulong kinUserId);
    private void createAssociation(ulong associate_user_id, Func<Association, bool> callback);
    private void syncPlagueLevel();
    private void loadAssociations();
    private void loadKinList();
    private void loadKinRequestList();
    private class Association
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

    private class Kin
    {
        public int self_id;
        public int kin_id;
        public ulong kin_user_id;
        public string kin_name;
        public int player_one_id;
        public int player_two_id;
        private Kin();
        public Kin(int p_self_id);
        public void create();
        public void load(Dictionary<string, object> kin);
    }

}

private class Association
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

private class Kin
{
    public int self_id;
    public int kin_id;
    public ulong kin_user_id;
    public string kin_name;
    public int player_one_id;
    public int player_two_id;
    private Kin();
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
     void notifyPlayerProximity(BasePlayer[] players);
     void notifyPlayerAlone();
}


```

---

## PlayerChallenges

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("PlayerChallenges", "k1lly0u", "2.0.36", ResourceId = 1442)]
 class PlayerChallenges : RustPlugin
{
    [PluginReference]
     Plugin BetterChat;
    [PluginReference]
     Plugin EventManager;
    [PluginReference]
     Plugin LustyMap;
    [PluginReference]
     Plugin Clans;
    [PluginReference]
     Plugin Friends;
     ChallengeData chData;
    private DynamicConfigFile data;
    private Dictionary<ulong, StatData> statCache;
    private Dictionary<CTypes, LeaderData> titleCache;
    private bool UIDisabled;
     class PCUI
    {
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align, float fadein);
        static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, float fadein);
    }

    private Dictionary<string, string> UIColors;
    static string UIMain;
    static string UIPanel;
    private void CreateMenu(BasePlayer player);
    private void CreateMenuContents(BasePlayer player, int page);
    private void AddMenuStats(CuiElementContainer MenuElement, string panel, string type, string posMinA, string posMaxA, string posMinB, string posMaxB);
    [ConsoleCommand("PCUI_ChangePage")]
    private void cmdChangePage(ConsoleSystem.Arg arg);
    [ConsoleCommand("PCUI_DestroyAll")]
    private void cmdDestroyAll(ConsoleSystem.Arg arg);
    private string GetLeaders(CTypes type);
    private object GetTypeFromString(string name);
    private void DestroyUI(BasePlayer player);
    private void CloseMap(BasePlayer player);
    private void OpenMap(BasePlayer player);
    private bool IsPlaying(BasePlayer player);
    private bool IsClanmate(ulong playerId, ulong friendId);
    private bool IsFriend(ulong playerId, ulong friendId);
     void Loaded();
     void OnServerInitialized();
     void Unload();
     void OnRocketLaunched(BasePlayer player, BaseEntity entity);
     void OnHealingItemUse(HeldEntity item, BasePlayer target);
     void OnItemCraftFinished(ItemCraftTask task, Item item);
     void OnPlantGather(PlantEntity plant, Item item, BasePlayer player);
     void OnCollectiblePickup(Item item, BasePlayer player, CollectibleEntity entity);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnEntityBuilt(Planner plan, GameObject go);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
     void OnStructureRepair(BaseCombatEntity block, BasePlayer player);
    [HookMethod("CompletedQuest")]
    public void CompletedQuest(BasePlayer player);
    private void AddPoints(BasePlayer player, CTypes type, int amount);
    private void AddDistance(BasePlayer player, CTypes type, int amount);
    private void CheckForUpdate(BasePlayer player, CTypes type);
    private void SwitchLeader(ulong ID, CTypes type);
    private void AddAllUsergroups();
    private void RemoveAllUsergroups();
    private void CheckUpdateTimer();
    [ChatCommand("pc")]
    private void cmdPC(BasePlayer player, string command, string[] args);
    [ChatCommand("pc_wipe")]
    private void cmdPCWipe(BasePlayer player, string command, string[] args);
    private void CheckEntry(BasePlayer player);
    private string GetGroupName(CTypes type);
    static double GrabCurrentTime();
    private void RegisterGroups();
    private void RegisterGroup(CTypes type);
    private bool GroupExists(string name);
    private bool NewGroup(string name);
    private bool UserInGroup(string ID, string name);
    private bool AddToGroup(string ID, string name);
    private bool RemoveFromGroup(string ID, string name);
    private object SetGroupTitle(string name);
    private object SetGroupColor(string name);
    private ConfigData configData;
     class ConfigData
    {
        public Titles Titles { get; set; }
        public Dictionary<string, bool> ActiveChallengeTypes { get; set; }
        public Options Options { get; set; }
        public Messaging Messaging { get; set; }
        public List<string> UI_Arrangement { get; set; }
    }

     class Titles
    {
        public string ThrownExplosives;
        public string RocketsFired;
        public string ArrowKills;
        public string PlayersKilled;
        public string Headshots;
        public string AnimalsKilled;
        public string StructuresBuilt;
        public string WoodGathered;
        public string RocksGathered;
        public string PlantsGathered;
        public string ClothesCrafted;
        public string WeaponsCrafted;
        public string PlayersHealed;
        public string RevolverKills;
        public string StructuresRepaired;
        public string MeleeKills;
        public string BladeKills;
        public string QuestsCompleted;
        public string PVPKillDistance;
        public string PVEKillDistance;
    }

     class Options
    {
        public bool IgnoreSleepers;
        public bool UseBetterChat;
        public bool IgnoreAdmins;
        public bool IgnoreEventKills;
        public bool AnnounceNewLeaders;
        public bool UseUpdateTimer;
        public int UpdateTimer;
        public int SaveTimer;
    }

     class Messaging
    {
        public string MSG_ColorMain;
        public string MSG_ColorMsg;
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     void SaveLoop();
     void SaveData();
     void LoadData();
     void CheckValidData();
     class ChallengeData
    {
        public Dictionary<ulong, StatData> Stats;
        public Dictionary<CTypes, LeaderData> Titles;
        public double LastUpdate;
    }

     class StatData
    {
        public string DisplayName;
        public Dictionary<CTypes, int> Stats;
    }

     class LeaderData
    {
        public ulong UserID;
        public string DisplayName;
        public int Count;
    }

     List<CTypes> typeList;
     List<string> meleeShortnames;
     List<string> bladeShortnames;
     List<string> plantShortnames;
    private string MSG(string key, string id);
     Dictionary<string, string> Messages;
}

 class PCUI
{
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align, float fadein);
    static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, float fadein);
}

 class ConfigData
{
    public Titles Titles { get; set; }
    public Dictionary<string, bool> ActiveChallengeTypes { get; set; }
    public Options Options { get; set; }
    public Messaging Messaging { get; set; }
    public List<string> UI_Arrangement { get; set; }
}

 class Titles
{
    public string ThrownExplosives;
    public string RocketsFired;
    public string ArrowKills;
    public string PlayersKilled;
    public string Headshots;
    public string AnimalsKilled;
    public string StructuresBuilt;
    public string WoodGathered;
    public string RocksGathered;
    public string PlantsGathered;
    public string ClothesCrafted;
    public string WeaponsCrafted;
    public string PlayersHealed;
    public string RevolverKills;
    public string StructuresRepaired;
    public string MeleeKills;
    public string BladeKills;
    public string QuestsCompleted;
    public string PVPKillDistance;
    public string PVEKillDistance;
}

 class Options
{
    public bool IgnoreSleepers;
    public bool UseBetterChat;
    public bool IgnoreAdmins;
    public bool IgnoreEventKills;
    public bool AnnounceNewLeaders;
    public bool UseUpdateTimer;
    public int UpdateTimer;
    public int SaveTimer;
}

 class Messaging
{
    public string MSG_ColorMain;
    public string MSG_ColorMsg;
}

 class ChallengeData
{
    public Dictionary<ulong, StatData> Stats;
    public Dictionary<CTypes, LeaderData> Titles;
    public double LastUpdate;
}

 class StatData
{
    public string DisplayName;
    public Dictionary<CTypes, int> Stats;
}

 class LeaderData
{
    public ulong UserID;
    public string DisplayName;
    public int Count;
}


```

---

## PlayerChatcontrol

```csharp
using System.Collections.Generic;
using System.Reflection;
using System;
using System.Data;
using UnityEngine;
using Oxide.Core;

Oxide.Plugins
[Info("Player Chatcontrol", "LaserHydra", "2.0.0", ResourceId = 866)]
[Description("Write as another player")]
 class PlayerChatcontrol : RustPlugin
{
    [ChatCommand("talk")]
     void cmdTalk(BasePlayer player, string cmd, string[] args);
     void ForcePlayerChat(BasePlayer target, string message);
     string[] GetPlayer(string searchedPlayer);
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}


```

---

## PlayerCounter

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("PlayerCounter", "Bamabo", "1.5.1")]
[Description("Adds a discrete player counter to the HUD")]
 class PlayerCounter : RustPlugin
{
    public class PlayerPreferences
    {
        public string position { get; set; }
        public bool toggle { get; set; }
        public CuiElementContainer elements { get; set; }
        public string container { get; set; }
        public CuiLabel playerCounter { get; set; }
        public PlayerPreferences();
        public PlayerPreferences(string position, bool toggle, int fontSize, string color);
    }

    private Dictionary<ulong, PlayerPreferences> preferences;
    private int fontSize { get; set; }
    private string iconID { get; set; }
    private string color { get; set; }
    private bool usePerms { get; set; }
    private string defaultPos { get; set; }
    private bool showSleepers { get; set; }
     void Init();
     void Loaded();
     void Unload();
     void OnPlayerSleepEnded(BasePlayer player);
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
    [ChatCommand("playercounter")]
     void cmdPlayerCounter(BasePlayer sender, string command, String[] args);
     void UpdateCounterForPlayer(BasePlayer player);
     void UpdateCounter();
     void AlignLeft(ulong userID);
     void AlignMiddle(ulong userID);
     void AlignRight(ulong userID);
    protected override void LoadDefaultConfig();
     T GetConfigEntry(string configEntry, T defaultValue);
     string GetUpdatedCounterText(ulong userID);
     void RegisterMessages();
}

public class PlayerPreferences
{
    public string position { get; set; }
    public bool toggle { get; set; }
    public CuiElementContainer elements { get; set; }
    public string container { get; set; }
    public CuiLabel playerCounter { get; set; }
    public PlayerPreferences();
    public PlayerPreferences(string position, bool toggle, int fontSize, string color);
}


```

---

## PlayerDatabase

```csharp
using System.Collections.Generic;
using System;
using System.Reflection;
using System.Data;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Logging;
using Oxide.Core.Plugins;
using Rust;

Oxide.Plugins
[Info("PlayerDatabase", "Reneb", "1.0.5")]
 class PlayerDatabase : RustPlugin
{
    public static DataFileSystem datafile;
     string subDirectory;
     Hash<string, Dictionary<string, Dictionary<string, object>>> playersData;
     List<string> changedPlayersData;
     StoredData storedData;
     class StoredData
    {
        public HashSet<string> knownPlayers;
        public StoredData();
    }

     void OnServerSave();
     void SaveData();
     void LoadData();
     bool isKnownPlayer(BasePlayer player);
     bool isKnownPlayer(string userid);
     void Loaded();
     void SavePlayerDatabase();
     void Unload();
     void OnPlayerInit(BasePlayer player);
     HashSet<string> GetAllKnownPlayers();
     object FindPlayer(string arg);
     void LoadPlayer(BasePlayer player);
     void LoadPlayer(string userid);
     void SetPlayerData(string userid, string key, Dictionary<string, object> data);
     object GetPlayerData(string userid, string key);
}

 class StoredData
{
    public HashSet<string> knownPlayers;
    public StoredData();
}


```

---

## PlayerGifts

```csharp
using Oxide.Core;
using System.Collections.Generic;
using System.Linq;
using System;
using UnityEngine;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("Player Gifts", "TopPlugin.ru", "2.0.1")]
 class PlayerGifts : RustPlugin
{
     Dictionary<BasePlayer, int> timers;
     List<ulong> activePlayers;
    public int GameActive;
     string EnableGUIPlayerMin;
     string EnableGUIPlayerMax;
     string GUIEnabledColor;
     string GUIColor;
     string ImagesGUI;
    private void LoadDefaultConfig();
    private void GetConfig(string menu, string Key, T var);
     bool CanTake(BasePlayer player);
     bool TakeGifts(BasePlayer player, string gift);
    public BasePlayer FindBasePlayer(string nameOrUserId);
     void TimerHandler();
     void UpdateTimer(BasePlayer player);
     void DeactivateTimer(BasePlayer player);
     void ActivateTimer(ulong userId);
    public static string FormatTime(TimeSpan time);
    private static string Format(int units, string form1, string form2, string form3);
    [ChatCommand("gift")]
     void cmdGiveGift(BasePlayer player);
    [ConsoleCommand("getGift")]
     void CmdGetGift(ConsoleSystem.Arg arg);
     int getExperiencePercentInt(int skill);
     void DrawUIBalance(BasePlayer player, int seconds);
     void DrawUIGetGift(BasePlayer player);
     void DrawUIPlayer(BasePlayer player);
     void DestroyUIPlayer(BasePlayer player);
     void Unload();
     void OnPlayerDisconnected(BasePlayer player);
     void OnServerInitialized();
     void LoadData();
     void SaveData();
     void OnServerSave();
     void OnPlayerConnected(BasePlayer player);
     class DataStorage
    {
        public Dictionary<ulong, GiftsData> GiftPlayers;
        public DataStorage();
    }

     class GiftsData
    {
        public string Name;
        public int ActiveGifts;
        public int Time;
    }

     DataStorage data;
    private DynamicConfigFile GiftData;
    static PlayerGifts instance;
    public class GiftDefinition
    {
        public string Type;
        public List<CaseItem> Items;
        public CaseItem Open();
    }

    public class CaseItem
    {
        public string Shortname;
        public int Min;
        public int Max;
        public int GetRandom();
    }

    public Dictionary<string, GiftDefinition> gifts;
     Dictionary<string, string> Messages;
}

 class DataStorage
{
    public Dictionary<ulong, GiftsData> GiftPlayers;
    public DataStorage();
}

 class GiftsData
{
    public string Name;
    public int ActiveGifts;
    public int Time;
}

public class GiftDefinition
{
    public string Type;
    public List<CaseItem> Items;
    public CaseItem Open();
}

public class CaseItem
{
    public string Shortname;
    public int Min;
    public int Max;
    public int GetRandom();
}


```

---

## PlayerInformations

```csharp
using System.Collections.Generic;
using System;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("PlayerInformations", "Reneb", "1.0.5", ResourceId = 1345)]
[Description("Logs players informations.")]
public class PlayerInformations : RustPlugin
{
    [PluginReference]
     Plugin PlayerDatabase;
    private static bool IPuse;
    private static string IPpermission;
    private static int IPauthlevel;
    private static int IPmaxLogs;
    private static bool NAMESuse;
    private static string NAMESpermission;
    private static int NAMESauthlevel;
    private static int NAMESmaxLogs;
    private static bool FCuse;
    private static string FCpermission;
    private static int FCauthlevel;
    private static bool LSuse;
    private static string LSpermission;
    private static int LSauthlevel;
    private static bool LPuse;
    private static string LPpermission;
    private static int LPauthlevel;
    private static bool TPuse;
    private static string TPpermission;
    private static int TPauthlevel;
    static DateTime epoch;
     void Loaded();
     void LoadDefaultConfig();
    private void CheckCfg(string Key, T var);
     void Init();
     void OnServerInitialized();
     void OnEntityDeath(BaseCombatEntity ent, HitInfo info);
     void Unload();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
    [HookMethod("SendHelpText")]
    private void SendHelpText(BasePlayer player);
    static double LogTime();
     string TimeMinToString(string time);
     string TimeMinToString(double time);
     string SecondsToString(string time);
     string SecondsToString(decimal time);
    private object FindPlayer(string arg);
    private BasePlayer FindBasePlayerPlayer(ulong steamid);
     bool hasPermission(BasePlayer player, int authlevel, string permissionName);
     void RecordIP(BasePlayer player);
    [ChatCommand("lastips")]
     void cmdChatLastIps(BasePlayer player, string command, string[] args);
    [ChatCommand("ipowners")]
     void cmdChatIps(BasePlayer player, string command, string[] args);
     void RecordLastSeen(BasePlayer player);
    [ChatCommand("lastseen")]
    private void cmdChatLastseen(BasePlayer player, string command, string[] args);
     void RecordFirstConnection(BasePlayer player);
    [ChatCommand("firstconnection")]
    private void cmdChatfirstconnection(BasePlayer player, string command, string[] args);
     void RecordName(BasePlayer player);
    [ChatCommand("lastnames")]
    private void cmdChatLastname(BasePlayer player, string command, string[] args);
     Dictionary<BasePlayer, double> recordPlayTime;
     void StartRecordTime(BasePlayer player);
     void EndRecordTime(BasePlayer player);
    [ChatCommand("played")]
    private void cmdChatPlayed(BasePlayer player, string command, string[] args);
    [ChatCommand("lastposition")]
    private void cmdChatLastPosition(BasePlayer player, string command, string[] args);
}


```

---

## PlayerRadar

```csharp
using System.Collections.Generic;
using System;
using System.Reflection;
using System.Data;
using UnityEngine;
using Oxide.Core;

Oxide.Plugins
[Info("PlayerRadar", "Reneb", "1.0.2", ResourceId = 1326)]
 class PlayerRadar : RustPlugin
{
    private static int authlevel;
    private static FieldInfo serverinput;
    static int playerCol;
    private static string xmin;
    private static string xmax;
    private static string ymin;
    private static string ymax;
    private static string refreshSpeed;
    private static bool showSleepers;
    private static string permName;
    private static string playerColor;
    private static string npcColor;
    private static string sleeperColor;
    private static string radarUrl;
    private static float refreshspeed;
     void LoadDefaultConfig();
    private void CheckCfg(string Key, T var);
     void Init();
     void Unload();
     void Loaded();
    public class RadarClass : MonoBehaviour
    {
        public BasePlayer player;
        public float distance;
         void Awake();
         void Radar();
         void OnDestroy();
    }

    public static string radaroverlay;
    public static string radarunderlay;
    public static string playerjson;
    static void ShowGUI(BasePlayer player, float distance);
    [ChatCommand("radar")]
     void cmdChatRadar(BasePlayer player, string command, string[] args);
}

public class RadarClass : MonoBehaviour
{
    public BasePlayer player;
    public float distance;
     void Awake();
     void Radar();
     void OnDestroy();
}


```

---

## PlayerRankings

```csharp
using System.Collections.Generic;
using Oxide.Core.Plugins;
using System.Linq;
using System;

Oxide.Plugins
[Info("PlayerRankings", "Ankawi", "2.0.2")]
[Description("Gives players ranks based on playtime on a server")]
 class PlayerRankings : RustPlugin
{
    [PluginReference]
     Plugin ConnectionDB;
    [PluginReference]
     Plugin BetterChat;
     void Loaded();
     bool IsUserInGroup(BasePlayer player, string group);
     void AddUserToGroup(BasePlayer player, string group);
     void RemoveUserFromGroup(BasePlayer player, string group);
     void CreateGroup(string group);
     bool GroupExists(string group);
    new void LoadConfig();
    protected override void LoadDefaultConfig();
     void SetConfig(object[] args);
    [ChatCommand("ranks")]
    private void RanksCommand(BasePlayer player, string command, string[] args);
     void UpdateGroups(BasePlayer player);
     void RevokeLower(BasePlayer player, double time);
     double GetPlayTime(BasePlayer player);
}


```

---

## PlayerSkins

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;
using Steamworks;
using System.Linq;
using System.Collections;
using System.Globalization;

Oxide.Plugins
[Info("PlayerSkins", "k1lly0u", "2.0.21")]
[Description("Fully Customization Skin Management Plugin - Sell Skinned items or Skins!")]
 class PlayerSkins : RustPlugin
{
    [PluginReference]
     Plugin Economics;
     Plugin ImageLibrary;
     Plugin ServerRewards;
    private Dictionary<string, Dictionary<ulong, SkinData>> skinData;
    private Dictionary<ulong, UserData> userData;
    private List<ulong> excludedSkins;
    private DynamicConfigFile userdata;
    private DynamicConfigFile skindata;
    private DynamicConfigFile excludeddata;
    private Dictionary<string, Dictionary<ulong, WorkshopItem>> workshopItems;
    private Dictionary<string, string> shortnameToDisplayname;
    private List<ulong> adminToggle;
    private List<ulong> ownedToggle;
    private List<ulong> approvedIds;
    private bool initialized;
    private bool workshopInitialized;
    private bool itemlistInitialized;
    private JsonSerializerSettings errorHandling;
    private static Dictionary<Colors, string> uiColor;
    private static bool forceHttp;
    private TokenType purchaseType;
    private DisplayMode forcedMode;
    private void Loaded();
    private void OnServerInitialized();
    private void OnServerSave();
    private void Unload();
    private void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
    private void OnUseNPC(BasePlayer npc, BasePlayer player);
    private void OnItemCraftFinished(ItemCraftTask task, Item item);
    private T ParseType(string type);
    private bool HasPermission(BasePlayer player, string perm);
    private void BroadcastAnnouncement();
    private void ChangeItemSkin(BasePlayer player, ulong targetSkin);
    private void ChangeItemSkin(Item item, ulong targetSkin);
    private Dictionary<string, string> workshopNameToShortname;
    private string[] blockedItems;
    private void StartApprovedRequest();
    private void GetApprovedItemSkins(List<ulong> itemsToDownload, int page);
    private IEnumerator ProcessApprovedBlock(List<ulong> itemsToDownload, PublishedFileQueryDetail[] items, int page, int totalPages);
    private List<ulong> BuildApprovedItemList();
    private const string publishedFileQuery;
    private const string publishedFileDetails;
    private int workshopPage;
    private int totalPages;
    private int totalCount;
    private int loadOrderCount;
    private void GetWorkshopSkins();
    private void WorkshopQuery(string queryStr);
    private IEnumerator ProcessWorkshopResponse(PublishedFileDetails[] response);
    private List<ulong> FindMissingSkins();
    private void FindSkinsWithNoData(List<ulong> invalidSkins, int page);
    private IEnumerator ProcessMissingBlock(List<ulong> invalidSkins, PublishedFileQueryDetail[] items, int page, int totalPages);
    private void InitializeWorkshop();
    private bool ContainsKeyword(string title);
    private bool IsValid(PublishedFileDetails item);
    private bool IsValid(PublishedFileQueryDetail item);
    private string BuildDetailsString(List<ulong> list, int page);
    private void QueueFileQueryRequest(string details, Action<PublishedFileQueryDetail[]> callback);
    private void ValidateDataShortnames();
    private string GetImage(string shortname, ulong skin);
    private string GetImageURL(string shortname, ulong skin);
    private bool AddImage(string url, string shortname, ulong skin);
    private bool HasImage(string shortname, ulong skin);
    private bool IsReady();
    [ChatCommand("skin")]
    private void cmdSkin(BasePlayer player, string command, string[] args);
    [ConsoleCommand("playerskins.skins")]
    private void ccmdSkinManager(ConsoleSystem.Arg arg);
    [ConsoleCommand("playerskins.setprice")]
    private void ccmdSetSkinPrice(ConsoleSystem.Arg arg);
    const string MainPanel;
    const string SelectPanel;
    const string PopupPanel;
    const string ReskinPanel;
    private void DestroyUI(BasePlayer player);
    public static class UI
    {
        static public CuiElementContainer Container(string panel, string color, UI4 dimensions, bool useCursor, string parent);
        static public void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions, bool cursor);
        static public void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
        static public void Button(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, string command, TextAnchor align);
        static public void Image(CuiElementContainer container, string panel, string png, UI4 dimensions);
        public static string Color(string hexColor, float alpha);
    }

    public class UI4
    {
        public float xMin;
        public float yMin;
        public float xMax;
        public float yMax;
        public UI4(float xMin, float yMin, float xMax, float yMax);
        public string GetMin();
        public string GetMax();
    }

    private void OpenSkinMenu(BasePlayer player, string shortname, int column, int page);
    private void CreateSelectionMenu(BasePlayer player, string shortname, int column, int page);
    private void CreateSmallSelectionMenu(BasePlayer player, string shortname, int column, int page);
    private void CreateItemPopup(BasePlayer player, string shortname, ulong skinId);
    private void CreateAdminPopup(CuiElementContainer container, string shortname, ulong skinId, ulong playerId);
    private void CreateReskinMenu(BasePlayer player, int page);
    private float[] GetItemPosition(int number, float offsetx, float offsety, float width, float height);
    private float[] GetButtonPosition(int number, int rows, float xOffset, float width, float yOffset, float height);
    private int RowNumber(int max, int count);
    private int GetBalance(ulong playerId);
    private bool TakeMoney(ulong playerId, int amount);
    private void GiveMoney(ulong playerId, int amount);
    private string FormatHelpText(DisplayMode displayMode, ulong playerId);
    [ConsoleCommand("psui.changepage")]
    private void ccmdChangePage(ConsoleSystem.Arg arg);
    [ConsoleCommand("psui.changeskinpage")]
    private void ccmdChangeSkinPage(ConsoleSystem.Arg arg);
    [ConsoleCommand("psui.reskinitem")]
    private void ccmdChangeItemSkin(ConsoleSystem.Arg arg);
    [ConsoleCommand("psui.selectitem")]
    private void ccmdSelectItem(ConsoleSystem.Arg arg);
    [ConsoleCommand("psui.exit")]
    private void ccmdExit(ConsoleSystem.Arg arg);
    [ConsoleCommand("psui.exitpopup")]
    private void ccmdExitPopup(ConsoleSystem.Arg arg);
    [ConsoleCommand("psui.remove")]
    private void ccmdRemoveSkin(ConsoleSystem.Arg arg);
    [ConsoleCommand("psui.sellskin")]
    private void ccmdSellSkin(ConsoleSystem.Arg arg);
    [ConsoleCommand("psui.setprice")]
    private void ccmdSetPrice(ConsoleSystem.Arg arg);
    [ConsoleCommand("psui.setpermission")]
    private void ccmdSetPermission(ConsoleSystem.Arg arg);
    [ConsoleCommand("psui.setdefault")]
    private void ccmdSetDefault(ConsoleSystem.Arg arg);
    [ConsoleCommand("psui.purchase")]
    private void ccmdPurchase(ConsoleSystem.Arg arg);
    [ConsoleCommand("psui.toggle")]
    private void ccmdToggleAdmin(ConsoleSystem.Arg arg);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Announcement Options")]
        public AnnouncementOptions Announcements { get; set; }
        [JsonProperty(PropertyName = "Purchase Options")]
        public PurchaseOptions Purchase { get; set; }
        [JsonProperty(PropertyName = "Skin Shop Options")]
        public ShopOptions Shop { get; set; }
        [JsonProperty(PropertyName = "Re-skin Options")]
        public ReskinOptions Reskin { get; set; }
        [JsonProperty(PropertyName = "Workshop Options")]
        public WorkshopOptions Workshop { get; set; }
        [JsonProperty(PropertyName = "UI Options")]
        public UIOptions UI { get; set; }
        public class AnnouncementOptions
        {
            [JsonProperty(PropertyName = "Display help information to players")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Information display interval (minutes)")]
            public int Interval { get; set; }
        }

        public class PurchaseOptions
        {
            [JsonProperty(PropertyName = "Enable purchase system")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Plugin used to purchase skins (ServerRewards, Economics)")]
            public string Type { get; set; }
        }

        public class ShopOptions
        {
            [JsonProperty(PropertyName = "Custom permissions which can be assigned to skins")]
            public string[] Permissions { get; set; }
            [JsonProperty(PropertyName = "NPC user IDs that players can interact with to open the skin shop")]
            public string[] NPCs { get; set; }
            [JsonProperty(PropertyName = "Disable the '/skin shop' command and force players to access it via a NPC")]
            public bool DisableCommand { get; set; }
            [JsonProperty(PropertyName = "Allow players to sell unwanted skins back to the skin store")]
            public bool SellSkins { get; set; }
            [JsonProperty(PropertyName = "Give player the item when they purchase a skin (this disables the reskin menu)")]
            public bool GiveItemOnPurchase { get; set; }
            [JsonProperty(PropertyName = "Forced display mode for skin shop (Full, Minimalist, None)")]
            public string ForcedMode { get; set; }
            [JsonProperty(PropertyName = "Send a help message to players when exiting the skin shop")]
            public bool HelpOnExit { get; set; }
            [JsonProperty(PropertyName = "List of shortnames for items to be blocked from appearing in the skin shop")]
            public string[] BlockedItems { get; set; }
        }

        public class ReskinOptions
        {
            [JsonProperty(PropertyName = "NPC user IDs that players can interact with to open the re-skin menu")]
            public string[] NPCs { get; set; }
            [JsonProperty(PropertyName = "Disable the '/skin' command and force players to access it via a NPC")]
            public bool DisableCommand { get; set; }
        }

        public class WorkshopOptions
        {
            [JsonProperty(PropertyName = "Disable approved skins from the skin shop")]
            public bool ApprovedDisabled { get; set; }
            [JsonProperty(PropertyName = "Enable workshop skins in the skin shop")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Word filter for workshop skins. If the skin title partially contains any of these words it will not be available as a potential skin")]
            public string[] Filter { get; set; }
            [JsonProperty(PropertyName = "Force image URLs to use HTTP instead of HTTPS")]
            public bool ForceHTTP { get; set; }
            [JsonProperty(PropertyName = "Steam API key (get one here https://steamcommunity.com/dev/apikey)")]
            public string SteamAPIKey { get; set; }
        }

        public class UIOptions
        {
            [JsonProperty(PropertyName = "UI Colors")]
            public Dictionary<Colors, UIColor> Colors { get; set; }
            public class UIColor
            {
                [JsonProperty(PropertyName = "Color (hex)")]
                public string Color { get; set; }
                [JsonProperty(PropertyName = "Alpha (0.0 - 1.0)")]
                public float Alpha { get; set; }
            }

        }

        public VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private void SaveUserData();
    private void SaveSkinData();
    private void LoadData();
    private class SkinData
    {
        public string permission;
        public int cost;
        public bool isDisabled;
    }

    private class UserData
    {
        public Dictionary<string, ulong> defaultSkins;
        public Dictionary<string, List<ulong>> purchasedSkins;
        public DisplayMode displayMode;
        public UserData();
        public bool IsDefaultSkin(string shortname, ulong skinId);
        public bool IsOwned(string shortname, ulong skinId);
    }

    private class WorkshopItem
    {
        public string title;
        public string description;
        public string imageUrl;
        public WorkshopItem();
        public WorkshopItem(PublishedFileDetails item);
        public WorkshopItem(PublishedFileQueryDetail item);
        public WorkshopItem(InventoryDef item, string url);
    }

    public class QueryResponse
    {
        public Response response;
    }

    public class Response
    {
        public int total;
        public PublishedFileDetails[] publishedfiledetails;
    }

    public class PublishedFileDetails
    {
        public int result;
        public string publishedfileid;
        public string creator;
        public int creator_appid;
        public int consumer_appid;
        public int consumer_shortcutid;
        public string filename;
        public string file_size;
        public string preview_file_size;
        public string file_url;
        public string preview_url;
        public string url;
        public string hcontent_file;
        public string hcontent_preview;
        public string title;
        public string file_description;
        public int time_created;
        public int time_updated;
        public int visibility;
        public int flags;
        public bool workshop_file;
        public bool workshop_accepted;
        public bool show_subscribe_all;
        public int num_comments_public;
        public bool banned;
        public string ban_reason;
        public string banner;
        public bool can_be_deleted;
        public string app_name;
        public int file_type;
        public bool can_subscribe;
        public int subscriptions;
        public int favorited;
        public int followers;
        public int lifetime_subscriptions;
        public int lifetime_favorited;
        public int lifetime_followers;
        public string lifetime_playtime;
        public string lifetime_playtime_sessions;
        public int views;
        public int num_children;
        public int num_reports;
        public Preview[] previews;
        public Tag[] tags;
        public int language;
        public bool maybe_inappropriate_sex;
        public bool maybe_inappropriate_violence;
        public class Tag
        {
            public string tag;
            public bool adminonly;
        }

    }

    public class PublishedFileQueryResponse
    {
        public FileResponse response { get; set; }
    }

    public class FileResponse
    {
        public int result { get; set; }
        public int resultcount { get; set; }
        public PublishedFileQueryDetail[] publishedfiledetails { get; set; }
    }

    public class PublishedFileQueryDetail
    {
        public string publishedfileid { get; set; }
        public int result { get; set; }
        public string creator { get; set; }
        public int creator_app_id { get; set; }
        public int consumer_app_id { get; set; }
        public string filename { get; set; }
        public int file_size { get; set; }
        public string preview_url { get; set; }
        public string hcontent_preview { get; set; }
        public string title { get; set; }
        public string description { get; set; }
        public int time_created { get; set; }
        public int time_updated { get; set; }
        public int visibility { get; set; }
        public int banned { get; set; }
        public string ban_reason { get; set; }
        public int subscriptions { get; set; }
        public int favorited { get; set; }
        public int lifetime_subscriptions { get; set; }
        public int lifetime_favorited { get; set; }
        public int views { get; set; }
        public Tag[] tags { get; set; }
        public class Tag
        {
            public string tag { get; set; }
        }

    }

    public class Preview
    {
        public string previewid;
        public int sortorder;
        public string url;
        public int size;
        public string filename;
        public int preview_type;
        public string youtubevideoid;
        public string external_reference;
    }

    private string msg(string key, ulong playerId);
    private Dictionary<string, string> Messages;
}

public static class UI
{
    static public CuiElementContainer Container(string panel, string color, UI4 dimensions, bool useCursor, string parent);
    static public void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions, bool cursor);
    static public void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
    static public void Button(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, string command, TextAnchor align);
    static public void Image(CuiElementContainer container, string panel, string png, UI4 dimensions);
    public static string Color(string hexColor, float alpha);
}

public class UI4
{
    public float xMin;
    public float yMin;
    public float xMax;
    public float yMax;
    public UI4(float xMin, float yMin, float xMax, float yMax);
    public string GetMin();
    public string GetMax();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Announcement Options")]
    public AnnouncementOptions Announcements { get; set; }
    [JsonProperty(PropertyName = "Purchase Options")]
    public PurchaseOptions Purchase { get; set; }
    [JsonProperty(PropertyName = "Skin Shop Options")]
    public ShopOptions Shop { get; set; }
    [JsonProperty(PropertyName = "Re-skin Options")]
    public ReskinOptions Reskin { get; set; }
    [JsonProperty(PropertyName = "Workshop Options")]
    public WorkshopOptions Workshop { get; set; }
    [JsonProperty(PropertyName = "UI Options")]
    public UIOptions UI { get; set; }
    public class AnnouncementOptions
    {
        [JsonProperty(PropertyName = "Display help information to players")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Information display interval (minutes)")]
        public int Interval { get; set; }
    }

    public class PurchaseOptions
    {
        [JsonProperty(PropertyName = "Enable purchase system")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Plugin used to purchase skins (ServerRewards, Economics)")]
        public string Type { get; set; }
    }

    public class ShopOptions
    {
        [JsonProperty(PropertyName = "Custom permissions which can be assigned to skins")]
        public string[] Permissions { get; set; }
        [JsonProperty(PropertyName = "NPC user IDs that players can interact with to open the skin shop")]
        public string[] NPCs { get; set; }
        [JsonProperty(PropertyName = "Disable the '/skin shop' command and force players to access it via a NPC")]
        public bool DisableCommand { get; set; }
        [JsonProperty(PropertyName = "Allow players to sell unwanted skins back to the skin store")]
        public bool SellSkins { get; set; }
        [JsonProperty(PropertyName = "Give player the item when they purchase a skin (this disables the reskin menu)")]
        public bool GiveItemOnPurchase { get; set; }
        [JsonProperty(PropertyName = "Forced display mode for skin shop (Full, Minimalist, None)")]
        public string ForcedMode { get; set; }
        [JsonProperty(PropertyName = "Send a help message to players when exiting the skin shop")]
        public bool HelpOnExit { get; set; }
        [JsonProperty(PropertyName = "List of shortnames for items to be blocked from appearing in the skin shop")]
        public string[] BlockedItems { get; set; }
    }

    public class ReskinOptions
    {
        [JsonProperty(PropertyName = "NPC user IDs that players can interact with to open the re-skin menu")]
        public string[] NPCs { get; set; }
        [JsonProperty(PropertyName = "Disable the '/skin' command and force players to access it via a NPC")]
        public bool DisableCommand { get; set; }
    }

    public class WorkshopOptions
    {
        [JsonProperty(PropertyName = "Disable approved skins from the skin shop")]
        public bool ApprovedDisabled { get; set; }
        [JsonProperty(PropertyName = "Enable workshop skins in the skin shop")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Word filter for workshop skins. If the skin title partially contains any of these words it will not be available as a potential skin")]
        public string[] Filter { get; set; }
        [JsonProperty(PropertyName = "Force image URLs to use HTTP instead of HTTPS")]
        public bool ForceHTTP { get; set; }
        [JsonProperty(PropertyName = "Steam API key (get one here https://steamcommunity.com/dev/apikey)")]
        public string SteamAPIKey { get; set; }
    }

    public class UIOptions
    {
        [JsonProperty(PropertyName = "UI Colors")]
        public Dictionary<Colors, UIColor> Colors { get; set; }
        public class UIColor
        {
            [JsonProperty(PropertyName = "Color (hex)")]
            public string Color { get; set; }
            [JsonProperty(PropertyName = "Alpha (0.0 - 1.0)")]
            public float Alpha { get; set; }
        }

    }

    public VersionNumber Version { get; set; }
}

public class AnnouncementOptions
{
    [JsonProperty(PropertyName = "Display help information to players")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Information display interval (minutes)")]
    public int Interval { get; set; }
}

public class PurchaseOptions
{
    [JsonProperty(PropertyName = "Enable purchase system")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Plugin used to purchase skins (ServerRewards, Economics)")]
    public string Type { get; set; }
}

public class ShopOptions
{
    [JsonProperty(PropertyName = "Custom permissions which can be assigned to skins")]
    public string[] Permissions { get; set; }
    [JsonProperty(PropertyName = "NPC user IDs that players can interact with to open the skin shop")]
    public string[] NPCs { get; set; }
    [JsonProperty(PropertyName = "Disable the '/skin shop' command and force players to access it via a NPC")]
    public bool DisableCommand { get; set; }
    [JsonProperty(PropertyName = "Allow players to sell unwanted skins back to the skin store")]
    public bool SellSkins { get; set; }
    [JsonProperty(PropertyName = "Give player the item when they purchase a skin (this disables the reskin menu)")]
    public bool GiveItemOnPurchase { get; set; }
    [JsonProperty(PropertyName = "Forced display mode for skin shop (Full, Minimalist, None)")]
    public string ForcedMode { get; set; }
    [JsonProperty(PropertyName = "Send a help message to players when exiting the skin shop")]
    public bool HelpOnExit { get; set; }
    [JsonProperty(PropertyName = "List of shortnames for items to be blocked from appearing in the skin shop")]
    public string[] BlockedItems { get; set; }
}

public class ReskinOptions
{
    [JsonProperty(PropertyName = "NPC user IDs that players can interact with to open the re-skin menu")]
    public string[] NPCs { get; set; }
    [JsonProperty(PropertyName = "Disable the '/skin' command and force players to access it via a NPC")]
    public bool DisableCommand { get; set; }
}

public class WorkshopOptions
{
    [JsonProperty(PropertyName = "Disable approved skins from the skin shop")]
    public bool ApprovedDisabled { get; set; }
    [JsonProperty(PropertyName = "Enable workshop skins in the skin shop")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Word filter for workshop skins. If the skin title partially contains any of these words it will not be available as a potential skin")]
    public string[] Filter { get; set; }
    [JsonProperty(PropertyName = "Force image URLs to use HTTP instead of HTTPS")]
    public bool ForceHTTP { get; set; }
    [JsonProperty(PropertyName = "Steam API key (get one here https://steamcommunity.com/dev/apikey)")]
    public string SteamAPIKey { get; set; }
}

public class UIOptions
{
    [JsonProperty(PropertyName = "UI Colors")]
    public Dictionary<Colors, UIColor> Colors { get; set; }
    public class UIColor
    {
        [JsonProperty(PropertyName = "Color (hex)")]
        public string Color { get; set; }
        [JsonProperty(PropertyName = "Alpha (0.0 - 1.0)")]
        public float Alpha { get; set; }
    }

}

public class UIColor
{
    [JsonProperty(PropertyName = "Color (hex)")]
    public string Color { get; set; }
    [JsonProperty(PropertyName = "Alpha (0.0 - 1.0)")]
    public float Alpha { get; set; }
}

private class SkinData
{
    public string permission;
    public int cost;
    public bool isDisabled;
}

private class UserData
{
    public Dictionary<string, ulong> defaultSkins;
    public Dictionary<string, List<ulong>> purchasedSkins;
    public DisplayMode displayMode;
    public UserData();
    public bool IsDefaultSkin(string shortname, ulong skinId);
    public bool IsOwned(string shortname, ulong skinId);
}

private class WorkshopItem
{
    public string title;
    public string description;
    public string imageUrl;
    public WorkshopItem();
    public WorkshopItem(PublishedFileDetails item);
    public WorkshopItem(PublishedFileQueryDetail item);
    public WorkshopItem(InventoryDef item, string url);
}

public class QueryResponse
{
    public Response response;
}

public class Response
{
    public int total;
    public PublishedFileDetails[] publishedfiledetails;
}

public class PublishedFileDetails
{
    public int result;
    public string publishedfileid;
    public string creator;
    public int creator_appid;
    public int consumer_appid;
    public int consumer_shortcutid;
    public string filename;
    public string file_size;
    public string preview_file_size;
    public string file_url;
    public string preview_url;
    public string url;
    public string hcontent_file;
    public string hcontent_preview;
    public string title;
    public string file_description;
    public int time_created;
    public int time_updated;
    public int visibility;
    public int flags;
    public bool workshop_file;
    public bool workshop_accepted;
    public bool show_subscribe_all;
    public int num_comments_public;
    public bool banned;
    public string ban_reason;
    public string banner;
    public bool can_be_deleted;
    public string app_name;
    public int file_type;
    public bool can_subscribe;
    public int subscriptions;
    public int favorited;
    public int followers;
    public int lifetime_subscriptions;
    public int lifetime_favorited;
    public int lifetime_followers;
    public string lifetime_playtime;
    public string lifetime_playtime_sessions;
    public int views;
    public int num_children;
    public int num_reports;
    public Preview[] previews;
    public Tag[] tags;
    public int language;
    public bool maybe_inappropriate_sex;
    public bool maybe_inappropriate_violence;
    public class Tag
    {
        public string tag;
        public bool adminonly;
    }

}

public class Tag
{
    public string tag;
    public bool adminonly;
}

public class PublishedFileQueryResponse
{
    public FileResponse response { get; set; }
}

public class FileResponse
{
    public int result { get; set; }
    public int resultcount { get; set; }
    public PublishedFileQueryDetail[] publishedfiledetails { get; set; }
}

public class PublishedFileQueryDetail
{
    public string publishedfileid { get; set; }
    public int result { get; set; }
    public string creator { get; set; }
    public int creator_app_id { get; set; }
    public int consumer_app_id { get; set; }
    public string filename { get; set; }
    public int file_size { get; set; }
    public string preview_url { get; set; }
    public string hcontent_preview { get; set; }
    public string title { get; set; }
    public string description { get; set; }
    public int time_created { get; set; }
    public int time_updated { get; set; }
    public int visibility { get; set; }
    public int banned { get; set; }
    public string ban_reason { get; set; }
    public int subscriptions { get; set; }
    public int favorited { get; set; }
    public int lifetime_subscriptions { get; set; }
    public int lifetime_favorited { get; set; }
    public int views { get; set; }
    public Tag[] tags { get; set; }
    public class Tag
    {
        public string tag { get; set; }
    }

}

public class Tag
{
    public string tag { get; set; }
}

public class Preview
{
    public string previewid;
    public int sortorder;
    public string url;
    public int size;
    public string filename;
    public int preview_type;
    public string youtubevideoid;
    public string external_reference;
}


```

---

## PlayersOnline

```csharp
using System.Collections.Generic;
using System;
using System.Reflection;
using System.Data;
using System.Text;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("PlayersOnline", "Steven", "1.0.0", ResourceId = 8907)]
 class PlayersOnline : RustPlugin
{
     int PlayersToList;
    [ChatCommand("who")]
     void cmdWho(BasePlayer player, string command, string[] args);
    [ChatCommand("players")]
     void cmdPlayers(BasePlayer player, string command, string[] args);
}


```

---

## PlayerStats

```csharp
using UnityEngine;
using System;
using System.Globalization;
using System.Linq;
using Oxide.Game.Rust.Cui;
using System.Collections;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using Newtonsoft.Json.Linq;

Oxide.Plugins
[Info("Player Stats", "HzHzHzHz", "0.1.4")]
 class PlayerStats : RustPlugin
{
     bool init;
     bool autoGrantOnWipe;
     string cmdConfigStat;
     List<string> Kills;
     List<string> Gathers;
     List<string> Raiders;
     List<string> Onlines;
     List<string> Topformonths;
    private void LoadConfigValues();
    private bool GetConfig(string MainMenu, string Key, T var);
     float kills;
     float deaths;
     float resGather;
     float Raider;
     float level;
     int topCount;
     int timeInHours;
    readonly DynamicConfigFile dataFile;
    readonly DynamicConfigFile topFile;
    readonly DynamicConfigFile timingFile;
    readonly DynamicConfigFile lastWipeTopUsersDataFile;
     int time;
     Dictionary<ulong, PlayerData> data;
     List<SkillTopUser> skillTopUsers;
     class PlayerData
    {
        public string name;
        public string avatar;
        public Gather gather;
        public Pvp pvp;
        public Raid raid;
        public int minutes;
        public PlayerData();
    }

     class Gather
    {
        public int wood;
        public int stone;
        public int metalOre;
        public int sulfurOre;
        public int hqmetalOre;
        public Gather();
    }

     class Pvp
    {
        public int kills;
        public int deaths;
        public Pvp();
    }

     class Raid
    {
        public int Explosive;
        public int Beancan;
        public int F1;
        public int Satchel;
        public int TimedExpl;
        public int RocketLaunched;
        public int Rocket;
        public int Incendiary;
        public int High;
        public Raid();
    }

     class SkillTopUser
    {
        public topSkill skillType;
        public ulong userId;
        public int wipeCount;
        public bool giftTaken;
        public SkillTopUser(topSkill skillType, ulong userId, int wipeCount);
    }

     void CreatePlayerData(ulong id);
    private bool IsNPC(BasePlayer player);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     void ProcessItem(BasePlayer player, Item item);
     void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
     void OnRocketLaunched(BasePlayer player, BaseEntity entity);
     void OnEntityDeath(BaseCombatEntity victim, HitInfo info);
     void SaveData();
     void OnServerSave();
     void OnServerInitialized();
     IEnumerator LoadImages();
     Dictionary<string, string> Images;
     void OnNewSave();
     bool enabled;
     void GenerateLastWipeTopPlayers();
     int GetWipeCount(topSkill skill, ulong userId);
     void Loaded();
     void timingHandle();
     void playtimeHandle();
     void Unload();
     void OnPlayerInit(BasePlayer player);
    [ChatCommand("topgift")]
     void cmdChat_topGift(BasePlayer player, string cmd, string[] args);
     void showGui(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("stat")]
     void drawstatConsole(ConsoleSystem.Arg arg);
    [ConsoleCommand("grantplayers")]
     void grantstatConsole(ConsoleSystem.Arg arg);
    [ConsoleCommand("closestat2")]
     void destroyStat2(ConsoleSystem.Arg arg);
    [ConsoleCommand("drawTop")]
     void drawtopConsole(ConsoleSystem.Arg arg);
    [ConsoleCommand("closestat")]
     void destroyStat(ConsoleSystem.Arg arg);
    [PluginReference]
     Plugin EventManager;
    [PluginReference]
     Plugin Duel;
     bool InEvent(BasePlayer player);
     bool InDuel(BasePlayer player);
     Dictionary<ulong, string> startGui;
     void GetAvatar(ulong uid, Action<string> callback);
     string GUI;
     string NextGUI;
     void CreateUINext(BasePlayer player);
     void CreateUI(BasePlayer player);
    public static string FormatTime(TimeSpan time);
    private static string Format(int units, string form1, string form2, string form3);
     void drawWindow(BasePlayer player);
     topList calcSum();
     class topList
    {
        public List<string> list;
        public string arr;
    }

     void writeTop();
     BasePlayer findPlayer(string name);
     void StartSleeping(BasePlayer player);
    private GameObject FileManagerObject;
    private FileManager m_FileManager;
     void InitFileManager();
     class FileManager : MonoBehaviour
    {
         int loaded;
         int needed;
        public bool IsFinished { get; set; }
        const ulong MaxActiveLoads;
         Dictionary<string, FileInfo> files;
        private class FileInfo
        {
            public string Url;
            public string Png;
        }

        public string GetPng(string name);
        public IEnumerator LoadFile(string name, string url, int size);
         IEnumerator LoadImageCoroutine(string name, string url, int size);
    }

}

 class PlayerData
{
    public string name;
    public string avatar;
    public Gather gather;
    public Pvp pvp;
    public Raid raid;
    public int minutes;
    public PlayerData();
}

 class Gather
{
    public int wood;
    public int stone;
    public int metalOre;
    public int sulfurOre;
    public int hqmetalOre;
    public Gather();
}

 class Pvp
{
    public int kills;
    public int deaths;
    public Pvp();
}

 class Raid
{
    public int Explosive;
    public int Beancan;
    public int F1;
    public int Satchel;
    public int TimedExpl;
    public int RocketLaunched;
    public int Rocket;
    public int Incendiary;
    public int High;
    public Raid();
}

 class SkillTopUser
{
    public topSkill skillType;
    public ulong userId;
    public int wipeCount;
    public bool giftTaken;
    public SkillTopUser(topSkill skillType, ulong userId, int wipeCount);
}

 class topList
{
    public List<string> list;
    public string arr;
}

 class FileManager : MonoBehaviour
{
     int loaded;
     int needed;
    public bool IsFinished { get; set; }
    const ulong MaxActiveLoads;
     Dictionary<string, FileInfo> files;
    private class FileInfo
    {
        public string Url;
        public string Png;
    }

    public string GetPng(string name);
    public IEnumerator LoadFile(string name, string url, int size);
     IEnumerator LoadImageCoroutine(string name, string url, int size);
}

private class FileInfo
{
    public string Url;
    public string Png;
}


```

---

## PlayerTopStats

```csharp
using UnityEngine;
using System;
using System.IO;
using System.Globalization;
using System.Linq;
using Oxide.Game.Rust.Cui;
using System.Text;
using System.Collections;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Player Stats", "Anonymuspro", "1.0.0")]
 class PlayerTopStats : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
    [PluginReference]
     Plugin UniversalShop;
     float kills;
     float deaths;
     float resGather;
     float resQuarry;
     float level;
     int topCount;
     int timeInHours;
    public class RewardsConfig
    {
        [JsonProperty("Награда за ресурсы")]
        public int OreReward;
        [JsonProperty("Награда за убийство")]
        public int KillReward;
        [JsonProperty("Награда за сломанную бочку")]
        public int BarrelReward;
        [JsonProperty("Награ за убийство животного")]
        public int AnimalReward;
    }

     RewardsConfig cfg;
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    readonly DynamicConfigFile dataFile;
    readonly DynamicConfigFile topFile;
    readonly DynamicConfigFile timingFile;
     int time;
     Dictionary<ulong, PlayerData> data;
     class PlayerData
    {
        public string name;
        public Gather gather;
        public Barrel barrel;
        public Pvp pvp;
        public int minutes;
        public PlayerData();
    }

     class Gather
    {
        public int wood;
        public int stone;
        public int metalOre;
        public int sulfurOre;
        public Gather();
    }

     class Barrel
    {
        public int Count;
    }

     class Pvp
    {
        public int AnimalKills;
        public int kills;
        public int deaths;
        public Pvp();
    }

     void CreatePlayerData(ulong id);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnEntityDeath(BaseCombatEntity victim, HitInfo info);
     void SaveData();
     void OnPluginLoaded(Plugin name);
     void OnServerSave();
     void Loaded();
     void timingHandle();
     void playtimeHandle();
     void Unload();
    [ChatCommand("stat")]
     void showGui(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("stat")]
     void drawstatConsole(ConsoleSystem.Arg arg);
    public string str1;
    public string str2;
     void OnServerInitialized();
    [ConsoleCommand("drawTop")]
     void drawtopConsole(ConsoleSystem.Arg arg);
    [PluginReference]
     Plugin EventManager;
    [PluginReference]
     Plugin Duels;
     bool InEvent(BasePlayer player);
     bool InDuel(BasePlayer player);
     Dictionary<ulong, string> startGui;
     void drawWindow(BasePlayer player);
     string prettyTime(int time);
     topList calcSum();
     class topList
    {
        public List<string> list;
        public string arr;
    }

     void drawTopMenu(BasePlayer player);
     void writeTop();
     void debugtestcons(ConsoleSystem.Arg arg);
     void debugtestcons2(ConsoleSystem.Arg arg);
     BasePlayer findPlayer(string name);
     void StartSleeping(BasePlayer player);
    public class cui
    {
        public static CuiElementContainer elements;
        public static CuiElement createparentcurs(string name, string color, string anchmin, string anchmax);
        public static CuiElement createparent(string name, string color, string anchmin, string anchmax);
        public static CuiElement createbutton(string name, string parent, string command, string close, string color, string anchmin, string anchmax);
        public static CuiElement createbox(string name, string parent, string color, string anchmin, string anchmax);
        public static CuiElement createtext(string name, string parent, string text, int size, string anchmin, string anchmax, TextAnchor anch);
        public static CuiElement createimg(string name, string parent, string img, string anchmin, string anchmax);
    }

}

public class RewardsConfig
{
    [JsonProperty("Награда за ресурсы")]
    public int OreReward;
    [JsonProperty("Награда за убийство")]
    public int KillReward;
    [JsonProperty("Награда за сломанную бочку")]
    public int BarrelReward;
    [JsonProperty("Награ за убийство животного")]
    public int AnimalReward;
}

 class PlayerData
{
    public string name;
    public Gather gather;
    public Barrel barrel;
    public Pvp pvp;
    public int minutes;
    public PlayerData();
}

 class Gather
{
    public int wood;
    public int stone;
    public int metalOre;
    public int sulfurOre;
    public Gather();
}

 class Barrel
{
    public int Count;
}

 class Pvp
{
    public int AnimalKills;
    public int kills;
    public int deaths;
    public Pvp();
}

 class topList
{
    public List<string> list;
    public string arr;
}

public class cui
{
    public static CuiElementContainer elements;
    public static CuiElement createparentcurs(string name, string color, string anchmin, string anchmax);
    public static CuiElement createparent(string name, string color, string anchmin, string anchmax);
    public static CuiElement createbutton(string name, string parent, string command, string close, string color, string anchmin, string anchmax);
    public static CuiElement createbox(string name, string parent, string color, string anchmin, string anchmax);
    public static CuiElement createtext(string name, string parent, string text, int size, string anchmin, string anchmax, TextAnchor anch);
    public static CuiElement createimg(string name, string parent, string img, string anchmin, string anchmax);
}


```

---

## PlayerWear

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Color = UnityEngine.Color;
using Newtonsoft.Json;
using System.Linq;

Oxide.Plugins
[Info("PlayerWear", "Drop Dead", "1.0.0")]
public class PlayerWear : RustPlugin
{
    public Dictionary<ulong, string> command;
    private void SaveData();
    private void LoadData();
    public class items
    {
        [JsonProperty("Шортнейм")]
        public string shortname;
        [JsonProperty("Количество")]
        public int amount;
        [JsonProperty("Скин")]
        public ulong skinid;
        [JsonProperty("Контейнер (wear/belt/main)")]
        public string container;
        [JsonProperty("Заблокировать перемещение?")]
        public bool move;
        [JsonProperty("Заблокировать дроп?")]
        public bool drop;
    }

    private PluginConfig cfg;
    public class PluginConfig
    {
        [JsonProperty("Предметы по группам")]
        public Dictionary<string, List<items>> perms;
    }

    private void Init();
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     void Unload();
     void OnServerSave();
     void OnPlayerConnected(BasePlayer player);
     void CheckPlayer(BasePlayer player);
     void OnPlayerRespawned(BasePlayer player);
     object CanMoveItem(Item item, PlayerInventory playerLoot, uint targetContainer, int targetSlot, int amount);
     object OnItemAction(Item item, string action, BasePlayer player);
    private object CanUseLockedEntity(BasePlayer player, BaseLock @lock);
    [ChatCommand("events")]
     void EventCmd(BasePlayer player, string commands, string[] args);
}

public class items
{
    [JsonProperty("Шортнейм")]
    public string shortname;
    [JsonProperty("Количество")]
    public int amount;
    [JsonProperty("Скин")]
    public ulong skinid;
    [JsonProperty("Контейнер (wear/belt/main)")]
    public string container;
    [JsonProperty("Заблокировать перемещение?")]
    public bool move;
    [JsonProperty("Заблокировать дроп?")]
    public bool drop;
}

public class PluginConfig
{
    [JsonProperty("Предметы по группам")]
    public Dictionary<string, List<items>> perms;
}


```

---

## PlayingCards

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using System;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("PlayingCards", "k1lly0u", "0.1.1")]
[Description("Casino image management system")]
 class PlayingCards : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
    private string imageUrl;
    public static bool IsReady { get; set; }
    public static PlayingCards Instance { get; set; }
    private void OnServerInitialized();
    private void Unload();
    private void ImportCardImages(int attempts);
    private void OnImagesLoaded();
    public static void AddImage(string imageName, string fileName);
    public static string GetCardImage(string value, string suit);
    public static string GetChipImage(int value);
    public static string GetChipStackImage();
    public static string GetBoardImage(string gameType);
    public static string GetCardBackground(string color);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Force image update on load")]
        public bool ForceUpdate { get; set; }
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
    [JsonProperty(PropertyName = "Force image update on load")]
    public bool ForceUpdate { get; set; }
    public Oxide.Core.VersionNumber Version { get; set; }
}


```

---

## PlayTime

```csharp
using System.Collections.Generic;
using System;
using System.Linq;
using Oxide.Core;

Oxide.Plugins
[Info("PlayTime", "Waizujin", 2.0)]
[Description("Logs players play time and allows you to view the players play time with a command.")]
public class PlayTime : RustPlugin
{
    public string Prefix;
    public int SaveInterval { get; set; }
    public bool BroadcastLastSeenOnConnect { get; set; }
    protected override void LoadDefaultConfig();
     class PlayTimeData
    {
        public Dictionary<string, PlayTimeInfo> Players;
        public PlayTimeData();
    }

     class PlayTimeInfo
    {
        public string SteamID;
        public string Name;
        public long LastPlayTimeIncrement;
        public long LastLogoutTime;
        public long PlayTime;
        public PlayTimeInfo();
        public PlayTimeInfo(BasePlayer player);
    }

     PlayTimeData playTimeData;
    private void OnServerInitialized();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
    public void updatePlayTime();
    [ChatCommand("playtime")]
    private void PlayTimeCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("lastseen")]
    private void LastSeenCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("mostonline")]
    private void MostOnlineCommand(BasePlayer player, string command, string[] args);
    private long FindPlayer(string queriedPlayer);
    private static long GrabCurrentTimestamp();
     bool hasPermission(BasePlayer player, string perm);
}

 class PlayTimeData
{
    public Dictionary<string, PlayTimeInfo> Players;
    public PlayTimeData();
}

 class PlayTimeInfo
{
    public string SteamID;
    public string Name;
    public long LastPlayTimeIncrement;
    public long LastLogoutTime;
    public long PlayTime;
    public PlayTimeInfo();
    public PlayTimeInfo(BasePlayer player);
}


```

---

## PlayTimeTracker

```csharp
using System;
using Oxide.Core;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Playtime and AFK Tracker", "ArcaneCraeda", 1.4)]
[Description("Logs every players' play and afk time, separately.")]
public class PlayTimeTracker : RustPlugin
{
    protected override void LoadDefaultConfig();
     class PlayTimeData
    {
        public Dictionary<string, PlayTimeInfo> Players;
        public PlayTimeData();
    }

     class PlayTimeInfo
    {
        public string SteamID;
        public string Name;
        public long PlayTime;
        public long AfkTime;
        public string HumanPlayTime;
        public string HumanAfkTime;
        public string LastSeen;
        public PlayTimeInfo();
        public PlayTimeInfo(BasePlayer player);
    }

     class PlayerStateData
    {
        public Dictionary<string, PlayerStateInfo> Players;
        public PlayerStateData();
    }

     class PlayerStateInfo
    {
        public string SteamID;
        public long InitTimeStamp;
        public int AfkCount;
        public long AfkTime;
        public double[] Position;
        public string LiveName;
        public PlayerStateInfo();
        public PlayerStateInfo(BasePlayer player);
    }

     PlayTimeData playTimeData;
     PlayerStateData playerStateData;
    public string prefix;
     int afkCheckInterval { get; set; }
     int cyclesUntilAfk { get; set; }
     bool afkCounts { get; set; }
     void Init();
     void LoadPermissions();
     void OnPluginLoaded();
     void OnPluginUnloaded();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
    [ChatCommand("playtime")]
     void cmdPlayTime(BasePlayer player, string cmd, string[] args);
    [ChatCommand("afktime")]
     void cmdAfkTime(BasePlayer player, string cmd, string[] args);
    [ChatCommand("lastseen")]
     void cmdLastSeen(BasePlayer player, string command, string[] args);
    private void afkCheck();
    private void initPlayerState(BasePlayer player);
    private void savePlayerState(BasePlayer player);
    private static long GrabCurrentTimestamp();
    private bool hasPermission(BasePlayer player, string _permission);
    private string FindPlayer(string name);
}

 class PlayTimeData
{
    public Dictionary<string, PlayTimeInfo> Players;
    public PlayTimeData();
}

 class PlayTimeInfo
{
    public string SteamID;
    public string Name;
    public long PlayTime;
    public long AfkTime;
    public string HumanPlayTime;
    public string HumanAfkTime;
    public string LastSeen;
    public PlayTimeInfo();
    public PlayTimeInfo(BasePlayer player);
}

 class PlayerStateData
{
    public Dictionary<string, PlayerStateInfo> Players;
    public PlayerStateData();
}

 class PlayerStateInfo
{
    public string SteamID;
    public long InitTimeStamp;
    public int AfkCount;
    public long AfkTime;
    public double[] Position;
    public string LiveName;
    public PlayerStateInfo();
    public PlayerStateInfo(BasePlayer player);
}


```

---

## PluginCleaner

```csharp
using System;
using System.IO;
using System.Linq;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("PluginCleaner", "Vinni P.", "1.5.8")]
[Description("Removes config, lang, and data files for plugins that no longer exist.")]
public class PluginCleaner : RustPlugin
{
    private PluginCleanerConfig config;
    private class PluginCleanerConfig
    {
        public bool AutoRemoveFiles { get; set; }
        public Dictionary<string, List<string>> CustomDataMappings { get; set; }
        public Dictionary<string, List<string>> IgnoredFiles { get; set; }
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    [ConsoleCommand("cleanplugins")]
    private void CleanPluginsCommand(ConsoleSystem.Arg arg);
}

private class PluginCleanerConfig
{
    public bool AutoRemoveFiles { get; set; }
    public Dictionary<string, List<string>> CustomDataMappings { get; set; }
    public Dictionary<string, List<string>> IgnoredFiles { get; set; }
}


```

---

## PM

```csharp
using System.Collections.Generic;
using System;
using System.Reflection;
using System.Data;
using System.Text;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("PM", "Steven", "1.0.2", ResourceId = 8906)]
 class PM : RustPlugin
{
     bool FindByHoleName;
     Dictionary<ulong, ulong> PmHistory;
    [HookMethod("OnPlayerDisconnected")]
     void OnPlayerDisconnected(BasePlayer player);
    [ChatCommand("pm")]
     void cmdPm(BasePlayer player, string command, string[] args);
    [ChatCommand("r")]
     void cmdPmReply(BasePlayer player, string command, string[] args);
}


```

---

## PoliticalSurvival

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using MySql.Data;
using MySql.Data.MySqlClient;
using MySql.Data.Common;
using UnityEngine;

Oxide.Plugins
[Info("PoliticalSurvival", "Jonty", 0.2)]
[Description("Political Survival - Become the President, tax your subjects and keep them in line!")]
 class PoliticalSurvival : RustPlugin
{
    static void Main(string[] args);
    public class StrayPlayer
    {
        public ulong SteamId;
        public bool IsSettingTaxChest;
        public StrayPlayer(ulong pSteamId);
    }

     Dictionary<BasePlayer, StrayPlayer> OnlinePlayers;
     Dictionary<string, string> ServerMessages;
     MySqlConnection Database;
     string DatabaseHost;
     string DatabasePort;
     string DatabaseUsername;
     string DatabasePassword;
     string DatabaseName;
     ulong President;
     double TaxLevel;
     string RealmName;
     float TaxChestX;
     float TaxChestY;
     float TaxChestZ;
     StorageContainer TaxContainer;
    private void LoadDefaultConfig();
     void Init();
     void OnPlayerInit(BasePlayer Player);
     void OnPlayerDisconnected(BasePlayer Player, string Reason);
     void OnDispenserGather(ResourceDispenser Dispenser, BaseEntity Entity, Item Item);
     void OnPlantGather(PlantEntity Plant, Item Item, BasePlayer Player);
     void OnEntityDeath(BaseCombatEntity Entity, HitInfo Info);
     void OnPlayerAttack(BasePlayer Attacker, HitInfo Info);
    [ChatCommand("settaxchest")]
     void SetTaxChestCommand(BasePlayer Player, string Command, string[] Arguments);
    [ChatCommand("info")]
     void InfoCommand(BasePlayer Player, string Command, string[] Arguments);
    [ChatCommand("claimpresident")]
     void ClaimPresident(BasePlayer Player, string Command, string[] Arguments);
    [ChatCommand("settax")]
     void SetTaxCommand(BasePlayer Player, string Command, string[] Arguments);
    [ChatCommand("realmname")]
     void RealmNameCommand(BasePlayer Player, string Command, string[] Arguments);
    [ChatCommand("pm")]
     void PrivateMessage(BasePlayer Player, string Command, string[] Arguments);
    [ChatCommand("players")]
     void PlayersCommand(BasePlayer Player, string Command, string[] Arguments);
     void AddPlayer(BasePlayer Player);
     void RemovePlayer(BasePlayer Player);
     StrayPlayer GetStrayPlayer(string Username);
     bool IsPlayerOnline(string Username);
     BasePlayer GetPlayer(string Username);
     string MergeParams(string[] Params, int Start);
     bool IsPresident(ulong SteamId);
     void SetPresident(ulong SteamId);
     void SetTaxLevel(double NewTaxLevel);
     void SetRealmName(string NewName);
     void GetPlayerFromDatabase(BasePlayer Player);
     void LoadTaxContainer();
     void SaveTaxContainer();
    private void CreateConfigEntry(string Key, string SubKey, string Value);
    private void LoadServerMessages();
}

public class StrayPlayer
{
    public ulong SteamId;
    public bool IsSettingTaxChest;
    public StrayPlayer(ulong pSteamId);
}


```

---

## PopupNotifications

```csharp
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Popup Notifications", "emu / k1lly0u", "0.1.0", ResourceId = 1252)]
public class PopupNotifications : RustPlugin
{
    private static Vector2 position;
    private static Vector2 dimensions;
    private static ConfigData config;
     void OnServerInitialized();
    private ConfigData configData;
     class ConfigData
    {
        public float ShowDuration { get; set; }
        public int MaxShownMessages { get; set; }
        public bool ScrollDown { get; set; }
        public float PositionX { get; set; }
        public float PositionY { get; set; }
        public float Width { get; set; }
        public float Height { get; set; }
        public float Spacing { get; set; }
        public float Transparency { get; set; }
        public float FadeTime { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
    [ChatCommand("popupmsg")]
    private void SendPopupMessage(BasePlayer player, string command, string[] args);
    [ConsoleCommand("popupmsg.global")]
    private void ConPopupMessageGlobal(ConsoleSystem.Arg arg);
    [ConsoleCommand("popupmsg.toplayer")]
    private void ConPopupMessageToPlayer(ConsoleSystem.Arg arg);
    private object GetPlayerByName(string name);
    [ConsoleCommand("popupmsg.close")]
    private void CloseCommand(ConsoleSystem.Arg arg);
    [HookMethod("CreatePopupNotification")]
     void CreatePopupNotification(string message, BasePlayer player, float duration);
    private void CreatePopupOnPlayer(string message, BasePlayer player, float duration);
    private void CreateGlobalPopup(string message, float duration);
     class Notifier : MonoBehaviour
    {
        private List<string> msgQueue;
        private Dictionary<int, string> usedSlots;
        private BasePlayer player;
         void Awake();
        public static Notifier GetPlayerNotifier(BasePlayer player);
        private int GetEmptySlot();
        public void CreateNotification(string message, float showDuration);
        public void DestroyNotification(int slot);
        private IEnumerator DelayedRemoveFromSlot(int slot);
        private IEnumerator DestroyAfterDuration(int slot, float duration);
    }

    static string PUMSG;
    static CuiElementContainer CreatePopupMessage(string panelName, string color, string text, int slot, string aMin, string aMax);
     string msg(string key, string id);
     Dictionary<string, string> Messages;
}

 class ConfigData
{
    public float ShowDuration { get; set; }
    public int MaxShownMessages { get; set; }
    public bool ScrollDown { get; set; }
    public float PositionX { get; set; }
    public float PositionY { get; set; }
    public float Width { get; set; }
    public float Height { get; set; }
    public float Spacing { get; set; }
    public float Transparency { get; set; }
    public float FadeTime { get; set; }
}

 class Notifier : MonoBehaviour
{
    private List<string> msgQueue;
    private Dictionary<int, string> usedSlots;
    private BasePlayer player;
     void Awake();
    public static Notifier GetPlayerNotifier(BasePlayer player);
    private int GetEmptySlot();
    public void CreateNotification(string message, float showDuration);
    public void DestroyNotification(int slot);
    private IEnumerator DelayedRemoveFromSlot(int slot);
    private IEnumerator DestroyAfterDuration(int slot, float duration);
}


```

---

## PortableVehicles

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("Portable Vehicles", "Paulsimik", "1.1.2")]
[Description("Give vehicles as item to your players")]
public class PortableVehicles : RustPlugin
{
    private static Configuration config;
    private const string permUse;
    private const string permAdmin;
    private const string permPickup;
    private string[] chatCommands;
    private void Init();
    private void OnEntityBuilt(Planner plan, GameObject go);
    private object OnHammerHit(BasePlayer player, HitInfo info);
    private void CheckPlacement(Planner plan, GameObject go);
    private bool CheckPickup(BasePlayer player, BaseVehicle entity);
    private bool CheckPickupBalloon(BasePlayer player, HotAirBalloon balloon);
    private void GiveItem(BasePlayer player, ulong skin);
    private void GiveItem(BasePlayer player, ulong skinID, string name, bool isWaterVehicle);
    private ulong GetSkin(string name);
    private void cmdPortableVehicles(BasePlayer player, string command, string[] args);
    [ConsoleCommand("portablevehicles.give")]
    private void cmdGiveConsole(ConsoleSystem.Arg arg);
    private class Configuration
    {
        [JsonProperty(PropertyName = "Chat Icon")]
        public uint chatIcon;
        [JsonProperty("Hits count to pickup vehicle")]
        public int hitsToPickup;
        [JsonProperty("Automatically mount players")]
        public bool autoMount;
        [JsonProperty(PropertyName = "Item shortname for water entity")]
        public string waterEntityShortName;
        [JsonProperty(PropertyName = "Item shortname for ground entity")]
        public string groundEntityShortName;
        [JsonProperty(PropertyName = "Blacklist pickupable vehicles shortname")]
        public List<string> pickupableBlacklist;
        public VersionNumber version;
    }

    private class VehicleEntry
    {
        public ulong skinId;
        public string displayName;
        public string prefab;
        public bool bigModel;
        public bool isWaterVehicle;
    }

    private VehicleEntry[] AllVehicles;
    private class PickupScript : MonoBehaviour
    {
        private BaseEntity entity;
        private int hits;
        private void Awake();
        public void AddHit();
        private void ResetHits();
        public int GetHitsLeft();
    }

    private Configuration GetDefaultConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
    private string GetLang(string key, string playerID, object[] args);
    protected override void LoadDefaultMessages();
    private bool IsAdmin(BasePlayer player);
    private BasePlayer FindPlayer(ConsoleSystem.Arg arg, string nameOrID);
    private BasePlayer FindPlayer(BasePlayer player, string nameOrID);
    private void Message(ConsoleSystem.Arg arg, string messageKey, object[] args);
    private void Message(BasePlayer player, string messageKey, object[] args);
    private void SendMessage(BasePlayer player, string msg);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Chat Icon")]
    public uint chatIcon;
    [JsonProperty("Hits count to pickup vehicle")]
    public int hitsToPickup;
    [JsonProperty("Automatically mount players")]
    public bool autoMount;
    [JsonProperty(PropertyName = "Item shortname for water entity")]
    public string waterEntityShortName;
    [JsonProperty(PropertyName = "Item shortname for ground entity")]
    public string groundEntityShortName;
    [JsonProperty(PropertyName = "Blacklist pickupable vehicles shortname")]
    public List<string> pickupableBlacklist;
    public VersionNumber version;
}

private class VehicleEntry
{
    public ulong skinId;
    public string displayName;
    public string prefab;
    public bool bigModel;
    public bool isWaterVehicle;
}

private class PickupScript : MonoBehaviour
{
    private BaseEntity entity;
    private int hits;
    private void Awake();
    public void AddHit();
    private void ResetHits();
    public int GetHitsLeft();
}


```

---

## Portgun

```csharp
using System.Collections.Generic;
using System;
using System.Reflection;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Portgun", "Reneb", "2.0.1")]
 class Portgun : RustPlugin
{
    private FieldInfo serverinput;
    private int collLayers;
     void Loaded();
     void OnServerInitialized();
    private static string permissionPortgun;
    private static int authLevel;
    protected override void LoadDefaultConfig();
    private void CheckCfg(string Key, T var);
     void Init();
     bool hasAccess(BasePlayer player);
    [ChatCommand("p")]
     void cmdChatPortgun(BasePlayer player, string command, string[] args);
}


```

---

## PrefabSniffer

```csharp
using System;
using System.Collections.Generic;
using System.Reflection;
using UnityEngine;
using Facepunch;

Oxide.Plugins
[Info("PrefabSniffer", "Ayrin", "1.1.1", ResourceId = 1938)]
 class PrefabSniffer : RustPlugin
{
    private static List<string> resourcesList;
    private string argmsg;
    [ConsoleCommand("prefabs")]
     void cmdSniffPrefabs(ConsoleSystem.Arg arg);
}


```

---

## PreferredEnvironment

```csharp
using Facepunch;
using Network;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Globalization;
using UnityEngine;

Oxide.Plugins
[Info("PreferredEnvironment", "Sempai#3239", "2.0.12")]
[Description("Allows players to customize their environment settings, or create presets that apply to specified zones")]
public class PreferredEnvironment : RustPlugin
{
    private const string PERMISSION_USE;
    private const string PERMISSION_ADMIN;
    private const string WEATHER_VAR_FILTER;
    private Dictionary<ulong, EnvironmentInfo> userEnvironmentInfo;
    private DynamicConfigFile userData;
    private static Hash<string, ConsoleSystem.Command> weatherConvars;
    private bool initialized;
    private void Init();
    private void OnServerInitialized();
    private void OnServerSave();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnUserPermissionRevoked(string id, string permission);
    private object CanNetworkTo(EnvSync env, BasePlayer player);
    private void OnServerCommand(string cmd, string[] args);
    private void Unload();
    private void LoadData();
    private void LoadServerVars();
    private void SetWeatherVars();
    private void DelayedEnvironmentUpdate(BasePlayer player);
    private void OnReplicatedVarChanged(string command, string value);
    private void SendReplicatedVarsAll();
    private IEnumerator SendReplicatedVarsAllEnumerator();
    public void SendServerReplicatedVars(BasePlayer player);
    [ConsoleCommand("setenv")]
    private void ccmdSetEnvironment(ConsoleSystem.Arg arg);
    private BasePlayer FindPlayer(string partialNameOrID);
    private string _msg(string key, string id);
    private void OnEnterZone(string zoneID, BasePlayer player);
    private void OnExitZone(string zoneID, BasePlayer player);
    public static class UI
    {
        public static CuiElementContainer Container(string panelName, string color, UI4 dimensions, bool useCursor, string parent);
        public static CuiElementContainer BlurContainer(string panelName, UI4 dimensions, string color, string parent);
        public static void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions, bool cursor);
        public static void BlurPanel(CuiElementContainer container, string panel, UI4 dimensions, string color);
        public static void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align, FontStyle fontStyle);
        public static void Button(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, string command, TextAnchor align, FontStyle fontStyle);
        internal static void Toggle(CuiElementContainer container, string panel, string color, int fontSize, UI4 dimensions, string command, bool isOn);
        public static void Image(CuiElementContainer container, string panel, string png, UI4 dimensions);
        public static void Input(CuiElementContainer container, string panel, string text, int size, string command, UI4 dimensions);
        public static string Color(string hexColor, float alpha);
        private static string ToFontString(FontStyle fontStyle);
    }

    public class UI4
    {
        [JsonProperty(PropertyName = "Left (0.0 - 1.0)")]
        public float xMin;
        [JsonProperty(PropertyName = "Bottom (0.0 - 1.0)")]
        public float yMin;
        [JsonProperty(PropertyName = "Right (0.0 - 1.0)")]
        public float xMax;
        [JsonProperty(PropertyName = "Top (0.0 - 1.0)")]
        public float yMax;
        public UI4(float xMin, float yMin, float xMax, float yMax);
        public string GetMin();
        public string GetMax();
        public static UI4 zero;
    }

    private const string UI_MENU;
    private const string UI_DUMMY;
    private void OpenDummyContainer(BasePlayer player);
    private void OpenEnvironmentEditor(BasePlayer player, EnvironmentInfo environmentInfo);
    private void OpenServerEnvironmentEditor(BasePlayer player);
    private void AddMenuOption(CuiElementContainer container, string title, string variable, float currentValue, int position, float size, string playerId, bool isServer);
    private void AddTimeOption(CuiElementContainer container, float currentValue, int position, float size, string playerId, bool isServer);
    private void AddMenuToggle(CuiElementContainer container, string title, string variable, bool currentValue, int position, float size, string playerId, bool isServer);
    [ConsoleCommand("pe.closeui")]
    private void ccmdCloseUI(ConsoleSystem.Arg arg);
    [ConsoleCommand("pe.reset")]
    private void ccmdReset(ConsoleSystem.Arg arg);
    [ConsoleCommand("pe.servreset")]
    private void ccmdServerReset(ConsoleSystem.Arg arg);
    [ConsoleCommand("pe.editvalue")]
    private void ccmdEditValue(ConsoleSystem.Arg arg);
    [ConsoleCommand("pe.server.editvalue")]
    private void ccmdEditServerValue(ConsoleSystem.Arg arg);
    [ChatCommand("env")]
    private void cmdEnv(BasePlayer player, string command, string[] args);
    [ChatCommand("senv")]
    private void cmdServerEnv(BasePlayer player, string command, string[] args);
    [ChatCommand("mytime")]
    private void cmdMyTime(BasePlayer player, string command, string[] args);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty("Save players custom environment settings and apply after restart/relog")]
        public bool EnableSaving { get; set; }
        [JsonProperty("Custom permission to change time")]
        public string TimePermission { get; set; }
        [JsonProperty("Custom permission to change weather")]
        public string WeatherPermission { get; set; }
        [JsonProperty("Zone Environment Profiles. (To disable a variable and use the value set on the server, set the option to -1)")]
        public Dictionary<string, EnvironmentInfo> Zones { get; set; }
        [JsonProperty("Server Environment Profile")]
        public ServerEnvironmentInfo Server { get; set; }
        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private class ServerEnvironmentInfo : EnvironmentInfo
    {
        [JsonProperty("Progress Time")]
        public bool _progressTime;
        [JsonProperty("Storm Chance")]
        public float _stormChance;
        [JsonProperty("Rain Chance")]
        public float _rainChance;
        [JsonProperty("Overcast Chance")]
        public float _overcastChance;
        [JsonProperty("Fog Chance")]
        public float _fogChance;
        [JsonProperty("Dust Chance")]
        public float _dustChance;
        [JsonProperty("Clear Chance")]
        public float _clearChance;
    }

    private class EnvironmentInfo
    {
        [JsonProperty("Time (0.0 - 24.0)")]
        public float _time;
        [JsonProperty("Rain (0.0 - 1.0)")]
        public float _rain;
        [JsonProperty("Wind (0.0 - 1.0)")]
        public float _wind;
        [JsonProperty("Fog (0.0 - 1.0)")]
        public float _fog;
        [JsonProperty("Rainbow (0.0 - 1.0)")]
        public float _rainbow;
        [JsonProperty("Thunder (0.0 - 1.0)")]
        public float _thunder;
        [JsonProperty("Atmosphere Brightness (0.0 - 1.0)")]
        public float _atmosphere_brightness;
        [JsonProperty("Atmosphere Contrast (0.0 - 1.0)")]
        public float _atmosphere_contrast;
        [JsonProperty("Atmosphere Directionality (0.0 - 1.0)")]
        public float _atmosphere_directionality;
        [JsonProperty("Atmosphere Mie (0.0 - 1.0)")]
        public float _atmosphere_mie;
        [JsonProperty("Atmosphere Rayleigh (0.0 - 1.0)")]
        public float _atmosphere_rayleigh;
        [JsonProperty("Cloud Attenuation (0.0 - 1.0)")]
        public float _cloud_attenuation;
        [JsonProperty("Cloud Brightness (0.0 - 1.0)")]
        public float _cloud_brightness;
        [JsonProperty("Cloud Coloring (0.0 - 1.0)")]
        public float _cloud_coloring;
        [JsonProperty("Cloud Coverage (0.0 - 1.0)")]
        public float _cloud_coverage;
        [JsonProperty("Cloud Opacity (0.0 - 1.0)")]
        public float _cloud_opacity;
        [JsonProperty("Cloud Saturation (0.0 - 1.0)")]
        public float _cloud_saturation;
        [JsonProperty("Cloud Scattering (0.0 - 1.0)")]
        public float _cloud_scattering;
        [JsonProperty("Cloud Sharpness (0.0 - 1.0)")]
        public float _cloud_sharpness;
        [JsonProperty("Cloud Size (0.0 - 1.0)")]
        public float _cloud_size;
        [JsonIgnore]
        public float Rain { get; set; }
        [JsonIgnore]
        public float Wind { get; set; }
        [JsonIgnore]
        public float Fog { get; set; }
        [JsonIgnore]
        public float AtmosphereBrightness { get; set; }
        [JsonIgnore]
        public float AtmosphereContrast { get; set; }
        [JsonIgnore]
        public float AtmosphereDirectionality { get; set; }
        [JsonIgnore]
        public float AtmosphereMie { get; set; }
        [JsonIgnore]
        public float AtmosphereRayleigh { get; set; }
        [JsonIgnore]
        public float CloudAttenuation { get; set; }
        [JsonIgnore]
        public float CloudBrightness { get; set; }
        [JsonIgnore]
        public float CloudColoring { get; set; }
        [JsonIgnore]
        public float CloudCoverage { get; set; }
        [JsonIgnore]
        public float CloudOpacity { get; set; }
        [JsonIgnore]
        public float CloudSaturation { get; set; }
        [JsonIgnore]
        public float CloudScattering { get; set; }
        [JsonIgnore]
        public float CloudSharpness { get; set; }
        [JsonIgnore]
        public float CloudSize { get; set; }
        [JsonIgnore]
        public float Rainbow { get; set; }
        [JsonIgnore]
        public float Thunder { get; set; }
        [JsonIgnore]
        public float Time { get; set; }
        [JsonIgnore]
        private Hash<string, EnvironmentInfo> zoneOverrides;
        [JsonIgnore]
        private string currentZoneId;
        public bool HasZoneOverride();
        public bool GetZoneOverride(EnvironmentInfo environmentInfo);
        public void OnEnterZone(string zoneId, EnvironmentInfo environmentInfo);
        public void OnExitZone(string zoneId, BasePlayer player);
        [JsonIgnore]
        public bool HasReplicatedVars { get; set; }
        [JsonIgnore]
        public bool VarsDirty;
        [JsonIgnore]
        private Hash<string, string> replicatedVars;
        [JsonIgnore]
        public float GetDesiredTime { get; set; }
        public bool ShouldRemove();
        public void SendCustomReplicatedVars(BasePlayer player);
        public void BuildReplicatedConvarList();
    }

}

public static class UI
{
    public static CuiElementContainer Container(string panelName, string color, UI4 dimensions, bool useCursor, string parent);
    public static CuiElementContainer BlurContainer(string panelName, UI4 dimensions, string color, string parent);
    public static void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions, bool cursor);
    public static void BlurPanel(CuiElementContainer container, string panel, UI4 dimensions, string color);
    public static void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align, FontStyle fontStyle);
    public static void Button(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, string command, TextAnchor align, FontStyle fontStyle);
    internal static void Toggle(CuiElementContainer container, string panel, string color, int fontSize, UI4 dimensions, string command, bool isOn);
    public static void Image(CuiElementContainer container, string panel, string png, UI4 dimensions);
    public static void Input(CuiElementContainer container, string panel, string text, int size, string command, UI4 dimensions);
    public static string Color(string hexColor, float alpha);
    private static string ToFontString(FontStyle fontStyle);
}

public class UI4
{
    [JsonProperty(PropertyName = "Left (0.0 - 1.0)")]
    public float xMin;
    [JsonProperty(PropertyName = "Bottom (0.0 - 1.0)")]
    public float yMin;
    [JsonProperty(PropertyName = "Right (0.0 - 1.0)")]
    public float xMax;
    [JsonProperty(PropertyName = "Top (0.0 - 1.0)")]
    public float yMax;
    public UI4(float xMin, float yMin, float xMax, float yMax);
    public string GetMin();
    public string GetMax();
    public static UI4 zero;
}

private class ConfigData
{
    [JsonProperty("Save players custom environment settings and apply after restart/relog")]
    public bool EnableSaving { get; set; }
    [JsonProperty("Custom permission to change time")]
    public string TimePermission { get; set; }
    [JsonProperty("Custom permission to change weather")]
    public string WeatherPermission { get; set; }
    [JsonProperty("Zone Environment Profiles. (To disable a variable and use the value set on the server, set the option to -1)")]
    public Dictionary<string, EnvironmentInfo> Zones { get; set; }
    [JsonProperty("Server Environment Profile")]
    public ServerEnvironmentInfo Server { get; set; }
    public Oxide.Core.VersionNumber Version { get; set; }
}

private class ServerEnvironmentInfo : EnvironmentInfo
{
    [JsonProperty("Progress Time")]
    public bool _progressTime;
    [JsonProperty("Storm Chance")]
    public float _stormChance;
    [JsonProperty("Rain Chance")]
    public float _rainChance;
    [JsonProperty("Overcast Chance")]
    public float _overcastChance;
    [JsonProperty("Fog Chance")]
    public float _fogChance;
    [JsonProperty("Dust Chance")]
    public float _dustChance;
    [JsonProperty("Clear Chance")]
    public float _clearChance;
}

private class EnvironmentInfo
{
    [JsonProperty("Time (0.0 - 24.0)")]
    public float _time;
    [JsonProperty("Rain (0.0 - 1.0)")]
    public float _rain;
    [JsonProperty("Wind (0.0 - 1.0)")]
    public float _wind;
    [JsonProperty("Fog (0.0 - 1.0)")]
    public float _fog;
    [JsonProperty("Rainbow (0.0 - 1.0)")]
    public float _rainbow;
    [JsonProperty("Thunder (0.0 - 1.0)")]
    public float _thunder;
    [JsonProperty("Atmosphere Brightness (0.0 - 1.0)")]
    public float _atmosphere_brightness;
    [JsonProperty("Atmosphere Contrast (0.0 - 1.0)")]
    public float _atmosphere_contrast;
    [JsonProperty("Atmosphere Directionality (0.0 - 1.0)")]
    public float _atmosphere_directionality;
    [JsonProperty("Atmosphere Mie (0.0 - 1.0)")]
    public float _atmosphere_mie;
    [JsonProperty("Atmosphere Rayleigh (0.0 - 1.0)")]
    public float _atmosphere_rayleigh;
    [JsonProperty("Cloud Attenuation (0.0 - 1.0)")]
    public float _cloud_attenuation;
    [JsonProperty("Cloud Brightness (0.0 - 1.0)")]
    public float _cloud_brightness;
    [JsonProperty("Cloud Coloring (0.0 - 1.0)")]
    public float _cloud_coloring;
    [JsonProperty("Cloud Coverage (0.0 - 1.0)")]
    public float _cloud_coverage;
    [JsonProperty("Cloud Opacity (0.0 - 1.0)")]
    public float _cloud_opacity;
    [JsonProperty("Cloud Saturation (0.0 - 1.0)")]
    public float _cloud_saturation;
    [JsonProperty("Cloud Scattering (0.0 - 1.0)")]
    public float _cloud_scattering;
    [JsonProperty("Cloud Sharpness (0.0 - 1.0)")]
    public float _cloud_sharpness;
    [JsonProperty("Cloud Size (0.0 - 1.0)")]
    public float _cloud_size;
    [JsonIgnore]
    public float Rain { get; set; }
    [JsonIgnore]
    public float Wind { get; set; }
    [JsonIgnore]
    public float Fog { get; set; }
    [JsonIgnore]
    public float AtmosphereBrightness { get; set; }
    [JsonIgnore]
    public float AtmosphereContrast { get; set; }
    [JsonIgnore]
    public float AtmosphereDirectionality { get; set; }
    [JsonIgnore]
    public float AtmosphereMie { get; set; }
    [JsonIgnore]
    public float AtmosphereRayleigh { get; set; }
    [JsonIgnore]
    public float CloudAttenuation { get; set; }
    [JsonIgnore]
    public float CloudBrightness { get; set; }
    [JsonIgnore]
    public float CloudColoring { get; set; }
    [JsonIgnore]
    public float CloudCoverage { get; set; }
    [JsonIgnore]
    public float CloudOpacity { get; set; }
    [JsonIgnore]
    public float CloudSaturation { get; set; }
    [JsonIgnore]
    public float CloudScattering { get; set; }
    [JsonIgnore]
    public float CloudSharpness { get; set; }
    [JsonIgnore]
    public float CloudSize { get; set; }
    [JsonIgnore]
    public float Rainbow { get; set; }
    [JsonIgnore]
    public float Thunder { get; set; }
    [JsonIgnore]
    public float Time { get; set; }
    [JsonIgnore]
    private Hash<string, EnvironmentInfo> zoneOverrides;
    [JsonIgnore]
    private string currentZoneId;
    public bool HasZoneOverride();
    public bool GetZoneOverride(EnvironmentInfo environmentInfo);
    public void OnEnterZone(string zoneId, EnvironmentInfo environmentInfo);
    public void OnExitZone(string zoneId, BasePlayer player);
    [JsonIgnore]
    public bool HasReplicatedVars { get; set; }
    [JsonIgnore]
    public bool VarsDirty;
    [JsonIgnore]
    private Hash<string, string> replicatedVars;
    [JsonIgnore]
    public float GetDesiredTime { get; set; }
    public bool ShouldRemove();
    public void SendCustomReplicatedVars(BasePlayer player);
    public void BuildReplicatedConvarList();
}


```

---

## PrivateMessage

```csharp
using System.Collections.Generic;
using System.Globalization;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("PrivateMessage", "Nogrod", "2.0.4", ResourceId = 659)]
 class PrivateMessage : RustPlugin
{
    private readonly Dictionary<ulong, ulong> pmHistory;
    [PluginReference]
    private Plugin Ignore;
    [PluginReference]
    private Plugin BetterChat;
    private void Init();
     void OnPlayerDisconnected(BasePlayer player);
    [ChatCommand("pm")]
     void cmdPm(BasePlayer player, string command, string[] args);
    [ChatCommand("r")]
     void cmdPmReply(BasePlayer player, string command, string[] args);
    private void PrintMessage(BasePlayer player, string msgId, object[] args);
    private static BasePlayer FindPlayer(string nameOrIdOrIp);
    private static BasePlayer FindPlayer(ulong id);
}


```

---

## PrivateZones

```csharp
using System.Collections.Generic;
using Oxide.Core.Plugins;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("PrivateZones", "k1lly0u", "0.1.3", ResourceId = 1703)]
 class PrivateZones : RustPlugin
{
    [PluginReference]
     Plugin ZoneManager;
    [PluginReference]
     Plugin PopupNotifications;
    private bool Changed;
     ZoneDataStorage data;
    private DynamicConfigFile ZoneData;
    private static LayerMask GROUND_MASKS;
     void Loaded();
     void OnServerInitialized();
     void InitPerms();
     void Unload();
    private void TPPlayer(BasePlayer player, Vector3 pos);
    private Vector3 CalculateOutsidePos(BasePlayer player, string zoneID);
    static Vector3 CalculateGroundPos(Vector3 sourcePos);
     void OnEnterZone(string ZoneID, BasePlayer player);
    [ChatCommand("pz")]
    private void cmdPZ(BasePlayer player, string command, string[] args);
     bool isAuth(BasePlayer player);
     bool hasPermission(BasePlayer player);
     void SaveData();
     void LoadData();
     class ZoneDataStorage
    {
        public Dictionary<string, string> zones;
        public ZoneDataStorage();
    }

    private void SendMsg(BasePlayer player, string msg);
    private Dictionary<string, string> messages;
}

 class ZoneDataStorage
{
    public Dictionary<string, string> zones;
    public ZoneDataStorage();
}


```

---

## PrivilegeDeploy

```csharp
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("PrivilegeDeploy", "k1lly0u", "0.1.23", ResourceId = 1800)]
 class PrivilegeDeploy : RustPlugin
{
    private readonly int triggerMask;
    private bool Loaded;
    private Dictionary<ulong, PendingItem> pendingItems;
     void OnServerInitialized();
     void OnEntitySpawned(BaseNetworkable entity);
    private void CheckForDuplicate(BasePlayer player);
    private void GivePlayerItem(BasePlayer player);
    private bool HasPriv(BasePlayer player);
    private ConfigData configData;
     class ConfigData
    {
        public List<string> deployables { get; set; }
    }

    private void LoadVariables();
    private void RegisterMessages();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     class PendingItem
    {
        public Timer timer;
        public Item item;
    }

     Dictionary<string, string> messages;
}

 class ConfigData
{
    public List<string> deployables { get; set; }
}

 class PendingItem
{
    public Timer timer;
    public Item item;
}


```

---

## Prod

```csharp
using System.Collections.Generic;
using System;
using System.Reflection;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Libraries.Covalence;

Oxide.Plugins
[Info("Prod", "Reneb", "2.2.5", ResourceId = 683)]
 class Prod : RustPlugin
{
    private int prodAuth;
    private string helpProd;
    private string noAccess;
    private string noTargetfound;
    private string noCupboardPlayers;
    private string Toolcupboard;
    private string noBlockOwnerfound;
    private string noCodeAccess;
    private string codeLockList;
    private string boxNeedsCode;
    private string boxCode;
    private FieldInfo serverinput;
    private FieldInfo codelockwhitelist;
    private FieldInfo codenum;
    private FieldInfo npcnextTick;
    private FieldInfo meshinstances;
    private Vector3 eyesAdjust;
    private bool Changed;
    [PluginReference]
     Plugin BuildingOwners;
    [PluginReference]
     Plugin DeadPlayersList;
    [PluginReference]
     Plugin PlayerDatabase;
     void Loaded();
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private bool isPluginDev;
    private bool dumpAll;
    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private bool hasAccess(BasePlayer player);
    [ChatCommand("prod")]
     void cmdChatProd(BasePlayer player, string command, string[] args);
    private void GetDeployableCode(BasePlayer player, BaseEntity block);
    private void GetDeployedItemOwner(BasePlayer player, SleepingBag ditem);
    private object FindOwnerBlock(BuildingBlock block);
    private string FindPlayerName(ulong userId);
    private void SendBasePlayerFind(BasePlayer player, ulong ownerid);
    private void GetBuildingblockOwner(BasePlayer player, BuildingBlock block);
    private void GetToolCupboardUsers(BasePlayer player, BuildingPrivlidge cupboard);
    private void Dump(BaseEntity col);
    private BaseEntity DoRay(Vector3 Pos, Vector3 Aim);
     void SendHelpText(BasePlayer player);
}


```

---

## ProfanityFilter

```csharp
using System.Collections.Generic;
using System;
using System.Reflection;
using System.Data;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("ProfanityFilter", "OwnProx", "1.0.1")]
 class ProfanityFilter : RustPlugin
{
     string[] IllegalWords;
     bool TempBanOnBadWords;
     bool BanOnBadWord;
     bool KickOnBadWord;
     int TempBanTime;
    private Dictionary<ulong, float> Bans;
    private System.Timers.Timer timer;
    private DateTime NowTimePlease;
    private List<ulong> IdsToRemove;
    private object OnPlayerChat(ConsoleSystem.Arg arg);
    private void OnPlayerInit(BasePlayer player);
    private void Loaded();
    private void Unload();
    private double GetTimestamp();
    private void OnTimer(object sender, System.Timers.ElapsedEventArgs e);
}


```

---

## Promo-1.0.1

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Promo", "xkrystalll", "1.0.1")]
 class Promo : RustPlugin
{
    internal class ItemData
    {
        [JsonProperty(Order = 0)]
        public string Shortname;
        [JsonProperty("Skin", Order = 1)]
        public ulong SkinID;
        [JsonProperty(Order = 2)]
        public int Amount;
        [JsonProperty("Display name (still empty if not need to change)", Order = 4)]
        public string DisplayName;
        public Item ToItem();
    }

    internal class Reward
    {
        [JsonProperty("Команды (%STEAMID% - id игрока) | Оставить пустым если нужно выдавать предметы ниже", Order = 0)]
        public List<string> Commands;
        [JsonProperty("Предметы", Order = 1)]
        public List<ItemData> Items;
        public new RewardType GetType();
    }

    internal class Promocode
    {
        public string Code;
        public int Usages;
        public bool Enabled;
        public Reward Reward;
    }

    private Dictionary<string, int> EnteredPromocodesAmount;
    private Dictionary<ulong, List<string>> EnteredPromocodesPlayers;
    private void OnServerInitialized();
    private void Unload();
    private void OnServerSave();
    private bool CanTakePromocode(BasePlayer target, string code);
    private void TakeCode(BasePlayer player, KeyValuePair<string, Promocode> promocodeInfo);
    private void SendMessage(BasePlayer player, string message);
    private void cmdTakePromocode(BasePlayer player, string command);
    private ConfigData cfg;
    public class ConfigData
    {
        [JsonProperty("Аватарка")]
        public ulong AvatarID;
        [JsonProperty("Промокоды")]
        public Dictionary<string, Promocode> Promocodes;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void SaveConfig(object config);
    private void SaveData();
    private void LoadData();
    private void LoadDefaultMessages();
    private string GetMsg(string key, ulong id, object[] args);
}

internal class ItemData
{
    [JsonProperty(Order = 0)]
    public string Shortname;
    [JsonProperty("Skin", Order = 1)]
    public ulong SkinID;
    [JsonProperty(Order = 2)]
    public int Amount;
    [JsonProperty("Display name (still empty if not need to change)", Order = 4)]
    public string DisplayName;
    public Item ToItem();
}

internal class Reward
{
    [JsonProperty("Команды (%STEAMID% - id игрока) | Оставить пустым если нужно выдавать предметы ниже", Order = 0)]
    public List<string> Commands;
    [JsonProperty("Предметы", Order = 1)]
    public List<ItemData> Items;
    public new RewardType GetType();
}

internal class Promocode
{
    public string Code;
    public int Usages;
    public bool Enabled;
    public Reward Reward;
}

public class ConfigData
{
    [JsonProperty("Аватарка")]
    public ulong AvatarID;
    [JsonProperty("Промокоды")]
    public Dictionary<string, Promocode> Promocodes;
}


```

---

## Promocode

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Promocode", "rustmods.ru", "1.0.1")]
public class Promocode : RustPlugin
{
    public class PSettings
    {
        [JsonProperty("Промокод, который нужно ввести")]
        public string Promocode;
        [JsonProperty("Сколько игрок получит рублей за данный промокод?")]
        public int GRub;
    }

    private ConfigData _config;
    public class ConfigData
    {
        [JsonProperty("Настройка промокодов")]
        public List<PSettings> PList;
        [JsonProperty("Номер магазина!")]
        public string ShopID;
        [JsonProperty("Секретный ключ")]
        public string APIKey;
        public static ConfigData GetNewCong();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public class PlayerPromo
    {
        [JsonProperty("Введеный промокод")]
        public string PromoCode;
        [JsonProperty("Во сколько он ввел промокод")]
        public string Date;
    }

    public class PlayerPromoSettings
    {
        public List<PlayerPromo> _promoList;
    }

    public Dictionary<ulong, PlayerPromoSettings> _playerPromo;
     void OnServerInitialized();
    [PluginReference]
    private Plugin ImageLibrary;
    public string Layer;
    public string ImageButton;
    [ConsoleCommand("destory.menusss")]
     void DestroyMenu(ConsoleSystem.Arg args);
    [ChatCommand("promo")]
     void PieMenu(BasePlayer player, string command, string[] args);
    [ConsoleCommand("UI_HandlerPromo")]
     void GetPromo(ConsoleSystem.Arg args);
     void MoneyPlus(ulong userId, int amount);
     void ExecuteApiRequest(Dictionary<string, string> args);
     void OnServerSave();
     void Unload();
}

public class PSettings
{
    [JsonProperty("Промокод, который нужно ввести")]
    public string Promocode;
    [JsonProperty("Сколько игрок получит рублей за данный промокод?")]
    public int GRub;
}

public class ConfigData
{
    [JsonProperty("Настройка промокодов")]
    public List<PSettings> PList;
    [JsonProperty("Номер магазина!")]
    public string ShopID;
    [JsonProperty("Секретный ключ")]
    public string APIKey;
    public static ConfigData GetNewCong();
}

public class PlayerPromo
{
    [JsonProperty("Введеный промокод")]
    public string PromoCode;
    [JsonProperty("Во сколько он ввел промокод")]
    public string Date;
}

public class PlayerPromoSettings
{
    public List<PlayerPromo> _promoList;
}


```

---

## Promocodes

```csharp
using System.Collections.Generic;
using System.Linq;
using System;

Oxide.Plugins
[Info("Promocodes", "LaserHydra", "2.1.2", ResourceId = 1471)]
[Description("Set up promocodes which run a command on the player who redeemed a code")]
 class Promocodes : RustPlugin
{
    static List<Promocode> promocodes;
     class Promocode
    {
        public List<object> availableCodes;
        public List<object> commands;
        public Promocode();
        internal Promocode(string command, int generateCodes);
        internal Promocode(object obj);
        internal static Promocode Get(string code);
        internal static void Redeem(string code, BasePlayer player);
        internal static string GenerateCode();
    }

     void Loaded();
     void LoadMessages();
     void LoadConfig();
    protected override void LoadDefaultConfig();
    [ChatCommand("redeem")]
     void cmdRedeem(BasePlayer player, string cmd, string[] args);
     string ListToString(List<T> list, int first, string seperator);
    static T TryConvert(S source, T converted);
     void SetConfig(object[] args);
     T GetConfig(T defaultVal, object[] args);
     string GetMsg(string key, object userID);
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}

 class Promocode
{
    public List<object> availableCodes;
    public List<object> commands;
    public Promocode();
    internal Promocode(string command, int generateCodes);
    internal Promocode(object obj);
    internal static Promocode Get(string code);
    internal static void Redeem(string code, BasePlayer player);
    internal static string GenerateCode();
}


```

---

## ProtocolKickInfo

```csharp
using System;
using System.Collections.Generic;
using Network;
using UnityEngine;

Oxide.Plugins
[Info("ProtocolKickInfo", "Fujikura", "1.0.2", ResourceId = 2041)]
 class ProtocolKickInfo : RustPlugin
{
    private int serverProtocol;
     Dictionary <ulong, int> antiSpam;
     Dictionary <ulong, int> quitTimer;
     void LoadDefaultMessages();
     void Init();
     void OnClientAuth(Connection connection);
}


```

---

## ProximityAlert

```csharp
using System.Collections.Generic;
using Oxide.Core;
using System.Linq;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("ProximityAlert", "k1lly0u", "0.1.22", ResourceId = 1801)]
 class ProximityAlert : RustPlugin
{
    [PluginReference]
     Plugin Clans;
    [PluginReference]
     Plugin Friends;
    [PluginReference]
     Plugin EventManager;
    private static int playerLayer;
     List<ProximityPlayer> playerList;
    private Vector2 guiPos;
    private Vector2 guiDim;
     void OnServerInitialized();
     void Unload();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
    private void DestroyPlayer(BasePlayer player);
    private void InitializePlugin();
    private void InitializePlayer(BasePlayer player);
    private void CheckDependencies();
    private void ProxCollisionEnter(BasePlayer player);
    private void ProxCollisionLeave(BasePlayer player);
    private bool PA_IsClanmate(ulong playerId, ulong friendId);
    private bool PA_IsFriend(ulong playerID, ulong friendID);
    private bool PA_IsPlaying(BasePlayer player);
    private void SendUI(BasePlayer player, string msg);
    private ProximityPlayer GetPlayer(BasePlayer player);
    [ChatCommand("prox")]
    private void cmdProx(BasePlayer player, string command, string[] args);
     class ProximityPlayer : MonoBehaviour
    {
        private BasePlayer player;
        private List<ulong> inProximity;
        private float collisionRadius;
        public ProximityAlert Instance;
        public bool GUIDestroyed;
        public bool Activated;
        private void Awake();
        public void SetRadius(float radius);
        private void OnDestroy();
        private void UpdateTrigger();
        private bool IsClanmate(BasePlayer target);
        private bool IsFriend(BasePlayer target);
        private bool IsPlaying(BasePlayer player);
         void EnterTrigger();
         void LeaveTrigger();
        public void UseUI(string msg, Vector2 pos, Vector2 dim, int size);
        private void DestroyNotification();
    }

    private ConfigData configData;
     class ConfigData
    {
        public float GUI_X_Pos { get; set; }
        public float GUI_X_Dim { get; set; }
        public float GUI_Y_Pos { get; set; }
        public float GUI_Y_Dim { get; set; }
        public float TriggerRadius { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void SaveConfig(ConfigData config);
    private void RegisterMessages();
     Dictionary<string, string> messages;
}

 class ProximityPlayer : MonoBehaviour
{
    private BasePlayer player;
    private List<ulong> inProximity;
    private float collisionRadius;
    public ProximityAlert Instance;
    public bool GUIDestroyed;
    public bool Activated;
    private void Awake();
    public void SetRadius(float radius);
    private void OnDestroy();
    private void UpdateTrigger();
    private bool IsClanmate(BasePlayer target);
    private bool IsFriend(BasePlayer target);
    private bool IsPlaying(BasePlayer player);
     void EnterTrigger();
     void LeaveTrigger();
    public void UseUI(string msg, Vector2 pos, Vector2 dim, int size);
    private void DestroyNotification();
}

 class ConfigData
{
    public float GUI_X_Pos { get; set; }
    public float GUI_X_Dim { get; set; }
    public float GUI_Y_Pos { get; set; }
    public float GUI_Y_Dim { get; set; }
    public float TriggerRadius { get; set; }
}


```

---

## ProxyBlocker

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Proxy Blocker", "Frizen", "2.0.0")]
public class ProxyBlocker : RustPlugin
{
    public Configuration config;
    [JsonProperty("Токен от группы ВК(От группы будут идти сообщения в беседу.Вам нужно добавить свою группу в беседу!)")]
    public string Token;
    [JsonProperty("ID беседы для группы")]
    public string ChatID;
    public class Configuration
    {
        [JsonProperty("Причина кика за VPN")]
        public string KickPlayerMessage;
        [JsonProperty("Список SteamID которых не нужно проверять")]
        public List<ulong> IgnoreList;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
     void OnPlayerConnected(BasePlayer player);
    [ConsoleCommand("proxy.ignore")]
     void CmdACIgnore(ConsoleSystem.Arg arg);
     void VKSendMessage(string Message);
}

public class Configuration
{
    [JsonProperty("Причина кика за VPN")]
    public string KickPlayerMessage;
    [JsonProperty("Список SteamID которых не нужно проверять")]
    public List<ulong> IgnoreList;
}


```

---

## PsyArrows

```csharp
using Oxide.Core;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Reflection;
using System.Linq;
using UnityEngine;
using System.Text;
using Rust;
using Oxide.Core.Libraries;

Oxide.Plugins
[Info("PsyArrows", "Psyk", "0.1", ResourceId = 714)]
[Description("More Arrow Types For Rust! - By Psyk.")]
 class PsyArrows : RustPlugin
{
     string arrow_type;
     bool enabled;
     float detonationTime;
     float projectileSpeed;
     float gravityModifier;
     float expRad;
     int arrow_curr;
     int arrow_price;
     bool sticky;
     bool highjump;
    static System.Random rnd;
     Dictionary<ulong, string> bowmen;
     int GetAmountItems(BasePlayer player);
     void ArrowBleed(BasePlayer player);
     void OnPlayerAttack(BasePlayer attacker, HitInfo hitInfo);
    private static readonly FieldInfo ServerInput;
    private BaseEntity CreateRocket(Vector3 startPoint, Vector3 direction, bool isFireRocket);
    private ItemDefinition GetRocket();
    private ItemDefinition GetFireRocket();
     void poisionPlayer(BasePlayer player);
     string GetVictim(BaseCombatEntity vic);
    [ChatCommand("arrow")]
     void arrow(BasePlayer player, string command, string[] args);
}


```

---

## PumpkinBombs

```csharp
using System.Collections.Generic;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("PumpkinBombs", "k1lly0u", "0.1.1", ResourceId = 2070)]
 class PumpkinBombs : RustPlugin
{
    private const string Jack1;
    private const string Jack2;
    private Dictionary<string, ItemDefinition> ItemDefs;
    private List<ulong> craftedBombs;
     void Loaded();
     void OnServerInitialized();
     void OnPlayerDisconnected(BasePlayer player);
     void OnEntitySpawned(BaseNetworkable entity);
     bool CanUse(BasePlayer player);
     bool IsFree(BasePlayer player);
    private bool HasEnoughRes(BasePlayer player, int itemid, int amount);
    private void TakeResources(BasePlayer player, int itemid, int amount);
     class BombLight : MonoBehaviour
    {
         bool isOn;
        public void Awake();
        public void OnDestroy();
        private void StartLight();
    }

    [ChatCommand("pb")]
     void cmdPB(BasePlayer player, string command, string[] args);
     class CraftCost
    {
        public string shortname;
        public int itemid;
        public int amount;
    }

     class Explosive
    {
        public int DetonationTimer { get; set; }
        public float ExplosionRadius { get; set; }
        public float DamageAmount { get; set; }
    }

     class Messaging
    {
        public string Main { get; set; }
    }

    private ConfigData configData;
     class ConfigData
    {
        public Explosive ExplosiveSettings { get; set; }
        public List<CraftCost> CraftingCosts { get; set; }
        public Messaging Messaging { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     string msg(string key, string id);
     Dictionary<string, string> Messages;
}

 class BombLight : MonoBehaviour
{
     bool isOn;
    public void Awake();
    public void OnDestroy();
    private void StartLight();
}

 class CraftCost
{
    public string shortname;
    public int itemid;
    public int amount;
}

 class Explosive
{
    public int DetonationTimer { get; set; }
    public float ExplosionRadius { get; set; }
    public float DamageAmount { get; set; }
}

 class Messaging
{
    public string Main { get; set; }
}

 class ConfigData
{
    public Explosive ExplosiveSettings { get; set; }
    public List<CraftCost> CraftingCosts { get; set; }
    public Messaging Messaging { get; set; }
}


```

---

## Purge

```csharp
using System;
using UnityEngine;
using Oxide.Core.Plugins;
using Rust;

Oxide.Plugins
[Info("Purge", "innominata", "0.0.1")]
 class Purge : RustPlugin
{
    public int sunset;
    public int sunrise;
    private int tickCount;
    private bool night;
     void startPurge();
     void endPurge();
    private bool isNight();
    private bool isDay();
    private void checkTime();
    [HookMethod("OnTick")]
    private void OnTick();
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void Announce(string msg);
}


```

---

## PurifierConfig

```csharp
using System;
using System.Collections.Generic;
using System.Reflection;
using UnityEngine;

Oxide.Plugins
[Info("Purifier Config", "Shady", "1.0.3", ResourceId = 1911)]
[Description("Tweak settings for water purifiers.")]
 class PurifierConfig : RustPlugin
{
     bool configWasChanged;
     FieldInfo warmUpTime;
     bool init;
     int WPM { get; set; }
     int WaterRatio { get; set; }
     float Warmup { get; set; }
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     void Init();
     void OnEntitySpawned(BaseNetworkable entity);
     void ConfigurePurifier(WaterPurifier purifier);
     T GetConfig(string name, T defaultValue);
}


```

---

## PvpMarkers

```csharp
using System.Collections.Generic;
using Oxide.Core;
using UnityEngine;
using System;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Pvp Markers", "Paul", "1.0.1")]
[Description("PvP Markers on the map")]
 class PvpMarkers : RustPlugin
{
    private const string permAllow;
    private Configuration config;
    private HashSet<MapMarkerGenericRadius> pvpMarkers;
    private void Init();
    private void Unload();
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
    private object CanNetworkTo(MapMarkerGenericRadius marker, BasePlayer player);
    private void CreateMarker(Vector3 position);
    private void ClearMarkers();
    private bool IsFar(Vector3 position);
    private double GetDistance(Vector3 pos1, Vector3 pos2);
    private Color ParseColor(string hexColor);
    [ChatCommand("pmtest")]
    private void cmdRaidMarker(BasePlayer player, string command, string[] args);
    private class Configuration
    {
        [JsonProperty(PropertyName = "Distance when place new marker from another marker")]
        public int markerDistance;
        [JsonProperty(PropertyName = "Allow NPC")]
        public bool allowNpc;
        [JsonProperty(PropertyName = "Marker configuration")]
        public MarkerConfiguration markerConfiguration;
        public VersionNumber version;
    }

    private class MarkerConfiguration
    {
        [JsonProperty(PropertyName = "Alpha")]
        public float markerAlpha;
        [JsonProperty(PropertyName = "Radius")]
        public float markerRadius;
        [JsonProperty(PropertyName = "Color1")]
        public string markerColor1;
        [JsonProperty(PropertyName = "Color2")]
        public string markerColor2;
        [JsonProperty(PropertyName = "Duration")]
        public float markerDuration;
    }

    private Configuration GetDefaultConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
    private void UpdateConfig();
    private string GetLang(string key, string playerID);
    protected override void LoadDefaultMessages();
}

private class Configuration
{
    [JsonProperty(PropertyName = "Distance when place new marker from another marker")]
    public int markerDistance;
    [JsonProperty(PropertyName = "Allow NPC")]
    public bool allowNpc;
    [JsonProperty(PropertyName = "Marker configuration")]
    public MarkerConfiguration markerConfiguration;
    public VersionNumber version;
}

private class MarkerConfiguration
{
    [JsonProperty(PropertyName = "Alpha")]
    public float markerAlpha;
    [JsonProperty(PropertyName = "Radius")]
    public float markerRadius;
    [JsonProperty(PropertyName = "Color1")]
    public string markerColor1;
    [JsonProperty(PropertyName = "Color2")]
    public string markerColor2;
    [JsonProperty(PropertyName = "Duration")]
    public float markerDuration;
}


```

---

## PvXselector

```csharp
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("PvXSelector", "Alphawar", "0.9.5", ResourceId = 1817)]
[Description("Player vs X Selector: Beta version 14")]
 class PvXselector : RustPlugin
{
     PlayerDataStorage playerData;
    private DynamicConfigFile PlayerData;
     TicketDataStorage ticketData;
    private DynamicConfigFile TicketData;
     TicketLogStorage ticketLog;
    private DynamicConfigFile TicketLog;
    private List<ulong> selectGuiOpen;
    private Hash<ulong, PlayerInfo> InfoCache;
    private List<ulong> SleeperCache;
    private List<ulong> UnknownUserCache;
    private List<BasePlayer> AdminPlayerMode;
    private List<BasePlayer> activeAdmins;
    private List<ulong> antiChatSpam;
    private Dictionary<ulong, List<string>> OpenUI;
     class PlayerDataStorage
    {
        public Hash<ulong, PlayerInfo> Info;
        public List<ulong> sleepers;
        public List<ulong> UnknownUser;
    }

     class TicketDataStorage
    {
        public Dictionary<int, ulong> Link;
        public Dictionary<ulong, Ticket> Info;
        public Dictionary<ulong, string> Notification;
    }

     class TicketLogStorage
    {
        public Dictionary<int, LogData> Log;
    }

     class PlayerInfo
    {
        public string username;
        public string FirstConnection;
        public string LatestConnection;
        public string mode;
        public bool ticket;
    }

     class Ticket
    {
        public int TicketNumber;
        public string Username;
        public string UserId;
        public string requested;
        public string reason;
        public string CreatedTimeStamp;
    }

     class LogData
    {
        public string UserId;
        public string AdminId;
        public string Username;
        public string AdminName;
        public string requested;
        public string reason;
        public bool Accepted;
        public string CreatedTimeStamp;
        public string ClosedTimeStamp;
    }

     void OnServerInitialized();
     void Loaded();
     void Unloaded();
     void saveCacheData();
     void saveTicketData();
     void saveTicketLog();
     void SaveAll();
     void LoadData();
     void OnPlayerInit(BasePlayer _player);
     void PlayerLoaded(BasePlayer _player);
     void OnPlayerDisconnected(BasePlayer _player);
     void OnPlayerRespawned(BasePlayer _player);
     void addTicketLog(BasePlayer _admin, int _ticketID, bool _);
     void checkPlayersRegistered();
     void addPlayer(BasePlayer _player);
     void addSleeper(BasePlayer _player);
     void addOffline(ulong _userID);
    static string pvxPlayerSelectorUI;
    static string pvxPlayerUI;
    static string pvxAdminUI;
     string[] GuiList;
    private bool PvEAttackPvE;
    private bool PvEAttackPvP;
    private bool PvPAttackPvE;
    private bool PvPAttackPvP;
    private bool PvELootPvE;
    private bool PvELootPvP;
    private bool PvPLootPvE;
    private bool PvPLootPvP;
    private bool PvEUsePvPDoor;
    private bool PvPUsePvEDoor;
    private float PvEDamagePvE;
    private float PvEDamagePvP;
    private float PvPDamagePvE;
    private float PvPDamagePvP;
    private float PvEDamagePvEStruct;
    private float PvEDamagePvPStruct;
    private float PvPDamagePvEStruct;
    private float PvPDamagePvPStruct;
    private float PvEFoodLossRate;
    private float PvEWaterLossRate;
    private float PvEHealthGainRate;
    private float PvEFoodSpawn;
    private float PvEWaterSpawn;
    private float PvEHealthSpawn;
    private float PvPFoodLossRate;
    private float PvPWaterLossRate;
    private float PvPHealthGainRate;
    private float PvPFoodSpawn;
    private float PvPWaterSpawn;
    private float PvPHealthSpawn;
    private bool NPCAttackPvE;
    private bool NPCAttackPvP;
    private bool PvEAttackNPC;
    private bool PvPAttackNPC;
    private float NPCDamagePvE;
    private float NPCDamagePvP;
    private float PvEDamageNPC;
    private float PvPDamageNPC;
    private bool PvELootNPC;
    private bool PvPLootNPC;
    private float PvEDamageAnimals;
    private float PvPDamageAnimals;
    private float NPCDamageAnimals;
    private float AnimalsDamagePvE;
    private float AnimalsDamagePvP;
    private float AnimalsDamageNPC;
    private bool TurretPvETargetPvE;
    private bool TurretPvETargetPvP;
    private bool TurretPvPTargetPvE;
    private bool TurretPvPTargetPvP;
    private bool TurretPvETargetNPC;
    private bool TurretPvPTargetNPC;
    private bool TurretPvETargetAnimal;
    private bool TurretPvPTargetAnimal;
    private float TurretPvEDamagePvEAmnt;
    private float TurretPvEDamagePvPAmnt;
    private float TurretPvPDamagePvEAmnt;
    private float TurretPvPDamagePvPAmnt;
    private float TurretPvEDamageNPCAmnt;
    private float TurretPvPDamageNPCAmnt;
    private float TurretPvEDamageAnimalAmnt;
    private float TurretPvPDamageAnimalAmnt;
    private bool HeliTargetPvE;
    private bool HeliTargetPvP;
    private bool HeliTargetNPC;
    private float HeliDamagePvE;
    private float HeliDamagePvP;
    private float HeliDamageNPC;
    private float HeliDamagePvEStruct;
    private float HeliDamagePvPStruct;
    private float HeliDamageAnimal;
    private float HeliDamageByPvE;
    private float HeliDamageByPvP;
    private float FireDamagePvE;
    private float FireDamagePvP;
    private float FireDamageNPC;
    private float FireDamagePvEStruc;
    private float FireDamagePvPStruc;
    public static bool DisableUI_FadeIn;
    private bool DebugMode;
    private bool NamesIncludeSleepers;
    private string ChatPrefixColor;
    private string ChatPrefix;
    private string ChatMessageColor;
    protected override void LoadDefaultConfig();
     void LoadVariables();
     object GetConfig(string menu, string dataValue, object defaultValue);
     T GetConfig(object[] args);
     bool hasPerm(BasePlayer _player, string perm, string reason);
     void permissionHandle();
     class QUI
    {
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align, float fadein);
        static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, float fadein);
        static public void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
        static public void CreateTextOverlay(CuiElementContainer container, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
    }

    private void DestroyAllPvXUI(BasePlayer player);
    private void DestroyPvXUI(BasePlayer player, string _ui);
    private void AddUIString(BasePlayer player, string name);
    private Dictionary<string, string> UIColors;
    private void createPvXSelector(BasePlayer _player);
    private void updatePvXSelector(BasePlayer _player);
    private void createPvXIndicator(BasePlayer _player);
    private void updatePvXIndicator(BasePlayer _player);
    private void createAdminIndicator(BasePlayer _player);
    private void UpdateAdminIndicator();
    private void createButton(BasePlayer _player);
     void LangMSG(BasePlayer _player, string langMsg, object[] args);
     void LangMSGBroadcast(string langMsg, object[] args);
     void BroadcastMessageHandle(string message, object[] args);
     void ChatMessageHandle(BasePlayer _player, string message, object[] args);
     void PutsLang(string langMsg, object[] args);
     void PutsPlayerHandle(BasePlayer _player, string msg, object[] args);
     void PutsPlayerHandleLang(BasePlayer _player, string langMsg, object[] args);
     Dictionary<string, string> messages;
     void createTicket(BasePlayer _player, string selection);
     void cancelTicket(BasePlayer _player);
     void ticketAccept(BasePlayer _admin, int _ticketID);
     void ticketDecline(BasePlayer _admin, int _ticketID);
     void ticketCount(BasePlayer _player);
     void listTickets(BasePlayer _player);
     void displayTicket(BasePlayer _player, int _ticketID);
     bool playerHasTicket(BasePlayer _player);
     bool playerHasTicket(ulong _userID);
     int GetNewID();
     int NewLogID();
     void consolListTickets();
     void consolListLog();
     ItemContainer.CanAcceptResult CanAcceptItem(ItemContainer container, Item item);
    private object CanLootPlayer(BasePlayer _target, BasePlayer _looter);
    private void OnLootPlayer(BasePlayer _looter, BasePlayer _target);
    private void OnLootEntity(BasePlayer _looter, BaseEntity _target);
    private List<object> BuildEntityList;
    private List<object> BasePartEntityList;
    private List<object> CombatPartEntityList;
     void OnEntitySpawned(BaseNetworkable _entity);
    [PluginReference]
     Plugin Vanish;
    [PluginReference]
     Plugin Skills;
     bool checkInvis(BasePlayer _player);
    [PluginReference]
    private Plugin BetterChat;
     void RegisterGroups();
     void updatePlayerChatTag(BasePlayer _player);
    private bool GroupExists(string name);
    private bool NewGroup(string name);
    private bool UserInGroup(string ID, string name);
    private bool AddToGroup(string ID, string name);
    private bool RemoveFromGroup(string ID, string name);
    private object SetGroupTitle(string name);
    private object SetGroupColor(string name);
    [PluginReference]
    private Plugin HumanNPC;
     bool isNPC(ulong _test);
     bool isNPC(BasePlayer _test);
     bool isNPC(BaseCombatEntity _player);
     bool isNPC(PlayerCorpse _test);
     void npcDamageHandle(BasePlayer _NPC, HitInfo _hitInfo);
     void npcAttackHandle(BasePlayer _target, HitInfo _hitInfo);
     bool canLootNPC(BasePlayer _player);
     void npcLootHandle(BasePlayer _player);
    [PluginReference]
    private Plugin EventManager;
     bool isInEvent(BasePlayer _player1);
     bool areInEvent(BasePlayer _player1, BasePlayer _player2);
    [PluginReference]
    private Plugin Godmode;
    private bool checkIsGod(string _player);
    private bool isGod(ulong _player);
    private bool isGod(BasePlayer _player);
     void OnDoorOpened(Door _door, BasePlayer _player);
    private bool PvPOnlyCheck(BasePlayer _player1, BasePlayer _player2);
    private bool PvPOnlyCheck(ulong _player1, ulong _player2);
    private bool PvEOnlyCheck(BasePlayer _player1, BasePlayer _player2);
    private bool PvEOnlyCheck(ulong _player1, ulong _player2);
    private bool SameOnlyCheck(BasePlayer _player1, BasePlayer _player2);
    private bool SameOnlyCheck(ulong _player1, ulong _player2);
     bool isplayerNA(BasePlayer _player);
     bool isplayerNA(ulong _playerID);
     bool isPvP(ulong _playerID);
     bool isPvP(BasePlayer _player);
     bool isPvP(BaseCombatEntity _BaseCombat);
     bool isPvE(ulong _playerID);
     bool isPvE(BasePlayer _player);
     bool isPvE(BaseCombatEntity _BaseCombat);
     bool BaseplayerCheck(BasePlayer _attacker, BasePlayer _victim);
     bool IsDigitsOnly(string str);
     BasePlayer basePlayerByID(ulong _ID);
    public void updatePvXPlayerData(BasePlayer _player);
     bool isPvEUlong(ulong _playerID);
     bool isPvEBaseplayer(BasePlayer _player);
    [ChatCommand("pvx")]
     void PvXChatCmd(BasePlayer _player, string cmd, string[] args);
    [ConsoleCommand("pvx.cmd")]
     void PvXConsoleCmd(ConsoleSystem.Arg arg);
    [ConsoleCommand("PvXGuiCMD")]
     void PvXGuiCMD(ConsoleSystem.Arg arg);
    [ChatCommand("pvxhide")]
     void test1(BasePlayer _player, string cmd, string[] args);
    [ChatCommand("pvxshow")]
     void test(BasePlayer _player, string cmd, string[] args);
     void adminFunction(BasePlayer _player, string[] args);
     void changeFunction(BasePlayer _player);
     void selectFunction(BasePlayer _player, string[] args);
     void ticketFunction(BasePlayer _player, string[] args);
     void guiFunction(BasePlayer _player, string[] args);
     void debugFunction();
     void developerFunction();
     void helpFunction(BasePlayer _player);
     void OnEntityTakeDamage(BaseCombatEntity _target, HitInfo hitinfo);
     void testvar(BaseCombatEntity _target, HitInfo hitinfo);
     void PlayerVPlayer(BasePlayer _victim, BasePlayer _attacker, HitInfo _hitinfo);
     void PlayerVBuilding(BaseEntity _target, BasePlayer _attacker, HitInfo _hitinfo);
     void PlayerVHeli(BasePlayer _attacker, HitInfo _hitinfo);
     void HeliVPlayer(BasePlayer _victim, HitInfo _hitinfo);
     void HeliVBuilding(BaseEntity _target, HitInfo _hitinfo);
     void HeliVAnimal(BaseNPC _target, HitInfo _hitinfo);
     void PlayerVTurret(AutoTurret _target, BasePlayer _attacker, HitInfo _hitinfo);
     void TurretVPlayer(BasePlayer _target, AutoTurret _attacker, HitInfo _hitinfo);
     void TurretVTurret(AutoTurret _target, AutoTurret _attacker, HitInfo _hitinfo);
     void TurretVAnimal(BaseNPC _target, AutoTurret _attacker, HitInfo _hitinfo);
     void PlayerVAnimal(BasePlayer _attacker, HitInfo _hitinfo);
     void AnimalVPlayer(BasePlayer _target, HitInfo _hitinfo);
     void FireVPlayer(BasePlayer _target, HitInfo _hitinfo);
     void FireVBuilding(BaseEntity _target, HitInfo _hitinfo);
    private object CanBeTargeted(BaseCombatEntity _target, MonoBehaviour turret);
     bool HeliTargetPlayer(BasePlayer _target);
     bool TurretTargetPlayer(BasePlayer _target, AutoTurret _attacker);
     bool TurretTargetAnimals(BaseNPC _target, AutoTurret _attacker);
     void adminMode(BasePlayer _player);
     bool isInAdminMode(BasePlayer _player);
     void OnItemRemovedFromContainer(ItemContainer container, Item item);
     void ModifyDamage(HitInfo hitinfo, float scale);
     string DateTimeStamp();
     double GetTimeStamp();
     int DebugLevel;
     void DebugMessage(int _minDebuglvl, string _msg);
     void OnRunPlayerMetabolism(PlayerMetabolism metabolism);
}

 class PlayerDataStorage
{
    public Hash<ulong, PlayerInfo> Info;
    public List<ulong> sleepers;
    public List<ulong> UnknownUser;
}

 class TicketDataStorage
{
    public Dictionary<int, ulong> Link;
    public Dictionary<ulong, Ticket> Info;
    public Dictionary<ulong, string> Notification;
}

 class TicketLogStorage
{
    public Dictionary<int, LogData> Log;
}

 class PlayerInfo
{
    public string username;
    public string FirstConnection;
    public string LatestConnection;
    public string mode;
    public bool ticket;
}

 class Ticket
{
    public int TicketNumber;
    public string Username;
    public string UserId;
    public string requested;
    public string reason;
    public string CreatedTimeStamp;
}

 class LogData
{
    public string UserId;
    public string AdminId;
    public string Username;
    public string AdminName;
    public string requested;
    public string reason;
    public bool Accepted;
    public string CreatedTimeStamp;
    public string ClosedTimeStamp;
}

 class QUI
{
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align, float fadein);
    static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, float fadein);
    static public void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
    static public void CreateTextOverlay(CuiElementContainer container, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
}


```

---

## QuarryFactory

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("QuarryFactory", "Masteroliw", "1.0.3" , ResourceId = 1376)]
[Description("Spawn items inside the quarry when it gathers resources")]
public class QuarryFactory : RustPlugin
{
     void OnQuarryGather(MiningQuarry quarry, Item item);
    public void callSpawn(string name, int randomnumber, MiningQuarry quarry);
    protected override void LoadDefaultConfig();
    [ChatCommand("help")]
    private void HelpCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("QF")]
    private void QF(BasePlayer player, string command, string[] args);
    public static string GetResourceTypes(string arg);
     bool HasAccess(BasePlayer player);
}


```

---

## QuarryHealth

```csharp
using System;

Oxide.Plugins
[Info("Quarry Health", "Waizujin", 1.0)]
[Description("Changes the health value of quarries.")]
public class QuarryHealth : RustPlugin
{
    public float quarryHealth { get; set; }
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     void OnEntityBuilt(Planner planner, UnityEngine.GameObject component);
    public void updateQuarries();
}


```

---

## QuarryLevels

```csharp
using UnityEngine;
using System.Linq;
using Oxide.Core.Plugins;
using System.Globalization;
using Oxide.Game.Rust.Cui;
using System.Collections.Generic;

Oxide.Plugins
[Info("QuarryLevels", "Death", "1.0.3")]
 class QuarryLevels : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
     Plugin Economics;
    const string perm;
     System.Random random;
     Dictionary<BasePlayer, MiningQuarry> ActiveGUI;
     void OnServerInitialized();
     void Unload();
     object CanLootEntity(BasePlayer player, ResourceExtractorFuelStorage quarry);
     void OnLootEntity(BasePlayer player, ResourceExtractorFuelStorage quarry);
     void OnLootEntityEnd(BasePlayer player, ResourceExtractorFuelStorage quarry);
     void OnEntityKill(MiningQuarry quarry);
     void OnQuarryToggled(MiningQuarry quarry, BasePlayer player);
     void OnQuarryConsumeFuel(MiningQuarry quarry, Item item);
     void OnEntitySpawned(MiningQuarry quarry);
     void OnResourceDepositCreatedWIP(ResourceDepositManager.ResourceDeposit resourceDeposit);
     bool HasFuel(MiningQuarry quarry);
     bool CanAfford(BasePlayer player, bool pumpjack);
     void DirectMessage(BasePlayer player, string message);
     void CreateGUI(BasePlayer player);
    [ConsoleCommand("ALSd01LASKDkaK2Qlasdka(1Kaklsdja2")]
     void CreateConfirmation(ConsoleSystem.Arg arg);
     void DestroyCUI(BasePlayer player);
    [ConsoleCommand("ql")]
     void ConfigCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("$19%(!*aslLKAK123(!@*AKJSK!(49128!(@#!@*#$%!")]
     void CloseConfirm(ConsoleSystem.Arg arg);
    [ConsoleCommand("gLx$_+!)@laKS4391LAKS1291@$(!RKQSMDIO!@@")]
     void Upgrade(ConsoleSystem.Arg arg);
     ConfigFile options;
     class ConfigFile
    {
        public SurveyConfig SurveySettings;
        public PlayerConfig PlayerSettings;
        public QuarryLevelOptions QuarrySettings;
        public QuarryProduction QuarryOptions;
        public ButtonConfig Button;
        public PanelConfig Panel;
    }

     class PlayerConfig
    {
        public bool PreventUnauthorizedToggling;
        public bool PreventUnauthorizedLooting;
    }

     class SurveyConfig
    {
        public bool EnableOilCraters;
        public int OilCraterChance;
    }

     class QuarryLevelOptions
    {
        public int QuarryMaxLevel;
        public int PumpjackMaxLevel;
        public bool EnableEconomics;
        public double EconomicsCost;
        public string EconomicsCurrency;
    }

     class QuarryProduction
    {
        public float Metal_Production;
        public float Sulfur_Production;
        public float HQM_Production;
    }

     class ButtonConfig
    {
        public UI4 ButtonBounds;
        public string ButtonColor;
        public float ButtonOpacity;
        public string ButtonFontColor;
    }

     class PanelConfig
    {
        public UI4 PanelBounds;
        public string PanelColor;
        public float PanelOpacity;
        public string PanelFontColor;
    }

     void LoadDefaultConfig();
     void LoadConfig();
     void SaveConfig(ConfigFile config);
    private string GetImage(string imageName, ulong skinid);
    public static class UI
    {
        static public CuiElementContainer Container(string panel, string color, UI4 dimensions, bool useCursor, string parent);
        static public void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions, bool cursor);
        static public void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
        static public void Label_Lower(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
        static public void Button(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, string command, TextAnchor align);
        static public void Image(CuiElementContainer container, string panel, string png, UI4 dimensions);
        public static string Color(string hexColor, float alpha);
    }

    public class UI4
    {
        public float xMin;
        public float yMin;
        public float xMax;
        public float yMax;
        public UI4(float xMin, float yMin, float xMax, float yMax);
        public string GetMin();
        public string GetMax();
    }

}

 class ConfigFile
{
    public SurveyConfig SurveySettings;
    public PlayerConfig PlayerSettings;
    public QuarryLevelOptions QuarrySettings;
    public QuarryProduction QuarryOptions;
    public ButtonConfig Button;
    public PanelConfig Panel;
}

 class PlayerConfig
{
    public bool PreventUnauthorizedToggling;
    public bool PreventUnauthorizedLooting;
}

 class SurveyConfig
{
    public bool EnableOilCraters;
    public int OilCraterChance;
}

 class QuarryLevelOptions
{
    public int QuarryMaxLevel;
    public int PumpjackMaxLevel;
    public bool EnableEconomics;
    public double EconomicsCost;
    public string EconomicsCurrency;
}

 class QuarryProduction
{
    public float Metal_Production;
    public float Sulfur_Production;
    public float HQM_Production;
}

 class ButtonConfig
{
    public UI4 ButtonBounds;
    public string ButtonColor;
    public float ButtonOpacity;
    public string ButtonFontColor;
}

 class PanelConfig
{
    public UI4 PanelBounds;
    public string PanelColor;
    public float PanelOpacity;
    public string PanelFontColor;
}

public static class UI
{
    static public CuiElementContainer Container(string panel, string color, UI4 dimensions, bool useCursor, string parent);
    static public void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions, bool cursor);
    static public void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
    static public void Label_Lower(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
    static public void Button(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, string command, TextAnchor align);
    static public void Image(CuiElementContainer container, string panel, string png, UI4 dimensions);
    public static string Color(string hexColor, float alpha);
}

public class UI4
{
    public float xMin;
    public float yMin;
    public float xMax;
    public float yMax;
    public UI4(float xMin, float yMin, float xMax, float yMax);
    public string GetMin();
    public string GetMax();
}


```

---

## QuarryLocks

```csharp
using System.Collections.Generic;
using System.Data;
using System.Linq;
using System;
using Oxide.Core.Plugins;
using Oxide.Core;

Oxide.Plugins
[Info("Quarry-Locks", "DylanSMR", "1.0.5", ResourceId = 1819)]
[Description("Added customizable locks to a quarry")]
 class QuarryLocks : RustPlugin
{
     void LoadDefaultConfig();
     class QuarryData
    {
        public Dictionary<ulong, ExtraData> QD;
        public QuarryData();
    }

     class MessageData
    {
        public Dictionary<ulong, ExtraMessage> MD;
        public MessageData();
    }

     class ExtraData
    {
        public string Name;
        public float NameID;
        public int Code;
        public bool CodeEnabled;
        public int MaxLogsAllowed;
        public int MaxLogsFromPlayer;
        public Dictionary<ulong, NewLogsFromP> LogsFromPlayer;
        public Dictionary<ulong, string> HasAccess;
        public Dictionary<ulong, string> PlayersBlocked;
        public ExtraData();
    }

     class NewLogsFromP
    {
        public int Logs;
        public NewLogsFromP();
    }

     class ExtraMessage
    {
        public string Name;
        public float NameID;
        public List<string> HasAccessed;
        public List<string> AttemptedAccess;
        public int Messages;
        public ExtraMessage();
    }

    public List<string> HelpIM;
    public List<string> HelpIM2;
    public bool WarningE;
     QuarryData quarryData;
     MessageData messageData;
     void SaveData();
     void Loaded();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnServerSave();
     void LoadLangauge();
    private string GetMessage(string name, string sid);
     void SendReplyHelp(BasePlayer player);
     void SendReplyHelp2(BasePlayer player);
    private object FindPlayer(string arg);
     void PlayerAlertMessages(BasePlayer player);
    [ChatCommand("qlock")]
     void cmdQLock(BasePlayer player, string command, string[] args);
    [HookMethod("OnLootEntity")]
     void OnLootEntity(BasePlayer looter, BaseEntity entry);
}

 class QuarryData
{
    public Dictionary<ulong, ExtraData> QD;
    public QuarryData();
}

 class MessageData
{
    public Dictionary<ulong, ExtraMessage> MD;
    public MessageData();
}

 class ExtraData
{
    public string Name;
    public float NameID;
    public int Code;
    public bool CodeEnabled;
    public int MaxLogsAllowed;
    public int MaxLogsFromPlayer;
    public Dictionary<ulong, NewLogsFromP> LogsFromPlayer;
    public Dictionary<ulong, string> HasAccess;
    public Dictionary<ulong, string> PlayersBlocked;
    public ExtraData();
}

 class NewLogsFromP
{
    public int Logs;
    public NewLogsFromP();
}

 class ExtraMessage
{
    public string Name;
    public float NameID;
    public List<string> HasAccessed;
    public List<string> AttemptedAccess;
    public int Messages;
    public ExtraMessage();
}


```

---

## Quests

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("Quests", "k1lly0u modet GRAZYCAT Fixed Kaprith Games on YT", "2.2.8", ResourceId = 1084)]
[Description("Creates quests for players to go on to earn rewards, complete with a GUI menu")]
public class Quests : RustPlugin
{
    [PluginReference]
     Plugin HumanNPC;
    [PluginReference]
     Plugin RustShop;
    [PluginReference]
     Plugin Economics;
    [PluginReference]
     Plugin LustyMap;
    [PluginReference]
     Plugin EventManager;
    [PluginReference]
     Plugin HuntRPG;
    [PluginReference]
     Plugin PlayerChallenges;
    [PluginReference]
     Plugin BetterChat;
    [PluginReference]
     Plugin KarmaSystem;
     ConfigData configData;
     QuestData questData;
     PlayerData playerData;
     NPCData vendors;
     ItemNames itemNames;
    private DynamicConfigFile Quest_Data;
    private DynamicConfigFile Player_Data;
    private DynamicConfigFile Quest_Vendors;
    private DynamicConfigFile Item_Names;
    private Dictionary<ulong, PlayerQuestData> PlayerProgress;
    private Dictionary<QuestType, Dictionary<string, QuestEntry>> Quest;
    private Dictionary<string, ItemDefinition> ItemDefs;
    private Dictionary<string, string> DisplayNames;
    private Dictionary<ulong, QuestCreator> ActiveCreations;
    private Dictionary<ulong, QuestCreator> ActiveEditors;
    private Dictionary<ulong, bool> AddVendor;
    private Dictionary<QuestType, List<string>> AllObjectives;
    private Dictionary<uint, Dictionary<ulong, int>> HeliAttackers;
    private Dictionary<ulong, List<string>> OpenUI;
    private Dictionary<uint, ulong> Looters;
    private List<ulong> StatsMenu;
    private List<ulong> OpenMenuBind;
    static string UIMain;
    static string UIPanel;
    static string UIEntry;
    private string textPrimary;
    private string textSecondary;
     class PlayerQuestData
    {
        public Dictionary<string, PlayerQuestInfo> Quests;
        public List<QuestInfo> RequiredItems;
        public ActiveDelivery CurrentDelivery;
    }

     class PlayerQuestInfo
    {
        public QuestStatus Status;
        public QuestType Type;
        public int AmountCollected;
        public bool RewardClaimed;
        public double ResetTime;
    }

     class QuestEntry
    {
        public string QuestName;
        public string Description;
        public string Objective;
        public string ObjectiveName;
        public int AmountRequired;
        public int Cooldown;
        public bool ItemDeduction;
        public List<RewardItem> Rewards;
    }

     class NPCInfo
    {
        public float x;
        public float z;
        public string ID;
        public string Name;
    }

     class DeliveryInfo
    {
        public string Description;
        public NPCInfo Info;
        public RewardItem Reward;
        public float Multiplier;
    }

     class ActiveDelivery
    {
        public string VendorID;
        public string TargetID;
        public float Distance;
    }

     class QuestInfo
    {
        public string ShortName;
        public QuestType Type;
    }

     class RewardItem
    {
        public bool isRP;
        public bool isKR;
        public bool isCoins;
        public bool isHuntXP;
        public string DisplayName;
        public string ShortName;
        public int ID;
        public float Amount;
        public bool BP;
        public ulong Skin;
    }

     class QuestCreator
    {
        public QuestType type;
        public QuestEntry entry;
        public DeliveryInfo deliveryInfo;
        public RewardItem item;
        public string oldEntry;
        public int partNum;
    }

     class ItemNames
    {
        public Dictionary<string, string> DisplayNames;
    }

     class QUI
    {
        public static bool disableFade;
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align, float fadein);
        static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, float fadein);
        static public void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
        static public void CreateTextOverlay(CuiElementContainer container, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
        static public string Color(string hexColor, float alpha);
    }

     void Loaded();
     void OnServerInitialized();
     void Unload();
     void OnPlayerInit(BasePlayer player);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnEntityTakeDamage(BaseCombatEntity victim, HitInfo info);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnGrowableGather(GrowableEntity plant, BasePlayer player, Item item);
     void OnCollectiblePickup(Item item, BasePlayer player, CollectibleEntity entity);
     void OnItemCraftFinished(ItemCraftTask task, Item item);
     void OnItemAddedToContainer(ItemContainer container, Item item);
     void OnItemRemovedFromContainer(ItemContainer container, Item item);
     void OnUseNPC(BasePlayer npc, BasePlayer player);
     object OnPlayerChat(ConsoleSystem.Arg arg);
     object OnBetterChat(Dictionary<string, object> dict);
     void QuestChat(BasePlayer player, string[] arg);
    private bool isPlaying(BasePlayer player);
    private void CloseMap(BasePlayer player);
    private void OpenMap(BasePlayer player);
    private void AddMapMarker(float x, float z, string name, string icon, float r);
    private void RemoveMapMarker(string name);
    private object CanTeleport(BasePlayer player);
    private void FillObjectiveList();
    private void GetAllItems();
    private void GetAllCraftables();
    private void GetAllResources();
    private void GetAllKillables();
     void AddMapIcons();
    private void ProcessProgress(BasePlayer player, QuestType questType, string type, int amount);
    private void TakeQuestItem(BasePlayer player, string item, int amount);
    private void CompleteQuest(BasePlayer player, string questName);
    private ItemDefinition FindItemDefinition(string shortname);
    private string GetRewardString(List<RewardItem> entry);
    private bool GiveReward(BasePlayer player, List<RewardItem> rewards);
    private void ReturnItems(BasePlayer player, string itemname, int amount);
    private RewardItem GetItem(BasePlayer player);
    private bool hasQuests(ulong player);
    private bool isQuestItem(ulong player, string name, QuestType type);
    private void CheckPlayerEntry(BasePlayer player);
    private object GetQuestType(string name);
    private QuestEntry GetQuest(string name);
    private void SaveQuest(BasePlayer player, bool isCreating);
    private void SaveRewardsEdit(BasePlayer player);
    private void ExitQuest(BasePlayer player, bool isCreating);
    private void RemoveQuest(string questName);
    private ulong GetLastAttacker(uint id);
    private string GetTypeDescription(QuestType type);
    private QuestType ConvertStringToType(string type);
    private string isNPCRegistered(string ID);
    static double GrabCurrentTime();
    private BasePlayer FindEntity(BasePlayer player);
    private object Ray(BasePlayer player, Vector3 Aim);
    private void SetVendorName();
    private void RemoveVendor(BasePlayer player, string ID, bool isVendor);
    private string GetRandomNPC(string ID);
    private string LA(string key, string userID);
    private void CreateMenu(BasePlayer player);
    private void CreateEmptyMenu(BasePlayer player);
    private void CreateMenuButton(CuiElementContainer container, string panelName, string buttonname, string command, int number);
    private void ListElement(BasePlayer player, QuestType type, int page);
    private void CreateQuestEntry(BasePlayer player, QuestEntry entry, int num);
    private void PlayerStats(BasePlayer player, int page);
    private void CreateStatEntry(BasePlayer player, QuestEntry entry, int num);
    private void PlayerDelivery(BasePlayer player);
    private void CreationMenu(BasePlayer player);
    private void CreationHelp(BasePlayer player, int page);
    private void CreateObjectiveMenu(BasePlayer player, int page);
    private void DeliveryHelp(BasePlayer player, int page);
    private void AcceptDelivery(BasePlayer player, string npcID, int page);
    private void DeletionEditMenu(BasePlayer player, string page, string command);
    private void DeleteNPCMenu(BasePlayer player);
    private void ConfirmDeletion(BasePlayer player, string questName);
    private void ConfirmCancellation(BasePlayer player, string questName);
    private void QuestEditorMenu(BasePlayer player);
    private void CreateObjectiveEntry(CuiElementContainer container, string panelName, string name, int number);
    private void CreateNewQuestButton(CuiElementContainer container, string panelName, string buttonname, string command, int number);
    private void CreateRewardTypeButton(CuiElementContainer container, string panelName, string buttonname, string command, int number);
    private void CreateDelEditButton(CuiElementContainer container, float xPos, string panelName, string buttonname, int number, string command, float width);
    private void CreateDelVendorButton(CuiElementContainer container, float xPos, string panelName, string buttonname, int number, string command);
    private void PopupMessage(BasePlayer player, string msg);
    private Vector2 CalcQuestPos(int number);
    private float[] CalcEntryPos(int number);
    private void AddUIString(BasePlayer player, string name);
    private void DestroyUI(BasePlayer player);
    private void DestroyEntries(BasePlayer player);
    [ConsoleCommand("QUI_AcceptQuest")]
    private void cmdAcceptQuest(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_AcceptDelivery")]
    private void cmdAcceptDelivery(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_CancelDelivery")]
    private void cmdCancelDelivery(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_FinishDelivery")]
    private void cmdFinishDelivery(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_ChangeElement")]
    private void cmdChangeElement(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_DestroyAll")]
    private void cmdDestroyAll(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_NewQuest")]
    private void cmdNewQuest(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_AddVendor")]
    private void cmdAddVendor(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_SelectObj")]
    private void cmdSelectObj(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_RewardType")]
    private void cmdRewardType(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_ClaimReward")]
    private void cmdClaimReward(ConsoleSystem.Arg arg);
     bool IsQuestCompleted(ulong playerId, string questName);
    [ConsoleCommand("QUI_CancelQuest")]
    private void cmdCancelQuest(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_ItemDeduction")]
    private void cmdItemDeduction(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_ConfirmCancel")]
    private void cmdConfirmCancel(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_RemoveCompleted")]
    private void cmdRemoveCompleted(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_DeleteQuest")]
    private void cmdDeleteQuest(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_DeleteNPCMenu")]
    private void cmdDeleteNPCMenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_RemoveVendor")]
    private void cmdRemoveVendor(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_ConfirmDelete")]
    private void cmdConfirmDelete(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_EditQuest")]
    private void cmdEditQuest(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_EditQuestVar")]
    private void cmdEditQuestVar(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_RemoveReward")]
    private void cmdEditReward(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_EndEditing")]
    private void cmdEndEditing(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_SaveQuest")]
    private void cmdSaveQuest(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_ExitQuest")]
    private void cmdExitQuest(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_AddReward")]
    private void cmdAddReward(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_RewardFinish")]
    private void cmdFinishReward(ConsoleSystem.Arg arg);
    [ConsoleCommand("QUI_OpenQuestMenu")]
    private void cmdOpenQuestMenu(ConsoleSystem.Arg arg);
    [ChatCommand("q")]
     void cmdOpenMenu(BasePlayer player, string command, string[] args);
    [ChatCommand("questnpc")]
     void cmdQuestNPC(BasePlayer player, string command, string[] args);
     void SaveQuestData();
     void SaveVendorData();
     void SavePlayerData();
     void SaveDisplayNames();
    private void SaveLoop();
     void LoadData();
     class QuestData
    {
        public Dictionary<QuestType, Dictionary<string, QuestEntry>> Quest;
    }

     class PlayerData
    {
        public Dictionary<ulong, PlayerQuestData> PlayerProgress;
    }

     class NPCData
    {
        public Dictionary<string, NPCInfo> QuestVendors;
        public Dictionary<string, DeliveryInfo> DeliveryVendors;
    }

     class UIColor
    {
        public string Color { get; set; }
        public float Alpha { get; set; }
    }

     class Colors
    {
        public string TextColor_Primary { get; set; }
        public string TextColor_Secondary { get; set; }
        public UIColor Background_Dark { get; set; }
        public UIColor Background_Light { get; set; }
        public UIColor Button_Standard { get; set; }
        public UIColor Button_Accept { get; set; }
        public UIColor Button_Completed { get; set; }
        public UIColor Button_Cancel { get; set; }
        public UIColor Button_Pending { get; set; }
    }

     class Keybinds
    {
        public bool Autoset_KeyBind { get; set; }
        public string KeyBind_Key { get; set; }
    }

     class LMIcons
    {
        public string Icon_Vendor { get; set; }
        public string Icon_Delivery { get; set; }
    }

     class ConfigData
    {
        public Colors Colors { get; set; }
        public Keybinds KeybindOptions { get; set; }
        public LMIcons LustyMapIntegration { get; set; }
        public bool DisableUI_FadeIn { get; set; }
        public bool UseNPCVendors { get; set; }
    }

    private void LoadVariables();
    private void LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConfig(ConfigData config);
     void SendMSG(BasePlayer player, string message, string keyword);
     Dictionary<string, string> Localization;
}

 class PlayerQuestData
{
    public Dictionary<string, PlayerQuestInfo> Quests;
    public List<QuestInfo> RequiredItems;
    public ActiveDelivery CurrentDelivery;
}

 class PlayerQuestInfo
{
    public QuestStatus Status;
    public QuestType Type;
    public int AmountCollected;
    public bool RewardClaimed;
    public double ResetTime;
}

 class QuestEntry
{
    public string QuestName;
    public string Description;
    public string Objective;
    public string ObjectiveName;
    public int AmountRequired;
    public int Cooldown;
    public bool ItemDeduction;
    public List<RewardItem> Rewards;
}

 class NPCInfo
{
    public float x;
    public float z;
    public string ID;
    public string Name;
}

 class DeliveryInfo
{
    public string Description;
    public NPCInfo Info;
    public RewardItem Reward;
    public float Multiplier;
}

 class ActiveDelivery
{
    public string VendorID;
    public string TargetID;
    public float Distance;
}

 class QuestInfo
{
    public string ShortName;
    public QuestType Type;
}

 class RewardItem
{
    public bool isRP;
    public bool isKR;
    public bool isCoins;
    public bool isHuntXP;
    public string DisplayName;
    public string ShortName;
    public int ID;
    public float Amount;
    public bool BP;
    public ulong Skin;
}

 class QuestCreator
{
    public QuestType type;
    public QuestEntry entry;
    public DeliveryInfo deliveryInfo;
    public RewardItem item;
    public string oldEntry;
    public int partNum;
}

 class ItemNames
{
    public Dictionary<string, string> DisplayNames;
}

 class QUI
{
    public static bool disableFade;
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align, float fadein);
    static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, float fadein);
    static public void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
    static public void CreateTextOverlay(CuiElementContainer container, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
    static public string Color(string hexColor, float alpha);
}

 class QuestData
{
    public Dictionary<QuestType, Dictionary<string, QuestEntry>> Quest;
}

 class PlayerData
{
    public Dictionary<ulong, PlayerQuestData> PlayerProgress;
}

 class NPCData
{
    public Dictionary<string, NPCInfo> QuestVendors;
    public Dictionary<string, DeliveryInfo> DeliveryVendors;
}

 class UIColor
{
    public string Color { get; set; }
    public float Alpha { get; set; }
}

 class Colors
{
    public string TextColor_Primary { get; set; }
    public string TextColor_Secondary { get; set; }
    public UIColor Background_Dark { get; set; }
    public UIColor Background_Light { get; set; }
    public UIColor Button_Standard { get; set; }
    public UIColor Button_Accept { get; set; }
    public UIColor Button_Completed { get; set; }
    public UIColor Button_Cancel { get; set; }
    public UIColor Button_Pending { get; set; }
}

 class Keybinds
{
    public bool Autoset_KeyBind { get; set; }
    public string KeyBind_Key { get; set; }
}

 class LMIcons
{
    public string Icon_Vendor { get; set; }
    public string Icon_Delivery { get; set; }
}

 class ConfigData
{
    public Colors Colors { get; set; }
    public Keybinds KeybindOptions { get; set; }
    public LMIcons LustyMapIntegration { get; set; }
    public bool DisableUI_FadeIn { get; set; }
    public bool UseNPCVendors { get; set; }
}


```

---

## QuestSystem

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Plugins.SignArtistClasses;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Drawing;
using System.Drawing.Imaging;
using System.IO;
using System.Linq;
using UnityEngine;
using Oxide.Core.Plugins;
using Color = UnityEngine.Color;
using System.Globalization;
using Oxide.Game.Rust.Cui;
using ProtoBuf;
using ConVar;
using Oxide.Core.Libraries;
using VLB;

Oxide.Plugins
[Info("QuestSystem", "http://topplugin.ru/ DezLife - НУ ты и лошара", "2.0.7")]
public class QuestSystem : RustPlugin
{
    public static QuestSystem instance;
    [PluginReference]
     Plugin IQChat;
     Plugin RustMap;
    private class Quest
    {
        internal class Prize
        {
            [JsonProperty("Это кастом предмет ? (Если это обычный предмет ставим false)")]
            public bool CustomItem;
            [JsonProperty("Отображаемое имя (Для кастом)")]
            public string DisplayName;
            [JsonProperty("Skin id (Для кастом но можно и для обычного предмета чтоб дать ему скин)")]
            public ulong Skin;
            [JsonProperty("Shortname выдаваемого предмета")]
            public string Shortname;
            [JsonProperty("количество")]
            public int Amount;
            [JsonProperty("Ссылка на картинку (Если используете команду для выдачи то обязательно!)")]
            public string ExternalURL;
            [JsonProperty("Команда")]
            public string Command;
        }

        [JsonProperty("Названия квеста")]
        public string DisplayName;
        [JsonProperty("Описания")]
        public string Description;
        [JsonProperty("Тип квеста (0 - убить, 1 - добыть, 2 - скрафтить, 3 - найти, 4 - улучшить)")]
        public QuestType QuestType;
        [JsonProperty("То чего добыть (На русском)")]
        public string NiceTarget;
        [JsonProperty("В зависимости от квеста. Если убить человека то player. Если добыть дерева то wood; Если это улучшения то 1 - дерево, 2 - камень и тд")]
        public string Target;
        [JsonProperty("Количевство")]
        public int Amount;
        [JsonProperty("Настройка награды")]
        public List<Prize> PrizeList;
        public List<ulong> FinishedPlayers;
    }

    private class PlayerQuest
    {
        public Quest parentQuest;
        public ulong UserID;
        public bool Finished;
        public int Count;
        public void AddCount(int amount);
        public int LeftAmount();
    }

    protected override void LoadDefaultMessages();
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Можно ли повторно брать квесты ?")]
        public bool QuestGo;
        [JsonProperty("Картинка для доски заданий (Желательный размер 256 на 128)")]
        public string ImageURL;
        [JsonProperty("Максимум квестов которых игрок сможет взять за раз")]
        public int MaxQuestAmount;
        [JsonProperty("Оповестить игрока звуковым оповещением о том что он выполнил квест?")]
        public bool SoundEff;
        [JsonProperty("Эфект для проигрования")]
        public string SoundEffPath;
        [JsonProperty("Делать маркер на карте j?")]
        public bool MapMarkers;
        [JsonProperty("Сбрасывать прогрес квестов у игроков во время вайпа:?")]
        public bool WipeData;
        [JsonProperty("Включить прогресс бар для игроков ?")]
        public bool ProgressBar;
        [JsonProperty("SteamWebApiKey (Получить можно тут https://steamcommunity.com/dev/apikey)")]
        public string SteamWebApiKey;
        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [PluginReference]
    private Plugin ImageLibrary;
    private BaseEntity Sign;
    private List<Quest> QuestList;
    private Dictionary<ulong, List<PlayerQuest>> PlayerQuests;
    private Dictionary<string, List<ulong>> PlayerQuestsFinish;
    private void CreateMarker(Vector3 position, float refreshRate, string name, string displayName, float radius, string colorMarker, string colorOutline);
    private void RemoveMarkers();
    private const string genericPrefab;
    private const string vendingPrefab;
    private class CustomMapMarker : MonoBehaviour
    {
        private VendingMachineMapMarker vending;
        private MapMarkerGenericRadius generic;
        public BaseEntity parent;
        private bool asChild;
        public float radius;
        public Color color1;
        public Color color2;
        public string displayName;
        public float refreshRate;
        public Vector3 position;
        public bool placedByPlayer;
        private void Start();
        private void CreateMarkers();
        private void UpdatePosition();
        private void UpdateMarkers();
        private void DestroyMakers();
        private void OnDestroy();
    }

    private void OnServerInitialized();
    private bool TryPlaceSign();
     void OnNewSave(string filename);
    private void OnPlayerInput(BasePlayer player, InputState input);
    private void Unload();
    private void OnPlayerInit(BasePlayer player);
    private void CanAffordUpgrade(BasePlayer player, BuildingBlock block, BuildingGrade.Enum grade);
    private Dictionary<uint, Dictionary<ulong, int>> HeliAttackers;
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnEntityTakeDamage(BaseCombatEntity victim, HitInfo info);
    private ulong GetLastAttacker(uint id);
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnDispenserBonus(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnCollectiblePickup(Item item, BasePlayer player);
     void OnItemCraftFinished(ItemCraftTask task, Item item);
    [ChatCommand("quest")]
    private void CmdChatQuest(BasePlayer player, string command, string[] args);
    [ConsoleCommand("UI_Handler")]
    private void CmdConsoleHandler(ConsoleSystem.Arg args);
    private const string Layer;
    private void UI_DrawInterface(BasePlayer player, int page, string questUpdate);
    public void SendChat(string Descrip, string Message, BasePlayer player, Chat.ChatChannel channel);
     void RunEffect(BasePlayer player, string path);
    private bool IsEntityPlayer(BaseEntity entity, BasePlayer result);
    private void LoadData(string name, T data, bool enableSaving);
    private void SaveData(string name, T data, bool autoSave);
    private static string HexToRustFormat(string hex);
    private IEnumerator DownloadImage(string url, Signage sign);
    private class SteampoweredResult
    {
        public Response response;
        public class Response
        {
            [JsonProperty("result")]
            public int result;
            [JsonProperty("resultcount")]
            public int resultcount;
            [JsonProperty("publishedfiledetails")]
            public List<PublishedFiled> publishedfiledetails;
            public class PublishedFiled
            {
                [JsonProperty("publishedfileid")]
                public ulong publishedfileid;
                [JsonProperty("result")]
                public int result;
                [JsonProperty("creator")]
                public string creator;
                [JsonProperty("creator_app_id")]
                public int creator_app_id;
                [JsonProperty("consumer_app_id")]
                public int consumer_app_id;
                [JsonProperty("filename")]
                public string filename;
                [JsonProperty("file_size")]
                public int file_size;
                [JsonProperty("preview_url")]
                public string preview_url;
                [JsonProperty("hcontent_preview")]
                public string hcontent_preview;
                [JsonProperty("title")]
                public string title;
                [JsonProperty("description")]
                public string description;
                [JsonProperty("time_created")]
                public int time_created;
                [JsonProperty("time_updated")]
                public int time_updated;
                [JsonProperty("visibility")]
                public int visibility;
                [JsonProperty("banned")]
                public int banned;
                [JsonProperty("ban_reason")]
                public string ban_reason;
                [JsonProperty("subscriptions")]
                public int subscriptions;
                [JsonProperty("favorited")]
                public int favorited;
                [JsonProperty("lifetime_subscriptions")]
                public int lifetime_subscriptions;
                [JsonProperty("lifetime_favorited")]
                public int lifetime_favorited;
                [JsonProperty("views")]
                public int views;
                [JsonProperty("tags")]
                public List<Tag> tags;
                public class Tag
                {
                    [JsonProperty("tag")]
                    public string tag;
                }

            }

        }

    }

    public string GetItemImage(string shortname, ulong skinID);
}

private class Quest
{
    internal class Prize
    {
        [JsonProperty("Это кастом предмет ? (Если это обычный предмет ставим false)")]
        public bool CustomItem;
        [JsonProperty("Отображаемое имя (Для кастом)")]
        public string DisplayName;
        [JsonProperty("Skin id (Для кастом но можно и для обычного предмета чтоб дать ему скин)")]
        public ulong Skin;
        [JsonProperty("Shortname выдаваемого предмета")]
        public string Shortname;
        [JsonProperty("количество")]
        public int Amount;
        [JsonProperty("Ссылка на картинку (Если используете команду для выдачи то обязательно!)")]
        public string ExternalURL;
        [JsonProperty("Команда")]
        public string Command;
    }

    [JsonProperty("Названия квеста")]
    public string DisplayName;
    [JsonProperty("Описания")]
    public string Description;
    [JsonProperty("Тип квеста (0 - убить, 1 - добыть, 2 - скрафтить, 3 - найти, 4 - улучшить)")]
    public QuestType QuestType;
    [JsonProperty("То чего добыть (На русском)")]
    public string NiceTarget;
    [JsonProperty("В зависимости от квеста. Если убить человека то player. Если добыть дерева то wood; Если это улучшения то 1 - дерево, 2 - камень и тд")]
    public string Target;
    [JsonProperty("Количевство")]
    public int Amount;
    [JsonProperty("Настройка награды")]
    public List<Prize> PrizeList;
    public List<ulong> FinishedPlayers;
}

internal class Prize
{
    [JsonProperty("Это кастом предмет ? (Если это обычный предмет ставим false)")]
    public bool CustomItem;
    [JsonProperty("Отображаемое имя (Для кастом)")]
    public string DisplayName;
    [JsonProperty("Skin id (Для кастом но можно и для обычного предмета чтоб дать ему скин)")]
    public ulong Skin;
    [JsonProperty("Shortname выдаваемого предмета")]
    public string Shortname;
    [JsonProperty("количество")]
    public int Amount;
    [JsonProperty("Ссылка на картинку (Если используете команду для выдачи то обязательно!)")]
    public string ExternalURL;
    [JsonProperty("Команда")]
    public string Command;
}

private class PlayerQuest
{
    public Quest parentQuest;
    public ulong UserID;
    public bool Finished;
    public int Count;
    public void AddCount(int amount);
    public int LeftAmount();
}

private class Configuration
{
    [JsonProperty("Можно ли повторно брать квесты ?")]
    public bool QuestGo;
    [JsonProperty("Картинка для доски заданий (Желательный размер 256 на 128)")]
    public string ImageURL;
    [JsonProperty("Максимум квестов которых игрок сможет взять за раз")]
    public int MaxQuestAmount;
    [JsonProperty("Оповестить игрока звуковым оповещением о том что он выполнил квест?")]
    public bool SoundEff;
    [JsonProperty("Эфект для проигрования")]
    public string SoundEffPath;
    [JsonProperty("Делать маркер на карте j?")]
    public bool MapMarkers;
    [JsonProperty("Сбрасывать прогрес квестов у игроков во время вайпа:?")]
    public bool WipeData;
    [JsonProperty("Включить прогресс бар для игроков ?")]
    public bool ProgressBar;
    [JsonProperty("SteamWebApiKey (Получить можно тут https://steamcommunity.com/dev/apikey)")]
    public string SteamWebApiKey;
    public static Configuration GetNewConfiguration();
}

private class CustomMapMarker : MonoBehaviour
{
    private VendingMachineMapMarker vending;
    private MapMarkerGenericRadius generic;
    public BaseEntity parent;
    private bool asChild;
    public float radius;
    public Color color1;
    public Color color2;
    public string displayName;
    public float refreshRate;
    public Vector3 position;
    public bool placedByPlayer;
    private void Start();
    private void CreateMarkers();
    private void UpdatePosition();
    private void UpdateMarkers();
    private void DestroyMakers();
    private void OnDestroy();
}

private class SteampoweredResult
{
    public Response response;
    public class Response
    {
        [JsonProperty("result")]
        public int result;
        [JsonProperty("resultcount")]
        public int resultcount;
        [JsonProperty("publishedfiledetails")]
        public List<PublishedFiled> publishedfiledetails;
        public class PublishedFiled
        {
            [JsonProperty("publishedfileid")]
            public ulong publishedfileid;
            [JsonProperty("result")]
            public int result;
            [JsonProperty("creator")]
            public string creator;
            [JsonProperty("creator_app_id")]
            public int creator_app_id;
            [JsonProperty("consumer_app_id")]
            public int consumer_app_id;
            [JsonProperty("filename")]
            public string filename;
            [JsonProperty("file_size")]
            public int file_size;
            [JsonProperty("preview_url")]
            public string preview_url;
            [JsonProperty("hcontent_preview")]
            public string hcontent_preview;
            [JsonProperty("title")]
            public string title;
            [JsonProperty("description")]
            public string description;
            [JsonProperty("time_created")]
            public int time_created;
            [JsonProperty("time_updated")]
            public int time_updated;
            [JsonProperty("visibility")]
            public int visibility;
            [JsonProperty("banned")]
            public int banned;
            [JsonProperty("ban_reason")]
            public string ban_reason;
            [JsonProperty("subscriptions")]
            public int subscriptions;
            [JsonProperty("favorited")]
            public int favorited;
            [JsonProperty("lifetime_subscriptions")]
            public int lifetime_subscriptions;
            [JsonProperty("lifetime_favorited")]
            public int lifetime_favorited;
            [JsonProperty("views")]
            public int views;
            [JsonProperty("tags")]
            public List<Tag> tags;
            public class Tag
            {
                [JsonProperty("tag")]
                public string tag;
            }

        }

    }

}

public class Response
{
    [JsonProperty("result")]
    public int result;
    [JsonProperty("resultcount")]
    public int resultcount;
    [JsonProperty("publishedfiledetails")]
    public List<PublishedFiled> publishedfiledetails;
    public class PublishedFiled
    {
        [JsonProperty("publishedfileid")]
        public ulong publishedfileid;
        [JsonProperty("result")]
        public int result;
        [JsonProperty("creator")]
        public string creator;
        [JsonProperty("creator_app_id")]
        public int creator_app_id;
        [JsonProperty("consumer_app_id")]
        public int consumer_app_id;
        [JsonProperty("filename")]
        public string filename;
        [JsonProperty("file_size")]
        public int file_size;
        [JsonProperty("preview_url")]
        public string preview_url;
        [JsonProperty("hcontent_preview")]
        public string hcontent_preview;
        [JsonProperty("title")]
        public string title;
        [JsonProperty("description")]
        public string description;
        [JsonProperty("time_created")]
        public int time_created;
        [JsonProperty("time_updated")]
        public int time_updated;
        [JsonProperty("visibility")]
        public int visibility;
        [JsonProperty("banned")]
        public int banned;
        [JsonProperty("ban_reason")]
        public string ban_reason;
        [JsonProperty("subscriptions")]
        public int subscriptions;
        [JsonProperty("favorited")]
        public int favorited;
        [JsonProperty("lifetime_subscriptions")]
        public int lifetime_subscriptions;
        [JsonProperty("lifetime_favorited")]
        public int lifetime_favorited;
        [JsonProperty("views")]
        public int views;
        [JsonProperty("tags")]
        public List<Tag> tags;
        public class Tag
        {
            [JsonProperty("tag")]
            public string tag;
        }

    }

}

public class PublishedFiled
{
    [JsonProperty("publishedfileid")]
    public ulong publishedfileid;
    [JsonProperty("result")]
    public int result;
    [JsonProperty("creator")]
    public string creator;
    [JsonProperty("creator_app_id")]
    public int creator_app_id;
    [JsonProperty("consumer_app_id")]
    public int consumer_app_id;
    [JsonProperty("filename")]
    public string filename;
    [JsonProperty("file_size")]
    public int file_size;
    [JsonProperty("preview_url")]
    public string preview_url;
    [JsonProperty("hcontent_preview")]
    public string hcontent_preview;
    [JsonProperty("title")]
    public string title;
    [JsonProperty("description")]
    public string description;
    [JsonProperty("time_created")]
    public int time_created;
    [JsonProperty("time_updated")]
    public int time_updated;
    [JsonProperty("visibility")]
    public int visibility;
    [JsonProperty("banned")]
    public int banned;
    [JsonProperty("ban_reason")]
    public string ban_reason;
    [JsonProperty("subscriptions")]
    public int subscriptions;
    [JsonProperty("favorited")]
    public int favorited;
    [JsonProperty("lifetime_subscriptions")]
    public int lifetime_subscriptions;
    [JsonProperty("lifetime_favorited")]
    public int lifetime_favorited;
    [JsonProperty("views")]
    public int views;
    [JsonProperty("tags")]
    public List<Tag> tags;
    public class Tag
    {
        [JsonProperty("tag")]
        public string tag;
    }

}

public class Tag
{
    [JsonProperty("tag")]
    public string tag;
}

public static class Extensions
{
    public static byte[] ResizeImage(byte[] bytes, int width, int height, int targetWidth, int targetHeight, bool enforceJpeg);
    public static string EscapeForUrl(string stringToEscape);
}

SignArtistClasses
public static class Extensions
{
    public static byte[] ResizeImage(byte[] bytes, int width, int height, int targetWidth, int targetHeight, bool enforceJpeg);
    public static string EscapeForUrl(string stringToEscape);
}


```

---

## QuickSmelt

```csharp
using System;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("QuickSmelt", "Wulf/lukespragg", "1.3.0", ResourceId = 1067)]
[Description("Increases the speed of the furnace smelting")]
 class QuickSmelt : RustPlugin
{
    const string permAllow;
     float byproductModifier;
     int byproductPercent;
     float cookedModifier;
     int cookedPercent;
     int fuelUsageModifier;
     bool overcookMeat;
     bool usePermissions;
    protected override void LoadDefaultConfig();
     void Init();
     void OnServerInitialized();
     void Unload();
     void OnConsumeFuel(BaseOven oven, Item fuel, ItemModBurnable burnable);
     int TakeFromInventorySlot(ItemContainer container, int itemId, int amount, int slot);
     T GetConfig(string name, T value);
}


```

---

## RadHouse

```csharp
using Facepunch;
using Network;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("RadHouse", "123", "1.2.1")]
 class RadHouse : RustPlugin
{
    static RadHouse instance;
    [PluginReference]
     Plugin RandomSpawns;
    [PluginReference]
     Plugin RustMap;
    [PluginReference]
     Plugin Map;
    [PluginReference]
     Plugin LustyMap;
    static RadHouse ins;
    private List<ZoneList> RadiationZones;
    private static readonly int playerLayer;
    private static readonly Collider[] colBuffer;
    private ZoneList RadHouseZone;
    private BaseEntity LootBox;
    public List<BaseEntity> BaseEntityList;
    public List<ulong> PlayerAuth;
    private DateTime DateOfWipe;
    private string DateOfWipeStr;
    public bool CanLoot;
    public bool NowLooted;
    public Timer mytimer;
    public Timer mytimer2;
    public Timer mytimer3;
    public Timer mytimer4;
    public Timer mytimer5;
    public int timercallbackdelay;
    public class Amount
    {
        public object ShortName;
        public object Min;
        public object Max;
    }

    public class DataStorage
    {
        public Dictionary<string, Amount>[] Common;
        public Dictionary<string, Amount>[] Rare;
        public Dictionary<string, Amount>[] Top;
        public Dictionary<string, float>[] RadiationRadius;
        public Dictionary<string, float>[] RadiationIntensity;
        public DataStorage();
    }

     DataStorage data;
    private DynamicConfigFile RadData;
    public bool GuiOn;
    public string AnchorMinCfg;
    public string AnchorMaxCfg;
    public string ColorCfg;
    public string TextGUI;
    public bool RadiationTrue;
    public string ChatPrefix;
    public int TimerSpawnHouse;
    public int TimerDestroyHouse;
    public int TimerLoot;
    public int TimeToRemove;
    public int GradeNum;
    public int MinPlayers;
    public bool NativeMap;
    public float NativeMapAlpha;
    public string NativeMapColor;
    public int NativeMapRadius;
    public bool EnabledNPC;
    public int AmountNPC;
    public bool LootNPC;
    protected override void LoadDefaultConfig();
    private void LoadConfigValues();
    private void GetConfig(string menu, string Key, T var);
     void OnServerInitialized();
     void LoadData();
     void Unload();
     void OnNewSave(string filename);
    public object success;
    [ConsoleCommand("rh")]
     void CreateRadHouseConsoleCommand(ConsoleSystem.Arg arg);
    [ChatCommand("rh")]
     void CreateRadHouseCommand(BasePlayer player, string cmd, string[] Args);
    private void OnServerRadiation();
     Vector3 RadPosition;
     void CreateRadHouse(bool IsAdminCreate);
    private void CreateNps(Vector3 position, int amount);
    private void AddMapMarker();
    private void RemoveMapMarker();
    private MapMarkerGenericRadius mapMarker;
    const string markerEnt;
    private Color ConvertToColor(string color);
    private void CreatePrivateMap(Vector3 pos);
     void DestroyRadHouse();
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void CreateLoot(StorageContainer Container, BaseEntity Box);
     void CanLootEntity(BasePlayer player, StorageContainer container);
     void OnEntitySpawned(BaseEntity entity);
     void RemoveEntity(BaseEntity entity);
     bool IsRadZone(Vector3 pos);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnLootEntityEnd(BasePlayer player, BaseCombatEntity entity);
     object OnCupboardAuthorize(BuildingPrivlidge privilege, BasePlayer player);
     SpawnFilter filter;
     List<Vector3> monuments;
    static float GetGroundPosition(Vector3 pos);
    public Vector3 RandomDropPosition();
     List<int> BlockedLayers;
    static int blockedMask;
    public Vector3 GetSafeDropPosition(Vector3 position);
    public Vector3 GetEventPosition();
     Vector3 RandomCircle(Vector3 center, float radius);
     void OnPlayerSleepEnded(BasePlayer player);
     void CreateGui(BasePlayer player);
     void DestroyGui(BasePlayer player);
    private void InitializeZone(Vector3 Location, float intensity, float radius, int ZoneID);
    private void DestroyZone(ZoneList zone);
    public class ZoneList
    {
        public RadZones zone;
    }

    public class RadZones : MonoBehaviour
    {
        private int ID;
        private Vector3 Position;
        private float ZoneRadius;
        private float RadiationAmount;
        private List<BasePlayer> InZone;
        private void Awake();
        public void Activate(Vector3 pos, float radius, float amount, int ZoneID);
        private void OnDestroy();
        private void UpdateCollider();
        private void UpdateTrigger();
    }

}

public class Amount
{
    public object ShortName;
    public object Min;
    public object Max;
}

public class DataStorage
{
    public Dictionary<string, Amount>[] Common;
    public Dictionary<string, Amount>[] Rare;
    public Dictionary<string, Amount>[] Top;
    public Dictionary<string, float>[] RadiationRadius;
    public Dictionary<string, float>[] RadiationIntensity;
    public DataStorage();
}

public class ZoneList
{
    public RadZones zone;
}

public class RadZones : MonoBehaviour
{
    private int ID;
    private Vector3 Position;
    private float ZoneRadius;
    private float RadiationAmount;
    private List<BasePlayer> InZone;
    private void Awake();
    public void Activate(Vector3 pos, float radius, float amount, int ZoneID);
    private void OnDestroy();
    private void UpdateCollider();
    private void UpdateTrigger();
}


```

---

## RadHouse120

```csharp
using Facepunch;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("RadHouse120", "RustPlugin.ru", "1.2.0")]
 class RadHouse120 : RustPlugin
{
    [PluginReference]
     Plugin RandomSpawns;
    [PluginReference]
     Plugin RustMap;
    [PluginReference]
     Plugin Map;
    [PluginReference]
     Plugin LustyMap;
    static RadHouse120 ins;
    private List<ZoneList> RadiationZones;
    private static readonly int playerLayer;
    private static readonly Collider[] colBuffer;
    private ZoneList RadHouseZone;
    private BaseEntity LootBox;
    public List<BaseEntity> BaseEntityList;
    public List<ulong> PlayerAuth;
    private DateTime DateOfWipe;
    private string DateOfWipeStr;
    public bool CanLoot;
    public bool NowLooted;
    public Timer mytimer;
    public Timer mytimer2;
    public Timer mytimer3;
    public Timer mytimer4;
    public Timer mytimer5;
    public int timercallbackdelay;
    public class Amount
    {
        public object ShortName;
        public object Min;
        public object Max;
    }

    public class DataStorage
    {
        public Dictionary<string, Amount>[] Common;
        public Dictionary<string, Amount>[] Rare;
        public Dictionary<string, Amount>[] Top;
        public Dictionary<string, float>[] RadiationRadius;
        public Dictionary<string, float>[] RadiationIntensity;
        public DataStorage();
    }

     DataStorage data;
    private DynamicConfigFile RadData;
    public bool GuiOn;
    public string AnchorMinCfg;
    public string AnchorMaxCfg;
    public string ColorCfg;
    public string TextGUI;
    public bool RadiationTrue;
    public string ChatPrefix;
    public int TimerSpawnHouse;
    public int TimerDestroyHouse;
    public int TimerLoot;
    public int TimeToRemove;
    public int GradeNum;
    public int MinPlayers;
    public bool EnabledNPC;
    public int AmountNPC;
    public bool LootNPC;
    protected override void LoadDefaultConfig();
    private void LoadConfigValues();
    private void GetConfig(string menu, string Key, T var);
     void OnServerInitialized();
     void LoadData();
     void Unload();
     void OnNewSave(string filename);
    public object success;
    [ConsoleCommand("rh")]
     void CreateRadHouseConsoleCommand(ConsoleSystem.Arg arg);
    [ChatCommand("rh")]
     void CreateRadHouseCommand(BasePlayer player, string cmd, string[] Args);
    private void OnServerRadiation();
     Vector3 RadPosition;
     void CreateRadHouse(bool IsAdminCreate);
    private void CreateNps(Vector3 position, int amount);
    private void AddMapMarker();
    private void RemoveMapMarker();
     void DestroyRadHouse();
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void CreateLoot(StorageContainer Container, BaseEntity Box);
     void CanLootEntity(BasePlayer player, StorageContainer container);
     void OnEntitySpawned(BaseEntity entity);
     void RemoveEntity(BaseEntity entity);
     bool IsRadZone(Vector3 pos);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnLootEntityEnd(BasePlayer player, BaseCombatEntity entity);
     object OnCupboardAuthorize(BuildingPrivlidge privilege, BasePlayer player);
     SpawnFilter filter;
     List<Vector3> monuments;
    static float GetGroundPosition(Vector3 pos);
    public Vector3 RandomDropPosition();
     List<int> BlockedLayers;
    static int blockedMask;
    public Vector3 GetSafeDropPosition(Vector3 position);
    public Vector3 GetEventPosition();
     Vector3 RandomCircle(Vector3 center, float radius);
     void OnPlayerSleepEnded(BasePlayer player);
     void CreateGui(BasePlayer player);
     void DestroyGui(BasePlayer player);
    private void InitializeZone(Vector3 Location, float intensity, float radius, int ZoneID);
    private void DestroyZone(ZoneList zone);
    public class ZoneList
    {
        public RadZones zone;
    }

    public class RadZones : MonoBehaviour
    {
        private int ID;
        private Vector3 Position;
        private float ZoneRadius;
        private float RadiationAmount;
        private List<BasePlayer> InZone;
        private void Awake();
        public void Activate(Vector3 pos, float radius, float amount, int ZoneID);
        private void OnDestroy();
        private void UpdateCollider();
        private void UpdateTrigger();
    }

}

public class Amount
{
    public object ShortName;
    public object Min;
    public object Max;
}

public class DataStorage
{
    public Dictionary<string, Amount>[] Common;
    public Dictionary<string, Amount>[] Rare;
    public Dictionary<string, Amount>[] Top;
    public Dictionary<string, float>[] RadiationRadius;
    public Dictionary<string, float>[] RadiationIntensity;
    public DataStorage();
}

public class ZoneList
{
    public RadZones zone;
}

public class RadZones : MonoBehaviour
{
    private int ID;
    private Vector3 Position;
    private float ZoneRadius;
    private float RadiationAmount;
    private List<BasePlayer> InZone;
    private void Awake();
    public void Activate(Vector3 pos, float radius, float amount, int ZoneID);
    private void OnDestroy();
    private void UpdateCollider();
    private void UpdateTrigger();
}


```

---

## RadLine

```csharp
using System.Text.RegularExpressions;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using System;

Oxide.Plugins
[Info("RAD-Line", "SkinN", "3.0.1", ResourceId = 914)]
[Description("Enables and disables radiation every X minutes")]
 class RadLine : RustPlugin
{
    private readonly bool Dev;
    private string Prefix;
    private string IconProfile;
    private bool EnableIconProfile;
    private bool BroadcastToConsole;
    private bool EnablePluginPrefix;
    private int EnabledInterval;
    private int DisabledInterval;
    private DateTime LastTimer;
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private T GetConfig(object[] args);
    private void LoadMessages();
    private string GetMsg(string key, object uid);
    private void Con(string msg);
    private void Say(string msg, string profile, bool prefix);
    private void Tell(BasePlayer player, string msg, string profile, bool prefix);
    public string SimpleColorFormat(string text, bool removeTags);
     void Init();
    private void Loop(bool force);
    [ChatCommand("rad")]
     void Rad_Command(BasePlayer player, string command, string[] args);
    [ChatCommand("radline")]
     void Plugin_Command(BasePlayer player, string command, string[] args);
}


```

---

## RadShrinkZone

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using System.Text.RegularExpressions;

Oxide.Plugins
[Info("RadShrinkZone", "vaalberith", "1.0.4", ResourceId = 1828)]
 class RadShrinkZone : RustPlugin
{
     Vector3 target;
     float saferadius;
     float saferadiusmin;
     float eventradius;
     float radpower;
     float step;
     float period;
     string permissionrad;
     bool breaking;
     bool ok;
     List<Vector3> position;
    readonly DynamicConfigFile dataFile;
     Dictionary<string, List<string>> radzonedata;
     string GetMessage(string key, string steamId);
     void LoadDefaultMessages();
    [PluginReference]
     Plugin ZoneManager;
     void OnServerInitialized();
     void execcfg();
     void safecfg();
     void Unload();
     void Loaded();
    private List<BaseEntity> Spheres;
    private const string SphereEnt;
    private void CreateSphere(Vector3 position, float radius);
    private void DestroyAllSpheres();
    private void createZone(string zoneID, Vector3 pos, float radius, float rads);
    private void eraseZone(string zoneID);
    private void CalcPos(Vector3 pos);
    private void DelPos();
    private void started();
    private void stop();
    private void StartZoneShrink();
    private void Shrink();
     bool IsAllowed(BasePlayer player, string perm);
    [ChatCommand("rad")]
     void radchat(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("rad")]
     void radconsole(ConsoleSystem.Arg arg);
}


```

---

## RadStorm

```csharp
using ConVar;
using Network;
using Oxide.Core.Plugins;
using Rust;
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("RadStorm", "redBDGR", "1.0.8")]
[Description("Dynamic radiation storm events")]
 class RadStorm : RustPlugin
{
    private static RadStorm plugin;
    private bool Changed;
    [PluginReference]
     Plugin GUIAnnouncements;
    private const string permissionNameADMIN;
    private const string permissionNameEXEMPT;
    private float stormLength;
    private float radiationAmount;
    private bool hurtSleepers;
    private float minStormTime;
    private float maxStormTime;
    private bool randomEventEnabled;
    private bool deathAnnouncementsEnabled;
    private float stormChargeTime;
    private bool scaleRadiationBasedOnClothing;
    private bool useChatAnnouncements;
    private bool useGUIAnnouncements;
    private string GUIBannerColour;
    private string GUITextColour;
    private bool announceDeaths;
    private float weatherRainAmount;
    private float weatherFogAmount;
    private float weatherWindAmount;
    private float gatherRateBonus;
    private float pickupRateBonus;
    private float bonusRateBonus;
     RadiationStorm storm;
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private void LoadLang();
    private void Init();
    private void Loaded();
    private void Unload();
    private object OnPlayerDie(BasePlayer player, HitInfo info);
    private void OnPlayerMetabolize(PlayerMetabolism metabolism, BaseCombatEntity entity, float delta);
    private object OnCollectiblePickup(Item item, BasePlayer player);
    private object OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private object OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private class RadiationStorm : FacepunchBehaviour
    {
        public GameObject parentObj;
        public List<string> outsidePlayerCache;
        private bool radiationWasFalse;
        public bool isFullStrength;
        private float originalRainAmount;
        private float originalFogAmount;
        private float originalWindAmount;
        public float rainAmount;
        private float fogAmount;
        private float windAmount;
        private float maxRain;
        private float maxFog;
        private float maxWind;
         Coroutine onlineDamage;
         Coroutine offlineDamage;
        private void Awake();
        private void OnDestroy();
        public void DestroyThis();
        public void StartStorm();
        public void StopStorm();
        private void StormCycle();
        private void IncreaseStormSeverity();
        private void DecreaseStormSeverity();
        private IEnumerator CacheOutdoorPlayers();
        private IEnumerator DamageOfflinePlayers();
        private void UnCachePlayer(string id);
        private void CachePlayer(string id);
    }

    [ChatCommand("radstorm")]
    private void RadstormChatCMD(BasePlayer player, string command, string[] args);
    [ConsoleCommand("radstorm")]
    private void RadStormCMD(ConsoleSystem.Arg arg);
    private void StartStorm();
    private void StopStorm();
    private void RandomStorm();
    private void SendNotificaion(string text, BasePlayer player);
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private string msg(string key, string id);
}

private class RadiationStorm : FacepunchBehaviour
{
    public GameObject parentObj;
    public List<string> outsidePlayerCache;
    private bool radiationWasFalse;
    public bool isFullStrength;
    private float originalRainAmount;
    private float originalFogAmount;
    private float originalWindAmount;
    public float rainAmount;
    private float fogAmount;
    private float windAmount;
    private float maxRain;
    private float maxFog;
    private float maxWind;
     Coroutine onlineDamage;
     Coroutine offlineDamage;
    private void Awake();
    private void OnDestroy();
    public void DestroyThis();
    public void StartStorm();
    public void StopStorm();
    private void StormCycle();
    private void IncreaseStormSeverity();
    private void DecreaseStormSeverity();
    private IEnumerator CacheOutdoorPlayers();
    private IEnumerator DamageOfflinePlayers();
    private void UnCachePlayer(string id);
    private void CachePlayer(string id);
}


```

---

## RadtownAnimals

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using System.Linq;
using System.Reflection;

Oxide.Plugins
[Info("RadtownAnimals", "k1lly0u", "0.2.11", ResourceId = 1561)]
 class RadtownAnimals : RustPlugin
{
    private Dictionary<BaseEntity, Vector3> animalList;
    private List<Timer> refreshTimers;
     void Loaded();
     void OnServerInitialized();
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void Unload();
    private void InitializeAnimalSpawns();
    private Dictionary<string, int> GetSpawnList(AnimalCounts counts);
    private void SpawnAnimals(Vector3 position, Dictionary<string,int> spawnList);
    private void InitiateRefresh(BaseEntity animal);
    private void InitializeNewSpawn(string type, Vector3 position);
    private BaseEntity SpawnAnimalEntity(string type, Vector3 pos);
    private Vector3 AdjustPosition(Vector3 pos);
    static Vector3 GetGroundPosition(Vector3 sourcePos);
     class RAController : MonoBehaviour
    {
        private readonly MethodInfo SetDeltaTimeMethod;
        internal static double targetAttackRange;
        internal Vector3 Home;
        internal Vector3 NextPos;
        internal BaseCombatEntity Target;
        internal bool isAttacking;
        public BaseNPC NPC;
        public NPCAI AI;
        public NPCMetabolism Metabolism;
         void Awake();
         void FixedUpdate();
        public void SetHome(Vector3 pos);
         void CalculateNextPos();
         void Move(Vector3 pos);
         void Sleep();
        internal void OnAttacked(HitInfo info);
        internal void Attack(BaseCombatEntity ent);
    }

    [ChatCommand("ra_killall")]
    private void chatKillAnimals(BasePlayer player, string command, string[] args);
    [ConsoleCommand("ra_killall")]
    private void ccmdKillAnimals(ConsoleSystem.Arg arg);
     class AnimalCounts
    {
        public int Bears;
        public int Boars;
        public int Chickens;
        public int Horses;
        public int Stags;
        public int Wolfs;
    }

     class LightHouses
    {
        public AnimalCounts AnimalCounts { get; set; }
        public bool Enabled { get; set; }
    }

     class Airfield
    {
        public AnimalCounts AnimalCounts { get; set; }
        public bool Enabled { get; set; }
    }

     class Powerplant
    {
        public AnimalCounts AnimalCounts { get; set; }
        public bool Enabled { get; set; }
    }

     class Trainyard
    {
        public AnimalCounts AnimalCounts { get; set; }
        public bool Enabled { get; set; }
    }

     class WaterTreatmentPlant
    {
        public AnimalCounts AnimalCounts { get; set; }
        public bool Enabled { get; set; }
    }

     class Warehouses
    {
        public AnimalCounts AnimalCounts { get; set; }
        public bool Enabled { get; set; }
    }

     class Satellite
    {
        public AnimalCounts AnimalCounts { get; set; }
        public bool Enabled { get; set; }
    }

     class SphereTank
    {
        public AnimalCounts AnimalCounts { get; set; }
        public bool Enabled { get; set; }
    }

     class Radtowns
    {
        public AnimalCounts AnimalCounts { get; set; }
        public bool Enabled { get; set; }
    }

     class MilitaryTunnels
    {
        public AnimalCounts AnimalCounts { get; set; }
        public bool Enabled { get; set; }
    }

     class Options
    {
        public int RespawnTimer;
        public float SpawnSpread;
        public int TotalMaximumAmount;
    }

    private ConfigData configData;
     class ConfigData
    {
        public LightHouses Lighthouses { get; set; }
        public Airfield Airfield { get; set; }
        public Powerplant Powerplant { get; set; }
        public Trainyard Trainyard { get; set; }
        public WaterTreatmentPlant WaterTreatmentPlant { get; set; }
        public Warehouses Warehouses { get; set; }
        public Satellite Satellite { get; set; }
        public SphereTank SphereTank { get; set; }
        public Radtowns Radtowns { get; set; }
        public MilitaryTunnels MilitaryTunnels { get; set; }
        public Options a_Options { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     Dictionary<string, string> messages;
}

 class RAController : MonoBehaviour
{
    private readonly MethodInfo SetDeltaTimeMethod;
    internal static double targetAttackRange;
    internal Vector3 Home;
    internal Vector3 NextPos;
    internal BaseCombatEntity Target;
    internal bool isAttacking;
    public BaseNPC NPC;
    public NPCAI AI;
    public NPCMetabolism Metabolism;
     void Awake();
     void FixedUpdate();
    public void SetHome(Vector3 pos);
     void CalculateNextPos();
     void Move(Vector3 pos);
     void Sleep();
    internal void OnAttacked(HitInfo info);
    internal void Attack(BaseCombatEntity ent);
}

 class AnimalCounts
{
    public int Bears;
    public int Boars;
    public int Chickens;
    public int Horses;
    public int Stags;
    public int Wolfs;
}

 class LightHouses
{
    public AnimalCounts AnimalCounts { get; set; }
    public bool Enabled { get; set; }
}

 class Airfield
{
    public AnimalCounts AnimalCounts { get; set; }
    public bool Enabled { get; set; }
}

 class Powerplant
{
    public AnimalCounts AnimalCounts { get; set; }
    public bool Enabled { get; set; }
}

 class Trainyard
{
    public AnimalCounts AnimalCounts { get; set; }
    public bool Enabled { get; set; }
}

 class WaterTreatmentPlant
{
    public AnimalCounts AnimalCounts { get; set; }
    public bool Enabled { get; set; }
}

 class Warehouses
{
    public AnimalCounts AnimalCounts { get; set; }
    public bool Enabled { get; set; }
}

 class Satellite
{
    public AnimalCounts AnimalCounts { get; set; }
    public bool Enabled { get; set; }
}

 class SphereTank
{
    public AnimalCounts AnimalCounts { get; set; }
    public bool Enabled { get; set; }
}

 class Radtowns
{
    public AnimalCounts AnimalCounts { get; set; }
    public bool Enabled { get; set; }
}

 class MilitaryTunnels
{
    public AnimalCounts AnimalCounts { get; set; }
    public bool Enabled { get; set; }
}

 class Options
{
    public int RespawnTimer;
    public float SpawnSpread;
    public int TotalMaximumAmount;
}

 class ConfigData
{
    public LightHouses Lighthouses { get; set; }
    public Airfield Airfield { get; set; }
    public Powerplant Powerplant { get; set; }
    public Trainyard Trainyard { get; set; }
    public WaterTreatmentPlant WaterTreatmentPlant { get; set; }
    public Warehouses Warehouses { get; set; }
    public Satellite Satellite { get; set; }
    public SphereTank SphereTank { get; set; }
    public Radtowns Radtowns { get; set; }
    public MilitaryTunnels MilitaryTunnels { get; set; }
    public Options a_Options { get; set; }
}


```

---

## Raft

```csharp
using System;
using Oxide.Core;
using Oxide.Core.Configuration;
using Network;
using System.Runtime.CompilerServices;
using System.Collections.Generic;
using UnityEngine;
using Facepunch;
using System.Linq;

Oxide.Plugins
[Info("Raft", "Colon Blow", "1.0.24")]
 class Raft : RustPlugin
{
     BaseEntity newRaft;
    static Dictionary<ulong, string> hasRaft;
    static List<uint> storedRafts;
    private DynamicConfigFile data;
    private bool initialized;
     void Loaded();
    private void OnServerInitialized();
    private void OnServerSave();
    private void RestoreRafts();
     void SaveData();
     void LoadData();
     bool isAllowed(BasePlayer player, string perm);
    public void AddPlayerRaft(ulong id);
    public void RemovePlayerRaft(ulong id);
    public void BuildRaft(BasePlayer player);
    public void RespawnRaft(Vector3 spawnpos, Quaternion spawnrot, ulong userid, bool hassail, bool hasfire, bool hasnet, bool hasroof, bool haswalls, bool haschairs, string codestr, string guestcodestr);
     void ScrapRaft(BasePlayer player);
     bool CheckUpgradeMats(BasePlayer player, int itemID, int amount, string str);
    public bool IsStandingInWater(BasePlayer player);
    private void OnPlayerInput(BasePlayer player, InputState input);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     object OnEntityGroundMissing(BaseEntity entity);
     void OnEntityMounted(BaseMountable mountable, BasePlayer player);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     object CanPickupEntity(BaseCombatEntity entity, BasePlayer player);
     object CanPickupLock(BasePlayer player, BaseLock baseLock);
     void Unload();
     void DestroyAll();
    [ChatCommand("raft")]
     void cmdChatRaftHelp(BasePlayer player, string command, string[] args);
    [ChatCommand("raft.build")]
     void cmdChatRaftBuild(BasePlayer player, string command, string[] args);
    [ConsoleCommand("raft.build")]
     void cmdConsoleRaftBuild(ConsoleSystem.Arg arg);
    [ChatCommand("raft.addwalls")]
     void cmdRaftAddWalls(BasePlayer player, string command, string[] args);
    [ConsoleCommand("raft.addwalls")]
     void cmdConsoleRaftAddWalls(ConsoleSystem.Arg arg);
    [ChatCommand("raft.addroof")]
     void cmdRaftAddRoof(BasePlayer player, string command, string[] args);
    [ConsoleCommand("raft.addroof")]
     void cmdConsoleRaftAddRoof(ConsoleSystem.Arg arg);
    [ChatCommand("raft.addsail")]
     void cmdRaftAddSail(BasePlayer player, string command, string[] args);
    [ConsoleCommand("raft.addsail")]
     void cmdConsoleRaftAddSail(ConsoleSystem.Arg arg);
    [ChatCommand("raft.addfire")]
     void cmdRaftAddFire(BasePlayer player, string command, string[] args);
    [ConsoleCommand("raft.addfire")]
     void cmdConsoleRaftAddFire(ConsoleSystem.Arg arg);
    [ChatCommand("raft.addchairs")]
     void cmdRaftAddChairs(BasePlayer player, string command, string[] args);
    [ConsoleCommand("raft.addchairs")]
     void cmdConsoleRaftAddChairs(ConsoleSystem.Arg arg);
    [ChatCommand("raft.addnet")]
     void cmdRaftAddNet(BasePlayer player, string command, string[] args);
    [ConsoleCommand("raft.addnet")]
     void cmdConsoleRaftAddNet(ConsoleSystem.Arg arg);
    [ChatCommand("raft.setsail")]
     void cmdRaftSail(BasePlayer player, string command, string[] args);
    [ConsoleCommand("raft.setsail")]
     void cmdConsoleRaftSail(ConsoleSystem.Arg arg);
    [ChatCommand("raft.loc")]
     void cmdRaftLoc(BasePlayer player, string command, string[] args);
    [ConsoleCommand("raft.loc")]
     void cmdConsoleRaftLoc(ConsoleSystem.Arg arg);
    [ChatCommand("raft.destroy")]
     void cmdRaftDestroy(BasePlayer player, string command, string[] args);
    [ChatCommand("raft.stash")]
     void cmdRaftStash(BasePlayer player, string command, string[] args);
    public class RaftEntity : BaseEntity
    {
         Raft raft;
        public BaseEntity entity;
         BaseEntity floor1;
         BaseEntity floor2;
         BaseEntity barrel1;
         BaseEntity barrel2;
         BaseEntity barrel3;
         BaseEntity barrel4;
         BaseEntity barrel5;
         BaseEntity barrel6;
         BaseEntity backtop;
         BaseEntity backside1;
         BaseEntity backside2;
         BaseEntity rudder;
         BaseEntity pole1;
         BaseEntity pole2;
         BaseEntity pole3;
         BaseEntity pole4;
        public BaseEntity roof1;
        public BaseEntity roof2;
         BaseOven fire;
         BaseOven lantern;
        public BaseEntity net1;
        public BaseEntity door;
         BaseEntity door2;
         BaseEntity chairbackright;
         BaseEntity chairbackleft;
         BaseEntity stash;
         BaseEntity lootbarrel;
        public BaseEntity boatlock;
         Vector3 entitypos;
         Quaternion entityrot;
        public BasePlayer player;
         ulong ownerid;
         int counter;
         bool lootready;
        public bool ismoving;
        public bool moveforward;
        public bool movebackward;
        public bool rotright;
        public bool rotleft;
        public bool setsail;
        public bool hasroof;
        public bool haswalls;
        public bool hassail;
        public bool hasfire;
        public bool hasnet;
        public bool haschairs;
         bool spawnfullybuilt;
         Vector3 movedirection;
         Vector3 rotdirection;
         Vector3 startloc;
         Vector3 startrot;
         Vector3 endloc;
         float steps;
         float sailsteps;
         float incrementor;
         float waterheight;
         float groundheight;
        public float ghitDistance;
        public float whitDistance;
        private static int waterlayer;
        private static int groundlayer;
        private static int buildinglayer;
         void Awake();
        public void SpawnStash();
         void SpawnLock();
        public void SpawnSail();
         void SpawnRaft();
        public void SpawnPassengerChairs();
        public void SpawnNet();
        public void SpawnCampfire();
        public void SpawnSideWalls();
        public void SpawnRoof();
         bool hitSomething(Vector3 position);
         bool isStillInWater(Vector3 position);
         bool PlayerIsMounted();
        public void ToggleStash();
        public void UnfurlTheSails();
        public void FurlTheSails();
         void SplashEffect();
         void SpawnRandomLoot();
         void ResetLootSpawn(BaseEntity lootbarrel);
         Vector3 GetSpawnLocation();
         void FixedUpdate();
         void ResetMovement();
         void RefreshAll();
        public void OnDestroy();
    }

    static float DefaultRaftMovementSpeed;
    static float DefaultRaftSailingSpeed;
    static bool DisableRaftStash;
    static bool SpawnFullyBuilt;
    static bool OnlyOneActiveRaft;
    static bool ShowWaterSplash;
    static bool RandomOceanLootSpawn;
    static int RandomLootChance;
    static int RandomLootTick;
    static string LootOption1;
    static string LootOption2;
    static string LootOption3;
    static string LootOption4;
    static string LootOption5;
    static int MaterialsForRoof;
    static int MaterialsForWalls;
    static int MaterialsForRaft;
    static int MaterialsForSail;
    static int MaterialsForCampfire;
    static int MaterialsForNet;
    static int MaterialsForChairs;
    static int RefundForScrap;
     bool Changed;
     bool isRestricted;
    private void LoadVariables();
     void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
     object GetConfig(string menu, string datavalue, object defaultValue);
     string msg(string key, string playerId);
     Dictionary<string, string> Messages;
}

public class RaftEntity : BaseEntity
{
     Raft raft;
    public BaseEntity entity;
     BaseEntity floor1;
     BaseEntity floor2;
     BaseEntity barrel1;
     BaseEntity barrel2;
     BaseEntity barrel3;
     BaseEntity barrel4;
     BaseEntity barrel5;
     BaseEntity barrel6;
     BaseEntity backtop;
     BaseEntity backside1;
     BaseEntity backside2;
     BaseEntity rudder;
     BaseEntity pole1;
     BaseEntity pole2;
     BaseEntity pole3;
     BaseEntity pole4;
    public BaseEntity roof1;
    public BaseEntity roof2;
     BaseOven fire;
     BaseOven lantern;
    public BaseEntity net1;
    public BaseEntity door;
     BaseEntity door2;
     BaseEntity chairbackright;
     BaseEntity chairbackleft;
     BaseEntity stash;
     BaseEntity lootbarrel;
    public BaseEntity boatlock;
     Vector3 entitypos;
     Quaternion entityrot;
    public BasePlayer player;
     ulong ownerid;
     int counter;
     bool lootready;
    public bool ismoving;
    public bool moveforward;
    public bool movebackward;
    public bool rotright;
    public bool rotleft;
    public bool setsail;
    public bool hasroof;
    public bool haswalls;
    public bool hassail;
    public bool hasfire;
    public bool hasnet;
    public bool haschairs;
     bool spawnfullybuilt;
     Vector3 movedirection;
     Vector3 rotdirection;
     Vector3 startloc;
     Vector3 startrot;
     Vector3 endloc;
     float steps;
     float sailsteps;
     float incrementor;
     float waterheight;
     float groundheight;
    public float ghitDistance;
    public float whitDistance;
    private static int waterlayer;
    private static int groundlayer;
    private static int buildinglayer;
     void Awake();
    public void SpawnStash();
     void SpawnLock();
    public void SpawnSail();
     void SpawnRaft();
    public void SpawnPassengerChairs();
    public void SpawnNet();
    public void SpawnCampfire();
    public void SpawnSideWalls();
    public void SpawnRoof();
     bool hitSomething(Vector3 position);
     bool isStillInWater(Vector3 position);
     bool PlayerIsMounted();
    public void ToggleStash();
    public void UnfurlTheSails();
    public void FurlTheSails();
     void SplashEffect();
     void SpawnRandomLoot();
     void ResetLootSpawn(BaseEntity lootbarrel);
     Vector3 GetSpawnLocation();
     void FixedUpdate();
     void ResetMovement();
     void RefreshAll();
    public void OnDestroy();
}


```

---

## Ragnarok

```csharp
using System;
using System.Collections.Generic;
using System.Reflection;
using UnityEngine;
using Rust;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Ragnarok", "Drefetr et Shmitt", "0.7.8", ResourceId = 1985)]
public class Ragnarok : RustPlugin
{
    private float minLaunchAngle;
    private float maxLaunchAngle;
    private float minLaunchHeight;
    private float maxLaunchHeight;
    private float minLaunchVelocity;
    private float maxLaunchVelocity;
    private int meteorFrequency;
    private int maxClusterSize;
    private int minClusterRange;
    private int maxClusterRange;
    private float spawnResourcePercent;
    private float spawnResourceNodePercent;
    private int tickCounter;
    private void OnServerInitialized();
    protected override void LoadDefaultConfig();
     void OnTick();
    private void spawnMeteor(Vector3 origin);
    private void spawnResource(Vector3 location);
    private void spawnResourceNode(Vector3 location);
    private ItemDefinition getBasicRocket();
    private ItemDefinition getFireRocket();
    private ItemDefinition getHighVelocityRocket();
    private ItemDefinition getSmokeRocket();
    private Vector3 getRandomMapPosition();
    private float getMapSize();
}


```

---

## RaidAlerts

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Rust;
using UnityEngine;
using Server = ConVar.Server;

Oxide.Plugins
[Info("Raid Alerts", "Ryz0r/Mevent", "1.0.2")]
[Description("Allows players with permissions to receive alerts when explosives are thrown or fired.")]
public class RaidAlerts : CovalencePlugin
{
    private const string RaidAlertCommands;
    private readonly Dictionary<string, float> _alertedUsers;
    private readonly List<string> _enabledPlayersList;
    private void SaveData();
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "UseWebhook")]
        public bool UseWebhook;
        [JsonProperty(PropertyName = "WebhookURL")]
        public string WebhookUrl;
        [JsonProperty(PropertyName = "OutputCooldown")]
        public float OutputCooldown;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private void OnNewSave(string filename);
    private void Init();
    [Command("alerts")]
    private void AlertsCommand(IPlayer player, string command, string[] args);
    private void OnExplosiveThrown(BasePlayer player, BaseEntity entity, ThrownWeapon item);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnRocketLaunched(BasePlayer player, BaseEntity entity);
    private void SendDiscordMessage(string playerName, Vector3 entityLocation, string explosive);
    private class DiscordMessage
    {
        public DiscordMessage(string content, Embed[] embeds);
        [JsonProperty("content")]
        public string Content { get; set; }
        [JsonProperty("embeds")]
        public List<Embed> Embeds { get; set; }
        public string ToJson();
    }

    private class Embed
    {
        [JsonProperty("fields")]
        public List<Field> Fields { get; set; }
        public Embed AddField(string name, string value, bool inline);
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

    private static string GetGrid(Vector3 pos);
}

private class Configuration
{
    [JsonProperty(PropertyName = "UseWebhook")]
    public bool UseWebhook;
    [JsonProperty(PropertyName = "WebhookURL")]
    public string WebhookUrl;
    [JsonProperty(PropertyName = "OutputCooldown")]
    public float OutputCooldown;
}

private class DiscordMessage
{
    public DiscordMessage(string content, Embed[] embeds);
    [JsonProperty("content")]
    public string Content { get; set; }
    [JsonProperty("embeds")]
    public List<Embed> Embeds { get; set; }
    public string ToJson();
}

private class Embed
{
    [JsonProperty("fields")]
    public List<Field> Fields { get; set; }
    public Embed AddField(string name, string value, bool inline);
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

## RaidBlock

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using ConVar;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;
using VLB;
using Oxide.Plugins.RaidBlockExt;
using UnityEngine.Networking;
using Layer = Rust.Layer;
using Pool = Facepunch.Pool;
using Time = UnityEngine.Time;

Oxide.Plugins
[Info("RaidBlock", "Mercury", "1.1.24")]
public class RaidBlock : RustPlugin
{
    [PluginReference]
     Plugin Friends;
     Plugin Clans;
     Plugin IQChat;
    private List<BasePlayer> playerInCache;
    private void SendChat(string message, BasePlayer player, Single timeout, Chat.ChatChannel channel);
    private bool IsFriends(BasePlayer player, ulong targetID);
    private ulong[] GetFriendList(BasePlayer targetPlayer);
    public static RaidBlock Instance;
    private static InterfaceBuilder _interface;
    private List<RaidableZone> _raidZoneComponents;
    private static ImageUI _imageUI;
    private const Boolean LanguageEn;
    private const string GENERIC_MAP_MARKER_PREFAB;
    private const string EXPLOSION_MAP_MARKER_PREFAB;
    private const string VENDING_MAP_MARKER_PREFAB;
    private const string VISUALIZATION_SPHERE_PREFAB;
    private const string VISUALIZATION_BR_SPHERE_PREFAB_BLUE;
    private const string VISUALIZATION_BR_SPHERE_PREFAB_GREEN;
    private const string VISUALIZATION_BR_SPHERE_PREFAB_PURPLE;
    private const string VISUALIZATION_BR_SPHERE_PREFAB_RED;
    private const string RB_WHITE_AND_BLACK_LIST_ITEM;
    private const ulong RB_WHITELIST_ITEM_SKIN;
    private const ulong RB_BLACKLIST_ITEM_SKIN;
    private readonly string _permIgnoreRaid;
    private readonly string _permHelperToolGun;
    private Configuration config;
    private class Configuration
    {
        public class IQChat
        {
            [JsonProperty(LanguageEn ? "IQChat : Custom prefix in the chat" : "IQChat : Кастомный префикс в чате")]
            public String CustomPrefix;
            [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
            public String CustomAvatar;
            [JsonProperty(LanguageEn ? "IQChat : Use UI notifications" : "IQChat : Использовать UI уведомления")]
            public Boolean UIAlertUse;
        }

        public class RaidableBases
        {
            [JsonProperty(LanguageEn ? "RaidableBases: Enable support for Raidable Bases?" : "RaidableBases : Включить поддержку Raidable Bases ?")]
            public bool useRaidableBases;
            [JsonProperty(LanguageEn ? "RaidableBases: Blocking time (Seconds)" : "RaidableBases: Время блокировки (Секунды)")]
            public int RaidBlockDuration;
            [JsonProperty(LanguageEn ? "RaidableBases : Give a raid block when entering the Raidable Bases zone" : "RaidableBases : Давать рейд блок при входе в зону Raidable Bases")]
            public bool BlockOnEnterZone;
            [JsonProperty(LanguageEn ? "RaidableBases : Remove the raid block when leaving the Raidable Bases zone" : "RaidableBases : Снимать рейд блок при выходе из зоны Raidable Bases")]
            public bool BlockOnExitZone;
        }

        public class RaidBlockActionsBlocked
        {
            [JsonProperty(LanguageEn ? "Forbid building repair" : "Запретить починку строений")]
            public bool CanRepairObjects;
            [JsonProperty(LanguageEn ? "Forbid picking up items (furnaces, boxes, etc.)" : "Запретить поднятие вещей (печки/ящики и т.д)")]
            public bool CanPickUpObjects;
            [JsonProperty(LanguageEn ? "Forbid upgrading buildings" : "Запретить улучшение строений")]
            public bool CanUpgradeObjects;
            [JsonProperty(LanguageEn ? "Forbid building removal" : "Запретить удаление строений")]
            public bool CanDemolishObjects;
            [JsonProperty(LanguageEn ? "Forbid teleportation" : "Запретить телепортацию")]
            public bool CanUseTeleport;
            [JsonProperty(LanguageEn ? "Forbid the use of kits" : "Запретить использование китов")]
            public bool CanUseKit;
            [JsonProperty(LanguageEn ? "Forbid the use of trade" : "Запретить использование обмена (Trade)")]
            public bool CanUseTrade;
            [JsonProperty(LanguageEn ? "Allow building" : "Запретить строение")]
            public bool CanBuildTwig;
            [JsonProperty(LanguageEn ? "Allow object placement (furnaces, boxes, etc.)" : "Запретить размещение объектов (Печки, ящики и другое)")]
            public bool CanDeployObjects;
            [JsonProperty(LanguageEn ? "List of objects allowed to build/place during the raidblock (shortname)" : "Список объектов, которые разрешено строить/размещать во время рейдблока (PrefabName)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public List<string> RbDeployWhiteList;
            [JsonProperty(LanguageEn ? "List of prohibited commands when blocking is active [specify them without a slash (/) in lower case]" : "Список запрещенных команд при активной блокировки [указывайте их без слэша (/) в нижнем регистре]", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public List<string> RbBlackListCommand;
        }

        public class RaidBlockDetect
        {
            [JsonProperty(LanguageEn ? "List of items that will trigger a raid block upon destruction (prefabID)" : "Список предметов за которые при уничтожении будет даваться рейдблок (prefabID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public HashSet<uint> RbBlackList;
            [JsonProperty(LanguageEn ? "List of items that will not trigger a raid block upon destruction (prefabID)" : "Список предметов за которые при уничтожении не будет даваться рейдблок (prefabID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public HashSet<uint> RbWhiteList;
            [JsonProperty(LanguageEn ? "Ignore objects with a maximum health state less than N (0 - disabled)" : "Игнорировать объекты с максимальным состоянием здоровья меньше N (0 - отключено)")]
            public int IgnoreEntsHealth;
            [JsonProperty(LanguageEn ? "Activate raidblock upon the destruction of own or friends' buildings" : "Активировать рейдблок при уничтожении собственного строения или строения друзей")]
            public bool RaidYourself;
            [JsonProperty(LanguageEn ? "Activate raidblock if there is no tool cupboard in the building" : "Активировать рейдблок если в строении нет шкафа")]
            public bool CanRaidIfNotCupboard;
        }

        public class RaidBlock
        {
            [JsonProperty(LanguageEn ? "Radius of blocking zone (Meters)" : "Радиус зоны блокировки (Метры)")]
            public int RaidBlockDistance;
            [JsonProperty(LanguageEn ? "Blocking time (Seconds)" : "Время блокировки (Секунды)")]
            public int RaidBlockDuration;
            [JsonProperty(LanguageEn ? "Use dynamic raid zone (shift the zone center to the explosion location)" : "Использовать динамичную зону рейда (смещение центра зоны к месту взрыва)")]
            public bool IsDynamicRaidZone;
            [JsonProperty(LanguageEn ? "Spread raidblock to all team players" : "Распространять рейд блок на всех игроков в команде инициатора рейда")]
            public bool RaidBlockShareOnFriends;
            [JsonProperty(LanguageEn ? "When entering the active raid block zone, activate the player raid block" : "При входе в активную зону рейдблока - активировать рейдблок игроку")]
            public bool RaidBlockOnEnterRaidZone;
            [JsonProperty(LanguageEn ? "Upon exiting the raid block zone, deactivate the raid block for the player" : "При выходе из зоны рейдблока - деактивировать рейдблок игроку")]
            public bool RaidBlockOnExitRaidZone;
            [JsonProperty(LanguageEn ? "When exiting the raid block zone, leave a lock for N seconds (leave 0 if you don't need it)" : "При выходе из зоны рейдблока - оставлять N секунд блокировки (оставьте 0 - если вам не нужно это)")]
            public Int32 TimeLeftOnExitZone;
            [JsonProperty(LanguageEn ? "Activate raid block for all players within the effective radius after the raid starts" : "Активировать рейдблок для всех игроков в радиусе действия после начала рейда")]
            public bool RaidBlockAddedAllPlayersInZoneRaid;
            [JsonProperty(LanguageEn ? "Deactivate the raid block for the player upon death" : "Деактивировать рейдблок игроку после смерти")]
            public bool RaidBlockOnPlayerDeath;
            [JsonProperty(LanguageEn ? "Raid zone map marker settings" : "Настройки маркера на карте в зоне рейда")]
            public MapMarkerSettings mapMarkerSettings;
            [JsonProperty(LanguageEn ? "Visual raid zone (Dome) settings" : "Настройка визуальной зоны рейда (купол)")]
            public SphereSettings RaidZoneSphereSettings;
            [JsonProperty(LanguageEn ? "Integration settings with RaidableBases" : "Настройки интеграции с RaidableBases")]
            public RaidableBases RaidableBasesIntegration;
            public class SphereSettings
            {
                [JsonProperty(LanguageEn ? "Activate visual raid zone (Dome)" : "Активировать визуальную зону рейда (купол)")]
                public bool IsSphereEnabled;
                [JsonProperty(LanguageEn ? "Choose marker type: 0 - standard dome, 1 - BattleRoyale dome" : "Выберете тип купола : 0 - cтандартный купол, 1 - BattleRoyale купол")]
                public SphereTypes SphereType;
                [JsonProperty(LanguageEn ? "Color for BattleRoyale dome: 0 - blue | 1 - green | 2 - purple | 3 - red" : "Цвет для BattleRoyale купола : 0 - blue | 1 - green | 2 - purple | 3 - red")]
                public BRZoneColor BRZoneColor;
                [JsonProperty(LanguageEn ? "Transparency level of the standard dome (Lower values mean more transparency. The value should not exceed 5)" : "Уровень прозрачности стандартного купола (Чем меньше - тем прозрачнее. Значения должно быть не более 5)")]
                public int DomeTransparencyLevel;
                public string GetBRZonePrefab();
            }

            public class MapMarkerSettings
            {
                [JsonProperty(LanguageEn ? "Display the block zone on the G map" : "Отображать зону блокировки на G карте")]
                public bool IsRaidBlockMarkerEnabled;
                [JsonProperty(LanguageEn ? "Choose marker type: 0 - Explosion, 1 - Circle, 2 - Explosion + Circle, 3 - Circle + Timer" : "Выберите тип маркера: 0 - Explosion | 1 - Marker Radius | 2 - Explosion + Marker Radius | 3 - Marker Radius + Timer")]
                public MarkerTypes RaidBlockMarkerType;
                [JsonProperty(LanguageEn ? "Marker color (without #) (For marker types 1, 2, and 3)" : "Цвет маркера (без #) (Для маркера типа 1, 2 и 3)")]
                public string MarkerColorHex;
                [JsonProperty(LanguageEn ? "Outline color (without #) (For marker types 1, 2, and 3)" : "Цвет обводки (без #) (Для маркера типа 1, 2 и 3)")]
                public string OutlineColorHex;
                [JsonProperty(LanguageEn ? "Marker transparency (For marker types 1, 2, and 3)" : "Прозрачность маркера (Для маркера типа 1, 2 и 3)")]
                public float MarkerAlpha;
            }

        }

        public class RaidBlockUi
        {
            [JsonProperty(LanguageEn ? "Interface variant (0, 1, 2) - example: " : "Вариант интерфейса (0, 1, 2)")]
            public int UiType;
            [JsonProperty(LanguageEn ? "Interface layer: Overlay - will overlay other UI, Hud - will be overlaid by other interfaces" : "Слой интерфейса : Overlay - будет перекрывать другие UI, Hud - будет перекрываться другим интерфейсом")]
            public string Layers;
            [JsonProperty(LanguageEn ? "Vertical padding" : "Вертикальный отступ")]
            public int OffsetY;
            [JsonProperty(LanguageEn ? "Horizontal padding" : "Горизонтальный отступ")]
            public int OffsetX;
            [JsonProperty(LanguageEn ? "Interface settings for variant 0" : "Настройки интерфейса для варианта 0")]
            public RaidBlockUiSettings InterfaceSettingsVariant0;
            [JsonProperty(LanguageEn ? "Interface settings for variant 1" : "Настройки интерфейса для варианта 1")]
            public RaidBlockUiSettings InterfaceSettingsVariant1;
            [JsonProperty(LanguageEn ? "Interface settings for variant 2" : "Настройки интерфейса для варианта 2")]
            public RaidBlockUiSettings InterfaceSettingsVariant2;
            public class RaidBlockUiSettings
            {
                [JsonProperty(LanguageEn ? "Background color (RGBA)" : "Цвет фона (RGBA)")]
                public string BackgroundColor;
                [JsonProperty(LanguageEn ? "Icon color (RGBA)" : "Цвет иконки (RGBA)")]
                public string IconColor;
                [JsonProperty(LanguageEn ? "Color of additional elements (RGBA)" : "Цвет дополнительных элементов (RGBA)")]
                public string AdditionalElementsColor;
                [JsonProperty(LanguageEn ? "Main text color (RGBA)" : "Цвет основного текста (RGBA)")]
                public string MainTextColor;
                [JsonProperty(LanguageEn ? "Secondary text Color (RGBA)" : "Цвет второстепенного текста (RGBA)")]
                public string SecondaryTextColor;
                [JsonProperty(LanguageEn ? "Main color of the progress-bar (RGBA)" : "Основной цвет прогресс-бара (RGBA)")]
                public string ProgressBarMainColor;
                [JsonProperty(LanguageEn ? "Background Color of the Progress Bar (RGBA)" : "Цвет фона прогресс-бара (RGBA)")]
                public string ProgressBarBackgroundColor;
                [JsonProperty(LanguageEn ? "Delay before the UI appears and disappears (for smooth transitions)" : "Задержка перед появлением и исчезновением UI (для плавности)")]
                public float SmoothTransition;
            }

        }

        [JsonProperty(LanguageEn ? "Main raidblock settings" : "Основные настройки рейд-блока")]
        public RaidBlock RaidBlockMain;
        [JsonProperty(LanguageEn ? "Setting up triggers for the raidblock" : "Настройка триггеров для рейдблока")]
        public RaidBlockDetect BlockDetect;
        [JsonProperty(LanguageEn ? "Setting restrictions during the raid block" : "Настройка ограничений во время рейдблока")]
        public RaidBlockActionsBlocked ActionsBlocked;
        [JsonProperty(LanguageEn ? "Interface settings" : "Настройки интерфейса")]
        public RaidBlockUi RaidBlockInterface;
        [JsonProperty(LanguageEn ? "Setting IQChat" : "Настройка IQChat")]
        public IQChat IQChatSetting;
    }

    protected override void LoadConfig();
    private void ValidateConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void Unload();
    private void Init();
    private void OnServerInitialized();
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private void OnPlayerConnected(BasePlayer player);
     void OnPlayerSleepEnded(BasePlayer player);
    private void OnPlayerDeath(BasePlayer player, HitInfo hitInfo);
     void OnPlayerEnteredRaidableBase(BasePlayer player, Vector3 raidPos, bool allowPVP, int mode);
     void OnPlayerExitedRaidableBase(BasePlayer player, Vector3 raidPos, bool allowPVP, int mode);
    private object CanBuild(Planner planner, Construction prefab, Construction.Target target);
    private object OnPlayerCommand(BasePlayer player, String command, String[] args);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private object OnStructureRepair(BaseCombatEntity entity, BasePlayer player);
    private object OnStructureUpgrade(BuildingBlock block, BasePlayer player, BuildingGrade.Enum grade);
    private object OnStructureDemolish(BaseCombatEntity entity, BasePlayer player, bool immediate);
    private object OnStructureRotate(BaseCombatEntity entity, BasePlayer player);
    private object CanPickupEntity(BasePlayer player, BaseEntity entity);
    private object CanBGrade(BasePlayer player, int grade, BuildingBlock buildingBlock, Planner planner);
    private object CanTeleport(BasePlayer player);
    private object canTeleport(BasePlayer player);
    private object CanRedeemKit(BasePlayer player);
    private object canRemove(BasePlayer player);
    private object CanRemove(BasePlayer player);
    private object canTrade(BasePlayer player);
    private object CanTrade(BasePlayer player);
    private bool IsBlocked(BasePlayer player);
    private RaidPlayer GetRaidPlayer(BasePlayer player);
    private bool IsRaidBlocked(string playerId);
    private bool IsRaidBlocked(ulong playerId);
    private bool IsRaidBlocked(BasePlayer player);
    private bool IsRaidBlock(ulong userId);
    private int ApiGetTime(ulong userId);
    private void CheckUnsubscribeOrSubscribeHooks();
    private void SubscribeHook(bool main, bool raidActions);
    private void UnsubscribeHook(bool main, bool raidActions);
    private static void RunEffect(BasePlayer player, string prefab);
    private object CanActions(BasePlayer player, bool returnMessage);
    private void OnEntCheck(BaseCombatEntity entity, HitInfo info);
    private void CreateOrRefreshRaidblock(Vector3 position, BasePlayer player);
    [ChatCommand("rbtest")]
     void rbtest(BasePlayer player);
    private static string GetGridString(Vector3 position);
    private bool CheckEntity(BaseCombatEntity entity, HitInfo info, BasePlayer raider);
    private bool IsBlockedClass(BaseCombatEntity entity);
    private bool IsWhiteList(BaseCombatEntity entity);
    private bool IsBlackList(BaseCombatEntity entity);
    private object OnHammerHit(BasePlayer player, HitInfo info);
    private void UpdateWhiteOrBlackList(BasePlayer player, ulong skinId, HitInfo info);
    [ChatCommand("rb.white")]
    private void RaidBlockDetectWhiteList(BasePlayer player);
    [ChatCommand("rb.black")]
    private void RaidBlockDetectBlackList(BasePlayer player);
    private bool HasPermission(BasePlayer player);
    private bool FindItemInInventory(BasePlayer player, ulong skinId);
    private void GameTipsSendPlayer(BasePlayer player, string message, float seconds, bool error);
    private readonly Dictionary<BasePlayer, Timer> _playerTimer;
    private void DeleteNotification(BasePlayer player, float seconds);
    private void CreateToolGunItem(BasePlayer player, string name, ulong skinId);
    private List<RaidPlayer> raidPlayersList;
    private class RaidPlayer : FacepunchBehaviour
    {
        public BasePlayer player;
        public Single blockEnds;
        public float UnblockTimeLeft { get; set; }
        private void Awake();
        public void Kill(Boolean force);
        private void OnDestroy();
        public void UpdateTime(Single time, Boolean customTime);
        public void ActivateBlock(Single time);
        private void CheckTimeLeft();
        public void CrateUI();
        private void RefreshUI();
    }

    private static RaidableZone GetRbZone(Vector3 position);
    private class RaidableZone : MonoBehaviour
    {
        private Dictionary<BasePlayer, RaidPlayer> _playersAndComponentZone;
        private MapMarkerExplosion marker;
        private MapMarkerGenericRadius mapMarkerGenericRadius;
        private VendingMachineMapMarker vendingMakrer;
        private List<BaseEntity> _spheres;
        private SphereCollider triggerZone;
        private BasePlayer initiatorRaid;
        private Single timeToUnblock;
        private Single UnblockTimeLeft { get; set; }
        private Int32 raidBlockDistance;
        private Int32 raidBlockDuration;
        private bool IsDynamicRaidZone;
        private void CreateSphere();
        private void CreateMapMarker();
        private void CreateExplosionMapMarker();
        private void CreateVendingMapMarker();
        private void CreateGenericRadiusMapMarker();
        public void UpdateGenericRadiusMapMarker();
        private void UpdateZonePosition(Vector3 position);
        private void InitializeTriggerZone();
        public void CreateRaidZone(Vector3 raidPos, BasePlayer initiator);
        public void RefreshTimer(Vector3 pos, BasePlayer initiatorReply);
        private void EndRaid();
        private void AddAllPlayerInZoneDistance();
        private void AddAllPlayerFriendsInitiator(BasePlayer initiator);
        public void AddPlayer(BasePlayer player, Boolean force);
        public void RemovePlayer(BasePlayer player);
        public void RecheackRbZone(BasePlayer player);
        private void Awake();
        private void OnDestroy();
        private void OnTriggerEnter(Collider collider);
        private void OnTriggerExit(Collider collider);
    }

    private List<BasePlayer> GetPlayerFriends(BasePlayer player);
    private void DrawUI_RB_Main(BasePlayer player, float timeLeft);
    private void DrawUI_RB_Updated(BasePlayer player, float timeLeft, bool upd);
    private class InterfaceBuilder
    {
        private static InterfaceBuilder _instance;
        public const string RB_MAIN;
        public const string RB_PROGRESS_BAR;
        public const string RB_PROGRESS;
        public const string RB_PROGRESS_TIMER;
        public static int TypeUi;
        public static double Factor;
        private Configuration.RaidBlockUi.RaidBlockUiSettings _uiSettings;
        private static float _fade;
        private Dictionary<string, string> _interfaces;
        public InterfaceBuilder();
        private static void AddInterface(string name, string json);
        public static string GetInterface(string name);
        public static void DestroyAll();
        private void BuildingRaidBlockMain();
        private void BuildingRaidBlockUpdated();
        private void BuildingRaidBlockMainV2();
        private void BuildingRaidBlockUpdatedV2();
        private void BuildingRaidBlockMainV3();
        private void BuildingRaidBlockUpdatedV3();
    }

    private class ImageUI
    {
        private readonly string _paths;
        private readonly string _printPath;
        private readonly Dictionary<string, ImageData> _images;
        public ImageUI();
        private class ImageData
        {
            public ImageStatus Status;
            public string Id { get; set; }
        }

        public string GetImage(string name);
        public void DownloadImage();
        public void UnloadImages();
        private IEnumerator ProcessDownloadImage(KeyValuePair<string, ImageData> image);
    }

}

private class Configuration
{
    public class IQChat
    {
        [JsonProperty(LanguageEn ? "IQChat : Custom prefix in the chat" : "IQChat : Кастомный префикс в чате")]
        public String CustomPrefix;
        [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
        public String CustomAvatar;
        [JsonProperty(LanguageEn ? "IQChat : Use UI notifications" : "IQChat : Использовать UI уведомления")]
        public Boolean UIAlertUse;
    }

    public class RaidableBases
    {
        [JsonProperty(LanguageEn ? "RaidableBases: Enable support for Raidable Bases?" : "RaidableBases : Включить поддержку Raidable Bases ?")]
        public bool useRaidableBases;
        [JsonProperty(LanguageEn ? "RaidableBases: Blocking time (Seconds)" : "RaidableBases: Время блокировки (Секунды)")]
        public int RaidBlockDuration;
        [JsonProperty(LanguageEn ? "RaidableBases : Give a raid block when entering the Raidable Bases zone" : "RaidableBases : Давать рейд блок при входе в зону Raidable Bases")]
        public bool BlockOnEnterZone;
        [JsonProperty(LanguageEn ? "RaidableBases : Remove the raid block when leaving the Raidable Bases zone" : "RaidableBases : Снимать рейд блок при выходе из зоны Raidable Bases")]
        public bool BlockOnExitZone;
    }

    public class RaidBlockActionsBlocked
    {
        [JsonProperty(LanguageEn ? "Forbid building repair" : "Запретить починку строений")]
        public bool CanRepairObjects;
        [JsonProperty(LanguageEn ? "Forbid picking up items (furnaces, boxes, etc.)" : "Запретить поднятие вещей (печки/ящики и т.д)")]
        public bool CanPickUpObjects;
        [JsonProperty(LanguageEn ? "Forbid upgrading buildings" : "Запретить улучшение строений")]
        public bool CanUpgradeObjects;
        [JsonProperty(LanguageEn ? "Forbid building removal" : "Запретить удаление строений")]
        public bool CanDemolishObjects;
        [JsonProperty(LanguageEn ? "Forbid teleportation" : "Запретить телепортацию")]
        public bool CanUseTeleport;
        [JsonProperty(LanguageEn ? "Forbid the use of kits" : "Запретить использование китов")]
        public bool CanUseKit;
        [JsonProperty(LanguageEn ? "Forbid the use of trade" : "Запретить использование обмена (Trade)")]
        public bool CanUseTrade;
        [JsonProperty(LanguageEn ? "Allow building" : "Запретить строение")]
        public bool CanBuildTwig;
        [JsonProperty(LanguageEn ? "Allow object placement (furnaces, boxes, etc.)" : "Запретить размещение объектов (Печки, ящики и другое)")]
        public bool CanDeployObjects;
        [JsonProperty(LanguageEn ? "List of objects allowed to build/place during the raidblock (shortname)" : "Список объектов, которые разрешено строить/размещать во время рейдблока (PrefabName)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> RbDeployWhiteList;
        [JsonProperty(LanguageEn ? "List of prohibited commands when blocking is active [specify them without a slash (/) in lower case]" : "Список запрещенных команд при активной блокировки [указывайте их без слэша (/) в нижнем регистре]", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> RbBlackListCommand;
    }

    public class RaidBlockDetect
    {
        [JsonProperty(LanguageEn ? "List of items that will trigger a raid block upon destruction (prefabID)" : "Список предметов за которые при уничтожении будет даваться рейдблок (prefabID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public HashSet<uint> RbBlackList;
        [JsonProperty(LanguageEn ? "List of items that will not trigger a raid block upon destruction (prefabID)" : "Список предметов за которые при уничтожении не будет даваться рейдблок (prefabID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public HashSet<uint> RbWhiteList;
        [JsonProperty(LanguageEn ? "Ignore objects with a maximum health state less than N (0 - disabled)" : "Игнорировать объекты с максимальным состоянием здоровья меньше N (0 - отключено)")]
        public int IgnoreEntsHealth;
        [JsonProperty(LanguageEn ? "Activate raidblock upon the destruction of own or friends' buildings" : "Активировать рейдблок при уничтожении собственного строения или строения друзей")]
        public bool RaidYourself;
        [JsonProperty(LanguageEn ? "Activate raidblock if there is no tool cupboard in the building" : "Активировать рейдблок если в строении нет шкафа")]
        public bool CanRaidIfNotCupboard;
    }

    public class RaidBlock
    {
        [JsonProperty(LanguageEn ? "Radius of blocking zone (Meters)" : "Радиус зоны блокировки (Метры)")]
        public int RaidBlockDistance;
        [JsonProperty(LanguageEn ? "Blocking time (Seconds)" : "Время блокировки (Секунды)")]
        public int RaidBlockDuration;
        [JsonProperty(LanguageEn ? "Use dynamic raid zone (shift the zone center to the explosion location)" : "Использовать динамичную зону рейда (смещение центра зоны к месту взрыва)")]
        public bool IsDynamicRaidZone;
        [JsonProperty(LanguageEn ? "Spread raidblock to all team players" : "Распространять рейд блок на всех игроков в команде инициатора рейда")]
        public bool RaidBlockShareOnFriends;
        [JsonProperty(LanguageEn ? "When entering the active raid block zone, activate the player raid block" : "При входе в активную зону рейдблока - активировать рейдблок игроку")]
        public bool RaidBlockOnEnterRaidZone;
        [JsonProperty(LanguageEn ? "Upon exiting the raid block zone, deactivate the raid block for the player" : "При выходе из зоны рейдблока - деактивировать рейдблок игроку")]
        public bool RaidBlockOnExitRaidZone;
        [JsonProperty(LanguageEn ? "When exiting the raid block zone, leave a lock for N seconds (leave 0 if you don't need it)" : "При выходе из зоны рейдблока - оставлять N секунд блокировки (оставьте 0 - если вам не нужно это)")]
        public Int32 TimeLeftOnExitZone;
        [JsonProperty(LanguageEn ? "Activate raid block for all players within the effective radius after the raid starts" : "Активировать рейдблок для всех игроков в радиусе действия после начала рейда")]
        public bool RaidBlockAddedAllPlayersInZoneRaid;
        [JsonProperty(LanguageEn ? "Deactivate the raid block for the player upon death" : "Деактивировать рейдблок игроку после смерти")]
        public bool RaidBlockOnPlayerDeath;
        [JsonProperty(LanguageEn ? "Raid zone map marker settings" : "Настройки маркера на карте в зоне рейда")]
        public MapMarkerSettings mapMarkerSettings;
        [JsonProperty(LanguageEn ? "Visual raid zone (Dome) settings" : "Настройка визуальной зоны рейда (купол)")]
        public SphereSettings RaidZoneSphereSettings;
        [JsonProperty(LanguageEn ? "Integration settings with RaidableBases" : "Настройки интеграции с RaidableBases")]
        public RaidableBases RaidableBasesIntegration;
        public class SphereSettings
        {
            [JsonProperty(LanguageEn ? "Activate visual raid zone (Dome)" : "Активировать визуальную зону рейда (купол)")]
            public bool IsSphereEnabled;
            [JsonProperty(LanguageEn ? "Choose marker type: 0 - standard dome, 1 - BattleRoyale dome" : "Выберете тип купола : 0 - cтандартный купол, 1 - BattleRoyale купол")]
            public SphereTypes SphereType;
            [JsonProperty(LanguageEn ? "Color for BattleRoyale dome: 0 - blue | 1 - green | 2 - purple | 3 - red" : "Цвет для BattleRoyale купола : 0 - blue | 1 - green | 2 - purple | 3 - red")]
            public BRZoneColor BRZoneColor;
            [JsonProperty(LanguageEn ? "Transparency level of the standard dome (Lower values mean more transparency. The value should not exceed 5)" : "Уровень прозрачности стандартного купола (Чем меньше - тем прозрачнее. Значения должно быть не более 5)")]
            public int DomeTransparencyLevel;
            public string GetBRZonePrefab();
        }

        public class MapMarkerSettings
        {
            [JsonProperty(LanguageEn ? "Display the block zone on the G map" : "Отображать зону блокировки на G карте")]
            public bool IsRaidBlockMarkerEnabled;
            [JsonProperty(LanguageEn ? "Choose marker type: 0 - Explosion, 1 - Circle, 2 - Explosion + Circle, 3 - Circle + Timer" : "Выберите тип маркера: 0 - Explosion | 1 - Marker Radius | 2 - Explosion + Marker Radius | 3 - Marker Radius + Timer")]
            public MarkerTypes RaidBlockMarkerType;
            [JsonProperty(LanguageEn ? "Marker color (without #) (For marker types 1, 2, and 3)" : "Цвет маркера (без #) (Для маркера типа 1, 2 и 3)")]
            public string MarkerColorHex;
            [JsonProperty(LanguageEn ? "Outline color (without #) (For marker types 1, 2, and 3)" : "Цвет обводки (без #) (Для маркера типа 1, 2 и 3)")]
            public string OutlineColorHex;
            [JsonProperty(LanguageEn ? "Marker transparency (For marker types 1, 2, and 3)" : "Прозрачность маркера (Для маркера типа 1, 2 и 3)")]
            public float MarkerAlpha;
        }

    }

    public class RaidBlockUi
    {
        [JsonProperty(LanguageEn ? "Interface variant (0, 1, 2) - example: " : "Вариант интерфейса (0, 1, 2)")]
        public int UiType;
        [JsonProperty(LanguageEn ? "Interface layer: Overlay - will overlay other UI, Hud - will be overlaid by other interfaces" : "Слой интерфейса : Overlay - будет перекрывать другие UI, Hud - будет перекрываться другим интерфейсом")]
        public string Layers;
        [JsonProperty(LanguageEn ? "Vertical padding" : "Вертикальный отступ")]
        public int OffsetY;
        [JsonProperty(LanguageEn ? "Horizontal padding" : "Горизонтальный отступ")]
        public int OffsetX;
        [JsonProperty(LanguageEn ? "Interface settings for variant 0" : "Настройки интерфейса для варианта 0")]
        public RaidBlockUiSettings InterfaceSettingsVariant0;
        [JsonProperty(LanguageEn ? "Interface settings for variant 1" : "Настройки интерфейса для варианта 1")]
        public RaidBlockUiSettings InterfaceSettingsVariant1;
        [JsonProperty(LanguageEn ? "Interface settings for variant 2" : "Настройки интерфейса для варианта 2")]
        public RaidBlockUiSettings InterfaceSettingsVariant2;
        public class RaidBlockUiSettings
        {
            [JsonProperty(LanguageEn ? "Background color (RGBA)" : "Цвет фона (RGBA)")]
            public string BackgroundColor;
            [JsonProperty(LanguageEn ? "Icon color (RGBA)" : "Цвет иконки (RGBA)")]
            public string IconColor;
            [JsonProperty(LanguageEn ? "Color of additional elements (RGBA)" : "Цвет дополнительных элементов (RGBA)")]
            public string AdditionalElementsColor;
            [JsonProperty(LanguageEn ? "Main text color (RGBA)" : "Цвет основного текста (RGBA)")]
            public string MainTextColor;
            [JsonProperty(LanguageEn ? "Secondary text Color (RGBA)" : "Цвет второстепенного текста (RGBA)")]
            public string SecondaryTextColor;
            [JsonProperty(LanguageEn ? "Main color of the progress-bar (RGBA)" : "Основной цвет прогресс-бара (RGBA)")]
            public string ProgressBarMainColor;
            [JsonProperty(LanguageEn ? "Background Color of the Progress Bar (RGBA)" : "Цвет фона прогресс-бара (RGBA)")]
            public string ProgressBarBackgroundColor;
            [JsonProperty(LanguageEn ? "Delay before the UI appears and disappears (for smooth transitions)" : "Задержка перед появлением и исчезновением UI (для плавности)")]
            public float SmoothTransition;
        }

    }

    [JsonProperty(LanguageEn ? "Main raidblock settings" : "Основные настройки рейд-блока")]
    public RaidBlock RaidBlockMain;
    [JsonProperty(LanguageEn ? "Setting up triggers for the raidblock" : "Настройка триггеров для рейдблока")]
    public RaidBlockDetect BlockDetect;
    [JsonProperty(LanguageEn ? "Setting restrictions during the raid block" : "Настройка ограничений во время рейдблока")]
    public RaidBlockActionsBlocked ActionsBlocked;
    [JsonProperty(LanguageEn ? "Interface settings" : "Настройки интерфейса")]
    public RaidBlockUi RaidBlockInterface;
    [JsonProperty(LanguageEn ? "Setting IQChat" : "Настройка IQChat")]
    public IQChat IQChatSetting;
}

public class IQChat
{
    [JsonProperty(LanguageEn ? "IQChat : Custom prefix in the chat" : "IQChat : Кастомный префикс в чате")]
    public String CustomPrefix;
    [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
    public String CustomAvatar;
    [JsonProperty(LanguageEn ? "IQChat : Use UI notifications" : "IQChat : Использовать UI уведомления")]
    public Boolean UIAlertUse;
}

public class RaidableBases
{
    [JsonProperty(LanguageEn ? "RaidableBases: Enable support for Raidable Bases?" : "RaidableBases : Включить поддержку Raidable Bases ?")]
    public bool useRaidableBases;
    [JsonProperty(LanguageEn ? "RaidableBases: Blocking time (Seconds)" : "RaidableBases: Время блокировки (Секунды)")]
    public int RaidBlockDuration;
    [JsonProperty(LanguageEn ? "RaidableBases : Give a raid block when entering the Raidable Bases zone" : "RaidableBases : Давать рейд блок при входе в зону Raidable Bases")]
    public bool BlockOnEnterZone;
    [JsonProperty(LanguageEn ? "RaidableBases : Remove the raid block when leaving the Raidable Bases zone" : "RaidableBases : Снимать рейд блок при выходе из зоны Raidable Bases")]
    public bool BlockOnExitZone;
}

public class RaidBlockActionsBlocked
{
    [JsonProperty(LanguageEn ? "Forbid building repair" : "Запретить починку строений")]
    public bool CanRepairObjects;
    [JsonProperty(LanguageEn ? "Forbid picking up items (furnaces, boxes, etc.)" : "Запретить поднятие вещей (печки/ящики и т.д)")]
    public bool CanPickUpObjects;
    [JsonProperty(LanguageEn ? "Forbid upgrading buildings" : "Запретить улучшение строений")]
    public bool CanUpgradeObjects;
    [JsonProperty(LanguageEn ? "Forbid building removal" : "Запретить удаление строений")]
    public bool CanDemolishObjects;
    [JsonProperty(LanguageEn ? "Forbid teleportation" : "Запретить телепортацию")]
    public bool CanUseTeleport;
    [JsonProperty(LanguageEn ? "Forbid the use of kits" : "Запретить использование китов")]
    public bool CanUseKit;
    [JsonProperty(LanguageEn ? "Forbid the use of trade" : "Запретить использование обмена (Trade)")]
    public bool CanUseTrade;
    [JsonProperty(LanguageEn ? "Allow building" : "Запретить строение")]
    public bool CanBuildTwig;
    [JsonProperty(LanguageEn ? "Allow object placement (furnaces, boxes, etc.)" : "Запретить размещение объектов (Печки, ящики и другое)")]
    public bool CanDeployObjects;
    [JsonProperty(LanguageEn ? "List of objects allowed to build/place during the raidblock (shortname)" : "Список объектов, которые разрешено строить/размещать во время рейдблока (PrefabName)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> RbDeployWhiteList;
    [JsonProperty(LanguageEn ? "List of prohibited commands when blocking is active [specify them without a slash (/) in lower case]" : "Список запрещенных команд при активной блокировки [указывайте их без слэша (/) в нижнем регистре]", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> RbBlackListCommand;
}

public class RaidBlockDetect
{
    [JsonProperty(LanguageEn ? "List of items that will trigger a raid block upon destruction (prefabID)" : "Список предметов за которые при уничтожении будет даваться рейдблок (prefabID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public HashSet<uint> RbBlackList;
    [JsonProperty(LanguageEn ? "List of items that will not trigger a raid block upon destruction (prefabID)" : "Список предметов за которые при уничтожении не будет даваться рейдблок (prefabID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public HashSet<uint> RbWhiteList;
    [JsonProperty(LanguageEn ? "Ignore objects with a maximum health state less than N (0 - disabled)" : "Игнорировать объекты с максимальным состоянием здоровья меньше N (0 - отключено)")]
    public int IgnoreEntsHealth;
    [JsonProperty(LanguageEn ? "Activate raidblock upon the destruction of own or friends' buildings" : "Активировать рейдблок при уничтожении собственного строения или строения друзей")]
    public bool RaidYourself;
    [JsonProperty(LanguageEn ? "Activate raidblock if there is no tool cupboard in the building" : "Активировать рейдблок если в строении нет шкафа")]
    public bool CanRaidIfNotCupboard;
}

public class RaidBlock
{
    [JsonProperty(LanguageEn ? "Radius of blocking zone (Meters)" : "Радиус зоны блокировки (Метры)")]
    public int RaidBlockDistance;
    [JsonProperty(LanguageEn ? "Blocking time (Seconds)" : "Время блокировки (Секунды)")]
    public int RaidBlockDuration;
    [JsonProperty(LanguageEn ? "Use dynamic raid zone (shift the zone center to the explosion location)" : "Использовать динамичную зону рейда (смещение центра зоны к месту взрыва)")]
    public bool IsDynamicRaidZone;
    [JsonProperty(LanguageEn ? "Spread raidblock to all team players" : "Распространять рейд блок на всех игроков в команде инициатора рейда")]
    public bool RaidBlockShareOnFriends;
    [JsonProperty(LanguageEn ? "When entering the active raid block zone, activate the player raid block" : "При входе в активную зону рейдблока - активировать рейдблок игроку")]
    public bool RaidBlockOnEnterRaidZone;
    [JsonProperty(LanguageEn ? "Upon exiting the raid block zone, deactivate the raid block for the player" : "При выходе из зоны рейдблока - деактивировать рейдблок игроку")]
    public bool RaidBlockOnExitRaidZone;
    [JsonProperty(LanguageEn ? "When exiting the raid block zone, leave a lock for N seconds (leave 0 if you don't need it)" : "При выходе из зоны рейдблока - оставлять N секунд блокировки (оставьте 0 - если вам не нужно это)")]
    public Int32 TimeLeftOnExitZone;
    [JsonProperty(LanguageEn ? "Activate raid block for all players within the effective radius after the raid starts" : "Активировать рейдблок для всех игроков в радиусе действия после начала рейда")]
    public bool RaidBlockAddedAllPlayersInZoneRaid;
    [JsonProperty(LanguageEn ? "Deactivate the raid block for the player upon death" : "Деактивировать рейдблок игроку после смерти")]
    public bool RaidBlockOnPlayerDeath;
    [JsonProperty(LanguageEn ? "Raid zone map marker settings" : "Настройки маркера на карте в зоне рейда")]
    public MapMarkerSettings mapMarkerSettings;
    [JsonProperty(LanguageEn ? "Visual raid zone (Dome) settings" : "Настройка визуальной зоны рейда (купол)")]
    public SphereSettings RaidZoneSphereSettings;
    [JsonProperty(LanguageEn ? "Integration settings with RaidableBases" : "Настройки интеграции с RaidableBases")]
    public RaidableBases RaidableBasesIntegration;
    public class SphereSettings
    {
        [JsonProperty(LanguageEn ? "Activate visual raid zone (Dome)" : "Активировать визуальную зону рейда (купол)")]
        public bool IsSphereEnabled;
        [JsonProperty(LanguageEn ? "Choose marker type: 0 - standard dome, 1 - BattleRoyale dome" : "Выберете тип купола : 0 - cтандартный купол, 1 - BattleRoyale купол")]
        public SphereTypes SphereType;
        [JsonProperty(LanguageEn ? "Color for BattleRoyale dome: 0 - blue | 1 - green | 2 - purple | 3 - red" : "Цвет для BattleRoyale купола : 0 - blue | 1 - green | 2 - purple | 3 - red")]
        public BRZoneColor BRZoneColor;
        [JsonProperty(LanguageEn ? "Transparency level of the standard dome (Lower values mean more transparency. The value should not exceed 5)" : "Уровень прозрачности стандартного купола (Чем меньше - тем прозрачнее. Значения должно быть не более 5)")]
        public int DomeTransparencyLevel;
        public string GetBRZonePrefab();
    }

    public class MapMarkerSettings
    {
        [JsonProperty(LanguageEn ? "Display the block zone on the G map" : "Отображать зону блокировки на G карте")]
        public bool IsRaidBlockMarkerEnabled;
        [JsonProperty(LanguageEn ? "Choose marker type: 0 - Explosion, 1 - Circle, 2 - Explosion + Circle, 3 - Circle + Timer" : "Выберите тип маркера: 0 - Explosion | 1 - Marker Radius | 2 - Explosion + Marker Radius | 3 - Marker Radius + Timer")]
        public MarkerTypes RaidBlockMarkerType;
        [JsonProperty(LanguageEn ? "Marker color (without #) (For marker types 1, 2, and 3)" : "Цвет маркера (без #) (Для маркера типа 1, 2 и 3)")]
        public string MarkerColorHex;
        [JsonProperty(LanguageEn ? "Outline color (without #) (For marker types 1, 2, and 3)" : "Цвет обводки (без #) (Для маркера типа 1, 2 и 3)")]
        public string OutlineColorHex;
        [JsonProperty(LanguageEn ? "Marker transparency (For marker types 1, 2, and 3)" : "Прозрачность маркера (Для маркера типа 1, 2 и 3)")]
        public float MarkerAlpha;
    }

}

public class SphereSettings
{
    [JsonProperty(LanguageEn ? "Activate visual raid zone (Dome)" : "Активировать визуальную зону рейда (купол)")]
    public bool IsSphereEnabled;
    [JsonProperty(LanguageEn ? "Choose marker type: 0 - standard dome, 1 - BattleRoyale dome" : "Выберете тип купола : 0 - cтандартный купол, 1 - BattleRoyale купол")]
    public SphereTypes SphereType;
    [JsonProperty(LanguageEn ? "Color for BattleRoyale dome: 0 - blue | 1 - green | 2 - purple | 3 - red" : "Цвет для BattleRoyale купола : 0 - blue | 1 - green | 2 - purple | 3 - red")]
    public BRZoneColor BRZoneColor;
    [JsonProperty(LanguageEn ? "Transparency level of the standard dome (Lower values mean more transparency. The value should not exceed 5)" : "Уровень прозрачности стандартного купола (Чем меньше - тем прозрачнее. Значения должно быть не более 5)")]
    public int DomeTransparencyLevel;
    public string GetBRZonePrefab();
}

public class MapMarkerSettings
{
    [JsonProperty(LanguageEn ? "Display the block zone on the G map" : "Отображать зону блокировки на G карте")]
    public bool IsRaidBlockMarkerEnabled;
    [JsonProperty(LanguageEn ? "Choose marker type: 0 - Explosion, 1 - Circle, 2 - Explosion + Circle, 3 - Circle + Timer" : "Выберите тип маркера: 0 - Explosion | 1 - Marker Radius | 2 - Explosion + Marker Radius | 3 - Marker Radius + Timer")]
    public MarkerTypes RaidBlockMarkerType;
    [JsonProperty(LanguageEn ? "Marker color (without #) (For marker types 1, 2, and 3)" : "Цвет маркера (без #) (Для маркера типа 1, 2 и 3)")]
    public string MarkerColorHex;
    [JsonProperty(LanguageEn ? "Outline color (without #) (For marker types 1, 2, and 3)" : "Цвет обводки (без #) (Для маркера типа 1, 2 и 3)")]
    public string OutlineColorHex;
    [JsonProperty(LanguageEn ? "Marker transparency (For marker types 1, 2, and 3)" : "Прозрачность маркера (Для маркера типа 1, 2 и 3)")]
    public float MarkerAlpha;
}

public class RaidBlockUi
{
    [JsonProperty(LanguageEn ? "Interface variant (0, 1, 2) - example: " : "Вариант интерфейса (0, 1, 2)")]
    public int UiType;
    [JsonProperty(LanguageEn ? "Interface layer: Overlay - will overlay other UI, Hud - will be overlaid by other interfaces" : "Слой интерфейса : Overlay - будет перекрывать другие UI, Hud - будет перекрываться другим интерфейсом")]
    public string Layers;
    [JsonProperty(LanguageEn ? "Vertical padding" : "Вертикальный отступ")]
    public int OffsetY;
    [JsonProperty(LanguageEn ? "Horizontal padding" : "Горизонтальный отступ")]
    public int OffsetX;
    [JsonProperty(LanguageEn ? "Interface settings for variant 0" : "Настройки интерфейса для варианта 0")]
    public RaidBlockUiSettings InterfaceSettingsVariant0;
    [JsonProperty(LanguageEn ? "Interface settings for variant 1" : "Настройки интерфейса для варианта 1")]
    public RaidBlockUiSettings InterfaceSettingsVariant1;
    [JsonProperty(LanguageEn ? "Interface settings for variant 2" : "Настройки интерфейса для варианта 2")]
    public RaidBlockUiSettings InterfaceSettingsVariant2;
    public class RaidBlockUiSettings
    {
        [JsonProperty(LanguageEn ? "Background color (RGBA)" : "Цвет фона (RGBA)")]
        public string BackgroundColor;
        [JsonProperty(LanguageEn ? "Icon color (RGBA)" : "Цвет иконки (RGBA)")]
        public string IconColor;
        [JsonProperty(LanguageEn ? "Color of additional elements (RGBA)" : "Цвет дополнительных элементов (RGBA)")]
        public string AdditionalElementsColor;
        [JsonProperty(LanguageEn ? "Main text color (RGBA)" : "Цвет основного текста (RGBA)")]
        public string MainTextColor;
        [JsonProperty(LanguageEn ? "Secondary text Color (RGBA)" : "Цвет второстепенного текста (RGBA)")]
        public string SecondaryTextColor;
        [JsonProperty(LanguageEn ? "Main color of the progress-bar (RGBA)" : "Основной цвет прогресс-бара (RGBA)")]
        public string ProgressBarMainColor;
        [JsonProperty(LanguageEn ? "Background Color of the Progress Bar (RGBA)" : "Цвет фона прогресс-бара (RGBA)")]
        public string ProgressBarBackgroundColor;
        [JsonProperty(LanguageEn ? "Delay before the UI appears and disappears (for smooth transitions)" : "Задержка перед появлением и исчезновением UI (для плавности)")]
        public float SmoothTransition;
    }

}

public class RaidBlockUiSettings
{
    [JsonProperty(LanguageEn ? "Background color (RGBA)" : "Цвет фона (RGBA)")]
    public string BackgroundColor;
    [JsonProperty(LanguageEn ? "Icon color (RGBA)" : "Цвет иконки (RGBA)")]
    public string IconColor;
    [JsonProperty(LanguageEn ? "Color of additional elements (RGBA)" : "Цвет дополнительных элементов (RGBA)")]
    public string AdditionalElementsColor;
    [JsonProperty(LanguageEn ? "Main text color (RGBA)" : "Цвет основного текста (RGBA)")]
    public string MainTextColor;
    [JsonProperty(LanguageEn ? "Secondary text Color (RGBA)" : "Цвет второстепенного текста (RGBA)")]
    public string SecondaryTextColor;
    [JsonProperty(LanguageEn ? "Main color of the progress-bar (RGBA)" : "Основной цвет прогресс-бара (RGBA)")]
    public string ProgressBarMainColor;
    [JsonProperty(LanguageEn ? "Background Color of the Progress Bar (RGBA)" : "Цвет фона прогресс-бара (RGBA)")]
    public string ProgressBarBackgroundColor;
    [JsonProperty(LanguageEn ? "Delay before the UI appears and disappears (for smooth transitions)" : "Задержка перед появлением и исчезновением UI (для плавности)")]
    public float SmoothTransition;
}

private class RaidPlayer : FacepunchBehaviour
{
    public BasePlayer player;
    public Single blockEnds;
    public float UnblockTimeLeft { get; set; }
    private void Awake();
    public void Kill(Boolean force);
    private void OnDestroy();
    public void UpdateTime(Single time, Boolean customTime);
    public void ActivateBlock(Single time);
    private void CheckTimeLeft();
    public void CrateUI();
    private void RefreshUI();
}

private class RaidableZone : MonoBehaviour
{
    private Dictionary<BasePlayer, RaidPlayer> _playersAndComponentZone;
    private MapMarkerExplosion marker;
    private MapMarkerGenericRadius mapMarkerGenericRadius;
    private VendingMachineMapMarker vendingMakrer;
    private List<BaseEntity> _spheres;
    private SphereCollider triggerZone;
    private BasePlayer initiatorRaid;
    private Single timeToUnblock;
    private Single UnblockTimeLeft { get; set; }
    private Int32 raidBlockDistance;
    private Int32 raidBlockDuration;
    private bool IsDynamicRaidZone;
    private void CreateSphere();
    private void CreateMapMarker();
    private void CreateExplosionMapMarker();
    private void CreateVendingMapMarker();
    private void CreateGenericRadiusMapMarker();
    public void UpdateGenericRadiusMapMarker();
    private void UpdateZonePosition(Vector3 position);
    private void InitializeTriggerZone();
    public void CreateRaidZone(Vector3 raidPos, BasePlayer initiator);
    public void RefreshTimer(Vector3 pos, BasePlayer initiatorReply);
    private void EndRaid();
    private void AddAllPlayerInZoneDistance();
    private void AddAllPlayerFriendsInitiator(BasePlayer initiator);
    public void AddPlayer(BasePlayer player, Boolean force);
    public void RemovePlayer(BasePlayer player);
    public void RecheackRbZone(BasePlayer player);
    private void Awake();
    private void OnDestroy();
    private void OnTriggerEnter(Collider collider);
    private void OnTriggerExit(Collider collider);
}

private class InterfaceBuilder
{
    private static InterfaceBuilder _instance;
    public const string RB_MAIN;
    public const string RB_PROGRESS_BAR;
    public const string RB_PROGRESS;
    public const string RB_PROGRESS_TIMER;
    public static int TypeUi;
    public static double Factor;
    private Configuration.RaidBlockUi.RaidBlockUiSettings _uiSettings;
    private static float _fade;
    private Dictionary<string, string> _interfaces;
    public InterfaceBuilder();
    private static void AddInterface(string name, string json);
    public static string GetInterface(string name);
    public static void DestroyAll();
    private void BuildingRaidBlockMain();
    private void BuildingRaidBlockUpdated();
    private void BuildingRaidBlockMainV2();
    private void BuildingRaidBlockUpdatedV2();
    private void BuildingRaidBlockMainV3();
    private void BuildingRaidBlockUpdatedV3();
}

private class ImageUI
{
    private readonly string _paths;
    private readonly string _printPath;
    private readonly Dictionary<string, ImageData> _images;
    public ImageUI();
    private class ImageData
    {
        public ImageStatus Status;
        public string Id { get; set; }
    }

    public string GetImage(string name);
    public void DownloadImage();
    public void UnloadImages();
    private IEnumerator ProcessDownloadImage(KeyValuePair<string, ImageData> image);
}

private class ImageData
{
    public ImageStatus Status;
    public string Id { get; set; }
}

Oxide.Plugins.RaidBlockExt
public static class ExtensionMethods
{
    private static readonly Lang Lang;
    public static string GetAdaptedMessage(string langKey, string userID, object[] args);
    public static string ToTimeFormat(float source);
}


```

---

## RaidMarkers

```csharp
using System.Collections.Generic;
using Oxide.Core;
using UnityEngine;
using System;
using Rust;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Raid Markers", "KACAT КОРМИТ", "1.0.3")]
[Description("Raid Markers on the map")]
 class RaidMarkers : RustPlugin
{
    private const string permAllow;
    private Configuration config;
    private HashSet<MapMarkerGenericRadius> raidMarkers;
    private void Init();
    private void Unload();
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private object CanNetworkTo(MapMarkerGenericRadius marker, BasePlayer player);
    private void CreateRaidMarker(Vector3 position);
    private void ClearRaidMarkers();
    private bool IsFar(Vector3 position);
    private bool IsAuthed(BasePlayer player, BuildingPrivlidge buildingPrivlidge);
    private bool IsOnlineRaid(BuildingPrivlidge buildingPrivlidge);
    private double GetDistance(Vector3 pos1, Vector3 pos2);
    private Color ParseColor(string hexColor);
    public static string GetGridPosition(Vector3 position);
    [ChatCommand("rmtest")]
    private void cmdRaidMarker(BasePlayer player, string command, string[] args);
    private class Configuration
    {
        [JsonProperty(PropertyName = "Blaclisted prefabs")]
        public List<string> blacklistedPrefab;
        [JsonProperty(PropertyName = "Additional prefabs")]
        public List<string> additionalPrefab;
        [JsonProperty(PropertyName = "Distance when place new marker from another marker")]
        public int markerDistance;
        [JsonProperty("Enable write grid position to chat")]
        public bool chatGridPosition;
        [JsonProperty("Disable marker for authorized players in cupboard")]
        public bool authorizedPlayer;
        [JsonProperty("Create marker for online raid")]
        public bool showOnline;
        [JsonProperty("Create marker for offline raid")]
        public bool showOffline;
        [JsonProperty(PropertyName = "Marker configuration")]
        public MarkerConfiguration markerConfiguration;
        public VersionNumber version;
    }

    private class MarkerConfiguration
    {
        [JsonProperty(PropertyName = "Alpha")]
        public float markerAlpha;
        [JsonProperty(PropertyName = "Radius")]
        public float markerRadius;
        [JsonProperty(PropertyName = "Color1")]
        public string markerColor1;
        [JsonProperty(PropertyName = "Color2")]
        public string markerColor2;
        [JsonProperty(PropertyName = "Duration")]
        public float markerDuration;
    }

    private Configuration GetDefaultConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
    private void UpdateConfig();
    private string GetLang(string key, string playerID, object[] args);
    protected override void LoadDefaultMessages();
}

private class Configuration
{
    [JsonProperty(PropertyName = "Blaclisted prefabs")]
    public List<string> blacklistedPrefab;
    [JsonProperty(PropertyName = "Additional prefabs")]
    public List<string> additionalPrefab;
    [JsonProperty(PropertyName = "Distance when place new marker from another marker")]
    public int markerDistance;
    [JsonProperty("Enable write grid position to chat")]
    public bool chatGridPosition;
    [JsonProperty("Disable marker for authorized players in cupboard")]
    public bool authorizedPlayer;
    [JsonProperty("Create marker for online raid")]
    public bool showOnline;
    [JsonProperty("Create marker for offline raid")]
    public bool showOffline;
    [JsonProperty(PropertyName = "Marker configuration")]
    public MarkerConfiguration markerConfiguration;
    public VersionNumber version;
}

private class MarkerConfiguration
{
    [JsonProperty(PropertyName = "Alpha")]
    public float markerAlpha;
    [JsonProperty(PropertyName = "Radius")]
    public float markerRadius;
    [JsonProperty(PropertyName = "Color1")]
    public string markerColor1;
    [JsonProperty(PropertyName = "Color2")]
    public string markerColor2;
    [JsonProperty(PropertyName = "Duration")]
    public float markerDuration;
}


```

---

## RaidNotice

```csharp
using UnityEngine;
using System;
using System.Collections;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using System.IO;
using System.Reflection;
using System.Text;
using System.Linq.Expressions;
using Oxide.Core.Libraries;
using System.Linq;
using System.Globalization;

Oxide.Plugins
[Info("RaidNotice", "S1m0n", "1.0.0")]
[Description("RaidNotice for players")]
 class RaidNotice : RustPlugin
{
     string permissionvk;
     string permissionphone;
     string raidnotice;
     Dictionary<ulong, int> playersCodePhone;
     Dictionary<ulong, int> playersCodeVk;
     Dictionary<BasePlayer, float> lastTimeSended;
    public Dictionary<ulong, string> tempvk;
    public Dictionary<ulong, string> tempphone;
     List<string> allowedentity;
     class StoredData
    {
        public Dictionary<ulong, DateTime> raidCDVK;
        public Dictionary<ulong, DateTime> raidCDPHONE;
        public Dictionary<ulong, DateTime> msgVKCD;
        public Dictionary<ulong, DateTime> msgPHONECD;
        public Dictionary<ulong, int> addMAX;
        public Dictionary<ulong, string> vkids;
        public Dictionary<ulong, string> phones;
    }

     StoredData db;
     void SaveData();
     void Loaded();
     void Unloaded();
     void OnServerSave();
     void OnEntityDeath(BaseCombatEntity entity, HitInfo hitInfo);
    [ChatCommand("rn")]
     void rn(BasePlayer player, string command, string[] arg);
     void GetRequest(BasePlayer player, string id, string key, string device);
     void GetCallback(int code, string response, BasePlayer player, string id, string device);
     void SendMsg(BasePlayer player);
     bool isOnline(ulong id);
    public static BasePlayer FindOnlinePlayer(string nameOrIdOrIp);
     void SendOfflineMessage(ulong id);
     void Msg(BasePlayer player);
     void OnServerInitialized();
}

 class StoredData
{
    public Dictionary<ulong, DateTime> raidCDVK;
    public Dictionary<ulong, DateTime> raidCDPHONE;
    public Dictionary<ulong, DateTime> msgVKCD;
    public Dictionary<ulong, DateTime> msgPHONECD;
    public Dictionary<ulong, int> addMAX;
    public Dictionary<ulong, string> vkids;
    public Dictionary<ulong, string> phones;
}


```

---

## RaidProtector

```csharp
using System;
using Oxide.Core.Plugins;
using UnityEngine;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Game.Rust.Libraries;
using Oxide.Core;

Oxide.Plugins
[Info("RaidProtector", "Vlad-00003", "1.0.0")]
[Description("Decrese damage based on permissions, time and online state of the owner.")]
 class RaidProtector : RustPlugin
{
    private PluginConfig config;
    private Dictionary<BasePlayer, Timer> Informed;
    private class TimeConfig
    {
        [JsonProperty("Использовать защиту в указанный промежуток времени")]
        public bool UseTime;
        [JsonProperty("Час начала защиты")]
        public int Start;
        [JsonProperty("Час снятия защиты")]
        public int End;
    }

    private class PermisssionConfig
    {
        [JsonProperty("Множитель урона по постройкам")]
        public float modifier;
        [JsonProperty("Настройка времени")]
        public TimeConfig timeConfig;
        [JsonProperty("Защищать постройки когда игрок вне сети")]
        public bool Offline;
    }

    private class ProtectionSetup
    {
        [JsonProperty("Чат-команда для получения короткого имени префаба предмета, на который вы смотрите")]
        public string ChatCommand;
        [JsonProperty("Защищать строительные блоки(Стены, фундаменты, каркасы...)")]
        public bool BuildingBlock;
        [JsonProperty("Защищать двери(обычные, двойные, высокие)")]
        public bool Door;
        [JsonProperty("Защищать простые строительные блоки(высокие стены)")]
        public bool SimpleBuildingBlock;
        [JsonProperty("Список префабов, которые необходимо защищать(короткое или полное имя префаба)")]
        public List<string> Prefabs;
    }

    private class PluginConfig
    {
        [JsonProperty("Настройка привилегий")]
        public Dictionary<string, PermisssionConfig> Custom;
        [JsonProperty("Стандартные настройки для всех игроков")]
        public PermisssionConfig Default;
        [JsonProperty("Настройки защиты")]
        public ProtectionSetup Protection;
        [JsonProperty("Формат сообщений в чате")]
        public string ChatFormat;
        [JsonProperty("Задержка между сообщениями в чат о блокировке")]
        public float MessageCooldown;
        public static PluginConfig DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void LoadMessages();
    private string GetMsg(string key, BasePlayer player);
    private void GetShortName(BasePlayer player, string command, string[] args);
     void Loaded();
     void Unloaded();
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    private void Reply(BasePlayer player, string langkey, object[] args);
    private bool CheckEntity(BaseCombatEntity ent, HitInfo info);
    private bool CheckTime(PermisssionConfig perm);
    private bool CheckOffline(ulong UserID, PermisssionConfig perm);
    private PermisssionConfig GetPerm(ulong userID);
}

private class TimeConfig
{
    [JsonProperty("Использовать защиту в указанный промежуток времени")]
    public bool UseTime;
    [JsonProperty("Час начала защиты")]
    public int Start;
    [JsonProperty("Час снятия защиты")]
    public int End;
}

private class PermisssionConfig
{
    [JsonProperty("Множитель урона по постройкам")]
    public float modifier;
    [JsonProperty("Настройка времени")]
    public TimeConfig timeConfig;
    [JsonProperty("Защищать постройки когда игрок вне сети")]
    public bool Offline;
}

private class ProtectionSetup
{
    [JsonProperty("Чат-команда для получения короткого имени префаба предмета, на который вы смотрите")]
    public string ChatCommand;
    [JsonProperty("Защищать строительные блоки(Стены, фундаменты, каркасы...)")]
    public bool BuildingBlock;
    [JsonProperty("Защищать двери(обычные, двойные, высокие)")]
    public bool Door;
    [JsonProperty("Защищать простые строительные блоки(высокие стены)")]
    public bool SimpleBuildingBlock;
    [JsonProperty("Список префабов, которые необходимо защищать(короткое или полное имя префаба)")]
    public List<string> Prefabs;
}

private class PluginConfig
{
    [JsonProperty("Настройка привилегий")]
    public Dictionary<string, PermisssionConfig> Custom;
    [JsonProperty("Стандартные настройки для всех игроков")]
    public PermisssionConfig Default;
    [JsonProperty("Настройки защиты")]
    public ProtectionSetup Protection;
    [JsonProperty("Формат сообщений в чате")]
    public string ChatFormat;
    [JsonProperty("Задержка между сообщениями в чат о блокировке")]
    public float MessageCooldown;
    public static PluginConfig DefaultConfig();
}


```

---

## RaidZone

```csharp
using Oxide.Core;
using System.Collections.Generic;
using System.Linq;
using Rust;
using UnityEngine;
using System.Collections;
using System;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Random = UnityEngine.Random;
using Oxide.Core.Plugins;
using Newtonsoft.Json.Linq;
using System.Text.RegularExpressions;
using ru = Oxide.Game.Rust;

Oxide.Plugins
[Info("RaidZone", "fermens", "0.1.51")]
[Description("Рейблок по зонам")]
public class RaidZone : RustPlugin
{
    private static PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private static Dictionary<string, string> _names;
    private static string[] _spisok;
     class POSITION
    {
        [JsonProperty("Нулевая точка")]
        public string zero;
        [JsonProperty("offsetmax")]
        public string offsetmax;
        [JsonProperty("offsetmin")]
        public string offsetmin;
    }

     class GUI
    {
        [JsonProperty("Текст")]
        public string text;
        [JsonProperty("Цвет фона")]
        public string background;
        [JsonProperty("Цвет нижней полоски")]
        public string footline;
        [JsonProperty("Цвет текста")]
        public string colortext;
        [JsonProperty("Размер текста")]
        public string sizetext;
        [JsonProperty("Время капсом?")]
        public bool timeupper;
        [JsonProperty("Расположение")]
        public POSITION position;
    }

     class MARKER
    {
        [JsonProperty("Включить?")]
        public bool enable;
        [JsonProperty("Цвет маркера")]
        public string color1;
        [JsonProperty("Цвет обводки")]
        public string color2;
        [JsonProperty("Прозрачность")]
        public float alfa;
        [JsonProperty("Отображать круг?")]
        public bool circle;
        [JsonProperty("Отображать маркером взрыва?")]
        public bool boom;
    }

     class BLOCK
    {
        [JsonProperty("Телепорт")]
        public bool tp;
        [JsonProperty("Киты")]
        public bool kits;
        [JsonProperty("Трейд")]
        public bool trade;
        [JsonProperty("Строительство")]
        public bool build;
        [JsonProperty("Ремонт/улучшение/ремув - не плагином")]
        public bool ingame;
        [JsonProperty("Команды")]
        public string[] commands;
        [JsonProperty("Сообщение о блоке")]
        public string text;
        [JsonProperty("Можно строить/устанавливать во время блокировки [prefabId]")]
        public uint[] whitelist;
    }

     class VK
    {
        [JsonProperty("Включить?")]
        public bool enable;
        [JsonProperty("API от группы")]
        public string api;
        [JsonProperty("Текст")]
        public string text;
        [JsonProperty("Кд на отправку")]
        public float cooldown;
        [JsonProperty("Сообщение при входе игрока на сервер, при условии, что он не присоеденил свой вк")]
        public string message;
    }

     class Discord
    {
        [JsonProperty("Включить?")]
        public bool enable;
        [JsonProperty("Текст")]
        public string text;
        [JsonProperty("Кд на отправку")]
        public float cooldown;
    }

     class COMBATBLOCK
    {
        [JsonProperty("Включить?")]
        public bool enable;
        [JsonProperty("Блокировать при попадании по игроку?")]
        public bool damageto;
        [JsonProperty("Блокировать при получении урона от игрока?")]
        public bool damagefrom;
        [JsonProperty("Блокировать команды")]
        public string[] blacklist;
        [JsonProperty("Текст")]
        public string text;
        [JsonProperty("Время блокировки")]
        public float blockseconds;
        [JsonProperty("Включить GUI?")]
        public bool enablegui;
    }

     class GAME
    {
        [JsonProperty("Включить?")]
        public bool enable;
        [JsonProperty("Текст")]
        public string text;
        [JsonProperty("Кд на отправку")]
        public float cooldown;
    }

    private class PluginConfig
    {
        [JsonProperty("Время блокировки")]
        public int blockseconds;
        [JsonProperty("Название сервера - для оповещений")]
        public string servername;
        [JsonProperty("Радиус")]
        public float radius;
        [JsonProperty("Снимать блокировку если вышел из рейд-зоны?")]
        public bool blockremove;
        [JsonProperty("Сброс рейдблока при смерти?")]
        public bool removedeath;
        [JsonProperty("Рейдблок установливается даже если на территории нет шкафа?")]
        public bool cupboard;
        [JsonProperty("Настройка маркера на карте")]
        public MARKER marker;
        [JsonProperty("Настройка GUI")]
        public GUI gui;
        [JsonProperty("Настройка блокировки")]
        public BLOCK block;
        [JsonProperty("Команда")]
        public string command;
        [JsonProperty("Настройка комбатблока")]
        public COMBATBLOCK combatblock;
        [JsonProperty("Оповещение о рейде в игре")]
        public GAME GAME;
        [JsonProperty("Оповещание о рейде в ВК")]
        public VK vk;
        [JsonProperty("Оповещание о рейде в Дискорд")]
        public Discord discord;
        [JsonProperty("Сообщения")]
        public Dictionary<MES, string> messages;
        [JsonProperty("Названия - для оповещаний")]
        public Dictionary<string, string> names;
        [JsonProperty("Дополнительный список на что кидать РБ")]
        public string[] spisok;
        public static PluginConfig DefaultConfig();
    }

    private object OnServerCommand(ConsoleSystem.Arg arg);
    private object OnUserCommand(IPlayer ipplayer, string com, string[] args);
    private object blocker(BasePlayer player, string command);
    private static List<ulong> combatblock;
    private bool HasCombatBlock(BasePlayer player);
    private void OnPlayerAttack(BasePlayer attacker, HitInfo info);
    private void ADDCOMBATBLOCK(BasePlayer player, int time, bool raidblock);
     class COMBATBK : MonoBehaviour
    {
         BasePlayer player;
        public int tick;
        public bool raidblock;
        private void Awake();
        public void ADDRAID();
        private void TICK();
        public void DoDestroy();
        private void OnDestroy();
    }

     Dictionary<BasePlayer, GameObject> disconnected;
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private static Dictionary<string, Vector3> Grids;
    private void CreateSpawnGrid();
    private string GetNameGrid(Vector3 pos);
    [ConsoleCommand("vkintegra")]
    private void Cmdvkintegra(ConsoleSystem.Arg arg);
    private static RaidZone ins;
    private void Init();
    private void OnServerInitialized();
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
    [PluginReference]
    private Plugin DiscordCore;
    private Plugin HaxBot;
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info, Item item);
    private IEnumerator GORAID(BaseCombatEntity entity, BasePlayer player, int tt);
     class ALERT
    {
        public DateTime gamecooldown;
        public DateTime discordcooldown;
        public DateTime vkcooldown;
        public DateTime vkcodecooldown;
    }

    private static Dictionary<ulong, ALERT> alerts;
    private static Dictionary<ulong, string> VkPlayers;
    private void ALERTPLAYER(ulong ID, string name, string quad, string connect, string destroy);
    private object CanBuild(Planner plan, Construction prefab);
    private void CreateTrigger(Vector3 position, int time);
     class CODE
    {
        public string id;
        public ulong gameid;
    }

    private static Dictionary<string, CODE> VKCODES;
    private void callcommandrn(BasePlayer player, string command, string[] arg);
    private void GetRequest(string reciverID, string msg, BasePlayer player, string num);
    private IEnumerator GetCallback(int code, string response, string id, BasePlayer player, string num);
    private const string permvk;
    private const string genericPrefab;
    private const string raidPrefab;
    private static string GUIJSON;
    private static string GUITEXT;
    private static Color COLOR1;
    private static Color COLOR2;
    private Dictionary<ulong, GameObject> IsBlock;
    private static List<MapMarkerGenericRadius> mapMarkerGenericRadii;
     class ZONE : MonoBehaviour
    {
        private MapMarkerGenericRadius generic;
        private MapMarkerExplosion explosion;
        private SphereCollider sphere;
        private List<Network.Connection> ZONEPLAYERS;
        public int seconds;
         void Awake();
        public void START(float radius, int time);
        public void Refresh(int time);
        private void OneSecond();
        public void AddPlayer(BasePlayer player);
        public void RemovePlayer(BasePlayer player, bool newzone);
        private void CHECKNEWZONE(BasePlayer player);
        public void DoDestroy();
        private void OnDestroy();
    }

    private void OnEntityEnter(TriggerBase trigger, BaseEntity entity);
    private void OnEntityLeave(TriggerBase trigger, BaseEntity entity);
    private bool HasBlock(ulong ID);
    private bool HasBlockTera(Vector3 position);
    private string CanTeleport(BasePlayer player);
    private string canTeleport(BasePlayer player);
    private int? CanBGrade(BasePlayer player, int grade, BuildingBlock block, Planner plan);
    private string CanTrade(BasePlayer player);
    private string canRemove(BasePlayer player);
     object canRedeemKit(BasePlayer player);
    private bool? CanAffordUpgrade(BasePlayer player, BuildingBlock block, BuildingGrade.Enum grade);
    private object OnStructureUpgrade(BuildingBlock block, BasePlayer player, BuildingGrade.Enum grade);
    private bool? OnStructureRepair(BaseCombatEntity entity, BasePlayer player);
     object OnStructureDemolish(BaseCombatEntity entity, BasePlayer player);
    private static ZONE GETZONE(Vector3 position);
    private static void RemoveGeneric(MapMarkerGenericRadius mapMarker);
    private string RANDOMNUM();
    private void CanDismountEntity(BasePlayer player, BaseMountable entity);
    private void SaveVK();
    private void Unload();
    private void OnPlayerSleepEnded(BasePlayer player);
    private void OnPlayerConnected(BasePlayer player);
    private static bool IsNPC(BasePlayer player);
    private static string m0;
    private static string m1;
    private static string m2;
    private static string s0;
    private static string s1;
    private static string s2;
    private static string FormatTime(TimeSpan time);
    private static string FormatMinutes(int minutes);
    private static string FormatSeconds(int seconds);
    private static string FormatUnits(int units, string form1, string form2, string form3);
}

 class POSITION
{
    [JsonProperty("Нулевая точка")]
    public string zero;
    [JsonProperty("offsetmax")]
    public string offsetmax;
    [JsonProperty("offsetmin")]
    public string offsetmin;
}

 class GUI
{
    [JsonProperty("Текст")]
    public string text;
    [JsonProperty("Цвет фона")]
    public string background;
    [JsonProperty("Цвет нижней полоски")]
    public string footline;
    [JsonProperty("Цвет текста")]
    public string colortext;
    [JsonProperty("Размер текста")]
    public string sizetext;
    [JsonProperty("Время капсом?")]
    public bool timeupper;
    [JsonProperty("Расположение")]
    public POSITION position;
}

 class MARKER
{
    [JsonProperty("Включить?")]
    public bool enable;
    [JsonProperty("Цвет маркера")]
    public string color1;
    [JsonProperty("Цвет обводки")]
    public string color2;
    [JsonProperty("Прозрачность")]
    public float alfa;
    [JsonProperty("Отображать круг?")]
    public bool circle;
    [JsonProperty("Отображать маркером взрыва?")]
    public bool boom;
}

 class BLOCK
{
    [JsonProperty("Телепорт")]
    public bool tp;
    [JsonProperty("Киты")]
    public bool kits;
    [JsonProperty("Трейд")]
    public bool trade;
    [JsonProperty("Строительство")]
    public bool build;
    [JsonProperty("Ремонт/улучшение/ремув - не плагином")]
    public bool ingame;
    [JsonProperty("Команды")]
    public string[] commands;
    [JsonProperty("Сообщение о блоке")]
    public string text;
    [JsonProperty("Можно строить/устанавливать во время блокировки [prefabId]")]
    public uint[] whitelist;
}

 class VK
{
    [JsonProperty("Включить?")]
    public bool enable;
    [JsonProperty("API от группы")]
    public string api;
    [JsonProperty("Текст")]
    public string text;
    [JsonProperty("Кд на отправку")]
    public float cooldown;
    [JsonProperty("Сообщение при входе игрока на сервер, при условии, что он не присоеденил свой вк")]
    public string message;
}

 class Discord
{
    [JsonProperty("Включить?")]
    public bool enable;
    [JsonProperty("Текст")]
    public string text;
    [JsonProperty("Кд на отправку")]
    public float cooldown;
}

 class COMBATBLOCK
{
    [JsonProperty("Включить?")]
    public bool enable;
    [JsonProperty("Блокировать при попадании по игроку?")]
    public bool damageto;
    [JsonProperty("Блокировать при получении урона от игрока?")]
    public bool damagefrom;
    [JsonProperty("Блокировать команды")]
    public string[] blacklist;
    [JsonProperty("Текст")]
    public string text;
    [JsonProperty("Время блокировки")]
    public float blockseconds;
    [JsonProperty("Включить GUI?")]
    public bool enablegui;
}

 class GAME
{
    [JsonProperty("Включить?")]
    public bool enable;
    [JsonProperty("Текст")]
    public string text;
    [JsonProperty("Кд на отправку")]
    public float cooldown;
}

private class PluginConfig
{
    [JsonProperty("Время блокировки")]
    public int blockseconds;
    [JsonProperty("Название сервера - для оповещений")]
    public string servername;
    [JsonProperty("Радиус")]
    public float radius;
    [JsonProperty("Снимать блокировку если вышел из рейд-зоны?")]
    public bool blockremove;
    [JsonProperty("Сброс рейдблока при смерти?")]
    public bool removedeath;
    [JsonProperty("Рейдблок установливается даже если на территории нет шкафа?")]
    public bool cupboard;
    [JsonProperty("Настройка маркера на карте")]
    public MARKER marker;
    [JsonProperty("Настройка GUI")]
    public GUI gui;
    [JsonProperty("Настройка блокировки")]
    public BLOCK block;
    [JsonProperty("Команда")]
    public string command;
    [JsonProperty("Настройка комбатблока")]
    public COMBATBLOCK combatblock;
    [JsonProperty("Оповещение о рейде в игре")]
    public GAME GAME;
    [JsonProperty("Оповещание о рейде в ВК")]
    public VK vk;
    [JsonProperty("Оповещание о рейде в Дискорд")]
    public Discord discord;
    [JsonProperty("Сообщения")]
    public Dictionary<MES, string> messages;
    [JsonProperty("Названия - для оповещаний")]
    public Dictionary<string, string> names;
    [JsonProperty("Дополнительный список на что кидать РБ")]
    public string[] spisok;
    public static PluginConfig DefaultConfig();
}

 class COMBATBK : MonoBehaviour
{
     BasePlayer player;
    public int tick;
    public bool raidblock;
    private void Awake();
    public void ADDRAID();
    private void TICK();
    public void DoDestroy();
    private void OnDestroy();
}

 class ALERT
{
    public DateTime gamecooldown;
    public DateTime discordcooldown;
    public DateTime vkcooldown;
    public DateTime vkcodecooldown;
}

 class CODE
{
    public string id;
    public ulong gameid;
}

 class ZONE : MonoBehaviour
{
    private MapMarkerGenericRadius generic;
    private MapMarkerExplosion explosion;
    private SphereCollider sphere;
    private List<Network.Connection> ZONEPLAYERS;
    public int seconds;
     void Awake();
    public void START(float radius, int time);
    public void Refresh(int time);
    private void OneSecond();
    public void AddPlayer(BasePlayer player);
    public void RemovePlayer(BasePlayer player, bool newzone);
    private void CHECKNEWZONE(BasePlayer player);
    public void DoDestroy();
    private void OnDestroy();
}


```

---

## RainbowName

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Random = System.Random;

Oxide.Plugins
[Info("Rainbow Name", "sami37", "1.0.3")]
[Description("Set your vip a special color name")]
public class RainbowName : RustPlugin
{
    [PluginReference]
     Plugin BetterChat;
    private string colors;
    private List<string> colorsList;
    private Random rand;
    private DynamicConfigFile PDATA;
     PlayersData pdata;
    public class PlayersData
    {
        public Dictionary<string, bool> StatePlayers;
    }

     void Init();
    protected override void LoadDefaultConfig();
     void Loaded();
     string ListToString(List<T> list, int first, string seperator);
     void SetConfig(object[] args);
     T GetConfig(T defaultVal, object[] args);
     void LoadConfig();
     bool BetterChatIns();
     object OnPlayerChat(ConsoleSystem.Arg arg);
    private object OnBetterChat(Dictionary<string, object> data);
    private string StripRichText(string text);
    [ChatCommand("rn")]
     void CmdChat(BasePlayer player, string cmd, string[] args);
}

public class PlayersData
{
    public Dictionary<string, bool> StatePlayers;
}


```

---

## RainOfFire

```csharp
using System.Collections.Generic;
using Oxide.Core.Plugins;
using UnityEngine;
using Rust;

Oxide.Plugins
[Info("RainOfFire", "emu / k1lly0u", "0.2.11", ResourceId = 1249)]
 class RainOfFire : RustPlugin
{
    [PluginReference]
     Plugin PopupNotifications;
    const string s_Incoming;
    private Timer EventTimer;
    private List<Timer> RocketTimers;
    private float launchHeight;
    private float fireRocketChance;
    private float launchStraightness;
    private float projectileSpeed;
    private float gravityModifier;
    private float detonationTime;
     void OnServerInitialized();
     void Unload();
    private void StartEventTimer();
    private void StopTimer();
    private void StartRandomOnMap();
    private bool StartOnPlayer(string playerName, Settings setting);
    private void StartBarrage(Vector3 origin, Vector3 direction);
    private void StartRainOfFire(Vector3 origin, Settings setting);
    private void RandomRocket(Vector3 origin, float radius, Settings setting);
    private void SpreadRocket(Vector3 origin, Vector3 direction);
    private BaseEntity CreateRocket(Vector3 startPoint, Vector3 direction, bool isFireRocket);
    private void ScaleAllDamage(List<DamageTypeEntry> damageTypes, float scale);
    private void SetIntervals(int intervals);
    private void SetDamageMult(float scale);
    private void SetNotifyEvent(bool notify);
    private void SetDropRate(float rate);
    [ChatCommand("rof")]
    private void cmdROF(BasePlayer player, string command, string[] args);
    [ConsoleCommand("rof.random")]
    private void ccmdEventRandom(ConsoleSystem.Arg arg);
    [ConsoleCommand("rof.onposition")]
    private void ccmdEventOnPosition(ConsoleSystem.Arg arg);
    private BasePlayer GetPlayerByName(string name);
    static int GetRandom(int min, int max);
    private float MapSize();
    private ItemDefinition GetRocket();
    private ItemDefinition GetFireRocket();
    static Vector3 GetGroundPosition(Vector3 sourcePos);
     class ItemCarrier : MonoBehaviour
    {
        private ItemDrop[] carriedItems;
        private float multiplier;
        public void SetCarriedItems(ItemDrop[] carriedItems);
        public void SetDropMultiplier(float multiplier);
        private void OnDestroy();
    }

     class ItemDrop
    {
        public string Shortname;
        public int Minimum;
        public int Maximum;
    }

    private ConfigData configData;
     class Damage
    {
        public float DamageMultiplier { get; set; }
    }

     class Barrage
    {
        public int NumberOfRockets { get; set; }
        public float RocketDelay { get; set; }
        public float RocketSpread { get; set; }
    }

     class Drops
    {
        public bool EnableItemDrop { get; set; }
        public ItemDrop[] ItemsToDrop { get; set; }
    }

     class Options
    {
        public bool EnableAutomaticEvents { get; set; }
        public Timers EventTimers { get; set; }
        public float GlobalDropMultiplier { get; set; }
        public bool NotifyEvent { get; set; }
    }

     class Timers
    {
        public int EventInterval { get; set; }
        public bool UseRandomTimer { get; set; }
        public int RandomTimerMin { get; set; }
        public int RandomTimerMax { get; set; }
    }

     class Settings
    {
        public int FireRocketChance { get; set; }
        public float Radius { get; set; }
        public int RocketAmount { get; set; }
        public int Duration { get; set; }
        public Drops ItemDropControl { get; set; }
    }

     class Intensity
    {
        public Settings Settings_Mild { get; set; }
        public Settings Settings_Optimal { get; set; }
        public Settings Settings_Extreme { get; set; }
    }

     class ConfigData
    {
        public Barrage BarrageSettings { get; set; }
        public Damage DamageControl { get; set; }
        public Options Options { get; set; }
        public Intensity z_IntensitySettings { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
}

 class ItemCarrier : MonoBehaviour
{
    private ItemDrop[] carriedItems;
    private float multiplier;
    public void SetCarriedItems(ItemDrop[] carriedItems);
    public void SetDropMultiplier(float multiplier);
    private void OnDestroy();
}

 class ItemDrop
{
    public string Shortname;
    public int Minimum;
    public int Maximum;
}

 class Damage
{
    public float DamageMultiplier { get; set; }
}

 class Barrage
{
    public int NumberOfRockets { get; set; }
    public float RocketDelay { get; set; }
    public float RocketSpread { get; set; }
}

 class Drops
{
    public bool EnableItemDrop { get; set; }
    public ItemDrop[] ItemsToDrop { get; set; }
}

 class Options
{
    public bool EnableAutomaticEvents { get; set; }
    public Timers EventTimers { get; set; }
    public float GlobalDropMultiplier { get; set; }
    public bool NotifyEvent { get; set; }
}

 class Timers
{
    public int EventInterval { get; set; }
    public bool UseRandomTimer { get; set; }
    public int RandomTimerMin { get; set; }
    public int RandomTimerMax { get; set; }
}

 class Settings
{
    public int FireRocketChance { get; set; }
    public float Radius { get; set; }
    public int RocketAmount { get; set; }
    public int Duration { get; set; }
    public Drops ItemDropControl { get; set; }
}

 class Intensity
{
    public Settings Settings_Mild { get; set; }
    public Settings Settings_Optimal { get; set; }
    public Settings Settings_Extreme { get; set; }
}

 class ConfigData
{
    public Barrage BarrageSettings { get; set; }
    public Damage DamageControl { get; set; }
    public Options Options { get; set; }
    public Intensity z_IntensitySettings { get; set; }
}


```

---

## RandomCases

```csharp
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Drawing;
using System.Drawing.Imaging;
using System.IO;
using System.Linq;
using UnityEngine;
using System.Diagnostics;

Oxide.Plugins
[Info("Random Cases", "OxideBro", "0.1.11")]
public class RandomCases : RustPlugin
{
    [PluginReference]
     Plugin Duel;
    private bool IsDuelPlayer(BasePlayer player);
    static RandomCases instance;
    public class CaseDefinition
    {
        public string Type;
        public string Name;
        public string Images;
        public string Description;
        public int CoolDown;
        public List<CaseItem> Items;
        public CaseItem Open();
    }

    public class CaseItem
    {
        public string Shortname;
        public int Min;
        public int Max;
        public int GetRandom();
    }

    public class Case
    {
        public string Type;
        public int CoolDown;
        public int Amount;
        public CaseDefinition info();
    }

    public class CasePlayer
    {
        public List<Case> CasesQueue;
        public List<string> Inventory;
        public bool giftcases;
        public void OnTimer(ulong steamid, int delay);
    }

    public Dictionary<string, CaseDefinition> cases;
    public Dictionary<ulong, CasePlayer> players;
     List<string> CasesList;
    const int TIMEOUT;
     bool init;
    private bool StartGiftCases;
     string cmdChatCommand;
     bool enabledGiftCase;
     int giftcaseAmount;
     string imagesServerLogo;
     string Eventpicture;
    private float EndTime;
    private bool StartEventEnabled;
    private float MinTimeEvent;
    private float MaxTimeEvent;
    private int MinAMounts;
    private int MaxAMounts;
    private void LoadConfigValues();
    private bool GetConfig(string MainMenu, string Key, T var);
     void OnServerInitialized();
     void SaveData();
     void Unload();
     void OnServerSave();
     void OnPlayerInit(BasePlayer player);
     void PlayerCheck(BasePlayer player, int Current, Case ccase2);
     void OnPlayerSleepEnded(BasePlayer player);
    [ConsoleCommand("casegive")]
     void cmdRandomCaseGive(ConsoleSystem.Arg arg);
    [ConsoleCommand("casedrop")]
     void cmdDropUser(ConsoleSystem.Arg arg);
     void cmdChatCase(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("nextpage.case2")]
     void cmdCloseMenu1(ConsoleSystem.Arg arg);
    [ConsoleCommand("nextpage.case1")]
     void cmdCloseMenu0(ConsoleSystem.Arg arg);
    [ConsoleCommand("drawhelp")]
     void cmdHelpMenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("drawcasesbonus")]
     void cmdOpenBonus(ConsoleSystem.Arg arg);
    [ConsoleCommand("bonusDisabled")]
     void cmdOpenDisabled(ConsoleSystem.Arg arg);
    [ConsoleCommand("randomcase.open")]
     void cmdOpenCase(ConsoleSystem.Arg arg);
     List<ulong> activatePlayers;
    [ConsoleCommand("randomcase.giverandom")]
     void cmdGiveCasesRandom(ConsoleSystem.Arg arg);
    [ConsoleCommand("openplayercase")]
     void cmdOpenCaseTest(ConsoleSystem.Arg arg);
    [ConsoleCommand("casesmenuclose")]
     void cmdCloseMenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("caseses")]
     void cmdMenuStart(ConsoleSystem.Arg arg);
    private double nextTrigger;
    private Timer nextEvent;
    private Timer mytimer;
    private double GrabCurrentTime();
    [ConsoleCommand("start.random")]
     void cmdStartEvent(ConsoleSystem.Arg arg);
    private void StartEventCases();
    private void StartRandomCase();
     void TimerHandle();
     bool GiveCase(ulong userID, string ccase, Case ccase1, int amount, string values);
     void GiveCaseGift(ulong userID, string ccase, int amount);
     bool OpenCase(BasePlayer player, string ccase);
     bool CanTake(BasePlayer player);
    public static string FormatTime(TimeSpan time);
    private static string Format(int units, string form1, string form2, string form3);
     string giftpng;
     string imagesccase;
     string GUI;
     string GUIInfo;
     string GUIBuyCases;
     string BonusParrent;
     string GiveRandomCase;
     string Button;
     string ButtonOff;
     string GUIHelp;
     void DrawHelp(BasePlayer player);
     void DrawButton(BasePlayer player);
     string HandleArgs(string json, object[] args);
     void DrawCasesParrent(BasePlayer player);
     void DrawCasesGive(BasePlayer player, string ccase, int amount);
     void DrawCaseInfo(BasePlayer player, string ccase1);
     void DrawCases(BasePlayer player, int page);
    private void InitilizeKitImageUI(CuiElementContainer container, string ccase);
     DynamicConfigFile players_File;
     void LoadData();
    private GameObject FileManagerObject;
    private FileManager m_FileManager;
     void InitFileManager();
     class FileManager : MonoBehaviour
    {
         int loaded;
         int needed;
        public bool IsFinished { get; set; }
        const ulong MaxActiveLoads;
         Dictionary<string, FileInfo> files;
        private class FileInfo
        {
            public string Url;
            public string Png;
        }

        public string GetPng(string name);
        public IEnumerator LoadFile(string name, string url, int size);
         IEnumerator LoadImageCoroutine(string name, string url, int size);
        static byte[] Resize(byte[] bytes, int size);
    }

     Dictionary<string, string> Messages;
}

public class CaseDefinition
{
    public string Type;
    public string Name;
    public string Images;
    public string Description;
    public int CoolDown;
    public List<CaseItem> Items;
    public CaseItem Open();
}

public class CaseItem
{
    public string Shortname;
    public int Min;
    public int Max;
    public int GetRandom();
}

public class Case
{
    public string Type;
    public int CoolDown;
    public int Amount;
    public CaseDefinition info();
}

public class CasePlayer
{
    public List<Case> CasesQueue;
    public List<string> Inventory;
    public bool giftcases;
    public void OnTimer(ulong steamid, int delay);
}

 class FileManager : MonoBehaviour
{
     int loaded;
     int needed;
    public bool IsFinished { get; set; }
    const ulong MaxActiveLoads;
     Dictionary<string, FileInfo> files;
    private class FileInfo
    {
        public string Url;
        public string Png;
    }

    public string GetPng(string name);
    public IEnumerator LoadFile(string name, string url, int size);
     IEnumerator LoadImageCoroutine(string name, string url, int size);
    static byte[] Resize(byte[] bytes, int size);
}

private class FileInfo
{
    public string Url;
    public string Png;
}


```

---

## RandomDeployables

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("RandomDeployables", "Norn", 0.5, ResourceId = 2187)]
[Description("Randomize deployable skins")]
 class RandomDeployables : RustPlugin
{
     void Loaded();
    private static Dictionary<string, int> deployedToItem;
    private static List<ulong> SkinList;
    private void InitializeTable();
    protected override void LoadDefaultConfig();
    private List<int> GetSkins(ItemDefinition def);
    private void OnEntityBuilt(Planner planner, GameObject gameObject);
}


```

---

## RandomSpawner

```csharp
using System.Collections.Generic;
using System.Linq;
using System;
using UnityEngine;

Oxide.Plugins
[Info("Random Spawner", "LaserHydra", "1.0.0", ResourceId = 0)]
[Description("Randomly Spawn a specific amount of an entity on the map")]
 class RandomSpawner : RustPlugin
{
     void Loaded();
    [ConsoleCommand("getprefabs")]
     void ccmdGetPrefabs(ConsoleSystem.Arg arg);
    [ConsoleCommand("rspawn")]
     void ccmdSpawnRandom(ConsoleSystem.Arg arg);
    [ChatCommand("rspawn")]
     void cmdSpawnRandom(BasePlayer player, string cmd, string[] args);
    private void SpawnEntity(string prefab, Vector3 location);
     Vector3 GetRandomVector();
     object GetTerrainHeight(Vector3 location);
     void Teleport(BasePlayer player, Vector3 destination);
     void RunAsChatCommand(ConsoleSystem.Arg arg, Action<BasePlayer, string, string[]> command);
     string ListToString(List<string> list, int first, string seperator);
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}


```

---

## RandomSpawns

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("RandomSpawns", "ReCoDeX", "0.0.2", ResourceId = 0)]
 class RandomSpawns : RustPlugin
{
    private bool isDisabled;
    private int spawnCount;
    private Hash<BiomeType, List<Vector3>> spawnPoints;
    private int minPlayerReq;
     System.Random random;
    [ChatCommand("showspawns")]
    private void cmdShowSpawns(BasePlayer player, string command, string[] args);
    private void OnServerInitialized();
    private object OnPlayerRespawn(BasePlayer player);
    private BiomeType GetMajorityBiome(Vector3 position);
    private void CalculateMinimumPlayers();
    private object GetSpawnPoint(bool ignorePlayerRestriction);
    private void GenerateSpawnpoints();
    private Vector3 CalculatePoint(Vector3 position, float max);
    private object FindNewPosition(Vector3 position, float max, bool failed);
    private object ProcessRay(RaycastHit hitInfo);
    private RaycastHit RayPosition(Vector3 sourcePos);
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Biome Options")]
        public Dictionary<BiomeType, BiomeOptions> SpawnAreas { get; set; }
        public class BiomeOptions
        {
            [JsonProperty(PropertyName = "Enable spawn points to be generated in this biome")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Minimum required online players before spawns from this biome will be selected")]
            public int Players { get; set; }
        }

    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Biome Options")]
    public Dictionary<BiomeType, BiomeOptions> SpawnAreas { get; set; }
    public class BiomeOptions
    {
        [JsonProperty(PropertyName = "Enable spawn points to be generated in this biome")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Minimum required online players before spawns from this biome will be selected")]
        public int Players { get; set; }
    }

}

public class BiomeOptions
{
    [JsonProperty(PropertyName = "Enable spawn points to be generated in this biome")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Minimum required online players before spawns from this biome will be selected")]
    public int Players { get; set; }
}


```

---

## RandomWarps

```csharp
using System.Collections.Generic;
using UnityEngine;
using System.Linq;
using System;
using Oxide.Core;

Oxide.Plugins
[Info("Random Warps", "LaserHydra", "1.1.2", ResourceId = 1397)]
[Description("Teleports you to a random location of a multi-location warp")]
 class RandomWarps : RustPlugin
{
     string pluginColor;
     class Data
    {
        public Dictionary<string, List<Dictionary<char, float>>> warps;
        public Data();
    }

     Data data;
     void LoadData();
     void SaveData();
     Vector3 GetRandom(List<Dictionary<char, float>> warpPositions);
     void Loaded();
     void LoadConfig();
    protected override void LoadDefaultConfig();
    [ChatCommand("rwarp")]
     void rWarp(BasePlayer player, string cmd, string[] args);
     bool IsAdmin(BasePlayer player);
    public void Teleport(BasePlayer player, Vector3 pos);
     BasePlayer GetPlayer(string searchedPlayer, BasePlayer executer, string prefix);
     string ListToString(List<string> list, int first, string seperator);
     void SetConfig(string Arg1, object Arg2, object Arg3, object Arg4);
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}

 class Data
{
    public Dictionary<string, List<Dictionary<char, float>>> warps;
    public Data();
}


```

---

## RankMe

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using System.Globalization;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("RankMe", "NameTAG", "0.0.1")]
 class RankMe : RustPlugin
{
    [PluginReference]
    private Plugin Friends;
    private Plugin TimedItemBlocker;
    [ChatCommand("stats")]
     void TurboRankCommand(BasePlayer player, string command);
     void OnEntityDeath(BaseCombatEntity victim, HitInfo info);
    private bool AreFriendsAPIFriend(string playerId, string friendId);
     void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
     void OnRocketLaunched(BasePlayer player, BaseEntity entity);
     void OnEntityBuilt(Planner plan, GameObject go);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void CreateInfo(BasePlayer player);
     void OnPlayerInit(BasePlayer player);
     void Loaded();
     Timer saveDataBatchedTimer;
     void Saved(float delay);
     void Unload();
     void OnServerShutdown();
     void saveDataImmediate();
    public HashSet<TopData> Tops;
    public class TopData
    {
        public TopData(string Ник, string UID, int РакетВыпущено, int УбийствPVP, int Хедшоты, int Дистанция, int ВзрывчатокИспользовано, int УбийствЖивотных, int Смертей, int Дерево, int Камень, int Построек);
        public string Ник { get; set; }
        public string UID { get; set; }
        public int РакетВыпущено { get; set; }
        public int УбийствPVP { get; set; }
        public int Хедшоты { get; set; }
        public int Дистанция { get; set; }
        public int ВзрывчатокИспользовано { get; set; }
        public int УбийствЖивотных { get; set; }
        public int Смертей { get; set; }
        public int Камень { get; set; }
        public int Дерево { get; set; }
        public int Построек { get; set; }
    }

}

public class TopData
{
    public TopData(string Ник, string UID, int РакетВыпущено, int УбийствPVP, int Хедшоты, int Дистанция, int ВзрывчатокИспользовано, int УбийствЖивотных, int Смертей, int Дерево, int Камень, int Построек);
    public string Ник { get; set; }
    public string UID { get; set; }
    public int РакетВыпущено { get; set; }
    public int УбийствPVP { get; set; }
    public int Хедшоты { get; set; }
    public int Дистанция { get; set; }
    public int ВзрывчатокИспользовано { get; set; }
    public int УбийствЖивотных { get; set; }
    public int Смертей { get; set; }
    public int Камень { get; set; }
    public int Дерево { get; set; }
    public int Построек { get; set; }
}


```

---

## RareBox

```csharp
using System;
using Oxide.Core.Plugins;
using System.Collections.Generic;
using System.Globalization;
using UnityEngine;
using System.Linq;
using Rust;
using Facepunch;
using Newtonsoft.Json;

Oxide.Plugins
[Info("RareBox", "Sparkless", "0.1.1")]
public class RareBox : RustPlugin
{
     void RemoveMap();
     void AddMap();
     SpawnFilter filter;
     List<Vector3> monuments;
    static float GetGroundPosition(Vector3 pos);
    public Vector3 RandomDropPosition();
     List<int> BlockedLayers;
    static int blockedMask;
    public Vector3 GetSafeDropPosition(Vector3 position);
    public Vector3 GetEventPosition();
    [PluginReference]
    private Plugin RustMap;
    [PluginReference]
    private Plugin Map;
    public List<BaseEntity> BaseEntityList;
    private ConfigData _config;
    public Timer mytimer;
    public Timer mytimer2;
    public Timer mytimer3;
    private BaseEntity BoxLoot;
    public bool CanLoot;
    public class Itemss
    {
        [JsonProperty("Предмет из игры(shortname)")]
        public string ShortName;
        [JsonProperty("мин кол-во предмета")]
        public int MinDrop;
        [JsonProperty("макс кол-во предмета")]
        public int MaxDrop;
        [JsonProperty("Шанс добавление предмета(0 - отключить выпадение)")]
        public int Chance;
    }

     class ConfigData
    {
        [JsonProperty("Иконка на карте")]
        public string Icons;
        [JsonProperty("Каждое n секунд будет запускаться ивент!")]
        public int CheckTimeForStart;
        [JsonProperty("Пермишенс для команды /rarebox")]
        public string CheckPermission;
        [JsonProperty("Сколько будет надо будет времени подождать, дабы открыть сундук?(в секундах)")]
        public int CheckTime;
        [JsonProperty("Через сколько секунд после открытия ящика он удалится(в секундах)")]
        public int CheckTimeForRemove;
        [JsonProperty("skinID на ящик!(0 - дефолт)")]
        public ulong skinID;
        [JsonProperty("Размер иконки на игровой карте")]
        public float Radius;
        [JsonProperty("Сколько ресурсов ложить в ящик")]
        public int capacity;
        [JsonProperty("Вещи, которые могут попаться именно в ящике")]
        public List<Itemss> ListDrop { get; set; }
        public static ConfigData GetNewCong();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    static DateTime epoch;
    static double CurrentTime();
     void OnServerInitialized();
     void StartEvents();
    public Dictionary<ulong, double> Time;
     void CreateRareTownLoot(Vector3 vector);
     void TownLootIsOpen();
     void DestroyTownLoot();
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     object CanLootEntity(BasePlayer player, StorageContainer container);
     void Unload();
    [ChatCommand("rarebox")]
     void CmdStartTownLoot(BasePlayer player, string command, string[] args);
     void AddLoot(StorageContainer container, BaseEntity Box);
     MapMarkerGenericRadius mapmarker;
     VendingMachineMapMarker MarkerName;
    public void SpawnMapMarkers();
}

public class Itemss
{
    [JsonProperty("Предмет из игры(shortname)")]
    public string ShortName;
    [JsonProperty("мин кол-во предмета")]
    public int MinDrop;
    [JsonProperty("макс кол-во предмета")]
    public int MaxDrop;
    [JsonProperty("Шанс добавление предмета(0 - отключить выпадение)")]
    public int Chance;
}

 class ConfigData
{
    [JsonProperty("Иконка на карте")]
    public string Icons;
    [JsonProperty("Каждое n секунд будет запускаться ивент!")]
    public int CheckTimeForStart;
    [JsonProperty("Пермишенс для команды /rarebox")]
    public string CheckPermission;
    [JsonProperty("Сколько будет надо будет времени подождать, дабы открыть сундук?(в секундах)")]
    public int CheckTime;
    [JsonProperty("Через сколько секунд после открытия ящика он удалится(в секундах)")]
    public int CheckTimeForRemove;
    [JsonProperty("skinID на ящик!(0 - дефолт)")]
    public ulong skinID;
    [JsonProperty("Размер иконки на игровой карте")]
    public float Radius;
    [JsonProperty("Сколько ресурсов ложить в ящик")]
    public int capacity;
    [JsonProperty("Вещи, которые могут попаться именно в ящике")]
    public List<Itemss> ListDrop { get; set; }
    public static ConfigData GetNewCong();
}


```

---

## RatesController

```csharp
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("RatesController", "", "2.1.3")]
[Description("Rate and Time controller - can change rates based on current game time")]
 class RatesController : RustPlugin
{
    private bool isDay;
    private string PickUpDayString;
    private string PickUpNightString;
    private string GatherDayString;
    private string GatherNightString;
    private string QuerryDayString;
    private string QuerryNightString;
    private string LootDayString;
    private string LootNightString;
    private string SmeltDayString;
    private string SmeltNightString;
    private string DefaultRatesCfg;
    private string CustomRatesCfg;
    private string DefaultPickupRatesCfg;
    private string DefaultGatherRatesCfg;
    private string DefaultSmeltRatesCfg;
    private string DefaultQuerryRatesCfg;
    private string PrefixCfg;
    private string PrefixColorCfg;
    private string UseLootMultyplierCfg;
    private string DayStartCfg;
    private string DayLenghtCfg;
    private string NightStartCfg;
    private string NightLenghtCfg;
    private string WarnChatCfg;
    private string CoalRateDayCfg;
    private string CoalRateNightCfg;
    private string CoalChanceDayCfg;
    private string CoalChanceNightCfg;
    private string BlacklistedLootCfg;
    private string MoreHQMCfg;
    private List<string> AvaliableMods;
    private Dictionary<string, float> SmeltBackup;
     Dictionary<int, DateTime> CratesCD;
     Dictionary<BaseEntity, Dictionary<string, float>> DefaultFinishBonuses;
    private double CoalRate;
    private int CoalChance;
    private double SmeltRate;
    private double GatherRate;
    private double PickupRate;
    private double QuerryRate;
    private double LootRate;
    private string Prefix;
    private bool UseLootMultyplier;
    private string PrefixColor;
    private float DayStart;
    private float NightStart;
    private uint DayLenght;
    private uint NightLenght;
    private bool WarnChat;
    private bool MoreHQM;
    private Dictionary<string, double> DefaultRates;
    private Dictionary<string, Dictionary<string, double>> CustomRates;
    private double CoalRateDay;
    private double CoalRateNight;
    private int CoalChanceDay;
    private int CoalChanceNight;
    private Dictionary<string, double> DefaultSmeltRates;
    private Dictionary<string, double> DefaultGatherRates;
    private Dictionary<string, double> DefaultPickupRates;
    private Dictionary<string, double> DefaultQuerryRates;
    private List<string> BlacklistedLoot;
    protected override void LoadDefaultConfig();
     void LoadConfigValues();
     void LoadMessages();
     void OnServerInitialized();
     void Unload();
    private TOD_Time timeComponent;
    private bool Frozen;
    private uint componentSearchAttempts;
    private void GetTimeComponent();
    private bool ProgressTime { get; set; }
    private void OnHour();
     void UpdateDayLenght(uint Lenght, bool night);
    public float CurrentHour { get; set; }
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void OnLootEntity(BasePlayer player, BaseEntity entity);
    private void UpdateFurnaces();
     float CookingTemperature(BaseOven.TemperatureType temperature);
     object OnOvenToggle(BaseOven oven, BasePlayer player);
     void StartCooking(BaseOven oven, BaseEntity entity, double ovenMultiplier);
     Item FindBurnable(BaseOven oven);
    private void OnDayStart();
    private void OnNightStart();
    private void RatesToChat();
     void OnCollectiblePickup(Item item, BasePlayer player);
     void OnCropGather(PlantEntity plant, Item item, BasePlayer player);
     void OnEntityKill(BaseNetworkable entity);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnQuarryGather(MiningQuarry quarry, Item item);
     void OnConsumeFuel(BaseOven oven, Item fuel, ItemModBurnable burnable);
    [ConsoleCommand("env.freeze")]
     void TimeFreeze(ConsoleSystem.Arg arg);
    [ConsoleCommand("env.unfreeze")]
     void TimeUnFreeze(ConsoleSystem.Arg arg);
    [ConsoleCommand("rates.show")]
     void ShowRates(ConsoleSystem.Arg arg);
    [ChatCommand("rates")]
    private void ShowRatesChat(BasePlayer player, string command, string[] args);
    private void GetConfig(string Key, T var);
    private void SendToChat(BasePlayer Player, string Message);
    private void SendToChat(string Message);
     string GetMsg(string key, object userID);
     double GetUserRates(string steamId, string RateType);
     double GetUserRates(ulong steamId, string RateType);
     Dictionary<string, object> CreatePerms(List<string> mods, double rate);
}


```

---

## RATHealer

```csharp
using System.Collections.Generic;
using System.Reflection;
using System;
using System.Data;
using UnityEngine;
using Oxide.Core;

Oxide.Plugins
[Info("RATHealer", "@lonestarcanuck", 0.1, ResourceId = 1963)]
[Description("The Rust Admin Tool (RAT) provides ingame and console heal features")]
public class RATHealer : RustPlugin
{
    public BasePlayer cachedPlayer;
     void Init();
     void Unload();
    private object GetPlayer(string tofind);
    [ConsoleCommand("RATHealer.heal")]
    private void HealCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("RATHealer.all")]
    private void HealAllCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("RATHealer.list")]
    private void ListCommand(ConsoleSystem.Arg arg);
}


```

---

## RecoilRecorder

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using ProtoBuf;
using UnityEngine;

Oxide.Plugins
[Info("Recoil Recorder", "Orange", "1.1.6")]
[Description("https://rustworkshop.space/resources/recoil-recorder.247/")]
public class RecoilRecorder : RustPlugin
{
    private const string hookOnRecorded;
    private const int border;
    private static Dictionary<string, int[]> patternsList;
    private const string urlPatterns;
    private const float shootingCooldown;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnWeaponFired(BaseProjectile projectile, BasePlayer player, ItemModProjectile mod, ProjectileShoot projectiles);
    private void OnReloadWeapon(BasePlayer player, BaseProjectile projectile);
    private void OnEnterZone(string ZoneID, BasePlayer player);
    private void OnExitZone(string ZoneID, BasePlayer player);
    private void CheckPlayers();
    private bool Match(string zoneId);
    private void LoadPatterns();
    private const string elemMain;
    private const string elemPattern;
    private static string jsonMain;
    private const int startY;
    private const int offsetY;
    private const int sizePanel;
    private void BuildUI();
    private static void ShowPattern(BasePlayer player, int[] pattern);
    private static void ShowShot(BasePlayer player, int shotValue, int shotCount, bool hit);
    private static ConfigDefinition config;
    private class ConfigDefinition
    {
        [JsonProperty("Active zones (name or id)")]
        public string[] activeZonesIdOrName;
        [JsonProperty("Hit Symbol")]
        public string hitSymbol;
        [JsonProperty("Header text")]
        public string textHeader;
        [JsonProperty("Maximal distance between shots to count hit")]
        public int hitMaxDistance;
        [JsonProperty("UI Position (Anchor)")]
        public string uiPositionAnchor;
    }

    private class ScriptRecorder : MonoBehaviour
    {
        private List<int> shots;
        private BaseProjectile lastWeapon;
        private BasePlayer player;
        private float firstShot;
        private float lastShootTime;
        private bool showUi;
        public int[] Pattern;
        public int[] Shots { get; set; }
        private BasePlayer[] spectators { get; set; }
        private void Awake();
        private void Start();
        public void Enable();
        public void Disable();
        private void OnDestroy();
        public void OnFired(BaseProjectile weapon);
        public void OnReloaded();
        private void AddValue();
        private void CheckWeapon(BaseProjectile weapon);
        private void ClearShots();
        private void CheckConnection();
    }

    protected override void LoadConfig();
    private void ValidateConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class Zone
    {
        private static Oxide.Core.Libraries.Plugins plugins;
        private const string filename;
        private static Plugin plugin;
        public static void Unload();
        public static string GetName(string id);
        public static Vector3? GetLocation(string id);
        public static string[] GetAllZones();
        public static List<BasePlayer> GetPlayersInZone(string id);
        public static bool IsInside(string id, BasePlayer player);
        private static object Call(string name, object[] args);
        private static void FindPlugin();
    }

}

private class ConfigDefinition
{
    [JsonProperty("Active zones (name or id)")]
    public string[] activeZonesIdOrName;
    [JsonProperty("Hit Symbol")]
    public string hitSymbol;
    [JsonProperty("Header text")]
    public string textHeader;
    [JsonProperty("Maximal distance between shots to count hit")]
    public int hitMaxDistance;
    [JsonProperty("UI Position (Anchor)")]
    public string uiPositionAnchor;
}

private class ScriptRecorder : MonoBehaviour
{
    private List<int> shots;
    private BaseProjectile lastWeapon;
    private BasePlayer player;
    private float firstShot;
    private float lastShootTime;
    private bool showUi;
    public int[] Pattern;
    public int[] Shots { get; set; }
    private BasePlayer[] spectators { get; set; }
    private void Awake();
    private void Start();
    public void Enable();
    public void Disable();
    private void OnDestroy();
    public void OnFired(BaseProjectile weapon);
    public void OnReloaded();
    private void AddValue();
    private void CheckWeapon(BaseProjectile weapon);
    private void ClearShots();
    private void CheckConnection();
}

private class Zone
{
    private static Oxide.Core.Libraries.Plugins plugins;
    private const string filename;
    private static Plugin plugin;
    public static void Unload();
    public static string GetName(string id);
    public static Vector3? GetLocation(string id);
    public static string[] GetAllZones();
    public static List<BasePlayer> GetPlayersInZone(string id);
    public static bool IsInside(string id, BasePlayer player);
    private static object Call(string name, object[] args);
    private static void FindPlugin();
}


```

---

## RecoveryItems

```csharp
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Oxide.Core.Plugins;
using Oxide.Core;

Oxide.Plugins
[Info("RecoveryItems", "RedMat", "1.0.1", ResourceId = 2091)]
[Description("If you wear an item it is recovered.")]
 class RecoveryItems : RustPlugin
{
     string gstrItemListFileNm;
     string gstrItemListInfoFileNm;
     Timer gobjTimer;
     void Init();
     void LoadDefaultMessages();
     void Loaded();
    private void fStartTimer();
    private bool fGetWear(BasePlayer pobjBasePlayer, int pintItemID);
    [ChatCommand("rt")]
     void fChatCmdHpTime(BasePlayer pbasPlayer, string pstrCmd, string[] args);
    [ChatCommand("ri")]
     void fChatCmdHpItem(BasePlayer pbasPlayer, string pstrCmd, string[] args);
    protected override void LoadDefaultConfig();
     RecoveryItemsData recoveryItemsData;
     RecoveryItemsDataInfo recoveryItemsDataInfo;
     class RecoveryItemsData
    {
        public List<int> glstRecoveryItemsData;
        public RecoveryItemsData();
    }

     class RecoveryItemsDataInfo
    {
        public Dictionary<int, int> glstRecoveryItemsDataInfo;
        public RecoveryItemsDataInfo();
    }

     string Lang(string key, string id, object[] args);
}

 class RecoveryItemsData
{
    public List<int> glstRecoveryItemsData;
    public RecoveryItemsData();
}

 class RecoveryItemsDataInfo
{
    public Dictionary<int, int> glstRecoveryItemsDataInfo;
    public RecoveryItemsDataInfo();
}


```

---

## Recycle

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core;
using System.Text;
using System.Linq;

Oxide.Plugins
[Info("Recycle", "Calytic", "2.0.8", ResourceId = 1296)]
[Description("Recycle crafted items to base resources")]
 class Recycle : RustPlugin
{
    private float cooldownMinutes;
    private float refundRatio;
    private string box;
    private bool npconly;
    private List<object> npcids;
    private float radiationMax;
    private Dictionary<string, DateTime> recycleCooldowns;
     class OnlinePlayer
    {
        public BasePlayer Player;
        public BasePlayer Target;
        public StorageContainer View;
        public List<BasePlayer> Matches;
        public OnlinePlayer(BasePlayer player);
    }

    public Dictionary<ItemContainer, ulong> containers;
    [OnlinePlayers]
     Hash<BasePlayer, OnlinePlayer> onlinePlayers;
    protected override void LoadDefaultConfig();
     void Unloaded();
     void Init();
     void Loaded();
     void CheckConfig();
    protected void ReloadConfig();
     void LoadMessages();
    private bool IsBox(BaseNetworkable entity);
     object CanNetworkTo(BaseNetworkable entity, BasePlayer target);
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void OnPlayerLootEnd(PlayerLoot inventory);
     void OnItemAddedToContainer(ItemContainer container, Item item);
     void OnUseNPC(BasePlayer npc, BasePlayer player);
    [ConsoleCommand("rec")]
     void ccRec(ConsoleSystem.Arg arg);
    [ChatCommand("rec")]
     void cmdRec(BasePlayer player, string command, string[] args);
     void ShowBox(BasePlayer player, BaseEntity target);
     void HideBox(BasePlayer player);
     void OpenBoxView(BasePlayer player, BaseEntity targArg);
     void CloseBoxView(BasePlayer player, StorageContainer view);
     bool SalvageItem(BasePlayer player, Item item);
     bool CanPlayerRecycle(BasePlayer player);
    public string jsonNotify;
    public void ShowNotification(BasePlayer player, string msg);
    public void HideNotification(BasePlayer player);
    private void SendHelpText(BasePlayer player);
     string GetMsg(string key, BasePlayer player);
    private T GetConfig(string name, T defaultValue);
    private T GetConfig(string name, string name2, T defaultValue);
}

 class OnlinePlayer
{
    public BasePlayer Player;
    public BasePlayer Target;
    public StorageContainer View;
    public List<BasePlayer> Matches;
    public OnlinePlayer(BasePlayer player);
}


```

---

## Recycler

```csharp
using System;
using UnityEngine;
using Oxide.Core;
using System.Text;
using System.Linq;
using Oxide.Core.Plugins;
using System.Collections.Generic;

Oxide.Plugins
[Info("Recycler", "Fartus", "1.0.0")]
[Description("Карманный переработчик ресурсов")]
public class Recycler : RustPlugin
{
    public class RecyclerBox : MonoBehaviour
    {
        private const int SIZE;
         StorageContainer storage;
         BasePlayer player;
        public void Init(StorageContainer storage, BasePlayer player);
        public bool HasRecyclable(Item slot);
         void RecycleItem(Item slot);
        public void MoveItemToOutput(Item newItem);
        public static RecyclerBox Spawn(BasePlayer player);
        private static StorageContainer SpawnContainer(BasePlayer player);
        private void PlayerStoppedLooting(BasePlayer player);
        public void Close();
        public void StartLoot();
        public void Push(List<Item> items);
        public void ClearItems();
        public List<Item> Items { get; set; }
    }

     void OnServerInitialized();
    const string permissionName;
    [ChatCommand("rec")]
     void cmdChatRecycler(BasePlayer player, string cmd, string[] args);
     void OpenRecycler(BasePlayer player);
    [PluginReference]
     Plugin Duels;
     bool InDuel(BasePlayer player);
}

public class RecyclerBox : MonoBehaviour
{
    private const int SIZE;
     StorageContainer storage;
     BasePlayer player;
    public void Init(StorageContainer storage, BasePlayer player);
    public bool HasRecyclable(Item slot);
     void RecycleItem(Item slot);
    public void MoveItemToOutput(Item newItem);
    public static RecyclerBox Spawn(BasePlayer player);
    private static StorageContainer SpawnContainer(BasePlayer player);
    private void PlayerStoppedLooting(BasePlayer player);
    public void Close();
    public void StartLoot();
    public void Push(List<Item> items);
    public void ClearItems();
    public List<Item> Items { get; set; }
}


```

---

## RecyclerSpeed

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Recycler Speed", "Ryz0r/yetzt", "2.0.2")]
[Description("Easily set the speed at which the recycler... recycles")]
public class RecyclerSpeed : RustPlugin
{
    private const string UsePerm;
    private Configuration _config;
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private class Configuration
    {
        [JsonProperty(PropertyName = "Recyler Speed (Lower = Faster) (Seconds)")]
        public float RecyclerSpeed;
    }

    protected override void LoadConfig();
    private void Init();
    private void OnRecyclerToggle(Recycler recycler, BasePlayer player);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Recyler Speed (Lower = Faster) (Seconds)")]
    public float RecyclerSpeed;
}


```

---

## RemoteDoors

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Reflection;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("RemoteDoors", "Reneb", "1.0.8", ResourceId = 1379)]
 class RemoteDoors : RustPlugin
{
    protected override void LoadDefaultConfig();
    private void CheckCfg(string Key, T var);
    static string permissionRemoteDoors;
    static bool allowusers;
    static Dictionary<string, object> cost;
    static int maxDistance;
    static int antiTrapDistance;
    static int maxDoors;
     void Init();
    static Dictionary<string, object> defaultCost();
     bool hasAccess(BasePlayer player);
    static int constructionColl;
    static int doorColl;
    static int signColl;
    static int playerColl;
     Vector3 VectorForward;
     RaycastHit cachedHit;
    static FieldInfo serverinput;
    static FieldInfo buildingPriviledge;
    static FieldInfo fieldWhiteList;
    static MethodInfo updatelayer;
    static Hash<Vector3, RemoteActivator> remoteActivators;
     void Loaded();
     void OnServerInitialized();
    readonly Dictionary<string, string> displaynameToShortname;
    private void InitializeTable();
    static StoredData storedData;
     class StoredData
    {
        public HashSet<RemoteActivator> RemoteActivators;
        public StoredData();
    }

     void LoadData();
     void Unloaded();
     void SaveData();
    public class RemoteDoor
    {
        public string x;
        public string y;
        public string z;
         Door door;
         Vector3 pos;
        public RemoteDoor();
        public RemoteDoor(Vector3 pos);
        public bool OpenDoor(bool openclose);
        public Vector3 Pos();
         Door FindDoor();
    }

     Dictionary<Vector3, float> allowAuth;
    public class RemoteActivator
    {
        public string name;
        public string x;
        public string y;
        public string z;
        public string owner;
        public List<string> autorizedUsers;
        public List<RemoteDoor> listedDoors;
         Vector3 pos;
        public RemoteActivator();
        public RemoteActivator(Vector3 pos, string name, string owner);
        public Vector3 Pos();
    }

     string SignTexture();
    private void SpawnDeployableSign(string prefab, Vector3 pos, Quaternion angles);
    public string doorsoverlay;
    public string getaccessoverlay;
    public string adminoverlay;
     void ShowUI(BasePlayer player, Vector3 remotePos, string ttype);
     void OnUseSignage(BasePlayer player, Signage sign);
     void OnPlayerInput(BasePlayer player, InputState input);
     class RemoteDoorAdder : MonoBehaviour
    {
         BasePlayer player;
         float lastUpdate;
        public Vector3 remoteActivate;
         InputState inputState;
         void Awake();
         void FixedUpdate();
        public void Refresh();
         void DestroyThis();
    }

    static void PrintToChat(BasePlayer player, string message);
    static void TryAddDoor(BasePlayer player, Vector3 remoteActivate);
    static BaseEntity FindRayStructure(Ray ray, int currentCol);
    [ConsoleCommand("remote.close")]
     void ccmdRemoteClose(ConsoleSystem.Arg arg);
    [ConsoleCommand("remote.cmd")]
     void ccmdRemoteCommand(ConsoleSystem.Arg arg);
     void Pay(BasePlayer player);
     bool CanPay(BasePlayer player);
    [ChatCommand("remote")]
     void cmdChatNPCPathTest(BasePlayer player, string command, string[] args);
     string GetCostInString();
     void SendHelp(BasePlayer player, bool full);
    [HookMethod("SendHelpText")]
    private void SendHelpText(BasePlayer player);
}

 class StoredData
{
    public HashSet<RemoteActivator> RemoteActivators;
    public StoredData();
}

public class RemoteDoor
{
    public string x;
    public string y;
    public string z;
     Door door;
     Vector3 pos;
    public RemoteDoor();
    public RemoteDoor(Vector3 pos);
    public bool OpenDoor(bool openclose);
    public Vector3 Pos();
     Door FindDoor();
}

public class RemoteActivator
{
    public string name;
    public string x;
    public string y;
    public string z;
    public string owner;
    public List<string> autorizedUsers;
    public List<RemoteDoor> listedDoors;
     Vector3 pos;
    public RemoteActivator();
    public RemoteActivator(Vector3 pos, string name, string owner);
    public Vector3 Pos();
}

 class RemoteDoorAdder : MonoBehaviour
{
     BasePlayer player;
     float lastUpdate;
    public Vector3 remoteActivate;
     InputState inputState;
     void Awake();
     void FixedUpdate();
    public void Refresh();
     void DestroyThis();
}


```

---

## Remove

```csharp
using Facepunch;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Remove", "Admin", "1.2.4")]
 class Remove : RustPlugin
{
    static Remove inst;
     int resetTime;
     float refundPercent;
     float refundItemsPercent;
     float refundStoragePercent;
     bool friendRemove;
     bool clanRemove;
     bool EnTimedRemove;
     bool cupboardRemove;
     bool selfRemove;
     bool removeFriends;
     bool removeClans;
     bool refundItemsGive;
     float Timeout;
    private string PanelAnchorMin;
    private string PanelAnchorMax;
    private string PanelColor;
    private bool useNoEscape;
    private int TextFontSize;
    private string TextСolor;
    private string TextAnchorMin;
    private string TextAnchorMax;
    private bool EnabledBuildingUpgrade;
    protected override void LoadDefaultConfig();
    public static void GetVariable(DynamicConfigFile config, string name, T value, T defaultValue);
    static int constructionColl;
    private static Dictionary<string, int> deployedToItem;
     Dictionary<BasePlayer, int> timers;
     Dictionary<ulong, string> activePlayers;
     int currentRemove;
    [PluginReference]
     Plugin Clans;
    [PluginReference]
     Plugin Friends;
    [PluginReference]
     Plugin NoEscape;
    [PluginReference]
     Plugin MutualPermission;
    [PluginReference]
     Plugin BuildingUpgrade;
     bool IsClanMember(ulong playerID, ulong targetID);
     bool IsFriends(ulong playerID, ulong friendId);
    private Dictionary<BasePlayer, DateTime> Cooldowns;
    private double Cooldown;
    private void OnPlayerActiveItemChanged(BasePlayer player, Item newItem);
    [ChatCommand("remove")]
     void cmdRemove(BasePlayer player, string command, string[] args);
    [ConsoleCommand("remove.toggle")]
     void cmdConsoleRemove(ConsoleSystem.Arg args);
     Dictionary<uint, float> entityes;
     void LoadEntity();
     void CheckEntity();
     void OnEntityBuilt(Planner plan, GameObject go);
     void OnEntityKill(BaseNetworkable entity);
     void OnNewSave();
     void OnServerSave();
     void Loaded();
    public List<string> permisions;
     void Unload();
    private Timer entitycheck;
     int check;
     void OnServerInitialized();
    private bool CupboardPrivlidge(BasePlayer player, Vector3 position, BaseEntity entity);
     void RemoveAllFrom(Vector3 pos);
     List<BaseEntity> wasRemoved;
     List<Vector3> removeFrom;
     void DelayRemoveAll();
    static void DoRemove(BaseEntity removeObject);
     void TryRemove(BasePlayer player, BaseEntity removeObject);
     bool OnRemoveActivate(ulong player);
     void RemoveDeativate(ulong player);
     object OnHammerHit(BasePlayer player, HitInfo info);
    private static string Format(int units, string form1, string form2, string form3);
    private static class NumericalFormatter
    {
        private static string GetNumEndings(int origNum, string[] forms);
        private static bool IsEng(object player);
        private static string FormatSeconds(int seconds, bool eng);
        private static string FormatMinutes(int minutes, bool eng);
        private static string FormatHours(int hours, bool eng);
        private static string FormatDays(int days, bool eng);
        private static string FormatTime(TimeSpan timeSpan, bool eng);
        public static string FormatTime(int seconds, object player);
        public static string FormatTime(float seconds, object player);
        public static string FormatTime(TimeSpan time, object player);
        public static string FromatSlots(int slots, object player);
    }

    private static string GetUserId(object player);
     void TimerEntity();
     void TimerHandler();
     void RemoveEntity(BasePlayer player, BaseEntity entity);
     void RemoveEntityAdmin(BasePlayer player, BaseEntity entity);
     void RemoveEntityAll(BasePlayer player, BaseEntity entity, Vector3 pos);
     Dictionary<uint, Dictionary<ItemDefinition, int>> refundItems;
     void Refund(BasePlayer player, BaseEntity entity);
     void GiveAndShowItem(BasePlayer player, int item, int amount);
     void InitRefundItems();
    private string GUI;
     void DrawUI(BasePlayer player, int seconds, string type);
     void DestroyUI(BasePlayer player);
     void ActivateRemove(ulong userId, string type);
     void DeactivateRemove(ulong userId);
     void UpdateTimer(BasePlayer player, string type);
     void UpdateTimerAdmin(BasePlayer player, string type);
     void UpdateTimerAll(BasePlayer player, string type);
     Dictionary<string, string> Messages;
    public static class PermissionService
    {
        public static Permission permission;
        public static bool HasPermission(BasePlayer player, string permissionName);
        public static void RegisterPermissions(Plugin owner, List<string> permissions);
    }

}

private static class NumericalFormatter
{
    private static string GetNumEndings(int origNum, string[] forms);
    private static bool IsEng(object player);
    private static string FormatSeconds(int seconds, bool eng);
    private static string FormatMinutes(int minutes, bool eng);
    private static string FormatHours(int hours, bool eng);
    private static string FormatDays(int days, bool eng);
    private static string FormatTime(TimeSpan timeSpan, bool eng);
    public static string FormatTime(int seconds, object player);
    public static string FormatTime(float seconds, object player);
    public static string FormatTime(TimeSpan time, object player);
    public static string FromatSlots(int slots, object player);
}

public static class PermissionService
{
    public static Permission permission;
    public static bool HasPermission(BasePlayer player, string permissionName);
    public static void RegisterPermissions(Plugin owner, List<string> permissions);
}


```

---

## RemoveDefaultRadiation

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("RemoveDefaultRadiation", "k1lly0u", "0.1.0", ResourceId = 0)]
 class RemoveDefaultRadiation : RustPlugin
{
     void OnServerInitialized();
    private ConfigData configData;
     class ConfigData
    {
        public List<string> PluginList { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
}

 class ConfigData
{
    public List<string> PluginList { get; set; }
}


```

---

## RemoverTool

```csharp
using System.Collections.Generic;
using System;
using System.Reflection;
using System.Data;
using UnityEngine;
using Facepunch;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using System.Linq;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using System.Collections;

Oxide.Plugins
[Info("RemoverTool", "Reneb", "4.0.9", ResourceId = 651)]
 class RemoverTool : RustPlugin
{
    [PluginReference]
     Plugin Friends;
    static RemoverTool rt;
    static FieldInfo serverInput;
    static FieldInfo buildingPrivilege;
    static int colliderRemovable;
    static int colliderBuilding;
    static int colliderPlayer;
     bool RemoveOverride;
    static string permissionNormal;
    static string permissionOverride;
    static string permissionAdmin;
    static string permissionAll;
    static string permissionTarget;
    static int authTarget;
    static int authNormal;
    static int authAdmin;
    static int authAll;
    static int authOverride;
    static int removeDistanceNormal;
    static int removeDistanceAdmin;
    static int removeDistanceAll;
    static bool removeGibsNormal;
    static bool removeGibsAdmin;
    static bool removeGibsAll;
    static int RemoveDefaultTime;
    static int RemoveMaxTime;
    static bool RemoveWithToolCupboards;
    static bool RemoveWithEntityOwners;
    static bool RemoveWithBuildingOwners;
    static bool RemoveWithRustIO;
    static bool RemoveWithFriends;
    static bool RaidBlocker;
    static bool RaidBlockerBlockBuildingID;
    static bool RaidBlockerBlockSurroundingPlayers;
    static int RaidBlockerRadius;
    static int RaidBlockerTime;
    static Dictionary<string, object> Price;
    static Dictionary<string, object> Refund;
    static Dictionary<string, object> ValidEntities;
    static string GUIRemoverToolBackgroundColor;
    static string GUIRemoverToolAnchorMin;
    static string GUIRemoverToolAnchorMax;
    static string GUIRemoveBackgroundColor;
    static string GUIRemoveAnchorMin;
    static string GUIRemoveAnchorMax;
    static string GUIRemoveTextColor;
    static int GUIRemoveTextSize;
    static string GUIRemoveTextAnchorMin;
    static string GUIRemoveTextAnchorMax;
    static string GUITimeLeftBackgroundColor;
    static string GUITimeLeftAnchorMin;
    static string GUITimeLeftAnchorMax;
    static string GUITimeLeftTextColor;
    static int GUITimeLeftTextSize;
    static string GUITimeLeftTextAnchorMin;
    static string GUITimeLeftTextAnchorMax;
    static string GUIEntityBackgroundColor;
    static string GUIEntityAnchorMin;
    static string GUIEntityAnchorMax;
    static string GUIEntityTextColor;
    static int GUIEntityTextSize;
    static string GUIEntityTextAnchorMin;
    static string GUIEntityTextAnchorMax;
    static bool GUIAuthorizations;
    static string GUIAllowedBackgroundColor;
    static string GUIRefusedBackgroundColor;
    static string GUIAuthorizationsAnchorMin;
    static string GUIAuthorizationsAnchorMax;
    static string GUIPriceBackgroundColor;
    static string GUIPriceAnchorMin;
    static string GUIPriceAnchorMax;
    static bool GUIPrices;
    static string GUIPriceTextColor;
    static int GUIPriceTextSize;
    static string GUIPriceTextAnchorMin;
    static string GUIPriceTextAnchorMax;
    static string GUIPrice2TextColor;
    static int GUIPrice2TextSize;
    static string GUIPrice2TextAnchorMin;
    static string GUIPrice2TextAnchorMax;
    static string GUIRefundBackgroundColor;
    static string GUIRefundAnchorMin;
    static string GUIRefundAnchorMax;
    static bool GUIRefund;
    static string GUIRefundTextColor;
    static int GUIRefundTextSize;
    static string GUIRefundTextAnchorMin;
    static string GUIRefundTextAnchorMax;
    static string GUIRefund2TextColor;
    static int GUIRefund2TextSize;
    static string GUIRefund2TextAnchorMin;
    static string GUIRefund2TextAnchorMax;
    static Dictionary<string, string> PrefabNameToDeployable;
    static Dictionary<string, string> PrefabNameToStructure;
    static Dictionary<string, int> ItemNameToItemID;
    static Hash<uint, float> LastAttackedBuildings;
    static Hash<ulong, float> LastBlockedPlayers;
    protected override void LoadDefaultConfig();
    private void CheckCfg(string Key, T var);
     void LoadConfigs();
     void OnServerInitialized();
     void Loaded();
     void Unload();
    private static Library RustIO;
    private static MethodInfo isInstalled;
    private static MethodInfo hasFriend;
    private static bool RustIOIsInstalled();
    private void InitializeRustIO();
    private static bool HasFriend(string playerId, string friendId);
     void InitializeItems();
     void InitializeConstruction();
     Dictionary<string, object> DefaultPay();
     Dictionary<string, object> DefaultEntities();
     Dictionary<string, object> DefaultRefund();
    static string GetMsg(string key, BasePlayer source);
     bool hasPermission(BasePlayer player, string perm, int authlevel);
     string ListPlayersToString(List<IPlayer> players);
     bool GetParameters(BasePlayer player, string[] args, RemoveType RemoveType, BasePlayer Target, int Time, string Reason);
    static void DoRemove(BaseEntity Entity, bool gibs);
    public class UI
    {
        static public CuiElementContainer CreateElementContainer(string parent, string panelName, string color, string aMin, string aMax, bool useCursor);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    }

    public static string GetName(string prefabname);
    public static void CreateGUI(BasePlayer player, RemoveType removeType);
    public static void GUITimeLeftUpdate(BasePlayer player, int timeleft);
    public static void GUIEntityUpdate(BasePlayer player, BaseEntity TargetEntity);
    public static void GUIPricesUpdate(BasePlayer player, bool usePrice, BaseEntity TargetEntity);
    public static void GUIRefundUpdate(BasePlayer player, bool useRefund, BaseEntity TargetEntity);
    public static void GUIAuthorizationUpdate(BasePlayer player, RemoveType removeType, BaseEntity TargetEntity, bool shouldPay);
    public static void DestroyGUI(BasePlayer player);
     class ToolRemover : MonoBehaviour
    {
        public BasePlayer player { get; set; }
        public BasePlayer source { get; set; }
        public int timeLeft { get; set; }
        public float distance { get; set; }
        public RemoveType removetype { get; set; }
        public bool Pay { get; set; }
        public bool Refund { get; set; }
        public BaseEntity TargetEntity { get; set; }
         RaycastHit RayHit;
         InputState state;
         float lastUpdate { get; set; }
         float lastRemove { get; set; }
         void Awake();
        public void Start();
         void RemoveUpdate();
         void FixedUpdate();
        public void Destroy();
    }

    static bool Pay(BasePlayer player, BaseEntity TargetEntity);
    static Dictionary<string, object> GetPrice(BaseEntity TargetEntity);
    static bool CanPay(BasePlayer player, BaseEntity TargetEntity);
    static void GiveRefund(BasePlayer player, BaseEntity TargetEntity);
    static Dictionary<string, object> GetRefund(BaseEntity TargetEntity);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void BlockRemove(BaseCombatEntity entity);
    static bool IsBlocked(BasePlayer player, BaseEntity TargetEntity, float timeLeft);
    static string TryRemove(BasePlayer player, RemoveType removeType, float distance, bool shouldPay, bool shouldRefund);
    static bool CanRemoveEntity(BasePlayer player, RemoveType removeType, BaseEntity TargetEntity, bool shouldPay, string Reason);
     bool AreFriends(string steamid, string friend);
    static bool HasAccess(BasePlayer player, BaseEntity TargetEntity);
    static bool IsRemovableEntity(BaseEntity entity);
    static bool IsValidEntity(BaseEntity entity);
    static bool hasTotalAccess(BasePlayer player);
    static void RemoveAll(BaseEntity sourceEntity);
    static bool RemoveStructure(BaseEntity sourceEntity);
    public static IEnumerator DelayRemove(List<BuildingBlock> entities);
    public static IEnumerator DelayRemove(List<BaseEntity> entities);
     string ToggleRemove(BasePlayer player, string[] args);
    [ChatCommand("remove")]
     void cmdChatRemove(BasePlayer player, string command, string[] args);
    [ConsoleCommand("remove.toggle")]
     void ccmdRemoveToggle(ConsoleSystem.Arg arg);
    [ConsoleCommand("remove")]
     void ccmdRemove(ConsoleSystem.Arg arg);
    [ConsoleCommand("remove.allow")]
     void ccmdRemoveAllow(ConsoleSystem.Arg arg);
    [HookMethod("SendHelpText")]
    private void SendHelpText(BasePlayer player);
}

public class UI
{
    static public CuiElementContainer CreateElementContainer(string parent, string panelName, string color, string aMin, string aMax, bool useCursor);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
}

 class ToolRemover : MonoBehaviour
{
    public BasePlayer player { get; set; }
    public BasePlayer source { get; set; }
    public int timeLeft { get; set; }
    public float distance { get; set; }
    public RemoveType removetype { get; set; }
    public bool Pay { get; set; }
    public bool Refund { get; set; }
    public BaseEntity TargetEntity { get; set; }
     RaycastHit RayHit;
     InputState state;
     float lastUpdate { get; set; }
     float lastRemove { get; set; }
     void Awake();
    public void Start();
     void RemoveUpdate();
     void FixedUpdate();
    public void Destroy();
}


```

---

## Rename

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Rename", "Wulf/lukespragg", "0.3.0", ResourceId = 1184)]
[Description("Allows players with permission to instantly rename other players or self")]
 class Rename : CovalencePlugin
{
     StoredData storedData;
    const string permOthers;
    const string permSelf;
     bool persistent;
     bool preventAdmin;
    protected override void LoadDefaultConfig();
     void Init();
     class StoredData
    {
        public readonly HashSet<PlayerInfo> Renames;
    }

     class PlayerInfo
    {
        public string Id;
        public string Old;
        public string New;
        public PlayerInfo();
        public PlayerInfo(IPlayer player, string newName);
    }

     void LoadPersistentData();
     void SaveData();
     void LoadDefaultMessages();
     void OnUserConnected(IPlayer player);
    [Command("rename")]
     void RenameCommand(IPlayer player, string command, string[] args);
    [Command("resetname", "namereset")]
     void NameResetCommand(IPlayer player, string command, string[] args);
     T GetConfig(string name, T value);
     string Lang(string key, string id, object[] args);
}

 class StoredData
{
    public readonly HashSet<PlayerInfo> Renames;
}

 class PlayerInfo
{
    public string Id;
    public string Old;
    public string New;
    public PlayerInfo();
    public PlayerInfo(IPlayer player, string newName);
}


```

---

## RepairTool

```csharp
using System.Collections.Generic;
using UnityEngine;
using System.Reflection;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("RepairTool", "k1lly0u", "0.1.1", ResourceId = 1883)]
 class RepairTool : RustPlugin
{
    private static FieldInfo serverinput;
    private string panelName;
    private List<Repairer> activeRepairers;
     void OnServerInitialized();
     void Unload();
     void OnPlayerInput(BasePlayer player, InputState input);
    private void CreateUI(BasePlayer player, int time, string name);
    private void DestroyUI(BasePlayer player);
    private void InitPlugin();
    private object RepairEntity(BaseEntity entity);
    private BaseEntity FindEntity(BasePlayer player);
    private object Ray(BasePlayer player, Vector3 Aim);
    private BaseEntity FindBuildingBlock(Ray ray, float distance);
    private void MSG(BasePlayer player, string msg, bool title);
    private string GetMSG(string key);
    private void DestroyAllPlayers();
    [ChatCommand("rt")]
    private void cmdRA(BasePlayer player, string command, string[] args);
    private bool HasPerm(BasePlayer player);
     class Repairer : MonoBehaviour
    {
        public BasePlayer player;
        public int TimeRemaining;
        public InputState inputState;
         RepairTool ra;
        public void InitPlayer(BasePlayer p, int time, RepairTool repairtool);
        public void FindTarget();
        private void RefreshGUI();
         void DestroyGUI();
        public void DestroyComponent();
    }

}

 class Repairer : MonoBehaviour
{
    public BasePlayer player;
    public int TimeRemaining;
    public InputState inputState;
     RepairTool ra;
    public void InitPlayer(BasePlayer p, int time, RepairTool repairtool);
    public void FindTarget();
    private void RefreshGUI();
     void DestroyGUI();
    public void DestroyComponent();
}


```

---

## Replenish

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Replenish", "Skrallex", "1.3.1", ResourceId = 1956)]
[Description("Easily replenish chests")]
 class Replenish : RustPlugin
{
     List<ReplenishableContainer> containers;
     List<ReplenishPlayer> playersUsing;
     Dictionary<string, string> allowableContainers;
     StoredData data;
     bool RequireAllSlotsEmpty;
     int DefaultTimerLength;
     bool UsePermissionsOnly;
    const string adminPerm;
    const string canEdit;
    const string canTest;
    const string canList;
     void Loaded();
     void Unload();
     void OnServerSave();
    protected override void LoadDefaultConfig();
     void LoadConfig();
     void LoadDefaultMessages();
     void OnHammerHit(BasePlayer player, HitInfo info);
     void OnItemRemovedFromContainer(ItemContainer container, Item item);
     void CreateReplenishableContainer(BasePlayer player, ReplenishPlayer rPlayer, StorageContainer container, HitInfo info, string type);
     void RemoveReplenishableContainer(BasePlayer player, StorageContainer container);
     void TestReplenishableContainer(BasePlayer player, StorageContainer container);
    [ChatCommand("replenish")]
     void chatCmdReplenish(BasePlayer player, string cmd, string[] args);
    [ChatCommand("replenish_add")]
     void chatCmdReplenishAdd(BasePlayer player, string cmd, string[] args);
    [ChatCommand("replenish_addm")]
     void chatCmdReplenishAddm(BasePlayer player, string cmd, string[] args);
     void Add(BasePlayer player, int timer, bool multi);
    [ChatCommand("replenish_remove")]
     void chatCmdReplenishRemove(BasePlayer player, string cmd, string[] args);
    [ChatCommand("replenish_removem")]
     void chatCmdReplenishRemovem(BasePlayer player, string cmd, string[] args);
     void Remove(BasePlayer player, bool multi);
     void RemoveByID(BasePlayer player, string id);
    [ChatCommand("replenish_test")]
     void chatCmdReplenishTest(BasePlayer player, string cmd, string[] args);
    [ChatCommand("replenish_testm")]
     void chatCmdReplenishTestm(BasePlayer player, string cmd, string[] args);
     void Test(BasePlayer player, bool multi);
    [ChatCommand("replenish_list")]
     void chatCmdReplenishList(BasePlayer player, string cmd, string[] args);
     void List(BasePlayer player);
    [ChatCommand("replenish_stop")]
     void chatCmdReplenishStop(BasePlayer player, string cmd, string[] args);
     void Stop(BasePlayer player);
     bool IsPlayerEditing(BasePlayer player);
     bool IsPlayerAdding(BasePlayer player);
     bool IsPlayerRemoving(BasePlayer player);
     bool IsPlayerTesting(BasePlayer player);
     bool IsPlayerMulti(BasePlayer player);
     void ReplyPlayer(BasePlayer player, string langKey);
     void ReplyFormatted(BasePlayer player, string msg);
     ReplenishPlayer GetReplenishPlayer(BasePlayer player);
     ReplenishableContainer GetReplenishableContainer(ItemContainer container);
     string Lang(string key);
     bool IsAllowed(BasePlayer player, string perm);
    public class ReplenishPlayer
    {
        public BasePlayer player;
        public int timer;
        public bool adding;
        public bool removing;
        public bool testing;
        public bool multi;
        public ReplenishPlayer(BasePlayer player);
        public ReplenishPlayer();
        public void StopUsing();
    }

    public class ReplenishableContainer
    {
        public uint uid;
        public string type;
        public int timer;
        public Pos pos;
        public List<ContainerItem> items;
        public ReplenishableContainer(uint uid, int timer);
        public ReplenishableContainer();
        public void SaveItems(ItemContainer container);
        public void Replenish(ItemContainer container);
        private IEnumerable<ContainerItem> GetItems(ItemContainer container);
    }

    public class ContainerItem
    {
        public int itemId;
        public int amount;
        public int ammo;
        public ulong skin;
        public float condition;
        public ContainerItem[] contents;
    }

    public class StoredData
    {
        public List<ReplenishableContainer> containers;
    }

    [System.Serializable]
    public class Pos
    {
        public float x;
        public float y;
        public float z;
        public Pos(float x, float y, float z);
        public Pos(Vector3 vec3);
        public Pos();
    }

}

public class ReplenishPlayer
{
    public BasePlayer player;
    public int timer;
    public bool adding;
    public bool removing;
    public bool testing;
    public bool multi;
    public ReplenishPlayer(BasePlayer player);
    public ReplenishPlayer();
    public void StopUsing();
}

public class ReplenishableContainer
{
    public uint uid;
    public string type;
    public int timer;
    public Pos pos;
    public List<ContainerItem> items;
    public ReplenishableContainer(uint uid, int timer);
    public ReplenishableContainer();
    public void SaveItems(ItemContainer container);
    public void Replenish(ItemContainer container);
    private IEnumerable<ContainerItem> GetItems(ItemContainer container);
}

public class ContainerItem
{
    public int itemId;
    public int amount;
    public int ammo;
    public ulong skin;
    public float condition;
    public ContainerItem[] contents;
}

public class StoredData
{
    public List<ReplenishableContainer> containers;
}

[System.Serializable]
public class Pos
{
    public float x;
    public float y;
    public float z;
    public Pos(float x, float y, float z);
    public Pos(Vector3 vec3);
    public Pos();
}


```

---

## Report

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Report", "https://topplugin.ru/", "1.0.0")]
public class Report : RustPlugin
{
    private Configuration config;
    private class Configuration
    {
        [JsonProperty("Основания для жалобы", Order = 0)]
        public List <string> Reason;
        [JsonProperty("Кнопки с основанием - ширина кнопки", Order = 1)]
        public float ReasonSizeX;
        [JsonProperty("Кнопки с основанием - отступ между кнопками", Order = 2)]
        public float ReasonSepX;
        [JsonProperty("VK - AccessToken для отправки сообщений", Order = 3)]
        public string AccessToken;
        [JsonProperty("VK - ID чата сервера, пользователя или групповой беседы. Для отправки нескольким укажите id через запятую", Order = 4)]
        public string VKServerID;
        [JsonProperty("Оповещать администрацию если количество жалоб больше чем:", Order = 5)]
        public int reportCount;
        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public static Report Instance;
    public string Layer;
    [ChatCommand("reportmenu")]
     void ReportMenu(BasePlayer player, string command, string[] args);
    private bool AlreadyReported(ulong playerID, ulong targetID);
    private void ChatCmdReport(BasePlayer player, string command, string[] args);
    private void ConsoleCmdBackPage(ConsoleSystem.Arg arg);
    private void ConsoleCmdNextPage(ConsoleSystem.Arg arg);
    private void ConsoleCmdSetTarget(ConsoleSystem.Arg arg);
    private void ShowPlayers(BasePlayer player, CuiElementContainer container, MenuData data);
    private static readonly char[] CacheString;
    public static string ClearString(string str, bool removeHtml);
    private class MenuData
    {
        public string Name;
        public BasePlayer Target;
        public BasePlayer[] Players;
        public int Page;
        public int Reason;
    }

    private readonly Dictionary<BasePlayer, MenuData> _playersMenu;
    private void ConsoleCmdInputTarget(ConsoleSystem.Arg arg);
    private void ConsoleCmdSetReason(ConsoleSystem.Arg arg);
    private void ConsoleCmdSendReport(ConsoleSystem.Arg arg);
     void SendVK(string msg);
     void PostCallback(int code, string response);
     void SendReport(BasePlayer target);
     object OnBanSystemBan(ulong steam, ulong owner, string reason, uint banTime, ulong initiator);
     void OnChatPlusMute(ulong initiator, ulong steam, string reason, uint seconds);
     void OnBanSystemUnban(ulong steam, ulong initiator);
     void Unload();
     void OnPlayerConnected(BasePlayer player);
    [HookMethod("OnCheckedPlayer")]
     void OnCheckedPlayer(BasePlayer player, BasePlayer target);
     void OnServerInitialized();
     class StoredData
    {
        public Dictionary<ulong, ReportData> players;
    }

     class ReportData
    {
        [JsonProperty("ri")]
        public List<ReportInfo> reportInfo;
    }

     class ReportInfo
    {
        public string displayName;
        public ulong userID;
        public string reason;
    }

     void SaveData();
     void LoadData();
     StoredData storedData;
    private DynamicConfigFile ReportD;
    public static bool FindPlayerByName(string findString, BasePlayer player, List<BasePlayer> players);
    private static class PlayerHelper
    {
        private static bool FindPlayerPredicate(BasePlayer player, string nameOrUserId);
        public static bool Find(string nameOrUserId, BasePlayer target);
    }

    public CuiRawImageComponent GetAvatarImageComponent(ulong user_id, string color);
    public CuiRawImageComponent GetImageComponent(string url, string shortName, string color);
    public CuiRawImageComponent GetItemImageComponent(string shortName);
    public bool AddImage(string url, string shortName);
}

private class Configuration
{
    [JsonProperty("Основания для жалобы", Order = 0)]
    public List <string> Reason;
    [JsonProperty("Кнопки с основанием - ширина кнопки", Order = 1)]
    public float ReasonSizeX;
    [JsonProperty("Кнопки с основанием - отступ между кнопками", Order = 2)]
    public float ReasonSepX;
    [JsonProperty("VK - AccessToken для отправки сообщений", Order = 3)]
    public string AccessToken;
    [JsonProperty("VK - ID чата сервера, пользователя или групповой беседы. Для отправки нескольким укажите id через запятую", Order = 4)]
    public string VKServerID;
    [JsonProperty("Оповещать администрацию если количество жалоб больше чем:", Order = 5)]
    public int reportCount;
    public static Configuration GetNewConfiguration();
}

private class MenuData
{
    public string Name;
    public BasePlayer Target;
    public BasePlayer[] Players;
    public int Page;
    public int Reason;
}

 class StoredData
{
    public Dictionary<ulong, ReportData> players;
}

 class ReportData
{
    [JsonProperty("ri")]
    public List<ReportInfo> reportInfo;
}

 class ReportInfo
{
    public string displayName;
    public ulong userID;
    public string reason;
}

private static class PlayerHelper
{
    private static bool FindPlayerPredicate(BasePlayer player, string nameOrUserId);
    public static bool Find(string nameOrUserId, BasePlayer target);
}


```

---

## ReportBot

```csharp
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("ReportBot", "Spicy", "1.0.5")]
[Description("Allows server reports to be sent over Steam to server owners.")]
 class ReportBot : CovalencePlugin
{
     string reportPermission;
     string ownerSteamID;
     string requestPageURL;
     void Init();
    protected override void LoadDefaultConfig();
     void SetupConfig();
     void SetupLang();
    [Command("report", "reportbot.report")]
     void cmdReport(IPlayer player, string command, string[] args);
}


```

---

## ReportVK

```csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Globalization;
using System.Linq;
using System.Reflection;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info ("ReportVK and AlertCheater", "BROK#", "1.1.1")]
[Description ("Reports notifications via VK.COM and alert cheater!")]
 class ReportVK : RustPlugin
{
    private Dictionary<BasePlayer, DateTime> CooldownsReport;
    private Dictionary<BasePlayer, DateTime> CooldownsSkype;
    private double CooldownReport;
    private double CooldownSkype;
    public const string permissionName;
    private Dictionary<string, bool> GUIinfo;
    private Dictionary<string, int> adminProtection;
    private void LoadMessages();
     void Loaded();
    protected override void LoadDefaultConfig();
     void Unload();
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    [ChatCommand ("report")]
     void cmdVKChat(BasePlayer player, string cmd, string[] args);
    [ChatCommand ("skype")]
     void cmdVKSkype(BasePlayer player, string cmd, string[] args);
    [ChatCommand ("cc")]
     void blindCMD(BasePlayer player, string command, string[] args);
    [ChatCommand ("uncc")]
     void unblindCMD(BasePlayer player, string command, string[] args);
    [ConsoleCommand ("alert")]
     void cmdalert(ConsoleSystem.Arg arg);
    [ConsoleCommand ("unalert")]
     void cmdunalert(ConsoleSystem.Arg arg);
     void GUIDestroy(BasePlayer player);
     void DoGUI(BasePlayer targetplayer, float length, bool indic, string message);
    private string Panel;
    public class UI
    {
        static public CuiElementContainer CreateElementContainer(string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateTextOutline(CuiElementContainer element, string panel, string text, string colorText, string colorOutline, string DistanceA, string DistanceB, int size, string aMin, string aMax, TextAnchor align);
    }

    private static List<BasePlayer> FindPlayer(string nameOrId);
     void PostCallback(int code, string response, BasePlayer player);
     string msg(string key, object userID);
}

public class UI
{
    static public CuiElementContainer CreateElementContainer(string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateTextOutline(CuiElementContainer element, string panel, string text, string colorText, string colorOutline, string DistanceA, string DistanceB, int size, string aMin, string aMax, TextAnchor align);
}


```

---

## RepRewards

```csharp
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Rep Rewards", "TopPlugin.ru", 1.0)]
[Description("Reward players for representing your server!")]
public class RepRewards : RustPlugin
{
    [PluginReference]
    private Plugin economics;
    [PluginReference]
    private Plugin serverRewards;
    private Hash<string, Timer> users;
    private void Loaded();
    private void OnPluginLoaded(Plugin plugin);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void RewardPlayer(BasePlayer player);
    protected override void LoadDefaultConfig();
}


```

---

## ResolutionAPI

```csharp
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("ResolutionAPI", "azalea`", "0.1")]
public class ResolutionAPI : RustPlugin
{
     Dictionary<ulong, string> resolutionData;
     DynamicConfigFile resolutionDataFile;
     void LoadDefaultMessages();
     string GetLangMessage(string key, string steamID);
     void Loaded();
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     void OnServerSave();
     void Unload();
     void OnPlayerInit(BasePlayer player);
     void ShowResolutionMenu(BasePlayer player);
     void CreateBox(CuiElementContainer container, string AnchorMin, string AnchorMax, string Resolution, bool Active);
    [ConsoleCommand("resolution.select")]
     void ConsoleCmd_Select(ConsoleSystem.Arg arg);
    [ChatCommand("ratio")]
     void ChatCmd_Ratio(BasePlayer player, string command, string[] args);
     object GetUserResolution(ulong userId);
}


```

---

## RespawnMessages

```csharp
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("RespawnMessages", "Kappasaurus", 0.1)]
[Description("Make customized notes players view on respawn!")]
 class RespawnMessages : RustPlugin
{
    [PluginReference]
     Plugin PopupNotifications;
     void LoadDefaultConfig();
     void OnPlayerRespawn(BasePlayer player);
}


```

---

## RestartBack

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Apex;
using Newtonsoft.Json;
using Oxide.Core;
using WebSocketSharp;

Oxide.Plugins
[Info("RestartBack", "Hougan", "0.0.1")]
public class RestartBack : RustPlugin
{
    private class Configuration
    {
        [JsonProperty("Оповещение о включение сервера (%STEAMID%, %NAME%, %LENGTH%)")]
        public string BackMessage;
        [JsonProperty("Отправлять сообщение после вайпа")]
        public bool AfterWipe;
        [JsonProperty("Также уведомлять вышедших %КОЛ-ВО% секунд назад")]
        public int DisconnectBefore;
    }

    private class RestartInfo
    {
        public double DestroyTime;
        public Dictionary<ulong, string> UsersInfo;
    }

    private Configuration Settings;
    private RestartInfo CurrentInfo;
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private void Unload();
    private void ProcessRestart();
    private double CurrentTime();
}

private class Configuration
{
    [JsonProperty("Оповещение о включение сервера (%STEAMID%, %NAME%, %LENGTH%)")]
    public string BackMessage;
    [JsonProperty("Отправлять сообщение после вайпа")]
    public bool AfterWipe;
    [JsonProperty("Также уведомлять вышедших %КОЛ-ВО% секунд назад")]
    public int DisconnectBefore;
}

private class RestartInfo
{
    public double DestroyTime;
    public Dictionary<ulong, string> UsersInfo;
}


```

---

## RestartGUI

```csharp
using System;
using System.Globalization;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Console = ConVar.Console;
using Time = Oxide.Core.Libraries.Time;

Oxide.Plugins
[Info("RestartGUI", "poof", "1.0.0")]
public class RestartGUI : RustPlugin
{
    private const string Layer;
    private static class RestartConfig
    {
        public static int Time;
        public static bool Restart;
        public static bool First;
    }

    private Configuration config;
    private class Configuration
    {
        [JsonProperty("Время рестарта")]
        public string Time;
        [JsonProperty("Время до рестарта")]
        public int RestartTime;
        [JsonProperty("Звук проигрывания")]
        public string Effect;
        [JsonProperty("Сообщение о рестарте")]
        public string RestartTimerText;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private void OnTick();
    [ConsoleCommand("reload")]
    private void ConsoleReload(ConsoleSystem.Arg arg);
    [ChatCommand("reload")]
    private void CMDReload(BasePlayer player, string cmd, string[] args);
    private void ExecuteRestart(int time);
    private void ResetConfig();
    private void DrawGUI(CuiElementContainer container);
    private void DestroyGUI();
    private string HexToRustFormat(string hex);
}

private static class RestartConfig
{
    public static int Time;
    public static bool Restart;
    public static bool First;
}

private class Configuration
{
    [JsonProperty("Время рестарта")]
    public string Time;
    [JsonProperty("Время до рестарта")]
    public int RestartTime;
    [JsonProperty("Звук проигрывания")]
    public string Effect;
    [JsonProperty("Сообщение о рестарте")]
    public string RestartTimerText;
}


```

---

## RestoreUponDeath

```csharp
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using System;

Oxide.Plugins
[Info("RestoreUponDeath", "k1lly0u", "0.1.4", ResourceId = 1859)]
 class RestoreUponDeath : RustPlugin
{
     RODData rodData;
    private DynamicConfigFile PlayerInvData;
    private Dictionary<ulong, List<SavedItem>> playerInv;
     void Loaded();
     void OnServerInitialized();
     void OnPlayerRespawned(BasePlayer player);
     void OnEntitySpawned(BaseNetworkable entity);
     void Unload();
    [ChatCommand("rod")]
    private void cmdRod(BasePlayer player, string command, string[] args);
    private void SendMSG(BasePlayer player, string key);
    private int GetPercentage(ulong playerid);
    private void SaveInventory(ulong playerid);
    private void ProcessItems(LootableCorpse corpse, int container);
    private SavedItem ProcessItem(Item item, string container);
    private void RestoreInventory(BasePlayer player);
    private void GivePlayerInventory(BasePlayer player, List<SavedItem> items);
    private void GiveItem(BasePlayer player, Item item, string container);
    private Item BuildItem(SavedItem sItem);
    private Item BuildWeapon(SavedItem sItem);
     class RODData
    {
        public Dictionary<ulong, List<SavedItem>> Inventorys;
        public Dictionary<string, int> Permissions;
    }

     class SavedItem
    {
        public string shortname;
        public int itemid;
        public string container;
        public float condition;
        public int amount;
        public int ammoamount;
        public string ammotype;
        public ulong skinid;
        public bool weapon;
        public List<SavedItem> mods;
    }

     void SaveData();
    private void SaveLoop();
     void LoadData();
    private ConfigData configData;
     class ConfigData
    {
        public int PercentageOfItemsLost { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
}

 class RODData
{
    public Dictionary<ulong, List<SavedItem>> Inventorys;
    public Dictionary<string, int> Permissions;
}

 class SavedItem
{
    public string shortname;
    public int itemid;
    public string container;
    public float condition;
    public int amount;
    public int ammoamount;
    public string ammotype;
    public ulong skinid;
    public bool weapon;
    public List<SavedItem> mods;
}

 class ConfigData
{
    public int PercentageOfItemsLost { get; set; }
}


```

---

## Rewards

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Reflection;
using Oxide.Core.Plugins;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Rewards", "Tarek", "1.3.5", ResourceId = 1961)]
[Description("Reward players for killing animals, players, other entities, and activity using Economic and/or ServerRewards")]
 class Rewards : RustPlugin
{
    [PluginReference]
     Plugin Economics;
    [PluginReference]
     Plugin ServerRewards;
    [PluginReference]
     Plugin Friends;
    [PluginReference]
     Plugin Clans;
    [PluginReference]
     Plugin HumanNPC;
    private bool IsFriendsLoaded;
    private bool IsEconomicsLoaded;
    private bool IsServerRewardsLoaded;
    private bool IsClansLoaded;
    private bool IsNPCLoaded;
    private bool HappyHourActive;
     TimeSpan hhstart;
     TimeSpan hhend;
     TimeSpan hhnow;
     StoredData storedData;
     RewardRates rr;
     Multipliers m;
     Options o;
     Rewards_Version rv;
    public List<string> Options_itemList;
    public List<string> Multipliers_itemList;
    public List<string> Rewards_itemList;
    private Rewards_Version rewardsversion;
    private RewardRates rewardrates;
    private Options options;
    private Multipliers multipliers;
    private Dictionary<BasePlayer, int> LastReward;
    private void OnServerInitialized();
    protected override void LoadDefaultConfig();
     void LoadDefaultMessages();
    private void SaveData();
     void Loaded();
    private void SetDefaultConfigValues();
    private void FixConfig();
     void Loadcfg();
     void Init();
     bool checktime(float gtime, double cfgtime);
     void OnPlayerInit(BasePlayer player);
     string Lang(string key, string id, object[] args);
     bool HasPerm(BasePlayer p, string pe);
     void SendChatMessage(BasePlayer player, string prefix, string msg, object uid);
     void BroadcastMessage(string prefix, string msg, object uid);
     void OnKillNPC(BasePlayer victim, HitInfo info);
     void OnEntityDeath(BaseCombatEntity victim, HitInfo info);
    private void RewardPlayer(BasePlayer player, double amount, double multiplier, string reason, bool isWelcomeReward);
    private static float GameTime();
    private void RewardForPlayerKill(BasePlayer player, BasePlayer victim, double multiplier);
    [ConsoleCommand("setreward")]
    private void setreward(ConsoleSystem.Arg arg);
    [ConsoleCommand("showrewards")]
    private void showrewards(ConsoleSystem.Arg arg);
    [ChatCommand("setreward")]
    private void setrewardCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("showrewards")]
    private void showrewardsCommand(BasePlayer player, string command, string[] args);
     class StoredData
    {
        public HashSet<string> Players;
        public StoredData();
    }

     class RewardRates
    {
        public double human { get; set; }
        public double bear { get; set; }
        public double wolf { get; set; }
        public double chicken { get; set; }
        public double horse { get; set; }
        public double boar { get; set; }
        public double stag { get; set; }
        public double helicopter { get; set; }
        public double autoturret { get; set; }
        public double ActivityRewardRate_minutes { get; set; }
        public double ActivityReward { get; set; }
        public double WelcomeMoney { get; set; }
        public double HappyHour_BeginHour { get; set; }
        public double HappyHour_DurationInHours { get; set; }
        public double HappyHour_EndHour { get; set; }
        public double NPCKill_Reward { get; set; }
        public double GetItemByString(string itemName);
    }

     class Multipliers
    {
        public double HuntingBow { get; set; }
        public double Crossbow { get; set; }
        public double AssaultRifle { get; set; }
        public double PumpShotgun { get; set; }
        public double SemiAutomaticRifle { get; set; }
        public double Thompson { get; set; }
        public double CustomSMG { get; set; }
        public double BoltActionRifle { get; set; }
        public double TimedExplosiveCharge { get; set; }
        public double M249 { get; set; }
        public double EokaPistol { get; set; }
        public double Revolver { get; set; }
        public double WaterpipeShotgun { get; set; }
        public double SemiAutomaticPistol { get; set; }
        public double DoubleBarrelShotgun { get; set; }
        public double SatchelCharge { get; set; }
        public double distance_50 { get; set; }
        public double distance_100 { get; set; }
        public double distance_200 { get; set; }
        public double distance_300 { get; set; }
        public double distance_400 { get; set; }
        public double HappyHourMultiplier { get; set; }
        public double VIPMultiplier { get; set; }
        public double CustomPermissionMultiplier { get; set; }
        public double LR300 { get; set; }
        public double GetWeaponM(string wn);
        public double GetDistanceM(float distance);
        public double GetItemByString(string itemName);
    }

     class Options
    {
        public bool ActivityReward_Enabled { get; set; }
        public bool WelcomeMoney_Enabled { get; set; }
        public bool WeaponMultiplier_Enabled { get; set; }
        public bool DistanceMultiplier_Enabled { get; set; }
        public bool HappyHour_Enabled { get; set; }
        public bool VIPMultiplier_Enabled { get; set; }
        public bool UseEconomicsPlugin { get; set; }
        public bool UseServerRewardsPlugin { get; set; }
        public bool UseFriendsPlugin { get; set; }
        public bool UseClansPlugin { get; set; }
        public bool Economincs_TakeMoneyFromVictim { get; set; }
        public bool ServerRewards_TakeMoneyFromVictim { get; set; }
        public bool PrintToConsole { get; set; }
        public bool CustomPermissionMultiplier_Enabled { get; set; }
        public bool NPCReward_Enabled { get; set; }
        public bool GetItemByString(string itemName);
    }

     class Rewards_Version
    {
        public string Version { get; set; }
    }

}

 class StoredData
{
    public HashSet<string> Players;
    public StoredData();
}

 class RewardRates
{
    public double human { get; set; }
    public double bear { get; set; }
    public double wolf { get; set; }
    public double chicken { get; set; }
    public double horse { get; set; }
    public double boar { get; set; }
    public double stag { get; set; }
    public double helicopter { get; set; }
    public double autoturret { get; set; }
    public double ActivityRewardRate_minutes { get; set; }
    public double ActivityReward { get; set; }
    public double WelcomeMoney { get; set; }
    public double HappyHour_BeginHour { get; set; }
    public double HappyHour_DurationInHours { get; set; }
    public double HappyHour_EndHour { get; set; }
    public double NPCKill_Reward { get; set; }
    public double GetItemByString(string itemName);
}

 class Multipliers
{
    public double HuntingBow { get; set; }
    public double Crossbow { get; set; }
    public double AssaultRifle { get; set; }
    public double PumpShotgun { get; set; }
    public double SemiAutomaticRifle { get; set; }
    public double Thompson { get; set; }
    public double CustomSMG { get; set; }
    public double BoltActionRifle { get; set; }
    public double TimedExplosiveCharge { get; set; }
    public double M249 { get; set; }
    public double EokaPistol { get; set; }
    public double Revolver { get; set; }
    public double WaterpipeShotgun { get; set; }
    public double SemiAutomaticPistol { get; set; }
    public double DoubleBarrelShotgun { get; set; }
    public double SatchelCharge { get; set; }
    public double distance_50 { get; set; }
    public double distance_100 { get; set; }
    public double distance_200 { get; set; }
    public double distance_300 { get; set; }
    public double distance_400 { get; set; }
    public double HappyHourMultiplier { get; set; }
    public double VIPMultiplier { get; set; }
    public double CustomPermissionMultiplier { get; set; }
    public double LR300 { get; set; }
    public double GetWeaponM(string wn);
    public double GetDistanceM(float distance);
    public double GetItemByString(string itemName);
}

 class Options
{
    public bool ActivityReward_Enabled { get; set; }
    public bool WelcomeMoney_Enabled { get; set; }
    public bool WeaponMultiplier_Enabled { get; set; }
    public bool DistanceMultiplier_Enabled { get; set; }
    public bool HappyHour_Enabled { get; set; }
    public bool VIPMultiplier_Enabled { get; set; }
    public bool UseEconomicsPlugin { get; set; }
    public bool UseServerRewardsPlugin { get; set; }
    public bool UseFriendsPlugin { get; set; }
    public bool UseClansPlugin { get; set; }
    public bool Economincs_TakeMoneyFromVictim { get; set; }
    public bool ServerRewards_TakeMoneyFromVictim { get; set; }
    public bool PrintToConsole { get; set; }
    public bool CustomPermissionMultiplier_Enabled { get; set; }
    public bool NPCReward_Enabled { get; set; }
    public bool GetItemByString(string itemName);
}

 class Rewards_Version
{
    public string Version { get; set; }
}


```

---

## RFTeleports

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("RFTeleports", "LAGZYA", "1.0.7")]
public class RFTeleports : RustPlugin
{
    private ConfigData cfg { get; set; }
    private class ConfigData
    {
        [JsonProperty("Разрешить телепорт c коптера?(true = да)")]
        public bool blockcopter;
        [JsonProperty("Разрешить телепорт c лошади?(true = да)")]
        public bool blockhorse;
        [JsonProperty("Разрешить телепорт c каргошипа?(true = да)")]
        public bool blockcargo;
        [JsonProperty("Разрешить телепорт c воздушного шара?(true = да)")]
        public bool blockhot;
        [JsonProperty("Разрешить телепорт во время плавания?(true = да)")]
        public bool blockswim;
        [JsonProperty("Разрешить телепорт c кровотечением?(true = да)")]
        public bool blockblood;
        [JsonProperty("Кол-во кровотечения для блока")]
        public int blood;
        [JsonProperty("Разрешить телепорт если жарко?(true = да)")]
        public bool blocktemp;
        [JsonProperty("Кол-во тепла для блока")]
        public int temp;
        [JsonProperty("Разрешить телепорт если холодно?(true = да)")]
        public bool blockсcold;
        [JsonProperty("Кол-во холода для блока")]
        public int cold;
        [JsonProperty("Разрешить телепорт если радиация?(true = да)")]
        public bool blockrad;
        [JsonProperty("Кол-во радиации для блока")]
        public int rad;
        [JsonProperty("Разрешить телепорт в запрете строительства?(true = да)")]
        public bool blockbuild;
        [JsonProperty("Скорость телепорта на спальнике(Чем больше число тем быстрее телепортация)")]
        public int sleepbeg;
        [JsonProperty("Скорость телепорта на кровате(Чем больше число тем быстрее телепортация))")]
        public int bad;
        [JsonProperty("Время перезарядки на спальнике(Секунды)")]
        public int sleepcd;
        [JsonProperty("Время перезарядки на кровате(Секунды)")]
        public int badcd;
        [JsonProperty("Включить сетхом (true = да)")]
        public bool hometeleport;
        [JsonProperty("Включить телепорт к игрокам (true = да)")]
        public bool playerteleport;
        [JsonProperty("Привелегия: кол-во домов")]
        public Dictionary<string, int> permHomeLimit;
        [JsonProperty("Привелегия: Ускорение телепорта(Чем больше число тем быстрее телепортация)")]
        public Dictionary<string, int> permTPRTIME;
        [JsonProperty("Привелегия: время перезарядки тп к игроку")]
        public Dictionary<string, int> permTPRCD;
        public static ConfigData GetNewConf();
    }

    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
     int GetHomeLimit(ulong uid);
    private Dictionary<ulong, PlayerData> _homeList;
     void OnServerSave();
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void OnItemAddedToContainer(ItemContainer container, Item item);
    private void Unload();
     class PlayerData
    {
        public string NickName;
        public double HOMECD;
        public double TPRCD;
        public Dictionary<int, Homes> _homeList;
        public double IsTPR();
        public double IsHome();
    }

     class Homes
    {
        public Vector3 position;
        public string ShortName;
        public uint netId;
    }

    public static RFTeleports ins;
    private void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
    private string AcceptLayer;
     void AcceptHouse(BasePlayer player, uint numid);
     void HomeGenerate(uint nument, BasePlayer player);
     void OnEntitySpawned(SleepingBag entity);
     void Init();
    [ConsoleCommand("rfteleport")]
     void RFCommand(ConsoleSystem.Arg arg);
    private static string Blur;
     Dictionary<ulong, int> _pagerList;
     class RFPager : MonoBehaviour
    {
        private BasePlayer player;
        private static PagerEntity pager;
        private int num;
        private void Awake();
        private void Update();
        public void OnDestroy();
    }

    public class RFTeleport : MonoBehaviour
    {
        private BasePlayer playerTarget;
        private Item rftrans;
        private Detonator racia;
        private BasePlayer player;
        private double procent;
        private PlayerData playerData;
        private Dictionary<int, Homes> homes;
        private string color;
        private int tprtime;
        private string text;
        private void Awake();
        public void OnDestroy();
        private void StartUI();
        private void DrawUi(int time, int max);
        private void DrawUiCD(int time, int max);
         bool CheckPlayer(BasePlayer basePlayer);
         int GetTprCD(ulong uid);
         int GetTPRTime(ulong uid);
        private void Update();
        private void Teleport(BasePlayer player, Vector3 pos);
    }

    [ChatCommand("sethome")]
     void SetHome(BasePlayer player);
    private void Teleport(BasePlayer player, Vector3 pos);
    [ChatCommand("tp")]
     void AdminTeleport(BasePlayer p, string c, string[] a);
    [ChatCommand("tpr")]
     void Tpr(BasePlayer p, string c, string[] a);
    [ChatCommand("tpa")]
     void Tpa(BasePlayer p, string c, string[] a);
    [ChatCommand("tpc")]
     void Tpc(BasePlayer p, string c, string[] a);
    [ChatCommand("home")]
     void Home(BasePlayer p, string c, string[] a);
     string getGrid(Vector3 pos);
    private void ReplySend(BasePlayer player, string message);
    private static DateTime epoch;
    private string HexToRustFormat(string hex);
    private static double CurrentTime();
}

private class ConfigData
{
    [JsonProperty("Разрешить телепорт c коптера?(true = да)")]
    public bool blockcopter;
    [JsonProperty("Разрешить телепорт c лошади?(true = да)")]
    public bool blockhorse;
    [JsonProperty("Разрешить телепорт c каргошипа?(true = да)")]
    public bool blockcargo;
    [JsonProperty("Разрешить телепорт c воздушного шара?(true = да)")]
    public bool blockhot;
    [JsonProperty("Разрешить телепорт во время плавания?(true = да)")]
    public bool blockswim;
    [JsonProperty("Разрешить телепорт c кровотечением?(true = да)")]
    public bool blockblood;
    [JsonProperty("Кол-во кровотечения для блока")]
    public int blood;
    [JsonProperty("Разрешить телепорт если жарко?(true = да)")]
    public bool blocktemp;
    [JsonProperty("Кол-во тепла для блока")]
    public int temp;
    [JsonProperty("Разрешить телепорт если холодно?(true = да)")]
    public bool blockсcold;
    [JsonProperty("Кол-во холода для блока")]
    public int cold;
    [JsonProperty("Разрешить телепорт если радиация?(true = да)")]
    public bool blockrad;
    [JsonProperty("Кол-во радиации для блока")]
    public int rad;
    [JsonProperty("Разрешить телепорт в запрете строительства?(true = да)")]
    public bool blockbuild;
    [JsonProperty("Скорость телепорта на спальнике(Чем больше число тем быстрее телепортация)")]
    public int sleepbeg;
    [JsonProperty("Скорость телепорта на кровате(Чем больше число тем быстрее телепортация))")]
    public int bad;
    [JsonProperty("Время перезарядки на спальнике(Секунды)")]
    public int sleepcd;
    [JsonProperty("Время перезарядки на кровате(Секунды)")]
    public int badcd;
    [JsonProperty("Включить сетхом (true = да)")]
    public bool hometeleport;
    [JsonProperty("Включить телепорт к игрокам (true = да)")]
    public bool playerteleport;
    [JsonProperty("Привелегия: кол-во домов")]
    public Dictionary<string, int> permHomeLimit;
    [JsonProperty("Привелегия: Ускорение телепорта(Чем больше число тем быстрее телепортация)")]
    public Dictionary<string, int> permTPRTIME;
    [JsonProperty("Привелегия: время перезарядки тп к игроку")]
    public Dictionary<string, int> permTPRCD;
    public static ConfigData GetNewConf();
}

 class PlayerData
{
    public string NickName;
    public double HOMECD;
    public double TPRCD;
    public Dictionary<int, Homes> _homeList;
    public double IsTPR();
    public double IsHome();
}

 class Homes
{
    public Vector3 position;
    public string ShortName;
    public uint netId;
}

 class RFPager : MonoBehaviour
{
    private BasePlayer player;
    private static PagerEntity pager;
    private int num;
    private void Awake();
    private void Update();
    public void OnDestroy();
}

public class RFTeleport : MonoBehaviour
{
    private BasePlayer playerTarget;
    private Item rftrans;
    private Detonator racia;
    private BasePlayer player;
    private double procent;
    private PlayerData playerData;
    private Dictionary<int, Homes> homes;
    private string color;
    private int tprtime;
    private string text;
    private void Awake();
    public void OnDestroy();
    private void StartUI();
    private void DrawUi(int time, int max);
    private void DrawUiCD(int time, int max);
     bool CheckPlayer(BasePlayer basePlayer);
     int GetTprCD(ulong uid);
     int GetTPRTime(ulong uid);
    private void Update();
    private void Teleport(BasePlayer player, Vector3 pos);
}


```

---

## RGive

```csharp
using System.Text.RegularExpressions;
using System.Collections.Generic;
using System.Linq;
using System;

Oxide.Plugins
[Info("RGive", "LaserHydra", "2.0.2", ResourceId = 929)]
[Description("Random item giving")]
 class RGive : RustPlugin
{
     class Category : Dictionary<string, object>
    {
        public Category(int MinimalAmount, int MaximalAmount, bool Enabled);
    }

     void Loaded();
     void LoadConfig();
    protected override void LoadDefaultConfig();
     object GetRandomAmount(string category);
     BasePlayer GetRandomPlayer();
     void GiveRandomItem(BasePlayer player);
     void GiveItem(Item item, int amount, BasePlayer player);
    [ChatCommand("rgive")]
     void cmdRGive(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("rgive")]
     void ccmdRGive(ConsoleSystem.Arg arg);
     Item GetItem(string searchedItem, BasePlayer player);
     BasePlayer GetPlayer(string searchedPlayer, BasePlayer player);
     string ListToString(List<T> list, int first, string seperator);
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
     string StripTags(string original);
}

 class Category : Dictionary<string, object>
{
    public Category(int MinimalAmount, int MaximalAmount, bool Enabled);
}


```

---

## Robbery

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Robbery", "Wulf/lukespragg", "4.0.1", ResourceId = 736)]
[Description("Players can steal money, points, and/or items from other players")]
 class Robbery : RustPlugin
{
    [PluginReference]
     Plugin Clans;
    [PluginReference]
     Plugin Economics;
    [PluginReference]
     Plugin EventManager;
    [PluginReference]
     Plugin Factions;
    [PluginReference]
     Plugin Friends;
    [PluginReference]
     Plugin RustIO;
    [PluginReference]
     Plugin ServerRewards;
    [PluginReference]
     Plugin UEconomics;
    [PluginReference]
     Plugin ZoneManager;
    readonly Hash<string, float> cooldowns;
    const string permKilling;
    const string permMugging;
    const string permPickpocket;
    const string permProtection;
     bool clanProtection;
     bool friendProtection;
     bool itemStealing;
     bool moneyStealing;
     bool pointStealing;
     float percentAwake;
     float percentSleeping;
     int usageCooldown;
    protected override void LoadDefaultConfig();
     void Init();
     void LoadDefaultMessages();
     void StealPoints(BasePlayer victim, BasePlayer attacker);
     void StealMoney(BasePlayer victim, BasePlayer attacker);
     void StealItem(BasePlayer victim, BasePlayer attacker);
     bool InNoLootZone(BasePlayer victim, BasePlayer attacker);
     bool IsFriend(BasePlayer victim, BasePlayer attacker);
     bool IsClanmate(BasePlayer victim, BasePlayer attacker);
     void OnEntityDeath(BaseEntity entity, HitInfo info);
     void OnEntityTakeDamage(BaseEntity entity, HitInfo info);
     void OnPlayerInput(BasePlayer attacker, InputState input);
     T GetConfig(string name, T value);
     string Lang(string key, string id, object[] args);
    static BaseEntity FindObject(Ray ray, float distance);
}


```

---

## RockBlock

```csharp
using UnityEngine;

Oxide.Plugins
[Info("RockBlock", "Nogrod", "1.0.4")]
 class RockBlock : RustPlugin
{
    private readonly int worldLayer;
    private const BaseNetworkable.DestroyMode DestroyMode;
    private ConfigData configData;
     class ConfigData
    {
        public int MaxHeight { get; set; }
        public bool AllowCave { get; set; }
    }

    protected override void LoadDefaultConfig();
    private void Init();
     void OnEntityBuilt(Planner planner, GameObject gameObject);
     void OnItemDeployed(Deployer deployer, BaseEntity entity);
    private void CheckEntity(BaseEntity entity, BasePlayer player);
    private bool IsInCave(BaseEntity entity, BasePlayer player);
    private static bool IsInside(Collider collider, BaseEntity entity);
}

 class ConfigData
{
    public int MaxHeight { get; set; }
    public bool AllowCave { get; set; }
}


```

---

## RocketTurrets

```csharp
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Rust;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using UnityEngine;

Oxide.Plugins
[Info("RocketTurrets", "redBDGR", "1.3.15")]
[Description("Create rocket shooting turrets")]
 class RocketTurrets : RustPlugin
{
    private bool changed;
    private List<AutoTurret> rocketTurrets;
    private List<AutoTurret> antiairTurrets;
    private List<AutoTurret> javelinTurrets;
    private Dictionary<string, PlayerData> turretNumbers;
    private List<ulong> turretIDs;
    private List<ulong> antiAirIDs;
    private List<ulong> javelinIDs;
    private static RocketTurrets plugin;
    [PluginReference]
     Plugin RemoteTurrets;
    public static LayerMask collLayers;
    private float rocketFireRate;
    private float rocketLockonTime;
    private float minHitDistance;
    private float rocketLockonRadius;
    private float rocketSpeed;
    private uint rocketLauncherSkin;
    private float explosionRadius;
    private bool explodeAtTarget;
    private int lockonSoundEveryxUpdate;
    private bool requiresAmmo;
    private int maxNumberAllowed;
    private bool rocketHeatSeeking;
    private bool customRocketTrail;
    private float aaFireRate;
    private float aaLockonTime;
    private float aaminHitDistance;
    private float aaLockonRadius;
    private float aarocketSpeed;
    private uint aaLauncherSkin;
    private float aaexplosionRadius;
    private int aalockonSoundEveryxUpdate;
    private float damageToHeli;
    private bool aaRequiresAmmo;
    private int aaMaxNumberAllowed;
    private bool aaRequireBuildingRightsToAccessInventory;
    private bool aaCustomRocketTrail;
    private float jrocketFireRate;
    private float jrocketLockonTime;
    private float jminHitDistance;
    private float jrocketLockonRadius;
    private float jrocketSpeed;
    private uint jrocketLauncherSkin;
    private float jexplosionRadius;
    private int jlockonSoundEveryxUpdate;
    private bool jrequiresAmmo;
    private int jmaxNumberAllowed;
    private bool jSeekTarget;
    private float jHeightBeforeCurve;
    private bool jCustomRocketTrail;
    private bool returnCostOnPickup;
    private bool returnCostOnDowngrade;
    private static Dictionary<string, object> RocketUpgradeCost();
    private static Dictionary<string, object> AntiAirUpgradeCost();
    private static Dictionary<string, object> JavelinUpgradeCost();
    private Dictionary<string, object> rocketUpgradeCost;
    private Dictionary<string, object> antiAirUpgradeCost;
    private Dictionary<string, object> javelinUpgradeCost;
    private const string permissionName;
    private const string permissionNameROCKET;
    private const string permissionNameANTIAIR;
    private const string permissionNameJAVELIN;
    private const string permissionNameUNLIMITED;
    private const string permissionNameAMMOUNLIMITED;
    private class StoredData
    {
        public List<ulong> turretIDs;
        public List<ulong> antiAirIDs;
        public List<ulong> javelinIDs;
    }

    private class StoredData2
    {
        public Dictionary<string, PlayerData> turretNumbers;
    }

    private class PlayerData
    {
        public int RocketTurrets;
        public int AntiAirTurrets;
        public int JavelinTurrets;
    }

    private DynamicConfigFile data;
    private DynamicConfigFile data2;
    private StoredData storedData;
    private StoredData2 storedData2;
    private void OnServerSave();
    private void SaveData();
    private void LoadData();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private bool IsRocketTurret(AutoTurret turret);
    private void ToggleTurretAutomation(AutoTurret turret, bool enabled);
    private void TryFireRocket(AutoTurret turret);
    private class RocketTurret : MonoBehaviour
    {
        private float aimRadius;
        private float fireRate;
        private float rocketSpeed;
        private float minHitDistance;
        private float lockonTime;
        private uint rocketLauncherSkin;
        private float explosionRadius;
        private bool explodeOnTargetPos;
        private int soundEveryxUpdates;
        private bool requiresAmmo;
        private bool heatSeek;
        private int updateCounter;
        private bool isLockedOn;
        private bool isLockingOn;
        private float lockonFinish;
        private float nextShoot;
        private AutoTurret turret;
        private BaseCombatEntity target;
        private DroppedItem launcher_1;
        private DroppedItem rocket_1;
        private DroppedItem rocket_2;
        private DestroyOnGroundMissing desGround;
        private GroundWatch groundWatch;
        private void Awake();
        private void Update();
        private void OnDestroy();
        public void Destroy();
        private void DestroyTurret();
        public void UnloadDestroy();
        private Item HasAmmo();
        private void LockToTarget(BaseCombatEntity entity);
        private void InitAesthetics();
        public void TryFireRocket();
        private void FireRocket(Item rocketItem);
        private ServerProjectile CreateRocket(Vector3 launchPos, Item rocketItem);
    }

    private class AntiAirTurret : MonoBehaviour
    {
        private float aimRadius;
        private float fireRate;
        private float rocketSpeed;
        private float minHitDistance;
        private float lockonTime;
        private uint rocketLauncherSkin;
        private float explosionRadius;
        private int soundEveryxUpdates;
        private float damage;
        private bool requiresAmmo;
        private int updateCounter;
        private bool isLockedOn;
        private bool isLockingOn;
        private float lockonFinish;
        private float nextShoot;
        private AutoTurret turret;
        private BaseEntity entity;
        private BaseCombatEntity target;
        private SphereCollider sCol;
        private DroppedItem launcher_1;
        private DroppedItem rocket_1;
        private DroppedItem rocket_2;
        private DroppedItem computer;
        private DroppedItem camera_1;
        private DroppedItem camera_2;
        private void Awake();
        private void Update();
        private void OnTriggerEnter(Collider col);
        private void OnTriggerExit(Collider col);
        private void OnDestroy();
        public void Destroy();
        private void DestroyTurret();
        public void UnloadDestroy();
        private Item HasAmmo();
        private void LockToTarget(BaseCombatEntity _entity);
        private void InitAesthetics();
        public void TryFireRocket();
        private void FireRocket(Item rocketItem);
        private ServerProjectile CreateRocket(Vector3 launchPos, Item rocketItem);
    }

    private class JavelinTurret : MonoBehaviour
    {
        private float aimRadius;
        private float fireRate;
        private float rocketSpeed;
        private float minHitDistance;
        private float lockonTime;
        private uint rocketLauncherSkin;
        private float explosionRadius;
        private int soundEveryxUpdates;
        private bool requiresAmmo;
        private bool seekTarget;
        private int updateCounter;
        private bool isLockedOn;
        private bool isLockingOn;
        private float lockonFinish;
        private float nextShoot;
        private AutoTurret turret;
        private BaseCombatEntity target;
        private BaseCombatEntity externalTarget;
        private DroppedItem launcher_1;
        private DroppedItem rocket_1;
        private DroppedItem rocket_2;
        private DestroyOnGroundMissing desGround;
        private GroundWatch groundWatch;
        private void Awake();
        private void Update();
        private void OnTriggerEnter(Collider col);
        private void OnTriggerExit(Collider col);
        private void OnDestroy();
        public void Destroy();
        private void DestroyTurret();
        public void UnloadDestroy();
        private Item HasAmmo();
        private void LockToTarget(BaseCombatEntity entity);
        private void InitAesthetics();
        public void TryFireRocket();
        private void FireRocket(Item rocketItem);
        private ServerProjectile CreateRocket(Vector3 launchPos, Item rocketItem);
    }

    private class HeatRocket : MonoBehaviour
    {
        private ServerProjectile rocket;
        private BaseEntity trail;
        private TimedExplosive expl;
        public BaseCombatEntity target;
        private void Awake();
        private void FixedUpdate();
    }

    private class JavelinMissile : MonoBehaviour
    {
        public BaseNetworkable targetEntity;
        private BaseEntity trail;
        public Vector3 targetPos;
        private ServerProjectile rocket;
        private TimedExplosive expl;
        private float verticality;
        private bool seekTarget;
        private Vector3 startPos;
        private Vector3 endPos;
        private Vector3 center;
        private Vector3 vertPos;
        private Vector3 lastPos;
        private Vector3 nextPos;
        private Vector3 cachedPosition;
        private float startTime;
        private float journeyTime;
        private float fracComplete;
        private bool start;
        private bool finishedVertical;
        private void Awake();
        private void FixedUpdate();
    }

    private void Init();
    private void Loaded();
    private void Unload();
    private void OnServerInitialized();
    private IEnumerator InitTurrets();
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private object OnTurretStartup(AutoTurret turret);
    private object CanAcceptItem(ItemContainer container, Item item);
    private void CanPickupEntity(BaseCombatEntity entity, BasePlayer player);
    private void OnEntityKill(BaseNetworkable entity);
    [ChatCommand("turret")]
    private void RocketTurretCMD(BasePlayer player, string command, string[] args);
    private static void ReturnItems(Vector3 dropPos, Dictionary<string, object> dic);
    private static void DropAmmo(AutoTurret turret);
    private void DoHelpMenu(BasePlayer player);
    private void DoItemMenu(BasePlayer player);
    private bool CanUpgade(ItemContainer container, ItemContainer container2, string type);
    private static void RemoveItems(Dictionary<Item, int> dic);
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private string msg(string key, string id);
}

private class StoredData
{
    public List<ulong> turretIDs;
    public List<ulong> antiAirIDs;
    public List<ulong> javelinIDs;
}

private class StoredData2
{
    public Dictionary<string, PlayerData> turretNumbers;
}

private class PlayerData
{
    public int RocketTurrets;
    public int AntiAirTurrets;
    public int JavelinTurrets;
}

private class RocketTurret : MonoBehaviour
{
    private float aimRadius;
    private float fireRate;
    private float rocketSpeed;
    private float minHitDistance;
    private float lockonTime;
    private uint rocketLauncherSkin;
    private float explosionRadius;
    private bool explodeOnTargetPos;
    private int soundEveryxUpdates;
    private bool requiresAmmo;
    private bool heatSeek;
    private int updateCounter;
    private bool isLockedOn;
    private bool isLockingOn;
    private float lockonFinish;
    private float nextShoot;
    private AutoTurret turret;
    private BaseCombatEntity target;
    private DroppedItem launcher_1;
    private DroppedItem rocket_1;
    private DroppedItem rocket_2;
    private DestroyOnGroundMissing desGround;
    private GroundWatch groundWatch;
    private void Awake();
    private void Update();
    private void OnDestroy();
    public void Destroy();
    private void DestroyTurret();
    public void UnloadDestroy();
    private Item HasAmmo();
    private void LockToTarget(BaseCombatEntity entity);
    private void InitAesthetics();
    public void TryFireRocket();
    private void FireRocket(Item rocketItem);
    private ServerProjectile CreateRocket(Vector3 launchPos, Item rocketItem);
}

private class AntiAirTurret : MonoBehaviour
{
    private float aimRadius;
    private float fireRate;
    private float rocketSpeed;
    private float minHitDistance;
    private float lockonTime;
    private uint rocketLauncherSkin;
    private float explosionRadius;
    private int soundEveryxUpdates;
    private float damage;
    private bool requiresAmmo;
    private int updateCounter;
    private bool isLockedOn;
    private bool isLockingOn;
    private float lockonFinish;
    private float nextShoot;
    private AutoTurret turret;
    private BaseEntity entity;
    private BaseCombatEntity target;
    private SphereCollider sCol;
    private DroppedItem launcher_1;
    private DroppedItem rocket_1;
    private DroppedItem rocket_2;
    private DroppedItem computer;
    private DroppedItem camera_1;
    private DroppedItem camera_2;
    private void Awake();
    private void Update();
    private void OnTriggerEnter(Collider col);
    private void OnTriggerExit(Collider col);
    private void OnDestroy();
    public void Destroy();
    private void DestroyTurret();
    public void UnloadDestroy();
    private Item HasAmmo();
    private void LockToTarget(BaseCombatEntity _entity);
    private void InitAesthetics();
    public void TryFireRocket();
    private void FireRocket(Item rocketItem);
    private ServerProjectile CreateRocket(Vector3 launchPos, Item rocketItem);
}

private class JavelinTurret : MonoBehaviour
{
    private float aimRadius;
    private float fireRate;
    private float rocketSpeed;
    private float minHitDistance;
    private float lockonTime;
    private uint rocketLauncherSkin;
    private float explosionRadius;
    private int soundEveryxUpdates;
    private bool requiresAmmo;
    private bool seekTarget;
    private int updateCounter;
    private bool isLockedOn;
    private bool isLockingOn;
    private float lockonFinish;
    private float nextShoot;
    private AutoTurret turret;
    private BaseCombatEntity target;
    private BaseCombatEntity externalTarget;
    private DroppedItem launcher_1;
    private DroppedItem rocket_1;
    private DroppedItem rocket_2;
    private DestroyOnGroundMissing desGround;
    private GroundWatch groundWatch;
    private void Awake();
    private void Update();
    private void OnTriggerEnter(Collider col);
    private void OnTriggerExit(Collider col);
    private void OnDestroy();
    public void Destroy();
    private void DestroyTurret();
    public void UnloadDestroy();
    private Item HasAmmo();
    private void LockToTarget(BaseCombatEntity entity);
    private void InitAesthetics();
    public void TryFireRocket();
    private void FireRocket(Item rocketItem);
    private ServerProjectile CreateRocket(Vector3 launchPos, Item rocketItem);
}

private class HeatRocket : MonoBehaviour
{
    private ServerProjectile rocket;
    private BaseEntity trail;
    private TimedExplosive expl;
    public BaseCombatEntity target;
    private void Awake();
    private void FixedUpdate();
}

private class JavelinMissile : MonoBehaviour
{
    public BaseNetworkable targetEntity;
    private BaseEntity trail;
    public Vector3 targetPos;
    private ServerProjectile rocket;
    private TimedExplosive expl;
    private float verticality;
    private bool seekTarget;
    private Vector3 startPos;
    private Vector3 endPos;
    private Vector3 center;
    private Vector3 vertPos;
    private Vector3 lastPos;
    private Vector3 nextPos;
    private Vector3 cachedPosition;
    private float startTime;
    private float journeyTime;
    private float fracComplete;
    private bool start;
    private bool finishedVertical;
    private void Awake();
    private void FixedUpdate();
}


```

---

## RockEvent

```csharp
using System;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("RockEvent", "Drop Dead & Deversive", "1.0.5")]
public class RockEvent : RustPlugin
{
    [PluginReference]
    private Plugin ComponentsEvent;
     bool EventHasStart;
     string WorkLayer;
     DateTime canceldate;
     int count;
     int allcount;
    private List<BaseEntity> SpawnedStones;
    private HashSet<Tuple<Vector3, Quaternion>> _spawnData;
    private const int ScanHeight;
    private static int GetBlockMask { get; set; }
    private static bool MaskIsBlocked(int mask);
    private Dictionary<MonumentInfo, float> monuments { get; set; }
    private PluginConfig cfg;
    public class PluginConfig
    {
        [JsonProperty("Основные настройки")]
        public Settings MainSettings;
        [JsonProperty("Настройки выигрышей")]
        public AccesSets AccesSettings;
        [JsonProperty("Дополнительные настройки")]
        public AdditionalSettings AddSettings;
        public class Settings
        {
            [JsonProperty("Включить автоматичесский старт ивента?")]
            public bool AutoStartEvent;
            [JsonProperty("Включить ли минимальное количество игроков для старта ивента?")]
            public bool MinPlayers;
            [JsonProperty("Минимальное количество игроков для старта ивента")]
            public int MinPlayersCount;
            [JsonProperty("Время для начала ивента после старта сервера, перезагрузки плагина (первый раз)")]
            public float FirstStartTime;
            [JsonProperty("Время для начала ивента в последующие разы (второй, третий и тд)")]
            public float RepeatTime;
            [JsonProperty("Время до конца ивента (в минутах)")]
            public double EventDuration;
        }

        public class AccesSets
        {
            [JsonProperty("Название пермишна для использования команды /rockevent (с приставкой RockEvent)")]
            public string StartPermission;
        }

        public class AdditionalSettings
        {
            [JsonProperty("Цвет выделения текста в интерфейсе")]
            public string Color;
        }

    }

    private void Init();
    protected override void LoadDefaultConfig();
     void Unload();
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
     void OnEntityDeath(BaseEntity entity, HitInfo info);
    [HookMethod("RockEventIsStart")]
    public object RockEventIsStart();
     bool ComponentsEventIsStart();
     void StartEvent();
     void StopEvent();
     void UpdateUI();
    private void GeneratePositions();
    private bool IsMonumentPosition(Vector3 target);
    private void SetupMonuments();
    private float GetMonumentFloat(string monumentName);
    private static bool InRange(Vector3 a, Vector3 b, float distance, bool ex);
    private Tuple<Vector3, Quaternion> GetClosestValidPosition(Vector3 original);
    private bool IsValidPosition(Vector3 position);
     bool IsOnRoad(Vector3 target);
     string RandomStonePrefab();
     void SpawnStone();
    private int GenerateStones();
    private Tuple<Vector3, Quaternion> GetValidSpawnData();
    [ConsoleCommand("rockevent.start")]
    private void ConsoleForcedEventStart(ConsoleSystem.Arg args);
    [ConsoleCommand("rockevent.stop")]
    private void ConsoleForcedEventStop(ConsoleSystem.Arg args);
    [ConsoleCommand("rockevent.ui.close")]
     void ConsoleUIClose(ConsoleSystem.Arg args);
    [ConsoleCommand("rockevent.ui.open")]
     void ConsoleUIOpen(ConsoleSystem.Arg args);
    [ChatCommand("rockevent")]
     void ChatForcedEventHandler(BasePlayer player, string command, string[] args);
     void EventLog(string text);
    private void InitializeUI(BasePlayer player);
}

public class PluginConfig
{
    [JsonProperty("Основные настройки")]
    public Settings MainSettings;
    [JsonProperty("Настройки выигрышей")]
    public AccesSets AccesSettings;
    [JsonProperty("Дополнительные настройки")]
    public AdditionalSettings AddSettings;
    public class Settings
    {
        [JsonProperty("Включить автоматичесский старт ивента?")]
        public bool AutoStartEvent;
        [JsonProperty("Включить ли минимальное количество игроков для старта ивента?")]
        public bool MinPlayers;
        [JsonProperty("Минимальное количество игроков для старта ивента")]
        public int MinPlayersCount;
        [JsonProperty("Время для начала ивента после старта сервера, перезагрузки плагина (первый раз)")]
        public float FirstStartTime;
        [JsonProperty("Время для начала ивента в последующие разы (второй, третий и тд)")]
        public float RepeatTime;
        [JsonProperty("Время до конца ивента (в минутах)")]
        public double EventDuration;
    }

    public class AccesSets
    {
        [JsonProperty("Название пермишна для использования команды /rockevent (с приставкой RockEvent)")]
        public string StartPermission;
    }

    public class AdditionalSettings
    {
        [JsonProperty("Цвет выделения текста в интерфейсе")]
        public string Color;
    }

}

public class Settings
{
    [JsonProperty("Включить автоматичесский старт ивента?")]
    public bool AutoStartEvent;
    [JsonProperty("Включить ли минимальное количество игроков для старта ивента?")]
    public bool MinPlayers;
    [JsonProperty("Минимальное количество игроков для старта ивента")]
    public int MinPlayersCount;
    [JsonProperty("Время для начала ивента после старта сервера, перезагрузки плагина (первый раз)")]
    public float FirstStartTime;
    [JsonProperty("Время для начала ивента в последующие разы (второй, третий и тд)")]
    public float RepeatTime;
    [JsonProperty("Время до конца ивента (в минутах)")]
    public double EventDuration;
}

public class AccesSets
{
    [JsonProperty("Название пермишна для использования команды /rockevent (с приставкой RockEvent)")]
    public string StartPermission;
}

public class AdditionalSettings
{
    [JsonProperty("Цвет выделения текста в интерфейсе")]
    public string Color;
}


```

---

## RotatingBillboards

```csharp
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using UnityEngine;
using System.Reflection;

Oxide.Plugins
[Info("RotatingBillboards", "k1lly0u", "0.1.0", ResourceId = 0)]
 class RotatingBillboards : RustPlugin
{
     StoredData storedData;
    private DynamicConfigFile data;
    private List<Rotator> billBoards;
    static RotatingBillboards instance;
    private Vector3 eyesAdjust;
    private FieldInfo serverinput;
     void Loaded();
     void OnServerInitialized();
     void OnEntityKill(BaseNetworkable netEntity);
     void Unload();
     class Rotator : Signage
    {
        private Signage entity;
        private float secsToTake;
        private float secsTaken;
        private Vector3 initialRot;
        private Vector3 startRot;
        private Vector3 endRot;
        private bool isRotating;
         void Awake();
         void Destroy();
         void FixedUpdate();
        public void ToggleRotation();
        public bool IsRotating();
    }

     void FindAllEntities();
     object FindEntityFromRay(BasePlayer player);
    [ChatCommand("rot")]
     void cmdRot(BasePlayer player, string command, string[] args);
     string msg(string key, string userId);
     Dictionary<string, string> Messages;
    private ConfigData configData;
     class ConfigData
    {
        public float RotationSpeed { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     void SaveData();
     void LoadData();
     class StoredData
    {
        public List<uint> data;
    }

}

 class Rotator : Signage
{
    private Signage entity;
    private float secsToTake;
    private float secsTaken;
    private Vector3 initialRot;
    private Vector3 startRot;
    private Vector3 endRot;
    private bool isRotating;
     void Awake();
     void Destroy();
     void FixedUpdate();
    public void ToggleRotation();
    public bool IsRotating();
}

 class ConfigData
{
    public float RotationSpeed { get; set; }
}

 class StoredData
{
    public List<uint> data;
}


```

---

## RPGFarm

```csharp
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("RPGFarm", "Replace ServerRust", "0.1.1")]
public class RPGFarm : RustPlugin
{
    private static readonly string _permissionMax;
     Dictionary<ulong, string> playerResolution;
    readonly DynamicConfigFile dataFile;
     Dictionary<ulong, UserStats> userStats;
     Dictionary<Skills, double> MaxRate;
     Dictionary<Skills, int> MaxHits;
     Dictionary<Skills, string> SkillTranslate;
     Dictionary<Skills, string> skillColors;
     class UserStats
    {
        public Dictionary<Skills, int> Hits;
    }

    private void OnServerInitialized();
     void Loaded();
     void Unload();
     void OnServerSave();
     void OnPlayerConnected(BasePlayer player);
     void OnUserPermissionGranted(string id, string perm);
     void OnUserPermissionRevoked(string id, string perm);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void OnQuarryGather(MiningQuarry quarry, Item item);
     void OnCollectiblePickup(Item item, BasePlayer player);
     void RateHandler(BasePlayer player, Item item, Skills skill);
    private void RenderUI(BasePlayer player);
    private void FillElements(CuiElementContainer elements, Skills skill, string mainPanel, int rowNumber, int maxRows, UserStats stats, int fontSize);
     double GetCurrentRate(Skills skill, int Hits);
     int GetCurrentPercent(Skills skill, int Hits);
     int GetLeftHits(Skills skill, int Hits);
}

 class UserStats
{
    public Dictionary<Skills, int> Hits;
}


```

---

## RPSBattles

```csharp
using System.Collections.Generic;
using System.Reflection;
using System;
using Oxide.Core;
using UnityEngine;
using System.Linq;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("RPS Battles", "PaiN", "0.3", ResourceId = 1929)]
[Description("Rock Paper Scissors game")]
 class RPSBattles : RustPlugin
{
    [PluginReference]
     Plugin Economics;
    private static FieldInfo _condition;
     void Loaded();
     string sendlang(string key, string userID);
     void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
     void Unloaded();
     class Data
    {
        public List<BattleInfo> Battles;
    }

     Data data;
     void SaveData();
    public class BattleInfo
    {
        public int battleid;
        public string ccreatorname;
        public string citemtype;
        public string citemfullname;
        public int citemid;
        public int ciamount;
        public bool cisbp;
        public string cRPSbet;
        public ulong csteamid;
        public string citemshortname;
        public int cicondition;
        public string eplayername;
        public string eitemtype;
        public string eitemfullname;
        public int eitemid;
        public int eiamount;
        public bool eisbp;
        public string eRPSbet;
        public ulong esteamid;
        public string eitemshortname;
        public int eicondition;
        public bool enabled;
        public string status;
        public BattleInfo(string stats, string bet, int id, BasePlayer player, Item item, bool rpsenabled);
        public BattleInfo();
    }

     void UseUI(BasePlayer player, string itemurl, string itemurl2, string rightginfo, string leftginfo);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnPlayerInit(BasePlayer player);
     int GetNewId(string battle);
     int GetPlayerBattleCount(BasePlayer player);
    [ChatCommand("rps")]
     void cmdRPS(BasePlayer player, string cmd, string[] args);
     string GetItemImage(string itemname);
     string GetFullChoise(string s);
     void Win(BasePlayer player, BattleInfo info, string rpschoice);
     void Lose(BasePlayer player, BattleInfo info);
     void DisplayGUI(BasePlayer player, string enifullname, int eniamount, int enicondition, string crifullname, int criamount, int cricondition, string lchoice, string rchoice);
     T GetConfig(string name, T defaultValue);
}

 class Data
{
    public List<BattleInfo> Battles;
}

public class BattleInfo
{
    public int battleid;
    public string ccreatorname;
    public string citemtype;
    public string citemfullname;
    public int citemid;
    public int ciamount;
    public bool cisbp;
    public string cRPSbet;
    public ulong csteamid;
    public string citemshortname;
    public int cicondition;
    public string eplayername;
    public string eitemtype;
    public string eitemfullname;
    public int eitemid;
    public int eiamount;
    public bool eisbp;
    public string eRPSbet;
    public ulong esteamid;
    public string eitemshortname;
    public int eicondition;
    public bool enabled;
    public string status;
    public BattleInfo(string stats, string bet, int id, BasePlayer player, Item item, bool rpsenabled);
    public BattleInfo();
}


```

---

## RSVote

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("RustServer.gg Vote", "NightHawk@Codefling", "1.2.2")]
[Description("Voting reward plugin for RustServers.gg")]
public class RSVote : RustPlugin
{
    private Dictionary<ulong, DateTime> cooldown;
    private void Init();
    private void cmdChat(BasePlayer player, string command, string[] args);
    private string CheckUrl(string steamId);
    private string ClaimUrl(string steamId);
    private string BuildUrl(string action, string userId);
    private void Execute(string url, string action, BasePlayer player);
    private void HandleResponse(string url, int code, string response, string action, BasePlayer player);
    private void GiveRandomReward(BasePlayer player);
    private class ConfigData
    {
        [JsonProperty("API Key")]
        public string apiKey;
        [JsonProperty("Server Id")]
        public string serverId;
        [JsonProperty("Chat Commands")]
        public string[] commands;
        [JsonProperty("Reward items")]
        public RewardItem[] rewardItems;
        [JsonProperty("Reward Commands Help")]
        public string commandHelp;
        [JsonProperty("Reward Commands")]
        public RewardCommand[] rewardCommands;
        [JsonIgnore]
        public const string baseUrl;
    }

    private static ConfigData config;
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class RewardItem : IReward
    {
        [JsonProperty("Item Shortname")]
        public string shortname;
        [JsonProperty("Description")]
        public string description;
        [JsonProperty("Item Amount Min")]
        public int amountMin;
        [JsonProperty("Item Amount Max")]
        public int amountMax;
        [JsonProperty("Item Skin id")]
        public ulong skinId;
        public Item GetItem();
        public bool GiveTo(string userId);
        public string Info();
    }

    private class RewardCommand : IReward
    {
        [JsonProperty("Command")]
        public string command;
        [JsonProperty("Description")]
        public string description;
        [JsonProperty("Type")]
        public string type;
        public bool GiveTo(string userId);
        public string Info();
    }

}

private class ConfigData
{
    [JsonProperty("API Key")]
    public string apiKey;
    [JsonProperty("Server Id")]
    public string serverId;
    [JsonProperty("Chat Commands")]
    public string[] commands;
    [JsonProperty("Reward items")]
    public RewardItem[] rewardItems;
    [JsonProperty("Reward Commands Help")]
    public string commandHelp;
    [JsonProperty("Reward Commands")]
    public RewardCommand[] rewardCommands;
    [JsonIgnore]
    public const string baseUrl;
}

private class RewardItem : IReward
{
    [JsonProperty("Item Shortname")]
    public string shortname;
    [JsonProperty("Description")]
    public string description;
    [JsonProperty("Item Amount Min")]
    public int amountMin;
    [JsonProperty("Item Amount Max")]
    public int amountMax;
    [JsonProperty("Item Skin id")]
    public ulong skinId;
    public Item GetItem();
    public bool GiveTo(string userId);
    public string Info();
}

private class RewardCommand : IReward
{
    [JsonProperty("Command")]
    public string command;
    [JsonProperty("Description")]
    public string description;
    [JsonProperty("Type")]
    public string type;
    public bool GiveTo(string userId);
    public string Info();
}


```

---

## RTEvent

```csharp
using Network;
using Newtonsoft.Json;
using Oxide.Core;
using System;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System.Collections.Generic;
using System.Linq;
using Steamworks.ServerList;
using UnityEngine;

Oxide.Plugins
[Info("RTEvent", "EcoSmile Сделал CUI - Deversive", "1.0.7")]
 class RTEvent : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin NoteUI;
    private string mainui;
    static RTEvent ins;
     PluginConfig config;
    public class PluginConfig
    {
        [JsonProperty("Настройка спавна")]
        public Dictionary<string, RTSetting> zoneSettings;
        [JsonProperty("Радиус маркера")]
        public float MarkerRadius;
        [JsonProperty("Часы дня когд ивент стартует")]
        public string[] StartTimes;
        [JsonProperty("Время до открытия ящика после спвна в минутах")]
        public float OpenTime;
        [JsonProperty("Список предметов выпадаемых из ящика")]
        public List<CustomItem> dropList;
        [JsonProperty("Время через которое ящик будет удален если не залутали до конца (в сек)")]
        public float DeletTime;
        [JsonProperty("Цвет маркера")]
        public string MarkerColor;
        [JsonProperty("Сообщение о начале ивента")]
        public string StartMessage;
        [JsonProperty("Сообщение об разблокировании {rt}")]
        public string UnlockMessage;
        [JsonProperty("Сообщение об открытии ящика {player}, {rt}")]
        public string OpenMessage;
    }

    public class RTSetting
    {
        [JsonProperty("Спавнить ящик на этом РТ?")]
        public bool Enable;
        [JsonProperty("Координаты точки спавна (команда /pos)")]
        public string Offset;
    }

    public class CustomItem
    {
        [JsonProperty("Шортнейм предмета")]
        public string ShortName;
        [JsonProperty("Кастомное имя предмета")]
        public string CustomName;
        [JsonProperty("Количество предмета Min")]
        public int AmountMin;
        [JsonProperty("Количество предмета Max")]
        public int AmountMax;
        [JsonProperty("SkinID предмета")]
        public ulong SkinID;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     Dictionary<string, RTSetting> GetMonuments();
     EventData data;
    public class EventData
    {
        public int EventCount;
    }

     void LoadData();
    private DateTime NextStart;
    private Dictionary<DateTime, TimeSpan> CalcNextRestartDict;
    private List<DateTime> EventTimes;
    private void OnServerInitialized();
    private string mainui1;
    private string close;
    private string GetImage(string fileName, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
     void LoadImage();
    [ConsoleCommand("testgive")]
     void testGiveGameStores();
     void GameStoresGive(string userID);
     void MainUi(BasePlayer player);
    private static string HexToRustFormat(string hex);
     void Unload();
     void OnServerSave();
     void OnNewSave();
     void OnPlayerConnected(BasePlayer player);
     bool started;
     void CheckStart();
    [ConsoleCommand("tst")]
     void tstCmd(ConsoleSystem.Arg arg);
    [ChatCommand("tst")]
     void Start(BasePlayer player);
     void StartEvent();
     string RtName;
     MapMarkerGenericRadius mapMarker;
     StorageContainer crate;
     void SpawnCrate(Vector3 pos);
     Timer destroyCrate;
     Timer downTimer;
     int lockedTime;
     void TimerDown();
     object CanLootEntity(BasePlayer player, StorageContainer container);
    [ChatCommand("testui")]
     void testui(BasePlayer player);
    [ChatCommand("closeui")]
     void closeui(BasePlayer player);
     void OnLootEntityEnd(BasePlayer player, BaseCombatEntity container);
    private void CheckRespawn(StorageContainer container);
     void EventEnd();
     void GetNextRestart(List<DateTime> DateTimes);
    [ChatCommand("poscrate")]
     void CheckPos(BasePlayer player);
    public static UnityEngine.Color hexToColor(string hex);
}

public class PluginConfig
{
    [JsonProperty("Настройка спавна")]
    public Dictionary<string, RTSetting> zoneSettings;
    [JsonProperty("Радиус маркера")]
    public float MarkerRadius;
    [JsonProperty("Часы дня когд ивент стартует")]
    public string[] StartTimes;
    [JsonProperty("Время до открытия ящика после спвна в минутах")]
    public float OpenTime;
    [JsonProperty("Список предметов выпадаемых из ящика")]
    public List<CustomItem> dropList;
    [JsonProperty("Время через которое ящик будет удален если не залутали до конца (в сек)")]
    public float DeletTime;
    [JsonProperty("Цвет маркера")]
    public string MarkerColor;
    [JsonProperty("Сообщение о начале ивента")]
    public string StartMessage;
    [JsonProperty("Сообщение об разблокировании {rt}")]
    public string UnlockMessage;
    [JsonProperty("Сообщение об открытии ящика {player}, {rt}")]
    public string OpenMessage;
}

public class RTSetting
{
    [JsonProperty("Спавнить ящик на этом РТ?")]
    public bool Enable;
    [JsonProperty("Координаты точки спавна (команда /pos)")]
    public string Offset;
}

public class CustomItem
{
    [JsonProperty("Шортнейм предмета")]
    public string ShortName;
    [JsonProperty("Кастомное имя предмета")]
    public string CustomName;
    [JsonProperty("Количество предмета Min")]
    public int AmountMin;
    [JsonProperty("Количество предмета Max")]
    public int AmountMax;
    [JsonProperty("SkinID предмета")]
    public ulong SkinID;
}

public class EventData
{
    public int EventCount;
}


```

---

## RulesGUI

```csharp
using UnityEngine;
using Rust;
using Oxide.Core.Plugins;
using System.Collections.Generic;
using System;
using System.Reflection;
using Oxide.Core;
using System.Linq;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("Rules GUI", "Server-Rust", "1.4.9")]
 class RulesGUI : RustPlugin
{
     List<string> text;
     void Unloaded();
     void UseUI(BasePlayer player, string msg);
     void DisplayUI(BasePlayer player);
     void OnPlayerInit(BasePlayer player);
    [ChatCommand("bind")]
     void bindinfocmd(BasePlayer player, string command);
}


```

---

## RustApp

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Plugins;
using Oxide.Core.Plugins;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Text;
using JetBrains.Annotations;
using Oxide.Core.Libraries;
using Oxide.Game.Rust.Cui;
using System.Linq;
using UnityEngine;
using Network;
using UnityEngine.Networking;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using UnityEngine;
using UnityEngine.Networking;
using System.IO;
using ConVar;
using Oxide.Core.Database;
using Facepunch;
using Rust;
using Steamworks;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Drawing;
using System.Drawing.Imaging;
using System.IO;
using System.Text;
using UnityEngine;
using Color = System.Drawing.Color;
using Graphics = System.Drawing.Graphics;
using Star = ProtoBuf.PatternFirework.Star;

Oxide.Plugins
[Info("RustApp", "Hougan & Xacku & Olkuts", "1.7.0")]
public class RustApp : RustPlugin
{
    public static class EncodingBase64
    {
        public static string Encode(string text);
        public static string Decode(string encodedText);
    }

    public class StableRequest
    {
        private static Dictionary<string, string> DefaultHeaders(string secret);
        private string url;
        private RequestMethod method;
        private object data;
        public Dictionary<string, string> headers;
        public Action<string> onException;
        public Action<T, string> onComplete;
        public bool debug;
        public bool silent;
        private DateTime _created;
        public StableRequest(string url, RequestMethod method, object? data, string secret);
        public double Elapsed();
        public void Execute(Action<T, string> onComplete, Action<string> onException);
        private IEnumerator WaitForRequest(UnityWebRequest request);
    }

    private class LastException
    {
        public static List<LastException> History;
        public string module;
        public string secret;
        public string payload;
        public string response;
        public DateTime time;
        public LastException(string module, object payload, string response, string secret);
    }

    private static class CourtUrls
    {
        private static string Base;
        public static string Pair;
        public static string Validate;
        public static string SendContact;
        public static string SendState;
        public static string SendChat;
        public static string SendAlerts;
        public static string SendCustomAlert;
        public static string SendReports;
        public static string SendDestroyedSignage;
        public static string SendSignage;
        public static string SendWipe;
        public static string BanCreate;
        public static string BanDelete;
    }

    private static class QueueUrls
    {
        private static string Base;
        public static string Fetch;
        public static string Response;
    }

    private static class BanUrls
    {
        private static string Base;
        public static string Fetch;
    }

     class PluginChatMessagesPayload
    {
        public List<PluginChatMessageEntry> messages;
    }

    public class PluginChatMessageEntry
    {
        public string steam_id;
        [CanBeNull]
        public string target_steam_id;
        public bool is_team;
        public string text;
    }

     class PluginReportsPayload
    {
        public List<PluginReportEntry> reports;
    }

    public static class PlayerAlertType
    {
        public static readonly string join_with_ip_ban;
        public static readonly string dug_up_stash;
        public static readonly string custom_api;
    }

    public class PluginPlayerAlertEntry
    {
        public string type;
        public object meta;
    }

    public class PluginPlayerAlertJoinWithIpBanMeta
    {
        public string steam_id;
        public string ip;
        public int ban_id;
    }

    public class PluginPlayerAlertDugUpStashMeta
    {
        public string steam_id;
        public string owner_steam_id;
        public string position;
        public string square;
    }

    public class PluginReportEntry
    {
        public string initiator_steam_id;
        [CanBeNull]
        public string target_steam_id;
        public List<string> sub_targets_steam_ids;
        public string reason;
        [CanBeNull]
        public string message;
    }

     class PluginPairDto
    {
        [CanBeNull]
        public int ttl;
        [CanBeNull]
        public string token;
    }

     class PluginPlayerPayload
    {
        private static bool IsRaidBlocked(BasePlayer player);
        private static bool IsBuildingAuthed(BasePlayer player);
        private static bool NoLicense(Network.Connection connection);
        public static PluginPlayerPayload FromPlayer(BasePlayer player);
        public static PluginPlayerPayload FromConnection(Network.Connection connection, string status);
        public string steam_id;
        public string steam_name;
        public string ip;
        [CanBeNull]
        public string position;
        [CanBeNull]
        public string rotation;
        [CanBeNull]
        public string coords;
        public bool can_build;
        public bool is_raiding;
        public bool no_license;
        public string status;
        public List<string> team;
    }

    public class MetaInfo
    {
        public static MetaInfo Read();
        public static void write(MetaInfo courtMeta);
        [JsonProperty("It is access for RustApp Court, never edit or share it")]
        public string Value;
    }

    public class CheckInfo
    {
        public static CheckInfo Read();
        public static void write(CheckInfo courtMeta);
        [JsonProperty("List of recent checks to show green-check on player")]
        public Dictionary<string, double> LastChecks;
    }

    public class PairWorker : BaseWorker
    {
        public string Code;
        public void EnterCode(string code);
        public void Notify(int left);
        public void Complete(string token);
        public void PairWaitFinish();
        private void @PairAssignData(string code, Action<PluginPairDto> onComplete, Action<string> onException);
    }

    public class CourtWorker : BaseWorker
    {
        public ActionWorker Action;
        public UpdateWorker Update;
        public QueueWorker Queue;
        public PairWorker Pair;
        public BanWorker Ban;
        private MetaInfo Meta;
        public void Awake();
        public void Connect();
        public void EnableModules();
        public void @ValidateSecret(Action? onComplete, Action<string>? onException);
        public void StartPair(string code);
        public void OnDestroy();
    }

    public class ActionWorker : BaseWorker
    {
        public void @SendContact(string steam_id, string message, Action<bool> callback);
        public void @SendWipe(Action<bool> callback);
        public void @SendBan(string steam_id, string reason, string duration, bool global, bool ban_ip);
        public void @SendBanDelete(string steam_id);
        public void @SendCustomAlert(string message, object data);
        public void @SendSignage(BaseImageUpdate upload);
    }

    public class BanWorker : BaseWorker
    {
        public class BanFetchResponse
        {
            public List<BanFetchEntry> entries;
        }

        public class BanFetchPayload
        {
            public string steam_id;
            public string ip;
        }

        public class BanFetchEntry : BanFetchPayload
        {
            public List<BanEntry> bans;
        }

        public class BanEntry
        {
            public int id;
            public string steam_id;
            public string ban_ip;
            public string reason;
            public long expired_at;
            public bool ban_ip_active;
            public bool computed_is_active;
            public int sync_project_id;
            public bool sync_should_kick;
        }

        private Dictionary<string, string> PlayersCollection;
        public void Awake();
        protected override void OnReady();
        public void FetchBan(BasePlayer player);
        public void FetchBan(string steamId, string ip);
        private void FetchBans();
        public void CreateAlertForIpBan(BanEntry ban, string steamId);
        public void @FetchBans(Dictionary<string, string> entries, Action<string, BanEntry> onBan, Action onException);
    }

    public class UpdateWorker : BaseWorker
    {
        private List<PluginPlayerAlertEntry> PlayerAlertCollection;
        private List<PluginReportEntry> ReportCollection;
        private List<PluginChatMessageEntry> ChatCollection;
        private Dictionary<string, string> DisconnectHistory;
        private Dictionary<string, string> TeamChangeHistory;
        private List<string> DestroyedSignsCollection;
        protected override void OnReady();
        public void SaveChat(PluginChatMessageEntry message);
        public void SaveAlert(PluginPlayerAlertEntry playerAlert);
        public void SaveDestroyedSign(string net_id);
        public void SaveDisconnect(BasePlayer player, string reason);
        public void SaveTeamHistory(string initiator_steam_id, string target_steam_id);
        public void SaveReport(PluginReportEntry report);
        private void SendDestroyedSigns();
        private void SendAlerts();
        private void SendChat();
        public void SendReports();
        public void SendUpdate();
    }

    public class QueueWorker : BaseWorker
    {
        private List<string> ProcessedQueues;
        public Dictionary<string, bool> Notices;
        public class QueueElement
        {
            public string id;
            public QueueRequest request;
        }

        public class QueueRequest
        {
            public string name;
            public JObject data;
        }

        private class QueueKickPayload
        {
            public string steam_id;
            public string reason;
            public bool announce;
        }

        private class QueueBanPayload
        {
            public string steam_id;
            public string name;
            public string reason;
        }

        private class QueueDebugLog
        {
            public string message;
        }

        private class QueuePaidAnnounceBan
        {
            public bool broadcast;
            public string suspect_name;
            public string suspect_id;
            public string reason;
            public List<string> targets;
        }

        private class QueuePaidAnnounceClean
        {
            public bool broadcast;
            public string suspect_name;
            public string suspect_id;
            public List<string> targets;
        }

         class QueueNoticeStateGetPayload
        {
            public string steam_id;
        }

        private class QueueNoticeStateSetPayload
        {
            public string steam_id;
            public bool value;
        }

        private class QueueChatMessage
        {
            public string initiator_name;
            public string initiator_steam_id;
            [CanBeNull]
            public string target_steam_id;
            public string message;
            public string mode;
        }

        private class QueueExecuteCommand
        {
            public List<string> commands;
        }

        private class QueueDeleteEntityPayload
        {
            public string net_id;
        }

        protected override void OnReady();
        private void QueueRetreive();
        private void @QueueRetreive(Action<List<QueueElement>> callback);
        public void QueueProcess(Dictionary<string, object> responses);
        private object OnQueueHealthCheck();
        public object QueueProcess(QueueElement element);
        private object OnDeleteEntity(QueueDeleteEntityPayload payload);
        private object OnNoticeStateGet(QueueNoticeStateGetPayload payload);
        private object OnNoticeStateSet(QueueNoticeStateSetPayload payload);
        private object OnQueueKick(QueueKickPayload payload);
        private object OnQueueBan(QueueBanPayload payload);
        private object OnQueueDebugLog(QueueDebugLog payload);
        private object OnQueuePaidAnnounceBan(QueuePaidAnnounceBan payload);
        private object OnQueuePaidAnnounceClean(QueuePaidAnnounceClean payload);
        private object OnExecuteCommand(QueueExecuteCommand payload);
        private object OnChatMessage(QueueChatMessage payload);
        private object NoticeStateSet(BasePlayer player, bool value);
    }

    public class BaseWorker : MonoBehaviour
    {
        protected string secret;
        public void Auth(string secret);
        protected StableRequest<T> Request(string url, RequestMethod method, object? data);
        public bool IsReady();
        public bool IsAuthed();
        protected virtual void OnReady();
    }

    private class Configuration
    {
        [JsonProperty("[UI] Chat commands")]
        public List<string> report_ui_commands;
        [JsonProperty("[UI] Report reasons")]
        public List<string> report_ui_reasons;
        [JsonProperty("[UI] Cooldown between reports (seconds)")]
        public int report_ui_cooldown;
        [JsonProperty("[UI] Auto-parse reports from F7 (ingame reports)")]
        public bool report_ui_auto_parse;
        [JsonProperty("[UI • Starter Plan] Show 'recently checked' checkbox (amount of days)")]
        public int report_ui_show_check_in;
        [JsonProperty("[Chat] SteamID for message avatar (default account contains RustApp logo)")]
        public string chat_default_avatar_steamid;
        [JsonProperty("[Chat] Global message format")]
        public string chat_global_format;
        [JsonProperty("[Chat] Direct message format")]
        public string chat_direct_format;
        [JsonProperty("[Check] Command to send contact")]
        public string check_contact_command;
        [JsonProperty("[Ban] Enable broadcast server bans")]
        public bool ban_enable_broadcast;
        [JsonProperty("[Ban] Ban broadcast format")]
        public string ban_broadcast_format;
        [JsonProperty("[Ban] Kick message format (%REASON% - ban reason)")]
        public string ban_reason_format;
        [JsonProperty("[Ban] Kick message format temporary (%REASON% - ban reason)")]
        public string ban_reason_format_temporary;
        [JsonProperty("[Ban] Message format when kicking due to IP")]
        public string ban_reason_ip_format;
        [JsonProperty("[Ban-Sync] Kick message format (%REASON% - ban reason)")]
        public string ban_sync_reason_format;
        [JsonProperty("[Ban-Sync] Kick message format temporary (%REASON% - ban reason)")]
        public string ban_sync_reason_format_temporary;
        [JsonProperty("[Custom Actions] Allow custom actions")]
        public bool custom_actions_allow;
        public static Configuration Generate();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private static string ReportLayer;
    private void DrawReportInterface(BasePlayer player, int page, string search, bool redraw);
    private const string CheckLayer;
    private void DrawInterface(BasePlayer player);
    private static string HexToRustFormat(string hex);
    private CourtWorker _Worker;
    private Dictionary<ulong, double> _Cooldowns;
    private static RustApp _RustApp;
    private static Configuration _Settings;
    private static CheckInfo _Checks;
    [PluginReference]
    private Plugin NoEscape;
    private Plugin RaidZone;
    private Plugin RaidBlock;
    private Plugin MultiFighting;
    private void OnServerInitialized();
    private void RegisterMessages();
    private void Unload();
    private void RA_DirectMessageHandler(string from, string to, string message);
    private void RA_ReportSend(string initiator_steam_id, string target_steam_id, string reason, string message);
    private void RA_CustomAlert(string message, object data);
    private void OnPlayerReported(BasePlayer reporter, string targetName, string targetId, string subject, string message, string type);
    private void OnStashExposed(StashContainer stash, BasePlayer player);
    private void CanUserLogin(string name, string id, string ipAddress);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnTeamKick(RelationshipManager.PlayerTeam team, BasePlayer player, ulong target);
    private void OnTeamDisband(RelationshipManager.PlayerTeam team);
    private void OnTeamLeave(RelationshipManager.PlayerTeam team, BasePlayer player);
    private void OnPlayerChat(BasePlayer player, string message, ConVar.Chat.ChatChannel channel);
    private void OnEntityKill(BaseNetworkable entity);
    private void OnNewSave(string saveName);
    private void CmdChatContact(BasePlayer player, string command, string[] args);
    [ConsoleCommand("UI_RP_ReportPanel")]
    private void CmdConsoleReportPanel(ConsoleSystem.Arg args);
    private void ChatCmdReport(BasePlayer player);
    [ConsoleCommand("ra.help")]
    private void CmdConsoleHelp(ConsoleSystem.Arg args);
    [ConsoleCommand("ra.debug")]
    private void CmdConsoleStatus(ConsoleSystem.Arg args);
    [ConsoleCommand("ra.ban_server")]
    private void CmdConsoleBanServerDeprecated(ConsoleSystem.Arg args);
    [ConsoleCommand("ra.ban_global")]
    private void CmdConsoleBanGlobalDeprecated(ConsoleSystem.Arg args);
    [ConsoleCommand("ra.ban")]
    private void CmdConsoleBan(ConsoleSystem.Arg args);
    [ConsoleCommand("ra.unban")]
    private void CmdConsoleBanDelete(ConsoleSystem.Arg args);
    [ConsoleCommand("ra.pair")]
    private void CmdConsoleCourtSetup(ConsoleSystem.Arg args);
    private void SendGlobalMessage(string message);
    private bool SendMessage(string steamId, string message);
    private void SendMessage(BasePlayer player, string message, string initiator_steam_id);
    private void SoundInfoToast(BasePlayer player, string text);
    private void SoundErrorToast(BasePlayer player, string text);
    private void SoundToast(BasePlayer player, string text, int type);
    private bool CloseConnection(string steamId, string reason);
    private double CurrentTime();
    private static List<char> Letters;
    private static string NormalizeString(string text);
    private static string GridReference(Vector3 position);
    private long getUnixTime();
    private Network.Connection? getPlayerConnection(string steamId);
    public string Msg(string ru, string en);
    public void Log(string ru, string en);
    public void Warning(string ru, string en);
    public void Error(string ru, string en);
    public abstract class BaseImageUpdate
    {
        public BasePlayer Player { get; set; }
        public ulong PlayerId { get; set; }
        public string DisplayName { get; set; }
        public BaseEntity Entity { get; set; }
        public int ItemId { get; set; }
        public uint TextureIndex { get; set; }
        public abstract bool SupportsTextureIndex { get; set; }
        protected BaseImageUpdate(BasePlayer player, BaseEntity entity);
        public abstract byte[] GetImage();
    }

    public class FireworkUpdate : BaseImageUpdate
    {
        static readonly Hash<UnityEngine.Color, Brush> FireworkBrushes;
        public override bool SupportsTextureIndex { get; set; }
        public PatternFirework Firework { get; set; }
        public FireworkUpdate(BasePlayer player, PatternFirework entity);
        public override byte[] GetImage();
        private Brush GetBrush(UnityEngine.Color color);
        private Color FromUnityColor(UnityEngine.Color color);
        private int FromUnityColorField(float color);
        private byte[] GetImageBytes(Bitmap image);
    }

    public class PaintedItemUpdate : BaseImageUpdate
    {
        private readonly byte[] _image;
        public PaintedItemUpdate(BasePlayer player, PaintedItemStorageEntity entity, Item item, byte[] image);
        public override bool SupportsTextureIndex { get; set; }
        public override byte[] GetImage();
    }

    public class SignageUpdate : BaseImageUpdate
    {
        public string Url { get; set; }
        public override bool SupportsTextureIndex { get; set; }
        public ISignage Signage { get; set; }
        public SignageUpdate(BasePlayer player, ISignage entity, uint textureIndex, string url);
        public override byte[] GetImage();
    }

    private void OnImagePost(BasePlayer player, string url, bool raw, ISignage signage, uint textureIndex);
    private void OnSignUpdated(ISignage signage, BasePlayer player, int textureIndex);
    private void OnItemPainted(PaintedItemStorageEntity entity, Item item, BasePlayer player, byte[] image);
    private void OnFireworkDesignChanged(PatternFirework firework, ProtoBuf.PatternFirework.Design design, BasePlayer player);
    private object CanUpdateSign(BasePlayer player, BaseEntity entity);
    private object OnFireworkDesignChange(PatternFirework firework, ProtoBuf.PatternFirework.Design design, BasePlayer player);
    private void OnEntityBuilt(Planner plan, GameObject go);
}

public static class EncodingBase64
{
    public static string Encode(string text);
    public static string Decode(string encodedText);
}

public class StableRequest
{
    private static Dictionary<string, string> DefaultHeaders(string secret);
    private string url;
    private RequestMethod method;
    private object data;
    public Dictionary<string, string> headers;
    public Action<string> onException;
    public Action<T, string> onComplete;
    public bool debug;
    public bool silent;
    private DateTime _created;
    public StableRequest(string url, RequestMethod method, object? data, string secret);
    public double Elapsed();
    public void Execute(Action<T, string> onComplete, Action<string> onException);
    private IEnumerator WaitForRequest(UnityWebRequest request);
}

private class LastException
{
    public static List<LastException> History;
    public string module;
    public string secret;
    public string payload;
    public string response;
    public DateTime time;
    public LastException(string module, object payload, string response, string secret);
}

private static class CourtUrls
{
    private static string Base;
    public static string Pair;
    public static string Validate;
    public static string SendContact;
    public static string SendState;
    public static string SendChat;
    public static string SendAlerts;
    public static string SendCustomAlert;
    public static string SendReports;
    public static string SendDestroyedSignage;
    public static string SendSignage;
    public static string SendWipe;
    public static string BanCreate;
    public static string BanDelete;
}

private static class QueueUrls
{
    private static string Base;
    public static string Fetch;
    public static string Response;
}

private static class BanUrls
{
    private static string Base;
    public static string Fetch;
}

 class PluginChatMessagesPayload
{
    public List<PluginChatMessageEntry> messages;
}

public class PluginChatMessageEntry
{
    public string steam_id;
    [CanBeNull]
    public string target_steam_id;
    public bool is_team;
    public string text;
}

 class PluginReportsPayload
{
    public List<PluginReportEntry> reports;
}

public static class PlayerAlertType
{
    public static readonly string join_with_ip_ban;
    public static readonly string dug_up_stash;
    public static readonly string custom_api;
}

public class PluginPlayerAlertEntry
{
    public string type;
    public object meta;
}

public class PluginPlayerAlertJoinWithIpBanMeta
{
    public string steam_id;
    public string ip;
    public int ban_id;
}

public class PluginPlayerAlertDugUpStashMeta
{
    public string steam_id;
    public string owner_steam_id;
    public string position;
    public string square;
}

public class PluginReportEntry
{
    public string initiator_steam_id;
    [CanBeNull]
    public string target_steam_id;
    public List<string> sub_targets_steam_ids;
    public string reason;
    [CanBeNull]
    public string message;
}

 class PluginPairDto
{
    [CanBeNull]
    public int ttl;
    [CanBeNull]
    public string token;
}

 class PluginPlayerPayload
{
    private static bool IsRaidBlocked(BasePlayer player);
    private static bool IsBuildingAuthed(BasePlayer player);
    private static bool NoLicense(Network.Connection connection);
    public static PluginPlayerPayload FromPlayer(BasePlayer player);
    public static PluginPlayerPayload FromConnection(Network.Connection connection, string status);
    public string steam_id;
    public string steam_name;
    public string ip;
    [CanBeNull]
    public string position;
    [CanBeNull]
    public string rotation;
    [CanBeNull]
    public string coords;
    public bool can_build;
    public bool is_raiding;
    public bool no_license;
    public string status;
    public List<string> team;
}

public class MetaInfo
{
    public static MetaInfo Read();
    public static void write(MetaInfo courtMeta);
    [JsonProperty("It is access for RustApp Court, never edit or share it")]
    public string Value;
}

public class CheckInfo
{
    public static CheckInfo Read();
    public static void write(CheckInfo courtMeta);
    [JsonProperty("List of recent checks to show green-check on player")]
    public Dictionary<string, double> LastChecks;
}

public class PairWorker : BaseWorker
{
    public string Code;
    public void EnterCode(string code);
    public void Notify(int left);
    public void Complete(string token);
    public void PairWaitFinish();
    private void @PairAssignData(string code, Action<PluginPairDto> onComplete, Action<string> onException);
}

public class CourtWorker : BaseWorker
{
    public ActionWorker Action;
    public UpdateWorker Update;
    public QueueWorker Queue;
    public PairWorker Pair;
    public BanWorker Ban;
    private MetaInfo Meta;
    public void Awake();
    public void Connect();
    public void EnableModules();
    public void @ValidateSecret(Action? onComplete, Action<string>? onException);
    public void StartPair(string code);
    public void OnDestroy();
}

public class ActionWorker : BaseWorker
{
    public void @SendContact(string steam_id, string message, Action<bool> callback);
    public void @SendWipe(Action<bool> callback);
    public void @SendBan(string steam_id, string reason, string duration, bool global, bool ban_ip);
    public void @SendBanDelete(string steam_id);
    public void @SendCustomAlert(string message, object data);
    public void @SendSignage(BaseImageUpdate upload);
}

public class BanWorker : BaseWorker
{
    public class BanFetchResponse
    {
        public List<BanFetchEntry> entries;
    }

    public class BanFetchPayload
    {
        public string steam_id;
        public string ip;
    }

    public class BanFetchEntry : BanFetchPayload
    {
        public List<BanEntry> bans;
    }

    public class BanEntry
    {
        public int id;
        public string steam_id;
        public string ban_ip;
        public string reason;
        public long expired_at;
        public bool ban_ip_active;
        public bool computed_is_active;
        public int sync_project_id;
        public bool sync_should_kick;
    }

    private Dictionary<string, string> PlayersCollection;
    public void Awake();
    protected override void OnReady();
    public void FetchBan(BasePlayer player);
    public void FetchBan(string steamId, string ip);
    private void FetchBans();
    public void CreateAlertForIpBan(BanEntry ban, string steamId);
    public void @FetchBans(Dictionary<string, string> entries, Action<string, BanEntry> onBan, Action onException);
}

public class BanFetchResponse
{
    public List<BanFetchEntry> entries;
}

public class BanFetchPayload
{
    public string steam_id;
    public string ip;
}

public class BanFetchEntry : BanFetchPayload
{
    public List<BanEntry> bans;
}

public class BanEntry
{
    public int id;
    public string steam_id;
    public string ban_ip;
    public string reason;
    public long expired_at;
    public bool ban_ip_active;
    public bool computed_is_active;
    public int sync_project_id;
    public bool sync_should_kick;
}

public class UpdateWorker : BaseWorker
{
    private List<PluginPlayerAlertEntry> PlayerAlertCollection;
    private List<PluginReportEntry> ReportCollection;
    private List<PluginChatMessageEntry> ChatCollection;
    private Dictionary<string, string> DisconnectHistory;
    private Dictionary<string, string> TeamChangeHistory;
    private List<string> DestroyedSignsCollection;
    protected override void OnReady();
    public void SaveChat(PluginChatMessageEntry message);
    public void SaveAlert(PluginPlayerAlertEntry playerAlert);
    public void SaveDestroyedSign(string net_id);
    public void SaveDisconnect(BasePlayer player, string reason);
    public void SaveTeamHistory(string initiator_steam_id, string target_steam_id);
    public void SaveReport(PluginReportEntry report);
    private void SendDestroyedSigns();
    private void SendAlerts();
    private void SendChat();
    public void SendReports();
    public void SendUpdate();
}

public class QueueWorker : BaseWorker
{
    private List<string> ProcessedQueues;
    public Dictionary<string, bool> Notices;
    public class QueueElement
    {
        public string id;
        public QueueRequest request;
    }

    public class QueueRequest
    {
        public string name;
        public JObject data;
    }

    private class QueueKickPayload
    {
        public string steam_id;
        public string reason;
        public bool announce;
    }

    private class QueueBanPayload
    {
        public string steam_id;
        public string name;
        public string reason;
    }

    private class QueueDebugLog
    {
        public string message;
    }

    private class QueuePaidAnnounceBan
    {
        public bool broadcast;
        public string suspect_name;
        public string suspect_id;
        public string reason;
        public List<string> targets;
    }

    private class QueuePaidAnnounceClean
    {
        public bool broadcast;
        public string suspect_name;
        public string suspect_id;
        public List<string> targets;
    }

     class QueueNoticeStateGetPayload
    {
        public string steam_id;
    }

    private class QueueNoticeStateSetPayload
    {
        public string steam_id;
        public bool value;
    }

    private class QueueChatMessage
    {
        public string initiator_name;
        public string initiator_steam_id;
        [CanBeNull]
        public string target_steam_id;
        public string message;
        public string mode;
    }

    private class QueueExecuteCommand
    {
        public List<string> commands;
    }

    private class QueueDeleteEntityPayload
    {
        public string net_id;
    }

    protected override void OnReady();
    private void QueueRetreive();
    private void @QueueRetreive(Action<List<QueueElement>> callback);
    public void QueueProcess(Dictionary<string, object> responses);
    private object OnQueueHealthCheck();
    public object QueueProcess(QueueElement element);
    private object OnDeleteEntity(QueueDeleteEntityPayload payload);
    private object OnNoticeStateGet(QueueNoticeStateGetPayload payload);
    private object OnNoticeStateSet(QueueNoticeStateSetPayload payload);
    private object OnQueueKick(QueueKickPayload payload);
    private object OnQueueBan(QueueBanPayload payload);
    private object OnQueueDebugLog(QueueDebugLog payload);
    private object OnQueuePaidAnnounceBan(QueuePaidAnnounceBan payload);
    private object OnQueuePaidAnnounceClean(QueuePaidAnnounceClean payload);
    private object OnExecuteCommand(QueueExecuteCommand payload);
    private object OnChatMessage(QueueChatMessage payload);
    private object NoticeStateSet(BasePlayer player, bool value);
}

public class QueueElement
{
    public string id;
    public QueueRequest request;
}

public class QueueRequest
{
    public string name;
    public JObject data;
}

private class QueueKickPayload
{
    public string steam_id;
    public string reason;
    public bool announce;
}

private class QueueBanPayload
{
    public string steam_id;
    public string name;
    public string reason;
}

private class QueueDebugLog
{
    public string message;
}

private class QueuePaidAnnounceBan
{
    public bool broadcast;
    public string suspect_name;
    public string suspect_id;
    public string reason;
    public List<string> targets;
}

private class QueuePaidAnnounceClean
{
    public bool broadcast;
    public string suspect_name;
    public string suspect_id;
    public List<string> targets;
}

 class QueueNoticeStateGetPayload
{
    public string steam_id;
}

private class QueueNoticeStateSetPayload
{
    public string steam_id;
    public bool value;
}

private class QueueChatMessage
{
    public string initiator_name;
    public string initiator_steam_id;
    [CanBeNull]
    public string target_steam_id;
    public string message;
    public string mode;
}

private class QueueExecuteCommand
{
    public List<string> commands;
}

private class QueueDeleteEntityPayload
{
    public string net_id;
}

public class BaseWorker : MonoBehaviour
{
    protected string secret;
    public void Auth(string secret);
    protected StableRequest<T> Request(string url, RequestMethod method, object? data);
    public bool IsReady();
    public bool IsAuthed();
    protected virtual void OnReady();
}

private class Configuration
{
    [JsonProperty("[UI] Chat commands")]
    public List<string> report_ui_commands;
    [JsonProperty("[UI] Report reasons")]
    public List<string> report_ui_reasons;
    [JsonProperty("[UI] Cooldown between reports (seconds)")]
    public int report_ui_cooldown;
    [JsonProperty("[UI] Auto-parse reports from F7 (ingame reports)")]
    public bool report_ui_auto_parse;
    [JsonProperty("[UI • Starter Plan] Show 'recently checked' checkbox (amount of days)")]
    public int report_ui_show_check_in;
    [JsonProperty("[Chat] SteamID for message avatar (default account contains RustApp logo)")]
    public string chat_default_avatar_steamid;
    [JsonProperty("[Chat] Global message format")]
    public string chat_global_format;
    [JsonProperty("[Chat] Direct message format")]
    public string chat_direct_format;
    [JsonProperty("[Check] Command to send contact")]
    public string check_contact_command;
    [JsonProperty("[Ban] Enable broadcast server bans")]
    public bool ban_enable_broadcast;
    [JsonProperty("[Ban] Ban broadcast format")]
    public string ban_broadcast_format;
    [JsonProperty("[Ban] Kick message format (%REASON% - ban reason)")]
    public string ban_reason_format;
    [JsonProperty("[Ban] Kick message format temporary (%REASON% - ban reason)")]
    public string ban_reason_format_temporary;
    [JsonProperty("[Ban] Message format when kicking due to IP")]
    public string ban_reason_ip_format;
    [JsonProperty("[Ban-Sync] Kick message format (%REASON% - ban reason)")]
    public string ban_sync_reason_format;
    [JsonProperty("[Ban-Sync] Kick message format temporary (%REASON% - ban reason)")]
    public string ban_sync_reason_format_temporary;
    [JsonProperty("[Custom Actions] Allow custom actions")]
    public bool custom_actions_allow;
    public static Configuration Generate();
}

public abstract class BaseImageUpdate
{
    public BasePlayer Player { get; set; }
    public ulong PlayerId { get; set; }
    public string DisplayName { get; set; }
    public BaseEntity Entity { get; set; }
    public int ItemId { get; set; }
    public uint TextureIndex { get; set; }
    public abstract bool SupportsTextureIndex { get; set; }
    protected BaseImageUpdate(BasePlayer player, BaseEntity entity);
    public abstract byte[] GetImage();
}

public class FireworkUpdate : BaseImageUpdate
{
    static readonly Hash<UnityEngine.Color, Brush> FireworkBrushes;
    public override bool SupportsTextureIndex { get; set; }
    public PatternFirework Firework { get; set; }
    public FireworkUpdate(BasePlayer player, PatternFirework entity);
    public override byte[] GetImage();
    private Brush GetBrush(UnityEngine.Color color);
    private Color FromUnityColor(UnityEngine.Color color);
    private int FromUnityColorField(float color);
    private byte[] GetImageBytes(Bitmap image);
}

public class PaintedItemUpdate : BaseImageUpdate
{
    private readonly byte[] _image;
    public PaintedItemUpdate(BasePlayer player, PaintedItemStorageEntity entity, Item item, byte[] image);
    public override bool SupportsTextureIndex { get; set; }
    public override byte[] GetImage();
}

public class SignageUpdate : BaseImageUpdate
{
    public string Url { get; set; }
    public override bool SupportsTextureIndex { get; set; }
    public ISignage Signage { get; set; }
    public SignageUpdate(BasePlayer player, ISignage entity, uint textureIndex, string url);
    public override byte[] GetImage();
}


```

---

## RustBugFixes

```csharp
using UnityEngine;

Oxide.Plugins
[Info("Rust Bug Fixes", "Mughisi", 1.0)]
 class RustBugFixes : RustPlugin
{
     void OnServerInitialized();
     void OnEntityBuilt(Planner planner, GameObject component);
}


```

---

## RustKits

```csharp
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("RustKits", "VooDoo", "1.0.0")]
[Description("Menu for RUST")]
public class RustKits : RustPlugin
{
    private static RustKits Instance;
    [PluginReference]
     Plugin XMenu;
    [PluginReference]
     Plugin ImageLibrary;
    [PluginReference]
     Plugin Notifications;
    private bool AddImage(string url, string imageName, ulong imageId, Action callback);
    private string GetImage(string imageName, ulong imageId, bool returnUrl);
    private List<Kit> _kits;
    private Dictionary<ulong, Dictionary<string, KitData>> _kitsData;
    public class KitData
    {
        public int Amount { get; set; }
        public double Cooldown { get; set; }
    }

    public class Kit
    {
        public string Name { get; set; }
        public string Image { get; set; }
        public string Permission { get; set; }
        public int Amount { get; set; }
        public double Cooldown { get; set; }
        public List<KitItem> Items { get; set; }
    }

    public class KitItem
    {
        public string Name { get; set; }
        public string Container { get; set; }
        public int Amount { get; set; }
        public ulong SkinID { get; set; }
        public float Condition { get; set; }
        public Weapon Weapon { get; set; }
    }

    public class Weapon
    {
        public string AmmoName { get; set; }
        public int Amount;
        public List<string> Content { get; set; }
    }

    private List<Kit> GetKitsForPlayer(BasePlayer player);
    private KitData GetKitData(BasePlayer player, string kitName);
    private List<KitItem> GetPlayerItems(BasePlayer player);
    private KitItem ItemToKit(Item item, string container);
    private void OnPlayerRespawned(BasePlayer player);
    private void SaveKits();
    private void SaveData();
    protected override void LoadDefaultMessages();
    private Timer initTimer;
    private void OnServerInitialized();
    private void Unload();
    [ConsoleCommand("kitadd")]
    private void AddKitCmd(ConsoleSystem.Arg arg);
    [ConsoleCommand("kit")]
    private void GiveKitCmd(ConsoleSystem.Arg args);
    public const string MenuLayer;
    public const string MenuItemsLayer;
    public const string MenuSubItemsLayer;
    public const string MenuContent;
    private void RenderKits(ulong userID, object[] objects);
    private void RenderKit(BasePlayer player, CuiElementContainer Container, List<Kit> Kits, int number, int positionX, int positionY);
    [ConsoleCommand("kitshow")]
     void CaseShowCmd(ConsoleSystem.Arg arg);
    private void RenderKitInfo(BasePlayer player, Kit kit);
    private void GiveKit(BasePlayer player, Kit kit);
    private void GiveItem(PlayerInventory inv, Item item, ItemContainer cont);
    private Item BuildItem(string ShortName, int Amount, ulong SkinID, float Condition, Weapon weapon);
    private double GetCurrentTime();
    private static string HexToRustFormat(string hex);
    private static class TimeExtensions
    {
        public static string FormatShortTime(TimeSpan time);
        public static string FormatTime(TimeSpan time);
        private static string Format(int units, string form1, string form2, string form3);
    }

}

public class KitData
{
    public int Amount { get; set; }
    public double Cooldown { get; set; }
}

public class Kit
{
    public string Name { get; set; }
    public string Image { get; set; }
    public string Permission { get; set; }
    public int Amount { get; set; }
    public double Cooldown { get; set; }
    public List<KitItem> Items { get; set; }
}

public class KitItem
{
    public string Name { get; set; }
    public string Container { get; set; }
    public int Amount { get; set; }
    public ulong SkinID { get; set; }
    public float Condition { get; set; }
    public Weapon Weapon { get; set; }
}

public class Weapon
{
    public string AmmoName { get; set; }
    public int Amount;
    public List<string> Content { get; set; }
}

private static class TimeExtensions
{
    public static string FormatShortTime(TimeSpan time);
    public static string FormatTime(TimeSpan time);
    private static string Format(int units, string form1, string form2, string form3);
}


```

---

## RustLax

```csharp
using System.Linq;
using UnityEngine;
using Newtonsoft.Json;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Rust Lax", "Colon Blow", "1.0.3")]
[Description("Control when Horses drop dung in minutes")]
public class RustLax : CovalencePlugin
{
    private const string permAdmin;
    private const string permMounted;
    private void Init();
    private void OnServerInitialized();
    private PluginConfig config;
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Dung Time - Number of Minutes between dung drops : ")]
        public float dungTimeGlobal { get; set; }
        [JsonProperty(PropertyName = "Dung Time Mounted - Mounted players with rustlax.mounted perms, will change dung drop time to (Minutes) : ")]
        public float dungTimeMounted { get; set; }
    }

    private PluginConfig GetDefaultConfig();
    protected override void LoadDefaultConfig();
    private void SaveConfig();
    protected override void LoadDefaultMessages();
    [Command("rustlax.reset")]
    private void cmdRustLaxReset(IPlayer player, string command, string[] args);
    private void OnEntitySpawned(BaseRidableAnimal animal);
    private void OnEntityMounted(BaseMountable entity, BasePlayer player);
    private void OnEntityDismounted(BaseMountable entity, BasePlayer player);
    private void ProcessExistingAnimals(float dungAdjustment);
    private void ProcessAnimal(BaseRidableAnimal animal, float dungAdjustment);
}

private class PluginConfig
{
    [JsonProperty(PropertyName = "Dung Time - Number of Minutes between dung drops : ")]
    public float dungTimeGlobal { get; set; }
    [JsonProperty(PropertyName = "Dung Time Mounted - Mounted players with rustlax.mounted perms, will change dung drop time to (Minutes) : ")]
    public float dungTimeMounted { get; set; }
}


```

---

## RustMap

```csharp
using Network;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Drawing;
using System.Drawing.Imaging;
using System.IO;
using System.Linq;
using System.Text;
using UnityEngine;

Oxide.Plugins
[Info("RustMap", "RustPlugin.ru", "1.3.5")]
 class RustMap : RustPlugin
{
    [PluginReference]
     Plugin Clans;
    [PluginReference]
     Plugin Friends;
    [PluginReference]
     Plugin MutualPermission;
    [PluginReference]
     Plugin NTeleportation;
    [PluginReference]
     Plugin Teleport;
    [PluginReference]
     Plugin Teleportation;
    [PluginReference]
     Plugin CustomQuarry;
    [PluginReference]
     Plugin HomesGUI;
    [PluginReference]
     Plugin PlayersClasses;
     class MapMarker
    {
        public Transform transform;
        public int Rotation;
        public Vector2 anchorPosition { get; set; }
        public Vector2 position;
        public string name;
        public string png;
        public bool rotSupport;
        public string text;
        public float size;
        public float alpha;
        public int fontsize;
        public ulong counter;
        public bool inMap;
        protected MapMarker(Transform transform);
        public virtual bool NeedRedraw();
        public static MapMarker Create(Transform transform);
    }

     class MapPlayer : MapMarker
    {
        public BasePlayer player;
        public List <MapPlayer> clanTeam;
        protected MapPlayer(BasePlayer player);
        public override bool NeedRedraw();
        public void OnCloseMap();
        public static MapPlayer Create(BasePlayer player);
    }

     string beancanKey;
     float mapAlpha;
     float mapSize;
     bool clanSupport;
     bool friendsSupport;
     float playerIconSize;
     float playerIconAlpha;
     bool playerCoordinates;
     bool monuments;
     bool caves;
     bool powersub;
     float monumentIconSize;
     float monumentIconAlpha;
     bool monumentIconNames;
     float MapUpdate;
     bool raidhomes;
     float raidhomeSIze;
     int monumentsFontSize;
     bool bradley;
     float bradleyIconSize;
     float bradleyIconAlpha;
     bool car;
     bool plane;
     float planeIconSize;
     float planeIconAlpha;
     float TimeToClose;
     bool NewHeli;
     float NewheliIconSize;
     float NewheliIconAlpha;
     bool NewHeliCrate;
     float NewHeliCrateIconSize;
     float NewHeliCrateIconAlpha;
     bool ship;
     float shipIconSize;
     float shipIconAlpha;
     bool ballon;
     float ballonIconSize;
     float ballonIconAlpha;
     bool planeDrop;
     bool sethome;
     float sethomeIconSize;
     float deathIconSize;
     float planeDropIconSize;
     float planeDropIconAlpha;
     bool heli;
     float heliIconSize;
     float heliIconAlpha;
     bool heliDrop;
     bool quarry;
     float quarryIconAlpha;
     float quarryIconSize;
     float heliDropIconSize;
     float heliDropIconAlpha;
     bool vendingMachine;
     bool vendingMachineEmpty;
     float vendingMachineIconSize;
     float vendingMachineIconAlpha;
     string textColor;
     string textColor1;
     float bannedSize;
     string mapUrl;
     string MapText;
    private string version;
     bool NPCVendingMachine;
    protected override void LoadDefaultConfig();
     T GetConfig(string name, T defaultValue);
    static RustMap m_Instance;
    private string mapIconJson;
    private string mapIconTextJson;
    private string mapIconTextJsonIcon;
    private string mapJson;
    private string mapCoordsTextJson;
    private string mapHelpJson;
     Dictionary<BasePlayer, MapPlayer> mapPlayers;
     Dictionary<BasePlayer, MapPlayer> subscribers;
     List<MapMarker> temporaryMarkers;
    const string MARKER_BANNED_PERM;
    const string MAP_ADMIN;
    const string MAP_DEATH;
    static float mapSize1;
    static string mapSeed;
    static int worldSize;
    static string level;
     string monumentsJson;
     bool init;
    private List<BasePlayer> AllPlayerUsers;
    private Timer CloseMapTimer;
    [ChatCommand("map")]
     void cmdMapControl(BasePlayer player, string command, string[] args);
     string button;
    private string MapHelp;
     void HelpUI(BasePlayer player);
    [ConsoleCommand("cmd.help")]
    private void CmdHelpControll(ConsoleSystem.Arg arg);
    [ConsoleCommand("map.wipe")]
    private void CmdTest(ConsoleSystem.Arg arg);
    [ConsoleCommand("map.open")]
     void ConsoleMap(ConsoleSystem.Arg arg);
    private Timer mtimer;
     void OnServerInitialized();
     void OnPlayerInit(BasePlayer player);
     void OnNewSave();
     void Unload();
     void OnEntitySpawned(BaseEntity entity);
     void OnEntityKill(BaseNetworkable entity);
     void LoadData();
    private void PlayerSaveData();
     class DataStorage
    {
        public Dictionary<ulong, MAPDATA> MapPlayerData;
        public DataStorage();
    }

     class MAPDATA
    {
        public string Name;
        public bool Homes;
        public bool Friends;
        public bool Clans;
        public bool Death;
        public bool AllPlayers;
        public bool BanPlayers;
    }

     DataStorage data;
    private DynamicConfigFile MapPlayerData;
     void OpenMap(BasePlayer player);
    private Dictionary<ulong, string> playerDic;
    private object OnPlayerDie(BasePlayer player, HitInfo info);
    [PluginReference]
     Plugin NoEscape;
     List<Vector3> GetRaidZones(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void RedrawPlayers(MapPlayer mapPlayer);
     void CloseMap(BasePlayer player);
     void DrawMapPlayer(BasePlayer player, MapPlayer mp, bool friend);
     void AddTemporaryMarker(string png, bool rotSupport, float size, float alpha, Transform transform, string name);
     void RemoveTemporaryMarkerByName(string name);
     void RemoveTemporaryMarker(MapMarker mm);
     void DrawIcon(BasePlayer player, string lastName, string name, string[] anchors, string png, float alpha, string text, int fontsize, bool destroy);
     void DrawIconNull(BasePlayer player, string lastName, string name, string[] anchors, string png, float alpha, string text, int fontsize, bool destroy);
     void DrawDeath(BasePlayer player, string lastName, string name, string[] anchors, string png, float alpha, string text, int fontsize, bool destroy);
     void DrawMapMarker(BasePlayer player, MapMarker mm);
     bool InMap(Vector3 pos);
     List<ulong> bannedCache;
     Dictionary<BasePlayer, MapMarker> bannedMarkers;
     void AddBannedMarker(BasePlayer player);
     void RemoveBannedMarker(BasePlayer player);
     void BansUpdate();
     void OnPlayerBanned(Connection connection, string reason);
     void OnEntityDeath(BaseCombatEntity entity);
     void FindStaticMarkers();
    [PluginReference]
     Plugin SpawnsControl;
     Dictionary<Vector3, int> GetSpawnZones();
     Dictionary<string, Vector3> GetHomes(BasePlayer player);
     Dictionary<Vector3, int> GetPlayerQuarries(ulong userId);
     string[] ToAnchors(Vector3 position, float size);
     Vector2 ToScreenCoords(Vector3 vec);
    static int GetRotation(float angle);
    private void DisableMaps(BasePlayer player);
     bool mapLoaded;
     IEnumerator DownloadMapImage();
     void GetQueueID();
     void CheckAvailability(string queueId);
     void ProcessResponse(string queueId, string response);
     void GetMapURL(string queueId);
     string Format(string value, object[] args);
    private System.Random random;
     IEnumerator LoadImages();
     string MapFilename { get; set; }
     List<string> imagesKeys { get; set; }
     string PlayerPng(int rot);
     string FriendPng(int rot);
     string PlanePng(int rot);
     string MapPng();
    [HookMethod("RaidHomePng")]
     string RaidHomePng();
     Dictionary<string, string> images;
    private GameObject FileManagerObject;
    private FileManager m_FileManager;
     void InitFileManager();
     class FileManager : MonoBehaviour
    {
         int loaded;
         int needed;
        public bool IsFinished { get; set; }
        const ulong MaxActiveLoads;
         Dictionary<string, FileInfo> files;
         DynamicConfigFile dataFile;
        private class FileInfo
        {
            public string Url;
            public string Png;
        }

        public void SaveData();
        public void WipeData();
        public string GetPng(string name);
        private void Awake();
        public IEnumerator LoadFile(string name, string url, int size);
         IEnumerator LoadImageCoroutine(string name, string url, int size);
        static byte[] Resize(byte[] bytes, int size);
    }

    public static class PermissionService
    {
        public static Permission permission;
        public static bool HasPermission(ulong uid, string permissionName);
        public static void RegisterPermissions(Plugin owner, List<string> permissions);
    }

}

 class MapMarker
{
    public Transform transform;
    public int Rotation;
    public Vector2 anchorPosition { get; set; }
    public Vector2 position;
    public string name;
    public string png;
    public bool rotSupport;
    public string text;
    public float size;
    public float alpha;
    public int fontsize;
    public ulong counter;
    public bool inMap;
    protected MapMarker(Transform transform);
    public virtual bool NeedRedraw();
    public static MapMarker Create(Transform transform);
}

 class MapPlayer : MapMarker
{
    public BasePlayer player;
    public List <MapPlayer> clanTeam;
    protected MapPlayer(BasePlayer player);
    public override bool NeedRedraw();
    public void OnCloseMap();
    public static MapPlayer Create(BasePlayer player);
}

 class DataStorage
{
    public Dictionary<ulong, MAPDATA> MapPlayerData;
    public DataStorage();
}

 class MAPDATA
{
    public string Name;
    public bool Homes;
    public bool Friends;
    public bool Clans;
    public bool Death;
    public bool AllPlayers;
    public bool BanPlayers;
}

 class FileManager : MonoBehaviour
{
     int loaded;
     int needed;
    public bool IsFinished { get; set; }
    const ulong MaxActiveLoads;
     Dictionary<string, FileInfo> files;
     DynamicConfigFile dataFile;
    private class FileInfo
    {
        public string Url;
        public string Png;
    }

    public void SaveData();
    public void WipeData();
    public string GetPng(string name);
    private void Awake();
    public IEnumerator LoadFile(string name, string url, int size);
     IEnumerator LoadImageCoroutine(string name, string url, int size);
    static byte[] Resize(byte[] bytes, int size);
}

private class FileInfo
{
    public string Url;
    public string Png;
}

public static class PermissionService
{
    public static Permission permission;
    public static bool HasPermission(ulong uid, string permissionName);
    public static void RegisterPermissions(Plugin owner, List<string> permissions);
}


```

---

## RustMap (1)

```csharp
using Network;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Drawing;
using System.Drawing.Imaging;
using System.IO;
using System.Linq;
using System.Text;
using UnityEngine;

Oxide.Plugins
[Info("RustMap", "Ernieleo", "1.3.42")]
 class RustMap : RustPlugin
{
    [PluginReference]
     Plugin Clans;
     Plugin Friends;
     Plugin MutualPermission;
     Plugin NTeleportation;
     Plugin Teleport;
     Plugin Teleportation;
     Plugin CustomQuarry;
     Plugin HomesGUI;
     Plugin PlayersClasses;
     class MapMarker
    {
        public Transform transform;
        public int Rotation;
        public Vector2 anchorPosition { get; set; }
        public Vector2 position;
        public string name;
        public string png;
        public bool rotSupport;
        public string text;
        public float size;
        public float alpha;
        public int fontsize;
        public ulong counter;
        public bool inMap;
        protected MapMarker(Transform transform);
        public virtual bool NeedRedraw();
        public static MapMarker Create(Transform transform);
    }

     class MapPlayer : MapMarker
    {
        public BasePlayer player;
        public List<MapPlayer> clanTeam;
        protected MapPlayer(BasePlayer player);
        public override bool NeedRedraw();
        public void OnCloseMap();
        public static MapPlayer Create(BasePlayer player);
    }

     string beancanKey;
     float mapAlpha;
     float mapSize;
     bool clanSupport;
     bool TeamSupport;
     bool friendsSupport;
     float playerIconSize;
     float playerIconAlpha;
     bool playerCoordinates;
     bool monuments;
     bool caves;
     bool water;
     bool powersub;
     float monumentIconSize;
     float monumentIconAlpha;
     bool monumentIconNames;
     float MapUpdate;
     bool raidhomes;
     float raidhomeSIze;
     int monumentsFontSize;
     bool bradley;
     float bradleyIconSize;
     float bradleyIconAlpha;
     bool car;
     bool plane;
     float planeIconSize;
     float planeIconAlpha;
     float TimeToClose;
     bool NewHeli;
     float NewheliIconSize;
     float NewheliIconAlpha;
     bool ship;
     float CargoIconSize;
     float CargoIconAlpha;
     bool swamp;
     bool Oilrig;
     bool NewHeliCrate;
     float NewHeliCrateIconSize;
     float NewHeliCrateIconAlpha;
     bool planeDrop;
     bool sethome;
     float sethomeIconSize;
     float deathIconSize;
     float planeDropIconSize;
     float planeDropIconAlpha;
     bool heli;
     float heliIconSize;
     float heliIconAlpha;
     bool heliDrop;
     bool quarry;
     float quarryIconAlpha;
     float quarryIconSize;
     float heliDropIconSize;
     float heliDropIconAlpha;
     bool vendingMachine;
     bool vendingMachineEmpty;
     float vendingMachineIconSize;
     float vendingMachineIconAlpha;
     string textColor;
     string textColor1;
     float bannedSize;
     string mapUrl;
     string MapText;
    private string version;
     bool NPCVendingMachine;
    protected override void LoadDefaultConfig();
     T GetConfig(string name, T defaultValue);
    static RustMap m_Instance;
    private string mapIconJson;
    private string mapIconTextJson;
    private string mapIconTextJsonIcon;
    private string mapJson;
    private string mapCoordsTextJson;
    private string mapHelpJson;
     Dictionary<BasePlayer, MapPlayer> mapPlayers;
     Dictionary<BasePlayer, MapPlayer> subscribers;
     List<MapMarker> temporaryMarkers;
    const string MAP_ADMIN;
    static float mapSize1;
    static string mapSeed;
    static int worldSize;
    static string level;
     string monumentsJson;
     bool init;
    private List<BasePlayer> AllPlayerUsers;
    private Timer CloseMapTimer;
    [ChatCommand("map")]
     void cmdMapControl(BasePlayer player, string command, string[] args);
     string button;
    private string MapHelp;
     void HelpUI(BasePlayer player);
    [ConsoleCommand("cmd.help")]
    private void CmdHelpControll(ConsoleSystem.Arg arg);
    [ConsoleCommand("map.wipe")]
    private void CmdTest(ConsoleSystem.Arg arg);
    [ConsoleCommand("map.open")]
     void ConsoleMap(ConsoleSystem.Arg arg);
    private Timer mtimer;
     void OnServerInitialized();
     void LoadData();
     void OnServerSave();
    private void PlayerSaveData();
     class DataStorage
    {
        public Dictionary<ulong, MAPDATA> MapPlayerData;
        public DataStorage();
    }

     class MAPDATA
    {
        public string Name;
        public bool Homes;
        public bool Friends;
        public bool Clans;
        public bool Death;
        public bool AllPlayers;
        public bool Teams;
        public bool BanPlayers;
        public Vector3 CustomIcon;
    }

     DataStorage data;
    private DynamicConfigFile MapPlayerData;
     void OnPlayerInit(BasePlayer player);
     void OnNewSave();
     void Unload();
     void OnEntitySpawned(BaseEntity entity);
     void OnEntityKill(BaseNetworkable entity);
     void OpenMap(BasePlayer player);
    private List<ulong> GetTeamMembers(BasePlayer player);
    private Dictionary<ulong, string> playerDic;
     string permissionName;
    private object OnPlayerDie(BasePlayer player, HitInfo info);
    [PluginReference]
     Plugin NoEscape;
     List<Vector3> GetRaidZones(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void RedrawPlayers(MapPlayer mapPlayer);
     void CloseMap(BasePlayer player);
     void DrawMapPlayer(BasePlayer player, MapPlayer mp, bool friend);
     void AddTemporaryMarker(string png, bool rotSupport, float size, float alpha, Transform transform, string name);
     void RemoveTemporaryMarkerByName(string name);
     void RemoveTemporaryMarker(MapMarker mm);
     void DrawIcon(BasePlayer player, string lastName, string name, string[] anchors, string png, float alpha, string text, int fontsize, bool destroy);
     void DrawIconNull(BasePlayer player, string lastName, string name, string[] anchors, string png, float alpha, string text, int fontsize, bool destroy);
     void DrawDeath(BasePlayer player, string lastName, string name, string[] anchors, string png, float alpha, string text, int fontsize, bool destroy);
     void DrawMapMarker(BasePlayer player, MapMarker mm);
     bool InMap(Vector3 pos);
     List<ulong> bannedCache;
     Dictionary<BasePlayer, MapMarker> bannedMarkers;
     void AddBannedMarker(BasePlayer player);
     void RemoveBannedMarker(BasePlayer player);
     void BansUpdate();
     void OnPlayerBanned(Connection connection, string reason);
     void OnEntityDeath(BaseCombatEntity entity);
     void FindStaticMarkers();
    [PluginReference]
     Plugin SpawnsControl;
     Dictionary<Vector3, int> GetSpawnZones();
     Dictionary<string, Vector3> GetHomes(BasePlayer player);
     Dictionary<Vector3, int> GetPlayerQuarries(ulong userId);
     string[] ToAnchors(Vector3 position, float size, string yes);
     Vector2 ToScreenCoords(Vector3 vec);
    static int GetRotation(float angle);
    private void DisableMaps(BasePlayer player);
     bool mapLoaded;
     IEnumerator DownloadMapImage();
     void GetQueueID();
     void CheckAvailability(string queueId);
     void ProcessResponse(string queueId, string response);
     void GetMapURL(string queueId);
     string Format(string value, object[] args);
    private System.Random random;
     IEnumerator LoadImages();
     string MapFilename { get; set; }
     List<string> imagesKeys { get; set; }
     string PlayerPng(int rot);
     string FriendPng(int rot);
     string PlanePng(int rot);
     string MapPng();
    [HookMethod("RaidHomePng")]
     string RaidHomePng();
     Dictionary<string, string> images;
    private GameObject FileManagerObject;
    private FileManager m_FileManager;
     void InitFileManager();
     class FileManager : MonoBehaviour
    {
         int loaded;
         int needed;
        public bool IsFinished { get; set; }
        const ulong MaxActiveLoads;
         Dictionary<string, FileInfo> files;
         DynamicConfigFile dataFile;
        private class FileInfo
        {
            public string Url;
            public string Png;
        }

        public void SaveData();
        public void WipeData();
        public string GetPng(string name);
        private void Awake();
        public IEnumerator LoadFile(string name, string url, int size);
         IEnumerator LoadImageCoroutine(string name, string url, int size);
        static byte[] Resize(byte[] bytes, int size);
    }

    public static class PermissionService
    {
        public static Permission permission;
        public static bool HasPermission(ulong uid, string permissionName);
        public static void RegisterPermissions(Plugin owner, List<string> permissions);
    }

}

 class MapMarker
{
    public Transform transform;
    public int Rotation;
    public Vector2 anchorPosition { get; set; }
    public Vector2 position;
    public string name;
    public string png;
    public bool rotSupport;
    public string text;
    public float size;
    public float alpha;
    public int fontsize;
    public ulong counter;
    public bool inMap;
    protected MapMarker(Transform transform);
    public virtual bool NeedRedraw();
    public static MapMarker Create(Transform transform);
}

 class MapPlayer : MapMarker
{
    public BasePlayer player;
    public List<MapPlayer> clanTeam;
    protected MapPlayer(BasePlayer player);
    public override bool NeedRedraw();
    public void OnCloseMap();
    public static MapPlayer Create(BasePlayer player);
}

 class DataStorage
{
    public Dictionary<ulong, MAPDATA> MapPlayerData;
    public DataStorage();
}

 class MAPDATA
{
    public string Name;
    public bool Homes;
    public bool Friends;
    public bool Clans;
    public bool Death;
    public bool AllPlayers;
    public bool Teams;
    public bool BanPlayers;
    public Vector3 CustomIcon;
}

 class FileManager : MonoBehaviour
{
     int loaded;
     int needed;
    public bool IsFinished { get; set; }
    const ulong MaxActiveLoads;
     Dictionary<string, FileInfo> files;
     DynamicConfigFile dataFile;
    private class FileInfo
    {
        public string Url;
        public string Png;
    }

    public void SaveData();
    public void WipeData();
    public string GetPng(string name);
    private void Awake();
    public IEnumerator LoadFile(string name, string url, int size);
     IEnumerator LoadImageCoroutine(string name, string url, int size);
    static byte[] Resize(byte[] bytes, int size);
}

private class FileInfo
{
    public string Url;
    public string Png;
}

public static class PermissionService
{
    public static Permission permission;
    public static bool HasPermission(ulong uid, string permissionName);
    public static void RegisterPermissions(Plugin owner, List<string> permissions);
}


```

---

## RustMenu

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using ProtoBuf;
using UnityEngine;

Oxide.Plugins
[Info("Rust Menu", "Hougan", "0.0.1")]
public class RustMenu : RustPlugin
{
    private Dictionary<BasePlayer, MenuPoint> OpenMenu;
    private class Notification
    {
        public string Title;
        public string Information;
        public string Color;
        public int Duration;
        public string SoundEffect;
        public string ImageID;
        public string Command;
    }

    private static class Interface
    {
        public static string External;
        public static string Internal;
        public static string InterInternal;
        public static string PrivPoint;
        public static string MenuPoint;
        public static string Header;
        public static string NotificationImage;
        public static void DrawPrivileges(BasePlayer player);
        private static string GetTimeFromSecs(int sec);
        public static void DrawMenuWithoutPoints(BasePlayer player);
        public static void DrawMenuPoints(BasePlayer player, MenuPoint choosed);
        public static void DrawExternalLayer(BasePlayer player);
        public static void OpenMenu(BasePlayer player);
    }

    private class MenuPoint
    {
        public string DisplayName;
        public string DrawMethod;
        public float TextMargin;
        public string TextMethod;
        public bool NewInfo;
    }

    private class Configuration
    {
        [JsonProperty("Название сервера")]
        public string ServerName;
        [JsonProperty("Список доступных разделов меню")]
        public List<MenuPoint> Points;
        public Dictionary<string, string> GroupImages;
        public Dictionary<float, string> RawImages;
        public static Configuration Generate();
    }

    [PluginReference]
    private Plugin ImageLibrary;
    private static RustMenu _;
    private static Configuration Settings;
    private void OnServerInitialized();
    [ConsoleCommand("UI_RM_Handler")]
    private void CmdConsoleRustMenu(ConsoleSystem.Arg args);
    [ChatCommand("menuSecret")]
    private void CmdChatXs(BasePlayer player);
    [ChatCommand("menu")]
    private void CmdChatX(BasePlayer player);
    private void Unload();
}

private class Notification
{
    public string Title;
    public string Information;
    public string Color;
    public int Duration;
    public string SoundEffect;
    public string ImageID;
    public string Command;
}

private static class Interface
{
    public static string External;
    public static string Internal;
    public static string InterInternal;
    public static string PrivPoint;
    public static string MenuPoint;
    public static string Header;
    public static string NotificationImage;
    public static void DrawPrivileges(BasePlayer player);
    private static string GetTimeFromSecs(int sec);
    public static void DrawMenuWithoutPoints(BasePlayer player);
    public static void DrawMenuPoints(BasePlayer player, MenuPoint choosed);
    public static void DrawExternalLayer(BasePlayer player);
    public static void OpenMenu(BasePlayer player);
}

private class MenuPoint
{
    public string DisplayName;
    public string DrawMethod;
    public float TextMargin;
    public string TextMethod;
    public bool NewInfo;
}

private class Configuration
{
    [JsonProperty("Название сервера")]
    public string ServerName;
    [JsonProperty("Список доступных разделов меню")]
    public List<MenuPoint> Points;
    public Dictionary<string, string> GroupImages;
    public Dictionary<float, string> RawImages;
    public static Configuration Generate();
}


```

---

## RustShop

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("RustShop", "topplugin.ru", "1.0.4")]
public class RustShop : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
     class DataStorage
    {
        public Dictionary<ulong, PlayersBalances> PlayerBalance;
        public DataStorage();
    }

     class PlayersBalances
    {
        public string Name;
        public int Balances;
        public int Time;
    }

     DataStorage data;
    private List<string> ListedCategories;
    private class ProductsData
    {
        [JsonProperty("Название предмета")]
        public string Name;
        [JsonProperty("Категория предмета")]
        public string Category;
        [JsonProperty("Стоимость предмета")]
        public int Price;
        [JsonProperty("Количество предмета")]
        public int Amount;
        [JsonProperty("Система. id предмета")]
        public string ShortName;
    }

    public int StartBalance;
    public int HourAmount;
    private List<ProductsData> shopElements;
    static List<string> permisions;
    static int GetDiscountSize(BasePlayer player);
     bool BPEnabled;
     double BPPrice;
     string AMin;
     string AMax;
    private void LoadConfigValues();
    private bool GetConfig(string MainMenu, string Key, T var);
    private List<string> Available;
    private void OnServerInitialized();
     Dictionary<BasePlayer, int> timers;
     void TimerHandler();
     List<ulong> activePlayers;
     void DeactivateTimer(BasePlayer player);
    private static string Format(int units, string form1, string form2, string form3);
     void OnPlayerConnected(BasePlayer player);
     void ActivateTimer(ulong userId);
     void OnPlayerDisconnected(BasePlayer player);
     void OnServerSave();
     void SaveData();
     void LoadData();
     void ChangeBalance(BasePlayer player, string mode, int Amount, bool change);
    private void GiveBlueprint(BasePlayer player, string itemkey, int amount);
     void Unload();
    [ConsoleCommand("balance")]
     void cmdChangeBalance(ConsoleSystem.Arg args);
    [ChatCommand("shop")]
     void cmdChatShop(BasePlayer player, string command, string[] args);
    [ConsoleCommand("shop")]
     void cmdConsoleShop(ConsoleSystem.Arg args);
    [ConsoleCommand("product_info")]
     void cmdProductInfo(ConsoleSystem.Arg args);
    [ConsoleCommand("Buy")]
     void cmdConsoleBuy(ConsoleSystem.Arg args);
     string Button;
     void DrawButton(BasePlayer player);
     string Product;
     void DrawProductInfo(BasePlayer player, string product);
     string GUI;
    private void ShopGUI(BasePlayer player, string name);
    private void AddBalance(ulong userId, int Amount);
    private object RemoveBalance(ulong userId, int Amount);
    private object GetBalance(ulong userId);
    public static class PermissionService
    {
        public static Permission permission;
        public static bool HasPermission(BasePlayer player, string permissionName);
        public static void RegisterPermissions(Plugin owner, List<string> permissions);
    }

     Dictionary<string, string> Messages;
}

 class DataStorage
{
    public Dictionary<ulong, PlayersBalances> PlayerBalance;
    public DataStorage();
}

 class PlayersBalances
{
    public string Name;
    public int Balances;
    public int Time;
}

private class ProductsData
{
    [JsonProperty("Название предмета")]
    public string Name;
    [JsonProperty("Категория предмета")]
    public string Category;
    [JsonProperty("Стоимость предмета")]
    public int Price;
    [JsonProperty("Количество предмета")]
    public int Amount;
    [JsonProperty("Система. id предмета")]
    public string ShortName;
}

public static class PermissionService
{
    public static Permission permission;
    public static bool HasPermission(BasePlayer player, string permissionName);
    public static void RegisterPermissions(Plugin owner, List<string> permissions);
}


```

---

## RustTanic

```csharp
using Oxide.Core;
using Rust;
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System.Reflection;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("RustTanic", "redBDGR", "1.0.3")]
[Description("Server event for the Cargo Ship")]
 class RustTanic : RustPlugin
{
    private bool Changed;
    private static RustTanic plugin;
    private TitanicManager manager;
    private int worldLayer;
    private int defaultLayer;
    private FieldInfo tooHotUntil;
    private const string permissionNameADMIN;
    private List<ItemInfo> floatingCrateTable;
    private List<ItemInfo> icebergCrateTable;
    private class ConfigFile
    {
        public static float sequencingGaps;
        public static float spawnDistance;
        public static float boatSpeed;
        public static bool disableIcebergsWithCupboard;
        public static bool disableIcebergsWithBuilding;
        public static bool debugMode;
        public static int floatingCratesToSpawn;
        public static float floatingCratesRadius;
        public static float floatingCrateDespawnTime;
        public static int floatingCrateMinItems;
        public static int floatingCrateMaxItems;
        public static int debisToSpawn;
        public static int lootPerDebris;
        public static float hackableCrateSeconds;
        public static bool spawnIcebergCrates;
        public static int icebergCratesToSpawn;
        public static float icebergCratesSpawnRadius;
        public static string icebergCratePrefab;
        public static int icebergCrateMinItems;
        public static int icebergCrateMaxItems;
        public static bool killBoatScientists;
        public static int icebergNPCToSpawn;
        public static float icebergNPCSpawnRadius;
        public static string icebergNPCType;
        public static string icebergNPCNames;
        public static bool boatExhaustFlames;
        public static bool boatRotatingLights;
        public static bool disableExtraEffects;
        public static float boatHornInterval;
        public static bool randomEventEnabled;
        public static float minRandomEventTime;
        public static float maxRandomEventTime;
        public static int minPlayersForEvent;
        public static bool targetIcebergMarkerEnabled;
        public static float targetIcebergMarkerRadius;
        public static string targetIcerbergText;
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private DynamicConfigFile floatingCrateFile;
    private DynamicConfigFile icebergCrateFile;
     StoredData storedData;
    private class StoredData
    {
        public List<ItemInfo> lootTable;
    }

    private class ItemInfo
    {
        public string itemName;
        public int minItemAmount;
        public int maxItemAmount;
        public float chance;
        public ulong skinID;
    }

     void SaveData();
     void LoadData();
    private void OnServerInitialized();
    private void Unload();
    private class TitanicManager : FacepunchBehaviour
    {
        private List<GameObject> allIcebergs;
        public List<GameObject> targetIcebergs;
        public List<Titanic> currentTitanics;
        private Dictionary<CargoShip, GameObject> setIcebergs;
        private void Awake();
        private void OnDestroy();
        private void PopulateAllIcebergs();
        private IEnumerator UpdateViableIcebergs();
        public void TimedEvent();
        public void SpawnNearestIceberg(Vector3 pos);
        public void SpawnTitanic(GameObject targetIceberg);
        public GameObject GetTargetIceberg(CargoShip ship);
    }

    private class Titanic : FacepunchBehaviour
    {
        private CargoShip ship;
        public GameObject targetIceberg;
        private MapMarkerGenericRadius radiusMarker;
        private VendingMachineMapMarker textMarker;
        private Transform nosePosition;
        private Transform exhaust1;
        private Transform exhaust2;
        private Vector3 collisionPoint;
        private Vector3 startSinkingPosition;
        private Vector3 halfSinkingPosition;
        private Vector3 finishedSinkingPosition;
        private List<Vector3> deckExplosionPositions;
        private List<Vector3> searchLightPositions;
        private List<BaseEntity> searchLights;
        private List<BaseEntity> npcs;
        private List<Transform> shipTransforms;
        private Coroutine exhaustFireRoutine;
        private float tippingSpeed;
        public bool hasCollided;
        public bool isTipping;
        public bool isSinking;
        public bool isReverseTipping;
        public bool isFinalSinking;
        public bool isPaused;
        private float finalSinkStartTime;
        private float pauseEndTime;
        private float startTime;
        private float startDistance;
        private void Awake();
        private void OnDestroy();
        private void FixedUpdate();
        private void SetInitExtraEffects();
        private void InitSearchLights();
        private IEnumerator FireFromExhaust();
        private IEnumerator ExplodeDeck();
        private void SetCollisionPoint();
        private void OnCollideWithIceberg();
        private void OnFinishTipping();
        private void OnFinishSinking();
        private void StartTipCorrection();
        private void OnFinishReverseTipping();
        private IEnumerator FinalExplosion();
        private IEnumerator HandleNPCs();
        private void HandleLoot();
        private IEnumerator SpawnIcebergCrates();
        private IEnumerator SpawnFloatingCrates();
        private IEnumerator SpawnItems(ItemContainer container, List<ItemInfo> table, int min, int max);
        private IEnumerator SetHackableCrateTimers();
        public void PauseToLoot(float pauseTime);
        private Vector3 GetRandomOceanRadiusPosition(Vector3 centre, float radius);
        private void InitializeSpawnPosition();
        private Transform InitializeTransform(Transform trans, Vector3 offset);
        private void TransformDebug();
        private void DebugPos(Vector3 pos);
        private void DebugRay(Vector3 pos1, Vector3 pos2);
        private void DebugHit(RaycastHit hit);
        private void Broadcast(string text);
    }

    private class TitanicSpinningLight : FacepunchBehaviour
    {
        private SearchLight light;
        private float rotation;
        private void Awake();
        private void FixedUpdate();
        private Vector3 NewLookPosition();
    }

    [ChatCommand("calltitanic")]
    private void SpawnTitanicCMD(BasePlayer player, string command, string[] args);
    [ConsoleCommand("calltitanic")]
    private void SpawnTitanicConsoleCMD(ConsoleSystem.Arg arg);
    [ConsoleCommand("rusttanic.updatedata")]
    private void UpdateTableDataConsoleCMD(ConsoleSystem.Arg arg);
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private string msg(string key, string id);
}

private class ConfigFile
{
    public static float sequencingGaps;
    public static float spawnDistance;
    public static float boatSpeed;
    public static bool disableIcebergsWithCupboard;
    public static bool disableIcebergsWithBuilding;
    public static bool debugMode;
    public static int floatingCratesToSpawn;
    public static float floatingCratesRadius;
    public static float floatingCrateDespawnTime;
    public static int floatingCrateMinItems;
    public static int floatingCrateMaxItems;
    public static int debisToSpawn;
    public static int lootPerDebris;
    public static float hackableCrateSeconds;
    public static bool spawnIcebergCrates;
    public static int icebergCratesToSpawn;
    public static float icebergCratesSpawnRadius;
    public static string icebergCratePrefab;
    public static int icebergCrateMinItems;
    public static int icebergCrateMaxItems;
    public static bool killBoatScientists;
    public static int icebergNPCToSpawn;
    public static float icebergNPCSpawnRadius;
    public static string icebergNPCType;
    public static string icebergNPCNames;
    public static bool boatExhaustFlames;
    public static bool boatRotatingLights;
    public static bool disableExtraEffects;
    public static float boatHornInterval;
    public static bool randomEventEnabled;
    public static float minRandomEventTime;
    public static float maxRandomEventTime;
    public static int minPlayersForEvent;
    public static bool targetIcebergMarkerEnabled;
    public static float targetIcebergMarkerRadius;
    public static string targetIcerbergText;
}

private class StoredData
{
    public List<ItemInfo> lootTable;
}

private class ItemInfo
{
    public string itemName;
    public int minItemAmount;
    public int maxItemAmount;
    public float chance;
    public ulong skinID;
}

private class TitanicManager : FacepunchBehaviour
{
    private List<GameObject> allIcebergs;
    public List<GameObject> targetIcebergs;
    public List<Titanic> currentTitanics;
    private Dictionary<CargoShip, GameObject> setIcebergs;
    private void Awake();
    private void OnDestroy();
    private void PopulateAllIcebergs();
    private IEnumerator UpdateViableIcebergs();
    public void TimedEvent();
    public void SpawnNearestIceberg(Vector3 pos);
    public void SpawnTitanic(GameObject targetIceberg);
    public GameObject GetTargetIceberg(CargoShip ship);
}

private class Titanic : FacepunchBehaviour
{
    private CargoShip ship;
    public GameObject targetIceberg;
    private MapMarkerGenericRadius radiusMarker;
    private VendingMachineMapMarker textMarker;
    private Transform nosePosition;
    private Transform exhaust1;
    private Transform exhaust2;
    private Vector3 collisionPoint;
    private Vector3 startSinkingPosition;
    private Vector3 halfSinkingPosition;
    private Vector3 finishedSinkingPosition;
    private List<Vector3> deckExplosionPositions;
    private List<Vector3> searchLightPositions;
    private List<BaseEntity> searchLights;
    private List<BaseEntity> npcs;
    private List<Transform> shipTransforms;
    private Coroutine exhaustFireRoutine;
    private float tippingSpeed;
    public bool hasCollided;
    public bool isTipping;
    public bool isSinking;
    public bool isReverseTipping;
    public bool isFinalSinking;
    public bool isPaused;
    private float finalSinkStartTime;
    private float pauseEndTime;
    private float startTime;
    private float startDistance;
    private void Awake();
    private void OnDestroy();
    private void FixedUpdate();
    private void SetInitExtraEffects();
    private void InitSearchLights();
    private IEnumerator FireFromExhaust();
    private IEnumerator ExplodeDeck();
    private void SetCollisionPoint();
    private void OnCollideWithIceberg();
    private void OnFinishTipping();
    private void OnFinishSinking();
    private void StartTipCorrection();
    private void OnFinishReverseTipping();
    private IEnumerator FinalExplosion();
    private IEnumerator HandleNPCs();
    private void HandleLoot();
    private IEnumerator SpawnIcebergCrates();
    private IEnumerator SpawnFloatingCrates();
    private IEnumerator SpawnItems(ItemContainer container, List<ItemInfo> table, int min, int max);
    private IEnumerator SetHackableCrateTimers();
    public void PauseToLoot(float pauseTime);
    private Vector3 GetRandomOceanRadiusPosition(Vector3 centre, float radius);
    private void InitializeSpawnPosition();
    private Transform InitializeTransform(Transform trans, Vector3 offset);
    private void TransformDebug();
    private void DebugPos(Vector3 pos);
    private void DebugRay(Vector3 pos1, Vector3 pos2);
    private void DebugHit(RaycastHit hit);
    private void Broadcast(string text);
}

private class TitanicSpinningLight : FacepunchBehaviour
{
    private SearchLight light;
    private float rotation;
    private void Awake();
    private void FixedUpdate();
    private Vector3 NewLookPosition();
}


```

---

## RustTanic-2.0.10

```csharp
using Facepunch;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Ext.ChaosNPC;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Reflection;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("RustTanic", "k1lly0u", "2.0.10")]
 class RustTanic : RustPlugin, IChaosNPCPlugin
{
    private static RustTanic Instance;
    private static RaycastHit RaycastHit;
    private const string HELI_EXPLOSION;
    private const string C4_EXPLOSION;
    private const string FLOATING_CRATE_PREFAB;
    private const string ICEBERG_NAME;
    private const string PERMISSION_ADMIN;
    private void OnServerInitialized();
    protected override void LoadDefaultMessages();
    private void OnEntityEnter(TriggerParent triggerParent, BasePlayer player);
    private void Unload();
    private void AutomationCycle();
    private bool SpawnCargoShip(Vector3 nearestTo, string failReason);
    private void SpawnCargoShip(Vector3 spawnPosition, Iceberg iceberg);
    private static void CopySerializeableFields(T src, T dst);
    public bool InitializeStates(BaseAIBrain customNPCBrain);
    public bool WantsToPopulateLoot(CustomScientistNPC customNpc, NPCPlayerCorpse npcplayerCorpse);
    public byte[] GetCustomDesign();
    private Vector3 RotateAroundPoint(Vector3 point, Vector3 origin, float degrees);
    private static Quaternion RandomUpRotation();
    private static void ReleaseFreeableLootContainer(FreeableLootContainer freeableLootContainer);
    private static void Broadcast(string key, object[] args);
    private float WorldSize;
    private Vector3 BottomLeft;
    private Vector3 TopLeft;
    private Vector3 BottomRight;
    private Vector3 TopRight;
    private bool CalculateSpawnPosition2D(Iceberg iceberg, Vector3 intersection);
    private bool IntersectsLine(Vector3 from, Vector3 to, Vector3 lineStart, Vector3 lineEnd, Vector3 intersection);
    private partial class Iceberg
    {
        public Transform Transform { get; set; }
        public Collider Collider { get; set; }
        public bool HasBuildingBlocks { get; set; }
        public bool HasToolCupboard { get; set; }
        private float validationExpireTime;
        public Iceberg(Transform transform);
        public void Validate();
    }

    private partial class Iceberg
    {
        private static readonly RaycastHit[] buffer;
        private static readonly List<Iceberg> icebergs;
        public static bool IsValid { get; set; }
        public static void FindAll();
        public static Iceberg FindClosestIceberg(Vector3 position);
        public static Iceberg GetRandom();
    }

    private class Titanic : CargoShip
    {
        private Transform Transform;
        internal Iceberg Iceberg;
        private Vector3 destination;
        private bool isSinking;
        private float timeToTake;
        private float timeTaken;
        private SinkingPhase Phase;
        private SinkPhase Phase0;
        private SinkPhase Phase1;
        private SinkPhase Phase2;
        private SinkPhase Phase3;
        private bool isWithinProximity;
        private const float HALF_LENGTH;
        public override void ServerInit();
        public override void DestroyShared();
        private new void FixedUpdate();
        private static readonly Transform[] EMPTY_SCIENTIST_SPAWNS;
        private BaseEntity explosionMarker;
        private void ProximityTriggerEvents();
        internal void OnStruckIceberg();
        private void StartSinking();
        private void SetupSinkingPhases();
        private void CreateExplosionMarker();
        private IEnumerator ProcessChildren();
        private void PauseFlyhackLoop();
        private void ExecuteSmallExplosion();
        private IEnumerator ExecuteLargeExplosions();
        private IEnumerator ExecuteNapalmLaunchers();
        private void CreateAndLaunchFireball(string prefab, Vector3 position);
        private void InvokedEffect(string effect, Vector3 localOffset, float time);
        private static readonly List<Vector3> DECK_EXPLOSION_POSITIONS;
        private List<LootContainer> shipLootContainers;
        private void SpawnInitialLoot();
        private void ProcessParentedLootContainer(LootContainer lootContainer);
        private void ReleaseLootContainers();
        private IEnumerator SpawnLoot();
        private void CreateGibs();
        private void CreateGibs(string resourcePath, GameObject source, Vector3 position, float spread);
        private static readonly LootContainer.LootSpawnSlot[] EMPTY_LOOT_SPAWNS;
        private void ProcessParentedNPC(BasePlayer player);
        private IEnumerator SpawnNPCs();
    }

    private class IcebergDetectionTrigger : MonoBehaviour
    {
        private Titanic titanic;
        private BoxCollider boxCollider;
        private void Awake();
        private void OnTriggerEnter(Collider col);
        internal static void Create(Titanic titanic);
    }

    private class DrownWhenUnderWater : MonoBehaviour
    {
        private BasePlayer player;
        private void Awake();
        private void CheckWaterLevel();
    }

    [ChatCommand("titanic")]
    private void SpawnTitanicCommand(BasePlayer player, string command, string[] args);
    [ConsoleCommand("titanic")]
    private void SpawnTitanicConsoleCommand(ConsoleSystem.Arg arg);
    private static ConfigData Configuration;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Event Automation Settings")]
        public AutomationSettings Automation { get; set; }
        [JsonProperty(PropertyName = "Iceberg Selection")]
        public IcebergSelection Iceberg { get; set; }
        [JsonProperty(PropertyName = "Boat Settings")]
        public BoatSettings Boat { get; set; }
        [JsonProperty(PropertyName = "NPC Settings")]
        public NPCSettings NPC { get; set; }
        [JsonProperty(PropertyName = "Loot Options")]
        public LootSettings Loot { get; set; }
        public class AutomationSettings
        {
            [JsonProperty(PropertyName = "Enable event automation")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Minimum time between events (seconds)")]
            public int Minimum { get; set; }
            [JsonProperty(PropertyName = "Maximum time between events (seconds)")]
            public int Maximum { get; set; }
            [JsonProperty(PropertyName = "Minimum required players to trigger the event")]
            public int RequiredPlayers { get; set; }
            [JsonIgnore]
            public int RandomTime { get; set; }
        }

        public class BoatSettings
        {
            [JsonProperty(PropertyName = "Movement speed")]
            public float Speed { get; set; }
            [JsonProperty(PropertyName = "Amount of time it takes to sink - Phase 1 (seconds)")]
            public float SinkTime1 { get; set; }
            [JsonProperty(PropertyName = "Amount of time it takes to sink - Phase 2 (seconds)")]
            public float SinkTime2 { get; set; }
            [JsonProperty(PropertyName = "Amount of time it takes to sink - Phase 3 (seconds)")]
            public float SinkTime3 { get; set; }
            [JsonProperty(PropertyName = "Cargo ship despawn time once sunk (seconds)")]
            public int DespawnTime { get; set; }
            [JsonProperty(PropertyName = "Effect Settings")]
            public EffectSettings Effects { get; set; }
            public class EffectSettings
            {
                [JsonProperty(PropertyName = "Large explosion enabled")]
                public bool LargeExplosion { get; set; }
                [JsonProperty(PropertyName = "Napalm launchers enabled")]
                public bool NapalmLaunchers { get; set; }
                [JsonProperty(PropertyName = "Alarm sound on proximity enabled")]
                public bool AlarmProximity { get; set; }
                [JsonProperty(PropertyName = "Proximity distance to trigger effects")]
                public float ProximityDistance { get; set; }
            }

        }

        public class IcebergSelection
        {
            [JsonProperty(PropertyName = "Don't target icebergs that have player construction on them")]
            public bool DisableWithBases { get; set; }
            [JsonProperty(PropertyName = "Don't target icebergs that have tool cupboards on them")]
            public bool DisableWithCupboards { get; set; }
        }

        public class NPCSettings
        {
            [JsonProperty(PropertyName = "Cargo Ship NPC Options")]
            public ShipNPCSettings ShipNPCs { get; set; }
            [JsonProperty(PropertyName = "Amount of NPCs to spawn on the iceberg")]
            public int AmountToSpawn { get; set; }
            [JsonProperty(PropertyName = "NPC Options")]
            public Oxide.Ext.ChaosNPC.NPCSettings Settings { get; set; }
            public class ShipNPCSettings
            {
                [JsonProperty(PropertyName = "Cargo Ship NPCs drop loot")]
                public bool ShipNPCDropLoot { get; set; }
                [JsonProperty(PropertyName = "Cargo Ship NPC kill mode ( KillInstantly, DieByDrowning )")]
                public KillMode Kill { get; set; }
                [JsonProperty(PropertyName = "Cargo Ship corpse mode ( NoCorpse, DropCorpse )")]
                public CorpseMode Corpse { get; set; }
            }

        }

        public class LootSettings
        {
            [JsonProperty(PropertyName = "Cargo ship loot mode ( Despawn, LeaveOnShip, FloatToSurface )")]
            public ShipLootMode ShipLoot { get; set; }
            [JsonProperty(PropertyName = "Amount of mine-able debris to spawn")]
            public int GibsToSpawn { get; set; }
            [JsonProperty(PropertyName = "Debris spread amount")]
            public float GibsSpread { get; set; }
            [JsonProperty(PropertyName = "Iceberg loot crates")]
            public LootTable Iceberg { get; set; }
            [JsonProperty(PropertyName = "Floating loot crates")]
            public LootTable Floating { get; set; }
            public class LootTable
            {
                [JsonProperty(PropertyName = "Drop loot crates")]
                public bool Enabled { get; set; }
                [JsonProperty(PropertyName = "Amount of crates to spawn")]
                public int Amount { get; set; }
                [JsonProperty(PropertyName = "Use custom loot table")]
                public bool UseCustomLoot { get; set; }
                [JsonProperty(PropertyName = "Minimum number of items per crate")]
                public int Minimum { get; set; }
                [JsonProperty(PropertyName = "Maximum number of items per crate")]
                public int Maximum { get; set; }
                [JsonProperty(PropertyName = "Despawn time (seconds)")]
                public int DespawnTime { get; set; }
                public List<LootItem> Table { get; set; }
                public class LootItem
                {
                    public string Shortname { get; set; }
                    public int Minimum { get; set; }
                    public int Maximum { get; set; }
                    public ulong Skin { get; set; }
                    [JsonProperty(PropertyName = "Minimum condition (0.0 - 1.0)")]
                    public float MinCondition { get; set; }
                    [JsonProperty(PropertyName = "Maximum condition (0.0 - 1.0)")]
                    public float MaxCondition { get; set; }
                    public float Probability { get; set; }
                    public LootItem();
                    public LootItem(string shortname, int minimum, int maximum, float probability);
                    private int GetAmount();
                    public void Create(ItemContainer container);
                }

                public IEnumerator SpawnInWater(Vector3 position, float radius);
                public IEnumerator SpawnOnIceberg(Iceberg iceberg);
                private LootContainer SpawnContainer(string prefabPath, Vector3 position, Quaternion rotation);
                private void PopulateLoot(LootContainer lootContainer);
            }

        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
}

private partial class Iceberg
{
    public Transform Transform { get; set; }
    public Collider Collider { get; set; }
    public bool HasBuildingBlocks { get; set; }
    public bool HasToolCupboard { get; set; }
    private float validationExpireTime;
    public Iceberg(Transform transform);
    public void Validate();
}

private partial class Iceberg
{
    private static readonly RaycastHit[] buffer;
    private static readonly List<Iceberg> icebergs;
    public static bool IsValid { get; set; }
    public static void FindAll();
    public static Iceberg FindClosestIceberg(Vector3 position);
    public static Iceberg GetRandom();
}

private class Titanic : CargoShip
{
    private Transform Transform;
    internal Iceberg Iceberg;
    private Vector3 destination;
    private bool isSinking;
    private float timeToTake;
    private float timeTaken;
    private SinkingPhase Phase;
    private SinkPhase Phase0;
    private SinkPhase Phase1;
    private SinkPhase Phase2;
    private SinkPhase Phase3;
    private bool isWithinProximity;
    private const float HALF_LENGTH;
    public override void ServerInit();
    public override void DestroyShared();
    private new void FixedUpdate();
    private static readonly Transform[] EMPTY_SCIENTIST_SPAWNS;
    private BaseEntity explosionMarker;
    private void ProximityTriggerEvents();
    internal void OnStruckIceberg();
    private void StartSinking();
    private void SetupSinkingPhases();
    private void CreateExplosionMarker();
    private IEnumerator ProcessChildren();
    private void PauseFlyhackLoop();
    private void ExecuteSmallExplosion();
    private IEnumerator ExecuteLargeExplosions();
    private IEnumerator ExecuteNapalmLaunchers();
    private void CreateAndLaunchFireball(string prefab, Vector3 position);
    private void InvokedEffect(string effect, Vector3 localOffset, float time);
    private static readonly List<Vector3> DECK_EXPLOSION_POSITIONS;
    private List<LootContainer> shipLootContainers;
    private void SpawnInitialLoot();
    private void ProcessParentedLootContainer(LootContainer lootContainer);
    private void ReleaseLootContainers();
    private IEnumerator SpawnLoot();
    private void CreateGibs();
    private void CreateGibs(string resourcePath, GameObject source, Vector3 position, float spread);
    private static readonly LootContainer.LootSpawnSlot[] EMPTY_LOOT_SPAWNS;
    private void ProcessParentedNPC(BasePlayer player);
    private IEnumerator SpawnNPCs();
}

private class IcebergDetectionTrigger : MonoBehaviour
{
    private Titanic titanic;
    private BoxCollider boxCollider;
    private void Awake();
    private void OnTriggerEnter(Collider col);
    internal static void Create(Titanic titanic);
}

private class DrownWhenUnderWater : MonoBehaviour
{
    private BasePlayer player;
    private void Awake();
    private void CheckWaterLevel();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Event Automation Settings")]
    public AutomationSettings Automation { get; set; }
    [JsonProperty(PropertyName = "Iceberg Selection")]
    public IcebergSelection Iceberg { get; set; }
    [JsonProperty(PropertyName = "Boat Settings")]
    public BoatSettings Boat { get; set; }
    [JsonProperty(PropertyName = "NPC Settings")]
    public NPCSettings NPC { get; set; }
    [JsonProperty(PropertyName = "Loot Options")]
    public LootSettings Loot { get; set; }
    public class AutomationSettings
    {
        [JsonProperty(PropertyName = "Enable event automation")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Minimum time between events (seconds)")]
        public int Minimum { get; set; }
        [JsonProperty(PropertyName = "Maximum time between events (seconds)")]
        public int Maximum { get; set; }
        [JsonProperty(PropertyName = "Minimum required players to trigger the event")]
        public int RequiredPlayers { get; set; }
        [JsonIgnore]
        public int RandomTime { get; set; }
    }

    public class BoatSettings
    {
        [JsonProperty(PropertyName = "Movement speed")]
        public float Speed { get; set; }
        [JsonProperty(PropertyName = "Amount of time it takes to sink - Phase 1 (seconds)")]
        public float SinkTime1 { get; set; }
        [JsonProperty(PropertyName = "Amount of time it takes to sink - Phase 2 (seconds)")]
        public float SinkTime2 { get; set; }
        [JsonProperty(PropertyName = "Amount of time it takes to sink - Phase 3 (seconds)")]
        public float SinkTime3 { get; set; }
        [JsonProperty(PropertyName = "Cargo ship despawn time once sunk (seconds)")]
        public int DespawnTime { get; set; }
        [JsonProperty(PropertyName = "Effect Settings")]
        public EffectSettings Effects { get; set; }
        public class EffectSettings
        {
            [JsonProperty(PropertyName = "Large explosion enabled")]
            public bool LargeExplosion { get; set; }
            [JsonProperty(PropertyName = "Napalm launchers enabled")]
            public bool NapalmLaunchers { get; set; }
            [JsonProperty(PropertyName = "Alarm sound on proximity enabled")]
            public bool AlarmProximity { get; set; }
            [JsonProperty(PropertyName = "Proximity distance to trigger effects")]
            public float ProximityDistance { get; set; }
        }

    }

    public class IcebergSelection
    {
        [JsonProperty(PropertyName = "Don't target icebergs that have player construction on them")]
        public bool DisableWithBases { get; set; }
        [JsonProperty(PropertyName = "Don't target icebergs that have tool cupboards on them")]
        public bool DisableWithCupboards { get; set; }
    }

    public class NPCSettings
    {
        [JsonProperty(PropertyName = "Cargo Ship NPC Options")]
        public ShipNPCSettings ShipNPCs { get; set; }
        [JsonProperty(PropertyName = "Amount of NPCs to spawn on the iceberg")]
        public int AmountToSpawn { get; set; }
        [JsonProperty(PropertyName = "NPC Options")]
        public Oxide.Ext.ChaosNPC.NPCSettings Settings { get; set; }
        public class ShipNPCSettings
        {
            [JsonProperty(PropertyName = "Cargo Ship NPCs drop loot")]
            public bool ShipNPCDropLoot { get; set; }
            [JsonProperty(PropertyName = "Cargo Ship NPC kill mode ( KillInstantly, DieByDrowning )")]
            public KillMode Kill { get; set; }
            [JsonProperty(PropertyName = "Cargo Ship corpse mode ( NoCorpse, DropCorpse )")]
            public CorpseMode Corpse { get; set; }
        }

    }

    public class LootSettings
    {
        [JsonProperty(PropertyName = "Cargo ship loot mode ( Despawn, LeaveOnShip, FloatToSurface )")]
        public ShipLootMode ShipLoot { get; set; }
        [JsonProperty(PropertyName = "Amount of mine-able debris to spawn")]
        public int GibsToSpawn { get; set; }
        [JsonProperty(PropertyName = "Debris spread amount")]
        public float GibsSpread { get; set; }
        [JsonProperty(PropertyName = "Iceberg loot crates")]
        public LootTable Iceberg { get; set; }
        [JsonProperty(PropertyName = "Floating loot crates")]
        public LootTable Floating { get; set; }
        public class LootTable
        {
            [JsonProperty(PropertyName = "Drop loot crates")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Amount of crates to spawn")]
            public int Amount { get; set; }
            [JsonProperty(PropertyName = "Use custom loot table")]
            public bool UseCustomLoot { get; set; }
            [JsonProperty(PropertyName = "Minimum number of items per crate")]
            public int Minimum { get; set; }
            [JsonProperty(PropertyName = "Maximum number of items per crate")]
            public int Maximum { get; set; }
            [JsonProperty(PropertyName = "Despawn time (seconds)")]
            public int DespawnTime { get; set; }
            public List<LootItem> Table { get; set; }
            public class LootItem
            {
                public string Shortname { get; set; }
                public int Minimum { get; set; }
                public int Maximum { get; set; }
                public ulong Skin { get; set; }
                [JsonProperty(PropertyName = "Minimum condition (0.0 - 1.0)")]
                public float MinCondition { get; set; }
                [JsonProperty(PropertyName = "Maximum condition (0.0 - 1.0)")]
                public float MaxCondition { get; set; }
                public float Probability { get; set; }
                public LootItem();
                public LootItem(string shortname, int minimum, int maximum, float probability);
                private int GetAmount();
                public void Create(ItemContainer container);
            }

            public IEnumerator SpawnInWater(Vector3 position, float radius);
            public IEnumerator SpawnOnIceberg(Iceberg iceberg);
            private LootContainer SpawnContainer(string prefabPath, Vector3 position, Quaternion rotation);
            private void PopulateLoot(LootContainer lootContainer);
        }

    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class AutomationSettings
{
    [JsonProperty(PropertyName = "Enable event automation")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Minimum time between events (seconds)")]
    public int Minimum { get; set; }
    [JsonProperty(PropertyName = "Maximum time between events (seconds)")]
    public int Maximum { get; set; }
    [JsonProperty(PropertyName = "Minimum required players to trigger the event")]
    public int RequiredPlayers { get; set; }
    [JsonIgnore]
    public int RandomTime { get; set; }
}

public class BoatSettings
{
    [JsonProperty(PropertyName = "Movement speed")]
    public float Speed { get; set; }
    [JsonProperty(PropertyName = "Amount of time it takes to sink - Phase 1 (seconds)")]
    public float SinkTime1 { get; set; }
    [JsonProperty(PropertyName = "Amount of time it takes to sink - Phase 2 (seconds)")]
    public float SinkTime2 { get; set; }
    [JsonProperty(PropertyName = "Amount of time it takes to sink - Phase 3 (seconds)")]
    public float SinkTime3 { get; set; }
    [JsonProperty(PropertyName = "Cargo ship despawn time once sunk (seconds)")]
    public int DespawnTime { get; set; }
    [JsonProperty(PropertyName = "Effect Settings")]
    public EffectSettings Effects { get; set; }
    public class EffectSettings
    {
        [JsonProperty(PropertyName = "Large explosion enabled")]
        public bool LargeExplosion { get; set; }
        [JsonProperty(PropertyName = "Napalm launchers enabled")]
        public bool NapalmLaunchers { get; set; }
        [JsonProperty(PropertyName = "Alarm sound on proximity enabled")]
        public bool AlarmProximity { get; set; }
        [JsonProperty(PropertyName = "Proximity distance to trigger effects")]
        public float ProximityDistance { get; set; }
    }

}

public class EffectSettings
{
    [JsonProperty(PropertyName = "Large explosion enabled")]
    public bool LargeExplosion { get; set; }
    [JsonProperty(PropertyName = "Napalm launchers enabled")]
    public bool NapalmLaunchers { get; set; }
    [JsonProperty(PropertyName = "Alarm sound on proximity enabled")]
    public bool AlarmProximity { get; set; }
    [JsonProperty(PropertyName = "Proximity distance to trigger effects")]
    public float ProximityDistance { get; set; }
}

public class IcebergSelection
{
    [JsonProperty(PropertyName = "Don't target icebergs that have player construction on them")]
    public bool DisableWithBases { get; set; }
    [JsonProperty(PropertyName = "Don't target icebergs that have tool cupboards on them")]
    public bool DisableWithCupboards { get; set; }
}

public class NPCSettings
{
    [JsonProperty(PropertyName = "Cargo Ship NPC Options")]
    public ShipNPCSettings ShipNPCs { get; set; }
    [JsonProperty(PropertyName = "Amount of NPCs to spawn on the iceberg")]
    public int AmountToSpawn { get; set; }
    [JsonProperty(PropertyName = "NPC Options")]
    public Oxide.Ext.ChaosNPC.NPCSettings Settings { get; set; }
    public class ShipNPCSettings
    {
        [JsonProperty(PropertyName = "Cargo Ship NPCs drop loot")]
        public bool ShipNPCDropLoot { get; set; }
        [JsonProperty(PropertyName = "Cargo Ship NPC kill mode ( KillInstantly, DieByDrowning )")]
        public KillMode Kill { get; set; }
        [JsonProperty(PropertyName = "Cargo Ship corpse mode ( NoCorpse, DropCorpse )")]
        public CorpseMode Corpse { get; set; }
    }

}

public class ShipNPCSettings
{
    [JsonProperty(PropertyName = "Cargo Ship NPCs drop loot")]
    public bool ShipNPCDropLoot { get; set; }
    [JsonProperty(PropertyName = "Cargo Ship NPC kill mode ( KillInstantly, DieByDrowning )")]
    public KillMode Kill { get; set; }
    [JsonProperty(PropertyName = "Cargo Ship corpse mode ( NoCorpse, DropCorpse )")]
    public CorpseMode Corpse { get; set; }
}

public class LootSettings
{
    [JsonProperty(PropertyName = "Cargo ship loot mode ( Despawn, LeaveOnShip, FloatToSurface )")]
    public ShipLootMode ShipLoot { get; set; }
    [JsonProperty(PropertyName = "Amount of mine-able debris to spawn")]
    public int GibsToSpawn { get; set; }
    [JsonProperty(PropertyName = "Debris spread amount")]
    public float GibsSpread { get; set; }
    [JsonProperty(PropertyName = "Iceberg loot crates")]
    public LootTable Iceberg { get; set; }
    [JsonProperty(PropertyName = "Floating loot crates")]
    public LootTable Floating { get; set; }
    public class LootTable
    {
        [JsonProperty(PropertyName = "Drop loot crates")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Amount of crates to spawn")]
        public int Amount { get; set; }
        [JsonProperty(PropertyName = "Use custom loot table")]
        public bool UseCustomLoot { get; set; }
        [JsonProperty(PropertyName = "Minimum number of items per crate")]
        public int Minimum { get; set; }
        [JsonProperty(PropertyName = "Maximum number of items per crate")]
        public int Maximum { get; set; }
        [JsonProperty(PropertyName = "Despawn time (seconds)")]
        public int DespawnTime { get; set; }
        public List<LootItem> Table { get; set; }
        public class LootItem
        {
            public string Shortname { get; set; }
            public int Minimum { get; set; }
            public int Maximum { get; set; }
            public ulong Skin { get; set; }
            [JsonProperty(PropertyName = "Minimum condition (0.0 - 1.0)")]
            public float MinCondition { get; set; }
            [JsonProperty(PropertyName = "Maximum condition (0.0 - 1.0)")]
            public float MaxCondition { get; set; }
            public float Probability { get; set; }
            public LootItem();
            public LootItem(string shortname, int minimum, int maximum, float probability);
            private int GetAmount();
            public void Create(ItemContainer container);
        }

        public IEnumerator SpawnInWater(Vector3 position, float radius);
        public IEnumerator SpawnOnIceberg(Iceberg iceberg);
        private LootContainer SpawnContainer(string prefabPath, Vector3 position, Quaternion rotation);
        private void PopulateLoot(LootContainer lootContainer);
    }

}

public class LootTable
{
    [JsonProperty(PropertyName = "Drop loot crates")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Amount of crates to spawn")]
    public int Amount { get; set; }
    [JsonProperty(PropertyName = "Use custom loot table")]
    public bool UseCustomLoot { get; set; }
    [JsonProperty(PropertyName = "Minimum number of items per crate")]
    public int Minimum { get; set; }
    [JsonProperty(PropertyName = "Maximum number of items per crate")]
    public int Maximum { get; set; }
    [JsonProperty(PropertyName = "Despawn time (seconds)")]
    public int DespawnTime { get; set; }
    public List<LootItem> Table { get; set; }
    public class LootItem
    {
        public string Shortname { get; set; }
        public int Minimum { get; set; }
        public int Maximum { get; set; }
        public ulong Skin { get; set; }
        [JsonProperty(PropertyName = "Minimum condition (0.0 - 1.0)")]
        public float MinCondition { get; set; }
        [JsonProperty(PropertyName = "Maximum condition (0.0 - 1.0)")]
        public float MaxCondition { get; set; }
        public float Probability { get; set; }
        public LootItem();
        public LootItem(string shortname, int minimum, int maximum, float probability);
        private int GetAmount();
        public void Create(ItemContainer container);
    }

    public IEnumerator SpawnInWater(Vector3 position, float radius);
    public IEnumerator SpawnOnIceberg(Iceberg iceberg);
    private LootContainer SpawnContainer(string prefabPath, Vector3 position, Quaternion rotation);
    private void PopulateLoot(LootContainer lootContainer);
}

public class LootItem
{
    public string Shortname { get; set; }
    public int Minimum { get; set; }
    public int Maximum { get; set; }
    public ulong Skin { get; set; }
    [JsonProperty(PropertyName = "Minimum condition (0.0 - 1.0)")]
    public float MinCondition { get; set; }
    [JsonProperty(PropertyName = "Maximum condition (0.0 - 1.0)")]
    public float MaxCondition { get; set; }
    public float Probability { get; set; }
    public LootItem();
    public LootItem(string shortname, int minimum, int maximum, float probability);
    private int GetAmount();
    public void Create(ItemContainer container);
}


```

---

## rustvk

```csharp
using System.Collections.Generic;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("VK STATUS", "Athreem", "0.0.1")]
[Description("Auto reload status vk = online server")]
public class Rustvk : RustPlugin
{
     string token;
     string message;
    public int count;
     void Init();
     vkupdate2();
     Puts(response );
}


```

---

## RustySheriffAlertSystem

```csharp
using System.Collections.Generic;
using System;
using System.Reflection;
using System.Data;
using UnityEngine;
using Oxide.Core;
using System.IO;
using Oxide.Core.Libraries;
using System.Linq;

Oxide.Plugins
[Info("RaidAlert", "By JV", "1.0.4")]
 class RustySheriffAlertSystem : RustPlugin
{
     List<BasePlayer> allBasePlayer;
     List<Auth> authList;
     List<DrawListEntry> drawList;
     List<IgnoreListEntry> ignoreList;
     List<ulong> muteList;
     List<Trespasser> newTrespassers;
     List<Trespasser> oldTrespassers;
     List<Perimeter> perimeters;
     List<TestAlert> testAlerts;
     List<ValReq> valReqs;
     List<ulong> validated;
     Dictionary<ulong, int> authDict;
     Dictionary<ulong, int> ignoreDict;
     Dictionary<ulong, int> perimeterDict;
    private static DateTime epoch;
     string url;
     double drawCheck;
     double nextCheck;
     double playerUpdateCheck;
     int maxPerimeterDelta;
     int maxPerimeterPoints;
     int maxPerimetersPerPlayer;
     int playerCheckIndex;
     int timeBetweenChecks;
     bool alertsVisibleInGame;
     bool alertTriggeredOnlyWhenSleepingInPerimeter;
     bool automaticCheckingEnabled;
     bool enabledForAuthorisedUsersOnly;
     bool sendAnon;
     bool syncWithRustySheriffServer;
     bool updatedOldData;
     bool useCupboards;
     bool adminsAuthed;
    private FieldInfo buildingPrivlidges;
     float perimeterHeightDelta;
     System.Random rnd;
    [ChatCommand("rs")]
     void cmdChatRS(BasePlayer player, string command, string[] args);
    [ConsoleCommand("rs.adduser")]
     void cmdRSAddUser(ConsoleSystem.Arg arg);
    [ConsoleCommand("rs.auths")]
     void cmdRSAuths(ConsoleSystem.Arg arg);
    [ConsoleCommand("rs.deluser")]
     void cmdRSDelUser(ConsoleSystem.Arg arg);
    [ConsoleCommand("rs.save")]
     void cmdRSSave(ConsoleSystem.Arg arg);
     void addPerimeter(BasePlayer player, string perimeterName);
     int addPerimeterEntry(BasePlayer player, bool stop);
     void addUser(BasePlayer initPlayer, string[] args);
     void addValidationRequest(ulong steamid, bool newCode);
     void cancelPerimeter(BasePlayer player);
     void checkAdminsAuthed();
     int checkDupes(string playerName);
     void checkTrespass();
     string cleanString(string msg);
     void clearIgnoreList(BasePlayer player);
     void clearPerimeters(BasePlayer player);
     void convertOldAlertsObj(OldAlertsObjectDict oldAlertsObj);
     void convertOldAuthObjs(OldAuthObjectDict oldAuthObj);
     void convertOldIgnoresObjs(OldIgnoresObjectDict oldIgnoresObj);
     void convertOldMuteList(OldMuteList oldMuteList);
     void convertOldValObjs(OldValidatedObjectDict oldValObj);
     int countPerimeters(ulong steamid);
     double currentTime();
     void deletePerimeter(BasePlayer player, string[] args);
     void delUser(BasePlayer player, string[] args);
     Point doFacesJoinTogether(List<Point> perimeterPoints, int facesIndex2, List<Face> faces);
     bool doFacesOverlap(int facesIndex, int facesIndex2, List<Face> faces);
     void drawFaces(BasePlayer player, List<Face> faces, double height);
     void drawPerimeterPoints(BasePlayer player, List<Point> perimeterPoints, double height);
     void FindAllFrom(BasePlayer player, string perimeterName);
     bool findTolerances(PerimeterPosition pPos);
     int getAuthIndex(ulong steamid);
     int getAuthLevel(ulong steamid);
     object GetConfig(string menu, string datavalue, object defaultValue);
     Euler GetEulerAngles(Quat q);
     int getIgnoreListIndex(ulong steamid);
     PerimeterPosition getInProgress(ulong steamid);
     int getPerimeterIndex(ulong steamid);
     PerimeterPosition getPerimeterIndex(ulong steamid, string name);
     int getPlayerIndex(ulong steamid);
     void hashAuths();
     void hashIgnores();
     void hashPerimeters();
     bool hasTotalAccess(BasePlayer player);
     void ignoreDetect(BasePlayer player);
     void ignorePlayer(BasePlayer player, string[] args);
     bool ignorePlayer(ulong baseSteamID, ulong otherPlayerSteamID, string otherPlayerName);
     bool isInProgress(ulong steamid);
     bool isPerimeterNameInUse(ulong steamid, string name);
     bool isPlayerIgnored(ulong steamid, ulong tpsteamid);
     bool isServerModerator(BasePlayer player);
     bool isValidated(ulong steamid);
     double isLeft(Vector3 p0, Vector3 p1, Vector3 p2);
     void LoadDataFiles();
    protected override void LoadDefaultConfig();
     void Loaded();
     void loadOldConfig();
     void LoadVariables();
     void muteIngameAlerts(BasePlayer player, bool mute);
     void OnPlayerDisconnected(BasePlayer player);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnServerInitialized();
     void OnServerSave();
     void OnTick();
     void parseWebReply(int code, string response);
     void plotPerimeter(BasePlayer player, int perimeterIndex, int playerPerimeterIndex);
     List<Point> removePointlessPoints(List<Point> perimeterPoints);
     void respondToValRequests(string response);
     void saveAllData(BasePlayer player);
     void saveConfiguration();
     void saveDataFiles();
     void sendTestAlert(BasePlayer player);
     void set(BasePlayer player, string[] args);
     void setAuthLevel(ulong steamid, int level, string name, BasePlayer initPlayer, bool quiet);
     void setCheckInterval(BasePlayer player, string[] args);
     void setMaxPerimeters(BasePlayer player, string[] args);
     void setMaxPoints(BasePlayer player, string[] args);
     void setMaxSize(BasePlayer player, string[] args);
     void setTimeBetweenChecks(BasePlayer player, string[] args);
     void showAdminMenu(BasePlayer player);
     void showAdvancedMenu(BasePlayer player);
     void showAuthMenu(BasePlayer player);
     void showAuthorisedUsers(BasePlayer player);
     void showHelpMenu(BasePlayer player);
     void showMainMenu(BasePlayer player);
     void showStatus(BasePlayer player);
     void sortAuths();
     void sortIgnores(int ignoreListIndex);
     void sortPlayerPerimeters(int perimeterIndex);
     void startDrawing(BasePlayer player, int perimeterIndex, int playerPerimeterIndex);
     void startPerimeter(BasePlayer player, string[] args);
     void stopDrawing(BasePlayer player);
     void stopPerimeter(BasePlayer player);
     void stopPerimeter(BasePlayer player, bool cancel);
     void testPerimeter(BasePlayer player);
     void toggleAnon(BasePlayer player);
     void toggleAutoCheck(BasePlayer player, bool enable);
     void toggleChatMute(BasePlayer player);
     void toggleSecureMode(BasePlayer player);
     void toggleSleepers(BasePlayer player);
     void toggleSync(BasePlayer player);
     void toggleUseCupboard(BasePlayer player);
     void validatePlayer(BasePlayer player, bool newCode);
     void undoLastPoint(BasePlayer player);
     void unignorePlayer(BasePlayer player, string[] args);
     void updateRSSServer();
     void viewIgnoreList(BasePlayer player);
     void viewPerimeters(BasePlayer player, string[] args);
     int windingPoly(Vector3 position, PerimeterPosition pPos);
    public class Auth
    {
        public ulong steamid;
        public string name;
        public int level;
    }

     class DrawListEntry
    {
        public BasePlayer basePlayer;
        public int perimeterIndex;
        public int playerPerimeterIndex;
    }

     class Euler
    {
        public double x;
        public double y;
        public double z;
    }

     class Face
    {
        public double sx;
        public double sy;
        public double ex;
        public double ey;
        public string name;
    }

     class IgnoredPlayer
    {
        public ulong steamid;
        public string name;
    }

     class IgnoreListEntry
    {
        public ulong steamid;
        public List<IgnoredPlayer> ignores;
    }

     class name
    {
        public string foundationName;
    }

     class OldAlert
    {
        public Dictionary<string, OldAlertDetails> alertName;
    }

     class OldAlertDetails
    {
        public Dictionary<string, Dictionary<string, double>> coords;
        public int count;
        public bool finished;
        public uint id;
        public double maxx;
        public double maxy;
        public double minx;
        public double miny;
        public bool SleeperPresent;
    }

     class OldAlertsObjectDict
    {
        public Dictionary<string, Dictionary<string, OldAlertDetails>> alerts;
    }

     class OldAuthObjectDict
    {
        public Dictionary<string, string> authNames;
        public Dictionary<string, string> authorised;
    }

     class OldIgnoresObjectDict
    {
        public Dictionary<string, Dictionary<string, string>> ignores;
    }

     class OldIgnoredPlayers
    {
        public Dictionary<string, string> ignoredPlayers;
    }

     class OldMuteList
    {
        public Dictionary<string, string> muted;
    }

     class OldValidatedObjectDict
    {
        public Dictionary<string, string> validated;
    }

    public class Perimeter
    {
        public ulong steamid;
        public List<PlayerPerimeter> playerPerimeters;
    }

     class PerimeterPosition
    {
        public int perimeterIndex;
        public int playerPerimeterIndex;
    }

    public class PlayerPerimeter
    {
        public string name;
        public bool finished;
        public uint id;
        public List<Vector3> coords;
        public double minX;
        public double minY;
        public double maxX;
        public double maxY;
        public bool sleeperPresent;
    }

     class Rect
    {
        public Point[] points;
    }

     class SaveObject
    {
        public List<ulong> validated;
        public List<Auth> authList;
        public List<ulong> muteList;
        public List<IgnoreListEntry> ignoreList;
    }

    public class SavePerimeterEntry
    {
        public ulong steamid;
        public string name;
        public bool finished;
        public uint id;
        public List<double> coordsX;
        public List<double> coordsY;
        public List<double> coordsZ;
        public double minX;
        public double minY;
        public double maxX;
        public double maxY;
        public bool sleeperPresent;
    }

    public class TestAlert
    {
        public ulong steamid;
        public string name;
    }

     class ValReq
    {
        public ulong steamid;
        public bool newCode;
    }

     class Validation
    {
        public ulong steamid;
        public string details;
    }

}

public class Auth
{
    public ulong steamid;
    public string name;
    public int level;
}

 class DrawListEntry
{
    public BasePlayer basePlayer;
    public int perimeterIndex;
    public int playerPerimeterIndex;
}

 class Euler
{
    public double x;
    public double y;
    public double z;
}

 class Face
{
    public double sx;
    public double sy;
    public double ex;
    public double ey;
    public string name;
}

 class IgnoredPlayer
{
    public ulong steamid;
    public string name;
}

 class IgnoreListEntry
{
    public ulong steamid;
    public List<IgnoredPlayer> ignores;
}

 class name
{
    public string foundationName;
}

 class OldAlert
{
    public Dictionary<string, OldAlertDetails> alertName;
}

 class OldAlertDetails
{
    public Dictionary<string, Dictionary<string, double>> coords;
    public int count;
    public bool finished;
    public uint id;
    public double maxx;
    public double maxy;
    public double minx;
    public double miny;
    public bool SleeperPresent;
}

 class OldAlertsObjectDict
{
    public Dictionary<string, Dictionary<string, OldAlertDetails>> alerts;
}

 class OldAuthObjectDict
{
    public Dictionary<string, string> authNames;
    public Dictionary<string, string> authorised;
}

 class OldIgnoresObjectDict
{
    public Dictionary<string, Dictionary<string, string>> ignores;
}

 class OldIgnoredPlayers
{
    public Dictionary<string, string> ignoredPlayers;
}

 class OldMuteList
{
    public Dictionary<string, string> muted;
}

 class OldValidatedObjectDict
{
    public Dictionary<string, string> validated;
}

public class Perimeter
{
    public ulong steamid;
    public List<PlayerPerimeter> playerPerimeters;
}

 class PerimeterPosition
{
    public int perimeterIndex;
    public int playerPerimeterIndex;
}

public class PlayerPerimeter
{
    public string name;
    public bool finished;
    public uint id;
    public List<Vector3> coords;
    public double minX;
    public double minY;
    public double maxX;
    public double maxY;
    public bool sleeperPresent;
}

 class Rect
{
    public Point[] points;
}

 class SaveObject
{
    public List<ulong> validated;
    public List<Auth> authList;
    public List<ulong> muteList;
    public List<IgnoreListEntry> ignoreList;
}

public class SavePerimeterEntry
{
    public ulong steamid;
    public string name;
    public bool finished;
    public uint id;
    public List<double> coordsX;
    public List<double> coordsY;
    public List<double> coordsZ;
    public double minX;
    public double minY;
    public double maxX;
    public double maxY;
    public bool sleeperPresent;
}

public class TestAlert
{
    public ulong steamid;
    public string name;
}

 class ValReq
{
    public ulong steamid;
    public bool newCode;
}

 class Validation
{
    public ulong steamid;
    public string details;
}


```

---

## SafeHomes

```csharp
using System;
using Rust;
using System.Reflection;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("SafeHomes", "Vypr/Phoenix", "1.0.0")]
 class SafeHomes : RustPlugin
{
    static DamageTypeList emptyDamage;
     bool BuildingPrivilege(BasePlayer player);
     void CancelDamage(HitInfo hitinfo);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
}


```

---

## Salvager

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Collections;

Oxide.Plugins
[Info("Salvager", "Deicide666ra", "1.0.2")]
 class Salvager : RustPlugin
{
     Dictionary<string, string> c_salvagers;
     string c_price;
     double c_refundRatio;
     Dictionary<ulong, string> g_playerCommands;
     Dictionary<string, ItemDefinition> g_itemDefinitions;
     Dictionary<string, ItemBlueprint> g_blueprintDefinitions;
     Dictionary<string, BasePlayer> g_lootCache;
     Dictionary<string, int> g_priceDict;
     bool g_configChanged;
     void Loaded();
    protected override void LoadDefaultConfig();
     void LoadConfigValues();
     object GetConfigValue(string category, string setting, object defaultValue);
     void SetConfigValue(string category, string setting, object newValue);
     void OnServerInitialized();
    [ChatCommand("salvager")]
     void cmdSalvager(BasePlayer player, string cmd, string[] args);
     void LinkContainer(BasePlayer player);
     void UnlinkContainer(BasePlayer player);
     void OnPlayerLoot(PlayerLoot lootInventory, object lootable);
     bool CheckPlayerInventoryForItems(BasePlayer player, string shortname, int amount);
     int RemoveItemsFromInventory(BasePlayer player, string shortname, int amount);
     string GetPriceString();
     bool HasPrice(BasePlayer player);
     void RunCommand(BasePlayer player, string command, BaseEntity container);
    private string GetActiveCommand(BasePlayer player);
    private void SetActiveCommand(BasePlayer player, string command);
    private void ClearActiveCommand(BasePlayer player);
    private ulong GetSalvagerOwner(BaseEntity container);
    private bool CreateSalvager(BaseEntity container, ulong steamIdOwner);
     void OnItemAddedToContainer(ItemContainer container, Item item);
     void SalvageItem(BasePlayer player, Item item);
     string UniqueId(BaseEntity entity);
     string FormatError(string message);
}


```

---

## SAMSiteAuth

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net;
using ProtoBuf;
using UnityEngine;

Oxide.Plugins
[Info("SAMSiteAuth", "Kektus", "1.0.1")]
[Description("Makes SAM Sites act in a similar fashion to shotgun traps and flame turrets.")]
public class SAMSiteAuth : RustPlugin
{
    private void Init();
    private void Unload();
    private void OnEntityBuilt(Planner plan, GameObject go);
    private static bool IsAuthed(BasePlayer player, BaseEntity entity);
    private static List<BasePlayer> Targets;
    public class SamController : MonoBehaviour
    {
        public SamSite entity;
        private void Awake();
        public void FixedUpdate();
    }

}

public class SamController : MonoBehaviour
{
    public SamSite entity;
    private void Awake();
    public void FixedUpdate();
}


```

---

## SaveMyMap

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using UnityEngine;
using IEnumerator = System.Collections.IEnumerator;

Oxide.Plugins
[Info("SaveMyMap", "Fujikura", "1.0.0", ResourceId = 2111)]
 class SaveMyMap : RustPlugin
{
     bool Changed;
     SaveRestore saveRestore;
     int Rounds;
     bool Initialized;
     string saveFolder;
     bool loadReload;
     string [] saveFolders;
     int saveInterval;
     int saveCustomAfter;
     bool callOnServerSave;
     float delayCallOnServerSave;
     bool saveAfterLoadFile;
     bool allowOutOfDateSaves;
     bool enableLoadOverride;
     int numberOfSaves;
     object GetConfig(string menu, string datavalue, object defaultValue);
     void LoadVariables();
     void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
     void Init();
     void Unload();
     void OnServerInitialized();
     void SaveLoop();
     void OnServerSave(object file);
     object OnSaveLoad(Dictionary<BaseEntity, ProtoBuf.Entity> dictionary);
    [ConsoleCommand("smm.save")]
     void cMapSave(ConsoleSystem.Arg arg);
    [ConsoleCommand("smm.loadmap")]
     void cLoadMap(ConsoleSystem.Arg arg);
    [ConsoleCommand("smm.loadfile")]
     void cLoadFile(ConsoleSystem.Arg arg);
     Int32 UnixTimeStampUTC();
     string [] SaveFolders();
     void SaveBackupCreate();
}


```

---

## SchedShutdown

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries;

Oxide.Plugins
[Info("SchedShutdown", "db_arcane", "1.1.2")]
public class SchedShutdown : RustPlugin
{
    static List<Oxide.Core.Libraries.Timer.TimerInstance> Timers;
     Oxide.Core.Libraries.Timer MainTimer;
     string TimeFormat;
     string EnabledStr;
     string DisabledStr;
     void Init();
    [ConsoleCommand("schedule.shutdown")]
    private void ScheduleShutdown(ConsoleSystem.Arg arg);
    [HookMethod("OnServerInitialized")]
     void myOnServerInitialized();
    [HookMethod("Unload")]
     void myUnload();
    [HookMethod("LoadDefaultConfig")]
     void myLoadDefaultConfig();
    private void ResetShutdownTimer();
    private void PrintShutdownStatus();
    private void CleanupConfig();
    private bool IsTimeValid(string timeString);
}


```

---

## ScientistLoot

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using System.Linq;
using Oxide.Core;

Oxide.Plugins
[Info("ScientistLoot", "SkiTles", "1.0")]
 class ScientistLoot : RustPlugin
{
    private bool Initialized;
     class NPCItem
    {
        public string shortname;
        public int minamount;
        public int maxamount;
        public bool bp;
        public int rarity;
    }

    private static ConfigFile config;
    private class ConfigFile
    {
        [JsonProperty(PropertyName = "Минимальное количество предметов")]
        public int minamount { get; set; }
        [JsonProperty(PropertyName = "Максимальное количество предметов")]
        public int maxamount { get; set; }
        [JsonProperty(PropertyName = "Сохранять шанс на выпадение оружия?")]
        public bool weaponsave { get; set; }
        [JsonProperty(PropertyName = "Список лута")]
        public List<NPCItem> npcitemlist { get; set; }
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void Regenerate();
    private void OnServerInitialized();
     void OnEntitySpawned(BaseNetworkable entity);
    private void ChangeLoot(LootableCorpse npccorpse);
}

 class NPCItem
{
    public string shortname;
    public int minamount;
    public int maxamount;
    public bool bp;
    public int rarity;
}

private class ConfigFile
{
    [JsonProperty(PropertyName = "Минимальное количество предметов")]
    public int minamount { get; set; }
    [JsonProperty(PropertyName = "Максимальное количество предметов")]
    public int maxamount { get; set; }
    [JsonProperty(PropertyName = "Сохранять шанс на выпадение оружия?")]
    public bool weaponsave { get; set; }
    [JsonProperty(PropertyName = "Список лута")]
    public List<NPCItem> npcitemlist { get; set; }
    public static ConfigFile DefaultConfig();
}


```

---

## Scoreboards

```csharp
using System.Collections.Generic;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Linq;
using System;

Oxide.Plugins
[Info("Scoreboards", "LaserHydra", "1.0.0", ResourceId = 0)]
[Description("Allows you to create Scoreboards and plugins to insert data")]
 class Scoreboards : RustPlugin
{
    static Scoreboards Instance;
    static ScoreboardData Data;
    static string AnchorMin;
    static string AnchorMax;
    static string BackgroundColor;
    static string HeaderColor;
    static string TitleColor;
    static string ContentColor;
    public class ScoreboardData
    {
        public string ActiveScoreboard;
        public List<Scoreboard> All;
    }

    public class Scoreboard
    {
        public static Scoreboard Find(string Title);
        public string Title;
        public string Description;
        internal bool Active { get; set; }
        internal KeyValuePair<string, string>[] Entries;
        public override int GetHashCode();
        public void Remove();
        public void SetActive();
        public void SetEntries(KeyValuePair<string, string>[] Entries);
        public static void SetActiveScoreboard(Scoreboard scoreboard);
        public static Scoreboard GetActiveScoreboard();
        public static void AddScoreboard(Scoreboard scoreboard);
    }

     void UpdateScoreboard(string ScoreboardTitle, KeyValuePair<string, string>[] Entries);
     void CreateScoreboard(string ScoreboardTitle, string ScoreboardDescription, KeyValuePair<string, string>[] Entries);
     void RemoveScoreboard(string ScoreboardTitle);
     void ScoreboardUpdated(Scoreboard scoreboard);
     void ActiveScoreboardChanged(Scoreboard scoreboard);
     void UpdateScoreboardUI();
     void DrawScoreboardUI(BasePlayer player);
    static CuiElementContainer GetCUI();
    [ChatCommand("scoreboard")]
     void cmdScoreboard(BasePlayer player, string cmd, string[] args);
     void Loaded();
     void Unloaded();
     void OnPlayerSleepEnded(BasePlayer player);
     void LoadMessages();
    new void LoadConfig();
    protected override void LoadDefaultConfig();
     string LangMsg(string key, object id, string[] replacements);
     DataValue ReadData(DataValue data, string filename);
     void WriteData(DataValue data, string filename);
     ConfigValue UpdateConfig(object[] fullPath);
}

public class ScoreboardData
{
    public string ActiveScoreboard;
    public List<Scoreboard> All;
}

public class Scoreboard
{
    public static Scoreboard Find(string Title);
    public string Title;
    public string Description;
    internal bool Active { get; set; }
    internal KeyValuePair<string, string>[] Entries;
    public override int GetHashCode();
    public void Remove();
    public void SetActive();
    public void SetEntries(KeyValuePair<string, string>[] Entries);
    public static void SetActiveScoreboard(Scoreboard scoreboard);
    public static Scoreboard GetActiveScoreboard();
    public static void AddScoreboard(Scoreboard scoreboard);
}


```

---

## ScrapVendingExchange

```csharp
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Scrap Vending Exchange", "Sempai#3239", "1.0.5")]
[Description("Vending Exchange, exchange scrap for custom currency via any npc vending machine.")]
public class ScrapVendingExchange : RustPlugin
{
    private const string PERM_USE;
    private const string GRANTED_PREFAB;
    private const string DENIED_PREFAB;
    private Effect _effectInstance;
    private PluginConfig _config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private class PluginConfig
    {
        [JsonProperty("Item to sell item id")]
        public int SellItemId;
        [JsonProperty("Item to sell skin id")]
        public ulong SellSkinId;
        [JsonProperty("Min amount to be exchanged")]
        public int MinAmount;
        [JsonProperty("Max amount to be exchanged")]
        public int MaxAmount;
        [JsonProperty("Exchange rate (default payout 80% percent)")]
        public double ExchangeRate;
        [JsonProperty("Deposit command (default Economics arguments {userid} {amount})")]
        public string DepositCommand;
        [JsonProperty("Allowed Machines (allowed prefab names, leave empty for all)")]
        public string[] AllowedMachines;
        [JsonProperty("Scrap Ui Position (sets position of the scrap vending ui)")]
        public string[] ScrapUiPosition;
        [JsonProperty("Popup Ui Position (sets position of the popup vending ui)")]
        public string[] PopupUiPosition;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
        public static PluginConfig DefaultConfig();
    }

    private const string MESSAGE_INVALID;
    private const string MESSAGE_ENOUGH;
    private const string MESSAGE_TITLE;
    private const string MESSAGE_AMOUNT;
    private const string MESSAGE_EXCHANGED;
    private const string BUTTON_EXCHANGE;
    private const string BUTTON_CLEAR;
    protected override void LoadDefaultMessages();
    private string GetMessage(string key, string userid, object[] args);
    private void OnServerInitialized();
    private void Unload();
    private void OnVendingShopOpened(NPCVendingMachine machine, BasePlayer player);
    private void OnLootEntityEnd(BasePlayer player, NPCVendingMachine machine);
    private const string MAIN_FONT_COLOR;
    private const string GREEN_BUTTON_COLOR;
    private const string GREEN_BUTTON_FONT_COLOR;
    private const string GRAY_BUTTON_COLOR;
    private const string GRAY_BUTTON_FONT_COLOR;
    private const string INPUT_FONT_COLOR;
    private const string INPUT_COLOR;
    private const string POPUP_FONT_COLOR;
    private const string POPUP_ERROR_COLOR;
    private const string POPUP_SUCCESS_COLOR;
    private const string EXCHANGE_NAME;
    private const string EXCHANGE_POPUP_NAME;
    private const string EXCHANGE_HEADER_NAME;
    private const string EXCHANGE_CONTENT_NAME;
    private void CreateUI(BasePlayer player, int amount, bool isFirst);
    private void RemoveUI(BasePlayer player);
    private class UiHelpers
    {
        public static CuiPanel Panel(string panelColor, string anchorMin, string anchorMax);
        public static CuiLabel Label(string textColor, string anchorMin, string anchorMax, string text, TextAnchor textAnchor);
        public static CuiElement Input(string textColor, string anchorMin, string anchorMax, string command, string parent, TextAnchor textAnchor);
        public static CuiButton Button(string color, string textColor, string anchorMin, string anchorMax, string text, string command, TextAnchor textAnchor);
        public static void Popup(BasePlayer player, string message, string panelColor, string textColor, string anchorMin, string anchorMax);
    }

    private void CurrencyCommand(BasePlayer player, double amount);
    private bool HasAmount(BasePlayer player, int amount);
    private void TakeAmount(BasePlayer player, int amount);
    private int GetAmount(PlayerInventory inventory, int itemid, ulong skinID);
    private int GetAmount(ItemContainer container, int itemid, ulong skinID, bool usable);
    private int TakeAmount(PlayerInventory inventory, int itemid, ulong skinID, int amount);
    private int TakeAmount(ItemContainer container, int itemid, int amount, ulong skinID);
    [ConsoleCommand("vendingscrapexchange.set")]
    private void AmountCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("vendingscrapexchange.scrap")]
    private void ScrapCommand(ConsoleSystem.Arg arg);
    private bool HasPermission(BasePlayer player);
    private bool IsAllowedMachine(string prefab);
    private bool IsLootingMachine(BasePlayer player);
    private void PlayEffect(BasePlayer player, string prefabPath);
}

private class PluginConfig
{
    [JsonProperty("Item to sell item id")]
    public int SellItemId;
    [JsonProperty("Item to sell skin id")]
    public ulong SellSkinId;
    [JsonProperty("Min amount to be exchanged")]
    public int MinAmount;
    [JsonProperty("Max amount to be exchanged")]
    public int MaxAmount;
    [JsonProperty("Exchange rate (default payout 80% percent)")]
    public double ExchangeRate;
    [JsonProperty("Deposit command (default Economics arguments {userid} {amount})")]
    public string DepositCommand;
    [JsonProperty("Allowed Machines (allowed prefab names, leave empty for all)")]
    public string[] AllowedMachines;
    [JsonProperty("Scrap Ui Position (sets position of the scrap vending ui)")]
    public string[] ScrapUiPosition;
    [JsonProperty("Popup Ui Position (sets position of the popup vending ui)")]
    public string[] PopupUiPosition;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
    public static PluginConfig DefaultConfig();
}

private class UiHelpers
{
    public static CuiPanel Panel(string panelColor, string anchorMin, string anchorMax);
    public static CuiLabel Label(string textColor, string anchorMin, string anchorMax, string text, TextAnchor textAnchor);
    public static CuiElement Input(string textColor, string anchorMin, string anchorMax, string command, string parent, TextAnchor textAnchor);
    public static CuiButton Button(string color, string textColor, string anchorMin, string anchorMax, string text, string command, TextAnchor textAnchor);
    public static void Popup(BasePlayer player, string message, string panelColor, string textColor, string anchorMin, string anchorMax);
}


```

---

## ScubaSteve

```csharp
using Oxide.Core;
using System.Collections.Generic;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using System;

Oxide.Plugins
[Info("Scuba Steve", "DaBludger", 1.6, ResourceId = 0)]
[Description("Be the SEAL, this will protect you from drowning and cold damage while swimming.")]
public class ScubaSteve : RustPlugin
{
    private bool Changed;
    private bool damageArmour;
    private bool configloaded;
    private float armourDamageAmount;
    private float head;
    private float chest;
    private float pants;
    private float gloves;
    private float boots;
    private float chead;
    private float cchest;
    private float cpants;
    private float cgloves;
    private float cboots;
     void OnPluginLoaded(Plugin name);
    protected override void LoadDefaultConfig();
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitinfo);
    private float getScaledDamage(float current, float deduction);
    private float getDamageDeduction(BasePlayer player, Rust.DamageType damageType);
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private void LoadVariables();
}


```

---

## SecureAdmin

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Rust;
using Oxide.Core.Plugins;
using Oxide.Core;

Oxide.Plugins
[Info("SecureAdmin", "OwnProx", 0.2, ResourceId = 13661)]
[Description("Secure Admins.")]
public class SecureAdmin : RustPlugin
{
    private Dictionary<ulong, float> Bans;
    private System.Timers.Timer timer;
    private DateTime NowTimePlease;
    private List<ulong> IdsToRemove;
    [ChatCommand("spectate")]
    private void SpectateChatCmd(BasePlayer player, string command, string[] args);
    [ChatCommand("tempban")]
    private void TempBanPlayer(BasePlayer player, string command, string[] args);
    [ChatCommand("ban")]
    private void BanPlayer(BasePlayer player, string command, string[] args);
    [ChatCommand("kick")]
    private void KickPlayer(BasePlayer player, string command, string[] args);
    [ChatCommand("say")]
    private void SayPlayer(BasePlayer player, string command, string[] args);
    [ChatCommand("permission")]
    private void EditPlayerPermission(BasePlayer player, string command, string[] args);
    [HookMethod("OnPlayerInit")]
    private void OnPlayerInit(BasePlayer player);
    [HookMethod("OnRunCommand")]
    private object OnRunCommand(ConsoleSystem.Arg arg);
    [HookMethod("Loaded")

    private void Loaded();
    [HookMethod("Unload")]
    private void Unload();
    private double GetTimestamp();
    private void EveryTenHours(object sender, System.Timers.ElapsedEventArgs e);
    private bool HandlePermission(string name, string userID, string Permission);
}


```

---

## SecurityCameras

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Linq;
using Rust;
using Facepunch;
using Network;

Oxide.Plugins
[Info("SecurityCameras", "k1lly0u", "0.2.28")]
[Description("Deploy security cameras around your base and view them by accessing a RustNET terminal")]
 class SecurityCameras : RustPlugin
{
    private StoredData storedData;
    private DynamicConfigFile data;
    private static SecurityCameras ins;
    private static LinkManager linkManager;
    private static int layerPlcmnt;
    private List<CameraManager> cameraManagers;
    private bool wipeDetected;
    private bool isInitialized;
    const string permUse;
    const string permIgnore;
    const string permPublic;
    const string SCUI_Overlay;
    const string CHAIR_PREFAB;
    const int CAMERA_ID;
    private void Loaded();
    private void OnServerInitialized();
    private void OnPlayerDisconnected(BasePlayer player);
    private object OnPlayerCommand(ConsoleSystem.Arg arg);
    private object OnPlayerTick(BasePlayer player, PlayerTick msg, bool wasPlayerStalled);
    private void OnEntityKill(BaseNetworkable networkable);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnNewSave(string filename);
    private void OnServerSave();
    private object CanDismountEntity(BasePlayer player, BaseMountable mountable);
    private void Unload();
    private bool CanPlaceItem(BasePlayer player, bool ignoreRestriction);
    private void InitializeAllLinks();
    private CameraManager InitializeCamera(CameraManager.CameraData cameraData, int terminalId);
    private void InitializeCamera(BasePlayer player, DroppedItem droppedItem);
    private void InitializeCamera(BasePlayer player, DroppedItem droppedItem, int terminalId);
    private int GetMaxCameras(ulong playerId);
    private void DestroyAllLinks();
    private void OnTerminalCreated(RustNET.TerminalManager terminal);
    private void OnTerminalRemoved(int terminalId);
    private void OnLinkShutdown(int terminalId);
    private void OnLinkDestroyed(int terminalId);
    private CameraManager[] GetAvailableCameras(int terminalId);
    private void InitializeController(BasePlayer player, uint managerId, int terminalId);
    private string GetHelpString(ulong playerId, bool title);
    private bool AllowPublicAccess();
    private class LinkManager
    {
        public List<CameraLink> links;
        public CameraLink GetLinkOf(BuildingManager.Building building);
        public CameraLink GetLinkOf(CameraManager camera);
        public CameraLink GetLinkOf(Controller controller);
        public CameraLink GetLinkOf(int terminalId);
        public CameraLink GetLinkOf(DroppedItem camera);
        public void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
        public void OnEntityDeath(BaseNetworkable networkable);
        public void DestroyAllLinks(bool shutdown);
        public class CameraLink
        {
            public int terminalId { get; set; }
            public BuildingManager.Building building { get; set; }
            public List<Controller> controllers { get; set; }
            public List<CameraManager> managers { get; set; }
            public CameraLink();
            public CameraLink(int terminalId, BuildingManager.Building building, CameraManager camera);
            public CameraLink(int terminalId, BuildingManager.Building building, CameraManager.CameraData cameraData);
            public void AddCameraToLink(CameraManager camera);
            public void AddCameraToLink(CameraManager.CameraData cameraData);
            public void InitiateLink(BasePlayer player, uint managerId);
            public void CloseLink(Controller controller, bool isDead);
            public void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
            public void OnEntityDeath(BaseNetworkable networkable);
            public void OnLinkTerminated(bool isDestroyed);
            public void OnCameraDestroyed(CameraManager manager);
            public void DestroyCameraManagers();
        }

    }

     class CameraManager : MonoBehaviour
    {
        public DroppedItem camera { get; set; }
        public Controller controller { get; set; }
        public BuildingBlock parent { get; set; }
        public Vector3 baseRotation { get; set; }
        public int terminalId { get; set; }
        public string cameraName { get; set; }
        private void Awake();
        private void OnDestroy();
        public void Destroy();
        public void SetRotation(float[] rotation);
        public void SetController(Controller controller);
        public CameraData GetCameraData();
        public class CameraData
        {
            public float[] position;
            public float[] baseRotation;
            public string cameraName;
            public int terminalId;
            public CameraData();
            public CameraData(CameraManager camera);
        }

    }

     class Controller : RustNET.Controller
    {
        public CameraManager manager { get; set; }
        public BaseMountable mountPoint { get; set; }
        private LinkManager.CameraLink link;
        private int spectateIndex;
        private bool switchingTargets;
        private bool canCyle;
        public override void Awake();
        public override void OnDestroy();
        private void Update();
        public void SetCameraLink(LinkManager.CameraLink link);
        public void BeginSpectating();
        public void FinishSpectating(bool isDead);
        public void SetSpectateTarget(int spectateIndex);
        private void UpdateNetworkGroup();
        public void UpdateSpectateTarget(int index);
        public override void OnPlayerDeath(HitInfo info);
        private void CreateCameraOverlay();
    }

    private void CreateConsoleWindow(BasePlayer player, int terminalId, int page);
    [ConsoleCommand("securitycameras.control")]
    private void ccmdControl(ConsoleSystem.Arg arg);
    [ChatCommand("sc")]
    private void cmdSC(BasePlayer player, string command, string[] args);
    private void LoadDefaultImages(int attempts);
    private void AddImage(string imageName, string fileName);
    private string GetImage(string name);
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Camera Options")]
        public CameraOptions Camera { get; set; }
        public class CameraOptions
        {
            [JsonProperty(PropertyName = "Allow players to cycle through all linked cameras")]
            public bool CanCycle { get; set; }
            [JsonProperty(PropertyName = "Allow friends and clan members to place/remove cameras")]
            public bool Friends { get; set; }
            [JsonProperty(PropertyName = "Require building privilege to place/remove cameras")]
            public bool RequirePrivilege { get; set; }
            [JsonProperty(PropertyName = "Maximum allowed cameras per base (Permission | Amount)")]
            public Dictionary<string, int> Max { get; set; }
            [JsonProperty(PropertyName = "Camera placement and removal distance")]
            public int Distance { get; set; }
            [JsonProperty(PropertyName = "Display camera overlay UI")]
            public bool Overlay { get; set; }
            [JsonProperty(PropertyName = "Camera overlay image URL")]
            public string OverlayImage { get; set; }
            [JsonProperty(PropertyName = "Camera icon URL for RustNET menu")]
            public string RustNETIcon { get; set; }
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private void SaveData();
    private void LoadData();
    private class StoredData
    {
        public CameraManager.CameraData[] cameraData;
    }

     string msg(string key, ulong playerId);
     Dictionary<string, string> Messages;
}

private class LinkManager
{
    public List<CameraLink> links;
    public CameraLink GetLinkOf(BuildingManager.Building building);
    public CameraLink GetLinkOf(CameraManager camera);
    public CameraLink GetLinkOf(Controller controller);
    public CameraLink GetLinkOf(int terminalId);
    public CameraLink GetLinkOf(DroppedItem camera);
    public void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    public void OnEntityDeath(BaseNetworkable networkable);
    public void DestroyAllLinks(bool shutdown);
    public class CameraLink
    {
        public int terminalId { get; set; }
        public BuildingManager.Building building { get; set; }
        public List<Controller> controllers { get; set; }
        public List<CameraManager> managers { get; set; }
        public CameraLink();
        public CameraLink(int terminalId, BuildingManager.Building building, CameraManager camera);
        public CameraLink(int terminalId, BuildingManager.Building building, CameraManager.CameraData cameraData);
        public void AddCameraToLink(CameraManager camera);
        public void AddCameraToLink(CameraManager.CameraData cameraData);
        public void InitiateLink(BasePlayer player, uint managerId);
        public void CloseLink(Controller controller, bool isDead);
        public void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
        public void OnEntityDeath(BaseNetworkable networkable);
        public void OnLinkTerminated(bool isDestroyed);
        public void OnCameraDestroyed(CameraManager manager);
        public void DestroyCameraManagers();
    }

}

public class CameraLink
{
    public int terminalId { get; set; }
    public BuildingManager.Building building { get; set; }
    public List<Controller> controllers { get; set; }
    public List<CameraManager> managers { get; set; }
    public CameraLink();
    public CameraLink(int terminalId, BuildingManager.Building building, CameraManager camera);
    public CameraLink(int terminalId, BuildingManager.Building building, CameraManager.CameraData cameraData);
    public void AddCameraToLink(CameraManager camera);
    public void AddCameraToLink(CameraManager.CameraData cameraData);
    public void InitiateLink(BasePlayer player, uint managerId);
    public void CloseLink(Controller controller, bool isDead);
    public void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    public void OnEntityDeath(BaseNetworkable networkable);
    public void OnLinkTerminated(bool isDestroyed);
    public void OnCameraDestroyed(CameraManager manager);
    public void DestroyCameraManagers();
}

 class CameraManager : MonoBehaviour
{
    public DroppedItem camera { get; set; }
    public Controller controller { get; set; }
    public BuildingBlock parent { get; set; }
    public Vector3 baseRotation { get; set; }
    public int terminalId { get; set; }
    public string cameraName { get; set; }
    private void Awake();
    private void OnDestroy();
    public void Destroy();
    public void SetRotation(float[] rotation);
    public void SetController(Controller controller);
    public CameraData GetCameraData();
    public class CameraData
    {
        public float[] position;
        public float[] baseRotation;
        public string cameraName;
        public int terminalId;
        public CameraData();
        public CameraData(CameraManager camera);
    }

}

public class CameraData
{
    public float[] position;
    public float[] baseRotation;
    public string cameraName;
    public int terminalId;
    public CameraData();
    public CameraData(CameraManager camera);
}

 class Controller : RustNET.Controller
{
    public CameraManager manager { get; set; }
    public BaseMountable mountPoint { get; set; }
    private LinkManager.CameraLink link;
    private int spectateIndex;
    private bool switchingTargets;
    private bool canCyle;
    public override void Awake();
    public override void OnDestroy();
    private void Update();
    public void SetCameraLink(LinkManager.CameraLink link);
    public void BeginSpectating();
    public void FinishSpectating(bool isDead);
    public void SetSpectateTarget(int spectateIndex);
    private void UpdateNetworkGroup();
    public void UpdateSpectateTarget(int index);
    public override void OnPlayerDeath(HitInfo info);
    private void CreateCameraOverlay();
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Camera Options")]
    public CameraOptions Camera { get; set; }
    public class CameraOptions
    {
        [JsonProperty(PropertyName = "Allow players to cycle through all linked cameras")]
        public bool CanCycle { get; set; }
        [JsonProperty(PropertyName = "Allow friends and clan members to place/remove cameras")]
        public bool Friends { get; set; }
        [JsonProperty(PropertyName = "Require building privilege to place/remove cameras")]
        public bool RequirePrivilege { get; set; }
        [JsonProperty(PropertyName = "Maximum allowed cameras per base (Permission | Amount)")]
        public Dictionary<string, int> Max { get; set; }
        [JsonProperty(PropertyName = "Camera placement and removal distance")]
        public int Distance { get; set; }
        [JsonProperty(PropertyName = "Display camera overlay UI")]
        public bool Overlay { get; set; }
        [JsonProperty(PropertyName = "Camera overlay image URL")]
        public string OverlayImage { get; set; }
        [JsonProperty(PropertyName = "Camera icon URL for RustNET menu")]
        public string RustNETIcon { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class CameraOptions
{
    [JsonProperty(PropertyName = "Allow players to cycle through all linked cameras")]
    public bool CanCycle { get; set; }
    [JsonProperty(PropertyName = "Allow friends and clan members to place/remove cameras")]
    public bool Friends { get; set; }
    [JsonProperty(PropertyName = "Require building privilege to place/remove cameras")]
    public bool RequirePrivilege { get; set; }
    [JsonProperty(PropertyName = "Maximum allowed cameras per base (Permission | Amount)")]
    public Dictionary<string, int> Max { get; set; }
    [JsonProperty(PropertyName = "Camera placement and removal distance")]
    public int Distance { get; set; }
    [JsonProperty(PropertyName = "Display camera overlay UI")]
    public bool Overlay { get; set; }
    [JsonProperty(PropertyName = "Camera overlay image URL")]
    public string OverlayImage { get; set; }
    [JsonProperty(PropertyName = "Camera icon URL for RustNET menu")]
    public string RustNETIcon { get; set; }
}

private class StoredData
{
    public CameraManager.CameraData[] cameraData;
}


```

---

## SeedEvent

```csharp
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("SeedEvent", "Own3r/Nericai", "1.1.0")]
[Description("Ивент позволяет выращивать камни на грядках")]
 class SeedEvent : RustPlugin
{
    private class Random
    {
        [JsonProperty("Название ресурса")]
        public string DisplayName;
        [JsonProperty("Название объекта (не менять)")]
        public string Prefabs;
        [JsonProperty("Используется в рандоме")]
        public bool Active;
    }

    private class PlantSeedConfig
    {
        [JsonProperty("Время прорастания семечки")]
        public int TimeToUp;
        [JsonProperty("Список выращиваемых предметов")]
        public List<Random> RandomList;
        [JsonProperty("Skin семечки")]
        public ulong SkinIdPlant;
        [JsonProperty("Максимальное ХП камня (Стандартное 1)")]
        public float startHealth;
    }

    private PlantSeedConfig config;
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
    private void SendInfo(BasePlayer player, string message);
     void OnServerInitialized();
     void OnEntityBuilt(Planner plan, GameObject go);
    private void GiveSeeds(BasePlayer player);
    private string GetRandomOre();
    private void StartTimerToThis(OreResourceEntity ore);
    private void Iterac(OreResourceEntity ore, StagedResourceEntity stageComponent, BaseCombatEntity healthComponent, ResourceDispenser resComponent);
    private void UpdateVisible(StagedResourceEntity stageComponent);
     void UnsubscribeSplit();
     void SubscribeSplit();
     object OnItemSplit(Item thisI, int split_Amount);
     object CanStackItem(Item thisI, Item item);
    [ConsoleCommand("seedevent")]
    private void cmdChange(ConsoleSystem.Arg args);
}

private class Random
{
    [JsonProperty("Название ресурса")]
    public string DisplayName;
    [JsonProperty("Название объекта (не менять)")]
    public string Prefabs;
    [JsonProperty("Используется в рандоме")]
    public bool Active;
}

private class PlantSeedConfig
{
    [JsonProperty("Время прорастания семечки")]
    public int TimeToUp;
    [JsonProperty("Список выращиваемых предметов")]
    public List<Random> RandomList;
    [JsonProperty("Skin семечки")]
    public ulong SkinIdPlant;
    [JsonProperty("Максимальное ХП камня (Стандартное 1)")]
    public float startHealth;
}


```

---

## SeedOre

```csharp
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("SeedOre", "LAGZYA", "1.0.6")]
public class SeedOre : RustPlugin
{
    private ConfigData cfg { get; set; }
    private class ConfigData
    {
        [JsonProperty("Скин айди семечки")]
        public ulong skinId;
        [JsonProperty("Время роста одной стадии")]
        public int cd;
        [JsonProperty("Шанс выпадение руды при добыче")]
        public int random;
        [JsonProperty("Рейты добычи?")]
        public float xd;
        [JsonProperty("Плавить ресурсы при добыче?")]
        public bool cook;
        [JsonProperty("Разрешить ставить ток в грядке??")]
        public bool planted;
        [JsonProperty("Список руд. Которые могут появится.")]
        public List<string> itemList;
        public static ConfigData GetNewConf();
    }

    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
     List<uint> _oreList;
     object OnDispenserGather(ResourceDispenser dispenser, BasePlayer player, Item item);
     object OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     void Unload();
    [PluginReference]
    private Plugin StacksExtended;
    private Plugin CustomSkinsStacksFix;
    private Item OnItemSplit(Item item, int amount);
    private object CanCombineDroppedItem(WorldItem first, WorldItem second);
     object CanStackItem(Item item, Item targetItem);
     List<uint> oreList;
     IEnumerator LoadData();
    private Coroutine start;
    private void OnServerInitialized();
     void Init();
    public static SeedOre ins;
     class ResourceSeed : MonoBehaviour
    {
        private ResourceEntity ore;
        private uint netId;
        private float health;
        private BaseEntity parent;
        private void Awake();
        public void OnDestroy();
         void UpdateStage();
    }

     object CanBuild(Planner planner, Construction prefan, Construction.Target target);
     void OnEntitySpawned(GrowableEntity entity);
    private void ReplySend(BasePlayer player, string message);
    [ChatCommand("giveseedore")]
     void Update(BasePlayer player);
}

private class ConfigData
{
    [JsonProperty("Скин айди семечки")]
    public ulong skinId;
    [JsonProperty("Время роста одной стадии")]
    public int cd;
    [JsonProperty("Шанс выпадение руды при добыче")]
    public int random;
    [JsonProperty("Рейты добычи?")]
    public float xd;
    [JsonProperty("Плавить ресурсы при добыче?")]
    public bool cook;
    [JsonProperty("Разрешить ставить ток в грядке??")]
    public bool planted;
    [JsonProperty("Список руд. Которые могут появится.")]
    public List<string> itemList;
    public static ConfigData GetNewConf();
}

 class ResourceSeed : MonoBehaviour
{
    private ResourceEntity ore;
    private uint netId;
    private float health;
    private BaseEntity parent;
    private void Awake();
    public void OnDestroy();
     void UpdateStage();
}


```

---

## SentryTurrets

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("Sentry Turrets", "Rust", "2.2.4")]
[Description("Leak By SparK")]
public class SentryTurrets : RustPlugin
{
    private const ulong skinID;
    private const string prefabSentry;
    private const string itemName;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnEntityBuilt(Planner plan, GameObject go);
    private object CanPickupEntity(BasePlayer player, NPCAutoTurret entity);
    private object OnTurretTarget(NPCAutoTurret turret, BasePlayer player);
    private void cmdGiveConsole(ConsoleSystem.Arg arg);
    [ChatCommand("t")]
    private void cmdToggleChat(BasePlayer player);
    private object CheckPickup(BasePlayer player, NPCAutoTurret entity);
    private void CheckPlacement(Planner plan, GameObject go);
    public bool CanAcceptItem(Item item, int targetSlot);
    private void SetupTurret(NPCAutoTurret turret);
    private void CheckExistingTurrets();
    private void SetupProtection(BaseCombatEntity turret);
    private void AddComponent(BaseNetworkable entity);
    private static Item CreateItem();
    private static void GiveItem(Vector3 position);
    private void GiveItem(BasePlayer player);
    private BasePlayer FindPlayer(string nameOrID);
    private bool CanShootBullet(NPCAutoTurret nPCAutoTurret, BasePlayer player);
    private NPCAutoTurret GetLookEntity(BasePlayer player);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Can get damage")]
        public bool getDamage;
        [JsonProperty(PropertyName = "Amount of ammo for one spray (set to 0 for no-ammo mode)")]
        public int spray;
        [JsonProperty(PropertyName = "Range (normal turret - 30")]
        public int range;
        [JsonProperty(PropertyName = "Give back on ground missing")]
        public bool itemOnGroundMissing;
        [JsonProperty(PropertyName = "Health (normal turret - 1000)")]
        public float health;
        [JsonProperty(PropertyName = "Aim cone (normal turret - 4)")]
        public float aimCone;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class CheckGround : MonoBehaviour
    {
        private BaseEntity entity;
        private void Start();
        private void Check();
        private void GroundMissing();
    }

}

private class ConfigData
{
    [JsonProperty(PropertyName = "Can get damage")]
    public bool getDamage;
    [JsonProperty(PropertyName = "Amount of ammo for one spray (set to 0 for no-ammo mode)")]
    public int spray;
    [JsonProperty(PropertyName = "Range (normal turret - 30")]
    public int range;
    [JsonProperty(PropertyName = "Give back on ground missing")]
    public bool itemOnGroundMissing;
    [JsonProperty(PropertyName = "Health (normal turret - 1000)")]
    public float health;
    [JsonProperty(PropertyName = "Aim cone (normal turret - 4)")]
    public float aimCone;
}

private class CheckGround : MonoBehaviour
{
    private BaseEntity entity;
    private void Start();
    private void Check();
    private void GroundMissing();
}


```

---

## ServerPop

```csharp
using ConVar;
using Network;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Libraries;
using System;
using System.Collections.Generic;
using System.Text;
using UnityEngine;

Oxide.Plugins
[Info("Server Pop", "Mabel", "1.1.3")]
[Description("Show server pop in chat with !pop trigger.")]
public class ServerPop : RustPlugin
{
    static Configuration config;
    static Dictionary<ulong, DateTime> cooldowns;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Cooldown Settings")]
        public CooldownSettings CooldownSettings { get; set; }
        [JsonProperty(PropertyName = "Chat Settings")]
        public ChatSettings ChatSettings { get; set; }
        [JsonProperty(PropertyName = "Messgae Settings")]
        public MessageSettings MessageSettings { get; set; }
        [JsonProperty(PropertyName = "Response Settings")]
        public ResponseSettings ResponseSettings { get; set; }
        [JsonProperty(PropertyName = "Connect Settings")]
        public ConnectSettings ConnectSettings { get; set; }
        [JsonProperty(PropertyName = "Wipe Response Settings")]
        public WipeSettings WipeSettings { get; set; }
        [JsonProperty(PropertyName = "Discord Response Settings")]
        public DiscordSettings DiscordSettings { get; set; }
        public Core.VersionNumber Version { get; set; }
    }

    public class CooldownSettings
    {
        [JsonProperty(PropertyName = "Cooldown (seconds)")]
        public int cooldownSeconds { get; set; }
    }

    public class ChatSettings
    {
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string chatPrefix { get; set; }
        [JsonProperty(PropertyName = "Chat Icon SteamID")]
        public ulong chatSteamID { get; set; }
    }

    public class MessageSettings
    {
        [JsonProperty(PropertyName = "Global Response (true = global response, false = player response)")]
        public bool globalResponse { get; set; }
        [JsonProperty(PropertyName = "Use Chat Response")]
        public bool chat { get; set; }
        [JsonProperty(PropertyName = "Use Game Tip Response")]
        public bool toast { get; set; }
        [JsonProperty(PropertyName = "Use Single Line Chat Pop Response")]
        public bool oneLine { get; set; }
        [JsonProperty(PropertyName = "Value Color (HEX)")]
        public string valueColor { get; set; }
    }

    public class ResponseSettings
    {
        [JsonProperty(PropertyName = "Show Online Players")]
        public bool showOnlinePlayers { get; set; }
        [JsonProperty(PropertyName = "Show Sleeping Players")]
        public bool showSleepingPlayers { get; set; }
        [JsonProperty(PropertyName = "Show Joining Players")]
        public bool showJoiningPlayers { get; set; }
        [JsonProperty(PropertyName = "Show Queued Players")]
        public bool showQueuedPlayers { get; set; }
    }

    public class ConnectSettings
    {
        [JsonProperty(PropertyName = "Show Pop On Connect")]
        public bool showPopOnConnect { get; set; }
        [JsonProperty(PropertyName = "Show Welcome Message")]
        public bool showWelcomeMessage { get; set; }
        [JsonProperty(PropertyName = "Show Wipe On Connect")]
        public bool showWipeOnConnect { get; set; }
    }

    public class WipeSettings
    {
        [JsonProperty(PropertyName = "Wipe Timer Enabled")]
        public bool wipeTimerEnabled { get; set; }
        [JsonProperty(PropertyName = "Wipe Timer (epoch)")]
        public long wipeTimer { get; set; }
    }

    public class DiscordSettings
    {
        [JsonProperty(PropertyName = "Discord Enabled")]
        public bool discordEnabled { get; set; }
        [JsonProperty(PropertyName = "Discord Invite Link")]
        public string discordLink { get; set; }
    }

    public static Configuration DefaultConfig();
    protected override void LoadConfig();
    private void UpdateConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerChat(BasePlayer player, string message, Chat.ChatChannel channel);
    private bool CanUseTrigger(ulong userID);
    private void SendMessage(BasePlayer player);
     void SendToastToActivePlayers(List<string> toastMessages);
    private string ApplyColor(string text, string hexColor);
    private object OnBetterChat(Dictionary<string, object> messageData);
    private object Filter(Dictionary<string, object> messageData);
    private bool RemoveMessage(string message);
    private string GetWipeTime(WipeSettings timer);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Cooldown Settings")]
    public CooldownSettings CooldownSettings { get; set; }
    [JsonProperty(PropertyName = "Chat Settings")]
    public ChatSettings ChatSettings { get; set; }
    [JsonProperty(PropertyName = "Messgae Settings")]
    public MessageSettings MessageSettings { get; set; }
    [JsonProperty(PropertyName = "Response Settings")]
    public ResponseSettings ResponseSettings { get; set; }
    [JsonProperty(PropertyName = "Connect Settings")]
    public ConnectSettings ConnectSettings { get; set; }
    [JsonProperty(PropertyName = "Wipe Response Settings")]
    public WipeSettings WipeSettings { get; set; }
    [JsonProperty(PropertyName = "Discord Response Settings")]
    public DiscordSettings DiscordSettings { get; set; }
    public Core.VersionNumber Version { get; set; }
}

public class CooldownSettings
{
    [JsonProperty(PropertyName = "Cooldown (seconds)")]
    public int cooldownSeconds { get; set; }
}

public class ChatSettings
{
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string chatPrefix { get; set; }
    [JsonProperty(PropertyName = "Chat Icon SteamID")]
    public ulong chatSteamID { get; set; }
}

public class MessageSettings
{
    [JsonProperty(PropertyName = "Global Response (true = global response, false = player response)")]
    public bool globalResponse { get; set; }
    [JsonProperty(PropertyName = "Use Chat Response")]
    public bool chat { get; set; }
    [JsonProperty(PropertyName = "Use Game Tip Response")]
    public bool toast { get; set; }
    [JsonProperty(PropertyName = "Use Single Line Chat Pop Response")]
    public bool oneLine { get; set; }
    [JsonProperty(PropertyName = "Value Color (HEX)")]
    public string valueColor { get; set; }
}

public class ResponseSettings
{
    [JsonProperty(PropertyName = "Show Online Players")]
    public bool showOnlinePlayers { get; set; }
    [JsonProperty(PropertyName = "Show Sleeping Players")]
    public bool showSleepingPlayers { get; set; }
    [JsonProperty(PropertyName = "Show Joining Players")]
    public bool showJoiningPlayers { get; set; }
    [JsonProperty(PropertyName = "Show Queued Players")]
    public bool showQueuedPlayers { get; set; }
}

public class ConnectSettings
{
    [JsonProperty(PropertyName = "Show Pop On Connect")]
    public bool showPopOnConnect { get; set; }
    [JsonProperty(PropertyName = "Show Welcome Message")]
    public bool showWelcomeMessage { get; set; }
    [JsonProperty(PropertyName = "Show Wipe On Connect")]
    public bool showWipeOnConnect { get; set; }
}

public class WipeSettings
{
    [JsonProperty(PropertyName = "Wipe Timer Enabled")]
    public bool wipeTimerEnabled { get; set; }
    [JsonProperty(PropertyName = "Wipe Timer (epoch)")]
    public long wipeTimer { get; set; }
}

public class DiscordSettings
{
    [JsonProperty(PropertyName = "Discord Enabled")]
    public bool discordEnabled { get; set; }
    [JsonProperty(PropertyName = "Discord Invite Link")]
    public string discordLink { get; set; }
}


```

---

## ServerRewards

```csharp
using System;
using Oxide.Core;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;
using System.Collections;
using System.Reflection;
using System.IO;
using System.Linq;

Oxide.Plugins
[Info("ServerRewards", "k1lly0u", "0.3.6", ResourceId = 1751)]
public class ServerRewards : RustPlugin
{
    [PluginReference]
     Plugin Kits;
    [PluginReference]
     Plugin Economics;
    [PluginReference]
     Plugin HumanNPC;
    [PluginReference]
     Plugin LustyMap;
    [PluginReference]
     Plugin PlaytimeTracker;
     PointData playerData;
    private DynamicConfigFile PlayerData;
     RewardDataStorage rewardData;
    private DynamicConfigFile RewardData;
     SaleDataStorage saleData;
    private DynamicConfigFile SaleData;
     NPCDealers npcDealers;
    private DynamicConfigFile NPC_Dealers;
    static GameObject webObject;
    static UnityWeb uWeb;
    static MethodInfo getFileData;
    static ServerRewards instance;
     ConfigData configData;
    private Timer saveTimer;
    private Dictionary<ulong, OUIData> OpenUI;
    private Dictionary<string, string> ItemNames;
    private Dictionary<ulong, int> PointCache;
    private Dictionary<ulong, NPCInfos> NPCCreator;
     class PointData
    {
        public Dictionary<ulong, int> Players;
    }

     class RewardDataStorage
    {
        public Dictionary<string, KitInfo> RewardKits;
        public Dictionary<int, ItemInfo> RewardItems;
        public Dictionary<string, CommandInfo> RewardCommands;
        public Dictionary<string, Dictionary<ulong, uint>> storedImages;
    }

     class SaleDataStorage
    {
        public Dictionary<int, Dictionary<ulong, SaleInfo>> Prices;
    }

     class SaleInfo
    {
        public float SalePrice;
        public string Name;
        public bool Enabled;
    }

     class KitInfo
    {
        public string KitName;
        public string Description;
        public string URL;
        public int Cost;
    }

     class ItemInfo
    {
        public string DisplayName;
        public string URL;
        public int ID;
        public int Amount;
        public ulong Skin;
        public int Cost;
        public int TargetID;
    }

     class CommandInfo
    {
        public List<string> Command;
        public string Description;
        public int Cost;
    }

     class NPCDealers
    {
        public Dictionary<string, NPCInfos> NPCIDs;
    }

     class NPCInfos
    {
        public float X;
        public float Z;
        public float ID;
        public string NPCID;
        public bool isCustom;
        public List<int> itemList;
        public List<string> kitList;
        public List<string> commandList;
        public bool allowExchange;
        public bool allowTransfer;
        public bool allowSales;
    }

     class KitItemEntry
    {
        public int ItemAmount;
        public List<string> ItemMods;
    }

    public class OUIData
    {
        public ElementType type;
        public int page;
        public string npcid;
    }

    static string UIMain;
    static string UISelect;
    static string UIRP;
    static string UIPopup;
     class SR_UI
    {
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align, float fadein);
        static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, float fadein);
        static public void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
        static public void CreateTextOverlay(CuiElementContainer container, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
    }

    private Dictionary<string, string> UIColors;
    private void DisplayPoints(BasePlayer player);
    private void PopupMessage(BasePlayer player, string msg);
    private void OpenNavMenu(BasePlayer player, string npcid);
    private void SwitchElement(BasePlayer player, ElementType type, int page, string npcid);
    private CuiElementContainer GetElement(ElementType type, int page, string npcid);
    private void DestroyUI(BasePlayer player);
    public static class UIElements
    {
        public static Dictionary<ElementType, CuiElementContainer[]> standardElements;
        public static Dictionary<string, Dictionary<ElementType, CuiElementContainer[]>> npcElements;
        public static List<string> elementIDs;
        public static void RenameComponents(CuiElementContainer[] container);
        public static void DestroyUI(BasePlayer player, OUIData data);
        public static void DestroyNav(BasePlayer player, OUIData data);
        static void DestroyWholeList(BasePlayer player);
    }

     void InitializeAllElements();
    private void CreateNavUI();
    private void CreateKitsUI();
    private void CreateItemsUI();
    private void CreateCommandsUI();
    private void CreateExchangeUI();
    private CuiElementContainer CreateKitsElement(int page);
    private CuiElementContainer CreateItemsElement(int page);
    private CuiElementContainer CreateCommandsElement(int page);
    private void CreateAllNPCs();
    private void CreateNPCMenu(string npcid);
    private void CreateNPCNavUI(string npcid);
    private void CreateNPCKitsUI(string npcid);
    private void CreateNPCItemsUI(string npcid);
    private void CreateNPCCommandsUI(string npcid);
    private CuiElementContainer CreateNPCKitsElement(string npcid, int page);
    private CuiElementContainer CreateNPCItemsElement(string npcid, int page);
    private CuiElementContainer CreateNPCCommandsElement(string npcid, int page);
    private void CreateSaleElement(BasePlayer player);
    private void CreateInventoryEntry(CuiElementContainer container, string panelName, int itemId, ulong skinId, string name, int amount, int number);
    private void SellItem(BasePlayer player, int itemId, ulong skinId, string name, int amount);
    [ConsoleCommand("SRUI_CancelSale")]
    private void cmdCancelSale(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_SellItem")]
    private void cmdSellItem(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_Sell")]
    private void cmdSell(ConsoleSystem.Arg arg);
    private float[] CalcPosInv(int number);
    private int GetAmount(BasePlayer player, int itemid, ulong skinid);
    private void TakeResources(BasePlayer player, int itemid, ulong skinId, int iAmount);
    private int TakeResourcesFrom(BasePlayer player, List<Item> container, int itemid, ulong skinId, int iAmount);
    private void CreateTransferElement(BasePlayer player, int page);
    private void TransferElement(BasePlayer player, string name, string id);
    private void CreatePlayerNameEntry(CuiElementContainer container, string panelName, string name, string id, int number);
    private float[] CalcPlayerNamePos(int number);
    private void CreateMenuButton(CuiElementContainer container, string panelName, string buttonname, string command, int number);
    private void CreateKitCommandEntry(CuiElementContainer container, string panelName, string name, string description, int cost, int number, bool kit);
    private void CreateItemEntry(CuiElementContainer container, string panelName, int itemnumber, int number);
    private string GetKitContents(string kitname);
    [ConsoleCommand("SRUI_BuyKit")]
    private void cmdBuyKit(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_BuyCommand")]
    private void cmdBuyCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_BuyItem")]
    private void cmdBuyItem(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_ChangeElement")]
    private void cmdChangeElement(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_Exchange")]
    private void cmdExchange(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_Transfer")]
    private void ccmdTransfer(ConsoleSystem.Arg args);
    [ConsoleCommand("SRUI_TransferNext")]
    private void ccmdTransferNext(ConsoleSystem.Arg args);
    [ConsoleCommand("SRUI_TransferID")]
    private void ccmdTransferID(ConsoleSystem.Arg args);
    [ConsoleCommand("SRUI_DestroyAll")]
    private void cmdDestroyAll(ConsoleSystem.Arg arg);
     void Loaded();
     void OnServerInitialized();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void Unload();
    private void SendMSG(BasePlayer player, string msg, string keyword);
    private void InformPoints(BasePlayer player);
    private void OpenStore(BasePlayer player, string npcid);
    private string GetPlaytimeClock(double time);
    private void GiveItem(BasePlayer player, int itemkey);
    private object FindPlayer(BasePlayer player, string arg);
    private bool RemovePlayer(ulong ID);
     void SendEchoConsole(Network.Connection cn, string msg);
    [HookMethod("AddPoints")]
    public object AddPoints(object userID, int amount);
    [HookMethod("TakePoints")]
    public object TakePoints(object userID, int amount, string item);
    [HookMethod("CheckPoints")]
    public object CheckPoints(object userID);
    private object GetUserID(object userID);
    private void CloseMap(BasePlayer player);
    private void OpenMap(BasePlayer player);
    private void AddMapMarker(float x, float z, string name, string icon);
    private void RemoveMapMarker(string name);
     void OnUseNPC(BasePlayer npc, BasePlayer player);
    private static FieldInfo serverinput;
    private BasePlayer FindEntity(BasePlayer player);
    private object Ray(BasePlayer player, Vector3 Aim);
    private string isNPCRegistered(string ID);
    [ChatCommand("srnpc")]
     void cmdSRNPC(BasePlayer player, string command, string[] args);
    private void NPCLootMenu(BasePlayer player, int page);
     void CreateItemButton(CuiElementContainer Main, string panel, string b1color, string b1text, string b1command, string b2color, string b2text, string b2command, int number, float xPos);
    [ConsoleCommand("SRUI_CustomList")]
    private void cmdCustomList(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_NPCPage")]
    private void cmdNPCPage(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_NPCOption")]
    private void cmdNPCOption(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_NPCCancel")]
    private void cmdNPCCancel(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_NPCSave")]
    private void cmdNPCSave(ConsoleSystem.Arg arg);
     void UpdatePriceList();
     bool HasSkins(ItemDefinition item);
    [ChatCommand("s")]
    private void cmdStore(BasePlayer player, string command, string[] args);
    [ChatCommand("rewards")]
    private void cmdRewards(BasePlayer player, string command, string[] args);
    [ChatCommand("sr")]
    private void cmdSR(BasePlayer player, string command, string[] args);
    [ConsoleCommand("sr")]
    private void ccmdSR(ConsoleSystem.Arg arg);
     bool isAuth(BasePlayer player);
     bool isAuthCon(ConsoleSystem.Arg arg);
     void SaveData();
     void SaveRewards();
     void SaveNPC();
     void SavePrices();
    private void SaveLoop();
     void LoadData();
     class Tabs
    {
        public bool Disable_Kits { get; set; }
        public bool Disable_Items { get; set; }
        public bool Disable_Commands { get; set; }
        public bool Disable_CurrencyExchange { get; set; }
        public bool Disable_CurrencyTransfer { get; set; }
        public bool Disable_SellersScreen { get; set; }
    }

     class Messaging
    {
        public string MSG_MainColor { get; set; }
        public string MSG_Color { get; set; }
    }

     class Exchange
    {
        public int Econ_ExchangeRate { get; set; }
        public int RP_ExchangeRate { get; set; }
    }

     class Options
    {
        public bool LogRPTransactions { get; set; }
        public int Save_Interval { get; set; }
        public bool NPCDealers_Only { get; set; }
        public bool Use_PTT { get; set; }
    }

     class UIOptions
    {
        public bool DisableUI_FadeIn { get; set; }
        public bool ShowKitContents { get; set; }
    }

     class ConfigData
    {
        public Tabs Categories { get; set; }
        public Exchange CurrencyExchange { get; set; }
        public Messaging Messaging { get; set; }
        public Options Options { get; set; }
        public UIOptions UI_Options { get; set; }
    }

    private void LoadVariables();
    private void LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConfig(ConfigData config);
     class QueueItem
    {
        public string url;
        public string itemid;
        public ulong skinid;
        public QueueItem(string ur, string na, ulong sk);
    }

     class UnityWeb : MonoBehaviour
    {
         ServerRewards filehandler;
        const int MaxActiveLoads;
        private Queue<QueueItem> QueueList;
        static byte activeLoads;
        private MemoryStream stream;
        private void Awake();
        private void OnDestroy();
        public void Add(string url, string itemid, ulong skinid);
         void Next();
        private void ClearStream();
         IEnumerator WaitForRequest(QueueItem info);
    }

    [ConsoleCommand("loadimages")]
    private void cmdLoadImages(ConsoleSystem.Arg arg);
    [ConsoleCommand("loadlocalimages")]
    private void cmdLoadLocalImages(ConsoleSystem.Arg arg);
    private void LoadImages(bool isLocal);
     string msg(string key, string id);
     Dictionary<string, string> messages;
}

 class PointData
{
    public Dictionary<ulong, int> Players;
}

 class RewardDataStorage
{
    public Dictionary<string, KitInfo> RewardKits;
    public Dictionary<int, ItemInfo> RewardItems;
    public Dictionary<string, CommandInfo> RewardCommands;
    public Dictionary<string, Dictionary<ulong, uint>> storedImages;
}

 class SaleDataStorage
{
    public Dictionary<int, Dictionary<ulong, SaleInfo>> Prices;
}

 class SaleInfo
{
    public float SalePrice;
    public string Name;
    public bool Enabled;
}

 class KitInfo
{
    public string KitName;
    public string Description;
    public string URL;
    public int Cost;
}

 class ItemInfo
{
    public string DisplayName;
    public string URL;
    public int ID;
    public int Amount;
    public ulong Skin;
    public int Cost;
    public int TargetID;
}

 class CommandInfo
{
    public List<string> Command;
    public string Description;
    public int Cost;
}

 class NPCDealers
{
    public Dictionary<string, NPCInfos> NPCIDs;
}

 class NPCInfos
{
    public float X;
    public float Z;
    public float ID;
    public string NPCID;
    public bool isCustom;
    public List<int> itemList;
    public List<string> kitList;
    public List<string> commandList;
    public bool allowExchange;
    public bool allowTransfer;
    public bool allowSales;
}

 class KitItemEntry
{
    public int ItemAmount;
    public List<string> ItemMods;
}

public class OUIData
{
    public ElementType type;
    public int page;
    public string npcid;
}

 class SR_UI
{
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align, float fadein);
    static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, float fadein);
    static public void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
    static public void CreateTextOverlay(CuiElementContainer container, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
}

public static class UIElements
{
    public static Dictionary<ElementType, CuiElementContainer[]> standardElements;
    public static Dictionary<string, Dictionary<ElementType, CuiElementContainer[]>> npcElements;
    public static List<string> elementIDs;
    public static void RenameComponents(CuiElementContainer[] container);
    public static void DestroyUI(BasePlayer player, OUIData data);
    public static void DestroyNav(BasePlayer player, OUIData data);
    static void DestroyWholeList(BasePlayer player);
}

 class Tabs
{
    public bool Disable_Kits { get; set; }
    public bool Disable_Items { get; set; }
    public bool Disable_Commands { get; set; }
    public bool Disable_CurrencyExchange { get; set; }
    public bool Disable_CurrencyTransfer { get; set; }
    public bool Disable_SellersScreen { get; set; }
}

 class Messaging
{
    public string MSG_MainColor { get; set; }
    public string MSG_Color { get; set; }
}

 class Exchange
{
    public int Econ_ExchangeRate { get; set; }
    public int RP_ExchangeRate { get; set; }
}

 class Options
{
    public bool LogRPTransactions { get; set; }
    public int Save_Interval { get; set; }
    public bool NPCDealers_Only { get; set; }
    public bool Use_PTT { get; set; }
}

 class UIOptions
{
    public bool DisableUI_FadeIn { get; set; }
    public bool ShowKitContents { get; set; }
}

 class ConfigData
{
    public Tabs Categories { get; set; }
    public Exchange CurrencyExchange { get; set; }
    public Messaging Messaging { get; set; }
    public Options Options { get; set; }
    public UIOptions UI_Options { get; set; }
}

 class QueueItem
{
    public string url;
    public string itemid;
    public ulong skinid;
    public QueueItem(string ur, string na, ulong sk);
}

 class UnityWeb : MonoBehaviour
{
     ServerRewards filehandler;
    const int MaxActiveLoads;
    private Queue<QueueItem> QueueList;
    static byte activeLoads;
    private MemoryStream stream;
    private void Awake();
    private void OnDestroy();
    public void Add(string url, string itemid, ulong skinid);
     void Next();
    private void ClearStream();
     IEnumerator WaitForRequest(QueueItem info);
}


```

---

## ServerStats

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Rust;
using UnityEngine;
using WebSocketSharp;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("ServerStats", "TheRyuzaki & skyplugins.ru", "1.0.6")]
public class ServerStats : CovalencePlugin
{
    private static ServerStats _instance;
    private WSH _wsh;
    private FPSVisor ActiveVisor;
    public class WSH
    {
        public WebSocket client;
        public bool hasUnloaded;
        public bool networkStatus;
        public List<string> pool;
        public Coroutine coroutine;
        public WSH(string WS);
        public void connect();
        public void close();
        public bool send(Dictionary<string, object> packet, bool networkIgnore);
        public bool send(string packet, bool networkIgnore);
        private void OnSocketConnected(object sender, EventArgs e);
        private void OnSocketMessage(object sender, MessageEventArgs e);
        private void OnSocketDisconnected(object sender, CloseEventArgs e);
        private IEnumerator Sender();
    }

    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private int MinimalFPS;
    protected override void LoadDefaultConfig();
    public class FPSVisor : MonoBehaviour
    {
        private void Update();
    }

    public class NetworkWelcomePacket : Pool.IPooled
    {
        [JsonProperty("method")]
        public string Method { get; set; }
        [JsonProperty("ServerIp")]
        public string ServerIp { get; set; }
        [JsonProperty("serverName")]
        public string ServerName { get; set; }
        [JsonProperty("password")]
        public string Password { get; set; }
        public void EnterPool();
        public void LeavePool();
    }

    public class NetworkTickPacket : Pool.IPooled
    {
        [JsonProperty("method")]
        public string Method { get; set; }
        [JsonProperty("listPlugins")]
        public List<PluginItem> ListPlugins { get; set; }
        [JsonProperty("minfps")]
        public int MinimalFps { get; set; }
        [JsonProperty("fps")]
        public int Fps { get; set; }
        [JsonProperty("ent")]
        public int Ent { get; set; }
        [JsonProperty("online")]
        public int Online { get; set; }
        [JsonProperty("SleepPlayer")]
        public int SleepPlayer { get; set; }
        [JsonProperty("JoiningPlayer")]
        public int JoiningPlayer { get; set; }
        [JsonProperty("QueuedPlayer")]
        public int QueuedPlayer { get; set; }
        public void EnterPool();
        public void LeavePool();
    }

}

public class WSH
{
    public WebSocket client;
    public bool hasUnloaded;
    public bool networkStatus;
    public List<string> pool;
    public Coroutine coroutine;
    public WSH(string WS);
    public void connect();
    public void close();
    public bool send(Dictionary<string, object> packet, bool networkIgnore);
    public bool send(string packet, bool networkIgnore);
    private void OnSocketConnected(object sender, EventArgs e);
    private void OnSocketMessage(object sender, MessageEventArgs e);
    private void OnSocketDisconnected(object sender, CloseEventArgs e);
    private IEnumerator Sender();
}

public class FPSVisor : MonoBehaviour
{
    private void Update();
}

public class NetworkWelcomePacket : Pool.IPooled
{
    [JsonProperty("method")]
    public string Method { get; set; }
    [JsonProperty("ServerIp")]
    public string ServerIp { get; set; }
    [JsonProperty("serverName")]
    public string ServerName { get; set; }
    [JsonProperty("password")]
    public string Password { get; set; }
    public void EnterPool();
    public void LeavePool();
}

public class NetworkTickPacket : Pool.IPooled
{
    [JsonProperty("method")]
    public string Method { get; set; }
    [JsonProperty("listPlugins")]
    public List<PluginItem> ListPlugins { get; set; }
    [JsonProperty("minfps")]
    public int MinimalFps { get; set; }
    [JsonProperty("fps")]
    public int Fps { get; set; }
    [JsonProperty("ent")]
    public int Ent { get; set; }
    [JsonProperty("online")]
    public int Online { get; set; }
    [JsonProperty("SleepPlayer")]
    public int SleepPlayer { get; set; }
    [JsonProperty("JoiningPlayer")]
    public int JoiningPlayer { get; set; }
    [JsonProperty("QueuedPlayer")]
    public int QueuedPlayer { get; set; }
    public void EnterPool();
    public void LeavePool();
}


```

---

## Settings

```csharp
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Settings", "VooDoo", "1.0.0")]
[Description("Settings switch's")]
public class Settings : RustPlugin
{
    [PluginReference]
     Plugin XMenu;
    public Dictionary<ulong, Dictionary<int, UserSettings>> userSettings;
    public class UserSettings
    {
        public bool isOn;
        public Dictionary<int, UserSettings> settings;
    }

    private PluginConfig config;
    private class PluginConfig
    {
        public ColorConfig colorConfig;
        public class ColorConfig
        {
            public string menuContentHighlighting;
            public string menuContentHighlightingalternative;
            public string gradientColor;
        }

        public Dictionary<int, PluginSettings> pluginSettings;
        public class PluginSettings
        {
            public string commandDescription;
            public string command;
            public bool isChecked;
            public bool onlyOne;
            public Dictionary<int, PluginSettings> subSettings;
        }

    }

    private void Init();
    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
    public const string MenuLayer;
    public const string MenuItemsLayer;
    public const string MenuSubItemsLayer;
    public const string MenuContent;
     Timer TimerInitialize;
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void RenderSettings(ulong userID, object[] objects);
    [ConsoleCommand("settings.sendcommand")]
    private void SendCommand(ConsoleSystem.Arg arg);
    private static string HexToRustFormat(string hex);
}

public class UserSettings
{
    public bool isOn;
    public Dictionary<int, UserSettings> settings;
}

private class PluginConfig
{
    public ColorConfig colorConfig;
    public class ColorConfig
    {
        public string menuContentHighlighting;
        public string menuContentHighlightingalternative;
        public string gradientColor;
    }

    public Dictionary<int, PluginSettings> pluginSettings;
    public class PluginSettings
    {
        public string commandDescription;
        public string command;
        public bool isChecked;
        public bool onlyOne;
        public Dictionary<int, PluginSettings> subSettings;
    }

}

public class ColorConfig
{
    public string menuContentHighlighting;
    public string menuContentHighlightingalternative;
    public string gradientColor;
}

public class PluginSettings
{
    public string commandDescription;
    public string command;
    public bool isChecked;
    public bool onlyOne;
    public Dictionary<int, PluginSettings> subSettings;
}


```

---

## SetupFurnaces

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("SetupFurnaces", "Sempai", "1.1.0")]
internal class SetupFurnaces : RustPlugin
{
    private const string Layer;
    private const string perm;
    private Configuration _config;
    private Dictionary<string, FurnaceDefenition> furnacesSlots;
    private class FurnaceDefenition
    {
        public string InputType;
        public float OutputAmount;
        public List<SlotType> SlotsType;
    }

    private class Configuration
    {
        [JsonProperty(PropertyName = "Oven setup for players", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, SetupOven> playersOvenSetup;
    }

    private class SetupOven
    {
        [JsonProperty(PropertyName = "Quick smelt value")]
        public int quickSmelt;
        public Dictionary<string, bool> stopBurn;
        public Dictionary<string, bool> quickSmelting;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    private void Unload();
    private void OnOvenCook(BaseOven oven, Item item, BaseEntity slot);
    private void OnLootEntity(BasePlayer player, BaseOven oven);
    [ChatCommand("fsetup")]
    private void cmdChatmenu(BasePlayer player, string command, string[] args);
    [ConsoleCommand("UI_SF")]
    private void cmdConsole(ConsoleSystem.Arg arg);
    private void Take(IEnumerable<Item> itemList, string shortname, int iAmount);
    private void ShowUISetupFurnaces(BasePlayer player, string fperm);
    private void ShowUISelectPERM(BasePlayer player);
    private void ShowUIBG(BasePlayer player);
}

private class FurnaceDefenition
{
    public string InputType;
    public float OutputAmount;
    public List<SlotType> SlotsType;
}

private class Configuration
{
    [JsonProperty(PropertyName = "Oven setup for players", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, SetupOven> playersOvenSetup;
}

private class SetupOven
{
    [JsonProperty(PropertyName = "Quick smelt value")]
    public int quickSmelt;
    public Dictionary<string, bool> stopBurn;
    public Dictionary<string, bool> quickSmelting;
}


```

---

## SharkBait

```csharp
using System;
using System.Collections;
using Network;
using System.Collections.Generic;
using UnityEngine;
using Facepunch;
using Rust;
using System.Linq;

Oxide.Plugins
[Info("SharkBait", "Colon Blow", "1.0.4")]
 class SharkBait : RustPlugin
{
     void Loaded();
     void OnServerInitialized();
    static ulong GWFin;
    static ulong GWFront;
    static ulong GWBack;
    static ulong HHFin;
    static ulong HHFront;
    static ulong HHBack;
     bool EnableAutoSpawn;
     bool EnableSpawnAtRockFormations;
     bool EnableSpawnAtDiveSites;
     bool EnableSpawnAtFloatingLoot;
    static float SpawnActivationRadius;
    static float SharkAggroRadius;
    static float SharkBiteRange;
    static float SharkDespawnTime;
    static float SharkSwimSpeed;
    static float SharkDamageToPlayer;
     int SharkSpawnChance;
    static bool SharksCanAttackBoats;
    static bool SharksCanAttackPlayers;
     bool Changed;
     void LoadVariables();
     void LoadDefaultConfig();
     void LoadConfigVariables();
     void CheckCfg(string Key, T var);
     void CheckCfgFloat(string Key, float var);
     void CheckCfgUlong(string Key, ulong var);
    private void LoadMessages();
    [ChatCommand("sharkbait")]
     void chatSharkBait(BasePlayer player, string command, string[] args);
    [ChatCommand("sharkbait.respawn")]
     void chatSharkBaitRespawn(BasePlayer player, string command, string[] args);
    [ConsoleCommand("sharkbait.respawn")]
     void cmdConsoleSharkBaitRespawn(ConsoleSystem.Arg arg);
    [ChatCommand("sharkbait.killall")]
     void chatSharkBaitKillAll(BasePlayer player, string command, string[] args);
    private void RemoveAllSharks();
     void OnEntitySpawned(BaseEntity entity, UnityEngine.GameObject gameObject);
     void RespawnAllSharks();
    public void RespawnHandler(GameObject gobject);
    public void AddSharkEntity(Vector3 pos);
     void SpawnShark(Vector3 pos);
     void Unload();
     void DestroyAll();
     class SharkSpawnController : MonoBehaviour
    {
         SharkBait _instance;
         SphereCollider detectionradius;
         Vector3 spawnlocation;
         bool doactivation;
         Timer mytimer;
         void Awake();
        private void OnTriggerEnter(Collider col);
         void ToggleTrigger();
         void OnDestroy();
    }

     class SharkEntity : BaseEntity
    {
         SharkBait instance;
         BaseEntity sharkentity;
         BaseEntity sharkfront;
         BaseEntity sharkback;
         BaseEntity sharkfin;
         BaseEntity sharkbait;
         int counter;
         SphereCollider aggroradius;
         Rigidbody rigidbody;
         Vector3 initialspawn;
         Vector3 spawnlocation;
         Vector3 position;
         Vector3 attackpos;
         Vector3 offset;
        private float _angle;
         float speed;
         float despawntimer;
         float despawntimelimit;
         int attackcounter;
         bool didattack;
         bool reversemovment;
         bool moveback;
         void Awake();
         void MovementRoll();
         void SpawnSharky();
        private void OnTriggerStay(Collider col);
         void MovementSplash();
         void NearSharkBait();
        public bool IsWaterDeepEnough(Vector3 position);
         Vector3 GetNewPos();
         void FixedUpdate();
         void OnDestroy();
    }

}

 class SharkSpawnController : MonoBehaviour
{
     SharkBait _instance;
     SphereCollider detectionradius;
     Vector3 spawnlocation;
     bool doactivation;
     Timer mytimer;
     void Awake();
    private void OnTriggerEnter(Collider col);
     void ToggleTrigger();
     void OnDestroy();
}

 class SharkEntity : BaseEntity
{
     SharkBait instance;
     BaseEntity sharkentity;
     BaseEntity sharkfront;
     BaseEntity sharkback;
     BaseEntity sharkfin;
     BaseEntity sharkbait;
     int counter;
     SphereCollider aggroradius;
     Rigidbody rigidbody;
     Vector3 initialspawn;
     Vector3 spawnlocation;
     Vector3 position;
     Vector3 attackpos;
     Vector3 offset;
    private float _angle;
     float speed;
     float despawntimer;
     float despawntimelimit;
     int attackcounter;
     bool didattack;
     bool reversemovment;
     bool moveback;
     void Awake();
     void MovementRoll();
     void SpawnSharky();
    private void OnTriggerStay(Collider col);
     void MovementSplash();
     void NearSharkBait();
    public bool IsWaterDeepEnough(Vector3 position);
     Vector3 GetNewPos();
     void FixedUpdate();
     void OnDestroy();
}


```

---

## ShiningSeed

```csharp
using UnityEngine;
using Oxide.Core;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("ShiningSeed", "Anonymuspro", "0.0.2")]
public class ShiningSeed : RustPlugin
{
    public class Seed
    {
        public string shortname;
        public string name;
        public ulong skinId;
    }

     Seed seed;
    public class Wood
    {
        [JsonProperty("Не трогать!")]
        public BasePlayer player;
        [JsonProperty("Не трогать!")]
        public BaseEntity wood;
        public Vector3 woodPos;
        public PlantEntity plant;
        public List<BaseEntity> boxs;
        public int BoxCount;
        public bool DownBox;
        public bool isSpawned;
        public int CurrentMaturation;
        public int MaxMaturation;
        public int BoxMat;
        public int BoxMaxMat;
        public int CurrentEtap;
        public int MaxEtap;
    }

    public class CaseItem
    {
        public string shortname;
        public bool Random;
        public int Min;
        public int Max;
        public int Count;
    }

    public List<Wood> woods;
    public Dictionary<string, string> Messages;
    public class DataConfig
    {
        [JsonProperty("Время роста дерева в секундах")]
        public int Time;
        [JsonProperty("Количество вещей в ящике")]
        public int ItemsCount;
        [JsonProperty("Кол-во ящиков на дереве")]
        public int BoxCount;
        [JsonProperty("Время появления ящиков в секундах")]
        public int BoxTime;
        [JsonProperty("Максимальное кол-во этапов для дерева")]
        public int MaxEtap;
        [JsonProperty("Список префабов этапов дерева")]
        public List<string> etaps;
        [JsonProperty("Права на выдачу")]
        public string Permission;
        [JsonProperty("Путь до ящика")]
        public string CrateBasic;
        [JsonProperty("Шанс выпдаения с дерева (макс-100)")]
        public int Chance;
        [JsonProperty("Список вещей в ящиках")]
        public List<CaseItem> casesItems;
        [JsonProperty("Ссылка на удачный эффект")]
        public string SucEffect;
        [JsonProperty("Ссылка на эффект ошибки")]
        public string ErrorEffect;
        [JsonProperty("Эффект сообщения после спавна для игрока")]
        public string WoodMessageEffect;
        [JsonProperty("Размер шрифта")]
        public int FontSize;
        [JsonProperty("Цвет текста")]
        public string Color;
        [JsonProperty("Позиция текста на дереве")]
        public Vector3 pos;
        [JsonProperty("Размер шрифта процентов")]
        public int FontProcentSize;
        [JsonProperty("Размер шрифта этапов")]
        public int FontEtapSize;
    }

     DataConfig cfg;
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void Init();
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnEntityBuilt(Planner plan, GameObject go);
     object OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void Unload();
    public void TimerTick();
    public void WoodRost(Wood wood);
    public void SpawnWood(Wood wood, int etap);
    [ChatCommand("seed")]
     void GiveSeed(BasePlayer player, string command, string[] args);
     void AddSeed(BasePlayer player);
    public void SpawnBox(Wood wood, int i);
    public void AddLoot(BaseEntity box);
     Wood SearchFromEntity(PlantEntity entity);
    public void UpdateText();
    public void ShowText(Wood wood, BasePlayer player);
}

public class Seed
{
    public string shortname;
    public string name;
    public ulong skinId;
}

public class Wood
{
    [JsonProperty("Не трогать!")]
    public BasePlayer player;
    [JsonProperty("Не трогать!")]
    public BaseEntity wood;
    public Vector3 woodPos;
    public PlantEntity plant;
    public List<BaseEntity> boxs;
    public int BoxCount;
    public bool DownBox;
    public bool isSpawned;
    public int CurrentMaturation;
    public int MaxMaturation;
    public int BoxMat;
    public int BoxMaxMat;
    public int CurrentEtap;
    public int MaxEtap;
}

public class CaseItem
{
    public string shortname;
    public bool Random;
    public int Min;
    public int Max;
    public int Count;
}

public class DataConfig
{
    [JsonProperty("Время роста дерева в секундах")]
    public int Time;
    [JsonProperty("Количество вещей в ящике")]
    public int ItemsCount;
    [JsonProperty("Кол-во ящиков на дереве")]
    public int BoxCount;
    [JsonProperty("Время появления ящиков в секундах")]
    public int BoxTime;
    [JsonProperty("Максимальное кол-во этапов для дерева")]
    public int MaxEtap;
    [JsonProperty("Список префабов этапов дерева")]
    public List<string> etaps;
    [JsonProperty("Права на выдачу")]
    public string Permission;
    [JsonProperty("Путь до ящика")]
    public string CrateBasic;
    [JsonProperty("Шанс выпдаения с дерева (макс-100)")]
    public int Chance;
    [JsonProperty("Список вещей в ящиках")]
    public List<CaseItem> casesItems;
    [JsonProperty("Ссылка на удачный эффект")]
    public string SucEffect;
    [JsonProperty("Ссылка на эффект ошибки")]
    public string ErrorEffect;
    [JsonProperty("Эффект сообщения после спавна для игрока")]
    public string WoodMessageEffect;
    [JsonProperty("Размер шрифта")]
    public int FontSize;
    [JsonProperty("Цвет текста")]
    public string Color;
    [JsonProperty("Позиция текста на дереве")]
    public Vector3 pos;
    [JsonProperty("Размер шрифта процентов")]
    public int FontProcentSize;
    [JsonProperty("Размер шрифта этапов")]
    public int FontEtapSize;
}


```

---

## ShipControl

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("ShipControl", "Hougan", "0.0.2")]
public class ShipControl : RustPlugin
{
    private class AdditionalContainer
    {
        [JsonProperty("Отображаемое имя")]
        public string DisplayName;
        [JsonProperty("Название префаба")]
        public string PrefabName;
        [JsonProperty("Локальная позиция")]
        public string LocalPosition;
        [JsonProperty("Возможные предметы")]
        public List<Dictionary<string, int>> RandomItems;
    }

    private class Configuration
    {
        [JsonProperty("Отключить появление корабля на совсем")]
        public bool DisableEvent;
        [JsonProperty("Время плавания корабля")]
        public float ActiveTime;
        [JsonProperty("Время уплывания корабля")]
        public float DeActivateTime;
        [JsonProperty("Оповещение о прибытии корабля")]
        public string SpawnedAlert;
        [JsonProperty("Отключить появление лодки на борту")]
        public bool DisableRHIB;
        [JsonProperty("Изменить количество топлива в лодке на борту")]
        public int RHIB_Fuel;
        [JsonProperty("Отключить появление стандартных контейнеров")]
        public bool DisableDefaultContainers;
        [JsonProperty("Дополнительные ящики с предметами")]
        public List<AdditionalContainer> CustomContainers;
        public static Configuration GetNewConf();
    }

    public bool EventEnabled;
    public float EventDuration;
    public float EgressDuration;
    public float currentRadition;
    private List<AdditionalContainer> PossibleContainers;
    private static Configuration config;
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [ChatCommand("add.crate")]
    private void cmdChatAdmin(BasePlayer player);
    [ConsoleCommand("UI_SC")]
    private void cmdConsoleHandler(ConsoleSystem.Arg args);
    private static string HexToRustFormat(string hex);
    private const string Layer;
    private void UI_DrawChooseType(BasePlayer player);
    private void OnServerInitialized();
    private void OnEntitySpawned(BaseNetworkable entity);
    private void SpawnCustomContainer(AdditionalContainer customContainer, CargoShip ship);
}

private class AdditionalContainer
{
    [JsonProperty("Отображаемое имя")]
    public string DisplayName;
    [JsonProperty("Название префаба")]
    public string PrefabName;
    [JsonProperty("Локальная позиция")]
    public string LocalPosition;
    [JsonProperty("Возможные предметы")]
    public List<Dictionary<string, int>> RandomItems;
}

private class Configuration
{
    [JsonProperty("Отключить появление корабля на совсем")]
    public bool DisableEvent;
    [JsonProperty("Время плавания корабля")]
    public float ActiveTime;
    [JsonProperty("Время уплывания корабля")]
    public float DeActivateTime;
    [JsonProperty("Оповещение о прибытии корабля")]
    public string SpawnedAlert;
    [JsonProperty("Отключить появление лодки на борту")]
    public bool DisableRHIB;
    [JsonProperty("Изменить количество топлива в лодке на борту")]
    public int RHIB_Fuel;
    [JsonProperty("Отключить появление стандартных контейнеров")]
    public bool DisableDefaultContainers;
    [JsonProperty("Дополнительные ящики с предметами")]
    public List<AdditionalContainer> CustomContainers;
    public static Configuration GetNewConf();
}


```

---

## ShopSystem

```csharp
using System.Collections.Generic;
using System.Linq;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;
using System;
using Oxide.Core;

Oxide.Plugins
[Info("ShopSystem", "Sempai#3239", "1.0.0")]
 class ShopSystem : RustPlugin
{
     string Layer;
     ShopSystem ins;
    [PluginReference]
     Plugin ImageLibrary;
     Dictionary<ulong, string> ActiveButton;
     Dictionary<ulong, DataBase> DB;
    public int Times;
    public int RP;
     List<string> Category;
    public class Settings
    {
        public string ShortName;
        public int Amount;
        public int Price;
        public string Category;
    }

    public class DataBase
    {
        public int Rp;
        public int Time;
        public List<string> Item;
    }

     Configuration config;
     class Configuration
    {
        public List<Settings> settings;
        public static Configuration GetNewConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void Unload();
     void CreateDataBase(BasePlayer player);
     void SaveDataBase(ulong userId);
    [ConsoleCommand("shop")]
     void ConsoleShop(ConsoleSystem.Arg args);
     void ShopUI(BasePlayer player);
     void ItemUI(BasePlayer player, string category, int page);
     void UpdateBalance(BasePlayer player);
     Timer Timer;
     void TimeUpdate(BasePlayer player);
    public static string FormatShortTime(TimeSpan time);
     double CurTime();
}

public class Settings
{
    public string ShortName;
    public int Amount;
    public int Price;
    public string Category;
}

public class DataBase
{
    public int Rp;
    public int Time;
    public List<string> Item;
}

 class Configuration
{
    public List<Settings> settings;
    public static Configuration GetNewConfig();
}


```

---

## ShowCrosshair

```csharp
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using System;

Oxide.Plugins
[Info("ShowCrosshair", "Marat", "1.0.7", ResourceId = 2057)]
[Description("Shows a crosshair on the screen.")]
 class ShowCrosshair : RustPlugin
{
     List<ulong> Cross;
     List<ulong> Menu;
     bool EnableCross(BasePlayer player);
     bool EnableMenu(BasePlayer player);
    private bool configChanged;
    private const string permShowCrosshair;
    private string background;
    private string background2;
    private void Loaded();
    protected override void LoadDefaultConfig();
    private bool usePermissions;
    private bool ShowOnLogin;
    private bool EnableSound;
    private bool ShowMessage;
    private bool KeyBindSet;
    private string SoundOpen;
    private string SoundDisable;
    private string SoundSelect;
    private string SoundToggle;
    private string commandmenu;
    private string command;
    private string keybind;
    private string colorClose;
    private string colorBackground;
    private string colorToggle;
    private string colorDisable;
    private string image1;
    private string image2;
    private string image3;
    private string image4;
    private string image5;
    private string image6;
    private string image7;
    private string image8;
    private void LoadConfiguration();
    private T GetConfigValue(string category, string setting, T defaultValue);
    private void LoadDefaultMessages();
    private void cmdChatCrosshair(BasePlayer player);
    private void cmdChatShowMenu(BasePlayer player);
    [ConsoleCommand("ShowMenu")]
    private void cmdConsoleShowMenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("CloseMenu")]
     void cmdConsoleCloseMenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("command1")]
     void cmdConsoleCommand1(ConsoleSystem.Arg arg);
    [ConsoleCommand("command2")]
     void cmdConsoleCommand2(ConsoleSystem.Arg arg);
    [ConsoleCommand("command3")]
     void cmdConsoleCommand3(ConsoleSystem.Arg arg);
    [ConsoleCommand("command4")]
     void cmdConsoleCommand4(ConsoleSystem.Arg arg);
    [ConsoleCommand("command5")]
     void cmdConsoleCommand5(ConsoleSystem.Arg arg);
    [ConsoleCommand("command6")]
     void cmdConsoleCommand6(ConsoleSystem.Arg arg);
    [ConsoleCommand("command7")]
     void cmdConsoleCommand7(ConsoleSystem.Arg arg);
    [ConsoleCommand("command8")]
     void cmdConsoleCommand8(ConsoleSystem.Arg arg);
    [ConsoleCommand("commandNext")]
     void cmdConsoleCommandNext(ConsoleSystem.Arg arg);
    [ConsoleCommand("commandBack")]
     void cmdConsoleCommandBack(ConsoleSystem.Arg arg);
    [ConsoleCommand("commandDisable")]
     void cmdConsoleCommandDisable(ConsoleSystem.Arg arg);
    private void OnPlayerInit(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void Unload();
    private void DestroyAll(BasePlayer player);
    private void DestroyCrosshair(BasePlayer player);
    private void DestroyGUImenu(BasePlayer player);
    private void EnabledCrosshair(BasePlayer player);
    private void DisabledCrosshair(BasePlayer player);
    private void EnabledMenu(BasePlayer player);
    private void DisabledMenu(BasePlayer player);
    private void Crosshair1(BasePlayer player);
    private void Crosshair2(BasePlayer player);
    private void Crosshair3(BasePlayer player);
    private void Crosshair4(BasePlayer player);
    private void Crosshair5(BasePlayer player);
    private void Crosshair6(BasePlayer player);
    private void Crosshair7(BasePlayer player);
    private void Crosshair8(BasePlayer player);
    private void ShowMenu(BasePlayer player, string text);
    private void NextMenu(BasePlayer player, string text);
     string Lang(string key, string id, object[] args);
     void Reply(BasePlayer player, string message, string args);
     bool IsAllowed(string id, string perm);
}


```

---

## ShowHealth

```csharp
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("ShowHealth", "Troubled", "0.2.1")]
[Description("Check any player's health")]
 class ShowHealth : RustPlugin
{
     void Init();
     void LoadDefaultMessages();
    [ChatCommand("hp")]
     void Health(BasePlayer player, string command, string[] args);
     string Lang(string key, string userId);
     bool IsAllowed(BasePlayer player, string perm);
}


```

---

## ShowHitInfo

```csharp
using Rust;
using System;
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[Info("ShowHitInfo", "nomizzz", "1.1.0")]
[Description("Displays the amount of damage a player deals upon hitting another entity to the player's chat log.")]
 class ShowHitInfo : RustPlugin
{
     HashSet<ulong> users;
     void Unload();
     void OnServerSave();
    private void OnEntityTakeDamage(BaseCombatEntity victim, HitInfo hitInfo);
     void Loaded();
     string GetMessage(string name, string sid);
     void SaveData();
     void LoadSavedData();
     void LoadPermissions();
     bool IsAllowed(BasePlayer player);
    [ChatCommand("hitinfo")]
     void ToggleHitInfo(BasePlayer player, string cmd, string[] args);
}


```

---

## SightsSystem

```csharp
using System.Linq;
using System.Collections.Generic;
using Oxide.Core;
using UnityEngine;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("SightsSystem", "Chibubrik", "1.1.0")]
 class SightsSystem : RustPlugin
{
     string Layer;
    private string MainIMG;
    [PluginReference]
     Plugin ImageLibrary;
    public Dictionary<ulong, string> DB;
     void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void Unload();
     void SaveDataBase();
     List<string> Hair;
    [ChatCommand("hair")]
     void ChatHair(BasePlayer player);
    [ConsoleCommand("hair")]
     void ConsoleHair(ConsoleSystem.Arg args);
    [ConsoleCommand("closeui")]
     void closeui228(ConsoleSystem.Arg args);
     void SightsUI(BasePlayer player);
     void InterfaceUI(BasePlayer player);
     void HairUI(BasePlayer player);
    private static string HexToRustFormat(string hex);
}


```

---

## SignArtist

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.IO;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Sign Artist", "Bombardir", "0.3.2", ResourceId = 992)]
 class SignArtist : RustPlugin
{
     GameObject WebObject;
     UnityWeb UWeb;
     Dictionary<BasePlayer, float> CoolDowns;
     class QueueItem
    {
        public string url;
        public Signage sign;
        public BasePlayer sender;
        public bool raw;
        public QueueItem(string ur, BasePlayer se, Signage si, bool raw);
    }

     class UnityWeb : MonoBehaviour
    {
        private Queue<QueueItem> QueueList;
        private byte ActiveLoads;
        private SignArtist SignArtist;
        private void Awake();
        private void OnDestroy();
        public void Add(string url, BasePlayer player, Signage s, bool raw);
         void Next();
         byte[] GetImageBytes(WWW www);
         IEnumerator WaitForRequest(QueueItem info);
    }

    [ConsoleCommand("sil")]
     void ccmdSil(ConsoleSystem.Arg arg);
    [ChatCommand("sil")]
     void sil(BasePlayer player, string command, string[] args);
    [ConsoleCommand("silt")]
     void ccmdSilt(ConsoleSystem.Arg arg);
    [ChatCommand("silt")]
     void silt(BasePlayer player, string command, string[] args);
     float MaxDist;
     float UrlCooldown;
     uint MaxSize;
     byte JPGCompression;
     bool ForceJPG;
     string NoPerm;
     string Syntax;
     string NoSignFound;
     string NotYourSign;
     string CooldownMsg;
     string AddedToQueue;
     string Loaded;
     string Error;
     string NotExists;
     string SizeError;
     bool ConsoleLog;
     string ConsoleLogMsg;
     int MaxActiveLoads;
     void LoadDefaultConfig();
     void OnServerInitialized();
     void Unload();
     void CheckCfg(string Key, T var);
     bool HasPerm(BasePlayer p, string pe);
    static string ToReadableString(float seconds);
}

 class QueueItem
{
    public string url;
    public Signage sign;
    public BasePlayer sender;
    public bool raw;
    public QueueItem(string ur, BasePlayer se, Signage si, bool raw);
}

 class UnityWeb : MonoBehaviour
{
    private Queue<QueueItem> QueueList;
    private byte ActiveLoads;
    private SignArtist SignArtist;
    private void Awake();
    private void OnDestroy();
    public void Add(string url, BasePlayer player, Signage s, bool raw);
     void Next();
     byte[] GetImageBytes(WWW www);
     IEnumerator WaitForRequest(QueueItem info);
}


```

---

## SignManager

```csharp
using System.Collections.Generic;
using Oxide.Core;
using UnityEngine;
using Oxide.Game.Rust.Cui;
using System.Linq;
using System;
using System.Drawing;
using System.IO;

Oxide.Plugins
[Info("SignManager", "Sempai#3239", "1.0.2", ResourceId = 0)]
[Description("User/Admin image management plugin.")]
 class SignManager : RustPlugin
{
    const string Font;
     string sprite;
     List<ulong> Buffer;
    public List<BaseEntity> signs;
     Dictionary<ulong, BaseEntity> Back;
     bool exists(uint ID, uint texture);
     bool loaded;
     void OnServerInitialized();
    private void OnServerSave();
     void OnPlayerConnected(BasePlayer player);
     void OnSignUpdated(Signage sign, BasePlayer player);
     void OnSignUpdated(PhotoFrame frame, BasePlayer player);
     void OnEntitySpawned(Signage sign);
     void OnEntitySpawned(PhotoFrame sign);
     void OnEntityDeath(Signage sign);
     void OnEntityDeath(PhotoFrame sign);
     void OnEntityKill(Signage sign);
     void OnEntityKill(PhotoFrame sign);
     void DoSign(BaseEntity e, BasePlayer player);
     void AddSign(BaseEntity e);
     void RemoveSign(BaseEntity sign);
     bool SavePlayerSign(BasePlayer player, BaseEntity e, bool drawn, int layer, bool p);
     uint[] ReplaceSign(uint ID, uint entity, uint server, uint layer);
     bool RestoreSign(BasePlayer player, BaseEntity e, uint ID, uint layer);
     BaseEntity GetClosestSign(BasePlayer player);
     void Import();
     void Export();
     void MsgUI(BasePlayer player, string message);
     void Unload();
     void OnPlayerDisconnected(BasePlayer player);
     void DestroyMenu(BasePlayer player);
     void SMUI(BasePlayer player);
     bool GotSign(ulong playerid, uint id);
    [ConsoleCommand("SM")]
    private void SMCmd(ConsoleSystem.Arg arg);
     void SMAUI(BasePlayer player, BaseEntity sign, ulong userID);
    [ConsoleCommand("SMA")]
    private void SMACmd(ConsoleSystem.Arg arg);
    const string permAdmin;
    const string permAuto;
    const string permManual;
     bool HasPermission(string id, string perm);
    readonly Dictionary<string, string> Messages;
     void Init();
    [ChatCommand("sm")]
     void SM(BasePlayer player, string command, string[] args);
    [ChatCommand("sma")]
     void SMA(BasePlayer player, string command, string[] args);
     StoredData storedData;
     class StoredData
    {
        public Dictionary<uint, List<SignData>> Signs;
        public Dictionary<ulong, List<SignData>> UserID;
        public Dictionary<uint, List<ExportInfo>> Export;
    }

    public class ExportInfo
    {
        public byte[] data;
        public UInt32 layer;
    }

    public class SignData
    {
        public uint PermaCRC;
        public uint InsCRC;
        public UInt32 layer;
        public DateTime time;
        public ulong creator;
    }

     void Loaded();
     void SaveData();
    private ConfigData configData;
     class ConfigData
    {
        public string CommandAlias;
        public string AdminCommandAlias;
        public string ButtonColour;
    }

    private bool LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConf();
}

 class StoredData
{
    public Dictionary<uint, List<SignData>> Signs;
    public Dictionary<ulong, List<SignData>> UserID;
    public Dictionary<uint, List<ExportInfo>> Export;
}

public class ExportInfo
{
    public byte[] data;
    public UInt32 layer;
}

public class SignData
{
    public uint PermaCRC;
    public uint InsCRC;
    public UInt32 layer;
    public DateTime time;
    public ulong creator;
}

 class ConfigData
{
    public string CommandAlias;
    public string AdminCommandAlias;
    public string ButtonColour;
}


```

---

## SimpleColouredNameAndChat

```csharp
using UnityEngine;
using Rust;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("SimpleColouredNameAndChat", "Steven", "1.0.0", ResourceId = 8909)]
 class SimpleColouredNameAndChat : RustPlugin
{
     ulong[] IDS;
     string[] NameColours;
     string[] TextColours;
     string GetPlayerColour(string name, ulong id);
     string GetPlayerTextColour(string msg, ulong id);
    [HookMethod("OnPlayerChat")]
     object OnPlayerChat(ConsoleSystem.Arg arg);
}


```

---

## SimpleKDR

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System.Collections.Generic;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("SimpleKDR", "Sempai#3239", "3.0.01")]
[Description("Simple KDR system for pvp servers")]
public class SimpleKDR : RustPlugin
{
    static SimpleKDR _instance;
     CuiElementContainer mainContainer;
     CuiElementContainer killText;
     CuiElementContainer baseUi;
    [PluginReference]
     Plugin ImageLibrary;
    private void Init();
     void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void Unload();
     void OnServerSave();
     void OnNewSave();
     void OnEntityDeath(BasePlayer player, HitInfo info);
    private Dictionary<BasePlayer, BehaviorScript> _monoBehavior;
    private void PlayerComponent(bool add, BasePlayer player);
    private class BehaviorScript : FacepunchBehaviour
    {
         BasePlayer player;
         void Awake();
        public void ShowKillMsg(BasePlayer _player);
         void DestroyUI();
    }

    [ConsoleCommand("kdr_wipe")]
    private void simplekdr_wipedata(ConsoleSystem.Arg arg);
     void ChatCommand(BasePlayer player, string command, string[] args);
    private void createBaseUI();
     void createKillText();
     void createMainContainer();
    private string Img(string link);
    private void RunMono(BasePlayer player);
     string InsertData(BasePlayer player, string ui);
    private void DestroyPanels(BasePlayer player);
    private void SaveData();
    private Dictionary<ulong, PlayerData> playerData;
    private class PlayerData
    {
        public int kills;
        public int deaths;
        public string name;
    }

    private void LoadData();
    public class CUIClass
    {
        public static void CreatePanel(CuiElementContainer _container, string _name, string _parent, string _color, string _anchorMin, string _anchorMax, bool _cursorOn, float _fade, string _mat2);
        public static void CreateImage(CuiElementContainer _container, string _parent, string _image, string _anchorMin, string _anchorMax, float _fade);
        public static void CreateText(CuiElementContainer _container, string _name, string _parent, string _color, string _text, int _size, string _anchorMin, string _anchorMax, TextAnchor _align, string _font, string _outlineColor, string _outlineScale);
    }

    private Configuration config;
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     class Configuration
    {
        [JsonProperty(PropertyName = "Main Settings")]
        public MS ms { get; set; }
        public class MS
        {
            [JsonProperty("Reset data on wipe")]
            public bool wipeReset { get; set; }
            [JsonProperty("Count NPC kills")]
            public bool countNpc { get; set; }
            [JsonProperty("Count suicides")]
            public bool countSuicide { get; set; }
            [JsonProperty("Chat command to hide ui")]
            public string hideCmd { get; set; }
        }

        [JsonProperty(PropertyName = "Main Ui Container (used to position all panels at once)")]
        public UiC uic { get; set; }
        public class UiC
        {
            [JsonProperty("Anchor Min")]
            public string anchorMin { get; set; }
            [JsonProperty("Anchor Max")]
            public string anchorMax { get; set; }
            [JsonProperty("Offset Min")]
            public string offsetMin { get; set; }
            [JsonProperty("Offset Max")]
            public string offsetMax { get; set; }
        }

        [JsonProperty(PropertyName = "Kills Panel")]
        public PanelKills panelKills { get; set; }
        public class PanelKills
        {
            [JsonProperty("Enabled")]
            public bool panelEnabled { get; set; }
            [JsonProperty("Panel Color")]
            public string panelColor { get; set; }
            [JsonProperty("Img")]
            public string panelIcon { get; set; }
            [JsonProperty("Text")]
            public string panelText { get; set; }
            [JsonProperty("Font")]
            public string panelFont { get; set; }
            [JsonProperty("Text Outline Thickness")]
            public string panelFontOut { get; set; }
            [JsonProperty("Text Outline Color")]
            public string panelFontOutColor { get; set; }
            [JsonProperty("Anchor Min")]
            public string panelAnchorMin { get; set; }
            [JsonProperty("Anchor Max")]
            public string panelAnchorMax { get; set; }
        }

        [JsonProperty(PropertyName = "Deaths Panel")]
        public PanelDeaths panelDeaths { get; set; }
        public class PanelDeaths
        {
            [JsonProperty("Enabled")]
            public bool panelEnabled { get; set; }
            [JsonProperty("Panel Color")]
            public string panelColor { get; set; }
            [JsonProperty("Img")]
            public string panelIcon { get; set; }
            [JsonProperty("Text")]
            public string panelText { get; set; }
            [JsonProperty("Font")]
            public string panelFont { get; set; }
            [JsonProperty("Text Outline Thickness")]
            public string panelFontOut { get; set; }
            [JsonProperty("Text Outline Color")]
            public string panelFontOutColor { get; set; }
            [JsonProperty("Anchor Min")]
            public string panelAnchorMin { get; set; }
            [JsonProperty("Anchor Max")]
            public string panelAnchorMax { get; set; }
        }

        [JsonProperty(PropertyName = "Ratio Panel")]
        public PanelRatio panelRatio { get; set; }
        public class PanelRatio
        {
            [JsonProperty("Enabled")]
            public bool panelEnabled { get; set; }
            [JsonProperty("Panel Color")]
            public string panelColor { get; set; }
            [JsonProperty("Img")]
            public string panelIcon { get; set; }
            [JsonProperty("Text")]
            public string panelText { get; set; }
            [JsonProperty("Font")]
            public string panelFont { get; set; }
            [JsonProperty("Text Outline Thickness")]
            public string panelFontOut { get; set; }
            [JsonProperty("Text Outline Color")]
            public string panelFontOutColor { get; set; }
            [JsonProperty("Anchor Min")]
            public string panelAnchorMin { get; set; }
            [JsonProperty("Anchor Max")]
            public string panelAnchorMax { get; set; }
        }

        [JsonProperty(PropertyName = "Pop up notification on kill")]
        public KillPopUp killPopUp { get; set; }
        public class KillPopUp
        {
            [JsonProperty("Enabled")]
            public bool panelEnabled { get; set; }
            [JsonProperty("Text")]
            public string panelText { get; set; }
            [JsonProperty("Font")]
            public string panelFont { get; set; }
            [JsonProperty("Text Outline Thickness")]
            public string panelFontOut { get; set; }
            [JsonProperty("Text Outline Color")]
            public string panelFontOutColor { get; set; }
            [JsonProperty("Anchor Min")]
            public string panelAnchorMin { get; set; }
            [JsonProperty("Anchor Max")]
            public string panelAnchorMax { get; set; }
        }

        public static Configuration CreateConfig();
    }

    protected override void LoadDefaultMessages();
    private string GetLang(string _message);
}

private class BehaviorScript : FacepunchBehaviour
{
     BasePlayer player;
     void Awake();
    public void ShowKillMsg(BasePlayer _player);
     void DestroyUI();
}

private class PlayerData
{
    public int kills;
    public int deaths;
    public string name;
}

public class CUIClass
{
    public static void CreatePanel(CuiElementContainer _container, string _name, string _parent, string _color, string _anchorMin, string _anchorMax, bool _cursorOn, float _fade, string _mat2);
    public static void CreateImage(CuiElementContainer _container, string _parent, string _image, string _anchorMin, string _anchorMax, float _fade);
    public static void CreateText(CuiElementContainer _container, string _name, string _parent, string _color, string _text, int _size, string _anchorMin, string _anchorMax, TextAnchor _align, string _font, string _outlineColor, string _outlineScale);
}

 class Configuration
{
    [JsonProperty(PropertyName = "Main Settings")]
    public MS ms { get; set; }
    public class MS
    {
        [JsonProperty("Reset data on wipe")]
        public bool wipeReset { get; set; }
        [JsonProperty("Count NPC kills")]
        public bool countNpc { get; set; }
        [JsonProperty("Count suicides")]
        public bool countSuicide { get; set; }
        [JsonProperty("Chat command to hide ui")]
        public string hideCmd { get; set; }
    }

    [JsonProperty(PropertyName = "Main Ui Container (used to position all panels at once)")]
    public UiC uic { get; set; }
    public class UiC
    {
        [JsonProperty("Anchor Min")]
        public string anchorMin { get; set; }
        [JsonProperty("Anchor Max")]
        public string anchorMax { get; set; }
        [JsonProperty("Offset Min")]
        public string offsetMin { get; set; }
        [JsonProperty("Offset Max")]
        public string offsetMax { get; set; }
    }

    [JsonProperty(PropertyName = "Kills Panel")]
    public PanelKills panelKills { get; set; }
    public class PanelKills
    {
        [JsonProperty("Enabled")]
        public bool panelEnabled { get; set; }
        [JsonProperty("Panel Color")]
        public string panelColor { get; set; }
        [JsonProperty("Img")]
        public string panelIcon { get; set; }
        [JsonProperty("Text")]
        public string panelText { get; set; }
        [JsonProperty("Font")]
        public string panelFont { get; set; }
        [JsonProperty("Text Outline Thickness")]
        public string panelFontOut { get; set; }
        [JsonProperty("Text Outline Color")]
        public string panelFontOutColor { get; set; }
        [JsonProperty("Anchor Min")]
        public string panelAnchorMin { get; set; }
        [JsonProperty("Anchor Max")]
        public string panelAnchorMax { get; set; }
    }

    [JsonProperty(PropertyName = "Deaths Panel")]
    public PanelDeaths panelDeaths { get; set; }
    public class PanelDeaths
    {
        [JsonProperty("Enabled")]
        public bool panelEnabled { get; set; }
        [JsonProperty("Panel Color")]
        public string panelColor { get; set; }
        [JsonProperty("Img")]
        public string panelIcon { get; set; }
        [JsonProperty("Text")]
        public string panelText { get; set; }
        [JsonProperty("Font")]
        public string panelFont { get; set; }
        [JsonProperty("Text Outline Thickness")]
        public string panelFontOut { get; set; }
        [JsonProperty("Text Outline Color")]
        public string panelFontOutColor { get; set; }
        [JsonProperty("Anchor Min")]
        public string panelAnchorMin { get; set; }
        [JsonProperty("Anchor Max")]
        public string panelAnchorMax { get; set; }
    }

    [JsonProperty(PropertyName = "Ratio Panel")]
    public PanelRatio panelRatio { get; set; }
    public class PanelRatio
    {
        [JsonProperty("Enabled")]
        public bool panelEnabled { get; set; }
        [JsonProperty("Panel Color")]
        public string panelColor { get; set; }
        [JsonProperty("Img")]
        public string panelIcon { get; set; }
        [JsonProperty("Text")]
        public string panelText { get; set; }
        [JsonProperty("Font")]
        public string panelFont { get; set; }
        [JsonProperty("Text Outline Thickness")]
        public string panelFontOut { get; set; }
        [JsonProperty("Text Outline Color")]
        public string panelFontOutColor { get; set; }
        [JsonProperty("Anchor Min")]
        public string panelAnchorMin { get; set; }
        [JsonProperty("Anchor Max")]
        public string panelAnchorMax { get; set; }
    }

    [JsonProperty(PropertyName = "Pop up notification on kill")]
    public KillPopUp killPopUp { get; set; }
    public class KillPopUp
    {
        [JsonProperty("Enabled")]
        public bool panelEnabled { get; set; }
        [JsonProperty("Text")]
        public string panelText { get; set; }
        [JsonProperty("Font")]
        public string panelFont { get; set; }
        [JsonProperty("Text Outline Thickness")]
        public string panelFontOut { get; set; }
        [JsonProperty("Text Outline Color")]
        public string panelFontOutColor { get; set; }
        [JsonProperty("Anchor Min")]
        public string panelAnchorMin { get; set; }
        [JsonProperty("Anchor Max")]
        public string panelAnchorMax { get; set; }
    }

    public static Configuration CreateConfig();
}

public class MS
{
    [JsonProperty("Reset data on wipe")]
    public bool wipeReset { get; set; }
    [JsonProperty("Count NPC kills")]
    public bool countNpc { get; set; }
    [JsonProperty("Count suicides")]
    public bool countSuicide { get; set; }
    [JsonProperty("Chat command to hide ui")]
    public string hideCmd { get; set; }
}

public class UiC
{
    [JsonProperty("Anchor Min")]
    public string anchorMin { get; set; }
    [JsonProperty("Anchor Max")]
    public string anchorMax { get; set; }
    [JsonProperty("Offset Min")]
    public string offsetMin { get; set; }
    [JsonProperty("Offset Max")]
    public string offsetMax { get; set; }
}

public class PanelKills
{
    [JsonProperty("Enabled")]
    public bool panelEnabled { get; set; }
    [JsonProperty("Panel Color")]
    public string panelColor { get; set; }
    [JsonProperty("Img")]
    public string panelIcon { get; set; }
    [JsonProperty("Text")]
    public string panelText { get; set; }
    [JsonProperty("Font")]
    public string panelFont { get; set; }
    [JsonProperty("Text Outline Thickness")]
    public string panelFontOut { get; set; }
    [JsonProperty("Text Outline Color")]
    public string panelFontOutColor { get; set; }
    [JsonProperty("Anchor Min")]
    public string panelAnchorMin { get; set; }
    [JsonProperty("Anchor Max")]
    public string panelAnchorMax { get; set; }
}

public class PanelDeaths
{
    [JsonProperty("Enabled")]
    public bool panelEnabled { get; set; }
    [JsonProperty("Panel Color")]
    public string panelColor { get; set; }
    [JsonProperty("Img")]
    public string panelIcon { get; set; }
    [JsonProperty("Text")]
    public string panelText { get; set; }
    [JsonProperty("Font")]
    public string panelFont { get; set; }
    [JsonProperty("Text Outline Thickness")]
    public string panelFontOut { get; set; }
    [JsonProperty("Text Outline Color")]
    public string panelFontOutColor { get; set; }
    [JsonProperty("Anchor Min")]
    public string panelAnchorMin { get; set; }
    [JsonProperty("Anchor Max")]
    public string panelAnchorMax { get; set; }
}

public class PanelRatio
{
    [JsonProperty("Enabled")]
    public bool panelEnabled { get; set; }
    [JsonProperty("Panel Color")]
    public string panelColor { get; set; }
    [JsonProperty("Img")]
    public string panelIcon { get; set; }
    [JsonProperty("Text")]
    public string panelText { get; set; }
    [JsonProperty("Font")]
    public string panelFont { get; set; }
    [JsonProperty("Text Outline Thickness")]
    public string panelFontOut { get; set; }
    [JsonProperty("Text Outline Color")]
    public string panelFontOutColor { get; set; }
    [JsonProperty("Anchor Min")]
    public string panelAnchorMin { get; set; }
    [JsonProperty("Anchor Max")]
    public string panelAnchorMax { get; set; }
}

public class KillPopUp
{
    [JsonProperty("Enabled")]
    public bool panelEnabled { get; set; }
    [JsonProperty("Text")]
    public string panelText { get; set; }
    [JsonProperty("Font")]
    public string panelFont { get; set; }
    [JsonProperty("Text Outline Thickness")]
    public string panelFontOut { get; set; }
    [JsonProperty("Text Outline Color")]
    public string panelFontOutColor { get; set; }
    [JsonProperty("Anchor Min")]
    public string panelAnchorMin { get; set; }
    [JsonProperty("Anchor Max")]
    public string panelAnchorMax { get; set; }
}


```

---

## SimpleKillFeed

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Runtime.Serialization;
using System.Text;
using Facepunch;
using Facepunch.Math;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;
using ProtoBuf;

Oxide.Plugins
[Info("Simple Kill Feed", "Krungh Crow", "2.2.8")]
[Description("A kill feed, that displays various kill events in the top right corner.")]
public class SimpleKillFeed : RustPlugin
{
    [PluginReference]
    private Plugin Clans;
    private bool _isClansReborn;
    private bool _isClansIo;
    private bool _isClansMevent;
    private readonly Dictionary<uint, string> _itemNameMapping;
    private GameObject _holder;
    private KillQueue _killQueue;
    private static SKFConfig configdata;
    private static SimpleKillFeedData _data;
    private class SKFConfig
    {
        [JsonProperty("Show Traps and Entitys in Kill Feed")]
        public bool EnableEntityFeed;
        [JsonProperty("Show Animals kills (default true)")]
        public bool EnableAnimalFeed;
        [JsonProperty("Show Npcs kills (Default true)")]
        public bool EnableNpcFeed;
        [JsonProperty("Show suicides (Default: true)")]
        public bool EnableSuicides;
        [JsonProperty("Show Deaths by Animals (Default: true)")]
        public bool EnableAnimal;
        [JsonProperty("Show Deaths by Cold (Default: true)")]
        public bool EnableCold;
        [JsonProperty("Show deaths by Drowning (Default: true)")]
        public bool EnableDrowning;
        [JsonProperty("Show Deaths by Fall (Default: true)")]
        public bool EnableFall;
        [JsonProperty("Show Deaths by Hunger (Default: true)")]
        public bool EnableHunger;
        [JsonProperty("Show Deaths by Electricution (Default: true)")]
        public bool EnableElectricution;
        [JsonProperty("Show Deaths by Radiation (Default: true)")]
        public bool EnableRadiationKills;
        [JsonProperty("Chat Icon Id (Steam profile ID)")]
        public ulong IconId;
        [JsonProperty("Max messages in feed (Default: 5)")]
        public int MaxFeedMessages;
        [JsonProperty("Max player name length in feed (Default: 18)")]
        public int MaxPlayerNameLength;
        [JsonProperty("Feed message TTL in seconds (Default: 7)")]
        public int FeedMessageTtlSec;
        [JsonProperty("Allow kill messages in chat along with kill feed")]
        public bool EnableChatFeed;
        [JsonProperty("Log PvP Kill events")]
        public bool EnableLogging;
        [JsonProperty("Height ident (space between messages). Default: 0.0185")]
        public float HeightIdent;
        [JsonProperty("Feed Position - Anchor Max. (Default: 0.995 0.986")]
        public string AnchorMax;
        [JsonProperty("Feed Position - Anchor Min. (Default: 0.723 0.964")]
        public string AnchorMin;
        [JsonProperty("Font size of kill feed (Default: 12)")]
        public int FontSize;
        [JsonProperty("Outline Text Size (Default: 0.5 0.5)")]
        public string OutlineSize;
        [JsonProperty("Default color for distance (if too far from any from the list). Default: #FF8000")]
        public string DefaultDistanceColor;
        [JsonProperty("Distance Colors List (Certain color will apply if distance is <= than specified)")]
        public DistanceColor[] DistanceColors;
        [JsonProperty("Custom Entity Names, you can remove or add more!")]
        public Dictionary<string, string> Ents;
        [JsonProperty("Custom Animal Names, you can remove or add more!")]
        public Dictionary<string, string> Animal;
        [JsonProperty("Custom Weapon Names, you can add more!")]
        public Dictionary<string, string> Weapons;
        [JsonProperty("Custom Npc Names, you can add more!")]
        public Dictionary<string, string> Npcs;
        [OnDeserialized]
        internal void OnDeserialized(StreamingContext ctx);
        public class DistanceColor
        {
            public int DistanceThreshold;
            public string Color;
            public bool TestDistance(int distance);
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [ProtoContract(ImplicitFields = ImplicitFields.AllPublic)]
    private class SimpleKillFeedData
    {
        public HashSet<ulong> DisabledUsers;
    }

    private void LoadData();
    private void SaveData();
    [ChatCommand("feed")]
    private void ToggleFeed(BasePlayer player);
    private void OnServerInitialized();
    private void Init();
    private void Unload();
    private void OnEntityDeath(BaseAnimalNPC victim, HitInfo hitInfo);
    private void OnEntityDeath(SimpleShark victim, HitInfo hitInfo);
    private void OnPlayerDeath(BasePlayer victim, HitInfo hitInfo);
    protected override void LoadDefaultMessages();
    private void SendKillfeedmessage(string msg);
    private void OnKilled(BasePlayer attacker, BasePlayer victim, HitInfo hitInfo, int dist);
    private void OnSuicide(BasePlayer victim);
    private void OnKilledByRadiation(BasePlayer victim);
    private void OnKilledByEnt(BaseEntity attacker, BasePlayer victim);
    private void OnKilledByBradley(BasePlayer victim);
    private void OnKilledByAnimal(BaseEntity attacker, BasePlayer victim);
    private void DeathByFall(BasePlayer victim);
    private void OnDeathFromWounds(BasePlayer attacker, BasePlayer victim);
    private void OnBurned(BasePlayer attacker, BasePlayer victim);
    private void OnDrowning(BasePlayer victim);
    private void OnFrozen(BasePlayer victim);
    private void OnHunger(BasePlayer victim);
    private void OnShock(BasePlayer victim);
    private void OnSentry(BasePlayer victim);
    private void OnCactus(BasePlayer victim);
    private class KillEvent : Pool.IPooled
    {
        public int DisplayUntil;
        public string Text;
        public KillEvent Init(string text, int displayUntil);
        public void EnterPool();
        public void LeavePool();
    }

    private class KillQueue : MonoBehaviour
    {
        private readonly WaitForSeconds _secondDelay;
        private readonly Queue<KillEvent> _queue;
        private readonly CuiOutlineComponent _outlineStatic;
        private readonly CuiRectTransformComponent[] _rectTransformStatic;
        private readonly CuiTextComponent[] _textStatic;
        private readonly CuiElementContainer _cui;
        private bool _needsRedraw;
        private int _currentlyDrawn;
        public void OnDeath(BasePlayer victim, BasePlayer attacker, string text);
        private void PushEvent(KillEvent evt);
        private void Start();
        private void DequeueEvent(KillEvent evt);
        private void DoProccessQueue();
        private IEnumerator ProccessQueue();
        private static void SendKillCui(CuiElementContainer cui, int toBeRemoved);
        public static void RemoveKillCui(string name);
    }

    private string _(string msg, BasePlayer player);
    private string GetCustomWeaponName(HitInfo hitInfo);
    private string CustomNpcName(BasePlayer npc);
    private string CustomEntName(BaseEntity attacker);
    private string CustomAnimalName(BaseEntity attacker);
    private string GetWeaponName(HitInfo hitInfo);
    private static bool IsExplosion(HitInfo hit);
    private static bool IsFlame(HitInfo hit);
    private static bool IsRadiation(HitInfo hit);
    private static bool IsCold(HitInfo hit);
    private static bool IsHunger(HitInfo hit);
    private static bool IsThirst(HitInfo hit);
    private static bool IsDrowning(HitInfo hit);
    private static bool IsPoison(HitInfo hit);
    private static bool IsShock(HitInfo hit);
    private static bool IsFall(HitInfo hit);
    private static bool IsCar(HitInfo hit);
    private static bool IsTrap(BaseEntity ent);
    private static bool IsAnimal(BaseEntity animal);
    private static bool IsZombieHorde(BasePlayer player);
    private static string GetDistanceColor(int dist);
    private string GetClan(BasePlayer player);
    private static string SanitizeName(string name);
}

private class SKFConfig
{
    [JsonProperty("Show Traps and Entitys in Kill Feed")]
    public bool EnableEntityFeed;
    [JsonProperty("Show Animals kills (default true)")]
    public bool EnableAnimalFeed;
    [JsonProperty("Show Npcs kills (Default true)")]
    public bool EnableNpcFeed;
    [JsonProperty("Show suicides (Default: true)")]
    public bool EnableSuicides;
    [JsonProperty("Show Deaths by Animals (Default: true)")]
    public bool EnableAnimal;
    [JsonProperty("Show Deaths by Cold (Default: true)")]
    public bool EnableCold;
    [JsonProperty("Show deaths by Drowning (Default: true)")]
    public bool EnableDrowning;
    [JsonProperty("Show Deaths by Fall (Default: true)")]
    public bool EnableFall;
    [JsonProperty("Show Deaths by Hunger (Default: true)")]
    public bool EnableHunger;
    [JsonProperty("Show Deaths by Electricution (Default: true)")]
    public bool EnableElectricution;
    [JsonProperty("Show Deaths by Radiation (Default: true)")]
    public bool EnableRadiationKills;
    [JsonProperty("Chat Icon Id (Steam profile ID)")]
    public ulong IconId;
    [JsonProperty("Max messages in feed (Default: 5)")]
    public int MaxFeedMessages;
    [JsonProperty("Max player name length in feed (Default: 18)")]
    public int MaxPlayerNameLength;
    [JsonProperty("Feed message TTL in seconds (Default: 7)")]
    public int FeedMessageTtlSec;
    [JsonProperty("Allow kill messages in chat along with kill feed")]
    public bool EnableChatFeed;
    [JsonProperty("Log PvP Kill events")]
    public bool EnableLogging;
    [JsonProperty("Height ident (space between messages). Default: 0.0185")]
    public float HeightIdent;
    [JsonProperty("Feed Position - Anchor Max. (Default: 0.995 0.986")]
    public string AnchorMax;
    [JsonProperty("Feed Position - Anchor Min. (Default: 0.723 0.964")]
    public string AnchorMin;
    [JsonProperty("Font size of kill feed (Default: 12)")]
    public int FontSize;
    [JsonProperty("Outline Text Size (Default: 0.5 0.5)")]
    public string OutlineSize;
    [JsonProperty("Default color for distance (if too far from any from the list). Default: #FF8000")]
    public string DefaultDistanceColor;
    [JsonProperty("Distance Colors List (Certain color will apply if distance is <= than specified)")]
    public DistanceColor[] DistanceColors;
    [JsonProperty("Custom Entity Names, you can remove or add more!")]
    public Dictionary<string, string> Ents;
    [JsonProperty("Custom Animal Names, you can remove or add more!")]
    public Dictionary<string, string> Animal;
    [JsonProperty("Custom Weapon Names, you can add more!")]
    public Dictionary<string, string> Weapons;
    [JsonProperty("Custom Npc Names, you can add more!")]
    public Dictionary<string, string> Npcs;
    [OnDeserialized]
    internal void OnDeserialized(StreamingContext ctx);
    public class DistanceColor
    {
        public int DistanceThreshold;
        public string Color;
        public bool TestDistance(int distance);
    }

}

public class DistanceColor
{
    public int DistanceThreshold;
    public string Color;
    public bool TestDistance(int distance);
}

[ProtoContract(ImplicitFields = ImplicitFields.AllPublic)]
private class SimpleKillFeedData
{
    public HashSet<ulong> DisabledUsers;
}

private class KillEvent : Pool.IPooled
{
    public int DisplayUntil;
    public string Text;
    public KillEvent Init(string text, int displayUntil);
    public void EnterPool();
    public void LeavePool();
}

private class KillQueue : MonoBehaviour
{
    private readonly WaitForSeconds _secondDelay;
    private readonly Queue<KillEvent> _queue;
    private readonly CuiOutlineComponent _outlineStatic;
    private readonly CuiRectTransformComponent[] _rectTransformStatic;
    private readonly CuiTextComponent[] _textStatic;
    private readonly CuiElementContainer _cui;
    private bool _needsRedraw;
    private int _currentlyDrawn;
    public void OnDeath(BasePlayer victim, BasePlayer attacker, string text);
    private void PushEvent(KillEvent evt);
    private void Start();
    private void DequeueEvent(KillEvent evt);
    private void DoProccessQueue();
    private IEnumerator ProccessQueue();
    private static void SendKillCui(CuiElementContainer cui, int toBeRemoved);
    public static void RemoveKillCui(string name);
}


```

---

## SimpleMapKeeper

```csharp
using Oxide.Core;
using System;
using System.Linq;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("SimpleMapKeeper", "CARNY666", "1.1.0", ResourceId = 1579)]
 class SimpleMapKeeper : RustPlugin
{
    private Dictionary<BasePlayer, Item> mapStorage;
    private void LoadDefaultMessages();
    private string GetMessage(string key, string steamId);
    private void Init();
    private void OnPlayerSleepEnded(BasePlayer player);
    private void OnItemAddedToContainer(ItemContainer container, Item item);
    private void Loaded();
    [ChatCommand("simplemap")]
    private void simplemap(BasePlayer player, string command, string[] args);
    private void SaveAllPlayerMaps();
    private bool playerAlreadyHasMap(BasePlayer player);
    private bool saveMap(BasePlayer player);
    private bool restoreMap(BasePlayer player);
}


```

---

## SimpleScan-1.0.0

```csharp
using UnityEngine;
using System.Collections.Generic;

Oxide.Plugins
[Info("Simple Scan", "walkinrey", "1.0.0")]
 class SimpleScan : RustPlugin
{
    [ChatCommand("simplescan")]
     void ScanCommand(BasePlayer player);
     void Init();
     string IsKeyLockLocked(KeyLock locker);
     string IsCodeLockLocked(CodeLock codeLock);
     string isOnline(ulong id);
     ulong GetOwner(BaseEntity entity);
}


```

---

## SkillSystem

```csharp
using System;
using System.Reflection;
using System.Collections.Generic;
using System.Collections;
using System.Linq;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using System.Globalization;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("SkillSystem", "TopPlugin.ru", "1.1.3")]
public class SkillSystem : RustPlugin
{
    public class classSkillParameters
    {
        [JsonProperty("Количество XP(%)")]
        public double countXP;
        [JsonProperty("Количество DNK")]
        public float countDNK;
    }

    public class PlayerSkillData
    {
        [JsonProperty("Название")]
        public string Name;
        [JsonProperty("Описание")]
        public string Description;
        [JsonProperty("Shortname на который будет действовать увеличенный рейтинг скилла")]
        public List<string> ShortnameList;
        [JsonProperty("Рейты игрока ( ПРИБАВЛЯЮТСЯ % ОТ ДОБЫТОГО ИМ РЕСУРСА)")]
        public int Rate;
        [JsonProperty("Сколько % прибавлять за уровень")]
        public int PercentIncrement;
        [JsonProperty("Сколько количество жизней будет увеличиваться на XRate")]
        public double HPIncrementInRate;
        [JsonProperty("Сколько максимум хп игрок получит от регенерации")]
        public int MaxRegen;
        [JsonProperty("Максимальная защита от способности")]
        public int MaxProtection;
        [JsonProperty("Максимальный урон от способности")]
        public int MaxAtack;
        [JsonProperty("Уровень способности")]
        public int Level;
        [JsonProperty("Сколько возвращать DNK при откате способности(0-100%)")]
        public int ReturnPercent;
    }

    public static string CUILayerName;
    public static string DescLayerName;
    public static string MessageUILayerName;
    public static string UIName;
    private Dictionary<uint, BasePlayer> BradleyAtackerList;
    private Dictionary<uint, BasePlayer> HeliAtackerList;
    public Dictionary<ulong, classSkillParameters> PlayersSkillParameters;
    public Dictionary<ulong, List<PlayerSkillData>> PlayersSkillDataMassiv;
    private static DefaultSettings MainGUISettings;
     List<PlayerSkillData> DefaultPlayersSkillData();
    private class DefaultSettings
    {
        public class classSkill
        {
            [JsonProperty("Название", Order = 0)]
            public string Name;
            [JsonProperty("Описание", Order = 1)]
            public string Description;
            [JsonProperty("Shortname на который будет действовать увеличенный рейтинг скилла", Order = 2)]
            public List<string> ShortnameList;
            [JsonProperty("Рейты игрока ( ПРИБАВЛЯЮТСЯ % ОТ ДОБЫТОГО ИМ РЕСУРСА)", Order = 3)]
            public int Rate;
            [JsonProperty("Сколько % прибавлять за уровень", Order = 4)]
            public int PercentIncrement;
            [JsonProperty("Сколько количество жизней будет увеличиваться на XRate", Order = 5)]
            public double HPIncrementInRate;
            [JsonProperty("Сколько максимум хп игрок получит от регенерации", Order = 6)]
            public int MaxRegen;
            [JsonProperty("Максимальная защита от способности", Order = 7)]
            public int MaxProtection;
            [JsonProperty("Максимальный урон от способности", Order = 8)]
            public int MaxAtack;
            [JsonProperty("Максимальный уровень", Order = 9)]
            public int MaxLevel;
            [JsonProperty("Сколько возвращать DNK при откате способности(0-100%)", Order = 10)]
            public int ReturnPercent;
        }

        [JsonProperty("Команда для открытия системы улучшения", Order = 1)]
        public string CommandName;
        [JsonProperty("Предметы-исключения(если игрок добаывает этим предметом , опыт не зачисляется)", Order = 3)]
        public List<string> ItemExceptionList;
        [JsonProperty("Настройка способностей", Order = 2)]
        public List<classSkill> configSkillSettings;
        internal class classSkillSettings
        {
            [JsonProperty("Сколько опыта давать за убийство животных", Order = 5)]
            public double xpForKillAnimal;
            [JsonProperty("Сколько опыта давать за убийство NPC", Order = 2)]
            public double xpForKillNPC;
            [JsonProperty("Сколько опыта давать за убийство игрока в голову", Order = 4)]
            public double xpForKillHeadshot;
            [JsonProperty("Сколько опыта давать за убийство игрока", Order = 3)]
            public double xpForKillPlayer;
            [JsonProperty("Сколько опыта давать за добычу ресурсов", Order = 0)]
            public double xpForResourceExtraction;
            [JsonProperty("Сколько опыта давать за сбитие чинука", Order = 8)]
            public double xpForKillChinuk;
            [JsonProperty("Сколько опыта давать за разбитие бочек", Order = 1)]
            public double xpForBreakBarrels;
            [JsonProperty("Сколько опыта давать за взрыв танка", Order = 7)]
            public double xpForKillBredley;
            [JsonProperty("Сколько опыта давать за сбитие вертолета", Order = 6)]
            public double xpForKillHely;
        }

        [JsonProperty("Настройка опыта за действия", Order = 0)]
        public classSkillSettings xpSkillSettingsConfig;
        [JsonProperty("Сколько DNK давать за новый уровень", Order = 0)]
        public int countDNKForNewLevel;
        public static DefaultSettings SetDefaultSettings();
        [JsonProperty("Настройка UI", Order = 1)]
        public classGUISettings GUISettings;
        internal class classGUISettings
        {
            [JsonProperty("Лого в меню способностей (PNG)", Order = 0)]
            public string BGImage;
            [JsonProperty("Цвет кнопки + (HEX)", Order = 1)]
            public string ButtonPlusColorHEX;
            [JsonProperty("Цвет панели с действием над скиллом ( + - ) (HEX)", Order = 2)]
            public string ActionColorHEX;
            [JsonProperty("Цвет кнопки - (HEX)", Order = 3)]
            public string ButtonMinusColorHEX;
            [JsonProperty("Цвет заднего фона в меню способностей", Order = 4)]
            public string BackgroundColor;
            [JsonProperty("Текст в меню способностей", Order = 5)]
            public string TitleText;
            [JsonProperty("Цвет заднего фона способности", Order = 6)]
            public string SkillBackgroundColor;
            [JsonProperty("Цвет кнопки открывающей описание", Order = 7)]
            public string ButtonInfoColorHEX;
            [JsonProperty("Текст <Очки ДНК>", Order = 8)]
            public string DNKText;
            internal class Progress
            {
                [JsonProperty("AnchorMin прогресс бара", Order = 0)]
                public string AnchorMin;
                [JsonProperty("AnchorMax прогресс бара", Order = 1)]
                public string AnchorMax;
                [JsonProperty("OffsetMin прогресс бара", Order = 2)]
                public string OffsetMin;
                [JsonProperty("OffsetMax прогресс бара", Order = 3)]
                public string OffsetMax;
                [JsonProperty("Цвет заполняющейся полосы (HEX)", Order = 4)]
                public string ProgressBGColor;
            }

            [JsonProperty("Настройка UI-прогресс-бара", Order = 7)]
            public Progress ProgressSettings;
        }

        [JsonProperty("Сообщение при 100% опыта", Order = 2)]
        public string LevelupText;
    }

    protected override void SaveConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    [Oxide.Core.Plugins.HookMethod("OnDispenserGather")]
     void HookOnDisoenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void DispenserGatherFunc(ResourceDispenser dispenser, BaseEntity entity, Item item);
    [Oxide.Core.Plugins.HookMethod("OnDispenserBonus")]
     object MyHookDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     object DispenserBonusFunc(ResourceDispenser dispenser, BasePlayer player, Item item);
     void OnCollectiblePickup(Item item, BasePlayer player, CollectibleEntity entity);
    [Oxide.Core.Plugins.HookMethod("OnPlayerRespawned")]
     void myHookOnPlayerRespawn(BasePlayer player);
    private void AddHealth(BasePlayer player);
     object OnPlayerHealthChange(BasePlayer player, float oldValue, float newValue);
    [Oxide.Core.Plugins.HookMethod("OnEntityDeath")]
     void OnDeadEntity(BaseCombatEntity entity, HitInfo info);
     void EntityDeathFunc(BaseCombatEntity entity, HitInfo info);
    [Oxide.Core.Plugins.HookMethod("OnEntityTakeDamage")]
     void MyHookTakeDamage(BaseCombatEntity entity, HitInfo info);
     void TakeDamageFunc(BaseCombatEntity entity, HitInfo info);
    [Oxide.Core.Plugins.HookMethod("OnServerInitialized")]
     void MyHookServerInit();
    private void InitSkillSystem();
    [Oxide.Core.Plugins.HookMethod("Unload")]
     void MyHookUnload();
     void UnloadFunc();
    [Oxide.Core.Plugins.HookMethod("OnServerSave")]
     void MyHookOnServerSave();
     void ServerSaveFunc();
    [Oxide.Core.Plugins.HookMethod("OnPlayerConnected")]
     void MyHookPlayerInit(BasePlayer player);
    public void PlayerInitFunc(BasePlayer player);
    public void ShowMainUI(BasePlayer player);
     void ShowXPProgressGUI(BasePlayer player);
    public void GetXP(BasePlayer player, double param);
    private IEnumerator ShowSkillGUI(BasePlayer player, CuiElementContainer container);
    private IEnumerator UnloadUI(BasePlayer player, CuiElementContainer container, int element);
     Plugin ImageLibrary { get; set; }
    public bool AddImage(string url, string shortname, ulong skin);
    public string GetImage(string Name, ulong l);
    private static string getHEXColor(string colorValue);
    [ConsoleCommand("plussbtn")]
     void cmdPlusBtnPress(ConsoleSystem.Arg Argum);
    public void cmdPlusLevel(BasePlayer player, int element);
    [ConsoleCommand("skillpoint")]
     void GiveSkillPointConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("minusbtn")]
     void cmdMinusBtnPress(ConsoleSystem.Arg Argum);
    public void cmdMinusLevel(BasePlayer player, int element);
     void cmdShowSkillWindow(BasePlayer player);
    [ConsoleCommand("opendescription")]
     void cmdOpenDescription(ConsoleSystem.Arg Args);
    [ConsoleCommand("open_dnk_system")]
     void cmdOpenDNKPanel(ConsoleSystem.Arg Argum);
     void Log(string filename, string text);
}

public class classSkillParameters
{
    [JsonProperty("Количество XP(%)")]
    public double countXP;
    [JsonProperty("Количество DNK")]
    public float countDNK;
}

public class PlayerSkillData
{
    [JsonProperty("Название")]
    public string Name;
    [JsonProperty("Описание")]
    public string Description;
    [JsonProperty("Shortname на который будет действовать увеличенный рейтинг скилла")]
    public List<string> ShortnameList;
    [JsonProperty("Рейты игрока ( ПРИБАВЛЯЮТСЯ % ОТ ДОБЫТОГО ИМ РЕСУРСА)")]
    public int Rate;
    [JsonProperty("Сколько % прибавлять за уровень")]
    public int PercentIncrement;
    [JsonProperty("Сколько количество жизней будет увеличиваться на XRate")]
    public double HPIncrementInRate;
    [JsonProperty("Сколько максимум хп игрок получит от регенерации")]
    public int MaxRegen;
    [JsonProperty("Максимальная защита от способности")]
    public int MaxProtection;
    [JsonProperty("Максимальный урон от способности")]
    public int MaxAtack;
    [JsonProperty("Уровень способности")]
    public int Level;
    [JsonProperty("Сколько возвращать DNK при откате способности(0-100%)")]
    public int ReturnPercent;
}

private class DefaultSettings
{
    public class classSkill
    {
        [JsonProperty("Название", Order = 0)]
        public string Name;
        [JsonProperty("Описание", Order = 1)]
        public string Description;
        [JsonProperty("Shortname на который будет действовать увеличенный рейтинг скилла", Order = 2)]
        public List<string> ShortnameList;
        [JsonProperty("Рейты игрока ( ПРИБАВЛЯЮТСЯ % ОТ ДОБЫТОГО ИМ РЕСУРСА)", Order = 3)]
        public int Rate;
        [JsonProperty("Сколько % прибавлять за уровень", Order = 4)]
        public int PercentIncrement;
        [JsonProperty("Сколько количество жизней будет увеличиваться на XRate", Order = 5)]
        public double HPIncrementInRate;
        [JsonProperty("Сколько максимум хп игрок получит от регенерации", Order = 6)]
        public int MaxRegen;
        [JsonProperty("Максимальная защита от способности", Order = 7)]
        public int MaxProtection;
        [JsonProperty("Максимальный урон от способности", Order = 8)]
        public int MaxAtack;
        [JsonProperty("Максимальный уровень", Order = 9)]
        public int MaxLevel;
        [JsonProperty("Сколько возвращать DNK при откате способности(0-100%)", Order = 10)]
        public int ReturnPercent;
    }

    [JsonProperty("Команда для открытия системы улучшения", Order = 1)]
    public string CommandName;
    [JsonProperty("Предметы-исключения(если игрок добаывает этим предметом , опыт не зачисляется)", Order = 3)]
    public List<string> ItemExceptionList;
    [JsonProperty("Настройка способностей", Order = 2)]
    public List<classSkill> configSkillSettings;
    internal class classSkillSettings
    {
        [JsonProperty("Сколько опыта давать за убийство животных", Order = 5)]
        public double xpForKillAnimal;
        [JsonProperty("Сколько опыта давать за убийство NPC", Order = 2)]
        public double xpForKillNPC;
        [JsonProperty("Сколько опыта давать за убийство игрока в голову", Order = 4)]
        public double xpForKillHeadshot;
        [JsonProperty("Сколько опыта давать за убийство игрока", Order = 3)]
        public double xpForKillPlayer;
        [JsonProperty("Сколько опыта давать за добычу ресурсов", Order = 0)]
        public double xpForResourceExtraction;
        [JsonProperty("Сколько опыта давать за сбитие чинука", Order = 8)]
        public double xpForKillChinuk;
        [JsonProperty("Сколько опыта давать за разбитие бочек", Order = 1)]
        public double xpForBreakBarrels;
        [JsonProperty("Сколько опыта давать за взрыв танка", Order = 7)]
        public double xpForKillBredley;
        [JsonProperty("Сколько опыта давать за сбитие вертолета", Order = 6)]
        public double xpForKillHely;
    }

    [JsonProperty("Настройка опыта за действия", Order = 0)]
    public classSkillSettings xpSkillSettingsConfig;
    [JsonProperty("Сколько DNK давать за новый уровень", Order = 0)]
    public int countDNKForNewLevel;
    public static DefaultSettings SetDefaultSettings();
    [JsonProperty("Настройка UI", Order = 1)]
    public classGUISettings GUISettings;
    internal class classGUISettings
    {
        [JsonProperty("Лого в меню способностей (PNG)", Order = 0)]
        public string BGImage;
        [JsonProperty("Цвет кнопки + (HEX)", Order = 1)]
        public string ButtonPlusColorHEX;
        [JsonProperty("Цвет панели с действием над скиллом ( + - ) (HEX)", Order = 2)]
        public string ActionColorHEX;
        [JsonProperty("Цвет кнопки - (HEX)", Order = 3)]
        public string ButtonMinusColorHEX;
        [JsonProperty("Цвет заднего фона в меню способностей", Order = 4)]
        public string BackgroundColor;
        [JsonProperty("Текст в меню способностей", Order = 5)]
        public string TitleText;
        [JsonProperty("Цвет заднего фона способности", Order = 6)]
        public string SkillBackgroundColor;
        [JsonProperty("Цвет кнопки открывающей описание", Order = 7)]
        public string ButtonInfoColorHEX;
        [JsonProperty("Текст <Очки ДНК>", Order = 8)]
        public string DNKText;
        internal class Progress
        {
            [JsonProperty("AnchorMin прогресс бара", Order = 0)]
            public string AnchorMin;
            [JsonProperty("AnchorMax прогресс бара", Order = 1)]
            public string AnchorMax;
            [JsonProperty("OffsetMin прогресс бара", Order = 2)]
            public string OffsetMin;
            [JsonProperty("OffsetMax прогресс бара", Order = 3)]
            public string OffsetMax;
            [JsonProperty("Цвет заполняющейся полосы (HEX)", Order = 4)]
            public string ProgressBGColor;
        }

        [JsonProperty("Настройка UI-прогресс-бара", Order = 7)]
        public Progress ProgressSettings;
    }

    [JsonProperty("Сообщение при 100% опыта", Order = 2)]
    public string LevelupText;
}

public class classSkill
{
    [JsonProperty("Название", Order = 0)]
    public string Name;
    [JsonProperty("Описание", Order = 1)]
    public string Description;
    [JsonProperty("Shortname на который будет действовать увеличенный рейтинг скилла", Order = 2)]
    public List<string> ShortnameList;
    [JsonProperty("Рейты игрока ( ПРИБАВЛЯЮТСЯ % ОТ ДОБЫТОГО ИМ РЕСУРСА)", Order = 3)]
    public int Rate;
    [JsonProperty("Сколько % прибавлять за уровень", Order = 4)]
    public int PercentIncrement;
    [JsonProperty("Сколько количество жизней будет увеличиваться на XRate", Order = 5)]
    public double HPIncrementInRate;
    [JsonProperty("Сколько максимум хп игрок получит от регенерации", Order = 6)]
    public int MaxRegen;
    [JsonProperty("Максимальная защита от способности", Order = 7)]
    public int MaxProtection;
    [JsonProperty("Максимальный урон от способности", Order = 8)]
    public int MaxAtack;
    [JsonProperty("Максимальный уровень", Order = 9)]
    public int MaxLevel;
    [JsonProperty("Сколько возвращать DNK при откате способности(0-100%)", Order = 10)]
    public int ReturnPercent;
}

internal class classSkillSettings
{
    [JsonProperty("Сколько опыта давать за убийство животных", Order = 5)]
    public double xpForKillAnimal;
    [JsonProperty("Сколько опыта давать за убийство NPC", Order = 2)]
    public double xpForKillNPC;
    [JsonProperty("Сколько опыта давать за убийство игрока в голову", Order = 4)]
    public double xpForKillHeadshot;
    [JsonProperty("Сколько опыта давать за убийство игрока", Order = 3)]
    public double xpForKillPlayer;
    [JsonProperty("Сколько опыта давать за добычу ресурсов", Order = 0)]
    public double xpForResourceExtraction;
    [JsonProperty("Сколько опыта давать за сбитие чинука", Order = 8)]
    public double xpForKillChinuk;
    [JsonProperty("Сколько опыта давать за разбитие бочек", Order = 1)]
    public double xpForBreakBarrels;
    [JsonProperty("Сколько опыта давать за взрыв танка", Order = 7)]
    public double xpForKillBredley;
    [JsonProperty("Сколько опыта давать за сбитие вертолета", Order = 6)]
    public double xpForKillHely;
}

internal class classGUISettings
{
    [JsonProperty("Лого в меню способностей (PNG)", Order = 0)]
    public string BGImage;
    [JsonProperty("Цвет кнопки + (HEX)", Order = 1)]
    public string ButtonPlusColorHEX;
    [JsonProperty("Цвет панели с действием над скиллом ( + - ) (HEX)", Order = 2)]
    public string ActionColorHEX;
    [JsonProperty("Цвет кнопки - (HEX)", Order = 3)]
    public string ButtonMinusColorHEX;
    [JsonProperty("Цвет заднего фона в меню способностей", Order = 4)]
    public string BackgroundColor;
    [JsonProperty("Текст в меню способностей", Order = 5)]
    public string TitleText;
    [JsonProperty("Цвет заднего фона способности", Order = 6)]
    public string SkillBackgroundColor;
    [JsonProperty("Цвет кнопки открывающей описание", Order = 7)]
    public string ButtonInfoColorHEX;
    [JsonProperty("Текст <Очки ДНК>", Order = 8)]
    public string DNKText;
    internal class Progress
    {
        [JsonProperty("AnchorMin прогресс бара", Order = 0)]
        public string AnchorMin;
        [JsonProperty("AnchorMax прогресс бара", Order = 1)]
        public string AnchorMax;
        [JsonProperty("OffsetMin прогресс бара", Order = 2)]
        public string OffsetMin;
        [JsonProperty("OffsetMax прогресс бара", Order = 3)]
        public string OffsetMax;
        [JsonProperty("Цвет заполняющейся полосы (HEX)", Order = 4)]
        public string ProgressBGColor;
    }

    [JsonProperty("Настройка UI-прогресс-бара", Order = 7)]
    public Progress ProgressSettings;
}

internal class Progress
{
    [JsonProperty("AnchorMin прогресс бара", Order = 0)]
    public string AnchorMin;
    [JsonProperty("AnchorMax прогресс бара", Order = 1)]
    public string AnchorMax;
    [JsonProperty("OffsetMin прогресс бара", Order = 2)]
    public string OffsetMin;
    [JsonProperty("OffsetMax прогресс бара", Order = 3)]
    public string OffsetMax;
    [JsonProperty("Цвет заполняющейся полосы (HEX)", Order = 4)]
    public string ProgressBGColor;
}


```

---

## SkillSystem (1)

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("SkillSystem", "Chibubrik", "1.2.0")]
 class SkillSystem : RustPlugin
{
    private string Layer;
    private string LayersAlert;
    [PluginReference]
    private Plugin ImageLibrary;
    public class SkillSettings
    {
        [JsonProperty("Название панельки")]
        public string Name;
        [JsonProperty("Информация")]
        public string Info;
        [JsonProperty("Название предмета")]
        public string DisplayName;
        [JsonProperty("Используемый предмет Glue")]
        public string ShortName;
        [JsonProperty("SkinId предмета")]
        public ulong SkinID;
        [JsonProperty("Шанс выпадения листка в %")]
        public float DropChance;
    }

    public class Settings
    {
        [JsonProperty("Название навыка")]
        public string DisplayName;
        [JsonProperty("Рейтинг увеличения навыка за уровень")]
        public float Rate;
        [JsonProperty("Сколько нужно листов с информацией, чтобы прокачать навык")]
        public int Price;
        [JsonProperty("Максимальный уровень прокачки навыка")]
        public int LevelMax;
        [JsonProperty("Изображение навыка")]
        public string Url;
        [JsonProperty("Короткое название предмета, на который будет действовать увеличенный рейтинг навыка")]
        public Dictionary<string, string> ShortName;
    }

    private Dictionary<ulong, Dictionary<string, SettingsData>> settingsData;
    public class SettingsData
    {
        [JsonProperty("Навык")]
        public int Level;
        [JsonProperty("Рейт")]
        public float Rate;
    }

    public Configuration config;
    public class Configuration
    {
        [JsonProperty("Настройки")]
        public SkillSettings skill;
        [JsonProperty("Список")]
        public List<Settings> settings;
        [JsonProperty("Список ящиков")]
        public List<string> container;
        public static Configuration GetNewCong();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [ChatCommand("skill")]
    private void CommandSkill(BasePlayer player);
    [ConsoleCommand("skill")]
    private void Command(ConsoleSystem.Arg args);
    private void Loaded();
    private SettingsData AddPlayersData(ulong userID, string name);
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void SaveData();
    private void OnDispenserGather(ResourceDispenser dispenser, BasePlayer player, Item item);
    private void OnDispenserBonus(ResourceDispenser disp, BasePlayer player, Item item);
    private void OnLootEntity(BasePlayer player, BaseEntity entity, Item item);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     int GetList(BasePlayer player, string type);
    private void OnLootSpawn(LootContainer lootContainer);
     object OnItemSplit(Item thisI, int split_Amount);
     object CanStackItem(Item item, Item targetItem);
    private void SkillUI(BasePlayer player);
    private void AlertUI(BasePlayer player, string DisplayName);
    private static string HexToUiColor(string hex);
}

public class SkillSettings
{
    [JsonProperty("Название панельки")]
    public string Name;
    [JsonProperty("Информация")]
    public string Info;
    [JsonProperty("Название предмета")]
    public string DisplayName;
    [JsonProperty("Используемый предмет Glue")]
    public string ShortName;
    [JsonProperty("SkinId предмета")]
    public ulong SkinID;
    [JsonProperty("Шанс выпадения листка в %")]
    public float DropChance;
}

public class Settings
{
    [JsonProperty("Название навыка")]
    public string DisplayName;
    [JsonProperty("Рейтинг увеличения навыка за уровень")]
    public float Rate;
    [JsonProperty("Сколько нужно листов с информацией, чтобы прокачать навык")]
    public int Price;
    [JsonProperty("Максимальный уровень прокачки навыка")]
    public int LevelMax;
    [JsonProperty("Изображение навыка")]
    public string Url;
    [JsonProperty("Короткое название предмета, на который будет действовать увеличенный рейтинг навыка")]
    public Dictionary<string, string> ShortName;
}

public class SettingsData
{
    [JsonProperty("Навык")]
    public int Level;
    [JsonProperty("Рейт")]
    public float Rate;
}

public class Configuration
{
    [JsonProperty("Настройки")]
    public SkillSettings skill;
    [JsonProperty("Список")]
    public List<Settings> settings;
    [JsonProperty("Список ящиков")]
    public List<string> container;
    public static Configuration GetNewCong();
}


```

---

## SkinBox

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using System.Text;
using Oxide.Core;

Oxide.Plugins
[Info("SkinBox", "k1lly0u", "2.0.4"), Description("Allows you to reskin item's by placing it in the SkinBox and selecting a new skin")]
 class SkinBox : RustPlugin
{
    [PluginReference]
    private Plugin ServerRewards;
    private Plugin Economics;
    private Hash<string, HashSet<ulong>> _skinList;
    private Hash<ulong, string> _skinPermissions;
    private Hash<ulong, LootHandler> _activeSkinBoxes;
    private Hash<string, string> _shortnameToDisplayname;
    private Hash<ulong, string> _skinNameLookup;
    private Hash<ulong, double> _cooldownTimes;
    private bool _apiKeyMissing;
    private bool _skinsLoaded;
    private CostType _costType;
    private static SkinBox Instance { get; set; }
    private static Func<string, ulong, string> GetMessage;
    private const string COFFIN_PREFAB;
    private const int SCRAP_ITEM_ID;
    private void Loaded();
    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private object CanAcceptItem(ItemContainer container, Item item);
    private void OnItemSplit(Item item, int amount);
    private object CanMoveItem(Item movedItem, PlayerInventory inventory, uint targetContainerID, int targetSlot, int amount);
    private void OnItemAddedToContainer(ItemContainer container, Item item);
    private void OnItemRemovedFromContainer(ItemContainer container, Item item);
    private void OnLootEntityEnd(BasePlayer player, StorageContainer storageContainer);
    private void Unload();
    private void SendAPIMissingWarning();
    private void ChatMessage(BasePlayer player, string key, object[] args);
    private void CreateSkinBox(BasePlayer player);
    private void GetSkinsFor(BasePlayer player, string shortname, List<ulong> list);
    private bool HasItemPermissions(BasePlayer player, Item item);
    private T ParseType(string type);
    private double CurrentTime { get; set; }
    private string GetCostType(ulong playerId);
    private int GetReskinCost(Item item);
    private bool CanAffordToUse(BasePlayer player, int amount);
    private bool ChargePlayer(BasePlayer player, ItemCategory itemCategory);
    private bool ChargePlayer(BasePlayer player, int amount);
    private void ApplyCooldown(BasePlayer player);
    private bool IsOnCooldown(BasePlayer player);
    private bool IsOnCooldown(BasePlayer player, double remaining);
    private void StartSkinRequest();
    private void CollectApprovedSkins();
    private List<ulong> _skinsToVerify;
    private const string PUBLISHED_FILE_DETAILS;
    private const string COLLECTION_DETAILS;
    private const string ITEMS_BODY;
    private const string ITEM_ENTRY;
    private const string COLLECTION_BODY;
    private void VerifyWorkshopSkins();
    private void SendWorkshopQuery(int page, ConsoleSystem.Arg arg, string perm);
    private void SendWorkshopCollectionQuery(ulong collectionId, bool add, ConsoleSystem.Arg arg, string perm);
    private IEnumerator ValidateRequiredSkins(int code, string response, int page, int totalPages, bool isCollection, ConsoleSystem.Arg arg, string perm);
    private IEnumerator ProcessCollectionRequest(int code, string response, bool add, ConsoleSystem.Arg arg, string perm);
    private void RemoveSkins(ConsoleSystem.Arg arg, string perm);
    private void SendResponse(string message, ConsoleSystem.Arg arg);
    private Dictionary<string, string> _workshopNameToShortname;
    private void UpdateWorkshopNameConversionList();
    private Hash<string, string> _itemSkinRedirects;
    private void FindItemRedirects();
    private string GetRedirectedShortname(string shortname);
    private class LootHandler : MonoBehaviour
    {
        internal StorageContainer Entity { get; set; }
        internal BasePlayer Looter { get; set; }
        internal bool HasItem { get; set; }
        internal int InputAmount { get; set; }
        private InputItem inputItem;
        private int _currentPage;
        private int _maximumPages;
        private int _itemsPerPage;
        private List<ulong> _availableSkins;
        private void Awake();
        private void OnDestroy();
        internal void OnItemAdded(Item item);
        internal void OnItemRemoved(Item item);
        internal void ChangePage(int change);
        private IEnumerator RefillContainer();
        private IEnumerator FillContainer();
        private void ClearContainer();
        private Item CreateItem(ulong skinId);
        private bool InsertItem(Item item);
        private void RemoveItem(Item item);
        internal void CheckItemHasSplit(Item item);
        private IEnumerator CheckItemHasSplit(Item item, int originalAmount);
        private class InputItem
        {
            public ItemDefinition itemDefinition;
            public int amount;
            public ulong skin;
            public float condition;
            public float maxCondition;
            public int magazineContents;
            public int magazineCapacity;
            public ItemDefinition ammoType;
            public List<InputItem> contents;
            internal InputItem(string shortname, Item item);
            internal void Dispose();
            internal Item Create();
            internal void CloneTo(Item item);
        }

        private const string UI_PANEL;
        private const string UI_POPUP;
        private const string PAGE_COLOR;
        private const string PAGE_TEXT_COLOR;
        private const string BUTTON_COLOR;
        private const string BUTTON_TEXT_COLOR;
        private readonly UI4 Popup;
        private readonly UI4 Container;
        private readonly UI4 BackButton;
        private readonly UI4 Text;
        private readonly UI4 NextButton;
        private void CreateOverlay();
        internal void PopupMessage(string message);
    }

    public static class UI
    {
        public static CuiElementContainer Container(string panel, UI4 dimensions, string parent);
        public static void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions);
        public static void Label(CuiElementContainer container, string panel, string text, string color, int size, UI4 dimensions, TextAnchor align);
        public static void Button(CuiElementContainer container, string panel, string color, string text, string textColor, int size, UI4 dimensions, string command, TextAnchor align);
    }

    public class UI4
    {
        public float xMin;
        public float yMin;
        public float xMax;
        public float yMax;
        public static UI4 Full { get; set; }
        public UI4(float xMin, float yMin, float xMax, float yMax);
        public string GetMin();
        public string GetMax();
    }

    [ConsoleCommand("skinbox.pagenext")]
    private void cmdPageNext(ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.pageprev")]
    private void cmdPagePrev(ConsoleSystem.Arg arg);
    private void cmdSkinBox(BasePlayer player, string command, string[] args);
    [ConsoleCommand("skinbox.cmds")]
    private void cmdListCmds(ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.addskin")]
    private void consoleAddSkin(ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.removeskin")]
    private void consoleRemoveSkin(ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.addvipskin")]
    private void consoleAddVIPSkin(ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.removevipskin")]
    private void consoleRemoveVIPSkin(ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.addcollection")]
    private void consoleAddCollection(ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.removecollection")]
    private void consoleRemoveCollection(ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.addvipcollection")]
    private void consoleAddVIPCollection(ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.removevipcollection")]
    private void consoleRemoveVIPCollection(ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.addexcluded")]
    private void consoleAddExcluded(ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.removeexcluded")]
    private void consoleRemoveExcluded(ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.open")]
    private void consoleSkinboxOpen(ConsoleSystem.Arg arg);
    private bool IsSkinBoxPlayer(ulong playerId);
    private static ConfigData Configuration;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Skin Options")]
        public SkinOptions Skins { get; set; }
        [JsonProperty(PropertyName = "Cooldown Options")]
        public CooldownOptions Cooldown { get; set; }
        [JsonProperty(PropertyName = "Command Options")]
        public CommandOptions Command { get; set; }
        [JsonProperty(PropertyName = "Permission Options")]
        public PermissionOptions Permission { get; set; }
        [JsonProperty(PropertyName = "Usage Cost Options")]
        public CostOptions Cost { get; set; }
        [JsonProperty(PropertyName = "Other Options")]
        public OtherOptions Other { get; set; }
        [JsonProperty(PropertyName = "Imported Workshop Skins")]
        public Hash<string, HashSet<ulong>> SkinList { get; set; }
        [JsonProperty(PropertyName = "Blacklisted Skin ID's")]
        public HashSet<ulong> Blacklist { get; set; }
        public class SkinOptions
        {
            [JsonProperty(PropertyName = "Maximum number of approved skins allowed for each item (-1 is unlimited)")]
            public int ApprovedLimit { get; set; }
            [JsonProperty(PropertyName = "Maximum number of pages viewable")]
            public int MaximumPages { get; set; }
            [JsonProperty(PropertyName = "Include approved skins")]
            public bool UseApproved { get; set; }
            [JsonProperty(PropertyName = "Include manually imported workshop skins")]
            public bool UseWorkshop { get; set; }
            [JsonProperty(PropertyName = "Remove approved skin ID's from config workshop skin list")]
            public bool RemoveApproved { get; set; }
            [JsonProperty(PropertyName = "Include redirected skins")]
            public bool UseRedirected { get; set; }
            [JsonProperty(PropertyName = "Steam API key for workshop skins (https://steamcommunity.com/dev/apikey)")]
            public string APIKey { get; set; }
        }

        public class CooldownOptions
        {
            [JsonProperty(PropertyName = "Enable cooldowns")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Cooldown time start's when a item is removed from the box")]
            public bool ActivateOnTake { get; set; }
            [JsonProperty(PropertyName = "Length of cooldown time (seconds)")]
            public int Time { get; set; }
        }

        public class PermissionOptions
        {
            [JsonProperty(PropertyName = "Permission required to use SkinBox")]
            public string Use { get; set; }
            [JsonProperty(PropertyName = "Permission required to use admin functions")]
            public string Admin { get; set; }
            [JsonProperty(PropertyName = "Permission that bypasses usage costs")]
            public string NoCost { get; set; }
            [JsonProperty(PropertyName = "Permission that bypasses usage cooldown")]
            public string NoCooldown { get; set; }
            [JsonProperty(PropertyName = "Permission required to skin weapons")]
            public string Weapon { get; set; }
            [JsonProperty(PropertyName = "Permission required to skin deployables")]
            public string Deployable { get; set; }
            [JsonProperty(PropertyName = "Permission required to skin attire")]
            public string Attire { get; set; }
            [JsonProperty(PropertyName = "Permission required to view approved skins")]
            public string Approved { get; set; }
            [JsonProperty(PropertyName = "Custom permissions per skin")]
            public Hash<string, List<ulong>> Custom { get; set; }
            public void RegisterPermissions(Permission permission, Plugin plugin);
            public void ReverseCustomSkinPermissions(Hash<ulong, string> list);
        }

        public class CommandOptions
        {
            [JsonProperty(PropertyName = "Commands to open the SkinBox")]
            public string[] Commands { get; set; }
            internal void RegisterCommands(Game.Rust.Libraries.Command cmd, Plugin plugin);
        }

        public class CostOptions
        {
            [JsonProperty(PropertyName = "Enable usage costs")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Currency used for usage costs (Scrap, Economics, ServerRewards)")]
            public string Currency { get; set; }
            [JsonProperty(PropertyName = "Cost to open the SkinBox")]
            public int Open { get; set; }
            [JsonProperty(PropertyName = "Cost to skin deployables")]
            public int Deployable { get; set; }
            [JsonProperty(PropertyName = "Cost to skin attire")]
            public int Attire { get; set; }
            [JsonProperty(PropertyName = "Cost to skin weapons")]
            public int Weapon { get; set; }
        }

        public class OtherOptions
        {
            [JsonProperty(PropertyName = "Allow stacked items")]
            public bool AllowStacks { get; set; }
            [JsonProperty(PropertyName = "Auth-level required to view blacklisted skins")]
            public int BlacklistAuth { get; set; }
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    public class QueryResponse
    {
        public Response response;
    }

    public class Response
    {
        public int total;
        public PublishedFileDetails[] publishedfiledetails;
    }

    public class PublishedFileDetails
    {
        public int result;
        public string publishedfileid;
        public string creator;
        public int creator_appid;
        public int consumer_appid;
        public int consumer_shortcutid;
        public string filename;
        public string file_size;
        public string preview_file_size;
        public string file_url;
        public string preview_url;
        public string url;
        public string hcontent_file;
        public string hcontent_preview;
        public string title;
        public string file_description;
        public int time_created;
        public int time_updated;
        public int visibility;
        public int flags;
        public bool workshop_file;
        public bool workshop_accepted;
        public bool show_subscribe_all;
        public int num_comments_public;
        public bool banned;
        public string ban_reason;
        public string banner;
        public bool can_be_deleted;
        public string app_name;
        public int file_type;
        public bool can_subscribe;
        public int subscriptions;
        public int favorited;
        public int followers;
        public int lifetime_subscriptions;
        public int lifetime_favorited;
        public int lifetime_followers;
        public string lifetime_playtime;
        public string lifetime_playtime_sessions;
        public int views;
        public int num_children;
        public int num_reports;
        public Preview[] previews;
        public Tag[] tags;
        public int language;
        public bool maybe_inappropriate_sex;
        public bool maybe_inappropriate_violence;
        public class Tag
        {
            public string tag;
            public bool adminonly;
        }

    }

    public class PublishedFileQueryResponse
    {
        public FileResponse response { get; set; }
    }

    public class FileResponse
    {
        public int result { get; set; }
        public int resultcount { get; set; }
        public PublishedFileQueryDetail[] publishedfiledetails { get; set; }
    }

    public class PublishedFileQueryDetail
    {
        public string publishedfileid { get; set; }
        public int result { get; set; }
        public string creator { get; set; }
        public int creator_app_id { get; set; }
        public int consumer_app_id { get; set; }
        public string filename { get; set; }
        public int file_size { get; set; }
        public string preview_url { get; set; }
        public string hcontent_preview { get; set; }
        public string title { get; set; }
        public string description { get; set; }
        public int time_created { get; set; }
        public int time_updated { get; set; }
        public int visibility { get; set; }
        public int banned { get; set; }
        public string ban_reason { get; set; }
        public int subscriptions { get; set; }
        public int favorited { get; set; }
        public int lifetime_subscriptions { get; set; }
        public int lifetime_favorited { get; set; }
        public int views { get; set; }
        public Tag[] tags { get; set; }
        public class Tag
        {
            public string tag { get; set; }
        }

    }

    public class Preview
    {
        public string previewid;
        public int sortorder;
        public string url;
        public int size;
        public string filename;
        public int preview_type;
        public string youtubevideoid;
        public string external_reference;
    }

    public class CollectionQueryResponse
    {
        public CollectionResponse response { get; set; }
    }

    public class CollectionResponse
    {
        public int result { get; set; }
        public int resultcount { get; set; }
        public CollectionDetails[] collectiondetails { get; set; }
    }

    public class CollectionDetails
    {
        public string publishedfileid { get; set; }
        public int result { get; set; }
        public CollectionChild[] children { get; set; }
    }

    public class CollectionChild
    {
        public string publishedfileid { get; set; }
        public int sortorder { get; set; }
        public int filetype { get; set; }
    }

}

private class LootHandler : MonoBehaviour
{
    internal StorageContainer Entity { get; set; }
    internal BasePlayer Looter { get; set; }
    internal bool HasItem { get; set; }
    internal int InputAmount { get; set; }
    private InputItem inputItem;
    private int _currentPage;
    private int _maximumPages;
    private int _itemsPerPage;
    private List<ulong> _availableSkins;
    private void Awake();
    private void OnDestroy();
    internal void OnItemAdded(Item item);
    internal void OnItemRemoved(Item item);
    internal void ChangePage(int change);
    private IEnumerator RefillContainer();
    private IEnumerator FillContainer();
    private void ClearContainer();
    private Item CreateItem(ulong skinId);
    private bool InsertItem(Item item);
    private void RemoveItem(Item item);
    internal void CheckItemHasSplit(Item item);
    private IEnumerator CheckItemHasSplit(Item item, int originalAmount);
    private class InputItem
    {
        public ItemDefinition itemDefinition;
        public int amount;
        public ulong skin;
        public float condition;
        public float maxCondition;
        public int magazineContents;
        public int magazineCapacity;
        public ItemDefinition ammoType;
        public List<InputItem> contents;
        internal InputItem(string shortname, Item item);
        internal void Dispose();
        internal Item Create();
        internal void CloneTo(Item item);
    }

    private const string UI_PANEL;
    private const string UI_POPUP;
    private const string PAGE_COLOR;
    private const string PAGE_TEXT_COLOR;
    private const string BUTTON_COLOR;
    private const string BUTTON_TEXT_COLOR;
    private readonly UI4 Popup;
    private readonly UI4 Container;
    private readonly UI4 BackButton;
    private readonly UI4 Text;
    private readonly UI4 NextButton;
    private void CreateOverlay();
    internal void PopupMessage(string message);
}

private class InputItem
{
    public ItemDefinition itemDefinition;
    public int amount;
    public ulong skin;
    public float condition;
    public float maxCondition;
    public int magazineContents;
    public int magazineCapacity;
    public ItemDefinition ammoType;
    public List<InputItem> contents;
    internal InputItem(string shortname, Item item);
    internal void Dispose();
    internal Item Create();
    internal void CloneTo(Item item);
}

public static class UI
{
    public static CuiElementContainer Container(string panel, UI4 dimensions, string parent);
    public static void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions);
    public static void Label(CuiElementContainer container, string panel, string text, string color, int size, UI4 dimensions, TextAnchor align);
    public static void Button(CuiElementContainer container, string panel, string color, string text, string textColor, int size, UI4 dimensions, string command, TextAnchor align);
}

public class UI4
{
    public float xMin;
    public float yMin;
    public float xMax;
    public float yMax;
    public static UI4 Full { get; set; }
    public UI4(float xMin, float yMin, float xMax, float yMax);
    public string GetMin();
    public string GetMax();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Skin Options")]
    public SkinOptions Skins { get; set; }
    [JsonProperty(PropertyName = "Cooldown Options")]
    public CooldownOptions Cooldown { get; set; }
    [JsonProperty(PropertyName = "Command Options")]
    public CommandOptions Command { get; set; }
    [JsonProperty(PropertyName = "Permission Options")]
    public PermissionOptions Permission { get; set; }
    [JsonProperty(PropertyName = "Usage Cost Options")]
    public CostOptions Cost { get; set; }
    [JsonProperty(PropertyName = "Other Options")]
    public OtherOptions Other { get; set; }
    [JsonProperty(PropertyName = "Imported Workshop Skins")]
    public Hash<string, HashSet<ulong>> SkinList { get; set; }
    [JsonProperty(PropertyName = "Blacklisted Skin ID's")]
    public HashSet<ulong> Blacklist { get; set; }
    public class SkinOptions
    {
        [JsonProperty(PropertyName = "Maximum number of approved skins allowed for each item (-1 is unlimited)")]
        public int ApprovedLimit { get; set; }
        [JsonProperty(PropertyName = "Maximum number of pages viewable")]
        public int MaximumPages { get; set; }
        [JsonProperty(PropertyName = "Include approved skins")]
        public bool UseApproved { get; set; }
        [JsonProperty(PropertyName = "Include manually imported workshop skins")]
        public bool UseWorkshop { get; set; }
        [JsonProperty(PropertyName = "Remove approved skin ID's from config workshop skin list")]
        public bool RemoveApproved { get; set; }
        [JsonProperty(PropertyName = "Include redirected skins")]
        public bool UseRedirected { get; set; }
        [JsonProperty(PropertyName = "Steam API key for workshop skins (https://steamcommunity.com/dev/apikey)")]
        public string APIKey { get; set; }
    }

    public class CooldownOptions
    {
        [JsonProperty(PropertyName = "Enable cooldowns")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Cooldown time start's when a item is removed from the box")]
        public bool ActivateOnTake { get; set; }
        [JsonProperty(PropertyName = "Length of cooldown time (seconds)")]
        public int Time { get; set; }
    }

    public class PermissionOptions
    {
        [JsonProperty(PropertyName = "Permission required to use SkinBox")]
        public string Use { get; set; }
        [JsonProperty(PropertyName = "Permission required to use admin functions")]
        public string Admin { get; set; }
        [JsonProperty(PropertyName = "Permission that bypasses usage costs")]
        public string NoCost { get; set; }
        [JsonProperty(PropertyName = "Permission that bypasses usage cooldown")]
        public string NoCooldown { get; set; }
        [JsonProperty(PropertyName = "Permission required to skin weapons")]
        public string Weapon { get; set; }
        [JsonProperty(PropertyName = "Permission required to skin deployables")]
        public string Deployable { get; set; }
        [JsonProperty(PropertyName = "Permission required to skin attire")]
        public string Attire { get; set; }
        [JsonProperty(PropertyName = "Permission required to view approved skins")]
        public string Approved { get; set; }
        [JsonProperty(PropertyName = "Custom permissions per skin")]
        public Hash<string, List<ulong>> Custom { get; set; }
        public void RegisterPermissions(Permission permission, Plugin plugin);
        public void ReverseCustomSkinPermissions(Hash<ulong, string> list);
    }

    public class CommandOptions
    {
        [JsonProperty(PropertyName = "Commands to open the SkinBox")]
        public string[] Commands { get; set; }
        internal void RegisterCommands(Game.Rust.Libraries.Command cmd, Plugin plugin);
    }

    public class CostOptions
    {
        [JsonProperty(PropertyName = "Enable usage costs")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Currency used for usage costs (Scrap, Economics, ServerRewards)")]
        public string Currency { get; set; }
        [JsonProperty(PropertyName = "Cost to open the SkinBox")]
        public int Open { get; set; }
        [JsonProperty(PropertyName = "Cost to skin deployables")]
        public int Deployable { get; set; }
        [JsonProperty(PropertyName = "Cost to skin attire")]
        public int Attire { get; set; }
        [JsonProperty(PropertyName = "Cost to skin weapons")]
        public int Weapon { get; set; }
    }

    public class OtherOptions
    {
        [JsonProperty(PropertyName = "Allow stacked items")]
        public bool AllowStacks { get; set; }
        [JsonProperty(PropertyName = "Auth-level required to view blacklisted skins")]
        public int BlacklistAuth { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class SkinOptions
{
    [JsonProperty(PropertyName = "Maximum number of approved skins allowed for each item (-1 is unlimited)")]
    public int ApprovedLimit { get; set; }
    [JsonProperty(PropertyName = "Maximum number of pages viewable")]
    public int MaximumPages { get; set; }
    [JsonProperty(PropertyName = "Include approved skins")]
    public bool UseApproved { get; set; }
    [JsonProperty(PropertyName = "Include manually imported workshop skins")]
    public bool UseWorkshop { get; set; }
    [JsonProperty(PropertyName = "Remove approved skin ID's from config workshop skin list")]
    public bool RemoveApproved { get; set; }
    [JsonProperty(PropertyName = "Include redirected skins")]
    public bool UseRedirected { get; set; }
    [JsonProperty(PropertyName = "Steam API key for workshop skins (https://steamcommunity.com/dev/apikey)")]
    public string APIKey { get; set; }
}

public class CooldownOptions
{
    [JsonProperty(PropertyName = "Enable cooldowns")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Cooldown time start's when a item is removed from the box")]
    public bool ActivateOnTake { get; set; }
    [JsonProperty(PropertyName = "Length of cooldown time (seconds)")]
    public int Time { get; set; }
}

public class PermissionOptions
{
    [JsonProperty(PropertyName = "Permission required to use SkinBox")]
    public string Use { get; set; }
    [JsonProperty(PropertyName = "Permission required to use admin functions")]
    public string Admin { get; set; }
    [JsonProperty(PropertyName = "Permission that bypasses usage costs")]
    public string NoCost { get; set; }
    [JsonProperty(PropertyName = "Permission that bypasses usage cooldown")]
    public string NoCooldown { get; set; }
    [JsonProperty(PropertyName = "Permission required to skin weapons")]
    public string Weapon { get; set; }
    [JsonProperty(PropertyName = "Permission required to skin deployables")]
    public string Deployable { get; set; }
    [JsonProperty(PropertyName = "Permission required to skin attire")]
    public string Attire { get; set; }
    [JsonProperty(PropertyName = "Permission required to view approved skins")]
    public string Approved { get; set; }
    [JsonProperty(PropertyName = "Custom permissions per skin")]
    public Hash<string, List<ulong>> Custom { get; set; }
    public void RegisterPermissions(Permission permission, Plugin plugin);
    public void ReverseCustomSkinPermissions(Hash<ulong, string> list);
}

public class CommandOptions
{
    [JsonProperty(PropertyName = "Commands to open the SkinBox")]
    public string[] Commands { get; set; }
    internal void RegisterCommands(Game.Rust.Libraries.Command cmd, Plugin plugin);
}

public class CostOptions
{
    [JsonProperty(PropertyName = "Enable usage costs")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Currency used for usage costs (Scrap, Economics, ServerRewards)")]
    public string Currency { get; set; }
    [JsonProperty(PropertyName = "Cost to open the SkinBox")]
    public int Open { get; set; }
    [JsonProperty(PropertyName = "Cost to skin deployables")]
    public int Deployable { get; set; }
    [JsonProperty(PropertyName = "Cost to skin attire")]
    public int Attire { get; set; }
    [JsonProperty(PropertyName = "Cost to skin weapons")]
    public int Weapon { get; set; }
}

public class OtherOptions
{
    [JsonProperty(PropertyName = "Allow stacked items")]
    public bool AllowStacks { get; set; }
    [JsonProperty(PropertyName = "Auth-level required to view blacklisted skins")]
    public int BlacklistAuth { get; set; }
}

public class QueryResponse
{
    public Response response;
}

public class Response
{
    public int total;
    public PublishedFileDetails[] publishedfiledetails;
}

public class PublishedFileDetails
{
    public int result;
    public string publishedfileid;
    public string creator;
    public int creator_appid;
    public int consumer_appid;
    public int consumer_shortcutid;
    public string filename;
    public string file_size;
    public string preview_file_size;
    public string file_url;
    public string preview_url;
    public string url;
    public string hcontent_file;
    public string hcontent_preview;
    public string title;
    public string file_description;
    public int time_created;
    public int time_updated;
    public int visibility;
    public int flags;
    public bool workshop_file;
    public bool workshop_accepted;
    public bool show_subscribe_all;
    public int num_comments_public;
    public bool banned;
    public string ban_reason;
    public string banner;
    public bool can_be_deleted;
    public string app_name;
    public int file_type;
    public bool can_subscribe;
    public int subscriptions;
    public int favorited;
    public int followers;
    public int lifetime_subscriptions;
    public int lifetime_favorited;
    public int lifetime_followers;
    public string lifetime_playtime;
    public string lifetime_playtime_sessions;
    public int views;
    public int num_children;
    public int num_reports;
    public Preview[] previews;
    public Tag[] tags;
    public int language;
    public bool maybe_inappropriate_sex;
    public bool maybe_inappropriate_violence;
    public class Tag
    {
        public string tag;
        public bool adminonly;
    }

}

public class Tag
{
    public string tag;
    public bool adminonly;
}

public class PublishedFileQueryResponse
{
    public FileResponse response { get; set; }
}

public class FileResponse
{
    public int result { get; set; }
    public int resultcount { get; set; }
    public PublishedFileQueryDetail[] publishedfiledetails { get; set; }
}

public class PublishedFileQueryDetail
{
    public string publishedfileid { get; set; }
    public int result { get; set; }
    public string creator { get; set; }
    public int creator_app_id { get; set; }
    public int consumer_app_id { get; set; }
    public string filename { get; set; }
    public int file_size { get; set; }
    public string preview_url { get; set; }
    public string hcontent_preview { get; set; }
    public string title { get; set; }
    public string description { get; set; }
    public int time_created { get; set; }
    public int time_updated { get; set; }
    public int visibility { get; set; }
    public int banned { get; set; }
    public string ban_reason { get; set; }
    public int subscriptions { get; set; }
    public int favorited { get; set; }
    public int lifetime_subscriptions { get; set; }
    public int lifetime_favorited { get; set; }
    public int views { get; set; }
    public Tag[] tags { get; set; }
    public class Tag
    {
        public string tag { get; set; }
    }

}

public class Tag
{
    public string tag { get; set; }
}

public class Preview
{
    public string previewid;
    public int sortorder;
    public string url;
    public int size;
    public string filename;
    public int preview_type;
    public string youtubevideoid;
    public string external_reference;
}

public class CollectionQueryResponse
{
    public CollectionResponse response { get; set; }
}

public class CollectionResponse
{
    public int result { get; set; }
    public int resultcount { get; set; }
    public CollectionDetails[] collectiondetails { get; set; }
}

public class CollectionDetails
{
    public string publishedfileid { get; set; }
    public int result { get; set; }
    public CollectionChild[] children { get; set; }
}

public class CollectionChild
{
    public string publishedfileid { get; set; }
    public int sortorder { get; set; }
    public int filetype { get; set; }
}


```

---

## SkinBox (1)

```csharp
using System;
using System.Text;
using System.Collections.Generic;
using System.Collections;
using System.Linq;
using UnityEngine;
using Steamworks;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("SkinBox", "FuJiCuRa", "1.16.3", ResourceId = 17)]
[Description("SkinBox is a plugin to convert any skinnable item into each skin variant")]
internal class SkinBox : RustPlugin
{
    [PluginReference]
    private Plugin QuickSort;
    private Plugin ServerRewards;
    private Plugin Economics;
    private Plugin StacksExtended;
    private bool skinsLoaded;
    private bool Initialized;
    private bool Changed;
    private static SkinBox skinBox;
    private bool activeServerRewards;
    private bool activeEconomics;
    private bool activePointSystem;
    private int maxItemsShown;
    private bool _stacksExtendedExtrasDisabled;
    private Dictionary<string, LinkedList<ulong>> skinsCache;
    private Dictionary<string, LinkedList<ulong>> skinsCacheLimited;
    private Dictionary<string, int> approvedSkinsCount;
    private Dictionary<string, DateTime> cooldownTimes;
    private Dictionary<string, string> NameToItemName;
    private Dictionary<string, string> ItemNameToName;
    private Dictionary<string, object> manualAddedSkinsPre;
    private Dictionary<string, List<ulong>> manualAddedSkins;
    private List<ulong> excludedSkins;
    private List<object> excludedSkinsPre;
    private Dictionary<ulong, SBH> activeSkinBoxes;
    private Dictionary<ulong, string> skinWorkshopNames;
    private List<object> altSkinBoxCommand;
    private string skinBoxCommand;
    private string permissionUse;
    private bool showLoadedSkinCounts;
    private int exludedSkinsAuthLevel;
    private bool hideQuickSort;
    private string steamApiKey;
    private int accessOverrideAuthLevel;
    private bool allowStackedItems;
    private bool enableCustomPerms;
    private string permCustomPlayerwearable;
    private string permCustomWeapon;
    private string permCustomDeployable;
    private bool useInbuiltSkins;
    private bool useApprovedSkins;
    private int approvedSkinsLimit;
    private bool useManualAddedSkins;
    private int maxPagesShown;
    private bool enableCooldown;
    private int cooldownBox;
    private bool cooldownOverrideAdmin;
    private int cooldownOverrideAuthLevel;
    private bool activateAfterSkinTaken;
    private bool enableUsageCost;
    private bool useServerRewards;
    private bool useEconomics;
    private int costBoxOpen;
    private int costWeapon;
    private int costPlayerwearable;
    private int costDeployable;
    private bool costExcludeAdmins;
    private string costExcludePerm;
    private bool costExcludePermEnabled;
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private object getSkincache();
    private void LoadVariables();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void Loaded();
    private void Unload();
    private void OnServerInitialized();
    private void FnllyLdd();
    private void GtItmSkns();
    private string BldDtlsSt(List<ulong> list, int pg);
    private void CllMnlSkinsWb(List<ulong> i, int pg);
    private IEnumerator PrfMnlSkinsWb(int cd, string res, List<ulong> i, int pg, int ttlPgs);
    private void ChckApprvdSkns();
    [ConsoleCommand("skinbox.cmds")]
    private void cmdListCmds(ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.addskin")]
    private void consoleAddSkin(ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.removeskin")]
    private void consoleRemoveSkin(ConsoleSystem.Arg arg);
    private void CallSkinsImportWeb(List<ulong> i, ConsoleSystem.Arg arg);
    private void ProofSkinsImportWeb(int cd, string res, ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.addexcluded")]
    private void consoleAddExcluded(ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.removeexcluded")]
    private void consoleRemoveExcluded(ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.addcollection")]
    private void consoleAddCollection(ConsoleSystem.Arg arg);
    public void PstCllbckAdd(int cd, string res, ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.removecollection")]
    private void consoleRemoveCollection(ConsoleSystem.Arg arg);
    public void PostCallbackRemove(int cd, string res, ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.open")]
    private void consoleSkinboxOpen(ConsoleSystem.Arg arg);
    private void cmdSknBx(BasePlayer player, string command, string[] args);
    public bool ChckOpnBlnc(BasePlayer player);
    public bool ChckSknBlnc(BasePlayer player, Item item);
    public bool WthdrwBlnc(BasePlayer player, Item item);
    internal class SBH : FacepunchBehaviour
    {
        public bool isCreating;
        public bool isBlocked;
        public bool isEmptied;
        public int itemId;
        public int itemAmount;
        public Item currentItem;
        public BasePlayer looter;
        public ItemContainer loot;
        public BaseEntity entityOwner;
        public ulong skinId;
        public int currentPage;
        public int totalPages;
        public int lastPageCount;
        public LinkedList<ulong> itemSkins;
        public int skinsTotal;
        public int perPageTotal;
        public int maxPages;
        public bool refundItem;
        private void Awake();
        public void ShwUi();
        public void ClsUi();
        public void PgNxt();
        public void PgPrv();
        public void StrtNwItm(ItemContainer container, Item item);
        public void FllSknBx(int page);
        public void PlayerStoppedLooting(BasePlayer player);
        private void OnDestroy();
    }

    private void OpnSknBx(BasePlayer player, bool refundItem);
    public void StrtLtngEntty(PlayerLoot loot, BaseEntity targetEntity);
    public void ClrCntnr(ItemContainer container);
    private object CanAcceptItem(ItemContainer container, Item item);
    public bool ChckItmPrms(BasePlayer player, Item item);
    private void OnItemAddedToContainer(ItemContainer container, Item item);
    public bool InsrtItm(ItemContainer container, Item item, bool mark);
    public bool RmvItm(ItemContainer container, Item item, bool mark);
    private void OnItemRemovedFromContainer(ItemContainer container, Item item);
    public void UnsbscrbSplt();
    public void SbscrbSplt();
    public void SndRplyCl(ConsoleSystem.Arg arg, string format);
    private static string _(string i);
    public void DstryUi(BasePlayer player);
    public void CrtUi(BasePlayer player, int page, int total);
    [ConsoleCommand("skinbox.pagenext")]
    private void cmdPageNext(ConsoleSystem.Arg arg);
    [ConsoleCommand("skinbox.pageprev")]
    private void cmdPagePrev(ConsoleSystem.Arg arg);
    private bool IsSkinBoxPlayer(ulong playerId);
    private void OnPluginUnloaded(Plugin name);
    private void OnPluginLoaded(Plugin name);
    public void ChckSbscrptns(bool wasUnload);
    private object OnItemSplit(Item thisI, int split_Amount);
    private object CanStackItem(Item thisI, Item item);
    private string u1;
    private string u2;
    public class GtPblshdFlDtls
    {
        [JsonProperty("response")]
        public Response response;
        public class Tag
        {
            [JsonProperty("tag")]
            public string tag;
        }

        public class Response
        {
            [JsonProperty("result")]
            public int result;
            [JsonProperty("resultcount")]
            public int resultcount;
            [JsonProperty("publishedfiledetails")]
            public List<Publishedfiledetail> publishedfiledetails;
            public class Publishedfiledetail
            {
                [JsonProperty("publishedfileid")]
                public ulong publishedfileid;
                [JsonProperty("result")]
                public int result;
                [JsonProperty("creator")]
                public string creator;
                [JsonProperty("creator_app_id")]
                public int creator_app_id;
                [JsonProperty("consumer_app_id")]
                public int consumer_app_id;
                [JsonProperty("filename")]
                public string filename;
                [JsonProperty("file_size")]
                public int file_size;
                [JsonProperty("preview_url")]
                public string preview_url;
                [JsonProperty("hcontent_preview")]
                public string hcontent_preview;
                [JsonProperty("title")]
                public string title;
                [JsonProperty("description")]
                public string description;
                [JsonProperty("time_created")]
                public int time_created;
                [JsonProperty("time_updated")]
                public int time_updated;
                [JsonProperty("visibility")]
                public int visibility;
                [JsonProperty("banned")]
                public int banned;
                [JsonProperty("ban_reason")]
                public string ban_reason;
                [JsonProperty("subscriptions")]
                public int subscriptions;
                [JsonProperty("favorited")]
                public int favorited;
                [JsonProperty("lifetime_subscriptions")]
                public int lifetime_subscriptions;
                [JsonProperty("lifetime_favorited")]
                public int lifetime_favorited;
                [JsonProperty("views")]
                public int views;
                [JsonProperty("tags")]
                public List<Tag> tags;
            }

        }

    }

    public class GtCllctnDtls
    {
        [JsonProperty("response")]
        public Response response;
        public class Response
        {
            [JsonProperty("result")]
            public int result;
            [JsonProperty("resultcount")]
            public int resultcount;
            [JsonProperty("collectiondetails")]
            public List<Collectiondetail> collectiondetails;
            public class Collectiondetail
            {
                [JsonProperty("publishedfileid")]
                public string publishedfileid;
                [JsonProperty("result")]
                public int result;
                [JsonProperty("children")]
                public List<Child> children;
                public class Child
                {
                    [JsonProperty("publishedfileid")]
                    public string publishedfileid;
                    [JsonProperty("sortorder")]
                    public int sortorder;
                    [JsonProperty("filetype")]
                    public int filetype;
                }

            }

        }

    }

}

internal class SBH : FacepunchBehaviour
{
    public bool isCreating;
    public bool isBlocked;
    public bool isEmptied;
    public int itemId;
    public int itemAmount;
    public Item currentItem;
    public BasePlayer looter;
    public ItemContainer loot;
    public BaseEntity entityOwner;
    public ulong skinId;
    public int currentPage;
    public int totalPages;
    public int lastPageCount;
    public LinkedList<ulong> itemSkins;
    public int skinsTotal;
    public int perPageTotal;
    public int maxPages;
    public bool refundItem;
    private void Awake();
    public void ShwUi();
    public void ClsUi();
    public void PgNxt();
    public void PgPrv();
    public void StrtNwItm(ItemContainer container, Item item);
    public void FllSknBx(int page);
    public void PlayerStoppedLooting(BasePlayer player);
    private void OnDestroy();
}

public class GtPblshdFlDtls
{
    [JsonProperty("response")]
    public Response response;
    public class Tag
    {
        [JsonProperty("tag")]
        public string tag;
    }

    public class Response
    {
        [JsonProperty("result")]
        public int result;
        [JsonProperty("resultcount")]
        public int resultcount;
        [JsonProperty("publishedfiledetails")]
        public List<Publishedfiledetail> publishedfiledetails;
        public class Publishedfiledetail
        {
            [JsonProperty("publishedfileid")]
            public ulong publishedfileid;
            [JsonProperty("result")]
            public int result;
            [JsonProperty("creator")]
            public string creator;
            [JsonProperty("creator_app_id")]
            public int creator_app_id;
            [JsonProperty("consumer_app_id")]
            public int consumer_app_id;
            [JsonProperty("filename")]
            public string filename;
            [JsonProperty("file_size")]
            public int file_size;
            [JsonProperty("preview_url")]
            public string preview_url;
            [JsonProperty("hcontent_preview")]
            public string hcontent_preview;
            [JsonProperty("title")]
            public string title;
            [JsonProperty("description")]
            public string description;
            [JsonProperty("time_created")]
            public int time_created;
            [JsonProperty("time_updated")]
            public int time_updated;
            [JsonProperty("visibility")]
            public int visibility;
            [JsonProperty("banned")]
            public int banned;
            [JsonProperty("ban_reason")]
            public string ban_reason;
            [JsonProperty("subscriptions")]
            public int subscriptions;
            [JsonProperty("favorited")]
            public int favorited;
            [JsonProperty("lifetime_subscriptions")]
            public int lifetime_subscriptions;
            [JsonProperty("lifetime_favorited")]
            public int lifetime_favorited;
            [JsonProperty("views")]
            public int views;
            [JsonProperty("tags")]
            public List<Tag> tags;
        }

    }

}

public class Tag
{
    [JsonProperty("tag")]
    public string tag;
}

public class Response
{
    [JsonProperty("result")]
    public int result;
    [JsonProperty("resultcount")]
    public int resultcount;
    [JsonProperty("publishedfiledetails")]
    public List<Publishedfiledetail> publishedfiledetails;
    public class Publishedfiledetail
    {
        [JsonProperty("publishedfileid")]
        public ulong publishedfileid;
        [JsonProperty("result")]
        public int result;
        [JsonProperty("creator")]
        public string creator;
        [JsonProperty("creator_app_id")]
        public int creator_app_id;
        [JsonProperty("consumer_app_id")]
        public int consumer_app_id;
        [JsonProperty("filename")]
        public string filename;
        [JsonProperty("file_size")]
        public int file_size;
        [JsonProperty("preview_url")]
        public string preview_url;
        [JsonProperty("hcontent_preview")]
        public string hcontent_preview;
        [JsonProperty("title")]
        public string title;
        [JsonProperty("description")]
        public string description;
        [JsonProperty("time_created")]
        public int time_created;
        [JsonProperty("time_updated")]
        public int time_updated;
        [JsonProperty("visibility")]
        public int visibility;
        [JsonProperty("banned")]
        public int banned;
        [JsonProperty("ban_reason")]
        public string ban_reason;
        [JsonProperty("subscriptions")]
        public int subscriptions;
        [JsonProperty("favorited")]
        public int favorited;
        [JsonProperty("lifetime_subscriptions")]
        public int lifetime_subscriptions;
        [JsonProperty("lifetime_favorited")]
        public int lifetime_favorited;
        [JsonProperty("views")]
        public int views;
        [JsonProperty("tags")]
        public List<Tag> tags;
    }

}

public class Publishedfiledetail
{
    [JsonProperty("publishedfileid")]
    public ulong publishedfileid;
    [JsonProperty("result")]
    public int result;
    [JsonProperty("creator")]
    public string creator;
    [JsonProperty("creator_app_id")]
    public int creator_app_id;
    [JsonProperty("consumer_app_id")]
    public int consumer_app_id;
    [JsonProperty("filename")]
    public string filename;
    [JsonProperty("file_size")]
    public int file_size;
    [JsonProperty("preview_url")]
    public string preview_url;
    [JsonProperty("hcontent_preview")]
    public string hcontent_preview;
    [JsonProperty("title")]
    public string title;
    [JsonProperty("description")]
    public string description;
    [JsonProperty("time_created")]
    public int time_created;
    [JsonProperty("time_updated")]
    public int time_updated;
    [JsonProperty("visibility")]
    public int visibility;
    [JsonProperty("banned")]
    public int banned;
    [JsonProperty("ban_reason")]
    public string ban_reason;
    [JsonProperty("subscriptions")]
    public int subscriptions;
    [JsonProperty("favorited")]
    public int favorited;
    [JsonProperty("lifetime_subscriptions")]
    public int lifetime_subscriptions;
    [JsonProperty("lifetime_favorited")]
    public int lifetime_favorited;
    [JsonProperty("views")]
    public int views;
    [JsonProperty("tags")]
    public List<Tag> tags;
}

public class GtCllctnDtls
{
    [JsonProperty("response")]
    public Response response;
    public class Response
    {
        [JsonProperty("result")]
        public int result;
        [JsonProperty("resultcount")]
        public int resultcount;
        [JsonProperty("collectiondetails")]
        public List<Collectiondetail> collectiondetails;
        public class Collectiondetail
        {
            [JsonProperty("publishedfileid")]
            public string publishedfileid;
            [JsonProperty("result")]
            public int result;
            [JsonProperty("children")]
            public List<Child> children;
            public class Child
            {
                [JsonProperty("publishedfileid")]
                public string publishedfileid;
                [JsonProperty("sortorder")]
                public int sortorder;
                [JsonProperty("filetype")]
                public int filetype;
            }

        }

    }

}

public class Response
{
    [JsonProperty("result")]
    public int result;
    [JsonProperty("resultcount")]
    public int resultcount;
    [JsonProperty("collectiondetails")]
    public List<Collectiondetail> collectiondetails;
    public class Collectiondetail
    {
        [JsonProperty("publishedfileid")]
        public string publishedfileid;
        [JsonProperty("result")]
        public int result;
        [JsonProperty("children")]
        public List<Child> children;
        public class Child
        {
            [JsonProperty("publishedfileid")]
            public string publishedfileid;
            [JsonProperty("sortorder")]
            public int sortorder;
            [JsonProperty("filetype")]
            public int filetype;
        }

    }

}

public class Collectiondetail
{
    [JsonProperty("publishedfileid")]
    public string publishedfileid;
    [JsonProperty("result")]
    public int result;
    [JsonProperty("children")]
    public List<Child> children;
    public class Child
    {
        [JsonProperty("publishedfileid")]
        public string publishedfileid;
        [JsonProperty("sortorder")]
        public int sortorder;
        [JsonProperty("filetype")]
        public int filetype;
    }

}

public class Child
{
    [JsonProperty("publishedfileid")]
    public string publishedfileid;
    [JsonProperty("sortorder")]
    public int sortorder;
    [JsonProperty("filetype")]
    public int filetype;
}


```

---

## SkinChanger

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("SkinChanger", "rever", "1.0.0")]
[Description("Меняет скины и имена предметов по их shortname")]
 class SkinChanger : RustPlugin
{
    public class ItemRecord
    {
        [JsonProperty(PropertyName = "Shortname")]
        public string target;
        [JsonProperty(PropertyName = "Новое имя")]
        public string name;
        [JsonProperty(PropertyName = "Новый id скина")]
        public ulong skinid;
    }

    public class Configurarion
    {
        [JsonProperty(PropertyName = "Список предметов для замены скинов и имени")]
        public List<ItemRecord> items;
    }

    public Configurarion config;
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Loaded();
     void OnItemAddedToContainer(ItemContainer container, Item item);
     void OnItemDropped(Item item, BaseEntity entity);
}

public class ItemRecord
{
    [JsonProperty(PropertyName = "Shortname")]
    public string target;
    [JsonProperty(PropertyName = "Новое имя")]
    public string name;
    [JsonProperty(PropertyName = "Новый id скина")]
    public ulong skinid;
}

public class Configurarion
{
    [JsonProperty(PropertyName = "Список предметов для замены скинов и имени")]
    public List<ItemRecord> items;
}


```

---

## Skinner-2.1.5

```csharp
using Network;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using ProtoBuf;
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using UnityEngine.Pool;
using System.Text;
using Facepunch;

Oxide.Plugins
[Info("Skinner", "Whispers88", "2.1.5")]
[Description("Brings automation and ease to skinning items")]
public class Skinner : CovalencePlugin
{
    static Skinner skinner;
    private const string permdefault;
    private const string permitems;
    private const string permcraft;
    private const string permskininv;
    private const string permskinteam;
    private const string permskinteamblock;
    private const string permskincon;
    private const string permbypassauth;
    private const string permimport;
    private const string permskinbase;
    private const string permskinall;
    private const string permskinauto;
    private const string permskinautotoggle;
    private const string permskinrequest;
    private const string permskintry;
    private List<string> permissions;
    private void OnServerInitialized();
    private void Loaded();
    private void Unload();
    public static Dictionary<ulong, string>? _skinNames;
    public static Dictionary<int, List<ulong>>? _cachedSkins;
    private static ulong maskID;
    private ulong SetMask(ulong num);
    public static ulong GetMask(ulong skinID, int itemID, bool redirectSkin);
    public static bool HasMask(ulong uID);
    public static ulong UnsetMask(ulong num);
    private void AddSkin(ulong skinID, int itemID, string displayName, int redirectID);
    private int totskins;
    private void GetSkins();
    public static List<int>? _cachedSkinKeys;
    private void UpdateImportedSkins();
    private Configuration config;
    public class Configuration
    {
        [JsonProperty("Skin Commands (skin items in you inventory")]
        public string[] cmdsskin;
        [JsonProperty("Skin Items Commands (skin items you have already placed")]
        public string[] cmdsskinitems;
        [JsonProperty("Set default items to be skinned")]
        public string[] cmdsskincraft;
        [JsonProperty("Automatically set all items in you inventory to your default skins")]
        public string[] cmdsskininv;
        [JsonProperty("Automatically set all items a container to your default skins")]
        public string[] cmdsskincon;
        [JsonProperty("Automatically skin all deployables in your base")]
        public string[] cmdskinbase;
        [JsonProperty("Automatically skin all items in your base")]
        public string[] cmdskinallitems;
        [JsonProperty("Automatically skin all items that are moved into you inventory")]
        public string[] cmdtoggleautoskin;
        [JsonProperty("Skin your teams inventories with your skin set")]
        public string[] cmdskinteam;
        [JsonProperty("Request workshop skins via workshop ID")]
        public string[] cmdskinrequest;
        [JsonProperty("Approve workshop skin requests")]
        public string[] cmdskinrequests;
        [JsonProperty("Set your selected skin set")]
        public string[] cmdskinset;
        [JsonProperty("Import Custom Skins")]
        public string[] cmdskinimport;
        [JsonProperty("Import Workshop Collection Command")]
        public string[] cmdcollectionimport;
        [JsonProperty("Skin Request Notification Discord Webhook")]
        public string DiscordWebhook;
        [JsonProperty("Custom Page Change UI Positon anchor/offset 'min x, min y', 'max x', max y'")]
        public string[] uiposition;
        [JsonProperty("Custom Searchbar UI Positon anchor/offset 'min x, min y', 'max x', max y'")]
        public string[] uisearchposition;
        [JsonProperty("Custom Set Selection UI Positon anchor/offset 'min x, min y', 'max x', max y'")]
        public string[] uisetsposition;
        [JsonProperty("Auto import approved skins")]
        public bool autoImportApproved;
        [JsonProperty("Remove player data after inactivity (days)")]
        public int removedataTime;
        [JsonProperty("Apply names of skins to skinned items")]
        public bool applySkinNames;
        [JsonProperty("Add Search Bar UI")]
        public bool searchbar;
        [JsonProperty("Use on itemcraft hook (skin items after crafting - not required when using skinauto)")]
        public bool useOnItemCraft;
        [JsonProperty("Override spraycan behaviour")]
        public bool sprayCanOveride;
        [JsonProperty("Use spraycan effect when holding spraycan and skinning deployables")]
        public bool sprayCanEffect;
        [JsonProperty("Blacklisted Skins (skinID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ulong> blacklistedskins;
        [JsonProperty("Blacklisted Items (itemID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<int> blacklisteditems;
        [JsonProperty("Import Skin collections (steam workshop ID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ulong> skinCollectionIDs;
        [JsonProperty("Command based cooldowns ('permission' : 'command' seconds")]
        public Dictionary<string, CoolDowns> Cooldowns;
        [JsonProperty("Imported Skins List", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<ulong, ImportedItem> ImportedSkinList;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private DynamicConfigFile _defaultSkins;
    private DynamicConfigFile _playerData;
    private DynamicConfigFile _skinRequestsData;
    private Dictionary<ulong, CoolDowns> _playercooldowns;
    private Dictionary<ulong, int> _playerSelectedSet;
    private void LoadData();
    private void SaveData();
    private Dictionary<ulong, Dictionary<int, ulong>[]> _playerDefaultSkins;
    private Dictionary<ulong, PlayerData> _playerUsageData;
    private List<RequestItem> _requestsData;
    public class PlayerDataDec
    {
        public Dictionary<int, List<decimal>> skinusage;
        public long lastonline { get; set; }
    }

    public class PlayerData
    {
        public Dictionary<int, List<ulong>> skinusage;
        public long lastonline { get; set; }
        public void AddSkinUsage(ulong skinID, int itemID, int rItemID);
        public List<ulong>? GetSkinUsage(int itemID);
        public void UpdateLastOnline();
    }

    public class CoolDowns
    {
        public float skin;
        public float skinitem;
        public float skincraft;
        public float skincon;
        public float skininv;
        public float skinteam;
        public float skinbase;
        public float skinall;
    }

    protected override void LoadDefaultMessages();
    private void SkinAutoCMD(IPlayer iplayer, string command, string[] args);
    private void SkinImportCMD(IPlayer iplayer, string command, string[] args);
    private void SkinImportCollection(IPlayer iplayer, string command, string[] args);
    private void SkinCMD(IPlayer iplayer, string command, string[] args);
    private void DefaultSkinsCMD(IPlayer iplayer, string command, string[] args);
    private void SkinRequestCMD(IPlayer iplayer, string command, string[] args);
    private void SkinRequestsCMD(IPlayer iplayer, string command, string[] args);
    private static WaitForSeconds _WaitForSecondsMore;
    private IEnumerator CheckforRequests(BasePlayer player, ItemContainer itemContainer, BoxController boxController);
    private static int Layermask;
    private void SkinItemCMD(IPlayer iplayer, string command, string[] args);
    private void SkinInvCMD(IPlayer iplayer, string command, string[] args);
    private void SkinTeamCMD(IPlayer iplayer, string command, string[] args);
    private void SkinConCMD(IPlayer iplayer, string command, string[] args);
    private void SkinBaseCMD(IPlayer iplayer, string command, string[] args);
    private Dictionary<int, ulong> GetCachedSkins(BasePlayer player, int set);
    private void SkinAllItemsCMD(IPlayer iplayer, string command, string[] args);
    public Dictionary<BasePlayer, BoxController> _boxcontrollers;
    private void SBNextPageCMD(IPlayer iplayer, string command, string[] args);
    private void SBBackPageCMD(IPlayer iplayer, string command, string[] args);
    private void SearchCMD(IPlayer iplayer, string command, string[] args);
    private void SetSelectCMD(IPlayer iplayer, string command, string[] args);
    private void RequestSelectCMD(IPlayer iplayer, string command, string[] args);
    private Dictionary<ulong, BoxController> _viewingcon;
    private void OnPlayerLootEnd(PlayerLoot instance);
     void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string strReason);
    private void OnItemCraftFinished(ItemCraftTask task, Item item, ItemCrafter itemCrafter);
    private object OnItemAction(Item item, string action, BasePlayer player);
    private static string _coffinPrefab;
    private BaseEntity _limitedEntity;
    private ItemContainer CreateContainer();
    private void StartLooting(BasePlayer player, ItemContainer itemContainer);
    private static readonly uint RPCOpenLootstr;
    private static readonly byte[] RPCOpenLootbytes;
    public static void RPCStartLooting(BasePlayer player);
    public void SkinItem(Item item, ulong uID);
    public BaseCombatEntity? SkinDeployable(BaseCombatEntity baseCombatEntity, ulong uID);
    private bool GetEntityPrefabPath(ItemDefinition def, string resourcePath);
     void SaveEntityStorage(BaseEntity baseEntity, Dictionary<ContainerSet, List<Item>> dictionary, int index);
     void RestoreEntityStorage(BaseEntity baseEntity, int index, Dictionary<ContainerSet, List<Item>> copy);
    private class InventoryWatcher : FacepunchBehaviour
    {
        private BasePlayer player;
         bool _enabled;
         Dictionary<int, ulong> cachedSkins;
        private void Awake();
        private void subContainerWatch();
        private void skinWatch(Item item, bool f);
        public void refreshSkins();
        private void OnDestroy();
    }

    private static int spraycanid;
     void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
    private class SpraycanController : FacepunchBehaviour
    {
        private BasePlayer player;
        private SprayCan sprayCan;
        private string skinitem;
         BUTTON _fire2;
         BaseEntity.Flags fbusy;
         float lasttime;
        private void Awake();
        private void FixedUpdate();
    }

    public Queue<Item> itemPool;
    public class BoxController : FacepunchBehaviour
    {
        public ItemContainer inventory;
        private BasePlayer player;
        private Item? mainitem;
        private Item? returnitem;
        private Item? returnitemplayer;
        private Vector3 ogpos;
        public bool _fillingbox;
        public bool _clearingbox;
        private ItemDefinition? itemselected;
        public BaseCombatEntity? maindeployable;
        public string searchtxt;
        public int setSelect;
        public string requestselected;
        public Dictionary<int, ulong> setSkins;
         bool rebuildsearchUI;
         bool rebuildPageUI;
         bool fsearchUI;
         bool fpageUI;
         bool fskinSetsUI;
         bool fskinRequestsUI;
        public string boxtype;
        private int page;
        private int scpage;
        public void StartAwake();
        private void Preremove(Item item);
        public void SkinDeplyoables(BaseCombatEntity entity, ItemDefinition itemDefinition);
        private void CheckforItemDply(Item item, bool b);
        private void GetDeployableSkins(bool skipchecks);
        private void FillSkins(ItemDefinition itemdef, bool bbreak);
        private int olditempos;
        private ItemContainer oldcon;
        private Item? posWatchItem;
        private Item? backpack;
        public void StartItemSkin();
        private Item SplitItem(Item item2, int split_Amount);
        private void OnItemAddedToStack(Item item, int amount);
        private void PosWatch(Item item);
        private void CheckforItem(Item item, bool b);
        private List<ulong>? cachedskins;
        private List<ulong>? usageskins;
        private void GetSkins(bool skipchecks);
        private void ResetCon();
        private bool searchtextwas;
        private bool PrepareSkins(ItemDefinition itemdef);
        public void QuickRemove(Item item);
        private void ItemRemoveCheck(Item item);
        public void UpdateActiveItem(BasePlayer player, Item item);
        public void SkinCraft();
        private void GetDefaultSkins();
        private void CheckforItemSelect(Item item, bool b);
        private void CheckforSkinSelect(Item item, bool b);
        public void SkinRequests();
        private void GetRequestSkins();
        private void CheckforRequestSelect(Item item, bool b);
        public void NextPage();
        public void BackPage();
        public void SetUpdate();
        public void SearchUpdate();
         bool IsMainItemBlacklisted(Item mainitem);
         bool IsMainDeployableBlacklisted(BaseCombatEntity maindeployable);
        private Item GiveItem(Item item, bool drop);
        private void Remove(Item item, bool nextTick);
        private void DoRemove(Item item);
        private void ClearCon(bool nexttick);
        private Item GetItemFromPool(ulong cachedSkin, ItemDefinition itemDef, int pos);
        private void InsertItem(ulong cachedSkin, ItemDefinition? itemDef, int amount, int pos);
        private List<Item>? GetPlayerItems(int itemid);
        public void OnDestroy();
    }

    public string searchUIstring;
    public string pageUIstring;
    public string setsUIstring;
    public string requestsUIstring;
    public class UI
    {
        public static readonly uint AddUIstr;
        public static bool AddUI(BasePlayer player, byte[] elements);
        public static readonly uint DestroyUIstr;
        public static bool DestroyUI(BasePlayer player, byte[] bytes);
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, string oMin, string oMax, bool useCursor, string parent);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string sprite, string material, string anchorMin, string anchorMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string text, int size, string anchorMin, string anchorMax, string colour, TextAnchor align);
        static public void CreateButton(CuiElementContainer container, string panel, string bgcolor, string textcolour, string text, int size, string anchorMin, string anchorMax, string command, TextAnchor align);
        static public void AddInputField(CuiElementContainer container, string panel, string bgcolor, string txtcolor, string text, int size, string command, string anchorMin, string anchorMax);
    }

    public const string SkinPageUI;
    public const string SkinSearchUI;
    public const string SkinSetsSelectUI;
    public const string SkinRequestsUI;
    public static byte[] BSkinPageUI;
    public static byte[] BSkinSearchUI;
    public static byte[] BSkinSetsSelectUI;
    public static byte[] BSkinRequestsUI;
    private void InitUI();
    private static Dictionary<ValueTuple<int, int>, byte[]> cacheduiPageDictionary;
    public byte[] AddPageUI(int minpage, int maxpage);
    private byte[] cachedsearchUIDefault;
    public byte[] AddSearchUI(string searchtxt);
    private static Dictionary<int, byte[]> cacheduiSetsDictionary;
    public byte[] AddSetsUI(int selectedset);
    private static Dictionary<string, byte[]> cacheduiRequestsDictionary;
    public byte[] AddRequestsUI(string requesttype);
    private string InitPageUI();
    public string InitSearchUI();
    public static string EscapeBrackets(string input);
    public string InitSetsUI();
    public string InitRequestsUI();
    private Message.Type RPCMessage;
    private uint refreshSkin;
    private void SendNetworkUpdate(BaseEntity ent, SendInfo sendInfo);
    private void DoRemove(Item item);
    private bool HasPerm(string id, string perm);
    private string GetLang(string langKey, string playerId, object[] args);
    private void ChatMessage(IPlayer player, string langKey, object[] args);
    private IEnumerator getCollectionscouroutine;
    private List<string> _WorkshopSkinIDCollectionList;
    private IEnumerator GetCollectionSkinIDS();
    private IEnumerator getSteamWorkshopSkinData;
    private IEnumerator GetSteamWorkshopSkinData();
    private ValueTuple<string, string>? htmllines2shortname(string[] htmlslines, string workshopid);
    private IEnumerator getSteamWorkshopRequestData;
    private IEnumerator GetSteamWorkshopSkinRequests();
    private List<DiscordData> _discordData;
    private IEnumerator notifyDiscordCoroutine;
    private IEnumerator NotifyDiscord();
    public class Root
    {
        public string content { get; set; }
        public bool tts { get; set; }
        public List<Embed> embeds { get; set; }
        public string avatar_url { get; set; }
        public string username { get; set; }
    }

    public class Embed
    {
        public int id { get; set; }
        public List<Field> fields { get; set; }
        public Author author { get; set; }
        public string title { get; set; }
        public Image image { get; set; }
        public DateTime timestamp { get; set; }
    }

    public class Field
    {
        public int id { get; set; }
        public string name { get; set; }
        public string value { get; set; }
    }

    public class Author
    {
        public string name { get; set; }
        public string icon_url { get; set; }
    }

    public class Image
    {
        public string url { get; set; }
    }

    private Dictionary<string, string> WorkshopSkinNameConversion;
    public Dictionary<int, List<ulong>> GetAllCachedSkins();
    public bool IsRedirectID(ulong uID);
    public int RedirectIDtoItemID(ulong uID);
    public List<ulong>? GetSkinsItemList(int itemid);
}

public class Configuration
{
    [JsonProperty("Skin Commands (skin items in you inventory")]
    public string[] cmdsskin;
    [JsonProperty("Skin Items Commands (skin items you have already placed")]
    public string[] cmdsskinitems;
    [JsonProperty("Set default items to be skinned")]
    public string[] cmdsskincraft;
    [JsonProperty("Automatically set all items in you inventory to your default skins")]
    public string[] cmdsskininv;
    [JsonProperty("Automatically set all items a container to your default skins")]
    public string[] cmdsskincon;
    [JsonProperty("Automatically skin all deployables in your base")]
    public string[] cmdskinbase;
    [JsonProperty("Automatically skin all items in your base")]
    public string[] cmdskinallitems;
    [JsonProperty("Automatically skin all items that are moved into you inventory")]
    public string[] cmdtoggleautoskin;
    [JsonProperty("Skin your teams inventories with your skin set")]
    public string[] cmdskinteam;
    [JsonProperty("Request workshop skins via workshop ID")]
    public string[] cmdskinrequest;
    [JsonProperty("Approve workshop skin requests")]
    public string[] cmdskinrequests;
    [JsonProperty("Set your selected skin set")]
    public string[] cmdskinset;
    [JsonProperty("Import Custom Skins")]
    public string[] cmdskinimport;
    [JsonProperty("Import Workshop Collection Command")]
    public string[] cmdcollectionimport;
    [JsonProperty("Skin Request Notification Discord Webhook")]
    public string DiscordWebhook;
    [JsonProperty("Custom Page Change UI Positon anchor/offset 'min x, min y', 'max x', max y'")]
    public string[] uiposition;
    [JsonProperty("Custom Searchbar UI Positon anchor/offset 'min x, min y', 'max x', max y'")]
    public string[] uisearchposition;
    [JsonProperty("Custom Set Selection UI Positon anchor/offset 'min x, min y', 'max x', max y'")]
    public string[] uisetsposition;
    [JsonProperty("Auto import approved skins")]
    public bool autoImportApproved;
    [JsonProperty("Remove player data after inactivity (days)")]
    public int removedataTime;
    [JsonProperty("Apply names of skins to skinned items")]
    public bool applySkinNames;
    [JsonProperty("Add Search Bar UI")]
    public bool searchbar;
    [JsonProperty("Use on itemcraft hook (skin items after crafting - not required when using skinauto)")]
    public bool useOnItemCraft;
    [JsonProperty("Override spraycan behaviour")]
    public bool sprayCanOveride;
    [JsonProperty("Use spraycan effect when holding spraycan and skinning deployables")]
    public bool sprayCanEffect;
    [JsonProperty("Blacklisted Skins (skinID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ulong> blacklistedskins;
    [JsonProperty("Blacklisted Items (itemID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<int> blacklisteditems;
    [JsonProperty("Import Skin collections (steam workshop ID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ulong> skinCollectionIDs;
    [JsonProperty("Command based cooldowns ('permission' : 'command' seconds")]
    public Dictionary<string, CoolDowns> Cooldowns;
    [JsonProperty("Imported Skins List", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<ulong, ImportedItem> ImportedSkinList;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

public class PlayerDataDec
{
    public Dictionary<int, List<decimal>> skinusage;
    public long lastonline { get; set; }
}

public class PlayerData
{
    public Dictionary<int, List<ulong>> skinusage;
    public long lastonline { get; set; }
    public void AddSkinUsage(ulong skinID, int itemID, int rItemID);
    public List<ulong>? GetSkinUsage(int itemID);
    public void UpdateLastOnline();
}

public class CoolDowns
{
    public float skin;
    public float skinitem;
    public float skincraft;
    public float skincon;
    public float skininv;
    public float skinteam;
    public float skinbase;
    public float skinall;
}

private class InventoryWatcher : FacepunchBehaviour
{
    private BasePlayer player;
     bool _enabled;
     Dictionary<int, ulong> cachedSkins;
    private void Awake();
    private void subContainerWatch();
    private void skinWatch(Item item, bool f);
    public void refreshSkins();
    private void OnDestroy();
}

private class SpraycanController : FacepunchBehaviour
{
    private BasePlayer player;
    private SprayCan sprayCan;
    private string skinitem;
     BUTTON _fire2;
     BaseEntity.Flags fbusy;
     float lasttime;
    private void Awake();
    private void FixedUpdate();
}

public class BoxController : FacepunchBehaviour
{
    public ItemContainer inventory;
    private BasePlayer player;
    private Item? mainitem;
    private Item? returnitem;
    private Item? returnitemplayer;
    private Vector3 ogpos;
    public bool _fillingbox;
    public bool _clearingbox;
    private ItemDefinition? itemselected;
    public BaseCombatEntity? maindeployable;
    public string searchtxt;
    public int setSelect;
    public string requestselected;
    public Dictionary<int, ulong> setSkins;
     bool rebuildsearchUI;
     bool rebuildPageUI;
     bool fsearchUI;
     bool fpageUI;
     bool fskinSetsUI;
     bool fskinRequestsUI;
    public string boxtype;
    private int page;
    private int scpage;
    public void StartAwake();
    private void Preremove(Item item);
    public void SkinDeplyoables(BaseCombatEntity entity, ItemDefinition itemDefinition);
    private void CheckforItemDply(Item item, bool b);
    private void GetDeployableSkins(bool skipchecks);
    private void FillSkins(ItemDefinition itemdef, bool bbreak);
    private int olditempos;
    private ItemContainer oldcon;
    private Item? posWatchItem;
    private Item? backpack;
    public void StartItemSkin();
    private Item SplitItem(Item item2, int split_Amount);
    private void OnItemAddedToStack(Item item, int amount);
    private void PosWatch(Item item);
    private void CheckforItem(Item item, bool b);
    private List<ulong>? cachedskins;
    private List<ulong>? usageskins;
    private void GetSkins(bool skipchecks);
    private void ResetCon();
    private bool searchtextwas;
    private bool PrepareSkins(ItemDefinition itemdef);
    public void QuickRemove(Item item);
    private void ItemRemoveCheck(Item item);
    public void UpdateActiveItem(BasePlayer player, Item item);
    public void SkinCraft();
    private void GetDefaultSkins();
    private void CheckforItemSelect(Item item, bool b);
    private void CheckforSkinSelect(Item item, bool b);
    public void SkinRequests();
    private void GetRequestSkins();
    private void CheckforRequestSelect(Item item, bool b);
    public void NextPage();
    public void BackPage();
    public void SetUpdate();
    public void SearchUpdate();
     bool IsMainItemBlacklisted(Item mainitem);
     bool IsMainDeployableBlacklisted(BaseCombatEntity maindeployable);
    private Item GiveItem(Item item, bool drop);
    private void Remove(Item item, bool nextTick);
    private void DoRemove(Item item);
    private void ClearCon(bool nexttick);
    private Item GetItemFromPool(ulong cachedSkin, ItemDefinition itemDef, int pos);
    private void InsertItem(ulong cachedSkin, ItemDefinition? itemDef, int amount, int pos);
    private List<Item>? GetPlayerItems(int itemid);
    public void OnDestroy();
}

public class UI
{
    public static readonly uint AddUIstr;
    public static bool AddUI(BasePlayer player, byte[] elements);
    public static readonly uint DestroyUIstr;
    public static bool DestroyUI(BasePlayer player, byte[] bytes);
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, string oMin, string oMax, bool useCursor, string parent);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string sprite, string material, string anchorMin, string anchorMax, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string text, int size, string anchorMin, string anchorMax, string colour, TextAnchor align);
    static public void CreateButton(CuiElementContainer container, string panel, string bgcolor, string textcolour, string text, int size, string anchorMin, string anchorMax, string command, TextAnchor align);
    static public void AddInputField(CuiElementContainer container, string panel, string bgcolor, string txtcolor, string text, int size, string command, string anchorMin, string anchorMax);
}

public class Root
{
    public string content { get; set; }
    public bool tts { get; set; }
    public List<Embed> embeds { get; set; }
    public string avatar_url { get; set; }
    public string username { get; set; }
}

public class Embed
{
    public int id { get; set; }
    public List<Field> fields { get; set; }
    public Author author { get; set; }
    public string title { get; set; }
    public Image image { get; set; }
    public DateTime timestamp { get; set; }
}

public class Field
{
    public int id { get; set; }
    public string name { get; set; }
    public string value { get; set; }
}

public class Author
{
    public string name { get; set; }
    public string icon_url { get; set; }
}

public class Image
{
    public string url { get; set; }
}


```

---

