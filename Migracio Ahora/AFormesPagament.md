# [..](..)\Migracio Ahora

## AFormesPagament

### Index

- [Index](#index)
- [Descripcio](#descripcio)
- [Interface](#interface)
  - [published](#published)
  - [private](#private)
  - [public](#public)
- [Enumeracions](#enumeracions)
  - [TFormesPagamentAhora](#tformespagamentahora)
- [Metodes](#metodes)
  - [function FormaPagamentAhora(FormaPagamentPrigest: Integer; PrimerVencimentPrigest: Integer): TFormesPagamentAhora](#function-formapagamentahoraformapagamentprigest-integer-primervencimentprigest-integer-tformespagamentahora)
  - [function Between(num, min, max: Integer): Boolean](#function-betweennum-min-max-integer-boolean)

### Descripcio

### Interface

#### published

#### private

#### public

[TFormesPagamentAhora](#tformespagamentahora)

[FormaPagamentAhora](#function-formapagamentahoraformapagamentprigest-integer-primervencimentprigest-integer-tformespagamentahora)

```Delphi
TFormesPagamentAhora = (REBUT_BANCARI_07 = 1, REBUT_BANCARI_10,
    REBUT_BANCARI_15, REBUT_BANCARI_20, REBUT_BANCARI_25, REBUT_BANCARI_30,
    REBUT_BANCARI_45, REBUT_BANCARI_50, REBUT_BANCARI_60, REBUT_BANCARI_90,
    REBUT_BANCARI_120, TRANSFER_07, TRANSFER_10, TRANSFER_15, TRANSFER_20,
    TRANSFER_25, TRANSFER_30, TRANSFER_40, TRANSFER_45, TRANSFER_60, TRANSFER_75,
    TRANSFER_90, TRANSFER_120, TALO_07, TALO_10, TALO_15, TALO_30, TALO_45,
    TALO_60, TALO_70, TALO_90, TALO_120, TARGETA_07, TARGETA_10, EFECTIU_0,
    EFECTIU_07, EFECTIU_10, EFECTIU_15, EFECTIU_30, EFECTIU_60, EFECTIU_120,
    COMPTAT_RIGOROS_0);

function FormaPagamentAhora(FormaPagamentPrigest: Integer;
  PrimerVencimentPrigest: Integer): TFormesPagamentAhora;
```

### Enumeracions

#### TFormesPagamentAhora

|Nom|Valor|
|---|---|
|REBUT_BANCARI_07|1|
|REBUT_BANCARI_10|2|
|REBUT_BANCARI_15|3|
|REBUT_BANCARI_20|4|
|REBUT_BANCARI_25|5|
|REBUT_BANCARI_30|6|
|REBUT_BANCARI_45|7|
|REBUT_BANCARI_50|8|
|REBUT_BANCARI_60|9|
|REBUT_BANCARI_90|10|
|REBUT_BANCARI_120|11|
|TRANSFER_07|12|
|TRANSFER_10|13|
|TRANSFER_15|14|
|TRANSFER_20|15|
|TRANSFER_25|16|
|TRANSFER_30|17|
|TRANSFER_40|18|
|TRANSFER_45|19|
|TRANSFER_60|20|
|TRANSFER_75|21|
|TRANSFER_90|22|
|TRANSFER_120|23|
|TALO_07|24|
|TALO_10|25|
|TALO_15|26|
|TALO_30|27|
|TALO_45|28|
|TALO_60|29|
|TALO_70|30|
|TALO_90|31|
|TALO_120|32|
|TARGETA_07|33|
|TARGETA_10|34|
|EFECTIU_0|35|
|EFECTIU_07|36|
|EFECTIU_10|37|
|EFECTIU_15|38|
|EFECTIU_30|39|
|EFECTIU_60|40|
|EFECTIU_120|41|
|COMPTAT_RIGOROS_0|42|

```Delphi
TFormesPagamentAhora = (REBUT_BANCARI_07 = 1, REBUT_BANCARI_10,
    REBUT_BANCARI_15, REBUT_BANCARI_20, REBUT_BANCARI_25, REBUT_BANCARI_30,
    REBUT_BANCARI_45, REBUT_BANCARI_50, REBUT_BANCARI_60, REBUT_BANCARI_90,
    REBUT_BANCARI_120, TRANSFER_07, TRANSFER_10, TRANSFER_15, TRANSFER_20,
    TRANSFER_25, TRANSFER_30, TRANSFER_40, TRANSFER_45, TRANSFER_60, TRANSFER_75,
    TRANSFER_90, TRANSFER_120, TALO_07, TALO_10, TALO_15, TALO_30, TALO_45,
    TALO_60, TALO_70, TALO_90, TALO_120, TARGETA_07, TARGETA_10, EFECTIU_0,
    EFECTIU_07, EFECTIU_10, EFECTIU_15, EFECTIU_30, EFECTIU_60, EFECTIU_120,
    COMPTAT_RIGOROS_0);
```

### Metodes

#### function FormaPagamentAhora(FormaPagamentPrigest: Integer; PrimerVencimentPrigest: Integer): TFormesPagamentAhora

Tradueix els valors FormaPagamentPrigest i PrimerVencimentPrigest per a donar de resultat la forma de pagament de Ahora.

[Between](#function-betweennum-min-max-integer-boolean)

```Delphi
function FormaPagamentAhora(FormaPagamentPrigest: Integer;
  PrimerVencimentPrigest: Integer): TFormesPagamentAhora;
begin
  case FormaPagamentPrigest of
    1, 11:
      begin
        if Between(PrimerVencimentPrigest, 0, 7) then
          Exit(REBUT_BANCARI_07)
        else if Between(PrimerVencimentPrigest, 8, 10) then
          Exit(REBUT_BANCARI_10)
        else if Between(PrimerVencimentPrigest, 11, 18) then
          Exit(REBUT_BANCARI_15)
        else if Between(PrimerVencimentPrigest, 19, 20) then
          Exit(REBUT_BANCARI_20)
        else if Between(PrimerVencimentPrigest, 25, 25) then
          Exit(REBUT_BANCARI_25)
        else if Between(PrimerVencimentPrigest, 30, 30) then
          Exit(REBUT_BANCARI_30)
        else if Between(PrimerVencimentPrigest, 40, 45) then
          Exit(REBUT_BANCARI_45)
        else if Between(PrimerVencimentPrigest, 50, 50) then
          Exit(REBUT_BANCARI_50)
        else if Between(PrimerVencimentPrigest, 60, 60) then
          Exit(REBUT_BANCARI_60)
        else if Between(PrimerVencimentPrigest, 90, 90) then
          Exit(REBUT_BANCARI_90)
        else if Between(PrimerVencimentPrigest, 120, 120) then
          Exit(REBUT_BANCARI_120)
        else
          Exit(REBUT_BANCARI_07)
      end;
    41:
      begin
        if Between(PrimerVencimentPrigest, 0, 7) then
          Exit(TRANSFER_07)
        else if Between(PrimerVencimentPrigest, 10, 10) then
          Exit(TRANSFER_10)
        else if Between(PrimerVencimentPrigest, 15, 15) then
          Exit(TRANSFER_15)
        else if Between(PrimerVencimentPrigest, 20, 20) then
          Exit(TRANSFER_20)
        else if Between(PrimerVencimentPrigest, 25, 25) then
          Exit(TRANSFER_25)
        else if Between(PrimerVencimentPrigest, 30, 30) then
          Exit(TRANSFER_30)
        else if Between(PrimerVencimentPrigest, 40, 40) then
          Exit(TRANSFER_40)
        else if Between(PrimerVencimentPrigest, 45, 45) then
          Exit(TRANSFER_45)
        else if Between(PrimerVencimentPrigest, 60, 65) then
          Exit(TRANSFER_60)
        else if Between(PrimerVencimentPrigest, 70, 75) then
          Exit(TRANSFER_75)
        else if Between(PrimerVencimentPrigest, 85, 90) then
          Exit(TRANSFER_90)
        else if Between(PrimerVencimentPrigest, 120, 120) then
          Exit(TRANSFER_120)
        else
          Exit(TRANSFER_07)
      end;
    91, 72:
      begin
        if Between(PrimerVencimentPrigest, 0, 7) then
          Exit(TALO_07)
        else if Between(PrimerVencimentPrigest, 10, 10) then
          Exit(TALO_10)
        else if Between(PrimerVencimentPrigest, 15, 15) then
          Exit(TALO_15)
        else if Between(PrimerVencimentPrigest, 30, 30) then
          Exit(TALO_30)
        else if Between(PrimerVencimentPrigest, 40, 45) then
          Exit(TALO_45)
        else if Between(PrimerVencimentPrigest, 60, 60) then
          Exit(TALO_60)
        else if Between(PrimerVencimentPrigest, 70, 70) then
          Exit(TALO_70)
        else if Between(PrimerVencimentPrigest, 90, 90) then
          Exit(TALO_90)
        else if Between(PrimerVencimentPrigest, 120, 120) then
          Exit(TALO_120)
        else
          Exit(TALO_07)
      end;
    82:
      begin
        if Between(PrimerVencimentPrigest, 0, 7) then
          Exit(TARGETA_07)
        else if Between(PrimerVencimentPrigest, 10, 10) then
          Exit(TARGETA_10)
        else
          Exit(TARGETA_07)
      end;
    71, 73:
      begin
        if Between(PrimerVencimentPrigest, 0, 1) then
          Exit(EFECTIU_0)
        else if Between(PrimerVencimentPrigest, 7, 7) then
          Exit(EFECTIU_07)
        else if Between(PrimerVencimentPrigest, 10, 10) then
          Exit(EFECTIU_10)
        else if Between(PrimerVencimentPrigest, 15, 20) then
          Exit(EFECTIU_15)
        else if Between(PrimerVencimentPrigest, 30, 45) then
          Exit(EFECTIU_30)
        else if Between(PrimerVencimentPrigest, 60, 60) then
          Exit(EFECTIU_60)
        else if Between(PrimerVencimentPrigest, 120, 120) then
          Exit(EFECTIU_120)
        else
          Exit(EFECTIU_0)
      end;
    51:
      begin
        if Between(PrimerVencimentPrigest, 0, 15) then
          Exit(COMPTAT_RIGOROS_0)
        else
          Exit(COMPTAT_RIGOROS_0)
      end;
  else
    Exit(COMPTAT_RIGOROS_0)
  end;
end;
```

#### function Between(num, min, max: Integer): Boolean

```Delphi
function Between(num, min, max: Integer): Boolean;
begin
  Result := (min <= num) and (num <= max);
end;
```
