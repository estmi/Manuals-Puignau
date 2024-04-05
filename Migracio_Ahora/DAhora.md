# [..](..)\Migracio Ahora\DAhora

## Index

- [Index](#index)
- [Descripcio](#descripcio)
- [Interface](#interface)
  - [published](#published)
  - [private](#private)
  - [public](#public)
- [Events](#events)
  - [procedure quClientsEconomicosCalcFields(DataSet: TDataSet)](#procedure-quclientseconomicoscalcfieldsdataset-tdataset)
- [Metodes](#metodes)
  - [procedure CrearSerieFacturacio(SerieFactura, SerieFacturaRectificativa: Integer; CodSerie, Descrip: string)](#procedure-crearseriefacturacioseriefactura-seriefacturarectificativa-integer-codserie-descrip-string)
  - [procedure UpdateEconomicos(IdCliente: string; TipoFacturacion: string; Descuento: double; ProntoPago: double; IdIVA, DiaPago1, DiaPago2: Integer; Riesgo: double; Portes: string; Periodicidad: Integer; FormaPagament: Integer; Diari: Integer)](#procedure-updateeconomicosidcliente-string-tipofacturacion-string-descuento-double-prontopago-double-idiva-diapago1-diapago2-integer-riesgo-double-portes-string-periodicidad-integer-formapagament-integer-diari-integer)
  - [procedure InsertarClient(Tipus: Integer; NomClient: string; NomFiscal: string; IdClient: Integer; Direccio: string; Telf: string; Poblacio: string; DataAlta, DataBaixa: TDateTime; IdTipus2: Integer; Nif: string; Nivel: Integer; TipusCaixaPrep: Integer; Merma: Boolean; Gel: Boolean; EnviarEtiqMail: Boolean; CodPostal: string; TipusFacturacio, TipusABC: string; Potencial, Objectiu: double; IniciVac, FiVac: TDateTime)](#procedure-insertarclienttipus-integer-nomclient-string-nomfiscal-string-idclient-integer-direccio-string-telf-string-poblacio-string-dataalta-databaixa-tdatetime-idtipus2-integer-nif-string-nivel-integer-tipuscaixaprep-integer-merma-boolean-gel-boolean-enviaretiqmail-boolean-codpostal-string-tipusfacturacio-tipusabc-string-potencial-objectiu-double-inicivac-fivac-tdatetime)
  - [procedure ExportarClients(Titol: string; query: TADOQuery)](#procedure-exportarclientstitol-string-query-tadoquery)
  - [procedure CanviarParametreComprobacioNIF(Activar: Boolean)](#procedure-canviarparametrecomprobacionifactivar-boolean)
  - [procedure ExportarClientsPrigestAhora](#procedure-exportarclientsprigestahora)
  - [procedure ExportarClientsAmbPlantesPrigestAhora](#procedure-exportarclientsambplantesprigestahora)
  - [procedure ExportarPlantesPrigestAhora](#procedure-exportarplantesprigestahora)
  - [procedure ExportarPlantesPrigestAhora2](#procedure-exportarplantesprigestahora2)
  - [procedure ProcesUpdateEconomicos](#procedure-procesupdateeconomicos)
  - [procedure DeleteClients](#procedure-deleteclients)
  - [procedure GenerarSeriesClients](#procedure-generarseriesclients)
- [SQL](#sql)
  - [BD Ahora](#bd-ahora)
    - [quAddClient](#quaddclient)
    - [quInsertSerie](#quinsertserie)
    - [quUpdateEconomicos](#quupdateeconomicos)
    - [quActivarDesactivarControlNIF](#quactivardesactivarcontrolnif)
    - [quDeleteClients](#qudeleteclients)
  - [BD Prigest](#bd-prigest)
    - [quClientsSensePlantaPrigest](#quclientssenseplantaprigest)
    - [quClientsPlantesPrigest](#quclientsplantesprigest)
    - [quClientsAmbPlantes](#quclientsambplantes)
    - [quClientsPlantesPrigest2](#quclientsplantesprigest2)
    - [quClientsEconomicos](#quclientseconomicos)
    - [quDiarisSeries](#qudiarisseries)

## Descripcio

## Interface

### published

```Delphi
procedure quClientsEconomicosCalcFields(DataSet: TDataSet);
```

### private

```Delphi
procedure CrearSerieFacturacio(SerieFactura, SerieFacturaRectificativa:
  Integer; CodSerie, Descrip: string);
procedure UpdateEconomicos(IdCliente, TipoFacturacion: string; Descuento,
  ProntoPago: double; IdIVA, DiaPago1, DiaPago2: Integer; Riesgo: double;
  Portes: string; Periodicidad, FormaPagament, Diari: Integer);
procedure InsertarClient(Tipus: Integer; NomClient, NomFiscal: string;
  IdClient: Integer; Direccio, Telf, Poblacio: string; DataAlta, DataBaixa:
  TDateTime; IdTipus2: Integer; Nif: string; Nivel, TipusCaixaPrep: Integer;
  Merma, Gel, EnviarEtiqMail: Boolean; CodPostal, TipusFacturacio, TipusABC:
  string; Potencial, Objectiu: double; IniciVac, FiVac: TDateTime);
procedure ExportarClients(Titol: string; query: TADOQuery);
```

### public

```Delphi
procedure CanviarParametreComprobacioNIF(Activar: Boolean);
procedure ExportarClientsPrigestAhora;
procedure ExportarClientsAmbPlantesPrigestAhora;
procedure ExportarPlantesPrigestAhora;
procedure ExportarPlantesPrigestAhora2;
procedure ProcesUpdateEconomicos;
procedure DeleteClients;
procedure GenerarSeriesClients;
```

## Events

### procedure quClientsEconomicosCalcFields(DataSet: TDataSet)

Calcula la forma de pagament d'Ahora.

[FormaPagamentAhora]

```Delphi
procedure TdmAhora.quClientsEconomicosCalcFields(DataSet: TDataSet);
begin
  with DataSet do
  begin
    FieldByName('FormaPagoAhora').Value := Ord(FormaPagamentAhora(FieldByName('CLIE_FORMAPAGAMENT').AsInteger,
      FieldByName('CLIE_PRIMERVENCIMENT').AsInteger));
  end;
end;
```

## Metodes

### procedure CrearSerieFacturacio(SerieFactura, SerieFacturaRectificativa: Integer; CodSerie, Descrip: string)

[quInsertSerie](#quinsertserie)

```Delphi
procedure TdmAhora.CrearSerieFacturacio(SerieFactura, SerieFacturaRectificativa:
  Integer; CodSerie, Descrip: string);
begin
  try
    with quInsertSerie, Parameters do
    begin
      ParamByName('SerieFactura').Value := SerieFactura;
      ParamByName('CodSerie').Value := CodSerie;
      ParamByName('SerieRectificativa').Value := SerieFacturaRectificativa;
      ParamByName('Descrip').Value := Descrip;
      ExecSQL;
    end;
  except
    on E: Exception do
    begin
      TGFMemoPopUp.PopUp(E.Message);
      TGFMemoPopUp.PopUp(quInsertSerie);
    end;
  end;
end;
```

### procedure UpdateEconomicos(IdCliente: string; TipoFacturacion: string; Descuento: double; ProntoPago: double; IdIVA, DiaPago1, DiaPago2: Integer; Riesgo: double; Portes: string; Periodicidad: Integer; FormaPagament: Integer; Diari: Integer)

En cas de saltar excepcio, com que podria ser algun dels clients que no hem traspassat, revisem que la excepcio no es refereixi a un d'aquests.

[quUpdateEconomicos](#quupdateeconomicos)

```Delphi
procedure TdmAhora.UpdateEconomicos(IdCliente: string; TipoFacturacion: string;
  Descuento: double; ProntoPago: double; IdIVA, DiaPago1, DiaPago2: Integer;
  Riesgo: double; Portes: string; Periodicidad: Integer; FormaPagament: Integer;
  Diari: Integer);
var
  v: Integer;
begin
  try
    with quUpdateEconomicos, Parameters do
    begin
      ParamByName('Client').Value := IdCliente;
      ParamByName('TipusFacturacio').Value := TipoFacturacion;
      ParamByName('Descuento').Value := Descuento;
      ParamByName('ProntoPago').Value := ProntoPago;
      ParamByName('IdIVA').Value := IdIVA;
      ParamByName('DiaPago1').Value := DiaPago1;
      ParamByName('DiaPago2').Value := DiaPago2;
      ParamByName('Riesgo').Value := Riesgo;
      ParamByName('IdPortes').Value := Portes;
      ParamByName('PeriodicidadPago').Value := Periodicidad;
      ParamByName('FormaPago').Value := FormaPagament;
      ParamByName('Diari').Value := Diari;
      Open;
      Close;
    end;
  except
    on E: Exception do
    begin
      v := Pos('CLIENT NO TROBAT', E.Message);
      if v <= 0 then
      begin
        TGFMemoPopUp.PopUp(E.Message);
        TGFMemoPopUp.PopUp(quUpdateEconomicos);
      end;
    end;
  end;
end;
```

### procedure InsertarClient(Tipus: Integer; NomClient: string; NomFiscal: string; IdClient: Integer; Direccio: string; Telf: string; Poblacio: string; DataAlta, DataBaixa: TDateTime; IdTipus2: Integer; Nif: string; Nivel: Integer; TipusCaixaPrep: Integer; Merma: Boolean; Gel: Boolean; EnviarEtiqMail: Boolean; CodPostal: string; TipusFacturacio, TipusABC: string; Potencial, Objectiu: double; IniciVac, FiVac: TDateTime)

En cas de saltar excepcio, utilitzo aquest codi per mostrar dos memos per pantalla els quals em permeten veure l'error i els parametres utilitzats per a replicar.

```Delphi
on E: Exception do
begin
  TGFMemoPopUp.PopUp(E.Message);
  TGFMemoPopUp.PopUp(quAddClient);
end;
```

[quAddClient](#quaddclient)

```Delphi
procedure TdmAhora.InsertarClient(Tipus: Integer; NomClient: string; NomFiscal:
  string; IdClient: Integer; Direccio: string; Telf: string; Poblacio: string;
  DataAlta, DataBaixa: TDateTime; IdTipus2: Integer; Nif: string; Nivel: Integer;
  TipusCaixaPrep: Integer; Merma: Boolean; Gel: Boolean; EnviarEtiqMail: Boolean;
  CodPostal: string; TipusFacturacio, TipusABC: string; Potencial, Objectiu:
  double; IniciVac, FiVac: TDateTime);
begin
  try
    with quAddClient, Parameters do
    begin
      ParamByName('IdTipo').Value := Tipus;
      ParamByName('NomClient').Value := NomClient;
      ParamByName('NomFiscal').Value := NomFiscal;
      ParamByName('CLIE_CLIENT').Value := IdClient;
      ParamByName('Direccio').Value := Direccio;
      ParamByName('Poblacio').Value := Poblacio;
      ParamByName('Telf').Value := Telf;
      if DataAlta <= 0 then
        ParamByName('DataAlta').Value := Unassigned
      else
        ParamByName('DataAlta').Value := DataAlta;
      if DataBaixa <= 0 then
        ParamByName('DataBaixa').Value := Unassigned
      else
        ParamByName('DataBaixa').Value := DataBaixa;
      ParamByName('Tipus2').Value := IdTipus2;
      ParamByName('Nif').Value := Nif;
      ParamByName('Nivel').Value := Nivel;
      ParamByName('Gel').Value := Gel;
      ParamByName('EnviarEtiqMail').Value := EnviarEtiqMail;
      ParamByName('Merma').Value := Merma;
      ParamByName('TipusCaixaPrep').Value := TipusCaixaPrep;
      ParamByName('CodigoPostal').Value := CodPostal;
      ParamByName('TipusFact').Value := TipusFacturacio;
      ParamByName('TipusABC').Value := TipusABC;
      ParamByName('Potencial').Value := Potencial;
      ParamByName('Objectiu').Value := Objectiu;

      if IniciVac <= 0 then
        ParamByName('IniciVacances').Value := Unassigned
      else
        ParamByName('IniciVacances').Value := IniciVac;

      if FiVac <= 0 then
        ParamByName('FiVacances').Value := Unassigned
      else
        ParamByName('FiVacances').Value := FiVac;

      Open;
      Close;
    end;
  except
    on E: Exception do
    begin
      TGFMemoPopUp.PopUp(E.Message);
      TGFMemoPopUp.PopUp(quAddClient);
    end;
  end;
end;
```

### procedure ExportarClients(Titol: string; query: TADOQuery)

Com que hi ha varis casos diferents per insertar els clients, utilitzo aquest metode generic per a poder simplificar el codi.

[InsertarClient](#procedure-insertarclienttipus-integer-nomclient-string-nomfiscal-string-idclient-integer-direccio-string-telf-string-poblacio-string-dataalta-databaixa-tdatetime-idtipus2-integer-nif-string-nivel-integer-tipuscaixaprep-integer-merma-boolean-gel-boolean-enviaretiqmail-boolean-codpostal-string-tipusfacturacio-tipusabc-string-potencial-objectiu-double-inicivac-fivac-tdatetime)

```Delphi
procedure TdmAhora.ExportarClients(Titol: string; query: TADOQuery);
var
  f: TGFProgressBar;
begin
  with query do
  begin
    Close;
    Open;

    First;
    DisableControls;
  end;
  Application.CreateForm(TGFProgressBar, f);
  f.Title(Titol);
  f.SetUpProgressBar(query.RecordCount);
  while not query.Eof do
  begin
    f.Step(query.RecNo, query.FieldByName('CLIE_NOMCOMERCIAL').Value);
    InsertarClient(query.FieldByName('Tipus').AsInteger, query.FieldByName('CLIE_NOMCOMERCIAL').AsString,
      query.FieldByName('CLIE_NOMFISCAL').AsString, query.FieldByName('CLIE_CLIENT').AsInteger,
      query.FieldByName('CLIE_ADRECA').AsString, query.FieldByName('TELE_TELEFON').AsString,
      query.FieldByName('CLIE_POBLACIO').AsString, query.FieldByName('CLIE_DATAALTA').AsDateTime,
      query.FieldByName('CLIE_DATABAIXA').AsDateTime, query.FieldByName('Tipus2').AsInteger,
      query.FieldByName('CLIE_NIF').Value, query.FieldByName('Nivel').Value,
      query.FieldByName('TipusCaixa').AsInteger, query.FieldByName('Merma').AsInteger
      = 1, query.FieldByName('Gel').AsInteger = 1, query.FieldByName('EnviarEmail').AsInteger
      = 1, query.FieldByName('CLIE_CODIPOSTAL').AsString, query.FieldByName('TipusFact').AsString,
      query.FieldByName('TipusABC').AsString, query.FieldByName('Potencial').AsFloat,
      query.FieldByName('Objectiu').AsFloat, query.FieldByName('DataInici').AsDateTime,
      query.FieldByName('DataFi').AsDateTime);
    query.Next;
  end;
  f.StopProgressBar;
  f.Free;
  query.EnableControls;
end;
```

### procedure CanviarParametreComprobacioNIF(Activar: Boolean)

Activa i Desactiva la verificacio de NIF. Aixo ens permet insertar tots els clients, ja que n'hi ha alguns amb NIFs invalids.

[quActivarDesactivarControlNIF](#quactivardesactivarcontrolnif)

```Delphi
procedure TdmAhora.CanviarParametreComprobacioNIF(Activar: Boolean);
begin
  with quActivarDesactivarControlNIF, Parameters do
  begin
    if Activar then
      ParamByName('Activar').Value := 1
    else
      ParamByName('Activar').Value := 0;
    ExecSQL;
  end;
end;
```

### procedure ExportarClientsPrigestAhora

[quClientsSensePlantaPrigest](#quclientssenseplantaprigest)

```Delphi
procedure TdmAhora.ExportarClientsPrigestAhora;
begin
  ExportarClients('(1/4) Clients Normals Sense Planta de Prigest -> Ahora',
    quClientsSensePlantaPrigest);
end;
```

### procedure ExportarClientsAmbPlantesPrigestAhora

[quClientsAmbPlantes](#quclientsambplantes)

```Delphi
procedure TdmAhora.ExportarClientsAmbPlantesPrigestAhora;
begin
  ExportarClients('(2/4) Clients Amb Plantes de Prigest -> Ahora', quClientsAmbPlantes);
end;
```

### procedure ExportarPlantesPrigestAhora

[quClientsPlantesPrigest](#quclientsplantesprigest)

```Delphi
procedure TdmAhora.ExportarPlantesPrigestAhora;
begin
  ExportarClients('(3/4) Plantes de Clients de Prigest (ADRE_PRINCIPAL = ''S'') -> Ahora',
    quClientsPlantesPrigest);
end;
```

### procedure ExportarPlantesPrigestAhora2

[funcFormaPagamentAhora]

```Delphi
procedure TdmAhora.ExportarPlantesPrigestAhora2;
begin
  ExportarClients('(4/4) Plantes de Clients de Prigest (ADRE_PRINCIPAL = ''N'') -> Ahora',
    quClientsPlantesPrigest2);
end;
```

### procedure ProcesUpdateEconomicos

[UpdateEconomicos](#procedure-updateeconomicosidcliente-string-tipofacturacion-string-descuento-double-prontopago-double-idiva-diapago1-diapago2-integer-riesgo-double-portes-string-periodicidad-integer-formapagament-integer-diari-integer)

```Delphi
procedure TdmAhora.ProcesUpdateEconomicos;
var
  f: TGFProgressBar;
begin
  quClientsEconomicos.First;
  quClientsEconomicos.DisableControls;
  Application.CreateForm(TGFProgressBar, f);
  f.Title('Actualitzant dades Economicos Prigest -> Ahora');
  f.SetUpProgressBar(quClientsEconomicos.RecordCount);
  while not quClientsEconomicos.Eof do
  begin
    f.Step(quClientsEconomicos.RecNo, quClientsEconomicosCLIE_CLIENT.AsString);
    UpdateEconomicos(quClientsEconomicosCLIE_CLIENT.AsString,
      quClientsEconomicosCLIE_TIPUSFACTURACIO.Value,
      quClientsEconomicosCLIE_DTEC.Value, quClientsEconomicosCLIE_DTEPP.Value,
      quClientsEconomicosTipusIVA.Value, quClientsEconomicosCLIE_DIAPAGAMENT1.Value,
      quClientsEconomicosCLIE_DIAPAGAMENT2.Value, quClientsEconomicosCLIE_RISC.Value,
      quClientsEconomicosCLIE_PORTS.Value, quClientsEconomicosCLIE_PERIODICITAT.Value,
      quClientsEconomicosFormaPAgoAhora.AsInteger,
      quClientsEconomicosCODI_DIARIFACTURA.AsInteger);
    quClientsEconomicos.Next;
  end;
  f.StopProgressBar;
  f.Free;
  quClientsEconomicos.EnableControls;
end;
```

### procedure DeleteClients

Degut a que eliminar els clients es un proces llarg, utilitzem una progressbar per a poder visualitzar alguna cosa.

[quDeleteClients](#qudeleteclients)

```Delphi
procedure TdmAhora.DeleteClients;
var
  f: TGFProgressBar;
begin
  Application.CreateForm(TGFProgressBar, f);
  f.Title('Eliminar Clients');
  f.SetUpProgressBar(1);
  f.Step(0, 'Eliminant Clients...');
  quDeleteClients.ExecSQL;
  f.Step(1, 'Clients Eliminats');
  f.StopProgressBar;
  f.Free;
end;
```

### procedure GenerarSeriesClients

[quDiarisSeries](#qudiarisseries)

[CrearSerieFacturacio](#procedure-crearseriefacturacioseriefactura-seriefacturarectificativa-integer-codserie-descrip-string)

```Delphi
procedure TdmAhora.GenerarSeriesClients;
var
  f: TGFProgressBar;
begin
  with quDiarisSeries do
  begin
    Close;
    Open;
    First;
    DisableControls;
  end;
  Application.CreateForm(TGFProgressBar, f);
  f.Title('Generant Series de Clients');
  f.SetUpProgressBar(quDiarisSeries.RecordCount);
  while not quDiarisSeries.Eof do
  begin
    f.Step(quDiarisSeries.RecNo, quDiarisSeriesCODI_DIARIRECTIFICATIVA.AsString);
    CrearSerieFacturacio(quDiarisSeriesCODI_DIARIRECTIFICATIVA.AsInteger,
      quDiarisSeriesdiariRect.AsInteger, quDiarisSeriesCODI_SERIERECTIFICATIVA.AsString,
      quDiarisSeriesCODI_DESCRIPCIO.AsString + ' ' +
      quDiarisSeriesDIAR_DESCRIPCIO.AsString);
    quDiarisSeries.Next;
  end;
  f.StopProgressBar;
  f.Free;
  quDiarisSeries.EnableControls;
end;
```

## SQL

### BD Ahora

#### quAddClient

[Pers_PClientes_Datos_I]

```SQL
DECLARE @RC int
DECLARE @ClienteIN [dbo].[T_Nombre_Corto]
DECLARE @RazonSocialIN varchar(255)
DECLARE @DireccionIN [dbo].[T_Direccion]
DECLARE @IdTipoIN [dbo].[T_Id_Tipo]
DECLARE @NumTelefonoIN [dbo].[T_Num_Telefono]
DECLARE @FechaAltaIN [dbo].[T_Fecha_Corta]
DECLARE @NifIN [dbo].[T_NIF]
DECLARE @MicodIN varchar(15)
DECLARE @IdTipoOtroIN [dbo].[T_Id_Tipo]
DECLARE @IdPoblacionIN varchar(50)
DECLARE @DataBaixaIN datetime
DECLARE @EnviarEtiqMailIN bit
DECLARE @GelIN bit
DECLARE @TipusCaixPrepIN smallint
DECLARE @MermaIN bit
DECLARE @NivelIN tinyint
DECLARE @CodigoPostalIN [dbo].[T_Codigo_Postal] 
DECLARE @PotencialIN numeric(18,2)
DECLARE @ObjectiuIN numeric(18,2)
DECLARE @TipusABCIN varchar(1)
DECLARE @TipusFacturacioIN varchar(10)
DECLARE @IniciVacancesIN datetime
DECLARE @FiVacancesIN datetime

Set @IdTipoIN = isnull(:IdTipo, 0);
Set @ClienteIN = isnull(:NomClient, '');
Set @RazonSocialIN = isnull(:NomFiscal, '');
Set @MiCodIN = isnull(:CLIE_CLIENT, '')
Set @DireccionIN = isnull(:Direccio, '')
Set @NumTelefonoIN = isnull(:Telf, '')
Set @IdPoblacionIN = isnull(:Poblacio, '')
Set @FechaAltaIN = isnull(:DataAlta, '20000101')
Set @DataBaixaIN = :DataBaixa
Set @IdTipoOtroIN = isnull(:Tipus2, 0)
Set @NifIN = isnull(:Nif, '')
Set @NivelIN = isnull(:Nivel, 0)
Set @TipusCaixPrepIN = isnull(:TipusCaixaPrep, 0)
Set @MermaIN = isnull(:Merma, 0)
Set @GelIN = isnull(:Gel, 0)
Set @EnviarEtiqMailIN = isnull(:EnviarEtiqMail, 0)
Set @CodigoPostalIN = isnull(:CodigoPostal, '0')
Set @PotencialIN = isnull(:Potencial, 0)
Set @ObjectiuIN = isnull(:Objectiu, 0)
Set @TipusABCIN = isnull(:TipusABC, '0')
Set @TipusFacturacioIN = isnull(:TipusFact, '0')
Set @IniciVacancesIN = :IniciVacances
Set @FiVacancesIN = :FiVacances

EXECUTE @RC = [dbo].[Pers_PClientes_Datos_I] 
   @ClienteIN OUTPUT
  ,@DireccionIN OUTPUT
  ,@IdTipoIN OUTPUT
  ,@NumTelefonoIN OUTPUT
  ,@FechaAltaIN OUTPUT
  ,@NifIN OUTPUT
  ,@MicodIN OUTPUT
  ,@RazonSocialIN OUTPUT
  ,@IdTipoOtroIN OUTPUT
  ,@IdPoblacionIN OUTPUT
  ,@DataBaixaIN OUTPUT
  ,@EnviarEtiqMailIN OUTPUT
  ,@GelIN OUTPUT
  ,@TipusCaixPrepIN OUTPUT
  ,@MermaIN OUTPUT
  ,@NivelIN OUTPUT
  ,@CodigoPostalIN OUTPUT
  ,@PotencialIN
  ,@ObjectiuIN
  ,@TipusABCIN
  ,@TipusFacturacioIN
  ,@IniciVacancesIN
  ,@FiVacancesIN
  Select @RC Result
```

#### quInsertSerie

Inserta a la taula de `Series_Facturacion` les series de faturacio.

```SQL
DECLARE @SerieFactura int, @CuentaVentas T_Cuenta_Contable, @NOFacturable T_Booleano, @IdDelegacion T_Id_Delegacion, 
	@Descrip varchar(255), @Rectificativa T_Booleano, @Recuperacion T_Booleano, @Agrupar T_Booleano, @IdRetencion smallint,	
	@Codigo_Serie varchar(20), @Verificable bit, @CentroCoste varchar(50), @SerieRectificativa T_Serie, @CodSerie varchar(3)

Set @SerieFactura = isnull(:SerieFactura,(select isnull(max(SerieFactura),0)+1 from SEries_Facturacion))
Set @CodSerie = isnull(:CodSerie,'')
Set @Descrip = CONCAT(@CodSerie, ' - ', isnull(:Descrip,''))
Set @SerieRectificativa = isnull(:SerieRectificativa,0)
Set @CuentaVentas = NULL
Set @NOFacturable = 0
Set @IdDelegacion = 0
Set @Rectificativa = 0
Set @Recuperacion = 0
Set @Agrupar = 1
Set @IdRetencion = 0
Set @Codigo_Serie = cast(@SerieFactura as varchar(20))
Set @Verificable = 1
Set @CentroCoste = NULL

INSERT INTO [dbo].[Series_Facturacion]
           ([SerieFactura]
           ,[Descrip]
           ,[CuentaVentas]
           ,[NoFacturable]
           ,[IdDelegacion]
           ,[CentroCoste]
           ,[Agrupar]
           ,[Rectificativa]
           ,[Recuperacion]
           ,[SerieRectificativa]
           ,[IdRetencion]
           ,[CodSerie]
           ,[Codigo_Serie]
           ,[Verificable])
     VALUES
           (@SerieFactura
           ,@Descrip
           ,@CuentaVentas
           ,@NOFacturable
           ,@IdDelegacion
           ,@CentroCoste
           ,@Agrupar
           ,@Rectificativa
           ,@Recuperacion
           ,@SerieRectificativa
           ,@IdRetencion
           ,@CodSerie
           ,@Codigo_Serie
           ,@Verificable)
```

#### quUpdateEconomicos

[Pers_PClientes_Datos_Economicos_U]

```SQL
DECLARE @RC int
DECLARE @IdClienteIN [dbo].[T_Id_Cliente]
DECLARE @TipusFacturacioIN varchar(3)
DECLARE @DescuentoIN [dbo].[T_Decimal]
DECLARE @ProntoPagoIN [dbo].[T_Decimal]
DECLARE @IdIvaIN [dbo].[T_Id_Iva]
DECLARE @DiaPago1IN [dbo].[T_Dia_Pago]
DECLARE @DiaPago2IN [dbo].[T_Dia_Pago]
DECLARE @RiesgoIN [dbo].[T_Riesgo]
DECLARE @IdPortesIN [dbo].[T_Id_Portes]
DECLARE @PeriodicidadPagoIN [dbo].[T_Periodicidad_Pago]
DeCLARE @FormaPagoIN [dbo].[T_Forma_Pago]
DECLARE @SerieFacturacionIN [dbo].[T_Serie]

Set @IdClienteIN = ISNULL(:Client, 0);
Set @TipusFacturacioIN = isnull(:TipusFacturacio, 'D')
Set @DescuentoIN = isnull(:Descuento,0)
Set @ProntoPagoIN = isnull(:ProntoPago, 0)
Set @IdIvaIN = isnull(:IdIVA, 1)
Set @DiaPago1IN = isnull(:DiaPago1, 0)
Set @DiaPago2IN = isnull(:DiaPago2, 0)
Set @RiesgoIN = isnull(:Riesgo, 0)
Set @IdPortesIN = isnull(:IdPortes, 'D')
Set @PeriodicidadPagoIN = isnull(:PeriodicidadPago, 0)
Set @FormaPagoIN = isnull(:FormaPago, 0)
Set @SerieFacturacionIN = isnull(:Diari, 0)

EXECUTE @RC = [dbo].[Pers_PClientes_Datos_Economicos_U] 
   @TipusFacturacioIN
  ,@IdClienteIN
  ,@DescuentoIN
  ,@ProntoPagoIN
  ,@IdIvaIN
  ,@DiaPago1IN
  ,@DiaPago2IN
  ,@RiesgoIN
  ,@IdPortesIN
  ,@PeriodicidadPagoIN
  ,@FormaPagoIN
  ,@SerieFacturacionIN
```

#### quActivarDesactivarControlNIF

```SQL
Declare @Activar int
Set @Activar = isnull(:Activar, 1)
update Ceesi_configuracion
set Valor =
  case @Activar
    when 0 then 'OFF'
    when 1 then 'ON'
    else 'ON'
  end
where Parametro = 'NIF_NACIONAL_COMPROBAR'
```

#### quDeleteClients

Aquesta query crida l'stored d'eliminar el client 1 per 1. el primer cop es fa per les plantes i el segon per els clients.

```SQL
DECLARE @IdClient T_Id_cliente, @User T_CEESI_Usuario, @Date T_CEESI_Fecha_Sistema
DECLARE clients CURSOR FOR SELECT IdCliente, Usuario, FechaInsertUpdate FROM clientes_datos WHERE IdCliente <> '0' and Nivel = 1;

OPEN clients;

FETCH NEXT FROM clients into @Idclient,@User,@Date;

WHILE @@FETCH_STATUS = 0
BEGIN
	EXECUTE PClientes_Datos_D @IdClient,@User,@Date;
	FETCH NEXT FROM clients into @Idclient,@User,@Date;	
END;

CLOSE clients;

DEALLOCATE clients;

DECLARE clients CURSOR FOR SELECT IdCliente, Usuario, FechaInsertUpdate FROM clientes_datos WHERE IdCliente <> '0' and Nivel = 0;

OPEN clients;

FETCH NEXT FROM clients into @Idclient,@User,@Date;

WHILE @@FETCH_STATUS = 0
BEGIN
	EXECUTE PClientes_Datos_D @IdClient,@User,@Date;
	FETCH NEXT FROM clients into @Idclient,@User,@Date;	
END;

CLOSE clients;

DEALLOCATE clients;
```

### BD Prigest

#### quClientsSensePlantaPrigest

[ppn_clients]

```SQL
select *, 0 Nivel
from ppn_clients c
where (((Select count(CLIE_NIF) from clients c2 where c2.CLIE_NIF = c.clie_nif) = 1) and ifnull(ADRE_ADRECA,'') = ifnull(CLIE_ADRECA,''))
order by CLIE_NOMCOMERCIAL
```

#### quClientsPlantesPrigest

[ppn_clients]

```SQL
select *, 1 Nivel
from ppn_clients c
where (((Select count(CLIE_NIF) from clients c2 where c2.CLIE_NIF = c.clie_nif) <> 1) or ifnull(ADRE_ADRECA,'') <> ifnull(CLIE_ADRECA,''))
order by CLIE_NOMCOMERCIAL
```

#### quClientsAmbPlantes

[ppn_clients]

```SQL
select MAX(CLIE_NOMCOMERCIAL) CLIE_NOMCOMERCIAL, 
MAX(CLIE_NOMFISCAL) CLIE_NOMFISCAL, 
MAX(CLIE_CLIENT) CLIE_CLIENT, 
MAX(CLIE_NIF) CLIE_NIF, 
MIN(CLIE_DATAALTA) CLIE_DATAALTA, 
cast(nullif(MIN(ifnull(CLIE_DATABAIXA, cast(10000101 as date))), cast(10000101 as date)) as date) CLIE_DATABAIXA, 
MAX(ADRE_ADRECA) ADRE_ADRECA, 
MAX(CLIE_ADRECA) CLIE_ADRECA, 
MAX(EMAI_EMAIL) EMAI_EMAIL, 
MAX(TELE_TELEFON) TELE_TELEFON,
MAX(Tipus) Tipus, 
MAX(Tipus2) Tipus2, 
MAX(NIFCount) NIFCount,
MAX(TipusCaixa) TipusCaixa, 
MAX(Merma) Merma, 
MAX(EnviarEmail) EnviarEmail, 
MAX(Gel) Gel,
MAX(CLIE_POBLACIO) CLIE_POBLACIO,
MAX(c.CLIE_CODIPOSTAL) CLIE_CODIPOSTAL,
MAX(c.TipusFact) as TipusFact,
MAX(c.Potencial) as Potencial,
MAX(c.TipusABC) as TipusABC,
MAX(c.Objectiu) as Objectiu,
MAX(c.DataInici) as DataInici,
MAX(c.DataFi) as DataFi, 0 Nivel
from ppn_clients c
where (((Select count(CLIE_NIF) from clients c2 where c2.CLIE_NIF = c.clie_nif) <> 1) or ifnull(ADRE_ADRECA,'') <> ifnull(CLIE_ADRECA,''))
group by clie_nif
order by clie_nif
```

#### quClientsPlantesPrigest2

[ppn_clients]

```SQL
SELECT 
    c.CLIE_NOMCOMERCIAL AS CLIE_NOMCOMERCIAL,
    c.CLIE_NOMFISCAL AS CLIE_NOMFISCAL,
    c.CLIE_CLIENT AS CLIE_CLIENT,
    c.CLIE_NIF AS CLIE_NIF,
    c.CLIE_DATAALTA AS CLIE_DATAALTA,
    c.CLIE_DATABAIXA AS CLIE_DATABAIXA,
    adrecesenviament.ADRE_ADRECA AS ADRE_ADRECA,
    c.CLIE_ADRECA AS CLIE_ADRECA,
    emails.EMAI_EMAIL AS EMAI_EMAIL,
    telefons.TELE_TELEFON AS TELE_TELEFON,
    (CASE
        WHEN (c.CLIE_NACIONALITAT = 11) THEN 0
        ELSE 1
    END) AS Tipus,
    (CASE c.CLIE_VIP
        WHEN 'V' THEN 2
        WHEN 'D' THEN 1
        ELSE 0
    END) AS Tipus2,
    (SELECT 
            COUNT(c2.CLIE_NIF)
        FROM
            clients c2
        WHERE
            (c2.CLIE_NIF = c.CLIE_NIF)) AS NIFCount,
    c.CLIE_NUMERO1 TipusCaixa,
    c.CLIE_NUMERO2 Merma,
    c.CLIE_NUMERO3 EnviarEmail,
    c.CLIE_NUMERO4 Gel,
    c.CLIE_POBLACIO,
    CLIE_CODIPOSTAL,
    c.CLIE_EDIEANCOMPRADOR AS TipusFact,
    IFNULL(NULLIF(c.CLIE_EDIEANCLIENT, ''), 0) AS Potencial,
    c.CLIE_EDIEANRECEPTOR AS TipusABC,
    IFNULL(NULLIF(c.CLIE_EDIEANPAGADOR, ''), 0) AS Objectiu,
    c.CLIE_DATA3 AS DataInici,
    c.CLIE_DATA4 AS DataFi,
    1 Nivel
FROM
    (((clients c
    LEFT JOIN emails ON (((emails.EMAI_TIPUS = 'C')
        AND (emails.EMAI_CODI = c.CLIE_CLIENT)
        AND (emails.EMAI_PRINCIPAL = 'S'))))
    LEFT JOIN telefons ON (((telefons.TELE_CODI = c.CLIE_CLIENT)
        AND (telefons.TELE_TIPUS = 'C')
        AND (telefons.TELE_PRINCIPAL = 'S'))))
    LEFT JOIN adrecesenviament ON (((adrecesenviament.ADRE_TIPUS = 'C')
        AND (adrecesenviament.ADRE_CODI = c.CLIE_CLIENT)
        AND (adrecesenviament.ADRE_PRINCIPAL = 'N'))))
WHERE
    ((NOT ((c.CLIE_NOMFISCAL LIKE 'CLIENTS PROVISIONALS%')))
        AND (NOT ((c.CLIE_NOMFISCAL LIKE 'CLIENTS COMPTAT%')))
        AND (NOT ((c.CLIE_NOMFISCAL LIKE '%CLIENTS COMPTAT%')))
        AND (c.CLIE_NOMFISCAL <> 'PC PUIGNAU A CASA')
        AND (c.CLIE_NOMFISCAL <> 'PCBCN PUIGNAU A CASA')
        AND (c.CLIE_NOMFISCAL <> 'PC-PUIGNAU A CASA')
        AND (c.CLIE_NOMFISCAL <> 'PCBCN ISABEL RODRIGUEZ')
        AND (c.CLIE_NOMFISCAL <> 'CLIENT DE PROVA DEL MERCAT')
        AND (c.CLIE_NOMFISCAL <> 'CLIENT DE PROVA DEL SISTEMA')
        AND (c.CLIE_NIF <> 'A1')
        AND (c.CLIE_NIF <> 'A')
        AND (c.CLIE_NIF <> 'A.')
        AND (c.CLIE_NIF <> 'a')
        AND (c.CLIE_NIF <> '')
        AND (c.CLIE_NIF <> 'A9')
        AND (NOT ((c.CLIE_CLIENT LIKE '17%'))))
        AND ADRE_ADRECA IS NOT NULL;
```

#### quClientsEconomicos

```SQL
select CLIE_CLIENT, CLIE_TIPUSFACTURACIO, CLIE_DTEC, CLIE_DTEPP, 
case CLIE_TIPUSIVA when 'S' then 1 else 2 end TipusIVA, 
CLIE_DIAPAGAMENT1, CLIE_DIAPAGAMENT2, CLIE_RISC, CLIE_PORTS, 
CLIE_PERIODICITAT,CLIE_PRIMERVENCIMENT,CLIE_FORMAPAGAMENT, cd.CODI_DIARIFACTURA
from clients c
left join tipusfacturacio tf on c.CLIE_TIPUSFACTURACIO = tf.TIFA_TIPUS
left join codisdiaris cd on cd.CODI_CODIDIARI = tf.TIFA_CODIDIARI
order by clie_client
```

#### quDiarisSeries

El union s'utilitza per a que surtin primer les del select de dalt i despres les del de abaix, ja que les inferiors son dependents de les superiors.

```SQL
select CODI_DESCRIPCIO, cd.CODI_DIARIRECTIFICATIVA, d2.DIAR_DESCRIPCIO,null diariRect, cd.CODI_SERIERECTIFICATIVA from codisdiaris cd
left join priconta_1.diaris d on d.DIAR_DIARI = cd.CODI_DIARIFACTURA
left join priconta_1.diaris d2 on d2.DIAR_DIARI = cd.CODI_DIARIRECTIFICATIVA
where CODI_DIARIRECTIFICATIVA in (31,42,35,37,39,44)
union all
select CODI_DESCRIPCIO, cd.CODI_DIARIFACTURA,d.DIAR_DESCRIPCIO,cd.CODI_DIARIRECTIFICATIVA, cd.CODI_SERIEFACTURA from codisdiaris cd
left join priconta_1.diaris d on d.DIAR_DIARI = cd.CODI_DIARIFACTURA
left join priconta_1.diaris d2 on d2.DIAR_DIARI = cd.CODI_DIARIRECTIFICATIVA
where CODI_DIARIFACTURA in (30,32,34,36,38,43)
```

[FormaPagamentAhora]: AFormesPagament#function-formapagamentahoraformapagamentprigest-integer-primervencimentprigest-integer-tformespagamentahora
[ppn_clients]: MySQL#ppn_clients
[Pers_PClientes_Datos_Economicos_U]: StoredProcedures#pers_pclientes_datos_economicos_u
[Pers_PClientes_Datos_I]: StoredProcedures.md#pers_pclientes_datos_i
