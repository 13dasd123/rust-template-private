# uMod Plugins Dataset - Code Abstractions (Continued)

Chunk 5 - Generated: 2025-07-06 20:25:18

## Identifier by MrBlue - Gets identification information for one or all connected players

```csharp
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Identifier", "Wulf", "2.0.3")]
[Description("Gets identification information for one or all connected players")]
public class Identifier : CovalencePlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty("Use permission system")]
        public bool UsePermissions;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private const string permId;
    private const string permIpAddress;
    private void Init();
    private void CommandId(IPlayer player, string command, string[] args);
    private void CommandIdAll(IPlayer player, string command, string[] args);
    private IPlayer FindPlayer(string playerNameOrId, IPlayer player);
    private void AddLocalizedCommand(string command);
    private string GetLang(string langKey, string playerId, object[] args);
    private void Message(IPlayer player, string textOrLang, object[] args);
}

public class Configuration
{
    [JsonProperty("Use permission system")]
    public bool UsePermissions;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## IgniterPlus by BranYorath - Configure Igniters

```csharp
using System.Collections.Generic;
using Facepunch;
using Rust;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("IgniterPlus", "Bran", "0.0.5")]
[Description("Configure electrical igniters")]
public class IgniterPlus : RustPlugin
{
     List<Igniter> igniters;
     int counter;
    private void OnServerInitialized();
    private void OnEntitySpawned(Igniter entity);
    private void Unload();
     void Init();
    private ConfigData configData;
     class ConfigData
    {
        public Options Options;
    }

     class Options
    {
        public float SelfDamagePerIgnite;
        public float IgniteRange;
        public float IgniteFrequency;
        public int PowerConsumption;
        public bool Power;
    }

    private void LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConfig(ConfigData config);
}

 class ConfigData
{
    public Options Options;
}

 class Options
{
    public float SelfDamagePerIgnite;
    public float IgniteRange;
    public float IgniteFrequency;
    public int PowerConsumption;
    public bool Power;
}


```

---

## Ignore by MisterPixie - An API for managing ignore lists

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Ignore", "MisterPixie", "2.1.03")]
[Description("Just an ignore API.")]
 class Ignore : CovalencePlugin
{
    private ConfigData configData;
    private Dictionary<string, PlayerData> IgnoreData;
     class ConfigData
    {
        public int IgnoreLimit { get; set; }
    }

     class PlayerData
    {
        public string Name { get; set; }
        public HashSet<string> Ignores { get; set; }
    }

    protected override void LoadDefaultConfig();
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
    private void Init();
    private void SaveIgnores();
    private bool AddIgnore(string playerId, string ignoreId);
    private bool RemoveIgnore(string playerId, string ignoreId);
    private bool HasIgnored(string playerId, string ignoreId);
    private bool AreIgnored(string playerId, string ignoreId);
    private bool IsIgnored(string playerId, string ignoreId);
    private string[] GetIgnoreList(string playerId);
    private string[] IsIgnoredBy(string player);
    private PlayerData GetPlayerData(string playerId);
    private void cmdIgnore(IPlayer player, string command, string[] args);
    private void SendHelpText(IPlayer player);
    private string FindIgnore(string ignore);
    private IPlayer FindPlayer(string nameOrIdOrIp);
}

 class ConfigData
{
    public int IgnoreLimit { get; set; }
}

 class PlayerData
{
    public string Name { get; set; }
    public HashSet<string> Ignores { get; set; }
}


```

---

## IgnoreCollision by 2CHEVSKII - Automatically disables collision of dropped items.

```csharp
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("IgnoreCollision", "2CHEVSKII", "1.1.1")]
[Description("This plugin removes collisions between dropped items")]
internal class IgnoreCollision : RustPlugin
{
    private bool ignore;
    private bool dynamicIgnore;
    private bool logs;
    private int ignoreStartAmount;
    private int droppedItemCount;
    private bool nowDisabled;
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void CheckConfig(string key, T value);
    private void Init();
    private void Loaded();
    private void OnServerInitialized();
    private void OnItemDropped(Item item, BaseEntity entity);
    private void OnItemPickup(Item item, BasePlayer player);
    private void Unload();
    private void DisableCollision();
    private void EnableCollision();
    private void RefreshDroppedItems();
    private void PrintConsoleWarning(WarningType warningType);
}


```

---

## ImageLibrary by k1lly0u - Plugin API for managing images

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Steamworks;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using UnityEngine.Networking;

Oxide.Plugins
[Info("Image Library", "Absolut & K1lly0u", "2.0.62")]
[Description("Plugin API for downloading and managing images")]
 class ImageLibrary : RustPlugin
{
    private ImageIdentifiers imageIdentifiers;
    private ImageURLs imageUrls;
    private SkinInformation skinInformation;
    private DynamicConfigFile identifiers;
    private DynamicConfigFile urls;
    private DynamicConfigFile skininfo;
    private static ImageLibrary il;
    private ImageAssets assets;
    private Queue<LoadOrder> loadOrders;
    private bool orderPending;
    private bool isInitialized;
    private JsonSerializerSettings errorHandling;
    private const string STEAM_API_URL;
    private const string STEAM_AVATAR_URL;
    private string[] itemShortNames;
    private void Loaded();
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void Unload();
    private IEnumerator ProcessLoadOrders();
    private void GetPlayerAvatar(string userId);
    private void RefreshImagery();
    private void CheckForRefresh();
    private void RestoreLoadedImages();
    private void AddDefaultUrls();
    private void LoadInbuiltSkinLookup();
    private readonly Dictionary<string, string> workshopNameToShortname;
    [HookMethod("AddImage")]
    public bool AddImage(string url, string imageName, ulong imageId, Action callback);
    [HookMethod("AddImageData")]
    public bool AddImageData(string imageName, byte[] array, ulong imageId, Action callback);
    [HookMethod("GetImageURL")]
    public string GetImageURL(string imageName, ulong imageId);
    [HookMethod("GetImage")]
    public string GetImage(string imageName, ulong imageId, bool returnUrl);
    [HookMethod("GetImageList")]
    public List<ulong> GetImageList(string name);
    [HookMethod("GetSkinInfo")]
    public Dictionary<string, object> GetSkinInfo(string name, ulong id);
    [HookMethod("HasImage")]
    public bool HasImage(string imageName, ulong imageId);
    public bool IsInStorage(uint crc);
    [HookMethod("IsReady")]
    public bool IsReady();
    [HookMethod("ImportImageList")]
    public void ImportImageList(string title, Dictionary<string, string> imageList, ulong imageId, bool replace, Action callback);
    [HookMethod("ImportItemList")]
    public void ImportItemList(string title, Dictionary<string, Dictionary<ulong, string>> itemList, bool replace, Action callback);
    [HookMethod("ImportImageData")]
    public void ImportImageData(string title, Dictionary<string, byte[]> imageList, ulong imageId, bool replace, Action callback);
    [HookMethod("LoadImageList")]
    public void LoadImageList(string title, List<KeyValuePair<string, ulong>> imageList, Action callback);
    [HookMethod("RemoveImage")]
    public void RemoveImage(string imageName, ulong imageId);
    [HookMethod("SendImage")]
    public void SendImage(BasePlayer player, string imageName, ulong imageId);
    private List<ulong> BuildApprovedItemList();
    private string BuildDetailsString(List<ulong> list, int page);
    private string BuildDetailsString(List<ulong> list);
    private bool IsValid(PublishedFileDetails item);
    private void GetItemSkins();
    private void QueueFileQueryRequest(string details, Action<PublishedFileDetails[]> callback);
    private void GetApprovedItemSkins(List<ulong> itemsToDownload, int page);
    private IEnumerator ProcessApprovedBlock(List<ulong> itemsToDownload, PublishedFileDetails[] items, int page, int totalPages);
    private void QueueWorkshopDownload(string title, Dictionary<string, string> newLoadOrderURL, List<KeyValuePair<string, ulong>> workshopDownloads, int page, Action callback);
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

    [ConsoleCommand("cancelstorage")]
    private void cmdCancelStorage(ConsoleSystem.Arg arg);
    private List<ulong> pendingAnswers;
    [ConsoleCommand("refreshallimages")]
    private void cmdRefreshAllImages(ConsoleSystem.Arg arg);
    [ConsoleCommand("yes")]
    private void cmdRefreshAllImagesYes(ConsoleSystem.Arg arg);
    [ConsoleCommand("no")]
    private void cmdRefreshAllImagesNo(ConsoleSystem.Arg arg);
    private class ImageAssets : MonoBehaviour
    {
        private Queue<QueueItem> queueList;
        private bool isLoading;
        private double nextUpdate;
        private int listCount;
        private string request;
        private Action callback;
        private void OnDestroy();
        public void Add(string name, string url, byte[] bytes);
        public void RegisterCallback(Action callback);
        public void BeginLoad(string request);
        public void ClearList();
        private void Next();
        private IEnumerator DownloadImage(QueueItem info);
        private void StoreByteArray(byte[] bytes, string name);
        private class QueueItem
        {
            public byte[] bytes;
            public string url;
            public string name;
            public QueueItem(string name, string url, byte[] bytes);
        }

    }

    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Avatars - Store player avatars")]
        public bool StoreAvatars { get; set; }
        [JsonProperty(PropertyName = "Steam API key (get one here https://steamcommunity.com/dev/apikey)")]
        public string SteamAPIKey { get; set; }
        [JsonProperty(PropertyName = "URL to web folder containing all item icons")]
        public string ImageURL { get; set; }
        [JsonProperty(PropertyName = "Progress - Show download progress in console")]
        public bool ShowProgress { get; set; }
        [JsonProperty(PropertyName = "Progress - Time between update notifications")]
        public int UpdateInterval { get; set; }
        [JsonProperty(PropertyName = "User Images - Manually define images to be loaded")]
        public Dictionary<string, string> UserImages { get; set; }
        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private void SaveData();
    private void SaveSkinInfo();
    private void SaveUrls();
    private void LoadData();
    private class ImageIdentifiers
    {
        public ulong lastCEID;
        public Hash<string, string> imageIds;
    }

    private class SkinInformation
    {
        public Hash<string, Dictionary<string, object>> skinData;
    }

    private class ImageURLs
    {
        public Hash<string, string> URLs;
    }

    public class AvatarRoot
    {
        public Response response { get; set; }
        public class Response
        {
            public Player[] players { get; set; }
            public class Player
            {
                public string steamid { get; set; }
                public int communityvisibilitystate { get; set; }
                public int profilestate { get; set; }
                public string personaname { get; set; }
                public int lastlogoff { get; set; }
                public string profileurl { get; set; }
                public string avatar { get; set; }
                public string avatarmedium { get; set; }
                public string avatarfull { get; set; }
                public int personastate { get; set; }
                public string realname { get; set; }
                public string primaryclanid { get; set; }
                public int timecreated { get; set; }
                public int personastateflags { get; set; }
            }

        }

    }

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

private class ImageAssets : MonoBehaviour
{
    private Queue<QueueItem> queueList;
    private bool isLoading;
    private double nextUpdate;
    private int listCount;
    private string request;
    private Action callback;
    private void OnDestroy();
    public void Add(string name, string url, byte[] bytes);
    public void RegisterCallback(Action callback);
    public void BeginLoad(string request);
    public void ClearList();
    private void Next();
    private IEnumerator DownloadImage(QueueItem info);
    private void StoreByteArray(byte[] bytes, string name);
    private class QueueItem
    {
        public byte[] bytes;
        public string url;
        public string name;
        public QueueItem(string name, string url, byte[] bytes);
    }

}

private class QueueItem
{
    public byte[] bytes;
    public string url;
    public string name;
    public QueueItem(string name, string url, byte[] bytes);
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Avatars - Store player avatars")]
    public bool StoreAvatars { get; set; }
    [JsonProperty(PropertyName = "Steam API key (get one here https://steamcommunity.com/dev/apikey)")]
    public string SteamAPIKey { get; set; }
    [JsonProperty(PropertyName = "URL to web folder containing all item icons")]
    public string ImageURL { get; set; }
    [JsonProperty(PropertyName = "Progress - Show download progress in console")]
    public bool ShowProgress { get; set; }
    [JsonProperty(PropertyName = "Progress - Time between update notifications")]
    public int UpdateInterval { get; set; }
    [JsonProperty(PropertyName = "User Images - Manually define images to be loaded")]
    public Dictionary<string, string> UserImages { get; set; }
    public Oxide.Core.VersionNumber Version { get; set; }
}

private class ImageIdentifiers
{
    public ulong lastCEID;
    public Hash<string, string> imageIds;
}

private class SkinInformation
{
    public Hash<string, Dictionary<string, object>> skinData;
}

private class ImageURLs
{
    public Hash<string, string> URLs;
}

public class AvatarRoot
{
    public Response response { get; set; }
    public class Response
    {
        public Player[] players { get; set; }
        public class Player
        {
            public string steamid { get; set; }
            public int communityvisibilitystate { get; set; }
            public int profilestate { get; set; }
            public string personaname { get; set; }
            public int lastlogoff { get; set; }
            public string profileurl { get; set; }
            public string avatar { get; set; }
            public string avatarmedium { get; set; }
            public string avatarfull { get; set; }
            public int personastate { get; set; }
            public string realname { get; set; }
            public string primaryclanid { get; set; }
            public int timecreated { get; set; }
            public int personastateflags { get; set; }
        }

    }

}

public class Response
{
    public Player[] players { get; set; }
    public class Player
    {
        public string steamid { get; set; }
        public int communityvisibilitystate { get; set; }
        public int profilestate { get; set; }
        public string personaname { get; set; }
        public int lastlogoff { get; set; }
        public string profileurl { get; set; }
        public string avatar { get; set; }
        public string avatarmedium { get; set; }
        public string avatarfull { get; set; }
        public int personastate { get; set; }
        public string realname { get; set; }
        public string primaryclanid { get; set; }
        public int timecreated { get; set; }
        public int personastateflags { get; set; }
    }

}

public class Player
{
    public string steamid { get; set; }
    public int communityvisibilitystate { get; set; }
    public int profilestate { get; set; }
    public string personaname { get; set; }
    public int lastlogoff { get; set; }
    public string profileurl { get; set; }
    public string avatar { get; set; }
    public string avatarmedium { get; set; }
    public string avatarfull { get; set; }
    public int personastate { get; set; }
    public string realname { get; set; }
    public string primaryclanid { get; set; }
    public int timecreated { get; set; }
    public int personastateflags { get; set; }
}


```

---

## ImgurApi by MJSU - Plugin API for uploading images to imgur.com

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using Newtonsoft.Json;
using UnityEngine;
using UnityEngine.Networking;

Oxide.Plugins
[Info("Imgur API", "MJSU", "1.0.2")]
[Description("Allows plugins to upload images to imgur")]
internal class ImgurApi : CovalencePlugin
{
    private PluginConfig _pluginConfig;
    private const string ClientIdUrl;
    private const string UploadImageUrl;
    private const string DeleteImageUrl;
    private const string CreateAlbumUrl;
    private const string DeleteAlbumUrl;
    private const string AlbumLink;
    private GameObject _go;
    private ImgurBehavior _behavior;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void Unload();
    private void UploadImage(byte[] image, Action<Hash<string, object>> callback, string title, string description);
    private void DeleteSingleImage(string deleteHash, Action<Hash<string, object>> callback);
    private void UploadAlbum(List<Hash<string, object>> images, Action<Hash<string, Hash<string, object>>> callback, string title, string description);
    private void DeleteAlbum(string deleteHash, Action<Hash<string, object>> callback);
    private IEnumerator HandleUploadImage(byte[] image, Action<ApiResponse<UploadResponse>> callback, string title, string description);
    private IEnumerator HandleDeleteSingleImage(string deleteHash, Action<Hash<string, object>> callback);
    private IEnumerator HandleUploadAlbum(List<Hash<string, object>> images, Action<Hash<string, Hash<string, object>>> callback, string title, string description);
    private IEnumerator HandleDeleteAlbum(string deleteHash, Action<Hash<string, object>> callback);
    private IEnumerator HandleImgurRequest(string url, List<IMultipartFormSection> data, Action<ApiResponse<T>> action);
    private class ImgurBehavior : MonoBehaviour
    {
    }

    private class PluginConfig
    {
        [DefaultValue(ClientIdUrl)]
        [JsonProperty(PropertyName = "Imgur Client ID")]
        public string ClientId { get; set; }
    }

    private class ApiResponse
    {
        [JsonProperty(PropertyName = "status")]
        public int Status { get; set; }
        [JsonProperty(PropertyName = "success")]
        public bool Success { get; set; }
        [JsonProperty(PropertyName = "data")]
        public T Data { get; set; }
        [JsonProperty(PropertyName = "errors")]
        public ErrorMessage[] Errors { get; set; }
        public Hash<string, object> ToHash();
    }

    private class ErrorMessage
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        [JsonProperty(PropertyName = "code")]
        public string Code { get; set; }
        [JsonProperty(PropertyName = "status")]
        public string Status { get; set; }
        [JsonProperty(PropertyName = "detail")]
        public string Detail { get; set; }
        public Hash<string, object> ToHash();
    }

    private abstract class BaseDataResponse
    {
        public abstract Hash<string, object> ToHash();
    }

    private class UploadResponse : BaseDataResponse
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        [JsonProperty(PropertyName = "deletehash")]
        public string DeleteHash { get; set; }
        [JsonProperty(PropertyName = "link")]
        public string Link { get; set; }
        public override Hash<string, object> ToHash();
    }

    private class AlbumResponse : BaseDataResponse
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        [JsonProperty(PropertyName = "deletehash")]
        public string DeleteHash { get; set; }
        public string Link { get; set; }
        public override Hash<string, object> ToHash();
    }

}

private class ImgurBehavior : MonoBehaviour
{
}

private class PluginConfig
{
    [DefaultValue(ClientIdUrl)]
    [JsonProperty(PropertyName = "Imgur Client ID")]
    public string ClientId { get; set; }
}

private class ApiResponse
{
    [JsonProperty(PropertyName = "status")]
    public int Status { get; set; }
    [JsonProperty(PropertyName = "success")]
    public bool Success { get; set; }
    [JsonProperty(PropertyName = "data")]
    public T Data { get; set; }
    [JsonProperty(PropertyName = "errors")]
    public ErrorMessage[] Errors { get; set; }
    public Hash<string, object> ToHash();
}

private class ErrorMessage
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }
    [JsonProperty(PropertyName = "code")]
    public string Code { get; set; }
    [JsonProperty(PropertyName = "status")]
    public string Status { get; set; }
    [JsonProperty(PropertyName = "detail")]
    public string Detail { get; set; }
    public Hash<string, object> ToHash();
}

private abstract class BaseDataResponse
{
    public abstract Hash<string, object> ToHash();
}

private class UploadResponse : BaseDataResponse
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }
    [JsonProperty(PropertyName = "deletehash")]
    public string DeleteHash { get; set; }
    [JsonProperty(PropertyName = "link")]
    public string Link { get; set; }
    public override Hash<string, object> ToHash();
}

private class AlbumResponse : BaseDataResponse
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }
    [JsonProperty(PropertyName = "deletehash")]
    public string DeleteHash { get; set; }
    public string Link { get; set; }
    public override Hash<string, object> ToHash();
}


```

---

## Imperium by evict - Team up, claim land, and battle for supremacy

```csharp
using System;
using System.IO;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;
using System.Collections.Generic;
using System.Linq;
using Network;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;
using UnityEngine;
using UnityEngine;
using System;
using System.Text.RegularExpressions;
using System;
using UnityEngine;
using System.Text;
using System.Linq;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Collections.Generic;
using System.Linq;
using System.Collections.Generic;
using System.Linq;
using System.Linq;
using System.Collections.Generic;
using System.Text;
using System;
using System.Linq;
using System.Text;
using System;
using System;
using System.Linq;
using System;
using System;
using System.Text;
using System.Text;
using System.Linq;
using System;
using System.Collections.Generic;
using UnityEngine;
using System;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System;
using System.Linq;
using System.Text;
using System;
using System.Linq;
using System;
using System.Text;
using System.Text;
using System.Text;
using UnityEngine;
using System.Text;
using UnityEngine;
using System.Collections.Generic;
using System.Text;
using System;
using System.Linq;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Linq;
using System.Text;
using System;
using System.Text;
using System;
using System.Text;
using System.Linq;
using System;
using System.Text;
using System;
using System.Text;
using System.Linq;
using System;
using System.Text;
using System.Linq;
using System;
using System.Text;
using System.Linq;
using System;
using System.Text;
using System.Linq;
using System;
using System.Text;
using Oxide.Core.Plugins;
using Oxide.Core;
using Network;
using Oxide.Core;
using UnityEngine;
using Newtonsoft.Json.Linq;
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using System.Collections.Generic;
using System;
using System.Linq;
using System;
using System.Linq;
using UnityEngine;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Rust;
using Rust.UI;
using UnityEngine;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using UnityEngine;
using System;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Newtonsoft.Json.Linq;
using UnityEngine;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using UnityEngine;
using System;
using System.Collections.Generic;
using System.Linq;
using System;
using System.Collections.Generic;
using System.Text;
using UnityEngine;
using System.Linq;
using Newtonsoft.Json.Linq;
using System.Collections.Generic;
using System.Linq;
using System;
using System;
using System.Collections.Generic;
using System;
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Newtonsoft.Json.Linq;
using System;
using System.Collections.Generic;
using System.Linq;
using Rust;
using System;
using System.Collections.Generic;
using UnityEngine;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Rust;
using UnityEngine;
using System.Collections.Generic;
using System.Drawing;
using System;
using UnityEngine;
using System.Collections;
using System.Drawing;
using System.Drawing.Drawing2D;
using System.Reflection;
using System;
using Newtonsoft.Json;
using UnityEngine;
using System;
using System.Collections;
using System.Text;
using UnityEngine;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json;
using System.Collections.Generic;
using Newtonsoft.Json;
using Newtonsoft.Json;
using Newtonsoft.Json;
using System.Collections.Generic;
using Newtonsoft.Json;
using Newtonsoft.Json;
using Newtonsoft.Json;
using Newtonsoft.Json;
using Newtonsoft.Json;
using Newtonsoft.Json;
using Newtonsoft.Json;
using Newtonsoft.Json;
using System.Collections.Generic;
using Newtonsoft.Json;
using System.Collections.Generic;
using Newtonsoft.Json;
using System.Collections.Generic;
using System.Collections.Generic;
using UnityEngine;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using Oxide.Game.Rust.Cui;
using System;
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;
using System.Collections.Generic;
using Oxide.Game.Rust.Cui;
using System;
using System.Linq;
using UnityEngine;
using System.Globalization;
using Oxide.Game.Rust.Cui;
using System;
using System.Linq;
using UnityEngine;
using Oxide.Game.Rust.Cui;
using System.Collections.Generic;
using System;
using System.Linq;
using UnityEngine;
using System;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Linq;
using System.Collections.Generic;

Oxide.Plugins
[Info("Imperium", "chucklenugget/evict", "2.2.9")]
[Description("Land Claims for Rust")]
public partial class Imperium : RustPlugin
{
    [PluginReference]
    private Plugin BetterChat;
    private Plugin Clans;
    private Plugin RaidableBases;
    private Plugin NPCSpawn;
    private List<HookDeferral> HookDeferralRegistry;
    public class HookDeferral
    {
        public string hookName;
        public Plugin plugin;
        public HookDeferral(string HookName, Plugin Plugin);
    }

    [PluginReference]
    private Plugin NpcSpawn;
    private Plugin AirEvent;
    private void InitDeferList();
    private static Imperium Instance;
    private bool Ready;
    public static string dataDirectory;
    private DynamicConfigFile AreasFile;
    private DynamicConfigFile FactionsFile;
    private DynamicConfigFile PinsFile;
    private DynamicConfigFile WarsFile;
    private GameObject GO;
    private ImperiumOptions Options;
    private Timer UpkeepCollectionTimer;
    private AreaManager Areas;
    private FactionManager Factions;
    private HudManager Hud;
    private PinManager Pins;
    private UserManager Users;
    private WarManager Wars;
    private ZoneManager Zones;
    private RecruitManager Recruits;
    private void Init();
    private void RegisterHookDeferral(string hook, Plugin plugin);
    private object GetExternalHookResult(string hook, object[] args);
    private void Loaded();
    private void OnServerInitialized(bool initial);
    private void Setup();
    private void Unload();
    private void OnServerSave();
    private void SaveData();
    private DynamicConfigFile GetDataFile(string name);
    private IEnumerable<T> TryLoad(DynamicConfigFile file);
    private void Log(string message, object[] args);
    private bool EnsureUserCanChangeFactionClaims(User user, Faction faction);
    private bool EnsureFactionCanClaimArea(User user, Faction faction, Area area);
    private bool EnsureCupboardCanBeUsedForClaim(User user, BuildingPrivlidge cupboard);
    private bool EnsureLockerCanBeUsedForArmory(User user, Locker locker, Area area);
    private bool EnsureUserAndFactionCanEngageInDiplomacy(User user, Faction faction);
    private bool EnforceCommandCooldown(User user, string command, int cooldownSeconds);
    private bool TryCollectFromStacks(ItemDefinition itemDef, IEnumerable<Item> stacks, int amount);
}

public class HookDeferral
{
    public string hookName;
    public Plugin plugin;
    public HookDeferral(string HookName, Plugin Plugin);
}

Oxide.Plugins
public partial class Imperium
{
    private string BetterChat_FormattedFactionTag(IPlayer player);
}

Oxide.Plugins
public partial class Imperium
{
    [ConsoleCommand("imperium.panel.close")]
    private void ccmdImperiumPanelClose(ConsoleSystem.Arg arg);
}

Oxide.Plugins
public partial class Imperium
{
    [ConsoleCommand("imperium.panel.opentab")]
    private void ccmdImperiumPanelOpenTab(ConsoleSystem.Arg arg);
}

Oxide.Plugins
public partial class Imperium
{
    [ConsoleCommand("imperium.panel.opencmd")]
    private void ccmdImperiumPanelOpenCmd(ConsoleSystem.Arg arg);
}

Oxide.Plugins
public partial class Imperium
{
    [ConsoleCommand("imperium.panel.run")]
    private void ccmdImperiumPanelRun(ConsoleSystem.Arg arg);
}

Oxide.Plugins
public partial class Imperium
{
    [ConsoleCommand("imperium.panel.setarg")]
    private void ccmdImperiumPanelSetArg(ConsoleSystem.Arg arg);
}

Oxide.Plugins
public partial class Imperium
{
    [ChatCommand("cancel")]
    private void OnCancelCommand(BasePlayer player, string command, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    [ChatCommand("help")]
    private void OnHelpCommand(BasePlayer player, string command, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    [ChatCommand("i")]
    private void OnImperiumCommand(BasePlayer player, string command, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    [ChatCommand("pvp")]
    private void OnPvpCommand(BasePlayer player, string command, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    [ChatCommand("badlands")]
    private void OnBadlandsCommand(BasePlayer player, string command, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnAddBadlandsCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private User user;
    private void OnBadlandsHelpCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnRemoveBadlandsCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnSetBadlandsCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    [ChatCommand("claim")]
    private void OnClaimCommand(BasePlayer player, string command, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnClaimAddCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnClaimAssignCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnClaimCostCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnClaimDeleteCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnClaimGiveCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnClaimHeadquartersCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnClaimHelpCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnClaimListCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnClaimRemoveCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnClaimRenameCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnClaimShowCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnClaimUpkeepCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    [ChatCommand("faction")]
    private void OnFactionCommand(BasePlayer player, string command, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    [ChatCommand("f")]
    private void OnFactionChatCommand(BasePlayer player, string command, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnFactionCreateCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnFactionDemoteCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnFactionBadlandsCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnFactionDisbandCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnFactionHelpCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnFactionInviteCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnFactionJoinCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnFactionKickCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnFactionLeaveCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnFactionPromoteCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnFactionShowCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    [ConsoleCommand("imperium.images.refresh")]
    private void OnRefreshImagesConsoleCommand(ConsoleSystem.Arg arg);
}

Oxide.Plugins
public partial class Imperium
{
    [ChatCommand("pin")]
    private void OnPinCommand(BasePlayer player, string command, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnPinAddCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnPinDeleteCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnPinHelpCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnPinListCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnPinRemoveCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    [ChatCommand("tax")]
    private void OnTaxCommand(BasePlayer player, string command, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnTaxChestCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnTaxRateCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnTaxHelpCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnRecruitLockerCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnRecruitHereCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnRecruitHelpCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    [ChatCommand("upgrade")]
    private void OnUpgradeCommand(BasePlayer player, string command, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnUpgradeCostCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnUpgradeLandCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnUpgradeHelpCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    [ChatCommand("hud")]
    private void OnHudCommand(BasePlayer player, string command, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    [ConsoleCommand("imperium.hud.toggle")]
    private void OnHudToggleConsoleCommand(ConsoleSystem.Arg arg);
}

Oxide.Plugins
public partial class Imperium
{
    [ConsoleCommand("imperium.map.togglelayer")]
    private void OnMapToggleLayerConsoleCommand(ConsoleSystem.Arg arg);
}

Oxide.Plugins
public partial class Imperium
{
    [ChatCommand("map")]
    private void OnMapCommand(BasePlayer player, string command, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    [ConsoleCommand("imperium.map.toggle")]
    private void OnMapToggleConsoleCommand(ConsoleSystem.Arg arg);
}

Oxide.Plugins
public partial class Imperium
{
    [ChatCommand("war")]
    private void OnWarCommand(BasePlayer player, string command, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnWarDeclareCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnWarEndCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnWarHelpCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnWarListCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnWarStatusCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnWarAdminCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnWarAdminPendingCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnWarAdminApproveCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnWarAdminDenyCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnWarApproveCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnWarDenyCommand(User user, string[] args);
}

Oxide.Plugins
public partial class Imperium
{
    private void OnWarPendingCommand(User user);
}

Oxide.Plugins
public partial class Imperium
{
    [HookMethod(nameof(GetFactionName))]
    public object GetFactionName(BasePlayer player);
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private static class Events
    {
        public static void OnAreaChanged(Area area);
        public static void OnUserEnteredArea(User user, Area area);
        public static void OnUserLeftArea(User user, Area area);
        public static void OnUserEnteredZone(User user, Zone zone);
        public static void OnUserLeftZone(User user, Zone zone);
        public static void OnFactionCreated(Faction faction);
        public static void OnFactionDisbanded(Faction faction);
        public static void OnFactionTaxesChanged(Faction faction);
        public static void OnFactionArmoryChanged(Faction faction);
        public static void OnFactionBadlandsChanged(Faction faction);
        public static void OnPlayerJoinedFaction(Faction faction, User user);
        public static void OnPlayerLeftFaction(Faction faction, User user);
        public static void OnPlayerInvitedToFaction(Faction faction, User user);
        public static void OnPlayerUninvitedFromFaction(Faction faction, User user);
        public static void OnPlayerPromoted(Faction faction, User user);
        public static void OnPlayerDemoted(Faction faction, User user);
        public static void OnPinCreated(Pin pin);
        public static void OnPinRemoved(Pin pin);
    }

}

private static class Events
{
    public static void OnAreaChanged(Area area);
    public static void OnUserEnteredArea(User user, Area area);
    public static void OnUserLeftArea(User user, Area area);
    public static void OnUserEnteredZone(User user, Zone zone);
    public static void OnUserLeftZone(User user, Zone zone);
    public static void OnFactionCreated(Faction faction);
    public static void OnFactionDisbanded(Faction faction);
    public static void OnFactionTaxesChanged(Faction faction);
    public static void OnFactionArmoryChanged(Faction faction);
    public static void OnFactionBadlandsChanged(Faction faction);
    public static void OnPlayerJoinedFaction(Faction faction, User user);
    public static void OnPlayerLeftFaction(Faction faction, User user);
    public static void OnPlayerInvitedToFaction(Faction faction, User user);
    public static void OnPlayerUninvitedFromFaction(Faction faction, User user);
    public static void OnPlayerPromoted(Faction faction, User user);
    public static void OnPlayerDemoted(Faction faction, User user);
    public static void OnPinCreated(Pin pin);
    public static void OnPinRemoved(Pin pin);
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private void OnUserApprove(Connection connection);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerSleepEnded(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private object OnTeamCreate(BasePlayer player);
    private object OnTeamInvite(BasePlayer inviter, BasePlayer target);
    private object OnTeamPromote(RelationshipManager.PlayerTeam team, BasePlayer newLeader);
    private object OnTeamKick(ulong currentTeam, ulong newTeam, BasePlayer player);
    private object OnTeamLeave(RelationshipManager.PlayerTeam team, BasePlayer player);
    private object OnTeamDisband(RelationshipManager.PlayerTeam team);
    private void OnHammerHit(BasePlayer player, HitInfo hit);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hit);
    private object OnTrapTrigger(BaseTrap trap, GameObject obj);
    private object CanBeTargeted(BaseCombatEntity target, MonoBehaviour turret);
    private void OnEntitySpawned(BaseNetworkable entity);
    private object OnEntityReskin(BaseEntity entity, ItemSkinDirectory.Skin skin, BasePlayer player);
    private object OnEntityReskinned(BaseEntity entity, ItemSkinDirectory.Skin skin, BasePlayer player);
    private void OnEntityKill(BaseNetworkable networkable);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void OnDispenserBonus(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private object OnEntityDeath(BasePlayer player, HitInfo hit);
    private object OnShopAcceptClick(ShopFront entity, BasePlayer player);
    private object OnShopCompleteTrade(ShopFront entity);
    private void OnUserEnteredArea(User user, Area area);
    private void OnUserLeftArea(User user, Area area);
    private void OnUserEnteredZone(User user, Zone zone);
    private void OnUserLeftZone(User user, Zone zone);
    private void OnFactionCreated(Faction faction);
    private void OnFactionDisbanded(Faction faction);
    private void OnFactionTaxesChanged(Faction faction);
    private void OnFactionArmoryChanged(Faction faction);
    private void OnAreaChanged(Area area);
    private void OnDiplomacyChanged();
    private void OnPluginLoaded(CSharpPlugin plugin);
    private void OnClanCreate(string tag);
    private void OnClanDisbanded(string tag, List<string> memberUserIDs);
    private void OnClanMemberJoined(string userID, string tag);
    private void OnClanMemberGone(string userID, string tag);
    private void OnPlayerEnteredRaidableBase(BasePlayer player, Vector3 raidPos, bool allowPVP, int mode);
    private void OnPlayerExitedRaidableBase(BasePlayer player, Vector3 raidPos, bool allowPVP, int mode);
    private object OnCustomNpcTarget(BasePlayer npc, BasePlayer target);
}

Oxide.Plugins
public partial class Imperium
{
    private static class Decay
    {
        public static object AlterDecayDamage(BaseEntity entity, HitInfo hit);
        private static Area GetAreaForDecayCalculation(BaseEntity entity);
    }

}

private static class Decay
{
    public static object AlterDecayDamage(BaseEntity entity, HitInfo hit);
    private static Area GetAreaForDecayCalculation(BaseEntity entity);
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private static class Messages
    {
        public const string AreaClaimsDisabled;
        public const string TaxationDisabled;
        public const string RecruitingDisabled;
        public const string BadlandsDisabled;
        public const string UpkeepDisabled;
        public const string WarDisabled;
        public const string PinsDisabled;
        public const string PvpModeDisabled;
        public const string UpgradingDisabled;
        public const string AreaIsBadlands;
        public const string AreaIsClaimed;
        public const string AreaIsHeadquarters;
        public const string AreaIsWilderness;
        public const string AreaNotBadlands;
        public const string AreaNotOwnedByYourFaction;
        public const string AreaNotWilderness;
        public const string AreaNotContiguous;
        public const string YouAreInTheGreatUnknown;
        public const string InteractionCanceled;
        public const string NoInteractionInProgress;
        public const string NoAreasClaimed;
        public const string FactionChatMessage;
        public const string NotMemberOfFaction;
        public const string AlreadyMemberOfFaction;
        public const string NotLeaderOfFaction;
        public const string FactionTooSmallToOwnLand;
        public const string FactionOwnsTooMuchLand;
        public const string FactionHasTooManyMembers;
        public const string AreaIsMaximumLevel;
        public const string FactionIsBadlands;
        public const string FactionIsNotBadlands;
        public const string NoFactionBadlandsAllowed;
        public const string FactionDoesNotOwnLand;
        public const string FactionAlreadyExists;
        public const string FactionDoesNotExist;
        public const string InvalidUser;
        public const string InvalidFactionName;
        public const string NotAtWar;
        public const string Usage;
        public const string CommandIsOnCooldown;
        public const string NoPermission;
        public const string MemberAdded;
        public const string MemberRemoved;
        public const string ManagerAdded;
        public const string ManagerRemoved;
        public const string UserIsAlreadyMemberOfFaction;
        public const string UserIsNotMemberOfFaction;
        public const string UserIsAlreadyManagerOfFaction;
        public const string UserIsNotManagerOfFaction;
        public const string CannotPromoteOrDemoteOwnerOfFaction;
        public const string CannotKickLeaderOfFaction;
        public const string InviteAdded;
        public const string InviteReceived;
        public const string CannotJoinFactionNotInvited;
        public const string CannotManageFactionUseClansInstead;
        public const string YouJoinedFaction;
        public const string YouLeftFaction;
        public const string SelectingCupboardFailedInvalidTarget;
        public const string SelectingCupboardFailedNotAuthorized;
        public const string SelectingCupboardFailedCantUnclaimHeadquarters;
        public const string SelectingCupboardFailedNotClaimCupboard;
        public const string CannotClaimAreaAlreadyClaimed;
        public const string CannotClaimAreaCannotAfford;
        public const string CannotUpgradeAreaCannotAfford;
        public const string CannotClaimAreaAlreadyOwned;
        public const string SelectClaimCupboardToAdd;
        public const string SelectClaimCupboardToRemove;
        public const string SelectClaimCupboardForHeadquarters;
        public const string SelectClaimCupboardToAssign;
        public const string SelectClaimCupboardToTransfer;
        public const string ClaimCupboardMoved;
        public const string ClaimCaptured;
        public const string ClaimAdded;
        public const string ClaimRemoved;
        public const string ClaimTransferred;
        public const string InvalidAreaName;
        public const string UnknownArea;
        public const string AreaRenamed;
        public const string ClaimsList;
        public const string ClaimCost;
        public const string UpkeepCost;
        public const string UpkeepCostOverdue;
        public const string SelectArmoryLocker;
        public const string SelectTaxChest;
        public const string SelectingTaxChestFailedInvalidTarget;
        public const string SelectingTaxChestSucceeded;
        public const string SelectingArmoryLockerFailedInvalidTarget;
        public const string SelectingArmoryLockerSucceeded;
        public const string CannotSetTaxRateInvalidValue;
        public const string SetTaxRateSuccessful;
        public const string BadlandsSet;
        public const string BadlandsList;
        public const string CannotDeclareWarAgainstYourself;
        public const string CannotDeclareWarAlreadyAtWar;
        public const string CannotDeclareWarNoobAttacker;
        public const string CannotDeclareWarDefenderProtected;
        public const string CannotDeclareWarInvalidCassusBelli;
        public const string CannotDeclareWarCannotAfford;
        public const string CannotDeclareWarDefendersNotOnline;
        public const string CannotOfferPeaceAlreadyOfferedPeace;
        public const string PeaceOffered;
        public const string EnteredBadlands;
        public const string EnteredWilderness;
        public const string EnteredClaimedArea;
        public const string EnteredPvpMode;
        public const string ExitedPvpMode;
        public const string PvpModeOnCooldown;
        public const string InvalidPinType;
        public const string InvalidPinName;
        public const string CannotCreatePinCannotAfford;
        public const string CannotCreatePinAlreadyExists;
        public const string UnknownPin;
        public const string CannotRemovePinAreaNotOwnedByYourFaction;
        public const string PinRemoved;
        public const string FactionCreatedAnnouncement;
        public const string FactionDisbandedAnnouncement;
        public const string FactionMemberJoinedAnnouncement;
        public const string FactionMemberLeftAnnouncement;
        public const string AreaClaimedAnnouncement;
        public const string AreaClaimedAsHeadquartersAnnouncement;
        public const string AreaLevelUpgraded;
        public const string AreaCapturedAnnouncement;
        public const string AreaClaimRemovedAnnouncement;
        public const string AreaClaimTransferredAnnouncement;
        public const string AreaClaimAssignedAnnouncement;
        public const string AreaClaimDeletedAnnouncement;
        public const string AreaClaimLostCupboardDestroyedAnnouncement;
        public const string AreaClaimLostArmoryDestroyedAnnouncement;
        public const string AreaClaimLostFactionDisbandedAnnouncement;
        public const string AreaClaimLostUpkeepNotPaidAnnouncement;
        public const string HeadquartersChangedAnnouncement;
        public const string NoWarBetweenFactions;
        public const string WarDeclaredAnnouncement;
        public const string WarDeclaredAdminApproved;
        public const string WarDeclaredAdminDenied;
        public const string WarDeclaredDefenderApproved;
        public const string WarDeclaredDefenderDenied;
        public const string WarEndedTreatyAcceptedAnnouncement;
        public const string WarEndedFactionEliminatedAnnouncement;
        public const string PinAddedAnnouncement;
        public static string Get(string key, string userId);
        public static Dictionary<string, string> AsDictionary(BindingFlags bindingAttr);
    }

    private void InitLang();
}

private static class Messages
{
    public const string AreaClaimsDisabled;
    public const string TaxationDisabled;
    public const string RecruitingDisabled;
    public const string BadlandsDisabled;
    public const string UpkeepDisabled;
    public const string WarDisabled;
    public const string PinsDisabled;
    public const string PvpModeDisabled;
    public const string UpgradingDisabled;
    public const string AreaIsBadlands;
    public const string AreaIsClaimed;
    public const string AreaIsHeadquarters;
    public const string AreaIsWilderness;
    public const string AreaNotBadlands;
    public const string AreaNotOwnedByYourFaction;
    public const string AreaNotWilderness;
    public const string AreaNotContiguous;
    public const string YouAreInTheGreatUnknown;
    public const string InteractionCanceled;
    public const string NoInteractionInProgress;
    public const string NoAreasClaimed;
    public const string FactionChatMessage;
    public const string NotMemberOfFaction;
    public const string AlreadyMemberOfFaction;
    public const string NotLeaderOfFaction;
    public const string FactionTooSmallToOwnLand;
    public const string FactionOwnsTooMuchLand;
    public const string FactionHasTooManyMembers;
    public const string AreaIsMaximumLevel;
    public const string FactionIsBadlands;
    public const string FactionIsNotBadlands;
    public const string NoFactionBadlandsAllowed;
    public const string FactionDoesNotOwnLand;
    public const string FactionAlreadyExists;
    public const string FactionDoesNotExist;
    public const string InvalidUser;
    public const string InvalidFactionName;
    public const string NotAtWar;
    public const string Usage;
    public const string CommandIsOnCooldown;
    public const string NoPermission;
    public const string MemberAdded;
    public const string MemberRemoved;
    public const string ManagerAdded;
    public const string ManagerRemoved;
    public const string UserIsAlreadyMemberOfFaction;
    public const string UserIsNotMemberOfFaction;
    public const string UserIsAlreadyManagerOfFaction;
    public const string UserIsNotManagerOfFaction;
    public const string CannotPromoteOrDemoteOwnerOfFaction;
    public const string CannotKickLeaderOfFaction;
    public const string InviteAdded;
    public const string InviteReceived;
    public const string CannotJoinFactionNotInvited;
    public const string CannotManageFactionUseClansInstead;
    public const string YouJoinedFaction;
    public const string YouLeftFaction;
    public const string SelectingCupboardFailedInvalidTarget;
    public const string SelectingCupboardFailedNotAuthorized;
    public const string SelectingCupboardFailedCantUnclaimHeadquarters;
    public const string SelectingCupboardFailedNotClaimCupboard;
    public const string CannotClaimAreaAlreadyClaimed;
    public const string CannotClaimAreaCannotAfford;
    public const string CannotUpgradeAreaCannotAfford;
    public const string CannotClaimAreaAlreadyOwned;
    public const string SelectClaimCupboardToAdd;
    public const string SelectClaimCupboardToRemove;
    public const string SelectClaimCupboardForHeadquarters;
    public const string SelectClaimCupboardToAssign;
    public const string SelectClaimCupboardToTransfer;
    public const string ClaimCupboardMoved;
    public const string ClaimCaptured;
    public const string ClaimAdded;
    public const string ClaimRemoved;
    public const string ClaimTransferred;
    public const string InvalidAreaName;
    public const string UnknownArea;
    public const string AreaRenamed;
    public const string ClaimsList;
    public const string ClaimCost;
    public const string UpkeepCost;
    public const string UpkeepCostOverdue;
    public const string SelectArmoryLocker;
    public const string SelectTaxChest;
    public const string SelectingTaxChestFailedInvalidTarget;
    public const string SelectingTaxChestSucceeded;
    public const string SelectingArmoryLockerFailedInvalidTarget;
    public const string SelectingArmoryLockerSucceeded;
    public const string CannotSetTaxRateInvalidValue;
    public const string SetTaxRateSuccessful;
    public const string BadlandsSet;
    public const string BadlandsList;
    public const string CannotDeclareWarAgainstYourself;
    public const string CannotDeclareWarAlreadyAtWar;
    public const string CannotDeclareWarNoobAttacker;
    public const string CannotDeclareWarDefenderProtected;
    public const string CannotDeclareWarInvalidCassusBelli;
    public const string CannotDeclareWarCannotAfford;
    public const string CannotDeclareWarDefendersNotOnline;
    public const string CannotOfferPeaceAlreadyOfferedPeace;
    public const string PeaceOffered;
    public const string EnteredBadlands;
    public const string EnteredWilderness;
    public const string EnteredClaimedArea;
    public const string EnteredPvpMode;
    public const string ExitedPvpMode;
    public const string PvpModeOnCooldown;
    public const string InvalidPinType;
    public const string InvalidPinName;
    public const string CannotCreatePinCannotAfford;
    public const string CannotCreatePinAlreadyExists;
    public const string UnknownPin;
    public const string CannotRemovePinAreaNotOwnedByYourFaction;
    public const string PinRemoved;
    public const string FactionCreatedAnnouncement;
    public const string FactionDisbandedAnnouncement;
    public const string FactionMemberJoinedAnnouncement;
    public const string FactionMemberLeftAnnouncement;
    public const string AreaClaimedAnnouncement;
    public const string AreaClaimedAsHeadquartersAnnouncement;
    public const string AreaLevelUpgraded;
    public const string AreaCapturedAnnouncement;
    public const string AreaClaimRemovedAnnouncement;
    public const string AreaClaimTransferredAnnouncement;
    public const string AreaClaimAssignedAnnouncement;
    public const string AreaClaimDeletedAnnouncement;
    public const string AreaClaimLostCupboardDestroyedAnnouncement;
    public const string AreaClaimLostArmoryDestroyedAnnouncement;
    public const string AreaClaimLostFactionDisbandedAnnouncement;
    public const string AreaClaimLostUpkeepNotPaidAnnouncement;
    public const string HeadquartersChangedAnnouncement;
    public const string NoWarBetweenFactions;
    public const string WarDeclaredAnnouncement;
    public const string WarDeclaredAdminApproved;
    public const string WarDeclaredAdminDenied;
    public const string WarDeclaredDefenderApproved;
    public const string WarDeclaredDefenderDenied;
    public const string WarEndedTreatyAcceptedAnnouncement;
    public const string WarEndedFactionEliminatedAnnouncement;
    public const string PinAddedAnnouncement;
    public static string Get(string key, string userId);
    public static Dictionary<string, string> AsDictionary(BindingFlags bindingAttr);
}

Oxide.Plugins
public partial class Imperium
{
    private static class Pvp
    {
        private static string[] BlockedPrefabs;
        public static object HandleDamageBetweenPlayers(User attacker, User defender, HitInfo hit);
        public static object HandleIncidentalDamage(User defender, HitInfo hit);
        public static object HandleTrapTrigger(BaseTrap trap, User defender);
        public static object HandleTurretTarget(BaseCombatEntity turret, User defender);
        public static bool IsUserInDanger(User user);
        public static bool IsPvpZone(Zone zone);
        public static bool IsPvpArea(Area area);
    }

}

private static class Pvp
{
    private static string[] BlockedPrefabs;
    public static object HandleDamageBetweenPlayers(User attacker, User defender, HitInfo hit);
    public static object HandleIncidentalDamage(User defender, HitInfo hit);
    public static object HandleTrapTrigger(BaseTrap trap, User defender);
    public static object HandleTurretTarget(BaseCombatEntity turret, User defender);
    public static bool IsUserInDanger(User user);
    public static bool IsPvpZone(Zone zone);
    public static bool IsPvpArea(Area area);
}

Oxide.Plugins
public partial class Imperium
{
    private static class Raiding
    {
        private const bool EnableTestMode;
        private static string[] BlockedPrefabs;
        private static string[] ProtectedPrefabs;
        private static string[] RaidTriggeringPrefabs;
        public static object HandleDamageAgainstStructure(User attacker, BaseEntity entity, HitInfo hit);
        private static DamageResult DetermineDamageResult(User attacker, Area area, BaseEntity entity);
        public static object HandleIncidentalDamage(BaseEntity entity, HitInfo hit);
        private static bool IsProtectedEntity(BaseEntity entity);
        private static bool IsRaidTriggeringEntity(BaseEntity entity);
        private static bool IsRaidableArea(Area area);
    }

}

private static class Raiding
{
    private const bool EnableTestMode;
    private static string[] BlockedPrefabs;
    private static string[] ProtectedPrefabs;
    private static string[] RaidTriggeringPrefabs;
    public static object HandleDamageAgainstStructure(User attacker, BaseEntity entity, HitInfo hit);
    private static DamageResult DetermineDamageResult(User attacker, Area area, BaseEntity entity);
    public static object HandleIncidentalDamage(BaseEntity entity, HitInfo hit);
    private static bool IsProtectedEntity(BaseEntity entity);
    private static bool IsRaidTriggeringEntity(BaseEntity entity);
    private static bool IsRaidableArea(Area area);
}

Oxide.Plugins
public partial class Imperium
{
    private static class Taxes
    {
        public static void ProcessTaxesIfApplicable(ResourceDispenser dispenser, BaseEntity entity, Item item);
        public static void AwardBadlandsBonusIfApplicable(ResourceDispenser dispenser, BaseEntity entity, Item item);
    }

}

private static class Taxes
{
    public static void ProcessTaxesIfApplicable(ResourceDispenser dispenser, BaseEntity entity, Item item);
    public static void AwardBadlandsBonusIfApplicable(ResourceDispenser dispenser, BaseEntity entity, Item item);
}

Oxide.Plugins
public partial class Imperium
{
    private static class Upkeep
    {
        public static void CollectForAllFactions();
        public static void Collect(Faction faction);
    }

}

private static class Upkeep
{
    public static void CollectForAllFactions();
    public static void Collect(Faction faction);
}

Oxide.Plugins
public partial class Imperium
{
    private class Area : MonoBehaviour
    {
        public Vector3 Position { get; set; }
        public Vector3 Size { get; set; }
        public string Id { get; set; }
        public int Row { get; set; }
        public int Col { get; set; }
        public AreaType Type { get; set; }
        public string Name { get; set; }
        public string FactionId { get; set; }
        public string ClaimantId { get; set; }
        public BuildingPrivlidge ClaimCupboard { get; set; }
        public BasePlayer ClaimReskinningPlayer { get; set; }
        public Vector3 ReskinnedCupboardLastPosition { get; set; }
        public bool IsCupboardChangingSkin { get; set; }
        public Locker ArmoryLocker { get; set; }
        public int Level { get; set; }
        public bool HasRecruit;
        public string RecruitSpawnPosition;
        public MapMarkerGenericRadius mapMarker;
        public VendingMachineMapMarker hqMarker;
        public MapMarkerGenericRadius hqMarkerColor;
        public BaseEntity textMarker;
        public bool IsClaimed { get; set; }
        public bool IsTaxableClaim { get; set; }
        public bool IsWarZone { get; set; }
        public bool IsHostile { get; set; }
        public int UpgradeCost { get; set; }
        public void Init(string id, int row, int col, Vector3 position, Vector3 size, AreaInfo info);
        private void Awake();
        private void OnDestroy();
        private void TryLoadInfo(AreaInfo info);
        private void CheckClaimCupboard();
        private void CheckArmoryLocker();
        private void OnTriggerEnter(Collider collider);
        private void OnTriggerExit(Collider collider);
        public void UpdateAreaMarker(FactionColorPicker colorPicker);
        public float GetDistanceFromEntity(BaseEntity entity);
        public int GetClaimCost(Faction faction);
        public float GetDefensiveBonus();
        public float GetTaxRate();
        public War[] GetActiveWars();
        private float GetRatio(int level, int maxLevel, float maxBonus);
        public float GetLevelDefensiveBonus();
        public float GetLevelDecayReduction();
        public float GetLevelTaxBonus();
        public AreaInfo Serialize();
    }

}

private class Area : MonoBehaviour
{
    public Vector3 Position { get; set; }
    public Vector3 Size { get; set; }
    public string Id { get; set; }
    public int Row { get; set; }
    public int Col { get; set; }
    public AreaType Type { get; set; }
    public string Name { get; set; }
    public string FactionId { get; set; }
    public string ClaimantId { get; set; }
    public BuildingPrivlidge ClaimCupboard { get; set; }
    public BasePlayer ClaimReskinningPlayer { get; set; }
    public Vector3 ReskinnedCupboardLastPosition { get; set; }
    public bool IsCupboardChangingSkin { get; set; }
    public Locker ArmoryLocker { get; set; }
    public int Level { get; set; }
    public bool HasRecruit;
    public string RecruitSpawnPosition;
    public MapMarkerGenericRadius mapMarker;
    public VendingMachineMapMarker hqMarker;
    public MapMarkerGenericRadius hqMarkerColor;
    public BaseEntity textMarker;
    public bool IsClaimed { get; set; }
    public bool IsTaxableClaim { get; set; }
    public bool IsWarZone { get; set; }
    public bool IsHostile { get; set; }
    public int UpgradeCost { get; set; }
    public void Init(string id, int row, int col, Vector3 position, Vector3 size, AreaInfo info);
    private void Awake();
    private void OnDestroy();
    private void TryLoadInfo(AreaInfo info);
    private void CheckClaimCupboard();
    private void CheckArmoryLocker();
    private void OnTriggerEnter(Collider collider);
    private void OnTriggerExit(Collider collider);
    public void UpdateAreaMarker(FactionColorPicker colorPicker);
    public float GetDistanceFromEntity(BaseEntity entity);
    public int GetClaimCost(Faction faction);
    public float GetDefensiveBonus();
    public float GetTaxRate();
    public War[] GetActiveWars();
    private float GetRatio(int level, int maxLevel, float maxBonus);
    public float GetLevelDefensiveBonus();
    public float GetLevelDecayReduction();
    public float GetLevelTaxBonus();
    public AreaInfo Serialize();
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    public class AreaInfo
    {
        [JsonProperty("id")]
        public string Id;
        [JsonProperty("name")]
        public string Name;
        [JsonProperty("type"), JsonConverter(typeof(StringEnumConverter))]
        public AreaType Type;
        [JsonProperty("factionId")]
        public string FactionId;
        [JsonProperty("claimantId")]
        public string ClaimantId;
        [JsonProperty("cupboardId")]
        public ulong? CupboardId;
        [JsonProperty("armoryId")]
        public ulong? ArmoryId;
        [JsonProperty("level")]
        public int Level;
        [JsonProperty("hasRecruit")]
        public bool HasRecruit;
        [JsonProperty("recruitSpawnPosition")]
        public string RecruitSpawnPosition;
    }

}

public class AreaInfo
{
    [JsonProperty("id")]
    public string Id;
    [JsonProperty("name")]
    public string Name;
    [JsonProperty("type"), JsonConverter(typeof(StringEnumConverter))]
    public AreaType Type;
    [JsonProperty("factionId")]
    public string FactionId;
    [JsonProperty("claimantId")]
    public string ClaimantId;
    [JsonProperty("cupboardId")]
    public ulong? CupboardId;
    [JsonProperty("armoryId")]
    public ulong? ArmoryId;
    [JsonProperty("level")]
    public int Level;
    [JsonProperty("hasRecruit")]
    public bool HasRecruit;
    [JsonProperty("recruitSpawnPosition")]
    public string RecruitSpawnPosition;
}

Oxide.Plugins
public partial class Imperium
{
    private class AreaManager
    {
        private Dictionary<string, Area> Areas;
        private Area[,] Layout;
        public MapGrid MapGrid { get; set; }
        public int Count { get; set; }
        public AreaManager();
        public Area Get(string areaId);
        public Area Get(int row, int col);
        public Area[] GetAll();
        public Area[] GetAllByType(AreaType type);
        public Area[] GetAllClaimedByFaction(Faction faction);
        public Area[] GetAllClaimedByFaction(string factionId);
        public Area[] GetAllTaxableClaimsByFaction(Faction faction);
        public Area[] GetAllTaxableClaimsByFaction(string factionId);
        public Area GetByClaimCupboard(BuildingPrivlidge cupboard);
        public Area GetByClaimReskinningPlayer(BasePlayer player);
        public Area GetByReskinnedCupboardLastPosition(Vector3 testPosition);
        private Vector3 Abs(Vector3 v);
        private bool IsApproximatedly(Vector3 v1, Vector3 v2);
        public Area GetByClaimCupboard(ulong cupboardId);
        public Area GetByArmoryLocker(Locker locker);
        public Area GetByArmoryLocker(ulong lockerId);
        public Area GetByEntityPosition(BaseEntity entity);
        public Area GetByWorldPosition(Vector3 position);
        public void Claim(Area area, AreaType type, Faction faction, User claimant, BuildingPrivlidge cupboard);
        public void SetHeadquarters(Area area, Faction faction);
        public void Unclaim(IEnumerable<Area> areas);
        public void Unclaim(Area[] areas);
        public void SetArmory(Area area, Locker locker);
        public void RemoveArmory(Area area);
        public void AddBadlands(Area[] areas);
        public void AddBadlands(IEnumerable<Area> areas);
        public int GetNumberOfContiguousClaimedAreas(Area area, Faction owner);
        public int GetDepthInsideFriendlyTerritory(Area area);
        public void Init(IEnumerable<AreaInfo> areaInfos);
        public void Destroy();
        public AreaInfo[] Serialize();
        public void UpdateAreaMarkers();
        public void DestroyAreaMarkers();
    }

}

private class AreaManager
{
    private Dictionary<string, Area> Areas;
    private Area[,] Layout;
    public MapGrid MapGrid { get; set; }
    public int Count { get; set; }
    public AreaManager();
    public Area Get(string areaId);
    public Area Get(int row, int col);
    public Area[] GetAll();
    public Area[] GetAllByType(AreaType type);
    public Area[] GetAllClaimedByFaction(Faction faction);
    public Area[] GetAllClaimedByFaction(string factionId);
    public Area[] GetAllTaxableClaimsByFaction(Faction faction);
    public Area[] GetAllTaxableClaimsByFaction(string factionId);
    public Area GetByClaimCupboard(BuildingPrivlidge cupboard);
    public Area GetByClaimReskinningPlayer(BasePlayer player);
    public Area GetByReskinnedCupboardLastPosition(Vector3 testPosition);
    private Vector3 Abs(Vector3 v);
    private bool IsApproximatedly(Vector3 v1, Vector3 v2);
    public Area GetByClaimCupboard(ulong cupboardId);
    public Area GetByArmoryLocker(Locker locker);
    public Area GetByArmoryLocker(ulong lockerId);
    public Area GetByEntityPosition(BaseEntity entity);
    public Area GetByWorldPosition(Vector3 position);
    public void Claim(Area area, AreaType type, Faction faction, User claimant, BuildingPrivlidge cupboard);
    public void SetHeadquarters(Area area, Faction faction);
    public void Unclaim(IEnumerable<Area> areas);
    public void Unclaim(Area[] areas);
    public void SetArmory(Area area, Locker locker);
    public void RemoveArmory(Area area);
    public void AddBadlands(Area[] areas);
    public void AddBadlands(IEnumerable<Area> areas);
    public int GetNumberOfContiguousClaimedAreas(Area area, Faction owner);
    public int GetDepthInsideFriendlyTerritory(Area area);
    public void Init(IEnumerable<AreaInfo> areaInfos);
    public void Destroy();
    public AreaInfo[] Serialize();
    public void UpdateAreaMarkers();
    public void DestroyAreaMarkers();
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
}

Oxide.Plugins
public partial class Imperium
{
    private class Faction
    {
        public string Id { get; set; }
        public string OwnerId { get; set; }
        public HashSet<string> MemberIds { get; set; }
        public HashSet<string> ManagerIds { get; set; }
        public HashSet<string> InviteIds { get; set; }
        public HashSet<string> Aggressors { get; set; }
        public ulong InGameTeamID { get; set; }
        public float TaxRate { get; set; }
        public StorageContainer TaxChest { get; set; }
        public DateTime NextUpkeepPaymentTime { get; set; }
        public bool IsUpkeepPastDue { get; set; }
        public bool IsBadlands { get; set; }
        public DateTime? BadlandsCommandUsedTime { get; set; }
        public DateTime CreationTime { get; set; }
        public bool CanCollectTaxes { get; set; }
        public int MemberCount { get; set; }
        public Faction(string id, User owner);
        public Faction(FactionInfo info);
        public bool AddMember(User user);
        public bool RemoveMember(User user);
        public bool AddInvite(User user);
        public bool RemoveInvite(User user);
        public bool Promote(User user);
        public bool Demote(User user);
        public bool SetBadlands(bool value);
        public bool HasOwner(User user);
        public bool HasOwner(string userId);
        public bool HasLeader(User user);
        public bool HasLeader(string userId);
        public bool HasManager(User user);
        public bool HasManager(string userId);
        public bool HasInvite(User user);
        public bool HasInvite(string userId);
        public bool HasMember(User user);
        public bool HasMember(string userId);
        public User[] GetAllActiveMembers();
        public User[] GetAllActiveInvitedUsers();
        public int GetUpkeepPerPeriod();
        public void SendChatMessage(string message, object[] args);
        public void CreateFactionTeam();
        private RelationshipManager.PlayerTeam GetFactionPlayerTeam();
        private RelationshipManager.PlayerTeam GetOwnerPlayerTeam();
        public FactionInfo Serialize();
    }

}

private class Faction
{
    public string Id { get; set; }
    public string OwnerId { get; set; }
    public HashSet<string> MemberIds { get; set; }
    public HashSet<string> ManagerIds { get; set; }
    public HashSet<string> InviteIds { get; set; }
    public HashSet<string> Aggressors { get; set; }
    public ulong InGameTeamID { get; set; }
    public float TaxRate { get; set; }
    public StorageContainer TaxChest { get; set; }
    public DateTime NextUpkeepPaymentTime { get; set; }
    public bool IsUpkeepPastDue { get; set; }
    public bool IsBadlands { get; set; }
    public DateTime? BadlandsCommandUsedTime { get; set; }
    public DateTime CreationTime { get; set; }
    public bool CanCollectTaxes { get; set; }
    public int MemberCount { get; set; }
    public Faction(string id, User owner);
    public Faction(FactionInfo info);
    public bool AddMember(User user);
    public bool RemoveMember(User user);
    public bool AddInvite(User user);
    public bool RemoveInvite(User user);
    public bool Promote(User user);
    public bool Demote(User user);
    public bool SetBadlands(bool value);
    public bool HasOwner(User user);
    public bool HasOwner(string userId);
    public bool HasLeader(User user);
    public bool HasLeader(string userId);
    public bool HasManager(User user);
    public bool HasManager(string userId);
    public bool HasInvite(User user);
    public bool HasInvite(string userId);
    public bool HasMember(User user);
    public bool HasMember(string userId);
    public User[] GetAllActiveMembers();
    public User[] GetAllActiveInvitedUsers();
    public int GetUpkeepPerPeriod();
    public void SendChatMessage(string message, object[] args);
    public void CreateFactionTeam();
    private RelationshipManager.PlayerTeam GetFactionPlayerTeam();
    private RelationshipManager.PlayerTeam GetOwnerPlayerTeam();
    public FactionInfo Serialize();
}

Oxide.Plugins
public partial class Imperium
{
    private class FactionEntityMonitor : MonoBehaviour
    {
        private void Awake();
        private void OnDestroy();
        private void EnsureAllTaxChestsStillExist();
        private void EnsureTaxChestExists(Faction faction);
    }

}

private class FactionEntityMonitor : MonoBehaviour
{
    private void Awake();
    private void OnDestroy();
    private void EnsureAllTaxChestsStillExist();
    private void EnsureTaxChestExists(Faction faction);
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class FactionInfo
    {
        [JsonProperty("id")]
        public string Id;
        [JsonProperty("ownerId")]
        public string OwnerId;
        [JsonProperty("memberIds")]
        public string[] MemberIds;
        [JsonProperty("managerIds")]
        public string[] ManagerIds;
        [JsonProperty("inviteIds")]
        public string[] InviteIds;
        [JsonProperty("aggressors")]
        public string[] Aggressors;
        [JsonProperty("inGameTeamID")]
        public ulong InGameTeamID;
        [JsonProperty("taxRate")]
        public float TaxRate;
        [JsonProperty("taxChestId")]
        public ulong? TaxChestId;
        [JsonProperty("nextUpkeepPaymentTime")]
        public DateTime NextUpkeepPaymentTime;
        [JsonProperty("isUpkeepPastDue")]
        public bool IsUpkeepPastDue;
        [JsonProperty("isBadlands")]
        public bool IsBadlands;
        [JsonProperty("badlandsCommandUsedTime"), JsonConverter(typeof(IsoDateTimeConverter))]
        public DateTime? BadlandsCommandUsedTime;
        [JsonProperty("creationTime"), JsonConverter(typeof(IsoDateTimeConverter))]
        public DateTime CreationTime;
    }

}

private class FactionInfo
{
    [JsonProperty("id")]
    public string Id;
    [JsonProperty("ownerId")]
    public string OwnerId;
    [JsonProperty("memberIds")]
    public string[] MemberIds;
    [JsonProperty("managerIds")]
    public string[] ManagerIds;
    [JsonProperty("inviteIds")]
    public string[] InviteIds;
    [JsonProperty("aggressors")]
    public string[] Aggressors;
    [JsonProperty("inGameTeamID")]
    public ulong InGameTeamID;
    [JsonProperty("taxRate")]
    public float TaxRate;
    [JsonProperty("taxChestId")]
    public ulong? TaxChestId;
    [JsonProperty("nextUpkeepPaymentTime")]
    public DateTime NextUpkeepPaymentTime;
    [JsonProperty("isUpkeepPastDue")]
    public bool IsUpkeepPastDue;
    [JsonProperty("isBadlands")]
    public bool IsBadlands;
    [JsonProperty("badlandsCommandUsedTime"), JsonConverter(typeof(IsoDateTimeConverter))]
    public DateTime? BadlandsCommandUsedTime;
    [JsonProperty("creationTime"), JsonConverter(typeof(IsoDateTimeConverter))]
    public DateTime CreationTime;
}

Oxide.Plugins
public partial class Imperium
{
    private class FactionManager
    {
        private Dictionary<string, Faction> Factions;
        private FactionEntityMonitor EntityMonitor;
        public FactionManager();
        public Faction Create(string id, User owner);
        public void Disband(Faction faction);
        public Faction[] GetAll();
        public Faction Get(string id);
        public bool Exists(string id);
        public Faction GetByMember(User user);
        public Faction GetByMember(string userId);
        public Faction GetByTaxChest(StorageContainer container);
        public Faction GetByTaxChest(ulong containerId);
        public void SetTaxRate(Faction faction, float taxRate);
        public void SetTaxChest(Faction faction, StorageContainer taxChest);
        public void Init(IEnumerable<FactionInfo> factionInfos);
        public void Destroy();
        public FactionInfo[] Serialize();
        internal void SyncAllWithClans();
    }

}

private class FactionManager
{
    private Dictionary<string, Faction> Factions;
    private FactionEntityMonitor EntityMonitor;
    public FactionManager();
    public Faction Create(string id, User owner);
    public void Disband(Faction faction);
    public Faction[] GetAll();
    public Faction Get(string id);
    public bool Exists(string id);
    public Faction GetByMember(User user);
    public Faction GetByMember(string userId);
    public Faction GetByTaxChest(StorageContainer container);
    public Faction GetByTaxChest(ulong containerId);
    public void SetTaxRate(Faction faction, float taxRate);
    public void SetTaxChest(Faction faction, StorageContainer taxChest);
    public void Init(IEnumerable<FactionInfo> factionInfos);
    public void Destroy();
    public FactionInfo[] Serialize();
    internal void SyncAllWithClans();
}

Oxide.Plugins
public partial class Imperium
{
    private class Pin
    {
        public Vector3 Position { get; set; }
        public string AreaId { get; set; }
        public string CreatorId { get; set; }
        public PinType Type { get; set; }
        public string Name { get; set; }
        public Pin(Vector3 position, Area area, User creator, PinType type, string name);
        public Pin(PinInfo info);
        public float GetDistanceFrom(BaseEntity entity);
        public float GetDistanceFrom(Vector3 position);
        public PinInfo Serialize();
    }

}

private class Pin
{
    public Vector3 Position { get; set; }
    public string AreaId { get; set; }
    public string CreatorId { get; set; }
    public PinType Type { get; set; }
    public string Name { get; set; }
    public Pin(Vector3 position, Area area, User creator, PinType type, string name);
    public Pin(PinInfo info);
    public float GetDistanceFrom(BaseEntity entity);
    public float GetDistanceFrom(Vector3 position);
    public PinInfo Serialize();
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class PinInfo
    {
        [JsonProperty("name")]
        public string Name;
        [JsonProperty("type"), JsonConverter(typeof(StringEnumConverter))]
        public PinType Type;
        [JsonProperty("position"), JsonConverter(typeof(UnityVector3Converter))]
        public Vector3 Position;
        [JsonProperty("areaId")]
        public string AreaId;
        [JsonProperty("creatorId")]
        public string CreatorId;
    }

}

private class PinInfo
{
    [JsonProperty("name")]
    public string Name;
    [JsonProperty("type"), JsonConverter(typeof(StringEnumConverter))]
    public PinType Type;
    [JsonProperty("position"), JsonConverter(typeof(UnityVector3Converter))]
    public Vector3 Position;
    [JsonProperty("areaId")]
    public string AreaId;
    [JsonProperty("creatorId")]
    public string CreatorId;
}

Oxide.Plugins
public partial class Imperium
{
    private class PinManager
    {
        private Dictionary<string, Pin> Pins;
        public PinManager();
        public Pin Get(string name);
        public Pin[] GetAll();
        public void Add(Pin pin);
        public void Remove(Pin pin);
        public void RemoveAllPinsInUnclaimedAreas();
        public void Init(IEnumerable<PinInfo> pinInfos);
        public void Destroy();
        public PinInfo[] Serialize();
    }

}

private class PinManager
{
    private Dictionary<string, Pin> Pins;
    public PinManager();
    public Pin Get(string name);
    public Pin[] GetAll();
    public void Add(Pin pin);
    public void Remove(Pin pin);
    public void RemoveAllPinsInUnclaimedAreas();
    public void Init(IEnumerable<PinInfo> pinInfos);
    public void Destroy();
    public PinInfo[] Serialize();
}

Oxide.Plugins
public partial class Imperium
{
}

Oxide.Plugins
public partial class Imperium
{
    private class User : MonoBehaviour
    {
        private string OriginalName;
        private Dictionary<string, DateTime> CommandCooldownExpirations;
        public BasePlayer Player { get; set; }
        public UserMap Map { get; set; }
        public UserHud Hud { get; set; }
        public UserPanel Panel { get; set; }
        public UserPreferences Preferences { get; set; }
        public Area CurrentArea { get; set; }
        public HashSet<Zone> CurrentZones { get; set; }
        public Faction Faction { get; set; }
        public Interaction CurrentInteraction { get; set; }
        public DateTime MapCommandCooldownExpiration { get; set; }
        public DateTime PvpCommandCooldownExpiration { get; set; }
        public bool IsInPvpMode { get; set; }
        public bool IsInsideRaidableBase { get; set; }
        public bool UpdatedMarkers;
        public string Id { get; set; }
        public string UserName { get; set; }
        public string UserNameWithFactionTag { get; set; }
        public void Init(BasePlayer player);
        private void OnDestroy();
        public void SetFaction(Faction faction);
        public bool HasPermission(string permission);
        public void BeginInteraction(Interaction interaction);
        public void CompleteInteraction(HitInfo hit);
        public void CancelInteraction();
        public void SendChatMessage(string message, object[] args);
        public void SendChatMessage(StringBuilder sb);
        public void SendConsoleMessage(string message, object[] args);
        private void UpdateHud();
        public int GetSecondsLeftOnCooldown(string command);
        public void SetCooldownExpiration(string command, DateTime time);
        private void CheckArea();
        public void EnsureIsInFactionTeam();
        public void SyncWithClan();
        private void CheckZones();
    }

}

private class User : MonoBehaviour
{
    private string OriginalName;
    private Dictionary<string, DateTime> CommandCooldownExpirations;
    public BasePlayer Player { get; set; }
    public UserMap Map { get; set; }
    public UserHud Hud { get; set; }
    public UserPanel Panel { get; set; }
    public UserPreferences Preferences { get; set; }
    public Area CurrentArea { get; set; }
    public HashSet<Zone> CurrentZones { get; set; }
    public Faction Faction { get; set; }
    public Interaction CurrentInteraction { get; set; }
    public DateTime MapCommandCooldownExpiration { get; set; }
    public DateTime PvpCommandCooldownExpiration { get; set; }
    public bool IsInPvpMode { get; set; }
    public bool IsInsideRaidableBase { get; set; }
    public bool UpdatedMarkers;
    public string Id { get; set; }
    public string UserName { get; set; }
    public string UserNameWithFactionTag { get; set; }
    public void Init(BasePlayer player);
    private void OnDestroy();
    public void SetFaction(Faction faction);
    public bool HasPermission(string permission);
    public void BeginInteraction(Interaction interaction);
    public void CompleteInteraction(HitInfo hit);
    public void CancelInteraction();
    public void SendChatMessage(string message, object[] args);
    public void SendChatMessage(StringBuilder sb);
    public void SendConsoleMessage(string message, object[] args);
    private void UpdateHud();
    public int GetSecondsLeftOnCooldown(string command);
    public void SetCooldownExpiration(string command, DateTime time);
    private void CheckArea();
    public void EnsureIsInFactionTeam();
    public void SyncWithClan();
    private void CheckZones();
}

Oxide.Plugins
public partial class Imperium
{
    private class UserManager
    {
        private Dictionary<string, User> Users;
        private Dictionary<string, string> OriginalNames;
        public User[] GetAll();
        public User Get(BasePlayer player);
        public User Get(string userId);
        public User Find(string searchString);
        public User Add(BasePlayer player);
        public bool Remove(BasePlayer player);
        public void SetOriginalName(string userId, string name);
        public void Init();
        public void Destroy();
    }

}

private class UserManager
{
    private Dictionary<string, User> Users;
    private Dictionary<string, string> OriginalNames;
    public User[] GetAll();
    public User Get(BasePlayer player);
    public User Get(string userId);
    public User Find(string searchString);
    public User Add(BasePlayer player);
    public bool Remove(BasePlayer player);
    public void SetOriginalName(string userId, string name);
    public void Init();
    public void Destroy();
}

Oxide.Plugins
public partial class Imperium
{
    private class UserPreferences
    {
        public UserMapLayer VisibleMapLayers { get; set; }
        public void ShowMapLayer(UserMapLayer layer);
        public void HideMapLayer(UserMapLayer layer);
        public void ToggleMapLayer(UserMapLayer layer);
        public bool IsMapLayerVisible(UserMapLayer layer);
        public static UserPreferences Default { get; set; }
    }

}

private class UserPreferences
{
    public UserMapLayer VisibleMapLayers { get; set; }
    public void ShowMapLayer(UserMapLayer layer);
    public void HideMapLayer(UserMapLayer layer);
    public void ToggleMapLayer(UserMapLayer layer);
    public bool IsMapLayerVisible(UserMapLayer layer);
    public static UserPreferences Default { get; set; }
}

Oxide.Plugins
public partial class Imperium
{
    private class War
    {
        public string AttackerId { get; set; }
        public string DefenderId { get; set; }
        public string DeclarerId { get; set; }
        public string CassusBelli { get; set; }
        public DateTime? AttackerPeaceOfferingTime { get; set; }
        public DateTime? DefenderPeaceOfferingTime { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime? EndTime { get; set; }
        public WarEndReason? EndReason { get; set; }
        public bool AdminApproved { get; set; }
        public bool DefenderApproved { get; set; }
        public bool IsActive { get; set; }
        public bool IsAttackerOfferingPeace { get; set; }
        public bool IsDefenderOfferingPeace { get; set; }
        public War(Faction attacker, Faction defender, User declarer, string cassusBelli);
        public War(WarInfo info);
        public void OfferPeace(Faction faction);
        public bool IsOfferingPeace(Faction faction);
        public bool IsOfferingPeace(string factionId);
        public WarInfo Serialize();
    }

}

private class War
{
    public string AttackerId { get; set; }
    public string DefenderId { get; set; }
    public string DeclarerId { get; set; }
    public string CassusBelli { get; set; }
    public DateTime? AttackerPeaceOfferingTime { get; set; }
    public DateTime? DefenderPeaceOfferingTime { get; set; }
    public DateTime StartTime { get; set; }
    public DateTime? EndTime { get; set; }
    public WarEndReason? EndReason { get; set; }
    public bool AdminApproved { get; set; }
    public bool DefenderApproved { get; set; }
    public bool IsActive { get; set; }
    public bool IsAttackerOfferingPeace { get; set; }
    public bool IsDefenderOfferingPeace { get; set; }
    public War(Faction attacker, Faction defender, User declarer, string cassusBelli);
    public War(WarInfo info);
    public void OfferPeace(Faction faction);
    public bool IsOfferingPeace(Faction faction);
    public bool IsOfferingPeace(string factionId);
    public WarInfo Serialize();
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
}

Oxide.Plugins
public partial class Imperium
{
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class WarInfo
    {
        [JsonProperty("attackerId")]
        public string AttackerId;
        [JsonProperty("defenderId")]
        public string DefenderId;
        [JsonProperty("declarerId")]
        public string DeclarerId;
        [JsonProperty("cassusBelli")]
        public string CassusBelli;
        [JsonProperty("adminApproved")]
        public bool AdminApproved;
        [JsonProperty("defenderApproved")]
        public bool DefenderApproved;
        [JsonProperty("attackerPeaceOfferingTime"), JsonConverter(typeof(IsoDateTimeConverter))]
        public DateTime? AttackerPeaceOfferingTime;
        [JsonProperty("defenderPeaceOfferingTime"), JsonConverter(typeof(IsoDateTimeConverter))]
        public DateTime? DefenderPeaceOfferingTime;
        [JsonProperty("startTime"), JsonConverter(typeof(IsoDateTimeConverter))]
        public DateTime StartTime;
        [JsonProperty("endTime"), JsonConverter(typeof(IsoDateTimeConverter))]
        public DateTime? EndTime;
        [JsonProperty("endReason"), JsonConverter(typeof(StringEnumConverter))]
        public WarEndReason? EndReason;
    }

}

private class WarInfo
{
    [JsonProperty("attackerId")]
    public string AttackerId;
    [JsonProperty("defenderId")]
    public string DefenderId;
    [JsonProperty("declarerId")]
    public string DeclarerId;
    [JsonProperty("cassusBelli")]
    public string CassusBelli;
    [JsonProperty("adminApproved")]
    public bool AdminApproved;
    [JsonProperty("defenderApproved")]
    public bool DefenderApproved;
    [JsonProperty("attackerPeaceOfferingTime"), JsonConverter(typeof(IsoDateTimeConverter))]
    public DateTime? AttackerPeaceOfferingTime;
    [JsonProperty("defenderPeaceOfferingTime"), JsonConverter(typeof(IsoDateTimeConverter))]
    public DateTime? DefenderPeaceOfferingTime;
    [JsonProperty("startTime"), JsonConverter(typeof(IsoDateTimeConverter))]
    public DateTime StartTime;
    [JsonProperty("endTime"), JsonConverter(typeof(IsoDateTimeConverter))]
    public DateTime? EndTime;
    [JsonProperty("endReason"), JsonConverter(typeof(StringEnumConverter))]
    public WarEndReason? EndReason;
}

Oxide.Plugins
public partial class Imperium
{
    private class WarManager
    {
        private List<War> Wars;
        public War[] GetAllActiveWars();
        public War[] GetAllInactiveWars();
        public War[] GetAllActiveWarsByFaction(Faction faction);
        public War[] GetAllAdminUnnaprovedWars();
        public War[] GetAllUnapprovedWarsByFaction(Faction faction);
        public War[] GetAllActiveWarsByFaction(string factionId);
        public War GetActiveWarBetween(Faction firstFaction, Faction secondFaction);
        public War GetActiveWarBetween(string firstFactionId, string secondFactionId);
        public bool AreFactionsAtWar(Faction firstFaction, Faction secondFaction);
        public bool AreFactionsAtWar(string firstFactionId, string secondFactionId);
        public War DeclareWar(Faction attacker, Faction defender, User user, string cassusBelli);
        public bool TryShopfrontTreaty(BasePlayer player1, BasePlayer player2);
        public void AdminApproveWar(War war);
        public void AdminDenyeWar(War war);
        public void DefenderApproveWar(War war);
        public void DefenderDenyWar(War war);
        public void EndWar(War war, WarEndReason reason);
        public void EndAllWarsForEliminatedFactions();
        public void Init(IEnumerable<WarInfo> warInfos);
        public void Destroy();
        public WarInfo[] Serialize();
    }

}

private class WarManager
{
    private List<War> Wars;
    public War[] GetAllActiveWars();
    public War[] GetAllInactiveWars();
    public War[] GetAllActiveWarsByFaction(Faction faction);
    public War[] GetAllAdminUnnaprovedWars();
    public War[] GetAllUnapprovedWarsByFaction(Faction faction);
    public War[] GetAllActiveWarsByFaction(string factionId);
    public War GetActiveWarBetween(Faction firstFaction, Faction secondFaction);
    public War GetActiveWarBetween(string firstFactionId, string secondFactionId);
    public bool AreFactionsAtWar(Faction firstFaction, Faction secondFaction);
    public bool AreFactionsAtWar(string firstFactionId, string secondFactionId);
    public War DeclareWar(Faction attacker, Faction defender, User user, string cassusBelli);
    public bool TryShopfrontTreaty(BasePlayer player1, BasePlayer player2);
    public void AdminApproveWar(War war);
    public void AdminDenyeWar(War war);
    public void DefenderApproveWar(War war);
    public void DefenderDenyWar(War war);
    public void EndWar(War war, WarEndReason reason);
    public void EndAllWarsForEliminatedFactions();
    public void Init(IEnumerable<WarInfo> warInfos);
    public void Destroy();
    public WarInfo[] Serialize();
}

Oxide.Plugins
public partial class Imperium
{
    private class Zone : MonoBehaviour
    {
        private const string SpherePrefab;
        private List<BaseEntity> Spheres;
        public ZoneType Type { get; set; }
        public string Name { get; set; }
        public MonoBehaviour Owner { get; set; }
        public DateTime? EndTime { get; set; }
        public void Init(ZoneType type, string name, MonoBehaviour owner, float radius, int darkness, DateTime? endTime);
        private void OnDestroy();
        private void OnTriggerEnter(Collider collider);
        private void OnTriggerExit(Collider collider);
        private void CheckIfShouldDestroy();
        private Vector3 GetGroundPosition(Vector3 pos);
    }

}

private class Zone : MonoBehaviour
{
    private const string SpherePrefab;
    private List<BaseEntity> Spheres;
    public ZoneType Type { get; set; }
    public string Name { get; set; }
    public MonoBehaviour Owner { get; set; }
    public DateTime? EndTime { get; set; }
    public void Init(ZoneType type, string name, MonoBehaviour owner, float radius, int darkness, DateTime? endTime);
    private void OnDestroy();
    private void OnTriggerEnter(Collider collider);
    private void OnTriggerExit(Collider collider);
    private void CheckIfShouldDestroy();
    private Vector3 GetGroundPosition(Vector3 pos);
}

Oxide.Plugins
public partial class Imperium
{
    private class ZoneManager
    {
        private Dictionary<MonoBehaviour, Zone> Zones;
        public void Init();
        public Zone GetByOwner(MonoBehaviour owner);
        public void CreateForDebrisField(BaseHelicopter helicopter);
        public void CreateForSupplyDrop(SupplyDrop drop);
        public void CreateForCargoShip(CargoShip cargoShip);
        public void CreateForRaid(BuildingPrivlidge cupboard);
        public void Remove(Zone zone);
        public void Destroy();
        private void Create(ZoneType type, string name, MonoBehaviour owner, float radius, DateTime? endTime);
        private float? GetMonumentZoneRadius(MonumentInfo monument);
        private DateTime GetEventEndTime();
    }

}

private class ZoneManager
{
    private Dictionary<MonoBehaviour, Zone> Zones;
    public void Init();
    public Zone GetByOwner(MonoBehaviour owner);
    public void CreateForDebrisField(BaseHelicopter helicopter);
    public void CreateForSupplyDrop(SupplyDrop drop);
    public void CreateForCargoShip(CargoShip cargoShip);
    public void CreateForRaid(BuildingPrivlidge cupboard);
    public void Remove(Zone zone);
    public void Destroy();
    private void Create(ZoneType type, string name, MonoBehaviour owner, float radius, DateTime? endTime);
    private float? GetMonumentZoneRadius(MonumentInfo monument);
    private DateTime GetEventEndTime();
}

Oxide.Plugins
public partial class Imperium
{
}

Oxide.Plugins
public partial class Imperium
{
    public class Recruit : MonoBehaviour
    {
    }

}

public class Recruit : MonoBehaviour
{
}

Oxide.Plugins
public partial class Imperium
{
    public class RecruitInfo
    {
    }

}

public class RecruitInfo
{
}

Oxide.Plugins
public partial class Imperium
{
    public class RecruitManager
    {
    }

}

public class RecruitManager
{
}

Oxide.Plugins
public partial class Imperium
{
    private class FactionColorPicker
    {
        private static string[] Colors;
        private Dictionary<string, Color> AssignedColors;
        private int NextColor;
        public FactionColorPicker();
        public Color GetColorForFaction(string factionId);
        public string GetHexColorForFaction(string factionId);
    }

}

private class FactionColorPicker
{
    private static string[] Colors;
    private Dictionary<string, Color> AssignedColors;
    private int NextColor;
    public FactionColorPicker();
    public Color GetColorForFaction(string factionId);
    public string GetHexColorForFaction(string factionId);
}

Oxide.Plugins
public partial class Imperium
{
    public class MapGrid
    {
        public float GRID_CELL_SIZE;
        public float MapSize { get; set; }
        public float MapWidth { get; set; }
        public float MapHeight { get; set; }
        public float MapOffsetX { get; set; }
        public float MapOffsetZ { get; set; }
        public float CellSize { get; set; }
        public float CellSizeRatio { get; set; }
        public float CellSizeRatioWidth { get; set; }
        public float CellSizeRatioHeight { get; set; }
        public int NumberOfCells { get; set; }
        public int NumberOfColumns { get; set; }
        public int NumberOfRows { get; set; }
        private string[] RowIds;
        private string[] ColumnIds;
        private string[,] AreaIds;
        private Vector3[,] Positions;
        public MapGrid();
        public string GetRowId(int row);
        public string GetColumnId(int col);
        public string GetAreaId(int row, int col);
        public Vector3 GetPosition(int row, int col);
        private void Build();
    }

}

public class MapGrid
{
    public float GRID_CELL_SIZE;
    public float MapSize { get; set; }
    public float MapWidth { get; set; }
    public float MapHeight { get; set; }
    public float MapOffsetX { get; set; }
    public float MapOffsetZ { get; set; }
    public float CellSize { get; set; }
    public float CellSizeRatio { get; set; }
    public float CellSizeRatioWidth { get; set; }
    public float CellSizeRatioHeight { get; set; }
    public int NumberOfCells { get; set; }
    public int NumberOfColumns { get; set; }
    public int NumberOfRows { get; set; }
    private string[] RowIds;
    private string[] ColumnIds;
    private string[,] AreaIds;
    private Vector3[,] Positions;
    public MapGrid();
    public string GetRowId(int row);
    public string GetColumnId(int col);
    public string GetAreaId(int row, int col);
    public Vector3 GetPosition(int row, int col);
    private void Build();
}

Oxide.Plugins
public partial class Imperium
{
    private class MapOverlayGenerator : UnityEngine.MonoBehaviour
    {
        public bool IsGenerating { get; set; }
        public void Generate();
        private IEnumerator GenerateOverlayImage();
    }

}

private class MapOverlayGenerator : UnityEngine.MonoBehaviour
{
    public bool IsGenerating { get; set; }
    public void Generate();
    private IEnumerator GenerateOverlayImage();
}

Oxide.Plugins
public partial class Imperium
{
    public static class MonumentPrefab
    {
        private const string PrefabPrefix;
        public const string Airfield;
        public const string BanditTown;
        public const string Compound;
        public const string Dome;
        public const string Harbor1;
        public const string Harbor2;
        public const string GasStation;
        public const string Junkyard;
        public const string LaunchSite;
        public const string Lighthouse;
        public const string MilitaryTunnel;
        public const string MiningOutpost;
        public const string QuarryStone;
        public const string QuarrySulfur;
        public const string QuaryHqm;
        public const string PowerPlant;
        public const string Trainyard;
        public const string SatelliteDish;
        public const string SewerBranch;
        public const string Supermarket;
        public const string WaterTreatmentPlant;
        public const string WaterWellA;
        public const string WaterWellB;
        public const string WaterWellC;
        public const string WaterWellD;
        public const string WaterWellE;
    }

}

public static class MonumentPrefab
{
    private const string PrefabPrefix;
    public const string Airfield;
    public const string BanditTown;
    public const string Compound;
    public const string Dome;
    public const string Harbor1;
    public const string Harbor2;
    public const string GasStation;
    public const string Junkyard;
    public const string LaunchSite;
    public const string Lighthouse;
    public const string MilitaryTunnel;
    public const string MiningOutpost;
    public const string QuarryStone;
    public const string QuarrySulfur;
    public const string QuaryHqm;
    public const string PowerPlant;
    public const string Trainyard;
    public const string SatelliteDish;
    public const string SewerBranch;
    public const string Supermarket;
    public const string WaterTreatmentPlant;
    public const string WaterWellA;
    public const string WaterWellB;
    public const string WaterWellC;
    public const string WaterWellD;
    public const string WaterWellE;
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    public static class Permission
    {
        public const string AdminFactions;
        public const string AdminClaims;
        public const string AdminBadlands;
        public const string AdminWars;
        public const string AdminPins;
        public const string ManageFactions;
        public static void RegisterAll(Imperium instance);
    }

}

public static class Permission
{
    public const string AdminFactions;
    public const string AdminClaims;
    public const string AdminBadlands;
    public const string AdminWars;
    public const string AdminPins;
    public const string ManageFactions;
    public static void RegisterAll(Imperium instance);
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class UnityVector3Converter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

}

private class UnityVector3Converter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}

Oxide.Plugins
public partial class Imperium
{
    private static class Util
    {
        private const string NullString;
        public static string Format(object obj);
        public static string Format(User user);
        public static string Format(Area area);
        public static string Format(BaseEntity entity);
        public static string Format(IEnumerable items);
        public static string NormalizeAreaId(string input);
        public static string NormalizeAreaName(string input);
        public static string NormalizePinName(string input);
        public static string NormalizeFactionId(string input);
        public static string RemoveSpecialCharacters(string str);
        public static int GetLevenshteinDistance(string source, string target);
        public static bool TryParseEnum(string str, T value);
        public static void RunEffect(Vector3 position, string prefab, BasePlayer player);
        public static void BroadcastEffect(string prefab);
        public static void PrintToChat(string format, object[] args);
        public static int GetSecondsBetween(DateTime start, DateTime end);
        public static Color ConvertSystemToUnityColor(System.Drawing.Color color);
    }

}

private static class Util
{
    private const string NullString;
    public static string Format(object obj);
    public static string Format(User user);
    public static string Format(Area area);
    public static string Format(BaseEntity entity);
    public static string Format(IEnumerable items);
    public static string NormalizeAreaId(string input);
    public static string NormalizeAreaName(string input);
    public static string NormalizePinName(string input);
    public static string NormalizeFactionId(string input);
    public static string RemoveSpecialCharacters(string str);
    public static int GetLevenshteinDistance(string source, string target);
    public static bool TryParseEnum(string str, T value);
    public static void RunEffect(Vector3 position, string prefab, BasePlayer player);
    public static void BroadcastEffect(string prefab);
    public static void PrintToChat(string format, object[] args);
    public static int GetSecondsBetween(DateTime start, DateTime end);
    public static Color ConvertSystemToUnityColor(System.Drawing.Color color);
}

Oxide.Plugins
public partial class Imperium
{
    private class AddingClaimInteraction : Interaction
    {
        public Faction Faction { get; set; }
        public AddingClaimInteraction(Faction faction);
        public override bool TryComplete(HitInfo hit);
    }

}

private class AddingClaimInteraction : Interaction
{
    public Faction Faction { get; set; }
    public AddingClaimInteraction(Faction faction);
    public override bool TryComplete(HitInfo hit);
}

Oxide.Plugins
public partial class Imperium
{
    private class AssigningClaimInteraction : Interaction
    {
        public Faction Faction { get; set; }
        public AssigningClaimInteraction(Faction faction);
        public override bool TryComplete(HitInfo hit);
    }

}

private class AssigningClaimInteraction : Interaction
{
    public Faction Faction { get; set; }
    public AssigningClaimInteraction(Faction faction);
    public override bool TryComplete(HitInfo hit);
}

Oxide.Plugins
public partial class Imperium
{
    private abstract class Interaction
    {
        public User User { get; set; }
        public abstract bool TryComplete(HitInfo hit);
    }

}

private abstract class Interaction
{
    public User User { get; set; }
    public abstract bool TryComplete(HitInfo hit);
}

Oxide.Plugins
public partial class Imperium
{
    private class RemovingClaimInteraction : Interaction
    {
        public Faction Faction { get; set; }
        public RemovingClaimInteraction(Faction faction);
        public override bool TryComplete(HitInfo hit);
    }

}

private class RemovingClaimInteraction : Interaction
{
    public Faction Faction { get; set; }
    public RemovingClaimInteraction(Faction faction);
    public override bool TryComplete(HitInfo hit);
}

Oxide.Plugins
public partial class Imperium
{
    private class SelectingHeadquartersInteraction : Interaction
    {
        public Faction Faction { get; set; }
        public SelectingHeadquartersInteraction(Faction faction);
        public override bool TryComplete(HitInfo hit);
    }

}

private class SelectingHeadquartersInteraction : Interaction
{
    public Faction Faction { get; set; }
    public SelectingHeadquartersInteraction(Faction faction);
    public override bool TryComplete(HitInfo hit);
}

Oxide.Plugins
public partial class Imperium
{
    private class SelectingTaxChestInteraction : Interaction
    {
        public Faction Faction { get; set; }
        public SelectingTaxChestInteraction(Faction faction);
        public override bool TryComplete(HitInfo hit);
    }

}

private class SelectingTaxChestInteraction : Interaction
{
    public Faction Faction { get; set; }
    public SelectingTaxChestInteraction(Faction faction);
    public override bool TryComplete(HitInfo hit);
}

Oxide.Plugins
public partial class Imperium
{
    private class SelectingArmoryLockerInteraction : Interaction
    {
        public Faction Faction { get; set; }
        public SelectingArmoryLockerInteraction(Faction faction);
        public override bool TryComplete(HitInfo hit);
    }

}

private class SelectingArmoryLockerInteraction : Interaction
{
    public Faction Faction { get; set; }
    public SelectingArmoryLockerInteraction(Faction faction);
    public override bool TryComplete(HitInfo hit);
}

Oxide.Plugins
public partial class Imperium
{
    private class TransferringClaimInteraction : Interaction
    {
        public Faction SourceFaction { get; set; }
        public Faction TargetFaction { get; set; }
        public TransferringClaimInteraction(Faction sourceFaction, Faction targetFaction);
        public override bool TryComplete(HitInfo hit);
    }

}

private class TransferringClaimInteraction : Interaction
{
    public Faction SourceFaction { get; set; }
    public Faction TargetFaction { get; set; }
    public TransferringClaimInteraction(Faction sourceFaction, Faction targetFaction);
    public override bool TryComplete(HitInfo hit);
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class BadlandsOptions
    {
        [JsonProperty("enabled")]
        public bool Enabled;
        public static BadlandsOptions Default;
    }

}

private class BadlandsOptions
{
    [JsonProperty("enabled")]
    public bool Enabled;
    public static BadlandsOptions Default;
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class ClaimOptions
    {
        [JsonProperty("enabled")]
        public bool Enabled;
        [JsonProperty("costs")]
        public List<int> Costs;
        [JsonProperty("maxClaims")]
        public int? MaxClaims;
        [JsonProperty("minAreaNameLength")]
        public int MinAreaNameLength;
        [JsonProperty("maxAreaNameLength")]
        public int MaxAreaNameLength;
        [JsonProperty("minFactionMembers")]
        public int MinFactionMembers;
        [JsonProperty("requireContiguousClaims")]
        public bool RequireContiguousClaims;
        public static ClaimOptions Default;
    }

}

private class ClaimOptions
{
    [JsonProperty("enabled")]
    public bool Enabled;
    [JsonProperty("costs")]
    public List<int> Costs;
    [JsonProperty("maxClaims")]
    public int? MaxClaims;
    [JsonProperty("minAreaNameLength")]
    public int MinAreaNameLength;
    [JsonProperty("maxAreaNameLength")]
    public int MaxAreaNameLength;
    [JsonProperty("minFactionMembers")]
    public int MinFactionMembers;
    [JsonProperty("requireContiguousClaims")]
    public bool RequireContiguousClaims;
    public static ClaimOptions Default;
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class DecayOptions
    {
        [JsonProperty("enabled")]
        public bool Enabled;
        [JsonProperty("claimedLandDecayReduction")]
        public float ClaimedLandDecayReduction;
        public static DecayOptions Default;
    }

}

private class DecayOptions
{
    [JsonProperty("enabled")]
    public bool Enabled;
    [JsonProperty("claimedLandDecayReduction")]
    public float ClaimedLandDecayReduction;
    public static DecayOptions Default;
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class FactionOptions
    {
        [JsonProperty("minFactionNameLength")]
        public int MinFactionNameLength;
        [JsonProperty("maxFactionNameLength")]
        public int MaxFactionNameLength;
        [JsonProperty("maxMembers")]
        public int? MaxMembers;
        [JsonProperty("allowFactionBadlands")]
        public bool AllowFactionBadlands;
        [JsonProperty("factionBadlandsCommandCooldownSeconds")]
        public int CommandCooldownSeconds;
        [JsonProperty("overrideInGameTeamSystem")]
        public bool OverrideInGameTeamSystem;
        [JsonProperty("memberOwnLandEcoRaidingDamageScale")]
        public float MemberOwnLandEcoRaidingDamageScale;
        [JsonProperty("memberOwnLandExplosiveRaidingDamageScale")]
        public float MemberOwnLandExplosiveRaidingDamageScale;
        [JsonProperty("useClansPlugin")]
        private bool _UseClansPlugin;
        [JsonIgnore]
        public bool UseClansPlugin { get; set; }
        public static FactionOptions Default;
    }

}

private class FactionOptions
{
    [JsonProperty("minFactionNameLength")]
    public int MinFactionNameLength;
    [JsonProperty("maxFactionNameLength")]
    public int MaxFactionNameLength;
    [JsonProperty("maxMembers")]
    public int? MaxMembers;
    [JsonProperty("allowFactionBadlands")]
    public bool AllowFactionBadlands;
    [JsonProperty("factionBadlandsCommandCooldownSeconds")]
    public int CommandCooldownSeconds;
    [JsonProperty("overrideInGameTeamSystem")]
    public bool OverrideInGameTeamSystem;
    [JsonProperty("memberOwnLandEcoRaidingDamageScale")]
    public float MemberOwnLandEcoRaidingDamageScale;
    [JsonProperty("memberOwnLandExplosiveRaidingDamageScale")]
    public float MemberOwnLandExplosiveRaidingDamageScale;
    [JsonProperty("useClansPlugin")]
    private bool _UseClansPlugin;
    [JsonIgnore]
    public bool UseClansPlugin { get; set; }
    public static FactionOptions Default;
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class UpgradingOptions
    {
        [JsonProperty("enabled [THIS IS NOT AVAILABLE YET]")]
        public bool Enabled;
        [JsonProperty("maxUpgradeLevel")]
        public int MaxUpgradeLevel;
        [JsonProperty("maxProduceBonus")]
        public float MaxProduceBonus;
        [JsonProperty("maxTaxChestBonus")]
        public float MaxTaxChestBonus;
        [JsonProperty("maxRaidDefenseBonus")]
        public float MaxRaidDefenseBonus;
        [JsonProperty("maxDecayExtraReduction")]
        public float MaxDecayExtraReduction;
        [JsonProperty("maxRecruitBotBuffs")]
        public float MaxRecruitBotsBuffs;
        [JsonProperty("costs")]
        public List<int> Costs;
        public static UpgradingOptions Default;
    }

}

private class UpgradingOptions
{
    [JsonProperty("enabled [THIS IS NOT AVAILABLE YET]")]
    public bool Enabled;
    [JsonProperty("maxUpgradeLevel")]
    public int MaxUpgradeLevel;
    [JsonProperty("maxProduceBonus")]
    public float MaxProduceBonus;
    [JsonProperty("maxTaxChestBonus")]
    public float MaxTaxChestBonus;
    [JsonProperty("maxRaidDefenseBonus")]
    public float MaxRaidDefenseBonus;
    [JsonProperty("maxDecayExtraReduction")]
    public float MaxDecayExtraReduction;
    [JsonProperty("maxRecruitBotBuffs")]
    public float MaxRecruitBotsBuffs;
    [JsonProperty("costs")]
    public List<int> Costs;
    public static UpgradingOptions Default;
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class MapOptions
    {
        [JsonProperty("mapGridYOffset")]
        public int MapGridYOffset;
        [JsonProperty("pinsEnabled")]
        public bool PinsEnabled;
        [JsonProperty("minPinNameLength")]
        public int MinPinNameLength;
        [JsonProperty("maxPinNameLength")]
        public int MaxPinNameLength;
        [JsonProperty("pinCost")]
        public int PinCost;
        [JsonProperty("commandCooldownSeconds")]
        public int CommandCooldownSeconds;
        [JsonProperty("imageUrl")]
        public string ImageUrl;
        [JsonProperty("imageSize")]
        public int ImageSize;
        [JsonProperty("serverLogoUrl")]
        public string ServerLogoUrl;
        public static MapOptions Default;
    }

}

private class MapOptions
{
    [JsonProperty("mapGridYOffset")]
    public int MapGridYOffset;
    [JsonProperty("pinsEnabled")]
    public bool PinsEnabled;
    [JsonProperty("minPinNameLength")]
    public int MinPinNameLength;
    [JsonProperty("maxPinNameLength")]
    public int MaxPinNameLength;
    [JsonProperty("pinCost")]
    public int PinCost;
    [JsonProperty("commandCooldownSeconds")]
    public int CommandCooldownSeconds;
    [JsonProperty("imageUrl")]
    public string ImageUrl;
    [JsonProperty("imageSize")]
    public int ImageSize;
    [JsonProperty("serverLogoUrl")]
    public string ServerLogoUrl;
    public static MapOptions Default;
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class ImperiumOptions
    {
        [JsonProperty("badlands")]
        public BadlandsOptions Badlands;
        [JsonProperty("claims")]
        public ClaimOptions Claims;
        [JsonProperty("decay")]
        public DecayOptions Decay;
        [JsonProperty("factions")]
        public FactionOptions Factions;
        [JsonProperty("hud")]
        public HudOptions Hud;
        [JsonProperty("map")]
        public MapOptions Map;
        [JsonProperty("pvp")]
        public PvpOptions Pvp;
        [JsonProperty("raiding")]
        public RaidingOptions Raiding;
        [JsonProperty("taxes")]
        public TaxOptions Taxes;
        [JsonProperty("upkeep")]
        public UpkeepOptions Upkeep;
        [JsonProperty("war")]
        public WarOptions War;
        [JsonProperty("zones")]
        public ZoneOptions Zones;
        [JsonProperty("recruiting")]
        public RecruitingOptions Recruiting;
        [JsonProperty("upgrading")]
        public UpgradingOptions Upgrading;
        public static ImperiumOptions Default;
    }

    protected override void LoadDefaultConfig();
}

private class ImperiumOptions
{
    [JsonProperty("badlands")]
    public BadlandsOptions Badlands;
    [JsonProperty("claims")]
    public ClaimOptions Claims;
    [JsonProperty("decay")]
    public DecayOptions Decay;
    [JsonProperty("factions")]
    public FactionOptions Factions;
    [JsonProperty("hud")]
    public HudOptions Hud;
    [JsonProperty("map")]
    public MapOptions Map;
    [JsonProperty("pvp")]
    public PvpOptions Pvp;
    [JsonProperty("raiding")]
    public RaidingOptions Raiding;
    [JsonProperty("taxes")]
    public TaxOptions Taxes;
    [JsonProperty("upkeep")]
    public UpkeepOptions Upkeep;
    [JsonProperty("war")]
    public WarOptions War;
    [JsonProperty("zones")]
    public ZoneOptions Zones;
    [JsonProperty("recruiting")]
    public RecruitingOptions Recruiting;
    [JsonProperty("upgrading")]
    public UpgradingOptions Upgrading;
    public static ImperiumOptions Default;
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class PvpOptions
    {
        [JsonProperty("restrictPvp")]
        public bool RestrictPvp;
        [JsonProperty("allowedInBadlands")]
        public bool AllowedInBadlands;
        [JsonProperty("allowedInClaimedLand")]
        public bool AllowedInClaimedLand;
        [JsonProperty("allowedInWilderness")]
        public bool AllowedInWilderness;
        [JsonProperty("allowedInEventZones")]
        public bool AllowedInEventZones;
        [JsonProperty("allowedInMonumentZones")]
        public bool AllowedInMonumentZones;
        [JsonProperty("allowedInRaidZones")]
        public bool AllowedInRaidZones;
        [JsonProperty("allowedInDeepWater")]
        public bool AllowedInDeepWater;
        [JsonProperty("allowedUnderground")]
        public bool AllowedUnderground;
        [JsonProperty("enablePvpCommand")]
        public bool EnablePvpCommand;
        [JsonProperty("commandCooldownSeconds")]
        public int CommandCooldownSeconds;
        public static PvpOptions Default;
    }

}

private class PvpOptions
{
    [JsonProperty("restrictPvp")]
    public bool RestrictPvp;
    [JsonProperty("allowedInBadlands")]
    public bool AllowedInBadlands;
    [JsonProperty("allowedInClaimedLand")]
    public bool AllowedInClaimedLand;
    [JsonProperty("allowedInWilderness")]
    public bool AllowedInWilderness;
    [JsonProperty("allowedInEventZones")]
    public bool AllowedInEventZones;
    [JsonProperty("allowedInMonumentZones")]
    public bool AllowedInMonumentZones;
    [JsonProperty("allowedInRaidZones")]
    public bool AllowedInRaidZones;
    [JsonProperty("allowedInDeepWater")]
    public bool AllowedInDeepWater;
    [JsonProperty("allowedUnderground")]
    public bool AllowedUnderground;
    [JsonProperty("enablePvpCommand")]
    public bool EnablePvpCommand;
    [JsonProperty("commandCooldownSeconds")]
    public int CommandCooldownSeconds;
    public static PvpOptions Default;
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class RaidingOptions
    {
        [JsonProperty("restrictRaiding")]
        public bool RestrictRaiding;
        [JsonProperty("allowedInBadlands")]
        public bool AllowedInBadlands;
        [JsonProperty("allowedInClaimedLand")]
        public bool AllowedInClaimedLand;
        [JsonProperty("allowedInWilderness")]
        public bool AllowedInWilderness;
        public static RaidingOptions Default;
    }

}

private class RaidingOptions
{
    [JsonProperty("restrictRaiding")]
    public bool RestrictRaiding;
    [JsonProperty("allowedInBadlands")]
    public bool AllowedInBadlands;
    [JsonProperty("allowedInClaimedLand")]
    public bool AllowedInClaimedLand;
    [JsonProperty("allowedInWilderness")]
    public bool AllowedInWilderness;
    public static RaidingOptions Default;
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class TaxOptions
    {
        [JsonProperty("enabled")]
        public bool Enabled;
        [JsonProperty("defaultTaxRate")]
        public float DefaultTaxRate;
        [JsonProperty("maxTaxRate")]
        public float MaxTaxRate;
        [JsonProperty("claimedLandGatherBonus")]
        public float ClaimedLandGatherBonus;
        [JsonProperty("badlandsGatherBonus")]
        public float BadlandsGatherBonus;
        public static TaxOptions Default;
    }

}

private class TaxOptions
{
    [JsonProperty("enabled")]
    public bool Enabled;
    [JsonProperty("defaultTaxRate")]
    public float DefaultTaxRate;
    [JsonProperty("maxTaxRate")]
    public float MaxTaxRate;
    [JsonProperty("claimedLandGatherBonus")]
    public float ClaimedLandGatherBonus;
    [JsonProperty("badlandsGatherBonus")]
    public float BadlandsGatherBonus;
    public static TaxOptions Default;
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class RecruitingOptions
    {
        [JsonProperty("enabled [THIS IS NOT AVAILABLE YET]")]
        public bool Enabled;
        public static RecruitingOptions Default;
    }

}

private class RecruitingOptions
{
    [JsonProperty("enabled [THIS IS NOT AVAILABLE YET]")]
    public bool Enabled;
    public static RecruitingOptions Default;
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class HudOptions
    {
        [JsonProperty("showEventsHud")]
        public bool ShowEventsHUD;
        [JsonProperty("leftPanelXOffset")]
        public float LeftPanelXOffset;
        [JsonProperty("leftPanelYOffset")]
        public float LeftPanelYOffset;
        [JsonProperty("rightPanelXOffset")]
        public float RightPanelXOffset;
        [JsonProperty("rightPanelYOffset")]
        public float RightPanelYOffset;
        public static HudOptions Default;
    }

}

private class HudOptions
{
    [JsonProperty("showEventsHud")]
    public bool ShowEventsHUD;
    [JsonProperty("leftPanelXOffset")]
    public float LeftPanelXOffset;
    [JsonProperty("leftPanelYOffset")]
    public float LeftPanelYOffset;
    [JsonProperty("rightPanelXOffset")]
    public float RightPanelXOffset;
    [JsonProperty("rightPanelYOffset")]
    public float RightPanelYOffset;
    public static HudOptions Default;
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class UpkeepOptions
    {
        [JsonProperty("enabled")]
        public bool Enabled;
        [JsonProperty("costs")]
        public List<int> Costs;
        [JsonProperty("checkIntervalMinutes")]
        public int CheckIntervalMinutes;
        [JsonProperty("collectionPeriodHours")]
        public int CollectionPeriodHours;
        [JsonProperty("gracePeriodHours")]
        public int GracePeriodHours;
        public static UpkeepOptions Default;
    }

}

private class UpkeepOptions
{
    [JsonProperty("enabled")]
    public bool Enabled;
    [JsonProperty("costs")]
    public List<int> Costs;
    [JsonProperty("checkIntervalMinutes")]
    public int CheckIntervalMinutes;
    [JsonProperty("collectionPeriodHours")]
    public int CollectionPeriodHours;
    [JsonProperty("gracePeriodHours")]
    public int GracePeriodHours;
    public static UpkeepOptions Default;
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class WarOptions
    {
        [JsonProperty("enabled")]
        public bool Enabled;
        [JsonProperty("noobFactionProtectionInSeconds")]
        public int NoobFactionProtectionInSeconds;
        [JsonProperty("declarationCost")]
        public int DeclarationCost;
        [JsonProperty("onlineDefendersRequired")]
        public int OnlineDefendersRequired;
        [JsonProperty("adminApprovalRequired")]
        public bool AdminApprovalRequired;
        [JsonProperty("defenderApprovalRequired")]
        public bool DefenderApprovalRequired;
        [JsonProperty("enableShopfrontPeace")]
        public bool EnableShopfrontPeace;
        [JsonProperty("priorAggressionRequired")]
        public bool PriorAggressionRequired;
        [JsonProperty("spamPreventionSeconds")]
        public int SpamPreventionSeconds;
        [JsonProperty("minCassusBelliLength")]
        public int MinCassusBelliLength;
        [JsonProperty("defensiveBonuses")]
        public List<float> DefensiveBonuses;
        public static WarOptions Default;
    }

}

private class WarOptions
{
    [JsonProperty("enabled")]
    public bool Enabled;
    [JsonProperty("noobFactionProtectionInSeconds")]
    public int NoobFactionProtectionInSeconds;
    [JsonProperty("declarationCost")]
    public int DeclarationCost;
    [JsonProperty("onlineDefendersRequired")]
    public int OnlineDefendersRequired;
    [JsonProperty("adminApprovalRequired")]
    public bool AdminApprovalRequired;
    [JsonProperty("defenderApprovalRequired")]
    public bool DefenderApprovalRequired;
    [JsonProperty("enableShopfrontPeace")]
    public bool EnableShopfrontPeace;
    [JsonProperty("priorAggressionRequired")]
    public bool PriorAggressionRequired;
    [JsonProperty("spamPreventionSeconds")]
    public int SpamPreventionSeconds;
    [JsonProperty("minCassusBelliLength")]
    public int MinCassusBelliLength;
    [JsonProperty("defensiveBonuses")]
    public List<float> DefensiveBonuses;
    public static WarOptions Default;
}

Oxide.Plugins
public partial class Imperium : RustPlugin
{
    private class ZoneOptions
    {
        [JsonProperty("enabled")]
        public bool Enabled;
        [JsonProperty("domeDarkness")]
        public int DomeDarkness;
        [JsonProperty("eventZoneRadius")]
        public float EventZoneRadius;
        [JsonProperty("eventZoneLifespanSeconds")]
        public float EventZoneLifespanSeconds;
        [JsonProperty("monumentZones")]
        public Dictionary<string, float> MonumentZones;
        public static ZoneOptions Default;
    }

}

private class ZoneOptions
{
    [JsonProperty("enabled")]
    public bool Enabled;
    [JsonProperty("domeDarkness")]
    public int DomeDarkness;
    [JsonProperty("eventZoneRadius")]
    public float EventZoneRadius;
    [JsonProperty("eventZoneLifespanSeconds")]
    public float EventZoneLifespanSeconds;
    [JsonProperty("monumentZones")]
    public Dictionary<string, float> MonumentZones;
    public static ZoneOptions Default;
}

Oxide.Plugins
public partial class Imperium
{
    private class GameEventWatcher : MonoBehaviour
    {
        private const float CheckIntervalSeconds;
        private HashSet<CargoPlane> CargoPlanes;
        private HashSet<BaseHelicopter> PatrolHelicopters;
        private HashSet<CH47Helicopter> ChinookHelicopters;
        private HashSet<HackableLockedCrate> LockedCrates;
        private HashSet<CargoShip> CargoShips;
        public bool IsCargoPlaneActive { get; set; }
        public bool IsHelicopterActive { get; set; }
        public bool IsChinookOrLockedCrateActive { get; set; }
        public bool IsCargoShipActive { get; set; }
        private void Awake();
        private void OnDestroy();
        public void BeginEvent(CargoPlane plane);
        public void BeginEvent(BaseHelicopter heli);
        public void BeginEvent(CH47Helicopter chinook);
        public void BeginEvent(HackableLockedCrate crate);
        public void BeginEvent(CargoShip ship);
        private void CheckEvents();
        private bool IsEntityGone(BaseEntity entity);
    }

}

private class GameEventWatcher : MonoBehaviour
{
    private const float CheckIntervalSeconds;
    private HashSet<CargoPlane> CargoPlanes;
    private HashSet<BaseHelicopter> PatrolHelicopters;
    private HashSet<CH47Helicopter> ChinookHelicopters;
    private HashSet<HackableLockedCrate> LockedCrates;
    private HashSet<CargoShip> CargoShips;
    public bool IsCargoPlaneActive { get; set; }
    public bool IsHelicopterActive { get; set; }
    public bool IsChinookOrLockedCrateActive { get; set; }
    public bool IsCargoShipActive { get; set; }
    private void Awake();
    private void OnDestroy();
    public void BeginEvent(CargoPlane plane);
    public void BeginEvent(BaseHelicopter heli);
    public void BeginEvent(CH47Helicopter chinook);
    public void BeginEvent(HackableLockedCrate crate);
    public void BeginEvent(CargoShip ship);
    private void CheckEvents();
    private bool IsEntityGone(BaseEntity entity);
}

Oxide.Plugins
public partial class Imperium
{
    private class HudManager
    {
        private Dictionary<string, Image> Images;
        private bool UpdatePending;
        public GameEventWatcher GameEvents { get; set; }
        private ImageDownloader ImageDownloader;
        private MapOverlayGenerator MapOverlayGenerator;
        public HudManager();
        public void RefreshForAllPlayers();
        public Image RegisterImage(string url, byte[] imageData, bool overwrite);
        public void RefreshAllImages();
        public CuiRawImageComponent CreateImageComponent(string imageUrl);
        public void GenerateMapOverlayImage();
        public void Init();
        public void Destroy();
        private void RegisterDefaultImages(Type type);
    }

}

private class HudManager
{
    private Dictionary<string, Image> Images;
    private bool UpdatePending;
    public GameEventWatcher GameEvents { get; set; }
    private ImageDownloader ImageDownloader;
    private MapOverlayGenerator MapOverlayGenerator;
    public HudManager();
    public void RefreshForAllPlayers();
    public Image RegisterImage(string url, byte[] imageData, bool overwrite);
    public void RefreshAllImages();
    public CuiRawImageComponent CreateImageComponent(string imageUrl);
    public void GenerateMapOverlayImage();
    public void Init();
    public void Destroy();
    private void RegisterDefaultImages(Type type);
}

Oxide.Plugins
public partial class Imperium
{
    private class Image
    {
        public string Url { get; set; }
        public string Id { get; set; }
        public bool IsDownloaded { get; set; }
        public bool IsGenerated { get; set; }
        public Image(string url, string id);
        public string Save(byte[] data);
        public void Delete();
    }

}

private class Image
{
    public string Url { get; set; }
    public string Id { get; set; }
    public bool IsDownloaded { get; set; }
    public bool IsGenerated { get; set; }
    public Image(string url, string id);
    public string Save(byte[] data);
    public void Delete();
}

Oxide.Plugins
public partial class Imperium
{
    private class ImageDownloader : MonoBehaviour
    {
        private Queue<Image> PendingImages;
        public bool IsDownloading { get; set; }
        public void Download(Image image);
        private void DownloadNext();
        private IEnumerator DownloadImage(Image image);
    }

}

private class ImageDownloader : MonoBehaviour
{
    private Queue<Image> PendingImages;
    public bool IsDownloading { get; set; }
    public void Download(Image image);
    private void DownloadNext();
    private IEnumerator DownloadImage(Image image);
}

Oxide.Plugins
public partial class Imperium
{
    private class ImperiumMapMarker
    {
        public string IconUrl;
        public string Label;
        public float X;
        public float Z;
        public static ImperiumMapMarker ForUser(User user);
        public static ImperiumMapMarker ForHeadquarters(Area area, Faction faction);
        public static ImperiumMapMarker ForMonument(MonumentInfo monument);
        public static ImperiumMapMarker ForPin(Pin pin);
        private static float TranslatePositionX(float pos);
        private static float TranslatePositionZ(float pos);
        private static string GetIconForMonument(MonumentInfo monument);
    }

    private static string GetIconForPin(Pin pin);
}

private class ImperiumMapMarker
{
    public string IconUrl;
    public string Label;
    public float X;
    public float Z;
    public static ImperiumMapMarker ForUser(User user);
    public static ImperiumMapMarker ForHeadquarters(Area area, Faction faction);
    public static ImperiumMapMarker ForMonument(MonumentInfo monument);
    public static ImperiumMapMarker ForPin(Pin pin);
    private static float TranslatePositionX(float pos);
    private static float TranslatePositionZ(float pos);
    private static string GetIconForMonument(MonumentInfo monument);
}

Oxide.Plugins
public partial class Imperium
{
    public static class UI
    {
        public const string MapOverlayImageUrl;
        public const string TransparentTexture;
        public static class Element
        {
            public static string Hud;
            public static string HudPanelLeft;
            public static string HudPanelRight;
            public static string HudPanelWarning;
            public static string HudPanelText;
            public static string HudPanelIcon;
            public static string Overlay;
            public static string MapDialog;
            public static string MapHeader;
            public static string MapHeaderTitle;
            public static string MapHeaderCloseButton;
            public static string MapContainer;
            public static string MapTerrainImage;
            public static string MapLayers;
            public static string MapClaimsImage;
            public static string MapMarkerIcon;
            public static string MapMarkerLabel;
            public static string MapSidebar;
            public static string MapButton;
            public static string MapServerLogoImage;
            public static string PanelWindow;
            public static string PanelDialog;
            public static string PanelHeader;
            public static string PanelHeaderTitle;
            public static string PanelSidebar;
            public static string PanelTab;
            public static string PanelLabel;
            public static string PanelCommandButton;
            public static string PanelTextInput;
            public static string PanelConfirmButton;
        }

        public static class HudIcon
        {
            public static string Badlands;
            public static string CargoPlaneIndicatorOn;
            public static string CargoPlaneIndicatorOff;
            public static string CargoShipIndicatorOn;
            public static string CargoShipIndicatorOff;
            public static string ChinookIndicatorOn;
            public static string ChinookIndicatorOff;
            public static string Claimed;
            public static string Clock;
            public static string Debris;
            public static string Defense;
            public static string Harvest;
            public static string Headquarters;
            public static string HelicopterIndicatorOn;
            public static string HelicopterIndicatorOff;
            public static string Monument;
            public static string Players;
            public static string PvpMode;
            public static string Raid;
            public static string Ruins;
            public static string Sleepers;
            public static string SupplyDrop;
            public static string Taxes;
            public static string Warning;
            public static string WarZone;
            public static string Wilderness;
        }

        public static class MapIcon
        {
            public static string Airfield;
            public static string Arena;
            public static string BanditTown;
            public static string Cave;
            public static string Compound;
            public static string Dome;
            public static string GasStation;
            public static string Harbor;
            public static string Headquarters;
            public static string Hotel;
            public static string Junkyard;
            public static string LaunchSite;
            public static string Lighthouse;
            public static string Marina;
            public static string MilitaryTunnel;
            public static string MiningOutpost;
            public static string Player;
            public static string PowerPlant;
            public static string Quarry;
            public static string SatelliteDish;
            public static string SewerBranch;
            public static string Shop;
            public static string Substation;
            public static string Supermarket;
            public static string Town;
            public static string Trainyard;
            public static string Unknown;
            public static string WaterTreatmentPlant;
        }

        public static class Colors
        {
            public static string Primary;
            public static string Secondary;
            public static string Highlight;
            public static string Disabled;
            public static string Success;
            public static string Info;
        }

        public static CuiElementContainer Container(string panel, string color, UI4 dimensions, bool blur, string parent);
        public static CuiElementContainer Popup(string panel, string text, int size, UI4 dimensions, TextAnchor align, string parent);
        public static void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions);
        public static void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
        public static void Label(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, TextAnchor align);
        public static void Button(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, string command, TextAnchor align);
        public static void Input(CuiElementContainer container, string panel, string color, string text, int size, string command, UI4 dimensions, TextAnchor anchor);
        public static void Image(CuiElementContainer container, string panel, string png, UI4 dimensions);
        public static void Image(CuiElementContainer container, string panel, int itemId, ulong skinId, UI4 dimensions);
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

}

public static class UI
{
    public const string MapOverlayImageUrl;
    public const string TransparentTexture;
    public static class Element
    {
        public static string Hud;
        public static string HudPanelLeft;
        public static string HudPanelRight;
        public static string HudPanelWarning;
        public static string HudPanelText;
        public static string HudPanelIcon;
        public static string Overlay;
        public static string MapDialog;
        public static string MapHeader;
        public static string MapHeaderTitle;
        public static string MapHeaderCloseButton;
        public static string MapContainer;
        public static string MapTerrainImage;
        public static string MapLayers;
        public static string MapClaimsImage;
        public static string MapMarkerIcon;
        public static string MapMarkerLabel;
        public static string MapSidebar;
        public static string MapButton;
        public static string MapServerLogoImage;
        public static string PanelWindow;
        public static string PanelDialog;
        public static string PanelHeader;
        public static string PanelHeaderTitle;
        public static string PanelSidebar;
        public static string PanelTab;
        public static string PanelLabel;
        public static string PanelCommandButton;
        public static string PanelTextInput;
        public static string PanelConfirmButton;
    }

    public static class HudIcon
    {
        public static string Badlands;
        public static string CargoPlaneIndicatorOn;
        public static string CargoPlaneIndicatorOff;
        public static string CargoShipIndicatorOn;
        public static string CargoShipIndicatorOff;
        public static string ChinookIndicatorOn;
        public static string ChinookIndicatorOff;
        public static string Claimed;
        public static string Clock;
        public static string Debris;
        public static string Defense;
        public static string Harvest;
        public static string Headquarters;
        public static string HelicopterIndicatorOn;
        public static string HelicopterIndicatorOff;
        public static string Monument;
        public static string Players;
        public static string PvpMode;
        public static string Raid;
        public static string Ruins;
        public static string Sleepers;
        public static string SupplyDrop;
        public static string Taxes;
        public static string Warning;
        public static string WarZone;
        public static string Wilderness;
    }

    public static class MapIcon
    {
        public static string Airfield;
        public static string Arena;
        public static string BanditTown;
        public static string Cave;
        public static string Compound;
        public static string Dome;
        public static string GasStation;
        public static string Harbor;
        public static string Headquarters;
        public static string Hotel;
        public static string Junkyard;
        public static string LaunchSite;
        public static string Lighthouse;
        public static string Marina;
        public static string MilitaryTunnel;
        public static string MiningOutpost;
        public static string Player;
        public static string PowerPlant;
        public static string Quarry;
        public static string SatelliteDish;
        public static string SewerBranch;
        public static string Shop;
        public static string Substation;
        public static string Supermarket;
        public static string Town;
        public static string Trainyard;
        public static string Unknown;
        public static string WaterTreatmentPlant;
    }

    public static class Colors
    {
        public static string Primary;
        public static string Secondary;
        public static string Highlight;
        public static string Disabled;
        public static string Success;
        public static string Info;
    }

    public static CuiElementContainer Container(string panel, string color, UI4 dimensions, bool blur, string parent);
    public static CuiElementContainer Popup(string panel, string text, int size, UI4 dimensions, TextAnchor align, string parent);
    public static void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions);
    public static void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
    public static void Label(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, TextAnchor align);
    public static void Button(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, string command, TextAnchor align);
    public static void Input(CuiElementContainer container, string panel, string color, string text, int size, string command, UI4 dimensions, TextAnchor anchor);
    public static void Image(CuiElementContainer container, string panel, string png, UI4 dimensions);
    public static void Image(CuiElementContainer container, string panel, int itemId, ulong skinId, UI4 dimensions);
    public static void Toggle(CuiElementContainer container, string panel, string boxColor, int fontSize, UI4 dimensions, string command, bool isOn);
    public static string Color(string hexColor, float alpha);
}

public static class Element
{
    public static string Hud;
    public static string HudPanelLeft;
    public static string HudPanelRight;
    public static string HudPanelWarning;
    public static string HudPanelText;
    public static string HudPanelIcon;
    public static string Overlay;
    public static string MapDialog;
    public static string MapHeader;
    public static string MapHeaderTitle;
    public static string MapHeaderCloseButton;
    public static string MapContainer;
    public static string MapTerrainImage;
    public static string MapLayers;
    public static string MapClaimsImage;
    public static string MapMarkerIcon;
    public static string MapMarkerLabel;
    public static string MapSidebar;
    public static string MapButton;
    public static string MapServerLogoImage;
    public static string PanelWindow;
    public static string PanelDialog;
    public static string PanelHeader;
    public static string PanelHeaderTitle;
    public static string PanelSidebar;
    public static string PanelTab;
    public static string PanelLabel;
    public static string PanelCommandButton;
    public static string PanelTextInput;
    public static string PanelConfirmButton;
}

public static class HudIcon
{
    public static string Badlands;
    public static string CargoPlaneIndicatorOn;
    public static string CargoPlaneIndicatorOff;
    public static string CargoShipIndicatorOn;
    public static string CargoShipIndicatorOff;
    public static string ChinookIndicatorOn;
    public static string ChinookIndicatorOff;
    public static string Claimed;
    public static string Clock;
    public static string Debris;
    public static string Defense;
    public static string Harvest;
    public static string Headquarters;
    public static string HelicopterIndicatorOn;
    public static string HelicopterIndicatorOff;
    public static string Monument;
    public static string Players;
    public static string PvpMode;
    public static string Raid;
    public static string Ruins;
    public static string Sleepers;
    public static string SupplyDrop;
    public static string Taxes;
    public static string Warning;
    public static string WarZone;
    public static string Wilderness;
}

public static class MapIcon
{
    public static string Airfield;
    public static string Arena;
    public static string BanditTown;
    public static string Cave;
    public static string Compound;
    public static string Dome;
    public static string GasStation;
    public static string Harbor;
    public static string Headquarters;
    public static string Hotel;
    public static string Junkyard;
    public static string LaunchSite;
    public static string Lighthouse;
    public static string Marina;
    public static string MilitaryTunnel;
    public static string MiningOutpost;
    public static string Player;
    public static string PowerPlant;
    public static string Quarry;
    public static string SatelliteDish;
    public static string SewerBranch;
    public static string Shop;
    public static string Substation;
    public static string Supermarket;
    public static string Town;
    public static string Trainyard;
    public static string Unknown;
    public static string WaterTreatmentPlant;
}

public static class Colors
{
    public static string Primary;
    public static string Secondary;
    public static string Highlight;
    public static string Disabled;
    public static string Success;
    public static string Info;
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

Oxide.Plugins
public partial class Imperium
{
    private class UserHud
    {
        private const float IconSize;
        private static class PanelColor
        {
            public const string BackgroundNormal;
            public const string BackgroundDanger;
            public const string BackgroundSafe;
            public const string TextNormal;
            public const string TextDanger;
            public const string TextSafe;
        }

        public User User { get; set; }
        public bool IsDisabled { get; set; }
        public UserHud(User user);
        public void Show();
        public void Hide();
        public void Toggle();
        public void Refresh();
        private CuiElementContainer Build();
        private string GetRightPanelBackgroundColor();
        private string GetRightPanelTextColor();
        private string GetLocationIcon();
        private string GetLocationDescription();
        private string GetLeftPanelBackgroundColor();
        private string GetLeftPanelTextColor();
        private void AddWidget(CuiElementContainer container, string parent, string iconName, string textColor, string text, float left);
        private void AddWidget(CuiElementContainer container, string parent, string iconName, float left);
    }

}

private class UserHud
{
    private const float IconSize;
    private static class PanelColor
    {
        public const string BackgroundNormal;
        public const string BackgroundDanger;
        public const string BackgroundSafe;
        public const string TextNormal;
        public const string TextDanger;
        public const string TextSafe;
    }

    public User User { get; set; }
    public bool IsDisabled { get; set; }
    public UserHud(User user);
    public void Show();
    public void Hide();
    public void Toggle();
    public void Refresh();
    private CuiElementContainer Build();
    private string GetRightPanelBackgroundColor();
    private string GetRightPanelTextColor();
    private string GetLocationIcon();
    private string GetLocationDescription();
    private string GetLeftPanelBackgroundColor();
    private string GetLeftPanelTextColor();
    private void AddWidget(CuiElementContainer container, string parent, string iconName, string textColor, string text, float left);
    private void AddWidget(CuiElementContainer container, string parent, string iconName, float left);
}

private static class PanelColor
{
    public const string BackgroundNormal;
    public const string BackgroundDanger;
    public const string BackgroundSafe;
    public const string TextNormal;
    public const string TextDanger;
    public const string TextSafe;
}

Oxide.Plugins
public partial class Imperium
{
    private class UserPanel
    {
        public static List<UIChatCommandDef> UiCommands;
        public static void InitializeUserPanelCommandDefs();
        public User User { get; set; }
        public bool IsDisabled;
        public string currentCategory;
        public UIChatCommandDef selectedCommand;
        public string currentCommand;
        public Dictionary<int, string> indexedArgs;
        public int currentRequiredArgs;
        public const float SPACING;
        public class UIChatCommandDef
        {
            public string displayName;
            public string shortDescription;
            public string category;
            public string command;
            public string uid;
            public List<UIChatCommandArg> args;
            public FactionAuth auth;
            public bool authExclusive;
            public bool closesUI;
            public UIChatCommandDef();
        }

        public class UIChatCommandArg
        {
            public string label;
            public string description;
            public bool isSubstring;
        }

        public UserPanel(User user);
        public void ClearCurrentCommand();
        public void OpenTab(string category);
        public void OpenCommand(string uid);
        private List<CuiElementContainer> Build();
        private CuiElementContainer CreatePanelHeader(CuiElementContainer container);
        private CuiElementContainer CreatePanelSidebar(CuiElementContainer container);
        private void CreateSelectedCommandDialog(CuiElementContainer container);
        private void CreateSelectedCategoryButtons(CuiElementContainer container);
        private void CreateHomeDialog(CuiElementContainer container);
        public void Close();
        public void SetCommand(string command);
        public string GetFullConsoleCommand();
        public void SetArg(int index, string arg, bool isSubstring);
        public void RemoveArg(int index);
        public bool HasArg(int index);
        public void Show();
        public void Hide();
        public void Toggle();
        public void Refresh();
    }

}

private class UserPanel
{
    public static List<UIChatCommandDef> UiCommands;
    public static void InitializeUserPanelCommandDefs();
    public User User { get; set; }
    public bool IsDisabled;
    public string currentCategory;
    public UIChatCommandDef selectedCommand;
    public string currentCommand;
    public Dictionary<int, string> indexedArgs;
    public int currentRequiredArgs;
    public const float SPACING;
    public class UIChatCommandDef
    {
        public string displayName;
        public string shortDescription;
        public string category;
        public string command;
        public string uid;
        public List<UIChatCommandArg> args;
        public FactionAuth auth;
        public bool authExclusive;
        public bool closesUI;
        public UIChatCommandDef();
    }

    public class UIChatCommandArg
    {
        public string label;
        public string description;
        public bool isSubstring;
    }

    public UserPanel(User user);
    public void ClearCurrentCommand();
    public void OpenTab(string category);
    public void OpenCommand(string uid);
    private List<CuiElementContainer> Build();
    private CuiElementContainer CreatePanelHeader(CuiElementContainer container);
    private CuiElementContainer CreatePanelSidebar(CuiElementContainer container);
    private void CreateSelectedCommandDialog(CuiElementContainer container);
    private void CreateSelectedCategoryButtons(CuiElementContainer container);
    private void CreateHomeDialog(CuiElementContainer container);
    public void Close();
    public void SetCommand(string command);
    public string GetFullConsoleCommand();
    public void SetArg(int index, string arg, bool isSubstring);
    public void RemoveArg(int index);
    public bool HasArg(int index);
    public void Show();
    public void Hide();
    public void Toggle();
    public void Refresh();
}

public class UIChatCommandDef
{
    public string displayName;
    public string shortDescription;
    public string category;
    public string command;
    public string uid;
    public List<UIChatCommandArg> args;
    public FactionAuth auth;
    public bool authExclusive;
    public bool closesUI;
    public UIChatCommandDef();
}

public class UIChatCommandArg
{
    public string label;
    public string description;
    public bool isSubstring;
}

Oxide.Plugins
public partial class Imperium
{
    private class UserMap
    {
        public User User { get; set; }
        public bool IsVisible { get; set; }
        public UserMap(User user);
        public void Show();
        public void Hide();
        public void Toggle();
        public void Refresh();
        private CuiElementContainer BuildDialog();
        private void AddDialogHeader(CuiElementContainer container);
        private void AddMapTerrainImage(CuiElementContainer container);
        private CuiElementContainer BuildSidebar();
        private void AddLayerToggleButtons(CuiElementContainer container);
        private void AddServerLogo(CuiElementContainer container);
        private CuiElementContainer BuildMapLayers();
        private void AddClaimsLayer(CuiElementContainer container);
        private void AddMonumentsLayer(CuiElementContainer container);
        private void AddHeadquartersLayer(CuiElementContainer container);
        private void AddPinsLayer(CuiElementContainer container);
        private void AddMarker(CuiElementContainer container, ImperiumMapMarker marker, float iconSize);
        private bool ShowMonumentOnMap(MonumentInfo monument);
        private string GetButtonColor(UserMapLayer layer);
    }

}

private class UserMap
{
    public User User { get; set; }
    public bool IsVisible { get; set; }
    public UserMap(User user);
    public void Show();
    public void Hide();
    public void Toggle();
    public void Refresh();
    private CuiElementContainer BuildDialog();
    private void AddDialogHeader(CuiElementContainer container);
    private void AddMapTerrainImage(CuiElementContainer container);
    private CuiElementContainer BuildSidebar();
    private void AddLayerToggleButtons(CuiElementContainer container);
    private void AddServerLogo(CuiElementContainer container);
    private CuiElementContainer BuildMapLayers();
    private void AddClaimsLayer(CuiElementContainer container);
    private void AddMonumentsLayer(CuiElementContainer container);
    private void AddHeadquartersLayer(CuiElementContainer container);
    private void AddPinsLayer(CuiElementContainer container);
    private void AddMarker(CuiElementContainer container, ImperiumMapMarker marker, float iconSize);
    private bool ShowMonumentOnMap(MonumentInfo monument);
    private string GetButtonColor(UserMapLayer layer);
}

Oxide.Plugins
public partial class Imperium
{
    private class LruCache
    {
        private Dictionary<K, LinkedListNode<LruCacheItem>> Nodes;
        private LinkedList<LruCacheItem> RecencyList;
        public int Capacity { get; set; }
        public LruCache(int capacity);
        public bool TryGetValue(K key, V value);
        public void Set(K key, V value);
        public bool Remove(K key);
        public void Clear();
        private void Evict();
        private class LruCacheItem
        {
            public K Key;
            public V Value;
            public LruCacheItem(K key, V value);
        }

    }

}

private class LruCache
{
    private Dictionary<K, LinkedListNode<LruCacheItem>> Nodes;
    private LinkedList<LruCacheItem> RecencyList;
    public int Capacity { get; set; }
    public LruCache(int capacity);
    public bool TryGetValue(K key, V value);
    public void Set(K key, V value);
    public bool Remove(K key);
    public void Clear();
    private void Evict();
    private class LruCacheItem
    {
        public K Key;
        public V Value;
        public LruCacheItem(K key, V value);
    }

}

private class LruCacheItem
{
    public K Key;
    public V Value;
    public LruCacheItem(K key, V value);
}


```

---

## Inbound by Substrata - Broadcasts notifications when patrol helicopters, supply drops, cargo ships, etc. are inbound

```csharp
using System;
using System.Collections.Generic;
using System.Text;
using System.Text.RegularExpressions;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Inbound", "Substrata", "0.6.9")]
[Description("Broadcasts notifications when patrol helicopters, supply drops, cargo ships, etc. are inbound")]
 class Inbound : RustPlugin
{
    [PluginReference]
     Plugin DiscordMessages;
     Plugin PopupNotifications;
     Plugin UINotify;
     Plugin AirdropPrecision;
     Plugin FancyDrop;
     bool initialized;
     float worldSize;
     int cellCount;
     float cellSize;
     float gridBottom;
     float gridTop;
     float gridLeft;
     float gridRight;
     bool hasOilRig;
     bool hasLargeRig;
     Vector3 oilRigPos;
     Vector3 largeRigPos;
     bool hasExcavator;
     Vector3 excavatorPos;
     ulong chatIconID;
     string webhookURL;
     void OnServerInitialized(bool initial);
     void OnEntitySpawned(PatrolHelicopter heli);
     void OnEntitySpawned(CargoShip ship);
     void OnCargoShipHarborApproach(CargoShip ship);
     void OnCargoShipHarborArrived(CargoShip ship);
     void OnCargoShipHarborLeave(CargoShip ship);
     void OnEntitySpawned(CH47HelicopterAIController ch47);
     void OnBradleyApcInitialize(BradleyAPC apc);
     void OnEntitySpawned(TravellingVendor travellingVendor);
     void OnExcavatorResourceSet(ExcavatorArm arm, string resourceName, BasePlayer player);
     void OnEntitySpawned(HackableLockedCrate crate);
     void CanHackCrate(BasePlayer player, HackableLockedCrate crate);
    private HashSet<CalledDrop> calledDrops;
    private class CalledDrop
    {
        public IPlayer _iplayer;
        public SupplySignal _signal;
        public CargoPlane _plane;
        public SupplyDrop _drop;
    }

     void OnExplosiveThrown(BasePlayer player, SupplySignal signal);
     void OnExplosiveDropped(BasePlayer player, SupplySignal signal);
     void OnCargoPlaneSignaled(CargoPlane plane, SupplySignal signal);
     void OnExcavatorSuppliesRequested(ExcavatorSignalComputer computer, BasePlayer player, CargoPlane plane);
     void OnAirdrop(CargoPlane plane, Vector3 dest);
    private HashSet<ulong> droppedDrops;
     void OnSupplyDropDropped(SupplyDrop drop, CargoPlane plane);
     void OnEntitySpawned(SupplyDrop drop);
    private HashSet<ulong> landedDrops;
     void OnSupplyDropLanded(SupplyDrop drop);
     void OnEntityKill(SupplyDrop drop);
     void SendInboundMessage(string message, bool alert);
     void SendUINotify(string msg);
     void SendDiscordMessage(string msg);
     void LogInboundMessage(string msg);
    private string Location(Vector3 pos, BaseEntity entity, bool hideExc);
    private string Destination(Vector3 pos);
    private string GetLocation(Vector3 pos, BaseEntity entity, bool hideExc);
     string GetHarborLocation(CargoShip cargoShip);
     string GetGrid(Vector3 pos);
    private CalledDrop GetCalledDrop(CargoPlane plane, SupplyDrop drop);
    private string SupplyDropPlayer(CalledDrop calledDrop);
    private bool HideSupplyAlert(CalledDrop calledDrop);
    private bool HideCrateAlert(HackableLockedCrate crate);
    const string filterTags;
    private bool IsAtOilRig(Vector3 pos);
    private bool IsAtLargeRig(Vector3 pos);
    private bool IsAtCargoShip(BaseEntity entity);
    private bool IsAtExcavator(Vector3 pos);
    private bool IsOffGrid(Vector3 pos);
    private bool SupplyPlayerCompatible();
     void InitVariables();
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Notifications")]
        public Notifications notifications { get; set; }
        [JsonProperty(PropertyName = "Discord Messages")]
        public DiscordMessages discordMessages { get; set; }
        [JsonProperty(PropertyName = "UI Notify")]
        public UINotify uiNotify { get; set; }
        [JsonProperty(PropertyName = "Alerts")]
        public Alerts alerts { get; set; }
        [JsonProperty(PropertyName = "Location")]
        public Location location { get; set; }
        [JsonProperty(PropertyName = "Misc")]
        public Misc misc { get; set; }
        [JsonProperty(PropertyName = "Logging")]
        public Logging logging { get; set; }
        public class Notifications
        {
            [JsonProperty(PropertyName = "Chat Notifications")]
            public bool chat { get; set; }
            [JsonProperty(PropertyName = "Popup Notifications")]
            public bool popup { get; set; }
        }

        public class DiscordMessages
        {
            [JsonProperty(PropertyName = "Enabled")]
            public bool enabled { get; set; }
            [JsonProperty(PropertyName = "Webhook URL")]
            public string webhookURL { get; set; }
            [JsonProperty(PropertyName = "Embedded Messages")]
            public bool embedded { get; set; }
            [JsonProperty(PropertyName = "Embed Color")]
            public int embedColor { get; set; }
            [JsonProperty(PropertyName = "Embed Title")]
            public string embedTitle { get; set; }
        }

        public class UINotify
        {
            [JsonProperty(PropertyName = "Enabled")]
            public bool enabled { get; set; }
            [JsonProperty(PropertyName = "Notification Type")]
            public int type { get; set; }
        }

        public class Alerts
        {
            [JsonProperty(PropertyName = "Patrol Helicopter Alerts")]
            public bool patrolHeli { get; set; }
            [JsonProperty(PropertyName = "Cargo Ship Alerts")]
            public bool cargoShip { get; set; }
            [JsonProperty(PropertyName = "Cargo Ship Harbor Alerts")]
            public bool cargoShipHarbor { get; set; }
            [JsonProperty(PropertyName = "Cargo Plane Alerts")]
            public bool cargoPlane { get; set; }
            [JsonProperty(PropertyName = "CH47 Chinook Alerts")]
            public bool ch47 { get; set; }
            [JsonProperty(PropertyName = "Bradley APC Alerts")]
            public bool bradleyAPC { get; set; }
            [JsonProperty(PropertyName = "Travelling Vendor Alerts")]
            public bool travellingVendor { get; set; }
            [JsonProperty(PropertyName = "Excavator Activated Alerts")]
            public bool excavator { get; set; }
            [JsonProperty(PropertyName = "Excavator Supply Request Alerts")]
            public bool excavatorSupply { get; set; }
            [JsonProperty(PropertyName = "Hackable Crate Alerts")]
            public bool hackableCrateSpawn { get; set; }
            [JsonProperty(PropertyName = "Player Hacking Crate Alerts")]
            public bool hackingCrate { get; set; }
            [JsonProperty(PropertyName = "Supply Signal Alerts")]
            public bool supplySignal { get; set; }
            [JsonProperty(PropertyName = "Supply Drop Alerts")]
            public bool supplyDrop { get; set; }
            [JsonProperty(PropertyName = "Supply Drop Landed Alerts")]
            public bool supplyDropLand { get; set; }
        }

        public class Location
        {
            [JsonProperty(PropertyName = "Show Grid")]
            public bool showGrid { get; set; }
            [JsonProperty(PropertyName = "Show 'Oil Rig' Labels")]
            public bool showOilRigs { get; set; }
            [JsonProperty(PropertyName = "Show 'Cargo Ship' Label")]
            public bool showCargoShip { get; set; }
            [JsonProperty(PropertyName = "Show 'Excavator' Label")]
            public bool showExcavator { get; set; }
            [JsonProperty(PropertyName = "Hide Unmarked Grids")]
            public bool hideOffGrid { get; set; }
            [JsonProperty(PropertyName = "Show Coordinates")]
            public bool showCoords { get; set; }
            [JsonProperty(PropertyName = "Hide Y Coordinate")]
            public bool hideYCoord { get; set; }
            [JsonProperty(PropertyName = "Hide Coordinate Decimals")]
            public bool hideCoordDecimals { get; set; }
        }

        public class Misc
        {
            [JsonProperty(PropertyName = "Chat Icon (SteamID64)")]
            public ulong chatIcon { get; set; }
            [JsonProperty(PropertyName = "Hide Cargo Ship Crate Messages")]
            public bool hideCargoCrates { get; set; }
            [JsonProperty(PropertyName = "Hide Oil Rig Crate & Chinook Messages")]
            public bool hideRigCrates { get; set; }
            [JsonProperty(PropertyName = "Show Supply Drop Player")]
            public bool showSupplyPlayer { get; set; }
            [JsonProperty(PropertyName = "Hide Player-Called Supply Drop Messages")]
            public bool hideCalledSupply { get; set; }
            [JsonProperty(PropertyName = "Hide Random Supply Drop Messages")]
            public bool hideRandomSupply { get; set; }
        }

        public class Logging
        {
            [JsonProperty(PropertyName = "Log To Console")]
            public bool console { get; set; }
            [JsonProperty(PropertyName = "Log To File")]
            public bool file { get; set; }
            [JsonProperty(PropertyName = "Log All Events")]
            public bool allEvents { get; set; }
        }

        [JsonProperty(PropertyName = "Version (Do not modify)")]
        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    private ConfigData GetBaseConfig();
    private void UpdateConfigValues();
     object GetConfig(string menu, string dataValue, object defaultValue);
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private string Lang(string key, string id, object[] args);
     void BackupLang();
}

private class CalledDrop
{
    public IPlayer _iplayer;
    public SupplySignal _signal;
    public CargoPlane _plane;
    public SupplyDrop _drop;
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Notifications")]
    public Notifications notifications { get; set; }
    [JsonProperty(PropertyName = "Discord Messages")]
    public DiscordMessages discordMessages { get; set; }
    [JsonProperty(PropertyName = "UI Notify")]
    public UINotify uiNotify { get; set; }
    [JsonProperty(PropertyName = "Alerts")]
    public Alerts alerts { get; set; }
    [JsonProperty(PropertyName = "Location")]
    public Location location { get; set; }
    [JsonProperty(PropertyName = "Misc")]
    public Misc misc { get; set; }
    [JsonProperty(PropertyName = "Logging")]
    public Logging logging { get; set; }
    public class Notifications
    {
        [JsonProperty(PropertyName = "Chat Notifications")]
        public bool chat { get; set; }
        [JsonProperty(PropertyName = "Popup Notifications")]
        public bool popup { get; set; }
    }

    public class DiscordMessages
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool enabled { get; set; }
        [JsonProperty(PropertyName = "Webhook URL")]
        public string webhookURL { get; set; }
        [JsonProperty(PropertyName = "Embedded Messages")]
        public bool embedded { get; set; }
        [JsonProperty(PropertyName = "Embed Color")]
        public int embedColor { get; set; }
        [JsonProperty(PropertyName = "Embed Title")]
        public string embedTitle { get; set; }
    }

    public class UINotify
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool enabled { get; set; }
        [JsonProperty(PropertyName = "Notification Type")]
        public int type { get; set; }
    }

    public class Alerts
    {
        [JsonProperty(PropertyName = "Patrol Helicopter Alerts")]
        public bool patrolHeli { get; set; }
        [JsonProperty(PropertyName = "Cargo Ship Alerts")]
        public bool cargoShip { get; set; }
        [JsonProperty(PropertyName = "Cargo Ship Harbor Alerts")]
        public bool cargoShipHarbor { get; set; }
        [JsonProperty(PropertyName = "Cargo Plane Alerts")]
        public bool cargoPlane { get; set; }
        [JsonProperty(PropertyName = "CH47 Chinook Alerts")]
        public bool ch47 { get; set; }
        [JsonProperty(PropertyName = "Bradley APC Alerts")]
        public bool bradleyAPC { get; set; }
        [JsonProperty(PropertyName = "Travelling Vendor Alerts")]
        public bool travellingVendor { get; set; }
        [JsonProperty(PropertyName = "Excavator Activated Alerts")]
        public bool excavator { get; set; }
        [JsonProperty(PropertyName = "Excavator Supply Request Alerts")]
        public bool excavatorSupply { get; set; }
        [JsonProperty(PropertyName = "Hackable Crate Alerts")]
        public bool hackableCrateSpawn { get; set; }
        [JsonProperty(PropertyName = "Player Hacking Crate Alerts")]
        public bool hackingCrate { get; set; }
        [JsonProperty(PropertyName = "Supply Signal Alerts")]
        public bool supplySignal { get; set; }
        [JsonProperty(PropertyName = "Supply Drop Alerts")]
        public bool supplyDrop { get; set; }
        [JsonProperty(PropertyName = "Supply Drop Landed Alerts")]
        public bool supplyDropLand { get; set; }
    }

    public class Location
    {
        [JsonProperty(PropertyName = "Show Grid")]
        public bool showGrid { get; set; }
        [JsonProperty(PropertyName = "Show 'Oil Rig' Labels")]
        public bool showOilRigs { get; set; }
        [JsonProperty(PropertyName = "Show 'Cargo Ship' Label")]
        public bool showCargoShip { get; set; }
        [JsonProperty(PropertyName = "Show 'Excavator' Label")]
        public bool showExcavator { get; set; }
        [JsonProperty(PropertyName = "Hide Unmarked Grids")]
        public bool hideOffGrid { get; set; }
        [JsonProperty(PropertyName = "Show Coordinates")]
        public bool showCoords { get; set; }
        [JsonProperty(PropertyName = "Hide Y Coordinate")]
        public bool hideYCoord { get; set; }
        [JsonProperty(PropertyName = "Hide Coordinate Decimals")]
        public bool hideCoordDecimals { get; set; }
    }

    public class Misc
    {
        [JsonProperty(PropertyName = "Chat Icon (SteamID64)")]
        public ulong chatIcon { get; set; }
        [JsonProperty(PropertyName = "Hide Cargo Ship Crate Messages")]
        public bool hideCargoCrates { get; set; }
        [JsonProperty(PropertyName = "Hide Oil Rig Crate & Chinook Messages")]
        public bool hideRigCrates { get; set; }
        [JsonProperty(PropertyName = "Show Supply Drop Player")]
        public bool showSupplyPlayer { get; set; }
        [JsonProperty(PropertyName = "Hide Player-Called Supply Drop Messages")]
        public bool hideCalledSupply { get; set; }
        [JsonProperty(PropertyName = "Hide Random Supply Drop Messages")]
        public bool hideRandomSupply { get; set; }
    }

    public class Logging
    {
        [JsonProperty(PropertyName = "Log To Console")]
        public bool console { get; set; }
        [JsonProperty(PropertyName = "Log To File")]
        public bool file { get; set; }
        [JsonProperty(PropertyName = "Log All Events")]
        public bool allEvents { get; set; }
    }

    [JsonProperty(PropertyName = "Version (Do not modify)")]
    public Oxide.Core.VersionNumber Version { get; set; }
}

public class Notifications
{
    [JsonProperty(PropertyName = "Chat Notifications")]
    public bool chat { get; set; }
    [JsonProperty(PropertyName = "Popup Notifications")]
    public bool popup { get; set; }
}

public class DiscordMessages
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool enabled { get; set; }
    [JsonProperty(PropertyName = "Webhook URL")]
    public string webhookURL { get; set; }
    [JsonProperty(PropertyName = "Embedded Messages")]
    public bool embedded { get; set; }
    [JsonProperty(PropertyName = "Embed Color")]
    public int embedColor { get; set; }
    [JsonProperty(PropertyName = "Embed Title")]
    public string embedTitle { get; set; }
}

public class UINotify
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool enabled { get; set; }
    [JsonProperty(PropertyName = "Notification Type")]
    public int type { get; set; }
}

public class Alerts
{
    [JsonProperty(PropertyName = "Patrol Helicopter Alerts")]
    public bool patrolHeli { get; set; }
    [JsonProperty(PropertyName = "Cargo Ship Alerts")]
    public bool cargoShip { get; set; }
    [JsonProperty(PropertyName = "Cargo Ship Harbor Alerts")]
    public bool cargoShipHarbor { get; set; }
    [JsonProperty(PropertyName = "Cargo Plane Alerts")]
    public bool cargoPlane { get; set; }
    [JsonProperty(PropertyName = "CH47 Chinook Alerts")]
    public bool ch47 { get; set; }
    [JsonProperty(PropertyName = "Bradley APC Alerts")]
    public bool bradleyAPC { get; set; }
    [JsonProperty(PropertyName = "Travelling Vendor Alerts")]
    public bool travellingVendor { get; set; }
    [JsonProperty(PropertyName = "Excavator Activated Alerts")]
    public bool excavator { get; set; }
    [JsonProperty(PropertyName = "Excavator Supply Request Alerts")]
    public bool excavatorSupply { get; set; }
    [JsonProperty(PropertyName = "Hackable Crate Alerts")]
    public bool hackableCrateSpawn { get; set; }
    [JsonProperty(PropertyName = "Player Hacking Crate Alerts")]
    public bool hackingCrate { get; set; }
    [JsonProperty(PropertyName = "Supply Signal Alerts")]
    public bool supplySignal { get; set; }
    [JsonProperty(PropertyName = "Supply Drop Alerts")]
    public bool supplyDrop { get; set; }
    [JsonProperty(PropertyName = "Supply Drop Landed Alerts")]
    public bool supplyDropLand { get; set; }
}

public class Location
{
    [JsonProperty(PropertyName = "Show Grid")]
    public bool showGrid { get; set; }
    [JsonProperty(PropertyName = "Show 'Oil Rig' Labels")]
    public bool showOilRigs { get; set; }
    [JsonProperty(PropertyName = "Show 'Cargo Ship' Label")]
    public bool showCargoShip { get; set; }
    [JsonProperty(PropertyName = "Show 'Excavator' Label")]
    public bool showExcavator { get; set; }
    [JsonProperty(PropertyName = "Hide Unmarked Grids")]
    public bool hideOffGrid { get; set; }
    [JsonProperty(PropertyName = "Show Coordinates")]
    public bool showCoords { get; set; }
    [JsonProperty(PropertyName = "Hide Y Coordinate")]
    public bool hideYCoord { get; set; }
    [JsonProperty(PropertyName = "Hide Coordinate Decimals")]
    public bool hideCoordDecimals { get; set; }
}

public class Misc
{
    [JsonProperty(PropertyName = "Chat Icon (SteamID64)")]
    public ulong chatIcon { get; set; }
    [JsonProperty(PropertyName = "Hide Cargo Ship Crate Messages")]
    public bool hideCargoCrates { get; set; }
    [JsonProperty(PropertyName = "Hide Oil Rig Crate & Chinook Messages")]
    public bool hideRigCrates { get; set; }
    [JsonProperty(PropertyName = "Show Supply Drop Player")]
    public bool showSupplyPlayer { get; set; }
    [JsonProperty(PropertyName = "Hide Player-Called Supply Drop Messages")]
    public bool hideCalledSupply { get; set; }
    [JsonProperty(PropertyName = "Hide Random Supply Drop Messages")]
    public bool hideRandomSupply { get; set; }
}

public class Logging
{
    [JsonProperty(PropertyName = "Log To Console")]
    public bool console { get; set; }
    [JsonProperty(PropertyName = "Log To File")]
    public bool file { get; set; }
    [JsonProperty(PropertyName = "Log All Events")]
    public bool allEvents { get; set; }
}


```

---

## IndestructableBuildings by  - Disables damage being dealt to foundations or buildings on the server

```csharp
using System;
using System.Collections.Generic;
using Rust;

Oxide.Plugins
[Info("Indestructable Buildings", "Mughisi", "1.0.1", ResourceId=966)]
 class IndestructableBuildings : RustPlugin
{
     bool configChanged;
     string defaultChatPrefix;
     string defaultChatPrefixColor;
     string chatPrefix;
     string chatPrefixColor;
     bool defaultProtectFoundations;
     bool defaultProtectAllBuildingBlocks;
     bool defaultInformPlayer;
     float defaultInformInterval;
     bool protectFoundations;
     bool protectAllBuildingBlocks;
     bool informPlayer;
     float informInterval;
     string defaultHelpText;
     string defaultInformMessage;
     string helpText;
     string informMessage;
     class OnlinePlayer
    {
        public BasePlayer Player;
        public float LastInformTime;
        public OnlinePlayer(BasePlayer player);
    }

    [OnlinePlayers]
     Hash<BasePlayer, OnlinePlayer> onlinePlayers;
     DateTime epoch;
    protected override void LoadDefaultConfig();
     void Loaded();
     void LoadVariables();
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void OnPlayerInit(BasePlayer player);
     void SendHelpText(BasePlayer player);
     void Log(string message);
     void SendChatMessage(BasePlayer player, string message, object[] arguments);
     object GetConfigValue(string category, string setting, object defaultValue);
    private long GetTimestamp();
}

 class OnlinePlayer
{
    public BasePlayer Player;
    public float LastInformTime;
    public OnlinePlayer(BasePlayer player);
}


```

---

## IndividualDC by k1lly0u - Changes damage values for each weapon to any body part

```csharp
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("IndividualDC", "k1lly0u", "0.1.3", ResourceId = 1758)]
[Description("Damage controller for individual bones and weapons")]
 class IndividualDC : RustPlugin
{
    private ConfigData configData;
    public string[] Bodyparts;
     class ConfigData
    {
        public Dictionary<string, Dictionary<string, float>> Weapons { get; set; }
    }

     void OnServerInitialized();
    protected override void LoadDefaultConfig();
    private Dictionary<string, Dictionary<string, float>> SetConfigData();
    private void LoadVariables();
    private void LoadConfigVariables();
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    private bool InList(string weapon, string bodypart);
    [ChatCommand("scale")]
    private void cmdScale(BasePlayer player, string command, string[] args);
    private bool isAuth(BasePlayer player);
     Dictionary<string, string> messages;
}

 class ConfigData
{
    public Dictionary<string, Dictionary<string, float>> Weapons { get; set; }
}


```

---

## InfiniteAmmo by collectvood - Allows permission based Infinite Ammo

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Infinite Ammo", "collect_vood & Mughisi", "1.3.0")]
[Description("Allows permission based Infinite Ammo")]
public class InfiniteAmmo : CovalencePlugin
{
    private readonly string _usePermission;
    protected override void LoadDefaultMessages();
    private ConfigurationFile _configuration;
    public class ConfigurationFile
    {
        [JsonProperty(PropertyName = "Ammo Toggle Command")]
        public string AmmoToggleCommand;
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string ChatPrefix;
        [JsonProperty(PropertyName = "Chat Prefix Color")]
        public string ChatPrefixColor;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private StoredData _storedData;
    public class StoredData
    {
        [JsonProperty(PropertyName = "Active Infinite Ammo Player UserIds", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ulong> ActiveUsers;
    }

    private void SaveData();
    private void Unload();
    private void Init();
    private void OnWeaponFired(BaseProjectile projectile, BasePlayer player);
    private void OnRocketLaunched(BasePlayer player);
    private void OnMeleeThrown(BasePlayer player, Item item);
    private void OnServerSave();
    private void CommandToggleAmmo(IPlayer iPlayer, string cmd, string[] args);
    private void SendMessage(BasePlayer player, string message);
    private bool IsInfiniteAmmo(ulong userId);
    private bool CanUseInfiniteAmmo(BasePlayer player);
    private string GetMessage(string key, BasePlayer player, string[] args);
}

public class ConfigurationFile
{
    [JsonProperty(PropertyName = "Ammo Toggle Command")]
    public string AmmoToggleCommand;
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string ChatPrefix;
    [JsonProperty(PropertyName = "Chat Prefix Color")]
    public string ChatPrefixColor;
}

public class StoredData
{
    [JsonProperty(PropertyName = "Active Infinite Ammo Player UserIds", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ulong> ActiveUsers;
}


```

---

## InfiniteTool by birthdates - Allows player with permission to obtain unlimited ammo / durability

```csharp

Oxide.Plugins
[Info("Infinite Tool", "birthdates", "2.0.1")]
[Description("Allows player with permission to obtain unlimited ammo / durability")]
public class InfiniteTool : RustPlugin
{
    private readonly string permission_durability;
    private readonly string permission_explosives;
    private readonly string permission_rockets;
    private readonly string permission_ammo;
    private void Init();
     void OnWeaponFired(BaseProjectile projectile, BasePlayer player);
     void OnExplosiveThrown(BasePlayer player);
     void OnLoseCondition(Item item, float amount);
     void OnRocketLaunched(BasePlayer player);
}


```

---

## InfiniteVendingStock by Rustic0 - A very simple plugin for infinite buy amount and stock in NPC Vending Machines.

```csharp
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using static NPCVendingMachine;

Oxide.Plugins
[Info("Infinite Vending Stock", "Rustic", "1.0.2")]
[Description("A very simple plugin unblocking buy amount limit and giving unlimited stock to all NPC Vending Machines")]
public class InfiniteVendingStock : CovalencePlugin
{
    private void OnServerInitialized();
    private void OnVendingTransaction(NPCVendingMachine vendingMachine);
    private void RestockItems(NPCVendingMachine vendingMachine);
}


```

---

## InfoMenu by misticos - Shows server info and help with the ability to run popular commands

```csharp
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Info Menu", "Iv Misticos", "1.0.3")]
[Description("Show server info and help with the ability to run popular commands")]
 class InfoMenu : RustPlugin
{
    private const float InitialLoadDelay;
    private bool _firstCached;
    private bool _firstCaching;
    private const string PermissionRecacheUI;
    private const string CommandRecacheUI;
    private static InfoMenu _ins;
    [PluginReference]
    private Plugin ImageLibrary;
    [PluginReference]
    private Plugin PlaceholderAPI;
    private static Configuration _config;
    internal class Configuration
    {
        [JsonProperty(PropertyName = "Tabs", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<Tab> Tabs;
        [JsonProperty(PropertyName = "Static Elements", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<Button> Buttons;
        [JsonProperty(PropertyName = "Default Tab")]
        public string DefaultTab;
        [JsonProperty(PropertyName = "Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Commands;
        [JsonProperty(PropertyName = "Menu UI")]
        public UISettings UI;
        public class UISettings
        {
            [JsonProperty(PropertyName = "Menu Position")]
            public UIData.Position MenuPosition;
            [JsonProperty(PropertyName = "Menu Background Color")]
            public UIData.Colors MenuBackgroundColor;
            [JsonProperty(PropertyName = "Menu Color")]
            public UIData.Colors MenuColor;
            [JsonProperty(PropertyName = "Menu Title Background Position")]
            public UIData.Position MenuTitleBackgroundPosition;
            [JsonProperty(PropertyName = "Menu Title Background Color")]
            public UIData.Colors MenuTitleBackgroundColor;
            [JsonProperty(PropertyName = "Menu Title Text")]
            public string MenuTitleText;
            [JsonProperty(PropertyName = "Menu Title Text Anchor")]
            [JsonConverter(typeof(StringEnumConverter))]
            public TextAnchor MenuTitleTextAnchor;
            [JsonProperty(PropertyName = "Menu Title Placeholder API")]
            public bool MenuTitlePlaceholder;
            [JsonIgnore]
            public CuiElement ParsedMenuBackground;
            [JsonIgnore]
            public CuiElement ParsedMenuBackgroundButton;
            [JsonIgnore]
            public CuiElement ParsedMenu;
            [JsonIgnore]
            public CuiElement ParsedMenuTitleBackground;
            [JsonIgnore]
            public CuiElement ParsedMenuTitle;
            [JsonIgnore]
            public readonly string MenuName;
            [JsonIgnore]
            public readonly string MenuBackgroundName;
            [JsonIgnore]
            public readonly string MenuBackgroundButtonName;
            [JsonIgnore]
            public readonly string MenuTitleBackgroundName;
            [JsonIgnore]
            public readonly string MenuTitleName;
            public CuiElement GetMenuBackground();
            public CuiElement GetMenuBackgroundButton();
            public CuiElement GetMenu();
            public CuiElement GetMenuTitleBackground();
            public CuiElement GetMenuTitle(IPlayer player);
        }

        public class Button
        {
            [JsonProperty(PropertyName = "Permission")]
            public string Permission;
            [JsonProperty(PropertyName = "Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public List<CommandData> Commands;
            [JsonProperty(PropertyName = "UI")]
            public ButtonUI UI;
            public class ButtonUI
            {
                [JsonProperty(PropertyName = "Background Color")]
                public UIData.Colors Color;
                [JsonProperty(PropertyName = "Background Position")]
                public UIData.Position Position;
                [JsonProperty(PropertyName = "Text")]
                public string Text;
                [JsonProperty(PropertyName = "Text Anchor")]
                [JsonConverter(typeof(StringEnumConverter))]
                public TextAnchor TextAnchor;
                [JsonProperty(PropertyName = "Text Placeholder API")]
                public bool TextPlaceholder;
                [JsonIgnore]
                public CuiElement ParsedButtonBackground;
                [JsonIgnore]
                public CuiElement ParsedButton;
                [JsonIgnore]
                public CuiElement ParsedButtonText;
                [JsonIgnore]
                public string CommandName;
                [JsonIgnore]
                public string ButtonBackgroundName;
                [JsonIgnore]
                public string ButtonName;
                [JsonIgnore]
                public string ButtonTextName;
                public CuiElement GetButtonBackground();
                public CuiElement GetButton();
                public CuiElement GetButtonText(IPlayer player);
            }

        }

        public class Tab
        {
            [JsonProperty(PropertyName = "Technical Name")]
            public string Name;
            [JsonProperty(PropertyName = "Permission")]
            public string Permission;
            [JsonProperty(PropertyName = "Elements", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public List<Button> Buttons;
        }

        public class CommandData
        {
            [JsonProperty(PropertyName = "Command")]
            public string Command;
            [JsonProperty(PropertyName = "Arguments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public object[] Arguments;
        }

    }

    internal class UIData
    {
        public class Colors
        {
            [JsonProperty(PropertyName = "Transparency")]
            public float Transparency;
            [JsonProperty(PropertyName = "Color")]
            public string Color;
            [JsonProperty(PropertyName = "Link")]
            public string Link;
            [JsonProperty(PropertyName = "Sprite")]
            public string Sprite;
            [JsonProperty(PropertyName = "Material")]
            public string Material;
            [JsonIgnore]
            public string GetColor { get; set; }
            [JsonIgnore]
            public bool IsLink { get; set; }
        }

        public class Position
        {
            [JsonProperty(PropertyName = "Anchors")]
            public Anchors Anchors;
            [JsonProperty(PropertyName = "Offsets")]
            public Offsets Offsets;
        }

        public class Anchors
        {
            [JsonProperty(PropertyName = "Anchor Min X")]
            public float AnchorMinX;
            [JsonProperty(PropertyName = "Anchor Min Y")]
            public float AnchorMinY;
            [JsonProperty(PropertyName = "Anchor Max X")]
            public float AnchorMaxX;
            [JsonProperty(PropertyName = "Anchor Max Y")]
            public float AnchorMaxY;
            [JsonIgnore]
            public string AnchorsMin { get; set; }
            [JsonIgnore]
            public string AnchorsMax { get; set; }
        }

        public class Offsets
        {
            [JsonProperty(PropertyName = "Offset Min X")]
            public int OffsetMinX;
            [JsonProperty(PropertyName = "Offset Min Y")]
            public int OffsetMinY;
            [JsonProperty(PropertyName = "Offset Max X")]
            public int OffsetMaxX;
            [JsonProperty(PropertyName = "Offset Max Y")]
            public int OffsetMaxY;
            [JsonIgnore]
            public string OffsetsMin { get; set; }
            [JsonIgnore]
            public string OffsetsMax { get; set; }
        }

    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void CommandInfoMenuRecacheUI(IPlayer player, string command, string[] args);
    private void CommandInfoMenu(IPlayer player, string command, string[] args);
    private void CacheUI();
    private void InterfaceShow(IPlayer player, string tabName);
    private void InterfaceAddButton(IPlayer player, CuiElementContainer container, Configuration.Button button);
    private void InterfaceClose(BasePlayer player);
    private void SetupButton(Configuration.Button button);
    private void CacheButton(Configuration.Button button);
    private static string ProcessPlaceholders(IPlayer player, string text);
    private static string ImageLibraryGet(string name);
    private static void ImageLibraryLoad(string name, string link);
    private bool ImageLibraryLoaded();
    private bool PlaceholderAPILoaded();
    private static string GetColor(string hex, float alpha);
    private string GetMsg(string key, string userId);
}

internal class Configuration
{
    [JsonProperty(PropertyName = "Tabs", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<Tab> Tabs;
    [JsonProperty(PropertyName = "Static Elements", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<Button> Buttons;
    [JsonProperty(PropertyName = "Default Tab")]
    public string DefaultTab;
    [JsonProperty(PropertyName = "Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Commands;
    [JsonProperty(PropertyName = "Menu UI")]
    public UISettings UI;
    public class UISettings
    {
        [JsonProperty(PropertyName = "Menu Position")]
        public UIData.Position MenuPosition;
        [JsonProperty(PropertyName = "Menu Background Color")]
        public UIData.Colors MenuBackgroundColor;
        [JsonProperty(PropertyName = "Menu Color")]
        public UIData.Colors MenuColor;
        [JsonProperty(PropertyName = "Menu Title Background Position")]
        public UIData.Position MenuTitleBackgroundPosition;
        [JsonProperty(PropertyName = "Menu Title Background Color")]
        public UIData.Colors MenuTitleBackgroundColor;
        [JsonProperty(PropertyName = "Menu Title Text")]
        public string MenuTitleText;
        [JsonProperty(PropertyName = "Menu Title Text Anchor")]
        [JsonConverter(typeof(StringEnumConverter))]
        public TextAnchor MenuTitleTextAnchor;
        [JsonProperty(PropertyName = "Menu Title Placeholder API")]
        public bool MenuTitlePlaceholder;
        [JsonIgnore]
        public CuiElement ParsedMenuBackground;
        [JsonIgnore]
        public CuiElement ParsedMenuBackgroundButton;
        [JsonIgnore]
        public CuiElement ParsedMenu;
        [JsonIgnore]
        public CuiElement ParsedMenuTitleBackground;
        [JsonIgnore]
        public CuiElement ParsedMenuTitle;
        [JsonIgnore]
        public readonly string MenuName;
        [JsonIgnore]
        public readonly string MenuBackgroundName;
        [JsonIgnore]
        public readonly string MenuBackgroundButtonName;
        [JsonIgnore]
        public readonly string MenuTitleBackgroundName;
        [JsonIgnore]
        public readonly string MenuTitleName;
        public CuiElement GetMenuBackground();
        public CuiElement GetMenuBackgroundButton();
        public CuiElement GetMenu();
        public CuiElement GetMenuTitleBackground();
        public CuiElement GetMenuTitle(IPlayer player);
    }

    public class Button
    {
        [JsonProperty(PropertyName = "Permission")]
        public string Permission;
        [JsonProperty(PropertyName = "Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<CommandData> Commands;
        [JsonProperty(PropertyName = "UI")]
        public ButtonUI UI;
        public class ButtonUI
        {
            [JsonProperty(PropertyName = "Background Color")]
            public UIData.Colors Color;
            [JsonProperty(PropertyName = "Background Position")]
            public UIData.Position Position;
            [JsonProperty(PropertyName = "Text")]
            public string Text;
            [JsonProperty(PropertyName = "Text Anchor")]
            [JsonConverter(typeof(StringEnumConverter))]
            public TextAnchor TextAnchor;
            [JsonProperty(PropertyName = "Text Placeholder API")]
            public bool TextPlaceholder;
            [JsonIgnore]
            public CuiElement ParsedButtonBackground;
            [JsonIgnore]
            public CuiElement ParsedButton;
            [JsonIgnore]
            public CuiElement ParsedButtonText;
            [JsonIgnore]
            public string CommandName;
            [JsonIgnore]
            public string ButtonBackgroundName;
            [JsonIgnore]
            public string ButtonName;
            [JsonIgnore]
            public string ButtonTextName;
            public CuiElement GetButtonBackground();
            public CuiElement GetButton();
            public CuiElement GetButtonText(IPlayer player);
        }

    }

    public class Tab
    {
        [JsonProperty(PropertyName = "Technical Name")]
        public string Name;
        [JsonProperty(PropertyName = "Permission")]
        public string Permission;
        [JsonProperty(PropertyName = "Elements", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<Button> Buttons;
    }

    public class CommandData
    {
        [JsonProperty(PropertyName = "Command")]
        public string Command;
        [JsonProperty(PropertyName = "Arguments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public object[] Arguments;
    }

}

public class UISettings
{
    [JsonProperty(PropertyName = "Menu Position")]
    public UIData.Position MenuPosition;
    [JsonProperty(PropertyName = "Menu Background Color")]
    public UIData.Colors MenuBackgroundColor;
    [JsonProperty(PropertyName = "Menu Color")]
    public UIData.Colors MenuColor;
    [JsonProperty(PropertyName = "Menu Title Background Position")]
    public UIData.Position MenuTitleBackgroundPosition;
    [JsonProperty(PropertyName = "Menu Title Background Color")]
    public UIData.Colors MenuTitleBackgroundColor;
    [JsonProperty(PropertyName = "Menu Title Text")]
    public string MenuTitleText;
    [JsonProperty(PropertyName = "Menu Title Text Anchor")]
    [JsonConverter(typeof(StringEnumConverter))]
    public TextAnchor MenuTitleTextAnchor;
    [JsonProperty(PropertyName = "Menu Title Placeholder API")]
    public bool MenuTitlePlaceholder;
    [JsonIgnore]
    public CuiElement ParsedMenuBackground;
    [JsonIgnore]
    public CuiElement ParsedMenuBackgroundButton;
    [JsonIgnore]
    public CuiElement ParsedMenu;
    [JsonIgnore]
    public CuiElement ParsedMenuTitleBackground;
    [JsonIgnore]
    public CuiElement ParsedMenuTitle;
    [JsonIgnore]
    public readonly string MenuName;
    [JsonIgnore]
    public readonly string MenuBackgroundName;
    [JsonIgnore]
    public readonly string MenuBackgroundButtonName;
    [JsonIgnore]
    public readonly string MenuTitleBackgroundName;
    [JsonIgnore]
    public readonly string MenuTitleName;
    public CuiElement GetMenuBackground();
    public CuiElement GetMenuBackgroundButton();
    public CuiElement GetMenu();
    public CuiElement GetMenuTitleBackground();
    public CuiElement GetMenuTitle(IPlayer player);
}

public class Button
{
    [JsonProperty(PropertyName = "Permission")]
    public string Permission;
    [JsonProperty(PropertyName = "Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<CommandData> Commands;
    [JsonProperty(PropertyName = "UI")]
    public ButtonUI UI;
    public class ButtonUI
    {
        [JsonProperty(PropertyName = "Background Color")]
        public UIData.Colors Color;
        [JsonProperty(PropertyName = "Background Position")]
        public UIData.Position Position;
        [JsonProperty(PropertyName = "Text")]
        public string Text;
        [JsonProperty(PropertyName = "Text Anchor")]
        [JsonConverter(typeof(StringEnumConverter))]
        public TextAnchor TextAnchor;
        [JsonProperty(PropertyName = "Text Placeholder API")]
        public bool TextPlaceholder;
        [JsonIgnore]
        public CuiElement ParsedButtonBackground;
        [JsonIgnore]
        public CuiElement ParsedButton;
        [JsonIgnore]
        public CuiElement ParsedButtonText;
        [JsonIgnore]
        public string CommandName;
        [JsonIgnore]
        public string ButtonBackgroundName;
        [JsonIgnore]
        public string ButtonName;
        [JsonIgnore]
        public string ButtonTextName;
        public CuiElement GetButtonBackground();
        public CuiElement GetButton();
        public CuiElement GetButtonText(IPlayer player);
    }

}

public class ButtonUI
{
    [JsonProperty(PropertyName = "Background Color")]
    public UIData.Colors Color;
    [JsonProperty(PropertyName = "Background Position")]
    public UIData.Position Position;
    [JsonProperty(PropertyName = "Text")]
    public string Text;
    [JsonProperty(PropertyName = "Text Anchor")]
    [JsonConverter(typeof(StringEnumConverter))]
    public TextAnchor TextAnchor;
    [JsonProperty(PropertyName = "Text Placeholder API")]
    public bool TextPlaceholder;
    [JsonIgnore]
    public CuiElement ParsedButtonBackground;
    [JsonIgnore]
    public CuiElement ParsedButton;
    [JsonIgnore]
    public CuiElement ParsedButtonText;
    [JsonIgnore]
    public string CommandName;
    [JsonIgnore]
    public string ButtonBackgroundName;
    [JsonIgnore]
    public string ButtonName;
    [JsonIgnore]
    public string ButtonTextName;
    public CuiElement GetButtonBackground();
    public CuiElement GetButton();
    public CuiElement GetButtonText(IPlayer player);
}

public class Tab
{
    [JsonProperty(PropertyName = "Technical Name")]
    public string Name;
    [JsonProperty(PropertyName = "Permission")]
    public string Permission;
    [JsonProperty(PropertyName = "Elements", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<Button> Buttons;
}

public class CommandData
{
    [JsonProperty(PropertyName = "Command")]
    public string Command;
    [JsonProperty(PropertyName = "Arguments", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public object[] Arguments;
}

internal class UIData
{
    public class Colors
    {
        [JsonProperty(PropertyName = "Transparency")]
        public float Transparency;
        [JsonProperty(PropertyName = "Color")]
        public string Color;
        [JsonProperty(PropertyName = "Link")]
        public string Link;
        [JsonProperty(PropertyName = "Sprite")]
        public string Sprite;
        [JsonProperty(PropertyName = "Material")]
        public string Material;
        [JsonIgnore]
        public string GetColor { get; set; }
        [JsonIgnore]
        public bool IsLink { get; set; }
    }

    public class Position
    {
        [JsonProperty(PropertyName = "Anchors")]
        public Anchors Anchors;
        [JsonProperty(PropertyName = "Offsets")]
        public Offsets Offsets;
    }

    public class Anchors
    {
        [JsonProperty(PropertyName = "Anchor Min X")]
        public float AnchorMinX;
        [JsonProperty(PropertyName = "Anchor Min Y")]
        public float AnchorMinY;
        [JsonProperty(PropertyName = "Anchor Max X")]
        public float AnchorMaxX;
        [JsonProperty(PropertyName = "Anchor Max Y")]
        public float AnchorMaxY;
        [JsonIgnore]
        public string AnchorsMin { get; set; }
        [JsonIgnore]
        public string AnchorsMax { get; set; }
    }

    public class Offsets
    {
        [JsonProperty(PropertyName = "Offset Min X")]
        public int OffsetMinX;
        [JsonProperty(PropertyName = "Offset Min Y")]
        public int OffsetMinY;
        [JsonProperty(PropertyName = "Offset Max X")]
        public int OffsetMaxX;
        [JsonProperty(PropertyName = "Offset Max Y")]
        public int OffsetMaxY;
        [JsonIgnore]
        public string OffsetsMin { get; set; }
        [JsonIgnore]
        public string OffsetsMax { get; set; }
    }

}

public class Colors
{
    [JsonProperty(PropertyName = "Transparency")]
    public float Transparency;
    [JsonProperty(PropertyName = "Color")]
    public string Color;
    [JsonProperty(PropertyName = "Link")]
    public string Link;
    [JsonProperty(PropertyName = "Sprite")]
    public string Sprite;
    [JsonProperty(PropertyName = "Material")]
    public string Material;
    [JsonIgnore]
    public string GetColor { get; set; }
    [JsonIgnore]
    public bool IsLink { get; set; }
}

public class Position
{
    [JsonProperty(PropertyName = "Anchors")]
    public Anchors Anchors;
    [JsonProperty(PropertyName = "Offsets")]
    public Offsets Offsets;
}

public class Anchors
{
    [JsonProperty(PropertyName = "Anchor Min X")]
    public float AnchorMinX;
    [JsonProperty(PropertyName = "Anchor Min Y")]
    public float AnchorMinY;
    [JsonProperty(PropertyName = "Anchor Max X")]
    public float AnchorMaxX;
    [JsonProperty(PropertyName = "Anchor Max Y")]
    public float AnchorMaxY;
    [JsonIgnore]
    public string AnchorsMin { get; set; }
    [JsonIgnore]
    public string AnchorsMax { get; set; }
}

public class Offsets
{
    [JsonProperty(PropertyName = "Offset Min X")]
    public int OffsetMinX;
    [JsonProperty(PropertyName = "Offset Min Y")]
    public int OffsetMinY;
    [JsonProperty(PropertyName = "Offset Max X")]
    public int OffsetMaxX;
    [JsonProperty(PropertyName = "Offset Max Y")]
    public int OffsetMaxY;
    [JsonIgnore]
    public string OffsetsMin { get; set; }
    [JsonIgnore]
    public string OffsetsMax { get; set; }
}


```

---

## InfoPanel by DevGonzi - GUI panel with in-game clock, online players, sleepers and custom messages, etc.

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("InfoPanel", "Gonzi", "1.0.9")]
[Description("A little panel with useful information.")]
public class InfoPanel : RustPlugin
{
    private static string DefaultFontColor;
    public bool IsReady;
    private Timer TestTimer;
    private Dictionary<string, Dictionary<string, IPanel>> PlayerPanels;
    private Dictionary<string, Dictionary<string, IPanel>> PlayerDockPanels;
    private Dictionary<string, List<string>> LoadedPluginPanels;
    private static PluginConfig Settings;
    private readonly List<string> TimeFormats;
     PluginConfig DefaultConfig();
     class PluginConfig
    {
        public Dictionary<string, DockConfig> Docks { get; set; }
        public Dictionary<string, PanelConfig> Panels { get; set; }
        public int CoordType { get; set; }
        public Dictionary<string, Dictionary<string, PanelConfig>> ThirdPartyPanels { get; set; }
        public List<string> Messages { get; set; }
        public List<string> TimeFormats { get; set; }
        public Dictionary<string, string> CompassDirections { get; set; }
        public T GetPanelSettingsValue(string Panel, string Setting, T defaultValue);
        public bool CheckPanelAvailability(string Panel);
    }

     class DockConfig
    {
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public bool Available { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string AnchorX { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string AnchorY { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public float Width { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public float Height { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string BackgroundColor { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string Margin { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public PanelImageConfig Image { get; set; }
    }

     class BasePanelConfig
    {
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public bool Available { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string AnchorX { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string AnchorY { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public float Width { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public float Height { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int Order { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string BackgroundColor { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string Margin { get; set; }
    }

     class PanelConfig : BasePanelConfig
    {
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public bool Autoload { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string Dock { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public Dictionary<string, object> PanelSettings { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public PanelImageConfig Image { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public PanelTextConfig Text { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public float FadeOut { get; set; }
        public string Permission { get; set; }
    }

     class PanelTextConfig : BasePanelConfig
    {
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public new float Width { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public TextAnchor Align { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string FontColor { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int FontSize { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string Content { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public float FadeIn { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public float FadeOut { get; set; }
    }

     class PanelImageConfig : BasePanelConfig
    {
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public new float Width { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string Url { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string Color { get; set; }
    }

    protected void LoadConfigValues();
     int PanelReOrder(string DockName, string AnchorX);
    protected override void LoadDefaultConfig();
     void Init();
     void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void OnPlayerSleepEnded(BasePlayer player);
    private void OnEntitySpawned(BaseEntity Entity);
    private void Unload();
     void OnPluginUnloaded(Plugin plugin);
     void OnServerSave();
     void OnServerShutdown();
    private void LoadPanels(BasePlayer Player);
    private IPanel LoadDockPanel(string DockName, BasePlayer Player);
    private void LoadPanel(IPanel Dock, string PanelName, PanelConfig PCfg);
    private Watch Clock;
    private int ClockUpdateFrequency;
    private Timer TimeUpdater;
    public class Watch
    {
         string ClockFormat;
        public int RefresRate;
        public bool Available;
         TOD_Sky Sky;
        public Watch(int RefreshRate, bool Available);
        public string GetServerTime(string PlayerID, StoredData storedData);
        public string GetSkyTime(string PlayerID, StoredData storedData);
        public string ShowTime(string PlayerID, StoredData storedData);
        public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
    }

    private Messenger MessageBox;
    private Timer MsgUpdater;
    private int MessageUpdateFrequency;
    private List<string> Messages;
    private bool MessageBoxAvailable;
    public class Messenger
    {
         List<string> Messages;
        public int RefreshRate;
        private int Counter;
        private string MsgOrder;
        public Messenger(List<string> msgs, int RefreshRate, string MsgOrder);
        public string GetMessage();
        private void RefreshCounter();
        public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
    }

    private Timer HeliAttack;
    private Timer RadiationUpdater;
    private AirplaneEvent Airplane;
    private List<CargoPlane> ActivePlanes;
    private bool AirplaneTimer;
    private HelicopterEvent Helicopter;
    private List<PatrolHelicopter> ActiveHelicopters;
    private bool HelicopterTimer;
    private CargoshipEvent Cargoship;
    private List<CargoShip> ActiveCargoships;
    private bool CargoshipTimer;
    private ChinookEvent Chinook;
    private List<CH47Helicopter> ActiveChinooks;
    private bool ChinookTimer;
    private Radiation Rad;
    private PatrolHelicopter ActiveHelicopter;
    private CH47Helicopter ActiveChinook;
    private CargoShip ActiveCargoship;
    private BradleyEvent Bradley;
    private List<BradleyAPC> ActiveBradleys;
    private bool BradleyTimer;
    public class AirplaneEvent
    {
        public bool isActive;
        public Color ImageColor;
        public AirplaneEvent();
        public void SetActivity(bool active);
        public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
    }

    public class HelicopterEvent
    {
        public bool isActive;
        public Color ImageColor;
        public HelicopterEvent();
        public void SetActivity(bool active);
        public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
    }

    public class CargoshipEvent
    {
        public bool isActive;
        public Color ImageColor;
        public CargoshipEvent();
        public void SetActivity(bool active);
        public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
    }

    public class ChinookEvent
    {
        public bool isActive;
        public Color ImageColor;
        public ChinookEvent();
        public void SetActivity(bool active);
        public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
    }

    public class Radiation
    {
         bool isActive;
        public Color ImageColor;
        public int RefreshRate;
        public Radiation(int RefreshRate);
        public void SetActivity(bool active);
        public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
    }

    public class BradleyEvent
    {
        public bool isActive;
        public Color ImageColor;
        public BradleyEvent();
        public void SetActivity(bool active);
        public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
    }

    public void CheckAirplane();
    public void CheckHelicopter();
    public void CheckCargoship();
    public void CheckBradley();
    public void CheckChinook();
    private Balance Bala;
    private Timer BalanceUpdater;
    public class Balance
    {
        public int RefreshRate;
        public Balance(int RefreshRate);
        public double GetBalance(string PlayerID);
        public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
    }

    private Points Poi;
    private Timer PointsUpdater;
    public class Points
    {
        public int RefreshRate;
        public Points(int RefreshRate);
        public int GetPoints(string PlayerID);
        public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
    }

    private Coordinates Coord;
    private Timer CoordUpdater;
    public class Coordinates
    {
        public int RefreshRate;
        public Coordinates(int RefreshRate);
        public string GetCoord(string PlayerID);
        public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
    }

    private Compass CompassObj;
    private Timer CompassUpdater;
    public class Compass
    {
        public int RefreshRate;
        public Compass(int RefreshRate);
        public string GetDirection(string PlayerID);
        public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
    }

    [ChatCommand("ipanel")]
    private void IPanelCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("iptest")]
    private void IPaCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("iperr")]
    private void IPCommand(BasePlayer player, string command, string[] args);
    public static StoredData storedData;
    public class StoredData
    {
        public Dictionary<string, PlayerSettings> Players;
        public StoredData();
        public bool CheckPlayerData(BasePlayer Player);
        public T GetPlayerSettings(string PlayerID, string Key, T DefaultValue);
        public T GetPlayerPanelSettings(BasePlayer Player, string Panel, string Key, T DefaultValue);
        public T GetPlayerPanelSettings(string PlayerID, string Panel, string Key, T DefaultValue);
    }

    public class PlayerSettings
    {
        public string UserId;
        public Dictionary<string, string> Settings;
        public Dictionary<string, Dictionary<string, string>> PanelSettings;
        public PlayerSettings();
        public PlayerSettings(BasePlayer player);
        public void SetSetting(string Key, string Value);
        public void SetPanelSetting(string Panel, string Key, string Value);
        public T GetPanelSetting(string Panel, string Key, T DefaultValue);
        public T GetSetting(string Key, T DefaultValue);
    }

    public void LoadData();
    public void SaveData();
    public void ChangePlayerSettings(BasePlayer player, string Key, string Value);
    public void ChangePlayerPanelSettings(BasePlayer player, string Panel, string Key, string Value);
    public List<string> Err;
    public Dictionary<string, List<string>> ErrA;
    public Dictionary<string, int> ErrB;
    public Dictionary<string, int> ErrD;
    [JsonObject(MemberSerialization.OptIn)]
    public class IPanel
    {
        [JsonProperty("name")]
        public string Name { get; set; }
        [JsonProperty("parent")]
        public string ParentName { get; set; }
        [JsonProperty("components")]
        public List<ICuiComponent> Components;
        [JsonProperty("fadeOut")]
        public float FadeOut { get; set; }
        public Vector2 HorizontalPosition { get; set; }
        public Vector2 VerticalPosition { get; set; }
        public string AnchorX { get; set; }
        public string AnchorY { get; set; }
        public Vector4 Padding;
        public Vector4 Margin { get; set; }
        public float Width { get; set; }
        public float Height { get; set; }
        public Color _BGColor;
        public Color BackgroundColor { get; set; }
        public int Order;
        public float _VerticalOffset;
        public float VerticalOffset { get; set; }
        public List<string> Childs;
        public CuiRectTransformComponent RecTransform;
        public CuiImageComponent ImageComponent;
         BasePlayer Owner;
        public string DockName;
        public bool IsActive;
        public bool IsHidden;
        public bool IsPanel;
        public bool IsDock;
        public bool Autoload;
        private Dictionary<string, Dictionary<string, IPanel>> playerPanels;
        private Dictionary<string, Dictionary<string, IPanel>> playerDockPanels;
        public IPanel(string name, BasePlayer Player, Dictionary<string, Dictionary<string, IPanel>> playerPanels, Dictionary<string, Dictionary<string, IPanel>> playerDockPanels);
        public void SetAnchorXY(string Horizontal, string Vertical);
        public void SetHorizontalPosition();
        public void SetVerticalPosition();
         float FullWidth();
         float GetSiblingsFullWidth();
        public string ToJson();
        public float GetOffset();
        public string GetJson(bool Brackets);
        public int GetLastChild();
        public IPanelText AddText(string Name);
        public IPanelRawImage AddImage(string Name);
        public IPanel AddPanel(string Name);
         List<string> GetActiveAfterThis();
        public Dictionary<string, IPanel> GetChilds();
        public IPanel GetParent();
        public List<IPanel> GetSiblings();
        public IPanel GetPanel(string PName);
        public IPanel GetDock();
        public void Hide();
        public void Reveal();
         void ReDrawPanels(List<string> PanelsName);
        public void ShowPanel(bool Childs);
         void ShowPanelIfHidden(bool Childs);
        public void DestroyPanel(bool Redraw);
        public virtual void Refresh();
        public void Remover();
        protected string ColorToString(Color color);
    }

    public class IPanelText : IPanel
    {
        public string Content { get; set; }
        public TextAnchor Align { get; set; }
        public int FontSize { get; set; }
        public Color _FontColor;
        public Color FontColor { get; set; }
        public CuiTextComponent TextComponent;
        public IPanelText(string Name, BasePlayer Player, Dictionary<string, Dictionary<string, IPanel>> playerPanels, Dictionary<string, Dictionary<string, IPanel>> playerDockPanels);
        public void RefreshText(BasePlayer player, string text);
    }

    public class IPanelRawImage : IPanel
    {
        public string Url { get; set; }
        public Color _Color;
        public Color Color { get; set; }
        public CuiRawImageComponent RawImageComponent;
        public IPanelRawImage(string Name, BasePlayer Player, Dictionary<string, Dictionary<string, IPanel>> playerPanels, Dictionary<string, Dictionary<string, IPanel>> playerDockPanels);
    }

    private void DestroyGUI(BasePlayer player);
     void GUITimerInit(BasePlayer player);
    private void InitializeGUI(BasePlayer player);
    private void RevealGUI(BasePlayer player);
    private void RefreshOnlinePlayers();
    private void RefreshSleepers();
    private bool PanelRegister(string PluginName, string PanelName, string json);
    private bool ShowPanel(string PluginName, string PanelName, string PlayerId);
    private bool HidePanel(string PluginName, string PanelName, string PlayerId);
    private bool RefreshPanel(string PluginName, string PanelName, string PlayerId);
    private void SetPanelAttribute(string PluginName, string PanelName, string Attribute, string Value, string PlayerId);
    private bool SendPanelInfo(string PluginName, List<string> Panels);
    private bool IsPlayerGUILoaded(string PlayerId);
    internal static Vector4 Vector4Parser(string p);
    public static string FormatGridReference(Vector3 position);
    public static string NumberToLetter(int num);
}

 class PluginConfig
{
    public Dictionary<string, DockConfig> Docks { get; set; }
    public Dictionary<string, PanelConfig> Panels { get; set; }
    public int CoordType { get; set; }
    public Dictionary<string, Dictionary<string, PanelConfig>> ThirdPartyPanels { get; set; }
    public List<string> Messages { get; set; }
    public List<string> TimeFormats { get; set; }
    public Dictionary<string, string> CompassDirections { get; set; }
    public T GetPanelSettingsValue(string Panel, string Setting, T defaultValue);
    public bool CheckPanelAvailability(string Panel);
}

 class DockConfig
{
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public bool Available { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public string AnchorX { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public string AnchorY { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public float Width { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public float Height { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public string BackgroundColor { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public string Margin { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public PanelImageConfig Image { get; set; }
}

 class BasePanelConfig
{
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public bool Available { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public string AnchorX { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public string AnchorY { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public float Width { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public float Height { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public int Order { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public string BackgroundColor { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public string Margin { get; set; }
}

 class PanelConfig : BasePanelConfig
{
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public bool Autoload { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public string Dock { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public Dictionary<string, object> PanelSettings { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public PanelImageConfig Image { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public PanelTextConfig Text { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public float FadeOut { get; set; }
    public string Permission { get; set; }
}

 class PanelTextConfig : BasePanelConfig
{
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public new float Width { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public TextAnchor Align { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public string FontColor { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public int FontSize { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public string Content { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public float FadeIn { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public float FadeOut { get; set; }
}

 class PanelImageConfig : BasePanelConfig
{
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public new float Width { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public string Url { get; set; }
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public string Color { get; set; }
}

public class Watch
{
     string ClockFormat;
    public int RefresRate;
    public bool Available;
     TOD_Sky Sky;
    public Watch(int RefreshRate, bool Available);
    public string GetServerTime(string PlayerID, StoredData storedData);
    public string GetSkyTime(string PlayerID, StoredData storedData);
    public string ShowTime(string PlayerID, StoredData storedData);
    public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
}

public class Messenger
{
     List<string> Messages;
    public int RefreshRate;
    private int Counter;
    private string MsgOrder;
    public Messenger(List<string> msgs, int RefreshRate, string MsgOrder);
    public string GetMessage();
    private void RefreshCounter();
    public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
}

public class AirplaneEvent
{
    public bool isActive;
    public Color ImageColor;
    public AirplaneEvent();
    public void SetActivity(bool active);
    public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
}

public class HelicopterEvent
{
    public bool isActive;
    public Color ImageColor;
    public HelicopterEvent();
    public void SetActivity(bool active);
    public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
}

public class CargoshipEvent
{
    public bool isActive;
    public Color ImageColor;
    public CargoshipEvent();
    public void SetActivity(bool active);
    public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
}

public class ChinookEvent
{
    public bool isActive;
    public Color ImageColor;
    public ChinookEvent();
    public void SetActivity(bool active);
    public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
}

public class Radiation
{
     bool isActive;
    public Color ImageColor;
    public int RefreshRate;
    public Radiation(int RefreshRate);
    public void SetActivity(bool active);
    public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
}

public class BradleyEvent
{
    public bool isActive;
    public Color ImageColor;
    public BradleyEvent();
    public void SetActivity(bool active);
    public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
}

public class Balance
{
    public int RefreshRate;
    public Balance(int RefreshRate);
    public double GetBalance(string PlayerID);
    public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
}

public class Points
{
    public int RefreshRate;
    public Points(int RefreshRate);
    public int GetPoints(string PlayerID);
    public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
}

public class Coordinates
{
    public int RefreshRate;
    public Coordinates(int RefreshRate);
    public string GetCoord(string PlayerID);
    public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
}

public class Compass
{
    public int RefreshRate;
    public Compass(int RefreshRate);
    public string GetDirection(string PlayerID);
    public void Refresh(StoredData storedData, Dictionary<string, Dictionary<string, IPanel>> panels);
}

public class StoredData
{
    public Dictionary<string, PlayerSettings> Players;
    public StoredData();
    public bool CheckPlayerData(BasePlayer Player);
    public T GetPlayerSettings(string PlayerID, string Key, T DefaultValue);
    public T GetPlayerPanelSettings(BasePlayer Player, string Panel, string Key, T DefaultValue);
    public T GetPlayerPanelSettings(string PlayerID, string Panel, string Key, T DefaultValue);
}

public class PlayerSettings
{
    public string UserId;
    public Dictionary<string, string> Settings;
    public Dictionary<string, Dictionary<string, string>> PanelSettings;
    public PlayerSettings();
    public PlayerSettings(BasePlayer player);
    public void SetSetting(string Key, string Value);
    public void SetPanelSetting(string Panel, string Key, string Value);
    public T GetPanelSetting(string Panel, string Key, T DefaultValue);
    public T GetSetting(string Key, T DefaultValue);
}

[JsonObject(MemberSerialization.OptIn)]
public class IPanel
{
    [JsonProperty("name")]
    public string Name { get; set; }
    [JsonProperty("parent")]
    public string ParentName { get; set; }
    [JsonProperty("components")]
    public List<ICuiComponent> Components;
    [JsonProperty("fadeOut")]
    public float FadeOut { get; set; }
    public Vector2 HorizontalPosition { get; set; }
    public Vector2 VerticalPosition { get; set; }
    public string AnchorX { get; set; }
    public string AnchorY { get; set; }
    public Vector4 Padding;
    public Vector4 Margin { get; set; }
    public float Width { get; set; }
    public float Height { get; set; }
    public Color _BGColor;
    public Color BackgroundColor { get; set; }
    public int Order;
    public float _VerticalOffset;
    public float VerticalOffset { get; set; }
    public List<string> Childs;
    public CuiRectTransformComponent RecTransform;
    public CuiImageComponent ImageComponent;
     BasePlayer Owner;
    public string DockName;
    public bool IsActive;
    public bool IsHidden;
    public bool IsPanel;
    public bool IsDock;
    public bool Autoload;
    private Dictionary<string, Dictionary<string, IPanel>> playerPanels;
    private Dictionary<string, Dictionary<string, IPanel>> playerDockPanels;
    public IPanel(string name, BasePlayer Player, Dictionary<string, Dictionary<string, IPanel>> playerPanels, Dictionary<string, Dictionary<string, IPanel>> playerDockPanels);
    public void SetAnchorXY(string Horizontal, string Vertical);
    public void SetHorizontalPosition();
    public void SetVerticalPosition();
     float FullWidth();
     float GetSiblingsFullWidth();
    public string ToJson();
    public float GetOffset();
    public string GetJson(bool Brackets);
    public int GetLastChild();
    public IPanelText AddText(string Name);
    public IPanelRawImage AddImage(string Name);
    public IPanel AddPanel(string Name);
     List<string> GetActiveAfterThis();
    public Dictionary<string, IPanel> GetChilds();
    public IPanel GetParent();
    public List<IPanel> GetSiblings();
    public IPanel GetPanel(string PName);
    public IPanel GetDock();
    public void Hide();
    public void Reveal();
     void ReDrawPanels(List<string> PanelsName);
    public void ShowPanel(bool Childs);
     void ShowPanelIfHidden(bool Childs);
    public void DestroyPanel(bool Redraw);
    public virtual void Refresh();
    public void Remover();
    protected string ColorToString(Color color);
}

public class IPanelText : IPanel
{
    public string Content { get; set; }
    public TextAnchor Align { get; set; }
    public int FontSize { get; set; }
    public Color _FontColor;
    public Color FontColor { get; set; }
    public CuiTextComponent TextComponent;
    public IPanelText(string Name, BasePlayer Player, Dictionary<string, Dictionary<string, IPanel>> playerPanels, Dictionary<string, Dictionary<string, IPanel>> playerDockPanels);
    public void RefreshText(BasePlayer player, string text);
}

public class IPanelRawImage : IPanel
{
    public string Url { get; set; }
    public Color _Color;
    public Color Color { get; set; }
    public CuiRawImageComponent RawImageComponent;
    public IPanelRawImage(string Name, BasePlayer Player, Dictionary<string, Dictionary<string, IPanel>> playerPanels, Dictionary<string, Dictionary<string, IPanel>> playerDockPanels);
}


```

---

## IngameClockGUI by EsCL337 - Displays in-game time and server time

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[Info("Ingame Clock GUI", "deer_SWAG", "0.0.7", ResourceId = 1245)]
[Description("Displays ingame and server time")]
public class IngameClockGUI : RustPlugin
{
    const string databaseName;
    const int isClockEnabled;
    const int isServerTime;
    const string defaultInfoSize;
     class Data
    {
        public HashSet<Player> Players;
        public Data();
    }

     class Player
    {
        public ulong userID;
        public short options;
        public Player();
        public Player(ulong id, short o);
    }

    private class TimedInfo
    {
        public DateTime startTime;
        public DateTime endTime;
        public string text;
        public bool serverTime;
        public string size;
        public TimedInfo(DateTime st, DateTime et, string txt, bool server, string s);
    }

    private string clockJson;
    private string infoJson;
     Data data;
     Timer updateTimer;
     TOD_Sky sky;
     DateTime dt;
     string time;
     DateTime gameTime;
     DateTime serverTime;
    private TimedInfo currentTI;
    private List<TimedInfo> tiList;
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     void Unload();
    [ChatCommand("clock")]
     void cmdChat(BasePlayer player, string command, string[] args);
     void AddGUI();
    private void UpdateTime();
    private void DestroyGUI();
    private void ShowTime();
     void ShowInfo(string text, string iSize);
     void UpdateInfo();
     void DestroyInfo();
     void SaveData();
     bool GetOption(int options, int option);
     void SendHelpText(BasePlayer player);
    private TimedInfo GetTimedInfo(string source);
     void CheckCreateConfig();
}

 class Data
{
    public HashSet<Player> Players;
    public Data();
}

 class Player
{
    public ulong userID;
    public short options;
    public Player();
    public Player(ulong id, short o);
}

private class TimedInfo
{
    public DateTime startTime;
    public DateTime endTime;
    public string text;
    public bool serverTime;
    public string size;
    public TimedInfo(DateTime st, DateTime et, string txt, bool server, string s);
}


```

---

## InstantBarrels by Mevent - Allows you to destroy barrels with one hit

```csharp

Oxide.Plugins
[Info("Instant Barrels", "Mevent", "1.0.3")]
[Description("Allows you to destroy barrels and roadsigns with one hit")]
public class InstantBarrels : CovalencePlugin
{
    private const string PermUse;
    private const string PermRoadSigns;
    private void OnServerInitialized();
    private void OnEntityTakeDamage(LootContainer container, HitInfo info);
}


```

---

## InstantBarricades by lunedi - This plugin allows players to break wooden barricades in Markets etc. instantly!

```csharp
using ConVar;
using JetBrains.Annotations;
using Oxide.Core;
using Oxide.Core.Libraries;
using ProtoBuf;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using UnityEngine;

Oxide.Plugins
[Info("InstantBarricades", "4yzhen", "1.0.0")]
[Description("A Simple Light Weight plugin to destroy wooden barricades in Supermarkets. etc")]
public class InstantBarricades : RustPlugin
{
    private void OnEntityTakeDamage(BaseEntity entity, HitInfo info);
}


```

---

## InstantBuy by bushhy - No need to wait between transactions on a vending machine

```csharp

Oxide.Plugins
[Info("Instant Buy", "Jake_Rich/collect_vood/Bushhy", "1.0.3")]
[Description("Vending Machine has no delay")]
public class InstantBuy : CovalencePlugin
{
    private const string permUse;
    private void Init();
    private object OnBuyVendingItem(VendingMachine machine, BasePlayer player, int sellOrderID, int amount);
}


```

---

## InstantCraft by rostov114 - Allows players to instantly craft items with features

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using Oxide.Core;
using UnityEngine;
using System.Linq;
using System;

Oxide.Plugins
[Info("Instant Craft", "Vlad-0003 / Orange / rostov114", "2.2.8")]
[Description("Allows players to instantly craft items with features")]
public class InstantCraft : RustPlugin
{
    private const string permUse;
    private const string permNormal;
    private readonly object True;
    private readonly object False;
    private void Init();
    private object CanCraft(ItemCrafter itemCrafter, ItemBlueprint blueprint, int amount, bool free);
    private object OnItemCraft(ItemCraftTask task, BasePlayer owner);
    [HookMethod(nameof(API_IsItemBlocked))]
    public bool API_IsItemBlocked(ItemDefinition itemDefinition);
    [HookMethod(nameof(API_GetIsItemBlockedCallback))]
    public Func<ItemDefinition, bool> API_GetIsItemBlockedCallback();
    public void CancelTask(ItemCraftTask task, BasePlayer owner, string reason, object[] args);
    public void GiveRefund(ItemCraftTask task, BasePlayer owner);
    public bool GiveItem(ItemCraftTask task, BasePlayer owner, List<int> stacks);
    public bool Give(ItemCraftTask task, BasePlayer owner, int amount, ulong skin);
    public int FreeSlots(BasePlayer player);
    public List<int> GetStacks(ItemDefinition item, int amount);
    public bool HasPlace(int slots, List<int> stacks);
    protected override void LoadDefaultMessages();
    public void Message(BasePlayer player, string messageKey, object[] args);
    public string GetMessage(string messageKey, string playerID, object[] args);
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Check for free place")]
        public bool checkPlace;
        [JsonProperty(PropertyName = "Split crafted stacks")]
        public bool split;
        [JsonProperty(PropertyName = "Normal Speed")]
        private string[] normal;
        [JsonProperty(PropertyName = "Blacklist")]
        private string[] blocked;
        private HashSet<int> _normalCraftItemIds;
        private HashSet<int> _blockedItemIds;
        [JsonIgnore]
        public bool HasAnyBlockedItems { get; set; }
        public void Init(InstantCraft plugin);
        public bool IsNormal(ItemDefinition itemDefinition);
        public bool IsBlocked(ItemDefinition itemDefinition);
        private void ValidateItemShortNames(InstantCraft plugin, string[] itemShortNameList, HashSet<int> validItemIdList);
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class Configuration
{
    [JsonProperty(PropertyName = "Check for free place")]
    public bool checkPlace;
    [JsonProperty(PropertyName = "Split crafted stacks")]
    public bool split;
    [JsonProperty(PropertyName = "Normal Speed")]
    private string[] normal;
    [JsonProperty(PropertyName = "Blacklist")]
    private string[] blocked;
    private HashSet<int> _normalCraftItemIds;
    private HashSet<int> _blockedItemIds;
    [JsonIgnore]
    public bool HasAnyBlockedItems { get; set; }
    public void Init(InstantCraft plugin);
    public bool IsNormal(ItemDefinition itemDefinition);
    public bool IsBlocked(ItemDefinition itemDefinition);
    private void ValidateItemShortNames(InstantCraft plugin, string[] itemShortNameList, HashSet<int> validItemIdList);
}


```

---

## InstantExperiment by MJSU - Allows players to instantly experiment

```csharp
using System.ComponentModel;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Instant Experiment", "MJSU", "1.0.2")]
[Description("Allows players to instantly experiment")]
internal class InstantExperiment : RustPlugin
{
    private PluginConfig _pluginConfig;
    private const string UsePermission;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void OnExperimentStarted(Workbench workbench, BasePlayer player);
    private bool HasPermission(BasePlayer player, string perm);
    private class PluginConfig
    {
        [DefaultValue(0f)]
        [JsonProperty(PropertyName = "Experiment Time (Seconds)")]
        public float ExperimentTime { get; set; }
    }

}

private class PluginConfig
{
    [DefaultValue(0f)]
    [JsonProperty(PropertyName = "Experiment Time (Seconds)")]
    public float ExperimentTime { get; set; }
}


```

---

## InstantGather by supreme - Enhances the tools in order to gather instantly

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Instant Gather", "supreme", "1.0.9")]
[Description("Enhances the tools used for mining in order to gather instantly")]
public class InstantGather : RustPlugin
{
    private Configuration _pluginConfig;
    private const string UsePermission;
    private const string SalvagedAxeShortname;
    private const string ChainsawShortname;
    private const string JackhammerShortname;
    private void Init();
    private void OnMeleeAttack(BasePlayer player, HitInfo hitInfo);
    private void ChopTree(TreeEntity tree, BasePlayer player, HitInfo hitInfo, Item activeItem);
    private void MineOre(OreResourceEntity ore, BasePlayer player, HitInfo hitInfo, Item activeItem);
    private class Configuration
    {
        [JsonProperty(PropertyName = "Tools")]
        public List<string> Tools { get; set; }
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
}

private class Configuration
{
    [JsonProperty(PropertyName = "Tools")]
    public List<string> Tools { get; set; }
}


```

---

## InstantH2O by Lincoln - Instantly place small and large water catchers completely filled

```csharp
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;

Oxide.Plugins
[Info("Instant H2O", "Lincoln", "1.1.2")]
[Description("Instantly fills water catchers upon placement")]
public class InstantH2O : RustPlugin
{
    private HashSet<ulong> playerList;
    private HashSet<ulong> persistentPlayerList;
    private Dictionary<WaterCatcher, Timer> timerDict;
    private const string permUse;
    private const string dataFileName;
    private class PluginData
    {
        public HashSet<ulong> PersistentPlayerList { get; set; }
    }

    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private bool HasPermission(BasePlayer player);
    [ChatCommand("h2o")]
    private void H2OCMD(BasePlayer player, string command, string[] args);
    private void UpdateWaterCatchers(ulong userId);
    private void StartWaterCatcherTimer(WaterCatcher entity);
    private void StopWaterCatcherTimer(WaterCatcher entity);
    private void OnEntitySpawned(WaterCatcher entity);
    private void Unload();
    private void LoadData();
    private void SaveData();
    private void RestoreWaterCatchers();
}

private class PluginData
{
    public HashSet<ulong> PersistentPlayerList { get; set; }
}


```

---

## InstantHeliLoot by August - Makes Heli (and Bradley) gibs lootable/harvestable immediately after being destroyed

```csharp
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Instant Heli Loot", "August", "1.0.3")]
[Description("Makes Heli (and Bradley) Gibs lootable/harvestable immediately after being destroyed.")]
 class InstantHeliLoot : CovalencePlugin
{
    protected override void LoadDefaultConfig();
    private void Init();
    private void OnServerShutdown();
    private void UnlockCrates(Vector3 pos);
    private void KillFire(Vector3 pos);
    private void OnEntitySpawned(HelicopterDebris debris);
    private void OnEntityDeath(BradleyAPC apc, HitInfo info);
    private void OnEntityDeath(BaseHelicopter heli, HitInfo info);
}


```

---

## InstantMixingTable by MJSU - Allows players to instantly mix. Speed multiplier can be configured with permissions

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Instant Mixing Table", "MJSU", "1.0.0")]
[Description("Allows players to instantly mix teas")]
internal class InstantMixingTable : RustPlugin
{
    private PluginConfig _pluginConfig;
    private const string PermBase;
    private const string UsePermission;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnMixingTableToggle(MixingTable table, BasePlayer player);
    private bool HasPermission(BasePlayer player, string perm);
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Player Duration Multiplier (0 = instant, 1 = normal speed)")]
        public Hash<string, float> PlayerDurationMultiplier { get; set; }
    }

}

private class PluginConfig
{
    [JsonProperty(PropertyName = "Player Duration Multiplier (0 = instant, 1 = normal speed)")]
    public Hash<string, float> PlayerDurationMultiplier { get; set; }
}


```

---

## InstantResearch by Tori1157 - Allows control over research speed

```csharp
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Instant Research", "Artasan/Tori1157", "2.0.3", ResourceId = 1318)]
[Description("Allows control over research speed.")]
public class InstantResearch : RustPlugin
{
    private bool Changed;
    private bool Override;
    private float researchSpeed;
    private const string instantPermission;
    private const string controlledPermission;
    private void Init();
    private void LoadVariables();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void OnItemResearch(ResearchTable table, Item targetItem, BasePlayer player);
     bool CanInstant(BasePlayer player);
     bool CanControlled(BasePlayer player);
    private object GetConfig(string menu, string datavalue, object defaultValue);
}


```

---

## InstantSmelt by  - Smelt resources as soon as they are mined

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;

Oxide.Plugins
[Info("Instant Smelt", "Orange", "2.0.5")]
[Description("Smelt resources as soon as they are mined")]
public class InstantSmelt : RustPlugin
{
    private const string permUse;
    private const string charcoalItemName;
    private const string woodItemName;
    private void Init();
    private void Unload();
    private object OnCollectiblePickup(Item item, BasePlayer player);
    private object OnDispenserGather(ResourceDispenser dispenser, BasePlayer player, Item item);
    private object OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private void cmdToggleChat(BasePlayer player);
    private object OnGather(BasePlayer player, Item item, bool bonus, bool pickup);
    private bool HasPermission(BasePlayer player);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Command")]
        public string command;
        [JsonProperty(PropertyName = "A. Blacklist")]
        public List<string> blackList;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private const string filename;
    private List<ulong> data;
    private void LoadData();
    private void SaveData();
    protected override void LoadDefaultMessages();
    private void Message(BasePlayer player, string messageKey, object[] args);
    private string GetMessage(string messageKey, string playerID, object[] args);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Command")]
    public string command;
    [JsonProperty(PropertyName = "A. Blacklist")]
    public List<string> blackList;
}


```

---

## InstantStartup by kaysharp - Instant engine startup for Minicopter and Scrap Transport Helicopter

```csharp

Oxide.Plugins
[Info("Instant Startup", "Kaysharp/patched by chrome", "1.0.3")]
[Description("Instant engine Startup for Minicopter and Scrap Transport Helicopter")]
public class InstantStartup : RustPlugin
{
    private string permissionuse;
    private void Init();
    private void OnEngineStarted(BaseMountable entity, BasePlayer player);
}


```

---

## InstantUntie by MJSU - Instantly untie underwater boxes

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using Newtonsoft.Json;
using UnityEngine;

[Info("Instant Untie", "MJSU", "1.0.13")]
[Description("Instantly untie underwater boxes")]
internal class InstantUntie : RustPlugin
{
    private PluginConfig _pluginConfig;
    private const string UsePermission;
    private static InstantUntie _ins;
    private const string AccentColor;
    private void Init();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void Unload();
    private void OnUserPermissionGranted(string playerId, string permName);
    private void OnUserPermissionRevoked(string playerId, string permName);
    private void OnUserGroupAdded(string playerId, string groupName);
    private void OnUserGroupRemoved(string playerId, string groupName);
    private void OnGroupPermissionGranted(string groupName, string permName);
    private void OnGroupPermissionRevoked(string groupName, string permName);
    private void HandleUserChanges(string id);
    private void HandleUserChanges(BasePlayer player);
    private T Raycast(Ray ray, float distance);
    private void AddBehavior(BasePlayer player);
    private void DestroyBehavior(BasePlayer player);
    private void Chat(BasePlayer player, string format);
    private bool HasPermission(BasePlayer player, string perm);
    private string Lang(string key, BasePlayer player);
    private string Lang(string key, BasePlayer player, object[] args);
    private class UnderwaterBehavior : FacepunchBehaviour
    {
        private BasePlayer _player;
        private FreeableLootContainer _box;
        private float _nextRaycastTime;
        private void Awake();
        private void UpdateUnderwater();
        private void FixedUpdate();
        private void Untie();
        public void DoDestroy();
    }

    private class LangKeys
    {
        public const string Chat;
        public const string Untie;
        public const string Canceled;
    }

    private class PluginConfig
    {
        [DefaultValue(0f)]
        [JsonProperty(PropertyName = "Untie Duration (Seconds)")]
        public float UntieDuration { get; set; }
        [DefaultValue(5f)]
        [JsonProperty(PropertyName = "How often to check if player is underwater (Seconds)")]
        public float UnderWaterUpdateRate { get; set; }
        [DefaultValue(1f)]
        [JsonProperty(PropertyName = "How often to check if a player is holding the use button (Seconds)")]
        public float HeldKeyUpdateRate { get; set; }
        [DefaultValue(true)]
        [JsonProperty(PropertyName = "Show Untie Message")]
        public bool ShowUntieMessage { get; set; }
        [DefaultValue(true)]
        [JsonProperty(PropertyName = "Show canceled message")]
        public bool ShowCanceledMessage { get; set; }
        [DefaultValue(1)]
        [JsonProperty(PropertyName = "Buoyancy Scale")]
        public float BuoyancyScale { get; set; }
        [DefaultValue(false)]
        [JsonProperty(PropertyName = "Set box owner as untie player")]
        public bool SetOwnerId { get; set; }
    }

}

private class UnderwaterBehavior : FacepunchBehaviour
{
    private BasePlayer _player;
    private FreeableLootContainer _box;
    private float _nextRaycastTime;
    private void Awake();
    private void UpdateUnderwater();
    private void FixedUpdate();
    private void Untie();
    public void DoDestroy();
}

private class LangKeys
{
    public const string Chat;
    public const string Untie;
    public const string Canceled;
}

private class PluginConfig
{
    [DefaultValue(0f)]
    [JsonProperty(PropertyName = "Untie Duration (Seconds)")]
    public float UntieDuration { get; set; }
    [DefaultValue(5f)]
    [JsonProperty(PropertyName = "How often to check if player is underwater (Seconds)")]
    public float UnderWaterUpdateRate { get; set; }
    [DefaultValue(1f)]
    [JsonProperty(PropertyName = "How often to check if a player is holding the use button (Seconds)")]
    public float HeldKeyUpdateRate { get; set; }
    [DefaultValue(true)]
    [JsonProperty(PropertyName = "Show Untie Message")]
    public bool ShowUntieMessage { get; set; }
    [DefaultValue(true)]
    [JsonProperty(PropertyName = "Show canceled message")]
    public bool ShowCanceledMessage { get; set; }
    [DefaultValue(1)]
    [JsonProperty(PropertyName = "Buoyancy Scale")]
    public float BuoyancyScale { get; set; }
    [DefaultValue(false)]
    [JsonProperty(PropertyName = "Set box owner as untie player")]
    public bool SetOwnerId { get; set; }
}


```

---

## InvalidDistanceDetector by H1TT4 - Bans suspicious players based on weapon distances

```csharp
using Newtonsoft.Json;

Oxide.Plugins
[Info("Invalid Distance Detector", "HiTTA", "1.3.0")]
[Description("Bans suspicious players based on weapon distances")]
 class InvalidDistanceDetector : CovalencePlugin
{
    private Configuration _config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private class Configuration
    {
        [JsonProperty("Compound Bow Distance")]
        public float CompoundBow;
        [JsonProperty("Bow Distance")]
        public float Bow;
        [JsonProperty("Crossbow Distance")]
        public float Crossbow;
        [JsonProperty("M249 Distance")]
        public float M249;
        [JsonProperty("Eoka Pistol Distance")]
        public float EokaPistol;
        [JsonProperty("M92 Pistol Distance")]
        public float M92Pistol;
        [JsonProperty("Nailgun Distance")]
        public float Nailgun;
        [JsonProperty("Python Revolver Distance")]
        public float PythonRevolver;
        [JsonProperty("Revolver Distance")]
        public float Revolver;
        [JsonProperty("Semi-Automatic Pistol Distance")]
        public float SemiAutoPistol;
        [JsonProperty("Assault Rifle Distance")]
        public float AssaultRifle;
        [JsonProperty("Bolt Action Rifle Distance")]
        public float BoltActionRifle;
        [JsonProperty("L96 Distance")]
        public float L96Rifle;
        [JsonProperty("LR-300 Assault Rifle distance")]
        public float LR300AssaultRifle;
        [JsonProperty("M39 Rifle distance")]
        public float M39Rifle;
        [JsonProperty("Semi-Automatic Rifle distance")]
        public float SemiAutomaticRifle;
        [JsonProperty("MP5A4 distance")]
        public float MP5A4;
        [JsonProperty("Thompson distance")]
        public float Thompson;
    }

    private void OnEntityTakeDamage(BasePlayer victim, HitInfo hitInfo);
}

private class Configuration
{
    [JsonProperty("Compound Bow Distance")]
    public float CompoundBow;
    [JsonProperty("Bow Distance")]
    public float Bow;
    [JsonProperty("Crossbow Distance")]
    public float Crossbow;
    [JsonProperty("M249 Distance")]
    public float M249;
    [JsonProperty("Eoka Pistol Distance")]
    public float EokaPistol;
    [JsonProperty("M92 Pistol Distance")]
    public float M92Pistol;
    [JsonProperty("Nailgun Distance")]
    public float Nailgun;
    [JsonProperty("Python Revolver Distance")]
    public float PythonRevolver;
    [JsonProperty("Revolver Distance")]
    public float Revolver;
    [JsonProperty("Semi-Automatic Pistol Distance")]
    public float SemiAutoPistol;
    [JsonProperty("Assault Rifle Distance")]
    public float AssaultRifle;
    [JsonProperty("Bolt Action Rifle Distance")]
    public float BoltActionRifle;
    [JsonProperty("L96 Distance")]
    public float L96Rifle;
    [JsonProperty("LR-300 Assault Rifle distance")]
    public float LR300AssaultRifle;
    [JsonProperty("M39 Rifle distance")]
    public float M39Rifle;
    [JsonProperty("Semi-Automatic Rifle distance")]
    public float SemiAutomaticRifle;
    [JsonProperty("MP5A4 distance")]
    public float MP5A4;
    [JsonProperty("Thompson distance")]
    public float Thompson;
}


```

---

## InventoryBackup by MONaH - Allows to save and restore players inventories

```csharp
using System;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Inventory Backup", "MON@H", "1.0.6")]
[Description("Allows to save and restore players inventories​")]
public class InventoryBackup : RustPlugin
{
    private const string PermissionUse;
    private static readonly Regex _regexStripTags;
    private void Init();
    private void OnNewSave(string filename);
    private ConfigData _configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Storage duration (days)")]
        public double StorageDuration;
        [JsonProperty(PropertyName = "Clear inventories data on wipe")]
        public bool ClearOnWipe;
        [JsonProperty(PropertyName = "Logging enabled")]
        public bool LoggingEnabled;
        [JsonProperty(PropertyName = "Chat steamID icon")]
        public ulong SteamIDIcon;
        [JsonProperty(PropertyName = "Commands list", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Commands;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private StoredData _storedData;
    private class StoredData
    {
        public readonly Hash<ulong, Hash<string, PlayerInventoryData>> Inventories;
    }

    public class PlayerInventoryData
    {
        public DateTime SaveDate;
        public List<ItemData> ItemsBelt;
        public List<ItemData> ItemsMain;
        public List<ItemData> ItemsWear;
    }

    public class ItemData
    {
        public bool IsBlueprint;
        public float Condition;
        public float Fuel;
        public float MaxCondition;
        public int Ammo;
        public int AmmoType;
        public int Amount;
        public int BlueprintTarget;
        public int DataInt;
        public int FlameFuel;
        public int ID;
        public int Position;
        public string Name;
        public string Text;
        public ulong Skin;
        public List<ItemData> Contents;
        public Item ToItem();
        public static ItemData FromItem(Item item);
    }

    public void LoadData();
    public void SaveData();
    public void ClearData();
    public string Lang(string key, string userIDString, object[] args);
    private static class LangKeys
    {
        public static class Error
        {
            private const string Base;
            public const string Failed;
            public const string NoPermission;
            public const string Syntax;
        }

        public static class Info
        {
            private const string Base;
            public const string Success;
        }

        public static class Format
        {
            private const string Base;
            public const string Prefix;
        }

    }

    protected override void LoadDefaultMessages();
    [ConsoleCommand("invbackup")]
    private void ConsoleCmdInventoryBackup(ConsoleSystem.Arg arg);
    private void CmdInventoryBackup(BasePlayer player, string cmd, string[] args);
    private bool InventorySave(ulong userID, string inventoryName);
    private bool InventoryRestore(ulong userID, string inventoryName, bool remove);
    private bool InventoryRemove(ulong userID, string inventoryName);
    public void RegisterPermissions();
    public void AddCommands();
    public string StripRustTags(string text);
    public BasePlayer FindPlayer(ulong userID);
    public BasePlayer FindPlayer(string userIDString);
    public void Log(string text);
    public void PlayerSendMessage(BasePlayer player, string message);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Storage duration (days)")]
    public double StorageDuration;
    [JsonProperty(PropertyName = "Clear inventories data on wipe")]
    public bool ClearOnWipe;
    [JsonProperty(PropertyName = "Logging enabled")]
    public bool LoggingEnabled;
    [JsonProperty(PropertyName = "Chat steamID icon")]
    public ulong SteamIDIcon;
    [JsonProperty(PropertyName = "Commands list", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Commands;
}

private class StoredData
{
    public readonly Hash<ulong, Hash<string, PlayerInventoryData>> Inventories;
}

public class PlayerInventoryData
{
    public DateTime SaveDate;
    public List<ItemData> ItemsBelt;
    public List<ItemData> ItemsMain;
    public List<ItemData> ItemsWear;
}

public class ItemData
{
    public bool IsBlueprint;
    public float Condition;
    public float Fuel;
    public float MaxCondition;
    public int Ammo;
    public int AmmoType;
    public int Amount;
    public int BlueprintTarget;
    public int DataInt;
    public int FlameFuel;
    public int ID;
    public int Position;
    public string Name;
    public string Text;
    public ulong Skin;
    public List<ItemData> Contents;
    public Item ToItem();
    public static ItemData FromItem(Item item);
}

private static class LangKeys
{
    public static class Error
    {
        private const string Base;
        public const string Failed;
        public const string NoPermission;
        public const string Syntax;
    }

    public static class Info
    {
        private const string Base;
        public const string Success;
    }

    public static class Format
    {
        private const string Base;
        public const string Prefix;
    }

}

public static class Error
{
    private const string Base;
    public const string Failed;
    public const string NoPermission;
    public const string Syntax;
}

public static class Info
{
    private const string Base;
    public const string Success;
}

public static class Format
{
    private const string Base;
    public const string Prefix;
}


```

---

## InventoryCleaner by JoaoPster - Cleans your inventory on command or on exit

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using System;
using Rust;
using UnityEngine;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;
using System.ComponentModel;
using System.Runtime.InteropServices;
using System.Security;
using System.Reflection;
using Object = System.Object;
using System.Text;
using System.Numerics;
using System.Runtime;
using Network;

Oxide.Plugins
[Info("Inventory Cleaner", "Joao Pster", "2.1.1")]
[Description("Allows players to clear their own or another player's inventory.")]
public class InventoryCleaner : RustPlugin
{
    private class MyPermissions
    {
        public const string Clear;
        public const string ClearOthers;
        public const string ClearOnDeath;
        public const string ClearOnLogout;
    }

    private readonly string[] _permissions;
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "[Message Image]")]
        public ulong MessageImage { get; set; }
        [JsonProperty(PropertyName = "[Message Prefix]")]
        public string MessagePrefix { get; set; }
    }

    private Configuration DefaultConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
    private static List<T> GetAllPublicConstantValues(Type type);
    private string GenerateMessage(string message, string color, int size, bool italic);
    private bool HasPermission(BasePlayer player, string permissionName, bool send);
    private void SendConsoleMessage(BasePlayer player, string message);
    private void SendChatMessage(BasePlayer player, string message);
    private void ReplyPlayer(IPlayer player, string message);
    private void SendMessageToAll(string message);
    private bool[] hasAllPermissions(IPlayer iplayer);
    private string[] CheckPermissions(string[] permArray);
    private bool IsAllowed(BasePlayer player, string perm);
    private void LoadPermissions();
    private void Init();
     object OnPlayerDeath(BasePlayer player, HitInfo info);
     void OnPlayerDisconnected(BasePlayer player, string reason);
    private void CommandsPanel(IPlayer iplayer, string[] args);
    private void HelpPanel(IPlayer iplayer);
    private void DeleteFromEveryone(IPlayer iplayer, string opt);
    private void ClearOneContainer(IPlayer iplayer, ItemContainer container, string msgKey, string option, bool every);
    private void ClearAllContainers(IPlayer iplayer, string msgKey, string option, bool every);
    private void ClearCovalenceCommand(IPlayer iplayer, string command, string[] args);
    private static class MessageKey
    {
        public const string NoPermission;
        public const string NotFound;
        public const string OptNotFound;
        public const string CorrectUse;
        public const string BeltCleaned;
        public const string EveryBeltCleaned;
        public const string InvCleaned;
        public const string EveryInvCleaned;
        public const string WearCleaned;
        public const string EveryWearCleaned;
        public const string AllCleaned;
        public const string EveryAllCleaned;
        public const string OnDeath;
        public const string Header;
        public const string Gone;
        public const string Opts;
        public const string Perms;
        public const string OptAll;
        public const string OptInv;
        public const string OptBelt;
        public const string OptWear;
        public const string OptEvery;
        public const string PermUse;
        public const string PermEvery;
        public const string PermDeath;
        public const string PermLogout;
        public const string InvComands;
    }

    protected override void LoadDefaultMessages();
    private string GetMessage(string messageKey, string playerId, object[] args);
}

private class MyPermissions
{
    public const string Clear;
    public const string ClearOthers;
    public const string ClearOnDeath;
    public const string ClearOnLogout;
}

private class Configuration
{
    [JsonProperty(PropertyName = "[Message Image]")]
    public ulong MessageImage { get; set; }
    [JsonProperty(PropertyName = "[Message Prefix]")]
    public string MessagePrefix { get; set; }
}

private static class MessageKey
{
    public const string NoPermission;
    public const string NotFound;
    public const string OptNotFound;
    public const string CorrectUse;
    public const string BeltCleaned;
    public const string EveryBeltCleaned;
    public const string InvCleaned;
    public const string EveryInvCleaned;
    public const string WearCleaned;
    public const string EveryWearCleaned;
    public const string AllCleaned;
    public const string EveryAllCleaned;
    public const string OnDeath;
    public const string Header;
    public const string Gone;
    public const string Opts;
    public const string Perms;
    public const string OptAll;
    public const string OptInv;
    public const string OptBelt;
    public const string OptWear;
    public const string OptEvery;
    public const string PermUse;
    public const string PermEvery;
    public const string PermDeath;
    public const string PermLogout;
    public const string InvComands;
}


```

---

## InventoryCleaner_wLgAO by misticos - Allows players with permission to clean inventories.

```csharp
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Inventory Cleaner", "Iv Misticos", "2.0.0")]
[Description("This plugin allows players with permission to clean inventories.")]
 class InventoryCleaner : CovalencePlugin
{
    private const string PermissionSelf;
    private const string PermissionTarget;
    private const string PermissionAll;
    private const string CommandName;
    protected override void LoadDefaultMessages();
    private void Init();
    private void CommandUse(IPlayer player, string command, string[] args);
    private string GetMsg(string key, string userId);
}


```

---

## InventoryGuardian by k1lly0u - Restore players inventory even after server wipe

```csharp
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("InventoryGuardian", "k1lly0u", "0.2.7"), Description("Restore players inventory even after server wipe")]
 class InventoryGuardian : RustPlugin
{
    private IGData igData;
    private DynamicConfigFile Inventory_Data;
    private Dictionary<ulong, PlayerInfo> cachedInventories;
    private bool isNewSave;
    private void Loaded();
    private void OnServerInitialized();
    private void OnNewSave(string filename);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void Unload();
    private void CheckProtocol();
    private void RestoreAll();
    private void SaveAll();
    private void RemoveAll();
    private BasePlayer FindPlayer(BasePlayer player, string arg);
    private void MSG(BasePlayer player, string message, string key, bool title);
    private bool SaveInventory(BasePlayer player);
    private List<SavedItem> GetPlayerItems(BasePlayer player);
    private SavedItem ProcessItem(Item item, string container);
    private bool RemoveInventory(BasePlayer player);
    private bool RestoreInventory(BasePlayer player);
    private void GiveItem(BasePlayer player, Item item, string container);
    private Item BuildItem(SavedItem itemData);
    private void RegisterPermisions();
    private bool IsAdmin(BasePlayer player);
    private bool IsUser(BasePlayer player);
    [ChatCommand("ig")]
    private void cmdInvGuard(BasePlayer player, string command, string[] args);
    [ConsoleCommand("ig")]
    private void ccmdInvGuard(ConsoleSystem.Arg arg);
    private class IGData
    {
        public bool IsActivated;
        public bool AutoRestore;
        public bool KeepCondition;
        public int AuthLevel;
        public Dictionary<ulong, PlayerInfo> Inventories;
    }

    private class PlayerInfo
    {
        public bool RestoreOnce;
        public List<SavedItem> Items;
    }

    private class SavedItem
    {
        public string container;
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
        public SavedItem[] contents;
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

    private void SaveData();
    private void SaveLoop();
    private void LoadData();
    private ConfigData configData;
    private class ConfigData
    {
        public string Messages_MainColor { get; set; }
        public string Messages_MsgColor { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void SaveConfig(ConfigData config);
}

private class IGData
{
    public bool IsActivated;
    public bool AutoRestore;
    public bool KeepCondition;
    public int AuthLevel;
    public Dictionary<ulong, PlayerInfo> Inventories;
}

private class PlayerInfo
{
    public bool RestoreOnce;
    public List<SavedItem> Items;
}

private class SavedItem
{
    public string container;
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
    public SavedItem[] contents;
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

private class ConfigData
{
    public string Messages_MainColor { get; set; }
    public string Messages_MsgColor { get; set; }
}


```

---

## InventoryLock by VisEntities - Locks players' inventories when joining the server or upon entering certain zones

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Inventory Lock", "VisEntities", "2.0.0")]
[Description("Locks player's inventory when joining the server or upon entering certain zones.")]
public class InventoryLock : RustPlugin
{
    [PluginReference]
    private readonly Plugin ZoneManager;
    private static InventoryLock _plugin;
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty("Version")]
        public string Version { get; set; }
        [JsonProperty("Lock On Connect")]
        public List<ContainerType> LockOnConnect { get; set; }
        [JsonProperty("Locked Zones")]
        public List<LockedZone> LockedZones { get; set; }
        public class LockedZone
        {
            [JsonProperty("Zone Id")]
            public string ZoneId { get; set; }
            [JsonProperty("Container Types")]
            public List<ContainerType> ContainerTypes { get; set; }
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfig();
    private Configuration GetDefaultConfig();
    private void Init();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnEnterZone(string zoneId, BasePlayer player);
    private void OnExitZone(string zoneId, BasePlayer player);
    public static void LockOrUnlockContainerSlots(BasePlayer player, ContainerType containerType, bool lockSlots);
    private static bool VerifyPluginBeingLoaded(Plugin plugin);
    private static class PermissionUtil
    {
        public const string ADMIN;
        public const string BYPASS;
        public static void RegisterPermissions();
        public static bool VerifyHasPermission(BasePlayer player, string permissionName);
    }

    [ConsoleCommand("inv.lock")]
    private void cmdLockInventory(ConsoleSystem.Arg conArgs);
    private class Lang
    {
        public const string InvalidArguments;
        public const string PlayerNotFound;
        public const string InvalidContainerType;
        public const string LockSuccess;
        public const string UnlockSuccess;
        public const string PlayerHasBypass;
        public const string AdminPermissionRequired;
    }

    protected override void LoadDefaultMessages();
    private void SendReplyToPlayer(BasePlayer player, string messageKey, object[] args);
}

private class Configuration
{
    [JsonProperty("Version")]
    public string Version { get; set; }
    [JsonProperty("Lock On Connect")]
    public List<ContainerType> LockOnConnect { get; set; }
    [JsonProperty("Locked Zones")]
    public List<LockedZone> LockedZones { get; set; }
    public class LockedZone
    {
        [JsonProperty("Zone Id")]
        public string ZoneId { get; set; }
        [JsonProperty("Container Types")]
        public List<ContainerType> ContainerTypes { get; set; }
    }

}

public class LockedZone
{
    [JsonProperty("Zone Id")]
    public string ZoneId { get; set; }
    [JsonProperty("Container Types")]
    public List<ContainerType> ContainerTypes { get; set; }
}

private static class PermissionUtil
{
    public const string ADMIN;
    public const string BYPASS;
    public static void RegisterPermissions();
    public static bool VerifyHasPermission(BasePlayer player, string permissionName);
}

private class Lang
{
    public const string InvalidArguments;
    public const string PlayerNotFound;
    public const string InvalidContainerType;
    public const string LockSuccess;
    public const string UnlockSuccess;
    public const string PlayerHasBypass;
    public const string AdminPermissionRequired;
}


```

---

## InventoryViewer by Whispers88 - Look into a player's inventory

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using ProtoBuf;
using Rust;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using UnityEngine.Networking;

Oxide.Plugins
[Info("Inventory Viewer", "Whispers88", "4.1.2")]
[Description("Allows players with permission assigned to view anyone's inventory")]
public class InventoryViewer : CovalencePlugin
{
    private static List<string> _registeredhooks;
    private const string permuse;
    private const string permunlock;
    private void OnServerInitialized();
    private void Unload();
    private Configuration config;
    public class Configuration
    {
        [JsonProperty("View inventory raycast distance")]
        public float raycastdist;
        [JsonProperty("View inventory timeout (seconds) set to 0 to disable")]
        public float timeout;
        [JsonProperty("Use console logging")]
        public bool consolelogging;
        [JsonProperty("Use discord logging")]
        public bool discordlogging;
        [JsonProperty("Webhook URL")]
        public string discordwebhook;
        [JsonProperty("Discord name")]
        public string discordname;
        [JsonProperty("Discord avatar URL")]
        public string discordavatarurl;
        [JsonProperty("View Backpack Button AnchorMin")]
        public string ImageAnchorMin;
        [JsonProperty("View Backpack Button AnchorMax")]
        public string ImageAnchorMax;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private void ViewInvCmd(IPlayer iplayer, string command, string[] args);
    private Dictionary<ulong, LootingData> _viewingtarget;
    private void ViewInventory(BasePlayer player, BasePlayer targetplayer);
    private void StartLooting(BasePlayer player, BasePlayer targetplayer, LootableCorpse corpse);
    private void OnLootEntityEnd(BasePlayer player, StorageContainer container);
    private void EndCorpseLooting(BasePlayer player, LootableCorpse corpse);
    private class LootingData
    {
        public LootableCorpse? corpse;
        public StorageContainer? backpack;
        public BasePlayer targetPlayer;
    }

    private Dictionary<LootableCorpse, List<Item>> _logtaken;
    private Dictionary<LootableCorpse, List<Item>> _loggiven;
    private object CanMoveItem(Item item, PlayerInventory playerInventory, ItemContainerId targetContainer, int targetSlot, int amount);
    private static string _coffinPrefab;
    private void ViewBackpack(BasePlayer player, Item targetItem);
    private IPlayer FindPlayer(string nameOrId);
    private bool HasPerm(string id, string perm);
    private string GetLang(string langKey, string playerId, object[] args);
    private void ChatMessage(IPlayer player, string langKey, object[] args);
    private void UnSubscribeFromHooks();
    private void SubscribeToHooks();
    public void _ViewInventory(BasePlayer basePlayer, BasePlayer targetPlayer);
    private void ViewInventoryCommand(BasePlayer player, string command, string[] args);
    private IEnumerator LogToDiscord(BasePlayer viewer, BasePlayer viewing, LootableCorpse corpse);
    private Message DiscordMessage(BasePlayer viewer, BasePlayer viewing, LootableCorpse corpse);
    public class Message
    {
        public string username { get; set; }
        public string avatar_url { get; set; }
        public List<Embeds> embeds { get; set; }
        public class Fields
        {
            public string name { get; set; }
            public string value { get; set; }
            public bool inline { get; set; }
            public Fields(string name, string value, bool inline);
        }

        public class Footer
        {
            public string text { get; set; }
            public Footer(string text);
        }

        public class Embeds
        {
            public string title { get; set; }
            public string description { get; set; }
            public List<Fields> fields { get; set; }
            public Footer footer { get; set; }
            public Embeds(string title, string description, List<Fields> fields, Footer footer);
        }

        public Message(string username, string avatar_url, List<Embeds> embeds);
    }

}

public class Configuration
{
    [JsonProperty("View inventory raycast distance")]
    public float raycastdist;
    [JsonProperty("View inventory timeout (seconds) set to 0 to disable")]
    public float timeout;
    [JsonProperty("Use console logging")]
    public bool consolelogging;
    [JsonProperty("Use discord logging")]
    public bool discordlogging;
    [JsonProperty("Webhook URL")]
    public string discordwebhook;
    [JsonProperty("Discord name")]
    public string discordname;
    [JsonProperty("Discord avatar URL")]
    public string discordavatarurl;
    [JsonProperty("View Backpack Button AnchorMin")]
    public string ImageAnchorMin;
    [JsonProperty("View Backpack Button AnchorMax")]
    public string ImageAnchorMax;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private class LootingData
{
    public LootableCorpse? corpse;
    public StorageContainer? backpack;
    public BasePlayer targetPlayer;
}

public class Message
{
    public string username { get; set; }
    public string avatar_url { get; set; }
    public List<Embeds> embeds { get; set; }
    public class Fields
    {
        public string name { get; set; }
        public string value { get; set; }
        public bool inline { get; set; }
        public Fields(string name, string value, bool inline);
    }

    public class Footer
    {
        public string text { get; set; }
        public Footer(string text);
    }

    public class Embeds
    {
        public string title { get; set; }
        public string description { get; set; }
        public List<Fields> fields { get; set; }
        public Footer footer { get; set; }
        public Embeds(string title, string description, List<Fields> fields, Footer footer);
    }

    public Message(string username, string avatar_url, List<Embeds> embeds);
}

public class Fields
{
    public string name { get; set; }
    public string value { get; set; }
    public bool inline { get; set; }
    public Fields(string name, string value, bool inline);
}

public class Footer
{
    public string text { get; set; }
    public Footer(string text);
}

public class Embeds
{
    public string title { get; set; }
    public string description { get; set; }
    public List<Fields> fields { get; set; }
    public Footer footer { get; set; }
    public Embeds(string title, string description, List<Fields> fields, Footer footer);
}


```

---

## InvFoundation by  - Invulnerable foundation

```csharp
using System;
using Rust;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("InvFoundation", "sami37", "1.2.6", ResourceId = 2096)]
[Description("Invulnerable foundation")]
public class InvFoundation : RustPlugin
{
    [PluginReference("BuildingOwners")]
     Plugin BuildingOwners;
    [PluginReference("EntityOwner")]
     Plugin EntityOwner;
    private Dictionary<string, object> damageList { get; set; }
    private Dictionary<string, object> damageGradeScaling { get; set; }
    private bool UseEntityOwner { get; set; }
    private bool UseBuildOwners { get; set; }
    private bool UseDamageScaling { get; set; }
    private bool ExcludeCave { get; set; }
    private bool allowdecay { get; set; }
    private static readonly int colisionentity;
    private readonly int cupboardMask;
    private readonly int groundLayer;
     T GetConfig(string name, T defaultValue);
    static Dictionary<string,object> defaultDamageScale();
    static Dictionary<string, object> defaultDamageTierScaling();
     void Loaded();
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     bool IsOwner(BasePlayer player, BaseEntity targetEntity);
    private bool CupboardPrivlidge(BasePlayer player, Vector3 position);
}


```

---

## InvisibleSleepers by collectvood - Makes sleepers go invisible

```csharp
using Network;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Invisible Sleepers", "collect_vood", "1.0.14")]
[Description("Makes all sleepers invisible")]
public class InvisibleSleepers : RustPlugin
{
    [PluginReference]
    private Plugin Friends;
    private Plugin Clans;
    const string permAllow;
    const string permBypass;
    private ConfigurationFile Configuration;
    private class ConfigurationFile
    {
        [JsonProperty(PropertyName = "Performance Mode")]
        public bool PerformanceMode;
        [JsonProperty(PropertyName = "Show clan members")]
        public bool ShowClan;
        [JsonProperty(PropertyName = "Show team members")]
        public bool ShowTeam;
        [JsonProperty(PropertyName = "Show friends")]
        public bool ShowFriends;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void Init();
    private void OnServerInitialized();
    private object CanNetworkTo(BasePlayer player, BasePlayer target);
    private object CanNetworkTo(HeldEntity entity, BasePlayer target);
    private object CanBeTargeted(BasePlayer player, MonoBehaviour monoBehaviour);
    private void HideSleeper(BasePlayer player);
    private void OnPlayerSleep(BasePlayer player);
    private void OnPlayerSleepEnded(BasePlayer player);
    private bool HasPerm(BasePlayer player);
    private bool CanBypass(BasePlayer player, BasePlayer target);
    private bool IsClanMember(BasePlayer player, BasePlayer target);
    private bool IsTeamMember(BasePlayer player, BasePlayer target);
    private bool IsFriend(BasePlayer player, BasePlayer target);
}

private class ConfigurationFile
{
    [JsonProperty(PropertyName = "Performance Mode")]
    public bool PerformanceMode;
    [JsonProperty(PropertyName = "Show clan members")]
    public bool ShowClan;
    [JsonProperty(PropertyName = "Show team members")]
    public bool ShowTeam;
    [JsonProperty(PropertyName = "Show friends")]
    public bool ShowFriends;
}


```

---

## InvulnerablePlayers by  - Allows players to be invisible to NPCs, APС, heli, and traps

```csharp
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Invulnerable Players", "Hamster", "1.0.4")]
[Description("Prevents aggression / targeting of entities to the player")]
public class InvulnerablePlayers : RustPlugin
{
    private const string PermUse;
    private List<BasePlayer> _data;
    private void Init();
    protected override void LoadDefaultMessages();
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private object CanHelicopterStrafeTarget(PatrolHelicopterAI entity, BasePlayer target);
    private object CanBeTargeted(BaseCombatEntity entity, MonoBehaviour behaviour);
    private object CanBradleyApcTarget(BradleyAPC apc, BaseEntity entity);
    private object CanHelicopterTarget(PatrolHelicopterAI heli, BasePlayer basePlayer);
    private object OnNpcPlayerTarget(NPCPlayerApex npc, BaseEntity entity);
    private object OnNpcTarget(BaseNpc npc, BaseEntity entity);
    private bool CanNpcAttack(BaseNpc npc, BaseEntity entity);
    private object OnTurretTarget(AutoTurret turret, BaseCombatEntity entity);
    private object OnTrapTrigger(BaseTrap trap, GameObject go);
    private object OnPlayerLand(BasePlayer basePlayer, float num);
    private static bool IsNpc(BasePlayer player);
    private object OnRunPlayerMetabolism(PlayerMetabolism metabolism);
    [ChatCommand("inv")]
    private void CmdCraftBoat(BasePlayer basePlayer, string command, string[] args);
    private string GetMsg(string key, string userId);
}


```

---

## IPBlacklist by Ankawi - Blacklist IPs to prevent them from joining your server

```csharp
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("IPBlacklist", "Ankawi", "1.0.1")]
[Description("Blacklist IP addresses from joining your server")]
 class IPBlacklist : CovalencePlugin
{
    private const string IPPerm;
    private static HashSet<PlayerData> LoadedPlayerData;
     class PlayerData
    {
        public string PlayerName;
        public string SteamID;
        public string IP;
    }

    private void LoadData();
    private void SaveData();
    [Command("banip")]
    private void BanipCommand(IPlayer player, string command, string[] args);
    [Command("unbanip")]
    private void UnbanipCommand(IPlayer player, string command, string[] args);
    private void Init();
    private object CanUserLogin(string name, string id, string ip);
    private void LoadDefaultMessages();
    private string GetMsg(string key, string id, object[] args);
}

 class PlayerData
{
    public string PlayerName;
    public string SteamID;
    public string IP;
}


```

---

## IPLogger by redBDGR - Logs all player IP addresses for easy comparison

```csharp
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("IP Logger", "redBDGR", "1.0.1")]
[Description("Logs all player IP addresses for easy comparison")]
 class IPLogger : CovalencePlugin
{
     bool Changed;
     DynamicConfigFile IPInfo;
     StoredData storedData;
     Dictionary<string, List<string>> PlayerInfo;
     class StoredData
    {
        public Dictionary<string, List<string>> IPLog;
    }

     void Init();
     void LoadData();
     void SaveData();
     void Unload();
     void OnServerSave();
     void OnUserConnected(IPlayer player);
    private List<string> RetrieveIPs(string userID);
     object GetConfig(string menu, string datavalue, object defaultValue);
}

 class StoredData
{
    public Dictionary<string, List<string>> IPLog;
}


```

---

## ItemCleaner by lorddy - Remove specified item from all containers and players

```csharp
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Item Cleaner", "Lorddy", "0.0.6")]
[Description("Remove specified item from all containers and players")]
public class ItemCleaner : RustPlugin
{
    private const string PERMISSION_USE;
    protected override void LoadDefaultMessages();
    private void Loaded();
    [ChatCommand("itemcleaner")]
    private void ItemCleanerCommand(BasePlayer player, string command, string[] args);
    private void RemoveItem(BasePlayer player, ItemDefinition itemDefinition, bool remove, IPlayer entityOwner, bool priv);
     void RemoveItem(StorageContainer sc, BasePlayer player, ItemDefinition itemDefinition, BuildingPrivlidge playerPriv, bool remove, IPlayer entityOwner, bool priv);
     void RemoveItem(BasePlayer bp, BasePlayer player, ItemDefinition itemDefinition, BuildingPrivlidge playerPriv, bool remove, IPlayer entityOwner, bool priv);
    private void RemoveItem(BasePlayer player, ItemDefinition itemDefinition, BasePlayer target, bool remove);
    private void RemoveItem(BasePlayer player, ItemDefinition itemDefinition, StorageContainer sc, bool remove);
}


```

---

## ItemCostCalculator by  - Automatically calculates item costs based on ingredients

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Item Cost Calculator", "Absolut/Arainrr", "2.0.15", ResourceId = 2109)]
internal class ItemCostCalculator : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin RustTranslationAPI;
    private readonly Dictionary<ItemDefinition, double> itemsCost;
    private void OnServerInitialized();
    private void CalculateItemsCost();
    private Dictionary<ItemDefinition, int> GetItemIngredients(ItemBlueprint itemBlueprint);
    private void CreateDataFile(FileType fileType, string language);
    private string GetImageUrl(ItemDefinition itemDefinition, ulong skin);
    private bool IsSupportedLanguage(string language);
    private string GetItemTranslationByShortName(string language, string itemShortName);
    private string GetItemDisplayName(string language, ItemDefinition itemDefinition);
    private double GetItemCost(string shortname);
    private double GetItemCost(int itemID);
    private double GetItemCost(ItemDefinition itemDefinition);
    private Dictionary<string, double> GetItemsCostByShortName();
    private Dictionary<int, double> GetItemsCostByID();
    private Dictionary<ItemDefinition, double> GetItemsCostByDefinition();
    [ConsoleCommand("costfile")]
    private void CmdCostFile(ConsoleSystem.Arg arg);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "GUIShop - Recovery rate (Sell / Buy)")]
        public float recoveryRate;
        [JsonProperty(PropertyName = "GUIShop - Keep decimal")]
        public int keepdecimal;
        [JsonProperty(PropertyName = "Gather rate offset")]
        public float gatherRateOffset;
        [JsonProperty(PropertyName = "Materials list")]
        public Dictionary<string, double> materials;
        [JsonProperty(PropertyName = "No materials list")]
        public Dictionary<string, double> noMaterials;
        [JsonProperty(PropertyName = "Rarity list", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> rarityList;
        [JsonProperty(PropertyName = "Workbench level multiplier")]
        public Dictionary<int, float> workbenchMultiplier;
        [JsonProperty(PropertyName = "Item ingredients override", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, Dictionary<string, int>> ingredientsOverride;
        [JsonProperty(PropertyName = "Item displayNames")]
        public Dictionary<string, string> displayNames;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class RewardData
    {
        public string shortname;
        public string customIcon;
        public int amount;
        public ulong skinId;
        public bool isBp;
        public Category category;
        public string displayName;
        public int cost;
        public int cooldown;
    }

    private class ShopData
    {
        [JsonProperty("Shop - Shop Categories")]
        public Dictionary<string, ShopCategory> ShopCategories;
        [JsonProperty("Shop - Shop List")]
        public Dictionary<string, ShopItem> ShopItems;
    }

    private class ShopItem
    {
        public string DisplayName;
        public bool CraftAsDisplayName;
        public string Shortname;
        public int ItemId;
        public bool MakeBlueprint;
        public bool AllowSellOfUsedItems;
        public float Condition;
        public bool EnableBuy;
        public bool EnableSell;
        public string Image;
        public double SellPrice;
        public double BuyPrice;
        public int BuyCooldown;
        public int SellCooldown;
        public int[] BuyQuantity;
        public int[] SellQuantity;
        public int BuyLimit;
        public int BuyLimitResetCoolDown;
        public bool SwapLimitToQuantityBuyLimit;
        public int SellLimit;
        public int SellLimitResetCoolDown;
        public bool SwapLimitToQuantitySoldLimit;
        public string KitName;
        public List<string> Command;
        public bool RunCommandAndCustomShopItem;
        public List<char> GeneTypes;
        public ulong SkinId;
    }

    private class ShopCategory
    {
        public string DisplayName;
        public string DisplayNameColor;
        public string Description;
        public string DescriptionColor;
        public string Permission;
        public string Currency;
        public bool CustomCurrencyAllowSellOfUsedItems;
        public string CustomCurrencyNames;
        public int CustomCurrencyIDs;
        public ulong CustomCurrencySkinIDs;
        public bool EnabledCategory;
        public bool EnableNPC;
        public string NPCId;
        public HashSet<string> NpcIds;
        public HashSet<string> Items;
    }

    private void SaveData(string name, T data);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "GUIShop - Recovery rate (Sell / Buy)")]
    public float recoveryRate;
    [JsonProperty(PropertyName = "GUIShop - Keep decimal")]
    public int keepdecimal;
    [JsonProperty(PropertyName = "Gather rate offset")]
    public float gatherRateOffset;
    [JsonProperty(PropertyName = "Materials list")]
    public Dictionary<string, double> materials;
    [JsonProperty(PropertyName = "No materials list")]
    public Dictionary<string, double> noMaterials;
    [JsonProperty(PropertyName = "Rarity list", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> rarityList;
    [JsonProperty(PropertyName = "Workbench level multiplier")]
    public Dictionary<int, float> workbenchMultiplier;
    [JsonProperty(PropertyName = "Item ingredients override", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, Dictionary<string, int>> ingredientsOverride;
    [JsonProperty(PropertyName = "Item displayNames")]
    public Dictionary<string, string> displayNames;
}

private class RewardData
{
    public string shortname;
    public string customIcon;
    public int amount;
    public ulong skinId;
    public bool isBp;
    public Category category;
    public string displayName;
    public int cost;
    public int cooldown;
}

private class ShopData
{
    [JsonProperty("Shop - Shop Categories")]
    public Dictionary<string, ShopCategory> ShopCategories;
    [JsonProperty("Shop - Shop List")]
    public Dictionary<string, ShopItem> ShopItems;
}

private class ShopItem
{
    public string DisplayName;
    public bool CraftAsDisplayName;
    public string Shortname;
    public int ItemId;
    public bool MakeBlueprint;
    public bool AllowSellOfUsedItems;
    public float Condition;
    public bool EnableBuy;
    public bool EnableSell;
    public string Image;
    public double SellPrice;
    public double BuyPrice;
    public int BuyCooldown;
    public int SellCooldown;
    public int[] BuyQuantity;
    public int[] SellQuantity;
    public int BuyLimit;
    public int BuyLimitResetCoolDown;
    public bool SwapLimitToQuantityBuyLimit;
    public int SellLimit;
    public int SellLimitResetCoolDown;
    public bool SwapLimitToQuantitySoldLimit;
    public string KitName;
    public List<string> Command;
    public bool RunCommandAndCustomShopItem;
    public List<char> GeneTypes;
    public ulong SkinId;
}

private class ShopCategory
{
    public string DisplayName;
    public string DisplayNameColor;
    public string Description;
    public string DescriptionColor;
    public string Permission;
    public string Currency;
    public bool CustomCurrencyAllowSellOfUsedItems;
    public string CustomCurrencyNames;
    public int CustomCurrencyIDs;
    public ulong CustomCurrencySkinIDs;
    public bool EnabledCategory;
    public bool EnableNPC;
    public string NPCId;
    public HashSet<string> NpcIds;
    public HashSet<string> Items;
}


```

---

## ItemFinder by Camoec - Gets the count of specific items on the server

```csharp
using Oxide.Core.Libraries.Covalence;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core;
using System;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Item Finder", "Camoec", 1.3)]
[Description("Get count of specific item in the server")]
public class ItemFinder : CovalencePlugin
{
    private const string Perm;
    private DateTime lastCommand;
    private PluginConfig _config;
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Cooldown (is seconds)")]
        public int Cooldown;
    }

    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void Init();
    private class ItemsInfo
    {
        public string shortname;
        public int itemId;
        public int droppedCount;
        public int dropped;
        public int inPlayers;
        public int inCointaners;
        public int totalCount { get; set; }
    }

    [Command("itemfinder")]
    private void GetActiveEnts(IPlayer player, string command, string[] args);
    private int? GetItemId(string shortname);
    private ItemsInfo GetInfo(string shortname);
    protected override void LoadDefaultMessages();
    private string Lang(string key, string userid);
}

private class PluginConfig
{
    [JsonProperty(PropertyName = "Cooldown (is seconds)")]
    public int Cooldown;
}

private class ItemsInfo
{
    public string shortname;
    public int itemId;
    public int droppedCount;
    public int dropped;
    public int inPlayers;
    public int inCointaners;
    public int totalCount { get; set; }
}


```

---

## ItemHistory by birthdates - Keep track of which player an item came from

```csharp
using System;
using Newtonsoft.Json;
using System.Collections.Generic;

Oxide.Plugins
[Info("Item History", "birthdates", "1.2.1")]
[Description("Keep history of an item")]
public class ItemHistory : RustPlugin
{
    private const string Permission;
    private void Init();
    private void OnItemAddedToContainer(ItemContainer container, Item item);
    private ConfigFile _config;
    public class ConfigFile
    {
        [JsonProperty("Blacklisted Items (Won't get any history)")]
        public List<string> Blacklist { get; set; }
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

public class ConfigFile
{
    [JsonProperty("Blacklisted Items (Won't get any history)")]
    public List<string> Blacklist { get; set; }
    public static ConfigFile DefaultConfig();
}


```

---

## ItemInspector by mr01sam - Allows players to inspect items to see their true stats, info and modifiers.

```csharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System.Linq;
using System.Text;
using Rust;

Oxide.Plugins
[Info("Item Inspector", "mr01sam", "1.0.0")]
[Description("Allows players to inspect items to see their true stats")]
public class ItemInspector : CovalencePlugin
{
    [PluginReference]
    private readonly Plugin Craftsman;
    private const string PermissionUse;
    private const string PermissionAdmin;
     void OnServerInitialized(bool initial);
    private Dictionary<string, object> GetItemData(Item item);
    private class ItemData
    {
        public readonly Dictionary<string, object> metadata;
        public readonly Dictionary<string, object> info;
        public ItemData();
        public void AddInfo(string key, object value);
        public void AddMetadata(string key, object value);
    }

    private bool IsClassOf(object entity, Type type);
    private string Size(int size, string text);
    private string Color(string color, string text);
    private string Header(string titlestr, string color);
    private string Label(string labelstr, string color);
    private string Text(string statstr, string color);
    private string FormatList(string[] elements);
    private string FormatDict(string[] keys, string[] values);
    private string FormatString(string str);
    private string FormatRatio(string left, string right);
    private string LabelText(string label, string text, string color);
    private void PrintItemStats(IPlayer player, Dictionary<string, object> data);
    [Command("inspect")]
    private void cmd_inspect(IPlayer player, string command, string[] args);
    private Configuration config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Colors")]
        public ColorsConfig Colors;
        public class ColorsConfig
        {
            [JsonProperty(PropertyName = "Title")]
            public string Title { get; set; }
            [JsonProperty(PropertyName = "Labels")]
            public string Labels { get; set; }
            [JsonProperty(PropertyName = "Info")]
            public string Info { get; set; }
            [JsonProperty(PropertyName = "Metadata")]
            public string Metadata { get; set; }
            [JsonProperty(PropertyName = "Lists")]
            public string Lists { get; set; }
            [JsonProperty(PropertyName = "Ratios")]
            public string Ratios { get; set; }
            [JsonProperty(PropertyName = "Mappings")]
            public string Mappings { get; set; }
        }

    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private string Lang(string key, string id, object[] args);
    private Dictionary<string, float> WeaponDamages;
    private class ClothingResistances
    {
        [JsonProperty(PropertyName = "Projectile")]
        public float Projectile;
        [JsonProperty(PropertyName = "Melee")]
        public float Melee;
        [JsonProperty(PropertyName = "Bite")]
        public float Bite;
        [JsonProperty(PropertyName = "Radiation")]
        public float Radiation;
        [JsonProperty(PropertyName = "Explosion")]
        public float Explosion;
        [JsonProperty(PropertyName = "Cold")]
        public float Cold;
    }

    private Dictionary<string, ClothingResistances> ClothingProtection;
}

private class ItemData
{
    public readonly Dictionary<string, object> metadata;
    public readonly Dictionary<string, object> info;
    public ItemData();
    public void AddInfo(string key, object value);
    public void AddMetadata(string key, object value);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Colors")]
    public ColorsConfig Colors;
    public class ColorsConfig
    {
        [JsonProperty(PropertyName = "Title")]
        public string Title { get; set; }
        [JsonProperty(PropertyName = "Labels")]
        public string Labels { get; set; }
        [JsonProperty(PropertyName = "Info")]
        public string Info { get; set; }
        [JsonProperty(PropertyName = "Metadata")]
        public string Metadata { get; set; }
        [JsonProperty(PropertyName = "Lists")]
        public string Lists { get; set; }
        [JsonProperty(PropertyName = "Ratios")]
        public string Ratios { get; set; }
        [JsonProperty(PropertyName = "Mappings")]
        public string Mappings { get; set; }
    }

}

public class ColorsConfig
{
    [JsonProperty(PropertyName = "Title")]
    public string Title { get; set; }
    [JsonProperty(PropertyName = "Labels")]
    public string Labels { get; set; }
    [JsonProperty(PropertyName = "Info")]
    public string Info { get; set; }
    [JsonProperty(PropertyName = "Metadata")]
    public string Metadata { get; set; }
    [JsonProperty(PropertyName = "Lists")]
    public string Lists { get; set; }
    [JsonProperty(PropertyName = "Ratios")]
    public string Ratios { get; set; }
    [JsonProperty(PropertyName = "Mappings")]
    public string Mappings { get; set; }
}

private class ClothingResistances
{
    [JsonProperty(PropertyName = "Projectile")]
    public float Projectile;
    [JsonProperty(PropertyName = "Melee")]
    public float Melee;
    [JsonProperty(PropertyName = "Bite")]
    public float Bite;
    [JsonProperty(PropertyName = "Radiation")]
    public float Radiation;
    [JsonProperty(PropertyName = "Explosion")]
    public float Explosion;
    [JsonProperty(PropertyName = "Cold")]
    public float Cold;
}


```

---

## ItemPrinter by Razor - Craft configured items using a large box and some power

```csharp
using System;
using Oxide.Core.Plugins;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Oxide.Core.Configuration;
using Oxide.Core;
using System.Globalization;

Oxide.Plugins
[Info("Item Printer", "Ts3Hosting", "1.1.19")]
[Description("Craft set items in a large wooden box with power hookups and box decay")]
public class ItemPrinter : RustPlugin
{
     itemEntity npcData;
     playerEntity playerData;
     IEntity iData;
     boxEntity pcdData;
    private DynamicConfigFile PCDDATA;
    private DynamicConfigFile NPCDATA;
    private DynamicConfigFile PLAYERDATA;
    private DynamicConfigFile I;
    private const string adminAllow;
    private const string printerAllow;
    private const string useAllow;
    public ulong CardskinID;
    private int damage;
    private int itemamounts;
    private bool Changed;
    public int itemID;
    public ulong skinID;
    public string itemname;
    public string printername;
    public bool craftmode;
    public int seconds;
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
     void LoadVariables();
     object GetConfig(string menu, string datavalue, object defaultValue);
     void Init();
    private void OnServerInitialized();
    private void RegisterPermissions();
     void LoadData();
     void checkbox();
    public class vipData1
    {
        public int item;
        public ulong skinid;
        public int amount;
        public string name;
        public string displayName;
        public string Permission;
        public string totalGive;
    }

    public class vipData
    {
        public int item;
        public ulong skinid;
        public int amount;
        public string name;
        public string displayName;
        public string Permission;
        public string totalGive;
    }

     class itemEntity
    {
        public Dictionary<string, NPCInfo> iEntity;
        public itemEntity();
    }

     class NPCInfo
    {
        public int item;
        public ulong skinid;
        public int amount;
        public string name;
        public string displayName;
        public string Permission;
    }

     void SaveitemData();
     class boxEntity
    {
        public Dictionary<ulong, PCDInfo> pEntity;
        public boxEntity();
    }

     class PCDInfo
    {
        public uint batt;
        public uint printer;
        public uint light;
        public uint counter;
    }

     void SaveData();
     class playerEntity
    {
        public Dictionary<ulong, PInfo> playerpEntity;
        public playerEntity();
    }

     class PInfo
    {
        public string displayName;
        public string Permission;
    }

     void SavePlayerData();
     class IEntity
    {
        public Dictionary<string, IInfo> iEntity;
    }

     class IInfo
    {
        public int totalGive;
        public Dictionary<string, vipData> items;
    }

     void SaveIData();
     void OnPlayerConnected(BasePlayer player);
    [ConsoleCommand("printer")]
    private void CmdConsolePage(ConsoleSystem.Arg args);
    private void updategroup(BasePlayer player, string id, string perm, ConsoleSystem.Arg args);
     object CanPickupEntity(BasePlayer player, BaseCombatEntity entity);
     void OnItemAddedToContainer(ItemContainer container, Item item);
     object OnSwitchToggle(ElectricSwitch sw, BasePlayer player);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnEntityBuilt(Planner plan, GameObject go);
     void SpawnRefresh(BaseNetworkable entity1);
    private void additem(BasePlayer player, BoxStorage box1);
    [ChatCommand("printer")]
     void GetPrinter(BasePlayer player, string command, string[] args);
     void GetPrint(BasePlayer player);
     void GetPrinte1r(BasePlayer player, string perm, string amount);
    private void GetPlayerItems(BasePlayer player, string perm);
    private void ProcessItem(Item item, string container, string perm);
    private void GetPlayerItemsvip(BasePlayer player, string perm, string amount);
    private void ProcessItemvip(Item item, string container, string perm, string amount);
    public bool giveitem(StorageContainer box, int number1, string item);
    public bool hasbp(BasePlayer player, string itemboxid);
    private void stopbox(BasePlayer player, ElectricSwitch sw, StorageContainer box, FlasherLight light, uint box1);
    readonly Dictionary<ulong, Timer> timers;
     void DoPrinterThings(BasePlayer player, ElectricSwitch sw, uint box1, uint light, uint counterID);
}

public class vipData1
{
    public int item;
    public ulong skinid;
    public int amount;
    public string name;
    public string displayName;
    public string Permission;
    public string totalGive;
}

public class vipData
{
    public int item;
    public ulong skinid;
    public int amount;
    public string name;
    public string displayName;
    public string Permission;
    public string totalGive;
}

 class itemEntity
{
    public Dictionary<string, NPCInfo> iEntity;
    public itemEntity();
}

 class NPCInfo
{
    public int item;
    public ulong skinid;
    public int amount;
    public string name;
    public string displayName;
    public string Permission;
}

 class boxEntity
{
    public Dictionary<ulong, PCDInfo> pEntity;
    public boxEntity();
}

 class PCDInfo
{
    public uint batt;
    public uint printer;
    public uint light;
    public uint counter;
}

 class playerEntity
{
    public Dictionary<ulong, PInfo> playerpEntity;
    public playerEntity();
}

 class PInfo
{
    public string displayName;
    public string Permission;
}

 class IEntity
{
    public Dictionary<string, IInfo> iEntity;
}

 class IInfo
{
    public int totalGive;
    public Dictionary<string, vipData> items;
}


```

---

## ItemPuller by collectvood - Gives players the ability to pull items from containers

```csharp
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Item Puller", "collect_vood", "1.2.4")]
[Description("Gives you the ability to pull items from containers")]
 class ItemPuller : CovalencePlugin
{
    [PluginReference]
    private Plugin Friends;
    private Plugin Clans;
    const string permUse;
    const string permForcePull;
    const string permBuildPull;
     List<BasePlayer> uiEnabled;
    protected override void LoadDefaultMessages();
    private Configuration config;
    private class Configuration
    {
        [JsonProperty("Command")]
        public string Command;
        [JsonProperty("Pull on build")]
        public bool pullOnBuild;
        [JsonProperty("Check for Owner")]
        public bool checkForOwner;
        [JsonProperty("Check for Team")]
        public bool checkForTeam;
        [JsonProperty("Check for Friend")]
        public bool checkForFriends;
        [JsonProperty("Check for Clan")]
        public bool checkForClans;
        [JsonProperty(PropertyName = "Player Default Settings", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, bool> PlayerDefaultSettings;
        [JsonProperty(PropertyName = "Ui Enabled")]
        public bool useUi;
        [JsonProperty(PropertyName = "Global Ui Position")]
        public Vector2 UiPosition;
        [JsonProperty(PropertyName = "Custom Ui Positions Enabled")]
        public bool CustomUiPosition;
        [JsonProperty(PropertyName = "Custom Positions (Box wise)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<CustomOtherPositions> CustomOtherPositions;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private class CustomOtherPositions
    {
        [JsonProperty(PropertyName = "Box Shortname")]
        public string boxShortname;
        [JsonProperty(PropertyName = "Disable Ui")]
        public bool DisableUi;
        [JsonProperty(PropertyName = "Ui Position")]
        public Vector2 UiPosition { get; set; }
    }

    private StoredData storedData;
    private Dictionary<ulong, PlayerSettings> allPlayerSettings { get; set; }
    private class PlayerSettings
    {
        public bool enabled;
        public bool autocraft;
        public bool fromTC;
        public bool fp;
    }

    private class StoredData
    {
        public Dictionary<ulong, PlayerSettings> AllPlayerSettings { get; set; }
    }

    private void SaveData();
    private void OnServerSave();
    private void Unload();
    private void CreatePlayerSettings(BasePlayer player);
    private void Init();
    private void Loaded();
     object OnMessagePlayer(string message, BasePlayer player);
     object CanCraft(ItemCrafter itemCrafter, ItemBlueprint bp, int amount);
     object CanAffordToPlace(BasePlayer player, Planner planner, Construction construction);
     void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnPlayerConnected(BasePlayer player);
     void ItemPullerChatCommand(IPlayer player, string cmd, string[] args);
     string GetOptionFormatted(bool option);
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private void OnLootEntityEnd(BasePlayer player, BaseCombatEntity entity);
     string ToggleButtonColor(bool Enabled);
     string ToggleButtonTextColor(bool Enabled);
    private CuiElementContainer CreateUi(BasePlayer player, BaseEntity entity);
    private void DestroyUi(BasePlayer player);
    private void UpdateUi(BasePlayer player, BaseEntity entity);
     class Results
    {
        public bool hasResources;
        public Dictionary<Item, int> transferItems;
        public int check;
    }

     Results ScanItems(BasePlayer player, List<ItemAmount> ingredients, int amount);
     object GiveItems(BasePlayer player, Results results, List<ItemAmount> ingredients);
    private void ForceUsableItem(ItemCrafter itemCrafter, int itemid, int required);
     Dictionary<Item, int> GetUsableItems(BasePlayer player, int itemid, int required);
    private bool IsTeamMember(BasePlayer player, BasePlayer possibleMember);
    private bool IsFull(BasePlayer player);
    private bool HasIngredient(ItemCrafter itemCrafter, ItemAmount itemAmount, int amount);
     object CheckPermissions(BasePlayer player, bool OnBuild);
    private bool IsEnabled(BasePlayer player);
    private bool IsAutocraft(BasePlayer player);
    private bool IsFromTC(BasePlayer player);
    private bool IsForcePulling(BasePlayer player);
    private void ChangeEnabled(BasePlayer player, string setting);
    private bool HasPerm(BasePlayer player, string perm);
    private bool IsInBuildingZone(BasePlayer player);
    private bool CanForcePull(BasePlayer player);
}

private class Configuration
{
    [JsonProperty("Command")]
    public string Command;
    [JsonProperty("Pull on build")]
    public bool pullOnBuild;
    [JsonProperty("Check for Owner")]
    public bool checkForOwner;
    [JsonProperty("Check for Team")]
    public bool checkForTeam;
    [JsonProperty("Check for Friend")]
    public bool checkForFriends;
    [JsonProperty("Check for Clan")]
    public bool checkForClans;
    [JsonProperty(PropertyName = "Player Default Settings", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, bool> PlayerDefaultSettings;
    [JsonProperty(PropertyName = "Ui Enabled")]
    public bool useUi;
    [JsonProperty(PropertyName = "Global Ui Position")]
    public Vector2 UiPosition;
    [JsonProperty(PropertyName = "Custom Ui Positions Enabled")]
    public bool CustomUiPosition;
    [JsonProperty(PropertyName = "Custom Positions (Box wise)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<CustomOtherPositions> CustomOtherPositions;
}

private class CustomOtherPositions
{
    [JsonProperty(PropertyName = "Box Shortname")]
    public string boxShortname;
    [JsonProperty(PropertyName = "Disable Ui")]
    public bool DisableUi;
    [JsonProperty(PropertyName = "Ui Position")]
    public Vector2 UiPosition { get; set; }
}

private class PlayerSettings
{
    public bool enabled;
    public bool autocraft;
    public bool fromTC;
    public bool fp;
}

private class StoredData
{
    public Dictionary<ulong, PlayerSettings> AllPlayerSettings { get; set; }
}

 class Results
{
    public bool hasResources;
    public Dictionary<Item, int> transferItems;
    public int check;
}


```

---

## ItemRenamer by birthdates - Rename an item with style

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Item Renamer", "birthdates", "1.1.1")]
[Description("Rename items with style")]
public class ItemRenamer : RustPlugin
{
    public const string Permission;
    private void Init();
    private void ItemRenameCommand(BasePlayer player, string arg2, string[] args);
    private ConfigFile _config;
    public class ConfigFile
    {
        [JsonProperty(PropertyName = "Max characters in a rename including color")]
        public int chars;
        [JsonProperty(PropertyName = "Blacklisted words")]
        public List<string> bw;
        [JsonProperty(PropertyName = "Blacklisted items")]
        public List<string> items;
        [JsonProperty(PropertyName = "Ability to use color")]
        public bool color;
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadDefaultMessages();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

public class ConfigFile
{
    [JsonProperty(PropertyName = "Max characters in a rename including color")]
    public int chars;
    [JsonProperty(PropertyName = "Blacklisted words")]
    public List<string> bw;
    [JsonProperty(PropertyName = "Blacklisted items")]
    public List<string> items;
    [JsonProperty(PropertyName = "Ability to use color")]
    public bool color;
    public static ConfigFile DefaultConfig();
}


```

---

## ItemRepair by birthdates - Repair your active item to full health

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Item Repair", "birthdates", "1.1.0")]
[Description("Repair your active item to full health")]
public class ItemRepair : RustPlugin
{
    private const string UsePermission;
    private void Init();
    [ChatCommand("repair")]
    private void ChatCommand(BasePlayer player);
    protected override void LoadDefaultMessages();
    private ConfigFile _config;
    public class ConfigFile
    {
        [JsonProperty("Blacklisted Items (shortnames)")]
        public IList<string> Blacklist;
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

public class ConfigFile
{
    [JsonProperty("Blacklisted Items (shortnames)")]
    public IList<string> Blacklist;
    public static ConfigFile DefaultConfig();
}


```

---

## ItemRetriever by WhiteThunder - Allows players to build, craft, reload and more using items from external sources

```csharp
using HarmonyLib;
using Oxide.Core;
using Oxide.Core.Plugins;
using Rust;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using System.Reflection.Emit;

Oxide.Plugins
[Info("Item Retriever", "WhiteThunder", "0.7.4")]
[Description("Allows players to build, craft, reload and more using items from external containers.")]
internal class ItemRetriever : CovalencePlugin
{
    [PluginReference]
    private readonly Plugin InstantCraft;
    private static ItemRetriever _instance;
    private const int InventorySize;
    private const Item.Flag SearchableItemFlag;
    private const Item.Flag UnsearchableItemFlag;
    private const ItemDefinition.Flag SearchableItemDefinitionFlag;
    private const ItemDefinition.Flag CustomItemDefinitionFlag;
    private static readonly object True;
    private static readonly object False;
    private readonly CustomItemDefinitionTracker _customItemDefinitionTracker;
    private readonly SupplierManager _supplierManager;
    private readonly ContainerManager _containerManager;
    private readonly ApiInstance _api;
    private readonly Dictionary<int, int> _overridenIngredients;
    private readonly List<Item> _reusableItemList;
    private Func<ItemDefinition, bool> _isBlockedByInstantCraft;
    public ItemRetriever();
    [AutoPatch]
    [HarmonyPatch(typeof(PlayerInventory), "FindItemByItemID", typeof(int))]
    private static class Patch_PlayerInventory_FindItemByItemID
    {
        private static readonly CodeInstruction OnInventoryItemFindInstruction;
        private static readonly MethodInfo _replacementMethod;
        private static readonly CodeInstruction[] _newInstructions;
        private static Item ReplacementMethod(PlayerInventory playerInventory, int itemId);
        private static IEnumerable<CodeInstruction> Transpiler(IEnumerable<CodeInstruction> instructions);
    }

    [AutoPatch]
    [HarmonyPatch(typeof(PlayerInventory), "FindAmmo", typeof(AmmoTypes))]
    private static class Patch_PlayerInventory_FindAmmo
    {
        private static readonly CodeInstruction OnInventoryAmmoItemFindInstruction;
        private static readonly MethodInfo _replacementMethod;
        private static readonly CodeInstruction[] _newInstructions;
        private static Item ReplacementMethod(PlayerInventory playerInventory, AmmoTypes ammoTypes);
        private static IEnumerable<CodeInstruction> Transpiler(IEnumerable<CodeInstruction> instructions);
    }

    private static class HarmonyUtils
    {
        public static bool InstructionsMatch(CodeInstruction a, CodeInstruction b);
    }

    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnPluginLoaded(Plugin plugin);
    private void OnPluginUnloaded(Plugin plugin);
    private void OnEntitySaved(BasePlayer player, BaseNetworkable.SaveInfo saveInfo);
    private void OnInventoryNetworkUpdate(PlayerInventory inventory, ItemContainer container, ProtoBuf.UpdateItemContainer updatedItemContainer, PlayerInventory.Type inventoryType);
    private object OnInventoryItemsCount(PlayerInventory inventory, int itemId);
    private object OnInventoryItemsTake(PlayerInventory inventory, List<Item> collect, int itemId, int amount);
    private object OnInventoryItemsFind(PlayerInventory inventory, int itemId);
    private object OnInventoryItemFind(PlayerInventory inventory, int itemId);
    private object OnInventoryAmmoFind(PlayerInventory inventory, List<Item> collect, AmmoTypes ammoType);
    private Item OnInventoryAmmoItemFind(PlayerInventory inventory, ItemDefinition itemDefinition);
    private Item OnInventoryAmmoItemFind(PlayerInventory inventory, AmmoTypes ammoType);
    private object OnIngredientsCollect(ItemCrafter itemCrafter, ItemBlueprint blueprint, ItemCraftTask task, int amount, BasePlayer player);
    private object CanCraft(ItemCrafter itemCrafter, ItemBlueprint blueprint, int amount, bool free);
    private class ApiInstance
    {
        public readonly Dictionary<string, object> ApiWrapper;
        private readonly ItemRetriever _plugin;
        private SupplierManager _supplierManager { get; set; }
        private ContainerManager _containerManager { get; set; }
        public ApiInstance(ItemRetriever plugin);
        public void AddSupplier(Plugin plugin, Dictionary<string, object> spec);
        public void RemoveSupplier(Plugin plugin);
        public bool HasContainer(BasePlayer player, ItemContainer container);
        public void AddContainer(Plugin plugin, BasePlayer player, IItemContainerEntity containerEntity, ItemContainer container, Func<Plugin, BasePlayer, ItemContainer, bool> canUseContainer);
        public void RemoveContainer(Plugin plugin, BasePlayer player, ItemContainer container);
        public void RemoveAllContainersForPlayer(Plugin plugin, BasePlayer player);
        public void RemoveAllContainersForPlugin(Plugin plugin);
        public void FindPlayerItems(BasePlayer player, Dictionary<string, object> itemQueryDict, List<Item> collect);
        public int SumPlayerItems(BasePlayer player, Dictionary<string, object> itemQueryDict);
        public int TakePlayerItems(BasePlayer player, Dictionary<string, object> itemQueryDict, int amount, List<Item> collect);
        public void FindPlayerAmmo(BasePlayer player, AmmoTypes ammoType, List<Item> collect);
    }

    [HookMethod(nameof(API_GetApi))]
    public Dictionary<string, object> API_GetApi();
    [HookMethod(nameof(API_AddSupplier))]
    public void API_AddSupplier(Plugin plugin, Dictionary<string, object> spec);
    [HookMethod(nameof(API_RemoveSupplier))]
    public void API_RemoveSupplier(Plugin plugin);
    [HookMethod(nameof(API_HasContainer))]
    public object API_HasContainer(BasePlayer player, ItemContainer container);
    [HookMethod(nameof(API_AddContainer))]
    public void API_AddContainer(Plugin plugin, BasePlayer player, IItemContainerEntity containerEntity, ItemContainer container, Func<Plugin, BasePlayer, ItemContainer, bool> canUseContainer);
    [HookMethod(nameof(API_RemoveContainer))]
    public void API_RemoveContainer(Plugin plugin, BasePlayer player, ItemContainer container);
    [HookMethod(nameof(API_RemoveAllContainersForPlayer))]
    public void API_RemoveAllContainersForPlayer(Plugin plugin, BasePlayer player);
    [HookMethod(nameof(API_RemoveAllContainersForPlugin))]
    public void API_RemoveAllContainersForPlugin(Plugin plugin);
    [HookMethod(nameof(API_FindPlayerItems))]
    public void API_FindPlayerItems(BasePlayer player, Dictionary<string, object> itemQuery, List<Item> collect);
    [HookMethod(nameof(API_SumPlayerItems))]
    public object API_SumPlayerItems(BasePlayer player, Dictionary<string, object> itemQuery);
    [HookMethod(nameof(API_TakePlayerItems))]
    public object API_TakePlayerItems(BasePlayer player, Dictionary<string, object> itemQuery, int amount, List<Item> collect);
    [HookMethod(nameof(API_FindPlayerAmmo))]
    public void API_FindPlayerAmmo(BasePlayer player, AmmoTypes ammoType, List<Item> collect);
    private static class ExposedHooks
    {
        public static void OnIngredientsDetermine(Dictionary<int, int> overridenIngredients, ItemBlueprint blueprint, int amount, BasePlayer player);
    }

    private static void MarkInventoryDirty(BasePlayer player);
    private static int GetHighestUsedSlot(ProtoBuf.ItemContainer containerData);
    private void RefreshInstantCraftCallback();
    private void SerializeForNetwork(BasePlayer player, ProtoBuf.ItemContainer containerData);
    private void FindPlayerItems(BasePlayer player, T itemQuery, List<Item> collect, bool includePlayerWearables);
    private int SumPlayerItems(BasePlayer player, T itemQuery, bool countPlayerWearables);
    private int TakePlayerItems(BasePlayer player, T itemQuery, int amountToTake, List<Item> collect, ItemCraftTask itemCraftTask, bool includePlayerWearables);
    private void FindPlayerAmmo(BasePlayer player, AmmoTypes ammoType, List<Item> collect);
    private static class StringUtils
    {
        public static bool EqualsIgnoreCase(string a, string b);
    }

    private static class ObjectCache
    {
        private static class StaticObjectCache
        {
            private static readonly Dictionary<T, object> _cacheByValue;
            public static object Get(T value);
            public static void Clear();
        }

        public static object Get(T value);
        public static object Get(bool value);
        public static void Clear();
    }

    private class CustomItemDefinitionTracker
    {
        private readonly HashSet<int> _customItemDefinitionIds;
        public bool IsCustomItemDefinition(ItemDefinition itemDefinition);
        public bool IsCustomItemDefinition(Item item);
        public bool IsCustomItemDefinition(int itemId);
    }

    private static class ItemUtils
    {
        public static int SerializeForNetwork(CustomItemDefinitionTracker customItemDefinitionTracker, List<Item> itemList, ProtoBuf.ItemContainer containerData, int nextInvisibleSlot, bool addChildContainersOnly);
        public static int SerializeForNetwork(CustomItemDefinitionTracker customItemDefinitionTracker, List<ProtoBuf.Item> itemList, ProtoBuf.ItemContainer containerData, int nextInvisibleSlot);
        public static void FindItems(List<Item> itemList, T itemQuery, List<Item> collect, bool childItemsOnly);
        public static int SumItems(List<Item> itemList, T itemQuery, bool childItemsOnly);
        public static int TakeItems(List<Item> itemList, T itemQuery, int totalAmountToTake, List<Item> collect, bool childItemsOnly);
        private static bool IsSearchableItemDefinition(ItemDefinition itemDefinition);
        private static bool IsSearchableItemDefinition(int itemId);
        private static bool HasSearchableContainer(Item item, List<Item> itemList);
        private static bool HasSearchableContainer(ProtoBuf.Item itemData, List<ProtoBuf.Item> itemList);
    }

    private class ItemSupplier
    {
        public static ItemSupplier FromSpec(Plugin plugin, Dictionary<string, object> spec);
        private static void GetOption(Dictionary<string, object> dict, string key, T result);
        public Plugin Plugin { get; set; }
        public int Priority;
        public Action<BasePlayer, Dictionary<string, object>, List<Item>> FindPlayerItems;
        public Func<BasePlayer, Dictionary<string, object>, int> SumPlayerItems;
        public Func<BasePlayer, Dictionary<string, object>, int, List<Item>, int> TakePlayerItems;
        public Func<BasePlayer, Dictionary<string, object>, int, List<Item>, ItemCraftTask, int> TakePlayerItemsV2;
        public Action<BasePlayer, AmmoTypes, List<Item>> FindPlayerAmmo;
        public Action<BasePlayer, List<ProtoBuf.Item>> SerializeForNetwork;
    }

    private class SupplierManager
    {
        private static void RemoveSupplier(List<ItemSupplier> supplierList, Plugin plugin);
        private static void SortSupplierList(List<ItemSupplier> supplierList);
        private CustomItemDefinitionTracker _customItemDefinitionTracker;
        private List<ItemSupplier> _allSuppliers;
        private List<ItemSupplier> _beforeInventorySuppliers;
        private List<ItemSupplier> _afterInventorySuppliers;
        private List<ProtoBuf.Item> _reusableItemListForNetwork;
        private Dictionary<string, object> _reusableItemQuery;
        public SupplierManager(CustomItemDefinitionTracker customItemDefinitionTracker);
        public void AddSupplier(Plugin plugin, Dictionary<string, object> spec);
        public void RemoveSupplier(Plugin plugin);
        public int SerializeForNetwork(BasePlayer player, ProtoBuf.ItemContainer containerData, int nextInvisibleSlot);
        public void FindPlayerItems(BasePlayer player, T itemQuery, List<Item> collect, bool beforeInventory);
        public int SumPlayerItems(BasePlayer player, T itemQuery);
        public int TakePlayerItems(BasePlayer player, T itemQuery, int amountToTake, List<Item> collect, ItemCraftTask itemCraftTask, bool beforeInventory);
        public void FindPlayerAmmo(BasePlayer player, AmmoTypes ammoType, List<Item> collect, bool beforeInventory);
    }

    private class EntityTracker : FacepunchBehaviour
    {
        public static EntityTracker AddToEntity(BaseEntity entity, ContainerManager containerManager);
        private BaseEntity _entity;
        private ContainerManager _containerManager;
        private List<ContainerEntry> _containerList;
        public void AddContainer(ContainerEntry containerEntry);
        public void RemoveContainer(ContainerEntry containerEntry);
        private void OnDestroy();
    }

    private class ContainerManager
    {
        private static bool HasContainer(List<ContainerEntry> containerList, ItemContainer container);
        private static bool RemoveEntries(List<ContainerEntry> containerList, Plugin plugin, BasePlayer player);
        private static bool RemoveEntry(List<ContainerEntry> containerList, Plugin plugin, BasePlayer player, ItemContainer container);
        private CustomItemDefinitionTracker _customItemDefinitionTracker;
        private Dictionary<ulong, List<ContainerEntry>> _playerContainerEntries;
        private Dictionary<BaseEntity, EntityTracker> _entityTrackers;
        public ContainerManager(CustomItemDefinitionTracker customItemDefinitionTracker);
        public void UnregisterEntity(BaseEntity entity);
        public bool HasContainer(BasePlayer player, ItemContainer container);
        public bool AddContainer(Plugin plugin, BasePlayer player, IItemContainerEntity containerEntity, ItemContainer container, Func<Plugin, BasePlayer, ItemContainer, bool> canUseContainer);
        public bool RemoveContainer(Plugin plugin, BasePlayer player, ItemContainer container);
        public bool RemoveContainer(ContainerEntry containerEntry);
        public bool RemoveContainers(Plugin plugin, BasePlayer player);
        public HashSet<BasePlayer> RemoveContainers(Plugin plugin);
        public void RemoveContainers();
        public List<ContainerEntry> GetContainerList(BasePlayer player);
        public int SerializeForNetwork(BasePlayer player, ProtoBuf.ItemContainer containerData, int nextInvisibleSlot);
        public void FindPlayerItems(BasePlayer player, T itemQuery, List<Item> collect);
        public int SumPlayerItems(BasePlayer player, T itemQuery);
        public int TakePlayerItems(BasePlayer player, T itemQuery, int amountToTake, List<Item> collect);
        public void FindPlayerAmmo(BasePlayer player, AmmoTypes ammoType, List<Item> collect);
    }

}

[AutoPatch]
[HarmonyPatch(typeof(PlayerInventory), "FindItemByItemID", typeof(int))]
private static class Patch_PlayerInventory_FindItemByItemID
{
    private static readonly CodeInstruction OnInventoryItemFindInstruction;
    private static readonly MethodInfo _replacementMethod;
    private static readonly CodeInstruction[] _newInstructions;
    private static Item ReplacementMethod(PlayerInventory playerInventory, int itemId);
    private static IEnumerable<CodeInstruction> Transpiler(IEnumerable<CodeInstruction> instructions);
}

[AutoPatch]
[HarmonyPatch(typeof(PlayerInventory), "FindAmmo", typeof(AmmoTypes))]
private static class Patch_PlayerInventory_FindAmmo
{
    private static readonly CodeInstruction OnInventoryAmmoItemFindInstruction;
    private static readonly MethodInfo _replacementMethod;
    private static readonly CodeInstruction[] _newInstructions;
    private static Item ReplacementMethod(PlayerInventory playerInventory, AmmoTypes ammoTypes);
    private static IEnumerable<CodeInstruction> Transpiler(IEnumerable<CodeInstruction> instructions);
}

private static class HarmonyUtils
{
    public static bool InstructionsMatch(CodeInstruction a, CodeInstruction b);
}

private class ApiInstance
{
    public readonly Dictionary<string, object> ApiWrapper;
    private readonly ItemRetriever _plugin;
    private SupplierManager _supplierManager { get; set; }
    private ContainerManager _containerManager { get; set; }
    public ApiInstance(ItemRetriever plugin);
    public void AddSupplier(Plugin plugin, Dictionary<string, object> spec);
    public void RemoveSupplier(Plugin plugin);
    public bool HasContainer(BasePlayer player, ItemContainer container);
    public void AddContainer(Plugin plugin, BasePlayer player, IItemContainerEntity containerEntity, ItemContainer container, Func<Plugin, BasePlayer, ItemContainer, bool> canUseContainer);
    public void RemoveContainer(Plugin plugin, BasePlayer player, ItemContainer container);
    public void RemoveAllContainersForPlayer(Plugin plugin, BasePlayer player);
    public void RemoveAllContainersForPlugin(Plugin plugin);
    public void FindPlayerItems(BasePlayer player, Dictionary<string, object> itemQueryDict, List<Item> collect);
    public int SumPlayerItems(BasePlayer player, Dictionary<string, object> itemQueryDict);
    public int TakePlayerItems(BasePlayer player, Dictionary<string, object> itemQueryDict, int amount, List<Item> collect);
    public void FindPlayerAmmo(BasePlayer player, AmmoTypes ammoType, List<Item> collect);
}

private static class ExposedHooks
{
    public static void OnIngredientsDetermine(Dictionary<int, int> overridenIngredients, ItemBlueprint blueprint, int amount, BasePlayer player);
}

private static class StringUtils
{
    public static bool EqualsIgnoreCase(string a, string b);
}

private static class ObjectCache
{
    private static class StaticObjectCache
    {
        private static readonly Dictionary<T, object> _cacheByValue;
        public static object Get(T value);
        public static void Clear();
    }

    public static object Get(T value);
    public static object Get(bool value);
    public static void Clear();
}

private static class StaticObjectCache
{
    private static readonly Dictionary<T, object> _cacheByValue;
    public static object Get(T value);
    public static void Clear();
}

private class CustomItemDefinitionTracker
{
    private readonly HashSet<int> _customItemDefinitionIds;
    public bool IsCustomItemDefinition(ItemDefinition itemDefinition);
    public bool IsCustomItemDefinition(Item item);
    public bool IsCustomItemDefinition(int itemId);
}

private static class ItemUtils
{
    public static int SerializeForNetwork(CustomItemDefinitionTracker customItemDefinitionTracker, List<Item> itemList, ProtoBuf.ItemContainer containerData, int nextInvisibleSlot, bool addChildContainersOnly);
    public static int SerializeForNetwork(CustomItemDefinitionTracker customItemDefinitionTracker, List<ProtoBuf.Item> itemList, ProtoBuf.ItemContainer containerData, int nextInvisibleSlot);
    public static void FindItems(List<Item> itemList, T itemQuery, List<Item> collect, bool childItemsOnly);
    public static int SumItems(List<Item> itemList, T itemQuery, bool childItemsOnly);
    public static int TakeItems(List<Item> itemList, T itemQuery, int totalAmountToTake, List<Item> collect, bool childItemsOnly);
    private static bool IsSearchableItemDefinition(ItemDefinition itemDefinition);
    private static bool IsSearchableItemDefinition(int itemId);
    private static bool HasSearchableContainer(Item item, List<Item> itemList);
    private static bool HasSearchableContainer(ProtoBuf.Item itemData, List<ProtoBuf.Item> itemList);
}

private class ItemSupplier
{
    public static ItemSupplier FromSpec(Plugin plugin, Dictionary<string, object> spec);
    private static void GetOption(Dictionary<string, object> dict, string key, T result);
    public Plugin Plugin { get; set; }
    public int Priority;
    public Action<BasePlayer, Dictionary<string, object>, List<Item>> FindPlayerItems;
    public Func<BasePlayer, Dictionary<string, object>, int> SumPlayerItems;
    public Func<BasePlayer, Dictionary<string, object>, int, List<Item>, int> TakePlayerItems;
    public Func<BasePlayer, Dictionary<string, object>, int, List<Item>, ItemCraftTask, int> TakePlayerItemsV2;
    public Action<BasePlayer, AmmoTypes, List<Item>> FindPlayerAmmo;
    public Action<BasePlayer, List<ProtoBuf.Item>> SerializeForNetwork;
}

private class SupplierManager
{
    private static void RemoveSupplier(List<ItemSupplier> supplierList, Plugin plugin);
    private static void SortSupplierList(List<ItemSupplier> supplierList);
    private CustomItemDefinitionTracker _customItemDefinitionTracker;
    private List<ItemSupplier> _allSuppliers;
    private List<ItemSupplier> _beforeInventorySuppliers;
    private List<ItemSupplier> _afterInventorySuppliers;
    private List<ProtoBuf.Item> _reusableItemListForNetwork;
    private Dictionary<string, object> _reusableItemQuery;
    public SupplierManager(CustomItemDefinitionTracker customItemDefinitionTracker);
    public void AddSupplier(Plugin plugin, Dictionary<string, object> spec);
    public void RemoveSupplier(Plugin plugin);
    public int SerializeForNetwork(BasePlayer player, ProtoBuf.ItemContainer containerData, int nextInvisibleSlot);
    public void FindPlayerItems(BasePlayer player, T itemQuery, List<Item> collect, bool beforeInventory);
    public int SumPlayerItems(BasePlayer player, T itemQuery);
    public int TakePlayerItems(BasePlayer player, T itemQuery, int amountToTake, List<Item> collect, ItemCraftTask itemCraftTask, bool beforeInventory);
    public void FindPlayerAmmo(BasePlayer player, AmmoTypes ammoType, List<Item> collect, bool beforeInventory);
}

private class EntityTracker : FacepunchBehaviour
{
    public static EntityTracker AddToEntity(BaseEntity entity, ContainerManager containerManager);
    private BaseEntity _entity;
    private ContainerManager _containerManager;
    private List<ContainerEntry> _containerList;
    public void AddContainer(ContainerEntry containerEntry);
    public void RemoveContainer(ContainerEntry containerEntry);
    private void OnDestroy();
}

private class ContainerManager
{
    private static bool HasContainer(List<ContainerEntry> containerList, ItemContainer container);
    private static bool RemoveEntries(List<ContainerEntry> containerList, Plugin plugin, BasePlayer player);
    private static bool RemoveEntry(List<ContainerEntry> containerList, Plugin plugin, BasePlayer player, ItemContainer container);
    private CustomItemDefinitionTracker _customItemDefinitionTracker;
    private Dictionary<ulong, List<ContainerEntry>> _playerContainerEntries;
    private Dictionary<BaseEntity, EntityTracker> _entityTrackers;
    public ContainerManager(CustomItemDefinitionTracker customItemDefinitionTracker);
    public void UnregisterEntity(BaseEntity entity);
    public bool HasContainer(BasePlayer player, ItemContainer container);
    public bool AddContainer(Plugin plugin, BasePlayer player, IItemContainerEntity containerEntity, ItemContainer container, Func<Plugin, BasePlayer, ItemContainer, bool> canUseContainer);
    public bool RemoveContainer(Plugin plugin, BasePlayer player, ItemContainer container);
    public bool RemoveContainer(ContainerEntry containerEntry);
    public bool RemoveContainers(Plugin plugin, BasePlayer player);
    public HashSet<BasePlayer> RemoveContainers(Plugin plugin);
    public void RemoveContainers();
    public List<ContainerEntry> GetContainerList(BasePlayer player);
    public int SerializeForNetwork(BasePlayer player, ProtoBuf.ItemContainer containerData, int nextInvisibleSlot);
    public void FindPlayerItems(BasePlayer player, T itemQuery, List<Item> collect);
    public int SumPlayerItems(BasePlayer player, T itemQuery);
    public int TakePlayerItems(BasePlayer player, T itemQuery, int amountToTake, List<Item> collect);
    public void FindPlayerAmmo(BasePlayer player, AmmoTypes ammoType, List<Item> collect);
}


```

---

## ItemsBlocker by  - Prevents some items from being used for a limited period of time

```csharp
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Globalization;
using UnityEngine;
using System.Linq;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using System.Collections;
using Oxide.Core;
using System.IO;

Oxide.Plugins
[Info("ItemsBlocker", "Vlad-00003", "3.1.2", ResourceId = 2407)]
[Description("Prevents some items from being used for a limited period of time.")]
 class ItemsBlocker : RustPlugin
{
    private Dictionary<string, string> Image;
    private PluginConfig config;
    [PluginReference]
     Plugin Duel;
    private Dictionary<BasePlayer, Timer> Main;
    private List<BasePlayer> OnScreen;
    private Timer OnScreenUpdater;
    private class GUIPanel
    {
        [JsonProperty("Minimum anchor")]
        public string Amin;
        [JsonProperty("Maximum anchor")]
        public string Amax;
        [JsonProperty("Color")]
        public string Color;
    }

    private class GUIText : GUIPanel
    {
        [JsonProperty("Size")]
        public int Size;
        [JsonProperty("Outline")]
        public GUIOutline Outline;
    }

    private class GUIImage
    {
        [JsonProperty("Minimum anchor")]
        public string Amin;
        [JsonProperty("Maximum anchor")]
        public string Amax;
        [JsonProperty("Link to the image or file in the data folder")]
        public string Image;
        [JsonProperty("Opacity of the image")]
        public float Opacity;
    }

    private class GUIOutline
    {
        [JsonProperty("Use Outline")]
        public bool Use;
        [JsonProperty("Outline color")]
        public string Color;
        [JsonProperty("Outline distance")]
        public string Distance;
    }

    private class GUISettings
    {
        [JsonProperty("Backgound for main panel")]
        public GUIPanel Background;
        [JsonProperty("Main panel settings (shows if player attemts to use blocked cloth/item)")]
        public GUIPanel MainPanel;
        [JsonProperty("Settings for the text on main panel")]
        public GUIText TextOnMain;
        [JsonProperty("Use On Screen Panel")]
        public bool UseOnScreenPanel;
        [JsonProperty("On Screen Panel (shown if the block is active)")]
        public GUIPanel OnScreenPanel;
        [JsonProperty("Text settings for On Screen Panel")]
        public GUIText TextOnScreen;
        [JsonProperty("Image (shown on On Screen Panel")]
        public GUIImage Image;
    }

    private class PluginConfig
    {
        [JsonProperty("Block end time")]
        public string BlockEndStr;
        [JsonProperty("Hour of block after wipe")]
        public int HoursOfBlock;
        [JsonProperty("Chat prefix")]
        public string Prefix;
        [JsonProperty("Chat prefix color")]
        public string PrefixColor;
        [JsonProperty("Use chat insted of GUI")]
        public bool UseChat;
        [JsonProperty("Bypass permission")]
        public string BypassPermission;
        [JsonProperty("GUI Settings")]
        public GUISettings Gui;
        [JsonProperty("List of blocked items")]
        public List<string> BlockedItems;
        [JsonProperty("List of blocked clothes")]
        public List<string> BlockedClothes;
        [JsonProperty("List of blocked ammunition")]
        public List<string> BlockedAmmo;
        [JsonIgnore]
        public DateTime BlockEnd;
        public static PluginConfig DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void LoadData();
    private void SaveData();
     void OnServerInitialized();
     void Unload();
    protected override void LoadDefaultMessages();
     string GetMsg(string key, BasePlayer player);
     string GetMsg(string key);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnNewSave(string filename);
     object CanEquipItem(PlayerInventory inventory, Item item);
     object CanWearItem(PlayerInventory inventory, Item item);
     void OnReloadWeapon(BasePlayer player, BaseProjectile projectile);
     object OnReloadMagazine(BasePlayer player, BaseProjectile projectile);
    [ConsoleCommand("ib.refresh")]
    private void CmdRefresh(ConsoleSystem.Arg arg);
    private void DownloadImage();
     IEnumerator DownloadImage(string url);
    private string BlockerParent;
    private string BlockerPanel;
    private string BlockerText;
    private string OnScreenParent;
    private string OnScreenText;
    private class UI
    {
        private static string ToRustColor(string input);
        public static void CreatePanel(CuiElementContainer container, string Parent, string Name, GUIPanel panel, bool CursorEnabled);
        public static void CreateImage(CuiElementContainer container, string Parent, string Name, GUIImage Image, string Png);
        public static void CreateText(CuiElementContainer container, string Parent, string Name, GUIText TextComp, string Text, TextAnchor Anchor);
        public static void CreatePanel(CuiElementContainer container, string Parent, string Name, string Color, string Amin, string Amax, bool CursorEnabled);
        public static void CreateImage(CuiElementContainer container, string Parent, string Name, float opacity, string Amin, string Amax, string Image);
        public static void CreateFulscreenButton(CuiElementContainer container, string Parent, string Name, string Command);
        public static void CreateText(CuiElementContainer container, string Parent, string Name, string Amin, string Amax, string Text, string TextColor, int FontSize, GUIOutline outline, TextAnchor Anchor);
    }

    [ConsoleCommand("ib.close")]
    private void CmdCloseUI(ConsoleSystem.Arg arg);
    private void DestroyAllGui();
    private void OnScreenPanel(bool init);
    private void OnScreenPanelMain(BasePlayer player);
    private void BlockerUi(BasePlayer player, string inputText);
    private void ShowBlocker(BasePlayer player);
    private void Debug(string text);
    private bool IsAmmoBlocked(BasePlayer owner, BaseProjectile proj);
    private bool IsNPC(BasePlayer player);
    private bool InDuel(BasePlayer player);
     TimeSpan TimeLeft { get; set; }
    private bool InBlock { get; set; }
    private void SendToChat(BasePlayer Player, string Message);
}

private class GUIPanel
{
    [JsonProperty("Minimum anchor")]
    public string Amin;
    [JsonProperty("Maximum anchor")]
    public string Amax;
    [JsonProperty("Color")]
    public string Color;
}

private class GUIText : GUIPanel
{
    [JsonProperty("Size")]
    public int Size;
    [JsonProperty("Outline")]
    public GUIOutline Outline;
}

private class GUIImage
{
    [JsonProperty("Minimum anchor")]
    public string Amin;
    [JsonProperty("Maximum anchor")]
    public string Amax;
    [JsonProperty("Link to the image or file in the data folder")]
    public string Image;
    [JsonProperty("Opacity of the image")]
    public float Opacity;
}

private class GUIOutline
{
    [JsonProperty("Use Outline")]
    public bool Use;
    [JsonProperty("Outline color")]
    public string Color;
    [JsonProperty("Outline distance")]
    public string Distance;
}

private class GUISettings
{
    [JsonProperty("Backgound for main panel")]
    public GUIPanel Background;
    [JsonProperty("Main panel settings (shows if player attemts to use blocked cloth/item)")]
    public GUIPanel MainPanel;
    [JsonProperty("Settings for the text on main panel")]
    public GUIText TextOnMain;
    [JsonProperty("Use On Screen Panel")]
    public bool UseOnScreenPanel;
    [JsonProperty("On Screen Panel (shown if the block is active)")]
    public GUIPanel OnScreenPanel;
    [JsonProperty("Text settings for On Screen Panel")]
    public GUIText TextOnScreen;
    [JsonProperty("Image (shown on On Screen Panel")]
    public GUIImage Image;
}

private class PluginConfig
{
    [JsonProperty("Block end time")]
    public string BlockEndStr;
    [JsonProperty("Hour of block after wipe")]
    public int HoursOfBlock;
    [JsonProperty("Chat prefix")]
    public string Prefix;
    [JsonProperty("Chat prefix color")]
    public string PrefixColor;
    [JsonProperty("Use chat insted of GUI")]
    public bool UseChat;
    [JsonProperty("Bypass permission")]
    public string BypassPermission;
    [JsonProperty("GUI Settings")]
    public GUISettings Gui;
    [JsonProperty("List of blocked items")]
    public List<string> BlockedItems;
    [JsonProperty("List of blocked clothes")]
    public List<string> BlockedClothes;
    [JsonProperty("List of blocked ammunition")]
    public List<string> BlockedAmmo;
    [JsonIgnore]
    public DateTime BlockEnd;
    public static PluginConfig DefaultConfig();
}

private class UI
{
    private static string ToRustColor(string input);
    public static void CreatePanel(CuiElementContainer container, string Parent, string Name, GUIPanel panel, bool CursorEnabled);
    public static void CreateImage(CuiElementContainer container, string Parent, string Name, GUIImage Image, string Png);
    public static void CreateText(CuiElementContainer container, string Parent, string Name, GUIText TextComp, string Text, TextAnchor Anchor);
    public static void CreatePanel(CuiElementContainer container, string Parent, string Name, string Color, string Amin, string Amax, bool CursorEnabled);
    public static void CreateImage(CuiElementContainer container, string Parent, string Name, float opacity, string Amin, string Amax, string Image);
    public static void CreateFulscreenButton(CuiElementContainer container, string Parent, string Name, string Command);
    public static void CreateText(CuiElementContainer container, string Parent, string Name, string Amin, string Amax, string Text, string TextColor, int FontSize, GUIOutline outline, TextAnchor Anchor);
}


```

---

## ItemSearch by  - Search items by proper name or shortname.

```csharp
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Item Search", "Jacob", "1.0.2", ResourceId = 2679)]
 class ItemSearch : RustPlugin
{
    [ChatCommand("item")]
    private void ItemSearchCommand(BasePlayer player, string command, string[] args);
    protected override void LoadDefaultMessages();
    private void Init();
}


```

---

## ItemsInfo by misticos - Gets actual information about all items

```csharp
using System;
using System.Collections.Generic;
using System.Text;
using UnityEngine;

Oxide.Plugins
[Info("Items Info", "Iv Misticos", "1.0.3")]
[Description("Get actual information about items.")]
 class ItemsInfo : RustPlugin
{
    protected override void LoadDefaultMessages();
    private void Init();
    private bool CommandConsoleHandle(ConsoleSystem.Arg arg);
    private string GetItemsInfo(IReadOnlyList<string> parameters, bool search, string id);
    private string GetMsg(string key, string userId);
}


```

---

## ItemSkinRandomizer by Mevent - Randomizes item skins on item creation/in hands/in house

```csharp
using System.Collections.Generic;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Rust.Workshop;
using UnityEngine;

Oxide.Plugins
[Info("Item Skin Randomizer", "Mevent", "1.6.3")]
[Description("Simple plugin that will select a random skin for an item when crafting")]
public class ItemSkinRandomizer : RustPlugin
{
    private const string permUse;
    private const string permUseEntities;
    private const string permReSkin;
    private Dictionary<string, List<ulong>> skins;
    private static ConfigData _config;
    private class ConfigData
    {
        [JsonProperty("Commands")]
        public string[] Commands;
        [JsonProperty("Blocked skin id's")]
        public ulong[] BlockedSkins;
        [JsonProperty("Blocked items")]
        public string[] BlockedItems;
        [JsonProperty("Set random skins for entities?")]
        public bool UseEntities;
        [JsonProperty("Blocked entities")]
        public string[] BlockedEntities;
    }

    protected override void LoadConfig();
    private void ValidateConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnItemCraftFinished(ItemCraftTask task, Item item, ItemCrafter crafter);
    private void OnEntitySpawned(BaseEntity entity);
    private void CmdControl(IPlayer cov, string command, string[] args);
    private void RegisterCommands();
    private void RegisterPermissions();
    private static string FixNames(string name);
    private void GenerateSkins();
    private void SetRandomSkin(BasePlayer player, Item item);
    private void SetRandomSkin(BasePlayer player, BaseEntity entity);
    private ulong GetRandomSkin(string key);
    private static T GetLookEntity(BasePlayer player);
    private const string CantBuild;
    private const string ChangedTo;
    private const string NoObject;
    private const string Permission;
    protected override void LoadDefaultMessages();
    private void Message(BasePlayer player, string messageKey, object[] args);
    private string GetMessage(string messageKey, string playerID, object[] args);
}

private class ConfigData
{
    [JsonProperty("Commands")]
    public string[] Commands;
    [JsonProperty("Blocked skin id's")]
    public ulong[] BlockedSkins;
    [JsonProperty("Blocked items")]
    public string[] BlockedItems;
    [JsonProperty("Set random skins for entities?")]
    public bool UseEntities;
    [JsonProperty("Blocked entities")]
    public string[] BlockedEntities;
}


```

---

## ItemSkinSetter by ThibmoRozier - Sets the default skin ID for newly crafted items.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Item Skin Setter", "ThibmoRozier", "1.1.6")]
[Description("Sets the default skin ID for newly crafted items.")]
public class ItemSkinSetter : RustPlugin
{
    private class ConfigData
    {
        [JsonProperty("Bindings")]
        public List<ShortnameToWorkshopId> Bindings;
    }

    private ConfigData FConfigData;
    private Dictionary<int, ulong> FItemSkinBindings;
    private string _(string aKey, string aPlayerId);
    private bool IsNumber(string aStr);
    private void PerformStartupConfigCheck();
    protected override void LoadDefaultMessages();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void OnServerInitialized();
     void OnItemCraftFinished(ItemCraftTask aTask, Item aItem);
    [ConsoleCommand("iss_get")]
    private void ItemSkinSetterGetCmd(ConsoleSystem.Arg aArg);
    [ConsoleCommand("iss_getskins")]
    private void ItemSkinSetterGetSkinsCmd(ConsoleSystem.Arg aArg);
}

private class ConfigData
{
    [JsonProperty("Bindings")]
    public List<ShortnameToWorkshopId> Bindings;
}


```

---

## ItemTranslations by Ryan - Provides a data file of item names for translation

```csharp
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[Info("Item Translations", "Ryan", "1.0.0")]
[Description("Provides a data file of item names for translation")]
 class ItemTranslations : RustPlugin
{
    private StoredData sData;
    private class StoredData
    {
        public Dictionary<string, string> ItemTranslations;
    }

    private void OnServerInitialized();
    private void Init();
}

private class StoredData
{
    public Dictionary<string, string> ItemTranslations;
}


```

---

## ItemVoid by  - Teleports items to a chest via a void

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Item Void", "Default", "1.0.3")]
[Description("Transport items to a chest via the void")]
 class ItemVoid : RustPlugin
{
    private static ItemVoid instance;
    private const string permissionName;
    private Dictionary<string, ItemContainer> voidList;
    private bool pluginready;
    [PluginReference]
    private Plugin Economics;
    private void Init();
     void OnServerInitialized();
     void Unload();
     void OnEntityKill(StorageContainer container);
    protected override void LoadDefaultMessages();
    private void CreateVoidTeleportCMD(BasePlayer player, string command, string[] args);
    private void LinkChestCMD(BasePlayer player, string command, string[] args);
    private BaseEntity CreateVoid(BasePlayer player);
    public class TeleportVoid : MonoBehaviour
    {
        private ItemContainer linkedContainer;
        public BaseEntity entity;
        private BoxCollider collider;
        public BasePlayer ownerPlayer;
        private float initTime;
        private float destroyTime;
        private string t;
        private void Awake();
        private void Update();
        private void OnTriggerEnter(Collider col);
        private void TransportThroughVoid(DroppedItem droppedItem);
        private void DoVanishEffect(Vector3 pos);
        private bool ContainerHasSpace(ItemContainer container);
    }

    public ConfigFile _config;
    public class ConfigFile
    {
        [JsonProperty("Time until the void despawns")]
        public float voidLifeLength;
        [JsonProperty("Skin to use for the void (Default skin is: 1277211259")]
        public ulong skin;
        [JsonProperty("Effect to use when an item is transferred to a linked container")]
        public string effectUsed;
        [JsonProperty("Command to create a void")]
        public string voidCommand;
        [JsonProperty("Command to link a chest")]
        public string linkCommand;
        [JsonProperty("Economics")]
        public ItemVoidEconomics eco;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public class ItemVoidEconomics
    {
        [JsonProperty("Enable economics support?")]
        public bool economicsSupport;
        [JsonProperty("Cost to link a container")]
        public double linkCost;
        [JsonProperty("Cost to create a void")]
        public double voidCost;
        [JsonProperty("Item ID and costs")]
        public Dictionary<string, double> itemTable;
    }

    private void Withdraw(string playerId, double amount);
    private bool EnoughMoney(string playerId, double requiredAmount);
    public static BasePlayer FindPlayer(ulong userId);
}

public class TeleportVoid : MonoBehaviour
{
    private ItemContainer linkedContainer;
    public BaseEntity entity;
    private BoxCollider collider;
    public BasePlayer ownerPlayer;
    private float initTime;
    private float destroyTime;
    private string t;
    private void Awake();
    private void Update();
    private void OnTriggerEnter(Collider col);
    private void TransportThroughVoid(DroppedItem droppedItem);
    private void DoVanishEffect(Vector3 pos);
    private bool ContainerHasSpace(ItemContainer container);
}

public class ConfigFile
{
    [JsonProperty("Time until the void despawns")]
    public float voidLifeLength;
    [JsonProperty("Skin to use for the void (Default skin is: 1277211259")]
    public ulong skin;
    [JsonProperty("Effect to use when an item is transferred to a linked container")]
    public string effectUsed;
    [JsonProperty("Command to create a void")]
    public string voidCommand;
    [JsonProperty("Command to link a chest")]
    public string linkCommand;
    [JsonProperty("Economics")]
    public ItemVoidEconomics eco;
}

public class ItemVoidEconomics
{
    [JsonProperty("Enable economics support?")]
    public bool economicsSupport;
    [JsonProperty("Cost to link a container")]
    public double linkCost;
    [JsonProperty("Cost to create a void")]
    public double voidCost;
    [JsonProperty("Item ID and costs")]
    public Dictionary<string, double> itemTable;
}


```

---

## Jail by k1lly0u - Create a jail system where all the naughty players go

```csharp
using Facepunch;
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("Jail", "Reneb / k1lly0u", "4.0.6")]
[Description("Create a Jail system where all the naughty players go")]
 class Jail : RustPlugin
{
    [PluginReference]
     Plugin ZoneManager;
     Plugin Spawns;
     Plugin Kits;
    private PrisonData prisonData;
    private PrisonerData prisonerData;
    private RestoreData restoreData;
    private DynamicConfigFile prisondata;
    private DynamicConfigFile prisonerdata;
    private DynamicConfigFile restoredata;
    private Dictionary<string, PrisonData.PrisonEntry> prisons;
    private Dictionary<ulong, PrisonerData.PrisonerEntry> prisoners;
    private static Jail ins;
    private LayerMask layerMask;
    const string UIJailTimer;
    private void Loaded();
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnPlayerRespawned(BasePlayer player);
    private object OnPlayerChat(BasePlayer player, string message, ConVar.Chat.ChatChannel channel);
    private object OnPlayerCommand(BasePlayer player, string command, string[] args);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private void OnServerSave();
    private void Unload();
    private void SendToPrison(BasePlayer player, string prisonName, double time);
    private int GetEmptyCell(string prisonName);
    private void CheckIn(BasePlayer player, string prisonName);
    private void FreeFromJail(BasePlayer player);
    private object GetSpawnLocation(string prisonName, int cellNumber);
    private Vector3 CalculateFreePosition(string prisonName);
    private void PushBack(BasePlayer player, string prisonName);
    private void MovePosition(BasePlayer player, Vector3 destination);
    private void StartSleeping(BasePlayer player);
    private class UI
    {
        static public CuiElementContainer Element(string panelName, string color, string aMin, string aMax);
        static public void Label(CuiElementContainer container, string panel, string text, int size, string aMin, string aMax, TextAnchor align);
    }

    private void ShowJailTimer(BasePlayer player);
    private string FormatTime(double time);
    private bool IsInZone(BasePlayer player, string zoneID);
    private void OnEnterZone(string zoneID, BasePlayer player);
    private void OnExitZone(string zoneID, BasePlayer player);
    public static void StripInventory(BasePlayer player);
    public class RestoreData
    {
        public Hash<ulong, PlayerData> restoreData;
        public void AddData(BasePlayer player);
        public void RemoveData(ulong playerId);
        public bool HasRestoreData(ulong playerId);
        public void RestorePlayer(BasePlayer player, bool returnHome);
        private void RestoreAllItems(BasePlayer player, PlayerData playerData);
        private bool RestoreItems(BasePlayer player, ItemData[] itemData, string type);
        private Item CreateItem(ItemData itemData);
        public class PlayerData
        {
            public float[] stats;
            public float[] position;
            public ItemData[] containerMain;
            public ItemData[] containerWear;
            public ItemData[] containerBelt;
            public PlayerData();
            public PlayerData(BasePlayer player);
            private IEnumerable<ItemData> GetItems(ItemContainer container);
            private float[] GetStats(BasePlayer player);
            public void SetStats(BasePlayer player);
            private float[] GetPosition(Vector3 position);
            public Vector3 GetPosition();
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
            public InstanceData instanceData;
            public ItemData[] contents;
            public class InstanceData
            {
                public int dataInt;
                public int blueprintTarget;
                public int blueprintAmount;
                public InstanceData();
                public InstanceData(Item item);
                public void Restore(Item item);
            }

        }

    }

    private double GrabCurrentTime();
    private bool HasPermission(ulong playerId, string perm);
    private bool IsPrisoner(BasePlayer player);
    private bool HasEmptyCells(string prisonName);
    [ChatCommand("leavejail")]
    private void cmdLeaveJail(BasePlayer player, string command, string[] args);
    [ChatCommand("jail")]
    private void cmdJail(BasePlayer player, string command, string[] args);
    [ConsoleCommand("jail")]
    private void ccmdJail(ConsoleSystem.Arg arg);
    private void FreePrisoners(string prisonName);
    public object ValidateSpawnFile(string name);
    public object ValidateZoneID(string name);
    public object ValidateKit(string name);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Return prisoners to the position they were incarcerated at when released")]
        public bool ReturnHomeAfterRelease { get; set; }
        [JsonProperty(PropertyName = "Block chat for inmates")]
        public bool BlockChat { get; set; }
        [JsonProperty(PropertyName = "Allow chat between inmates (when chat is blocked)")]
        public bool InmateChat { get; set; }
        [JsonProperty(PropertyName = "Allow players to escape jail")]
        public bool AllowBreakouts { get; set; }
        [JsonProperty(PropertyName = "Return prisoners belongings if they escape jail")]
        public bool ReturnGearOnBreakout { get; set; }
        [JsonProperty(PropertyName = "Automatically release prisoners when their sentence has expired")]
        public bool AutoReleaseWhenExpired { get; set; }
        [JsonProperty(PropertyName = "Give prisoners a designated kit")]
        public bool GiveInmateKits { get; set; }
        [JsonProperty(PropertyName = "Disable damage dealt by prisoners")]
        public bool DisablePrisonerDamage { get; set; }
        [JsonProperty(PropertyName = "Broadcast player imprisonment globally")]
        public bool BroadcastImprisonment { get; set; }
        [JsonProperty(PropertyName = "Restrict public access to prison zones")]
        public bool BlockPublicAccessToPrisons { get; set; }
        [JsonProperty(PropertyName = "Blacklisted commands for prisoners")]
        public string[] CommandBlacklist { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void SaveConfig(ConfigData config);
    private void SavePrisonData();
    private void SavePrisonerData();
    private void LoadData();
    private class PrisonData
    {
        public Dictionary<string, PrisonEntry> prisons;
        public class PrisonEntry
        {
            public string zoneId;
            public string spawnFile;
            public string inmateKit;
            public float x;
            public float y;
            public float z;
            public float radius;
            public Dictionary<int, bool> occupiedCells;
            public PrisonEntry();
            public PrisonEntry(string zoneId, string spawnFile, string inmateKit, Vector3 position, float radius, int spawnCount);
        }

    }

    private class PrisonerData
    {
        public Dictionary<ulong, PrisonerEntry> prisoners;
        public class PrisonerEntry
        {
            public string prisonName;
            public int cellNumber;
            public double releaseDate;
        }

    }

    private string msg(string key, ulong playerId);
    private Dictionary<string, string> Messages;
}

private class UI
{
    static public CuiElementContainer Element(string panelName, string color, string aMin, string aMax);
    static public void Label(CuiElementContainer container, string panel, string text, int size, string aMin, string aMax, TextAnchor align);
}

public class RestoreData
{
    public Hash<ulong, PlayerData> restoreData;
    public void AddData(BasePlayer player);
    public void RemoveData(ulong playerId);
    public bool HasRestoreData(ulong playerId);
    public void RestorePlayer(BasePlayer player, bool returnHome);
    private void RestoreAllItems(BasePlayer player, PlayerData playerData);
    private bool RestoreItems(BasePlayer player, ItemData[] itemData, string type);
    private Item CreateItem(ItemData itemData);
    public class PlayerData
    {
        public float[] stats;
        public float[] position;
        public ItemData[] containerMain;
        public ItemData[] containerWear;
        public ItemData[] containerBelt;
        public PlayerData();
        public PlayerData(BasePlayer player);
        private IEnumerable<ItemData> GetItems(ItemContainer container);
        private float[] GetStats(BasePlayer player);
        public void SetStats(BasePlayer player);
        private float[] GetPosition(Vector3 position);
        public Vector3 GetPosition();
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
        public InstanceData instanceData;
        public ItemData[] contents;
        public class InstanceData
        {
            public int dataInt;
            public int blueprintTarget;
            public int blueprintAmount;
            public InstanceData();
            public InstanceData(Item item);
            public void Restore(Item item);
        }

    }

}

public class PlayerData
{
    public float[] stats;
    public float[] position;
    public ItemData[] containerMain;
    public ItemData[] containerWear;
    public ItemData[] containerBelt;
    public PlayerData();
    public PlayerData(BasePlayer player);
    private IEnumerable<ItemData> GetItems(ItemContainer container);
    private float[] GetStats(BasePlayer player);
    public void SetStats(BasePlayer player);
    private float[] GetPosition(Vector3 position);
    public Vector3 GetPosition();
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
    public InstanceData instanceData;
    public ItemData[] contents;
    public class InstanceData
    {
        public int dataInt;
        public int blueprintTarget;
        public int blueprintAmount;
        public InstanceData();
        public InstanceData(Item item);
        public void Restore(Item item);
    }

}

public class InstanceData
{
    public int dataInt;
    public int blueprintTarget;
    public int blueprintAmount;
    public InstanceData();
    public InstanceData(Item item);
    public void Restore(Item item);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Return prisoners to the position they were incarcerated at when released")]
    public bool ReturnHomeAfterRelease { get; set; }
    [JsonProperty(PropertyName = "Block chat for inmates")]
    public bool BlockChat { get; set; }
    [JsonProperty(PropertyName = "Allow chat between inmates (when chat is blocked)")]
    public bool InmateChat { get; set; }
    [JsonProperty(PropertyName = "Allow players to escape jail")]
    public bool AllowBreakouts { get; set; }
    [JsonProperty(PropertyName = "Return prisoners belongings if they escape jail")]
    public bool ReturnGearOnBreakout { get; set; }
    [JsonProperty(PropertyName = "Automatically release prisoners when their sentence has expired")]
    public bool AutoReleaseWhenExpired { get; set; }
    [JsonProperty(PropertyName = "Give prisoners a designated kit")]
    public bool GiveInmateKits { get; set; }
    [JsonProperty(PropertyName = "Disable damage dealt by prisoners")]
    public bool DisablePrisonerDamage { get; set; }
    [JsonProperty(PropertyName = "Broadcast player imprisonment globally")]
    public bool BroadcastImprisonment { get; set; }
    [JsonProperty(PropertyName = "Restrict public access to prison zones")]
    public bool BlockPublicAccessToPrisons { get; set; }
    [JsonProperty(PropertyName = "Blacklisted commands for prisoners")]
    public string[] CommandBlacklist { get; set; }
}

private class PrisonData
{
    public Dictionary<string, PrisonEntry> prisons;
    public class PrisonEntry
    {
        public string zoneId;
        public string spawnFile;
        public string inmateKit;
        public float x;
        public float y;
        public float z;
        public float radius;
        public Dictionary<int, bool> occupiedCells;
        public PrisonEntry();
        public PrisonEntry(string zoneId, string spawnFile, string inmateKit, Vector3 position, float radius, int spawnCount);
    }

}

public class PrisonEntry
{
    public string zoneId;
    public string spawnFile;
    public string inmateKit;
    public float x;
    public float y;
    public float z;
    public float radius;
    public Dictionary<int, bool> occupiedCells;
    public PrisonEntry();
    public PrisonEntry(string zoneId, string spawnFile, string inmateKit, Vector3 position, float radius, int spawnCount);
}

private class PrisonerData
{
    public Dictionary<ulong, PrisonerEntry> prisoners;
    public class PrisonerEntry
    {
        public string prisonName;
        public int cellNumber;
        public double releaseDate;
    }

}

public class PrisonerEntry
{
    public string prisonName;
    public int cellNumber;
    public double releaseDate;
}


```

---

## JoinMessagePlus by MisterPixie - Advanced join and leave messages

```csharp
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;

Oxide.Plugins
[Info("Join Message Plus", "MisterPixie", "1.0.54")]
[Description("Advanced join/leave messages")]
 class JoinMessagePlus : CovalencePlugin
{
    private System.Random _rnd;
    private const string _permission;
    private List<string> _joinMessagePlusData;
    private void SaveData();
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
    private void Init();
    private void OnUserConnected(IPlayer player);
    private void OnUserDisconnected(IPlayer player);
    private void Unload();
    private void OnServerSave();
    private void VIPToggle(IPlayer player, string command, string[] arg);
    private ConfigData configData;
    private class ConfigData
    {
        public bool EnableJoinMsg;
        public bool EnableLeaveMsg;
        public bool IsAdminJoin;
        public bool IsAdminLeave;
        public bool UseVIPTogglePerm;
        public string VIPToggleCommand;
        public List<string> JoinMessages;
        public List<string> LeaveMessages;
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
}

private class ConfigData
{
    public bool EnableJoinMsg;
    public bool EnableLeaveMsg;
    public bool IsAdminJoin;
    public bool IsAdminLeave;
    public bool UseVIPTogglePerm;
    public string VIPToggleCommand;
    public List<string> JoinMessages;
    public List<string> LeaveMessages;
}


```

---

## JPipes by TheGreatJ - Pipes that automatically transfer items between boxes, furnaces, turrets, quarries, etc.

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;
using Object = UnityEngine.Object;
using Random = System.Random;

Oxide.Plugins
[Info("JPipes", "TheGreatJ", "0.6.10")]
[Description("Pipes that automatically transfer items between boxes, furnaces, turrets, quarries, etc.")]
 class JPipes : RustPlugin
{
    [PluginReference]
    private Plugin FurnaceSplitter;
    private Dictionary<ulong, UserInfo> users;
    private Dictionary<ulong, jPipe> regpipes;
    private Dictionary<ulong, jSyncBox> regsyncboxes;
    private PipeSaveData storedData;
    private static Color blue;
    private static Color orange;
    private static string bluestring;
    private static string orangestring;
     void Init();
     void OnServerInitialized();
    private int loadindex;
     void PipeLazyLoad();
     void LoadEnd();
    private void Loaded();
     void Unload();
     void OnNewSave(string filename);
     void OnServerSave();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void OnHammerHit(BasePlayer player, HitInfo hit);
     void OnStructureDemolish(BaseCombatEntity entity, BasePlayer player);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnEntityKill(BaseNetworkable entity);
     object OnEntityGroundMissing(BaseEntity entity);
     bool? OnStructureUpgrade(BaseCombatEntity entity, BasePlayer player, BuildingGrade.Enum grade);
     void OnStructureRepair(BaseCombatEntity entity, BasePlayer player);
     bool? OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     bool? CanPickupEntity(BaseCombatEntity entity, BasePlayer player);
     ItemContainer.CanAcceptResult? CanAcceptItem(ItemContainer container, Item item);
     bool? CanVendingAcceptItem(VendingMachine container, Item item);
     void OnItemRemovedFromContainer(ItemContainer container, Item item);
     bool? CanStackItem(Item targetItem, Item item);
    private bool commandperm(BasePlayer player);
    private void LoadCommands();
    private void cmdpipehelp(IPlayer cmdplayer, string cmd, string[] args);
    private void cmdpipecopy(IPlayer cmdplayer, string cmd, string[] args);
    private void cmdpiperemove(IPlayer cmdplayer, string cmd, string[] args);
    private void cmdpipestats(IPlayer cmdplayer, string cmd, string[] args);
    private void cmdpipelink(IPlayer cmdplayer, string cmd, string[] args);
    private void pipemainchat(IPlayer cmdplayer, string cmd, string[] args);
    private void pipehelp(BasePlayer player, string cmd, string[] args);
    private void pipecopy(BasePlayer player, string cmd, string[] args);
    private void piperemove(BasePlayer player, string cmd, string[] args);
    private void pipestats(BasePlayer player, string cmd, string[] args);
    private void pipelink(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("jpipes.create")]
    private void pipecreate(ConsoleSystem.Arg arg);
    [ConsoleCommand("jpipes.openmenu")]
    private void pipeopenmenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("jpipes.closemenu")]
    private void pipeclosemenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("jpipes.closemenudestroy")]
    private void pipeclosemenudestroy(ConsoleSystem.Arg arg);
    [ConsoleCommand("jpipes.refreshmenu")]
    private void piperefreshmenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("jpipes.changedir")]
    private void cmdpipechangedir(ConsoleSystem.Arg arg);
    [ConsoleCommand("jpipes.openfilter")]
    private void cmdpipeopenfilter(ConsoleSystem.Arg arg);
    [ConsoleCommand("jpipes.turnon")]
    private void pipeturnon(ConsoleSystem.Arg arg);
    [ConsoleCommand("jpipes.turnoff")]
    private void pipeturnoff(ConsoleSystem.Arg arg);
    [ConsoleCommand("jpipes.autostarton")]
    private void pipeautostarton(ConsoleSystem.Arg arg);
    [ConsoleCommand("jpipes.autostartoff")]
    private void pipeautostartoff(ConsoleSystem.Arg arg);
    [ConsoleCommand("jpipes.stackon")]
    private void pipestackon(ConsoleSystem.Arg arg);
    [ConsoleCommand("jpipes.stackoff")]
    private void pipestackoff(ConsoleSystem.Arg arg);
    [ConsoleCommand("jpipes.fsenable")]
    private void pipeFSenable(ConsoleSystem.Arg arg);
    [ConsoleCommand("jpipes.fsdisable")]
    private void pipeFSdisable(ConsoleSystem.Arg arg);
    [ConsoleCommand("jpipes.fsstack")]
    private void pipeFSstack(ConsoleSystem.Arg arg);
    private class UserInfo
    {
        public UserState state;
        public bool isUsingBind;
        public BaseEntity placestart;
        public BaseEntity placeend;
        public jPipeData clipboard;
        public bool isMenuOpen;
        public string menu;
        public string overlay;
        public string overlaytext;
        public string overlaysubtext;
        public Dictionary<ulong, jPipe> pipes;
    }

    private UserInfo GetUserInfo(BasePlayer player);
    private UserInfo GetUserInfo(ulong id);
    private class jPipe
    {
        public Action<ulong, bool> remover;
        public Action<Item, int, StorageContainer, int> moveitem;
        private JPipes pipeplugin;
        public ulong id;
        public string initerr;
        public string debugstring;
        public ulong ownerid;
        public string ownername;
        public bool isEnabled;
        public bool isWaterPipe;
        public BaseEntity mainparent;
        public BaseEntity source;
        public BaseEntity dest;
        public Vector3 sourcepos;
        public Vector3 endpos;
        public string sourceiconurl;
        public string endiconurl;
        public StorageContainer sourcecont;
        public StorageContainer destcont;
        public uint sourcechild;
        public uint destchild;
        public jPipeLogic mainlogic;
        public BuildingGrade.Enum pipegrade;
        public float health;
        public List<BaseEntity> pillars;
        private BaseEntity filterstash;
        private StorageContainer stashcont;
        private int lookingatstash;
        public bool singlestack;
        public bool autostarter;
        private bool destisstartable;
        public List<int> filteritems;
        public bool fsplit;
        public int fsstacks;
        public List<BasePlayer> playerslookingatmenu;
        public List<BasePlayer> playerslookingatfilter;
        private float distance;
        private Quaternion rotation;
        public jPipe();
        public bool init(JPipes pplug, ulong nid, jPipeData data, Action<ulong, bool> rem, Action<Item, int, StorageContainer, int> mover);
        private Vector3 containeroffset(BaseEntity e);
        private bool isStartable(BaseEntity e);
        public void OpenFilter(BasePlayer player);
        public void LookInFilter(BasePlayer player, StorageContainer stash);
        public void FilterCallback(BasePlayer player);
        private void DestroyFilter();
        public void UpdateFilterItems(Item item);
        public void ChangeDirection();
        public void OnSegmentKilled();
        public void Destroy(bool removeme);
        public void Upgrade(BuildingGrade.Enum grade);
        public void SetHealth(float nhealth);
        private static string ArrowString(int count);
        public void OpenMenu(BasePlayer player, UserInfo userinfo);
        public void CloseMenu(BasePlayer player, UserInfo userinfo);
        public void RefreshMenu();
        public string GetContIcon(BaseEntity e);
    }

    private class jSyncBox
    {
        private JPipes pipeplugin;
        public ulong id;
        public string initerr;
        public ulong ownerid;
        public string ownername;
        public bool isEnabled;
    }

    private static float pipesegdist;
    private static Vector3 pipefightoffset;
    private static class contoffset
    {
        public static Vector3 turret;
        public static Vector3 refinery;
        public static Vector3 furnace;
        public static Vector3 largefurnace;
        public static Vector3 searchlight;
        public static Vector3 pumpfuel;
        public static Vector3 pumpoutput;
        public static Vector3 recycler;
        public static Vector3 largewatercatcher;
        public static Vector3 smallwatercatcher;
        public static Vector3 waterbarrel;
        public static Vector3 waterpurifier;
        public static Vector3 bbq;
    }

    readonly static Dictionary<string, string> ItemUrls;
    private void startplacingpipe(BasePlayer player, bool isUsingBind);
    private void startlinking(BasePlayer player, bool isUsingBind);
    private void NewPipe(BasePlayer player, UserInfo userinfo);
    private void NewSyncBox(BasePlayer player, UserInfo userinfo);
    private Random randomidgen;
    private ulong pipeidgen();
    private ulong syncboxidgen();
    private static bool checkpipeoverlap(Dictionary<ulong, jPipe> rps, jPipeData data);
    private static bool checkcontwhitelist(BaseEntity e);
    private bool checkcontprivlage(BaseEntity e, BasePlayer p);
    private bool checkbuildingprivlage(BasePlayer p);
    private bool checkplayerpipelimit(BasePlayer p, UserInfo user);
    private bool checkplayerpipelimit(BasePlayer p, UserInfo user, int pipelimit);
    private int getplayerpipelimit(BasePlayer p);
    private int getplayerupgradelimit(BasePlayer p);
    private static StorageContainer getcontfromid(uint id, uint cid);
    private static StorageContainer getchildcont(BaseEntity parent, uint id);
    private bool IsPipe(BaseEntity entity);
    private void RegisterPipe(jPipe pipe);
    private void UnRegisterPipe(jPipe pipe);
    private void UnRegisterPipe(ulong id);
    public void RemovePipe(ulong id, bool remove);
    private void UnloadPipes();
    private void UnloadPipe(jPipe p);
    private void SavePipes();
    public void MoveItem(Item itemtomove, int amounttotake, StorageContainer cont, int stacks);
    private class jPipeLogic : MonoBehaviour
    {
        public jPipe pipe;
        public int tickdelay;
        public int flowrate;
        public static jPipeLogic Attach(BaseEntity entity, jPipe pipe, int tickdelay, int flowrate);
        private float period;
         void Update();
        public void pipeEnable(BasePlayer player);
        public void pipeDisable(BasePlayer player);
        private static Item FilterItem(List<Item> cont, List<int> filter);
        private bool CanAcceptItem(Item itemtomove);
        private Item FindItem();
        private void TurnOnDest();
    }

    private class jPipeSegChild : MonoBehaviour
    {
        public jPipe pipe;
        public static void Attach(BaseEntity entity, jPipe pipe);
    }

    private class jPipeSegChildLights : jPipeSegChild
    {
        public static void Attach(BaseEntity entity, jPipe pipe);
    }

    private class jPipeGroundWatch : MonoBehaviour
    {
        public HashSet<jPipe> connectedpipes;
        public static void Attach(BaseEntity entity, jPipe pipe);
    }

    private class jPipeFilterStash : MonoBehaviour
    {
        private Action<BasePlayer> callback;
        private Action<Item> itemcallback;
        public BaseEntity entityOwner;
        public bool itemadded;
        public bool loading;
        public static jPipeFilterStash Attach(BaseEntity entity, Action<BasePlayer> callback, Action<Item> itemcallback);
        private void PlayerStoppedLooting(BasePlayer player);
        public void UpdateFilter(Item item);
    }

    private class jPipeVendingMachine : MonoBehaviour
    {
        public jPipe pipe;
        public VendingMachine vm;
        public static void Attach(VendingMachine entity, jPipe pipe);
    }

    private class jSyncBoxLogic : MonoBehaviour
    {
        public jSyncBox syncbox;
        public static jSyncBoxLogic Attach(BaseEntity entity, jSyncBox syncbox);
    }

    private class jSyncBoxChild : MonoBehaviour
    {
        public jSyncBox syncbox;
        public static void Attach(BaseEntity entity, jSyncBox syncbox);
    }

    private static CuiLabel CreateLabel(string text, int i, float rowHeight, TextAnchor align, int fontSize, string xMin, string xMax, string color);
    private static CuiButton CreateButton(string command, float i, float rowHeight, int fontSize, string content, string xMin, string xMax, string color, string textcolor, float offset);
    private static CuiPanel CreatePanel(string anchorMin, string anchorMax, string color);
    private static CuiElement CuiInputField(string parent, string command, string text, int fontsize, int charlimit, string name);
    private static CuiElement CuiLabelWithOutline(CuiLabel label, string parent, string color, string dist, bool usealpha, string name);
    private void ShowOverlayText(BasePlayer player, string text, string subtext, string textcolor);
    private void HideOverlayText(BasePlayer player, float delay);
    private static CuiElement CreateItemIcon(string parent, string anchorMin, string anchorMax, string imageurl, string color);
    static string GetItemIconURL(string name, int size);
    private class permlevel
    {
        public int pipelimit;
        public int upgradelimit;
    }

    private void registerpermlevels();
    private static float maxpipedist;
    private static float minpipedist;
    private static int updaterate;
    private static bool drawflowarrows;
    private static bool animatearrows;
    private string pipecommandprefix;
    private string pipehotkey;
    private List<int> flowrates;
    private List<int> filtersizes;
    private bool nodecay;
    private bool xmaslights;
    private Dictionary<string, permlevel> permlevels;
    protected override void LoadDefaultConfig();
    private void LoadConfig();
    private T ConfigGet(string configstring, T fallback, Func<T, bool> cond, string warning);
    private bool IsNoDecayEnabled();
    private class PipeSaveData
    {
        public Dictionary<ulong, jPipeData> p;
        public Dictionary<ulong, jSyncBoxData> s;
        public PipeSaveData();
    }

    private class jPipeData
    {
        public bool e;
        public int g;
        public uint s;
        public uint d;
        public uint cs;
        public uint cd;
        public float h;
        public List<int> f;
        public bool st;
        public bool a;
        public bool fs;
        public int fss;
        public ulong o;
        public string on;
        public jPipeData();
        public void fromPipe(jPipe p);
        public void toPipe(jPipe p);
        public void setContainers(BaseEntity start, BaseEntity end);
        private uint setCont(BaseEntity cont, uint cid);
    }

    private class jSyncBoxData
    {
        public bool e;
        public uint s;
        public uint d;
        public float h;
        public ulong o;
        public string on;
        public jSyncBoxData();
        public void fromSyncBox(jSyncBox p);
        public void toSyncBox(jSyncBox p);
    }

    private static void LoadData(T data);
    private static void SaveData(T data);
}

private class UserInfo
{
    public UserState state;
    public bool isUsingBind;
    public BaseEntity placestart;
    public BaseEntity placeend;
    public jPipeData clipboard;
    public bool isMenuOpen;
    public string menu;
    public string overlay;
    public string overlaytext;
    public string overlaysubtext;
    public Dictionary<ulong, jPipe> pipes;
}

private class jPipe
{
    public Action<ulong, bool> remover;
    public Action<Item, int, StorageContainer, int> moveitem;
    private JPipes pipeplugin;
    public ulong id;
    public string initerr;
    public string debugstring;
    public ulong ownerid;
    public string ownername;
    public bool isEnabled;
    public bool isWaterPipe;
    public BaseEntity mainparent;
    public BaseEntity source;
    public BaseEntity dest;
    public Vector3 sourcepos;
    public Vector3 endpos;
    public string sourceiconurl;
    public string endiconurl;
    public StorageContainer sourcecont;
    public StorageContainer destcont;
    public uint sourcechild;
    public uint destchild;
    public jPipeLogic mainlogic;
    public BuildingGrade.Enum pipegrade;
    public float health;
    public List<BaseEntity> pillars;
    private BaseEntity filterstash;
    private StorageContainer stashcont;
    private int lookingatstash;
    public bool singlestack;
    public bool autostarter;
    private bool destisstartable;
    public List<int> filteritems;
    public bool fsplit;
    public int fsstacks;
    public List<BasePlayer> playerslookingatmenu;
    public List<BasePlayer> playerslookingatfilter;
    private float distance;
    private Quaternion rotation;
    public jPipe();
    public bool init(JPipes pplug, ulong nid, jPipeData data, Action<ulong, bool> rem, Action<Item, int, StorageContainer, int> mover);
    private Vector3 containeroffset(BaseEntity e);
    private bool isStartable(BaseEntity e);
    public void OpenFilter(BasePlayer player);
    public void LookInFilter(BasePlayer player, StorageContainer stash);
    public void FilterCallback(BasePlayer player);
    private void DestroyFilter();
    public void UpdateFilterItems(Item item);
    public void ChangeDirection();
    public void OnSegmentKilled();
    public void Destroy(bool removeme);
    public void Upgrade(BuildingGrade.Enum grade);
    public void SetHealth(float nhealth);
    private static string ArrowString(int count);
    public void OpenMenu(BasePlayer player, UserInfo userinfo);
    public void CloseMenu(BasePlayer player, UserInfo userinfo);
    public void RefreshMenu();
    public string GetContIcon(BaseEntity e);
}

private class jSyncBox
{
    private JPipes pipeplugin;
    public ulong id;
    public string initerr;
    public ulong ownerid;
    public string ownername;
    public bool isEnabled;
}

private static class contoffset
{
    public static Vector3 turret;
    public static Vector3 refinery;
    public static Vector3 furnace;
    public static Vector3 largefurnace;
    public static Vector3 searchlight;
    public static Vector3 pumpfuel;
    public static Vector3 pumpoutput;
    public static Vector3 recycler;
    public static Vector3 largewatercatcher;
    public static Vector3 smallwatercatcher;
    public static Vector3 waterbarrel;
    public static Vector3 waterpurifier;
    public static Vector3 bbq;
}

private class jPipeLogic : MonoBehaviour
{
    public jPipe pipe;
    public int tickdelay;
    public int flowrate;
    public static jPipeLogic Attach(BaseEntity entity, jPipe pipe, int tickdelay, int flowrate);
    private float period;
     void Update();
    public void pipeEnable(BasePlayer player);
    public void pipeDisable(BasePlayer player);
    private static Item FilterItem(List<Item> cont, List<int> filter);
    private bool CanAcceptItem(Item itemtomove);
    private Item FindItem();
    private void TurnOnDest();
}

private class jPipeSegChild : MonoBehaviour
{
    public jPipe pipe;
    public static void Attach(BaseEntity entity, jPipe pipe);
}

private class jPipeSegChildLights : jPipeSegChild
{
    public static void Attach(BaseEntity entity, jPipe pipe);
}

private class jPipeGroundWatch : MonoBehaviour
{
    public HashSet<jPipe> connectedpipes;
    public static void Attach(BaseEntity entity, jPipe pipe);
}

private class jPipeFilterStash : MonoBehaviour
{
    private Action<BasePlayer> callback;
    private Action<Item> itemcallback;
    public BaseEntity entityOwner;
    public bool itemadded;
    public bool loading;
    public static jPipeFilterStash Attach(BaseEntity entity, Action<BasePlayer> callback, Action<Item> itemcallback);
    private void PlayerStoppedLooting(BasePlayer player);
    public void UpdateFilter(Item item);
}

private class jPipeVendingMachine : MonoBehaviour
{
    public jPipe pipe;
    public VendingMachine vm;
    public static void Attach(VendingMachine entity, jPipe pipe);
}

private class jSyncBoxLogic : MonoBehaviour
{
    public jSyncBox syncbox;
    public static jSyncBoxLogic Attach(BaseEntity entity, jSyncBox syncbox);
}

private class jSyncBoxChild : MonoBehaviour
{
    public jSyncBox syncbox;
    public static void Attach(BaseEntity entity, jSyncBox syncbox);
}

private class permlevel
{
    public int pipelimit;
    public int upgradelimit;
}

private class PipeSaveData
{
    public Dictionary<ulong, jPipeData> p;
    public Dictionary<ulong, jSyncBoxData> s;
    public PipeSaveData();
}

private class jPipeData
{
    public bool e;
    public int g;
    public uint s;
    public uint d;
    public uint cs;
    public uint cd;
    public float h;
    public List<int> f;
    public bool st;
    public bool a;
    public bool fs;
    public int fss;
    public ulong o;
    public string on;
    public jPipeData();
    public void fromPipe(jPipe p);
    public void toPipe(jPipe p);
    public void setContainers(BaseEntity start, BaseEntity end);
    private uint setCont(BaseEntity cont, uint cid);
}

private class jSyncBoxData
{
    public bool e;
    public uint s;
    public uint d;
    public float h;
    public ulong o;
    public string on;
    public jSyncBoxData();
    public void fromSyncBox(jSyncBox p);
    public void toSyncBox(jSyncBox p);
}


```

---

## JunkyardShredder by Clearshot - Configure the items output by the junkyard shredder

```csharp
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Junkyard Shredder", "Clearshot", "1.1.0")]
[Description("Configure the items output by the junkyard shredder")]
 class JunkyardShredder : CovalencePlugin
{
    private PluginConfig _config;
    private Dictionary<string, Dictionary<string, int>> _vanillaItemAmounts;
     void Init();
     void OnServerInitialized();
     void OnEntitySpawned(BaseEntity ent);
     void Unload();
     void UpdateShredResources(MagnetLiftable ml, Dictionary<string, int> list);
    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
    protected override void LoadConfig();
    private class PluginConfig
    {
        public Dictionary<string, ShredConfig> shredderConfig;
    }

    private class ShredConfig
    {
        public Dictionary<string, int> vanilla;
        public Dictionary<string, int> modified;
    }

}

private class PluginConfig
{
    public Dictionary<string, ShredConfig> shredderConfig;
}

private class ShredConfig
{
    public Dictionary<string, int> vanilla;
    public Dictionary<string, int> modified;
}


```

---

## KarmaKills by Ryan - Rewards players on karma on kill, or takes karma away from them

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using System.Collections.Generic;

Oxide.Plugins
[Info("Karma Kills", "Ryan", "1.0.3")]
[Description("Rewards players on karma on kill, or takes karma away from them")]
public class KarmaKills : RustPlugin
{
    private string Lang(string key, string id, object[] args);
    [PluginReference]
    private Plugin KarmaSystem;
    private ConfigFile configFile;
    public class ConfigFile
    {
        public PlayerKills PlayerKills { get; set; }
        public AnimalKills AnimalKills { get; set; }
        public Killstreak Killstreak { get; set; }
        public Hero Hero { get; set; }
        public Bandit Bandit { get; set; }
        public static ConfigFile DefaultConfig();
    }

    public class Killstreak
    {
        [JsonProperty(PropertyName = "Kills before a bounty is placed")]
        public int Amount { get; set; }
    }

    public class PlayerKills
    {
        public bool Enabled { get; set; }
        public Karma AddKarma { get; set; }
        public Karma RemoveKarma { get; set; }
    }

    public class AnimalKills
    {
        public bool Enabled { get; set; }
        public Karma AddKarma { get; set; }
        public Karma RemoveKarma { get; set; }
    }

    public class Karma
    {
        public bool Enabled { get; set; }
        public int Amount { get; set; }
    }

    public class Hero
    {
        [JsonProperty(PropertyName = "Remove Karma to killer of Hero")]
        public Karma RemoveKarma { get; set; }
    }

    public class Bandit
    {
        [JsonProperty(PropertyName = "Add Karma to killer of Bandit")]
        public Karma AddKarma { get; set; }
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private static StoredData storedData;
    public class StoredData
    {
        public Dictionary<ulong, int> Killstreaks;
        public List<ulong> Heroes;
        public List<ulong> Bandits;
    }

    public class Data
    {
        public class Killstreaks
        {
            public static void Add(BasePlayer player);
            public static void Remove(BasePlayer player);
            public static bool Exists(BasePlayer player);
            public static void IncrementStreak(BasePlayer player);
            public static int GetStreak(BasePlayer player);
        }

        public class Heroes
        {
            public static void Add(BasePlayer player);
            public static void Remove(BasePlayer player);
            public static bool Exists(BasePlayer player);
        }

        public class Bandits
        {
            public static void Add(BasePlayer player);
            public static void Remove(BasePlayer player);
            public static bool Exists(BasePlayer player);
        }

        public static void SaveData();
    }

    private new void LoadDefaultMessages();
    private void CheckStatus(BasePlayer player);
    private bool WasBandit(BasePlayer player);
    private bool WasHero(BasePlayer player);
    private string GetAnimalName(string name);
    private void Loaded();
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    [ChatCommand("bandits")]
    private void banditCmd(BasePlayer player, string command, string[] args);
    [ChatCommand("heroes")]
    private void heroCmd(BasePlayer player, string command, string[] args);
}

public class ConfigFile
{
    public PlayerKills PlayerKills { get; set; }
    public AnimalKills AnimalKills { get; set; }
    public Killstreak Killstreak { get; set; }
    public Hero Hero { get; set; }
    public Bandit Bandit { get; set; }
    public static ConfigFile DefaultConfig();
}

public class Killstreak
{
    [JsonProperty(PropertyName = "Kills before a bounty is placed")]
    public int Amount { get; set; }
}

public class PlayerKills
{
    public bool Enabled { get; set; }
    public Karma AddKarma { get; set; }
    public Karma RemoveKarma { get; set; }
}

public class AnimalKills
{
    public bool Enabled { get; set; }
    public Karma AddKarma { get; set; }
    public Karma RemoveKarma { get; set; }
}

public class Karma
{
    public bool Enabled { get; set; }
    public int Amount { get; set; }
}

public class Hero
{
    [JsonProperty(PropertyName = "Remove Karma to killer of Hero")]
    public Karma RemoveKarma { get; set; }
}

public class Bandit
{
    [JsonProperty(PropertyName = "Add Karma to killer of Bandit")]
    public Karma AddKarma { get; set; }
}

public class StoredData
{
    public Dictionary<ulong, int> Killstreaks;
    public List<ulong> Heroes;
    public List<ulong> Bandits;
}

public class Data
{
    public class Killstreaks
    {
        public static void Add(BasePlayer player);
        public static void Remove(BasePlayer player);
        public static bool Exists(BasePlayer player);
        public static void IncrementStreak(BasePlayer player);
        public static int GetStreak(BasePlayer player);
    }

    public class Heroes
    {
        public static void Add(BasePlayer player);
        public static void Remove(BasePlayer player);
        public static bool Exists(BasePlayer player);
    }

    public class Bandits
    {
        public static void Add(BasePlayer player);
        public static void Remove(BasePlayer player);
        public static bool Exists(BasePlayer player);
    }

    public static void SaveData();
}

public class Killstreaks
{
    public static void Add(BasePlayer player);
    public static void Remove(BasePlayer player);
    public static bool Exists(BasePlayer player);
    public static void IncrementStreak(BasePlayer player);
    public static int GetStreak(BasePlayer player);
}

public class Heroes
{
    public static void Add(BasePlayer player);
    public static void Remove(BasePlayer player);
    public static bool Exists(BasePlayer player);
}

public class Bandits
{
    public static void Add(BasePlayer player);
    public static void Remove(BasePlayer player);
    public static bool Exists(BasePlayer player);
}


```

---

## KarmaSystem by Ryan - Allows players to upvote/downvote other players

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("KarmaSystem", "Ryan", "1.1.0")]
[Description("Allows players to upvote/downvote other players")]
internal class KarmaSystem : CovalencePlugin
{
    private ConfigFile configFile;
    public class ConfigFile
    {
        public Upvote UpvoteSettings { get; set; }
        public Downvote DownvoteSettings { get; set; }
        public Ranking Top { get; set; }
        public Ranking Bottom { get; set; }
        public bool EnableKarmaCommand { get; set; }
        public float Cooldown { get; set; }
        public bool OneVotePerPlayer { get; set; }
        public static ConfigFile DefaultConfig();
    }

    public class Upvote
    {
        public int Amount { get; set; }
        public bool EnableCommand { get; set; }
    }

    public class Downvote
    {
        public int Amount { get; set; }
        public bool EnableCommand { get; set; }
    }

    public class Ranking
    {
        public int ShowAmount { get; set; }
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
    private static StoredData storedData;
    public class StoredData
    {
        public Dictionary<string, PlayerData> Players;
    }

    public class PlayerData
    {
        public PlayerData();
        public int Karma { get; set; }
        public DateTime? Cooldown { get; set; }
        public List<string> Votes { get; set; }
    }

    public class Data
    {
        public static void AddNew(IPlayer player);
        public static void AddVoted(IPlayer player, string ID);
        public static bool Exists(IPlayer player);
        public static void Remove(IPlayer player);
        public static void Clear();
        public static void Save();
    }

    private IPlayer FindPlayer(IPlayer player, string input);
    private bool CanVote(IPlayer player, IPlayer target);
     int GetKarma(IPlayer player);
     void AddKarma(IPlayer player, int amount);
     void RemoveKarma(IPlayer player, int amount);
     void AddPlayer(IPlayer player);
     void AddVoted(IPlayer player, string ID);
     void OnVoted(IPlayer player, IPlayer target, int amount);
    private void Unload();
    private void Loaded();
    [Command("upvote")]
    private void upvoteCmd(IPlayer player, string command, string[] args);
    [Command("downvote")]
    private void downvoteCmd(IPlayer player, string command, string[] args);
    [Command("karma")]
    private void karmaCmd(IPlayer player, string command, string[] args);
}

public class ConfigFile
{
    public Upvote UpvoteSettings { get; set; }
    public Downvote DownvoteSettings { get; set; }
    public Ranking Top { get; set; }
    public Ranking Bottom { get; set; }
    public bool EnableKarmaCommand { get; set; }
    public float Cooldown { get; set; }
    public bool OneVotePerPlayer { get; set; }
    public static ConfigFile DefaultConfig();
}

public class Upvote
{
    public int Amount { get; set; }
    public bool EnableCommand { get; set; }
}

public class Downvote
{
    public int Amount { get; set; }
    public bool EnableCommand { get; set; }
}

public class Ranking
{
    public int ShowAmount { get; set; }
}

public class StoredData
{
    public Dictionary<string, PlayerData> Players;
}

public class PlayerData
{
    public PlayerData();
    public int Karma { get; set; }
    public DateTime? Cooldown { get; set; }
    public List<string> Votes { get; set; }
}

public class Data
{
    public static void AddNew(IPlayer player);
    public static void AddVoted(IPlayer player, string ID);
    public static bool Exists(IPlayer player);
    public static void Remove(IPlayer player);
    public static void Clear();
    public static void Save();
}


```

---

## KDRGui by Ankawi - GUI that shows kills, deaths, player name, and K/D Ratio

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("KDR GUI", "Ankawi", "2.0.3")]
[Description("GUI that portrays kills, deaths, player name, and K/D ratio")]
 class KDRGui : RustPlugin
{
     Dictionary<ulong, HitInfo> LastWounded;
    static HashSet<PlayerData> LoadedPlayerData;
     List<UIObject> UsedUI;
     class UIColor
    {
         double red;
         double green;
         double blue;
         double alpha;
        public UIColor(double red, double green, double blue, double alpha);
        public override string ToString();
    }

     class UIObject
    {
         List<object> ui;
         List<string> objectList;
        public UIObject();
        public string RandomString();
        public void Draw(BasePlayer player);
        public void Destroy(BasePlayer player);
        public string AddPanel(string name, double left, double top, double width, double height, UIColor color, bool mouse, string parent);
        public string AddText(string name, double left, double top, double width, double height, UIColor color, string text, int textsize, string parent, int alignmode);
        public string AddButton(string name, double left, double top, double width, double height, UIColor color, string command, string parent, string closeUi);
        public string AddImage(string name, double left, double top, double width, double height, UIColor color, string url, string parent);
    }

     class PlayerData
    {
        public ulong id;
        public string name;
        public int kills;
        public int deaths;
        internal float KDR { get; set; }
        internal static void TryLoad(BasePlayer player);
        internal void Update(BasePlayer player);
        internal void Save();
        internal static PlayerData Find(BasePlayer player);
    }

     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void Unload();
     void OnServerInitialized();
     HitInfo TryGetLastWounded(ulong id, HitInfo info);
     void OnEntityTakeDamage(BasePlayer player, HitInfo info);
     void OnPlayerDeath(BasePlayer player, HitInfo info);
     void DrawKDRWindow(BasePlayer player);
    private void LoadSleepers();
     string GetTopKills();
     string GetDeaths();
     string GetKDRs();
     string GetNames();
    [ChatCommand("top")]
     void cmdTop(BasePlayer player, string command, string[] args);
    [ChatCommand("kdr")]
     void cmdKdr(BasePlayer player, string command, string[] args);
     void GetCurrentStats(BasePlayer player);
}

 class UIColor
{
     double red;
     double green;
     double blue;
     double alpha;
    public UIColor(double red, double green, double blue, double alpha);
    public override string ToString();
}

 class UIObject
{
     List<object> ui;
     List<string> objectList;
    public UIObject();
    public string RandomString();
    public void Draw(BasePlayer player);
    public void Destroy(BasePlayer player);
    public string AddPanel(string name, double left, double top, double width, double height, UIColor color, bool mouse, string parent);
    public string AddText(string name, double left, double top, double width, double height, UIColor color, string text, int textsize, string parent, int alignmode);
    public string AddButton(string name, double left, double top, double width, double height, UIColor color, string command, string parent, string closeUi);
    public string AddImage(string name, double left, double top, double width, double height, UIColor color, string url, string parent);
}

 class PlayerData
{
    public ulong id;
    public string name;
    public int kills;
    public int deaths;
    internal float KDR { get; set; }
    internal static void TryLoad(BasePlayer player);
    internal void Update(BasePlayer player);
    internal void Save();
    internal static PlayerData Find(BasePlayer player);
}


```

---

## KDRScoreboard by Ankawi - Provides scoreboards that can show kills, deaths, or kill/death ratio

```csharp
using System.Collections.Generic;
using Oxide.Core;
using System.Linq;
using System;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("KDR Scoreboard", "Ankawi", "1.0.2")]
[Description("Enables scoreboards that can show Kills, Deaths, or K/D ratio")]
 class KDRScoreboard : RustPlugin
{
    [PluginReference]
     Plugin Scoreboards;
    private static KDRScoreboard Instance;
     Dictionary<ulong, HitInfo> LastWounded;
    static HashSet<PlayerData> LoadedPlayerData;
     class PlayerData
    {
        public ulong id;
        public string name;
        public int kills;
        public int deaths;
        internal float KDR { get; set; }
        internal static void TryLoad(BasePlayer player);
        internal void Update(BasePlayer player);
        internal void Save();
        internal static PlayerData Find(BasePlayer player);
    }

     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void OnServerInitialized();
     HitInfo TryGetLastWounded(ulong id, HitInfo info);
     void OnEntityTakeDamage(BasePlayer player, HitInfo info);
     void OnPlayerDeath(BasePlayer player, HitInfo info);
     void KDR_Scoreboard();
     void KillsScoreboard();
     void DeathsScoreboard();
     void UpdateScoreboard();
    [ChatCommand("kdr")]
     void cmdKdr(BasePlayer player, string command, string[] args);
     void GetCurrentStats(BasePlayer player);
}

 class PlayerData
{
    public ulong id;
    public string name;
    public int kills;
    public int deaths;
    internal float KDR { get; set; }
    internal static void TryLoad(BasePlayer player);
    internal void Update(BasePlayer player);
    internal void Save();
    internal static PlayerData Find(BasePlayer player);
}


```

---

## KeepActiveItem by Ryz0r - Restores a player's held item in their corpse's hotbar when they die.

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("Keep Active Item", "Ryz0r", "2.1.0")]
[Description("Restores a player's held item in their corpse's hotbar when they die.")]
 class KeepActiveItem : RustPlugin
{
    const string PermissionName;
     Dictionary<string, Item> _playerHeldItem;
    private void Init();
    private void OnActiveItemChange(BasePlayer player, Item oldItem, ItemId newItemId);
    private void OnPlayerWound(BasePlayer player);
    private void OnPlayerRecover(BasePlayer player);
    private void OnEntityDeath(BasePlayer player, HitInfo info);
}


```

---

## KeyLockRaiding by birthdates - Bring back key lock guessing!

```csharp
using Newtonsoft.Json;
using JetBrains.Annotations;

Oxide.Plugins
[Info("Key Lock Raiding", "birthdates", "1.0.2")]
[Description("Bring back key lock guessing!")]
public class KeyLockRaiding : RustPlugin
{
    [UsedImplicitly]
    private void OnServerInitialized();
    private void OnItemDeployed(Deployer deployer, BaseEntity entity, KeyLock keyLock);
    private ConfigFile _config;
    public class ConfigFile
    {
        [JsonProperty("Max Possible Combinations (Rust default is 100000)")]
        public int MaxCombinations { get; set; }
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

public class ConfigFile
{
    [JsonProperty("Max Possible Combinations (Rust default is 100000)")]
    public int MaxCombinations { get; set; }
    public static ConfigFile DefaultConfig();
}


```

---

## Keywords by MrBlue - Get notified when a keyword is triggered in chat

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Keywords", "Wulf", "1.3.1")]
[Description("Sends notifications when a keyword is used in the chat")]
public class Keywords : CovalencePlugin
{
    private Configuration _config;
    public class Configuration
    {
        [JsonProperty("Permission required to trigger keywords")]
        public bool UsePermissions;
        [JsonProperty("Include original message with notification")]
        public bool IncludeOriginal;
        [JsonProperty("Match only exact keywords")]
        public bool MatchExact;
        [JsonProperty("Auto-reply for triggered keywords")]
        public bool AutoReply;
        [JsonProperty("Notify configured players in chat")]
        public bool NotifyPlayers;
        [JsonProperty("Players to notify in chat", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> PlayersToNotify;
        [JsonProperty("Notify configured groups in chat")]
        public bool NotifyGroups;
        [JsonProperty("Notify configured group in chat")]
        private bool NotifyGroupsOld { get; set; }
        [JsonProperty("Groups to notify in chat", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> GroupsToNotify;
        [JsonProperty("Notify in Discord channel")]
        public bool NotifyInDiscord;
        [JsonProperty("Discord embed color (decimal color code)")]
        public string DiscordEmbedColor;
        [JsonProperty("Role IDs to mention on Discord", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ulong> RolesToMention;
        [JsonProperty("Roles to mention on Discord",
            ObjectCreationHandling = ObjectCreationHandling.Replace)]
        private List<string> RolesToMentionOld { get; set; }
        [JsonProperty("User IDs to mention on Discord", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ulong> UsersToMention;
        [JsonProperty("Users to mention on Discord", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        private List<string> UsersToMentionOld { get; set; }
        [JsonProperty("Discord webhook URL")]
        public string DiscordWebhook;
        [JsonProperty("Keywords to listen for in chat", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Keywords;
        private string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    [PluginReference]
    private Plugin BetterChat;
    private Plugin GUIAnnouncements;
    private Plugin UINotify;
    private readonly Regex _keywordRegex;
    private const string DiscordJson;
    private const string PermissionUse;
    private const string PermissionBypass;
    private string _roleMentions;
    private string _userMentions;
    private void OnServerInitialized();
    private void HandleChat(IPlayer player, string message);
    private void OnBetterChat(Dictionary<string, object> data);
    private void OnUserChat(IPlayer player, string message);
    private string GetLang(string langKey, string playerId, object[] args);
    private void SendGuiNotification(IPlayer target, string notification);
}

public class Configuration
{
    [JsonProperty("Permission required to trigger keywords")]
    public bool UsePermissions;
    [JsonProperty("Include original message with notification")]
    public bool IncludeOriginal;
    [JsonProperty("Match only exact keywords")]
    public bool MatchExact;
    [JsonProperty("Auto-reply for triggered keywords")]
    public bool AutoReply;
    [JsonProperty("Notify configured players in chat")]
    public bool NotifyPlayers;
    [JsonProperty("Players to notify in chat", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> PlayersToNotify;
    [JsonProperty("Notify configured groups in chat")]
    public bool NotifyGroups;
    [JsonProperty("Notify configured group in chat")]
    private bool NotifyGroupsOld { get; set; }
    [JsonProperty("Groups to notify in chat", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> GroupsToNotify;
    [JsonProperty("Notify in Discord channel")]
    public bool NotifyInDiscord;
    [JsonProperty("Discord embed color (decimal color code)")]
    public string DiscordEmbedColor;
    [JsonProperty("Role IDs to mention on Discord", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ulong> RolesToMention;
    [JsonProperty("Roles to mention on Discord",
            ObjectCreationHandling = ObjectCreationHandling.Replace)]
    private List<string> RolesToMentionOld { get; set; }
    [JsonProperty("User IDs to mention on Discord", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ulong> UsersToMention;
    [JsonProperty("Users to mention on Discord", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    private List<string> UsersToMentionOld { get; set; }
    [JsonProperty("Discord webhook URL")]
    public string DiscordWebhook;
    [JsonProperty("Keywords to listen for in chat", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Keywords;
    private string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## KickCooldown by Rick6 - Gives players a cooldown after being kicked to prevent abuse

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("KickCooldown", "Kappasaurus", "1.0.1")]
 class KickCooldown : CovalencePlugin
{
     List<string> kicked;
     float kickCooldown;
     void Init();
     void OnUserDisconnected(IPlayer player, string reason);
     object CanUserLogin(string name, string id, string ip);
    private new void LoadConfig();
    protected override void LoadDefaultConfig();
    private void GetConfig(T variable, string[] path);
    protected override void LoadDefaultMessages();
}


```

---

## KillComplement by ProCelle - Heals and reloads a player's weapon after killing another player

```csharp
using Oxide.Core;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Kill Complement", "ProCelle", "1.0.5")]
[Description("Get health and reload your gun for killing another player")]
public class KillComplement : RustPlugin
{
    private PluginConfig config;
     void OnServerInitialized(bool initial);
    private const string USE;
    private void OnEntityDeath(BasePlayer player, HitInfo info);
    private class PluginConfig
    {
        [JsonProperty("Healed ammoun on kill")]
        public float Heal { get; set; }
    }

    private PluginConfig GetDefaultConfig();
    protected override void LoadDefaultConfig();
}

private class PluginConfig
{
    [JsonProperty("Healed ammoun on kill")]
    public float Heal { get; set; }
}


```

---

## KillFeed by VisEntities - Displays a basic kill feed on screen

```csharp
using System.Collections.Generic;
using System.Collections;
using System.Text;
using System;
using Oxide.Game.Rust.Cui;
using Oxide.Core;
using Newtonsoft.Json;
using UnityEngine;
using Network;
using Rust;

Oxide.Plugins
[Info("Kill Feed", "Tuntenfisch", "1.18")]
[Description("Displays a basic Kill Feed on screen")]
public class KillFeed : RustPlugin
{
    const float _screenAspectRatio;
    const float _height;
    const float _halfHeight;
    static int _debugging;
    static bool useServerFileStorage;
    static char _formattingChar;
    static List<string> debugLog;
    static GameObject _killFeedObject;
    static FileManager _fileManager;
    static Timer _timer;
    static Dictionary<ulong, KillFeedPlayer> _players;
     float width;
     float horizontalSpacing;
     float verticalSpacing;
     float fadeIn;
     float fadeOut;
     float destroyAfter;
     float iconHalfHeight;
     float iconHalfWidth;
     float halfWidth;
     int numberOfCharacters;
     int fontSize;
     bool outline;
     bool removeTags;
     bool removeSpecialCharacters;
     bool enableAnimals;
     bool displayPlayerDeaths;
     bool logEntries;
     bool printEntriesToConsole;
     bool[] allowedCharacters;
     string chatIcon;
     string font;
     string formatting;
     Vector2 anchormin;
     Vector2 anchormax;
     CuiElementContainer[] entries;
     List<string> entryLog;
     void OnServerInitialized();
     void Unload();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void CanBeWounded(BasePlayer player, HitInfo info);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    [ChatCommand("killfeed")]
     void ChatCommand(BasePlayer player, string command, string[] args);
    static class DefaultConfig
    {
        public static readonly Dictionary<string, ConfigValue> values;
        private static Dictionary<string, string> GetDefaultFiles();
        private static Dictionary<string, string> GetDefaultDamageTypeFiles();
        private static Dictionary<string, string> GetDefaultNPCNames();
        private static Dictionary<string, string> GetDefaultBoneNames();
        private static List<char> GetDefaultAllowedSpecialCharacters();
    }

     class ConfigValue
    {
        public object value { get; set; }
        public string[] path { get; set; }
        public ConfigValue(object value, string[] path);
    }

     T GetConfig(bool saveConfig, T defaultValue, string[] keys);
     T GetConfig(bool saveConfig, ConfigValue configValue);
     bool IsValueValid(object value, object defaultValue);
     void CleanupConfig();
    protected override void LoadDefaultConfig();
    new void LoadConfig();
    protected override void LoadDefaultMessages();
    static class StringHelper
    {
        public static string RemoveTag(string str);
        public static string RemoveSpecialCharacters(string str, bool[] characters, bool flag);
        public static string TrimToSize(string str, int size);
    }

    static void LogDebug(string[] args);
     class FileManager : MonoBehaviour
    {
        public static string fileDirectory;
        public static Dictionary<string, string> fileIDs;
        public static void Store(string key, string value);
        public static void Store(Dictionary<string, string> files);
        private static IEnumerator WaitForRequest(string shortname, string url);
    }

     class Timer : MonoBehaviour
    {
         bool isRunning;
         Coroutine instance;
        public void DelayedAction(Action action, float delay);
        private IEnumerator WaitForDelay(Action action, float delay);
    }

     class Key
    {
        public string key;
        public string action;
        public string defaultAction;
    }

     class KillFeedPlayer
    {
        public bool enabled { get; set; }
        public string username { get; set; }
        public string lastAction { get; set; }
        public Connection connection { get; set; }
        public KillFeedPlayer(Connection connection, string username);
    }

     string FormatUsername(string username);
     void AddPlayer(BasePlayer player);
     void RemovePlayer(BasePlayer player);
     class EntryData
    {
        public static string _initiatorColor { get; set; }
        public static string _infoColor { get; set; }
        public static string _hitEntityColor { get; set; }
        public static string _npcColor { get; set; }
        public static Dictionary<string, string> _npcNames { get; set; }
        public static Dictionary<string, string> _boneNames { get; set; }
        public bool selfInflicted { get; set; }
        public bool needsFormatting { get; set; }
        public EntityInfo initiatorInfo { get; set; }
        public EntityInfo hitEntityInfo { get; set; }
        public string initiatorColor { get; set; }
        public string hitEntityColor { get; set; }
        public WeaponInfo weaponInfo { get; set; }
        public string hitBone { get; set; }
        public string distance { get; set; }
        public string infoColor { get; set; }
        public EntryData(BaseCombatEntity entity, HitInfo info);
         EntityInfo GetInitiator(HitInfo info);
         EntityInfo GetHitEntity(BaseCombatEntity entity);
        public class EntityInfo
        {
            public string name { get; set; }
            public ulong userID { get; set; }
            public EntityInfo(string name, ulong userID);
        }

         WeaponInfo GetWeapon(HitInfo info);
        public class WeaponInfo
        {
            public string shortname { get; set; }
            public string weaponID { get; set; }
            public WeaponInfo(string shortname, string weaponID);
        }

         string GetDistance(BaseCombatEntity entity, HitInfo info);
         string GetHitBone(HitInfo info);
    }

     CuiElementContainer GetKillFeedEntry(EntryData entryData);
     void OnWoundedOrDeath(EntryData entryData);
    static void AddUI(CuiElementContainer elements);
    static void AddUI(CuiElementContainer elements, KillFeedPlayer player);
    static void DestroyUI(CuiElementContainer elements);
    static void DestroyUI(CuiElementContainer elements, KillFeedPlayer player);
    static CuiElementContainer ConvertToSingleContainer(CuiElementContainer[] containers);
}

static class DefaultConfig
{
    public static readonly Dictionary<string, ConfigValue> values;
    private static Dictionary<string, string> GetDefaultFiles();
    private static Dictionary<string, string> GetDefaultDamageTypeFiles();
    private static Dictionary<string, string> GetDefaultNPCNames();
    private static Dictionary<string, string> GetDefaultBoneNames();
    private static List<char> GetDefaultAllowedSpecialCharacters();
}

 class ConfigValue
{
    public object value { get; set; }
    public string[] path { get; set; }
    public ConfigValue(object value, string[] path);
}

static class StringHelper
{
    public static string RemoveTag(string str);
    public static string RemoveSpecialCharacters(string str, bool[] characters, bool flag);
    public static string TrimToSize(string str, int size);
}

 class FileManager : MonoBehaviour
{
    public static string fileDirectory;
    public static Dictionary<string, string> fileIDs;
    public static void Store(string key, string value);
    public static void Store(Dictionary<string, string> files);
    private static IEnumerator WaitForRequest(string shortname, string url);
}

 class Timer : MonoBehaviour
{
     bool isRunning;
     Coroutine instance;
    public void DelayedAction(Action action, float delay);
    private IEnumerator WaitForDelay(Action action, float delay);
}

 class Key
{
    public string key;
    public string action;
    public string defaultAction;
}

 class KillFeedPlayer
{
    public bool enabled { get; set; }
    public string username { get; set; }
    public string lastAction { get; set; }
    public Connection connection { get; set; }
    public KillFeedPlayer(Connection connection, string username);
}

 class EntryData
{
    public static string _initiatorColor { get; set; }
    public static string _infoColor { get; set; }
    public static string _hitEntityColor { get; set; }
    public static string _npcColor { get; set; }
    public static Dictionary<string, string> _npcNames { get; set; }
    public static Dictionary<string, string> _boneNames { get; set; }
    public bool selfInflicted { get; set; }
    public bool needsFormatting { get; set; }
    public EntityInfo initiatorInfo { get; set; }
    public EntityInfo hitEntityInfo { get; set; }
    public string initiatorColor { get; set; }
    public string hitEntityColor { get; set; }
    public WeaponInfo weaponInfo { get; set; }
    public string hitBone { get; set; }
    public string distance { get; set; }
    public string infoColor { get; set; }
    public EntryData(BaseCombatEntity entity, HitInfo info);
     EntityInfo GetInitiator(HitInfo info);
     EntityInfo GetHitEntity(BaseCombatEntity entity);
    public class EntityInfo
    {
        public string name { get; set; }
        public ulong userID { get; set; }
        public EntityInfo(string name, ulong userID);
    }

     WeaponInfo GetWeapon(HitInfo info);
    public class WeaponInfo
    {
        public string shortname { get; set; }
        public string weaponID { get; set; }
        public WeaponInfo(string shortname, string weaponID);
    }

     string GetDistance(BaseCombatEntity entity, HitInfo info);
     string GetHitBone(HitInfo info);
}

public class EntityInfo
{
    public string name { get; set; }
    public ulong userID { get; set; }
    public EntityInfo(string name, ulong userID);
}

public class WeaponInfo
{
    public string shortname { get; set; }
    public string weaponID { get; set; }
    public WeaponInfo(string shortname, string weaponID);
}


```

---

## KillHealth by  - Gives players health for killing another player

```csharp
using Oxide.Core;

Oxide.Plugins
[Info("Kill Health", "Orange", "1.0.0")]
[Description("Get health for killing another player")]
public class KillHealth : RustPlugin
{
    private void OnEntityDeath(BasePlayer player, HitInfo info);
}


```

---

## KillInfo by  - This plugin will inform you when you wound or kill a player.

```csharp
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Kill Info", "birthdates", "1.0", ResourceId = 0)]
[Description("Kill and Wound Info")]
public class KillInfo : RustPlugin
{
    private bool Wounded_Info;
    private bool Killed_Info;
    protected override void LoadDefaultConfig();
     void Init();
    protected override void LoadDefaultMessages();
     void OnPlayerWound(BasePlayer player);
     void OnPlayerDie(BasePlayer player, HitInfo info);
}


```

---

## KillRecords by MACHIN3 - Tracks kills for all players for every entity

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;
using MySql.Data.MySqlClient;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Database;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("Kill Records", "MACHIN3", "1.3.800")]
[Description("Records number of kills by each player for selected entities")]
public class KillRecords : RustPlugin
{
    public const string version;
    [PluginReference]
    private readonly Plugin ImageLibrary;
    private readonly Plugin UINotify;
    private PlayData _playData;
    private TempData _tempData;
    private HeliHits _heliHits;
    private DynamicConfigFile _KillData;
    private DynamicConfigFile _EntityData;
    private Dictionary<string, Record> _recordCache;
    private Dictionary<ulong, Data> _dataCache;
    private Dictionary<ulong, Heli> _heliCache;
    private Configuration config;
    private const string KRUIName;
    private const string KRUIMainName;
    private const string KRUISelection;
    private const string Admin;
    private const string Killchat;
    private Timer _helitracker;
    private class Configuration : SerializableConfiguration
    {
        [JsonProperty("Tracking Options")]
        public TrackingOptions trackingoptions;
        [JsonProperty("Player Chat Commands")]
        public PlayerChatCommands playerChatCommands;
        [JsonProperty("Admin Chat Commands")]
        public AdminChatCommands adminChatCommands;
        [JsonProperty("Harvest Options")]
        public HarvestOptions harvestoptions;
        [JsonProperty("Order Options")]
        public OrderOptions orderoptions;
        [JsonProperty("Chat & UI Options")]
        public ChatUI chatui;
        [JsonProperty("Web Request")]
        public WebRequest webrequest;
        [JsonProperty("SQL")]
        public KRSQL krsql;
    }

    public class PlayerChatCommands
    {
        public string krhelp;
        public string pkills;
        public string pkillschat;
        public string topkills;
        public string topkillschat;
        public string totalkills;
        public string totalkillschat;
        public string leadkills;
        public string pstats;
        public string pstatschat;
        public string topstats;
        public string totalstats;
        public string totalstatschat;
        public string killchat;
    }

    public class AdminChatCommands
    {
        public string krhelpadmin;
        public string krweb;
        public string krsql;
        public string krbackup;
        public string krreset;
    }

    private class TrackingOptions
    {
        public bool Trackchicken;
        public bool Trackboar;
        public bool Trackstag;
        public bool Trackwolf;
        public bool Trackbear;
        public bool Trackpolarbear;
        public bool Trackshark;
        public bool Trackhorse;
        public bool Trackfish;
        public bool TrackPlayer;
        public bool Trackscientist;
        public bool Trackscarecrow;
        public bool Trackdweller;
        public bool Tracklootcontainer;
        public bool Trackunderwaterlootcontainer;
        public bool Trackbradhelicrate;
        public bool Trackhackablecrate;
        public bool Trackdeaths;
        public bool Tracksuicides;
        public bool TrackAnimalHarvest;
        public bool TrackCorpseHarvest;
        public bool TrackBradley;
        public bool TrackHeli;
        public bool Trackturret;
        public bool Trackzombie;
    }

    private class HarvestOptions
    {
        public bool treescut;
        public bool oremined;
        public bool cactuscut;
        public bool woodpickup;
        public bool orepickup;
        public bool berriespickup;
        public bool pumpkinpickup;
        public bool potatopickup;
        public bool cornpickup;
        public bool mushroompickup;
        public bool hemppickup;
        public bool seedpickup;
    }

    private class OrderOptions
    {
        public int chickenpos;
        public int boarpos;
        public int stagpos;
        public int wolfpos;
        public int bearpos;
        public int polarbearpos;
        public int sharkpos;
        public int horsepos;
        public int fishpos;
        public int playerpos;
        public int scientistpos;
        public int dwellerpos;
        public int lootpos;
        public int unlootpos;
        public int bradhelicratepos;
        public int hackablecratepos;
        public int deathpos;
        public int suicidepos;
        public int corpsepos;
        public int pcorpsepos;
        public int bradleypos;
        public int helipos;
        public int scarecrowpos;
        public int turretpos;
    }

    private class ChatUI
    {
        public bool enableui;
        public bool UseImageLibrary;
        public bool ShowKillMessages;
        public int KillMessageInterval;
        public int KillMessageLimit;
        public bool enableuinotify;
        public bool disablechats;
        public int uinotifytype;
    }

    private class WebRequest
    {
        public bool UseWebrequests;
        public string DataURL;
        public string SecretKey;
    }

    private class KRSQL
    {
        public bool UseSQL;
        public int FileType;
        public string SQLhost;
        public int SQLport;
        public string SQLdatabase;
        public string SQLusername;
        public string SQLpassword;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    internal class SerializableConfiguration
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
    private void SaveTemp();
    private void LoadData();
    private class TempData
    {
        public Dictionary<ulong, Data> EntityRecords;
    }

    private class Data
    {
        public ulong entity;
        public List<string> id;
    }

    private class PlayData
    {
        public Dictionary<string, Record> KillRecords;
    }

    private class Record
    {
        public int chicken;
        public int boar;
        public int stag;
        public int wolf;
        public int bear;
        public int polarbear;
        public int shark;
        public int horse;
        public int fish;
        public int scientistnpcnew;
        public int scarecrow;
        public int dweller;
        public int baseplayer;
        public int basecorpse;
        public int npcplayercorpse;
        public int zombie;
        public int deaths;
        public int suicides;
        public int lootcontainer;
        public int underwaterlootcontainer;
        public int bradhelicrate;
        public int hackablecrate;
        public int bradley;
        public int heli;
        public int turret;
        public string displayname;
        public string id;
        public bool killchat;
        public int treescut;
        public int oremined;
        public int cactuscut;
        public int woodpickup;
        public int orepickup;
        public int berriespickup;
        public int pumpkinpickup;
        public int potatopickup;
        public int cornpickup;
        public int mushroompickup;
        public int hemppickup;
        public int seedpickup;
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

    private string RemoveSpecialCharacters(string name);
    private readonly Core.MySql.Libraries.MySql sqlLibrary;
     Connection sqlConnection;
    private void CreatSQLTable();
    private void UpdateSQLTable();
    private void CreatePlayerDataSQL(BasePlayer player);
    private void UpdatePlayersSQL();
    private void UpdatePlayersDataSQL();
    private void UpdatePlayerDataSQL(BasePlayer player);
    private void CheckPlayerDataSQL(BasePlayer player);
    private void LoadSQL();
    private void DeleteSQL();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnServerShutDown();
    private void OnServerSave();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void AddData(BasePlayer player, BaseEntity entity);
    private void UpdateData(BasePlayer player);
    private Record GetRecord(BasePlayer player);
    private static BasePlayer FindPlayer(string playerid);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo hitInfo);
    private void OnPlayerDeath(BasePlayer player, HitInfo hitInfo);
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private void OnFishCatch(Item fish, BasePlayer player);
    private void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private void OnCollectiblePickup(CollectibleEntity collectible, BasePlayer player);
    private void OnGrowableGathered(GrowableEntity growable, Item item, BasePlayer player);
    private void OnEntityKill(BaseNetworkable entity);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    private void KRHelp(BasePlayer player, string command, string[] args);
    private void PKills(BasePlayer player, string command, string[] args);
    private void PKillsChat(BasePlayer player, string command, string[] args);
    private void TopKills(BasePlayer player, string command, string[] args);
    private void TopKillsChat(BasePlayer player, string command, string[] args);
    private void TotalKills(BasePlayer player, string command, string[] args);
    private void TotalKillsChat(BasePlayer player, string command, string[] args);
    private void LeadKills(BasePlayer player, string command, string[] args);
    private void PStats(BasePlayer player, string command, string[] args);
    private void PStatsChat(BasePlayer player, string command, string[] args);
    private void TopStats(BasePlayer player, string command, string[] args);
    private void TotalStats(BasePlayer player, string command, string[] args);
    private void TotalStatsChat(BasePlayer player, string command, string[] args);
    private void KillChat(BasePlayer player, string command, string[] args);
    private void AdminKRHelp(BasePlayer player, string command, string[] args);
    private void AdminKRWeb(BasePlayer player, string command, string[] args);
    private void AdminKRsql(BasePlayer player, string command, string[] args);
    private void AdminKRBackup(BasePlayer player, string command, string[] args);
    private void AdminKRReset(BasePlayer player, string command, string[] args);
    [ConsoleCommand("kr.topkills")]
    private void Cmdtopkills(ConsoleSystem.Arg arg);
    [ConsoleCommand("kr.topstats")]
    private void Cmdtopstats(ConsoleSystem.Arg arg);
    [ConsoleCommand("kr.topplayers")]
    private void Cmdtopplayers(ConsoleSystem.Arg arg);
    [ConsoleCommand("kr.topplayersstats")]
    private void Cmdtopplayersstats(ConsoleSystem.Arg arg);
    [ConsoleCommand("kr.leaderboard")]
    private void Cmdleaderboard(ConsoleSystem.Arg arg);
    [ConsoleCommand("kr.totalkills")]
    private void Cmdtotalkills(ConsoleSystem.Arg arg);
    private IEnumerable<Record> GetTopKills(int page, int takeCount, string KillType);
    private int GetTotalKills(string KillType);
    private void KRKillMessages(BasePlayer player, string languagetype, int newkills, bool killchat);
    private void PlayerKillsChat(BasePlayer player, string playerinfo);
    private void PlayerStatsChat(BasePlayer player, string playerinfo);
    private void TopKillsChat(BasePlayer player, string KillType);
    private void TotalKillsChat(BasePlayer player);
    private void TotalStatsChat(BasePlayer player);
    private string KillTypesEnabled();
    private string GatherTypesEnabled();
    private string LeaderboardPositionLB(int position);
    private string LeaderboardPositionRT(int position);
    private object UILabels(BasePlayer player, string KillType);
    private CuiPanel KRUIPanel(string anchorMin, string anchorMax, string color);
    private CuiLabel KRUILabel(string text, int i, float height, TextAnchor align, int fontSize, string xMin, string xMax, string color);
    private CuiButton KRUIButton(string command, int i, float rowHeight, int fontSize, string color, string content, string xMin, string xMax, TextAnchor align);
    private void KRUIplayers(BasePlayer player, string playerinfo);
    private void KRUIStatsplayers(BasePlayer player, string playerinfo);
    private void KRUITop(BasePlayer player, string KillType);
    private void KRUIStatsTop(BasePlayer player, string KillType);
    private void KRUITopAll(BasePlayer player);
    private void KRUITotal(BasePlayer player);
    private void KRUIStatsTotal(BasePlayer player);
    private void DestroyUi(BasePlayer player, string name);
    protected override void LoadDefaultMessages();
    private string KRLang(string key, string id, object[] args);
    private void SaveKillRecordWeb(BasePlayer player);
    private object GetKillRecord(string playerid, string KillType);
    private object GetCach();
    private string GetVersion();
}

private class Configuration : SerializableConfiguration
{
    [JsonProperty("Tracking Options")]
    public TrackingOptions trackingoptions;
    [JsonProperty("Player Chat Commands")]
    public PlayerChatCommands playerChatCommands;
    [JsonProperty("Admin Chat Commands")]
    public AdminChatCommands adminChatCommands;
    [JsonProperty("Harvest Options")]
    public HarvestOptions harvestoptions;
    [JsonProperty("Order Options")]
    public OrderOptions orderoptions;
    [JsonProperty("Chat & UI Options")]
    public ChatUI chatui;
    [JsonProperty("Web Request")]
    public WebRequest webrequest;
    [JsonProperty("SQL")]
    public KRSQL krsql;
}

public class PlayerChatCommands
{
    public string krhelp;
    public string pkills;
    public string pkillschat;
    public string topkills;
    public string topkillschat;
    public string totalkills;
    public string totalkillschat;
    public string leadkills;
    public string pstats;
    public string pstatschat;
    public string topstats;
    public string totalstats;
    public string totalstatschat;
    public string killchat;
}

public class AdminChatCommands
{
    public string krhelpadmin;
    public string krweb;
    public string krsql;
    public string krbackup;
    public string krreset;
}

private class TrackingOptions
{
    public bool Trackchicken;
    public bool Trackboar;
    public bool Trackstag;
    public bool Trackwolf;
    public bool Trackbear;
    public bool Trackpolarbear;
    public bool Trackshark;
    public bool Trackhorse;
    public bool Trackfish;
    public bool TrackPlayer;
    public bool Trackscientist;
    public bool Trackscarecrow;
    public bool Trackdweller;
    public bool Tracklootcontainer;
    public bool Trackunderwaterlootcontainer;
    public bool Trackbradhelicrate;
    public bool Trackhackablecrate;
    public bool Trackdeaths;
    public bool Tracksuicides;
    public bool TrackAnimalHarvest;
    public bool TrackCorpseHarvest;
    public bool TrackBradley;
    public bool TrackHeli;
    public bool Trackturret;
    public bool Trackzombie;
}

private class HarvestOptions
{
    public bool treescut;
    public bool oremined;
    public bool cactuscut;
    public bool woodpickup;
    public bool orepickup;
    public bool berriespickup;
    public bool pumpkinpickup;
    public bool potatopickup;
    public bool cornpickup;
    public bool mushroompickup;
    public bool hemppickup;
    public bool seedpickup;
}

private class OrderOptions
{
    public int chickenpos;
    public int boarpos;
    public int stagpos;
    public int wolfpos;
    public int bearpos;
    public int polarbearpos;
    public int sharkpos;
    public int horsepos;
    public int fishpos;
    public int playerpos;
    public int scientistpos;
    public int dwellerpos;
    public int lootpos;
    public int unlootpos;
    public int bradhelicratepos;
    public int hackablecratepos;
    public int deathpos;
    public int suicidepos;
    public int corpsepos;
    public int pcorpsepos;
    public int bradleypos;
    public int helipos;
    public int scarecrowpos;
    public int turretpos;
}

private class ChatUI
{
    public bool enableui;
    public bool UseImageLibrary;
    public bool ShowKillMessages;
    public int KillMessageInterval;
    public int KillMessageLimit;
    public bool enableuinotify;
    public bool disablechats;
    public int uinotifytype;
}

private class WebRequest
{
    public bool UseWebrequests;
    public string DataURL;
    public string SecretKey;
}

private class KRSQL
{
    public bool UseSQL;
    public int FileType;
    public string SQLhost;
    public int SQLport;
    public string SQLdatabase;
    public string SQLusername;
    public string SQLpassword;
}

internal class SerializableConfiguration
{
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private static class JsonHelper
{
    public static object Deserialize(string json);
    private static object ToObject(JToken token);
}

private class TempData
{
    public Dictionary<ulong, Data> EntityRecords;
}

private class Data
{
    public ulong entity;
    public List<string> id;
}

private class PlayData
{
    public Dictionary<string, Record> KillRecords;
}

private class Record
{
    public int chicken;
    public int boar;
    public int stag;
    public int wolf;
    public int bear;
    public int polarbear;
    public int shark;
    public int horse;
    public int fish;
    public int scientistnpcnew;
    public int scarecrow;
    public int dweller;
    public int baseplayer;
    public int basecorpse;
    public int npcplayercorpse;
    public int zombie;
    public int deaths;
    public int suicides;
    public int lootcontainer;
    public int underwaterlootcontainer;
    public int bradhelicrate;
    public int hackablecrate;
    public int bradley;
    public int heli;
    public int turret;
    public string displayname;
    public string id;
    public bool killchat;
    public int treescut;
    public int oremined;
    public int cactuscut;
    public int woodpickup;
    public int orepickup;
    public int berriespickup;
    public int pumpkinpickup;
    public int potatopickup;
    public int cornpickup;
    public int mushroompickup;
    public int hemppickup;
    public int seedpickup;
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


```

---

## KillRewards by birthdates - Get rewards for getting x amount of kills during 1 life or the entire wipe

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;

Oxide.Plugins
[Info("Kill Rewards", "birthdates", "1.1.2")]
[Description("Get rewards for getting x amount of kills during 1 life or the entire wipe")]
public class KillRewards : RustPlugin
{
    private const string permission_use;
    private void Init();
    private void Unload();
    private void OnEntityDeath(BasePlayer target, HitInfo info);
    private bool TryGetReward(int Kills, KillReward Reward);
    private void ClearKills(string ID);
    private int AddKill(string ID);
    private ConfigFile _config;
    private Data _data;
    protected override void LoadDefaultMessages();
    private class Data
    {
        public readonly Dictionary<string, int> kills;
    }

    private void SaveData();
    public class KillReward
    {
        public List<string> Commands;
        [JsonProperty("Kill Goal")]
        public int Kills;
    }

    public class ConfigFile
    {
        [JsonProperty("One Life?")]
        public bool oneLife;
        [JsonProperty("Rewards")]
        public List<KillReward> rewards;
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class Data
{
    public readonly Dictionary<string, int> kills;
}

public class KillReward
{
    public List<string> Commands;
    [JsonProperty("Kill Goal")]
    public int Kills;
}

public class ConfigFile
{
    [JsonProperty("One Life?")]
    public bool oneLife;
    [JsonProperty("Rewards")]
    public List<KillReward> rewards;
    public static ConfigFile DefaultConfig();
}


```

---

## KillStreaks by  - Keeps track of player kill streaks and either reward or punish them

```csharp
using System.Collections.Generic;
using System;
using UnityEngine;
using Oxide.Core.Plugins;
using Oxide.Core;
using Oxide.Core.Configuration;
using System.Linq;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("Kill Streaks", "Default", "0.1.8")]
[Description("Adds redeemable kilstreak rewards")]
 class KillStreaks : RustPlugin
{
    [PluginReference]
    private Plugin Airstrike;
    private Plugin Clans;
    private Plugin Economics;
    private Plugin EventManager;
    private Plugin Friends;
    private Plugin Kits;
    private Plugin ServerRewards;
    private Plugin AdvAirstrike;
    private Plugin FancyDrop;
    private Plugin ZoneManager;
    private Dictionary<ulong, int> cachedData;
    readonly DynamicConfigFile zdataFile;
    private List<BaseHelicopter> activeHelis;
    private List<ulong> aasGren;
    private List<ulong> sqGren;
    private List<ulong> naGren;
    private List<ulong> nuGren;
    private List<ulong> spGren;
    private List<ulong> asGren;
    private List<ulong> ssGren;
    private List<ulong> arGren;
    private List<ulong> heGren;
    private List<ulong> mrtdm;
    private List<ulong> turret;
    private List<AutoTurret> TurretList;
    private List<string> zones;
    private Dictionary<ulong, StreakType> activeGrenades;
    private Dictionary<int, StreakType> streakTypes;
     DataStorage data;
     Player_DataStorage pdata;
    private DynamicConfigFile KSData;
    private DynamicConfigFile KSPData;
    private static Vector2 warningPos;
    private static Vector2 warningDim;
    private readonly int triggerMask;
    private const string supplydrop_prefab;
    private const string KillStreakPermission;
     void OnServerInitialized();
    private void CheckDependencies();
    protected override void LoadDefaultConfig();
     void OnEntityDeath(BaseEntity entity, HitInfo hitinfo);
     void OnPlayerDisconnected(BasePlayer player);
     void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
     void Unload();
    private bool HasPriv(BasePlayer player);
    private void ProcessKill(BasePlayer player, BasePlayer victim);
    private void ProcessDeath(BasePlayer player, HitInfo info, bool disconnected);
    private void ClearPlayerRewards(BasePlayer player);
    public string GetDeathType(BaseEntity entity);
    private void BroadcastToAll(string msg, string keyword);
    private void BroadcastToPlayer(BasePlayer player, string msg, string keyword);
    private void GUIToPlayer(BasePlayer player, string msg, string keyword);
    private bool IsClanmate(string playerId, string friendId);
    private List<BasePlayer> FindNearbyFriends(BasePlayer player);
    private bool IsFriend(ulong playerID, ulong friendID);
    private Item GiveSmokeSignal();
    private Item GiveSupplySignal();
    private void Deal(BasePlayer player, BasePlayer victim);
    private void GiveRewardGrenade(BasePlayer player, StreakType type);
    private void CallAirstrike(Vector3 target, bool type);
    private void CallAdvAirstrike(Vector3 target, StreakType type);
    private void LaunchArtillery(Vector3 target);
    private void RocketSpread(Vector3 targetPos);
    private BaseEntity CreateRocket(Vector3 targetPos);
    private int HeliDistance;
    private static LayerMask GROUND_MASKS;
    private void CallHeli(Vector3 pos, int streaknum, bool onSmoke, BasePlayer player);
    private BaseEntity CreateHeli(Vector3 pos);
    private Vector3 calculateSpawnPos(Vector3 arenaPos);
    private float RandomRange(float distance, float difference);
    private void MoveEntity(BaseEntity entity, Vector3 pos);
    private void CheckDistance(BaseEntity entity, BasePlayer player);
    static Vector3 GetGroundPosition(Vector3 sourcePos);
    private void KillHeli();
    private void KillTurret();
    private void SetMartyrdom(BasePlayer player);
    private void ChooseRandomExp(Vector3 pos);
    private void dropGrenade(Vector3 deathPos);
    private void dropBeancan(Vector3 deathPos);
    private void dropExplosive(Vector3 deathPos);
    private void SendSupplyDrop(BasePlayer player, int streaknum);
    private void SpawnSignal(BasePlayer player);
    private void DropTurret(Vector3 pos, BasePlayer player);
    private AutoTurret CreateTurret(Vector3 targetPos);
    private void AssignTurretAuth(BasePlayer player, AutoTurret turret);
    private string GiveEconomics(BasePlayer player, int streaknum);
    private string GiveRP(BasePlayer player, int streaknum);
    private string GiveKit(BasePlayer player, string kitname, int streaknum);
    private void dealDamage(Vector3 deathPos, float damage, float radius);
    [ChatCommand("ks")]
     void cmdTarget(BasePlayer player, string command, string[] args);
     bool isAuth(BasePlayer player);
     class KSUI : MonoBehaviour
    {
         int i;
        private BasePlayer player;
         void Awake();
        public static KSUI GetPlayer(BasePlayer player);
        public void UseUI(string msg, Vector2 pos, Vector2 dim, int size);
    }

    private void DestroyNotification(BasePlayer player, string msgNum);
    private void DestroyWarningMsg(BasePlayer player, string msgNum, int duration);
     bool Changed;
    static bool broadcastEnd;
    static bool broadcastMsg;
    static bool ignoreBuildPriv;
    static bool showVictim;
    static bool useAdvAirstrike;
    static bool useAirstrike;
    static bool useClans;
    static bool useFriendsAPI;
    static bool usePermissions;
    static bool useZoneManager;
    static int saveTimer;
    static bool PVPOnly;
    static bool PeopleOnly;
    static bool NoDisconnect;
    static float HeliBulletDamage;
    static float HeliHealth;
    static float MainRotorHealth;
    static float TailRotorHealth;
    static float HeliSpeed;
    static float HeliAccuracy;
    static float SpawnDistance;
    static string fontColor1;
    static string fontColor2;
    static float rocketInterval;
    static float rocketSpread;
    static int rocketAmount;
    static float grenadeRadius;
    static float grenadeDamage;
    static float beancanRadius;
    static float beancanDamage;
    static float explosiveRadius;
    static float explosiveDamage;
    static float nearbyRadius;
    static string turretAmmoTypeName;
    static int turretAmmoCount;
    private void LoadVariables();
    private void LoadConfigVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
     object GetConfig(string menu, string datavalue, object defaultValue);
     void SaveStreakData();
     void SaveData();
     void SaveLoop();
     void LoadData();
     void RegisterMessages();
     class Player_DataStorage
    {
        public Dictionary<ulong, KSPDATA> killStreakData;
        public Player_DataStorage();
    }

     class DataStorage
    {
        public Dictionary<int, Streaks> killStreaks;
        public DataStorage();
    }

     class KSPDATA
    {
        public string Name;
        public int highestKS;
    }

     class Streaks
    {
        public string Message;
        public StreakType StreakType;
        public int Amount;
        public string KitName;
    }

     Dictionary<int, Streaks> ksDefault;
     Dictionary<string, string> messages;
}

 class KSUI : MonoBehaviour
{
     int i;
    private BasePlayer player;
     void Awake();
    public static KSUI GetPlayer(BasePlayer player);
    public void UseUI(string msg, Vector2 pos, Vector2 dim, int size);
}

 class Player_DataStorage
{
    public Dictionary<ulong, KSPDATA> killStreakData;
    public Player_DataStorage();
}

 class DataStorage
{
    public Dictionary<int, Streaks> killStreaks;
    public DataStorage();
}

 class KSPDATA
{
    public string Name;
    public int highestKS;
}

 class Streaks
{
    public string Message;
    public StreakType StreakType;
    public int Amount;
    public string KitName;
}


```

---

## Kits by k1lly0u - Item kits, autokits, kit cooldowns, and more

```csharp
using Facepunch;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Kits", "k1lly0u", "4.4.7"), Description("Create kits containing items that players can redeem")]
 class Kits : RustPlugin
{
    [PluginReference]
    private Plugin CopyPaste;
    private Plugin ImageLibrary;
    private Plugin ServerRewards;
    private Plugin Economics;
    private DateTime _deprecatedHookTime;
    private Hash<ulong, KitData.Kit> _kitCreators;
    private const string ADMIN_PERMISSION;
    private const string BLUEPRINT_BASE;
    private void Loaded();
    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private void OnNewSave(string filename);
    private void OnServerSave();
    private void OnPlayerRespawned(BasePlayer player);
    private void Unload();
    private bool TryClaimKit(BasePlayer player, string name, bool usingUI);
    private object CanClaimKit(BasePlayer player, KitData.Kit kit, bool ignoreAuthCost);
    private object GiveKit(BasePlayer player, KitData.Kit kit);
    private void OnKitReceived(BasePlayer player, KitData.Kit kit);
    private CostType _costType;
    private const int SCRAP_ITEM_ID;
    private bool ChargePlayer(BasePlayer player, int amount);
    private T ParseType(string type);
    private string FormatTime(double time);
    private void GetUserValidKits(BasePlayer player, List<KitData.Kit> list, ulong npcId);
    private bool IsAdmin(BasePlayer player);
    private BasePlayer FindPlayer(string partialNameOrID);
    private BasePlayer RaycastPlayer(BasePlayer player);
    private static DateTime Epoch;
    private static double LastWipeTime;
    private static double CurrentTime { get; set; }
    private void RegisterImage(string name, string url);
    private string GetImage(string name, ulong skinId);
    private void OnUseNPC(BasePlayer npcPlayer, BasePlayer player);
    [HookMethod("isKit")]
    public bool isKit(string name);
    [HookMethod("GetAllKits")]
    public string[] GetAllKits();
    [HookMethod("KitImage")]
    public string KitImage(string name);
    [HookMethod("KitDescription")]
    public string KitDescription(string name);
    [HookMethod("KitMax")]
    public int KitMax(string name);
    [HookMethod("KitCooldown")]
    public double KitCooldown(string name);
    [HookMethod("PlayerKitMax")]
    public int PlayerKitMax(ulong playerId, string name);
    [HookMethod("PlayerKitCooldown")]
    public double PlayerKitCooldown(ulong playerId, string name);
    [HookMethod("GetKitContents")]
    public string[] GetKitContents(string name);
    [HookMethod("GetKitInfo")]
    public object GetKitInfo(string name);
    private void GetItemObject_Old(JArray array, ItemData[] items, string container);
    [HookMethod("GiveKit")]
    public object GiveKit(BasePlayer player, string name);
    [HookMethod("IsKit")]
    public bool IsKit(string name);
    [HookMethod("GetKitNames")]
    public void GetKitNames(List<string> list);
    [HookMethod("GetKitImage")]
    public string GetKitImage(string name);
    [HookMethod("GetKitDescription")]
    public string GetKitDescription(string name);
    [HookMethod("GetKitMaxUses")]
    public int GetKitMaxUses(string name);
    [HookMethod("GetKitCooldown")]
    public int GetKitCooldown(string name);
    [HookMethod("GetPlayerKitUses")]
    public int GetPlayerKitUses(ulong playerId, string name);
    [HookMethod("SetPlayerKitUses")]
    public void SetPlayerKitUses(ulong playerId, string name, int amount);
    [HookMethod("GetPlayerKitCooldown")]
    public double GetPlayerKitCooldown(ulong playerId, string name);
    [HookMethod("SetPlayerKitCooldown")]
    public void SetPlayerCooldown(ulong playerId, string name, double seconds);
    [HookMethod("GetKitObject")]
    public JObject GetKitObject(string name);
    [HookMethod("CreateKitItems")]
    public IEnumerable<Item> CreateKitItems(string name);
    private const string UI_MENU;
    private const string UI_POPUP;
    private const string DEFAULT_ICON;
    private const string MAGNIFY_ICON;
    private void OpenKitGrid(BasePlayer player, int page, ulong npcId);
    private void CreateGridView(BasePlayer player, CuiElementContainer container, int page, ulong npcId);
    private void CreateKitEntry(BasePlayer player, PlayerData.PlayerUsageData playerUsageData, CuiElementContainer container, KitData.Kit kit, int index, int page, ulong npcId);
    private void OpenKitView(BasePlayer player, string name, int page, ulong npcId);
    private const string ICON_BACKGROUND_COLOR;
    private void CreateKitLayout(BasePlayer player, CuiElementContainer container, KitData.Kit kit);
    private void CreateInventoryItems(CuiElementContainer container, GridAlignment alignment, ItemData[] items, int capacity);
    private void OpenKitsEditor(BasePlayer player, bool overwrite);
    private const float EDITOR_ELEMENT_HEIGHT;
    private void AddInputField(CuiElementContainer container, int index, string title, string fieldName, object currentValue, int additionalHeight);
    private void AddTitleSperator(CuiElementContainer container, int index, string title);
    private void AddLabelField(CuiElementContainer container, int index, string title, string value, int additionalHeight);
    private void AddToggleField(CuiElementContainer container, int index, string title, string fieldName, bool currentValue);
    private string GetInputLabel(object obj);
    private float GetVerticalPos(int i, float start);
    private void CreateMenuPopup(BasePlayer player, string text, float duration);
    private readonly GridAlignment KitAlign;
    private readonly GridAlignment MainAlign;
    private readonly GridAlignment WearAlign;
    private readonly GridAlignment BeltAlign;
    private class GridAlignment
    {
        private readonly int _columns;
        private readonly float _xOffset;
        private readonly float _width;
        private readonly float _xSpacing;
        private readonly float _yOffset;
        private readonly float _height;
        private readonly float _ySpacing;
        internal GridAlignment(int columns, float xOffset, float width, float xSpacing, float yOffset, float height, float ySpacing);
        internal UI4 Get(int index);
    }

    [ConsoleCommand("kits.close")]
    private void ccmdKitsClose(ConsoleSystem.Arg arg);
    [ConsoleCommand("kits.gridview")]
    private void ccmdKitsGridView(ConsoleSystem.Arg arg);
    [ConsoleCommand("kits.create")]
    private void ccmdCreateKit(ConsoleSystem.Arg arg);
    [ConsoleCommand("kits.edit")]
    private void ccmdEditKit(ConsoleSystem.Arg arg);
    [ConsoleCommand("kits.savekit")]
    private void ccmdSaveKit(ConsoleSystem.Arg arg);
    [ConsoleCommand("kits.toggleoverwrite")]
    private void ccmdToggleOverwrite(ConsoleSystem.Arg arg);
    [ConsoleCommand("kits.clearitems")]
    private void ccmdClearItems(ConsoleSystem.Arg arg);
    [ConsoleCommand("kits.copyinv")]
    private void ccmdCopyInv(ConsoleSystem.Arg arg);
    [ConsoleCommand("kits.clear")]
    private void ccmdClearField(ConsoleSystem.Arg arg);
    [ConsoleCommand("kits.creator")]
    private void ccmdSetField(ConsoleSystem.Arg arg);
    private void SetParameter(BasePlayer player, KitData.Kit kit, string fieldName, object value);
    private bool TryConvertValue(object value, T result);
    private static string CommandSafe(string text, bool unpack);
    public static class UI
    {
        public static CuiElementContainer Container(string panel, string color, UI4 dimensions, bool blur, string parent);
        public static CuiElementContainer Popup(string panel, string text, int size, UI4 dimensions, TextAnchor align, string parent);
        public static void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions);
        public static void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
        public static void Button(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, string command, TextAnchor align);
        public static void Button(CuiElementContainer container, string panel, string color, string png, UI4 dimensions, string command);
        public static void Input(CuiElementContainer container, string panel, string text, int size, string command, UI4 dimensions, TextAnchor anchor);
        public static void Image(CuiElementContainer container, string panel, string png, UI4 dimensions);
        public static void Image(CuiElementContainer container, string panel, int itemId, ulong skinId, UI4 dimensions);
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

    private void cmdKit(BasePlayer player, string command, string[] args);
    private void ccmdKit(ConsoleSystem.Arg arg);
    private void ReplyHelp(BasePlayer player);
    [ConsoleCommand("kits.convertolddata")]
    private void ccmdConvertKitsData(ConsoleSystem.Arg arg);
    [ConsoleCommand("kits.convertoldplayerdata")]
    private void ccmdConvertPlayerData(ConsoleSystem.Arg arg);
    private void ConvertOldPlayerData();
    private void ConvertOldKitData();
    private void TryConvertItems(KitData.Kit kit, List<OldKitItem> items);
    private ItemDefinition FindItemDefinition(int itemID);
    private void ConvertItems(List<ItemData> list, IEnumerable<OldKitItem> items);
    private class OldStoredData
    {
        public Dictionary<string, OldKit> Kits;
    }

    private class OldKitData
    {
        public int max;
        public double cooldown;
    }

    private class OldKitItem
    {
        public int itemid;
        public string container;
        public int amount;
        public ulong skinid;
        public bool weapon;
        public int blueprintTarget;
        public List<int> mods;
    }

    private class OldKit
    {
        public string name;
        public string description;
        public int max;
        public double cooldown;
        public int authlevel;
        public bool hide;
        public bool npconly;
        public string permission;
        public string image;
        public string building;
        public List<OldKitItem> items;
    }

    private readonly Dictionary<int, string> _itemIdShortnameConversions;
    private void CheckForShortnameUpdates();
    private readonly Dictionary<string, string> _itemShortnameReplacements;
    private static ConfigData Configuration;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Kit chat command")]
        public string Command { get; set; }
        [JsonProperty(PropertyName = "Currency used for purchase costs (Scrap, Economics, ServerRewards)")]
        public string Currency { get; set; }
        [JsonProperty(PropertyName = "Log kits given")]
        public bool LogKitsGiven { get; set; }
        [JsonProperty(PropertyName = "Wipe player data when the server is wiped")]
        public bool WipeData { get; set; }
        [JsonProperty(PropertyName = "Use the Kits UI menu")]
        public bool UseUI { get; set; }
        [JsonProperty(PropertyName = "Allow players to toggle auto-kits on spawn")]
        public bool AllowAutoToggle { get; set; }
        [JsonProperty(PropertyName = "Show kits with permissions assigned to players without the permission")]
        public bool ShowPermissionKits { get; set; }
        [JsonProperty(PropertyName = "Players with the admin permission ignore usage restrictions")]
        public bool AdminIgnoreRestrictions { get; set; }
        [JsonProperty(PropertyName = "Autokits ordered by priority")]
        public List<string> AutoKits { get; set; }
        [JsonProperty(PropertyName = "Post wipe cooldowns (kit name | seconds)")]
        public Hash<string, int> WipeCooldowns { get; set; }
        [JsonProperty(PropertyName = "Parameters used when pasting a building via CopyPaste")]
        public string[] CopyPasteParams { get; set; }
        [JsonProperty(PropertyName = "UI Options")]
        public MenuOptions Menu { get; set; }
        [JsonProperty(PropertyName = "Kit menu items when opened via HumanNPC (NPC user ID | Items)")]
        public Hash<ulong, NPCKit> NPCKitMenu { get; set; }
        public class MenuOptions
        {
            [JsonProperty(PropertyName = "Panel Color")]
            public UIColor Panel { get; set; }
            [JsonProperty(PropertyName = "Disabled Color")]
            public UIColor Disabled { get; set; }
            [JsonProperty(PropertyName = "Color 1")]
            public UIColor Color1 { get; set; }
            [JsonProperty(PropertyName = "Color 2")]
            public UIColor Color2 { get; set; }
            [JsonProperty(PropertyName = "Color 3")]
            public UIColor Color3 { get; set; }
            [JsonProperty(PropertyName = "Color 4")]
            public UIColor Color4 { get; set; }
            [JsonProperty(PropertyName = "Default kit image URL")]
            public string DefaultKitURL { get; set; }
            [JsonProperty(PropertyName = "View kit icon URL")]
            public string MagnifyIconURL { get; set; }
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

        public class NPCKit
        {
            [JsonProperty(PropertyName = "The list of kits that can be claimed from this NPC")]
            public List<string> Kits { get; set; }
            [JsonProperty(PropertyName = "The NPC's response to opening their kit menu")]
            public string Description { get; set; }
        }

        public VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private KitData kitData;
    private PlayerData playerData;
    private DynamicConfigFile kitdata;
    private DynamicConfigFile playerdata;
    private void SaveKitData();
    private void SavePlayerData();
    private void LoadData();
    private class KitData
    {
        [JsonProperty]
        private Dictionary<string, Kit> _kits;
        internal int Count { get; set; }
        internal bool Find(string name, Kit kit);
        internal bool Exists(string name);
        internal ICollection<string> Keys { get; set; }
        internal ICollection<Kit> Values { get; set; }
        internal void ForEach(Action<Kit> action);
        internal void RegisterPermissions(Permission permission, Plugin plugin);
        internal void RegisterImages(Plugin plugin);
        internal bool IsOnWipeCooldown(int seconds, int remaining);
        internal void Remove(Kit kit);
        internal bool IsValid { get; set; }
        public class Kit
        {
            public string Name { get; set; }
            public string Description { get; set; }
            public string RequiredPermission { get; set; }
            public int MaximumUses { get; set; }
            public int RequiredAuth { get; set; }
            public int Cooldown { get; set; }
            public int Cost { get; set; }
            public bool IsHidden { get; set; }
            public string CopyPasteFile { get; set; }
            public string KitImage { get; set; }
            public ItemData[] MainItems { get; set; }
            public ItemData[] WearItems { get; set; }
            public ItemData[] BeltItems { get; set; }
            [JsonIgnore]
            internal int ItemCount { get; set; }
            [JsonIgnore]
            private JObject _jObject;
            [JsonIgnore]
            internal JObject ToJObject { get; set; }
            internal static Kit CloneOf(Kit other);
            internal ItemData GetBackpackSlot();
            internal bool HasSpaceForItems(BasePlayer player);
            internal void GiveItemsTo(BasePlayer player);
            private void GiveItems(ItemData[] items, ItemContainer container, List<ItemData> leftOverItems, bool isWearContainer);
            internal IEnumerable<Item> CreateItems();
            private bool MoveToIdealContainer(PlayerInventory playerInventory, Item item);
            private bool CanWearItem(ItemContainer containerWear, Item item);
            internal void CopyItemsFrom(BasePlayer player);
            private void CopyItems(ItemData[] array, ItemContainer container, int limit);
            internal void ClearItems();
        }

    }

    private class PlayerData
    {
        [JsonProperty]
        private Dictionary<ulong, PlayerUsageData> _players;
        internal bool Find(ulong playerId, PlayerUsageData playerUsageData);
        internal bool Exists(ulong playerId);
        internal void OnKitClaimed(BasePlayer player, KitData.Kit kit);
        internal bool ClearKitUses(BasePlayer player, KitData.Kit kit);
        internal void Wipe();
        internal bool IsValid { get; set; }
        public class PlayerUsageData
        {
            [JsonProperty]
            private Hash<string, KitUsageData> _usageData;
            public bool ClaimAutoKits { get; set; }
            internal double GetCooldownRemaining(string name);
            internal void SetCooldownRemaining(string name, double seconds);
            internal int GetKitUses(string name);
            internal void SetKitUses(string name, int amount);
            internal void ClearKitUses(string name);
            internal void OnKitClaimed(KitData.Kit kit);
            internal void InsertOldData(string name, int totalUses, double nextUse);
            public class KitUsageData
            {
                public int TotalUses { get; set; }
                public double NextUseTime { get; set; }
                internal void OnKitClaimed(int cooldownSeconds);
            }

        }

    }

    private static Item CreateItem(ItemData itemData);
    private static T FindItemMod(Item item);
    public class ItemData
    {
        public string Shortname { get; set; }
        public string DisplayName { get; set; }
        public ulong Skin { get; set; }
        public int Amount { get; set; }
        public float Condition { get; set; }
        public float MaxCondition { get; set; }
        public int Ammo { get; set; }
        public string Ammotype { get; set; }
        public int Position { get; set; }
        public int Frequency { get; set; }
        public string BlueprintShortname { get; set; }
        public string Text { get; set; }
        public ItemData[] Contents { get; set; }
        public ItemContainer Container { get; set; }
        [JsonIgnore]
        private int _itemId;
        [JsonIgnore]
        private int _blueprintItemId;
        [JsonIgnore]
        private JObject _jObject;
        [JsonIgnore]
        internal int ItemID { get; set; }
        [JsonIgnore]
        internal bool IsBlueprint { get; set; }
        [JsonIgnore]
        internal int BlueprintItemID { get; set; }
        [JsonIgnore]
        internal JObject ToJObject { get; set; }
        internal ItemData();
        internal ItemData(Item item);
        public class InstanceData
        {
            public int DataInt { get; set; }
            public int BlueprintTarget { get; set; }
            public int BlueprintAmount { get; set; }
            public uint SubEntityNetID { get; set; }
            internal InstanceData();
            internal InstanceData(Item item);
            internal void Restore(Item item);
            internal bool IsValid { get; set; }
        }

        public class ItemContainer
        {
            public int slots;
            public float temperature;
            public int flags;
            public int allowedContents;
            public int maxStackSize;
            public List<int> allowedItems;
            public List<int> availableSlots;
            public int volume;
            public List<ItemData> contents;
            public ItemContainer();
            public ItemContainer(global::ItemContainer container);
            public void Load(global::ItemContainer itemContainer);
        }

    }

    private string Message(string key, ulong playerId);
    private readonly Dictionary<string, string> Messages;
}

private class GridAlignment
{
    private readonly int _columns;
    private readonly float _xOffset;
    private readonly float _width;
    private readonly float _xSpacing;
    private readonly float _yOffset;
    private readonly float _height;
    private readonly float _ySpacing;
    internal GridAlignment(int columns, float xOffset, float width, float xSpacing, float yOffset, float height, float ySpacing);
    internal UI4 Get(int index);
}

public static class UI
{
    public static CuiElementContainer Container(string panel, string color, UI4 dimensions, bool blur, string parent);
    public static CuiElementContainer Popup(string panel, string text, int size, UI4 dimensions, TextAnchor align, string parent);
    public static void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions);
    public static void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
    public static void Button(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, string command, TextAnchor align);
    public static void Button(CuiElementContainer container, string panel, string color, string png, UI4 dimensions, string command);
    public static void Input(CuiElementContainer container, string panel, string text, int size, string command, UI4 dimensions, TextAnchor anchor);
    public static void Image(CuiElementContainer container, string panel, string png, UI4 dimensions);
    public static void Image(CuiElementContainer container, string panel, int itemId, ulong skinId, UI4 dimensions);
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

private class OldStoredData
{
    public Dictionary<string, OldKit> Kits;
}

private class OldKitData
{
    public int max;
    public double cooldown;
}

private class OldKitItem
{
    public int itemid;
    public string container;
    public int amount;
    public ulong skinid;
    public bool weapon;
    public int blueprintTarget;
    public List<int> mods;
}

private class OldKit
{
    public string name;
    public string description;
    public int max;
    public double cooldown;
    public int authlevel;
    public bool hide;
    public bool npconly;
    public string permission;
    public string image;
    public string building;
    public List<OldKitItem> items;
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Kit chat command")]
    public string Command { get; set; }
    [JsonProperty(PropertyName = "Currency used for purchase costs (Scrap, Economics, ServerRewards)")]
    public string Currency { get; set; }
    [JsonProperty(PropertyName = "Log kits given")]
    public bool LogKitsGiven { get; set; }
    [JsonProperty(PropertyName = "Wipe player data when the server is wiped")]
    public bool WipeData { get; set; }
    [JsonProperty(PropertyName = "Use the Kits UI menu")]
    public bool UseUI { get; set; }
    [JsonProperty(PropertyName = "Allow players to toggle auto-kits on spawn")]
    public bool AllowAutoToggle { get; set; }
    [JsonProperty(PropertyName = "Show kits with permissions assigned to players without the permission")]
    public bool ShowPermissionKits { get; set; }
    [JsonProperty(PropertyName = "Players with the admin permission ignore usage restrictions")]
    public bool AdminIgnoreRestrictions { get; set; }
    [JsonProperty(PropertyName = "Autokits ordered by priority")]
    public List<string> AutoKits { get; set; }
    [JsonProperty(PropertyName = "Post wipe cooldowns (kit name | seconds)")]
    public Hash<string, int> WipeCooldowns { get; set; }
    [JsonProperty(PropertyName = "Parameters used when pasting a building via CopyPaste")]
    public string[] CopyPasteParams { get; set; }
    [JsonProperty(PropertyName = "UI Options")]
    public MenuOptions Menu { get; set; }
    [JsonProperty(PropertyName = "Kit menu items when opened via HumanNPC (NPC user ID | Items)")]
    public Hash<ulong, NPCKit> NPCKitMenu { get; set; }
    public class MenuOptions
    {
        [JsonProperty(PropertyName = "Panel Color")]
        public UIColor Panel { get; set; }
        [JsonProperty(PropertyName = "Disabled Color")]
        public UIColor Disabled { get; set; }
        [JsonProperty(PropertyName = "Color 1")]
        public UIColor Color1 { get; set; }
        [JsonProperty(PropertyName = "Color 2")]
        public UIColor Color2 { get; set; }
        [JsonProperty(PropertyName = "Color 3")]
        public UIColor Color3 { get; set; }
        [JsonProperty(PropertyName = "Color 4")]
        public UIColor Color4 { get; set; }
        [JsonProperty(PropertyName = "Default kit image URL")]
        public string DefaultKitURL { get; set; }
        [JsonProperty(PropertyName = "View kit icon URL")]
        public string MagnifyIconURL { get; set; }
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

    public class NPCKit
    {
        [JsonProperty(PropertyName = "The list of kits that can be claimed from this NPC")]
        public List<string> Kits { get; set; }
        [JsonProperty(PropertyName = "The NPC's response to opening their kit menu")]
        public string Description { get; set; }
    }

    public VersionNumber Version { get; set; }
}

public class MenuOptions
{
    [JsonProperty(PropertyName = "Panel Color")]
    public UIColor Panel { get; set; }
    [JsonProperty(PropertyName = "Disabled Color")]
    public UIColor Disabled { get; set; }
    [JsonProperty(PropertyName = "Color 1")]
    public UIColor Color1 { get; set; }
    [JsonProperty(PropertyName = "Color 2")]
    public UIColor Color2 { get; set; }
    [JsonProperty(PropertyName = "Color 3")]
    public UIColor Color3 { get; set; }
    [JsonProperty(PropertyName = "Color 4")]
    public UIColor Color4 { get; set; }
    [JsonProperty(PropertyName = "Default kit image URL")]
    public string DefaultKitURL { get; set; }
    [JsonProperty(PropertyName = "View kit icon URL")]
    public string MagnifyIconURL { get; set; }
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

public class NPCKit
{
    [JsonProperty(PropertyName = "The list of kits that can be claimed from this NPC")]
    public List<string> Kits { get; set; }
    [JsonProperty(PropertyName = "The NPC's response to opening their kit menu")]
    public string Description { get; set; }
}

private class KitData
{
    [JsonProperty]
    private Dictionary<string, Kit> _kits;
    internal int Count { get; set; }
    internal bool Find(string name, Kit kit);
    internal bool Exists(string name);
    internal ICollection<string> Keys { get; set; }
    internal ICollection<Kit> Values { get; set; }
    internal void ForEach(Action<Kit> action);
    internal void RegisterPermissions(Permission permission, Plugin plugin);
    internal void RegisterImages(Plugin plugin);
    internal bool IsOnWipeCooldown(int seconds, int remaining);
    internal void Remove(Kit kit);
    internal bool IsValid { get; set; }
    public class Kit
    {
        public string Name { get; set; }
        public string Description { get; set; }
        public string RequiredPermission { get; set; }
        public int MaximumUses { get; set; }
        public int RequiredAuth { get; set; }
        public int Cooldown { get; set; }
        public int Cost { get; set; }
        public bool IsHidden { get; set; }
        public string CopyPasteFile { get; set; }
        public string KitImage { get; set; }
        public ItemData[] MainItems { get; set; }
        public ItemData[] WearItems { get; set; }
        public ItemData[] BeltItems { get; set; }
        [JsonIgnore]
        internal int ItemCount { get; set; }
        [JsonIgnore]
        private JObject _jObject;
        [JsonIgnore]
        internal JObject ToJObject { get; set; }
        internal static Kit CloneOf(Kit other);
        internal ItemData GetBackpackSlot();
        internal bool HasSpaceForItems(BasePlayer player);
        internal void GiveItemsTo(BasePlayer player);
        private void GiveItems(ItemData[] items, ItemContainer container, List<ItemData> leftOverItems, bool isWearContainer);
        internal IEnumerable<Item> CreateItems();
        private bool MoveToIdealContainer(PlayerInventory playerInventory, Item item);
        private bool CanWearItem(ItemContainer containerWear, Item item);
        internal void CopyItemsFrom(BasePlayer player);
        private void CopyItems(ItemData[] array, ItemContainer container, int limit);
        internal void ClearItems();
    }

}

public class Kit
{
    public string Name { get; set; }
    public string Description { get; set; }
    public string RequiredPermission { get; set; }
    public int MaximumUses { get; set; }
    public int RequiredAuth { get; set; }
    public int Cooldown { get; set; }
    public int Cost { get; set; }
    public bool IsHidden { get; set; }
    public string CopyPasteFile { get; set; }
    public string KitImage { get; set; }
    public ItemData[] MainItems { get; set; }
    public ItemData[] WearItems { get; set; }
    public ItemData[] BeltItems { get; set; }
    [JsonIgnore]
    internal int ItemCount { get; set; }
    [JsonIgnore]
    private JObject _jObject;
    [JsonIgnore]
    internal JObject ToJObject { get; set; }
    internal static Kit CloneOf(Kit other);
    internal ItemData GetBackpackSlot();
    internal bool HasSpaceForItems(BasePlayer player);
    internal void GiveItemsTo(BasePlayer player);
    private void GiveItems(ItemData[] items, ItemContainer container, List<ItemData> leftOverItems, bool isWearContainer);
    internal IEnumerable<Item> CreateItems();
    private bool MoveToIdealContainer(PlayerInventory playerInventory, Item item);
    private bool CanWearItem(ItemContainer containerWear, Item item);
    internal void CopyItemsFrom(BasePlayer player);
    private void CopyItems(ItemData[] array, ItemContainer container, int limit);
    internal void ClearItems();
}

private class PlayerData
{
    [JsonProperty]
    private Dictionary<ulong, PlayerUsageData> _players;
    internal bool Find(ulong playerId, PlayerUsageData playerUsageData);
    internal bool Exists(ulong playerId);
    internal void OnKitClaimed(BasePlayer player, KitData.Kit kit);
    internal bool ClearKitUses(BasePlayer player, KitData.Kit kit);
    internal void Wipe();
    internal bool IsValid { get; set; }
    public class PlayerUsageData
    {
        [JsonProperty]
        private Hash<string, KitUsageData> _usageData;
        public bool ClaimAutoKits { get; set; }
        internal double GetCooldownRemaining(string name);
        internal void SetCooldownRemaining(string name, double seconds);
        internal int GetKitUses(string name);
        internal void SetKitUses(string name, int amount);
        internal void ClearKitUses(string name);
        internal void OnKitClaimed(KitData.Kit kit);
        internal void InsertOldData(string name, int totalUses, double nextUse);
        public class KitUsageData
        {
            public int TotalUses { get; set; }
            public double NextUseTime { get; set; }
            internal void OnKitClaimed(int cooldownSeconds);
        }

    }

}

public class PlayerUsageData
{
    [JsonProperty]
    private Hash<string, KitUsageData> _usageData;
    public bool ClaimAutoKits { get; set; }
    internal double GetCooldownRemaining(string name);
    internal void SetCooldownRemaining(string name, double seconds);
    internal int GetKitUses(string name);
    internal void SetKitUses(string name, int amount);
    internal void ClearKitUses(string name);
    internal void OnKitClaimed(KitData.Kit kit);
    internal void InsertOldData(string name, int totalUses, double nextUse);
    public class KitUsageData
    {
        public int TotalUses { get; set; }
        public double NextUseTime { get; set; }
        internal void OnKitClaimed(int cooldownSeconds);
    }

}

public class KitUsageData
{
    public int TotalUses { get; set; }
    public double NextUseTime { get; set; }
    internal void OnKitClaimed(int cooldownSeconds);
}

public class ItemData
{
    public string Shortname { get; set; }
    public string DisplayName { get; set; }
    public ulong Skin { get; set; }
    public int Amount { get; set; }
    public float Condition { get; set; }
    public float MaxCondition { get; set; }
    public int Ammo { get; set; }
    public string Ammotype { get; set; }
    public int Position { get; set; }
    public int Frequency { get; set; }
    public string BlueprintShortname { get; set; }
    public string Text { get; set; }
    public ItemData[] Contents { get; set; }
    public ItemContainer Container { get; set; }
    [JsonIgnore]
    private int _itemId;
    [JsonIgnore]
    private int _blueprintItemId;
    [JsonIgnore]
    private JObject _jObject;
    [JsonIgnore]
    internal int ItemID { get; set; }
    [JsonIgnore]
    internal bool IsBlueprint { get; set; }
    [JsonIgnore]
    internal int BlueprintItemID { get; set; }
    [JsonIgnore]
    internal JObject ToJObject { get; set; }
    internal ItemData();
    internal ItemData(Item item);
    public class InstanceData
    {
        public int DataInt { get; set; }
        public int BlueprintTarget { get; set; }
        public int BlueprintAmount { get; set; }
        public uint SubEntityNetID { get; set; }
        internal InstanceData();
        internal InstanceData(Item item);
        internal void Restore(Item item);
        internal bool IsValid { get; set; }
    }

    public class ItemContainer
    {
        public int slots;
        public float temperature;
        public int flags;
        public int allowedContents;
        public int maxStackSize;
        public List<int> allowedItems;
        public List<int> availableSlots;
        public int volume;
        public List<ItemData> contents;
        public ItemContainer();
        public ItemContainer(global::ItemContainer container);
        public void Load(global::ItemContainer itemContainer);
    }

}

public class InstanceData
{
    public int DataInt { get; set; }
    public int BlueprintTarget { get; set; }
    public int BlueprintAmount { get; set; }
    public uint SubEntityNetID { get; set; }
    internal InstanceData();
    internal InstanceData(Item item);
    internal void Restore(Item item);
    internal bool IsValid { get; set; }
}

public class ItemContainer
{
    public int slots;
    public float temperature;
    public int flags;
    public int allowedContents;
    public int maxStackSize;
    public List<int> allowedItems;
    public List<int> availableSlots;
    public int volume;
    public List<ItemData> contents;
    public ItemContainer();
    public ItemContainer(global::ItemContainer container);
    public void Load(global::ItemContainer itemContainer);
}


```

---

## KnockKnock by MisterPixie - Allows players to set a message on a door, so when someone comes knocking they will get a message

```csharp
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Knock Knock", "MisterPixie", "1.1.13")]
[Description("Allows users to set messages on doors when someone knocks")]
 class KnockKnock : RustPlugin
{
    public string KnockPerm;
    public string KnockAutoPerm;
     List<string> doors;
     Dictionary<ulong, KnockData> knockData;
    private class KnockData
    {
        public bool EnableAutoMessages { get; set; }
        public string AutoMessage { get; set; }
        public Dictionary<int, KnockMessages> knockMessages { get; set; }
    }

    private class KnockMessages
    {
        public string Message { get; set; }
    }

     void SaveData();
     void OnServerSave();
    private string Lang(string key, string id, object[] args);
    private void LoadDefaultMessages();
    private void Init();
     void Unload();
     void OnNewSave();
     void OnEntityBuilt(Planner plan, GameObject go);
    private void KnockCommand(BasePlayer player, string command, string[] args);
    private void OnDoorKnocked(Door door, BasePlayer player);
    private void KnockAdd(BasePlayer player, string[] args, int id, KnockData value);
    private void KnockRemove(BasePlayer player, string[] args, int id, KnockData value);
    private void KnockAuto(BasePlayer player, string[] args, KnockData value);
    private BaseEntity GetRaycast(BasePlayer player);
    private class Permissions
    {
        [JsonProperty(PropertyName = "Permission to use Auto Message Function")]
        public bool UseAutoPerm;
        [JsonProperty(PropertyName = "Permission to use Main Plugin Functions")]
        public bool UsePermission;
    }

    private class Prefixs
    {
        [JsonProperty(PropertyName = "Message Prefix")]
        public string Prefix;
        [JsonProperty(PropertyName = "Use Message Prefix")]
        public bool UsePerfix;
    }

    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Permission Settings")]
        public Permissions permissions;
        [JsonProperty(PropertyName = "Prefix Settings")]
        public Prefixs prefixs;
        [JsonProperty(PropertyName = "Enable Auto Function")]
        public bool EnableAuto;
        public string Command;
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
}

private class KnockData
{
    public bool EnableAutoMessages { get; set; }
    public string AutoMessage { get; set; }
    public Dictionary<int, KnockMessages> knockMessages { get; set; }
}

private class KnockMessages
{
    public string Message { get; set; }
}

private class Permissions
{
    [JsonProperty(PropertyName = "Permission to use Auto Message Function")]
    public bool UseAutoPerm;
    [JsonProperty(PropertyName = "Permission to use Main Plugin Functions")]
    public bool UsePermission;
}

private class Prefixs
{
    [JsonProperty(PropertyName = "Message Prefix")]
    public string Prefix;
    [JsonProperty(PropertyName = "Use Message Prefix")]
    public bool UsePerfix;
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Permission Settings")]
    public Permissions permissions;
    [JsonProperty(PropertyName = "Prefix Settings")]
    public Prefixs prefixs;
    [JsonProperty(PropertyName = "Enable Auto Function")]
    public bool EnableAuto;
    public string Command;
}


```

---

## KnockOpen by OfficerJAKE - Opens security doors when knocked on by players holding the correct keycards

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using System;
using UnityEngine;

Oxide.Plugins
[Info("Knock Open", "OfficerJAKE", "2.5.8")]
[Description("Opens security doors when knocked on by players and holding the correct card")]
internal class KnockOpen : RustPlugin
{
    public static string eff;
    public static Vector3 effTarget;
    public static string cardColor;
    public static string doorColor;
    public static string logMsg;
     Configuration config;
     class Configuration
    {
        [JsonProperty(PropertyName = "Settings")]
        public Settings settings;
        [JsonProperty(PropertyName = "Tuners")]
        public Tuners tuners;
        public class Settings
        {
            [JsonProperty(PropertyName = "Post To Chat")]
            public bool PostToChat;
            [JsonProperty(PropertyName = "Damage Keycards")]
            public bool DamageKeycards;
            [JsonProperty(PropertyName = "Use Effects")]
            public bool UseEffects;
            [JsonProperty(PropertyName = "Close Doors")]
            public bool CloseDoors;
            [JsonProperty(PropertyName = "Hurt On Shock")]
            public bool HurtOnShock;
        }

        public class Tuners
        {
            [JsonProperty(PropertyName = "Close Door Delay")]
            public float CloseDoorDelay;
            [JsonProperty(PropertyName = "Damage To Deal")]
            public float DamageToDeal;
            [JsonProperty(PropertyName = "Hurt Amount")]
            public float HurtAmount;
            [JsonProperty(PropertyName = "Effect On Success")]
            public string EffectSuccess;
            [JsonProperty(PropertyName = "Effect On Failure")]
            public string EffectFailure;
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
     void OnDoorKnocked(Door door, BasePlayer player);
    private void ToggleDoor(BasePlayer player, Door door, string eff, string doorColor, string cardColor, Item card);
    private void DamageCards(BasePlayer player, Door door, string eff, string doorColor, string cardColor, Item card);
    private void DoEffect(BasePlayer player, Door door, Item card, string eff);
    private void Logging(BasePlayer player, string logMsg);
    private string Lang(string key, string id, object[] args);
}

 class Configuration
{
    [JsonProperty(PropertyName = "Settings")]
    public Settings settings;
    [JsonProperty(PropertyName = "Tuners")]
    public Tuners tuners;
    public class Settings
    {
        [JsonProperty(PropertyName = "Post To Chat")]
        public bool PostToChat;
        [JsonProperty(PropertyName = "Damage Keycards")]
        public bool DamageKeycards;
        [JsonProperty(PropertyName = "Use Effects")]
        public bool UseEffects;
        [JsonProperty(PropertyName = "Close Doors")]
        public bool CloseDoors;
        [JsonProperty(PropertyName = "Hurt On Shock")]
        public bool HurtOnShock;
    }

    public class Tuners
    {
        [JsonProperty(PropertyName = "Close Door Delay")]
        public float CloseDoorDelay;
        [JsonProperty(PropertyName = "Damage To Deal")]
        public float DamageToDeal;
        [JsonProperty(PropertyName = "Hurt Amount")]
        public float HurtAmount;
        [JsonProperty(PropertyName = "Effect On Success")]
        public string EffectSuccess;
        [JsonProperty(PropertyName = "Effect On Failure")]
        public string EffectFailure;
    }

}

public class Settings
{
    [JsonProperty(PropertyName = "Post To Chat")]
    public bool PostToChat;
    [JsonProperty(PropertyName = "Damage Keycards")]
    public bool DamageKeycards;
    [JsonProperty(PropertyName = "Use Effects")]
    public bool UseEffects;
    [JsonProperty(PropertyName = "Close Doors")]
    public bool CloseDoors;
    [JsonProperty(PropertyName = "Hurt On Shock")]
    public bool HurtOnShock;
}

public class Tuners
{
    [JsonProperty(PropertyName = "Close Door Delay")]
    public float CloseDoorDelay;
    [JsonProperty(PropertyName = "Damage To Deal")]
    public float DamageToDeal;
    [JsonProperty(PropertyName = "Hurt Amount")]
    public float HurtAmount;
    [JsonProperty(PropertyName = "Effect On Success")]
    public string EffectSuccess;
    [JsonProperty(PropertyName = "Effect On Failure")]
    public string EffectFailure;
}


```

---

## LandLord by bravoavo - Game modification that brings new rules to the original game

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("LandLord", "bravoavo", "1.0.17")]
[Description("Take control of the map")]
 class LandLord : RustPlugin
{
    [PluginReference]
     Plugin ZoneManager;
     Plugin Clans;
     ConfigData configData;
    private DynamicConfigFile data;
    readonly uint bannerprefabid;
    readonly float size;
    readonly bool debug;
     int notrespassgather;
     int onlyconnected;
     int gatherratio;
     int graycircles;
    readonly string zonenameprefix;
    readonly string datafile_name;
    private const string permUse;
    readonly Color[] colorArray;
    private Color colors;
    public Dictionary<string, int> currentsettings;
    public Dictionary<ulong, HashSet<int[]>> flagi;
    public Dictionary<ulong, List<string>> quadrants;
    public Dictionary<string, Vector3> poles;
    public Dictionary<ulong, int> gatherMultiplier;
    public Dictionary<string, ulong> teamList;
    public Dictionary<string, MapMarkerGenericRadius> allmarkers;
    public Dictionary<string, List<MapMarkerGenericRadius>> flagmarkers;
    public List<string> blockedItemsList { get; set; }
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
    private void MyZonesInit();
    private void OnServerInitialized();
     void OnPlayerSleepEnded(BasePlayer player);
    private void GenarateFlag(ulong playerId);
     void OnEntityKill(BaseEntity entity);
     void OnEntityBuilt(Planner plan, GameObject go);
     void OnCollectiblePickup(CollectibleEntity collectible, BasePlayer player);
     object OnDispenserGather(ResourceDispenser dispenser, BasePlayer player, Item item);
     object CanBuild(Planner planner, Construction prefab, Construction.Target target);
    private void OnClanCreate(string tag);
    private void OnClanDestroy(string tag);
    private void DeleteMarkerFromMap(MapMarkerGenericRadius marker);
    private void SpawnMarkerOnMap(Vector3 position, string curentZoneId);
    private void SpawnFlag(Vector3 position, string curentZoneId, ulong playerId);
     class ConfigData
    {
        public Dictionary<ulong, HashSet<int[]>> FlagiData;
        public Dictionary<ulong, List<string>> QuadrantsData;
        public Dictionary<string, Vector3> PolesLocationData;
        public Dictionary<ulong, int> GatherRateData;
        public Dictionary<string, ulong> TeamListData;
        public Dictionary<string, int> LandlordSettings;
        public List<string> BlockedItems;
    }

    private void LoadData();
    private void SaveData();
    private JObject GetClan(string tag);
    [ChatCommand("lord")]
    private void CmdChatLordStats(BasePlayer player, string command, string[] args);
    [ChatCommand("lordadmin")]
     void LordAdminCommand(BasePlayer player, string command, string[] args);
    private string GetLordZoneId(string[] zids);
    private static string GetGrid(Vector3 position);
    private bool CheckConnected(string zid, ulong teampid);
}

 class ConfigData
{
    public Dictionary<ulong, HashSet<int[]>> FlagiData;
    public Dictionary<ulong, List<string>> QuadrantsData;
    public Dictionary<string, Vector3> PolesLocationData;
    public Dictionary<ulong, int> GatherRateData;
    public Dictionary<string, ulong> TeamListData;
    public Dictionary<string, int> LandlordSettings;
    public List<string> BlockedItems;
}


```

---

## LandOnCargoShip by  - Allows the MiniCopter/ScrapHelicopter to land on the cargo ship.

```csharp
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Land On Cargo Ship", "Arainrr", "1.0.0")]
[Description("Allow the mini copter to land on the cargo ship")]
public class LandOnCargoShip : RustPlugin
{
    private void Init();
    private void OnServerInitialized();
    private void OnEntitySpawned(MiniCopter miniCopter);
    private void OnEntitySpawned(CargoShip cargoShip);
    private void Unload();
}


```

---

## LaptopCrateHack by VisEntities - Hack locked crates using targeting computers

```csharp
using Network;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Laptop Crate Hack", "VisEntities", "2.0.2")]
[Description("Hack locked crates using targeting computers.")]
public class LaptopCrateHack : RustPlugin
{
    [PluginReference]
    private readonly Plugin Clans;
    private readonly Plugin Friends;
    private static LaptopCrateHack _plugin;
    private static Configuration _config;
    private const int ITEM_ID_COMPUTER;
    private const string FX_CODE_LOCK_SHOCK;
    private Dictionary<ulong, DateTime> _playerLastHackTimes;
    private class Configuration
    {
        [JsonProperty("Version")]
        public string Version { get; set; }
        [JsonProperty("Required Targeting Computers For Hack")]
        public int RequiredTargetingComputersForHack { get; set; }
        [JsonProperty("Consume Targeting Computer On Hack")]
        public bool ConsumeTargetingComputerOnHack { get; set; }
        [JsonProperty("Crate Unlock Time Seconds")]
        public float CrateUnlockTimeSeconds { get; set; }
        [JsonProperty("Cooldown Between Hacks Seconds")]
        public float CooldownBetweenHacksSeconds { get; set; }
        [JsonProperty("Crate Lootable By Hacker Only")]
        public bool CrateLootableByHackerOnly { get; set; }
        [JsonProperty("Can Be Looted By Hacker Teammates")]
        public bool CanBeLootedByHackerTeammates { get; set; }
        [JsonProperty("Can Be Looted By Hacker Friends")]
        public bool CanBeLootedByHackerFriends { get; set; }
        [JsonProperty("Can Be Looted By Hacker Clanmates")]
        public bool CanBeLootedByHackerClanmates { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfig();
    private Configuration GetDefaultConfig();
    private void Init();
    private void Unload();
    private object CanHackCrate(BasePlayer player, HackableLockedCrate crate);
    private void OnCrateHack(HackableLockedCrate crate);
    private object CanLootEntity(BasePlayer player, HackableLockedCrate crate);
    private static class ClanUtil
    {
        public static bool AreClanmates(ulong firstPlayerId, ulong secondPlayerId);
    }

    private static class FriendUtil
    {
        public static bool AreFriends(ulong firstPlayerId, ulong secondPlayerId);
    }

    private void OnCrateHackFailed(HackableLockedCrate crate);
    public static bool VerifyPluginBeingLoaded(Plugin plugin);
    public static bool AreTeammates(BasePlayer firstPlayer, ulong secondPlayerId);
    private static void RunEffect(string prefab, Vector3 worldPosition, Vector3 worldDirection, Connection effectRecipient, bool sendToAll);
    private class Lang
    {
        public const string NeedTargetingComputer;
        public const string CooldownBeforeNextHack;
        public const string CrateLootDenied;
    }

    protected override void LoadDefaultMessages();
    public void SendReplyToPlayer(BasePlayer player, string messageKey, object[] args);
}

private class Configuration
{
    [JsonProperty("Version")]
    public string Version { get; set; }
    [JsonProperty("Required Targeting Computers For Hack")]
    public int RequiredTargetingComputersForHack { get; set; }
    [JsonProperty("Consume Targeting Computer On Hack")]
    public bool ConsumeTargetingComputerOnHack { get; set; }
    [JsonProperty("Crate Unlock Time Seconds")]
    public float CrateUnlockTimeSeconds { get; set; }
    [JsonProperty("Cooldown Between Hacks Seconds")]
    public float CooldownBetweenHacksSeconds { get; set; }
    [JsonProperty("Crate Lootable By Hacker Only")]
    public bool CrateLootableByHackerOnly { get; set; }
    [JsonProperty("Can Be Looted By Hacker Teammates")]
    public bool CanBeLootedByHackerTeammates { get; set; }
    [JsonProperty("Can Be Looted By Hacker Friends")]
    public bool CanBeLootedByHackerFriends { get; set; }
    [JsonProperty("Can Be Looted By Hacker Clanmates")]
    public bool CanBeLootedByHackerClanmates { get; set; }
}

private static class ClanUtil
{
    public static bool AreClanmates(ulong firstPlayerId, ulong secondPlayerId);
}

private static class FriendUtil
{
    public static bool AreFriends(ulong firstPlayerId, ulong secondPlayerId);
}

private class Lang
{
    public const string NeedTargetingComputer;
    public const string CooldownBeforeNextHack;
    public const string CrateLootDenied;
}


```

---

## LastManStanding by k1lly0u - Fight to the death event for Event Manager

```csharp
using Newtonsoft.Json;
using Oxide.Plugins.EventManagerEx;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("LastManStanding", "k1lly0u", "3.0.1"), Description("Last man standing event mode for EventManager")]
 class LastManStanding : RustPlugin, IEventPlugin
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
    private static string ToOrdinal(int i);
    public class LastManStandingEvent : EventManager.BaseEventGame
    {
        public EventManager.BaseEventPlayer winner;
        protected override EventManager.BaseEventPlayer AddPlayerComponent(BasePlayer player);
        internal override void PrestartEvent();
        internal override void OnEventPlayerDeath(EventManager.BaseEventPlayer victim, EventManager.BaseEventPlayer attacker, HitInfo info);
        protected override void GetWinningPlayers(List<EventManager.BaseEventPlayer> winners);
        protected override void BuildScoreboard();
        protected override float GetFirstScoreValue(EventManager.BaseEventPlayer eventPlayer);
        protected override float GetSecondScoreValue(EventManager.BaseEventPlayer eventPlayer);
        protected override void SortScores(List<EventManager.ScoreEntry> list);
    }

    private class LastManStandingPlayer : EventManager.BaseEventPlayer
    {
        internal override void OnPlayerDeath(EventManager.BaseEventPlayer attacker, float respawnTime);
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

public class LastManStandingEvent : EventManager.BaseEventGame
{
    public EventManager.BaseEventPlayer winner;
    protected override EventManager.BaseEventPlayer AddPlayerComponent(BasePlayer player);
    internal override void PrestartEvent();
    internal override void OnEventPlayerDeath(EventManager.BaseEventPlayer victim, EventManager.BaseEventPlayer attacker, HitInfo info);
    protected override void GetWinningPlayers(List<EventManager.BaseEventPlayer> winners);
    protected override void BuildScoreboard();
    protected override float GetFirstScoreValue(EventManager.BaseEventPlayer eventPlayer);
    protected override float GetSecondScoreValue(EventManager.BaseEventPlayer eventPlayer);
    protected override void SortScores(List<EventManager.ScoreEntry> list);
}

private class LastManStandingPlayer : EventManager.BaseEventPlayer
{
    internal override void OnPlayerDeath(EventManager.BaseEventPlayer attacker, float respawnTime);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Respawn time (seconds)")]
    public int RespawnTime { get; set; }
    public Oxide.Core.VersionNumber Version { get; set; }
}


```

---

## LayersOfRisk by MikeDanielsson - Play in this new game changing world where more focus is on how you choose your risks.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Random = System.Random;

Oxide.Plugins
[Info("Layers of Risk", "Mike Danielsson", 1.24)]
[Description("Play in this new game changing world where more focus is on how you choose your Risks.")]
 class LayersOfRisk : RustPlugin
{
     bool hasLateInitializedBeenRun;
     float TimeLookPlayerUiIsDrawn;
     Vector3 middlePosition;
     Vector3 downDirection;
     Vector3 vectormultiplier;
     int hackableCrateDefaultHackTime;
     bool enableZoneIndicators;
     int zoneIndicatorStrength;
     int outerWall;
     int middleWall;
     int InnerWall;
     int distanceToBuildFromWalls;
     int hackAbleWarZoneExtraTime;
     int maxHackableCrateSpawns;
     int hackableCratesSpawedAtPluginStart;
     int timeBetweenHackableSpawnTries;
     int hackableCrateZone1Time;
     int hackableCrateZone2Time;
     int hackableCrateZone3Time;
     int hackableCrateZone4Time;
     int hackableCrateZone1ArmourStart;
     int hackableCrateZone1ArmourEnd;
     int hackableCrateZone2ArmourStart;
     int hackableCrateZone2ArmourEnd;
     int hackableCrateZone3ArmourStart;
     int hackableCrateZone3ArmourEnd;
     int hackableCrateZone4ArmourStart;
     int hackableCrateZone4ArmourEnd;
     int hackableCrateZone1WeaponStart;
     int hackableCrateZone1WeaponEnd;
     int hackableCrateZone2WeaponStart;
     int hackableCrateZone2WeaponEnd;
     int hackableCrateZone3WeaponStart;
     int hackableCrateZone3WeaponEnd;
     int hackableCrateZone4WeaponStart;
     int hackableCrateZone4WeaponEnd;
     int totalWarZones;
     int warZoneTimeBetweenIntervalls;
     float playerWarZoneStartDecreser;
     int warZoneSize;
     int warZoneRaidAliveTimer;
     int playerWarZone1Delay;
     int playerWarZone2Delay;
     int playerWarZone3Delay;
     int playerWarZone4Delay;
     int timeBeforePlayerTakeDamageWhenEnterWarZone;
     int startWarZoneAfterDamageToStructure;
     int warZoneEnterDamage;
     bool canDieWhenEnteringWarZone;
     float updateTierRefreshRate;
     float initializeTimerDelay;
     float tierPointChangeDelay;
     int updateTierTime;
     string hackableCreate;
     string warZoneSphere;
     string vomit;
     string uiTierBox;
     string uiTierRangeBox;
     string uiZoneBox;
     string uiMeterZoneBox;
     string uiTierLoading;
     string uiTierPoints;
     string uiInfoText;
     string uiGetPlayerTierOnLook;
     string uiTierPointsChange;
     float tierBoxPosX;
     float tierBoxPosY;
     float tierBoxSizeX;
     float tierBoxSizeY;
     float tierProgressPosX;
     float tierProgressPosY;
     float tierProgressSizeX;
     float tierProgressSizeY;
     float tierRangeBoxPosX;
     float tierRangeBoxPosY;
     float tierRangeBoxSizeX;
     float tierRangeBoxSizeY;
     float zoneBoxPosX;
     float zoneBoxPosY;
     float zoneBoxSizeX;
     float zoneBoxSizeY;
     float meterZoneBoxPosX;
     float meterZoneBoxPosY;
     float meterZoneBoxSizeX;
     float meterZoneBoxSizeY;
     float getTierOnLookPosX;
     float getTierOnLookPosY;
     float getTierOnLookSizeX;
     float getTierOnLookSizeY;
     float getTierPointsChangePosX;
     float getTierPointsChangePosY;
     float getTierPointsChangeSizeX;
     float getTierPointsChangeSizeY;
     DynamicConfigFile weaponsDataFile;
     DynamicConfigFile armoursDataFile;
     DynamicConfigFile tiersDataFile;
     DynamicConfigFile hackableCreatPositionsDataFile;
     class playerDataClass
    {
        public bool isUpdateTierTimerRunning { get; set; }
        public bool isPlayerInitialized { get; set; }
        public int currentTier { get; set; }
        public int nextTier { get; set; }
        public int oldTier { get; set; }
        public int currentZone { get; set; }
        public float distanceFromMiddle { get; set; }
        public float cantStartRaidZoneTimer { get; set; }
        public float startRaidZoneAfterDamageDealtToStructure { get; set; }
        public float currentUpdateCycle { get; set; }
        public int tierId { get; set; }
        public float lookPlayerDelay { get; set; }
    }

     class tierDataClass
    {
        public string tierLetter { get; set; }
        public int min { get; set; }
        public int max { get; set; }
    }

     class itemDataClass
    {
        public string shortName { get; set; }
        public int value { get; set; }
        public int followItem { get; set; }
    }

     class warZoneDataClass
    {
        public int zone { get; set; }
        public float size { get; set; }
        public float currentUpTime { get; set; }
        public float aliveTime { get; set; }
        public Vector3 position { get; set; }
    }

     class hackableCrateDataClass
    {
        public bool inUse { get; set; }
    }

     Dictionary<ulong, playerDataClass> playerDataDic;
     Dictionary<int, tierDataClass> tierDataDic;
     Dictionary<int, itemDataClass> weaponDataDic;
     Dictionary<int, itemDataClass> armourDataDic;
     Dictionary<int, warZoneDataClass> warZoneDataDic;
     Dictionary<int, Timer> timerHolderDic;
     Dictionary<Vector3, hackableCrateDataClass> hackableCrateDataDic;
     void Init();
    protected override void LoadDefaultConfig();
     void updateSettingsFromConfigFile();
    protected override void LoadDefaultMessages();
     DynamicConfigFile createConfigFileIfDontExist(string file);
     void dataInitialization();
     void lateInitializing();
     BaseEntity spawnSpherePreFab(float size, Vector3 spawnPos);
     void hackableCrateSpawnTimer();
     void hackableCrateSpawnController();
     void spawnHackableCreate(Vector3 spawnPos);
     int getHowManyHackableCratesSpawned();
     void updateHackableCrateContainer(BaseEntity crate);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     object OnStructureUpgrade(BaseCombatEntity entity, BasePlayer player, BuildingGrade.Enum grade);
     object CanBuild(Planner planner, Construction prefab, Construction.Target target);
     void OnCrateHack(HackableLockedCrate crate);
     void setPlayerData(BasePlayer player);
     int getTierPointsForPlayer(BasePlayer player);
     int getArmourItemPointsFromContainer(ItemContainer containerBelt, ItemContainer containerWear, ItemContainer containerMain, int maxItemsCounted);
     int getHighestWeaponValueFromContainer(ItemContainer container);
     int getTierId(int value);
     void updatePlayer(BasePlayer player, int nextTier, bool setTierData);
     void updatePlayerData(BasePlayer player, int nextTier, bool setTierData);
     void updatePlayerGui(BasePlayer player, int nextTier, bool setTierData);
     bool isPlayerAwake(BasePlayer player);
     bool isPlayerAliveAndMobile(BasePlayer player);
     int getCurrentZone(float distance);
     int getDistanceToNextZone(float distanceToMiddle);
     float getDistanceFromPosition(Vector3 pos, Vector3 pos2, bool ignoreY);
     bool canVictimTakeDamage(BasePlayer victim, BasePlayer attacker);
     int checkTierRange(BasePlayer player, int tierId);
     bool isPlayerInsideRaidZone(BasePlayer player, Vector3 position, float raidZoneSize);
     void startWarZone(Vector3 spawnPos, float size, int timeAlive);
     BaseEntity getEntityFromRayCastHit(Ray ray);
     void displayTierFromLookPlayer(BasePlayer player, BasePlayer lookPlayer);
     bool canPlayerBuild(Planner planner, Construction prefab);
     string timeFormating(int time);
     Vector3 getRayCastHitPosition(Ray ray);
     void playerInitializeTimer(BasePlayer player);
     void playerTierTimer(BasePlayer player);
     void destroyGuiElement(BasePlayer player, string element);
     void destroyAllUiForPlayer(BasePlayer player);
     void createInfoBox(BasePlayer player, string uiElement, string text, float posX, float posY, float sizeX, float sizeY);
     void createInfoText(BasePlayer player, string uiElement, string text, float posX, float posY, float sizeX, float sizeY);
     void createTierProgressBar(BasePlayer player, string uiElement, string text, float loadingValue, float posX, float posY, float sizeX, float sizeY);
     void createTearRangeBox(BasePlayer player, string uiElement, int a, int b, int c, int d, int e);
     void getPlayerTierOnLookTextBox(BasePlayer player, string uiElement, string text, string color, float posX, float posY, float sizeX, float sizeY);
    [ChatCommand("cratepos")]
     void addHackableCreateSpawnPos(BasePlayer player);
}

 class playerDataClass
{
    public bool isUpdateTierTimerRunning { get; set; }
    public bool isPlayerInitialized { get; set; }
    public int currentTier { get; set; }
    public int nextTier { get; set; }
    public int oldTier { get; set; }
    public int currentZone { get; set; }
    public float distanceFromMiddle { get; set; }
    public float cantStartRaidZoneTimer { get; set; }
    public float startRaidZoneAfterDamageDealtToStructure { get; set; }
    public float currentUpdateCycle { get; set; }
    public int tierId { get; set; }
    public float lookPlayerDelay { get; set; }
}

 class tierDataClass
{
    public string tierLetter { get; set; }
    public int min { get; set; }
    public int max { get; set; }
}

 class itemDataClass
{
    public string shortName { get; set; }
    public int value { get; set; }
    public int followItem { get; set; }
}

 class warZoneDataClass
{
    public int zone { get; set; }
    public float size { get; set; }
    public float currentUpTime { get; set; }
    public float aliveTime { get; set; }
    public Vector3 position { get; set; }
}

 class hackableCrateDataClass
{
    public bool inUse { get; set; }
}


```

---

## LegacyNodes by OfficeEgg - Return Legacy nodes to rust. All nodes will now return a share of each resource, and HQM on last hit

```csharp
using System;
using Newtonsoft.Json;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Legacy Nodes", "xdEggie", "1.0.2")]
[Description("Restore legacy nodes to current rust")]
 class LegacyNodes : CovalencePlugin
{
    [PluginReference]
    private Plugin GatherRates;
    private string permPrefix;
    private List<BasePlayer> quedLastHit;
    private List<ResourceDispenser> blacklisted;
    private List<ResourceDispenser> nonbonusmetal;
    private Dictionary<ResourceDispenser, NodeData> nodes;
    private int stone;
    private int sulfur;
    private int metal;
    private int highqual;
    public class NodeData
    {
        public int rewardedstone { get; set; }
        public int rewardedmetal { get; set; }
        public int rewardedsulfur { get; set; }
        public bool sulfurComplete { get; set; }
        public bool stoneComplete { get; set; }
        public bool metalComplete { get; set; }
    }

    private void Init();
    private void OnServerInitialized();
    private LegacyNodeConfig legacyConfig;
    public class LegacyNodeConfig
    {
        [JsonProperty(PropertyName = "General Settings")]
        public GeneralSettings General { get; set; }
        [JsonProperty(PropertyName = "Chat Settings")]
        public ChatSettings Chat { get; set; }
        public class GeneralSettings
        {
            [JsonProperty("Bonus Enabled")]
            public bool bonusEnabled { get; set; }
            [JsonProperty("Permission Only Mode")]
            public bool permissionMode { get; set; }
        }

        public class ChatSettings
        {
            [JsonProperty("Announce Plugin Loaded In Game")]
            public bool AnnouncePluginLoaded { get; set; }
            [JsonProperty("Chat Prefix Enabled")]
            public bool PrefixEnabled { get; set; }
            [JsonProperty(PropertyName = "Chat Prefix Color")]
            public string PrefixColor { get; set; }
            [JsonProperty(PropertyName = "Chat Message Color")]
            public string MessageColor { get; set; }
        }

        [JsonProperty(PropertyName = "Version: ")]
        public Oxide.Core.VersionNumber Version { get; set; }
    }

    private void LoadDefaultConfig();
    private void SaveConfig();
    protected override void LoadConfig();
    private LegacyNodeConfig LoadBaseConfig();
    private void Unload();
    private void OnEntityDeath(ResourceEntity entity, HitInfo info);
    private void OnDispenserGather(ResourceDispenser dispenser, BasePlayer basePlayer, Item item);
    private void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer basePlayer, Item item);
    private void OnNodeLastHit(ResourceDispenser dispenser, BasePlayer basePlayer);
    private void DoItemAdd(BasePlayer player, int quantity, string itemid);
    private ItemDefinition FindItemDef(string idOrName);
    private int ValidateAmount(ResourceDispenser dispenser, int amount, string nodetype);
    private float ItemModifier(string userId, string itemid);
    private decimal FindDifference(decimal amountone, decimal amounttwo);
    private string GetLang(string message);
    protected override void LoadDefaultMessages();
    private const string permFormat;
    private static string GetPermission(string name);
    private GatherRatesConfig grconfig;
    private class GatherRatesConfig
    {
        [JsonProperty("GatherRateRulesets")]
        public GatherRateRuleset[] GatherRateRulesets;
    }

    private GatherRateRuleset GetPlayerRuleset(string userId);
    private class GatherRateRuleset
    {
        [JsonProperty("Name")]
        public string Name;
        [JsonProperty("DefaultRate")]
        public float DefaultRate;
        [JsonProperty("ItemRateOverrides", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public Dictionary<string, float> ItemRateOverrides;
        [JsonProperty("DispenserRateOverrides", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public Dictionary<string, Dictionary<string, float>> DispenserRateOverrides;
        public float GetGatherRate(Item item);
    }

    private void LoadGatherRatesConfig();
}

public class NodeData
{
    public int rewardedstone { get; set; }
    public int rewardedmetal { get; set; }
    public int rewardedsulfur { get; set; }
    public bool sulfurComplete { get; set; }
    public bool stoneComplete { get; set; }
    public bool metalComplete { get; set; }
}

public class LegacyNodeConfig
{
    [JsonProperty(PropertyName = "General Settings")]
    public GeneralSettings General { get; set; }
    [JsonProperty(PropertyName = "Chat Settings")]
    public ChatSettings Chat { get; set; }
    public class GeneralSettings
    {
        [JsonProperty("Bonus Enabled")]
        public bool bonusEnabled { get; set; }
        [JsonProperty("Permission Only Mode")]
        public bool permissionMode { get; set; }
    }

    public class ChatSettings
    {
        [JsonProperty("Announce Plugin Loaded In Game")]
        public bool AnnouncePluginLoaded { get; set; }
        [JsonProperty("Chat Prefix Enabled")]
        public bool PrefixEnabled { get; set; }
        [JsonProperty(PropertyName = "Chat Prefix Color")]
        public string PrefixColor { get; set; }
        [JsonProperty(PropertyName = "Chat Message Color")]
        public string MessageColor { get; set; }
    }

    [JsonProperty(PropertyName = "Version: ")]
    public Oxide.Core.VersionNumber Version { get; set; }
}

public class GeneralSettings
{
    [JsonProperty("Bonus Enabled")]
    public bool bonusEnabled { get; set; }
    [JsonProperty("Permission Only Mode")]
    public bool permissionMode { get; set; }
}

public class ChatSettings
{
    [JsonProperty("Announce Plugin Loaded In Game")]
    public bool AnnouncePluginLoaded { get; set; }
    [JsonProperty("Chat Prefix Enabled")]
    public bool PrefixEnabled { get; set; }
    [JsonProperty(PropertyName = "Chat Prefix Color")]
    public string PrefixColor { get; set; }
    [JsonProperty(PropertyName = "Chat Message Color")]
    public string MessageColor { get; set; }
}

private class GatherRatesConfig
{
    [JsonProperty("GatherRateRulesets")]
    public GatherRateRuleset[] GatherRateRulesets;
}

private class GatherRateRuleset
{
    [JsonProperty("Name")]
    public string Name;
    [JsonProperty("DefaultRate")]
    public float DefaultRate;
    [JsonProperty("ItemRateOverrides", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public Dictionary<string, float> ItemRateOverrides;
    [JsonProperty("DispenserRateOverrides", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public Dictionary<string, Dictionary<string, float>> DispenserRateOverrides;
    public float GetGatherRate(Item item);
}


```

---

## LifeStealer by beee - Gives players health when they deal damage

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Rust;
using Oxide.Core;
using Newtonsoft.Json;

Oxide.Plugins
[Info("LifeStealer", "beee", "1.2.0")]
[Description("Applies lifesteal based on player damage")]
 class LifeStealer : RustPlugin
{
    private static PluginConfig _config;
    private Dictionary<ulong, string> PlayersPermissionsCache;
     void OnServerInitialized();
     void Unload();
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void OnUserPermissionGranted(string userId, string permName);
     void OnUserPermissionRevoked(string userId, string permName);
    private void OnUserGroupAdded(string userId, string groupName);
    private void OnUserGroupRemoved(string userId, string groupName);
    private void RegisterPermissions();
    private KeyValuePair<string, PermissionSettings> GetPlayerPermission(BasePlayer player);
    private class PluginConfig
    {
        public Oxide.Core.VersionNumber Version;
        [JsonProperty("Permissions")]
        public Dictionary<string, PermissionSettings> Permissions { get; set; }
    }

    private class PermissionSettings
    {
        [JsonProperty("Lifesteal % (of damage dealt)")]
        public int LifestealPercent { get; set; }
        [JsonProperty("Static Heal")]
        public int StaticHeal { get; set; }
        [JsonProperty("Heal from Players?")]
        public bool FromPlayers { get; set; }
        [JsonProperty("Heal from Scientists?")]
        public bool FromScientists { get; set; }
        [JsonProperty("Heal from Animals?")]
        public bool FromAnimals { get; set; }
        [JsonProperty("Heal from Patrol Helicopter?")]
        public bool FromHelicopters { get; set; }
        [JsonProperty("Heal from Bradley APC?")]
        public bool FromBradley { get; set; }
        [JsonProperty("Custom Victims ShortPrefabName (Optional)")]
        public List<string> VictimPrefabs { get; set; }
    }

    private PluginConfig GetDefaultConfig();
    private void CheckForConfigUpdates();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
}

private class PluginConfig
{
    public Oxide.Core.VersionNumber Version;
    [JsonProperty("Permissions")]
    public Dictionary<string, PermissionSettings> Permissions { get; set; }
}

private class PermissionSettings
{
    [JsonProperty("Lifesteal % (of damage dealt)")]
    public int LifestealPercent { get; set; }
    [JsonProperty("Static Heal")]
    public int StaticHeal { get; set; }
    [JsonProperty("Heal from Players?")]
    public bool FromPlayers { get; set; }
    [JsonProperty("Heal from Scientists?")]
    public bool FromScientists { get; set; }
    [JsonProperty("Heal from Animals?")]
    public bool FromAnimals { get; set; }
    [JsonProperty("Heal from Patrol Helicopter?")]
    public bool FromHelicopters { get; set; }
    [JsonProperty("Heal from Bradley APC?")]
    public bool FromBradley { get; set; }
    [JsonProperty("Custom Victims ShortPrefabName (Optional)")]
    public List<string> VictimPrefabs { get; set; }
}


```

---

## LifeSupport by OG61 - Prevents death and restores health to 100% when activated

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Core;
using System.Collections.Generic;
using System;

Oxide.Plugins
[Info("Life Support", "OG61", "1.5.0")]
[Description("Use reward points to prevent player from dying")]
public class LifeSupport : CovalencePlugin
{
    [PluginReference]
    private Plugin RaidableBases;
    private Plugin ServerRewards;
    private Plugin DangerousTreasures;
    private Plugin ZoneManager;
    private const string PERMISSION_BLOCKED;
    private class Perms
    {
        public string Permission { get; set; }
        public int Cost { get; set; }
    }

    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Use Zone Manager (true/false)")]
        public bool UseZoneManager;
        [JsonProperty(PropertyName = "Use Server Rewards (true/false)")]
        public bool UseServerRewards;
        [JsonProperty(PropertyName = "Disable LifeSupport in RaidableBases Zones (true/false)")]
        public bool UseRaidableBases;
        [JsonProperty(PropertyName = "Disable LifeSupport in DangerousTreasures Zones (true/false)")]
        public bool UseDangerousTreasures;
        [JsonProperty(PropertyName = "Enable Log file (true/false)")]
        public bool LogToFile;
        [JsonProperty(PropertyName = "Log output to console (true/false)")]
        public bool LogToConsole;
        [JsonProperty(PropertyName = "Permissions and cost", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<Perms> perms;
    }

    private PluginConfig config;
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnPluginLoaded(Plugin plugin);
    private void OnPluginUnloaded(Plugin plugin);
    private Data data;
    private class Data
    {
        public List<string> activatedIDs;
        public List<string> excludedZones;
    }

    private void SaveData();
    private void Loaded();
    private object OnPlayerWound(BasePlayer player);
    private object OnPlayerDeath(BasePlayer player, HitInfo hitInfo);
     bool CanDropActiveItem(BasePlayer player);
    private object SaveLife(BasePlayer player);
    private string GetLang(string key, string id, object[] args);
    private void Logger(string key, IPlayer player, object[] args);
    private void Message(string key, IPlayer player, object[] args);
    [Command("lifesupport")]
    private void LifeSupportToggle(IPlayer player, string msg, string[] args);
    [Command("lsZones")]
    private void LsZones(IPlayer player, string msg, string[] args);
    protected override void LoadDefaultMessages();
}

private class Perms
{
    public string Permission { get; set; }
    public int Cost { get; set; }
}

private class PluginConfig
{
    [JsonProperty(PropertyName = "Use Zone Manager (true/false)")]
    public bool UseZoneManager;
    [JsonProperty(PropertyName = "Use Server Rewards (true/false)")]
    public bool UseServerRewards;
    [JsonProperty(PropertyName = "Disable LifeSupport in RaidableBases Zones (true/false)")]
    public bool UseRaidableBases;
    [JsonProperty(PropertyName = "Disable LifeSupport in DangerousTreasures Zones (true/false)")]
    public bool UseDangerousTreasures;
    [JsonProperty(PropertyName = "Enable Log file (true/false)")]
    public bool LogToFile;
    [JsonProperty(PropertyName = "Log output to console (true/false)")]
    public bool LogToConsole;
    [JsonProperty(PropertyName = "Permissions and cost", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<Perms> perms;
}

private class Data
{
    public List<string> activatedIDs;
    public List<string> excludedZones;
}


```

---

## LifeVest by baconjake - Give players a unique way to revive at death location.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("Life Vest", "baconjake", "0.6.2")]
[Description("Allows players a way to revive at death location")]
public class LifeVest : RustPlugin
{
    private const string PERMISSION_USE;
    private const string PERMISSION_GIVE;
    private const string PERMISSION_NOCD;
    private Dictionary<string, float> lifeVestCooldowns;
    private BaseEntity parentEntity;
    private Vector3 deathPosition;
    protected override void LoadDefaultMessages();
    private class ConfigData
    {
        public string Version { get; set; }
        public float LifeVestDurabilityLoss { get; set; }
        public float StartingDurability { get; set; }
        public int RevivePlayerHealth { get; set; }
        public ulong ItemSkinId { get; set; }
        public string ItemShortName { get; set; }
        public string ItemDisplayName { get; set; }
        public bool EnableDurabilityLoss { get; set; }
        public bool EnableServerMessage { get; set; }
        public int LifeVestCooldown { get; set; }
    }

    private ConfigData configData;
    private bool IsWearingLifeVest(BasePlayer player);
    private void LoadConfigData();
    private void SaveConfigData();
    protected override void LoadDefaultConfig();
    private void Init();
    private Quaternion deathRotation;
     void OnPlayerDeath(BasePlayer player, HitInfo info);
    private void CreateLifeVestButton(BasePlayer player);
    private void RemoveLifeVestButton(BasePlayer player);
    private void RevivePlayer(BasePlayer player, float lifeVestCondition);
     void OnPlayerRespawned(BasePlayer player);
    [ConsoleCommand("lifevest.revive")]
    private void LifeVestReviveCommand(ConsoleSystem.Arg args);
    [ChatCommand("givelifevestall")]
    private void GiveLifeVestAllCommand(BasePlayer player, string command, string[] args);
    private void GiveLifeVestToPlayer(BasePlayer target);
    [ChatCommand("giveLifeVest")]
    private void GiveLifeVestCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("lvc")]
    private void LifeVestCooldownCommand(BasePlayer player, string command, string[] args);
}

private class ConfigData
{
    public string Version { get; set; }
    public float LifeVestDurabilityLoss { get; set; }
    public float StartingDurability { get; set; }
    public int RevivePlayerHealth { get; set; }
    public ulong ItemSkinId { get; set; }
    public string ItemShortName { get; set; }
    public string ItemDisplayName { get; set; }
    public bool EnableDurabilityLoss { get; set; }
    public bool EnableServerMessage { get; set; }
    public int LifeVestCooldown { get; set; }
}


```

---

## LightControl by Hovmodet - Toggle players lights separately
Changes the behavior of "lighttoggle"

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;
using Rust.Modular;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using Network;
using Oxide.Core.Configuration;
using Rust;
using System.Collections;

Oxide.Plugins
[Info("Light Control", "Hovmodet", "1.0.4")]
[Description("Toggle lights separately")]
public class LightControl : RustPlugin
{
    private const string UsePermission;
    private void Init();
    protected override void LoadDefaultMessages();
    public bool hasPerm(BasePlayer player);
    [ConsoleCommand("headtoggle")]
    private void lightheadcmd(ConsoleSystem.Arg arg);
    private void lighthand(BasePlayer player);
     object OnServerCommand(ConsoleSystem.Arg arg);
     object CanWearItem(PlayerInventory inventory, Item item, int targetSlot);
}


```

---

## LighthouseBeacon by S0N0FBISCUIT - Adds a beacon to the lighthouse monument

```csharp
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Lighthouse Beacon", "S0N_0F_BISCUIT", "1.0.3")]
[Description("Adds a beacon to the lighthouse monument")]
 class LighthouseBeacon : RustPlugin
{
    private List<SearchLight> beacons;
     class Beacon : MonoBehaviour
    {
        private SearchLight light { get; set; }
        private void Awake();
        private void Update();
        public void OnDestroy();
    }

     void OnServerInitialized();
     void Unload();
     void CreateBeacon(Vector3 position);
}

 class Beacon : MonoBehaviour
{
    private SearchLight light { get; set; }
    private void Awake();
    private void Update();
    public void OnDestroy();
}


```

---

## LightsOn by  - Control light state with no fuel usage (and most devices too)

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.Threading.Tasks;
using System;
using System.IO;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Lights On", "mspeedie", "1.6.19")]
[Description("Toggle lights on/off either as configured or by name")]
public class LightsOn : CovalencePlugin
{
    const string perm_lightson;
    private bool InitialPassNight;
    private bool NightToggleactive;
    private bool nightcross24;
    private bool config_changed;
    private Timer Nighttimer;
    private Timer Alwaystimer;
    private Timer Devicetimer;
    const string auto_turret_name;
    const string bbq_name;
    const string campfire_name;
    const string cctv_name;
    const string ceilinglight_name;
    const string chineselantern_name;
    const string cursedcauldron_name;
    const string deluxe_lightstring_name;
    const string elevator_name;
    const string fireplace_name;
    const string fluidswitch_name;
    const string fogmachine_name;
    const string furnace_large_name;
    const string furnace_name;
    const string hatcandle_name;
    const string hatminer_name;
    const string heater_name;
    const string hobobarrel_name;
    const string igniter_name;
    const string jackolanternangry_name;
    const string jackolanternhappy_name;
    const string lantern_name;
    const string largecandleset_name;
    const string mixingtable_name;
    const string reactivetarget_name;
    const string rfbroadcaster_name;
    const string rfreceiver_name;
    const string sam_site_name;
    const string refinerysmall_name;
    const string searchlight_name;
    const string simplelight_name;
    const string skullfirepit_name;
    const string smallcandleset_name;
    const string smallrefinery_name;
    const string smart_alarm_name;
    const string smart_switch_name;
    const string snowmachine_name;
    const string storage_monitor_name;
    const string telephone_name;
    const string teslacoil_name;
    const string tunalight_name;
    const string vehiclelift_name;
    const string water_purifier_name;
    const string waterpump_name;
    const string flasherlight_name;
    const string sirenlight_name;
    const string spookyspeaker_name;
    const string strobelight_name;
    const string sign_name;
    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Hats do not use fuel (true/false)")]
        public bool Hats { get; set; }
        [JsonProperty(PropertyName = "BBQs (true/false)")]
        public bool BBQs { get; set; }
        [JsonProperty(PropertyName = "Campfires (true/false)")]
        public bool Campfires { get; set; }
        [JsonProperty(PropertyName = "Candles (true/false)")]
        public bool Candles { get; set; }
        [JsonProperty(PropertyName = "Cauldrons (true/false)")]
        public bool Cauldrons { get; set; }
        [JsonProperty(PropertyName = "CCTVs (true/false)")]
        public bool CCTVs { get; set; }
        [JsonProperty(PropertyName = "Ceiling Lights (true/false)")]
        public bool CeilingLights { get; set; }
        [JsonProperty(PropertyName = "Deluxe Light Strings (true/false)")]
        public bool Deluxe_lightstrings { get; set; }
        [JsonProperty(PropertyName = "Elevators (true/false)")]
        public bool Elevators { get; set; }
        [JsonProperty(PropertyName = "Fire Pits (true/false)")]
        public bool FirePits { get; set; }
        [JsonProperty(PropertyName = "Fireplaces (true/false)")]
        public bool Fireplaces { get; set; }
        [JsonProperty(PropertyName = "Flasher Lights (true/false)")]
        public bool FlasherLights { get; set; }
        [JsonProperty(PropertyName = "FluidSwitches (true/false)")]
        public bool FluidSwitches { get; set; }
        [JsonProperty(PropertyName = "Fog Machines (true/false)")]
        public bool Fog_Machines { get; set; }
        [JsonProperty(PropertyName = "Furnaces (true/false)")]
        public bool Furnaces { get; set; }
        [JsonProperty(PropertyName = "Hobo Barrels (true/false)")]
        public bool HoboBarrels { get; set; }
        [JsonProperty(PropertyName = "Heaters (true/false)")]
        public bool Heaters { get; set; }
        [JsonProperty(PropertyName = "Igniters (true/false)")]
        public bool Igniters { get; set; }
        [JsonProperty(PropertyName = "Lanterns (true/false)")]
        public bool Lanterns { get; set; }
        [JsonProperty(PropertyName = "Mixing Tables (true/false)")]
        public bool MixingTables { get; set; }
        [JsonProperty(PropertyName = "Reactive Targets (true/false)")]
        public bool ReactiveTargets { get; set; }
        [JsonProperty(PropertyName = "RF Broadcasters (true/false)")]
        public bool RFBroadcasters { get; set; }
        [JsonProperty(PropertyName = "RF Receivers (true/false)")]
        public bool RFReceivers { get; set; }
        [JsonProperty(PropertyName = "Refineries (true/false)")]
        public bool Refineries { get; set; }
        [JsonProperty(PropertyName = "SAM Sites (true/false)")]
        public bool SamSites { get; set; }
        [JsonProperty(PropertyName = "Search Lights (true/false)")]
        public bool SearchLights { get; set; }
        [JsonProperty(PropertyName = "Signs (true/false)")]
        public bool Signs { get; set; }
        [JsonProperty(PropertyName = "Simple Lights (true/false)")]
        public bool SimpleLights { get; set; }
        [JsonProperty(PropertyName = "Siren Lights (true/false)")]
        public bool SirenLights { get; set; }
        [JsonProperty(PropertyName = "Smart Alarms (true/false)")]
        public bool SmartAlarms { get; set; }
        [JsonProperty(PropertyName = "Smart Switches (true/false)")]
        public bool SmartSwitches { get; set; }
        [JsonProperty(PropertyName = "SnowMachines (true/false)")]
        public bool Snow_Machines { get; set; }
        [JsonProperty(PropertyName = "SpookySpeakers (true/false)")]
        public bool Speakers { get; set; }
        [JsonProperty(PropertyName = "Storage Monitor (true/false)")]
        public bool StorageMonitors { get; set; }
        [JsonProperty(PropertyName = "Strobe Lights (true/false)")]
        public bool StrobeLights { get; set; }
        [JsonProperty(PropertyName = "Telephones (true/false)")]
        public bool Telephones { get; set; }
        [JsonProperty(PropertyName = "VehicleLifts (true/false)")]
        public bool VehicleLifts { get; set; }
        [JsonProperty(PropertyName = "Water Pumps (true/false)")]
        public bool WaterPumps { get; set; }
        [JsonProperty(PropertyName = "Water Purifier s(true/false)")]
        public bool WaterPurifiers { get; set; }
        [JsonProperty(PropertyName = "Protect BBQs (true/false)")]
        public bool ProtectBBQs { get; set; }
        [JsonProperty(PropertyName = "Protect Campfires (true/false)")]
        public bool ProtectCampfires { get; set; }
        [JsonProperty(PropertyName = "Protect Cauldrons (true/false)")]
        public bool ProtectCauldrons { get; set; }
        [JsonProperty(PropertyName = "Protect Fire Pits (true/false)")]
        public bool ProtectFirePits { get; set; }
        [JsonProperty(PropertyName = "Protect Fireplaces (true/false)")]
        public bool ProtectFireplaces { get; set; }
        [JsonProperty(PropertyName = "Protect Furnaces (true/false)")]
        public bool ProtectFurnaces { get; set; }
        [JsonProperty(PropertyName = "Protect Hobo Barrels (true/false)")]
        public bool ProtectHoboBarrels { get; set; }
        [JsonProperty(PropertyName = "Protect Mixing Tables (true/false)")]
        public bool ProtectMixingTables { get; set; }
        [JsonProperty(PropertyName = "Protect Refineries (true/false)")]
        public bool ProtectRefineries { get; set; }
        [JsonProperty(PropertyName = "Devices Always On (true/false)")]
        public bool DevicesAlwaysOn { get; set; }
        [JsonProperty(PropertyName = "Always On (true/false)")]
        public bool AlwaysOn { get; set; }
        [JsonProperty(PropertyName = "Night Toggle (true/false)")]
        public bool NightToggle { get; set; }
        [JsonProperty(PropertyName = "Console Output (true/false)")]
        public bool ConsoleMsg { get; set; }
        [JsonProperty(PropertyName = "Night Toggle Check Frequency (in seconds)")]
        public int NightCheckFrequency { get; set; }
        [JsonProperty(PropertyName = "Always On Check Frequency (in seconds)")]
        public int AlwaysCheckFrequency { get; set; }
        [JsonProperty(PropertyName = "Device Check Frequency (in seconds)")]
        public int DeviceCheckFrequency { get; set; }
        [JsonProperty(PropertyName = "Dusk Time (HH in a 24 hour clock)")]
        public float DuskTime { get; set; }
        [JsonProperty(PropertyName = "Dawn Time (HH in a 24 hour clock)")]
        public float DawnTime { get; set; }
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
    protected void CheckConfig();
    private void OnServerInitialized();
     string CleanedName(string prefabName);
     bool IsOvenPrefabName(string prefabName);
     bool IsLightToggle(string prefabName);
     bool IsLightPrefabName(string prefabName);
     bool IsHatPrefabName(string prefabName);
     bool IsDevicePrefabName(string prefabName);
     bool CanCookShortPrefabName(string prefabName);
     bool ProtectShortPrefabName(string prefabName);
     bool SignShortPrefabName(string prefabName);
     bool ProcessShortPrefabName(string prefabName);
    private void AlwaysTimerProcess();
    private void DeviceTimerProcess();
    private void NightTimerProcess();
    private void ProcessNight();
    private void ProcessLights(bool state, string prefabName);
    private void ProcessDevices(bool state, string prefabName);
    private object OnFindBurnable(BaseOven oven);
    private void OnFuelConsume(BaseOven oven, Item fuel, ItemModBurnable burnable);
    private void OnItemUse(Item item, int amount);
     object OnOvenToggle(BaseOven oven, BasePlayer player);
     void SamSiteChangePower();
     void LightStringChangePower();
     void FlasherLightChangePower();
     void SirenLightChangePower();
     void SmartAlarmChangePower();
     void SmartSwitchChangePower();
     void StorageMonitorChangePower();
     void CCTVChangePower();
     void SignChangePower();
     void IgniterChangePower();
     void VehicleLiftChangePower();
     void HeaterChangePower();
     void ElevatorChangePower();
     void SearchLightChangePower();
     void WaterPumpChangePower();
     void WaterPurifierChangePower();
     void FluidSwitchChangePower();
     void ReactiveTargetChangePower();
     void RFBroadcasterChangePower();
     void RFReceiverChangePower();
     void TelephoneChangePower();
     void OnEntityBuilt(Planner plan, GameObject go);
    private void OnEntitySpawned(BaseNetworkable entity);
    [Command("lights")]
    private void ChatCommandlo(IPlayer player, string cmd, string[] args);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Hats do not use fuel (true/false)")]
    public bool Hats { get; set; }
    [JsonProperty(PropertyName = "BBQs (true/false)")]
    public bool BBQs { get; set; }
    [JsonProperty(PropertyName = "Campfires (true/false)")]
    public bool Campfires { get; set; }
    [JsonProperty(PropertyName = "Candles (true/false)")]
    public bool Candles { get; set; }
    [JsonProperty(PropertyName = "Cauldrons (true/false)")]
    public bool Cauldrons { get; set; }
    [JsonProperty(PropertyName = "CCTVs (true/false)")]
    public bool CCTVs { get; set; }
    [JsonProperty(PropertyName = "Ceiling Lights (true/false)")]
    public bool CeilingLights { get; set; }
    [JsonProperty(PropertyName = "Deluxe Light Strings (true/false)")]
    public bool Deluxe_lightstrings { get; set; }
    [JsonProperty(PropertyName = "Elevators (true/false)")]
    public bool Elevators { get; set; }
    [JsonProperty(PropertyName = "Fire Pits (true/false)")]
    public bool FirePits { get; set; }
    [JsonProperty(PropertyName = "Fireplaces (true/false)")]
    public bool Fireplaces { get; set; }
    [JsonProperty(PropertyName = "Flasher Lights (true/false)")]
    public bool FlasherLights { get; set; }
    [JsonProperty(PropertyName = "FluidSwitches (true/false)")]
    public bool FluidSwitches { get; set; }
    [JsonProperty(PropertyName = "Fog Machines (true/false)")]
    public bool Fog_Machines { get; set; }
    [JsonProperty(PropertyName = "Furnaces (true/false)")]
    public bool Furnaces { get; set; }
    [JsonProperty(PropertyName = "Hobo Barrels (true/false)")]
    public bool HoboBarrels { get; set; }
    [JsonProperty(PropertyName = "Heaters (true/false)")]
    public bool Heaters { get; set; }
    [JsonProperty(PropertyName = "Igniters (true/false)")]
    public bool Igniters { get; set; }
    [JsonProperty(PropertyName = "Lanterns (true/false)")]
    public bool Lanterns { get; set; }
    [JsonProperty(PropertyName = "Mixing Tables (true/false)")]
    public bool MixingTables { get; set; }
    [JsonProperty(PropertyName = "Reactive Targets (true/false)")]
    public bool ReactiveTargets { get; set; }
    [JsonProperty(PropertyName = "RF Broadcasters (true/false)")]
    public bool RFBroadcasters { get; set; }
    [JsonProperty(PropertyName = "RF Receivers (true/false)")]
    public bool RFReceivers { get; set; }
    [JsonProperty(PropertyName = "Refineries (true/false)")]
    public bool Refineries { get; set; }
    [JsonProperty(PropertyName = "SAM Sites (true/false)")]
    public bool SamSites { get; set; }
    [JsonProperty(PropertyName = "Search Lights (true/false)")]
    public bool SearchLights { get; set; }
    [JsonProperty(PropertyName = "Signs (true/false)")]
    public bool Signs { get; set; }
    [JsonProperty(PropertyName = "Simple Lights (true/false)")]
    public bool SimpleLights { get; set; }
    [JsonProperty(PropertyName = "Siren Lights (true/false)")]
    public bool SirenLights { get; set; }
    [JsonProperty(PropertyName = "Smart Alarms (true/false)")]
    public bool SmartAlarms { get; set; }
    [JsonProperty(PropertyName = "Smart Switches (true/false)")]
    public bool SmartSwitches { get; set; }
    [JsonProperty(PropertyName = "SnowMachines (true/false)")]
    public bool Snow_Machines { get; set; }
    [JsonProperty(PropertyName = "SpookySpeakers (true/false)")]
    public bool Speakers { get; set; }
    [JsonProperty(PropertyName = "Storage Monitor (true/false)")]
    public bool StorageMonitors { get; set; }
    [JsonProperty(PropertyName = "Strobe Lights (true/false)")]
    public bool StrobeLights { get; set; }
    [JsonProperty(PropertyName = "Telephones (true/false)")]
    public bool Telephones { get; set; }
    [JsonProperty(PropertyName = "VehicleLifts (true/false)")]
    public bool VehicleLifts { get; set; }
    [JsonProperty(PropertyName = "Water Pumps (true/false)")]
    public bool WaterPumps { get; set; }
    [JsonProperty(PropertyName = "Water Purifier s(true/false)")]
    public bool WaterPurifiers { get; set; }
    [JsonProperty(PropertyName = "Protect BBQs (true/false)")]
    public bool ProtectBBQs { get; set; }
    [JsonProperty(PropertyName = "Protect Campfires (true/false)")]
    public bool ProtectCampfires { get; set; }
    [JsonProperty(PropertyName = "Protect Cauldrons (true/false)")]
    public bool ProtectCauldrons { get; set; }
    [JsonProperty(PropertyName = "Protect Fire Pits (true/false)")]
    public bool ProtectFirePits { get; set; }
    [JsonProperty(PropertyName = "Protect Fireplaces (true/false)")]
    public bool ProtectFireplaces { get; set; }
    [JsonProperty(PropertyName = "Protect Furnaces (true/false)")]
    public bool ProtectFurnaces { get; set; }
    [JsonProperty(PropertyName = "Protect Hobo Barrels (true/false)")]
    public bool ProtectHoboBarrels { get; set; }
    [JsonProperty(PropertyName = "Protect Mixing Tables (true/false)")]
    public bool ProtectMixingTables { get; set; }
    [JsonProperty(PropertyName = "Protect Refineries (true/false)")]
    public bool ProtectRefineries { get; set; }
    [JsonProperty(PropertyName = "Devices Always On (true/false)")]
    public bool DevicesAlwaysOn { get; set; }
    [JsonProperty(PropertyName = "Always On (true/false)")]
    public bool AlwaysOn { get; set; }
    [JsonProperty(PropertyName = "Night Toggle (true/false)")]
    public bool NightToggle { get; set; }
    [JsonProperty(PropertyName = "Console Output (true/false)")]
    public bool ConsoleMsg { get; set; }
    [JsonProperty(PropertyName = "Night Toggle Check Frequency (in seconds)")]
    public int NightCheckFrequency { get; set; }
    [JsonProperty(PropertyName = "Always On Check Frequency (in seconds)")]
    public int AlwaysCheckFrequency { get; set; }
    [JsonProperty(PropertyName = "Device Check Frequency (in seconds)")]
    public int DeviceCheckFrequency { get; set; }
    [JsonProperty(PropertyName = "Dusk Time (HH in a 24 hour clock)")]
    public float DuskTime { get; set; }
    [JsonProperty(PropertyName = "Dawn Time (HH in a 24 hour clock)")]
    public float DawnTime { get; set; }
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## LimitedAdmin by misticos - Prevents admin abuse by blocking actions and commands

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;
using Object = UnityEngine.Object;

Oxide.Plugins
[Info("Limited Admin", "misticos", "1.0.8")]
[Description("Prevents admin abuse by blocking actions and commands")]
 class LimitedAdmin : RustPlugin
{
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Limited Admins (SteamID)",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ulong> Admins;
        [JsonProperty(PropertyName = "Limited Auth Levels",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<uint> LimitedAuthLevels;
        [JsonProperty(PropertyName = "Whitelisted Admins (SteamID)",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ulong> ExcludedAdmins;
        [JsonProperty(PropertyName = "Limit All Admins Exclude Whitelisted")]
        public bool LimitAll;
        [JsonProperty(PropertyName = "Blocked Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Blacklist;
        [JsonProperty(PropertyName = "Can Loot Entity")]
        public bool CanLootEntity;
        [JsonProperty(PropertyName = "Can Loot Player")]
        public bool CanLootPlayer;
        [JsonProperty(PropertyName = "Can Pickup Entity")]
        public bool CanPickupEntity;
        [JsonProperty(PropertyName = "Can Rename Bed")]
        public bool CanRenameBed;
        [JsonProperty(PropertyName = "Can Use Locked Entity")]
        public bool CanUseLockedEntity;
        [JsonProperty(PropertyName = "Can Interact With Lock")]
        public bool CanInteractLock;
        [JsonProperty(PropertyName = "Can Use Voice Chat")]
        public bool CanUseVoiceChat;
        [JsonProperty(PropertyName = "Can Be Targeted")]
        public bool CanBeTargeted;
        [JsonProperty(PropertyName = "Can Build")]
        public bool CanBuild;
        [JsonProperty(PropertyName = "Can Interact With Structure")]
        public bool CanInteractStructure;
        [JsonProperty(PropertyName = "Can Hack CH47 Crate")]
        public bool CanHackCrate;
        [JsonProperty(PropertyName = "Can Interact With Item")]
        public bool CanInteractItem;
        [JsonProperty(PropertyName = "Can Pickup Item")]
        public bool CanItemPickup;
        [JsonProperty(PropertyName = "Can Damage")]
        public bool CanDamage;
        [JsonProperty(PropertyName = "Can Use Lift")]
        public bool CanUseLift;
        [JsonProperty(PropertyName = "Can Toggle Oven")]
        public bool CanToggleOven;
        [JsonProperty(PropertyName = "Can Toggle Recycler")]
        public bool CanToggleRecycler;
        [JsonProperty(PropertyName = "Can Interact With Turret")]
        public bool CanInteractTurret;
        [JsonProperty(PropertyName = "Can Gather")]
        public bool CanGather;
        [JsonProperty(PropertyName = "Can Update Sign")]
        public bool CanUpdateSign;
        [JsonProperty(PropertyName = "Can Interact With Cupboard")]
        public bool CanInteractCupboard;
        [JsonProperty(PropertyName = "Can Interact With Vending Machine")]
        public bool CanInteractVending;
        [JsonProperty(PropertyName = "Can Interact With Weapons")]
        public bool CanInteractWeapons;
        [JsonProperty(PropertyName = "Debug")]
        public bool Debug;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private object CanLootEntity(BasePlayer player, Object container);
    private object CanLootPlayer(BasePlayer target, BasePlayer looter);
    private object CanPickupEntity(BasePlayer player, BaseCombatEntity entity);
    private object CanRenameBed(BasePlayer player, SleepingBag bed, string bedName);
    private object CanUseLockedEntity(BasePlayer player, BaseLock baseLock);
    private object OnPlayerVoice(BasePlayer player, byte[] data);
    private object CanBeTargeted(BaseCombatEntity entity, MonoBehaviour behaviour);
    private object CanBradleyApcTarget(BradleyAPC apc, BaseEntity entity);
    private object CanHelicopterStrafeTarget(PatrolHelicopterAI entity, BasePlayer target);
    private object CanHelicopterTarget(PatrolHelicopterAI heli, BasePlayer player);
    private object OnNpcTarget(BaseNpc npc, BaseEntity entity);
    private object OnTurretTarget(AutoTurret turret, BaseCombatEntity entity);
    private object CanBuild(Planner planner, Construction prefab, Construction.Target target);
    private object OnStructureDemolish(BaseCombatEntity entity, BasePlayer player, bool immediate);
    private object OnStructureRepair(BaseCombatEntity entity, BasePlayer player, bool immediate);
    private object OnStructureRotate(BaseCombatEntity entity, BasePlayer player, bool immediate);
    private object OnStructureUpgrade(BaseCombatEntity entity, BasePlayer player, bool immediate);
    private object CanHackCrate(BasePlayer player, HackableLockedCrate crate);
    private object OnItemAction(Item item, string action, BasePlayer player);
    private object OnItemPickup(Item item, BasePlayer player);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private object OnLiftUse(Object lift, BasePlayer player);
    private object OnOvenToggle(BaseOven oven, BasePlayer player);
    private object OnRecyclerToggle(Recycler recycler, BasePlayer player);
    private object OnTurretAuthorize(AutoTurret turret, BasePlayer player);
    private object OnTurretClearList(AutoTurret turret, BasePlayer player);
    private object OnTurretDeauthorize(AutoTurret turret, BasePlayer player);
    private object OnCollectiblePickup(Item item, BasePlayer player);
    private object OnGrowableGather(GrowableEntity plant, BasePlayer player);
    private object OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private object OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private object CanUpdateSign(BasePlayer player, Signage sign);
    private object CanLock(BasePlayer player, BaseLock baseLock);
    private object CanUnlock(BasePlayer player, BaseLock baseLock);
    private object OnCodeEntered(CodeLock codeLock, BasePlayer player, string code);
    private object CanChangeCode(CodeLock codeLock, BasePlayer player, string newCode, bool isGuestCode);
    private object CanPickupLock(BasePlayer player, BaseLock baseLock);
    private object OnCupboardAuthorize(BuildingPrivlidge privilege, BasePlayer player);
    private object OnCupboardClearList(BuildingPrivlidge privilege, BasePlayer player);
    private object OnCupboardDeauthorize(BuildingPrivlidge privilege, BasePlayer player);
    private object CanAdministerVending(BasePlayer player, VendingMachine machine);
    private object CanUseVending(VendingMachine machine, BasePlayer player);
    private object OnRotateVendingMachine(VendingMachine machine, BasePlayer player);
    private object CanCreateWorldProjectile(HitInfo info, ItemDefinition itemDef);
    private object OnReloadMagazine(BasePlayer player, BaseProjectile projectile);
    private object OnReloadWeapon(BasePlayer player, BaseProjectile projectile);
    private object OnSwitchAmmo(BasePlayer player, BaseProjectile projectile);
    private bool IsLimited(BasePlayer player);
    private void PrintDebug(string message);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Limited Admins (SteamID)",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ulong> Admins;
    [JsonProperty(PropertyName = "Limited Auth Levels",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<uint> LimitedAuthLevels;
    [JsonProperty(PropertyName = "Whitelisted Admins (SteamID)",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ulong> ExcludedAdmins;
    [JsonProperty(PropertyName = "Limit All Admins Exclude Whitelisted")]
    public bool LimitAll;
    [JsonProperty(PropertyName = "Blocked Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Blacklist;
    [JsonProperty(PropertyName = "Can Loot Entity")]
    public bool CanLootEntity;
    [JsonProperty(PropertyName = "Can Loot Player")]
    public bool CanLootPlayer;
    [JsonProperty(PropertyName = "Can Pickup Entity")]
    public bool CanPickupEntity;
    [JsonProperty(PropertyName = "Can Rename Bed")]
    public bool CanRenameBed;
    [JsonProperty(PropertyName = "Can Use Locked Entity")]
    public bool CanUseLockedEntity;
    [JsonProperty(PropertyName = "Can Interact With Lock")]
    public bool CanInteractLock;
    [JsonProperty(PropertyName = "Can Use Voice Chat")]
    public bool CanUseVoiceChat;
    [JsonProperty(PropertyName = "Can Be Targeted")]
    public bool CanBeTargeted;
    [JsonProperty(PropertyName = "Can Build")]
    public bool CanBuild;
    [JsonProperty(PropertyName = "Can Interact With Structure")]
    public bool CanInteractStructure;
    [JsonProperty(PropertyName = "Can Hack CH47 Crate")]
    public bool CanHackCrate;
    [JsonProperty(PropertyName = "Can Interact With Item")]
    public bool CanInteractItem;
    [JsonProperty(PropertyName = "Can Pickup Item")]
    public bool CanItemPickup;
    [JsonProperty(PropertyName = "Can Damage")]
    public bool CanDamage;
    [JsonProperty(PropertyName = "Can Use Lift")]
    public bool CanUseLift;
    [JsonProperty(PropertyName = "Can Toggle Oven")]
    public bool CanToggleOven;
    [JsonProperty(PropertyName = "Can Toggle Recycler")]
    public bool CanToggleRecycler;
    [JsonProperty(PropertyName = "Can Interact With Turret")]
    public bool CanInteractTurret;
    [JsonProperty(PropertyName = "Can Gather")]
    public bool CanGather;
    [JsonProperty(PropertyName = "Can Update Sign")]
    public bool CanUpdateSign;
    [JsonProperty(PropertyName = "Can Interact With Cupboard")]
    public bool CanInteractCupboard;
    [JsonProperty(PropertyName = "Can Interact With Vending Machine")]
    public bool CanInteractVending;
    [JsonProperty(PropertyName = "Can Interact With Weapons")]
    public bool CanInteractWeapons;
    [JsonProperty(PropertyName = "Debug")]
    public bool Debug;
}


```

---

## LimitedDroneHeight by WhiteThunder - Limits how high RC drones can be flown above terrain

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Serialization;
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("Limited Drone Height", "WhiteThunder", "1.0.0")]
[Description("Limits how high RC drones can be flown above terrain.")]
internal class LimitedDroneHeight : CovalencePlugin
{
    private static LimitedDroneHeight _pluginInstance;
    private static Configuration _pluginConfig;
    private const string PermissionProfilePrefix;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnBookmarkControlStarted(ComputerStation computerStation, BasePlayer player, string bookmarkName, Drone drone);
    private void OnBookmarkControlEnded(ComputerStation computerStation, BasePlayer player, Drone drone);
    private void OnDroneControlStarted(Drone drone);
    private void OnDroneControlEnded(Drone drone);
    private static bool LimitHeightWasBlocked(Drone drone);
    private bool IsDroneEligible(Drone drone);
    private static string GetProfilePermission(string profileSuffix);
    private class HeightLimiter : EntityComponent<Drone>
    {
        private const float DistanceFromMaxHeightToSlow;
        private const float DistanceToleranceAboveMaxHeight;
        public static void StartControl(Drone drone, int maxHeight);
        public static void StopControl(Drone drone);
        public static void RemoveFromDrone(Drone drone);
        private Transform _droneTransform;
        private int _maxHeight;
        private int _numControllers;
        private void Awake();
        private void OnControlStarted(int maxHeight);
        private void DelayedDestroy();
        private void OnControlStopped();
        private void LateUpdate();
    }

    private class HeightProfile
    {
        [JsonProperty("PermissionSuffix")]
        public string PermissionSuffix;
        [JsonProperty("MaxHeight")]
        public int MaxHeight;
        [JsonIgnore]
        public string Permission;
        public void Init(LimitedDroneHeight pluginInstance);
    }

    private class Configuration : SerializableConfiguration
    {
        [JsonProperty("DefaultMaxHeight")]
        public int DefaultMaxHeight;
        [JsonProperty("ProfilesRequiringPermission")]
        public HeightProfile[] ProfilesRequiringPermission;
        public void Init(LimitedDroneHeight pluginInstance);
        public int GetMaxHeightForDrone(Drone drone);
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

private class HeightLimiter : EntityComponent<Drone>
{
    private const float DistanceFromMaxHeightToSlow;
    private const float DistanceToleranceAboveMaxHeight;
    public static void StartControl(Drone drone, int maxHeight);
    public static void StopControl(Drone drone);
    public static void RemoveFromDrone(Drone drone);
    private Transform _droneTransform;
    private int _maxHeight;
    private int _numControllers;
    private void Awake();
    private void OnControlStarted(int maxHeight);
    private void DelayedDestroy();
    private void OnControlStopped();
    private void LateUpdate();
}

private class HeightProfile
{
    [JsonProperty("PermissionSuffix")]
    public string PermissionSuffix;
    [JsonProperty("MaxHeight")]
    public int MaxHeight;
    [JsonIgnore]
    public string Permission;
    public void Init(LimitedDroneHeight pluginInstance);
}

private class Configuration : SerializableConfiguration
{
    [JsonProperty("DefaultMaxHeight")]
    public int DefaultMaxHeight;
    [JsonProperty("ProfilesRequiringPermission")]
    public HeightProfile[] ProfilesRequiringPermission;
    public void Init(LimitedDroneHeight pluginInstance);
    public int GetMaxHeightForDrone(Drone drone);
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

## LimitedDroneRange by WhiteThunder - Limits how far RC drones can be controlled from computer stations

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using Network;
using UnityEngine;

Oxide.Plugins
[Info("Limited Drone Range", "WhiteThunder", "1.1.1")]
[Description("Limits how far RC drones can be controlled from computer stations.")]
internal class LimitedDroneRange : CovalencePlugin
{
    private readonly object False;
    private const float VanillaStaticDistanceFraction;
    private const int ForcedMaxRange;
    private const string DroneMaxControlRangeConVar;
    private readonly string ForcedMaxRangeString;
    private Configuration _config;
    private UIManager _uiManager;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private object OnBookmarkControl(ComputerStation station, BasePlayer player, string bookmarkName, Drone drone);
    private void OnBookmarkControlStarted(ComputerStation station, BasePlayer player, string bookmarkName, Drone drone);
    private void OnBookmarkControlEnded(ComputerStation station, BasePlayer player, Drone drone);
    private static bool LimitRangeWasBlocked(Drone drone, ComputerStation station, BasePlayer player);
    private static bool IsWithinRange(Drone drone, ComputerStation station, float range);
    private static void SendReplicatedVar(Connection connection, string fullName, string value);
    private static void SendMaxRangeConVar(BasePlayer player, string value);
    private bool ShouldLimitRange(Drone drone, ComputerStation station, BasePlayer player, int maxRange);
    private class RangeLimiter : FacepunchBehaviour
    {
        public static RangeLimiter AddToPlayer(LimitedDroneRange plugin, BasePlayer player, ComputerStation station, Drone drone, int maxDistance);
        public static void RemoveFromPlayer(BasePlayer player);
        public static void DestroyAll();
        private LimitedDroneRange _plugin;
        private BasePlayer _player;
        private ComputerStation _station;
        private Transform _stationTransform;
        private Transform _droneTransform;
        private int _maxDistance;
        private int _previousDisplayedDistance;
        private void CheckRange();
        public void OnDestroy();
    }

    private class UIManager
    {
        private const string UIName;
        private const string PlaceholderText;
        private const string PlaceholderColor;
        private string _cachedJson;
        public static void Destroy(BasePlayer player);
        private string GetJsonWithPlaceholders(LimitedDroneRange plugin);
        public void CreateDistanceUI(LimitedDroneRange plugin, BasePlayer player, int distance, int maxDistance);
        public void CreateOutOfRangeUI(LimitedDroneRange plugin, BasePlayer player);
        private void CreateLabel(LimitedDroneRange plugin, BasePlayer player, string text, string color);
    }

    private class RangeProfile
    {
        [JsonProperty("PermissionSuffix")]
        public string PermissionSuffix;
        [JsonProperty("MaxRange")]
        public int MaxRange;
        [JsonIgnore]
        public string Permission;
        public void Init(LimitedDroneRange plugin);
    }

    private class ColorConfig
    {
        [JsonProperty("DistanceRemaining")]
        public int DistanceRemaining;
        [JsonProperty("Color")]
        public string Color;
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
        [JsonProperty("TextSize")]
        public int TextSize;
        [JsonProperty("DefaultColor")]
        public string DefaultColor;
        [JsonProperty("OutOfRangeColor")]
        public string OutOfRangeColor;
        [JsonProperty("DynamicColors")]
        public ColorConfig[] DynamicColors;
        [JsonProperty("SecondsBetweenUpdates")]
        public float SecondsBetweenUpdates;
        public string GetDynamicColor(int distance, int maxDistance);
        public void Init();
    }

    private class Configuration : BaseConfiguration
    {
        [JsonProperty("DefaultMaxRange")]
        public int DefaultMaxRange;
        [JsonProperty("ProfilesRequiringPermission")]
        public RangeProfile[] ProfilesRequiringPermission;
        [JsonProperty("UISettings")]
        public UISettings UISettings;
        public void Init(LimitedDroneRange pluginInstance);
        public int GetMaxRangeForPlayer(LimitedDroneRange plugin, string userId);
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
    private class LangEntry
    {
        public static readonly List<LangEntry> AllLangEntries;
        public static readonly LangEntry UIOutOfRange;
        public static readonly LangEntry UIDistance;
        public string Name;
        public string English;
        public LangEntry(string name, string english);
    }

    private string GetMessage(string playerId, LangEntry langEntry);
    private string GetMessage(string playerId, LangEntry langEntry, object arg1, object arg2);
    protected override void LoadDefaultMessages();
}

private class RangeLimiter : FacepunchBehaviour
{
    public static RangeLimiter AddToPlayer(LimitedDroneRange plugin, BasePlayer player, ComputerStation station, Drone drone, int maxDistance);
    public static void RemoveFromPlayer(BasePlayer player);
    public static void DestroyAll();
    private LimitedDroneRange _plugin;
    private BasePlayer _player;
    private ComputerStation _station;
    private Transform _stationTransform;
    private Transform _droneTransform;
    private int _maxDistance;
    private int _previousDisplayedDistance;
    private void CheckRange();
    public void OnDestroy();
}

private class UIManager
{
    private const string UIName;
    private const string PlaceholderText;
    private const string PlaceholderColor;
    private string _cachedJson;
    public static void Destroy(BasePlayer player);
    private string GetJsonWithPlaceholders(LimitedDroneRange plugin);
    public void CreateDistanceUI(LimitedDroneRange plugin, BasePlayer player, int distance, int maxDistance);
    public void CreateOutOfRangeUI(LimitedDroneRange plugin, BasePlayer player);
    private void CreateLabel(LimitedDroneRange plugin, BasePlayer player, string text, string color);
}

private class RangeProfile
{
    [JsonProperty("PermissionSuffix")]
    public string PermissionSuffix;
    [JsonProperty("MaxRange")]
    public int MaxRange;
    [JsonIgnore]
    public string Permission;
    public void Init(LimitedDroneRange plugin);
}

private class ColorConfig
{
    [JsonProperty("DistanceRemaining")]
    public int DistanceRemaining;
    [JsonProperty("Color")]
    public string Color;
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
    [JsonProperty("TextSize")]
    public int TextSize;
    [JsonProperty("DefaultColor")]
    public string DefaultColor;
    [JsonProperty("OutOfRangeColor")]
    public string OutOfRangeColor;
    [JsonProperty("DynamicColors")]
    public ColorConfig[] DynamicColors;
    [JsonProperty("SecondsBetweenUpdates")]
    public float SecondsBetweenUpdates;
    public string GetDynamicColor(int distance, int maxDistance);
    public void Init();
}

private class Configuration : BaseConfiguration
{
    [JsonProperty("DefaultMaxRange")]
    public int DefaultMaxRange;
    [JsonProperty("ProfilesRequiringPermission")]
    public RangeProfile[] ProfilesRequiringPermission;
    [JsonProperty("UISettings")]
    public UISettings UISettings;
    public void Init(LimitedDroneRange pluginInstance);
    public int GetMaxRangeForPlayer(LimitedDroneRange plugin, string userId);
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
    public static readonly List<LangEntry> AllLangEntries;
    public static readonly LangEntry UIOutOfRange;
    public static readonly LangEntry UIDistance;
    public string Name;
    public string English;
    public LangEntry(string name, string english);
}


```

---

## LimitedLadders by redBDGR - Prevents the placement of ladders where building is blocked

```csharp

Oxide.Plugins
[Info("LimitedLadders", "redBDGR", "2.0.1")]
[Description("Prevents the placement of ladders where building is blocked")]
 class LimitedLadders : RustPlugin
{
    private void Unload();
    private void OnServerInitialized();
    private void AllowBuildingBypass(bool allow);
}


```

---

## LimitedSmokeGrenade by Lincoln - Adjust the amount of time smoke grenades last for

```csharp
using Newtonsoft.Json;

Oxide.Plugins
[Info("Limited Smoke Grenade", "Lincoln", "1.0.3")]
[Description("Limit the smoke grenade duration time.")]
public class LimitedSmokeGrenade : RustPlugin
{
    private const string permUse;
    private void Init();
    private void OnExplosiveThrown(BasePlayer player, SmokeGrenade smokeGrenade);
    private void OnExplosiveDropped(BasePlayer player, SmokeGrenade smokeGrenade);
    private void OnRocketLaunched(BasePlayer player, SmokeGrenade smokeGrenade);
    private void SmokeGrenade(BasePlayer player, SmokeGrenade smokeGrenade);
    private class PluginConfig
    {
        [JsonProperty("Smoke Duration (seconds)")]
        public int SmokeDurationInSeconds;
    }

    private PluginConfig config;
    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
}

private class PluginConfig
{
    [JsonProperty("Smoke Duration (seconds)")]
    public int SmokeDurationInSeconds;
}


```

---

## LimitRCON by Lorenc - Limits RCON access to specific IP addresses

```csharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net;

Oxide.Plugins
[Info("Limit RCON", "Wulf", "1.0.0")]
[Description("Limits RCON access to specific IP addresses")]
 class LimitRCON : CovalencePlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty("Allow local IP addresses")]
        public bool AllowLocal;
        [JsonProperty("List of allowed IP addresses to allow", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> IpAddresses;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private object OnRconConnection(IPAddress ipAddress);
    private static bool IsLocalIp(string ipAddress);
}

public class Configuration
{
    [JsonProperty("Allow local IP addresses")]
    public bool AllowLocal;
    [JsonProperty("List of allowed IP addresses to allow", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> IpAddresses;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## LoadingMessages by klauz24 - Shows your own customizable texts on the loading screen

```csharp
using Network;
using System.Linq;
using Newtonsoft.Json;
using System.Collections.Generic;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Loading Messages", "CosaNostra/Def/klauz24", "1.1.3")]
[Description("Shows custom texts on loading screen")]
public class LoadingMessages : RustPlugin
{
    private readonly Dictionary<ulong, Connection> _clients;
    private readonly List<ulong> _disconnectedClients;
    private static MsgConfig _config;
    private Timer _timer;
    private List<Connection> _queueConnections;
    private static MsgCollection _messages;
    private static MsgCollection _messagesQueue;
    private class MsgCollection
    {
        public List<MsgEntry> MessagesList;
        public MsgEntry CurrentMessage;
        private int _messageIndex;
        public void AdvanceMessage();
        public void SelectFirst();
    }

    private class MsgConfig
    {
        [JsonProperty("Cycle Messages Every ~N Seconds")]
        public float CyclicityFreq;
        [JsonProperty("Enable Messages Cyclicity")]
        public bool EnableCyclicity;
        [JsonProperty("Use Random Cyclicity (Instead of sequential)")]
        public bool EnableRandomCyclicity;
        [JsonProperty("Messages")]
        public List<MsgEntry> Msgs;
        [JsonProperty("Enable Queue Messages")]
        public bool EnableQueueMessages;
        [JsonProperty("Queue Messages")]
        public List<MsgEntry> QueueMsgs;
        [JsonProperty("Last Message (When entering game)")]
        public MsgEntry LastMessage;
    }

    private class MsgEntry
    {
        [JsonProperty("Icon name")]
        public string IconName;
        [JsonProperty("Message")]
        public string Message;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Unload();
    private void Loaded();
    private void OnServerInitialized();
    private void OnUserApprove(Connection connection);
    private void OnPlayerConnected(BasePlayer player);
    private void HandleClients();
    private void SendPacket(Connection conn, MsgEntry entry);
    private static bool IsValidStr(string str);
    private static T PickRandom(IReadOnlyList<T> list);
    private static MsgEntry GetCurrentMessage();
    private static MsgEntry GetLastMessage();
    private static MsgEntry GetCurrentQueueMessage();
    private static MsgCollection GetMessagesCollection();
    private static MsgCollection GetQueueMessagesCollection();
    private static void UpdateCurrentMessages();
    private int GetQueuePosition(Connection con);
    private static void SuppressDefaultQueueMessage();
}

private class MsgCollection
{
    public List<MsgEntry> MessagesList;
    public MsgEntry CurrentMessage;
    private int _messageIndex;
    public void AdvanceMessage();
    public void SelectFirst();
}

private class MsgConfig
{
    [JsonProperty("Cycle Messages Every ~N Seconds")]
    public float CyclicityFreq;
    [JsonProperty("Enable Messages Cyclicity")]
    public bool EnableCyclicity;
    [JsonProperty("Use Random Cyclicity (Instead of sequential)")]
    public bool EnableRandomCyclicity;
    [JsonProperty("Messages")]
    public List<MsgEntry> Msgs;
    [JsonProperty("Enable Queue Messages")]
    public bool EnableQueueMessages;
    [JsonProperty("Queue Messages")]
    public List<MsgEntry> QueueMsgs;
    [JsonProperty("Last Message (When entering game)")]
    public MsgEntry LastMessage;
}

private class MsgEntry
{
    [JsonProperty("Icon name")]
    public string IconName;
    [JsonProperty("Message")]
    public string Message;
}


```

---

## Loadoutless by  - Saves player loadouts and loads them on spawn

```csharp
using Oxide.Core;
using System;
using System.Collections;
using Newtonsoft.Json;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Loadoutless", "Khan", "2.1.2")]
[Description("Players can respawn with a loadout")]
public class Loadoutless : RustPlugin
{
    private const string Default;
    private const string Use;
    private const string Admin;
    private const string FileBanlist;
    private const string FileMain;
    private static Loadoutless _instance;
    private Dictionary<string, PlayerLoadout> _playerLoadouts;
    private PluginConfig _config;
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void CheckConfig();
    public class Limit
    {
        public string DisplayName;
        public double Amount;
    }

    private class PluginConfig
    {
        [JsonProperty("Set Max Player Loadouts", Order = 1)]
        public int MaxLoadouts;
        [JsonProperty("Enable Item Limits", Order = 2)]
        public bool EnableLimits;
        [JsonProperty("Enable Auto Update Limits", Order = 3)]
        public bool EnableAutoLimits;
        [JsonProperty(Order = 4)]
        public List<LoadoutItem> DefaultLoadout;
        [JsonProperty("Item Loadout Limits Defaults to your Servers Stack Size", Order = 5)]
        public Dictionary<string, Limit> Limits;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private Coroutine _coroutine;
    private IEnumerator ResetPlayersLoadout();
    private IEnumerator ClearPlayersLoadouts();
    public string RemoveFilePath(string value);
    public void AddInvBan(Item[] invItems);
     void UpdateBanFile(List<string> ban_list);
     bool CheckBanFile(string itemid);
     List<string> RemoveItemBanlist(List<string> banfile, string itemid);
     List<string> LoadBanFile();
    private static List<LoadoutItem> GetPlayerLoadout(BasePlayer player, bool admin);
    private static LoadoutItem ProcessItem(Item item, string container);
    private static Item BuildWeapon(int id, ulong skin, int Ammotype, int ammoAmount, List<int> mods);
    private static Item BuildItem(int itemid, int amount, ulong skin);
    private static Item CreateByItemID(int itemID, int amount, ulong skin);
    private static bool GiveItem(PlayerInventory inv, Item item, ItemContainer container, int position);
     List<LoadoutItem> CheckBannedList(List<LoadoutItem> list);
    public object GiveLoadout(BasePlayer player);
    public void SendMessage(BasePlayer player, string message);
    public class PlayerLoadout
    {
        public ulong ID;
        public string Name;
        public string Loadout;
        public Dictionary<string, List<LoadoutItem>> AvailableLoadouts;
        internal static void TryLoad(BasePlayer player);
        internal void Update(BasePlayer player);
        internal void Save();
        internal static PlayerLoadout Find(BasePlayer player);
        internal object GiveLoadout(BasePlayer player);
    }

    public class LoadoutItem
    {
        public int Itemid;
        public bool Bp;
        public ulong Skinid;
        public string Container;
        public int Slot;
        public int Amount;
        public bool Weapon;
        public int Ammotype;
        public int AmmoAmount;
        public List<int> Mods;
    }

    [ChatCommand("loadout")]
    private void LoadoutLessCommand(BasePlayer player, string command, string[] args);
}

public class Limit
{
    public string DisplayName;
    public double Amount;
}

private class PluginConfig
{
    [JsonProperty("Set Max Player Loadouts", Order = 1)]
    public int MaxLoadouts;
    [JsonProperty("Enable Item Limits", Order = 2)]
    public bool EnableLimits;
    [JsonProperty("Enable Auto Update Limits", Order = 3)]
    public bool EnableAutoLimits;
    [JsonProperty(Order = 4)]
    public List<LoadoutItem> DefaultLoadout;
    [JsonProperty("Item Loadout Limits Defaults to your Servers Stack Size", Order = 5)]
    public Dictionary<string, Limit> Limits;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

public class PlayerLoadout
{
    public ulong ID;
    public string Name;
    public string Loadout;
    public Dictionary<string, List<LoadoutItem>> AvailableLoadouts;
    internal static void TryLoad(BasePlayer player);
    internal void Update(BasePlayer player);
    internal void Save();
    internal static PlayerLoadout Find(BasePlayer player);
    internal object GiveLoadout(BasePlayer player);
}

public class LoadoutItem
{
    public int Itemid;
    public bool Bp;
    public ulong Skinid;
    public string Container;
    public int Slot;
    public int Amount;
    public bool Weapon;
    public int Ammotype;
    public int AmmoAmount;
    public List<int> Mods;
}


```

---

## Localize by MrBlue - Generates localization files for other languages using existing plugin localization

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Localize", "Wulf", "1.0.1")]
[Description("Generates localization files for other languages using existing plugin localization")]
public class Localize : CovalencePlugin
{
    protected override void LoadDefaultMessages();
    [PluginReference]
    private Plugin TranslationAPI;
    private const string permUse;
    private void OnServerInitialized();
    private void CommandLocalize(IPlayer player, string command, string[] args);
    private void AddLocalizedCommand(string command);
    private string GetLang(string langKey, string playerId, object[] args);
    private void Message(IPlayer player, string textOrLang, object[] args);
    private void MigratePermission(string oldPerm, string newPerm);
}


```

---

## LocalTimeDamageControl by CARNY666 - Allows admin to control when damage can be done by non-owner players to using the local server time.

```csharp
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using System.Collections;
using System.Globalization;

Oxide.Plugins
[Info("LocalTimeDamageControl", "CARNY666", "1.0.11", ResourceId = 2720)]
 class LocalTimeDamageControl : RustPlugin
{
    const string adminPriv;
     bool activated;
     void Init();
     void Loaded();
    protected override void LoadDefaultConfig();
    public bool IsDamageControlOn();
    public DateTime getStartTime();
    public DateTime getEndTime();
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    [ChatCommand("lset")]
    private void lset(BasePlayer player, string command, string[] args);
     string ShowTime(object TimeIn);
     Dictionary<string, string> get();
}


```

---

## LockableShutters by OG61 - Allows players to place a lock on shutters

```csharp
using System;

Oxide.Plugins
[Info("Lockable Shutters", "OG61", "1.1.1")]
[Description("Allows players to place locks on shutters")]
public class LockableShutters : CovalencePlugin
{
    const string _perm;
    private void OnServerInitialized();
    private void CheckDoors();
    private void Init();
    private void OnEntitySpawned(Door door);
     void OnUserPermissionGranted(string id, string permName);
     void OnUserPermissionRevoked(string id, string permName);
}


```

---

## LockedCrateTimer by Cltdj - Allows you to edit the amount time it takes to unlock hackable crates.

```csharp
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;

Oxide.Plugins
[Info("Hackable Crate Time Editor", "Cltdj", "0.2.8")]
[Description("Allows you to edit the amount time it takes to unlock locked crates.")]
 class LockedCrateTimer : CovalencePlugin
{
    protected override void LoadDefaultConfig();
    const float TOTAL_TIME;
    public int HackingSeconds;
    public bool CheckTimer;
    private void Init();
    private void LoadConfig();
     void OnCrateHack(HackableLockedCrate crate);
    [Command("lockedcratetimer.conf"), Permission("lockedcratetimer.conf.use")]
    private void TimeCommand(IPlayer player, string command, string[] args);
}


```

---

## LockMaster by FastBurst - Lock your storage and deployables (furnaces, refineries, and more)

```csharp
using UnityEngine;
using System;
using System.Collections.Generic;
using System.Globalization;
using Oxide.Core;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Lock Master", "FastBurst", "1.1.8")]
[Description("Lock all your storages and deployables")]
 class LockMaster : RustPlugin
{
    private static bool isPlayer(ulong id);
    private const string COMPOSTER_PREFAB;
    private const string DROPBOX_PREFAB;
    private const string VENDING_PREFAB;
    private const string FURNACE_PREFAB;
    private const string LARGE_FURNACE_PREFAB;
    private const string REFINERY_PREFAB;
    private const string BBQ_PREFAB;
    private const string SMALL_PLANTER_PREFAB;
    private const string LARGE_PLANTER_PREFAB;
    private const string STATIC_REFINERY_PREFAB;
    private const string STATIC_BBQ_PREFAB;
    private const string HITCH_PREFAB;
    private const string MIXINGTABLE_PREFAB;
    public static LockMaster Instance;
    private void Init();
    private void CmdRefresh(BasePlayer player, string command, string[] args);
    private void OnServerInitialized();
    private void OnEntitySpawned(BaseNetworkable entity);
    private static ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "General Options")]
        public GeneralOptions GeneralSettings { get; set; }
        [JsonProperty(PropertyName = "Configuration")]
        public ConfigOptions ConfigSettings { get; set; }
        public class GeneralOptions
        {
            [JsonProperty(PropertyName = "Command")]
            public string commandOption { get; set; }
            [JsonProperty(PropertyName = "Permission Name")]
            public string permissionName { get; set; }
        }

        public class ConfigOptions
        {
            [JsonProperty(PropertyName = "Enable Locks on Composters")]
            public bool lockComposter { get; set; }
            [JsonProperty(PropertyName = "Enable Locks on DropBoxes")]
            public bool lockDropBox { get; set; }
            [JsonProperty(PropertyName = "Enable Locks on Vending Machines")]
            public bool lockVending { get; set; }
            [JsonProperty(PropertyName = "Enable Locks on Ovens (Furnaces, BBQ Grills, Small Oil Refineries)")]
            public bool lockOvens { get; set; }
            [JsonProperty(PropertyName = "Enable Locks on Small & Large Planters")]
            public bool lockPlanter { get; set; }
            [JsonProperty(PropertyName = "Enable Locks on Hitch & Trough")]
            public bool lockHitch { get; set; }
            [JsonProperty(PropertyName = "Enable Locks on Mixing Table")]
            public bool lockMixing { get; set; }
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
    private string Lang(string key, string id, object[] args);
    private void Message(BasePlayer player, string key, object[] args);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "General Options")]
    public GeneralOptions GeneralSettings { get; set; }
    [JsonProperty(PropertyName = "Configuration")]
    public ConfigOptions ConfigSettings { get; set; }
    public class GeneralOptions
    {
        [JsonProperty(PropertyName = "Command")]
        public string commandOption { get; set; }
        [JsonProperty(PropertyName = "Permission Name")]
        public string permissionName { get; set; }
    }

    public class ConfigOptions
    {
        [JsonProperty(PropertyName = "Enable Locks on Composters")]
        public bool lockComposter { get; set; }
        [JsonProperty(PropertyName = "Enable Locks on DropBoxes")]
        public bool lockDropBox { get; set; }
        [JsonProperty(PropertyName = "Enable Locks on Vending Machines")]
        public bool lockVending { get; set; }
        [JsonProperty(PropertyName = "Enable Locks on Ovens (Furnaces, BBQ Grills, Small Oil Refineries)")]
        public bool lockOvens { get; set; }
        [JsonProperty(PropertyName = "Enable Locks on Small & Large Planters")]
        public bool lockPlanter { get; set; }
        [JsonProperty(PropertyName = "Enable Locks on Hitch & Trough")]
        public bool lockHitch { get; set; }
        [JsonProperty(PropertyName = "Enable Locks on Mixing Table")]
        public bool lockMixing { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class GeneralOptions
{
    [JsonProperty(PropertyName = "Command")]
    public string commandOption { get; set; }
    [JsonProperty(PropertyName = "Permission Name")]
    public string permissionName { get; set; }
}

public class ConfigOptions
{
    [JsonProperty(PropertyName = "Enable Locks on Composters")]
    public bool lockComposter { get; set; }
    [JsonProperty(PropertyName = "Enable Locks on DropBoxes")]
    public bool lockDropBox { get; set; }
    [JsonProperty(PropertyName = "Enable Locks on Vending Machines")]
    public bool lockVending { get; set; }
    [JsonProperty(PropertyName = "Enable Locks on Ovens (Furnaces, BBQ Grills, Small Oil Refineries)")]
    public bool lockOvens { get; set; }
    [JsonProperty(PropertyName = "Enable Locks on Small & Large Planters")]
    public bool lockPlanter { get; set; }
    [JsonProperty(PropertyName = "Enable Locks on Hitch & Trough")]
    public bool lockHitch { get; set; }
    [JsonProperty(PropertyName = "Enable Locks on Mixing Table")]
    public bool lockMixing { get; set; }
}


```

---

## Logger by MrBlue - Configurable logging of chat, commands, connections, and more

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Net;

Oxide.Plugins
[Info("Logger", "Wulf/lukespragg", "2.2.2")]
[Description("Configurable logging of chat, commands, connections, and more")]
public class Logger : CovalencePlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Log chat messages (true/false)")]
        public bool LogChat { get; set; }
        [JsonProperty(PropertyName = "Log command usage (true/false)")]
        public bool LogCommands { get; set; }
        [JsonProperty(PropertyName = "Log player connections (true/false)")]
        public bool LogConnections { get; set; }
        [JsonProperty(PropertyName = "Log player disconnections (true/false)")]
        public bool LogDisconnections { get; set; }
        [JsonProperty(PropertyName = "Log player respawns (true/false)")]
        public bool LogRespawns { get; set; }
        [JsonProperty(PropertyName = "Log output to console (true/false)")]
        public bool LogToConsole { get; set; }
        [JsonProperty(PropertyName = "Rotate logs daily (true/false)")]
        public bool RotateLogs { get; set; }
        [JsonProperty(PropertyName = "Command list (full or short commands)")]
        public List<string> CommandList { get; set; }
        [JsonProperty(PropertyName = "Command list type (blacklist or whitelist)")]
        public string CommandListType { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private new void LoadDefaultMessages();
    private const string commandReason;
    private const string permReason;
    private void Init();
    private void OnUserChat(IPlayer player, string message);
    private void OnUserConnected(IPlayer player);
    private void OnUserDisconnected(IPlayer player);
    private void OnUserRespawned(IPlayer player);
    private void OnUserCommand(IPlayer player, string command, string[] args);
    private void ReasonCommand(IPlayer player, string command, string[] args);
    private string Lang(string key, string id, object[] args);
    private void AddLocalizedCommand(string key, string command);
    private void Log(string filename, string key, object[] args);
    private void Message(IPlayer player, string key, object[] args);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Log chat messages (true/false)")]
    public bool LogChat { get; set; }
    [JsonProperty(PropertyName = "Log command usage (true/false)")]
    public bool LogCommands { get; set; }
    [JsonProperty(PropertyName = "Log player connections (true/false)")]
    public bool LogConnections { get; set; }
    [JsonProperty(PropertyName = "Log player disconnections (true/false)")]
    public bool LogDisconnections { get; set; }
    [JsonProperty(PropertyName = "Log player respawns (true/false)")]
    public bool LogRespawns { get; set; }
    [JsonProperty(PropertyName = "Log output to console (true/false)")]
    public bool LogToConsole { get; set; }
    [JsonProperty(PropertyName = "Rotate logs daily (true/false)")]
    public bool RotateLogs { get; set; }
    [JsonProperty(PropertyName = "Command list (full or short commands)")]
    public List<string> CommandList { get; set; }
    [JsonProperty(PropertyName = "Command list type (blacklist or whitelist)")]
    public string CommandListType { get; set; }
}


```

---

## LootBouncer by VisEntities - Empty the containers when players do not pick up all the items

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Rust;

Oxide.Plugins
[Info("Loot Bouncer", "Sorrow/Arainrr", "1.0.10")]
[Description("Empty the containers when players do not pick up all the items")]
public class LootBouncer : RustPlugin
{
    [PluginReference]
    private Plugin Slap;
    private Plugin Trade;
    private readonly Dictionary<ulong, int> _lootEntities;
    private readonly Dictionary<ulong, HashSet<ulong>> _entityPlayers;
    private readonly Dictionary<ulong, Timer> _lootDestroyTimer;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnLootEntity(BasePlayer player, LootContainer lootContainer);
    private void OnLootEntityEnd(BasePlayer player, LootContainer lootContainer);
    private void OnPlayerAttack(BasePlayer attacker, HitInfo info);
    private void OnEntityDeath(LootContainer barrel, HitInfo info);
    private void OnEntityKill(LootContainer lootContainer);
    private static bool IsBarrel(string shortPrefabName);
    private void DropItems(LootContainer lootContainer);
    private void EmptyJunkPile(LootContainer lootContainer);
    private void UpdateConfig();
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Time before the loot containers are empties (seconds)")]
        public float timeBeforeLootEmpty;
        [JsonProperty(PropertyName = "Empty the entire junkpile when automatically empty loot")]
        public bool emptyJunkpile;
        [JsonProperty(PropertyName = "Empty the nearby loot when emptying junkpile")]
        public bool dropNearbyLoot;
        [JsonProperty(PropertyName = "Time before the junkpile are empties (seconds)")]
        public float timeBeforeJunkpileEmpty;
        [JsonProperty(PropertyName = "Slaps players who don't empty containers")]
        public bool slapPlayer;
        [JsonProperty(PropertyName = "Remove instead bouncing")]
        public bool removeItems;
        [JsonProperty(PropertyName = "Chat Settings")]
        public ChatSettings chat;
        public class ChatSettings
        {
            [JsonProperty(PropertyName = "Chat Prefix")]
            public string prefix;
            [JsonProperty(PropertyName = "Chat SteamID Icon")]
            public ulong steamIDIcon;
        }

        [JsonProperty(PropertyName = "Loot container settings")]
        public Dictionary<string, bool> lootContainers;
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
    [JsonProperty(PropertyName = "Time before the loot containers are empties (seconds)")]
    public float timeBeforeLootEmpty;
    [JsonProperty(PropertyName = "Empty the entire junkpile when automatically empty loot")]
    public bool emptyJunkpile;
    [JsonProperty(PropertyName = "Empty the nearby loot when emptying junkpile")]
    public bool dropNearbyLoot;
    [JsonProperty(PropertyName = "Time before the junkpile are empties (seconds)")]
    public float timeBeforeJunkpileEmpty;
    [JsonProperty(PropertyName = "Slaps players who don't empty containers")]
    public bool slapPlayer;
    [JsonProperty(PropertyName = "Remove instead bouncing")]
    public bool removeItems;
    [JsonProperty(PropertyName = "Chat Settings")]
    public ChatSettings chat;
    public class ChatSettings
    {
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string prefix;
        [JsonProperty(PropertyName = "Chat SteamID Icon")]
        public ulong steamIDIcon;
    }

    [JsonProperty(PropertyName = "Loot container settings")]
    public Dictionary<string, bool> lootContainers;
    [JsonProperty(PropertyName = "Version")]
    public VersionNumber version;
}

public class ChatSettings
{
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string prefix;
    [JsonProperty(PropertyName = "Chat SteamID Icon")]
    public ulong steamIDIcon;
}


```

---

## LootChecker by Rustoholics - View a summary of loot in your crates and barrels

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;

Oxide.Plugins
[Info("Loot Checker", "Rustoholics", "0.1.0")]
[Description("View a summary of all the loot in crates and barrels")]
public class LootChecker : CovalencePlugin
{
    private int DefaultPrefab;
    private List<string> _prefabs;
    private void Init();
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
    private string GetPrefabName(string prefab);
    private static string ToFixedLength(object obj, int length, char pad);
    [Command("loot.help"), Permission("lootchecker.use")]
    private void ListPrefabsCommand(IPlayer iplayer, string command, string[] args);
    [Command("loot.view"), Permission("lootchecker.use")]
    private void ViewLootCommand(IPlayer iplayer, string command, string[] args);
}


```

---

## LootDefender by nivex - Defends loot from other players who dealt less damage than you

```csharp
using Facepunch;
using Facepunch.Math;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using UnityEngine;

Oxide.Plugins
[Info("Loot Defender", "Author Egor Blagov, Maintainer nivex", "2.2.4")]
[Description("Defends loot from other players who dealt less damage than you.")]
internal class LootDefender : RustPlugin
{
    [PluginReference]
     Plugin PersonalHeli;
     Plugin Friends;
     Plugin Clans;
     Plugin RustRewards;
     Plugin HeliSignals;
     Plugin BradleyDrops;
     Plugin HelpfulSupply;
     Plugin ShoppyStock;
     Plugin XLevels;
     Plugin XPerience;
     Plugin SkillTree;
    private static LootDefender Instance;
    private static StringBuilder sb;
    private const ulong supplyDropSkinID;
    private const string bypassLootPerm;
    private const string bypassDamagePerm;
    private const string bypassLockoutsPerm;
    private Dictionary<ulong, List<DamageKey>> _apcAttackers;
    private Dictionary<ulong, List<DamageKey>> _heliAttackers;
    private Dictionary<ulong, ulong> _locked { get; set; }
    private List<ulong> _personal { get; set; }
    private List<ulong> _boss { get; set; }
    private List<string> _sent { get; set; }
    private static StoredData data { get; set; }
    private MonumentInfo launchSite { get; set; }
    private List<MonumentInfo> harbors { get; set; }
    private List<ulong> ownerids;
    public class Lockout
    {
        public double Bradley { get; set; }
        public double Heli { get; set; }
        public bool Any();
    }

    private class StoredData
    {
        public Dictionary<string, Lockout> Lockouts { get; set; }
        public Dictionary<string, UI.Info> UI { get; set; }
        [JsonProperty(PropertyName = "DamageInfo")]
        public Dictionary<ulong, DamageInfo> Damage { get; set; }
        [JsonProperty(PropertyName = "LockInfo")]
        public Dictionary<ulong, LockInfo> Lock { get; set; }
        public void Sanitize();
    }

    private class DamageEntry
    {
        public float DamageDealt { get; set; }
        public DateTime Timestamp { get; set; }
        public string TeamID { get; set; }
        public DamageEntry();
        public DamageEntry(ulong teamID);
        public bool IsOutdated(int timeout);
    }

    private class DamageKey
    {
        public ulong userid { get; set; }
        public string name { get; set; }
        public DamageEntry damageEntry { get; set; }
        internal BasePlayer attacker { get; set; }
        public DamageKey();
        public DamageKey(BasePlayer attacker);
    }

    private class DamageInfo
    {
        public List<DamageKey> damageKeys { get; set; }
        [JsonIgnore]
        public Dictionary<ulong, BasePlayer> interact { get; set; }
        private List<ulong> participants { get; set; }
        public DamageEntryType damageEntryType { get; set; }
        public string NPCName { get; set; }
        public ulong OwnerID { get; set; }
        public ulong SkinID { get; set; }
        public DateTime start { get; set; }
        internal int _lockTime { get; set; }
        [JsonIgnore]
        internal BaseEntity _entity { get; set; }
        internal Vector3 _position { get; set; }
        internal Vector3 lastAttackedPosition { get; set; }
        internal ulong _uid { get; set; }
        [JsonIgnore]
        internal Timer _timer { get; set; }
        internal List<DamageKey> keys { get; set; }
         List<DamageGroup> damageGroups;
        internal float FullDamage { get; set; }
        public DamageInfo();
        public DamageInfo(DamageEntryType damageEntryType, string NPCName, BaseEntity entity, DateTime start);
        public void Start();
        public void DestroyTimer();
        private void CheckExpiration();
        public void Unlock();
        private void Lock(BaseEntity entity, ulong id);
        public void AddDamage(BaseCombatEntity entity, BasePlayer attacker, DamageEntry entry, float amount);
        public DamageEntry TryGet(ulong id);
        public DamageEntry Get(BasePlayer attacker);
        public bool isKilled;
        public void OnKilled(Vector3 position, HitInfo hitInfo, float distance);
        private void FindLooters(Vector3 position, HitInfo hitInfo, float distance);
        public void DisplayDamageReport();
        private bool CanDisplayReport(BasePlayer target);
        public void SetCanInteract();
        public string GetDamageReport(ulong targetId);
        public bool IsParticipant(ulong userid, BasePlayer player);
        public bool CanInteract(ulong userid, BasePlayer player);
        private List<DamageGroup> GetTopDamageGroups(List<DamageGroup> damageGroups, DamageEntryType damageEntryType);
        private List<DamageGroup> GetDamageGroups();
    }

    private class LockInfo
    {
        public DamageInfo damageInfo { get; set; }
        private DateTime LockTimestamp { get; set; }
        private int LockTimeout { get; set; }
        internal bool IsLockOutdated { get; set; }
        public LockInfo();
        public LockInfo(DamageInfo damageInfo, int lockTimeout);
        public bool CanInteract(ulong userId, BasePlayer target);
        public string GetDamageReport(ulong userId);
    }

    private class DamageGroup
    {
        public float TotalDamage { get; set; }
        public DamageKey FirstDamagerDealer { get; set; }
        private List<ulong> additionalPlayers { get; set; }
        [JsonIgnore]
        public List<ulong> Players { get; set; }
        public DamageGroup();
        public DamageGroup(DamageKey x);
        public string ToReport(DamageKey damageKey, DamageInfo damageInfo);
    }

    private void OnServerSave();
    private void Init();
    private void OnServerInitialized(bool serverinit);
    private bool IsF15EventActive;
    private void OnEventTrigger(TriggeredEventPrefab prefab);
    private void Unload();
    private void OnPlayerSleepEnded(BasePlayer player);
    private object OnTeamLeave(RelationshipManager.PlayerTeam team, BasePlayer player);
    private object OnTeamKick(RelationshipManager.PlayerTeam team, BasePlayer player, ulong targetId);
    private object OnPlayerAttack(BasePlayer attacker, HitInfo hitInfo);
    private object OnEntityTakeDamage(PatrolHelicopter heli, HitInfo hitInfo);
    private object OnEntityTakeDamage(BradleyAPC apc, HitInfo hitInfo);
    private object OnEntityTakeDamage(BasePlayer player, HitInfo hitInfo);
    private object OnEntityTakeDamageHandler(BaseCombatEntity entity, HitInfo hitInfo, DamageEntryType damageEntryType, string npcName);
    public static bool IsKilled(BaseNetworkable a);
    private bool BlockDamage(DamageEntryType damageEntryType);
    private object CanBradleyTakeDamage(BradleyAPC apc, HitInfo hitInfo);
    private object CanHeliTakeDamage(PatrolHelicopter heli, HitInfo hitInfo);
    private void OnEntityDeath(PatrolHelicopter heli, HitInfo hitInfo);
    private void OnEntityKill(PatrolHelicopter heli);
    private void OnEntityDeath(BradleyAPC apc, HitInfo hitInfo);
    private void OnEntityDeath(BasePlayer player, HitInfo hitInfo);
    private void OnEntityDeath(NPCPlayerCorpse corpse, HitInfo hitInfo);
    private void OnEntityKill(NPCPlayerCorpse corpse);
    private bool IsInBounds(MonumentInfo monument, Vector3 target);
    private bool CanLockBradley(BaseEntity entity);
    private bool CanLockHeli(BaseCombatEntity entity);
    private bool CanLockNpc(BaseEntity entity);
    private void OnEntityDeathHandler(BaseCombatEntity entity, DamageEntryType damageEntryType, HitInfo hitInfo);
     void GiveRustReward(BaseEntity entity, DamageInfo damageInfo, ulong userid, string weapon, int total);
     void GiveXpReward(BaseEntity entity, DamageInfo damageInfo, ulong userid, string weapon, float distance, int total);
     void GiveShopReward(BaseEntity entity, DamageInfo damageInfo, ulong userid, string weapon, float distance, int total);
    private static void ApplyWeaponMultiplierReward(DamageInfo damageInfo, string weapon, double amount, float distance);
    private object OnAutoPickupEntity(BasePlayer player, BaseEntity entity);
    private object CanLootEntity(BasePlayer player, DroppedItemContainer container);
    private object CanLootEntity(BasePlayer player, LootableCorpse corpse);
    private object CanLootEntity(BasePlayer player, StorageContainer container);
    private object CanLootEntityHandler(BasePlayer player, BaseEntity entity);
    private void OnBossSpawn(ScientistNPC boss);
    private void OnBossKilled(ScientistNPC boss, BasePlayer attacker);
    private void OnPersonalHeliSpawned(BasePlayer player, PatrolHelicopter heli);
    private void OnPersonalApcSpawned(BasePlayer player, BradleyAPC apc);
    private void OnEntitySpawned(CH47Helicopter heli);
    private void OnExplosiveDropped(BasePlayer player, SupplySignal ss, ThrownWeapon tw);
    private void OnExplosiveThrown(BasePlayer player, SupplySignal ss, ThrownWeapon tw);
    private List<ulong> crateLock;
    private List<ulong> thrown;
    private void Explode(SupplySignal ss, ulong userid, Vector3 position, string resourcePath, BasePlayer player);
    private void DelayedDestroySupplyDrop(SupplyDrop drop);
    private void OnExcavatorSuppliesRequested(ExcavatorSignalComputer computer, BasePlayer player, CargoPlane plane);
    private void OnRandomRaidWin(SupplyDrop drop, List<ulong> playerID);
    private void OnCargoPlaneSignaled(CargoPlane plane, SupplySignal ss);
    private void SetupCargoPlane(CargoPlane plane, BaseEntity entity, ulong userid);
    private void OnSupplyDropDropped(SupplyDrop drop, CargoPlane plane);
    private void OnSupplyDropLanded(SupplyDrop drop);
    private List<CargoPlane> cargoPlanes;
    private void OnEntitySpawned(SupplyDrop drop);
    private void OnHelpfulSupplyDropped(SupplyDrop drop);
    private void OnGuardedCrateEventEnded(BasePlayer player, HackableLockedCrate crate);
    private bool CanLockHackableCrate(BasePlayer player, HackableLockedCrate crate);
    private void CanHackCrate(BasePlayer player, HackableLockedCrate crate);
    private void SetupLaunchSite();
    private void SetupHackableCrate(BasePlayer owner, HackableLockedCrate crate);
    private void CancelDamage(HitInfo hitInfo);
    private bool CanMessage(BasePlayer player);
    public bool HasLockout(BasePlayer player, DamageEntryType damageEntryType, ulong skinid);
    private string FormatTime(double seconds);
    private void ApplyCooldowns(DamageEntryType damageEntryType);
    public void TrySetLockout(string userid, BasePlayer player, DamageEntryType damageEntryType, ulong skinID);
    private void LockoutLooters(HashSet<ulong> looters, Vector3 position, DamageEntryType damageEntryType, ulong skinID);
    private void LockoutTeam(HashSet<ulong> members, ulong looterId, DamageEntryType damageEntryType, ulong skinID);
    private void LockoutClan(HashSet<ulong> members, ulong looterId, DamageEntryType damageEntryType, ulong skinID);
    private object HandleTeam(RelationshipManager.PlayerTeam team, ulong userid);
    private bool IsDefended(PatrolHelicopter heli);
    private bool IsDefended(BaseCombatEntity victim);
    private void DoLockoutRemoves();
    private void Unsubscribe();
    private void SaveData();
    private void LoadData();
    private void RegisterPermissions();
    private bool IsAlly(ulong playerId, ulong targetId);
    private static List<T> FindEntitiesOfType(Vector3 a, float n, int m);
    private bool CanRemoveFire(DamageEntryType damageEntryType);
    private void RemoveFireFromCrates(Vector3 position, DamageEntryType damageEntryType);
    private void LockInRadius(Vector3 position, LockInfo lockInfo, DamageEntryType damageEntryType);
    private void LockInRadius(Vector3 position, ulong damageKey, DamageInfo damageInfo, ulong playerSteamID);
    private void LockInRadius(Vector3 position, LockInfo lockInfo, ulong playerSteamID);
    private static int GetLockTime(DamageEntryType damageEntryType);
    public class UI
    {
        private static CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor, string parent);
        private static void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        private static string Color(string hexColor, float a);
        public static void DestroyLockoutUI(BasePlayer player);
        public static void DestroyAllLockoutUI();
        private static void Create(BasePlayer player, string panelName, string text, int fontSize, string color, string panelColor, string aMin, string aMax);
        public static bool ShowTemporaryLockouts(BasePlayer player, string min, string max);
        public static void ShowLockouts(BasePlayer player);
        public static double GetLockoutTime(DamageEntryType damageEntryType);
        public static double GetLockoutTime(DamageEntryType damageEntryType, Lockout lo, string playerId);
        public static void UpdateLockoutUI(BasePlayer player);
        private static void SetLockoutUpdate(BasePlayer player);
        public static void DestroyLockoutUpdate(BasePlayer player);
        public static Info GetSettings(string playerId);
        private const string BradleyPanelName;
        private const string HeliPanelName;
        public static List<BasePlayer> Lockouts { get; set; }
        public static Dictionary<ulong, Timers> InvokeTimers { get; set; }
        public class Timers
        {
            public Timer Lockout;
        }

        public class Info
        {
            public bool Enabled { get; set; }
            public bool Lockouts { get; set; }
        }

    }

    private void CommandUI(IPlayer user, string command, string[] args);
    private void CommandLootDefender(IPlayer user, string command, string[] args);
    private void CommandLockouts(IPlayer user, string command, string[] args);
    private bool CanSendDiscordMessage();
    private static string PositionToGrid(Vector3 position);
    private void SendDiscordMessage(HashSet<ulong> members, List<string> usernames, Vector3 position, DamageEntryType damageEntryType);
    private void SendDiscordMessage(Dictionary<string, string> members, Vector3 position, string text);
    private class NotifySettings
    {
        [JsonProperty(PropertyName = "Broadcast Kill Notification To Chat")]
        public bool NotifyChat { get; set; }
        [JsonProperty(PropertyName = "Broadcast Kill Notification To Killer")]
        public bool NotifyKiller { get; set; }
        [JsonProperty(PropertyName = "Broadcast Locked Notification To Chat", NullValueHandling = NullValueHandling.Ignore)]
        public bool? NotifyLocked { get; set; }
    }

    private class HackPermission
    {
        [JsonProperty(PropertyName = "Permission")]
        public string Permission { get; set; }
        [JsonProperty(PropertyName = "Hack Time")]
        public float Value { get; set; }
    }

    private static List<HackPermission> DefaultHackPermissions { get; set; }
    private class HackableSettings
    {
        [JsonProperty(PropertyName = "Permissions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<HackPermission> Permissions;
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Permissions Enabled To Set Required Hack Seconds")]
        public bool Seconds { get; set; }
        [JsonProperty(PropertyName = "Lock For X Seconds (0 = Forever)")]
        public int LockTime { get; set; }
        [JsonProperty(PropertyName = "Lock Hackable Crates At Harbor")]
        public bool Harbor { get; set; }
        [JsonProperty(PropertyName = "Block Timer Increase On Damage To Laptop")]
        public bool Laptop { get; set; }
        [JsonProperty(PropertyName = "Broadcast Locked Notification To Chat", NullValueHandling = NullValueHandling.Ignore)]
        public bool NotifyLocked { get; set; }
        [JsonProperty(PropertyName = "Broadcast Unlocked Notification To Chat", NullValueHandling = NullValueHandling.Ignore)]
        public bool NotifyUnlocked { get; set; }
        [JsonProperty(PropertyName = "Cooldown Between Notifications For Each Player")]
        public float NotifyCooldown { get; set; }
    }

    private class BradleySettings
    {
        [JsonProperty(PropertyName = "Messages")]
        public NotifySettings Messages { get; set; }
        [JsonProperty(PropertyName = "Damage Lock Threshold")]
        public float Threshold { get; set; }
        [JsonProperty(PropertyName = "Harvest Too Hot Until (0 = Never)")]
        public float TooHotUntil { get; set; }
        [JsonProperty(PropertyName = "Lock For X Seconds (0 = Forever)")]
        public int LockTime { get; set; }
        [JsonProperty(PropertyName = "Remove Fire From Crates")]
        public bool RemoveFireFromCrates { get; set; }
        [JsonProperty(PropertyName = "Lock Bradley At Launch Site")]
        public bool LockLaunchSite { get; set; }
        [JsonProperty(PropertyName = "Lock Bradley At Harbor")]
        public bool LockHarbor { get; set; }
        [JsonProperty(PropertyName = "Lock Bradley From Personal Apc Plugin")]
        public bool LockPersonal { get; set; }
        [JsonProperty(PropertyName = "Lock Bradley From Monument Bradley Plugin")]
        public bool LockMonument { get; set; }
        [JsonProperty(PropertyName = "Lock Bradley From Convoy Plugin")]
        public bool LockConvoy { get; set; }
        [JsonProperty(PropertyName = "Lock Bradley From Bradley Tiers Plugin")]
        public bool LockBradleyTiers { get; set; }
        [JsonProperty(PropertyName = "Lock Bradley From Everywhere Else")]
        public bool LockWorldly { get; set; }
        [JsonProperty(PropertyName = "Block Looting Only")]
        public bool LootingOnly { get; set; }
        [JsonProperty(PropertyName = "Rust Rewards RP")]
        public double RRP { get; set; }
        [JsonProperty(PropertyName = "XP Reward")]
        public double XP { get; set; }
        [JsonProperty(PropertyName = "ShoppyStock Reward Value")]
        public double SS { get; set; }
        [JsonProperty(PropertyName = "ShoppyStock Shop Name")]
        public string ShoppyStockShopName { get; set; }
    }

    private class HelicopterSettings
    {
        [JsonProperty(PropertyName = "Messages")]
        public NotifySettings Messages { get; set; }
        [JsonProperty(PropertyName = "Damage Lock Threshold")]
        public float Threshold { get; set; }
        [JsonProperty(PropertyName = "Harvest Too Hot Until (0 = Never)")]
        public float TooHotUntil { get; set; }
        [JsonProperty(PropertyName = "Lock For X Seconds (0 = Forever)")]
        public int LockTime { get; set; }
        [JsonProperty(PropertyName = "Remove Fire From Crates")]
        public bool RemoveFireFromCrates { get; set; }
        [JsonProperty(PropertyName = "Lock Heli From Convoy Plugin")]
        public bool LockConvoy { get; set; }
        [JsonProperty(PropertyName = "Lock Heli At Harbor")]
        public bool? LockHarbor { get; set; }
        [JsonProperty(PropertyName = "Lock Heli From Personal Heli Plugin")]
        public bool LockPersonal { get; set; }
        [JsonProperty(PropertyName = "Block Looting Only")]
        public bool LootingOnly { get; set; }
        [JsonProperty(PropertyName = "Rust Rewards RP")]
        public double RRP { get; set; }
        [JsonProperty(PropertyName = "XP Reward")]
        public double XP { get; set; }
        [JsonProperty(PropertyName = "ShoppyStock Reward Value")]
        public double SS { get; set; }
        [JsonProperty(PropertyName = "ShoppyStock Shop Name")]
        public string ShoppyStockShopName { get; set; }
    }

    private class NpcSettings
    {
        [JsonProperty(PropertyName = "Reward Distance Multiplier")]
        public DistanceMultiplierSettings Distance { get; set; }
        [JsonProperty(PropertyName = "Reward Weapon Multiplier", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, double> WeaponMultipliers { get; set; }
        [JsonProperty(PropertyName = "Messages")]
        public NotifySettings Messages { get; set; }
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Damage Lock Threshold")]
        public float Threshold { get; set; }
        [JsonProperty(PropertyName = "Lock For X Seconds (0 = Forever)")]
        public int LockTime { get; set; }
        [JsonProperty(PropertyName = "Minimum Starting Health Requirement")]
        public float Min { get; set; }
        [JsonProperty(PropertyName = "Lock BossMonster Npcs")]
        public bool BossMonster { get; set; }
        [JsonProperty(PropertyName = "Block Looting Only")]
        public bool LootingOnly { get; set; }
        [JsonProperty(PropertyName = "Rust Rewards RP")]
        public double RRP { get; set; }
        [JsonProperty(PropertyName = "XP Reward")]
        public double XP { get; set; }
        [JsonProperty(PropertyName = "ShoppyStock Reward Value")]
        public double SS { get; set; }
        [JsonProperty(PropertyName = "ShoppyStock Shop Name")]
        public string ShoppyStockShopName { get; set; }
    }

    private class DistanceMultiplierSettings
    {
        [JsonProperty(PropertyName = "400 meters")]
        public float meters400 { get; set; }
        [JsonProperty(PropertyName = "300 meters")]
        public float meters300 { get; set; }
        [JsonProperty(PropertyName = "200 meters")]
        public float meters200 { get; set; }
        [JsonProperty(PropertyName = "100 meters")]
        public float meters100 { get; set; }
        [JsonProperty(PropertyName = "75 meters")]
        public float meters75 { get; set; }
        [JsonProperty(PropertyName = "50 meters")]
        public float meters50 { get; set; }
        [JsonProperty(PropertyName = "25 meters")]
        public float meters25 { get; set; }
        [JsonProperty(PropertyName = "under")]
        public float under { get; set; }
        public double GetDistanceMult(float distance);
    }

    private class SupplyDropSettings
    {
        [JsonProperty(PropertyName = "Allow Locking Signals With These Skins", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ulong> Skins { get; set; }
        [JsonProperty(PropertyName = "Lock Supply Drops To Players")]
        public bool Lock { get; set; }
        [JsonProperty(PropertyName = "Lock Supply Drops From Excavator")]
        public bool Excavator { get; set; }
        [JsonProperty(PropertyName = "Lock Supply Drops From Helpful Supply Plugin")]
        public bool HelpfulSupply { get; set; }
        [JsonProperty(PropertyName = "Lock Supply Drops From Npc Random Raids Plugin")]
        public bool NpcRandomRaids { get; set; }
        [JsonProperty(PropertyName = "Lock To Player For X Seconds (0 = Forever)")]
        public float LockTime { get; set; }
        [JsonProperty(PropertyName = "Supply Drop Drag")]
        public float Drag { get; set; }
        [JsonProperty(PropertyName = "Show Grid In Thrown Notification")]
        public bool ThrownAt { get; set; }
        [JsonProperty(PropertyName = "Show Thrown Notification In Chat")]
        public bool NotifyChat { get; set; }
        [JsonProperty(PropertyName = "Show Notification In Server Console")]
        public bool NotifyConsole { get; set; }
        [JsonProperty(PropertyName = "Cooldown Between Notifications For Each Player")]
        public float NotifyCooldown { get; set; }
        [JsonProperty(PropertyName = "Cargo Plane Speed (Meters Per Second)")]
        public float Speed { get; set; }
        [JsonProperty(PropertyName = "Cargo Plane Low Altitude Drop")]
        public bool LowDrop { get; set; }
        [JsonProperty(PropertyName = "Bypass Spawning Cargo Plane")]
        public bool Bypass { get; set; }
        [JsonProperty(PropertyName = "Smoke Duration")]
        public float Smoke { get; set; }
        [JsonProperty(PropertyName = "Maximum Drop Distance From Signal")]
        public float DistanceFromSignal { get; set; }
        [JsonProperty(PropertyName = "Destroy Drop After X Seconds")]
        public float DestroyTime { get; set; }
    }

    private class DamageReportSettings
    {
        [JsonProperty(PropertyName = "Hex Color - Single Player")]
        public string SinglePlayer { get; set; }
        [JsonProperty(PropertyName = "Hex Color - Team")]
        public string Team { get; set; }
        [JsonProperty(PropertyName = "Hex Color - Ok")]
        public string Ok { get; set; }
        [JsonProperty(PropertyName = "Hex Color - Not Ok")]
        public string NotOk { get; set; }
    }

    public class PluginSettingsBaseLockout
    {
        [JsonProperty(PropertyName = "Bypass During F15 Server Wipe Event")]
        public bool F15 { get; set; }
        [JsonProperty(PropertyName = "Command To See Lockout Times")]
        public string Command { get; set; }
        [JsonProperty(PropertyName = "Time Between Bradley In Minutes")]
        public double Bradley { get; set; }
        [JsonProperty(PropertyName = "Time Between Heli In Minutes")]
        public double Heli { get; set; }
        [JsonProperty(PropertyName = "Lockout Entire Team")]
        public bool Team { get; set; }
        [JsonProperty(PropertyName = "Lockout Entire Clan")]
        public bool Clan { get; set; }
        [JsonProperty(PropertyName = "Exclude Members Offline For More Than X Minutes")]
        public float Time { get; set; }
        [JsonProperty(PropertyName = "Lockouts Ignored For Entities With Skin ID", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ulong> Exceptions { get; set; }
    }

    public class UIBradleyLockoutSettings
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Bradley Anchor Min")]
        public string AnchorMin { get; set; }
        [JsonProperty(PropertyName = "Bradley Anchor Max")]
        public string AnchorMax { get; set; }
        [JsonProperty(PropertyName = "Bradley Background Color")]
        public string BackgroundColor { get; set; }
        [JsonProperty(PropertyName = "Bradley Text Color")]
        public string TextColor { get; set; }
        [JsonProperty(PropertyName = "Panel Alpha")]
        public float Alpha { get; set; }
        [JsonProperty(PropertyName = "Font Size")]
        public int FontSize { get; set; }
    }

    public class UIHeliLockoutSettings
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Heli Anchor Min")]
        public string AnchorMin { get; set; }
        [JsonProperty(PropertyName = "Heli Anchor Max")]
        public string AnchorMax { get; set; }
        [JsonProperty(PropertyName = "Heli Background Color")]
        public string BackgroundColor { get; set; }
        [JsonProperty(PropertyName = "Heli Text Color")]
        public string TextColor { get; set; }
        [JsonProperty(PropertyName = "Panel Alpha")]
        public float Alpha { get; set; }
        [JsonProperty(PropertyName = "Font Size")]
        public int FontSize { get; set; }
    }

    public class UISettings
    {
        [JsonProperty(PropertyName = "Command To Toggle UI")]
        public string Command { get; set; }
        [JsonProperty(PropertyName = "Bradley")]
        public UIBradleyLockoutSettings Bradley { get; set; }
        [JsonProperty(PropertyName = "Heli")]
        public UIHeliLockoutSettings Heli { get; set; }
    }

    public class DiscordMessagesSettings
    {
        [JsonProperty(PropertyName = "Message - Webhook URL")]
        public string WebhookUrl { get; set; }
        [JsonProperty(PropertyName = "Message - Embed Color (DECIMAL)")]
        public int MessageColor { get; set; }
        [JsonProperty(PropertyName = "Embed_MessageTitle")]
        public string EmbedMessageTitle { get; set; }
        [JsonProperty(PropertyName = "Embed_MessagePlayer")]
        public string EmbedMessagePlayer { get; set; }
        [JsonProperty(PropertyName = "Embed_MessageMessage")]
        public string EmbedMessageMessage { get; set; }
        [JsonProperty(PropertyName = "Embed_MessageServer")]
        public string EmbedMessageServer { get; set; }
        [JsonProperty(PropertyName = "Add BattleMetrics Link")]
        public bool BattleMetrics { get; set; }
        [JsonProperty(PropertyName = "Show Notification In Server Console")]
        public bool NotifyConsole { get; set; }
    }

    private class Configuration
    {
        [JsonProperty(PropertyName = "Bradley Settings")]
        public BradleySettings Bradley { get; set; }
        [JsonProperty(PropertyName = "Helicopter Settings")]
        public HelicopterSettings Helicopter { get; set; }
        [JsonProperty(PropertyName = "Hackable Crate Settings")]
        public HackableSettings Hackable { get; set; }
        [JsonProperty(PropertyName = "Npc Settings")]
        public NpcSettings Npc { get; set; }
        [JsonProperty(PropertyName = "Supply Drop Settings")]
        public SupplyDropSettings SupplyDrop { get; set; }
        [JsonProperty(PropertyName = "Damage Report Settings")]
        public DamageReportSettings Report { get; set; }
        [JsonProperty(PropertyName = "Player Lockouts (0 = ignore)")]
        public PluginSettingsBaseLockout Lockout { get; set; }
        [JsonProperty(PropertyName = "Lockout UI")]
        public UISettings UI { get; set; }
        [JsonProperty(PropertyName = "Discord Messages")]
        public DiscordMessagesSettings DiscordMessages { get; set; }
        [JsonProperty(PropertyName = "Disable CH47 Gibs")]
        public bool CH47Gibs { get; set; }
        [JsonProperty(PropertyName = "Chat ID")]
        public ulong ChatID { get; set; }
    }

    private static Configuration config;
    private bool configLoaded;
    protected override void LoadConfig();
    private bool canSaveConfig;
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private bool HasPermission(BasePlayer player, string perm);
    protected override void LoadDefaultMessages();
    private static string _(string key, string userId, object[] args);
    public static string RemoveFormatting(string source);
    private static void CreateMessage(BasePlayer player, string key, object[] args);
    private static void Message(BasePlayer player, string message);
}

public class Lockout
{
    public double Bradley { get; set; }
    public double Heli { get; set; }
    public bool Any();
}

private class StoredData
{
    public Dictionary<string, Lockout> Lockouts { get; set; }
    public Dictionary<string, UI.Info> UI { get; set; }
    [JsonProperty(PropertyName = "DamageInfo")]
    public Dictionary<ulong, DamageInfo> Damage { get; set; }
    [JsonProperty(PropertyName = "LockInfo")]
    public Dictionary<ulong, LockInfo> Lock { get; set; }
    public void Sanitize();
}

private class DamageEntry
{
    public float DamageDealt { get; set; }
    public DateTime Timestamp { get; set; }
    public string TeamID { get; set; }
    public DamageEntry();
    public DamageEntry(ulong teamID);
    public bool IsOutdated(int timeout);
}

private class DamageKey
{
    public ulong userid { get; set; }
    public string name { get; set; }
    public DamageEntry damageEntry { get; set; }
    internal BasePlayer attacker { get; set; }
    public DamageKey();
    public DamageKey(BasePlayer attacker);
}

private class DamageInfo
{
    public List<DamageKey> damageKeys { get; set; }
    [JsonIgnore]
    public Dictionary<ulong, BasePlayer> interact { get; set; }
    private List<ulong> participants { get; set; }
    public DamageEntryType damageEntryType { get; set; }
    public string NPCName { get; set; }
    public ulong OwnerID { get; set; }
    public ulong SkinID { get; set; }
    public DateTime start { get; set; }
    internal int _lockTime { get; set; }
    [JsonIgnore]
    internal BaseEntity _entity { get; set; }
    internal Vector3 _position { get; set; }
    internal Vector3 lastAttackedPosition { get; set; }
    internal ulong _uid { get; set; }
    [JsonIgnore]
    internal Timer _timer { get; set; }
    internal List<DamageKey> keys { get; set; }
     List<DamageGroup> damageGroups;
    internal float FullDamage { get; set; }
    public DamageInfo();
    public DamageInfo(DamageEntryType damageEntryType, string NPCName, BaseEntity entity, DateTime start);
    public void Start();
    public void DestroyTimer();
    private void CheckExpiration();
    public void Unlock();
    private void Lock(BaseEntity entity, ulong id);
    public void AddDamage(BaseCombatEntity entity, BasePlayer attacker, DamageEntry entry, float amount);
    public DamageEntry TryGet(ulong id);
    public DamageEntry Get(BasePlayer attacker);
    public bool isKilled;
    public void OnKilled(Vector3 position, HitInfo hitInfo, float distance);
    private void FindLooters(Vector3 position, HitInfo hitInfo, float distance);
    public void DisplayDamageReport();
    private bool CanDisplayReport(BasePlayer target);
    public void SetCanInteract();
    public string GetDamageReport(ulong targetId);
    public bool IsParticipant(ulong userid, BasePlayer player);
    public bool CanInteract(ulong userid, BasePlayer player);
    private List<DamageGroup> GetTopDamageGroups(List<DamageGroup> damageGroups, DamageEntryType damageEntryType);
    private List<DamageGroup> GetDamageGroups();
}

private class LockInfo
{
    public DamageInfo damageInfo { get; set; }
    private DateTime LockTimestamp { get; set; }
    private int LockTimeout { get; set; }
    internal bool IsLockOutdated { get; set; }
    public LockInfo();
    public LockInfo(DamageInfo damageInfo, int lockTimeout);
    public bool CanInteract(ulong userId, BasePlayer target);
    public string GetDamageReport(ulong userId);
}

private class DamageGroup
{
    public float TotalDamage { get; set; }
    public DamageKey FirstDamagerDealer { get; set; }
    private List<ulong> additionalPlayers { get; set; }
    [JsonIgnore]
    public List<ulong> Players { get; set; }
    public DamageGroup();
    public DamageGroup(DamageKey x);
    public string ToReport(DamageKey damageKey, DamageInfo damageInfo);
}

public class UI
{
    private static CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor, string parent);
    private static void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    private static string Color(string hexColor, float a);
    public static void DestroyLockoutUI(BasePlayer player);
    public static void DestroyAllLockoutUI();
    private static void Create(BasePlayer player, string panelName, string text, int fontSize, string color, string panelColor, string aMin, string aMax);
    public static bool ShowTemporaryLockouts(BasePlayer player, string min, string max);
    public static void ShowLockouts(BasePlayer player);
    public static double GetLockoutTime(DamageEntryType damageEntryType);
    public static double GetLockoutTime(DamageEntryType damageEntryType, Lockout lo, string playerId);
    public static void UpdateLockoutUI(BasePlayer player);
    private static void SetLockoutUpdate(BasePlayer player);
    public static void DestroyLockoutUpdate(BasePlayer player);
    public static Info GetSettings(string playerId);
    private const string BradleyPanelName;
    private const string HeliPanelName;
    public static List<BasePlayer> Lockouts { get; set; }
    public static Dictionary<ulong, Timers> InvokeTimers { get; set; }
    public class Timers
    {
        public Timer Lockout;
    }

    public class Info
    {
        public bool Enabled { get; set; }
        public bool Lockouts { get; set; }
    }

}

public class Timers
{
    public Timer Lockout;
}

public class Info
{
    public bool Enabled { get; set; }
    public bool Lockouts { get; set; }
}

private class NotifySettings
{
    [JsonProperty(PropertyName = "Broadcast Kill Notification To Chat")]
    public bool NotifyChat { get; set; }
    [JsonProperty(PropertyName = "Broadcast Kill Notification To Killer")]
    public bool NotifyKiller { get; set; }
    [JsonProperty(PropertyName = "Broadcast Locked Notification To Chat", NullValueHandling = NullValueHandling.Ignore)]
    public bool? NotifyLocked { get; set; }
}

private class HackPermission
{
    [JsonProperty(PropertyName = "Permission")]
    public string Permission { get; set; }
    [JsonProperty(PropertyName = "Hack Time")]
    public float Value { get; set; }
}

private class HackableSettings
{
    [JsonProperty(PropertyName = "Permissions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<HackPermission> Permissions;
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Permissions Enabled To Set Required Hack Seconds")]
    public bool Seconds { get; set; }
    [JsonProperty(PropertyName = "Lock For X Seconds (0 = Forever)")]
    public int LockTime { get; set; }
    [JsonProperty(PropertyName = "Lock Hackable Crates At Harbor")]
    public bool Harbor { get; set; }
    [JsonProperty(PropertyName = "Block Timer Increase On Damage To Laptop")]
    public bool Laptop { get; set; }
    [JsonProperty(PropertyName = "Broadcast Locked Notification To Chat", NullValueHandling = NullValueHandling.Ignore)]
    public bool NotifyLocked { get; set; }
    [JsonProperty(PropertyName = "Broadcast Unlocked Notification To Chat", NullValueHandling = NullValueHandling.Ignore)]
    public bool NotifyUnlocked { get; set; }
    [JsonProperty(PropertyName = "Cooldown Between Notifications For Each Player")]
    public float NotifyCooldown { get; set; }
}

private class BradleySettings
{
    [JsonProperty(PropertyName = "Messages")]
    public NotifySettings Messages { get; set; }
    [JsonProperty(PropertyName = "Damage Lock Threshold")]
    public float Threshold { get; set; }
    [JsonProperty(PropertyName = "Harvest Too Hot Until (0 = Never)")]
    public float TooHotUntil { get; set; }
    [JsonProperty(PropertyName = "Lock For X Seconds (0 = Forever)")]
    public int LockTime { get; set; }
    [JsonProperty(PropertyName = "Remove Fire From Crates")]
    public bool RemoveFireFromCrates { get; set; }
    [JsonProperty(PropertyName = "Lock Bradley At Launch Site")]
    public bool LockLaunchSite { get; set; }
    [JsonProperty(PropertyName = "Lock Bradley At Harbor")]
    public bool LockHarbor { get; set; }
    [JsonProperty(PropertyName = "Lock Bradley From Personal Apc Plugin")]
    public bool LockPersonal { get; set; }
    [JsonProperty(PropertyName = "Lock Bradley From Monument Bradley Plugin")]
    public bool LockMonument { get; set; }
    [JsonProperty(PropertyName = "Lock Bradley From Convoy Plugin")]
    public bool LockConvoy { get; set; }
    [JsonProperty(PropertyName = "Lock Bradley From Bradley Tiers Plugin")]
    public bool LockBradleyTiers { get; set; }
    [JsonProperty(PropertyName = "Lock Bradley From Everywhere Else")]
    public bool LockWorldly { get; set; }
    [JsonProperty(PropertyName = "Block Looting Only")]
    public bool LootingOnly { get; set; }
    [JsonProperty(PropertyName = "Rust Rewards RP")]
    public double RRP { get; set; }
    [JsonProperty(PropertyName = "XP Reward")]
    public double XP { get; set; }
    [JsonProperty(PropertyName = "ShoppyStock Reward Value")]
    public double SS { get; set; }
    [JsonProperty(PropertyName = "ShoppyStock Shop Name")]
    public string ShoppyStockShopName { get; set; }
}

private class HelicopterSettings
{
    [JsonProperty(PropertyName = "Messages")]
    public NotifySettings Messages { get; set; }
    [JsonProperty(PropertyName = "Damage Lock Threshold")]
    public float Threshold { get; set; }
    [JsonProperty(PropertyName = "Harvest Too Hot Until (0 = Never)")]
    public float TooHotUntil { get; set; }
    [JsonProperty(PropertyName = "Lock For X Seconds (0 = Forever)")]
    public int LockTime { get; set; }
    [JsonProperty(PropertyName = "Remove Fire From Crates")]
    public bool RemoveFireFromCrates { get; set; }
    [JsonProperty(PropertyName = "Lock Heli From Convoy Plugin")]
    public bool LockConvoy { get; set; }
    [JsonProperty(PropertyName = "Lock Heli At Harbor")]
    public bool? LockHarbor { get; set; }
    [JsonProperty(PropertyName = "Lock Heli From Personal Heli Plugin")]
    public bool LockPersonal { get; set; }
    [JsonProperty(PropertyName = "Block Looting Only")]
    public bool LootingOnly { get; set; }
    [JsonProperty(PropertyName = "Rust Rewards RP")]
    public double RRP { get; set; }
    [JsonProperty(PropertyName = "XP Reward")]
    public double XP { get; set; }
    [JsonProperty(PropertyName = "ShoppyStock Reward Value")]
    public double SS { get; set; }
    [JsonProperty(PropertyName = "ShoppyStock Shop Name")]
    public string ShoppyStockShopName { get; set; }
}

private class NpcSettings
{
    [JsonProperty(PropertyName = "Reward Distance Multiplier")]
    public DistanceMultiplierSettings Distance { get; set; }
    [JsonProperty(PropertyName = "Reward Weapon Multiplier", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, double> WeaponMultipliers { get; set; }
    [JsonProperty(PropertyName = "Messages")]
    public NotifySettings Messages { get; set; }
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Damage Lock Threshold")]
    public float Threshold { get; set; }
    [JsonProperty(PropertyName = "Lock For X Seconds (0 = Forever)")]
    public int LockTime { get; set; }
    [JsonProperty(PropertyName = "Minimum Starting Health Requirement")]
    public float Min { get; set; }
    [JsonProperty(PropertyName = "Lock BossMonster Npcs")]
    public bool BossMonster { get; set; }
    [JsonProperty(PropertyName = "Block Looting Only")]
    public bool LootingOnly { get; set; }
    [JsonProperty(PropertyName = "Rust Rewards RP")]
    public double RRP { get; set; }
    [JsonProperty(PropertyName = "XP Reward")]
    public double XP { get; set; }
    [JsonProperty(PropertyName = "ShoppyStock Reward Value")]
    public double SS { get; set; }
    [JsonProperty(PropertyName = "ShoppyStock Shop Name")]
    public string ShoppyStockShopName { get; set; }
}

private class DistanceMultiplierSettings
{
    [JsonProperty(PropertyName = "400 meters")]
    public float meters400 { get; set; }
    [JsonProperty(PropertyName = "300 meters")]
    public float meters300 { get; set; }
    [JsonProperty(PropertyName = "200 meters")]
    public float meters200 { get; set; }
    [JsonProperty(PropertyName = "100 meters")]
    public float meters100 { get; set; }
    [JsonProperty(PropertyName = "75 meters")]
    public float meters75 { get; set; }
    [JsonProperty(PropertyName = "50 meters")]
    public float meters50 { get; set; }
    [JsonProperty(PropertyName = "25 meters")]
    public float meters25 { get; set; }
    [JsonProperty(PropertyName = "under")]
    public float under { get; set; }
    public double GetDistanceMult(float distance);
}

private class SupplyDropSettings
{
    [JsonProperty(PropertyName = "Allow Locking Signals With These Skins", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ulong> Skins { get; set; }
    [JsonProperty(PropertyName = "Lock Supply Drops To Players")]
    public bool Lock { get; set; }
    [JsonProperty(PropertyName = "Lock Supply Drops From Excavator")]
    public bool Excavator { get; set; }
    [JsonProperty(PropertyName = "Lock Supply Drops From Helpful Supply Plugin")]
    public bool HelpfulSupply { get; set; }
    [JsonProperty(PropertyName = "Lock Supply Drops From Npc Random Raids Plugin")]
    public bool NpcRandomRaids { get; set; }
    [JsonProperty(PropertyName = "Lock To Player For X Seconds (0 = Forever)")]
    public float LockTime { get; set; }
    [JsonProperty(PropertyName = "Supply Drop Drag")]
    public float Drag { get; set; }
    [JsonProperty(PropertyName = "Show Grid In Thrown Notification")]
    public bool ThrownAt { get; set; }
    [JsonProperty(PropertyName = "Show Thrown Notification In Chat")]
    public bool NotifyChat { get; set; }
    [JsonProperty(PropertyName = "Show Notification In Server Console")]
    public bool NotifyConsole { get; set; }
    [JsonProperty(PropertyName = "Cooldown Between Notifications For Each Player")]
    public float NotifyCooldown { get; set; }
    [JsonProperty(PropertyName = "Cargo Plane Speed (Meters Per Second)")]
    public float Speed { get; set; }
    [JsonProperty(PropertyName = "Cargo Plane Low Altitude Drop")]
    public bool LowDrop { get; set; }
    [JsonProperty(PropertyName = "Bypass Spawning Cargo Plane")]
    public bool Bypass { get; set; }
    [JsonProperty(PropertyName = "Smoke Duration")]
    public float Smoke { get; set; }
    [JsonProperty(PropertyName = "Maximum Drop Distance From Signal")]
    public float DistanceFromSignal { get; set; }
    [JsonProperty(PropertyName = "Destroy Drop After X Seconds")]
    public float DestroyTime { get; set; }
}

private class DamageReportSettings
{
    [JsonProperty(PropertyName = "Hex Color - Single Player")]
    public string SinglePlayer { get; set; }
    [JsonProperty(PropertyName = "Hex Color - Team")]
    public string Team { get; set; }
    [JsonProperty(PropertyName = "Hex Color - Ok")]
    public string Ok { get; set; }
    [JsonProperty(PropertyName = "Hex Color - Not Ok")]
    public string NotOk { get; set; }
}

public class PluginSettingsBaseLockout
{
    [JsonProperty(PropertyName = "Bypass During F15 Server Wipe Event")]
    public bool F15 { get; set; }
    [JsonProperty(PropertyName = "Command To See Lockout Times")]
    public string Command { get; set; }
    [JsonProperty(PropertyName = "Time Between Bradley In Minutes")]
    public double Bradley { get; set; }
    [JsonProperty(PropertyName = "Time Between Heli In Minutes")]
    public double Heli { get; set; }
    [JsonProperty(PropertyName = "Lockout Entire Team")]
    public bool Team { get; set; }
    [JsonProperty(PropertyName = "Lockout Entire Clan")]
    public bool Clan { get; set; }
    [JsonProperty(PropertyName = "Exclude Members Offline For More Than X Minutes")]
    public float Time { get; set; }
    [JsonProperty(PropertyName = "Lockouts Ignored For Entities With Skin ID", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ulong> Exceptions { get; set; }
}

public class UIBradleyLockoutSettings
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Bradley Anchor Min")]
    public string AnchorMin { get; set; }
    [JsonProperty(PropertyName = "Bradley Anchor Max")]
    public string AnchorMax { get; set; }
    [JsonProperty(PropertyName = "Bradley Background Color")]
    public string BackgroundColor { get; set; }
    [JsonProperty(PropertyName = "Bradley Text Color")]
    public string TextColor { get; set; }
    [JsonProperty(PropertyName = "Panel Alpha")]
    public float Alpha { get; set; }
    [JsonProperty(PropertyName = "Font Size")]
    public int FontSize { get; set; }
}

public class UIHeliLockoutSettings
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Heli Anchor Min")]
    public string AnchorMin { get; set; }
    [JsonProperty(PropertyName = "Heli Anchor Max")]
    public string AnchorMax { get; set; }
    [JsonProperty(PropertyName = "Heli Background Color")]
    public string BackgroundColor { get; set; }
    [JsonProperty(PropertyName = "Heli Text Color")]
    public string TextColor { get; set; }
    [JsonProperty(PropertyName = "Panel Alpha")]
    public float Alpha { get; set; }
    [JsonProperty(PropertyName = "Font Size")]
    public int FontSize { get; set; }
}

public class UISettings
{
    [JsonProperty(PropertyName = "Command To Toggle UI")]
    public string Command { get; set; }
    [JsonProperty(PropertyName = "Bradley")]
    public UIBradleyLockoutSettings Bradley { get; set; }
    [JsonProperty(PropertyName = "Heli")]
    public UIHeliLockoutSettings Heli { get; set; }
}

public class DiscordMessagesSettings
{
    [JsonProperty(PropertyName = "Message - Webhook URL")]
    public string WebhookUrl { get; set; }
    [JsonProperty(PropertyName = "Message - Embed Color (DECIMAL)")]
    public int MessageColor { get; set; }
    [JsonProperty(PropertyName = "Embed_MessageTitle")]
    public string EmbedMessageTitle { get; set; }
    [JsonProperty(PropertyName = "Embed_MessagePlayer")]
    public string EmbedMessagePlayer { get; set; }
    [JsonProperty(PropertyName = "Embed_MessageMessage")]
    public string EmbedMessageMessage { get; set; }
    [JsonProperty(PropertyName = "Embed_MessageServer")]
    public string EmbedMessageServer { get; set; }
    [JsonProperty(PropertyName = "Add BattleMetrics Link")]
    public bool BattleMetrics { get; set; }
    [JsonProperty(PropertyName = "Show Notification In Server Console")]
    public bool NotifyConsole { get; set; }
}

private class Configuration
{
    [JsonProperty(PropertyName = "Bradley Settings")]
    public BradleySettings Bradley { get; set; }
    [JsonProperty(PropertyName = "Helicopter Settings")]
    public HelicopterSettings Helicopter { get; set; }
    [JsonProperty(PropertyName = "Hackable Crate Settings")]
    public HackableSettings Hackable { get; set; }
    [JsonProperty(PropertyName = "Npc Settings")]
    public NpcSettings Npc { get; set; }
    [JsonProperty(PropertyName = "Supply Drop Settings")]
    public SupplyDropSettings SupplyDrop { get; set; }
    [JsonProperty(PropertyName = "Damage Report Settings")]
    public DamageReportSettings Report { get; set; }
    [JsonProperty(PropertyName = "Player Lockouts (0 = ignore)")]
    public PluginSettingsBaseLockout Lockout { get; set; }
    [JsonProperty(PropertyName = "Lockout UI")]
    public UISettings UI { get; set; }
    [JsonProperty(PropertyName = "Discord Messages")]
    public DiscordMessagesSettings DiscordMessages { get; set; }
    [JsonProperty(PropertyName = "Disable CH47 Gibs")]
    public bool CH47Gibs { get; set; }
    [JsonProperty(PropertyName = "Chat ID")]
    public ulong ChatID { get; set; }
}


```

---

## LootDespawn by misticos - Customize loot despawn

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Loot Despawn", "Iv Misticos", "2.0.3")]
[Description("Customize loot despawn")]
public class LootDespawn : RustPlugin
{
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled;
        [JsonProperty(PropertyName = "Multiplier Inside Building Privilege")]
        public float MultiplierCupboard;
        [JsonProperty(PropertyName = "Multiplier Outside Building Privilege")]
        public float MultiplierNonCupboard;
        [JsonProperty(PropertyName = "Items' Multipliers", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, float> MultiplierItems;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnItemDropped(Item item, BaseEntity entity);
    private void SetDespawnTime(Item item, DroppedItem droppedItem);
    private float GetItemRate(Item item, DroppedItem droppedItem);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled;
    [JsonProperty(PropertyName = "Multiplier Inside Building Privilege")]
    public float MultiplierCupboard;
    [JsonProperty(PropertyName = "Multiplier Outside Building Privilege")]
    public float MultiplierNonCupboard;
    [JsonProperty(PropertyName = "Items' Multipliers", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, float> MultiplierItems;
}


```

---

## LootHacker by zeeuss - Unlocks codelocks on storages.

```csharp
using UnityEngine;
using Newtonsoft.Json;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Loot Hacker", "Zeeuss", "0.1.2")]
[Description("Unlocks codelocks on storages")]
public class LootHacker : RustPlugin
{
    private const string PermissionUseCmnd;
    private const string PermissionUseItem;
    private void Init();
    protected override void LoadDefaultMessages();
    private void OnPlayerInput(BasePlayer player, InputState input);
    [ChatCommand("loothack")]
    private void UnlockCommand(BasePlayer player, string command, string[] args);
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Seconds until storage unlocks")]
        public float unlockTime;
        [JsonProperty(PropertyName = "Consume Targeting Computer on use")]
        public bool consumeComp;
    }

    private bool LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConfig(ConfigData config);
    private static int Layers;
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Seconds until storage unlocks")]
    public float unlockTime;
    [JsonProperty(PropertyName = "Consume Targeting Computer on use")]
    public bool consumeComp;
}


```

---

## LootLogs by k1lly0u - Log all items deposited and removed from storage, stash and oven containers

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using UnityEngine;
using System.IO;
using System.Text;
using Newtonsoft.Json;
using Oxide.Core;

Oxide.Plugins
[Info("LootLogs", "k1lly0u", "0.2.1"), Description("Log all items deposited and removed from storage, stash and oven containers")]
 class LootLogs : RustPlugin
{
    private readonly Hash<ItemId, LootData> m_TrackedItems;
    private void Init();
    private void OnServerInitialized();
    protected override void LoadDefaultMessages();
    private void OnEntityDeath(StorageContainer entity, HitInfo hitInfo);
    private void OnItemAddedToContainer(ItemContainer container, Item item);
    private void OnItemRemovedFromContainer(ItemContainer container, Item item);
    private readonly RaycastHit[] m_RaycastHits;
    private BaseEntity FindEntity(BasePlayer player);
    private static readonly object _logFileLock;
    private static readonly string _directory;
    private readonly Hash<string, Hash<string, List<string>>> m_QueuedLogs;
    private const string DESTROYED_CONTAINERS;
    private void QueueLogEntry(string path, string fileName, string text);
    private void OnServerSave();
    private void Log(string playername, string item, Type type, ulong id, string entityname, bool take);
    private void DeathLog(string type, string entityname, string id, string position, string killer);
    private void LogToFile(string path, string filename, string text);
    private void DeleteExpiredLogs();
    private void DeleteExpiredLogFolders(string subDirectory);
    [ConsoleCommand("testlog")]
    private void ConsoleCommandTestLog(ConsoleSystem.Arg arg);
    [ChatCommand("findid")]
    private void cmdFindID(BasePlayer player, string command, string[] args);
    private void TranslatedString(BasePlayer player, string key);
    private void FormatString(BasePlayer player, string key, object[] args);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty("Delete logs after X days (0 to disable)")]
        public int DeleteAfterDays { get; set; }
        public VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
}

private class ConfigData
{
    [JsonProperty("Delete logs after X days (0 to disable)")]
    public int DeleteAfterDays { get; set; }
    public VersionNumber Version { get; set; }
}


```

---

## LootMultiplier by Rick6 - Multiply items in all loot containers in the game

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Loot Multiplier", "Rick", "1.3.0")]
[Description("Multiply items in all loot containers in the game")]
public class LootMultiplier : RustPlugin
{
    private static LootMultiplier _instance;
    private bool _initialised;
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Global settings")]
        public GlobalSettings globalS;
        [JsonProperty(PropertyName = "Items and containers settings")]
        public ItemSettings itemS;
        public class GlobalSettings
        {
            [JsonProperty(PropertyName = "Default Multiplier for new containers")]
            public int defaultContainerMultiplier;
            [JsonProperty(PropertyName = "Default Multiplier for new Categories")]
            public int defaultCategoryMultiplier;
            [JsonProperty(PropertyName = "Multiply items with condition")]
            public bool multiplyItemsWithCondition;
            [JsonProperty(PropertyName = "Multiply blueprints")]
            public bool multiplyBlueprints;
            [JsonProperty(PropertyName = "Delay after spawning container to multiply it")]
            public float delay;
        }

        public class ItemSettings
        {
            [JsonProperty(PropertyName = "Containers list (shortPrefabName: multiplier)")]
            public Dictionary<string, int> containers;
            [JsonProperty(PropertyName = "Categories list (Category: multiplier)")]
            public Dictionary<string, int> categories;
            [JsonProperty(PropertyName = "Items list (shortname: multiplier)")]
            public Dictionary<string, int> items;
            [JsonProperty(PropertyName = "Item | Multiplier Blacklist", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public List<string> multiblacklist;
            [JsonProperty(PropertyName = "Item | Item Blacklist", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public List<string> blacklistitems;
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnLootSpawn(StorageContainer container);
    private void OnServerInitialised();
    private void Unload();
    private void Multiply(StorageContainer container);
    private int GetContainerMultiplier(string containerName);
    private int GetCategoryMultiplier(string category);
    private Dictionary<string, int> SortDictionary(Dictionary<string, int> dic);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Global settings")]
    public GlobalSettings globalS;
    [JsonProperty(PropertyName = "Items and containers settings")]
    public ItemSettings itemS;
    public class GlobalSettings
    {
        [JsonProperty(PropertyName = "Default Multiplier for new containers")]
        public int defaultContainerMultiplier;
        [JsonProperty(PropertyName = "Default Multiplier for new Categories")]
        public int defaultCategoryMultiplier;
        [JsonProperty(PropertyName = "Multiply items with condition")]
        public bool multiplyItemsWithCondition;
        [JsonProperty(PropertyName = "Multiply blueprints")]
        public bool multiplyBlueprints;
        [JsonProperty(PropertyName = "Delay after spawning container to multiply it")]
        public float delay;
    }

    public class ItemSettings
    {
        [JsonProperty(PropertyName = "Containers list (shortPrefabName: multiplier)")]
        public Dictionary<string, int> containers;
        [JsonProperty(PropertyName = "Categories list (Category: multiplier)")]
        public Dictionary<string, int> categories;
        [JsonProperty(PropertyName = "Items list (shortname: multiplier)")]
        public Dictionary<string, int> items;
        [JsonProperty(PropertyName = "Item | Multiplier Blacklist", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> multiblacklist;
        [JsonProperty(PropertyName = "Item | Item Blacklist", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> blacklistitems;
    }

}

public class GlobalSettings
{
    [JsonProperty(PropertyName = "Default Multiplier for new containers")]
    public int defaultContainerMultiplier;
    [JsonProperty(PropertyName = "Default Multiplier for new Categories")]
    public int defaultCategoryMultiplier;
    [JsonProperty(PropertyName = "Multiply items with condition")]
    public bool multiplyItemsWithCondition;
    [JsonProperty(PropertyName = "Multiply blueprints")]
    public bool multiplyBlueprints;
    [JsonProperty(PropertyName = "Delay after spawning container to multiply it")]
    public float delay;
}

public class ItemSettings
{
    [JsonProperty(PropertyName = "Containers list (shortPrefabName: multiplier)")]
    public Dictionary<string, int> containers;
    [JsonProperty(PropertyName = "Categories list (Category: multiplier)")]
    public Dictionary<string, int> categories;
    [JsonProperty(PropertyName = "Items list (shortname: multiplier)")]
    public Dictionary<string, int> items;
    [JsonProperty(PropertyName = "Item | Multiplier Blacklist", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> multiblacklist;
    [JsonProperty(PropertyName = "Item | Item Blacklist", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> blacklistitems;
}


```

---

## LootPlus by misticos - Modify loot on the server

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;
using ConVar;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Rust;
using UnityEngine;
using Physics = UnityEngine.Physics;
using Pool = Facepunch.Pool;
using Random = System.Random;

Oxide.Plugins
[Info("Loot Plus", "Iv Misticos", "2.2.6")]
[Description("Modify loot on your server.")]
public class LootPlus : RustPlugin
{
    private static LootPlus _ins;
    private Random _random;
    private bool _initialized;
    private const string PermissionLootSave;
    private const string PermissionLootRefill;
    private const string PermissionLoadConfig;
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Refill Loot On Plugin Load")]
        public bool RefillOnLoad;
        [JsonProperty(PropertyName = "Process Corpses", NullValueHandling = NullValueHandling.Ignore)]
        public bool? ProcessCorpses;
        [JsonProperty(PropertyName = "Process Loot Containers", NullValueHandling = NullValueHandling.Ignore)]
        public bool? ProcessLootContainers;
        [JsonProperty(PropertyName = "Container Loot Save Command")]
        public string LootSaveCommand;
        [JsonProperty(PropertyName = "Container Refill Command")]
        public string LootRefillCommand;
        [JsonProperty(PropertyName = "Load Config Command")]
        public string LoadConfigCommand;
        [JsonProperty(PropertyName = "Containers", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ContainerData> Containers;
        [JsonProperty(PropertyName = "Debug")]
        public bool Debug;
    }

    private class ContainerData
    {
        [JsonProperty(PropertyName = "Entity Shortnames", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ShortnameData> Shortnames;
        [JsonProperty(PropertyName = "Process Corpses")]
        public bool ProcessCorpses;
        [JsonProperty(PropertyName = "Process Loot Containers")]
        public bool ProcessLootContainers;
        [JsonProperty(PropertyName = "Monument Prefabs", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Monuments;
        [JsonProperty(PropertyName = "Shuffle Items")]
        public bool ShuffleItems;
        [JsonProperty(PropertyName = "Allow Duplicate Items")]
        public bool AllowDuplicateItems;
        [JsonProperty(PropertyName = "Allow Duplicate Items With Different Skins")]
        public bool AllowDuplicateItemsDifferentSkins;
        [JsonProperty(PropertyName = "Remove Container")]
        public bool RemoveContainer;
        [JsonProperty(PropertyName = "Item Container Indexes",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<int> ContainerIndexes;
        [JsonProperty(PropertyName = "Replace Items")]
        public bool ReplaceItems;
        [JsonProperty(PropertyName = "Add Items")]
        public bool AddItems;
        [JsonProperty(PropertyName = "Modify Items")]
        public bool ModifyItems;
        [JsonProperty(PropertyName = "Modify Ignores Blueprint State")]
        public bool ModifyIgnoreBlueprint;
        [JsonProperty(PropertyName = "Fill With Default Items")]
        public bool DefaultLoot;
        [JsonProperty(PropertyName = "Online Condition")]
        public OnlineData Online;
        [JsonProperty(PropertyName = "Maximal Failures To Add An Item")]
        public int MaxRetries;
        [JsonProperty(PropertyName = "Capacity", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<CapacityData> Capacity;
        [JsonProperty(PropertyName = "Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ItemData> Items;
        public bool ShortnameFits(string shortname, bool api);
        public class ShortnameData
        {
            [JsonProperty(PropertyName = "Shortname")]
            public string Shortname;
            [JsonProperty(PropertyName = "Enable Regex")]
            public bool Regex;
            [JsonProperty(PropertyName = "Exclude")]
            public bool Exclude;
            [JsonProperty(PropertyName = "API Shortname")]
            public bool API;
            [JsonIgnore]
            public Regex ParsedRegex;
            public bool FitsExpression(string shortname);
        }

    }

    private class ItemData : ChanceData
    {
        [JsonProperty(PropertyName = "Item Shortname", NullValueHandling = NullValueHandling.Ignore)]
        public string Shortname;
        [JsonProperty(PropertyName = "Item Shortnames", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ShortnameData> Shortnames;
        [JsonProperty(PropertyName = "Item Name (Empty To Ignore)")]
        public string Name;
        [JsonProperty(PropertyName = "Is Blueprint")]
        public bool IsBlueprint;
        [JsonProperty(PropertyName = "Allow Stacking")]
        public bool AllowStacking;
        [JsonProperty(PropertyName = "Ignore Max Stack")]
        public bool IgnoreStack;
        [JsonProperty(PropertyName = "Ignore Max Condition")]
        public bool IgnoreCondition;
        [JsonProperty(PropertyName = "Remove Item")]
        public bool RemoveItem;
        [JsonProperty(PropertyName = "Replace Item With Default Loot")]
        public bool ReplaceDefaultLoot;
        [JsonProperty(PropertyName = "Conditions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ConditionData> Conditions;
        [JsonProperty(PropertyName = "Skins", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<SkinData> Skins;
        [JsonProperty(PropertyName = "Amount", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<AmountData> Amount;
        public bool ShortnameFits(Rarity rarity, ItemCategory category, string shortname);
        public bool ShortnameFits(Item item, ContainerData container);
        public class ShortnameData
        {
            [JsonProperty(PropertyName = "Shortname")]
            public string Shortname;
            [JsonProperty(PropertyName = "Rarity")]
            public string Rarity;
            [JsonProperty(PropertyName = "Category")]
            public string Category;
            [JsonProperty(PropertyName = "Enable Regex")]
            public bool Regex;
            [JsonProperty(PropertyName = "Exclude")]
            public bool Exclude;
            [JsonIgnore]
            public Regex ParsedRegex;
            public bool FitsExpression(Rarity rarity, ItemCategory category, string shortname);
        }

    }

    private class ConditionData : ChanceData
    {
        [JsonProperty(PropertyName = "Condition", NullValueHandling = NullValueHandling.Ignore)]
        public float? Condition;
        [JsonProperty(PropertyName = "Minimal Condition")]
        public float MinCondition;
        [JsonProperty(PropertyName = "Maximal Condition")]
        public float MaxCondition;
        [JsonProperty(PropertyName = "Max Condition Of Item")]
        public float MaxItemCondition;
        [JsonProperty(PropertyName = "Condition Rate")]
        public float ConditionRate;
        [JsonProperty(PropertyName = "Max Condition Rate")]
        public float MaxItemConditionRate;
        public void Modify(float condition, float maxCondition);
    }

    private class SkinData : ChanceData
    {
        [JsonProperty(PropertyName = "Skin")]
        public ulong Skin;
    }

    private class AmountData : ChanceData
    {
        [JsonProperty(PropertyName = "Amount", NullValueHandling = NullValueHandling.Ignore)]
        public int? Amount;
        [JsonProperty(PropertyName = "Minimal Amount")]
        public int MinAmount;
        [JsonProperty(PropertyName = "Maximal Amount")]
        public int MaxAmount;
        [JsonProperty(PropertyName = "Rate")]
        public float Rate;
        public void Modify(int amount);
    }

    private class CapacityData : ChanceData
    {
        [JsonProperty(PropertyName = "Capacity")]
        public int Capacity;
    }

    private class OnlineData
    {
        [JsonProperty(PropertyName = "Minimal Online")]
        public int MinOnline;
        [JsonProperty(PropertyName = "Maximal Online")]
        public int MaxOnline;
        public bool IsOkay();
    }

    public class ChanceData
    {
        [JsonProperty(PropertyName = "Chance")]
        public int Chance;
        public static T Select(IReadOnlyList<T> data);
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void CommandLootSave(IPlayer iplayer, string command, string[] args);
    private void CommandLootRefill(IPlayer player, string command, string[] args);
    private void CommandLoadConfig(IPlayer player, string command, string[] args);
    protected override void LoadDefaultMessages();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnEntitySpawned(BaseNetworkable entity);
    private void OnLootSpawn(BaseNetworkable entity);
    private void FillContainer(ItemContainer container, string apiShortname, int containerIndex);
    private void RunLootHandler(BaseNetworkable networkable, ItemContainer inventory, int containerIndex);
    private IEnumerator LootHandler(BaseNetworkable networkable, ItemContainer inventory, int containerIndex, string apiShortname);
    private IEnumerator HandleInventory(BaseNetworkable networkable, ItemContainer inventory, ContainerData container, List<ItemData> blacklisted);
    private void HandleAPIFill(ItemContainer container, string apiShortname, int containerIndex);
    private static IEnumerator HandleInventoryAddReplace(ItemContainer inventory, ContainerData container);
    private static IEnumerator HandleInventoryModify(ItemContainer inventory, ContainerData container, List<ItemData> blacklisted);
    private static Item GenerateDefaultItem(LootContainer lootContainer, IReadOnlyList<ItemData> blacklisted, ContainerData containerData, IList<Item> containerItems);
    private static void GetLootSpawnItem(LootSpawn loot, IReadOnlyList<ItemData> blacklisted, ContainerData containerData, IList<Item> containerItems, ICollection<Item> items);
    private static bool IsItemDuplicate(IEnumerable<Item> items, Item origin, ContainerData container);
    private static bool IsItemBlacklisted(Item item, IReadOnlyList<ItemData> blacklisted, ContainerData container);
    private static bool IsDuplicate(IReadOnlyList<Item> list, ContainerData container, ItemData dataItem, string shortname, ulong skin);
    private string GetMsg(string key, string userId);
    private static void PrintDebug(string message);
    private void LootRefill();
    private string GetMonumentName(Vector3 position);
    private static void Shuffle(IList<T> list);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Refill Loot On Plugin Load")]
    public bool RefillOnLoad;
    [JsonProperty(PropertyName = "Process Corpses", NullValueHandling = NullValueHandling.Ignore)]
    public bool? ProcessCorpses;
    [JsonProperty(PropertyName = "Process Loot Containers", NullValueHandling = NullValueHandling.Ignore)]
    public bool? ProcessLootContainers;
    [JsonProperty(PropertyName = "Container Loot Save Command")]
    public string LootSaveCommand;
    [JsonProperty(PropertyName = "Container Refill Command")]
    public string LootRefillCommand;
    [JsonProperty(PropertyName = "Load Config Command")]
    public string LoadConfigCommand;
    [JsonProperty(PropertyName = "Containers", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ContainerData> Containers;
    [JsonProperty(PropertyName = "Debug")]
    public bool Debug;
}

private class ContainerData
{
    [JsonProperty(PropertyName = "Entity Shortnames", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ShortnameData> Shortnames;
    [JsonProperty(PropertyName = "Process Corpses")]
    public bool ProcessCorpses;
    [JsonProperty(PropertyName = "Process Loot Containers")]
    public bool ProcessLootContainers;
    [JsonProperty(PropertyName = "Monument Prefabs", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Monuments;
    [JsonProperty(PropertyName = "Shuffle Items")]
    public bool ShuffleItems;
    [JsonProperty(PropertyName = "Allow Duplicate Items")]
    public bool AllowDuplicateItems;
    [JsonProperty(PropertyName = "Allow Duplicate Items With Different Skins")]
    public bool AllowDuplicateItemsDifferentSkins;
    [JsonProperty(PropertyName = "Remove Container")]
    public bool RemoveContainer;
    [JsonProperty(PropertyName = "Item Container Indexes",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<int> ContainerIndexes;
    [JsonProperty(PropertyName = "Replace Items")]
    public bool ReplaceItems;
    [JsonProperty(PropertyName = "Add Items")]
    public bool AddItems;
    [JsonProperty(PropertyName = "Modify Items")]
    public bool ModifyItems;
    [JsonProperty(PropertyName = "Modify Ignores Blueprint State")]
    public bool ModifyIgnoreBlueprint;
    [JsonProperty(PropertyName = "Fill With Default Items")]
    public bool DefaultLoot;
    [JsonProperty(PropertyName = "Online Condition")]
    public OnlineData Online;
    [JsonProperty(PropertyName = "Maximal Failures To Add An Item")]
    public int MaxRetries;
    [JsonProperty(PropertyName = "Capacity", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<CapacityData> Capacity;
    [JsonProperty(PropertyName = "Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ItemData> Items;
    public bool ShortnameFits(string shortname, bool api);
    public class ShortnameData
    {
        [JsonProperty(PropertyName = "Shortname")]
        public string Shortname;
        [JsonProperty(PropertyName = "Enable Regex")]
        public bool Regex;
        [JsonProperty(PropertyName = "Exclude")]
        public bool Exclude;
        [JsonProperty(PropertyName = "API Shortname")]
        public bool API;
        [JsonIgnore]
        public Regex ParsedRegex;
        public bool FitsExpression(string shortname);
    }

}

public class ShortnameData
{
    [JsonProperty(PropertyName = "Shortname")]
    public string Shortname;
    [JsonProperty(PropertyName = "Enable Regex")]
    public bool Regex;
    [JsonProperty(PropertyName = "Exclude")]
    public bool Exclude;
    [JsonProperty(PropertyName = "API Shortname")]
    public bool API;
    [JsonIgnore]
    public Regex ParsedRegex;
    public bool FitsExpression(string shortname);
}

private class ItemData : ChanceData
{
    [JsonProperty(PropertyName = "Item Shortname", NullValueHandling = NullValueHandling.Ignore)]
    public string Shortname;
    [JsonProperty(PropertyName = "Item Shortnames", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ShortnameData> Shortnames;
    [JsonProperty(PropertyName = "Item Name (Empty To Ignore)")]
    public string Name;
    [JsonProperty(PropertyName = "Is Blueprint")]
    public bool IsBlueprint;
    [JsonProperty(PropertyName = "Allow Stacking")]
    public bool AllowStacking;
    [JsonProperty(PropertyName = "Ignore Max Stack")]
    public bool IgnoreStack;
    [JsonProperty(PropertyName = "Ignore Max Condition")]
    public bool IgnoreCondition;
    [JsonProperty(PropertyName = "Remove Item")]
    public bool RemoveItem;
    [JsonProperty(PropertyName = "Replace Item With Default Loot")]
    public bool ReplaceDefaultLoot;
    [JsonProperty(PropertyName = "Conditions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ConditionData> Conditions;
    [JsonProperty(PropertyName = "Skins", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<SkinData> Skins;
    [JsonProperty(PropertyName = "Amount", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<AmountData> Amount;
    public bool ShortnameFits(Rarity rarity, ItemCategory category, string shortname);
    public bool ShortnameFits(Item item, ContainerData container);
    public class ShortnameData
    {
        [JsonProperty(PropertyName = "Shortname")]
        public string Shortname;
        [JsonProperty(PropertyName = "Rarity")]
        public string Rarity;
        [JsonProperty(PropertyName = "Category")]
        public string Category;
        [JsonProperty(PropertyName = "Enable Regex")]
        public bool Regex;
        [JsonProperty(PropertyName = "Exclude")]
        public bool Exclude;
        [JsonIgnore]
        public Regex ParsedRegex;
        public bool FitsExpression(Rarity rarity, ItemCategory category, string shortname);
    }

}

public class ShortnameData
{
    [JsonProperty(PropertyName = "Shortname")]
    public string Shortname;
    [JsonProperty(PropertyName = "Rarity")]
    public string Rarity;
    [JsonProperty(PropertyName = "Category")]
    public string Category;
    [JsonProperty(PropertyName = "Enable Regex")]
    public bool Regex;
    [JsonProperty(PropertyName = "Exclude")]
    public bool Exclude;
    [JsonIgnore]
    public Regex ParsedRegex;
    public bool FitsExpression(Rarity rarity, ItemCategory category, string shortname);
}

private class ConditionData : ChanceData
{
    [JsonProperty(PropertyName = "Condition", NullValueHandling = NullValueHandling.Ignore)]
    public float? Condition;
    [JsonProperty(PropertyName = "Minimal Condition")]
    public float MinCondition;
    [JsonProperty(PropertyName = "Maximal Condition")]
    public float MaxCondition;
    [JsonProperty(PropertyName = "Max Condition Of Item")]
    public float MaxItemCondition;
    [JsonProperty(PropertyName = "Condition Rate")]
    public float ConditionRate;
    [JsonProperty(PropertyName = "Max Condition Rate")]
    public float MaxItemConditionRate;
    public void Modify(float condition, float maxCondition);
}

private class SkinData : ChanceData
{
    [JsonProperty(PropertyName = "Skin")]
    public ulong Skin;
}

private class AmountData : ChanceData
{
    [JsonProperty(PropertyName = "Amount", NullValueHandling = NullValueHandling.Ignore)]
    public int? Amount;
    [JsonProperty(PropertyName = "Minimal Amount")]
    public int MinAmount;
    [JsonProperty(PropertyName = "Maximal Amount")]
    public int MaxAmount;
    [JsonProperty(PropertyName = "Rate")]
    public float Rate;
    public void Modify(int amount);
}

private class CapacityData : ChanceData
{
    [JsonProperty(PropertyName = "Capacity")]
    public int Capacity;
}

private class OnlineData
{
    [JsonProperty(PropertyName = "Minimal Online")]
    public int MinOnline;
    [JsonProperty(PropertyName = "Maximal Online")]
    public int MaxOnline;
    public bool IsOkay();
}

public class ChanceData
{
    [JsonProperty(PropertyName = "Chance")]
    public int Chance;
    public static T Select(IReadOnlyList<T> data);
}


```

---

## LootProtection by MrBlue - Protects lootables from being looted by other players

```csharp
using System.Collections.Generic;
using System.Text.RegularExpressions;

Oxide.Plugins
[Info("Loot Protection", "Wulf", "1.0.1")]
[Description("Protects lootables from being looted by other players")]
 class LootProtection : CovalencePlugin
{
    private const string permBypass;
    private const string permEnable;
    private void Init();
    protected override void LoadDefaultMessages();
    private object ProtectLoot(BasePlayer looter, BaseEntity entity, string entityId, string entityName);
    private object OnLootEntity(BasePlayer looter, BasePlayer sleeper);
    private object OnLootEntity(BasePlayer looter, LootableCorpse corpse);
    private object OnLootEntity(BasePlayer looter, DroppedItemContainer container);
    private void MigratePermission(string oldPerm, string newPerm);
}


```

---

## LootScaling by  - Scale loot spawn rate/density by player count

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Loot Scaling", "Kyrah Abattoir/Arainrr", "1.0.0", ResourceId = 1874)]
[Description("Scale loot spawn rate/density by player count.")]
internal class LootScaling : RustPlugin
{
    private void OnServerInitialized();
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Population Scaling")]
        public Dictionary<string, bool> populationScaling;
        public static ConfigData DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Population Scaling")]
    public Dictionary<string, bool> populationScaling;
    public static ConfigData DefaultConfig();
}


```

---

## LootScanner by VisEntities - Allows player to scan loot containers with binoculars

```csharp
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Oxide.Plugins;
using ProtoBuf;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using UnityEngine;

Oxide.Plugins
[Info("Loot Scanner", "Sorrow", "0.4.0")]
[Description("Allows player to scan loot container with binoculars")]
 class LootScanner : RustPlugin
{
    [PluginReference]
     Plugin PopupNotifications;
    private const int BinocularsId;
    private const string SupplyDrop;
    private const string CH47Crate;
    private static readonly Dictionary<string, string> PrefabList;
    private const string PermissionWorld;
    private const string PermissionPlayerContainer;
    private const string PermissionSupplyDrop;
    private const string PermissionCh47Crate;
    private const string PermissionPlayerCorpse;
    private const string PermissionAlivePlayer;
    private const string PermissionBackpack;
    internal static string SideOfGui;
    internal static float PositionY;
    internal static string ColorNone;
    internal static string ColorCommon;
    internal static string ColorUncommon;
    internal static string ColorRare;
    internal static string ColorVeryRare;
    internal static bool HideAirdrop;
    internal static bool HideCh47Crate;
    internal static bool HideCrashSiteCrate;
    internal static bool HideCommonLootableCrate;
    internal static bool HidePlayerName;
    internal static bool HideCorpseName;
    private void OnServerInitialized();
    private void OnPlayerInput(BasePlayer player, InputState input);
    private void ProcessBackpack(BasePlayer player, DroppedItemContainer backpack);
    private void ProcessTargetPlayer(BasePlayer player, BasePlayer targetPlayer);
    private void ProcessCorpseEntity(BasePlayer player, LootableCorpse corpse);
    private void ProcessStorageEntity(BasePlayer player, StorageContainer storage);
    private void SendPermissionMessage(BasePlayer player);
    private void CreateUi(BasePlayer player, string headerMessage, string contentMessage);
    private void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
    private static void InitPrefabList();
    private static string BuildScannerMessage(string scannerMessage, Item item);
    private static string BuildRarityScannerMessage(string scannerMessage, List<Item> itemList);
    private void SendInfoMessage(BasePlayer player, string message);
    private static string GetColorOfItem(Item item);
    private static BaseEntity GetEntityScanned(BasePlayer player, InputState input);
    private static string GetPrefabName(BaseNetworkable entity);
    private static string SplitPrefabName(string prefabName);
    private static string BeautifyPrefabName(string str);
    private static string BeautifyPermissionName(string str);
    private class UI
    {
        private static CuiElementContainer CreateElementContainer(string name, string color, string anchorMin, string anchorMax, string parent);
        private static void CreateLabel(string name, string parent, CuiElementContainer container, TextAnchor textAnchor, string text, string color, int fontSize, string anchorMin, string anchorMax);
        private static void CreateElement(string name, string parent, CuiElementContainer container, string anchorMin, string anchorMax, string color);
        public static CuiElementContainer ConstructScanUi(BasePlayer player, string title, string message);
    }

    private new void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
}

private class UI
{
    private static CuiElementContainer CreateElementContainer(string name, string color, string anchorMin, string anchorMax, string parent);
    private static void CreateLabel(string name, string parent, CuiElementContainer container, TextAnchor textAnchor, string text, string color, int fontSize, string anchorMin, string anchorMax);
    private static void CreateElement(string name, string parent, CuiElementContainer container, string anchorMin, string anchorMax, string color);
    public static CuiElementContainer ConstructScanUi(BasePlayer player, string title, string message);
}


```

---

## LootSnitch by August - Players with permission get messaged who/when a player is looting their corpse

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[Info("Loot Snitch", "August", "1.0.7")]
[Description("Players with permission get messaged who/when a player is looting their body.")]
public class LootSnitch : RustPlugin
{
    private const string PermUse;
    private const string PermOverride;
    private const string PermMangage;
    private bool IsEnabled;
    private int Cooldown;
     void Init();
    protected override void LoadDefaultMessages();
    private static Data data;
    private class Data
    {
        public Dictionary<string, long> Cooldowns;
    }

    private static readonly DateTime Epoch;
    public uint GetUnixTimestamp();
    private void OnLootEntityEnd(BasePlayer player, LootableCorpse corpse);
    [ChatCommand("snitch")]
    private void SnitchChatCommand(BasePlayer player, string cmd, string[] args);
    private string Lang(string key, string id, object[] args);
    private void ToggleSnitch(BasePlayer player);
}

private class Data
{
    public Dictionary<string, long> Cooldowns;
}


```

---

## LootSpawnpoints by  - Adds extra loot spawn points on custom maps

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Loot Spawnpoints", "Orange", "1.0.0")]
[Description("Add extra loot spawnpoints on custom maps")]
public class LootSpawnpoints : RustPlugin
{
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void cmdGetSpawnpointsConsole(ConsoleSystem.Arg arg);
    private void GetSpawnpoints();
    private bool IsAllowed(string shortName);
    private void CreateSpawnpoints();
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "A. Whitelist (keep empty to use blacklist)")]
        public List<string> whitelist;
        [JsonProperty(PropertyName = "B. Blacklist")]
        public List<string> blacklist;
        [JsonProperty(PropertyName = "C. Respawn timers")]
        public Dictionary<string, int> respawnTimers;
        [JsonProperty(PropertyName = "Default respawn timer")]
        public int defaultRespawnTimer;
        [JsonProperty(PropertyName = "Allow stacking")]
        public bool allowStacking;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private const string filename;
    private List<BaseEntityToSave> data;
    private void LoadData();
    private void SaveData();
    private class BaseEntityToSave
    {
        public string prefab;
        public Quaternion rotation;
        public Vector3 position;
    }

    private class Spawner : MonoBehaviour
    {
        public string prefab;
        public int time;
        private void Start();
        private void Spawn();
        private bool HasContainerNearby();
    }

}

private class ConfigData
{
    [JsonProperty(PropertyName = "A. Whitelist (keep empty to use blacklist)")]
    public List<string> whitelist;
    [JsonProperty(PropertyName = "B. Blacklist")]
    public List<string> blacklist;
    [JsonProperty(PropertyName = "C. Respawn timers")]
    public Dictionary<string, int> respawnTimers;
    [JsonProperty(PropertyName = "Default respawn timer")]
    public int defaultRespawnTimer;
    [JsonProperty(PropertyName = "Allow stacking")]
    public bool allowStacking;
}

private class BaseEntityToSave
{
    public string prefab;
    public Quaternion rotation;
    public Vector3 position;
}

private class Spawner : MonoBehaviour
{
    public string prefab;
    public int time;
    private void Start();
    private void Spawn();
    private bool HasContainerNearby();
}


```

---

## Lottery by sami37 - Allows players to roll number to win more money or points

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Lottery", "Sami37", "1.2.5", ResourceId = 2145)]
internal class Lottery : RustPlugin
{
    [PluginReference("Economics")]
     Plugin Economy;
    [PluginReference("ServerRewards")]
     Plugin ServerRewards;
    [PluginReference("HumanNPC")]
     Plugin HumanNPC;
    internal class playerinfo
    {
        public int multiplicator { get; set; }
        public double currentbet { get; set; }
        public double totalbet { get; set; }
    }

     int[] GetIntArray(int num);
    private object DoRay(Vector3 Pos, Vector3 Aim);
    private bool newConfig;
    private bool UseSR;
    private bool UseNPC;
    private bool AutoCloseAfterPlaceingBet;
    public Dictionary<ulong, playerinfo> Currentbet;
    private string container;
    private string containerwin;
    private string BackgroundUrl;
    private string BackgroundColor;
    private string WinBackgroundUrl;
    private string WinBackgroundColor;
    private string anchorMin;
    private string anchorMax;
    private DynamicConfigFile data;
    private List<object> NPCID;
    private double jackpot;
    private double SRMinBet;
    private double SRjackpot;
    private double MinBetjackpot;
    private double MinBetjackpotEco;
    private int JackpotNumber;
    private int SRJackpotNumber;
    private int DefaultMaxRange;
    private int DefaultMinRange;
    public Dictionary<string, object> IndividualRates { get; set; }
    public Dictionary<string, int> SRRates { get; set; }
    private Dictionary<string, object> DefaultWinRates;
    private Dictionary<string, object> SRWinRates;
    private List<object> DefaultBasePoint;
    private FieldInfo serverinput;
    private Vector3 eyesAdjust;
    private string MainContainer;
    private T GetConfigValue(string category, string setting, T defaultValue);
    private void SetConfigValue(string category, string setting, T newValue);
    protected override void LoadDefaultConfig();
     void LoadConfig();
    static Dictionary<string, object> DefaultPay();
    static List<object> DefaultSRPay();
    static Dictionary<string, object> DefautSRWinPay();
     void LoadDefaultMessages();
     void OnServerInitialized();
     void Unload();
     void SaveData(Dictionary<ulong, playerinfo> datas);
    private CuiElement CreateImage(string panelName, bool win);
     void RefreshUI(BasePlayer player, string[] args);
     void GUIDestroy(BasePlayer player);
     void ShowLotery(BasePlayer player, string[] args);
     void ShowSrLotery(BasePlayer player, string[] args);
    private static CuiElementContainer ChangeBonusPage(int pageless, int pagemore);
    private object FindReward(BasePlayer player, int bet, int reference, int multiplicator);
    [ChatCommand("lot")]
    private void cmdLotery(BasePlayer player, string command, string[] args);
     void OnUseNPC(BasePlayer npc, BasePlayer player, Vector3 destination);
    [ConsoleCommand("cmdDestroyUI")]
     void cmdDestroyUI(ConsoleSystem.Arg arg);
    [ConsoleCommand("cmdLess")]
     void cmdLess(ConsoleSystem.Arg arg);
    [ConsoleCommand("cmdBet")]
     void cmdBet(ConsoleSystem.Arg arg);
    [ConsoleCommand("cmdPlus")]
     void cmdPlus(ConsoleSystem.Arg arg);
    [ConsoleCommand("cmdPage")]
     void cmdPage(ConsoleSystem.Arg arg);
    [ConsoleCommand("cmdPlaceBet")]
     void cmdPlaceBet(ConsoleSystem.Arg arg);
    [ConsoleCommand("cmdResetBet")]
     void cmdResetBet(ConsoleSystem.Arg arg);
}

internal class playerinfo
{
    public int multiplicator { get; set; }
    public double currentbet { get; set; }
    public double totalbet { get; set; }
}


```

---

## LustyMap by k1lly0u - In-game map and minimap GUI

```csharp
using System;
using System.Text;
using System.Collections.Generic;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;
using System.Linq;
using System.Globalization;
using System.IO;
using System.Drawing;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Lusty Map", "k1lly0u", "2.1.42")]
[Description("In-game map and minimap GUI")]
 class LustyMap : RustPlugin
{
    [PluginReference]
     Plugin Clans;
     Plugin EventManager;
     Plugin Friends;
    [PluginReference]
     ImageLibrary ImageLibrary;
    static MapSplitter mapSplitter;
    static LustyMap instance;
    static float mapSize;
    static string mapSeed;
    static string worldSize;
    static string level;
    private bool activated;
    private bool isNewSave;
     MarkerData storedMarkers;
    private DynamicConfigFile markerData;
    private Dictionary<string, MapUser> mapUsers;
    private List<MapMarker> staticMarkers;
    private Dictionary<string, MapMarker> customMarkers;
    private Dictionary<string, MapMarker> temporaryMarkers;
    private Dictionary<uint, ActiveEntity> entityMarkers;
    private Dictionary<string, List<string>> clanData;
    static string dataDirectory;
     class MapUser : MonoBehaviour
    {
        private Dictionary<string, List<string>> friends;
        public HashSet<string> friendList;
        public BasePlayer player;
        public MapMode mode;
        public MapMode lastMode;
        private MapMarker marker;
        private int mapX;
        private int mapZ;
        private int currentX;
        private int currentZ;
        private int mapZoom;
        private bool mapOpen;
        private bool inEvent;
        private bool adminMode;
        private int changeCount;
        private double lastChange;
        private bool isBlocked;
        private ConfigData.SpamOptions spam;
        private bool afkDisabled;
        private int lastMoveTime;
        private float lastX;
        private float lastZ;
         void Awake();
         void OnDestroy();
        public void InitializeComponent();
        private void FillFriendList();
        private void UpdateMembers();
        public float Rotation();
        public int Position(bool x);
        public void Position(bool x, int pos);
        public void ToggleMapType(MapMode mapMode);
        public void UpdateMap();
        public bool HasMapOpen();
        public int Zoom();
        public void Zoom(bool zoomIn);
        private void SwitchZoom(int zoom);
        public int Current(bool x);
        public void Current(bool x, int num);
        private void CheckForChange();
        private bool IsSpam();
        private void Block();
        private void Unblock();
        public bool InEvent();
        public bool IsAdmin { get; set; }
        public void ToggleEvent(bool isPlaying);
        public void ToggleAdmin(bool enabled);
        public void DestroyUI();
        private void UpdateMarker();
        public MapMarker GetMarker();
        public void ToggleMain();
        public void DisableUser();
        public void EnableUser();
        public void EnterEvent();
        public void ExitEvent();
        public bool HasFriendList(string name);
        public void AddFriendList(string name, List<string> friendlist);
        public void RemoveFriendList(string name);
        public void UpdateFriendList(string name, List<string> friendlist);
        public bool HasFriend(string name, string friendId);
        public void AddFriend(string name, string friendId);
        public void RemoveFriend(string name, string friendId);
    }

     MapUser GetUser(BasePlayer player);
     MapUser GetUserByID(string playerId);
     class ActiveEntity : MonoBehaviour
    {
        public BaseEntity entity;
        private MapMarker marker;
        public AEType type;
        private string icon;
         void Awake();
         void OnDestroy();
        public void SetType(AEType type);
        public MapMarker GetMarker();
         void UpdatePosition();
    }

     class MapMarker
    {
        public string name { get; set; }
        public float x { get; set; }
        public float z { get; set; }
        public float r { get; set; }
        public string icon { get; set; }
    }

    static class MapSettings
    {
        static public bool minimap;
        static public bool complexmap;
        static public bool monuments;
        static public bool names;
        static public bool compass;
        static public bool caves;
        static public bool plane;
        static public bool heli;
        static public bool supply;
        static public bool debris;
        static public bool player;
        static public bool allplayers;
        static public bool friends;
        static public bool vending;
        static public bool forcedzoom;
        static public bool cars;
        static public bool tanks;
        static public int zoomlevel;
    }

     class LMUI
    {
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor, string parent);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
        static public void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
    }

     void OnNewSave(string filename);
     void Loaded();
     void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void OnEntitySpawned(BaseEntity entity);
     void OnEntityKill(BaseNetworkable entity);
     void Unload();
    static class LustyUI
    {
        public static string Main;
        public static string Mini;
        public static string Complex;
        public static string MainOverlay;
        public static string MiniOverlay;
        public static string ComplexOverlay;
        public static string Buttons;
        public static string MainMin;
        public static string MainMax;
        public static string MiniMin;
        public static string MiniMax;
        public static CuiElementContainer StaticMain;
        public static CuiElementContainer StaticMini;
        public static Dictionary<int, CuiElementContainer[,]> StaticComplex;
        private static Dictionary<ulong, List<string>> OpenUI;
        public static void RenameComponents();
        public static void AddBaseUI(BasePlayer player, MapMode type);
        private static void AddElementIds(BasePlayer player, CuiElementContainer container);
        public static void DestroyUI(BasePlayer player);
        public static string Color(string hexColor, float alpha);
    }

     void GenerateMaps(bool main, bool mini, bool complex);
     void SetMinimapSize();
     void CreateStaticMain();
     void CreateStaticMini();
     void CreateStaticComplex();
    static int ZoomToCount(int zoom);
    static int CountToZoom(int count);
     void ActivateMaps();
     void AddMapCompass(BasePlayer player, CuiElementContainer mapContainer, string panel, int fontsize, string offsetMin, string offsetMax);
     void AddMapButtons(BasePlayer player);
     void CreateShrunkUI(BasePlayer player);
     void OpenMainMap(BasePlayer player);
     void OpenMiniMap(BasePlayer player);
     void UpdateOverlay(BasePlayer player, string panel, string posMin, string posMax, float iconsize);
     void AddIconToMap(CuiElementContainer mapContainer, string panel, string image, string name, float iconsize, float posX, float posZ);
     void OpenComplexMap(BasePlayer player);
     void UpdateCompOverlay(BasePlayer player);
     void AddComplexIcon(CuiElementContainer mapContainer, string panel, string image, string name, float iconsize, float x, float z, double colStart, double colEnd, double rowStart, double rowEnd);
    [ConsoleCommand("LMUI_Control")]
    private void cmdLustyControl(ConsoleSystem.Arg arg);
    [ConsoleCommand("resetmap")]
     void ccmdResetmap(ConsoleSystem.Arg arg);
    [ChatCommand("map")]
     void cmdOpenMap(BasePlayer player, string command, string[] args);
    [ChatCommand("marker")]
     void cmdMarker(BasePlayer player, string command, string[] args);
    private void AddTemporaryEntityMarker(BaseEntity entity);
    private void LoadSettings();
    private void FindStaticMarkers();
    private void FindVendingMachines();
    private void FindVehicles();
    static double GrabCurrentTime();
    public static string RemoveSpecialCharacters(string str);
    static float GetPosition(float pos);
    static int GetDirection(float rotation);
     void EnableMaps(BasePlayer player);
     void DisableMaps(BasePlayer player);
     bool AddMarker(float x, float z, string name, string icon, float r);
     void UpdateMarker(float x, float z, string name, string icon, float r);
     bool RemoveMarker(string name);
     bool AddTemporaryMarker(float x, float z, string name, string icon, float r);
     void UpdateTemporaryMarker(float x, float z, string name, string icon, float r);
     bool RemoveTemporaryMarker(string name);
     bool RemoveTemporaryMarkerStartsWith(string name);
     bool AddFriendList(string playerId, string name, List<string> list, bool bypass);
     bool RemoveFriendList(string playerId, string name, bool bypass);
     bool UpdateFriendList(string playerId, string name, List<string> list, bool bypass);
     bool AddFriend(string playerId, string name, string friendId, bool bypass);
     bool RemoveFriend(string playerId, string name, string friendId, bool bypass);
     string GetMap();
     bool SplitMap(int splices);
     void JoinedEvent(BasePlayer player);
     void LeftEvent(BasePlayer player);
     List<string> GetFriends(ulong playerId);
     void OnFriendAdded(object playerId, object friendId);
     void OnFriendRemoved(object playerId, object friendId);
     void GetClans();
     string GetClan(ulong playerId);
     List<string> GetClanMembers(string clanTag);
     void OnClanCreate(string tag);
     void OnClanUpdate(string tag);
     void OnClanDestroy(string tag);
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Friend Options")]
        public FriendOptions Friends { get; set; }
        [JsonProperty(PropertyName = "Marker Options")]
        public MapMarkers Markers { get; set; }
        [JsonProperty(PropertyName = "Map - Main Options")]
        public MapOptions Map { get; set; }
        [JsonProperty(PropertyName = "Map - Mini Options")]
        public Minimap MiniMap { get; set; }
        [JsonProperty(PropertyName = "Map - Complex Options")]
        public ComplexMap ComplexOptions { get; set; }
        [JsonProperty(PropertyName = "Spam Options")]
        public SpamOptions Spam { get; set; }
        public class FriendOptions
        {
            [JsonProperty(PropertyName = "Allow custom friend lists from other plugins")]
            public bool AllowCustomLists { get; set; }
            [JsonProperty(PropertyName = "Enable clans support")]
            public bool UseClans { get; set; }
            [JsonProperty(PropertyName = "Enable friends support")]
            public bool UseFriends { get; set; }
        }

        public class MapMarkers
        {
            [JsonProperty(PropertyName = "Show all players")]
            public bool ShowAllPlayers { get; set; }
            [JsonProperty(PropertyName = "Show caves")]
            public bool ShowCaves { get; set; }
            [JsonProperty(PropertyName = "Show debris")]
            public bool ShowDebris { get; set; }
            [JsonProperty(PropertyName = "Show friends and clanmates")]
            public bool ShowFriends { get; set; }
            [JsonProperty(PropertyName = "Show helicopters")]
            public bool ShowHelicopters { get; set; }
            [JsonProperty(PropertyName = "Show marker names")]
            public bool ShowMarkerNames { get; set; }
            [JsonProperty(PropertyName = "Show monuments")]
            public bool ShowMonuments { get; set; }
            [JsonProperty(PropertyName = "Show planes")]
            public bool ShowPlanes { get; set; }
            [JsonProperty(PropertyName = "Show self")]
            public bool ShowPlayer { get; set; }
            [JsonProperty(PropertyName = "Show supply drops")]
            public bool ShowSupplyDrops { get; set; }
            [JsonProperty(PropertyName = "Show vending machines (public broadcast only)")]
            public bool ShowPublicVendingMachines { get; set; }
            [JsonProperty(PropertyName = "Show cars (un-occupied only)")]
            public bool ShowCars { get; set; }
            [JsonProperty(PropertyName = "Show tanks")]
            public bool ShowTanks { get; set; }
        }

        public class MapOptions
        {
            [JsonProperty(PropertyName = "Enable AFK tracking")]
            public bool EnableAFKTracking { get; set; }
            [JsonProperty(PropertyName = "Hide event players")]
            public bool HideEventPlayers { get; set; }
            [JsonProperty(PropertyName = "Open map on for player's when they connect")]
            public bool StartOpen { get; set; }
            [JsonProperty(PropertyName = "Show map compass")]
            public bool ShowCompass { get; set; }
            [JsonProperty(PropertyName = "Map image options")]
            public MapImages MapImage { get; set; }
            [JsonProperty(PropertyName = "Map update time (seconds)")]
            public float UpdateSpeed { get; set; }
            public class MapImages
            {
                [JsonProperty(PropertyName = "Beancan.io API key (if applicable)")]
                public string APIKey { get; set; }
                [JsonProperty(PropertyName = "Use custom map")]
                public bool CustomMap_Use { get; set; }
                [JsonProperty(PropertyName = "Custom map filename")]
                public string CustomMap_Filename { get; set; }
            }

        }

        public class Minimap
        {
            [JsonProperty(PropertyName = "Enable the minimap")]
            public bool UseMinimap { get; set; }
            [JsonProperty(PropertyName = "Minimap horizontal scale")]
            public float HorizontalScale { get; set; }
            [JsonProperty(PropertyName = "Minimap vertical scale")]
            public float VerticalScale { get; set; }
            [JsonProperty(PropertyName = "Minimap docked on the left side of the screen")]
            public bool OnLeftSide { get; set; }
            [JsonProperty(PropertyName = "Minimap offset from side of the screen")]
            public float OffsetSide { get; set; }
            [JsonProperty(PropertyName = "Minimap offset from top of the screen")]
            public float OffsetTop { get; set; }
        }

        public class ComplexMap
        {
            [JsonProperty(PropertyName = "Enable the complex map")]
            public bool UseComplexMap { get; set; }
            [JsonProperty(PropertyName = "Force complex zoom mode")]
            public bool ForceMapZoom { get; set; }
            [JsonProperty(PropertyName = "Forced zoom number (1, 2 or 3)")]
            public int ForcedZoomLevel { get; set; }
        }

        public class SpamOptions
        {
            [JsonProperty(PropertyName = "Allowed time between map changes")]
            public int TimeBetweenAttempts { get; set; }
            [JsonProperty(PropertyName = "Attempts before warning the user they are spamming")]
            public int WarningAttempts { get; set; }
            [JsonProperty(PropertyName = "Attempts before disabling the users map")]
            public int DisableAttempts { get; set; }
            [JsonProperty(PropertyName = "Amount of time a users map will be disabled")]
            public int DisableSeconds { get; set; }
            [JsonProperty(PropertyName = "Enable spam monitoring")]
            public bool Enabled { get; set; }
        }

    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     void SaveMarkers();
     void LoadData();
     class MarkerData
    {
        public Dictionary<string, MapMarker> data;
    }

    private string GetImage(string name);
     void ValidateImages();
    private void LoadImages();
    private void LoadMapImage();
     void DownloadMapImage();
     void GetQueueID();
     void CheckAvailability(string queueId);
     void ProcessResponse(string queueId, string response);
     void GetMapURL(string queueId);
     void DownloadMap(string url);
     void ScaleMapImage();
     void AttemptSplit(int attempts);
     class MapSplitter
    {
        public bool SplitMap(string imageId, int amount);
        public System.Drawing.Image ImageFromStorage(uint imageId);
        public byte[] ResizeImage(Image image, int pixels);
    }

     string msg(string key, string playerid);
    protected override void LoadDefaultMessages();
}

 class MapUser : MonoBehaviour
{
    private Dictionary<string, List<string>> friends;
    public HashSet<string> friendList;
    public BasePlayer player;
    public MapMode mode;
    public MapMode lastMode;
    private MapMarker marker;
    private int mapX;
    private int mapZ;
    private int currentX;
    private int currentZ;
    private int mapZoom;
    private bool mapOpen;
    private bool inEvent;
    private bool adminMode;
    private int changeCount;
    private double lastChange;
    private bool isBlocked;
    private ConfigData.SpamOptions spam;
    private bool afkDisabled;
    private int lastMoveTime;
    private float lastX;
    private float lastZ;
     void Awake();
     void OnDestroy();
    public void InitializeComponent();
    private void FillFriendList();
    private void UpdateMembers();
    public float Rotation();
    public int Position(bool x);
    public void Position(bool x, int pos);
    public void ToggleMapType(MapMode mapMode);
    public void UpdateMap();
    public bool HasMapOpen();
    public int Zoom();
    public void Zoom(bool zoomIn);
    private void SwitchZoom(int zoom);
    public int Current(bool x);
    public void Current(bool x, int num);
    private void CheckForChange();
    private bool IsSpam();
    private void Block();
    private void Unblock();
    public bool InEvent();
    public bool IsAdmin { get; set; }
    public void ToggleEvent(bool isPlaying);
    public void ToggleAdmin(bool enabled);
    public void DestroyUI();
    private void UpdateMarker();
    public MapMarker GetMarker();
    public void ToggleMain();
    public void DisableUser();
    public void EnableUser();
    public void EnterEvent();
    public void ExitEvent();
    public bool HasFriendList(string name);
    public void AddFriendList(string name, List<string> friendlist);
    public void RemoveFriendList(string name);
    public void UpdateFriendList(string name, List<string> friendlist);
    public bool HasFriend(string name, string friendId);
    public void AddFriend(string name, string friendId);
    public void RemoveFriend(string name, string friendId);
}

 class ActiveEntity : MonoBehaviour
{
    public BaseEntity entity;
    private MapMarker marker;
    public AEType type;
    private string icon;
     void Awake();
     void OnDestroy();
    public void SetType(AEType type);
    public MapMarker GetMarker();
     void UpdatePosition();
}

 class MapMarker
{
    public string name { get; set; }
    public float x { get; set; }
    public float z { get; set; }
    public float r { get; set; }
    public string icon { get; set; }
}

static class MapSettings
{
    static public bool minimap;
    static public bool complexmap;
    static public bool monuments;
    static public bool names;
    static public bool compass;
    static public bool caves;
    static public bool plane;
    static public bool heli;
    static public bool supply;
    static public bool debris;
    static public bool player;
    static public bool allplayers;
    static public bool friends;
    static public bool vending;
    static public bool forcedzoom;
    static public bool cars;
    static public bool tanks;
    static public int zoomlevel;
}

 class LMUI
{
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor, string parent);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    static public void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
}

static class LustyUI
{
    public static string Main;
    public static string Mini;
    public static string Complex;
    public static string MainOverlay;
    public static string MiniOverlay;
    public static string ComplexOverlay;
    public static string Buttons;
    public static string MainMin;
    public static string MainMax;
    public static string MiniMin;
    public static string MiniMax;
    public static CuiElementContainer StaticMain;
    public static CuiElementContainer StaticMini;
    public static Dictionary<int, CuiElementContainer[,]> StaticComplex;
    private static Dictionary<ulong, List<string>> OpenUI;
    public static void RenameComponents();
    public static void AddBaseUI(BasePlayer player, MapMode type);
    private static void AddElementIds(BasePlayer player, CuiElementContainer container);
    public static void DestroyUI(BasePlayer player);
    public static string Color(string hexColor, float alpha);
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Friend Options")]
    public FriendOptions Friends { get; set; }
    [JsonProperty(PropertyName = "Marker Options")]
    public MapMarkers Markers { get; set; }
    [JsonProperty(PropertyName = "Map - Main Options")]
    public MapOptions Map { get; set; }
    [JsonProperty(PropertyName = "Map - Mini Options")]
    public Minimap MiniMap { get; set; }
    [JsonProperty(PropertyName = "Map - Complex Options")]
    public ComplexMap ComplexOptions { get; set; }
    [JsonProperty(PropertyName = "Spam Options")]
    public SpamOptions Spam { get; set; }
    public class FriendOptions
    {
        [JsonProperty(PropertyName = "Allow custom friend lists from other plugins")]
        public bool AllowCustomLists { get; set; }
        [JsonProperty(PropertyName = "Enable clans support")]
        public bool UseClans { get; set; }
        [JsonProperty(PropertyName = "Enable friends support")]
        public bool UseFriends { get; set; }
    }

    public class MapMarkers
    {
        [JsonProperty(PropertyName = "Show all players")]
        public bool ShowAllPlayers { get; set; }
        [JsonProperty(PropertyName = "Show caves")]
        public bool ShowCaves { get; set; }
        [JsonProperty(PropertyName = "Show debris")]
        public bool ShowDebris { get; set; }
        [JsonProperty(PropertyName = "Show friends and clanmates")]
        public bool ShowFriends { get; set; }
        [JsonProperty(PropertyName = "Show helicopters")]
        public bool ShowHelicopters { get; set; }
        [JsonProperty(PropertyName = "Show marker names")]
        public bool ShowMarkerNames { get; set; }
        [JsonProperty(PropertyName = "Show monuments")]
        public bool ShowMonuments { get; set; }
        [JsonProperty(PropertyName = "Show planes")]
        public bool ShowPlanes { get; set; }
        [JsonProperty(PropertyName = "Show self")]
        public bool ShowPlayer { get; set; }
        [JsonProperty(PropertyName = "Show supply drops")]
        public bool ShowSupplyDrops { get; set; }
        [JsonProperty(PropertyName = "Show vending machines (public broadcast only)")]
        public bool ShowPublicVendingMachines { get; set; }
        [JsonProperty(PropertyName = "Show cars (un-occupied only)")]
        public bool ShowCars { get; set; }
        [JsonProperty(PropertyName = "Show tanks")]
        public bool ShowTanks { get; set; }
    }

    public class MapOptions
    {
        [JsonProperty(PropertyName = "Enable AFK tracking")]
        public bool EnableAFKTracking { get; set; }
        [JsonProperty(PropertyName = "Hide event players")]
        public bool HideEventPlayers { get; set; }
        [JsonProperty(PropertyName = "Open map on for player's when they connect")]
        public bool StartOpen { get; set; }
        [JsonProperty(PropertyName = "Show map compass")]
        public bool ShowCompass { get; set; }
        [JsonProperty(PropertyName = "Map image options")]
        public MapImages MapImage { get; set; }
        [JsonProperty(PropertyName = "Map update time (seconds)")]
        public float UpdateSpeed { get; set; }
        public class MapImages
        {
            [JsonProperty(PropertyName = "Beancan.io API key (if applicable)")]
            public string APIKey { get; set; }
            [JsonProperty(PropertyName = "Use custom map")]
            public bool CustomMap_Use { get; set; }
            [JsonProperty(PropertyName = "Custom map filename")]
            public string CustomMap_Filename { get; set; }
        }

    }

    public class Minimap
    {
        [JsonProperty(PropertyName = "Enable the minimap")]
        public bool UseMinimap { get; set; }
        [JsonProperty(PropertyName = "Minimap horizontal scale")]
        public float HorizontalScale { get; set; }
        [JsonProperty(PropertyName = "Minimap vertical scale")]
        public float VerticalScale { get; set; }
        [JsonProperty(PropertyName = "Minimap docked on the left side of the screen")]
        public bool OnLeftSide { get; set; }
        [JsonProperty(PropertyName = "Minimap offset from side of the screen")]
        public float OffsetSide { get; set; }
        [JsonProperty(PropertyName = "Minimap offset from top of the screen")]
        public float OffsetTop { get; set; }
    }

    public class ComplexMap
    {
        [JsonProperty(PropertyName = "Enable the complex map")]
        public bool UseComplexMap { get; set; }
        [JsonProperty(PropertyName = "Force complex zoom mode")]
        public bool ForceMapZoom { get; set; }
        [JsonProperty(PropertyName = "Forced zoom number (1, 2 or 3)")]
        public int ForcedZoomLevel { get; set; }
    }

    public class SpamOptions
    {
        [JsonProperty(PropertyName = "Allowed time between map changes")]
        public int TimeBetweenAttempts { get; set; }
        [JsonProperty(PropertyName = "Attempts before warning the user they are spamming")]
        public int WarningAttempts { get; set; }
        [JsonProperty(PropertyName = "Attempts before disabling the users map")]
        public int DisableAttempts { get; set; }
        [JsonProperty(PropertyName = "Amount of time a users map will be disabled")]
        public int DisableSeconds { get; set; }
        [JsonProperty(PropertyName = "Enable spam monitoring")]
        public bool Enabled { get; set; }
    }

}

public class FriendOptions
{
    [JsonProperty(PropertyName = "Allow custom friend lists from other plugins")]
    public bool AllowCustomLists { get; set; }
    [JsonProperty(PropertyName = "Enable clans support")]
    public bool UseClans { get; set; }
    [JsonProperty(PropertyName = "Enable friends support")]
    public bool UseFriends { get; set; }
}

public class MapMarkers
{
    [JsonProperty(PropertyName = "Show all players")]
    public bool ShowAllPlayers { get; set; }
    [JsonProperty(PropertyName = "Show caves")]
    public bool ShowCaves { get; set; }
    [JsonProperty(PropertyName = "Show debris")]
    public bool ShowDebris { get; set; }
    [JsonProperty(PropertyName = "Show friends and clanmates")]
    public bool ShowFriends { get; set; }
    [JsonProperty(PropertyName = "Show helicopters")]
    public bool ShowHelicopters { get; set; }
    [JsonProperty(PropertyName = "Show marker names")]
    public bool ShowMarkerNames { get; set; }
    [JsonProperty(PropertyName = "Show monuments")]
    public bool ShowMonuments { get; set; }
    [JsonProperty(PropertyName = "Show planes")]
    public bool ShowPlanes { get; set; }
    [JsonProperty(PropertyName = "Show self")]
    public bool ShowPlayer { get; set; }
    [JsonProperty(PropertyName = "Show supply drops")]
    public bool ShowSupplyDrops { get; set; }
    [JsonProperty(PropertyName = "Show vending machines (public broadcast only)")]
    public bool ShowPublicVendingMachines { get; set; }
    [JsonProperty(PropertyName = "Show cars (un-occupied only)")]
    public bool ShowCars { get; set; }
    [JsonProperty(PropertyName = "Show tanks")]
    public bool ShowTanks { get; set; }
}

public class MapOptions
{
    [JsonProperty(PropertyName = "Enable AFK tracking")]
    public bool EnableAFKTracking { get; set; }
    [JsonProperty(PropertyName = "Hide event players")]
    public bool HideEventPlayers { get; set; }
    [JsonProperty(PropertyName = "Open map on for player's when they connect")]
    public bool StartOpen { get; set; }
    [JsonProperty(PropertyName = "Show map compass")]
    public bool ShowCompass { get; set; }
    [JsonProperty(PropertyName = "Map image options")]
    public MapImages MapImage { get; set; }
    [JsonProperty(PropertyName = "Map update time (seconds)")]
    public float UpdateSpeed { get; set; }
    public class MapImages
    {
        [JsonProperty(PropertyName = "Beancan.io API key (if applicable)")]
        public string APIKey { get; set; }
        [JsonProperty(PropertyName = "Use custom map")]
        public bool CustomMap_Use { get; set; }
        [JsonProperty(PropertyName = "Custom map filename")]
        public string CustomMap_Filename { get; set; }
    }

}

public class MapImages
{
    [JsonProperty(PropertyName = "Beancan.io API key (if applicable)")]
    public string APIKey { get; set; }
    [JsonProperty(PropertyName = "Use custom map")]
    public bool CustomMap_Use { get; set; }
    [JsonProperty(PropertyName = "Custom map filename")]
    public string CustomMap_Filename { get; set; }
}

public class Minimap
{
    [JsonProperty(PropertyName = "Enable the minimap")]
    public bool UseMinimap { get; set; }
    [JsonProperty(PropertyName = "Minimap horizontal scale")]
    public float HorizontalScale { get; set; }
    [JsonProperty(PropertyName = "Minimap vertical scale")]
    public float VerticalScale { get; set; }
    [JsonProperty(PropertyName = "Minimap docked on the left side of the screen")]
    public bool OnLeftSide { get; set; }
    [JsonProperty(PropertyName = "Minimap offset from side of the screen")]
    public float OffsetSide { get; set; }
    [JsonProperty(PropertyName = "Minimap offset from top of the screen")]
    public float OffsetTop { get; set; }
}

public class ComplexMap
{
    [JsonProperty(PropertyName = "Enable the complex map")]
    public bool UseComplexMap { get; set; }
    [JsonProperty(PropertyName = "Force complex zoom mode")]
    public bool ForceMapZoom { get; set; }
    [JsonProperty(PropertyName = "Forced zoom number (1, 2 or 3)")]
    public int ForcedZoomLevel { get; set; }
}

public class SpamOptions
{
    [JsonProperty(PropertyName = "Allowed time between map changes")]
    public int TimeBetweenAttempts { get; set; }
    [JsonProperty(PropertyName = "Attempts before warning the user they are spamming")]
    public int WarningAttempts { get; set; }
    [JsonProperty(PropertyName = "Attempts before disabling the users map")]
    public int DisableAttempts { get; set; }
    [JsonProperty(PropertyName = "Amount of time a users map will be disabled")]
    public int DisableSeconds { get; set; }
    [JsonProperty(PropertyName = "Enable spam monitoring")]
    public bool Enabled { get; set; }
}

 class MarkerData
{
    public Dictionary<string, MapMarker> data;
}

 class MapSplitter
{
    public bool SplitMap(string imageId, int amount);
    public System.Drawing.Image ImageFromStorage(uint imageId);
    public byte[] ResizeImage(Image image, int pixels);
}


```

---

## MagazinBoost by  - Can change magazines, ammo and condition for most projectile weapons

```csharp
using System;
using System.Collections.Generic;
using System.Collections;
using System.Linq;
using System.Reflection;

Oxide.Plugins
[Info("MagazinBoost", "Fujikura", "1.6.2", ResourceId = 1962)]
[Description("Can change magazines, ammo and condition for most projectile weapons")]
public class MagazinBoost : RustPlugin
{
     bool Changed;
     Dictionary<string, object> weaponContainer;
     Dictionary<string, string> guidToPath;
     string permissionAll;
     string permissionMaxAmmo;
     string permissionPreLoad;
     string permissionMaxCondition;
     string permissionAmmoType;
     bool checkPermission;
     bool removeSkinIfNoRights;
     object GetConfig(string menu, string datavalue, object defaultValue);
     void LoadVariables();
    protected override void LoadDefaultConfig();
     void GetWeapons();
     bool hasAnyRight(BasePlayer player);
     bool hasRight(BasePlayer player, string perm);
     void OnServerInitialized();
     void OnItemCraftFinished(ItemCraftTask task, Item item);
    private void OnItemAddedToContainer(ItemContainer container, Item item);
    [ConsoleCommand("mb.giveplayer")]
     void BoostGive(ConsoleSystem.Arg arg);
}


```

---

## MagicAirdropPanel by MJSU - Displays if the airdrop event is active

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
[Info("Magic Airdrop Panel", "MJSU", "1.0.2")]
[Description("Displays if the airdrop event is active")]
public class MagicAirdropPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private readonly Plugin PlaneCrash;
    private readonly Plugin Airstrike;
    private PluginConfig _pluginConfig;
    private List<CargoPlane> _activeAirdrops;
    private bool _init;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void CheckEvent();
    private void UnsubscribeAll();
    private void OnEntitySpawned(CargoPlane plane);
    private void OnEntityKill(CargoPlane plane);
    private Hash<string, object> GetPanel();
    private bool CanShowPanel(CargoPlane plane);
    private bool IsCrashPlane(CargoPlane plane);
    private bool IsStrikePlane(CargoPlane plane);
    private class PluginConfig
    {
        [DefaultValue("#00FF00FF")]
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
    [DefaultValue("#00FF00FF")]
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

## MagicAirStrikePanel by MJSU - Displays when the air strike event is active in Magic Panel

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Magic Air Strike Panel", "MJSU", "1.0.4")]
[Description("Displays if the air strike event is active")]
public class MagicAirStrikePanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private readonly Plugin Airstrike;
    private PluginConfig _pluginConfig;
    private List<CargoPlane> _activeStrikePlanes;
    private bool _init;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void CheckEvent();
    private void UnsubscribeAll();
    private void OnEntitySpawned(CargoPlane plane);
    private void OnEntityKill(CargoPlane plane);
    private Hash<string, object> GetPanel();
    private bool CanShowPanel(CargoPlane plane);
    private bool IsStrikePlane(CargoPlane plane);
    private class PluginConfig
    {
        [DefaultValue("#00FF00FF")]
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
    [DefaultValue("#00FF00FF")]
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

## MagicArea by  - PvP/PvE areas and developer API

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("MagicArea", "Norn", "0.2.1", ResourceId = 1551)]
[Description("Areas to practice building/pvp.")]
public class MagicArea : RustPlugin
{
    [PluginReference]
     Plugin Kits;
     Plugin MagicTeleportation;
     class MA
    {
        public Dictionary<int, AreaInfo> Areas;
        public Dictionary<ulong, PlayerData> PlayerData;
        public Dictionary<uint, AreaEntities> Entities;
        public MA();
    }

    public class AreaEntities
    {
        public uint iID;
        public int iAreaID;
        public ulong uCreatorID;
        public int iCreated;
        public int iExpire;
        public AreaEntities();
    }

     class PlayerData
    {
        public ulong uUserID;
        public int iInArea;
        public Int32 iInitStamp;
        public PlayerData();
    }

     class SpawnInfo
    {
        public float fX;
        public float fY;
        public float fZ;
        public int iEID;
        public SpawnInfo();
    }

     class AreaInfo
    {
        public int iID;
        public string tTitle;
        public string tDescription;
        public float fMinX;
        public float fMinY;
        public float fMinZ;
        public float fRadius;
        public bool uEnabled;
        public bool bGod;
        public bool bResetInv;
        public int iCount;
        public string tKit;
        public bool bCanResearch;
        public bool bRemoveEntities;
        public int iEntityExpire;
        public Dictionary<int, SpawnInfo> Spawns;
        public AreaInfo();
    }

     MA MAData;
    private void LoadDefaultMessages();
     string GetMessage(string key, string steamId);
     Timer AreaSync;
    private void OnServerInitialized();
    private void AreaTimer();
    private void OnPlayerExitMagicArea(BasePlayer player, int area);
    private void OnPlayerEnterMagicArea(BasePlayer player, int area);
    private int CreateMagicArea(Vector3 position, float radius, string title, string description, bool enabled);
    private bool MagicAreaExists(int id);
    public static DateTime UnixTimeStampToDateTime(double unixTimeStamp);
    private void OnEntityBuilt(Planner planner, GameObject gameObject);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private void OnEntitySpawned(BaseNetworkable entity);
    private object OnItemResearch(Item item, BasePlayer player);
    [ConsoleCommand("area.create")]
    private void ccmdCreateArea(ConsoleSystem.Arg arg);
    private object OnPlayerRespawned(BasePlayer player);
    private bool PlayerToPoint(BasePlayer player, float radi, float x, float y, float z);
    private void Loaded();
    private void Unload();
    private void SaveData();
    private bool PlayerExists(BasePlayer player);
    private bool InitPlayer(BasePlayer player);
    private bool IsPlayerInArea(BasePlayer player, float MinX, float MinY, float MaxX, float MaxY);
    protected override void LoadDefaultConfig();
    private HitInfo OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     object OnStructureUpgrade(BuildingBlock block, BasePlayer player, BuildingGrade.Enum grade);
    public static Int32 UnixTimeStampUTC();
    public static int GetRandomNumber(int min, int max);
}

 class MA
{
    public Dictionary<int, AreaInfo> Areas;
    public Dictionary<ulong, PlayerData> PlayerData;
    public Dictionary<uint, AreaEntities> Entities;
    public MA();
}

public class AreaEntities
{
    public uint iID;
    public int iAreaID;
    public ulong uCreatorID;
    public int iCreated;
    public int iExpire;
    public AreaEntities();
}

 class PlayerData
{
    public ulong uUserID;
    public int iInArea;
    public Int32 iInitStamp;
    public PlayerData();
}

 class SpawnInfo
{
    public float fX;
    public float fY;
    public float fZ;
    public int iEID;
    public SpawnInfo();
}

 class AreaInfo
{
    public int iID;
    public string tTitle;
    public string tDescription;
    public float fMinX;
    public float fMinY;
    public float fMinZ;
    public float fRadius;
    public bool uEnabled;
    public bool bGod;
    public bool bResetInv;
    public int iCount;
    public string tKit;
    public bool bCanResearch;
    public bool bRemoveEntities;
    public int iEntityExpire;
    public Dictionary<int, SpawnInfo> Spawns;
    public AreaInfo();
}


```

---

## MagicBradleyPanel by MJSU - Displays when the Bradley event is active in Magic Panel

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
[Info("Magic Bradley Panel", "MJSU", "1.0.2")]
[Description("Displays if the bradley event is active")]
public class MagicBradleyPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private List<BradleyAPC> _activeBradlys;
    private bool _init;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void CheckEvent();
    private void UnsubscribeAll();
    private void OnEntitySpawned(BradleyAPC bradley);
    private void OnEntityKill(BradleyAPC bradley);
    private Hash<string, object> GetPanel();
    private bool CanShowPanel(BradleyAPC bradley);
    private class PluginConfig
    {
        [DefaultValue("#00FF00FF")]
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
    [DefaultValue("#00FF00FF")]
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

## MagicCargoShipPanel by MJSU - Displays when the cargo ship event is active in Magic Panel

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
[Info("Magic Cargo Ship Panel", "MJSU", "1.0.2")]
[Description("Displays if the cargo ship event is active")]
public class MagicCargoShipPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private List<CargoShip> _activeCargoShips;
    private bool _init;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void CheckEvent();
    private void UnsubscribeAll();
    private void OnEntitySpawned(CargoShip ship);
    private void OnEntityKill(CargoShip ship);
    private Hash<string, object> GetPanel();
    private bool CanShowPanel(CargoShip cargo);
    private class PluginConfig
    {
        [DefaultValue("#DE8732FF")]
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
    [DefaultValue("#DE8732FF")]
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

## MagicCarpet by  - Take a magic carpet ride

```csharp
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Magic Carpet", "redBDGR", "1.0.1")]
[Description("Take a magic carpet ride")]
 class MagicCarpet : RustPlugin
{
    private const string permissionName;
    public static LayerMask collLayers;
    private Dictionary<string, BaseEntity> users;
    private void Init();
    private void Unload();
    private void OnEntityMounted(BaseMountable mountable, BasePlayer player);
    private void OnEntityDismounted(BaseMountable mountable, BasePlayer player);
    [ChatCommand("mc")]
     void attachCMD(BasePlayer player, string command, string[] args);
    private class _MagicCarpet : MonoBehaviour
    {
        public BasePlayer player;
        private BaseEntity ent;
        private BaseEntity chair;
        private BaseMountable mountable;
        private void Awake();
        private void Update();
        private Vector3 GetMovementPosition();
        public void Destroy();
    }

    private string msg(string key, string id);
}

private class _MagicCarpet : MonoBehaviour
{
    public BasePlayer player;
    private BaseEntity ent;
    private BaseEntity chair;
    private BaseMountable mountable;
    private void Awake();
    private void Update();
    private Vector3 GetMovementPosition();
    public void Destroy();
}


```

---

## MagicCashSystemPanel by MJSU - Displays players cash system currencies in Magic Panel

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.ComponentModel;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Magic Cash System Panel", "MJSU", "1.0.3")]
[Description("Displays players cash system currencies in MagicPanel")]
public class MagicCashSystemPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private readonly Plugin CashSystem;
    private PluginConfig _pluginConfig;
    private string _textFormat;
    private int _currencyIndex;
    private List<string> _currencies;
    private Coroutine _updateRoutine;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void Unload();
    private void UpdatePlayerCash();
    private IEnumerator HandleUpdatePlayerCash();
    private Hash<string, object> GetPanel(BasePlayer player);
    private double GetPlayerCash(ulong userId, string currency);
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

## MagicCh47Panel by MJSU - Displays when the CH47 event is active in Magic Panel

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
[Info("Magic Ch47 Panel", "MJSU", "1.0.2")]
[Description("Displays is the Ch47 event is active")]
public class MagicCh47Panel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private List<CH47Helicopter> _activeCh47;
    private bool _init;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void CheckEvent();
    private void UnsubscribeAll();
    private void OnEntitySpawned(CH47Helicopter heli);
    private void OnEntityKill(CH47Helicopter heli);
    private Hash<string, object> GetPanel();
    private bool CanShowPanel(CH47Helicopter heli);
    private class PluginConfig
    {
        [DefaultValue("#00FF00FF")]
        [JsonProperty(PropertyName = "Active Color")]
        public string ActiveColor { get; set; }
        [DefaultValue("#ffffff1A")]
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
    [DefaultValue("#00FF00FF")]
    [JsonProperty(PropertyName = "Active Color")]
    public string ActiveColor { get; set; }
    [DefaultValue("#ffffff1A")]
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

## MagicChat by  - Custom chat system for public and local chat

```csharp
using System;
using System.Collections.Generic;
using Rust;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Magic Chat", "Norn", "0.1.3")]
[Description("An alternative chat system")]
public class MagicChat : RustPlugin
{
     string DEFAULT_COLOR;
    [PluginReference]
     Plugin PopupNotifications;
     class StoredData
    {
        public Dictionary<ulong, UserInfo> Users;
        public StoredData();
    }

     class UserInfo
    {
        public ulong iUserId;
        public string tLastName;
        public int iWorld;
        public bool uVoiceMuted;
        public bool uPublicMuted;
        public bool uLocalMuted;
        public bool uCanColor;
        public string tColor;
        public bool uColorEnabled;
        public bool uCanCustomTag;
        public bool uCustomTagEnabled;
        public string tCustomTag;
        public int iMessagesSent;
        public int iInitTimestamp;
        public bool uShowIcon;
        public bool uIconStatus;
        public int iLastSeenTimestamp;
        public bool uShowPublicChat;
        public UserInfo();
    }

     StoredData MCData;
    private void OnServerInitialized();
     void Unload();
     string GetUserTag(BasePlayer player);
     bool UserUpdateColor(BasePlayer player, string color);
     bool UserUpdateTag(BasePlayer player, string tag);
    [ChatCommand("chat")]
    private void ChatCommand(BasePlayer player, string command, string[] args);
     void OnPlayerConnected(BasePlayer player);
    public static Int32 UnixTimeStampUTC();
    private object OnPlayerChat(ConsoleSystem.Arg arg);
    private void UserTextPublic(BasePlayer player, string text);
    private string UserNameColor(BasePlayer i);
    private void UserTextRadius(BasePlayer player, double radius, string text, string name_color);
     void OnPlayerDisconnected(BasePlayer player, string reason);
    private bool InitUserData(BasePlayer player, bool debug);
    private bool CanUserToggleSteamIcon(BasePlayer player);
    private bool CanUserCustomColor(BasePlayer player);
    private bool UserColorToggle(BasePlayer player);
    private bool UserTagToggle(BasePlayer player);
    private bool UserTagEnabled(BasePlayer player);
    private bool CanUserCustomTag(BasePlayer player);
    private bool UserDataExists(BasePlayer player);
     void SaveData();
    protected override void LoadDefaultConfig();
}

 class StoredData
{
    public Dictionary<ulong, UserInfo> Users;
    public StoredData();
}

 class UserInfo
{
    public ulong iUserId;
    public string tLastName;
    public int iWorld;
    public bool uVoiceMuted;
    public bool uPublicMuted;
    public bool uLocalMuted;
    public bool uCanColor;
    public string tColor;
    public bool uColorEnabled;
    public bool uCanCustomTag;
    public bool uCustomTagEnabled;
    public string tCustomTag;
    public int iMessagesSent;
    public int iInitTimestamp;
    public bool uShowIcon;
    public bool uIconStatus;
    public int iLastSeenTimestamp;
    public bool uShowPublicChat;
    public UserInfo();
}


```

---

## MagicClockPanel by MJSU - Displays the in-game time in Magic Panel

```csharp
using System;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Magic Clock Panel", "MJSU", "1.0.3")]
[Description("Displays the in game time in magic panel")]
public class MagicClockPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private TOD_Sky _sky;
    private TOD_Time _time;
    private string _textFormat;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void UpdateTime();
    private void Unload();
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

## MagicCoordinatesPanel by MJSU - Displays players coordinates in Magic Panel

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
[Info("Magic Coordinates Panel", "MJSU", "1.0.4")]
[Description("Displays players coordinates in magic panel")]
public class MagicCoordinatesPanel : RustPlugin
{
    [PluginReference]
    private Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private string _coordText;
    private readonly Hash<ulong, Vector3> _playerPositions;
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
    private void UpdatePlayerCoords();
    private IEnumerator HandleUpdatePlayerCoords();
    private Hash<string, object> GetPanel(BasePlayer player);
    private class PluginConfig
    {
        [DefaultValue(5f)]
        [JsonProperty(PropertyName = "Update Rate (Seconds)")]
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
    [JsonProperty(PropertyName = "Update Rate (Seconds)")]
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

## MagicCraft by Mevent - Alternative crafting system

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[Info("Magic Craft", "Orange", "1.0.0")]
[Description("Alternative crafting system")]
public class MagicCraft : RustPlugin
{
    private const string permUse;
    private void Init();
    private object OnItemCraft(ItemCraftTask item);
    private object OnCraft(ItemCraftTask task);
    private void GiveItem(BasePlayer player, ItemCraftTask task, ItemDefinition def, List<int> stacks, int taskSkinID);
    private int FreeSlots(BasePlayer player);
    private void GiveRefund(BasePlayer player, List<Item> items);
    private List<int> GetStacks(ItemDefinition item, int amount);
    private bool IsNormalItem(string name);
    private bool IsBlocked(string name);
    private bool HasPlace(int slots, List<int> stacks);
    protected override void LoadDefaultMessages();
    private void Message(BasePlayer player, string messageKey, object[] args);
    private string GetMessage(string messageKey, string playerID, object[] args);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Check for free place")]
        public bool checkPlace;
        [JsonProperty(PropertyName = "Normal Speed")]
        public List<string> normal;
        [JsonProperty(PropertyName = "Blacklist")]
        public List<string> blocked;
        [JsonProperty(PropertyName = "Split crafted stacks")]
        public bool split;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Check for free place")]
    public bool checkPlace;
    [JsonProperty(PropertyName = "Normal Speed")]
    public List<string> normal;
    [JsonProperty(PropertyName = "Blacklist")]
    public List<string> blocked;
    [JsonProperty(PropertyName = "Split crafted stacks")]
    public bool split;
}


```

---

## MagicCrashPanel by MJSU - Displays if the plane crash event is active

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
[Info("Magic Crash Panel", "MJSU", "1.0.4")]
[Description("Displays if the plane crash event is active")]
public class MagicCrashPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private readonly Plugin PlaneCrash;
    private PluginConfig _pluginConfig;
    private List<BaseEntity> _activeCrash;
    private bool _init;
    private const string CargoPlanePrefab;
    private uint _prefabId;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void CheckEvent();
    private void UnsubscribeAll();
    private void OnEntitySpawned(BaseEntity plane);
    private void OnEntityKill(BaseEntity plane);
    private Hash<string, object> GetPanel();
    private bool CanShowPanel(BaseEntity plane);
    private bool IsCrashPlane(BaseEntity plane);
    private class PluginConfig
    {
        [DefaultValue("#00FF00FF")]
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
    [DefaultValue("#00FF00FF")]
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

## MagicDangerousTreasuresPanel by MJSU - Displays if the dangerous treasures event is active

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Component = UnityEngine.Component;

Oxide.Plugins
[Info("Magic Dangerous Treasures Panel", "MJSU", "1.0.4")]
[Description("Displays if the dangerous treasures event is active")]
public class MagicDangerousTreasuresPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private readonly Plugin DangerousTreasures;
    private PluginConfig _pluginConfig;
    private readonly List<BoxStorage> _activeTreasure;
    private bool _init;
    private const string ComponentName;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void CheckEvent();
    private void UnsubscribeAll();
    private void OnEntitySpawned(BoxStorage box);
    private void OnEntityKill(BoxStorage box);
    private Hash<string, object> GetPanel();
    private bool CanShowPanel(BoxStorage box);
    private bool? MagicPanelCanShow(string name, BoxStorage box);
    private bool HasComponent(BaseEntity entity, string name);
    private class PluginConfig
    {
        [DefaultValue("#EECD60FF")]
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
    [DefaultValue("#EECD60FF")]
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

## MagicDeathNotesPanel by MJSU - Displays death notes in Magic Panel

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Magic Death Notes Panel", "MJSU", "1.0.2")]
[Description("Displays death notes in MagicPanel")]
public class MagicDeathNotesPanel : RustPlugin
{
    [PluginReference]
    private Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private string _deathText;
    private Timer _updateTimer;
    private readonly List<string> _displayText;
    private readonly List<DateTime> _expireTime;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void OnDeathNotice(Dictionary<string, object> data, string message);
    private void UpdateDisplay();
    private DateTime GetExpireTime();
    private Hash<string, object> GetPanel();
    private class PluginConfig
    {
        [DefaultValue(5f)]
        [JsonProperty(PropertyName = "Display Duration (Seconds)")]
        public float DisplayDuration { get; set; }
        [DefaultValue(1)]
        [JsonProperty(PropertyName = "Max Entries To Show")]
        public int MaxDisplayEntries { get; set; }
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
    [JsonProperty(PropertyName = "Display Duration (Seconds)")]
    public float DisplayDuration { get; set; }
    [DefaultValue(1)]
    [JsonProperty(PropertyName = "Max Entries To Show")]
    public int MaxDisplayEntries { get; set; }
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

## MagicDescription by MrBlue - Adds dynamic information in the server description

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;

Oxide.Plugins
[Info("Magic Description", "Wulf/lukespragg", "1.4.0")]
[Description("Adds dynamic information in the server description")]
public class MagicDescription : RustPlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Server description")]
        public string Description { get; set; }
        [JsonProperty(PropertyName = "Update interval (seconds)")]
        public int UpdateInterval { get; set; }
        [JsonProperty(PropertyName = "Show loaded plugins (true/false)")]
        public bool ShowPlugins { get; set; }
        [JsonProperty(PropertyName = "Hidden plugins (filename or title)")]
        public List<string> HiddenPlugins { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private static readonly Regex varRegex;
    private static bool serverInitialized;
    private void OnServerInitialized();
    private void OnServerSave();
    private string UpdateDescription(string text);
    private void OnPluginLoaded(Plugin plugin);
    private void OnPluginUnloaded(Plugin plugin);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("magic.description")]
    private void DescriptionCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("magic.version")]
    private void VersionCommand(ConsoleSystem.Arg arg);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Server description")]
    public string Description { get; set; }
    [JsonProperty(PropertyName = "Update interval (seconds)")]
    public int UpdateInterval { get; set; }
    [JsonProperty(PropertyName = "Show loaded plugins (true/false)")]
    public bool ShowPlugins { get; set; }
    [JsonProperty(PropertyName = "Hidden plugins (filename or title)")]
    public List<string> HiddenPlugins { get; set; }
}


```

---

## MagicDirectionPanel by MJSU - Displays players direction in magic panel

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.ComponentModel;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Magic Direction Panel", "MJSU", "1.0.1")]
[Description("Displays players direction in magic panel")]
public class MagicDirectionPanel : RustPlugin
{
    [PluginReference]
    private Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private string _directionText;
    private readonly Hash<ulong, string> _playerDirection;
    private Coroutine _updateRoutine;
    private void Init();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void Unload();
    private void UnsubscribeAll();
    private void UpdatePlayerCoords();
    private IEnumerator HandleUpdatePlayerCoords();
    private string GetDirection(BasePlayer player);
    private Hash<string, object> GetPanel(BasePlayer player);
    private string Lang(string key, BasePlayer player, object[] args);
    private class PluginConfig
    {
        [DefaultValue(5f)]
        [JsonProperty(PropertyName = "Update Rate (Seconds)")]
        public float UpdateRate { get; set; }
        [DefaultValue(false)]
        [JsonProperty(PropertyName = "Show Angle")]
        public bool ShowAngle { get; set; }
        [JsonProperty(PropertyName = "Panel Settings")]
        public PanelRegistration PanelSettings { get; set; }
        [JsonProperty(PropertyName = "Panel Layout")]
        public Panel Panel { get; set; }
    }

    private class LangKeys
    {
        public const string North;
        public const string Northeast;
        public const string East;
        public const string Southeast;
        public const string South;
        public const string Southwest;
        public const string West;
        public const string Northwest;
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
    [JsonProperty(PropertyName = "Update Rate (Seconds)")]
    public float UpdateRate { get; set; }
    [DefaultValue(false)]
    [JsonProperty(PropertyName = "Show Angle")]
    public bool ShowAngle { get; set; }
    [JsonProperty(PropertyName = "Panel Settings")]
    public PanelRegistration PanelSettings { get; set; }
    [JsonProperty(PropertyName = "Panel Layout")]
    public Panel Panel { get; set; }
}

private class LangKeys
{
    public const string North;
    public const string Northeast;
    public const string East;
    public const string Southeast;
    public const string South;
    public const string Southwest;
    public const string West;
    public const string Northwest;
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

## MagicEarthquakePanel by MJSU - Displays if the earthquake event is active

```csharp
using System;
using System.ComponentModel;
using Newtonsoft.Json;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Magic Earthquake Panel", "MJSU", "1.0.1")]
[Description("Displays if the earthquake event is active")]
public class MagicEarthquakePanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private readonly Plugin Earthquake;
    private PluginConfig _pluginConfig;
    private int _activeEarthQuakes;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void CheckEvent();
    private void UnsubscribeAll();
    private void OnEarthquakeStarted();
    private void OnEarthquakeFinished();
    private Hash<string, object> GetPanel();
    private class PluginConfig
    {
        [DefaultValue("#00FF00FF")]
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
    [DefaultValue("#00FF00FF")]
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

## MagicEasterPanel by MJSU - Displays when the easter event is active in Magic Panel

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
[Info("Magic Easter Panel", "MJSU", "1.0.3")]
[Description("Displays if the easter event is active")]
public class MagicEasterPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private List<EggHuntEvent> _eggEvents;
    private bool _init;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void CheckEvent();
    private void UnsubscribeAll();
    private void OnEntitySpawned(EggHuntEvent egg);
    private void OnEntityKill(EggHuntEvent egg);
    private Hash<string, object> GetPanel();
    private bool CanShowPanel(EggHuntEvent egg);
    private class PluginConfig
    {
        [DefaultValue("#00FF00FF")]
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
    [DefaultValue("#00FF00FF")]
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

## MagicEconomicsPanel by MJSU - Displays player economics data in Magic Panel

```csharp
using System;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

[Info("Magic Economics Panel", "MJSU", "1.0.8")]
[Description("Displays player economics data in MagicPanel")]
public class MagicEconomicsPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private readonly Plugin Economics;
    private PluginConfig _pluginConfig;
    private string _textFormat;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void OnEconomicsBalanceUpdated(string playerId, double amount);
    private Hash<string, object> GetPanel(BasePlayer player);
    public double GetBalance(ulong userId);
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

## MagicFpsPanel by MJSU - Displays the current server FPS in Magic Panel

```csharp
using System;
using System.ComponentModel;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Magic Fps Panel", "MJSU", "1.0.2")]
[Description("Displays the server fps in magic panel")]
public class MagicFpsPanel : RustPlugin
{
    [PluginReference]
    private Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private const string ViewPermission;
    private string _fpsText;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private Hash<string, object> GetPanel();
    private class PluginConfig
    {
        [DefaultValue(5f)]
        [JsonProperty(PropertyName = "Update Rate (Seconds)")]
        public float UpdateRate { get; set; }
        [DefaultValue(false)]
        [JsonProperty(PropertyName = "Requires permission to view")]
        public bool UsePermission { get; set; }
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
    [JsonProperty(PropertyName = "Update Rate (Seconds)")]
    public float UpdateRate { get; set; }
    [DefaultValue(false)]
    [JsonProperty(PropertyName = "Requires permission to view")]
    public bool UsePermission { get; set; }
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

## MagicGatherPanel by MJSU - Displays gather rate in Magic Panel

```csharp
using System;
using System.ComponentModel;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Magic Gather Panel", "MJSU", "1.0.2")]
[Description("Displays gather rate in magic panel")]
public class MagicGatherPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private string _gatherFormat;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void OnGlobalGatherUpdated();
    private void OnPlayerGatherUpdated(BasePlayer player);
    private Hash<string, object> GetPanel(BasePlayer player);
    private class PluginConfig
    {
        [DefaultValue(1f)]
        [JsonProperty(PropertyName = "Default Gather")]
        public float DefaultGather { get; set; }
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
    [DefaultValue(1f)]
    [JsonProperty(PropertyName = "Default Gather")]
    public float DefaultGather { get; set; }
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

## MagicGridPanel by MJSU - Displays players grid position in magic panel

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

[Info("Magic Grid Panel", "MJSU", "1.0.8")]
[Description("Displays players grid position in magic panel")]
public class MagicGridPanel : RustPlugin
{
    [PluginReference]
    private Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private string _gridText;
    private readonly Hash<ulong, Vector2i> _playersGrid;
    private readonly Hash<Vector2i, string> _gridToString;
    private Coroutine _updateRoutine;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void Unload();
    private void UnsubscribeAll();
    private void UpdatePlayerCoords();
    private IEnumerator HandleUpdatePlayerCoords();
    private Hash<string, object> GetPanel(BasePlayer player);
    public string GetGrid(Vector2i grid);
    private class PluginConfig
    {
        [DefaultValue(5f)]
        [JsonProperty(PropertyName = "Update Rate (Seconds)")]
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
    [JsonProperty(PropertyName = "Update Rate (Seconds)")]
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

## MagicHalloweenPanel by MJSU - Displays when the Halloween event is active in Magic Panel

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
[Info("Magic Halloween Panel", "MJSU", "1.0.3")]
[Description("Displays if the halloween event is active")]
public class MagicHalloweenPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private List<HalloweenHunt> _halloweenEvents;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    public void CheckEvent();
    public void SubscribeAll();
    public void UnsubscribeAll();
    private void OnEntitySpawned(HalloweenHunt hunt);
    private void OnEntityKill(HalloweenHunt hunt);
    private Hash<string, object> GetPanel();
    private bool CanShowPanel(HalloweenHunt hunt);
    private class PluginConfig
    {
        [DefaultValue("#EB6123FF")]
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
    [DefaultValue("#EB6123FF")]
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

## MagicHeliPanel by MJSU - Displays when the patrol helicopter event is active in Magic Panel

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
[Info("Magic Heli Panel", "MJSU", "1.0.4")]
[Description("Displays if the patrol helicopter event is active")]
public class MagicHeliPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private List<PatrolHelicopter> _activeHelis;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void CheckEvent();
    private void SubscribeAll();
    private void UnsubscribeAll();
    private void OnEntitySpawned(PatrolHelicopter heli);
    private void OnEntityKill(PatrolHelicopter heli);
    private Hash<string, object> GetPanel();
    private bool CanShowPanel(PatrolHelicopter heli);
    private class PluginConfig
    {
        [DefaultValue("#B33333FF")]
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
    [DefaultValue("#B33333FF")]
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

## MagicHostilePanel by MJSU - Displays how much longer a player is considered hostile

```csharp
using System;
using System.ComponentModel;
using Network;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Magic Hostile Panel", "MJSU", "1.0.10")]
[Description("Displays how much longer a player is considered hostile")]
public class MagicHostilePanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private string _panelText;
    private readonly Hash<ulong, Timer> _hostileTimer;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void OnPlayerConnected(BasePlayer player);
    private void OnEntityMarkHostile(BasePlayer player);
    private void SetupHostile(BasePlayer player);
    private Hash<string, object> GetPanel(BasePlayer player);
    private void HidePanel(BasePlayer player);
    private void ShowPanel(BasePlayer player);
    private void UpdatePanel(BasePlayer player);
    private class PluginConfig
    {
        [DefaultValue(false)]
        [JsonProperty(PropertyName = "Show/Hide panel")]
        public bool ShowHide { get; set; }
        [DefaultValue(false)]
        [JsonProperty(PropertyName = "Change Icon Color")]
        public bool ChangeIconColor { get; set; }
        [DefaultValue("#4EE44EFF")]
        [JsonProperty(PropertyName = "Active Color")]
        public string ActiveColor { get; set; }
        [DefaultValue("#B33333FF")]
        [JsonProperty(PropertyName = "Inactive Color")]
        public string InactiveColor { get; set; }
        [DefaultValue(1.0f)]
        [JsonProperty(PropertyName = "Update Rate (Seconds)")]
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
    [DefaultValue(false)]
    [JsonProperty(PropertyName = "Show/Hide panel")]
    public bool ShowHide { get; set; }
    [DefaultValue(false)]
    [JsonProperty(PropertyName = "Change Icon Color")]
    public bool ChangeIconColor { get; set; }
    [DefaultValue("#4EE44EFF")]
    [JsonProperty(PropertyName = "Active Color")]
    public string ActiveColor { get; set; }
    [DefaultValue("#B33333FF")]
    [JsonProperty(PropertyName = "Inactive Color")]
    public string InactiveColor { get; set; }
    [DefaultValue(1.0f)]
    [JsonProperty(PropertyName = "Update Rate (Seconds)")]
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

## MagicImagesPanel by MJSU - Displays custom images in Magic Panel

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using Newtonsoft.Json;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Magic Images Panel", "MJSU", "1.0.3")]
[Description("Displays images in Magic Panel")]
public class MagicImagesPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private Hash<string, object> GetPanel(string panelName);
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Image Panels")]
        public Hash<string, PanelData> Panels { get; set; }
    }

    private class PanelData
    {
        [JsonProperty(PropertyName = "Panel Settings")]
        public PanelRegistration PanelSettings { get; set; }
        [JsonProperty(PropertyName = "Panel Layout")]
        public Panel Panel { get; set; }
        [DefaultValue(true)]
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
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
    [JsonProperty(PropertyName = "Image Panels")]
    public Hash<string, PanelData> Panels { get; set; }
}

private class PanelData
{
    [JsonProperty(PropertyName = "Panel Settings")]
    public PanelRegistration PanelSettings { get; set; }
    [JsonProperty(PropertyName = "Panel Layout")]
    public Panel Panel { get; set; }
    [DefaultValue(true)]
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
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

## MagicJoiningPanel by MJSU - Displays joining player count in Magic Panel

```csharp
using System;
using System.ComponentModel;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Magic Joining Panel", "MJSU", "1.0.3")]
[Description("Displays joining player count in magic panel")]
public class MagicJoiningPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private string _textFormat;
    private int _joiningCount;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void UpdatePanel();
    private Hash<string, object> GetPanel();
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Panel Settings")]
        public PanelRegistration PanelSettings { get; set; }
        [JsonProperty(PropertyName = "Panel Layout")]
        public Panel Panel { get; set; }
        [DefaultValue(5f)]
        [JsonProperty(PropertyName = "Update Rates (Seconds)")]
        public float UpdateRate { get; set; }
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
    [DefaultValue(5f)]
    [JsonProperty(PropertyName = "Update Rates (Seconds)")]
    public float UpdateRate { get; set; }
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

## MagicLastWipePanel by MJSU - Displays the date the server last wiped in Magic Panel

```csharp
using System;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Magic Last Wipe Panel", "MJSU", "1.0.1")]
[Description("Displays the date the server wiped in magic panel")]
public class MagicLastWipePanel : RustPlugin
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

## MagicLoot by Malmo - Simple components multiplier and loot system

```csharp
using System;
using System.Collections.Generic;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Rust;

Oxide.Plugins
[Info("Magic Loot", "collect_vood", "1.0.5")]
[Description("Simple components multiplier and loot system")]
public class MagicLoot : CovalencePlugin
{
    private bool _initialized;
    private readonly int _maxContainerSlots;
    private int _scrapStack;
    private Dictionary<Rarity, List<string>> _sortedRarities;
    private Dictionary<int, List<ulong>> _skinsCache;
    private Dictionary<LootSpawn, ItemAmountRanged[]> _originItemAmountRange;
    private ConfigurationFile _configuration;
    public class ConfigurationFile
    {
        [JsonProperty(PropertyName = "General Settings")]
        public SettingsFile Settings;
        [JsonProperty(PropertyName = "Extra Loot")]
        public ExtraLootFile ExtraLoot;
        [JsonProperty(PropertyName = "Blacklisted Items (Item-Shortnames)",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> BlacklistedItems;
        [JsonProperty(PropertyName = "Blacklisted Workshop Skins (Workshop Ids)",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ulong> BlacklistSkins;
        [JsonProperty(PropertyName = "Manual Item Multipliers (Key: Item-Shortname, Value: Multiplier)",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, float> ManualItemMultipliers;
        [JsonProperty(PropertyName = "Containers Data (Key: Container-Shortname, Value: Container Settings)",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, ContainerData> ContainersData;
        [JsonProperty(PropertyName = "Debug")]
        public bool Debug;
    }

    public class SettingsFile
    {
        [JsonProperty(PropertyName = "General Item List Multiplier (All items in the 'Manual Item Multipliers' List)")]
        public float ItemListMultiplier;
        [JsonProperty(PropertyName = "Non Item List Multiplier (All items not listed in the 'Manual Item Multipliers' List)")]
        public float NonItemListMultiplier;
        [JsonProperty(PropertyName = "Limit Multipliers to Stacksizes")]
        public bool LimitToStacksizes;
        [JsonProperty(PropertyName = "Multiply Blueprints")]
        public bool BlueprintDuplication;
        [JsonProperty(PropertyName = "Disable Blueprint Drops")]
        public bool DisableBlueprints;
        [JsonProperty(PropertyName = "Random Workshop Skins")]
        public bool RandomWorkshopSkins;
        [JsonProperty(PropertyName = "Multiply Tea Buffs")]
        public bool MultiplyTeaBuff;
        [JsonProperty(PropertyName = "Force Custom Loot Tables for Default Loot on all Containers")]
        public bool ForceCustomLootTables;
    }

    public class ExtraLootFile
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled;
        [JsonProperty(PropertyName = "Extra Items Min")]
        public int ExtraItemsMin;
        [JsonProperty(PropertyName = "Extra Items Max")]
        public int ExtraItemsMax;
        [JsonProperty(PropertyName = "Prevent Duplicates")]
        public bool PreventDuplicates;
        [JsonProperty(PropertyName = "Prevent Duplicates Retries")]
        public int PreventDuplicatesRetries;
        [JsonProperty(PropertyName = "Force Custom Loot Tables for Extra Loot on all Containers")]
        public bool ForceCustomLootTables;
    }

    public class ContainerData
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled;
        [JsonProperty(PropertyName = "Extra Items Min")]
        public int ExtraItemsMin;
        [JsonProperty(PropertyName = "Extra Items Max")]
        public int ExtraItemsMax;
        [JsonProperty(PropertyName = "Loot Multiplier")]
        public float Multiplier;
        [JsonProperty(PropertyName = "Utilize Vanilla Loot Tables on Default Loot")]
        public bool VanillaLootTablesDefault;
        [JsonProperty(PropertyName = "Utilize Vanilla Loot Tables on Extra Loot")]
        public bool VanillaLootTablesExtra;
        [JsonProperty(PropertyName = "Utilize Random Rarity (depending on Items ALREADY in the container)")]
        public bool RandomRarities;
        [JsonProperty(PropertyName = "Rarity To Use (ONLY if 'Utilize Vanilla Loot Tables' is FALSE & 'Utilize Random Rarity' is FALSE | 0 = None, 1 = Common, 2 = Uncommon, 3 = Rare, 4 = Very Rare)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<Rarity> Rarities;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void LoadData();
    private void SaveData();
    private DataFile _data;
    public class DataFile
    {
        [JsonProperty(PropertyName = "Item Data (Key: Item Shortname, Value: Stacksize)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> Items;
    }

    private void OnServerInitialized();
    private object OnLootSpawn(LootContainer container);
    private void OnBonusItemDrop(Item item, BasePlayer player);
    private void Unload();
    private void AddMissingContainers();
    private void RepopulateContainers();
    private void PopulateContainer(LootContainer container, ContainerData containerData);
    private void ReinforceRules(LootContainer container, ContainerData containerData);
    private void ReinforceRules(Item item, ContainerData containerData);
    private void FindDuplicates(int itemId, List<Item> items, List<Item> duplicateItems);
    private bool IsValid(ItemDefinition itemDefintion);
    private void ModifyItemAmountRanges();
    private void ModifyItemAmountRange(LootSpawn origin);
    private void RestoreItemAmountRanges();
    private void AddDefaultLoot(LootContainer container, ContainerData containerData);
    private void AddExtraLoot(LootContainer container, ContainerData containerData);
    private void AddRandomVanillaItem(LootContainer container);
    private void AddRandomItem(LootContainer container, ContainerData containerData);
    private void RandomizeDurability(LootContainer container);
    private Dictionary<Rarity, List<string>> GetSortedRarities();
    private bool IgnoreContainer(ContainerData containerData);
    private ulong GetRandomSkin(ItemDefinition itemDef);
    private void ApplySkinToItem(Item item, ulong skinId);
    private void PrintDebug(object message);
}

public class ConfigurationFile
{
    [JsonProperty(PropertyName = "General Settings")]
    public SettingsFile Settings;
    [JsonProperty(PropertyName = "Extra Loot")]
    public ExtraLootFile ExtraLoot;
    [JsonProperty(PropertyName = "Blacklisted Items (Item-Shortnames)",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> BlacklistedItems;
    [JsonProperty(PropertyName = "Blacklisted Workshop Skins (Workshop Ids)",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ulong> BlacklistSkins;
    [JsonProperty(PropertyName = "Manual Item Multipliers (Key: Item-Shortname, Value: Multiplier)",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, float> ManualItemMultipliers;
    [JsonProperty(PropertyName = "Containers Data (Key: Container-Shortname, Value: Container Settings)",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, ContainerData> ContainersData;
    [JsonProperty(PropertyName = "Debug")]
    public bool Debug;
}

public class SettingsFile
{
    [JsonProperty(PropertyName = "General Item List Multiplier (All items in the 'Manual Item Multipliers' List)")]
    public float ItemListMultiplier;
    [JsonProperty(PropertyName = "Non Item List Multiplier (All items not listed in the 'Manual Item Multipliers' List)")]
    public float NonItemListMultiplier;
    [JsonProperty(PropertyName = "Limit Multipliers to Stacksizes")]
    public bool LimitToStacksizes;
    [JsonProperty(PropertyName = "Multiply Blueprints")]
    public bool BlueprintDuplication;
    [JsonProperty(PropertyName = "Disable Blueprint Drops")]
    public bool DisableBlueprints;
    [JsonProperty(PropertyName = "Random Workshop Skins")]
    public bool RandomWorkshopSkins;
    [JsonProperty(PropertyName = "Multiply Tea Buffs")]
    public bool MultiplyTeaBuff;
    [JsonProperty(PropertyName = "Force Custom Loot Tables for Default Loot on all Containers")]
    public bool ForceCustomLootTables;
}

public class ExtraLootFile
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled;
    [JsonProperty(PropertyName = "Extra Items Min")]
    public int ExtraItemsMin;
    [JsonProperty(PropertyName = "Extra Items Max")]
    public int ExtraItemsMax;
    [JsonProperty(PropertyName = "Prevent Duplicates")]
    public bool PreventDuplicates;
    [JsonProperty(PropertyName = "Prevent Duplicates Retries")]
    public int PreventDuplicatesRetries;
    [JsonProperty(PropertyName = "Force Custom Loot Tables for Extra Loot on all Containers")]
    public bool ForceCustomLootTables;
}

public class ContainerData
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled;
    [JsonProperty(PropertyName = "Extra Items Min")]
    public int ExtraItemsMin;
    [JsonProperty(PropertyName = "Extra Items Max")]
    public int ExtraItemsMax;
    [JsonProperty(PropertyName = "Loot Multiplier")]
    public float Multiplier;
    [JsonProperty(PropertyName = "Utilize Vanilla Loot Tables on Default Loot")]
    public bool VanillaLootTablesDefault;
    [JsonProperty(PropertyName = "Utilize Vanilla Loot Tables on Extra Loot")]
    public bool VanillaLootTablesExtra;
    [JsonProperty(PropertyName = "Utilize Random Rarity (depending on Items ALREADY in the container)")]
    public bool RandomRarities;
    [JsonProperty(PropertyName = "Rarity To Use (ONLY if 'Utilize Vanilla Loot Tables' is FALSE & 'Utilize Random Rarity' is FALSE | 0 = None, 1 = Common, 2 = Uncommon, 3 = Rare, 4 = Very Rare)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<Rarity> Rarities;
}

public class DataFile
{
    [JsonProperty(PropertyName = "Item Data (Key: Item Shortname, Value: Stacksize)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> Items;
}


```

---

## MagicMessagePanel by MJSU - Displays custom messages in Magic Panel

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Magic Message Panel", "MJSU", "1.2.1")]
[Description("Displays messages in magic panel")]
public class MagicMessagePanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private readonly Plugin PlaceholderAPI;
    private DynamicConfigFile _newConfig;
    private PluginConfig _pluginConfig;
    private readonly Hash<ulong, int> _playerIndex;
    private readonly Hash<ulong, string> _playerLastMessage;
    private Action<IPlayer, StringBuilder, bool> _replacer;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private List<string> GetPlayerMessages(BasePlayer player, string panelName);
    private bool InPermOrGroup(BasePlayer player, string permGroup);
    private Hash<string, object> GetPanel(BasePlayer player, string panelName);
    private string ParseText(string text);
    private void OnPluginUnloaded(Plugin plugin);
    private Action<IPlayer, StringBuilder, bool> GetReplacer();
    private bool IsPlaceholderApiLoaded();
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Message Panels")]
        public Hash<string, PanelData> Panels { get; set; }
    }

    private class PanelData
    {
        [JsonProperty(PropertyName = "Panel Settings")]
        public PanelRegistration PanelSettings { get; set; }
        [JsonProperty(PropertyName = "Panel Layout")]
        public Panel Panel { get; set; }
        [JsonProperty(PropertyName = "Messages to all players")]
        public List<string> Messages { get; set; }
        [JsonProperty(PropertyName = "Perm or Group messages")]
        public List<MessageGroupData> PermMessages;
        [JsonProperty(PropertyName = "Random Message Display Order")]
        public bool RandomOrder { get; set; }
        [JsonProperty(PropertyName = "Update Rate (Seconds)")]
        public float UpdateRate { get; set; }
        [DefaultValue(true)]
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        public bool ShouldSerializeMessages();
    }

    private class MessageGroupData
    {
        [JsonProperty(PropertyName = "Messages")]
        public List<string> Messages { get; set; }
        [JsonProperty(PropertyName = "Group or Permission")]
        public string GroupOrPerm { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty(PropertyName = "Player Group or Permission State (In, NotIn)")]
        public State State { get; set; }
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
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
    [JsonProperty(PropertyName = "Message Panels")]
    public Hash<string, PanelData> Panels { get; set; }
}

private class PanelData
{
    [JsonProperty(PropertyName = "Panel Settings")]
    public PanelRegistration PanelSettings { get; set; }
    [JsonProperty(PropertyName = "Panel Layout")]
    public Panel Panel { get; set; }
    [JsonProperty(PropertyName = "Messages to all players")]
    public List<string> Messages { get; set; }
    [JsonProperty(PropertyName = "Perm or Group messages")]
    public List<MessageGroupData> PermMessages;
    [JsonProperty(PropertyName = "Random Message Display Order")]
    public bool RandomOrder { get; set; }
    [JsonProperty(PropertyName = "Update Rate (Seconds)")]
    public float UpdateRate { get; set; }
    [DefaultValue(true)]
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    public bool ShouldSerializeMessages();
}

private class MessageGroupData
{
    [JsonProperty(PropertyName = "Messages")]
    public List<string> Messages { get; set; }
    [JsonProperty(PropertyName = "Group or Permission")]
    public string GroupOrPerm { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty(PropertyName = "Player Group or Permission State (In, NotIn)")]
    public State State { get; set; }
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
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

## MagicOilRigPanel by MJSU - Displays if the small of large oil rig crate is present

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

[Info("Magic Oil Rig Panel", "MJSU", "1.0.4")]
[Description("Displays if the small of large oil rig crate is present")]
public class MagicOilRigPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private string _largeName;
    private readonly List<HackableLockedCrate> _activeLargeOil;
    private Vector3 _largePos;
    private const string LargeOilPrefab;
    private string _smallName;
    private readonly List<HackableLockedCrate> _activeSmallOil;
    private Vector3 _smallPos;
    private const string SmallOilPrefab;
    private bool _init;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void CheckSmallEvent(bool force);
    private void CheckLargeEvent(bool force);
    private void UnsubscribeAll();
    private void OnEntitySpawned(HackableLockedCrate crate);
    private void OnCrateHack(HackableLockedCrate crate);
    private void OnCrateHackEnd(HackableLockedCrate crate);
    private void OnEntityKill(HackableLockedCrate crate);
    private Hash<string, object> GetPanel(string name);
    private string GetPanelColor(List<HackableLockedCrate> crates, PanelSetup setup);
    private bool IsCrateAt(HackableLockedCrate crate, Vector3 pos);
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Small Oil Rig")]
        public PanelSetup SmallOilRig { get; set; }
        [JsonProperty(PropertyName = "Large Oil Rig")]
        public PanelSetup LargeOilRig { get; set; }
    }

    private class PanelSetup
    {
        [JsonProperty(PropertyName = "No Crate Color")]
        public string InactiveColor { get; set; }
        [JsonProperty(PropertyName = "Untouched Crate Color")]
        public string ActiveColor { get; set; }
        [JsonProperty(PropertyName = "Hacking Crate Color")]
        public string HackingColor { get; set; }
        [JsonProperty(PropertyName = "Fully Hacked Crate Color")]
        public string FullyHackedColor { get; set; }
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
    [JsonProperty(PropertyName = "Small Oil Rig")]
    public PanelSetup SmallOilRig { get; set; }
    [JsonProperty(PropertyName = "Large Oil Rig")]
    public PanelSetup LargeOilRig { get; set; }
}

private class PanelSetup
{
    [JsonProperty(PropertyName = "No Crate Color")]
    public string InactiveColor { get; set; }
    [JsonProperty(PropertyName = "Untouched Crate Color")]
    public string ActiveColor { get; set; }
    [JsonProperty(PropertyName = "Hacking Crate Color")]
    public string HackingColor { get; set; }
    [JsonProperty(PropertyName = "Fully Hacked Crate Color")]
    public string FullyHackedColor { get; set; }
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

## MagicPanel by MJSU - Modular panel and API for displaying information to the players on their HUD

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
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Magic Panel", "MJSU", "1.0.8")]
[Description("Displays information to the players on their hud.")]
internal class MagicPanel : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private StoredData _storedData;
    private PluginConfig _pluginConfig;
    private readonly Hash<string, Hash<string, float>> _panelPositions;
    private readonly Hash<string, PanelRegistration> _registeredPanels;
    private readonly Hash<string, HiddenPanelInfo> _hiddenPanels;
    private readonly Hash<string, string> _imageCache;
    private const string AccentColor;
    private bool _init;
    private bool _imageLibraryEnabled;
    private void Init();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void Unload();
    private void MagicPanelChatCommand(BasePlayer player, string cmd, string[] args);
    private void DisplayHelp(BasePlayer player);
    private void OnPluginUnloaded(Plugin plugin);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void RegisterPlayerPanel(Plugin plugin, string name, string panelData, string getMethodName, string permission);
    private void RegisterGlobalPanel(Plugin plugin, string name, string panelData, string getMethodName, string permission);
    private void RegisterPanel(Plugin plugin, string name, string panelData, string getMethodName, PanelTypeEnum type, string permission);
    private void UnregisterPanel(PanelRegistration panel);
    private void UnregisterPluginPanels(Plugin plugin);
    private void RecalculatePositions(string dockName);
    private void UpdatePanel(string panelName, int update);
    private void UpdatePanel(BasePlayer player, string panelName, int update);
    private void ShowPanel(string name);
    private void ShowPanel(BasePlayer player, string name);
    private void HidePanel(string name);
    private void HidePanel(BasePlayer player, string name);
    private void DrawDock(IEnumerable<BasePlayer> players);
    private void DrawPanel(IEnumerable<BasePlayer> players, PanelRegistration registeredPanel, UpdateEnum updateEnum);
    private void DrawGlobalPanel(IEnumerable<BasePlayer> players, PanelSetup setup, UpdateEnum updateEnum);
    private void DrawPlayersPanel(IEnumerable<BasePlayer> players, PanelSetup setup, UpdateEnum updateEnum);
    private List<PanelUpdate> CreatePanel(Panel panel, PanelSetup setup, UpdateEnum update);
    private PanelUpdate CreateImage(Panel panel, string panelName, float offset);
    private PanelUpdate CreateText(Panel panel, string panelName, float offset);
    private bool IsImageLibraryReady();
    private void ImageLibraryAddImage(string image);
    private bool ImageLibraryHasImage(string image);
    private string ImageLibraryGetImage(string image);
    private string GetPanelUiName(string panelName);
    private string GetPanelUiImageName(string panelName);
    private string GetPanelUiTextName(string panelName);
    private string GetDockUiName(string dockName);
    private UiPosition GetTypePosition(float startPos, float width, TypePadding padding);
    private UiPosition GetPanelPosition(PanelSetup setup, TypePadding dockPadding);
    private UiPosition GetDockUiPosition(DockPosition pos, string dockName);
    private void SaveData();
    private void Chat(BasePlayer player, string format, object[] args);
    private string Lang(string key, BasePlayer player, object[] args);
    private class PluginConfig
    {
        [DefaultValue("mp")]
        [JsonProperty(PropertyName = "Chat Command")]
        public string ChatCommand { get; set; }
        [DefaultValue(true)]
        [JsonProperty(PropertyName = "Use Image Library")]
        public bool UseImageLibrary { get; set; }
        [DefaultValue("Under")]
        [JsonProperty(PropertyName = "Parent UI Layer (Overlay, Hud.Menu, Hud, Under)")]
        public string ParentLayer { get; set; }
        [JsonProperty(PropertyName = "Text Outline Settings")]
        public TextOutlineSettings TextOutline { get; set; }
        [JsonProperty(PropertyName = "Docks")]
        public Hash<string, DockData> Docks { get; set; }
    }

    private class TextOutlineSettings
    {
        [JsonProperty(PropertyName = "Enable Text Outline")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Text Outline Color")]
        public string Color { get; set; }
        [JsonProperty(PropertyName = "Text Outline Distance")]
        public string Distance { get; set; }
    }

    private class DockData
    {
        [JsonProperty(PropertyName = "Position")]
        public DockPosition Position { get; set; }
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Background Color")]
        public string BackgroundColor { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty(PropertyName = "Panel Alignment (Left, Center, Right)")]
        public PanelAlignEnum Alignment { get; set; }
        [JsonProperty(PropertyName = "Panel Padding")]
        public float PanelPadding { get; set; }
        [JsonProperty(PropertyName = "Dock Padding")]
        public TypePadding DockPadding { get; set; }
    }

    private class DockPosition
    {
        [JsonProperty(PropertyName = "X Position")]
        public float XPos { get; set; }
        [JsonProperty(PropertyName = "Y Start Position")]
        public float StartYPos { get; set; }
        [JsonProperty(PropertyName = "Height")]
        public float Height { get; set; }
    }

    private class StoredData
    {
        public Hash<ulong, PlayerSettings> Settings;
    }

    private static class LangKeys
    {
        public const string On;
        public const string Off;
        public const string SettingsChanged;
        public const string Chat;
        public const string Help;
    }

    private class PlayerSettings
    {
        public bool Enabled { get; set; }
    }

    private class PanelRegistration
    {
        public string Name { get; set; }
        public string Dock { get; set; }
        public float Width { get; set; }
        public int Order { get; set; }
        public string BackgroundColor { get; set; }
        public string GetPanelMethod { get; set; }
        public PanelTypeEnum PanelType { get; set; }
        public Plugin Plugin { get; set; }
        public string Permission { get; set; }
    }

    private class Panel
    {
        public PanelImage Image { get; set; }
        public PanelText Text { get; set; }
        public Panel(Hash<string, object> data);
    }

    private abstract class PanelType
    {
        public bool Enabled { get; set; }
        public string Color { get; set; }
        public int Order { get; set; }
        public float Width { get; set; }
        public TypePadding Padding { get; set; }
        protected PanelType(Hash<string, object> data);
    }

    private class PanelImage : PanelType
    {
        public string Url { get; set; }
        public PanelImage(Hash<string, object> data);
    }

    private class PanelText : PanelType
    {
        public string Text { get; set; }
        public int FontSize { get; set; }
        public TextAnchor TextAnchor { get; set; }
        public PanelText(Hash<string, object> data);
    }

    private class TypePadding
    {
        public float Left { get; set; }
        public float Right { get; set; }
        public float Top { get; set; }
        public float Bottom { get; set; }
        [JsonConstructor]
        public TypePadding();
        public TypePadding(Hash<string, object> data);
        public TypePadding(float left, float right, float top, float bottom);
    }

    private class PanelSetup
    {
        public DockPosition Pos { get; set; }
        public float StartPos { get; set; }
        public string UiParentPanel { get; set; }
        public string PanelColor { get; set; }
        public PanelRegistration PanelReg { get; set; }
    }

    private class PanelUpdate
    {
        public CuiElementContainer Container { get; set; }
        public string PanelName { get; set; }
    }

    private class HiddenPanelInfo
    {
        public List<ulong> PlayerHidden { get; set; }
        public bool All { get; set; }
        public HiddenPanelInfo();
    }

    private const string UiPanelName;
    private readonly string _clearColor;
    private readonly UiPosition _fullSize;
    private static class Ui
    {
        private static string UiPanel { get; set; }
        public static CuiElementContainer Container(string color, UiPosition pos, string panel, string parent);
        public static void Label(CuiElementContainer container, string text, int size, string color, UiPosition pos, TextAnchor align);
        public static void OutlineLabel(CuiElementContainer container, string text, int size, string color, string outlineColor, string distance, UiPosition pos, TextAnchor align);
        public static void Image(CuiElementContainer container, string png, string color, UiPosition pos);
        public static string Color(string hexColor);
    }

    private class CuiOutlineLabel : CuiLabel
    {
        public CuiOutlineComponent Outline { get; set; }
    }

    private class UiPosition
    {
        private float XPos { get; set; }
        private float YPos { get; set; }
        private float Width { get; set; }
        private float Height { get; set; }
        public UiPosition(float xPos, float yPos, float width, float height);
        public string GetMin();
        public string GetMax();
        public override string ToString();
    }

    private void DestroyAllUi(BasePlayer player);
}

private class PluginConfig
{
    [DefaultValue("mp")]
    [JsonProperty(PropertyName = "Chat Command")]
    public string ChatCommand { get; set; }
    [DefaultValue(true)]
    [JsonProperty(PropertyName = "Use Image Library")]
    public bool UseImageLibrary { get; set; }
    [DefaultValue("Under")]
    [JsonProperty(PropertyName = "Parent UI Layer (Overlay, Hud.Menu, Hud, Under)")]
    public string ParentLayer { get; set; }
    [JsonProperty(PropertyName = "Text Outline Settings")]
    public TextOutlineSettings TextOutline { get; set; }
    [JsonProperty(PropertyName = "Docks")]
    public Hash<string, DockData> Docks { get; set; }
}

private class TextOutlineSettings
{
    [JsonProperty(PropertyName = "Enable Text Outline")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Text Outline Color")]
    public string Color { get; set; }
    [JsonProperty(PropertyName = "Text Outline Distance")]
    public string Distance { get; set; }
}

private class DockData
{
    [JsonProperty(PropertyName = "Position")]
    public DockPosition Position { get; set; }
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Background Color")]
    public string BackgroundColor { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty(PropertyName = "Panel Alignment (Left, Center, Right)")]
    public PanelAlignEnum Alignment { get; set; }
    [JsonProperty(PropertyName = "Panel Padding")]
    public float PanelPadding { get; set; }
    [JsonProperty(PropertyName = "Dock Padding")]
    public TypePadding DockPadding { get; set; }
}

private class DockPosition
{
    [JsonProperty(PropertyName = "X Position")]
    public float XPos { get; set; }
    [JsonProperty(PropertyName = "Y Start Position")]
    public float StartYPos { get; set; }
    [JsonProperty(PropertyName = "Height")]
    public float Height { get; set; }
}

private class StoredData
{
    public Hash<ulong, PlayerSettings> Settings;
}

private static class LangKeys
{
    public const string On;
    public const string Off;
    public const string SettingsChanged;
    public const string Chat;
    public const string Help;
}

private class PlayerSettings
{
    public bool Enabled { get; set; }
}

private class PanelRegistration
{
    public string Name { get; set; }
    public string Dock { get; set; }
    public float Width { get; set; }
    public int Order { get; set; }
    public string BackgroundColor { get; set; }
    public string GetPanelMethod { get; set; }
    public PanelTypeEnum PanelType { get; set; }
    public Plugin Plugin { get; set; }
    public string Permission { get; set; }
}

private class Panel
{
    public PanelImage Image { get; set; }
    public PanelText Text { get; set; }
    public Panel(Hash<string, object> data);
}

private abstract class PanelType
{
    public bool Enabled { get; set; }
    public string Color { get; set; }
    public int Order { get; set; }
    public float Width { get; set; }
    public TypePadding Padding { get; set; }
    protected PanelType(Hash<string, object> data);
}

private class PanelImage : PanelType
{
    public string Url { get; set; }
    public PanelImage(Hash<string, object> data);
}

private class PanelText : PanelType
{
    public string Text { get; set; }
    public int FontSize { get; set; }
    public TextAnchor TextAnchor { get; set; }
    public PanelText(Hash<string, object> data);
}

private class TypePadding
{
    public float Left { get; set; }
    public float Right { get; set; }
    public float Top { get; set; }
    public float Bottom { get; set; }
    [JsonConstructor]
    public TypePadding();
    public TypePadding(Hash<string, object> data);
    public TypePadding(float left, float right, float top, float bottom);
}

private class PanelSetup
{
    public DockPosition Pos { get; set; }
    public float StartPos { get; set; }
    public string UiParentPanel { get; set; }
    public string PanelColor { get; set; }
    public PanelRegistration PanelReg { get; set; }
}

private class PanelUpdate
{
    public CuiElementContainer Container { get; set; }
    public string PanelName { get; set; }
}

private class HiddenPanelInfo
{
    public List<ulong> PlayerHidden { get; set; }
    public bool All { get; set; }
    public HiddenPanelInfo();
}

private static class Ui
{
    private static string UiPanel { get; set; }
    public static CuiElementContainer Container(string color, UiPosition pos, string panel, string parent);
    public static void Label(CuiElementContainer container, string text, int size, string color, UiPosition pos, TextAnchor align);
    public static void OutlineLabel(CuiElementContainer container, string text, int size, string color, string outlineColor, string distance, UiPosition pos, TextAnchor align);
    public static void Image(CuiElementContainer container, string png, string color, UiPosition pos);
    public static string Color(string hexColor);
}

private class CuiOutlineLabel : CuiLabel
{
    public CuiOutlineComponent Outline { get; set; }
}

private class UiPosition
{
    private float XPos { get; set; }
    private float YPos { get; set; }
    private float Width { get; set; }
    private float Height { get; set; }
    public UiPosition(float xPos, float yPos, float width, float height);
    public string GetMin();
    public string GetMax();
    public override string ToString();
}


```

---

## MagicParatroopersPanel by MJSU - Displays if the paratroopers event is active

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Component = UnityEngine.Component;

Oxide.Plugins
[Info("Magic Paratroopers Panel", "MJSU", "1.0.1")]
[Description("Displays if the paratroopers event is active")]
public class MagicParatroopersPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private List<CargoPlane> _activePlanes;
    private bool _init;
    private const string ComponentName;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void CheckEvent();
    private void UnsubscribeAll();
    private void OnEntitySpawned(CargoPlane plane);
    private void OnEntityKill(CargoPlane plane);
    private Hash<string, object> GetPanel();
    private bool CanShowPanel(CargoPlane plane);
    private bool? MagicPanelCanShow(string name, CargoPlane plane);
    private bool HasComponent(BaseEntity entity, string name);
    private class PluginConfig
    {
        [DefaultValue("#DE8732FF")]
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
    [DefaultValue("#DE8732FF")]
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

## MagicPingPanel by MJSU - Displays the current ping for player in Magic Panel

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using Network;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Magic Ping Panel", "MJSU", "1.0.5")]
[Description("Displays players ping in magic panel")]
public class MagicPingPanel : RustPlugin
{
    [PluginReference]
    private Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private readonly Hash<ulong, int> _playerPing;
    private string _defaultColor;
    private Coroutine _updateRoutine;
    private string _text;
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
    private void UpdatePlayerPing();
    private IEnumerator HandleUpdatePlayerPing();
    private Hash<string, object> GetPanel(BasePlayer player);
    private int CheckPing(BasePlayer player);
    private class PluginConfig
    {
        [DefaultValue(5f)]
        [JsonProperty(PropertyName = "Update Rate (Seconds)")]
        public float UpdateRate { get; set; }
        [DefaultValue(true)]
        [JsonProperty(PropertyName = "Enable ping colors")]
        public bool EnablePingColors { get; set; }
        [JsonProperty(PropertyName = "Ping Colors")]
        public Hash<int, string> PingColors { get; set; }
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
    [JsonProperty(PropertyName = "Update Rate (Seconds)")]
    public float UpdateRate { get; set; }
    [DefaultValue(true)]
    [JsonProperty(PropertyName = "Enable ping colors")]
    public bool EnablePingColors { get; set; }
    [JsonProperty(PropertyName = "Ping Colors")]
    public Hash<int, string> PingColors { get; set; }
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

## MagicPlayersPanel by MJSU - Displays connected players in Magic Panel

```csharp
using System;
using System.ComponentModel;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Magic Players Panel", "MJSU", "1.0.3")]
[Description("Displays connected players in magic panel")]
public class MagicPlayersPanel : RustPlugin
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
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void UpdatePanel();
    private Hash<string, object> GetPanel();
    private class PluginConfig
    {
        [DefaultValue(false)]
        [JsonProperty(PropertyName = "Exclude Admins")]
        public bool ExcludeAdmins { get; set; }
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
    [DefaultValue(false)]
    [JsonProperty(PropertyName = "Exclude Admins")]
    public bool ExcludeAdmins { get; set; }
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

## MagicQueuePanel by MJSU - Displays queued players in Magic Panel

```csharp
using System;
using System.ComponentModel;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Magic Queue Panel", "MJSU", "1.0.3")]
[Description("Displays queued players in magic panel")]
public class MagicQueuePanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private string _textFormat;
    private int _queueCount;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void UpdatePanel();
    private Hash<string, object> GetPanel();
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Panel Settings")]
        public PanelRegistration PanelSettings { get; set; }
        [JsonProperty(PropertyName = "Panel Layout")]
        public Panel Panel { get; set; }
        [DefaultValue(5f)]
        [JsonProperty(PropertyName = "Update Rates (Seconds)")]
        public float UpdateRate { get; set; }
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
    [DefaultValue(5f)]
    [JsonProperty(PropertyName = "Update Rates (Seconds)")]
    public float UpdateRate { get; set; }
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

## MagicRadiationInfoPanel by MJSU - Displays how much radiation protection the player has verse how much they need

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.ComponentModel;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Magic Radiation Info Panel", "MJSU", "1.0.4")]
[Description("Displays how much radiation protection the player has verse how much they need")]
public class MagicRadiationInfoPanel : RustPlugin
{
    [PluginReference]
    private Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    private string _textFormat;
    private readonly Hash<ulong, PlayerRadiationData> _playerRadiation;
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
    private void UpdatePlayerRadiation();
    private IEnumerator HandleUpdatePlayerRadiation();
    private Hash<string, object> GetPanel(BasePlayer player);
    private PlayerRadiationData GetPlayerData(BasePlayer player);
    private void UpdatePlayerRadiation(BasePlayer player, PlayerRadiationData data);
    private class PluginConfig
    {
        [DefaultValue(5f)]
        [JsonProperty(PropertyName = "Update Rate (Seconds)")]
        public float UpdateRate { get; set; }
        [JsonProperty(PropertyName = "Panel Settings")]
        public PanelRegistration PanelSettings { get; set; }
        [JsonProperty(PropertyName = "Panel Layout")]
        public Panel Panel { get; set; }
    }

    private class PlayerRadiationData
    {
        public float RadAmount { get; set; }
        public float ProtectionAmount { get; set; }
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
    [JsonProperty(PropertyName = "Update Rate (Seconds)")]
    public float UpdateRate { get; set; }
    [JsonProperty(PropertyName = "Panel Settings")]
    public PanelRegistration PanelSettings { get; set; }
    [JsonProperty(PropertyName = "Panel Layout")]
    public Panel Panel { get; set; }
}

private class PlayerRadiationData
{
    public float RadAmount { get; set; }
    public float ProtectionAmount { get; set; }
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

## MagicRadiationPanel by MJSU - Displays if a player is in a radiation zone in Magic Panel

```csharp
using System;
using System.ComponentModel;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Magic Radiation Panel", "MJSU", "1.0.3")]
[Description("Displays if a player is in a radiation zone")]
public class MagicRadiationPanel : RustPlugin
{
    [PluginReference]
    private readonly Plugin MagicPanel;
    private PluginConfig _pluginConfig;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void MagicPanelRegisterPanels();
    private void OnEntityEnter(TriggerBase trigger, BaseEntity entity);
    private void OnEntityLeave(TriggerBase trigger, BaseEntity entity);
    private Hash<string, object> GetPanel(BasePlayer player);
    private class PluginConfig
    {
        [DefaultValue("#FFFF00FF")]
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
    [DefaultValue("#FFFF00FF")]
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

