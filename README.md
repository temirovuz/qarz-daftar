# Qarzdorlar Hisoboti API

Bu loyiha foydalanuvchilar, ularning qarzlari va to‘lovlarini boshqarish uchun REST API taqdim etadi.

---

## **Model tuzilmalari**

### **Users** (Foydalanuvchilar)

- `full_name` — Foydalanuvchi nomi.
- `phone_number` — Telefon raqami.
- `credit_balance` — Hisobidagi mablag‘.

### **Debt** (Qarz)

- `user` — Qarzdor foydalanuvchi.
- `amount` — To‘lanishi shart bo‘lgan miqdor.
- `remaining` — Qolgan qarz miqdori.
- `status` — Qarz holati (`pending`, `partially_paid`, `paid`).
- `description` — Qarz haqida ma’lumot.
- `created_at` — Yaratilgan vaqti.

### **Payment** (To‘lov)

- `user` — Qarzdor foydalanuvchi.
- `amount` — To‘lov miqdori.
- `description` — To‘lov haqida ma’lumot.
- `created_at` — Yaratilgan vaqti.

### **DebtPayment** (Qarzga Tegishli To‘lov)

- `debt` — Qarz.
- `payment` — To‘lov.
- `amount` — Miqdor.
- `created_at` — Yaratilgan vaqti.

---

## **O‘rnatish va ishga tushirish**

```bash
git clone <repo-url>
cd <project-folder>

# Virtual environment yaratish va faollashtirish
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Kerakli paketlarni o‘rnatish
pip install -r requirements.txt
```

<br>

## Api `Qo'llanma`

<br>

| Method   | Description                                                                                    |
| -------- | ---------------------------------------------------------------------------------------------- |
| `GET`    | Bitta element yoki elementlar to'plamini olish uchun ishlatiladi.                              |
| `POST`   | Yangi narsalarni yaratishda foydalaniladi, masalan: yangi ishchi, maxsulot ...                 |
| `PATCH`  | Elementdagi bir yoki bir nechta maydonlarni yangilash uchun ishlatiladi.                       |
| `PUT`    | Butun elementni (barcha maydonlarni) yangi ma'lumotlar bilan almashtirish uchun foydalaniladi. |
| `DELETE` | Elementni o'chirish uchun ishlatiladi.                                                         |

<br>

| Method   | URL                                    | Description                            |
| -------- | -------------------------------------- | -------------------------------------- |
| `GET`    | `/api/v1/users`                        | Barcha qarzdorlarni olish.             |
| `POST`   | `/api/v1/users`                        | Yangi qarzdor yaratish.                |
| `GET`    | `/api/v1/user/28`                      | №28 qarzdor olish.                     |
| `PATCH`  | `/api/v1/user/28`                      | Qarzdorni ma'lumotlarni yangilash #28. |
| `DELETE` | `/api/v1/user/28` or `/api/v1/user/50` | Qarzdorni o'chirish                    |

<br>
<br>

## HTTP javob holati kodlari

Bu API ishlatayotganda server tomonidan qaytariladigan muhim status xabarlaridir. Har bir kod ma'lum bir holatni
bildiradi va tushunish oson bo'lishi uchun turlarga bo'lingan.

### Asosiy HTTP Status Kodlari:

| Code  | Title                   | Description                                                                                   |
| ----- | ----------------------- | --------------------------------------------------------------------------------------------- |
| `200` | `OK`                    | So'rov muvaffaqiyatli bajarildi (masalan. foydalanganda `GET`, `PATCH`, `PUT` yoki `DELETE`). |
| `201` | `Created`               | Yangi resurs yaratildi (masalan, POST so'rovi orqali).                                        |
| `400` | `Bad request`           | So'rov noto'g'ri yuborilgan (masalan, majburiy maydonlar yo'q)                                |
| `401` | `Unauthorized`          | Foydalanuvchi avtorizatsiyadan o'tmagan.                                                      |
| `403` | `Forbidden`             | Foydalanuvchiga ruxsat yo'q (avtorizatsiyadan o'tgan bo'lsa ham).                             |
| `404` | `Not found`             | So'ralgan resurs topilmadi.                                                                   |
| `500` | `Internal server error` | Serverda kutilmagan xato yuz berdi.                                                           |
| `502` | `Bad Gateway`           | Gateway yoki proxy server noto'g'ri javob qaytardi.                                           |
| `503` | `Service Unavailable`   | Server hozircha ishlamayapti (vaqtincha).                                                     |

<br>
<br>
<br>

## 🔐 Authentication (JWT)

- **Ushbu loyiha JWT (JSON Web Token) orqali autentifikatsiya qiladi.**
- **Har bir so‘rovda Authorization header quyidagicha bo‘lishi kerak:**

```
Authorization: Bearer <access_token>
```

## Token olish


**POST** ``/api/v1/login/``


```
{
    "phone_number": "+998947999898",
    "password": "Password123"
}
```

**Response**

```
{
  "phone_number": "+998947999898",
  "tokens": {
    "refresh": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9........",
    "access": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9........"
  }
}
```

## Qarzdorlar uchun api qo'llanma

<br>

**GET** - ALL USERS url `api/v1/users/`

