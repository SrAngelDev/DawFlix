# 🍿 DawFlix

### Implementación de un Servicio SaaS de Streaming Multimedia

*(Sustituye este link por tu logo si lo subes a algún sitio)*

## 📋 Descripción del Proyecto

**DAWflix** es una plataforma de **Video bajo Demanda (VoD)** implementada como un servicio **SaaS (Software as a Service)**.

Este proyecto demuestra la capacidad de desplegar, administrar y ofrecer una aplicación compleja a usuarios finales utilizando tecnologías de contenedorización modernas. Los usuarios pueden acceder al contenido desde cualquier dispositivo (Móvil, SmartTV, PC) a través del navegador, sin instalar software adicional.

* **Fecha de presentación:** Viernes 9 de Enero, 2026.
* **Modelo de Servicio:** SaaS (Software as a Service).
* **Modelo de Despliegue:** On-Premise (Simulado en IaaS virtualizado).

---

## 🛠️ Stack Tecnológico

* **Motor Principal:** [Jellyfin](https://jellyfin.org/) (Open Source Media System).
* **Contenedorización:** Docker y Docker Compose V2.
* **Sistema Operativo Base:** Ubuntu (Linux).
* **Red:** Configuración en modo **Bridge** (Puente) para acceso LAN.
* **Gestión de Medios:** `yt-dlp` para la ingesta de contenido desde fuentes externas.

---

## 🚀 Instalación y Despliegue

### 1. Prerrequisitos

* Máquina Virtual o Servidor con Ubuntu/Debian.
* **Importante:** La interfaz de red debe estar en **Modo Puente (Bridged Adapter)** para permitir conexiones externas.
* Docker y Docker Compose instalados.

### 2. Estructura de Directorios

El servicio utiliza volúmenes persistentes para no perder la configuración ni los datos.

```bash
mkdir -p ~/dawflix/{config,cache,media}
cd ~/dawflix

```

### 3. Definición del Servicio (docker-compose.yml)

Archivo de orquestación para levantar el servicio:

```yaml
services:
  jellyfin:
    image: jellyfin/jellyfin:latest
    container_name: dawflix_server
    network_mode: "host" # Usa la IP directa de la máquina para mejor descubrimiento DLNA
    volumes:
      - ./config:/config
      - ./cache:/cache
      - ./media:/media
    restart: "unless-stopped"
    environment:
      - JELLYFIN_PublishedServerUrl=http://192.168.1.X # Opcional

```

### 4. Ingesta de Contenido (Ejemplos)

Para poblar el catálogo, utilizamos herramientas de descarga directa en el servidor o generamos archivos dummy para pruebas de carga.

**Opción A: Descarga real con yt-dlp**

```bash
# Descargar trailer en la carpeta de medios
cd ~/dawflix/media
yt-dlp -f mp4 -o "%(title)s.%(ext)s" "URL_DEL_VIDEO"

```

**Opción B: Generación de catálogo (Archivos para rellenar)**

```bash
touch "Matrix (1999).mp4" "Avatar (2009).mp4"

```

### 5. Arranque

```bash
docker compose up -d

```

---

## 💻 Guía de Uso (Usuario Final)

1. **Acceso:**
Abre tu navegador favorito (Chrome, Firefox, Safari) y navega a:
`http://<IP-DEL-SERVIDOR>:8096`
*(Ejemplo: [http://192.168.10.103:8096](https://www.google.com/search?q=http://192.168.10.103:8096))*
2. **Credenciales de Admin:**
* **Usuario:** `angel`
* **Contraseña:** `160722`


3. **Funcionalidades:**
* Reproducción en streaming adaptativo (HLS).
* Transcodificación en tiempo real (si la red lo requiere).
* Metadatos automáticos (Carátulas, Sinopsis, Actores).

---

## 👥 Autores

**Grupo DAWflix**

* Angel, Jorge y Cristian

---

*Proyecto educativo realizado para el ciclo de Desarrollo de Aplicaciones Web en la asignatura de Digitalización.*
