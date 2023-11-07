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
     dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer --version 7.0.10
     dotnet add package Microsoft.EntityFrameworkCore --version 7.0.10
     dotnet add package Microsoft.EntityFrameworkCore.Design --version 7.0.10
     dotnet add package Microsoft.Extensions.DependencyInjection --version 7.0.0
     dotnet add package System.IdentityModel.Tokens.Jwt --version 6.32.3
     ```
     
Proyecto Persistence
     
```
     dotnet add package Pomelo.EntityFrameworkCore.MySql --version 7.0.0
     dotnet add package Microsoft.EntityFrameworkCore --version 7.0.10
     ```
     
Domain
     
```
     dotnet add package FluentValidation.AspNetCore --version 11.3.0
     dotnet add package itext7.pdfhtml --version 5.0.1
     dotnet add package Microsoft.EntityFrameworkCore --version 7.0.10
     ```
     
##### Nota. Recuerde que la instalación de los paquetes la puede realizar usando el Administrador de paquetes Nuget o el terminal dependiendo del sistema operativo anfitrión. Si realiza la instalación desde el terminal debe asegurarse que se encuentre ubicado en la carpeta de cada proyecto. Los paquetes pueden ser buscados en la pagina oficial nuget.org. La versión de cada paquete depende de la versión del Dotnet SDK y de la versión del dotnet-ef tools.

## Estructura Base de datos

```sql
CREATE DATABASE /*!32312 IF NOT EXISTS*/`production` /*!40100 DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci */ /*!80016 DEFAULT ENCRYPTION='N' */;

USE `production`;

/*Table structure for table `city` */

DROP TABLE IF EXISTS `city`;

CREATE TABLE `city` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL,
  `IdstateFk` int NOT NULL,
  PRIMARY KEY (`Id`),
  KEY `IX_city_IdstateFk` (`IdstateFk`),
  CONSTRAINT `FK_city_state_IdstateFk` FOREIGN KEY (`IdstateFk`) REFERENCES `state` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `city` */

/*Table structure for table `color` */

DROP TABLE IF EXISTS `color`;

