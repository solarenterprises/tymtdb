# TymtDB

TymtDB is the game database for Tymt Launcher, storing metadata for all games available on the platform. It provides structured JSON data to ensure a smooth experience when fetching game details, downloading content, and managing updates.

## 📌 Structure

Each game entry in `index.json` follows the structure below:

```json
[
    {
      "GameID": "gta2",  // Unique identifier for the game
      "GameName": "GTA 2",  // Display name of the game
      "Publisher": "Rockstar Games",  // Name of the publisher
      "Category": ["RPG", "First Shooter"],  // List of game genres
      "ShortDescription": "A game by Rockstar Games",  // Short description
      "Visible": true,  // Determines if the game is visible in the store
      "XRated": false,  // Indicates if the game is adult-rated (18+)
      "GameFile": "gamesdb/rockstar-games/gta2/game.json",  // Path to detailed game metadata
      "Preview": "gamesdb/rockstar-games/gta2/preview.webp",  // Path to the preview image
      "Assets": "gamesdb/rockstar-games/gta2/assets/*.webp",  // Path pattern for additional assets
      "Price": 0.00,  // Game price (0.00 means free)
      "Blockchain": null,  // Blockchain integration (if applicable)
      "Type": "native",  // Game type (native, emulator, web, etc.)
      "downloadType": "torrent",  // Specifies the download method (torrent, direct, etc.)
      "Platform": 1,  // Target platform (1 = Windows, 2 = macOS, 3 = Linux)
      "isHyperPlayExclusive": false  // If the game is exclusive to HyperPlay
    }
]
```

## 📂 Folder Structure

```
index.json
/gamesdb/
  ├── rockstar-games/
  │   ├── gta2/
  │   │   ├── game.json  # Detailed game metadata
  │   │   ├── preview.webp  # Game preview image
  │   │   ├── assets/
  │   │   │   ├── image1.webp
  │   │   │   ├── image2.webp
  │   │   │   ├── image3.webp
  │   │   ├── dlc/
  │   │   │   ├── dlc1.json  # Metadata for DLC content
  │   │   ├── update/
  │   │   │   ├── update_1.1.json  # Patch/update information
```

## 📥 How It Works

### 🔍 **Fetching Game Data**
1. On launch, the Tymt client loads `index.json` from:
   ```
   https://raw.githubusercontent.com/tymtltd/gamedb/main/index.json
   ```
2. It **only downloads previews** to reduce bandwidth usage.
3. When a game is selected, the client fetches its detailed metadata from `game.json`.

### 🎮 **Game Download Process**
- If `downloadType` is `torrent`, the client will retrieve the **magnet link** from `game.json` and start downloading the game.
- If `downloadType` is `direct`, the client will download the game directly from the provided URL.

### 🔄 **Handling Game Updates**
- Updates are stored under `/update/` and listed in `game.json`.
- The client checks for updates and **downloads patch files only**, preventing full redownloads.
- The original game stays intact for **seeding**, ensuring other users can still download it via torrent.

### 🎟 **Handling DLCs**
- DLCs are stored under `/dlc/` and listed in `game.json`.
- The launcher fetches DLC information dynamically and downloads it separately.

## 🔗 Example: `game.json`
```json
{
  "_id": "gta2",
  "name": "GTA 2",
  "short_description": "A classic open-world game by Rockstar Games.",
  "description": "Experience the iconic top-down view action with crime and chaos.",
  "tags": ["RPG", "Action", "Open-World"],
  "price": 0.00,
  "dlcs": [
    {
      "id": "gta2-dlc1",
      "name": "GTA 2 Expansion Pack",
      "price": 5.99,
      "gameFile": "gamesdb/rockstar-games/gta2/dlc/dlc1.json"
    }
  ],
  "updates": [
    {
      "version": "1.1",
      "changelog": "Bug fixes and improved graphics.",
      "release_date": "2025-03-10",
      "gameFile": "gamesdb/rockstar-games/gta2/update/update_1.1.json"
    }
  ],
  "platforms": {
    "windows_amd64": {
      "name": "GTA2",
      "executable": "GTA2/gta2.exe",
      "installSize": "404MB",
      "external_url": "magnet:?xt=urn:btih:60672151faed81357b1659c8ead665da4d1ff3da"
    }
  }
}
```

## 🛠 **Future Improvements**
- Implementing **delta patching** to avoid full-game redownloads.
- Adding **checksum validation** to ensure file integrity.

---

🚀 **TymtDB is designed to be fast, efficient, and scalable, ensuring seamless game distribution with minimal overhead.**
