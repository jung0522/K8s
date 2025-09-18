# 🩸 [2025 kakao X 9oorm 시즌톤] 4팀 단짝이

# 기술 구현

### 기술 스택 적합성
- 문제 해결에 적절한 기술 스택 선택이 되었는가?

- **Flask (Python 3.11)** - AI 서비스 (LLM 기반 퀘스트 생성, RAG 기반 전문적 혈당 분석 후 피드백 제공)  
- **Spring Boot (Java 17)** - 기본적인 CRUD (도메인 중심 설계)
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

#### Spring-Service/danjjang (Java): **계층화된 아키텍처**
```plaintext
Spring-Service/
└── danjjang/                                    # 메인 Spring Boot 프로젝트
    └── src/
        ├── main/
        │   ├── java/com/cloud/danjjang/
        │   │   ├── DanjjangApplication.java     
        │   │   │
        │   │   ├── common/                      # 공통 모듈
        │   │   │   ├── apiPayload/              # API 응답 표준화
        │   │   │   │   ├── ApiResponse.java
        │   │   │   │   └── code/
        │   │   │   │       ├── BaseCode.java
        │   │   │   │       ├── BaseErrorCode.java  # 에러 헨들링 클래스들
        │   │   │   │       ├── ErrorReasonDTO.java
        │   │   │   │       ├── ReasonDTO.java
        │   │   │   │       └── status/
        │   │   │   │           ├── ErrorCode.java
        │   │   │   │           └── SuccessCode.java
        │   │   │   │
        │   │   │   ├── config/                  # 설정 클래스들
        │   │   │   │   ├── SecurityConfig.java
        │   │   │   │   ├── SwaggerConfig.java
        │   │   │   │   └── WebConfig.java
        │   │   │   │
        │   │   │   ├── entity/                  # 공통 엔티티
        │   │   │   │   └── BaseTimeEntity.java
        │   │   │   │
        │   │   │   ├── exception/               # 예외 처리
        │   │   │   │   ├── ExceptionAdvice.java
        │   │   │   │   ├── GeneralException.java
        │   │   │   │   └── handler/
        │   │   │   │       └── GeneralHandler.java
        │   │   │   │
        │   │   │   └── jwt/                     # JWT 인증
        │   │   │       ├── LoginService.java
        │   │   │       ├── SecurityUtils.java
        │   │   │       ├── TokenDTO.java
        │   │   │       ├── TokenProvider.java
        │   │   │       └── filter/
        │   │   │           ├── JwtAuthenticationFilter.java
        │   │   │           ├── LoginFilter.java
        │   │   │           └── LogoutFilter.java
        │   │   │
        │   │   └── domain/                      # 도메인별 모듈 (DDD 패턴)
        │   │       ├── enums/                   # 열거형(enmus) 사용
        │   │       │   ├── DiabetesType.java
        │   │       │   ├── ExerciseType.java
        │   │       │   ├── Gender.java
        │   │       │   ├── Sensor.java
        │   │       │   └── Status.java
        │   │       │
        │   │       ├── exercise/                # 운동 도메인
        │   │       │   ├── controller/
        │   │       │   │   └── ExerciseController.java
        │   │       │   ├── dto/
        │   │       │   │   ├── ExerciseRequestDTO.java
        │   │       │   │   └── ExerciseResponseDTO.java
        │   │       │   ├── entity/
        │   │       │   │   └── Exercise.java
        │   │       │   ├── repository/
        │   │       │   │   └── ExerciseRepository.java
        │   │       │   └── service/
        │   │       │       └── ExerciseService.java
        │   │       │
        │   │       ├── member/                  # 사용자 도메인
        │   │       │   ├── annotation/
        │   │       │   │   ├── AuthUser.java
        │   │       │   │   └── AuthUserArgumentResolver.java
        │   │       │   ├── controller/
        │   │       │   │   └── MemberController.java
        │   │       │   ├── dto/
        │   │       │   │   ├── CustomUserDetails.java
        │   │       │   │   ├── MemberRequestDTO.java
        │   │       │   │   ├── MemberResponseDTO.java
        │   │       │   │   ├── MemberSignDTO.java
        │   │       │   │   └── RefreshTokenDTO.java
        │   │       │   ├── entity/
        │   │       │   │   ├── Member.java
        │   │       │   │   └── Refresh.java
        │   │       │   ├── repository/
        │   │       │   │   ├── MemberRepository.java
        │   │       │   │   └── RefreshRepository.java
        │   │       │   └── service/
        │   │       │       ├── CodeGenerator.java
        │   │       │       ├── CustomUserDetailService.java
        │   │       │       ├── MemberMapper.java
        │   │       │       └── MemberService.java
        │   │       │
        │   │       ├── parent/                  # 부모 도메인
        │   │       │   └── entity/
        │   │       │       └── Parent.java
        │   │       │
        │   │       └── parentChild/             # 부모-자녀 관계
        │   │           ├── entity/
        │   │           │   └── ParentChild.java
        │   │           └── repository/
        │   │               └── ParentChildRepository.java
        │   │
      

```
---

