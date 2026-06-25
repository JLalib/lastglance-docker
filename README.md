# 📅 LastGLANCE Docker - Rastreador de Hábitos y Tareas Recurrentes

**LastGLANCE Docker** es la implementación en contenedores de **LastGLANCE**, un rastreador minimalista diseñado para responder a la pregunta: *"¿Cuándo fue la última vez que hice esto?"*. Es ideal para gestionar el mantenimiento del hogar, chequeos de salud, tareas recurrentes y hábitos sin la complejidad de la gamificación, priorizando la privacidad y el enfoque *local-first*.

---

## ✨ Características principales

- **Rastreo de "Última Vez"**: Registra la fecha y hora exacta de cualquier actividad con un solo clic.
- **Recordatorios Automáticos**: Configura frecuencias personalizadas para que la app te notifique cuándo es momento de repetir una tarea.
- **Histórico y Streaks**: Visualiza tu racha de días/semanas y accede a un timeline completo de cada actividad.
- **Organización por Categorías**: Clasifica tus tareas en grupos como Salud, Hogar, Trabajo o Mantenimiento.
- **Enfoque Local-First**: Los datos se almacenan en tu navegador, garantizando privacidad total y cero tracking.
- **PWA y Móvil**: Disponible como aplicación web progresiva (PWA) para iOS/Android o mediante app nativa.
- **Estadísticas Detalladas**: Calcula promedios de días entre actividades y analiza tendencias de cumplimiento.
- **Importación/Exportación**: Soporta backups en formato JSON para migrar datos entre dispositivos fácilmente.

---

## ⚠️ Requisitos previos

- **Docker** y **Docker Compose** (versión 2.x o superior).
- **256 MB - 512 MB RAM** (Extremadamente ligero).
- **100 MB - 1 GB espacio en disco**.
- **Puerto 3000** disponible (configurable).
- **Navegador moderno** con soporte para IndexedDB.

---

## ⚙️ Configuración con Docker Compose

### 1. Clona este repositorio y accede al directorio:
```bash
git clone https://github.com/JLalib/lastglance-docker.git
cd lastglance-docker
```

### 2. Archivo `docker-compose.yml`
Crea el archivo `docker-compose.yml` con la siguiente configuración:

```yaml
version: '3.8'

services:
  lastglance:
    image: ghcr.io/krelltunez/lastglance:latest
    container_name: lastglance
    restart: unless-stopped
    ports:
      - "3000:80"
    healthcheck:
      test: ["CMD-SHELL", "nc -z 127.0.0.1 80 || exit 1"]
      interval: 10s
      timeout: 5s
      retries: 3
```

### 3. Despliega la aplicación
```bash
docker compose up -d
```

### 4. Accede a la interfaz
Abre en tu navegador:
🔗 [http://localhost:3000](http://localhost:3000)

---

## 🛠️ Primeros pasos

### 1. Crear tu primer ítem
- Haz clic en el botón **"+"** o **"New Item"**.
- Asigna un nombre (ej: "Cambiar aceite del coche" o "Limpiar filtros AC").
- Selecciona una categoría y, si lo deseas, define una frecuencia ideal.

### 2. Registrar actividad
- Cuando completes la tarea, selecciona el ítem y haz clic en **"Mark Done"**. El timestamp se registrará automáticamente.

### 3. Configurar recordatorios
- Edita un ítem y habilita la opción **"Reminder"**.
- Define la frecuencia (ej: cada 1 mes) para recibir notificaciones.

### 4. Instalación en móvil
- En tu móvil, abre la URL y selecciona **"Añadir a la pantalla de inicio"** para usarlo como una PWA.

---

## 🔒 Seguridad y recomendaciones

### 1. Privacidad de Datos
Recuerda que LastGLANCE es *local-first*. Los datos residen en el almacenamiento del navegador (`IndexedDB`). Si borras los datos del sitio en el navegador, perderás tu información.

### 2. Backups Manuales
Para evitar la pérdida de datos, utiliza la función de exportación:
- Ve a **Settings → Export JSON** y guarda el archivo en un lugar seguro.
- Para restaurar, usa **Settings → Import JSON**.

### 3. Acceso Remoto Seguro (HTTPS)
Para acceder desde cualquier lugar, utiliza **Caddy** como reverse proxy:

**`Caddyfile`**:
```
track.tudominio.com {
    reverse_proxy localhost:3000
}
```

---

## 📂 Estructura del proyecto

```
./
├── docker-compose.yml    # Orquestación del servicio
└── README-LastGLANCE-Docker.md  # Este archivo
```

---

## 🔄 Actualización y mantenimiento

### Actualizar LastGLANCE
```bash
docker compose pull
docker compose up -d
```

### Comandos útiles
| Comando                          | Descripción                                  |
|----------------------------------|----------------------------------------------|
| `docker logs -f lastglance`      | Ver logs del contenedor.                      |
| `docker compose restart`         | Reiniciar el servicio.                         |
| `docker ps`                      | Verificar el estado (debe aparecer como *healthy*). |

---

## 📊 Comparativa con alternativas

| Característica               | LastGLANCE Docker | Habitica          | Streaks App       | Simple Habit Tracker |
|------------------------------|-------------------|-------------------|-------------------|----------------------|
| **Autohospedado**            | ✅ Sí             | ❌ No             | ❌ No             | ❌ No                |
| **Local-First / Privacidad** | ✅ Total          | ❌ Nube           | ✅ Sí             | ⚠️ Parcial           |
| **Gamificación**             | ❌ No             | ✅ Sí             | ❌ No             | ❌ No                |
| **Sencillez (Zero Friction)**| ✅ Máxima         | ❌ Complejo       | ✅ Alta           | ✅ Alta              |
| **Soporte PWA/Web**          | ✅ Sí             | ✅ Sí             | ❌ No             | ❌ No                |

---

## 📚 Referencias

- [LastGLANCE GitHub Repository](https://github.com/krelltunez/lastGLANCE)
- [Krelltunez (Desarrollador)](https://github.com/krelltunez)
- [dayGLANCE (Planificador diario)](https://github.com/krelltunez/dayGLANCE)
- [lifeGLANCE (Timeline de vida)](https://github.com/krelltunez/lifeGLANCE)

---
