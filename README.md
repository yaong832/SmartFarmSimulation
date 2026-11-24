# SmartFarm Simulation - 스마트팜 제어 HMI (WinForms)

> 📂 [포트폴리오 (Notion)](https://iced-tarsier-455.notion.site/350af40f5e6680d19f93f1653925a3b8) · 🎥 [시연 영상](https://youtu.be/gh-pwRRpz9Q) · 🔗 [Flask 서버 (Flask-SmartFarmFlask)](https://github.com/yaong832/Flask-SmartFarmFlask)

스마트팜 3동의 센서(습도·온도·채광·토양 습도)를 TwinCAT PLC에서 읽어 보여 주고, 농장별 기준값을 관리하고, [Flask 서버](https://github.com/yaong832/Flask-SmartFarmFlask)의 AI 분석 결과로 자동 제어하는 WinForms 프로그램입니다.

## 구성

```
TwinCAT PLC (EtherCAT I/O)  ◄─ ADS ─►  이 프로그램 (WinForms)  ◄─ HTTP ─►  Flask 서버  ◄─  웹 대시보드
 아날로그 입력: 센서 4개                  화면, 기준값, 로그                     분석, AI 제어 명령
 디지털 입력: 장비 버튼 1~4
 디지털 출력: 램프 1~4
```

| PLC 변수 | 내용 |
| --- | --- |
| `GVL.NX_AD4203` | 아날로그 입력 (센서 4개). 1초마다 읽음 |
| `GVL.NX_ID5342` | 디지털 입력. 장비 버튼 1~3 = 스마트팜 1~3 선택, 버튼 4 = AI 자동 제어 켜기/끄기 |
| `GVL.NX_OD5121` | 디지털 출력 (램프 1~4). 램프 1 = 전원, 2 = 기준보다 낮은 센서 있음, 3 = 기준보다 높은 센서 있음, 4 = 모두 정상 |

## 주요 기능

- **스마트팜 1~3 선택**: 화면 버튼 또는 장비 버튼 1~3. 농장마다 작물·메모·센서 기준값(최소·최대)·보정값을 따로 가짐
- **작물 기준 자동 적용**: 작물을 고르면 Flask에서 그 작물의 최적 조건을 받아 기준값으로 씀 (`/api/crops/<작물>`), 새 작물 추가 창
- **AI 자동 제어**: 화면 버튼 또는 장비 버튼 4. Flask가 돌려준 명령(`/api/ai/control`)대로 센서 보정값을 조정
- **웹 연결**: PLC에서 읽을 때마다 센서 값을 Flask로 보내 웹 대시보드에 표시 (`/api/sensor-data`)
- **로그**: 화면 아래 최근 10줄, 전체 로그 창(전체·오류·경고·정보 탭), 5분마다 센서 값 기록
- **설정 저장**: 기준값과 농장 정보는 실행 폴더의 `sensor_thresholds.txt`, `farm_info.txt`에 저장

---

## 요구 사항

- Windows, Visual Studio 2019 이상 (.NET Framework 4.7.2)
- **TwinCAT 3** (같은 PC, PLC 포트 851). 빌드할 때 `C:\TwinCAT\AdsApi\.NET\v4.0.30319\TwinCAT.Ads.dll`을 참조하므로 TwinCAT이 설치된 PC에서만 빌드됩니다.
- PLC 프로젝트에 위 `GVL` 변수 3개가 있어야 센서 값이 들어옵니다. PLC 없이 실행하면 화면은 뜨지만 EtherCAT 연결이 실패하고 센서 값이 없습니다.
- (선택) [Flask-SmartFarmFlask](https://github.com/yaong832/Flask-SmartFarmFlask) 서버. 주소는 `http://localhost:5000`으로 코드에 고정 (`Form1.cs`의 `FlaskServerUrl`)

## 실행

1. (선택) Flask 서버를 먼저 실행 (Flask 저장소의 `run_farmui.bat`)
2. TwinCAT PLC를 Run 모드로
3. `SmartFarmSimulation.sln`을 열고 F5 (NuGet이 `Newtonsoft.Json`을 복원). 실행 파일 이름은 `farm.exe`
4. **전원** → **EtherCAT 전원** 순서로 켜면 센서 값이 들어옵니다
5. **웹 연결**을 누르면 Flask로 데이터를 보내기 시작하고, 브라우저 `http://localhost:5000`에서 볼 수 있습니다

다른 PC에 옮겨 실행하는 방법: [docs/배포_가이드.md](docs/배포_가이드.md)

## 파일

| 파일 | 역할 |
| --- | --- |
| `Form1.cs` | 메인 화면, PLC(ADS) 읽기, Flask 통신, AI 자동 제어, 로그 |
| `AddCropForm.cs` | 새 작물 추가 (Flask `/api/crops`) |
| `FarmManageForm.cs` | 농장별 작물·메모 설정 |
| `LogForm.cs` | 전체 로그 창 |
| `docs/` | 배포 가이드, AI 기능 제안·코드 개선사항·데이터 요구사항 문서, 학습용 샘플 로그 |
