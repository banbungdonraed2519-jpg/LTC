# API Documentation

## Base URL

```
http://localhost:4000/api
```

All requests require `Content-Type: application/json` header.

---

## Authentication

### Login

**POST** `/auth/login`

Create a new JWT session.

**Request Body:**
```json
{
  "username": "admin",
  "password": "admin123"
}
```

**Response (201):**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": 1,
    "username": "admin",
    "full_name": "Admin Hospital",
    "role": "admin"
  }
}
```

### Get Current User

**GET** `/auth/me`

Requires: `Authorization: Bearer <token>`

**Response (200):**
```json
{
  "user": {
    "id": 1,
    "username": "admin",
    "full_name": "Admin Hospital",
    "role": "admin"
  }
}
```

---

## Dashboard

### Get Summary

**GET** `/dashboard/summary`

Requires: `Authorization: Bearer <token>`

Returns totals and breakdowns by village.

**Response (200):**
```json
{
  "totals": {
    "patients": 2,
    "highRisk": 1,
    "visits": 1,
    "assessments": 1
  },
  "byVillage": [
    {
      "village": "บ้านบึง ม.1",
      "count": 1
    },
    {
      "village": "บ้านโนนกลาง ม.4",
      "count": 1
    }
  ]
}
```

---

## Patients

### List All Patients

**GET** `/patients`

Requires: `Authorization: Bearer <token>`

**Response (200):**
```json
[
  {
    "id": 1,
    "hn": "HN001",
    "cid": "1234567890123",
    "full_name": "คุณตา สมบูรณ์ ใจดี",
    "village": "บ้านบึง ม.1",
    "age": 78,
    "gender": "ชาย",
    "adl": 4,
    "risk": "สูง",
    "caregiver": "คุณดวงใจ ใจดี",
    "phone": "08x-xxx-xxxx",
    "bp": "150/95",
    "sugar": "195",
    "weight": "52",
    "note": "มีแผลกดทับระยะเริ่มต้น",
    "latitude": 15.3188,
    "longitude": 103.8540,
    "created_at": "2026-06-03T12:00:00Z"
  }
]
```

### Create Patient

**POST** `/patients`

Requires: `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "hn": "HN003",
  "cid": "3456789012345",
  "full_name": "คุณนาย ทดสอบ",
  "village": "บ้านบึง ม.1",
  "age": 65,
  "gender": "ชาย",
  "adl": 5,
  "risk": "กลาง",
  "caregiver": "หลาน",
  "phone": "08x-xxx-xxxx",
  "bp": "140/90",
  "sugar": "120",
  "weight": "70",
  "note": "ติดเตียง",
  "latitude": 15.32,
  "longitude": 103.85
}
```

**Response (201):** Returns the created patient object.

---

## Visits

### List All Visits

**GET** `/visits`

Requires: `Authorization: Bearer <token>`

**Response (200):**
```json
[
  {
    "id": 1,
    "patient_id": 1,
    "patient_name": "คุณตา สมบูรณ์ ใจดี",
    "village": "บ้านบึง ม.1",
    "visit_date": "2026-06-03T12:00:00Z",
    "note": "เปลี่ยนแผล, ประเมิน ADL, ให้คำแนะนำญาติ",
    "created_by": "nurse",
    "created_at": "2026-06-03T12:00:00Z"
  }
]
```

### Create Visit

**POST** `/visits`

Requires: `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "patient_id": 1,
  "visit_date": "2026-06-03T14:30:00Z",
  "note": "ตรวจแผล, บันทึก SOAP, ให้ยา"
}
```

If `visit_date` is omitted, it defaults to the current timestamp.

**Response (201):** Returns the created visit object.

---

## Assessments

### List All Assessments

**GET** `/assessments`

Requires: `Authorization: Bearer <token>`

**Response (200):**
```json
[
  {
    "id": 1,
    "patient_id": 1,
    "patient_name": "คุณตา สมบูรณ์ ใจดี",
    "type": "Braden",
    "score": 12,
    "result": "เสี่ยงสูง",
    "assessor": "nurse",
    "created_at": "2026-06-03T12:00:00Z"
  }
]
```

### Create Assessment

**POST** `/assessments`

Requires: `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "patient_id": 1,
  "type": "Barthel",
  "score": 11,
  "result": "พึ่งพิงบางส่วน"
}
```

Allowed assessment types: `Barthel`, `ADL`, `TAI`, `Braden`

**Response (201):** Returns the created assessment object.

---

## AI Chat

### Send Message

**POST** `/ai/chat`

Requires: `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "message": "ผู้ป่วยมีอาการไข้สูง 39.5°C เป็นเวลา 2 วัน แนะนำอะไรครับ"
}
```

**Response (200):**
```json
{
  "response": "ควรตรวจเลือด เก็บตัวอย่างเพื่อหาสาเหตุ..."
}
```

---

## Error Responses

All errors follow this format:

**401 Unauthorized:**
```json
{
  "message": "Unauthorized"
}
```

**500 Internal Server Error:**
```json
{
  "message": "Internal server error"
}
```

---

## Headers

All authenticated requests must include:
```
Authorization: Bearer <JWT_TOKEN>
Content-Type: application/json
```

---

## Rate Limiting

Currently, there is no rate limiting implemented. For production, consider adding rate-limiting middleware.

---

## Pagination

Currently, list endpoints return all records. For large datasets, consider implementing pagination:
- Add `?page=1&limit=20` query parameters
- Return `{ data: [...], total, page, limit }`
