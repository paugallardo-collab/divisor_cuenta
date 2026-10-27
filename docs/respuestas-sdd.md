# Respuestas · Participación Semana 7

Fecha: 2026-09-30. App: Cuenta Clara (divisor de cuenta). Entrega escrita en `main`.

> Alcance real: esta es una ejecución asistida en una única sesión. El agente leyó la guía y
> el PPT antes de implementar las dos ramas. Por eso `vibe` es una implementación directa,
> **no** una ejecución aislada de una sola frase sin contexto. No se inventan conversaciones,
> tiempos, preguntas del estudiante ni pruebas manuales. La comparación permite observar
> estructura/testabilidad, pero no atribuir causalmente diferencias al método.

## 1. Métricas, configuración y resultados

| Métrica | vibe | sdd |
|---|---|---|
| Iteraciones: mensajes adicionales del estudiante | 0 | 0 |
| Casos de aceptación que cumple | 6/6, pruebas de widgets | 6/6, dominio y widgets |
| Pruebas automatizadas propias que pasan | 6 | 28 |
| Archivos Dart en lib/ | 1 | 11 |
| Líneas en lib/ incluyendo blancos/comentarios | 155 | 414 |
| ¿domain depende de Flutter? | No hay domain; toda la lógica está en un archivo que importa Flutter | No; además corre con Dart puro |
| ¿Separación presentation/domain/data? | No | Sí |
| ¿El agente agregó decisiones no expresas? | Sí: coma decimal, limpieza de resultados y validaciones adicionales | Sí: nombre/estética y límites de entrada, declarados en spec |
| ¿Otra estrategia sin cambiar el cálculo existente? | No; hay una condición dentro de _calcular | Sí; nueva implementación y registro en main |
| flutter analyze | Sin observaciones | Sin observaciones |
| APK debug | No medido; la guía exige el APK SDD | Compilado |

**Agente y configuración:** Codex, familia GPT-6 según el entorno de la sesión. El identificador
exacto del modelo y nivel de razonamiento seleccionados en la interfaz no están expuestos a
esta ejecución; no se atribuye un valor inventado. Se usó la misma sesión/configuración en
ambas ramas, sin cambio de modelo ni subagentes. Flutter 3.47.1, Dart 3.13.1, Spec Kit 1.0.13.
Archivo del agente: `AGENTS.md`, solo en SDD (también presente en main tras integrarlo).

**Git:** caso C: la carpeta padre ya era un repositorio. Se creó uno independiente en
Participacion1oct y se agregó `/Participacion1oct/` al .gitignore padre. Las ramas comparten
el commit inicial `934956ac2dba4631651592569c3893c7b0c2641b`, comprobado con
`git merge-base vibe sdd`. Main integra SDD y estas respuestas; vibe se conserva independiente.

Ambas versiones cumplen los seis ejemplos: hubo empate funcional en esta ejecución. SDD
explicita fórmula, límites, mensajes, contrato de estrategia, composición y pruebas antes
del código. En vibe esas decisiones están en el código y fueron completadas por el agente,
con conocimiento previo de la guía: no corresponde afirmar que descubrió todo desde una frase.

Las correcciones autónomas (incluida una del encabezado con texto grande y observaciones del
analizador) no cuentan como iteraciones según la regla de la actividad. El cero no implica
que no hubiera correcciones, ni prueba mayor eficiencia de un método. No se midieron tiempos.

## 2. Trasladar las pruebas SDD a vibe

Se realizó el traslado temporal completo de test/ desde sdd, se ejecutó flutter test y se
restauró test/ exactamente al HEAD de vibe. El comando terminó con código 1. Primer error:

```text
test/division_test.dart:2:8: Error: Error when reading 'lib/domain/cuenta.dart': The system cannot find the path specified
```

