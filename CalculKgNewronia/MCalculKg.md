# [..](..)\CalculKgNewronia\MCalculKg

## Index

- [Index](#index)
- [Descripcio](#descripcio)
- [Interface](#interface)
  - [published](#published)
  - [private](#private)
  - [public](#public)
- [Events](#events)
  - [procedure bbLlegirClick(Sender: TObject)](#procedure-bbllegirclicksender-tobject)
  - [procedure FormShow(Sender: TObject)](#procedure-formshowsender-tobject)
  - [procedure cxButton1Click(Sender: TObject)](#procedure-cxbutton1clicksender-tobject)
- [Metodes](#metodes)
  - [procedure ExportarGraellaExcel](#procedure-exportargraellaexcel)

## Descripcio

## Interface

### published

```Delphi
procedure bbLlegirClick(Sender: TObject);
procedure FormShow(Sender: TObject);
procedure cxButton1Click(Sender: TObject);
```

### private

```Delphi
procedure ExportarGraellaExcel;
```

### public

## Events

### procedure bbLlegirClick(Sender: TObject)

[dmCalculKg.CalcularKgs]

[ExportarGraellaExcel](#procedure-exportargraellaexcel)

```Delphi
procedure TMFCalculKg.bbLlegirClick(Sender: TObject);
var
  cursorVell: TCursor;
begin
  cursorVell := Screen.Cursor;
  try
    Screen.Cursor := crHourglass;
    cxgrid1.BeginUpdate;
    cxgrid2.BeginUpdate;

    dmCalculKg.CalcularKgs(Trunc(deData.Date), ProgressBar1);

    ExportarGraellaExcel;
  finally
    Screen.Cursor := cursorVell;
    cxgrid1.EndUpdate;
    cxgrid2.EndUpdate;
    cxGrid1DBBandedTableView1.ApplyBestFit;
    cxGridDBTableView1.ApplyBestFit;
  end;
end;
```

### procedure FormShow(Sender: TObject)

[GetAppVersion](../BI/ARutines.md#getappversion)

```Delphi
procedure TMFCalculKg.FormShow(Sender: TObject);
begin
  dedata.Date := Date;
  Caption := Caption + ' - ' + GetAppVersion;
end;
```

### procedure cxButton1Click(Sender: TObject)

Aquest es el buto de Excel, demana una ubicacio i guarda uina copia de la Grid en format excel.

```Delphi
procedure TMFCalculKg.cxButton1Click(Sender: TObject);
begin
  SaveDialog1.FileName := 'CalculKgsComandes ' + FormatDateTime('dd-mm-yyyy',
    Now) + '.xls';
  if SaveDialog1.Execute then
  begin
    ExportGridToExcel(SaveDialog1.FileName, cxGrid1);
    if FileExists(SaveDialog1.FileName) then
    begin
      ExecutarFitxer('', SaveDialog1.FileName, '', '', SW_SHOW);
    end;
  end;
end;
```

## Metodes

### procedure ExportarGraellaExcel

Es crida automaticament per a guardar una copia dels calculs de kilos.

```Delphi
procedure TMFCalculKg.ExportarGraellaExcel;
var
  Carpeta, Fitxer: string;
begin
  Carpeta := ExtractFilePath(Application.ExeName) + '\Dades\CalculKgsComandes';
  ForceDirectories(Carpeta);
  Fitxer := Carpeta + '\' + FormatDateTime('yyyy-mm-dd', deData.Date) +
    '-CalculKgsComandes-' + FormatDateTime('dd-mm-yyy-hh-nn', Now) + '.xls';
  ExportGridToExcel(Fitxer, cxGrid1);
end;
```
