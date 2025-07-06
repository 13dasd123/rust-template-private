# uMod Plugins Dataset - Code Abstractions (Continued)

Chunk 10 - Generated: 2025-07-06 20:25:19

## XPerience by MACHIN3 - RPG based levels and experience mod with stats and skills

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Security.Cryptography;
using System.Text.RegularExpressions;
using MySql.Data.MySqlClient;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Core.Database;
using Oxide.Core.Libraries.Covalence;
using Oxide.Plugins.XPerienceEx;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;
using Random = UnityEngine.Random;
using WebSocketSharp;
using Facepunch;
using Facepunch.Math;
using ConVar;
using System.IO;
using System.Net;

Oxide.Plugins
[Info("XPerience", "MACHIN3", "1.8.800")]
[Description("Player level system with xp, stats, and skills")]
public class XPerience : RustPlugin
{
    public const string version;
    [PluginReference]
    private readonly Plugin XPerienceAddon;
    private readonly Plugin WeaponMechanics;
    private readonly Plugin KillRecords;
    private readonly Plugin Economics;
    private readonly Plugin ServerRewards;
    private readonly Plugin ImageLibrary;
    private readonly Plugin TeaModifiers;
    private readonly Plugin BetterChat;
    private readonly Plugin ColouredChat;
    private readonly Plugin IQChat;
    private readonly Plugin Backpacks;
    private readonly Plugin RaidableBases;
    private readonly Plugin ZoneManager;
    private readonly Plugin PersonalAnimal;
    private readonly Plugin SkinBox;
    private readonly Plugin BuildingGrades;
    private readonly Plugin ItemRetriever;
    private readonly Plugin BotReSpawn;
    private readonly Plugin NeverWear;
    private readonly Plugin Cooking;
    private readonly Plugin EventHelper;
    private readonly Plugin SurvivalArena;
    private readonly Plugin MonumentOwner;
    private readonly Plugin Triangulation;
    private XPData _xpData;
    private DailyData _dailyData;
    private LootData _lootData;
    private CorpseData _corpseData;
    private HorseData _horseData;
    private WeaponData _weaponData;
    private BoatData _boatData;
    private VehicleData _vehicleData;
    private MinicopterData _minicopterData;
    private SnowmobData _snowmobData;
    private SmithyData _smithyData;
    private ElectricianData _electricianData;
    private HeliHits _heliHits;
    private DynamicConfigFile _XPerienceData;
    private DynamicConfigFile _DailyXPerienceData;
    private DynamicConfigFile _LootContainData;
    private DynamicConfigFile _CorpseContainData;
    private DynamicConfigFile _HorseData;
    private DynamicConfigFile _WeaponData;
    private DynamicConfigFile _BoatData;
    private DynamicConfigFile _VehicleData;
    private DynamicConfigFile _MinicopterData;
    private DynamicConfigFile _SnowmobData;
    private DynamicConfigFile _SmithyData;
    private DynamicConfigFile _ElectricianData;
    private Dictionary<string, XPRecord> _xperienceCache;
    private Dictionary<string, DailyRecord> _dailyxperienceCache;
    private Dictionary<ulong, Loot> _lootCache;
    private Dictionary<ulong, Corpse> _corpseCache;
    private Dictionary<ulong, Horse> _horseCache;
    private Dictionary<ulong, Weapon> _weaponCache;
    private Dictionary<ulong, Boat> _boatCache;
    private Dictionary<ulong, Vehicle> _vehicleCache;
    private Dictionary<ulong, MiniCopterP> _minicopterCache;
    private Dictionary<ulong, Snowmob> _snowmobCache;
    private Dictionary<string, SmithyD> _smithyCache;
    private Dictionary<ulong, ElectricianD> _electricianCache;
    private Dictionary<ulong, Heli> _heliCache;
    public Configuration config;
    private static Configuration configData;
    private static readonly RNGCryptoServiceProvider _generator;
    private const string Admin;
    private const string VIP;
    private const string PermMentality;
    private const string PermDexterity;
    private const string PermMight;
    private const string PermCaptaincy;
    private const string PermWeaponry;
    private const string PermNinjary;
    private const string PermWoodCutter;
    private const string PermSmithy;
    private const string PermMiner;
    private const string PermForager;
    private const string PermHunter;
    private const string PermFisher;
    private const string PermCrafter;
    private const string PermFramer;
    private const string PermMedic;
    private const string PermScavenger;
    private const string PermElectrician;
    private const string PermDemolitionist;
    private const string PermTamer;
    private const string PermXPBoost;
    private readonly Hash<ulong, double> _notifyCooldowns;
    private readonly Hash<ulong, double> _buildCooldowns;
    private readonly Hash<ulong, double> _craftCooldowns;
    private readonly Hash<ulong, int> _TopUIPage;
    private Timer DashPanelTimer;
    private Timer _helitracker;
    private double CurrentTime { get; set; }
    private bool _isXPReady;
    private bool _isRestart;
    private int _imageLibraryCheck;
    private Dictionary<string, string> _xperienceImageList;
    private Dictionary<string, string> _CheckImageList;
    private Dictionary<string, string> _CheckImageListReload;
    public static class RandomNumber
    {
        private static readonly RNGCryptoServiceProvider _generator;
        public static int Between(int minimumValue, int maximumValue);
    }

    public class Configuration : SerializableConfiguration
    {
        [JsonProperty("Player Chat Commands")]
        public PlayerChatCommands playerchatCommands;
        [JsonProperty("Admin Chat Commands")]
        public AdminChatCommands adminchatCommands;
        [JsonProperty("Player Info Box")]
        public PlayerProfileSettings playerprofilesettings;
        [JsonProperty("Default Options")]
        public DefaultOptions defaultOptions;
        [JsonProperty("Sound Effects")]
        public SoundEffects soundEffects;
        [JsonProperty("UI Text Colors")]
        public UITextColor uitextColor;
        [JsonProperty("Image Icons")]
        public ImageIcons imageicons;
        [JsonProperty("XP - Level Config")]
        public XpLevel xpLevel;
        [JsonProperty("Daily Timer Config")]
        public DailyTimer dailytimer;
        [JsonProperty("Daily XP Limit Config")]
        public DailyXpLimit dailyxpLimit;
        [JsonProperty("Daily Reset Limit Config")]
        public DailyResetLimit dailyresetLimit;
        [JsonProperty("XP - Level Ranks")]
        public XpLevelRanks xpLevelRanks;
        [JsonProperty("Rank Boosts")]
        public RankBoostsSettings Rankboostssettings;
        [JsonProperty("Special Groups")]
        public SpecialGroups specialGroups;
        [JsonProperty("XP - Night Bonus")]
        public NightBonus nightBonus;
        [JsonProperty("XP - Gain Amounts")]
        public XpGain xpGain;
        [JsonProperty("XP - Gather Amounts")]
        public XpGather xpGather;
        [JsonProperty("XP - Building Amounts")]
        public XpBuilding xpBuilding;
        [JsonProperty("XP - Teams")]
        public XpTeams xpTeams;
        [JsonProperty("XP - Mission Amounts")]
        public XpMissions xpMissions;
        [JsonProperty("XP - Reducer Amounts")]
        public XpReducer xpReducer;
        [JsonProperty("BonusXP - Bonus Amounts (requires KillRecords plugin)")]
        public XpBonus xpBonus;
        [JsonProperty("Economics Rewards (requires Economics plugin)")]
        public XpEcon xpEcon;
        [JsonProperty("Server Rewards (requires ServerRewards plugin)")]
        public SRewards sRewards;
        [JsonProperty("Mentality Stat")]
        public Mentality mentality;
        [JsonProperty("Dexterity Stat")]
        public Dexterity dexterity;
        [JsonProperty("Might Stat")]
        public Might might;
        [JsonProperty("Captaincy Stat")]
        public Captaincy captaincy;
        [JsonProperty("Weaponry Stat")]
        public Weaponry weaponry;
        [JsonProperty("Ninjary Stat")]
        public Ninjary ninjary;
        [JsonProperty("WoodCutter Skill")]
        public Woodcutter woodcutter;
        [JsonProperty("Smithy Skill")]
        public Smithy smithy;
        [JsonProperty("Miner Skill")]
        public Miner miner;
        [JsonProperty("Forager Skill")]
        public Forager forager;
        [JsonProperty("Hunter Skill")]
        public Hunter hunter;
        [JsonProperty("Fisher Skill")]
        public Fisher fisher;
        [JsonProperty("Crafter Skill")]
        public Crafter crafter;
        [JsonProperty("Framer Skill")]
        public Framer framer;
        [JsonProperty("Medic Skill")]
        public Medic medic;
        [JsonProperty("Scavenger Skill")]
        public Scavenger scavenger;
        [JsonProperty("Electrician Skill")]
        public Electrician electrician;
        [JsonProperty("Demolitionist Skill")]
        public Demolitionist demolitionist;
        [JsonProperty("Tamer Skill")]
        public Tamer tamer;
        [JsonProperty("SQL Info")]
        public SQL sql;
        [JsonProperty("Backpacks Mod")]
        public BackpacksMod backpacksmod;
        [JsonProperty("ZoneManager Mod")]
        public ZoneManagerMod zonemanagermod;
        [JsonProperty("EventHelper Mod")]
        public EventHelperMod eventhelpermod;
        [JsonProperty("SurvivalArena Mod")]
        public SurvivalArenaMod survivalarenamod;
        [JsonProperty("Raidable Bases")]
        public RaidableBasesMod raidablebasesmod;
    }

    public class PlayerChatCommands
    {
        public string openplayerstats;
        public string openplayerstats2;
        public string openplayerstats3;
        public string showplayerstatschat;
        public string opentopplayers;
        public string playeraddstat;
        public string playeraddskill;
        public string playerresetstats;
        public string playerresetskills;
        public string playerresetall;
        public string playerliveuichange;
        public string openhelp;
    }

    public class AdminChatCommands
    {
        public string showadminhelp;
        public string openadminpanel;
        public string adminresetxperience;
        public string adminxpgive;
        public string adminxpgiveall;
        public string adminpointsgive;
        public string adminxptake;
        public string adminresetplayer;
        public string adminfixdata;
        public string adminitemchange;
        public string adminresetharvest;
        public string adminresetlevelonly;
        public string adminresetrankonly;
        public string adminresetstat;
        public string adminresetskill;
        public string adminresetlevelonlyall;
        public string adminresetrankonlyall;
        public string adminexcludeplayer;
        public string admingiveitem;
    }

    public class PlayerProfileSettings
    {
        public bool showunusedeffects;
        public bool useplayeravatar;
        public bool profilemenusettings;
        public bool profilemenutopplayers;
        public bool profilemenuraids;
        public bool profilemenuhelp;
        public bool profilemenucalculations;
        public bool skillshelp;
        public bool profilemenuwelcome;
        public bool playtime;
        public bool alivetime;
        public bool sleepingtime;
        public bool swimingtime;
        public bool drivingtime;
        public bool flyingtime;
        public bool boatingtime;
        public bool basetime;
        public bool monumenttime;
        public bool wildernesstime;
        public bool metersran;
        public bool meterswalked;
        public bool lastdmgrec;
        public bool lastdmgrecby;
        public bool lastdmgdelt;
        public bool lastdmgdeltto;
        public string AnchorMin;
        public string AnchorMax;
        public string OffsetMin;
        public string OffsetMax;
        public string InsideAnchorMin;
        public string InsideAnchorMax;
        public int menutype;
        public bool usebgimage;
        public int profilebg;
        public bool allowprofilebgchange;
        public bool usemenubgimage;
        public double bgfadein;
        public double menuwidth;
        public double menuheight;
        public double menubuttonheight;
        public int menubuttonfont;
    }

    public class DefaultOptions
    {
        public bool userpermissions;
        public int liveuistatslocation;
        public bool liveuistatslocationmoveable;
        public bool showchatprofileonconnect;
        public int NotifcationCooldown;
        public bool restristresets;
        public bool allowrespec;
        public int resetminsstats;
        public int resetminsskills;
        public bool bypassadminreset;
        public int vipresetminstats;
        public int vipresetminsskills;
        public int playerfixdatatimer;
        public bool disableplayerfixdata;
        public bool disablearmorchat;
        public bool hardcorenoreset;
        public bool allowplayersearch;
        public bool allowplayerreset;
        public int topplayersperpage;
        public bool showonlinestatus;
        public bool useprogressivelevelicons;
        public bool showfuelguage;
        public bool showspeedometer;
        public int speedometertype;
        public bool dropsgotoplayerinventory;
        public bool wipedataonnewsave;
        public bool enabledashpanel;
        public bool enableconfirmationprompt;
        public bool showchatnotifications;
        public bool showlevelinchat;
        public bool hidechatnotifications;
        public bool debugmode;
    }

    public class SoundEffects
    {
        public bool levelup;
        public bool leveldown;
        public bool rankup;
        public bool statup;
        public bool skillup;
        public bool statreset;
        public bool skillreset;
        public bool scavengerloot;
        public bool foragerloot;
        public string levelupeffect;
        public string leveldowneffect;
        public string rankupeffect;
        public string statupeffect;
        public string skillupeffect;
        public string statreseteffect;
        public string skillreseteffect;
        public string scavengerlooteffect;
        public string foragerlooteffect;
    }

    public class UITextColor
    {
        public string defaultcolor;
        public string level;
        public string ranklevel;
        public string rankxp;
        public string rankname;
        public string experience;
        public string nextlevel;
        public string remainingxp;
        public string statskilllevels;
        public string perks;
        public string unspentpoints;
        public string spentpoints;
        public string pets;
        public string mentality;
        public string dexterity;
        public string might;
        public string captaincy;
        public string weaponry;
        public string Ninjary;
        public string woodcutter;
        public string smithy;
        public string miner;
        public string forager;
        public string hunter;
        public string fisher;
        public string crafter;
        public string framer;
        public string medic;
        public string scavenger;
        public string electrician;
        public string demolitionist;
        public string tamer;
        public string xpbar;
        public string armorbar;
    }

    public class ImageIcons
    {
        public bool uselocalpath;
        public string rootpath;
        public string xperiencelogo;
        public string mainicon;
        public string mentality;
        public string dexterity;
        public string might;
        public string captaincy;
        public string weaponry;
        public string ninjary;
        public string woodcutter;
        public string smithy;
        public string miner;
        public string forager;
        public string hunter;
        public string fisher;
        public string crafter;
        public string framer;
        public string medic;
        public string scavenger;
        public string electrician;
        public string demolitionist;
        public string tamer;
        public string chicken;
        public string boar;
        public string stag;
        public string wolf;
        public string bear;
        public string polarbear;
        public string archery;
        public string wizardry;
        public string online;
        public string offline;
        public string backpack;
        public string xp;
        public string level;
        public string armor;
        public string level0;
        public string level2;
        public string level4;
        public string level6;
        public string level8;
        public string level10;
        public string dash;
        public string raideasy;
        public string raidmedium;
        public string raidhard;
        public string raidexpert;
        public string raidnightmare;
        public string profilebg;
        public string menubg;
        public Dictionary<int, BackgroundImgs> bgimages;
    }

    public class BackgroundImgs
    {
        public string name;
        public string url;
    }

    public class XpLevel
    {
        public double levelstart;
        public double levelmultiplier;
        public int maxlevel;
        public double levelxpboost;
        public int statpointsperlvl;
        public int skillpointsperlvl;
        public bool alwaysearnxp;
        public bool fullhealth;
        public bool fullmetabolism;
    }

    public class XpLevelRanks
    {
        public bool enableresetranks;
        public bool resetallstatsskills;
        public bool allowplayerdisable;
        public bool increaselevelmultiplier;
        public double levelmultiplierincrease;
        public int maxresetrank;
        public bool enablerankxpboost;
        public double rankxpboost;
        public bool rankstatboost;
        public double rankstatboostamount;
        public int rankstatpointstart;
        public int rankstatpointincrease;
        public bool rankskillboost;
        public double rankskillboostamount;
        public int rankskillpointstart;
        public int rankskillpointincrease;
        public bool keepremainingxp;
        public bool showtruelevelprofile;
        public bool showrankinchat;
        public bool showtruexpprofile;
        public bool showrankinliveui;
        public bool keepgrouponrank;
        public Dictionary<int, Ranks> ranks;
    }

    public class RankBoostsSettings
    {
        public bool researchcost;
        public bool researchspeed;
        public bool block;
        public bool armor;
        public bool distance;
        public bool meleedmg;
        public bool metabolism;
        public bool woodcuttergr;
        public bool woodcutterbonus;
        public bool smithypr;
        public bool smithyps;
        public bool smithyfc;
        public bool smithyhqmc;
        public bool smithyhqma;
        public bool minergr;
        public bool minerbonus;
        public bool minermfc;
        public bool minerfuel;
        public bool minermfa;
        public bool fisherfa;
        public bool fisheria;
        public bool fisherotr;
        public bool foragergr;
        public bool foragergwa;
        public bool foragerric;
        public bool huntergr;
        public bool hunterbonus;
        public bool hunterdmg;
        public bool hunterndmg;
        public bool crafterspeed;
        public bool craftercost;
        public bool crafterri;
        public bool crafterrc;
        public bool craftercc;
        public bool crafterca;
        public bool framerucost;
        public bool framerrcost;
        public bool medicrevivala;
        public bool medicrecovera;
        public bool medictools;
        public bool scavelc;
        public bool scavelm;
        public bool scavcic;
        public bool scavcim;
    }

    public class Ranks
    {
        public string name;
        public string sig;
        public string image;
        public string group;
        public string description;
    }

    public class SpecialGroups
    {
        public Dictionary<int, Specialgroups> specialgroups;
    }

    public class Specialgroups
    {
        public string groupname;
        public string permissionname;
        public int grouppriority;
        public double xpboost;
        public int dailyxplimit;
        public int dailystatlimitboost;
        public int dailyskilllimitboost;
    }

    public class DailyTimer
    {
        public int dailyresettimerhours;
        public DateTime lastdailyreset;
    }

    public class DailyXpLimit
    {
        public bool enabledailyxplimit;
        public int dailyxplimit;
        public int dailyxplimitvip;
        public int limitmultipliertype;
        public int limitmultiplier;
        public double limitpercentage;
    }

    public class DailyResetLimit
    {
        public bool enabledailyresetlimit;
        public int dailystatlimit;
        public int dailystatlimitvip;
        public int dailyskilllimit;
        public int dailyskilllimitvip;
    }

    public class NightBonus
    {
        public bool Enable;
        public int StartTime;
        public int EndTime;
        public double Bonus;
        public bool enableskillboosts;
    }

    public class XpGain
    {
        public double chickenxp;
        public double fishxp;
        public double boarxp;
        public double stagxp;
        public double wolfxp;
        public double bearxp;
        public double polarbearxp;
        public double sharkxp;
        public double horsexp;
        public double scientistxp;
        public double sc_full;
        public double sc_heavy;
        public double sc_cargo;
        public double sc_junkpile;
        public double sc_oilrig;
        public double sc_patrol;
        public double sc_peacekeeper;
        public double sc_roam;
        public double dwellerxp;
        public double tunneldwellerxp;
        public double underwaterdwellerxp;
        public double scarecrownpc;
        public double customnpc;
        public double zombienpc;
        public double playerxp;
        public double lootcontainerxp;
        public double lootbarrel;
        public double oilbarrel;
        public double vehicleparts;
        public double toolcrate;
        public double normalcrate;
        public double elitecrate;
        public double foodcrate;
        public double medicalcrate;
        public double animalharvestxp;
        public double corpseharvestxp;
        public double underwaterlootcontainerxp;
        public double lockedcratexp;
        public double hackablecratexp;
        public double craftingxp;
        public bool craftingxpdelay;
        public double craftingxpdelayseconds;
        public double bradley;
        public double patrolhelicopter;
        public double turretxp;
        public bool allowturretxp;
        public double playerrevive;
        public bool enablexpboost;
        public double xpboostamount;
        public int xpboostorder;
        public double gifts;
        public double opengifts;
        public double opengiftsmed;
        public double opengiftslarge;
        public double upgradegiftsmed;
        public double upgradegiftslarge;
        public double craftmeal;
    }

    public class XpGather
    {
        public double treexp;
        public double orexp;
        public double metalorexp;
        public double stoneorexp;
        public double sulfurorexp;
        public double harvestxp;
        public double plantxp;
        public bool noxptools;
        public bool onetimexp;
        public double toolxpchance;
        public double toolxppercent;
    }

    public class XpBuilding
    {
        public double twigstructure;
        public double woodstructure;
        public double stonestructure;
        public double metalstructure;
        public double armoredstructure;
        public bool preventBGxp;
        public bool buildxpdelay;
        public bool requirebuildingprivlidge;
        public int buildxpdelayseconds;
        public bool reducexp;
        public double buildxpreduction;
    }

    public class XpTeams
    {
        public bool enableteamxpgain;
        public bool enableteamxploss;
        public double teamxpgainamount;
        public double teamxplossamount;
        public float teamdistance;
    }

    public class XpMissions
    {
        public double missionsucceededxp;
        public bool missionfailed;
        public double missionfailedxp;
    }

    public class XpReducer
    {
        public bool suicidereduce;
        public double suicidereduceamount;
        public bool deathreduce;
        public double deathreduceamount;
        public bool rankdeathreduce;
    }

    public class XpBonus
    {
        public bool showkrbutton;
        public bool enablebonus;
        public int requiredkills;
        public double bonusxp;
        public int endbonus;
        public bool multibonus;
        public string multibonustype;
    }

    public class XpEcon
    {
        public bool showbalanceprofile;
        public bool econlevelup;
        public bool econleveldown;
        public bool econresetstats;
        public bool econresetskills;
        public bool econresetstat;
        public bool econresetskill;
        public double econlevelreward;
        public double econlevelreduction;
        public double econresetstatscost;
        public double econresetskillscost;
        public double econresetstatcost;
        public double econresetskillcost;
        public bool econstatlevelcost;
        public bool econskilllevelcost;
        public double econstatlevelcostmultiplier;
        public double econskilllevelcostmultiplier;
        public double econmentality;
        public double econdexterity;
        public double econmight;
        public double econcaptaincy;
        public double econweaponry;
        public double econninjary;
        public double econwoodcutter;
        public double econsmithy;
        public double econminer;
        public double econforager;
        public double econhunter;
        public double econfisher;
        public double econcrafter;
        public double econframer;
        public double econmedic;
        public double econscavenger;
        public double econelectrician;
        public double econdemolitionist;
        public double econtamer;
    }

    public class SRewards
    {
        public bool srewardlevelup;
        public bool srewardleveldown;
        public bool srewardresetstats;
        public bool srewardresetskills;
        public bool srewardresetstat;
        public bool srewardresetskill;
        public int srewardlevelupamt;
        public int srewardleveldownamt;
        public int srewardresetstatscost;
        public int srewardresetskillscost;
        public int srewardresetstatcost;
        public int srewardresetskillcost;
        public bool srewardstatlevelcost;
        public bool srewardskilllevelcost;
        public int srewardstatlevelcostmultiplier;
        public int srewardskilllevelcostmultiplier;
        public int srewardmentality;
        public int srewarddexterity;
        public int srewardmight;
        public int srewardcaptaincy;
        public int srewardweaponry;
        public int srewardninjary;
        public int srewardwoodcutter;
        public int srewardsmithy;
        public int srewardminer;
        public int srewardforager;
        public int srewardhunter;
        public int srewardfisher;
        public int srewardcrafter;
        public int srewardframer;
        public int srewardmedic;
        public int srewardscavenger;
        public int srewardelectrician;
        public int srewardemolitionist;
        public int srewardtamer;
    }

    public class Mentality
    {
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public double researchcost;
        public double researchcosttechtree;
        public double researchspeed;
        public double criticalchance;
        public double criticaldgm;
        public double damageincrease;
        public bool useotherresearchmod;
        public bool locktechtree;
        public int unlocktechtreelevel;
    }

    public class Dexterity
    {
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public double blockchance;
        public double blockamount;
        public double dodgechance;
        public double reducearmordmg;
        public double horsespeed;
        public double boatspeed;
        public double vehiclespeed;
        public double fuelreduce;
    }

    public class Might
    {
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public double armor;
        public double meleedmg;
        public double metabolism;
        public double bleedreduction;
        public double radreduction;
        public double heattolerance;
        public double coldtolerance;
    }

    public class Captaincy
    {
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public bool allownoteam;
        public double skillboost;
        public bool enablexpboost;
        public double xpboost;
        public float captaincydistance;
    }

    public class Weaponry
    {
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public double reductionchance;
        public double tool;
        public double powertools;
        public double meleeweapons;
        public double projectileweapons;
        public double mindamage;
        public double maxammo;
        public double maxammolimit;
        public bool skinboxdisable;
        public bool neverweartools;
        public bool neverwearweapons;
        public string reloadhook;
        public string excludedweapons;
        public bool useweaponmechanics;
    }

    public class Ninjary
    {
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public double patrolstealth;
        public double ch47stealth;
        public double bradleystealth;
        public double npcstealth;
        public double turretstealth;
        public double knifeincrease;
        public double swordincrease;
    }

    public class Woodcutter
    {
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public double gatherrate;
        public double bonusincrease;
        public double applechance;
    }

    public class Smithy
    {
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public double productionrate;
        public double productionspeed;
        public double fuelconsumption;
        public double metalchance;
        public int metalamount;
    }

    public class Miner
    {
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public double gatherrate;
        public double bonusincrease;
        public double fuelconsumption;
        public double metalchance;
        public int metalamount;
    }

    public class Forager
    {
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public double gatherrate;
        public double chanceincrease;
        public double grubwormincrease;
        public double randomchance;
        public Dictionary<int, RandomChanceList> randomChanceList;
    }

    public class RandomChanceList
    {
        public string shortname;
        public string displayname;
        public ulong SkinID;
        public int amount;
    }

    public class Hunter
    {
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public double gatherrate;
        public double bonusincrease;
        public double damageincrease;
        public double nightdmgincrease;
        public double bowdmgincrease;
        public bool excludelongrangeweapons;
        public bool excludemedrangeweapons;
    }

    public class Fisher
    {
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public double fishamountincrease;
        public double itemamountincrease;
        public double oxygenreduction;
        public double oxygentankreduction;
    }

    public class Crafter
    {
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public double craftspeed;
        public double craftcost;
        public double repairincrease;
        public double repaircost;
        public double conditionchance;
        public double conditionamount;
    }

    public class Framer
    {
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public double upgradecost;
        public double repaircost;
        public double repairtime;
        public int woodcost;
        public int stonecost;
        public int metalcost;
        public int armorcost;
    }

    public class Electrician
    {
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public int solarpaneldefault;
        public int smallbatterydefault;
        public int mediumbatterydefault;
        public int largebatterydefault;
        public int smallgeneratordefault;
        public int testgeneratordefault;
        public int electricwindmilldefault;
        public double solarpanelinputincrease;
        public double solarpanelmaxincrease;
        public double smallbatterymaxincrease;
        public double mediumbatterymaxincrease;
        public double largebatterymaxincrease;
        public double smallgeneratormaxincrease;
        public double testgeneratormaxincrease;
        public double electricwindmillincrease;
        public double electricwindmillmaxincrease;
        public bool allowminsolarinput;
        public int minsolarinput;
    }

    public class Demolitionist
    {
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public double explosivedudreduction;
        public double explosivedamage;
        public double explosiveradius;
    }

    public class Medic
    {
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public double revivehp;
        public double recoverhp;
        public double crafttime;
        public double tools;
        public double teas;
        public bool preventbandageboost;
    }

    public class Scavenger
    {
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public double scavlootchance;
        public double scavchance;
        public double scavmultiplier;
        public double customscavmultiplier;
        public bool customscavrandom;
        public bool usecustomscavlist;
        public bool drops;
        public bool crates;
        public bool uncrates;
        public bool lockedcrates;
        public bool hackcrates;
        public bool scientists;
        public bool componentsonly;
        public Dictionary<int, ScavChanceList> scavChanceList;
    }

    public class ScavChanceList
    {
        public string shortname;
        public string displayname;
        public ulong SkinID;
        public int amount;
        public int maxamount;
        public int requiredlevel;
    }

    public class Tamer
    {
        public bool enabletame;
        public int maxlvl;
        public int pointcoststart;
        public int costmultiplier;
        public bool tamechicken;
        public bool tameboar;
        public bool tamestag;
        public bool tamewolf;
        public bool tamebear;
        public bool tamepolarbear;
        public int chickenlevel;
        public int boarlevel;
        public int staglevel;
        public int wolflevel;
        public int bearlevel;
        public int polarbearlevel;
    }

