# INFRA.md — Infraestructura ALIHA (Referencia Legacy v1 + Estado v2)

> Este documento preserva la referencia del estado v1 (Dokploy/Oracle) y documenta la arquitectura v2 (Codespace).

---

## Servidores v1 (Legacy — Producción)

### Oracle A (Producción — Joseias Core)
* Host: 159.54.142.68
* Usuario SSH: ubuntu
* Hostname: mayordomia
* OS: Ubuntu 22.04 LTS (aarch64)
* RAM: 12GB / 100GB disco
* Orquestador: Dokploy
* Panel: https://mydokploy.alianed.com

### Oracle B (Pendiente configurar)
* Rol: Servidor de workspaces para clientes

---

## Infraestructura v2 (Actual)

### GitHub Codespaces
* Entorno: .devcontainer/devcontainer.json
* Stack: Docker Compose (OpenClaw + NemoClaw)
* Auto-provisioning: post-create.sh
* Dashboard: puerto 8080 (público)

### Dokploy (Producción futura)
* Se desplegará desde la configuración validada en Codespace

---

## Agentes v2

### Joseias — Interfaz Humana (ACTIVO)
* Motor: NVIDIA NIM (Kimi-K2-thinking)
* Seguridad: NemoClaw Safety Guard
* Canal: Telegram @Alianedbot + Web Dashboard

---

## Redes Docker
* dokploy-network
* core-agentes-net

## Neurogestor (Memoria del Sistema)
* Motor: Engram (SQLite)
* Endpoint interno: http://aliha-neurogestor:7437
* Lectura: todos los agentes
* Escritura Nivel 1+: Motor principal
* Escritura Nivel 0: Joseias

## Estado del Sistema v2
### Activo:
* [x] Joseias (OpenClaw) en Codespace
* [x] NVIDIA NIM como motor
* [x] NemoClaw Safety Guard
* [x] Telegram webhook limpiado
* [x] Dashboard accesible (puerto 8080 público)

### Pendiente:
* [ ] Migrar a Dokploy (producción)
* [ ] Activar Agente Watchdog Local (aprox. 5B)
* [ ] Sub-agentes vía OpenRouter
* [ ] Conectar Neurogestor/Engram

---

## Principio Fundamental
La infraestructura NO es solo servidores. Es el soporte físico de un sistema cognitivo multi-agente.
Todo cambio aquí impacta el comportamiento del sistema completo.
