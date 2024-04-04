# [..](..)\Comandes

## FMain

### Index

- [Index](#index)
- [Descripcio](#descripcio)
- [Interface](#interface)
  - [published](#published)
  - [private](#private)
  - [public](#public)
- [Events](#events)
  - [procedure btnActualitzarClick(Sender: TObject)](#procedure-btnactualitzarclicksender-tobject)
  - [procedure btnPermisosClick(Sender: TObject)](#procedure-btnpermisosclicksender-tobject)
  - [procedure btnConfiguracioClick(Sender: TObject)](#procedure-btnconfiguracioclicksender-tobject)
  - [procedure pcPreusClientsChange(Sender: TObject)](#procedure-pcpreusclientschangesender-tobject)
    - [pcPreusClientsChange\\ function ZonaVisiblePermisos: Boolean](#pcpreusclientschange-function-zonavisiblepermisos-boolean)
    - [pcPreusClientsChange\\ procedure GestioCRMClients](#pcpreusclientschange-procedure-gestiocrmclients)
    - [pcPreusClientsChange\\ procedure GestioCobraments](#pcpreusclientschange-procedure-gestiocobraments)
    - [pcPreusClientsChange\\ procedure InformeVendes](#pcpreusclientschange-procedure-informevendes)
- [Metodes](#metodes)
- [SQL](#sql)

### Descripcio

![][FMain]

### Interface

#### published

```Delphi
procedure btnActualitzarClick(Sender: TObject);
procedure btnPermisosClick(Sender: TObject);
procedure btnConfiguracioClick(Sender: TObject);
procedure pcPreusClientsChange(Sender: TObject);
```

#### private

```Delphi
fClient: Integer;
function ComercialSeleccionat: Integer;
procedure AplicarFiltres(Sender: TObject = nil);
function ZonaSeleccionada: Integer;
function CapDeZonaSeleccionat: integer;
procedure EstablirControls;
```

#### public

```Delphi
procedure CrearFrame;
procedure ObrirFrame;
```

### Events

#### procedure btnActualitzarClick(Sender: TObject)

```Delphi
procedure TFFMain.btnActualitzarClick(Sender: TObject);
begin
  AplicarFiltres;

  // Actualitzaci� de Pressupostos
  FFPressupostsClients1.Actualitzar;
  dmPressupostosPreus.LlegirLiniesPerAutoritzar;

  // Cridem com si haguessim canviat de pàgina per rellegir;
  pcPreusClientsChange(pcPreusClients);
end;
```

#### procedure btnPermisosClick(Sender: TObject)

```Delphi
procedure TFFMain.btnPermisosClick(Sender: TObject);
var
  f: TMFMantenimentPErmisos;
begin
  Application.CreateForm(TMFMantenimentPermisos, f);
  f.Connection := dmComandes.ADOConnection1;
  f.ShowModal;
end;
```

#### procedure btnConfiguracioClick(Sender: TObject)

```Delphi
procedure TFFMain.btnConfiguracioClick(Sender: TObject);
var
  f: TGFConfiguracioHorariComercial;
begin
  Application.CreateForm(TGFConfiguracioHorariComercial, f);
  f.Comercial := ComercialSeleccionat;
  f.ShowModal;
end;
```

#### procedure pcPreusClientsChange(Sender: TObject)

Al canviar de pestanya (primera fila), si no estem a Gestio Clients ni a Informe Cobraments, veurem el panell de seleccionar la Zona.\
Si entrem a una de les pestanyes indicades s'executa un proces especific per a poder inicialitzar els valors.

![](Images/FMain_Pestanyes.png)

[ZonaVisiblePermisos](#pcpreusclientschange-function-zonavisiblepermisos-boolean)\
[InformeVendes](#pcpreusclientschange-procedure-informevendes)\
[GestioCobraments](#pcpreusclientschange-procedure-gestiocobraments)\
[GestioCRMClients](#pcpreusclientschange-procedure-gestiocrmclients)

```Delphi
procedure TFFMain.pcPreusClientsChange(Sender: TObject);
begin
  pZones.Visible := //(pcPreusClients.ActivePage <> tsInformeVendes) and
    (pcPreusClients.ActivePage <> tsGestioCobraments) and (pcPreusClients.ActivePage
    <> tsGestioCRMClients) and ZonaVisiblePermisos;

  if pcPreusClients.ActivePage = tsInformeVendes then
    InformeVendes;
  if pcPreusClients.ActivePage = tsGestioCobraments then
    GestioCobraments;
  if pcPreusClients.ActivePage = tsGestioCRMClients then
    GestioCRMClients;
end;
```

##### pcPreusClientsChange\ function ZonaVisiblePermisos: Boolean

Defineix qui pot veure el desplegable de Zones.

```Delphi
function ZonaVisiblePermisos: Boolean;
begin
  case dmPressupostos.TipusUsuari of
    USUARI_GESTIO_CLIENTS_CONSULTA:
      Result := True;
    USUARI_GESTIO_CLIENTS_COMERCIAL:
      Result := true;
    USUARI_GESTIO_CLIENTS_CAP_ZONA:
      Result := True;
    USUARI_GESTIO_CLIENTS_ADMIN, USUARI_GESTIO_CLIENTS_SUPERVISOR:
      Result := True;
  else
    Result := false;
  end;
end;
```

##### pcPreusClientsChange\ procedure GestioCRMClients

Prepara el [FFCRMTasques](FFCRMTasques) i carrega les dades.

```Delphi
procedure GestioCRMClients;
begin
  FFCRMTasques1.CapZona := CapDeZonaSeleccionat;
  FFCRMTasques1.Comercial := ComercialSeleccionat;
  FFCRMTasques1.gbCapZona.Visible := False;
  FFCRMTasques1.cbComercial.Enabled := False;
  FFCRMTasques1.ObrirFrame;
  FFCRMTasques1.btActualitza.Click;
end;
```

##### pcPreusClientsChange\ procedure GestioCobraments

Prepara el [FFCRMClientsSeguimentCobraments](FFCRMClientsSeguimentCobraments) amb les dades necessaries. Si no hi ha cap comercial seleccionat, et fa fora.

```Delphi
procedure GestioCobraments;
var
  Year, Month, Day: word;
begin
// Visualitzem els cobraments
  DecodeDate(Date, Year, Month, Day);
  FFCRMClientsSeguimentCobraments1.deDataFi.Date := Date;
  FFCRMClientsSeguimentCobraments1.deDataVmtFi.Date := Date;
  FFCRMClientsSeguimentCobraments1.bbSortir.Visible := False;
  FFCRMClientsSeguimentCobraments1.paComercial.Visible := True;
  if (ComercialSeleccionat = -1) then
  begin
    ShowMessage('Selecciona un comercial per poder veure informe.');
    pcPreusClients.ActivePageIndex := 0;
  end
  else
  begin
    FFCRMClientsSeguimentCobraments1.Comercial := ComercialSeleccionat;
    FFCRMClientsSeguimentCobraments1.NomComercial := cbComercial.Text;
    FFCRMClientsSeguimentCobraments1.Zona := ComercialSeleccionat;
    FFCRMClientsSeguimentCobraments1.ObrirFrame;
  end;
end;
```

##### pcPreusClientsChange\ procedure InformeVendes

Prepara el [FFCRMClients](FFCRMClients) per a ser visionat.

```Delphi
procedure InformeVendes;
begin
  FFCRMClients1.CapComercial := CapDeZonaSeleccionat;
  FFCRMClients1.Comercial := ComercialSeleccionat;
  FFCRMClients1.Zona := ZonaSeleccionada;
  FFCRMClients1.paZona.Visible := False;
  FFCRMClients1.paComercial.Visible := False;
  FFCRMClients1.bbSortir.Visible := False;
  FFCRMClients1.DataInici := Date - 14;
  FFCRMClients1.DataFi := Date - 1;
  FFCRMClients1.deDataInici.Date := FFCRMClients1.DataInici;
  FFCRMClients1.deDataFi.Date := FFCRMClients1.DataFi;
  FFCRMClients1.PreusClients := true;
  FFCRMClients1.ObrirFrame;
end;
```

### Metodes

### SQL

[FMain]: Images/FMain.png
