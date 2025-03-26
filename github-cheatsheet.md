# GitHub Cheat Sheet

## Configuración Inicial

```bash
# Configurar información del usuario
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"

# Configurar editor predeterminado
git config --global core.editor "code --wait"  # Para VS Code

# Verificar configuración
git config --list
```

## Comandos Básicos

```bash
# Inicializar un repositorio
git init

# Clonar un repositorio existente
git clone https://github.com/usuario/repositorio.git
git clone https://github.com/usuario/repositorio.git nombre-directorio

# Estado del repositorio
git status

# Añadir archivos al área de preparación (staging)
git add nombre-archivo
git add .                  # Añadir todos los cambios
git add *.py               # Añadir todos los archivos .py

# Hacer commit de los cambios
git commit -m "Mensaje descriptivo del commit"
git commit -am "Mensaje"   # Añadir y hacer commit en un solo paso (solo archivos rastreados)

# Ver historial de commits
git log
git log --oneline          # Formato resumido
git log --graph            # Formato visual de ramas
```

## Trabajar con Ramas

```bash
# Listar ramas
git branch                 # Locales
git branch -r              # Remotas
git branch -a              # Todas

# Crear rama
git branch nombre-rama

# Cambiar a una rama
git checkout nombre-rama
git switch nombre-rama     # Git moderno

# Crear y cambiar a una rama en un solo paso
git checkout -b nombre-rama
git switch -c nombre-rama  # Git moderno

# Eliminar rama
git branch -d nombre-rama  # Eliminación segura
git branch -D nombre-rama  # Forzar eliminación

# Fusionar ramas
git merge nombre-rama      # Fusionar nombre-rama en la rama actual
```

## Trabajar con Repositorios Remotos

```bash
# Listar remotos
git remote -v

# Añadir remoto
git remote add origin https://github.com/usuario/repositorio.git

# Cambiar URL del remoto
git remote set-url origin https://github.com/usuario/nuevo-repo.git

# Descargar cambios sin fusionar
git fetch origin

# Descargar cambios y fusionar
git pull origin nombre-rama

# Enviar cambios al remoto
git push origin nombre-rama
git push -u origin nombre-rama  # Establecer rama upstream
```

## Deshacer Cambios

```bash
# Descartar cambios en archivos no preparados
git restore nombre-archivo
git checkout -- nombre-archivo  # Git antiguo

# Deshacer cambios preparados (quitar del staging)
git restore --staged nombre-archivo
git reset HEAD nombre-archivo   # Git antiguo

# Modificar último commit
git commit --amend -m "Nuevo mensaje"

# Revertir un commit (crea un nuevo commit que deshace los cambios)
git revert hash-commit

# Resetear a un commit específico
git reset --soft hash-commit    # Mantiene cambios en staging
git reset --mixed hash-commit   # Mantiene cambios en working directory
git reset --hard hash-commit    # Elimina todos los cambios (¡peligroso!)
```

## Stash (Guardado Temporal)

```bash
# Guardar cambios temporalmente
git stash

# Guardar con mensaje
git stash save "Mensaje descriptivo"

# Listar stashes
git stash list

# Aplicar último stash
git stash apply

# Aplicar stash específico
git stash apply stash@{n}

# Aplicar y eliminar último stash
git stash pop

# Eliminar último stash
git stash drop

# Eliminar stash específico
git stash drop stash@{n}

# Eliminar todos los stashes
git stash clear
```

## Pull Requests

1. Hacer fork del repositorio en GitHub
2. Clonar tu fork: `git clone https://github.com/tu-usuario/repositorio.git`
3. Crear rama para cambios: `git checkout -b feature/nueva-funcionalidad`
4. Realizar cambios, commit, y push: `git push origin feature/nueva-funcionalidad`
5. Ir a GitHub y crear Pull Request desde tu rama

## Trabajar con Tags

```bash
# Listar tags
git tag

# Crear tag ligero
git tag v1.0.0

# Crear tag anotado (recomendado)
git tag -a v1.0.0 -m "Versión 1.0.0"

# Crear tag para un commit específico
git tag -a v0.9.0 -m "Versión Beta" hash-commit

# Subir tags al remoto
git push origin v1.0.0        # Tag específico
git push origin --tags        # Todos los tags
```

## Git Flow (Flujo de Trabajo)

```bash
# Instalar Git Flow
apt-get install git-flow      # Debian/Ubuntu
brew install git-flow         # macOS

# Inicializar Git Flow en un repositorio
git flow init

# Trabajar con features
git flow feature start nombre-feature
git flow feature finish nombre-feature

# Trabajar con releases
git flow release start v1.0.0
git flow release finish v1.0.0
```

## Solución de Problemas

```bash
# Ver reflog (historial de referencias)
git reflog

# Verificar integridad del repositorio
git fsck

# Limpiar archivos no rastreados
git clean -n                   # Simulación (muestra qué se eliminaría)
git clean -f                   # Eliminar archivos no rastreados
git clean -fd                  # Incluir directorios

# Mantenimiento y optimización
git gc                         # Garbage collection
git prune                      # Eliminar objetos inaccesibles
```

## Configuración de .gitignore

Ejemplo de archivo `.gitignore`:

```
# Archivos del sistema
.DS_Store
Thumbs.db

# Directorios de entornos virtuales
venv/
env/
.env/

# Archivos de Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
*.egg-info/

# Archivos de logs
*.log

# Directorios de dependencias
node_modules/
vendor/

# Archivos de configuración local
config.local.js
.env.local
```

## GitHub CLI

```bash
# Instalar GitHub CLI
# Visitar: https://cli.github.com/

# Autenticarse
gh auth login

# Clonar repositorio
gh repo clone usuario/repositorio

# Crear repositorio
gh repo create nombre-repo

# Crear issue
gh issue create --title "Título" --body "Descripción"

# Crear pull request
gh pr create --title "Título" --body "Descripción"
```