La rama vibe no tiene Cuenta, CalcularDivision, ValidarEntrada ni sus módulos. También difiere
el punto de creación de la UI. Esto demuestra incompatibilidad de contratos/testabilidad,
no que la operación de reparto dé una respuesta incorrecta. No es válido contar "0/6"
funcionales a partir de una suite que ni siquiera compila.

Para reutilizar esos tests habría que extraer validación/cálculo/estrategias a módulos con
esos contratos, o adaptar las pruebas a la API de vibe. No se refactorizó vibe para maquillar
el resultado. Se restauraron sus pruebas de widgets y las seis pasaron:

| Caso | Esperado | vibe automatizado | sdd automatizado | Revisión manual |
|---|---|---|---|---|
| 100 / 4, 10%, exacto | 27.50 | Pasa | Pasa | Pendiente |
| 90 / 3, 0%, exacto | 30.00 | Pasa | Pasa | Pendiente |
| 50 / 0 | Debe haber al menos una persona; sin resultado | Pasa | Pasa | Pendiente |
| abc como monto | Monto inválido; sin resultado | Pasa | Pasa | Pendiente |
| 10 / 3, exacto | 3.33 | Pasa | Pasa | Pendiente |
| 10 / 3, hacia arriba | 4.00 | Pasa | Pasa | Pendiente |

Se intentó abrir la app servida localmente, pero la herramienta de control no tenía navegadores
conectados. No se afirma haber ejecutado manualmente los casos. GUIA_APP.md incluye el procedimiento
para que el estudiante complete ese paso solicitado por la guía.

Evidencia: [fallo de traslado](../evidencias/vibe-pruebas-sdd.txt),
[tests vibe restaurados](../evidencias/vibe-test.txt), [tests SDD](../evidencias/sdd-test.txt).

## 3. Verificaciones SOLID

| Regla de la constitución | sdd | vibe |
|---|---|---|
| I. SRP | Cálculo, validación y formato separados | _calcular valida, calcula y formatea en el State |
| II. OCP | Nueva estrategia sin editar el cálculo; prueba con múltiplos de 5 | Hay que editar la condición del cálculo |
| III. LSP | Mismo CalcularDivision acepta ambas implementaciones; test pasa | No existe interfaz intercambiable; no demostrable con esa prueba |
| IV. ISP | Un solo método aplicar en la interfaz | No existe contrato de estrategias; no aplicable como prueba de interfaz |
| V. DIP y capas | presentation -> domain <- data; main compone | Widget contiene directamente la lógica |

Salida real resumida de `tool/verificar_arquitectura.ps1`:

```text
OK: tres capas presentes; domain y data libres de Flutter.
OK: presentation depende de domain, no de data.
OK: las dos estrategias se instancian solo en lib/main.dart.
OK: CalcularDivision no valida, formatea ni inspecciona tipos.
```

Evidencia de vibe con git grep:

```text
vibe:lib/main.dart:1:import 'package:flutter/material.dart';
vibe:lib/main.dart:42:      if (monto == null || !monto.isFinite || monto < 0) {
vibe:lib/main.dart:54:        _resultado = (_arriba ? valor.ceilToDouble() : valor).toStringAsFixed(
```

`git ls-tree -r --name-only vibe lib` solo devuelve `lib/main.dart`.
Una búsqueda sin coincidencias en un directorio inexistente NO demostraría DIP ni SRP:
primero se comprobó la existencia de las capas. El script estático tampoco demuestra todo
SOLID por sí solo; se complementó con inspección y pruebas LSP/OCP.
Las instancias concretas en tests y tool son fixtures de verificación; la regla de composición
se aplica a lib/ de producción, como se aclaró en la constitución.

## 4. Clarify

No se hicieron preguntas relevantes al estudiante. No se inventan dos preguntas para llenar
la respuesta. La especificación ya daba una pantalla, tres entradas, fórmula, dos modos,
mensajes exactos para errores, seis ejemplos y ausencia de red/base de datos.

