# [..](..)\Library\Logger\ALoggerFiles

## Index

- [Index](#index)
- [TMyLogger](#tmylogger)
  - [constructor Create](#constructor-create)
  - [constructor Create(path: string; \_tag: string)](#constructor-createpath-string-_tag-string)
  - [constructor Create(path: string)](#constructor-createpath-string)
  - [function FilterLog(aLogItem: TLogItem): Boolean](#function-filterlogalogitem-tlogitem-boolean)
  - [procedure MyInit(num: Integer = 0)](#procedure-myinitnum-integer--0)
  - [procedure Log(const cMsg: string; cEventType: TEventType)](#procedure-logconst-cmsg-string-ceventtype-teventtype)
- [initialization](#initialization)

## TMyLogger

### constructor Create

OnFilterItem: ens permet filtrar el que volem veure. El que fem es utilitzar un sistema de tags per a poder grabar nomes el que ens interessa.
[FilterLog](#function-filterlogalogitem-tlogitem-boolean)

```Delphi
constructor TMyLogger.Create;
begin
  inherited;
  DailyRotate := true;
  MaxFileSizeInMB := 20;
  LogLevel := LOG_VERBOSE;
  DailyRotateFileDateFormat := 'yyyymmdd_hh';
  CustomMsgOutput := true;
  CustomFormatOutput := '%{DATE} %{TIME} - [%{LEVEL}] [%{USERNAME}]: %{MESSAGE}';
  IncludedInfo := [iiAppName, iiHost, iiEnvironment, iiPlatform];
  OnFilterItem := FilterLog;
  ShowHeaderInfo := false;
  Logger.Providers.Add(Self);
end;
```

### constructor Create(path: string; _tag: string)

[Create(path)](#constructor-createpath-string)

```Delphi
constructor TMyLogger.Create(path: string; _tag: string);
begin
  Tag := _tag;
  Create(path);
end;
```

### constructor Create(path: string)

[Create](#constructor-create)

```Delphi
constructor TMyLogger.Create(path: string);
begin
  Create;
  LogPath := ExtractFileDir(path) + '\' + SystemInfo.UserName + '\' + TPath.GetFileNameWithoutExtension
    (path) + '.log';
end;
```

### function FilterLog(aLogItem: TLogItem): Boolean

Filtre per el log, en cas de haver definit un tag, nomes deixa fer **Logging** d'aquella tag

```Delphi
function TMyLogger.FilterLog(aLogItem: TLogItem): Boolean;
begin
  Result := (Tag = '') or (Pos(Tag, aLogItem.Msg) > 0);
end;
```

### procedure MyInit(num: Integer = 0)

Crea els directoris.

L'ultima part es complexe degut a que al activar Enabled fa bastantes coses per darrere i podria ser que quedes en false.

```Delphi
procedure TMyLogger.MyInit(num: Integer = 0);
begin
  flogNum := num;
  Enabled := false;
  if not DirectoryExists(ExtractFileDir(LogPath)) then
    ForceDirectories(ExtractFileDir(LogPath));

  if num <> 0 then
    FileName := ExtractFileDir(LogPath) + '\' + TPath.GetFileNameWithoutExtension
      (LogPath) + '.' + inttostr(num) + '.log'
  else
    FileName := ExtractFileDir(LogPath) + '\' + TPath.GetFileNameWithoutExtension
      (LogPath) + '.log';

  Enabled := true;
  if not Enabled then
  begin
    MyInit(num + 1);
  end;
end;
```

### procedure Log(const cMsg: string; cEventType: TEventType)

Fa el Logging de el missatge donat amb un format especific.

```Delphi
procedure TMyLogger.Log(const cMsg: string; cEventType: TEventType);
begin
  Quick.Logger.Log(Tag + ':' + CustomMsg + ' ' + cMsg, cEventType);
end;
```

## initialization

Inicialitza Logger global el qual guarda tot el que passa a la aplicacio.
Tot i inicialitzar-lo, s'ha de activar dintre del programa.
Per exemple GestioComercial:

MGestioComercial.pas

```Delphi
uses ALoggerFiles;
FormShow()
begin
  GlobalMyLogFileProvider.MyInit;
end;
```

```Delphi
initialization
  GlobalMyLogFileProvider := TMyLogger.Create(Alog.FitxerLog(TPath.GetFileNameWithoutExtension
    (Application.ExeName)));
  GlobalMyLogFileProvider.DailyRotate := true;
  GlobalMyLogFileProvider.DailyRotateFileDateFormat := 'yyyymmdd_hh';
  GlobalMyLogFileProvider.IncludedInfo := [iiAppName, iiHost, iiUserName];
```