CREATE TABLE `color` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `description` varchar(255) NOT NULL,
  PRIMARY KEY (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `color` */

/*Table structure for table `country` */

DROP TABLE IF EXISTS `country`;

CREATE TABLE `country` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL,
  PRIMARY KEY (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `country` */

/*Table structure for table `customer` */

DROP TABLE IF EXISTS `customer`;

CREATE TABLE `customer` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL,
  `Idcustomer` varchar(255) NOT NULL,
  `IdTipoPersonaFk` int NOT NULL,
  `date_register` date NOT NULL,
  `IdcityFk` int NOT NULL,
  PRIMARY KEY (`Id`),
  KEY `IX_customer_IdcityFk` (`IdcityFk`),
  KEY `IX_customer_IdTipoPersonaFk` (`IdTipoPersonaFk`),
  CONSTRAINT `FK_customer_city_IdcityFk` FOREIGN KEY (`IdcityFk`) REFERENCES `city` (`Id`),
  CONSTRAINT `FK_customer_person_type_IdTipoPersonaFk` FOREIGN KEY (`IdTipoPersonaFk`) REFERENCES `person_type` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `customer` */

/*Table structure for table `detail_sale` */

DROP TABLE IF EXISTS `detail_sale`;

CREATE TABLE `detail_sale` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `IdsaleFk` int NOT NULL,
  `IdProductoFk` int NOT NULL,
  `IdsizeFk` int NOT NULL,
  `cantidad` int NOT NULL,
  `value_unit` double NOT NULL,
  PRIMARY KEY (`Id`),
  KEY `IX_detalle_sale_IdProductoFk` (`IdProductoFk`),
  KEY `IX_detalle_sale_IdsizeFk` (`IdsizeFk`),
  KEY `IX_detalle_sale_IdsaleFk` (`IdsaleFk`),
  CONSTRAINT `FK_detalle_sale_insalerio_IdProductoFk` FOREIGN KEY (`IdProductoFk`) REFERENCES `store` (`Id`),
  CONSTRAINT `FK_detalle_sale_sale_IdsaleFk` FOREIGN KEY (`IdsaleFk`) REFERENCES `sale` (`Id`),
  CONSTRAINT `FK_detalle_sale_size_IdsizeFk` FOREIGN KEY (`IdsizeFk`) REFERENCES `size` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `detail_sale` */

/*Table structure for table `employee` */

DROP TABLE IF EXISTS `employee`;

CREATE TABLE `employee` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `na,e` varchar(50) NOT NULL,
  `IdPositionFk` int NOT NULL,
  `date_admision` date NOT NULL,
  `IdcityFk` int NOT NULL,
  PRIMARY KEY (`Id`),
  KEY `IX_empleado_IdCargoFk` (`IdPositionFk`),
  KEY `IX_empleado_IdcityFk` (`IdcityFk`),
  CONSTRAINT `FK_empleado_Cargos_IdCargoFk` FOREIGN KEY (`IdPositionFk`) REFERENCES `position` (`Id`),
  CONSTRAINT `FK_empleado_city_IdcityFk` FOREIGN KEY (`IdcityFk`) REFERENCES `city` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `employee` */

/*Table structure for table `garment` */

DROP TABLE IF EXISTS `garment`;

CREATE TABLE `garment` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL,
  `ValueUnitCop` double NOT NULL,
  `ValueUnitUsd` double NOT NULL,
  `Idproduction_stateFk` int NOT NULL,
  `IdTipoProteccion` int NOT NULL,
  `IdGeneroFk` int NOT NULL,
  `Code` varchar(50) NOT NULL DEFAULT '',
  PRIMARY KEY (`Id`),
  KEY `IX_prenda_Idproduction_stateFk` (`Idproduction_stateFk`),
  KEY `IX_prenda_IdGeneroFk` (`IdGeneroFk`),
  KEY `IX_prenda_IdTipoProteccion` (`IdTipoProteccion`),
  CONSTRAINT `FK_prenda_genero_IdGeneroFk` FOREIGN KEY (`IdGeneroFk`) REFERENCES `gender` (`Id`),
  CONSTRAINT `FK_prenda_production_state_Idproduction_stateFk` FOREIGN KEY (`Idproduction_stateFk`) REFERENCES `production_state` (`Id`),
  CONSTRAINT `FK_prenda_protection_type_IdTipoProteccion` FOREIGN KEY (`IdTipoProteccion`) REFERENCES `protection_type` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `garment` */

/*Table structure for table `gender` */

DROP TABLE IF EXISTS `gender`;

CREATE TABLE `gender` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `descripcion` varchar(50) NOT NULL,
  PRIMARY KEY (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `gender` */

/*Table structure for table `material` */

DROP TABLE IF EXISTS `material`;

CREATE TABLE `material` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL,
  `unit_value` double NOT NULL,
  `stock_min` double NOT NULL,
  `stock_max` double NOT NULL,
  PRIMARY KEY (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `material` */

/*Table structure for table `material_garment` */

DROP TABLE IF EXISTS `material_garment`;

CREATE TABLE `material_garment` (
  `IdmaterialFk` int NOT NULL,
  `IdGarmentFk` int NOT NULL,
  `amount` int NOT NULL,
  PRIMARY KEY (`IdGarmentFk`,`IdmaterialFk`),
  KEY `IX_material_prendas_IdmaterialFk` (`IdmaterialFk`),
  CONSTRAINT `FK_material_prendas_material_IdmaterialFk` FOREIGN KEY (`IdmaterialFk`) REFERENCES `material` (`Id`),
  CONSTRAINT `FK_material_prendas_prenda_IdGarmentFk` FOREIGN KEY (`IdGarmentFk`) REFERENCES `garment` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `material_garment` */

/*Table structure for table `material_supplier` */

DROP TABLE IF EXISTS `material_supplier`;

CREATE TABLE `material_supplier` (
  `IdmaterialFk` int NOT NULL,
  `IdProveedorFk` int NOT NULL,
  PRIMARY KEY (`IdmaterialFk`,`IdProveedorFk`),
  KEY `IX_material_proveedor_IdProveedorFk` (`IdProveedorFk`),
  CONSTRAINT `FK_material_proveedor_material_IdmaterialFk` FOREIGN KEY (`IdmaterialFk`) REFERENCES `material` (`Id`),
  CONSTRAINT `FK_material_proveedor_proveedor_IdProveedorFk` FOREIGN KEY (`IdProveedorFk`) REFERENCES `supplier` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `material_supplier` */

/*Table structure for table `order` */

DROP TABLE IF EXISTS `order`;

CREATE TABLE `order` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `date` date NOT NULL,
  `IdEmpleadoFk` int NOT NULL,
  `IdcustomerFk` int NOT NULL,
  `Idproduction_stateFk` int NOT NULL,
  PRIMARY KEY (`Id`),
  KEY `IX_orden_IdcustomerFk` (`IdcustomerFk`),
  KEY `IX_orden_IdEmpleadoFk` (`IdEmpleadoFk`),
  KEY `IX_orden_Idproduction_stateFk` (`Idproduction_stateFk`),
  CONSTRAINT `FK_orden_customer_IdcustomerFk` FOREIGN KEY (`IdcustomerFk`) REFERENCES `customer` (`Id`),
  CONSTRAINT `FK_orden_empleado_IdEmpleadoFk` FOREIGN KEY (`IdEmpleadoFk`) REFERENCES `employee` (`Id`),
  CONSTRAINT `FK_orden_production_state_Idproduction_stateFk` FOREIGN KEY (`Idproduction_stateFk`) REFERENCES `production_state` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `order` */

/*Table structure for table `payment_method` */

DROP TABLE IF EXISTS `payment_method`;

CREATE TABLE `payment_method` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `description` varchar(50) NOT NULL,
  PRIMARY KEY (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `payment_method` */

/*Table structure for table `person_type` */

DROP TABLE IF EXISTS `person_type`;

CREATE TABLE `person_type` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL,
  PRIMARY KEY (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `person_type` */

/*Table structure for table `position` */

DROP TABLE IF EXISTS `position`;

CREATE TABLE `position` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `description` varchar(50) NOT NULL,
  `salary` double NOT NULL,
  PRIMARY KEY (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `position` */

/*Table structure for table `production_state` */

DROP TABLE IF EXISTS `production_state`;

CREATE TABLE `production_state` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `description` varchar(50) NOT NULL,
  `IdTipoproduction_stateFk` int NOT NULL,
  PRIMARY KEY (`Id`),
  KEY `IX_production_state_IdTipoproduction_stateFk` (`IdTipoproduction_stateFk`),
  CONSTRAINT `FK_production_state_state_type_IdTipoproduction_stateFk` FOREIGN KEY (`IdTipoproduction_stateFk`) REFERENCES `state_type` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `production_state` */

/*Table structure for table `protection_type` */

DROP TABLE IF EXISTS `protection_type`;

CREATE TABLE `protection_type` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `description` varchar(50) NOT NULL,
  PRIMARY KEY (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `protection_type` */

/*Table structure for table `sale` */

DROP TABLE IF EXISTS `sale`;

CREATE TABLE `sale` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `date` date NOT NULL,
  `IdEmpleadoFk` int NOT NULL,
  `IdcustomerFk` int NOT NULL,
  `IdFormaPagoFk` int NOT NULL,
  PRIMARY KEY (`Id`),
  KEY `IX_sale_IdcustomerFk` (`IdcustomerFk`),
  KEY `IX_sale_IdEmpleadoFk` (`IdEmpleadoFk`),
  KEY `IX_sale_IdFormaPagoFk` (`IdFormaPagoFk`),
  CONSTRAINT `FK_sale_customer_IdcustomerFk` FOREIGN KEY (`IdcustomerFk`) REFERENCES `customer` (`Id`),
  CONSTRAINT `FK_sale_empleado_IdEmpleadoFk` FOREIGN KEY (`IdEmpleadoFk`) REFERENCES `employee` (`Id`),
  CONSTRAINT `FK_sale_forma_pago_IdFormaPagoFk` FOREIGN KEY (`IdFormaPagoFk`) REFERENCES `payment_method` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `sale` */

/*Table structure for table `size` */

DROP TABLE IF EXISTS `size`;

CREATE TABLE `size` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `description` varchar(50) NOT NULL,
  PRIMARY KEY (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `size` */

/*Table structure for table `state` */

DROP TABLE IF EXISTS `state`;

CREATE TABLE `state` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL,
  `IdcountryFk` int NOT NULL,
  PRIMARY KEY (`Id`),
  KEY `IX_state_IdcountryFk` (`IdcountryFk`),
  CONSTRAINT `FK_state_country_IdcountryFk` FOREIGN KEY (`IdcountryFk`) REFERENCES `country` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `state` */

/*Table structure for table `state_type` */

DROP TABLE IF EXISTS `state_type`;

CREATE TABLE `state_type` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `description` varchar(50) NOT NULL,
  PRIMARY KEY (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `state_type` */

/*Table structure for table `store` */

DROP TABLE IF EXISTS `store`;

CREATE TABLE `store` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `CodInv` varchar(255) NOT NULL,
  `IdPrendaFk` int NOT NULL,
  `ValueVtaCop` double NOT NULL,
  `ValueVtaUsd` double NOT NULL,
  PRIMARY KEY (`Id`),
  KEY `IX_insalerio_IdPrendaFk` (`IdPrendaFk`),
  CONSTRAINT `FK_insalerio_prenda_IdPrendaFk` FOREIGN KEY (`IdPrendaFk`) REFERENCES `garment` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `store` */

/*Table structure for table `store_size` */

DROP TABLE IF EXISTS `store_size`;

CREATE TABLE `store_size` (
  `IdStoreFk` int NOT NULL,
  `IdsizeFk` int NOT NULL,
  PRIMARY KEY (`IdStoreFk`,`IdsizeFk`),
  KEY `IX_insalerio_size_IdsizeFk` (`IdsizeFk`),
  CONSTRAINT `FK_insalerio_size_insalerio_IdStoreFk` FOREIGN KEY (`IdStoreFk`) REFERENCES `store` (`Id`),
  CONSTRAINT `FK_insalerio_size_size_IdsizeFk` FOREIGN KEY (`IdsizeFk`) REFERENCES `size` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `store_size` */

/*Table structure for table `supplier` */

DROP TABLE IF EXISTS `supplier`;

CREATE TABLE `supplier` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `NitSupp` varchar(50) NOT NULL,
  `name` varchar(50) NOT NULL,
  `IdTipoPersona` int NOT NULL,
  `IdcityFk` int NOT NULL,
  PRIMARY KEY (`Id`),
  UNIQUE KEY `IX_proveedor_NitSupp` (`NitSupp`),
  KEY `IX_proveedor_IdcityFk` (`IdcityFk`),
  KEY `IX_proveedor_IdTipoPersona` (`IdTipoPersona`),
  CONSTRAINT `FK_proveedor_city_IdcityFk` FOREIGN KEY (`IdcityFk`) REFERENCES `city` (`Id`),
  CONSTRAINT `FK_proveedor_person_type_IdTipoPersona` FOREIGN KEY (`IdTipoPersona`) REFERENCES `person_type` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

/*Data for the table `supplier` */

/*!40101 SET SQL_MODE=@OLD_SQL_MODE */;
/*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */;
/*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;
/*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;
```

