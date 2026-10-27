# Revisión crítica del workflow

La revisión usa el primer run real de CI, sus avisos y los metadatos de las acciones en sus repositorios de origen. Los SHA comprobados están en `evidencias/acciones_verificadas.json`.

## Cambios aceptados

- **Actualizar acciones obsoletas.** El primer run avisó que checkout v4 y setup-java v4 usaban Node.js 20, y que setup-java v4 ya no recibía actualizaciones. Se verificaron checkout v7.0.1, setup-java v6.1.0 y upload-artifact v7.0.2: sus metadatos declaran Node.js 24.
- **Fijar cada acción por SHA.** Una etiqueta como `v2` puede cambiar. Los SHA fijan el código directo de las acciones utilizado por este workflow; su revisión y sus futuras actualizaciones siguen siendo necesarias.
- **Reducir permisos y exposición de credenciales.** `contents: read` es suficiente; `persist-credentials: false` evita dejar la credencial de checkout en la configuración Git durante la compilación. No hay secretos ni interpolaciones de títulos o comentarios de PR en comandos de shell.
- **Usar `pull_request`.** No se usa `pull_request_target` para ejecutar código no confiable con permisos elevados. Los runners son hospedados por GitHub y temporales.
- **Limitar los jobs.** Cada job tiene un máximo de 30 minutos para evitar ejecuciones colgadas.
- **Respetar el aislamiento entre jobs.** `entrega` vuelve a descargar el código y a generar el APK. `needs: verificar` impide entregar cuando falla CI, y la condición limita la entrega a push a main.
- **Usar el mismo SDK dentro del run.** `verificar` selecciona Flutter estable y exporta su versión; `entrega` instala esa versión exacta para no seleccionar otro SDK por separado.
- **Detectar APK ausente.** `if-no-files-found: error` evita una entrega aparentemente exitosa sin archivo. El nombre contiene el SHA del commit; los artifacts se conservan 30 días y se guarda también evidencia permanente de su existencia.

## Sugerencias no adoptadas en esta actividad

- Fijar también Ubuntu y Flutter a versiones exactas entre todos los runs mejoraría la reproducibilidad. Se mantienen `ubuntu-latest` y el canal estable porque la consigna los pide; se documenta la fragilidad. Dentro de cada run, ambos jobs sí comparten la versión seleccionada de Flutter.
- No se agregó un escáner externo ni una acción sugerida solo por su nombre. Instalar herramientas sin verificar procedencia ampliaría la cadena de suministro. `flutter analyze` no equivale a una auditoría SAST/SCA completa.
- No se agregó despliegue a producción, firma de release ni almacenamiento de claves en Flutter. Esta entrega publica un APK de depuración como artifact, que no es una versión de producción.

## Riesgos que permanecen

Flutter estable, Ubuntu, repositorios de paquetes y componentes del Android SDK pueden cambiar. `pubspec.lock` conserva las versiones resueltas de Dart, pero no fija todo el entorno. Fijar una acción por SHA no prueba que su contenido o sus dependencias sean seguros. Un PR de alguien con permisos puede proponer cambiar el workflow o las propias pruebas: sigue siendo necesario revisar el diff.

La protección de main exige PR y el check `verificar`, con la rama al día y aplicación también al administrador. No exige una aprobación de otra persona porque se trabaja en un repositorio individual. Un administrador conserva la capacidad de cambiar las reglas; la protección no elimina esa facultad.

Un CI verde solo acredita los controles ejecutados. No cubre por sí solo accesibilidad completa, comportamiento en todos los teléfonos, vulnerabilidades, requisitos ausentes ni errores en escenarios que las pruebas no contemplan.

## Fuentes verificadas

- [Checkout, código de la versión utilizada](https://github.com/actions/checkout/blob/3d3c42e5aac5ba805825da76410c181273ba90b1/action.yml).
- [Setup Java, código de la versión utilizada](https://github.com/actions/setup-java/blob/b843ebf9b913146b38da2eaa7aeddf76074ae1ad/action.yml).
- [Upload artifact, código de la versión utilizada](https://github.com/actions/upload-artifact/blob/cf430e030ddbb5b0abf93d22962f4752f3646cd9/action.yml).
- [Flutter action, versión utilizada](https://github.com/subosito/flutter-action/tree/1a449444c387b1966244ae4d4f8c696479add0b2).
