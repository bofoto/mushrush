# MushRush (메이플스토리 월드 프로젝트)

**MushRush**는 메이플스토리 월드(MapleStory Worlds, MSW) 플랫폼을 기반으로 개발 중인 2D 횡스크롤 액션/RPG 게임 프로젝트입니다.

---

## 📂 프로젝트 구조 (Directory Structure)

본 프로젝트는 MSW 개발 표준 구조 및 에이전트 도구를 포함하고 있습니다.

*   **`RootDesk/MyDesk/`**: 핵심 게임 로직 및 스크립트가 위치하는 공간입니다.
    *   `Field/`: 게임 맵 및 스테이지 관련 컴포넌트
    *   `Player/`: 플레이어 상태, 컨트롤러 및 물리 동작 관련 스크립트
    *   `Shop/`: 게임 내 상점 시스템
    *   `Model_Boss.model`: 보스 몬스터의 모델 정의 파일
    *   `Model_Monster.model`: 일반 몬스터의 모델 정의 파일
    *   `Model_SkillProjectile.model`: 투사체형 스킬 모델 정의 파일
    *   `UIPopup.mlua` / `UIToast.mlua`: UI 팝업 및 토스트 알림 제어 스크립트
*   **`ui/` / `map/`**: 월드 UI 레이아웃과 맵 환경설정 데이터가 포함되어 있습니다.
*   **`Environment/` / `Global/`**: 전역 설정 및 환경 변수 관련 데이터입니다.


---

## 🛠️ 개발 기술 스택

*   **Platform**: MapleStory Worlds (MSW)
*   **Language**: Maple LUA (`.mlua`)
*   **CI/CD & Tools**: Git, MSW Native Components & Systems

---

## 🚀 시작하기 (How to Play & Test)

1.  **메이플스토리 월드 (MapleStory Worlds)**을 실행합니다.
2.  본 저장소를 프로젝트 폴더로 지정하여 불러옵니다.
3.  디벨로퍼 툴 내의 **Play** 버튼을 눌러 테스트를 진행할 수 있습니다.
