# ALihas-v2 (NemoClaw Architect Edition)

Este repositorio unifica la agilidad del framework de automatización **ALihas** con el núcleo de seguridad restrictivo de **NVIDIA NemoClaw**.

## Arquitectura de Motor Dual NVIDIA

| Capa | Modelo | Propósito |
|---|---|---|
| 🛡️ Seguridad | `llama-3.1-nemotron-safety-guard-8b-v3` | Filtro de contenido NemoClaw |
| 🧠 Operativo | `meta/llama-3.1-70b-instruct` | Razonamiento y ejecución de tareas |
| 🔀 Agentes | OpenRouter (multi-modelo) | Agentes especializados (auditor, marketing) |

## Estructura del Proyecto

```
ALihas/
├── .devcontainer/          # Configuración para GitHub Codespace
│   ├── devcontainer.json   # Docker-in-Docker + autostart
│   └── post-create.sh      # Crea redes y levanta contenedores
├── .agents/                # Skills del agente IA
├── identity/               # Documentos de identidad del agente
│   ├── system-prompt.md    # Personalidad, capacidades, restricciones
│   ├── agent-config.json   # Configuración estructurada (modelos, empresa)
│   └── safety-rules.md     # Reglas de seguridad derivadas de NemoClaw
├── policies/               # Firewall egress estricto
│   └── openclaw-sandbox.yaml
├── docker-compose.yml      # Orquestación (Dokploy + Codespace compatible)
├── .env.example            # Template de variables
└── .env                    # Claves reales (NO se sube a GitHub)
```

## Guía de Despliegue

### Opción A: GitHub Codespace (Desarrollo/Testing)

1. Abre un **Codespace** desde este repositorio.
2. El `devcontainer.json` ejecutará automáticamente `post-create.sh`:
   - Crea las redes Docker necesarias.
   - Copia `.env.example` a `.env`.
3. Edita `.env` con tus claves API reales (mínimo: `NVIDIA_API_KEY`).
4. Ejecuta `docker compose up -d`.
5. Accede al Dashboard en el puerto **8080**.

### Opción B: Dokploy (Producción)

1. Añade este repositorio a tu panel de Dokploy seleccionando la opción **Compose**.
2. Vincula el **Git Repository** a la URL de GitHub.
3. Ve a la pestaña de **Environment** en Dokploy.
4. Copia el contenido de `.env.example` y **rellena con tus verdaderas claves API**.
5. Realiza un **Deploy**.

Dokploy montará automáticamente los volúmenes, leerá la regla "strict" desde las políticas e instanciará los contenedores blindados.

## Seguridad NemoClaw

- **Modo**: `strict` — Todo tráfico bloqueado por defecto.
- **Whitelist**: Solo proveedores LLM aprobados + Telegram + Neurogestor interno.
- **Política**: `policies/openclaw-sandbox.yaml`
- Bajo ninguna circunstancia se debe modificar o borrar el volumen `joseias-data` sin aprobación del Arquitecto.
