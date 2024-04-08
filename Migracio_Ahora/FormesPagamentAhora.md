# [..](..)\Migracio Ahora\Formes de Pagament Ahora

## SQL per a crear totes les formes de pagament

```SQL
DECLARE @IdFormaPago T_Forma_Pago, @User T_CEESI_Usuario, @Date T_CEESI_Fecha_Sistema
DECLARE c CURSOR FOR select IdFormaPago, Usuario, FechaInsertUpdate from FormasPago where IdFormaPago <> 0;

OPEN c;

FETCH NEXT FROM c into @IdFormaPago,@User,@Date;

WHILE @@FETCH_STATUS = 0
BEGIN
	EXECUTE PFormasPago_D @IdFormaPago,@User,@Date;
	FETCH NEXT FROM c into @IdFormaPago,@User,@Date;	
END;

CLOSE c;

DEALLOCATE c;

exec dbo.Pers_pFormasPago_I 1,REBUT_BANCARI_07
exec dbo.Pers_pFormasPago_I 2,REBUT_BANCARI_10
exec dbo.Pers_pFormasPago_I 3,REBUT_BANCARI_15
exec dbo.Pers_pFormasPago_I 4,REBUT_BANCARI_20
exec dbo.Pers_pFormasPago_I 5,REBUT_BANCARI_25
exec dbo.Pers_pFormasPago_I 6,REBUT_BANCARI_30
exec dbo.Pers_pFormasPago_I 7,REBUT_BANCARI_45
exec dbo.Pers_pFormasPago_I 8,REBUT_BANCARI_50
exec dbo.Pers_pFormasPago_I 9,REBUT_BANCARI_60
exec dbo.Pers_pFormasPago_I 10,REBUT_BANCARI_90
exec dbo.Pers_pFormasPago_I 11,REBUT_BANCARI_120
exec dbo.Pers_pFormasPago_I 12,TRANSFER_07
exec dbo.Pers_pFormasPago_I 13,TRANSFER_10
exec dbo.Pers_pFormasPago_I 14,TRANSFER_15
exec dbo.Pers_pFormasPago_I 15,TRANSFER_20
exec dbo.Pers_pFormasPago_I 16,TRANSFERS_25
exec dbo.Pers_pFormasPago_I 17,TRANSFERS_30
exec dbo.Pers_pFormasPago_I 18,TRANSFERS_40
exec dbo.Pers_pFormasPago_I 19,TRANSFERS_45
exec dbo.Pers_pFormasPago_I 20,TRANSFERS_60
exec dbo.Pers_pFormasPago_I 21,TRANSFERS_75
exec dbo.Pers_pFormasPago_I 22,TRANSFERS_90
exec dbo.Pers_pFormasPago_I 23,TRANSFERS_120
exec dbo.Pers_pFormasPago_I 24,TALO_07
exec dbo.Pers_pFormasPago_I 25,TALO_10
exec dbo.Pers_pFormasPago_I 26,TALO_15
exec dbo.Pers_pFormasPago_I 27,TALO_30
exec dbo.Pers_pFormasPago_I 28,TALO_45
exec dbo.Pers_pFormasPago_I 29,TALO_60
exec dbo.Pers_pFormasPago_I 30,TALO_70
exec dbo.Pers_pFormasPago_I 31,TALO_90
exec dbo.Pers_pFormasPago_I 32,TALO_120
exec dbo.Pers_pFormasPago_I 33,TARGETA_07
exec dbo.Pers_pFormasPago_I 34,TARGETA_10
exec dbo.Pers_pFormasPago_I 35,EFECTIU_0
exec dbo.Pers_pFormasPago_I 36,EFECTIU_07
exec dbo.Pers_pFormasPago_I 37,EFECTIU_10
exec dbo.Pers_pFormasPago_I 38,EFECTIU_15
exec dbo.Pers_pFormasPago_I 39,EFECTIU_30
exec dbo.Pers_pFormasPago_I 40,EFECTIU_60
exec dbo.Pers_pFormasPago_I 41,EFECTIU_120
exec dbo.Pers_pFormasPago_I 42,COMPTAT_RIGOROS_0

exec dbo.Pers_pFormasPago_Lineas_I 1,2,7
exec dbo.Pers_pFormasPago_Lineas_I 2,2,10
exec dbo.Pers_pFormasPago_Lineas_I 3,2,15
exec dbo.Pers_pFormasPago_Lineas_I 4,2,20
exec dbo.Pers_pFormasPago_Lineas_I 5,2,25
exec dbo.Pers_pFormasPago_Lineas_I 6,2,30
exec dbo.Pers_pFormasPago_Lineas_I 7,2,45
exec dbo.Pers_pFormasPago_Lineas_I 8,2,50
exec dbo.Pers_pFormasPago_Lineas_I 9,2,60
exec dbo.Pers_pFormasPago_Lineas_I 10,2,90
exec dbo.Pers_pFormasPago_Lineas_I 11,2,120
exec dbo.Pers_pFormasPago_Lineas_I 12,3,7
exec dbo.Pers_pFormasPago_Lineas_I 13,3,10
exec dbo.Pers_pFormasPago_Lineas_I 14,3,15
exec dbo.Pers_pFormasPago_Lineas_I 15,3,20
exec dbo.Pers_pFormasPago_Lineas_I 16,3,25
exec dbo.Pers_pFormasPago_Lineas_I 17,3,30
exec dbo.Pers_pFormasPago_Lineas_I 18,3,40
exec dbo.Pers_pFormasPago_Lineas_I 19,3,45
exec dbo.Pers_pFormasPago_Lineas_I 20,3,60
exec dbo.Pers_pFormasPago_Lineas_I 21,3,75
exec dbo.Pers_pFormasPago_Lineas_I 22,3,90
exec dbo.Pers_pFormasPago_Lineas_I 23,3,120
exec dbo.Pers_pFormasPago_Lineas_I 24,1,7
exec dbo.Pers_pFormasPago_Lineas_I 25,1,10
exec dbo.Pers_pFormasPago_Lineas_I 26,1,15
exec dbo.Pers_pFormasPago_Lineas_I 27,1,30
exec dbo.Pers_pFormasPago_Lineas_I 28,1,45
exec dbo.Pers_pFormasPago_Lineas_I 29,1,60
exec dbo.Pers_pFormasPago_Lineas_I 30,1,70
exec dbo.Pers_pFormasPago_Lineas_I 31,1,90
exec dbo.Pers_pFormasPago_Lineas_I 32,1,120
exec dbo.Pers_pFormasPago_Lineas_I 33,10,7
exec dbo.Pers_pFormasPago_Lineas_I 34,10,10
exec dbo.Pers_pFormasPago_Lineas_I 35,9,0
exec dbo.Pers_pFormasPago_Lineas_I 36,9,7
exec dbo.Pers_pFormasPago_Lineas_I 37,9,10
exec dbo.Pers_pFormasPago_Lineas_I 38,9,15
exec dbo.Pers_pFormasPago_Lineas_I 39,9,30
exec dbo.Pers_pFormasPago_Lineas_I 40,9,60
exec dbo.Pers_pFormasPago_Lineas_I 41,9,120
exec dbo.Pers_pFormasPago_Lineas_I 42,0,0
```
