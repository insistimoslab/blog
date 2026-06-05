---
title: 'Instrucciones breves para hostear página en GitHub Pages (DRAFT)'
date: 2026-06-05
permalink: /posts/2026/06/instrucciones-github-pages/
tags:
  - programming
published: true
---

Describo aquí muy resumidamente los pasos a seguir para hostear una página en Github Pages. Estos son los mismos pasos que seguí para hostear este blog. 

1. Descargar Git Bash para windows. 
2. Descargar Ruby para windows. (en mi caso funcionó con Ruby 3.2.10, Jekyll 3.10.0, Bundle 2.7.2)
3. Iniciar sesión en Github. Buscar un template y hacer fork. (yo utilicé academicpages)
4. Crear carpeta en local para clonar repositorio.
  > git clone repository_link
5. Creación del SSH key:
  > git --version
  > ls ~/.ssh
  > ssh-keygen -t ed25519 -f ~/.ssh/id_ed15519_USER -C "USER@email.com"
    (esto crea una ssh-key para el usuario USER, es util si se manejan varias cuentas de github, por ejemplo una personal y una profesional)
    (no es necesario usar passphrase)
6. Crear archivo config con las cuentas (no es necesario si solo se maneja una cuenta), el contenido del archivo es el siguiente:
    Host github-USER
          HostName github.com
          User git
          Identity File ~/.ssh/id_ed25519_USER  
7. Agregar SSH-KEY a la cuenta de GitHub: En profile/settings/SSH and GDP keys. Copiar el contenido del archivo id_25519_USER.pub
8. Cambiar repositorios para usar host correcto y probar conexión.
  Ir a la capeta del repositorio y verificar remote:
  > git remote -v
  (debe devolcer algo similar a "origin git@github.com USER/MY_REPO.git") (fetch y push)

  Usamos el archivo confir para direccionar los alias:
  > git remote set-url origin git@github-USER:USER/MY_REPO.git
  #git remote set-url origin git@github-INSISTIMOSLAB:insistimoslab/blog

  Luego corremos el agente (esto debe correrse cada vez que se inicie la sesion por Git Bash, a menos que se automatice):
  > eval "$(ssh-agent -s)"

  Luego cargamos claves:
  > ssh-add ~/.ssh/id_ed25519_USER

  Verificamos que cargaron:
  > ssh-add -l

  Probamos conexión:
  > ssh -T git@github-USER
  (debe devolcer "Hi USER! You have been succesfully authenticated...")


PARA HOSTEAR
======

1. Instalar Visual Studio Code y abrir el folder del proyecto.
2. Ir a Github/Settings del repositorio/Pages y seleccionar "Deploy from a branch" (elegir branch "master"). Esperar unos minutos a que se haga el deploy y recargar la página:
3. En el folder del proyecto, buscar el archivo config.yaml y copiar la URL del sitio (la que nos proporciona GitHub después del deploy). Cambiamos URL por la del sitio y Base URL por la del repositorio.
4. Configuramos identidad: agregamos email y nombreÑ
  > git config user.email "USER@email.com"
  > git config user.name "USER"
5. Guardar cambios con commit:
  > git status
  > git add .
  > git commit -m "mensaje del commit"
  > git push origin master


FLUJO PARA HACER CAMBIOS AL BLOG
======
1. Para iniciar, abrir Git Bash e ir al floder del proyecto en la terminal.
  > git remote -v
  > eval "$(ssh-agent -s)"
  > ssh-add ~/.ssh/id_ed25519_USER
  > ssh-add -l
  > git status
  > git pull

  Si se desea lanzar en local:
  > bundle exec jekyll serve

4. Para hacer cambios. Despues de los cambios realizados en los archivos correspondientes del proyecto:
  > git status
  > git add .
  > git commit -m "mensaje"
  > git branch
  > git remote -v
  > git push origin master