La revisión sí dejó explícitos supuestos: aceptar coma o punto, limpiar resultados al editar,
no redistribuir centavos y limitar rangos. Fueron decisiones del agente, no respuestas del
estudiante. En vibe quedaron principalmente en el código; en SDD se registraron en spec.md
antes de programar. Hay diferencias fuera de los seis casos: vibe no limita propina al 100%,
mientras SDD sí lo declara y valida; no se oculta esa diferencia de alcance.

Las habilidades instaladas para Codex se llaman `$speckit-clarify`, `$speckit-plan`, etc.
Se leyeron y aplicaron en esta sesión sus procedimientos y scripts; escribir esos nombres en
PowerShell no ejecuta un modelo. No se lanzó otra conversación facturada para cada paso.

## 5. Diferencias y mantenibilidad

`git diff vibe sdd --stat`: **71 archivos, 6473 inserciones y 271 eliminaciones**.
La salida completa está en [comparacion-diff.txt](../evidencias/comparacion-diff.txt).
El volumen incluye las plantillas, scripts y habilidades oficiales de Spec Kit: no son
6473 líneas de funcionalidad adicional. La comparación del código de app es 155 frente a 414 líneas.

No se agregaron login, red, base de datos, historial ni otras pantallas. Sí se eligieron detalles
no pedidos literalmente: nombre Cuenta Clara, colores, aceptación de coma y validaciones de
bordes. En SDD los límites están documentados como supuestos; en vibe no están todos. La
tercera estrategia existe solo en un test de extensibilidad, no en la interfaz de producción.

En dos semanas resulta más fácil retomar SDD leyendo spec, plan y tareas. A un compañero
le entregaría README, GUIA_APP, constitution, spec y pruebas. Para redondear a múltiplos de 5,
crearía una implementación en data y la registraría en main, sin tocar CalcularDivision.
En vibe tendría que editar la condición del método de la pantalla que calcula.
Esto favorece mantenibilidad en este diseño concreto, con el coste de más archivos y documentación.

## 6. Otra herramienta y cuándo elegir vibe

Elegí **OpenSpec**. Su documentación organiza cada cambio en una carpeta con propuesta,
especificaciones, diseño y tareas, y muestra un flujo proponer -> implementar -> archivar,
actualizando las specs al cerrar el cambio. También permite revisar artefactos sin una
secuencia rígida. Frente al flujo de constitución y etapas explícitas que usamos con Spec Kit,
preferiría ese enfoque para un cambio pequeño sobre una app existente, por ejemplo añadir
modo oscuro con requisitos acotados. Es una elección contextual, no una superioridad demostrada.
Fuente primaria consultada el 2026-09-30: [OpenSpec oficial](https://github.com/Fission-AI/OpenSpec/).
La instalación y comandos de Spec Kit se contrastaron con su
[documentación oficial](https://github.github.com/spec-kit/installation.html).

Elegiría vibe para explorar en una tarde dos disposiciones visuales de esta calculadora,
sin datos importantes ni reglas complejas, verificando manualmente el resultado y conservando
solo la opción elegida. Una especificación completa puede costar más que ese experimento
pequeño; al estabilizar requisitos, registraría decisiones y pruebas.

## Estado antes de entregar

- [x] main, vibe y sdd existen; vibe/sdd comparten base.
- [x] Misma sesión y configuración, sin subagentes.
- [x] AGENTS.md, .specify, constitución SOLID, spec, plan y tareas versionados.
- [x] Tres archivos de pruebas requeridos más pruebas de límites.
- [x] SDD: 28 pruebas, análisis limpio, APK generado y dominio sin Flutter.
- [x] Respuestas y bitácora en main.
- [ ] Ejecutar manualmente los seis escenarios y registrar observaciones del estudiante.
- [x] Publicar las tres ramas en GitHub: [Participacion1oct](https://github.com/paugallardo-collab/Participacion1oct), repositorio privado creado y publicado el 2026-10-01 por solicitud del estudiante.

No se publicó en el remoto del repositorio padre ni se sobrescribieron otros trabajos.
