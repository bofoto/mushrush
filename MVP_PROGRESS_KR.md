# MushRush MVP 개발 진행 현황 및 인수인계 로드맵

이 문서는 개발자(인간 또는 AI)가 어떤 작업 환경에서든 MushRush 방치형 RPG 프로젝트 개발을 원활하게 이어갈 수 있도록 돕는 진행 현황 보고서 및 다음 단계 가이드입니다.

---

## 🎮 게임 콘셉트 및 아키텍처

* **장르**: 세로 화면 방치형 RPG (Mobile/Portrait format)
* **화면 레이아웃**:
  * **상단 화면**: 게임 플레이 영역 (플레이어가 자동 전진 및 자동 공격하며, 몬스터가 전방 우측에서 스폰되고 카메라는 플레이어에 고정됨).
  * **하단 화면**: 탭 형식의 강화/상점 영역 (체육관 스탯 강화, 장비 상점, 스킬 상점, 코스튬 상점을 스크롤 리스트로 제어).
* **기술 프레임워크**: 메이플스토리 월드(MSW) 클라이언트-서버 아키텍처. 코어 버전: `26.5.0.0`.
* **상태 관리**: [PlayerInfo.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Player/PlayerInfo.mlua)를 통해 캐릭터 데이터를 관리 (서버 동기화 필드 및 `UserDataStorage`를 통한 자동 저장/로드).

---

## ✅ 구현 완료된 항목

### 1. 데이터 저장, 로드 및 오프라인 플레이
* **상태 지속성 (Save/Load)**: `GoldWalet` 및 `BeautyWalet`에 대한 실제 진행도 로드 복구 (`999999999` 하드코딩 제거). 최초 저장 데이터가 없을 경우 시작 기본값(1,000 골드, 100 뷰티)이 설정되도록 구조화했습니다.
* **오프라인 골드 누적**: 로그인 시, 서버는 유저의 `IdleGoldLv` 스탯과 마지막 로그아웃 대비 경과 시간을 체크해 오프라인 방치 보상을 계산합니다.
* **토스트 알림**: 오프라인 보상 지급 성공 시, 서버-클라이언트 RPC를 실행하여 화면에 획득 골드를 안내하는 토스트 메시지(`_UIToast:ShowMessage`)를 노출합니다.

### 2. 스테이지 진행 및 클린업
* **무한 루프 구조**: StageManager가 맵 이동(Stage 1 ➔ Stage 2)을 자동 처리하며, 다음 스테이지 데이터가 없을 시 최종 스테이지를 자동 반복 파밍하도록 연동했습니다.
* **몬스터 클린업 헬퍼**: [StageManager.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Field/StageManager.mlua)에 새로운 스테이지 웨이브가 시작될 때 기존에 살아있거나 잔존하는 몬스터/보스 엔티티(`script.MonsterComponent` 및 `script.BossMonsterComponent`)를 모두 파괴하는 `CleanUpMonsters` 로직을 추가했습니다. 이를 통해 동일 맵 반복 사냥 시 몬스터가 중복 누적되는 버그가 해결되었습니다.

### 3. UI 프레임워크 및 상호 배타성 (탭 시스템)
* **기본 UI 구조 설계**: UI 레이아웃 파일들(`ItemShopUI.ui`, `GymUI.ui`, `SkillShop.ui`, `BeautyUI.ui`) 내에 스크롤 리스트 구현을 위한 `ScrollLayoutGroupComponent`와 복제 대상이 될 아이템 슬롯 템플릿이 이미 에디터 상에 올바르게 구조화되어 있습니다.
* **상점 패널 자동 닫기**: `DefaultGroup` 내의 모든 탭 버튼(스탯, 장비, 스킬, 뷰티)의 클릭 이벤트를 수정하여, 특정 상점 UI 패널이 켜질 때 다른 UI 패널들은 전부 비활성화(Enable = false) 상태로 전환되도록 설정했습니다. 화면 하단에 탭 방식의 깔끔한 화면 교체가 보장됩니다.

---

## 📂 파일 레지스트리 (주요 프로젝트 파일)

