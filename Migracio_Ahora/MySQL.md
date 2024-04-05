# [..](..)\MySQL

## Index

- [Index](#index)
- [ppn\_clients](#ppn_clients)
- [KNIF\_CLIE](#knif_clie)

## ppn_clients

```SQL
CREATE OR REPLACE VIEW ppn_clients AS
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
        c.CLIE_NUMERO1 AS TipusCaixa,
        c.CLIE_NUMERO2 AS Merma,
        c.CLIE_NUMERO3 AS EnviarEmail,
        c.CLIE_NUMERO4 AS Gel,
		c.CLIE_POBLACIO AS CLIE_POBLACIO,
        c.CLIE_CODIPOSTAL,
        c.CLIE_EDIEANCOMPRADOR as TipusFact,
        ifnull(nullif(c.CLIE_EDIEANCLIENT,''), 0) as Potencial,
        c.CLIE_EDIEANRECEPTOR as TipusABC,
        ifnull(nullif(c.CLIE_EDIEANPAGADOR,''), 0) as Objectiu,
        c.CLIE_DATA3 as DataInici,
        c.CLIE_DATA4 as DataFi
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
            AND (adrecesenviament.ADRE_PRINCIPAL = 'S'))))
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
```

## KNIF_CLIE

Index per nif a la taula clients

```SQL
create index KNIF_CLIE
on clients(CLIE_NIF)
```
