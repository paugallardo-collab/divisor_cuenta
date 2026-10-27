# Cuenta Clara · CI/CD para Flutter

[![CI](https://github.com/paugallardo-collab/divisor_cuenta/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/paugallardo-collab/divisor_cuenta/actions/workflows/ci.yml)

Divisor de cuentas de una pantalla, sin conexión, con propina y dos modos de redondeo. Se conserva la app de las Semanas 7 y 8 y se automatiza su verificación y la entrega de un APK de depuración.

## Ejecutar y verificar

Desde la raíz de este repositorio, donde está `pubspec.yaml`:

```sh
flutter pub get
flutter run -d chrome
flutter analyze
flutter test --reporter expanded
flutter build apk --debug
```

Las 28 pruebas cubren escenarios de reparto, validación, redondeo, sustitución de estrategias y comportamiento de la pantalla. El APK local queda en `build/app/outputs/flutter-apk/app-debug.apk`.

## Automatización

Cada PR y push a `main` ejecuta, en orden, instalación de dependencias, análisis, pruebas y compilación del APK sobre `ubuntu-latest`, Java 17 y Flutter estable.

Después de pasar CI en un push a `main`, el job `entrega` recompila el APK usando la misma versión de Flutter y lo sube como artifact por 30 días. No se publica en producción. Se descarga desde la sección **Artifacts** del [run de entrega](https://github.com/paugallardo-collab/divisor_cuenta/actions/runs/40398847962).

`main` exige un PR y el check `verificar` aprobado, con la rama al día. La regla aplica también al administrador. El [PR #2](https://github.com/paugallardo-collab/divisor_cuenta/pull/2) conserva el fallo intencional y su corrección; ambos estados tienen evidencia. El [PR #4](https://github.com/paugallardo-collab/divisor_cuenta/pull/4) es auxiliar, se deja sin fusionar para capturar el bloqueo.

## Entrega escrita

- [Siete respuestas y workflow completo](respuestas.md).
- [Revisión crítica del workflow](revision-ci.md).
- [Evidencias y su procedencia](evidencias/README.md).
- [Dos capturas de interfaz autenticada pendientes](PENDIENTES_CAPTURAS.md).
- [Respuestas anteriores de SDD conservadas](docs/respuestas-sdd.md).
- [Guía de la aplicación](GUIA_APP.md).

Las capturas de Actions son de GitHub. Las imágenes de configuración y rechazo de merge obtenidas por API/CLI están identificadas como transcripciones; no se presentan como capturas de Settings o del botón.

## Arquitectura y antecedentes

Dependencias: `presentation -> domain <- data`. El dominio es Dart puro; `main.dart` compone las estrategias. No hay base de datos ni dependencias externas de ejecución.

La rama `vibe` conserva la implementación directa y `sdd` la implementación por capas. La [especificación y los seis escenarios](specs/001-divisor-cuenta/spec.md), la [constitución SOLID](.specify/memory/constitution.md), el plan y las tareas se conservan. El SDK local verificado fue Flutter 3.47.1 y Dart 3.13.1; CI selecciona el canal estable disponible en cada run.
