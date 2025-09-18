# 기술 구현

## 1. 기술적 설계 및 아키텍처

### 기술 스택 적합성
- 문제 해결에 적절한 기술 스택 선택이 되었는가?

- **Flask (Python 3.11)** - AI 서비스 (LLM 기반 퀘스트 생성, RAG 기반 전문적 혈당 분석 후 피드백 제공)  
- **Spring Boot (Java 17)** - 기본적인 CRUD (도메인 구조 설계, 하나의 엔티티로 부모/아이 회원 인증)  
- **MySQL** - 메인 데이터베이스  
- **ChromaDB** - 벡터 데이터베이스 (RAG용)  
- **Docker Compose** - 컨테이너화 및 배포  

---

### 아키텍처 설계
- 시스템 구조가 요구사항에 맞게 설계되었는가?

#### AI-Service (Python) : **계층화된 아키텍처**

'''
AI-Service/
├── main.py # 애플리케이션 진입점
├── app/
│ ├── init.py # Flask 앱 팩토리
│ ├── core/ # 핵심 설정 및 AI 모듈
│ │ ├── config.py # 환경 설정
│ │ ├── logging.py # 로깅 설정
│ │ └── ai.py # OpenAI API 클라이언트
│ ├── api/v1/ # API 레이어
│ │ ├── router.py # 라우터 등록
│ │ └── endpoints/ # 엔드포인트 구현
│ │ ├── health.py # 헬스체크
│ │ ├── quests.py # 퀘스트 관리
│ │ ├── parents.py # 부모 관련 기능
│ │ └── children.py # 아이 관련 기능
│ ├── database/ # 데이터베이스 레이어
│ │ ├── database.py # DB 연결 관리
│ │ ├── init.py # DB 초기화
│ │ └── database_utils.py # DB 유틸리티
│ ├── models/ # 데이터 모델
│ │ └── database_models.py # SQLAlchemy 모델
│ ├── services/ # 비즈니스 서비스
│ │ ├── chroma_rag_service.py # RAG 서비스
│ │ └── glucose_service.py # 혈당 서비스
│ ├── utils/ # 유틸리티 모듈
│ │ ├── auth/ # 인증/보안
│ │ ├── error/ # 에러 처리
│ │ ├── monitoring/ # 모니터링
│ │ ├── business/ # 비즈니스 로직
│ │ └── common/ # 공통 유틸리티
│ ├── prompts/ # AI 프롬프트 템플릿
│ ├── embedding_data/ # RAG 임베딩 데이터
│ └── cache/ # 캐시 디렉토리
└── requirements.txt # 의존성 관리
'''

