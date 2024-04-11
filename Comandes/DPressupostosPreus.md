# [..](..)\Comandes\DPressupostosPreus

## Index

- [Index](#index)
- [Descripcio](#descripcio)
- [Interface](#interface)
  - [published](#published)
  - [private](#private)
  - [public](#public)
- [Events](#events)
  - [procedure taDetallPressupostCongelatNewRecord(DataSet: TDataSet)](#procedure-tadetallpressupostcongelatnewrecorddataset-tdataset)
  - [procedure taDetallPressupostCongelatTarifaChange(Sender: TField)](#procedure-tadetallpressupostcongelattarifachangesender-tfield)
  - [procedure taDetallPressupostCongelatCalcFields(DataSet: TDataSet)](#procedure-tadetallpressupostcongelatcalcfieldsdataset-tdataset)
  - [procedure quPressupostAutoritzarCalcFields(DataSet: TDataSet)](#procedure-qupressupostautoritzarcalcfieldsdataset-tdataset)
- [Metodes](#metodes)
  - [procedure ClearPressupost(IdPressupost: Integer)](#procedure-clearpressupostidpressupost-integer)
  - [procedure CrearBaseTree(ProgressBar: IProgressBar = nil)](#procedure-crearbasetreeprogressbar-iprogressbar--nil)
    - [procedure CopyBkp(Tipus, Id, pId: Integer; Codi: string)](#procedure-copybkptipus-id-pid-integer-codi-string)
    - [procedure AfegirDetall(taTaula: TdxMemData; Tipus, pId: Integer; Codi, Nom: string; TipusArticle: TTipusArticle = taFresc; T1: Double = 0; T2: Double = 0; T3: Double = 0; T4: Double = 0; T5: Double = 0; Marca: string = ''; PCost: Double = 0; MargeT0: Double = 0; MargeT1: Double = 0; MargeT2: Double = 0; MargeT3: Double = 0; MargeT4: Double = 0)](#procedure-afegirdetalltataula-tdxmemdata-tipus-pid-integer-codi-nom-string-tipusarticle-ttipusarticle--tafresc-t1-double--0-t2-double--0-t3-double--0-t4-double--0-t5-double--0-marca-string---pcost-double--0-marget0-double--0-marget1-double--0-marget2-double--0-marget3-double--0-marget4-double--0)
    - [procedure AfegirFamilies(taTaula: TdxMemData; TipusArticle: TTipusArticle = taFresc)](#procedure-afegirfamiliestataula-tdxmemdata-tipusarticle-ttipusarticle--tafresc)
    - [procedure AfegirMarques(taTaula: TdxMemData)](#procedure-afegirmarquestataula-tdxmemdata)
    - [procedure AfegirGrups(taTaula: TdxMemData; TipusArticle: TTipusArticle = taFresc)](#procedure-afegirgrupstataula-tdxmemdata-tipusarticle-ttipusarticle--tafresc)
    - [procedure AfegirSubGrups(taTaula: TdxMemData; TipusArticle: TTipusArticle = taFresc)](#procedure-afegirsubgrupstataula-tdxmemdata-tipusarticle-ttipusarticle--tafresc)
    - [procedure AfegirArticles(taTaula: TdxMemData; TipusArticle: TTipusArticle = taFresc)](#procedure-afegirarticlestataula-tdxmemdata-tipusarticle-ttipusarticle--tafresc)
    - [procedure InicialitzarTaula(taTaula: TdxMemData)](#procedure-inicialitzartaulatataula-tdxmemdata)
  - [procedure NetejarParesOrfes(taTaula: TdxMemData)](#procedure-netejarparesorfestataula-tdxmemdata)
  - [procedure LlegirArticles(TipusArticle: TTipusArticle = taFresc)](#procedure-llegirarticlestipusarticle-ttipusarticle--tafresc)
  - [procedure LlegirFamilies(TipusArticle: TTipusArticle = taFresc)](#procedure-llegirfamiliestipusarticle-ttipusarticle--tafresc)
  - [procedure LlegirGrups(TipusArticle: TTipusArticle = taFresc)](#procedure-llegirgrupstipusarticle-ttipusarticle--tafresc)
  - [procedure LlegirSubGrups(TipusArticle: TTipusArticle = taFresc)](#procedure-llegirsubgrupstipusarticle-ttipusarticle--tafresc)
  - [procedure LlegirPressupostAutoritzat(IdPressupost: Integer)](#procedure-llegirpressupostautoritzatidpressupost-integer)
  - [procedure LlegirDadesPressupost(IdPressupost: Integer)](#procedure-llegirdadespressupostidpressupost-integer)
  - [procedure LlegirLiniesPerAutoritzar](#procedure-llegirliniesperautoritzar)
  - [procedure GuardarPressupost](#procedure-guardarpressupost)
    - [procedure AvisarPerEmailSolicituts](#procedure-avisarperemailsolicituts)
      - [function EmailNotificacio: string](#function-emailnotificacio-string)
      - [procedure EnviarCorreuNotificacio(dades: TDadesEmail; emailNotificar: string)](#procedure-enviarcorreunotificaciodades-tdadesemail-emailnotificar-string)
    - [procedure GuardarTaulaPress(taTaula: TdxMemData)](#procedure-guardartaulapresstataula-tdxmemdata)
  - [procedure LlegirPressuposts(Comercial: Integer = -1; Client: Integer = -1; Pressupost: Integer = -1)](#procedure-llegirpressupostscomercial-integer---1-client-integer---1-pressupost-integer---1)
  - [procedure CarregarPressupost(IdPressupost: Integer; ProgressBar: IProgressBar = nil)](#procedure-carregarpressupostidpressupost-integer-progressbar-iprogressbar--nil)
    - [procedure CarregarPressupost](#procedure-carregarpressupost)
      - [function AssignarTarifaPress(taTaula: TdxMemData; Tipus, ETarifa, ETipusExcepcio, EAmbit: Integer; EPercentIncCost, EImportIncCost, EPercentIncVenda, EPreuFix: Double; Ambit: string; DataAuto, DataSoli: TDateTime; PersonaAuto: Integer): Boolean](#function-assignartarifapresstataula-tdxmemdata-tipus-etarifa-etipusexcepcio-eambit-integer-epercentinccost-eimportinccost-epercentincvenda-epreufix-double-ambit-string-dataauto-datasoli-tdatetime-personaauto-integer-boolean)
    - [procedure LlegirExcepcions](#procedure-llegirexcepcions)
    - [procedure CarregarExcepcions](#procedure-carregarexcepcions)
      - [procedure AssignarExcepcio(taTaula: TdxMemData)](#procedure-assignarexcepciotataula-tdxmemdata)
      - [function TeExcepcio(taTaula: TdxMemData): Boolean](#function-teexcepciotataula-tdxmemdata-boolean)
      - [procedure AplicarExcepcions(taTaula: TdxMemData)](#procedure-aplicarexcepcionstataula-tdxmemdata)
  - [function ObtenirPermisosExcepcions(TipusClient: string; Columna: TColumnesPermisos): Boolean](#function-obtenirpermisosexcepcionstipusclient-string-columna-tcolumnespermisos-boolean)
  - [function EsModificable(Ambit: TTipusNode; TipusExcepcio: Integer; Tarifa: Integer; TipusABC: string): Boolean](#function-esmodificableambit-ttipusnode-tipusexcepcio-integer-tarifa-integer-tipusabc-string-boolean)
    - [function AmbitModificable: Boolean](#function-ambitmodificable-boolean)
    - [function TipusExcepcioModificable: Boolean](#function-tipusexcepciomodificable-boolean)
    - [function TarifaModificable: Boolean](#function-tarifamodificable-boolean)
  - [procedure AplicarExcepcionsParesAFills](#procedure-aplicarexcepcionsparesafills)
    - [procedure AplicarExcepcions(taTaula: TdxMemData)](#procedure-aplicarexcepcionstataula-tdxmemdata-1)
    - [procedure AssignarExcepcio](#procedure-assignarexcepcio)
    - [procedure CarregarExcepcio](#procedure-carregarexcepcio)
    - [function TeExcepcio(CheckExcepcio: Boolean): Boolean](#function-teexcepciocheckexcepcio-boolean-boolean)
  - [procedure ProcessarPressupost(progressBar: IProgressBar)](#procedure-processarpressupostprogressbar-iprogressbar)
    - [procedure FerExcepcio](#procedure-ferexcepcio)
    - [function ObtenirDataSetExcepcions(TipusArticle: Integer): TDataSet](#function-obtenirdatasetexcepcionstipusarticle-integer-tdataset)
    - [function FieldNameAmbit(Ambit: TTipusNode): string](#function-fieldnameambitambit-ttipusnode-string)
    - [function EsPotFerExcepcio(Ambit: TTipusNode; Codi: string): Boolean](#function-espotferexcepcioambit-ttipusnode-codi-string-boolean)
  - [procedure EliminarPressupost(Pressupost: Integer)](#procedure-eliminarpressupostpressupost-integer)
- [SQL](#sql)
  - [quClearPressupost](#quclearpressupost)
  - [quArticles](#quarticles)
  - [quFamilies](#qufamilies)
  - [quGrups](#qugrups)
  - [quSubGrups](#qusubgrups)
  - [quMarques](#qumarques)
  - [quDadesPressupostAutoritzades](#qudadespressupostautoritzades)
  - [quDadesPressupost](#qudadespressupost)
  - [quPressupostAutoritzar](#qupressupostautoritzar)
  - [quInsertDetallPressupost](#quinsertdetallpressupost)
  - [quPressupostsCapcalera](#qupressupostscapcalera)
  - [quAvisosEmail](#quavisosemail)
  - [quExcepcions](#quexcepcions)
  - [quEliminarPressupost](#queliminarpressupost)
  - [quArticlePreparaClients](#quarticlepreparaclients)
  - [quArticleCongelatClients](#quarticlecongelatclients)
  - [quArticleLlotjaExcepcions](#quarticlellotjaexcepcions)

## Descripcio

DataModule encarregat de gestionar tots els pressupostos en el programa de Gestio Comercial.

## Interface

### published

```Delphi
procedure taDetallPressupostCongelatNewRecord(DataSet: TDataSet);
procedure taDetallPressupostCongelatTarifaChange(Sender: TField);
procedure taDetallPressupostCongelatCalcFields(DataSet: TDataSet);
procedure quPressupostAutoritzarCalcFields(DataSet: TDataSet);
```

### private

```Delphi
FtaTaulaETarifaChange: Boolean;
procedure ClearPressupost(IdPressupost: Integer);
procedure CrearBaseTree(ProgressBar: IProgressBar = nil);
procedure NetejarParesOrfes(taTaula: TdxMemData);
procedure LlegirArticles(TipusArticle: TTipusArticle = taFresc);
procedure LlegirFamilies(TipusArticle: TTipusArticle = taFresc);
procedure LlegirGrups(TipusArticle: TTipusArticle = taFresc);
procedure LlegirSubGrups(TipusArticle: TTipusArticle = taFresc);
```

### public

```Delphi
procedure LlegirPressupostAutoritzat(IdPressupost: Integer);
procedure LlegirDadesPressupost(IdPressupost: Integer);
procedure LlegirLiniesPerAutoritzar;
procedure GuardarPressupost;
procedure LlegirPressuposts(Comercial: Integer = -1; Client: Integer = -1; Pressupost: Integer = -1);
procedure CarregarPressupost(IdPressupost: Integer; ProgressBar: IProgressBar = nil);
function ObtenirPermisosExcepcions(TipusClient: string; Columna: TColumnesPermisos): Boolean;
function EsModificable(Ambit: TTipusNode; TipusExcepcio, Tarifa: Integer; TipusABC: string): Boolean;
procedure AplicarExcepcionsParesAFills;
property TaTaulaETarifaChange: Boolean read FtaTaulaETarifaChange write FtaTaulaETarifaChange;
procedure ProcessarPressupost(progressBar: IProgressBar = nil);
procedure EliminarPressupost(Pressupost: Integer);
```

## Events

### procedure taDetallPressupostCongelatNewRecord(DataSet: TDataSet)

Inicialitza una linia de la taula en memoria.

La propietat `taTaulaETarifaChange` s'utilitza per deshabilitar l'event [taDetallPressupostCongelatTarifaChange](#procedure-tadetallpressupostcongelattarifachangesender-tfield)

```Delphi
procedure TdmPressupostosPreus.taDetallPressupostCongelatNewRecord(DataSet: TDataSet);
begin
  taTaulaETarifaChange := False;

  with DataSet do
  begin
    FieldByName('Inclos').Value := False;
    FieldByName('EAmbit').Value := -1;

    FieldByName('ETarifa').Value := 1;
    FieldByName('ETipusExcepcio').Value := -1;
    FieldByName('Epressupost').AsBoolean := False;
  end;
  taTaulaETarifaChange := true;
end;
```

### procedure taDetallPressupostCongelatTarifaChange(Sender: TField)

Al canviar la propietat tarifa, s'aplica l'Ambit al tipus de la linia i tambe s'aplica el tipus a tipus 3.

```Delphi
procedure TdmPressupostosPreus.taDetallPressupostCongelatTarifaChange(Sender: TField);
begin
  if not taTaulaETarifaChange then
    Exit;

  if not Sender.IsNull then
  begin
    Sender.DataSet.FieldByName('Inclos').Value := true;
    Sender.DataSet.FieldByName('EAmbit').Value := Sender.DataSet.FieldByName('Tipus').Value;
    Sender.DataSet.FieldByName('ETipusExcepcio').Value := 3;
  end;
end;
```

### procedure taDetallPressupostCongelatCalcFields(DataSet: TDataSet)

Calcula:

- `PreuAct`: retorna el preu actual segons si aplica tarifa, preu fix, etc.
- `DescExcepcio`: retorna `PreuAct` explicant quin proces fa.
- `AmbitExcepcio`: ens demostra el perque s'aplica aquesta excepcio.
- `Marge`: En cas de poder dividir per `PCost` ens diu quin marge tindriem.

```Delphi
procedure TdmPressupostosPreus.taDetallPressupostCongelatCalcFields(DataSet: TDataSet);
begin
  with DataSet do
  begin
    // Preu Actual

    if not FieldByName('ETipusExcepcio').IsNull then
    begin
      case TTipusExcepcio(FieldByName('ETipusExcepcio').Value) of
        tePercentCost:
          begin
            FieldByName('PreuAct').Value := FieldByName('PCost').Value + (FieldByName
              ('EPercentIncCost').Value * FieldByName('PCost').Value / 100);

            FieldByName('DescExcepcio').Value := 'PCost';
            if FieldByName('EPercentIncCost').Value <> 0 then
              FieldByName('DescExcepcio').Value := FieldByName('DescExcepcio').Value
                + ' + ' + FieldByName('EPercentIncCost').AsString + '%';
          end;
        teImportCost:
          begin
            FieldByName('PreuAct').Value := FieldByName('PCost').Value +
              FieldByName('EImportIncCost').Value;

            FieldByName('DescExcepcio').Value := 'PCost';
            if FieldByName('EImportIncCost').Value <> 0 then
              FieldByName('DescExcepcio').Value := FieldByName('DescExcepcio').Value
                + ' + ' + FieldByName('EImportIncCost').AsString + '€';
          end;
        tePercentVenda:
          begin
            case FieldByName('ETarifa').Value of
              2:
                begin
                  FieldByName('PreuAct').Value := FieldByName('T2').Value + (FieldByName
                    ('T2').Value * FieldByName('EPercentIncVenda').asfloat / 100);
                  FieldByName('DescExcepcio').Value := 'T2';
                end;
              3:
                begin
                  FieldByName('PreuAct').Value := FieldByName('T3').Value + (FieldByName
                    ('T3').Value * FieldByName('EPercentIncVenda').asfloat / 100);
                  FieldByName('DescExcepcio').Value := 'T3';
                end;
              4:
                begin
                  FieldByName('PreuAct').Value := FieldByName('T4').Value + (FieldByName
                    ('T4').Value * FieldByName('EPercentIncVenda').asfloat / 100);
                  FieldByName('DescExcepcio').Value := 'TB';
                end;
              5:
                begin
                  FieldByName('PreuAct').Value := FieldByName('T0').Value + (FieldByName
                    ('T0').Value * FieldByName('EPercentIncVenda').asfloat / 100);
                  FieldByName('DescExcepcio').Value := 'T0';
                end;
            else
              begin
                FieldByName('PreuAct').Value := FieldByName('T1').Value + (FieldByName
                  ('T1').Value * FieldByName('EPercentIncVenda').asfloat / 100);
                FieldByName('DescExcepcio').Value := 'T1';
              end;
            end;
            if FieldByName('EPercentIncVenda').AsFloat <> 0 then
              FieldByName('DescExcepcio').Value := FieldByName('DescExcepcio').Value
                + ' + ' + FieldByName('EPercentIncVenda').AsString + '%';
          end;
        teImportFix:
          begin
            FieldByName('PreuAct').Value := FieldByName('EPreuFix').Value;
            FieldByName('DescExcepcio').Value := FieldByName('EPreuFix').AsString + ' €';
          end;
      else
        FieldByName('PreuAct').Value := FieldByName('T1').Value;
        if FieldByName('PreuAct').IsNull then
          FieldByName('PreuAct').Value := 0;

        FieldByName('DescExcepcio').Value := 'T1 Base';
      end;

      if (not FieldByName('EAmbit').IsNull) and (FieldByName('ETipusExcepcio').Value
        <> -1) then
      begin
        if (not varisnull(FieldByName('EPressupost').Value)) and FieldByName('EPressupost').Value
          then
          FieldByName('AmbitExcepcio').Value := 'Pressupost'
        else
          FieldByName('AmbitExcepcio').Value := 'Excepcio';

        case TTipusNode(FieldByName('EAmbit').Value) of
          TTipusNode.tnTipusFamilia:
            FieldByName('AmbitExcepcio').Value := FieldByName('AmbitExcepcio').Value
              + ' de Familia';
          TTipusNode.tnTipusGrup:
            FieldByName('AmbitExcepcio').Value := FieldByName('AmbitExcepcio').Value
              + ' de Grup';
          TTipusNode.tnTipusSubGrup:
            FieldByName('AmbitExcepcio').Value := FieldByName('AmbitExcepcio').Value
              + ' de SubGrup';
          TTipusNode.tnTipusArticle:
            FieldByName('AmbitExcepcio').Value := FieldByName('AmbitExcepcio').Value
              + ' de Article';
        else
          FieldByName('AmbitExcepcio').Value := '';
        end;
      end;
    end;

    if FieldByName('PCost').Value <> 0 then
      FieldByName('Marge').Value := ((FieldByName('PreuAct').Value - FieldByName
        ('PCost').Value) * 100 / FieldByName('PCost').Value);
  end;
end;
```

### procedure quPressupostAutoritzarCalcFields(DataSet: TDataSet)

Calcula dintre el DataSet:

- `DescExc` per donar-nos una descripcio "humanitzada".
- `AmbitExc` per saber si s'esta aplicant a una familia, grup, subgrup o article.
- `DescAmbit` per saber a qui s'esta aplicant.

```Delphi
procedure TdmPressupostosPreus.quPressupostAutoritzarCalcFields(DataSet: TDataSet);
begin
  with DataSet do
  begin
    if not FieldByName('TipusExcepcio').IsNull then
    begin

      case TTipusExcepcio(FieldByName('TipusExcepcio').Value) of
        tePercentCost:
          begin
            FieldByName('DescExc').Value := 'PCost';
            if FieldByName('PercentIncCost').Value <> 0 then
              FieldByName('DescExc').Value := FieldByName('DescExc').Value +
                ' + ' + FieldByName('PercentIncCost').AsString + '%';
          end;
        teImportCost:
          begin
            FieldByName('DescExc').Value := 'PCost';
            if FieldByName('ImportIncCost').Value <> 0 then
              FieldByName('DescExc').Value := FieldByName('DescExc').Value +
                ' + ' + FieldByName('ImportIncCost').AsString + '€';
          end;
        tePercentVenda:
          begin
            case FieldByName('Tarifa').Value of
              2:
                begin
                  FieldByName('DescExc').Value := 'T2';
                end;
              3:
                begin
                  FieldByName('DescExc').Value := 'T3';
                end;
              4:
                begin
                  FieldByName('DescExc').Value := 'TB';
                end;
              5:
                begin
                  FieldByName('DescExc').Value := 'T0';
                end;
            else
              begin
                FieldByName('DescExc').Value := 'T1';
              end;
            end;
            if FieldByName('PercentIncVenda').AsFloat <> 0 then
              FieldByName('DescExc').Value := FieldByName('DescExc').Value +
                ' + ' + FieldByName('PercentIncVenda').AsString + '%';
          end;
        teImportFix:
          begin
            FieldByName('DescExc').Value := FieldByName('ImportFix').AsString + ' €';
          end;
      else
        FieldByName('DescExc').Value := 'T1 Base';
      end;

      if (not FieldByName('Tipus').IsNull) and (FieldByName('TipusExcepcio').Value
        <> -1) then
      begin
        case TTipusNode(FieldByName('Tipus').Value) of
          TTipusNode.tnTipusFamilia:
            FieldByName('AmbitExc').Value := 'Pressupost de Familia';
          TTipusNode.tnTipusGrup:
            FieldByName('AmbitExc').Value := 'Pressupost de Grup';
          TTipusNode.tnTipusSubGrup:
            FieldByName('AmbitExc').Value := 'Pressupost de SubGrup';
          TTipusNode.tnTipusArticle:
            FieldByName('AmbitExc').Value := 'Pressupost de Article';
        else
          FieldByName('AmbitExc').Value := '';
        end;
      end;

      FieldByName('DescAmbit').AsString := Trim(FieldByName('NomFamilia').AsString
        + ' ' + FieldByName('NomGrup').AsString + ' ' + FieldByName('NomSubGrup').AsString
        + ' ' + FieldByName('Marca').AsString + ' ' + FieldByName('NomArticle').AsString);
    end;
  end;
end;
```

## Metodes

### procedure ClearPressupost(IdPressupost: Integer)

[quClearPressupost](#quclearpressupost)

[LlegirDadesPressupost](#procedure-llegirdadespressupostidpressupost-integer)

```Delphi
procedure TdmPressupostosPreus.ClearPressupost(IdPressupost: Integer);
begin
  with quClearPressupost, Parameters do
  begin
    ParamByName('Pressupost').Value := IdPressupost;

    ExecSQL;
  end;
  LlegirDadesPressupost(IdPressupost);
end;
```

### procedure CrearBaseTree(ProgressBar: IProgressBar = nil)

[TTipusArticle]

[InicialitzarTaula](#procedure-inicialitzartaulatataula-tdxmemdata)\
[AfegirFamilies](#procedure-afegirfamiliestataula-tdxmemdata-tipusarticle-ttipusarticle--tafresc)\
[AfegirGrups](#procedure-afegirgrupstataula-tdxmemdata-tipusarticle-ttipusarticle--tafresc)\
[AfegirSubGrups](#procedure-afegirsubgrupstataula-tdxmemdata-tipusarticle-ttipusarticle--tafresc)\
[AfegirArticles](#procedure-afegirarticlestataula-tdxmemdata-tipusarticle-ttipusarticle--tafresc)\
[NetejarParesOrfes](#procedure-netejarparesorfestataula-tdxmemdata)\

```Delphi
procedure TdmPressupostosPreus.CrearBaseTree(ProgressBar: IProgressBar = nil);
var
  useProgressBar: Boolean;
begin
  useProgressBar := Assigned(ProgressBar);
  if useProgressBar then
    ProgressBar.SetUpProgressBar(9);

  InicialitzarTaula(taDetallPressupostCongelat); // Taula Congelat
  InicialitzarTaula(dxMemData1); // Taula Suport

  if useProgressBar then
    ProgressBar.Step('Afegint Families de Congelat');

  AfegirFamilies(taDetallPressupostCongelat, taCongelat);

  if useProgressBar then
    ProgressBar.Step('Afegint Families de Fresc');

  AfegirFamilies(taDetallPressupostCongelat, taFresc);

  if useProgressBar then
    ProgressBar.Step('Afegint Grups de Congelat');

  AfegirGrups(taDetallPressupostCongelat, taCongelat);

  if useProgressBar then
    ProgressBar.Step('Afegint Grups de Fresc');

  AfegirGrups(taDetallPressupostCongelat, taFresc);

  if useProgressBar then
    ProgressBar.Step('Afegint SubGrups de Congelat');

  AfegirSubGrups(taDetallPressupostCongelat, taCongelat);

  if useProgressBar then
    ProgressBar.Step('Afegint SubGrups de Fresc');

  AfegirSubGrups(taDetallPressupostCongelat, taFresc);

  if useProgressBar then
    ProgressBar.Step('Afegint Articles de Congelat');

  AfegirArticles(taDetallPressupostCongelat, taCongelat);

  if useProgressBar then
    ProgressBar.Step('Afegint Articles de Fresc');

  AfegirArticles(taDetallPressupostCongelat, taFresc);

  if useProgressBar then
    ProgressBar.Step('Netejar Arbre');
  NetejarParesOrfes(taDetallPressupostCongelat);

  if useProgressBar then
    ProgressBar.StopProgressBar;

  taDetallPressupostCongelat.EnableControls;
  dxMemData1.Close;
  dxMemData1.EnableControls;
end;
```

#### procedure CopyBkp(Tipus, Id, pId: Integer; Codi: string)

```Delphi
procedure CopyBkp(Tipus, Id, pId: Integer; Codi: string);
begin
  dxMemData1.Append;
  dxMemData1Tipus.Value := Tipus;
  dxMemData1Id.Value := Id;
  dxMemData1Codi.Value := Codi;
  dxMemData1.Post;
end;
```

#### procedure AfegirDetall(taTaula: TdxMemData; Tipus, pId: Integer; Codi, Nom: string; TipusArticle: TTipusArticle = taFresc; T1: Double = 0; T2: Double = 0; T3: Double = 0; T4: Double = 0; T5: Double = 0; Marca: string = ''; PCost: Double = 0; MargeT0: Double = 0; MargeT1: Double = 0; MargeT2: Double = 0; MargeT3: Double = 0; MargeT4: Double = 0)

[CopyBkp](#procedure-copybkptipus-id-pid-integer-codi-string)\
[TTipusArticle]

```Delphi
procedure AfegirDetall(taTaula: TdxMemData; Tipus, pId: Integer; Codi, Nom:
  string; TipusArticle: TTipusArticle = taFresc; T1: Double = 0; T2: Double =
  0; T3: Double = 0; T4: Double = 0; T5: Double = 0; Marca: string = ''; PCost:
  Double = 0; MargeT0: Double = 0; MargeT1: Double = 0; MargeT2: Double = 0;
  MargeT3: Double = 0; MargeT4: Double = 0);
begin
  if pId <> -1 then
  begin
    taTaula.Append;
    taTaula.FieldByName('Tipus').Value := Tipus;
    taTaula.FieldByName('ParentId').Value := pId;
    taTaula.FieldByName('Codi').Value := Codi;
    taTaula.FieldByName('Nom').Value := Nom;
    taTaula.FieldByName('PCost').Value := PCost;
    taTaula.FieldByName('T1').Value := T1;
    taTaula.FieldByName('T2').Value := T2;
    taTaula.FieldByName('T3').Value := T3;
    taTaula.FieldByName('T4').Value := T4;
    taTaula.FieldByName('T0').Value := T5;
    taTaula.FieldByName('Marca').Value := Marca;
    taTaula.FieldByName('TipusArticle').Value := Ord(TipusArticle);
    taTaula.FieldByName('MargeT0').Value := MargeT0;
    taTaula.FieldByName('MargeT1').Value := MargeT1;
    taTaula.FieldByName('MargeT2').Value := MargeT2;
    taTaula.FieldByName('MargeT3').Value := MargeT3;
    taTaula.FieldByName('MargeT4').Value := MargeT4;
    taTaula.Post;
    CopyBkp(Tipus, taTaula.FieldByName('Id').AsInteger, pId, Codi);
  end;
end;
```

#### procedure AfegirFamilies(taTaula: TdxMemData; TipusArticle: TTipusArticle = taFresc)

[TTipusArticle]

```Delphi
procedure AfegirFamilies(taTaula: TdxMemData; TipusArticle: TTipusArticle = taFresc);
begin
  LLegirFamilies(TipusArticle);
  while not quFamilies.eof do
  begin
    AfegirDetall(taTaula, Ord(tnTipusFamilia), PARENT_ID_BASE,
      quFamiliesFamilia.AsString, quFamiliesNom.AsString, TipusArticle);
    quFamilies.Next;
  end;
  quFamilies.Close;
end;
```

#### procedure AfegirMarques(taTaula: TdxMemData)

```Delphi
procedure AfegirMarques(taTaula: TdxMemData);
begin
  with quMarques, Parameters do
  begin
    Close;
    Open;
    First;
  end;
  while not quMarques.eof do
  begin
    AfegirDetall(taTaula, Ord(tnTipusMarca), PARENT_ID_BASE, quMarquesMarca.AsString,
      quMarquesNomMarca.AsString, taNull);
    quMarques.Next;
  end;
  quMarques.Close;
end;
```

#### procedure AfegirGrups(taTaula: TdxMemData; TipusArticle: TTipusArticle = taFresc)

[TTipusArticle]

```Delphi
procedure AfegirGrups(taTaula: TdxMemData; TipusArticle: TTipusArticle = taFresc);
var
  parentId: Integer;
begin
  LlegirGrups(TipusArticle);
  while not quGrups.eof do
  begin
    if dxMemData1.Locate('Tipus;Codi', vararrayof([Ord(tnTipusFamilia),
      quGrupsFamilia.Value]), []) then
      parentId := dxMemData1Id.Value
    else
      parentId := -1;
    AfegirDetall(taTaula, Ord(tnTipusGrup), parentId, quGrupsGrup.AsString,
      quGrupsNom.AsString, TipusArticle);
    quGrups.Next;
  end;
  quGrups.Close;
end;
```

#### procedure AfegirSubGrups(taTaula: TdxMemData; TipusArticle: TTipusArticle = taFresc)

[TTipusArticle]

```Delphi
procedure AfegirSubGrups(taTaula: TdxMemData; TipusArticle: TTipusArticle = taFresc);
var
  parentId: Integer;
begin
  LlegirSubGrups(TipusArticle);
  while not quSubGrups.eof do
  begin
    if dxMemData1.Locate('Tipus;Codi', vararrayof([Ord(tnTipusGrup),
      quSubGrupsGrup.Value]), []) then
      parentId := dxMemData1Id.Value
    else
      parentId := -1;
    AfegirDetall(taTaula, Ord(tnTipusSubGrup), parentId, quSubGrupsSubGrup.AsString,
      quSubGrupsNom.AsString, TipusArticle);
    quSubGrups.Next;
  end;
  quSubGrups.Close;
end;
```

#### procedure AfegirArticles(taTaula: TdxMemData; TipusArticle: TTipusArticle = taFresc)

[TTipusArticle]

```Delphi
procedure AfegirArticles(taTaula: TdxMemData; TipusArticle: TTipusArticle = taFresc);
var
  parentId: Integer;
begin
  LlegirArticles(TipusArticle);
  while not quArticles.eof do
  begin
    if dxMemData1.Locate('Tipus;Codi', vararrayof([Ord(tnTipusSubGrup),
      quArticlesSubgrup.Value]), []) then
      parentId := dxMemData1Id.Value
    else if dxMemData1.Locate('Tipus;Codi', vararrayof([Ord(tnTipusGrup),
      quArticlesGrup.Value]), []) then
      parentId := dxMemData1Id.Value
    else if dxMemData1.Locate('Tipus;Codi', vararrayof([Ord(tnTipusFamilia),
      quArticlesFamilia.Value]), []) then
      parentId := dxMemData1Id.Value
    else if dxMemData1.Locate('Tipus;Codi', vararrayof([Ord(tnTipusMarca),
      quArticlesMarca.Value]), []) then
      parentId := dxMemData1Id.Value
    else
      parentId := -1;
    AfegirDetall(taTaula, Ord(TTipusNode.tnTipusArticle), parentId,
      quArticlesArticle.AsString, quArticlesNomArticle.AsString, TipusArticle,
      quArticlesT1.Value, quArticlesT2.Value, quArticlesT3.Value, {quArticlesTB.Value}0,
      quArticlesT0.Value, quArticlesNomMarca.Value, quArticlesPreuCost.Value,
      quArticlesMargeT0.asfloat, quArticlesMargeT1.asfloat, quArticlesMargeT2.asfloat,
      quArticlesMargeT3.asfloat, {quArticlesMargeT4.asfloat}0);
    quArticles.Next;
  end;
  quArticles.Close;
end;
```

#### procedure InicialitzarTaula(taTaula: TdxMemData)

```Delphi
procedure InicialitzarTaula(taTaula: TdxMemData);
begin
  with taTaula do
  begin
    Close;
    Open;
    DisableControls;
  end;
end;
```

### procedure NetejarParesOrfes(taTaula: TdxMemData)

Recorre la taula desde el final fins al principi i en cas de que ningu tingui l'ID de la linia actual assignada com a ParentID, s'elimina de la taula.

```Delphi
procedure TdmPressupostosPreus.NetejarParesOrfes(taTaula: TdxMemData);
var
  x: Variant;
begin
  taTaula.Last;

  while not taTaula.Bof do
  begin
    if TTipusNode(taTaula.FieldByName('Tipus').Value) <> TTipusNode.tnTipusArticle then
    begin
      x := taTaula.Lookup('ParentId', taTaula.FieldByName('Id').Value, 'Id');
      if varisnull(x) then
        taTaula.Delete
      else
        taTaula.Prior;
    end
    else
      taTaula.Prior;
  end;
end;
```

### procedure LlegirArticles(TipusArticle: TTipusArticle = taFresc)

[quArticles](#quarticles)\
[TTipusArticle]

```Delphi
procedure TdmPressupostosPreus.LlegirArticles(TipusArticle: TTipusArticle = taFresc);
begin
  with quArticles, Parameters do
  begin
    Close;
    ParamByName('TipusArticles').Value := TipusArticle;
    Open;
    First;
  end;
end;
```

### procedure LlegirFamilies(TipusArticle: TTipusArticle = taFresc)

[quFamilies](#qufamilies)\
[TTipusArticle]

```Delphi
procedure TdmPressupostosPreus.LlegirFamilies(TipusArticle: TTipusArticle = taFresc);
begin
  with quFamilies, Parameters do
  begin
    Close;
    ParamByName('TipusArticles').Value := Ord(TipusArticle);
    Open;
    First;
  end;
end;
```

### procedure LlegirGrups(TipusArticle: TTipusArticle = taFresc)

[quGrups](#qugrups)\
[TTipusArticle]

```Delphi
procedure TdmPressupostosPreus.LlegirGrups(TipusArticle: TTipusArticle = taFresc);
begin
  with quGrups, Parameters do
  begin
    Close;
    ParamByName('TipusArticles').Value := Ord(TipusArticle);
    Open;
    First;
  end;
end;
```

### procedure LlegirSubGrups(TipusArticle: TTipusArticle = taFresc)

[quSubGrups](#qusubgrups)\
[TTipusArticle]

```Delphi
procedure TdmPressupostosPreus.LlegirSubGrups(TipusArticle: TTipusArticle = taFresc);
begin
  with quSubGrups, Parameters do
  begin
    Close;
    ParamByName('TipusArticles').Value := TipusArticle;
    Open;
    First;
  end;
end;
```

### procedure LlegirPressupostAutoritzat(IdPressupost: Integer)

```Delphi
procedure TdmPressupostosPreus.LlegirPressupostAutoritzat(IdPressupost: Integer);
begin
  with quDadesPressupostAutoritzades, Parameters do
  begin
    Close;

    ParamByName('Pressupost').Value := IdPressupost;

    Open;
  end;
end;
```

### procedure LlegirDadesPressupost(IdPressupost: Integer)

[quDadesPressupost](#qudadespressupost)

```Delphi
procedure TdmPressupostosPreus.LlegirDadesPressupost(IdPressupost: Integer);
begin
  with quDadesPressupost, Parameters do
  begin
    Close;

    ParamByName('Pressupost').Value := IdPressupost;

    Open;
  end;
end;
```

### procedure LlegirLiniesPerAutoritzar

[quPressupostAutoritzar](#qupressupostautoritzar)

```Delphi
procedure TdmPressupostosPreus.LlegirLiniesPerAutoritzar;
begin
  with quPressupostAutoritzar do
  begin
    Close;
    Open;
  end;
end;
```

### procedure GuardarPressupost

```Delphi
procedure TdmPressupostosPreus.GuardarPressupost;
var
  Pressupost: Integer;
begin
  Pressupost := quPressupostsCapcaleraId.Value;

  ClearPressupost(Pressupost);
  GuardarTaulaPress(taDetallPressupostCongelat);
  AvisarPerEmailSolicituts;
  if quPressupostsCapcalera.State in dsEditModes then
    quPressupostsCapcalera.Post;
end;
```

#### procedure AvisarPerEmailSolicituts

[EmailNotificacio](#function-emailnotificacio-string)\
[EnviarCorreuNotificacio](#procedure-enviarcorreunotificaciodades-tdadesemail-emailnotificar-string)

```Delphi
procedure AvisarPerEmailSolicituts;
var
  emails: string;
  dades: TDadesEmail;
begin
  emails := EmailNotificacio;
  if emails <> '' then
  begin
    dades := TDadesEmail.Create;
    dades.Subject := '[PRESSUPOSTS][SOLICITUTS] Client: ' +
      quPressupostsCapcaleraClient.AsString + ' - ' +
      quPressupostsCapcaleraNom.AsString;
    dades.Text.Text := 'En el pressupost num: ' + inttostr(Pressupost) +
      ' hi ha solicituts pendents de autoritzar';
    EnviarCorreuNotificacio(dades, emails);
  end;
end;
```

##### function EmailNotificacio: string

[quAvisosEmail](#quavisosemail)

```Delphi
function EmailNotificacio: string;
begin
  with quAvisosEmail, Parameters do
  begin
    Close;
    ParamByName('Pressupost').Value := Pressupost;
    Open;
    First;
    Result := '';
    while not Eof do
    begin
      if Result <> '' then
        Result := Result + ';';
      Result := Result + quAvisosEmailEmail.AsString;
      Next;
    end;
    Close;
  end;
end;
```

##### procedure EnviarCorreuNotificacio(dades: TDadesEmail; emailNotificar: string)

```Delphi
procedure EnviarCorreuNotificacio(dades: TDadesEmail; emailNotificar: string);
var
  f: TfrmMessageEditor;
  EmailAdmin, MailEmail, MailServer, MailUser, MailPasswd: string;
  TipusEmail, MailAuthentication, MailPort: integer;
  MailSSL: Boolean;
begin
// Processament i enviament del correu.
  Application.CreateForm(TfrmMessageEditor, f);
  try
    TipusEmail := 0;
    dmComandes.LlegirParamsEnviarEmail(TipusEmail, EmailAdmin, MailEmail,
      MailServer, MailUser, MailPasswd, MailAuthentication, MailPort, MailSSL);
    f.UserEmail := MailEmail;
    f.SmtpAuthType := MailAuthentication;
    f.SmtpServerUser := MailUser;
    f.SmtpServerPassword := MailPasswd;
    f.SmtpServerName := MailServer;
    f.SmtpServerPort := MailPort;
    f.SmtpAmbSSL := MailSSL;
    f.ConfirmarEnviament := False;
    f.CarregarParametres;
    f.edtSubject.Text := dades.Subject;
    f.edtBCC.Text := emailNotificar;
    f.grpAttachment.Visible := False;
    f.Memo1.Lines.AddStrings(dades.Text);
    f.bbtnOk.Click;
  finally
    f.Free;
  end;
end;
```

#### procedure GuardarTaulaPress(taTaula: TdxMemData)

[quInsertDetallPressupost](#quinsertdetallpressupost)\
[TTipusNode]

```Delphi
procedure GuardarTaulaPress(taTaula: TdxMemData);
var
  mId: Integer;
begin
  with quInsertDetallPressupost do
  begin
    Close;
    Open;
  end;
  taTaula.DisableControls;
  taTaula.Filtered := False;
  mId := taTaula.FieldByName('Id').Value;
  taTaula.First;
  while not taTaula.eof do
  begin
    if taTaula.FieldByName('Inclos').Value then
    begin
      quInsertDetallPressupost.Append;
      case TTipusNode(taTaula.FieldByName('Tipus').Value) of
        TTipusNode.tnTipusFamilia:
          quInsertDetallPressupostFamilia.Value := taTaula.FieldByName('Codi').Value;
        TTipusNode.tnTipusGrup:
          quInsertDetallPressupostGrup.Value := taTaula.FieldByName('Codi').Value;
        TTipusNode.tnTipusSubGrup:
          quInsertDetallPressupostSubGrup.Value := taTaula.FieldByName('Codi').Value;
        TTipusNode.tnTipusArticle:
          quInsertDetallPressupostArticle.Value := taTaula.FieldByName('Codi').Value;
        TTipusNode.tnTipusMarca:
          quInsertDetallPressupostMarca.Value := taTaula.FieldByName('Codi').Value;
      end;
      if (taTaula.FieldByName('Tipus').Value = taTaula.FieldByName('EAmbit').Value)
        and taTaula.FieldByName('EPressupost').Value then
      begin
        if not taTaula.FieldByName('ETipusExcepcio').IsNull then
          quInsertDetallPressupostTipusExcepcio.Value := taTaula.FieldByName('ETipusExcepcio').Value;
        if not taTaula.FieldByName('EPercentIncCost').IsNull then
          quInsertDetallPressupostPercentIncCost.Value := taTaula.FieldByName('EPercentIncCost').Value;
        if not taTaula.FieldByName('EImportIncCost').IsNull then
          quInsertDetallPressupostImportIncCost.Value := taTaula.FieldByName('EImportIncCost').Value;
        if not taTaula.FieldByName('EPercentIncVenda').IsNull then
          quInsertDetallPressupostPercentIncVenda.Value := taTaula.FieldByName
            ('EPercentIncVenda').Value;
        if not taTaula.FieldByName('EPreuFix').IsNull then
          quInsertDetallPressupostImportFix.Value := taTaula.FieldByName('EPreuFix').Value;
        if not taTaula.FieldByName('ETarifa').IsNull then
          quInsertDetallPressupostTarifa.Value := taTaula.FieldByName('ETarifa').Value;
        if taTaula.FieldByName('DataSolicitat').Value > 0 then
          quInsertDetallPressupostDataSolicitat.Value := taTaula.FieldByName('DataSolicitat').Value
        else
          quInsertDetallPressupostDataSolicitat.clear;
        if taTaula.FieldByName('DataAutoritzat').Value > 0 then
          quInsertDetallPressupostDataAutoritzat.Value := taTaula.FieldByName('DataAutoritzat').Value
        else
          quInsertDetallPressupostDataAutoritzat.clear;
        if not taTaula.FieldByName('PersonaAutoritza').IsNull then
          quInsertDetallPressupostPersonaAutoritza.Value := taTaula.FieldByName
            ('PersonaAutoritza').Value
        else
          quInsertDetallPressupostPersonaAutoritza.clear;
      end;
      quInsertDetallPressupostPressupost.Value := Pressupost;
      quInsertDetallPressupost.Post;
    end;
    taTaula.Next;
  end;
  taTaula.Locate('Id', mId, []);
  taTaula.Filtered := true;
  taTaula.EnableControls;
  quInsertDetallPressupost.Close;
end;
```

### procedure LlegirPressuposts(Comercial: Integer = -1; Client: Integer = -1; Pressupost: Integer = -1)

[quPressupostsCapcalera](#qupressupostscapcalera)

```Delphi
procedure TdmPressupostosPreus.LlegirPressuposts(Comercial: Integer = -1; Client:
  Integer = -1; Pressupost: Integer = -1);
begin
  with quPressupostsCapcalera, Parameters do
  begin
    Close;

    ParamByName('Client').Value := Client;
    ParamByName('Comercial').Value := Comercial;
    ParamByName('Pressupost').Value := Pressupost;

    Open;
  end;
end;
```

### procedure CarregarPressupost(IdPressupost: Integer; ProgressBar: IProgressBar = nil)

[quPressupostsCapcalera](#qupressupostscapcalera)\
[CrearBaseTree](#procedure-crearbasetreeprogressbar-iprogressbar--nil)\
[LlegirDadesPressupost](#procedure-llegirdadespressupostidpressupost-integer)\
[LlegirExcepcions](#procedure-llegirexcepcions)\
[CarregarExcepcions](#procedure-carregarexcepcions)\
[CarregarPressupost](#procedure-carregarpressupost)\
[AplicarExcepcionsParesAFills](#procedure-aplicarexcepcionsparesafills)

```Delphi
procedure TdmPressupostosPreus.CarregarPressupost(IdPressupost: Integer;
  ProgressBar: IProgressBar = nil);
var
  useProgressBar: Boolean;
begin
  useProgressBar := Assigned(ProgressBar);
  quPressupostsCapcalera.Locate('Id', IdPressupost, []);
  quPressupostsCapcalera.Edit;
  CrearBaseTree(ProgressBar);

  if useProgressBar then
  begin
    ProgressBar.SetUpProgressBar(3);
    ProgressBar.step('Llegint dades');
  end;
  LlegirDadesPressupost(IdPressupost);
  LlegirExcepcions;

  if useProgressBar then
    ProgressBar.step('Carregant dades');
  CarregarExcepcions;
  CarregarPressupost;

  if useProgressBar then
    ProgressBar.step('Replicant');
  AplicarExcepcionsParesAFills;

  if useProgressBar then
    ProgressBar.StopProgressBar;
end;
```

#### procedure CarregarPressupost

[AssignarTarifaPress](#function-assignartarifapresstataula-tdxmemdata-tipus-etarifa-etipusexcepcio-eambit-integer-epercentinccost-eimportinccost-epercentincvenda-epreufix-double-ambit-string-dataauto-datasoli-tdatetime-personaauto-integer-boolean)

```Delphi
procedure CarregarPressupost;
begin
  taDetallPressupostCongelat.DisableControls;
  while not quDadesPressupost.eof do
  begin
    AssignarTarifaPress(taDetallPressupostCongelat, quDadesPressupostTipus.Value,
      quDadesPressupostTarifa.Value, quDadesPressupostTipusExcepcio.Value,
      quDadesPressupostTipus.Value, quDadesPressupostPercentIncCost.Value,
      quDadesPressupostImportIncCost.Value, quDadesPressupostPercentIncVenda.Value,
      quDadesPressupostImportFix.Value, quDadesPressupostAmbit.Value,
      quDadesPressupostDataAutoritzat.Value, quDadesPressupostDataSolicitat.Value,
      quDadesPressupostPersonaAutoritza.Value);
    quDadesPressupost.Next;
  end;
  taDetallPressupostCongelat.EnableControls;
end;
```

##### function AssignarTarifaPress(taTaula: TdxMemData; Tipus, ETarifa, ETipusExcepcio, EAmbit: Integer; EPercentIncCost, EImportIncCost, EPercentIncVenda, EPreuFix: Double; Ambit: string; DataAuto, DataSoli: TDateTime; PersonaAuto: Integer): Boolean

```Delphi
function AssignarTarifaPress(taTaula: TdxMemData; Tipus, ETarifa,
  ETipusExcepcio, EAmbit: Integer; EPercentIncCost, EImportIncCost,
  EPercentIncVenda, EPreuFix: Double; Ambit: string; DataAuto, DataSoli:
  TDateTime; PersonaAuto: Integer): Boolean;
begin
  Result := False;
  if taTaula.Locate('Tipus; Codi', vararrayof([Tipus, Ambit]), []) then
  begin
    taTaulaETarifaChange := False;
    taTaula.Edit;
    taTaula.FieldByName('Inclos').Value := true;
    if ETipusExcepcio <> 0 then
    begin
      taTaula.FieldByName('ETarifa').Value := ETarifa;
      taTaula.FieldByName('ETipusExcepcio').Value := ETipusExcepcio;
      taTaula.FieldByName('EPercentIncCost').Value := EPercentIncCost;
      taTaula.FieldByName('EImportIncCost').Value := EImportIncCost;
      taTaula.FieldByName('EPercentIncVenda').Value := EPercentIncVenda;
      taTaula.FieldByName('EPreuFix').Value := EPreuFix;
      taTaula.FieldByName('EAmbit').Value := EAmbit;
      if DataAuto <> 0 then
        taTaula.FieldByName('DataAutoritzat').Value := DataAuto;
      if DataSoli <> 0 then
        taTaula.FieldByName('DataSolicitat').Value := DataSoli;
      taTaula.FieldByName('PersonaAutoritza').Value := PersonaAuto;
      taTaula.FieldByName('EPressupost').Value := true;
    end;
    taTaula.Post;
    taTaulaETarifaChange := true;
    Result := true;
  end;
end;
```

#### procedure LlegirExcepcions

[quExcepcions](#quexcepcions)
[quPressupostsCapcalera](#qupressupostscapcalera)

```Delphi
procedure LlegirExcepcions;
begin
  with quExcepcions, Parameters do
  begin
    Close;
    ParamByName('Client').Value := quPressupostsCapcaleraClient.AsInteger;
    Open;
  end;
end;
```

#### procedure CarregarExcepcions

[AplicarExcepcions](#procedure-aplicarexcepcionstataula-tdxmemdata)

```Delphi
procedure CarregarExcepcions;
begin
  AplicarExcepcions(taDetallPressupostCongelat);
end;
```

##### procedure AssignarExcepcio(taTaula: TdxMemData)

[quExcepcions](#quexcepcions)

```Delphi
procedure AssignarExcepcio(taTaula: TdxMemData);
begin
  taTaula.Edit;
  taTaula.FieldByName('ETipusExcepcio').Value := quExcepcionsETipusExcepcio.Value;
  taTaula.FieldByName('ETarifa').Value := quExcepcionsETarifa.Value;
  taTaula.FieldByName('EPercentIncCost').Value := quExcepcionsEPercentInc.Value;
  taTaula.FieldByName('EImportIncCost').Value := quExcepcionsEImportInc.Value;
  taTaula.FieldByName('EPercentIncVenda').Value := quExcepcionsEPercentIncVenda.Value;
  taTaula.FieldByName('EPreuFix').Value := quExcepcionsEImportFix.Value;
  taTaula.FieldByName('EAmbit').Value := quExcepcionsAmbit.Value;
  taTaula.Post;
end;
```

##### function TeExcepcio(taTaula: TdxMemData): Boolean

[TTipusNode]\
[TeExcepcio](#function-teexcepciotataula-tdxmemdata-boolean)

```Delphi
function TeExcepcio(taTaula: TdxMemData): Boolean;
var
  Id, pId: Integer;
begin
  case TTipusNode(taTaula.FieldByName('Tipus').Value) of
    TTipusNode.tnTipusFamilia:
      Result := quExcepcions.Locate('EFamilia', taTaula.FieldByName('Codi').Value,
        []);
    TTipusNode.tnTipusGrup:
      Result := quExcepcions.Locate('EGrup', taTaula.FieldByName('Codi').Value, []);
    TTipusNode.tnTipusSubGrup:
      Result := quExcepcions.Locate('ESubGrup', taTaula.FieldByName('Codi').Value,
        []);
    TTipusNode.tnTipusArticle:
      Result := quExcepcions.Locate('EArticle', taTaula.FieldByName('Codi').Value,
        []);
  end;
  if not Result then
  begin
    Id := taTaula.FieldByName('Id').Value;
    pId := taTaula.FieldByName('ParentId').Value;
    if pId <> -1 then
    begin
      if taTaula.Locate('Id', pId, []) then
      begin
        Result := TeExcepcio(taTaula);
      end;
      taTaula.Locate('Id', Id, []);
    end;
  end;
end;
```

##### procedure AplicarExcepcions(taTaula: TdxMemData)

[TeExcepcio](#function-teexcepciotataula-tdxmemdata-boolean)\
[AssignarExcepcio](#procedure-assignarexcepciotataula-tdxmemdata)

```Delphi
procedure AplicarExcepcions(taTaula: TdxMemData);
begin
  taTaula.DisableControls;
  taTaulaETarifaChange := False;
  taTaula.Filtered := False;
  taTaula.First;
  while not taTaula.eof do
  begin
    if TeExcepcio(taTaula) then
      AssignarExcepcio(taTaula);
    taTaula.Next;
  end;
  taTaulaETarifaChange := true;
  taTaula.EnableControls;
end;
```

### function ObtenirPermisosExcepcions(TipusClient: string; Columna: TColumnesPermisos): Boolean

Obte quins permisos te l'usuari a la hora de fer pressupostos.

```Delphi
function TdmPressupostosPreus.ObtenirPermisosExcepcions(TipusClient: string;
  Columna: TColumnesPermisos): Boolean;
var
  permis: string;
begin
  if dmPressupostos.EsAdministrador then
    permis := 'ADMIN'
  else
    permis := TipusClient;

  with quPermisosExcepcions, Parameters do
  begin
    if Active and (ParamByName('TipusClient').Value = permis) then
      Result := FieldByName(TConversions<TColumnesPermisos>.EnumerationToString(Columna)).Value
    else
    begin
      Close;
      ParamByName('TipusClient').Value := permis;
      Open;

      Result := ObtenirPermisosExcepcions(TipusClient, Columna);
    end;
  end;
end;
```

### function EsModificable(Ambit: TTipusNode; TipusExcepcio: Integer; Tarifa: Integer; TipusABC: string): Boolean

[AmbitModificable](#function-ambitmodificable-boolean)
[TipusExcepcioModificable](#function-tipusexcepciomodificable-boolean)
[TarifaModificable](#function-tarifamodificable-boolean)

```Delphi
function TdmPressupostosPreus.EsModificable(Ambit: TTipusNode; TipusExcepcio:
  Integer; Tarifa: Integer; TipusABC: string): Boolean;
begin
  Result := False;
  if AmbitModificable then
    if TipusExcepcioModificable then
    begin
      if TipusExcepcio = 3 then
        Exit(TarifaModificable)
      else
        Exit(true);
    end;
end;
```

#### function AmbitModificable: Boolean

[TColumnesPermisos]

```Delphi
function AmbitModificable: Boolean;
begin
  if Ambit <> tnTipusMarca then
    Exit(ObtenirPermisosExcepcions(quPressupostsCapcaleraTipusABCClient.Value,
      TColumnesPermisos(Ambit)));
  Exit(False)
end;
```

#### function TipusExcepcioModificable: Boolean

[TColumnesPermisos]

```Delphi
function TipusExcepcioModificable: Boolean;
begin
  Exit(ObtenirPermisosExcepcions(quPressupostsCapcaleraTipusABCClient.Value,
    TColumnesPermisos(TipusExcepcio + 3)));
end;
```

#### function TarifaModificable: Boolean

[TColumnesPermisos]

```Delphi
function TarifaModificable: Boolean;
begin
  Exit(ObtenirPermisosExcepcions(quPressupostsCapcaleraTipusABCClient.Value,
    TColumnesPermisos(Tarifa + 7)));
end;
```

### procedure AplicarExcepcionsParesAFills

[AplicarExcepcions](#procedure-aplicarexcepcionstataula-tdxmemdata-1)

```Delphi
procedure TdmPressupostosPreus.AplicarExcepcionsParesAFills;
begin
  AplicarExcepcions(taDetallPressupostCongelat);
end;
```

#### procedure AplicarExcepcions(taTaula: TdxMemData)

[TeExcepcio](#function-teexcepciocheckexcepcio-boolean-boolean)\
[AssignarExcepcio](#procedure-assignarexcepcio)

```Delphi
procedure AplicarExcepcions(taTaula: TdxMemData);
var
  ETipusExcepcio, ETarifa, EAmbit: Integer;
  EPreuFix, EPercentIncCost, EImportIncCost, EPercentIncVenda: Double;
  EPressupost, PersonaAutoritza, DataAuto, DataSoli: Variant;
  mId: Integer;
begin
  if taTaula.RecordCount > 0 then
  begin
    taTaula.DisableControls;
    mId := taTaula.FieldByName('Id').Value;
    taTaula.Filtered := False;
    taTaula.First;
    taTaulaETarifaChange := False;
    while not taTaula.eof do
    begin
      if TeExcepcio(true) then
        AssignarExcepcio;
      if TeExcepcio(False) then
        AssignarExcepcio;
      taTaula.Next;
    end;
    taTaula.Locate('Id', mId, []);
    taTaulaETarifaChange := true;
    taTaula.EnableControls;
  end;
end;
```

#### procedure AssignarExcepcio

```Delphi
procedure AssignarExcepcio;
begin
  taTaula.Edit;
  taTaula.FieldByName('ETipusExcepcio').Value := ETipusExcepcio;
  taTaula.FieldByName('ETarifa').Value := ETarifa;
  taTaula.FieldByName('EPercentIncCost').Value := EPercentIncCost;
  taTaula.FieldByName('EImportIncCost').Value := EImportIncCost;
  taTaula.FieldByName('EPercentIncVenda').Value := EPercentIncVenda;
  taTaula.FieldByName('EPreuFix').Value := EPreuFix;
  taTaula.FieldByName('EAmbit').Value := EAmbit;
  taTaula.FieldByName('Epressupost').Value := EPressupost;
  taTaula.FieldByName('PersonaAutoritza').Value := PersonaAutoritza;
  taTaula.FieldByName('DataAutoritzat').Value := DataAuto;
  taTaula.FieldByName('DataSolicitat').Value := DataSoli;
  taTaula.Post;
end;
```

#### procedure CarregarExcepcio

```Delphi
procedure CarregarExcepcio;
begin
  ETipusExcepcio := taTaula.FieldByName('ETipusExcepcio').Value;
  ETarifa := taTaula.FieldByName('ETarifa').Value;
  if taTaula.FieldByName('EPercentIncCost').IsNull then
    showmessage(taTaula.FieldByName('Id').AsString);
  EPercentIncCost := taTaula.FieldByName('EPercentIncCost').Value;
  EImportIncCost := taTaula.FieldByName('EImportIncCost').Value;
  EPercentIncVenda := taTaula.FieldByName('EPercentIncVenda').Value;
  EPreuFix := taTaula.FieldByName('EPreuFix').Value;
  EAmbit := taTaula.FieldByName('EAmbit').Value;
  EPressupost := taTaula.FieldByName('Epressupost').Value;
  PersonaAutoritza := taTaula.FieldByName('PersonaAutoritza').Value;
  DataAuto := taTaula.FieldByName('DataAutoritzat').Value;
  DataSoli := taTaula.FieldByName('DataSolicitat').Value;
end;
```

#### function TeExcepcio(CheckExcepcio: Boolean): Boolean

[CarregarExcepcio](#procedure-carregarexcepcio)

```Delphi
function TeExcepcio(CheckExcepcio: Boolean): Boolean;
var
  Id, pId: Integer;
begin
  Result := (taTaula.FieldByName('ETipusExcepcio').Value <> -1) and ((CheckExcepcio
    and taTaula.FieldByName('EPressupost').Value) or (not CheckExcepcio));
  if not Result then
  begin
    Id := taTaula.FieldByName('Id').Value;
    pId := taTaula.FieldByName('ParentId').Value;
    if pId <> PARENT_ID_BASE then
    begin
      if taTaula.Locate('Id', pId, []) then
      begin
        Result := TeExcepcio(CheckExcepcio);
      end;
      taTaula.Locate('Id', Id, []);
    end;
  end
  else
  begin
    CarregarExcepcio;
  end;
end;
```

### procedure ProcessarPressupost(progressBar: IProgressBar)

[quPressupostsCapcalera](#qupressupostscapcalera)\
[ObtenirDataSetExcepcions](#function-obtenirdatasetexcepcionstipusarticle-integer-tdataset)\

Explicacio:\
Degut a que totes les linies filles tenen la excepcio del pare amb una marca de a qui aplica la excepcio. `Tipus` ens diu que la linia actual es, per exemple, un article i `EAmbit` ens explica que la excepcio actual es de la familia, com que no es la linia que aplica, no es tractara. En cas de que `Tipus` sigui Familia i `EAmbit` tambe sigui familia, voldra dir que la excepcio es de la propia linia i no de un pare, i s'haura de processar.

```Delphi
if (taDetallPressupostCongelat.FieldByName('Tipus').Value =
      taDetallPressupostCongelat.FieldByName('EAmbit').Value) and
      taDetallPressupostCongelat.FieldByName('EPressupost').Value and not (taDetallPressupostCongelatPersonaAutoritza.IsNull
      or (taDetallPressupostCongelatPersonaAutoritza.Value = 0)) then
```

[EsPotFerExcepcio](#function-espotferexcepcioambit-ttipusnode-codi-string-boolean)\
[FerExcepcio](#procedure-ferexcepcio)

```Delphi
procedure TdmPressupostosPreus.ProcessarPressupost(progressBar: IProgressBar);
var
  dsExcepcions: TDataSet;
  DataIniciExcepcionsPress, DataFiExcepcionsPress: TDateTime;
  useProgressBar: Boolean;
  s: string;
begin
  useProgressBar := Assigned(progressBar);
  if useProgressBar then
  begin
    progressBar.SetUpProgressBar(taDetallPressupostCongelat.RecordCount);
  end;
  taDetallPressupostCongelat.DisableControls;
  DataIniciExcepcionsPress := quPressupostsCapcaleraDataIniciEx.AsDateTime;
  DataFiExcepcionsPress := quPressupostsCapcaleraDataFiEx.AsDateTime;
  taDetallPressupostCongelat.First;
  while not taDetallPressupostCongelat.Eof do
  begin
    if useProgressBar then
      progressBar.Step(FieldNameAmbit(TTipusNode(taDetallPressupostCongelatTipus.AsInteger))
        + ': ' + taDetallPressupostCongelatCodi.AsString + ' - ' +
        taDetallPressupostCongelatNom.AsString);

    if (taDetallPressupostCongelat.FieldByName('Tipus').Value =
      taDetallPressupostCongelat.FieldByName('EAmbit').Value) and
      taDetallPressupostCongelat.FieldByName('EPressupost').Value and not (taDetallPressupostCongelatPersonaAutoritza.IsNull
      or (taDetallPressupostCongelatPersonaAutoritza.Value = 0)) then
    begin
      dsExcepcions := ObtenirDataSetExcepcions(taDetallPressupostCongelatTipusArticle.AsInteger);
      if EsPotFerExcepcio(TTipusNode(taDetallPressupostCongelatTipus.AsInteger),
        taDetallPressupostCongelatCodi.AsString) then
      begin
        FerExcepcio;
      end;
    end;
    taDetallPressupostCongelat.Next;
  end;
  if quArticlePreparaClients.Active then
    quArticlePreparaClients.Close;
  if quArticleCongelatClients.Active then
    quArticleCongelatClients.Close;
  if quArticleLlotjaExcepcions.Active then
    quArticleLlotjaExcepcions.Close;

  if useProgressBar then
    progressBar.StopProgressBar;

  taDetallPressupostCongelat.First;
  taDetallPressupostCongelat.EnableControls;
end;
```

#### procedure FerExcepcio

```Delphi
procedure FerExcepcio;
begin
  dsExcepcions.Append;
  dsExcepcions.FieldByName(FieldNameAmbit(TTipusNode(taDetallPressupostCongelatTipus.AsInteger))).AsString
    := taDetallPressupostCongelatCodi.AsString;
  dsExcepcions.FieldByName('Tarifa').Value := taDetallPressupostCongelatETarifa.Value;
  dsExcepcions.FieldByName('TipusExcepcio').Value :=
    taDetallPressupostCongelatETipusExcepcio.Value;
  dsExcepcions.FieldByName('PercentInc').Value :=
    taDetallPressupostCongelatEPercentIncCost.Value;
  dsExcepcions.FieldByName('ImportInc').Value :=
    taDetallPressupostCongelatEImportIncCost.Value;
  dsExcepcions.FieldByName('PercentIncVenda').Value :=
    taDetallPressupostCongelatEPercentIncVenda.Value;
  dsExcepcions.FieldByName('ImportFix').Value :=
    taDetallPressupostCongelatEPreuFix.Value;
  dsExcepcions.FieldByName('PersonaAutoritza').Value :=
    taDetallPressupostCongelatPersonaAutoritza.Value;
  dsExcepcions.FieldByName('DataSolicitat').Value :=
    taDetallPressupostCongelatDataSolicitat.Value;
  dsExcepcions.FieldByName('Client').Value := quPressupostsCapcaleraClient.Value;
  dsExcepcions.FieldByName('DataInici').Value := DataIniciExcepcionsPress;
  dsExcepcions.FieldByName('DataFi').Value := DataFiExcepcionsPress;
  dsExcepcions.Post;
end;
```

#### function ObtenirDataSetExcepcions(TipusArticle: Integer): TDataSet

Retorna la query a utilitzar segons el [TTipusArticle] passat per parametre i obre la connexio.

Com es veu aqui, els pressupostos estan preparats per funcionar amb llotja.

[quArticlePreparaClients](#quarticlepreparaclients)\
[quArticleCongelatClients](#quarticlecongelatclients)\
[quArticleLlotjaExcepcions](#quarticlellotjaexcepcions)

```Delphi
function ObtenirDataSetExcepcions(TipusArticle: Integer): TDataSet;
var
  ds: TADOQuery;
begin
  case TTipusArticle(TipusArticle) of
    taFresc:
      ds := quArticlePreparaClients;
    taCongelat:
      ds := quArticleCongelatClients;
    taLlotja:
      ds := quArticleLlotjaExcepcions;
  end;
  // Obrir Query
  with ds, Parameters do
  begin
    Close;
    ParamByName('Client').value := quPressupostsCapcaleraClient.Value;
    Open;
  end;
  Exit(ds);
end;
```

#### function FieldNameAmbit(Ambit: TTipusNode): string

Segons el [TTipusNode] et retorna el nom de la columna que has de accedir.

```Delphi
function FieldNameAmbit(Ambit: TTipusNode): string;
begin
  case Ambit of
    tnTipusFamilia:
      Exit('Familia');
    tnTipusGrup:
      Exit('Grup');
    tnTipusSubGrup:
      Exit('SubGrup');
    tnTipusArticle:
      Exit('Article');
  end;
end;
```

#### function EsPotFerExcepcio(Ambit: TTipusNode; Codi: string): Boolean

[FieldNameAmbit](#function-fieldnameambitambit-ttipusnode-string)

Aquesta funcio revisa que no s'estigui intentant fer una excepcio que pugui interferir amb una excepcio activa.

```Delphi
function EsPotFerExcepcio(Ambit: TTipusNode; Codi: string): Boolean;
var
  CanBeDone: Boolean;
begin
  dsExcepcions.Filter := FieldNameAmbit(Ambit) + ' = ' + QuotedStr(Codi);
  dsExcepcions.Filtered := True;
  dsExcepcions.First;
  CanBeDone := true;
  while (not dsExcepcions.Eof) and CanBeDone do
  begin
  // Verifiquem que no es solapi
    if ((dsExcepcions.FieldByName('DataInici').Value <= DataFiExcepcionsPress)
      or (dsExcepcions.FieldByName('DataFi').Value >= DataIniciExcepcionsPress)) then
      CanBeDone := false;
    dsExcepcions.Next;
  end;
  dsExcepcions.Filtered := false;
  dsExcepcions.Filter := '';
  Exit(CanBeDone);
end;
```

### procedure EliminarPressupost(Pressupost: Integer)

[quEliminarPressupost](#queliminarpressupost)

```Delphi
procedure TdmPressupostosPreus.EliminarPressupost(Pressupost: Integer);
begin
  with quEliminarPressupost, Parameters do
  begin
    ParamByName('Pressupost').Value := Pressupost;

    ExecSQL;
  end;
end;
```

## SQL

### quClearPressupost

```SQL
Delete From ClientsPressupostsDetall
where Pressupost = :Pressupost
```

### quArticles

Llista les seguents columnes: [Article, Nom Article, PreuCost, T0, T1, T2, T3, TB, Familia, Grup, SubGrup, Marca, NomMarca, MargeT0, MargeT1, MargeT2, MargeT3, MargeT4]

Es filtra segons:

- 0 - Fresc: Familia <> "PFC" i Familia.PeixFresc = "S"
- 1 - Llotja: Familia = "PFC"
- 2 - Congelat: Familia = ["CONGE", "REFRI", "AMBNT"]

```SQL
Declare @TipusArticles int

Set @TipusArticles = IsNull(:TipusArticles, 0) -- 0 Fresc, 1 Llotja, 2 Congelat

Select *, 
case when PreuCost <> 0 then Round((T0-PreuCost)*100/PreuCost,2) else 0 end MargeT0,
case when PreuCost <> 0 then Round((T1-PreuCost)*100/PreuCost,2) else 0 end MargeT1,
case when PreuCost <> 0 then Round((T2-PreuCost)*100/PreuCost,2) else 0 end MargeT2,
case when PreuCost <> 0 then Round((T3-PreuCost)*100/PreuCost,2) else 0 end MargeT3,
case when PreuCost <> 0 then Round((TB-PreuCost)*100/PreuCost,2) else 0 end MargeT4
from 
(Select
A.Article,
A.NomArticle,

isnull(Case @TipusArticles
when 0 then ap.PreuCost2
when 2 then ac.PreuCost2
else 0 end, 0)
as PreuCost,

IsNull(A.PreuVenda5,0) T0,
IsNull(A.PreuVenda1,0) T1,
IsNull(A.PreuVenda2,0) T2,
IsNull(A.PreuVenda3,0) T3,
IsNull(A.PreuVenda4,0) TB,

A.Familia,
A.Grup,
A.Subgrup,
A.Marca,
A.NomMarca
from Articles A
left Join Families f on f.Familia = A.Familia

left join ArticlePrepara ap on ap.Article = a.Article and ap.Data = (select TOP(1) data from ArticlePrepara order by Data desc)
left join ArticleCongelat ac on ac.Article = a.Article
left join Bundles b on b.ArticlePare = a.Article
where a.DataBaixa is null and
  ((@TipusArticles = 2 and f.Familia in ('CONGE','REFRI','AMBNT'))
    or (@TipusArticles = 0 and f.PeixFresc = 'S' and f.Familia <> 'PFC')
    or (@TipusArticles = 1 and f.Familia = 'PFC'))
	and b.Id is null
	) taula
order by NomArticle
```

### quFamilies

Es filtra segons:

- 0 - Fresc: Familia <> "PFC" i Familia.PeixFresc = "S"
- 1 - Llotja: Familia = "PFC"
- 2 - Congelat: Familia = ["CONGE", "REFRI", "AMBNT"]

```SQL
Declare @TipusArticles int

Set @TipusArticles = IsNull(:TipusArticles, 0) -- 0 Fresc, 1 Llotja, 2 Congelat

SELECT f.[Familia]
      ,[Nom]
  FROM [Puignau].[dbo].[Families] f
where (@TipusArticles = 2 and f.Familia in ('CONGE','REFRI','AMBNT'))
    or (@TipusArticles = 0 and f.PeixFresc = 'S' and f.Familia <> 'PFC' and f.Familia <> 'PFE' )
    or (@TipusArticles = 1 and f.Familia = 'PFC')
```

### quGrups

Es filtra segons:

- 0 - Fresc: Familia <> "PFC" i Familia.PeixFresc = "S"
- 1 - Llotja: Familia = "PFC"
- 2 - Congelat: Familia = ["CONGE", "REFRI", "AMBNT"]

```SQL
Declare @TipusArticles int

Set @TipusArticles = IsNull(:TipusArticles, 0) -- 0 Fresc, 1 Llotja, 2 Congelat

  SELECT ga.[Grup]
      ,ga.[Nom]
      ,ga.[Familia]
  FROM [Puignau].[dbo].[GrupsArticles] ga
  left join families f on f.familia = ga.familia
  where (@TipusArticles = 2 and f.Familia in ('CONGE','REFRI','AMBNT'))
    or (@TipusArticles = 0 and f.PeixFresc = 'S' and f.Familia <> 'PFC')
    or (@TipusArticles = 1 and f.Familia = 'PFC')
```

### quSubGrups

Es filtra segons:

- 0 - Fresc: Familia <> "PFC" i Familia.PeixFresc = "S"
- 1 - Llotja: Familia = "PFC"
- 2 - Congelat: Familia = ["CONGE", "REFRI", "AMBNT"]

```SQL
Declare @TipusArticles int

Set @TipusArticles = IsNull(:TipusArticles, 0) -- 0 Fresc, 1 Llotja, 2 Congelat

Select sg.SubGrup, sg.Nom, sg.Grup
from SubGrups sg
left join GrupsArticles ga on ga.Grup = sg.Grup
left join families f on f.familia = ga.familia
where (@TipusArticles = 2 and f.Familia in ('CONGE','REFRI','AMBNT'))
    or (@TipusArticles = 0 and f.PeixFresc = 'S' and f.Familia <> 'PFC')
    or (@TipusArticles = 1 and f.Familia = 'PFC')
```

### quMarques

```SQL
SELECT f.Marca
      ,[NomMarca]
  FROM [Puignau].[dbo].Marques f
```

### quDadesPressupostAutoritzades

```SQL
SELECT Id
      ,Familia
      ,Grup
      ,SubGrup
      ,Article
      ,Tarifa
	    ,TipusExcepcio
	    ,PercentIncCost
	    ,ImportIncCost
	    ,PercentIncVenda
	    ,ImportFix
      ,Pressupost
	    ,Marca
      ,isnull(Familia,'')+isnull(Grup,'')+isnull(SubGrup,'')+isnull(Article ,'')+isnull(Marca ,'') Ambit
	    ,case 
        when Article is not null then 3
		    when SubGrup is not null then 2
		    when Grup is not null then 1
		    when Familia is not null then 0
		    when Marca is not null then 4
		    else -1 end Tipus
      ,DataSolicitat
      ,DataAutoritzat
      ,PersonaAutoritza
FROM [Puignau].[dbo].[ClientsPressupostsDetall]
where Pressupost = :Pressupost and DataSolicitat is not null and DataAutoritzat is not null
```

### quDadesPressupost

```SQL
SELECT Id
      ,Familia
      ,Grup
      ,SubGrup
      ,Article
      ,Tarifa
	    ,TipusExcepcio
	    ,PercentIncCost
	    ,ImportIncCost
	    ,PercentIncVenda
	    ,ImportFix
      ,Pressupost
	    ,Marca
      ,isnull(Familia,'')+isnull(Grup,'')+isnull(SubGrup,'')+isnull(Article ,'')+isnull(Marca ,'') Ambit
	    ,case 
        when Article is not null then 3
		    when SubGrup is not null then 2
		    when Grup is not null then 1
		    when Familia is not null then 0
		    when Marca is not null then 4
		    else -1 end Tipus
      ,DataSolicitat
      ,DataAutoritzat
      ,PersonaAutoritza
FROM [Puignau].[dbo].[ClientsPressupostsDetall]
where Pressupost = :Pressupost
```

### quPressupostAutoritzar

```SQL
SELECT D.Id, D.Tarifa, D.Pressupost, D.TipusExcepcio, D.PercentIncCost,
D.ImportIncCost, D.PercentIncVenda, D.ImportFix, D.DataSolicitat,
D.DataAutoritzat, D.PersonaAutoritza,
case when D.Article is not null then 3
     when D.SubGrup is not null then 2
		 when D.Grup is not null then 1
		 when D.Familia is not null then 0
		 when D.Marca is not null then 4
   	 else -1
end as Tipus,
Isnull(D.Familia,'') + Isnull(D.Grup,'') + Isnull(D.SubGrup,'')+
Isnull(D.Article ,'')+ Isnull(D.Marca ,'') as Ambit,

IsNull((Select F.Nom From Families as F Where F.Familia = IsNull(D.Familia, '')), '') as NomFamilia,
IsNull((Select G.Nom From GrupsArticles as G Where G.Familia = IsNull(D.Familia, '') and G.Grup = IsNull(D.Grup, '')), '') as NomGrup,
IsNull((Select S.Nom From SubGrups as S Where S.Grup = IsNull(D.Grup, '') and S.Subgrup = IsNull(D.SubGrup, '')), '') as NomSubGrup,
D.Marca,
IsNull((Select A.NomArticle From Articles as A Where A.Article = IsNull(D.Article, '')), '') as NomArticle,

C.Client as Client, C.Nom as NomClient,
C.Comercial, PC.Descripcio as NomComercial
From ClientsPressupostsDetall as D
Join ClientsPressuposts as P on P.Id = D.Pressupost
Join Clients as C on C.Client = P.Client
Left Join PersonalComercial as PC on PC.Comercial = C.Comercial
Where D.DataSolicitat is not null
and D.DataAutoritzat is null
and Isnull(D.TipusExcepcio,-1) <> -1
```

### quInsertDetallPressupost

```SQL
SELECT TOP(0) [Id]
      ,[Familia]
      ,[Grup]
      ,[SubGrup]
      ,[Article]
      ,[Tarifa]
      ,[Pressupost]
      ,[TipusExcepcio]
      ,[PercentIncCost]
      ,[ImportIncCost]
      ,[PercentIncVenda]
      ,[ImportFix]
      ,[DataSolicitat]
      ,[DataAutoritzat]
      ,[PersonaAutoritza]
      ,[Marca]
  FROM [dbo].[ClientsPressupostsDetall]
```

### quPressupostsCapcalera

```SQL
Declare @Client int, @Comercial int, @Pressupost int

Set @Client = :Client
Set @Comercial = :Comercial
Set @Pressupost = :Pressupost

SELECT cp.Id
      ,cp.Client
      ,cp.Data
      ,cp.Persona
      ,c.Nom
      ,c.Comercial
	  ,Cast(c.Comercial as varchar) + ' - ' + pc.Descripcio NomComercial
	  ,cp.DataAcceptat
	  ,cp.DataIniciEx
	  ,cp.DataFiEx
	  ,cp.DataProcessat
	  ,cp.UsuariProcessat
,c.TipusABCClient
FROM [dbo].[ClientsPressuposts] cp
	left join Clients c on cp.Client = c.Client
	left join PersonalComercial pc on c.Comercial = pc.Comercial
where 
	(@Pressupost = -1 or @Pressupost = cp.Id)
	and (@Comercial = -1 or @Comercial = c.Comercial)
	and (@Client = -1 or @Client = cp.Client)
```

### quAvisosEmail

Tot s'envia a la paula, menys el que es de Congelat i es Preu Fix.

```SQL
Declare @Pressupost integer

Set @Pressupost = isnull(:Pressupost,-1)

select distinct
case 
when (Familia = 'CONGE' or Familia = 'AMBNT' or Familia = 'REFRI') and TipusExcepcio = 4 then 
(Select STRING_AGG(Email,';') from Emails where Tipus = 45)
else
(Select STRING_AGG(Email,';') from Emails where Tipus = 43)
end Email
from (
select concat(f.Familia, g.Familia, sgg.Familia, a.Familia) Familia, cpd.TipusExcepcio
from ClientsPressupostsDetall cpd

left join Families f on f.Familia = cpd.Familia
left join GrupsArticles g on cpd.Grup = g.Grup
left join Subgrups sg on cpd.SubGrup = sg.Subgrup
left join GrupsArticles sgg on sg.Grup = sgg.Grup
left join Articles a on a.Article = cpd.Article

where Pressupost = @Pressupost and (PersonaAutoritza is null or PersonaAutoritza = 0) and TipusExcepcio is not null) taula
group by Familia, TipusExcepcio
```

### quExcepcions

```SQL
Declare @Data date, @Client int

set @Data = GETDATE()
Set @Client = :Client

Select EFamilia, 
	EGrup, 
	ESubgrup,
	EArticle, 
	ETipusExcepcio, 
	ETarifa,
	EPercentInc,
	EImportInc,
	EPercentIncVenda,
	EImportFix,
	case
	when EFamilia is not null then 0
	when EGrup is not null then 1
	when ESubgrup is not null then 2
	when EArticle is not null then 3
	end as Ambit
from (
Select 
	Familia EFamilia, 
	Grup EGrup, 
	Subgrup ESubgrup,
	Article EArticle, 
	TipusExcepcio ETipusExcepcio, 
	Tarifa ETarifa,
	PercentInc EPercentInc,
	ImportInc EImportInc,
	PercentIncVenda EPercentIncVenda,
	ImportFix EImportFix,
	DataInici,
	DataFi,
	Client
from 
	ArticlePreparaClients 
union all 
Select 
	Familia EFamilia, 
	Grup EGrup, 
	Subgrup ESubgrup,
	Article EArticle, 
	TipusExcepcio ETipusExcepcio, 
	Tarifa ETarifa, 
	PercentInc EPercentInc, 
	ImportInc EImportInc,
	PercentIncVenda EPercentIncVenda, 
	ImportFix EImportFix,
	DataInici,
	DataFi,
	Client
from 
	ArticleCongelatClients 
union all 
Select 
	Familia EFamilia, 
	Grup EGrup, 
	Subgrup ESubgrup,
	Article EArticle, 
	TipusExcepcio ETipusExcepcio, 
	Tarifa ETarifa, 
	PercentInc EPercentInc, 
	ImportInc EImportInc,
	PercentIncVenda EPercentIncVenda, 
	ImportFix EImportFix,
	DataInici,
	DataFi,
	Client
from 
	ArticleLlotjaExcepcions) a
Where 
	DataInici <= @Data 
	and @Data <= DataFi 
	and @Client = Client
```

### quEliminarPressupost

```SQL
Declare @Pressupost Int

Set @Pressupost = :Pressupost

Delete from ClientsPressuposts
where Id = @Pressupost;

Delete from ClientsPressupostsDetall
where Pressupost = @Pressupost;
```

### quArticlePreparaClients

```SQL
Declare @Client integer

Set @Client = :Client

SELECT [Id]
      ,[Familia]
      ,[Grup]
      ,[Subgrup]
      ,[Article]
      ,[Client]
      ,[Tarifa]
      ,[TipusExcepcio]
      ,[PercentInc]
      ,[ImportInc]
      ,[PercentIncVenda]
      ,[ImportFix]
      ,[DataInici]
      ,[DataFi]
      ,[PersonaAutoritza]
      ,[AliasPersonaAutoritza]
      ,[Motiu]
      ,[DataAlta]
      ,[EsFeinaNeteja]
      ,[DataSolicitat]
      ,[PersonaSolicitat]
      ,[ExcGrup]
      ,[AliasPersonaSolicitat]
  FROM [Puignau].[dbo].[ArticlePreparaClients]
Where Client = @Client
```

### quArticleCongelatClients

```SQL
Declare @Client integer

Set @Client = :Client

SELECT [Id]
      ,[Familia]
      ,[Grup]
      ,[Subgrup]
      ,[Article]
      ,[Client]
      ,[Tarifa]
      ,[TipusExcepcio]
      ,[PercentInc]
      ,[ImportInc]
      ,[PercentIncVenda]
      ,[ImportFix]
      ,[DataInici]
      ,[DataFi]
      ,[PersonaAutoritza]
      ,[AliasPersonaAutoritza]
      ,[Motiu]
      ,[DataSolicitat]
      ,[PersonaSolicitat]
      ,[ExcGrup]
      ,[AliasPersonaSolicitat]
  FROM [Puignau].[dbo].[ArticleCongelatClients]
  Where Client = @Client
```

### quArticleLlotjaExcepcions

```SQL
Declare @Client integer

Set @Client = :Client

SELECT TOP (1000) [Id]
      ,[TipusLlotja]
      ,[Familia]
      ,[Grup]
      ,[Subgrup]
      ,[Article]
      ,[Client]
      ,[Tarifa]
      ,[TipusExcepcio]
      ,[PercentInc]
      ,[ImportInc]
      ,[PercentIncVenda]
      ,[ImportFix]
      ,[DataInici]
      ,[DataFi]
      ,[PersonaAutoritza]
      ,[AliasPersonaAutoritza]
      ,[Motiu]
      ,[DataSolicitat]
      ,[PersonaSolicitat]
      ,[ExcGrup]
      ,[AliasPersonaSolicitat]
  FROM [Puignau].[dbo].[ArticleLlotjaExcepcions]
Where Client = @Client
```

[TTipusArticle]: ../Library/AFuncions.md#ttipusarticle
[TTipusNode]: ../Library/AFuncions.md#ttipusnode
[TColumnesPermisos]: ../Library/AFuncions.md#tcolumnespermisos