```
[
    {
        "id": 5,
        "full_name": "Temirov Muhammad",
        "phone_number": "+998947999898",
        "credit_balance": "350.00"
    },
    {
        "id": 6,
        "full_name": "Temirov Jahongir",
        "phone_number": "+998332999898",
        "credit_balance": "0.00"
    }
]
```

**POST** CREATE USER url `api/v1/users/`

```
{
    "phone_number": "+998947999899",
    "full_name": "Temirov Jahongir"
}
```

**GET** SEARCH USER (phone_number or full_name) `api/v1/users/?query=Muhammad`

```
[
    {
        "id": 5,
        "full_name": "Temirov Muhammad",
        "phone_number": "+998947999898",
        "credit_balance": "350.00"
    }
]
```

<br>
<br>

## Qarzlar uchun api qo'llanma

<br>

**GET** - ALL DEBT url `api/v1/debts/`

```
[
    {
        "id": 15,
        "user": 5,
        "full_name": "Temirov Muhammad",
        "phone_number": "+998947999898",
        "amount": "150.00",
        "remaining": "0.00",
        "status": "paid",
        "description": "",
        "created_at": "2025-08-08T15:03:49.166587+05:00"
    },
    {
        "id": 16,
        "user": 5,
        "full_name": "Temirov Muhammad",
        "phone_number": "+998947999898",
        "amount": "1000.00",
        "remaining": "0.00",
        "status": "paid",
        "description": "",
        "created_at": "2025-08-08T15:04:29.999033+05:00"
    },
    {
        "id": 17,
        "user": 5,
        "full_name": "Temirov Muhammad",
        "phone_number": "+998947999898",
        "amount": "500.00",
        "remaining": "0.00",
        "status": "paid",
        "description": "",
        "created_at": "2025-08-08T15:06:07.412026+05:00"
    },
    {
        "id": 18,
        "user": 5,
        "full_name": "Temirov Muhammad",
        "phone_number": "+998947999898",
        "amount": "500.00",
        "remaining": "50.00",
        "status": "partially_paid",
        "description": "",
        "created_at": "2025-08-08T15:07:41.457576+05:00"
    }
]
```

**POST** CREATE DEBT url `api/v1/debts/`

```
{
    "user": 5,
    "amount": 100,
    "description": "Qarz haqida malumot Ixtiyoriy"
}
```

**GET** FILTER DEBT `api/v1/debts/?/api/v1/debts/?user=1&status=pending&created_at=2025-08-09&full_name=Muhamamd` **_(har biri alohida alohida ishlatilishi ham mumkin)_**

```
[
  {
    "id": 1,
    "user": 1,
    "full_name": "Temirov Muhammad",
    "phone_number": "+998947999898",
    "amount": "100.00",
    "remaining": "100.00",
    "status": "pending",
    "description": "",
    "created_at": "2025-08-09T23:46:57.227019+05:00"
  },
  {
    "id": 2,
    "user": 2,
    "full_name": "Alisher",
    "phone_number": "+998888688783",
    "amount": "100.00",
    "remaining": "150.00",
    "status": "pending",
    "description": "",
    "created_at": "2025-08-09T23:48:24.589064+05:00"
  }
]
```

<br>

## To'lovlar uchun api qo'llanma

<br>

**GET** - ALL PAYMENT url `api/v1/payments/`

```
[
  {
    "id": 1,
    "user": 1,
    "full_name": "Temirov Muhammad",
    "phone_number": "+998947999898",
    "amount": "100.00",
    "description": null,
    "created_at": "2025-08-10T14:35:27.405674+05:00"
  }
]
```

**POST** - CREATE PAYMENT url `api/v1/payments/`

```
{
  "user": 1,
  "amount": "100000.00",
  "description": "To'lov qilindi."
}
```

**SEARCH PAYMENT** GET PAYMENT FILTERS `api/v1/payments/?user=1&created_at=2025-08-10` **(Alohida alohida foydalansa ham bo'ladi)**

```

{
    "id": 1,
    "user": 1,
    "full_name": "Temirov Muhammad",
    "phone_number": "+998947999898",
    "amount": "100.00",
    "description": null,
    "created_at": "2025-08-10T14:35:27.405674+05:00"
  }
```

<br>
<br>

[![Python Version](https://img.shields.io/badge/python-3.11%2B-blue.svg)](https://www.python.org/downloads/)

- [GitHub](https://github.com/temirovuz)
- [Instagram](https://www.instagram.com/dev_temirov/)
- [Telegram](https://t.me/Temirov)


## 🛠 Ishlatilgan texnologiyalar

![JWT](https://img.shields.io/badge/JWT-black?style=for-the-badge&logo=JSON%20web%20tokens)
![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white) 
![Redis](https://img.shields.io/badge/redis-%23DD0031.svg?style=for-the-badge&logo=redis&logoColor=white)
![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=for-the-badge&logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)


![Django](https://img.shields.io/badge/django-%23092E20.svg?style=for-the-badge&logo=django&logoColor=white) 
![DjangoREST](https://img.shields.io/badge/DJANGO-REST-ff1709?style=for-the-badge&logo=django&logoColor=white&color=ff1709&labelColor=gray) 
