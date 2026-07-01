# MushRush MVP Development Progress & Handoff Roadmap

This document serves as a status report and next-step guide for developers (human or AI) to continue coding the MushRush idle RPG project seamlessly from any workspace.

---

## 🎮 Game Concept & Architecture

* **Genre**: Vertical-screen Idle RPG (방치형 RPG)
* **Screen Layout**:
  * **Top Screen**: Game Play Zone (Player auto-attacks and moves right; monsters spawn in front; camera is locked to the player).
  * **Bottom Screen**: Tabbed Upgrade/Shop Zone (Scrollable lists for Gym upgrades, Item Shop, Skill Shop, and Cosmetic Shop).
* **Technical Framework**: MapleStory Worlds (MSW) client-server architecture. Core Version: `26.5.0.0`.
* **State Management**: Managed via [PlayerInfo.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Player/PlayerInfo.mlua) (Server-synced fields, automatic saving/loading to `UserDataStorage`).

---

## ✅ Completed Implementations

### 1. Data Saving, Loading & Offline Play
* **State Persistence**: Restored real progress loading for `GoldWalet` and `BeautyWalet` (removed the `999999999` hardcode). Starting defaults are configured (1,000 Gold, 100 Beauty) if no save is found.
* **Offline Gold Accumulation**: When logging in, the server calculates how much gold was accumulated offline (based on `IdleGoldLv` and elapsed time).
* **Toast Notification**: Upon calculating offline rewards, a server-to-client RPC triggers a toast notification informing the player of the gold gained.

### 2. Stage Progression & Cleanup
* **Infinite Looping**: StageManager automatically transitions players through maps (Stage 1 ➔ Stage 2). If there are no further stages in the CSV, it repeats the final stage.
* **Monster Cleanup Helper**: Added a `CleanUpMonsters` logic in [StageManager.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Field/StageManager.mlua) that clears all old normal/boss monster entities before spawning new ones. This resolves the monster duplication/accumulation bug on stage transition/repeats.

### 3. UI Framework & Mutual Exclusion (Tab System)
* **Pre-built Templates**: The UI layouts are visually designed in `.ui` files (`ItemShopUI.ui`, `GymUI.ui`, `SkillShop.ui`, `BeautyUI.ui`) and use native `ScrollLayoutGroupComponent` or `GridViewComponent` to support list generation.
* **Single-Panel Exhibition**: Every tab open button in `DefaultGroup` has been scripted to disable other shop panel UIs when opening its own. This guarantees that only one shop screen is active on the lower half of the screen at a time.

---

## 📂 File Registry (Key Project Files)

* **Game Loop & Spawning**: [StageManager.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Field/StageManager.mlua)
* **Player Stats & Wallets**: [PlayerInfo.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Player/PlayerInfo.mlua)
* **Auto-combat Logic**: [PlayerAttackComponent.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Player/PlayerAttackComponent.mlua), [PlayerAutoComponent.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Player/PlayerAutoComponent.mlua)
* **Shop Logics**:
  * Stats Upgrade: [Gym.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Shop/Gym.mlua)
  * Gear Shop: [ItemShopLogic.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Shop/ItemShopLogic.mlua)
  * Skill Shop: [SkillShop.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Shop/SkillShop.mlua)
  * Cosmetic Shop: [BeautyShop.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Shop/BeautyShop.mlua)
* **UI Layouts**:
  * Shop Screens: [ItemShopUI.ui](file:///c:/Workspace/MushRush/ui/ItemShopUI.ui), [GymUI.ui](file:///c:/Workspace/MushRush/ui/GymUI.ui), [SkillShop.ui](file:///c:/Workspace/MushRush/ui/SkillShop.ui), [BeautyUI.ui](file:///c:/Workspace/MushRush/ui/BeautyUI.ui)
  * Main Interface: [DefaultGroup.ui](file:///c:/Workspace/MushRush/ui/DefaultGroup.ui)
* **Database Tables**:
  * [StageDataSet.csv](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Field/StageDataSet.csv)
  * [NormalMonsterDataSet.csv](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Field/NormalMonsterDataSet.csv)
  * [BossMonsterDataSet.csv](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Field/BossMonsterDataSet.csv)
  * [ShopDataSet.csv](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Shop/ShopDataSet.csv)
  * [SkillDataSet.csv](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Shop/SkillDataSet.csv)

---

## 🚀 Handoff: Next Steps & Future Roadmap

When continuing development, focus on the following tasks:

### 1. Game Balance and Data Structuring
* **Goal / Upgrades Cost Scale**: Fine-tune the gold cost increase coefficient for Gym training (currently `1.08 ^ level`).
* **Monster Scaling**: Scale monster HP and gold rewards across stages. Currently:
  * Normal Snail (Stage 1) rewards `5,000,000,000` gold (overpowered for testing).
  * Orange Mushroom (Stage 2) rewards `7` gold.
  * Adjust these in [NormalMonsterDataSet.csv](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Field/NormalMonsterDataSet.csv) and [BossMonsterDataSet.csv](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Field/BossMonsterDataSet.csv) for real idle progression curve.

### 2. UI / UX Visual Polish (Maker Editor)
* **Layout Design**: Open the Maker UI editor and visually polish the borders, colors, and margins of the cloned shop items to align with the visual design expected from `UI 예상도.png`.
* **Meso & Icon Sprites**: Apply correct MapleStory asset Sprite RUIDs to item images, skill icons, and gold icons.

### 3. Combat Feedback & Polish
* **Floating Damage Numbers**: Integrate a damage skin system to draw floating damage text when the player hits a monster.
* **Sprite Hit Flash**: Trigger visual flashes on monsters when damaged.
* **SFX Pass**: Connect audio feedback (swing sounds, impact sounds, shop buying sounds) using `_SoundService:PlaySound()`.

### 4. Advanced Features
* **Premium Offline Reward Modal**: Replace the simple `_UIToast` notification with a modal popup window showing: "You were away for X hours. Gained +Y Gold!" with a "Double Reward" or "Claim" button.
