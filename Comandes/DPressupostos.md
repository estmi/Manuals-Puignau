# [..](..)\Comandes

## DPressupostos

### Index

- [Index](#index)
- [Descripcio](#descripcio)
- [Interface](#interface)
  - [published](#published)
  - [private](#private)
  - [public](#public)
- [Events](#events)
  - [procedure DataModuleCreate(Sender: TObject)](#procedure-datamodulecreatesender-tobject)
  - [procedure quArticlePreparaPressupostosNewRecord(DataSet: TDataSet)](#procedure-quarticlepreparapressupostosnewrecorddataset-tdataset)
- [Metodes](#metodes)
  - [procedure EliminarTarifaEspecial(id: Integer)](#procedure-eliminartarifaespecialid-integer)
  - [procedure LlegirPressupostos(comercial, Client, Tipus: Integer; actiu: Boolean = false)](#procedure-llegirpressupostoscomercial-client-tipus-integer-actiu-boolean--false)
  - [procedure LlegirClientsEx(\_comercial: Integer; \_Zona: Integer; ClientsBaixa: Boolean)](#procedure-llegirclientsex_comercial-integer-_zona-integer-clientsbaixa-boolean)
  - [procedure EliminarTarifaEspecialActual](#procedure-eliminartarifaespecialactual)
  - [procedure CopiarTarifaPers(TarifaPersOld, ClientNew: Integer; DataInici, DataFi: TDateTime)](#procedure-copiartarifaperstarifapersold-clientnew-integer-datainici-datafi-tdatetime)
  - [procedure CrearClientPlantillesComercial(comercial: Integer)](#procedure-crearclientplantillescomercialcomercial-integer)
  - [procedure ObrirEmail(Client: Integer) \[DEPRECATED\]](#procedure-obriremailclient-integer-deprecated)
  - [procedure ObrirPressupost(Pressupost: Integer; Data: TDateTime)](#procedure-obrirpressupostpressupost-integer-data-tdatetime)
    - [ObrirPressupost\\ procedure CarregarEmails \[DEPRECATED\]](#obrirpressupost-procedure-carregaremails-deprecated)
    - [ObrirPressupost\\ procedure CarregarDetallPressupost](#obrirpressupost-procedure-carregardetallpressupost)
    - [ObrirPressupost\\ procedure CarregarDetallPressupostNoArticlePrepara](#obrirpressupost-procedure-carregardetallpressupostnoarticleprepara)
  - [procedure ActualitzarPressupost](#procedure-actualitzarpressupost)
    - [ActualitzarPressupost\\ procedure EliminarDetallPressupost](#actualitzarpressupost-procedure-eliminardetallpressupost)
    - [ActualitzarPressupost\\ procedure ObrirDetallPressupost](#actualitzarpressupost-procedure-obrirdetallpressupost)
    - [ActualitzarPressupost\\ procedure AfegirLiniesDataDetallPressupost](#actualitzarpressupost-procedure-afegirliniesdatadetallpressupost)
    - [ActualitzarPressupost\\ procedure AfegirLiniesCompletDetallPressupost \[DEPRECATED\]](#actualitzarpressupost-procedure-afegirliniescompletdetallpressupost-deprecated)
  - [function EsAdministrador: Boolean](#function-esadministrador-boolean)
  - [procedure ObrirComercials](#procedure-obrircomercials)
  - [procedure ObrirZones](#procedure-obrirzones)
  - [procedure ObrirCapsDeZona](#procedure-obrircapsdezona)
  - [procedure LlegirPlantilles(comercial: Integer; Tipus: Integer = -1)](#procedure-llegirplantillescomercial-integer-tipus-integer---1)
- [SQL](#sql)
  - [SQL\\ quArticlePreparaPressupostos](#sql-quarticlepreparapressupostos)
  - [SQL\\ quDeletePressupost](#sql-qudeletepressupost)
  - [SQL\\ quClients](#sql-quclients)
  - [SQL\\ quCopiarPressupost](#sql-qucopiarpressupost)
  - [SQL\\ quCrearClientPlantilles](#sql-qucrearclientplantilles)
  - [SQL\\ quEmail](#sql-quemail)
  - [SQL\\ quDetallPressupost](#sql-qudetallpressupost)
  - [SQL\\ quDetallPressupostNOArticlePrepara](#sql-qudetallpressupostnoarticleprepara)
  - [SQL\\ quDetallPressupostInsert](#sql-qudetallpressupostinsert)
  - [SQL\\ quDetallPressupostDelete](#sql-qudetallpressupostdelete)
  - [SQL\\ quComercials](#sql-qucomercials)
    - [Exemple Comercials](#exemple-comercials)
  - [SQL\\ quZones](#sql-quzones)
    - [Exemple Zones](#exemple-zones)
  - [SQL\\ quCapDeZones](#sql-qucapdezones)
  - [SQL\\ quPlantilles](#sql-quplantilles)
  - [SQL\\ quDetallPlantilles](#sql-qudetallplantilles)
  - [SQL\\ quDetallPressupostConfig \[DEPRECATED\]](#sql-qudetallpressupostconfig-deprecated)

<!-- markdownlint-disable-next-line MD033 -->
<div class="page"/>

### Descripcio

DataModule utilitzat en el programa PGestioComercial. Tot el que es diu pressupost en aquest DataModule es refereix a les **Tarifes Personalitzades**.

### Interface

#### published

[DataModuleCreate](#procedure-datamodulecreatesender-tobject)\
[quArticlePreparaPressupostosNewRecord](#procedure-quarticlepreparapressupostosnewrecorddataset-tdataset)

```Delphi
procedure DataModuleCreate(Sender: TObject);
procedure quArticlePreparaPressupostosNewRecord(DataSet: TDataSet);
```

#### private

[EliminarTarifaEspecial](#procedure-eliminartarifaespecialid-integer)

```Delphi
FPersona: Integer;
FComercial: Integer;
FZona: Integer;
FCapZona: Integer;
FComercialInicial: Integer;
FZonaInicial: Integer;
FCapZonaInicial: Integer;
FTipusUsuari: Integer;
procedure EliminarTarifaEspecial(id: Integer);
```

#### public

[LlegirPressupostos](#procedure-llegirpressupostoscomercial-client-tipus-integer-actiu-boolean--false)\
[LlegirClientsEx](#procedure-llegirclientsex_comercial-integer-_zona-integer-clientsbaixa-boolean)\
[EliminarTarifaEspecialActual](#procedure-eliminartarifaespecialactual)\
[CopiarTarifaPers](#procedure-copiartarifaperstarifapersold-clientnew-integer-datainici-datafi-tdatetime)\
[CrearClientPlantillesComercial](#procedure-crearclientplantillescomercialcomercial-integer)\
[ObrirEmail](#procedure-obriremailclient-integer-deprecated)\
[ObrirPressupost](#procedure-obrirpressupostpressupost-integer-data-tdatetime)\
[ActualitzarPressupost](#procedure-actualitzarpressupost)\
[EsAdministrador](#function-esadministrador-boolean)\
[ObrirComercials](#procedure-obrircomercials)\
[ObrirZones](#procedure-obrirzones)\
[ObrirCapsDeZona](#procedure-obrircapsdezona)\
[LlegirPlantilles](#procedure-llegirplantillescomercial-integer-tipus-integer---1)\

ComercialInicial (ref. [FMain][FMain])\
ZonaInicial (ref. [FMain][FMain])\
CapZonaInicial (ref. [FMain][FMain])\
Comercial (ref. [FMain][FMain])\
Zona (ref. [FMain][FMain])\
CapZona (ref. [FMain][FMain])\
TipusUsuari (ref. [FMain][FMain])

```Delphi
procedure LlegirPressupostos(comercial, Client, Tipus: Integer; actiu: Boolean = false);
procedure LlegirClientsEx(_comercial: Integer; _Zona: Integer; ClientsBaixa: Boolean);
property Persona: Integer read fPersona write fPersona;
procedure EliminarTarifaEspecialActual;
procedure CopiarTarifaPers(TarifaPersOld, ClientNew: Integer; DataInici, DataFi: TDateTime);
procedure CrearClientPlantillesComercial(comercial: Integer);
procedure ObrirEmail(Client: Integer);
procedure ObrirPressupost(Pressupost: Integer; Data: TDateTime);
procedure ActualitzarPressupost;
function EsAdministrador: Boolean;
procedure ObrirComercials;
procedure ObrirZones;
procedure ObrirCapsDeZona;
procedure LlegirPlantilles(comercial: Integer; Tipus: Integer = -1);
property ComercialInicial: Integer read FComercialInicial write FComercialInicial;
property ZonaInicial: Integer read FZonaInicial write FZonaInicial;
property CapZonaInicial: Integer read FCapZonaInicial write FCapZonaInicial;
property Comercial: Integer read fComercial write fComercial;
property Zona: Integer read FZona write FZona;
property CapZona: Integer read FCapZona write FCapZona;
property TipusUsuari: Integer read FTipusUsuari write FTipusUsuari;
```

### Events

#### procedure DataModuleCreate(Sender: TObject)

No caldria, ja que al iniciar l'App es fa la lectura de les dades amb els paramentres pertinents.
[quArticlePreparaPressupostos](#sql-quarticlepreparapressupostos)

```Delphi
procedure TdmPressupostos.DataModuleCreate(Sender: TObject);
begin
  quArticlePreparaPressupostos.Open;
end;
```

#### procedure quArticlePreparaPressupostosNewRecord(DataSet: TDataSet)

S'assigna el propietari de la tarifa personalitzada.

[quArticlePreparaPressupostos](#sql-quarticlepreparapressupostos)

```Delphi
procedure TdmPressupostos.quArticlePreparaPressupostosNewRecord(DataSet: TDataSet);
begin
  DataSet.FieldByName('Persona').Value := LlegirPersonaReg;
end;
```

### Metodes

#### procedure EliminarTarifaEspecial(id: Integer)

Elimina una tarifa personalitzada i totes les seves linies filles del detall.

[quDeletePressupost](#sql-qudeletepressupost)

```Delphi
procedure TdmPressupostos.EliminarTarifaEspecial(id: Integer);
begin
  with quDeletePressupost, Parameters do
  begin
    ParamByName('Pressupost').Value := id;
    ExecSQL;
  end;
end;
```

#### procedure LlegirPressupostos(comercial, Client, Tipus: Integer; actiu: Boolean = false)

Llegir Tarifes personalitzades.\
Paramestres:

- Client:
  - -1 Tots els clients
  - -2 Totes les plantilles Comercials (Clients -10(Codi Comercial) ex. Joan Horta -104)
  - -100 Plantilles Generals
  - Codi Client
- Actiu:
  - 0 Tot
  - 1 Nomes DataInici <= @Today <= DataFi
- Tipus: Ens indica si nomes volem veure Fresc, Congelat o Llotja

[quArticlePreparaPressupostos](#sql-quarticlepreparapressupostos)

```Delphi
procedure TdmPressupostos.LlegirPressupostos(comercial, Client, Tipus: Integer; actiu: Boolean = false);
begin
  with quArticlePreparaPressupostos, Parameters do
  begin
    Close;
    ParamByName('Comercial').Value := comercial;
    ParamByName('Client').Value := Client;
    ParamByName('Zona').Value := Zona;
    ParamByName('CapComercial').Value := CapZona;
    if actiu then
      ParamByName('Actiu').Value := 1
    else
      ParamByName('Actiu').Value := 0;
    Open;
  end;

  if Tipus <> -1 then
  begin
    quArticlePreparaPressupostos.Filtered := false;
    quArticlePreparaPressupostos.Filter := 'Tipus = ' + inttostr(Tipus);
    quArticlePreparaPressupostos.Filtered := True;
  end
  else
  begin
    quArticlePreparaPressupostos.Filter := '';
    quArticlePreparaPressupostos.Filtered := false;
  end;
end;
```
<!-- markdownlint-disable-next-line MD037 -->
#### procedure LlegirClientsEx(_comercial: Integer; _Zona: Integer; ClientsBaixa: Boolean)

Si ens passen tots els parametres a -1, si el CapZona = -1 i CapZonaInicial = -1 ens indica que es un comercial sense permis, per el qual nomes ha de poder veure la seva zona i comercial.
[quClients](#sql-quclients)

```Delphi
procedure TdmPressupostos.LlegirClientsEx(_comercial: Integer; _Zona: Integer;
  ClientsBaixa: Boolean);
begin
  if (comercial = -1) and (Zona = -1) and (CapZona = -1) and (CapZonaInicial = -1) then
  begin
    _comercial := ComercialInicial;
    _Zona := ZonaInicial;
  end;

  with quClients, Parameters do
  begin
    DisableControls;
    Close;
    ParamByName('Data').Value := Trunc(Date);
    ParamByName('Comercial').Value := _comercial;
    ParamByName('Zona').Value := _Zona;
    if ClientsBaixa then
      ParamByName('ClientsBaixa').Value := -1
    else
      ParamByName('ClientsBaixa').Value := 0;
    ParamByName('CapComercial').Value := CapZona;
    Open;
    EnableControls;
  end;
end;
```

#### procedure EliminarTarifaEspecialActual

Elimina la tarifa personalitzada i totes les seves linies filles del detall.

[EliminarTarifaEspecial](#procedure-eliminartarifaespecialid-integer)

```Delphi
procedure TdmPressupostos.EliminarTarifaEspecialActual;
begin
  EliminarTarifaEspecial(quArticlePreparaPressupostosId.Value);
end;
```

#### procedure CopiarTarifaPers(TarifaPersOld, ClientNew: Integer; DataInici, DataFi: TDateTime)

Copia les dades d'una **TarifaPersOld** a un **ClientNew** amb les dates seleccionades.

[quCopiarPressupost](#sql-qucopiarpressupost)

```Delphi
procedure TdmPressupostos.CopiarTarifaPers(TarifaPersOld, ClientNew: Integer;
  DataInici, DataFi: TDateTime);
begin
  quArticlePreparaPressupostos.Cancel;

  with quCopiarPressupost, Parameters do
  begin
    ParamByName('Client').Value := ClientNew;

    if DataInici > 0 then
      ParamByName('DataInici').Value := DataInici;

    if DataFi > 0 then
      ParamByName('DataFi').Value := DataInici;

    ParamByName('TarifaBase').Value := TarifaPersOld;
    ParamByName('Persona').Value := LlegirPersonaReg;
    ExecSQL;
  end;

  quArticlePreparaPressupostos.Requery;
end;
```

#### procedure CrearClientPlantillesComercial(comercial: Integer)

Comproba i crea un client Plantilles pel comercial.\
Aquest client te com a funcio guardar plantilles per a despres poder copiar cap a un client.

```Delphi
procedure TdmPressupostos.CrearClientPlantillesComercial(comercial: Integer);
begin
  with quCrearClientPlantilles, Parameters do
  begin
    ParamByName('Comercial').Value := comercial;

    ExecSQL;
  end;
end;
```

#### procedure ObrirEmail(Client: Integer) [DEPRECATED]

DEPRECATED.
Nomes te un DataSource assignat i no s'utilitza per a res, segurament es podria eliminar.

[quEmail](#sql-quemail)

```Delphi
procedure TdmPressupostos.ObrirEmail(Client: Integer);
var
  current: Integer;
begin
  quEmail.Open;
  with quEmail, Parameters do
  begin
    Cancel;

    if not Locate('Client', Client, [loPartialKey]) then
    begin
      Append;
      quEmailClient.Value := Client;
    end
    else
      Edit;
  end;
end;
```

#### procedure ObrirPressupost(Pressupost: Integer; Data: TDateTime)

Llegeix i carrega les dades d'una **Tarifa Pers**.

[CarregarEmails](#obrirpressupost-procedure-carregaremails-deprecated)
[CarregarDetallPressupost](#obrirpressupost-procedure-carregardetallpressupost)
[CarregarDetallPressupostNoArticlePrepara](#obrirpressupost-procedure-carregardetallpressupostnoarticleprepara)

```Delphi
procedure TdmPressupostos.ObrirPressupost(Pressupost: Integer; Data: TDateTime);
begin
  CarregarEmails;
  CarregarDetallPressupost;
  CarregarDetallPressupostNoArticlePrepara;
end;
```

##### ObrirPressupost\ procedure CarregarEmails [DEPRECATED]

DEPRECATED.

[ObrirEmail](#procedure-obriremailclient-integer-deprecated)

```Delphi
procedure CarregarEmails;
begin
  ObrirEmail(quArticlePreparaPressupostosClient.Value);
end;
```

##### ObrirPressupost\ procedure CarregarDetallPressupost

Llegeix la Tarifa Personalitzada del servidor i la carrega a una taula en memoria.

dxMemData1:

- Actiu: TBooleanField
- Article: TStringField
- Client: TBooleanField
- Nom: TStringField
- Marca: TStringField
- ArticlePrepara: TBooleanField
- Familia: TStringField

[quDetallPressupost](#sql-qudetallpressupost)

```Delphi
procedure CarregarDetallPressupost;
begin
  quDetallPressupost.Close;
  quDetallPressupost.Parameters.FindParam('Pressupost').Value := Pressupost;
  quDetallPressupost.Parameters.FindParam('Data').Value := Trunc(Data);
  quDetallPressupost.Open;
  dxMemData1.Close;
  dxMemData1.Open;
  dxMemData1.DisableControls;
  try
    begin
      while not quDetallPressupost.Eof do
      begin
        dxMemData1.Append;
        dxMemData1Client.Value := quDetallPressupostClient.Value = 1;
        dxMemData1Article.Value := quDetallPressupostArticle.Value;
        dxMemData1Nom.Value := quDetallPressupostNom.Value;
        dxMemData1Actiu.Value := quDetallPressupostActiu.Value;
        dxMemData1Marca.Value := quDetallPressupostNomMarca.Value;
        dxMemData1ArticlePrepara.Value := True;
        dxMemData1Familia.Value := quDetallPressupostFamilia.Value;
        dxMemData1.Post;
        quDetallPressupost.Next;
      end;
    end;
  finally
    dxMemData1.First;
    dxMemData1.EnableControls;
  end;
end;
```

##### ObrirPressupost\ procedure CarregarDetallPressupostNoArticlePrepara

Afegeix tots els articles que estan seleccionats a **Tarifa Personalitzada** i que no es troben a la Tarifa del dia.

[quDetallPressupostNOArticlePrepara](#sql-qudetallpressupostnoarticleprepara)

```Delphi
procedure CarregarDetallPressupostNoArticlePrepara;
begin
  quDetallPressupostNOArticlePrepara.Close;
  quDetallPressupostNOArticlePrepara.Parameters.FindParam('Pressupost').Value
    := Pressupost;
  quDetallPressupostNOArticlePrepara.Parameters.FindParam('Data').Value := Trunc(Data);
  quDetallPressupostNOArticlePrepara.Open;
  try
    begin
      dxMemData1.DisableControls;
      while not quDetallPressupostNOArticlePrepara.Eof do
      begin
        dxMemData1.Append;
        dxMemData1Client.Value := True;
        dxMemData1Article.Value := quDetallPressupostNOArticlePreparaArticle.Value;
        dxMemData1Nom.Value := quDetallPressupostNOArticlePreparaNomArticle.Value;
        dxMemData1Actiu.Value := false;
        dxMemData1Marca.Value := quDetallPressupostNOArticlePreparaNomMarca.Value;
        dxMemData1ArticlePrepara.Value := false;
        dxMemData1.Post;
        quDetallPressupostNOArticlePrepara.Next;
      end;
    end;
  finally
    dxMemData1.First;
    dxMemData1.EnableControls;
  end;
end;
```

#### procedure ActualitzarPressupost

Actualitza un pressupost, eliminant-ne el detall i creant-lo de nou.

[EliminarDetallPressupost](#actualitzarpressupost-procedure-eliminardetallpressupost)\
[AfegirLiniesDataDetallPressupost](#actualitzarpressupost-procedure-afegirliniesdatadetallpressupost)\
[AfegirLiniesCompletDetallPressupost DEPRECATED](#actualitzarpressupost-procedure-afegirliniescompletdetallpressupost-deprecated)\

```Delphi
procedure TdmPressupostos.ActualitzarPressupost;
begin
  // Primer eliminem completament tot el detall del pressupost
  EliminarDetallPressupost;

  // Afegir les línies marcades de la tarifa segons la data.
  AfegirLiniesDataDetallPressupost;

  // Ja no es fa servir la Grid aquesta
  // Afegir les línies completes de la tarifa que ja hi fossin.
  // AfegirLiniesCompletDetallPressupost;
end;
```

##### ActualitzarPressupost\ procedure EliminarDetallPressupost

Elimina les linies de la **Tarifa Pers**.

[quDetallPressupostDelete](#sql-qudetallpressupostdelete)

```Delphi
procedure EliminarDetallPressupost;
  begin
    quDetallPressupostDelete.Parameters.FindParam('Pressupost').Value :=
      quArticlePreparaPressupostosId.Value;
    quDetallPressupostDelete.ExecSQL;
  end;
```

##### ActualitzarPressupost\ procedure ObrirDetallPressupost

Obre un query utilitzat per a fer Append de les linies de la **Tarifa Pers**.

[quDetallPressupostInsert](#sql-qudetallpressupostinsert)

```Delphi
procedure ObrirDetallPressupost;
  begin
    quDetallPressupostInsert.Parameters.FindParam('Pressupost').Value :=
      quArticlePreparaPressupostosId.Value;
    quDetallPressupostInsert.Open;
  end;
```

##### ActualitzarPressupost\ procedure AfegirLiniesDataDetallPressupost

[ObrirDetallPressupost](#actualitzarpressupost-procedure-obrirdetallpressupost)

Recorrem tota la taula en memoria i en cas de estar inclos a la **Tarifa Personalitzada**, ho guardem a traves de la següent query: [quDetallPressupostInsert](#sql-qudetallpressupostinsert).

```Delphi
procedure AfegirLiniesDataDetallPressupost;
  begin
    try
      // Obrir la taula per fer les insercions d'articles.
      ObrirDetallPressupost;

      // Carregar les línies del detall segons pressupost data
      dxMemData1.First;
      dxMemData1.DisableControls;
      while not dxMemData1.Eof do
      begin
        if dxMemData1Client.Value then
        begin
          quDetallPressupostInsert.Append;
          quDetallPressupostInsertIdPressupost.Value :=
            quArticlePreparaPressupostosId.Value;
          quDetallPressupostInsertArticle.Value := dxMemData1Article.AsString;
          quDetallPressupostInsert.Post;
        end;
        dxMemData1.Next;
      end;

    finally
      quDetallPressupostInsert.Close;
      dxMemData1.EnableControls;
    end;
  end;
```

##### ActualitzarPressupost\ procedure AfegirLiniesCompletDetallPressupost [DEPRECATED]

DEPRECATED

```Delphi
procedure AfegirLiniesCompletDetallPressupost;
begin
  try
    // Obrir la taula per fer les insercions d'articles.
    ObrirDetallPressupost;
    // Carregar les línies del detall pressupost completes que NO existeixin
    // dins la data actual i que ja fossin a la llista completa
    taDetallPressupostConfig.First;
    while not taDetallPressupostConfig.Eof do
    begin
      if taDetallPressupostConfigClient.Value then
      begin
        if not quDetallPressupostInsert.Locate('Article',
          taDetallPressupostConfigArticle.AsString, []) then
        begin
          quDetallPressupostInsert.Append;
          quDetallPressupostInsertIdPressupost.Value :=
            quArticlePreparaPressupostosId.Value;
          quDetallPressupostInsertArticle.Value :=
            taDetallPressupostConfigArticle.AsString;
          quDetallPressupostInsert.Post;
        end;
      end;
      taDetallPressupostConfig.Next;
    end;
    quDetallPressupostInsert.Close;
  finally
  end;
end;
```

#### function EsAdministrador: Boolean

Comproba si l'usuari es administrador. **TipusUsuari**, es una property la qual es guarda al primer moment de entrar a la aplicacio.

USUARI_PREUS_CLIENT_RESPONSABLE = 30
USUARI_PREUS_PEIX_FRESC_ADMIN = 999

```Delphi
function TdmPressupostos.EsAdministrador: Boolean;
// Retorna cert si l'usuari �s administrador.
begin
  Result := (TipusUsuari = USUARI_PREUS_CLIENT_RESPONSABLE) or (TipusUsuari =
    USUARI_PREUS_PEIX_FRESC_ADMIN);
end;
```

#### procedure ObrirComercials

Llegeix Comercials.

Query utilitzat al desplegable superior de [FMain][FMain].

[quComercials](#sql-qucomercials)

```Delphi
procedure TdmPressupostos.ObrirComercials;
begin
  with quComercials, Parameters do
  begin
    Close;
    ParamByName('CapComercial').Value := FCapZona;
    ParamByName('Comercial').Value := FComercial;
    Open;
    First;
  end;
end;
```

#### procedure ObrirZones

Llegeix les Zones permeses.

Query utilitzat al desplegable superior de [FMain][FMain].

[quZones](#sql-quzones)

```Delphi
procedure ObrirZones;
begin
  with quZones, Parameters do
  begin
    Close;
    ParamByName('CapComercial').Value := FCapZona;
    ParamByName('Zona').Value := ZonaInicial;
    Open;
    First;
  end;
end;
```

#### procedure ObrirCapsDeZona

[quCapDeZones](#sql-qucapdezones)

```Delphi
procedure TdmPressupostos.ObrirCapsDeZona;
begin
  with quCapDeZones do
  begin
    Close;
    Open;
    First;
  end;
end;
```

#### procedure LlegirPlantilles(comercial: Integer; Tipus: Integer = -1)

Llegeix les plantilles a les que te acces un comercial per a poder utilitzar com a base a una **Tarifa Pers**.

S'utilitza a GFSeleccionarPlantilla el qual et permet seleccionar una plantilla o **Tarifa Pers**. Et llista a l'esquerra el primer query amb les capçaleres i a la dreta el detall de la plantilla.

[quPlantilles](#sql-quplantilles)\
[quDetallPlantilles](#sql-qudetallplantilles)

```Delphi
procedure TdmPressupostos.LlegirPlantilles(comercial: Integer; Tipus: Integer = -1);
begin
  with quPlantilles, Parameters do
  begin
    Close;

    ParamByName('Comercial').Value := comercial;
    ParamByName('Tipus').Value := Tipus;

    Open;
  end;

  with quDetallPlantilles do
  begin
    Close;

    Open;
  end;
end;
```

### SQL

#### SQL\ quArticlePreparaPressupostos

Query principal de tarifes personalitzades.

```SQL
Declare @Comercial integer, @Client integer, @Actiu integer, @Today DateTime, @Zona Integer, @CapComercial integer

Set @Comercial = IsNull(:Comercial, -1) -- -1 Tots els comercials / Codi Comercial
Set @Zona = IsNull(:Zona, -1) -- -1 Totes les Zones / Codi Zona
Set @CapComercial = IsNull(:CapComercial, -1)
Set @Client = IsNull(:Client, -1) -- -1 Tots els clients / -2 Totes les plantilles Comercials (Clients -10(Codi Comercial) ex. Joan Horta -104) / -100 Plantilles Generals / Codi Client
Set @Actiu = :Actiu -- 0 Tot / 1 Nomes DI <= @Today <= DF
Set @Today =  CONVERT(date, getdate())

SELECT app.[Id]
      ,app.[Client]
      ,c.Nom
      ,c.Comercial CComercial
      ,c.Zona CZona
      ,(Cast(c.Comercial as varchar(5)) + '-' + pc.Descripcio) as Comercial
      ,[DataInici]
      ,[DataFi]
      ,[Comentaris]
      ,app.Tipus
      ,c.PreClient
      ,app.Persona
  FROM [Puignau].[dbo].[ArticlePreparaPressupostos] app
  left join Puignau.dbo.Clients c on app.Client = c.Client
  left Join Puignau.dbo.PersonalComercial pc on pc.Comercial = c.Comercial
  left join Puignau.dbo.ZonesCapComercial zc on zc.Zona = c.Zona
Where
(
    @Client = -100
    or
    (
        (@Comercial = -1
            or @Comercial = c.Comercial)
        and
        (@Zona = -1
            or @Zona = c.Zona)
        and
        (@CapComercial = -1
            or @CapComercial = zc.CapComercial)
    )
)
and
(
    @Client = -1 and app.Client > 0
    or @Client = -2 and app.Client < -100
    or @Client = app.Client
)
and (
    @Actiu = 0
    or (
        (
          app.DataInici is null
		      or app.DataInici<=@Today
        )
	      and (
              app.DataFi is null
		          or app.DataFi>=@Today
            )
      )
    )
```

#### SQL\ quDeletePressupost

Elimina una tarifa personalitzada i totes les seves linies filles del detall.

```Delphi
Declare @Pressupost int

Set @Pressupost = :Pressupost

Delete from ArticlePreparaPressupostosLinies
where IdPressupost = @Pressupost;

Delete from ArticlePreparaPressupostos where id = @Pressupost;
```

#### SQL\ quClients

Ens retorna un llistat de tots els clients amb informacio sobre **Excepcions**, **Tarifes Personalitzades** i **programacio**.

Exemple:

|NComercial|Client|NClient|PreClient|TipusABC|NomZona|FrescEx|LlotjaEx|CongeEx|FeinaEx|FrescPers|LlotjaPers|CongePers|FrescProg|LlotjaProg|CongeProg|Zona|CapComercial|ObsCRM|
|---|---|---|:---:|:---:|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|---|---|
|Enric Auladell|3426|AJUNTAMENT DE GARRIGOLES|0|D|ENRIC AULADELL (5017)|X|0|0|0|0|0|0|0|0|0|17|9|NULL|
|Enric Auladell|3603|AJUNTAMENT DE VILAMACOLUM|0|D|ENRIC AULADELL (5017)|X|0|0|0|0|0|0|0|0|0|17|9|NULL|
|Enric Auladell|3916|ALBO,  RESTAURANT|0|D|ENRIC AULADELL (5017)|X|0|0|0|0|0|0|0|0|0|17|9|NULL|
|Enric Auladell|2695|AMB ELS 5 SENTITS|0|D|ENRIC AULADELL (5017)|X|0|0|0|0|0|0|X|0|0|17|9|NULL|
|Enric Auladell|1661|ANTIC CASINO RESTAURANT|0|B|ENRIC AULADELL (5017)|0|0|0|0|0|0|0|X|0|0|17|9|NULL|

```SQL
Declare @Data Datetime, @Comercial integer, @Zona integer, @ClientsBaixa integer, @CapZona Integer
Set @Data = :Data
Set @Comercial = :Comercial
Set @Zona = :Zona
Set @ClientsBaixa = :ClientsBaixa
Set @CapZona = :CapComercial

Select pc.Descripcio NomComercial, c.Client, c.Nom NomClient, c.PreClient, c.TipusABCClient, z.Nom as NomZona,
(SELECT case when count(*) >=1then CAST(1 AS BIT) else CAST(0 AS BIT) end from ArticlePreparaClients
	where Client = c.client and ((DataInici<=@Data or DataInici is null) and (DataFi>=@Data or DataFi is null)) and isnull(EsFeinaNeteja,0) = 0) as FrescEx,
(SELECT case when count(*) >=1then CAST(1 AS BIT) else CAST(0 AS BIT) end from ArticleLlotjaExcepcions
	where Client = c.client and ((DataInici<=@Data or DataInici is null) and (DataFi>=@Data or DataFi is null))) as LlotjaEx,
(SELECT case when count(*) >=1then CAST(1 AS BIT) else CAST(0 AS BIT) end from ArticleCongelatClients
	where Client = c.client and ((DataInici<=@Data or DataInici is null) and (DataFi>=@Data or DataFi is null))) as CongelatEx,
(SELECT case when count(*) >=1then CAST(1 AS BIT) else CAST(0 AS BIT) end from ArticlePreparaClients
	where Client = c.client and ((DataInici<=@Data or DataInici is null) and (DataFi>=@Data or DataFi is null)) and isnull(EsFeinaNeteja,0) = 1) as FeinaEx,
(Select case when count(*) >=1then CAST(1 AS BIT) else CAST(0 AS BIT) end From ArticlePreparaPressupostos app
	where Client = c.Client and (DataInici<=@Data or DataInici is null) and (DataFi>=@Data or DataFi is null) and app.Tipus = 1) as FrescPers,
(Select case when count(*) >=1then CAST(1 AS BIT) else CAST(0 AS BIT) end From ArticlePreparaPressupostos app
	where Client = c.Client and (DataInici<=@Data or DataInici is null) and (DataFi>=@Data or DataFi is null) and app.Tipus = 2) as LlotjaPers,
(Select case when count(*) >=1then CAST(1 AS BIT) else CAST(0 AS BIT) end From ArticlePreparaPressupostos app
	where Client = c.Client and (DataInici<=@Data or DataInici is null) and (DataFi>=@Data or DataFi is null) and app.Tipus = 3) as CongelatPers,
(Select case when count(*) >=1then CAST(1 AS BIT) else CAST(0 AS BIT) end from ArticlePreparaClientsEmails apce
	where (c.Client = apce.Client) and apce.Actiu = 1) as FrescProg,
(Select case when count(*) >=1then CAST(1 AS BIT) else CAST(0 AS BIT) end from ArticleLlotjaClientsEmails apce
	where (c.Client = apce.Client) and apce.Actiu = 1) as LlotjaProg,
(Select case when count(*) >=1then CAST(1 AS BIT) else CAST(0 AS BIT) end from ArticleCongelatClientsEmails apce
	where (c.Client = apce.Client) and apce.Actiu = 1) as CongelatProg,
	c.Zona,
	zc.CapComercial,
  c.ObservacionsCRM
From Clients c
left join PersonalComercial pc on c.Comercial = pc.Comercial
left join Zones z on c.Zona = z.Zona
left join ZonesCapComercial zc on zc.Zona = c.Zona
where 
(
(c.Comercial = @Comercial or @Comercial = -1 
	or (c.PreClient = 1 and c.Zona = @Comercial))
and (c.Zona = @Zona or @Zona = -1) 
and (@CapZona = -1 or zc.CapComercial = @CapZona))
and (c.DataBaixa is null or @ClientsBaixa = -1)

order by PreClient, Zona, NomClient, Client
```

#### SQL\ quCopiarPressupost

Donada una **Tarifa Personalitzada** base, crea una copia amb les dates passades per parametre i el client establert.

```SQL
Declare @Client integer, @DataInici DateTime, @DataFi datetime, @TarifaBase integer, @Persona integer

Set @Client = :Client
Set @DataInici = :DataInici
Set @DataFi = :DataFi
Set @TarifaBase = :TarifaBase
Set @Persona = :Persona

insert into ArticlePreparaPressupostos(Client, DataInici, DataFi, Comentaris, Tipus, Persona)
Select @Client, @DataInici, @DataFi,
('Copia de TP' + cast(id as varchar(5)) + '; ' + isnull(Comentaris, '')) as Comentaris,
Tipus, @Persona from ArticlePreparaPressupostos
where id = @TarifaBase;

insert into ArticlePreparaPressupostosLinies(IdPressupost, Article)
Select SCOPE_IDENTITY() IdPressupost,Article from ArticlePreparaPressupostosLinies
where IdPressupost = @TarifaBase;
```

#### SQL\ quCrearClientPlantilles

En cas de no existir, crea un client per a el comercial, el qual esta codificat -10**CodiComercial** nom "PLANTILLES **NomComercial**".

```SQL
Declare @CComercial integer

Set @CComercial = :Comercial

if (select count(*) from Clients where client = cast(('-10' + cast(@CComercial as varchar)) as int)) = 0 
insert into Clients(Client, Nom,Zona,Comercial) 
(Select top 1
	cast(('-10' + cast(@CComercial as varchar)) as int) Client,
  ('PLANTILLES ' +  UPPER(Descripcio)) Nom,
	@CComercial Zona,
	@CComercial Comercial
from PersonalComercial
where Comercial = @CComercial
)
```

#### SQL\ quEmail

Nomes te un DataSource assignat i no s'utilitza per a res, segurament es podria eliminar.

```SQL
select * from ArticlePreparaClientsEmails
```

#### SQL\ quDetallPressupost

Llista una **Tarifa Personalitzada**.

- Actiu: Article actiu aquell dia
- Client:
  - 1 Article dins Tarifa
  - 0 Article fora Tarifa
- NomMarca: En el cas de Conge es el nom del Grup, ja que la jerarquia de articles a Conge esta ben estructurada.

S'utilitza el "Where @Tipus = 1/2/3" per indicar de quina query agafar.

```SQL
Declare @Pressupost int, @Data DateTime, @Tipus int

Set @Pressupost = :Pressupost
Set @Data = :Data
Set @Tipus = (Select Tipus from ArticlePreparaPressupostos where id = @Pressupost)

SELECT
    ap.Actiu Actiu,
    case when exists
    (select appl.Article
     from ArticlePreparaPressupostosLinies appl
     where appl.Article = cast(ap.Article as INT) and
           appl.IdPressupost = @Pressupost) then 1
     else 0
     end Client,
    ap.Article,
    a.NomArticle Nom,
	a.NomMarca,
  a.NomFamilia Familia
From ArticlePrepara ap
left join Articles a on ap.Article = a.Article
Where ap.[Data] = @Data and a.DataBaixa is null
and @Tipus = 1

union all

SELECT
    cast(1 as bit) Actiu,
    case when exists
    (select appl.Article
     from ArticlePreparaPressupostosLinies appl
     where appl.Article = cast(ap.Article as INT) and
           appl.IdPressupost = @Pressupost) then 1
     else 0
     end Client,
    ap.Article,
    a.NomArticle Nom,
	a.NomMarca,
  a.NomFamilia Familia
From ArticleLlotja ap
left join Articles a on ap.Article = a.Article
Where ap.[Data] = @Data and a.DataBaixa is null
and @Tipus = 2

union all

SELECT
    ap.Actiu Actiu,
    case when exists
    (select appl.Article
     from ArticlePreparaPressupostosLinies appl
     where appl.Article = cast(ap.Article as INT) and
           appl.IdPressupost = @Pressupost) then 1
     else 0
     end Client,
    ap.Article,
    a.NomArticle Nom,
	a.NomGrup as NomMarca,
  a.NomFamilia Familia
From ArticleCongelat ap
left join Articles a on ap.Article = a.Article
Where @Tipus = 3
order by NomMarca, NomArticle
```

#### SQL\ quDetallPressupostNOArticlePrepara

Ens retorna tots els articles que tenim seleccionats a la **Tarifa Personalitzada**, i que no es troben al llistat de Tarifa de venda.

```SQL
Select appl.Article, a.NomArticle, a.NomMarca from ArticlePreparaPressupostosLinies appl 
left join Articles a on a.Article = appl.Article
where 
	IdPressupost = :Pressupost
	and appl.Article not in (Select Article from ArticlePrepara where Data = :Data)
```

#### SQL\ quDetallPressupostInsert

Utilitzat per a fer els Appends de una **Tarifa Pers**

```SQL
Select 
    Id,
    IdPressupost,
    Article 
From 
    ArticlePreparaPressupostosLinies
Where 
    IdPressupost = :Pressupost
```

#### SQL\ quDetallPressupostDelete

Elimina les linies de la **Tarifa Pers** marcat.

```SQL
Delete
From ArticlePreparaPressupostosLinies
Where IdPressupost = :Pressupost
```

#### SQL\ quComercials

Obte un llistat de els comercials disponibles segons si ets un comercial, cap de zona o admin.

##### Exemple Comercials

Admin (@Comercial = -1, @CapComercial = -1):

|Comercial|Persona|Descripcio|Email|
|---|---|---|---|
|-1|0|Tots els Comercials||
|4|65|Joan Horta|<joan.horta@peixospuignau.net>|
|5|6|Dani Sucarrat|<dani.sucarrat@peixospuignau.net>|
|6|185|Jordi Pujol|<jordi.pujol@peixospuignau.net>|
|8|141|Fabienne Bonet|<fabianne.bonet@peixospuignau.net>|
|9|148|Sergi Oliveras|<sergi.oliveras@peixospuignau.net>|
|11|234|Toni Escobar|<toni.escobar@peixospuignau.net>|
|12|228|David Campasol|<david.campasol@peixospuignau.net>|
|17|286|Enric Auladell|<enric.auladell@peixospuignau.net>|
|19|1025|Jaume M|<jaume.m@peixospuignau.net>|
|20|290|Xavier Cornell&#224;|<xavier.cornella@peixospuignau.net>|
|37|1013|Laurent Guerin|<laurent.guerin@peixospuignau.net>|
|38|193|Jordi Pijoan|<jordi.pijoan@peixospuignau.net>|
|41|1064|Brian Mu&#241;oz|<brian.munoz@peixospuignau.net>|
|49|372|&#211;scar Plaza|<oscar.plaza@peixospuignau.net>|
|53|683|David Ventosa|<david.ventosa@peixospuignau.net>|
|54|483|David Tejero|<david.tejero@peixospuignau.net>|
|64|553|L&#237;dia Crous|<lidia.crous@peixospuignau.net>|

Cap de Zona (@Comercial = 9, @CapComercial = 9):

|Comercial|Persona|Descripcio|Email|
|---|---|---|---|
|-1|0|Tots els Comercials||
|5|6|Dani Sucarrat|<dani.sucarrat@peixospuignau.net>|
|9|148|Sergi Oliveras|<sergi.oliveras@peixospuignau.net>|
|11|234|Toni Escobar|<toni.escobar@peixospuignau.net>|
|17|286|Enric Auladell|<enric.auladell@peixospuignau.net>|
|20|290|Xavier Cornell&#224;|<xavier.cornella@peixospuignau.net>|
|41|1064|Brian Mu&#241;oz|<brian.munoz@peixospuignau.net>|

Comercial (@Comercial = 17, @CapComercial = -1):

|Comercial|Persona|Descripcio|Email|
|---|---|---|---|
|-1|0|Tots els Comercials||
|17|286|Enric Auladell|<enric.auladell@peixospuignau.net>|

```SQL
declare @Comercial INTEGER, @CapComercial integer

Set @Comercial = :Comercial
Set @CapComercial = IsNull(:CapComercial, -1)

Select Comercial, pc.Persona, Descripcio, Email
From PersonalComercial pc
left join Personal p on pc.Persona = p.Persona
Left Join ZonesCapComercial as ZC on ZC.Zona = pc.Comercial
where DataBaixa is null
and pc.Persona < 9000
and Departament = 4
and ((@Comercial = -1 and @CapComercial = -1) or @Comercial = pc.Comercial
	or ZC.CapComercial = @CapComercial)
union all
Select -1 Comercial, 0 Persona, 'Tots els Comercials' Descripcio, '' Email
order by Comercial
```

#### SQL\ quZones

Obte un llistat de les zones disponibles segons si ets un comercial, cap de zona o admin.

##### Exemple Zones

Admin (@CapComercial = -1, @Zona = -1):

|Zona|NomZona|
|---|---|
|-1|Totes les Zones|
|1|JOSEP PUIGNAU (5007)|
|2|TASTAMAR|
|4|JOAN HORTA (5008)|
|5|DANI S. (5005)|
|6|JORDI PUJ. (5004)|
|8|FABIENNE|
|9|SERGI O. (5039)|
|10|BOTIGUES (5010)|
|11|TONI ESCOBAR (5003)|
|12|DAVID C. (5049)|
|17|ENRIC AULADELL (5017)|
|18|TONI ESCA&#209;O (5016)|
|19|JAUME M.(5028)|
|20|X. CORNELL&#192; (5002)|
|26|JOAN MASDEVALL|
|30|PUIGNAU-2|
|37|LAURENT G. (5033)|
|38|PIJOAN, JORDI (5059)|
|39|DAVID BASAGA&#209;A (5016)|
|40|DAVID GARCIA|
|41|BRIAN MUNOZ|
|49|OSCAR PLAZA|
|53|DAVID VENTOSA (53)|
|54|DAVID TEJERO|
|55|SEANOW|
|56|DIST / MAJORIST / C.PRODUCCI&#211;|
|64|LIDIA CROUS|
|99|FRAN&#199;A|
|701|701 - NAUFRESC-BOTIGA PLA&#199;A|
|702|702 - NAUFRESC-BOTIGA PERICOT |
|703|703 - NAUFRESC-BOT. AMET GIR  |
|704|704 - NAUFRESC-BOTIGA BANYOLES|
|705|705 - NAUFRESC-BOT. PLJ D&#39;ARO|
|706|706 - NAUFRESC-BOTIGA STGREGOR|
|707|707 - NAUFRESC-BOTIGA CALONGE|
|708|708 - NAUFRESC-BOTIGA QUART|
|710|710 - NAUFRESC-BOTIGA PALAFRUG|
|999|PROVES IT|

Cap de Zona (@CapComercial = 9, @Zona = 9):

|Zona|NomZona|
|---|---|
|-1|Totes les Zones|
|5|DANI S. (5005)|
|9|SERGI O. (5039)|
|11|TONI ESCOBAR (5003)|
|17|ENRIC AULADELL (5017)|
|20|X. CORNELL&#192; (5002)|
|41|BRIAN MUNOZ|

Comercial (@CapComercial = -1, @Zona = 17):

|Zona|NomZona|
|---|---|
|-1|Totes les Zones|
|17|ENRIC AULADELL (5017)|

```SQL
Declare @CapComercial Integer, @Zona Integer

Set @CapComercial = :CapComercial
Set @Zona = :Zona

SELECT z.[Zona]
    ,z.Nom NomZona
  FROM 
  Zones z
  left Join  [Puignau].[dbo].[ZonesCapComercial] zc on z.Zona = zc.Zona
  where (@CapComercial = -1
  and (@Zona = -1 or @Zona = z.Zona))
  or (@CapComercial = zc.CapComercial)
union all
SELECT distinct -1 Zona
    ,'Totes les Zones' NomZona
  order by z.Zona
```

#### SQL\ quCapDeZones

Llegir tots els caps de Zona.

```SQL
Select -1 CapComercial, 'Tots' Descripcio
union all
Select CapComercial, MAX(PC.Descripcio) as Descripcio
FROM ZonesCapComercial zc
Join PersonalComercial as PC on PC.Comercial = ZC.CapComercial
Group by CapComercial
```

#### SQL\ quPlantilles

Mostra totes les plantilles que pot utilitzar un Comercial.

```SQL
Declare @ClientPlantillaComercial integer,@Comercial integer, @Tipus integer

Set @Comercial = :Comercial
Set @ClientPlantillaComercial = case @Comercial when -1 then -1 else cast('-10' + cast(@Comercial as varchar) as numeric) end
Set @Tipus = :Tipus
Select
case app.Client
	when -100 then 'General'
	when @ClientPlantillaComercial then 'Propia'
	else
		(case
			when app.Client < 0 then 'Plantilla Comercial'
			else 'Client '+ cast(app.Client as varchar) + ' - ' + c.Nom
		end)
end TipusPlant,
case app.Client
	when -100 then 'General'
	when @ClientPlantillaComercial then 'Propia'
	else
		(case
			when app.Client < 0 then 'Plantilla ' +
				(Select top 1 upper(pc.Descripcio)
				from PersonalComercial pc
				where
					pc.Comercial = c.Comercial)
			else 'Client '+ cast(app.Client as varchar)
		end)
end CTipusPlant,
case app.Client
	when -100 then 1
	when @ClientPlantillaComercial then 2
	else
		(case
			when app.Client < 0 then 2
			else 3
		end)
end OTipusPlant,
app.Id, app.Comentaris, Tipus,
case Tipus
	when 1 then 'Fresc'
	when 2 then 'Llotja'
	when 3 then 'Congelat'
end as CTipus, (Select top 1 upper(pc.Descripcio)
				from PersonalComercial pc
				where
					pc.Comercial = c.Comercial) as Comercial
from  ArticlePreparaPressupostos app
left join Puignau.dbo.Clients c on app.Client = c.Client
Where
  ((@Comercial = -1
	or C.Zona = @Comercial) or app.CLient = -100)
  and (@Tipus = -1 or @Tipus = Tipus)
Order by app.Tipus,3,c.Comercial,c.client,app.Id
```

#### SQL\ quDetallPlantilles

DataSource:= dsPlantilles;

dsPlantilles.DataSet:= quPlantilles

```SQL
Select appl.Article, case Tipus when 1 then a.NomMarca else NomGrup end as Grup,a.NomFamilia, a.NomGrup, a.NomSubgrup,a.NomArticle
from  ArticlePreparaPressupostosLinies appl
left join ArticlePreparaPressupostos app on appl.IdPressupost = app.Id
left join Articles a on appl.Article = a.Article
Where appl.IdPressupost = :Id
```

#### SQL\ quDetallPressupostConfig [DEPRECATED]

No s'utilitza

```SQL
SELECT 1 as Actiu, 1 as Client, AP.Article, A.NomArticle as Nom, A.NomMarca
From ArticlePreparaPressupostosLinies as AP
Left Join Articles as A on AP.Article = A.Article
Where AP.IdPressupost = :Pressupost
Order by AP.Article
```

[FMain]:./FMain.md
