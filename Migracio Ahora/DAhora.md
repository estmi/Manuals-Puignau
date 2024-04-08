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
  - [procedure quClientsEconomicosCalcFields(DataSet: TDataSet)](#procedure-quclientseconomicoscalcfieldsdataset-tdataset)
- [Metodes](#metodes)
  - [procedure InsertarClient(Tipus: Integer; NomClient: string; NomFiscal: string; IdClient: Integer; Direccio: string; Telf: string; Poblacio: string; DataAlta, DataBaixa: TDateTime; IdTipus2: Integer; Nif: string; Nivel: Integer; TipusCaixaPrep: Integer; Merma: Boolean; Gel: Boolean; EnviarEtiqMail: Boolean; CodPostal: string; TipusFacturacio, TipusABC: string; Potencial, Objectiu: double; IniciVac, FiVac: TDateTime)](#procedure-insertarclienttipus-integer-nomclient-string-nomfiscal-string-idclient-integer-direccio-string-telf-string-poblacio-string-dataalta-databaixa-tdatetime-idtipus2-integer-nif-string-nivel-integer-tipuscaixaprep-integer-merma-boolean-gel-boolean-enviaretiqmail-boolean-codpostal-string-tipusfacturacio-tipusabc-string-potencial-objectiu-double-inicivac-fivac-tdatetime)
- [SQL](#sql)
  - [quAddClient](#quaddclient)

### Descripcio

### Interface

#### published

```Delphi
procedure quClientsEconomicosCalcFields(DataSet: TDataSet);
```

#### private

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

#### public

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

### Events

#### procedure quClientsEconomicosCalcFields(DataSet: TDataSet)

Calcula la forma de pagament d'Ahora.

[FormaPagamentAhora][funcFormaPagamentAhora]

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

### Metodes

#### procedure InsertarClient(Tipus: Integer; NomClient: string; NomFiscal: string; IdClient: Integer; Direccio: string; Telf: string; Poblacio: string; DataAlta, DataBaixa: TDateTime; IdTipus2: Integer; Nif: string; Nivel: Integer; TipusCaixaPrep: Integer; Merma: Boolean; Gel: Boolean; EnviarEtiqMail: Boolean; CodPostal: string; TipusFacturacio, TipusABC: string; Potencial, Objectiu: double; IniciVac, FiVac: TDateTime)

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

### SQL

#### quAddClient

[Pers_PClientes_Datos_I](StoredProcedures.md#pers_pclientes_datos_i)

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

[funcFormaPagamentAhora]: AFormesPagament.md#test
