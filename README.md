# Sistema de Encuestas FESPA (SPA)

Aplicación Single Page Application (SPA) para gestión de encuestas personalizadas por token único, generador de campañas mediante CSV, métricas en tiempo real con Chart.js y persistencia en Firebase Firestore.

## 🚀 Características
- **Tokens Únicos por Contacto:** Cada usuario recibe un link único (`?token=XYZ`).
- **Control de Estado:** Solo permite contestar una única vez por token (`Pendiente` -> `Contestado`).
- **Módulo de Preguntas:** Soporta preguntas de 1 a 5 estrellas, selección múltiple y texto abierto.
- **Gestor de Campañas (CSV):** Subida masiva con PapaParse que genera enlaces directos para compartir.
- **Dashboard & Métricas:** Gráficas automáticas con Chart.js y exportación completa a `.csv`.
- **Panel Administrativo:** Protegido por PIN dinámico almacenado en Firebase Firestore (`configuracion/acceso`) y configurable directamente desde el panel.

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
      allow read, write: if true;
    }
    match /tokens/{tokenId} {
      allow read, write: if true;
    }
    match /respuestas/{respuestaId} {
      allow read, write: if true;
    }
    match /configuracion/{docId} {
      allow read, write: if true;
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
- **PIN Inicial en Firestore:** `10188852` (puede ser cambiado desde el botón "Cambiar PIN" dentro del panel)
