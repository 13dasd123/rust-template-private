# uMod Plugins Dataset - Code Abstractions (Continued)

Chunk 3 - Generated: 2025-07-06 20:25:18

## DangerousTreasures by nivex - Dangerous event with treasure chests

```csharp
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Plugins.DangerousTreasuresExtensionMethods;
using Rust;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.Text;
using System.Text.RegularExpressions;
using UnityEngine;
using UnityEngine.AI;
using UnityEngine.SceneManagement;

Oxide.Plugins
[Info("Dangerous Treasures", "nivex", "2.4.7")]
[Description("Event with treasure chests.")]
internal class DangerousTreasures : RustPlugin
{
    [PluginReference]
     Plugin ZoneManager;
     Plugin Economics;
     Plugin ServerRewards;
     Plugin GUIAnnouncements;
     Plugin MarkerManager;
     Plugin Kits;
     Plugin Duelist;
     Plugin RaidableBases;
     Plugin AbandonedBases;
     Plugin Notify;
     Plugin AdvancedAlerts;
     Plugin Clans;
     Plugin Friends;
    private new const string Name;
    private bool wipeChestsSeed;
    private StoredData data;
    private List<int> BlockedLayers;
    private Dictionary<ulong, HumanoidBrain> HumanoidBrains;
    private Dictionary<string, MonumentInfoEx> allowedMonuments;
    private List<MonumentInfoEx> monuments;
    private Dictionary<Vector3, ZoneInfo> managedZones;
    private Dictionary<NetworkableId, TreasureChest> treasureChests;
    private Dictionary<NetworkableId, string> looters;
    private Dictionary<string, ItemDefinition> _definitions;
    private Dictionary<string, SkinInfo> Skins;
    private List<ulong> newmanProtections;
    private List<ulong> indestructibleWarnings;
    private List<ulong> drawGrants;
    private List<int> obstructionLayers;
    private List<string> _blockedColliders;
    private List<string> underground;
    private HashSet<Vector3> _gridPositions;
    private const int TARGET_MASK;
    private const int targetMask;
    private const int visibleMask;
    private const int obstructionLayer;
    private const int heightLayer;
    private StringBuilder _sb;
    private Vector3 sd_customPos;
    public class ZoneInfo
    {
        public Vector3 Position;
        public Vector3 Size;
        public float Distance;
        public OBB OBB;
    }

    public class SkinInfo
    {
        public List<ulong> skins;
        public List<ulong> workshopSkins;
        public List<ulong> importedSkins;
        public List<ulong> allSkins;
    }

    private class PlayerInfo
    {
        public int StolenChestsTotal;
        public int StolenChestsSeed;
        public PlayerInfo();
    }

    private class StoredData
    {
        public Dictionary<string, PlayerInfo> Players;
        public double SecondsUntilEvent;
        public string CustomPosition;
        public int TotalEvents;
        public StoredData();
    }

    public class HumanoidNPC : ScientistNPC
    {
        public new HumanoidBrain Brain;
        public Configuration config { get; set; }
        public new Translate.Phrase LootPanelTitle { get; set; }
        public override string Categorize();
        public override bool ShouldDropActiveItem();
        public override string displayName { get; set; }
        public override void AttackerInfo(ProtoBuf.PlayerLifeStory.DeathInfo info);
        public override void OnDied(HitInfo info);
    }

    public class HumanoidBrain : ScientistBrain
    {
        public void DisableShouldThink();
        internal DangerousTreasures Instance;
        internal string displayName;
        internal Transform NpcTransform;
        internal HumanoidNPC npc;
        internal AttackEntity _attackEntity;
        internal FlameThrower flameThrower;
        internal LiquidWeapon liquidWeapon;
        internal BaseMelee baseMelee;
        internal BaseProjectile baseProjectile;
        internal BasePlayer AttackTarget;
        internal Transform AttackTransform;
        internal TreasureChest tc;
        internal NpcSettings Settings;
        internal List<Vector3> positions;
        internal Vector3 DestinationOverride;
        internal bool isKilled;
        internal bool isMurderer;
        internal ulong uid;
        internal float lastWarpTime;
        internal float softLimitSenseRange;
        internal float nextAttackTime;
        internal float attackRange;
        internal float attackCooldown;
        internal AttackType attackType;
        internal BaseNavigator.NavigationSpeed CurrentSpeed;
        internal Vector3 AttackPosition { get; set; }
        internal Vector3 ServerPosition { get; set; }
        private Configuration config { get; set; }
        internal AttackEntity AttackEntity { get; set; }
        public void UpdateWeapon(AttackEntity attackEntity, ItemId uid);
        internal void IdentifyWeapon();
        private void SetAttackRestrictions(AttackType attackType, float attackRange, float attackCooldown, float effectiveRange);
        public bool ValidTarget { get; set; }
        public override void OnDestroy();
        public override void InitializeAI();
        public override void AddStates();
        public class AttackState : BaseAttackState
        {
            private new HumanoidBrain brain;
            private global::HumanNPC npc;
            private Transform NpcTransform;
            private IAIAttack attack { get; set; }
            public AttackState(HumanoidBrain humanoidBrain);
            public override void StateEnter(BaseAIBrain _brain, BaseEntity _entity);
            public override void StateLeave(BaseAIBrain _brain, BaseEntity _entity);
            private void StopAttacking();
            public override StateStatus StateThink(float delta, BaseAIBrain _brain, BaseEntity _entity);
            private bool InAttackRange();
            private void StartAttacking();
            private void RealisticShotTest();
        }

        private bool init;
        public void Init();
        private void Converge();
        public void Forget();
        private void RandomMove(float radius);
        public void SetupNavigator(BaseCombatEntity owner, BaseNavigator navigator, float distance);
        public Vector3 GetAimDirection();
        private void SetAimDirection();
        private void SetDestination();
        private void SetDestination(Vector3 destination);
        public bool CanUseNavMesh();
        public bool SetTarget(BasePlayer player, bool converge);
        private void TryReturnHome();
        private void TryToAttack();
        private void TryToAttack(BasePlayer attacker);
        private void TryMurdererActions();
        private void TryScientistActions();
        public void SetupMovement(List<Vector3> positions);
        private void TryToRoam();
        private bool IsStuck();
        public void Warp();
        private void UseFlameThrower();
        private void UseWaterGun();
        private void UseChainsaw();
        private void MeleeAttack();
        private bool CanConverge(global::HumanNPC other);
        private bool CanLeave(Vector3 destination);
        private bool CanSeeTarget(BasePlayer target);
        public bool CanRoam(Vector3 destination);
        private bool CanShoot();
        public BasePlayer GetBestTarget();
        private Vector3 GetRandomRoamPosition();
        private bool IsAttackOnCooldown();
        private bool IsInAttackRange(float range);
        private bool IsInHomeRange();
        private bool IsInLeaveRange(Vector3 destination);
        private bool IsInReachableRange();
        private bool IsInSenseRange(Vector3 destination);
        private bool IsInTargetRange(Vector3 destination);
        private bool ShouldForgetTarget(BasePlayer target);
    }

    private class GuidanceSystem : FacepunchBehaviour
    {
        private TimedExplosive missile;
        private ServerProjectile projectile;
        private BaseEntity target;
        private Vector3 launchPos;
        private List<ulong> newmans;
        internal DangerousTreasures Instance;
        private Configuration config { get; set; }
        private void Awake();
        public void SetTarget(BaseEntity target);
        public void Launch(float targettingTime);
        public void Exclude(List<ulong> newmans);
        private void GuideMissile();
        private void OnDestroy();
    }

    public class TreasureChest : FacepunchBehaviour
    {
        internal DangerousTreasures Instance;
        internal ulong userid;
        internal GameObject go;
        internal StorageContainer container;
        internal Vector3 containerPos;
        internal Vector3 lastFirePos;
        internal int npcMaxAmountMurderers;
        internal int npcMaxAmountScientists;
        internal int npcSpawnedAmount;
        internal int countdownTime;
        internal bool started;
        internal bool opened;
        internal bool firstEntered;
        internal bool markerCreated;
        internal bool killed;
        internal bool IsUnloading;
        internal bool requireAllNpcsDie;
        internal bool whenNpcsDie;
        internal float claimTime;
        internal float _radius;
        internal long _unlockTime;
        internal NetworkableId uid;
        private Dictionary<string, List<string>> npcKits;
        private Dictionary<ulong, float> fireticks;
        private List<FireBall> fireballs;
        private List<ulong> newmans;
        private List<ulong> traitors;
        private List<ulong> protects;
        private List<ulong> players;
        private List<TimedExplosive> missiles;
        private List<int> times;
        private List<SphereEntity> spheres;
        private List<Vector3> missilePositions;
        private List<Vector3> firePositions;
        public List<HumanoidNPC> npcs;
        private Timer destruct;
        private Timer unlock;
        private Timer countdown;
        private Timer announcement;
        private MapMarkerExplosion explosionMarker;
        private MapMarkerGenericRadius genericMarker;
        private VendingMachineMapMarker vendingMarker;
        private string FormatGridReference(Vector3 position);
        private void Message(BasePlayer player, string key, object[] args);
        private Configuration config { get; set; }
        public float Radius { get; set; }
        private void Free();
        private class NewmanTracker : FacepunchBehaviour
        {
             BasePlayer player;
             TreasureChest chest;
             DangerousTreasures Instance;
             Configuration config { get; set; }
            private void Message(BasePlayer player, string key, object[] args);
            private void Awake();
            public void Assign(DangerousTreasures instance, TreasureChest chest);
            private void Track();
            private void OnDestroy();
        }

        public void Kill(bool isUnloading);
        public bool HasRustMarker { get; set; }
        public void Awaken();
         void Awake();
        public void SpawnLoot(StorageContainer container, List<LootItem> treasure);
        private Dictionary<string, ulong> skinIds { get; set; }
        private bool IsBlacklistedSkin(ItemDefinition def, int num);
        public ulong GetItemSkin(ItemDefinition def, ulong defaultSkin, bool unique);
        public SkinInfo GetItemSkins(ItemDefinition def);
         void OnTriggerEnter(Collider col);
         void OnTriggerExit(Collider col);
        public void SpawnNpcs();
        private NavMeshHit _navHit;
        private Vector3 FindPointOnNavmesh(Vector3 target, float radius);
        private RaycastHit _hit;
        private bool IsAcceptableWaterDepth(Vector3 position);
        private bool TestInsideObject(Vector3 position);
        private bool TestInsideRock(Vector3 a);
        private bool IsRockFaceDownwards(Vector3 a);
        private bool IsRockFaceUpwards(Vector3 point);
        private bool IsRock(string name);
        private List<string> _prefabs;
        private static void CopySerializableFields(T src, T dst);
        private bool InstantiateEntity(Vector3 position, bool isMurderer, HumanoidBrain humanoidBrain, HumanoidNPC npc);
        private Vector3 RandomPosition(float radius);
        private List<Vector3> RandomWanderPositions(float radius);
        private Vector3 GetRandomPoint(float radius);
        private HumanoidNPC SpawnNpc(bool isMurderer);
        public class Loadout
        {
            public List<PlayerInventoryProperties.ItemAmountSkinned> belt;
            public List<PlayerInventoryProperties.ItemAmountSkinned> main;
            public List<PlayerInventoryProperties.ItemAmountSkinned> wear;
        }

        private PlayerInventoryProperties GetLoadout(HumanoidNPC npc, HumanoidBrain brain, bool isMurderer);
        private Loadout CreateLoadout(HumanoidNPC npc, HumanoidBrain brain, bool isMurderer);
        private void AddItemAmountSkinned(List<PlayerInventoryProperties.ItemAmountSkinned> source, List<string> shortnames);
        private void SetupNpc(HumanoidNPC npc, HumanoidBrain brain, bool isMurderer, List<Vector3> positions);
        private void GiveKit(HumanoidNPC npc, HumanoidBrain brain, bool isMurderer);
        private void UpdateItems(HumanoidNPC npc, HumanoidBrain brain, bool isMurderer);
        private bool ToggleNpcMinerHat(HumanoidNPC npc, bool state);
        public void EquipWeapon(HumanoidNPC npc, HumanoidBrain brain);
         void SetupNpcKits();
         bool IsKit(string kit);
        public void UpdateMarker();
        private void CreateGenericMarker();
        private void SafelyKill(HumanoidNPC npc);
        private void KillNpc();
        public void RemoveMapMarkers();
        private void OnDestroy();
        public void DestroyMe();
        public void LaunchMissile();
         void SpawnFire();
         void SpawnFire(Vector3 firePos);
        public void Destruct();
         void Unclaimed();
        public string GetUnlockTime(string userID);
        public void Unlock();
        public void SetUnlockTime(float time);
        public void TrySetOwner(HitInfo hitInfo);
        private void SafelyKill(BaseEntity e);
        public void DestroyLauncher();
        public void DestroySphere();
        public void DestroyFire();
    }

     void OnNewSave(string filename);
     void Init();
     void OnServerInitialized(bool isStartup);
     void Unload();
     object canTeleport(BasePlayer player);
     object CanTeleport(BasePlayer player);
     object CanBradleyApcTarget(BradleyAPC apc, HumanoidNPC npc);
     object OnEntityEnter(TriggerBase trigger, BasePlayer player);
    private object OnNpcDuck(HumanoidNPC npc);
    private object OnNpcDestinationSet(HumanoidNPC npc, Vector3 newDestination);
    private object OnNpcResume(HumanoidNPC npc);
     object OnNpcTarget(BasePlayer player, BasePlayer target);
     object OnNpcTarget(BaseNpc npc, BasePlayer target);
     object OnNpcTarget(BasePlayer target, BaseNpc npc);
     void OnEntitySpawned(BaseLock entity);
     void OnEntitySpawned(NPCPlayerCorpse corpse);
     void OnEntitySpawned(DroppedItemContainer backpack);
     void OnEntitySpawned(PlayerCorpse corpse);
     object CanBuild(Planner planner, Construction prefab, Construction.Target target);
    private bool IsAlly(ulong playerId, ulong targetId);
    private object CanLootEntity(BasePlayer player, BoxStorage container);
    private void OnItemRemovedFromContainer(ItemContainer container, Item item);
    private object CanEntityBeTargeted(BasePlayer player, BaseEntity target);
    private object CanEntityTrapTrigger(BaseTrap trap, BasePlayer player);
    private object CanEntityTakeDamage(BaseEntity entity, HitInfo hitInfo);
    private void OnEntityTakeDamage(BasePlayer player, HitInfo hitInfo);
    private void OnEntityTakeDamage(BoxStorage box, HitInfo hitInfo);
    private void ProtectionDamageHelper(HitInfo hitInfo, string key);
    private object OnNpcKits(ulong targetId);
    private bool HasNPC(ulong userID);
    private TreasureChest Get(Vector3 target);
    private bool IsTrueDamage(BaseEntity entity);
    private bool EventTerritory(Vector3 target);
    private void LoadData();
     void TryWipeData();
     void BlockZoneManagerZones(bool show);
    private class MonumentInfoEx
    {
        public MonumentInfo monument;
        public Vector3 position;
        public float radius;
        public string name;
        public string prefab;
        public MonumentInfoEx();
        public MonumentInfoEx(MonumentInfo monument, Vector3 position, float radius, string name, string prefab);
        public bool IsInBounds(Vector3 target);
    }

    private Coroutine _cmc;
    private void InitializeMonuments();
    private IEnumerator SetupMonuments();
    public IEnumerator CalculateMonumentSize(MonumentInfo monument, Vector3 from, string text, string prefab);
    public bool ContainsTopology(TerrainTopology.Enum mask, Vector3 position, float radius);
    public List<Vector3> GetCircumferencePositions(Vector3 center, float radius, float next);
    private void SortMonuments();
     void InitializeSkins();
     void StartAutomation();
    private static PooledList<T> FindEntitiesOfType(Vector3 a, float n, int m);
     void NpcDamageHelper(BasePlayer player, HitInfo hitInfo);
    private static bool InRange2D(Vector3 a, Vector3 b, float distance);
    private static bool InRange(Vector3 a, Vector3 b, float distance);
     bool IsMelee(BasePlayer player);
     void SaveData();
    protected new static void Puts(string format, object[] args);
     void SubscribeHooks(bool flag);
    private static List<Vector3> GetRandomPositions(Vector3 destination, float radius, int amount, float y);
    private bool IsInsideBounds(OBB obb, Vector3 worldPos);
    public Vector3 GetEventPosition();
     Vector3 TryGetMonumentDropPosition();
     bool IsTooClose(Vector3 vector, float multi);
     bool IsZoneBlocked(Vector3 vector);
     bool IsSafeZone(Vector3 a);
     Vector3 GetSafeDropPosition(Vector3 position);
     float GetSpawnHeight(Vector3 target, bool flag, bool draw);
     bool IsLayerBlocked(Vector3 position, float radius, int mask);
     Vector3 GetRandomMonumentDropPosition(Vector3 position);
     bool IsMonumentPosition(Vector3 target);
     Vector3 GetMonumentDropPosition(int retry);
    private void SetupPositions();
    private bool IsPositionBlocked(Vector3 pos);
    public Vector3 RandomDropPosition();
     TreasureChest TryOpenEvent(BasePlayer player);
     void AnnounceEventSpawn();
     void AnnounceEventSpawn(StorageContainer container, float unlockTime, string posStr);
     void API_SetContainer(StorageContainer container, float radius, bool spawnNpcs);
     void CheckSecondsUntilEvent();
    public string FormatGridReference(Vector3 position, bool showGrid);
    private string FormatTime(double seconds, string id);
     bool AssignTreasureHunters(List<KeyValuePair<string, int>> ladder);
     void DrawText(BasePlayer player, Vector3 drawPos, string text);
     void AddItem(BasePlayer player, string[] args);
     void cmdTreasureHunter(BasePlayer player, string command, string[] args);
    private void ccmdDangerousTreasures(ConsoleSystem.Arg arg);
     void cmdDangerousTreasures(BasePlayer player, string command, string[] args);
     Dictionary<string, string> GetMessages();
    protected override void LoadDefaultMessages();
    private int GetPercentIncreasedAmount(int amount);
    public static Color __(string hex);
    private string _(string key, string id, object[] args);
    private Regex IndexRegex;
    public string Format(string format, object[] args);
    private string msg(string key, string id, object[] args);
    private string msg2(string key, string id, object[] args);
    private string RemoveFormatting(string source);
    private void Message(BasePlayer player, string key, object[] args);
    private void Message(IPlayer user, string key, object[] args);
    private void Message(ConsoleSystem.Arg arg, string key, object[] args);
    public class Notification
    {
        public BasePlayer player;
        public string messageEx;
    }

    private Dictionary<ulong, List<Notification>> _notifications;
    private void CheckNotifications();
    private void SendNotification(Notification notification);
    private Configuration config;
    private static List<LootItem> DefaultLoot { get; set; }
    public class PluginSettings
    {
        [JsonProperty(PropertyName = "Permission Name")]
        public string PermName { get; set; }
        [JsonProperty(PropertyName = "Event Chat Command")]
        public string EventChatCommand { get; set; }
        [JsonProperty(PropertyName = "Distance Chat Command")]
        public string DistanceChatCommand { get; set; }
        [JsonProperty(PropertyName = "Draw Location On Screen With Distance Command")]
        public bool AllowDrawText { get; set; }
        [JsonProperty(PropertyName = "Event Console Command")]
        public string EventConsoleCommand { get; set; }
        [JsonProperty(PropertyName = "Show X Z Coordinates")]
        public bool ShowXZ { get; set; }
        [JsonProperty(PropertyName = "Show Grid Coordinates")]
        public bool ShowGrid { get; set; }
        [JsonProperty(PropertyName = "Grids To Block Spawns At", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> BlockedGrids;
        [JsonProperty(PropertyName = "Block Spawns At Positions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ManagementSettingsLocations> BlockedPositions;
    }

    public class ManagementSettingsLocations
    {
        [JsonProperty(PropertyName = "position")]
        [JsonConverter(typeof(UnityVector3Converter))]
        public Vector3 position;
        public float radius;
        public ManagementSettingsLocations();
        public ManagementSettingsLocations(Vector3 position, float radius);
    }

    private class UnityVector3Converter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

    public class CountdownSettings
    {
        [JsonProperty(PropertyName = "Use Countdown Before Event Starts")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Time In Seconds", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<int> Times { get; set; }
    }

    public class EventSettings
    {
        [JsonProperty(PropertyName = "Allow Player Bags To Be Lootable At Events")]
        public bool PlayersLootable;
        [JsonProperty(PropertyName = "Automated")]
        public bool Automated { get; set; }
        [JsonProperty(PropertyName = "Every Min Seconds")]
        public float IntervalMin { get; set; }
        [JsonProperty(PropertyName = "Every Max Seconds")]
        public float IntervalMax { get; set; }
        [JsonProperty(PropertyName = "Use Vending Map Marker")]
        public bool MarkerVending { get; set; }
        [JsonProperty(PropertyName = "Use Marker Manager Plugin")]
        public bool MarkerManager { get; set; }
        [JsonProperty(PropertyName = "Use Explosion Map Marker")]
        public bool MarkerExplosion { get; set; }
        [JsonProperty(PropertyName = "Marker Color")]
        public string MarkerColor { get; set; }
        [JsonProperty(PropertyName = "Marker Radius")]
        public float MarkerRadius { get; set; }
        [JsonProperty(PropertyName = "Marker Radius (Smaller Maps)")]
        public float MarkerRadiusSmall { get; set; }
        [JsonProperty(PropertyName = "Marker Event Name")]
        public string MarkerName { get; set; }
        [JsonProperty(PropertyName = "Max Manual Events")]
        public int Max { get; set; }
        [JsonProperty(PropertyName = "Always Spawn Max Manual Events")]
        public bool SpawnMax { get; set; }
        [JsonProperty(PropertyName = "Stagger Spawns Every X Seconds")]
        public float Stagger { get; set; }
        [JsonProperty(PropertyName = "Amount Of Items To Spawn")]
        public int TreasureAmount { get; set; }
        [JsonProperty(PropertyName = "Use Spheres")]
        public bool Spheres { get; set; }
        [JsonProperty(PropertyName = "Amount Of Spheres")]
        public int SphereAmount { get; set; }
        [JsonProperty(PropertyName = "Destroy Spheres When Event Starts")]
        public bool DestroySphereOnStart { get; set; }
        [JsonProperty(PropertyName = "Player Limit For Event")]
        public int PlayerLimit { get; set; }
        [JsonProperty(PropertyName = "Fire Aura Radius (Advanced Users Only)")]
        public float Radius { get; set; }
        [JsonProperty(PropertyName = "Auto Draw On New Event For Nearby Players")]
        public bool DrawTreasureIfNearby { get; set; }
        [JsonProperty(PropertyName = "Auto Draw Minimum Distance")]
        public float AutoDrawDistance { get; set; }
        [JsonProperty(PropertyName = "Grant DDRAW temporarily to players")]
        public bool GrantDraw { get; set; }
        [JsonProperty(PropertyName = "Grant Draw Time")]
        public float DrawTime { get; set; }
        [JsonProperty(PropertyName = "Time To Loot")]
        public float DestructTime { get; set; }
    }

    public class UIAdvancedAlertSettings
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Anchor Min")]
        public string AnchorMin { get; set; }
        [JsonProperty(PropertyName = "Anchor Max")]
        public string AnchorMax { get; set; }
        [JsonProperty(PropertyName = "Time Shown")]
        public float Time { get; set; }
    }

    public class EventMessageSettings
    {
        [JsonProperty(PropertyName = "Advanced Alerts UI")]
        public UIAdvancedAlertSettings AA { get; set; }
        [JsonProperty(PropertyName = "Notify Plugin - Type (-1 = disabled)")]
        public int NotifyType { get; set; }
        [JsonProperty(PropertyName = "UI Popup Interval")]
        public float Interval { get; set; }
        [JsonProperty(PropertyName = "Show Noob Warning Message")]
        public bool NoobWarning { get; set; }
        [JsonProperty(PropertyName = "Show Barrage Message")]
        public bool Barrage { get; set; }
        [JsonProperty(PropertyName = "Show Despawn Message")]
        public bool Destruct { get; set; }
        [JsonProperty(PropertyName = "Show You Have Entered")]
        public bool Entered { get; set; }
        [JsonProperty(PropertyName = "Show First Player Entered")]
        public bool FirstEntered { get; set; }
        [JsonProperty(PropertyName = "Show First Player Opened")]
        public bool FirstOpened { get; set; }
        [JsonProperty(PropertyName = "Show Opened Message")]
        public bool Opened { get; set; }
        [JsonProperty(PropertyName = "Show Prefix")]
        public bool Prefix { get; set; }
        [JsonProperty(PropertyName = "Show Started Message")]
        public bool Started { get; set; }
        [JsonProperty(PropertyName = "Show Thief Message")]
        public bool Thief { get; set; }
        [JsonProperty(PropertyName = "Send Messages To Player")]
        public bool Message { get; set; }
    }

    public class FireballSettings
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Damage Per Second")]
        public float DamagePerSecond { get; set; }
        [JsonProperty(PropertyName = "Lifetime Min")]
        public float LifeTimeMin { get; set; }
        [JsonProperty(PropertyName = "Lifetime Max")]
        public float LifeTimeMax { get; set; }
        [JsonProperty(PropertyName = "Radius")]
        public float Radius { get; set; }
        [JsonProperty(PropertyName = "Tick Rate")]
        public float TickRate { get; set; }
        [JsonProperty(PropertyName = "Generation")]
        public float Generation { get; set; }
        [JsonProperty(PropertyName = "Water To Extinguish")]
        public int WaterToExtinguish { get; set; }
        [JsonProperty(PropertyName = "Spawn Every X Seconds")]
        public int SecondsBeforeTick { get; set; }
    }

    public class GUIAnnouncementSettings
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Text Color")]
        public string TextColor { get; set; }
        [JsonProperty(PropertyName = "Banner Tint Color")]
        public string TintColor { get; set; }
        [JsonProperty(PropertyName = "Maximum Distance")]
        public float Distance { get; set; }
    }

    public class MissileLauncherSettings
    {
        [JsonProperty(PropertyName = "Acquire Time In Seconds")]
        public float TargettingTime { get; set; }
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Damage Per Missile")]
        public float Damage { get; set; }
        [JsonProperty(PropertyName = "Detection Distance")]
        public float Distance { get; set; }
        [JsonProperty(PropertyName = "Life Time In Seconds")]
        public float Lifetime { get; set; }
        [JsonProperty(PropertyName = "Ignore Flying Players")]
        public bool IgnoreFlying { get; set; }
        [JsonProperty(PropertyName = "Spawn Every X Seconds")]
        public float Frequency { get; set; }
        [JsonProperty(PropertyName = "Target Chest If No Player Target")]
        public bool TargetChest { get; set; }
    }

    public class MonumentSettings
    {
        [JsonProperty(PropertyName = "Blacklisted Monuments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, bool> Blacklist { get; set; }
        [JsonProperty(PropertyName = "Auto Spawn At Monuments Only")]
        public bool Only { get; set; }
        [JsonProperty(PropertyName = "Chance To Spawn At Monuments Instead")]
        public float Chance { get; set; }
        [JsonProperty(PropertyName = "Allow Treasure Loot Underground")]
        public bool Underground { get; set; }
    }

    public class NewmanModeSettings
    {
        [JsonProperty(PropertyName = "Protect Nakeds From Fire Aura")]
        public bool Aura { get; set; }
        [JsonProperty(PropertyName = "Protect Nakeds From Other Harm")]
        public bool Harm { get; set; }
    }

    public class NpcKitSettings
    {
        [JsonProperty(PropertyName = "Helm", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Helm;
        [JsonProperty(PropertyName = "Torso", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Torso;
        [JsonProperty(PropertyName = "Pants", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Pants;
        [JsonProperty(PropertyName = "Gloves", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Gloves;
        [JsonProperty(PropertyName = "Boots", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Boots;
        [JsonProperty(PropertyName = "Shirt", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Shirt;
        [JsonProperty(PropertyName = "Kilts", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Kilts;
        [JsonProperty(PropertyName = "Weapon", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Weapon;
    }

    public class NpcLootSettings
    {
        [JsonProperty(PropertyName = "Prefab ID List", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> IDs { get; set; }
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Disable All Prefab Loot Spawns")]
        public bool None { get; set; }
        public uint GetRandom();
    }

    public class NpcSettingsAccuracy
    {
        [JsonProperty(PropertyName = "AK47")]
        public double AK47 { get; set; }
        [JsonProperty(PropertyName = "AK47 ICE")]
        public double AK47ICE { get; set; }
        [JsonProperty(PropertyName = "Bolt Rifle")]
        public double BOLT_RIFLE { get; set; }
        [JsonProperty(PropertyName = "Compound Bow")]
        public double COMPOUND_BOW { get; set; }
        [JsonProperty(PropertyName = "Crossbow")]
        public double CROSSBOW { get; set; }
        [JsonProperty(PropertyName = "Double Barrel Shotgun")]
        public double DOUBLE_SHOTGUN { get; set; }
        [JsonProperty(PropertyName = "Eoka")]
        public double EOKA { get; set; }
        [JsonProperty(PropertyName = "Glock")]
        public double GLOCK { get; set; }
        [JsonProperty(PropertyName = "HMLMG")]
        public double HMLMG { get; set; }
        [JsonProperty(PropertyName = "L96")]
        public double L96 { get; set; }
        [JsonProperty(PropertyName = "LR300")]
        public double LR300 { get; set; }
        [JsonProperty(PropertyName = "M249")]
        public double M249 { get; set; }
        [JsonProperty(PropertyName = "M39")]
        public double M39 { get; set; }
        [JsonProperty(PropertyName = "M92")]
        public double M92 { get; set; }
        [JsonProperty(PropertyName = "MP5")]
        public double MP5 { get; set; }
        [JsonProperty(PropertyName = "Nailgun")]
        public double NAILGUN { get; set; }
        [JsonProperty(PropertyName = "Pump Shotgun")]
        public double PUMP_SHOTGUN { get; set; }
        [JsonProperty(PropertyName = "Python")]
        public double PYTHON { get; set; }
        [JsonProperty(PropertyName = "Revolver")]
        public double REVOLVER { get; set; }
        [JsonProperty(PropertyName = "Semi Auto Pistol")]
        public double SEMI_AUTO_PISTOL { get; set; }
        [JsonProperty(PropertyName = "Semi Auto Rifle")]
        public double SEMI_AUTO_RIFLE { get; set; }
        [JsonProperty(PropertyName = "Spas12")]
        public double SPAS12 { get; set; }
        [JsonProperty(PropertyName = "Speargun")]
        public double SPEARGUN { get; set; }
        [JsonProperty(PropertyName = "SMG")]
        public double SMG { get; set; }
        [JsonProperty(PropertyName = "Snowball Gun")]
        public double SNOWBALL_GUN { get; set; }
        [JsonProperty(PropertyName = "Thompson")]
        public double THOMPSON { get; set; }
        [JsonProperty(PropertyName = "Waterpipe Shotgun")]
        public double WATERPIPE_SHOTGUN { get; set; }
        public NpcSettingsAccuracy(double accuracy);
        public double Get(HumanoidBrain brain);
    }

    public class NpcSettingsMurderer
    {
        [JsonProperty(PropertyName = "Random Names", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> RandomNames { get; set; }
        [JsonProperty(PropertyName = "Items)")]
        public NpcKitSettings Items { get; set; }
        [JsonProperty(PropertyName = "Kits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Kits { get; set; }
        [JsonProperty(PropertyName = "Spawn Alternate Loot")]
        public NpcLootSettings Alternate { get; set; }
        [JsonProperty(PropertyName = "Weapon Accuracy (0 - 100)")]
        public NpcSettingsAccuracy Accuracy { get; set; }
        [JsonProperty(PropertyName = "Aggression Range")]
        public float AggressionRange { get; set; }
        [JsonProperty(PropertyName = "Despawn Inventory On Death")]
        public bool DespawnInventory { get; set; }
        [JsonProperty(PropertyName = "Corpse Despawn Time When Despawn Inventory On Death")]
        public float DespawnInventoryTime { get; set; }
        [JsonProperty(PropertyName = "Corpse Despawn Time Otherwise")]
        public float CorpseDespawnTime { get; set; }
        [JsonProperty(PropertyName = "Die Instantly From Headshots")]
        public bool Headshot { get; set; }
        [JsonProperty(PropertyName = "Amount To Spawn (min)")]
        public int SpawnMinAmount { get; set; }
        [JsonProperty(PropertyName = "Amount To Spawn (max)")]
        public int SpawnAmount { get; set; }
        [JsonProperty(PropertyName = "Health")]
        public float Health { get; set; }
    }

    public class NpcSettingsScientist
    {
        [JsonProperty(PropertyName = "Random Names", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> RandomNames { get; set; }
        [JsonProperty(PropertyName = "Items")]
        public NpcKitSettings Items { get; set; }
        [JsonProperty(PropertyName = "Kits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Kits { get; set; }
        [JsonProperty(PropertyName = "Spawn Alternate Loot")]
        public NpcLootSettings Alternate { get; set; }
        [JsonProperty(PropertyName = "Weapon Accuracy (0 - 100)")]
        public NpcSettingsAccuracy Accuracy { get; set; }
        [JsonProperty(PropertyName = "Aggression Range")]
        public float AggressionRange { get; set; }
        [JsonProperty(PropertyName = "Despawn Inventory On Death")]
        public bool DespawnInventory { get; set; }
        [JsonProperty(PropertyName = "Corpse Despawn Time When Despawn Inventory On Death")]
        public float DespawnInventoryTime { get; set; }
        [JsonProperty(PropertyName = "Corpse Despawn Time Otherwise")]
        public float CorpseDespawnTime { get; set; }
        [JsonProperty(PropertyName = "Die Instantly From Headshots")]
        public bool Headshot { get; set; }
        [JsonProperty(PropertyName = "Amount To Spawn (min)")]
        public int SpawnMinAmount { get; set; }
        [JsonProperty(PropertyName = "Amount To Spawn (max)")]
        public int SpawnAmount { get; set; }
        [JsonProperty(PropertyName = "Health (100 min, 5000 max)")]
        public float Health { get; set; }
    }

    public class NpcSettings
    {
        [JsonProperty(PropertyName = "Blacklisted Monuments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, bool> BlacklistedMonuments { get; set; }
        [JsonProperty(PropertyName = "Murderers")]
        public NpcSettingsMurderer Murderers { get; set; }
        [JsonProperty(PropertyName = "Scientists")]
        public NpcSettingsScientist Scientists { get; set; }
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Allow Npcs To Leave Dome When Attacking")]
        public bool CanLeave { get; set; }
        [JsonProperty(PropertyName = "Allow Npcs To Target Other Npcs")]
        public bool TargetNpcs { get; set; }
        [JsonProperty(PropertyName = "Block Damage From Players Beyond X Distance (0 = disabled)")]
        public float Range { get; set; }
        [JsonProperty(PropertyName = "Block Npc Kits Plugin")]
        public bool BlockNpcKits { get; set; }
        [JsonProperty(PropertyName = "Kill Underwater Npcs")]
        public bool KillUnderwater { get; set; }
    }

    public class PasteOption
    {
        [JsonProperty(PropertyName = "Option")]
        public string Key { get; set; }
        [JsonProperty(PropertyName = "Value")]
        public string Value { get; set; }
    }

    public class RankedLadderSettings
    {
        [JsonProperty(PropertyName = "Award Top X Players On Wipe")]
        public int Amount { get; set; }
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Group Name")]
        public string Group { get; set; }
        [JsonProperty(PropertyName = "Permission Name")]
        public string Permission { get; set; }
    }

    public class RewardSettings
    {
        [JsonProperty(PropertyName = "Economics Money")]
        public double Money { get; set; }
        [JsonProperty(PropertyName = "ServerRewards Points")]
        public double Points { get; set; }
        [JsonProperty(PropertyName = "Use Economics")]
        public bool Economics { get; set; }
        [JsonProperty(PropertyName = "Use ServerRewards")]
        public bool ServerRewards { get; set; }
    }

    public class RocketOpenerSettings
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Rockets")]
        public int Amount { get; set; }
        [JsonProperty(PropertyName = "Speed")]
        public float Speed { get; set; }
        [JsonProperty(PropertyName = "Use Fire Rockets")]
        public bool FireRockets { get; set; }
    }

    public class SkinSettings
    {
        [JsonProperty(PropertyName = "Custom Skins", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ulong> Custom { get; set; }
        [JsonProperty(PropertyName = "Use Random Skin")]
        public bool RandomSkins { get; set; }
        [JsonProperty(PropertyName = "Preset Skin")]
        public ulong PresetSkin { get; set; }
        [JsonProperty(PropertyName = "Include Workshop Skins")]
        public bool RandomWorkshopSkins { get; set; }
        [JsonProperty(PropertyName = "Randomize Npc Item Skins")]
        public bool Npcs { get; set; }
        [JsonProperty(PropertyName = "Use Identical Skins For All Npcs")]
        public bool UniqueNpcs { get; set; }
    }

    public class LootItem
    {
        public string shortname { get; set; }
        public string name { get; set; }
        public int amount { get; set; }
        public ulong skin { get; set; }
        public int amountMin { get; set; }
        public float condition { get; set; }
        public float probability { get; set; }
        [JsonProperty(PropertyName = "Skins", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ulong> skins { get; set; }
    }

    public class TreasureSettings
    {
        [JsonProperty(PropertyName = "Loot", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<LootItem> Loot { get; set; }
        [JsonProperty(PropertyName = "Minimum Percent Loss")]
        public decimal PercentLoss { get; set; }
        [JsonProperty(PropertyName = "Percent Increase When Using Day Of Week Loot")]
        public bool Increased { get; set; }
        [JsonProperty(PropertyName = "Use Random Skins")]
        public bool RandomSkins { get; set; }
        [JsonProperty(PropertyName = "Include Workshop Skins")]
        public bool RandomWorkshopSkins { get; set; }
        [JsonProperty(PropertyName = "Day Of Week Loot Monday", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<LootItem> DOWL_Monday { get; set; }
        [JsonProperty(PropertyName = "Day Of Week Loot Tuesday", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<LootItem> DOWL_Tuesday { get; set; }
        [JsonProperty(PropertyName = "Day Of Week Loot Wednesday", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<LootItem> DOWL_Wednesday { get; set; }
        [JsonProperty(PropertyName = "Day Of Week Loot Thursday", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<LootItem> DOWL_Thursday { get; set; }
        [JsonProperty(PropertyName = "Day Of Week Loot Friday", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<LootItem> DOWL_Friday { get; set; }
        [JsonProperty(PropertyName = "Day Of Week Loot Saturday", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<LootItem> DOWL_Saturday { get; set; }
        [JsonProperty(PropertyName = "Day Of Week Loot Sunday", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<LootItem> DOWL_Sunday { get; set; }
        [JsonProperty(PropertyName = "Use Day Of Week Loot")]
        public bool UseDOWL { get; set; }
        [JsonProperty(PropertyName = "Percent Increase On Monday")]
        public decimal PercentIncreaseOnMonday { get; set; }
        [JsonProperty(PropertyName = "Percent Increase On Tuesday")]
        public decimal PercentIncreaseOnTuesday { get; set; }
        [JsonProperty(PropertyName = "Percent Increase On Wednesday")]
        public decimal PercentIncreaseOnWednesday { get; set; }
        [JsonProperty(PropertyName = "Percent Increase On Thursday")]
        public decimal PercentIncreaseOnThursday { get; set; }
        [JsonProperty(PropertyName = "Percent Increase On Friday")]
        public decimal PercentIncreaseOnFriday { get; set; }
        [JsonProperty(PropertyName = "Percent Increase On Saturday")]
        public decimal PercentIncreaseOnSaturday { get; set; }
        [JsonProperty(PropertyName = "Percent Increase On Sunday")]
        public decimal PercentIncreaseOnSunday { get; set; }
    }

    public class TruePVESettings
    {
        [JsonProperty(PropertyName = "Allow Building Damage At Events")]
        public bool AllowBuildingDamageAtEvents { get; set; }
        [JsonProperty(PropertyName = "Allow PVP At Events")]
        public bool AllowPVPAtEvents { get; set; }
        [JsonProperty(PropertyName = "Allow PVP Server-Wide During Events")]
        public bool ServerWidePVP { get; set; }
    }

    public class UnlockSettings
    {
        [JsonProperty(PropertyName = "Min Seconds")]
        public float MinTime { get; set; }
        [JsonProperty(PropertyName = "Max Seconds")]
        public float MaxTime { get; set; }
        [JsonProperty(PropertyName = "Unlock When Npcs Die")]
        public bool WhenNpcsDie { get; set; }
        [JsonProperty(PropertyName = "Require All Npcs Die Before Unlocking")]
        public bool RequireAllNpcsDie { get; set; }
        [JsonProperty(PropertyName = "Lock Event To Player On Npc Death")]
        public bool LockToPlayerOnNpcDeath { get; set; }
        [JsonProperty(PropertyName = "Lock Event To Player On First Entered")]
        public bool LockToPlayerFirstEntered { get; set; }
    }

    public class UnlootedAnnouncementSettings
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Notify Every X Minutes (Minimum 1)")]
        public float Interval { get; set; }
    }

    public class Configuration
    {
        [JsonProperty(PropertyName = "Settings")]
        public PluginSettings Settings;
        [JsonProperty(PropertyName = "Countdown")]
        public CountdownSettings Countdown;
        [JsonProperty(PropertyName = "Events")]
        public EventSettings Event;
        [JsonProperty(PropertyName = "Event Messages")]
        public EventMessageSettings EventMessages;
        [JsonProperty(PropertyName = "Fireballs")]
        public FireballSettings Fireballs;
        [JsonProperty(PropertyName = "GUIAnnouncements")]
        public GUIAnnouncementSettings GUIAnnouncement;
        [JsonProperty(PropertyName = "Monuments")]
        public MonumentSettings Monuments;
        [JsonProperty(PropertyName = "Newman Mode")]
        public NewmanModeSettings NewmanMode;
        [JsonProperty(PropertyName = "NPCs")]
        public NpcSettings NPC;
        [JsonProperty(PropertyName = "Missile Launcher")]
        public MissileLauncherSettings MissileLauncher;
        [JsonProperty(PropertyName = "Ranked Ladder")]
        public RankedLadderSettings RankedLadder;
        [JsonProperty(PropertyName = "Rewards")]
        public RewardSettings Rewards;
        [JsonProperty(PropertyName = "Rocket Opener")]
        public RocketOpenerSettings Rocket;
        [JsonProperty(PropertyName = "Skins")]
        public SkinSettings Skins;
        [JsonProperty(PropertyName = "Treasure")]
        public TreasureSettings Treasure;
        [JsonProperty(PropertyName = "TruePVE")]
        public TruePVESettings TruePVE;
        [JsonProperty(PropertyName = "Unlock Time")]
        public UnlockSettings Unlock;
        [JsonProperty(PropertyName = "Unlooted Announcements")]
        public UnlootedAnnouncementSettings UnlootedAnnouncements;
    }

    protected override void LoadConfig();
    private void ValidateConfig();
     List<LootItem> ChestLoot { get; set; }
    private bool canSaveConfig;
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
}

public class ZoneInfo
{
    public Vector3 Position;
    public Vector3 Size;
    public float Distance;
    public OBB OBB;
}

public class SkinInfo
{
    public List<ulong> skins;
    public List<ulong> workshopSkins;
    public List<ulong> importedSkins;
    public List<ulong> allSkins;
}

private class PlayerInfo
{
    public int StolenChestsTotal;
    public int StolenChestsSeed;
    public PlayerInfo();
}

private class StoredData
{
    public Dictionary<string, PlayerInfo> Players;
    public double SecondsUntilEvent;
    public string CustomPosition;
    public int TotalEvents;
    public StoredData();
}

public class HumanoidNPC : ScientistNPC
{
    public new HumanoidBrain Brain;
    public Configuration config { get; set; }
    public new Translate.Phrase LootPanelTitle { get; set; }
    public override string Categorize();
    public override bool ShouldDropActiveItem();
    public override string displayName { get; set; }
    public override void AttackerInfo(ProtoBuf.PlayerLifeStory.DeathInfo info);
    public override void OnDied(HitInfo info);
}

public class HumanoidBrain : ScientistBrain
{
    public void DisableShouldThink();
    internal DangerousTreasures Instance;
    internal string displayName;
    internal Transform NpcTransform;
    internal HumanoidNPC npc;
    internal AttackEntity _attackEntity;
    internal FlameThrower flameThrower;
    internal LiquidWeapon liquidWeapon;
    internal BaseMelee baseMelee;
    internal BaseProjectile baseProjectile;
    internal BasePlayer AttackTarget;
    internal Transform AttackTransform;
    internal TreasureChest tc;
    internal NpcSettings Settings;
    internal List<Vector3> positions;
    internal Vector3 DestinationOverride;
    internal bool isKilled;
    internal bool isMurderer;
    internal ulong uid;
    internal float lastWarpTime;
    internal float softLimitSenseRange;
    internal float nextAttackTime;
    internal float attackRange;
    internal float attackCooldown;
    internal AttackType attackType;
    internal BaseNavigator.NavigationSpeed CurrentSpeed;
    internal Vector3 AttackPosition { get; set; }
    internal Vector3 ServerPosition { get; set; }
    private Configuration config { get; set; }
    internal AttackEntity AttackEntity { get; set; }
    public void UpdateWeapon(AttackEntity attackEntity, ItemId uid);
    internal void IdentifyWeapon();
    private void SetAttackRestrictions(AttackType attackType, float attackRange, float attackCooldown, float effectiveRange);
    public bool ValidTarget { get; set; }
    public override void OnDestroy();
    public override void InitializeAI();
    public override void AddStates();
    public class AttackState : BaseAttackState
    {
        private new HumanoidBrain brain;
        private global::HumanNPC npc;
        private Transform NpcTransform;
        private IAIAttack attack { get; set; }
        public AttackState(HumanoidBrain humanoidBrain);
        public override void StateEnter(BaseAIBrain _brain, BaseEntity _entity);
        public override void StateLeave(BaseAIBrain _brain, BaseEntity _entity);
        private void StopAttacking();
        public override StateStatus StateThink(float delta, BaseAIBrain _brain, BaseEntity _entity);
        private bool InAttackRange();
        private void StartAttacking();
        private void RealisticShotTest();
    }

    private bool init;
    public void Init();
    private void Converge();
    public void Forget();
    private void RandomMove(float radius);
    public void SetupNavigator(BaseCombatEntity owner, BaseNavigator navigator, float distance);
    public Vector3 GetAimDirection();
    private void SetAimDirection();
    private void SetDestination();
    private void SetDestination(Vector3 destination);
    public bool CanUseNavMesh();
    public bool SetTarget(BasePlayer player, bool converge);
    private void TryReturnHome();
    private void TryToAttack();
    private void TryToAttack(BasePlayer attacker);
    private void TryMurdererActions();
    private void TryScientistActions();
    public void SetupMovement(List<Vector3> positions);
    private void TryToRoam();
    private bool IsStuck();
    public void Warp();
    private void UseFlameThrower();
    private void UseWaterGun();
    private void UseChainsaw();
    private void MeleeAttack();
    private bool CanConverge(global::HumanNPC other);
    private bool CanLeave(Vector3 destination);
    private bool CanSeeTarget(BasePlayer target);
    public bool CanRoam(Vector3 destination);
    private bool CanShoot();
    public BasePlayer GetBestTarget();
    private Vector3 GetRandomRoamPosition();
    private bool IsAttackOnCooldown();
    private bool IsInAttackRange(float range);
    private bool IsInHomeRange();
    private bool IsInLeaveRange(Vector3 destination);
    private bool IsInReachableRange();
    private bool IsInSenseRange(Vector3 destination);
    private bool IsInTargetRange(Vector3 destination);
    private bool ShouldForgetTarget(BasePlayer target);
}

public class AttackState : BaseAttackState
{
    private new HumanoidBrain brain;
    private global::HumanNPC npc;
    private Transform NpcTransform;
    private IAIAttack attack { get; set; }
    public AttackState(HumanoidBrain humanoidBrain);
    public override void StateEnter(BaseAIBrain _brain, BaseEntity _entity);
    public override void StateLeave(BaseAIBrain _brain, BaseEntity _entity);
    private void StopAttacking();
    public override StateStatus StateThink(float delta, BaseAIBrain _brain, BaseEntity _entity);
    private bool InAttackRange();
    private void StartAttacking();
    private void RealisticShotTest();
}

private class GuidanceSystem : FacepunchBehaviour
{
    private TimedExplosive missile;
    private ServerProjectile projectile;
    private BaseEntity target;
    private Vector3 launchPos;
    private List<ulong> newmans;
    internal DangerousTreasures Instance;
    private Configuration config { get; set; }
    private void Awake();
    public void SetTarget(BaseEntity target);
    public void Launch(float targettingTime);
    public void Exclude(List<ulong> newmans);
    private void GuideMissile();
    private void OnDestroy();
}

public class TreasureChest : FacepunchBehaviour
{
    internal DangerousTreasures Instance;
    internal ulong userid;
    internal GameObject go;
    internal StorageContainer container;
    internal Vector3 containerPos;
    internal Vector3 lastFirePos;
    internal int npcMaxAmountMurderers;
    internal int npcMaxAmountScientists;
    internal int npcSpawnedAmount;
    internal int countdownTime;
    internal bool started;
    internal bool opened;
    internal bool firstEntered;
    internal bool markerCreated;
    internal bool killed;
    internal bool IsUnloading;
    internal bool requireAllNpcsDie;
    internal bool whenNpcsDie;
    internal float claimTime;
    internal float _radius;
    internal long _unlockTime;
    internal NetworkableId uid;
    private Dictionary<string, List<string>> npcKits;
    private Dictionary<ulong, float> fireticks;
    private List<FireBall> fireballs;
    private List<ulong> newmans;
    private List<ulong> traitors;
    private List<ulong> protects;
    private List<ulong> players;
    private List<TimedExplosive> missiles;
    private List<int> times;
    private List<SphereEntity> spheres;
    private List<Vector3> missilePositions;
    private List<Vector3> firePositions;
    public List<HumanoidNPC> npcs;
    private Timer destruct;
    private Timer unlock;
    private Timer countdown;
    private Timer announcement;
    private MapMarkerExplosion explosionMarker;
    private MapMarkerGenericRadius genericMarker;
    private VendingMachineMapMarker vendingMarker;
    private string FormatGridReference(Vector3 position);
    private void Message(BasePlayer player, string key, object[] args);
    private Configuration config { get; set; }
    public float Radius { get; set; }
    private void Free();
    private class NewmanTracker : FacepunchBehaviour
    {
         BasePlayer player;
         TreasureChest chest;
         DangerousTreasures Instance;
         Configuration config { get; set; }
        private void Message(BasePlayer player, string key, object[] args);
        private void Awake();
        public void Assign(DangerousTreasures instance, TreasureChest chest);
        private void Track();
        private void OnDestroy();
    }

    public void Kill(bool isUnloading);
    public bool HasRustMarker { get; set; }
    public void Awaken();
     void Awake();
    public void SpawnLoot(StorageContainer container, List<LootItem> treasure);
    private Dictionary<string, ulong> skinIds { get; set; }
    private bool IsBlacklistedSkin(ItemDefinition def, int num);
    public ulong GetItemSkin(ItemDefinition def, ulong defaultSkin, bool unique);
    public SkinInfo GetItemSkins(ItemDefinition def);
     void OnTriggerEnter(Collider col);
     void OnTriggerExit(Collider col);
    public void SpawnNpcs();
    private NavMeshHit _navHit;
    private Vector3 FindPointOnNavmesh(Vector3 target, float radius);
    private RaycastHit _hit;
    private bool IsAcceptableWaterDepth(Vector3 position);
    private bool TestInsideObject(Vector3 position);
    private bool TestInsideRock(Vector3 a);
    private bool IsRockFaceDownwards(Vector3 a);
    private bool IsRockFaceUpwards(Vector3 point);
    private bool IsRock(string name);
    private List<string> _prefabs;
    private static void CopySerializableFields(T src, T dst);
    private bool InstantiateEntity(Vector3 position, bool isMurderer, HumanoidBrain humanoidBrain, HumanoidNPC npc);
    private Vector3 RandomPosition(float radius);
    private List<Vector3> RandomWanderPositions(float radius);
    private Vector3 GetRandomPoint(float radius);
    private HumanoidNPC SpawnNpc(bool isMurderer);
    public class Loadout
    {
        public List<PlayerInventoryProperties.ItemAmountSkinned> belt;
        public List<PlayerInventoryProperties.ItemAmountSkinned> main;
        public List<PlayerInventoryProperties.ItemAmountSkinned> wear;
    }

    private PlayerInventoryProperties GetLoadout(HumanoidNPC npc, HumanoidBrain brain, bool isMurderer);
    private Loadout CreateLoadout(HumanoidNPC npc, HumanoidBrain brain, bool isMurderer);
    private void AddItemAmountSkinned(List<PlayerInventoryProperties.ItemAmountSkinned> source, List<string> shortnames);
    private void SetupNpc(HumanoidNPC npc, HumanoidBrain brain, bool isMurderer, List<Vector3> positions);
    private void GiveKit(HumanoidNPC npc, HumanoidBrain brain, bool isMurderer);
    private void UpdateItems(HumanoidNPC npc, HumanoidBrain brain, bool isMurderer);
    private bool ToggleNpcMinerHat(HumanoidNPC npc, bool state);
    public void EquipWeapon(HumanoidNPC npc, HumanoidBrain brain);
     void SetupNpcKits();
     bool IsKit(string kit);
    public void UpdateMarker();
    private void CreateGenericMarker();
    private void SafelyKill(HumanoidNPC npc);
    private void KillNpc();
    public void RemoveMapMarkers();
    private void OnDestroy();
    public void DestroyMe();
    public void LaunchMissile();
     void SpawnFire();
     void SpawnFire(Vector3 firePos);
    public void Destruct();
     void Unclaimed();
    public string GetUnlockTime(string userID);
    public void Unlock();
    public void SetUnlockTime(float time);
    public void TrySetOwner(HitInfo hitInfo);
    private void SafelyKill(BaseEntity e);
    public void DestroyLauncher();
    public void DestroySphere();
    public void DestroyFire();
}

private class NewmanTracker : FacepunchBehaviour
{
     BasePlayer player;
     TreasureChest chest;
     DangerousTreasures Instance;
     Configuration config { get; set; }
    private void Message(BasePlayer player, string key, object[] args);
    private void Awake();
    public void Assign(DangerousTreasures instance, TreasureChest chest);
    private void Track();
    private void OnDestroy();
}

public class Loadout
{
    public List<PlayerInventoryProperties.ItemAmountSkinned> belt;
    public List<PlayerInventoryProperties.ItemAmountSkinned> main;
    public List<PlayerInventoryProperties.ItemAmountSkinned> wear;
}

private class MonumentInfoEx
{
    public MonumentInfo monument;
    public Vector3 position;
    public float radius;
    public string name;
    public string prefab;
    public MonumentInfoEx();
    public MonumentInfoEx(MonumentInfo monument, Vector3 position, float radius, string name, string prefab);
    public bool IsInBounds(Vector3 target);
}

public class Notification
{
    public BasePlayer player;
    public string messageEx;
}

public class PluginSettings
{
    [JsonProperty(PropertyName = "Permission Name")]
    public string PermName { get; set; }
    [JsonProperty(PropertyName = "Event Chat Command")]
    public string EventChatCommand { get; set; }
    [JsonProperty(PropertyName = "Distance Chat Command")]
    public string DistanceChatCommand { get; set; }
    [JsonProperty(PropertyName = "Draw Location On Screen With Distance Command")]
    public bool AllowDrawText { get; set; }
    [JsonProperty(PropertyName = "Event Console Command")]
    public string EventConsoleCommand { get; set; }
    [JsonProperty(PropertyName = "Show X Z Coordinates")]
    public bool ShowXZ { get; set; }
    [JsonProperty(PropertyName = "Show Grid Coordinates")]
    public bool ShowGrid { get; set; }
    [JsonProperty(PropertyName = "Grids To Block Spawns At", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> BlockedGrids;
    [JsonProperty(PropertyName = "Block Spawns At Positions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ManagementSettingsLocations> BlockedPositions;
}

public class ManagementSettingsLocations
{
    [JsonProperty(PropertyName = "position")]
    [JsonConverter(typeof(UnityVector3Converter))]
    public Vector3 position;
    public float radius;
    public ManagementSettingsLocations();
    public ManagementSettingsLocations(Vector3 position, float radius);
}

private class UnityVector3Converter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}

public class CountdownSettings
{
    [JsonProperty(PropertyName = "Use Countdown Before Event Starts")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Time In Seconds", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<int> Times { get; set; }
}

public class EventSettings
{
    [JsonProperty(PropertyName = "Allow Player Bags To Be Lootable At Events")]
    public bool PlayersLootable;
    [JsonProperty(PropertyName = "Automated")]
    public bool Automated { get; set; }
    [JsonProperty(PropertyName = "Every Min Seconds")]
    public float IntervalMin { get; set; }
    [JsonProperty(PropertyName = "Every Max Seconds")]
    public float IntervalMax { get; set; }
    [JsonProperty(PropertyName = "Use Vending Map Marker")]
    public bool MarkerVending { get; set; }
    [JsonProperty(PropertyName = "Use Marker Manager Plugin")]
    public bool MarkerManager { get; set; }
    [JsonProperty(PropertyName = "Use Explosion Map Marker")]
    public bool MarkerExplosion { get; set; }
    [JsonProperty(PropertyName = "Marker Color")]
    public string MarkerColor { get; set; }
    [JsonProperty(PropertyName = "Marker Radius")]
    public float MarkerRadius { get; set; }
    [JsonProperty(PropertyName = "Marker Radius (Smaller Maps)")]
    public float MarkerRadiusSmall { get; set; }
    [JsonProperty(PropertyName = "Marker Event Name")]
    public string MarkerName { get; set; }
    [JsonProperty(PropertyName = "Max Manual Events")]
    public int Max { get; set; }
    [JsonProperty(PropertyName = "Always Spawn Max Manual Events")]
    public bool SpawnMax { get; set; }
    [JsonProperty(PropertyName = "Stagger Spawns Every X Seconds")]
    public float Stagger { get; set; }
    [JsonProperty(PropertyName = "Amount Of Items To Spawn")]
    public int TreasureAmount { get; set; }
    [JsonProperty(PropertyName = "Use Spheres")]
    public bool Spheres { get; set; }
    [JsonProperty(PropertyName = "Amount Of Spheres")]
    public int SphereAmount { get; set; }
    [JsonProperty(PropertyName = "Destroy Spheres When Event Starts")]
    public bool DestroySphereOnStart { get; set; }
    [JsonProperty(PropertyName = "Player Limit For Event")]
    public int PlayerLimit { get; set; }
    [JsonProperty(PropertyName = "Fire Aura Radius (Advanced Users Only)")]
    public float Radius { get; set; }
    [JsonProperty(PropertyName = "Auto Draw On New Event For Nearby Players")]
    public bool DrawTreasureIfNearby { get; set; }
    [JsonProperty(PropertyName = "Auto Draw Minimum Distance")]
    public float AutoDrawDistance { get; set; }
    [JsonProperty(PropertyName = "Grant DDRAW temporarily to players")]
    public bool GrantDraw { get; set; }
    [JsonProperty(PropertyName = "Grant Draw Time")]
    public float DrawTime { get; set; }
    [JsonProperty(PropertyName = "Time To Loot")]
    public float DestructTime { get; set; }
}

public class UIAdvancedAlertSettings
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Anchor Min")]
    public string AnchorMin { get; set; }
    [JsonProperty(PropertyName = "Anchor Max")]
    public string AnchorMax { get; set; }
    [JsonProperty(PropertyName = "Time Shown")]
    public float Time { get; set; }
}

public class EventMessageSettings
{
    [JsonProperty(PropertyName = "Advanced Alerts UI")]
    public UIAdvancedAlertSettings AA { get; set; }
    [JsonProperty(PropertyName = "Notify Plugin - Type (-1 = disabled)")]
    public int NotifyType { get; set; }
    [JsonProperty(PropertyName = "UI Popup Interval")]
    public float Interval { get; set; }
    [JsonProperty(PropertyName = "Show Noob Warning Message")]
    public bool NoobWarning { get; set; }
    [JsonProperty(PropertyName = "Show Barrage Message")]
    public bool Barrage { get; set; }
    [JsonProperty(PropertyName = "Show Despawn Message")]
    public bool Destruct { get; set; }
    [JsonProperty(PropertyName = "Show You Have Entered")]
    public bool Entered { get; set; }
    [JsonProperty(PropertyName = "Show First Player Entered")]
    public bool FirstEntered { get; set; }
    [JsonProperty(PropertyName = "Show First Player Opened")]
    public bool FirstOpened { get; set; }
    [JsonProperty(PropertyName = "Show Opened Message")]
    public bool Opened { get; set; }
    [JsonProperty(PropertyName = "Show Prefix")]
    public bool Prefix { get; set; }
    [JsonProperty(PropertyName = "Show Started Message")]
    public bool Started { get; set; }
    [JsonProperty(PropertyName = "Show Thief Message")]
    public bool Thief { get; set; }
    [JsonProperty(PropertyName = "Send Messages To Player")]
    public bool Message { get; set; }
}

public class FireballSettings
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Damage Per Second")]
    public float DamagePerSecond { get; set; }
    [JsonProperty(PropertyName = "Lifetime Min")]
    public float LifeTimeMin { get; set; }
    [JsonProperty(PropertyName = "Lifetime Max")]
    public float LifeTimeMax { get; set; }
    [JsonProperty(PropertyName = "Radius")]
    public float Radius { get; set; }
    [JsonProperty(PropertyName = "Tick Rate")]
    public float TickRate { get; set; }
    [JsonProperty(PropertyName = "Generation")]
    public float Generation { get; set; }
    [JsonProperty(PropertyName = "Water To Extinguish")]
    public int WaterToExtinguish { get; set; }
    [JsonProperty(PropertyName = "Spawn Every X Seconds")]
    public int SecondsBeforeTick { get; set; }
}

public class GUIAnnouncementSettings
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Text Color")]
    public string TextColor { get; set; }
    [JsonProperty(PropertyName = "Banner Tint Color")]
    public string TintColor { get; set; }
    [JsonProperty(PropertyName = "Maximum Distance")]
    public float Distance { get; set; }
}

public class MissileLauncherSettings
{
    [JsonProperty(PropertyName = "Acquire Time In Seconds")]
    public float TargettingTime { get; set; }
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Damage Per Missile")]
    public float Damage { get; set; }
    [JsonProperty(PropertyName = "Detection Distance")]
    public float Distance { get; set; }
    [JsonProperty(PropertyName = "Life Time In Seconds")]
    public float Lifetime { get; set; }
    [JsonProperty(PropertyName = "Ignore Flying Players")]
    public bool IgnoreFlying { get; set; }
    [JsonProperty(PropertyName = "Spawn Every X Seconds")]
    public float Frequency { get; set; }
    [JsonProperty(PropertyName = "Target Chest If No Player Target")]
    public bool TargetChest { get; set; }
}

public class MonumentSettings
{
    [JsonProperty(PropertyName = "Blacklisted Monuments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, bool> Blacklist { get; set; }
    [JsonProperty(PropertyName = "Auto Spawn At Monuments Only")]
    public bool Only { get; set; }
    [JsonProperty(PropertyName = "Chance To Spawn At Monuments Instead")]
    public float Chance { get; set; }
    [JsonProperty(PropertyName = "Allow Treasure Loot Underground")]
    public bool Underground { get; set; }
}

public class NewmanModeSettings
{
    [JsonProperty(PropertyName = "Protect Nakeds From Fire Aura")]
    public bool Aura { get; set; }
    [JsonProperty(PropertyName = "Protect Nakeds From Other Harm")]
    public bool Harm { get; set; }
}

public class NpcKitSettings
{
    [JsonProperty(PropertyName = "Helm", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Helm;
    [JsonProperty(PropertyName = "Torso", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Torso;
    [JsonProperty(PropertyName = "Pants", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Pants;
    [JsonProperty(PropertyName = "Gloves", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Gloves;
    [JsonProperty(PropertyName = "Boots", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Boots;
    [JsonProperty(PropertyName = "Shirt", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Shirt;
    [JsonProperty(PropertyName = "Kilts", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Kilts;
    [JsonProperty(PropertyName = "Weapon", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Weapon;
}

public class NpcLootSettings
{
    [JsonProperty(PropertyName = "Prefab ID List", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> IDs { get; set; }
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Disable All Prefab Loot Spawns")]
    public bool None { get; set; }
    public uint GetRandom();
}

public class NpcSettingsAccuracy
{
    [JsonProperty(PropertyName = "AK47")]
    public double AK47 { get; set; }
    [JsonProperty(PropertyName = "AK47 ICE")]
    public double AK47ICE { get; set; }
    [JsonProperty(PropertyName = "Bolt Rifle")]
    public double BOLT_RIFLE { get; set; }
    [JsonProperty(PropertyName = "Compound Bow")]
    public double COMPOUND_BOW { get; set; }
    [JsonProperty(PropertyName = "Crossbow")]
    public double CROSSBOW { get; set; }
    [JsonProperty(PropertyName = "Double Barrel Shotgun")]
    public double DOUBLE_SHOTGUN { get; set; }
    [JsonProperty(PropertyName = "Eoka")]
    public double EOKA { get; set; }
    [JsonProperty(PropertyName = "Glock")]
    public double GLOCK { get; set; }
    [JsonProperty(PropertyName = "HMLMG")]
    public double HMLMG { get; set; }
    [JsonProperty(PropertyName = "L96")]
    public double L96 { get; set; }
    [JsonProperty(PropertyName = "LR300")]
    public double LR300 { get; set; }
    [JsonProperty(PropertyName = "M249")]
    public double M249 { get; set; }
    [JsonProperty(PropertyName = "M39")]
    public double M39 { get; set; }
    [JsonProperty(PropertyName = "M92")]
    public double M92 { get; set; }
    [JsonProperty(PropertyName = "MP5")]
    public double MP5 { get; set; }
    [JsonProperty(PropertyName = "Nailgun")]
    public double NAILGUN { get; set; }
    [JsonProperty(PropertyName = "Pump Shotgun")]
    public double PUMP_SHOTGUN { get; set; }
    [JsonProperty(PropertyName = "Python")]
    public double PYTHON { get; set; }
    [JsonProperty(PropertyName = "Revolver")]
    public double REVOLVER { get; set; }
    [JsonProperty(PropertyName = "Semi Auto Pistol")]
    public double SEMI_AUTO_PISTOL { get; set; }
    [JsonProperty(PropertyName = "Semi Auto Rifle")]
    public double SEMI_AUTO_RIFLE { get; set; }
    [JsonProperty(PropertyName = "Spas12")]
    public double SPAS12 { get; set; }
    [JsonProperty(PropertyName = "Speargun")]
    public double SPEARGUN { get; set; }
    [JsonProperty(PropertyName = "SMG")]
    public double SMG { get; set; }
    [JsonProperty(PropertyName = "Snowball Gun")]
    public double SNOWBALL_GUN { get; set; }
    [JsonProperty(PropertyName = "Thompson")]
    public double THOMPSON { get; set; }
    [JsonProperty(PropertyName = "Waterpipe Shotgun")]
    public double WATERPIPE_SHOTGUN { get; set; }
    public NpcSettingsAccuracy(double accuracy);
    public double Get(HumanoidBrain brain);
}

public class NpcSettingsMurderer
{
    [JsonProperty(PropertyName = "Random Names", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> RandomNames { get; set; }
    [JsonProperty(PropertyName = "Items)")]
    public NpcKitSettings Items { get; set; }
    [JsonProperty(PropertyName = "Kits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Kits { get; set; }
    [JsonProperty(PropertyName = "Spawn Alternate Loot")]
    public NpcLootSettings Alternate { get; set; }
    [JsonProperty(PropertyName = "Weapon Accuracy (0 - 100)")]
    public NpcSettingsAccuracy Accuracy { get; set; }
    [JsonProperty(PropertyName = "Aggression Range")]
    public float AggressionRange { get; set; }
    [JsonProperty(PropertyName = "Despawn Inventory On Death")]
    public bool DespawnInventory { get; set; }
    [JsonProperty(PropertyName = "Corpse Despawn Time When Despawn Inventory On Death")]
    public float DespawnInventoryTime { get; set; }
    [JsonProperty(PropertyName = "Corpse Despawn Time Otherwise")]
    public float CorpseDespawnTime { get; set; }
    [JsonProperty(PropertyName = "Die Instantly From Headshots")]
    public bool Headshot { get; set; }
    [JsonProperty(PropertyName = "Amount To Spawn (min)")]
    public int SpawnMinAmount { get; set; }
    [JsonProperty(PropertyName = "Amount To Spawn (max)")]
    public int SpawnAmount { get; set; }
    [JsonProperty(PropertyName = "Health")]
    public float Health { get; set; }
}

public class NpcSettingsScientist
{
    [JsonProperty(PropertyName = "Random Names", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> RandomNames { get; set; }
    [JsonProperty(PropertyName = "Items")]
    public NpcKitSettings Items { get; set; }
    [JsonProperty(PropertyName = "Kits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Kits { get; set; }
    [JsonProperty(PropertyName = "Spawn Alternate Loot")]
    public NpcLootSettings Alternate { get; set; }
    [JsonProperty(PropertyName = "Weapon Accuracy (0 - 100)")]
    public NpcSettingsAccuracy Accuracy { get; set; }
    [JsonProperty(PropertyName = "Aggression Range")]
    public float AggressionRange { get; set; }
    [JsonProperty(PropertyName = "Despawn Inventory On Death")]
    public bool DespawnInventory { get; set; }
    [JsonProperty(PropertyName = "Corpse Despawn Time When Despawn Inventory On Death")]
    public float DespawnInventoryTime { get; set; }
    [JsonProperty(PropertyName = "Corpse Despawn Time Otherwise")]
    public float CorpseDespawnTime { get; set; }
    [JsonProperty(PropertyName = "Die Instantly From Headshots")]
    public bool Headshot { get; set; }
    [JsonProperty(PropertyName = "Amount To Spawn (min)")]
    public int SpawnMinAmount { get; set; }
    [JsonProperty(PropertyName = "Amount To Spawn (max)")]
    public int SpawnAmount { get; set; }
    [JsonProperty(PropertyName = "Health (100 min, 5000 max)")]
    public float Health { get; set; }
}

public class NpcSettings
{
    [JsonProperty(PropertyName = "Blacklisted Monuments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, bool> BlacklistedMonuments { get; set; }
    [JsonProperty(PropertyName = "Murderers")]
    public NpcSettingsMurderer Murderers { get; set; }
    [JsonProperty(PropertyName = "Scientists")]
    public NpcSettingsScientist Scientists { get; set; }
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Allow Npcs To Leave Dome When Attacking")]
    public bool CanLeave { get; set; }
    [JsonProperty(PropertyName = "Allow Npcs To Target Other Npcs")]
    public bool TargetNpcs { get; set; }
    [JsonProperty(PropertyName = "Block Damage From Players Beyond X Distance (0 = disabled)")]
    public float Range { get; set; }
    [JsonProperty(PropertyName = "Block Npc Kits Plugin")]
    public bool BlockNpcKits { get; set; }
    [JsonProperty(PropertyName = "Kill Underwater Npcs")]
    public bool KillUnderwater { get; set; }
}

public class PasteOption
{
    [JsonProperty(PropertyName = "Option")]
    public string Key { get; set; }
    [JsonProperty(PropertyName = "Value")]
    public string Value { get; set; }
}

public class RankedLadderSettings
{
    [JsonProperty(PropertyName = "Award Top X Players On Wipe")]
    public int Amount { get; set; }
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Group Name")]
    public string Group { get; set; }
    [JsonProperty(PropertyName = "Permission Name")]
    public string Permission { get; set; }
}

public class RewardSettings
{
    [JsonProperty(PropertyName = "Economics Money")]
    public double Money { get; set; }
    [JsonProperty(PropertyName = "ServerRewards Points")]
    public double Points { get; set; }
    [JsonProperty(PropertyName = "Use Economics")]
    public bool Economics { get; set; }
    [JsonProperty(PropertyName = "Use ServerRewards")]
    public bool ServerRewards { get; set; }
}

public class RocketOpenerSettings
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Rockets")]
    public int Amount { get; set; }
    [JsonProperty(PropertyName = "Speed")]
    public float Speed { get; set; }
    [JsonProperty(PropertyName = "Use Fire Rockets")]
    public bool FireRockets { get; set; }
}

public class SkinSettings
{
    [JsonProperty(PropertyName = "Custom Skins", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ulong> Custom { get; set; }
    [JsonProperty(PropertyName = "Use Random Skin")]
    public bool RandomSkins { get; set; }
    [JsonProperty(PropertyName = "Preset Skin")]
    public ulong PresetSkin { get; set; }
    [JsonProperty(PropertyName = "Include Workshop Skins")]
    public bool RandomWorkshopSkins { get; set; }
    [JsonProperty(PropertyName = "Randomize Npc Item Skins")]
    public bool Npcs { get; set; }
    [JsonProperty(PropertyName = "Use Identical Skins For All Npcs")]
    public bool UniqueNpcs { get; set; }
}

public class LootItem
{
    public string shortname { get; set; }
    public string name { get; set; }
    public int amount { get; set; }
    public ulong skin { get; set; }
    public int amountMin { get; set; }
    public float condition { get; set; }
    public float probability { get; set; }
    [JsonProperty(PropertyName = "Skins", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ulong> skins { get; set; }
}

public class TreasureSettings
{
    [JsonProperty(PropertyName = "Loot", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<LootItem> Loot { get; set; }
    [JsonProperty(PropertyName = "Minimum Percent Loss")]
    public decimal PercentLoss { get; set; }
    [JsonProperty(PropertyName = "Percent Increase When Using Day Of Week Loot")]
    public bool Increased { get; set; }
    [JsonProperty(PropertyName = "Use Random Skins")]
    public bool RandomSkins { get; set; }
    [JsonProperty(PropertyName = "Include Workshop Skins")]
    public bool RandomWorkshopSkins { get; set; }
    [JsonProperty(PropertyName = "Day Of Week Loot Monday", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<LootItem> DOWL_Monday { get; set; }
    [JsonProperty(PropertyName = "Day Of Week Loot Tuesday", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<LootItem> DOWL_Tuesday { get; set; }
    [JsonProperty(PropertyName = "Day Of Week Loot Wednesday", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<LootItem> DOWL_Wednesday { get; set; }
    [JsonProperty(PropertyName = "Day Of Week Loot Thursday", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<LootItem> DOWL_Thursday { get; set; }
    [JsonProperty(PropertyName = "Day Of Week Loot Friday", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<LootItem> DOWL_Friday { get; set; }
    [JsonProperty(PropertyName = "Day Of Week Loot Saturday", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<LootItem> DOWL_Saturday { get; set; }
    [JsonProperty(PropertyName = "Day Of Week Loot Sunday", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<LootItem> DOWL_Sunday { get; set; }
    [JsonProperty(PropertyName = "Use Day Of Week Loot")]
    public bool UseDOWL { get; set; }
    [JsonProperty(PropertyName = "Percent Increase On Monday")]
    public decimal PercentIncreaseOnMonday { get; set; }
    [JsonProperty(PropertyName = "Percent Increase On Tuesday")]
    public decimal PercentIncreaseOnTuesday { get; set; }
    [JsonProperty(PropertyName = "Percent Increase On Wednesday")]
    public decimal PercentIncreaseOnWednesday { get; set; }
    [JsonProperty(PropertyName = "Percent Increase On Thursday")]
    public decimal PercentIncreaseOnThursday { get; set; }
    [JsonProperty(PropertyName = "Percent Increase On Friday")]
    public decimal PercentIncreaseOnFriday { get; set; }
    [JsonProperty(PropertyName = "Percent Increase On Saturday")]
    public decimal PercentIncreaseOnSaturday { get; set; }
    [JsonProperty(PropertyName = "Percent Increase On Sunday")]
    public decimal PercentIncreaseOnSunday { get; set; }
}

public class TruePVESettings
{
    [JsonProperty(PropertyName = "Allow Building Damage At Events")]
    public bool AllowBuildingDamageAtEvents { get; set; }
    [JsonProperty(PropertyName = "Allow PVP At Events")]
    public bool AllowPVPAtEvents { get; set; }
    [JsonProperty(PropertyName = "Allow PVP Server-Wide During Events")]
    public bool ServerWidePVP { get; set; }
}

public class UnlockSettings
{
    [JsonProperty(PropertyName = "Min Seconds")]
    public float MinTime { get; set; }
    [JsonProperty(PropertyName = "Max Seconds")]
    public float MaxTime { get; set; }
    [JsonProperty(PropertyName = "Unlock When Npcs Die")]
    public bool WhenNpcsDie { get; set; }
    [JsonProperty(PropertyName = "Require All Npcs Die Before Unlocking")]
    public bool RequireAllNpcsDie { get; set; }
    [JsonProperty(PropertyName = "Lock Event To Player On Npc Death")]
    public bool LockToPlayerOnNpcDeath { get; set; }
    [JsonProperty(PropertyName = "Lock Event To Player On First Entered")]
    public bool LockToPlayerFirstEntered { get; set; }
}

public class UnlootedAnnouncementSettings
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Notify Every X Minutes (Minimum 1)")]
    public float Interval { get; set; }
}

public class Configuration
{
    [JsonProperty(PropertyName = "Settings")]
    public PluginSettings Settings;
    [JsonProperty(PropertyName = "Countdown")]
    public CountdownSettings Countdown;
    [JsonProperty(PropertyName = "Events")]
    public EventSettings Event;
    [JsonProperty(PropertyName = "Event Messages")]
    public EventMessageSettings EventMessages;
    [JsonProperty(PropertyName = "Fireballs")]
    public FireballSettings Fireballs;
    [JsonProperty(PropertyName = "GUIAnnouncements")]
    public GUIAnnouncementSettings GUIAnnouncement;
    [JsonProperty(PropertyName = "Monuments")]
    public MonumentSettings Monuments;
    [JsonProperty(PropertyName = "Newman Mode")]
    public NewmanModeSettings NewmanMode;
    [JsonProperty(PropertyName = "NPCs")]
    public NpcSettings NPC;
    [JsonProperty(PropertyName = "Missile Launcher")]
    public MissileLauncherSettings MissileLauncher;
    [JsonProperty(PropertyName = "Ranked Ladder")]
    public RankedLadderSettings RankedLadder;
    [JsonProperty(PropertyName = "Rewards")]
    public RewardSettings Rewards;
    [JsonProperty(PropertyName = "Rocket Opener")]
    public RocketOpenerSettings Rocket;
    [JsonProperty(PropertyName = "Skins")]
    public SkinSettings Skins;
    [JsonProperty(PropertyName = "Treasure")]
    public TreasureSettings Treasure;
    [JsonProperty(PropertyName = "TruePVE")]
    public TruePVESettings TruePVE;
    [JsonProperty(PropertyName = "Unlock Time")]
    public UnlockSettings Unlock;
    [JsonProperty(PropertyName = "Unlooted Announcements")]
    public UnlootedAnnouncementSettings UnlootedAnnouncements;
}

Oxide.Plugins.DangerousTreasuresExtensionMethods
public static class ExtensionMethods
{
    internal static Core.Libraries.Permission p;
    public static bool All(IEnumerable<T> a, Func<T, bool> b);
    public static T ElementAt(IEnumerable<T> a, int b);
    public static bool Exists(IEnumerable<T> a, Func<T, bool> b);
    public static T FirstOrDefault(IEnumerable<T> a, Func<T, bool> b);
    public static IEnumerable<V> Select(IEnumerable<T> a, Func<T, V> b);
    public static string[] Skip(string[] a, int b);
    public static List<T> Take(IList<T> a, int b);
    public static Dictionary<T, V> ToDictionary(IEnumerable<S> a, Func<S, T> b, Func<S, V> c);
    public static List<T> ToList(IEnumerable<T> a);
    public static List<T> Where(IEnumerable<T> a, Func<T, bool> b);
    public static List<T> OfType(IEnumerable<BaseNetworkable> a);
    public static int Sum(IEnumerable<T> a, Func<T, int> b);
    public static bool UserHasGroup(string a, string b);
    public static bool UserHasGroup(IPlayer a, string b);
    public static bool IsReallyConnected(BasePlayer a);
    public static bool IsKilled(BaseNetworkable a);
    public static bool IsNull(T a);
    public static bool IsNull(BasePlayer a);
    public static bool IsReallyValid(BaseNetworkable a);
    public static void SafelyKill(BaseNetworkable a);
    public static bool CanCall(Plugin o);
    public static bool IsHuman(BasePlayer a);
    public static float Distance(Vector3 a, Vector3 b);
    public static void ResetToPool(Dictionary<K, V> obj);
    public static void ResetToPool(List<T> obj);
}


```

---

## DataLogging by Rustoholics - Base class to record stats from the server for allowing other plugins to record data

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;
using JetBrains.Annotations;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Data Logging", "Rustoholics", "0.1.1")]
[Description("Record stats from the server")]
public class DataLogging : CovalencePlugin
{
    private Dictionary<string, CacheItem> _cache;
    public object GetCache(string key);
    public void AddCache(string key, object value, int ttl);
     class CacheItem
    {
        public DateTime Expires;
        public object Value;
        public bool IsExpired();
    }

    private void OnServerInitialized();
    protected void SaveConfig(T obj);
    private class Configuration
    {
        public bool Debug;
    }

    public void SetupConfig(T config);
    private Configuration _config;
    public class DataManager
    {
        private Dictionary<string, DataList<T>> _data;
        private List<string> _needsWriting;
        public string _fileName;
        public string _filePattern;
        public DataManager(string filename, string filepattern);
        public List<string> GetKeys();
        public void Load();
        private string GetFileExt();
        public List<T> GetData(string uid);
        public Dictionary<string, DataList<T>> GetAllData();
        public void NeedsWriting(string uid);
        public void AddData(string uid, T obj);
        public void Save();
        public T GetDataLast(string playerId);
    }

    public class DataList
    {
        private string _uid;
        private List<T> _list;
        private string _fileExt;
        public DataList(string userId, string filename);
        private string Filename();
        public void Load();
        public void Save();
        public List<T> GetData();
        public void AddData(T obj);
        public T GetDataLast();
    }

    public void Debug(string txt);
    [CanBeNull]
    public IPlayer GetCommandPlayer(IPlayer iplayer, string[] args);
}

 class CacheItem
{
    public DateTime Expires;
    public object Value;
    public bool IsExpired();
}

private class Configuration
{
    public bool Debug;
}

public class DataManager
{
    private Dictionary<string, DataList<T>> _data;
    private List<string> _needsWriting;
    public string _fileName;
    public string _filePattern;
    public DataManager(string filename, string filepattern);
    public List<string> GetKeys();
    public void Load();
    private string GetFileExt();
    public List<T> GetData(string uid);
    public Dictionary<string, DataList<T>> GetAllData();
    public void NeedsWriting(string uid);
    public void AddData(string uid, T obj);
    public void Save();
    public T GetDataLast(string playerId);
}

public class DataList
{
    private string _uid;
    private List<T> _list;
    private string _fileExt;
    public DataList(string userId, string filename);
    private string Filename();
    public void Load();
    public void Save();
    public List<T> GetData();
    public void AddData(T obj);
    public T GetDataLast();
}


```

---

## DataLoggingActivity by Rustoholics - Logs player connection time and active time

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json.Linq;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;

Oxide.Plugins
[Info("Data Logging: Activity", "Rustoholics", "0.1.0")]
[Description("Log when some is connected/disconnect and AFK")]
public class DataLoggingActivity : DataLogging
{
    public class Activity
    {
        public Types Type;
        public DateTime Date;
        public string Notes;
    }

    public class ActivityData
    {
        public int PlayTime;
        public int AfkTime;
        public int ConnectedTime;
    }

    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
    private class Configuration
    {
        public bool Debug;
    }

    private Configuration config;
     DataManager<Activity> _data;
    private Dictionary<string, Vector3> _positions;
    private void OnServerInitialized();
    private void Unload();
    private void OnServerSave();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
    public int TimeConnected(string playerId, DateTime startTime);
    public int TimeActive(string playerId, DateTime startTime);
    public Dictionary<string, ActivityData> GetAllActivity(DateTime startDate);
    [Command("datalogging.activity")]
    private void ActivityCommand(IPlayer iplayer, string command, string[] args);
    [Command("datalogging.activetime")]
    private void TimeActiveCommand(IPlayer iplayer, string command, string[] args);
    [Command("datalogging.allactivity")]
    private void AllActivityCommand(IPlayer iplayer, string command, string[] args);
    private bool IsAfk(BasePlayer player);
    private JObject API_GetAllActivity(DateTime startDate);
    private int API_GetActiveTime(string playerId);
    private int API_GetConnectedTime(string playerId);
}

public class Activity
{
    public Types Type;
    public DateTime Date;
    public string Notes;
}

public class ActivityData
{
    public int PlayTime;
    public int AfkTime;
    public int ConnectedTime;
}

private class Configuration
{
    public bool Debug;
}


```

---

## DataLoggingDeaths by Rustoholics - Log all player kills and deaths

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using JetBrains.Annotations;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Data Logging: Kills / Deaths", "Rustoholics", "0.1.3")]
[Description("Log all kills and deaths")]
public class DataLoggingDeaths : DataLogging
{
    public class DeathObject
    {
        public bool Naked;
        public string KillerType;
        public ulong KillerId;
        public string VictimType;
        public ulong VictimId;
        public int WeaponId;
        public DateTime Date;
        [JsonIgnore]
        public bool Animal { get; set; }
        [JsonIgnore]
        public bool Npc { get; set; }
        [CanBeNull]
        public DeathObject SetHitInfo(HitInfo hitinfo, bool naked);
        private bool IsAnimal();
        private bool IsNpc();
        public string WeaponName();
    }

    public class PvpData
    {
        public KillData Kills;
        public KillData Deaths;
        public int NakedKills;
        public int KilLStreak;
        public class KillData
        {
            public int Player;
            public int Animal;
            public int Npc;
        }

    }

     DataManager<DeathObject> _deathData;
     DataManager<DeathObject> _killData;
    private Dictionary<string, bool> _hadWeapon;
    private void OnServerInitialized();
    private void Unload();
    private void OnServerSave();
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
     void OnEntityDeath(BasePlayer entity, HitInfo info);
    private int GetKillStreak(string playerId, DateTime startDate, DateTime endDate);
    private int GetTotalKills(string playerId, string victimType);
    private int GetTotalDeaths(string playerId, string killerType, DateTime startDate, DateTime endDate);
    private double PercentageNakedKills(string playerId, int lastNKills, int secondsAgo);
    public Dictionary<string, PvpData> AllPVPData(DateTime startDate, DateTime endDate);
    [Command("datalogging.kills")]
    private void KillsCommand(IPlayer iplayer, string command, string[] args);
    [Command("datalogging.deaths")]
    private void DeathsCommand(IPlayer iplayer, string command, string[] args);
    [Command("datalogging.pvp")]
    private void PVPCommand(IPlayer iplayer, string command, string[] args);
    private bool IsNaked(BasePlayer bp);
    private bool HasWeapon(BasePlayer player);
    private bool isWeapon(Item item);
    private void SetHasWeapon(string userid, bool weapon);
    private JObject API_AllPVPData(DateTime startDate, DateTime endDate);
    private int API_TotalKills(string playerId);
    private int API_GetKillStreak(string playerId);
    private double API_PercentageNakedKills(string playerid);
    private bool API_Helper_IsNaked(BasePlayer player);
    private JObject API_GetPlayerKills(string playerId);
    private JObject API_GetPlayerDeaths(string playerId);
}

public class DeathObject
{
    public bool Naked;
    public string KillerType;
    public ulong KillerId;
    public string VictimType;
    public ulong VictimId;
    public int WeaponId;
    public DateTime Date;
    [JsonIgnore]
    public bool Animal { get; set; }
    [JsonIgnore]
    public bool Npc { get; set; }
    [CanBeNull]
    public DeathObject SetHitInfo(HitInfo hitinfo, bool naked);
    private bool IsAnimal();
    private bool IsNpc();
    public string WeaponName();
}

public class PvpData
{
    public KillData Kills;
    public KillData Deaths;
    public int NakedKills;
    public int KilLStreak;
    public class KillData
    {
        public int Player;
        public int Animal;
        public int Npc;
    }

}

public class KillData
{
    public int Player;
    public int Animal;
    public int Npc;
}


```

---

## DataLoggingWipes by Rustoholics - Keep a history of all wipe dates

```csharp
using System;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using Oxide.Core.Extensions;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Logging;
using Oxide.Game.Rust;

Oxide.Plugins
[Info("Data Logging: Wipes", "Rustoholics", "0.2.0")]
[Description("Log every wipe")]
public class DataLoggingWipes : DataLogging
{
    public class Wipe
    {
        public DateTime Date;
        public int Seed;
        public string SaveFile;
        public string MapFile;
        public int Version;
        public string OxideVersion;
    }

    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
     DataList<Wipe> _data;
    private void OnServerInitialized();
    [Command("datalogging.lastwipe")]
    private void LastWipeCommand(IPlayer iplayer, string command, string[] args);
    private DateTime API_LastWipe();
}

public class Wipe
{
    public DateTime Date;
    public int Seed;
    public string SaveFile;
    public string MapFile;
    public int Version;
    public string OxideVersion;
}


```

---

## DayNightGather by klauz24 - Sets different gather rates for day and night.

```csharp
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System.Collections.Generic;

Oxide.Plugins
[Info("Day Night Gather", "klauz24", "1.1.1"), Description("Sets different gather rates for day and night.")]
internal class DayNightGather : RustPlugin
{
    [PluginReference]
    readonly Plugin TimeOfDay;
    private bool _isDay;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Scale dispensers")]
        public bool ScaleDispensers;
        [JsonProperty(PropertyName = "Chat announcements")]
        public bool ChatAnnouncements;
        [JsonProperty(PropertyName = "Time check interval (Only used if TimeOfDay plugin is not installed)")]
        public int TimeCheckInterval;
        [JsonProperty(PropertyName = "Day")]
        public Values Day;
        [JsonProperty(PropertyName = "Night")]
        public Values Night;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    private class Values
    {
        public int Dispenser;
        public int Pickup;
        public int Quarry;
        public int Excavator;
        public int Survey;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private void OnTimeSunrise();
    private void OnTimeSunset();
    private void OnTimeChange(int dispenser, int pickup, int quarry, int excavator, int survey, bool boolean);
    private void SetRates(bool boolean);
    private void IsDayOrNight();
    private bool IsDay { get; set; }
}

private class Configuration
{
    [JsonProperty(PropertyName = "Scale dispensers")]
    public bool ScaleDispensers;
    [JsonProperty(PropertyName = "Chat announcements")]
    public bool ChatAnnouncements;
    [JsonProperty(PropertyName = "Time check interval (Only used if TimeOfDay plugin is not installed)")]
    public int TimeCheckInterval;
    [JsonProperty(PropertyName = "Day")]
    public Values Day;
    [JsonProperty(PropertyName = "Night")]
    public Values Night;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private class Values
{
    public int Dispenser;
    public int Pickup;
    public int Quarry;
    public int Excavator;
    public int Survey;
}


```

---

## DCProtect by FireStorm78 - Prevents looting of players for a specified time after disconnecting or server start

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("DCProtect", "FireStorm78", "1.0.6")]
[Description("Prevents players from looting other players that have disconnected for a specified time.")]
 class DCProtect : RustPlugin
{
     List<string> PlayerList;
     void Init();
     int DC_DelayInSeconds;
     int Start_DelayInSeconds;
     bool PreventLooting;
     bool PreventDamage;
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
     T GetConfig(string name, T defaultValue);
     string Lang(string key, string id, object[] args);
    private bool IsAdmin(BasePlayer player);
    [ChatCommand("DCP")]
    private void cmdDCP(BasePlayer player, string command, string[] args);
    private void cmdDCP_Add(BasePlayer player, string command, string[] args);
    private void cmdDCP_Remove(BasePlayer player, string command, string[] args);
    private void cmdDCP_List(BasePlayer player, string command, string[] args);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     bool CanLootPlayer(BasePlayer target, BasePlayer looter);
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
}


```

---

## DeathChat by  - Provides ability to customize prefix for someone who has just died

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Death Chat", "Tricky", "1.1.2")]
[Description("Provides ability to customize prefix for someone who has just died")]
public class DeathChat : RustPlugin
{
    [PluginReference]
    private Plugin BetterChat;
     Configuration config;
     class Configuration
    {
        [JsonProperty(PropertyName = "Track Player Kills Only")]
        public bool PlayerKills;
        [JsonProperty(PropertyName = "Time Until Prefix will show (seconds)")]
        public int Time;
        [JsonProperty(PropertyName = "Prefix")]
        public string Prefix;
        [JsonProperty(PropertyName = "Prefix Color")]
        public string PrefixColor;
        [JsonProperty(PropertyName = "Prefix Size")]
        public int PrefixSize;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     List<string> _players;
    private void OnServerInitialized();
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
    private object OnPlayerChat(BasePlayer player, string message);
    private object OnBetterChat(Dictionary<string, object> data);
}

 class Configuration
{
    [JsonProperty(PropertyName = "Track Player Kills Only")]
    public bool PlayerKills;
    [JsonProperty(PropertyName = "Time Until Prefix will show (seconds)")]
    public int Time;
    [JsonProperty(PropertyName = "Prefix")]
    public string Prefix;
    [JsonProperty(PropertyName = "Prefix Color")]
    public string PrefixColor;
    [JsonProperty(PropertyName = "Prefix Size")]
    public int PrefixSize;
}


```

---

## DeathHistory by MadKingCraig - Get the locations of previous deaths in the form of Grid or Coordinates.

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Rust;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using UnityEngine;

Oxide.Plugins
[Info("Death History", "MadKingCraig", "1.2.1")]
[Description("Get the locations of previous deaths.")]
 class DeathHistory : RustPlugin
{
    private const string CanUsePermission;
    private const string AdminPermission;
    private PluginConfiguration _configuration;
    private DynamicConfigFile _data;
     DeathHistoryData dhData;
    private Dictionary<string, List<List<float>>> _deaths;
    private float _worldSize;
    private bool _newSaveDetected;
    [Command("deaths")]
    private void CommandDeaths(IPlayer user, string command, string[] args);
    private void Init();
    private void Loaded();
    private void OnServerInitialized();
    protected override void LoadDefaultMessages();
    private void OnNewSave(string filename);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    protected override void LoadDefaultConfig();
    private sealed class PluginConfiguration
    {
        [JsonProperty(PropertyName = "Number of Deaths to Keep")]
        public int MaxNumberOfDeaths;
        [JsonProperty(PropertyName = "Oldest Death First (false = Most Recent Death First)")]
        public bool OldestFirst;
        [JsonProperty(PropertyName = "Display Grid (false = Coordinates)")]
        public bool DisplayGrid;
    }

    private string CalculateGridPosition(Vector3 position);
    private List<string> GetGrids(List<Vector3> deathLocations);
    private List<string> GetCoordinates(List<Vector3> deathLocations);
    private List<string> SendCorpseLocations(BasePlayer player);
    private void SaveData();
    private void LoadData();
    private void NewData();
    public class DeathHistoryData
    {
        public Dictionary<string, List<List<float>>> Deaths;
    }

}

private sealed class PluginConfiguration
{
    [JsonProperty(PropertyName = "Number of Deaths to Keep")]
    public int MaxNumberOfDeaths;
    [JsonProperty(PropertyName = "Oldest Death First (false = Most Recent Death First)")]
    public bool OldestFirst;
    [JsonProperty(PropertyName = "Display Grid (false = Coordinates)")]
    public bool DisplayGrid;
}

public class DeathHistoryData
{
    public Dictionary<string, List<List<float>>> Deaths;
}


```

---

## DeathKick by MrBlue - Kicks players for a specified amount of time upon death

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Death Kick", "Wulf", "1.0.1")]
[Description("Kicks players for a specified amount of time upon death")]
 class DeathKick : CovalencePlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty("Death by animals")]
        public bool DeathByAnimals;
        [JsonProperty("Death by autoturrets")]
        public bool DeathByAutoturrets;
        [JsonProperty("Death by barricades")]
        public bool DeathByBarricades;
        [JsonProperty("Death by beartraps")]
        public bool DeathByBeartraps;
        [JsonProperty("Death by fall")]
        public bool DeathByFall;
        [JsonProperty("Death by floorspikes")]
        public bool DeathByFloorspikes;
        [JsonProperty("Death by helicopters")]
        public bool DeathByHelicopters;
        [JsonProperty("Death by landmines")]
        public bool DeathByLandmines;
        [JsonProperty("Death by players")]
        public bool DeathByPlayers;
        [JsonProperty("Death by suicide")]
        public bool DeathBySuicide;
        [JsonProperty("Deaths a player is limited to")]
        public int DeathLimit;
        [JsonProperty("Time before reconnection (minutes)")]
        public int KickCooldown;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private readonly Dictionary<string, double> deadPlayers;
    private readonly Dictionary<string, int> deathCounts;
    private readonly List<Timer> cooldowns;
    private const string permExempt;
    private void Init();
    private void Unload();
    private void OnEntityDeath(BasePlayer player, HitInfo hitInfo);
    private void ProcessDeath(IPlayer player, HitInfo hitInfo);
    private bool GetDeathType(HitInfo hitInfo);
    private object CanUserLogin(string playerName, string playerId);
    private void ClearData();
    private static double GetCurrentTime();
    private string GetLang(string langKey, string playerId, object[] args);
    private void Message(IPlayer player, string textOrLang, object[] args);
}

public class Configuration
{
    [JsonProperty("Death by animals")]
    public bool DeathByAnimals;
    [JsonProperty("Death by autoturrets")]
    public bool DeathByAutoturrets;
    [JsonProperty("Death by barricades")]
    public bool DeathByBarricades;
    [JsonProperty("Death by beartraps")]
    public bool DeathByBeartraps;
    [JsonProperty("Death by fall")]
    public bool DeathByFall;
    [JsonProperty("Death by floorspikes")]
    public bool DeathByFloorspikes;
    [JsonProperty("Death by helicopters")]
    public bool DeathByHelicopters;
    [JsonProperty("Death by landmines")]
    public bool DeathByLandmines;
    [JsonProperty("Death by players")]
    public bool DeathByPlayers;
    [JsonProperty("Death by suicide")]
    public bool DeathBySuicide;
    [JsonProperty("Deaths a player is limited to")]
    public int DeathLimit;
    [JsonProperty("Time before reconnection (minutes)")]
    public int KickCooldown;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## Deathmatch by k1lly0u - Arena deathmatch event for Event Manager

```csharp
using Newtonsoft.Json;
using Oxide.Plugins.EventManagerEx;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Deathmatch", "k1lly0u", "3.0.1"), Description("Deathmatch event mode for EventManager")]
 class Deathmatch : RustPlugin, IEventPlugin
{
    private void OnServerInitialized();
    protected override void LoadDefaultMessages();
    private void Unload();
    public bool InitializeEvent(EventManager.EventConfig config);
    public bool CanUseClassSelector { get; set; }
    public bool RequireTimeLimit { get; set; }
    public bool RequireScoreLimit { get; set; }
    public bool UseScoreLimit { get; set; }
    public bool UseTimeLimit { get; set; }
    public bool IsTeamEvent { get; set; }
    public void FormatScoreEntry(EventManager.ScoreEntry scoreEntry, ulong langUserId, string score1, string score2);
    public List<EventManager.EventParameter> AdditionalParameters { get; set; }
    public string ParameterIsValid(string fieldName, object value);
    public class DeathmatchEvent : EventManager.BaseEventGame
    {
        public EventManager.BaseEventPlayer winner;
        internal override void PrestartEvent();
        internal override void OnEventPlayerDeath(EventManager.BaseEventPlayer victim, EventManager.BaseEventPlayer attacker, HitInfo info);
        protected override void GetWinningPlayers(List<EventManager.BaseEventPlayer> winners);
        protected override void BuildScoreboard();
        protected override float GetFirstScoreValue(EventManager.BaseEventPlayer eventPlayer);
        protected override float GetSecondScoreValue(EventManager.BaseEventPlayer eventPlayer);
        protected override void SortScores(List<EventManager.ScoreEntry> list);
    }

    private static ConfigData Configuration;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Respawn time (seconds)")]
        public int RespawnTime { get; set; }
        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    public string Message(string key, ulong playerId);
    private static Func<string, ulong, string> GetMessage;
    private readonly Dictionary<string, string> Messages;
}

public class DeathmatchEvent : EventManager.BaseEventGame
{
    public EventManager.BaseEventPlayer winner;
    internal override void PrestartEvent();
    internal override void OnEventPlayerDeath(EventManager.BaseEventPlayer victim, EventManager.BaseEventPlayer attacker, HitInfo info);
    protected override void GetWinningPlayers(List<EventManager.BaseEventPlayer> winners);
    protected override void BuildScoreboard();
    protected override float GetFirstScoreValue(EventManager.BaseEventPlayer eventPlayer);
    protected override float GetSecondScoreValue(EventManager.BaseEventPlayer eventPlayer);
    protected override void SortScores(List<EventManager.ScoreEntry> list);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Respawn time (seconds)")]
    public int RespawnTime { get; set; }
    public Oxide.Core.VersionNumber Version { get; set; }
}


```

---

## DeathNotes by MrBlue - Broadcasts deaths to chat along with detailed information.

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Rust;
using Rust.Ai.Gen2;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using UnityEngine;

Oxide.Plugins
[Info("Death Notes", "Terceran/Mr. Blue", "6.4.3")]
[Description("Broadcasts deaths to chat along with detailed information")]
 class DeathNotes : RustPlugin
{
    private const string WildcardCharacter;
    private const string CanSeePermission;
    private const string SuppressPermission;
    private static DeathNotes _instance;
    private PluginConfiguration _configuration;
    private readonly Dictionary<string, string> _enemyPrefabs;
    private readonly Dictionary<string, string> _weaponPrefabs;
    private readonly Dictionary<string, CombatEntityType> _combatEntityTypes;
    private readonly Regex _colorTagRegex;
    private readonly Regex _sizeTagRegex;
    private readonly List<string> _richTextLiterals;
    private readonly Dictionary<ulong, AttackInfo> _previousAttack;
    private Dictionary<ulong, HitInfo> _patrolHeliTagTracker;
    private HashSet<ulong> _bradleyApcTagTracker;
    private readonly Func<PluginConfiguration.DeathMessage, DeathData, bool>[] _messageMatchingStages;
    private void Init();
    private void Unload();
    private void OnEntityTakeDamage(BasePlayer victimEntity, HitInfo hitInfo);
    private void OnEntityTakeDamage(BradleyAPC victimEntity, HitInfo hitInfo);
    private void OnPatrolHelicopterTakeDamage(PatrolHelicopter victimEntity, HitInfo hitInfo);
    private void HandleEntityDamage(BaseCombatEntity victimEntity, HitInfo hitInfo);
    private void OnEntityKill(PatrolHelicopter entity);
    private void OnEntityKill(BradleyAPC entity);
    private void OnEntityDeath(BaseCombatEntity victimEntity, HitInfo hitInfo);
    private void RepairEntityTypes(DeathData data);
    private void OnFlameThrowerBurn(FlameThrower flameThrower, BaseEntity baseEntity);
    private void OnFlameExplosion(FlameExplosive explosive, BaseEntity baseEntity);
    private void OnFireBallSpread(FireBall fireBall, BaseEntity newFire);
    private void OnFireBallDamage(FireBall fireBall, BaseCombatEntity target, HitInfo hitInfo);
    private string GetDeathMessage(DeathData data);
    private string PopulateMessageVariables(string message, DeathData data);
    private CombatEntityType GetCombatEntityType(BaseEntity entity);
    private string GetCustomizedEntityName(BaseEntity entity, CombatEntityType combatEntityType);
    private string GetEntityName(BaseEntity entity, CombatEntityType combatEntityType);
    private void HandleInconsistencies(DeathData data);
    private class Flame : MonoBehaviour
    {
        public FlameSource Source { get; set; }
        public BaseEntity SourceEntity { get; set; }
        public BaseEntity Initiator { get; set; }
    }

    private string GetCustomizedWeaponName(DeathData deathData);
    private string GetWeaponName(DeathData deathData);
    private string[] GetCustomizedAttachmentNames(HitInfo info);
    private string GetCustomizedAttachmentName(string name);
    private string GetCustomizedBodypartName(HitInfo hitInfo);
    private string GetBodypartName(HitInfo hitInfo);
    private static string GetDistance(float meters, bool useMetric);
    private static string ApplyVariableFormat(string text, string variableName);
    private static string InsertPlaceholderValues(string text, Dictionary<string, string> values);
    private static string HumanizePascalCase(string text);
    private string StripRichText(string text);
    private static bool MatchesCombatEntityType(CombatEntityType combatEntityType, string text);
    private static bool MatchesDamageType(DamageType damageType, string text);
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    private sealed class PluginConfiguration
    {
        [JsonProperty("Translations")]
        public Translation Translations;
        [JsonProperty("Variable Formats")]
        public Dictionary<string, string> VariableFormats;
        [JsonProperty("Variable Colors")]
        public Dictionary<string, string> VariableColors;
        [JsonProperty("Chat Format")]
        public string ChatFormat;
        [JsonProperty("Chat Icon (SteamID)")]
        public string ChatIcon;
        [JsonProperty("Show Kills in Console")]
        public bool ShowInConsole;
        [JsonProperty("Show Kills in Chat")]
        public bool ShowInChat;
        [JsonProperty("Show Patrol Heli Tags")]
        public bool ShowPatrolHeliTags;
        [JsonProperty("Show Bradley APC Tags")]
        public bool ShowBradleyTags;
        [JsonProperty("Patrol Helicopter Tag Message")]
        public string PatrolHeliTagMessage;
        [JsonProperty("Bradley Tag Message")]
        public string BradleyTagMessage;
        [JsonProperty("Message Broadcast Radius (in meters)")]
        public int MessageRadius;
        [JsonProperty("Use Metric Distance")]
        public bool UseMetricDistance;
        [JsonProperty("Require Permission (deathnotes.cansee)")]
        public bool RequirePermission;
        public void LoadDefaults();
        public class DeathMessage
        {
            public string KillerType { get; set; }
            public string VictimType { get; set; }
            public string DamageType { get; set; }
            public string[] Messages { get; set; }
            protected bool Equals(DeathMessage other);
            public DeathMessage(string killerType, string victimType, string damageType, string message);
        }

        public class Translation
        {
            [JsonProperty("Death Messages")]
            public List<DeathMessage> Messages;
            [JsonProperty("Names")]
            public Dictionary<string, string> Names;
            [JsonProperty("Bodyparts")]
            public Dictionary<string, string> Bodyparts;
            [JsonProperty("Weapons")]
            public Dictionary<string, string> Weapons;
            [JsonProperty("Attachments")]
            public Dictionary<string, string> Attachments;
        }

    }

}

private class Flame : MonoBehaviour
{
    public FlameSource Source { get; set; }
    public BaseEntity SourceEntity { get; set; }
    public BaseEntity Initiator { get; set; }
}

private sealed class PluginConfiguration
{
    [JsonProperty("Translations")]
    public Translation Translations;
    [JsonProperty("Variable Formats")]
    public Dictionary<string, string> VariableFormats;
    [JsonProperty("Variable Colors")]
    public Dictionary<string, string> VariableColors;
    [JsonProperty("Chat Format")]
    public string ChatFormat;
    [JsonProperty("Chat Icon (SteamID)")]
    public string ChatIcon;
    [JsonProperty("Show Kills in Console")]
    public bool ShowInConsole;
    [JsonProperty("Show Kills in Chat")]
    public bool ShowInChat;
    [JsonProperty("Show Patrol Heli Tags")]
    public bool ShowPatrolHeliTags;
    [JsonProperty("Show Bradley APC Tags")]
    public bool ShowBradleyTags;
    [JsonProperty("Patrol Helicopter Tag Message")]
    public string PatrolHeliTagMessage;
    [JsonProperty("Bradley Tag Message")]
    public string BradleyTagMessage;
    [JsonProperty("Message Broadcast Radius (in meters)")]
    public int MessageRadius;
    [JsonProperty("Use Metric Distance")]
    public bool UseMetricDistance;
    [JsonProperty("Require Permission (deathnotes.cansee)")]
    public bool RequirePermission;
    public void LoadDefaults();
    public class DeathMessage
    {
        public string KillerType { get; set; }
        public string VictimType { get; set; }
        public string DamageType { get; set; }
        public string[] Messages { get; set; }
        protected bool Equals(DeathMessage other);
        public DeathMessage(string killerType, string victimType, string damageType, string message);
    }

    public class Translation
    {
        [JsonProperty("Death Messages")]
        public List<DeathMessage> Messages;
        [JsonProperty("Names")]
        public Dictionary<string, string> Names;
        [JsonProperty("Bodyparts")]
        public Dictionary<string, string> Bodyparts;
        [JsonProperty("Weapons")]
        public Dictionary<string, string> Weapons;
        [JsonProperty("Attachments")]
        public Dictionary<string, string> Attachments;
    }

}

public class DeathMessage
{
    public string KillerType { get; set; }
    public string VictimType { get; set; }
    public string DamageType { get; set; }
    public string[] Messages { get; set; }
    protected bool Equals(DeathMessage other);
    public DeathMessage(string killerType, string victimType, string damageType, string message);
}

public class Translation
{
    [JsonProperty("Death Messages")]
    public List<DeathMessage> Messages;
    [JsonProperty("Names")]
    public Dictionary<string, string> Names;
    [JsonProperty("Bodyparts")]
    public Dictionary<string, string> Bodyparts;
    [JsonProperty("Weapons")]
    public Dictionary<string, string> Weapons;
    [JsonProperty("Attachments")]
    public Dictionary<string, string> Attachments;
}


```

---

## DeathNotesGui by LaserHydra - Provides a GUI output for Death Notes

```csharp
using Oxide.Core.Plugins;
using System.Collections.Generic;

Oxide.Plugins
[Info("Death Notes GUI", "LaserHydra", "1.0.0")]
[Description("Provides an GUI output option for Death Notes")]
public class DeathNotesGui : RustPlugin
{
    [PluginReference("PopupNotifications")]
     Plugin _popupNotifications;
    private void OnDeathNotice(Dictionary<string, object> data, string message);
}


```

---

## DeathNotesToggle by 0x2422 - Allows players to toggle Death Notes

```csharp
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Death Notes Toggle", "0x2422", "0.0.7")]
[Description("Allows players to toggle DeathNotes")]
 class DeathNotesToggle : RustPlugin
{
    [PluginReference]
    private Plugin DeathNotes;
    private bool ShowInConsole { get; set; }
    private bool ShowInChat { get; set; }
    private string ChatFormat { get; set; }
    private string ChatIcon { get; set; }
    private int MessageRadius { get; set; }
    private bool RequirePermission { get; set; }
    private void Init();
    private void Unload();
    private void OnServerSave();
    private void Loaded();
    [ChatCommand("dnt")]
    private void CmdToggleDeathNotes(BasePlayer player, string command, string[] args);
     object OnDeathNotice(Dictionary<string, object> data, string message);
    private static class DeathNotesCopy
    {
        internal const string Permission;
        public static void Puts(string format, object[] args);
    }

    private StoredData _storedData;
    private class StoredData
    {
        public readonly Dictionary<ulong, bool> PlayerData;
    }

    private void LoadData();
    private void SaveData();
    private void ClearData();
    private string GetMessage(BasePlayer player, string key, object[] args);
    protected override void LoadDefaultMessages();
}

private static class DeathNotesCopy
{
    internal const string Permission;
    public static void Puts(string format, object[] args);
}

private class StoredData
{
    public readonly Dictionary<ulong, bool> PlayerData;
}


```

---

## DecayNotifications by Ankawi - Automatic decay notifications and a command for upkeep status in minutes

```csharp
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("DecayNotifications", "Ankawi", "1.0.2")]
[Description("Automatic decay notifications and a command for upkeep status in minutes.")]
 class DecayNotifications : RustPlugin
{
    [PluginReference]
     Plugin HelpText;
    protected override void LoadDefaultConfig();
    private void Init();
    private void SendNotifications();
    [ChatCommand("tcstatus")]
     void TcstatusCommand(BasePlayer player, string command, string[] args);
    private void SendHelpText(BasePlayer player);
}


```

---

## DefaultRadioStation by marcuzz - Set default radio station for spawned and crafted boomboxes.

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Default Radio Station", "marcuzz", "1.1.0")]
[Description("Set default radio station for spawned, created or placed boomboxes.")]
public class DefaultRadioStation : RustPlugin
{
    private static PluginConfig _config;
     void OnEntitySpawned(DeployableBoomBox boombox);
     void OnEntitySpawned(HeldBoomBox boombox);
     void Unload();
    private void SetBoomboxRadioIP(BoomBox box);
    private static PluginConfig GetDefaultConfig();
    private class PluginConfig
    {
        [JsonProperty("Default radio station URL list (mp3 streams): ")]
        public List<string> DefaultRadioStationUrlList { get; set; }
        [JsonProperty("Set for deployed: ")]
        public bool SetDefaultDeployed { get; set; }
        [JsonProperty("Set for held: ")]
        public bool SetDefaultHeld { get; set; }
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
}

private class PluginConfig
{
    [JsonProperty("Default radio station URL list (mp3 streams): ")]
    public List<string> DefaultRadioStationUrlList { get; set; }
    [JsonProperty("Set for deployed: ")]
    public bool SetDefaultDeployed { get; set; }
    [JsonProperty("Set for held: ")]
    public bool SetDefaultHeld { get; set; }
}


```

---

## Deforestation by Lincoln - Delete bushes and make trees fall over like a boss!

```csharp
using Oxide.Core.Libraries.Covalence;
using System;
using UnityEngine;
using System.Collections.Generic;
using Newtonsoft.Json;
using static ConsoleSystem;

Oxide.Plugins
[Info("Deforestation", "Lincoln & redBDGR", "1.0.5")]
[Description("Make trees fall over like a boss.")]
 class Deforestation : RustPlugin
{
    private const string permUse;
    private const string permRadiusBypass;
    private float radius;
    private static PluginConfig config;
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Max Radius: ")]
        public int maxRadius { get; set; }
        [JsonProperty(PropertyName = "Number Of Tree Spawns: ")]
        public int numberOfTreesToSpawn { get; set; }
        public static PluginConfig DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     bool ValidationChecks(BasePlayer player, string[] args);
    private void Init();
    [ChatCommand("counttrees")]
    private void CountTreesCMD(BasePlayer player, string command, string[] args);
    [ChatCommand("killtree")]
    private void TreeDeforestationCMD(BasePlayer player, string command, string[] args);
    [ChatCommand("killbush")]
    private void BushDeforestationCMD(BasePlayer player, string command, string[] args);
    [ChatCommand("growtree")]
    private void TreeGrowCMD(BasePlayer player, string command, string[] args);
    private Vector3 GetRandomPositionWithinRadius(Vector3 center, float radius);
    private void SpawnTree(Vector3 position);
    private void ReplyToPlayer(IPlayer player, string messageName, object[] args);
    private void ChatMessage(BasePlayer player, string messageName, object[] args);
    private string GetMessage(IPlayer player, string messageName, object[] args);
    private string GetMessage(string playerId, string messageName, object[] args);
    protected override void LoadDefaultMessages();
    private void Unload();
}

private class PluginConfig
{
    [JsonProperty(PropertyName = "Max Radius: ")]
    public int maxRadius { get; set; }
    [JsonProperty(PropertyName = "Number Of Tree Spawns: ")]
    public int numberOfTreesToSpawn { get; set; }
    public static PluginConfig DefaultConfig();
}


```

---

## DegreeTags by yoshi2 - Gives chat tags to players that have building degrees from the bulletin discord

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Plugins;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

Oxide.Plugins
[Info("Bulletin Degree Tags", "Yoshi", 0.2)]
 class DegreeTags : CovalencePlugin
{
    [PluginReference]
    private Plugin BetterChat;
    private void OnPluginLoaded(Plugin plugin);
     void OnServerInitialized();
    private string GetBulletinTags(IPlayer player);
    private string GetTagColor(Degrees degree);
     Dictionary<ulong, List<Degrees>> playerDegrees;
     void UpdateDegreeCache();
    public class DegreeData
    {
        public ulong UserID { get; set; }
        public List<Degrees> Degrees { get; set; }
    }

    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Bachelors tag color")]
        public string BachelorsColor;
        [JsonProperty(PropertyName = "Masters tag color")]
        public string MastersColor;
        [JsonProperty(PropertyName = "PhD tag color")]
        public string PhDColor;
        [JsonProperty(PropertyName = "Professor tag color")]
        public string ProfessorColor;
        [JsonProperty(PropertyName = "Only show highest degree")]
        public bool highestDegree;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
}

public class DegreeData
{
    public ulong UserID { get; set; }
    public List<Degrees> Degrees { get; set; }
}

public class Configuration
{
    [JsonProperty(PropertyName = "Bachelors tag color")]
    public string BachelorsColor;
    [JsonProperty(PropertyName = "Masters tag color")]
    public string MastersColor;
    [JsonProperty(PropertyName = "PhD tag color")]
    public string PhDColor;
    [JsonProperty(PropertyName = "Professor tag color")]
    public string ProfessorColor;
    [JsonProperty(PropertyName = "Only show highest degree")]
    public bool highestDegree;
}


```

---

## DespawnConfig by MrBlue - Configurable despawn times for dropped items and item containers

```csharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Despawn Config", "Wulf", "2.2.3")]
[Description("Configurable despawn times for dropped items and item containers")]
 class DespawnConfig : CovalencePlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty("Global despawn time")]
        public float GlobalDespawnTime;
        [JsonProperty("Item container despawn time")]
        public float ItemContainerDespawnTime;
        [JsonProperty("Despawn item container with items")]
        public bool DespawnContainerWithItems;
        [JsonProperty("Item despawn times", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public SortedDictionary<string, float> ItemDespawnTimes;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
        public float GetItemDespawnTime(Item item);
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private static DespawnConfig _plugin;
    private void Init();
    private void Unload();
    private void OnServerInitialized();
    private void OnDroppedItemCombined(DroppedItem item);
    private void OnEntitySpawned(DroppedItem item);
    private void OnEntitySpawned(DroppedItemContainer itemContainer);
    private void SetDespawnTime(BaseNetworkable itemOrContainer);
}

public class Configuration
{
    [JsonProperty("Global despawn time")]
    public float GlobalDespawnTime;
    [JsonProperty("Item container despawn time")]
    public float ItemContainerDespawnTime;
    [JsonProperty("Despawn item container with items")]
    public bool DespawnContainerWithItems;
    [JsonProperty("Item despawn times", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public SortedDictionary<string, float> ItemDespawnTimes;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
    public float GetItemDespawnTime(Item item);
}


```

---

## DevblogAnnouncer by LaserHydra - Broadcasts to the chat and console when a new Devblog or Community Update was released

```csharp
using System.Text.RegularExpressions;
using System.Collections.Generic;
using Oxide.Core.Libraries;
using System.Linq;
using Oxide.Core;
using System;

Oxide.Plugins
[Info("Devblog Announcer", "LaserHydra", "1.2.0", ResourceId = 1340)]
[Description("Broadcasts to chat when a new Devblog or Community Update was released.")]
 class DevblogAnnouncer : RustPlugin
{
    private readonly WebRequests webRequests;
     class Data
    {
        public int Devblog;
        public int CommunityUpdate;
    }

     Data data;
     void Loaded();
    protected override void LoadDefaultConfig();
    [ConsoleCommand("getblogs")]
     void GetLatest();
     void SaveData();
     void LoadConfig();
     void CheckForBlogs();
     void DataRecieved(int code, string response);
     string ListToString(List<string> list, int first, string seperator);
     void SetConfig(object[] args);
     T GetConfig(T defaultVal, object[] args);
     void BroadcastChat(string prefix, string msg, object userID);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}

 class Data
{
    public int Devblog;
    public int CommunityUpdate;
}


```

---

## DevilsIsland by  - Complete game mode that provides more endgame content and more

```csharp
using DevilsIsland;
using JetStream;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Plugins;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using UnityEngine;

Oxide.Plugins
[Info("Devil's Island", "Nick Holmes/Default", "0.8.0", ResourceId = 1372)]
[Description("Devil's Island Game Mode")]
public class DevilsIsland : GameModePlugin<DevilsIslandConfig, DevilsIslandState>
{
    private ILocator liveLocator;
    private ILocator locator;
    private bool Changed;
    protected override void Initialize();
    private void LoadMessages();
     void LoadVariables();
     void AdviseBossPosition();
    public void AdviseRules();
    public void TryForceBoss();
    [ChatCommand("rules")]
     void RulesCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("status")]
     void StatusCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("claim")]
     void ClaimCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("loot")]
     void LootCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("heli")]
     void HeliCommmand(BasePlayer player, string command, string[] args);
    [ChatCommand("decoy")]
     void DecoyCommmand(BasePlayer player, string command, string[] args);
    [ChatCommand("tax")]
     void SetTaxCommmand(BasePlayer player, string command, string[] args);
    [ChatCommand("where")]
     void WhereCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("rebel")]
     void RebelCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("recruit")]
     void RecruitCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("accept")]
     void AcceptCommand(BasePlayer player, string command, string[] args);
    [ConsoleCommand("devilsisland.diagnostic")]
    private void DiagnosticCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("devilsisland.reset")]
    private void ResetCommand(ConsoleSystem.Arg arg);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnQuarryGather(MiningQuarry quarry, Item item);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnPlayerDisconnected(BasePlayer player, string reason);
    public BasePlayer Boss { get; set; }
}

DevilsIsland
public class DevilsIslandState : IGameState
{
    public DevilsIslandState();
    public void AttachConfig(IGameConfig configFile);
    private DevilsIslandConfig config;
    private BasePlayer currentBoss;
    internal DateTime noBossSince;
    private float taxRate;
    private Dictionary<ItemDefinition, float> coffers;
    public bool NoBoss();
    public bool BossExists();
    public bool IsBoss(BasePlayer player);
    public bool IsNotBoss(BasePlayer player);
    public BasePlayer Boss { get; set; }
    public string BossName { get; set; }
    public void SetBoss(BasePlayer newBoss);
    public bool TryForceNewBoss();
    public float TaxRate { get; set; }
    public StorageContainer LootContainer { get; set; }
    public IEnumerable<ItemDefinition> CofferItems { get; set; }
    public float CofferAmount(ItemDefinition key);
    public void CollectTaxFrom(Item item);
    public bool CanNotAffordheliStrike(BasePlayer player);
    public void OrderheliStrike(string targetPartialName);
    public Outlaws Outlaws { get; set; }
    public bool CanAffordDecoy(BasePlayer player);
    public void Decoy(BasePlayer player, string targetPartialName);
    public Henchmen Henchmen { get; set; }
    public List<BasePlayer> PendingRequest { get; set; }
    public BasePlayer TryRecruit(string playerPartialName);
    public bool TryPromote(BasePlayer player);
    public bool PlayerGoodMatch(string playerPartialName);
}

public class Outlaws
{
     List<Outlaw> outlaws;
    public void Clear();
    public void Add(BasePlayer newOutlaw);
    public void Remove(BasePlayer oldOutlaw);
    public bool Contains(BasePlayer player);
    public bool Any();
    public IEnumerable<Outlaw> All();
    public bool TryResovleByPlayer(BasePlayer player, Outlaw matchingOutlaw);
    public bool HasMatchByPartialName(string partialName);
    public bool TryResolveByPartialName(string partialName, Outlaw matchingOutlaw);
}

public class Outlaw
{
    public Outlaw(BasePlayer player);
    public BasePlayer Player { get; set; }
    private BasePlayer decoyTarget;
    private DateTime decoyUntil;
    public void SetDecoy(BasePlayer player, DateTime until);
    public BasePlayer GetEffectiveTarget();
}

public class Henchmen
{
     List<Henchman> henchmen;
    public void Clear();
    public void Add(BasePlayer newHenchman);
    public void Remove(BasePlayer oldHenchman);
    public bool Contains(BasePlayer player);
    public bool Any();
    public IEnumerable<Henchman> All();
    public bool TryResovleByPlayer(BasePlayer player, Henchman matchingHenchman);
    public bool HasMatchByPartialName(string partialName);
    public bool TryResolveByPartialName(string partialName, Henchman matchingHenchman);
}

public class Henchman
{
    public Henchman(BasePlayer player);
    public BasePlayer Player { get; set; }
}

public class DevilsIslandConfig : IGameConfig
{
    public void AttachConfigFile(DynamicConfigFile configFile);
    private DynamicConfigFile configFile;
    public void UpdateConfigFile();
    private bool DefaultValue(string key, object defaultValue);
    public bool IsHelpNotiferEnabled { get; set; }
    public int HelpNotifierInverval { get; set; }
    public bool IsBossPositionNotifierEnabled { get; set; }
    public int BossPositionNotifierInterval { get; set; }
    public int AutoBossPromoteDelay { get; set; }
    public int AutoBossPromoteMinPlayers { get; set; }
    private ItemDefinition heliStrikePriceItemDef;
    public ItemDefinition heliStrikePrice_ItemDef { get; set; }
    public int HeliStrikePrice_Quantity { get; set; }
    public int Maxhelis { get; set; }
    private ItemDefinition decoyPriceItemDef;
    public ItemDefinition DecoyPrice_ItemId { get; set; }
    public int DecoyPrice_Quantity { get; set; }
    private ItemDefinition evadePriceItemDef;
    public ItemDefinition EvadePrice_ItemId { get; set; }
    public int EvadePrice_Quantity { get; set; }
    public int FallbackWorldSize { get; set; }
    private static ItemDefinition FindOrDefault(string itemName, string fallbackName);
    public bool AllowBossDisconnect { get; set; }
}

public class RustIOLocator : ILocator
{
    public RustIOLocator(int worldSize);
    private readonly float translate;
    private readonly float scale;
    public string GridReference(Component component, bool moved);
}

public class LocatorWithDelay : ILocator
{
    public LocatorWithDelay(ILocator liveLocator, int updateInterval);
    private readonly ILocator liveLocator;
    private readonly int updateInterval;
    private readonly Dictionary<Component, ExpiringCoordinates> locations;
    public string GridReference(Component component, bool moved);
     class ExpiringCoordinates
    {
        public string Location { get; set; }
        public bool GridChanged { get; set; }
        public DateTime Expires { get; set; }
    }

}

 class ExpiringCoordinates
{
    public string Location { get; set; }
    public bool GridChanged { get; set; }
    public DateTime Expires { get; set; }
}

public static class Text
{
    public const string NoBoss;
    public const string YouAreTheBoss;
    public const string YouAreAnOutlaw;
    public const string CurrentBoss;
    public const string CurrentTaxBox;
    public const string TaxRate;
    public const string CofferItem;
    public const string YourLocation;
    public const string Synopsis;
    public const string PlayerCommandSection;
    public const string StatusCommandHint;
    public const string ClaimCommandHint;
    public const string RebelCommandHint;
    public const string WhereCommandHint;
    public const string BossCommandSection;
    public const string TaxCommandHint;
    public const string LootCommandHint;
    public const string heliStrikeCommandHint;
    public const string RecruitCommandHint;
    public const string Broadcast_ClaimAvailable;
    public const string Broadcast_HelpAdvice;
    public const string Error_YourAreBoss;
    public const string Error_OtherIsBoss;
    public const string Success_WelcomeNewBoss;
    public const string Broadcast_FirstBoss;
    public const string Error_Loot_NoBoss;
    public const string Error_Loot_NotBoss;
    public const string Success_Looting;
    public const string Error_TaxChange_NoBoss;
    public const string Error_TaxChange_NotBoss;
    public const string Error_TaxChange_BadArgs;
    public const string Broadcast_TaxIncrease;
    public const string Broadcast_TaxDecrease;
    public const string Error_Rebel_IsTheBoss;
    public const string Error_Rebel_IsAlreadyOutlaw;
    public const string Broadcast_NewOutlaw;
    public const string Broadcast_BossSuicide;
    public const string Broadcast_NewBoss;
    public const string Broadcast_BossLocation_Moved;
    public const string Broadcast_BossLocation_Static;
}

JetStream
public class GameModePlugin : RustPlugin
{
    private Dictionary<string, Timer> timers;
    protected Dictionary<string, Timer> Timers { get; set; }
    protected TConfig GameConfig { get; set; }
    protected TState State { get; set; }
    protected override void LoadDefaultConfig();
    [HookMethod("OnServerInitialized")]
     void base_OnServerInitialized();
    [HookMethod("Unload")]
     void base_Unload();
    protected virtual void Initialize();
    protected bool GuardAgainst(Func<bool> condition, BasePlayer player, string errorMsgFormat, object[] args);
}


```

---

## Dice by MrBlue - Feeling lucky? Roll dice to get a random number

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Dice", "Wulf/lukespragg", "0.3.0", ResourceId = 655)]
[Description("Feeling lucky? Roll dice to get a random number")]
public class Dice : CovalencePlugin
{
    private static System.Random random;
    private const string permUse;
    private void Init();
    private new void LoadDefaultMessages();
    private void DiceCommand(IPlayer player, string command, string[] args);
    private void AddCommandAliases(string key, string command);
    private string Lang(string key, string id, object[] args);
    private void Broadcast(string key, object[] args);
    private void Message(IPlayer player, string key, object[] args);
}


```

---

## DisableColdDamage by Talha - Prevents cold damage for players with permission

```csharp
using System.Linq;

Oxide.Plugins
[Info("Disable Cold Damage", "Talha", "1.0.4")]
[Description("Prevents cold damage for players, with permission.")]
public class DisableColdDamage : RustPlugin
{
    private const string permDisable;
     void Init();
     void OnServerInitialized();
     void OnGroupPermissionGranted();
     void OnGroupPermissionRevoked();
     void OnPlayerConnected(BasePlayer player);
     void Unload();
     void CheckAll();
     void Cold(BasePlayer player);
}


```

---

## DisableDamage by 2CHEVSKII - Allows players with permission to disable other player's damage

```csharp
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[Info("Disable Damage", "2CHEVSKII", "0.4.0")]
[Description("Allows players with permission to disable other player's damage.")]
 class DisableDamage : RustPlugin
{
     bool displayerdmg;
     bool disNPCdmg;
     bool disStructdmg;
     bool disAnimaldmg;
     bool disBarreldmg;
     bool disHelidmg;
     bool disTransportdmg;
     bool announceTarget;
     bool announceName;
     bool savedisabled;
     bool disDefaultdmg;
     List<ulong> DisabledDamage { get; set; }
    const string PERMISSION_USE;
    const string M_PREFIX;
    const string M_SELF_DAMAGE_ENABLED;
    const string M_SELF_DAMAGE_DISABLED;
    const string M_PLAYER_DAMAGE_ENABLED;
    const string M_PLAYER_DAMAGE_DISABLED;
    const string M_ENABLED_BY;
    const string M_DISABLED_BY;
    const string M_NO_PERMISSION;
    const string M_PLAYER_NOT_FOUND;
    const string M_HELP;
    const string M_UNAVAILABLE;
     void CheckConfig(string menu, string key, T value);
    protected override void LoadDefaultConfig();
     void LoadConfiguration();
    protected override void LoadDefaultMessages();
     void Replier(BasePlayer player, string message, string[] args);
     Dictionary<string, string> defmessages;
    [ChatCommand("dd")]
     void CmdDD(BasePlayer player, string command, string[] args);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void Init();
     void Unload();
}


```

---

## DisableRadiation by bushhy - Removes radiation damage from players based on permission.

```csharp
using System;
using UnityEngine.Experimental;

Oxide.Plugins
[Info("Disable Radiation", "SwenenzY/Bushhy", "1.0.2")]
[Description("Disable radiation with permission")]
 class DisableRadiation : CovalencePlugin
{
    private const string permUse;
    private void Init();
    private void OnUserPermissionGranted(string playerId, string perm);
    private void OnUserPermissionRevoked(string playerId, string perm);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
}


```

---

## DisableTemperatureFunctions by TheFriendlyChap - Prevents cold/heat damage/overlay for players

```csharp
using System.Linq;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Disable Temperature Functions", "The Friendly Chap", "1.0.2")]
[Description("Prevents cold/heat damage/overlay for players")]
public class DisableTemperatureFunctions : RustPlugin
{
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Debug Mode")]
        public bool debug;
        [JsonProperty(PropertyName = "Set Temprature to (�C)")]
        public float usertemp;
        [JsonProperty(PropertyName = "Use permission : ")]
        public bool usePerm;
    }

    private bool LoadConfigVariables();
     void Init();
    protected override void LoadDefaultConfig();
     void SaveConfig(ConfigData config);
    private const string permDisable;
    private void OnServerInitialized();
    private void OnPlayerSleep(BasePlayer player);
    private void OnPlayerSleepEnded(BasePlayer player);
    private void FixTemp(BasePlayer player);
    private void Check(BasePlayer player);
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Debug Mode")]
    public bool debug;
    [JsonProperty(PropertyName = "Set Temprature to (�C)")]
    public float usertemp;
    [JsonProperty(PropertyName = "Use permission : ")]
    public bool usePerm;
}


```

---

## DisableWet by SwenenzY - Stops/eliminates wetness for authorized players

```csharp
using System;

Oxide.Plugins
[Info("Disable Wet", "SwenenzY", "1.0.4")]
[Description("disable wet count, with permission.")]
public class DisableWet : RustPlugin
{
    private const string Perm;
    private void Init();
    private void OnUserPermissionGranted(string id, string permName);
    private void OnUserPermissionRevoked(string id, string permName);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
}


```

---

## DisconnectToHome by Ryz0r - Allows players with permission to be teleported back "home" on disconnect.

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("Disconnect To Home", "Ryz0r", "1.0.2")]
[Description("Sends a player back to their defined home location when they disconnect.")]
public class DisconnectToHome : RustPlugin
{
    const string UsePerm;
    private PluginData _data;
    private void SaveData();
    private void LoadData();
    private class PluginData
    {
        [JsonProperty(PropertyName = "Home Locations")]
        public Dictionary<string, Vector3> HomeLocations;
    }

    protected override void LoadDefaultMessages();
    private void Loaded();
    private void OnServerSave();
    [ChatCommand("disconnecthere")]
    private void DisconnectCommand(BasePlayer bp, string command, string[] args);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnPlayerConnected(BasePlayer player);
    private bool CheckIfOnFoundationOrFloor(BasePlayer player);
}

private class PluginData
{
    [JsonProperty(PropertyName = "Home Locations")]
    public Dictionary<string, Vector3> HomeLocations;
}


```

---

## DiscordAuth by  - Allows players to connect their discord account with Steam

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Text;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord.Builders;
using Oxide.Ext.Discord.Clients;
using Oxide.Ext.Discord.Connections;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Extensions;
using Oxide.Ext.Discord.Interfaces;
using Oxide.Ext.Discord.Libraries;
using Oxide.Ext.Discord.Logging;
using Random = Oxide.Core.Random;

Oxide.Plugins
[Info("Discord Auth", "OuTSMoKE", "1.4.0")]
[Description("Allows players to connect their discord account with steam")]
public class DiscordAuth : CovalencePlugin, IDiscordPlugin, IDiscordLink
{
    public DiscordClient Client { get; set; }
    private Configuration _pluginConfig;
    private Data _pluginData;
    private readonly BotConnection _settings;
    private readonly DiscordLink _link;
    private DiscordGuild _guild;
    private string _groupNames;
    private string _roleNames;
    private readonly List<DiscordRole> _roles;
    private readonly StringBuilder _builder;
    private readonly Dictionary<string, string> _codes;
    private char[] _codeCharacters;
    private const string AuthPerm;
    private const string DeauthPerm;
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private void AuthCommand(IPlayer player, string command, string[] args);
    private void DeauthCommand(IPlayer player, string command, string[] args);
    private void Init();
    private void OnServerInitialized();
    private void OnUserConnected(IPlayer player);
    private void OnServerSave();
    private void Unload();
    [HookMethod(DiscordExtHooks.OnDiscordClientCreated)]
    private void OnDiscordClientCreated();
    [HookMethod(DiscordExtHooks.OnDiscordGatewayReady)]
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
    [HookMethod(DiscordExtHooks.OnDiscordGuildMembersLoaded)]
    private void OnDiscordGuildMembersLoaded(DiscordGuild guild);
    [HookMethod(DiscordExtHooks.OnDiscordGuildMemberRemoved)]
    private void OnDiscordGuildMemberRemoved(GuildMemberRemovedEvent removed, DiscordGuild guild);
    [HookMethod(DiscordExtHooks.OnDiscordGuildMemberAdded)]
    private void OnDiscordGuildMemberAdded(GuildMember member, DiscordGuild guild);
    [HookMethod(DiscordExtHooks.OnDiscordDirectMessageCreated)]
    private void OnDiscordDirectMessageCreated(DiscordMessage message);
    public void Authenticate(IPlayer player, DiscordUser user);
    public void Deauthenticate(IPlayer player, DiscordUser user, DeauthReason reason);
    public void ProcessNextLeft(List<KeyValuePair<string, Snowflake>> leftLinks);
    public void CheckInactivePlayers();
    public DiscordUser GetDiscordUser(Snowflake userId);
    public void SaveData();
    public string Lang(string key, IPlayer player);
    public string Lang(string key, IPlayer player, object[] args);
    public void Message(IPlayer player, string key, object[] args);
    public void Message(IPlayer player, string key);
    public string GenerateCode();
    public DiscordEmbed GetEmbed(string text, uint color);
    public IDictionary<PlayerId, Snowflake> GetPlayerIdToDiscordIds();
     class Configuration
    {
        [JsonProperty(PropertyName = "Settings")]
        public Settings Info;
        [JsonProperty(PropertyName = "Authentication Code")]
        public AuthCode Code;
        public class Settings
        {
            [JsonProperty(PropertyName = "Bot Token")]
            public string BotToken;
            [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
            public Snowflake GuildId;
            [JsonProperty(PropertyName = "Auth Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public string[] AuthCommands;
            [JsonProperty(PropertyName = "Deauth Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public string[] DeauthCommands;
            [JsonProperty(PropertyName = "Oxide Groups to Assign", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public List<string> Groups;
            [JsonProperty(PropertyName = "Discord Roles to Assign", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public List<string> Roles;
            [JsonProperty(PropertyName = "Revoke Oxide Groups on Deauthenticate")]
            public bool RemoveFromGroups;
            [JsonProperty(PropertyName = "Revoke Discord Roles on Deauthenticate")]
            public bool RemoveFromRoles;
            [JsonProperty(PropertyName = "Automatically Reauthenticate on Leaving and Rejoining the Discord Server")]
            public bool AutomaticallyReauthenticate;
            [JsonProperty(PropertyName = "Automatically Deauthenticate Non Active Players")]
            public bool DeauthNonActive;
            [JsonProperty(PropertyName = "Player Considered Non Active After (Days)")]
            public float NonActiveDuration;
            [JsonConverter(typeof(StringEnumConverter))]
            [DefaultValue(DiscordLogLevel.Info)]
            [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
            public DiscordLogLevel ExtensionDebugging;
        }

        public class AuthCode
        {
            [JsonProperty(PropertyName = "Code Lifetime (minutes)")]
            public int CodeLifetime;
            [JsonProperty(PropertyName = "Code Length")]
            public int CodeLength;
            [JsonProperty(PropertyName = "Code Case Insensitive Match")]
            public bool CaseInsensitiveMatch;
            [JsonProperty(PropertyName = "Code Characters")]
            public string CodeChars;
        }

    }

    private class Data
    {
        public Hash<string, Snowflake> Players;
        public Hash<Snowflake, string> Backup;
        public Hash<string, DateTime> LastJoinedDate;
    }

}

 class Configuration
{
    [JsonProperty(PropertyName = "Settings")]
    public Settings Info;
    [JsonProperty(PropertyName = "Authentication Code")]
    public AuthCode Code;
    public class Settings
    {
        [JsonProperty(PropertyName = "Bot Token")]
        public string BotToken;
        [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
        public Snowflake GuildId;
        [JsonProperty(PropertyName = "Auth Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public string[] AuthCommands;
        [JsonProperty(PropertyName = "Deauth Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public string[] DeauthCommands;
        [JsonProperty(PropertyName = "Oxide Groups to Assign", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Groups;
        [JsonProperty(PropertyName = "Discord Roles to Assign", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Roles;
        [JsonProperty(PropertyName = "Revoke Oxide Groups on Deauthenticate")]
        public bool RemoveFromGroups;
        [JsonProperty(PropertyName = "Revoke Discord Roles on Deauthenticate")]
        public bool RemoveFromRoles;
        [JsonProperty(PropertyName = "Automatically Reauthenticate on Leaving and Rejoining the Discord Server")]
        public bool AutomaticallyReauthenticate;
        [JsonProperty(PropertyName = "Automatically Deauthenticate Non Active Players")]
        public bool DeauthNonActive;
        [JsonProperty(PropertyName = "Player Considered Non Active After (Days)")]
        public float NonActiveDuration;
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DiscordLogLevel.Info)]
        [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel ExtensionDebugging;
    }

    public class AuthCode
    {
        [JsonProperty(PropertyName = "Code Lifetime (minutes)")]
        public int CodeLifetime;
        [JsonProperty(PropertyName = "Code Length")]
        public int CodeLength;
        [JsonProperty(PropertyName = "Code Case Insensitive Match")]
        public bool CaseInsensitiveMatch;
        [JsonProperty(PropertyName = "Code Characters")]
        public string CodeChars;
    }

}

public class Settings
{
    [JsonProperty(PropertyName = "Bot Token")]
    public string BotToken;
    [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
    public Snowflake GuildId;
    [JsonProperty(PropertyName = "Auth Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public string[] AuthCommands;
    [JsonProperty(PropertyName = "Deauth Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public string[] DeauthCommands;
    [JsonProperty(PropertyName = "Oxide Groups to Assign", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Groups;
    [JsonProperty(PropertyName = "Discord Roles to Assign", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Roles;
    [JsonProperty(PropertyName = "Revoke Oxide Groups on Deauthenticate")]
    public bool RemoveFromGroups;
    [JsonProperty(PropertyName = "Revoke Discord Roles on Deauthenticate")]
    public bool RemoveFromRoles;
    [JsonProperty(PropertyName = "Automatically Reauthenticate on Leaving and Rejoining the Discord Server")]
    public bool AutomaticallyReauthenticate;
    [JsonProperty(PropertyName = "Automatically Deauthenticate Non Active Players")]
    public bool DeauthNonActive;
    [JsonProperty(PropertyName = "Player Considered Non Active After (Days)")]
    public float NonActiveDuration;
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DiscordLogLevel.Info)]
    [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel ExtensionDebugging;
}

public class AuthCode
{
    [JsonProperty(PropertyName = "Code Lifetime (minutes)")]
    public int CodeLifetime;
    [JsonProperty(PropertyName = "Code Length")]
    public int CodeLength;
    [JsonProperty(PropertyName = "Code Case Insensitive Match")]
    public bool CaseInsensitiveMatch;
    [JsonProperty(PropertyName = "Code Characters")]
    public string CodeChars;
}

private class Data
{
    public Hash<string, Snowflake> Players;
    public Hash<Snowflake, string> Backup;
    public Hash<string, DateTime> LastJoinedDate;
}


```

---

## DiscordChat by MJSU - Allows players to chat between discord and the game server

```csharp
using ConVar;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord.Cache;
using Oxide.Ext.Discord.Clients;
using Oxide.Ext.Discord.Connections;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Extensions;
using Oxide.Ext.Discord.Interfaces;
using Oxide.Ext.Discord.Libraries;
using Oxide.Ext.Discord.Logging;
using Oxide.Ext.Discord.Types;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;

Oxide.Plugins
[Info("Discord Chat", "MJSU", "3.0.5")]
[Description("Allows chatting between discord and game server")]
public partial class DiscordChat : CovalencePlugin, IDiscordPlugin, IDiscordPool
{
    private AdminChatSettings _adminChatSettings;
    private const string AdminChatPermission;
    [HookMethod(nameof(OnAdminChat))]
    public void OnAdminChat(IPlayer player, string message);
    public void HandleAdminChatDiscordMessage(DiscordMessage message);
    public bool IsAdminChatEnabled();
    public bool CanPlayerAdminChat(IPlayer player);
    public bool SendBetterChatMessage(IPlayer player, string message, MessageSource source);
    public Dictionary<string, object> GetBetterChatMessageData(IPlayer player, string message);
    public List<string> GetBetterChatTags(Dictionary<string, object> data);
    private void OnClanChat(IPlayer player, string message);
    private void OnAllianceChat(IPlayer player, string message);
    public PlaceholderData GetClanPlaceholders(IPlayer player, string message);
    [HookMethod(DiscordExtHooks.OnDiscordClientCreated)]
    private void OnDiscordClientCreated();
    [HookMethod(DiscordExtHooks.OnDiscordGatewayReady)]
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
    [HookMethod(DiscordExtHooks.OnDiscordGuildCreated)]
    private void OnDiscordGuildCreated(DiscordGuild guild);
    public void SetupChannel(DiscordGuild guild, MessageSource source, Snowflake id, bool wipeNonBotMessages, Action<DiscordMessage> callback);
    private void OnGetChannelMessages(List<DiscordMessage> messages, Action<DiscordMessage> callback);
    public void HandleDiscordChatMessage(DiscordMessage message);
    [PluginReference]
    private Plugin BetterChat;
    public DiscordClient Client { get; set; }
    public DiscordPluginPool Pool { get; set; }
    private PluginConfig _pluginConfig;
    private readonly DiscordSubscriptions _subscriptions;
    private readonly DiscordPlaceholders _placeholders;
    private readonly DiscordMessageTemplates _templates;
    private bool _serverInitCalled;
    public readonly Hash<MessageSource, DiscordSendQueue> Sends;
    private readonly List<IPluginHandler> _plugins;
    public static DiscordChat Instance;
    private readonly object _true;
    public MessageSource GetSourceFromServerChannel(int channel);
    public DiscordChannel FindChannel(Snowflake channelId);
    public bool IsPluginLoaded(Plugin plugin);
    public new void Subscribe(string hook);
    public new void Unsubscribe(string hook);
    public void Puts(string message);
    private void OnUserApproved(string name, string id, string ip);
    private void OnUserConnected(IPlayer player);
    private void OnUserDisconnected(IPlayer player, string reason);
    public void ProcessPlayerState(MessageSource source, string langKey, PlaceholderData data);
    private void OnPluginLoaded(Plugin plugin);
    public void AddHandler(IPluginHandler handler);
    private void OnPluginUnloaded(Plugin plugin);
    private void OnUserChat(IPlayer player, string message);
    public void HandleChat(IPlayer player, string message, int channel);
    public string Lang(string key, PlaceholderData data);
    protected override void LoadDefaultMessages();
    private readonly Regex _channelMention;
    private readonly Regex _emojiRegex;
    private readonly MatchEvaluator _emojiEvaluator;
    public void HandleMessage(string content, IPlayer player, DiscordUser user, MessageSource source, DiscordMessage sourceMessage);
    public string ProcessEmojis(string message);
    public void ProcessMentions(DiscordMessage message, StringBuilder sb);
    public bool CanSendMessage(string message, IPlayer player, DiscordUser user, MessageSource source, DiscordMessage sourceMessage);
    public void ProcessCallbackMessages(string message, IPlayer player, DiscordUser user, MessageSource source, Action<string> completed, int index);
    public void ProcessMessage(StringBuilder message, IPlayer player, DiscordUser user, MessageSource source);
    public void SendMessage(string message, IPlayer player, DiscordUser user, MessageSource source, DiscordMessage sourceMessage);
    private PlaceholderData GetPlaceholders(string message, IPlayer player, DiscordUser user, DiscordMessage sourceMessage);
    public void RegisterPlaceholders();
    public string GetPlayerName(IPlayer player);
    public PlaceholderData GetDefault();
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized(bool startup);
    private void OnServerShutdown();
    private void Unload();
    public void RegisterTemplates();
    public DiscordMessageTemplate CreateTemplateEmbed(string description, DiscordColor color);
    public DiscordMessageTemplate CreatePrefixedTemplateEmbed(string description, DiscordColor color);
    public void SendGlobalTemplateMessage(TemplateKey templateName, DiscordChannel channel, PlaceholderData placeholders);
    public TemplateKey GetTemplateName(MessageSource source);
    public class ChatSettings
    {
        [JsonProperty("Chat Channel ID")]
        public Snowflake ChatChannel { get; set; }
        [JsonProperty("Replace Discord User Message With Bot Message")]
        public bool UseBotToDisplayChat { get; set; }
        [JsonProperty("Send Messages From Server Chat To Discord Channel")]
        public bool ServerToDiscord { get; set; }
        [JsonProperty("Send Messages From Discord Channel To Server Chat")]
        public bool DiscordToServer { get; set; }
        [JsonProperty("Add Discord Tag To In Game Messages When Sent From Discord")]
        public string DiscordTag { get; set; }
        [JsonProperty("Allow plugins to process Discord to Server Chat Messages")]
        public bool AllowPluginProcessing { get; set; }
        [JsonProperty("Text Replacements")]
        public Hash<string, string> TextReplacements { get; set; }
        [JsonProperty("Unlinked Settings")]
        public UnlinkedSettings UnlinkedSettings { get; set; }
        [JsonProperty("Message Filter Settings")]
        public MessageFilterSettings Filter { get; set; }
        public ChatSettings(ChatSettings settings);
    }

    public class MessageFilterSettings
    {
        [JsonProperty("Ignore messages from users in this list (Discord ID)")]
        public List<Snowflake> IgnoreUsers { get; set; }
        [JsonProperty("Ignore messages from users in this role (Role ID)")]
        public List<Snowflake> IgnoreRoles { get; set; }
        [JsonProperty("Ignored Prefixes")]
        public List<string> IgnoredPrefixes { get; set; }
        public MessageFilterSettings(MessageFilterSettings settings);
        public bool IgnoreMessage(DiscordMessage message, GuildMember member);
        public bool IsIgnoredUser(DiscordUser user, GuildMember member);
        public bool IsRoleIgnoredMember(GuildMember member);
        public bool IsIgnoredPrefix(string content);
    }

    public class PlayerStateSettings
    {
        [JsonProperty("Player State Channel ID")]
        public Snowflake PlayerStateChannel { get; set; }
        [JsonProperty("Show Admins")]
        public bool ShowAdmins { get; set; }
        [JsonProperty("Send Connecting Message")]
        public bool SendConnectingMessage { get; set; }
        [JsonProperty("Send Connected Message")]
        public bool SendConnectedMessage { get; set; }
        [JsonProperty("Send Disconnected Message")]
        public bool SendDisconnectedMessage { get; set; }
        public PlayerStateSettings(PlayerStateSettings settings);
    }

    public class PluginConfig
    {
        [JsonProperty(PropertyName = "Discord Bot Token")]
        public string DiscordApiKey { get; set; }
        [JsonProperty("Chat Settings")]
        public ChatSettings ChatSettings { get; set; }
        [JsonProperty("Player State Settings")]
        public PlayerStateSettings PlayerStateSettings { get; set; }
        [JsonProperty("Server State Settings")]
        public ServerStateSettings ServerStateSettings { get; set; }
        [JsonProperty("Plugin Support")]
        public PluginSupport PluginSupport { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DiscordLogLevel.Info)]
        [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel ExtensionDebugging { get; set; }
    }

    public class ServerStateSettings
    {
        [JsonProperty("Server State Channel ID")]
        public Snowflake ServerStateChannel { get; set; }
        [JsonProperty("Send Booting Message")]
        public bool SendBootingMessage { get; set; }
        [JsonProperty("Send Online Message")]
        public bool SendOnlineMessage { get; set; }
        [JsonProperty("Send Shutdown Message")]
        public bool SendShutdownMessage { get; set; }
        public ServerStateSettings(ServerStateSettings settings);
    }

    public class UnlinkedSettings
    {
        [JsonProperty("Allow Unlinked Players To Chat With Server")]
        public bool AllowedUnlinked { get; set; }
        public UnlinkedSettings(UnlinkedSettings settings);
    }

    public class DiscordSendQueue
    {
        private readonly StringBuilder _message;
        private Timer _sendTimer;
        private readonly DiscordChannel _channel;
        private readonly TemplateKey _templateId;
        private readonly Action _callback;
        private readonly PluginTimers _timer;
        public DiscordSendQueue(DiscordChannel channel, TemplateKey templateId, PluginTimers timers);
        public void QueueMessage(string message);
        public void Send();
    }

    public static class LangKeys
    {
        public const string Root;
        public static class Discord
        {
            private const string Base;
            public static class Chat
            {
                private const string Base;
                public const string Server;
                public const string LinkedMessage;
                public const string UnlinkedMessage;
                public const string PlayerName;
            }

            public static class Team
            {
                private const string Base;
                public const string Message;
            }

            public static class Cards
            {
                private const string Base;
                public const string Message;
            }

            public static class Clans
            {
                private const string Base;
                public const string Message;
            }

            public static class Player
            {
                private const string Base;
                public const string Connecting;
                public const string Connected;
                public const string Disconnected;
            }

            public static class AdminChat
            {
                private const string Base;
                public const string ServerMessage;
                public const string DiscordMessage;
            }

            public static class PluginClans
            {
                private const string Base;
                public const string ClanMessage;
                public const string AllianceMessage;
            }

        }

        public static class Server
        {
            private const string Base;
            public const string LinkedMessage;
            public const string UnlinkedMessage;
        }

    }

    public class PlaceholderDataKeys
    {
        public static readonly PlaceholderDataKey TemplateMessage;
        public static readonly PlaceholderDataKey PlayerMessage;
        public static readonly PlaceholderDataKey DisconnectReason;
    }

    public class PlaceholderKeys
    {
        public static readonly PlaceholderKey TemplateMessage;
        public static readonly PlaceholderKey PlayerMessage;
        public static readonly PlaceholderKey DisconnectReason;
        public static readonly PlaceholderKey PlayerName;
        public static readonly PlaceholderKey DiscordTag;
    }

    public class AdminChatHandler : BasePluginHandler
    {
        private readonly AdminChatSettings _settings;
        private readonly DiscordClient _client;
        public AdminChatHandler(DiscordClient client, DiscordChat chat, AdminChatSettings settings, Plugin plugin);
        public override bool CanSendMessage(string message, IPlayer player, DiscordUser user, MessageSource source, DiscordMessage sourceMessage);
        public override bool SendMessage(string message, IPlayer player, DiscordUser user, MessageSource source, DiscordMessage sourceMessage, PlaceholderData data);
        private bool IsAdminChatMessage(IPlayer player, string message);
    }

    public class AdminDeepCoverHandler : BasePluginHandler
    {
        public AdminDeepCoverHandler(DiscordChat chat, Plugin plugin);
        public override bool CanSendMessage(string message, IPlayer player, DiscordUser user, MessageSource type, DiscordMessage sourceMessage);
    }

    public class AntiSpamHandler : BasePluginHandler
    {
        private readonly AntiSpamSettings _settings;
        public AntiSpamHandler(DiscordChat chat, AntiSpamSettings settings, Plugin plugin);
        public override void ProcessPlayerName(StringBuilder name, IPlayer player);
        public override void ProcessMessage(StringBuilder message, IPlayer player, DiscordUser user, MessageSource source);
        private bool CanFilterMessage(MessageSource source);
    }

    public abstract class BasePluginHandler : IPluginHandler
    {
        protected readonly DiscordChat Chat;
        protected readonly Plugin Plugin;
        private readonly string _pluginName;
        protected BasePluginHandler(DiscordChat chat, Plugin plugin);
        public virtual bool CanSendMessage(string message, IPlayer player, DiscordUser user, MessageSource source, DiscordMessage sourceMessage);
        public virtual void ProcessPlayerName(StringBuilder name, IPlayer player);
        public virtual bool HasCallbackMessage();
        public virtual void ProcessCallbackMessage(string message, IPlayer player, DiscordUser user, MessageSource source, Action<string> callback);
        public virtual void ProcessMessage(StringBuilder message, IPlayer player, DiscordUser user, MessageSource source);
        public virtual bool SendMessage(string message, IPlayer player, DiscordUser user, MessageSource source, DiscordMessage sourceMessage, PlaceholderData data);
        public string GetPluginName();
    }

    public class BetterChatHandler : BasePluginHandler
    {
        private readonly BetterChatSettings _settings;
        private readonly Regex _rustRegex;
        public BetterChatHandler(DiscordChat chat, BetterChatSettings settings, Plugin plugin);
        public override void ProcessPlayerName(StringBuilder name, IPlayer player);
    }

    public class BetterChatMuteHandler : BasePluginHandler
    {
        private readonly BetterChatMuteSettings _settings;
        public BetterChatMuteHandler(DiscordChat chat, BetterChatMuteSettings settings, Plugin plugin);
        public override bool CanSendMessage(string message, IPlayer player, DiscordUser user, MessageSource source, DiscordMessage sourceMessage);
    }

    public class DiscordChatHandler : BasePluginHandler
    {
        private readonly ChatSettings _settings;
        private readonly IServer _server;
        public DiscordChatHandler(DiscordChat chat, ChatSettings settings, Plugin plugin, IServer server);
        public override bool CanSendMessage(string message, IPlayer player, DiscordUser user, MessageSource source, DiscordMessage sourceMessage);
        public override bool SendMessage(string message, IPlayer player, DiscordUser user, MessageSource source, DiscordMessage sourceMessage, PlaceholderData data);
        public void SendLinkedToServer(IPlayer player, string message, PlaceholderData placeholders, MessageSource source);
        public void SendUnlinkedToServer(PlaceholderData placeholders);
        public override void ProcessMessage(StringBuilder message, IPlayer player, DiscordUser user, MessageSource source);
    }

    public class TranslationApiHandler : BasePluginHandler
    {
        private readonly ChatTranslatorSettings _settings;
        public TranslationApiHandler(DiscordChat chat, ChatTranslatorSettings settings, Plugin plugin);
        public override bool HasCallbackMessage();
        public override void ProcessCallbackMessage(string message, IPlayer player, DiscordUser user, MessageSource source, Action<string> callback);
        public bool CanChatTranslatorSource(MessageSource source);
    }

    public class UFilterHandler : BasePluginHandler
    {
        private readonly UFilterSettings _settings;
        private readonly List<string> _replacements;
        public UFilterHandler(DiscordChat chat, UFilterSettings settings, Plugin plugin);
        public override void ProcessPlayerName(StringBuilder name, IPlayer player);
        public override void ProcessMessage(StringBuilder message, IPlayer player, DiscordUser user, MessageSource source);
        private bool CanFilterMessage(MessageSource source);
        private void UFilterText(StringBuilder text);
        private string GetProfanityReplacement(string profanity);
    }

    public static class TemplateKeys
    {
        public static class Player
        {
            private const string Base;
            public static readonly TemplateKey Connecting;
            public static readonly TemplateKey Connected;
            public static readonly TemplateKey Disconnected;
        }

        public static class Server
        {
            private const string Base;
            public static readonly TemplateKey Online;
            public static readonly TemplateKey Shutdown;
            public static readonly TemplateKey Booting;
        }

        public static class Chat
        {
            private const string Base;
            public static readonly TemplateKey General;
            public static readonly TemplateKey Teams;
            public static readonly TemplateKey Cards;
            public static readonly TemplateKey Clan;
            public static class Clans
            {
                private const string Base;
                public static readonly TemplateKey Clan;
                public static readonly TemplateKey Alliance;
            }

            public static class AdminChat
            {
                private const string Base;
                public static readonly TemplateKey Message;
            }

        }

        public static class Error
        {
            private const string Base;
            public static readonly TemplateKey NotLinked;
            public static class AdminChat
            {
                private const string Base;
                public static readonly TemplateKey NotLinked;
                public static readonly TemplateKey NoPermission;
            }

            public static class BetterChatMute
            {
                private const string Base;
                public static readonly TemplateKey Muted;
            }

        }

    }

    public class AdminChatSettings
    {
        [JsonProperty("Enable AdminChat Plugin Support")]
        public bool Enabled { get; set; }
        [JsonProperty("Chat Channel ID")]
        public Snowflake ChatChannel { get; set; }
        [JsonProperty("Chat Prefix")]
        public string AdminChatPrefix { get; set; }
        [JsonProperty("Replace Discord Message With Bot")]
        public bool ReplaceWithBot { get; set; }
        public AdminChatSettings(AdminChatSettings settings);
    }

    public class AntiSpamSettings
    {
        [JsonProperty("Use AntiSpam On Player Names")]
        public bool PlayerName { get; set; }
        [JsonProperty("Use AntiSpam On Server Messages")]
        public bool ServerMessage { get; set; }
        [JsonProperty("Use AntiSpam On Chat Messages")]
        public bool DiscordMessage { get; set; }
        [JsonProperty("Use AntiSpam On Plugin Messages")]
        public bool PluginMessage { get; set; }
        public AntiSpamSettings(AntiSpamSettings settings);
    }

    public class BetterChatMuteSettings
    {
        [JsonProperty("Ignore Muted Players")]
        public bool IgnoreMuted { get; set; }
        [JsonProperty("Send Muted Notification")]
        public bool SendMutedNotification { get; set; }
        public BetterChatMuteSettings(BetterChatMuteSettings settings);
    }

    public class BetterChatSettings
    {
        [JsonProperty("Max BetterChat Tags To Show When Sent From Discord")]
        public byte ServerMaxTags { get; set; }
        [JsonProperty("Max BetterChat Tags To Show When Sent From Server")]
        public byte DiscordMaxTags { get; set; }
        public BetterChatSettings(BetterChatSettings settings);
    }

    public class ChatTranslatorSettings
    {
        [JsonProperty("Enable Chat Translator")]
        public bool Enabled { get; set; }
        [JsonProperty("Use ChatTranslator On Server Messages")]
        public bool ServerMessage { get; set; }
        [JsonProperty("Use ChatTranslator On Chat Messages")]
        public bool DiscordMessage { get; set; }
        [JsonProperty("Use ChatTranslator On Plugin Messages")]
        public bool PluginMessage { get; set; }
        [JsonProperty("Discord Server Chat Language")]
        public string DiscordServerLanguage { get; set; }
        public ChatTranslatorSettings(ChatTranslatorSettings settings);
    }

    public class ClansSettings
    {
        [JsonProperty("Clans Chat Channel ID")]
        public Snowflake ClansChatChannel { get; set; }
        [JsonProperty("Alliance Chat Channel ID")]
        public Snowflake AllianceChatChannel { get; set; }
        public ClansSettings(ClansSettings settings);
    }

    public class PluginSupport
    {
        [JsonProperty("AdminChat Settings")]
        public AdminChatSettings AdminChat { get; set; }
        [JsonProperty("AntiSpam Settings")]
        public AntiSpamSettings AntiSpam { get; set; }
        [JsonProperty("BetterChat Settings")]
        public BetterChatSettings BetterChat { get; set; }
        [JsonProperty("BetterChatMute Settings")]
        public BetterChatMuteSettings BetterChatMute { get; set; }
        [JsonProperty("ChatTranslator Settings")]
        public ChatTranslatorSettings ChatTranslator { get; set; }
        [JsonProperty("Clan Settings")]
        public ClansSettings Clans { get; set; }
        [JsonProperty("UFilter Settings")]
        public UFilterSettings UFilter { get; set; }
        public PluginSupport(PluginSupport settings);
    }

    public class UFilterSettings
    {
        [JsonProperty("Use UFilter On Player Names")]
        public bool PlayerNames { get; set; }
        [JsonProperty("Use UFilter On Server Messages")]
        public bool ServerMessage { get; set; }
        [JsonProperty("Use UFilter On Discord Messages")]
        public bool DiscordMessages { get; set; }
        [JsonProperty("Use UFilter On Plugin Messages")]
        public bool PluginMessages { get; set; }
        [JsonProperty("Replacement Character")]
        public char ReplacementCharacter { get; set; }
        public UFilterSettings(UFilterSettings settings);
    }

}

public class ChatSettings
{
    [JsonProperty("Chat Channel ID")]
    public Snowflake ChatChannel { get; set; }
    [JsonProperty("Replace Discord User Message With Bot Message")]
    public bool UseBotToDisplayChat { get; set; }
    [JsonProperty("Send Messages From Server Chat To Discord Channel")]
    public bool ServerToDiscord { get; set; }
    [JsonProperty("Send Messages From Discord Channel To Server Chat")]
    public bool DiscordToServer { get; set; }
    [JsonProperty("Add Discord Tag To In Game Messages When Sent From Discord")]
    public string DiscordTag { get; set; }
    [JsonProperty("Allow plugins to process Discord to Server Chat Messages")]
    public bool AllowPluginProcessing { get; set; }
    [JsonProperty("Text Replacements")]
    public Hash<string, string> TextReplacements { get; set; }
    [JsonProperty("Unlinked Settings")]
    public UnlinkedSettings UnlinkedSettings { get; set; }
    [JsonProperty("Message Filter Settings")]
    public MessageFilterSettings Filter { get; set; }
    public ChatSettings(ChatSettings settings);
}

public class MessageFilterSettings
{
    [JsonProperty("Ignore messages from users in this list (Discord ID)")]
    public List<Snowflake> IgnoreUsers { get; set; }
    [JsonProperty("Ignore messages from users in this role (Role ID)")]
    public List<Snowflake> IgnoreRoles { get; set; }
    [JsonProperty("Ignored Prefixes")]
    public List<string> IgnoredPrefixes { get; set; }
    public MessageFilterSettings(MessageFilterSettings settings);
    public bool IgnoreMessage(DiscordMessage message, GuildMember member);
    public bool IsIgnoredUser(DiscordUser user, GuildMember member);
    public bool IsRoleIgnoredMember(GuildMember member);
    public bool IsIgnoredPrefix(string content);
}

public class PlayerStateSettings
{
    [JsonProperty("Player State Channel ID")]
    public Snowflake PlayerStateChannel { get; set; }
    [JsonProperty("Show Admins")]
    public bool ShowAdmins { get; set; }
    [JsonProperty("Send Connecting Message")]
    public bool SendConnectingMessage { get; set; }
    [JsonProperty("Send Connected Message")]
    public bool SendConnectedMessage { get; set; }
    [JsonProperty("Send Disconnected Message")]
    public bool SendDisconnectedMessage { get; set; }
    public PlayerStateSettings(PlayerStateSettings settings);
}

public class PluginConfig
{
    [JsonProperty(PropertyName = "Discord Bot Token")]
    public string DiscordApiKey { get; set; }
    [JsonProperty("Chat Settings")]
    public ChatSettings ChatSettings { get; set; }
    [JsonProperty("Player State Settings")]
    public PlayerStateSettings PlayerStateSettings { get; set; }
    [JsonProperty("Server State Settings")]
    public ServerStateSettings ServerStateSettings { get; set; }
    [JsonProperty("Plugin Support")]
    public PluginSupport PluginSupport { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DiscordLogLevel.Info)]
    [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel ExtensionDebugging { get; set; }
}

public class ServerStateSettings
{
    [JsonProperty("Server State Channel ID")]
    public Snowflake ServerStateChannel { get; set; }
    [JsonProperty("Send Booting Message")]
    public bool SendBootingMessage { get; set; }
    [JsonProperty("Send Online Message")]
    public bool SendOnlineMessage { get; set; }
    [JsonProperty("Send Shutdown Message")]
    public bool SendShutdownMessage { get; set; }
    public ServerStateSettings(ServerStateSettings settings);
}

public class UnlinkedSettings
{
    [JsonProperty("Allow Unlinked Players To Chat With Server")]
    public bool AllowedUnlinked { get; set; }
    public UnlinkedSettings(UnlinkedSettings settings);
}

public class DiscordSendQueue
{
    private readonly StringBuilder _message;
    private Timer _sendTimer;
    private readonly DiscordChannel _channel;
    private readonly TemplateKey _templateId;
    private readonly Action _callback;
    private readonly PluginTimers _timer;
    public DiscordSendQueue(DiscordChannel channel, TemplateKey templateId, PluginTimers timers);
    public void QueueMessage(string message);
    public void Send();
}

public static class LangKeys
{
    public const string Root;
    public static class Discord
    {
        private const string Base;
        public static class Chat
        {
            private const string Base;
            public const string Server;
            public const string LinkedMessage;
            public const string UnlinkedMessage;
            public const string PlayerName;
        }

        public static class Team
        {
            private const string Base;
            public const string Message;
        }

        public static class Cards
        {
            private const string Base;
            public const string Message;
        }

        public static class Clans
        {
            private const string Base;
            public const string Message;
        }

        public static class Player
        {
            private const string Base;
            public const string Connecting;
            public const string Connected;
            public const string Disconnected;
        }

        public static class AdminChat
        {
            private const string Base;
            public const string ServerMessage;
            public const string DiscordMessage;
        }

        public static class PluginClans
        {
            private const string Base;
            public const string ClanMessage;
            public const string AllianceMessage;
        }

    }

    public static class Server
    {
        private const string Base;
        public const string LinkedMessage;
        public const string UnlinkedMessage;
    }

}

public static class Discord
{
    private const string Base;
    public static class Chat
    {
        private const string Base;
        public const string Server;
        public const string LinkedMessage;
        public const string UnlinkedMessage;
        public const string PlayerName;
    }

    public static class Team
    {
        private const string Base;
        public const string Message;
    }

    public static class Cards
    {
        private const string Base;
        public const string Message;
    }

    public static class Clans
    {
        private const string Base;
        public const string Message;
    }

    public static class Player
    {
        private const string Base;
        public const string Connecting;
        public const string Connected;
        public const string Disconnected;
    }

    public static class AdminChat
    {
        private const string Base;
        public const string ServerMessage;
        public const string DiscordMessage;
    }

    public static class PluginClans
    {
        private const string Base;
        public const string ClanMessage;
        public const string AllianceMessage;
    }

}

public static class Chat
{
    private const string Base;
    public const string Server;
    public const string LinkedMessage;
    public const string UnlinkedMessage;
    public const string PlayerName;
}

public static class Team
{
    private const string Base;
    public const string Message;
}

public static class Cards
{
    private const string Base;
    public const string Message;
}

public static class Clans
{
    private const string Base;
    public const string Message;
}

public static class Player
{
    private const string Base;
    public const string Connecting;
    public const string Connected;
    public const string Disconnected;
}

public static class AdminChat
{
    private const string Base;
    public const string ServerMessage;
    public const string DiscordMessage;
}

public static class PluginClans
{
    private const string Base;
    public const string ClanMessage;
    public const string AllianceMessage;
}

public static class Server
{
    private const string Base;
    public const string LinkedMessage;
    public const string UnlinkedMessage;
}

public class PlaceholderDataKeys
{
    public static readonly PlaceholderDataKey TemplateMessage;
    public static readonly PlaceholderDataKey PlayerMessage;
    public static readonly PlaceholderDataKey DisconnectReason;
}

public class PlaceholderKeys
{
    public static readonly PlaceholderKey TemplateMessage;
    public static readonly PlaceholderKey PlayerMessage;
    public static readonly PlaceholderKey DisconnectReason;
    public static readonly PlaceholderKey PlayerName;
    public static readonly PlaceholderKey DiscordTag;
}

public class AdminChatHandler : BasePluginHandler
{
    private readonly AdminChatSettings _settings;
    private readonly DiscordClient _client;
    public AdminChatHandler(DiscordClient client, DiscordChat chat, AdminChatSettings settings, Plugin plugin);
    public override bool CanSendMessage(string message, IPlayer player, DiscordUser user, MessageSource source, DiscordMessage sourceMessage);
    public override bool SendMessage(string message, IPlayer player, DiscordUser user, MessageSource source, DiscordMessage sourceMessage, PlaceholderData data);
    private bool IsAdminChatMessage(IPlayer player, string message);
}

public class AdminDeepCoverHandler : BasePluginHandler
{
    public AdminDeepCoverHandler(DiscordChat chat, Plugin plugin);
    public override bool CanSendMessage(string message, IPlayer player, DiscordUser user, MessageSource type, DiscordMessage sourceMessage);
}

public class AntiSpamHandler : BasePluginHandler
{
    private readonly AntiSpamSettings _settings;
    public AntiSpamHandler(DiscordChat chat, AntiSpamSettings settings, Plugin plugin);
    public override void ProcessPlayerName(StringBuilder name, IPlayer player);
    public override void ProcessMessage(StringBuilder message, IPlayer player, DiscordUser user, MessageSource source);
    private bool CanFilterMessage(MessageSource source);
}

public abstract class BasePluginHandler : IPluginHandler
{
    protected readonly DiscordChat Chat;
    protected readonly Plugin Plugin;
    private readonly string _pluginName;
    protected BasePluginHandler(DiscordChat chat, Plugin plugin);
    public virtual bool CanSendMessage(string message, IPlayer player, DiscordUser user, MessageSource source, DiscordMessage sourceMessage);
    public virtual void ProcessPlayerName(StringBuilder name, IPlayer player);
    public virtual bool HasCallbackMessage();
    public virtual void ProcessCallbackMessage(string message, IPlayer player, DiscordUser user, MessageSource source, Action<string> callback);
    public virtual void ProcessMessage(StringBuilder message, IPlayer player, DiscordUser user, MessageSource source);
    public virtual bool SendMessage(string message, IPlayer player, DiscordUser user, MessageSource source, DiscordMessage sourceMessage, PlaceholderData data);
    public string GetPluginName();
}

public class BetterChatHandler : BasePluginHandler
{
    private readonly BetterChatSettings _settings;
    private readonly Regex _rustRegex;
    public BetterChatHandler(DiscordChat chat, BetterChatSettings settings, Plugin plugin);
    public override void ProcessPlayerName(StringBuilder name, IPlayer player);
}

public class BetterChatMuteHandler : BasePluginHandler
{
    private readonly BetterChatMuteSettings _settings;
    public BetterChatMuteHandler(DiscordChat chat, BetterChatMuteSettings settings, Plugin plugin);
    public override bool CanSendMessage(string message, IPlayer player, DiscordUser user, MessageSource source, DiscordMessage sourceMessage);
}

public class DiscordChatHandler : BasePluginHandler
{
    private readonly ChatSettings _settings;
    private readonly IServer _server;
    public DiscordChatHandler(DiscordChat chat, ChatSettings settings, Plugin plugin, IServer server);
    public override bool CanSendMessage(string message, IPlayer player, DiscordUser user, MessageSource source, DiscordMessage sourceMessage);
    public override bool SendMessage(string message, IPlayer player, DiscordUser user, MessageSource source, DiscordMessage sourceMessage, PlaceholderData data);
    public void SendLinkedToServer(IPlayer player, string message, PlaceholderData placeholders, MessageSource source);
    public void SendUnlinkedToServer(PlaceholderData placeholders);
    public override void ProcessMessage(StringBuilder message, IPlayer player, DiscordUser user, MessageSource source);
}

public class TranslationApiHandler : BasePluginHandler
{
    private readonly ChatTranslatorSettings _settings;
    public TranslationApiHandler(DiscordChat chat, ChatTranslatorSettings settings, Plugin plugin);
    public override bool HasCallbackMessage();
    public override void ProcessCallbackMessage(string message, IPlayer player, DiscordUser user, MessageSource source, Action<string> callback);
    public bool CanChatTranslatorSource(MessageSource source);
}

public class UFilterHandler : BasePluginHandler
{
    private readonly UFilterSettings _settings;
    private readonly List<string> _replacements;
    public UFilterHandler(DiscordChat chat, UFilterSettings settings, Plugin plugin);
    public override void ProcessPlayerName(StringBuilder name, IPlayer player);
    public override void ProcessMessage(StringBuilder message, IPlayer player, DiscordUser user, MessageSource source);
    private bool CanFilterMessage(MessageSource source);
    private void UFilterText(StringBuilder text);
    private string GetProfanityReplacement(string profanity);
}

public static class TemplateKeys
{
    public static class Player
    {
        private const string Base;
        public static readonly TemplateKey Connecting;
        public static readonly TemplateKey Connected;
        public static readonly TemplateKey Disconnected;
    }

    public static class Server
    {
        private const string Base;
        public static readonly TemplateKey Online;
        public static readonly TemplateKey Shutdown;
        public static readonly TemplateKey Booting;
    }

    public static class Chat
    {
        private const string Base;
        public static readonly TemplateKey General;
        public static readonly TemplateKey Teams;
        public static readonly TemplateKey Cards;
        public static readonly TemplateKey Clan;
        public static class Clans
        {
            private const string Base;
            public static readonly TemplateKey Clan;
            public static readonly TemplateKey Alliance;
        }

        public static class AdminChat
        {
            private const string Base;
            public static readonly TemplateKey Message;
        }

    }

    public static class Error
    {
        private const string Base;
        public static readonly TemplateKey NotLinked;
        public static class AdminChat
        {
            private const string Base;
            public static readonly TemplateKey NotLinked;
            public static readonly TemplateKey NoPermission;
        }

        public static class BetterChatMute
        {
            private const string Base;
            public static readonly TemplateKey Muted;
        }

    }

}

public static class Player
{
    private const string Base;
    public static readonly TemplateKey Connecting;
    public static readonly TemplateKey Connected;
    public static readonly TemplateKey Disconnected;
}

public static class Server
{
    private const string Base;
    public static readonly TemplateKey Online;
    public static readonly TemplateKey Shutdown;
    public static readonly TemplateKey Booting;
}

public static class Chat
{
    private const string Base;
    public static readonly TemplateKey General;
    public static readonly TemplateKey Teams;
    public static readonly TemplateKey Cards;
    public static readonly TemplateKey Clan;
    public static class Clans
    {
        private const string Base;
        public static readonly TemplateKey Clan;
        public static readonly TemplateKey Alliance;
    }

    public static class AdminChat
    {
        private const string Base;
        public static readonly TemplateKey Message;
    }

}

public static class Clans
{
    private const string Base;
    public static readonly TemplateKey Clan;
    public static readonly TemplateKey Alliance;
}

public static class AdminChat
{
    private const string Base;
    public static readonly TemplateKey Message;
}

public static class Error
{
    private const string Base;
    public static readonly TemplateKey NotLinked;
    public static class AdminChat
    {
        private const string Base;
        public static readonly TemplateKey NotLinked;
        public static readonly TemplateKey NoPermission;
    }

    public static class BetterChatMute
    {
        private const string Base;
        public static readonly TemplateKey Muted;
    }

}

public static class AdminChat
{
    private const string Base;
    public static readonly TemplateKey NotLinked;
    public static readonly TemplateKey NoPermission;
}

public static class BetterChatMute
{
    private const string Base;
    public static readonly TemplateKey Muted;
}

public class AdminChatSettings
{
    [JsonProperty("Enable AdminChat Plugin Support")]
    public bool Enabled { get; set; }
    [JsonProperty("Chat Channel ID")]
    public Snowflake ChatChannel { get; set; }
    [JsonProperty("Chat Prefix")]
    public string AdminChatPrefix { get; set; }
    [JsonProperty("Replace Discord Message With Bot")]
    public bool ReplaceWithBot { get; set; }
    public AdminChatSettings(AdminChatSettings settings);
}

public class AntiSpamSettings
{
    [JsonProperty("Use AntiSpam On Player Names")]
    public bool PlayerName { get; set; }
    [JsonProperty("Use AntiSpam On Server Messages")]
    public bool ServerMessage { get; set; }
    [JsonProperty("Use AntiSpam On Chat Messages")]
    public bool DiscordMessage { get; set; }
    [JsonProperty("Use AntiSpam On Plugin Messages")]
    public bool PluginMessage { get; set; }
    public AntiSpamSettings(AntiSpamSettings settings);
}

public class BetterChatMuteSettings
{
    [JsonProperty("Ignore Muted Players")]
    public bool IgnoreMuted { get; set; }
    [JsonProperty("Send Muted Notification")]
    public bool SendMutedNotification { get; set; }
    public BetterChatMuteSettings(BetterChatMuteSettings settings);
}

public class BetterChatSettings
{
    [JsonProperty("Max BetterChat Tags To Show When Sent From Discord")]
    public byte ServerMaxTags { get; set; }
    [JsonProperty("Max BetterChat Tags To Show When Sent From Server")]
    public byte DiscordMaxTags { get; set; }
    public BetterChatSettings(BetterChatSettings settings);
}

public class ChatTranslatorSettings
{
    [JsonProperty("Enable Chat Translator")]
    public bool Enabled { get; set; }
    [JsonProperty("Use ChatTranslator On Server Messages")]
    public bool ServerMessage { get; set; }
    [JsonProperty("Use ChatTranslator On Chat Messages")]
    public bool DiscordMessage { get; set; }
    [JsonProperty("Use ChatTranslator On Plugin Messages")]
    public bool PluginMessage { get; set; }
    [JsonProperty("Discord Server Chat Language")]
    public string DiscordServerLanguage { get; set; }
    public ChatTranslatorSettings(ChatTranslatorSettings settings);
}

public class ClansSettings
{
    [JsonProperty("Clans Chat Channel ID")]
    public Snowflake ClansChatChannel { get; set; }
    [JsonProperty("Alliance Chat Channel ID")]
    public Snowflake AllianceChatChannel { get; set; }
    public ClansSettings(ClansSettings settings);
}

public class PluginSupport
{
    [JsonProperty("AdminChat Settings")]
    public AdminChatSettings AdminChat { get; set; }
    [JsonProperty("AntiSpam Settings")]
    public AntiSpamSettings AntiSpam { get; set; }
    [JsonProperty("BetterChat Settings")]
    public BetterChatSettings BetterChat { get; set; }
    [JsonProperty("BetterChatMute Settings")]
    public BetterChatMuteSettings BetterChatMute { get; set; }
    [JsonProperty("ChatTranslator Settings")]
    public ChatTranslatorSettings ChatTranslator { get; set; }
    [JsonProperty("Clan Settings")]
    public ClansSettings Clans { get; set; }
    [JsonProperty("UFilter Settings")]
    public UFilterSettings UFilter { get; set; }
    public PluginSupport(PluginSupport settings);
}

public class UFilterSettings
{
    [JsonProperty("Use UFilter On Player Names")]
    public bool PlayerNames { get; set; }
    [JsonProperty("Use UFilter On Server Messages")]
    public bool ServerMessage { get; set; }
    [JsonProperty("Use UFilter On Discord Messages")]
    public bool DiscordMessages { get; set; }
    [JsonProperty("Use UFilter On Plugin Messages")]
    public bool PluginMessages { get; set; }
    [JsonProperty("Replacement Character")]
    public char ReplacementCharacter { get; set; }
    public UFilterSettings(UFilterSettings settings);
}


```

---

## DiscordCommands by MJSU - Allows players with permission to run commands on the server using Discord

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord.Clients;
using Oxide.Ext.Discord.Connections;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Interfaces;
using Oxide.Ext.Discord.Libraries;
using Oxide.Ext.Discord.Logging;

Oxide.Plugins
[Info("Discord Commands", "MJSU", "2.1.1")]
[Description("Allows using discord to execute commands")]
internal class DiscordCommands : CovalencePlugin, IDiscordPlugin
{
    public DiscordClient Client { get; set; }
    private PluginConfig _pluginConfig;
    private const string UsePermission;
    private readonly DiscordCommand _dcCommands;
    private readonly BotConnection _discordSettings;
    private readonly Hash<Snowflake, StringBuilder> _playerLogs;
    private DiscordGuild _guild;
    private bool _logActive;
    private void Init();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void Unload();
    [HookMethod(DiscordExtHooks.OnDiscordGatewayReady)]
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
    private void ExecCommand(DiscordMessage message, string cmd, string[] args);
    public bool HasCommandPermissions(DiscordMessage message);
    private bool CanRunCommand(DiscordMessage message, string command, IPlayer player, GuildMember member);
    private static bool IsPlayerAllowed(IPlayer player, GuildMember member, RestrictionSettings restriction);
    private void RunCommand(DiscordMessage message, string command, string[] commandArgs, IPlayer player, GuildMember member, string commandString);
    private void HandleCommandLog(string message, string stackTrace, UnityEngine.LogType type);
    public void RegisterDiscordLangCommand(string command, string langKey, bool direct, bool guild, List<Snowflake> allowedChannels);
    public string Lang(string key, IPlayer player);
    public string Lang(string key, IPlayer player, object[] args);
    private class PluginConfig
    {
        [DefaultValue("")]
        [JsonProperty(PropertyName = "Discord Bot Token")]
        public string DiscordApiKey { get; set; }
        [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
        public Snowflake GuildId { get; set; }
        [JsonProperty(PropertyName = "Command Settings")]
        public CommandSettings CommandSettings { get; set; }
        [JsonProperty(PropertyName = "Log Settings")]
        public LogSettings LogSettings { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DiscordLogLevel.Info)]
        [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel ExtensionDebugging { get; set; }
    }

    private class LogSettings
    {
        [JsonProperty(PropertyName = "Log command usage in server console")]
        public bool LogToConsole { get; set; }
        [JsonProperty(PropertyName = "Command Usage Logging Channel ID")]
        public Snowflake LoggingChannel { get; set; }
        [JsonProperty(PropertyName = "Display Server Log Messages to user after running command")]
        public bool DisplayServerLog { get; set; }
        [JsonProperty(PropertyName = "Display Server Log Messages Duration (Seconds)")]
        public float DisplayServerLogDuration { get; set; }
        public LogSettings(LogSettings settings);
    }

    private class CommandSettings
    {
        [JsonProperty(PropertyName = "Allow Discord Commands In Direct Messages")]
        public bool AllowInDm { get; set; }
        [JsonProperty(PropertyName = "Allow Discord Commands In Guild")]
        public bool AllowInGuild { get; set; }
        [JsonProperty(PropertyName = "Allow Guild Commands Only In The Following Guild Channel Or Category (Channel ID Or Category ID)")]
        public List<Snowflake> AllowedChannels { get; set; }
        [JsonProperty(PropertyName = "Allow Commands for members having role (Role ID)")]
        public List<Snowflake> AllowedRoles { get; set; }
        public CommandRestrictions Restrictions { get; set; }
        public CommandSettings(CommandSettings settings);
    }

    private class CommandRestrictions
    {
        [JsonProperty(PropertyName = "Enable Command Restrictions")]
        public bool EnableRestrictions { get; set; }
        [JsonProperty(PropertyName = "Blacklist = listed commands cannot be used without permission, Whitelist = Cannot use any commands unless listed and have permission")]
        [JsonConverter(typeof(StringEnumConverter))]
        public RestrictionMode RestrictionMode { get; set; }
        [JsonProperty(PropertyName = "Command Restrictions")]
        public Hash<string, RestrictionSettings> Restrictions { get; set; }
        public CommandRestrictions(CommandRestrictions settings);
    }

    private class RestrictionSettings
    {
        [JsonProperty(PropertyName = "Allowed Discord Roles")]
        public List<Snowflake> AllowedRoles { get; set; }
        [JsonProperty(PropertyName = "Allowed Server Groups")]
        public List<string> AllowedGroups { get; set; }
    }

    private static class LangKeys
    {
        public const string CommandInfoText;
        public const string CommandHelpText;
        public const string RanCommand;
        public const string CommandLogging;
        public const string ExecCommand;
        public const string NoPermission;
        public const string Blacklisted;
        public const string WhiteListedNotAdded;
        public const string WhiteListedNoPermission;
    }

}

private class PluginConfig
{
    [DefaultValue("")]
    [JsonProperty(PropertyName = "Discord Bot Token")]
    public string DiscordApiKey { get; set; }
    [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
    public Snowflake GuildId { get; set; }
    [JsonProperty(PropertyName = "Command Settings")]
    public CommandSettings CommandSettings { get; set; }
    [JsonProperty(PropertyName = "Log Settings")]
    public LogSettings LogSettings { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DiscordLogLevel.Info)]
    [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel ExtensionDebugging { get; set; }
}

private class LogSettings
{
    [JsonProperty(PropertyName = "Log command usage in server console")]
    public bool LogToConsole { get; set; }
    [JsonProperty(PropertyName = "Command Usage Logging Channel ID")]
    public Snowflake LoggingChannel { get; set; }
    [JsonProperty(PropertyName = "Display Server Log Messages to user after running command")]
    public bool DisplayServerLog { get; set; }
    [JsonProperty(PropertyName = "Display Server Log Messages Duration (Seconds)")]
    public float DisplayServerLogDuration { get; set; }
    public LogSettings(LogSettings settings);
}

private class CommandSettings
{
    [JsonProperty(PropertyName = "Allow Discord Commands In Direct Messages")]
    public bool AllowInDm { get; set; }
    [JsonProperty(PropertyName = "Allow Discord Commands In Guild")]
    public bool AllowInGuild { get; set; }
    [JsonProperty(PropertyName = "Allow Guild Commands Only In The Following Guild Channel Or Category (Channel ID Or Category ID)")]
    public List<Snowflake> AllowedChannels { get; set; }
    [JsonProperty(PropertyName = "Allow Commands for members having role (Role ID)")]
    public List<Snowflake> AllowedRoles { get; set; }
    public CommandRestrictions Restrictions { get; set; }
    public CommandSettings(CommandSettings settings);
}

private class CommandRestrictions
{
    [JsonProperty(PropertyName = "Enable Command Restrictions")]
    public bool EnableRestrictions { get; set; }
    [JsonProperty(PropertyName = "Blacklist = listed commands cannot be used without permission, Whitelist = Cannot use any commands unless listed and have permission")]
    [JsonConverter(typeof(StringEnumConverter))]
    public RestrictionMode RestrictionMode { get; set; }
    [JsonProperty(PropertyName = "Command Restrictions")]
    public Hash<string, RestrictionSettings> Restrictions { get; set; }
    public CommandRestrictions(CommandRestrictions settings);
}

private class RestrictionSettings
{
    [JsonProperty(PropertyName = "Allowed Discord Roles")]
    public List<Snowflake> AllowedRoles { get; set; }
    [JsonProperty(PropertyName = "Allowed Server Groups")]
    public List<string> AllowedGroups { get; set; }
}

private static class LangKeys
{
    public const string CommandInfoText;
    public const string CommandHelpText;
    public const string RanCommand;
    public const string CommandLogging;
    public const string ExecCommand;
    public const string NoPermission;
    public const string Blacklisted;
    public const string WhiteListedNotAdded;
    public const string WhiteListedNoPermission;
}


```

---

## DiscordConnect by misticos - Discord account connection with API

```csharp
using System;
using System.Collections.Generic;
using System.Text;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Ext.Discord;
using Oxide.Ext.Discord.Attributes;
using Oxide.Ext.Discord.DiscordObjects;

Oxide.Plugins
[Info("Discord Connect", "Iv Misticos", "1.0.11")]
[Description("Discord account connection with API")]
public class DiscordConnect : CovalencePlugin
{
    [DiscordClient]
    private DiscordClient _client;
    private static List<KeyInfo> _keys;
    private static List<PlayerData> _data;
    private static DiscordConnect _ins;
    private static Time _time;
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Discord Bot Token")]
        public string Token;
        [JsonProperty(PropertyName = "Channel ID For Authentication Log")]
        public string AuthLogChannel;
        [JsonProperty(PropertyName = "Enable Bot Status")]
        public bool EnableStatus;
        [JsonProperty(PropertyName = "Bot Status")]
        public string Status;
        [JsonProperty(PropertyName = "Group To Assign Once Connected")]
        public string GroupConnected;
        [JsonProperty(PropertyName = "Group To Revoke Once Left")]
        public string GroupLeft;
        [JsonProperty(PropertyName = "Delete Data On Discord Leave")]
        public bool DeleteData;
        [JsonProperty(PropertyName = "Allow Data Overwrite")]
        public bool OverwriteData;
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string Prefix;
        [JsonProperty(PropertyName = "Auth Command")]
        public string Command;
        [JsonProperty(PropertyName = "Code Lifetime")]
        public string CodeLifetime;
        [JsonIgnore]
        public uint ParsedCodeLifetime;
        [JsonProperty(PropertyName = "Code Length")]
        public int CodeLength;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private class KeyInfo
    {
        public IPlayer Player;
        public string Key;
        public uint ValidUntil;
        public static KeyInfo FindByID(string userId);
        public static KeyInfo FindByKey(string key);
        public void ExpireMessage();
        public void ResetValidUntil();
    }

    private class PlayerData
    {
        public string GameId;
        public string DiscordId;
        public static PlayerData FindByGame(string id);
        public static PlayerData FindByDiscord(string id);
    }

    private void SaveData();
    private void LoadData();
    protected override void LoadDefaultMessages();
    private void Init();
    private void OnServerSave();
    private void Unload();
    private void Discord_MemberRemoved(GuildMember member);
    private void Discord_MessageCreate(Message message);
    private void CommandAuth(IPlayer player, string command, string[] args);
    private string GetDiscordOf(string id);
    private string GetGameOf(string id);
    private void DoExpiration();
    private string GenerateCode();
    private static string GetMsg(string key, string userId);
    private static readonly Regex RegexStringTime;
    private static bool ConvertToSeconds(string time, uint seconds);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Discord Bot Token")]
    public string Token;
    [JsonProperty(PropertyName = "Channel ID For Authentication Log")]
    public string AuthLogChannel;
    [JsonProperty(PropertyName = "Enable Bot Status")]
    public bool EnableStatus;
    [JsonProperty(PropertyName = "Bot Status")]
    public string Status;
    [JsonProperty(PropertyName = "Group To Assign Once Connected")]
    public string GroupConnected;
    [JsonProperty(PropertyName = "Group To Revoke Once Left")]
    public string GroupLeft;
    [JsonProperty(PropertyName = "Delete Data On Discord Leave")]
    public bool DeleteData;
    [JsonProperty(PropertyName = "Allow Data Overwrite")]
    public bool OverwriteData;
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string Prefix;
    [JsonProperty(PropertyName = "Auth Command")]
    public string Command;
    [JsonProperty(PropertyName = "Code Lifetime")]
    public string CodeLifetime;
    [JsonIgnore]
    public uint ParsedCodeLifetime;
    [JsonProperty(PropertyName = "Code Length")]
    public int CodeLength;
}

private class KeyInfo
{
    public IPlayer Player;
    public string Key;
    public uint ValidUntil;
    public static KeyInfo FindByID(string userId);
    public static KeyInfo FindByKey(string key);
    public void ExpireMessage();
    public void ResetValidUntil();
}

private class PlayerData
{
    public string GameId;
    public string DiscordId;
    public static PlayerData FindByGame(string id);
    public static PlayerData FindByDiscord(string id);
}


```

---

## DiscordConnectCommands by misticos - Execute commands on Discord Connect events

```csharp
using System;
using System.Collections.Generic;
using System.Text;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Discord Connect Commands", "Iv Misticos", "1.0.4")]
[Description("Execute commands on Discord Connect events")]
 class DiscordConnectCommands : CovalencePlugin
{
    private StringBuilder _builder;
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Commands On Connect",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> CommandsConnect;
        [JsonProperty(PropertyName = "Commands On Overwrite",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> CommandsOverwrite;
        [JsonProperty(PropertyName = "Commands On Server Leave",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> CommandsLeave;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private string FormatCommand(string command, string gameId, string discordId);
    private string FormatCommand(string command, string oldGameId, string newGameId, string oldDiscordId, string newDiscordId);
    private void ExecuteCommand(string command);
    private void OnDiscordAuthenticate(string gameId, string discordId);
    private void OnDiscordAuthOverwrite(string oldGameId, string newGameId, string oldDiscordId, string newDiscordId);
    private void OnDiscordAuthLeave(string gameId, string discordId);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Commands On Connect",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> CommandsConnect;
    [JsonProperty(PropertyName = "Commands On Overwrite",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> CommandsOverwrite;
    [JsonProperty(PropertyName = "Commands On Server Leave",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> CommandsLeave;
}


```

---

## DiscordCore by MJSU - Creates a link between a player and a Discord server

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord.Attributes;
using Oxide.Ext.Discord.Builders;
using Oxide.Ext.Discord.Cache;
using Oxide.Ext.Discord.Clients;
using Oxide.Ext.Discord.Connections;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Extensions;
using Oxide.Ext.Discord.Helpers;
using Oxide.Ext.Discord.Interfaces;
using Oxide.Ext.Discord.Libraries;
using Oxide.Ext.Discord.Logging;
using Oxide.Ext.Discord.Types;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using System.Text;

Oxide.Plugins
[Info("Discord Core", "MJSU", "3.0.2")]
[Description("Creates a link between a player and discord")]
public partial class DiscordCore : CovalencePlugin, IDiscordPlugin, IDiscordLink
{
    public DiscordClient Client { get; set; }
    private PluginData _pluginData;
    private PluginConfig _pluginConfig;
    private DiscordUser _bot;
    public DiscordGuild Guild;
    private readonly BotConnection _discordSettings;
    private readonly DiscordLink _link;
    private readonly DiscordMessageTemplates _templates;
    private readonly DiscordPlaceholders _placeholders;
    private readonly DiscordLocales _lang;
    private readonly DiscordCommandLocalizations _local;
    private readonly StringBuilder _sb;
    private JoinHandler _joinHandler;
    private JoinBanHandler _banHandler;
    private LinkHandler _linkHandler;
    private const string UsePermission;
    private static readonly DiscordColor AccentColor;
    private static readonly DiscordColor Success;
    private static readonly DiscordColor Danger;
    private const string PlayerArg;
    private const string UserArg;
    private const string CodeArg;
    private DiscordApplicationCommand _appCommand;
    private string _allowedChannels;
    public static DiscordCore Instance;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    public PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    public void ValidateGroups();
    private void Unload();
    public void Chat(IPlayer player, string key, PlaceholderData data);
    public void BroadcastMessage(string key, PlaceholderData data);
    public string Lang(string key, IPlayer player, PlaceholderData data);
    public void RegisterChatLangCommand(string command, string langKey);
    protected override void LoadDefaultMessages();
    private void DiscordCoreChatCommand(IPlayer player, string cmd, string[] args);
    public void DisplayHelp(IPlayer player);
    public void HandleServerCodeJoin(IPlayer player);
    public void HandleServerUserJoin(IPlayer player, string[] args);
    public void HandleChatJoinUserResults(IPlayer player, List<GuildMember> members, string userName, string discriminator);
    public void UserSearchMatchFound(IPlayer player, DiscordUser user);
    public void HandleServerLeave(IPlayer player);
    public void HandleUserJoinAccept(IPlayer player);
    public void HandleUserJoinDecline(IPlayer player);
    public void HandleServerCompleteLink(IPlayer player, string[] args);
    private void OnUserConnected(IPlayer player);
    [HookMethod(DiscordExtHooks.OnDiscordGatewayReady)]
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
    [HookMethod(DiscordExtHooks.OnDiscordBotFullyLoaded)]
    private void OnDiscordBotFullyLoaded();
    [HookMethod(DiscordExtHooks.OnDiscordGuildMemberAdded)]
    private void OnDiscordGuildMemberAdded(GuildMemberAddedEvent member, DiscordGuild guild);
    [HookMethod(DiscordExtHooks.OnDiscordGuildMemberRemoved)]
    private void OnDiscordGuildMemberRemoved(GuildMemberRemovedEvent member, DiscordGuild guild);
    [HookMethod(DiscordExtHooks.OnDiscordGuildMemberRoleAdded)]
    private void OnDiscordGuildMemberRoleAdded(GuildMember member, Snowflake roleId, DiscordGuild guild);
    public void RegisterUserApplicationCommands();
    public void AddUserCodeCommand(ApplicationCommandBuilder builder);
    public void AddUserUserCommand(ApplicationCommandBuilder builder);
    public void AddUserLeaveCommand(ApplicationCommandBuilder builder);
    public void AddUserLinkCommand(ApplicationCommandBuilder builder);
    public void CreateAllowedChannels(DiscordApplicationCommand command);
    public void CreateAllowedChannels(GuildCommandPermissions permissions);
    [DiscordApplicationCommand(UserAppCommands.Command, UserAppCommands.CodeCommand)]
    private void DiscordCodeCommand(DiscordInteraction interaction, InteractionDataParsed parsed);
    [DiscordApplicationCommand(UserAppCommands.Command, UserAppCommands.UserCommand)]
    private void DiscordUserCommand(DiscordInteraction interaction, InteractionDataParsed parsed);
    [DiscordAutoCompleteCommand(UserAppCommands.Command, PlayerArg, UserAppCommands.UserCommand)]
    private void HandleNameAutoComplete(DiscordInteraction interaction, InteractionDataOption focused);
    [DiscordApplicationCommand(UserAppCommands.Command, UserAppCommands.LeaveCommand)]
    private void DiscordLeaveCommand(DiscordInteraction interaction, InteractionDataParsed parsed);
    [DiscordApplicationCommand(UserAppCommands.Command, UserAppCommands.LinkCommand)]
    private void DiscordLinkCommand(DiscordInteraction interaction, InteractionDataParsed parsed);
    private const string WelcomeMessageLinkAccountsButtonId;
    private const string GuildWelcomeMessageLinkAccountsButtonId;
    private const string AcceptLinkButtonId;
    private const string DeclineLinkButtonId;
    [DiscordMessageComponentCommand(WelcomeMessageLinkAccountsButtonId)]
    private void HandleWelcomeMessageLinkAccounts(DiscordInteraction interaction);
    [DiscordMessageComponentCommand(GuildWelcomeMessageLinkAccountsButtonId)]
    private void HandleGuildWelcomeMessageLinkAccounts(DiscordInteraction interaction);
    [DiscordMessageComponentCommand(AcceptLinkButtonId)]
    private void HandleAcceptLinkButton(DiscordInteraction interaction);
    [DiscordMessageComponentCommand(DeclineLinkButtonId)]
    private void HandleDeclineLinkButton(DiscordInteraction interaction);
    public void RegisterAdminApplicationCommands();
    public void AddAdminLinkCommand(ApplicationCommandBuilder builder);
    public void AddAdminUnlinkCommand(ApplicationCommandBuilder builder);
    public void AddAdminSearchGroupCommand(ApplicationCommandBuilder builder);
    public void AddAdminSearchByPlayerCommand(ApplicationCommandGroupBuilder builder);
    public void AddAdminSearchByUserCommand(ApplicationCommandGroupBuilder builder);
    public void AddAdminUnbanGroupCommand(ApplicationCommandBuilder builder);
    public void AddAdminUnbanByPlayerCommand(ApplicationCommandGroupBuilder builder);
    public void AddAdminUnbanByUserCommand(ApplicationCommandGroupBuilder builder);
    [DiscordApplicationCommand(AdminAppCommands.Command, AdminAppCommands.LinkCommand)]
    private void DiscordAdminLinkCommand(DiscordInteraction interaction, InteractionDataParsed parsed);
    [DiscordApplicationCommand(AdminAppCommands.Command, AdminAppCommands.UnlinkCommand)]
    private void DiscordAdminUnlinkCommand(DiscordInteraction interaction, InteractionDataParsed parsed);
    [DiscordApplicationCommand(AdminAppCommands.Command, AdminAppCommands.PlayerCommand, AdminAppCommands.SearchCommand)]
    private void DiscordAdminSearchByPlayer(DiscordInteraction interaction, InteractionDataParsed parsed);
    [DiscordApplicationCommand(AdminAppCommands.Command, AdminAppCommands.UserCommand, AdminAppCommands.Unban)]
    private void DiscordAdminUnbanByUser(DiscordInteraction interaction, InteractionDataParsed parsed);
    [DiscordApplicationCommand(AdminAppCommands.Command, AdminAppCommands.PlayerCommand, AdminAppCommands.Unban)]
    private void DiscordAdminUnbanByPlayer(DiscordInteraction interaction, InteractionDataParsed parsed);
    [DiscordApplicationCommand(AdminAppCommands.Command, AdminAppCommands.UserCommand, AdminAppCommands.SearchCommand)]
    private void DiscordAdminSearchByUser(DiscordInteraction interaction, InteractionDataParsed parsed);
    [DiscordAutoCompleteCommand(AdminAppCommands.Command, PlayerArg, AdminAppCommands.PlayerCommand, AdminAppCommands.Unban)]
    [DiscordAutoCompleteCommand(AdminAppCommands.Command, PlayerArg, AdminAppCommands.PlayerCommand, AdminAppCommands.SearchCommand)]
    [DiscordAutoCompleteCommand(AdminAppCommands.Command, PlayerArg, AdminAppCommands.LinkCommand)]
    [DiscordAutoCompleteCommand(AdminAppCommands.Command, PlayerArg, AdminAppCommands.UnlinkCommand)]
    private void HandleAdminNameAutoComplete(DiscordInteraction interaction, InteractionDataOption focused);
    private const string AcceptEmoji;
    private const string DeclineEmoji;
    public void RegisterTemplates();
    public void RegisterAnnouncements();
    public void RegisterWelcomeMessages();
    public void RegisterCommandMessages();
    public void RegisterAdminCommandMessages();
    public void RegisterLinkMessages();
    public void RegisterBanMessages();
    public void RegisterJoinMessages();
    public void RegisterErrorMessages();
    public DiscordMessageTemplate CreateTemplateEmbed(string description, DiscordColor color);
    public void SendTemplateMessage(TemplateKey templateName, DiscordInteraction interaction, PlaceholderData placeholders);
    public void SendTemplateMessage(TemplateKey templateName, DiscordUser user, IPlayer player, PlaceholderData placeholders);
    public void SendGlobalTemplateMessage(TemplateKey templateName, DiscordUser user, IPlayer player, PlaceholderData placeholders);
    public IPromise<DiscordMessage> SendGlobalTemplateMessage(TemplateKey templateName, Snowflake channelId, DiscordUser user, IPlayer player, PlaceholderData placeholders);
    public void UpdateGuildTemplateMessage(TemplateKey templateName, DiscordMessage message, PlaceholderData placeholders);
    private void AddDefaultPlaceholders(PlaceholderData placeholders, DiscordUser user, IPlayer player);
    public void RegisterPlaceholders();
    public string LangPlaceholder(string key, PlaceholderData data);
    public PlaceholderData GetDefault();
    public PlaceholderData GetDefault(IPlayer player);
    public PlaceholderData GetDefault(DiscordUser user);
    public PlaceholderData GetDefault(IPlayer player, DiscordUser user);
    public IDictionary<PlayerId, Snowflake> GetPlayerIdToDiscordIds();
    private string API_Link(IPlayer player, DiscordUser user);
    private string API_Unlink(IPlayer player, DiscordUser user);
    public void SaveData();
    public void SetupGuildWelcomeMessage();
    private void CreateGuildWelcomeMessage(GuildMessageSettings settings);
    public static class ApiErrorCodes
    {
        public const string PlayerIsLinked;
        public const string PlayerIsNotLinked;
        public const string UserIsLinked;
        public const string UserIsNotLinked;
    }

    public static class AdminAppCommands
    {
        public const string Command;
        public const string LinkCommand;
        public const string UnlinkCommand;
        public const string SearchCommand;
        public const string PlayerCommand;
        public const string UserCommand;
        public const string Unban;
    }

    public static class UserAppCommands
    {
        public const string Command;
        public const string CodeCommand;
        public const string UserCommand;
        public const string LeaveCommand;
        public const string LinkCommand;
    }

    public class GuildMessageSettings
    {
        [JsonProperty(PropertyName = "Enable Guild Link Message")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Message Channel ID")]
        public Snowflake ChannelId { get; set; }
        public GuildMessageSettings(GuildMessageSettings settings);
    }

    public class InactiveSettings
    {
        [JsonProperty(PropertyName = "Automatically Unlink Inactive Players")]
        public bool UnlinkInactive { get; set; }
        [JsonProperty(PropertyName = "Player Considered Inactive After X (Days)")]
        public float UnlinkInactiveDays { get; set; }
        [JsonProperty(PropertyName = "Automatically Relink Inactive Players On Game Server Join")]
        public bool AutoRelinkInactive { get; set; }
        public InactiveSettings(InactiveSettings settings);
    }

    public class LinkBanSettings
    {
        [JsonProperty(PropertyName = "Enable Link Ban")]
        public bool EnableLinkBanning { get; set; }
        [JsonProperty(PropertyName = "Ban Announcement Channel ID")]
        public Snowflake BanAnnouncementChannel { get; set; }
        [JsonProperty(PropertyName = "Ban Link After X Join Declines")]
        public int BanDeclineAmount { get; set; }
        [JsonProperty(PropertyName = "Ban Duration (Hours)")]
        public int BanDuration { get; set; }
        public LinkBanSettings(LinkBanSettings settings);
    }

    public class LinkPermissionSettings
    {
        [JsonProperty(PropertyName = "On Link Server Permissions To Add")]
        public List<string> LinkPermissions { get; set; }
        [JsonProperty(PropertyName = "On Unlink Server Permissions To Remove")]
        public List<string> UnlinkPermissions { get; set; }
        [JsonProperty(PropertyName = "On Link Server Groups To Add")]
        public List<string> LinkGroups { get; set; }
        [JsonProperty(PropertyName = "On Unlink Server Groups To Remove")]
        public List<string> UnlinkGroups { get; set; }
        [JsonProperty(PropertyName = "On Link Discord Roles To Add")]
        public List<Snowflake> LinkRoles { get; set; }
        [JsonProperty(PropertyName = "On Unlink Discord Roles To Remove")]
        public List<Snowflake> UnlinkRoles { get; set; }
        public LinkPermissionSettings(LinkPermissionSettings settings);
    }

    public class LinkSettings
    {
        [JsonProperty(PropertyName = "Announcement Channel Id")]
        public Snowflake AnnouncementChannel { get; set; }
        [JsonProperty(PropertyName = "Link Code Generator Characters")]
        public string LinkCodeCharacters { get; set; }
        [JsonProperty(PropertyName = "Link Code Generator Length")]
        public int LinkCodeLength { get; set; }
        [JsonProperty(PropertyName = "Automatically Relink A Player If They Leave And Rejoin The Discord Server")]
        public bool AutoRelinkPlayer { get; set; }
        [JsonProperty(PropertyName = "Inactive Settings")]
        public InactiveSettings InactiveSettings { get; set; }
        public LinkSettings(LinkSettings settings);
    }

    public class PluginConfig
    {
        [DefaultValue("")]
        [JsonProperty(PropertyName = "Discord Bot Token")]
        public string ApiKey { get; set; }
        [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
        public Snowflake GuildId { get; set; }
        [DefaultValue("")]
        [JsonProperty(PropertyName = "Discord Server Name Override")]
        public string ServerNameOverride { get; set; }
        [DefaultValue("")]
        [JsonProperty(PropertyName = "Discord Server Invite Url")]
        public string InviteUrl { get; set; }
        [JsonProperty(PropertyName = "Link Settings")]
        public LinkSettings LinkSettings { get; set; }
        [JsonProperty(PropertyName = "Welcome Message Settings")]
        public WelcomeMessageSettings WelcomeMessageSettings { get; set; }
        [JsonProperty(PropertyName = "Guild Link Message Settings")]
        public GuildMessageSettings LinkMessageSettings { get; set; }
        [JsonProperty(PropertyName = "Link Permission Settings")]
        public LinkPermissionSettings PermissionSettings { get; set; }
        [JsonProperty(PropertyName = "Link Ban Settings")]
        public LinkBanSettings LinkBanSettings { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DiscordLogLevel.Info)]
        [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel ExtensionDebugging { get; set; }
    }

    public class WelcomeMessageSettings
    {
        [JsonProperty(PropertyName = "Enable Welcome DM Message")]
        public bool EnableWelcomeMessage { get; set; }
        [JsonProperty(PropertyName = "Send Welcome Message On Discord Server Join")]
        public bool SendOnGuildJoin { get; set; }
        [JsonProperty(PropertyName = "Send Welcome Message On Role ID Added")]
        public List<Snowflake> SendOnRoleAdded { get; set; }
        [JsonProperty(PropertyName = "Add Link Accounts Button In Welcome Message")]
        public bool EnableLinkButton { get; set; }
        public WelcomeMessageSettings(WelcomeMessageSettings settings);
    }

    public class DiscordInfo
    {
        public Snowflake DiscordId { get; set; }
        public string PlayerId { get; set; }
        public DateTime LastOnline { get; set; }
        [JsonConstructor]
        public DiscordInfo();
        public DiscordInfo(IPlayer player, DiscordUser user);
    }

    public class LinkMessageData
    {
        public Snowflake ChannelId { get; set; }
        public Snowflake MessageId { get; set; }
        [JsonConstructor]
        public LinkMessageData();
        public LinkMessageData(Snowflake channelId, Snowflake messageId);
    }

    public class PluginData
    {
        public Hash<string, DiscordInfo> PlayerDiscordInfo;
        public Hash<Snowflake, DiscordInfo> LeftPlayerInfo;
        public Hash<string, DiscordInfo> InactivePlayerInfo;
        public LinkMessageData MessageData;
    }

    public class JoinBanData
    {
        public int Times { get; set; }
        private DateTime _bannedUntil;
        public void AddDeclined();
        public bool IsBanned();
        public TimeSpan GetRemainingBan();
        public void SetBanDuration(float hours);
    }

    public class JoinBanHandler
    {
        private readonly Hash<string, JoinBanData> _playerBans;
        private readonly Hash<Snowflake, JoinBanData> _discordBans;
        private readonly LinkBanSettings _settings;
        public JoinBanHandler(LinkBanSettings settings);
        public void AddBan(IPlayer player);
        public void AddBan(DiscordUser user);
        public bool Unban(IPlayer player);
        public bool Unban(DiscordUser user);
        public bool IsBanned(IPlayer player);
        public bool IsBanned(DiscordUser user);
        public TimeSpan GetRemainingDuration(IPlayer player);
        public DateTimeOffset GetBannedEndDate(IPlayer player);
        public TimeSpan GetRemainingDuration(DiscordUser user);
        public DateTimeOffset GetBannedEndDate(DiscordUser user);
        private JoinBanData GetBan(IPlayer player);
        private JoinBanData GetBan(DiscordUser user);
    }

    public class JoinData
    {
        public IPlayer Player { get; set; }
        public DiscordUser Discord { get; set; }
        public string Code { get; set; }
        public JoinSource From { get; set; }
        private JoinData();
        public static JoinData CreateServerActivation(IPlayer player, string code);
        public static JoinData CreateDiscordActivation(DiscordUser user, string code);
        public static JoinData CreateLinkedActivation(JoinSource source, IPlayer player, DiscordUser user);
        public bool IsCompleted();
        public bool IsMatch(IPlayer player);
        public bool IsMatch(DiscordUser user);
    }

    public class JoinHandler
    {
        private readonly List<JoinData> _activations;
        private readonly LinkSettings _settings;
        private readonly LinkHandler _linkHandler;
        private readonly JoinBanHandler _ban;
        private readonly DiscordCore _plugin;
        private readonly StringBuilder _sb;
        public JoinHandler(LinkSettings settings, LinkHandler linkHandler, JoinBanHandler ban);
        public JoinData FindByCode(string code);
        public JoinData FindCompletedByPlayer(IPlayer player);
        public JoinData FindCompletedByUser(DiscordUser user);
        public void RemoveByPlayer(IPlayer player);
        public void RemoveByUser(DiscordUser user);
        public JoinData CreateActivation(IPlayer player);
        public JoinData CreateActivation(DiscordUser user);
        public JoinData CreateActivation(IPlayer player, DiscordUser user, JoinSource from);
        private string GenerateCode();
        public void CompleteLink(JoinData data, DiscordInteraction interaction);
        public void DeclineLink(JoinData data, DiscordInteraction interaction);
    }

    public class LinkHandler
    {
        private readonly PluginData _pluginData;
        private readonly LinkPermissionSettings _permissionSettings;
        private readonly LinkSettings _settings;
        private readonly DiscordLink _link;
        private readonly IPlayerManager _players;
        private readonly DiscordCore _plugin;
        private readonly Hash<LinkReason, LinkMessage> _linkMessages;
        private readonly Hash<UnlinkedReason, LinkMessage> _unlinkMessages;
        public LinkHandler(PluginData pluginData, PluginConfig config);
        public void HandleLink(IPlayer player, DiscordUser user, LinkReason reason, DiscordInteraction interaction);
        public void HandleUnlink(IPlayer player, DiscordUser user, UnlinkedReason reason, DiscordInteraction interaction);
        public void OnUserConnected(IPlayer player);
        public void OnGuildMemberLeft(DiscordUser user);
        public void OnGuildMemberJoin(DiscordUser user);
        public void ProcessLeaveAndRejoin();
        private void ProcessLeftPlayers(List<DiscordInfo> possiblyLeftPlayers);
        private void ProcessLeftPlayer(DiscordInfo info, List<DiscordInfo> remaining);
        private void AddPermissions(IPlayer player, DiscordUser user);
        private void RemovePermissions(IPlayer player, DiscordUser user, UnlinkedReason reason);
    }

    public class LinkMessage
    {
        private readonly string _chatLang;
        private readonly string _chatAnnouncement;
        private readonly TemplateKey _discordTemplate;
        private readonly TemplateKey _announcementTemplate;
        private readonly DiscordCore _plugin;
        private readonly LinkSettings _link;
        public LinkMessage(string chatLang, string chatAnnouncement, TemplateKey discordTemplate, TemplateKey announcementTemplate, DiscordCore plugin, LinkSettings link);
        public void SendMessages(IPlayer player, DiscordUser user, DiscordInteraction interaction, PlaceholderData data);
    }

    public static class ServerLang
    {
        public const string Format;
        public const string NoPermission;
        public static class Announcements
        {
            private const string Base;
            public static class Link
            {
                private const string Base;
                public const string Command;
                public const string Admin;
                public const string Api;
                public const string GuildRejoin;
                public const string InactiveRejoin;
            }

            public static class Unlink
            {
                private const string Base;
                public const string Command;
                public const string Admin;
                public const string Api;
                public const string LeftGuild;
                public const string Inactive;
            }

        }

        public static class Commands
        {
            private const string Base;
            public const string DcCommand;
            public const string CodeCommand;
            public const string UserCommand;
            public const string LeaveCommand;
            public const string AcceptCommand;
            public const string DeclineCommand;
            public const string LinkCommand;
            public const string HelpMessage;
            public static class Code
            {
                private const string Base;
                public const string LinkInfo;
                public const string LinkServer;
                public const string LinkInGuild;
                public const string LinkInDm;
            }

            public static class User
            {
                private const string Base;
                public const string MatchFound;
                public static class Errors
                {
                    private const string Base;
                    public const string InvalidSyntax;
                    public const string UserIdNotFound;
                    public const string UserNotFound;
                    public const string MultipleUsersFound;
                    public const string SearchError;
                }

            }

            public static class Leave
            {
                private const string Base;
                public static class Errors
                {
                    private const string Base;
                    public const string NotLinked;
                }

            }

        }

        public static class Link
        {
            private const string Base;
            public static class Completed
            {
                private const string Base;
                public const string Command;
                public const string Admin;
                public const string Api;
                public const string GuildRejoin;
                public const string InactiveRejoin;
            }

            public static class Declined
            {
                private const string Base;
                public const string JoinWithPlayer;
                public const string JoinWithUser;
            }

            public static class Errors
            {
                private const string Base;
                public const string InvalidSyntax;
            }

        }

        public static class Unlink
        {
            private const string Base;
            public static class Completed
            {
                private const string Base;
                public const string Command;
                public const string LeftGuild;
                public const string Admin;
                public const string Api;
            }

        }

        public static class Banned
        {
            private const string Base;
            public const string IsUserBanned;
        }

        public static class Join
        {
            private const string Base;
            public const string ByPlayer;
            public static class Errors
            {
                private const string Base;
                public const string PlayerJoinActivationNotFound;
            }

        }

        public static class Discord
        {
            private const string Base;
            public const string DiscordCommand;
            public const string LinkCommand;
        }

        public static class Errors
        {
            private const string Base;
            public const string PlayerAlreadyLinked;
            public const string DiscordAlreadyLinked;
            public const string ActivationNotFound;
            public const string MustBeCompletedInDiscord;
            public const string ConsolePlayerNotSupported;
        }

    }

    public class PlaceholderDataKeys
    {
        public static readonly PlaceholderDataKey Code;
        public static readonly PlaceholderDataKey NotFound;
    }

    public class PlaceholderKeys
    {
        public static readonly PlaceholderKey InviteUrl;
        public static readonly PlaceholderKey LinkCode;
        public static readonly PlaceholderKey CommandChannels;
        public static readonly PlaceholderKey NotFound;
    }

    public static class TemplateKeys
    {
        public static class Announcements
        {
            private const string Base;
            public static class Link
            {
                private const string Base;
                public static readonly TemplateKey Command;
                public static readonly TemplateKey Admin;
                public static readonly TemplateKey Api;
                public static readonly TemplateKey GuildRejoin;
                public static readonly TemplateKey InactiveRejoin;
            }

            public static class Unlink
            {
                private const string Base;
                public static readonly TemplateKey Command;
                public static readonly TemplateKey Admin;
                public static readonly TemplateKey Api;
                public static readonly TemplateKey LeftGuild;
                public static readonly TemplateKey Inactive;
            }

            public static class Ban
            {
                private const string Base;
                public static readonly TemplateKey PlayerBanned;
                public static readonly TemplateKey UserBanned;
            }

        }

        public static class WelcomeMessage
        {
            private const string Base;
            public static readonly TemplateKey PmWelcomeMessage;
            public static readonly TemplateKey GuildWelcomeMessage;
            public static class Error
            {
                private const string Base;
                public static readonly TemplateKey AlreadyLinked;
            }

        }

        public static class Commands
        {
            private const string Base;
            public static class Code
            {
                private const string Base;
                public static readonly TemplateKey Success;
            }

            public static class User
            {
                private const string Base;
                public static readonly TemplateKey Success;
                public static class Error
                {
                    private const string Base;
                    public static readonly TemplateKey PlayerIsInvalid;
                    public static readonly TemplateKey PlayerNotConnected;
                }

            }

            public static class Leave
            {
                private const string Base;
                public static class Error
                {
                    private const string Base;
                    public static readonly TemplateKey UserNotLinked;
                }

            }

            public static class Admin
            {
                private const string Base;
                public static class Link
                {
                    private const string Base;
                    public static readonly TemplateKey Success;
                    public static class Error
                    {
                        private const string Base;
                        public static readonly TemplateKey PlayerNotFound;
                        public static readonly TemplateKey PlayerAlreadyLinked;
                        public static readonly TemplateKey UserAlreadyLinked;
                    }

                }

                public static class Unlink
                {
                    private const string Base;
                    public static readonly TemplateKey Success;
                    public static class Error
                    {
                        private const string Base;
                        public static readonly TemplateKey MustSpecifyOne;
                        public static readonly TemplateKey PlayerIsNotLinked;
                        public static readonly TemplateKey UserIsNotLinked;
                        public static readonly TemplateKey LinkNotSame;
                    }

                }

                public static class Search
                {
                    private const string Base;
                    public static readonly TemplateKey Success;
                    public static class Error
                    {
                        private const string Base;
                        public static readonly TemplateKey PlayerNotFound;
                    }

                }

                public static class Unban
                {
                    private const string Base;
                    public static readonly TemplateKey Player;
                    public static readonly TemplateKey User;
                    public static class Error
                    {
                        private const string Base;
                        public static readonly TemplateKey PlayerNotFound;
                        public static readonly TemplateKey PlayerNotBanned;
                        public static readonly TemplateKey UserNotBanned;
                    }

                }

            }

        }

        public static class Link
        {
            private const string Base;
            public static class Completed
            {
                private const string Base;
                public static readonly TemplateKey Command;
                public static readonly TemplateKey Admin;
                public static readonly TemplateKey Api;
                public static readonly TemplateKey GuildRejoin;
                public static readonly TemplateKey InactiveRejoin;
            }

            public static class Declined
            {
                private const string Base;
                public static readonly TemplateKey JoinWithUser;
                public static readonly TemplateKey JoinWithPlayer;
            }

            public static class WelcomeMessage
            {
                private const string Base;
                public static readonly TemplateKey DmLinkAccounts;
                public static readonly TemplateKey GuildLinkAccounts;
            }

        }

        public static class Unlink
        {
            private const string Base;
            public static class Completed
            {
                private const string Base;
                public static readonly TemplateKey Command;
                public static readonly TemplateKey Admin;
                public static readonly TemplateKey Api;
                public static readonly TemplateKey Inactive;
            }

        }

        public static class Banned
        {
            private const string Base;
            public static readonly TemplateKey PlayerBanned;
        }

        public static class Join
        {
            private const string Base;
            public static readonly TemplateKey CompleteLink;
        }

        public static class Errors
        {
            private const string Base;
            public static readonly TemplateKey UserAlreadyLinked;
            public static readonly TemplateKey PlayerAlreadyLinked;
            public static readonly TemplateKey CodeActivationNotFound;
            public static readonly TemplateKey LookupActivationNotFound;
            public static readonly TemplateKey MustBeCompletedInServer;
        }

    }

}

public static class ApiErrorCodes
{
    public const string PlayerIsLinked;
    public const string PlayerIsNotLinked;
    public const string UserIsLinked;
    public const string UserIsNotLinked;
}

public static class AdminAppCommands
{
    public const string Command;
    public const string LinkCommand;
    public const string UnlinkCommand;
    public const string SearchCommand;
    public const string PlayerCommand;
    public const string UserCommand;
    public const string Unban;
}

public static class UserAppCommands
{
    public const string Command;
    public const string CodeCommand;
    public const string UserCommand;
    public const string LeaveCommand;
    public const string LinkCommand;
}

public class GuildMessageSettings
{
    [JsonProperty(PropertyName = "Enable Guild Link Message")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Message Channel ID")]
    public Snowflake ChannelId { get; set; }
    public GuildMessageSettings(GuildMessageSettings settings);
}

public class InactiveSettings
{
    [JsonProperty(PropertyName = "Automatically Unlink Inactive Players")]
    public bool UnlinkInactive { get; set; }
    [JsonProperty(PropertyName = "Player Considered Inactive After X (Days)")]
    public float UnlinkInactiveDays { get; set; }
    [JsonProperty(PropertyName = "Automatically Relink Inactive Players On Game Server Join")]
    public bool AutoRelinkInactive { get; set; }
    public InactiveSettings(InactiveSettings settings);
}

public class LinkBanSettings
{
    [JsonProperty(PropertyName = "Enable Link Ban")]
    public bool EnableLinkBanning { get; set; }
    [JsonProperty(PropertyName = "Ban Announcement Channel ID")]
    public Snowflake BanAnnouncementChannel { get; set; }
    [JsonProperty(PropertyName = "Ban Link After X Join Declines")]
    public int BanDeclineAmount { get; set; }
    [JsonProperty(PropertyName = "Ban Duration (Hours)")]
    public int BanDuration { get; set; }
    public LinkBanSettings(LinkBanSettings settings);
}

public class LinkPermissionSettings
{
    [JsonProperty(PropertyName = "On Link Server Permissions To Add")]
    public List<string> LinkPermissions { get; set; }
    [JsonProperty(PropertyName = "On Unlink Server Permissions To Remove")]
    public List<string> UnlinkPermissions { get; set; }
    [JsonProperty(PropertyName = "On Link Server Groups To Add")]
    public List<string> LinkGroups { get; set; }
    [JsonProperty(PropertyName = "On Unlink Server Groups To Remove")]
    public List<string> UnlinkGroups { get; set; }
    [JsonProperty(PropertyName = "On Link Discord Roles To Add")]
    public List<Snowflake> LinkRoles { get; set; }
    [JsonProperty(PropertyName = "On Unlink Discord Roles To Remove")]
    public List<Snowflake> UnlinkRoles { get; set; }
    public LinkPermissionSettings(LinkPermissionSettings settings);
}

public class LinkSettings
{
    [JsonProperty(PropertyName = "Announcement Channel Id")]
    public Snowflake AnnouncementChannel { get; set; }
    [JsonProperty(PropertyName = "Link Code Generator Characters")]
    public string LinkCodeCharacters { get; set; }
    [JsonProperty(PropertyName = "Link Code Generator Length")]
    public int LinkCodeLength { get; set; }
    [JsonProperty(PropertyName = "Automatically Relink A Player If They Leave And Rejoin The Discord Server")]
    public bool AutoRelinkPlayer { get; set; }
    [JsonProperty(PropertyName = "Inactive Settings")]
    public InactiveSettings InactiveSettings { get; set; }
    public LinkSettings(LinkSettings settings);
}

public class PluginConfig
{
    [DefaultValue("")]
    [JsonProperty(PropertyName = "Discord Bot Token")]
    public string ApiKey { get; set; }
    [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
    public Snowflake GuildId { get; set; }
    [DefaultValue("")]
    [JsonProperty(PropertyName = "Discord Server Name Override")]
    public string ServerNameOverride { get; set; }
    [DefaultValue("")]
    [JsonProperty(PropertyName = "Discord Server Invite Url")]
    public string InviteUrl { get; set; }
    [JsonProperty(PropertyName = "Link Settings")]
    public LinkSettings LinkSettings { get; set; }
    [JsonProperty(PropertyName = "Welcome Message Settings")]
    public WelcomeMessageSettings WelcomeMessageSettings { get; set; }
    [JsonProperty(PropertyName = "Guild Link Message Settings")]
    public GuildMessageSettings LinkMessageSettings { get; set; }
    [JsonProperty(PropertyName = "Link Permission Settings")]
    public LinkPermissionSettings PermissionSettings { get; set; }
    [JsonProperty(PropertyName = "Link Ban Settings")]
    public LinkBanSettings LinkBanSettings { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DiscordLogLevel.Info)]
    [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel ExtensionDebugging { get; set; }
}

public class WelcomeMessageSettings
{
    [JsonProperty(PropertyName = "Enable Welcome DM Message")]
    public bool EnableWelcomeMessage { get; set; }
    [JsonProperty(PropertyName = "Send Welcome Message On Discord Server Join")]
    public bool SendOnGuildJoin { get; set; }
    [JsonProperty(PropertyName = "Send Welcome Message On Role ID Added")]
    public List<Snowflake> SendOnRoleAdded { get; set; }
    [JsonProperty(PropertyName = "Add Link Accounts Button In Welcome Message")]
    public bool EnableLinkButton { get; set; }
    public WelcomeMessageSettings(WelcomeMessageSettings settings);
}

public class DiscordInfo
{
    public Snowflake DiscordId { get; set; }
    public string PlayerId { get; set; }
    public DateTime LastOnline { get; set; }
    [JsonConstructor]
    public DiscordInfo();
    public DiscordInfo(IPlayer player, DiscordUser user);
}

public class LinkMessageData
{
    public Snowflake ChannelId { get; set; }
    public Snowflake MessageId { get; set; }
    [JsonConstructor]
    public LinkMessageData();
    public LinkMessageData(Snowflake channelId, Snowflake messageId);
}

public class PluginData
{
    public Hash<string, DiscordInfo> PlayerDiscordInfo;
    public Hash<Snowflake, DiscordInfo> LeftPlayerInfo;
    public Hash<string, DiscordInfo> InactivePlayerInfo;
    public LinkMessageData MessageData;
}

public class JoinBanData
{
    public int Times { get; set; }
    private DateTime _bannedUntil;
    public void AddDeclined();
    public bool IsBanned();
    public TimeSpan GetRemainingBan();
    public void SetBanDuration(float hours);
}

public class JoinBanHandler
{
    private readonly Hash<string, JoinBanData> _playerBans;
    private readonly Hash<Snowflake, JoinBanData> _discordBans;
    private readonly LinkBanSettings _settings;
    public JoinBanHandler(LinkBanSettings settings);
    public void AddBan(IPlayer player);
    public void AddBan(DiscordUser user);
    public bool Unban(IPlayer player);
    public bool Unban(DiscordUser user);
    public bool IsBanned(IPlayer player);
    public bool IsBanned(DiscordUser user);
    public TimeSpan GetRemainingDuration(IPlayer player);
    public DateTimeOffset GetBannedEndDate(IPlayer player);
    public TimeSpan GetRemainingDuration(DiscordUser user);
    public DateTimeOffset GetBannedEndDate(DiscordUser user);
    private JoinBanData GetBan(IPlayer player);
    private JoinBanData GetBan(DiscordUser user);
}

public class JoinData
{
    public IPlayer Player { get; set; }
    public DiscordUser Discord { get; set; }
    public string Code { get; set; }
    public JoinSource From { get; set; }
    private JoinData();
    public static JoinData CreateServerActivation(IPlayer player, string code);
    public static JoinData CreateDiscordActivation(DiscordUser user, string code);
    public static JoinData CreateLinkedActivation(JoinSource source, IPlayer player, DiscordUser user);
    public bool IsCompleted();
    public bool IsMatch(IPlayer player);
    public bool IsMatch(DiscordUser user);
}

public class JoinHandler
{
    private readonly List<JoinData> _activations;
    private readonly LinkSettings _settings;
    private readonly LinkHandler _linkHandler;
    private readonly JoinBanHandler _ban;
    private readonly DiscordCore _plugin;
    private readonly StringBuilder _sb;
    public JoinHandler(LinkSettings settings, LinkHandler linkHandler, JoinBanHandler ban);
    public JoinData FindByCode(string code);
    public JoinData FindCompletedByPlayer(IPlayer player);
    public JoinData FindCompletedByUser(DiscordUser user);
    public void RemoveByPlayer(IPlayer player);
    public void RemoveByUser(DiscordUser user);
    public JoinData CreateActivation(IPlayer player);
    public JoinData CreateActivation(DiscordUser user);
    public JoinData CreateActivation(IPlayer player, DiscordUser user, JoinSource from);
    private string GenerateCode();
    public void CompleteLink(JoinData data, DiscordInteraction interaction);
    public void DeclineLink(JoinData data, DiscordInteraction interaction);
}

public class LinkHandler
{
    private readonly PluginData _pluginData;
    private readonly LinkPermissionSettings _permissionSettings;
    private readonly LinkSettings _settings;
    private readonly DiscordLink _link;
    private readonly IPlayerManager _players;
    private readonly DiscordCore _plugin;
    private readonly Hash<LinkReason, LinkMessage> _linkMessages;
    private readonly Hash<UnlinkedReason, LinkMessage> _unlinkMessages;
    public LinkHandler(PluginData pluginData, PluginConfig config);
    public void HandleLink(IPlayer player, DiscordUser user, LinkReason reason, DiscordInteraction interaction);
    public void HandleUnlink(IPlayer player, DiscordUser user, UnlinkedReason reason, DiscordInteraction interaction);
    public void OnUserConnected(IPlayer player);
    public void OnGuildMemberLeft(DiscordUser user);
    public void OnGuildMemberJoin(DiscordUser user);
    public void ProcessLeaveAndRejoin();
    private void ProcessLeftPlayers(List<DiscordInfo> possiblyLeftPlayers);
    private void ProcessLeftPlayer(DiscordInfo info, List<DiscordInfo> remaining);
    private void AddPermissions(IPlayer player, DiscordUser user);
    private void RemovePermissions(IPlayer player, DiscordUser user, UnlinkedReason reason);
}

public class LinkMessage
{
    private readonly string _chatLang;
    private readonly string _chatAnnouncement;
    private readonly TemplateKey _discordTemplate;
    private readonly TemplateKey _announcementTemplate;
    private readonly DiscordCore _plugin;
    private readonly LinkSettings _link;
    public LinkMessage(string chatLang, string chatAnnouncement, TemplateKey discordTemplate, TemplateKey announcementTemplate, DiscordCore plugin, LinkSettings link);
    public void SendMessages(IPlayer player, DiscordUser user, DiscordInteraction interaction, PlaceholderData data);
}

public static class ServerLang
{
    public const string Format;
    public const string NoPermission;
    public static class Announcements
    {
        private const string Base;
        public static class Link
        {
            private const string Base;
            public const string Command;
            public const string Admin;
            public const string Api;
            public const string GuildRejoin;
            public const string InactiveRejoin;
        }

        public static class Unlink
        {
            private const string Base;
            public const string Command;
            public const string Admin;
            public const string Api;
            public const string LeftGuild;
            public const string Inactive;
        }

    }

    public static class Commands
    {
        private const string Base;
        public const string DcCommand;
        public const string CodeCommand;
        public const string UserCommand;
        public const string LeaveCommand;
        public const string AcceptCommand;
        public const string DeclineCommand;
        public const string LinkCommand;
        public const string HelpMessage;
        public static class Code
        {
            private const string Base;
            public const string LinkInfo;
            public const string LinkServer;
            public const string LinkInGuild;
            public const string LinkInDm;
        }

        public static class User
        {
            private const string Base;
            public const string MatchFound;
            public static class Errors
            {
                private const string Base;
                public const string InvalidSyntax;
                public const string UserIdNotFound;
                public const string UserNotFound;
                public const string MultipleUsersFound;
                public const string SearchError;
            }

        }

        public static class Leave
        {
            private const string Base;
            public static class Errors
            {
                private const string Base;
                public const string NotLinked;
            }

        }

    }

    public static class Link
    {
        private const string Base;
        public static class Completed
        {
            private const string Base;
            public const string Command;
            public const string Admin;
            public const string Api;
            public const string GuildRejoin;
            public const string InactiveRejoin;
        }

        public static class Declined
        {
            private const string Base;
            public const string JoinWithPlayer;
            public const string JoinWithUser;
        }

        public static class Errors
        {
            private const string Base;
            public const string InvalidSyntax;
        }

    }

    public static class Unlink
    {
        private const string Base;
        public static class Completed
        {
            private const string Base;
            public const string Command;
            public const string LeftGuild;
            public const string Admin;
            public const string Api;
        }

    }

    public static class Banned
    {
        private const string Base;
        public const string IsUserBanned;
    }

    public static class Join
    {
        private const string Base;
        public const string ByPlayer;
        public static class Errors
        {
            private const string Base;
            public const string PlayerJoinActivationNotFound;
        }

    }

    public static class Discord
    {
        private const string Base;
        public const string DiscordCommand;
        public const string LinkCommand;
    }

    public static class Errors
    {
        private const string Base;
        public const string PlayerAlreadyLinked;
        public const string DiscordAlreadyLinked;
        public const string ActivationNotFound;
        public const string MustBeCompletedInDiscord;
        public const string ConsolePlayerNotSupported;
    }

}

public static class Announcements
{
    private const string Base;
    public static class Link
    {
        private const string Base;
        public const string Command;
        public const string Admin;
        public const string Api;
        public const string GuildRejoin;
        public const string InactiveRejoin;
    }

    public static class Unlink
    {
        private const string Base;
        public const string Command;
        public const string Admin;
        public const string Api;
        public const string LeftGuild;
        public const string Inactive;
    }

}

public static class Link
{
    private const string Base;
    public const string Command;
    public const string Admin;
    public const string Api;
    public const string GuildRejoin;
    public const string InactiveRejoin;
}

public static class Unlink
{
    private const string Base;
    public const string Command;
    public const string Admin;
    public const string Api;
    public const string LeftGuild;
    public const string Inactive;
}

public static class Commands
{
    private const string Base;
    public const string DcCommand;
    public const string CodeCommand;
    public const string UserCommand;
    public const string LeaveCommand;
    public const string AcceptCommand;
    public const string DeclineCommand;
    public const string LinkCommand;
    public const string HelpMessage;
    public static class Code
    {
        private const string Base;
        public const string LinkInfo;
        public const string LinkServer;
        public const string LinkInGuild;
        public const string LinkInDm;
    }

    public static class User
    {
        private const string Base;
        public const string MatchFound;
        public static class Errors
        {
            private const string Base;
            public const string InvalidSyntax;
            public const string UserIdNotFound;
            public const string UserNotFound;
            public const string MultipleUsersFound;
            public const string SearchError;
        }

    }

    public static class Leave
    {
        private const string Base;
        public static class Errors
        {
            private const string Base;
            public const string NotLinked;
        }

    }

}

public static class Code
{
    private const string Base;
    public const string LinkInfo;
    public const string LinkServer;
    public const string LinkInGuild;
    public const string LinkInDm;
}

public static class User
{
    private const string Base;
    public const string MatchFound;
    public static class Errors
    {
        private const string Base;
        public const string InvalidSyntax;
        public const string UserIdNotFound;
        public const string UserNotFound;
        public const string MultipleUsersFound;
        public const string SearchError;
    }

}

public static class Errors
{
    private const string Base;
    public const string InvalidSyntax;
    public const string UserIdNotFound;
    public const string UserNotFound;
    public const string MultipleUsersFound;
    public const string SearchError;
}

public static class Leave
{
    private const string Base;
    public static class Errors
    {
        private const string Base;
        public const string NotLinked;
    }

}

public static class Errors
{
    private const string Base;
    public const string NotLinked;
}

public static class Link
{
    private const string Base;
    public static class Completed
    {
        private const string Base;
        public const string Command;
        public const string Admin;
        public const string Api;
        public const string GuildRejoin;
        public const string InactiveRejoin;
    }

    public static class Declined
    {
        private const string Base;
        public const string JoinWithPlayer;
        public const string JoinWithUser;
    }

    public static class Errors
    {
        private const string Base;
        public const string InvalidSyntax;
    }

}

public static class Completed
{
    private const string Base;
    public const string Command;
    public const string Admin;
    public const string Api;
    public const string GuildRejoin;
    public const string InactiveRejoin;
}

public static class Declined
{
    private const string Base;
    public const string JoinWithPlayer;
    public const string JoinWithUser;
}

public static class Errors
{
    private const string Base;
    public const string InvalidSyntax;
}

public static class Unlink
{
    private const string Base;
    public static class Completed
    {
        private const string Base;
        public const string Command;
        public const string LeftGuild;
        public const string Admin;
        public const string Api;
    }

}

public static class Completed
{
    private const string Base;
    public const string Command;
    public const string LeftGuild;
    public const string Admin;
    public const string Api;
}

public static class Banned
{
    private const string Base;
    public const string IsUserBanned;
}

public static class Join
{
    private const string Base;
    public const string ByPlayer;
    public static class Errors
    {
        private const string Base;
        public const string PlayerJoinActivationNotFound;
    }

}

public static class Errors
{
    private const string Base;
    public const string PlayerJoinActivationNotFound;
}

public static class Discord
{
    private const string Base;
    public const string DiscordCommand;
    public const string LinkCommand;
}

public static class Errors
{
    private const string Base;
    public const string PlayerAlreadyLinked;
    public const string DiscordAlreadyLinked;
    public const string ActivationNotFound;
    public const string MustBeCompletedInDiscord;
    public const string ConsolePlayerNotSupported;
}

public class PlaceholderDataKeys
{
    public static readonly PlaceholderDataKey Code;
    public static readonly PlaceholderDataKey NotFound;
}

public class PlaceholderKeys
{
    public static readonly PlaceholderKey InviteUrl;
    public static readonly PlaceholderKey LinkCode;
    public static readonly PlaceholderKey CommandChannels;
    public static readonly PlaceholderKey NotFound;
}

public static class TemplateKeys
{
    public static class Announcements
    {
        private const string Base;
        public static class Link
        {
            private const string Base;
            public static readonly TemplateKey Command;
            public static readonly TemplateKey Admin;
            public static readonly TemplateKey Api;
            public static readonly TemplateKey GuildRejoin;
            public static readonly TemplateKey InactiveRejoin;
        }

        public static class Unlink
        {
            private const string Base;
            public static readonly TemplateKey Command;
            public static readonly TemplateKey Admin;
            public static readonly TemplateKey Api;
            public static readonly TemplateKey LeftGuild;
            public static readonly TemplateKey Inactive;
        }

        public static class Ban
        {
            private const string Base;
            public static readonly TemplateKey PlayerBanned;
            public static readonly TemplateKey UserBanned;
        }

    }

    public static class WelcomeMessage
    {
        private const string Base;
        public static readonly TemplateKey PmWelcomeMessage;
        public static readonly TemplateKey GuildWelcomeMessage;
        public static class Error
        {
            private const string Base;
            public static readonly TemplateKey AlreadyLinked;
        }

    }

    public static class Commands
    {
        private const string Base;
        public static class Code
        {
            private const string Base;
            public static readonly TemplateKey Success;
        }

        public static class User
        {
            private const string Base;
            public static readonly TemplateKey Success;
            public static class Error
            {
                private const string Base;
                public static readonly TemplateKey PlayerIsInvalid;
                public static readonly TemplateKey PlayerNotConnected;
            }

        }

        public static class Leave
        {
            private const string Base;
            public static class Error
            {
                private const string Base;
                public static readonly TemplateKey UserNotLinked;
            }

        }

        public static class Admin
        {
            private const string Base;
            public static class Link
            {
                private const string Base;
                public static readonly TemplateKey Success;
                public static class Error
                {
                    private const string Base;
                    public static readonly TemplateKey PlayerNotFound;
                    public static readonly TemplateKey PlayerAlreadyLinked;
                    public static readonly TemplateKey UserAlreadyLinked;
                }

            }

            public static class Unlink
            {
                private const string Base;
                public static readonly TemplateKey Success;
                public static class Error
                {
                    private const string Base;
                    public static readonly TemplateKey MustSpecifyOne;
                    public static readonly TemplateKey PlayerIsNotLinked;
                    public static readonly TemplateKey UserIsNotLinked;
                    public static readonly TemplateKey LinkNotSame;
                }

            }

            public static class Search
            {
                private const string Base;
                public static readonly TemplateKey Success;
                public static class Error
                {
                    private const string Base;
                    public static readonly TemplateKey PlayerNotFound;
                }

            }

            public static class Unban
            {
                private const string Base;
                public static readonly TemplateKey Player;
                public static readonly TemplateKey User;
                public static class Error
                {
                    private const string Base;
                    public static readonly TemplateKey PlayerNotFound;
                    public static readonly TemplateKey PlayerNotBanned;
                    public static readonly TemplateKey UserNotBanned;
                }

            }

        }

    }

    public static class Link
    {
        private const string Base;
        public static class Completed
        {
            private const string Base;
            public static readonly TemplateKey Command;
            public static readonly TemplateKey Admin;
            public static readonly TemplateKey Api;
            public static readonly TemplateKey GuildRejoin;
            public static readonly TemplateKey InactiveRejoin;
        }

        public static class Declined
        {
            private const string Base;
            public static readonly TemplateKey JoinWithUser;
            public static readonly TemplateKey JoinWithPlayer;
        }

        public static class WelcomeMessage
        {
            private const string Base;
            public static readonly TemplateKey DmLinkAccounts;
            public static readonly TemplateKey GuildLinkAccounts;
        }

    }

    public static class Unlink
    {
        private const string Base;
        public static class Completed
        {
            private const string Base;
            public static readonly TemplateKey Command;
            public static readonly TemplateKey Admin;
            public static readonly TemplateKey Api;
            public static readonly TemplateKey Inactive;
        }

    }

    public static class Banned
    {
        private const string Base;
        public static readonly TemplateKey PlayerBanned;
    }

    public static class Join
    {
        private const string Base;
        public static readonly TemplateKey CompleteLink;
    }

    public static class Errors
    {
        private const string Base;
        public static readonly TemplateKey UserAlreadyLinked;
        public static readonly TemplateKey PlayerAlreadyLinked;
        public static readonly TemplateKey CodeActivationNotFound;
        public static readonly TemplateKey LookupActivationNotFound;
        public static readonly TemplateKey MustBeCompletedInServer;
    }

}

public static class Announcements
{
    private const string Base;
    public static class Link
    {
        private const string Base;
        public static readonly TemplateKey Command;
        public static readonly TemplateKey Admin;
        public static readonly TemplateKey Api;
        public static readonly TemplateKey GuildRejoin;
        public static readonly TemplateKey InactiveRejoin;
    }

    public static class Unlink
    {
        private const string Base;
        public static readonly TemplateKey Command;
        public static readonly TemplateKey Admin;
        public static readonly TemplateKey Api;
        public static readonly TemplateKey LeftGuild;
        public static readonly TemplateKey Inactive;
    }

    public static class Ban
    {
        private const string Base;
        public static readonly TemplateKey PlayerBanned;
        public static readonly TemplateKey UserBanned;
    }

}

public static class Link
{
    private const string Base;
    public static readonly TemplateKey Command;
    public static readonly TemplateKey Admin;
    public static readonly TemplateKey Api;
    public static readonly TemplateKey GuildRejoin;
    public static readonly TemplateKey InactiveRejoin;
}

public static class Unlink
{
    private const string Base;
    public static readonly TemplateKey Command;
    public static readonly TemplateKey Admin;
    public static readonly TemplateKey Api;
    public static readonly TemplateKey LeftGuild;
    public static readonly TemplateKey Inactive;
}

public static class Ban
{
    private const string Base;
    public static readonly TemplateKey PlayerBanned;
    public static readonly TemplateKey UserBanned;
}

public static class WelcomeMessage
{
    private const string Base;
    public static readonly TemplateKey PmWelcomeMessage;
    public static readonly TemplateKey GuildWelcomeMessage;
    public static class Error
    {
        private const string Base;
        public static readonly TemplateKey AlreadyLinked;
    }

}

public static class Error
{
    private const string Base;
    public static readonly TemplateKey AlreadyLinked;
}

public static class Commands
{
    private const string Base;
    public static class Code
    {
        private const string Base;
        public static readonly TemplateKey Success;
    }

    public static class User
    {
        private const string Base;
        public static readonly TemplateKey Success;
        public static class Error
        {
            private const string Base;
            public static readonly TemplateKey PlayerIsInvalid;
            public static readonly TemplateKey PlayerNotConnected;
        }

    }

    public static class Leave
    {
        private const string Base;
        public static class Error
        {
            private const string Base;
            public static readonly TemplateKey UserNotLinked;
        }

    }

    public static class Admin
    {
        private const string Base;
        public static class Link
        {
            private const string Base;
            public static readonly TemplateKey Success;
            public static class Error
            {
                private const string Base;
                public static readonly TemplateKey PlayerNotFound;
                public static readonly TemplateKey PlayerAlreadyLinked;
                public static readonly TemplateKey UserAlreadyLinked;
            }

        }

        public static class Unlink
        {
            private const string Base;
            public static readonly TemplateKey Success;
            public static class Error
            {
                private const string Base;
                public static readonly TemplateKey MustSpecifyOne;
                public static readonly TemplateKey PlayerIsNotLinked;
                public static readonly TemplateKey UserIsNotLinked;
                public static readonly TemplateKey LinkNotSame;
            }

        }

        public static class Search
        {
            private const string Base;
            public static readonly TemplateKey Success;
            public static class Error
            {
                private const string Base;
                public static readonly TemplateKey PlayerNotFound;
            }

        }

        public static class Unban
        {
            private const string Base;
            public static readonly TemplateKey Player;
            public static readonly TemplateKey User;
            public static class Error
            {
                private const string Base;
                public static readonly TemplateKey PlayerNotFound;
                public static readonly TemplateKey PlayerNotBanned;
                public static readonly TemplateKey UserNotBanned;
            }

        }

    }

}

public static class Code
{
    private const string Base;
    public static readonly TemplateKey Success;
}

public static class User
{
    private const string Base;
    public static readonly TemplateKey Success;
    public static class Error
    {
        private const string Base;
        public static readonly TemplateKey PlayerIsInvalid;
        public static readonly TemplateKey PlayerNotConnected;
    }

}

public static class Error
{
    private const string Base;
    public static readonly TemplateKey PlayerIsInvalid;
    public static readonly TemplateKey PlayerNotConnected;
}

public static class Leave
{
    private const string Base;
    public static class Error
    {
        private const string Base;
        public static readonly TemplateKey UserNotLinked;
    }

}

public static class Error
{
    private const string Base;
    public static readonly TemplateKey UserNotLinked;
}

public static class Admin
{
    private const string Base;
    public static class Link
    {
        private const string Base;
        public static readonly TemplateKey Success;
        public static class Error
        {
            private const string Base;
            public static readonly TemplateKey PlayerNotFound;
            public static readonly TemplateKey PlayerAlreadyLinked;
            public static readonly TemplateKey UserAlreadyLinked;
        }

    }

    public static class Unlink
    {
        private const string Base;
        public static readonly TemplateKey Success;
        public static class Error
        {
            private const string Base;
            public static readonly TemplateKey MustSpecifyOne;
            public static readonly TemplateKey PlayerIsNotLinked;
            public static readonly TemplateKey UserIsNotLinked;
            public static readonly TemplateKey LinkNotSame;
        }

    }

    public static class Search
    {
        private const string Base;
        public static readonly TemplateKey Success;
        public static class Error
        {
            private const string Base;
            public static readonly TemplateKey PlayerNotFound;
        }

    }

    public static class Unban
    {
        private const string Base;
        public static readonly TemplateKey Player;
        public static readonly TemplateKey User;
        public static class Error
        {
            private const string Base;
            public static readonly TemplateKey PlayerNotFound;
            public static readonly TemplateKey PlayerNotBanned;
            public static readonly TemplateKey UserNotBanned;
        }

    }

}

public static class Link
{
    private const string Base;
    public static readonly TemplateKey Success;
    public static class Error
    {
        private const string Base;
        public static readonly TemplateKey PlayerNotFound;
        public static readonly TemplateKey PlayerAlreadyLinked;
        public static readonly TemplateKey UserAlreadyLinked;
    }

}

public static class Error
{
    private const string Base;
    public static readonly TemplateKey PlayerNotFound;
    public static readonly TemplateKey PlayerAlreadyLinked;
    public static readonly TemplateKey UserAlreadyLinked;
}

public static class Unlink
{
    private const string Base;
    public static readonly TemplateKey Success;
    public static class Error
    {
        private const string Base;
        public static readonly TemplateKey MustSpecifyOne;
        public static readonly TemplateKey PlayerIsNotLinked;
        public static readonly TemplateKey UserIsNotLinked;
        public static readonly TemplateKey LinkNotSame;
    }

}

public static class Error
{
    private const string Base;
    public static readonly TemplateKey MustSpecifyOne;
    public static readonly TemplateKey PlayerIsNotLinked;
    public static readonly TemplateKey UserIsNotLinked;
    public static readonly TemplateKey LinkNotSame;
}

public static class Search
{
    private const string Base;
    public static readonly TemplateKey Success;
    public static class Error
    {
        private const string Base;
        public static readonly TemplateKey PlayerNotFound;
    }

}

public static class Error
{
    private const string Base;
    public static readonly TemplateKey PlayerNotFound;
}

public static class Unban
{
    private const string Base;
    public static readonly TemplateKey Player;
    public static readonly TemplateKey User;
    public static class Error
    {
        private const string Base;
        public static readonly TemplateKey PlayerNotFound;
        public static readonly TemplateKey PlayerNotBanned;
        public static readonly TemplateKey UserNotBanned;
    }

}

public static class Error
{
    private const string Base;
    public static readonly TemplateKey PlayerNotFound;
    public static readonly TemplateKey PlayerNotBanned;
    public static readonly TemplateKey UserNotBanned;
}

public static class Link
{
    private const string Base;
    public static class Completed
    {
        private const string Base;
        public static readonly TemplateKey Command;
        public static readonly TemplateKey Admin;
        public static readonly TemplateKey Api;
        public static readonly TemplateKey GuildRejoin;
        public static readonly TemplateKey InactiveRejoin;
    }

    public static class Declined
    {
        private const string Base;
        public static readonly TemplateKey JoinWithUser;
        public static readonly TemplateKey JoinWithPlayer;
    }

    public static class WelcomeMessage
    {
        private const string Base;
        public static readonly TemplateKey DmLinkAccounts;
        public static readonly TemplateKey GuildLinkAccounts;
    }

}

public static class Completed
{
    private const string Base;
    public static readonly TemplateKey Command;
    public static readonly TemplateKey Admin;
    public static readonly TemplateKey Api;
    public static readonly TemplateKey GuildRejoin;
    public static readonly TemplateKey InactiveRejoin;
}

public static class Declined
{
    private const string Base;
    public static readonly TemplateKey JoinWithUser;
    public static readonly TemplateKey JoinWithPlayer;
}

public static class WelcomeMessage
{
    private const string Base;
    public static readonly TemplateKey DmLinkAccounts;
    public static readonly TemplateKey GuildLinkAccounts;
}

public static class Unlink
{
    private const string Base;
    public static class Completed
    {
        private const string Base;
        public static readonly TemplateKey Command;
        public static readonly TemplateKey Admin;
        public static readonly TemplateKey Api;
        public static readonly TemplateKey Inactive;
    }

}

public static class Completed
{
    private const string Base;
    public static readonly TemplateKey Command;
    public static readonly TemplateKey Admin;
    public static readonly TemplateKey Api;
    public static readonly TemplateKey Inactive;
}

public static class Banned
{
    private const string Base;
    public static readonly TemplateKey PlayerBanned;
}

public static class Join
{
    private const string Base;
    public static readonly TemplateKey CompleteLink;
}

public static class Errors
{
    private const string Base;
    public static readonly TemplateKey UserAlreadyLinked;
    public static readonly TemplateKey PlayerAlreadyLinked;
    public static readonly TemplateKey CodeActivationNotFound;
    public static readonly TemplateKey LookupActivationNotFound;
    public static readonly TemplateKey MustBeCompletedInServer;
}


```

---

## DiscordDeath by MJSU - Displays the death notes from the server in a Discord channel

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Globalization;
using System.Text;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("Discord Death", "MJSU", "2.2.0")]
[Description("Displays deaths to a discord channel")]
internal class DiscordDeath : CovalencePlugin
{
    [PluginReference]
    private Plugin DeathNotes;
    private Plugin PlaceholderAPI;
    private PluginConfig _pluginConfig;
    private DeathNotesConfiguration _deathNotesConfig;
    private const string DefaultUrl;
    private const string EmptyField;
    private Hash<string, string> _weaponPrefabs;
    private Action<IPlayer, StringBuilder, bool> _replacer;
    private readonly StringBuilder _parser;
    private DeathData _deathData;
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void OnPluginLoaded(Plugin plugin);
    private void OnDeathNotice(Dictionary<string, object> data, string message);
    public DeathData CreateDeathData(DeathNotesData data, string message);
    public string GetEntityName(BaseEntity entity, object type);
    public string GetBodyPart(HitInfo info);
    public string GetAttachments(HitInfo info);
    private string GetCustomizedWeaponName(HitInfo info, DamageType type);
    public float GetDistance(float distance);
    private string GetWeaponName(HitInfo info, DamageType type);
    public void LoadWeaponPrefabs();
    public void LoadDeathNotesConfig();
    private sealed class DeathNotesConfiguration
    {
        [JsonProperty("Translations")]
        public Translation Translations;
        [JsonProperty("Use Metric Distance")]
        public bool UseMetricDistance;
        public class Translation
        {
            [JsonProperty("Names")]
            public Hash<string, string> Names;
            [JsonProperty("Bodyparts")]
            public Hash<string, string> Bodyparts;
            [JsonProperty("Weapons")]
            public Hash<string, string> Weapons;
            [JsonProperty("Attachments")]
            public Hash<string, string> Attachments;
        }

    }

    private string ParseField(string field);
    private void OnPluginUnloaded(Plugin plugin);
    private void OnPlaceholderAPIReady();
    private void RegisterPlaceholder(string key, Func<IPlayer, string, object> action, string description);
    private Action<IPlayer, StringBuilder, bool> GetReplacer();
    private bool IsPlaceholderApiLoaded();
    private bool IsDeathNotesLoaded();
    private readonly List<Regex> _regexTags;
    private readonly List<string> _tags;
    private string StripRustTags(string original);
    private void Debug(DebugEnum level, string message);
    public string Lang(string key, IPlayer player, object[] args);
    private class PluginConfig
    {
        [JsonProperty("Display Options")]
        public List<DisplayOption> DisplayOptions { get; set; }
        [JsonProperty(PropertyName = "Death Message")]
        public DiscordMessageConfig DeathMessage { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DebugEnum.Warning)]
        [JsonProperty(PropertyName = "Debug Level (None, Error, Warning, Info)")]
        public DebugEnum DebugLevel { get; set; }
    }

    public class DisplayOption
    {
        [JsonProperty(PropertyName = "Webhook url")]
        public string WebhookUrl { get; set; }
        [JsonProperty(PropertyName = "Killer Type")]
        public string KillerType { get; set; }
        [JsonProperty(PropertyName = "Victim Type")]
        public string VictimType { get; set; }
    }

    public class DeathNotesData
    {
        public object VictimEntityTypeRaw { get; set; }
        public CombatEntityType VictimEntityType { get; set; }
        public BaseEntity VictimEntity { get; set; }
        public object KillerEntityTypeRaw { get; set; }
        public CombatEntityType KillerEntityType { get; set; }
        public BaseEntity KillerEntity { get; set; }
        public DamageType DamageType { get; set; }
        public HitInfo HitInfo { get; set; }
        public DeathNotesData(Dictionary<string,object> data);
    }

    public class DeathData
    {
        public string VictimName { get; set; }
        public string VictimId { get; set; }
        public string VictimType { get; set; }
        public string KillerName { get; set; }
        public string KilledId { get; set; }
        public string KillerType { get; set; }
        public float KillerHealth { get; set; }
        public string Weapon { get; set; }
        public string BodyPart { get; set; }
        public string Attachments { get; set; }
        public float Distance { get; set; }
        public string Owner { get; set; }
        public string Message { get; set; }
        public void EnsureNotEmpty();
    }

    private static class LangKeys
    {
        public const string UnknownOwner;
    }

    private readonly Dictionary<string, string> _headers;
    private void SendDiscordMessage(string url, DiscordMessage message);
    private void SendDiscordMessageCallback(int code, string message);
    private const string OwnerIcon;
    private void AddPluginInfoFooter(Embed embed);
    private class DiscordMessage
    {
        [JsonProperty("username")]
        private string Username { get; set; }
        [JsonProperty("avatar_url")]
        private string AvatarUrl { get; set; }
        [JsonProperty("content")]
        private string Content { get; set; }
        [JsonProperty("embeds")]
        private List<Embed> Embeds { get; set; }
        public DiscordMessage(string username, string avatarUrl);
        public DiscordMessage(string content, string username, string avatarUrl);
        public DiscordMessage(Embed embed, string username, string avatarUrl);
        public DiscordMessage AddEmbed(Embed embed);
        public DiscordMessage AddContent(string content);
        public DiscordMessage AddSender(string username, string avatarUrl);
        public string ToJson();
    }

    private class Embed
    {
        [JsonProperty("color")]
        private int Color { get; set; }
        [JsonProperty("fields")]
        private List<Field> Fields { get; set; }
        [JsonProperty("title")]
        private string Title { get; set; }
        [JsonProperty("description")]
        private string Description { get; set; }
        [JsonProperty("url")]
        private string Url { get; set; }
        [JsonProperty("image")]
        private Image Image { get; set; }
        [JsonProperty("thumbnail")]
        private Image Thumbnail { get; set; }
        [JsonProperty("video")]
        private Video Video { get; set; }
        [JsonProperty("author")]
        private AuthorInfo Author { get; set; }
        [JsonProperty("footer")]
        private Footer Footer { get; set; }
        public Embed AddTitle(string title);
        public Embed AddDescription(string description);
        public Embed AddUrl(string url);
        public Embed AddAuthor(string name, string iconUrl, string url, string proxyIconUrl);
        public Embed AddFooter(string text, string iconUrl, string proxyIconUrl);
        public Embed AddColor(int color);
        public Embed AddColor(string color);
        public Embed AddColor(int red, int green, int blue);
        public Embed AddBlankField(bool inline);
        public Embed AddField(string name, string value, bool inline);
        public Embed AddImage(string url, int? width, int? height, string proxyUrl);
        public Embed AddThumbnail(string url, int? width, int? height, string proxyUrl);
        public Embed AddVideo(string url, int? width, int? height);
    }

    private class Field
    {
        [JsonProperty("name")]
        private string Name { get; set; }
        [JsonProperty("value")]
        private string Value { get; set; }
        [JsonProperty("inline")]
        private bool Inline { get; set; }
        public Field(string name, string value, bool inline);
    }

    private class Image
    {
        [JsonProperty("url")]
        private string Url { get; set; }
        [JsonProperty("width")]
        private int? Width { get; set; }
        [JsonProperty("height")]
        private int? Height { get; set; }
        [JsonProperty("proxyURL")]
        private string ProxyUrl { get; set; }
        public Image(string url, int? width, int? height, string proxyUrl);
    }

    private class Video
    {
        [JsonProperty("url")]
        private string Url { get; set; }
        [JsonProperty("width")]
        private int? Width { get; set; }
        [JsonProperty("height")]
        private int? Height { get; set; }
        public Video(string url, int? width, int? height);
    }

    private class AuthorInfo
    {
        [JsonProperty("name")]
        private string Name { get; set; }
        [JsonProperty("url")]
        private string Url { get; set; }
        [JsonProperty("icon_url")]
        private string IconUrl { get; set; }
        [JsonProperty("proxy_icon_url")]
        private string ProxyIconUrl { get; set; }
        public AuthorInfo(string name, string iconUrl, string url, string proxyIconUrl);
    }

    private class Footer
    {
        [JsonProperty("text")]
        private string Text { get; set; }
        [JsonProperty("icon_url")]
        private string IconUrl { get; set; }
        [JsonProperty("proxy_icon_url")]
        private string ProxyIconUrl { get; set; }
        public Footer(string text, string iconUrl, string proxyIconUrl);
    }

    private class Attachment
    {
        public byte[] Data { get; set; }
        public string Filename { get; set; }
        public string ContentType { get; set; }
        public Attachment(byte[] data, string filename, AttachmentContentType contentType);
        public Attachment(byte[] data, string filename, string contentType);
    }

    private class DiscordMessageConfig
    {
        public string Content { get; set; }
        public EmbedConfig Embed { get; set; }
    }

    private class EmbedConfig
    {
        [JsonProperty("Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty("Title")]
        public string Title { get; set; }
        [JsonProperty("Description")]
        public string Description { get; set; }
        [JsonProperty("Url")]
        public string Url { get; set; }
        [JsonProperty("Embed Color")]
        public string Color { get; set; }
        [JsonProperty("Image Url")]
        public string Image { get; set; }
        [JsonProperty("Thumbnail Url")]
        public string Thumbnail { get; set; }
        [JsonProperty("Fields")]
        public List<FieldConfig> Fields { get; set; }
        [JsonProperty("Footer")]
        public FooterConfig Footer { get; set; }
    }

    private class FieldConfig
    {
        [JsonProperty("Title")]
        public string Title { get; set; }
        [JsonProperty("Value")]
        public string Value { get; set; }
        [JsonProperty("Inline")]
        public bool Inline { get; set; }
    }

    private class FooterConfig
    {
        [JsonProperty("Icon Url")]
        public string IconUrl { get; set; }
        [JsonProperty("Text")]
        public string Text { get; set; }
        [JsonProperty("Enabled")]
        public bool Enabled { get; set; }
    }

    private DiscordMessage ParseMessage(DiscordMessageConfig config);
}

private sealed class DeathNotesConfiguration
{
    [JsonProperty("Translations")]
    public Translation Translations;
    [JsonProperty("Use Metric Distance")]
    public bool UseMetricDistance;
    public class Translation
    {
        [JsonProperty("Names")]
        public Hash<string, string> Names;
        [JsonProperty("Bodyparts")]
        public Hash<string, string> Bodyparts;
        [JsonProperty("Weapons")]
        public Hash<string, string> Weapons;
        [JsonProperty("Attachments")]
        public Hash<string, string> Attachments;
    }

}

public class Translation
{
    [JsonProperty("Names")]
    public Hash<string, string> Names;
    [JsonProperty("Bodyparts")]
    public Hash<string, string> Bodyparts;
    [JsonProperty("Weapons")]
    public Hash<string, string> Weapons;
    [JsonProperty("Attachments")]
    public Hash<string, string> Attachments;
}

private class PluginConfig
{
    [JsonProperty("Display Options")]
    public List<DisplayOption> DisplayOptions { get; set; }
    [JsonProperty(PropertyName = "Death Message")]
    public DiscordMessageConfig DeathMessage { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DebugEnum.Warning)]
    [JsonProperty(PropertyName = "Debug Level (None, Error, Warning, Info)")]
    public DebugEnum DebugLevel { get; set; }
}

public class DisplayOption
{
    [JsonProperty(PropertyName = "Webhook url")]
    public string WebhookUrl { get; set; }
    [JsonProperty(PropertyName = "Killer Type")]
    public string KillerType { get; set; }
    [JsonProperty(PropertyName = "Victim Type")]
    public string VictimType { get; set; }
}

public class DeathNotesData
{
    public object VictimEntityTypeRaw { get; set; }
    public CombatEntityType VictimEntityType { get; set; }
    public BaseEntity VictimEntity { get; set; }
    public object KillerEntityTypeRaw { get; set; }
    public CombatEntityType KillerEntityType { get; set; }
    public BaseEntity KillerEntity { get; set; }
    public DamageType DamageType { get; set; }
    public HitInfo HitInfo { get; set; }
    public DeathNotesData(Dictionary<string,object> data);
}

public class DeathData
{
    public string VictimName { get; set; }
    public string VictimId { get; set; }
    public string VictimType { get; set; }
    public string KillerName { get; set; }
    public string KilledId { get; set; }
    public string KillerType { get; set; }
    public float KillerHealth { get; set; }
    public string Weapon { get; set; }
    public string BodyPart { get; set; }
    public string Attachments { get; set; }
    public float Distance { get; set; }
    public string Owner { get; set; }
    public string Message { get; set; }
    public void EnsureNotEmpty();
}

private static class LangKeys
{
    public const string UnknownOwner;
}

private class DiscordMessage
{
    [JsonProperty("username")]
    private string Username { get; set; }
    [JsonProperty("avatar_url")]
    private string AvatarUrl { get; set; }
    [JsonProperty("content")]
    private string Content { get; set; }
    [JsonProperty("embeds")]
    private List<Embed> Embeds { get; set; }
    public DiscordMessage(string username, string avatarUrl);
    public DiscordMessage(string content, string username, string avatarUrl);
    public DiscordMessage(Embed embed, string username, string avatarUrl);
    public DiscordMessage AddEmbed(Embed embed);
    public DiscordMessage AddContent(string content);
    public DiscordMessage AddSender(string username, string avatarUrl);
    public string ToJson();
}

private class Embed
{
    [JsonProperty("color")]
    private int Color { get; set; }
    [JsonProperty("fields")]
    private List<Field> Fields { get; set; }
    [JsonProperty("title")]
    private string Title { get; set; }
    [JsonProperty("description")]
    private string Description { get; set; }
    [JsonProperty("url")]
    private string Url { get; set; }
    [JsonProperty("image")]
    private Image Image { get; set; }
    [JsonProperty("thumbnail")]
    private Image Thumbnail { get; set; }
    [JsonProperty("video")]
    private Video Video { get; set; }
    [JsonProperty("author")]
    private AuthorInfo Author { get; set; }
    [JsonProperty("footer")]
    private Footer Footer { get; set; }
    public Embed AddTitle(string title);
    public Embed AddDescription(string description);
    public Embed AddUrl(string url);
    public Embed AddAuthor(string name, string iconUrl, string url, string proxyIconUrl);
    public Embed AddFooter(string text, string iconUrl, string proxyIconUrl);
    public Embed AddColor(int color);
    public Embed AddColor(string color);
    public Embed AddColor(int red, int green, int blue);
    public Embed AddBlankField(bool inline);
    public Embed AddField(string name, string value, bool inline);
    public Embed AddImage(string url, int? width, int? height, string proxyUrl);
    public Embed AddThumbnail(string url, int? width, int? height, string proxyUrl);
    public Embed AddVideo(string url, int? width, int? height);
}

private class Field
{
    [JsonProperty("name")]
    private string Name { get; set; }
    [JsonProperty("value")]
    private string Value { get; set; }
    [JsonProperty("inline")]
    private bool Inline { get; set; }
    public Field(string name, string value, bool inline);
}

private class Image
{
    [JsonProperty("url")]
    private string Url { get; set; }
    [JsonProperty("width")]
    private int? Width { get; set; }
    [JsonProperty("height")]
    private int? Height { get; set; }
    [JsonProperty("proxyURL")]
    private string ProxyUrl { get; set; }
    public Image(string url, int? width, int? height, string proxyUrl);
}

private class Video
{
    [JsonProperty("url")]
    private string Url { get; set; }
    [JsonProperty("width")]
    private int? Width { get; set; }
    [JsonProperty("height")]
    private int? Height { get; set; }
    public Video(string url, int? width, int? height);
}

private class AuthorInfo
{
    [JsonProperty("name")]
    private string Name { get; set; }
    [JsonProperty("url")]
    private string Url { get; set; }
    [JsonProperty("icon_url")]
    private string IconUrl { get; set; }
    [JsonProperty("proxy_icon_url")]
    private string ProxyIconUrl { get; set; }
    public AuthorInfo(string name, string iconUrl, string url, string proxyIconUrl);
}

private class Footer
{
    [JsonProperty("text")]
    private string Text { get; set; }
    [JsonProperty("icon_url")]
    private string IconUrl { get; set; }
    [JsonProperty("proxy_icon_url")]
    private string ProxyIconUrl { get; set; }
    public Footer(string text, string iconUrl, string proxyIconUrl);
}

private class Attachment
{
    public byte[] Data { get; set; }
    public string Filename { get; set; }
    public string ContentType { get; set; }
    public Attachment(byte[] data, string filename, AttachmentContentType contentType);
    public Attachment(byte[] data, string filename, string contentType);
}

private class DiscordMessageConfig
{
    public string Content { get; set; }
    public EmbedConfig Embed { get; set; }
}

private class EmbedConfig
{
    [JsonProperty("Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty("Title")]
    public string Title { get; set; }
    [JsonProperty("Description")]
    public string Description { get; set; }
    [JsonProperty("Url")]
    public string Url { get; set; }
    [JsonProperty("Embed Color")]
    public string Color { get; set; }
    [JsonProperty("Image Url")]
    public string Image { get; set; }
    [JsonProperty("Thumbnail Url")]
    public string Thumbnail { get; set; }
    [JsonProperty("Fields")]
    public List<FieldConfig> Fields { get; set; }
    [JsonProperty("Footer")]
    public FooterConfig Footer { get; set; }
}

private class FieldConfig
{
    [JsonProperty("Title")]
    public string Title { get; set; }
    [JsonProperty("Value")]
    public string Value { get; set; }
    [JsonProperty("Inline")]
    public bool Inline { get; set; }
}

private class FooterConfig
{
    [JsonProperty("Icon Url")]
    public string IconUrl { get; set; }
    [JsonProperty("Text")]
    public string Text { get; set; }
    [JsonProperty("Enabled")]
    public bool Enabled { get; set; }
}


```

---

## DiscordGroup by MJSU - Adds players to the Discord role after linking their game and Discord accounts

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord.Clients;
using Oxide.Ext.Discord.Connections;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Extensions;
using Oxide.Ext.Discord.Interfaces;
using Oxide.Ext.Discord.Libraries;
using Oxide.Ext.Discord.Logging;

Oxide.Plugins
[Info("Discord Group", "MJSU", "2.1.0")]
[Description("Grants players rewards for linking their game and discord accounts")]
internal class DiscordGroup : CovalencePlugin, IDiscordPlugin
{
    public DiscordClient Client { get; set; }
    private PluginConfig _pluginConfig;
    private StoredData _storedData;
    private DiscordRole _role;
    private DiscordGuild _guild;
    private readonly Queue<SyncData> _processQueue;
    private readonly DiscordLink _link;
    private Action _processNext;
    private bool _initialized;
    private void Init();
    private void OnServerInitialized();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnNewSave(string filename);
    private void Unload();
    [HookMethod(DiscordExtHooks.OnDiscordGatewayReady)]
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
    [HookMethod(DiscordExtHooks.OnDiscordBotFullyLoaded)]
    private void OnDiscordBotFullyLoaded();
    private void OnUserConnected(IPlayer player);
    [HookMethod(DiscordExtHooks.OnDiscordPlayerLinked)]
    private void OnDiscordPlayerLinked(IPlayer player, DiscordUser user);
    [HookMethod(DiscordExtHooks.OnDiscordPlayerUnlinked)]
    private void OnDiscordPlayerUnlinked(IPlayer player, DiscordUser user);
    public void ProcessNext();
    private void HandlePlayerLinked(IPlayer player, DiscordUser user);
    private void AddToOxideGroup(IPlayer player);
    private void AddToDiscordRole(IPlayer player, DiscordUser user);
    private void HandlePlayerUnlinked(IPlayer player, DiscordUser user);
    private void RemoveFromOxide(IPlayer player);
    private void RemoveFromDiscord(IPlayer player, DiscordUser user);
    private void RunCommands(IPlayer player);
    private void SaveData();
    private class PluginConfig
    {
        [DefaultValue("")]
        [JsonProperty(PropertyName = "Discord Bot Token")]
        public string DiscordApiKey { get; set; }
        [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
        public Snowflake GuildId { get; set; }
        [JsonProperty("Add To Discord Role (Role ID)")]
        public Snowflake DiscordRole { get; set; }
        [DefaultValue("")]
        [JsonProperty("Add To Server Group")]
        public string OxideGroup { get; set; }
        [DefaultValue(2f)]
        [JsonProperty("Update Rate (Seconds)")]
        public float UpdateRate { get; set; }
        [DefaultValue(false)]
        [JsonProperty("Run Commands On Link")]
        public bool RunCommands { get; set; }
        [JsonProperty("Commands To Run")]
        public List<string> Commands { get; set; }
        [DefaultValue(false)]
        [JsonProperty("Reset Rewards On Wipe")]
        public bool ResetRewardsOnWipe { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DiscordLogLevel.Info)]
        [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel ExtensionDebugging { get; set; }
    }

    private class StoredData
    {
        public HashSet<string> RewardedPlayers;
    }

}

private class PluginConfig
{
    [DefaultValue("")]
    [JsonProperty(PropertyName = "Discord Bot Token")]
    public string DiscordApiKey { get; set; }
    [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
    public Snowflake GuildId { get; set; }
    [JsonProperty("Add To Discord Role (Role ID)")]
    public Snowflake DiscordRole { get; set; }
    [DefaultValue("")]
    [JsonProperty("Add To Server Group")]
    public string OxideGroup { get; set; }
    [DefaultValue(2f)]
    [JsonProperty("Update Rate (Seconds)")]
    public float UpdateRate { get; set; }
    [DefaultValue(false)]
    [JsonProperty("Run Commands On Link")]
    public bool RunCommands { get; set; }
    [JsonProperty("Commands To Run")]
    public List<string> Commands { get; set; }
    [DefaultValue(false)]
    [JsonProperty("Reset Rewards On Wipe")]
    public bool ResetRewardsOnWipe { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DiscordLogLevel.Info)]
    [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel ExtensionDebugging { get; set; }
}

private class StoredData
{
    public HashSet<string> RewardedPlayers;
}


```

---

## DiscordHooks by NooBlet - A plugin library to send messages to Discord

```csharp
using System;
using System.Net;
using System.Text;
using System.Collections.Generic;
using System.Collections.Specialized;
using UnityEngine;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Discord Hooks", "NooBlet", "0.2.2", ResourceId = 2149)]
[Description("Discord Client for Rust")]
 class DiscordHooks : CovalencePlugin
{
    private static PluginConfig Settings;
    private static string BaseURLTemplate;
     void SendMessage(string MessageText);
     void PostCallBack(int code, string response);
     void Init();
    protected override void LoadDefaultConfig();
    private void LoadConfigValues();
    private PluginConfig DefaultConfig();
    private class PluginConfig
    {
        public string BotToken { get; set; }
        public ulong ChannelID { get; set; }
    }

     class DiscordPayload
    {
        [JsonProperty("content")]
        public string MessageText { get; set; }
    }

}

private class PluginConfig
{
    public string BotToken { get; set; }
    public ulong ChannelID { get; set; }
}

 class DiscordPayload
{
    [JsonProperty("content")]
    public string MessageText { get; set; }
}


```

---

## DiscordLinker by mrcameron999 - Add discord and Steam account linking to your Discord and game server

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;

Oxide.Plugins
[Info("Discord Linker", "mrcameron999", "2.1.1")]
[Description("Provides a way of linking a players' Discord and Steam accounts")]
public class DiscordLinker : CovalencePlugin
{
    private readonly Dictionary<IPlayer, CachedPlayer> _cachedJoins;
    private readonly Dictionary<string, string> _headers;
    private readonly string connection;
    private readonly float timeout;
    private string _groupName;
    private string _groupNitro;
    private bool _showClaimMessage;
    private void Init();
    private void LoadConfigData();
    private void OnUserConnected(IPlayer player);
    private void MessageAllPlayers(string message);
    [Command("checklink")]
    private void CheckLinkCommand(IPlayer iplayer, string command, string[] args);
    [Command("nitro", "linked")]
    private void NitroCheck(IPlayer iplayer, string command, string[] args);
    public class LinkModel
    {
        public int LinkId { get; set; }
        public long SteamId { get; set; }
        public long DiscordId { get; set; }
        public int OrgId { get; set; }
        public DateTime LinkDate { get; set; }
        public bool InDiscord { get; set; }
        public bool ClaimedRewards { get; set; }
        public int? NitroBoostId { get; set; }
        public bool NitroBoosted { get; set; }
    }

    private class CachedPlayer
    {
        public CachedPlayer();
        public DateTime TimeOfAdd { get; set; }
    }

    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
}

public class LinkModel
{
    public int LinkId { get; set; }
    public long SteamId { get; set; }
    public long DiscordId { get; set; }
    public int OrgId { get; set; }
    public DateTime LinkDate { get; set; }
    public bool InDiscord { get; set; }
    public bool ClaimedRewards { get; set; }
    public int? NitroBoostId { get; set; }
    public bool NitroBoosted { get; set; }
}

private class CachedPlayer
{
    public CachedPlayer();
    public DateTime TimeOfAdd { get; set; }
}


```

---

## DiscordLogger by MONaH - Logs events to Discord channels using webhooks

```csharp
using System;
using System.Collections.Generic;
using System.Net;
using System.Text.RegularExpressions;
using System.Text;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Discord Logger", "MON@H", "2.0.20")]
[Description("Logs events to Discord channels using webhooks")]
public class DiscordLogger : RustPlugin
{
    [PluginReference]
    private readonly Plugin AntiSpam;
    private readonly Plugin BetterChatMute;
    private readonly Plugin CallHeli;
    private readonly Plugin PersonalHeli;
    private readonly Plugin UFilter;
    private readonly Hash<ulong, CargoShip> _cargoShips;
    private readonly List<ulong> _listBadCargoShips;
    private readonly List<ulong> _listSupplyDrops;
    private readonly Queue<QueuedMessage> _queue;
    private readonly StringBuilder _sb;
    private EventSettings _eventSettings;
    private int _retryCount;
    private object _resultCall;
    private QueuedMessage _nextMessage;
    private QueuedMessage _queuedMessage;
    private string _langKey;
    private string[] _profanities;
    private Timer _timerQueue;
    private Timer _timerQueueCooldown;
    private ulong _entityID;
    private Vector3 _locationLargeOilRig;
    private Vector3 _locationOilRig;
    private readonly List<Regex> _regexTags;
    private readonly List<string> _tags;
    private class QueuedMessage
    {
        public string WebhookUrl { get; set; }
        public string Message { get; set; }
    }

    private class Response
    {
        [JsonProperty("country")]
        public string Country { get; set; }
        [JsonProperty("countryCode")]
        public string CountryCode { get; set; }
    }

    private void Init();
    private void Unload();
    private void OnServerInitialized(bool isStartup);
    private void OnServerShutdown();
    private ConfigData _configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Global settings")]
        public GlobalSettings GlobalSettings;
        [JsonProperty(PropertyName = "Admin Hammer settings")]
        public EventSettings AdminHammerSettings;
        [JsonProperty(PropertyName = "Admin Radar settings")]
        public EventSettings AdminRadarSettings;
        [JsonProperty(PropertyName = "Bradley settings")]
        public EventSettings BradleySettings;
        [JsonProperty(PropertyName = "Cargo Ship settings")]
        public EventSettings CargoShipSettings;
        [JsonProperty(PropertyName = "Cargo Plane settings")]
        public EventSettings CargoPlaneSettings;
        [JsonProperty(PropertyName = "Chat settings")]
        public EventSettings ChatSettings;
        [JsonProperty(PropertyName = "Chat (Team) settings")]
        public EventSettings ChatTeamSettings;
        [JsonProperty(PropertyName = "CH47 Helicopter settings")]
        public EventSettings ChinookSettings;
        [JsonProperty(PropertyName = "Christmas settings")]
        public EventSettings ChristmasSettings;
        [JsonProperty(PropertyName = "Clan settings")]
        public EventSettings ClanSettings;
        [JsonProperty(PropertyName = "Dangerous Treasures settings")]
        public EventSettings DangerousTreasuresSettings;
        [JsonProperty(PropertyName = "Duel settings")]
        public EventSettings DuelSettings;
        [JsonProperty(PropertyName = "Godmode settings")]
        public EventSettings GodmodeSettings;
        [JsonProperty(PropertyName = "Easter settings")]
        public EventSettings EasterSettings;
        [JsonProperty(PropertyName = "Error settings")]
        public EventSettings ErrorSettings;
        [JsonProperty(PropertyName = "Hackable Locked Crate settings")]
        public EventSettings LockedCrateSettings;
        [JsonProperty(PropertyName = "Halloween settings")]
        public EventSettings HalloweenSettings;
        [JsonProperty(PropertyName = "Helicopter settings")]
        public EventSettings HelicopterSettings;
        [JsonProperty(PropertyName = "NTeleportation settings")]
        public EventSettings NTeleportationSettings;
        [JsonProperty(PropertyName = "Permissions settings")]
        public EventSettings PermissionsSettings;
        [JsonProperty(PropertyName = "Player death settings")]
        public EventSettings PlayerDeathSettings;
        [JsonProperty(PropertyName = "Player DeathNotes settings")]
        public EventSettings PlayerDeathNotesSettings;
        [JsonProperty(PropertyName = "Player connect advanced info settings")]
        public EventSettings PlayerConnectedInfoSettings;
        [JsonProperty(PropertyName = "Player connect settings")]
        public EventSettings PlayerConnectedSettings;
        [JsonProperty(PropertyName = "Player disconnect settings")]
        public EventSettings PlayerDisconnectedSettings;
        [JsonProperty(PropertyName = "Player Respawned settings")]
        public EventSettings PlayerRespawnedSettings;
        [JsonProperty(PropertyName = "Private Messages settings")]
        public EventSettings PrivateMessagesSettings;
        [JsonProperty(PropertyName = "Raidable Bases settings")]
        public EventSettings RaidableBasesSettings;
        [JsonProperty(PropertyName = "Rcon command settings")]
        public EventSettings RconCommandSettings;
        [JsonProperty(PropertyName = "Rcon connection settings")]
        public EventSettings RconConnectionSettings;
        [JsonProperty(PropertyName = "Rust Kits settings")]
        public EventSettings RustKitsSettings;
        [JsonProperty(PropertyName = "SantaSleigh settings")]
        public EventSettings SantaSleighSettings;
        [JsonProperty(PropertyName = "Server messages settings")]
        public EventSettings ServerMessagesSettings;
        [JsonProperty(PropertyName = "Server state settings")]
        public EventSettings ServerStateSettings;
        [JsonProperty(PropertyName = "Supply Drop settings")]
        public EventSettings SupplyDropSettings;
        [JsonProperty(PropertyName = "Teams settings")]
        public EventSettings TeamsSettings;
        [JsonProperty(PropertyName = "User Banned settings")]
        public EventSettings UserBannedSettings;
        [JsonProperty(PropertyName = "User Kicked settings")]
        public EventSettings UserKickedSettings;
        [JsonProperty(PropertyName = "User Muted settings")]
        public EventSettings UserMutedSettings;
        [JsonProperty(PropertyName = "User Name Updated settings")]
        public EventSettings UserNameUpdateSettings;
        [JsonProperty(PropertyName = "Vanish settings")]
        public EventSettings VanishSettings;
    }

    private class GlobalSettings
    {
        [JsonProperty(PropertyName = "Log to console?")]
        public bool LoggingEnabled;
        [JsonProperty(PropertyName = "Use AntiSpam plugin on chat messages")]
        public bool UseAntiSpam;
        [JsonProperty(PropertyName = "Use UFilter plugin on chat messages")]
        public bool UseUFilter;
        [JsonProperty(PropertyName = "Hide admin connect/disconnect messages")]
        public bool HideAdmin;
        [JsonProperty(PropertyName = "Hide NPC death messages")]
        public bool HideNPC;
        [JsonProperty(PropertyName = "Replacement string for tags")]
        public string TagsReplacement;
        [JsonProperty(PropertyName = "Queue interval (1 message per ? seconds)")]
        public float QueueInterval;
        [JsonProperty(PropertyName = "Queue cooldown if connection error (seconds)")]
        public float QueueCooldown;
        [JsonProperty(PropertyName = "Default WebhookURL")]
        public string DefaultWebhookURL;
        [JsonProperty(PropertyName = "RCON command blacklist", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> RCONCommandBlacklist;
    }

    private class EventSettings
    {
        [JsonProperty(PropertyName = "WebhookURL")]
        public string WebhookURL;
        [JsonProperty(PropertyName = "Enabled?")]
        public bool Enabled;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public string Lang(string key, string userIDString, object[] args);
    private static class LangKeys
    {
        public static class Event
        {
            private const string Base;
            public const string Bradley;
            public const string CargoPlane;
            public const string CargoShip;
            public const string Chat;
            public const string ChatTeam;
            public const string Chinook;
            public const string Christmas;
            public const string Death;
            public const string Easter;
            public const string EasterWinner;
            public const string Error;
            public const string Halloween;
            public const string HalloweenWinner;
            public const string Helicopter;
            public const string Initialized;
            public const string LockedCrate;
            public const string PlayerConnected;
            public const string PlayerConnectedInfo;
            public const string PlayerDisconnected;
            public const string PlayerRespawned;
            public const string RconCommand;
            public const string RconConnection;
            public const string SantaSleigh;
            public const string ServerMessage;
            public const string Shutdown;
            public const string SupplyDrop;
            public const string SupplyDropLanded;
            public const string SupplySignal;
            public const string Team;
            public const string UserBanned;
            public const string UserKicked;
            public const string UserMuted;
            public const string UserNameUpdated;
            public const string UserUnbanned;
            public const string UserUnmuted;
        }

        public static class Permission
        {
            private const string Base;
            public const string GroupCreated;
            public const string GroupDeleted;
            public const string UserGroupAdded;
            public const string UserGroupRemoved;
            public const string UserPermissionGranted;
            public const string UserPermissionRevoked;
        }

        public static class Plugin
        {
            private const string Base;
            public const string AdminHammerOff;
            public const string AdminHammerOn;
            public const string AdminRadarOff;
            public const string AdminRadarOn;
            public const string ClanCreated;
            public const string ClanDisbanded;
            public const string DangerousTreasuresEnded;
            public const string DangerousTreasuresStarted;
            public const string DeathNotes;
            public const string Duel;
            public const string GodmodeOff;
            public const string GodmodeOn;
            public const string NTeleportation;
            public const string PersonalHelicopter;
            public const string PrivateMessage;
            public const string RaidableBaseCompleted;
            public const string RaidableBaseEnded;
            public const string RaidableBaseStarted;
            public const string RustKits;
            public const string TimedGroupAdded;
            public const string TimedGroupExtended;
            public const string TimedPermissionExtended;
            public const string TimedPermissionGranted;
            public const string VanishOff;
            public const string VanishOn;
        }

        public static class Format
        {
            private const string Base;
            public const string CargoShip;
            public const string Created;
            public const string Day;
            public const string Days;
            public const string Disbanded;
            public const string Easy;
            public const string Expert;
            public const string Hard;
            public const string Hour;
            public const string Hours;
            public const string LargeOilRig;
            public const string Medium;
            public const string Minute;
            public const string Minutes;
            public const string Nightmare;
            public const string OilRig;
            public const string Second;
            public const string Seconds;
            public const string Updated;
        }

    }

    protected override void LoadDefaultMessages();
    private void OnAdminHammerEnabled(BasePlayer player);
    private void OnAdminHammerDisabled(BasePlayer player);
    private void OnBetterChatMuted(IPlayer target, IPlayer initiator, string reason);
    private void OnBetterChatMuteExpired(IPlayer player);
    private void OnBetterChatTimeMuted(IPlayer target, IPlayer initiator, TimeSpan time, string reason);
    private void OnBetterChatUnmuted(IPlayer target, IPlayer initiator);
    private void OnClanCreate(string tag);
    private void OnClanDisbanded(string tag);
    private void OnDangerousEventStarted(Vector3 containerPos);
    private void OnDangerousEventEnded(Vector3 containerPos);
    private void OnDeathNotice(Dictionary<string, object> data, string message);
    private void OnDuelistDefeated(BasePlayer attacker, BasePlayer victim);
    private void OnEntitySpawned(PatrolHelicopter entity);
    private void OnEntitySpawned(BradleyAPC entity);
    private void OnEntitySpawned(CargoPlane entity);
    private void OnEntitySpawned(CargoShip entity);
    private void OnEntitySpawned(CH47HelicopterAIController entity);
    private void OnEntitySpawned(EggHuntEvent entity);
    private void OnEntitySpawned(HackableLockedCrate entity);
    private void OnEntitySpawned(SantaSleigh entity);
    private void OnEntitySpawned(SupplyDrop entity);
    private void OnEntitySpawned(XMasRefill entity);
    private void OnEntityDeath(BasePlayer player, HitInfo info);
    private void OnEntityKill(EggHuntEvent entity);
    private void OnEntityKill(CargoShip cargoShip);
    private void OnExplosiveThrown(BasePlayer player, SupplySignal entity);
    private void OnExplosiveDropped(BasePlayer player, SupplySignal entity);
    private void OnGodmodeToggled(string playerID, bool enabled);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnPlayerChat(BasePlayer player, string message, ConVar.Chat.ChatChannel channel);
    private void OnPlayerTeleported(BasePlayer player, Vector3 oldPosition, Vector3 newPosition);
    private void OnPMProcessed(IPlayer sender, IPlayer target, string message);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnRadarActivated(BasePlayer player);
    private void OnRadarDeactivated(BasePlayer player);
    private void OnRaidableBaseStarted(Vector3 raidPos, int difficulty);
    private void OnRaidableBaseEnded(Vector3 raidPos, int difficulty);
    private void OnRaidableBaseCompleted(Vector3 raidPos, int difficulty, bool allowPVP, string id, float spawnTime, float despawnTime, float loadTime, ulong ownerId, BasePlayer owner, List<BasePlayer> raiders);
    private void OnRconConnection(IPAddress ip);
    private void OnRconCommand(IPAddress ip, string command, string[] args);
    private void OnSupplyDropLanded(SupplyDrop entity);
    private void OnUserBanned(string name, string id, string ipAddress, string reason);
    private void OnUserKicked(IPlayer player, string reason);
    private void OnUserUnbanned(string name, string id, string ipAddress);
    private void OnUserNameUpdated(string id, string oldName, string newName);
    private void OnServerMessage(string message, string name, string color, ulong id);
    private void OnKitRedeemed(BasePlayer player, string kitName);
    private void OnVanishDisappear(BasePlayer player);
    private void OnVanishReappear(BasePlayer player);
    private void OnTeamCreated(BasePlayer player, RelationshipManager.PlayerTeam team);
    private void OnTeamDisbanded(RelationshipManager.PlayerTeam team);
    private void OnTeamUpdated(ulong currentTeam, RelationshipManager.PlayerTeam team, BasePlayer player);
    private void OnTeamPromote(RelationshipManager.PlayerTeam team, BasePlayer newLeader);
    private void OnTeamLeave(RelationshipManager.PlayerTeam team, BasePlayer player);
    private void OnTeamKick(RelationshipManager.PlayerTeam team, BasePlayer player, ulong target);
    private void OnTeamAcceptInvite(RelationshipManager.PlayerTeam team, BasePlayer player);
    private void OnGroupCreated(string name);
    private void OnGroupDeleted(string name);
    private void OnTimedPermissionGranted(string playerID, string permission, TimeSpan duration);
    private void OnTimedPermissionExtended(string playerID, string permission, TimeSpan duration);
    private void OnTimedGroupAdded(string playerID, string group, TimeSpan duration);
    private void OnTimedGroupExtended(string playerID, string group, TimeSpan duration);
    private void OnUserGroupAdded(string playerID, string groupName);
    private void OnUserGroupRemoved(string playerID, string groupName);
    private void OnUserPermissionGranted(string playerID, string permName);
    private void OnUserPermissionRevoked(string playerID, string permName);
    public string ReplaceChars(string text);
    public void HandleQueue();
    public void QueueCooldownDisable();
    public void HandleEntity(BaseEntity baseEntity);
    public void HandleSupplySignal(BasePlayer player, SupplySignal entity);
    public void HandleRaidableBase(Vector3 raidPos, int difficulty, string langKey, BasePlayer owner, List<BasePlayer> raiders);
    public void HandleDangerousTreasures(Vector3 containerPos, string langKey);
    public void HandleLog(string logString, string stackTrace, LogType type);
    public void HandleTeam(RelationshipManager.PlayerTeam team, TeamEventType teamEventType);
    public void CacheOilRigsLocation();
    public string GetHackableLockedCratePosition(Vector3 position);
    private void DiscordSendMessage(string message, string webhookUrl, bool stripTags);
    public void UnsubscribeHooks();
    public void SubscribeHooks();
    public string StripRustTags(string text);
    public string GetWebhookURL(string url);
    public string GetGridPosition(Vector3 position);
    public string GetFormattedDurationTime(TimeSpan time, string id);
    public void BuildTime(StringBuilder sb, string lang, string playerID, int value);
    public bool IsPluginLoaded(Plugin plugin);
    public void LogToConsole(string text);
    private readonly Dictionary<string, string> _headers;
    public void DiscordSendMessage(string url, DiscordMessage message);
    public void DiscordSendMessageCallback(int code, string message);
    public class DiscordMessage
    {
        [JsonProperty("content")]
        private string Content { get; set; }
        public DiscordMessage(string content);
        public DiscordMessage AddContent(string content);
        public string GetContent();
        public string ToJson();
    }

}

private class QueuedMessage
{
    public string WebhookUrl { get; set; }
    public string Message { get; set; }
}

private class Response
{
    [JsonProperty("country")]
    public string Country { get; set; }
    [JsonProperty("countryCode")]
    public string CountryCode { get; set; }
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Global settings")]
    public GlobalSettings GlobalSettings;
    [JsonProperty(PropertyName = "Admin Hammer settings")]
    public EventSettings AdminHammerSettings;
    [JsonProperty(PropertyName = "Admin Radar settings")]
    public EventSettings AdminRadarSettings;
    [JsonProperty(PropertyName = "Bradley settings")]
    public EventSettings BradleySettings;
    [JsonProperty(PropertyName = "Cargo Ship settings")]
    public EventSettings CargoShipSettings;
    [JsonProperty(PropertyName = "Cargo Plane settings")]
    public EventSettings CargoPlaneSettings;
    [JsonProperty(PropertyName = "Chat settings")]
    public EventSettings ChatSettings;
    [JsonProperty(PropertyName = "Chat (Team) settings")]
    public EventSettings ChatTeamSettings;
    [JsonProperty(PropertyName = "CH47 Helicopter settings")]
    public EventSettings ChinookSettings;
    [JsonProperty(PropertyName = "Christmas settings")]
    public EventSettings ChristmasSettings;
    [JsonProperty(PropertyName = "Clan settings")]
    public EventSettings ClanSettings;
    [JsonProperty(PropertyName = "Dangerous Treasures settings")]
    public EventSettings DangerousTreasuresSettings;
    [JsonProperty(PropertyName = "Duel settings")]
    public EventSettings DuelSettings;
    [JsonProperty(PropertyName = "Godmode settings")]
    public EventSettings GodmodeSettings;
    [JsonProperty(PropertyName = "Easter settings")]
    public EventSettings EasterSettings;
    [JsonProperty(PropertyName = "Error settings")]
    public EventSettings ErrorSettings;
    [JsonProperty(PropertyName = "Hackable Locked Crate settings")]
    public EventSettings LockedCrateSettings;
    [JsonProperty(PropertyName = "Halloween settings")]
    public EventSettings HalloweenSettings;
    [JsonProperty(PropertyName = "Helicopter settings")]
    public EventSettings HelicopterSettings;
    [JsonProperty(PropertyName = "NTeleportation settings")]
    public EventSettings NTeleportationSettings;
    [JsonProperty(PropertyName = "Permissions settings")]
    public EventSettings PermissionsSettings;
    [JsonProperty(PropertyName = "Player death settings")]
    public EventSettings PlayerDeathSettings;
    [JsonProperty(PropertyName = "Player DeathNotes settings")]
    public EventSettings PlayerDeathNotesSettings;
    [JsonProperty(PropertyName = "Player connect advanced info settings")]
    public EventSettings PlayerConnectedInfoSettings;
    [JsonProperty(PropertyName = "Player connect settings")]
    public EventSettings PlayerConnectedSettings;
    [JsonProperty(PropertyName = "Player disconnect settings")]
    public EventSettings PlayerDisconnectedSettings;
    [JsonProperty(PropertyName = "Player Respawned settings")]
    public EventSettings PlayerRespawnedSettings;
    [JsonProperty(PropertyName = "Private Messages settings")]
    public EventSettings PrivateMessagesSettings;
    [JsonProperty(PropertyName = "Raidable Bases settings")]
    public EventSettings RaidableBasesSettings;
    [JsonProperty(PropertyName = "Rcon command settings")]
    public EventSettings RconCommandSettings;
    [JsonProperty(PropertyName = "Rcon connection settings")]
    public EventSettings RconConnectionSettings;
    [JsonProperty(PropertyName = "Rust Kits settings")]
    public EventSettings RustKitsSettings;
    [JsonProperty(PropertyName = "SantaSleigh settings")]
    public EventSettings SantaSleighSettings;
    [JsonProperty(PropertyName = "Server messages settings")]
    public EventSettings ServerMessagesSettings;
    [JsonProperty(PropertyName = "Server state settings")]
    public EventSettings ServerStateSettings;
    [JsonProperty(PropertyName = "Supply Drop settings")]
    public EventSettings SupplyDropSettings;
    [JsonProperty(PropertyName = "Teams settings")]
    public EventSettings TeamsSettings;
    [JsonProperty(PropertyName = "User Banned settings")]
    public EventSettings UserBannedSettings;
    [JsonProperty(PropertyName = "User Kicked settings")]
    public EventSettings UserKickedSettings;
    [JsonProperty(PropertyName = "User Muted settings")]
    public EventSettings UserMutedSettings;
    [JsonProperty(PropertyName = "User Name Updated settings")]
    public EventSettings UserNameUpdateSettings;
    [JsonProperty(PropertyName = "Vanish settings")]
    public EventSettings VanishSettings;
}

private class GlobalSettings
{
    [JsonProperty(PropertyName = "Log to console?")]
    public bool LoggingEnabled;
    [JsonProperty(PropertyName = "Use AntiSpam plugin on chat messages")]
    public bool UseAntiSpam;
    [JsonProperty(PropertyName = "Use UFilter plugin on chat messages")]
    public bool UseUFilter;
    [JsonProperty(PropertyName = "Hide admin connect/disconnect messages")]
    public bool HideAdmin;
    [JsonProperty(PropertyName = "Hide NPC death messages")]
    public bool HideNPC;
    [JsonProperty(PropertyName = "Replacement string for tags")]
    public string TagsReplacement;
    [JsonProperty(PropertyName = "Queue interval (1 message per ? seconds)")]
    public float QueueInterval;
    [JsonProperty(PropertyName = "Queue cooldown if connection error (seconds)")]
    public float QueueCooldown;
    [JsonProperty(PropertyName = "Default WebhookURL")]
    public string DefaultWebhookURL;
    [JsonProperty(PropertyName = "RCON command blacklist", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> RCONCommandBlacklist;
}

private class EventSettings
{
    [JsonProperty(PropertyName = "WebhookURL")]
    public string WebhookURL;
    [JsonProperty(PropertyName = "Enabled?")]
    public bool Enabled;
}

private static class LangKeys
{
    public static class Event
    {
        private const string Base;
        public const string Bradley;
        public const string CargoPlane;
        public const string CargoShip;
        public const string Chat;
        public const string ChatTeam;
        public const string Chinook;
        public const string Christmas;
        public const string Death;
        public const string Easter;
        public const string EasterWinner;
        public const string Error;
        public const string Halloween;
        public const string HalloweenWinner;
        public const string Helicopter;
        public const string Initialized;
        public const string LockedCrate;
        public const string PlayerConnected;
        public const string PlayerConnectedInfo;
        public const string PlayerDisconnected;
        public const string PlayerRespawned;
        public const string RconCommand;
        public const string RconConnection;
        public const string SantaSleigh;
        public const string ServerMessage;
        public const string Shutdown;
        public const string SupplyDrop;
        public const string SupplyDropLanded;
        public const string SupplySignal;
        public const string Team;
        public const string UserBanned;
        public const string UserKicked;
        public const string UserMuted;
        public const string UserNameUpdated;
        public const string UserUnbanned;
        public const string UserUnmuted;
    }

    public static class Permission
    {
        private const string Base;
        public const string GroupCreated;
        public const string GroupDeleted;
        public const string UserGroupAdded;
        public const string UserGroupRemoved;
        public const string UserPermissionGranted;
        public const string UserPermissionRevoked;
    }

    public static class Plugin
    {
        private const string Base;
        public const string AdminHammerOff;
        public const string AdminHammerOn;
        public const string AdminRadarOff;
        public const string AdminRadarOn;
        public const string ClanCreated;
        public const string ClanDisbanded;
        public const string DangerousTreasuresEnded;
        public const string DangerousTreasuresStarted;
        public const string DeathNotes;
        public const string Duel;
        public const string GodmodeOff;
        public const string GodmodeOn;
        public const string NTeleportation;
        public const string PersonalHelicopter;
        public const string PrivateMessage;
        public const string RaidableBaseCompleted;
        public const string RaidableBaseEnded;
        public const string RaidableBaseStarted;
        public const string RustKits;
        public const string TimedGroupAdded;
        public const string TimedGroupExtended;
        public const string TimedPermissionExtended;
        public const string TimedPermissionGranted;
        public const string VanishOff;
        public const string VanishOn;
    }

    public static class Format
    {
        private const string Base;
        public const string CargoShip;
        public const string Created;
        public const string Day;
        public const string Days;
        public const string Disbanded;
        public const string Easy;
        public const string Expert;
        public const string Hard;
        public const string Hour;
        public const string Hours;
        public const string LargeOilRig;
        public const string Medium;
        public const string Minute;
        public const string Minutes;
        public const string Nightmare;
        public const string OilRig;
        public const string Second;
        public const string Seconds;
        public const string Updated;
    }

}

public static class Event
{
    private const string Base;
    public const string Bradley;
    public const string CargoPlane;
    public const string CargoShip;
    public const string Chat;
    public const string ChatTeam;
    public const string Chinook;
    public const string Christmas;
    public const string Death;
    public const string Easter;
    public const string EasterWinner;
    public const string Error;
    public const string Halloween;
    public const string HalloweenWinner;
    public const string Helicopter;
    public const string Initialized;
    public const string LockedCrate;
    public const string PlayerConnected;
    public const string PlayerConnectedInfo;
    public const string PlayerDisconnected;
    public const string PlayerRespawned;
    public const string RconCommand;
    public const string RconConnection;
    public const string SantaSleigh;
    public const string ServerMessage;
    public const string Shutdown;
    public const string SupplyDrop;
    public const string SupplyDropLanded;
    public const string SupplySignal;
    public const string Team;
    public const string UserBanned;
    public const string UserKicked;
    public const string UserMuted;
    public const string UserNameUpdated;
    public const string UserUnbanned;
    public const string UserUnmuted;
}

public static class Permission
{
    private const string Base;
    public const string GroupCreated;
    public const string GroupDeleted;
    public const string UserGroupAdded;
    public const string UserGroupRemoved;
    public const string UserPermissionGranted;
    public const string UserPermissionRevoked;
}

public static class Plugin
{
    private const string Base;
    public const string AdminHammerOff;
    public const string AdminHammerOn;
    public const string AdminRadarOff;
    public const string AdminRadarOn;
    public const string ClanCreated;
    public const string ClanDisbanded;
    public const string DangerousTreasuresEnded;
    public const string DangerousTreasuresStarted;
    public const string DeathNotes;
    public const string Duel;
    public const string GodmodeOff;
    public const string GodmodeOn;
    public const string NTeleportation;
    public const string PersonalHelicopter;
    public const string PrivateMessage;
    public const string RaidableBaseCompleted;
    public const string RaidableBaseEnded;
    public const string RaidableBaseStarted;
    public const string RustKits;
    public const string TimedGroupAdded;
    public const string TimedGroupExtended;
    public const string TimedPermissionExtended;
    public const string TimedPermissionGranted;
    public const string VanishOff;
    public const string VanishOn;
}

public static class Format
{
    private const string Base;
    public const string CargoShip;
    public const string Created;
    public const string Day;
    public const string Days;
    public const string Disbanded;
    public const string Easy;
    public const string Expert;
    public const string Hard;
    public const string Hour;
    public const string Hours;
    public const string LargeOilRig;
    public const string Medium;
    public const string Minute;
    public const string Minutes;
    public const string Nightmare;
    public const string OilRig;
    public const string Second;
    public const string Seconds;
    public const string Updated;
}

public class DiscordMessage
{
    [JsonProperty("content")]
    private string Content { get; set; }
    public DiscordMessage(string content);
    public DiscordMessage AddContent(string content);
    public string GetContent();
    public string ToJson();
}


```

---

## DiscordMessages by ctv - Sends report and ban messages straight to Discord

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics.CodeAnalysis;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info( "DiscordMessages", "Slut", "2.1.8" )]
[SuppressMessage( "ReSharper", "UnusedMember.Local" )]
internal class DiscordMessages : CovalencePlugin
{
    private void OnUserChat(IPlayer player, string message);
    private void MessageCommand(IPlayer player, string command, string[] args);
    private class Data
    {
        public readonly Dictionary<string, PlayerData> Players;
    }

    private class PlayerData
    {
        public int Reports { get; set; }
        public DateTime? ReportCooldown { get; set; }
        public DateTime? MessageCooldown { get; set; }
        public bool ReportDisabled { get; set; }
    }

    public class FancyMessage
    {
        [JsonProperty( "content" )]
        private string Content { get; set; }
        [JsonProperty( "tts" )]
        private bool TextToSpeech { get; set; }
        [JsonProperty( "embeds" )]
        private EmbedBuilder[] Embeds { get; set; }
        public FancyMessage WithContent(string value);
        public FancyMessage AsTTS(bool value);
        public FancyMessage SetEmbed(EmbedBuilder value);
        public string GetContent();
        public bool IsTTS();
        public EmbedBuilder GetEmbed();
        public string ToJson();
    }

    public class EmbedBuilder
    {
        public EmbedBuilder();
        [JsonProperty( "title" )]
        private string Title { get; set; }
        [JsonProperty( "color" )]
        private int Color { get; set; }
        [JsonProperty( "fields" )]
        private List<Field> Fields { get; set; }
        [JsonProperty( "description" )]
        private string Description { get; set; }
        public EmbedBuilder WithTitle(string title);
        public EmbedBuilder WithDescription(string description);
        public EmbedBuilder SetColor(int color);
        public EmbedBuilder SetColor(string color);
        public EmbedBuilder AddInlineField(string name, object value);
        public EmbedBuilder AddField(string name, object value);
        public EmbedBuilder AddField(Field field);
        public EmbedBuilder AddFields(Field[] fields);
        public int GetColor();
        public string GetTitle();
        public Field[] GetFields();
        private int ParseColor(string input);
        public class Field
        {
            [JsonProperty( "inline" )]
            public bool Inline;
            [JsonProperty( "name" )]
            public string Name;
            [JsonProperty( "value" )]
            public object Value;
            public Field(string name, object value, bool inline);
            public Field();
        }

    }

    private abstract class Response
    {
        public int Code { get; set; }
        public string Message { get; set; }
    }

    private class BaseResponse : Response
    {
        public bool IsRatelimit { get; set; }
        public bool IsOk { get; set; }
        public bool IsBad { get; set; }
        public RateLimitResponse GetRateLimit();
    }

    private class Request
    {
        private static bool _rateLimited;
        private static bool _busy;
        private static Queue<Request> _requestQueue;
        private readonly string _payload;
        private readonly Plugin _plugin;
        private readonly Action<BaseResponse> _response;
        private readonly string _url;
        public static void Init();
        private Request(string url, FancyMessage message, Action<BaseResponse> response, Plugin plugin);
        private Request(string url, FancyMessage message, Plugin plugin);
        private static void SendNextRequest();
        private static void EnqueueRequest(Request request);
        private void Send();
        private static void OnRateLimit(int retryAfter);
        private static void OnRateLimitEnd();
        public static void Send(string url, FancyMessage message, Plugin plugin);
        public static void Send(string url, FancyMessage message, Action<BaseResponse> callback, Plugin plugin);
        public static void Dispose();
    }

    private class RateLimitResponse : BaseResponse
    {
        [JsonProperty( "retry_after" )]
        public int RetryAfter { get; set; }
    }

    private Configuration _config;
    private class Configuration
    {
        public General GeneralSettings { get; set; }
        public Ban BanSettings { get; set; }
        public Report ReportSettings { get; set; }
        public Message MessageSettings { get; set; }
        public Chat ChatSettings { get; set; }
        public Mute MuteSettings { get; set; }
        [JsonIgnore]
        public Dictionary<FeatureType, WebhookObject> FeatureTypes { get; set; }
        public static Configuration Defaults();
        public class General
        {
            public bool Announce { get; set; }
        }

        public class Ban : EmbedObject
        {
        }

        public class Message : EmbedObject
        {
            public bool LogToConsole { get; set; }
            public bool SuggestAlias { get; set; }
            public string Alert { get; set; }
            public int Cooldown { get; set; }
        }

        public class Report : EmbedObject
        {
            public bool LogToConsole { get; set; }
            public string Alert { get; set; }
            public int Cooldown { get; set; }
        }

        public class Chat : WebhookObject
        {
            public bool TextToSpeech { get; set; }
        }

        public class Mute : EmbedObject
        {
        }

        public class EmbedObject : WebhookObject
        {
            public string Color { get; set; }
        }

        public class WebhookObject
        {
            public bool Enabled { get; set; }
            public string WebhookUrl { get; set; }
        }

    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private T GetFeatureConfig(FeatureType type);
    private Data _data;
    [PluginReference]
    private readonly Plugin BetterChatMute;
    private readonly Plugin AdminChat;
    private static DiscordMessages _instance;
    private readonly JsonSerializerSettings _jsonSettings;
    private readonly Dictionary<string, string> _headers;
    private void Loaded();
    private void CheckHooks();
    private void Unload();
    private void OnServerSave();
    private void RegisterCommands();
    protected override void LoadDefaultMessages();
    private const string BanPermission;
    private const string ReportPermission;
    private const string MessagePermission;
    private const string AdminPermission;
    private void RegisterPermissions();
    private void API_SendFancyMessage(string webhookUrl, string content, string embedJsonString, Plugin plugin);
    private void API_SendFancyMessage(string webhookUrl, string embedName, int embedColor, string json, string content, Plugin plugin);
    private void API_SendFancyMessage(string webhookUrl, string embedName, string json, string content, int embedColor, Plugin plugin);
    private void API_SendTextMessage(string webhookUrl, string content, bool tts, Plugin plugin);
    private void ReportAdminCommand(IPlayer player, string command, string[] args);
    private void ReportCommand(IPlayer player, string command, string[] args);
    private static string FormatTime(TimeSpan time);
    private void OnBetterChatTimeMuted(IPlayer target, IPlayer player, TimeSpan expireDate, string reason);
    private void OnBetterChatMuted(IPlayer target, IPlayer player, string reason);
    private void SendMute(IPlayer target, IPlayer player, TimeSpan expireDate, bool timed, string reason);
    private bool _banFromCommand;
    private void BanCommand(IPlayer player, string command, string[] args);
    private void ExecuteBan(IPlayer target, IPlayer player, string reason);
    private void OnUserBanned(string name, string bannedId, string address, string reason, long expiry, IPlayer source);
    private string GetLang(string key, string id, object[] args);
    private void SendMessage(IPlayer player, string message);
    private bool OnCooldown(IPlayer player, CooldownType type, int secondsRemaining);
    private void SaveData();
    private void LoadData();
    private IPlayer GetPlayer(string nameOrId, IPlayer player, bool sendError);
}

private class Data
{
    public readonly Dictionary<string, PlayerData> Players;
}

private class PlayerData
{
    public int Reports { get; set; }
    public DateTime? ReportCooldown { get; set; }
    public DateTime? MessageCooldown { get; set; }
    public bool ReportDisabled { get; set; }
}

public class FancyMessage
{
    [JsonProperty( "content" )]
    private string Content { get; set; }
    [JsonProperty( "tts" )]
    private bool TextToSpeech { get; set; }
    [JsonProperty( "embeds" )]
    private EmbedBuilder[] Embeds { get; set; }
    public FancyMessage WithContent(string value);
    public FancyMessage AsTTS(bool value);
    public FancyMessage SetEmbed(EmbedBuilder value);
    public string GetContent();
    public bool IsTTS();
    public EmbedBuilder GetEmbed();
    public string ToJson();
}

public class EmbedBuilder
{
    public EmbedBuilder();
    [JsonProperty( "title" )]
    private string Title { get; set; }
    [JsonProperty( "color" )]
    private int Color { get; set; }
    [JsonProperty( "fields" )]
    private List<Field> Fields { get; set; }
    [JsonProperty( "description" )]
    private string Description { get; set; }
    public EmbedBuilder WithTitle(string title);
    public EmbedBuilder WithDescription(string description);
    public EmbedBuilder SetColor(int color);
    public EmbedBuilder SetColor(string color);
    public EmbedBuilder AddInlineField(string name, object value);
    public EmbedBuilder AddField(string name, object value);
    public EmbedBuilder AddField(Field field);
    public EmbedBuilder AddFields(Field[] fields);
    public int GetColor();
    public string GetTitle();
    public Field[] GetFields();
    private int ParseColor(string input);
    public class Field
    {
        [JsonProperty( "inline" )]
        public bool Inline;
        [JsonProperty( "name" )]
        public string Name;
        [JsonProperty( "value" )]
        public object Value;
        public Field(string name, object value, bool inline);
        public Field();
    }

}

public class Field
{
    [JsonProperty( "inline" )]
    public bool Inline;
    [JsonProperty( "name" )]
    public string Name;
    [JsonProperty( "value" )]
    public object Value;
    public Field(string name, object value, bool inline);
    public Field();
}

private abstract class Response
{
    public int Code { get; set; }
    public string Message { get; set; }
}

private class BaseResponse : Response
{
    public bool IsRatelimit { get; set; }
    public bool IsOk { get; set; }
    public bool IsBad { get; set; }
    public RateLimitResponse GetRateLimit();
}

private class Request
{
    private static bool _rateLimited;
    private static bool _busy;
    private static Queue<Request> _requestQueue;
    private readonly string _payload;
    private readonly Plugin _plugin;
    private readonly Action<BaseResponse> _response;
    private readonly string _url;
    public static void Init();
    private Request(string url, FancyMessage message, Action<BaseResponse> response, Plugin plugin);
    private Request(string url, FancyMessage message, Plugin plugin);
    private static void SendNextRequest();
    private static void EnqueueRequest(Request request);
    private void Send();
    private static void OnRateLimit(int retryAfter);
    private static void OnRateLimitEnd();
    public static void Send(string url, FancyMessage message, Plugin plugin);
    public static void Send(string url, FancyMessage message, Action<BaseResponse> callback, Plugin plugin);
    public static void Dispose();
}

private class RateLimitResponse : BaseResponse
{
    [JsonProperty( "retry_after" )]
    public int RetryAfter { get; set; }
}

private class Configuration
{
    public General GeneralSettings { get; set; }
    public Ban BanSettings { get; set; }
    public Report ReportSettings { get; set; }
    public Message MessageSettings { get; set; }
    public Chat ChatSettings { get; set; }
    public Mute MuteSettings { get; set; }
    [JsonIgnore]
    public Dictionary<FeatureType, WebhookObject> FeatureTypes { get; set; }
    public static Configuration Defaults();
    public class General
    {
        public bool Announce { get; set; }
    }

    public class Ban : EmbedObject
    {
    }

    public class Message : EmbedObject
    {
        public bool LogToConsole { get; set; }
        public bool SuggestAlias { get; set; }
        public string Alert { get; set; }
        public int Cooldown { get; set; }
    }

    public class Report : EmbedObject
    {
        public bool LogToConsole { get; set; }
        public string Alert { get; set; }
        public int Cooldown { get; set; }
    }

    public class Chat : WebhookObject
    {
        public bool TextToSpeech { get; set; }
    }

    public class Mute : EmbedObject
    {
    }

    public class EmbedObject : WebhookObject
    {
        public string Color { get; set; }
    }

    public class WebhookObject
    {
        public bool Enabled { get; set; }
        public string WebhookUrl { get; set; }
    }

}

public class General
{
    public bool Announce { get; set; }
}

public class Ban : EmbedObject
{
}

public class Message : EmbedObject
{
    public bool LogToConsole { get; set; }
    public bool SuggestAlias { get; set; }
    public string Alert { get; set; }
    public int Cooldown { get; set; }
}

public class Report : EmbedObject
{
    public bool LogToConsole { get; set; }
    public string Alert { get; set; }
    public int Cooldown { get; set; }
}

public class Chat : WebhookObject
{
    public bool TextToSpeech { get; set; }
}

public class Mute : EmbedObject
{
}

public class EmbedObject : WebhookObject
{
    public string Color { get; set; }
}

public class WebhookObject
{
    public bool Enabled { get; set; }
    public string WebhookUrl { get; set; }
}


```

---

## DiscordMessagesChat by ctv - Relays chat to Discord using configurable webhooks for both global and team chat

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Discord Messages Chat", "Slut", "1.0.4")]
[Description("Relay global / team chat to Discord!")]
public class DiscordMessagesChat : RustPlugin
{
    [PluginReference]
    private Plugin DiscordMessages;
    private Plugin BetterChatMute;
    private bool _teamChatEnabled;
    private bool _globalChatEnabled;
    private readonly object _falseObject;
    private Configuration _configuration;
    private class Configuration
    {
        public string GlobalChatWebhook;
        public string TeamChatWebhook;
        public bool AllowMutedPlayers;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void OnPlayerChat(BasePlayer player, string message, ConVar.Chat.ChatChannel channel);
    private void CheckWebhook(string webhookUrl, Action<bool> success);
}

private class Configuration
{
    public string GlobalChatWebhook;
    public string TeamChatWebhook;
    public bool AllowMutedPlayers;
}


```

---

## DiscordPlayers by MJSU - Adds a /players chat command to Discord with the currently connected players

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord.Attributes;
using Oxide.Ext.Discord.Builders;
using Oxide.Ext.Discord.Cache;
using Oxide.Ext.Discord.Clients;
using Oxide.Ext.Discord.Connections;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Extensions;
using Oxide.Ext.Discord.Interfaces;
using Oxide.Ext.Discord.Libraries;
using Oxide.Ext.Discord.Logging;
using Oxide.Ext.Discord.Types;
using ProtoBuf;
using System;
using System.Buffers;
using System.Collections.Generic;
using System.ComponentModel;
using System.IO;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Discord Players", "MJSU", "3.0.0")]
[Description("Displays online players in discord")]
public partial class DiscordPlayers : CovalencePlugin, IDiscordPlugin, IDiscordPool
{
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    [HookMethod(DiscordExtHooks.OnDiscordGatewayReady)]
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
    public void CreateApplicationCommand(CommandSettings settings);
    [HookMethod(DiscordExtHooks.OnDiscordGuildCreated)]
    private void OnDiscordGuildCreated(DiscordGuild created);
    private void CreatePermanentMessage(PermanentMessageSettings config, DiscordChannel channel);
    private void HandleApplicationCommand(DiscordInteraction interaction, InteractionDataParsed parsed);
    [DiscordMessageComponentCommand(BackCommand)]
    private void HandleBackCommand(DiscordInteraction interaction);
    [DiscordMessageComponentCommand(RefreshCommand)]
    private void HandleRefreshCommand(DiscordInteraction interaction);
    [DiscordMessageComponentCommand(ForwardCommand)]
    private void HandleForwardCommand(DiscordInteraction interaction);
    [DiscordMessageComponentCommand(ChangeSort)]
    private void HandleChangeSortCommand(DiscordInteraction interaction);
    private void HandleUpdate(DiscordInteraction interaction, MessageCache cache);
    public DiscordClient Client { get; set; }
    public DiscordPluginPool Pool { get; set; }
    private PluginConfig _pluginConfig;
    private PluginData _pluginData;
    private readonly DiscordAppCommand _appCommand;
    private readonly DiscordPlaceholders _placeholders;
    private readonly DiscordMessageTemplates _templates;
    private readonly DiscordEmbedTemplates _embed;
    private readonly DiscordEmbedFieldTemplates _field;
    private readonly DiscordCommandLocalizations _localizations;
    private readonly BotConnection _discordSettings;
    private readonly Hash<Snowflake, MessageCache> _messageCache;
    private readonly Hash<string, BaseMessageSettings> _commandCache;
    private readonly OnlinePlayerCache _playerCache;
    private readonly Hash<Snowflake, PermanentMessageHandler> _permanentHandler;
    private const string BaseCommand;
    private const string BackCommand;
    private const string RefreshCommand;
    private const string ForwardCommand;
    private const string ChangeSort;
    public static DiscordPlayers Instance;
    public PluginTimers Timer { get; set; }
    private const string PluginIcon;
    public MessageCache GetCache(DiscordInteraction interaction);
    public void SendResponse(DiscordInteraction interaction, TemplateKey templateName, PlaceholderData data, MessageFlags flags);
    public string Lang(string key);
    public string Lang(string key, object[] args);
    private void SaveData();
    public new void PrintError(string format, object[] args);
    protected override void LoadDefaultMessages();
    public void CreateMessage(MessageCache cache, DiscordInteraction interaction, T create, Action<T> callback);
    public List<IPlayer> GetPlayerList(MessageCache cache);
    public T CreateMessage(BaseMessageSettings settings, PlaceholderData data, DiscordInteraction interaction, T message);
    public void SetButtonState(IDiscordMessageTemplate message, string command, bool enabled);
    public DiscordEmbed CreateEmbeds(BaseMessageSettings settings, PlaceholderData data, DiscordInteraction interaction);
    public IPromise<List<EmbedField>> CreateFields(MessageCache cache, PlaceholderData data, DiscordInteraction interaction, List<IPlayer> onlineList);
    public void ProcessEmbeds(DiscordEmbed embed, List<EmbedField> fields);
    public void RegisterPlaceholders();
    public int GetPage(MessageState embed);
    public string GetSort(PlaceholderState state, MessageState embed);
    public PlaceholderData CloneForPlayer(PlaceholderData source, IPlayer player, int index);
    public PlaceholderData GetDefault(DiscordInteraction interaction);
    public PlaceholderData GetDefault(MessageCache cache, DiscordInteraction interaction);
    public PlaceholderData GetDefault(MessageCache cache, DiscordInteraction interaction, int maxPage);
    private void Init();
    private void OnServerInitialized();
    private void OnUserConnected(IPlayer player);
    private void OnUserDisconnected(IPlayer player);
    private void Unload();
    public void RegisterTemplates();
    private void CreateCommandTemplates(BaseMessageSettings command, DiscordEmbedFieldTemplate @default, bool isGlobal);
    public void RegisterTemplate(BaseMessageTemplateLibrary<TTemplate> library, TemplateKey name, TTemplate template, bool isGlobal, TemplateVersion version, TemplateVersion minVersion);
    public DiscordMessageTemplate CreateBaseMessage();
    public DiscordEmbedTemplate GetDefaultEmbedTemplate();
    public DiscordEmbedFieldTemplate GetDefaultFieldTemplate();
    public DiscordEmbedFieldTemplate GetDefaultAdminFieldTemplate();
    public DiscordMessageTemplate CreateTemplateEmbed(string description, DiscordColor color);
    public class MessageCache
    {
        public readonly BaseMessageSettings Settings;
        public readonly MessageState State;
        public MessageCache(BaseMessageSettings settings, MessageState state);
    }

    public class OnlinePlayerCache
    {
        private readonly PlayerListCache _byNameCache;
        private readonly PlayerListCache _byOnlineTime;
        private readonly Hash<string, DateTime> _onlineSince;
        public OnlinePlayerCache();
        public void Initialize(IEnumerable<IPlayer> connected);
        public TimeSpan GetOnlineDuration(IPlayer player);
        public List<IPlayer> GetList(SortBy sort, bool includeAdmin);
        public void OnUserConnected(IPlayer player);
        public void OnUserDisconnected(IPlayer player);
         class NameComparer : IComparer<IPlayer>
        {
            public int Compare(IPlayer x, IPlayer y);
        }

         class OnlineSinceComparer : IComparer<IPlayer>
        {
            private readonly Hash<string, DateTime> _onlineSince;
            public OnlineSinceComparer(Hash<string, DateTime> onlineSince);
            public int Compare(IPlayer x, IPlayer y);
        }

    }

    public class PlayerListCache
    {
        private readonly List<IPlayer> _allList;
        private readonly List<IPlayer> _nonAdminList;
        private readonly IComparer<IPlayer> _comparer;
        public PlayerListCache(IComparer<IPlayer> comparer);
        public void Add(IPlayer player);
        public void Insert(List<IPlayer> list, IPlayer player);
        public void Remove(IPlayer player);
        public List<IPlayer> GetList(bool includeAdmin);
    }

    public abstract class BaseMessageSettings
    {
        [JsonProperty(PropertyName = "Display Admins In The Player List", Order = 1001)]
        public bool ShowAdmins { get; set; }
        [DefaultValue(25)]
        [JsonProperty(PropertyName = "Players Per Embed (0 - 25)", Order = 1002)]
        public int EmbedFieldLimit { get; set; }
        public abstract bool IsPermanent();
        public abstract TemplateKey GetTemplateName();
        [JsonConstructor]
        public BaseMessageSettings();
        public BaseMessageSettings(BaseMessageSettings settings);
    }

    public class CommandSettings : BaseMessageSettings
    {
        [JsonProperty(PropertyName = "Command Name (Must Be Unique)")]
        public string Command { get; set; }
        [JsonProperty(PropertyName = "Allow Command In Direct Messages")]
        public bool AllowInDm { get; set; }
        [JsonConstructor]
        public CommandSettings();
        public CommandSettings(CommandSettings settings);
        public override bool IsPermanent();
        public override TemplateKey GetTemplateName();
    }

    public class PermanentMessageSettings : BaseMessageSettings
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Template Name (Must Be Unique)")]
        public string TemplateName { get; set; }
        [JsonProperty(PropertyName = "Permanent Message Channel ID")]
        public Snowflake ChannelId { get; set; }
        [JsonProperty(PropertyName = "Update Rate (Minutes)")]
        public float UpdateRate { get; set; }
        [JsonConstructor]
        public PermanentMessageSettings();
        public PermanentMessageSettings(PermanentMessageSettings settings);
        public override bool IsPermanent();
        public override TemplateKey GetTemplateName();
    }

    public class PluginConfig
    {
        [DefaultValue("")]
        [JsonProperty(PropertyName = "Discord Bot Token")]
        public string DiscordApiKey { get; set; }
        [JsonProperty(PropertyName = "Command Messages")]
        public List<CommandSettings> CommandMessages { get; set; }
        [JsonProperty(PropertyName = "Permanent Messages")]
        public List<PermanentMessageSettings> Permanent { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DiscordLogLevel.Info)]
        [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel ExtensionDebugging { get; set; }
    }

    public class PermanentMessageData
    {
        public Snowflake MessageId { get; set; }
    }

    public class PluginData
    {
        public Hash<string, PermanentMessageData> PermanentMessageIds;
        public Hash<string, Snowflake> RegisteredCommands;
        public PermanentMessageData GetPermanentMessage(PermanentMessageSettings config);
        public void SetPermanentMessage(PermanentMessageSettings config, PermanentMessageData data);
    }

    public class PermanentMessageHandler
    {
        private readonly DiscordClient _client;
        private readonly MessageCache _cache;
        private readonly DiscordMessage _message;
        private readonly MessageUpdate _update;
        private readonly Timer _timer;
        private DateTime _lastUpdate;
        public PermanentMessageHandler(DiscordClient client, MessageCache cache, float updateRate, DiscordMessage message);
        private void SendUpdate();
    }

    public static class LangKeys
    {
        public const string SortByEnumName;
        public const string SortByEnumTime;
    }

    public static class PlaceholderDataKeys
    {
        public static readonly PlaceholderDataKey CommandId;
        public static readonly PlaceholderDataKey CommandName;
        public static readonly PlaceholderDataKey PlayerIndex;
        public static readonly PlaceholderDataKey PlayerDuration;
        public static readonly PlaceholderDataKey MaxPage;
        public static readonly PlaceholderDataKey MessageState;
    }

    public class PlaceholderKeys
    {
        public static readonly PlaceholderKey PlayerIndex;
        public static readonly PlaceholderKey Page;
        public static readonly PlaceholderKey SortState;
        public static readonly PlaceholderKey CommandId;
        public static readonly PlaceholderKey CommandName;
        public static readonly PlaceholderKey MaxPage;
    }

    [ProtoContract]
    public class MessageState
    {
        [ProtoMember(1)]
        public short Page;
        [ProtoMember(2)]
        public SortBy Sort;
        [ProtoMember(3)]
        public string Command;
        private MessageState();
        public static MessageState CreateNew(TemplateKey command);
        public static MessageState Create(ReadOnlySpan<char> base64);
        public string CreateBase64String();
        public void NextPage();
        public void PreviousPage();
        public void ClampPage(short maxPage);
        public void NextSort();
        public override string ToString();
    }

    public static class TemplateKeys
    {
        public static class Errors
        {
            private const string Base;
            public static readonly TemplateKey UnknownState;
            public static readonly TemplateKey UnknownCommand;
        }

    }

}

public class MessageCache
{
    public readonly BaseMessageSettings Settings;
    public readonly MessageState State;
    public MessageCache(BaseMessageSettings settings, MessageState state);
}

public class OnlinePlayerCache
{
    private readonly PlayerListCache _byNameCache;
    private readonly PlayerListCache _byOnlineTime;
    private readonly Hash<string, DateTime> _onlineSince;
    public OnlinePlayerCache();
    public void Initialize(IEnumerable<IPlayer> connected);
    public TimeSpan GetOnlineDuration(IPlayer player);
    public List<IPlayer> GetList(SortBy sort, bool includeAdmin);
    public void OnUserConnected(IPlayer player);
    public void OnUserDisconnected(IPlayer player);
     class NameComparer : IComparer<IPlayer>
    {
        public int Compare(IPlayer x, IPlayer y);
    }

     class OnlineSinceComparer : IComparer<IPlayer>
    {
        private readonly Hash<string, DateTime> _onlineSince;
        public OnlineSinceComparer(Hash<string, DateTime> onlineSince);
        public int Compare(IPlayer x, IPlayer y);
    }

}

 class NameComparer : IComparer<IPlayer>
{
    public int Compare(IPlayer x, IPlayer y);
}

 class OnlineSinceComparer : IComparer<IPlayer>
{
    private readonly Hash<string, DateTime> _onlineSince;
    public OnlineSinceComparer(Hash<string, DateTime> onlineSince);
    public int Compare(IPlayer x, IPlayer y);
}

public class PlayerListCache
{
    private readonly List<IPlayer> _allList;
    private readonly List<IPlayer> _nonAdminList;
    private readonly IComparer<IPlayer> _comparer;
    public PlayerListCache(IComparer<IPlayer> comparer);
    public void Add(IPlayer player);
    public void Insert(List<IPlayer> list, IPlayer player);
    public void Remove(IPlayer player);
    public List<IPlayer> GetList(bool includeAdmin);
}

public abstract class BaseMessageSettings
{
    [JsonProperty(PropertyName = "Display Admins In The Player List", Order = 1001)]
    public bool ShowAdmins { get; set; }
    [DefaultValue(25)]
    [JsonProperty(PropertyName = "Players Per Embed (0 - 25)", Order = 1002)]
    public int EmbedFieldLimit { get; set; }
    public abstract bool IsPermanent();
    public abstract TemplateKey GetTemplateName();
    [JsonConstructor]
    public BaseMessageSettings();
    public BaseMessageSettings(BaseMessageSettings settings);
}

public class CommandSettings : BaseMessageSettings
{
    [JsonProperty(PropertyName = "Command Name (Must Be Unique)")]
    public string Command { get; set; }
    [JsonProperty(PropertyName = "Allow Command In Direct Messages")]
    public bool AllowInDm { get; set; }
    [JsonConstructor]
    public CommandSettings();
    public CommandSettings(CommandSettings settings);
    public override bool IsPermanent();
    public override TemplateKey GetTemplateName();
}

public class PermanentMessageSettings : BaseMessageSettings
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Template Name (Must Be Unique)")]
    public string TemplateName { get; set; }
    [JsonProperty(PropertyName = "Permanent Message Channel ID")]
    public Snowflake ChannelId { get; set; }
    [JsonProperty(PropertyName = "Update Rate (Minutes)")]
    public float UpdateRate { get; set; }
    [JsonConstructor]
    public PermanentMessageSettings();
    public PermanentMessageSettings(PermanentMessageSettings settings);
    public override bool IsPermanent();
    public override TemplateKey GetTemplateName();
}

public class PluginConfig
{
    [DefaultValue("")]
    [JsonProperty(PropertyName = "Discord Bot Token")]
    public string DiscordApiKey { get; set; }
    [JsonProperty(PropertyName = "Command Messages")]
    public List<CommandSettings> CommandMessages { get; set; }
    [JsonProperty(PropertyName = "Permanent Messages")]
    public List<PermanentMessageSettings> Permanent { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DiscordLogLevel.Info)]
    [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel ExtensionDebugging { get; set; }
}

public class PermanentMessageData
{
    public Snowflake MessageId { get; set; }
}

public class PluginData
{
    public Hash<string, PermanentMessageData> PermanentMessageIds;
    public Hash<string, Snowflake> RegisteredCommands;
    public PermanentMessageData GetPermanentMessage(PermanentMessageSettings config);
    public void SetPermanentMessage(PermanentMessageSettings config, PermanentMessageData data);
}

public class PermanentMessageHandler
{
    private readonly DiscordClient _client;
    private readonly MessageCache _cache;
    private readonly DiscordMessage _message;
    private readonly MessageUpdate _update;
    private readonly Timer _timer;
    private DateTime _lastUpdate;
    public PermanentMessageHandler(DiscordClient client, MessageCache cache, float updateRate, DiscordMessage message);
    private void SendUpdate();
}

public static class LangKeys
{
    public const string SortByEnumName;
    public const string SortByEnumTime;
}

public static class PlaceholderDataKeys
{
    public static readonly PlaceholderDataKey CommandId;
    public static readonly PlaceholderDataKey CommandName;
    public static readonly PlaceholderDataKey PlayerIndex;
    public static readonly PlaceholderDataKey PlayerDuration;
    public static readonly PlaceholderDataKey MaxPage;
    public static readonly PlaceholderDataKey MessageState;
}

public class PlaceholderKeys
{
    public static readonly PlaceholderKey PlayerIndex;
    public static readonly PlaceholderKey Page;
    public static readonly PlaceholderKey SortState;
    public static readonly PlaceholderKey CommandId;
    public static readonly PlaceholderKey CommandName;
    public static readonly PlaceholderKey MaxPage;
}

[ProtoContract]
public class MessageState
{
    [ProtoMember(1)]
    public short Page;
    [ProtoMember(2)]
    public SortBy Sort;
    [ProtoMember(3)]
    public string Command;
    private MessageState();
    public static MessageState CreateNew(TemplateKey command);
    public static MessageState Create(ReadOnlySpan<char> base64);
    public string CreateBase64String();
    public void NextPage();
    public void PreviousPage();
    public void ClampPage(short maxPage);
    public void NextSort();
    public override string ToString();
}

public static class TemplateKeys
{
    public static class Errors
    {
        private const string Base;
        public static readonly TemplateKey UnknownState;
        public static readonly TemplateKey UnknownCommand;
    }

}

public static class Errors
{
    private const string Base;
    public static readonly TemplateKey UnknownState;
    public static readonly TemplateKey UnknownCommand;
}


```

---

## DiscordPM by MJSU - Allows players to private message between the game server and Discord

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord.Attributes;
using Oxide.Ext.Discord.Builders;
using Oxide.Ext.Discord.Cache;
using Oxide.Ext.Discord.Clients;
using Oxide.Ext.Discord.Connections;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Extensions;
using Oxide.Ext.Discord.Interfaces;
using Oxide.Ext.Discord.Libraries;
using Oxide.Ext.Discord.Logging;
using Oxide.Ext.Discord.Types;

Oxide.Plugins
[Info("Discord PM", "MJSU", "3.0.0")]
[Description("Allows private messaging through discord")]
internal class DiscordPM : CovalencePlugin, IDiscordPlugin, IDiscordPool
{
    public DiscordClient Client { get; set; }
    public DiscordPluginPool Pool { get; set; }
    private PluginConfig _pluginConfig;
    private const string AccentColor;
    private const string PmCommand;
    private const string ReplyCommand;
    private const string NameArg;
    private const string MessageArg;
    private readonly DiscordPlaceholders _placeholders;
    private readonly DiscordMessageTemplates _templates;
    private readonly DiscordCommandLocalizations _localizations;
    private readonly Hash<string, IPlayer> _replies;
    private readonly BotConnection _discordSettings;
    private DiscordChannel _logChannel;
    private DiscordApplicationCommand _pmCommand;
    private void Init();
    [HookMethod(DiscordExtHooks.OnDiscordClientCreated)]
    private void OnDiscordClientCreated();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void DiscordPmChatCommand(IPlayer player, string cmd, string[] args);
    private void DiscordPmChatReplyCommand(IPlayer player, string cmd, string[] args);
    public void SendPrivateMessageFromServer(IPlayer sender, IPlayer target, string message);
    [HookMethod(DiscordExtHooks.OnDiscordGatewayReady)]
    private void OnDiscordGatewayReady();
    [HookMethod(DiscordExtHooks.OnDiscordGuildCreated)]
    private void OnDiscordGuildCreated(DiscordGuild guild);
    public void RegisterApplicationCommands();
    public void CreatePmCommand();
    public void CreateReplyCommand();
    public void AddCommandNameOption(ApplicationCommandBuilder builder);
    public void AddCommandMessageOption(ApplicationCommandBuilder builder);
    [DiscordApplicationCommand(PmCommand)]
    private void HandlePmCommand(DiscordInteraction interaction, InteractionDataParsed parsed);
    [DiscordApplicationCommand(ReplyCommand)]
    private void HandleReplyCommand(DiscordInteraction interaction, InteractionDataParsed parsed);
    public void SendPrivateMessageFromDiscord(DiscordInteraction interaction, IPlayer player, IPlayer target, string message);
    [DiscordAutoCompleteCommand(PmCommand, NameArg)]
    private void HandleNameAutoComplete(DiscordInteraction interaction, InteractionDataOption focused);
    public InteractionCallbackData GetInteractionCallback(DiscordInteraction interaction);
    public void RegisterPlaceholders();
    public PlaceholderData GetPmDefault(IPlayer from, IPlayer to, string message);
    public PlaceholderData GetDefault();
    public void RegisterGlobalTemplates();
    public void RegisterEnTemplates();
    public void RegisterRuTemplates();
    public DiscordMessageTemplate CreateTemplateEmbed(string description, DiscordColor color);
    public DiscordMessageTemplate CreatePrefixedTemplateEmbed(string description, DiscordColor color);
    public bool TryFindPlayer(IPlayer from, string name, IPlayer target);
    public void SendPlayerPrivateMessage(IPlayer player, string serverLang, TemplateKey templateKey, PlaceholderData data);
    public void LogPrivateMessage(PlaceholderData data);
    public void SendEffectToPlayer(IPlayer player);
    public void ServerPrivateMessage(IPlayer player, string langKey, PlaceholderData data);
    public void DiscordPrivateMessage(IPlayer player, TemplateKey templateKey, PlaceholderData data);
    public void RegisterServerLangCommand(string command, string langKey);
    public void Chat(IPlayer player, string key, PlaceholderData data);
    public string Lang(string key, IPlayer player);
    public string Lang(string key, IPlayer player, PlaceholderData data);
    private class PluginConfig
    {
        [DefaultValue("")]
        [JsonProperty(PropertyName = "Discord Bot Token")]
        public string DiscordApiKey { get; set; }
        [DefaultValue(true)]
        [JsonProperty(PropertyName = "Allow Discord Commands In Direct Messages")]
        public bool AllowInDm { get; set; }
        [JsonProperty(PropertyName = "Log Settings")]
        public LogSettings Log { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DiscordLogLevel.Info)]
        [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel ExtensionDebugging { get; set; }
    }

    public class LogSettings
    {
        [JsonProperty(PropertyName = "Log To Console")]
        public bool LogToConsole { get; set; }
        [JsonProperty(PropertyName = "Log To File")]
        public bool LogToFile { get; set; }
        [JsonProperty(PropertyName = "Log To Channel ID")]
        public Snowflake LogToChannelId { get; set; }
        public LogSettings(LogSettings settings);
    }

    private static class LangKeys
    {
        public const string Base;
        public const string Chat;
        public const string FromFormat;
        public const string ToFormat;
        public const string InvalidPmSyntax;
        public const string InvalidReplySyntax;
        public const string NoPreviousPm;
        public const string MultiplePlayersFound;
        public const string NoPlayersFound;
        public const string LogFormat;
        public const string ChatPmCommand;
        public const string ChatReplyCommand;
    }

    private static class TemplateKeys
    {
        public static class Messages
        {
            private const string Base;
            public static readonly TemplateKey To;
            public static readonly TemplateKey From;
            public static readonly TemplateKey Log;
        }

        public static class Errors
        {
            private const string Base;
            public static readonly TemplateKey UnlinkedUser;
            public static readonly TemplateKey InvalidAutoCompleteSelection;
            public static readonly TemplateKey NoPreviousPm;
        }

    }

    private static class PlaceholderKeys
    {
        public static readonly PlaceholderKey Message;
        public static readonly PlaceholderKey Chat;
        public static readonly PlaceholderKey NotFound;
    }

    private class PlaceholderDataKeys
    {
        public static readonly PlaceholderDataKey Message;
        public static readonly PlaceholderDataKey PlayerName;
        public static readonly PlaceholderDataKey Chat;
    }

}

private class PluginConfig
{
    [DefaultValue("")]
    [JsonProperty(PropertyName = "Discord Bot Token")]
    public string DiscordApiKey { get; set; }
    [DefaultValue(true)]
    [JsonProperty(PropertyName = "Allow Discord Commands In Direct Messages")]
    public bool AllowInDm { get; set; }
    [JsonProperty(PropertyName = "Log Settings")]
    public LogSettings Log { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DiscordLogLevel.Info)]
    [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel ExtensionDebugging { get; set; }
}

public class LogSettings
{
    [JsonProperty(PropertyName = "Log To Console")]
    public bool LogToConsole { get; set; }
    [JsonProperty(PropertyName = "Log To File")]
    public bool LogToFile { get; set; }
    [JsonProperty(PropertyName = "Log To Channel ID")]
    public Snowflake LogToChannelId { get; set; }
    public LogSettings(LogSettings settings);
}

private static class LangKeys
{
    public const string Base;
    public const string Chat;
    public const string FromFormat;
    public const string ToFormat;
    public const string InvalidPmSyntax;
    public const string InvalidReplySyntax;
    public const string NoPreviousPm;
    public const string MultiplePlayersFound;
    public const string NoPlayersFound;
    public const string LogFormat;
    public const string ChatPmCommand;
    public const string ChatReplyCommand;
}

private static class TemplateKeys
{
    public static class Messages
    {
        private const string Base;
        public static readonly TemplateKey To;
        public static readonly TemplateKey From;
        public static readonly TemplateKey Log;
    }

    public static class Errors
    {
        private const string Base;
        public static readonly TemplateKey UnlinkedUser;
        public static readonly TemplateKey InvalidAutoCompleteSelection;
        public static readonly TemplateKey NoPreviousPm;
    }

}

public static class Messages
{
    private const string Base;
    public static readonly TemplateKey To;
    public static readonly TemplateKey From;
    public static readonly TemplateKey Log;
}

public static class Errors
{
    private const string Base;
    public static readonly TemplateKey UnlinkedUser;
    public static readonly TemplateKey InvalidAutoCompleteSelection;
    public static readonly TemplateKey NoPreviousPm;
}

private static class PlaceholderKeys
{
    public static readonly PlaceholderKey Message;
    public static readonly PlaceholderKey Chat;
    public static readonly PlaceholderKey NotFound;
}

private class PlaceholderDataKeys
{
    public static readonly PlaceholderDataKey Message;
    public static readonly PlaceholderDataKey PlayerName;
    public static readonly PlaceholderDataKey Chat;
}


```

---

## DiscordPresence by MJSU - Updates the Discord bot status with server information

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord.Clients;
using Oxide.Ext.Discord.Connections;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Interfaces;
using Oxide.Ext.Discord.Libraries;
using Oxide.Ext.Discord.Logging;

Oxide.Plugins
[Info("Discord Presence", "MJSU", "3.0.1")]
[Description("Updates the Discord bot status message")]
internal class DiscordPresence : CovalencePlugin, IDiscordPlugin
{
    public DiscordClient Client { get; set; }
    private PluginConfig _pluginConfig;
    private int _index;
    private readonly DiscordPlaceholders _placeholders;
    private readonly UpdatePresenceCommand _command;
    private readonly DiscordActivity _activity;
    private Action _updatePresence;
    private bool _serverInit;
    private bool _gatewayReady;
    private DateTime _nextApiSend;
    private void Init();
    [HookMethod(DiscordExtHooks.OnDiscordClientCreated)]
    private void OnDiscordClientCreated();
    [HookMethod(DiscordExtHooks.OnDiscordGatewayReady)]
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    public PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void OnUserConnected(IPlayer player);
    private void OnUserDisconnected(IPlayer player);
    private void OnDiscordGatewayResumed();
    private void OnDiscordGatewayReconnected();
    public void SendLoadingMessage();
    public void UpdatePresence();
    public void SendUpdate(MessageSettings settings);
    public void SendUpdate(string message, ActivityType type);
    public PlaceholderData GetDefault();
    private void API_SendUpdateMessage(string message, int activity);
    public class PluginConfig
    {
        [DefaultValue("")]
        [JsonProperty(PropertyName = "Discord Application Bot Token")]
        public string Token { get; set; }
        [DefaultValue(true)]
        [JsonProperty(PropertyName = "Enable Sending Message Per Update Rate")]
        public bool EnableUpdateInterval { get; set; }
        [DefaultValue(true)]
        [JsonProperty(PropertyName = "Enable Sending Message On Player Leave/Join")]
        public bool UpdateOnPlayerStateChange { get; set; }
        [DefaultValue(true)]
        [JsonProperty(PropertyName = "Enable Sending Server Loading Message")]
        public bool EnableLoadingMessage { get; set; }
        [DefaultValue(15f)]
        [JsonProperty(PropertyName = "Update Rate (Seconds)")]
        public float UpdateRate { get; set; }
        [JsonProperty(PropertyName = "Status Messages")]
        public List<MessageSettings> StatusMessages { get; set; }
        [JsonProperty(PropertyName = "Server Loading Message")]
        public MessageSettings LoadingMessage { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DiscordLogLevel.Info)]
        [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel ExtensionDebugging { get; set; }
    }

    public class MessageSettings
    {
        public string Message { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(ActivityType.Custom)]
        public ActivityType Type { get; set; }
        [JsonConstructor]
        public MessageSettings();
        public MessageSettings(string message, ActivityType type);
        public MessageSettings(MessageSettings settings);
    }

}

public class PluginConfig
{
    [DefaultValue("")]
    [JsonProperty(PropertyName = "Discord Application Bot Token")]
    public string Token { get; set; }
    [DefaultValue(true)]
    [JsonProperty(PropertyName = "Enable Sending Message Per Update Rate")]
    public bool EnableUpdateInterval { get; set; }
    [DefaultValue(true)]
    [JsonProperty(PropertyName = "Enable Sending Message On Player Leave/Join")]
    public bool UpdateOnPlayerStateChange { get; set; }
    [DefaultValue(true)]
    [JsonProperty(PropertyName = "Enable Sending Server Loading Message")]
    public bool EnableLoadingMessage { get; set; }
    [DefaultValue(15f)]
    [JsonProperty(PropertyName = "Update Rate (Seconds)")]
    public float UpdateRate { get; set; }
    [JsonProperty(PropertyName = "Status Messages")]
    public List<MessageSettings> StatusMessages { get; set; }
    [JsonProperty(PropertyName = "Server Loading Message")]
    public MessageSettings LoadingMessage { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DiscordLogLevel.Info)]
    [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel ExtensionDebugging { get; set; }
}

public class MessageSettings
{
    public string Message { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(ActivityType.Custom)]
    public ActivityType Type { get; set; }
    [JsonConstructor]
    public MessageSettings();
    public MessageSettings(string message, ActivityType type);
    public MessageSettings(MessageSettings settings);
}


```

---

## DiscordReport by misticos - Send reports from players to a Discord channel (Including F7 reports for Rust)

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using Facepunch;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Discord Report", "misticos", "1.2.0")]
[Description("Send reports from players ingame to a Discord channel")]
 class DiscordReport : CovalencePlugin
{
    [PluginReference("PlaceholderAPI")]
    private Plugin _placeholders;
    private Action<IPlayer, StringBuilder, bool> _placeholderProcessor;
    private static DiscordReport _ins;
    private const string SteamProfileXML;
    private const string SteamProfile;
    private readonly Regex _steamProfileIconRegex;
    private Dictionary<string, uint> _cooldownData;
    private Time _time;
    private const string PermissionIgnoreCooldown;
    private const string PermissionUse;
    private const string PermissionAdmin;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty("Webhook URL")]
        public string Webhook;
        [JsonProperty("Message Content")]
        public string MessageContent;
        [JsonProperty("Embed Title")]
        public string EmbedTitle;
        [JsonProperty("Embed Description")]
        public string EmbedDescription;
        [JsonProperty("Embed Color")]
        public int EmbedColor;
        [JsonProperty("Set Author Icon From Player Profile")]
        public bool AuthorIcon;
        [JsonProperty("Use Reporter (True) Or Suspect (False) As Author")]
        public bool IsReporterIcon;
        [JsonProperty("Allow Reporting Admins")]
        public bool ReportAdmins;
        [JsonProperty("Report Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> ReportCommands;
        [JsonProperty("Allow Only Online Suspects Reports")]
        public bool OnlyOnlineSuspects;
        [JsonProperty("Threshold Before Sending Reports")]
        public int Threshold;
        [JsonProperty("Cooldown In Seconds")]
        public uint Cooldown;
        [JsonProperty("User Cache Validity In Seconds")]
        public uint UserCacheValidity;
        [JsonProperty("Minimum Message Length")]
        public int MessageMinimum;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private Dictionary<string, PlayerData> _loadedData;
    private void SaveData(string id);
    private PlayerData GetOrLoadData(string id);
    private class PlayerData
    {
        public string ImageURL;
        public string LastKnownAddress;
        public HashSet<string> Reporters;
        public uint LastImageUpdate;
    }

    private class WebhookBody
    {
        [JsonProperty("embeds")]
        public List<EmbedBody> Embeds;
        [JsonProperty("content")]
        public string Content;
    }

    private class EmbedBody
    {
        [JsonProperty("title")]
        public string Title;
        [JsonProperty("type")]
        public string Type;
        [JsonProperty("description")]
        public string Description;
        [JsonProperty("color")]
        public int Color;
        [JsonProperty("author")]
        public AuthorBody Author;
        [JsonProperty("fields")]
        public List<FieldBody> Fields;
        public class AuthorBody
        {
            [JsonProperty("name")]
            public string Name;
            [JsonProperty("url")]
            public string AuthorURL;
            [JsonProperty("icon_url")]
            public string AuthorIconURL;
        }

        public class FieldBody
        {
            [JsonProperty("name")]
            public string Name;
            [JsonProperty("value")]
            public string Value;
            [JsonProperty("inline")]
            public bool Inline;
        }

    }

    protected override void LoadDefaultMessages();
    private void Init();
    private void Loaded();
    private void Unload();
    private void OnUserConnected(IPlayer player);
    private void OnPlaceholderAPIReady();
    private void OnPluginUnloaded(Plugin plugin);
    private void CommandReport(IPlayer player, string command, string[] args);
    private void UpdateCachedImage(IPlayer player, PlayerData data);
    private void SendReport(IPlayer reporter, IPlayer suspect, string subject, string message);
    private string FormatUserDetails(StringBuilder builder, IPlayer player);
    private bool ExceedsCooldown(IPlayer player);
    private void SetCooldown(IPlayer player);
    private bool CanUse(IPlayer player);
    private string GetMsg(string key, string userId);
}

private class Configuration
{
    [JsonProperty("Webhook URL")]
    public string Webhook;
    [JsonProperty("Message Content")]
    public string MessageContent;
    [JsonProperty("Embed Title")]
    public string EmbedTitle;
    [JsonProperty("Embed Description")]
    public string EmbedDescription;
    [JsonProperty("Embed Color")]
    public int EmbedColor;
    [JsonProperty("Set Author Icon From Player Profile")]
    public bool AuthorIcon;
    [JsonProperty("Use Reporter (True) Or Suspect (False) As Author")]
    public bool IsReporterIcon;
    [JsonProperty("Allow Reporting Admins")]
    public bool ReportAdmins;
    [JsonProperty("Report Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> ReportCommands;
    [JsonProperty("Allow Only Online Suspects Reports")]
    public bool OnlyOnlineSuspects;
    [JsonProperty("Threshold Before Sending Reports")]
    public int Threshold;
    [JsonProperty("Cooldown In Seconds")]
    public uint Cooldown;
    [JsonProperty("User Cache Validity In Seconds")]
    public uint UserCacheValidity;
    [JsonProperty("Minimum Message Length")]
    public int MessageMinimum;
}

private class PlayerData
{
    public string ImageURL;
    public string LastKnownAddress;
    public HashSet<string> Reporters;
    public uint LastImageUpdate;
}

private class WebhookBody
{
    [JsonProperty("embeds")]
    public List<EmbedBody> Embeds;
    [JsonProperty("content")]
    public string Content;
}

private class EmbedBody
{
    [JsonProperty("title")]
    public string Title;
    [JsonProperty("type")]
    public string Type;
    [JsonProperty("description")]
    public string Description;
    [JsonProperty("color")]
    public int Color;
    [JsonProperty("author")]
    public AuthorBody Author;
    [JsonProperty("fields")]
    public List<FieldBody> Fields;
    public class AuthorBody
    {
        [JsonProperty("name")]
        public string Name;
        [JsonProperty("url")]
        public string AuthorURL;
        [JsonProperty("icon_url")]
        public string AuthorIconURL;
    }

    public class FieldBody
    {
        [JsonProperty("name")]
        public string Name;
        [JsonProperty("value")]
        public string Value;
        [JsonProperty("inline")]
        public bool Inline;
    }

}

public class AuthorBody
{
    [JsonProperty("name")]
    public string Name;
    [JsonProperty("url")]
    public string AuthorURL;
    [JsonProperty("icon_url")]
    public string AuthorIconURL;
}

public class FieldBody
{
    [JsonProperty("name")]
    public string Name;
    [JsonProperty("value")]
    public string Value;
    [JsonProperty("inline")]
    public bool Inline;
}


```

---

## DiscordRewards by birthdates - Gives players rewards for joining Discord

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord;
using Oxide.Ext.Discord.Attributes;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Entities.Applications;
using Oxide.Ext.Discord.Entities.Channels;
using Oxide.Ext.Discord.Entities.Gatway;
using Oxide.Ext.Discord.Entities.Gatway.Events;
using Oxide.Ext.Discord.Entities.Guilds;
using Oxide.Ext.Discord.Entities.Messages;
using Oxide.Ext.Discord.Entities.Permissions;
using Oxide.Ext.Discord.Logging;

Oxide.Plugins
[Info("Discord Rewards", "birthdates", "1.4.0")]
[Description("Get rewards for joining a discord!")]
public class DiscordRewards : CovalencePlugin
{
    [DiscordClient]
    private DiscordClient Client;
    private readonly DiscordSettings _settings;
    private DiscordRole role;
    private DiscordGuild _guild;
    private const string Perm;
    private Data data;
    private void Init();
    private void OnServerInitialized();
    private void OnNewSave();
    [HookMethod(DiscordExtHooks.OnDiscordGuildMembersLoaded)]
    private void OnDiscordGuildMembersLoaded();
    private void ChatCMD(IPlayer player);
    private readonly System.Random random;
    private string RandomString(int length);
    [HookMethod(DiscordExtHooks.OnDiscordDirectMessageCreated)]
    private void OnDiscordDirectMessageCreated(DiscordMessage message);
    [HookMethod(DiscordExtHooks.OnDiscordGatewayReady)]
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
    private void Unload();
    [HookMethod("IsAuthorized")]
    private bool IsAuthorized(string ID);
    [HookMethod("Deauthorize")]
    private bool Deauthorize(string ID);
    private ConfigFile _config;
    protected override void LoadDefaultMessages();
    private void SaveData();
    private class Data
    {
        public List<Snowflake> verified2;
        public List<string> verified;
        public Dictionary<string, string> codes;
    }

    public class ConfigFile
    {
        [JsonProperty("Command")]
        public string command;
        [JsonProperty("Discord bot key (Look at documentation for how to get this)")]
        public string botKey;
        [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
        public Snowflake GuildId { get; set; }
        [JsonProperty("Verification Role (role given when verified)")]
        public string role;
        [JsonProperty("Commands to execute when player is verified (use {0} for the player's steamid)")]
        public List<string> commands;
        [JsonProperty("Amount of characters in the code")]
        public int codeLength;
        [JsonProperty("Erase all verification data on wipe (new map save)?")]
        public bool wipeData;
        [JsonProperty("Characters used in the verification code")]
        public string codeChars;
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DiscordLogLevel.Info)]
        [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel ExtensionDebugging { get; set; }
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class Data
{
    public List<Snowflake> verified2;
    public List<string> verified;
    public Dictionary<string, string> codes;
}

public class ConfigFile
{
    [JsonProperty("Command")]
    public string command;
    [JsonProperty("Discord bot key (Look at documentation for how to get this)")]
    public string botKey;
    [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
    public Snowflake GuildId { get; set; }
    [JsonProperty("Verification Role (role given when verified)")]
    public string role;
    [JsonProperty("Commands to execute when player is verified (use {0} for the player's steamid)")]
    public List<string> commands;
    [JsonProperty("Amount of characters in the code")]
    public int codeLength;
    [JsonProperty("Erase all verification data on wipe (new map save)?")]
    public bool wipeData;
    [JsonProperty("Characters used in the verification code")]
    public string codeChars;
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DiscordLogLevel.Info)]
    [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel ExtensionDebugging { get; set; }
    public static ConfigFile DefaultConfig();
}


```

---

## DiscordRoles by MJSU - Syncs players Oxide group with Discord roles

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord.Clients;
using Oxide.Ext.Discord.Connections;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Extensions;
using Oxide.Ext.Discord.Interfaces;
using Oxide.Ext.Discord.Libraries;
using Oxide.Ext.Discord.Logging;

Oxide.Plugins
[Info("Discord Roles", "MJSU", "2.1.0")]
[Description("Syncs players oxide group with discord roles")]
 class DiscordRoles : CovalencePlugin, IDiscordPlugin
{
    [PluginReference]
    private Plugin AntiSpam;
    private Plugin Clans;
    public DiscordClient Client { get; set; }
    private PluginConfig _pluginConfig;
    private readonly List<PlayerSync> _processIds;
    private Timer _playerChecker;
    private DiscordGuild _guild;
    private BotConnection _discordSettings;
    private const string AccentColor;
    private readonly List<string> _added;
    private readonly List<string> _removed;
    private readonly DiscordLink _link;
    private readonly Hash<string, string> _nicknames;
    private readonly List<string> _userRoleList;
    private void Init();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    [HookMethod(DiscordExtHooks.OnDiscordGatewayReady)]
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
    [HookMethod(DiscordExtHooks.OnDiscordGuildMembersLoaded)]
    private void OnDiscordGuildMembersLoaded(DiscordGuild guild);
    private void HandleMembersLoaded();
    private void CheckAllPlayers();
    private void StartChecker();
    private void ProcessNextStartupId();
    [Command("dcr.forcecheck")]
    private void HandleCommand(IPlayer player, string cmd, string[] args);
    private void OnUserConnected(IPlayer player);
    private void OnUserGroupAdded(string id, string groupName);
    private void OnUserGroupRemoved(string id, string groupName);
    [HookMethod(DiscordExtHooks.OnDiscordPlayerLinked)]
    private void OnDiscordPlayerLinked(IPlayer player, DiscordUser user);
    [HookMethod(DiscordExtHooks.OnDiscordPlayerUnlinked)]
    private void OnDiscordPlayerUnlinked(IPlayer player, DiscordUser user);
    [HookMethod(DiscordExtHooks.OnDiscordGuildMemberAdded)]
    private void OnDiscordGuildMemberAdded(GuildMemberAddedEvent member);
    [HookMethod(DiscordExtHooks.OnDiscordGuildMemberRemoved)]
    private void OnDiscordGuildMemberRemoved(GuildMemberRemovedEvent member);
    [HookMethod(DiscordExtHooks.OnDiscordGuildMemberUpdated)]
    private void OnDiscordGuildMemberUpdated(GuildMember update, GuildMember oldMember, DiscordGuild guild);
    public void HandleDiscordChange(DiscordUser user, bool isLeaving, SyncEvent syncEvent);
    private void ProcessChange(string playerId, bool isLeaving, SyncEvent syncEvent);
    public void ProcessUser(PlayerSync sync);
    public void HandleServerGroups(PlayerSync playerSync);
    public void HandleDiscordRoles(PlayerSync playerSync);
    public void HandleUserNick(PlayerSync sync);
    private string GetPlayerName(IPlayer player);
    private void SendSyncNotification(PlayerSync sync, SyncData data, bool wasAdded);
    private StringBuilder GetServerMessage(SyncData sync, bool wasAdded);
    private StringBuilder GetDiscordMessage(SyncData sync, bool wasAdded);
    private void ProcessMessage(StringBuilder message, PlayerSync sync, SyncData data);
    public void UnsubscribeAll();
    public void SubscribeAll();
    public string GetUserRoles(GuildMember member);
    public void Debug(DebugEnum level, string message);
    public void Chat(string message);
    public string Lang(string key, IPlayer player, object[] args);
    public string LangNoFormat(string key, IPlayer player);
    public class PluginConfig
    {
        [DefaultValue("")]
        [JsonProperty(PropertyName = "Discord Bot Token")]
        public string DiscordApiKey { get; set; }
        [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
        public Snowflake GuildId { get; set; }
        [DefaultValue(false)]
        [JsonProperty(PropertyName = "Sync Nicknames")]
        public bool SyncNicknames { get; set; }
        [DefaultValue(false)]
        [JsonProperty(PropertyName = "Sync Clan Tag")]
        public bool SyncClanTag { get; set; }
        [DefaultValue(2f)]
        [JsonProperty(PropertyName = "Update Rate (Seconds)")]
        public float UpdateRate { get; set; }
        [DefaultValue(false)]
        [JsonProperty(PropertyName = "Use AntiSpam On Discord Nickname")]
        public bool UseAntiSpam { get; set; }
        [JsonProperty(PropertyName = "Action To Perform By Event")]
        public EventSettings EventSettings { get; set; }
        [JsonProperty(PropertyName = "Sync Data")]
        public List<SyncData> SyncData { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DebugEnum.Warning)]
        [JsonProperty(PropertyName = "Plugin Log Level (None, Error, Warning, Info)")]
        public DebugEnum DebugLevel { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DiscordLogLevel.Info)]
        [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel ExtensionDebugging { get; set; }
    }

    public class EventSettings
    {
        [JsonProperty("Events To Sync Server Groups -> Discord Roles")]
        public EnabledSyncEvents ServerSync { get; set; }
        [JsonProperty("Events To Sync Discord Roles -> Server Groups")]
        public EnabledSyncEvents DiscordSync { get; set; }
        [JsonProperty("Events To Sync Discord Nickname")]
        public EnabledSyncEvents NicknameSync { get; set; }
        [JsonConstructor]
        public EventSettings();
        public EventSettings(EventSettings settings);
        public bool IsAnyEnabled(SyncEvent syncEvent);
    }

    public class SyncData
    {
        [JsonProperty(PropertyName = "Server Group")]
        public string ServerGroup { get; set; }
        [JsonProperty(PropertyName = "Discord Role ID")]
        public Snowflake DiscordRole { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty(PropertyName = "Sync Source (Server or Discord)")]
        public Source Source { get; set; }
        [JsonProperty(PropertyName = "Sync Notification Settings")]
        public NotificationSettings Notifications { get; set; }
        [JsonConstructor]
        public SyncData();
        public SyncData(string serverGroup, Snowflake discordRole, Source source);
        public SyncData(SyncData settings);
    }

    public class NotificationSettings
    {
        [JsonProperty(PropertyName = "Send message to Server")]
        public bool SendMessageToServer { get; set; }
        [JsonProperty(PropertyName = "Send Message To Discord")]
        public bool SendMessageToDiscord { get; set; }
        [JsonProperty(PropertyName = "Discord Message Channel ID")]
        public Snowflake DiscordMessageChannelId { get; set; }
        [JsonProperty(PropertyName = "Send Message When Added")]
        public bool SendMessageOnAdd { get; set; }
        [JsonProperty(PropertyName = "Send Message When Removed")]
        public bool SendMessageOnRemove { get; set; }
        [JsonProperty(PropertyName = "Server Message Added Override Message")]
        public string ServerMessageAddedOverride { get; set; }
        [JsonProperty(PropertyName = "Server Message Removed Override Message")]
        public string ServerMessageRemovedOverride { get; set; }
        [JsonProperty(PropertyName = "Discord Message Added Override Message")]
        public string DiscordMessageAddedOverride { get; set; }
        [JsonProperty(PropertyName = "Discord Message Removed Override Message")]
        public string DiscordMessageRemovedOverride { get; set; }
        public NotificationSettings();
        public NotificationSettings(NotificationSettings settings);
    }

    public class EnabledSyncEvents
    {
        [JsonProperty("Sync On Plugin Load")]
        public bool SyncOnPluginLoad { get; set; }
        [JsonProperty("Sync On Player Connected")]
        public bool SyncOnPlayerConnected { get; set; }
        [JsonProperty("Sync On Server Group Changed")]
        public bool SyncOnServerGroupChanged { get; set; }
        [JsonProperty("Sync On Discord Role Changed")]
        public bool SyncOnDiscordRoleChanged { get; set; }
        [JsonProperty("Sync On Discord Nickname Changed")]
        public bool SyncOnDiscordNicknameChanged { get; set; }
        [JsonProperty("Sync On Player Linked / Unlinked")]
        public bool SyncOnLinkedChanged { get; set; }
        [JsonProperty("Sync On User Join / Leave Discord Server")]
        public bool SyncOnDiscordServerJoinLeave { get; set; }
        public bool IsEnabled(SyncEvent syncEvent);
    }

    public class PlayerSync
    {
        public IPlayer Player { get; set; }
        public GuildMember Member { get; set; }
        public Snowflake MemberId { get; set; }
        public SyncEvent Event { get; set; }
        public bool IsLeaving { get; set; }
        public PlayerSync(IPlayer player, Snowflake memberId, bool isLeaving, SyncEvent syncEvent);
    }

    public class LangKeys
    {
        public const string Chat;
        public const string ClanTag;
        public const string ServerMessageGroupAdded;
        public const string ServerMessageGroupRemoved;
        public const string ServerMessageRoleAdded;
        public const string ServerMessageRoleRemoved;
        public const string DiscordMessageGroupAdded;
        public const string DiscordMessageGroupRemoved;
        public const string DiscordMessageRoleAdded;
        public const string DiscordMessageRoleRemoved;
    }

}

public class PluginConfig
{
    [DefaultValue("")]
    [JsonProperty(PropertyName = "Discord Bot Token")]
    public string DiscordApiKey { get; set; }
    [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
    public Snowflake GuildId { get; set; }
    [DefaultValue(false)]
    [JsonProperty(PropertyName = "Sync Nicknames")]
    public bool SyncNicknames { get; set; }
    [DefaultValue(false)]
    [JsonProperty(PropertyName = "Sync Clan Tag")]
    public bool SyncClanTag { get; set; }
    [DefaultValue(2f)]
    [JsonProperty(PropertyName = "Update Rate (Seconds)")]
    public float UpdateRate { get; set; }
    [DefaultValue(false)]
    [JsonProperty(PropertyName = "Use AntiSpam On Discord Nickname")]
    public bool UseAntiSpam { get; set; }
    [JsonProperty(PropertyName = "Action To Perform By Event")]
    public EventSettings EventSettings { get; set; }
    [JsonProperty(PropertyName = "Sync Data")]
    public List<SyncData> SyncData { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DebugEnum.Warning)]
    [JsonProperty(PropertyName = "Plugin Log Level (None, Error, Warning, Info)")]
    public DebugEnum DebugLevel { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DiscordLogLevel.Info)]
    [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel ExtensionDebugging { get; set; }
}

public class EventSettings
{
    [JsonProperty("Events To Sync Server Groups -> Discord Roles")]
    public EnabledSyncEvents ServerSync { get; set; }
    [JsonProperty("Events To Sync Discord Roles -> Server Groups")]
    public EnabledSyncEvents DiscordSync { get; set; }
    [JsonProperty("Events To Sync Discord Nickname")]
    public EnabledSyncEvents NicknameSync { get; set; }
    [JsonConstructor]
    public EventSettings();
    public EventSettings(EventSettings settings);
    public bool IsAnyEnabled(SyncEvent syncEvent);
}

public class SyncData
{
    [JsonProperty(PropertyName = "Server Group")]
    public string ServerGroup { get; set; }
    [JsonProperty(PropertyName = "Discord Role ID")]
    public Snowflake DiscordRole { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty(PropertyName = "Sync Source (Server or Discord)")]
    public Source Source { get; set; }
    [JsonProperty(PropertyName = "Sync Notification Settings")]
    public NotificationSettings Notifications { get; set; }
    [JsonConstructor]
    public SyncData();
    public SyncData(string serverGroup, Snowflake discordRole, Source source);
    public SyncData(SyncData settings);
}

public class NotificationSettings
{
    [JsonProperty(PropertyName = "Send message to Server")]
    public bool SendMessageToServer { get; set; }
    [JsonProperty(PropertyName = "Send Message To Discord")]
    public bool SendMessageToDiscord { get; set; }
    [JsonProperty(PropertyName = "Discord Message Channel ID")]
    public Snowflake DiscordMessageChannelId { get; set; }
    [JsonProperty(PropertyName = "Send Message When Added")]
    public bool SendMessageOnAdd { get; set; }
    [JsonProperty(PropertyName = "Send Message When Removed")]
    public bool SendMessageOnRemove { get; set; }
    [JsonProperty(PropertyName = "Server Message Added Override Message")]
    public string ServerMessageAddedOverride { get; set; }
    [JsonProperty(PropertyName = "Server Message Removed Override Message")]
    public string ServerMessageRemovedOverride { get; set; }
    [JsonProperty(PropertyName = "Discord Message Added Override Message")]
    public string DiscordMessageAddedOverride { get; set; }
    [JsonProperty(PropertyName = "Discord Message Removed Override Message")]
    public string DiscordMessageRemovedOverride { get; set; }
    public NotificationSettings();
    public NotificationSettings(NotificationSettings settings);
}

public class EnabledSyncEvents
{
    [JsonProperty("Sync On Plugin Load")]
    public bool SyncOnPluginLoad { get; set; }
    [JsonProperty("Sync On Player Connected")]
    public bool SyncOnPlayerConnected { get; set; }
    [JsonProperty("Sync On Server Group Changed")]
    public bool SyncOnServerGroupChanged { get; set; }
    [JsonProperty("Sync On Discord Role Changed")]
    public bool SyncOnDiscordRoleChanged { get; set; }
    [JsonProperty("Sync On Discord Nickname Changed")]
    public bool SyncOnDiscordNicknameChanged { get; set; }
    [JsonProperty("Sync On Player Linked / Unlinked")]
    public bool SyncOnLinkedChanged { get; set; }
    [JsonProperty("Sync On User Join / Leave Discord Server")]
    public bool SyncOnDiscordServerJoinLeave { get; set; }
    public bool IsEnabled(SyncEvent syncEvent);
}

public class PlayerSync
{
    public IPlayer Player { get; set; }
    public GuildMember Member { get; set; }
    public Snowflake MemberId { get; set; }
    public SyncEvent Event { get; set; }
    public bool IsLeaving { get; set; }
    public PlayerSync(IPlayer player, Snowflake memberId, bool isLeaving, SyncEvent syncEvent);
}

public class LangKeys
{
    public const string Chat;
    public const string ClanTag;
    public const string ServerMessageGroupAdded;
    public const string ServerMessageGroupRemoved;
    public const string ServerMessageRoleAdded;
    public const string ServerMessageRoleRemoved;
    public const string DiscordMessageGroupAdded;
    public const string DiscordMessageGroupRemoved;
    public const string DiscordMessageRoleAdded;
    public const string DiscordMessageRoleRemoved;
}


```

---

## DiscordServerMessages by takoz53 - Logs SERVER messages to Discord

```csharp
using Oxide.Core.Plugins;
using System;
using System.Text.RegularExpressions;
using System.Linq;

Oxide.Plugins
[Info("Discord Server Messages", "takocchi", "1.2.41")]
[Description("Logs SERVER messages and cheat logs to Discord Chat")]
 class DiscordServerMessages : RustPlugin
{
    [PluginReference]
    private Plugin DiscordMessages;
     string webhookURL;
     string cheatLogURL;
     bool logCheats;
     bool fancyMessage;
    static readonly Regex isGivingItem;
    static readonly Regex isFlyhack;
    static readonly Regex eggCollected;
    protected override void LoadDefaultConfig();
     void Init();
     void OnServerMessage(string message, string name, string color, ulong id);
    private bool CheckItemCheat(string message);
    private bool CheckFlyHack(string message);
    private bool CheckEasterEvent(string message);
    private string ModifyMessage(string message, bool fancy);
     T GetConfig(string name, T value);
}


```

---

## DiscordServerStats by MJSU - Displays live server stats in a discord channel

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

Oxide.Plugins
[Info("Discord Server Stats", "MJSU", "2.1.0")]
[Description("Displays stats about the server in discord")]
public class DiscordServerStats : CovalencePlugin
{
    [PluginReference]
    private Plugin PlaceholderAPI;
    private StoredData _storedData;
    private PluginConfig _pluginConfig;
    private const string WebhookMessageUpdate;
    private const string WebhooksMessageCreate;
    private const string WebhookDefault;
    private readonly Hash<string, DateTime> _joinedDate;
    private readonly Dictionary<string, string> _headers;
    private readonly Hash<string, MessageHandler> _handlers;
    private Action<IPlayer, StringBuilder, bool> _replacer;
    private readonly StringBuilder _parser;
    private bool _isOnline;
    private static DiscordServerStats _ins;
    private void Init();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    public PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void OnServerSave();
    private void OnServerShutdown();
    private void Unload();
    private void OnUserConnected(IPlayer player);
    private void OnUserDisconnected(IPlayer player);
    public void SetupMessaging();
    public string ParseField(string field);
    private void OnPluginUnloaded(Plugin plugin);
    private void OnPlaceholderAPIReady();
    public void RegisterPlaceholder(string key, Func<IPlayer, string, object> action, string description);
    public Action<IPlayer, StringBuilder, bool> GetReplacer();
    public bool IsPlaceholderApiLoaded();
    public void Debug(DebugEnum level, string message);
    public string Lang(string key);
    public void SaveData();
    public class PluginConfig
    {
        [JsonProperty(PropertyName = "Stats Messages")]
        public List<MessageConfig> MessageConfigs { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DebugEnum.Warning)]
        [JsonProperty(PropertyName = "Debug Level (None, Error, Warning, Info)")]
        public DebugEnum DebugLevel { get; set; }
        [Obsolete("This was removed in version 2.1.0")]
        [JsonProperty(PropertyName = "Discord Webhook")]
        public string DiscordWebhook { get; set; }
        [Obsolete("This was removed in version 2.1.0")]
        [JsonProperty(PropertyName = "Message Update Interval (Minutes)")]
        public float UpdateInterval { get; set; }
        [Obsolete("This was removed in version 2.1.0")]
        [JsonProperty(PropertyName = "Stats Embed Message")]
        public DiscordMessageConfig StatsEmbed { get; set; }
        public bool ShouldSerializeDiscordWebhook();
        public bool ShouldSerializeUpdateInterval();
        public bool ShouldSerializeStatsEmbed();
    }

    public class MessageConfig
    {
        [DefaultValue(WebhookDefault)]
        [JsonProperty(PropertyName = "Discord Webhook")]
        public string DiscordWebhook { get; set; }
        [DefaultValue(1f)]
        [JsonProperty(PropertyName = "Message Update Interval (Minutes)")]
        public float UpdateInterval { get; set; }
        [JsonProperty(PropertyName = "Embed Message")]
        public DiscordMessageConfig StatsEmbed { get; set; }
        public MessageConfig(MessageConfig settings);
    }

    private class StoredData
    {
        [Obsolete("This was removed in version 2.1.0")]
        public string MessageId { get; set; }
        public string LastConnected { get; set; }
        public string LastDisconnected { get; set; }
        public TimeSpan LastDisconnectedDuration;
        public Hash<string, MessageData> WebhookMessages;
        public bool ShouldSerializeMessageId();
    }

    public class MessageData
    {
        public string MessageId { get; set; }
    }

    private static class PluginLang
    {
        public const string OnlineStatus;
        public const string OfflineStatus;
    }

    public class MessageHandler
    {
        public MessageConfig Config { get; set; }
        public MessageData Data { get; set; }
        public Timer Timer { get; set; }
        public string MessageId { get; set; }
        public MessageHandler(MessageConfig config);
        public void SendCreateMessage();
        public void SendUpdateMessage();
    }

    private void CreateDiscordMessage(string webhook, DiscordMessage message, Action<int, T> callback);
    private void UpdateDiscordMessage(string webhook, DiscordMessage message, Action<int, T> callback);
    private void SendDiscordMessageCallback(int code, string message, Action<int, T> callback);
    private const string OwnerIcon;
    private void AddPluginInfoFooter(Embed embed);
    private class DiscordMessage
    {
        [JsonProperty("id")]
        public string Id { get; set; }
        [JsonProperty("username")]
        private string Username { get; set; }
        [JsonProperty("avatar_url")]
        private string AvatarUrl { get; set; }
        [JsonProperty("content")]
        private string Content { get; set; }
        [JsonProperty("embeds")]
        private List<Embed> Embeds { get; set; }
        [JsonConstructor]
        public DiscordMessage();
        public DiscordMessage(string username, string avatarUrl);
        public DiscordMessage(string content, string username, string avatarUrl);
        public DiscordMessage(Embed embed, string username, string avatarUrl);
        public DiscordMessage AddEmbed(Embed embed);
        public DiscordMessage AddContent(string content);
        public DiscordMessage AddSender(string username, string avatarUrl);
        public StringBuilder ToJson();
    }

    private class Embed
    {
        [JsonProperty("color")]
        private int Color { get; set; }
        [JsonProperty("fields")]
        private List<Field> Fields { get; set; }
        [JsonProperty("title")]
        private string Title { get; set; }
        [JsonProperty("description")]
        private string Description { get; set; }
        [JsonProperty("url")]
        private string Url { get; set; }
        [JsonProperty("image")]
        private Image Image { get; set; }
        [JsonProperty("thumbnail")]
        private Image Thumbnail { get; set; }
        [JsonProperty("video")]
        private Video Video { get; set; }
        [JsonProperty("author")]
        private AuthorInfo Author { get; set; }
        [JsonProperty("footer")]
        private Footer Footer { get; set; }
        [JsonProperty("timestamp")]
        private DateTime? Timestamp { get; set; }
        public Embed AddTitle(string title);
        public Embed AddDescription(string description);
        public Embed AddUrl(string url);
        public Embed AddAuthor(string name, string iconUrl, string url, string proxyIconUrl);
        public Embed AddFooter(string text, string iconUrl, string proxyIconUrl);
        public Embed AddColor(int color);
        public Embed AddColor(string color);
        public Embed AddColor(int red, int green, int blue);
        public Embed AddBlankField(bool inline);
        public Embed AddField(string name, string value, bool inline);
        public Embed AddImage(string url, int? width, int? height, string proxyUrl);
        public Embed AddThumbnail(string url, int? width, int? height, string proxyUrl);
        public Embed AddVideo(string url, int? width, int? height);
        public Embed AddTimestamp();
        public Embed AddTimestamp(DateTime timestamp);
    }

    private class Field
    {
        [JsonProperty("name")]
        private string Name { get; set; }
        [JsonProperty("value")]
        private string Value { get; set; }
        [JsonProperty("inline")]
        private bool Inline { get; set; }
        public Field(string name, string value, bool inline);
    }

    private class Image
    {
        [JsonProperty("url")]
        private string Url { get; set; }
        [JsonProperty("width")]
        private int? Width { get; set; }
        [JsonProperty("height")]
        private int? Height { get; set; }
        [JsonProperty("proxyURL")]
        private string ProxyUrl { get; set; }
        public Image(string url, int? width, int? height, string proxyUrl);
    }

    private class Video
    {
        [JsonProperty("url")]
        private string Url { get; set; }
        [JsonProperty("width")]
        private int? Width { get; set; }
        [JsonProperty("height")]
        private int? Height { get; set; }
        public Video(string url, int? width, int? height);
    }

    private class AuthorInfo
    {
        [JsonProperty("name")]
        private string Name { get; set; }
        [JsonProperty("url")]
        private string Url { get; set; }
        [JsonProperty("icon_url")]
        private string IconUrl { get; set; }
        [JsonProperty("proxy_icon_url")]
        private string ProxyIconUrl { get; set; }
        public AuthorInfo(string name, string iconUrl, string url, string proxyIconUrl);
    }

    private class Footer
    {
        [JsonProperty("text")]
        private string Text { get; set; }
        [JsonProperty("icon_url")]
        private string IconUrl { get; set; }
        [JsonProperty("proxy_icon_url")]
        private string ProxyIconUrl { get; set; }
        public Footer(string text, string iconUrl, string proxyIconUrl);
    }

    private class Attachment
    {
        public byte[] Data { get; set; }
        public string Filename { get; set; }
        public string ContentType { get; set; }
        public Attachment(byte[] data, string filename, AttachmentContentType contentType);
        public Attachment(byte[] data, string filename, string contentType);
    }

    public class DiscordMessageConfig
    {
        public string Content { get; set; }
        public List<EmbedConfig> Embeds { get; set; }
    }

    public class EmbedConfig
    {
        [JsonProperty("Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty("Title")]
        public string Title { get; set; }
        [JsonProperty("Description")]
        public string Description { get; set; }
        [JsonProperty("Url")]
        public string Url { get; set; }
        [JsonProperty("Embed Color")]
        public string Color { get; set; }
        [JsonProperty("Image Url")]
        public string Image { get; set; }
        [JsonProperty("Thumbnail Url")]
        public string Thumbnail { get; set; }
        [JsonProperty("Add Timestamp")]
        public bool Timestamp { get; set; }
        [JsonProperty("Fields")]
        public List<FieldConfig> Fields { get; set; }
        [JsonProperty("Footer")]
        public FooterConfig Footer { get; set; }
    }

    public class FieldConfig
    {
        [JsonProperty("Title")]
        public string Title { get; set; }
        [JsonProperty("Value")]
        public string Value { get; set; }
        [JsonProperty("Inline")]
        public bool Inline { get; set; }
        [JsonProperty("Enabled")]
        public bool Enabled { get; set; }
    }

    public class FooterConfig
    {
        [JsonProperty("Icon Url")]
        public string IconUrl { get; set; }
        [JsonProperty("Text")]
        public string Text { get; set; }
        [JsonProperty("Enabled")]
        public bool Enabled { get; set; }
    }

    private DiscordMessage ParseMessage(DiscordMessageConfig config);
}

public class PluginConfig
{
    [JsonProperty(PropertyName = "Stats Messages")]
    public List<MessageConfig> MessageConfigs { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DebugEnum.Warning)]
    [JsonProperty(PropertyName = "Debug Level (None, Error, Warning, Info)")]
    public DebugEnum DebugLevel { get; set; }
    [Obsolete("This was removed in version 2.1.0")]
    [JsonProperty(PropertyName = "Discord Webhook")]
    public string DiscordWebhook { get; set; }
    [Obsolete("This was removed in version 2.1.0")]
    [JsonProperty(PropertyName = "Message Update Interval (Minutes)")]
    public float UpdateInterval { get; set; }
    [Obsolete("This was removed in version 2.1.0")]
    [JsonProperty(PropertyName = "Stats Embed Message")]
    public DiscordMessageConfig StatsEmbed { get; set; }
    public bool ShouldSerializeDiscordWebhook();
    public bool ShouldSerializeUpdateInterval();
    public bool ShouldSerializeStatsEmbed();
}

public class MessageConfig
{
    [DefaultValue(WebhookDefault)]
    [JsonProperty(PropertyName = "Discord Webhook")]
    public string DiscordWebhook { get; set; }
    [DefaultValue(1f)]
    [JsonProperty(PropertyName = "Message Update Interval (Minutes)")]
    public float UpdateInterval { get; set; }
    [JsonProperty(PropertyName = "Embed Message")]
    public DiscordMessageConfig StatsEmbed { get; set; }
    public MessageConfig(MessageConfig settings);
}

private class StoredData
{
    [Obsolete("This was removed in version 2.1.0")]
    public string MessageId { get; set; }
    public string LastConnected { get; set; }
    public string LastDisconnected { get; set; }
    public TimeSpan LastDisconnectedDuration;
    public Hash<string, MessageData> WebhookMessages;
    public bool ShouldSerializeMessageId();
}

public class MessageData
{
    public string MessageId { get; set; }
}

private static class PluginLang
{
    public const string OnlineStatus;
    public const string OfflineStatus;
}

public class MessageHandler
{
    public MessageConfig Config { get; set; }
    public MessageData Data { get; set; }
    public Timer Timer { get; set; }
    public string MessageId { get; set; }
    public MessageHandler(MessageConfig config);
    public void SendCreateMessage();
    public void SendUpdateMessage();
}

private class DiscordMessage
{
    [JsonProperty("id")]
    public string Id { get; set; }
    [JsonProperty("username")]
    private string Username { get; set; }
    [JsonProperty("avatar_url")]
    private string AvatarUrl { get; set; }
    [JsonProperty("content")]
    private string Content { get; set; }
    [JsonProperty("embeds")]
    private List<Embed> Embeds { get; set; }
    [JsonConstructor]
    public DiscordMessage();
    public DiscordMessage(string username, string avatarUrl);
    public DiscordMessage(string content, string username, string avatarUrl);
    public DiscordMessage(Embed embed, string username, string avatarUrl);
    public DiscordMessage AddEmbed(Embed embed);
    public DiscordMessage AddContent(string content);
    public DiscordMessage AddSender(string username, string avatarUrl);
    public StringBuilder ToJson();
}

private class Embed
{
    [JsonProperty("color")]
    private int Color { get; set; }
    [JsonProperty("fields")]
    private List<Field> Fields { get; set; }
    [JsonProperty("title")]
    private string Title { get; set; }
    [JsonProperty("description")]
    private string Description { get; set; }
    [JsonProperty("url")]
    private string Url { get; set; }
    [JsonProperty("image")]
    private Image Image { get; set; }
    [JsonProperty("thumbnail")]
    private Image Thumbnail { get; set; }
    [JsonProperty("video")]
    private Video Video { get; set; }
    [JsonProperty("author")]
    private AuthorInfo Author { get; set; }
    [JsonProperty("footer")]
    private Footer Footer { get; set; }
    [JsonProperty("timestamp")]
    private DateTime? Timestamp { get; set; }
    public Embed AddTitle(string title);
    public Embed AddDescription(string description);
    public Embed AddUrl(string url);
    public Embed AddAuthor(string name, string iconUrl, string url, string proxyIconUrl);
    public Embed AddFooter(string text, string iconUrl, string proxyIconUrl);
    public Embed AddColor(int color);
    public Embed AddColor(string color);
    public Embed AddColor(int red, int green, int blue);
    public Embed AddBlankField(bool inline);
    public Embed AddField(string name, string value, bool inline);
    public Embed AddImage(string url, int? width, int? height, string proxyUrl);
    public Embed AddThumbnail(string url, int? width, int? height, string proxyUrl);
    public Embed AddVideo(string url, int? width, int? height);
    public Embed AddTimestamp();
    public Embed AddTimestamp(DateTime timestamp);
}

private class Field
{
    [JsonProperty("name")]
    private string Name { get; set; }
    [JsonProperty("value")]
    private string Value { get; set; }
    [JsonProperty("inline")]
    private bool Inline { get; set; }
    public Field(string name, string value, bool inline);
}

private class Image
{
    [JsonProperty("url")]
    private string Url { get; set; }
    [JsonProperty("width")]
    private int? Width { get; set; }
    [JsonProperty("height")]
    private int? Height { get; set; }
    [JsonProperty("proxyURL")]
    private string ProxyUrl { get; set; }
    public Image(string url, int? width, int? height, string proxyUrl);
}

private class Video
{
    [JsonProperty("url")]
    private string Url { get; set; }
    [JsonProperty("width")]
    private int? Width { get; set; }
    [JsonProperty("height")]
    private int? Height { get; set; }
    public Video(string url, int? width, int? height);
}

private class AuthorInfo
{
    [JsonProperty("name")]
    private string Name { get; set; }
    [JsonProperty("url")]
    private string Url { get; set; }
    [JsonProperty("icon_url")]
    private string IconUrl { get; set; }
    [JsonProperty("proxy_icon_url")]
    private string ProxyIconUrl { get; set; }
    public AuthorInfo(string name, string iconUrl, string url, string proxyIconUrl);
}

private class Footer
{
    [JsonProperty("text")]
    private string Text { get; set; }
    [JsonProperty("icon_url")]
    private string IconUrl { get; set; }
    [JsonProperty("proxy_icon_url")]
    private string ProxyIconUrl { get; set; }
    public Footer(string text, string iconUrl, string proxyIconUrl);
}

private class Attachment
{
    public byte[] Data { get; set; }
    public string Filename { get; set; }
    public string ContentType { get; set; }
    public Attachment(byte[] data, string filename, AttachmentContentType contentType);
    public Attachment(byte[] data, string filename, string contentType);
}

public class DiscordMessageConfig
{
    public string Content { get; set; }
    public List<EmbedConfig> Embeds { get; set; }
}

public class EmbedConfig
{
    [JsonProperty("Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty("Title")]
    public string Title { get; set; }
    [JsonProperty("Description")]
    public string Description { get; set; }
    [JsonProperty("Url")]
    public string Url { get; set; }
    [JsonProperty("Embed Color")]
    public string Color { get; set; }
    [JsonProperty("Image Url")]
    public string Image { get; set; }
    [JsonProperty("Thumbnail Url")]
    public string Thumbnail { get; set; }
    [JsonProperty("Add Timestamp")]
    public bool Timestamp { get; set; }
    [JsonProperty("Fields")]
    public List<FieldConfig> Fields { get; set; }
    [JsonProperty("Footer")]
    public FooterConfig Footer { get; set; }
}

public class FieldConfig
{
    [JsonProperty("Title")]
    public string Title { get; set; }
    [JsonProperty("Value")]
    public string Value { get; set; }
    [JsonProperty("Inline")]
    public bool Inline { get; set; }
    [JsonProperty("Enabled")]
    public bool Enabled { get; set; }
}

public class FooterConfig
{
    [JsonProperty("Icon Url")]
    public string IconUrl { get; set; }
    [JsonProperty("Text")]
    public string Text { get; set; }
    [JsonProperty("Enabled")]
    public bool Enabled { get; set; }
}


```

---

## DiscordSignLogger by MJSU - Logs Sign / Firework Updates to Discord with button interactions

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord.Attributes;
using Oxide.Ext.Discord.Builders;
using Oxide.Ext.Discord.Cache;
using Oxide.Ext.Discord.Clients;
using Oxide.Ext.Discord.Connections;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Extensions;
using Oxide.Ext.Discord.Interfaces;
using Oxide.Ext.Discord.Libraries;
using Oxide.Ext.Discord.Logging;
using Oxide.Ext.Discord.Types;
using ProtoBuf;
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
[Info("Discord Sign Logger", "MJSU", "3.0.0")]
[Description("Logs Sign / Firework Changes To Discord")]
public partial class DiscordSignLogger : RustPlugin, IDiscordPlugin, IDiscordPool
{
    [PluginReference]
    private Plugin RustTranslationAPI;
    private Plugin SignArtist;
    public DiscordClient Client { get; set; }
    private PluginConfig _pluginConfig;
    private PluginData _pluginData;
    private const string CommandPrefix;
    private const string ActionPrefix;
    private const string ModalPrefix;
    private const string PlayerMessage;
    private const string ServerMessage;
    private const string AccentColor;
    private readonly MessageCreate _actionMessage;
    public DiscordPluginPool Pool { get; set; }
    private readonly StringBuilder _sb;
    public readonly Hash<UnityEngine.Color, Brush> FireworkBrushes;
    private readonly Hash<NetworkableId, SignageUpdate> _updates;
    private readonly Hash<uint, string> _prefabNameCache;
    private readonly Hash<int, string> _itemNameCache;
    private readonly Hash<TemplateKey, SignMessage> _signMessages;
    private readonly Hash<ButtonId, ImageButton> _imageButtons;
    private DiscordChannel _actionChannel;
    private readonly DiscordPlaceholders _placeholders;
    private readonly DiscordMessageTemplates _templates;
    private readonly DiscordButtonTemplates _buttonTemplates;
    private readonly DiscordCommandLocalizations _local;
    public int FireworkImageSize;
    public int FireworkHalfImageSize;
    public int FireworkCircleSize;
    private readonly object _true;
    private readonly object _false;
    public static DiscordSignLogger Instance;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void Unload();
    private void OnImagePost(BasePlayer player, string url, bool raw, ISignage signage, uint textureIndex);
    private void OnSignUpdated(ISignage signage, BasePlayer player, int textureIndex);
    private void OnItemPainted(PaintedItemStorageEntity entity, Item item, BasePlayer player, byte[] image);
    private void OnFireworkDesignChanged(PatternFirework firework, ProtoBuf.PatternFirework.Design design, BasePlayer player);
    private void OnCopyInfoToSign(SignContent content, ISignage sign, IUGCBrowserEntity browser);
    private object CanUpdateSign(BasePlayer player, BaseEntity entity);
    private object OnFireworkDesignChange(PatternFirework firework, ProtoBuf.PatternFirework.Design design, BasePlayer player);
    private object OnPlayerCommand(BasePlayer player, string cmd, string[] args);
    private void UnsubscribeAll();
    private void SubscribeAll();
    [HookMethod(DiscordExtHooks.OnDiscordGuildCreated)]
    private void OnDiscordGuildCreated(DiscordGuild guild);
    public void RunCommand(DiscordInteraction interaction, SignUpdateState state, ImageButton button, string playerMessage, string serverMessage);
    public void ShowConfirmationModal(DiscordInteraction interaction, SignUpdateState state, ImageButton button, TemplateKey messageId, ButtonId buttonId);
    public bool UserHasButtonPermission(DiscordInteraction interaction, ImageButton button);
    public bool TryParseCommand(string command, TemplateKey messageId, ButtonId buttonId, SignUpdateState state);
    public void DisableButton(DiscordMessage message, string id);
    public void DisableAllButtons(DiscordMessage message);
    public void SendErrorResponse(DiscordInteraction interaction, TemplateKey template, PlaceholderData data);
    public void SendComponentUpdateResponse(DiscordInteraction interaction);
    public void SendTemplateResponse(DiscordInteraction interaction, TemplateKey templateName, PlaceholderData data);
    public void SendFollowupResponse(DiscordInteraction interaction, TemplateKey templateName, PlaceholderData data);
    public IEnumerable<IPlayer> GetBannedPlayers();
    public void SendDiscordMessage(BaseImageUpdate update);
    private List<ActionRowComponent> CreateButtons(SignMessage signMessage, PlaceholderData data, StateKey encodedState);
    private string BuildCustomId(string command, IDiscordKey messageId, ButtonId? buttonId, IDiscordKey encodedState);
    [ConsoleCommand("dsl.erase")]
    private void EraseCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("dsl.signblock")]
    private void BanCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("dsl.signunblock")]
    private void UnbanCommand(ConsoleSystem.Arg arg);
    private void HandleReplaceImage(ISignage signage, uint index);
    public IPlayer FindPlayerById(string id);
    public void SaveData();
    public void Puts(string format);
    protected override void LoadDefaultMessages();
    public string Lang(string key, BasePlayer player);
    public string Lang(string key, BasePlayer player, PlaceholderData data);
    public string Lang(string key, BasePlayer player, object[] args);
    public void Chat(BasePlayer player, string key);
    public void Chat(BasePlayer player, string key, PlaceholderData data);
    public void RegisterPlaceholders();
    public PlaceholderData GetPlaceholderData(SignUpdateState state, DiscordInteraction interaction);
    public PlaceholderData GetPlaceholderData(SignUpdateState state);
    public PlaceholderData GetPlaceholderData(DiscordInteraction interaction);
    public PlaceholderData GetPlaceholderData();
    public void RegisterTemplates();
    public void RegisterEn();
    public void RegisterRu();
    public DiscordMessageTemplate CreateMessage(string description, DiscordColor color);
    public DiscordMessageTemplate CreateActionMessage(string description, DiscordColor color);
    public DiscordEmbedTemplate CreateEmbedTemplate(string description, DiscordColor color);
    public DiscordMessageTemplate CreateDefaultTemplate();
    public EmbedFooterTemplate GetFooterTemplate();
    public string GetEntityName(BaseEntity entity);
    public string GetItemName(int itemId);
    public void RegisterApplicationCommands();
    public void AddBlockCommand(ApplicationCommandBuilder builder);
    public void AddUnblockCommand(ApplicationCommandBuilder builder);
    [DiscordAutoCompleteCommand(AppCommand.Command, AppArgs.Player, AppCommand.Block)]
    private void DiscordBlockAutoComplete(DiscordInteraction interaction, InteractionDataOption focused);
    [DiscordAutoCompleteCommand(AppCommand.Command, AppArgs.Player, AppCommand.Unblock)]
    private void DiscordUnblockAutoComplete(DiscordInteraction interaction, InteractionDataOption focused);
    [DiscordApplicationCommand(AppCommand.Command, AppCommand.Block)]
    private void DiscordBlockCommand(DiscordInteraction interaction, InteractionDataParsed parsed);
    [DiscordApplicationCommand(AppCommand.Command, AppCommand.Unblock)]
    private void DiscordUnblockCommand(DiscordInteraction interaction, InteractionDataParsed parsed);
    [DiscordMessageComponentCommand(CommandPrefix)]
    private void DiscordSignLoggerCommand(DiscordInteraction interaction);
    [DiscordMessageComponentCommand(ActionPrefix)]
    private void DiscordSignLoggerAction(DiscordInteraction interaction);
    [DiscordModalSubmit(ModalPrefix)]
    private void DiscordSignLoggerModal(DiscordInteraction interaction);
    public class AppArgs
    {
        public const string Player;
        public const string Duration;
    }

    public class AppCommand
    {
        public const string Command;
        public const string Block;
        public const string Unblock;
    }

    public class FireworkSettings
    {
        [JsonProperty(PropertyName = "Image Size (Pixels)")]
        public int ImageSize { get; set; }
        [JsonProperty(PropertyName = "Circle Size (Pixels)")]
        public int CircleSize { get; set; }
        [JsonConstructor]
        private FireworkSettings();
        public FireworkSettings(FireworkSettings settings);
    }

    public class ImageButton
    {
        [JsonProperty(PropertyName = "Button ID")]
        public ButtonId ButtonId { get; set; }
        [JsonProperty(PropertyName = "Button Display Name")]
        public string DisplayName { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty(PropertyName = "Button Style")]
        public ButtonStyle Style { get; set; }
        [JsonProperty(PropertyName = "Commands")]
        public List<string> Commands { get; set; }
        [JsonProperty(PropertyName = "Player Message")]
        public string PlayerMessage { get; set; }
        [JsonProperty(PropertyName = "Server Message")]
        public string ServerMessage { get; set; }
        [JsonProperty(PropertyName = "Show Confirmation Modal")]
        public bool ConfirmModal { get; set; }
        [JsonProperty(PropertyName = "Requires Permissions To Use Button")]
        public bool RequirePermissions { get; set; }
        [JsonProperty(PropertyName = "Allowed Discord Roles (Role ID)")]
        public List<Snowflake> AllowedRoles { get; set; }
        [JsonProperty(PropertyName = "Allowed Oxide Groups (Group Name)")]
        public List<string> AllowedGroups { get; set; }
        [JsonConstructor]
        public ImageButton();
        public ImageButton(ImageButton settings);
    }

    public class PluginConfig
    {
        [DefaultValue("")]
        [JsonProperty(PropertyName = "Discord Bot Token")]
        public string DiscordApiKey { get; set; }
        [DefaultValue(true)]
        [JsonProperty(PropertyName = "Disable Discord Button After Use")]
        public bool DisableDiscordButton { get; set; }
        [JsonProperty(PropertyName = "Action Log Channel ID")]
        public Snowflake ActionLogChannel { get; set; }
        [JsonProperty(PropertyName = "Replace Erased Image (Requires SignArtist)")]
        public ReplaceImageSettings ReplaceImage { get; set; }
        [JsonProperty(PropertyName = "Firework Settings")]
        public FireworkSettings FireworkSettings { get; set; }
        [JsonProperty(PropertyName = "Sign Messages")]
        public List<SignMessage> SignMessages { get; set; }
        [JsonProperty(PropertyName = "Buttons")]
        public List<ImageButton> Buttons { get; set; }
        public PluginSettings PluginSettings { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DiscordLogLevel.Info)]
        [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel ExtensionDebugging { get; set; }
    }

    public class ReplaceImageSettings
    {
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty(PropertyName = "Replaced Mode (None, Url, Text)")]
        public EraseMode Mode { get; set; }
        [JsonProperty(PropertyName = "URL")]
        public string Url { get; set; }
        [JsonProperty(PropertyName = "Message")]
        public string Message { get; set; }
        [JsonProperty(PropertyName = "Font Size")]
        public int FontSize { get; set; }
        [JsonProperty(PropertyName = "Text Color")]
        public string TextColor { get; set; }
        [JsonProperty(PropertyName = "Body Color")]
        public string BodyColor { get; set; }
        [JsonConstructor]
        private ReplaceImageSettings();
        public ReplaceImageSettings(ReplaceImageSettings settings);
        private bool ShouldSerializeUrl();
        private bool ShouldSerializeMessage();
        private bool ShouldSerializeFontSize();
        private bool ShouldSerializeTextColor();
        private bool ShouldSerializeBodyColor();
    }

    public class SignMessage
    {
        [JsonProperty("Message ID")]
        public TemplateKey MessageId { get; set; }
        [JsonProperty("Discord Channel ID")]
        public Snowflake ChannelId { get; set; }
        [JsonProperty("Use Action Button")]
        public bool UseActionButton { get; set; }
        [JsonProperty("Buttons")]
        public List<ButtonId> Buttons { get; set; }
        [JsonIgnore]
        public DiscordChannel MessageChannel;
        [JsonConstructor]
        private SignMessage();
        public SignMessage(SignMessage settings);
    }

    public class PluginData
    {
        public Hash<ulong, DateTime> SignBannedUsers;
        public void AddSignBan(ulong player, float duration);
        public void RemoveSignBan(ulong player);
        public bool IsSignBanned(BasePlayer player);
        public bool IsSignBanned(string playerId);
        public bool IsSignBanned(ulong playerId);
        public TimeSpan GetRemainingBan(BasePlayer player);
    }

    public class ButtonIdConverter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

    public static class LangKeys
    {
        public const string Chat;
        public const string NoPermission;
        public const string KickReason;
        public const string BanReason;
        public const string BlockedMessage;
    }

    public class PlaceholderDataKeys
    {
        public static readonly PlaceholderDataKey Owner;
        public static readonly PlaceholderDataKey State;
        public static readonly PlaceholderDataKey PlayerMessage;
        public static readonly PlaceholderDataKey ServerMessage;
        public static readonly PlaceholderDataKey SignArtistUrl;
        public static readonly PlaceholderDataKey Command;
        public static readonly PlaceholderDataKey ButtonId;
        public static readonly PlaceholderDataKey MessageId;
        public static readonly PlaceholderDataKey MessageState;
        public static readonly PlaceholderDataKey PlayerId;
    }

    public class PlaceholderKeys
    {
        public static readonly PlaceholderKey EntityId;
        public static readonly PlaceholderKey EntityName;
        public static readonly PlaceholderKey TextureIndex;
        public static readonly PlaceholderKey Position;
        public static readonly PlaceholderKey ItemName;
        public static readonly PlaceholderKey PlayerMessage;
        public static readonly PlaceholderKey ServerMessage;
        public static readonly PlaceholderKey SignArtistUrl;
        public static readonly PlaceholderKey Command;
        public static readonly PlaceholderKey ButtonId;
        public static readonly PlaceholderKey MessageId;
        public static readonly PlaceholderKey MessageState;
        public static readonly PlaceholderKey PlayerId;
        public static readonly PlaceholderKey IsOutside;
        public static readonly PlayerKeys OwnerKeys;
    }

    [ProtoContract]
    public class SignUpdateState
    {
        [ProtoMember(1)]
        public ulong PlayerId { get; set; }
        [ProtoMember(2)]
        public ulong EntityId { get; set; }
        [ProtoMember(3)]
        public byte TextureIndex { get; set; }
        [ProtoMember(4)]
        public int ItemId { get; set; }
        private IPlayer _player;
        public IPlayer Player { get; set; }
        private IPlayer _owner;
        public IPlayer Owner { get; set; }
        private BaseEntity _entity;
        public BaseEntity Entity { get; set; }
        public SignUpdateState();
        public SignUpdateState(BaseImageUpdate update);
        public StateKey Serialize();
        public static SignUpdateState Deserialize(ReadOnlySpan<char> base64);
    }

    public class TemplateKeys
    {
        public static readonly TemplateKey NoPermission;
        public static class Action
        {
            private const string Base;
            public static readonly TemplateKey Message;
            public static readonly TemplateKey Button;
        }

        public static class Errors
        {
            private const string Base;
            public static readonly TemplateKey FailedToParse;
            public static readonly TemplateKey ButtonIdNotFound;
        }

        public static class Commands
        {
            private const string Base;
            public static class Block
            {
                private const string Base;
                public static readonly TemplateKey Success;
                public static class Errors
                {
                    private const string Base;
                    public static readonly TemplateKey PlayerNotFound;
                    public static readonly TemplateKey IsAlreadyBanned;
                }

            }

            public static class Unblock
            {
                private const string Base;
                public static readonly TemplateKey Success;
                public static class Errors
                {
                    private const string Base;
                    public static readonly TemplateKey PlayerNotFound;
                    public static readonly TemplateKey NotBanned;
                }

            }

        }

    }

    public abstract class BaseImageUpdate : ILogEvent
    {
        public IPlayer Player { get; set; }
        public ulong PlayerId { get; set; }
        public string DisplayName { get; set; }
        public BaseEntity Entity { get; set; }
        public bool IgnoreMessage { get; set; }
        public int ItemId { get; set; }
        public byte TextureIndex { get; set; }
        public virtual bool SupportsTextureIndex { get; set; }
        protected BaseImageUpdate(BasePlayer player, BaseEntity entity, bool ignoreMessage);
        public abstract byte[] GetImage();
    }

    public class FireworkUpdate : BaseImageUpdate
    {
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
        public PaintedItemUpdate(BasePlayer player, PaintedItemStorageEntity entity, Item item, byte[] image, bool ignoreMessage);
        public override byte[] GetImage();
    }

    public class SignageUpdate : BaseImageUpdate
    {
        public string Url { get; set; }
        public override bool SupportsTextureIndex { get; set; }
        public ISignage Signage { get; set; }
        public SignageUpdate(BasePlayer player, ISignage entity, byte textureIndex, bool ignoreMessage, string url);
        public override byte[] GetImage();
    }

    public class PluginSettings
    {
        [JsonProperty("Sign Artist Settings")]
        public SignArtistSettings SignArtist { get; set; }
        [JsonConstructor]
        private PluginSettings();
        public PluginSettings(PluginSettings settings);
    }

    public class SignArtistSettings
    {
        [JsonProperty("Log /sil")]
        public bool LogSil { get; set; }
        [JsonProperty("Log /sili")]
        public bool LogSili { get; set; }
        [JsonProperty("Log /silt")]
        public bool LogSilt { get; set; }
        [JsonConstructor]
        private SignArtistSettings();
        public SignArtistSettings(SignArtistSettings settings);
        public bool ShouldLog(string url);
    }

}

public class AppArgs
{
    public const string Player;
    public const string Duration;
}

public class AppCommand
{
    public const string Command;
    public const string Block;
    public const string Unblock;
}

public class FireworkSettings
{
    [JsonProperty(PropertyName = "Image Size (Pixels)")]
    public int ImageSize { get; set; }
    [JsonProperty(PropertyName = "Circle Size (Pixels)")]
    public int CircleSize { get; set; }
    [JsonConstructor]
    private FireworkSettings();
    public FireworkSettings(FireworkSettings settings);
}

public class ImageButton
{
    [JsonProperty(PropertyName = "Button ID")]
    public ButtonId ButtonId { get; set; }
    [JsonProperty(PropertyName = "Button Display Name")]
    public string DisplayName { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty(PropertyName = "Button Style")]
    public ButtonStyle Style { get; set; }
    [JsonProperty(PropertyName = "Commands")]
    public List<string> Commands { get; set; }
    [JsonProperty(PropertyName = "Player Message")]
    public string PlayerMessage { get; set; }
    [JsonProperty(PropertyName = "Server Message")]
    public string ServerMessage { get; set; }
    [JsonProperty(PropertyName = "Show Confirmation Modal")]
    public bool ConfirmModal { get; set; }
    [JsonProperty(PropertyName = "Requires Permissions To Use Button")]
    public bool RequirePermissions { get; set; }
    [JsonProperty(PropertyName = "Allowed Discord Roles (Role ID)")]
    public List<Snowflake> AllowedRoles { get; set; }
    [JsonProperty(PropertyName = "Allowed Oxide Groups (Group Name)")]
    public List<string> AllowedGroups { get; set; }
    [JsonConstructor]
    public ImageButton();
    public ImageButton(ImageButton settings);
}

public class PluginConfig
{
    [DefaultValue("")]
    [JsonProperty(PropertyName = "Discord Bot Token")]
    public string DiscordApiKey { get; set; }
    [DefaultValue(true)]
    [JsonProperty(PropertyName = "Disable Discord Button After Use")]
    public bool DisableDiscordButton { get; set; }
    [JsonProperty(PropertyName = "Action Log Channel ID")]
    public Snowflake ActionLogChannel { get; set; }
    [JsonProperty(PropertyName = "Replace Erased Image (Requires SignArtist)")]
    public ReplaceImageSettings ReplaceImage { get; set; }
    [JsonProperty(PropertyName = "Firework Settings")]
    public FireworkSettings FireworkSettings { get; set; }
    [JsonProperty(PropertyName = "Sign Messages")]
    public List<SignMessage> SignMessages { get; set; }
    [JsonProperty(PropertyName = "Buttons")]
    public List<ImageButton> Buttons { get; set; }
    public PluginSettings PluginSettings { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DiscordLogLevel.Info)]
    [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel ExtensionDebugging { get; set; }
}

public class ReplaceImageSettings
{
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty(PropertyName = "Replaced Mode (None, Url, Text)")]
    public EraseMode Mode { get; set; }
    [JsonProperty(PropertyName = "URL")]
    public string Url { get; set; }
    [JsonProperty(PropertyName = "Message")]
    public string Message { get; set; }
    [JsonProperty(PropertyName = "Font Size")]
    public int FontSize { get; set; }
    [JsonProperty(PropertyName = "Text Color")]
    public string TextColor { get; set; }
    [JsonProperty(PropertyName = "Body Color")]
    public string BodyColor { get; set; }
    [JsonConstructor]
    private ReplaceImageSettings();
    public ReplaceImageSettings(ReplaceImageSettings settings);
    private bool ShouldSerializeUrl();
    private bool ShouldSerializeMessage();
    private bool ShouldSerializeFontSize();
    private bool ShouldSerializeTextColor();
    private bool ShouldSerializeBodyColor();
}

public class SignMessage
{
    [JsonProperty("Message ID")]
    public TemplateKey MessageId { get; set; }
    [JsonProperty("Discord Channel ID")]
    public Snowflake ChannelId { get; set; }
    [JsonProperty("Use Action Button")]
    public bool UseActionButton { get; set; }
    [JsonProperty("Buttons")]
    public List<ButtonId> Buttons { get; set; }
    [JsonIgnore]
    public DiscordChannel MessageChannel;
    [JsonConstructor]
    private SignMessage();
    public SignMessage(SignMessage settings);
}

public class PluginData
{
    public Hash<ulong, DateTime> SignBannedUsers;
    public void AddSignBan(ulong player, float duration);
    public void RemoveSignBan(ulong player);
    public bool IsSignBanned(BasePlayer player);
    public bool IsSignBanned(string playerId);
    public bool IsSignBanned(ulong playerId);
    public TimeSpan GetRemainingBan(BasePlayer player);
}

public class ButtonIdConverter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}

public static class LangKeys
{
    public const string Chat;
    public const string NoPermission;
    public const string KickReason;
    public const string BanReason;
    public const string BlockedMessage;
}

public class PlaceholderDataKeys
{
    public static readonly PlaceholderDataKey Owner;
    public static readonly PlaceholderDataKey State;
    public static readonly PlaceholderDataKey PlayerMessage;
    public static readonly PlaceholderDataKey ServerMessage;
    public static readonly PlaceholderDataKey SignArtistUrl;
    public static readonly PlaceholderDataKey Command;
    public static readonly PlaceholderDataKey ButtonId;
    public static readonly PlaceholderDataKey MessageId;
    public static readonly PlaceholderDataKey MessageState;
    public static readonly PlaceholderDataKey PlayerId;
}

public class PlaceholderKeys
{
    public static readonly PlaceholderKey EntityId;
    public static readonly PlaceholderKey EntityName;
    public static readonly PlaceholderKey TextureIndex;
    public static readonly PlaceholderKey Position;
    public static readonly PlaceholderKey ItemName;
    public static readonly PlaceholderKey PlayerMessage;
    public static readonly PlaceholderKey ServerMessage;
    public static readonly PlaceholderKey SignArtistUrl;
    public static readonly PlaceholderKey Command;
    public static readonly PlaceholderKey ButtonId;
    public static readonly PlaceholderKey MessageId;
    public static readonly PlaceholderKey MessageState;
    public static readonly PlaceholderKey PlayerId;
    public static readonly PlaceholderKey IsOutside;
    public static readonly PlayerKeys OwnerKeys;
}

[ProtoContract]
public class SignUpdateState
{
    [ProtoMember(1)]
    public ulong PlayerId { get; set; }
    [ProtoMember(2)]
    public ulong EntityId { get; set; }
    [ProtoMember(3)]
    public byte TextureIndex { get; set; }
    [ProtoMember(4)]
    public int ItemId { get; set; }
    private IPlayer _player;
    public IPlayer Player { get; set; }
    private IPlayer _owner;
    public IPlayer Owner { get; set; }
    private BaseEntity _entity;
    public BaseEntity Entity { get; set; }
    public SignUpdateState();
    public SignUpdateState(BaseImageUpdate update);
    public StateKey Serialize();
    public static SignUpdateState Deserialize(ReadOnlySpan<char> base64);
}

public class TemplateKeys
{
    public static readonly TemplateKey NoPermission;
    public static class Action
    {
        private const string Base;
        public static readonly TemplateKey Message;
        public static readonly TemplateKey Button;
    }

    public static class Errors
    {
        private const string Base;
        public static readonly TemplateKey FailedToParse;
        public static readonly TemplateKey ButtonIdNotFound;
    }

    public static class Commands
    {
        private const string Base;
        public static class Block
        {
            private const string Base;
            public static readonly TemplateKey Success;
            public static class Errors
            {
                private const string Base;
                public static readonly TemplateKey PlayerNotFound;
                public static readonly TemplateKey IsAlreadyBanned;
            }

        }

        public static class Unblock
        {
            private const string Base;
            public static readonly TemplateKey Success;
            public static class Errors
            {
                private const string Base;
                public static readonly TemplateKey PlayerNotFound;
                public static readonly TemplateKey NotBanned;
            }

        }

    }

}

public static class Action
{
    private const string Base;
    public static readonly TemplateKey Message;
    public static readonly TemplateKey Button;
}

public static class Errors
{
    private const string Base;
    public static readonly TemplateKey FailedToParse;
    public static readonly TemplateKey ButtonIdNotFound;
}

public static class Commands
{
    private const string Base;
    public static class Block
    {
        private const string Base;
        public static readonly TemplateKey Success;
        public static class Errors
        {
            private const string Base;
            public static readonly TemplateKey PlayerNotFound;
            public static readonly TemplateKey IsAlreadyBanned;
        }

    }

    public static class Unblock
    {
        private const string Base;
        public static readonly TemplateKey Success;
        public static class Errors
        {
            private const string Base;
            public static readonly TemplateKey PlayerNotFound;
            public static readonly TemplateKey NotBanned;
        }

    }

}

public static class Block
{
    private const string Base;
    public static readonly TemplateKey Success;
    public static class Errors
    {
        private const string Base;
        public static readonly TemplateKey PlayerNotFound;
        public static readonly TemplateKey IsAlreadyBanned;
    }

}

public static class Errors
{
    private const string Base;
    public static readonly TemplateKey PlayerNotFound;
    public static readonly TemplateKey IsAlreadyBanned;
}

public static class Unblock
{
    private const string Base;
    public static readonly TemplateKey Success;
    public static class Errors
    {
        private const string Base;
        public static readonly TemplateKey PlayerNotFound;
        public static readonly TemplateKey NotBanned;
    }

}

public static class Errors
{
    private const string Base;
    public static readonly TemplateKey PlayerNotFound;
    public static readonly TemplateKey NotBanned;
}

public abstract class BaseImageUpdate : ILogEvent
{
    public IPlayer Player { get; set; }
    public ulong PlayerId { get; set; }
    public string DisplayName { get; set; }
    public BaseEntity Entity { get; set; }
    public bool IgnoreMessage { get; set; }
    public int ItemId { get; set; }
    public byte TextureIndex { get; set; }
    public virtual bool SupportsTextureIndex { get; set; }
    protected BaseImageUpdate(BasePlayer player, BaseEntity entity, bool ignoreMessage);
    public abstract byte[] GetImage();
}

public class FireworkUpdate : BaseImageUpdate
{
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
    public PaintedItemUpdate(BasePlayer player, PaintedItemStorageEntity entity, Item item, byte[] image, bool ignoreMessage);
    public override byte[] GetImage();
}

public class SignageUpdate : BaseImageUpdate
{
    public string Url { get; set; }
    public override bool SupportsTextureIndex { get; set; }
    public ISignage Signage { get; set; }
    public SignageUpdate(BasePlayer player, ISignage entity, byte textureIndex, bool ignoreMessage, string url);
    public override byte[] GetImage();
}

public class PluginSettings
{
    [JsonProperty("Sign Artist Settings")]
    public SignArtistSettings SignArtist { get; set; }
    [JsonConstructor]
    private PluginSettings();
    public PluginSettings(PluginSettings settings);
}

public class SignArtistSettings
{
    [JsonProperty("Log /sil")]
    public bool LogSil { get; set; }
    [JsonProperty("Log /sili")]
    public bool LogSili { get; set; }
    [JsonProperty("Log /silt")]
    public bool LogSilt { get; set; }
    [JsonConstructor]
    private SignArtistSettings();
    public SignArtistSettings(SignArtistSettings settings);
    public bool ShouldLog(string url);
}


```

---

## DiscordStatus by DevGonzi - Shows server information as a discord bot status

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord;
using Oxide.Ext.Discord.Attributes;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Entities.Activities;
using Oxide.Ext.Discord.Entities.Applications;
using Oxide.Ext.Discord.Entities.Channels;
using Oxide.Ext.Discord.Entities.Gatway;
using Oxide.Ext.Discord.Entities.Gatway.Commands;
using Oxide.Ext.Discord.Entities.Gatway.Events;
using Oxide.Ext.Discord.Entities.Guilds;
using Oxide.Ext.Discord.Entities.Messages;
using Oxide.Ext.Discord.Entities.Messages.Embeds;
using Oxide.Ext.Discord.Entities.Permissions;
using Oxide.Ext.Discord.Libraries.Linking;
using Oxide.Ext.Discord.Logging;
using Random = Oxide.Core.Random;

Oxide.Plugins
[Info("Discord Status", "Gonzi", "4.0.1")]
[Description("Shows server information as a discord bot status")]
public class DiscordStatus : CovalencePlugin
{
    private string seperatorText;
    private bool enableChatSeparators;
    [DiscordClient]
    private DiscordClient Client;
    private readonly DiscordSettings _settings;
    private DiscordGuild _guild;
    private readonly DiscordLink _link;
     Configuration config;
    private int statusIndex;
    private string[] StatusTypes;
     class Configuration
    {
        [JsonProperty(PropertyName = "Discord Bot Token")]
        public string BotToken;
        [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
        public Snowflake GuildId { get; set; }
        [JsonProperty(PropertyName = "Prefix")]
        public string Prefix;
        [JsonProperty(PropertyName = "Discord Group Id needed for Commands (null to disable)")]
        public Snowflake? GroupId;
        [JsonProperty(PropertyName = "Update Interval (Seconds)")]
        public int UpdateInterval;
        [JsonProperty(PropertyName = "Randomize Status")]
        public bool Randomize;
        [JsonProperty(PropertyName = "Status Type (Game/Stream/Listen/Watch)")]
        public string StatusType;
        [JsonProperty(PropertyName = "Status", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Status;
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DiscordLogLevel.Info)]
        [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel ExtensionDebugging { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private string Lang(string key, object[] args);
    public DiscordEmbed ServerStats(string content);
    [HookMethod(DiscordHooks.OnDiscordGuildMessageCreated)]
     void OnDiscordGuildMessageCreated(DiscordMessage message);
    private void DiscordCMD(string command, DiscordMessage message);
    private void OnServerInitialized();
    [HookMethod(DiscordHooks.OnDiscordGatewayReady)]
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
    private void UpdateStatus();
    private int GetStatusIndex();
    private ActivityType GetStatusType();
    private string Format(string message);
    private int GetAuthCount();
}

 class Configuration
{
    [JsonProperty(PropertyName = "Discord Bot Token")]
    public string BotToken;
    [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
    public Snowflake GuildId { get; set; }
    [JsonProperty(PropertyName = "Prefix")]
    public string Prefix;
    [JsonProperty(PropertyName = "Discord Group Id needed for Commands (null to disable)")]
    public Snowflake? GroupId;
    [JsonProperty(PropertyName = "Update Interval (Seconds)")]
    public int UpdateInterval;
    [JsonProperty(PropertyName = "Randomize Status")]
    public bool Randomize;
    [JsonProperty(PropertyName = "Status Type (Game/Stream/Listen/Watch)")]
    public string StatusType;
    [JsonProperty(PropertyName = "Status", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Status;
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DiscordLogLevel.Info)]
    [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel ExtensionDebugging { get; set; }
}


```

---

## DiscordSync by  - Integrates players with a discord server

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord.Clients;
using Oxide.Ext.Discord.Connections;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Extensions;
using Oxide.Ext.Discord.Interfaces;
using Oxide.Ext.Discord.Libraries;
using Oxide.Ext.Discord.Logging;

Oxide.Plugins
[Info("Discord Sync", "Tricky & OuTSMoKE", "1.3.0")]
[Description("Integrates players with the discord server")]
public class DiscordSync : CovalencePlugin, IDiscordPlugin
{
    public DiscordClient Client { get; set; }
    private readonly BotConnection _settings;
    private DiscordGuild _guild;
    private readonly DiscordLink _link;
     Configuration config;
     class Configuration
    {
        [JsonProperty(PropertyName = "Discord Bot Token")]
        public string BotToken;
        [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
        public Snowflake GuildId { get; set; }
        [JsonProperty(PropertyName = "Enable Nick Syncing")]
        public bool NickSync;
        [JsonProperty(PropertyName = "Enable Ban Syncing")]
        public bool BanSync;
        [JsonProperty(PropertyName = "Enable Role Syncing")]
        public bool RoleSync;
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DiscordLogLevel.Info)]
        [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel ExtensionDebugging { get; set; }
        [JsonProperty(PropertyName = "Role Setup", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<RoleInfo> RoleSetup;
        public class RoleInfo
        {
            [JsonProperty(PropertyName = "Oxide Group")]
            public string OxideGroup;
            [JsonProperty(PropertyName = "Discord Role")]
            public string DiscordRole;
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [HookMethod(DiscordExtHooks.OnDiscordClientCreated)]
    private void OnDiscordClientCreated();
     void OnUserConnected(IPlayer player);
    private void OnServerInitialized();
    [HookMethod(DiscordExtHooks.OnDiscordGatewayReady)]
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
    [HookMethod(DiscordExtHooks.OnDiscordPlayerLinked)]
    private void OnAuthenticate(IPlayer player, DiscordUser user);
    private void OnUserNameUpdated(string id);
    private void OnUserBanned(string name, string id);
    private void OnUserUnbanned(string name, string id);
    private void OnUserGroupAdded(string id, string groupName);
    private void OnUserGroupRemoved(string id, string groupName);
    private void HandleNick(IPlayer player);
    private void HandleBan(IPlayer player);
    private void HandleRole(string id, string roleName, string oxideGroup);
    private void HandleRole(IPlayer player);
    private bool HasGroup(string id, string groupName);
    private string[] GetGroups(string id);
    private DiscordRole GetRoleByName(string roleName);
    private GuildMember GetGuildMember(Snowflake discordId);
    private bool UserHasRole(Snowflake discordId, Snowflake roleId);
}

 class Configuration
{
    [JsonProperty(PropertyName = "Discord Bot Token")]
    public string BotToken;
    [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
    public Snowflake GuildId { get; set; }
    [JsonProperty(PropertyName = "Enable Nick Syncing")]
    public bool NickSync;
    [JsonProperty(PropertyName = "Enable Ban Syncing")]
    public bool BanSync;
    [JsonProperty(PropertyName = "Enable Role Syncing")]
    public bool RoleSync;
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DiscordLogLevel.Info)]
    [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel ExtensionDebugging { get; set; }
    [JsonProperty(PropertyName = "Role Setup", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<RoleInfo> RoleSetup;
    public class RoleInfo
    {
        [JsonProperty(PropertyName = "Oxide Group")]
        public string OxideGroup;
        [JsonProperty(PropertyName = "Discord Role")]
        public string DiscordRole;
    }

}

public class RoleInfo
{
    [JsonProperty(PropertyName = "Oxide Group")]
    public string OxideGroup;
    [JsonProperty(PropertyName = "Discord Role")]
    public string DiscordRole;
}


```

---

## DiscordTeam by MrBlue - Creates a private voice channel in Discord when a team is created in-game

```csharp
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord;
using Oxide.Ext.Discord.Attributes;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Entities.Applications;
using Oxide.Ext.Discord.Entities.Channels;
using Oxide.Ext.Discord.Entities.Gatway;
using Oxide.Ext.Discord.Entities.Gatway.Events;
using Oxide.Ext.Discord.Entities.Guilds;
using Oxide.Ext.Discord.Entities.Permissions;
using Oxide.Ext.Discord.Entities.Voice;
using Oxide.Ext.Discord.Libraries.Linking;
using Oxide.Ext.Discord.Logging;

Oxide.Plugins
[Info("Discord Team", "Owned", "2.0.0")]
[Description("Creates a private voice channel in Discord when creating a team in-game")]
 class DiscordTeam : CovalencePlugin
{
    [DiscordClient]
    private DiscordClient _client;
    private DiscordRole role;
    private Hash<string, DiscordChannel> listTeamChannels;
    private confData config;
    private bool _init;
    private readonly DiscordSettings _settings;
    private readonly DiscordLink _link;
    private DiscordGuild _guild;
    protected override void LoadDefaultConfig();
    public class confData
    {
        [JsonProperty("Discord Bot Token")]
        public string Token;
        [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
        public Snowflake GuildId { get; set; }
        [JsonProperty("Change channel when user create the team")]
        public bool moveLeader;
        [JsonProperty("Discord users can see other team's private vocal channel")]
        public bool seeOtherTeam;
        [JsonProperty("Using roles")]
        public bool roleUsage;
        [JsonProperty("Name of the player's role on discord (not @everyone)")]
        public string rolePlayer;
        [JsonProperty("Max players in a voice channel")]
        public int maxPlayersChannel;
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DiscordLogLevel.Info)]
        [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel ExtensionDebugging { get; set; }
    }

    protected override void LoadDefaultMessages();
     string GetMessage(string key, string steamId);
    private void Init();
    private void OnServerInitialized();
    private void StartDiscordTeam();
    [HookMethod(DiscordHooks.OnDiscordGatewayReady)]
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
     void Unload();
    private void OnUserConnected(IPlayer player);
     object OnTeamCreate(BasePlayer player);
     object OnTeamPromote(RelationshipManager.PlayerTeam team, BasePlayer newLeader);
     object OnTeamLeave(RelationshipManager.PlayerTeam team, BasePlayer player);
     object OnTeamKick(RelationshipManager.PlayerTeam team, BasePlayer player);
     object OnTeamAcceptInvite(RelationshipManager.PlayerTeam team, BasePlayer player);
     void OnTeamDisbanded(RelationshipManager.PlayerTeam team);
    public void CreateChannelGuild(BasePlayer player);
    public void addPlayerChannel(BasePlayer player, DiscordChannel channel);
    public void removePlayerChannel(BasePlayer player, DiscordChannel channel);
    public void initializeTeam();
    public void deleteChannel(DiscordChannel channel);
    public void renameChannel(DiscordChannel channel, BasePlayer newLeader);
    public DiscordRole GetRoleByName(string roleName);
    private Snowflake GetDiscord(string steamId);
    public GuildMember GetGuildMember(string steamId);
}

public class confData
{
    [JsonProperty("Discord Bot Token")]
    public string Token;
    [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
    public Snowflake GuildId { get; set; }
    [JsonProperty("Change channel when user create the team")]
    public bool moveLeader;
    [JsonProperty("Discord users can see other team's private vocal channel")]
    public bool seeOtherTeam;
    [JsonProperty("Using roles")]
    public bool roleUsage;
    [JsonProperty("Name of the player's role on discord (not @everyone)")]
    public string rolePlayer;
    [JsonProperty("Max players in a voice channel")]
    public int maxPlayersChannel;
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DiscordLogLevel.Info)]
    [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel ExtensionDebugging { get; set; }
}


```

---

## DiscordWelcomer by Trey - Welcome players to your Discord Server via customizable DM based messages

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord.Clients;
using Oxide.Ext.Discord.Connections;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Interfaces;
using Oxide.Ext.Discord.Logging;

Oxide.Plugins
[Info("Discord Welcomer", "Trey", "2.1.0")]
[Description("Welcomes players when they join your Discord server.")]
public class DiscordWelcomer : RustPlugin, IDiscordPlugin
{
    public DiscordClient Client { get; set; }
    private DiscordGuild _guild;
    private readonly BotConnection _settings;
     Data _Data;
    public class Data
    {
        public List<string> ExistingData;
    }

    private void LoadData();
    private void SaveData();
     Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Discord Bot Token")]
        public string BotToken;
        [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
        public Snowflake GuildId { get; set; }
        [JsonProperty(PropertyName = "Your Discord ID (For Testing)")]
        public Snowflake TestID;
        [JsonProperty(PropertyName = "Discord Embed Title")]
        public string EmbedTitle;
        [JsonProperty(PropertyName = "Discord Embed Color (No '#')")]
        public string EmbedColor;
        [JsonProperty(PropertyName = "Discord Embed Author Name (Leave Blank if Unwanted)")]
        public string EmbedAuthorName;
        [JsonProperty(PropertyName = "Discord Embed Author Icon URL (Leave Blank if Unwanted)")]
        public string EmbedAuthorURL;
        [JsonProperty(PropertyName = "Discord Embed Thumbnail Link (Leave Blank if Unwanted)")]
        public string EmbedThumbnailURL;
        [JsonProperty(PropertyName = "Discord Embed Full Image URL (Leave Blank if Unwanted)")]
        public string EmbedFullImageURL;
        [JsonProperty(PropertyName = "Discord Embed Footer Text (Leave Blank if Unwanted)")]
        public string EmbedFooterText;
        [JsonProperty(PropertyName = "Discord Embed Footer Image URL (Leave Blank if Unwanted)")]
        public string EmbedFooterURL;
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel ExtensionDebugging { get; set; }
        [JsonProperty(PropertyName = "Config Version")]
        public string ConfigVersion;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public class LangKeys
    {
        public const string Welcome_New;
        public const string Welcome_Existing;
    }

    private new void LoadDefaultMessages();
    private void OnServerInitialized();
    private void Unload();
    private void OnServerSave();
    [HookMethod(DiscordExtHooks.OnDiscordGatewayReady)]
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
    [HookMethod(DiscordExtHooks.OnDiscordGuildMemberAdded)]
    private void OnDiscordGuildMemberAdded(GuildMember member);
    [ConsoleCommand("testwelcomemessage")]
    private void TestWelcomeCommand(ConsoleSystem.Arg args);
    private DiscordEmbed CreateEmbed(string message);
    private DiscordColor ConvertColorToDiscordColor(string colorcode);
     string Lang(string key, string id, object[] args);
    private void CheckConfigVersion(VersionNumber version);
}

public class Data
{
    public List<string> ExistingData;
}

public class Configuration
{
    [JsonProperty(PropertyName = "Discord Bot Token")]
    public string BotToken;
    [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
    public Snowflake GuildId { get; set; }
    [JsonProperty(PropertyName = "Your Discord ID (For Testing)")]
    public Snowflake TestID;
    [JsonProperty(PropertyName = "Discord Embed Title")]
    public string EmbedTitle;
    [JsonProperty(PropertyName = "Discord Embed Color (No '#')")]
    public string EmbedColor;
    [JsonProperty(PropertyName = "Discord Embed Author Name (Leave Blank if Unwanted)")]
    public string EmbedAuthorName;
    [JsonProperty(PropertyName = "Discord Embed Author Icon URL (Leave Blank if Unwanted)")]
    public string EmbedAuthorURL;
    [JsonProperty(PropertyName = "Discord Embed Thumbnail Link (Leave Blank if Unwanted)")]
    public string EmbedThumbnailURL;
    [JsonProperty(PropertyName = "Discord Embed Full Image URL (Leave Blank if Unwanted)")]
    public string EmbedFullImageURL;
    [JsonProperty(PropertyName = "Discord Embed Footer Text (Leave Blank if Unwanted)")]
    public string EmbedFooterText;
    [JsonProperty(PropertyName = "Discord Embed Footer Image URL (Leave Blank if Unwanted)")]
    public string EmbedFooterURL;
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel ExtensionDebugging { get; set; }
    [JsonProperty(PropertyName = "Config Version")]
    public string ConfigVersion;
}

public class LangKeys
{
    public const string Welcome_New;
    public const string Welcome_Existing;
}


```

---

## DiscordWipe by MJSU - Sends a notification to a Discord channel when the server wipes

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Globalization;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;

[Info("Discord Wipe", "MJSU", "2.4.3")]
[Description("Sends a notification to a discord channel when the server wipes or protocol changes")]
internal class DiscordWipe : CovalencePlugin
{
    [PluginReference]
    private Plugin RustMapApi;
    private Plugin PlaceholderAPI;
    private PluginConfig _pluginConfig;
    private StoredData _storedData;
    private const string DefaultUrl;
    private const string DefaultRustMapsApiKey;
    private const string AdminPermission;
    private const string AttachmentBase;
    private const string MapAttachment;
    private const string MapFilename;
    private const int MaxImageSize;
    private string _protocol;
    private string _previousProtocol;
    private bool _hasStarted;
    private readonly StringBuilder _parser;
    private Action<IPlayer, StringBuilder, bool> _replacer;
    private void Init();
    public string UpdateWebhookUrl(string url);
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void OnRustMapApiReady();
    private void HandleStartup(string source);
    private void OnNewSave();
    private void Unload();
    private string GetProtocol();
    private bool SendWipeCommand(IPlayer player, string cmd, string[] args);
    private void SendWipe();
    private void SendProtocol();
    private void SendMessage(string url, List<DiscordMessageConfig> messageConfigs);
    private string ParseField(string field);
    private void OnPluginUnloaded(Plugin plugin);
    private void OnPlaceholderAPIReady();
    private void RegisterPlaceholder(string key, Func<IPlayer, string, object> action, string description, double ttl);
    private Action<IPlayer, StringBuilder, bool> GetReplacer();
    private bool IsPlaceholderApiLoaded();
    public long UnixTimeNow();
    public void UnsubscribeAll();
    public void SubscribeAll();
    private void Debug(DebugEnum level, string message);
    private string Lang(string key, IPlayer player, object[] args);
    private bool IsRustMapApiLoaded();
    private bool IsRustMapApiReady();
    private void SaveData();
    private bool HasPermission(IPlayer player, string perm);
    private class PluginConfig
    {
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DebugEnum.Warning)]
        [JsonProperty(PropertyName = "Debug Level (None, Error, Warning, Info)")]
        public DebugEnum DebugLevel { get; set; }
        [DefaultValue("dw")]
        [JsonProperty(PropertyName = "Command")]
        public string Command { get; set; }
        [DefaultValue(true)]
        [JsonProperty(PropertyName = "Send wipe message when server wipes")]
        public bool SendWipeAutomatically { get; set; }
        [DefaultValue(DefaultUrl)]
        [JsonProperty(PropertyName = "Wipe Webhook url")]
        public string WipeWebhook { get; set; }
        [DefaultValue(true)]
        [JsonProperty(PropertyName = "Send protocol message when server protocol changes")]
        public bool SendProtocolAutomatically { get; set; }
        [DefaultValue(DefaultUrl)]
        [JsonProperty(PropertyName = "Protocol Webhook url")]
        public string ProtocolWebhook { get; set; }
        [JsonProperty(PropertyName = "Wipe messages")]
        public List<DiscordMessageConfig> WipeEmbeds { get; set; }
        [JsonProperty(PropertyName = "Protocol messages")]
        public List<DiscordMessageConfig> ProtocolEmbeds { get; set; }
    }

    public class RustMapsApiRequest
    {
        [JsonProperty("size")]
        public uint Size { get; set; }
        [JsonProperty("seed")]
        public uint Seed { get; set; }
        [JsonProperty("staging")]
        public bool Staging { get; set; }
        [JsonProperty("barren")]
        public bool Barren { get; set; }
    }

    private class StoredData
    {
        public bool IsWipe { get; set; }
        public string Protocol { get; set; }
    }

    private class LangKeys
    {
        public const string NoPermission;
        public const string SentWipe;
        public const string SentProtocol;
        public const string Help;
    }

    private readonly Dictionary<string, string> _headers;
    private void SendDiscordMessage(string url, DiscordMessage message);
    private void SendDiscordMessageCallback(int code, string message);
    private const string OwnerIcon;
    private void AddPluginInfoFooter(Embed embed);
    private string GetPositionField(Vector3 pos);
    private class DiscordMessage
    {
        [JsonProperty("username")]
        private string Username { get; set; }
        [JsonProperty("avatar_url")]
        private string AvatarUrl { get; set; }
        [JsonProperty("content")]
        private string Content { get; set; }
        [JsonProperty("embeds")]
        private List<Embed> Embeds { get; set; }
        public DiscordMessage(string username, string avatarUrl);
        public DiscordMessage(string content, string username, string avatarUrl);
        public DiscordMessage(Embed embed, string username, string avatarUrl);
        public DiscordMessage AddEmbed(Embed embed);
        public DiscordMessage AddContent(string content);
        public DiscordMessage AddSender(string username, string avatarUrl);
        public StringBuilder ToJson();
    }

    private class Embed
    {
        [JsonProperty("color")]
        private int Color { get; set; }
        [JsonProperty("fields")]
        private List<Field> Fields { get; set; }
        [JsonProperty("title")]
        private string Title { get; set; }
        [JsonProperty("description")]
        private string Description { get; set; }
        [JsonProperty("url")]
        private string Url { get; set; }
        [JsonProperty("image")]
        private Image Image { get; set; }
        [JsonProperty("thumbnail")]
        private Image Thumbnail { get; set; }
        [JsonProperty("video")]
        private Video Video { get; set; }
        [JsonProperty("author")]
        private AuthorInfo Author { get; set; }
        [JsonProperty("footer")]
        private Footer Footer { get; set; }
        public Embed AddTitle(string title);
        public Embed AddDescription(string description);
        public Embed AddUrl(string url);
        public Embed AddAuthor(string name, string iconUrl, string url, string proxyIconUrl);
        public Embed AddFooter(string text, string iconUrl, string proxyIconUrl);
        public Embed AddColor(int color);
        public Embed AddColor(string color);
        public Embed AddColor(int red, int green, int blue);
        public Embed AddBlankField(bool inline);
        public Embed AddField(string name, string value, bool inline);
        public Embed AddImage(string url, int? width, int? height, string proxyUrl);
        public Embed AddThumbnail(string url, int? width, int? height, string proxyUrl);
        public Embed AddVideo(string url, int? width, int? height);
    }

    private class Field
    {
        [JsonProperty("name")]
        private string Name { get; set; }
        [JsonProperty("value")]
        private string Value { get; set; }
        [JsonProperty("inline")]
        private bool Inline { get; set; }
        public Field(string name, string value, bool inline);
    }

    private class Image
    {
        [JsonProperty("url")]
        private string Url { get; set; }
        [JsonProperty("width")]
        private int? Width { get; set; }
        [JsonProperty("height")]
        private int? Height { get; set; }
        [JsonProperty("proxyURL")]
        private string ProxyUrl { get; set; }
        public Image(string url, int? width, int? height, string proxyUrl);
    }

    private class Video
    {
        [JsonProperty("url")]
        private string Url { get; set; }
        [JsonProperty("width")]
        private int? Width { get; set; }
        [JsonProperty("height")]
        private int? Height { get; set; }
        public Video(string url, int? width, int? height);
    }

    private class AuthorInfo
    {
        [JsonProperty("name")]
        private string Name { get; set; }
        [JsonProperty("url")]
        private string Url { get; set; }
        [JsonProperty("icon_url")]
        private string IconUrl { get; set; }
        [JsonProperty("proxy_icon_url")]
        private string ProxyIconUrl { get; set; }
        public AuthorInfo(string name, string iconUrl, string url, string proxyIconUrl);
    }

    private class Footer
    {
        [JsonProperty("text")]
        private string Text { get; set; }
        [JsonProperty("icon_url")]
        private string IconUrl { get; set; }
        [JsonProperty("proxy_icon_url")]
        private string ProxyIconUrl { get; set; }
        public Footer(string text, string iconUrl, string proxyIconUrl);
    }

    private class Attachment
    {
        public byte[] Data { get; set; }
        public string Filename { get; set; }
        public string ContentType { get; set; }
        public Attachment(byte[] data, string filename, AttachmentContentType contentType);
        public Attachment(byte[] data, string filename, string contentType);
    }

    public class DiscordMessageConfig
    {
        public string Content { get; set; }
        [JsonProperty("Webhook Override (Overrides the default webhook for this message)")]
        public string WebhookOverride { get; set; }
        [JsonProperty("Send Mode (Always, Random)")]
        [JsonConverter(typeof(StringEnumConverter))]
        public SendMode SendMode { get; set; }
        public EmbedConfig Embed { get; set; }
    }

    public class EmbedConfig
    {
        [JsonProperty("Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty("Title")]
        public string Title { get; set; }
        [JsonProperty("Description")]
        public string Description { get; set; }
        [JsonProperty("Url")]
        public string Url { get; set; }
        [JsonProperty("Embed Color")]
        public string Color { get; set; }
        [JsonProperty("Image Url")]
        public string Image { get; set; }
        [JsonProperty("Thumbnail Url")]
        public string Thumbnail { get; set; }
        [JsonProperty("Fields")]
        public List<FieldConfig> Fields { get; set; }
        [JsonProperty("Footer")]
        public FooterConfig Footer { get; set; }
    }

    public class FieldConfig
    {
        [JsonProperty("Title")]
        public string Title { get; set; }
        [JsonProperty("Value")]
        public string Value { get; set; }
        [JsonProperty("Inline")]
        public bool Inline { get; set; }
        [JsonProperty("Enabled")]
        public bool Enabled { get; set; }
    }

    public class FooterConfig
    {
        [JsonProperty("Icon Url")]
        public string IconUrl { get; set; }
        [JsonProperty("Text")]
        public string Text { get; set; }
        [JsonProperty("Enabled")]
        public bool Enabled { get; set; }
    }

    private DiscordMessage ParseMessage(DiscordMessageConfig config);
}

private class PluginConfig
{
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DebugEnum.Warning)]
    [JsonProperty(PropertyName = "Debug Level (None, Error, Warning, Info)")]
    public DebugEnum DebugLevel { get; set; }
    [DefaultValue("dw")]
    [JsonProperty(PropertyName = "Command")]
    public string Command { get; set; }
    [DefaultValue(true)]
    [JsonProperty(PropertyName = "Send wipe message when server wipes")]
    public bool SendWipeAutomatically { get; set; }
    [DefaultValue(DefaultUrl)]
    [JsonProperty(PropertyName = "Wipe Webhook url")]
    public string WipeWebhook { get; set; }
    [DefaultValue(true)]
    [JsonProperty(PropertyName = "Send protocol message when server protocol changes")]
    public bool SendProtocolAutomatically { get; set; }
    [DefaultValue(DefaultUrl)]
    [JsonProperty(PropertyName = "Protocol Webhook url")]
    public string ProtocolWebhook { get; set; }
    [JsonProperty(PropertyName = "Wipe messages")]
    public List<DiscordMessageConfig> WipeEmbeds { get; set; }
    [JsonProperty(PropertyName = "Protocol messages")]
    public List<DiscordMessageConfig> ProtocolEmbeds { get; set; }
}

public class RustMapsApiRequest
{
    [JsonProperty("size")]
    public uint Size { get; set; }
    [JsonProperty("seed")]
    public uint Seed { get; set; }
    [JsonProperty("staging")]
    public bool Staging { get; set; }
    [JsonProperty("barren")]
    public bool Barren { get; set; }
}

private class StoredData
{
    public bool IsWipe { get; set; }
    public string Protocol { get; set; }
}

private class LangKeys
{
    public const string NoPermission;
    public const string SentWipe;
    public const string SentProtocol;
    public const string Help;
}

private class DiscordMessage
{
    [JsonProperty("username")]
    private string Username { get; set; }
    [JsonProperty("avatar_url")]
    private string AvatarUrl { get; set; }
    [JsonProperty("content")]
    private string Content { get; set; }
    [JsonProperty("embeds")]
    private List<Embed> Embeds { get; set; }
    public DiscordMessage(string username, string avatarUrl);
    public DiscordMessage(string content, string username, string avatarUrl);
    public DiscordMessage(Embed embed, string username, string avatarUrl);
    public DiscordMessage AddEmbed(Embed embed);
    public DiscordMessage AddContent(string content);
    public DiscordMessage AddSender(string username, string avatarUrl);
    public StringBuilder ToJson();
}

private class Embed
{
    [JsonProperty("color")]
    private int Color { get; set; }
    [JsonProperty("fields")]
    private List<Field> Fields { get; set; }
    [JsonProperty("title")]
    private string Title { get; set; }
    [JsonProperty("description")]
    private string Description { get; set; }
    [JsonProperty("url")]
    private string Url { get; set; }
    [JsonProperty("image")]
    private Image Image { get; set; }
    [JsonProperty("thumbnail")]
    private Image Thumbnail { get; set; }
    [JsonProperty("video")]
    private Video Video { get; set; }
    [JsonProperty("author")]
    private AuthorInfo Author { get; set; }
    [JsonProperty("footer")]
    private Footer Footer { get; set; }
    public Embed AddTitle(string title);
    public Embed AddDescription(string description);
    public Embed AddUrl(string url);
    public Embed AddAuthor(string name, string iconUrl, string url, string proxyIconUrl);
    public Embed AddFooter(string text, string iconUrl, string proxyIconUrl);
    public Embed AddColor(int color);
    public Embed AddColor(string color);
    public Embed AddColor(int red, int green, int blue);
    public Embed AddBlankField(bool inline);
    public Embed AddField(string name, string value, bool inline);
    public Embed AddImage(string url, int? width, int? height, string proxyUrl);
    public Embed AddThumbnail(string url, int? width, int? height, string proxyUrl);
    public Embed AddVideo(string url, int? width, int? height);
}

private class Field
{
    [JsonProperty("name")]
    private string Name { get; set; }
    [JsonProperty("value")]
    private string Value { get; set; }
    [JsonProperty("inline")]
    private bool Inline { get; set; }
    public Field(string name, string value, bool inline);
}

private class Image
{
    [JsonProperty("url")]
    private string Url { get; set; }
    [JsonProperty("width")]
    private int? Width { get; set; }
    [JsonProperty("height")]
    private int? Height { get; set; }
    [JsonProperty("proxyURL")]
    private string ProxyUrl { get; set; }
    public Image(string url, int? width, int? height, string proxyUrl);
}

private class Video
{
    [JsonProperty("url")]
    private string Url { get; set; }
    [JsonProperty("width")]
    private int? Width { get; set; }
    [JsonProperty("height")]
    private int? Height { get; set; }
    public Video(string url, int? width, int? height);
}

private class AuthorInfo
{
    [JsonProperty("name")]
    private string Name { get; set; }
    [JsonProperty("url")]
    private string Url { get; set; }
    [JsonProperty("icon_url")]
    private string IconUrl { get; set; }
    [JsonProperty("proxy_icon_url")]
    private string ProxyIconUrl { get; set; }
    public AuthorInfo(string name, string iconUrl, string url, string proxyIconUrl);
}

private class Footer
{
    [JsonProperty("text")]
    private string Text { get; set; }
    [JsonProperty("icon_url")]
    private string IconUrl { get; set; }
    [JsonProperty("proxy_icon_url")]
    private string ProxyIconUrl { get; set; }
    public Footer(string text, string iconUrl, string proxyIconUrl);
}

private class Attachment
{
    public byte[] Data { get; set; }
    public string Filename { get; set; }
    public string ContentType { get; set; }
    public Attachment(byte[] data, string filename, AttachmentContentType contentType);
    public Attachment(byte[] data, string filename, string contentType);
}

public class DiscordMessageConfig
{
    public string Content { get; set; }
    [JsonProperty("Webhook Override (Overrides the default webhook for this message)")]
    public string WebhookOverride { get; set; }
    [JsonProperty("Send Mode (Always, Random)")]
    [JsonConverter(typeof(StringEnumConverter))]
    public SendMode SendMode { get; set; }
    public EmbedConfig Embed { get; set; }
}

public class EmbedConfig
{
    [JsonProperty("Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty("Title")]
    public string Title { get; set; }
    [JsonProperty("Description")]
    public string Description { get; set; }
    [JsonProperty("Url")]
    public string Url { get; set; }
    [JsonProperty("Embed Color")]
    public string Color { get; set; }
    [JsonProperty("Image Url")]
    public string Image { get; set; }
    [JsonProperty("Thumbnail Url")]
    public string Thumbnail { get; set; }
    [JsonProperty("Fields")]
    public List<FieldConfig> Fields { get; set; }
    [JsonProperty("Footer")]
    public FooterConfig Footer { get; set; }
}

public class FieldConfig
{
    [JsonProperty("Title")]
    public string Title { get; set; }
    [JsonProperty("Value")]
    public string Value { get; set; }
    [JsonProperty("Inline")]
    public bool Inline { get; set; }
    [JsonProperty("Enabled")]
    public bool Enabled { get; set; }
}

public class FooterConfig
{
    [JsonProperty("Icon Url")]
    public string IconUrl { get; set; }
    [JsonProperty("Text")]
    public string Text { get; set; }
    [JsonProperty("Enabled")]
    public bool Enabled { get; set; }
}


```

---

## DiscordWipeAnnouncement by MisterPixie - Post's an announcement to discord when the server wipes.

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Discord Wipe Announcement", "MisterPixie", "1.1.0")]
[Description("Post to discord when server wipes")]
 class DiscordWipeAnnouncement : RustPlugin
{
    [PluginReference]
     Plugin DiscordMessages;
    private void Init();
    [ChatCommand("dwatest")]
     void DisCommand(BasePlayer player, string command, string[] args);
     void SendDiscordMessage();
    public class EmbedFieldList
    {
        public string name { get; set; }
        public string value { get; set; }
        public bool inline { get; set; }
    }

     void SendEmbedDiscordMessage();
     void OnNewSave(string filename);
    public class EmbedList
    {
        public string name;
        public string value;
        public bool inline;
    }

    private ConfigData configData;
    private class ConfigData
    {
        public string WebhookURL;
        public bool Enable;
        public bool UsingEmbedText;
        public string EmbedTextTitle;
        public int EmbedColor;
        public List<string> WipeMessage;
        public Dictionary<int, EmbedList> EmbedText;
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
}

public class EmbedFieldList
{
    public string name { get; set; }
    public string value { get; set; }
    public bool inline { get; set; }
}

public class EmbedList
{
    public string name;
    public string value;
    public bool inline;
}

private class ConfigData
{
    public string WebhookURL;
    public bool Enable;
    public bool UsingEmbedText;
    public string EmbedTextTitle;
    public int EmbedColor;
    public List<string> WipeMessage;
    public Dictionary<int, EmbedList> EmbedText;
}


```

---

## Diseases by mr01sam - Adds fully customizable diseases to Rust for tormenting your players with outbreaks

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Diseases", "mr01sam", "2.1.1")]
[Description("Players can be inflicted with disease that can be spread to others")]
public class Diseases : CovalencePlugin
{
    public static Diseases PLUGIN;
    private const string PermissionUse;
    private const string PermissionAdmin;
    [PluginReference]
    private Plugin ImageLibrary;
    private const float PLAYER_TICK_RATE;
    private static bool debug;
    private DiseaseManager diseaseManager;
    private EffectManager effectManager;
    private Dictionary<string, DiseaseStats> diseaseStats;
     void Init();
     void Unload();
     void OnServerInitialized(bool initial);
     void OnEntityDeath(BasePlayer player, HitInfo info);
     object OnPlayerSleep(BasePlayer player);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnItemUse(Item item, int amountToUse);
     object OnEntityTakeDamage(BasePlayer player, HitInfo info);
    private void LoadImages();
    private void DoDiseaseTick(Disease disease);
    private void DoInfectedTick(BasePlayer player, Disease disease);
    private void DoRecoverTick(BasePlayer player, Disease disease);
    private List<BaseCombatEntity> NearbyValidPlayers(Vector3 position, Disease disease);
    private void InfectPlayer(BasePlayer player, Disease disease, Action callback);
    private void RecoverPlayer(BasePlayer player, Disease disease, Action callback);
    private int Outbreak(Disease disease, int tries);
    private void CurePlayer(BasePlayer player, Disease disease, Action callback);
    private void TreatPlayer(BasePlayer player, Disease disease, int amount, int delay, Action callback);
    private void StatusMessage(BasePlayer player, Disease disease, string message);
    private void DoSymptomEffects(BasePlayer player, Disease disease);
    private void DoSymptomScreenEffects(BasePlayer player, Disease disease);
    private void DoSymptomDamage(BasePlayer player, Disease disease);
    private bool WasCauseOfDeath(BasePlayer player, HitInfo info, Disease disease);
    private float GetSpreadChance(BasePlayer player, Disease disease);
    private bool CompareStat(BasePlayer player, string statname, float value, Func<float, float, bool> comparison);
    private string AsTitle(string str);
     class DoubleDictionary
    {
        private Dictionary<TkeyA, Dictionary<TkeyB, Tvalue>> a2b;
        private Dictionary<TkeyB, Dictionary<TkeyA, Tvalue>> b2a;
        public DoubleDictionary();
        public void Set(TkeyA keyA, TkeyB keyB, Tvalue value);
        public Tvalue Get(TkeyA keyA, TkeyB keyB);
        public Dictionary<TkeyB, Tvalue> Get(TkeyA keyA);
        public Dictionary<TkeyA, Tvalue> Get(TkeyB keyB);
        public bool ContainsKey(TkeyA keyA, TkeyB keyB);
        public void Delete(TkeyA keyA, TkeyB keyB);
        public void Delete(TkeyA keyA);
        public void Delete(TkeyB keyB);
    }

     class LookupList
    {
        private Dictionary<TOne, List<TTwo>> a2b;
        private Dictionary<TTwo, List<TOne>> b2a;
        public LookupList();
        public int Count();
        public void Add(TOne a, TTwo b);
        public void AddAll(TOne[] listA, TTwo b);
        public void AddAll(TTwo[] listB, TOne a);
        public void Remove(TOne a);
        public void Remove(TTwo b);
        public void Remove(TOne a, TTwo b);
        public List<TTwo> Get(TOne a);
        public List<TOne> Get(TTwo b);
        public bool ContainsKey(TOne a);
        public bool ContainsKey(TTwo b);
        public bool Contains(TOne a, TTwo b);
        public bool Contains(TTwo b, TOne a);
    }

     class RustStat
    {
        public const string health;
        public const string bleeding;
        public const string calories;
        public const string hydration;
        public const string oxygen;
        public const string radiation;
        public const string temperature;
        public const string poison;
    }

     class SymptomEffect
    {
        [JsonProperty(PropertyName = "Asset path")]
        public string shortPrefabName;
        [JsonProperty(PropertyName = "Local only")]
        public bool LocalOnly;
    }

     class SymptomScreenEffect
    {
        [JsonProperty(PropertyName = "Sprite asset path")]
        public string shortPrefabName;
        [JsonProperty(PropertyName = "Opacity (0-1.0)")]
        public float opacity;
        [JsonProperty(PropertyName = "Duration (seconds)")]
        public float duration;
        [JsonProperty(PropertyName = "Fade out (seconds)")]
        public float fadeOut;
        [JsonProperty(PropertyName = "Material asset path (optional)")]
        public string materialName;
    }

     class SpreaderEntity
    {
        [JsonProperty(PropertyName = "Short prefab name")]
        public string shortPrefabName;
        [JsonProperty(PropertyName = "Infection chance on hit (0-100)")]
        public float infectionChanceOnHit;
    }

     class InfectionItem : ItemPrefab
    {
        [JsonProperty(PropertyName = "Infection chance on consumption (0-100)")]
        public float infectionChanceOnConsumption;
    }

     class TreatmentItem : ItemPrefab
    {
        [JsonProperty(PropertyName = "Min infection time decreased (seconds)")]
        public int minTimeDecrease;
        [JsonProperty(PropertyName = "Max infection time decreased (seconds)")]
        public int maxTimeDecrease;
        [JsonProperty(PropertyName = "Delay between reusing treament (seconds)")]
        public int treatmentDelay;
        public int Value();
    }

     class CureItem : ItemPrefab
    {
        [JsonProperty(PropertyName = "Cure chance on consumption (0-100)")]
        public float cureChanceOnConsumption;
    }

    abstract class ItemPrefab
    {
        [JsonProperty(PropertyName = "Short prefab name")]
        public string shortPrefabName;
    }

     class SymptomDamage
    {
        [JsonProperty(PropertyName = "Stat name")]
        public string stat;
        [JsonProperty(PropertyName = "Min damage")]
        public float minValue;
        [JsonProperty(PropertyName = "Max damage")]
        public float maxValue;
        public float Value();
        public Rust.DamageType GetDamageType();
    }

     class StatFilter
    {
        [JsonProperty(PropertyName = "Stat name")]
        public string stat;
        [JsonProperty(PropertyName = "Min value")]
        public float minValue;
        [JsonProperty(PropertyName = "Max value")]
        public float maxValue;
    }

     class OutbreakCondition
    {
        [JsonProperty(PropertyName = "Outbreaks enabled")]
        public bool enabled;
        [JsonProperty(PropertyName = "Outbreak chance (0-100)")]
        public float outbreakChance;
        [JsonProperty(PropertyName = "Time interval (seconds)")]
        public int timeInterval;
        [JsonProperty(PropertyName = "Only outbreak on players with the following stats")]
        public StatFilter[] statFilters;
    }

     class DiseaseStats
    {
        public class DiseaseStat
        {
            public string name;
            public int count;
        }

        public DiseaseStat activeInfected;
        public DiseaseStat sleepersInfected;
        public DiseaseStat totalInfected;
        public DiseaseStat playersKilled;
        public DiseaseStat numOutbreaks;
        public DiseaseStat infectedFromSpreading;
        public DiseaseStat infectedFromEntity;
        public DiseaseStat infectedFromItem;
        public DiseaseStat infectedFromOutbreak;
        public DiseaseStat infectedFromCommand;
        public DiseaseStat timesTreatedFromItem;
        public DiseaseStat timesCuredFromItem;
    }

     class Disease
    {
        [JsonProperty(PropertyName = "Disease name")]
        public string name { get; set; }
        [JsonProperty(PropertyName = "Description")]
        public string info { get; set; }
        [JsonProperty(PropertyName = "Min time immune after recovery (seconds)")]
        public int minImmunityTime { get; set; }
        [JsonProperty(PropertyName = "Max time immune after recovery (seconds)")]
        public int maxImmunityTime { get; set; }
        [JsonProperty(PropertyName = "Min time infected (seconds)")]
        public int minInfectionTime { get; set; }
        [JsonProperty(PropertyName = "Max time infected (seconds)")]
        public int maxInfectionTime { get; set; }
        [JsonProperty(PropertyName = "Min time between symptoms (seconds)")]
        public int minSpreadTime { get; set; }
        [JsonProperty(PropertyName = "Max time between symptoms (seconds)")]
        public int maxSpreadTime { get; set; }
        [JsonProperty(PropertyName = "Spread distance (4.0 = 1 foundation)")]
        public float spreadDistance { get; set; }
        [JsonProperty(PropertyName = "Spread chance with mask (0-100)")]
        public float spreadChanceCovered { get; set; }
        [JsonProperty(PropertyName = "Spread chance without mask (0-100)")]
        public float spreadChanceUncovered { get; set; }
        [JsonProperty(PropertyName = "Symptom damage effects")]
        public SymptomDamage[] damageValues { get; set; }
        [JsonProperty(PropertyName = "Items that cause infection on consumption")]
        public InfectionItem[] infectionItems { get; set; }
        [JsonProperty(PropertyName = "Items that cure on consumption")]
        public CureItem[] cureItems { get; set; }
        [JsonProperty(PropertyName = "Items that reduce infection time on consumption")]
        public TreatmentItem[] treatmentItems { get; set; }
        [JsonProperty(PropertyName = "Entities that cause infection on hit")]
        public SpreaderEntity[] spreaderEntities { get; set; }
        [JsonProperty(PropertyName = "Random outbreak settings")]
        public OutbreakCondition outbreakConditions { get; set; }
        [JsonProperty(PropertyName = "Symptom effects")]
        public SymptomEffect[] symptomEffects { get; set; }
        [JsonProperty(PropertyName = "Symptom screen effects")]
        public SymptomScreenEffect[] symptomScreenEffects { get; set; }
        [JsonProperty(PropertyName = "Version")]
        public VersionNumber version { get; set; }
        public int SpreadTime();
        public int InfectedTime();
        public int ImmunityTime();
        public static Disease GenerateDefault();
        public static void SaveToFile(Disease disease);
        public static Disease ReadFromFile(string diseaseName);
    }

     class EffectManager
    {
        public readonly Dictionary<ulong, List<string>> ActiveEffects;
        private ulong idCount;
        public EffectManager();
        private void InitPlayer(ulong userid);
        private void AddEffect(ulong userid, string effectString);
        private void RemoveEffect(ulong userid, string effectString);
        public void PlayEffect(BasePlayer player, string effectString, bool local);
        public void ClearAllEffects(BasePlayer player);
        public void ShowScreenEffect(BasePlayer player, string assetStringPath, string materialStringPath, float opacity, float duration, float fadeOut);
    }

     class DiseaseManager
    {
        public readonly LookupList<BasePlayer, string> InfectedPlayers;
        public readonly Dictionary<string, Disease> LoadedDiseases;
        private DoubleDictionary<ulong, string, int> InfectedTimes;
        private DoubleDictionary<ulong, string, int> NextGoalTimes;
        private DoubleDictionary<ulong, string, bool> IsImmune;
        private DoubleDictionary<ulong, string, bool> IsTreated;
        private DoubleDictionary<ulong, string, bool> ActiveTick;
        private Dictionary<ulong, bool> SafeZone;
        public readonly DoubleDictionary<string, string, CureItem> ItemsThatCure;
        public readonly DoubleDictionary<string, string, InfectionItem> ItemsThatInfect;
        public readonly DoubleDictionary<string, string, TreatmentItem> ItemsThatTreat;
        public readonly DoubleDictionary<string, string, SpreaderEntity> EntitiesThatInfect;
        public DiseaseManager();
        public void LoadAllDiseases(Action callback);
        public void LoadDisease(Disease disease, Action callback);
        public void UnloadDisease(string diseasename);
        public Disease GetDisease(string diseasename);
        public bool AddInfected(BasePlayer player, Disease disease);
        public bool RemoveInfected(BasePlayer player, Disease disease);
        public bool RemoveAllInfected(string diseasename);
        public BasePlayer[] GetInfectedList(string diseasename);
        public string[] GetPlayerInfections(BasePlayer player);
        public void SetActiveTick(BasePlayer player, Disease disease, bool value);
        public bool HasActiveTick(BasePlayer player, Disease disease);
        public bool CanInfect(BasePlayer player, Disease disease);
        public bool HasDisease(BasePlayer player, Disease disease);
        public bool HasImmunity(BasePlayer player, Disease disease);
        public void SetTreatment(BasePlayer player, Disease disease, bool value);
        public bool HasTreatment(BasePlayer player, Disease disease);
        public bool UpdateInfectedTime(BasePlayer player, Disease disease, int modifier);
        public bool SetRecovery(BasePlayer player, Disease disease);
        private void LoadItems(Disease disease);
        private void LoadEntities(Disease disease);
    }

    private Dictionary<string, ChatCmd> commands;
     void InitCommands();
     class ChatCmd
    {
        public string prefix;
        public List<string> usages;
        public List<string> perms;
        public Action<IPlayer, string, string[]> function;
        public bool HasPerms(string id);
        public string Usage(string prefix, string usg);
    }

    [Command("disease")]
    private void cmd_disease(IPlayer player, string command, string[] args);
    [Command("disease.help"), Permission(PermissionAdmin)]
    private void cmd_help(IPlayer player, string command, string[] args);
    [Command("infect"), Permission(PermissionAdmin)]
    private void cmd_infect(IPlayer player, string command, string[] args);
    [Command("cure"), Permission(PermissionAdmin)]
    private void cmd_cure(IPlayer player, string command, string[] args);
    [Command("outbreak"), Permission(PermissionAdmin)]
    private void cmd_outbreak(IPlayer player, string command, string[] args);
    [Command("eradicate"), Permission(PermissionAdmin)]
    private void cmd_eradicate(IPlayer player, string command, string[] args);
    [Command("disease.load"), Permission(PermissionAdmin)]
    private void cmd_load(IPlayer player, string command, string[] args);
    [Command("disease.unload"), Permission(PermissionAdmin)]
    private void cmd_unload(IPlayer player, string command, string[] args);
    [Command("disease.list"), Permission(PermissionAdmin)]
    private void cmd_list(IPlayer player, string command, string[] args);
    [Command("disease.info"), Permission(PermissionAdmin)]
    private void cmd_info(IPlayer player, string command, string[] args);
    [Command("disease.config"), Permission(PermissionAdmin)]
    private void cmd_config(IPlayer player, string command, string[] args);
    [Command("disease.stats"), Permission(PermissionAdmin)]
    private void cmd_stats(IPlayer player, string command, string[] args);
    private Configuration config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Generate default disease (true/false)")]
        public bool GenerateDefaultDisease;
        [JsonProperty(PropertyName = "Loaded diseases on startup")]
        public string[] DefaultLoadedDiseases { get; set; }
        [JsonProperty(PropertyName = "Max number of infected entities")]
        public int MaxInfectedEntities { get; set; }
        [JsonProperty(PropertyName = "Disease name color in messages")]
        public string DiseaseNameColor { get; set; }
        [JsonProperty(PropertyName = "Face covering items")]
        public string[] FaceCoveringItems { get; set; }
        [JsonProperty(PropertyName = "Show status chat messages")]
        public bool ShowStatusMessages { get; set; }
        [JsonProperty(PropertyName = "Infections can spread in safe zones")]
        public bool SafezoneInfectionSpread { get; set; }
        [JsonProperty(PropertyName = "Broadcast disease death messages")]
        public bool BroadcastDeathMessages { get; set; }
        [JsonProperty(PropertyName = "Broadcast outbreaks")]
        public bool BroadcastOutbreaks { get; set; }
        [JsonProperty(PropertyName = "HUD indicator list")]
        public PositionUI IndicatorHUD { get; set; }
    }

    private class PositionUI
    {
        [JsonProperty(PropertyName = "Anchor min")]
        public string AnchorMin;
        [JsonProperty(PropertyName = "Anchor max")]
        public string AnchorMax;
        [JsonProperty(PropertyName = "Entry height")]
        public float EntryHeight;
        [JsonProperty(PropertyName = "Anchor min (image)")]
        public string ImgAnchorMin;
        [JsonProperty(PropertyName = "Anchor max (image)")]
        public string ImgAnchorMax;
        [JsonProperty(PropertyName = "Show image (requires ImageLibrary)")]
        public bool ImgShow;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private string Lang(string key, string id, object[] args);
    private string Color(string text, string colorCode);
    private string Size(string text, string fontSize);
     string COLOR_RED;
     string COLOR_YELLOW;
     void RefreshIndicator(BasePlayer player);
     void AddIndicator(CuiElementContainer container, BasePlayer player, Disease disease, int index);
     void HideAllIndicators(BasePlayer player);
     string CalcUI(float left, float right);
}

 class DoubleDictionary
{
    private Dictionary<TkeyA, Dictionary<TkeyB, Tvalue>> a2b;
    private Dictionary<TkeyB, Dictionary<TkeyA, Tvalue>> b2a;
    public DoubleDictionary();
    public void Set(TkeyA keyA, TkeyB keyB, Tvalue value);
    public Tvalue Get(TkeyA keyA, TkeyB keyB);
    public Dictionary<TkeyB, Tvalue> Get(TkeyA keyA);
    public Dictionary<TkeyA, Tvalue> Get(TkeyB keyB);
    public bool ContainsKey(TkeyA keyA, TkeyB keyB);
    public void Delete(TkeyA keyA, TkeyB keyB);
    public void Delete(TkeyA keyA);
    public void Delete(TkeyB keyB);
}

 class LookupList
{
    private Dictionary<TOne, List<TTwo>> a2b;
    private Dictionary<TTwo, List<TOne>> b2a;
    public LookupList();
    public int Count();
    public void Add(TOne a, TTwo b);
    public void AddAll(TOne[] listA, TTwo b);
    public void AddAll(TTwo[] listB, TOne a);
    public void Remove(TOne a);
    public void Remove(TTwo b);
    public void Remove(TOne a, TTwo b);
    public List<TTwo> Get(TOne a);
    public List<TOne> Get(TTwo b);
    public bool ContainsKey(TOne a);
    public bool ContainsKey(TTwo b);
    public bool Contains(TOne a, TTwo b);
    public bool Contains(TTwo b, TOne a);
}

 class RustStat
{
    public const string health;
    public const string bleeding;
    public const string calories;
    public const string hydration;
    public const string oxygen;
    public const string radiation;
    public const string temperature;
    public const string poison;
}

 class SymptomEffect
{
    [JsonProperty(PropertyName = "Asset path")]
    public string shortPrefabName;
    [JsonProperty(PropertyName = "Local only")]
    public bool LocalOnly;
}

 class SymptomScreenEffect
{
    [JsonProperty(PropertyName = "Sprite asset path")]
    public string shortPrefabName;
    [JsonProperty(PropertyName = "Opacity (0-1.0)")]
    public float opacity;
    [JsonProperty(PropertyName = "Duration (seconds)")]
    public float duration;
    [JsonProperty(PropertyName = "Fade out (seconds)")]
    public float fadeOut;
    [JsonProperty(PropertyName = "Material asset path (optional)")]
    public string materialName;
}

 class SpreaderEntity
{
    [JsonProperty(PropertyName = "Short prefab name")]
    public string shortPrefabName;
    [JsonProperty(PropertyName = "Infection chance on hit (0-100)")]
    public float infectionChanceOnHit;
}

 class InfectionItem : ItemPrefab
{
    [JsonProperty(PropertyName = "Infection chance on consumption (0-100)")]
    public float infectionChanceOnConsumption;
}

 class TreatmentItem : ItemPrefab
{
    [JsonProperty(PropertyName = "Min infection time decreased (seconds)")]
    public int minTimeDecrease;
    [JsonProperty(PropertyName = "Max infection time decreased (seconds)")]
    public int maxTimeDecrease;
    [JsonProperty(PropertyName = "Delay between reusing treament (seconds)")]
    public int treatmentDelay;
    public int Value();
}

 class CureItem : ItemPrefab
{
    [JsonProperty(PropertyName = "Cure chance on consumption (0-100)")]
    public float cureChanceOnConsumption;
}

abstract class ItemPrefab
{
    [JsonProperty(PropertyName = "Short prefab name")]
    public string shortPrefabName;
}

 class SymptomDamage
{
    [JsonProperty(PropertyName = "Stat name")]
    public string stat;
    [JsonProperty(PropertyName = "Min damage")]
    public float minValue;
    [JsonProperty(PropertyName = "Max damage")]
    public float maxValue;
    public float Value();
    public Rust.DamageType GetDamageType();
}

 class StatFilter
{
    [JsonProperty(PropertyName = "Stat name")]
    public string stat;
    [JsonProperty(PropertyName = "Min value")]
    public float minValue;
    [JsonProperty(PropertyName = "Max value")]
    public float maxValue;
}

 class OutbreakCondition
{
    [JsonProperty(PropertyName = "Outbreaks enabled")]
    public bool enabled;
    [JsonProperty(PropertyName = "Outbreak chance (0-100)")]
    public float outbreakChance;
    [JsonProperty(PropertyName = "Time interval (seconds)")]
    public int timeInterval;
    [JsonProperty(PropertyName = "Only outbreak on players with the following stats")]
    public StatFilter[] statFilters;
}

 class DiseaseStats
{
    public class DiseaseStat
    {
        public string name;
        public int count;
    }

    public DiseaseStat activeInfected;
    public DiseaseStat sleepersInfected;
    public DiseaseStat totalInfected;
    public DiseaseStat playersKilled;
    public DiseaseStat numOutbreaks;
    public DiseaseStat infectedFromSpreading;
    public DiseaseStat infectedFromEntity;
    public DiseaseStat infectedFromItem;
    public DiseaseStat infectedFromOutbreak;
    public DiseaseStat infectedFromCommand;
    public DiseaseStat timesTreatedFromItem;
    public DiseaseStat timesCuredFromItem;
}

public class DiseaseStat
{
    public string name;
    public int count;
}

 class Disease
{
    [JsonProperty(PropertyName = "Disease name")]
    public string name { get; set; }
    [JsonProperty(PropertyName = "Description")]
    public string info { get; set; }
    [JsonProperty(PropertyName = "Min time immune after recovery (seconds)")]
    public int minImmunityTime { get; set; }
    [JsonProperty(PropertyName = "Max time immune after recovery (seconds)")]
    public int maxImmunityTime { get; set; }
    [JsonProperty(PropertyName = "Min time infected (seconds)")]
    public int minInfectionTime { get; set; }
    [JsonProperty(PropertyName = "Max time infected (seconds)")]
    public int maxInfectionTime { get; set; }
    [JsonProperty(PropertyName = "Min time between symptoms (seconds)")]
    public int minSpreadTime { get; set; }
    [JsonProperty(PropertyName = "Max time between symptoms (seconds)")]
    public int maxSpreadTime { get; set; }
    [JsonProperty(PropertyName = "Spread distance (4.0 = 1 foundation)")]
    public float spreadDistance { get; set; }
    [JsonProperty(PropertyName = "Spread chance with mask (0-100)")]
    public float spreadChanceCovered { get; set; }
    [JsonProperty(PropertyName = "Spread chance without mask (0-100)")]
    public float spreadChanceUncovered { get; set; }
    [JsonProperty(PropertyName = "Symptom damage effects")]
    public SymptomDamage[] damageValues { get; set; }
    [JsonProperty(PropertyName = "Items that cause infection on consumption")]
    public InfectionItem[] infectionItems { get; set; }
    [JsonProperty(PropertyName = "Items that cure on consumption")]
    public CureItem[] cureItems { get; set; }
    [JsonProperty(PropertyName = "Items that reduce infection time on consumption")]
    public TreatmentItem[] treatmentItems { get; set; }
    [JsonProperty(PropertyName = "Entities that cause infection on hit")]
    public SpreaderEntity[] spreaderEntities { get; set; }
    [JsonProperty(PropertyName = "Random outbreak settings")]
    public OutbreakCondition outbreakConditions { get; set; }
    [JsonProperty(PropertyName = "Symptom effects")]
    public SymptomEffect[] symptomEffects { get; set; }
    [JsonProperty(PropertyName = "Symptom screen effects")]
    public SymptomScreenEffect[] symptomScreenEffects { get; set; }
    [JsonProperty(PropertyName = "Version")]
    public VersionNumber version { get; set; }
    public int SpreadTime();
    public int InfectedTime();
    public int ImmunityTime();
    public static Disease GenerateDefault();
    public static void SaveToFile(Disease disease);
    public static Disease ReadFromFile(string diseaseName);
}

 class EffectManager
{
    public readonly Dictionary<ulong, List<string>> ActiveEffects;
    private ulong idCount;
    public EffectManager();
    private void InitPlayer(ulong userid);
    private void AddEffect(ulong userid, string effectString);
    private void RemoveEffect(ulong userid, string effectString);
    public void PlayEffect(BasePlayer player, string effectString, bool local);
    public void ClearAllEffects(BasePlayer player);
    public void ShowScreenEffect(BasePlayer player, string assetStringPath, string materialStringPath, float opacity, float duration, float fadeOut);
}

 class DiseaseManager
{
    public readonly LookupList<BasePlayer, string> InfectedPlayers;
    public readonly Dictionary<string, Disease> LoadedDiseases;
    private DoubleDictionary<ulong, string, int> InfectedTimes;
    private DoubleDictionary<ulong, string, int> NextGoalTimes;
    private DoubleDictionary<ulong, string, bool> IsImmune;
    private DoubleDictionary<ulong, string, bool> IsTreated;
    private DoubleDictionary<ulong, string, bool> ActiveTick;
    private Dictionary<ulong, bool> SafeZone;
    public readonly DoubleDictionary<string, string, CureItem> ItemsThatCure;
    public readonly DoubleDictionary<string, string, InfectionItem> ItemsThatInfect;
    public readonly DoubleDictionary<string, string, TreatmentItem> ItemsThatTreat;
    public readonly DoubleDictionary<string, string, SpreaderEntity> EntitiesThatInfect;
    public DiseaseManager();
    public void LoadAllDiseases(Action callback);
    public void LoadDisease(Disease disease, Action callback);
    public void UnloadDisease(string diseasename);
    public Disease GetDisease(string diseasename);
    public bool AddInfected(BasePlayer player, Disease disease);
    public bool RemoveInfected(BasePlayer player, Disease disease);
    public bool RemoveAllInfected(string diseasename);
    public BasePlayer[] GetInfectedList(string diseasename);
    public string[] GetPlayerInfections(BasePlayer player);
    public void SetActiveTick(BasePlayer player, Disease disease, bool value);
    public bool HasActiveTick(BasePlayer player, Disease disease);
    public bool CanInfect(BasePlayer player, Disease disease);
    public bool HasDisease(BasePlayer player, Disease disease);
    public bool HasImmunity(BasePlayer player, Disease disease);
    public void SetTreatment(BasePlayer player, Disease disease, bool value);
    public bool HasTreatment(BasePlayer player, Disease disease);
    public bool UpdateInfectedTime(BasePlayer player, Disease disease, int modifier);
    public bool SetRecovery(BasePlayer player, Disease disease);
    private void LoadItems(Disease disease);
    private void LoadEntities(Disease disease);
}

 class ChatCmd
{
    public string prefix;
    public List<string> usages;
    public List<string> perms;
    public Action<IPlayer, string, string[]> function;
    public bool HasPerms(string id);
    public string Usage(string prefix, string usg);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Generate default disease (true/false)")]
    public bool GenerateDefaultDisease;
    [JsonProperty(PropertyName = "Loaded diseases on startup")]
    public string[] DefaultLoadedDiseases { get; set; }
    [JsonProperty(PropertyName = "Max number of infected entities")]
    public int MaxInfectedEntities { get; set; }
    [JsonProperty(PropertyName = "Disease name color in messages")]
    public string DiseaseNameColor { get; set; }
    [JsonProperty(PropertyName = "Face covering items")]
    public string[] FaceCoveringItems { get; set; }
    [JsonProperty(PropertyName = "Show status chat messages")]
    public bool ShowStatusMessages { get; set; }
    [JsonProperty(PropertyName = "Infections can spread in safe zones")]
    public bool SafezoneInfectionSpread { get; set; }
    [JsonProperty(PropertyName = "Broadcast disease death messages")]
    public bool BroadcastDeathMessages { get; set; }
    [JsonProperty(PropertyName = "Broadcast outbreaks")]
    public bool BroadcastOutbreaks { get; set; }
    [JsonProperty(PropertyName = "HUD indicator list")]
    public PositionUI IndicatorHUD { get; set; }
}

private class PositionUI
{
    [JsonProperty(PropertyName = "Anchor min")]
    public string AnchorMin;
    [JsonProperty(PropertyName = "Anchor max")]
    public string AnchorMax;
    [JsonProperty(PropertyName = "Entry height")]
    public float EntryHeight;
    [JsonProperty(PropertyName = "Anchor min (image)")]
    public string ImgAnchorMin;
    [JsonProperty(PropertyName = "Anchor max (image)")]
    public string ImgAnchorMax;
    [JsonProperty(PropertyName = "Show image (requires ImageLibrary)")]
    public bool ImgShow;
}


```

---

## DMBuildingBlocks by ColonBlow - Prevents/allows damage to specific building blocks (walls, floors, etc.)

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using System;

Oxide.Plugins
[Info("DMBuildingBlocks", "ColonBlow", "1.0.9")]
 class DMBuildingBlocks : RustPlugin
{
    private void Loaded();
    private static PluginConfig config;
    private class PluginConfig
    {
        public GlobalSettings globalSettings { get; set; }
        public class GlobalSettings
        {
            [JsonProperty(PropertyName = "Scale - Precent of damage allowed to any Building block that is currently protected (default is 0%) : ")]
            public float ProtectionScale { get; set; }
            [JsonProperty(PropertyName = "Protect - Foundation Square ? ")]
            public bool ProtectFoundation { get; set; }
            [JsonProperty(PropertyName = "Protect - Foundation Steps ? ")]
            public bool ProtectFoundationSteps { get; set; }
            [JsonProperty(PropertyName = "Protect - Foundation Triange ? ")]
            public bool ProtectFoundationTriangle { get; set; }
            [JsonProperty(PropertyName = "Protect - Wall Low ? ")]
            public bool ProtectLowWall { get; set; }
            [JsonProperty(PropertyName = "Protect - Wall Half ? ")]
            public bool ProtectHalfWall { get; set; }
            [JsonProperty(PropertyName = "Protect - Wall Full ? ")]
            public bool ProtectWall { get; set; }
            [JsonProperty(PropertyName = "Protect - Wall Frame ? ")]
            public bool ProtectWallFrame { get; set; }
            [JsonProperty(PropertyName = "Protect - Wall Window ? ")]
            public bool ProtectWindowWall { get; set; }
            [JsonProperty(PropertyName = "Protect - Wall Doorway ? ")]
            public bool ProtectDoorway { get; set; }
            [JsonProperty(PropertyName = "Protect - Floor Square ? ")]
            public bool ProtectFloor { get; set; }
            [JsonProperty(PropertyName = "Protect - Floor Triangle ? ")]
            public bool ProtectFloorTriangle { get; set; }
            [JsonProperty(PropertyName = "Protect - Floor Triangle Frame ? ")]
            public bool ProtectTriangleFrame { get; set; }
            [JsonProperty(PropertyName = "Protect - Floor Frame ? ")]
            public bool ProtectFloorFrame { get; set; }
            [JsonProperty(PropertyName = "Protect - Stairs L Shaped ? ")]
            public bool ProtectStairsLShaped { get; set; }
            [JsonProperty(PropertyName = "Protect - Stairs U Shaped ? ")]
            public bool ProtectStairsUShaped { get; set; }
            [JsonProperty(PropertyName = "Protect - Stairs Spiral ? ")]
            public bool ProtectStairsSpiral { get; set; }
            [JsonProperty(PropertyName = "Protect - Stairs Spiral Triangle ? ")]
            public bool ProtectStairsSpiralTriangle { get; set; }
            [JsonProperty(PropertyName = "Protect - Ramp ? ")]
            public bool ProtectRamp { get; set; }
            [JsonProperty(PropertyName = "Protect - Roof ? ")]
            public bool ProtectRoof { get; set; }
        }

        public static PluginConfig DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     Dictionary<string, string> messages;
    private bool HasPermission(BasePlayer player, string perm);
    private void OnEntityTakeDamage(BuildingBlock buildingBlock, HitInfo hitInfo);
    [ChatCommand("ProtectionScale")]
    private void chatCommand_ProtectionScale(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectFoundation")]
    private void chatCommand_ProtectFoundation(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectFoundationSteps")]
    private void chatCommand_ProtectFoundationSteps(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectFoundationTriangle")]
    private void chatCommand_ProtectFoundationTriangle(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectTriangleFrame")]
    private void chatCommand_ProtectTriangleFrame(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectWindowWall")]
    private void chatCommand_ProtectWindowWall(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectDoorway")]
    private void chatCommand_ProtectDoorway(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectFloor")]
    private void chatCommand_ProtectFloor(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectFloorTriangle")]
    private void chatCommand_ProtectFloorTriangle(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectHalfWall")]
    private void chatCommand_ProtectHalfWall(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectStairsLShaped")]
    private void chatCommand_ProtectStairsLShaped(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectStairsUShaped")]
    private void chatCommand_ProtectStairsUShaped(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectStairsSpiral")]
    private void chatCommand_ProtectStairsSpiral(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectStairsSpiralTriangle")]
    private void chatCommand_ProtectStairsSpiralTriangle(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectRoof")]
    private void chatCommand_ProtectRoof(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectRamp")]
    private void chatCommand_ProtectRamp(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectLowWall")]
    private void chatCommand_ProtectLowWall(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectWall")]
    private void chatCommand_ProtectWall(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectWallFrame")]
    private void chatCommand_ProtectWallFrame(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectFloorFrame")]
    private void chatCommand_ProtectFloorFrame(BasePlayer player, string command, string[] args);
}

private class PluginConfig
{
    public GlobalSettings globalSettings { get; set; }
    public class GlobalSettings
    {
        [JsonProperty(PropertyName = "Scale - Precent of damage allowed to any Building block that is currently protected (default is 0%) : ")]
        public float ProtectionScale { get; set; }
        [JsonProperty(PropertyName = "Protect - Foundation Square ? ")]
        public bool ProtectFoundation { get; set; }
        [JsonProperty(PropertyName = "Protect - Foundation Steps ? ")]
        public bool ProtectFoundationSteps { get; set; }
        [JsonProperty(PropertyName = "Protect - Foundation Triange ? ")]
        public bool ProtectFoundationTriangle { get; set; }
        [JsonProperty(PropertyName = "Protect - Wall Low ? ")]
        public bool ProtectLowWall { get; set; }
        [JsonProperty(PropertyName = "Protect - Wall Half ? ")]
        public bool ProtectHalfWall { get; set; }
        [JsonProperty(PropertyName = "Protect - Wall Full ? ")]
        public bool ProtectWall { get; set; }
        [JsonProperty(PropertyName = "Protect - Wall Frame ? ")]
        public bool ProtectWallFrame { get; set; }
        [JsonProperty(PropertyName = "Protect - Wall Window ? ")]
        public bool ProtectWindowWall { get; set; }
        [JsonProperty(PropertyName = "Protect - Wall Doorway ? ")]
        public bool ProtectDoorway { get; set; }
        [JsonProperty(PropertyName = "Protect - Floor Square ? ")]
        public bool ProtectFloor { get; set; }
        [JsonProperty(PropertyName = "Protect - Floor Triangle ? ")]
        public bool ProtectFloorTriangle { get; set; }
        [JsonProperty(PropertyName = "Protect - Floor Triangle Frame ? ")]
        public bool ProtectTriangleFrame { get; set; }
        [JsonProperty(PropertyName = "Protect - Floor Frame ? ")]
        public bool ProtectFloorFrame { get; set; }
        [JsonProperty(PropertyName = "Protect - Stairs L Shaped ? ")]
        public bool ProtectStairsLShaped { get; set; }
        [JsonProperty(PropertyName = "Protect - Stairs U Shaped ? ")]
        public bool ProtectStairsUShaped { get; set; }
        [JsonProperty(PropertyName = "Protect - Stairs Spiral ? ")]
        public bool ProtectStairsSpiral { get; set; }
        [JsonProperty(PropertyName = "Protect - Stairs Spiral Triangle ? ")]
        public bool ProtectStairsSpiralTriangle { get; set; }
        [JsonProperty(PropertyName = "Protect - Ramp ? ")]
        public bool ProtectRamp { get; set; }
        [JsonProperty(PropertyName = "Protect - Roof ? ")]
        public bool ProtectRoof { get; set; }
    }

    public static PluginConfig DefaultConfig();
}

public class GlobalSettings
{
    [JsonProperty(PropertyName = "Scale - Precent of damage allowed to any Building block that is currently protected (default is 0%) : ")]
    public float ProtectionScale { get; set; }
    [JsonProperty(PropertyName = "Protect - Foundation Square ? ")]
    public bool ProtectFoundation { get; set; }
    [JsonProperty(PropertyName = "Protect - Foundation Steps ? ")]
    public bool ProtectFoundationSteps { get; set; }
    [JsonProperty(PropertyName = "Protect - Foundation Triange ? ")]
    public bool ProtectFoundationTriangle { get; set; }
    [JsonProperty(PropertyName = "Protect - Wall Low ? ")]
    public bool ProtectLowWall { get; set; }
    [JsonProperty(PropertyName = "Protect - Wall Half ? ")]
    public bool ProtectHalfWall { get; set; }
    [JsonProperty(PropertyName = "Protect - Wall Full ? ")]
    public bool ProtectWall { get; set; }
    [JsonProperty(PropertyName = "Protect - Wall Frame ? ")]
    public bool ProtectWallFrame { get; set; }
    [JsonProperty(PropertyName = "Protect - Wall Window ? ")]
    public bool ProtectWindowWall { get; set; }
    [JsonProperty(PropertyName = "Protect - Wall Doorway ? ")]
    public bool ProtectDoorway { get; set; }
    [JsonProperty(PropertyName = "Protect - Floor Square ? ")]
    public bool ProtectFloor { get; set; }
    [JsonProperty(PropertyName = "Protect - Floor Triangle ? ")]
    public bool ProtectFloorTriangle { get; set; }
    [JsonProperty(PropertyName = "Protect - Floor Triangle Frame ? ")]
    public bool ProtectTriangleFrame { get; set; }
    [JsonProperty(PropertyName = "Protect - Floor Frame ? ")]
    public bool ProtectFloorFrame { get; set; }
    [JsonProperty(PropertyName = "Protect - Stairs L Shaped ? ")]
    public bool ProtectStairsLShaped { get; set; }
    [JsonProperty(PropertyName = "Protect - Stairs U Shaped ? ")]
    public bool ProtectStairsUShaped { get; set; }
    [JsonProperty(PropertyName = "Protect - Stairs Spiral ? ")]
    public bool ProtectStairsSpiral { get; set; }
    [JsonProperty(PropertyName = "Protect - Stairs Spiral Triangle ? ")]
    public bool ProtectStairsSpiralTriangle { get; set; }
    [JsonProperty(PropertyName = "Protect - Ramp ? ")]
    public bool ProtectRamp { get; set; }
    [JsonProperty(PropertyName = "Protect - Roof ? ")]
    public bool ProtectRoof { get; set; }
}


```

---

## DMBuildings by ColonBlow - Allows you to Scale or Null all Damage types against Buildings

```csharp
using Rust;
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("DMBuildings", "ColonBlow", "1.2.6")]
internal class DMBuildings : CovalencePlugin
{
    private const int DamageTypeMax;
    private readonly float[] _modifiers;
    private bool _didConfigChange;
    private void Loaded();
    protected override void LoadDefaultConfig();
    private void LoadConfigValues();
    private object GetConfigValue(string category, string setting, object defaultValue);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
}


```

---

## DMDeployables by ColonBlow - Prevents/allows damage to numerous deployables

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Damage Mod: Deployables", "ColonBlow", "1.1.13")]
[Description("Prevents/allows damage to numerous deployables")]
 class DMDeployables : RustPlugin
{
    private Dictionary<string, bool> deployables;
    private Dictionary<string, string> prefabs;
    private bool init;
     void OnServerInitialized();
     void Unload();
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    private bool Changed;
    private bool blockOnlyUnderTC;
     void LoadVariables();
    protected override void LoadDefaultConfig();
    private void CheckCfg(string Key, T var);
     object GetConfig(string menu, string datavalue, object defaultValue);
}


```

---

## DMPlayers by ColonBlow - Allows you to Scale or Null all Damage types against Players

```csharp
using System.Collections.Generic;
using System;
using Rust;

Oxide.Plugins
[Info("DMPlayers", "ColonBlow", "1.2.5")]
internal class DMPlayers : RustPlugin
{
    private const int DamageTypeMax;
    private readonly float[] _modifiers;
    private bool _didConfigChange;
    private void Loaded();
    protected override void LoadDefaultConfig();
    private void LoadConfigValues();
    private object GetConfigValue(string category, string setting, object defaultValue);
    private void OnEntityTakeDamage(BasePlayer player, HitInfo hitInfo);
}


```

---

## DonateCredits by  - Rewards players with Economy credits for donations on a website

```csharp
using UnityEngine;
using System.Collections.Generic;
using System;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using System.Linq;
using Rust;
using System.Text;
using Oxide.Ext.SQLite;

Oxide.Plugins
[Info("DonateCredits", "OldeTobeh", "1.1.0")]
[Description("Web based donation rewards, players can purchase rewards on website or in-game.")]
 class DonateCredits : RustPlugin
{
    [PluginReference]
    private Plugin Economics;
    private Dictionary<ulong, double> Balances;
    private readonly Core.MySql.Libraries.MySql _mySql;
    private Core.Database.Connection _mySqlConnection;
    public Dictionary<ulong, Dictionary<string, long>> playerList;
     bool packetSent;
    private Timer _timer;
     void LoadDefaultMessages();
     string Address { get; set; }
     int Port { get; set; }
     string dbName { get; set; }
     string User { get; set; }
     string Password { get; set; }
    protected override void LoadDefaultConfig();
    private void StartConnection();
    public void loadUser(BasePlayer player);
     void initPlayer(BasePlayer player, Dictionary<string, long> statsInit, List<Dictionary<string, object>> sqlData);
    public void setPointsAndLevel(ulong steam_id, string skill, long quantity);
     void setPlayerData(ulong steam_id, string key, long value);
     void initPlayerData(BasePlayer player, Dictionary<string, long> playerData);
    public void saveUser(BasePlayer player);
    public void rewardUser(BasePlayer player);
    public void loadUsers();
    public void saveUsers();
     void reloadUser(BasePlayer player);
     void LoadCommand(BasePlayer player, string command, string[] args);
     void SaveCommand(BasePlayer player, string command, string[] args);
     void RefreshCommand(BasePlayer player, string command, string[] args);
     void DonationRewardCommand(BasePlayer player, string command, string[] args);
     void DonationsCommand(BasePlayer player, string command, string[] args);
     bool usingMySQL();
    public Dictionary<string, long> getConnectedPlayerDetailsData(ulong steam_id);
    static string EncodeNonAsciiCharacters(string value);
    [HookMethod("SendHelpText")]
    private void SendHelpText(BasePlayer player);
    private bool HasAccess(BasePlayer player);
     bool HasPermission(string userId, string perm);
     string GetFormattedMoney(BasePlayer player);
    private void DepositPlayerMoney(BasePlayer player, double money);
    private bool inPlayerList(UInt64 userID);
     T GetConfig(string name, T defaultValue);
     string GetMessage(string key, string steamId);
    private void Loaded();
    private void Unload();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void OnTimer();
}


```

---

## DonationClaim by MrBlue - Players can claim rewards for automatic PayPal donations

```csharp
using System;
using System.Collections.Generic;
using System.Text;
using Oxide.Core.Database;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("DonationClaim", "Wulf/lukespragg", "1.0.2", ResourceId = 1923)]
[Description("Players can claim rewards for automatic PayPal donations")]
 class DonationClaim : CovalencePlugin
{
    readonly Core.MySql.Libraries.MySql mySql;
     Connection connection;
     DefaultConfig config;
     class DefaultConfig
    {
        readonly List<string> exampleCommands;
        public readonly Dictionary<string, List<string>> Packages;
        public DefaultConfig();
        public string DatabaseHost;
        public int DatabasePort;
        public string DatabaseName;
        public string DatabaseUser;
        public string DatabasePassword;
    }

    protected override void LoadDefaultConfig();
     void LoadDefaultMessages();
     void Init();
    [Command("claim", "claimdonation", "claimreward")]
     void ChatCommand(IPlayer player, string command, string[] args);
    static string GetPackageKey(string packageName, Dictionary<string, List<string>> packages);
     void RunConsoleCommands(List<string> commandsList, string playerName);
     T GetConfig(string name, T value);
     string Lang(string key, string id, object[] args);
}

 class DefaultConfig
{
    readonly List<string> exampleCommands;
    public readonly Dictionary<string, List<string>> Packages;
    public DefaultConfig();
    public string DatabaseHost;
    public int DatabasePort;
    public string DatabaseName;
    public string DatabaseUser;
    public string DatabasePassword;
}


```

---

## DontTargetMe by  - Makes Turrets, Bradley, Heli and NPCs ignore you

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Don't Target Me", "Quantum/Arainrr", "1.1.4")]
[Description("Makes turrets, player npcs and normal npcs ignore you.")]
public class DontTargetMe : RustPlugin
{
    private const string PERMISSION_ALL;
    private const string PERMISSION_NPC;
    private const string PERMISSION_APC;
    private const string PERMISSION_SAM;
    private const string PERMISSION_HELI;
    private const string PERMISSION_TURRETS;
    private const string PERMISSION_HBHF;
    private static object True;
    private static object False;
    private readonly Dictionary<ulong, TargetFlags> playerFlags;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private object CanBeTargeted(BasePlayer player, MonoBehaviour behaviour);
    private object OnNpcTarget(BaseEntity npc, BasePlayer player);
    private object CanBradleyApcTarget(BradleyAPC apc, BasePlayer player);
    private object CanHelicopterTarget(PatrolHelicopterAI heli, BasePlayer player);
    private object CanHelicopterStrafeTarget(PatrolHelicopterAI heli, BasePlayer player);
    private object OnSamSiteTarget(SamSite samSite, BaseCombatEntity baseCombatEntity);
    private object OnSensorDetect(HBHFSensor hbhf, BasePlayer player);
    private bool AnyHasTargetFlags(BaseCombatEntity baseCombatEntity, TargetFlags flag);
    private static IEnumerable<BasePlayer> GetMountedPlayers(BaseVehicle baseVehicle);
    private bool HasTargetFlags(BasePlayer player, TargetFlags flag);
    private bool PlayerFlagsInit(BasePlayer player);
    private void CheckHooks();
    private void CmdToggle(BasePlayer player, string command, string[] args);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Disable when player disconnected")]
        public bool disableWhenDis;
        [JsonProperty(PropertyName = "Enable when player connected")]
        public bool enableWhenCon;
        [JsonProperty(PropertyName = "Chat Settings")]
        public ChatSettings chatS;
        public class ChatSettings
        {
            [JsonProperty(PropertyName = "Chat Command")]
            public string command;
            [JsonProperty(PropertyName = "Chat Prefix")]
            public string prefix;
            [JsonProperty(PropertyName = "Chat SteamID Icon")]
            public ulong steamIDIcon;
        }

        [JsonProperty(PropertyName = "Version")]
        public VersionNumber version;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private bool GetConfigValue(T value, string[] path);
    private void Print(BasePlayer player, string message);
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Disable when player disconnected")]
    public bool disableWhenDis;
    [JsonProperty(PropertyName = "Enable when player connected")]
    public bool enableWhenCon;
    [JsonProperty(PropertyName = "Chat Settings")]
    public ChatSettings chatS;
    public class ChatSettings
    {
        [JsonProperty(PropertyName = "Chat Command")]
        public string command;
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string prefix;
        [JsonProperty(PropertyName = "Chat SteamID Icon")]
        public ulong steamIDIcon;
    }

    [JsonProperty(PropertyName = "Version")]
    public VersionNumber version;
}

public class ChatSettings
{
    [JsonProperty(PropertyName = "Chat Command")]
    public string command;
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string prefix;
    [JsonProperty(PropertyName = "Chat SteamID Icon")]
    public ulong steamIDIcon;
}


```

---

## DoorLimiter by  - Limits the usage of doors to a certain number of players.

```csharp
using System.Collections.Generic;
using UnityEngine;
using System;
using Oxide.Core;
using System.Globalization;

Oxide.Plugins
[Info("DoorLimiter", "redBDGR", "1.0.6", ResourceId = 2334)]
[Description("Only allow a certain number of people to use one door")]
 class DoorLimiter : RustPlugin
{
     bool Changed;
    public bool silentMode;
    public int authedPlayersAllowed;
    public const string permissionName;
    public const string permissionNameADMIN;
    public const string permissionNameREMOVE;
     void Init();
     void DoLang();
     object CanUseLockedEntity(BasePlayer player, BaseLock baselock);
     void LoadVariables();
    protected override void LoadDefaultConfig();
    [ChatCommand("doorlimit")]
     void doorlimitCMD(BasePlayer player, string command, string[] args);
     bool DoPlayerChecks(CodeLock codelock, BasePlayer player);
     void DoDoorLimitHelp(BasePlayer player);
    private static BasePlayer FindPlayer(string nameOrId);
     object GetConfig(string menu, string datavalue, object defaultValue);
     string msg(string key, string id);
}


```

---

## DoorLogs by mvrb - Check who opens/closes doors

```csharp
using Oxide.Core;
using System.Collections.Generic;
using UnityEngine;
using System;
using Newtonsoft.Json;
using System.Text.RegularExpressions;
using System.Linq;

Oxide.Plugins
[Info("Door Logs", "mvrb", "0.2.1")]
[Description("Check who opens/closes doors.")]
 class DoorLogs : RustPlugin
{
    private const string PermissionUse;
    private StoredData storedData;
    private void LoadDefaultMessages();
    private void OnServerInitialized();
    [ChatCommand("door")]
    private void ChatCmdCheckDoor(BasePlayer player);
    private void OnDoorClosed(Door door, BasePlayer player);
    private void OnDoorOpened(Door door, BasePlayer player);
    private void UpdateDoor(Door door, BasePlayer player);
    private class StoredData
    {
        public readonly Dictionary<ulong, DoorData> Doors;
    }

    private class DoorData
    {
        public readonly Dictionary<ulong, PlayerData> Entries;
    }

    private class PlayerData
    {
        public Int32 F;
        public Int32 L;
    }

    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Log /door output to console")]
        public bool LogToConsole { get; set; }
        [JsonProperty(PropertyName = "Max entries to log per door")]
        public int MaxEntries { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
    private string UnixToDateTime(double unixTimeStamp);
    private Int32 GetUnix();
    private string GetNameFromId(string id);
    private void LoadData();
    private void SaveData();
    private void OnNewSave(string name);
    private void OnServerSave();
    private string Lang(string key, string id, object[] args);
}

private class StoredData
{
    public readonly Dictionary<ulong, DoorData> Doors;
}

private class DoorData
{
    public readonly Dictionary<ulong, PlayerData> Entries;
}

private class PlayerData
{
    public Int32 F;
    public Int32 L;
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Log /door output to console")]
    public bool LogToConsole { get; set; }
    [JsonProperty(PropertyName = "Max entries to log per door")]
    public int MaxEntries { get; set; }
}


```

---

## DoorPermissions by TheForbiddenAi - Allows admins to lock doors to permissions

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using Oxide.Core.Plugins;
using UnityEngine;
using Physics = UnityEngine.Physics;

Oxide.Plugins
[Info("Door Permissions", "TheForbiddenAi", "2.0.0"),
     Description("Allows admins to lock doors to specific permissions.")]
public class DoorPermissions : RustPlugin
{
    private readonly DynamicConfigFile _dataFile;
    private ConfigData _configData;
    private const BUTTON SelectDoorButton;
    private const string DoorPermPrefix;
    private const string LockDoorPerm;
    private const string UnlockDoorPerm;
    private const string ActivateDoorPerm;
    private const string DeactivateDoorPerm;
    private const string ViewDoorInfoPerm;
    private const string SetDoorNamePerm;
    private const string SetTeleportDoorPerm;
    private const string SetTeleportPointPerm;
    private const string SetCategoryPerm;
    private const string SetCategoryNamePerm;
    private const string AddCategoryPermPerm;
    private const string RemoveCategoryPermPerm;
    private const string CreateCategoryPerm;
    private const string DeleteCategoryPerm;
    private const string ViewCategoryInfoPerm;
    private const string ViewCategoriesPerm;
    private List<string> _zDoorFronts;
    private List<string> _positiveEntrances;
    private string _version;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Date File Version - DO NOT MODIFY")]
        public string DataFileVersion;
        [JsonProperty(PropertyName = "Send No Permissons Message When Opening Door Without Required Permissions")]
        public bool SendInsufficientPerms;
        [JsonProperty(PropertyName = "Allow Damage To Locked Doors")]
        public bool AllowDamage;
        [JsonProperty(PropertyName = "Should Delete Doors On Category Deletion")]
        public bool ShouldDeleteDoors;
        public static ConfigData DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetDefaultConfig();
    private void SaveConfig();
    protected override void LoadDefaultMessages();
    private string GetMessage(string key, BasePlayer player, object[] args);
    private void Init();
    private void CacheDataObjects();
    private void UpdateDataFile(string oldVersion);
    private void InitializeQuaternionLists();
    private object CanUseLockedEntity(BasePlayer player, BaseLock baseLock);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnEntityKill(BaseNetworkable entity);
    [ChatCommand("lockdoor")]
    private void LockDoorCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("unlockdoor")]
    private void UnlockDoorCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("activatedoor")]
    private void ActivateDoorCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("deactivatedoor")]
    private void DeactivateDoorCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("viewdoorinfo")]
    private void ViewDoorInfoCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("setdoorname")]
    private void SetDoorNameCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("setteleportdoor")]
    private void SetTeleportDoorCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("setteleportpoint")]
    private void SetTeleportPointCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("setcategory")]
    private void SetCategoryCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("setcategoryname")]
    private void SetCategoryNameCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("addcategoryperm")]
    private void AddCategoryPermCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("removecategoryperm")]
    private void RemoveCategoryPermCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("createcategory")]
    private void CreateCategoryCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("deletecategory")]
    private void DeleteCategoryCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("viewcategoryinfo")]
    private void ViewCategoryInfoCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("viewcategories")]
    private void ViewCategoriesCommand(BasePlayer player, string command, string[] args);
    private Action LockDoor(BasePlayer player, string[] args);
    private Action UnlockDoor(BasePlayer player, string[] args);
    private Action ActivateDoor(BasePlayer player);
    private Action DeactivateDoor(BasePlayer player);
    private Action ViewDoorInfo(BasePlayer player);
    private Action SetDoorName(BasePlayer player, string newName);
    private Action SetTeleDoor(BasePlayer player);
    private Action SetTelePoint(BasePlayer player, Vector3 playerPosition);
    private Action SetCategory(BasePlayer player, CategoryObject category);
    public class CategoryObject
    {
        private static readonly List<CategoryObject> CategoryCache;
        [JsonIgnore]
        public int Id { get; set; }
        public string Name { get; set; }
        public List<DoorObject> Doors { get; set; }
        public List<string> Permissions { get; set; }
        public void MoveAllDoors(DoorPermissions plugin, int id);
        public static int GetNewCategoryId();
        public static bool CategoryExists(int id);
        public static CategoryObject GetCategoryById(int id);
        public static List<CategoryObject> GetCategories();
        public static void AddCategory(CategoryObject[] categoryArray);
        public static void DeleteCategory(DoorPermissions plugin, int id);
        public void Save(DoorPermissions plugin);
        private static void SaveCategories(DoorPermissions plugin, CategoryObject[] categoryArray);
    }

    public class DoorObject
    {
        private static readonly List<DoorObject> DoorCache;
        public int CategoryId { get; set; }
        public string Name { get; set; }
        public Vector3 Position { get; set; }
        public bool IsActive { get; set; }
        public bool IsTeleportationDoor { get; set; }
        public Vector3 TeleportationEntrance { get; set; }
        public Vector3 TeleportationExit { get; set; }
        public List<string> Permissions { get; set; }
        [JsonIgnore]
        public CategoryObject Category { get; set; }
        public List<string> GetAllPermissions();
        public static List<DoorObject> GetDoorByName(string name);
        public static DoorObject GetDoorByLocation(Vector3 position);
        public static void AddDoors(DoorObject[] doorArray);
        public static void AddDoorsToCache(DoorPermissions plugin, DoorObject[] doorArray);
        public static void RemoveDoorsFromCache(DoorObject[] doorArray);
        public void UpdateDoor(DoorPermissions plugin);
        public void DeleteDoor(DoorPermissions plugin);
        public bool SetCategory(DoorPermissions plugin, int id);
    }

    private class TimerHandler
    {
        private static readonly Dictionary<string, TimerHandler> ActiveTimers;
        private readonly Timer _handledTimer;
        private readonly Timer _expireTimer;
        public readonly DoorActions Action;
        public TimerHandler(DoorPermissions plugin, BasePlayer player, DoorActions action, float interval, Action callback, long expireAfterSeconds);
        public void CancelTimer();
        public static bool HasTimer(BasePlayer player);
        public static TimerHandler GetTimer(BasePlayer player);
        private static void AddTimer(BasePlayer player, TimerHandler handler);
        public static void RemoveTimer(BasePlayer player);
    }

    private bool HasPermissions(BasePlayer player, string[] permissions);
    public void UpdateDoorPermissions(DoorObject door, string[] perms);
    public void UpdateCategoryPermissions(CategoryObject category, string[] perms);
    public void RegisterPermissions(string[] permissions);
    private bool PutLockOnDoor(Door door, BasePlayer player);
    private void RemoveLockOnDoor(Door door);
    private static BaseEntity FindObject(Ray ray, float distance);
    private Door RetrieveDoorEntity(BasePlayer player);
    private bool IsDoorEntrance(Door door, Vector3 playerPosition);
    private CategoryObject TryGetCategory(BasePlayer player, string strId);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Date File Version - DO NOT MODIFY")]
    public string DataFileVersion;
    [JsonProperty(PropertyName = "Send No Permissons Message When Opening Door Without Required Permissions")]
    public bool SendInsufficientPerms;
    [JsonProperty(PropertyName = "Allow Damage To Locked Doors")]
    public bool AllowDamage;
    [JsonProperty(PropertyName = "Should Delete Doors On Category Deletion")]
    public bool ShouldDeleteDoors;
    public static ConfigData DefaultConfig();
}

public class CategoryObject
{
    private static readonly List<CategoryObject> CategoryCache;
    [JsonIgnore]
    public int Id { get; set; }
    public string Name { get; set; }
    public List<DoorObject> Doors { get; set; }
    public List<string> Permissions { get; set; }
    public void MoveAllDoors(DoorPermissions plugin, int id);
    public static int GetNewCategoryId();
    public static bool CategoryExists(int id);
    public static CategoryObject GetCategoryById(int id);
    public static List<CategoryObject> GetCategories();
    public static void AddCategory(CategoryObject[] categoryArray);
    public static void DeleteCategory(DoorPermissions plugin, int id);
    public void Save(DoorPermissions plugin);
    private static void SaveCategories(DoorPermissions plugin, CategoryObject[] categoryArray);
}

public class DoorObject
{
    private static readonly List<DoorObject> DoorCache;
    public int CategoryId { get; set; }
    public string Name { get; set; }
    public Vector3 Position { get; set; }
    public bool IsActive { get; set; }
    public bool IsTeleportationDoor { get; set; }
    public Vector3 TeleportationEntrance { get; set; }
    public Vector3 TeleportationExit { get; set; }
    public List<string> Permissions { get; set; }
    [JsonIgnore]
    public CategoryObject Category { get; set; }
    public List<string> GetAllPermissions();
    public static List<DoorObject> GetDoorByName(string name);
    public static DoorObject GetDoorByLocation(Vector3 position);
    public static void AddDoors(DoorObject[] doorArray);
    public static void AddDoorsToCache(DoorPermissions plugin, DoorObject[] doorArray);
    public static void RemoveDoorsFromCache(DoorObject[] doorArray);
    public void UpdateDoor(DoorPermissions plugin);
    public void DeleteDoor(DoorPermissions plugin);
    public bool SetCategory(DoorPermissions plugin, int id);
}

private class TimerHandler
{
    private static readonly Dictionary<string, TimerHandler> ActiveTimers;
    private readonly Timer _handledTimer;
    private readonly Timer _expireTimer;
    public readonly DoorActions Action;
    public TimerHandler(DoorPermissions plugin, BasePlayer player, DoorActions action, float interval, Action callback, long expireAfterSeconds);
    public void CancelTimer();
    public static bool HasTimer(BasePlayer player);
    public static TimerHandler GetTimer(BasePlayer player);
    private static void AddTimer(BasePlayer player, TimerHandler handler);
    public static void RemoveTimer(BasePlayer player);
}


```

---

## DraggableCorpses by VisEntities - Bring corpses to life and take them for a walk

```csharp
using Newtonsoft.Json;
using UnityEngine;
using System.Collections.Generic;
using System;
using System.Linq;

Oxide.Plugins
[Info("Draggable Corpses", "Dana", "2.0.0")]
[Description("Bring corpses to life and take them for a walk.")]
public class DraggableCorpses : RustPlugin
{
    private static DraggableCorpses _instance;
    private static Configuration _config;
    private CorpseDragController _corpseDragController;
    private class Configuration
    {
        [JsonProperty("Version")]
        public string Version { get; set; }
        [JsonProperty("Drag Button")]
        public string DragButton { get; set; }
        [JsonIgnore]
        public BUTTON Button { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfig();
    private Configuration GetDefaultConfig();
    private void Init();
    private void Unload();
    private object CanLootEntity(BasePlayer player, PlayerCorpse corpse);
    private class CorpseDragController
    {
        private Dictionary<BasePlayer, CorpseDragComponent> _components;
        public void RegisterDragger(CorpseDragComponent component);
        public void UnregisterDragger(CorpseDragComponent component);
        public bool StartDragging(PlayerCorpse corpse, BasePlayer player);
        public bool CorpseBeingDragged(PlayerCorpse corpse);
        public bool PlayerIsDraggingCorpse(BasePlayer player);
        public CorpseDragComponent GetComponentForPlayer(BasePlayer player);
        public void StopDraggingForPlayer(BasePlayer player);
        public void StopDraggingForCorpse(PlayerCorpse corpse);
        public void StopDraggingForAll();
    }

    private class CorpseDragComponent : FacepunchBehaviour
    {
        private CorpseDragController _corpseDragController;
        private static int _raycastLayers;
        public BasePlayer Dragger { get; set; }
        public PlayerCorpse Corpse { get; set; }
        private void StopDraggingCorpse();
        private void UpdateCorpsePosition();
        private void Update();
        private void OnDestroy();
        public static void InstallComponent(PlayerCorpse corpse, BasePlayer player, CorpseDragController corpseDragController);
        public CorpseDragComponent InitializeComponent(BasePlayer player, CorpseDragController corpseDragController);
        public static CorpseDragComponent GetComponent(PlayerCorpse corpse);
        public void DestroyComponent();
    }

    private static class PermissionUtils
    {
        public const string USE;
        public static void Register();
        public static bool Verify(BasePlayer player, string permissionName);
    }

}

private class Configuration
{
    [JsonProperty("Version")]
    public string Version { get; set; }
    [JsonProperty("Drag Button")]
    public string DragButton { get; set; }
    [JsonIgnore]
    public BUTTON Button { get; set; }
}

private class CorpseDragController
{
    private Dictionary<BasePlayer, CorpseDragComponent> _components;
    public void RegisterDragger(CorpseDragComponent component);
    public void UnregisterDragger(CorpseDragComponent component);
    public bool StartDragging(PlayerCorpse corpse, BasePlayer player);
    public bool CorpseBeingDragged(PlayerCorpse corpse);
    public bool PlayerIsDraggingCorpse(BasePlayer player);
    public CorpseDragComponent GetComponentForPlayer(BasePlayer player);
    public void StopDraggingForPlayer(BasePlayer player);
    public void StopDraggingForCorpse(PlayerCorpse corpse);
    public void StopDraggingForAll();
}

private class CorpseDragComponent : FacepunchBehaviour
{
    private CorpseDragController _corpseDragController;
    private static int _raycastLayers;
    public BasePlayer Dragger { get; set; }
    public PlayerCorpse Corpse { get; set; }
    private void StopDraggingCorpse();
    private void UpdateCorpsePosition();
    private void Update();
    private void OnDestroy();
    public static void InstallComponent(PlayerCorpse corpse, BasePlayer player, CorpseDragController corpseDragController);
    public CorpseDragComponent InitializeComponent(BasePlayer player, CorpseDragController corpseDragController);
    public static CorpseDragComponent GetComponent(PlayerCorpse corpse);
    public void DestroyComponent();
}

private static class PermissionUtils
{
    public const string USE;
    public static void Register();
    public static bool Verify(BasePlayer player, string permissionName);
}


```

---

## DroneBoombox by WhiteThunder - Allows players to deploy boomboxes onto RC drones

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Serialization;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Drone Boombox", "WhiteThunder", "1.0.2")]
[Description("Allows players to deploy boomboxes onto RC drones.")]
internal class DroneBoombox : CovalencePlugin
{
    [PluginReference]
    private Plugin DroneSettings;
    private static DroneBoombox _pluginInstance;
    private Configuration _pluginConfig;
    private const string PermissionDeploy;
    private const string PermissionDeployFree;
    private const string PermissionAutoDeploy;
    private const string BoomboxPrefab;
    private const string DeployEffectPrefab;
    private const int PortableBoomboxItemId;
    private const int DeployableBoomboxItemId;
    private const BaseEntity.Slot BoomboxSlot;
    private static readonly Vector3 BoomboxLocalPosition;
    private static readonly Quaternion BoomboxLocalRotation;
    private DynamicHookSubscriber<NetworkableId> _boomboxDroneTracker;
    private void Init();
    private void Unload();
    private void OnServerInitialized();
    private void OnEntityBuilt(Planner planner, GameObject go);
    private void OnEntityKill(Drone drone);
    private void OnEntityKill(DeployableBoomBox boombox);
    private bool? OnEntityTakeDamage(DeployableBoomBox boombox, HitInfo info);
    private bool? CanPickupEntity(BasePlayer player, Drone drone);
    private string canRemove(BasePlayer player, Drone drone);
    private string OnDroneTypeDetermine(Drone drone);
    private DeployableBoomBox API_DeployBoombox(Drone drone, BasePlayer player);
    [Command("droneboombox", "dronebb")]
    private void DroneBoomboxCommand(IPlayer player, string cmd, string[] args);
    private bool VerifyPermission(IPlayer player, string perm);
    private bool VerifyDroneFound(IPlayer player, Drone drone);
    private bool VerifyCanBuild(IPlayer player, Drone drone);
    private bool VerifyDroneHasNoBoombox(IPlayer player, Drone drone);
    private bool VerifyDroneHasSlotVacant(IPlayer player, Drone drone);
    private static bool DeployBoomboxWasBlocked(Drone drone, BasePlayer deployer);
    private static bool IsDroneEligible(Drone drone);
    private static BaseEntity GetLookEntity(BasePlayer basePlayer, float maxDistance);
    private static Drone GetParentDrone(BaseEntity entity);
    private static DeployableBoomBox GetDroneBoombox(Drone drone);
    private static bool CanPickupInternal(BasePlayer player, Drone drone);
    private static void HitNotify(BaseEntity entity, HitInfo info);
    private static void RunOnEntityBuilt(Item boomboxItem, DeployableBoomBox boombox);
    private static void UseItem(BasePlayer basePlayer, Item item, int amountToConsume);
    private static Item FindPlayerBoomboxItem(BasePlayer basePlayer);
    private void RefreshDroneSettingsProfile(Drone drone);
    private void SetupDroneBoombox(Drone drone, DeployableBoomBox boombox);
    private DeployableBoomBox DeployBoombox(Drone drone, BasePlayer basePlayer);
    private class DynamicHookSubscriber
    {
        private HashSet<T> _list;
        private string[] _hookNames;
        public DynamicHookSubscriber(string[] hookNames);
        public void Add(T item);
        public void Remove(T item);
        public void SubscribeAll();
        public void UnsubscribeAll();
    }

    private class Configuration : SerializableConfiguration
    {
        [JsonProperty("TipChance")]
        public int TipChance;
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
    private void ReplyToPlayer(IPlayer player, string messageName, object[] args);
    private void ChatMessage(BasePlayer player, string messageName, object[] args);
    private string GetMessage(IPlayer player, string messageName, object[] args);
    private string GetMessage(BasePlayer player, string messageName, object[] args);
    private string GetMessage(string playerId, string messageName, object[] args);
    private class Lang
    {
        public const string TipDeployCommand;
        public const string ErrorNoPermission;
        public const string ErrorNoDroneFound;
        public const string ErrorBuildingBlocked;
        public const string ErrorNoBoomboxItem;
        public const string ErrorAlreadyHasBoombox;
        public const string ErrorIncompatibleAttachment;
        public const string ErrorDeployFailed;
        public const string ErrorCannotPickupWithCassette;
    }

    protected override void LoadDefaultMessages();
}

private class DynamicHookSubscriber
{
    private HashSet<T> _list;
    private string[] _hookNames;
    public DynamicHookSubscriber(string[] hookNames);
    public void Add(T item);
    public void Remove(T item);
    public void SubscribeAll();
    public void UnsubscribeAll();
}

private class Configuration : SerializableConfiguration
{
    [JsonProperty("TipChance")]
    public int TipChance;
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

private class Lang
{
    public const string TipDeployCommand;
    public const string ErrorNoPermission;
    public const string ErrorNoDroneFound;
    public const string ErrorBuildingBlocked;
    public const string ErrorNoBoomboxItem;
    public const string ErrorAlreadyHasBoombox;
    public const string ErrorIncompatibleAttachment;
    public const string ErrorDeployFailed;
    public const string ErrorCannotPickupWithCassette;
}


```

---

## DroneEffects by WhiteThunder - (Obsolete) Adds collision effects, on-death effects and propeller animations to RC drones

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Serialization;
using Oxide.Core;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("Drone Effects", "WhiteThunder", "1.0.3")]
[Description("Adds collision effects and propeller animations to RC drones.")]
internal class DroneEffects : CovalencePlugin
{
    [PluginReference]
    private Plugin BetterDroneCollision;
    private static DroneEffects _pluginInstance;
    private static Configuration _pluginConfig;
    private const string DeliveryDronePrefab;
    private const float CollisionDistanceFraction;
    private bool _usingCustomCollisionListener;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnPluginLoaded(Plugin plugin);
    private void OnBookmarkControlStarted(ComputerStation station, BasePlayer player, string bookmarkName, Drone drone);
    private void OnBookmarkControlEnded(ComputerStation station, BasePlayer player, Drone drone);
    private void OnEntitySpawned(Drone drone);
    private void OnEntityDeath(Drone drone);
    private void OnDroneCollisionImpact(Drone drone, Collision collision);
    private void OnDroneControlStarted(Drone drone);
    private void OnDroneControlEnded(Drone drone);
    private void API_StopAnimating(Drone drone);
    private static bool AnimateWasBlocked(Drone drone);
    private static bool CollisionEffectWasBlocked(Drone drone, Collision collision);
    private static bool IsDroneEligible(Drone drone);
    private static Drone GetControlledDrone(ComputerStation computerStation);
    private static T GetChildOfType(BaseEntity entity);
    private void ShowCollisionEffect(Drone drone, Collision collision);
    private static DeliveryDrone GetChildDeliveryDrone(Drone drone);
    private static void SetupDeliveryDrone(DeliveryDrone deliveryDrone);
    private static void StartAnimationg(Drone drone);
    private static void MaybeStartAnimating(Drone drone);
    private static void StopAnimating(Drone drone);
    private class DroneCollisionListener : EntityComponent<Drone>
    {
        public static void DestroyAll();
        private const float DelayBetweenCollisions;
        private float _nextCollisionFXTime;
        private void OnCollisionEnter(Collision collision);
        private void ShowCollisionFX(Collision collision);
    }

    private class Configuration : SerializableConfiguration
    {
        [JsonProperty("Animation")]
        public AnimationSettings Animation;
        [JsonProperty("CollisionEffect")]
        public CollisionSettings CollisionEffect;
        [JsonProperty("DeathEffect")]
        public DeathSettings DeathEffect;
    }

    private class AnimationSettings
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
    }

    private class CollisionSettings
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("RequiredMagnitude")]
        public int RequiredMagnitude;
        [JsonProperty("EffectPrefab")]
        public string EffectPrefab;
    }

    private class DeathSettings
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("EffectPrefab")]
        public string EffectPrefab;
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

private class DroneCollisionListener : EntityComponent<Drone>
{
    public static void DestroyAll();
    private const float DelayBetweenCollisions;
    private float _nextCollisionFXTime;
    private void OnCollisionEnter(Collision collision);
    private void ShowCollisionFX(Collision collision);
}

private class Configuration : SerializableConfiguration
{
    [JsonProperty("Animation")]
    public AnimationSettings Animation;
    [JsonProperty("CollisionEffect")]
    public CollisionSettings CollisionEffect;
    [JsonProperty("DeathEffect")]
    public DeathSettings DeathEffect;
}

private class AnimationSettings
{
    [JsonProperty("Enabled")]
    public bool Enabled;
}

private class CollisionSettings
{
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("RequiredMagnitude")]
    public int RequiredMagnitude;
    [JsonProperty("EffectPrefab")]
    public string EffectPrefab;
}

private class DeathSettings
{
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("EffectPrefab")]
    public string EffectPrefab;
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

## DroneHover by WhiteThunder - Allows RC drones to hover in place when a player disconnects control at a computer station

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using System.Collections.Generic;

Oxide.Plugins
[Info("Drone Hover", "WhiteThunder", "1.0.5")]
[Description("Allows RC drones to hover in place when a player disconnects control at a computer station.")]
internal class DroneHover : CovalencePlugin
{
    private const string PermissionUse;
    private readonly object False;
    private StoredData _pluginData;
    private void Init();
    private void Unload();
    private void OnServerInitialized();
    private void OnServerSave();
    private void OnNewSave();
    private void OnBookmarkControlStarted(ComputerStation station, BasePlayer player, string bookmarkName, Drone drone);
    private void OnBookmarkControlEnded(ComputerStation station, BasePlayer player, Drone drone);
    private void OnEntityKill(Drone drone);
    private object canRemove(BasePlayer player, Drone drone);
    private void OnDroneControlEnded(Drone drone, BasePlayer player);
    private static class RCUtils
    {
        public static bool HasFakeController(IRemoteControllable controllable);
        public static void RemoveController(IRemoteControllable controllable);
        public static bool AddViewer(IRemoteControllable controllable, BasePlayer player);
        public static void RemoveViewer(IRemoteControllable controllable, BasePlayer player);
        public static bool AddFakeViewer(IRemoteControllable controllable);
    }

    private bool HoverWasBlocked(Drone drone, BasePlayer formerPilot);
    private bool ShouldHover(Drone drone, BasePlayer formerPilot);
    private void MaybeStartDroneHover(Drone drone, BasePlayer formerPilot);
    private class StoredData
    {
        [JsonProperty("HoveringDrones")]
        public HashSet<ulong> HoveringDrones;
        public static StoredData Load();
        public static StoredData Clear();
        public StoredData Save();
    }

}

private static class RCUtils
{
    public static bool HasFakeController(IRemoteControllable controllable);
    public static void RemoveController(IRemoteControllable controllable);
    public static bool AddViewer(IRemoteControllable controllable, BasePlayer player);
    public static void RemoveViewer(IRemoteControllable controllable, BasePlayer player);
    public static bool AddFakeViewer(IRemoteControllable controllable);
}

private class StoredData
{
    [JsonProperty("HoveringDrones")]
    public HashSet<ulong> HoveringDrones;
    public static StoredData Load();
    public static StoredData Clear();
    public StoredData Save();
}


```

---

## DroneLights by WhiteThunder - Adds controllable search lights to RC drones

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using UnityEngine;

Oxide.Plugins
[Info("Drone Lights", "WhiteThunder", "2.0.2")]
[Description("Adds controllable search lights to RC drones.")]
internal class DroneLights : CovalencePlugin
{
    private const string PermissionAutoDeploy;
    private const string PermissionMoveLight;
    private const string SpherePrefab;
    private const string SearchLightPrefab;
    private const float SearchLightYAxisRotation;
    private const float SearchLightScale;
    private static readonly Vector3 SphereEntityLocalPosition;
    private static readonly Vector3 SearchLightLocalPosition;
    private static readonly FieldInfo DronePitchField;
    private Configuration _config;
    private ProtectionProperties _immortalProtection;
    private void Init();
    private void Unload();
    private void OnServerInitialized();
    private void OnEntitySpawned(Drone drone);
    private void OnBookmarkControlStarted(ComputerStation station, BasePlayer player, string bookmarkName, Drone drone);
    private static bool DeployLightWasBlocked(Drone drone);
    private static bool IsDroneEligible(Drone drone);
    private static Drone GetControlledDrone(ComputerStation station);
    private static Drone GetControlledDrone(BasePlayer player);
    private static T2 GetGrandChildOfType(BaseEntity entity, T1 childOfType);
    private static SearchLight GetDroneSearchLight(Drone drone, SphereEntity parentSphere);
    private static SearchLight GetControlledSearchLight(BasePlayer player, SphereEntity parentSphere, Drone drone);
    private static SearchLight GetControlledSearchLight(BasePlayer player);
    private static void RemoveProblemComponents(BaseEntity entity);
    private static void HideInputsAndOutputs(IOEntity ioEntity);
    private static void SetLightAngle(Drone drone, SphereEntity sphere, Transform transform, float overrideAngle);
    private SearchLight TryDeploySearchLight(Drone drone);
    private void SetupSphereEntity(SphereEntity sphereEntity);
    private void SetupSearchLight(SearchLight searchLight);
    private void AddOrUpdateSearchLight(Drone drone);
    private void MaybeAutoDeploySearchLight(Drone drone);
    private class SearchLightUpdater : FacepunchBehaviour
    {
        public static void AddOrUpdateForDrone(DroneLights plugin, Drone drone, SphereEntity sphereEntity, SearchLight searchLight, BasePlayer controller, bool canMove);
        public static void RemoveFromDrone(Drone drone);
        private static SearchLightUpdater GetForDrone(Drone drone);
        private DroneLights _plugin;
        private Drone _drone;
        private SphereEntity _sphereEntity;
        private Transform _sphereTransform;
        private SearchLight _searchLight;
        private BasePlayer _controller;
        private bool _canMove;
        private void Update();
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class Configuration : BaseConfiguration
    {
        [JsonProperty("SearchLight")]
        public SearchLightSettings SearchLight;
    }

    private class SearchLightSettings
    {
        [JsonProperty("DefaultAngle")]
        public int DefaultAngle;
    }

    private Configuration GetDefaultConfig();
    [JsonObject(MemberSerialization.OptIn)]
    private class BaseConfiguration
    {
        private string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    private static class JsonHelper
    {
        public static object Deserialize(string json);
        private static object ToObject(JToken token);
    }

    private bool MaybeUpdateConfig(BaseConfiguration config);
    private bool MaybeUpdateConfigSection(Dictionary<string, object> currentWithDefaults, Dictionary<string, object> currentRaw);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
}

private class SearchLightUpdater : FacepunchBehaviour
{
    public static void AddOrUpdateForDrone(DroneLights plugin, Drone drone, SphereEntity sphereEntity, SearchLight searchLight, BasePlayer controller, bool canMove);
    public static void RemoveFromDrone(Drone drone);
    private static SearchLightUpdater GetForDrone(Drone drone);
    private DroneLights _plugin;
    private Drone _drone;
    private SphereEntity _sphereEntity;
    private Transform _sphereTransform;
    private SearchLight _searchLight;
    private BasePlayer _controller;
    private bool _canMove;
    private void Update();
}

[JsonObject(MemberSerialization.OptIn)]
private class Configuration : BaseConfiguration
{
    [JsonProperty("SearchLight")]
    public SearchLightSettings SearchLight;
}

private class SearchLightSettings
{
    [JsonProperty("DefaultAngle")]
    public int DefaultAngle;
}

[JsonObject(MemberSerialization.OptIn)]
private class BaseConfiguration
{
    private string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private static class JsonHelper
{
    public static object Deserialize(string json);
    private static object ToObject(JToken token);
}


```

---

## DronePilot by k1lly0u - Allows players to fly drones

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Network;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("DronePilot", "k1lly0u", "1.0.7")]
[Description("Allow players with permission to fly drones")]
 class DronePilot : RustPlugin
{
    private Hash<ulong, double> _cooldownTimes;
    private const string USE_PERMISSION;
    private const string CREATE_PERMISSION;
    private const string IGNORE_COOLDOWN_PERMISSION;
    private const string IGNORE_COST_PERMISSION;
    private const float DRONE_DISABLE_HEALTH_FRACTION;
    private void Loaded();
    protected override void LoadDefaultMessages();
    private object CanBuild(Planner planner, Construction prefab);
    private void OnEntityBuilt(Planner planner, GameObject gameObject);
    private void OnEntityTakeDamage(Drone drone, HitInfo hitInfo);
    private void OnPlayerWound(BasePlayer player, HitInfo hitInfo);
    private void OnPlayerDeath(BasePlayer player, HitInfo hitInfo);
    private void OnEntityDeath(Drone drone, HitInfo hitInfo);
    private object CanPickupEntity(BasePlayer player, Drone drone);
    private object CanSpectateTarget(BasePlayer player, string name);
    private object CanBeTargeted(BasePlayer player);
    private object CanBradleyApcTarget(BasePlayer player);
    private object CanHelicopterTarget(BasePlayer player);
    private void Unload();
    private static void MoveInventoryTo(PlayerInventory from, PlayerInventory to);
    private string FormatTime(double time);
    private object CanTeleport(BasePlayer player);
    private class DroneController : MonoBehaviour
    {
        public static List<DroneController> _allDroneControllers;
        public Drone Drone { get; set; }
        public BasePlayer Controller { get; set; }
        public BasePlayer Dummy { get; set; }
        public bool IsBeingControlled { get; set; }
        private DroneInputState _currentInput;
        private float _lastInputTime;
        private double _lastCollision;
        private bool _isGrounded;
        private Transform _tr;
        private Rigidbody _rb;
        private BaseMountable _mountPoint;
        private float _avgTerrainHeight;
        internal bool _killOnDestroy;
        private void Awake();
        private void OnDestroy();
        internal void StartPiloting(BasePlayer player);
        internal void OnPlayerDeath(HitInfo hitInfo);
        private void StopControllingDrone();
        private void RestorePlayer();
        private void CreateDummyPlayer();
        private void CreateMountPoint();
        private void UserInput(InputState inputState);
        private void FixedUpdate();
        private void OnCollisionEnter(Collision collision);
        private void OnCollisionStay();
        private void UpdateNetworkGroup();
    }

    [ChatCommand("drone")]
    private void GiveDrone(BasePlayer player, string command, string[] args);
    private static ConfigData Configuration;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Vertical acceleration speed")]
        public float AltitudeAcceleration { get; set; }
        [JsonProperty(PropertyName = "Horizontal acceleration speed")]
        public float MovementAcceleration { get; set; }
        [JsonProperty(PropertyName = "Yaw speed")]
        public float YawSpeed { get; set; }
        [JsonProperty(PropertyName = "The speed that the drone will return to a levelled rotation after leaning")]
        public float UprightSpeed { get; set; }
        [JsonProperty(PropertyName = "Lean weight (how much the drone leans in to movement) (0.0 -> 1.0)")]
        public float LeanWeight { get; set; }
        [JsonProperty(PropertyName = "Lean max velocity (the maximum velocity for full lean)")]
        public float LeanMaxVelocity { get; set; }
        [JsonProperty(PropertyName = "The impact speed before damage is applied")]
        public float HurtVelocityThreshold { get; set; }
        [JsonProperty(PropertyName = "The amount of damage to apply from impacts (scales depending on speed)")]
        public float HurtDamagePower { get; set; }
        [JsonProperty(PropertyName = "The amount of time to disable the drone controls after a collision (seconds)")]
        public float CollisionDisableTime { get; set; }
        [JsonProperty(PropertyName = "Maximum cruising height above terrain")]
        public float MaximumCruiseHeight { get; set; }
        [JsonProperty(PropertyName = "Drone should autohover and not be affected by gravity when no user input is detected")]
        public bool AutoHover { get; set; }
        [JsonProperty(PropertyName = "Drone damage scaler (does not affect collision damage)")]
        public float DamageScale { get; set; }
        [JsonProperty(PropertyName = "The cooldown time on using the /drone command (seconds)")]
        public int CooldownTime { get; set; }
        [JsonProperty(PropertyName = "The scrap cost associated with using the /drone command")]
        public int Cost { get; set; }
        public Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    public string Message(string key, ulong playerId);
    private static Func<string, ulong, string> GetMessage;
    private readonly Dictionary<string, string> Messages;
}

private class DroneController : MonoBehaviour
{
    public static List<DroneController> _allDroneControllers;
    public Drone Drone { get; set; }
    public BasePlayer Controller { get; set; }
    public BasePlayer Dummy { get; set; }
    public bool IsBeingControlled { get; set; }
    private DroneInputState _currentInput;
    private float _lastInputTime;
    private double _lastCollision;
    private bool _isGrounded;
    private Transform _tr;
    private Rigidbody _rb;
    private BaseMountable _mountPoint;
    private float _avgTerrainHeight;
    internal bool _killOnDestroy;
    private void Awake();
    private void OnDestroy();
    internal void StartPiloting(BasePlayer player);
    internal void OnPlayerDeath(HitInfo hitInfo);
    private void StopControllingDrone();
    private void RestorePlayer();
    private void CreateDummyPlayer();
    private void CreateMountPoint();
    private void UserInput(InputState inputState);
    private void FixedUpdate();
    private void OnCollisionEnter(Collision collision);
    private void OnCollisionStay();
    private void UpdateNetworkGroup();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Vertical acceleration speed")]
    public float AltitudeAcceleration { get; set; }
    [JsonProperty(PropertyName = "Horizontal acceleration speed")]
    public float MovementAcceleration { get; set; }
    [JsonProperty(PropertyName = "Yaw speed")]
    public float YawSpeed { get; set; }
    [JsonProperty(PropertyName = "The speed that the drone will return to a levelled rotation after leaning")]
    public float UprightSpeed { get; set; }
    [JsonProperty(PropertyName = "Lean weight (how much the drone leans in to movement) (0.0 -> 1.0)")]
    public float LeanWeight { get; set; }
    [JsonProperty(PropertyName = "Lean max velocity (the maximum velocity for full lean)")]
    public float LeanMaxVelocity { get; set; }
    [JsonProperty(PropertyName = "The impact speed before damage is applied")]
    public float HurtVelocityThreshold { get; set; }
    [JsonProperty(PropertyName = "The amount of damage to apply from impacts (scales depending on speed)")]
    public float HurtDamagePower { get; set; }
    [JsonProperty(PropertyName = "The amount of time to disable the drone controls after a collision (seconds)")]
    public float CollisionDisableTime { get; set; }
    [JsonProperty(PropertyName = "Maximum cruising height above terrain")]
    public float MaximumCruiseHeight { get; set; }
    [JsonProperty(PropertyName = "Drone should autohover and not be affected by gravity when no user input is detected")]
    public bool AutoHover { get; set; }
    [JsonProperty(PropertyName = "Drone damage scaler (does not affect collision damage)")]
    public float DamageScale { get; set; }
    [JsonProperty(PropertyName = "The cooldown time on using the /drone command (seconds)")]
    public int CooldownTime { get; set; }
    [JsonProperty(PropertyName = "The scrap cost associated with using the /drone command")]
    public int Cost { get; set; }
    public Core.VersionNumber Version { get; set; }
}


```

---

## DroneScaleManager by WhiteThunder - Utilities for resizing RC drones

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("Drone Scale Manager", "WhiteThunder", "1.0.1")]
[Description("Utilities for resizing RC drones.")]
internal class DroneScaleManager : CovalencePlugin
{
    [PluginReference]
     Plugin DroneEffects;
     Plugin EntityScaleManager;
    private static DroneScaleManager _pluginInstance;
    private static StoredData _pluginData;
    private const string PermissionScaleUnrestricted;
    private const string SpherePrefab;
    private const float VanillaDroneGroundTraceDistance;
    private const float VanillaDroneYawSpeed;
    private const float RootEntityLocalY;
    private static readonly Vector3 RootEntityPosition;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnServerSave();
    private void OnNewSave();
    private void OnPluginLoaded(Plugin plugin);
    private void OnEntityKill(Drone drone);
    private bool? OnDroneAnimationStart(Drone drone);
    private bool API_ScaleDrone(Drone drone, float scale);
    private bool API_ParentEntity(Drone drone, BaseEntity childEntity);
    private bool API_ParentTransform(Drone drone, Transform childTransform);
    private Drone API_GetParentDrone(BaseEntity entity);
    private SphereEntity API_GetRootEntity(Drone drone);
    [Command("scaledrone", "dronescale")]
    private void CommandDroneScale(IPlayer player, string cmd, string[] args);
    private static bool DroneScaleWasBlocked(Drone drone, float scale);
    private static bool VerifyDependencies();
    private static bool IsScaledDrone(Drone drone);
    private static float GetDroneScale(Drone drone);
    private static bool IsDroneEligible(Drone drone);
    private static BaseEntity GetLookEntity(BasePlayer basePlayer, float maxDistance);
    private static T GetGrandChildOfType(BaseEntity entity);
    private static void CopyRigidBodySettings(Rigidbody sourceBody, Rigidbody destinationBody);
    private static void MaybeSetupRootRigidBody(Drone scaledDrone, SphereEntity rootEntity);
    private static void RestoreRigidBody(Drone scaledDrone, SphereEntity rootEntity);
    private static void EnableGlobalBroadcastFixed(BaseEntity entity, bool wants);
    private static void SetupRootEntity(Drone drone, SphereEntity rootEntity);
    private static void SetupRootEntityAfterSpawn(Drone drone, SphereEntity rootEntity);
    private static void SetupScaledDrone(Drone drone, float scale);
    private static void RefreshScaledDrone(Drone drone);
    private void RefreshAllScaledDrones();
    private static void PositionChildTransform(Transform rootTransform, Transform droneTransform, Transform childTransform);
    private static SphereEntity GetParentSphere(BaseEntity entity);
    private static SphereEntity GetRootEntity(Drone drone, SphereEntity parentSphere);
    private static SphereEntity GetRootEntity(Drone drone);
    private static SphereEntity GetRootEntityOrParentSphere(Drone drone);
    private static SphereEntity AddRootEntity(Drone drone);
    private static void RemoveRootEntity(Drone scaledDrone);
    private static bool ScaleDrone(Drone drone, SphereEntity rootEntity, float scale, float currentScale);
    private static bool TryScaleDrone(Drone drone, float desiredScale);
    private class DroneCollisionProxy : MonoBehaviour
    {
        private const string OnCollisionEnterMethodName;
        private const string OnCollisionStayMethodName;
        public static void DestroyAll();
        public Drone OwnerDrone;
        private void OnCollisionEnter(Collision collision);
        private void OnCollisionStay();
    }

    private class StoredData
    {
        [JsonProperty("ScaledDrones")]
        public HashSet<ulong> ScaledDrones;
        public static StoredData Load();
        public static StoredData Clear();
        public StoredData Save();
    }

    private void ReplyToPlayer(IPlayer player, string messageName, object[] args);
    private string GetMessage(IPlayer player, string messageName, object[] args);
    private string GetMessage(string playerId, string messageName, object[] args);
    private class Lang
    {
        public const string ErrorNoPermission;
        public const string ErrorSyntax;
        public const string ErrorNoDroneFound;
        public const string ScaleSuccess;
        public const string ScaleError;
    }

    protected override void LoadDefaultMessages();
}

private class DroneCollisionProxy : MonoBehaviour
{
    private const string OnCollisionEnterMethodName;
    private const string OnCollisionStayMethodName;
    public static void DestroyAll();
    public Drone OwnerDrone;
    private void OnCollisionEnter(Collision collision);
    private void OnCollisionStay();
}

private class StoredData
{
    [JsonProperty("ScaledDrones")]
    public HashSet<ulong> ScaledDrones;
    public static StoredData Load();
    public static StoredData Clear();
    public StoredData Save();
}

private class Lang
{
    public const string ErrorNoPermission;
    public const string ErrorSyntax;
    public const string ErrorNoDroneFound;
    public const string ScaleSuccess;
    public const string ScaleError;
}


```

---

## DroneStorage by WhiteThunder - Allows players to deploy a small stash to RC drones

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Drone Storage", "WhiteThunder", "1.3.1")]
[Description("Allows players to deploy a small stash to RC drones.")]
internal class DroneStorage : CovalencePlugin
{
    [PluginReference]
    private readonly Plugin DroneSettings;
    private Configuration _config;
    private const string PermissionDeploy;
    private const string PermissionDeployFree;
    private const string PermissionAutoDeploy;
    private const string PermissionLockable;
    private const string PermissionViewItems;
    private const string PermissionDropItems;
    private const string PermissionToggleLock;
    private const string PermissionCapacityPrefix;
    private const string StoragePrefab;
    private const string StorageDeployEffectPrefab;
    private const string DropBagPrefab;
    private const string LockEffectPrefab;
    private const string UnlockEffectPrefab;
    private const int StashItemId;
    private const BaseEntity.Slot StorageSlot;
    private const string ResizableLootPanelName;
    private static readonly Vector3 StorageLockPosition;
    private static readonly Quaternion StorageLockRotation;
    private static readonly Vector3 StorageLocalPosition;
    private static readonly Quaternion StorageLocalRotation;
    private static readonly Vector3 StorageDropForwardLocation;
    private static readonly Quaternion StorageDropRotation;
    private readonly object True;
    private readonly object False;
    private readonly Func<Item, int, bool> StashItemFilter;
    private readonly Dictionary<string, object> _removeInfo;
    private readonly Dictionary<string, object> _refundInfo;
    private readonly object[] NoteInvTakeOneArguments;
    private readonly StorageCapacityManager _storageCapacityManager;
    private readonly DynamicHookHashSet<StorageContainer> _droneStorageTracker;
    private readonly DynamicHookHashSet<ulong> _remoteStashViewerTracker;
    private readonly HashSet<BasePlayer> _uiViewers;
    public DroneStorage();
    private void Init();
    private void Unload();
    private void OnServerInitialized();
    private void OnEntityBuilt(Planner planner, GameObject go);
    private void OnEntityDeath(Drone drone);
    private object OnEntityTakeDamage(StorageContainer storage, HitInfo info);
    private void OnBookmarkControlStarted(ComputerStation computerStation, BasePlayer player, string bookmarkName, Drone drone);
    private void OnBookmarkControlEnded(ComputerStation station, BasePlayer player, Drone drone);
    private object CanPickupEntity(BasePlayer player, Drone drone);
    private void OnItemDeployed(Deployer deployer, StorageContainer storage, BaseLock baseLock);
    private object CanMoveItem(Item item, PlayerInventory playerInventory);
    private object OnItemAction(Item item, string text, BasePlayer player);
    private Dictionary<string, object> OnRemovableEntityInfo(StorageContainer storage, BasePlayer player);
    private string canRemove(BasePlayer player, Drone drone);
    private string OnDroneTypeDetermine(Drone drone);
    [Command("dronestash")]
    private void DroneStashCommand(IPlayer player);
    [Command("dronestorage.ui.viewitems")]
    private void UICommandViewItems(IPlayer player);
    [Command("dronestorage.ui.dropitems")]
    private void UICommandDropItems(IPlayer player);
    [Command("dronestorage.ui.togglelock")]
    private void UICommandLockStorage(IPlayer player);
    private static class UI
    {
        private const string Name;
        private static float GetButtonOffsetX(DroneStorage plugin, int index, int totalButtons);
        public static void CreateForPlayer(DroneStorage plugin, BasePlayer player, StorageContainer storage);
        public static void DestroyForPlayer(BasePlayer player);
    }

    private static class RCUtils
    {
        public static bool IsRCDrone(Drone drone);
        public static bool HasController(IRemoteControllable controllable, BasePlayer player);
        public static T GetControlledEntity(BasePlayer player, ComputerStation station);
        public static T GetControlledEntity(BasePlayer player);
    }

    private static bool DeployStorageWasBlocked(Drone drone, BasePlayer deployer);
    private static bool DropStorageWasBlocked(Drone drone, StorageContainer storage, BasePlayer pilot);
    private void RefreshDroneSettingsProfile(Drone drone);
    private static bool IsDroneEligible(Drone drone);
    private static bool IsDroneStorage(StorageContainer storage, Drone drone);
    private static bool IsDroneStorage(StorageContainer storage);
    private static bool CanPickupInternal(BasePlayer player, Drone drone);
    private static StorageContainer GetDroneStorage(Drone drone);
    private static BaseLock GetLock(StorageContainer storage);
    private static void HitNotify(BaseEntity entity, HitInfo info);
    private static void RemoveProblemComponents(BaseEntity ent);
    private static void DropItems(Drone drone, StorageContainer storage, BasePlayer pilot);
    private static BaseEntity GetLookEntity(BasePlayer basePlayer, float maxDistance);
    private static void EndLooting(BasePlayer player);
    private bool CanStashAcceptItem(Item item, int position);
    private bool TryGetControlledStorage(IPlayer player, string perm, BasePlayer basePlayer, Drone drone, StorageContainer storage);
    private void SetupDroneStorage(Drone drone, StorageContainer storage, int capacity);
    private StorageContainer TryDeployStorage(Drone drone, int capacity, bool allowRefund, BasePlayer deployer);
    private void RefreshStorage(Drone drone);
    private class DroneStorageComponent : MonoBehaviour
    {
        public static void AddToStorage(DroneStorage plugin, Drone drone, StorageContainer storageContainer);
        public static void RemoveFromStorage(StorageContainer storageContainer);
        private DroneStorage _plugin;
        private Drone _drone;
        private StorageContainer _storage;
        private void PlayerStoppedLooting(BasePlayer looter);
        private void OnDestroy();
    }

    private class HookCollection
    {
        public bool IsSubscribed { get; set; }
        private readonly DroneStorage _plugin;
        private readonly string[] _hookNames;
        private readonly Func<bool> _shouldSubscribe;
        public HookCollection(DroneStorage plugin, Func<bool> shouldSubscribe, string[] hookNames);
        public void Subscribe();
        public void Unsubscribe();
        public void Refresh();
    }

    private class DynamicHookHashSet : HashSet<T>
    {
        private readonly HookCollection _hookCollection;
        public DynamicHookHashSet(DroneStorage plugin, string[] hookNames);
        public new bool Add(T item);
        public new bool Remove(T item);
        public void Unsubscribe();
    }

    private class StorageCapacityManager
    {
        private class StorageSize
        {
            public readonly int Capacity;
            public readonly string Permission;
            public StorageSize(int capacity, string permission);
        }

        private readonly DroneStorage _plugin;
        private StorageSize[] _sortedStorageSizes;
        public StorageCapacityManager(DroneStorage plugin);
        public void Init();
        public int DetermineCapacityForUser(ulong userId);
    }

    private class Configuration : BaseConfiguration
    {
        [JsonProperty("TipChance")]
        public int TipChance;
        [JsonProperty("AssignStorageOwnership")]
        public bool AssignStorageOwnership;
        [JsonProperty("CapacityAmounts")]
        public int[] CapacityAmounts;
        [JsonProperty("DisallowedItems")]
        public string[] DisallowedItems;
        [JsonProperty("DisallowedSkins")]
        public ulong[] DisallowedSkins;
        [JsonProperty("UISettings")]
        public UISettings UISettings;
    }

    private class UISettings
    {
        [JsonProperty("AnchorMin")]
        public string AnchorMin;
        [JsonProperty("AnchorMax")]
        public string AnchorMax;
        [JsonProperty("OffsetMin")]
        public string OffsetMin;
        [JsonProperty("OffsetMax")]
        public string OffsetMax;
        [JsonProperty("Buttons")]
        public UIButtons Buttons;
    }

    private class UIButtons
    {
        [JsonProperty("Spacing")]
        public int Spacing;
        [JsonProperty("Width")]
        public int Width;
        [JsonProperty("Height")]
        public int Height;
        [JsonProperty("TextSize")]
        public int TextSize;
        [JsonProperty("ViewButtonColor")]
        public string ViewButtonColor;
        [JsonProperty("ViewButtonTextColor")]
        public string ViewButtonTextColor;
        [JsonProperty("DropButtonColor")]
        public string DropButtonColor;
        [JsonProperty("DropButtonTextColor")]
        public string DropButtonTextColor;
        [JsonProperty("LockButtonColor")]
        public string LockButtonColor;
        [JsonProperty("LockButtonTextColor")]
        public string LockButtonTextColor;
        [JsonProperty("UnlockButtonColor")]
        public string UnlockButtonColor;
        [JsonProperty("UnlockButtonTextColor")]
        public string UnlockButtonTextColor;
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
    private string GetMessage(BasePlayer player, string messageName, object[] args);
    private void ReplyToPlayer(IPlayer player, string messageName, object[] args);
    private void ChatMessage(BasePlayer player, string messageName, object[] args);
    private class Lang
    {
        public const string UIButtonViewItems;
        public const string UIButtonDropItems;
        public const string UIButtonLockStorage;
        public const string UIButtonUnlockStorage;
        public const string TipDeployCommand;
        public const string InfoStashName;
        public const string ErrorNoPermission;
        public const string ErrorBuildingBlocked;
        public const string ErrorNoDroneFound;
        public const string ErrorNoStashItem;
        public const string ErrorAlreadyHasStorage;
        public const string ErrorIncompatibleAttachment;
        public const string ErrorDeployFailed;
        public const string ErrorCannotPickupDroneWithItems;
    }

    protected override void LoadDefaultMessages();
}

private static class UI
{
    private const string Name;
    private static float GetButtonOffsetX(DroneStorage plugin, int index, int totalButtons);
    public static void CreateForPlayer(DroneStorage plugin, BasePlayer player, StorageContainer storage);
    public static void DestroyForPlayer(BasePlayer player);
}

private static class RCUtils
{
    public static bool IsRCDrone(Drone drone);
    public static bool HasController(IRemoteControllable controllable, BasePlayer player);
    public static T GetControlledEntity(BasePlayer player, ComputerStation station);
    public static T GetControlledEntity(BasePlayer player);
}

private class DroneStorageComponent : MonoBehaviour
{
    public static void AddToStorage(DroneStorage plugin, Drone drone, StorageContainer storageContainer);
    public static void RemoveFromStorage(StorageContainer storageContainer);
    private DroneStorage _plugin;
    private Drone _drone;
    private StorageContainer _storage;
    private void PlayerStoppedLooting(BasePlayer looter);
    private void OnDestroy();
}

private class HookCollection
{
    public bool IsSubscribed { get; set; }
    private readonly DroneStorage _plugin;
    private readonly string[] _hookNames;
    private readonly Func<bool> _shouldSubscribe;
    public HookCollection(DroneStorage plugin, Func<bool> shouldSubscribe, string[] hookNames);
    public void Subscribe();
    public void Unsubscribe();
    public void Refresh();
}

private class DynamicHookHashSet : HashSet<T>
{
    private readonly HookCollection _hookCollection;
    public DynamicHookHashSet(DroneStorage plugin, string[] hookNames);
    public new bool Add(T item);
    public new bool Remove(T item);
    public void Unsubscribe();
}

private class StorageCapacityManager
{
    private class StorageSize
    {
        public readonly int Capacity;
        public readonly string Permission;
        public StorageSize(int capacity, string permission);
    }

    private readonly DroneStorage _plugin;
    private StorageSize[] _sortedStorageSizes;
    public StorageCapacityManager(DroneStorage plugin);
    public void Init();
    public int DetermineCapacityForUser(ulong userId);
}

private class StorageSize
{
    public readonly int Capacity;
    public readonly string Permission;
    public StorageSize(int capacity, string permission);
}

private class Configuration : BaseConfiguration
{
    [JsonProperty("TipChance")]
    public int TipChance;
    [JsonProperty("AssignStorageOwnership")]
    public bool AssignStorageOwnership;
    [JsonProperty("CapacityAmounts")]
    public int[] CapacityAmounts;
    [JsonProperty("DisallowedItems")]
    public string[] DisallowedItems;
    [JsonProperty("DisallowedSkins")]
    public ulong[] DisallowedSkins;
    [JsonProperty("UISettings")]
    public UISettings UISettings;
}

private class UISettings
{
    [JsonProperty("AnchorMin")]
    public string AnchorMin;
    [JsonProperty("AnchorMax")]
    public string AnchorMax;
    [JsonProperty("OffsetMin")]
    public string OffsetMin;
    [JsonProperty("OffsetMax")]
    public string OffsetMax;
    [JsonProperty("Buttons")]
    public UIButtons Buttons;
}

private class UIButtons
{
    [JsonProperty("Spacing")]
    public int Spacing;
    [JsonProperty("Width")]
    public int Width;
    [JsonProperty("Height")]
    public int Height;
    [JsonProperty("TextSize")]
    public int TextSize;
    [JsonProperty("ViewButtonColor")]
    public string ViewButtonColor;
    [JsonProperty("ViewButtonTextColor")]
    public string ViewButtonTextColor;
    [JsonProperty("DropButtonColor")]
    public string DropButtonColor;
    [JsonProperty("DropButtonTextColor")]
    public string DropButtonTextColor;
    [JsonProperty("LockButtonColor")]
    public string LockButtonColor;
    [JsonProperty("LockButtonTextColor")]
    public string LockButtonTextColor;
    [JsonProperty("UnlockButtonColor")]
    public string UnlockButtonColor;
    [JsonProperty("UnlockButtonTextColor")]
    public string UnlockButtonTextColor;
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
    public const string UIButtonViewItems;
    public const string UIButtonDropItems;
    public const string UIButtonLockStorage;
    public const string UIButtonUnlockStorage;
    public const string TipDeployCommand;
    public const string InfoStashName;
    public const string ErrorNoPermission;
    public const string ErrorBuildingBlocked;
    public const string ErrorNoDroneFound;
    public const string ErrorNoStashItem;
    public const string ErrorAlreadyHasStorage;
    public const string ErrorIncompatibleAttachment;
    public const string ErrorDeployFailed;
    public const string ErrorCannotPickupDroneWithItems;
}


```

---

## DroneTurrets by WhiteThunder - Allows players to deploy auto turrets onto RC drones

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("Drone Turrets", "WhiteThunder", "1.3.3")]
[Description("Allows players to deploy auto turrets to RC drones.")]
internal class DroneTurrets : CovalencePlugin
{
    [PluginReference]
    private readonly Plugin DroneSettings;
    private readonly Plugin EntityScaleManager;
    private Configuration _config;
    private const float TurretScale;
    private const string PermissionDeploy;
    private const string PermissionDeployNpc;
    private const string PermissionDeployFree;
    private const string PermissionAutoDeploy;
    private const string PermissionControl;
    private const string SpherePrefab;
    private const string AutoTurretPrefab;
    private const string NpcAutoTurretPrefab;
    private const string ElectricSwitchPrefab;
    private const string AlarmPrefab;
    private const string SirenLightPrefab;
    private const string DeployEffectPrefab;
    private const string CodeLockDeniedEffectPrefab;
    private const int AutoTurretItemId;
    private const BaseEntity.Slot TurretSlot;
    private static readonly Vector3 SphereEntityLocalPosition;
    private static readonly Vector3 TurretSwitchPosition;
    private static readonly Quaternion TurretSwitchRotation;
    private static readonly Vector3 SphereTransformScale;
    private static readonly Vector3 TurretTransformScale;
    private readonly object True;
    private readonly object False;
    private DynamicHookSubscriber<NetworkableId> _turretDroneTracker;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private void OnEntityBuilt(Planner planner, GameObject go);
    private object OnSwitchToggle(ElectricSwitch electricSwitch, BasePlayer player);
    private void OnSwitchToggled(ElectricSwitch electricSwitch);
    private object OnTurretTarget(AutoTurret turret, BaseCombatEntity target);
    private object OnEntityTakeDamage(AutoTurret turret, HitInfo info);
    private object OnEntityTakeDamage(ElectricSwitch electricSwitch, HitInfo info);
    private void OnEntityKill(Drone drone);
    private void OnEntityKill(AutoTurret turret);
    private void OnEntityDeath(Drone drone);
    private object CanPickupEntity(BasePlayer player, Drone drone);
    private void OnBookmarkControlStarted(ComputerStation station, BasePlayer player, string bookmarkName, Drone drone);
    private void OnBookmarkControlStarted(ComputerStation station, BasePlayer player, string bookmarkName, AutoTurret turret);
    private void OnBookmarkControlEnded(ComputerStation station, BasePlayer player, Drone drone);
    private void OnBookmarkControlEnded(ComputerStation station, BasePlayer player, AutoTurret turret);
    private string canRemove(BasePlayer player, Drone drone);
    private string OnDroneTypeDetermine(Drone drone);
    private AutoTurret API_DeployAutoTurret(Drone drone, BasePlayer player);
    private NPCAutoTurret API_DeployNpcAutoTurret(Drone drone, BasePlayer player);
    private static class ExposedHooks
    {
        public static void OnBookmarkControlStarted(ComputerStation station, BasePlayer player, string name, IRemoteControllable controllable);
        public static void OnBookmarkControlEnded(ComputerStation station, BasePlayer player, IRemoteControllable controllable);
        public static void OnEntityBuilt(BaseEntity heldEntity, GameObject gameObject);
        public static object OnDroneTurretDeploy(Drone drone, BasePlayer deployer);
        public static void OnDroneTurretDeployed(Drone drone, AutoTurret turret, BasePlayer deployer);
        public static object OnDroneNpcTurretDeploy(Drone drone, BasePlayer deployer);
        public static void OnDroneNpcTurretDeployed(Drone drone, NPCAutoTurret turret, BasePlayer deployer);
    }

    [Command("droneturret")]
    private void DroneTurretCommand(IPlayer player);
    [Command("dronenpcturret")]
    private void DroneNpcTurretCommand(IPlayer player, string cmd, string[] args);
    private bool VerifyPermission(IPlayer player, string perm);
    private bool VerifyDroneFound(IPlayer player, Drone drone);
    private bool VerifyCanBuild(IPlayer player, Drone drone);
    private bool VerifyDroneHasNoTurret(IPlayer player, Drone drone);
    private bool VerifyDroneHasSlotVacant(IPlayer player, Drone drone);
    private static class RCUtils
    {
        public static bool IsRCDrone(Drone drone);
        public static bool HasController(IRemoteControllable controllable);
        public static bool HasController(IRemoteControllable controllable, BasePlayer player);
        public static bool HasFakeController(IRemoteControllable controllable);
        public static bool HasRealController(IRemoteControllable controllable);
        public static bool CanControl(BasePlayer player, IRemoteControllable controllable);
        public static void RemoveController(IRemoteControllable controllable);
        public static bool AddViewer(IRemoteControllable controllable, BasePlayer player);
        public static void RemoveViewer(IRemoteControllable controllable, BasePlayer player);
        public static bool AddFakeViewer(IRemoteControllable controllable);
        public static T GetControlledEntity(BasePlayer player, ComputerStation station);
        public static T GetControlledEntity(BasePlayer player);
    }

    private static bool DeployTurretWasBlocked(Drone drone, BasePlayer deployer);
    private static bool DeployNpcTurretWasBlocked(Drone drone, BasePlayer deployer);
    private static Drone GetParentDrone(BaseEntity entity, SphereEntity parentSphere);
    private static Drone GetParentDrone(BaseEntity entity);
    private static AutoTurret GetDroneTurret(Drone drone);
    private static T GetChildOfType(BaseEntity entity, string prefabName);
    private static IOEntity GetTurretAlarm(AutoTurret turret);
    private static IOEntity GetTurretLight(AutoTurret turret);
    private static bool ShouldPowerAlarm(Drone drone, AutoTurret turret);
    private static bool CanPickupInternal(Drone drone);
    private static void HitNotify(BaseEntity entity, HitInfo info);
    private static SphereEntity SpawnSphereEntity(Drone drone);
    private static void RemoveProblemComponents(BaseEntity entity);
    private static void AddRigidBodyToTriggerCollider(AutoTurret turret);
    private static IOEntity AttachTurretAlarm(Drone drone, AutoTurret turret);
    private static IOEntity AttachTurretLight(Drone drone, AutoTurret turret);
    private static ElectricSwitch AttachTurretSwitch(AutoTurret autoTurret);
    private static void HideInputsAndOutputs(IOEntity ioEntity);
    private static void SetupTurretSwitch(ElectricSwitch electricSwitch);
    private static void SetupSphereEntity(SphereEntity sphereEntity);
    private static BaseEntity GetLookEntity(BasePlayer basePlayer, float maxDistance);
    private static AutoTurret GetParentTurret(BaseEntity entity);
    private static void RunOnEntityBuilt(Item turretItem, AutoTurret autoTurret);
    private static void UseItem(BasePlayer basePlayer, Item item, int amountToConsume);
    private static float GetItemConditionFraction(Item item);
    private static Item FindPlayerAutoTurretItem(BasePlayer basePlayer);
    private void RefreshDroneSettingsProfile(Drone drone);
    private void SwitchControl(BasePlayer player, ComputerStation station, IRemoteControllable previous, IRemoteControllable next);
    private bool HasPermissionToControl(BasePlayer player);
    private void RegisterWithEntityScaleManager(BaseEntity entity);
    private void RefreshAlarmState(Drone drone, AutoTurret turret);
    private void SetupDroneTurret(Drone drone, AutoTurret turret);
    private void RefreshDroneTurret(Drone drone, AutoTurret turret);
    private NPCAutoTurret DeployNpcAutoTurret(Drone drone, BasePlayer deployer);
    private AutoTurret DeployAutoTurret(Drone drone, BasePlayer basePlayer, float conditionFraction);
    private object HandleLightToggle(BasePlayer player);
    private void HandleSwapSeats(BasePlayer player);
    private class DynamicHookSubscriber
    {
        private DroneTurrets _plugin;
        private HashSet<T> _list;
        private string[] _hookNames;
        public DynamicHookSubscriber(DroneTurrets plugin, string[] hookNames);
        public void Add(T item);
        public void Remove(T item);
        public void SubscribeAll();
        public void UnsubscribeAll();
    }

    private class DroneController : ListComponent<DroneController>
    {
        public static void AddToDrone(Drone drone, AutoTurret turret, BasePlayer player);
        public static void DestroyAll();
        private Drone _drone;
        private AutoTurret _turret;
        private BasePlayer _controller;
        private CameraViewerId _viewerId;
        private void Update();
        private void OnDestroy();
    }

    private class Configuration : BaseConfiguration
    {
        [JsonProperty("RequirePermissionToControlDroneTurrets")]
        public bool RequirePermission;
        [JsonProperty("TargetPlayers")]
        public bool TargetPlayers;
        [JsonProperty("TargetNPCs")]
        public bool TargetNPCs;
        [JsonProperty("TargetAnimals")]
        public bool TargetAnimals;
        [JsonProperty("EnableAudioAlarm")]
        public bool EnableAudioAlarm;
        [JsonProperty("EnableSirenLight")]
        public bool EnableSirenLight;
        [JsonProperty("TurretRange")]
        public float TurretRange;
        [JsonProperty("TipChance")]
        public int TipChance;
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
    private void ReplyToPlayer(IPlayer player, string messageName, object[] args);
    private void ChatMessage(BasePlayer player, string messageName, object[] args);
    private string GetMessage(IPlayer player, string messageName, object[] args);
    private string GetMessage(BasePlayer player, string messageName, object[] args);
    private string GetMessage(string playerId, string messageName, object[] args);
    private class Lang
    {
        public const string TipDeployCommand;
        public const string ErrorNoPermission;
        public const string ErrorNoDroneFound;
        public const string ErrorBuildingBlocked;
        public const string ErrorNoTurretItem;
        public const string ErrorAlreadyHasTurret;
        public const string ErrorIncompatibleAttachment;
        public const string ErrorDeployFailed;
        public const string ErrorCannotPickupWithTurret;
    }

    protected override void LoadDefaultMessages();
}

private static class ExposedHooks
{
    public static void OnBookmarkControlStarted(ComputerStation station, BasePlayer player, string name, IRemoteControllable controllable);
    public static void OnBookmarkControlEnded(ComputerStation station, BasePlayer player, IRemoteControllable controllable);
    public static void OnEntityBuilt(BaseEntity heldEntity, GameObject gameObject);
    public static object OnDroneTurretDeploy(Drone drone, BasePlayer deployer);
    public static void OnDroneTurretDeployed(Drone drone, AutoTurret turret, BasePlayer deployer);
    public static object OnDroneNpcTurretDeploy(Drone drone, BasePlayer deployer);
    public static void OnDroneNpcTurretDeployed(Drone drone, NPCAutoTurret turret, BasePlayer deployer);
}

private static class RCUtils
{
    public static bool IsRCDrone(Drone drone);
    public static bool HasController(IRemoteControllable controllable);
    public static bool HasController(IRemoteControllable controllable, BasePlayer player);
    public static bool HasFakeController(IRemoteControllable controllable);
    public static bool HasRealController(IRemoteControllable controllable);
    public static bool CanControl(BasePlayer player, IRemoteControllable controllable);
    public static void RemoveController(IRemoteControllable controllable);
    public static bool AddViewer(IRemoteControllable controllable, BasePlayer player);
    public static void RemoveViewer(IRemoteControllable controllable, BasePlayer player);
    public static bool AddFakeViewer(IRemoteControllable controllable);
    public static T GetControlledEntity(BasePlayer player, ComputerStation station);
    public static T GetControlledEntity(BasePlayer player);
}

private class DynamicHookSubscriber
{
    private DroneTurrets _plugin;
    private HashSet<T> _list;
    private string[] _hookNames;
    public DynamicHookSubscriber(DroneTurrets plugin, string[] hookNames);
    public void Add(T item);
    public void Remove(T item);
    public void SubscribeAll();
    public void UnsubscribeAll();
}

private class DroneController : ListComponent<DroneController>
{
    public static void AddToDrone(Drone drone, AutoTurret turret, BasePlayer player);
    public static void DestroyAll();
    private Drone _drone;
    private AutoTurret _turret;
    private BasePlayer _controller;
    private CameraViewerId _viewerId;
    private void Update();
    private void OnDestroy();
}

private class Configuration : BaseConfiguration
{
    [JsonProperty("RequirePermissionToControlDroneTurrets")]
    public bool RequirePermission;
    [JsonProperty("TargetPlayers")]
    public bool TargetPlayers;
    [JsonProperty("TargetNPCs")]
    public bool TargetNPCs;
    [JsonProperty("TargetAnimals")]
    public bool TargetAnimals;
    [JsonProperty("EnableAudioAlarm")]
    public bool EnableAudioAlarm;
    [JsonProperty("EnableSirenLight")]
    public bool EnableSirenLight;
    [JsonProperty("TurretRange")]
    public float TurretRange;
    [JsonProperty("TipChance")]
    public int TipChance;
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
    public const string TipDeployCommand;
    public const string ErrorNoPermission;
    public const string ErrorNoDroneFound;
    public const string ErrorBuildingBlocked;
    public const string ErrorNoTurretItem;
    public const string ErrorAlreadyHasTurret;
    public const string ErrorIncompatibleAttachment;
    public const string ErrorDeployFailed;
    public const string ErrorCannotPickupWithTurret;
}


```

---

## DropRemover by Ryan - Removes selected world items from the map with commands

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Drop Remover", "Ryan", "1.0.21")]
[Description("Removes world items from the map with no effect on server performance")]
public class DropRemover : RustPlugin
{
    private bool ConfigChanged;
    private List<string> ItemBlacklist;
    protected override void LoadDefaultConfig();
    private void InitConfig();
    private T GetConfig(T defaultVal, string[] path);
    private new void LoadDefaultMessages();
    private IEnumerator Delete(IEnumerable<BaseNetworkable> list, string userId, ItemCategory? category, Item item);
    private void Init();
    [ConsoleCommand("drops.remove")]
    private void DropRemoveCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("drops.itemremove")]
    private void ItemRemove(ConsoleSystem.Arg arg);
    [ConsoleCommand("drops.categoryremove")]
    private void CategoryRemove(ConsoleSystem.Arg arg);
}


```

---

## Duelist by nivex - 1v1 and team deathmatch event

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Text.RegularExpressions;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Oxide.Game.Rust.Libraries;
using Oxide.Plugins.DuelistExtensionMethods;
using Rust;
using Rust.Workshop;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Duelist", "nivex", "1.3.5")]
[Description("1v1 and team deathmatch event.")]
public class Duelist : RustPlugin
{
    [PluginReference]
     Plugin Kits;
     Plugin ZoneManager;
     Plugin Economics;
     Plugin ServerRewards;
     Plugin Clans;
     Plugin AimTrain;
     Plugin LustyMap;
    private static Duelist Instance;
    private const string hewwPrefab;
    private const string heswPrefab;
    private const bool debugMode;
    private List<string> readyUiList;
    private List<string> spectators;
    private List<Rematch> rematches;
    private Dictionary<string, AttackerInfo> tdmAttackers;
    private Dictionary<string, string> tdmKits;
    private HashSet<GoodVersusEvilMatch> tdmMatches;
    private List<DuelingZone> duelingZones;
    private StoredData duelsData;
    private Dictionary<string, string> dataDuelists;
    private Dictionary<string, long> dataImmunity;
    private Dictionary<string, Vector3> dataImmunitySpawns;
    private int blockedMask;
    private int constructionMask;
    private bool matchUpdateRequired;
    private int groundMask;
    private int wallMask;
    private int waterMask;
    private int worldMask;
    private Timer announceTimer;
    private SortedDictionary<string, string> boneTags;
    private Dictionary<string, string> announcements;
    private Dictionary<string, long> dataDeath;
    private Dictionary<string, string> dataRequests;
    private Dictionary<string, bool> deployables;
    private Dictionary<ulong, List<BaseEntity>> duelEntities;
    private DynamicConfigFile duelsFile;
    private Timer eventTimer;
    private Timer matchTimer;
    private SpawnFilter filter;
    private Dictionary<Vector3, float> managedZones;
    private List<Vector3> monuments;
    private Dictionary<string, string> prefabs;
    private bool resetDuelists;
    private Dictionary<string, List<ulong>> skinsCache;
    private Dictionary<string, string> tdmRequests;
    private Dictionary<string, List<ulong>> workshopskinsCache;
    private List<string> dcsBlock;
    private Dictionary<string, string> playerZones;
    public class StoredData
    {
        public List<string> Allowed;
        public Dictionary<string, List<string>> AutoGeneratedSpawns;
        public Dictionary<string, string> Bans;
        public Dictionary<string, BetInfo> Bets;
        public Dictionary<string, List<string>> BlockedUsers;
        public List<string> Chat;
        public List<string> ChatEx;
        public Dictionary<string, List<BetInfo>> ClaimBets;
        public Dictionary<string, string> CustomKits;
        public bool DuelsEnabled;
        public Dictionary<string, string> Homes;
        public Dictionary<string, string> Kits;
        public Dictionary<string, int> Losses;
        public Dictionary<string, int> LossesSeed;
        public SortedDictionary<long, string> Queued;
        public List<string> Restricted;
        public List<string> Spawns;
        public List<string> AutoReady;
        public Dictionary<string, int> MatchVictories;
        public Dictionary<string, int> MatchVictoriesSeed;
        public Dictionary<string, int> MatchLosses;
        public Dictionary<string, int> MatchLossesSeed;
        public Dictionary<string, int> MatchDeaths;
        public Dictionary<string, int> MatchDeathsSeed;
        public Dictionary<string, int> MatchKills;
        public Dictionary<string, int> MatchKillsSeed;
        public Dictionary<string, Dictionary<string, int>> MatchSizesVictories;
        public Dictionary<string, Dictionary<string, int>> MatchSizesVictoriesSeed;
        public Dictionary<string, Dictionary<string, int>> MatchSizesLosses;
        public Dictionary<string, Dictionary<string, int>> MatchSizesLossesSeed;
        public int TotalDuels;
        public Dictionary<string, int> Victories;
        public Dictionary<string, int> VictoriesSeed;
        public List<string> ZoneIds;
        public Dictionary<string, string> DuelZones;
    }

    private class Tracker : FacepunchBehaviour
    {
        public BasePlayer player;
        private Duelist _;
        private void Awake();
        public void Init(Duelist _);
        private void Track();
        private void OnDestroy();
    }

    public class Rematch
    {
        public Rematch(Duelist _);
        private Duelist _;
        public List<BasePlayer> Duelists;
        public List<BasePlayer> Ready;
        private List<BasePlayer> Evil;
        private List<BasePlayer> Good;
        public GoodVersusEvilMatch match;
        private Timer _notify;
        public List<BasePlayer> Players { get; set; }
        public bool HasPlayer(BasePlayer player);
        public bool AddRange(List<BasePlayer> players, Team team);
        public bool IsReady(BasePlayer player);
        public bool IsReady();
        private void Reset(string key);
        public void MessageAll(string key, object[] args);
        public void Notify();
        private void Cancel();
        public void Start();
        private bool AddMatchPlayers(List<BasePlayer> players, Team team);
    }

    public class AttackerInfo
    {
        public string AttackerName;
        public string AttackerId;
        public string BoneName;
        public string Distance;
        public string Weapon;
    }

    public class GoodVersusEvilMatch
    {
        public GoodVersusEvilMatch(Duelist _);
        private Duelist _;
        private HashSet<ulong> _banned;
        private HashSet<BasePlayer> _evil;
        private HashSet<ulong> _evilKIA;
        private List<BasePlayer> _evilRematch;
        private HashSet<BasePlayer> _good;
        private HashSet<ulong> _goodKIA;
        private List<BasePlayer> _goodRematch;
        private string _goodHostName;
        private string _evilHostName;
        private string _goodHostId;
        private string _evilHostId;
        private string _goodCode;
        private string _evilCode;
        private int _teamSize;
        private bool _started;
        private bool _ended;
        private string _kit;
        private DuelingZone _zone;
        private Timer _queueTimer;
        private bool _enteredQueue;
        private bool _public;
        public bool CanRematch;
        public string Id { get; set; }
        public string Versus { get; set; }
        public bool IsPublic { get; set; }
        public int TeamSize { get; set; }
        public DuelingZone Zone { get; set; }
        public bool EitherEmpty { get; set; }
        public bool IsStarted { get; set; }
        public bool IsOver { get; set; }
        public string Kit { get; set; }
        public void Reuse();
        public void Setup(BasePlayer player, BasePlayer target);
        public bool IsFull();
        public bool IsFull(Team team);
        public void MessageAll(string key, object[] args);
        public Team GetTeam(BasePlayer player);
        public bool IsHost(BasePlayer player);
        public void SetCode(BasePlayer player, string code);
        public string Code(Team team);
        public bool AlliedTo(BasePlayer player, Team team);
        public bool IsBanned(ulong targetId);
        public bool Ban(BasePlayer target);
        public bool Equals(GoodVersusEvilMatch match);
        public string GetNames(Team team);
        public void GiveShirt(BasePlayer player);
        public bool AddMatchPlayer(BasePlayer player, Team team);
        public bool RemoveMatchPlayer(BasePlayer player);
        private void AssignGoodHostId();
        private void AssignEvilHostId();
        private void Finalize(Team team);
        private bool SetupRematch();
        private void EndMatch(Team team);
        public void End(bool forced);
        private void Queue();
        public DuelingZone LastZone { get; set; }
        private void Start();
        private void Spawn(HashSet<BasePlayer> players, Vector3 spawn);
    }

    public class DuelKitItem
    {
        public string ammo;
        public int amount;
        public string container;
        public List<string> mods;
        public string shortname;
        public ulong skin;
        public int slot;
    }

    public class BetInfo
    {
        public string trigger;
        public int amount;
        public int itemid;
        public int max;
        public bool Equals(BetInfo bet);
    }

    public class DuelingZone : FacepunchBehaviour
    {
        private Duelist _ { get; set; }
        private HashSet<BasePlayer> _players;
        private HashSet<BasePlayer> _waiting;
        private Vector3 _zonePos;
        private List<Vector3> _duelSpawns;
        private List<SphereEntity> spheres;
        public bool IsLocked;
        public int Kills;
        public int TotalPlayers { get; set; }
        public List<BasePlayer> Players { get; set; }
        public List<Vector3> Spawns { get; set; }
        public bool IsFull { get; set; }
        public Vector3 Position { get; set; }
        private void OnDestroy();
        private void OnTriggerEnter(Collider col);
        public Vector3 GetEjectLocation(Vector3 a, float distance);
        public bool RemovePlayer(BasePlayer player);
        public void DismountAllPlayers(BaseMountable m);
        private List<BasePlayer> GetMountedPlayers(BaseMountable m);
        private List<BasePlayer> GetMountedPlayers(BaseVehicle vehicle);
        private bool RemoveMountable(BaseMountable m, List<BasePlayer> players);
        private bool IsFlying(BasePlayer player);
        private bool EjectMountable(BaseMountable m, float distance, List<BasePlayer> players);
        private int targetLayer;
        private static float GetSpawnHeight(Vector3 target);
        public void Setup(Vector3 position);
        private void RemoveSpheres();
        private void CreateSpheres();
        public float Distance(Vector3 position);
        public bool? AddWaiting(BasePlayer player, BasePlayer target);
        public bool IsWaiting(BasePlayer player);
        public void AddPlayer(BasePlayer player);
        public void RemovePlayer(string playerId);
        public bool HasPlayer(string playerId);
        public void Kill();
    }

    private object OnDangerousOpen(Vector3 treasurePos);
    private object OnPlayerDeathMessage(BasePlayer victim, HitInfo info);
    private void Init();
    private void OnServerInitialized();
    private void OnServerSave();
    private void OnNewSave(string filename);
    public void SaveData();
    private void DestroyAll();
    private void Unload();
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnPlayerConnected(BasePlayer player);
    private List<ulong> executes;
    private void OnPlayerSleepEnded(BasePlayer player);
    private bool IsExploiting(BasePlayer player, bool duel);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnEntityKill(SimpleBuildingBlock e);
    public void SetupPower(BaseEntity e);
    public void Track(BasePlayer player, bool enable);
    public void RecreateZoneWall(string prefab, Vector3 pos, Quaternion rot, ulong ownerId);
    public BaseEntity CreateZoneWall(string prefab, Vector3 pos, Quaternion rot, ulong ownerId);
    private void OnEntityDeath(BaseEntity entity, HitInfo hitInfo);
    private void OnPlayerHealthChange(BasePlayer player, float oldValue, float newValue);
    private void DefeatMessage(BasePlayer victim, GoodVersusEvilMatch match);
    private void OnDuelistLost(BasePlayer victim, bool sendHome);
    public string GetZoneName();
    public void SendDuelistsHome();
    public void SendSpectatorsHome();
    private void StartSpectate(BasePlayer player);
    private void EndSpectate(BasePlayer player);
    private static bool IsNull(BaseNetworkable a);
    private static bool IsNotConnected(BasePlayer a);
    public void HealDamage(BaseCombatEntity entity);
    public void CancelDamage(HitInfo hitInfo);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    private object OnRestoreUponDeath(BasePlayer player);
     class Explosives : FacepunchBehaviour
    {
         WorldItem worldItem;
         float minExplosionRadius;
         float explosionRadius;
         int layers;
         void Awake();
         void OnDestroy();
    }

     void OnEntitySpawned(WorldItem worldItem);
    private void OnEntitySpawned(BaseEntity entity);
     object CanBuild(Planner planner, Construction prefab, Construction.Target target);
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private object OnCreateWorldProjectile(HitInfo info, Item item);
    private void OnItemDropped(Item item, BaseEntity entity);
    private object IsPrisoner(BasePlayer player);
    private object CanEventJoin(BasePlayer player);
    private object canRemove(BasePlayer player);
    private object CanTrade(BasePlayer player);
    private object CanBank(BasePlayer player);
    private object CanOpenBackpack(BasePlayer player);
    private object canShop(BasePlayer player);
    private object CanShop(BasePlayer player);
    private object CanBePenalized(BasePlayer player);
    private object canTeleport(BasePlayer player);
    private object CanTeleport(BasePlayer player);
    private object CanJoinTDMEvent(BasePlayer player);
    private object CanEntityTakeDamage(BaseEntity entity, HitInfo hitinfo);
    private object CanJoinAimTrain(BasePlayer player);
    private bool InQueue(string userid);
     object OnPlayerCommand(BasePlayer player, string command, string[] args);
     object OnServerCommand(ConsoleSystem.Arg arg);
    public void CheckAutoReady(BasePlayer player);
    public void ToggleAutoReady(BasePlayer player);
    public void ReadyUp(BasePlayer player);
    public void cmdTDM(BasePlayer player, string command, string[] args);
    public void cmdQueue(BasePlayer player, string command, string[] args);
    private void cmdLadder(BasePlayer player, string command, string[] args);
    private void ccmdDuel(ConsoleSystem.Arg arg);
    private void CommandDuelist(IPlayer player, string command, string[] args);
    private void cmdDuel(BasePlayer player, string command, string[] args);
    public DuelingZone GetPlayerZone(BasePlayer player, int size);
    private List<ulong> _times;
    public void SetPlayerTime(BasePlayer player, bool set);
    public bool SetPlayerZone(BasePlayer player, string[] args);
    public void SetupTeams(BasePlayer player, BasePlayer target);
    private void ResetTemporaryData();
    public DuelingZone RemoveDuelist(string playerId);
    public void ResetDuelist(string targetId, bool removeHome);
    private void RemoveZeroStats();
    public void SetupZoneManager();
    public void SetupZones();
    public Vector3 SetupDuelZone(List<BaseEntity> entities, string zoneName);
    public DuelingZone SetupDuelZone(Vector3 zonePos, List<BaseEntity> entities, string zoneName);
    public List<BaseEntity> GetWallEntities();
    public int RemoveZoneWalls(ulong ownerId);
    public bool ZoneWallsExist(ulong ownerId, List<BaseEntity> entities);
    public void CreateZoneWalls(Vector3 center, float zoneRadius, string prefab, List<BaseEntity> entities, BasePlayer player);
    public void EjectPlayers(DuelingZone zone);
    public void EjectPlayer(BasePlayer player);
    public void RemoveDuelZone(DuelingZone zone);
    public void RemoveEntities(ulong playerId);
    public void RemoveEntities(DuelingZone zone);
    public DuelingZone GetDuelZone(Vector3 startPos, float offset);
    public void SendHome(BasePlayer player);
    public void GiveRespawnLoot(BasePlayer player);
    private void UpdateStability();
    private void CheckZoneHooks(bool message);
    private void CheckDuelistMortality();
    public void SubscribeHooks(bool flag);
    [HookMethod("DuelistTerritory")]
    public bool DuelistTerritory(Vector3 position);
    public bool DuelTerritory(Vector3 position, float offset);
    public ulong GetOwnerId(string uid);
    [HookMethod("inEvent")]
    public bool inEvent(BasePlayer player);
    public bool InEvent(BasePlayer player);
    public bool IsDueling(BasePlayer player);
    public bool InDeathmatch(BasePlayer player);
    public bool IsSpectator(BasePlayer player);
    public bool IsEventBanned(string targetId);
    public static long TimeStamp();
    public string GetDisplayName(string targetId);
    public void Log(string file, string message, bool timestamp);
    public GoodVersusEvilMatch GetMatch(BasePlayer player);
    public bool InMatch(BasePlayer target);
    public bool IsOnConstruction(Vector3 position);
    public bool Teleport(BasePlayer player, Vector3 destination);
    public bool IsThrownWeapon(Item item);
    public Vector3 RandomDropPosition();
    public Vector3 FindDuelingZone();
    public List<Vector3> GetCircumferencePositions(Vector3 center, float radius, float next, float y);
    public List<Vector3> GetAutoSpawns(DuelingZone zone);
    public List<Vector3> CreateSpawnPoints(Vector3 center);
    public bool ResetDuelists();
    public bool AssignDuelists();
    public bool IsNewman(BasePlayer player);
    public int GetAmount(BasePlayer player, string shortname);
    public bool RemoveFromQueue(string targetId);
    public void CheckQueue();
    public bool SelectZone(BasePlayer player, BasePlayer target);
    public string GetKit(BasePlayer player, BasePlayer target);
    public void VerifyKits();
    public string GetRandomKit();
    public void Initiate(BasePlayer player, BasePlayer target, bool checkInventory, DuelingZone destZone);
    public bool IsAllied(string playerId, string targetId);
    public bool IsAllied(BasePlayer player, BasePlayer target);
    public bool IsOnSameTeam(BasePlayer player, BasePlayer target);
    public bool IsInSameClan(BasePlayer player, BasePlayer target);
    private bool IsAuthorizing(BasePlayer player, BasePlayer target);
    private bool IsBunked(BasePlayer player, BasePlayer target);
    private bool IsCodeAuthed(BasePlayer player, BasePlayer target);
    private bool IsInSameBase(BasePlayer player, BasePlayer target);
    public void Metabolize(BasePlayer player, bool set);
    public bool IsKit(string kit);
    public void AwardPlayer(ulong playerId, double money, int points);
    public void GivePlayerKit(BasePlayer player);
    public bool GiveCustomKit(BasePlayer player, string kit);
    private void DuelAnnouncement(bool bypass);
    public bool CreateBet(BasePlayer player, int betAmount, BetInfo betInfo);
    private void GetWorkshopIDs(int code, string response);
    public List<ulong> GetItemSkins(ItemDefinition def);
    private void RemoveRequests(BasePlayer player);
    private void UpdateMatchSizeStats(string playerId, bool winner, bool loser, int teamSize);
    private void UpdateMatchStats(string playerId, bool winner, bool loser, bool death, bool kill);
    public void SendSpawnHelp(BasePlayer player);
    public void AddSpawnPoint(BasePlayer player, bool useHit);
    public void RemoveSpawnPoint(BasePlayer player);
    public void RemoveSpawnPoints(BasePlayer player);
    public void WipeSpawnPoints(BasePlayer player);
    public List<Vector3> GetSpawnPoints(DuelingZone zone);
    public string FormatBone(string source);
    public string FormatPosition(Vector3 position);
    private readonly List<string> createUI;
    private readonly List<string> duelistUI;
    private readonly List<string> kitsUI;
    private readonly List<string> matchesUI;
    [ConsoleCommand("UI_DuelistCommand")]
    private void ccmdDuelistUI(ConsoleSystem.Arg arg);
    public void DestroyAllUI();
    public bool DestroyUI(BasePlayer player);
    public void ccmdDUI(ConsoleSystem.Arg arg);
    public void cmdDUI(BasePlayer player, string command, string[] args);
    public void RefreshUI(BasePlayer player);
    public void ToggleMatchUI(BasePlayer player);
    public void ToggleKitUI(BasePlayer player);
    public void CreateAnnouncementUI(BasePlayer player, string text);
    public void CreateCountdownUI(BasePlayer player, string text);
    public void ToggleReadyUI(BasePlayer player);
    public void CreateDefeatUI(BasePlayer player);
    private void UpdateMatchUI();
    public class UI
    {
        public static CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor, string parent);
        public static void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        public static void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, string labelColor);
        public static float[] CalcButtonPos(int number, float dMinOffset);
    }

    private bool Changed;
    private string szMatchChatCommand;
    private string szDuelChatCommand;
    private string szQueueChatCommand;
    private const string duelistPerm;
    private const string duelistGroup;
    private float zoneRadius;
    private int deathTime;
    private int immunityTime;
    private int zoneCounter;
    private List<string> _hpDuelingKits;
    private List<string> _lpDuelingKits;
    private List<string> hpDuelingKits;
    private List<string> lpDuelingKits;
    private List<BetInfo> duelingBets;
    private bool recordStats;
    private int permsToGive;
    private float maxIncline;
    private bool allowBetForfeit;
    private bool allowBetRefund;
    private bool allowBets;
    private bool putToSleep;
    private bool blockSpawning;
    private bool killNpc;
    private float announceTime;
    private bool removePlayers;
    private bool useAnnouncement;
    private bool autoSetup;
    private bool broadcastDefeat;
    private double economicsMoney;
    private double requiredDuelMoney;
    private int serverRewardsPoints;
    private float damageScaleAmount;
    private int zoneAmount;
    private int playersPerZone;
    private bool visibleToAdmins;
    private float spDrawTime;
    private float spRemoveOneMaxDistance;
    private float spRemoveAllMaxDistance;
    private bool spAutoRemove;
    private bool avoidWaterSpawns;
    private int extraWallStacks;
    private bool useZoneWalls;
    private bool zoneUseWoodenWalls;
    private float buildingBlockExtensionRadius;
    private bool autoAllowAll;
    private bool useRandomSkins;
    private float playerHealth;
    private bool dmFF;
    private int minDeathmatchSize;
    private int maxDeathmatchSize;
    private bool autoEnable;
    private ulong teamGoodShirt;
    private ulong teamEvilShirt;
    private string teamShirt;
    private double teamEconomicsMoney;
    private int teamServerRewardsPoints;
    private float lesserKitChance;
    private bool tdmEnabled;
    private bool useLeastAmount;
    private bool tdmServerDeaths;
    private bool tdmMatchDeaths;
    private List<string> whitelistCommands;
    private bool useWhitelistCommands;
    private List<string> blacklistCommands;
    private bool useBlacklistCommands;
    private bool bypassNewmans;
    private bool saveRestoreEnabled;
    private List<DuelKitItem> respawnLoot;
    private bool respawnDeadDisconnect;
    private bool sendDeadHome;
    private bool resetSeed;
    private bool noStability;
    private bool noMovement;
    private bool requireTeamSize;
    private int requiredMinSpawns;
    private int requiredMaxSpawns;
    private bool guiAutoEnable;
    private bool guiUseCursor;
    private string szUIChatCommand;
    private bool useWorkshopSkins;
    private bool respawnWalls;
    private bool allowPlayerDeaths;
    private bool morphBarricadesStoneWalls;
    private bool morphBarricadesWoodenWalls;
    private bool guiUseCloseButton;
    private string autoKitName;
    private float guiAnnounceUITime;
    private bool sendDefeatedHome;
    private bool sendHomeRequeue;
    private bool sendHomeSpectatorWhenRematchTimesOut;
    private bool autoFlames;
    private bool autoOvens;
    private bool autoTurrets;
    private int sphereAmount;
    private bool wipeDuelZones;
    private bool setPlayerTime;
    private ulong chatSteamID;
    private List<object> RespawnLoot { get; set; }
    private List<object> BlacklistedCommands { get; set; }
    private List<object> WhitelistedCommands { get; set; }
    private List<object> DefaultBets { get; set; }
    private List<object> DefaultLesserKits { get; set; }
    private List<object> DefaultKits { get; set; }
    private static Dictionary<string, List<DuelKitItem>> customKits;
    private Dictionary<string, object> DefaultCustomKits { get; set; }
    protected override void LoadDefaultMessages();
    public List<string> VerifiedKits { get; set; }
    public string GetVerifiedKit(string kit);
    private void LoadVariables();
    private bool canSaveConfig;
    protected override void SaveConfig();
    private void LoadAnimalSettings();
    private void LoadNormalSettings();
    private void LoadDeviceSettings();
    private void LoadBetSettings();
    private void LoadZoneSettings();
    private void LoadDeployableSettings();
    private void LoadRankedSettings();
    private void LoadRespawnSettings();
    private void LoadRewardSettings();
    private void LoadSpawnSettings();
    private void LoadDeathmatchSettings();
    private void LoadAdvancedSettings();
    private void LoadUserInterfaceSettings();
    private void LoadSpectatorSettings();
    private void RegisterCommands();
    private void SetupBets();
    private void LoadKitSettings();
    private void EnsureLimits();
    private void SetupDefinitions();
    private void SetupRespawnItems(List<object> list, List<DuelKitItem> source);
    private void SetupCustomKits(Dictionary<string, object> dict, Dictionary<string, List<DuelKitItem>> source);
    protected override void LoadDefaultConfig();
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private Dictionary<string, string> hexColors;
    private string msg(string key, string id, object[] args);
    public string RemoveFormatting(string source);
    public static void Message(BasePlayer player, string message);
    public void Message(HashSet<BasePlayer> players, string key, object[] args);
}

public class StoredData
{
    public List<string> Allowed;
    public Dictionary<string, List<string>> AutoGeneratedSpawns;
    public Dictionary<string, string> Bans;
    public Dictionary<string, BetInfo> Bets;
    public Dictionary<string, List<string>> BlockedUsers;
    public List<string> Chat;
    public List<string> ChatEx;
    public Dictionary<string, List<BetInfo>> ClaimBets;
    public Dictionary<string, string> CustomKits;
    public bool DuelsEnabled;
    public Dictionary<string, string> Homes;
    public Dictionary<string, string> Kits;
    public Dictionary<string, int> Losses;
    public Dictionary<string, int> LossesSeed;
    public SortedDictionary<long, string> Queued;
    public List<string> Restricted;
    public List<string> Spawns;
    public List<string> AutoReady;
    public Dictionary<string, int> MatchVictories;
    public Dictionary<string, int> MatchVictoriesSeed;
    public Dictionary<string, int> MatchLosses;
    public Dictionary<string, int> MatchLossesSeed;
    public Dictionary<string, int> MatchDeaths;
    public Dictionary<string, int> MatchDeathsSeed;
    public Dictionary<string, int> MatchKills;
    public Dictionary<string, int> MatchKillsSeed;
    public Dictionary<string, Dictionary<string, int>> MatchSizesVictories;
    public Dictionary<string, Dictionary<string, int>> MatchSizesVictoriesSeed;
    public Dictionary<string, Dictionary<string, int>> MatchSizesLosses;
    public Dictionary<string, Dictionary<string, int>> MatchSizesLossesSeed;
    public int TotalDuels;
    public Dictionary<string, int> Victories;
    public Dictionary<string, int> VictoriesSeed;
    public List<string> ZoneIds;
    public Dictionary<string, string> DuelZones;
}

private class Tracker : FacepunchBehaviour
{
    public BasePlayer player;
    private Duelist _;
    private void Awake();
    public void Init(Duelist _);
    private void Track();
    private void OnDestroy();
}

public class Rematch
{
    public Rematch(Duelist _);
    private Duelist _;
    public List<BasePlayer> Duelists;
    public List<BasePlayer> Ready;
    private List<BasePlayer> Evil;
    private List<BasePlayer> Good;
    public GoodVersusEvilMatch match;
    private Timer _notify;
    public List<BasePlayer> Players { get; set; }
    public bool HasPlayer(BasePlayer player);
    public bool AddRange(List<BasePlayer> players, Team team);
    public bool IsReady(BasePlayer player);
    public bool IsReady();
    private void Reset(string key);
    public void MessageAll(string key, object[] args);
    public void Notify();
    private void Cancel();
    public void Start();
    private bool AddMatchPlayers(List<BasePlayer> players, Team team);
}

public class AttackerInfo
{
    public string AttackerName;
    public string AttackerId;
    public string BoneName;
    public string Distance;
    public string Weapon;
}

public class GoodVersusEvilMatch
{
    public GoodVersusEvilMatch(Duelist _);
    private Duelist _;
    private HashSet<ulong> _banned;
    private HashSet<BasePlayer> _evil;
    private HashSet<ulong> _evilKIA;
    private List<BasePlayer> _evilRematch;
    private HashSet<BasePlayer> _good;
    private HashSet<ulong> _goodKIA;
    private List<BasePlayer> _goodRematch;
    private string _goodHostName;
    private string _evilHostName;
    private string _goodHostId;
    private string _evilHostId;
    private string _goodCode;
    private string _evilCode;
    private int _teamSize;
    private bool _started;
    private bool _ended;
    private string _kit;
    private DuelingZone _zone;
    private Timer _queueTimer;
    private bool _enteredQueue;
    private bool _public;
    public bool CanRematch;
    public string Id { get; set; }
    public string Versus { get; set; }
    public bool IsPublic { get; set; }
    public int TeamSize { get; set; }
    public DuelingZone Zone { get; set; }
    public bool EitherEmpty { get; set; }
    public bool IsStarted { get; set; }
    public bool IsOver { get; set; }
    public string Kit { get; set; }
    public void Reuse();
    public void Setup(BasePlayer player, BasePlayer target);
    public bool IsFull();
    public bool IsFull(Team team);
    public void MessageAll(string key, object[] args);
    public Team GetTeam(BasePlayer player);
    public bool IsHost(BasePlayer player);
    public void SetCode(BasePlayer player, string code);
    public string Code(Team team);
    public bool AlliedTo(BasePlayer player, Team team);
    public bool IsBanned(ulong targetId);
    public bool Ban(BasePlayer target);
    public bool Equals(GoodVersusEvilMatch match);
    public string GetNames(Team team);
    public void GiveShirt(BasePlayer player);
    public bool AddMatchPlayer(BasePlayer player, Team team);
    public bool RemoveMatchPlayer(BasePlayer player);
    private void AssignGoodHostId();
    private void AssignEvilHostId();
    private void Finalize(Team team);
    private bool SetupRematch();
    private void EndMatch(Team team);
    public void End(bool forced);
    private void Queue();
    public DuelingZone LastZone { get; set; }
    private void Start();
    private void Spawn(HashSet<BasePlayer> players, Vector3 spawn);
}

public class DuelKitItem
{
    public string ammo;
    public int amount;
    public string container;
    public List<string> mods;
    public string shortname;
    public ulong skin;
    public int slot;
}

public class BetInfo
{
    public string trigger;
    public int amount;
    public int itemid;
    public int max;
    public bool Equals(BetInfo bet);
}

public class DuelingZone : FacepunchBehaviour
{
    private Duelist _ { get; set; }
    private HashSet<BasePlayer> _players;
    private HashSet<BasePlayer> _waiting;
    private Vector3 _zonePos;
    private List<Vector3> _duelSpawns;
    private List<SphereEntity> spheres;
    public bool IsLocked;
    public int Kills;
    public int TotalPlayers { get; set; }
    public List<BasePlayer> Players { get; set; }
    public List<Vector3> Spawns { get; set; }
    public bool IsFull { get; set; }
    public Vector3 Position { get; set; }
    private void OnDestroy();
    private void OnTriggerEnter(Collider col);
    public Vector3 GetEjectLocation(Vector3 a, float distance);
    public bool RemovePlayer(BasePlayer player);
    public void DismountAllPlayers(BaseMountable m);
    private List<BasePlayer> GetMountedPlayers(BaseMountable m);
    private List<BasePlayer> GetMountedPlayers(BaseVehicle vehicle);
    private bool RemoveMountable(BaseMountable m, List<BasePlayer> players);
    private bool IsFlying(BasePlayer player);
    private bool EjectMountable(BaseMountable m, float distance, List<BasePlayer> players);
    private int targetLayer;
    private static float GetSpawnHeight(Vector3 target);
    public void Setup(Vector3 position);
    private void RemoveSpheres();
    private void CreateSpheres();
    public float Distance(Vector3 position);
    public bool? AddWaiting(BasePlayer player, BasePlayer target);
    public bool IsWaiting(BasePlayer player);
    public void AddPlayer(BasePlayer player);
    public void RemovePlayer(string playerId);
    public bool HasPlayer(string playerId);
    public void Kill();
}

 class Explosives : FacepunchBehaviour
{
     WorldItem worldItem;
     float minExplosionRadius;
     float explosionRadius;
     int layers;
     void Awake();
     void OnDestroy();
}

public class UI
{
    public static CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor, string parent);
    public static void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    public static void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, string labelColor);
    public static float[] CalcButtonPos(int number, float dMinOffset);
}

Oxide.Plugins.DuelistExtensionMethods
public static class ExtensionMethods
{
    public static bool All(IEnumerable<T> a, Func<T, bool> b);
    public static T ElementAt(IEnumerable<T> a, int b);
    public static bool Exists(IEnumerable<T> a, Func<T, bool> b);
    public static T FirstOrDefault(IEnumerable<T> a, Func<T, bool> b);
    public static IEnumerable<T> Select(IList<Y> a, Func<Y, T> b);
    public static string[] Skip(string[] a, int b);
    public static List<T> Take(IList<T> a, int b);
    public static List<T> ToList(IEnumerable<T> a);
    public static IEnumerable<T> Where(IEnumerable<T> a, Func<T, bool> b);
    public static bool IsHuman(BasePlayer a);
    public static List<T> OfType(IEnumerable<BaseNetworkable> a);
    public static int Sum(IEnumerable<T> a, Func<T, int> b);
    public static bool IsKilled(BaseNetworkable a);
    public static void SafelyKill(BaseNetworkable a);
    public static bool CanCall(Plugin a);
}


```

---

## DungProtector by KajWithAJ - Prevent players from stealing horse dung.

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("Dung Protector", "KajWithAJ", "1.4.0")]
[Description("Prevent players from stealing horse dung.")]
 class DungProtector : RustPlugin
{
    private const string PermissionUse;
    private const string PermissionExclude;
    private void Init();
    protected override void LoadDefaultMessages();
     object OnItemPickup(Item item, BasePlayer player);
}


```

---

## DynamicConfig by FastBurst - Allows automatically update configs of other plugins, depending on time passed since last wipe

```csharp
using Oxide.Core;
using System.Collections.Generic;
using System;
using System.Text.RegularExpressions;
using System.IO;
using System.Linq;
using System.Text;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("Dynamic Config", "FastBurst", "1.0.5")]
[Description("Dynamically changes configs of other plugins depending on time from last wipe")]
 class DynamicConfig : RustPlugin
{
    private static DynamicConfig Instance;
    private ConfigLibrary configLibrary;
    private Timer updateTimer;
    private DynamicConfigFile configHandler;
    private DateTime WipeDate;
    protected override void LoadDefaultMessages();
    private static string _(string message, object[] args);
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private static string DataPath { get; set; }
    private static DataFileSystem DFS { get; set; }
     class ConfigLibrary
    {
         Dictionary<string, PluginInfo> pluginInfos;
        public ConfigLibrary();
        public void Update();
    }

     class PluginInfo
    {
        private string pluginName;
        private List<ConfigInfo> configs;
        public PluginInfo(string pluginName);
        public void AddConfig(string filename, DateTime applyTime);
        public void Update();
    }

     class ConfigInfo
    {
        public string filename;
        public DateTime applyTime;
        public bool applied;
        public ConfigInfo(string filename, DateTime applyTime);
        public void Load(string pluginName);
    }

    private static bool TryParseTimeSpan(string source, TimeSpan timeSpan);
    private static string TimeSpanToString(TimeSpan timeSpan);
    private static string GetConfigPath(string pluginName);
}

 class ConfigLibrary
{
     Dictionary<string, PluginInfo> pluginInfos;
    public ConfigLibrary();
    public void Update();
}

 class PluginInfo
{
    private string pluginName;
    private List<ConfigInfo> configs;
    public PluginInfo(string pluginName);
    public void AddConfig(string filename, DateTime applyTime);
    public void Update();
}

 class ConfigInfo
{
    public string filename;
    public DateTime applyTime;
    public bool applied;
    public ConfigInfo(string filename, DateTime applyTime);
    public void Load(string pluginName);
}


```

---

## DynamicLootDrops by birthdates - Drops loot on ground on death and allows players to auto pick up

```csharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Dynamic Loot Drops", "birthdates", "1.1.1")]
[Description("Adding a new way of looting.")]
public class DynamicLootDrops : RustPlugin
{
    private List<Item> pickUp;
    private ConfigFile config;
    private Dictionary<BasePlayer, long> cooldowns;
    private const string Permission;
     void OnPlayerDeath(BasePlayer player, HitInfo info);
     bool CanTake(BasePlayer player);
     void OnPlayerInput(BasePlayer player, InputState input);
    public class ConfigFile
    {
        [JsonProperty(PropertyName = "Blacklisted auto-pickups")]
        public List<string> bAP;
        [JsonProperty(PropertyName = "Pick up without death")]
        public bool pickUpWithoutDeath;
        [JsonProperty(PropertyName = "Cooldown in milliseconds")]
        public long cooldown;
        public static ConfigFile DefaultConfig();
    }

    protected override void SaveConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
     void Init();
}

public class ConfigFile
{
    [JsonProperty(PropertyName = "Blacklisted auto-pickups")]
    public List<string> bAP;
    [JsonProperty(PropertyName = "Pick up without death")]
    public bool pickUpWithoutDeath;
    [JsonProperty(PropertyName = "Cooldown in milliseconds")]
    public long cooldown;
    public static ConfigFile DefaultConfig();
}


```

---

## DynamicPlayerLimit by Pho3niX90 - Increases the player limit by demand

```csharp
using ConVar;
using Newtonsoft.Json;
using System;

Oxide.Plugins
[Info("Dynamic Player Limit", "Pho3niX90", "0.0.7")]
[Description("Increases the player limit by demand")]
 class DynamicPlayerLimit : RustPlugin
{
     int _originalLimit;
     Timer updateTimer;
     void OnServerInitialized(bool serverInitialized);
     void Unload();
    private void AdjustPlayers();
    private void UpdatePlayerLimit(int limit);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Starting Player Slots")]
        public int startPlayerSlots;
        [JsonProperty(PropertyName = "Maximum Player Slots")]
        public int maxPlayerSlots;
        [JsonProperty(PropertyName = "Increment Player Slots")]
        public int incrementPlayerSlots;
        [JsonProperty(PropertyName = "Increment Interval Minutes")]
        public int incrementInterval;
        [JsonProperty(PropertyName = "Increment When Slots Available")]
        public int incrementSlotsOpen;
        [JsonProperty(PropertyName = "Only increment when FPS above")]
        public int fpsLimit;
        [JsonProperty(PropertyName = "Auto decrease again")]
        public bool doAutoDecrease;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Starting Player Slots")]
    public int startPlayerSlots;
    [JsonProperty(PropertyName = "Maximum Player Slots")]
    public int maxPlayerSlots;
    [JsonProperty(PropertyName = "Increment Player Slots")]
    public int incrementPlayerSlots;
    [JsonProperty(PropertyName = "Increment Interval Minutes")]
    public int incrementInterval;
    [JsonProperty(PropertyName = "Increment When Slots Available")]
    public int incrementSlotsOpen;
    [JsonProperty(PropertyName = "Only increment when FPS above")]
    public int fpsLimit;
    [JsonProperty(PropertyName = "Auto decrease again")]
    public bool doAutoDecrease;
}


```

---

## DynamicPopulation by Aspectdev - Automatically manage your server's maximum player limit with in-depth configurable options.

```csharp
using Connection = Network.Connection;
using Oxide.Core.Libraries.Covalence;
using Exception = System.Exception;
using System.Collections.Generic;
using Newtonsoft.Json;
using ConVar;

Oxide.Plugins
[Info("Dynamic Population", "Aspectdev", "2.1.6")]
[Description("Automatically manage server MaxPlayers with customization options.")]
 class DynamicPopulation : CovalencePlugin
{
    public class PluginToggles
    {
        public bool Enable_Plugin;
        public bool Enable_Queueing;
        public bool OverageIsQueue;
        public bool FPS_Limiting;
        public bool Enable_Logs;
    }

    public class BaseServerVariables
    {
        public int Server_MinPlayers;
        public int Server_MaxPlayers;
        public int Average_FPS_Limit;
    }

    public class DecreasePopOptions
    {
        public int Decrease_Pop_Threshold;
        public int Pop_Decrease_Amount;
    }

    public class QueuedEnabledOptions
    {
        public int Queue_Increase_Threshold;
        public int Pop_Increase_Amount;
    }

    public class QueueingDisabledOptions
    {
        public int Increase_Pop_Threshold;
        public int Pop_Increase_Amount;
    }

    private static Configuration _config;
    private class Configuration
    {
        public PluginToggles PluginToggles;
        public BaseServerVariables BaseServerVariables;
        public DecreasePopOptions DecreasePopOptions;
        public QueuedEnabledOptions QueuedEnabledOptions;
        public QueueingDisabledOptions QueueingDisabledOptions;
    }

    private void Init();
     void OnPlayerConnected(BasePlayer player);
     void CanClientLogin(Connection connection);
     void OnPlayerDisconnected(BasePlayer player, string reason);
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void UpdatePop();
    private void SendUpdateRequest(int oldMaxPop, int newMaxPop);
}

public class PluginToggles
{
    public bool Enable_Plugin;
    public bool Enable_Queueing;
    public bool OverageIsQueue;
    public bool FPS_Limiting;
    public bool Enable_Logs;
}

public class BaseServerVariables
{
    public int Server_MinPlayers;
    public int Server_MaxPlayers;
    public int Average_FPS_Limit;
}

public class DecreasePopOptions
{
    public int Decrease_Pop_Threshold;
    public int Pop_Decrease_Amount;
}

public class QueuedEnabledOptions
{
    public int Queue_Increase_Threshold;
    public int Pop_Increase_Amount;
}

public class QueueingDisabledOptions
{
    public int Increase_Pop_Threshold;
    public int Pop_Increase_Amount;
}

private class Configuration
{
    public PluginToggles PluginToggles;
    public BaseServerVariables BaseServerVariables;
    public DecreasePopOptions DecreasePopOptions;
    public QueuedEnabledOptions QueuedEnabledOptions;
    public QueueingDisabledOptions QueueingDisabledOptions;
}


```

---

## DynamicPVP by HunterZ - Creates PvP zones upon certain actions/events/monuments

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using Facepunch;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;
using IEnumerator = System.Collections.IEnumerator;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Dynamic PVP", "HunterZ/CatMeat/Arainrr", "4.5.0", ResourceId = 2728)]
[Description("Creates temporary PvP zones on certain actions/events")]
public class DynamicPVP : RustPlugin
{
    [PluginReference]
    private readonly Plugin BotReSpawn;
    private readonly Plugin TruePVE;
    private readonly Plugin ZoneManager;
    private const string PermissionAdmin;
    private const string PrefabLargeOilRig;
    private const string PrefabOilRig;
    private const string PrefabSphere;
    private const string ZoneName;
    private readonly Dictionary<string, Timer> _eventTimers;
    private readonly Dictionary<ulong, LeftZone> _pvpDelays;
    private readonly Dictionary<string, string> _activeDynamicZones;
    private bool _dataChanged;
    private Vector3 _oilRigPosition;
    private Vector3 _largeOilRigPosition;
    private Coroutine _createEventsCoroutine;
    private bool _useExcludePlayer;
    private bool _subscribedCommands;
    private bool _subscribedDamage;
    private bool _subscribedZones;
    public static DynamicPVP Instance { get; set; }
    private sealed class LeftZone : Pool.IPooled
    {
        public string zoneId;
        public string eventName;
        public Timer zoneTimer;
        public void EnterPool();
        public void LeavePool();
    }

    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnServerSave();
    private void OnPlayerRespawned(BasePlayer player);
    private void TryRemoveEventTimer(string zoneId);
    private LeftZone GetOrAddPVPDelay(BasePlayer player, string zoneId, string eventName);
    private bool TryRemovePVPDelay(BasePlayer player);
    private bool CheckEntityOwner(BaseEntity baseEntity);
    private bool CanCreateDynamicPVP(string eventName, BaseEntity entity);
    private bool HasCommands();
    private void CheckCommandHooks(bool added);
    private void CheckPvpDelayHooks(bool added);
    private void CheckZoneHooks(bool added);
    private void CheckHooks(HookCheckReasons reasons);
    private BaseEvent GetBaseEvent(string eventName);
    private IEnumerator CreateEvents();
    private IEnumerator CreateGeneralEvents();
    private void StartupDieselEngine(DieselEngine dieselEngine);
    private void OnDieselEngineToggled(DieselEngine dieselEngine);
    private void StartupHackableLockedCrate(HackableLockedCrate hackableLockedCrate);
    private void OnEntitySpawned(HackableLockedCrate hackableLockedCrate);
    private void OnCrateHack(HackableLockedCrate hackableLockedCrate);
    private void OnCrateHackEnd(HackableLockedCrate hackableLockedCrate);
    private void OnLootEntity(BasePlayer player, HackableLockedCrate hackableLockedCrate);
    private void OnEntityKill(HackableLockedCrate hackableLockedCrate);
    private void LockedCrateEvent(HackableLockedCrate hackableLockedCrate);
    private static bool IsOnTheCargoShip(HackableLockedCrate hackableLockedCrate);
    private bool IsOnTheOilRig(HackableLockedCrate hackableLockedCrate);
    private void OnEntityDeath(PatrolHelicopter patrolHelicopter, HitInfo info);
    private void OnEntityDeath(BradleyAPC bradleyApc, HitInfo info);
    private void PatrolHelicopterEvent(PatrolHelicopter patrolHelicopter);
    private void BradleyApcEvent(BradleyAPC bradleyAPC);
    private readonly Dictionary<Vector3, Timer> activeSupplySignals;
    private void OnCargoPlaneSignaled(CargoPlane cargoPlane, SupplySignal supplySignal);
    private void OnEntitySpawned(SupplyDrop supplyDrop);
    private void OnSupplyDropLanded(SupplyDrop supplyDrop);
    private void OnLootEntity(BasePlayer _, SupplyDrop supplyDrop);
    private void OnEntityKill(SupplyDrop supplyDrop);
    private static string GetSupplyDropStateName(bool isLanded);
    private void OnSupplyDropEvent(SupplyDrop supplyDrop, bool isLanded);
    private KeyValuePair<Vector3, Timer>? GetSupplySignalNear(Vector3 position);
    private void StartupCargoShip(CargoShip cargoShip);
    private void HandleCargoState(CargoShip cargoShip, bool state);
    private void OnEntitySpawned(CargoShip cargoShip);
    private void OnEntityKill(CargoShip cargoShip);
    private void OnCargoShipEgress(CargoShip cargoShip);
    private void OnCargoShipHarborApproach(CargoShip cargoShip, CargoNotifier _);
    private void OnCargoShipHarborArrived(CargoShip cargoShip);
    private void OnCargoShipHarborLeave(CargoShip cargoShip);
    private IEnumerator CreateMonumentEvent(string monumentName, Transform transform, List<string> addedEvents, List<string> createdEvents);
    private IEnumerator CreateLandmarkMonumentEvents(List<string> addedEvents, List<string> createdEvents);
    private string ToTunnelSectionEventName(string cellName);
    private IEnumerator CreateTunnelSectionMonumentEvents(List<string> addedEvents, List<string> createdEvents);
    private int GetTunnelDwellingCenterIndex(string name, List<GameObject> links);
    private IEnumerator CreateTunnelDwellingMonumentEvents(List<string> addedEvents, List<string> createdEvents);
    private IEnumerator CreateMonumentEvents();
    private IEnumerator CreateAutoEvents();
    private object OnPlayerCommand(BasePlayer player, string command, string[] args);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private object CheckCommand(BasePlayer player, string command, bool isChat);
    private bool IsBlockedCommand(BaseEvent baseEvent, string command, bool isChat);
    private void HandleParentedEntityEvent(string eventName, BaseEntity parentEntity, bool parentOnCreate, bool delay);
    private bool HandleMonumentEvent(string eventName, Transform transform, MonumentEvent monumentEvent);
    private void HandleGeneralEvent(GeneralEventType generalEventType, BaseEntity baseEntity, bool useEntityId);
    private bool CreateDynamicZone(string eventName, Vector3 position, string zoneId, string[] zoneSettings, bool delay);
    private void HandleDeleteDynamicZone(string zoneId, float duration, string eventName);
    private void HandleDeleteDynamicZone(string zoneId);
    private bool DeleteDynamicZone(string zoneId);
    private void DeleteOldDynamicZones();
    private readonly Dictionary<string, List<SphereEntity>> _zoneSpheres;
    private static bool DomeCreateAllowed(DomeEvent domeEvent, ISphereZone sphereZone);
    private bool CreateDome(string zoneId, Vector3 position, float radius, int darkness);
    private void ParentDome(string zoneId, Vector3 position, BaseEntity parentEntity);
    private bool RemoveDome(string zoneId);
    private object CanEntityTakeDamage(BasePlayer victim, HitInfo info);
    private static bool TP_AddOrUpdateMapping(string zoneId, string mapping);
    private static bool TP_RemoveMapping(string zoneId);
    private bool BotReSpawnAllowed(BotDomeEvent botEvent);
    private bool SpawnBots(Vector3 location, string profileName, string groupId);
    private bool KillBots(string groupId);
    private string[] BS_AddGroupSpawn(Vector3 location, string profileName, string groupId, int quantity);
    private string[] BS_RemoveGroupSpawn(string groupId);
    private void OnEnterZone(string zoneId, BasePlayer player);
    private void OnExitZone(string zoneId, BasePlayer player);
    private bool ZM_CreateOrUpdateZone(string zoneId, string[] zoneArgs, Vector3 location);
    private bool ZM_EraseZone(string zoneId, string eventName);
    private string[] ZM_GetZoneIDs();
    private string ZM_GetZoneName(string zoneId);
    private ZoneManager.Zone ZM_GetZoneByID(string zoneId);
    private string[] ZM_GetPlayerZoneIDs(BasePlayer player);
    private bool ZM_IsPlayerInZone(LeftZone leftZone, BasePlayer player);
    private List<BasePlayer> ZM_GetPlayersInZone(string zoneId);
    private void ParentEventToEntity(string zoneId, BaseEvent baseEvent, BaseEntity parentEntity, bool deleteOnFailure, bool delay);
    private StringBuilder _debugStringBuilder;
    private void PrintDebug(string message, DebugLevel level);
    private void SaveDebug();
    private string[] AllDynamicPVPZones();
    private bool IsDynamicPVPZone(string zoneId);
    private bool EventDataExists(string eventName);
    private bool IsPlayerInPVPDelay(ulong playerId);
    private string GetPlayerPVPDelayedZoneID(ulong playerId);
    private string GetEventName(string zoneId);
    private bool CreateOrUpdateEventData(string eventName, string eventData, bool isTimed);
    private bool CreateEventData(string eventName, Vector3 position, bool isTimed);
    private bool RemoveEventData(string eventName, bool forceClose);
    private bool StartEvent(string eventName, Vector3 position);
    private bool StopEvent(string eventName);
    private bool ForceCloseZones(string eventName);
    private bool IsUsingExcludePlayer();
    private static void DrawCube(BasePlayer player, float duration, Color color, Vector3 pos, Vector3 size, float rotation);
    private static void DrawSphere(BasePlayer player, float duration, Color color, Vector3 pos, float radius, float rotation);
    private void CommandHelp(IPlayer iPlayer);
    private void CommandList(IPlayer iPlayer);
    private void CommandShow(BasePlayer player);
    private void CommandEdit(IPlayer iPlayer, string eventName, Vector3 position, string arg);
    private void CmdDynamicPVP(IPlayer iPlayer, string command, string[] args);
    private ConfigData configData;
    private sealed class ConfigData
    {
        [JsonProperty(PropertyName = "Global Settings")]
        public GlobalSettings Global { get; set; }
        [JsonProperty(PropertyName = "Chat Settings")]
        public ChatSettings Chat { get; set; }
        [JsonProperty(PropertyName = "General Event Settings")]
        public GeneralEventSettings GeneralEvents { get; set; }
        [JsonProperty(PropertyName = "Monument Event Settings")]
        public SortedDictionary<string, MonumentEvent> MonumentEvents { get; set; }
        [JsonProperty(PropertyName = "Version")]
        public VersionNumber Version { get; set; }
    }

    private sealed class GlobalSettings
    {
        [JsonProperty(PropertyName = "Enable Debug Mode")]
        public bool DebugEnabled { get; set; }
        [JsonProperty(PropertyName = "Log Debug To File")]
        public bool LogToFile { get; set; }
        [JsonProperty(PropertyName = "Compare Radius (Used to determine if it is a SupplySignal)")]
        public float CompareRadius { get; set; }
        [JsonProperty(PropertyName = "If the entity has an owner, don't create a PVP zone")]
        public bool CheckEntityOwner { get; set; }
        [JsonProperty(PropertyName = "Use TruePVE PVP Delay API (more efficient and cross-plugin, but supersedes PVP Delay Flags)")]
        public bool UseExcludePlayer { get; set; }
        [JsonProperty(PropertyName = "PVP Delay Flags")]
        public PvpDelayTypes PvpDelayFlags { get; set; }
    }

    private sealed class ChatSettings
    {
        [JsonProperty(PropertyName = "Command")]
        public string Command { get; set; }
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string Prefix { get; set; }
        [JsonProperty(PropertyName = "Chat Prefix Color")]
        public string PrefixColor { get; set; }
        [JsonProperty(PropertyName = "Chat SteamID Icon")]
        public ulong SteamIdIcon { get; set; }
        [JsonProperty(PropertyName = "Zone Show Duration (in seconds)")]
        public float ShowDuration { get; set; }
    }

    private sealed class GeneralEventSettings
    {
        [JsonProperty(PropertyName = "Bradley Event")]
        public TimedEvent BradleyApc { get; set; }
        [JsonProperty(PropertyName = "Patrol Helicopter Event")]
        public TimedEvent PatrolHelicopter { get; set; }
        [JsonProperty(PropertyName = "Supply Signal Event")]
        public SupplyDropEvent SupplySignal { get; set; }
        [JsonProperty(PropertyName = "Timed Supply Event")]
        public SupplyDropEvent TimedSupply { get; set; }
        [JsonProperty(PropertyName = "Hackable Crate Event")]
        public HackableCrateEvent HackableCrate { get; set; }
        [JsonProperty(PropertyName = "Excavator Ignition Event")]
        public MonumentEvent ExcavatorIgnition { get; set; }
        [JsonProperty(PropertyName = "Cargo Ship Event")]
        public CargoShipEvent CargoShip { get; set; }
    }

    public abstract class BaseEvent
    {
        [JsonProperty(PropertyName = "Enable Event", Order = 1)]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Delay In Starting Event", Order = 2)]
        public float EventStartDelay { get; set; }
        [JsonProperty(PropertyName = "Delay In Stopping Event", Order = 3)]
        public float EventStopDelay { get; set; }
        [JsonProperty(PropertyName = "Holster Time On Enter (In seconds, or 0 to disable)", Order = 4)]
        public float HolsterTime { get; set; }
        [JsonProperty(PropertyName = "Enable PVP Delay", Order = 5)]
        public bool PvpDelayEnabled { get; set; }
        [JsonProperty(PropertyName = "PVP Delay Time", Order = 6)]
        public float PvpDelayTime { get; set; }
        [JsonProperty(PropertyName = "TruePVE Mapping", Order = 7)]
        public string Mapping { get; set; }
        [JsonProperty(PropertyName = "Use Blacklist Commands (If false, a whitelist is used)", Order = 8)]
        public bool UseBlacklistCommands { get; set; }
        [JsonProperty(PropertyName = "Command works for PVP delayed players", Order = 9)]
        public bool CommandWorksForPVPDelay { get; set; }
        [JsonProperty(PropertyName = "Command List (If there is a '/' at the front, it is a chat command)", Order = 10)]
        public List<string> CommandList { get; set; }
        public abstract BaseDynamicZone GetDynamicZone();
    }

    public abstract class DomeEvent : BaseEvent
    {
        [JsonProperty(PropertyName = "Enable Domes", Order = 20)]
        public bool DomesEnabled { get; set; }
        [JsonProperty(PropertyName = "Domes Darkness", Order = 21)]
        public int DomesDarkness { get; set; }
    }

    public abstract class BotDomeEvent : DomeEvent
    {
        [JsonProperty(PropertyName = "Enable Bots (Need BotSpawn Plugin)", Order = 30)]
        public bool BotsEnabled { get; set; }
        [JsonProperty(PropertyName = "BotSpawn Profile Name", Order = 31)]
        public string BotProfileName { get; set; }
    }

    public class MonumentEvent : DomeEvent
    {
        [JsonProperty(PropertyName = "Dynamic PVP Zone Settings", Order = 40)]
        public SphereCubeDynamicZone DynamicZone { get; set; }
        [JsonProperty(PropertyName = "Zone ID", Order = 41)]
        public string ZoneId { get; set; }
        [JsonProperty(PropertyName = "Transform Position", Order = 42)]
        public Vector3 TransformPosition { get; set; }
        public override BaseDynamicZone GetDynamicZone();
    }

    public class AutoEvent : BotDomeEvent
    {
        [JsonProperty(PropertyName = "Dynamic PVP Zone Settings", Order = 50)]
        public SphereCubeDynamicZone DynamicZone { get; set; }
        [JsonProperty(PropertyName = "Auto Start", Order = 51)]
        public bool AutoStart { get; set; }
        [JsonProperty(PropertyName = "Zone ID", Order = 52)]
        public string ZoneId { get; set; }
        [JsonProperty(PropertyName = "Position", Order = 53)]
        public Vector3 Position { get; set; }
        public override BaseDynamicZone GetDynamicZone();
    }

    public abstract class BaseTimedEvent : BotDomeEvent
    {
        [JsonProperty(PropertyName = "Event Duration", Order = 60)]
        public float Duration { get; set; }
    }

    public class TimedEvent : BaseTimedEvent
    {
        [JsonProperty(PropertyName = "Dynamic PVP Zone Settings", Order = 65)]
        public SphereCubeDynamicZone DynamicZone { get; set; }
        public override BaseDynamicZone GetDynamicZone();
    }

    public class HackableCrateEvent : BaseTimedEvent, ITimedDisable
    {
        [JsonProperty(PropertyName = "Dynamic PVP Zone Settings", Order = 70)]
        public SphereCubeParentDynamicZone DynamicZone { get; set; }
        [JsonProperty(PropertyName = "Start Event When Spawned (If false, the event starts when unlocking)", Order = 71)]
        public bool StartWhenSpawned { get; set; }
        [JsonProperty(PropertyName = "Stop Event When Killed", Order = 72)]
        public bool StopWhenKilled { get; set; }
        [JsonProperty(PropertyName = "Event Timer Starts When Looted", Order = 73)]
        public bool TimerStartWhenLooted { get; set; }
        [JsonProperty(PropertyName = "Event Timer Starts When Unlocked", Order = 74)]
        public bool TimerStartWhenUnlocked { get; set; }
        [JsonProperty(PropertyName = "Excluding Hackable Crate On OilRig", Order = 75)]
        public bool ExcludeOilRig { get; set; }
        [JsonProperty(PropertyName = "Excluding Hackable Crate on Cargo Ship", Order = 76)]
        public bool ExcludeCargoShip { get; set; }
        public override BaseDynamicZone GetDynamicZone();
        public bool IsTimedDisabled();
    }

    public class SupplyDropEvent : BaseTimedEvent, ITimedDisable
    {
        [JsonProperty(PropertyName = "Dynamic PVP Zone Settings", Order = 80)]
        public SphereCubeParentDynamicZone DynamicZone { get; set; }
        [JsonProperty(PropertyName = "Start Event When Spawned (If false, the event starts when landed)", Order = 81)]
        public bool StartWhenSpawned { get; set; }
        [JsonProperty(PropertyName = "Stop Event When Killed", Order = 82)]
        public bool StopWhenKilled { get; set; }
        [JsonProperty(PropertyName = "Event Timer Starts When Looted", Order = 83)]
        public bool TimerStartWhenLooted { get; set; }
        public override BaseDynamicZone GetDynamicZone();
        public bool IsTimedDisabled();
    }

    public class CargoShipEvent : DomeEvent
    {
        [JsonProperty(PropertyName = "Event State On Spawn (true=enabled, false=disabled)", Order = 90)]
        public bool SpawnState { get; set; }
        [JsonProperty(PropertyName = "Event State On Harbor Approach", Order = 91)]
        public bool ApproachState { get; set; }
        [JsonProperty(PropertyName = "Event State On Harbor Docking", Order = 92)]
        public bool DockState { get; set; }
        [JsonProperty(PropertyName = "Event State On Harbor Departure", Order = 93)]
        public bool DepartState { get; set; }
        [JsonProperty(PropertyName = "Event State On Map Egress", Order = 94)]
        public bool EgressState { get; set; }
        [JsonProperty(PropertyName = "Dynamic PVP Zone Settings", Order = 95)]
        public SphereCubeParentDynamicZone DynamicZone { get; set; }
        public override BaseDynamicZone GetDynamicZone();
    }

    public abstract class BaseDynamicZone
    {
        [JsonProperty(PropertyName = "Zone Comfort", Order = 100)]
        public float Comfort { get; set; }
        [JsonProperty(PropertyName = "Zone Radiation", Order = 101)]
        public float Radiation { get; set; }
        [JsonProperty(PropertyName = "Zone Temperature", Order = 102)]
        public float Temperature { get; set; }
        [JsonProperty(PropertyName = "Enable Safe Zone", Order = 103)]
        public bool SafeZone { get; set; }
        [JsonProperty(PropertyName = "Eject Spawns", Order = 104)]
        public string EjectSpawns { get; set; }
        [JsonProperty(PropertyName = "Zone Parent ID", Order = 105)]
        public string ParentId { get; set; }
        [JsonProperty(PropertyName = "Enter Message", Order = 106)]
        public string EnterMessage { get; set; }
        [JsonProperty(PropertyName = "Leave Message", Order = 107)]
        public string LeaveMessage { get; set; }
        [JsonProperty(PropertyName = "Permission Required To Enter Zone", Order = 108)]
        public string Permission { get; set; }
        [JsonProperty(PropertyName = "Extra Zone Flags", Order = 109)]
        public List<string> ExtraZoneFlags { get; set; }
        private string[] _zoneSettings;
        public virtual string[] ZoneSettings(Transform transform);
        protected void GetBaseZoneSettings(List<string> zoneSettings);
        protected abstract string[] GetZoneSettings(Transform transform);
    }

    public class SphereCubeDynamicZone : BaseDynamicZone, ISphereZone, ICubeZone, IRotateZone
    {
        [JsonProperty(PropertyName = "Zone Radius", Order = 140)]
        public float Radius { get; set; }
        [JsonProperty(PropertyName = "Zone Size", Order = 141)]
        public Vector3 Size { get; set; }
        [JsonProperty(PropertyName = "Zone Rotation", Order = 142)]
        public float Rotation { get; set; }
        [JsonProperty(PropertyName = "Fixed Rotation", Order = 143)]
        public bool FixedRotation { get; set; }
        public override string[] ZoneSettings(Transform transform);
        protected override string[] GetZoneSettings(Transform transform);
    }

    public class SphereCubeParentDynamicZone : BaseDynamicZone, ISphereZone, ICubeZone, IParentZone
    {
        [JsonProperty(PropertyName = "Zone Radius", Order = 200)]
        public float Radius { get; set; }
        [JsonProperty(PropertyName = "Zone Size", Order = 201)]
        public Vector3 Size { get; set; }
        [JsonProperty(PropertyName = "Transform Position", Order = 202)]
        public Vector3 Center { get; set; }
        public override string[] ZoneSettings(Transform transform);
        protected override string[] GetZoneSettings(Transform transform);
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private bool GetConfigValue(T value, string[] path);
    private StoredData storedData;
    private sealed class StoredData
    {
        public readonly Dictionary<string, TimedEvent> timedEvents;
        public readonly Dictionary<string, AutoEvent> autoEvents;
        public bool EventDataExists(string eventName);
        public void RemoveEventData(string eventName);
        [JsonIgnore]
        public int CustomEventsCount { get; set; }
    }

    private void LoadData();
    private void ClearData();
    private void SaveData();
    private void Print(IPlayer iPlayer, string message);
    private void Print(BasePlayer player, string message);
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
}

private sealed class LeftZone : Pool.IPooled
{
    public string zoneId;
    public string eventName;
    public Timer zoneTimer;
    public void EnterPool();
    public void LeavePool();
}

private sealed class ConfigData
{
    [JsonProperty(PropertyName = "Global Settings")]
    public GlobalSettings Global { get; set; }
    [JsonProperty(PropertyName = "Chat Settings")]
    public ChatSettings Chat { get; set; }
    [JsonProperty(PropertyName = "General Event Settings")]
    public GeneralEventSettings GeneralEvents { get; set; }
    [JsonProperty(PropertyName = "Monument Event Settings")]
    public SortedDictionary<string, MonumentEvent> MonumentEvents { get; set; }
    [JsonProperty(PropertyName = "Version")]
    public VersionNumber Version { get; set; }
}

private sealed class GlobalSettings
{
    [JsonProperty(PropertyName = "Enable Debug Mode")]
    public bool DebugEnabled { get; set; }
    [JsonProperty(PropertyName = "Log Debug To File")]
    public bool LogToFile { get; set; }
    [JsonProperty(PropertyName = "Compare Radius (Used to determine if it is a SupplySignal)")]
    public float CompareRadius { get; set; }
    [JsonProperty(PropertyName = "If the entity has an owner, don't create a PVP zone")]
    public bool CheckEntityOwner { get; set; }
    [JsonProperty(PropertyName = "Use TruePVE PVP Delay API (more efficient and cross-plugin, but supersedes PVP Delay Flags)")]
    public bool UseExcludePlayer { get; set; }
    [JsonProperty(PropertyName = "PVP Delay Flags")]
    public PvpDelayTypes PvpDelayFlags { get; set; }
}

private sealed class ChatSettings
{
    [JsonProperty(PropertyName = "Command")]
    public string Command { get; set; }
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string Prefix { get; set; }
    [JsonProperty(PropertyName = "Chat Prefix Color")]
    public string PrefixColor { get; set; }
    [JsonProperty(PropertyName = "Chat SteamID Icon")]
    public ulong SteamIdIcon { get; set; }
    [JsonProperty(PropertyName = "Zone Show Duration (in seconds)")]
    public float ShowDuration { get; set; }
}

private sealed class GeneralEventSettings
{
    [JsonProperty(PropertyName = "Bradley Event")]
    public TimedEvent BradleyApc { get; set; }
    [JsonProperty(PropertyName = "Patrol Helicopter Event")]
    public TimedEvent PatrolHelicopter { get; set; }
    [JsonProperty(PropertyName = "Supply Signal Event")]
    public SupplyDropEvent SupplySignal { get; set; }
    [JsonProperty(PropertyName = "Timed Supply Event")]
    public SupplyDropEvent TimedSupply { get; set; }
    [JsonProperty(PropertyName = "Hackable Crate Event")]
    public HackableCrateEvent HackableCrate { get; set; }
    [JsonProperty(PropertyName = "Excavator Ignition Event")]
    public MonumentEvent ExcavatorIgnition { get; set; }
    [JsonProperty(PropertyName = "Cargo Ship Event")]
    public CargoShipEvent CargoShip { get; set; }
}

public abstract class BaseEvent
{
    [JsonProperty(PropertyName = "Enable Event", Order = 1)]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Delay In Starting Event", Order = 2)]
    public float EventStartDelay { get; set; }
    [JsonProperty(PropertyName = "Delay In Stopping Event", Order = 3)]
    public float EventStopDelay { get; set; }
    [JsonProperty(PropertyName = "Holster Time On Enter (In seconds, or 0 to disable)", Order = 4)]
    public float HolsterTime { get; set; }
    [JsonProperty(PropertyName = "Enable PVP Delay", Order = 5)]
    public bool PvpDelayEnabled { get; set; }
    [JsonProperty(PropertyName = "PVP Delay Time", Order = 6)]
    public float PvpDelayTime { get; set; }
    [JsonProperty(PropertyName = "TruePVE Mapping", Order = 7)]
    public string Mapping { get; set; }
    [JsonProperty(PropertyName = "Use Blacklist Commands (If false, a whitelist is used)", Order = 8)]
    public bool UseBlacklistCommands { get; set; }
    [JsonProperty(PropertyName = "Command works for PVP delayed players", Order = 9)]
    public bool CommandWorksForPVPDelay { get; set; }
    [JsonProperty(PropertyName = "Command List (If there is a '/' at the front, it is a chat command)", Order = 10)]
    public List<string> CommandList { get; set; }
    public abstract BaseDynamicZone GetDynamicZone();
}

public abstract class DomeEvent : BaseEvent
{
    [JsonProperty(PropertyName = "Enable Domes", Order = 20)]
    public bool DomesEnabled { get; set; }
    [JsonProperty(PropertyName = "Domes Darkness", Order = 21)]
    public int DomesDarkness { get; set; }
}

public abstract class BotDomeEvent : DomeEvent
{
    [JsonProperty(PropertyName = "Enable Bots (Need BotSpawn Plugin)", Order = 30)]
    public bool BotsEnabled { get; set; }
    [JsonProperty(PropertyName = "BotSpawn Profile Name", Order = 31)]
    public string BotProfileName { get; set; }
}

public class MonumentEvent : DomeEvent
{
    [JsonProperty(PropertyName = "Dynamic PVP Zone Settings", Order = 40)]
    public SphereCubeDynamicZone DynamicZone { get; set; }
    [JsonProperty(PropertyName = "Zone ID", Order = 41)]
    public string ZoneId { get; set; }
    [JsonProperty(PropertyName = "Transform Position", Order = 42)]
    public Vector3 TransformPosition { get; set; }
    public override BaseDynamicZone GetDynamicZone();
}

public class AutoEvent : BotDomeEvent
{
    [JsonProperty(PropertyName = "Dynamic PVP Zone Settings", Order = 50)]
    public SphereCubeDynamicZone DynamicZone { get; set; }
    [JsonProperty(PropertyName = "Auto Start", Order = 51)]
    public bool AutoStart { get; set; }
    [JsonProperty(PropertyName = "Zone ID", Order = 52)]
    public string ZoneId { get; set; }
    [JsonProperty(PropertyName = "Position", Order = 53)]
    public Vector3 Position { get; set; }
    public override BaseDynamicZone GetDynamicZone();
}

public abstract class BaseTimedEvent : BotDomeEvent
{
    [JsonProperty(PropertyName = "Event Duration", Order = 60)]
    public float Duration { get; set; }
}

public class TimedEvent : BaseTimedEvent
{
    [JsonProperty(PropertyName = "Dynamic PVP Zone Settings", Order = 65)]
    public SphereCubeDynamicZone DynamicZone { get; set; }
    public override BaseDynamicZone GetDynamicZone();
}

public class HackableCrateEvent : BaseTimedEvent, ITimedDisable
{
    [JsonProperty(PropertyName = "Dynamic PVP Zone Settings", Order = 70)]
    public SphereCubeParentDynamicZone DynamicZone { get; set; }
    [JsonProperty(PropertyName = "Start Event When Spawned (If false, the event starts when unlocking)", Order = 71)]
    public bool StartWhenSpawned { get; set; }
    [JsonProperty(PropertyName = "Stop Event When Killed", Order = 72)]
    public bool StopWhenKilled { get; set; }
    [JsonProperty(PropertyName = "Event Timer Starts When Looted", Order = 73)]
    public bool TimerStartWhenLooted { get; set; }
    [JsonProperty(PropertyName = "Event Timer Starts When Unlocked", Order = 74)]
    public bool TimerStartWhenUnlocked { get; set; }
    [JsonProperty(PropertyName = "Excluding Hackable Crate On OilRig", Order = 75)]
    public bool ExcludeOilRig { get; set; }
    [JsonProperty(PropertyName = "Excluding Hackable Crate on Cargo Ship", Order = 76)]
    public bool ExcludeCargoShip { get; set; }
    public override BaseDynamicZone GetDynamicZone();
    public bool IsTimedDisabled();
}

public class SupplyDropEvent : BaseTimedEvent, ITimedDisable
{
    [JsonProperty(PropertyName = "Dynamic PVP Zone Settings", Order = 80)]
    public SphereCubeParentDynamicZone DynamicZone { get; set; }
    [JsonProperty(PropertyName = "Start Event When Spawned (If false, the event starts when landed)", Order = 81)]
    public bool StartWhenSpawned { get; set; }
    [JsonProperty(PropertyName = "Stop Event When Killed", Order = 82)]
    public bool StopWhenKilled { get; set; }
    [JsonProperty(PropertyName = "Event Timer Starts When Looted", Order = 83)]
    public bool TimerStartWhenLooted { get; set; }
    public override BaseDynamicZone GetDynamicZone();
    public bool IsTimedDisabled();
}

public class CargoShipEvent : DomeEvent
{
    [JsonProperty(PropertyName = "Event State On Spawn (true=enabled, false=disabled)", Order = 90)]
    public bool SpawnState { get; set; }
    [JsonProperty(PropertyName = "Event State On Harbor Approach", Order = 91)]
    public bool ApproachState { get; set; }
    [JsonProperty(PropertyName = "Event State On Harbor Docking", Order = 92)]
    public bool DockState { get; set; }
    [JsonProperty(PropertyName = "Event State On Harbor Departure", Order = 93)]
    public bool DepartState { get; set; }
    [JsonProperty(PropertyName = "Event State On Map Egress", Order = 94)]
    public bool EgressState { get; set; }
    [JsonProperty(PropertyName = "Dynamic PVP Zone Settings", Order = 95)]
    public SphereCubeParentDynamicZone DynamicZone { get; set; }
    public override BaseDynamicZone GetDynamicZone();
}

public abstract class BaseDynamicZone
{
    [JsonProperty(PropertyName = "Zone Comfort", Order = 100)]
    public float Comfort { get; set; }
    [JsonProperty(PropertyName = "Zone Radiation", Order = 101)]
    public float Radiation { get; set; }
    [JsonProperty(PropertyName = "Zone Temperature", Order = 102)]
    public float Temperature { get; set; }
    [JsonProperty(PropertyName = "Enable Safe Zone", Order = 103)]
    public bool SafeZone { get; set; }
    [JsonProperty(PropertyName = "Eject Spawns", Order = 104)]
    public string EjectSpawns { get; set; }
    [JsonProperty(PropertyName = "Zone Parent ID", Order = 105)]
    public string ParentId { get; set; }
    [JsonProperty(PropertyName = "Enter Message", Order = 106)]
    public string EnterMessage { get; set; }
    [JsonProperty(PropertyName = "Leave Message", Order = 107)]
    public string LeaveMessage { get; set; }
    [JsonProperty(PropertyName = "Permission Required To Enter Zone", Order = 108)]
    public string Permission { get; set; }
    [JsonProperty(PropertyName = "Extra Zone Flags", Order = 109)]
    public List<string> ExtraZoneFlags { get; set; }
    private string[] _zoneSettings;
    public virtual string[] ZoneSettings(Transform transform);
    protected void GetBaseZoneSettings(List<string> zoneSettings);
    protected abstract string[] GetZoneSettings(Transform transform);
}

public class SphereCubeDynamicZone : BaseDynamicZone, ISphereZone, ICubeZone, IRotateZone
{
    [JsonProperty(PropertyName = "Zone Radius", Order = 140)]
    public float Radius { get; set; }
    [JsonProperty(PropertyName = "Zone Size", Order = 141)]
    public Vector3 Size { get; set; }
    [JsonProperty(PropertyName = "Zone Rotation", Order = 142)]
    public float Rotation { get; set; }
    [JsonProperty(PropertyName = "Fixed Rotation", Order = 143)]
    public bool FixedRotation { get; set; }
    public override string[] ZoneSettings(Transform transform);
    protected override string[] GetZoneSettings(Transform transform);
}

public class SphereCubeParentDynamicZone : BaseDynamicZone, ISphereZone, ICubeZone, IParentZone
{
    [JsonProperty(PropertyName = "Zone Radius", Order = 200)]
    public float Radius { get; set; }
    [JsonProperty(PropertyName = "Zone Size", Order = 201)]
    public Vector3 Size { get; set; }
    [JsonProperty(PropertyName = "Transform Position", Order = 202)]
    public Vector3 Center { get; set; }
    public override string[] ZoneSettings(Transform transform);
    protected override string[] GetZoneSettings(Transform transform);
}

private sealed class StoredData
{
    public readonly Dictionary<string, TimedEvent> timedEvents;
    public readonly Dictionary<string, AutoEvent> autoEvents;
    public bool EventDataExists(string eventName);
    public void RemoveEventData(string eventName);
    [JsonIgnore]
    public int CustomEventsCount { get; set; }
}


```

---

## DynamicSlotsLimit by  - Modifies maximal server slots based on current players count

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Dynamic Slots Limit", "Orange", "1.0.5")]
[Description("Modifies maximal server slots based on current players count")]
public class DynamicSlotsLimit : CovalencePlugin
{
    private void OnServerInitialized();
    private void OnUserConnected(IPlayer player);
    private void OnUserDisconnected(IPlayer player);
    private void Unload();
    private void CheckSlots();
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Minimal slots")]
        public int slotsMin;
        [JsonProperty(PropertyName = "Maximal slots")]
        public int slotsMax;
        [JsonProperty(PropertyName = "Step rate")]
        public int changeStep;
        [JsonProperty(PropertyName = "Trigger when X slots left")]
        public int triggerStep;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Minimal slots")]
    public int slotsMin;
    [JsonProperty(PropertyName = "Maximal slots")]
    public int slotsMax;
    [JsonProperty(PropertyName = "Step rate")]
    public int changeStep;
    [JsonProperty(PropertyName = "Trigger when X slots left")]
    public int triggerStep;
}


```

---

## DynamicWireColors by WhiteThunder - Changes wire & hose colors while they are providing insufficient power or fluid

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using static WireTool;
using static IOEntity;

Oxide.Plugins
[Info("Dynamic Wire Colors", "WhiteThunder", "1.1.2")]
[Description("Temporarily changes the color of wires and hoses while they are providing insufficient power or fluid.")]
internal class DynamicWireColors : CovalencePlugin
{
    private Configuration _config;
    private const string PermissionUse;
    private WaitWhile WaitWhileSaving;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnOutputUpdate(IOEntity sourceEntity);
    private void OnServerSave();
    private bool ChangeColorWasBlocked(IOEntity ioEntity, IOSlot slot, WireColour color);
    private IOSlot GetConnectedSourceSlot(IOSlot destinationSlot, IOEntity sourceEntity);
    private IOSlot GetConnectedDestinationSlot(IOSlot sourceSlot, IOEntity destinationEntity);
    private void CopySourceSlotColorsToDestinationSlots(IOEntity sourceEntity);
    private bool EntityHasPermission(IOEntity ioEntity);
    private bool EitherEntityHasPermission(IOEntity ioEntity1, IOEntity ioEntity2, string perm);
    private void ChangeSlotColor(IOEntity ioEntity, IOSlot slot, WireColour color, bool networkUpdate);
    private void SetupAllEntities(bool networkUpdate, bool fixDestinationColor);
    private void ResetAllEntities(bool networkUpdate);
    private bool HasOtherMainInput(IOEntity ioEntity, IOSlot currentSlot);
    private bool SufficientPowerOrFluid(IOEntity destinationEntity, IOSlot destinationSlot, int inputAmount);
    private void ProcessSourceEntity(IOEntity sourceEntity, bool networkUpdate);
    private IEnumerator ResetColorsWhileSaving();
    private class Configuration : BaseConfiguration
    {
        [JsonProperty("InsufficientPowerColor")]
        [JsonConverter(typeof(StringEnumConverter))]
        public WireColour InsufficientPowerColor;
        [JsonProperty("InsufficientFluidColor")]
        [JsonConverter(typeof(StringEnumConverter))]
        public WireColour InsufficientFluidColor;
        [JsonProperty("RequiresPermission")]
        public bool RequiresPermission;
        [JsonProperty("AppliesToUnownedEntities")]
        public bool AppliesToUnownedEntities;
        public WireColour GetInsufficientColorForType(IOType ioType);
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
}

private class Configuration : BaseConfiguration
{
    [JsonProperty("InsufficientPowerColor")]
    [JsonConverter(typeof(StringEnumConverter))]
    public WireColour InsufficientPowerColor;
    [JsonProperty("InsufficientFluidColor")]
    [JsonConverter(typeof(StringEnumConverter))]
    public WireColour InsufficientFluidColor;
    [JsonProperty("RequiresPermission")]
    public bool RequiresPermission;
    [JsonProperty("AppliesToUnownedEntities")]
    public bool AppliesToUnownedEntities;
    public WireColour GetInsufficientColorForType(IOType ioType);
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


```

---

## EasyAirdrop by LaserHydra - Call airdrops using simple commands

```csharp
using System.Collections.Generic;
using UnityEngine;
using System.Linq;
using System;

Oxide.Plugins
[Info("Easy Airdrop", "LaserHydra", "3.2.5", ResourceId = 860)]
[Description("Call airdrops using simple commands")]
 class EasyAirdrop : RustPlugin
{
     void Loaded();
    new void LoadConfig();
     void LoadMessages();
     string msg(string key, string id);
    protected override void LoadDefaultConfig();
    [ConsoleCommand("airdrop")]
     void ccmdAirdrop(ConsoleSystem.Arg arg);
    [ConsoleCommand("massdrop")]
     void ccmdMassdrop(ConsoleSystem.Arg arg);
    [ChatCommand("airdrop")]
     void cmdAirdrop(BasePlayer player, string cmd, string[] args);
    [ChatCommand("massdrop")]
     void cmdMassdrop(BasePlayer player, string cmd, string[] args);
     void SpawnPlayerAirdrop(BasePlayer player, BasePlayer target);
     void SpawnMassdrop(BasePlayer player, int amount);
     void SpawnRandomAirdrop(BasePlayer player);
     void SpawnAirdrop(Vector3 position);
     bool HasPermission(BasePlayer player, string perm);
     Vector3 GetRandomVector();
     void RunAsChatCommand(ConsoleSystem.Arg arg, Action<BasePlayer, string, string[]> command);
     BasePlayer GetPlayer(string searchedPlayer, BasePlayer executer, string prefix);
     string ListToString(List<string> list, int first, string seperator);
     void SetConfig(object[] args);
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
     void AnnounceAirdrop(BasePlayer player, Vector3[] locations);
}


```

---

## EasyBroadcast by LaserHydra - Provides a command to broadcast a message to the chat

```csharp
using System.Collections.Generic;
using System.Linq;
using System;

Oxide.Plugins
[Info("Easy Broadcast", "LaserHydra", "2.1.0", ResourceId = 863)]
[Description("Broadcast a message to the server")]
 class EasyBroadcast : RustPlugin
{
     void Loaded();
     void LoadConfig();
    protected override void LoadDefaultConfig();
    [ChatCommand("bcast")]
     void cmdBroadcast(BasePlayer player, string cmd, string[] args);
     string ListToString(List<string> list, int first, string seperator);
     void SetConfig(object[] args);
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}


```

---

## EasyResearch by  - Adds a few new features to Research System

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using WebSocketSharp;
using UnityEngine;
using System.Linq;
using System.Globalization;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using Oxide.Plugins.EasyResearchEx;
using Rust;

Oxide.Plugins
[Info("Easy Research", "Khan", "2.1.3")]
[Description("Adds new features to TechTree, Research Table & Research Systems")]
public class EasyResearch : CovalencePlugin
{
    [PluginReference]
    private Plugin PopupNotifications;
    private Plugin Economics;
    private Plugin ServerRewards;
    private Plugin XPerience;
    private Plugin Notify;
    private Plugin LangAPI;
     PluginConfig _confg;
    private const string UsePerm;
    private const string ByPass;
    private void CheckConfig();
    private class PluginConfig
    {
        [JsonProperty("Chat Prefix")]
        public string ChatPrefix;
        [JsonProperty("Research Requirement this is the type of resource used to research items (Expects Item Shortname)")]
        public string Requirement;
        [JsonProperty("Already Researched Toggle")]
        public bool Researched;
        [JsonProperty("Block Tech Tree Researching")]
        public bool BlockTechTree;
        [JsonProperty("Use Popup Notifications")]
        public bool Popup;
        [JsonProperty("Use Notify Notifications (Mevents Version CodeFling)")]
        public bool Notify;
        [JsonProperty("Notify Notification Type")]
        public int NotifyType;
        [JsonProperty("Use CustomUI Overlay Notifications")]
        public bool CustomUI;
        [JsonProperty("Enable Economics")]
        public bool Economics;
        [JsonProperty("Enable ServerRewards")]
        public bool ServerRewards;
        [JsonProperty("Enable Custom Currency")]
        public bool Custom;
        [JsonProperty("Custom Name")]
        public string CName;
        [JsonProperty("Custom Item ID")]
        public int CId;
        [JsonProperty("Custom SkinId")]
        public ulong CSkinId;
        [JsonProperty("Research Table UI Options")]
        public ResearchTableUI ResearchTableUI;
        [JsonProperty("TechTree UI Options")]
        public TechTreeUI TechTreeUI;
        [JsonProperty("Blocked Items")]
        public List<string> Blocked;
        [JsonProperty("Custom Research Options")]
        public Dictionary<string, Research> ResearchSettings;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    public class Research
    {
        public string DisplayName;
        public double EarnCurrencyAmount;
        public int TechTreeFirstUnlockCost;
        public int CostToResearchAmount;
        public float ResearchDuration;
        public float ResearchSuccessChance;
        public string SetPermission;
        [JsonIgnore]
        public string PrefixPermission { get; set; }
    }

    public class ResearchTableUI
    {
        public string CostColor;
        public string CurrencyColor;
    }

    public class TechTreeUI
    {
        public string CostColor;
    }

    private string EasyResearchLang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
    private void PopupMessage(IPlayer player, string message);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void Loaded();
    private void OnServerInitialized();
    private void Unload();
    private void OnEntitySpawned(ResearchTable table);
    private void OnLootEntity(BasePlayer player, ResearchTable table);
    private void OnLootEntityEnd(BasePlayer player, ResearchTable entity);
    private void OnEntityEnter(TriggerWorkbench trigger, BasePlayer player);
    private void OnEntityLeave(TriggerWorkbench trigger, BasePlayer player);
    private Dictionary<Rarity, int> _rarity;
    private object OnResearchCostDetermine(Item item, ResearchTable researchTable);
    private void OnItemAddedToContainer(ItemContainer container, Item item);
    private void OnItemRemovedFromContainer(ItemContainer itemContainer, Item item, ResearchTable researchTable);
    private object CanResearchItem(BasePlayer player, Item targetItem);
    private void OnItemResearch(ResearchTable table, Item targetItem, BasePlayer player);
    private float OnItemResearched(ResearchTable table, float chance);
    private bool CheckUnlockPath(BasePlayer player, TechTreeData.NodeInstance node, TechTreeData techTree);
    private object CanUnlockTechTreeNode(BasePlayer player, TechTreeData.NodeInstance node, TechTreeData techTree);
    private object OnTechTreeNodeUnlock(Workbench workbench, TechTreeData.NodeInstance node, BasePlayer player);
    private void UpdateResearchTables(string shortname);
    public Research FindResearch(string item);
    public int PriceRounder(double amount);
    private void AddCurrency(BasePlayer player, double amount);
    private bool GiveCurrency(BasePlayer player, string shortname);
    public class Rectangle
    {
        public decimal Height;
        public decimal Width;
        public decimal X;
        public decimal Y;
        public Rectangle();
        public Rectangle(decimal x, decimal y, decimal width, decimal height);
        public string GetMinAnchor();
        public string GetMaxAnchor();
        public string[] ToAnchors();
    }

    public void PanelUI1(CuiElementContainer container, string parent, string name, string min, string max, string offmin, string offmax, string bgColor, bool curser);
    public void PanelUI2(CuiElementContainer container, string parent, string name, string min, string max, string bgColor, bool curser);
    public void LabelUI1(CuiElementContainer container, string parent, string name, Rectangle rectangle, string textColor, string text, int fontSize, TextAnchor textAnchor, bool fontbold);
    public void LableUI2(CuiElementContainer container, string parent, string name, string textColor, string text, int fontSize, string anchorMin, string anchorMax, TextAnchor align, float fadeIn);
    private void DestroyUI(BasePlayer player);
    public class Color
    {
        public string HexColor;
        public float Alpha;
        public Color(string hexColor, float alpha);
        public static string ToRGB(string hexColor, float alpha);
        public string ToRGB();
    }

}

private class PluginConfig
{
    [JsonProperty("Chat Prefix")]
    public string ChatPrefix;
    [JsonProperty("Research Requirement this is the type of resource used to research items (Expects Item Shortname)")]
    public string Requirement;
    [JsonProperty("Already Researched Toggle")]
    public bool Researched;
    [JsonProperty("Block Tech Tree Researching")]
    public bool BlockTechTree;
    [JsonProperty("Use Popup Notifications")]
    public bool Popup;
    [JsonProperty("Use Notify Notifications (Mevents Version CodeFling)")]
    public bool Notify;
    [JsonProperty("Notify Notification Type")]
    public int NotifyType;
    [JsonProperty("Use CustomUI Overlay Notifications")]
    public bool CustomUI;
    [JsonProperty("Enable Economics")]
    public bool Economics;
    [JsonProperty("Enable ServerRewards")]
    public bool ServerRewards;
    [JsonProperty("Enable Custom Currency")]
    public bool Custom;
    [JsonProperty("Custom Name")]
    public string CName;
    [JsonProperty("Custom Item ID")]
    public int CId;
    [JsonProperty("Custom SkinId")]
    public ulong CSkinId;
    [JsonProperty("Research Table UI Options")]
    public ResearchTableUI ResearchTableUI;
    [JsonProperty("TechTree UI Options")]
    public TechTreeUI TechTreeUI;
    [JsonProperty("Blocked Items")]
    public List<string> Blocked;
    [JsonProperty("Custom Research Options")]
    public Dictionary<string, Research> ResearchSettings;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

public class Research
{
    public string DisplayName;
    public double EarnCurrencyAmount;
    public int TechTreeFirstUnlockCost;
    public int CostToResearchAmount;
    public float ResearchDuration;
    public float ResearchSuccessChance;
    public string SetPermission;
    [JsonIgnore]
    public string PrefixPermission { get; set; }
}

public class ResearchTableUI
{
    public string CostColor;
    public string CurrencyColor;
}

public class TechTreeUI
{
    public string CostColor;
}

public class Rectangle
{
    public decimal Height;
    public decimal Width;
    public decimal X;
    public decimal Y;
    public Rectangle();
    public Rectangle(decimal x, decimal y, decimal width, decimal height);
    public string GetMinAnchor();
    public string GetMaxAnchor();
    public string[] ToAnchors();
}

public class Color
{
    public string HexColor;
    public float Alpha;
    public Color(string hexColor, float alpha);
    public static string ToRGB(string hexColor, float alpha);
    public string ToRGB();
}

public static class PlayerEx
{
    public static IPlayer GetPlayer(BasePlayer player);
}

EasyResearchEx
public static class PlayerEx
{
    public static IPlayer GetPlayer(BasePlayer player);
}


```

---

## EasyVote by MikeHawke - Most customizable vote plugin for Rust servers

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using UnityEngine;
using System.Linq;
using System.Text;
using Oxide.Core.Libraries;
using System.Text.RegularExpressions;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("EasyVote", "Exel80", "2.0.43")]
[Description("Simple and smooth voting start by activating one scirpt.")]
 class EasyVote : RustPlugin
{
    [PluginReference]
    private Plugin DiscordMessages;
    private const bool DEBUG;
    private const string permUse;
    protected string[] voteStatus;
    protected string[] voteStatusColor;
     Dictionary<ulong, StringBuilder> claimCooldown;
     Dictionary<ulong, bool> checkCooldown;
     StringBuilder rewardsString;
     List<string> availableAPISites;
     StringBuilder _voteList;
     StringBuilder helpYou;
    private List<int> numberMax;
     void Loaded();
     string _lang(string key, string id, object[] args);
    private void LoadMessages();
     void OnPlayerDisconnected(BasePlayer player, string reason);
    private void SendHelpText(BasePlayer player);
     void OnPlayerSleepEnded(BasePlayer player);
    [ChatCommand("vote")]
     void cmdVote(BasePlayer player, string command, string[] args);
    [ChatCommand("claim")]
     void cmdClaim(BasePlayer player, string command, string[] args);
    [ChatCommand("rewardlist")]
     void cmdReward(BasePlayer player, string command, string[] args);
    private void RewardHandler(BasePlayer player, string serverName);
    private void GaveRewards(BasePlayer player, List<string> rewardValue);
    private string getCmdLine(BasePlayer player, string str, string value);
     class StoredData
    {
        public Dictionary<string, PlayerData> Players;
        public StoredData();
    }

     class PlayerData
    {
        public int voted;
        public DateTime lastTime_Voted;
        public PlayerData();
    }

     StoredData _storedData;
     void ClaimReward(int code, string response, BasePlayer player, string url, string serverName);
     void CheckStatus(int code, string response, BasePlayer player);
     PluginConfig DefaultConfig();
    private bool configChanged;
    private PluginConfig _config;
    protected override void LoadDefaultConfig();
     class PluginSettings
    {
        public const string apiClaim;
        public const string apiStatus;
        public const string apiLink;
        public const string Title;
        public const string WebhookURL;
        public const string DiscordEnabled;
        public const string Alert;
        public const string Prefix;
        public const string LogEnabled;
        public const string RewardIsCumulative;
        public const string GlobalChatAnnouncments;
        public const string LocalChatAnnouncments;
    }

     class PluginConfig
    {
        public Dictionary<string, string> Settings { get; set; }
        public Dictionary<string, string> Discord { get; set; }
        public Dictionary<string, Dictionary<string, string>> Servers { get; set; }
        public Dictionary<string, Dictionary<string, string>> VoteSitesAPI { get; set; }
        public Dictionary<string, List<string>> Rewards { get; set; }
        public Dictionary<string, string> Commands { get; set; }
    }

     void LoadConfigValues();
     void Merge(IDictionary<T1, T2> current, IDictionary<T1, T2> defaultDict, bool bypass);
    public void Chat(BasePlayer player, string str, bool prefix);
    public void _Debug(string msg);
    private void HelpText();
    private string CleanHTML(string input);
    private bool hasPermission(BasePlayer player, string perm);
    public class Fields
    {
        public string name { get; set; }
        public string value { get; set; }
        public bool inline { get; set; }
        public Fields(string name, string value, bool inline);
    }

    private void rewardList(BasePlayer player);
    private void BuildNumberMax();
    private void voteList();
    private void checkVoteSites();
    private string getHighestvoter();
    private string getLastvoter();
    private void resetPlayerVotedData(string steamID, bool displayMessage);
    private void resetData(bool backup);
}

 class StoredData
{
    public Dictionary<string, PlayerData> Players;
    public StoredData();
}

 class PlayerData
{
    public int voted;
    public DateTime lastTime_Voted;
    public PlayerData();
}

 class PluginSettings
{
    public const string apiClaim;
    public const string apiStatus;
    public const string apiLink;
    public const string Title;
    public const string WebhookURL;
    public const string DiscordEnabled;
    public const string Alert;
    public const string Prefix;
    public const string LogEnabled;
    public const string RewardIsCumulative;
    public const string GlobalChatAnnouncments;
    public const string LocalChatAnnouncments;
}

 class PluginConfig
{
    public Dictionary<string, string> Settings { get; set; }
    public Dictionary<string, string> Discord { get; set; }
    public Dictionary<string, Dictionary<string, string>> Servers { get; set; }
    public Dictionary<string, Dictionary<string, string>> VoteSitesAPI { get; set; }
    public Dictionary<string, List<string>> Rewards { get; set; }
    public Dictionary<string, string> Commands { get; set; }
}

public class Fields
{
    public string name { get; set; }
    public string value { get; set; }
    public bool inline { get; set; }
    public Fields(string name, string value, bool inline);
}


```

---

## EasyVoteHighestvoter by  - Monthly check for the highest voter and reward them

```csharp
using Facepunch.Extend;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using UnityEngine;

Oxide.Plugins
[Info("EasyVote-HighestVoter", "Exel80", "1.0.2", ResourceId = 2671)]
 class EasyVoteHighestvoter : RustPlugin
{
    [PluginReference]
    private Plugin EasyVote;
    private const string StoredDataName;
     void onUserReceiveHighestVoterReward(Dictionary<string, object> RewardData);
     void OnPlayerSleepEnded(BasePlayer player);
     void Init();
    private void nextMonth();
    private void GaveRewards(string HighestPlayer);
    private void GaveGroup(string HighestPlayer, string OldHighestPlayer);
    private Dictionary<string, object> GaveItems(Dictionary<string, object> RewardData, BasePlayer player);
     string _lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
     class StoredData
    {
        public int Month;
        public string highestVoterID;
        public bool hasReceivedReward;
        public StoredData();
    }

     StoredData _storedData;
    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Enable logging, save to oxide/logs/EasyVoteHighestvoter (true / false)")]
        public bool logEnabled;
        [JsonProperty(PropertyName = "Interval timer (seconds)")]
        public int checkTime;
        [JsonProperty(PropertyName = "Highest voter reward (item, group or both)")]
        public string rewardIs;
        [JsonProperty(PropertyName = "Highest voter reward group (group name)")]
        public string group;
        [JsonProperty(PropertyName = "Highest voter reward item(s) (Item.Shortname:Amount => http://docs.oxidemod.org/rust/#item-list)")]
        public string item;
        public static Configuration DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Announce();
    private void Congrats(string name, string id);
    private static HashSet<BasePlayer> FindPlayer(string nameOrIdOrIp);
}

 class StoredData
{
    public int Month;
    public string highestVoterID;
    public bool hasReceivedReward;
    public StoredData();
}

public class Configuration
{
    [JsonProperty(PropertyName = "Enable logging, save to oxide/logs/EasyVoteHighestvoter (true / false)")]
    public bool logEnabled;
    [JsonProperty(PropertyName = "Interval timer (seconds)")]
    public int checkTime;
    [JsonProperty(PropertyName = "Highest voter reward (item, group or both)")]
    public string rewardIs;
    [JsonProperty(PropertyName = "Highest voter reward group (group name)")]
    public string group;
    [JsonProperty(PropertyName = "Highest voter reward item(s) (Item.Shortname:Amount => http://docs.oxidemod.org/rust/#item-list)")]
    public string item;
    public static Configuration DefaultConfig();
}


```

---

## Economics by MrBlue - Basic economics system and economy API

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Economics", "Wulf", "3.9.2")]
[Description("Basic economics system and economy API")]
public class Economics : CovalencePlugin
{
    private Configuration config;
    private class Configuration
    {
        [JsonProperty("Allow negative balance for accounts")]
        public bool AllowNegativeBalance;
        [JsonProperty("Balance limit for accounts (0 to disable)")]
        public int BalanceLimit;
        [JsonProperty("Maximum balance for accounts (0 to disable)")]
        private int BalanceLimitOld { get; set; }
        [JsonProperty("Negative balance limit for accounts (0 to disable)")]
        public int NegativeBalanceLimit;
        [JsonProperty("Remove unused accounts")]
        public bool RemoveUnused;
        [JsonProperty("Log transactions to file")]
        public bool LogTransactions;
        [JsonProperty("Starting account balance (0 or higher)")]
        public int StartingBalance;
        [JsonProperty("Starting money amount (0 or higher)")]
        private int StartingBalanceOld { get; set; }
        [JsonProperty("Wipe balances on new save file")]
        public bool WipeOnNewSave;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private DynamicConfigFile data;
    private StoredData storedData;
    private bool changed;
    private class StoredData
    {
        public readonly Dictionary<string, double> Balances;
    }

    private void SaveData();
    private void OnServerSave();
    private void Unload();
    protected override void LoadDefaultMessages();
    private const string permissionBalance;
    private const string permissionDeposit;
    private const string permissionDepositAll;
    private const string permissionSetBalance;
    private const string permissionSetBalanceAll;
    private const string permissionTransfer;
    private const string permissionTransferAll;
    private const string permissionWithdraw;
    private const string permissionWithdrawAll;
    private const string permissionWipe;
    private void Init();
    private void OnNewSave();
    private double Balance(string playerId);
    private double Balance(object playerId);
    private string GetUserId(object playerId);
    private bool Deposit(string playerId, double amount);
    private bool Deposit(object playerId, double amount);
    private bool SetBalance(string playerId, double amount);
    private bool SetBalance(object playerId, double amount);
    private bool Transfer(string playerId, string targetId, double amount);
    private bool Transfer(object playerId, ulong targetId, double amount);
    private bool Withdraw(string playerId, double amount);
    private bool Withdraw(object playerId, double amount);
    private void CommandBalance(IPlayer player, string command, string[] args);
    private void CommandDeposit(IPlayer player, string command, string[] args);
    private void CommandSetBalance(IPlayer player, string command, string[] args);
    private void CommandTransfer(IPlayer player, string command, string[] args);
    private void CommandWithdraw(IPlayer player, string command, string[] args);
    private void CommandWipe(IPlayer player, string command, string[] args);
    private IPlayer FindPlayer(string playerNameOrId, IPlayer player);
    private void AddLocalizedCommand(string command);
    private string GetLang(string langKey, string playerId, object[] args);
    private void Message(IPlayer player, string textOrLang, object[] args);
}

private class Configuration
{
    [JsonProperty("Allow negative balance for accounts")]
    public bool AllowNegativeBalance;
    [JsonProperty("Balance limit for accounts (0 to disable)")]
    public int BalanceLimit;
    [JsonProperty("Maximum balance for accounts (0 to disable)")]
    private int BalanceLimitOld { get; set; }
    [JsonProperty("Negative balance limit for accounts (0 to disable)")]
    public int NegativeBalanceLimit;
    [JsonProperty("Remove unused accounts")]
    public bool RemoveUnused;
    [JsonProperty("Log transactions to file")]
    public bool LogTransactions;
    [JsonProperty("Starting account balance (0 or higher)")]
    public int StartingBalance;
    [JsonProperty("Starting money amount (0 or higher)")]
    private int StartingBalanceOld { get; set; }
    [JsonProperty("Wipe balances on new save file")]
    public bool WipeOnNewSave;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private class StoredData
{
    public readonly Dictionary<string, double> Balances;
}

Oxide.Plugins.EconomicsExtensionMethods
public static class ExtensionMethods
{
    public static T Clamp(T val, T min, T max);
}


```

---

## EconomicsBalanceGUI by misticos - Displays the player's Economics balance on the UI

```csharp
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Economics Balance GUI", "lethal_d0se & misticos", "1.1.4")]
[Description("Displays a players economics balance on the HUD.")]
public class EconomicsBalanceGUI : RustPlugin
{
    [PluginReference]
    private Plugin Economics;
    private List<ulong> Looters;
    private Dictionary<ulong, double> Balances;
    private string GUIColor { get; set; }
    private string GUIAnchorMin { get; set; }
    private string GUIAnchorMax { get; set; }
    private string GUICurrency { get; set; }
    private string GUICurrencySize { get; set; }
    private string GUICurrencyColor { get; set; }
    private string GUIBalanceSize { get; set; }
    private string GUIBalanceColor { get; set; }
    private void OnServerInitialized();
    private void Unload();
    protected override void LoadDefaultConfig();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerSleepEnded(BasePlayer player);
    private void OnPlayerLootEnd(PlayerLoot inventory);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnLootEntity(BasePlayer looter, BaseEntity target);
    private void OnLootPlayer(BasePlayer looter, BasePlayer beingLooter);
    private void OnLootItem(BasePlayer looter, Item lootedItem);
    private void GUICreate(BasePlayer player);
    private void GUIDestroy(BasePlayer player);
    private void GUIRefresh(BasePlayer player);
    private string GetFormattedMoney(BasePlayer player);
    private T GetConfig(string name, T defaultValue);
}


```

---

## EditTool by JakeRich - Provides a GMod type tool to edit your environment

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.IO;
using System.IO.Compression;
using System.Linq;
using System.Reflection;
using System.Text;
using Network;
using Newtonsoft.Json;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using ProtoBuf;
using UnityEngine;

Oxide.Plugins
[Info("Edit Tool", "JakeRich", "1.0.5")]
[Description("Modify entities in a map")]
public class EditTool : RustPlugin
{
    public static ulong ToolSkinID;
    public static string ToolItemShortname;
    public static string ToolName;
    public static EditTool _plugin;
    public static float EditToolDistance;
    public static UIManager UI;
    public static PlayerDataController<ToolPlayerData> PlayerData;
    private const string permUse;
    private void Init();
    private void Unload();
    private void OnPlayerInput(BasePlayer player, InputState input);
    private void OnEntityKill(BaseNetworkable entity);
    private void NotLookingAtEntity(BasePlayer player);
    private void LookingAtEntity(BasePlayer player, BaseEntity entity);
    public static bool IsEditTool(Item item);
    public static bool HasEditTool(BasePlayer player);
    public static int EditToolLayer;
    public static bool GetEntityLookingAt(BasePlayer player, BaseEntity entity, Vector3 hitPoint, float distance, bool announceOnNotFound);
    [ChatCommand("edittool")]
    private void GiveEditTool_Command(BasePlayer player, string command, string[] args);
    [ConsoleCommand("edittool.switchmode")]
    private void SwitchEditToolMode_ConsoleCommand(ConsoleSystem.Arg args);
    public void GiveEditTool(BasePlayer player);
    public class ToolPlayerData : PlayerDataBase
    {
        public ToolMode Mode;
        private BaseEntity selectedEntity;
        private Vector3 lastTransformPosition;
        private Vector3 transformOffset;
        private float TranslateDistance;
        private EntitySaver _entitySaver;
        private Quaternion _lastRotation;
        public void SwitchMode();
        public void OnLeaveMode(ToolMode mode);
        public void OnEnterMode(ToolMode mode);
        public void OnShiftRightClick(BaseEntity entity, Vector3 hitPoint);
        public void OnShiftLeftClick(BaseEntity entity, Vector3 hitPoint);
        public void OnLeftClick(BaseEntity entity, Vector3 hitPoint);
        public void OnLeftRelease();
        public void OnRightRelease();
        public void OnRightClick(BaseEntity entity, Vector3 hitPoint);
        public void OnReloadClick(BaseEntity entity, Vector3 hitPoint);
        public void OnPlayerInput(BaseEntity entity, InputState input);
        private void SelectEntity(BaseEntity entity);
        private void DeselectEntity();
        private void TryUpdateBuildingBlock(BaseEntity entity);
        private void TransformTick();
        private void RotationTick(InputState input);
        private void SavePrefabName(BaseEntity entity, bool fullClone);
        private void SpawnPrefab(Vector3 position, bool fullClone);
    }

    public class EntitySaver
    {
        private ulong _ownerID;
        private string _prefabName;
        private Vector3 lastPos;
        private Quaternion _rotation;
        private byte[] savedBytes;
        private BuildingGrade.Enum _grade;
        private Vector3 velocity;
        public void SaveEntity(BaseEntity entity, bool fullClone);
        public void SpawnEntity(Vector3 position, bool fullClone);
        private void SetupEntityIDs(BaseEntity entity);
        private void SetupItemContainerIDs(ItemContainer container);
        private void SetupItem(Item item);
    }

    public class UIManager
    {
        private UIImage Crosshair;
        public UILabel EntityInfoLabel;
        public UIBaseElement ToolUI;
        private UILabel ToolMode;
        public UIManager();
        public void Unload();
        private void SetupUI_Crosshair();
        private void SetupUI_ToolUI();
        public void SwitchToolMode(BasePlayer player, ToolMode mode);
    }

    public Dictionary<string, string> lang_en;
    public static string GetLangMessage(string key, BasePlayer player);
    public static string GetLangMessage(string key, ulong player);
    public static string GetLangMessage(string key, string player);
    [PluginReference]
    private RustPlugin EntityProperties;
    public static bool EntityPropertiesLoaded();
    public static void StartEditingEntityData(BasePlayer player, BaseEntity entity);
    public static void StopEditingEntityData(BasePlayer player);
    public static void CloneEntityData(BaseEntity source, BaseEntity target);
    [PluginReference]
    private RustPlugin Disguise;
    public static bool DisguiseLoaded();
    public static bool Debugging;
    public static void Debug(string format, object[] args);
    public static void Debug(object obj);
    public static void Error(string format, object[] args);
    public class PlayerDataBase
    {
        [JsonIgnore]
        public BasePlayer Player { get; set; }
        public string UserID { get; set; }
        public PlayerDataBase();
        public PlayerDataBase(BasePlayer player);
    }

    public class PlayerDataController
    {
        [JsonProperty(Required = Required.Always)]
        private Dictionary<string, T> playerData { get; set; }
        public T Get(string identifer);
        public T Get(ulong userID);
        public T Get(BasePlayer player);
    }

    public class JSONSpawnPoint
    {
        public float xPos { get; set; }
        public float yPos { get; set; }
        public float zPos { get; set; }
        public float xRot { get; set; }
        public float yRot { get; set; }
        public float zRot { get; set; }
        public float wRot { get; set; }
        public BasePlayer.SpawnPoint ToSpawnPoint();
        [JsonIgnore]
        public Vector3 Position { get; set; }
        [JsonIgnore]
        public Quaternion Rotation { get; set; }
        public JSONSpawnPoint();
        public JSONSpawnPoint(Vector3 position, Quaternion rot);
    }

    public class JSONEntity
    {
        public float xPos { get; set; }
        public float yPos { get; set; }
        public float zPos { get; set; }
        public float xRot { get; set; }
        public float yRot { get; set; }
        public float zRot { get; set; }
        public float wRot { get; set; }
        public string prefabName { get; set; }
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
        public BuildingGrade.Enum grade { get; set; }
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
        public int AssignedBaseID;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
        public ulong OwnerID;
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public JSONStorageContainer Inventory;
        [JsonIgnore]
        public Vector3 Position { get; set; }
        [JsonIgnore]
        public Quaternion Rotation { get; set; }
        [JsonIgnore]
        public BaseEntity Entity;
        public JSONEntity(BaseEntity entity);
        public JSONEntity(BaseEntity entity, Vector3 position, Quaternion rotation);
        public JSONEntity();
        private BaseEntity CreateEntity(bool floating);
        public void PreSpawn(BaseEntity entity);
        public void PostSpawn(BaseEntity entity);
        public BaseEntity Spawn();
        public bool IsEntity(BaseEntity entity);
    }

    public class JSONStorageContainer
    {
        public int MaxRefills;
        public bool Locked;
        public bool PerPlayer;
        public JSONItemContainer SavedInventory;
    }

    public class JSONItemAmount
    {
        public string shortname;
        public int amount;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
        public ulong skinID;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
        public int slot;
        public JSONItemAmount();
        public JSONItemAmount(string shortname, int amount, ulong skinID);
        public JSONItemAmount(ItemAmount itemAmount);
        public JSONItemAmount(Item item, int slot);
        public int ItemID();
        public Item CreateItem();
        public void GiveToPlayer(BasePlayer player);
        public void AddToContainer(ItemContainer container);
    }

    public class JSONItemContainer
    {
        public List<JSONItemAmount> Items;
        public JSONItemContainer();
        public JSONItemContainer(ItemContainer container);
        public void Load(ItemContainer container);
    }

    public class JSONFile
    {
        private DynamicConfigFile _file;
        public string _name { get; set; }
        public Type Instance { get; set; }
        private ConfigLocation _location { get; set; }
        private string _path { get; set; }
        public bool SaveOnUnload;
        public bool Compressed;
        public JSONFile(string name, ConfigLocation location, string path, string extension, bool saveOnUnload);
        public virtual void Init();
        public virtual void Load();
        private void LoadCompressed();
        public virtual void Save();
        private void SaveCompressed();
        public virtual void Reload();
        private void Unload(Plugin sender, PluginManager manager);
    }

    private Dictionary<string, UICallbackComponent> UIButtonCallBacks { get; set; }
    private void OnButtonClick(ConsoleSystem.Arg arg);
    public class UIElement : UIBaseElement
    {
        public CuiElement Element { get; set; }
        public UIOutline Outline { get; set; }
        public CuiRectTransformComponent transform { get; set; }
        public float FadeOut { get; set; }
        private float _fadeOut;
        public string Name { get; set; }
        public UIElement(UIBaseElement parent);
        public UIElement(Vector2 position, float width, float height, UIBaseElement parent);
        public UIElement(Vector2 min, Vector2 max, UIBaseElement parent);
        public void AddOutline(string color, string distance);
        public virtual void Init();
        public override void Show(BasePlayer player, bool children);
        public override void Hide(BasePlayer player, bool children);
        public override void UpdatePlacement();
        public void SetPositionAndSize(CuiRectTransformComponent trans);
        public void SetParent(UIElement element);
        public void SetParent(string parent);
    }

    public class UIButton : UIElement, UICallbackComponent
    {
        public CuiButtonComponent buttonComponent { get; set; }
        public CuiTextComponent textComponent { get; set; }
        public UILabel Label { get; set; }
        private string _textColor { get; set; }
        private string _buttonText { get; set; }
        public string Text { get; set; }
        public Func<BasePlayer, string> variableText { get; set; }
        public Action<ConsoleSystem.Arg> onCallback;
        private int _fontSize;
        public UIButton(Vector2 min, Vector2 max, string buttonText, string buttonColor, string textColor, int fontSize, UIBaseElement parent);
        public UIButton(Vector2 position, float width, float height, string buttonText, string buttonColor, string textColor, int fontSize, UIBaseElement parent);
        public override void Init();
        public void AddChatCommand(string fullCommand);
        public void AddCallback(Action<BasePlayer> callback);
        public override void Show(BasePlayer player, bool children);
        public void InvokeCallback(ConsoleSystem.Arg args);
    }

    public class UIBackgroundText : UIPanel
    {
        public UILabel Label;
        public UIBackgroundText(Vector2 min, Vector2 max, UIBaseElement parent, string backgroundColor, string labelText, int fontSize, string fontColor, TextAnchor alignment);
    }

    public class UILabel : UIElement
    {
        public CuiTextComponent text { get; set; }
        public UILabel(Vector2 min, Vector2 max, string labelText, int fontSize, string fontColor, UIBaseElement parent, TextAnchor alignment);
        public UILabel(Vector2 min, float width, float height, string labelText, int fontSize, string fontColor, UIBaseElement parent, TextAnchor alignment);
        public string Text { get; set; }
        public TextAnchor Allign { get; set; }
        public Color Color { get; set; }
        public string ColorString { get; set; }
        public Func<BasePlayer, string> variableText { get; set; }
        public Func<BasePlayer, string> variableFontColor { get; set; }
        public override void Show(BasePlayer player, bool children);
        public override void Init();
    }

    public class UIImageBase : UIElement
    {
        public UIImageBase(Vector2 min, Vector2 max, UIBaseElement parent);
        private CuiNeedsCursorComponent needsCursor { get; set; }
        private bool requiresFocus { get; set; }
        public bool CursorEnabled { get; set; }
    }

    public class UIPanel : UIImageBase
    {
        private CuiImageComponent panel;
        public string Color { get; set; }
        public string Material { get; set; }
        public Func<BasePlayer, string> variableColor { get; set; }
        public UIPanel(Vector2 min, Vector2 max, UIBaseElement parent, string color);
        public UIPanel(Vector2 position, float width, float height, UIBaseElement parent, string color);
        public override void Show(BasePlayer player, bool children);
    }

    public class UIButtonContainer : UIPanel
    {
        private float _padding;
        private float _buttonHeight;
        private string _buttonColor;
        private List<UIButton> buttons;
        public UIButtonContainer(Vector2 min, Vector2 max, UIBaseElement parent, string bgColor, float buttonHeight, float padding, string buttonColor);
        public UIButton AddButton(string text, int fontSize, string textColor, string buttonColor, Action<BasePlayer> callback);
    }

    public class UIPagedElements : UIBaseElement
    {
        private UIButton nextPage { get; set; }
        private UIButton prevPage { get; set; }
        private float _elementHeight { get; set; }
        private float _elementSpacing { get; set; }
        private int _elementWidth { get; set; }
        private Dictionary<BasePlayer, int> ElementIndex;
        private List<UIBaseElement> Elements;
        public UIPagedElements(Vector2 min, Vector2 max, float elementHeight, float elementSpacing, UIBaseElement parent, int elementWidth);
        public void NewElement(UIBaseElement element);
        public void NewElements(IEnumerable<UIBaseElement> elements);
        public override void Show(BasePlayer player, bool showChildren);
        public override void Hide(BasePlayer player, bool hideChildren);
    }

    public class UIButtonConfiguration
    {
        public string ButtonName { get; set; }
        public string ButtonCommand { get; set; }
        public string ButtonColor { get; set; }
        public Action<BasePlayer> callback { get; set; }
    }

    public class UIImage : UIImageBase
    {
        public CuiImageComponent Image { get; set; }
        public string Sprite { get; set; }
        public string Material { get; set; }
        public string PNG { get; set; }
        public UIImage(Vector2 min, Vector2 max, UIBaseElement parent);
        public UIImage(Vector2 position, float width, float height, UIBaseElement parent);
        public Func<BasePlayer, string> variableSprite { get; set; }
        public Func<BasePlayer, string> variablePNG { get; set; }
        public override void Show(BasePlayer player, bool children);
    }

    public class UIRawImage : UIImageBase
    {
        public CuiRawImageComponent Image { get; set; }
        public string Material { get; set; }
        public string Sprite { get; set; }
        public string PNG { get; set; }
        public string Color { get; set; }
        public UIRawImage(Vector2 position, float width, float height, UIBaseElement parent, string url);
        public UIRawImage(Vector2 min, Vector2 max, UIBaseElement parent, string url);
        public Func<BasePlayer, string> variablePNG { get; set; }
        public Func<BasePlayer, string> variableURL { get; set; }
        public Func<BasePlayer, string> variablePNGURL { get; set; }
        public override void Show(BasePlayer player, bool children);
    }

    public class UIBaseElement
    {
        public Vector2 localPosition { get; set; }
        public Vector2 localSize { get; set; }
        public Vector2 globalSize { get; set; }
        public Vector2 globalPosition { get; set; }
        public HashSet<BasePlayer> players { get; set; }
        public UIBaseElement _parent { get; set; }
        public HashSet<UIBaseElement> children { get; set; }
        public Vector2 min { get; set; }
        public Vector2 max { get; set; }
        public string Parent { get; set; }
        public bool _shouldShow;
        public Func<BasePlayer, bool> conditionalShow { get; set; }
        public Func<BasePlayer, Vector2> conditionalSize { get; set; }
        public Func<BasePlayer, Vector2> conditionalPosition { get; set; }
        public UIBaseElement(UIBaseElement parent);
        public UIBaseElement(Vector2 min, Vector2 max, UIBaseElement parent);
        public UIBaseElement(Vector2 min, float width, float height, UIBaseElement parent);
        public void AddElement(UIBaseElement element);
        public void RemoveElement(UIBaseElement element);
        public void Refresh(BasePlayer player);
        public bool AddPlayer(BasePlayer player);
        public bool RemovePlayer(BasePlayer player);
        public void Show(IEnumerable<BasePlayer> players);
        public virtual void SetParent(UIBaseElement parent);
        public virtual void Hide(BasePlayer player, bool hideChildren);
        public void Hide(IEnumerable<BasePlayer> players);
        public virtual bool Toggle(BasePlayer player);
        public virtual void Show(BasePlayer player, bool showChildren);
        public bool CanShow(BasePlayer player);
        public void HideAll();
        public void RefreshAll();
        public void SafeAddUi(BasePlayer player, CuiElement element);
        public void SafeDestroyUi(BasePlayer player, CuiElement element);
        public void SetSize(float x, float y);
        public void SetPosition(float x, float y);
        public virtual void UpdatePlacement();
    }

    public class UIReflectionElement : UIPanel
    {
        private object config { get; set; }
        private object _target { get; set; }
        private FieldInfo Field { get; set; }
        private UILabel Text { get; set; }
        private UIInputField InputField { get; set; }
        private UIButton editButton { get; set; }
        private bool EditBox { get; set; }
        public UIReflectionElement(Vector2 min, Vector2 max, FieldInfo field, object configuration, UIBaseElement parent);
        public override void Show(BasePlayer player, bool children);
        public string GetVisualText();
        public void AssignValue(string text);
    }

    public class UIGridDisplay
    {
        public UIGridDisplay(Vector2 min, Vector2 max, int width, int height, float paddingX, float paddingY);
    }

    public static bool IsValueType(object obj);
    public class ObjectMemoryInfo
    {
        public string name { get; set; }
        public int memoryUsed { get; set; }
        public int elements { get; set; }
        public object _target { get; set; }
        private int currentLayer { get; set; }
        public ObjectMemoryInfo _parent { get; set; }
        private bool _autoExpand { get; set; }
        public List<ObjectMemoryInfo> children { get; set; }
        public List<MethodInfo> methods { get; set; }
        public ObjectMemoryInfo(object targetObject, int layers, string variableName, ObjectMemoryInfo parent, bool autoExpand);
        public void Expand();
        public void SetupObject();
        private int GetCount();
        public string GetInfo();
        public string GetVisualText();
        public static string GetMethodText(MethodInfo info);
        private static string GetTypeName(Type type);
        private string GetMemoryUsage();
        public void CalculateSubObjects();
        public bool CheckParents();
        public List<string> GetOutput(int layer, bool justLists);
        public string PrintOutput(bool justLists);
    }

    public class UICheckbox : UIButton
    {
        public UICheckbox(Vector2 min, Vector2 max, UIBaseElement parent);
    }

    public class UIOutline
    {
        public CuiOutlineComponent component;
        public string Color { get; set; }
        public string Distance { get; set; }
        private string _color;
        private string _distance;
        public UIOutline();
        public UIOutline(string color, string distance);
        private void UpdateComponent();
    }

    public class UIInputField : UIPanel, UICallbackComponent
    {
        public CuiInputFieldComponent InputField { get; set; }
        public Action<ConsoleSystem.Arg> onCallback;
        public UIInputField(Vector2 min, Vector2 max, UIBaseElement parent, string defaultText, TextAnchor align, int fontSize, string panelColor, string textColor, bool password, int charLimit);
        public void AddCallback(Action<BasePlayer, string> callback);
        public void InvokeCallback(ConsoleSystem.Arg args);
    }

    public class UIInput_Raw : UIElement
    {
        public CuiInputFieldComponent InputField { get; set; }
        public UIInput_Raw(Vector2 min, Vector2 max, UIBaseElement parent, string defaultText, TextAnchor align, int fontSize, string textColor, bool password, int charLimit);
    }

}

public class ToolPlayerData : PlayerDataBase
{
    public ToolMode Mode;
    private BaseEntity selectedEntity;
    private Vector3 lastTransformPosition;
    private Vector3 transformOffset;
    private float TranslateDistance;
    private EntitySaver _entitySaver;
    private Quaternion _lastRotation;
    public void SwitchMode();
    public void OnLeaveMode(ToolMode mode);
    public void OnEnterMode(ToolMode mode);
    public void OnShiftRightClick(BaseEntity entity, Vector3 hitPoint);
    public void OnShiftLeftClick(BaseEntity entity, Vector3 hitPoint);
    public void OnLeftClick(BaseEntity entity, Vector3 hitPoint);
    public void OnLeftRelease();
    public void OnRightRelease();
    public void OnRightClick(BaseEntity entity, Vector3 hitPoint);
    public void OnReloadClick(BaseEntity entity, Vector3 hitPoint);
    public void OnPlayerInput(BaseEntity entity, InputState input);
    private void SelectEntity(BaseEntity entity);
    private void DeselectEntity();
    private void TryUpdateBuildingBlock(BaseEntity entity);
    private void TransformTick();
    private void RotationTick(InputState input);
    private void SavePrefabName(BaseEntity entity, bool fullClone);
    private void SpawnPrefab(Vector3 position, bool fullClone);
}

public class EntitySaver
{
    private ulong _ownerID;
    private string _prefabName;
    private Vector3 lastPos;
    private Quaternion _rotation;
    private byte[] savedBytes;
    private BuildingGrade.Enum _grade;
    private Vector3 velocity;
    public void SaveEntity(BaseEntity entity, bool fullClone);
    public void SpawnEntity(Vector3 position, bool fullClone);
    private void SetupEntityIDs(BaseEntity entity);
    private void SetupItemContainerIDs(ItemContainer container);
    private void SetupItem(Item item);
}

public class UIManager
{
    private UIImage Crosshair;
    public UILabel EntityInfoLabel;
    public UIBaseElement ToolUI;
    private UILabel ToolMode;
    public UIManager();
    public void Unload();
    private void SetupUI_Crosshair();
    private void SetupUI_ToolUI();
    public void SwitchToolMode(BasePlayer player, ToolMode mode);
}

public class PlayerDataBase
{
    [JsonIgnore]
    public BasePlayer Player { get; set; }
    public string UserID { get; set; }
    public PlayerDataBase();
    public PlayerDataBase(BasePlayer player);
}

public class PlayerDataController
{
    [JsonProperty(Required = Required.Always)]
    private Dictionary<string, T> playerData { get; set; }
    public T Get(string identifer);
    public T Get(ulong userID);
    public T Get(BasePlayer player);
}

public class JSONSpawnPoint
{
    public float xPos { get; set; }
    public float yPos { get; set; }
    public float zPos { get; set; }
    public float xRot { get; set; }
    public float yRot { get; set; }
    public float zRot { get; set; }
    public float wRot { get; set; }
    public BasePlayer.SpawnPoint ToSpawnPoint();
    [JsonIgnore]
    public Vector3 Position { get; set; }
    [JsonIgnore]
    public Quaternion Rotation { get; set; }
    public JSONSpawnPoint();
    public JSONSpawnPoint(Vector3 position, Quaternion rot);
}

public class JSONEntity
{
    public float xPos { get; set; }
    public float yPos { get; set; }
    public float zPos { get; set; }
    public float xRot { get; set; }
    public float yRot { get; set; }
    public float zRot { get; set; }
    public float wRot { get; set; }
    public string prefabName { get; set; }
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
    public BuildingGrade.Enum grade { get; set; }
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
    public int AssignedBaseID;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
    public ulong OwnerID;
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public JSONStorageContainer Inventory;
    [JsonIgnore]
    public Vector3 Position { get; set; }
    [JsonIgnore]
    public Quaternion Rotation { get; set; }
    [JsonIgnore]
    public BaseEntity Entity;
    public JSONEntity(BaseEntity entity);
    public JSONEntity(BaseEntity entity, Vector3 position, Quaternion rotation);
    public JSONEntity();
    private BaseEntity CreateEntity(bool floating);
    public void PreSpawn(BaseEntity entity);
    public void PostSpawn(BaseEntity entity);
    public BaseEntity Spawn();
    public bool IsEntity(BaseEntity entity);
}

public class JSONStorageContainer
{
    public int MaxRefills;
    public bool Locked;
    public bool PerPlayer;
    public JSONItemContainer SavedInventory;
}

public class JSONItemAmount
{
    public string shortname;
    public int amount;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
    public ulong skinID;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore)]
    public int slot;
    public JSONItemAmount();
    public JSONItemAmount(string shortname, int amount, ulong skinID);
    public JSONItemAmount(ItemAmount itemAmount);
    public JSONItemAmount(Item item, int slot);
    public int ItemID();
    public Item CreateItem();
    public void GiveToPlayer(BasePlayer player);
    public void AddToContainer(ItemContainer container);
}

public class JSONItemContainer
{
    public List<JSONItemAmount> Items;
    public JSONItemContainer();
    public JSONItemContainer(ItemContainer container);
    public void Load(ItemContainer container);
}

public class JSONFile
{
    private DynamicConfigFile _file;
    public string _name { get; set; }
    public Type Instance { get; set; }
    private ConfigLocation _location { get; set; }
    private string _path { get; set; }
    public bool SaveOnUnload;
    public bool Compressed;
    public JSONFile(string name, ConfigLocation location, string path, string extension, bool saveOnUnload);
    public virtual void Init();
    public virtual void Load();
    private void LoadCompressed();
    public virtual void Save();
    private void SaveCompressed();
    public virtual void Reload();
    private void Unload(Plugin sender, PluginManager manager);
}

public class UIElement : UIBaseElement
{
    public CuiElement Element { get; set; }
    public UIOutline Outline { get; set; }
    public CuiRectTransformComponent transform { get; set; }
    public float FadeOut { get; set; }
    private float _fadeOut;
    public string Name { get; set; }
    public UIElement(UIBaseElement parent);
    public UIElement(Vector2 position, float width, float height, UIBaseElement parent);
    public UIElement(Vector2 min, Vector2 max, UIBaseElement parent);
    public void AddOutline(string color, string distance);
    public virtual void Init();
    public override void Show(BasePlayer player, bool children);
    public override void Hide(BasePlayer player, bool children);
    public override void UpdatePlacement();
    public void SetPositionAndSize(CuiRectTransformComponent trans);
    public void SetParent(UIElement element);
    public void SetParent(string parent);
}

public class UIButton : UIElement, UICallbackComponent
{
    public CuiButtonComponent buttonComponent { get; set; }
    public CuiTextComponent textComponent { get; set; }
    public UILabel Label { get; set; }
    private string _textColor { get; set; }
    private string _buttonText { get; set; }
    public string Text { get; set; }
    public Func<BasePlayer, string> variableText { get; set; }
    public Action<ConsoleSystem.Arg> onCallback;
    private int _fontSize;
    public UIButton(Vector2 min, Vector2 max, string buttonText, string buttonColor, string textColor, int fontSize, UIBaseElement parent);
    public UIButton(Vector2 position, float width, float height, string buttonText, string buttonColor, string textColor, int fontSize, UIBaseElement parent);
    public override void Init();
    public void AddChatCommand(string fullCommand);
    public void AddCallback(Action<BasePlayer> callback);
    public override void Show(BasePlayer player, bool children);
    public void InvokeCallback(ConsoleSystem.Arg args);
}

public class UIBackgroundText : UIPanel
{
    public UILabel Label;
    public UIBackgroundText(Vector2 min, Vector2 max, UIBaseElement parent, string backgroundColor, string labelText, int fontSize, string fontColor, TextAnchor alignment);
}

public class UILabel : UIElement
{
    public CuiTextComponent text { get; set; }
    public UILabel(Vector2 min, Vector2 max, string labelText, int fontSize, string fontColor, UIBaseElement parent, TextAnchor alignment);
    public UILabel(Vector2 min, float width, float height, string labelText, int fontSize, string fontColor, UIBaseElement parent, TextAnchor alignment);
    public string Text { get; set; }
    public TextAnchor Allign { get; set; }
    public Color Color { get; set; }
    public string ColorString { get; set; }
    public Func<BasePlayer, string> variableText { get; set; }
    public Func<BasePlayer, string> variableFontColor { get; set; }
    public override void Show(BasePlayer player, bool children);
    public override void Init();
}

public class UIImageBase : UIElement
{
    public UIImageBase(Vector2 min, Vector2 max, UIBaseElement parent);
    private CuiNeedsCursorComponent needsCursor { get; set; }
    private bool requiresFocus { get; set; }
    public bool CursorEnabled { get; set; }
}

public class UIPanel : UIImageBase
{
    private CuiImageComponent panel;
    public string Color { get; set; }
    public string Material { get; set; }
    public Func<BasePlayer, string> variableColor { get; set; }
    public UIPanel(Vector2 min, Vector2 max, UIBaseElement parent, string color);
    public UIPanel(Vector2 position, float width, float height, UIBaseElement parent, string color);
    public override void Show(BasePlayer player, bool children);
}

public class UIButtonContainer : UIPanel
{
    private float _padding;
    private float _buttonHeight;
    private string _buttonColor;
    private List<UIButton> buttons;
    public UIButtonContainer(Vector2 min, Vector2 max, UIBaseElement parent, string bgColor, float buttonHeight, float padding, string buttonColor);
    public UIButton AddButton(string text, int fontSize, string textColor, string buttonColor, Action<BasePlayer> callback);
}

public class UIPagedElements : UIBaseElement
{
    private UIButton nextPage { get; set; }
    private UIButton prevPage { get; set; }
    private float _elementHeight { get; set; }
    private float _elementSpacing { get; set; }
    private int _elementWidth { get; set; }
    private Dictionary<BasePlayer, int> ElementIndex;
    private List<UIBaseElement> Elements;
    public UIPagedElements(Vector2 min, Vector2 max, float elementHeight, float elementSpacing, UIBaseElement parent, int elementWidth);
    public void NewElement(UIBaseElement element);
    public void NewElements(IEnumerable<UIBaseElement> elements);
    public override void Show(BasePlayer player, bool showChildren);
    public override void Hide(BasePlayer player, bool hideChildren);
}

public class UIButtonConfiguration
{
    public string ButtonName { get; set; }
    public string ButtonCommand { get; set; }
    public string ButtonColor { get; set; }
    public Action<BasePlayer> callback { get; set; }
}

public class UIImage : UIImageBase
{
    public CuiImageComponent Image { get; set; }
    public string Sprite { get; set; }
    public string Material { get; set; }
    public string PNG { get; set; }
    public UIImage(Vector2 min, Vector2 max, UIBaseElement parent);
    public UIImage(Vector2 position, float width, float height, UIBaseElement parent);
    public Func<BasePlayer, string> variableSprite { get; set; }
    public Func<BasePlayer, string> variablePNG { get; set; }
    public override void Show(BasePlayer player, bool children);
}

public class UIRawImage : UIImageBase
{
    public CuiRawImageComponent Image { get; set; }
    public string Material { get; set; }
    public string Sprite { get; set; }
    public string PNG { get; set; }
    public string Color { get; set; }
    public UIRawImage(Vector2 position, float width, float height, UIBaseElement parent, string url);
    public UIRawImage(Vector2 min, Vector2 max, UIBaseElement parent, string url);
    public Func<BasePlayer, string> variablePNG { get; set; }
    public Func<BasePlayer, string> variableURL { get; set; }
    public Func<BasePlayer, string> variablePNGURL { get; set; }
    public override void Show(BasePlayer player, bool children);
}

public class UIBaseElement
{
    public Vector2 localPosition { get; set; }
    public Vector2 localSize { get; set; }
    public Vector2 globalSize { get; set; }
    public Vector2 globalPosition { get; set; }
    public HashSet<BasePlayer> players { get; set; }
    public UIBaseElement _parent { get; set; }
    public HashSet<UIBaseElement> children { get; set; }
    public Vector2 min { get; set; }
    public Vector2 max { get; set; }
    public string Parent { get; set; }
    public bool _shouldShow;
    public Func<BasePlayer, bool> conditionalShow { get; set; }
    public Func<BasePlayer, Vector2> conditionalSize { get; set; }
    public Func<BasePlayer, Vector2> conditionalPosition { get; set; }
    public UIBaseElement(UIBaseElement parent);
    public UIBaseElement(Vector2 min, Vector2 max, UIBaseElement parent);
    public UIBaseElement(Vector2 min, float width, float height, UIBaseElement parent);
    public void AddElement(UIBaseElement element);
    public void RemoveElement(UIBaseElement element);
    public void Refresh(BasePlayer player);
    public bool AddPlayer(BasePlayer player);
    public bool RemovePlayer(BasePlayer player);
    public void Show(IEnumerable<BasePlayer> players);
    public virtual void SetParent(UIBaseElement parent);
    public virtual void Hide(BasePlayer player, bool hideChildren);
    public void Hide(IEnumerable<BasePlayer> players);
    public virtual bool Toggle(BasePlayer player);
    public virtual void Show(BasePlayer player, bool showChildren);
    public bool CanShow(BasePlayer player);
    public void HideAll();
    public void RefreshAll();
    public void SafeAddUi(BasePlayer player, CuiElement element);
    public void SafeDestroyUi(BasePlayer player, CuiElement element);
    public void SetSize(float x, float y);
    public void SetPosition(float x, float y);
    public virtual void UpdatePlacement();
}

public class UIReflectionElement : UIPanel
{
    private object config { get; set; }
    private object _target { get; set; }
    private FieldInfo Field { get; set; }
    private UILabel Text { get; set; }
    private UIInputField InputField { get; set; }
    private UIButton editButton { get; set; }
    private bool EditBox { get; set; }
    public UIReflectionElement(Vector2 min, Vector2 max, FieldInfo field, object configuration, UIBaseElement parent);
    public override void Show(BasePlayer player, bool children);
    public string GetVisualText();
    public void AssignValue(string text);
}

public class UIGridDisplay
{
    public UIGridDisplay(Vector2 min, Vector2 max, int width, int height, float paddingX, float paddingY);
}

public class ObjectMemoryInfo
{
    public string name { get; set; }
    public int memoryUsed { get; set; }
    public int elements { get; set; }
    public object _target { get; set; }
    private int currentLayer { get; set; }
    public ObjectMemoryInfo _parent { get; set; }
    private bool _autoExpand { get; set; }
    public List<ObjectMemoryInfo> children { get; set; }
    public List<MethodInfo> methods { get; set; }
    public ObjectMemoryInfo(object targetObject, int layers, string variableName, ObjectMemoryInfo parent, bool autoExpand);
    public void Expand();
    public void SetupObject();
    private int GetCount();
    public string GetInfo();
    public string GetVisualText();
    public static string GetMethodText(MethodInfo info);
    private static string GetTypeName(Type type);
    private string GetMemoryUsage();
    public void CalculateSubObjects();
    public bool CheckParents();
    public List<string> GetOutput(int layer, bool justLists);
    public string PrintOutput(bool justLists);
}

public class UICheckbox : UIButton
{
    public UICheckbox(Vector2 min, Vector2 max, UIBaseElement parent);
}

public class UIOutline
{
    public CuiOutlineComponent component;
    public string Color { get; set; }
    public string Distance { get; set; }
    private string _color;
    private string _distance;
    public UIOutline();
    public UIOutline(string color, string distance);
    private void UpdateComponent();
}

public class UIInputField : UIPanel, UICallbackComponent
{
    public CuiInputFieldComponent InputField { get; set; }
    public Action<ConsoleSystem.Arg> onCallback;
    public UIInputField(Vector2 min, Vector2 max, UIBaseElement parent, string defaultText, TextAnchor align, int fontSize, string panelColor, string textColor, bool password, int charLimit);
    public void AddCallback(Action<BasePlayer, string> callback);
    public void InvokeCallback(ConsoleSystem.Arg args);
}

public class UIInput_Raw : UIElement
{
    public CuiInputFieldComponent InputField { get; set; }
    public UIInput_Raw(Vector2 min, Vector2 max, UIBaseElement parent, string defaultText, TextAnchor align, int fontSize, string textColor, bool password, int charLimit);
}


```

---

## EffectsPanel by Mevent - Displays effects in the interface with the ability to play it and output the path to the console

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Effects Panel", "Mevent", "1.1.0")]
[Description("Displaying effects in the interface with the ability to play it and output it to the console")]
public class EffectsPanel : RustPlugin
{
    [PluginReference]
    private Plugin Notify;
    private const string Layer;
    private readonly List<string> _effects;
    private const string PermUse;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public string[] Commands;
        [JsonProperty(PropertyName = "Work with Notify?")]
        public bool UseNotify;
        [JsonProperty(PropertyName = "Interface Settings")]
        public InterfaceConf UI;
    }

    private class InterfaceConf
    {
        [JsonProperty(PropertyName = "Amount on page")]
        public int AmountOnPage;
        [JsonProperty(PropertyName = "Item Height")]
        public float ItemHeight;
        [JsonProperty(PropertyName = "Items Margin")]
        public float ItemsMargin;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    private void Unload();
    private void CmdOpenEffects(IPlayer cov, string command, string[] args);
    [ConsoleCommand("UI_Effects")]
    private void CmdConsoleEffects(ConsoleSystem.Arg arg);
    private void MainUi(BasePlayer player, int page, string search, bool first);
    private static string HexToCuiColor(string hex, float alpha);
    private static void SendEffect(BasePlayer player, string effect);
    private const string ShowEffect;
    private const string NoPermission;
    private const string PlayTitle;
    private const string DebugTitle;
    private const string SearchTitle;
    private const string BtnBack;
    private const string BtnNext;
    private const string CloseButton;
    private const string TitleMenu;
    protected override void LoadDefaultMessages();
    private string Msg(BasePlayer player, string key, object[] obj);
    private void Reply(BasePlayer player, string key, object[] obj);
    private void SendNotify(BasePlayer player, string key, int type, object[] obj);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public string[] Commands;
    [JsonProperty(PropertyName = "Work with Notify?")]
    public bool UseNotify;
    [JsonProperty(PropertyName = "Interface Settings")]
    public InterfaceConf UI;
}

private class InterfaceConf
{
    [JsonProperty(PropertyName = "Amount on page")]
    public int AmountOnPage;
    [JsonProperty(PropertyName = "Item Height")]
    public float ItemHeight;
    [JsonProperty(PropertyName = "Items Margin")]
    public float ItemsMargin;
}


```

---

## ElectricGeneratorTweaker by FastBurst - Allows tweaking/changing of electric Test Generator attributes

```csharp
using System.Collections.Generic;
using System;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Electric Generator Tweaker", "FastBurst", "1.0.1")]
[Description("Change Electric Generator Attributes")]
public class ElectricGeneratorTweaker : RustPlugin
{
    private const string PERMS;
    private void Init();
    private void OnServerInitialized();
    private void OnEntitySpawned(BaseNetworkable entity);
    private void SetAnElectricGeneratorWorld();
    private void ElectricGeneratorTweakerizer(ElectricGenerator generator);
    private static ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Electric Generator")]
        public GeneralSettings General { get; set; }
        [JsonProperty(PropertyName = "Electric Generator Attributes")]
        public OutputSettings Output { get; set; }
        public class GeneralSettings
        {
            [JsonProperty(PropertyName = "Setting for all World")]
            public bool ElectricGeneratorWorld { get; set; }
            [JsonProperty(PropertyName = "Enable Debug option to console output (default false)")]
            public bool enableDebug { get; set; }
        }

        public class OutputSettings
        {
            [JsonProperty(PropertyName = "Amount of electricity (100 by default)")]
            public float electricAmount { get; set; }
        }

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
    [JsonProperty(PropertyName = "Electric Generator")]
    public GeneralSettings General { get; set; }
    [JsonProperty(PropertyName = "Electric Generator Attributes")]
    public OutputSettings Output { get; set; }
    public class GeneralSettings
    {
        [JsonProperty(PropertyName = "Setting for all World")]
        public bool ElectricGeneratorWorld { get; set; }
        [JsonProperty(PropertyName = "Enable Debug option to console output (default false)")]
        public bool enableDebug { get; set; }
    }

    public class OutputSettings
    {
        [JsonProperty(PropertyName = "Amount of electricity (100 by default)")]
        public float electricAmount { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class GeneralSettings
{
    [JsonProperty(PropertyName = "Setting for all World")]
    public bool ElectricGeneratorWorld { get; set; }
    [JsonProperty(PropertyName = "Enable Debug option to console output (default false)")]
    public bool enableDebug { get; set; }
}

public class OutputSettings
{
    [JsonProperty(PropertyName = "Amount of electricity (100 by default)")]
    public float electricAmount { get; set; }
}


```

---

## ElectricTaser by ZockiRR - Gives players with permissions the ability to spawn a taser.

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Rust;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Electric Taser", "ZockiRR", "2.0.5")]
[Description("Gives players the ability to spawn a taser")]
 class ElectricTaser : CovalencePlugin
{
    private const string PERMISSION_GIVETASER;
    private const string PERMISSION_REMOVEALLTASERS;
    private const string PERMISSION_TASENPC;
    private const string PERMISSION_USETASER;
    private const string I18N_NO_PLAYER_FOR_NAME;
    private const string I18N_MULTIPLE_PLAYERS_FOR_NAME;
    private const string I18N_GAVE_TASER_TO;
    private const string I18N_GAVE_TASER_TO_YOU;
    private const string I18N_COULD_NOT_SPAWN;
    private const string I18N_TASER;
    private const string I18N_PLAYERS_ONLY;
    private const string I18N_CANNOT_MOVE_ITEM;
    private const string I18N_NOT_ALLOWED_TO_USE;
    private const string I18N_REMOVED_ALL_TASERS;
    private class DataContainer
    {
        public HashSet<ulong> NailgunIDs;
    }

    private Configuration config;
    private class Configuration
    {
        [JsonProperty("TaserCooldown")]
        public float TaserCooldown;
        [JsonProperty("TaserDistance")]
        public float TaserDistance;
        [JsonProperty("TaserShockDuration")]
        public float TaserShockDuration;
        [JsonProperty("TaserDamage")]
        public float TaserDamage;
        [JsonProperty("NoUsePermissionDamage")]
        public float NoUsePermissionDamage;
        [JsonProperty("InstantKillsNPCs")]
        public bool InstantKillsNPCs;
        [JsonProperty("NPCBeltLocked")]
        public bool NPCBeltLocked;
        [JsonProperty("NPCWearLocked")]
        public bool NPCWearLocked;
        [JsonProperty("ItemNailgun")]
        public string ItemNailgun;
        [JsonProperty("PrefabScream")]
        public string PrefabScream;
        [JsonProperty("PrefabShock")]
        public string PrefabShock;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    [Command("givetaser", "givetazer"), Permission(PERMISSION_GIVETASER)]
    private void GiveTaser(IPlayer aPlayer, string aCommand, string[] someArgs);
    [Command("removealltasers", "removealltazers"), Permission(PERMISSION_REMOVEALLTASERS)]
    private void RemoveAllTasers(IPlayer aPlayer, string aCommand, string[] someArgs);
    private void Unload();
    private void OnServerSave();
    private void OnServerInitialized(bool anInitialFlag);
    private void OnWeaponFired(BaseProjectile aProjectile, BasePlayer aPlayer, ItemModProjectile aMod, ProtoBuf.ProjectileShoot aProjectileProtoBuf);
    private object CanCreateWorldProjectile(HitInfo anInfo, ItemDefinition anItemDefinition);
    private object OnEntityTakeDamage(BaseCombatEntity anEntity, HitInfo aHitInfo);
    private object CanMoveItem(Item anItem, PlayerInventory aPlayerLoot, uint aTargetContainer, int aTargetSlot, int anAmount);
    private object OnPlayerRecover(BasePlayer aPlayer);
    private void OnItemDropped(Item anItem, BaseEntity aWorldEntity);
    private object OnItemPickup(Item anItem, BasePlayer aPlayer);
    private Item GiveItemToPlayer(BasePlayer aPlayer, string anItemName);
    private void EnableTaserBehaviour(BaseProjectile aBaseProjectile);
    private void DisableTaserBehaviour(BaseProjectile aBaseProjectile);
    private IPlayer FindPlayer(string aPlayerNameOrId, IPlayer aPlayer);
    private string GetText(string aKey, string aPlayerId, object[] someArgs);
    private void Message(IPlayer aPlayer, string anI18nKey, object[] someArgs);
    private void Message(BasePlayer aPlayer, string anI18nKey, object[] someArgs);
    private class TaserController : FacepunchBehaviour
    {
        public Configuration Config { get; set; }
        private BaseProjectile Taser { get; set; }
        private BaseProjectile taser;
        public void ResetTaser();
    }

    private class ShockedController : FacepunchBehaviour
    {
        public Configuration Config { get; set; }
        public bool IsShocked { get; set; }
        private BasePlayer Player { get; set; }
        private BasePlayer player;
        public void Shock(HitInfo aHitInfo);
        private void StopWounded();
    }

}

private class DataContainer
{
    public HashSet<ulong> NailgunIDs;
}

private class Configuration
{
    [JsonProperty("TaserCooldown")]
    public float TaserCooldown;
    [JsonProperty("TaserDistance")]
    public float TaserDistance;
    [JsonProperty("TaserShockDuration")]
    public float TaserShockDuration;
    [JsonProperty("TaserDamage")]
    public float TaserDamage;
    [JsonProperty("NoUsePermissionDamage")]
    public float NoUsePermissionDamage;
    [JsonProperty("InstantKillsNPCs")]
    public bool InstantKillsNPCs;
    [JsonProperty("NPCBeltLocked")]
    public bool NPCBeltLocked;
    [JsonProperty("NPCWearLocked")]
    public bool NPCWearLocked;
    [JsonProperty("ItemNailgun")]
    public string ItemNailgun;
    [JsonProperty("PrefabScream")]
    public string PrefabScream;
    [JsonProperty("PrefabShock")]
    public string PrefabShock;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private class TaserController : FacepunchBehaviour
{
    public Configuration Config { get; set; }
    private BaseProjectile Taser { get; set; }
    private BaseProjectile taser;
    public void ResetTaser();
}

private class ShockedController : FacepunchBehaviour
{
    public Configuration Config { get; set; }
    public bool IsShocked { get; set; }
    private BasePlayer Player { get; set; }
    private BasePlayer player;
    public void Shock(HitInfo aHitInfo);
    private void StopWounded();
}


```

---

## ElevatorControl by bluu - Control elevator options like speed and power requirement

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using ProtoBuf;
using UnityEngine;

Oxide.Plugins
[Info ("Elevator Control", "ghostr", "1.0.2")]
[Description ("Change elevator settings")]
public class ElevatorControl : RustPlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty (PropertyName = "ElevatorSpeed")]
        public float ElevatorSpeed { get; set; }
        [JsonProperty (PropertyName = "DoesNeedPower")]
        public bool DoesNeedPower { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
     void OnEntitySpawned(BaseNetworkable entity);
}

public class Configuration
{
    [JsonProperty (PropertyName = "ElevatorSpeed")]
    public float ElevatorSpeed { get; set; }
    [JsonProperty (PropertyName = "DoesNeedPower")]
    public bool DoesNeedPower { get; set; }
}


```

---

## ElevatorCounters by WhiteThunder - Allows wiring counters into elevators to display the current floor and function as a call button

```csharp
using System;
using System.Collections.Generic;
using System.Reflection;
using UnityEngine;

Oxide.Plugins
[Info("Elevator Counters", "WhiteThunder", "1.0.2")]
[Description("Allows wiring counters into elevators to display the current floor and function as a call button.")]
internal class ElevatorCounters : CovalencePlugin
{
    private static readonly PropertyInfo ElevatorLiftOwnerProperty;
    private const float MaxCounterUpdateFrequency;
    private readonly Dictionary<NetworkableId, Action> liftTimerActions;
    private void OnServerInitialized();
    private void HandleCounterInit(PowerCounter counter);
    private void OnEntityKill(Elevator elevator);
    private void OnEntityKill(ElevatorLift lift);
    private void OnElevatorMove(Elevator topElevator, int targetFloor);
    private object OnCounterModeToggle(PowerCounter counter, BasePlayer player, bool doShowPassthrough);
    private void OnInputUpdate(PowerCounter counter, int inputAmount);
    private void OnInputUpdate(Elevator elevator, int inputAmount, int inputSlot);
    private void OnInputUpdate(ElevatorIOEntity elevatorIOEntity, int inputAmount);
    private static Elevator GetOwnerElevator(ElevatorLift lift);
    private float GetTravelTime(ElevatorLift lift);
    private void StartUpdatingLiftCounters(ElevatorLift lift, PowerCounter[] counters, float timeToTravel);
    private void UpdateCounters(ElevatorLift lift, PowerCounter[] counters);
    private int GetDisplayFloor(Elevator topElevator);
    private bool IsPowered(Elevator topElevator);
    private Elevator GetTopElevator(Elevator elevator);
    private Elevator GetBottomElevator(Elevator elevator);
    private Elevator GetFarthestElevatorInDirection(Elevator elevator, Elevator.Direction direction);
    private void MaybeToggleCounter(Elevator elevator, PowerCounter counter);
    private void InitializeCounter(PowerCounter counter, int floor);
    private void ResetCounter(PowerCounter counter);
    private Elevator GetConnectedElevator(PowerCounter counter);
    private bool IsEligibleToBeElevatorCounter(PowerCounter counter);
    private bool HasConnectedInput(PowerCounter counter);
    private PowerCounter[] GetAllConnectedCounters(Elevator topElevator);
    private void GetConnectedCounters(Elevator elevator, PowerCounter counter1, PowerCounter counter2);
    private PowerCounter GetEligibleElevatorCounter(PowerCounter counter);
}


```

---

## ElevatorSpeed by Lincoln - Modify the speed of elevators per player

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Elevator Speed", "Lincoln", "1.0.6")]
[Description("Adjust the speed of the elevator.")]
public class ElevatorSpeed : RustPlugin
{
    private const string PermUse;
    private const string PermAdmin;
    private const int MinSpeed;
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Maximum Speed")]
        public int MaximumSpeed { get; set; }
        public static Configuration DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private List<Elevator> FindElevators(BasePlayer player, float radius);
    [ChatCommand("ls")]
    private void LiftSpeedCommandAlt(BasePlayer player, string command, string[] args);
    [ChatCommand("liftspeed")]
    private void LiftSpeedCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("lc")]
    private void LiftCheckCommandAlt(BasePlayer player, string command, string[] args);
    [ChatCommand("liftcheck")]
    private void LiftCheckCommand(BasePlayer player, string command, string[] args);
    private void CmdLiftSpeed(BasePlayer player, string command, string[] args);
    private void CmdLiftCheck(BasePlayer player, string command, string[] args);
    private void Message(BasePlayer player, string messageKey, object[] args);
    protected override void LoadDefaultMessages();
}

private class Configuration
{
    [JsonProperty("Maximum Speed")]
    public int MaximumSpeed { get; set; }
    public static Configuration DefaultConfig();
}


```

---

## Ember by Mkekala - Rust integration for the Ember store / ban management system

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json.Linq;
using Oxide.Core.Libraries;

Oxide.Plugins
[Info ("Ember", "Mkekala", "1.4.0")]
[Description ("Integrates Ember store & ban management with Rust")]
public class Ember : RustPlugin
{
    private class PluginConfig
    {
        public string Host;
        public string Token;
    }

    private PluginConfig config;
    private bool banLog;
    private Dictionary<string, bool> roleSync;
    private Dictionary<string, string> headers;
    protected override void LoadDefaultConfig();
    private bool connected;
    private Dictionary<string, JToken> userData;
    private Dictionary<string, string> usersToPost;
    private List<string> usersProcessed;
     void QueueUserToPost(string steamid, string name);
     void PostUsers(Dictionary<string, string> users);
     void ProcessUser(BasePlayer player);
     void PostUsersProcessed(List<String> users);
    private void PollUsers();
     void Ban(string offenderSteamid, string expiryMinutes, string reason, bool global, string adminSteamid, BasePlayer caller);
     void Unban(string offenderSteamid, BasePlayer caller);
     void PostRole(string steamid, string role);
     void DeleteRole(string steamid, string role);
    static List<BasePlayer> GetPlayersByName(string name);
    static BasePlayer GetPlayerBySteamID(string steamid);
    protected void SendApiConnectionWarning(BasePlayer player);
    private void Init();
     void OnUserBanned(string name, string id, string ipAddress, string reason);
     void OnUserUnbanned(string name, string id, string ipAddress);
     void OnUserGroupAdded(string id, string groupName);
     void OnUserGroupRemoved(string id, string groupName);
     void OnUserApproved(string name, string id, string ipAddress);
     void OnPlayerConnected(BasePlayer player);
    [ChatCommand ("ban")]
     void cmdBan(BasePlayer player, string command, string[] args);
    [ChatCommand ("unban")]
     void cmdUnban(BasePlayer player, string command, string[] args);
    [ChatCommand ("sync")]
     void cmdSync(BasePlayer player, string command, string[] args);
    protected override void LoadDefaultMessages();
}

private class PluginConfig
{
    public string Host;
    public string Token;
}


```

---

## EMInterface by k1lly0u - The control system for the Event Manager plugin

```csharp
using System;
using System.Text;
using System.Collections.Generic;
using System.Runtime.InteropServices;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;
using System.Linq;
using System.Globalization;

Oxide.Plugins
[Info("EMInterface", "k1lly0u", "2.0.2")]
[Description("Manages and provides user interface for event games")]
public class EMInterface : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    public static EMInterface Instance { get; set; }
    public static ConfigData Configuration { get; set; }
    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private void Unload();
    private const float ELEMENT_HEIGHT;
    public void OpenMenu(BasePlayer player, MenuArgs args);
    private void CreateMenuPopup(BasePlayer player, string text, float duration);
    private void AddMenuButtons(BasePlayer player, CuiElementContainer container, string panel, MenuTab menuTab);
    private void CreateEventDetails(BasePlayer player, CuiElementContainer container, string panel, int page);
    private void CreateListEntryLeft(CuiElementContainer container, string key, string value, float yMin);
    private void CreateListEntryRight(CuiElementContainer container, string key, string value, float yMin);
    private void CreateScoreEntryRight(CuiElementContainer container, string displayName, string score1, string score2, float yMin);
    private void CreateSplitEntryRight(CuiElementContainer container, string key, string value, float yMin);
    private void CreateAdminOptions(BasePlayer player, CuiElementContainer container, string panel, MenuArgs args);
    private Hash<ulong, EventManager.EventConfig> _eventCreators;
    private void EventCreatorMenu(BasePlayer player, CuiElementContainer container, string panel);
    private void AddInputField(CuiElementContainer container, string panel, int index, string title, string fieldName, object currentValue);
    private void AddLabelField(CuiElementContainer container, string panel, int index, string title, string value);
    private void AddToggleField(CuiElementContainer container, string panel, int index, string title, string fieldName, bool currentValue);
    private void AddSelectorField(CuiElementContainer container, string panel, int index, string title, string fieldName, string currentValue, string hook, bool allowMultiple);
    private string GetSelectorLabel(IEnumerable<object> list);
    private string GetInputLabel(object obj);
    private void OpenEventSelector(BasePlayer player, CuiElementContainer container, string panel, SelectorArgs args, int page);
    private void OpenSelector(BasePlayer player, CuiElementContainer container, string panel, SelectorArgs args, int page);
    private UI4 GetGridLayout(int index, float xMin, float yMin, float width, float height, int columns, int rows);
    private UI4 GetGridLayout(int columnNumber, int rowNumber, float xMin, float yMin, float width, float height, int columns, int rows);
    private void CreateStatisticsMenu(BasePlayer player, CuiElementContainer container, MenuArgs args);
    private void AddStatisticHeader(CuiElementContainer container, ulong playerId, StatisticTab openTab);
    private void AddStatistics(CuiElementContainer container, bool isGlobal, ulong playerId, int page);
    private void AddLeaderBoard(CuiElementContainer container, ulong playerId, int page, EventStatistics.Statistic sortBy);
    private void AddStatistic(CuiElementContainer container, string panel, string color, string message, float xMin, float xMax, float verticalPos, TextAnchor anchor);
    private void AddLeaderSortButton(CuiElementContainer container, string panel, string color, string message, int page, EventStatistics.Statistic statistic, float xMin, float xMax, float verticalPos, TextAnchor anchor);
    private float GetHorizontalPos(int i, float start, float size);
    private float GetVerticalPos(int i, float start);
    public static CuiElementContainer CreateScoreboardBase(EventManager.BaseEventGame baseEventGame);
    public static void CreateScoreEntry(CuiElementContainer container, string text, string value1, string value2, int index);
    public static void CreatePanelEntry(CuiElementContainer container, string text, int index);
    private const string DEATH_SKULL_ICON;
    private const string DEATH_BACKGROUND;
    private void RegisterDeathScreenImages();
    public static void DisplayDeathScreen(EventManager.BaseEventPlayer victim, string message, bool canRespawn);
    public static void UpdateRespawnButton(EventManager.BaseEventPlayer eventPlayer);
    public static void CreateLeaveButton(EventManager.BaseEventPlayer eventPlayer);
    internal void AddImage(string imageName, string url);
    internal string GetImage(string name);
    [ConsoleCommand("emui.create")]
    private void ccmdCreateEvent(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.saveevent")]
    private void ccmdSaveEvent(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.disposeevent")]
    private void ccmdDisposeEvent(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.clear")]
    private void ccmdClearField(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.creator")]
    private void ccmdSetField(ConsoleSystem.Arg arg);
    private void SetParameter(BasePlayer player, EventManager.EventConfig eventConfig, string fieldName, object value);
    private bool TryConvertValue(object value, T result);
    private void AddToRemoveFromList(List<string> list, string value);
    [ConsoleCommand("emui.toggleautospawn")]
    private void ccmdToggleAutoSpawn(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.respawn")]
    private void ccmdRespawn(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.selectkit")]
    private void ccmdSelectKit(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.close")]
    private void ccmdCloseUI(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.joinevent")]
    private void ccmdJoinEvent(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.leaveevent")]
    private void ccmdLeaveEvent(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.statistics")]
    private void ccmdStatistics(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.event")]
    private void ccmdEvent(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.eventselector")]
    private void ccmdOpenEventSelector(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.openevent")]
    private void ccmdOpenEvent(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.endevent")]
    private void ccmdEndEvent(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.startevent")]
    private void ccmdStartEvent(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.closeevent")]
    private void ccmdCloseEvent(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.editevent")]
    private void ccmdEditEvent(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.deleteevent")]
    private void ccmdDeleteEvent(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.closeselector")]
    private void ccmdCloseSelector(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.fieldselector")]
    private void ccmdOpenSelector(ConsoleSystem.Arg arg);
    [ConsoleCommand("emui.select")]
    private void ccmdSelect(ConsoleSystem.Arg arg);
    private static string CommandSafe(string text, bool unpack);
    internal const string UI_MENU;
    internal const string UI_TIMER;
    internal const string UI_SCORES;
    internal const string UI_POPUP;
    internal const string UI_DEATH;
    internal const string UI_RESPAWN;
    internal static void DestroyAllUI(BasePlayer player);
    public static class UI
    {
        public static CuiElementContainer Container(string panelName, string color, UI4 dimensions, bool useCursor, string parent);
        public static CuiElementContainer Popup(string panelName, string text, int size, UI4 dimensions, TextAnchor align, string parent);
        public static void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions);
        public static void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
        public static void Button(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, string command, TextAnchor align);
        public static void Input(CuiElementContainer container, string panel, string text, int size, string command, UI4 dimensions);
        public static void Image(CuiElementContainer container, string panel, string png, UI4 dimensions);
        public static void Toggle(CuiElementContainer container, string panel, string boxColor, int fontSize, UI4 dimensions, string command, bool isOn);
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
        private static UI4 _full;
        public static UI4 Full { get; set; }
    }

    public class ConfigData
    {
        [JsonProperty(PropertyName = "Death screen skull image")]
        public string DeathIcon { get; set; }
        [JsonProperty(PropertyName = "Death screen background image")]
        public string DeathBackground { get; set; }
        [JsonProperty(PropertyName = "Menu Colors")]
        public MenuColors Menu { get; set; }
        [JsonProperty(PropertyName = "Scoreboard Colors")]
        public ScoreboardColors Scoreboard { get; set; }
        public class MenuColors
        {
            [JsonProperty(PropertyName = "Background Color")]
            public UIColor Background { get; set; }
            [JsonProperty(PropertyName = "Foreground Color")]
            public UIColor Foreground { get; set; }
            [JsonProperty(PropertyName = "Panel Color")]
            public UIColor Panel { get; set; }
            [JsonProperty(PropertyName = "Button Color")]
            public UIColor Button { get; set; }
            [JsonProperty(PropertyName = "Highlight Color")]
            public UIColor Highlight { get; set; }
        }

        public class ScoreboardColors
        {
            [JsonProperty(PropertyName = "Background Color")]
            public UIColor Background { get; set; }
            [JsonProperty(PropertyName = "Foreground Color")]
            public UIColor Foreground { get; set; }
            [JsonProperty(PropertyName = "Panel Color")]
            public UIColor Panel { get; set; }
            [JsonProperty(PropertyName = "Highlight Color")]
            public UIColor Highlight { get; set; }
            [JsonProperty(PropertyName = "Screen Position")]
            public UIPosition Position { get; set; }
        }

        public class UIColor
        {
            public string Hex { get; set; }
            public float Alpha { get; set; }
            [JsonIgnore]
            private string _color;
            [JsonIgnore]
            public string Get { get; set; }
        }

        public class UIPosition
        {
            [JsonProperty(PropertyName = "Center Position X (0.0 - 1.0)")]
            public float CenterX { get; set; }
            [JsonProperty(PropertyName = "Center Position Y (0.0 - 1.0)")]
            public float CenterY { get; set; }
            [JsonProperty(PropertyName = "Panel Width")]
            public float Width { get; set; }
            [JsonProperty(PropertyName = "Panel Height")]
            public float Height { get; set; }
            private UI4 _ui4;
            public UI4 UI4 { get; set; }
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private static string Message(string key, ulong playerId);
    private readonly Dictionary<string, string> Messages;
}

public static class UI
{
    public static CuiElementContainer Container(string panelName, string color, UI4 dimensions, bool useCursor, string parent);
    public static CuiElementContainer Popup(string panelName, string text, int size, UI4 dimensions, TextAnchor align, string parent);
    public static void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions);
    public static void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
    public static void Button(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, string command, TextAnchor align);
    public static void Input(CuiElementContainer container, string panel, string text, int size, string command, UI4 dimensions);
    public static void Image(CuiElementContainer container, string panel, string png, UI4 dimensions);
    public static void Toggle(CuiElementContainer container, string panel, string boxColor, int fontSize, UI4 dimensions, string command, bool isOn);
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
    private static UI4 _full;
    public static UI4 Full { get; set; }
}

public class ConfigData
{
    [JsonProperty(PropertyName = "Death screen skull image")]
    public string DeathIcon { get; set; }
    [JsonProperty(PropertyName = "Death screen background image")]
    public string DeathBackground { get; set; }
    [JsonProperty(PropertyName = "Menu Colors")]
    public MenuColors Menu { get; set; }
    [JsonProperty(PropertyName = "Scoreboard Colors")]
    public ScoreboardColors Scoreboard { get; set; }
    public class MenuColors
    {
        [JsonProperty(PropertyName = "Background Color")]
        public UIColor Background { get; set; }
        [JsonProperty(PropertyName = "Foreground Color")]
        public UIColor Foreground { get; set; }
        [JsonProperty(PropertyName = "Panel Color")]
        public UIColor Panel { get; set; }
        [JsonProperty(PropertyName = "Button Color")]
        public UIColor Button { get; set; }
        [JsonProperty(PropertyName = "Highlight Color")]
        public UIColor Highlight { get; set; }
    }

    public class ScoreboardColors
    {
        [JsonProperty(PropertyName = "Background Color")]
        public UIColor Background { get; set; }
        [JsonProperty(PropertyName = "Foreground Color")]
        public UIColor Foreground { get; set; }
        [JsonProperty(PropertyName = "Panel Color")]
        public UIColor Panel { get; set; }
        [JsonProperty(PropertyName = "Highlight Color")]
        public UIColor Highlight { get; set; }
        [JsonProperty(PropertyName = "Screen Position")]
        public UIPosition Position { get; set; }
    }

    public class UIColor
    {
        public string Hex { get; set; }
        public float Alpha { get; set; }
        [JsonIgnore]
        private string _color;
        [JsonIgnore]
        public string Get { get; set; }
    }

    public class UIPosition
    {
        [JsonProperty(PropertyName = "Center Position X (0.0 - 1.0)")]
        public float CenterX { get; set; }
        [JsonProperty(PropertyName = "Center Position Y (0.0 - 1.0)")]
        public float CenterY { get; set; }
        [JsonProperty(PropertyName = "Panel Width")]
        public float Width { get; set; }
        [JsonProperty(PropertyName = "Panel Height")]
        public float Height { get; set; }
        private UI4 _ui4;
        public UI4 UI4 { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class MenuColors
{
    [JsonProperty(PropertyName = "Background Color")]
    public UIColor Background { get; set; }
    [JsonProperty(PropertyName = "Foreground Color")]
    public UIColor Foreground { get; set; }
    [JsonProperty(PropertyName = "Panel Color")]
    public UIColor Panel { get; set; }
    [JsonProperty(PropertyName = "Button Color")]
    public UIColor Button { get; set; }
    [JsonProperty(PropertyName = "Highlight Color")]
    public UIColor Highlight { get; set; }
}

public class ScoreboardColors
{
    [JsonProperty(PropertyName = "Background Color")]
    public UIColor Background { get; set; }
    [JsonProperty(PropertyName = "Foreground Color")]
    public UIColor Foreground { get; set; }
    [JsonProperty(PropertyName = "Panel Color")]
    public UIColor Panel { get; set; }
    [JsonProperty(PropertyName = "Highlight Color")]
    public UIColor Highlight { get; set; }
    [JsonProperty(PropertyName = "Screen Position")]
    public UIPosition Position { get; set; }
}

public class UIColor
{
    public string Hex { get; set; }
    public float Alpha { get; set; }
    [JsonIgnore]
    private string _color;
    [JsonIgnore]
    public string Get { get; set; }
}

public class UIPosition
{
    [JsonProperty(PropertyName = "Center Position X (0.0 - 1.0)")]
    public float CenterX { get; set; }
    [JsonProperty(PropertyName = "Center Position Y (0.0 - 1.0)")]
    public float CenterY { get; set; }
    [JsonProperty(PropertyName = "Panel Width")]
    public float Width { get; set; }
    [JsonProperty(PropertyName = "Panel Height")]
    public float Height { get; set; }
    private UI4 _ui4;
    public UI4 UI4 { get; set; }
}


```

---

## Emote by Hirsty - Because everyone loves to portray themselves in the third person

```csharp
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using System;

Oxide.Plugins
[Info("Emote", "Hirsty", "1.0.12", ResourceId = 1353)]
[Description("This will allow players to express their feelings!")]
 class Emote : CovalencePlugin
{
    public static Dictionary<string, object> ChatDict;
    public static string version;
    public string template;
    public string EnableEmotes;
    protected override void LoadDefaultConfig();
    private void Loaded();
    [PluginReference]
     Plugin BetterChat;
    [PluginReference]
     Plugin BetterChatMute;
    private void LoadConfigData();
    [HookMethod("CheckForEmotes")]
    public string EmoteCheck(IPlayer player, string checkmsg);
    [Command("me"), Permission("emote.use")]
     object MeCommand(IPlayer player, string command, string[] args);
     object OnBetterChat(Dictionary<string, object> Chat);
     object OnUserChat(IPlayer player, string message);
     object SendChatMessage(IPlayer player, string msg);
}


```

---

## EmptyLowFPS by MrBlue - Sets the server's frame rate to lower when no players are connected

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using System.IO;
using System.Linq;

Oxide.Plugins
[Info("Empty Low FPS", "Wulf/lukespragg", "1.0.2")]
[Description("Sets the server frame rate limit to lower when empty to save CPU usage")]
 class EmptyLowFPS : CovalencePlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Frame rate limit when empty")]
        public int FpsLimitEmpty { get; set; }
        [JsonProperty(PropertyName = "Frame rate limit when not empty")]
        public int FpsLimitNotEmpty { get; set; }
        [JsonProperty(PropertyName = "Log frame rate limit changes")]
        public bool LogChanges { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private void OnUserConnected();
    private void OnUserDisconnected();
    private void Unload();
    private void EmptyFpsLimit();
    private void NotEmptyFpsLimit();
}

public class Configuration
{
    [JsonProperty(PropertyName = "Frame rate limit when empty")]
    public int FpsLimitEmpty { get; set; }
    [JsonProperty(PropertyName = "Frame rate limit when not empty")]
    public int FpsLimitNotEmpty { get; set; }
    [JsonProperty(PropertyName = "Log frame rate limit changes")]
    public bool LogChanges { get; set; }
}


```

---

## EmptyOvens by  - Removes wood from campfires and same on placing

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Empty Ovens", "Orange", "1.0.0")]
[Description("Remove wood from campfires and same on placing")]
public class EmptyOvens : RustPlugin
{
    private void OnEntitySpawned(BaseOven entity);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Whitelist")]
        public List<string> whitelist;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Whitelist")]
    public List<string> whitelist;
}


```

---

## EnchantTools by  - Adds enchanted tools for mining melted resources

```csharp
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Enchant Tools", "Default", "1.1.2")]
[Description("Adds enchanted tools for mining melted resources")]
public class EnchantTools : RustPlugin
{
    private List<int> EnchantedTools;
    private void Init();
    protected override void LoadDefaultMessages();
    private void OnNewSave(string filename);
     void OnDispenserBonus(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private object OnItemRepair(BasePlayer player, Item item);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private void CmdEnchant(BasePlayer player, string command, string[] args);
    private void CcmdEnchant(ConsoleSystem.Arg arg);
    private void ReplaceContents(int ItemId, Item item);
    private void GiveEnchantTool(BasePlayer player, Tool tool);
    private void SaveEnchantedTools();
    private ConfigData configData;
    private class ConfigData
    {
        public string Command;
        public bool CanUseByAllPlayers;
        public List<Tool> Tools;
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void SaveConfig(ConfigData config);
    private class Tool
    {
        public string shortname;
        public uint skinId;
        public bool canRepair;
    }

}

private class ConfigData
{
    public string Command;
    public bool CanUseByAllPlayers;
    public List<Tool> Tools;
}

private class Tool
{
    public string shortname;
    public uint skinId;
    public bool canRepair;
}


```

---

## Enderpearl by Wolfleader101 - Throw an ender pearl and teleport to its location

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Enderpearl", "Wolfleader101", "0.4.1")]
[Description("Throw an ender pearl and teleport to its location")]
 class Enderpearl : RustPlugin
{
    private PluginConfig config;
    public const string enderPearlPerms;
    private void Init();
     void OnPlayerAttack(BasePlayer attacker, HitInfo info);
     void Teleport(BasePlayer attacker, HitInfo info);
    private class PluginConfig
    {
        [JsonProperty("Enderpearl")]
        public string enderpearl { get; set; }
    }

    private PluginConfig GetDefaultConfig();
    protected override void LoadDefaultConfig();
}

private class PluginConfig
{
    [JsonProperty("Enderpearl")]
    public string enderpearl { get; set; }
}


```

---

## EndlessCargo by OG61 - Calls a new cargo ship as soon as the active ship leaves

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;

Oxide.Plugins
[Info("Endless Cargo", "OG61", "1.1.1")]
[Description("Calls new cargo ship into game as soon as one departs.")]
public class EndlessCargo : CovalencePlugin
{
    const string cargoPrefab;
    const int MaxShips;
    private const string perm;
    readonly System.Random random;
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Update Rate (Seconds)")]
        public float UpdateRate;
        [JsonProperty(PropertyName = "How far off shore to spawn ship in grid blocks 1-10 (Default 5)")]
        public int GridBlocksOffShore;
        [JsonProperty(PropertyName = "NPC spawn on ship (Default true)")]
        public bool NpcSpawn;
        [JsonProperty(PropertyName = "Egress duration in minutes (Default 10)")]
        public int EgressMin;
        [JsonProperty(PropertyName = "Event duration in minutes (Default 50)")]
        public int DurationMin;
        [JsonProperty(PropertyName = "Loot round spacing in minutes (Default 10)")]
        public int LootSpacing;
        [JsonProperty(PropertyName = "Loot rounds 1-3 (Default 3)")]
        public int LootRounds;
        [JsonProperty(PropertyName = "Enable Log file (true/false)")]
        public bool LogToFile;
        [JsonProperty(PropertyName = "Log output to console (true/false)")]
        public bool LogToConsole;
    }

    private PluginConfig _config;
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private List<CargoShip> _activeCargoShips;
    private void OnServerInitialized();
    private void OnEntitySpawned(CargoShip ship);
    private string GetLang(string key, string id, object[] args);
    private void Logger(string key, IPlayer player, object[] args);
    private void Message(string key, IPlayer player, object[] args);
    private void CheckCargoShip();
    private void SpawnCargo();
    [Command("KillCargo"), Permission(perm)]
    private void KillCargo(IPlayer player);
    protected override void LoadDefaultMessages();
}

private class PluginConfig
{
    [JsonProperty(PropertyName = "Update Rate (Seconds)")]
    public float UpdateRate;
    [JsonProperty(PropertyName = "How far off shore to spawn ship in grid blocks 1-10 (Default 5)")]
    public int GridBlocksOffShore;
    [JsonProperty(PropertyName = "NPC spawn on ship (Default true)")]
    public bool NpcSpawn;
    [JsonProperty(PropertyName = "Egress duration in minutes (Default 10)")]
    public int EgressMin;
    [JsonProperty(PropertyName = "Event duration in minutes (Default 50)")]
    public int DurationMin;
    [JsonProperty(PropertyName = "Loot round spacing in minutes (Default 10)")]
    public int LootSpacing;
    [JsonProperty(PropertyName = "Loot rounds 1-3 (Default 3)")]
    public int LootRounds;
    [JsonProperty(PropertyName = "Enable Log file (true/false)")]
    public bool LogToFile;
    [JsonProperty(PropertyName = "Log output to console (true/false)")]
    public bool LogToConsole;
}


```

---

## EnforceSingleResourceExtractor by ThibmoRozier - Enforce players only being able to use a single quarry and/or pump jack.

```csharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;

Oxide.Plugins
[Info("Enforce Single Resource Extractor", "ThibmoRozier", "1.0.5")]
[Description("Enforce players only being able to use a single quarry and/or pump jack.")]
public class EnforceSingleResourceExtractor : RustPlugin
{
    private static readonly string[] CPumpJackPrefabs;
    private static readonly string[] CQuarryPrefabs;
    private static readonly IEnumerable<string> CCombinedPrefabs;
    private const String CPermWhitelist;
    private ConfigData FConfigData;
    private Timer FCleanupTimer;
    private readonly List<QuarryState> FPlayerExtractorList;
    private class ConfigData
    {
        [DefaultValue(true)]
        [JsonProperty("Ignore Extractor Type", DefaultValueHandling = DefaultValueHandling.Populate)]
        public bool IgnoreExtractorType { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void CheckExtractorIsOff();
     void OnServerInitialized();
     void Unload();
    protected override void LoadDefaultMessages();
     void OnQuarryToggled(MiningQuarry aExtractor, BasePlayer aPlayer);
     void OnEntityKill(BaseNetworkable entity);
}

private class ConfigData
{
    [DefaultValue(true)]
    [JsonProperty("Ignore Extractor Type", DefaultValueHandling = DefaultValueHandling.Populate)]
    public bool IgnoreExtractorType { get; set; }
}


```

---

## EnginePartsDurability by WhiteThunder - Alters engine part durability loss when modular cars are damaged

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Rust.Modular;
using System.Linq;

Oxide.Plugins
[Info("Engine Parts Durability", "WhiteThunder", "1.2.0")]
[Description("Alters engine part durability loss when modular cars are damaged.")]
internal class EnginePartsDurability : CovalencePlugin
{
    private DurabilityConfig _pluginConfig;
    private void Init();
    private void OnServerInitialized(bool serverInitialized);
    private void OnEntitySpawned(VehicleModuleEngine engineModule);
    private void API_RefreshMultiplier(EngineStorage engineStorage);
    private static bool MultiplierChangeWasBlocked(EngineStorage engineStorage, float desiredMultiplier);
    private void SetInternalDamageMultiplier(VehicleModuleEngine engineModule);
    protected override void LoadDefaultConfig();
    internal class DurabilityConfig
    {
        [JsonProperty("DurabilityLossMultiplier")]
        public float DurabilityLossMultiplier;
    }

}

internal class DurabilityConfig
{
    [JsonProperty("DurabilityLossMultiplier")]
    public float DurabilityLossMultiplier;
}


```

---

## EnhancedBanSystem by austinv900 - Ban players by Steam ID, IP address, local ban list, or remote ban list

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Database;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core.SQLite.Libraries;
using System.Text.RegularExpressions;

Oxide.Plugins
[Info("Enhanced Ban System", "Reneb/Slut", "5.2.6")]
 class EnhancedBanSystem : CovalencePlugin
{
    [PluginReference]
    private Plugin PlayerDatabase;
    private Plugin DiscordMessages;
    static DateTime epoch;
     char[] ipChrArray;
    private static BanSystem banSystem;
    static Hash<int, BanData> cachedBans;
    static List<int> wasBanned;
    private static string Platform;
    private static string Server;
    private static string Game;
     string PermissionBan;
     string PermissionUnban;
     string PermissionBanlist;
     string PermissionKick;
    private bool SQLite_use;
    private string SQLite_DB;
    private bool MySQL_use;
    private string MySQL_Host;
    private int MySQL_Port;
    private string MySQL_DB;
    private string MySQL_User;
    private string MySQL_Pass;
    private bool PlayerDatabase_use;
    private string PlayerDatabase_IPFile;
    private bool Files_use;
    private bool WebAPI_use;
    private string WebAPI_Ban_Request;
    private string WebAPI_Unban_Request;
    private string WebAPI_IsBanned_Request;
    private string WebAPI_Banlist_Request;
    private bool Native_use;
    private string BanDefaultReason;
    private string BanEvadeReason;
    private bool Kick_Broadcast;
    private bool Kick_Log;
    private bool Kick_OnBan;
    private bool Ban_Broadcast;
    private bool Ban_Log;
    private bool Discord_use;
    private string Discord_Webhook;
    private bool Ban_Escape;
    private bool Log_Denied;
    protected override void LoadDefaultConfig();
    private void CheckCfg(string Key, T var);
     void Init();
     void InitializeLang();
    private static DynamicConfigFile Ban_ID_File;
    private static int Ban_ID;
     void Load_ID();
     void Save_ID();
    static int GetNewID();
     class BanData
    {
        public int id;
        public string steamid;
        public string ip;
        public string name;
        public string game;
        public string server;
        public string source;
        public double date;
        public double expire;
        public string reason;
        public string platform;
        public BanData();
        public BanData(object source, string userID, string name, string ip, string reason, double duration);
        public BanData(int id, string source, string userID, string name, string ip, string reason, string duration);
        public string ToJson();
        public override string ToString();
    }

    private bool IsPluginLoaded(Plugin plugin);
     string FormatTime(TimeSpan time);
    private bool TryParseTimeSpan(string source, TimeSpan timeSpan);
    static double LogTime();
     string GetMsg(string key, object steamid, object[] args);
     bool hasPermission(IPlayer player, string permissionName);
     bool isIPAddress(string arg);
     bool IPRange(string sourceIP, string targetIP);
     bool RangeFromIP(string sourceIP, string range1, string range2, string range3);
     List<IPlayer> FindPlayers(string userIDorNameorIP, object source, string reason);
     List<IPlayer> FindConnectedPlayers(string userIDorNameorIP, object source, string reason);
     string GetPlayerIP(IPlayer iplayer);
     string GetPlayerIP(string userID);
     string GetPlayerName(string userID);
     bool HasDelayedAnswer();
     bool BanSystemHasFlag(BanSystem b, BanSystem t);
     string FormatReturn(BanSystem system, string msg, object[] args);
     void SendReply(object source, string msg);
    public static string ToShortString(TimeSpan timeSpan);
     void OnServerInitialized();
     void Unload();
     void OnServerSave();
    private void OnUserBanned(string name, string id, string address, string reason);
     object CanUserLogin(string name, string id, string ip);
     void OnUserConnected(IPlayer player);
     StoredData storedData;
     class StoredData
    {
        public HashSet<string> Banlist;
    }

     string Files_Load();
     void Save_Files();
     string Files_UpdateBan(BanData bandata);
     string Files_ExecuteBan(BanData bandata);
     string Files_RawUnban(List<BanData> unbanList);
     string Files_ExecuteUnban(string steamid, string name, string ip, List<BanData> unbanList);
     bool Files_IsBanned(string steamid, string ip, BanData bandata);
     string Files_Banlist(object source, int startid);
    static StoredIPData storedIPData;
     class StoredIPData
    {
        public HashSet<string> Banlist;
    }

     string PlayerDatabase_Load();
     void Save_PlayerDatabaseIP();
     string PlayerDatabase_ExecuteBan(BanData bandata);
     string PlayerDatabase_UpdateBan(BanData bandata, double expire);
     string PlayerDatabase_RawUnban(List<BanData> unbanList);
     string PlayerDatabase_ExecuteUnban(string steamid, string name, string ip, List<BanData> unbanList);
     bool PlayerDatabase_IsBanned(string steamid, string ip, BanData bandata);
     string PlayerDatabase_Banlist(object source, int startid);
     string FormatOnlineBansystem(string line, Dictionary<string, string> args);
     string WebAPI_ExecuteBan(object source, BanData bandata);
     string WebAPI_ExecuteUnban(object source, string steamid, string name, string ip);
     string WebAPI_IsBanned(BanData bandata, bool update);
     string WebAPI_Banlist(object source, int startid);
     string WebAPI_Load();
     SQLite Sqlite;
     Connection Sqlite_conn;
     string SQLite_Load();
     string SQLite_RawBan(BanData bandata);
     string SQLite_ExecuteBan(object source, BanData bandata);
     void SQLite_RawUnban(object source, List<long> unbanList);
     string SQLite_ExecuteUnban(object source, string steamid, string name, string ip);
     void SQLite_UpdateBan(BanData bandata);
     void SQLite_IsBanned(BanData bandata, bool update);
     string SQLite_Banlist(object source, int startid);
     Oxide.Core.MySql.Libraries.MySql Sql;
     Connection Sql_conn;
     string MySQL_Load();
     string MySQL_RawBan(BanData bandata);
     void MySQL_UpdateBan(BanData bandata);
     void MySQL_RawUnban(object source, List<int> unbanList);
     string MySQL_ExecuteBan(object source, BanData bandata);
     string MySQL_ExecuteUnban(object source, string steamid, string name, string ip);
     void MySQL_IsBanned(BanData bandata, bool update);
     string MySQL_Banlist(object source, int startid);
     string Native_Load();
     string Native_ExecuteBan(BanData bandata);
     string Native_ExecuteUnban(string steamid, string name);
     bool Native_IsBanned(string steamid);
     string Native_Banlist(object source, int startid);
     string Kick(object source, string target, string reason, bool shouldBroadcast);
     string TryKick(object source, string[] args);
     string ExecuteKick(object source, IPlayer player, string reason, bool shouldBroadcast);
     bool isBanned_Check(string name, string steamid, string ip);
     bool isBanned_NonDelayed(string name, string steamid, string ip, bool update, BanData bandata);
     void isBanned_Delayed(string name, string steamid, string ip, bool update);
     string TryBanlist(object source, string[] args);
     string Banlist(object source, BanSystem bs, int startID);
     string TryBan(object source, string[] args);
     string BanIP(object source, string ip, string reason, double duration);
     string BanID(object source, string steamid, string reason, double duration);
     string BanPlayer(object source, IPlayer player, string reason, double duration);
     string PrepareBan(object source, string userID, string name, string ip, string reason, double duration, bool kick);
     string ExecuteBan(object source, BanData bandata, bool kick);
     string ExecuteUnban(object source, string steamid, string name, string ip);
     string TryUnBan(object source, string[] args);
    [Command("ban", "player.ban")]
     void cmdBan(IPlayer player, string command, string[] args);
    [Command("banlist", "player.banlist")]
     void cmdBanlist(IPlayer player, string command, string[] args);
    [Command("kick", "player.kick")]
     void cmdKick(IPlayer player, string command, string[] args);
    [Command("unban", "player.unban")]
     void cmdUnban(IPlayer player, string command, string[] args);
}

 class BanData
{
    public int id;
    public string steamid;
    public string ip;
    public string name;
    public string game;
    public string server;
    public string source;
    public double date;
    public double expire;
    public string reason;
    public string platform;
    public BanData();
    public BanData(object source, string userID, string name, string ip, string reason, double duration);
    public BanData(int id, string source, string userID, string name, string ip, string reason, string duration);
    public string ToJson();
    public override string ToString();
}

 class StoredData
{
    public HashSet<string> Banlist;
}

 class StoredIPData
{
    public HashSet<string> Banlist;
}


```

---

## EnhancedHammer by misticos - Upgrade your buildings easily with a hammer

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using Facepunch;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;

Oxide.Plugins
[Info("Enhanced Hammer", "misticos", "2.1.1")]
[Description("Upgrade your buildings easily with a hammer")]
public class EnhancedHammer : CovalencePlugin
{
    private static EnhancedHammer _ins;
    private HashSet<BasePlayer> _activePlayers;
    private readonly int _maskConstruction;
    private const string PermissionUse;
    private const string PermissionFree;
    private const string PermissionDowngrade;
    private const string PermissionGradeHit;
    private const string PermissionGradeClick;
    private const string PermissionGradeBuild;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public string[] Commands;
        [JsonProperty("Allowed Grades", ItemConverterType = typeof(StringEnumConverter),
                NullValueHandling = NullValueHandling.Ignore)]
        public BuildingGrade.Enum[] OldGradesAllowed;
        [JsonProperty("Grades Enabled", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<GradeConfig> GradesEnabled;
        [JsonProperty(PropertyName = "Distance From Entity")]
        public float Distance;
        [JsonProperty(PropertyName = "Allow Auto Grade")]
        public bool AutoGrade;
        [JsonProperty(PropertyName = "Send Downgrade Disabled Message")]
        public bool DowngradeMessage;
        [JsonProperty(PropertyName = "Cancel Default Hammer Hit Behavior")]
        public bool CancelHammerHitDefaultBehavior;
        [JsonProperty(PropertyName = "Default Preferences")]
        public PlayerPreferences Preferences;
        public class GradeConfig
        {
            [JsonProperty("Grade")]
            public string GradeName;
            [JsonIgnore]
            public BuildingGrade.Enum Grade;
            [JsonProperty("Skin")]
            public ulong? Skin;
        }

    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void DeleteOldData();
    private void ConvertOldData();
    private PluginData LoadOldData();
    private class PluginData
    {
        [JsonProperty(PropertyName = "Player Preferences")]
        public Dictionary<string, OldPreference> Preferences;
        [Serializable]
        public class OldPreference
        {
            public float DisableIn;
            public bool AutoGrade;
        }

    }

    private abstract class SplitDatafile
    {
        public static Dictionary<string, T> LoadedData;
        protected static string[] GetFiles(string baseFolder);
        protected static T Save(string baseFolder, string filename);
        protected static T Get(string baseFolder, string filename);
        protected static T GetOrLoad(string baseFolder, string filename);
        protected static T GetOrCreate(string baseFolder, string path);
        protected virtual T SetDefaults();
    }

    private class PlayerPreferences : SplitDatafile<PlayerPreferences>
    {
        [JsonIgnore]
        public BuildingGrade.Enum Grade;
        [JsonIgnore]
        public ulong Skin;
        [JsonIgnore]
        public DateTime? LastUsed;
        [JsonIgnore]
        public bool Enabled { get; set; }
        public float? DisableIn;
        public bool AutoGrade;
        public void ResetState();
        public static readonly string BaseFolder;
        public static string[] GetFiles();
        public static PlayerPreferences Save(string filename);
        public static PlayerPreferences Get(string filename);
        public static PlayerPreferences GetOrLoad(string filename);
        public static PlayerPreferences GetOrCreate(string filename);
        protected override PlayerPreferences SetDefaults();
    }

    protected override void LoadDefaultMessages();
    private void Init();
    private void Unload();
    private void OnPlayerDisconnected(BasePlayer basePlayer);
    private object OnHammerHit(BasePlayer basePlayer, HitInfo info);
    private void OnEntityBuilt(Planner planner, GameObject gameObject);
    private void OnStructureUpgrade(BuildingBlock block, BasePlayer basePlayer, BuildingGrade.Enum grade, ulong skin);
    private void OnPlayerInput(BasePlayer basePlayer, InputState input);
    private void CommandGrade(IPlayer player, string command, string[] args);
    private Dictionary<string, Timer> _availabilityTimers;
    private void UpdateTimeout(BasePlayer basePlayer, PlayerPreferences preferences);
    private void CancelTimeoutUpdate(string id);
    private void SetActiveGrade(BasePlayer basePlayer, PlayerPreferences preferences, BuildingGrade.Enum grade, ulong skin, bool suppressMessages);
    private void SetActive(BasePlayer basePlayer, PlayerPreferences preferences, bool isActive);
    private void Upgrade(BasePlayer basePlayer, PlayerPreferences preferences, BuildingBlock block, bool suppressMessages);
    private bool IsGradeEnabled(BuildingGrade.Enum grade, ulong skin);
    private static bool CanUse(IPlayer player, GradingType type);
    private static string GetMsg(string key, string userId);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public string[] Commands;
    [JsonProperty("Allowed Grades", ItemConverterType = typeof(StringEnumConverter),
                NullValueHandling = NullValueHandling.Ignore)]
    public BuildingGrade.Enum[] OldGradesAllowed;
    [JsonProperty("Grades Enabled", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<GradeConfig> GradesEnabled;
    [JsonProperty(PropertyName = "Distance From Entity")]
    public float Distance;
    [JsonProperty(PropertyName = "Allow Auto Grade")]
    public bool AutoGrade;
    [JsonProperty(PropertyName = "Send Downgrade Disabled Message")]
    public bool DowngradeMessage;
    [JsonProperty(PropertyName = "Cancel Default Hammer Hit Behavior")]
    public bool CancelHammerHitDefaultBehavior;
    [JsonProperty(PropertyName = "Default Preferences")]
    public PlayerPreferences Preferences;
    public class GradeConfig
    {
        [JsonProperty("Grade")]
        public string GradeName;
        [JsonIgnore]
        public BuildingGrade.Enum Grade;
        [JsonProperty("Skin")]
        public ulong? Skin;
    }

}

public class GradeConfig
{
    [JsonProperty("Grade")]
    public string GradeName;
    [JsonIgnore]
    public BuildingGrade.Enum Grade;
    [JsonProperty("Skin")]
    public ulong? Skin;
}

private class PluginData
{
    [JsonProperty(PropertyName = "Player Preferences")]
    public Dictionary<string, OldPreference> Preferences;
    [Serializable]
    public class OldPreference
    {
        public float DisableIn;
        public bool AutoGrade;
    }

}

[Serializable]
public class OldPreference
{
    public float DisableIn;
    public bool AutoGrade;
}

private abstract class SplitDatafile
{
    public static Dictionary<string, T> LoadedData;
    protected static string[] GetFiles(string baseFolder);
    protected static T Save(string baseFolder, string filename);
    protected static T Get(string baseFolder, string filename);
    protected static T GetOrLoad(string baseFolder, string filename);
    protected static T GetOrCreate(string baseFolder, string path);
    protected virtual T SetDefaults();
}

private class PlayerPreferences : SplitDatafile<PlayerPreferences>
{
    [JsonIgnore]
    public BuildingGrade.Enum Grade;
    [JsonIgnore]
    public ulong Skin;
    [JsonIgnore]
    public DateTime? LastUsed;
    [JsonIgnore]
    public bool Enabled { get; set; }
    public float? DisableIn;
    public bool AutoGrade;
    public void ResetState();
    public static readonly string BaseFolder;
    public static string[] GetFiles();
    public static PlayerPreferences Save(string filename);
    public static PlayerPreferences Get(string filename);
    public static PlayerPreferences GetOrLoad(string filename);
    public static PlayerPreferences GetOrCreate(string filename);
    protected override PlayerPreferences SetDefaults();
}


```

---

## EntityCleanup by 2CHEVSKII - Easy way to cleanup your server from unnecessary entities.

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Facepunch;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;

Oxide.Plugins
[Info("Entity Cleanup", "2CHEVSKII", "4.0.0")]
[Description("Easy way to cleanup your server from unnecessary entities.")]
public class EntityCleanup : CovalencePlugin
{
    const string PERMISSION_USE;
    const string M_PREFIX;
    const string M_NO_PERMISSION;
    const string M_INVALID_USAGE;
    const string M_CLEANUP_STARTED;
    const string M_CLEANUP_FINISHED;
    const string M_CLEANUP_RUNNING;
     PluginSettings settings;
     CleanupHandler handler;
     void CommandHandler(IPlayer player, string command, string[] args);
     void Init();
     void OnServerInitialized();
     void Unload();
    protected override void LoadDefaultMessages();
     string GetMessage(IPlayer player, string langKey);
     void Message(IPlayer player, string langKey, object[] args);
     void Announce(string langKey, object[] args);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     class CleanupHandler : MonoBehaviour
    {
         PluginSettings settings;
         List<BaseNetworkable> entityList;
         Coroutine cleanupRoutine;
         HashSet<string> deployables;
         bool IsCleanupRunning { get; set; }
        public void Init(EntityCleanup plugin);
        public void Shutdown();
        public bool TryStartCleanup();
         void Start();
         void InitializeTimedCleanup();
         void StartCleanup();
         IEnumerator Cleanup();
         bool IsCleanupCandidate(BaseEntity entity);
    }

     class PluginSettings
    {
        public static PluginSettings Default { get; set; }
        public int Interval { get; set; }
        public bool CleanupBuildings { get; set; }
        public bool CleanupDeployables { get; set; }
        public bool RemoveOutsidePrivilege { get; set; }
        public float OutsideHealthFractionTheshold { get; set; }
        public bool RemoveInsidePrivilege { get; set; }
        public float InsideHealthFractionTheshold { get; set; }
        public string[] Whitelist { get; set; }
        public bool CheckOwnerIdPrivilegeAuthorized { get; set; }
    }

}

 class CleanupHandler : MonoBehaviour
{
     PluginSettings settings;
     List<BaseNetworkable> entityList;
     Coroutine cleanupRoutine;
     HashSet<string> deployables;
     bool IsCleanupRunning { get; set; }
    public void Init(EntityCleanup plugin);
    public void Shutdown();
    public bool TryStartCleanup();
     void Start();
     void InitializeTimedCleanup();
     void StartCleanup();
     IEnumerator Cleanup();
     bool IsCleanupCandidate(BaseEntity entity);
}

 class PluginSettings
{
    public static PluginSettings Default { get; set; }
    public int Interval { get; set; }
    public bool CleanupBuildings { get; set; }
    public bool CleanupDeployables { get; set; }
    public bool RemoveOutsidePrivilege { get; set; }
    public float OutsideHealthFractionTheshold { get; set; }
    public bool RemoveInsidePrivilege { get; set; }
    public float InsideHealthFractionTheshold { get; set; }
    public string[] Whitelist { get; set; }
    public bool CheckOwnerIdPrivilegeAuthorized { get; set; }
}


```

---

## EntityData by  - Saving specified information about player-owned entities

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[Info("Entity Data", "Orange", "1.0.3")]
[Description("Saving specified information about player-owned entities")]
public class EntityData : RustPlugin
{
    private class OEntity
    {
        public ulong ownerID;
        public double spawnTime;
    }

    private void OnServerInitialized();
    private void Unload();
    private void OnNewSave();
    private void OnEntitySpawned(BaseEntity entity);
    private void OnEntityKill(BaseEntity entity);
    private void CheckEntity(BaseEntity entity);
    private double Now();
    private int Passed(double a);
    private Dictionary<uint, OEntity> entities;
    private const string filename;
    private void LoadData();
    private void SaveData();
    private int API_GetLifeDuration(uint netID);
    private double API_GetSpawnTime(uint netID);
    private ulong API_GetOwner(uint netID);
}

private class OEntity
{
    public ulong ownerID;
    public double spawnTime;
}


```

---

## EntityItemPicker by 0x89A - Get the item of any deployable with a single button press

```csharp
using System;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Entity Item Picker", "0x89A", "1.0.1")]
[Description("Get the item of any deployable with a single button press")]
 class EntityItemPicker : RustPlugin
{
    private Configuration _config;
    private const string use;
    private const string bypassCooldown;
    private Regex _removeRegex;
    private Dictionary<string, string> _prefabToItemShortname;
    private Dictionary<ulong, float> _lastUseTime;
     void Init();
    protected override void LoadDefaultMessages();
     void OnPlayerInput(BasePlayer player, InputState input);
    private ItemDefinition GetDefinition(string[] comparisons);
    private string GetStringFromDict(string key);
    private class Configuration
    {
        [JsonProperty("Use cooldown")]
        public bool useCooldown;
        [JsonProperty("Cooldown duration")]
        public float cooldownTime;
        [JsonProperty("Maximum distance")]
        public float maxDist;
        [JsonProperty("Kill on pickup when holding sprint")]
        public bool killWhenShifting;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class Configuration
{
    [JsonProperty("Use cooldown")]
    public bool useCooldown;
    [JsonProperty("Cooldown duration")]
    public float cooldownTime;
    [JsonProperty("Maximum distance")]
    public float maxDist;
    [JsonProperty("Kill on pickup when holding sprint")]
    public bool killWhenShifting;
}


```

---

## EntityLimit by TheFriendlyChap - Limit entities per player or building

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Facepunch.Extend;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Entity Limit", "Orange/The Friendly Chap", "2.1.5")]
[Description("Limit entities per player or building")]
public class EntityLimit : RustPlugin
{
    private Coroutine lookup;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private object CanBuild(Planner planner, Construction entity, Construction.Target target);
    private void OnEntitySpawned(BaseEntity entity);
    private void OnEntityKill(BaseEntity entity);
    private void cmdControlConsole(ConsoleSystem.Arg arg);
    [ChatCommand("testmessage")]
    private void TestMessage(BasePlayer player);
    private void cmdControlChat(BasePlayer player);
    private object CheckBuild(BasePlayer player, string fullName);
    private void CheckLifeState(BaseEntity entity, bool dying);
    private static string GetShortname(string original);
    private static bool SameName(string original, string name1, string name2);
    private void CheckAllEntities();
    private IEnumerator LookupEntities();
    private Dictionary<string, PermissionEntry> cachePermission;
    private class PermissionEntry
    {
        public PermissionEntry GetClone();
        [JsonProperty(PropertyName = "Permission")]
        public string permission;
        [JsonProperty(PropertyName = "Priority")]
        public int priority;
        [JsonProperty(PropertyName = "Limits Global")]
        public Dictionary<string, int> limitsGlobal;
        [JsonProperty(PropertyName = "Limits Building")]
        public Dictionary<string, int> limitsBuilding;
    }

    private PermissionEntry GetPermissionFromPlayerID(string playerID, PermissionEntry[] permissions);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Commands")]
        public string[] commands;
        [JsonProperty(PropertyName = "Permission cache time (seconds)")]
        public int cacheTime;
        [JsonProperty(PropertyName = "Warn about limits every X entities")]
        public int warnCount;
        [JsonProperty(PropertyName = "Permissions")]
        public PermissionEntry[] permissions;
    }

    protected override void LoadConfig();
    private static void ValidateConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private Dictionary<object, string> langMessages;
    protected override void LoadDefaultMessages();
    private string GetMessage(MessageType key, string playerID, object[] args);
    private static Dictionary<string, object> OrganizeArgs(object[] args);
    private static string ReplaceArgs(string message, Dictionary<string, object> args);
    private void SendMessage(object receiver, MessageType key, object[] args);
    private void SendMessage(object receiver, string message);
    private static PluginData Data;
    private class DataEntry
    {
        public HashSet<EntityData> entities;
    }

    private class EntityData
    {
        public string prefab;
        public int count { get; set; }
        [JsonIgnore]
        public HashSet<BaseEntity> list;
    }

    private class PluginData
    {
        [JsonProperty]
        private Dictionary<string, DataEntry> values;
        [JsonIgnore]
        private Dictionary<string, DataEntry> cache;
        public DataEntry Get(object param, bool createNewOnMissing);
        public void Set(object param, DataEntry value);
        private static string GetKeyFrom(object obj);
    }

}

private class PermissionEntry
{
    public PermissionEntry GetClone();
    [JsonProperty(PropertyName = "Permission")]
    public string permission;
    [JsonProperty(PropertyName = "Priority")]
    public int priority;
    [JsonProperty(PropertyName = "Limits Global")]
    public Dictionary<string, int> limitsGlobal;
    [JsonProperty(PropertyName = "Limits Building")]
    public Dictionary<string, int> limitsBuilding;
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Commands")]
    public string[] commands;
    [JsonProperty(PropertyName = "Permission cache time (seconds)")]
    public int cacheTime;
    [JsonProperty(PropertyName = "Warn about limits every X entities")]
    public int warnCount;
    [JsonProperty(PropertyName = "Permissions")]
    public PermissionEntry[] permissions;
}

private class DataEntry
{
    public HashSet<EntityData> entities;
}

private class EntityData
{
    public string prefab;
    public int count { get; set; }
    [JsonIgnore]
    public HashSet<BaseEntity> list;
}

private class PluginData
{
    [JsonProperty]
    private Dictionary<string, DataEntry> values;
    [JsonIgnore]
    private Dictionary<string, DataEntry> cache;
    public DataEntry Get(object param, bool createNewOnMissing);
    public void Set(object param, DataEntry value);
    private static string GetKeyFrom(object obj);
}


```

---

## EntityOwner by Calytic - Show and modify entity ownership and cupboard/turret authorization

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Facepunch;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;

Oxide.Plugins
[Info("Entity Owner", "Calytic", "3.4.1")]
[Description("Modify entity ownership and cupboard/turret authorization")]
 class EntityOwner : RustPlugin
{
    readonly int layerMasks;
     int EntityLimit;
     float DistanceThreshold;
     float CupboardDistanceThreshold;
     bool debug;
     Dictionary<string, string> texts;
    protected override void LoadDefaultConfig();
    new void LoadDefaultMessages();
    protected void ReloadConfig();
     T GetConfig(string name, T defaultValue);
     string GetMsg(string key, BasePlayer player);
     void OnServerInitialized();
     void LoadData();
    [HookMethod("SendHelpText")]
     void SendHelpText(BasePlayer player);
    [ChatCommand("prod")]
     void cmdProd(BasePlayer player, string command, string[] args);
    [ChatCommand("setowner")]
     void cmdSetowner(BasePlayer player, string command, string[] args);
    [ChatCommand("own")]
     void cmdOwn(BasePlayer player, string command, string[] args);
    [ChatCommand("unown")]
     void cmdUnown(BasePlayer player, string command, string[] args);
    [ChatCommand("auth")]
     void cmdAuth(BasePlayer player, string command, string[] args);
    [ChatCommand("deauth")]
     void cmdDeauth(BasePlayer player, string command, string[] args);
    [ChatCommand("prod2")]
     void cmdProd2(BasePlayer player, string command, string[] args);
     bool canCheckOwners(BasePlayer player);
     bool canCheckCodes(BasePlayer player);
     bool canCheckAssignee(BasePlayer player);
     bool canSeeDetails(BasePlayer player);
     bool canChangeOwners(BasePlayer player);
     bool TryGetEntity(BasePlayer player, BaseEntity entity);
     void massChangeOwner(BasePlayer player, ulong target);
     void massProd(BasePlayer player, bool highlight, string[] filter);
     void SendHighlight(BasePlayer player, Vector3 position);
     void ProdCupboard(BasePlayer player, BuildingPrivlidge cupboard);
     void ProdTurret(BasePlayer player, AutoTurret turret);
     void massProdCupboard(BasePlayer player, bool highlight);
     void massProdTurret(BasePlayer player, bool highlight);
     void massCupboardAuthorize(BasePlayer player, BasePlayer target);
     void massCupboardDeauthorize(BasePlayer player, BasePlayer target);
     void massTurretAuthorize(BasePlayer player, BasePlayer target);
     void massTurretDeauthorize(BasePlayer player, BasePlayer target);
     bool TryGetCupboardUserNames(BuildingPrivlidge cupboard, List<string> names);
     bool TryGetTurretUserNames(AutoTurret turret, List<string> names);
     bool HasCupboardAccess(BuildingPrivlidge cupboard, BasePlayer player);
     bool HasTurretAccess(AutoTurret turret, BasePlayer player);
     string GetOwnerName(BaseEntity entity);
     BasePlayer GetOwnerPlayer(BaseEntity entity);
     IPlayer GetOwnerIPlayer(BaseEntity entity);
     void RemoveOwner(BaseEntity entity);
     void ChangeOwner(BaseEntity entity, object player);
     object FindEntityData(BaseEntity entity);
     object RaycastAll(Vector3 position, Vector3 aim);
     object RaycastAll(Ray ray);
     object FindBuilding(Vector3 position, float distance);
     object FindEntity(Vector3 position, float distance, string[] filter);
     T FindEntity(Vector3 position, float distance, string[] filter);
     List<T> FindEntities(Vector3 position, float distance);
     List<BuildingBlock> GetProfileConstructions(BasePlayer player);
     List<BaseEntity> GetProfileDeployables(BasePlayer player);
     void ClearProfile(BasePlayer player);
     string FindPlayerName(ulong playerID);
     string GetOwnerDisplayName(BaseEntity entity);
     ulong FindUserIDByPartialName(string name);
     BasePlayer FindPlayerByPartialName(string name);
}


```

---

## EntityReducer by MrBlue - Controls all spawn populations on the server

```csharp
using System;
using System.Collections.Generic;
using System.Text;
using ConVar;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Entity Reducer", "Arainrr", "2.1.4")]
[Description("Controls all spawn populations on the server")]
public class EntityReducer : RustPlugin
{
    private void OnServerInitialized();
    private void UpdateConfig();
    private void ApplySpawnHandler();
    public string GetReport();
    [ConsoleCommand("er.fillpopulations")]
    private void CmdFillPopulations(ConsoleSystem.Arg arg);
    [ConsoleCommand("er.getreport")]
    private void CmdGetReport(ConsoleSystem.Arg arg);
    [ConsoleCommand("er.enforcelimits")]
    private void CmdEnforceLimits(ConsoleSystem.Arg arg);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Enabled Plugin")]
        public bool pluginEnabled;
        [JsonProperty(PropertyName = "Population Settings")]
        public Dictionary<string, PopulationSetting> populationSettings;
    }

    private class PopulationSetting
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool enabled;
        [JsonProperty(PropertyName = "Target Count")]
        public int targetCount;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Enabled Plugin")]
    public bool pluginEnabled;
    [JsonProperty(PropertyName = "Population Settings")]
    public Dictionary<string, PopulationSetting> populationSettings;
}

private class PopulationSetting
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool enabled;
    [JsonProperty(PropertyName = "Target Count")]
    public int targetCount;
}


```

---

## EntityScaleManager by WhiteThunder - Allows players and other plugins to resize entities

```csharp
using Network;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using UnityEngine;

[Info("Entity Scale Manager", "WhiteThunder", "2.1.5")]
[Description("Utilities for resizing entities.")]
internal class EntityScaleManager : CovalencePlugin
{
    [PluginReference]
    private readonly Plugin ParentedEntityRenderFix;
    private Configuration _config;
    private StoredData _data;
    private const string PermissionScaleUnrestricted;
    private const string SpherePrefab;
    private readonly object True;
    private const float ExpectedResizeDuration;
    private EntitySubscriptionManager _entitySubscriptionManager;
    private NetworkSnapshotManager _networkSnapshotManager;
    private void Init();
    private void Unload();
    private void OnServerInitialized();
    private void OnServerSave();
    private void OnNewSave();
    private void OnEntityKill(BaseEntity entity);
    private void OnPlayerDisconnected(BasePlayer player);
    private object OnEntitySnapshot(BaseEntity entity, Connection connection);
    private void OnNetworkGroupLeft(BasePlayer player, Network.Visibility.Group group);
    private Tuple<BaseEntity, BaseEntity> OnTelekinesisStart(BasePlayer player, BaseEntity entity);
    private string CanStartTelekinesis(BasePlayer player, SphereEntity moveEntity, BaseEntity rotateEntity);
    [HookMethod(nameof(API_GetScale))]
    public float API_GetScale(BaseEntity entity);
    [HookMethod(nameof(API_ScaleEntity))]
    public bool API_ScaleEntity(BaseEntity entity, float scale);
    [HookMethod(nameof(API_ScaleEntity))]
    public bool API_ScaleEntity(BaseEntity entity, int scale);
    [HookMethod(nameof(API_RegisterScaledEntity))]
    public void API_RegisterScaledEntity(BaseEntity entity);
    [Command("scale")]
    private void CommandScale(IPlayer player, string cmd, string[] args);
    [Command("getscale")]
    private void CommandGetScale(IPlayer player);
    private static bool EntityScaleWasBlocked(BaseEntity entity, float scale);
    private static BaseEntity GetLookEntity(BasePlayer basePlayer, float maxDistance);
    private static void EnableGlobalBroadcastFixed(BaseEntity entity, bool wants);
    private static void SetupSphereEntity(SphereEntity sphereEntity, BaseEntity scaledEntity);
    private static void SetSphereSize(SphereEntity sphereEntity, float scale);
    private static SphereEntity CreateSphere(Vector3 position, Quaternion rotation, float scale, BaseEntity scaledEntity);
    private static SphereEntity GetParentSphere(BaseEntity entity);
    private void RefreshScaledEntity(BaseEntity scaledEntity, SphereEntity parentSphere);
    private void UnparentFromSphere(BaseEntity scaledEntity);
    private bool TryScaleEntity(BaseEntity entity, float scale);
    private static class NetworkUtils
    {
        public static void TerminateOnClient(BaseNetworkable entity, Connection connection);
        public static void SendUpdateImmediateRecursive(BaseEntity entity);
    }

    private class SimplePool
    {
        private List<T> _pool;
        public virtual T Get();
        public virtual void Free(T item);
        public void Clear();
    }

    private class SimpleDictionaryPool : SimplePool<Dictionary<TKey, TValue>>
    {
        public override void Free(Dictionary<TKey, TValue> dict);
    }

    private abstract class BaseNetworkSnapshotManager
    {
        private readonly Dictionary<NetworkableId, MemoryStream> _networkCache;
        public void Clear();
        public void InvalidateForEntity(NetworkableId entityId);
        public void SendModifiedSnapshot(BaseEntity entity, Connection connection);
        private void ToStream(BaseEntity entity, Stream stream, BaseNetworkable.SaveInfo saveInfo);
        private Stream ToStreamForNetwork(BaseEntity entity, Stream stream, BaseNetworkable.SaveInfo saveInfo);
        protected abstract void HandleOnEntitySaved(BaseEntity entity, BaseNetworkable.SaveInfo saveInfo);
    }

    private class NetworkSnapshotManager : BaseNetworkSnapshotManager
    {
        protected override void HandleOnEntitySaved(BaseEntity entity, BaseNetworkable.SaveInfo saveInfo);
    }

    private class EntitySubscriptionManager
    {
        private SimpleDictionaryPool<ulong, ResizeState> _dictPool;
        private readonly Dictionary<NetworkableId, Dictionary<ulong, ResizeState>> _networkResizeState;
        public void Clear();
        public void InitResized(NetworkableId entityId, ulong userId);
        public ResizeState GetResizeState(NetworkableId entityId, ulong userId);
        public bool DoneResizing(NetworkableId entityId, ulong userId);
        public void RemoveEntitySubscription(NetworkableId entityId, ulong userId);
        public void RemoveEntity(NetworkableId entityId);
        public void RemoveSubscriber(ulong userId);
        private Dictionary<ulong, ResizeState> EnsureEntity(NetworkableId entityId);
    }

    private class StoredData
    {
        [JsonProperty("ScaledEntities")]
        public HashSet<ulong> ScaledEntities;
        public static StoredData Load();
        public static StoredData Clear();
        public StoredData Save();
    }

    private class Configuration : SerializableConfiguration
    {
        [JsonProperty("Hide spheres after resize (performance intensive)")]
        public bool HideSpheresAfterResize;
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
    private void ChatMessage(BasePlayer player, string messageName, object[] args);
    private string GetMessage(IPlayer player, string messageName, object[] args);
    private string GetMessage(BasePlayer player, string messageName, object[] args);
    private void ReplyToPlayer(IPlayer player, string messageName, object[] args);
    protected override void LoadDefaultMessages();
}

private static class NetworkUtils
{
    public static void TerminateOnClient(BaseNetworkable entity, Connection connection);
    public static void SendUpdateImmediateRecursive(BaseEntity entity);
}

private class SimplePool
{
    private List<T> _pool;
    public virtual T Get();
    public virtual void Free(T item);
    public void Clear();
}

private class SimpleDictionaryPool : SimplePool<Dictionary<TKey, TValue>>
{
    public override void Free(Dictionary<TKey, TValue> dict);
}

private abstract class BaseNetworkSnapshotManager
{
    private readonly Dictionary<NetworkableId, MemoryStream> _networkCache;
    public void Clear();
    public void InvalidateForEntity(NetworkableId entityId);
    public void SendModifiedSnapshot(BaseEntity entity, Connection connection);
    private void ToStream(BaseEntity entity, Stream stream, BaseNetworkable.SaveInfo saveInfo);
    private Stream ToStreamForNetwork(BaseEntity entity, Stream stream, BaseNetworkable.SaveInfo saveInfo);
    protected abstract void HandleOnEntitySaved(BaseEntity entity, BaseNetworkable.SaveInfo saveInfo);
}

private class NetworkSnapshotManager : BaseNetworkSnapshotManager
{
    protected override void HandleOnEntitySaved(BaseEntity entity, BaseNetworkable.SaveInfo saveInfo);
}

private class EntitySubscriptionManager
{
    private SimpleDictionaryPool<ulong, ResizeState> _dictPool;
    private readonly Dictionary<NetworkableId, Dictionary<ulong, ResizeState>> _networkResizeState;
    public void Clear();
    public void InitResized(NetworkableId entityId, ulong userId);
    public ResizeState GetResizeState(NetworkableId entityId, ulong userId);
    public bool DoneResizing(NetworkableId entityId, ulong userId);
    public void RemoveEntitySubscription(NetworkableId entityId, ulong userId);
    public void RemoveEntity(NetworkableId entityId);
    public void RemoveSubscriber(ulong userId);
    private Dictionary<ulong, ResizeState> EnsureEntity(NetworkableId entityId);
}

private class StoredData
{
    [JsonProperty("ScaledEntities")]
    public HashSet<ulong> ScaledEntities;
    public static StoredData Load();
    public static StoredData Clear();
    public StoredData Save();
}

private class Configuration : SerializableConfiguration
{
    [JsonProperty("Hide spheres after resize (performance intensive)")]
    public bool HideSpheresAfterResize;
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

## EntitySpawn by Wolfleader101 - Throw a projectile and find a surprise

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Entity Spawn", "Wolfleader101", "1.0.0")]
[Description("Throw a projectile and find a surprise!")]
 class EntitySpawn : RustPlugin
{
    private PluginConfig config;
    private void Init();
     void OnPlayerAttack(BasePlayer attacker, HitInfo info);
     void SpawnChecks(string perm, string projectileName, string EntName, KeyValuePair<string, spawnItemConfig> item, BasePlayer attacker, HitInfo info);
     void SpawnIn(BasePlayer attacker, HitInfo info, string PrefName);
    private class PluginConfig
    {
        [JsonProperty("Spawns")]
        public Dictionary<string, spawnItemConfig> NewItem { get; set; }
    }

    private PluginConfig GetDefaultConfig();
    protected override void LoadDefaultConfig();
}

private class PluginConfig
{
    [JsonProperty("Spawns")]
    public Dictionary<string, spawnItemConfig> NewItem { get; set; }
}


```

---

## EternalPlants by 0x89A - Plants do not die after they are full grown

```csharp
using System;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Eternal Plants", "0x89A", "1.0.1")]
[Description("Plants do not die after they are full grown")]
 class EternalPlants : RustPlugin
{
    private const string _usePerm;
    private void Init();
    private object OnGrowableStateChange(GrowableEntity entity, PlantProperties.State state);
    private bool HasPermission(ulong id);
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty("Ownerless plants don't die")]
        public bool eternalOwnerless;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class Configuration
{
    [JsonProperty("Ownerless plants don't die")]
    public bool eternalOwnerless;
}


```

---

## EventManager by k1lly0u - A versitile arena event plugin

```csharp
using System;
using System.Collections.Generic;
using System.Collections;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;
using System.Linq;
using Network;
using Facepunch;
using UI = Oxide.Plugins.EMInterface.UI;
using UI4 = Oxide.Plugins.EMInterface.UI4;
using EventManagerEx;

Oxide.Plugins
[Info("EventManager", "k1lly0u", "4.0.7")]
[Description("The core mechanics for arena combat games")]
public class EventManager : RustPlugin
{
    private DynamicConfigFile restorationData;
    private DynamicConfigFile eventData;
    [PluginReference]
    private Plugin Economics;
    private Plugin Kits;
    private Plugin NoEscape;
    private Plugin ServerRewards;
    private Plugin Spawns;
    private Plugin ZoneManager;
    private Timer _autoEventTimer;
    private RewardType rewardType;
    private int scrapItemId;
    private static Regex hexFilter;
    public Hash<string, IEventPlugin> EventModes { get; set; }
    public EventData Events { get; set; }
    private RestoreData Restore { get; set; }
    public static EventManager Instance { get; set; }
    public static BaseEventGame BaseManager { get; set; }
    public static ConfigData Configuration { get; set; }
    public static EventResults LastEventResult { get; set; }
    public static bool IsUnloading { get; set; }
    internal const string ADMIN_PERMISSION;
    private void Loaded();
    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private void Unload();
    private void OnServerSave();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnEntityTakeDamage(BaseEntity entity, HitInfo hitInfo);
    private object CanBeWounded(BasePlayer player, HitInfo hitInfo);
    private object OnPlayerDeath(BasePlayer player, HitInfo hitInfo);
    private object CanSpectateTarget(BasePlayer player, string name);
    private void OnEntityBuilt(Planner planner, GameObject gameObject);
    private void OnItemDeployed(Deployer deployer, BaseCombatEntity baseCombatEntity);
    private object OnCreateWorldProjectile(HitInfo hitInfo, Item item);
    private object CanDropActiveItem(BasePlayer player);
    private object OnPlayerCommand(BasePlayer player, string command, string[] args);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private int _nextEventIndex;
    private void QueueAutoEvent();
    private void InitializeAutoEvent();
    public static void RegisterEvent(string eventName, IEventPlugin plugin);
    public static void UnregisterEvent(string eventName);
    public object OpenEvent(string eventName);
    public static bool InitializeEvent(IEventPlugin plugin, EventConfig config);
    public IEventPlugin GetPlugin(string name);
    private bool CheckDependencies();
    private void UnsubscribeAll();
    private void SubscribeAll();
    private static void Broadcast(string key, object[] args);
    internal static bool IsValidHex(string s);
    public class BaseEventGame : MonoBehaviour
    {
        internal IEventPlugin Plugin { get; set; }
        internal EventConfig Config { get; set; }
        public EventStatus Status { get; set; }
        protected GameTimer Timer { get; set; }
        internal SpawnSelector _spawnSelectorA;
        internal SpawnSelector _spawnSelectorB;
        protected CuiElementContainer scoreContainer;
        internal List<BasePlayer> joiningPlayers;
        internal List<BaseEventPlayer> eventPlayers;
        internal List<ScoreEntry> scoreData;
        private List<BaseCombatEntity> _deployedObjects;
        private bool _isClosed;
        private double _startsAtTime;
        internal string TeamAColor { get; set; }
        internal string TeamBColor { get; set; }
        internal string TeamAClothing { get; set; }
        internal string TeamBClothing { get; set; }
        public bool GodmodeEnabled { get; set; }
        internal string EventInformation { get; set; }
        internal string EventStatus { get; set; }
        protected virtual void OnDestroy();
        internal virtual void InitializeEvent(IEventPlugin plugin, EventConfig config);
        internal virtual void OpenEvent();
        internal virtual void CloseEvent();
        internal virtual void PrestartEvent();
        protected virtual void StartEvent();
        internal virtual void EndEvent();
        internal bool IsOpen();
        internal bool CanJoinEvent(BasePlayer player);
        protected virtual string CanJoinEvent();
        internal virtual void JoinEvent(BasePlayer player, Team team);
        internal virtual void LeaveEvent(BasePlayer player);
        internal virtual void LeaveEvent(BaseEventPlayer eventPlayer);
        private IEnumerator CreateEventPlayers();
        protected virtual void CreateEventPlayer(BasePlayer player, Team team);
        protected virtual Team GetPlayerTeam(BasePlayer player);
        protected virtual BaseEventPlayer AddPlayerComponent(BasePlayer player);
        internal virtual void OnPlayerRespawn(BaseEventPlayer baseEventPlayer);
        internal void SpawnPlayer(BaseEventPlayer eventPlayer, bool giveKit, bool sleep);
        protected virtual void OnPlayerSpawned(BaseEventPlayer eventPlayer);
        protected void EjectAllPlayers();
        protected void RespawnAllPlayers();
        private bool HasMinimumRequiredPlayers();
        internal virtual bool CanDealEntityDamage(BaseEventPlayer attacker, BaseEntity entity, HitInfo hitInfo);
        protected virtual float GetDamageModifier(BaseEventPlayer eventPlayer);
        internal virtual void OnPlayerTakeDamage(BaseEventPlayer eventPlayer, HitInfo hitInfo);
        internal virtual void PrePlayerDeath(BaseEventPlayer eventPlayer, HitInfo hitInfo);
        internal virtual void OnEventPlayerDeath(BaseEventPlayer victim, BaseEventPlayer attacker, HitInfo hitInfo);
        protected virtual void DisplayKillToChat(BaseEventPlayer victim, string attackerName);
        protected void ProcessWinners();
        protected virtual void GetWinningPlayers(List<BaseEventPlayer> list);
        protected virtual bool CanDropBackpack();
        protected virtual bool CanGiveKit(BaseEventPlayer eventPlayer);
        protected virtual void OnKitGiven(BaseEventPlayer eventPlayer);
        internal List<string> GetAvailableKits(Team team);
        internal virtual void GetAdditionalEventDetails(List<KeyValuePair<string, object>> list, ulong playerId);
        internal virtual void GetSpectateTargets(List<BaseEventPlayer> list);
        private void UpdateDeadSpectateTargets(BaseEventPlayer victim);
        internal int GetAlivePlayerCount();
        internal int GetTeamCount(Team team);
        internal int GetTeamAliveCount(Team team);
        internal virtual int GetTeamScore(Team team);
        protected void BalanceTeams();
        internal void OnEntityDeployed(BaseCombatEntity entity);
        private void CleanupEntities();
        internal void UpdateScoreboard();
        protected void UpdateScoreboard(BaseEventPlayer eventPlayer);
        protected void UpdateScores();
        protected virtual void BuildScoreboard();
        protected virtual float GetFirstScoreValue(BaseEventPlayer eventPlayer);
        protected virtual float GetSecondScoreValue(BaseEventPlayer eventPlayer);
        protected virtual void SortScores(List<ScoreEntry> list);
        internal void BroadcastToPlayers(string key, object[] args);
        internal void BroadcastToPlayers(Func<string, ulong, string> GetMessage, string key, object[] args);
        internal void BroadcastToTeam(Team team, string key, string[] args);
        internal void BroadcastToPlayer(BaseEventPlayer eventPlayer, string message);
        private void BroadcastOpenEvent();
    }

    public class BaseEventPlayer : MonoBehaviour
    {
        protected float _respawnDurationRemaining;
        protected float _invincibilityEndsAt;
        private double _resetDamageTime;
        private List<ulong> _damageContributors;
        private bool _isOOB;
        private int _oobTime;
        private int _spectateIndex;
        internal BasePlayer Player { get; set; }
        internal BaseEventGame Event { get; set; }
        internal Team Team { get; set; }
        internal int Kills { get; set; }
        internal int Deaths { get; set; }
        internal bool IsDead { get; set; }
        internal bool AutoRespawn { get; set; }
        internal bool CanRespawn { get; set; }
        internal int RespawnRemaining { get; set; }
        internal bool IsInvincible { get; set; }
        internal BaseEventPlayer SpectateTarget { get; set; }
        internal string Kit { get; set; }
        internal bool IsSelectingClass { get; set; }
        internal bool IsOutOfBounds { get; set; }
        private void Awake();
        private void OnDestroy();
        internal void ResetPlayer();
        internal void ForceSelectClass();
        protected void RespawnTick();
        internal void OnKilledPlayer(HitInfo hitInfo);
        internal virtual void OnPlayerDeath(BaseEventPlayer attacker, float respawnTime);
        internal void AddPlayerDeath(BaseEventPlayer attacker);
        protected void ApplyAssistPoints(BaseEventPlayer attacker);
        internal void ApplyInvincibility();
        protected void TickOutOfBounds();
        internal void DropInventory();
        internal void RemoveFromNetwork();
        internal void AddToNetwork();
        internal void OnTakeDamage(ulong attackerId);
        internal List<ulong> DamageContributors { get; set; }
        public void BeginSpectating();
        public void FinishSpectating();
        public void SetSpectateTarget(BaseEventPlayer eventPlayer);
        public void UpdateSpectateTarget();
        private List<string> _openPanels;
        internal void AddUI(string panel, CuiElementContainer container);
        internal void DestroyUI();
        internal void DestroyUI(string panel);
    }

    public class GameTimer
    {
        private BaseEventGame _owner;
        private string _message;
        private int _timeRemaining;
        private Action _callback;
        internal GameTimer(BaseEventGame owner);
        internal void StartTimer(int time, string message, Action callback);
        internal void StopTimer();
        private void TimerTick();
        private void UpdateTimer();
    }

    internal class SpawnSelector
    {
        private List<Vector3> _defaultSpawns;
        private List<Vector3> _availableSpawns;
        internal SpawnSelector(string eventName, string spawnFile);
        internal Vector3 GetSpawnPoint();
        internal Vector3 ReserveSpawnPoint(int index);
        internal void Destroy();
    }

    public class EventConfig
    {
        public string EventName { get; set; }
        public string EventType { get; set; }
        public string ZoneID { get; set; }
        public int TimeLimit { get; set; }
        public int ScoreLimit { get; set; }
        public int MinimumPlayers { get; set; }
        public int MaximumPlayers { get; set; }
        public bool AllowClassSelection { get; set; }
        public TeamConfig TeamConfigA { get; set; }
        public TeamConfig TeamConfigB { get; set; }
        public Hash<string, object> AdditionalParams { get; set; }
        public EventConfig();
        public EventConfig(string type, IEventPlugin eventPlugin);
        public T GetParameter(string key);
        public string GetString(string fieldName);
        public List<string> GetList(string fieldName);
        public class TeamConfig
        {
            public string Color { get; set; }
            public string Spawnfile { get; set; }
            public List<string> Kits { get; set; }
            public string Clothing { get; set; }
        }

        [JsonIgnore]
        public IEventPlugin Plugin { get; set; }
    }

    private void GiveReward(BaseEventPlayer baseEventPlayer, int amount);
    private T ParseType(string type);
    private object IsEventPlayer(BasePlayer player);
    private object isEventPlayer(BasePlayer player);
    internal static BaseEventPlayer GetUser(BasePlayer player);
    internal static void MovePosition(BasePlayer player, Vector3 destination, bool sleep);
    internal static void LockInventory(BasePlayer player);
    internal static void UnlockInventory(BasePlayer player);
    internal static void StripInventory(BasePlayer player);
    internal static void ResetMetabolism(BasePlayer player);
    internal static void GiveKit(BasePlayer player, string kitname);
    internal static void ClearDamage(HitInfo hitInfo);
    internal static void ResetPlayer(BasePlayer player);
    internal static void RespawnPlayer(BaseEventPlayer eventPlayer);
    internal static string StripTags(string str);
    internal static string TrimToSize(string str, int size);
    private void OnExitZone(string zoneId, BasePlayer player);
    private void OnEnterZone(string zoneId, BasePlayer player);
    internal object ValidateEventConfig(EventConfig eventConfig);
    internal object ValidateSpawnFile(string name);
    internal object ValidateZoneID(string name);
    internal object ValidateKit(string name);
    public class EventResults
    {
        public string EventName { get; set; }
        public string EventType { get; set; }
        public ScoreEntry TeamScore { get; set; }
        public IEventPlugin Plugin { get; set; }
        public List<ScoreEntry> Scores { get; set; }
        public bool IsValid { get; set; }
        public void UpdateFromEvent(BaseEventGame baseEventGame);
    }

    [ChatCommand("event")]
    private void cmdEvent(BasePlayer player, string command, string[] args);
    public class ConfigData
    {
        [JsonProperty(PropertyName = "Auto-Event Options")]
        public AutoEventOptions AutoEvents { get; set; }
        [JsonProperty(PropertyName = "Event Options")]
        public EventOptions Event { get; set; }
        [JsonProperty(PropertyName = "Reward Options")]
        public RewardOptions Reward { get; set; }
        [JsonProperty(PropertyName = "Timer Options")]
        public TimerOptions Timer { get; set; }
        [JsonProperty(PropertyName = "Message Options")]
        public MessageOptions Message { get; set; }
        public class EventOptions
        {
            [JsonProperty(PropertyName = "Blacklisted commands for event players")]
            public string[] CommandBlacklist { get; set; }
        }

        public class RewardOptions
        {
            [JsonProperty(PropertyName = "Amount rewarded for kills")]
            public int KillAmount { get; set; }
            [JsonProperty(PropertyName = "Amount rewarded for wins")]
            public int WinAmount { get; set; }
            [JsonProperty(PropertyName = "Amount rewarded for headshots")]
            public int HeadshotAmount { get; set; }
            [JsonProperty(PropertyName = "Reward type (ServerRewards, Economics, Scrap)")]
            public string Type { get; set; }
        }

        public class TimerOptions
        {
            [JsonProperty(PropertyName = "Match start timer (seconds)")]
            public int Start { get; set; }
            [JsonProperty(PropertyName = "Match pre-start timer (seconds)")]
            public int Prestart { get; set; }
            [JsonProperty(PropertyName = "Backpack despawn timer (seconds)")]
            public int Bag { get; set; }
        }

        public class MessageOptions
        {
            [JsonProperty(PropertyName = "Announce events when one opens")]
            public bool Announce { get; set; }
            [JsonProperty(PropertyName = "Event announcement interval (seconds)")]
            public int AnnounceInterval { get; set; }
            [JsonProperty(PropertyName = "Broadcast when a player joins an event to chat")]
            public bool BroadcastJoiners { get; set; }
            [JsonProperty(PropertyName = "Broadcast when a player leaves an event to chat")]
            public bool BroadcastLeavers { get; set; }
            [JsonProperty(PropertyName = "Broadcast the name(s) of the winning player(s) to chat")]
            public bool BroadcastWinners { get; set; }
            [JsonProperty(PropertyName = "Broadcast kills to chat")]
            public bool BroadcastKills { get; set; }
            [JsonProperty(PropertyName = "Chat icon Steam ID")]
            public ulong ChatIcon { get; set; }
        }

        public class AutoEventOptions
        {
            [JsonProperty(PropertyName = "Enable auto-events")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "List of event configs to run through")]
            public string[] Events { get; set; }
            [JsonProperty(PropertyName = "Randomize auto-event selection")]
            public bool Randomize { get; set; }
            [JsonProperty(PropertyName = "Auto-event interval (seconds)")]
            public int Interval { get; set; }
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    internal void SaveEventData();
    private void SaveRestoreData();
    private void LoadData();
    public static void SaveEventConfig(EventConfig eventConfig);
    public class EventData
    {
        public Hash<string, EventConfig> events;
    }

    public class EventParameter
    {
        public string Name;
        public InputType Input;
        public string Field;
        public string DataType;
        public bool IsRequired;
        public string SelectorHook;
        public bool SelectMultiple;
        public object DefaultValue;
        [JsonIgnore]
        public bool IsList { get; set; }
    }

    public class RestoreData
    {
        public Hash<ulong, PlayerData> Restore;
        internal void AddData(BasePlayer player);
        public void AddPrizeToData(ulong playerId, int itemId, int amount);
        private ItemData FindItem(PlayerData playerData, int itemId);
        internal void RemoveData(ulong playerId);
        internal bool HasRestoreData(ulong playerId);
        internal void RestorePlayer(BasePlayer player);
        private void RestoreAllItems(BasePlayer player, PlayerData playerData);
        private bool RestoreItems(BasePlayer player, ItemData[] itemData, Container type);
        public class PlayerData
        {
            public float[] stats;
            public Vector3 position;
            public ItemData[] containerMain;
            public ItemData[] containerWear;
            public ItemData[] containerBelt;
            public PlayerData();
            public PlayerData(BasePlayer player);
            private IEnumerable<ItemData> GetItems(ItemContainer container);
            private float[] GetStats(BasePlayer player);
            internal void SetStats(BasePlayer player);
        }

    }

    internal static Item CreateItem(ItemData itemData);
    internal static ItemData SerializeItem(Item item);
    public class ItemData
    {
        public int itemid;
        public ulong skin;
        public int amount;
        public float condition;
        public float maxCondition;
        public int ammo;
        public string ammotype;
        public int position;
        public int frequency;
        public InstanceData instanceData;
        public ItemData[] contents;
        public class InstanceData
        {
            public int dataInt;
            public int blueprintTarget;
            public int blueprintAmount;
            public uint subEntity;
            public InstanceData();
            public InstanceData(Item item);
            public void Restore(Item item);
            public bool IsValid();
        }

    }

    public static string Message(string key, ulong playerId);
    private readonly Dictionary<string, string> Messages;
}

public class BaseEventGame : MonoBehaviour
{
    internal IEventPlugin Plugin { get; set; }
    internal EventConfig Config { get; set; }
    public EventStatus Status { get; set; }
    protected GameTimer Timer { get; set; }
    internal SpawnSelector _spawnSelectorA;
    internal SpawnSelector _spawnSelectorB;
    protected CuiElementContainer scoreContainer;
    internal List<BasePlayer> joiningPlayers;
    internal List<BaseEventPlayer> eventPlayers;
    internal List<ScoreEntry> scoreData;
    private List<BaseCombatEntity> _deployedObjects;
    private bool _isClosed;
    private double _startsAtTime;
    internal string TeamAColor { get; set; }
    internal string TeamBColor { get; set; }
    internal string TeamAClothing { get; set; }
    internal string TeamBClothing { get; set; }
    public bool GodmodeEnabled { get; set; }
    internal string EventInformation { get; set; }
    internal string EventStatus { get; set; }
    protected virtual void OnDestroy();
    internal virtual void InitializeEvent(IEventPlugin plugin, EventConfig config);
    internal virtual void OpenEvent();
    internal virtual void CloseEvent();
    internal virtual void PrestartEvent();
    protected virtual void StartEvent();
    internal virtual void EndEvent();
    internal bool IsOpen();
    internal bool CanJoinEvent(BasePlayer player);
    protected virtual string CanJoinEvent();
    internal virtual void JoinEvent(BasePlayer player, Team team);
    internal virtual void LeaveEvent(BasePlayer player);
    internal virtual void LeaveEvent(BaseEventPlayer eventPlayer);
    private IEnumerator CreateEventPlayers();
    protected virtual void CreateEventPlayer(BasePlayer player, Team team);
    protected virtual Team GetPlayerTeam(BasePlayer player);
    protected virtual BaseEventPlayer AddPlayerComponent(BasePlayer player);
    internal virtual void OnPlayerRespawn(BaseEventPlayer baseEventPlayer);
    internal void SpawnPlayer(BaseEventPlayer eventPlayer, bool giveKit, bool sleep);
    protected virtual void OnPlayerSpawned(BaseEventPlayer eventPlayer);
    protected void EjectAllPlayers();
    protected void RespawnAllPlayers();
    private bool HasMinimumRequiredPlayers();
    internal virtual bool CanDealEntityDamage(BaseEventPlayer attacker, BaseEntity entity, HitInfo hitInfo);
    protected virtual float GetDamageModifier(BaseEventPlayer eventPlayer);
    internal virtual void OnPlayerTakeDamage(BaseEventPlayer eventPlayer, HitInfo hitInfo);
    internal virtual void PrePlayerDeath(BaseEventPlayer eventPlayer, HitInfo hitInfo);
    internal virtual void OnEventPlayerDeath(BaseEventPlayer victim, BaseEventPlayer attacker, HitInfo hitInfo);
    protected virtual void DisplayKillToChat(BaseEventPlayer victim, string attackerName);
    protected void ProcessWinners();
    protected virtual void GetWinningPlayers(List<BaseEventPlayer> list);
    protected virtual bool CanDropBackpack();
    protected virtual bool CanGiveKit(BaseEventPlayer eventPlayer);
    protected virtual void OnKitGiven(BaseEventPlayer eventPlayer);
    internal List<string> GetAvailableKits(Team team);
    internal virtual void GetAdditionalEventDetails(List<KeyValuePair<string, object>> list, ulong playerId);
    internal virtual void GetSpectateTargets(List<BaseEventPlayer> list);
    private void UpdateDeadSpectateTargets(BaseEventPlayer victim);
    internal int GetAlivePlayerCount();
    internal int GetTeamCount(Team team);
    internal int GetTeamAliveCount(Team team);
    internal virtual int GetTeamScore(Team team);
    protected void BalanceTeams();
    internal void OnEntityDeployed(BaseCombatEntity entity);
    private void CleanupEntities();
    internal void UpdateScoreboard();
    protected void UpdateScoreboard(BaseEventPlayer eventPlayer);
    protected void UpdateScores();
    protected virtual void BuildScoreboard();
    protected virtual float GetFirstScoreValue(BaseEventPlayer eventPlayer);
    protected virtual float GetSecondScoreValue(BaseEventPlayer eventPlayer);
    protected virtual void SortScores(List<ScoreEntry> list);
    internal void BroadcastToPlayers(string key, object[] args);
    internal void BroadcastToPlayers(Func<string, ulong, string> GetMessage, string key, object[] args);
    internal void BroadcastToTeam(Team team, string key, string[] args);
    internal void BroadcastToPlayer(BaseEventPlayer eventPlayer, string message);
    private void BroadcastOpenEvent();
}

public class BaseEventPlayer : MonoBehaviour
{
    protected float _respawnDurationRemaining;
    protected float _invincibilityEndsAt;
    private double _resetDamageTime;
    private List<ulong> _damageContributors;
    private bool _isOOB;
    private int _oobTime;
    private int _spectateIndex;
    internal BasePlayer Player { get; set; }
    internal BaseEventGame Event { get; set; }
    internal Team Team { get; set; }
    internal int Kills { get; set; }
    internal int Deaths { get; set; }
    internal bool IsDead { get; set; }
    internal bool AutoRespawn { get; set; }
    internal bool CanRespawn { get; set; }
    internal int RespawnRemaining { get; set; }
    internal bool IsInvincible { get; set; }
    internal BaseEventPlayer SpectateTarget { get; set; }
    internal string Kit { get; set; }
    internal bool IsSelectingClass { get; set; }
    internal bool IsOutOfBounds { get; set; }
    private void Awake();
    private void OnDestroy();
    internal void ResetPlayer();
    internal void ForceSelectClass();
    protected void RespawnTick();
    internal void OnKilledPlayer(HitInfo hitInfo);
    internal virtual void OnPlayerDeath(BaseEventPlayer attacker, float respawnTime);
    internal void AddPlayerDeath(BaseEventPlayer attacker);
    protected void ApplyAssistPoints(BaseEventPlayer attacker);
    internal void ApplyInvincibility();
    protected void TickOutOfBounds();
    internal void DropInventory();
    internal void RemoveFromNetwork();
    internal void AddToNetwork();
    internal void OnTakeDamage(ulong attackerId);
    internal List<ulong> DamageContributors { get; set; }
    public void BeginSpectating();
    public void FinishSpectating();
    public void SetSpectateTarget(BaseEventPlayer eventPlayer);
    public void UpdateSpectateTarget();
    private List<string> _openPanels;
    internal void AddUI(string panel, CuiElementContainer container);
    internal void DestroyUI();
    internal void DestroyUI(string panel);
}

public class GameTimer
{
    private BaseEventGame _owner;
    private string _message;
    private int _timeRemaining;
    private Action _callback;
    internal GameTimer(BaseEventGame owner);
    internal void StartTimer(int time, string message, Action callback);
    internal void StopTimer();
    private void TimerTick();
    private void UpdateTimer();
}

internal class SpawnSelector
{
    private List<Vector3> _defaultSpawns;
    private List<Vector3> _availableSpawns;
    internal SpawnSelector(string eventName, string spawnFile);
    internal Vector3 GetSpawnPoint();
    internal Vector3 ReserveSpawnPoint(int index);
    internal void Destroy();
}

public class EventConfig
{
    public string EventName { get; set; }
    public string EventType { get; set; }
    public string ZoneID { get; set; }
    public int TimeLimit { get; set; }
    public int ScoreLimit { get; set; }
    public int MinimumPlayers { get; set; }
    public int MaximumPlayers { get; set; }
    public bool AllowClassSelection { get; set; }
    public TeamConfig TeamConfigA { get; set; }
    public TeamConfig TeamConfigB { get; set; }
    public Hash<string, object> AdditionalParams { get; set; }
    public EventConfig();
    public EventConfig(string type, IEventPlugin eventPlugin);
    public T GetParameter(string key);
    public string GetString(string fieldName);
    public List<string> GetList(string fieldName);
    public class TeamConfig
    {
        public string Color { get; set; }
        public string Spawnfile { get; set; }
        public List<string> Kits { get; set; }
        public string Clothing { get; set; }
    }

    [JsonIgnore]
    public IEventPlugin Plugin { get; set; }
}

public class TeamConfig
{
    public string Color { get; set; }
    public string Spawnfile { get; set; }
    public List<string> Kits { get; set; }
    public string Clothing { get; set; }
}

public class EventResults
{
    public string EventName { get; set; }
    public string EventType { get; set; }
    public ScoreEntry TeamScore { get; set; }
    public IEventPlugin Plugin { get; set; }
    public List<ScoreEntry> Scores { get; set; }
    public bool IsValid { get; set; }
    public void UpdateFromEvent(BaseEventGame baseEventGame);
}

public class ConfigData
{
    [JsonProperty(PropertyName = "Auto-Event Options")]
    public AutoEventOptions AutoEvents { get; set; }
    [JsonProperty(PropertyName = "Event Options")]
    public EventOptions Event { get; set; }
    [JsonProperty(PropertyName = "Reward Options")]
    public RewardOptions Reward { get; set; }
    [JsonProperty(PropertyName = "Timer Options")]
    public TimerOptions Timer { get; set; }
    [JsonProperty(PropertyName = "Message Options")]
    public MessageOptions Message { get; set; }
    public class EventOptions
    {
        [JsonProperty(PropertyName = "Blacklisted commands for event players")]
        public string[] CommandBlacklist { get; set; }
    }

    public class RewardOptions
    {
        [JsonProperty(PropertyName = "Amount rewarded for kills")]
        public int KillAmount { get; set; }
        [JsonProperty(PropertyName = "Amount rewarded for wins")]
        public int WinAmount { get; set; }
        [JsonProperty(PropertyName = "Amount rewarded for headshots")]
        public int HeadshotAmount { get; set; }
        [JsonProperty(PropertyName = "Reward type (ServerRewards, Economics, Scrap)")]
        public string Type { get; set; }
    }

    public class TimerOptions
    {
        [JsonProperty(PropertyName = "Match start timer (seconds)")]
        public int Start { get; set; }
        [JsonProperty(PropertyName = "Match pre-start timer (seconds)")]
        public int Prestart { get; set; }
        [JsonProperty(PropertyName = "Backpack despawn timer (seconds)")]
        public int Bag { get; set; }
    }

    public class MessageOptions
    {
        [JsonProperty(PropertyName = "Announce events when one opens")]
        public bool Announce { get; set; }
        [JsonProperty(PropertyName = "Event announcement interval (seconds)")]
        public int AnnounceInterval { get; set; }
        [JsonProperty(PropertyName = "Broadcast when a player joins an event to chat")]
        public bool BroadcastJoiners { get; set; }
        [JsonProperty(PropertyName = "Broadcast when a player leaves an event to chat")]
        public bool BroadcastLeavers { get; set; }
        [JsonProperty(PropertyName = "Broadcast the name(s) of the winning player(s) to chat")]
        public bool BroadcastWinners { get; set; }
        [JsonProperty(PropertyName = "Broadcast kills to chat")]
        public bool BroadcastKills { get; set; }
        [JsonProperty(PropertyName = "Chat icon Steam ID")]
        public ulong ChatIcon { get; set; }
    }

    public class AutoEventOptions
    {
        [JsonProperty(PropertyName = "Enable auto-events")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "List of event configs to run through")]
        public string[] Events { get; set; }
        [JsonProperty(PropertyName = "Randomize auto-event selection")]
        public bool Randomize { get; set; }
        [JsonProperty(PropertyName = "Auto-event interval (seconds)")]
        public int Interval { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class EventOptions
{
    [JsonProperty(PropertyName = "Blacklisted commands for event players")]
    public string[] CommandBlacklist { get; set; }
}

public class RewardOptions
{
    [JsonProperty(PropertyName = "Amount rewarded for kills")]
    public int KillAmount { get; set; }
    [JsonProperty(PropertyName = "Amount rewarded for wins")]
    public int WinAmount { get; set; }
    [JsonProperty(PropertyName = "Amount rewarded for headshots")]
    public int HeadshotAmount { get; set; }
    [JsonProperty(PropertyName = "Reward type (ServerRewards, Economics, Scrap)")]
    public string Type { get; set; }
}

public class TimerOptions
{
    [JsonProperty(PropertyName = "Match start timer (seconds)")]
    public int Start { get; set; }
    [JsonProperty(PropertyName = "Match pre-start timer (seconds)")]
    public int Prestart { get; set; }
    [JsonProperty(PropertyName = "Backpack despawn timer (seconds)")]
    public int Bag { get; set; }
}

public class MessageOptions
{
    [JsonProperty(PropertyName = "Announce events when one opens")]
    public bool Announce { get; set; }
    [JsonProperty(PropertyName = "Event announcement interval (seconds)")]
    public int AnnounceInterval { get; set; }
    [JsonProperty(PropertyName = "Broadcast when a player joins an event to chat")]
    public bool BroadcastJoiners { get; set; }
    [JsonProperty(PropertyName = "Broadcast when a player leaves an event to chat")]
    public bool BroadcastLeavers { get; set; }
    [JsonProperty(PropertyName = "Broadcast the name(s) of the winning player(s) to chat")]
    public bool BroadcastWinners { get; set; }
    [JsonProperty(PropertyName = "Broadcast kills to chat")]
    public bool BroadcastKills { get; set; }
    [JsonProperty(PropertyName = "Chat icon Steam ID")]
    public ulong ChatIcon { get; set; }
}

public class AutoEventOptions
{
    [JsonProperty(PropertyName = "Enable auto-events")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "List of event configs to run through")]
    public string[] Events { get; set; }
    [JsonProperty(PropertyName = "Randomize auto-event selection")]
    public bool Randomize { get; set; }
    [JsonProperty(PropertyName = "Auto-event interval (seconds)")]
    public int Interval { get; set; }
}

public class EventData
{
    public Hash<string, EventConfig> events;
}

public class EventParameter
{
    public string Name;
    public InputType Input;
    public string Field;
    public string DataType;
    public bool IsRequired;
    public string SelectorHook;
    public bool SelectMultiple;
    public object DefaultValue;
    [JsonIgnore]
    public bool IsList { get; set; }
}

public class RestoreData
{
    public Hash<ulong, PlayerData> Restore;
    internal void AddData(BasePlayer player);
    public void AddPrizeToData(ulong playerId, int itemId, int amount);
    private ItemData FindItem(PlayerData playerData, int itemId);
    internal void RemoveData(ulong playerId);
    internal bool HasRestoreData(ulong playerId);
    internal void RestorePlayer(BasePlayer player);
    private void RestoreAllItems(BasePlayer player, PlayerData playerData);
    private bool RestoreItems(BasePlayer player, ItemData[] itemData, Container type);
    public class PlayerData
    {
        public float[] stats;
        public Vector3 position;
        public ItemData[] containerMain;
        public ItemData[] containerWear;
        public ItemData[] containerBelt;
        public PlayerData();
        public PlayerData(BasePlayer player);
        private IEnumerable<ItemData> GetItems(ItemContainer container);
        private float[] GetStats(BasePlayer player);
        internal void SetStats(BasePlayer player);
    }

}

public class PlayerData
{
    public float[] stats;
    public Vector3 position;
    public ItemData[] containerMain;
    public ItemData[] containerWear;
    public ItemData[] containerBelt;
    public PlayerData();
    public PlayerData(BasePlayer player);
    private IEnumerable<ItemData> GetItems(ItemContainer container);
    private float[] GetStats(BasePlayer player);
    internal void SetStats(BasePlayer player);
}

public class ItemData
{
    public int itemid;
    public ulong skin;
    public int amount;
    public float condition;
    public float maxCondition;
    public int ammo;
    public string ammotype;
    public int position;
    public int frequency;
    public InstanceData instanceData;
    public ItemData[] contents;
    public class InstanceData
    {
        public int dataInt;
        public int blueprintTarget;
        public int blueprintAmount;
        public uint subEntity;
        public InstanceData();
        public InstanceData(Item item);
        public void Restore(Item item);
        public bool IsValid();
    }

}

public class InstanceData
{
    public int dataInt;
    public int blueprintTarget;
    public int blueprintAmount;
    public uint subEntity;
    public InstanceData();
    public InstanceData(Item item);
    public void Restore(Item item);
    public bool IsValid();
}

EventManagerEx

```

---

## EventRandomizer by mvrb - Random timers for Airdrops, Cargo Ship, Patrol Helicopter, and Chinook

```csharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using ConVar;
using UnityEngine;

Oxide.Plugins
[Info("Event Randomizer", "mvrb", "0.3.4")]
[Description("Set random timers for server events")]
 class EventRandomizer : RustPlugin
{
    private float heliInterval;
    private float chinookInterval;
    private float cargoInterval;
    private float airdropInterval;
    private int lastHeli;
    private int lastChinook;
    private int lastCargo;
    private int lastAirdrop;
    private string permSpawnChinook;
    private string permSpawnHeli;
    private string permSpawnCargo;
    private string permSpawnAirdrop;
    private string permCheckTimer;
    private bool initialized;
    private class EventTimer
    {
        public float Min;
        public float Max;
    }

    private void LoadDefaultMessages();
    private void OnServerInitialized();
    private void OnEntitySpawned(BaseNetworkable entity);
    [ChatCommand("heli")]
    private void CmdHeli(BasePlayer player);
    [ChatCommand("chinook")]
    private void CmdChinook(BasePlayer player);
    [ChatCommand("cargo")]
    private void CmdCargo(BasePlayer player);
    [ConsoleCommand("ch47.spawn")]
    private void ConsoleCmdSpawnCh47(ConsoleSystem.Arg arg);
    [ConsoleCommand("heli.spawn")]
    private void ConsoleCmdSpawnHeli(ConsoleSystem.Arg arg);
    [ConsoleCommand("cargo.spawn")]
    private void ConsoleCmdSpawnCargo(ConsoleSystem.Arg arg);
    [ConsoleCommand("airdrop.spawn")]
    private void ConsoleCmdSpawnAirdrop(ConsoleSystem.Arg arg);
    private void SpawnCargoRandom();
    private void SpawnHeliRandom();
    private void SpawnChinookRandom();
    private void SpawnAirdropRandom();
    private string FormatTime(float seconds);
    private void SpawnAirdrop();
    private void SpawnCargo();
    private void SpawnHeli();
    private void SpawnChinook();
    private int GetUnix();
    private string Lang(string key, string id, object[] args);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Event Timers in seconds")]
        public Dictionary<string, EventTimer> EventTimers { get; set; }
        [JsonProperty(PropertyName = "Block Airdrops spawned by the server")]
        public bool blockServerAirdrops;
        [JsonProperty(PropertyName = "Block Cargo Ships spawned by the server")]
        public bool blockServerCargoShips;
        [JsonProperty(PropertyName = "Block Chinooks spawned by the server")]
        public bool blockServerChinooks;
        [JsonProperty(PropertyName = "Block Patrol Helicopters spawned by the server")]
        public bool blockServerPatrolHelicopters;
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void SaveConfig(ConfigData config);
}

private class EventTimer
{
    public float Min;
    public float Max;
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Event Timers in seconds")]
    public Dictionary<string, EventTimer> EventTimers { get; set; }
    [JsonProperty(PropertyName = "Block Airdrops spawned by the server")]
    public bool blockServerAirdrops;
    [JsonProperty(PropertyName = "Block Cargo Ships spawned by the server")]
    public bool blockServerCargoShips;
    [JsonProperty(PropertyName = "Block Chinooks spawned by the server")]
    public bool blockServerChinooks;
    [JsonProperty(PropertyName = "Block Patrol Helicopters spawned by the server")]
    public bool blockServerPatrolHelicopters;
}


```

---

## EventStatistics by k1lly0u - Stores statistic data from Event Manager

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Event Statistics", "k1lly0u", "1.0.1"), Description("Manages and provides API for statistics gathered from EventManager")]
public class EventStatistics : RustPlugin
{
    private DynamicConfigFile statisticsData;
    public static Statistics Data { get; set; }
    private void Loaded();
    private void OnServerSave();
    private void Unload();
    [HookMethod("AddStatistic")]
    public void AddStatistic(BasePlayer player, string statistic, int amount);
    [HookMethod("AddStatistic")]
    public void AddStatistic(ulong playerId, string statistic, int amount);
    [HookMethod("AddGlobalStatistic")]
    public void AddGlobalStatistic(string statistic, int amount);
    [HookMethod("OnGamePlayed")]
    public void OnGamePlayed(string eventName);
    [HookMethod("OnGamePlayed")]
    public void OnGamePlayed(BasePlayer player, string eventName);
    [HookMethod("GetStatistic")]
    public int GetStatistic(ulong playerId, string statistic);
    [HookMethod("GetRank")]
    public int GetRank(ulong playerId);
    [HookMethod("GetEventStatistic")]
    public int GetEventStatistic(ulong playerId, string eventName);
    [HookMethod("GetPlayerStatistics")]
    public void GetPlayerStatistics(List<KeyValuePair<string, int>> list, ulong playerId);
    [HookMethod("GetPlayerEvents")]
    public void GetPlayerEvents(List<KeyValuePair<string, int>> list, ulong playerId);
    [HookMethod("GetGlobalStatistic")]
    public int GetGlobalStatistic(string statistic);
    [HookMethod("GetGlobalEventStatistic")]
    public int GetGlobalEventStatistic(string eventName);
    [HookMethod("GetGlobalStatistics")]
    public void GetGlobalStatistics(List<KeyValuePair<string, int>> list);
    [HookMethod("GetGlobalEvents")]
    public void GetGlobalEvents(List<KeyValuePair<string, int>> list);
    [HookMethod("GetStatisticNames")]
    public void GetStatisticNames(List<string> list);
    private static string RemoveTags(string str);
    private static List<KeyValuePair<string, string>> _tags;
    public class Statistics
    {
        public Hash<ulong, Data> players;
        public Data global;
        [JsonIgnore]
        private Hash<Statistic, List<Data>> _cachedSortResults;
        public Data Find(ulong playerId);
        public void OnGamePlayed(string eventName);
        public void OnGamePlayed(BasePlayer player, string eventName);
        public void OnGamePlayed(ulong playerId, string eventName);
        public void AddStatistic(BasePlayer player, string statistic, int amount);
        public void AddStatistic(ulong playerId, string statistic, int amount);
        public void AddGlobalStatistic(string statistic, int amount);
        public int GetStatistic(ulong playerId, string statistic);
        public void GetPlayerStatistics(List<KeyValuePair<string, int>> list, ulong playerId);
        public void GetPlayerEvents(List<KeyValuePair<string, int>> list, ulong playerId);
        public int GetRank(ulong playerId);
        public int GetEventStatistic(ulong playerId, string eventName);
        public int GetGlobalStatistic(string statistic);
        public int GetGlobalEventStatistic(string eventName);
        public void GetGlobalStatistics(List<KeyValuePair<string, int>> list);
        public void GetGlobalEvents(List<KeyValuePair<string, int>> list);
        public void GetStatisticNames(List<string> list);
        public void ClearCachedSortResults();
        public List<Data> SortStatisticsBy(Statistic statistic);
        public void UpdateRankingScores();
        public class Data
        {
            public Hash<string, int> events;
            public Hash<string, int> statistics;
            public string DisplayName { get; set; }
            public ulong UserID { get; set; }
            [JsonIgnore]
            public float Score { get; set; }
            [JsonIgnore]
            public int Rank { get; set; }
            public Data(ulong userID, string displayName);
            public void UpdateName(string displayName);
            public void AddStatistic(string statisticName, int value);
            public void AddGamePlayed(string name);
            public int GetStatistic(string statistic);
            public void UpdateRankingScore();
        }

    }

    private void SaveData();
    private void LoadData();
}

public class Statistics
{
    public Hash<ulong, Data> players;
    public Data global;
    [JsonIgnore]
    private Hash<Statistic, List<Data>> _cachedSortResults;
    public Data Find(ulong playerId);
    public void OnGamePlayed(string eventName);
    public void OnGamePlayed(BasePlayer player, string eventName);
    public void OnGamePlayed(ulong playerId, string eventName);
    public void AddStatistic(BasePlayer player, string statistic, int amount);
    public void AddStatistic(ulong playerId, string statistic, int amount);
    public void AddGlobalStatistic(string statistic, int amount);
    public int GetStatistic(ulong playerId, string statistic);
    public void GetPlayerStatistics(List<KeyValuePair<string, int>> list, ulong playerId);
    public void GetPlayerEvents(List<KeyValuePair<string, int>> list, ulong playerId);
    public int GetRank(ulong playerId);
    public int GetEventStatistic(ulong playerId, string eventName);
    public int GetGlobalStatistic(string statistic);
    public int GetGlobalEventStatistic(string eventName);
    public void GetGlobalStatistics(List<KeyValuePair<string, int>> list);
    public void GetGlobalEvents(List<KeyValuePair<string, int>> list);
    public void GetStatisticNames(List<string> list);
    public void ClearCachedSortResults();
    public List<Data> SortStatisticsBy(Statistic statistic);
    public void UpdateRankingScores();
    public class Data
    {
        public Hash<string, int> events;
        public Hash<string, int> statistics;
        public string DisplayName { get; set; }
        public ulong UserID { get; set; }
        [JsonIgnore]
        public float Score { get; set; }
        [JsonIgnore]
        public int Rank { get; set; }
        public Data(ulong userID, string displayName);
        public void UpdateName(string displayName);
        public void AddStatistic(string statisticName, int value);
        public void AddGamePlayed(string name);
        public int GetStatistic(string statistic);
        public void UpdateRankingScore();
    }

}

public class Data
{
    public Hash<string, int> events;
    public Hash<string, int> statistics;
    public string DisplayName { get; set; }
    public ulong UserID { get; set; }
    [JsonIgnore]
    public float Score { get; set; }
    [JsonIgnore]
    public int Rank { get; set; }
    public Data(ulong userID, string displayName);
    public void UpdateName(string displayName);
    public void AddStatistic(string statisticName, int value);
    public void AddGamePlayed(string name);
    public int GetStatistic(string statistic);
    public void UpdateRankingScore();
}


```

---