* **게임 루프 및 스폰 제어**: [StageManager.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Field/StageManager.mlua)
* **플레이어 스탯 및 재화 상태**: [PlayerInfo.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Player/PlayerInfo.mlua)
* **자동 전투 및 사냥**: [PlayerAttackComponent.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Player/PlayerAttackComponent.mlua), [PlayerAutoComponent.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Player/PlayerAutoComponent.mlua)
* **상점 및 업그레이드**:
  * 스탯 훈련소: [Gym.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Shop/Gym.mlua)
  * 장비 상점: [ItemShopLogic.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Shop/ItemShopLogic.mlua)
  * 스킬 상점: [SkillShop.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Shop/SkillShop.mlua)
  * 코스튬 상점: [BeautyShop.mlua](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Shop/BeautyShop.mlua)
* **UI 레이아웃**:
  * 각 팝업 화면: [ItemShopUI.ui](file:///c:/Workspace/MushRush/ui/ItemShopUI.ui), [GymUI.ui](file:///c:/Workspace/MushRush/ui/GymUI.ui), [SkillShop.ui](file:///c:/Workspace/MushRush/ui/SkillShop.ui), [BeautyUI.ui](file:///c:/Workspace/MushRush/ui/BeautyUI.ui)
  * 메인 디폴트 화면: [DefaultGroup.ui](file:///c:/Workspace/MushRush/ui/DefaultGroup.ui)
* **데이터베이스 테이블**:
  * [StageDataSet.csv](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Field/StageDataSet.csv)
  * [NormalMonsterDataSet.csv](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Field/NormalMonsterDataSet.csv)
  * [BossMonsterDataSet.csv](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Field/BossMonsterDataSet.csv)
  * [ShopDataSet.csv](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Shop/ShopDataSet.csv)
  * [SkillDataSet.csv](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Shop/SkillDataSet.csv)

---

## 🚀 인수인계: 다음 작업 및 개발 로드맵

향후 바이브 코딩 및 이어서 진행할 고도화 작업은 아래 단계를 순차적으로 권장합니다:

### 1. 게임 밸런스 및 데이터 정교화
* **강화 비용 공식 튜닝**: 체육관(Gym) 훈련 시 다음 레벨 강화에 요구되는 골드 공식을 기획에 맞춰 수정 (현재는 `1.08 ^ 레벨` 비례).
* **스테이지 밸런스 곡선 설정**: 현재 테스트를 위해 달팽이(Stage 1) 처치 시 50억 골드를 지급하는 스케일 등은 지나치게 오버스펙입니다. 
  * [NormalMonsterDataSet.csv](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Field/NormalMonsterDataSet.csv) 및 [BossMonsterDataSet.csv](file:///c:/Workspace/MushRush/RootDesk/MyDesk/Field/BossMonsterDataSet.csv)에서 몬스터 HP와 골드 드롭 수치를 플레이 템포에 맞춰 밸런싱해야 합니다.

### 2. UI / UX 비주얼 꾸미기 (에디터 내 수동 작업)
* **슬롯 디자인 폴리싱**: 메이커 에디터 뷰포트에서 생성된 UI 파일들을 열고, 각 리스트 슬롯(`ItemSlot`, `StatSlot` 등)의 테두리 디자인, 마진, 버튼 위치, 색상 등을 [UI 예상도.png](file:///c:/Workspace/MushRush/UI%20예상도.png) 이미지 가이드에 맞춰 수동으로 가다듬습니다.
* **스프라이트 RUID 적용**: 아이콘 이미지 란에 실제 알맞은 메이플스토리 인벤토리 아이템 및 스킬 RUID 코드를 대입해 줍니다.

### 3. 전투 및 타격 타격감 연출
* **데미지 스킨 및 플로팅 텍스트**: 플레이어 공격 시 타격 데미지를 화면에 숫자로 띄워주는 데미지 스킨 효과 구현.
* **피격 플래시**: 몬스터가 피격당했을 때 일시적으로 몬스터 스프라이트를 흰색으로 반짝이게(Flash) 처리.
* **사운드 효과 (SFX)**: 무기 휘두르는 소리, 몬스터 타격 효과음, 상점 구매 효과음 등을 `_SoundService:PlaySound()`를 사용해 추가.

### 4. 고급 편의 기능 추가
* **방치 보상 획득 모달 팝업**: 현재 토스트 메시지로 안내하는 오프라인 보상 알림을 한 단계 높여, "오프라인 동안 X시간 방치하여 +Y 골드를 획득하였습니다!" 형태로 전용 모달 창(확인 / 2배 받기 등의 버튼 탑재)을 띄우도록 개선합니다.
