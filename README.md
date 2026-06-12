# SmartFarm Simulation

> 📂 [포트폴리오 (Notion)](https://iced-tarsier-455.notion.site/350af40f5e6680d19f93f1653925a3b8) · 🎥 [시연 영상](https://youtu.be/gh-pwRRpz9Q) · 🔗 [Flask 서버](https://github.com/yaong832/Flask-SmartFarmFlask)

WinForms 클라이언트 — 스마트팜 시뮬레이터와 TwinCAT ADS 연동, Flask 중계 서버를 통해 웹 대시보드와 실시간 연동합니다.

## 기술 스택

- C# WinForms
- TwinCAT ADS / EtherCAT PLC
- Flask REST API 연동 (`Flask-SmartFarmFlask`)

## 연동 구조

```
시뮬레이터 ↔ WinForms ↔ Flask ↔ 웹 대시보드
```

## 관련 저장소

| 저장소 | 역할 |
|--------|------|
| [SmartFarmSimulation](https://github.com/yaong832/SmartFarmSimulation) | WinForms 클라이언트 (본 repo) |
| [Flask-SmartFarmFlask](https://github.com/yaong832/Flask-SmartFarmFlask) | Flask 중계·API 서버 |
