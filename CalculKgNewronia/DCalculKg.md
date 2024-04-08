# [..](..)\CalculKgNewronia\DCalculKg

## Index

- [Index](#index)
- [Descripcio](#descripcio)
- [Interface](#interface)
  - [published](#published)
  - [private](#private)
  - [public](#public)
- [Events](#events)
  - [procedure DataModuleCreate(Sender: TObject)](#procedure-datamodulecreatesender-tobject)
  - [procedure quLineesComandesCalcFields(DataSet: TDataSet)](#procedure-qulineescomandescalcfieldsdataset-tdataset)
- [Metodes](#metodes)
  - [procedure LlegirComandesKgs(Data: TDateTime)](#procedure-llegircomandeskgsdata-tdatetime)
  - [procedure EliminarComandesKgs(Data: TDateTime)](#procedure-eliminarcomandeskgsdata-tdatetime)
  - [procedure EmplenarComandesKgs(Data: TDateTime; ProgressBar: TProgressBar)](#procedure-emplenarcomandeskgsdata-tdatetime-progressbar-tprogressbar)
  - [procedure LlegirLineesComandes(Data: TDateTime)](#procedure-llegirlineescomandesdata-tdatetime)
  - [procedure ConnectFromUDL(ADOConnection: TADOConnection; const FitxerUDL: string)](#procedure-connectfromudladoconnection-tadoconnection-const-fitxerudl-string)
  - [function EsFresc(familia: string): boolean \[DEPRECATED\]](#function-esfrescfamilia-string-boolean-deprecated)
  - [function CalcularPes(Fardos, Peces, Quantitat, KgXFardo, KgXPeca, PecesXFardo: Double; TipusUnitat: string): Double](#function-calcularpesfardos-peces-quantitat-kgxfardo-kgxpeca-pecesxfardo-double-tipusunitat-string-double)
  - [procedure LlegirAlbarans(Data: TDateTime) \[DEPRECATED\]](#procedure-llegiralbaransdata-tdatetime-deprecated)
  - [procedure CalcularKgs(Data: TDateTime; ProgressBar: TProgressBar)](#procedure-calcularkgsdata-tdatetime-progressbar-tprogressbar)
- [SQL](#sql)
  - [BD Prigest](#bd-prigest)
    - [quLineesComandes](#qulineescomandes)
    - [quAlbarans \[DEPRECATED\]](#qualbarans-deprecated)
  - [BD Newronia](#bd-newronia)
    - [quComandesKgs](#qucomandeskgs)
    - [quEliminarComandesKgs](#queliminarcomandeskgs)

## Descripcio

## Interface

### published

```Delphi
procedure DataModuleCreate(Sender: TObject);
procedure quLineesComandesCalcFields(DataSet: TDataSet);
```

### private

```Delphi
procedure LlegirComandesKgs(Data: TDateTime);
procedure EliminarComandesKgs(Data: TDateTime);
procedure EmplenarComandesKgs(Data: TDateTime; ProgressBar: TProgressBar);
procedure LlegirLineesComandes(Data: TDateTime);
procedure ConnectFromUDL(ADOConnection: TADOConnection; const FitxerUDL: string);
function EsFresc(familia: string): boolean;
function CalcularPes(Fardos, Peces, Quantitat, KgXFardo, KgXPeca, PecesXFardo: Double; TipusUnitat: string): Double;
procedure LlegirAlbarans(Data: TDateTime);
```

### public

```Delphi
procedure CalcularKgs(Data: TDateTime; ProgressBar: TProgressBar);
```

## Events

### procedure DataModuleCreate(Sender: TObject)

[ConnectFromUDL](#procedure-connectfromudladoconnection-tadoconnection-const-fitxerudl-string)

```Delphi
procedure TdmCalculKg.DataModuleCreate(Sender: TObject);
var
  FitxerUDL: string;
begin
  try
    // Connexi� de la BD de Prigest
    FitxerUDL := UpperCase(Application.ExeName);
    FitxerUDL := ExtractFilePath(Application.ExeName) + 'PRIGEST.UDL';
    ConnectFromUDL(acPrigest, FitxerUDL);
  except
    DataBaseError('ERROR. No s''ha trobat el fitxer de configuraci� ' +
      FitxerUDL + #13 + #13 + 'Demaneu suport inform�tic.');
    Halt;
  end;

  try
    // Connexi� de la BD de Prigest
    FitxerUDL := UpperCase(Application.ExeName);
    FitxerUDL := ExtractFilePath(Application.ExeName) + 'NEWRONIA.UDL';
    ConnectFromUDL(acNewronia, FitxerUDL);
  except
    DataBaseError('ERROR. No s''ha trobat el fitxer de configuraci� ' +
      FitxerUDL + #13 + #13 + 'Demaneu suport inform�tic.');
    Halt;
  end;
end;
```

### procedure quLineesComandesCalcFields(DataSet: TDataSet)

`quLineesComandesCBundle.Value := quLineesComandesBundle.Value = 'F';` ens diu si aquesta linia forma part d'un bundle, ja que nomes tenim els fills peix i els articles normals.

`quLineesComandestealbara` ens indica si la comanda d'aquesta linia te un albara assignat.

Per temes de logistica, l'article **00104** que es **GEL** son sacs de 25Kg pero no tenen un factor ben definit. Per nomes aquest cas es va fer una excepcio.

Un cop sabem aixo, el `quLineesComandesPesTotal` es calcula segons si te albara o no, i s'utilitza el metode [CalcularPes] per a saber quin pes representa la linia. Si te albara, es calcula segons la quantitat de l'albara, en cas contrari, es calcula segons la comanda.

```Delphi
procedure TdmCalculKg.quLineesComandesCalcFields(DataSet: TDataSet);
var
  quantitat: Double;
begin
  quLineesComandesCBundle.Value := quLineesComandesBundle.Value = 'F';

  if quLineesComandestealbara.Value > 0 then
  begin
    if quLineesComandesQuantitatAlbara.Value = 0 then
      quLineesComandesPesTotal.Value := 0
    else
    begin
      if quLineesComandesArticle.Value = '00104' then
        quLineesComandesPesTotal.Value := quLineesComandesQuantitatAlbara.Value * 25
      else
        quLineesComandesPesTotal.Value := CalcularPes(quLineesComandesFardos.Value,
          quLineesComandesPeces.Value, quLineesComandesQuantitatAlbara.Value,
          quLineesComandesKgXFardo.Value, quLineesComandesKgXPeca.Value,
          quLineesComandesQuantitatXFardo.Value, quLineesComandesMesura.Value);
    end;
  end
  else
  begin
    if quLineesComandesArticle.Value = '00104' then
      quLineesComandesPesTotal.Value := quLineesComandesQuantitat.Value * 25
    else
      quLineesComandesPesTotal.Value := CalcularPes(quLineesComandesFardos.Value,
        quLineesComandesPeces.Value, quLineesComandesQuantitat.Value,
        quLineesComandesKgXFardo.Value, quLineesComandesKgXPeca.Value,
        quLineesComandesQuantitatXFardo.Value, quLineesComandesMesura.Value);
  end;
end;
```

## Metodes

### procedure LlegirComandesKgs(Data: TDateTime)

Llegeix les comandes a Newronia.

[quComandesKgs](#qucomandeskgs)

```Delphi
procedure TdmCalculKg.LlegirComandesKgs(Data: TDateTime);
begin
  with quComandesKgs, Parameters do
  begin
    Close;
    ParamByName('Data').Value := Trunc(Data);
    Open;
  end;
end;
```

### procedure EliminarComandesKgs(Data: TDateTime)

S'utilitza el DecodeDate degut a que es passa la data en format integer.

[quEliminarComandesKgs](#queliminarcomandeskgs)

```Delphi
procedure TdmCalculKg.EliminarComandesKgs(Data: TDateTime);
var
  day, month, year: Word;
begin
  DecodeDate(Data, year, month, day);
  with quEliminarComandesKgs, Parameters do
  begin
    ParamByName('DataLliurament').Value := year * 10000 + month * 100 + day;
    ExecSQL;
  end;
end;
```

### procedure EmplenarComandesKgs(Data: TDateTime; ProgressBar: TProgressBar)

Aqui es creen les comandes a Newronia.

```Delphi
//Calcula els Kgs i els insereix a la taula ComandesKgs
procedure TdmCalculKg.EmplenarComandesKgs(Data: TDateTime; ProgressBar: TProgressBar);
var
  comanda: Integer;
  totalKgs: real;
begin
  // Emplenar taula
  comanda := -1;
  totalKgs := 0;

  with ProgressBar do
  begin
    Min := 0;
    if quLineesComandes.RecordCount <= 0 then
      Max := 0
    else
      Max := quLineesComandes.RecordCount - 1;

    Position := 0;
    Step := 1;
    Application.ProcessMessages;
  end;

  quLineesComandes.First;
  while not quLineesComandes.Eof do
  begin
    ProgressBar.StepIt;
    Application.ProcessMessages;

    if comanda <> quLineesComandescomanda.Value then
    begin
      if comanda <> -1 then
      begin
        // Inserir Comanda
        quComandesKgs.Append;

        quComandesKgsExercici.Value := YearOf(Data);
        quComandesKgsCanal.Value := 1;
        quComandesKgsDiari.Value := 0;
        quComandesKgsDataLliurament.Value := Data;
        quComandesKgsComanda.Value := comanda;
        quComandesKgsTotalKgs.Value := totalKgs;

        quComandesKgs.Post;
      end;
      comanda := quLineesComandescomanda.Value;
      totalKgs := quLineesComandesPesTotal.Value;
    end
    else
    begin
      // Sumar total Kgs
      totalKgs := totalKgs + quLineesComandesPesTotal.Value;
    end;
    quLineesComandes.Next;
  end;

  if comanda <> -1 then
  begin
    // Inserir Comanda
    quComandesKgs.Append;

    quComandesKgsExercici.Value := YearOf(Data);
    quComandesKgsCanal.Value := 1;
    quComandesKgsDiari.Value := 0;
    quComandesKgsDataLliurament.Value := Data;
    quComandesKgsComanda.Value := comanda;
    quComandesKgsTotalKgs.Value := totalKgs;

    quComandesKgs.Post;
  end;
  ProgressBar.Position := 0;
  Application.ProcessMessages;
end;
```

### procedure LlegirLineesComandes(Data: TDateTime)

[quLineesComandes](#qulineescomandes)

```Delphi
procedure TdmCalculKg.LlegirLineesComandes(Data: TDateTime);
begin
  with quLineesComandes, Parameters do
  begin
    Close;
    ParamByName('Any').Value := YearOf(Data);
    ParamByName('Data').Value := Trunc(Data);
    Open;
  end;
end;
```

### procedure ConnectFromUDL(ADOConnection: TADOConnection; const FitxerUDL: string)

```Delphi
procedure TdmCalculKg.ConnectFromUDL(ADOConnection: TADOConnection; const
  FitxerUDL: string);
begin
  with ADOConnection do
  begin
    Close;
    ConnectionString := Format('File Name=%s', [FitxerUDL]);
    Open;
  end;
end;
```

### function EsFresc(familia: string): boolean [DEPRECATED]

```Delphi
function tdmCalculKg.EsFresc(familia: string): boolean;
var
  Trobat: boolean;
  i: integer;
begin
  Trobat := False;
  i := 1;
  while (i <= Length(articlesFresc)) and (not Trobat) do
  begin
    if articlesFresc[i] = familia then
      Trobat := True
    else
      i := i + 1;
  end;
  Result := Trobat;
end;
```

### function CalcularPes(Fardos, Peces, Quantitat, KgXFardo, KgXPeca, PecesXFardo: Double; TipusUnitat: string): Double

Aixo ho vaig crear un dia fa temps, analitzant les comandes i albarans amb els tipus de mesures dels articles. Si es queixen revisar per sobre pero no buscar-hi sentit, ja que es molt especific.

```Delphi
function TdmCalculKg.CalcularPes(Fardos, Peces, Quantitat, KgXFardo, KgXPeca,
  PecesXFardo: Double; TipusUnitat: string): Double;
begin
  if TipusUnitat = 'KG' then
  begin
    if Quantitat = 0 then
    begin
      if Peces = 0 then
      begin
        if KgXPeca = 0 then
          Result := Fardos * PecesXFardo
        else
          Result := Fardos * PecesXFardo * KgXPeca
      end
      else
        Result := Peces * KgXPeca;
    end
    else
      Result := Quantitat;
  end
  else if TipusUnitat = 'Uts.' then
  begin
    if Quantitat = 0 then
    begin
      if Peces = 0 then
        Result := Fardos * PecesXFardo * KgXPeca
      else
        Result := Peces * KgXPeca;
    end
    else
      Result := Quantitat * KgXPeca;
  end
  else if TipusUnitat = 'Cax.' then
  begin
    Result := Quantitat * KgXPeca * PecesXFardo;
  end
  else if TipusUnitat = 'Kg.' then
  begin
    if Quantitat <> 0 then
      Result := Quantitat
    else
      Result := (Fardos * PecesXFardo + Peces) * KgXPeca;
  end
  else if TipusUnitat = 'kgs.' then
  begin
    if Quantitat = 0 then
    begin
      if Peces = 0 then
        Result := Fardos * PecesXFardo * KgXPeca
      else
        Result := Peces * KgXPeca;
    end
    else
      Result := Quantitat;
  end
  else if TipusUnitat = 'UTS' then
  begin
    if Quantitat = 0 then
    begin
      if Peces = 0 then
        Result := Fardos * PecesXFardo * KgXPeca
      else
        Result := Peces * KgXPeca;
    end
    else
      Result := Quantitat * KgXPeca;
  end
  else if TipusUnitat = 'Cx' then
  begin
    if Quantitat <> 0 then
    begin
      if Peces = 0 then
        Result := Quantitat * PecesXFardo * KgXPeca
      else
        Result := 0;
    end
    else
      Result := 0;
  end
  else if TipusUnitat = 'xKg' then
  begin
    if Quantitat = 0 then
    begin
      if Peces = 0 then
        Result := Fardos * KgXPeca
      else
        Result := Peces * KgXPeca;
    end
    else
      Result := Quantitat;
  end
  else if TipusUnitat = 'xKgs' then
  begin
    if Quantitat = 0 then
    begin
      if Peces = 0 then
        Result := Fardos * KgXPeca
      else
        Result := Peces * KgXPeca;
    end
    else
      Result := Quantitat;
  end
  else if TipusUnitat = 'xUts' then
  begin
    Result := Fardos * KgXPeca;
  end
  else if TipusUnitat = 'KG.p' then
  begin
    if Quantitat = 0 then
    begin
      if Peces = 0 then
        Result := Fardos * KgXPeca
      else
        Result := Peces * KgXPeca;
    end
    else
      Result := Quantitat;
  end
  else if TipusUnitat = 'KG.f' then
  begin
    if Quantitat = 0 then
    begin
      Result := Fardos * PecesXFardo;
    end
    else
      Result := Quantitat;
  end
  else
    Result := Quantitat;
end;
```

### procedure LlegirAlbarans(Data: TDateTime) [DEPRECATED]

[quAlbarans](#qualbarans-deprecated)

```Delphi
procedure TdmCalculKg.LlegirAlbarans(Data: TDateTime);
begin
  with quAlbarans, Parameters do
  begin
    Close;
    ParamByName('Exercici').Value := YearOf(Data);
    ParamByName('Data').Value := FormatDateTime('yyyymmdd', Data);
    Open;
  end;
end;
```

### procedure CalcularKgs(Data: TDateTime; ProgressBar: TProgressBar)

[LlegirLineesComandes](#procedure-llegirlineescomandesdata-tdatetime)

[EliminarComandesKgs](#procedure-eliminarcomandeskgsdata-tdatetime)

[LlegirComandesKgs](#procedure-llegircomandeskgsdata-tdatetime)

[EmplenarComandesKgs](#procedure-emplenarcomandeskgsdata-tdatetime-progressbar-tprogressbar)

```Delphi
procedure TdmCalculKg.CalcularKgs(Data: TDateTime; ProgressBar: TProgressBar);
begin
//  LlegirAlbarans(Data);
  LlegirLineesComandes(Data);
  EliminarComandesKgs(Data);
  LlegirComandesKgs(Data);
  EmplenarComandesKgs(Data, ProgressBar);
end;
```

## SQL

### BD Prigest

#### quLineesComandes

Retorna totes les linies de comandes del dia especificat.

```SQL
SELECT
    (case
      when CLIE_RUTA = '' then 0
      else CLIE_RUTA end) as Ruta,
  	ifnull(RUTA_DESCRIPCIO, 'SENSE RUTA') as NomRuta,
    deco_tipuslinia as bundle,
    DECO_COMANDA AS Comanda,
    DECO_LLIURAMENT AS Data,
    ARTI_PESPERCAIXA AS KgXFardo,
    ARTI_UNITATSPERCAIXA AS QuantitatXFardo,
    ARTI_QUANTITATPERPECA AS KgXPeca,
    DECO_CAIXES AS Fardos,
    DECO_PECES AS Peces,
    DECO_QUANTITAT AS Quantitat,
    MESU_DESCRIPCIO AS Mesura,
    DECO_ARTICLE AS Article,
    ARTI_DESCRIPCIO AS Descripcio,
    ARTI_FAMILIA AS CodiFamilia,
    FAMI_DESCRIPCIO AS Familia,
    COMA_CLIENT AS CodiClient,
    CLIE_NOMCOMERCIAL AS Client
      ,  ifnull(DEAL_QUANTITAT,0) as QuantitatAlbara,
    (select count(*) from detallalbarans where DEAL_COMANDA = DECO_COMANDA and DEAL_DIARICOMANDA = DECO_DIARI and DEAL_CANALCOMANDA = DECO_CANAL and DEAL_ANYCOMANDA = DECO_ANY) tealbara
FROM
    detallcomandes
        LEFT JOIN
    comandes ON DECO_COMANDA = COMA_COMANDA
        AND DECO_ANY = COMA_ANY
        AND COMA_DIARI = DECO_DIARI
        AND COMA_CANAL = DECO_CANAL
       LEFT JOIN
    articles ON ARTI_ARTICLE = DECO_ARTICLE
        LEFT JOIN
    mesures ON mesu_mesura = arti_mesura
        LEFT JOIN
    families ON ARTI_FAMILIA = FAMI_FAMILIA
        LEFT JOIN
    clients ON CLIE_CLIENT = COMA_CLIENT
    		LEFT JOIN
	rutes on clie_ruta = RUTA_RUTA
     LEFT JOIN 
    detallalbarans on DEAL_COMANDA = DECO_COMANDA and DEAL_DIARICOMANDA = DECO_DIARI and DEAL_CANALCOMANDA = DECO_CANAL and DEAL_ANYCOMANDA = DECO_ANY and DEAL_ARTICLE = DECO_ARTICLE
WHERE
    DECO_ANY = :Any AND DECO_CANAL = 1
        AND DECO_LLIURAMENT = :Data
        AND deco_diari = 0
        AND deco_tipuslinia <> 'B'
        and not (ARTI_FAMILIA = 'Z-TX'
        and ARTI_GRUP = '07000')
ORDER BY DECO_COMANDA , deco_detall;
```

#### quAlbarans [DEPRECATED]

```SQL
SELECT
    DEAL_COMANDA, DEAL_ARTICLE, sum(DEAL_QUANTITAT) DEAL_QUANTITAT
FROM
    albarans
        LEFT JOIN
    detallalbarans ON alba_albara = deal_albara
        AND ALBA_ANY = DEAL_ANY
        AND ALBA_CANAL = DEAL_CANAL
        AND ALBA_DIARI = DEAL_DIARI
WHERE
    ALBA_ANY = :Exercici AND ALBA_DATA = :Data
        AND ALBA_CANAL = 1
        AND ALBA_DIARI = 0
        AND DEAL_ARTICLE NOT BETWEEN '00991' AND '00999'
        AND DEAL_TIPUSLINIA <> 'B'
        and DEAL_ARTICLE <> 0
        and DEAL_COMANDA <> 0
group by DEAL_COMANDA, DEAL_ARTICLE
ORDER BY DEAL_COMANDA
```

### BD Newronia

#### quComandesKgs

```SQL
Select *
from Comandeskgs
where DataLliurament = :Data
```

#### quEliminarComandesKgs

Elimina les comandes de Newronia.

```SQL
delete from newronia.comandeskgs where DataLliurament = :DataLliurament
```

[CalcularPes]: #function-calcularpesfardos-peces-quantitat-kgxfardo-kgxpeca-pecesxfardo-double-tipusunitat-string-double
