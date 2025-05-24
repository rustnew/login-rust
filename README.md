

# 🔐 Actix Web JWT Auth API

Une API construite avec [Actix Web](https://actix.rs/) en Rust qui implémente un système simple d'authentification basé sur les JSON Web Tokens (JWT). Elle expose trois routes principales, dont une protégée nécessitant un token valide.

---

## 🚀 Fonctionnalités

* Génération de token JWT à partir des données utilisateur
* Authentification via header `Authorization: Bearer <token>`
* Route publique accessible sans authentification
* Route privée protégée par un middleware personnalisé
* Traitement des erreurs propre (401 Unauthorized, etc.)

---

## 📦 Dépendances principales

* `actix-web` – pour la création de l’API REST
* `serde` / `serde_json` – pour la sérialisation/désérialisation JSON
* `jsonwebtoken` (via ton module `jwt`) – pour la création et la validation des tokens JWT

---

## 📁 Architecture du code

```text
.
├── main.rs          # Point d'entrée principal
├── jwt.rs           # Gestion des JWT : création et décodage
```

---

## 📌 Endpoints

### 1. 🔓 `GET /public-view`

**Description** : Affiche une réponse publique accessible à tous.

**Réponse :**

```json
{
  "success": true,
  "data": {
    "message": "This data is visible to all users"
  }
}
```

---

### 2. 🔑 `POST /get-token`

**Description** : Génère un token JWT à partir d'un utilisateur (envoyé en JSON).

**Corps de la requête (exemple) :**

```json
{
  "username": "john_doe",
  "email": "john@example.com"
}
```

**Réponse (succès) :**

```json
{
  "success": true,
  "data": {
    "token": "<jwt_token>"
  }
}
```

**Réponse (échec) :**

```json
{
  "success": false,
  "data": {
    "message": "Invalid user data"
  }
}
```

---

### 3. 🔒 `GET /secret-view`

**Description** : Route protégée nécessitant un token JWT dans le header HTTP.

**Header requis :**

```
Authorization: Bearer <jwt_token>
```

**Réponse (avec token valide) :**

```json
{
  "success": true,
  "data": {
    "username": "john_doe",
    "email": "john@example.com"
  }
}
```

**Réponse (sans token ou invalide) :**

```json
{
  "success": false,
  "data": {
    "message": "No token provided" // ou message d'erreur JWT
  }
}
```

---

## 🛠️ Middleware d'authentification

Le middleware `Auth` extrait automatiquement le token JWT du header `Authorization`, le décode et fournit les données utilisateur à la route :

```rust
struct Auth(User);

impl FromRequest for Auth {
    // ... logique d'extraction et vérification du token
}
```

---

## ▶️ Lancer le projet

```bash
cargo run
```

L'API sera accessible sur `http://127.0.0.1:2424`.

---

## 🔐 Exemple de requête avec `curl`

```bash
# Récupérer un token
curl -X POST http://127.0.0.1:2424/get-token \
  -H "Content-Type: application/json" \
  -d '{"username":"john", "email":"john@example.com"}'

# Accéder à la route protégée avec le token
curl -X GET http://127.0.0.1:2424/secret-view \
  -H "Authorization: Bearer <jwt_token>"
```

---

## 📚 À propos

Ce projet est un point de départ minimaliste pour apprendre à sécuriser une API Actix Web avec JWT. Il peut être facilement étendu avec :

* Une base de données pour persister les utilisateurs
* Une stratégie de rafraîchissement de tokens
* Des rôles et permissions

---