    public class SQL
    {
        public bool enablesql;
        public string SQLhost;
        public int SQLport;
        public string SQLdatabase;
        public string SQLusername;
        public string SQLpassword;
    }

    public class BackpacksMod
    {
        public bool enablebackpacks;
        public string statorskill;
        public bool removeonunload;
        public SortedDictionary<int, BackPackSlots> BackPackSlots;
    }

    public class BackPackSlots
    {
        public int level;
        public int slots;
    }

    public class ZoneManagerMod
    {
        public string noxpgain;
        public string noxploss;
        public string disablestatsandskills;
    }

    public class EventHelperMod
    {
        public string noxpgain;
        public string noxploss;
        public string disablestatsandskills;
    }

    public class SurvivalArenaMod
    {
        public bool noxpgain;
        public bool noxploss;
        public bool disablestatsandskills;
    }

    public class RaidableBasesMod
    {
        public bool disableabilities;
        public bool noxploss;
        public bool noxpgain;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    public class SerializableConfiguration
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
    private void SaveData();
    private void LoadData();
    public class XPData
    {
        public Dictionary<string, XPRecord> XPerience;
    }

    public class XPRecord
    {
        public int rank;
        public int truelevel;
        public int trueexperience;
        public double level;
        public double experience;
        public double requiredxp;
        public int statpoint;
        public int skillpoint;
        public int Mentality;
        public int MentalityP;
        public int Dexterity;
        public int DexterityP;
        public int Might;
        public int MightP;
        public int Captaincy;
        public int CaptaincyP;
        public int Weaponry;
        public int WeaponryP;
        public int Ninjary;
        public int NinjaryP;
        public int WoodCutter;
        public int WoodCutterP;
        public int Smithy;
        public int SmithyP;
        public int Miner;
        public int MinerP;
        public int Forager;
        public int ForagerP;
        public int Hunter;
        public int HunterP;
        public int Fisher;
        public int FisherP;
        public int Crafter;
        public int CrafterP;
        public int Framer;
        public int FramerP;
        public int Electrician;
        public int ElectricianP;
        public int Medic;
        public int MedicP;
        public int Scavenger;
        public int ScavengerP;
        public int Demolitionist;
        public int DemolitionistP;
        public int Tamer;
        public int TamerP;
        public int Wood;
        public int Stone;
        public int Metal;
        public int Sulfur;
        public int Cactus;
        public int Berries;
        public int Pumpkin;
        public int Potato;
        public int Corn;
        public int Mushroom;
        public int Hemp;
        public int Seed;
        public bool Status;
        public bool DisableRank;
        public int UILocation;
        public string teatype;
        public double teacooldown;
        public DateTime resettimerstats;
        public DateTime resettimerskills;
        public DateTime playerfixdata;
        public int dash;
        public int dmgbar;
        public int profilebg;
        public bool fuelgauge;
        public bool speedometer;
        public int speedometertype;
        public bool enableconfirmationprompt;
        public bool showchatnotifications;
        public bool showchatprofileonconnect;
        public bool showwelcomepanel;
        public bool showchatxp;
        public bool exclude;
        public bool raidablebase;
        public string displayname;
        public string id;
    }

    public class DailyData
    {
        public Dictionary<string, DailyRecord> DailyXPerience;
    }

    public class DailyRecord
    {
        public double dailyexperience;
        public int dailystatresets;
        public int dailyskillresets;
        public DateTime lastexperiencereset;
        public DateTime laststatreset;
        public DateTime lastskillreset;
    }

    private class LootData
    {
        public Dictionary<ulong, Loot> LootRecords;
    }

    private class Loot
    {
        public List<string> id;
    }

    private class CorpseData
    {
        public Dictionary<ulong, Corpse> CorpseRecords;
    }

    private class Corpse
    {
        public ulong corpsecontainer;
        public List<string> id;
    }

    private class HorseData
    {
        public Dictionary<ulong, Horse> HorseRecords;
    }

    private class Horse
    {
        public ulong horse;
        public float maxSpeed;
        public float runSpeed;
        public float walkSpeed;
        public float trotSpeed;
        public ulong player;
    }

    private class WeaponData
    {
        public Dictionary<ulong, Weapon> WeaponRecords;
    }

    private class Weapon
    {
        public int defaultammo;
        public int maxammo;
        public double defaultreload;
        public double newreload;
        public double defaultdistance;
        public double maxdistance;
        public double defaultrange;
        public double maxrange;
        public ulong player;
        public NetworkableId weapondata;
    }

    private class BoatData
    {
        public Dictionary<ulong, Boat> BoatRecords;
    }

    private class Boat
    {
        public ulong boat;
        public float defaultSpeed;
        public ulong player;
    }

    private class VehicleData
    {
        public Dictionary<ulong, Vehicle> VehicleRecords;
    }

    private class Vehicle
    {
        public ulong vehicle;
        public float maxDriveSlip;
        public float reversePercentSpeed;
        public float driveForceToMaxSlip;
        public ulong player;
    }

    private class MinicopterData
    {
        public Dictionary<ulong, MiniCopterP> MinicopterRecords;
    }

    private class MiniCopterP
    {
        public ulong minicopter;
        public float maxRotorSpeed;
        public ulong player;
    }

    private class SnowmobData
    {
        public Dictionary<ulong, Snowmob> SnowmobRecords;
    }

    private class Snowmob
    {
        public ulong snowmob;
        public float terrain;
        public double engineKW;
        public ulong player;
    }

    private class SmithyData
    {
        public Dictionary<string, SmithyD> SmithyRecords;
    }

    private class SmithyD
    {
        public string resource;
        public float time;
    }

    private class ElectricianData
    {
        public Dictionary<ulong, ElectricianD> ElectricianRecords;
    }

    private class ElectricianD
    {
        public ulong id;
        public string type;
        public int defaultmaxoutput;
        public int newmaxoutput;
        public ulong owner;
    }

    private class HeliHits
    {
        public Dictionary<ulong, Heli> HeliRecords;
    }

    private class Heli
    {
        public ulong heli;
        public ulong player;
    }

    private readonly Core.MySql.Libraries.MySql sqlLibrary;
     Connection sqlConnection;
    private string RemoveSpecialCharacters(string name);
    private void CreatSQLTable();
    private void UpdateSQLTable();
    private void CreatePlayerDataSQL(BasePlayer player);
    private void UpdatePlayersDataSQL();
    private void UpdatePlayerDataSQL(BasePlayer player);
    private void CheckPlayerDataSQL(BasePlayer player);
    private void DeleteSQL();
    private void Init();
    private void OnServerInitialized();
    private void OnNewSave();
    private void OnPluginLoaded(Plugin name);
    private void Unload();
    private void OnServerShutdown();
    private void OnServerSave();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPlayerRespawn(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnPlayerKicked(BasePlayer player);
    private void CheckConfigValues();
    private void LibraryCheck();
    private void LoadImages(bool forcereload);
    private void Ready();
    private void DownloadImages();
    private void DownloadImagesCompleted(object sender, System.ComponentModel.AsyncCompletedEventArgs e);
    public XPRecord GetXPRecord(BasePlayer player);
    public DailyRecord GetDailyRecord(BasePlayer player);
    public XPRecord GetPlayerRecord(string player);
    public DailyRecord GetPlayerDailyRecord(string player);
    public Ranks GetXPRank(int rank);
    public BackgroundImgs GetBGImg(int bgid);
    private static BasePlayer FindPlayer(string playerid);
    private BasePlayer GetOwnerPlayer(Item item);
    private void CheckOnlineStatus();
    private void CheckStatsAndSkills(BasePlayer player);
    private void UpdateDisplayName(BasePlayer player);
    private void AddLootData(BasePlayer player, LootContainer lootcontainer);
    private void AddHorseData(BasePlayer player, RidableHorse horse);
    private void AddBoatData(BasePlayer player, BaseBoat boat);
    private void AddVehicleData(BasePlayer player, ModularCar car);
    private void AddMiniCopterData(BasePlayer player, Minicopter mini);
    private void AddSnowMobData(BasePlayer player, Snowmobile snowmob);
    private void AddWeaponData(BasePlayer player, BaseProjectile projectile, int defaultammo, int maxammo, double defaultreload, double newreload, double defaultdistance, double maxdistance, double defaultrange, double maxrange, NetworkableId weapondata);
    private void AddCorpseData(BasePlayer player, LootableCorpse corpse);
    private void AddSmithyData(string resource, float time);
    private void AddElectricianData(ulong id, string type, int defaultmaxoutput, int newmaxoutput, ulong owner);
    private double GetPlayerCooldown(ulong userID, string type);
    private double GetTeaCooldown(BasePlayer player);
    private string GetTeaTypes(BasePlayer player);
    private bool BetterChatActv();
    private bool ColouredChatAct();
    private bool IQChatAct();
    private object OnPlayerChat(BasePlayer player, string message, Chat.ChatChannel channel);
    private void OverrideChatMessage(BasePlayer player, string ranksig, string displayname, string level, string message, Chat.ChatChannel channel);
    private object OnBetterChat(Dictionary<string, object> chat);
    private void DailyLimit(bool reset);
    private void DailyLimitPlayer(BasePlayer player, bool reset);
    public void GainExp(BasePlayer player, double e);
    public void GainExpBasic(BasePlayer player, double e);
    private void GainExpID(string player, double e, int truexp, bool reset);
    private void GainExpAdmin(BasePlayer player, double e, int truexp, bool reset);
    private void GainExpAdminFix(string player, double e, int truexp, bool reset);
    public void GivePoints(BasePlayer player, string type, int amount);
    public void GivePointsOther(string player, string type, int amount);
    private void LvlUp(BasePlayer player, int chatstatpoint, int chatskillpoint, bool reset);
    private void LvlUpFix(string player);
    private void RankUp(BasePlayer player, double remainingxp, bool manualrank);
    private void RankUpFix(string player, double remainingxp);
    private void RankCheck(BasePlayer player, bool reset);
    private void LoseExp(BasePlayer player, double e);
    private void LoseExpAdmin(BasePlayer player, double e);
    private void LvlDown(BasePlayer player);
    private void StatUp(BasePlayer player, string stat);
    private void SkillUp(BasePlayer player, string skill);
    private void StatReset(BasePlayer player, string stat, bool bypass);
    private void StatsResetAll(BasePlayer player);
    private void SkillReset(BasePlayer player, string skill, bool bypass);
    private void SkillsResetAll(BasePlayer player);
    private void PlayerFixDataAll(BasePlayer player, bool reset);
    private void PlayerFixData(BasePlayer player, bool reset);
    private void PlayerReset(BasePlayer player);
    private void HarvestReset(BasePlayer player);
    private void SelectedPlayerReset(BasePlayer player, BasePlayer selectplayer);
    private void SelectedPlayerResetConsole(BasePlayer selectplayer);
    private void SelectedHarvestReset(BasePlayer player, BasePlayer selectplayer);
    private void SelectedLevelReset(BasePlayer player, BasePlayer selectplayer);
    private void SelectedRankReset(BasePlayer player, BasePlayer selectplayer);
    private void PlayerAllRankReset(BasePlayer player);
    private void PlayerAllLevelReset(BasePlayer player);
    private bool IsNight();
    private void KRBonus(BasePlayer player, string KillType, int reqkills, double bonus, int bonusend, bool enablemultibonus, string multibonustype);
    private void XPTeams(BasePlayer player, double e, string type);
    private void HarvestRecord(BasePlayer player, string item, int amount);
    public const string Tame;
    public const string PTameChicken;
    public const string PTameBoar;
    public const string PTameStag;
    public const string PTameWolf;
    public const string PTameBear;
    public const string PTamePolarBear;
    private void PetChecks(BasePlayer player, bool reset);
    private void BackPackChecks(BasePlayer player, string type, bool reset);
    private string GetScientistType(string scientist);
    private string GetLootType(string loot);
    private string GetDwellerType(string dweller);
    private void OnLootSpawn(LootContainer container);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo hitInfo);
    private void OnPlayerDeath(BasePlayer victim, HitInfo hitInfo);
    private void OnLootEntity(BasePlayer player, LootContainer lootcontainer);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    private void OnContainerDropItems(ItemContainer lootcontainer);
    private void CanLootEntity(BasePlayer player, LootableCorpse corpse);
    private void OnEntityKill(BaseNetworkable entity);
    private void OnHelicopterKilled(CH47HelicopterAIController heli);
    private object CanHelicopterTarget(PatrolHelicopterAI heli, BasePlayer player);
    private object CanBradleyApcTarget(BradleyAPC apc, BaseEntity entity);
    private object CanNpcAttack(BaseNpc npc, BaseEntity entity);
    private object OnHelicopterTarget(HelicopterTurret turret, BaseCombatEntity entity);
    private object OnNpcTarget(BaseEntity npc, BaseEntity entity);
    private object OnNpcTargetSense(BaseEntity owner, BaseEntity entity, AIBrainSenses brainSenses);
    private object OnTurretTarget(AutoTurret turret, BaseCombatEntity entity);
    private void OnExplosiveThrown(BasePlayer player, BaseEntity entity, ThrownWeapon item);
    private object OnExplosiveDud(DudTimedExplosive explosive);
    private void OnMissionSucceeded(BaseMission mission, BasePlayer assignee);
    private void OnMissionFailed(BaseMission mission, BasePlayer assignee);
    private int DetermineIngredientAmount(int item, int amount, BasePlayer player);
    public void CollectIngredient(int item, int amount, List<Item> collect, ItemCrafter itemCrafter, BasePlayer player);
     bool? OnIngredientsCollect(ItemCrafter itemCrafter, ItemBlueprint blueprint, ItemCraftTask task, int amount, BasePlayer player);
    private void OnIngredientsDetermine(Dictionary<int, int> overridenIngredients, ItemBlueprint blueprint, int amount, BasePlayer player);
    private void OnItemCraft(ItemCraftTask task, BasePlayer player);
    private void OnItemCraftFinished(ItemCraftTask task, Item item, ItemCrafter itemCrafter);
    private object OnItemUse(Item item);
    private List<ItemAmount> RepairItems(BasePlayer player, Item item);
    private List<ItemAmount> ApplyItemCostReduction(BasePlayer player, List<ItemAmount> list, Item item);
    private bool PlayerCanRepair(BasePlayer player, List<ItemAmount> list);
    private void TakeItems(BasePlayer player, List<ItemAmount> list);
    private void OnItemRepair(BasePlayer player, Item item);
    private void OnEntityBuilt(Planner plan);
    private void OnStructureUpgrade(BuildingBlock buildingBlock, BasePlayer player, BuildingGrade.Enum newgrade);
    private void CheckStructureUpgrade(BuildingBlock buildingBlock, BasePlayer player, BuildingGrade.Enum newGrade);
    public bool CanAffordUpgrade(BuildingBlock buildingBlock, BasePlayer player, BuildingGrade.Enum grade);
    private void RefundMaterials(BuildingBlock buildingBlock, BasePlayer player, BuildingGrade.Enum grade);
    private void OnStructureRepair(BaseCombatEntity entity, BasePlayer player);
    public static readonly string[] CookedItems;
    private Dictionary<string, int> lowTemps;
    private Dictionary<string, int> highTemps;
     ItemModCookable GetCookables(string shortname);
    private void OnFuelConsume(BaseOven oven, Item fuel, ItemModBurnable burnable, BaseOven.IndustrialSlotMode IndustrialMode);
    private void OnOvenToggle(BaseOven oven, BasePlayer player, BaseOven.IndustrialSlotMode IndustrialMode);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private void OnCollectiblePickup(CollectibleEntity collectible, BasePlayer player);
    private void OnGrowableGathered(GrowableEntity growable, Item item, BasePlayer player);
     void OnMealCrafted(BasePlayer player, string meal, Dictionary<string, int> ingredients, bool isIngredient);
     bool CheckPlayerLocation(BasePlayer player, string type);
     void OnEnterZone(string ZoneID, BasePlayer player);
     void OnExitZone(string ZoneID, BasePlayer player);
    private void OnPlayerEnteredRaidableBase(BasePlayer player);
    private void OnPlayerExitedRaidableBase(BasePlayer player);
    private bool CheckMonumentOwner(BasePlayer player);
     void EMOnEventJoined(BasePlayer player, string eventname);
    private object PlayerMetabolismControl(PlayerMetabolism metabolism, BasePlayer player, float delta, int might);
    private object OnRunPlayerMetabolism(PlayerMetabolism metabolism, BasePlayer player, float delta);
    private void OnPlayerHealthChange(BasePlayer player);
    private object OnHealingItemUse(MedicalTool tool, BasePlayer player);
    private void OnPlayerRevive(BasePlayer reviver, BasePlayer player);
    private void OnPlayerRecovered(BasePlayer player);
    private void MightAttributes(BasePlayer player, bool reset);
    private object OnPlayerAddModifiers(BasePlayer player, Item item, ItemModConsumable consumable);
    private void PlayerArmor(BasePlayer player, bool reset);
    private void OnPlayerInput(BasePlayer player, InputState input);
    private void OnItemAction(Item item, string action, BasePlayer player);
    private readonly Dictionary<Rarity, int> rarityValues;
    private object OnResearchCostDetermine(Item item, ResearchTable researchTable);
    [HookMethod("OnResearchCost")]
    private int OnResearchCost(int rarityvalue, BasePlayer player);
    [HookMethod("OnItemResearchReduction")]
    private float OnItemResearchReduction(float value, BasePlayer player);
    private void OnItemResearch(ResearchTable researchTable, Item item, BasePlayer player);
    private bool CheckUnlockPath(BasePlayer player, TechTreeData.NodeInstance node, TechTreeData techTree);
    private object CanUnlockTechTreeNode(BasePlayer player, TechTreeData.NodeInstance node, TechTreeData techTree);
    private object OnTechTreeNodeUnlock(Workbench workbench, TechTreeData.NodeInstance node, BasePlayer player);
    private void OnMixingTableToggle(MixingTable table, BasePlayer player);
    private double CaptaincyTeamSkillBoost(BasePlayer player);
    private double CaptaincyTeamXPBoost(BasePlayer player);
    private bool CaptaincyTeamDistance(BasePlayer player);
    private void RandomForagerItem(BasePlayer player);
    private void RandomScavengerItem(BasePlayer player);
    private void IncreaseLootContainers(BasePlayer player, LootContainer lootcontainer);
    private void IncreaseLootContainerDrops(ItemContainer lootcontainer);
    private void IncreaseLootCorpse(BasePlayer player, LootableCorpse corpse);
    private void OnFishCatch(Item fish, BaseFishingRod fishingRod, BasePlayer player);
    private void OnMagazineReload(BaseProjectile projectile, int desiredAmount, BasePlayer player);
    private void OnWeaponReload(BaseProjectile projectile, BasePlayer player);
    private object OnLoseCondition(Item item, float amount);
    private void OnSolarPanelSunUpdate(SolarPanel panel, int currentEnergy);
    private void OnInputUpdate(IOEntity entity, int inputAmount, int slot);
    private void OnOutputUpdate(IOEntity entity);
    private void OnEntitySpawned(BaseNetworkable entity);
    private void LoadElectricianEntities();
    private void CheckElectricianEntities(BasePlayer player, bool reset);
    private void OnEntityMounted(BaseMountable entity, BasePlayer player);
    private void OnEntityDismounted(BaseMountable entity, BasePlayer player);
    private void DefaultHorseData(RidableHorse horse);
    private void DefaultBoatData(BaseBoat boat);
    private void DefaultVehicleData(ModularCar car);
    private void DefaultMiniCopterData(Minicopter mini);
    private void DefaultSnowMobData(Snowmobile snowmob);
    private void ChangeHorseSpeed(BasePlayer player);
    private void ChangeBoatSpeed(BasePlayer player);
    private void ChangeVehicleSpeed(BasePlayer player);
    private void ChangeMiniCopterSpeed(BasePlayer player);
    private void ChangeSnowMobSpeed(BasePlayer player);
    private int GetFuel(BaseMountable entity);
    private string GetSpeed(BasePlayer player, BaseMountable entity);
    private object CanUseFuel(EntityFuelSystem fuelSystem);
    private void Openhelp(BasePlayer player, string command, string[] args);
    private void Openplayerstats(BasePlayer player, string command, string[] args);
    private void Showplayerstatschat(BasePlayer player, string command, string[] args);
    private void Opentopplayers(BasePlayer player, string command, string[] args);
    private void Playeraddstat(BasePlayer player, string command, string[] args);
    private void Playeraddskill(BasePlayer player, string command, string[] args);
    private void Playerresetstats(BasePlayer player, string command, string[] args);
    private void Playerresetskills(BasePlayer player, string command, string[] args);
    private void Playerresetall(BasePlayer player, string command, string[] args);
    private void Playerliveuichange(BasePlayer player, string command, string[] args);
    private void Adminitemchange(BasePlayer player, string command, string[] args);
    private void Showadminhelp(BasePlayer player, string command, string[] args);
    public void Openadminpanel(BasePlayer player, string command, string[] args);
    private void Adminresetxperience(BasePlayer player, string command, string[] args);
    private void Adminxpgive(BasePlayer player, string command, string[] args);
    private void Adminpointsgive(BasePlayer player, string command, string[] args);
    private void Adminxpgiveall(BasePlayer player, string command, string[] args);
    private void Adminxptake(BasePlayer player, string command, string[] args);
    private void Adminfixdata(BasePlayer player, string command, string[] args);
    private void Adminxpresetplayer(BasePlayer player, string command, string[] args);
    private void AdminHarvestReset(BasePlayer player, string command, string[] args);
    private void AdminLevelReset(BasePlayer player, string command, string[] args);
    private void AdminRankReset(BasePlayer player, string command, string[] args);
    private void AdminStatReset(BasePlayer player, string command, string[] args);
    private void AdminSkillReset(BasePlayer player, string command, string[] args);
    private void AdminLevelResetAll(BasePlayer player, string command, string[] args);
    private void AdminRankResetAll(BasePlayer player, string command, string[] args);
    private void AdminExcludePlayer(BasePlayer player, string command, string[] args);
    private void AdminGiveItem(BasePlayer player, string command, string[] args);
    private bool CanUseConsole(ConsoleSystem.Arg arg);
    [ConsoleCommand("xpresetall")]
    private void Consolereset(ConsoleSystem.Arg arg);
    [ConsoleCommand("xpdailyreset")]
    private void Consoleresetdailyxptimer(ConsoleSystem.Arg arg);
    [ConsoleCommand("xpdailyresetplayer")]
    private void Consoleresetdailyxpplayer(ConsoleSystem.Arg arg);
    [ConsoleCommand("xpexcludeplayer")]
    private void Consoleexcludeplayer(ConsoleSystem.Arg arg);
    [ConsoleCommand("xpresetplayer")]
    private void Consoleresetplayer(ConsoleSystem.Arg arg);
    [ConsoleCommand("xpresetstat")]
    private void Consoleresetstat(ConsoleSystem.Arg arg);
    [ConsoleCommand("xpresetskill")]
    private void Consoleresetskill(ConsoleSystem.Arg arg);
    [ConsoleCommand("xpgive")]
    private void Consolegivexp(ConsoleSystem.Arg arg);
    [ConsoleCommand("xptake")]
    private void Consoletakexp(ConsoleSystem.Arg arg);
    [ConsoleCommand("xpitem")]
    private void Consolexpitem(ConsoleSystem.Arg arg);
    private const string XPerienceLivePrimary;
    private const string XPerienceLiveIcon;
    private const string XPerienceLiveData;
    private const string XPerienceLiveFuel;
    private const string XPerienceLiveFuelBar;
    private const string XPerienceLiveSpeed;
    private const string XPerienceLiveSpeedIcon;
    private const string XPerienceLiveSpeedBar;
    private const string XPerienceLiveDashPanel;
    private const string XPerienceLiveDashPanelSet;
    private const string XPeriencePlayerProfileOutside;
    private const string XPeriencePlayerProfile;
    private const string XPeriencePlayerProfileMenu;
    private const string XPeriencePlayerProfileMain;
    private const string XPeriencePlayerProfileStatsAndSkills;
    private const string XPeriencePlayerProfileStatsAndSkillsMenu;
    private const string XPeriencePlayerProfileSettings;
    private const string XPeriencePlayerProfileKills;
    private const string XPeriencePlayerProfileKillsPages;
    private const string XPeriencePlayerProfileRaids;
    private const string XPeriencePlayerProfileRaidsPages;
    private const string XPeriencePlayerProfileHelp;
    private const string XPeriencePlayerProfileHelpPages;
    private const string XPeriencePlayerControlPromptBox;
    private const string XPeriencePlayerControlPrompt;
    private const string XPeriencePlayerDMGSkins;
    private const string XPeriencePlayerBGImgs;
    private const string XPeriencePlayerCalculations;
    private const string XPeriencePlayerCalculationsLevelSelection;
    private const string XPeriencePlayerCalculationsRanksSelection;
    private const string XPerienceTopSelection;
    private const string XPerienceTopInner;
    private const string XPerienceTopPageSelection;
    private const string XPerienceAdminPanelMain;
    private const string XPerienceAdminPanelMenu;
    private const string XPerienceAdminPanelInfo;
    private const string XPerienceAdminPanelLevelXP;
    private const string XPerienceAdminPanelRanks;
    private const string XPerienceAdminPanelStats;
    private const string XPerienceAdminPanelSkills;
    private const string XPerienceAdminPanelSkillItems;
    private const string XPerienceAdminPanelTimerColor;
    private const string XPerienceAdminPanelOtherMods;
    private const string XPerienceAdminPanelSQL;
    private const string XPerienceAdminPanelReset;
    private const string XPerienceAdminPanelInfoBox;
    private const string XPerienceAdminPanelAddon;
    private const string XPerienceAdminPanelDailyLimits;
    private const string XPerienceAdminPanelSoundEffects;
    private const string XPerienceAdminPanelElectricianSettings;
    private const string XPerienceAdminPanelProfileBackgrounds;
    private const string XPerienceAdminPanelImages;
    private const string XPerienceAdminPanelSpecialGroups;
    private const string XPerienceAdminPanelBackpackSelection;
    private const string XPerienceicon;
    private const string XPeriencelogo;
    private const string XPeriencementality;
    private const string XPeriencedexterity;
    private const string XPeriencemight;
    private const string XPeriencecaptaincy;
    private const string XPerienceweaponry;
    private const string XPerienceninjary;
    private const string XPeriencewoodcutter;
    private const string XPeriencesmithy;
    private const string XPerienceminer;
    private const string XPerienceforager;
    private const string XPeriencehunter;
    private const string XPeriencefisher;
    private const string XPeriencecrafter;
    private const string XPerienceframer;
    private const string XPeriencemedic;
    private const string XPeriencescavenger;
    private const string XPerienceelectrician;
    private const string XPeriencedemolitionist;
    private const string XPeriencetamer;
    private const string XPeriencechicken;
    private const string XPerienceboar;
    private const string XPeriencestag;
    private const string XPeriencewolf;
    private const string XPeriencebear;
    private const string XPeriencepolarbear;
    private const string XPeriencearchery;
    private const string XPeriencewizardry;
    private const string XPerienceonline;
    private const string XPerienceoffline;
    private const string XPeriencebackpack;
    private const string XPeriencelevel;
    private const string XPeriencexp;
    private const string XPeriencearmor;
    private const string XPeriencelevel0;
    private const string XPeriencelevel2;
    private const string XPeriencelevel4;
    private const string XPeriencelevel6;
    private const string XPeriencelevel8;
    private const string XPeriencelevel10;
    private const string XPeriencefuelguage;
    private const string XPeriencespeedometer;
    private const string XPeriencedash;
    private const string XPerienceraideasy;
    private const string XPerienceraidmedium;
    private const string XPerienceraidhard;
    private const string XPerienceraidexpert;
    private const string XPerienceraidnightmare;
    private const string XPerienceprofilebg;
    private const string XPeriencemenubg;
    private const string XPerienceaddondmgbarhealth1;
    private const string XPerienceaddondmgbarhealth2;
    private const string XPerienceaddondmgbarhealth3;
    private const string XPerienceaddondmgbarhealth4;
    private const string XPerienceaddondmgbarhealth5;
    private const string XPerienceaddondmgbarhealth6;
    private const string XPerienceaddondmgbarhealth7;
    private const string XPerienceaddondmgbarhealth8;
    private const string XPerienceaddondmgbarhealth9;
    private const string XPerienceaddondmgbarhealth10;
    private object TextColor(BasePlayer player, string type, double value, bool enabled);
    private object ValueSymbol(string type, double value, string symbol);
    private object LiveUISelection(string selection, int value);
    private object DashSelection(int selection, int value);
    private object DisableRankSelection(string selection, bool value, string color);
    private object DisableSelection(string selection, bool value, string color);
    private object ColorConverter(string color);
    private object LiveColorConverter(string color);
    private CuiPanel XPUIPanel(string anchorMin, string anchorMax, string color);
    private CuiPanel XPUIPanel2(string anchorMin, string anchorMax, string offsetMin, string offsetMax, string color);
    private CuiLabel XPUILabel(string text, int i, float height, TextAnchor align, int fontSize, string xMin, string xMax, string color);
    private CuiButton XPUIButton(string command, double i, float rowHeight, int fontSize, string color, string content, string xMin, string xMax, TextAnchor align, string fcolor);
    private CuiButton XPUIMenuButton(string command, double i, float rowHeight, int fontSize, string color, string content, string xMin, string xMax, TextAnchor align, string fcolor, double space);
    private CuiElement XPUIImage(string parent, string image, int i, float imgheight, string xMin, string xMax);
    private CuiElement XPUIInput(string parent, string command, int i, float height, int fontSize, string content, string xMin, string xMax, TextAnchor align, string color, int limit);
    private CuiPanel XPUIInputbackground(int i, float height, string color, string xMin, string xMax);
    private CuiButton XPToggle(string command, int i, float height, bool value, string xMin, double width);
    private void DestroyUi(BasePlayer player, string name);
    [ConsoleCommand("xp.playercontrol")]
    private void Cmdplayercontrolnew(ConsoleSystem.Arg arg);
    [ConsoleCommand("xp.playeredits")]
    private void Cmdplayeredits(ConsoleSystem.Arg arg);
    [ConsoleCommand("xp.topplayers")]
    private void Cmdtopplayernew(ConsoleSystem.Arg arg);
    private string LevelIcon(BasePlayer player, int percent);
    private object PlayerTimeValues(BasePlayer player, string type, double value);
    private object PlayerInfoValues(BasePlayer player, BaseEntity entity, string type);
    private string SelectedIcon(string type, string data);
    private object SelectedData(string type, string data, string page);
    private double IconAdjustment(double adjustment, string type);
    private double RankBoosts(BasePlayer player, string type, double amount, bool enabled);
    private string ConvertUnixTimeStampToDateTime(int unixtime);
    public class Deaths
    {
        public string victim;
        public string victimname;
        public string attacker;
        public string attackername;
        public string weapon;
        public string lastdamage;
        public float distance;
        public long timestamp;
    }

