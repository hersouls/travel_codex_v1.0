# 📘 API 명세서 (API Specification) – 최대 확장형 초안

본 문서는 Moonwave Travel 웹서비스의 모든 기능이 안정적이고 효율적으로 운영될 수 있도록 각 API의 명세를 상세하게 정의하여 개발 및 연동 시 에러 없이 구현 가능하게 합니다.

---

## 🌐 1. API 명세서 목적 (Purpose)

- 명확한 API 정의로 프론트엔드, 백엔드, AI 시스템 간 원활한 통합 지원
- 고성능, 고가용성 API 설계로 서비스 성능 극대화
- API 요청 및 응답 명세를 정확하게 제공하여 개발 효율성 향상
- 글로벌 데이터 보호 규정(GDPR, CCPA 등) 준수를 위한 데이터 처리 명확화

---

## 🌐 2. 공통 API 규칙 (Common API Rules)

### 📌 2.1 요청 및 응답 규격

- 데이터 형식: JSON
- 인코딩: UTF-8
- 응답 속도 목표: 평균 500ms 이내

### 📌 2.2 공통 응답 구조 (Response Structure)

```json
{
  "status": "success" | "error",
  "message": "상태 메시지",
  "data": { /* 응답 데이터 객체 */ }
}
🌐 3. API 상세 명세 (Detailed API Endpoints)
🚩 3.1 여행 일정 카드 CRUD API (/api/trip-cards)
📌 GET: 여행 일정 카드 목록 조회
URL: /api/trip-cards?userId={userId}

Method: GET

Request Parameters
파라미터	타입	필수	설명
userId	String	O	사용자 고유 ID

Response
json
복사
편집
{
  "status": "success",
  "message": "일정 목록 조회 성공",
  "data": [
    {
      "tripId": "trip_001",
      "tripName": "제주 힐링 여행",
      "startDate": "2025-08-01",
      "endDate": "2025-08-05",
      "destination": "제주도",
      "coverImageUrl": "https://...",
      "status": "PLANNED",
      "isFavorite": true
    },
    /* 추가 일정 카드 객체... */
  ]
}
📌 POST: 여행 일정 카드 등록
URL: /api/trip-cards

Method: POST

Request Body
json
복사
편집
{
  "userId": "user_001",
  "tripName": "제주 힐링 여행",
  "startDate": "2025-08-01",
  "endDate": "2025-08-05",
  "destination": "제주도",
  "coverImageUrl": "https://..."
}
Response
json
복사
편집
{
  "status": "success",
  "message": "일정 등록 성공",
  "data": {
    "tripId": "trip_001"
  }
}
📌 PUT: 여행 일정 카드 수정
URL: /api/trip-cards/{tripId}

Method: PUT

Request Body
json
복사
편집
{
  "tripName": "제주 힐링 여행 수정",
  "startDate": "2025-08-02",
  "endDate": "2025-08-06",
  "destination": "제주도 서귀포",
  "coverImageUrl": "https://..."
}
Response
json
복사
편집
{
  "status": "success",
  "message": "일정 수정 성공",
  "data": {
    "tripId": "trip_001"
  }
}
📌 DELETE: 여행 일정 카드 삭제
URL: /api/trip-cards/{tripId}

Method: DELETE

Response
json
복사
편집
{
  "status": "success",
  "message": "일정 삭제 성공",
  "data": {}
}
🚩 3.2 여행 세부 일정 카드 CRUD API (/api/trip-detail-cards)
📌 GET: 세부 일정 카드 목록 조회
URL: /api/trip-detail-cards?tripId={tripId}

Method: GET

Response
json
복사
편집
{
  "status": "success",
  "message": "세부 일정 목록 조회 성공",
  "data": [
    {
      "detailId": "detail_001",
      "title": "오설록 티뮤지엄 방문",
      "dateTime": "2025-08-02T10:00:00",
      "locationName": "오설록 티뮤지엄",
      "locationCoords": {"lat": 33.3061, "lng": 126.2895},
      "description": "녹차 아이스크림 맛보기"
    },
    /* 추가 세부 일정 객체... */
  ]
}
📌 POST: 세부 일정 카드 등록
URL: /api/trip-detail-cards

Method: POST

Request Body
json
복사
편집
{
  "tripId": "trip_001",
  "userId": "user_001",
  "title": "오설록 티뮤지엄 방문",
  "dateTime": "2025-08-02T10:00:00",
  "locationName": "오설록 티뮤지엄",
  "locationCoords": {"lat": 33.3061, "lng": 126.2895},
  "description": "녹차 아이스크림 맛보기",
  "googlePlaceId": "ChIJyWEHuEmjfDURgF5UCuL7xsI"
}
Response
json
복사
편집
{
  "status": "success",
  "message": "세부 일정 등록 성공",
  "data": {
    "detailId": "detail_001"
  }
}
📌 PUT: 세부 일정 카드 수정
URL: /api/trip-detail-cards/{detailId}

Method: PUT

Request Body
json
복사
편집
{
  "title": "오설록 티뮤지엄 재방문",
  "dateTime": "2025-08-02T11:00:00",
  "description": "추가로 녹차케이크 먹기"
}
Response
json
복사
편집
{
  "status": "success",
  "message": "세부 일정 수정 성공",
  "data": {
    "detailId": "detail_001"
  }
}
📌 DELETE: 세부 일정 카드 삭제
URL: /api/trip-detail-cards/{detailId}

Method: DELETE

Response
json
복사
편집
{
  "status": "success",
  "message": "세부 일정 삭제 성공",
  "data": {}
}
🚩 3.3 AI 여행 일정 자동 생성 및 추천 API (/api/ai/itinerary-generation)
URL: /api/ai/itinerary-generation

Method: POST

Request Body
json
복사
편집
{
  "userId": "user_001",
  "destination": "제주도",
  "startDate": "2025-08-01",
  "endDate": "2025-08-05",
  "travelStyle": ["힐링", "미식"]
}
Response
json
복사
편집
{
  "status": "success",
  "message": "AI 여행 일정 자동 생성 성공",
  "data": {
    "tripDetails": [ /* 자동 생성 세부 일정 리스트 */ ]
  }
}
🌐 4. API 에러 응답 구조 (Error Response)
json
복사
편집
{
  "status": "error",
  "message": "에러 발생 이유",
  "data": null
}
🌐 5. API 보안 및 인증 전략 (Security & Authentication)
모든 API 요청은 SSL/TLS 암호화 통신

Supabase Auth 기반 JWT 토큰 사용 (Bearer Token 방식)

권한별 API 접근 제한 명확히 정의 및 관리

🌐 6. API 성능 및 가용성 목표 (Performance & Availability Goals)
성능 지표	목표 기준
API 응답 속도	평균 500ms 이내
글로벌 가용성	99.99% 이상