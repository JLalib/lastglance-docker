# 📊 LastGLANCE Docker - Rastreador de Hábitos y Tareas Recurrentes

[![GitHub](https://img.shields.io/badge/GitHub-krelltunez%2Flastglance-181717?logo=github)](https://github.com/krelltunez/lastglance)
[![Docker](https://img.shields.io/badge/Docker-ghcr.io%2Fkrelltunez%2Flastglance-2496ED?logo=docker)](https://github.com/krelltunez/lastglance/pkgs/container/lastglance)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](https://github.com/krelltunez/lastglance/blob/main/LICENSE)

## 📋 Descripción general

**LastGLANCE** es un rastreador minimalista de "cuándo hiciste algo por última vez" con soporte para recordatorios automáticos, diseñado para rastrear hábitos, tareas de mantenimiento, chequeos de salud, o cualquier cosa que necesite ser hecha regularmente.

A diferencia de Habitica (gamificado, complejo) o Simple Habit Tracker (limitado), LastGLANCE es **local-first, sin cuentas, sin suscripción, y completamente autodesplegable**. Parte de la suite GLANCE del mismo desarrollador que creó dayGLANCE y lifeGLANCE.

- 🔒 **Local-first**: todos los datos en tu navegador (IndexedDB)
- 🚫 **Zero tracking, zero accounts**
- 📱 **PWA + Android nativo** (funciona offline)
- 📊 **Historiales con timestamps, streaks y estadísticas**
- 🔔 **Recordatorios opcionales automáticos**
- 📁 **Importar/Exportar JSON** para backup y migración

## ✨ Características principales

- **Rastrear "última vez"**: Anota cuándo hiciste algo con timestamp automático
- **Recordatorios automáticos**: Configura frecuencia y recibe notificaciones en tiempo
- **Streaks e histórico**: Ve tu racha de días/semanas, histórico completo y timeline
- **Categorías**: Organiza tareas en categorías (Salud, Hogar, Trabajo, etc.)
- **Local-first**: Datos en tu navegador, sin servidor, sin tracking
- **PWA + Mobile**: App web + Android nativo, funciona offline
- **Estadísticas**: Cuántas veces completado, promedio de días entre, tendencias
- **Importar/Exportar JSON**: Backup, restore y migración entre dispositivos
- **API simple**: Para integraciones personalizadas
- **Privacidad por defecto**: Sin cuentas, sin cloud, solo tus datos

## 📋 Requisitos del sistema

- Docker instalado
- 256-512 MB RAM (muy ligero)
- 100 MB - 1 GB espacio en disco
- Puerto 80 (contenedor) o personalizado
- Node.js base (Alpine Linux)
- Navegador moderno con soporte IndexedDB
- **Ideal para**: Raspberry Pi, NAS, VPS básico, cualquier servidor. Extremadamente ligero.

## 🐳 Instalación

### Opción 1: Instalación rápida con Docker Compose

```bash
cat > docker-compose.yml << 'EOF'
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
EOF
```

### Iniciar el contenedor

```bash
docker compose up -d
```

### Acceder a la aplicación

```
http://localhost:3000
```

## ⚙️ Configuración

1. **Puerto externo**: Modifica `"3000:80"` en `docker-compose.yml` si necesitas otro puerto (ej: `"8080:80"`)
2. **Healthcheck**: Verifica que el servicio responde en el puerto 80 interno cada 10 segundos
3. **Reinicio automático**: `unless-stopped` asegura que el contenedor se inicie tras reinicios del host
4. **Imagen**: Usa `ghcr.io/krelltunez/lastglance:latest` para siempre tener la versión más reciente
5. **Sin volúmenes necesarios**: Los datos se almacenan en el navegador (IndexedDB), no en el contenedor

## 🚀 Primeros pasos

1. **Acceder a la app**
   - Abre `http://localhost:3000` en tu navegador
   - No necesita login ni registro
   - Los datos se guardan localmente en tu navegador (IndexedDB)
   - Interfaz limpia y simple

2. **Crear primer ítem a rastrear**
   - Click en "+" o "New Item"
   - Nombre: "Limpiar baño", "Cambiar aceite coche", "Cortar cabello", etc.
   - Categoría (opcional): Hogar, Salud, Mantenimiento, Trabajo
   - Frecuencia ideal (opcional): "cada 3 días", "semanal", "mensual"
   - Guardar

3. **Registrar que lo hiciste**
   - Click en el ítem de la lista
   - Button "Mark Done" o "I Did This"
   - Se registra timestamp automáticamente
   - Se reinicia el contador

4. **Ver histórico y streaks**
   - Cada ítem muestra: "Última vez: X días atrás"
   - Timeline de todas las veces que lo hiciste
   - Racha actual: cuántos días seguidos (si aplica)
   - Promedio: cada cuántos días lo haces

5. **Configurar recordatorios (opcional)**
   - Click en ítem → Editar
   - Habilitar "Reminder"
   - Frecuencia: cada 1 semana, 2 semanas, 1 mes, etc.
   - La app te notifica cuando debes hacerlo

6. **Usar en móvil**
   - PWA: "Add to homescreen" en iOS/Android
   - O descarga Android app desde GitHub Releases
   - Mismo acceso a datos, funciona offline

## 💡 Casos de uso

- **Mantenimiento del hogar**: Limpiar, reparar, cambios de aire, refrigeración
- **Salud personal**: Chequeos médicos, dentista, cortes de cabello, manicura
- **Automóvil**: Cambio aceite, rotación llantas, inspección, lavado
- **Trabajo**: Reuniones periódicas, reportes, backups, auditorías
- **Hábitos**: Ejercicio, meditación, lectura (aunque dayGLANCE es mejor para esto)
- **Mascotas**: Veterinario, baño, grooming, vacunas
- **Suscripciones/Renovaciones**: Dominio, hosting, seguros, membresías

## 🔒 Acceso remoto seguro (producción)

### HTTPS con Caddy

```caddyfile
track.tudominio.com {
    reverse_proxy localhost:3000
}
```

Acceso remoto en `https://track.tudominio.com` con HTTPS automático.

### Habilitar WebSocket (recomendado)

```caddyfile
track.tudominio.com {
    reverse_proxy localhost:3000 {
        header_up Upgrade websocket
        header_up Connection "upgrade"
    }
}
```

## 🛠️ Gestión y mantenimiento

### Ver logs en tiempo real

```bash
docker logs -f lastglance
```

### Backup de datos (local - en navegador)

1. Settings → **Export JSON**
2. Descarga tus datos como archivo JSON

### Restore de datos

1. Settings → **Import JSON**
2. Carga tu backup anterior

### Reiniciar el servicio

```bash
docker compose restart lastglance
```

### Actualizar a la última versión

```bash
docker compose pull
docker compose up -d
```

### Ver estado de salud

```bash
docker ps
# Status mostrará "healthy" si todo está OK
```

### Limpiar datos locales (en navegador)

DevTools → Application → Storage → Clear Site Data

## 📝 Licencia

Este proyecto usa la imagen oficial de **LastGLANCE** desarrollada por [Krelltunez](https://github.com/krelltunez), licenciada bajo **MIT License**.

---

> 📖 **Guía completa**: [Cómo instalar LastGLANCE en Docker - Rastreador de hábitos y tareas recurrentes autohospedado en Docker](https://genbyte.blogspot.com/2026/06/como-instalar-lastglance-en-docker.html)