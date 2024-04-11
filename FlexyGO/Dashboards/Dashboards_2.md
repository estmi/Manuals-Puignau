# [..](..\..)\FlexyGO\Dashboards

## Index

- [Index](#index)
- [Objectes FlexyGO](#objectes-flexygo)
  - [1. Dashboard (Pantalles)](#1-dashboard-pantalles)
  - [2. Departament (Arees)](#2-departament-arees)
  - [3. Grup (Grups)](#3-grup-grups)
  - [4. Oferta (Ofertes)](#4-oferta-ofertes)
  - [5. LinkOfertaDashboard (LinkOfertaDashboards)](#5-linkofertadashboard-linkofertadashboards)
- [Custom Controls](#custom-controls)
  - [combo\_departaments](#combo_departaments)
  - [combo\_Grups](#combo_grups)
  - [combo\_ofertes](#combo_ofertes)
  - [combo\_dashboards](#combo_dashboards)

## Objectes FlexyGO

### 1. Dashboard (Pantalles)

|Tipus|Nom|Titol|Icon|
|---|---|---|---|
|Objecte|Dashboard|{{Descripcio}}|![icon-cardiogram]cardiogram|
|Coleccio|Pantalles|Pantalles|![icon-cardiogram]cardiogram|

![PropertiesDashboards]

Departaments es un desplegable amb les seguents opcions: [Combo Departaments](#combo_departaments)

### 2. Departament (Arees)

|Tipus|Nom|Titol|Icon|
|---|---|---|---|
|Objecte|Departament|{{Descripcio}}|![icon-departments]departments|
|Coleccio|Arees|Arees|![icon-departments]departments|

![PropertiesArea]

Grups es un desplegable amb les seguents opcions: [Combo Grups](#combo_grups)

### 3. Grup (Grups)

|Tipus|Nom|Titol|Icon|
|---|---|---|---|
|Objecte|Grup|{{Descrip}}|![icon-group-2]group-2|
|Coleccio|Grups|Grups|![icon-group-2]group-2|

![PropertiesGrups]

### 4. Oferta (Ofertes)

|Tipus|Nom|Titol|Icon|
|---|---|---|---|
|Objecte|Oferta|{{Descripcio}}|![icon-document-image]document-image|
|Coleccio|Ofertes|Ofertes|![icon-document-image]document-image|

![PropertiesOferta1]
![PropertiesOferta2]
![PropertiesOferta3]

### 5. LinkOfertaDashboard (LinkOfertaDashboards)

|Tipus|Nom|Titol|Icon|
|---|---|---|---|
|Objecte|LinkOfertaDashboard|{{Oferta_Id}} - {{Dashboard_Id}}|![flx-link]flx-link|
|Coleccio|LinksOfertesDashboards|LinksOfertesDashboards|![flx-link]flx-link|

![PropertiesLink]

Oferta es un desplegable amb les seguents opcions: [Combo Oferta](#combo_departaments)\
Departament es un desplegable amb les seguents opcions: [Combo Departament](#combo_departaments)\
Dashboard es un desplegable amb les seguents opcions: [Combo Pantalla](#combo_departaments)

## Custom Controls

### combo_departaments

Type: DbCombo

Select de les dades

```SQL
SELECT Id
      ,Descripcio
      ,IdGrup
  FROM Pers_Dashboards_Departaments
```

Template de les dades:

```HTML
<div>{{Id}} - {{Descripcio}}</div>
```

SQL Value Field: `Id`
SQL Display Field: `Descripcio`

### combo_Grups

Type: DbCombo

Select de les dades

```SQL
Select Id, Descrip from Pers_Dashboards_Grups
```

Template de les dades:

```HTML
<div>{{Id}} - {{Descrip}}</div>
```

SQL Value Field: `Id`
SQL Display Field: `Descrip`

### combo_ofertes

Type: DbCombo

Select de les dades

```SQL
SELECT Id
      ,Descripcio
  FROM Pers_Dashboards_Ofertes
```

Select de les dades en mode edicio:

```SQL
SELECT Id
      ,Descripcio
  FROM Pers_Dashboards_Ofertes
  where DATEADD(dd, 0, DATEDIFF(dd, 0, GETDATE())) /*Data Actual*/ BETWEEN DataInici and DataFi
```

Template de les dades:

```HTML
<div>{{Id}} - {{Descripcio}}</div>
```

SQL Value Field: `Id`
SQL Display Field: `Descripcio`

### combo_dashboards

Type: DbCombo

Select de les dades

```SQL
Select Id, Descripcio,departament_id from Pers_Dashboards_Dashboards
```

Template de les dades:

```HTML
<div>{{Id}} - {{Descripcio}}</div>
```

SQL Value Field: `Id`
SQL Display Field: `Descripcio`

[icon-cardiogram]: Images/icon-cardiogram-16_x_16.jpg
[icon-departments]: Images/departments-16_x_16.jpg
[icon-group-2]: Images/group-2-16_x_16.jpg
[icon-document-image]: Images/document-image-16_x_16.jpg
[flx-link]: Images/flx-link-16_x_16.jpg

[PropertiesDashboards]: Images/Properties_Dashboard.png
[PropertiesArea]:Images/PropertiesArea.png
[PropertiesGrups]:Images/PropertiesGrup.png
[PropertiesLink]:Images/PropertiesLink.png
[PropertiesOferta1]:Images/PropertiesOferta1.png
[PropertiesOferta2]:Images/PropertiesOferta2.png
[PropertiesOferta3]:Images/PropertiesOferta3.png
