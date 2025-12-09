📘 The Stack Game – README

이 프로젝트는 Unity로 제작된 스택 쌓기 게임으로,
UI 관리, 블록 생성/절단, 콤보 시스템, 스코어 처리 등이 포함된 구조입니다.

📌 Table of Contents

프로젝트 소개

게임 구조

스크립트 설명

UIManager

TheStack

UI (HomeUI / GameUI / ScoreUI / BaseUI)

DestroyZone

게임 플레이 흐름

저장 데이터

프로젝트 계층 구조 예시

🎮 프로젝트 소개

이 프로젝트는 인기 모바일 게임 Stack과 유사한 메커니즘을 기반으로 제작된 Unity 미니게임입니다.

플레이어는 좌우 혹은 앞뒤로 움직이는 블록을 정확히 쌓아 올려 스코어와 콤보를 기록하게 됩니다.

기능 구성:

블록 생성 / 이동 / 절단 로직

콤보 시스템 및 보정

게임 오버 시 블록 낙하 연출

최고 기록 저장 (PlayerPrefs)

UI 상태(Home/Game/Score) 전환 시스템

🏗 게임 구조
UIState

게임의 화면 상태를 관리합니다.

public enum UIState { Home, Game, Score }

주요 화면

Home : Start / Exit 버튼 표시

Game : 실시간 점수, 콤보, 최대 콤보 표시

Score : 최종 점수 + 최고 기록 표시

📜 스크립트 설명
UIManager

UI 전체를 제어하는 핵심 관리자.

public class UIManager : MonoBehaviour
{
    // 상태 관리, UI 초기화, Score 업데이트, State 전환 등
}

기능 요약

싱글톤으로 동작

HomeUI / GameUI / ScoreUI 초기화 및 활성화 관리

Start 버튼 → 게임 시작

Exit 버튼 → 종료

점수 변경 시 GameUI 업데이트

게임 오버 시 ScoreUI로 전환

TheStack

게임의 메인 로직. 블록을 움직이고 쌓고 절단하는 모든 처리를 담당.

public class TheStack : MonoBehaviour
{
    // 블록 생성, 이동, 절단, 콤보, 최고기록 저장 등
}

주요 기능

✔ 블록 생성 (Spawn_Block)
✔ 블록 이동 (MoveBlock)
✔ 블록 절단 및 콤보 처리 (PlaceBlock / ComboCheck)
✔ 랜덤 색상 및 그라데이션 적용
✔ 스코어 · 콤보 · 최고 기록 관리 (PlayerPrefs)
✔ 게임 오버 시 블록 낙하 연출 (GameOverEffect)
✔ 재시작 기능 (Restart)

저장 데이터 Key

"BestScore"

"BestCombo"

UI (HomeUI / GameUI / ScoreUI / BaseUI)

UI 구조는 BaseUI를 상속하여 구성됩니다.

BaseUI
public abstract class BaseUI : MonoBehaviour
{
    protected UIManager uiManager;
    public virtual void Init(UIManager uiManager) { ... }
    protected abstract UIState GetUIState();
    public void SetActive(UIState state) { ... }
}


각 UI는 자신이 활성화 되어야 할 State를 반환하며 UIManager가 이를 토대로 UI를 켜거나 끕니다.

HomeUI

Start Button → 게임 시작

Exit Button → 앱 종료

GameUI
public void SetUI(int score, int combo, int maxCombo)


게임 중 표시되는 UI:

현재 점수

현재 콤보

최대 콤보

ScoreUI

게임 종료 후 표시되는 UI:

최종 점수

최종 콤보

최고 점수 (Best)

최고 콤보 (Best Combo)

Start 버튼 → 게임 재시작
Exit 버튼 → 게임 종료

DestroyZone

떨어진 블록(Rubble)을 정리하는 영역.

private void OnCollisionEnter(Collision collision)
{
    if (collision.gameObject.name.Equals("Rubble"))
        Destroy(collision.gameObject);
}

🎮 게임 플레이 흐름

Home 화면

Start 버튼 클릭 → TheStack.Restart() → Game 상태 진입

Game 화면

블록 이동

클릭 시 블록 배치

실패 시 Game Over

ScoreUI로 전환

Score 화면

최종 점수 표시

최고 기록 저장

Start 버튼으로 다시 게임 시작

💾 저장 데이터

프로젝트는 PlayerPrefs를 통해 최고 기록을 저장합니다.

Key	설명
BestScore	최고 점수
BestCombo	최고 콤보
📂 프로젝트 계층 구조 예시
Assets/
│── Scripts/
│   ├── UI/
│   │   ├── BaseUI.cs
│   │   ├── UIManager.cs
│   │   ├── HomeUI.cs
│   │   ├── GameUI.cs
│   │   └── ScoreUI.cs
│   │
│   ├── Game/
│   │   ├── TheStack.cs
│   │   └── DestroyZone.cs
│
│── Prefabs/
│── Scenes/
│── Materials/
│── Resources/
