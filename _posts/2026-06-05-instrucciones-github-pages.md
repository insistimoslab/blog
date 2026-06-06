---
title: 'Instrucciones breves para hostear página en GitHub Pages'
date: 2026-06-05
permalink: /posts/2026/06/instrucciones-github-pages/
tags:
  - programming
published: true
---


# Cómo hostear un blog en GitHub Pages

Describo aquí, de forma resumida, los pasos que seguí para hostear este blog utilizando GitHub Pages y una plantilla de Jekyll.

## Instalación inicial

1. Descargar **Git Bash** para Windows.
2. Descargar **Ruby** para Windows.
   - En mi caso funcionó con:
     - Ruby 3.2.10
     - Jekyll 3.10.0
     - Bundler 2.7.2
3. Iniciar sesión en GitHub.
4. Buscar una plantilla y hacer un fork.
   - En mi caso utilicé **Academic Pages**.
5. Crear una carpeta local donde se clonará el repositorio.

```bash
git clone REPOSITORY_LINK
```

## Configuración de SSH

### Crear una clave SSH

Verificar la instalación de Git:

```bash
git --version
```

Verificar si existen claves SSH:

```bash
ls ~/.ssh
```

Crear una nueva clave SSH:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_USER -C "USER@email.com"
```

Esto crea una clave SSH para el usuario `USER`. Es especialmente útil si se manejan varias cuentas de GitHub (por ejemplo, una personal y una profesional).

No es necesario utilizar passphrase.

### Crear archivo de configuración SSH

Si se utiliza una sola cuenta de GitHub, este paso es opcional.

Crear o editar el archivo:

```text
~/.ssh/config
```

Contenido:

```text
Host github-USER
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_USER
```

### Agregar la clave SSH a GitHub

Ir a:

```text
Profile → Settings → SSH and GPG Keys
```

Copiar el contenido del archivo:

```text
id_ed25519_USER.pub
```

y agregar una nueva clave SSH.

### Configurar el repositorio para usar la clave correcta

Ir a la carpeta del repositorio y verificar el remote:

```bash
git remote -v
```

Debería mostrar algo similar a:

```text
origin git@github.com:USER/MY_REPO.git (fetch)
origin git@github.com:USER/MY_REPO.git (push)
```

Actualizar el remote para utilizar el alias definido en el archivo `config`:

```bash
git remote set-url origin git@github-USER:USER/MY_REPO.git
```

Ejemplo:

```bash
git remote set-url origin git@github-INSISTIMOSLAB:insistimoslab/blog.git
```

### Iniciar el agente SSH

Este comando debe ejecutarse cada vez que se inicia una sesión de Git Bash, a menos que se automatice.

```bash
eval "$(ssh-agent -s)"
```

Agregar la clave:

```bash
ssh-add ~/.ssh/id_ed25519_USER
```

Verificar que fue cargada:

```bash
ssh-add -l
```

Probar la conexión:

```bash
ssh -T git@github-USER
```

Debería aparecer un mensaje similar a:

```text
Hi USER! You've successfully authenticated...
```

## Publicar el sitio

1. Abrir la carpeta del proyecto en Visual Studio Code.

2. Ir a:

```text
GitHub → Settings → Pages
```

3. Seleccionar:

```text
Deploy from a branch
```

y elegir la rama:

```text
master
```

Esperar unos minutos y recargar la página.

4. Abrir el archivo:

```text
_config.yml
```

Actualizar los campos:

```yaml
url: https://USER.github.io
baseurl: /NOMBRE_REPOSITORIO
```

donde:

- `url` es la URL del sitio.
- `baseurl` es el nombre del repositorio.

5. Configurar la identidad de Git:

```bash
git config user.email "USER@email.com"
git config user.name "USER"
```

6. Guardar y subir cambios:

```bash
git status
git add .
git commit -m "Mensaje del commit"
git push origin master
```

## Flujo de trabajo para modificar el blog

### Iniciar sesión de trabajo

Abrir Git Bash y navegar a la carpeta del proyecto.

Verificar la configuración:

```bash
git remote -v
```

Iniciar el agente SSH:

```bash
eval "$(ssh-agent -s)"
```

Cargar la clave:

```bash
ssh-add ~/.ssh/id_ed25519_USER
```

Verificar:

```bash
ssh-add -l
```

Actualizar el repositorio:

```bash
git status
git pull
```

### Ejecutar el sitio localmente

```bash
bundle exec jekyll serve
```

### Guardar y publicar cambios

Después de modificar los archivos:

```bash
git status
git add .
git commit -m "Mensaje"
git branch
git remote -v
git push origin master
```


