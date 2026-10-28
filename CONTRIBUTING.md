# Cómo se trabaja en Argonauta Sistemas

Esta guía vale para todos los repositorios de la organización y para
cualquier persona que trabaje en ellos, incluido Claude Code. Cada repo tiene
además su `CLAUDE.md` con lo propio de ese proyecto.

La idea que la ordena: **un estándar que depende de acordarse es una
intención; uno que la herramienta hace cumplir es un proceso.** Por eso casi
todo lo de abajo lo revisa una máquina.

## 1. Cuentas y accesos

- **Cada persona entra con su propia cuenta**, con verificación en dos pasos
  (la organización la exige). Nunca se comparte una cuenta ni una contraseña.
- **Las contraseñas viven en un gestor** (Bitwarden). Nunca en el chat, en un
  archivo del repo, en una captura ni en un mensaje.
- **Dónde vive cada proyecto:**
  1. **Laboratorio:** nace en las cuentas de Argonauta Sistemas, con datos de
     prueba.
  2. **Casa propia:** antes de recibir datos reales, se muda a cuentas a nombre
     del cliente (su correo, su base de datos, su publicación y su dominio).
     Argonauta trabaja ahí como miembro invitado.
  3. **Entrega:** si el servicio termina, el cliente se queda con todo y
     Argonauta se quita como miembro.

  El paso a paso está en la skill `arranque` (`references/cuentas.md`) de
  [biblioteca-skills](https://github.com/argonauta-sistemas/biblioteca-skills).
- **El código** vive en esta organización y es de Argonauta Sistemas; el
  cliente tiene una licencia de uso, salvo que el acuerdo diga otra cosa.

## 2. Cómo se hace un cambio

1. **Una rama por cambio:** `feat/...` (algo nuevo), `fix/...` (un arreglo) o
   `docs/...` (documentación). Nada directo a `main`: cada push a `main`
   publica.
2. **Commits en español, en infinitivo**, diciendo qué cambia y por qué
   ("Cerrar la lectura anónima de los datos de los socios").
3. **Pull Request** con la plantilla: qué cambia, por qué y cómo se probó.
4. **La revisión automática (CI) en verde** antes de mezclar: lint, formato,
   tests y build. No se mezcla un PR en rojo.
5. **Merge normal.** Squash solo si el repo no usa `.git-blame-ignore-revs`.
6. **Después del merge**, comprobar en línea que el cambio llegó.

> En el plan gratuito de GitHub, la protección de ramas solo existe en repos
> públicos. En los privados, la regla de "todo por PR con CI en verde" la
> sostienen esta guía, las plantillas y los hooks de Claude Code de cada repo.

## 3. Un cambio está terminado cuando

- [ ] Lint, formato, tests y build pasan.
- [ ] Lo nuevo con riesgo (dinero, permisos, fechas, datos personales) tiene
      un test que falla si se rompe.
- [ ] Se probó en el navegador, también en el celular.
- [ ] Si tocó la base de datos: hay una migración nueva (nunca se edita una
      aplicada) y se avisó que hay que correrla.
- [ ] La documentación (`README.md`, `CLAUDE.md`) sigue diciendo la verdad.

## 4. Datos de los clientes

- Los datos son del cliente. Se tratan según la **Ley 29733** de protección de
  datos personales, con más cuidado si son de salud.
- **Nunca se leen datos de personas reales para probar algo:** se cuentan
  filas o se usan ids inventados.
- **Los permisos de una base en uso se cambian en tres pasos** (agregar lo
  nuevo, publicar la app que lo usa, recién ahí cerrar lo viejo), sin dejar la
  app caída ni un minuto.
- **Los respaldos con datos personales se suben cifrados.**

## 5. Antes de lanzar

Ningún proyecto recibe usuarios reales sin pasar por la skill `lanzamiento`:
qué ve alguien sin sesión, secretos, dinero, datos personales, respaldo,
cómo nos enteramos de un error y publicación. Lo que "bloquea" se arregla
antes.

## 6. Entrar y salir del equipo

**Cuando alguien entra:**

- [ ] Invitarlo a la organización con **su propia** cuenta de GitHub (con
      verificación en dos pasos).
- [ ] Darle acceso **solo** a los repos en los que va a trabajar (la
      organización arranca sin permisos por defecto).
- [ ] Si necesita Supabase o Cloudflare, invitarlo como miembro con el rol
      mínimo que le alcance. Nunca pasarle una contraseña.
- [ ] Que lea esta guía y el `CLAUDE.md` de cada repo.

**Cuando alguien sale:**

- [ ] Quitarlo de la organización de GitHub.
- [ ] Quitarlo de cada organización de Supabase y cuenta de Cloudflare.
- [ ] Si alguna vez vio una clave o un token, **cambiarlo** (rotarlo).
- [ ] Revisar que ningún proyecto dependa de una cuenta suya.

## 7. Herramientas

| Para qué | Qué |
|---|---|
| Definir un proyecto nuevo | skill `metis` |
| Montar el repo con este estándar | skill `arranque` |
| Revisar antes de lanzar | skill `lanzamiento` |
| Seguimiento | el tablero de Trello de Argonauta |
| Contraseñas | Bitwarden |
