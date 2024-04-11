# [..](..)\Comandes\FMain

## Index

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
    - [function ZonaVisiblePermisos: Boolean](#function-zonavisiblepermisos-boolean)
    - [procedure GestioCRMClients](#procedure-gestiocrmclients)
    - [procedure GestioCobraments](#procedure-gestiocobraments)
    - [procedure InformeVendes](#procedure-informevendes)
- [Metodes](#metodes)
  - [function ComercialSeleccionat: Integer](#function-comercialseleccionat-integer)
  - [procedure AplicarFiltres(Sender: TObject = nil)](#procedure-aplicarfiltressender-tobject--nil)
  - [function ZonaSeleccionada: Integer](#function-zonaseleccionada-integer)
  - [function CapDeZonaSeleccionat: integer](#function-capdezonaseleccionat-integer)
  - [procedure EstablirControls](#procedure-establircontrols)
  - [procedure CrearFrame](#procedure-crearframe)
    - [function ObtenirComercialSegonsPersona(Pers: Integer): integer](#function-obtenircomercialsegonspersonapers-integer-integer)
  - [procedure ObrirFrame](#procedure-obrirframe)
    - [procedure ObrirFrameInformeVendes](#procedure-obrirframeinformevendes)
- [SQL](#sql)

## Descripcio

![][FMain]

## Interface

### published

```Delphi
procedure btnActualitzarClick(Sender: TObject);
procedure btnPermisosClick(Sender: TObject);
procedure btnConfiguracioClick(Sender: TObject);
procedure pcPreusClientsChange(Sender: TObject);
```

### private

```Delphi
fClient: Integer;
function ComercialSeleccionat: Integer;
procedure AplicarFiltres(Sender: TObject = nil);
function ZonaSeleccionada: Integer;
function CapDeZonaSeleccionat: integer;
procedure EstablirControls;
```

### public

```Delphi
procedure CrearFrame;
procedure ObrirFrame;
```

## Events

### procedure btnActualitzarClick(Sender: TObject)

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

### procedure btnPermisosClick(Sender: TObject)

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

### procedure btnConfiguracioClick(Sender: TObject)

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

### procedure pcPreusClientsChange(Sender: TObject)

Al canviar de pestanya (primera fila), si no estem a Gestio Clients ni a Informe Cobraments, veurem el panell de seleccionar la Zona.\
Si entrem a una de les pestanyes indicades s'executa un proces especific per a poder inicialitzar els valors.

![](Images/FMain_Pestanyes.png)

[ZonaVisiblePermisos](#function-zonavisiblepermisos-boolean)\
[InformeVendes](#procedure-informevendes)\
[GestioCobraments](#procedure-gestiocobraments)\
[GestioCRMClients](#procedure-gestiocrmclients)

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

#### function ZonaVisiblePermisos: Boolean

Defineix qui pot veure el desplegable de Zones.

[Permisos GestioComercial][PermisosGestioComercial]

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

#### procedure GestioCRMClients

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

#### procedure GestioCobraments

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

#### procedure InformeVendes

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

## Metodes

### function ComercialSeleccionat: Integer

```Delphi
function TFFMain.ComercialSeleccionat: Integer;
begin
  Result := cbComercial.EditValue;
end;
```

### procedure AplicarFiltres(Sender: TObject = nil)

dmPressupostos te una copia de totes les variables de Comercial, Zona i CapdeZona, per a poder filtrar les querys.

`cbCapZona.Properties.OnChange` no te cap event assignat, pero se li assigna [AplicarFiltres](#procedure-aplicarfiltressender-tobject--nil) en el metode [ObrirFrame](#procedure-obrirframe)

Si canvia el cap de zona assignem el comercial -1 (Tots els comercials) i rellegim comercials i zones.

[dmPressupostos.ObrirComercials]

[dmPressupostos.ObrirZones]

Es crida aquest metode per a que apliqui els canvis a certs frames.

[pcPreusClientsChange](#procedure-pcpreusclientschangesender-tobject)

Passem parametres a tots els Frames i els hi mandem actualitzar les seves dades.

```Delphi
procedure TFFMain.AplicarFiltres(Sender: TObject = nil);
begin
  dmPressupostos.Zona := ZonaSeleccionada;
  if CapDeZonaSeleccionat <> dmPressupostos.CapZona then
  begin
    dmPressupostos.CapZona := CapDeZonaSeleccionat;

    cbComercial.Properties.OnChange := nil;
    dmPressupostos.Comercial := -1;
    dmPressupostos.ObrirComercials;
    cbComercial.EditValue := dmPressupostos.Comercial;
    cbComercial.Properties.OnChange := cbCapZona.Properties.OnChange;

    cbZones.Properties.OnChange := nil;
    dmPressupostos.ObrirZones;
    cbZones.Properties.OnChange := cbCapZona.Properties.OnChange;
    cbZones.EditValue := dmPressupostos.Zona;
  end;
  pcPreusClientsChange(pcPreusClients);

  dmPressupostos.Comercial := ComercialSeleccionat;

  FFPressupostsClients1.Comercial := dmPressupostos.Comercial;
  FFPressupostsClients1.Actualitzar;
  FFTarifesEspecialsClients1.Comercial := dmPressupostos.Comercial;
  FFTarifesEspecialsClients1.Client := -1;
  FFTarifesEspecialsClients1.AplicarFiltres;
  FFLlistatPreus1.Comercial := dmPressupostos.Comercial;
  FFLlistatPreus1.Zona := dmPressupostos.Zona;
  FFEnviarWApp1.Comercial := dmPressupostos.Comercial;
  FFEnviarCorreu1.Comercial := dmPressupostos.Comercial;
  FFExcepcions1.Comercial := dmPressupostos.Comercial;
  dmPressupostos.LlegirClientsEx(dmPressupostos.Comercial, dmPressupostos.Zona,
    FFLlistatPreus1.ckClientsBaixa.Checked);
  FFGrupsClientsExcepcions1.Comercial := dmPressupostos.Comercial;
  FFExcepcionsClientsSolicitades1.Comercial := dmPressupostos.Comercial;
  FFExcepcionsClientsSolicitades1.Actualitzar;
end;
```

### function ZonaSeleccionada: Integer

```Delphi
function TFFMain.ZonaSeleccionada: integer;
begin
  Result := cbZones.EditValue;
end;
```

### function CapDeZonaSeleccionat: integer

```Delphi
function TFFMain.CapDeZonaSeleccionat: integer;
begin
  Result := cbCapZona.EditValue;
end;
```

### procedure EstablirControls

Degut de problemes amb el tipus de Usuari i la seva persistencia degut a que si obres el programa al mati i despres obres un altre programa diferent, et canviava el TipusUsuari guardat en el Registre i feia que perdessis els permisos. S'ha solventat guardant-lo i llegint-lo de DPressuposts.

[Taula de codis de permisos.][PermisosGestioComercial]

```Delphi
procedure TFFMain.EstablirControls;
var
  Usuari, NomTipusUsuari: string;
  TipusUsuari: integer;
begin
  // Segons els tipus d'usuari, determinem controls
  Usuari := LlegirUsuariReg;
  TipusUsuari := dmPressupostos.TipusUsuari;

  case TipusUsuari of
    USUARI_GESTIO_CLIENTS_ADMIN:
      NomTipusUsuari := 'ADMIN';
    USUARI_GESTIO_CLIENTS_SUPERVISOR:
      NomTipusUsuari := 'SUPERVISOR';
    USUARI_GESTIO_CLIENTS_CAP_ZONA:
      NomTipusUsuari := 'CAP ZONA';
    USUARI_GESTIO_CLIENTS_COMERCIAL:
      NomTipusUsuari := 'COMERCIAL';
  else
    NomTipusUsuari := 'CONSULTA';
  end;
  StatusBar1.Panels[0].Text := Usuari;
  StatusBar1.Panels[1].Text := IntToStr(TipusUsuari) + ' ' + NomTipusUsuari;

  case TipusUsuari of
    USUARI_GESTIO_CLIENTS_CONSULTA:
      begin
        pCapZona.Visible := True;
        pZones.Visible := True;
        pComercial.Visible := True;

        tsGestioCobraments.TabVisible := False;
        tsGestioCRMClients.TabVisible := False;
        tsGestioPressuposts.TabVisible := False;
        tsGestioTarifes.TabVisible := true;
        tsLlistatsPreus.TabVisible := false;
        tsTarifesPersonalitzades.TabVisible := false;
        tsExcepcions.TabVisible:= false;
        tsInformeVendes.TabVisible := False;
        tsExcepcionsGrups.Visible := False;
        tsAutoritzarPressuposts.TabVisible := False;
        tsPressuposts.TabVisible := False;
      end;
    USUARI_GESTIO_CLIENTS_COMERCIAL:
      begin
        pCapZona.Visible := False;
        pZones.Visible := True;
        pComercial.Visible := true;
        tsGestioCRMClients.TabVisible := False;

        tsGestioTarifes.TabVisible := True;
        tsGestioPressuposts.TabVisible := true;

        tsGestioCobraments.TabVisible := False;
        tsInformeVendes.TabVisible := False;
        {
        tsGestioCobraments.TabVisible := True;
        tsInformeVendes.TabVisible := true;
        }

        tsExcepcionsGrups.Visible := False;
        tsAutoritzarPressuposts.TabVisible := False;
        tsPressuposts.TabVisible := true;
      end;
    USUARI_GESTIO_CLIENTS_CAP_ZONA:
      begin
        pCapZona.Visible := true;
        pZones.Visible := True;
        pComercial.Visible := true;
        tsGestioCobraments.TabVisible := True;
        tsGestioCRMClients.TabVisible := true;
        tsGestioPressuposts.TabVisible := true;
        tsInformeVendes.TabVisible := true;
        tsGestioTarifes.TabVisible := True;
        tsExcepcionsGrups.Visible := False;
        tsAutoritzarPressuposts.TabVisible := False;
        tsPressuposts.TabVisible := True;
      end;
    USUARI_GESTIO_CLIENTS_ADMIN, USUARI_GESTIO_CLIENTS_SUPERVISOR:
      begin
        pCapZona.Visible := true;
        pZones.Visible := True;
        pComercial.Visible := true;
        tsGestioCobraments.TabVisible := True;
        tsGestioCRMClients.TabVisible := true;
        tsGestioPressuposts.TabVisible := true;
        tsInformeVendes.TabVisible := true;
        tsGestioTarifes.TabVisible := True;
        btnPermisos.Visible := True;
        tsExcepcionsGrups.Visible := True;
        tsAutoritzarPressuposts.TabVisible := true;
        tsPressuposts.TabVisible := true;
      end;
  else
    begin
      pCapZona.Visible := False;
      pZones.Visible := false;
      pComercial.Visible := false;
      tsGestioCobraments.TabVisible := false;
      tsGestioCRMClients.TabVisible := false;
      tsGestioPressuposts.TabVisible := false;
      tsInformeVendes.TabVisible := false;
      tsGestioTarifes.TabVisible := false;
      pTop.Visible := False;
    end
  end;
end;
```

### procedure CrearFrame

[EstablirControls](#procedure-establircontrols)

```Delphi
procedure TFFMain.CrearFrame;
begin
  // Segons els tipus d'usuari, determinem controls
  EstablirControls;

  // Segons el tipus d'usuari inicialitzem comercial i zona
  case dmPressupostos.TipusUsuari of
    USUARI_GESTIO_CLIENTS_COMERCIAL:
      begin
        dmPressupostos.Comercial := ObtenirComercialSegonsPersona(LlegirPersonaReg);
        dmPressupostos.ComercialInicial := dmPressupostos.Comercial;

        dmPressupostos.Zona := dmPressupostos.Comercial;
        dmPressupostos.ZonaInicial := dmPressupostos.Comercial;

        dmPressupostos.CapZona := -1;
        dmPressupostos.CapZonaInicial := -1;
      end;
    USUARI_GESTIO_CLIENTS_CAP_ZONA:
      begin
        dmPressupostos.Comercial := ObtenirComercialSegonsPersona(LlegirPersonaReg);
        dmPressupostos.ComercialInicial := dmPressupostos.Comercial;

        dmPressupostos.Zona := dmPressupostos.Comercial;
        dmPressupostos.ZonaInicial := dmPressupostos.Comercial;

        dmPressupostos.ObrirCapsDeZona;

        if dmPressupostos.quCapDeZones.Locate('CapComercial', dmPressupostos.Comercial,
          []) then
        begin
          dmPressupostos.CapZona := dmPressupostos.quCapDeZonesCapComercial.AsInteger;
          dmPressupostos.CapZonaInicial := dmPressupostos.quCapDeZonesCapComercial.AsInteger;
        end
        else
        begin
          dmPressupostos.CapZona := -1;
          dmPressupostos.CapZonaInicial := -1;
        end;
      end;
    USUARI_GESTIO_CLIENTS_SUPERVISOR:
      begin
        dmPressupostos.CapZona := -1;
        dmPressupostos.CapZonaInicial := -1;

        dmPressupostos.Comercial := -1;
        dmPressupostos.ComercialInicial := -1;

        dmPressupostos.Zona := -1;
        dmPressupostos.ZonaInicial := -1;
      end;
  else
    begin
      dmPressupostos.CapZona := -1;
      dmPressupostos.CapZonaInicial := -1;

      dmPressupostos.Comercial := -1;
      dmPressupostos.ComercialInicial := -1;

      dmPressupostos.Zona := -1;
      dmPressupostos.ZonaInicial := -1;
    end
  end;

  FFTarifesEspecialsClients1.CrearFrame;
  FFLlistatPreus1.CrearFrame;
  FFEnviarWApp1.CrearFrame;
  FFEnviarCorreu1.CrearFrame;
  FFExcepcions1.CrearFrame;
  FFPressupostsClients1.CrearFrame;
  FFExcepcionsClientsSolicitades1.CrearFrame;

  FFCRMClients1.CrearFrame;
  FFCRMClientsSeguimentCobraments1.CrearFrame;
end;
```

#### function ObtenirComercialSegonsPersona(Pers: Integer): integer

Ens retorna el codi de comercial segons la persona que li entrem.

```Delphi
function ObtenirComercialSegonsPersona(Pers: Integer): integer;
begin
  dmPressupostos.CapZona := -1;
  dmPressupostos.Comercial := -1;
  dmPressupostos.ObrirComercials;
  if dmPressupostos.quComercials.Locate('Persona', Pers, []) then
    Result := dmPressupostos.quComercialsComercial.AsInteger
  else
    Result := -1;
  dmPressupostos.quComercials.Close;
end;
```

### procedure ObrirFrame

[dmPressupostos.ObrirCapsDeZona]

[dmPressupostos.ObrirZones]

[dmPressupostos.ObrirComercials]

```Delphi
begin
  dmPressupostos.ObrirCapsDeZona;
  dmPressupostos.ObrirZones;
  dmPressupostos.ObrirComercials;

  cbCapZona.EditValue := dmPressupostos.CapZona;
  cbComercial.EditValue := dmPressupostos.Comercial;
  cbZones.EditValue := dmPressupostos.Zona;

  cbCapZona.Enabled := dmPressupostos.CapZona = -1;

  cbCapZona.Properties.OnChange := AplicarFiltres;
  cbZones.Properties.OnChange := cbCapZona.Properties.OnChange;
  cbComercial.Properties.OnChange := cbCapZona.Properties.OnChange;

  AplicarFiltres;

  FFTarifesEspecialsClients1.ObrirFrame;
  FFTarifesGenerals1.ObrirFrame;
  FFLlistatPreus1.ObrirFrame;
  FFEnviarWApp1.ObrirFrame;
  FFEnviarCorreu1.ObrirFrame;
  FFExcepcions1.ObrirFrame;
  FFGrupsClientsExcepcions1.ObrirFrame;
  FFExcepcionsClientsSolicitades1.ObrirFrame;
  FFPressupostsClients1.ObrirFrame;
  FFPressupostsAcceptarSolicituts1.ObrirFrame;
end;
```

#### procedure ObrirFrameInformeVendes

```Delphi
procedure ObrirFrameInformeVendes;
var
  Year, Month, Day: word;
begin
  DecodeDate(Date, Year, Month, Day);
  FFCRMClients1.deDataInici.Date := EncodeDate(Year, Month, 1);
  FFCRMClients1.deDataFi.Date := Date;
  FFCRMClients1.Comercial := ComercialSeleccionat;
  FFCRMClients1.NomComercial := cbComercial.Text;
  FFCRMClients1.Zona := ZonaSeleccionada;
  if FFCRMClients1.CapComercial <> -1 then
    Self.Caption := Self.Caption + ' - CAP DE ZONA';
end;
```

## SQL

[FMain]: Images/FMain.png
[PermisosGestioComercial]: GestioComercial.md#taula-de-permisos
[dmPressupostos.ObrirCapsDeZona]: DPressupostos.md#procedure-obrircapsdezona
[dmPressupostos.ObrirComercials]: DPressupostos.md#procedure-obrircomercials
[dmPressupostos.ObrirZones]: DPressupostos.md#procedure-obrirzones
