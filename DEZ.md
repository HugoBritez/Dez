# Dez

Fork de [Zed](https://github.com/zed-industries/zed) para incorporar herramientas de bases de datos dentro del editor. El nombre Dez es provisional.

Repositorio: https://github.com/HugoBritez/Dez

## Estado inicial

- Base: Zed **v1.26.2**, publicada el 28 de octubre de 2026.
- `dev`: rama de trabajo y rama predeterminada de Dez.
- `main`: referencia original del fork; no desarrollar funcionalidades aquí.
- `origin`: `HugoBritez/Dez`.
- `upstream`: `zed-industries/zed`.

Esta preparación agrega el proceso de mantenimiento. Todavía no cambia el nombre, el identificador, las rutas de configuración, el actualizador ni la interfaz de la aplicación; tampoco implementa el cliente de bases de datos. Antes de instalar una aplicación llamada Dez, separar su identidad y sus actualizaciones de Zed.

## Desarrollo

```sh
git switch dev
git pull --ff-only origin dev
git switch -c feature/database-panel
```

Mantener los cambios propios pequeños y concentrados. Separar la conectividad y ejecución de SQL de los componentes de interfaz, y reutilizar los componentes de Zed cuando sea posible. Conservar licencias y atribuciones upstream.

## Actualizar desde Zed

Seguir las **versiones estables publicadas**, usando sus etiquetas oficiales. `upstream/main` se puede consultar para investigar, pero no se incorpora automáticamente a `dev`.

Requiere `git`, `bash` y GitHub CLI (`gh`) autenticado.

Para comprobar si la rama local `dev` ya contiene la última versión estable:

```sh
./script/dez-upstream --check
```

Para preparar una actualización:

```sh
git switch dev
git pull --ff-only origin dev
./script/dez-upstream --prepare
```

El comando exige una copia limpia de `dev` que coincida con `origin/dev`, crea `update/zed-<version>` e incorpora la etiqueta oficial con un merge. No sobrescribe ramas existentes, no hace rebase ni force-push, y no publica ni integra la actualización en `dev`.

Si aparecen conflictos, resolverlos en esa rama y completar el merge con `git add` y `git commit`. Para cancelar un merge con conflictos, usar `git merge --abort`; el comando no borra la rama de actualización.

Antes de integrar una actualización:

1. Revisar el diff, los conflictos resueltos y las notas de la versión de Zed.
2. Compilar y probar el editor según las instrucciones upstream.
3. Ejecutar los tests de los componentes modificados y probar manualmente las funciones de Dez.
4. Publicar la rama y abrir un PR dirigido explícitamente al fork:

```sh
git push -u origin HEAD
gh pr create --repo HugoBritez/Dez --base dev --head "$(git branch --show-current)"
```

Seguir `CONTRIBUTING.md` y `.rules` para el formato del PR. No abrir PRs hacia Zed oficial sin una revisión humana y coordinación previa con sus mantenedores.

No hay sincronización ni merges programados: el mantenimiento se ejecuta bajo demanda. Comprobar actualizaciones regularmente y antes de iniciar funcionalidades grandes.

## Compilar en macOS

Consultar [la guía upstream](docs/src/development/macos.md) y `rust-toolchain.toml` para las dependencias y versiones requeridas.

```sh
cargo run
```

La preparación del fork no confirma que el editor compile o funcione en esta máquina. La primera compilación y la separación de la identidad de la aplicación son pasos pendientes.
