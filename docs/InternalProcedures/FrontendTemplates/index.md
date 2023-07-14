# Crear nuevo proyecto a partir de una plantilla

Igresa a tu cuenta `github`

![Untitled](Resources/github.png)

Seleciona tu perfil e ingresa a las organizaciones

![Untitled](Resources/selectOrganizations.png)

Seleciona la organización donde vas a crear el repositorio

![Untitled](Resources/apsys.png)

Seleciona nuevo repositorio

![Untitled](Resources/addRepository.png)

Te debe apararcer la siguiente vista:

![Untitled](Resources/vewAddRepository.png)

Asegurate de tener acceso a la organición de `Apsys` y selecciona la opción `apsys-mx/apsys.frontend.base.turkey`.

Esta es la plantilla base que Apsys generó con la configuración de tecnologías necesarias para inicializar un proyecto.

- Sí no cuentas con el acceso al repositorio, deberas solicitarlo al administrador.

![Untitled](Resources/template.png)

- Agrega el nombre del proyecto, selecciona la opción de Repositorio Privado y finaliza el proceso seleccionando el botón `Crear repositorio`

  ![Untitled](Resources/saveRepository.png)

Una vez finalizado este proceso, Github te redirigirá al proyecto creado.

![Untitled](Resources/vewRepository.png)

## Clonar repositorio en un dispositivo local

Al abrir el proyecto, selecciona el botón Clonar y copia la ruta del proyecto.

![Untitled](Resources/cloneRepository.png)

En el explorador de archivos de tu dispositivo, ingresa a la ubicación donde deseas clonar el repositorio. Para clonar el repositorio existen varias formas de hacerlo:

- Forma manual desde la terminal.

Ubica la ruta de tu carperta donde quieres clonar el repositorio e ingresa el siguiente comando:

`git clone`

Por ultimo, pega la ruta de git, del repositorio que quieres clonar.

![Untitled](Resources/comandClone.png)

- Usando un programa de control de versiones.

En Apsys usualmente usamos Tortoise Git.

![Untitled](Resources/clone.png)
![Untitled](Resources/confirm.png)

Cunado termine de clonarce e repositorio debe aparcer una carpeta donde se guardo.

![Untitled](Resources/repository.png)

Cambiamos o creamos el branch a devel

![Untitled](Resources/changeBranch.png)
![Untitled](Resources/branch.png)

Revisamos la versión de node que tengamos instalas, abrimos una consola como administrador.
`nvm list`

![Untitled](Resources/node.png)

Utilizamos la 18.13.0, si no la tenemos la instalamos.

`nvm use 18.16.0` o instalamos `nvm install 18.13.0`
![Untitled](Resources/nvmUse.png)

Abrimos el protecto y abrimos la consola y comenzamos cons instalar los paquetes:

Ahora ejecutamos el proyecto con `npm install`

![Untitled](Resources/npmInstall.png)

Inicializamos el proyecto:

![Untitled](Resources/runDev.png)

Nos debe de aparecer la vista de inicio ingresamos ala plataforma:

![Untitled](Resources/inicio.png)

Si nos aparece este error debemos de inicializar el usuario:

![Untitled](Resources/error.png)

Ingresamos el postman:
![Untitled](Resources/postman2.png)

-agregamos la siguiente ruta:
`http://localhost:57065/clients`

-Indicamos que sea un método post

-Agregamos lo siguiente en el body:

```ruby linenums="1"

{
   "ClientId":"apsys.frontend.base",
   "ClientName":"SIT Alpunto Web Application",
   "AllowedGrantTypes":[
      "implicit"
   ],
   "AllowAccessTokensViaBrowser":true,
   "AlwaysIncludeUserClaimsInIdToken":true,
   "AllowedCorsOrigins":[
        "http://localhost:5173",
        "http://127.0.0.1:5173"
   ],
   "RedirectUris":[
        "http://localhost:5173/callback",
        "http://127.0.0.1:5173/callback"
   ],
   "PostLogoutRedirectUris":[
        "http://localhost:5173",
        "http://127.0.0.1:5173"
   ],
   "AllowedScopes":[
        "openid",
        "profile",
        "userprofile"
   ],
   "RequireConsent":false,
   "ClientSecrets":[
      {
         "value":"8bzLMAwmmlPbMWSeXmMp8qOo3yKhPCN85Ws28Qswhe8="
      }
   ]
}

```

-Ingresar clientId el id de tu proyecto del front:
![Untitled](Resources/id.png)

-En AllowedCorsOrigins, PostLogoutRedirectUris agregamos la ruta del front, y para RedirectUris asignamos a la ruta agregando `/callback`
![Untitled](Resources/idClient.png)

-Confirmamos que estén todos los campos como se ve en la imagen y para finalizar enviamos la petición y nos debe dar una repuesta ok:

![Untitled](Resources/postman2.png)

-
