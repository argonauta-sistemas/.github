# .github de Argonauta Sistemas

Archivos que GitHub aplica a todos los repositorios de la organización que no
tengan los suyos propios:

- `profile/README.md`: la página pública de la organización.
- `CONTRIBUTING.md`: cómo se trabaja.
- `SECURITY.md`: cómo reportar un problema de seguridad.
- `pull_request_template.md` e `ISSUE_TEMPLATE/`: plantillas de PR y de issues.

Este repositorio es público porque GitHub lo exige para que funcione. Solo
lleva reglas y la vitrina: nunca nada de un cliente.

Además guarda lo que comparten todos los repos y que GitHub no aplica solo:

- `.github/workflows/linceo.yml` y `agentes/linceo/`: Linceo, el revisor de
  PRs. Cada repo lo activa copiando `agentes/linceo/llamada.yml`; los pasos
  están en [`agentes/linceo/README.md`](agentes/linceo/README.md).
