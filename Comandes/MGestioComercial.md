# [..](..)\Comandes\MGestioComercial

## Index

- [Index](#index)
- [Descripcio](#descripcio)
- [Interface](#interface)
  - [published](#published)
- [Events](#events)
  - [procedure FormCreate(Sender: TObject)](#procedure-formcreatesender-tobject)
  - [procedure FormShow(Sender: TObject)](#procedure-formshowsender-tobject)

## Descripcio

Main Form de [PGestioComercial].

## Interface

### published

```Delphi
procedure FormShow(Sender: TObject);
procedure FormCreate(Sender: TObject);
```

## Events

### procedure FormCreate(Sender: TObject)

[FMain.CrearFrame]

[ALoggerFiles.MyInit]

```Delphi
procedure TMFGestioComercial.FormCreate(Sender: TObject);
begin
  FFMain1.CrearFrame;
  //Logger
  ALoggerFiles.GlobalMyLogFileProvider.MyInit;
end;
```

### procedure FormShow(Sender: TObject)

[FMain.ObrirFrame]

```Delphi
procedure TMFGestioComercial.FormShow(Sender: TObject);
begin
  Self.Caption := Self.Caption + ' ' + GetAppVersion;
  FFMain1.ObrirFrame;
end;
```

[PGestioComercial]: PGestioComercial
[FMain.CrearFrame]: FMain#procedure-crearframe
[FMain.ObrirFrame]: FMain#procedure-obrirframe
[ALoggerFiles.MyInit]: ../Library/Logger/ALoggerFiles#procedure-myinitnum-integer--0
