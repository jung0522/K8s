# 🩸 [2025 kakao X 9oorm 시즌톤] 4팀 단짝이



1형 당뇨 아동을 위한 게이미피케이션 기반 자가관리 플랫폼

---



### 🚀 핵심 프로세스

---



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

```plaintext
AI-Service/
├── main.py                          # 애플리케이션 진입점
├── app/
│   ├── __init__.py                  # Flask 앱 팩토리
│   ├── core/                        # 핵심 설정 및 AI 모듈
│   │   ├── config.py                # 환경 설정
│   │   ├── logging.py               # 로깅 설정 
│   │   └── ai.py                    # OpenAI API 클라이언트
│   ├── api/v1/                      # API 레이어
│   │   ├── router.py                # 라우터 등록
│   │   └── endpoints/               # 엔드포인트 구현
│   │       ├── health.py            # 헬스체크
│   │       ├── quests.py            # 퀘스트 관리
│   │       ├── parents.py           # 부모 관련 기능
│   │       └── children.py          # 아이 관련 기능
│   ├── database/                    # 데이터베이스 레이어
│   │   ├── database.py              # DB 연결 관리
│   │   ├── init.py                  # DB 초기화
│   │   └── database_utils.py        # DB 유틸리티
│   ├── models/                      # 데이터 모델
│   │   └── database_models.py       # SQLAlchemy 모델
│   ├── services/                    # 비즈니스 서비스
│   │   ├── chroma_rag_service.py    # RAG 서비스
│   │   └── glucose_service.py       # 혈당 서비스
│   ├── utils/                       # 유틸리티 모듈
│   │   ├── auth/                    # 인증/보안
│   │   ├── error/                   # 에러 처리
│   │   ├── monitoring/              # 모니터링
│   │   ├── business/                # 비즈니스 로직
│   │   └── common/                  # 공통 유틸리티
│   ├── prompts/                     # AI 프롬프트 템플릿
│   ├── embedding_data/              # RAG 임베딩 데이터
│   └── cache/                       # 캐시 디렉토리
└── requirements.txt                 # 의존성 관리
```

#### Spring-Service/danjjang (Java)

---

### 확장성 고려
- 향후 기능 추가 시 유연하게 확장 가능한가?

#### AI-Service (Python)
1. **새로운 API 엔드포인트 추가:** `app/api/v1/endpoints/`  
2. **새로운 서비스 추가:** `app/services/`  
3. **새로운 데이터 모델:** `app/models/database_models.py`  
4. **이에 대한 유틸리티:** `app/utils/`  

➡️ 새로운 API 구현은 위 순서로 유연하게 확장 가능  

---
### 코드 품질
- 가독성, 모듈화, 재사용성을 고려한 코드 작성 여부  

1. **가독성:** `app/utils/business/glucose_utils.py` - 명확한 함수명과 변수명  
2. **모듈화:** 계층별 모듈 분리  
3. **재사용성:** `app/utils/error/error_handler.py` - 공통 유틸리티 함수  
4. **데코레이터 패턴 활용:** `app/utils/auth/jwt_auth.py`  

---

### 유지보수성
- 코드 수정 및 기능 개선이 용이한가?  

1. **의존성 주입:** `app/__init__.py` - 팩토리 패턴  
2. **인터페이스 분리:** `app/utils/auth/authorization.py` - 권한 시스템  

---

### 에러 핸들링
- API 응답, 예외 상황에 대해 적절히 처리하였는가?  

1. **계층화된 에러 클래스:** `app/utils/error/error_handler.py`  
2. **API 레벨 예외 처리:** `app/api/v1/endpoints/parents.py`  
3. **전역 예외 핸들러 등록:** `app/__init__.py`  

---

### 사용자 경험
- 오류 발생 시 사용자 친화적 메시지를 제공하였는가?  

1. **구조화된 사용자 메시지:** `app/utils/error/user_messages.py`  
2. **상황별 맞춤 메시지:** `app/utils/error/error_handler.py`  

---

### 보안 고려사항
- 입력 검증, 인증/인가, 민감정보 보호 여부  

1. **포괄적인 입력 검증:** `app/utils/auth/input_validator.py`  
2. **강력한 JWT 인증:** `app/utils/auth/jwt_auth.py`  
3. **역할 기반 접근 제어:** `app/utils/auth/authorization.py`  
4. **보안 이벤트 추적:** `app/utils/auth/security_logger.py`  

---

### 모니터링
- 로깅, 디버깅이 용이한가?  

1. **체계적인 로깅 설정:** `app/core/logging.py`  
2. **성능 추적:** `app/utils/monitoring/performance_monitor.py`  
3. **에러 통계 수집:** `app/utils/monitoring/monitoring.py`  
4. **시스템 상태 모니터링:** `app/api/v1/endpoints/health.py`  