### 확장성 고려
- 향후 기능 추가 시 유연하게 확장 가능한가?

#### AI-Service (Python)
1. **새로운 API 엔드포인트 추가:** `app/api/v1/endpoints/`  
2. **새로운 서비스 추가:** `app/services/`  
3. **새로운 데이터 모델:** `app/models/database_models.py`  
4. **이에 대한 유틸리티:** `app/utils/`  

➡️ 새로운 API 구현은 위 순서로 유연하게 확장 가능  

#### Spring-Service/danjjang (Java)
1. 도메인 중심 설계 (DDD) 적용: `domain/` 패키지로 비즈니스 로직 분리
2. 인터페이스 기반 설계: `domain/member/repository/MemberRepository.java` - CrudRepository 확장
3. 커스텀 어노테이션으로 확장성 확보: `domain/member/annotation/AuthUser.java` - 인증 사용자 주입

### 효율성 고려 
- 불필요한 오버엔지니어링 없이 효율적 구현하였는가?

#### AI-Service(Python): 
1. **캐싱 전략:** `app/utils/auth/jwt_auth.py` – 토큰 캐싱  
2. **성능 모니터링:** `app/utils/monitoring/performance_monitor.py` – 성능 추적  
3. **데이터베이스 성능 최적화:** `app/models/database_models.py` – 인덱스 설정  
4. **RAG 최적화:** `app/services/chroma_rag_service.py` – 벡터 검색 최적화

#### Spring-Service/danjjang (Java)
1. 공통 엔티티로 중복 제거: `common/entity/BaseTimeEntity.java`
2. 매핑 로직 분리로 서비스 계층 간소화: `domain/member/service/MemberMapper.java`
3. 커스텀 어노테이션으로 인증 간소화: `domain/member/annotation/AuthUserArgumentResolver.java`


---
### 코드 품질
- 가독성, 모듈화, 재사용성을 고려한 코드 작성 여부

#### AI-Service(Python): 
1. **가독성:** `app/utils/business/glucose_utils.py` - 명확한 함수명과 변수명  
2. **모듈화:** 계층별 모듈 분리  
3. **재사용성:** `app/utils/error/error_handler.py` - 공통 유틸리티 함수,
4. **데코레이터 패턴 활용:** `app/utils/auth/jwt_auth.py`

#### Spring-Service/danjjang (Java)
1. **가독성:**: `domain/member/service/MemberService.java` - 비즈니스 로직의 명확한 표현
2. **모듈화:** 계층별 모듈 분리  
3. **재사용성:** `common/apiPayload/ApiResponse.java` - 공통 유틸리티 함수,
4. **데코레이터 패턴 활용:** `common/jwt/filter/JwtAuthenticationFilter.java` 

---

