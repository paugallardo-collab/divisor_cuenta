# Evidencias de CI/CD

Estas evidencias corresponden a ejecuciones y consultas reales del 26 de octubre de 2026. Las capturas de Actions se tomaron de las páginas públicas de GitHub con Chrome. Las imágenes que reproducen API/CLI indican expresamente esa procedencia y no simulan la interfaz de GitHub.

| Archivo | Qué acredita | Procedencia |
| --- | --- | --- |
| `parte1_ci_verde.png` | Primer workflow aprobado | Captura de GitHub Actions |
| `parte1_run.json`, `parte1_log.txt` | Pasos y salida del primer CI | GitHub CLI |
| `parte2_proteccion.png` | Regla de main guardada y aplicada al administrador | Render de respuesta auténtica de REST API; no captura de Settings |
| `parte2_proteccion.json` | Check verificar obligatorio, PR obligatorio, sin force push | REST API autenticada de GitHub |
| `parte3_rojo.png` | Run que falló por la expectativa 27.00 | Captura de GitHub Actions |
| `parte3_log.png` | Esperado 27.0, obtenido 27.5 y línea 37:7 | Render de fragmento auténtico del log obtenido por CLI |
| `parte3_rojo_log.txt`, `parte3_rojo_run.json` | Salida completa del paso fallido y estado del run | GitHub CLI |
| `parte3_merge_bloqueado.png` | Estado BLOCKED y rechazo efectivo del merge | Render de respuestas auténticas de API/CLI; no captura del botón |
| `parte3_merge_bloqueado.json`, `parte3_merge_rechazado.txt` | Estado y rechazo originales antes de restaurar la prueba | GitHub API/CLI |
| `parte3_verde_otra_vez.png` | Mismo PR aprobado después de corregir la expectativa | Captura de GitHub Actions |
| `parte3_verde_run.json`, `parte3_merge_habilitado.json` | Check SUCCESS y estado CLEAN antes del merge | GitHub API/CLI |
| `parte4_pr_run.json` | Verificación del cambio que agrega entrega | GitHub CLI |
| `parte4_artifact.png` | Artifact del APK visible tras CI y entrega | Captura de GitHub Actions |
| `parte4_run.json`, `parte4_artifacts.json` | Estado de ambos jobs, nombre, tamaño y vigencia del artifact | GitHub API/CLI |
| `parte4_apk_sha256.txt` | Nombre, tamaño y huella del APK descargado | Comprobación local del artifact descargado |
| `acciones_verificadas.json` | SHA y runtime de las acciones JavaScript actualizadas | Metadatos originales de sus repositorios |

Faltan las capturas de la interfaz autenticada de Settings y del botón bloqueado. Los datos de esas operaciones sí se conservaron antes de corregir el PR. Consulta `../PENDIENTES_CAPTURAS.md` para añadir las imágenes de interfaz sin presentar las transcripciones como si lo fueran.

Las otras evidencias SDD, vibe y de arquitectura de esta carpeta pertenecen a la actividad previa y se conservaron.
