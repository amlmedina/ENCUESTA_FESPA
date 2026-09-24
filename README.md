# Sistema de Encuestas FESPA (SPA)

Aplicación Single Page Application (SPA) para gestión de encuestas personalizadas por token único, generador de campañas mediante CSV, métricas en tiempo real con Chart.js y persistencia en Firebase Firestore.

## 🚀 Características
- **Tokens Únicos por Contacto:** Cada usuario recibe un link único (`?token=XYZ`).
- **Control de Estado:** Solo permite contestar una única vez por token (`Pendiente` -> `Contestado`).
- **Módulo de Preguntas:** Soporta preguntas de 1 a 5 estrellas, selección múltiple y texto abierto.
- **Gestor de Campañas (CSV):** Subida masiva con PapaParse que genera enlaces directos para compartir.
- **Dashboard & Métricas:** Gráficas automáticas con Chart.js y exportación completa a `.csv`.
- **Panel Administrativo:** Protegido por PIN maestro (`10188852`).

---

## ⚙️ Configuración Requerida

### 1. Claves de Firebase
Abre `index.html` y reemplaza el objeto `firebaseConfig` con las credenciales de tu proyecto en Firebase Console:

```javascript
const firebaseConfig = {
  apiKey: "TU_API_KEY",
  authDomain: "TU_PROJECT.firebaseapp.com",
  projectId: "TU_PROJECT_ID",
  storageBucket: "TU_PROJECT.appspot.com",
  messagingSenderId: "TU_SENDER_ID",
  appId: "TU_APP_ID"
};
```

### 2. Reglas de Seguridad de Firestore
En **Firebase Console > Firestore Database > Reglas**, pega y publica lo siguiente:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /preguntas/{preguntaId} {
      allow read: if true;
      allow write: if false;
    }
    match /tokens/{tokenId} {
      allow read: if true;
      allow create: if true;
      allow update: if resource.data.estado == 'Pendiente' 
                    && request.resource.data.estado == 'Contestado'
                    && request.resource.data.diff(resource.data).affectedKeys().hasOnly(['estado', 'respondidoEn']);
    }
    match /respuestas/{respuestaId} {
      allow create: if request.resource.data.token != null
                    && get(/databases/$(database)/documents/tokens/$(request.resource.data.token)).data.estado == 'Pendiente';
      allow read: if true;
    }
  }
}
```

---

## 🌐 Publicar en GitHub Pages

1. En este repositorio en GitHub, ve a **Settings** > **Pages**.
2. En **Build and deployment > Branch**, selecciona `main` y la carpeta `/(root)`.
3. Haz clic en **Save**.
4. En unos minutos tu encuesta estará publicada en:
   `https://amlmedina.github.io/ENCUESTA_FESPA/`

- **Panel de Administración:** `https://amlmedina.github.io/ENCUESTA_FESPA/#admin`
- **PIN Maestro:** `10188852`