### 유지보수성
- 코드 수정 및 기능 개선이 용이한가?  

#### AI-Service(Python)
1. **의존성 주입:** `app/__init__.py` - 팩토리 패턴  
2. **인터페이스 분리:** `app/utils/auth/authorization.py` - 권한 시스템 (ISP 준수)

#### Spring-Service/danjjang (Java)
1. **의존성 주입:**: `domain/member/service/MemberService.java`
2. **인터페이스 분리:**: `domain/member/repository/MemberRepository.java` (ISP 준수)


---

### 에러 핸들링
- API 응답, 예외 상황에 대해 적절히 처리하였는가?  

#### AI-Service(Python)
1. **계층화된 에러 클래스:** `app/utils/error/error_handler.py`  
2. **API 레벨 예외 처리:** `app/api/v1/endpoints/parents.py`  
3. **전역 예외 핸들러 등록:** `app/__init__.py`

#### Spring-Service/danjjang (Java)
1. 중앙화된 예외 처리: `common/exception/ExceptionAdvice.java` - 전역 예외 처리 핵심
2. 에러 코드 표준화: `common/apiPayload/code/status/ErrorCode.java` - 에러 코드 정의
3. 입력 검증 예외 처리: `domain/member/dto/MemberRequestDTO.java` - 입력 검증(Bean Validation 활용)

---

### 사용자 경험
- 오류 발생 시 사용자 친화적 메시지를 제공하였는가?  

#### AI-Service(Python)
1. **구조화된 사용자 메시지:** `app/utils/error/user_messages.py`  
2. **상황별 맞춤 메시지:** `app/utils/error/error_handler.py`  

#### Spring-Service/danjjang (Java)
1. 명확하고 이해하기 쉬운 에러 메시지: `common/apiPayload/code/status/ErrorCode.java`
2. 예외 상황별 맞춤형 메시지: `domain/member/service/MemberService.java` - 도메인별 예외 메시지

---

### 보안 고려사항
- 입력 검증, 인증/인가, 민감정보 보호 여부  

#### AI-Service(Python)
1. **포괄적인 입력 검증:** `app/utils/auth/input_validator.py`  
2. **강력한 JWT 인증:** `app/utils/auth/jwt_auth.py`  
3. **역할 기반 접근 제어:** `app/utils/auth/authorization.py`  
4. **보안 이벤트 추적:** `app/utils/auth/security_logger.py`

#### Spring-Service/danjjang (Java)
1. JWT 기반 강력한 인증 시스템: `common/jwt/TokenProvider.java` - 보안 강화된 JWT 구현
2. 권한 기반 접근 제어: `common/jwt/TokenProvider.java` - 세분화된 권한 관리
3. 데이터베이스 보안: `domain/member/entity/Member.java` - 엔티티 레벨 보안

---

### 모니터링
- 로깅, 디버깅이 용이한가?

#### AI-Service(Python)
1. **체계적인 로깅 설정:** `app/core/logging.py`  
2. **성능 추적:** `app/utils/monitoring/performance_monitor.py`  
3. **에러 통계 수집:** `app/utils/monitoring/monitoring.py`  
4. **시스템 상태 모니터링:** `app/api/v1/endpoints/health.py`

#### Spring-Service/danjjang (Java)
1. 보안 이벤트 로깅: `common/jwt/TokenProvider.java` - 보안 로깅
2. SLF4J 기반 로깅: `common/exception/ExceptionAdvice.java` - 전역 예외 로깅
3. Swagger를 통한 API 모니터링: `common/config/SwaggerConfig.java` - API 문서화 및 모니터링


### 👨‍💻 백엔드 개발자

- **정준영 (June Young Jung)** - AI-Service 🟢  
  🔗 [정준영](https://github.com/Jung0522)
- **김채원 (Cheolsu Kim)** - Spring Boot CRUD 🔵  
  🔗 [GitHub](https://github.com/kimcheolsu)

