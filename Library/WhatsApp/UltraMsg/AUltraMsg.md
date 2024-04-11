# [..](..)\Library\WhatsApp\UltraMsg\AUltraMsg

## Index

- [Index](#index)
- [TUltraMsg](#tultramsg)
  - [constructor Creator](#constructor-creator)
  - [class procedure EnviarDocument(telf, caption, document, filename: string)](#class-procedure-enviardocumenttelf-caption-document-filename-string)
  - [class procedure EnviarMissatge(telf, body: string)](#class-procedure-enviarmissatgetelf-body-string)
  - [class procedure Terminate(Sender: TObject)](#class-procedure-terminatesender-tobject)
  - [procedure CreateLogger](#procedure-createlogger)
  - [class procedure CrearThread(proce: TProc)](#class-procedure-crearthreadproce-tproc)
  - [procedure GetAcces](#procedure-getacces)
    - [function ObtenirValor(KeyName, DefaultValue: string): string](#function-obtenirvalorkeyname-defaultvalue-string-string)
  - [class function GetInstance: TUltraMsg](#class-function-getinstance-tultramsg)
  - [procedure Init](#procedure-init)
  - [function Enviar(endpoint: string; AParameters: TDictionary\<string, TPair\<string, TRESTRequestParameterOptions\>\>): boolean](#function-enviarendpoint-string-aparameters-tdictionarystring-tpairstring-trestrequestparameteroptions-boolean)
  - [function EnviarPreguntaGrups(): string](#function-enviarpreguntagrups-string)
  - [class function ObtenirGrups(telf: string): TdxMemData](#class-function-obtenirgrupstelf-string-tdxmemdata)
    - [procedure ObrirTaules](#procedure-obrirtaules)
    - [function CreateField(AMemData: TDataSet; AFieldName: string; AFieldType: TFieldType): TFieldDef](#function-createfieldamemdata-tdataset-afieldname-string-afieldtype-tfieldtype-tfielddef)
    - [function GetIds(participants: TObjectList\<WhatsAppGroupsResponse.TParticipants\>): boolean](#function-getidsparticipants-tobjectlistwhatsappgroupsresponsetparticipants-boolean)
- [Unit Methods](#unit-methods)
  - [function StrMaxLen(const S: string; MaxLen: integer): string](#function-strmaxlenconst-s-string-maxlen-integer-string)
  - [function DictionaryToString(dict: TDictionary\<string, TPair\<string, TRESTRequestParameterOptions\>\>): string](#function-dictionarytostringdict-tdictionarystring-tpairstring-trestrequestparameteroptions-string)
  - [function Convertfiletobase64(const AInFileName: string): string](#function-convertfiletobase64const-ainfilename-string-string)
- [finalization](#finalization)

## TUltraMsg

### constructor Creator

[Init](#procedure-init)

```Delphi
constructor TUltraMsg.Creator;
begin
  NeedsInit := true;
  Tasques := 0;
  Init;
end;
```

### class procedure EnviarDocument(telf, caption, document, filename: string)

Crea un diccionary amb tots els parametres necessaris per UltraMsg.
Crea una tasca per a enviar un document via WhatsApp.

[CrearThread](#class-procedure-crearthreadproce-tproc)
[Enviar](#function-enviarendpoint-string-aparameters-tdictionarystring-tpairstring-trestrequestparameteroptions-boolean)

```Delphi
class procedure TUltraMsg.EnviarDocument(telf, caption, document, filename: string);
var
  dict: TDictionary<string, TPair<string, TRESTRequestParameterOptions>>;
begin
  dict := TDictionary<string, TPair<string, TRESTRequestParameterOptions
    >>.Create;

  dict.Add('token', TPair<string, TRESTRequestParameterOptions>.Create
    (TUltraMsg.Instance.Token, []));
  dict.Add('to', TPair<string, TRESTRequestParameterOptions>.Create
    (telf, []));
  dict.Add('filename', TPair<string, TRESTRequestParameterOptions>.Create
    (filename, []));
  dict.Add('document', TPair<string, TRESTRequestParameterOptions>.Create
    (Convertfiletobase64(document), []));
  dict.Add('caption', TPair<string, TRESTRequestParameterOptions>.Create
    (caption, [poDoNotEncode]));
  TUltraMsg.Instance.FileLogger.Log('Crear tasca document :=' + telf, etDebug);
  CrearThread(
    procedure
    begin
      TUltraMsg.Instance.Enviar('/messages/document', dict);
    end);
  TUltraMsg.Instance.FileLogger.Log('Crear tasca document :=' + telf, etSuccess);
end;
```

### class procedure EnviarMissatge(telf, body: string)

Crea un diccionary amb tots els parametres necessaris per UltraMsg.
Crea una tasca per a enviar un missatge de text via WhatsApp.

[CrearThread](#class-procedure-crearthreadproce-tproc)
[Enviar](#function-enviarendpoint-string-aparameters-tdictionarystring-tpairstring-trestrequestparameteroptions-boolean)

```Delphi
class procedure TUltraMsg.EnviarMissatge(telf, body: string);
var
  dict: TDictionary<string, TPair<string, TRESTRequestParameterOptions>>;
begin
  CrearThread(
    procedure
    begin
      dict := TDictionary<string, TPair<string, TRESTRequestParameterOptions
        >>.Create;

      dict.Add('token', TPair<string, TRESTRequestParameterOptions>.Create
        (TUltraMsg.Instance.Token, []));
      dict.Add('to', TPair<string, TRESTRequestParameterOptions>.Create
        (telf, []));
      dict.Add('body', TPair<string, TRESTRequestParameterOptions>.Create
        (body, []));

      TUltraMsg.Instance.Enviar('/messages/document', dict);
    end);
end;
```

### class procedure Terminate(Sender: TObject)

Event OnTerminate dels Threads elimina la tasca de la cua.
[CrearThread](#class-procedure-crearthreadproce-tproc)

```Delphi
class procedure TUltraMsg.Terminate(Sender: TObject);
begin
  TUltraMsg.Tasques := TUltraMsg.Tasques - 1;
end;
```

### procedure CreateLogger

Crea un Logger de arxiu, el qual ens permet guardar tots els moviments que fa la API de UltraMsg.

[TMyLogger][ALoggerFiles]

```Delphi
procedure TUltraMsg.CreateLogger;
begin
  if not Assigned(FileLogger) then
  begin
    FileLogger := TMyLogger.Create(ExtractFileDir(Application.ExeName) +
      '\Dades\UltraMsgLogs\' + ALog.DateFolder + '\AUltraMsg.log', '[UltraMsg]');
  end;
  FileLogger.MyInit;
end;
```

### class procedure CrearThread(proce: TProc)

Crea una tasca de enviament.

```Delphi
class procedure TUltraMsg.CrearThread(proce: TProc);
var
  thread: TThread;
begin
  thread := TThread.CreateAnonymousThread(proce);
  TUltraMsg.Tasques := TUltraMsg.Tasques + 1;

  thread.OnTerminate := TUltraMsg.Terminate;
  thread.Start;
end;
```

### procedure GetAcces

Obte els parametres per a poder utilitzar la API de UltraMsg.

```Delphi
procedure TUltraMsg.GetAcces;
var
  query: TADOQuery;
  aCon: TADOCOnnection;
begin
  aCon := TADOCOnnection.Create(nil);
  try
    aCon.LoginPrompt := false;
    aCon.ConnectFromUDL(ExtractFilePath(Application.ExeName) + 'Puignau.udl');
    query := TADOQuery.Create(nil);
    try
      with query do
      begin
        Connection := aCon;
        SQL.Text :=
          'Select Clau, Valor From ConfiguracionsDetall Where IdConfiguracio = 4';
        Open;
      end;

      InstanceID := ObtenirValor('INSTANCE_ID', 'instance32179');
      URL := ObtenirValor('URL', 'https://api.ultramsg.com');
      Token := ObtenirValor('TOKEN', 'zio80ks2elp4uefu');
    finally
      with query do
      begin
        if Active then
          Close;
        Free;
      end;
    end;
  finally
    with aCon do
    begin
      if Connected then
        Close;
      Free;
    end;
  end;
end;
```

#### function ObtenirValor(KeyName, DefaultValue: string): string

Retorna el valor de la clau solicitada, en cas contrari te un valor per defecte.

```Delphi
function ObtenirValor(KeyName, DefaultValue: string): string;
  begin
    if query.Locate('Clau', KeyName, []) then
      Result := query.FieldByName('Valor').AsString
    else
      Result := DefaultValue;
  end;
```

### class function GetInstance: TUltraMsg

Ens retorna la instancia SingleTon.

```Delphi
class function TUltraMsg.GetInstance: TUltraMsg;
begin
  fLock := TCriticalSection.Create;
  fLock.Enter;
  try
    if not Assigned(fInstance) then
      fInstance := TUltraMsg.Creator;
    Result := fInstance;
  finally
    fLock.Leave;
    fLock.Free;
  end;
end;
```

### procedure Init

[GetAcces](#procedure-getacces)
[CreateLogger](#procedure-createlogger)

```Delphi
procedure TUltraMsg.Init;
begin
  if not NeedsInit then
    Exit;
  GetAcces;
  CreateLogger;
  NeedsInit := false;
end;
```

### function Enviar(endpoint: string; AParameters: TDictionary<string, TPair<string, TRESTRequestParameterOptions>>): boolean

[Init](#procedure-init)

Aquest metode utilitza la llibreria REST, en concret [REST.Client, REST.Types].

Per a enviar un missatge, primer fem un client REST i li donem la adreça on ha de enviar la request, la adreça esta formada per la URL, amb la instancia de UltraMsg i despres l'endpoint, que sera diferent segons el que necessitem:

```Delphi
LClient := TRESTClient.Create(URL + '/' + InstanceID + endpoint);
```

Despres creem una request, que contindra el que volem enviar i llavors la response que contindra la resposta de UltraMsg.
Com que passem els parametres de la request amb un diccionari, podem recorre'l per obtenir els parametres que hem de enviar a UltraMsg.

Ara ja tenim la request correcte, al fer execute ho enviem a UltraMsg i al cap d'uns segons ens retorna resposta on mirarem si conte el missatge sent, i si el missatge **sent** = 'true', significa que el missatge s'ha enviat correctament.
Amb aixo afegim al Log Enviat o No enviat amb els parametres per a poder fer proves a ma.

Tot aixo s'executa desde un Thread, o sigui funciona de forma asyncrona. Per aixo mantenim un recompte de quantes tasques tenim actives, perque si algu fes un enviament massiu, podria ser que el proces hagues creat les tasques pero que el programa encara els estigues creant per darrera, per aixo al block finalization de la unit tenim una espera segon les tasques que tenim.

```Delphi
function TUltraMsg.Enviar(endpoint: string; AParameters: TDictionary<string,
  TPair<string, TRESTRequestParameterOptions>>): boolean;
var
  sentResult: string;
  LClient: TRESTClient;
  LRequest: TRESTRequest;
  LResponse: TRESTResponse;
  KeyName: string;
begin
  if NeedsInit then
    Init;

  LClient := TRESTClient.Create(URL + '/' + InstanceID + endpoint);
  try
    LRequest := TRESTRequest.Create(LClient);
    try
      LResponse := TRESTResponse.Create(LClient);
      try

        try

          LRequest.Client := LClient;
          LRequest.Response := LResponse;

          LRequest.Method := rmPOST;

          for KeyName in AParameters.Keys do
          begin
            LRequest.AddParameter(KeyName, AParameters[KeyName].Key,
              pkGETorPOST, AParameters[KeyName].Value);
          end;

          LRequest.Execute;

          Result := LResponse.GetSimpleValue('sent', sentResult) and
            (sentResult = 'true');
          try
            if Result then
              FileLogger.Log('Enviat ' + DictionaryToString(AParameters),
                etSuccess)
            else
              FileLogger.Log('No enviat ' + LResponse.JSONText + #13#10 +
                DictionaryToString(AParameters), etError);
          finally

          end;
        except
          on E: Exception do
          begin
            FileLogger.Log(E.Message, etError);
            raise E;
          end;
        end;
      finally
        LResponse.Free;
      end;
    finally
      LRequest.Free;
    end;
  finally
    LClient.Free;
  end;
  sleep(100);
end;
```

### function EnviarPreguntaGrups(): string

[Init](#procedure-init)

Generem un Client contra l'enpoint `groups` de tipus **GET**, d'aquesta manera ens retornara un JSON el com el seguent:

```JSON
[
  {
    "id": "120363233649835896@g.us",
    "name": "4269 - La Taverne, Grupo",
    "isReadOnly": false,
    "isGroup": true,
    "groupMetadata": {
      "description": "",
      "edit_group_info": "all",
      "send_messages": "all",
      "owner": "34689233637@c.us",
      "creation": 1707756302,
      "participants": [
        {
          "id": "34659420872@c.us",
          "isAdmin": false,
          "isSuperAdmin": false
        },
        {
          "id": "33782212139@c.us",
          "isAdmin": false,
          "isSuperAdmin": false
        },
        {
          "id": "34689233637@c.us",
          "isAdmin": true,
          "isSuperAdmin": false
        },
        {
          "id": "34679964306@c.us",
          "isAdmin": false,
          "isSuperAdmin": false
        }
      ]
    },
    "last_time": 1712580355,
    "archived": false,
    "pinned": false,
    "isMuted": false,
    "unread": 6
  },
  {
    "id": "120363275178903027@g.us",
    "name": "Mariscco - Puignau",
    "isReadOnly": false,
    "isGroup": true,
    "groupMetadata": {
      "description": "",
      "edit_group_info": "all",
      "send_messages": "all",
      "owner": "34630762022@c.us",
      "creation": 1711623912,
      "participants": [
        {
          "id": "34659420872@c.us",
          "isAdmin": false,
          "isSuperAdmin": false
        },
        {
          "id": "34630762022@c.us",
          "isAdmin": true,
          "isSuperAdmin": false
        },
        {
          "id": "34637813646@c.us",
          "isAdmin": true,
          "isSuperAdmin": false
        },
        {
          "id": "34679964306@c.us",
          "isAdmin": false,
          "isSuperAdmin": false
        }
      ]
    },
    "last_time": 1712579970,
    "archived": false,
    "pinned": false,
    "isMuted": false,
    "unread": 1
  },
  {
    "id": "120363280607303979@g.us",
    "name": "Hotel Palace - Puignau",
    "isReadOnly": false,
    "isGroup": true,
    "groupMetadata": {
      "description": "",
      "edit_group_info": "all",
      "send_messages": "all",
      "owner": "34630762022@c.us",
      "creation": 1712578870,
      "participants": [
        {
          "id": "34659420872@c.us",
          "isAdmin": false,
          "isSuperAdmin": false
        },
        {
          "id": "34630762022@c.us",
          "isAdmin": true,
          "isSuperAdmin": true
        },
        {
          "id": "34679964306@c.us",
          "isAdmin": false,
          "isSuperAdmin": false
        },
        {
          "id": "34619147452@c.us",
          "isAdmin": false,
          "isSuperAdmin": false
        },
        {
          "id": "34679248368@c.us",
          "isAdmin": false,
          "isSuperAdmin": false
        }
      ]
    },
    "last_time": 1712578907,
    "archived": false,
    "pinned": false,
    "isMuted": false,
    "unread": 2
  }
]
```

[ALoggerFiles]

```Delphi
function TUltraMsg.EnviarPreguntaGrups(): string;
var
  sentResult: string;
  LClient: TRESTClient;
  LRequest: TRESTRequest;
  LResponse: TRESTResponse;
  KeyName: string;
begin
  if NeedsInit then
    Init;

  FileLogger.Log('Consultar Grups WA', etDebug);
  LClient := TRESTClient.Create(URL + '/' + InstanceID + '/groups');
  try
    LRequest := TRESTRequest.Create(LClient);
    try
      LResponse := TRESTResponse.Create(LClient);
      try
        LRequest.Client := LClient;
        LRequest.Response := LResponse;

        LRequest.Method := rmGET;

        LRequest.AddParameter('token', Token, pkGETorPOST);

        LRequest.Execute;
        Result := LResponse.JSONText;
      finally
        LResponse.Free;
      end;
    finally
      LRequest.Free;
    end;
  finally
    LClient.Free;
  end;
  FileLogger.Log('Consultar Grups WA', etDebug);
  sleep(100);
end;
```

### class function ObtenirGrups(telf: string): TdxMemData

Aquest metode genera una taula en memoria i la carrega amb els valors del JSON retornat per [TUltraMsg.Instance.EnviarPreguntaGrups](#function-enviarpreguntagrups-string).

Utilitza un filtre que el trobem a [GetIds](#function-getidsparticipants-tobjectlistwhatsappgroupsresponsetparticipants-boolean).

[ObrirTaules]

```Delphi
class function TUltraMsg.ObtenirGrups(telf: string): TdxMemData;
var
  taGrups: TdxMemData;
  taGrupsid: TStringField;
  taGrupstelfParticipant: TStringField;
  taGrupsNomGrup: TStringField;
  taGrupsCNomGrup: TStringField;
  taGrupsNomContacte: TStringField;

var
  groups: TRoot;
  iGroup, iParticipant: integer;
begin
  ObrirTaules;

  groups := TRoot.Create;
  groups.AsJson := TUltraMsg.Instance.EnviarPreguntaGrups;

  iGroup := 0;
  while groups.Items.Count > iGroup do
  begin
    iParticipant := 0;

    while groups.Items[iGroup].GroupMetadata.participants.Count > iParticipant do
    begin
      if ((telf <> '') and GetIds(groups.Items[iGroup]
        .GroupMetadata.participants)) or (telf = '') then
      begin
        taGrups.Append;

        taGrupsid.Value := groups.Items[iGroup].Id;
        taGrupsNomGrup.Value := groups.Items[iGroup].Name;

        taGrupstelfParticipant.Value :=
          Copy(groups.Items[iGroup].GroupMetadata.participants[iParticipant].Id,
          1, Pos('@', groups.Items[iGroup].GroupMetadata.participants
          [iParticipant].Id) - 1);
        taGrupsCNomGrup.Value := taGrupsNomGrup.Value + ' - ' + taGrupsid.Value;

        taGrups.Post;
      end;
      iParticipant := iParticipant + 1;
    end;
    iGroup := iGroup + 1;
  end;
  Result := taGrups;
end;
```

#### procedure ObrirTaules

[CreateField](#function-createfieldamemdata-tdataset-afieldname-string-afieldtype-tfieldtype-tfielddef)

Crea una taula en memoria per a despres poder emplenar-la amb els grups de WhatsApp.

```Delphi
procedure ObrirTaules;
begin
  taGrups := TdxMemData.Create(nil);
  CreateField(taGrups, 'Id', ftString);
  CreateField(taGrups, 'telfParticipant', ftString);
  CreateField(taGrups, 'NomGrup', ftString);
  CreateField(taGrups, 'CNomGrup', ftString);
  CreateField(taGrups, 'NomContacte', ftString);
  taGrups.Name := 'taGrups';
  taGrupsid := TStringField(taGrups.FieldByName('Id'));
  taGrupstelfParticipant :=
    TStringField(taGrups.FieldByName('telfParticipant'));
  taGrupsNomGrup := TStringField(taGrups.FieldByName('NomGrup'));
  taGrupsCNomGrup := TStringField(taGrups.FieldByName('CNomGrup'));
  taGrupsNomContacte := TStringField(taGrups.FieldByName('NomContacte'));
  taGrupsid.Size := 30;
  taGrupsCNomGrup.Size := 100;
  taGrupsNomGrup.Size := 50;
  taGrups.Close;
  taGrups.Open;
end;
```

#### function CreateField(AMemData: TDataSet; AFieldName: string; AFieldType: TFieldType): TFieldDef

Crea un camp en el Dataset.

```Delphi
function CreateField(AMemData: TDataSet; AFieldName: string; AFieldType:
    TFieldType): TFieldDef;
begin
  if (AMemData = nil) or (AFieldName = '') then
    Exit;
  Result := AMemData.FieldDefs.AddFieldDef;
  with Result do
  begin
    Name := AFieldName;
    DataType := AFieldType;
    CreateField(AMemData);
  end;
end;
```

#### function GetIds(participants: TObjectList<WhatsAppGroupsResponse.TParticipants>): boolean

Aquesta funcio treballa a mode de filtre per a poder establir si s'ha de carregar o no la linia.

El filtre es el seguent: es comproba que el participant sigui administrador i que sigui tambe el telefon del comercial.

```Delphi
function GetIds(participants: TObjectList<WhatsAppGroupsResponse.TParticipants>): boolean;
var
  i: integer;
begin
  i := 0;
  Result := false;
  while i < participants.Count do
  begin
    Result := Result or ((participants[i].Id = '34' + telf + '@c.us') and
      participants[i].IsAdmin);
    i := i + 1;
  end;
end;
```

## Unit Methods

### function StrMaxLen(const S: string; MaxLen: integer): string

Retorna la string entrant pero a la mida establerta amb "..." al final.

```Delphi
function StrMaxLen(const S: string; MaxLen: integer): string;
var
  i: integer;
begin
  Result := S;
  if Length(Result) <= MaxLen then
    Exit;
  SetLength(Result, MaxLen);
  for i := MaxLen downto MaxLen - 2 do
    Result[i] := '.';
end;
```

### function DictionaryToString(dict: TDictionary<string, TPair<string, TRESTRequestParameterOptions>>): string

Retorna una string amb els valors del diccionari escrits per poder escriure en el log.

```Delphi
function DictionaryToString(dict: TDictionary<string, TPair<string,
  TRESTRequestParameterOptions>>): string;
var
  Key: string;
begin
  Result := '';
  for Key in dict.Keys do
  begin
    Result := Result + #13#10 + Key + ' : ' + StrMaxLen(dict[Key].Key, 100);
  end;
end;
```

### function Convertfiletobase64(const AInFileName: string): string

Retorna el fitxer passat per parametre en base64.

```Delphi
function Convertfiletobase64(const AInFileName: string): string;
var
  inStream: TStream;
  outStream: TStringstream;
  base64Encoder: TBase64Encoding;
begin
  inStream := TFileStream.Create(AInFileName, fmOpenRead);
  try
    outStream := TStringstream.Create;
    try
      base64Encoder := TBase64Encoding.Create(0);
      try
        base64Encoder.Encode(inStream, outStream);
        Result := outStream.DataString;
      finally
        base64Encoder.Free;
      end;
    finally
      outStream.Free;
    end;
  finally
    inStream.Free;
  end;
end;
```

## finalization

Al tancar el programa finalitzen les units. Verifiquem que no hi hagin WhatsApps pendents, ja que sino no s'enviarien i tambe petaria el programa.

```Delphi
finalization
  while TUltraMsg.Tasques > 0 do
    Application.ProcessMessages;

  TUltraMsg.Instance.FileLogger.Destruct;
```

[ALoggerFiles]: ../../Logger/ALoggerFiles.md