    private class Raids
    {
        public Vector3 Location;
        public string BaseName;
        public int mode;
        public bool allowPVP;
        public string id;
        public float spawnTime;
        public float despawnTime;
        public float loadTime;
        public ulong ownerid;
        public List<ulong> raiders;
        public DateTime spawnDateTime;
        public DateTime despawnDateTime;
    }

    public class DmgBarImgs
    {
        public string name;
        public string url;
    }

    private static string PositionToGrid(Vector3 position);
    private object GetKillRecords(string player, string entity);
    private IEnumerable<XPRecord> GetTopXP(int page, int takeCount, string selection);
    private void ClearPlayerUIs(BasePlayer player, bool all, bool live);
    public void LiveStats(BasePlayer player, bool zone, string consumable);
    private void DashPanel(BasePlayer player, bool active, BaseMountable entity);
    public void PlayerProfile(BasePlayer player, BasePlayer otherplayer);
    public void PlayerProfileMain(BasePlayer player, BasePlayer otherplayer);
    private void PlayerProfileStatsAndSkills(BasePlayer player, string data, string type);
    private void PlayerProfileRecords(BasePlayer player, string data, BasePlayer otherplayer, int page, string ordertype, string order);
    private void PlayerProfileRaids(BasePlayer player, int page, BasePlayer otherplayer);
    private void PlayerTopList(BasePlayer player, int page, string selection, int number);
    private void PlayerSettings(BasePlayer player);
    private void PlayerHelp(BasePlayer player, string data, string type);
    private void PlayerCalculationPageLevels(BasePlayer player, int page, int rank);
    private void PlayerCalculationPageRanks(BasePlayer player, int page);
    private void PlayerCalculationPageStats(BasePlayer player);
    private void PlayerCalculationPageSkills(BasePlayer player);
    private void PlayerProfileBGImgs(BasePlayer player, int page);
    private void PlayerDamageBarSkins(BasePlayer player, int page);
    private void PlayerPromptBox(BasePlayer player, string function, string types, string name);
    private void PlayerStatsChat(BasePlayer player);
    [TextArea]
    private string UserInputText;
    [ConsoleCommand("xp.admin")]
    private void Cmdadminxp(ConsoleSystem.Arg arg);
    [ConsoleCommand("xp.config")]
    private void Cmdadminxpconfig(ConsoleSystem.Arg arg);
    [ConsoleCommand("xp.color")]
    private void Cmdadminxpcolors(ConsoleSystem.Arg arg);
    [ConsoleCommand("xp.electrician")]
    private void Cmdadminxpelectrician(ConsoleSystem.Arg arg);
    [ConsoleCommand("xp.ranks")]
    private void Cmdadminxpranks(ConsoleSystem.Arg arg);
    [ConsoleCommand("xp.images")]
    private void Cmdadminimages(ConsoleSystem.Arg arg);
    [ConsoleCommand("xp.skillitems")]
    private void Cmdadminskillitems(ConsoleSystem.Arg arg);
    [ConsoleCommand("xp.specialgroups")]
    private void Cmdadminspecialgroups(ConsoleSystem.Arg arg);
    private void ClearUIs(BasePlayer player);
    private void AdminControlPanel(BasePlayer player);
    private void AdminInfoPage(BasePlayer player);
    private void AdminLevelPage(BasePlayer player);
    private void AdminRanksPage(BasePlayer player, string page, int rank);
    private void AdminDailyLimitsPage(BasePlayer player);
    private void AdminStatsPage(BasePlayer player, string stat);
    private void AdminSkillsPage(BasePlayer player, string skill);
    private void AdminSkillItems(BasePlayer player, string page, string skill, int item);
    private void AdminSpecialGroups(BasePlayer player, string page, int groupid);
    private void AdminPlayerInfoPage(BasePlayer player);
    private void AdminTimerColorPage(BasePlayer player);
    private void AdminImagePaths(BasePlayer player, string page, int bg);
    private void AdminOtherModsPage(BasePlayer player, string mod, string page, int option);
    private void OtherMods_KillRecords(BasePlayer player);
    private void OtherMods_ZoneManager(BasePlayer player);
    private void OtherMods_EventHelper(BasePlayer player);
    private void OtherMods_Economics(BasePlayer player);
    private void OtherMods_ServerRewards(BasePlayer player);
    private void OtherMods_Pets(BasePlayer player);
    private void OtherMods_Backpacks(BasePlayer player, string page, int option);
    private void OtherMods_BackpacksSelection(BasePlayer player, string selected);
    private void AdminSoundEffectsPage(BasePlayer player);
    private void AdminSQLPage(BasePlayer player);
    private void AdminResetPage(BasePlayer player);
    private void AdminAddonPage(BasePlayer player);
    private void AdminElectricianSettings(BasePlayer player);
    private void AdminProfileBackgrounds(BasePlayer player, int selected, int page);
    private string XPLang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
    [HookMethod("GiveXPID")]
     void GiveXPID(ulong playerid, double amount);
    [HookMethod("GiveXP")]
     void GiveXP(BasePlayer player, double amount);
    [HookMethod("GiveXPBasic")]
     void GiveXPBasic(BasePlayer player, double amount);
    [HookMethod("GiveStatPoints")]
     void GiveStatPoints(BasePlayer player, int amount);
    [HookMethod("GiveSkillPoints")]
     void GiveSkillPoints(BasePlayer player, int amount);
    [HookMethod("TakeXP")]
     void TakeXP(BasePlayer player, double amount);
    [HookMethod("GetXPCache")]
     string GetXPCache(BasePlayer player, string info);
}

public static class RandomNumber
{
    private static readonly RNGCryptoServiceProvider _generator;
    public static int Between(int minimumValue, int maximumValue);
}

public class Configuration : SerializableConfiguration
{
    [JsonProperty("Player Chat Commands")]
    public PlayerChatCommands playerchatCommands;
    [JsonProperty("Admin Chat Commands")]
    public AdminChatCommands adminchatCommands;
    [JsonProperty("Player Info Box")]
    public PlayerProfileSettings playerprofilesettings;
    [JsonProperty("Default Options")]
    public DefaultOptions defaultOptions;
    [JsonProperty("Sound Effects")]
    public SoundEffects soundEffects;
    [JsonProperty("UI Text Colors")]
    public UITextColor uitextColor;
    [JsonProperty("Image Icons")]
    public ImageIcons imageicons;
    [JsonProperty("XP - Level Config")]
    public XpLevel xpLevel;
    [JsonProperty("Daily Timer Config")]
    public DailyTimer dailytimer;
    [JsonProperty("Daily XP Limit Config")]
    public DailyXpLimit dailyxpLimit;
    [JsonProperty("Daily Reset Limit Config")]
    public DailyResetLimit dailyresetLimit;
    [JsonProperty("XP - Level Ranks")]
    public XpLevelRanks xpLevelRanks;
    [JsonProperty("Rank Boosts")]
    public RankBoostsSettings Rankboostssettings;
    [JsonProperty("Special Groups")]
    public SpecialGroups specialGroups;
    [JsonProperty("XP - Night Bonus")]
    public NightBonus nightBonus;
    [JsonProperty("XP - Gain Amounts")]
    public XpGain xpGain;
    [JsonProperty("XP - Gather Amounts")]
    public XpGather xpGather;
    [JsonProperty("XP - Building Amounts")]
    public XpBuilding xpBuilding;
    [JsonProperty("XP - Teams")]
    public XpTeams xpTeams;
    [JsonProperty("XP - Mission Amounts")]
    public XpMissions xpMissions;
    [JsonProperty("XP - Reducer Amounts")]
    public XpReducer xpReducer;
    [JsonProperty("BonusXP - Bonus Amounts (requires KillRecords plugin)")]
    public XpBonus xpBonus;
    [JsonProperty("Economics Rewards (requires Economics plugin)")]
    public XpEcon xpEcon;
    [JsonProperty("Server Rewards (requires ServerRewards plugin)")]
    public SRewards sRewards;
    [JsonProperty("Mentality Stat")]
    public Mentality mentality;
    [JsonProperty("Dexterity Stat")]
    public Dexterity dexterity;
    [JsonProperty("Might Stat")]
    public Might might;
    [JsonProperty("Captaincy Stat")]
    public Captaincy captaincy;
    [JsonProperty("Weaponry Stat")]
    public Weaponry weaponry;
    [JsonProperty("Ninjary Stat")]
    public Ninjary ninjary;
    [JsonProperty("WoodCutter Skill")]
    public Woodcutter woodcutter;
    [JsonProperty("Smithy Skill")]
    public Smithy smithy;
    [JsonProperty("Miner Skill")]
    public Miner miner;
    [JsonProperty("Forager Skill")]
    public Forager forager;
    [JsonProperty("Hunter Skill")]
    public Hunter hunter;
    [JsonProperty("Fisher Skill")]
    public Fisher fisher;
    [JsonProperty("Crafter Skill")]
    public Crafter crafter;
    [JsonProperty("Framer Skill")]
    public Framer framer;
    [JsonProperty("Medic Skill")]
    public Medic medic;
    [JsonProperty("Scavenger Skill")]
    public Scavenger scavenger;
    [JsonProperty("Electrician Skill")]
    public Electrician electrician;
    [JsonProperty("Demolitionist Skill")]
    public Demolitionist demolitionist;
    [JsonProperty("Tamer Skill")]
    public Tamer tamer;
    [JsonProperty("SQL Info")]
    public SQL sql;
    [JsonProperty("Backpacks Mod")]
    public BackpacksMod backpacksmod;
    [JsonProperty("ZoneManager Mod")]
    public ZoneManagerMod zonemanagermod;
    [JsonProperty("EventHelper Mod")]
    public EventHelperMod eventhelpermod;
    [JsonProperty("SurvivalArena Mod")]
    public SurvivalArenaMod survivalarenamod;
    [JsonProperty("Raidable Bases")]
    public RaidableBasesMod raidablebasesmod;
}

public class PlayerChatCommands
{
    public string openplayerstats;
    public string openplayerstats2;
    public string openplayerstats3;
    public string showplayerstatschat;
    public string opentopplayers;
    public string playeraddstat;
    public string playeraddskill;
    public string playerresetstats;
    public string playerresetskills;
    public string playerresetall;
    public string playerliveuichange;
    public string openhelp;
}

public class AdminChatCommands
{
    public string showadminhelp;
    public string openadminpanel;
    public string adminresetxperience;
    public string adminxpgive;
    public string adminxpgiveall;
    public string adminpointsgive;
    public string adminxptake;
    public string adminresetplayer;
    public string adminfixdata;
    public string adminitemchange;
    public string adminresetharvest;
    public string adminresetlevelonly;
    public string adminresetrankonly;
    public string adminresetstat;
    public string adminresetskill;
    public string adminresetlevelonlyall;
    public string adminresetrankonlyall;
    public string adminexcludeplayer;
    public string admingiveitem;
}

public class PlayerProfileSettings
{
    public bool showunusedeffects;
    public bool useplayeravatar;
    public bool profilemenusettings;
    public bool profilemenutopplayers;
    public bool profilemenuraids;
    public bool profilemenuhelp;
    public bool profilemenucalculations;
    public bool skillshelp;
    public bool profilemenuwelcome;
    public bool playtime;
    public bool alivetime;
    public bool sleepingtime;
    public bool swimingtime;
    public bool drivingtime;
    public bool flyingtime;
    public bool boatingtime;
    public bool basetime;
    public bool monumenttime;
    public bool wildernesstime;
    public bool metersran;
    public bool meterswalked;
    public bool lastdmgrec;
    public bool lastdmgrecby;
    public bool lastdmgdelt;
    public bool lastdmgdeltto;
    public string AnchorMin;
    public string AnchorMax;
    public string OffsetMin;
    public string OffsetMax;
    public string InsideAnchorMin;
    public string InsideAnchorMax;
    public int menutype;
    public bool usebgimage;
    public int profilebg;
    public bool allowprofilebgchange;
    public bool usemenubgimage;
    public double bgfadein;
    public double menuwidth;
    public double menuheight;
    public double menubuttonheight;
    public int menubuttonfont;
}

public class DefaultOptions
{
    public bool userpermissions;
    public int liveuistatslocation;
    public bool liveuistatslocationmoveable;
    public bool showchatprofileonconnect;
    public int NotifcationCooldown;
    public bool restristresets;
    public bool allowrespec;
    public int resetminsstats;
    public int resetminsskills;
    public bool bypassadminreset;
    public int vipresetminstats;
    public int vipresetminsskills;
    public int playerfixdatatimer;
    public bool disableplayerfixdata;
    public bool disablearmorchat;
    public bool hardcorenoreset;
    public bool allowplayersearch;
    public bool allowplayerreset;
    public int topplayersperpage;
    public bool showonlinestatus;
    public bool useprogressivelevelicons;
    public bool showfuelguage;
    public bool showspeedometer;
    public int speedometertype;
    public bool dropsgotoplayerinventory;
    public bool wipedataonnewsave;
    public bool enabledashpanel;
    public bool enableconfirmationprompt;
    public bool showchatnotifications;
    public bool showlevelinchat;
    public bool hidechatnotifications;
    public bool debugmode;
}

public class SoundEffects
{
    public bool levelup;
    public bool leveldown;
    public bool rankup;
    public bool statup;
    public bool skillup;
    public bool statreset;
    public bool skillreset;
    public bool scavengerloot;
    public bool foragerloot;
    public string levelupeffect;
    public string leveldowneffect;
    public string rankupeffect;
    public string statupeffect;
    public string skillupeffect;
    public string statreseteffect;
    public string skillreseteffect;
    public string scavengerlooteffect;
    public string foragerlooteffect;
}

public class UITextColor
{
    public string defaultcolor;
    public string level;
    public string ranklevel;
    public string rankxp;
    public string rankname;
    public string experience;
    public string nextlevel;
    public string remainingxp;
    public string statskilllevels;
    public string perks;
    public string unspentpoints;
    public string spentpoints;
    public string pets;
    public string mentality;
    public string dexterity;
    public string might;
    public string captaincy;
    public string weaponry;
    public string Ninjary;
    public string woodcutter;
    public string smithy;
    public string miner;
    public string forager;
    public string hunter;
    public string fisher;
    public string crafter;
    public string framer;
    public string medic;
    public string scavenger;
    public string electrician;
    public string demolitionist;
    public string tamer;
    public string xpbar;
    public string armorbar;
}

public class ImageIcons
{
    public bool uselocalpath;
    public string rootpath;
    public string xperiencelogo;
    public string mainicon;
    public string mentality;
    public string dexterity;
    public string might;
    public string captaincy;
    public string weaponry;
    public string ninjary;
    public string woodcutter;
    public string smithy;
    public string miner;
    public string forager;
    public string hunter;
    public string fisher;
    public string crafter;
    public string framer;
    public string medic;
    public string scavenger;
    public string electrician;
    public string demolitionist;
    public string tamer;
    public string chicken;
    public string boar;
    public string stag;
    public string wolf;
    public string bear;
    public string polarbear;
    public string archery;
    public string wizardry;
    public string online;
    public string offline;
    public string backpack;
    public string xp;
    public string level;
    public string armor;
    public string level0;
    public string level2;
    public string level4;
    public string level6;
    public string level8;
    public string level10;
    public string dash;
    public string raideasy;
    public string raidmedium;
    public string raidhard;
    public string raidexpert;
    public string raidnightmare;
    public string profilebg;
    public string menubg;
    public Dictionary<int, BackgroundImgs> bgimages;
}

public class BackgroundImgs
{
    public string name;
    public string url;
}

public class XpLevel
{
    public double levelstart;
    public double levelmultiplier;
    public int maxlevel;
    public double levelxpboost;
    public int statpointsperlvl;
    public int skillpointsperlvl;
    public bool alwaysearnxp;
    public bool fullhealth;
    public bool fullmetabolism;
}

public class XpLevelRanks
{
    public bool enableresetranks;
    public bool resetallstatsskills;
    public bool allowplayerdisable;
    public bool increaselevelmultiplier;
    public double levelmultiplierincrease;
    public int maxresetrank;
    public bool enablerankxpboost;
    public double rankxpboost;
    public bool rankstatboost;
    public double rankstatboostamount;
    public int rankstatpointstart;
    public int rankstatpointincrease;
    public bool rankskillboost;
    public double rankskillboostamount;
    public int rankskillpointstart;
    public int rankskillpointincrease;
    public bool keepremainingxp;
    public bool showtruelevelprofile;
    public bool showrankinchat;
    public bool showtruexpprofile;
    public bool showrankinliveui;
    public bool keepgrouponrank;
    public Dictionary<int, Ranks> ranks;
}

public class RankBoostsSettings
{
    public bool researchcost;
    public bool researchspeed;
    public bool block;
    public bool armor;
    public bool distance;
    public bool meleedmg;
    public bool metabolism;
    public bool woodcuttergr;
    public bool woodcutterbonus;
    public bool smithypr;
    public bool smithyps;
    public bool smithyfc;
    public bool smithyhqmc;
    public bool smithyhqma;
    public bool minergr;
    public bool minerbonus;
    public bool minermfc;
    public bool minerfuel;
    public bool minermfa;
    public bool fisherfa;
    public bool fisheria;
    public bool fisherotr;
    public bool foragergr;
    public bool foragergwa;
    public bool foragerric;
    public bool huntergr;
    public bool hunterbonus;
    public bool hunterdmg;
    public bool hunterndmg;
    public bool crafterspeed;
    public bool craftercost;
    public bool crafterri;
    public bool crafterrc;
    public bool craftercc;
    public bool crafterca;
    public bool framerucost;
    public bool framerrcost;
    public bool medicrevivala;
    public bool medicrecovera;
    public bool medictools;
    public bool scavelc;
    public bool scavelm;
    public bool scavcic;
    public bool scavcim;
}

public class Ranks
{
    public string name;
    public string sig;
    public string image;
    public string group;
    public string description;
}

public class SpecialGroups
{
    public Dictionary<int, Specialgroups> specialgroups;
}

public class Specialgroups
{
    public string groupname;
    public string permissionname;
    public int grouppriority;
    public double xpboost;
    public int dailyxplimit;
    public int dailystatlimitboost;
    public int dailyskilllimitboost;
}

public class DailyTimer
{
    public int dailyresettimerhours;
    public DateTime lastdailyreset;
}

public class DailyXpLimit
{
    public bool enabledailyxplimit;
    public int dailyxplimit;
    public int dailyxplimitvip;
    public int limitmultipliertype;
    public int limitmultiplier;
    public double limitpercentage;
}

public class DailyResetLimit
{
    public bool enabledailyresetlimit;
    public int dailystatlimit;
    public int dailystatlimitvip;
    public int dailyskilllimit;
    public int dailyskilllimitvip;
}

public class NightBonus
{
    public bool Enable;
    public int StartTime;
    public int EndTime;
    public double Bonus;
    public bool enableskillboosts;
}

public class XpGain
{
    public double chickenxp;
    public double fishxp;
    public double boarxp;
    public double stagxp;
    public double wolfxp;
    public double bearxp;
    public double polarbearxp;
    public double sharkxp;
    public double horsexp;
    public double scientistxp;
    public double sc_full;
    public double sc_heavy;
    public double sc_cargo;
    public double sc_junkpile;
    public double sc_oilrig;
    public double sc_patrol;
    public double sc_peacekeeper;
    public double sc_roam;
    public double dwellerxp;
    public double tunneldwellerxp;
    public double underwaterdwellerxp;
    public double scarecrownpc;
    public double customnpc;
    public double zombienpc;
    public double playerxp;
    public double lootcontainerxp;
    public double lootbarrel;
    public double oilbarrel;
    public double vehicleparts;
    public double toolcrate;
    public double normalcrate;
    public double elitecrate;
    public double foodcrate;
    public double medicalcrate;
    public double animalharvestxp;
    public double corpseharvestxp;
    public double underwaterlootcontainerxp;
    public double lockedcratexp;
    public double hackablecratexp;
    public double craftingxp;
    public bool craftingxpdelay;
    public double craftingxpdelayseconds;
    public double bradley;
    public double patrolhelicopter;
    public double turretxp;
    public bool allowturretxp;
    public double playerrevive;
    public bool enablexpboost;
    public double xpboostamount;
    public int xpboostorder;
    public double gifts;
    public double opengifts;
    public double opengiftsmed;
    public double opengiftslarge;
    public double upgradegiftsmed;
    public double upgradegiftslarge;
    public double craftmeal;
}

public class XpGather
{
    public double treexp;
    public double orexp;
    public double metalorexp;
    public double stoneorexp;
    public double sulfurorexp;
    public double harvestxp;
    public double plantxp;
    public bool noxptools;
    public bool onetimexp;
    public double toolxpchance;
    public double toolxppercent;
}

public class XpBuilding
{
    public double twigstructure;
    public double woodstructure;
    public double stonestructure;
    public double metalstructure;
    public double armoredstructure;
    public bool preventBGxp;
    public bool buildxpdelay;
    public bool requirebuildingprivlidge;
    public int buildxpdelayseconds;
    public bool reducexp;
    public double buildxpreduction;
}

public class XpTeams
{
    public bool enableteamxpgain;
    public bool enableteamxploss;
    public double teamxpgainamount;
    public double teamxplossamount;
    public float teamdistance;
}

public class XpMissions
{
    public double missionsucceededxp;
    public bool missionfailed;
    public double missionfailedxp;
}

public class XpReducer
{
    public bool suicidereduce;
    public double suicidereduceamount;
    public bool deathreduce;
    public double deathreduceamount;
    public bool rankdeathreduce;
}

public class XpBonus
{
    public bool showkrbutton;
    public bool enablebonus;
    public int requiredkills;
    public double bonusxp;
    public int endbonus;
    public bool multibonus;
    public string multibonustype;
}

public class XpEcon
{
    public bool showbalanceprofile;
    public bool econlevelup;
    public bool econleveldown;
    public bool econresetstats;
    public bool econresetskills;
    public bool econresetstat;
    public bool econresetskill;
    public double econlevelreward;
    public double econlevelreduction;
    public double econresetstatscost;
    public double econresetskillscost;
    public double econresetstatcost;
    public double econresetskillcost;
    public bool econstatlevelcost;
    public bool econskilllevelcost;
    public double econstatlevelcostmultiplier;
    public double econskilllevelcostmultiplier;
    public double econmentality;
    public double econdexterity;
    public double econmight;
    public double econcaptaincy;
    public double econweaponry;
    public double econninjary;
    public double econwoodcutter;
    public double econsmithy;
    public double econminer;
    public double econforager;
    public double econhunter;
    public double econfisher;
    public double econcrafter;
    public double econframer;
    public double econmedic;
    public double econscavenger;
    public double econelectrician;
    public double econdemolitionist;
    public double econtamer;
}

public class SRewards
{
    public bool srewardlevelup;
    public bool srewardleveldown;
    public bool srewardresetstats;
    public bool srewardresetskills;
    public bool srewardresetstat;
    public bool srewardresetskill;
    public int srewardlevelupamt;
    public int srewardleveldownamt;
    public int srewardresetstatscost;
    public int srewardresetskillscost;
    public int srewardresetstatcost;
    public int srewardresetskillcost;
    public bool srewardstatlevelcost;
    public bool srewardskilllevelcost;
    public int srewardstatlevelcostmultiplier;
    public int srewardskilllevelcostmultiplier;
    public int srewardmentality;
    public int srewarddexterity;
    public int srewardmight;
    public int srewardcaptaincy;
    public int srewardweaponry;
    public int srewardninjary;
    public int srewardwoodcutter;
    public int srewardsmithy;
    public int srewardminer;
    public int srewardforager;
    public int srewardhunter;
    public int srewardfisher;
    public int srewardcrafter;
    public int srewardframer;
    public int srewardmedic;
    public int srewardscavenger;
    public int srewardelectrician;
    public int srewardemolitionist;
    public int srewardtamer;
}

public class Mentality
{
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public double researchcost;
    public double researchcosttechtree;
    public double researchspeed;
    public double criticalchance;
    public double criticaldgm;
    public double damageincrease;
    public bool useotherresearchmod;
    public bool locktechtree;
    public int unlocktechtreelevel;
}

public class Dexterity
{
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public double blockchance;
    public double blockamount;
    public double dodgechance;
    public double reducearmordmg;
    public double horsespeed;
    public double boatspeed;
    public double vehiclespeed;
    public double fuelreduce;
}

public class Might
{
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public double armor;
    public double meleedmg;
    public double metabolism;
    public double bleedreduction;
    public double radreduction;
    public double heattolerance;
    public double coldtolerance;
}

public class Captaincy
{
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public bool allownoteam;
    public double skillboost;
    public bool enablexpboost;
    public double xpboost;
    public float captaincydistance;
}

public class Weaponry
{
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public double reductionchance;
    public double tool;
    public double powertools;
    public double meleeweapons;
    public double projectileweapons;
    public double mindamage;
    public double maxammo;
    public double maxammolimit;
    public bool skinboxdisable;
    public bool neverweartools;
    public bool neverwearweapons;
    public string reloadhook;
    public string excludedweapons;
    public bool useweaponmechanics;
}

public class Ninjary
{
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public double patrolstealth;
    public double ch47stealth;
    public double bradleystealth;
    public double npcstealth;
    public double turretstealth;
    public double knifeincrease;
    public double swordincrease;
}

public class Woodcutter
{
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public double gatherrate;
    public double bonusincrease;
    public double applechance;
}

public class Smithy
{
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public double productionrate;
    public double productionspeed;
    public double fuelconsumption;
    public double metalchance;
    public int metalamount;
}

public class Miner
{
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public double gatherrate;
    public double bonusincrease;
    public double fuelconsumption;
    public double metalchance;
    public int metalamount;
}

public class Forager
{
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public double gatherrate;
    public double chanceincrease;
    public double grubwormincrease;
    public double randomchance;
    public Dictionary<int, RandomChanceList> randomChanceList;
}

public class RandomChanceList
{
    public string shortname;
    public string displayname;
    public ulong SkinID;
    public int amount;
}

public class Hunter
{
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public double gatherrate;
    public double bonusincrease;
    public double damageincrease;
    public double nightdmgincrease;
    public double bowdmgincrease;
    public bool excludelongrangeweapons;
    public bool excludemedrangeweapons;
}

public class Fisher
{
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public double fishamountincrease;
    public double itemamountincrease;
    public double oxygenreduction;
    public double oxygentankreduction;
}

public class Crafter
{
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public double craftspeed;
    public double craftcost;
    public double repairincrease;
    public double repaircost;
    public double conditionchance;
    public double conditionamount;
}

public class Framer
{
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public double upgradecost;
    public double repaircost;
    public double repairtime;
    public int woodcost;
    public int stonecost;
    public int metalcost;
    public int armorcost;
}

public class Electrician
{
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public int solarpaneldefault;
    public int smallbatterydefault;
    public int mediumbatterydefault;
    public int largebatterydefault;
    public int smallgeneratordefault;
    public int testgeneratordefault;
    public int electricwindmilldefault;
    public double solarpanelinputincrease;
    public double solarpanelmaxincrease;
    public double smallbatterymaxincrease;
    public double mediumbatterymaxincrease;
    public double largebatterymaxincrease;
    public double smallgeneratormaxincrease;
    public double testgeneratormaxincrease;
    public double electricwindmillincrease;
    public double electricwindmillmaxincrease;
    public bool allowminsolarinput;
    public int minsolarinput;
}

public class Demolitionist
{
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public double explosivedudreduction;
    public double explosivedamage;
    public double explosiveradius;
}

public class Medic
{
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public double revivehp;
    public double recoverhp;
    public double crafttime;
    public double tools;
    public double teas;
    public bool preventbandageboost;
}

public class Scavenger
{
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public double scavlootchance;
    public double scavchance;
    public double scavmultiplier;
    public double customscavmultiplier;
    public bool customscavrandom;
    public bool usecustomscavlist;
    public bool drops;
    public bool crates;
    public bool uncrates;
    public bool lockedcrates;
    public bool hackcrates;
    public bool scientists;
    public bool componentsonly;
    public Dictionary<int, ScavChanceList> scavChanceList;
}

public class ScavChanceList
{
    public string shortname;
    public string displayname;
    public ulong SkinID;
    public int amount;
    public int maxamount;
    public int requiredlevel;
}

public class Tamer
{
    public bool enabletame;
    public int maxlvl;
    public int pointcoststart;
    public int costmultiplier;
    public bool tamechicken;
    public bool tameboar;
    public bool tamestag;
    public bool tamewolf;
    public bool tamebear;
    public bool tamepolarbear;
    public int chickenlevel;
    public int boarlevel;
    public int staglevel;
    public int wolflevel;
    public int bearlevel;
    public int polarbearlevel;
}

public class SQL
{
    public bool enablesql;
    public string SQLhost;
    public int SQLport;
    public string SQLdatabase;
    public string SQLusername;
    public string SQLpassword;
}

public class BackpacksMod
{
    public bool enablebackpacks;
    public string statorskill;
    public bool removeonunload;
    public SortedDictionary<int, BackPackSlots> BackPackSlots;
}

public class BackPackSlots
{
    public int level;
    public int slots;
}

public class ZoneManagerMod
{
    public string noxpgain;
    public string noxploss;
    public string disablestatsandskills;
}

public class EventHelperMod
{
    public string noxpgain;
    public string noxploss;
    public string disablestatsandskills;
}

public class SurvivalArenaMod
{
    public bool noxpgain;
    public bool noxploss;
    public bool disablestatsandskills;
}

public class RaidableBasesMod
{
    public bool disableabilities;
    public bool noxploss;
    public bool noxpgain;
}

public class SerializableConfiguration
{
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private static class JsonHelper
{
    public static object Deserialize(string json);
    private static object ToObject(JToken token);
}

public class XPData
{
    public Dictionary<string, XPRecord> XPerience;
}

public class XPRecord
{
    public int rank;
    public int truelevel;
    public int trueexperience;
    public double level;
    public double experience;
    public double requiredxp;
    public int statpoint;
    public int skillpoint;
    public int Mentality;
    public int MentalityP;
    public int Dexterity;
    public int DexterityP;
    public int Might;
    public int MightP;
    public int Captaincy;
    public int CaptaincyP;
    public int Weaponry;
    public int WeaponryP;
    public int Ninjary;
    public int NinjaryP;
    public int WoodCutter;
    public int WoodCutterP;
    public int Smithy;
    public int SmithyP;
    public int Miner;
    public int MinerP;
    public int Forager;
    public int ForagerP;
    public int Hunter;
    public int HunterP;
    public int Fisher;
    public int FisherP;
    public int Crafter;
    public int CrafterP;
    public int Framer;
    public int FramerP;
    public int Electrician;
    public int ElectricianP;
    public int Medic;
    public int MedicP;
    public int Scavenger;
    public int ScavengerP;
    public int Demolitionist;
    public int DemolitionistP;
    public int Tamer;
    public int TamerP;
    public int Wood;
    public int Stone;
    public int Metal;
    public int Sulfur;
    public int Cactus;
    public int Berries;
    public int Pumpkin;
    public int Potato;
    public int Corn;
    public int Mushroom;
    public int Hemp;
    public int Seed;
    public bool Status;
    public bool DisableRank;
    public int UILocation;
    public string teatype;
    public double teacooldown;
    public DateTime resettimerstats;
    public DateTime resettimerskills;
    public DateTime playerfixdata;
    public int dash;
    public int dmgbar;
    public int profilebg;
    public bool fuelgauge;
    public bool speedometer;
    public int speedometertype;
    public bool enableconfirmationprompt;
    public bool showchatnotifications;
    public bool showchatprofileonconnect;
    public bool showwelcomepanel;
    public bool showchatxp;
    public bool exclude;
    public bool raidablebase;
    public string displayname;
    public string id;
}

public class DailyData
{
    public Dictionary<string, DailyRecord> DailyXPerience;
}

public class DailyRecord
{
    public double dailyexperience;
    public int dailystatresets;
    public int dailyskillresets;
    public DateTime lastexperiencereset;
    public DateTime laststatreset;
    public DateTime lastskillreset;
}

private class LootData
{
    public Dictionary<ulong, Loot> LootRecords;
}

private class Loot
{
    public List<string> id;
}

private class CorpseData
{
    public Dictionary<ulong, Corpse> CorpseRecords;
}

private class Corpse
{
    public ulong corpsecontainer;
    public List<string> id;
}

private class HorseData
{
    public Dictionary<ulong, Horse> HorseRecords;
}

private class Horse
{
    public ulong horse;
    public float maxSpeed;
    public float runSpeed;
    public float walkSpeed;
    public float trotSpeed;
    public ulong player;
}

private class WeaponData
{
    public Dictionary<ulong, Weapon> WeaponRecords;
}

private class Weapon
{
    public int defaultammo;
    public int maxammo;
    public double defaultreload;
    public double newreload;
    public double defaultdistance;
    public double maxdistance;
    public double defaultrange;
    public double maxrange;
    public ulong player;
    public NetworkableId weapondata;
}

private class BoatData
{
    public Dictionary<ulong, Boat> BoatRecords;
}

private class Boat
{
    public ulong boat;
    public float defaultSpeed;
    public ulong player;
}

private class VehicleData
{
    public Dictionary<ulong, Vehicle> VehicleRecords;
}

private class Vehicle
{
    public ulong vehicle;
    public float maxDriveSlip;
    public float reversePercentSpeed;
    public float driveForceToMaxSlip;
    public ulong player;
}

private class MinicopterData
{
    public Dictionary<ulong, MiniCopterP> MinicopterRecords;
}

private class MiniCopterP
{
    public ulong minicopter;
    public float maxRotorSpeed;
    public ulong player;
}

private class SnowmobData
{
    public Dictionary<ulong, Snowmob> SnowmobRecords;
}

private class Snowmob
{
    public ulong snowmob;
    public float terrain;
    public double engineKW;
    public ulong player;
}

private class SmithyData
{
    public Dictionary<string, SmithyD> SmithyRecords;
}

private class SmithyD
{
    public string resource;
    public float time;
}

private class ElectricianData
{
    public Dictionary<ulong, ElectricianD> ElectricianRecords;
}

private class ElectricianD
{
    public ulong id;
    public string type;
    public int defaultmaxoutput;
    public int newmaxoutput;
    public ulong owner;
}

private class HeliHits
{
    public Dictionary<ulong, Heli> HeliRecords;
}

private class Heli
{
    public ulong heli;
    public ulong player;
}

public class Deaths
{
    public string victim;
    public string victimname;
    public string attacker;
    public string attackername;
    public string weapon;
    public string lastdamage;
    public float distance;
    public long timestamp;
}

private class Raids
{
    public Vector3 Location;
    public string BaseName;
    public int mode;
    public bool allowPVP;
    public string id;
    public float spawnTime;
    public float despawnTime;
    public float loadTime;
    public ulong ownerid;
    public List<ulong> raiders;
    public DateTime spawnDateTime;
    public DateTime despawnDateTime;
}

public class DmgBarImgs
{
    public string name;
    public string url;
}

public static class PlayerEx
{
    public static void RunEffect(BasePlayer player, string prefab);
}

XPerienceEx
public static class PlayerEx
{
    public static void RunEffect(BasePlayer player, string prefab);
}


```

---

## XpRevived by  - Brings back the XP system

```csharp
using System;
using System.Linq;
using System.Globalization;
using System.Collections.Generic;
using System.Text;
using Newtonsoft.Json;
using UnityEngine;
using Oxide.Core;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("Xp Revived", "Mattparks", "0.2.8")]
[Description("Brings back the XP system")]
 class XpRevived : RustPlugin
{
    [PluginReference]
     RustPlugin ImageLibrary;
    static XpRevived _plugin;
    public class Images
    {
        public static string TryForImage(string shortname, ulong skin, bool localimage);
        public static string GetImageURL(string shortname, ulong skin);
        public static uint GetTextureID(string shortname, ulong skin);
        public static string GetImage(string shortname, ulong skin);
        public static bool AddImage(string url, string shortname, ulong skin);
        public static bool HasImage(string shortname, ulong skin);
        public static void TryAddImage(string url, string shortname, ulong skin);
        public static List<ulong> GetImageList(string shortname);
    }

