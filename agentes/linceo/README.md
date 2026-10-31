# Linceo · revisor de PRs

Linceo busca en cada PR los errores que romperían algo en producción o
expondrían datos de un cliente. Comenta; nunca aprueba ni hace merge. Su ficha
completa está en `conocimiento/agentes/linceo.md`.

- [`instrucciones.md`](instrucciones.md): misión, qué no hace, la lista de 6
  puntos y el formato de respuesta. Es el system prompt de cada revisión.
- [`llamada.yml`](llamada.yml): el archivo que cada repo copia.
- [`../../.github/workflows/linceo.yml`](../../.github/workflows/linceo.yml):
  el workflow reutilizable que hace el trabajo.

## Qué hace el workflow

1. **Preparar (sin IA, $0).** Lista los archivos del PR y descarta
   documentación, imágenes, lockfiles, `dist/`, `node_modules/` y tipos
   generados de Supabase. Si no queda nada, se salta el PR. Si quedan más de
   800 líneas, comenta `Linceo · Divide este PR` y no llama al modelo.
2. **Elegir modelo por rutas.** Sonnet 5.5 con esfuerzo bajo por defecto.
   Opus 5.5 con esfuerzo medio si algún archivo toca `supabase/migrations/`,
   `functions/`, RLS o políticas, autenticación o pagos.
3. **Revisar.** Corre la acción oficial `anthropics/claude-code-action@v1`
   con un máximo de 10 turnos, solo las herramientas `Read`, `Grep` y `Glob`,
   y las instrucciones como system prompt fijo (se cachea). El diff va en el
   mensaje, ya filtrado.
4. **Publicar.** Pone un comentario con el formato de la ficha.
5. **Registrar.** Escribe en el resumen del job el modelo, los turnos, el
   costo en dólares y los tokens.

Corre cuando un PR se abre listo para revisión, cuando pasa de borrador a
listo o cuando recibe la etiqueta `linceo`. Nunca en borradores ni en cada
push. La etiqueta `sin-linceo` lo apaga en ese PR.

Si falta la llave, se acabó el saldo o la llave es inválida, el job termina en
verde con un aviso: Linceo nunca bloquea el merge.

## Activarlo en un repo

1. **La llave.** En el plan gratuito de GitHub, los secretos de organización
   no llegan a los repos privados. Por eso la llave va como secreto en cada
   repo: *Settings → Secrets and variables → Actions → New repository secret*,
   nombre `ANTHROPIC_API_KEY`. La crea y la pega un socio desde la consola de
   Anthropic; nunca se escribe en un archivo, un issue ni un chat. Con la
   terminal:

   ```bash
   gh secret set ANTHROPIC_API_KEY --repo argonauta-sistemas/NOMBRE-DEL-REPO
   ```

   (`gh` pide la llave sin mostrarla).
2. **El archivo.** Copiar [`llamada.yml`](llamada.yml) en
   `.github/workflows/linceo.yml` del repo y subirlo en un PR.
3. **Las etiquetas.** Crear `linceo` y `sin-linceo`:

   ```bash
   gh label create linceo --repo argonauta-sistemas/NOMBRE-DEL-REPO --color 1d76db --description "Pedir revisión de Linceo"
   ```

   ```bash
   gh label create sin-linceo --repo argonauta-sistemas/NOMBRE-DEL-REPO --color cccccc --description "Linceo no revisa este PR"
   ```

## Primera prueba: `argonauta-sistemas/Argonauts`

Linceo no se ha probado contra la API porque todavía no hay llave. Cuando la
haya:

1. Poner la llave como secreto en `Argonauts` (paso 1 de arriba).
2. Crear las etiquetas y abrir un PR en `Argonauts` que agregue
   `.github/workflows/linceo.yml`. Ese mismo PR ya dispara la primera
   corrida.
3. Probar los cuatro caminos y anotar el costo de cada uno en el resumen del
   job:
   - Un cambio pequeño en `frontend/` → debe usar Sonnet y responder
     `Linceo · Sin hallazgos` o hallazgos con el formato.
   - Un cambio en `supabase/migrations/` con una tabla sin RLS → debe usar
     Opus y marcar 🔴.
   - Un PR de solo `*.md` → no debe llamar al modelo.
   - Un PR con la etiqueta `sin-linceo` → no debe correr.
4. Comprobar que el costo por PR quede por debajo de $0.15. Si no, bajar el
   esfuerzo o ajustar la lista de rutas de riesgo antes de extenderlo.
5. Medir un mes en `Argonauts` antes de copiarlo a otros repos.

## Cambiar a Linceo

- Las instrucciones y los filtros viven aquí; todos los repos los toman de
  `main`. Un cambio aquí cambia a Linceo en todos los repos a la vez.
- Para fijar una versión en un repo, cambiar `@main` en su `llamada.yml` y
  pasar la misma etiqueta en `with: version:`.
