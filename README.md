# 🛡️ PASETO v3 Token Server (Express.js)

This project demonstrates how to implement **PASETO (Platform-Agnostic Security Tokens) Version 3** in a simple **Express.js API**.  
It uses asymmetric signing (`v3.public`) with **ECDSA-P384** keys for secure token generation and verification.

---

## 🚀 Features

- 🔐 Uses **PASETO v3.public** (asymmetric, ECDSA-P384)
- ⚙️ Generates key pairs automatically at startup
- 🧩 Simple Express.js API (`/token` and `/verify`)
- 🧠 Built with modern ES Modules
- 🌱 Environment variable support via `.env`

---

## 📦 Requirements

- **Node.js** ≥ 18
- **npm** ≥ 9

---

## 🧰 Installation

```bash
git clone codefalconx/express-paseto-v3
cd express-paseto-v3
npm install express paseto dotenv
```

> The project uses built-in Node.js `crypto` for key generation.

---

## ⚙️ Environment Setup

Create a `.env` file (optional):

```
PORT=3000
```

If not set, it defaults to port `3000`.

---

## ▶️ Running the Server

```bash
node server.js
```

You should see logs like:

```
🟡 Generating ECDSA-P384 key pair (v3.public) ...
✅ Keys ready, starting Express...
🚀 Server running on http://localhost:3000
```

---

## 🔑 Endpoints

### 1️⃣ `POST /token` — Issue a Token

**Request Body:**
```json
{
  "userId": "123",
  "role": "admin"
}
```

**Response:**
```json
{
  "token": "v3.public.eyJ...<longPasetoToken>..."
}
```

---

### 2️⃣ `POST /verify` — Verify a Token

**Request Body:**
```json
{
  "token": "v3.public.eyJ...<paste token here>..."
}
```

**Response (valid token):**
```json
{
  "valid": true,
  "payload": {
    "userId": 123,
    "role": "admin",
    "issuedAt": "2025-12-28T10:29:52.529Z",
    "iat": "2025-12-28T10:29:52.549Z",
    "exp": "2025-12-28T11:29:52.549Z",
    "aud": "users",
    "iss": "my-app"
  }
}```

**Response (invalid or expired):**
```json
{
  "error": "Invalid or expired token"
}
```

---

## 🧪 API Testing

### 🔹 Using cURL

**Issue a token:**
```bash
curl -X POST http://localhost:3000/token   -H "Content-Type: application/json"   -d '{
    "userId": "12345",
    "role": "admin"
  }'
```

**Verify a token:**
```bash
curl -X POST http://localhost:3000/verify   -H "Content-Type: application/json"   -d '{
    "token": "v3.public.eyJ...<your token>..."
  }'
```

---

### 🔹 Using Insomnia or Postman

**Request 1 — `/token`**
- Method: `POST`
- URL: `http://localhost:3000/token`
- Body (JSON):
  ```json
  {
    "userId": "12345",
    "role": "admin"
  }
  ```

**Request 2 — `/verify`**
- Method: `POST`
- URL: `http://localhost:3000/verify`
- Body (JSON):
  ```json
  {
    "token": "{{token_from_previous_response}}"
  }
  ```

---

## ⚡ Key Details

| Field | Description |
|--------|--------------|
| **PASETO Version** | v3.public |
| **Algorithm** | ECDSA-P384 |
| **Issuer** | `my-app` |
| **Audience** | `users` |
| **Expiration** | 1 hour |

---

## 🧩 Project Structure

```
.
├── server.js        # Main Express server using PASETO v3
├── package.json
└── .env             # Optional environment configuration
```

---

## 🧠 Notes

- Keys are **generated in memory** on startup (for demo only).  
  In production, you should **save them to files or a secure vault**.
- PASETO is **more secure and simpler** than JWT — no `alg` confusion or signature malleability.

---

## 📚 References

- [PASETO Specification](https://paseto.io)
- [Node.js paseto package (npm)](https://www.npmjs.com/package/paseto)
- [Express.js Documentation](https://expressjs.com)
