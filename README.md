# 🏠 Housemate - 가정용 AI Agent

> 텔레그램을 통해 자연어로 모든 가정 관리 업무를 처리하는 스마트 AI 어시스턴트

## 📋 프로젝트 개요

Housemate는 텔레그램을 인터페이스로 사용하여 가정의 다양한 업무를 자연어로 처리할 수 있는 AI Agent입니다. n8n 워크플로우 기반으로 구축되어 있으며, 스케줄 관리, 사진 편집, 스마트홈 제어 등 다양한 기능을 제공합니다.

## ✨ 주요 기능

### 📅 스케줄 관리
- **Google Calendar 연동**: 일정 생성, 조회, 수정, 삭제
- **자연어 명령**: "내일 오후 2시에 회의 일정 잡아줘"
- **음성 인식**: 음성 메시지를 텍스트로 변환하여 처리

### 🖼️ 사진 편집 (나노바나나)
- **Gemini AI 기반**: 고품질 이미지 편집
- **다양한 편집 기능**: 배경 제거, 밝기/대비 조정, 크롭, 텍스트 추가
- **자동 처리**: 텔레그램으로 사진 전송 시 자동 편집

### 🏡 스마트홈 제어 (SmartThings)
- **가전제품 제어**: 에어컨, 조명, 스위치 등
- **자연어 명령**: "거실 에어컨 1도 낮춰줘"
- **장면 제어**: 미리 설정된 장면 실행
- **상태 확인**: 현재 가전제품 상태 조회

### 🤖 AI 에이전트 시스템
- **Parent AI Agent**: 사용자 요청을 분석하고 적절한 서브 에이전트 호출
- **도메인별 전문 에이전트**: 
  - Calendar Agent (일정 관리)
  - SmartThings Agent (스마트홈 제어)
  - Gemini Image Agent (이미지 편집)
  - Think Agent (정보 수집 및 정규화)

## 🚀 향후 계획

### 👶 베이비 케어 도우미 기능
- **수유 관리**: 수유 시간 기록 및 알림
- **수면 패턴**: 아기 수면 시간 추적
- **성장 기록**: 키, 몸무게 등 성장 데이터 관리
- **건강 모니터링**: 체온, 증상 기록

## 🛠️ 기술 스택

- **워크플로우 엔진**: n8n
- **AI 모델**: 
  - OpenAI GPT-5
  - OpenRouter (다양한 모델 지원)
  - Google Gemini (이미지 처리)
- **통합 서비스**:
  - Telegram Bot API
  - Google Calendar API
  - Google Drive API
  - Google Sheets API
  - SmartThings API
- **언어**: JavaScript (n8n 노드)

## 📱 사용법

1. **텔레그램 봇과 대화**: 텔레그램에서 봇에게 메시지 전송
2. **자연어 명령**: 원하는 작업을 자연어로 설명
3. **자동 처리**: AI가 명령을 분석하고 적절한 서비스 호출
4. **결과 확인**: 처리 결과를 텔레그램으로 수신

### 예시 명령어
```
"내일 오후 2시에 팀 미팅 일정 잡아줘"
"거실 에어컨 22도로 설정해줘"
"이 사진 배경 제거해줘"
"오늘 일정 알려줘"
```

## 🔧 설치 및 설정

### 필요 조건
- n8n 인스턴스
- Telegram Bot Token
- Google API 키 (Calendar, Drive, Sheets)
- SmartThings API 토큰
- OpenAI API 키

### 설정 방법
1. n8n에서 워크플로우 import
2. 각 서비스의 API 키 설정
3. 웹훅 URL 구성
4. 워크플로우 활성화

## 🔗 서비스 연동 방법

### 🍌 Nanobanana (이미지 편집) 연동

#### 1. 설정 파일 확인
```bash
# nanobanana/config.json 파일에서 엔드포인트 확인
{
  "endpoints": {
    "edit": "https://primary-production-42b01.up.railway.app/webhook/a099d311-150f-4a53-a7fa-e5d6f2bea2dc"
  }
}
```

#### 2. n8n 워크플로우 설정
- **Gemini Image Agent** 노드에서 HTTP Request Tool 설정
- URL: `https://primary-production-42b01.up.railway.app/webhook/a099d311-150f-4a53-a7fa-e5d6f2bea2dc`
- Method: POST
- Body: JSON 형태로 편집 파라미터 전송

#### 3. 사용 예시
```
텔레그램에서 이미지 전송 → "배경 제거해줘" → AI가 자동으로 Nanobanana API 호출 → 편집된 이미지 반환
```

### 🏠 SmartThings (스마트홈) 연동

#### 1. 설정 파일 확인
```bash
# smartthings/config.json 파일에서 지원 기기 및 명령 확인
{
  "supported_devices": ["air_conditioner", "light", "switch", "sensor", "scene"],
  "endpoints": {
    "devices": "https://primary-production-42b01.up.railway.app/webhook/18ed55b9-ed62-487c-b20e-7fd12bc86730"
  }
}
```

#### 2. n8n 워크플로우 설정
- **SmartThings** 노드에서 HTTP Request Tool 설정
- URL: `https://primary-production-42b01.up.railway.app/webhook/18ed55b9-ed62-487c-b20e-7fd12bc86730`
- Method: POST
- Body Parameters: deviceId, commands, summary, text

#### 3. 사용 예시
```
텔레그램에서 "거실 에어컨 22도로 설정해줘" → AI가 SmartThings API 호출 → 에어컨 온도 조절
```

### 📁 프로젝트 구조
```
Housemate/
├── README.md                 # 메인 문서
├── Housemate.json           # n8n 워크플로우
├── nanobanana/              # 이미지 편집 서비스
│   ├── README.md
│   └── config.json
└── smartthings/             # 스마트홈 제어 서비스
    ├── README.md
    └── config.json
```

## 📊 모니터링

- **Google Sheets 연동**: 모든 요청과 응답을 자동 로깅
- **에러 추적**: 실패한 작업에 대한 상세 로그
- **성능 모니터링**: 처리 시간 및 성공률 추적

## 🤝 기여하기

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 라이선스

이 프로젝트는 MIT 라이선스 하에 배포됩니다. 자세한 내용은 `LICENSE` 파일을 참조하세요.

## 📞 연락처

- 프로젝트 링크: [https://github.com/terrykim12/Housemate/](https://github.com/terrykim12/Housemate/)
- 이메일: terrykim0324@gmail.com

## 🙏 감사의 말

- n8n 커뮤니티
- OpenAI
- Google AI
- SmartThings 개발팀

---

**Housemate**로 더 스마트한 가정 생활을 시작하세요! 🏠✨
