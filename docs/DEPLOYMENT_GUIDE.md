# GUÍA DE DESPLIEGUE — ALihas-v2

**Versión:** 1.0
**Fecha:** 2026-03-30
**Propósito:** Documentar errores encontrados durante el despliegue en Codespace y pasos para replicar en Dokploy.

---

## 1. Errores Encontrados y Soluciones

### 1.1 Telegram Webhook Conflict (Error 409)
**Error:** `Call to 'getUpdates' failed! (409: Conflict: can't use getUpdates method while webhook is active)`

**Causa:** El bot de Telegram tenía un webhook activo de la instancia v1 (Dokploy). Cuando v2 intentó usar polling (getUpdates), Telegram rechazó la solicitud porque ambos métodos no pueden coexistir.

**Solución:**
```bash
# Ejecutar desde dentro del Codespace o cualquier terminal con acceso:
curl -s "https://api.telegram.org/bot<TELEGRAM_BOT_TOKEN>/deleteWebhook"
# Respuesta esperada: {"ok":true,"result":true,"description":"Webhook was deleted"}
```

**Prevención para Dokploy:** Al desplegar en Dokploy, decidir de antemano si usarás:
- **Polling** (getUpdates): No configurar webhook. Ejecutar deleteWebhook primero.
- **Webhook** (recomendado para producción): Configurar la URL del webhook con la URL de Dokploy:
```bash
curl -s "https://api.telegram.org/bot<TOKEN>/setWebhook?url=https://joseias.alianed.com/telegram/webhook"
```

---

### 1.2 Puerto 8080 No Accesible Externamente
**Error:** Dashboard de OpenClaw cargaba localmente pero no era accesible desde fuera del Codespace.

**Causa:** Por defecto, los puertos en GitHub Codespaces son privados.

**Solución:** Hacer el puerto 8080 público:
1. En el Codespace, ir a la pestaña "Ports"
2. Click derecho en el puerto 8080
3. Seleccionar "Port Visibility" -> "Public"

**Para Dokploy:** Esto no aplica. Dokploy/Traefik manejan el routing automáticamente con el dominio configurado (joseias.alianed.com).

---

### 1.3 Variables de Entorno Faltantes
**Error:** Los contenedores no arrancaban correctamente o fallaban al conectar con NVIDIA NIM.

**Causa:** Variables de entorno no configuradas en .env

**Solución:** Copiar .env.example a .env y configurar:
```bash
cp .env.example .env
# Editar con los valores reales:
# NVIDIA_API_KEY=nvapi-xxx
# TELEGRAM_BOT_TOKEN=xxx
# OPENCLAW_SECRET=xxx
```

**Para Dokploy:** Configurar las mismas variables en el panel de Dokploy -> Variables de Entorno del servicio.

---

## 2. Pasos para Desplegar en Dokploy (Producción)

### Pre-requisitos
1. Servidor Oracle A operativo (159.54.142.68)
2. Dokploy instalado y accesible (https://mydokploy.alianed.com)
3. Dominio joseias.alianed.com apuntando al servidor
4. Variables de entorno listas

### Paso a Paso
```
1. Acceder a Dokploy -> Crear nuevo servicio tipo Docker Compose
2. Conectar al repositorio: angelarismendyroa-lgtm/ALihas-v2
3. Branch: main
4. Configurar variables de entorno (copiar desde .env.example):
   - NVIDIA_API_KEY
   - TELEGRAM_BOT_TOKEN
   - OPENCLAW_SECRET
   - MODEL=moonshotai/kimi-k2-thinking
   - SAFETY_MODEL=nvidia/llama-3.1-nemotron-safety-guard-8b-v3
5. Configurar dominio: joseias.alianed.com -> puerto 8080
6. Activar HTTPS (Let's Encrypt / Traefik)
7. Deploy
8. Post-deploy: configurar webhook de Telegram:
   curl "https://api.telegram.org/bot<TOKEN>/setWebhook?url=https://joseias.alianed.com/telegram/webhook"
9. Verificar health: curl https://joseias.alianed.com/health
```

### Redes Docker en Dokploy
- **dokploy-network**: Red principal donde Traefik hace el routing
- **core-agentes-net**: Red interna entre los servicios de ALihas
- Los servicios deben estar en AMBAS redes para funcionar correctamente

---

## 3. Conexión con Memorias (Neurogestor/Engram)

### Estado Actual
El Neurogestor (Engram) NO está incluido en el docker-compose.yml de v2 todavía.

### Cómo debe conectarse:
1. Agregar servicio Engram al docker-compose.yml:
```yaml
  engram:
    image: engram/engram:latest
    ports:
      - "7437:7437"
    volumes:
      - engram_data:/data
    networks:
      - core-agentes-net
```

2. Configurar OpenClaw para usar el Neurogestor:
```yaml
  environment:
    - NEUROGESTOR_URL=http://engram:7437
```

3. Permisos de escritura:
   - Nivel 0 (Joseias): Solo datos personales y preferencias
   - Nivel 1+ (Motor): Reportes AMP completos y estado de tareas
   - Todos los agentes: Lectura

### Volumen persistente
En Dokploy, asegurarse de que el volumen `engram_data` sea persistente y esté en el disco del servidor (no efímero).

---

## 4. Checklist Pre-Producción
- [ ] Variables de entorno configuradas en Dokploy
- [ ] deleteWebhook ejecutado (si venías de polling)
- [ ] setWebhook configurado con URL de Dokploy
- [ ] Puerto 8080 mapeado al dominio
- [ ] HTTPS activo
- [ ] Redes Docker (dokploy-network + core-agentes-net) configuradas
- [ ] Engram/Neurogestor desplegado y conectado
- [ ] Health check respondiendo OK
- [ ] NemoClaw Safety Guard activo
- [ ] openclaw-sandbox.yaml montado correctamente

---

## 5. Troubleshooting Comunes

| Síntoma | Causa Probable | Solución |
|---------|---------------|----------|
| 409 Telegram | Webhook activo + polling | deleteWebhook |
| Dashboard no carga | Puerto no público/mapeado | Verificar port binding |
| Bot no responde | Token inválido o variable vacía | Verificar .env |
| NVIDIA API error | API key inválida | Regenerar en build.nvidia.com |
| Safety Guard falla | Modelo no accesible | Verificar NVIDIA_API_KEY |
| Contenedor reinicia | Falta variable crítica | docker compose logs |

---
*ALihas-v2 — Guía de Despliegue v1.0*
