# 🔐 비밀번호 변경 API 명세서 (v1)

사용자가 본인의 현재 비밀번호와 안드로이드 기기 인증을 통해 새로운 비밀번호로 변경하는 API입니다.

---

## 📌 개요

- **엔드포인트**: `/change_pass`
- **도메인**: [`https://api.smsapps78.club`](https://api.smsapps78.club)
- **지원 메서드**: `GET`, `POST`
- **응답 형식**: `application/json`

---

## 📥 요청(Request)

### ✅ 파라미터 목록

| 필드 | 타입 | 필수 | 설명 |
|------|------|------|------|
| `user_id` | string | ✅ | 사용자 ID |
| `current_password` | string | ✅ | 현재 비밀번호 |
| `new_password` | string | ✅ | 변경할 새 비밀번호 |
| `android_id` | string | ✅ | 등록된 안드로이드 고유 기기 ID |

---

### 📘 POST JSON 예시

```http
POST /change-password
Content-Type: application/json
```

```json
{
  "user_id": "testuser",
  "current_password": "oldpass",
  "new_password": "newpass123",
  "android_id": "device-abc"
}
```

---

## 📤 응답(Response)

### ✅ 성공

```json
{
  "success": true,
  "code": 200,
  "message": "비밀번호가 성공적으로 변경되었습니다.",
  "data": null
}
```

### ❌ 실패 예시 (비밀번호 불일치)

```json
{
  "success": false,
  "code": 401,
  "message": "현재 비밀번호가 일치하지 않습니다.",
  "data": null
}
```

---

## ✅ 유효성 검증

| 항목 | 설명 |
|------|------|
| 기존 비밀번호 검증 | 기존 비밀번호 확인 |
| 기기 인증 | 기존 저장된 id와 android_id 비교 |
| 암호화 방식 | bcrypt 기반 암호화 사용 |

---

## 🛑 제한된 요청 메서드

`GET`, `POST`만 허용  
기타 메서드는 `405 Method Not Allowed` 응답

---

## 📦 상태 코드 정의

| 코드 | 의미 |
|------|------|
| `200` | 비밀번호 변경 성공 |
| `400` | 필수 파라미터 누락 |
| `401` | 현재 비밀번호 불일치 |
| `403` | 등록되지 않은 기기 |
| `404` | 사용자 없음 |
| `405` | 허용되지 않은 메서드 |

