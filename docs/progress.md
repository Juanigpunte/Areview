# Avance

Última actualización: 2026-11-25. Estado: **en pausa, antes del primer hito**. No hay funcionalidad todavía, solo el esqueleto.

## Objetivo
App open source de soporte remoto para Windows, liviana y rápida: controlar una PC y transferir archivos. El ejecutable se le pasa a clientes para darles soporte, con sesiones largas.

## Decidido
- Dos ejecutables: cliente (PC asistida) y técnico (visor).
- v1 solo asistida: el cliente acepta cada sesión y ve un aviso mientras dura.
- Licencia: MIT o Apache-2.0. Repo privado por ahora.
- Workflow: agent-workflow-kit 0.3.0; parámetros en `AGENTS.md`. Mergea el orquestador; se frena tras 3 revisiones fallidas; `/security-review` en ramas sensibles.

## Propuesto (sin implementar)
- Stack: Rust + `windows-rs`, captura DXGI Desktop Duplication, H.264 con Media Foundation, visor D3D11/DXVA, red QUIC con `iroh`, UI Win32.
- Archivos por un stream aparte, con reanudación y hash BLAKE3.
- Red por etapas: hito local sin servidor; pruebas internas con los relays públicos de iroh; clientes con servidor propio (relay + ID de 9 dígitos) o el plan pago de n0. Relay configurable y búsqueda por ID separada desde el inicio.
- Servicio de Windows para ver los avisos de UAC; firma digital del instalador; reconexión automática.
- Objetivos estimados, a medir: exe < 12 MB, host < 60 MB de RAM, < 50 ms en LAN a 1080p60.

## Hecho
- Toolchain local: Rust 1.100, MSVC Build Tools y SDK de Windows.
- Workspace `proto`, `client`, `technician` y CI en Windows. Sin tests todavía.
- Kit instalado y `workflow-init` aplicado (PR #1).

## Pendiente
- Decidir el servidor de producción: VM Linux propia con IP pública y subdominio, VPS o plan pago. No frena el primer hito.
- Confirmar los puertos UDP de `iroh-relay` contra su código.
- Decidir la firma digital.
- Actualizar `actions/checkout@v4` (aviso de Node 20 deprecado).

## Para retomar
1. Reiniciar Claude Code en esta carpeta, para tener `cargo` en el PATH y el plugin cargado.
2. Liberar memoria: los pasos pesados del gate esperan 4 GB libres.
3. Lanzar el primer hito:
   ```
   /agent-workflow-kit:orchestrate feat/video-pipeline Capturar la pantalla con DXGI, codificar H.264 con Media Foundation, decodificar y mostrar en una ventana local, midiendo latencia, CPU y RAM
   ```