    public class UI
    {
        public static CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool useCursor, string parent);
        public static void LoadImage(CuiElementContainer container, string panel, string url, string aMin, string aMax);
        public static void CreateInput(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, bool password, int charLimit, TextAnchor align);
        public static void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        public static void CreateText(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        public static void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
        public static string Color(string hexColor, float alpha);
    }

    public class UIManager
    {
        public List<string> activeUis;
        public void AddContainer(string container);
        public void RemoveContainer(string container);
        public void DestroyUI(BasePlayer player, bool destroyNav);
    }

    private List<string> BlockedBps;
    public class ItemData
    {
        public string ShortName;
        public string EnglishName;
        public int UnlockLevel;
        public int CostXP;
        public bool UnlockOnLevel;
    }

    public class Options
    {
        public bool UnlimitedComponents;
        public List<string> BlockedItems;
        public List<ItemData> ItemDatas;
        public string LevelUpPrefab;
        public string LearnPrefab;
        public float LevelPivot;
        public float LevelXpRatio;
        public float XpFromGivenTool;
        public bool LootRemoveScrap;
        public bool RemoveIngredients;
        public bool UnlockAllOnLevel;
    }

    public class Display
    {
        public string LevelIcon;
        public string XpIcon;
        public float OffsetX;
        public float OffsetY;
        public string TextColourLearned;
        public string TextColourNotLearned;
        public string TextColourAutomatic;
        public string HudColourLevel;
        public string HudColourXp;
        public bool HudEnabled;
    }

    public class LevelRates
    {
        public float RepeatDelay;
        public float Repeat;
        public float PlayerKilled;
        public float PlayerRecovered;
        public float PlayerHelped;
        public float ItemCrafted;
        public float Recycling;
        public float Looting;
        public float KilledHeli;
        public float KilledAnimal;
        public float BrokeBarrel;
        public float ItemPickup;
        public float HitTree;
        public float HitOre;
        public float HitFlesh;
        public float SupplyThrown;
        public float StructureUpgraded;
    }

    public class NoobKill
    {
        public int MaxNoobLevel;
        public float XpPunishment;
    }

    public class ConfigData
    {
        public Options Options;
        public Display Display;
        public LevelRates LevelRates;
        public NoobKill NoobKill;
    }

    public class PlayerData
    {
        public float Level;
        public float Xp;
        public bool ResetBps;
    }

    public class StoredData
    {
        public Dictionary<ulong, PlayerData> PlayerData;
    }

