# [..](../..)\FlexyGO\Dashboards

## Index

- [Index](#index)
- [Descripcio](#descripcio)
- [SQL](#sql)
  - [Taules](#taules)
  - [Vistes](#vistes)
    - [vPers\_Dashboards\_extensio](#vpers_dashboards_extensio)
    - [vPers\_Dashboards\_OfertesDashBoards](#vpers_dashboards_ofertesdashboards)
  - [Stored Procedure](#stored-procedure)
    - [Pers\_dashboards\_CrearLinkOfertesDashboards(@Oferta int, @Dashboard int, @Departament int, @Grup int)](#pers_dashboards_crearlinkofertesdashboardsoferta-int-dashboard-int-departament-int-grup-int)
  - [Function](#function)
    - [ComprobarLinkOfertes(@Oferta int, @Dashboard int, @Departament int, @Grup int): bit](#comprobarlinkofertesoferta-int-dashboard-int-departament-int-grup-int-bit)

## Descripcio

Aquest projecte es tracta de una eina per mostrar a una serie de pantalles certa informacio. S'ha preparat d'una manera especial, per a que des de disseny es pugui gestionar facilment ofertes per a botigues.

Utilitzant la seguent vista podrem veure els dashboards disponibles: [vPers_Dashboards_extensio](#vpers_dashboards_extensio).
La vista la utilitzarem per saber un codi en base64 que usarem aixi: http://**IPSERVIDORIIS**:**PortIIS**/**AplicacioIIS**/Index#**BASE64QUERY**\
Exemple Ametller origen: [http://192.168.0.4:803/Dashboards/Index#eyJ0YXJnZXRpZCI6ImN1cnJlbnQiLCJuYXZpZ2F0ZUZ1biI6Im9wZW5wYWdlIiwib2JqZWN0bmFtZSI6IkRhc2hib2FyZCIsIm9iamVjdHdoZXJlIjoiKElkPScxJy...](http://192.168.0.4:803/Dashboards/Index#eyJ0YXJnZXRpZCI6ImN1cnJlbnQiLCJuYXZpZ2F0ZUZ1biI6Im9wZW5wYWdlIiwib2JqZWN0bmFtZSI6IkRhc2hib2FyZCIsIm9iamVjdHdoZXJlIjoiKElkPScxJykiLCJkZWZhdWx0cyI6Im51bGwiLCJwYWdldHlwZWlkIjoidmlldyIsImZpbHRlcnNWYWx1ZXMiOm51bGwsInByZXNldHNWYWx1ZXMiOm51bGwsInVzZXJpZCI6IjAiLCJwYWdlbmFtZSI6Ijk4Mjg0MDg4LUFERDktNEZCRC04Q0MwLTc1RDQ3QjMyNDJCRSJ9)

A la seguent imatge podem veure un exemple, el qual veiem un Header i un Footer definits per el departament i 2 ofertes, una composta de 3 articles i l'altre composta per 2 les quals es van intercanviant cada 20 segons.

![][DashboardsExampleSlides]

Estructura utilitzada:

1. Header Grup
2. Header Departament
3. Header Dashboard
4. Ofertes en format Slide
5. Footer Dashboard
6. Footer Departament
7. Footer Grup

## SQL

### Taules

![][Dashboards ER]

### Vistes

#### vPers_Dashboards_extensio

```SQL
ALTER view [dbo].[vPers_Dashboards_extensio] as

SELECT
    Id_dashboard, Dashboard,Id_Dept,Departament, CAST(N'' AS XML).value(
          'xs:base64Binary(xs:hexBinary(sql:column("bin")))'
        , 'VARCHAR(MAX)'
    )   Base64Encoding
FROM (
    SELECT 
	dash.Id Id_dashboard, dash.Descripcio as Dashboard, dept.Id Id_Dept, dept.Descripcio as Departament,
	CAST('{"targetid":"current","navigateFun":"openpage","objectname":"Dashboard","objectwhere":"(Id='''+convert(varchar,dash.Id)+''')","defaults":"null","pagetypeid":"view","filtersValues":null,"presetsValues":null,"userid":"0","pagename":"98284088-ADD9-4FBD-8CC0-75D47B3242BE"}' AS VARBINARY(MAX)) AS bin 
	from Pers_Dashboards_Dashboards dash
	left join Pers_Dashboards_Departaments dept on dept.Id = dash.departament_id
) AS bin_sql_server_temp;
```

#### vPers_Dashboards_OfertesDashBoards

```SQL
ALTER view [dbo].[vPers_Dashboards_OfertesDashBoards] as
select * from (
Select -2 Ordre,Id departamentId, null as DashboardId, HTMLHeader HTML,
'' Img1,'' Img2,'' Img3,'' Img4,
'' Titol1,'' Titol2,'' Titol3,'' Titol4,
'' Subtitol1,'' Subtitol2,'' Subtitol3,'' Subtitol4,
0 Preu1,0 Preu2,0 Preu3,0 Preu4,
'' Mesura1,'' Mesura2,'' Mesura3,'' Mesura4 from Pers_Dashboards_Grups
union all
Select -1 Ordre,Id departamentId, null as DashboardId, HTMLHeader HTML,
'' Img1,'' Img2,'' Img3,'' Img4,
'' Titol1,'' Titol2,'' Titol3,'' Titol4,
'' Subtitol1,'' Subtitol2,'' Subtitol3,'' Subtitol4,
0 Preu1,0 Preu2,0 Preu3,0 Preu4,
'' Mesura1,'' Mesura2,'' Mesura3,'' Mesura4 from Pers_Dashboards_Departaments
union all
Select 0, departament_id departamentId, Id as DashboardId, HTMLHeader HTML,
'' Img1,'' Img2,'' Img3,'' Img4,
'' Titol1,'' Titol2,'' Titol3,'' Titol4,
'' Subtitol1,'' Subtitol2,'' Subtitol3,'' Subtitol4,
0 Preu1,0 Preu2,0 Preu3,0 Preu4,
'' Mesura1,'' Mesura2,'' Mesura3,'' Mesura4  from Pers_Dashboards_Dashboards
union all
Select 1, 2,Id, '<div class=''slideshow-container''><div class=''slides''>' HTML, '' Img1,'' Img2,'' Img3,'' Img4,
'' Titol1,'' Titol2,'' Titol3,'' Titol4,
'' Subtitol1,'' Subtitol2,'' Subtitol3,'' Subtitol4,
0 Preu1,0 Preu2,0 Preu3,0 Preu4,
'' Mesura1,'' Mesura2,'' Mesura3,'' Mesura4 from Pers_Dashboards_Dashboards
union all
	select 2,od.departament_id as DepartmentId,od.Dashboard_Id as DashboardId, '<div class="slide" style="padding:0px">' + HTML + '</div>' as HTML
      ,[Img1]
      ,[Img2]
      ,[Img3]
      ,[Img4]
      ,[Titol1]
      ,[Titol2]
      ,[Titol3]
      ,[Titol4]
      ,[Subtitol1]
      ,[Subtitol2]
      ,[Subtitol3]
      ,[Subtitol4]
      ,[Preu1]
      ,[Preu2]
      ,[Preu3]
      ,[Preu4]
      ,[Mesura1]
      ,[Mesura2]
      ,[Mesura3]
      ,[Mesura4]
	
	from Pers_Dashboards_Ofertes o
	left join Pers_Dashboards_Oferta_Dashboard od on o.Id = od.Oferta_Id
	where DATEADD(dd, 0, DATEDIFF(dd, 0, GETDATE())) /*Data Actual*/ BETWEEN DataInici and DataFi and od.Departament_Id is not null
union all
Select 3, 2,Id, '</div></div>' HTML, '' Img1,'' Img2,'' Img3,'' Img4,
'' Titol1,'' Titol2,'' Titol3,'' Titol4,
'' Subtitol1,'' Subtitol2,'' Subtitol3,'' Subtitol4,
0 Preu1,0 Preu2,0 Preu3,0 Preu4,
'' Mesura1,'' Mesura2,'' Mesura3,'' Mesura4 from Pers_Dashboards_Dashboards
union all
Select 4,departament_id departamentId, Id as DashboardId, HTMLFooter HTML,
'' Img1,'' Img2,'' Img3,'' Img4,
'' Titol1,'' Titol2,'' Titol3,'' Titol4,
'' Subtitol1,'' Subtitol2,'' Subtitol3,'' Subtitol4,
0 Preu1,0 Preu2,0 Preu3,0 Preu4,
'' Mesura1,'' Mesura2,'' Mesura3,'' Mesura4  from Pers_Dashboards_Dashboards
union all
Select 5,Id departamentId, null as DashboardId, HTMLFooter HTML,
'' Img1,'' Img2,'' Img3,'' Img4,
'' Titol1,'' Titol2,'' Titol3,'' Titol4,
'' Subtitol1,'' Subtitol2,'' Subtitol3,'' Subtitol4,
0 Preu1,0 Preu2,0 Preu3,0 Preu4,
'' Mesura1,'' Mesura2,'' Mesura3,'' Mesura4  from Pers_Dashboards_Departaments
union all
Select 6 Ordre,Id departamentId, null as DashboardId, HTMLFooter HTML,
'' Img1,'' Img2,'' Img3,'' Img4,
'' Titol1,'' Titol2,'' Titol3,'' Titol4,
'' Subtitol1,'' Subtitol2,'' Subtitol3,'' Subtitol4,
0 Preu1,0 Preu2,0 Preu3,0 Preu4,
'' Mesura1,'' Mesura2,'' Mesura3,'' Mesura4 from Pers_Dashboards_Grups) a
where HTML is not null
union all
Select 7, 2,Id, '<script> let slideIndex = 0;
const slides = document.querySelectorAll(''.slides .slide'');

function showSlides() {
  for (let i = 0; i < slides.length; i++) {slides[i].style.display = ''none'';}
  slideIndex++;
  if (slideIndex > slides.length) {slideIndex = 1}
  slides[slideIndex - 1].style.display = ''block'';
  setTimeout(showSlides, 20000); // Change image every 2 seconds
}

showSlides(); </script>' HTML, '' Img1,'' Img2,'' Img3,'' Img4,
'' Titol1,'' Titol2,'' Titol3,'' Titol4,
'' Subtitol1,'' Subtitol2,'' Subtitol3,'' Subtitol4,
0 Preu1,0 Preu2,0 Preu3,0 Preu4,
'' Mesura1,'' Mesura2,'' Mesura3,'' Mesura4 from Pers_Dashboards_Dashboards
```

### Stored Procedure

#### Pers_dashboards_CrearLinkOfertesDashboards(@Oferta int, @Dashboard int, @Departament int, @Grup int)

Crea la relacio entre una oferta i un dashboard o un departament o Grup.
Abans de fer la relacio, verifica que no existeixi ja i en cas de existir, informa que ja existeix.

```SQL
ALTER PROCEDURE [dbo].[Pers_dashboards_CrearLinkOfertesDashboards]
	-- Add the parameters for the stored procedure here
	@Oferta  int,
	@Dashboard int = null, 
	@Departament int = null,
	@Grup int
AS
BEGIN
	DECLARE @LinkFet int;
    Set @LinkFet = (select dbo.ComprobarLinkOfertes(@Oferta, @Dashboard, @Departament, @Grup));
	if @LinkFet = 0 
	begin

		insert into Pers_Dashboards_Oferta_Dashboard(Oferta_Id,Dashboard_Id,Departament_Id, Grup_Id) values(@Oferta, @Dashboard, @Departament,@Grup);
		Set @LinkFet = (select dbo.ComprobarLinkOfertes(@Oferta, @Dashboard, @Departament, @Grup));
		if @LinkFet = 1
		begin
			select 'Link fet!!' as SuccessMessage;
			return -1;
		end
	end
	else
	begin
		select 'Link ja existent' as WarningMessage;
		return 1;
	end
END
```

### Function

#### ComprobarLinkOfertes(@Oferta int, @Dashboard int, @Departament int, @Grup int): bit

Comproba que no existeixi el link passat per parametre.

```SQL
ALTER FUNCTION [dbo].[ComprobarLinkOfertes]
(
	@Oferta  int,
	@Dashboard int = null, 
	@Departament int = null,
	@Grup int
)
RETURNS bit
AS
BEGIN
  RETURN  (select case when count(*) > 0 then 1 else 0 end from pers_dashboards_oferta_dashboard 
		where oferta_id = @Oferta 
			and Grup_Id = @Grup
			and isnull(@Departament , -1) = isnull(Departament_Id , -1)
			and isnull(@dashboard , -1) = isnull(dashboard_id , -1))

END
```

[Dashboards ER]: Images/Dashboards%20ER.png
[DashboardsExampleSlides]: Images/Dashboards%20Slides.gif
