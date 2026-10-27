# Dos capturas que requieren tu sesión web de GitHub

El workflow, los PR, la protección y el experimento de fallo se ejecutaron realmente. El agente pudo autenticarse con GitHub CLI, pero no dispone de una sesión web autenticada para fotografiar Settings y el botón de merge. Los JSON, logs y transcripciones de esas operaciones sí están guardados.

Para cumplir literalmente la consigna, faltan estas dos imágenes de la interfaz:

1. **Protección de main.** Abre [Settings → Branches](https://github.com/paugallardo-collab/divisor_cuenta/settings/branches), consulta la regla existente de `main` y captura los requisitos de PR y check `verificar`. No necesitas cambiar la configuración. Guarda la imagen como `evidencias/parte2_proteccion_interfaz.png`.
2. **Botón de merge bloqueado.** Abre el [PR auxiliar #4: NO FUSIONAR](https://github.com/paugallardo-collab/divisor_cuenta/pull/4), espera el check rojo y captura el check fallido y la sección de merge bloqueada con tu cuenta iniciada. Guarda la imagen como `evidencias/parte3_merge_bloqueado_interfaz.png`. No corrijas ni fusiones ese PR: existe para mantener disponible la captura.

Carpeta local para guardarlas:

```text
C:\FlutterProjects\Prog.-Asistida-de-Aplicaciones\Deber4\divisor-cuenta-cicd\evidencias
```

Una vez guardadas, deben agregarse y enlazarse desde `respuestas.md` en la misma rama de entrega `deber4`, mediante un nuevo commit al mismo PR. El PR de entrega debe quedar abierto hacia main; no hagas push directo a main.

El experimento original está en el [PR #2](https://github.com/paugallardo-collab/divisor_cuenta/pull/2): falló, se comprobó un rechazo real de merge, se restauró la prueba en ese mismo PR y se fusionó en verde. El PR #4 es una repetición auxiliar, identificada como tal; no reemplaza ese historial. Las pruebas de la rama de entrega y de main conservan el resultado correcto `27.50`.

Las imágenes `parte2_proteccion.png` y `parte3_merge_bloqueado.png` que ya existen son transcripciones auténticas de API/CLI. No se deben describir como capturas de la interfaz. Conserva también esas pruebas técnicas al añadir las nuevas imágenes.
