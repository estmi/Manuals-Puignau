# [..](..)\Library\AFuncions

## Index

- [Index](#index)
- [Descripcio](#descripcio)
- [Interface](#interface)
  - [published](#published)
- [Enums](#enums)
  - [TColumnesPermisos](#tcolumnespermisos)
  - [TTipusNode](#ttipusnode)
  - [TTipusExcepcio](#ttipusexcepcio)
  - [TTipusArticle](#ttipusarticle)
- [Events](#events)
- [Metodes](#metodes)
  - [procedure ConnectFromUDL(const FitxerUDL: string)](#procedure-connectfromudlconst-fitxerudl-string)
  - [function FindSubcontrolAtPos(AControl: TControl; AScreenPos, AClientPos: TPoint): TControl](#function-findsubcontrolatposacontrol-tcontrol-ascreenpos-aclientpos-tpoint-tcontrol)
  - [function FindControlAtPos(AScreenPos: TPoint): TControl](#function-findcontrolatposascreenpos-tpoint-tcontrol)
  - [class function TConversions.EnumerationToString(x: T): string](#class-function-tconversionsenumerationtostringx-t-string)
  - [class function TConversions.StringToEnumeration(x: string): T](#class-function-tconversionsstringtoenumerationx-string-t)
- [SQL](#sql)

## Descripcio

## Interface

### published

```Delphi
function FindSubcontrolAtPos(AControl: TControl; AScreenPos, AClientPos: TPoint):
  TControl;

function FindControlAtPos(AScreenPos: TPoint): TControl;

TConversions<T> = record
  class function StringToEnumeration(x: string): T; static;
  class function EnumerationToString(x: T): string; static;
end;

TColumnesPermisos = (AmbitFamilia, AmbitGrup, AmbitSubGrup, AmbitArticle,
  PermetPercentCost, PermetImportCost, PermetPercentVenda, PermetImportFix,
  PermetT1, PermetT2, PermetT3, PermetT4, PermetT5);

TTipusNode = (tnTipusFamilia, tnTipusGrup, tnTipusSubGrup, tnTipusArticle,
  tnTipusMarca);

TTipusExcepcio = (tePercentCost = 1, teImportCost, tePercentVenda, teImportFix);

TTipusArticle = (taFresc, taLlotja, taCongelat, taNull = -1);

TADOConnectionHelper = class helper for TADOCOnnection
public
  procedure ConnectFromUDL(const FitxerUDL: string);
end;

```

## Enums

### TColumnesPermisos

```Delphi
TColumnesPermisos = (AmbitFamilia, AmbitGrup, AmbitSubGrup, AmbitArticle,
  PermetPercentCost, PermetImportCost, PermetPercentVenda, PermetImportFix,
  PermetT1, PermetT2, PermetT3, PermetT4, PermetT5);
```

### TTipusNode

```Delphi
TTipusNode = (tnTipusFamilia, tnTipusGrup, tnTipusSubGrup, tnTipusArticle,
  tnTipusMarca);
```

### TTipusExcepcio

```Delphi
TTipusExcepcio = (tePercentCost = 1, teImportCost, tePercentVenda, teImportFix);
```

### TTipusArticle

```Delphi
TTipusArticle = (taFresc, taLlotja, taCongelat, taNull = -1);
```

## Events

## Metodes

### procedure ConnectFromUDL(const FitxerUDL: string)

```Delphi
procedure TADOConnectionHelper.ConnectFromUDL(const FitxerUDL: string);
begin
  with Self do
  begin
    Close;
    ConnectionString := Format('File Name=%s', [FitxerUDL]);
    Open;
  end;
end;
```

### function FindSubcontrolAtPos(AControl: TControl; AScreenPos, AClientPos: TPoint): TControl

```Delphi
function FindSubcontrolAtPos(AControl: TControl; AScreenPos, AClientPos: TPoint):
  TControl;
var
  i: Integer;
  C: TControl;
begin
  Result := nil;
  C := AControl;
  if (C = nil) or not C.Visible or not TRect.Create(C.Left, C.Top,
    C.Left + C.Width, C.Top + C.Height).Contains(AClientPos) then
    Exit;
  Result := AControl;
  if AControl is TWinControl then
    for i := 0 to TWinControl(AControl).ControlCount - 1 do
    begin
      C := FindSubcontrolAtPos(TWinControl(AControl).Controls[i], AScreenPos,
        AControl.ScreenToClient(AScreenPos));
      if C <> nil then
        Result := C;
    end;
end;
```

### function FindControlAtPos(AScreenPos: TPoint): TControl

Ens retorna quin control tenim a sota del ratoli.
Utilitzat a [FPressupostsClientsDetall](#).

```Delphi
function FindControlAtPos(AScreenPos: TPoint): TControl;
var
  i: Integer;
  f, m: TForm;
  p: TPoint;
  r: TRect;
begin
  Result := nil;
  for i := Screen.FormCount - 1 downto 0 do
  begin
    f := Screen.Forms[i];
    if f.Visible and (f.Parent = nil) and (f.FormStyle <> fsMDIChild) and
      TRect.Create(f.Left, f.Top, f.Left + f.Width, f.Top + f.Height)
      .Contains(AScreenPos) then
      Result := f;
  end;
  Result := FindSubcontrolAtPos(Result, AScreenPos, AScreenPos);
  if (Result is TForm) and (TForm(Result).ClientHandle <> 0) then
  begin
    Windows.GetWindowRect(TForm(Result).ClientHandle, r);
    p := TPoint.Create(AScreenPos.x - r.Left, AScreenPos.Y - r.Top);
    m := nil;
    for i := TForm(Result).MDIChildCount - 1 downto 0 do
    begin
      f := TForm(Result).MDIChildren[i];
      if TRect.Create(f.Left, f.Top, f.Left + f.Width, f.Top + f.Height)
        .Contains(p) then
        m := f;
    end;
    if m <> nil then
      Result := FindSubcontrolAtPos(m, AScreenPos, p);
  end;
end;
```
<!-- markdownlint-disable-next-line MD033-->
### class function TConversions<T>.EnumerationToString(x: T): string

Ens retorna un enum en format string, `TTipusArticle.taFresc` -> `"taFresc"`

```Delphi
class function TConversions<T>.EnumerationToString(x: T): string;
begin
  case Sizeof(T) of
    1:
      Result := GetEnumName(TypeInfo(T), PByte(@x)^);
    2:
      Result := GetEnumName(TypeInfo(T), PWord(@x)^);
    4:
      Result := GetEnumName(TypeInfo(T), PCardinal(@x)^);
  end;
end;
```
<!-- markdownlint-disable-next-line MD033-->
### class function TConversions<T>.StringToEnumeration(x: string): T

Ens retorna un enum en format string, `"taFresc"` -> `TTipusArticle.taFresc`

```Delphi
class function TConversions<T>.StringToEnumeration(x: string): T;
begin
  case Sizeof(T) of
    1:
      PByte(@Result)^ := GetEnumValue(TypeInfo(T), x);
    2:
      PWord(@Result)^ := GetEnumValue(TypeInfo(T), x);
    4:
      PCardinal(@Result)^ := GetEnumValue(TypeInfo(T), x);
  end;
end;
```

## SQL
