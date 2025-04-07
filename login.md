# 🔐 Login API 명세서 (v1)

API 엔드포인트 `/login`은 사용자의 로그인 요청을 처리하며, 안드로이드 고유번호 기반 기기 검증, 유효기간 검사를 포함합니다.

---

## 📌 개요

- **도메인**: [`https://api.smsapps78.club`](https://api.smsapps78.club)
- **엔드포인트**: `/login`
- **지원 메서드**: `GET`, `POST` (※ 기타 메서드는 405 응답)
- **응답 형식**: `application/json`
- **인증방식**: ID + PW + Android 고유 ID

---

## 📥 요청(Request)

### ✅ 지원 메서드

| Method | 설명 | Content-Type |
|--------|------|--------------|
| `GET`  | 파라미터 쿼리스트링 전달 | N/A |
| `POST` | 권장, `form` 또는 `JSON` 전송 | `application/x-www-form-urlencoded` or `application/json` |

---

### ✅ 파라미터 목록

| 필드 | 타입 | 필수 | 설명 |
|------|------|------|------|
| `user_id` | string | ✅ | 사용자 ID (mb_id) |
| `password` | string | ✅ | 비밀번호 |
| `android_id` | string | ✅ | 안드로이드 기기 고유 ID |

---

### 📘 GET 예시

```
GET /login?user_id=testuser&password=1234&android_id=device-abc
```

### 📘 POST JSON 예시

```http
POST /login
Content-Type: application/json
```

```json
{
  "user_id": "testuser",
  "password": "1234",
  "android_id": "device-abc"
}
```

---

## 📤 응답(Response)

### ✅ 공통 구조

```json
{
  "success": true | false,
  "code": 200,
  "message": "설명 메시지",
  "data": { ... } | null
}
```

---

### 📗 성공 예시

```json
{
  "success": true,
  "code": 200,
  "message": "로그인 성공",
  "data": {
    "mb_id": "testuser",
    "android_id": "device-abc"
  }
}
```

---

### 📕 실패 예시 (기기 불일치)

```json
{
  "success": false,
  "code": 403,
  "message": "다른 기기에서 이미 등록된 계정입니다.",
  "data": null
}
```

---

## ✅ 유효성 검증

| 항목 | 설명 |
|------|------|
| ID 존재 여부 | `member` 테이블에서 `mb_id` 조회 |
| 비밀번호 검증 | 전용 함수 사용 |
| 로그인 기간 | 접속가능 기간 검증 |
| 기기 ID 검증 | 등록 여부 및 일치 확인 |
| 최초 로그인 시 | `android_id` 자동 저장 |

---

## 🛑 제한된 요청 메서드

`GET`, `POST`만 허용  
이외의 요청(`PUT`, `PATCH`, `DELETE`, `HEAD`)은 `405 Method Not Allowed` 응답 처리됨.

---

## 📦 상태 코드 정의

| 코드 | 의미 |
|------|------|
| `200` | 로그인 성공 |
| `400` | 필수 파라미터 누락 |
| `401` | 비밀번호 불일치 |
| `403` | 날짜 또는 기기 불일치 |
| `404` | 사용자 없음 |
| `405` | 허용되지 않은 메서드 |
| `500` | 서버 내부 오류 |

---

## 🧾 버전 히스토리

| 버전 | 일자 | 내용 |
|------|------|------|
| v1.0 | 2025-03-29 | 최초 작성 및 구현 완료 (GET/POST 방식) |

---
