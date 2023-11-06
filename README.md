# Solución Reto Backend Producción

## Pasos Iniciales

### Creación del Proyecto

1. Genere un repositorio Git y seleccione la opcion de creacion de readme y gitignore para aplicaciones VisualStudio.

2. Cree la solución Inicial 

   ```
   dotnet new sln
   ```

3. Cree los proyectos teniendo en cuenta el Modelo MVC

   ```
   dotnet new classlib -o Application
   dotnet new classlib -o Domain
   dotnet new classlib -o Persistence
   dotnet new webapi  -o [Api]NombreProyecto
   ```

4. Asocie los proyectos a la solución creada en el punto (2)

   ```
   dotnet sln add .\Application
   dotnet sln add .\Domain
   dotnet sln add .\Persitence
   dotnet sln add .\[Api]NombreProyecto
   
   ```

   ##### Nota: Estos comandos se ejecutan desde la raiz del proyecto.

 5. Agregue las siguientes referencias entre los proyectos creados.

    ```
    cd Application
    dotnet add reference ..\Domain\
    dotnet add reference ..\Persistence\
    cd ..
    cd [Api]NombreProyecto
    dotnet add reference ..\Application\
    cd..
    cd Persistence
    dotnet add reference ..\Domain\
    ```

    ##### Nota. Recuerde que el esquema de rutas varia con cada sistema operativo.

  6. Instale los siguientes paquetes teniendo en cuenta el proyecto indicado:

      Proyecto WebApi

     ```
     Microsoft.AspNetCore.Authentication.JwtBearer
     Microsoft.AspNetCore.OpenApi
     Microsoft.EntityFrameworkCore
     Microsoft.EntityFrameworkCore.Design
     Microsoft.Extensions.DependencyInjection
     System.IdentityModel.Tokens.Jwt
     ```

     Proyecto Persistence

     ```
     Microsoft.EntityFrameworkCore
     Pomelo.EntityFrameworkCore.MySql
     ```

     Domain

     ```
     FluentValidation.AspNetCore
     itext7.pdfhtml
     Microsoft.EntityFrameworkCore
     ```

     ##### Nota. Recuerde que la instalacion de los paquetes la puede realizar usando el Administrador de paquetes Nuget o el terminal dependiendo del sistema operativo anfitrion. Si realiza la instalacion desde el terminal debe asegurarse que se encuentre ubicado en la carpeta de cada proyecto. Los paquetes pueden ser buscados en la pagina oficial nuget.org