    private UIManager _uiManager;
    private ConfigData _config;
    private StoredData _storedData;
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void LoadStoredData();
    private void SaveStoredData();
    private void Init();
    private void OnServerInitialized();
    private void OnPlayerInit(BasePlayer player);
    private void OnPlayerSpawn(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private object OnPlayerDie(BasePlayer player, HitInfo info);
    private void OnPlayerRecover(BasePlayer player);
    private void OnItemCraftCancelled(ItemCraftTask task);
    private void OnEntitySpawned(LootContainer container);
    private object OnRecycleItem(Recycler recycler, Item item);
    private object OnItemResearch(Item item, BasePlayer player);
    private void OnItemCraftFinished(ItemCraftTask task, Item item);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private void OnCollectiblePickup(Item item, BasePlayer player);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void OnStructureUpgrade(BaseCombatEntity entity, BasePlayer player);
    private void Unload();
    [ChatCommand("xp")]
    private void CommandXp(BasePlayer player, string command, string[] args);
    [ChatCommand("level")]
    private void CommandLevel(BasePlayer player, string command, string[] args);
    [ChatCommand("unlock")]
    private void CommandUnlock(BasePlayer player, string command, string[] args);
    [ChatCommand("learn")]
    private void CommandLearn(BasePlayer player, string command, string[] args);
    private PlayerData GetPlayerData(ulong playerID);
    public float GetPlayerLevel(ulong playerID);
    public float GetPlayerXp(ulong playerID);
    public void IncreaseXp(ulong playerID, float amount, bool updateAFK);
    public void IncreaseXp(BasePlayer player, float amount, bool updateAFK);
    public void UnlockLevel(BasePlayer player, int level);
    private bool IsUnlockable(string shortName, int level);
    private void ResetConfigXpLevels();
    private void InfiniteComponents(BasePlayer player, bool removeComponents, bool giveComponents);
    private void AssignLoot(LootContainer container);
    private string colourText;
    private string colourClear;
    private string colourBackground;
    private const string uiPrefix;
    private const string uiPanel;
    private const string uiLevel;
    private const string uiXp;
    private void UpdatePlayerUi(BasePlayer player, bool refreshAll);
    private string Lang(string key, BasePlayer player);
    private void MessagePlayer(string message, BasePlayer player);
    private BasePlayer GetPlayerFromId(ulong id, bool canBeOffline);
    private BasePlayer FindPlayer(string partialName, bool canBeOffline);
    public ItemDefinition GetItemDefinition(string shortName);
}

public class Images
{
    public static string TryForImage(string shortname, ulong skin, bool localimage);
    public static string GetImageURL(string shortname, ulong skin);
    public static uint GetTextureID(string shortname, ulong skin);
    public static string GetImage(string shortname, ulong skin);
    public static bool AddImage(string url, string shortname, ulong skin);
    public static bool HasImage(string shortname, ulong skin);
    public static void TryAddImage(string url, string shortname, ulong skin);
    public static List<ulong> GetImageList(string shortname);
}

public class UI
{
    public static CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool useCursor, string parent);
    public static void LoadImage(CuiElementContainer container, string panel, string url, string aMin, string aMax);
    public static void CreateInput(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, bool password, int charLimit, TextAnchor align);
    public static void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    public static void CreateText(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    public static void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    public static string Color(string hexColor, float alpha);
}

public class UIManager
{
    public List<string> activeUis;
    public void AddContainer(string container);
    public void RemoveContainer(string container);
    public void DestroyUI(BasePlayer player, bool destroyNav);
}

public class ItemData
{
    public string ShortName;
    public string EnglishName;
    public int UnlockLevel;
    public int CostXP;
    public bool UnlockOnLevel;
}

public class Options
{
    public bool UnlimitedComponents;
    public List<string> BlockedItems;
    public List<ItemData> ItemDatas;
    public string LevelUpPrefab;
    public string LearnPrefab;
    public float LevelPivot;
    public float LevelXpRatio;
    public float XpFromGivenTool;
    public bool LootRemoveScrap;
    public bool RemoveIngredients;
    public bool UnlockAllOnLevel;
}

public class Display
{
    public string LevelIcon;
    public string XpIcon;
    public float OffsetX;
    public float OffsetY;
    public string TextColourLearned;
    public string TextColourNotLearned;
    public string TextColourAutomatic;
    public string HudColourLevel;
    public string HudColourXp;
    public bool HudEnabled;
}

public class LevelRates
{
    public float RepeatDelay;
    public float Repeat;
    public float PlayerKilled;
    public float PlayerRecovered;
    public float PlayerHelped;
    public float ItemCrafted;
    public float Recycling;
    public float Looting;
    public float KilledHeli;
    public float KilledAnimal;
    public float BrokeBarrel;
    public float ItemPickup;
    public float HitTree;
    public float HitOre;
    public float HitFlesh;
    public float SupplyThrown;
    public float StructureUpgraded;
}

public class NoobKill
{
    public int MaxNoobLevel;
    public float XpPunishment;
}

public class ConfigData
{
    public Options Options;
    public Display Display;
    public LevelRates LevelRates;
    public NoobKill NoobKill;
}

public class PlayerData
{
    public float Level;
    public float Xp;
    public bool ResetBps;
}

public class StoredData
{
    public Dictionary<ulong, PlayerData> PlayerData;
}


```

---

## YouveGotMail by KajWithAJ - A small plugin that notifies an online player when receiving mail in his or her mailbox.

```csharp
using System.Linq;
using System.Collections.Generic;

Oxide.Plugins
[Info("You've Got Mail", "KajWithAJ", "1.0.2")]
[Description("Notifies online players when they receive mail in their mailbox.")]
 class YouveGotMail : RustPlugin
{
    private const string MailboxPermission;
    private void Init();
    protected override void LoadDefaultMessages();
     void OnItemSubmit(Item item, Mailbox mailbox, BasePlayer player);
}


```

---

## ZLevelsRemastered by nivex - Allows player to level up 5 different skills (Mining, Woodcutting, Crafting, Skinning, Aquire)

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Newtonsoft.Json;
using Rust;

Oxide.Plugins
[Info("ZLevels Remastered", "nivex", "3.2.2")]
[Description("Lets players level up as they harvest different resources and when crafting")]
 class ZLevelsRemastered : RustPlugin
{
    [PluginReference]
     Plugin EventManager;
     Plugin CraftingController;
     Plugin ZoneManager;
     Plugin Economics;
     Plugin IQEconomic;
     Plugin ServerRewards;
     Plugin SkillTree;
    private StoredData data;
    private Dictionary<string, ItemDefinition> CraftItems;
    private CraftData _craftData;
    private StringBuilder _sb;
    private bool newSaveDetected;
    private bool bonusOn;
    private int MaxB;
    private int MinB;
    private Skills[] AllSkills;
    private class CraftData
    {
        public Dictionary<string, CraftInfo> CraftList;
        public CraftData();
    }

    private class CraftInfo
    {
        public int MaxBulkCraft;
        public int MinBulkCraft;
        public string shortName;
        public bool Enabled;
        public CraftInfo();
    }

    private class StoredData
    {
        public Hash<ulong, PlayerInfo> PlayerInfo;
        public StoredData();
    }

    private class PlayerInfo : IEquatable<PlayerInfo>
    {
        private double _acquireLevel;
        private double _acquirePoints;
        private double _craftingLevel;
        private double _craftingPoints;
        private double _miningLevel;
        private double _miningPoints;
        private double _skinningLevel;
        private double _skinningPoints;
        private double _woodcuttingLevel;
        private double _woodcuttingPoints;
        private double _lastDeath;
        private double _xpMultiplier;
        private double RoundValue(double value);
        [JsonProperty(PropertyName = "AL")]
        public double ACQUIRE_LEVEL { get; set; }
        [JsonProperty(PropertyName = "AP")]
        public double ACQUIRE_POINTS { get; set; }
        [JsonProperty(PropertyName = "CL")]
        public double CRAFTING_LEVEL { get; set; }
        [JsonProperty(PropertyName = "CP")]
        public double CRAFTING_POINTS { get; set; }
        [JsonProperty(PropertyName = "ML")]
        public double MINING_LEVEL { get; set; }
        [JsonProperty(PropertyName = "MP")]
        public double MINING_POINTS { get; set; }
        [JsonProperty(PropertyName = "SL")]
        public double SKINNING_LEVEL { get; set; }
        [JsonProperty(PropertyName = "SP")]
        public double SKINNING_POINTS { get; set; }
        [JsonProperty(PropertyName = "WCL")]
        public double WOODCUTTING_LEVEL { get; set; }
        [JsonProperty(PropertyName = "WCP")]
        public double WOODCUTTING_POINTS { get; set; }
        [JsonProperty(PropertyName = "LD")]
        public double LAST_DEATH { get; set; }
        [JsonProperty(PropertyName = "XPM")]
        public double XP_MULTIPLIER { get; set; }
        [JsonProperty(PropertyName = "CUI")]
        public bool CUI { get; set; }
        [JsonProperty(PropertyName = "ONOFF")]
        public bool ENABLED { get; set; }
        public PlayerInfo();
        public bool Equals(PlayerInfo other);
        public override bool Equals(object obj);
        public override int GetHashCode();
        internal bool IsDefault(PlayerInfo other);
    }

    private void Init();
    private void Unload();
    private void OnNewSave(string strFilename);
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnEntityKill(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnPlayerSleepEnded(BasePlayer player);
    private void OnPlayerSleep(BasePlayer player);
    private void OnEntityDeath(BasePlayer player, HitInfo hitInfo);
    private void OnDispenserGather(ResourceDispenser dispenser, BasePlayer player, Item item);
    private bool CanUseJackhammer(BasePlayer player);
    private bool CanUseChainsaw(BasePlayer player);
    private void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private List<string> _warnings;
    private object OnCollectiblePickup(CollectibleEntity collectible, BasePlayer player);
    private void OnTimeSunset();
    private void OnTimeSunrise();
    private object OnGrowableGathered(GrowableEntity plant, Item item, BasePlayer player);
    private object OnItemCraft(ItemCraftTask task, BasePlayer crafter, Item fromTempBlueprint);
    private List<int> GetStacks(int amount, int maxStack);
    private bool HasPlace(BasePlayer crafter, List<int> stacks);
    private bool CanInstantCraftNoLevelRequirement(BasePlayer crafter);
    private bool CanInstantCraft(BasePlayer crafter);
    private void ReturnCraft(ItemCraftTask task, BasePlayer crafter);
    private object OnItemCraftFinished(ItemCraftTask task, Item item, ItemCrafter craft);
    [HookMethod("SendHelpText"), ChatCommand("stathelp")]
    private void SendHelpText(BasePlayer player);
    private void PointsPerHitCommand(IPlayer user, string command, string[] args);
    private void PlayerXpmCommand(IPlayer user, string command, string[] args);
    private void InfoCommand(IPlayer user, string command, string[] args);
    private bool GetLookingAtPlayer(IPlayer user);
    private void PrintInfo(IPlayer user, string targetId, string targetName, PlayerInfo playerData);
    private void ResetCommand(IPlayer user, string command, string[] args);
    private void ZlvlCommand(IPlayer user, string command, string[] args);
    private void CheckCommand(IPlayer user, string command, string[] args);
    private void CleanCommand(IPlayer user, string command, string[] args);
    private int Clean();
    private void PenaltyCommand(IPlayer user, string command, string[] args);
    [ChatCommand("stats")]
    private void StatsCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("statinfo")]
    private void StatInfoCommand(BasePlayer player, string command, string[] args);
    private void AppendLine(string color, double xp, Skills skill);
    [ChatCommand("statsui")]
    private void StatsUICommand(BasePlayer player, string command, string[] args);
    [ChatCommand("statsonoff")]
    private void StatsOnOffCommand(BasePlayer player, string command, string[] args);
    private void Unsubscribe();
    private void RegisterPermissions();
    private void RegisterCommands();
    private void Subscribe();
    private void CheckCollectible();
    private void SaveData();
    private void SaveDetails();
    private void LoadData();
    private void WipeData();
    private bool hasRights(string UserIDString);
    private void editMultiplierForPlayer(double multiplier, ulong userID);
    private void adminModifyPlayerStats(IPlayer user, Skills skill, double level, int mode, IPlayer target);
    private string getStatPrint(BasePlayer player, Skills skill);
    private void removePoints(PlayerInfo pi, Skills skill, double points);
    private double GetPenalty(BasePlayer player, PlayerInfo pi, Skills skill, bool isSuicide);
    private double getPenaltyPercent(BasePlayer player, PlayerInfo pi, Skills skill, bool isSuicide);
    private int levelHandler(PlayerInfo pi, BasePlayer player, int prevAmount, Skills skill, BaseEntity source, bool isPowerTool);
    private void BroadcastLevel(BasePlayer player, Skills skill, string color, double Level);
    private double getExperiencePercentProc(PlayerInfo pi, Skills skill);
    private double getExperiencePercent(PlayerInfo pi, Skills skill);
    private void setPointsAndLevel(PlayerInfo pi, Skills skill, double points, double level);
    private double getLevel(PlayerInfo pi, Skills skill);
    private double getPoints(PlayerInfo pi, Skills skill);
    private double getLevelPoints(double level);
    private double getPointsLevel(double points, Skills skill);
    private double getGathMult(double skillLevel, Skills skill);
    private double getXpMulti(PlayerInfo pi, string userid);
    private bool IsCraftingEnabled();
    private bool IsSkillEnabled(Skills skill);
    private double getPointsNeededForNextLevel(double level);
    private double getPercentAmount(double level, double percent);
    private bool IsValid(BasePlayer player);
    private bool isZoneExcluded(BasePlayer player);
    private string ReadableTimeSpan(TimeSpan span);
    private long ToEpochTime(DateTime dateTime);
    private DateTime ToDateTimeFromEpoch(double intDate);
    private string _(string key, string id);
    private void Message(BasePlayer player, string key, object[] args);
    private ItemDefinition GetItem(string shortname);
    private void GenerateItems(bool reset);
    private void GUIUpdateSkill(BasePlayer player, Skills skill);
    private void DestroyGUI(BasePlayer player);
    private void CreateGUI(BasePlayer player, PlayerInfo pi);
    private PlayerInfo GetPlayerInfo(BasePlayer player);
    private PlayerInfo CreatePlayerInfo();
    private PlayerInfo GetPlayerInfo(ulong userID);
    private class Configuration
    {
        [JsonProperty(PropertyName = "CUI")]
        public ConfigurationCui cui { get; set; }
        [JsonProperty(PropertyName = "Functions")]
        public ConfigurationFunctions functions { get; set; }
        [JsonProperty(PropertyName = "Generic")]
        public ConfigurationGeneric generic { get; set; }
        [JsonProperty(PropertyName = "Night Bonus")]
        public ConfigurationNightBonus nightbonus { get; set; }
        [JsonProperty(PropertyName = "Settings")]
        public ConfigurationSettings settings { get; set; }
        [JsonProperty(PropertyName = "Level Up Rewards (Reward * Level = Amount)")]
        public Rewards Rewards { get; set; }
    }

    private class ConfigurationCui
    {
        [JsonProperty(PropertyName = "Bounds")]
        public ConfigurationCuiPositions cuiPositioning { get; set; }
        [JsonProperty(PropertyName = "Xp Bar Colors")]
        public ConfigurationColors cuiColors { get; set; }
        [JsonProperty(PropertyName = "Bounds Background")]
        public string cuiBoundsBackground { get; set; }
        [JsonProperty(PropertyName = "CUI Enabled")]
        public bool cuiEnabled { get; set; }
        [JsonProperty(PropertyName = "Font Color")]
        public string cuiFontColor { get; set; }
        [JsonProperty(PropertyName = "FontSize Bar")]
        public int cuiFontSizeBar { get; set; }
        [JsonProperty(PropertyName = "FontSize Level")]
        public int cuiFontSizeLvl { get; set; }
        [JsonProperty(PropertyName = "FontSize Percent")]
        public int cuiFontSizePercent { get; set; }
        [JsonProperty(PropertyName = "Text Shadow Enabled")]
        public bool cuiTextShadow { get; set; }
        [JsonProperty(PropertyName = "Xp Bar Background")]
        public string cuiXpBarBackground { get; set; }
    }

    private class ConfigurationFunctions
    {
        [JsonProperty(PropertyName = "Collectible Entities", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, bool> enabledCollectibleEntity { get; set; }
        [JsonProperty(PropertyName = "Enable Collectible Pickup")]
        public bool enableCollectiblePickup { get; set; }
        [JsonProperty(PropertyName = "Enable Crop Gather")]
        public bool enableCropGather { get; set; }
        [JsonProperty(PropertyName = "Enable Wood Gather")]
        public bool wood { get; set; }
        [JsonProperty(PropertyName = "Grant Wood XP Only")]
        public bool woodXpOnly { get; set; }
        [JsonProperty(PropertyName = "Enable Stone Ore Gather")]
        public bool stone { get; set; }
        [JsonProperty(PropertyName = "Grant Stone XP Only")]
        public bool stoneXpOnly { get; set; }
        [JsonProperty(PropertyName = "Enable Sulfur Ore Gather")]
        public bool sulfur { get; set; }
        [JsonProperty(PropertyName = "Grant Sulfur XP Only")]
        public bool sulfurXpOnly { get; set; }
        [JsonProperty(PropertyName = "Enable Metal Gather")]
        public bool metal { get; set; }
        [JsonProperty(PropertyName = "Grant Metal XP Only")]
        public bool metalXpOnly { get; set; }
        [JsonProperty(PropertyName = "Enable HQM Gather")]
        public bool hqm { get; set; }
        [JsonProperty(PropertyName = "Grant HQM XP Only")]
        public bool hqmXpOnly { get; set; }
        [JsonProperty(PropertyName = "Allow Mining Multiplier On Gibs")]
        public bool gibs { get; set; }
        internal bool enableDispenserGather { get; set; }
        public bool ShouldAllowGather(string shortName, bool grantXPOnly);
    }

    private class ConfigurationGeneric
    {
        [JsonProperty(PropertyName = "Enable Level Up Broadcast")]
        public bool enableLevelupBroadcast { get; set; }
        [JsonProperty(PropertyName = "Enable Permission")]
        public bool enablePermission { get; set; }
        [JsonProperty(PropertyName = "Chainsaw On Gather Permission")]
        public string AllowChainsawGather { get; set; }
        [JsonProperty(PropertyName = "Jackhammer On Gather Permission")]
        public string AllowJackhammerGather { get; set; }
        [JsonProperty(PropertyName = "Weapons On Gather Permission")]
        public string BlockWeaponsGather { get; set; }
        [JsonProperty(PropertyName = "gameProtocol")]
        public int gameProtocol { get; set; }
        [JsonProperty(PropertyName = "Penalty Minutes")]
        public int penaltyMinutes { get; set; }
        [JsonProperty(PropertyName = "Penalty Minutes (Suicide)")]
        public int penaltySuicideMinutes { get; set; }
        [JsonProperty(PropertyName = "Penalty Resets All Levels To Default")]
        public bool penaltyReset { get; set; }
        [JsonProperty(PropertyName = "Penalty On Death")]
        public bool penaltyOnDeath { get; set; }
        [JsonProperty(PropertyName = "Penalty On Suicide")]
        public bool penaltyOnSuicide { get; set; }
        [JsonProperty(PropertyName = "Permission Name")]
        public string permissionName { get; set; }
        [JsonProperty(PropertyName = "Permission Name XP")]
        public string permissionNameXP { get; set; }
        [JsonProperty(PropertyName = "Additional Boost Multipliers")]
        public Dictionary<string, double> vip { get; set; }
        [JsonProperty(PropertyName = "Permission Name No Wipes")]
        public string nowipe { get; set; }
        [JsonProperty(PropertyName = "Player CUI Default Enabled")]
        public bool playerCuiDefaultEnabled { get; set; }
        [JsonProperty(PropertyName = "Player Plugin Default Enabled")]
        public bool playerPluginDefaultEnabled { get; set; }
        [JsonProperty(PropertyName = "Plugin Prefix")]
        public string pluginPrefix { get; set; }
        [JsonProperty(PropertyName = "SteamID Icon")]
        public ulong steamIDIcon { get; set; }
        [JsonProperty(PropertyName = "Wipe Data OnNewSave")]
        public bool wipeDataOnNewSave { get; set; }
    }

    private class ConfigurationNightBonus
    {
        [JsonProperty(PropertyName = "Points Per Hit At Night")]
        public ConfigurationResources pointsPerHitAtNight { get; set; }
        [JsonProperty(PropertyName = "Points Per PowerTool At Night")]
        public ConfigurationResources pointsPerPowerToolAtNight { get; set; }
        [JsonProperty(PropertyName = "Resource Per Level Multiplier At Night")]
        public ConfigurationResources resourceMultipliersAtNight { get; set; }
        [JsonProperty(PropertyName = "Enable Night Bonus")]
        public bool enableNightBonus { get; set; }
        [JsonProperty(PropertyName = "Broadcast Enabled Bonus")]
        public bool broadcastEnabledBonus { get; set; }
        [JsonProperty(PropertyName = "Log Enabled Bonus Console")]
        public bool logEnabledBonusConsole { get; set; }
    }

    private class ConfigurationSettings
    {
        [JsonProperty(PropertyName = "Crafting Details", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public ConfigurationCraftingDetails craftingDetails { get; set; }
        [JsonProperty(PropertyName = "Default Resource Multiplier")]
        public ConfigurationResources defaultMultipliers { get; set; }
        [JsonProperty(PropertyName = "Level Caps")]
        public ConfigurationResources levelCaps { get; set; }
        [JsonProperty(PropertyName = "Percent Lost On Death")]
        public ConfigurationResources percentLostOnDeath { get; set; }
        [JsonProperty(PropertyName = "Percent Lost On Suicide")]
        public ConfigurationResources percentLostOnSuicide { get; set; }
        [JsonProperty(PropertyName = "No Penalty Zones", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> zones { get; set; }
        [JsonProperty(PropertyName = "Points Per Hit")]
        public ConfigurationResources pointsPerHit { get; set; }
        [JsonProperty(PropertyName = "Points Per Power Tool")]
        public ConfigurationResources pointsPerPowerTool { get; set; }
        [JsonProperty(PropertyName = "Resource Per Level Multiplier")]
        public ConfigurationResources resourceMultipliers { get; set; }
        [JsonProperty(PropertyName = "Skill Colors")]
        public ConfigurationSkillColors colors { get; set; }
        [JsonProperty(PropertyName = "Starting Stats")]
        public ConfigurationStartingStats stats { get; set; }
        [JsonProperty(PropertyName = "Use Mining Skill When Picking Up Stones And Ore")]
        public bool MiningStonesOre { get; set; }
        [JsonProperty(PropertyName = "Use Acquire Skill When Picking Up Wood")]
        public bool AcquireWood { get; set; }
    }

    public class ConfigurationStartingStats
    {
        [JsonProperty(PropertyName = "Acquire Level")]
        public double acquire_level { get; set; }
        [JsonProperty(PropertyName = "Acquire Points")]
        public double acquire_points { get; set; }
        [JsonProperty(PropertyName = "Crafting Level")]
        public double crafting_level { get; set; }
        [JsonProperty(PropertyName = "Crafting Points")]
        public double crafting_points { get; set; }
        [JsonProperty(PropertyName = "Mining Level")]
        public double mining_level { get; set; }
        [JsonProperty(PropertyName = "Mining Points")]
        public double mining_points { get; set; }
        [JsonProperty(PropertyName = "Skinning Level")]
        public double skinning_level { get; set; }
        [JsonProperty(PropertyName = "Skinning Points")]
        public double skinning_points { get; set; }
        [JsonProperty(PropertyName = "Woodcutting Level")]
        public double woodcutting_level { get; set; }
        [JsonProperty(PropertyName = "Woodcutting Points")]
        public double woodcutting_points { get; set; }
        [JsonProperty(PropertyName = "XP Multiplier")]
        public double xpm { get; set; }
    }

    public class ConfigurationResources
    {
        [JsonProperty(PropertyName = nameof(Skills.ACQUIRE), NullValueHandling = NullValueHandling.Ignore)]
        public double? ACQUIRE { get; set; }
        [JsonProperty(PropertyName = nameof(Skills.CRAFTING), NullValueHandling = NullValueHandling.Ignore)]
        public double? CRAFTING { get; set; }
        [JsonProperty(PropertyName = nameof(Skills.MINING), NullValueHandling = NullValueHandling.Ignore)]
        public double? MINING { get; set; }
        [JsonProperty(PropertyName = nameof(Skills.SKINNING), NullValueHandling = NullValueHandling.Ignore)]
        public double? SKINNING { get; set; }
        [JsonProperty(PropertyName = nameof(Skills.WOODCUTTING), NullValueHandling = NullValueHandling.Ignore)]
        public double? WOODCUTTING { get; set; }
        public ConfigurationResources(double? ACQUIRE, double? CRAFTING, double? MINING, double? SKINNING, double? WOODCUTTING);
        public double Get(Skills skill);
        public void Set(Skills skill, double value);
    }

    public class ConfigurationColors
    {
        [JsonProperty(PropertyName = nameof(Skills.ACQUIRE))]
        public string ACQUIRE { get; set; }
        [JsonProperty(PropertyName = nameof(Skills.CRAFTING))]
        public string CRAFTING { get; set; }
        [JsonProperty(PropertyName = nameof(Skills.MINING))]
        public string MINING { get; set; }
        [JsonProperty(PropertyName = nameof(Skills.SKINNING))]
        public string SKINNING { get; set; }
        [JsonProperty(PropertyName = nameof(Skills.WOODCUTTING))]
        public string WOODCUTTING { get; set; }
        public ConfigurationColors(string ACQUIRE, string CRAFTING, string MINING, string SKINNING, string WOODCUTTING);
        public string Get(Skills skill);
    }

    public class ConfigurationCraftingDetails
    {
        [JsonProperty(PropertyName = "Time Spent")]
        public double time { get; set; }
        [JsonProperty(PropertyName = "XP Per Time Spent")]
        public double xp { get; set; }
        [JsonProperty(PropertyName = "Percent Faster Per Level")]
        public double percent { get; set; }
        [JsonProperty(PropertyName = "Require Permission For Instant Bulk Craft")]
        public bool usePermission { get; set; }
        [JsonProperty(PropertyName = "Instant Bulk Craft Permission Does Not Require Max Level")]
        public bool noLevelRequirement { get; set; }
        [JsonProperty(PropertyName = "Permission For Instant Bulk Crafting At Max Level")]
        public string Permission { get; set; }
        [JsonProperty(PropertyName = "Require Inventory Slots")]
        public bool slots;
        public ConfigurationCraftingDetails(double time, double xp, double percent);
    }

    public class ConfigurationCuiPositions
    {
        [JsonProperty(PropertyName = "Width Left")]
        public string widthLeft { get; set; }
        [JsonProperty(PropertyName = "Width Right")]
        public string widthRight { get; set; }
        [JsonProperty(PropertyName = "Height Lower")]
        public string heightLower { get; set; }
        [JsonProperty(PropertyName = "Height Upper")]
        public string heightUpper { get; set; }
        public ConfigurationCuiPositions(string widthLeft, string widthRight, string heightLower, string heightUpper);
    }

    public class ConfigurationSkillColors
    {
        [JsonProperty(PropertyName = nameof(Skills.ACQUIRE))]
        public string ACQUIRE { get; set; }
        [JsonProperty(PropertyName = nameof(Skills.CRAFTING))]
        public string CRAFTING { get; set; }
        [JsonProperty(PropertyName = nameof(Skills.MINING))]
        public string MINING { get; set; }
        [JsonProperty(PropertyName = nameof(Skills.SKINNING))]
        public string SKINNING { get; set; }
        [JsonProperty(PropertyName = nameof(Skills.WOODCUTTING))]
        public string WOODCUTTING { get; set; }
        public string Get(Skills skill);
    }

    public class RewardType
    {
        [JsonProperty(PropertyName = "Acquire")]
        public double Acquire { get; set; }
        [JsonProperty(PropertyName = "Crafting")]
        public double Crafting { get; set; }
        [JsonProperty(PropertyName = "Mining")]
        public double Mining { get; set; }
        [JsonProperty(PropertyName = "Skinning")]
        public double Skinning { get; set; }
        [JsonProperty(PropertyName = "Woodcutting")]
        public double Woodcutting { get; set; }
        public double Get(Skills skill);
    }

    public class Rewards
    {
        [JsonProperty(PropertyName = "Economics Money")]
        public RewardType Money { get; set; }
        [JsonProperty(PropertyName = "ServerRewards Points")]
        public RewardType Points { get; set; }
        [JsonProperty(PropertyName = "SkillTree XP")]
        public RewardType XP { get; set; }
    }

    public void GiveReward(BasePlayer player, Skills skill, double level);
    private Configuration config;
    private ConfigurationResources pointsPerPowerTool;
    private ConfigurationResources pointsPerHitCurrent;
    private ConfigurationResources pointsPerPowerToolAtNight;
    private ConfigurationResources pointsPerHitPowerToolCurrent;
    private ConfigurationResources resourceMultipliersCurrent;
    private Dictionary<Skills, int> skillIndex;
    protected override void LoadConfig();
    public void CheckStartingStats();
    private bool canSaveConfig;
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private double GetMultiplier(ulong userID, string skill);
    private double GetLevel(ulong userID, string skill);
    private string api_GetPlayerInfo(ulong playerid);
    private bool api_SetPlayerInfo(ulong userid, string data);
}

private class CraftData
{
    public Dictionary<string, CraftInfo> CraftList;
    public CraftData();
}

private class CraftInfo
{
    public int MaxBulkCraft;
    public int MinBulkCraft;
    public string shortName;
    public bool Enabled;
    public CraftInfo();
}

private class StoredData
{
    public Hash<ulong, PlayerInfo> PlayerInfo;
    public StoredData();
}

private class PlayerInfo : IEquatable<PlayerInfo>
{
    private double _acquireLevel;
    private double _acquirePoints;
    private double _craftingLevel;
    private double _craftingPoints;
    private double _miningLevel;
    private double _miningPoints;
    private double _skinningLevel;
    private double _skinningPoints;
    private double _woodcuttingLevel;
    private double _woodcuttingPoints;
    private double _lastDeath;
    private double _xpMultiplier;
    private double RoundValue(double value);
    [JsonProperty(PropertyName = "AL")]
    public double ACQUIRE_LEVEL { get; set; }
    [JsonProperty(PropertyName = "AP")]
    public double ACQUIRE_POINTS { get; set; }
    [JsonProperty(PropertyName = "CL")]
    public double CRAFTING_LEVEL { get; set; }
    [JsonProperty(PropertyName = "CP")]
    public double CRAFTING_POINTS { get; set; }
    [JsonProperty(PropertyName = "ML")]
    public double MINING_LEVEL { get; set; }
    [JsonProperty(PropertyName = "MP")]
    public double MINING_POINTS { get; set; }
    [JsonProperty(PropertyName = "SL")]
    public double SKINNING_LEVEL { get; set; }
    [JsonProperty(PropertyName = "SP")]
    public double SKINNING_POINTS { get; set; }
    [JsonProperty(PropertyName = "WCL")]
    public double WOODCUTTING_LEVEL { get; set; }
    [JsonProperty(PropertyName = "WCP")]
    public double WOODCUTTING_POINTS { get; set; }
    [JsonProperty(PropertyName = "LD")]
    public double LAST_DEATH { get; set; }
    [JsonProperty(PropertyName = "XPM")]
    public double XP_MULTIPLIER { get; set; }
    [JsonProperty(PropertyName = "CUI")]
    public bool CUI { get; set; }
    [JsonProperty(PropertyName = "ONOFF")]
    public bool ENABLED { get; set; }
    public PlayerInfo();
    public bool Equals(PlayerInfo other);
    public override bool Equals(object obj);
    public override int GetHashCode();
    internal bool IsDefault(PlayerInfo other);
}

private class Configuration
{
    [JsonProperty(PropertyName = "CUI")]
    public ConfigurationCui cui { get; set; }
    [JsonProperty(PropertyName = "Functions")]
    public ConfigurationFunctions functions { get; set; }
    [JsonProperty(PropertyName = "Generic")]
    public ConfigurationGeneric generic { get; set; }
    [JsonProperty(PropertyName = "Night Bonus")]
    public ConfigurationNightBonus nightbonus { get; set; }
    [JsonProperty(PropertyName = "Settings")]
    public ConfigurationSettings settings { get; set; }
    [JsonProperty(PropertyName = "Level Up Rewards (Reward * Level = Amount)")]
    public Rewards Rewards { get; set; }
}

private class ConfigurationCui
{
    [JsonProperty(PropertyName = "Bounds")]
    public ConfigurationCuiPositions cuiPositioning { get; set; }
    [JsonProperty(PropertyName = "Xp Bar Colors")]
    public ConfigurationColors cuiColors { get; set; }
    [JsonProperty(PropertyName = "Bounds Background")]
    public string cuiBoundsBackground { get; set; }
    [JsonProperty(PropertyName = "CUI Enabled")]
    public bool cuiEnabled { get; set; }
    [JsonProperty(PropertyName = "Font Color")]
    public string cuiFontColor { get; set; }
    [JsonProperty(PropertyName = "FontSize Bar")]
    public int cuiFontSizeBar { get; set; }
    [JsonProperty(PropertyName = "FontSize Level")]
    public int cuiFontSizeLvl { get; set; }
    [JsonProperty(PropertyName = "FontSize Percent")]
    public int cuiFontSizePercent { get; set; }
    [JsonProperty(PropertyName = "Text Shadow Enabled")]
    public bool cuiTextShadow { get; set; }
    [JsonProperty(PropertyName = "Xp Bar Background")]
    public string cuiXpBarBackground { get; set; }
}

private class ConfigurationFunctions
{
    [JsonProperty(PropertyName = "Collectible Entities", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, bool> enabledCollectibleEntity { get; set; }
    [JsonProperty(PropertyName = "Enable Collectible Pickup")]
    public bool enableCollectiblePickup { get; set; }
    [JsonProperty(PropertyName = "Enable Crop Gather")]
    public bool enableCropGather { get; set; }
    [JsonProperty(PropertyName = "Enable Wood Gather")]
    public bool wood { get; set; }
    [JsonProperty(PropertyName = "Grant Wood XP Only")]
    public bool woodXpOnly { get; set; }
    [JsonProperty(PropertyName = "Enable Stone Ore Gather")]
    public bool stone { get; set; }
    [JsonProperty(PropertyName = "Grant Stone XP Only")]
    public bool stoneXpOnly { get; set; }
    [JsonProperty(PropertyName = "Enable Sulfur Ore Gather")]
    public bool sulfur { get; set; }
    [JsonProperty(PropertyName = "Grant Sulfur XP Only")]
    public bool sulfurXpOnly { get; set; }
    [JsonProperty(PropertyName = "Enable Metal Gather")]
    public bool metal { get; set; }
    [JsonProperty(PropertyName = "Grant Metal XP Only")]
    public bool metalXpOnly { get; set; }
    [JsonProperty(PropertyName = "Enable HQM Gather")]
    public bool hqm { get; set; }
    [JsonProperty(PropertyName = "Grant HQM XP Only")]
    public bool hqmXpOnly { get; set; }
    [JsonProperty(PropertyName = "Allow Mining Multiplier On Gibs")]
    public bool gibs { get; set; }
    internal bool enableDispenserGather { get; set; }
    public bool ShouldAllowGather(string shortName, bool grantXPOnly);
}

private class ConfigurationGeneric
{
    [JsonProperty(PropertyName = "Enable Level Up Broadcast")]
    public bool enableLevelupBroadcast { get; set; }
    [JsonProperty(PropertyName = "Enable Permission")]
    public bool enablePermission { get; set; }
    [JsonProperty(PropertyName = "Chainsaw On Gather Permission")]
    public string AllowChainsawGather { get; set; }
    [JsonProperty(PropertyName = "Jackhammer On Gather Permission")]
    public string AllowJackhammerGather { get; set; }
    [JsonProperty(PropertyName = "Weapons On Gather Permission")]
    public string BlockWeaponsGather { get; set; }
    [JsonProperty(PropertyName = "gameProtocol")]
    public int gameProtocol { get; set; }
    [JsonProperty(PropertyName = "Penalty Minutes")]
    public int penaltyMinutes { get; set; }
    [JsonProperty(PropertyName = "Penalty Minutes (Suicide)")]
    public int penaltySuicideMinutes { get; set; }
    [JsonProperty(PropertyName = "Penalty Resets All Levels To Default")]
    public bool penaltyReset { get; set; }
    [JsonProperty(PropertyName = "Penalty On Death")]
    public bool penaltyOnDeath { get; set; }
    [JsonProperty(PropertyName = "Penalty On Suicide")]
    public bool penaltyOnSuicide { get; set; }
    [JsonProperty(PropertyName = "Permission Name")]
    public string permissionName { get; set; }
    [JsonProperty(PropertyName = "Permission Name XP")]
    public string permissionNameXP { get; set; }
    [JsonProperty(PropertyName = "Additional Boost Multipliers")]
    public Dictionary<string, double> vip { get; set; }
    [JsonProperty(PropertyName = "Permission Name No Wipes")]
    public string nowipe { get; set; }
    [JsonProperty(PropertyName = "Player CUI Default Enabled")]
    public bool playerCuiDefaultEnabled { get; set; }
    [JsonProperty(PropertyName = "Player Plugin Default Enabled")]
    public bool playerPluginDefaultEnabled { get; set; }
    [JsonProperty(PropertyName = "Plugin Prefix")]
    public string pluginPrefix { get; set; }
    [JsonProperty(PropertyName = "SteamID Icon")]
    public ulong steamIDIcon { get; set; }
    [JsonProperty(PropertyName = "Wipe Data OnNewSave")]
    public bool wipeDataOnNewSave { get; set; }
}

private class ConfigurationNightBonus
{
    [JsonProperty(PropertyName = "Points Per Hit At Night")]
    public ConfigurationResources pointsPerHitAtNight { get; set; }
    [JsonProperty(PropertyName = "Points Per PowerTool At Night")]
    public ConfigurationResources pointsPerPowerToolAtNight { get; set; }
    [JsonProperty(PropertyName = "Resource Per Level Multiplier At Night")]
    public ConfigurationResources resourceMultipliersAtNight { get; set; }
    [JsonProperty(PropertyName = "Enable Night Bonus")]
    public bool enableNightBonus { get; set; }
    [JsonProperty(PropertyName = "Broadcast Enabled Bonus")]
    public bool broadcastEnabledBonus { get; set; }
    [JsonProperty(PropertyName = "Log Enabled Bonus Console")]
    public bool logEnabledBonusConsole { get; set; }
}

private class ConfigurationSettings
{
    [JsonProperty(PropertyName = "Crafting Details", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public ConfigurationCraftingDetails craftingDetails { get; set; }
    [JsonProperty(PropertyName = "Default Resource Multiplier")]
    public ConfigurationResources defaultMultipliers { get; set; }
    [JsonProperty(PropertyName = "Level Caps")]
    public ConfigurationResources levelCaps { get; set; }
    [JsonProperty(PropertyName = "Percent Lost On Death")]
    public ConfigurationResources percentLostOnDeath { get; set; }
    [JsonProperty(PropertyName = "Percent Lost On Suicide")]
    public ConfigurationResources percentLostOnSuicide { get; set; }
    [JsonProperty(PropertyName = "No Penalty Zones", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> zones { get; set; }
    [JsonProperty(PropertyName = "Points Per Hit")]
    public ConfigurationResources pointsPerHit { get; set; }
    [JsonProperty(PropertyName = "Points Per Power Tool")]
    public ConfigurationResources pointsPerPowerTool { get; set; }
    [JsonProperty(PropertyName = "Resource Per Level Multiplier")]
    public ConfigurationResources resourceMultipliers { get; set; }
    [JsonProperty(PropertyName = "Skill Colors")]
    public ConfigurationSkillColors colors { get; set; }
    [JsonProperty(PropertyName = "Starting Stats")]
    public ConfigurationStartingStats stats { get; set; }
    [JsonProperty(PropertyName = "Use Mining Skill When Picking Up Stones And Ore")]
    public bool MiningStonesOre { get; set; }
    [JsonProperty(PropertyName = "Use Acquire Skill When Picking Up Wood")]
    public bool AcquireWood { get; set; }
}

public class ConfigurationStartingStats
{
    [JsonProperty(PropertyName = "Acquire Level")]
    public double acquire_level { get; set; }
    [JsonProperty(PropertyName = "Acquire Points")]
    public double acquire_points { get; set; }
    [JsonProperty(PropertyName = "Crafting Level")]
    public double crafting_level { get; set; }
    [JsonProperty(PropertyName = "Crafting Points")]
    public double crafting_points { get; set; }
    [JsonProperty(PropertyName = "Mining Level")]
    public double mining_level { get; set; }
    [JsonProperty(PropertyName = "Mining Points")]
    public double mining_points { get; set; }
    [JsonProperty(PropertyName = "Skinning Level")]
    public double skinning_level { get; set; }
    [JsonProperty(PropertyName = "Skinning Points")]
    public double skinning_points { get; set; }
    [JsonProperty(PropertyName = "Woodcutting Level")]
    public double woodcutting_level { get; set; }
    [JsonProperty(PropertyName = "Woodcutting Points")]
    public double woodcutting_points { get; set; }
    [JsonProperty(PropertyName = "XP Multiplier")]
    public double xpm { get; set; }
}

public class ConfigurationResources
{
    [JsonProperty(PropertyName = nameof(Skills.ACQUIRE), NullValueHandling = NullValueHandling.Ignore)]
    public double? ACQUIRE { get; set; }
    [JsonProperty(PropertyName = nameof(Skills.CRAFTING), NullValueHandling = NullValueHandling.Ignore)]
    public double? CRAFTING { get; set; }
    [JsonProperty(PropertyName = nameof(Skills.MINING), NullValueHandling = NullValueHandling.Ignore)]
    public double? MINING { get; set; }
    [JsonProperty(PropertyName = nameof(Skills.SKINNING), NullValueHandling = NullValueHandling.Ignore)]
    public double? SKINNING { get; set; }
    [JsonProperty(PropertyName = nameof(Skills.WOODCUTTING), NullValueHandling = NullValueHandling.Ignore)]
    public double? WOODCUTTING { get; set; }
    public ConfigurationResources(double? ACQUIRE, double? CRAFTING, double? MINING, double? SKINNING, double? WOODCUTTING);
    public double Get(Skills skill);
    public void Set(Skills skill, double value);
}

public class ConfigurationColors
{
    [JsonProperty(PropertyName = nameof(Skills.ACQUIRE))]
    public string ACQUIRE { get; set; }
    [JsonProperty(PropertyName = nameof(Skills.CRAFTING))]
    public string CRAFTING { get; set; }
    [JsonProperty(PropertyName = nameof(Skills.MINING))]
    public string MINING { get; set; }
    [JsonProperty(PropertyName = nameof(Skills.SKINNING))]
    public string SKINNING { get; set; }
    [JsonProperty(PropertyName = nameof(Skills.WOODCUTTING))]
    public string WOODCUTTING { get; set; }
    public ConfigurationColors(string ACQUIRE, string CRAFTING, string MINING, string SKINNING, string WOODCUTTING);
    public string Get(Skills skill);
}

public class ConfigurationCraftingDetails
{
    [JsonProperty(PropertyName = "Time Spent")]
    public double time { get; set; }
    [JsonProperty(PropertyName = "XP Per Time Spent")]
    public double xp { get; set; }
    [JsonProperty(PropertyName = "Percent Faster Per Level")]
    public double percent { get; set; }
    [JsonProperty(PropertyName = "Require Permission For Instant Bulk Craft")]
    public bool usePermission { get; set; }
    [JsonProperty(PropertyName = "Instant Bulk Craft Permission Does Not Require Max Level")]
    public bool noLevelRequirement { get; set; }
    [JsonProperty(PropertyName = "Permission For Instant Bulk Crafting At Max Level")]
    public string Permission { get; set; }
    [JsonProperty(PropertyName = "Require Inventory Slots")]
    public bool slots;
    public ConfigurationCraftingDetails(double time, double xp, double percent);
}

public class ConfigurationCuiPositions
{
    [JsonProperty(PropertyName = "Width Left")]
    public string widthLeft { get; set; }
    [JsonProperty(PropertyName = "Width Right")]
    public string widthRight { get; set; }
    [JsonProperty(PropertyName = "Height Lower")]
    public string heightLower { get; set; }
    [JsonProperty(PropertyName = "Height Upper")]
    public string heightUpper { get; set; }
    public ConfigurationCuiPositions(string widthLeft, string widthRight, string heightLower, string heightUpper);
}

public class ConfigurationSkillColors
{
    [JsonProperty(PropertyName = nameof(Skills.ACQUIRE))]
    public string ACQUIRE { get; set; }
    [JsonProperty(PropertyName = nameof(Skills.CRAFTING))]
    public string CRAFTING { get; set; }
    [JsonProperty(PropertyName = nameof(Skills.MINING))]
    public string MINING { get; set; }
    [JsonProperty(PropertyName = nameof(Skills.SKINNING))]
    public string SKINNING { get; set; }
    [JsonProperty(PropertyName = nameof(Skills.WOODCUTTING))]
    public string WOODCUTTING { get; set; }
    public string Get(Skills skill);
}

public class RewardType
{
    [JsonProperty(PropertyName = "Acquire")]
    public double Acquire { get; set; }
    [JsonProperty(PropertyName = "Crafting")]
    public double Crafting { get; set; }
    [JsonProperty(PropertyName = "Mining")]
    public double Mining { get; set; }
    [JsonProperty(PropertyName = "Skinning")]
    public double Skinning { get; set; }
    [JsonProperty(PropertyName = "Woodcutting")]
    public double Woodcutting { get; set; }
    public double Get(Skills skill);
}

public class Rewards
{
    [JsonProperty(PropertyName = "Economics Money")]
    public RewardType Money { get; set; }
    [JsonProperty(PropertyName = "ServerRewards Points")]
    public RewardType Points { get; set; }
    [JsonProperty(PropertyName = "SkillTree XP")]
    public RewardType XP { get; set; }
}


```

---

## ZoneChatPrefix by  - Adds a Zone prefix to player chat

```csharp
using System.Collections.Generic;
using System.Linq;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Zone Chat Prefix", "BuzZ", "0.0.5")]
[Description("Adds a zone prefix to player chat")]
public class ZoneChatPrefix : RustPlugin
{
    [PluginReference]
     Plugin ZoneManager;
     bool debug;
    public List<BasePlayer> blabla;
     string StringZonesPlayerIsIn(BasePlayer player);
     string GetThatZoneNamePlease(string zone_id);
     object OnPlayerChat(BasePlayer player, string message);
}


```

---

## ZoneCommand by misticos - Execute commands when player enters a zone

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Zone Command", "misticos", "1.0.0")]
[Description("Execute commands when player enters a zone")]
 class ZoneCommand : CovalencePlugin
{
    private const string PermissionCommand;
    [PluginReference("PlaceholderAPI")]
    private Plugin _placeholders;
    private Action<IPlayer, StringBuilder, bool> _placeholderProcessor;
    private StringBuilder _builder;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty("Commands")]
        public string[] Commands;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private Dictionary<string, HashSet<ZoneAction>> _actionsPerZone;
    private Dictionary<string, ZoneData> _loadedData;
    private string[] GetAllData();
    private void SaveData(string filename);
    private ZoneData GetOrLoadData(string filename);
    private class ZoneData
    {
        [JsonIgnore]
        public string Filename;
        [JsonProperty("Zone ID")]
        public string Zone;
        [JsonProperty("Actions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ZoneAction> Actions;
    }

    private class ZoneAction
    {
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty("Frequency")]
        public FrequencyMode Frequency;
        [JsonProperty("Time Frequency")]
        public TimeSpan? Time;
        [JsonProperty("Use Game Time")]
        public bool GameTime;
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty("Run On")]
        public RunMode RunOn;
        [JsonProperty("Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ZoneCommand> Commands;
        public class ZoneCommand
        {
            [JsonProperty("Command")]
            public string Command;
            [JsonProperty("Arguments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public object[] Arguments;
            [JsonProperty("Clientside")]
            public bool Clientside;
            [JsonProperty("Chat Command")]
            public bool Chat;
            public void Execute(BasePlayer player, Oxide.Plugins.ZoneCommand instance);
        }

    }

    private Dictionary<string, ZoneActionMeta> _metas;
    private string[] GetAllMeta();
    private void SaveMeta(string filename);
    private ZoneActionMeta GetOrLoadMeta(string filename);
    private class ZoneActionMeta
    {
        public Dictionary<ulong, List<DateTime>> TriggerTime;
        public Dictionary<ulong, List<DateTime>> TriggerGameTime;
        public List<DateTime> GetTriggerTime(ulong id);
        public List<DateTime> GetTriggerGameTime(ulong id);
    }

    protected override void LoadDefaultMessages();
    private void Init();
    private void OnServerSave();
    private void OnEnterZone(string zoneId, BasePlayer player);
    private void OnExitZone(string zoneId, BasePlayer player);
    private void OnPlaceholderAPIReady();
    private void OnPluginUnloaded(Plugin plugin);
    private void OnZone(string id, BasePlayer player, ZoneAction.RunMode mode);
    private void CommandZone(IPlayer player, string command, string[] args);
    private string GetMsg(string key, string id);
}

private class Configuration
{
    [JsonProperty("Commands")]
    public string[] Commands;
}

private class ZoneData
{
    [JsonIgnore]
    public string Filename;
    [JsonProperty("Zone ID")]
    public string Zone;
    [JsonProperty("Actions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ZoneAction> Actions;
}

private class ZoneAction
{
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty("Frequency")]
    public FrequencyMode Frequency;
    [JsonProperty("Time Frequency")]
    public TimeSpan? Time;
    [JsonProperty("Use Game Time")]
    public bool GameTime;
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty("Run On")]
    public RunMode RunOn;
    [JsonProperty("Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ZoneCommand> Commands;
    public class ZoneCommand
    {
        [JsonProperty("Command")]
        public string Command;
        [JsonProperty("Arguments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public object[] Arguments;
        [JsonProperty("Clientside")]
        public bool Clientside;
        [JsonProperty("Chat Command")]
        public bool Chat;
        public void Execute(BasePlayer player, Oxide.Plugins.ZoneCommand instance);
    }

}

public class ZoneCommand
{
    [JsonProperty("Command")]
    public string Command;
    [JsonProperty("Arguments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public object[] Arguments;
    [JsonProperty("Clientside")]
    public bool Clientside;
    [JsonProperty("Chat Command")]
    public bool Chat;
    public void Execute(BasePlayer player, Oxide.Plugins.ZoneCommand instance);
}

private class ZoneActionMeta
{
    public Dictionary<ulong, List<DateTime>> TriggerTime;
    public Dictionary<ulong, List<DateTime>> TriggerGameTime;
    public List<DateTime> GetTriggerTime(ulong id);
    public List<DateTime> GetTriggerGameTime(ulong id);
}


```

---

## ZoneDomes by k1lly0u - Assign shaded sphere entities to physically see the border of zones

```csharp
using Facepunch;
using System;
using System.Text;
using System.Collections.Generic;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("ZoneDomes", "k1lly0u", "2.0.2")]
[Description("Assign shaded sphere entities to physically see the border of zones")]
 class ZoneDomes : RustPlugin
{
    [PluginReference]
    private Plugin ZoneManager;
    private StoredData _storedData;
    private DynamicConfigFile _dataFile;
    private const string SHADED_SPHERE;
    private const string BR_SPHERE_RED;
    private const string BR_SPHERE_BLUE;
    private const string BR_SPHERE_GREEN;
    private const string BR_SPHERE_PURPLE;
    private const string USE_PERMISSION;
    private readonly Hash<string, List<SphereEntity>> _sphereEntities;
    private void Loaded();
    private void OnServerInitialized();
    protected override void LoadDefaultMessages();
    private void Unload();
    private void InitializeDomes();
    private void CreateDome(string zoneId, StoredData.Zone zone);
    private void DestroyDomeForZone(string zoneId);
    private void TranslateMessage(BasePlayer player, string key, string additional);
    private string TranslateMessage(string key, BasePlayer player);
    private bool IsValidZoneID(string zoneId);
    private bool TryGetZoneLocation(string zoneId, Vector3 position);
    private bool TryGetZoneRadius(string zoneId, float radius);
    [HookMethod("AddNewDome")]
    public bool AddNewDome(BasePlayer player, string zoneId, int type, int stack);
    [HookMethod("RemoveExistingDome")]
    public bool RemoveExistingDome(BasePlayer player, string zoneId);
    [ChatCommand("zd")]
    private void CommandZoneDomes(BasePlayer player, string command, string[] args);
    private ConfigData _configData;
    private class ConfigData
    {
        public VersionNumber Version { get; set; }
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
        public Dictionary<string, Zone> Zones;
        public class Zone
        {
            public Vector3 Position;
            public float Radius;
            public SphereType Type;
            public int Stack;
        }

    }

    private class UnityVector3Converter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

}

private class ConfigData
{
    public VersionNumber Version { get; set; }
}

private class StoredData
{
    public Dictionary<string, Zone> Zones;
    public class Zone
    {
        public Vector3 Position;
        public float Radius;
        public SphereType Type;
        public int Stack;
    }

}

public class Zone
{
    public Vector3 Position;
    public float Radius;
    public SphereType Type;
    public int Stack;
}

private class UnityVector3Converter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}


```

---

## ZoneManager by k1lly0u - Advanced zone manager for creating in-game zones

```csharp
using Facepunch;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Reflection;
using System.Text;
using UnityEngine;
using Debug = UnityEngine.Debug;

Oxide.Plugins
[Info("Zone Manager", "k1lly0u", "3.1.7")]
[Description("An advanced management system for creating in-game zones")]
public class ZoneManager : RustPlugin
{
    [PluginReference]
     Plugin Backpacks;
     Plugin PopupNotifications;
     Plugin Spawns;
    private StoredData storedData;
    private DynamicConfigFile data;
    private readonly Hash<string, Zone> zones;
    private static readonly Hash<ulong, EntityZones> zonedPlayers;
    private static readonly Hash<NetworkableId, EntityZones> zonedEntities;
    private readonly Dictionary<ulong, string> lastPlayerZone;
    private readonly ZoneFlags globalFlags;
    private readonly ZoneFlags adminBypass;
    private static readonly ZoneFlags tempFlags;
    private static readonly StringBuilder sb;
    private bool zonesInitialized;
    private static ZoneManager Instance { get; set; }
    private const string PERMISSION_ZONE;
    private const string PERMISSION_IGNORE_FLAG;
    private const int PLAYER_MASK;
    private const int TARGET_LAYERS;
    private void Init();
    private void OnServerInitialized();
    private void OnTerrainInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void OnEntityKill(BaseEntity baseEntity);
    private void Unload();
    private UpdateBehaviour m_UpdateBehaviour;
    private UpdateBehaviour updateBehaviour { get; set; }
    private void DestroyUpdateBehaviour();
    private class UpdateBehaviour : MonoBehaviour
    {
        private readonly System.Diagnostics.Stopwatch sw;
        private readonly Queue<BasePlayer> playerUpdateQueue;
        private const float MAX_MS;
        private void OnDestroy();
        public void QueueUpdate(BasePlayer player);
        public void Reset();
        private void Update();
    }

    private void OnEntityBuilt(Planner planner, GameObject gameObject);
    private object OnStructureUpgrade(BuildingBlock buildingBlock, BasePlayer player, BuildingGrade.Enum grade);
    private void OnItemDeployed(Deployer deployer, ItemModDeployable itemModDeployable, BaseEntity deployedEntity);
    private void OnItemDeployed(Deployer deployer, BaseEntity parentEntity, BaseEntity deployedEntity);
    private void KillEntityAndReturnItem(BasePlayer player, BaseEntity entity, Item item);
    private void OnItemUse(Item item, int amount);
    private object OnPlayerChat(BasePlayer player, string message, ConVar.Chat.ChatChannel channel);
    private object OnBetterChat(Oxide.Core.Libraries.Covalence.IPlayer iPlayer, string message);
    private object OnPlayerVoice(BasePlayer player, Byte[] data);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private void OnPlayerDisconnected(BasePlayer player);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitinfo);
    private void OnEntitySpawned(BaseEntity baseEntity);
    private void CanSpawn(BaseEntity baseEntity);
    private object CanBeWounded(BasePlayer player, HitInfo hitinfo);
    private object CanUpdateSign(BasePlayer player, Signage sign);
    private object OnOvenToggle(BaseOven oven, BasePlayer player);
    private object CanUseVending(BasePlayer player, VendingMachine machine);
    private object CanHideStash(BasePlayer player, StashContainer stash);
    private object CanCraft(ItemCrafter itemCrafter, ItemBlueprint bp, int amount);
    private void OnDoorOpened(Door door, BasePlayer player);
    private object OnSprayCreate(SprayCan sprayCan, Vector3 position, Quaternion rotation);
    private object CanLootPlayer(BasePlayer target, BasePlayer looter);
    private void OnLootPlayer(BasePlayer looter, BasePlayer target);
    private object OnLootPlayerInternal(BasePlayer looter, BasePlayer target);
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private object CanLootEntity(BasePlayer player, LootableCorpse corpse);
    private void OnLootCorpse(LootableCorpse corpse, BasePlayer player);
    private void OnLootContainer(DroppedItemContainer container, BasePlayer player);
    private object CanLootEntity(BasePlayer player, DroppedItemContainer container);
    private object CanLootEntity(BasePlayer player, StorageContainer container);
    private object CanLootInternal(BasePlayer player, int flag);
    private void OnLootInternal(BasePlayer player, int flag);
    private object CanPickupEntity(BasePlayer player, BaseCombatEntity entity);
    private object CanPickupLock(BasePlayer player, BaseLock baseLock);
    private object OnItemPickup(Item item, BasePlayer player);
    private object CanPickupInternal(BasePlayer player, int flag);
    private object CanLootEntity(ResourceContainer container, BasePlayer player);
    private object OnCollectiblePickup(Item item, BasePlayer player);
    private object OnGrowableGather(GrowableEntity plant, Item item, BasePlayer player);
    private object OnDispenserGather(ResourceDispenser dispenser, BasePlayer player, Item item);
    private object OnGatherInternal(BasePlayer player);
    private object OnTurretTarget(AutoTurret turret, BasePlayer player);
    private object CanBradleyApcTarget(BradleyAPC apc, BasePlayer player);
    private object CanHelicopterTarget(PatrolHelicopterAI heli, BasePlayer player);
    private object CanHelicopterStrafeTarget(PatrolHelicopterAI heli, BasePlayer player);
    private object OnHelicopterTarget(HelicopterTurret turret, BasePlayer player);
    private object OnNpcTarget(BaseCombatEntity entity, BasePlayer player);
    private object OnTargetPlayerInternal(BasePlayer player, int flag);
    private object CanMountEntity(BasePlayer player, BaseMountable entity);
    private object CanDismountEntity(BasePlayer player, BaseMountable entity);
    private void OnPlayerSleep(BasePlayer player);
    private void OnPlayerSleepEnd(BasePlayer player);
    private void KillSleepingPlayer(BasePlayer player);
    private void UpdatePlayerZones(BasePlayer player);
    private bool IsInsideBounds(Zone zone, Vector3 worldPos);
    private T GetEntityComponent(BaseEntity entity);
    private void InitializeZones();
    private void CreateZone(Zone.Definition definition);
    private static bool ReverseVelocity(BaseVehicle baseVehicle);
    private void EjectPlayer(BasePlayer player, Zone zone);
    private void AttractPlayer(BasePlayer player, Zone zone);
    private void ShowZone(BasePlayer player, string zoneId, float time);
    private Vector3 RotatePointAroundPivot(Vector3 point, Vector3 pivot, Quaternion rotation);
    public class Zone : MonoBehaviour
    {
        public Definition definition;
        public ZoneFlags disabledFlags;
        public Zone parent;
        public List<BasePlayer> players;
        public List<BaseEntity> entities;
        private List<IOEntity> ioEntities;
        public List<ulong> keepInList;
        public List<ulong> whitelist;
        public Hash<ulong, EntityZones> entityZones;
        private Rigidbody rigidbody;
        public Collider collider;
        public Bounds colliderBounds;
        private ChildSphereTrigger<TriggerRadiation> radiation;
        private ChildSphereTrigger<TriggerComfort> comfort;
        private ChildSphereTrigger<TriggerTemperature> temperature;
        private ChildSphereTrigger<TriggerSafeZone> safeZone;
        private readonly Hash<BaseVehicle, float> lastReversedTimes;
        private int creationFrame;
        private bool isTogglingLights;
        private void Awake();
        private void OnDestroy();
        private void EmptyZone();
        public void InitializeZone(Definition definition);
        public void FindZoneParent();
        public void Reset();
        public void OnZoneFlagsChanged();
        private void RegisterPermission();
        private void InitializeCollider();
        private void InitializeRadiation();
        private void InitializeComfort();
        private void InitializeTemperature();
        private void InitializeSafeZone();
        private void AddToTrigger(TriggerBase triggerBase, BasePlayer player);
        private void RemoveFromTrigger(TriggerBase triggerBase, BasePlayer player);
        private void AddPlayersToTriggers();
        private void RemovePlayersFromTriggers();
        private class ChildSphereTrigger
        {
            public GameObject Object { get; set; }
            public SphereCollider Collider { get; set; }
            public T Trigger { get; set; }
            public ChildSphereTrigger(GameObject parent, string name);
            public void Destroy();
        }

        private bool isLightsOn;
        private void InitializeAutoLights();
        private void CheckAlwaysLights();
        private void CheckLights();
        private IEnumerator ToggleLights(bool active);
        private bool ToggleLight(BaseEntity baseEntity, bool active, bool requiresFuel);
        private void OnTriggerEnter(Collider col);
        private void OnTriggerExit(Collider col);
        public void OnPlayerEnterZone(BasePlayer player);
        public void OnPlayerExitZone(BasePlayer player);
        public void OnEntityEnterZone(BaseEntity baseEntity);
        public void OnEntityExitZone(BaseEntity baseEntity, bool resetDecay, bool isDead);
        private void IOTick();
        public bool TryReverseVelocity(BaseEntity baseEntity);
        private bool CanReverseVelocity(BaseVehicle baseVehicle);
        public bool HasPermission(BasePlayer player);
        public bool CanLeaveZone(BasePlayer player);
        public bool CanEnterZone(BasePlayer player);
        public void AddFlag(int flag);
        public void RemoveFlag(int flag);
        public bool HasFlag(int flag);
        public bool HasDisabledFlag(int flag);
        public void AddDisabledFlag(int flag);
        public void RemoveDisabledFlag(int flag);
        public class Definition
        {
            public string Name { get; set; }
            public float Radius { get; set; }
            public float Radiation { get; set; }
            public float Comfort { get; set; }
            public float Temperature { get; set; }
            public bool SafeZone { get; set; }
            public Vector3 Location { get; set; }
            public Vector3 Size { get; set; }
            public Vector3 Rotation { get; set; }
            public string Id { get; set; }
            public string ParentID { get; set; }
            public string EnterMessage { get; set; }
            public string LeaveMessage { get; set; }
            public string Permission { get; set; }
            public string EjectSpawns { get; set; }
            public bool Enabled { get; set; }
            public ZoneFlags Flags { get; set; }
            public Definition();
            public Definition(Vector3 position);
        }

    }

    private void OnPlayerEnterZone(BasePlayer player, Zone zone);
    private void OnPlayerExitZone(BasePlayer player, Zone zone);
    private void UpdateMetabolismForPlayer(BasePlayer player);
    private void OnEntityEnterZone(BaseEntity baseEntity, Zone zone);
    private void OnEntityExitZone(BaseEntity baseEntity, Zone zone);
    private bool IsAdmin(BasePlayer player);
    private bool IsNpc(BasePlayer player);
    private bool HasPermission(BasePlayer player, string permname);
    private bool HasPermission(ConsoleSystem.Arg arg, string permname);
    private bool CanBypass(BasePlayer player, int flag);
    private bool CanBypass(ulong playerId, int flag);
    private bool CanBypass(string playerId, int flag);
    private void SendMessage(BasePlayer player, string message, object[] args);
    private Zone GetZoneByID(string zoneId);
    private void AddToKeepinlist(Zone zone, BasePlayer player);
    private void RemoveFromKeepinlist(Zone zone, BasePlayer player);
    private void AddToWhitelist(Zone zone, BasePlayer player);
    private void RemoveFromWhitelist(Zone zone, BasePlayer player);
    private bool HasPlayerFlag(BasePlayer player, int flag);
    private bool HasEntityFlag(BaseEntity baseEntity, int flag);
    private void SetZoneStatus(string zoneId, bool active);
    private Vector3 GetZoneLocation(string zoneId);
    private object GetZoneRadius(string zoneID);
    private object GetZoneSize(string zoneID);
    private object GetZoneName(string zoneID);
    private object CheckZoneID(string zoneID);
    private object GetZoneIDs();
    private bool IsPositionInZone(string zoneID, Vector3 position);
    private List<BasePlayer> GetPlayersInZone(string zoneID);
    private List<BaseEntity> GetEntitiesInZone(string zoneId);
    private bool isPlayerInZone(string zoneID, BasePlayer player);
    private bool isEntityInZone(string zoneID, BaseEntity entity);
    private bool IsPlayerInZone(string zoneID, BasePlayer player);
    private bool IsEntityInZone(string zoneID, BaseEntity entity);
    private string[] GetPlayerZoneIDs(BasePlayer player);
    private string[] GetEntityZoneIDs(BaseEntity entity);
    private bool HasFlag(string zoneId, string flagName);
    private void AddFlag(string zoneId, string flagName);
    private void RemoveFlag(string zoneId, string flagName);
    private bool HasDisabledFlag(string zoneId, string flagName);
    private void AddDisabledFlag(string zoneId, string flagName);
    private void RemoveDisabledFlag(string zoneId, string flagName);
    private bool CreateOrUpdateZone(string zoneId, string[] args, Vector3 position);
    private bool EraseZone(string zoneId);
    private List<string> ZoneFieldListRaw();
    private Dictionary<string, string> ZoneFieldList(string zoneId);
    private bool AddPlayerToZoneKeepinlist(string zoneId, BasePlayer player);
    private bool RemovePlayerFromZoneKeepinlist(string zoneId, BasePlayer player);
    private bool AddPlayerToZoneWhitelist(string zoneId, BasePlayer player);
    private bool RemovePlayerFromZoneWhitelist(string zoneId, BasePlayer player);
    private bool EntityHasFlag(BaseEntity baseEntity, string flagName);
    private bool PlayerHasFlag(BasePlayer player, string flagName);
    private object CanRedeemKit(BasePlayer player);
    private object CanTeleport(BasePlayer player);
    private object canRemove(BasePlayer player);
    private object CanRemove(BasePlayer player);
    private bool CanChat(BasePlayer player);
    private object CanTrade(BasePlayer player);
    private object canShop(BasePlayer player);
    private object CanShop(BasePlayer player);
    public class ZoneFlags
    {
        public static readonly int AutoLights;
        public static readonly int AlwaysLights;
        public static readonly int NoKits;
        public static readonly int NoTrade;
        public static readonly int NoShop;
        public static readonly int NoTp;
        public static readonly int NoRemove;
        public static readonly int NoPoison;
        public static readonly int NoStarvation;
        public static readonly int NoThirst;
        public static readonly int NoRadiation;
        public static readonly int NoDrown;
        public static readonly int NoBleed;
        public static readonly int NoWounded;
        public static readonly int Eject;
        public static readonly int EjectSleepers;
        public static readonly int PvpGod;
        public static readonly int PveGod;
        public static readonly int SleepGod;
        public static readonly int NoPve;
        public static readonly int NoFallDamage;
        public static readonly int UnDestr;
        public static readonly int NoBuildingDamage;
        public static readonly int NoDecay;
        public static readonly int NoBuild;
        public static readonly int NoStability;
        public static readonly int NoUpgrade;
        public static readonly int NoSprays;
        public static readonly int NoDeploy;
        public static readonly int PoweredSwitches;
        public static readonly int LootSelf;
        public static readonly int NoPlayerLoot;
        public static readonly int NoNPCLoot;
        public static readonly int NoBoxLoot;
        public static readonly int NoPickup;
        public static readonly int NoCollect;
        public static readonly int NoDrop;
        public static readonly int NoLootSpawns;
        public static readonly int NoEntityPickup;
        public static readonly int NoGather;
        public static readonly int NoHeliTargeting;
        public static readonly int NoTurretTargeting;
        public static readonly int NoAPCTargeting;
        public static readonly int NoNPCTargeting;
        public static readonly int NpcFreeze;
        public static readonly int NoNPCSpawns;
        public static readonly int KeepVehiclesIn;
        public static readonly int KeepVehiclesOut;
        public static readonly int NoVehicleMounting;
        public static readonly int NoVehicleDismounting;
        public static readonly int NoChat;
        public static readonly int NoVoice;
        public static readonly int NoCorpse;
        public static readonly int NoSuicide;
        public static readonly int KillSleepers;
        public static readonly int Kill;
        public static readonly int InfiniteTrapAmmo;
        public static readonly int NoCup;
        public static readonly int NoSignUpdates;
        public static readonly int NoOvenToggle;
        public static readonly int NoVending;
        public static readonly int NoStash;
        public static readonly int NoCraft;
        public static readonly int NoDoorAccess;
        public static readonly int Custom1;
        public static readonly int Custom2;
        public static readonly int Custom3;
        public static readonly int Custom4;
        public static readonly int Custom5;
        public static readonly Hash<string, int> NameToIndex;
        public static readonly Hash<int, string> IndexToName;
        private static readonly int Count;
        private readonly BitArray m_BitArray;
        static ZoneFlags();
        public static bool Find(string flagName, int index);
        public ZoneFlags();
        public bool HasFlag(int flag);
        public void AddFlag(int flag);
        public void RemoveFlag(int flag);
        public void SetFlags(int[] array);
        public void Clear();
        public bool HasFlag(string flagName);
        public void AddFlag(string flagName);
        public void RemoveFlag(string flagName);
        public void SetFlags(string[] array);
        public void AddFlags(ZoneFlags zoneFlags);
        public void RemoveFlags(ZoneFlags zoneFlags);
        public bool CompareTo(ZoneFlags zoneFlags);
        public override string ToString();
    }

    private void AddFlag(Zone zone, int flag);
    private void RemoveFlag(Zone zone, int flag);
    private bool HasFlag(Zone zone, int flag);
    private void UpdateZoneEntityFlags(Zone zone);
    private bool HasGlobalFlag(int flag);
    private void UpdateGlobalFlags();
    private bool NeedsUpdateSubscriptions();
    private void UpdateHookSubscriptions();
    private void UnsubscribeAll();
    private void UpdateZoneDefinition(Zone zone, string[] args, BasePlayer player);
    [ChatCommand("zone_add")]
    private void cmdChatZoneAdd(BasePlayer player, string command, string[] args);
    [ChatCommand("zone_wipe")]
    private void cmdChatZoneReset(BasePlayer player, string command, string[] args);
    [ChatCommand("zone_remove")]
    private void cmdChatZoneRemove(BasePlayer player, string command, string[] args);
    [ChatCommand("zone_stats")]
    private void cmdChatZoneStats(BasePlayer player, string command, string[] args);
    [ChatCommand("zone_edit")]
    private void cmdChatZoneEdit(BasePlayer player, string command, string[] args);
    [ChatCommand("zone_list")]
    private void cmdChatZoneList(BasePlayer player, string command, string[] args);
    [ChatCommand("zone")]
    private void cmdChatZone(BasePlayer player, string command, string[] args);
    [ChatCommand("zone_flags")]
    private void cmdChatZoneFlags(BasePlayer player, string command, string[] args);
    [ChatCommand("zone_player")]
    private void cmdChatZonePlayer(BasePlayer player, string command, string[] args);
    private RaycastHit[] m_RaycastBuffer;
    [ChatCommand("zone_entity")]
    private void cmdChatZoneEntity(BasePlayer player, string command, string[] args);
    [ConsoleCommand("zone")]
    private void ccmdZone(ConsoleSystem.Arg arg);
    const string ZMUI;
    public static class UI
    {
        public static CuiElementContainer Container(string panel, string color, string min, string max, bool useCursor, string parent);
        public static void Panel(CuiElementContainer container, string panel, string color, string min, string max, bool cursor);
        public static void Label(CuiElementContainer container, string panel, string text, int size, string min, string max, TextAnchor align);
        public static void Button(CuiElementContainer container, string panel, string color, string text, int size, string min, string max, string command, TextAnchor align);
        public static string Color(string hexColor, float alpha);
    }

    private const string COLOR1;
    private const string COLOR2;
    private const string COLOR3;
    private void OpenFlagEditor(BasePlayer player, string zoneId);
    private Vector4 GetButtonPosition(int i);
    private int ColumnNumber(int max, int count);
    [ConsoleCommand("zmui.editflag")]
    private void ccmdEditFlag(ConsoleSystem.Arg arg);
    private static ConfigData Configuration;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Autolight Options")]
        public AutoLightOptions AutoLights { get; set; }
        [JsonProperty(PropertyName = "Notification Options")]
        public NotificationOptions Notifications { get; set; }
        [JsonProperty(PropertyName = "NPC players can deal player damage in zones with PvpGod flag")]
        public bool NPCHurtPvpGod { get; set; }
        [JsonProperty(PropertyName = "Allow decay damage in zones with Undestr flag")]
        public bool DecayDamageUndestr { get; set; }
        public class AutoLightOptions
        {
            [JsonProperty(PropertyName = "Time to turn lights on")]
            public float OnTime { get; set; }
            [JsonProperty(PropertyName = "Time to turn lights off")]
            public float OffTime { get; set; }
            [JsonProperty(PropertyName = "Lights require fuel to activate automatically")]
            public bool RequiresFuel { get; set; }
        }

        public class NotificationOptions
        {
            [JsonProperty(PropertyName = "Display notifications via PopupNotifications")]
            public bool Popups { get; set; }
            [JsonProperty(PropertyName = "Chat prefix")]
            public string Prefix { get; set; }
            [JsonProperty(PropertyName = "Chat color (hex)")]
            public string Color { get; set; }
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
        public HashSet<Zone.Definition> definitions;
    }

    public class EntityZones
    {
        public ZoneFlags Flags { get; set; }
        public HashSet<Zone> Zones { get; set; }
        public EntityZones();
        public void AddFlags(ZoneFlags zoneFlags);
        public void RemoveFlags(ZoneFlags zoneFlags);
        public bool HasFlag(int flag);
        public void UpdateFlags();
        public bool EnterZone(Zone zone);
        public bool LeaveZone(Zone zone);
        public bool IsInZone(Zone zone);
        public bool IsInZone(string zoneId);
        public bool ShouldRemove();
        public int Count { get; set; }
    }

    private class Vector3Converter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

    private class ZoneFlagsConverter : JsonConverter
    {
        private const string SEPERATOR;
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

    private string Message(string key, string playerId);
    private readonly Dictionary<string, string> Messages;
}

private class UpdateBehaviour : MonoBehaviour
{
    private readonly System.Diagnostics.Stopwatch sw;
    private readonly Queue<BasePlayer> playerUpdateQueue;
    private const float MAX_MS;
    private void OnDestroy();
    public void QueueUpdate(BasePlayer player);
    public void Reset();
    private void Update();
}

public class Zone : MonoBehaviour
{
    public Definition definition;
    public ZoneFlags disabledFlags;
    public Zone parent;
    public List<BasePlayer> players;
    public List<BaseEntity> entities;
    private List<IOEntity> ioEntities;
    public List<ulong> keepInList;
    public List<ulong> whitelist;
    public Hash<ulong, EntityZones> entityZones;
    private Rigidbody rigidbody;
    public Collider collider;
    public Bounds colliderBounds;
    private ChildSphereTrigger<TriggerRadiation> radiation;
    private ChildSphereTrigger<TriggerComfort> comfort;
    private ChildSphereTrigger<TriggerTemperature> temperature;
    private ChildSphereTrigger<TriggerSafeZone> safeZone;
    private readonly Hash<BaseVehicle, float> lastReversedTimes;
    private int creationFrame;
    private bool isTogglingLights;
    private void Awake();
    private void OnDestroy();
    private void EmptyZone();
    public void InitializeZone(Definition definition);
    public void FindZoneParent();
    public void Reset();
    public void OnZoneFlagsChanged();
    private void RegisterPermission();
    private void InitializeCollider();
    private void InitializeRadiation();
    private void InitializeComfort();
    private void InitializeTemperature();
    private void InitializeSafeZone();
    private void AddToTrigger(TriggerBase triggerBase, BasePlayer player);
    private void RemoveFromTrigger(TriggerBase triggerBase, BasePlayer player);
    private void AddPlayersToTriggers();
    private void RemovePlayersFromTriggers();
    private class ChildSphereTrigger
    {
        public GameObject Object { get; set; }
        public SphereCollider Collider { get; set; }
        public T Trigger { get; set; }
        public ChildSphereTrigger(GameObject parent, string name);
        public void Destroy();
    }

    private bool isLightsOn;
    private void InitializeAutoLights();
    private void CheckAlwaysLights();
    private void CheckLights();
    private IEnumerator ToggleLights(bool active);
    private bool ToggleLight(BaseEntity baseEntity, bool active, bool requiresFuel);
    private void OnTriggerEnter(Collider col);
    private void OnTriggerExit(Collider col);
    public void OnPlayerEnterZone(BasePlayer player);
    public void OnPlayerExitZone(BasePlayer player);
    public void OnEntityEnterZone(BaseEntity baseEntity);
    public void OnEntityExitZone(BaseEntity baseEntity, bool resetDecay, bool isDead);
    private void IOTick();
    public bool TryReverseVelocity(BaseEntity baseEntity);
    private bool CanReverseVelocity(BaseVehicle baseVehicle);
    public bool HasPermission(BasePlayer player);
    public bool CanLeaveZone(BasePlayer player);
    public bool CanEnterZone(BasePlayer player);
    public void AddFlag(int flag);
    public void RemoveFlag(int flag);
    public bool HasFlag(int flag);
    public bool HasDisabledFlag(int flag);
    public void AddDisabledFlag(int flag);
    public void RemoveDisabledFlag(int flag);
    public class Definition
    {
        public string Name { get; set; }
        public float Radius { get; set; }
        public float Radiation { get; set; }
        public float Comfort { get; set; }
        public float Temperature { get; set; }
        public bool SafeZone { get; set; }
        public Vector3 Location { get; set; }
        public Vector3 Size { get; set; }
        public Vector3 Rotation { get; set; }
        public string Id { get; set; }
        public string ParentID { get; set; }
        public string EnterMessage { get; set; }
        public string LeaveMessage { get; set; }
        public string Permission { get; set; }
        public string EjectSpawns { get; set; }
        public bool Enabled { get; set; }
        public ZoneFlags Flags { get; set; }
        public Definition();
        public Definition(Vector3 position);
    }

}

private class ChildSphereTrigger
{
    public GameObject Object { get; set; }
    public SphereCollider Collider { get; set; }
    public T Trigger { get; set; }
    public ChildSphereTrigger(GameObject parent, string name);
    public void Destroy();
}

public class Definition
{
    public string Name { get; set; }
    public float Radius { get; set; }
    public float Radiation { get; set; }
    public float Comfort { get; set; }
    public float Temperature { get; set; }
    public bool SafeZone { get; set; }
    public Vector3 Location { get; set; }
    public Vector3 Size { get; set; }
    public Vector3 Rotation { get; set; }
    public string Id { get; set; }
    public string ParentID { get; set; }
    public string EnterMessage { get; set; }
    public string LeaveMessage { get; set; }
    public string Permission { get; set; }
    public string EjectSpawns { get; set; }
    public bool Enabled { get; set; }
    public ZoneFlags Flags { get; set; }
    public Definition();
    public Definition(Vector3 position);
}

public class ZoneFlags
{
    public static readonly int AutoLights;
    public static readonly int AlwaysLights;
    public static readonly int NoKits;
    public static readonly int NoTrade;
    public static readonly int NoShop;
    public static readonly int NoTp;
    public static readonly int NoRemove;
    public static readonly int NoPoison;
    public static readonly int NoStarvation;
    public static readonly int NoThirst;
    public static readonly int NoRadiation;
    public static readonly int NoDrown;
    public static readonly int NoBleed;
    public static readonly int NoWounded;
    public static readonly int Eject;
    public static readonly int EjectSleepers;
    public static readonly int PvpGod;
    public static readonly int PveGod;
    public static readonly int SleepGod;
    public static readonly int NoPve;
    public static readonly int NoFallDamage;
    public static readonly int UnDestr;
    public static readonly int NoBuildingDamage;
    public static readonly int NoDecay;
    public static readonly int NoBuild;
    public static readonly int NoStability;
    public static readonly int NoUpgrade;
    public static readonly int NoSprays;
    public static readonly int NoDeploy;
    public static readonly int PoweredSwitches;
    public static readonly int LootSelf;
    public static readonly int NoPlayerLoot;
    public static readonly int NoNPCLoot;
    public static readonly int NoBoxLoot;
    public static readonly int NoPickup;
    public static readonly int NoCollect;
    public static readonly int NoDrop;
    public static readonly int NoLootSpawns;
    public static readonly int NoEntityPickup;
    public static readonly int NoGather;
    public static readonly int NoHeliTargeting;
    public static readonly int NoTurretTargeting;
    public static readonly int NoAPCTargeting;
    public static readonly int NoNPCTargeting;
    public static readonly int NpcFreeze;
    public static readonly int NoNPCSpawns;
    public static readonly int KeepVehiclesIn;
    public static readonly int KeepVehiclesOut;
    public static readonly int NoVehicleMounting;
    public static readonly int NoVehicleDismounting;
    public static readonly int NoChat;
    public static readonly int NoVoice;
    public static readonly int NoCorpse;
    public static readonly int NoSuicide;
    public static readonly int KillSleepers;
    public static readonly int Kill;
    public static readonly int InfiniteTrapAmmo;
    public static readonly int NoCup;
    public static readonly int NoSignUpdates;
    public static readonly int NoOvenToggle;
    public static readonly int NoVending;
    public static readonly int NoStash;
    public static readonly int NoCraft;
    public static readonly int NoDoorAccess;
    public static readonly int Custom1;
    public static readonly int Custom2;
    public static readonly int Custom3;
    public static readonly int Custom4;
    public static readonly int Custom5;
    public static readonly Hash<string, int> NameToIndex;
    public static readonly Hash<int, string> IndexToName;
    private static readonly int Count;
    private readonly BitArray m_BitArray;
    static ZoneFlags();
    public static bool Find(string flagName, int index);
    public ZoneFlags();
    public bool HasFlag(int flag);
    public void AddFlag(int flag);
    public void RemoveFlag(int flag);
    public void SetFlags(int[] array);
    public void Clear();
    public bool HasFlag(string flagName);
    public void AddFlag(string flagName);
    public void RemoveFlag(string flagName);
    public void SetFlags(string[] array);
    public void AddFlags(ZoneFlags zoneFlags);
    public void RemoveFlags(ZoneFlags zoneFlags);
    public bool CompareTo(ZoneFlags zoneFlags);
    public override string ToString();
}

public static class UI
{
    public static CuiElementContainer Container(string panel, string color, string min, string max, bool useCursor, string parent);
    public static void Panel(CuiElementContainer container, string panel, string color, string min, string max, bool cursor);
    public static void Label(CuiElementContainer container, string panel, string text, int size, string min, string max, TextAnchor align);
    public static void Button(CuiElementContainer container, string panel, string color, string text, int size, string min, string max, string command, TextAnchor align);
    public static string Color(string hexColor, float alpha);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Autolight Options")]
    public AutoLightOptions AutoLights { get; set; }
    [JsonProperty(PropertyName = "Notification Options")]
    public NotificationOptions Notifications { get; set; }
    [JsonProperty(PropertyName = "NPC players can deal player damage in zones with PvpGod flag")]
    public bool NPCHurtPvpGod { get; set; }
    [JsonProperty(PropertyName = "Allow decay damage in zones with Undestr flag")]
    public bool DecayDamageUndestr { get; set; }
    public class AutoLightOptions
    {
        [JsonProperty(PropertyName = "Time to turn lights on")]
        public float OnTime { get; set; }
        [JsonProperty(PropertyName = "Time to turn lights off")]
        public float OffTime { get; set; }
        [JsonProperty(PropertyName = "Lights require fuel to activate automatically")]
        public bool RequiresFuel { get; set; }
    }

    public class NotificationOptions
    {
        [JsonProperty(PropertyName = "Display notifications via PopupNotifications")]
        public bool Popups { get; set; }
        [JsonProperty(PropertyName = "Chat prefix")]
        public string Prefix { get; set; }
        [JsonProperty(PropertyName = "Chat color (hex)")]
        public string Color { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class AutoLightOptions
{
    [JsonProperty(PropertyName = "Time to turn lights on")]
    public float OnTime { get; set; }
    [JsonProperty(PropertyName = "Time to turn lights off")]
    public float OffTime { get; set; }
    [JsonProperty(PropertyName = "Lights require fuel to activate automatically")]
    public bool RequiresFuel { get; set; }
}

public class NotificationOptions
{
    [JsonProperty(PropertyName = "Display notifications via PopupNotifications")]
    public bool Popups { get; set; }
    [JsonProperty(PropertyName = "Chat prefix")]
    public string Prefix { get; set; }
    [JsonProperty(PropertyName = "Chat color (hex)")]
    public string Color { get; set; }
}

private class StoredData
{
    public HashSet<Zone.Definition> definitions;
}

public class EntityZones
{
    public ZoneFlags Flags { get; set; }
    public HashSet<Zone> Zones { get; set; }
    public EntityZones();
    public void AddFlags(ZoneFlags zoneFlags);
    public void RemoveFlags(ZoneFlags zoneFlags);
    public bool HasFlag(int flag);
    public void UpdateFlags();
    public bool EnterZone(Zone zone);
    public bool LeaveZone(Zone zone);
    public bool IsInZone(Zone zone);
    public bool IsInZone(string zoneId);
    public bool ShouldRemove();
    public int Count { get; set; }
}

private class Vector3Converter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}

private class ZoneFlagsConverter : JsonConverter
{
    private const string SEPERATOR;
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}


```

---

## ZoneManagerAutoZones by FastBurst - Adds zones and domes to monuments automatically

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using Oxide.Core.Plugins;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Zone Manager Auto Zones", "FastBurst", "1.4.0")]
[Description("Adds zones and domes to monuments automatically.")]
public class ZoneManagerAutoZones : RustPlugin
{
    [PluginReference]
     Plugin ZoneManager;
     Plugin ZoneDomes;
     Plugin TruePVE;
     Plugin NextGenPVE;
     BasePlayer player;
     void OnServerInitialized();
    private void PopulateZoneLocations();
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "ZonesDome Option")]
        public Options ZoneOption { get; set; }
        public class Options
        {
            [JsonProperty(PropertyName = "Use Zone Domes Spheres over Zones")]
            public bool UseZoneDomes { get; set; }
            [JsonProperty(PropertyName = "Enable TruePVE to allow Rule Sets")]
            public bool UseTruePVE { get; set; }
            [JsonProperty(PropertyName = "Enable NextGenPVE to allow Rule Sets")]
            public bool UseNextGenPVE { get; set; }
        }

        [JsonProperty(PropertyName = "Zone Location Options")]
        public LocationOptions Location { get; set; }
        public class LocationOptions
        {
            [JsonProperty(PropertyName = "Monument Options")]
            public Monument Monuments { get; set; }
            public class Monument
            {
                [JsonProperty(PropertyName = "Airfield")]
                public Options Airfield { get; set; }
                [JsonProperty(PropertyName = "Small Harbour")]
                public Options HarborSmall { get; set; }
                [JsonProperty(PropertyName = "Large Harbour")]
                public Options HarborLarge { get; set; }
                [JsonProperty(PropertyName = "Launch Site")]
                public Options LaunchSite { get; set; }
                [JsonProperty(PropertyName = "Large Oil Rig")]
                public Options LargeOilRig { get; set; }
                [JsonProperty(PropertyName = "Small Oil Rig")]
                public Options SmallOilRig { get; set; }
                [JsonProperty(PropertyName = "Power Plant")]
                public Options Powerplant { get; set; }
                [JsonProperty(PropertyName = "Junk Yard")]
                public Options Junkyard { get; set; }
                [JsonProperty(PropertyName = "Military Tunnels")]
                public Options MilitaryTunnels { get; set; }
                [JsonProperty(PropertyName = "Train Yard")]
                public Options TrainYard { get; set; }
                [JsonProperty(PropertyName = "Water Treatment Plant")]
                public Options WaterTreatment { get; set; }
                [JsonProperty(PropertyName = "Giant Excavator Pit")]
                public Options Excavator { get; set; }
                [JsonProperty(PropertyName = "Satellite Dish")]
                public Options SatelliteDish { get; set; }
                [JsonProperty(PropertyName = "Sewer Branch")]
                public Options SewerBranch { get; set; }
                [JsonProperty(PropertyName = "The Dome")]
                public Options Dome { get; set; }
                [JsonProperty(PropertyName = "Oxum's Gas Station #1")]
                public Options GasStation1 { get; set; }
                [JsonProperty(PropertyName = "Oxum's Gas Station #2")]
                public Options GasStation2 { get; set; }
                [JsonProperty(PropertyName = "Oxum's Gas Station #3")]
                public Options GasStation3 { get; set; }
                [JsonProperty(PropertyName = "Sulfur Quarry")]
                public Options QuarryA { get; set; }
                [JsonProperty(PropertyName = "Stone Quarry")]
                public Options QuarryB { get; set; }
                [JsonProperty(PropertyName = "HQM Quarry")]
                public Options QuarryC { get; set; }
                [JsonProperty(PropertyName = "Wild Swamp 1")]
                public Options SwampA { get; set; }
                [JsonProperty(PropertyName = "Wild Swamp 2")]
                public Options SwampB { get; set; }
                [JsonProperty(PropertyName = "Abandoned Cabins")]
                public Options SwampC { get; set; }
                [JsonProperty(PropertyName = "Light House #1")]
                public Options LightHouse1 { get; set; }
                [JsonProperty(PropertyName = "Light House #2")]
                public Options LightHouse2 { get; set; }
                [JsonProperty(PropertyName = "Abandoned Supermarket #1")]
                public Options Supermarket1 { get; set; }
                [JsonProperty(PropertyName = "Abandoned Supermarket #2")]
                public Options Supermarket2 { get; set; }
                [JsonProperty(PropertyName = "Abandoned Supermarket #3")]
                public Options Supermarket3 { get; set; }
                [JsonProperty(PropertyName = "Mining Outpost #1")]
                public Options MiningOutpost1 { get; set; }
                [JsonProperty(PropertyName = "Mining Outpost #2")]
                public Options MiningOutpost2 { get; set; }
                [JsonProperty(PropertyName = "Mining Outpost #3")]
                public Options MiningOutpost3 { get; set; }
                [JsonProperty(PropertyName = "Fishing Village #1")]
                public Options FishingVillage1 { get; set; }
                [JsonProperty(PropertyName = "Fishing Village #2")]
                public Options FishingVillage2 { get; set; }
                [JsonProperty(PropertyName = "Fishing Village #3")]
                public Options FishingVillage3 { get; set; }
                [JsonProperty(PropertyName = "Outpost")]
                public Options Outpost { get; set; }
                [JsonProperty(PropertyName = "Bandit Camp")]
                public Options BanditCamp { get; set; }
                [JsonProperty(PropertyName = "Desert Base 1")]
                public Options DesertBaseA { get; set; }
                [JsonProperty(PropertyName = "Desert Base 2")]
                public Options DesertBaseB { get; set; }
                [JsonProperty(PropertyName = "Desert Base 3")]
                public Options DesertBaseC { get; set; }
                [JsonProperty(PropertyName = "Desert Base 4")]
                public Options DesertBaseD { get; set; }
                [JsonProperty(PropertyName = "Stables")]
                public Options StableA { get; set; }
                [JsonProperty(PropertyName = "Ranch")]
                public Options StableB { get; set; }
                [JsonProperty(PropertyName = "Artic Research Base")]
                public Options ArticA { get; set; }
                [JsonProperty(PropertyName = "Nuclear Missile Silo")]
                public Options missileSilo { get; set; }
                [JsonProperty(PropertyName = "Ferry Terminal")]
                public Options ferryTerminal { get; set; }
                [JsonProperty(PropertyName = "Rad Town")]
                public Options radTown { get; set; }
                public class Options
                {
                    public bool Enabled { get; set; }
                    [JsonProperty(PropertyName = "Radius")]
                    public int Radius { get; set; }
                    [JsonProperty(PropertyName = "Enter Zone Message")]
                    public string EnterMessage { get; set; }
                    [JsonProperty(PropertyName = "Leave Zone Message")]
                    public string LeaveMessage { get; set; }
                    [JsonProperty(PropertyName = "Zone Flags")]
                    public string[] ZoneFlags { get; set; }
                    [JsonProperty(PropertyName = "TruePVE RuleSet to use if TruePVE is enabled")]
                    public string TruePVERules { get; set; }
                    [JsonProperty(PropertyName = "NextGenPVE RuleSet to use if NextGenPVE is enabled")]
                    public string NextGenPVERules { get; set; }
                    [JsonIgnore]
                    public List<MonumentInfo> Monument { get; set; }
                }

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

 class ConfigData
{
    [JsonProperty(PropertyName = "ZonesDome Option")]
    public Options ZoneOption { get; set; }
    public class Options
    {
        [JsonProperty(PropertyName = "Use Zone Domes Spheres over Zones")]
        public bool UseZoneDomes { get; set; }
        [JsonProperty(PropertyName = "Enable TruePVE to allow Rule Sets")]
        public bool UseTruePVE { get; set; }
        [JsonProperty(PropertyName = "Enable NextGenPVE to allow Rule Sets")]
        public bool UseNextGenPVE { get; set; }
    }

    [JsonProperty(PropertyName = "Zone Location Options")]
    public LocationOptions Location { get; set; }
    public class LocationOptions
    {
        [JsonProperty(PropertyName = "Monument Options")]
        public Monument Monuments { get; set; }
        public class Monument
        {
            [JsonProperty(PropertyName = "Airfield")]
            public Options Airfield { get; set; }
            [JsonProperty(PropertyName = "Small Harbour")]
            public Options HarborSmall { get; set; }
            [JsonProperty(PropertyName = "Large Harbour")]
            public Options HarborLarge { get; set; }
            [JsonProperty(PropertyName = "Launch Site")]
            public Options LaunchSite { get; set; }
            [JsonProperty(PropertyName = "Large Oil Rig")]
            public Options LargeOilRig { get; set; }
            [JsonProperty(PropertyName = "Small Oil Rig")]
            public Options SmallOilRig { get; set; }
            [JsonProperty(PropertyName = "Power Plant")]
            public Options Powerplant { get; set; }
            [JsonProperty(PropertyName = "Junk Yard")]
            public Options Junkyard { get; set; }
            [JsonProperty(PropertyName = "Military Tunnels")]
            public Options MilitaryTunnels { get; set; }
            [JsonProperty(PropertyName = "Train Yard")]
            public Options TrainYard { get; set; }
            [JsonProperty(PropertyName = "Water Treatment Plant")]
            public Options WaterTreatment { get; set; }
            [JsonProperty(PropertyName = "Giant Excavator Pit")]
            public Options Excavator { get; set; }
            [JsonProperty(PropertyName = "Satellite Dish")]
            public Options SatelliteDish { get; set; }
            [JsonProperty(PropertyName = "Sewer Branch")]
            public Options SewerBranch { get; set; }
            [JsonProperty(PropertyName = "The Dome")]
            public Options Dome { get; set; }
            [JsonProperty(PropertyName = "Oxum's Gas Station #1")]
            public Options GasStation1 { get; set; }
            [JsonProperty(PropertyName = "Oxum's Gas Station #2")]
            public Options GasStation2 { get; set; }
            [JsonProperty(PropertyName = "Oxum's Gas Station #3")]
            public Options GasStation3 { get; set; }
            [JsonProperty(PropertyName = "Sulfur Quarry")]
            public Options QuarryA { get; set; }
            [JsonProperty(PropertyName = "Stone Quarry")]
            public Options QuarryB { get; set; }
            [JsonProperty(PropertyName = "HQM Quarry")]
            public Options QuarryC { get; set; }
            [JsonProperty(PropertyName = "Wild Swamp 1")]
            public Options SwampA { get; set; }
            [JsonProperty(PropertyName = "Wild Swamp 2")]
            public Options SwampB { get; set; }
            [JsonProperty(PropertyName = "Abandoned Cabins")]
            public Options SwampC { get; set; }
            [JsonProperty(PropertyName = "Light House #1")]
            public Options LightHouse1 { get; set; }
            [JsonProperty(PropertyName = "Light House #2")]
            public Options LightHouse2 { get; set; }
            [JsonProperty(PropertyName = "Abandoned Supermarket #1")]
            public Options Supermarket1 { get; set; }
            [JsonProperty(PropertyName = "Abandoned Supermarket #2")]
            public Options Supermarket2 { get; set; }
            [JsonProperty(PropertyName = "Abandoned Supermarket #3")]
            public Options Supermarket3 { get; set; }
            [JsonProperty(PropertyName = "Mining Outpost #1")]
            public Options MiningOutpost1 { get; set; }
            [JsonProperty(PropertyName = "Mining Outpost #2")]
            public Options MiningOutpost2 { get; set; }
            [JsonProperty(PropertyName = "Mining Outpost #3")]
            public Options MiningOutpost3 { get; set; }
            [JsonProperty(PropertyName = "Fishing Village #1")]
            public Options FishingVillage1 { get; set; }
            [JsonProperty(PropertyName = "Fishing Village #2")]
            public Options FishingVillage2 { get; set; }
            [JsonProperty(PropertyName = "Fishing Village #3")]
            public Options FishingVillage3 { get; set; }
            [JsonProperty(PropertyName = "Outpost")]
            public Options Outpost { get; set; }
            [JsonProperty(PropertyName = "Bandit Camp")]
            public Options BanditCamp { get; set; }
            [JsonProperty(PropertyName = "Desert Base 1")]
            public Options DesertBaseA { get; set; }
            [JsonProperty(PropertyName = "Desert Base 2")]
            public Options DesertBaseB { get; set; }
            [JsonProperty(PropertyName = "Desert Base 3")]
            public Options DesertBaseC { get; set; }
            [JsonProperty(PropertyName = "Desert Base 4")]
            public Options DesertBaseD { get; set; }
            [JsonProperty(PropertyName = "Stables")]
            public Options StableA { get; set; }
            [JsonProperty(PropertyName = "Ranch")]
            public Options StableB { get; set; }
            [JsonProperty(PropertyName = "Artic Research Base")]
            public Options ArticA { get; set; }
            [JsonProperty(PropertyName = "Nuclear Missile Silo")]
            public Options missileSilo { get; set; }
            [JsonProperty(PropertyName = "Ferry Terminal")]
            public Options ferryTerminal { get; set; }
            [JsonProperty(PropertyName = "Rad Town")]
            public Options radTown { get; set; }
            public class Options
            {
                public bool Enabled { get; set; }
                [JsonProperty(PropertyName = "Radius")]
                public int Radius { get; set; }
                [JsonProperty(PropertyName = "Enter Zone Message")]
                public string EnterMessage { get; set; }
                [JsonProperty(PropertyName = "Leave Zone Message")]
                public string LeaveMessage { get; set; }
                [JsonProperty(PropertyName = "Zone Flags")]
                public string[] ZoneFlags { get; set; }
                [JsonProperty(PropertyName = "TruePVE RuleSet to use if TruePVE is enabled")]
                public string TruePVERules { get; set; }
                [JsonProperty(PropertyName = "NextGenPVE RuleSet to use if NextGenPVE is enabled")]
                public string NextGenPVERules { get; set; }
                [JsonIgnore]
                public List<MonumentInfo> Monument { get; set; }
            }

        }

    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class Options
{
    [JsonProperty(PropertyName = "Use Zone Domes Spheres over Zones")]
    public bool UseZoneDomes { get; set; }
    [JsonProperty(PropertyName = "Enable TruePVE to allow Rule Sets")]
    public bool UseTruePVE { get; set; }
    [JsonProperty(PropertyName = "Enable NextGenPVE to allow Rule Sets")]
    public bool UseNextGenPVE { get; set; }
}

public class LocationOptions
{
    [JsonProperty(PropertyName = "Monument Options")]
    public Monument Monuments { get; set; }
    public class Monument
    {
        [JsonProperty(PropertyName = "Airfield")]
        public Options Airfield { get; set; }
        [JsonProperty(PropertyName = "Small Harbour")]
        public Options HarborSmall { get; set; }
        [JsonProperty(PropertyName = "Large Harbour")]
        public Options HarborLarge { get; set; }
        [JsonProperty(PropertyName = "Launch Site")]
        public Options LaunchSite { get; set; }
        [JsonProperty(PropertyName = "Large Oil Rig")]
        public Options LargeOilRig { get; set; }
        [JsonProperty(PropertyName = "Small Oil Rig")]
        public Options SmallOilRig { get; set; }
        [JsonProperty(PropertyName = "Power Plant")]
        public Options Powerplant { get; set; }
        [JsonProperty(PropertyName = "Junk Yard")]
        public Options Junkyard { get; set; }
        [JsonProperty(PropertyName = "Military Tunnels")]
        public Options MilitaryTunnels { get; set; }
        [JsonProperty(PropertyName = "Train Yard")]
        public Options TrainYard { get; set; }
        [JsonProperty(PropertyName = "Water Treatment Plant")]
        public Options WaterTreatment { get; set; }
        [JsonProperty(PropertyName = "Giant Excavator Pit")]
        public Options Excavator { get; set; }
        [JsonProperty(PropertyName = "Satellite Dish")]
        public Options SatelliteDish { get; set; }
        [JsonProperty(PropertyName = "Sewer Branch")]
        public Options SewerBranch { get; set; }
        [JsonProperty(PropertyName = "The Dome")]
        public Options Dome { get; set; }
        [JsonProperty(PropertyName = "Oxum's Gas Station #1")]
        public Options GasStation1 { get; set; }
        [JsonProperty(PropertyName = "Oxum's Gas Station #2")]
        public Options GasStation2 { get; set; }
        [JsonProperty(PropertyName = "Oxum's Gas Station #3")]
        public Options GasStation3 { get; set; }
        [JsonProperty(PropertyName = "Sulfur Quarry")]
        public Options QuarryA { get; set; }
        [JsonProperty(PropertyName = "Stone Quarry")]
        public Options QuarryB { get; set; }
        [JsonProperty(PropertyName = "HQM Quarry")]
        public Options QuarryC { get; set; }
        [JsonProperty(PropertyName = "Wild Swamp 1")]
        public Options SwampA { get; set; }
        [JsonProperty(PropertyName = "Wild Swamp 2")]
        public Options SwampB { get; set; }
        [JsonProperty(PropertyName = "Abandoned Cabins")]
        public Options SwampC { get; set; }
        [JsonProperty(PropertyName = "Light House #1")]
        public Options LightHouse1 { get; set; }
        [JsonProperty(PropertyName = "Light House #2")]
        public Options LightHouse2 { get; set; }
        [JsonProperty(PropertyName = "Abandoned Supermarket #1")]
        public Options Supermarket1 { get; set; }
        [JsonProperty(PropertyName = "Abandoned Supermarket #2")]
        public Options Supermarket2 { get; set; }
        [JsonProperty(PropertyName = "Abandoned Supermarket #3")]
        public Options Supermarket3 { get; set; }
        [JsonProperty(PropertyName = "Mining Outpost #1")]
        public Options MiningOutpost1 { get; set; }
        [JsonProperty(PropertyName = "Mining Outpost #2")]
        public Options MiningOutpost2 { get; set; }
        [JsonProperty(PropertyName = "Mining Outpost #3")]
        public Options MiningOutpost3 { get; set; }
        [JsonProperty(PropertyName = "Fishing Village #1")]
        public Options FishingVillage1 { get; set; }
        [JsonProperty(PropertyName = "Fishing Village #2")]
        public Options FishingVillage2 { get; set; }
        [JsonProperty(PropertyName = "Fishing Village #3")]
        public Options FishingVillage3 { get; set; }
        [JsonProperty(PropertyName = "Outpost")]
        public Options Outpost { get; set; }
        [JsonProperty(PropertyName = "Bandit Camp")]
        public Options BanditCamp { get; set; }
        [JsonProperty(PropertyName = "Desert Base 1")]
        public Options DesertBaseA { get; set; }
        [JsonProperty(PropertyName = "Desert Base 2")]
        public Options DesertBaseB { get; set; }
        [JsonProperty(PropertyName = "Desert Base 3")]
        public Options DesertBaseC { get; set; }
        [JsonProperty(PropertyName = "Desert Base 4")]
        public Options DesertBaseD { get; set; }
        [JsonProperty(PropertyName = "Stables")]
        public Options StableA { get; set; }
        [JsonProperty(PropertyName = "Ranch")]
        public Options StableB { get; set; }
        [JsonProperty(PropertyName = "Artic Research Base")]
        public Options ArticA { get; set; }
        [JsonProperty(PropertyName = "Nuclear Missile Silo")]
        public Options missileSilo { get; set; }
        [JsonProperty(PropertyName = "Ferry Terminal")]
        public Options ferryTerminal { get; set; }
        [JsonProperty(PropertyName = "Rad Town")]
        public Options radTown { get; set; }
        public class Options
        {
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Radius")]
            public int Radius { get; set; }
            [JsonProperty(PropertyName = "Enter Zone Message")]
            public string EnterMessage { get; set; }
            [JsonProperty(PropertyName = "Leave Zone Message")]
            public string LeaveMessage { get; set; }
            [JsonProperty(PropertyName = "Zone Flags")]
            public string[] ZoneFlags { get; set; }
            [JsonProperty(PropertyName = "TruePVE RuleSet to use if TruePVE is enabled")]
            public string TruePVERules { get; set; }
            [JsonProperty(PropertyName = "NextGenPVE RuleSet to use if NextGenPVE is enabled")]
            public string NextGenPVERules { get; set; }
            [JsonIgnore]
            public List<MonumentInfo> Monument { get; set; }
        }

    }

}

public class Monument
{
    [JsonProperty(PropertyName = "Airfield")]
    public Options Airfield { get; set; }
    [JsonProperty(PropertyName = "Small Harbour")]
    public Options HarborSmall { get; set; }
    [JsonProperty(PropertyName = "Large Harbour")]
    public Options HarborLarge { get; set; }
    [JsonProperty(PropertyName = "Launch Site")]
    public Options LaunchSite { get; set; }
    [JsonProperty(PropertyName = "Large Oil Rig")]
    public Options LargeOilRig { get; set; }
    [JsonProperty(PropertyName = "Small Oil Rig")]
    public Options SmallOilRig { get; set; }
    [JsonProperty(PropertyName = "Power Plant")]
    public Options Powerplant { get; set; }
    [JsonProperty(PropertyName = "Junk Yard")]
    public Options Junkyard { get; set; }
    [JsonProperty(PropertyName = "Military Tunnels")]
    public Options MilitaryTunnels { get; set; }
    [JsonProperty(PropertyName = "Train Yard")]
    public Options TrainYard { get; set; }
    [JsonProperty(PropertyName = "Water Treatment Plant")]
    public Options WaterTreatment { get; set; }
    [JsonProperty(PropertyName = "Giant Excavator Pit")]
    public Options Excavator { get; set; }
    [JsonProperty(PropertyName = "Satellite Dish")]
    public Options SatelliteDish { get; set; }
    [JsonProperty(PropertyName = "Sewer Branch")]
    public Options SewerBranch { get; set; }
    [JsonProperty(PropertyName = "The Dome")]
    public Options Dome { get; set; }
    [JsonProperty(PropertyName = "Oxum's Gas Station #1")]
    public Options GasStation1 { get; set; }
    [JsonProperty(PropertyName = "Oxum's Gas Station #2")]
    public Options GasStation2 { get; set; }
    [JsonProperty(PropertyName = "Oxum's Gas Station #3")]
    public Options GasStation3 { get; set; }
    [JsonProperty(PropertyName = "Sulfur Quarry")]
    public Options QuarryA { get; set; }
    [JsonProperty(PropertyName = "Stone Quarry")]
    public Options QuarryB { get; set; }
    [JsonProperty(PropertyName = "HQM Quarry")]
    public Options QuarryC { get; set; }
    [JsonProperty(PropertyName = "Wild Swamp 1")]
    public Options SwampA { get; set; }
    [JsonProperty(PropertyName = "Wild Swamp 2")]
    public Options SwampB { get; set; }
    [JsonProperty(PropertyName = "Abandoned Cabins")]
    public Options SwampC { get; set; }
    [JsonProperty(PropertyName = "Light House #1")]
    public Options LightHouse1 { get; set; }
    [JsonProperty(PropertyName = "Light House #2")]
    public Options LightHouse2 { get; set; }
    [JsonProperty(PropertyName = "Abandoned Supermarket #1")]
    public Options Supermarket1 { get; set; }
    [JsonProperty(PropertyName = "Abandoned Supermarket #2")]
    public Options Supermarket2 { get; set; }
    [JsonProperty(PropertyName = "Abandoned Supermarket #3")]
    public Options Supermarket3 { get; set; }
    [JsonProperty(PropertyName = "Mining Outpost #1")]
    public Options MiningOutpost1 { get; set; }
    [JsonProperty(PropertyName = "Mining Outpost #2")]
    public Options MiningOutpost2 { get; set; }
    [JsonProperty(PropertyName = "Mining Outpost #3")]
    public Options MiningOutpost3 { get; set; }
    [JsonProperty(PropertyName = "Fishing Village #1")]
    public Options FishingVillage1 { get; set; }
    [JsonProperty(PropertyName = "Fishing Village #2")]
    public Options FishingVillage2 { get; set; }
    [JsonProperty(PropertyName = "Fishing Village #3")]
    public Options FishingVillage3 { get; set; }
    [JsonProperty(PropertyName = "Outpost")]
    public Options Outpost { get; set; }
    [JsonProperty(PropertyName = "Bandit Camp")]
    public Options BanditCamp { get; set; }
    [JsonProperty(PropertyName = "Desert Base 1")]
    public Options DesertBaseA { get; set; }
    [JsonProperty(PropertyName = "Desert Base 2")]
    public Options DesertBaseB { get; set; }
    [JsonProperty(PropertyName = "Desert Base 3")]
    public Options DesertBaseC { get; set; }
    [JsonProperty(PropertyName = "Desert Base 4")]
    public Options DesertBaseD { get; set; }
    [JsonProperty(PropertyName = "Stables")]
    public Options StableA { get; set; }
    [JsonProperty(PropertyName = "Ranch")]
    public Options StableB { get; set; }
    [JsonProperty(PropertyName = "Artic Research Base")]
    public Options ArticA { get; set; }
    [JsonProperty(PropertyName = "Nuclear Missile Silo")]
    public Options missileSilo { get; set; }
    [JsonProperty(PropertyName = "Ferry Terminal")]
    public Options ferryTerminal { get; set; }
    [JsonProperty(PropertyName = "Rad Town")]
    public Options radTown { get; set; }
    public class Options
    {
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Radius")]
        public int Radius { get; set; }
        [JsonProperty(PropertyName = "Enter Zone Message")]
        public string EnterMessage { get; set; }
        [JsonProperty(PropertyName = "Leave Zone Message")]
        public string LeaveMessage { get; set; }
        [JsonProperty(PropertyName = "Zone Flags")]
        public string[] ZoneFlags { get; set; }
        [JsonProperty(PropertyName = "TruePVE RuleSet to use if TruePVE is enabled")]
        public string TruePVERules { get; set; }
        [JsonProperty(PropertyName = "NextGenPVE RuleSet to use if NextGenPVE is enabled")]
        public string NextGenPVERules { get; set; }
        [JsonIgnore]
        public List<MonumentInfo> Monument { get; set; }
    }

}

public class Options
{
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Radius")]
    public int Radius { get; set; }
    [JsonProperty(PropertyName = "Enter Zone Message")]
    public string EnterMessage { get; set; }
    [JsonProperty(PropertyName = "Leave Zone Message")]
    public string LeaveMessage { get; set; }
    [JsonProperty(PropertyName = "Zone Flags")]
    public string[] ZoneFlags { get; set; }
    [JsonProperty(PropertyName = "TruePVE RuleSet to use if TruePVE is enabled")]
    public string TruePVERules { get; set; }
    [JsonProperty(PropertyName = "NextGenPVE RuleSet to use if NextGenPVE is enabled")]
    public string NextGenPVERules { get; set; }
    [JsonIgnore]
    public List<MonumentInfo> Monument { get; set; }
}


```

---

## ZoneManagerTime by misticos - Set specific time for your zones

```csharp
using System;
using System.Collections.Generic;
using Network;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Zone Manager Time", "misticos", "1.0.1")]
[Description("Set specific time for your zones")]
 class ZoneManagerTime : CovalencePlugin
{
    [PluginReference]
    private Plugin ZoneManager;
    private Dictionary<string, string> _playerZones;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Zone ID Time", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, TimeSpan> ZoneTime;
        [JsonProperty(PropertyName = "Update Frequency")]
        public float UpdateFrequency;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    private void Unload();
    private void OnEnterZone(string zoneId, BasePlayer player);
    private void OnExitZone(string zoneId, BasePlayer player);
    private class DateController : SingletonComponent<DateController>
    {
        public ZoneManagerTime PluginInstance;
        private EnvSync _timeEntity;
        private void Start();
        protected override void OnDestroy();
        private void DoUpdate();
    }

}

private class Configuration
{
    [JsonProperty(PropertyName = "Zone ID Time", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, TimeSpan> ZoneTime;
    [JsonProperty(PropertyName = "Update Frequency")]
    public float UpdateFrequency;
}

private class DateController : SingletonComponent<DateController>
{
    public ZoneManagerTime PluginInstance;
    private EnvSync _timeEntity;
    private void Start();
    protected override void OnDestroy();
    private void DoUpdate();
}


```

---

## ZonePerms by  - Allows players to be granted permissions when entering a zone

```csharp
using Oxide.Core;
using Oxide.Core.Plugins;
using System.Collections.Generic;

Oxide.Plugins
[Info("Zone Perms", "MisterPixie", "1.1.0")]
[Description("Grant players permissions when entering a zone.")]
 class ZonePerms : RustPlugin
{
    [PluginReference]
     Plugin ZoneManager;
    private Dictionary<ulong, ZonePermsData> _zonePermsData;
    private class ZonePermsData
    {
        public List<string> permission;
        public List<string> groups;
        public ZonePermsData();
    }

    private void SaveData();
    private void Unload();
    private void OnServerSave();
    private void Init();
    private void OnEnterZone(string ZoneID, BasePlayer player);
    private void OnExitZone(string ZoneID, BasePlayer player);
    private class ZoneAddons
    {
        public bool EnableZone;
        public OnEnter OnZoneEnter;
        public OnExit OnZoneExit;
    }

    private class OnEnter
    {
        public List<string> Permissions;
        public List<string> Groups;
    }

    private class OnExit
    {
        public List<string> Permissions;
        public List<string> Groups;
    }

    private ConfigData configData;
    private class ConfigData
    {
        public bool Enable;
        public Dictionary<string, ZoneAddons> Zones;
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void SaveConfig(ConfigData config);
}

private class ZonePermsData
{
    public List<string> permission;
    public List<string> groups;
    public ZonePermsData();
}

private class ZoneAddons
{
    public bool EnableZone;
    public OnEnter OnZoneEnter;
    public OnExit OnZoneExit;
}

private class OnEnter
{
    public List<string> Permissions;
    public List<string> Groups;
}

private class OnExit
{
    public List<string> Permissions;
    public List<string> Groups;
}

private class ConfigData
{
    public bool Enable;
    public Dictionary<string, ZoneAddons> Zones;
}


```

---

## ZonePVxInfo by Mabel - Shows a HUD on PvE/PvP name defined zones

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Zone PVx Info", "Mabel", "1.1.9")]
[Description("HUD on PVx name defined Zones")]
public class ZonePVxInfo : RustPlugin
{
    [PluginReference]
    private readonly Plugin ZoneManager;
    private readonly Plugin DynamicPVP;
    private readonly Plugin RaidableBases;
    private readonly Plugin AbandonedBases;
    private readonly Plugin TruePVE;
    private readonly Plugin DangerousTreasures;
    private const string UinameMain;
    private bool _pvpAll;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private string GetZoneName(string zoneId);
    private string[] GetPlayerZoneIDs(BasePlayer player);
    private float GetZoneRadius(string zoneId);
    private Vector3 GetZoneSize(string zoneId);
    private void OnEnterZone(string zoneId, BasePlayer player);
    private void OnExitZone(string zoneId, BasePlayer player);
    private void CheckPlayerZone(BasePlayer player, bool checkPVPDelay);
    private string GetSmallestZoneName(BasePlayer player);
    private void OnPlayerEnteredRaidableBase(BasePlayer player, Vector3 location, bool allowPVP);
    private void OnPlayerExitedRaidableBase(BasePlayer player, Vector3 location, bool allowPVP);
    private void OnRaidableBaseEnded(Vector3 raidPos, int mode, float loadingTime);
     void OnPlayerEnteredAbandonedBase(BasePlayer player, Vector3 eventPos, float radius, bool allowPVP);
     void OnPlayerExitedAbandonedBase(BasePlayer player);
     void OnAbandonedBaseEnded();
     void OnPlayerEnteredDangerousEvent(BasePlayer player, Vector3 containerPos, bool allowPVP);
     void OnPlayerExitedDangerousEvent(BasePlayer player);
     void OnPlayerEnterConvoy(BasePlayer player);
     void OnPlayerExitConvoy(BasePlayer player);
     void OnPlayerEnterArmoredTrain(BasePlayer player);
     void OnPlayerExitArmoredTrain(BasePlayer player);
     void OnPlayerEnterCaravan(BasePlayer player);
     void OnPlayerExitCaravan(BasePlayer player);
     void OnPlayerEnterSputnik(BasePlayer player);
     void OnPlayerExitSputnik(BasePlayer player);
     void OnPlayerEnterSpace(BasePlayer player);
     void OnPlayerExitSpace(BasePlayer player);
     void OnPlayerEnterShipwreck(BasePlayer player);
     void OnPlayerExitShipwreck(BasePlayer player);
     void OnPlayerEnterPowerPlantEvent(BasePlayer player);
     void OnPlayerExitPowerPlantEvent(BasePlayer player);
     void OnPlayerEnterFerryTerminalEvent(BasePlayer player);
     void OnPlayerExitFerryTerminalEvent(BasePlayer player);
     void OnPlayerEnterSupermarketEvent(BasePlayer player);
     void OnPlayerExitSupermarketEvent(BasePlayer player);
     void OnPlayerEnterJunkyardEvent(BasePlayer player);
     void OnPlayerExitJunkyardEvent(BasePlayer player);
     void OnPlayerEnterArcticBaseEvent(BasePlayer player);
     void OnPlayerExitArcticBaseEvent(BasePlayer player);
     void OnPlayerEnterGasStationEvent(BasePlayer player);
     void OnPlayerExitGasStationEvent(BasePlayer player);
     void OnPlayerEnterAirEvent(BasePlayer player);
     void OnPlayerExitAirEvent(BasePlayer player);
     void OnPlayerEnterHarborEvent(BasePlayer player);
     void OnPlayerExitHarborEvent(BasePlayer player);
     void OnPlayerEnterSatDishEvent(BasePlayer player);
     void OnPlayerExitSatDishEvent(BasePlayer player);
     void OnPlayerEnterWaterEvent(BasePlayer player);
     void OnPlayerExitWaterEvent(BasePlayer player);
     void OnPlayerEnterTriangulation(BasePlayer player);
     void OnPlayerExitTriangulation(BasePlayer player);
     void OnPlayerEnterDefendableHomes(BasePlayer player);
     void OnPlayerExitDefendableHomes(BasePlayer player);
    private void OnPlayerEnterPVPBubble(TrainEngine trainEngine, BasePlayer player);
    private void OnPlayerExitPVPBubble(TrainEngine trainEngine, BasePlayer player);
    private void OnTrainEventEnded(TrainEngine trainEngine);
    private void OnPlayerRemovedFromPVPDelay(ulong playerId, string zoneId);
    private void OnPlayerPvpDelayExpired(BasePlayer player);
     void OnPlayerPvpDelayExpiredII(BasePlayer player);
    private bool IsPlayerInPVPDelay(ulong playerID);
     void OnPlayerPvpDelayStart(BasePlayer player);
    private void CmdServerPVx(IPlayer iPlayer, string command, string[] args);
    private void CreatePVxUI(BasePlayer player, PVxType type);
    private static void DestroyUI(BasePlayer player);
    private static string GetCuiJson(UiSettings settings);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Show Default PVx UI")]
        public bool showDefault;
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty(PropertyName = "Server Default PVx (pvp or pve)")]
        public PVxType defaultType;
        [JsonProperty(PropertyName = "Pvx UI Settings")]
        public Dictionary<PVxType, UiSettings> UISettings { get; set; }
        [JsonProperty(PropertyName = "Adem Event Settings")]
        public AdemEventSettings AdemEventSettings { get; set; }
        [JsonProperty(PropertyName = "KpucTaJl Event Settings")]
        public KpucTaJlEventSettings KpucTaJlEventSettings { get; set; }
    }

    private class UiSettings
    {
        [JsonProperty(PropertyName = "Min Anchor")]
        public string MinAnchor { get; set; }
        [JsonProperty(PropertyName = "Max Anchor")]
        public string MaxAnchor { get; set; }
        [JsonProperty(PropertyName = "Min Offset")]
        public string MinOffset { get; set; }
        [JsonProperty(PropertyName = "Max Offset")]
        public string MaxOffset { get; set; }
        [JsonProperty(PropertyName = "Layer")]
        public string Layer { get; set; }
        [JsonProperty(PropertyName = "Text")]
        public string Text { get; set; }
        [JsonProperty(PropertyName = "Text Size")]
        public int TextSize { get; set; }
        [JsonProperty(PropertyName = "Text Color")]
        public string TextColor { get; set; }
        [JsonProperty(PropertyName = "Background Color")]
        public string BackgroundColor { get; set; }
        [JsonProperty(PropertyName = "Fade In")]
        public float FadeIn { get; set; }
        [JsonProperty(PropertyName = "Fade Out")]
        public float FadeOut { get; set; }
        private string _json;
        [JsonIgnore]
        public string Json { get; set; }
    }

    private class AdemEventSettings
    {
        [JsonProperty(PropertyName = "Convoy Event")]
        public bool convoyEvent { get; set; }
        [JsonProperty(PropertyName = "Armored Train Event")]
        public bool armoredTrainEvent { get; set; }
        [JsonProperty(PropertyName = "Caravan Event")]
        public bool caravanEvent { get; set; }
        [JsonProperty(PropertyName = "Sputnik Event")]
        public bool sputnikEvent { get; set; }
        [JsonProperty(PropertyName = "Space Event")]
        public bool spaceEvent { get; set; }
        [JsonProperty(PropertyName = "Shipwreck Event")]
        public bool shipwreckEvent { get; set; }
    }

    private class KpucTaJlEventSettings
    {
        [JsonProperty(PropertyName = "Power Plant Event")]
        public bool powerPlantEvent { get; set; }
        [JsonProperty(PropertyName = "Ferry Terminal Event")]
        public bool ferryTerminalEvent { get; set; }
        [JsonProperty(PropertyName = "Supermarket Event")]
        public bool supermarketEvent { get; set; }
        [JsonProperty(PropertyName = "Junkyard Event")]
        public bool junkyardEvent { get; set; }
        [JsonProperty(PropertyName = "Arctic Base Event")]
        public bool arcticEvent { get; set; }
        [JsonProperty(PropertyName = "Gas Station Event")]
        public bool gasStationEvent { get; set; }
        [JsonProperty(PropertyName = "Air Event")]
        public bool airEvent { get; set; }
        [JsonProperty(PropertyName = "Harbor Event")]
        public bool harborEvent { get; set; }
        [JsonProperty(PropertyName = "Satellite Dish Event")]
        public bool satDishEvent { get; set; }
        [JsonProperty(PropertyName = "Water Event")]
        public bool waterEvent { get; set; }
        [JsonProperty(PropertyName = "Triangulation Event")]
        public bool triangulationEvent { get; set; }
        [JsonProperty(PropertyName = "Defendable Homes Event")]
        public bool defendableHomesEvent { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Show Default PVx UI")]
    public bool showDefault;
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty(PropertyName = "Server Default PVx (pvp or pve)")]
    public PVxType defaultType;
    [JsonProperty(PropertyName = "Pvx UI Settings")]
    public Dictionary<PVxType, UiSettings> UISettings { get; set; }
    [JsonProperty(PropertyName = "Adem Event Settings")]
    public AdemEventSettings AdemEventSettings { get; set; }
    [JsonProperty(PropertyName = "KpucTaJl Event Settings")]
    public KpucTaJlEventSettings KpucTaJlEventSettings { get; set; }
}

private class UiSettings
{
    [JsonProperty(PropertyName = "Min Anchor")]
    public string MinAnchor { get; set; }
    [JsonProperty(PropertyName = "Max Anchor")]
    public string MaxAnchor { get; set; }
    [JsonProperty(PropertyName = "Min Offset")]
    public string MinOffset { get; set; }
    [JsonProperty(PropertyName = "Max Offset")]
    public string MaxOffset { get; set; }
    [JsonProperty(PropertyName = "Layer")]
    public string Layer { get; set; }
    [JsonProperty(PropertyName = "Text")]
    public string Text { get; set; }
    [JsonProperty(PropertyName = "Text Size")]
    public int TextSize { get; set; }
    [JsonProperty(PropertyName = "Text Color")]
    public string TextColor { get; set; }
    [JsonProperty(PropertyName = "Background Color")]
    public string BackgroundColor { get; set; }
    [JsonProperty(PropertyName = "Fade In")]
    public float FadeIn { get; set; }
    [JsonProperty(PropertyName = "Fade Out")]
    public float FadeOut { get; set; }
    private string _json;
    [JsonIgnore]
    public string Json { get; set; }
}

private class AdemEventSettings
{
    [JsonProperty(PropertyName = "Convoy Event")]
    public bool convoyEvent { get; set; }
    [JsonProperty(PropertyName = "Armored Train Event")]
    public bool armoredTrainEvent { get; set; }
    [JsonProperty(PropertyName = "Caravan Event")]
    public bool caravanEvent { get; set; }
    [JsonProperty(PropertyName = "Sputnik Event")]
    public bool sputnikEvent { get; set; }
    [JsonProperty(PropertyName = "Space Event")]
    public bool spaceEvent { get; set; }
    [JsonProperty(PropertyName = "Shipwreck Event")]
    public bool shipwreckEvent { get; set; }
}

private class KpucTaJlEventSettings
{
    [JsonProperty(PropertyName = "Power Plant Event")]
    public bool powerPlantEvent { get; set; }
    [JsonProperty(PropertyName = "Ferry Terminal Event")]
    public bool ferryTerminalEvent { get; set; }
    [JsonProperty(PropertyName = "Supermarket Event")]
    public bool supermarketEvent { get; set; }
    [JsonProperty(PropertyName = "Junkyard Event")]
    public bool junkyardEvent { get; set; }
    [JsonProperty(PropertyName = "Arctic Base Event")]
    public bool arcticEvent { get; set; }
    [JsonProperty(PropertyName = "Gas Station Event")]
    public bool gasStationEvent { get; set; }
    [JsonProperty(PropertyName = "Air Event")]
    public bool airEvent { get; set; }
    [JsonProperty(PropertyName = "Harbor Event")]
    public bool harborEvent { get; set; }
    [JsonProperty(PropertyName = "Satellite Dish Event")]
    public bool satDishEvent { get; set; }
    [JsonProperty(PropertyName = "Water Event")]
    public bool waterEvent { get; set; }
    [JsonProperty(PropertyName = "Triangulation Event")]
    public bool triangulationEvent { get; set; }
    [JsonProperty(PropertyName = "Defendable Homes Event")]
    public bool defendableHomesEvent { get; set; }
}


```

---

## ZoneVending by rostov114 - Changes the logic of work of vending machines inside the zone

```csharp
using System;
using Oxide.Core;
using Oxide.Core.Plugins;
using System.Collections.Generic;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Zone Vending", "rostov114", "0.3.5")]
[Description("Changing the logic of work of vending machines inside the zone.")]
 class ZoneVending : RustPlugin
{
    [PluginReference]
    private Plugin ZoneManager;
    private Dictionary<NetworkableId, bool> vendingMachineCache;
    private Configuration _config;
    public class Configuration
    {
        [JsonProperty("Remove payment item")]
        public bool RemovePayment;
        [JsonProperty("List zones")]
        public List<string> Zones;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private void OnGiveSoldItem(VendingMachine vending, Item soldItem, BasePlayer buyer);
    private object OnTakeCurrencyItem(VendingMachine vending, Item takenCurrencyItem);
    private void OnEntityKill(BaseNetworkable entity);
    private bool CheckZone(VendingMachine vending);
}

public class Configuration
{
    [JsonProperty("Remove payment item")]
    public bool RemovePayment;
    [JsonProperty("List zones")]
    public List<string> Zones;
}


```

---

