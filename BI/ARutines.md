# [..](..)\BI\ARutines

## Index

- [Index](#index)
- [GetAppVersion](#getappversion)
- [RAW](#raw)

## GetAppVersion

Ens retorna un string amb la versio i data de compilacio de l'exe.

```Delphi
function GetAppVersion: string;
begin
  Result:= GetBuildInfoAsString + ' - ' +  GetBuildDateFormated;
end;
```

## RAW

```Delphi
unit ARutines;
{ Funcions  i procediments d'�s general.}

interface

Uses Windows, Messages, SysUtils, Classes, Graphics, Controls, Forms, Dialogs,
     Math, DB, Printers, ShellApi, WinInet, WinSock, CRegistre;

const
  // Codis de tecles per event OnKeyDown
  TD_ENTER  = 13;
  TD_INSERT = 45;
  TD_SUPR   = 46;

  NOM_DIES: array[1..7] of string = ('Diumenge', 'Dilluns', 'Dimarts', 'Dimecres',
                                     'Dijous', 'Divendres', 'Dissabte');

  NOM_MESOS : array[1..12] of string = ( 'Gener', 'Febrer', 'Mar�',
                                         'Abril', 'Maig', 'Juny', 'Juliol',
                                         'Agost', 'Setembre', 'Octubre',
                                         'Novembre', 'Desembre');
  NOM_MESOS_ABREUJAT : array[1..12] of string = ( 'Gen', 'Feb', 'Mar',
                                         'Abr', 'Mai', 'Jun', 'Jul',
                                         'Ago', 'Set', 'Oct',
                                         'Nov', 'Des');

function OmplirZerosDavant(num:integer; const s: string) : string;
// Omple amb 0's davant la cadena s fins deixar-la de longitud num.

function OmplirZerosDarrera(num:integer; const s: string) : string;
// Omple amb 0's darrera la cadena s fins deixar-la de longitud num.

function OmplirEspaisDavant(s: string; num: integer): string;
// Omple amb num espais davant la cadena s

function OmplirEspaisDarrera(s: string; num: integer): string;
// Omple amb num espais per darrera la cadena s

function StrTipus(DataType : TFieldType) : string;
// Retorna una string que indica el tipus de dades.

function StrIndexOptions(IndexOptions : TIndexOptions) : string;
// Retorna una string que indica les opcions de l'�ndex.

function DataFitxer(FileName: string): string;
// Retorna una cadena que cont� la data i hora de creaci� del fitxer.

function EsborrarDirectori(Directori : string) : Integer;
// Elimina un directori amb TOTS els seus fitxers. Es retorna codi d'error.

Procedure LPrint(x: integer; var y: integer; Linia: string);
// Imprimeix accedint a Printer la cadena L�nia a la Pos X, Y. Incrementa Y en
// en els pixels necessaris corresponents a 1 l�nia segons l'impressora.

function PathSenseUnitat( Path: string): string;
// Donat un Path ens retorna el mateix path sense la unitat.

function CharDavant( C: Char; S: string; n: integer): string;
// Retorna una string amb n-length(s) blancs davant de S.

function CharDarrera( C: Char; S: string; n: integer): string;
// Retorna una string amb n-length(s) blancs davant de S.

function IsNumber(const Valor: String): Boolean;
//Retorna Cert en cas que la cadena d'entrada representi un valor num�ric

function XDiv(op1:Real;Op2:Real):Real;
// Divisi� amb arrodoniment a 2 decimals.

function XMul(op1:Real;Op2:Real):Real;
// Producte amb arrodoniment a 2 decimals.

function ArrodonirMoneda(op: Real): Real;
// Arrodonim un valor real amb 2 decimals, arrodonit al tercer.

function ExecutarFitxer( const Operacio, NomFitxer, Params, DirDefecte : string;
	                       ShowCmd : Integer ) : THandle;
// Executa l'aplicaci� associada i obre el fitxer indicat.
// NomFitxer : Nom del fitxer sense el path.
// DirDefecte: Path del fitxer
// Params: Par�metres a passar a l'aplicaci�.
// ShowCmd: Mode de visualitzaci� de la finestra : SW_SHOW per veure-la normal.

function CopiarFitxer(const FitxerOrigen, FitxerDesti: string;
                      const SobreEscriure: boolean): boolean;
// Copia el fitxer or�gen al dest�. Ho sobreescriu si SobreEscriure �s Cert.

procedure CrearDocument(Document: string);
// Crea un fitxer nou amb el nom indicat a Document.

procedure CrearDocumentWord(Document, Plantilla: string);
// Crea un nou document de Word segons la plantilla indicada.

function ObtenirDirectoriUsuari: String;

function ObtenirUsuari: String;

function ObtenirIP: String;

function HiHaConnexioInternet: boolean;
// Retorna cert si hi ha connexi� a internet.

procedure DataSetToCsv(pDataSet : TDataSet; PathFitxer: String);
// Exportar un DataSet a CSV.

function TreurePuntsComa(const s: string): string;
// Treure els punts i coma

function TreureCometes(const s: string): string;
// Treure les cometes dobles i simples.

function GetMinutes(Hora: TDateTime): real;
// Retorna el datetime en minuts

function GetSeconds(Hora: TDateTime): Integer;
// Retorna el datetime en segons

procedure TreureCarFinalLiniesFitxer(Fitxer: string);
// Treu l'�ltim car�cter de cada l�nia del fitxer de text passat.

function LlegirUsuariReg: String;
procedure EscriureUsuariReg(Usuari: String);
function LlegirPersonaReg: Integer;
procedure EscriurePersonaReg(Persona: Integer);

procedure FindFilePattern(root:String; pattern:String; Recursive: boolean; ls: TStringList);

procedure ObrirDocumentAjuda(FitxerExecutable, NomDocument: string);

function DatesColisionen(DataInici1, DataFi1, DataInici2, DataFi2: TDateTime): boolean;
// Retorna cert si els dos rangs de dates col.lisionen entre elles.

function GetBuildInfoAsString: string;
// Retorna la versi� de Build

function GetFileModifiedDate(Fitxer: string): string;
// Retorna la data de modificaci� d'un fitxer.

function GetBuildDate: TDateTime;
// Retorna la data de modificaci� de l'Aplicaci�

function GetAppVersion: string;
// Retorna la versi� completa (release + Data) de l'applicaci�.

function CamelCase(s:string): string;
// Aplica CamelCase a tot l'string (primera lletra de cada paraula en mayus)

function PrimeraMajuscula(AText: string): string;
//Primera lletra de cada linia en Mayus

implementation


function OmplirZerosDavant(num:integer; const s: string) : string;
{ Omple amb 0's davant l'string s. La mida de l'string queda en num car�cters.}
//var i : Longint;
begin
{
  i:=Trunc(IntPower(10,num))+StrToInt(s);
  OmplirZerosDavant:=copy(IntToStr(i),2,num);
}
  if Length(s) < num then
    Result:= StringOfChar('0', Num - Length(s)) + s
  else Result:= s;
end;

function OmplirZerosDarrera(num:integer; const s: string) : string;
{ Omple amb 0's darrera l'string s. La mida de l'string queda en num car�cters.}
// var i : Longint;
begin
{
  if length(s) <> num then
  begin
    i:=Trunc(IntPower(10,num-length(s)))*StrToInt(s);
    OmplirZerosDarrera:=IntToStr(i);
  end
  else OmplirZerosDarrera:=s;
}
  if Length(s) < num then
    Result:= s + StringOfChar('0', Num - Length(s))
  else Result:= s;

end;

function OmplirEspaisDarrera(s: string; num: integer): string;
begin
  Result:= s + StringOfChar(' ', num - Length(s));
end;

function OmplirEspaisDavant(s: string; num: integer): string;
begin
  Result:= StringOfChar(' ', num - Length(s)) + s;
end;


function StrTipus(DataType : TFieldType) : string;
{ Retorna una string que indica el tipus de dades. }
begin
  Case DataType of
    ftUnknown	    : Result:='Desconegut';
    ftString	    : Result:='String';
    ftSmallint	  : Result:='SmallInt';
    ftInteger	    : Result:='Integer';
    ftWord	      : Result:='Word';
    ftBoolean	    : Result:='Boolean';
    ftFloat	      : Result:='Float';
    ftCurrency	  : Result:='Currency';
    ftBCD         : Result:='BCD';
    ftDate	      : Result:='Date';
    ftTime	      : Result:='Time';
    ftDateTime	  : Result:='DateTime';
    ftBytes	      : Result:='Fixed Bytes';
    ftVarBytes	  : Result:='Variable Bytes';
    ftAutoInc	    : Result:='Autoinc';
    ftBlob	      : Result:='Blob';
    ftMemo	      : Result:='Memo';
    ftGraphic	    : Result:='Graphic';
    ftFmtMemo	    : Result:='FmtMemo';
    ftParadoxOle	: Result:='Paradox OLE';
    ftDBaseOle	  : Result:='dBASE OLE';
    ftTypedBinary : Result:='Binary';
  end;
end;


function StrIndexOptions(IndexOptions : TIndexOptions) : string;
// Retorna una string que indica les opcions de l'�ndex.
Var s : string;
begin
  s:='';
  if ixPrimary in IndexOptions then
  begin
     if s <> '' then s:=s+',';
     s:=s+'Primary';
  end;
  if ixUnique in IndexOptions then
  begin
     if s <> '' then s:=s+',';
     s:=s+' Unique';
  end;
  if ixDescending in IndexOptions then
  begin
     if s <> '' then s:=s+',';
     s:=s+' Descending';
  end;
  if ixExpression in IndexOptions then
  begin
     if s <> '' then s:=s+',';
     s:=s+' Expression';
  end;
  if ixCaseInsensitive in IndexOptions then
  begin
     if s <> '' then s:=s+',';
     s:=s+' CaseInsensitive';
  end;
  Result:=s;
end;

function DataFitxer(FileName: string): string;
// Retorna una cadena que cont� la data i hora de creaci� del fitxer.
var
  FHandle: integer;
begin
  FHandle := FileOpen(FileName, 0);
  try
    Result := DateTimeToStr(FileDateToDateTime(FileGetDate(FHandle)));
  finally
    FileClose(FHandle);
  end;
end;

function EsborrarDirectori(Directori : string) : Integer;
// Elimina un directori amb TOTS els seus fitxers. Es retorna codi d'error.
begin
  if MessageDlg('Eliminar directori : '+Directori+' ?',mtWarning,[mbYes, mbNo],0) = mrYes then
  begin
    DeleteFile(Directori+'\*.*');
    RmDir(Directori);
    EsborrarDirectori:=1;
  end;
end;

Procedure LPrint(x: integer; var y: integer; Linia: string);
// Imprimeix accedint a Printer la cadena L�nia a la Pos X, Y. Incrementa Y en
// en els pixels necessaris corresponents a 1 l�nia segons l'impressora.
Var
  PixelsLinia : Integer;
begin
  PixelsLinia:=Trunc(Printer.PageHeight / 66);
  Printer.Canvas.TextOut(x, y*PixelsLinia, Linia);
  Inc(y);
end;


procedure CrearDocument(Document: string);
var F: TextFile;
begin
  AssignFile(F, Document);
  Rewrite(F);
  CloseFile(F);
  ShowMessage( 'Nou document creat '+#13+ Document);
end;

procedure CrearDocumentWord(Document, Plantilla: string);
begin
  if FileExists(Plantilla) then
  begin
    CopyFile(PChar(Plantilla), PChar(Document), False);
    ShowMessage( 'Nou document word creat '+#13+ Document);
  end else ShowMessage( 'Error. Plantilla no trobada'+#13+#13+Plantilla);
end;


function PathSenseUnitat( Path: string): string;
// Donat un Path ens retorna el mateix path sense la unitat.
var ch: Char;
begin
  if Copy(Path, 2,1) = ':' then Result:=Copy(Path, 3, Length(Path)-2)
                           else Result:=Path;
end;

function CharDavant( C: Char; S: string; n: integer): string;
// Retorna una string de longitud n posant n-length(s) blancs davant de S.
var i, l: integer;
begin
  S:=Trim(S);
  l:=Length(S);
  if l < n then
  begin
    For i:=1 to n-l do S:=C + S;
  end else S:=Copy(S, 1, n);
  Result:=S;
end;

function CharDarrera( C: Char; S: string; n: integer): string;
// Retorna una string de longitud n posant n-length(s) blancs darrera de S.
var i, l: integer;
begin
  S:=Trim(S);
  l:=Length(S);
  if l < n then
  begin
    For i:=1 to n-l do S:=S+C;
  end else S:=Copy(S,1, n);
  Result:=S;
end;

function XDiv(op1:Real;Op2:Real):Real;
// Divisi� amb arrodoniment a 2 decimals.
begin
  if Op2 <> 0 then Result:=ArrodonirMoneda(op1 / op2)
              else Result:=0;
end;


function XMul(op1:Real;Op2:Real):Real;
// Producte amb arrodoniment a 2 decimals.
begin
  Result:= ArrodonirMoneda(op1 * op2)
end;


function ArrodonirMoneda(op: Real): Real;
// Arrodonim un valor real amb 2 decimals, arrodonit al tercer.
begin
  Result:= Round(Op*100)/100;
end;


function ExecutarFitxer( const Operacio, NomFitxer, Params, DirDefecte : string;
	                       ShowCmd : Integer ) : THandle;
// Executa l'aplicaci� associada i obre el fitxer indicat.
// NomFitxer : Nom del fitxer sense el path.
// DirDefecte: Path del fitxer
// Params: Par�metres a passar a l'aplicaci�.
// ShowCmd: Mode de visualitzaci� de la finestra : SW_SHOW per veure-la normal.
var
	zOperacio : array[ 0..79 ] of Char;
	zNomFitxer : array[ 0..79 ] of Char;
	zParams : array[ 0..79 ] of Char;
	zDir : array[ 0..79 ] of Char;
begin
	Result := ShellExecute( Application.Handle,
		StrPCopy( zOperacio, Operacio ),
		StrPCopy( zNomFitxer, NomFitxer ),
		StrPCopy( zParams, Params ),
		StrPCopy( zDir, DirDefecte ), ShowCmd );
	if Result <= 32 then
		MessageDlg( 'ERROR - Can''t ' + Operacio + ' file  ' +
			NomFitxer, mtError, [ mbOK ], 0 );
end; 

function ObtenirDirectoriUsuari: String;
var
  Dir: String;
begin
  Result:= GetEnvironmentVariable('USERPROFILE');
end;

function ObtenirUsuari: string;
begin
  Result:= GetEnvironmentVariable('USERNAME');
end;


function HiHaConnexioInternet: boolean;
var origin : cardinal;
begin
  Result:= InternetGetConnectedState(@origin,0);

  //connections origins by origin value
  //NO INTERNET CONNECTION              = 0;
  //INTERNET_CONNECTION_MODEM           = 1;
  //INTERNET_CONNECTION_LAN             = 2;
  //INTERNET_CONNECTION_PROXY           = 4;
  //INTERNET_CONNECTION_MODEM_BUSY      = 8;
end;


procedure DataSetToCsv(pDataSet : TDataSet; PathFitxer: String);
var
  i : Integer;
  voFichero : TStringList;
  vsLinea : String;
  vsBookMark : TBytes;
begin
  vsBookMark := pDataSet.Bookmark;
  voFichero := nil;
  try
    voFichero := TStringList.Create;
    // Ponemos la cabecera
    vsLinea := '';
    for i := 0 to pDataSet.FieldCount - 1 do
    begin
      // if pDataSet.Fields[i].Visible then
      vsLinea := vsLinea + '"' + pDataSet.Fields[i].DisplayName + '"';
      if i < (pDataSet.FieldCount-1) then vsLinea:= vsLinea + ';';
    end;

    voFichero.Add(vsLinea);
    // Insertamos los registros
    pDataSet.DisableControls;
    pDataSet.First;
    while not pDataSet.Eof do
    begin
      vsLinea := '';
      for i := 0 to pDataSet.FieldCount - 1 do
      begin
        // if pDataSet.Fields[i].Visible then
        vsLinea := vsLinea + '"' + pDataSet.Fields[i].AsString + '"';
        if i < (pDataSet.FieldCount-1) then vsLinea:= vsLinea + ';';
      end;
      voFichero.Add(vsLinea);
      pDataSet.Next;
    end;
    // Lo guardamos en disco
    voFichero.SaveToFile(PathFitxer);
  finally
    FreeAndNil(voFichero);
    pDataSet.Bookmark := vsBookMark;
    pDataSet.EnableControls;
  end;
end;


function TreureCometes(const s: string): string;
begin
  Result:= StringReplace(s, '"', '', [rfReplaceAll]);
end;

function TreurePuntsComa(const s: string): string;
begin
  Result:= StringReplace(s, ';', '', [rfReplaceAll]);
end;


function GetIP(var HostName, IPaddr, WSAErr: string): Boolean;
type
  Name = array[0..100] of AnsiChar;
  PName = ^Name;
var
  HEnt: pHostEnt;
  HName: PName;
  WSAData: TWSAData;
  i: Integer;
begin
  Result := False;
  if WSAStartup($0101, WSAData) <> 0 then begin
    WSAErr := 'Winsock is not responding."';
    Exit;
  end;
  IPaddr := '';
  New(HName);
  if GetHostName(HName^, SizeOf(Name)) = 0 then
  begin
    HostName := StrPas(HName^);
    HEnt := GetHostByName(HName^);
    for i := 0 to HEnt^.h_length - 1 do
     IPaddr :=
      Concat(IPaddr,
      IntToStr(Ord(HEnt^.h_addr_list^[i])) + '.');
    SetLength(IPaddr, Length(IPaddr) - 1);
    Result := True;
  end
  else begin
   case WSAGetLastError of
    WSANOTINITIALISED:WSAErr:='WSANotInitialised';
    WSAENETDOWN      :WSAErr:='WSAENetDown';
    WSAEINPROGRESS   :WSAErr:='WSAEInProgress';
   end;
  end;
  Dispose(HName);
  WSACleanup;
end;

function ObtenirIP: String;
var
  Host, IP, Err: String;
begin
  GetIP(Host, IP, Err);
  Result:= IP;
end;

function IsNumber(const Valor: String): Boolean;
//Retorna Cert en cas que la cadena d'entrada representi un valor num�ric
var
  Dummy: Extended;
begin
  result:= TextToFloat(PChar(Valor), Dummy, fvExtended);
end;

function ShutDownWindows(Flag: word): Boolean;
var
	TokenPriv: TTokenPrivileges;
	H: DWord;
	HToken: THandle;
begin
	if Win32Platform = VER_PLATFORM_WIN32_NT then
	begin
		OpenProcessToken(GetCurrentProcess,
		TOKEN_ADJUST_PRIVILEGES,HToken);
		LookUpPrivilegeValue(NIL, 'SeShutdownPrivilege',
		TokenPriv.Privileges[0].Luid);
		TokenPriv.PrivilegeCount := 1;
		TokenPriv.Privileges[0].Attributes := SE_PRIVILEGE_ENABLED;
		H := 0;
		AdjustTokenPrivileges(HToken, FALSE,
		TokenPriv, 0, PTokenPrivileges(NIL)^, H);
		CloseHandle(HToken);
  end;
	Result := ExitWindowsEx(Flag, 0);
end;

function CopiarFitxer(const FitxerOrigen, FitxerDesti: string;
                      const SobreEscriure: boolean): boolean;
// Copia el fitxer or�gen al dest�. Ho sobreescriu si SobreEscriure �s Cert.
begin
//  Win95Copy(Application.Handle, FitxerOrigen, FitxerDesti);
  CopyFile(PChar(FitxerOrigen), PChar(FitxerDesti), not Sobreescriure);
  Result:=True;
end;

function GetMinutes(Hora: TDateTime): real;
var
  Hour, Min, Sec, MSec: Word;
begin
  DecodeTime(Hora, Hour, Min, Sec, MSec);
  Result := Hour * 60 + Min + Sec / 60;
end;

function GetSeconds(Hora: TDateTime): Integer;
var
  Hour, Min, Sec, MSec: Word;
begin
  DecodeTime(Hora, Hour, Min, Sec, MSec);
  if Trunc(Hora) > 0 then Hour:= Hour + Trunc(Hora) * 24;  
  Result:= Hour * 3600 + Min * 60 + Sec;
end;


procedure TreureCarFinalLiniesFitxer(Fitxer: string);
var i: integer;
    ls: TStringList;
begin
  ls:= TStringList.Create;
  Try
    ls.LoadFromFile(Fitxer);
    for i:= 0 to ls.Count -1 do
    begin
      ls.Strings[i]:= Copy(ls.Strings[i], 1, Length(ls.Strings[i])-1);
    end;
    ls.SaveToFile(Fitxer);
  Finally
    ls.Free;
  End;
end;

function LlegirUsuariReg: String;
var R: TRegistre;
begin
  R:= TRegistre.Create(nil);
  R.ClauArrel:= 'PUIGNAU';
  R.Clau:= 'PUIGNAU';
  R.Open;
  if R.FieldByName('USUARI') = nil then
    Result:= ''
  else
    Result:= R.FieldByName('USUARI').AsString;

  R.Close;
  R.Free;
end;

procedure EscriureUsuariReg(Usuari: String);
var
  R: TRegistre;
begin
  R:= TRegistre.Create(nil);
  R.ClauArrel:= 'PUIGNAU';
  R.Clau:= 'PUIGNAU';
  R.Open;
  if R.FieldByName('USUARI') = nil then
    R.InsertField('USUARI').AsString:= Usuari
  else
    R.FieldByName('USUARI').AsString:= Usuari;
  R.Close;
  R.Free;
end;

function LlegirPersonaReg: Integer;
var R: TRegistre;
begin
  R:= TRegistre.Create(nil);
  R.ClauArrel:= 'PUIGNAU';
  R.Clau:= 'PUIGNAU';
  R.Open;
  if R.FieldByName('PERSONA') = nil then
    Result:= -1
  else
    Result:= R.FieldByName('PERSONA').AsInteger;

  R.Close;
  R.Free;
end;

procedure EscriurePersonaReg(Persona: integer);
var
  R: TRegistre;
begin
  R:= TRegistre.Create(nil);
  R.ClauArrel:= 'PUIGNAU';
  R.Clau:= 'PUIGNAU';
  R.Open;
  if R.FieldByName('PERSONA') = nil then
    R.InsertField('PERSONA').AsInteger:= Persona
  else
    R.FieldByName('PERSONA').AsInteger:= Persona;
  R.Close;
  R.Free;
end;


procedure FindFilePattern(root:String; pattern:String; Recursive: boolean; ls: TStringList);
// Retorna a ls una llista de tots els fitxers que compleixen el patr�.
// Si s'indica recursiu, segueix cercant pels directoris. Assumeix que ls �s
// un TStringlist ja creat.
var SR:TSearchRec;
begin
  root:=IncludeTrailingPathDelimiter(root);
  if FindFirst(root+'*.*',faAnyFile,SR) = 0 then
  begin
    Repeat
      Application.ProcessMessages;
      if Recursive and ((SR.Attr and faDirectory) = SR.Attr ) and (pos('.',SR.Name)=0) then
         FindFilePattern(root+SR.Name, pattern, Recursive, ls)
      else
      begin
       if pos(pattern,SR.Name)>0 then ls.Add(Root+SR.Name);
      end;
    Until FindNext(SR)<>0;
  end;
end;

procedure ObrirDocumentAjuda(FitxerExecutable, NomDocument: string);
var Carpeta, Fitxer: string;
begin
  Carpeta:= ExtractFilePath(FitxerExecutable) + 'Documents\';
  Fitxer:= Carpeta + NomDocument + '.pdf';
  if FileExists(Fitxer) then
    ExecutarFitxer('', Fitxer, '', '', SW_NORMAL)
  else ShowMessage('No hi ha document d''ajuda' + #13 + Fitxer);
end;

function DatesColisionen(DataInici1, DataFi1, DataInici2, DataFi2: TDateTime): boolean;
// Retorna cert si els dos rangs de dates col.lisionen entre elles.
begin
  Result:= not ((DataFi1 < DataInici2) or (DataInici1 > DataFi2));
end;


procedure GetBuildInfo(var V1, V2, V3, V4: word);
var
  VerInfoSize, VerValueSize, Dummy: DWORD;
  VerInfo: Pointer;
  VerValue: PVSFixedFileInfo;
begin
  VerInfoSize := GetFileVersionInfoSize(PChar(ParamStr(0)), Dummy);
  if VerInfoSize > 0 then
  begin
      GetMem(VerInfo, VerInfoSize);
      try
        if GetFileVersionInfo(PChar(ParamStr(0)), 0, VerInfoSize, VerInfo) then
        begin
          VerQueryValue(VerInfo, '\', Pointer(VerValue), VerValueSize);
          with VerValue^ do
          begin
            V1 := dwFileVersionMS shr 16;
            V2 := dwFileVersionMS and $FFFF;
            V3 := dwFileVersionLS shr 16;
            V4 := dwFileVersionLS and $FFFF;
          end;
        end;
      finally
        FreeMem(VerInfo, VerInfoSize);
      end;
  end;
end;

function GetBuildInfoAsString: string;
var V1, V2, V3, V4: word;
begin
  GetBuildInfo(V1, V2, V3, V4);
  Result := IntToStr(V1) + '.' + IntToStr(V2) + '.' +
    IntToStr(V3) + '.' + IntToStr(V4);
end;

function GetFileModifiedDate(Fitxer: string): string;
var Data: TDateTime;
begin
  FileAge(Fitxer, Data);
  Result:= FormatDateTime('dd/mm/yyyy', Data);
end;

function GetBuildDateFormated: string;
begin
  Result := FormatDateTime('dd/mm/yyyy, hh:nn', GetBuildDate);
end;

function GetAppVersion: string;
begin
  Result:= GetBuildInfoAsString + ' - ' +  GetBuildDateFormated;
end;

function GetBuildDate: TDateTime;
begin
  Result := PImageNtHeaders(HInstance + cardinal(PImageDosHeader(HInstance)
    ^._lfanew))^.FileHeader.TimeDateStamp / SecsPerDay + UnixDateDelta;
end;


function CamelCase(s:string): string;
// Aplica CamelCase a tot l'string (primera lletra de cada paraula en mayus)
var t1: integer;
   first: boolean;
begin
   result := LowerCase(s);
   first := true;
   for t1 := 1 to length(result) do
     if result[t1] = ' ' then begin
       first := true;
     end else
       if first then begin
         result[t1] := UpCase(result[t1]);
         first := false;
       end;
end;

function PrimeraMajuscula(AText: string): string;
//Primera lletra de cada linia en Mayus
var
  i: Integer;
  len: Integer;
  s: string;
  p: PChar;
begin
  Result := AnsiLowerCase(AText);
  len := Length(Result);
  if len > 0 then
  begin
    p := PChar(Result);
    p^ := UpCase(p^);
    for i := 2 to len do
    begin
      if p[i - 1] = ' ' then
      begin
        p := p + i - 1;
        p^ := UpCase(p^);
      end;
    end;
  end;
end;

end.
```
