# [..](..)\Migracio Ahora\StoredProcedures

## Index

- [Index](#index)
- [Pers\_PClientes\_Datos\_I](#pers_pclientes_datos_i)
  - [@IdTipo](#idtipo)
  - [@IdPoblacion](#idpoblacion)
  - [@IdTipoOtro](#idtipootro)
  - [@Bloqueado](#bloqueado)
  - [Propietats no prigest](#propietats-no-prigest)
  - [Autocalculades per Ahora](#autocalculades-per-ahora)
  - [No utilitzades per Puignau](#no-utilitzades-per-puignau)
  - [Automatiques](#automatiques)
  - [Camps Configurables](#camps-configurables)
  - [SQL](#sql)
- [Pers\_PClientes\_Datos\_Economicos\_U](#pers_pclientes_datos_economicos_u)
  - [Cursor](#cursor)
  - [DiaPago1 i DiaPago2](#diapago1-i-diapago2)
  - [Camps per fer l'Update](#camps-per-fer-lupdate)
  - [SQL](#sql-1)
- [Pers\_PClientes\_Datos\_Comerciales\_U](#pers_pclientes_datos_comerciales_u)
  - [SQL](#sql-2)
- [Pers\_PFormasPago\_I](#pers_pformaspago_i)
  - [SQL](#sql-3)
- [Pers\_PFormasPago\_Lineas\_I](#pers_pformaspago_lineas_i)
  - [SQL](#sql-4)

## Pers_PClientes_Datos_I

### @IdTipo

Verifica que `@IdTipo` tingui un valor correcte i en cas contrari, posa un valor per defecte, el `MIN`.

```SQL
Set @IdTipo = 
	(Select case 
		when 
			@IdTipoIN < (select min(IdTipo) from Clientes_Tipos) 
			or (select max(IdTipo) from Clientes_Tipos) < @IdTipoIN 
		then 
			(select min(IdTipo) from Clientes_Tipos)
		else 
			@IdTipoIN
	end); -- Taula Clientes_Tipos (Potencial (-1), Nacional (0), Comunitario (1), Extranjero (2))
```

### @IdPoblacion

Busca la relacio entre les poblacions de prigest i les de Ahora.

```SQL
Set @IdPoblacion = isnull((Select tp.IdPoblacioAhora From Traduccions..Poblacions tp where tp.PoblacioPrigest = @IdPoblacionIN),0)
```

### @IdTipoOtro

Verifica que `@IdTipoOtro` tingui un valor correcte i en cas contrari, posa un valor per defecte, el `0`.

```SQL
Set @IdTipoOtro = (Select case when @IdTipoOtroIN < (select min(IdTipoOtro) from Clientes_TiposOtros) or (select max(IdTipoOtro) from Clientes_TiposOtros) < @IdTipoOtroIN then 0 else @IdTipoOtroIN end); -- Taula Clientes_TiposOtros (Normal (0), Destacat (1), VIP (2))
```

### @Bloqueado

Defineix `@Bloqueado` i `@IdMotivoBloqueo` segons si el client esta de baixa o no.

```SQL
Set @Bloqueado = (Select case when @DataBaixaIN is null then 0 else 1 end) -- Alta (0) Baixa (1)
Set @IdMotivoBloqueo = (Select case when @DataBaixaIN is null then 0 else 1 end) -- Sense Motiu (0) Client de Baixa (1)
Set @IdDelegacionCli = (select count(*) from Clientes_Datos where Nif = @Nif)
```

### Propietats no prigest

Aquestes propietats son obligatories, pero no les trobarem a prigest.

```SQL
-- Necessitem, no prigest
Set @TipoTransporte = 0;
Set @IdDelegacion = -1;
Set @Cliente_Contado = 0;
Set @EsUbicacion = '';
```

### Autocalculades per Ahora

Aquestes propietats les calcula Ahora en algun trigger o stored a partir d'aquest punt

```SQL
-- dummy Ja es calculen automaticament
Set @Kmts = 0;
Set @IdContacto = 0;
Set @IdContactoA = @IdContacto;
Set @IdContactoF = @IdContacto;
Set @IdContactoG = @IdContacto;
```

### No utilitzades per Puignau

Peixos Puignau no utilitza el Mode SWIN ni ECommerce.

```SQL
-- No Utilitzar
SEt @ECommerce_Activo = 0;
Set @SWIN_ComisionVendedor1 = 0;
Set @SWIN_ComisionVendedor2 = 0;
Set @SWIN_ComisionVendedor3 = 0;
Set @SWIN_ComisionVendedor4 = 0;
Set @SWIN_PedidoEnviarEmail = 0;
Set @SWIN_PresupuestoEnviarEmail = 0;
Set @SWIN_AlbaranEnviarEmail = 0;
Set @SWIN_FacturaEnviarEmail = 0;
Set @SWIN_VerObsEnDocumentos = 0;
```

### Automatiques

`@Padre` en cas de ser `@Nivell = 0` li posarem **NULL** ja que representa que es el pare. En cas contrari, buscarem qui es el seu pare, que sera el client amb `NifPare = Nif`

`@IdCliente` en cas de ser `@Padre is null` li assignarem un autonumeric. Sino, li assignarem un codi de client el qual es formara del codi de client pare i el concatenat el numero de planta d'aquell pare.

```SQL
-- Automatiques
Set @Padre = (Select case @Nivel when 0 then null else (Select IdCliente from Clientes_Datos where Nif = @Nif and Nivel = 0) end)
Set @IdCliente = (select case when @Padre is null then (select RIGHT('0000000' + cast(isnull(max(isnull(cast(IdCliente as int),0))+1,1) as varchar), 7) from Clientes_Datos where IdTipo <> -1�and�Nivel�=�0) else RIGHT('0000000' + @Padre, 7) + RIGHT('000' + cast(@IdDelegacionCli as varchar), 3) end);
Set @IdPais = (Select IdPais from VGeo_Poblaciones where IdPoblacion = @IdPoblacion)
Set @IdProvincia = (Select IdProvincia from VGeo_Poblaciones where IdPoblacion = @IdPoblacion)
Set @Ciudad = (Select Poblacion from VGeo_Poblaciones where IdPoblacion = @IdPoblacion)
Set @Provincia = (Select Provincia from Geo_Provincias where IdProvincia = @IdProvincia and IdPais = @IdPais)
Set @Pais = (Select Pais from Geo_Paises where IdPais = @IdPais)
```

### Camps Configurables

Actualitzem les propietats configurables.

```SQL
Update Conf_Clientes 
  Set P_DataBaixa = @DataBaixaIN,
  P_EnviarEtiqMail = @EnviarEtiqMailIN,
  P_Gel = @GelIN,
  P_TipusCaixPrep = @TipusCaixPrepIN,
  P_Merma = @MermaIN,
  P_Potencial = @PotencialIN,
  P_Objectiu = @ObjectiuIN,
  P_TipusABC = @TipusABCIN,
  P_TipusFacturacio = @TipusFacturacioIN,
  P_DataIniciVacances = @IniciVacancesIN,
  P_DataFiVacances = @FiVacancesIN
where IdCliente = @IdCliente;
```

### SQL

```SQL
alter PROCEDURE [dbo].[Pers_PClientes_Datos_I]
	@ClienteIN 		T_Nombre_Corto  OUTPUT,
	@DireccionIN 		T_Direccion  OUTPUT,	
	@IdTipoIN 		T_Id_Tipo  OUTPUT,
	@NumTelefonoIN 		T_Num_Telefono  OUTPUT,
	@FechaAltaIN 		T_Fecha_Corta  OUTPUT,
	@NifIN 			T_NIF  	OUTPUT,
	@MicodIN			Varchar(15) OUTPUT,
	@RazonSocialIN		varchar(255) OUTPUT,
	@IdTipoOtroIN		T_Id_Tipo	OUTPUT,
	@IdPoblacionIN varchar(50) OUTPUT,
	@DataBaixaIN DateTime OUTPUT,
	@EnviarEtiqMailIN bit OUTPUT,
	@GelIN bit OUTPUT,
	@TipusCaixPrepIN smallint OUTPUT,
	@MermaIN bit OUTPUT,
	@NivelIN tinyint OUTPUT,
	@CodPostalIN [dbo].[T_Codigo_Postal] OUTPUT,
	@PotencialIN numeric(18,2),
	@ObjectiuIN numeric(18,2),
	@TipusABCIN varchar(1),
	@TipusFacturacioIN varchar(10),
	@IniciVacancesIN Datetime,
	@FiVacancesIN Datetime
AS

DECLARE @RC int
DECLARE @IdCliente [dbo].[T_Id_Cliente]
DECLARE @Ciudad [dbo].[T_Ciudad]
DECLARE @Provincia [dbo].[T_Provincia]
DECLARE @CodPostal [dbo].[T_Codigo_Postal]
DECLARE @Pais [dbo].[T_Pais]
DECLARE @IdContacto int
DECLARE @IdContactoA int
DECLARE @IdContactoF int
DECLARE @Bloqueado [dbo].[T_Booleano]
DECLARE @TipoTransporte [dbo].[T_Id_TipoTransporte]
DECLARE @IdProvincia [dbo].[T_Id_Provincia]
DECLARE @IdPais [dbo].[T_Id_Pais]
DECLARE @Kmts [dbo].[T_Precio]
DECLARE @EsUbicacion [dbo].[T_Booleano]
DECLARE @Cliente_Contado [dbo].[T_Booleano]
DECLARE @IdMotivoBloqueo int
DECLARE @IdEmpleadoBloqueo int
DECLARE @IdContactoG [dbo].[T_Id_Contacto]
DECLARE @ECommerce_PersonaNombre varchar(255)
DECLARE @ECommerce_PersonaApellidos varchar(255)
DECLARE @ECommerce_Activo bit
DECLARE @SWIN_IdVendedor1 int
DECLARE @SWIN_ComisionVendedor1 decimal(38,14)
DECLARE @SWIN_IdVendedor2 int
DECLARE @SWIN_ComisionVendedor2 decimal(38,14)
DECLARE @SWIN_IdVendedor3 int
DECLARE @SWIN_ComisionVendedor3 decimal(38,14)
DECLARE @SWIN_IdVendedor4 int
DECLARE @SWIN_ComisionVendedor4 decimal(38,14)
DECLARE @SWIN_TextoLibre1 varchar(1000)
DECLARE @SWIN_TextoLibre2 varchar(1000)
DECLARE @SWIN_TextoLibre3 varchar(1000)
DECLARE @SWIN_TextoLibre4 varchar(1000)
DECLARE @SWIN_TextoLibre5 varchar(1000)
DECLARE @SWIN_IdFormaPago int
DECLARE @SWIN_IdTipoEfecto [dbo].[T_Id_TipoEfecto]
DECLARE @SWIN_IdAgencia int
DECLARE @SWIN_PedidoPlantilla int
DECLARE @SWIN_PedidoEnviarEmail bit
DECLARE @SWIN_PedidoEmail varchar(250)
DECLARE @SWIN_PedidoReceptor varchar(250)
DECLARE @SWIN_PresupuestoPlantilla int
DECLARE @SWIN_PresupuestoEnviarEmail bit
DECLARE @SWIN_PresupuestoEmail varchar(250)
DECLARE @SWIN_PresupuestoReceptor varchar(250)
DECLARE @SWIN_AlbaranPlantilla int
DECLARE @SWIN_AlbaranEnviarEmail bit
DECLARE @SWIN_AlbaranEmail varchar(250)
DECLARE @SWIN_AlbaranReceptor varchar(250)
DECLARE @SWIN_FacturaPlantilla int
DECLARE @SWIN_FacturaEnviarEmail bit
DECLARE @SWIN_FacturaEmail varchar(250)
DECLARE @SWIN_FacturaReceptor varchar(250)
DECLARE @SWIN_Observaciones varchar(8000)
DECLARE @SWIN_VerObsEnDocumentos bit
DECLARE @Padre [dbo].[T_Id_Cliente]
DECLARE @Nivel tinyint
DECLARE @IdTipo	T_Id_Tipo
DECLARE @Cliente [dbo].[T_Nombre_Corto]
DECLARE @Direccion [dbo].[T_Direccion]
DECLARE @NumTelefono [dbo].[T_Num_Telefono]
DECLARE @Extension [dbo].[T_Extension]
DECLARE @NumFax [dbo].[T_Num_Fax]
DECLARE @E_Mail [dbo].[T_E_Mail]
DECLARE @Web varchar(255)
DECLARE @FechaAlta [dbo].[T_Fecha_Corta]
DECLARE @Notas [dbo].[T_Notas]
DECLARE @Nif [dbo].[T_NIF]
DECLARE @Nif2 [dbo].[T_NIF]
DECLARE @Micod varchar(15)
DECLARE @RazonSocial varchar(255)
DECLARE @IdContactoCliente int
DECLARE @IdDelegacion [dbo].[T_Id_Delegacion]
DECLARE @IdCalendario [dbo].[T_Id_Calendario]
DECLARE @IdTipoOtro [dbo].[T_Id_Tipo]
DECLARE @IdRelacion int
DECLARE @RappelsPorPlanta [dbo].[T_Booleano]
DECLARE @IdDelegacionCli [dbo].[T_Id_Delegacion]
DECLARE @Observaciones [dbo].[T_Observaciones]
DECLARE @IdDoc [dbo].[T_Id_Doc]
DECLARE @Usuario [dbo].[T_CEESI_Usuario]
DECLARE @FechaInsertUpdate [dbo].[T_CEESI_Fecha_Sistema]
DECLARE @IdPoblacion [dbo].[T_Id_Poblacion]
DECLARE @IdCompañia [dbo].[T_Id_Compañia]


Set @IdTipo = 
	(Select case 
		when 
			@IdTipoIN < (select min(IdTipo) from Clientes_Tipos) 
			or (select max(IdTipo) from Clientes_Tipos) < @IdTipoIN 
		then 
			(select min(IdTipo) from Clientes_Tipos)
		else 
			@IdTipoIN
	end); -- Taula Clientes_Tipos (Potencial (-1), Nacional (0), Comunitario (1), Extranjero (2))

Set @Cliente = @ClienteIN;
Set @RazonSocial = @RazonSocialIN;
Set @Micod = @MiCodIN;
Set @Direccion = @DireccionIN;
Set @NumTelefono = @NumTelefonoIN;
Set @IdPoblacion = isnull((Select tp.IdPoblacioAhora From Traduccions..Poblacions tp where tp.PoblacioPrigest = @IdPoblacionIN),0)
Set @FechaAlta = @FechaAltaIN;
Set @IdTipoOtro = (Select case when @IdTipoOtroIN < (select min(IdTipoOtro) from Clientes_TiposOtros) or (select max(IdTipoOtro) from Clientes_TiposOtros) < @IdTipoOtroIN then 0 else @IdTipoOtroIN end); -- Taula Clientes_TiposOtros (Normal (0), Destacat (1), VIP (2))
Set @Nif = @NifIN;
Set @CodPostal = @CodPostalIN
Set @IdCompañia = 0; -- Taula Clientes_Compa�ias (Possible Agrupacio de clients)

Set @Bloqueado = (Select case when @DataBaixaIN is null then 0 else 1 end) -- Alta (0) Baixa (1)
Set @IdMotivoBloqueo = (Select case when @DataBaixaIN is null then 0 else 1 end) -- Sense Motiu (0) Client de Baixa (1)
Set @IdDelegacionCli = (select count(*) from Clientes_Datos where Nif = @Nif)
Set @Nivel = @NivelIN;  -- Taula Clientes_Niveles

-- Necessitem, no prigest
Set @TipoTransporte = 0;
Set @IdDelegacion = -1;
Set @Cliente_Contado = 0;
Set @EsUbicacion = '';

-- dummy Ja es calculen automaticament
Set @Kmts = 0;
Set @IdContacto = 0;
Set @IdContactoA = @IdContacto;
Set @IdContactoF = @IdContacto;
Set @IdContactoG = @IdContacto;

-- No Utilitzar
SEt @ECommerce_Activo = 0;
Set @SWIN_ComisionVendedor1 = 0;
Set @SWIN_ComisionVendedor2 = 0;
Set @SWIN_ComisionVendedor3 = 0;
Set @SWIN_ComisionVendedor4 = 0;
Set @SWIN_PedidoEnviarEmail = 0;
Set @SWIN_PresupuestoEnviarEmail = 0;
Set @SWIN_AlbaranEnviarEmail = 0;
Set @SWIN_FacturaEnviarEmail = 0;
Set @SWIN_VerObsEnDocumentos = 0;

-- Automatiques
Set @Padre = (Select case @Nivel when 0 then null else (Select IdCliente from Clientes_Datos where Nif = @Nif and Nivel = 0) end)
Set @IdCliente = (select case when @Padre is null then (select RIGHT('0000000' + cast(isnull(max(isnull(cast(IdCliente as int),0))+1,1) as varchar), 7) from Clientes_Datos where IdTipo <> -1�and�Nivel�=�0) else RIGHT('0000000' + @Padre, 7) + RIGHT('000' + cast(@IdDelegacionCli as varchar), 3) end);
Set @IdPais = (Select IdPais from VGeo_Poblaciones where IdPoblacion = @IdPoblacion)
Set @IdProvincia = (Select IdProvincia from VGeo_Poblaciones where IdPoblacion = @IdPoblacion)
Set @Ciudad = (Select Poblacion from VGeo_Poblaciones where IdPoblacion = @IdPoblacion)
Set @Provincia = (Select Provincia from Geo_Provincias where IdProvincia = @IdProvincia and IdPais = @IdPais)
Set @Pais = (Select Pais from Geo_Paises where IdPais = @IdPais)

EXECUTE @RC = [dbo].[PClientes_Datos_I] 
   @IdCompa�ia OUTPUT
  ,@IdCliente OUTPUT
  ,@Cliente OUTPUT
  ,@Direccion OUTPUT
  ,@Ciudad OUTPUT
  ,@Provincia OUTPUT
  ,@CodPostal OUTPUT
  ,@Pais OUTPUT
  ,@IdTipo OUTPUT
  ,@IdContacto OUTPUT
  ,@IdContactoA OUTPUT
  ,@IdContactoF OUTPUT
  ,@NumTelefono OUTPUT
  ,@Extension OUTPUT
  ,@NumFax OUTPUT
  ,@E_Mail OUTPUT
  ,@Web OUTPUT
  ,@FechaAlta OUTPUT
  ,@Notas OUTPUT
  ,@Nif OUTPUT
  ,@Nif2 OUTPUT
  ,@Padre OUTPUT
  ,@Nivel OUTPUT
  ,@Micod OUTPUT
  ,@Bloqueado OUTPUT
  ,@RazonSocial OUTPUT
  ,@IdContactoCliente OUTPUT
  ,@TipoTransporte OUTPUT
  ,@IdDelegacion OUTPUT
  ,@IdPoblacion OUTPUT
  ,@IdProvincia OUTPUT
  ,@IdPais OUTPUT
  ,@Kmts OUTPUT
  ,@IdCalendario OUTPUT
  ,@IdTipoOtro OUTPUT
  ,@IdRelacion OUTPUT
  ,@RappelsPorPlanta OUTPUT
  ,@EsUbicacion OUTPUT
  ,@Cliente_Contado OUTPUT
  ,@IdMotivoBloqueo OUTPUT
  ,@IdEmpleadoBloqueo OUTPUT
  ,@IdContactoG OUTPUT
  ,@IdDelegacionCli OUTPUT
  ,@ECommerce_PersonaNombre OUTPUT
  ,@ECommerce_PersonaApellidos OUTPUT
  ,@Observaciones OUTPUT
  ,@ECommerce_Activo OUTPUT
  ,@SWIN_IdVendedor1 OUTPUT
  ,@SWIN_ComisionVendedor1 OUTPUT
  ,@SWIN_IdVendedor2 OUTPUT
  ,@SWIN_ComisionVendedor2 OUTPUT
  ,@SWIN_IdVendedor3 OUTPUT
  ,@SWIN_ComisionVendedor3 OUTPUT
  ,@SWIN_IdVendedor4 OUTPUT
  ,@SWIN_ComisionVendedor4 OUTPUT
  ,@SWIN_TextoLibre1 OUTPUT
  ,@SWIN_TextoLibre2 OUTPUT
  ,@SWIN_TextoLibre3 OUTPUT
  ,@SWIN_TextoLibre4 OUTPUT
  ,@SWIN_TextoLibre5 OUTPUT
  ,@SWIN_IdFormaPago OUTPUT
  ,@SWIN_IdTipoEfecto OUTPUT
  ,@SWIN_IdAgencia OUTPUT
  ,@SWIN_PedidoPlantilla OUTPUT
  ,@SWIN_PedidoEnviarEmail OUTPUT
  ,@SWIN_PedidoEmail OUTPUT
  ,@SWIN_PedidoReceptor OUTPUT
  ,@SWIN_PresupuestoPlantilla OUTPUT
  ,@SWIN_PresupuestoEnviarEmail OUTPUT
  ,@SWIN_PresupuestoEmail OUTPUT
  ,@SWIN_PresupuestoReceptor OUTPUT
  ,@SWIN_AlbaranPlantilla OUTPUT
  ,@SWIN_AlbaranEnviarEmail OUTPUT
  ,@SWIN_AlbaranEmail OUTPUT
  ,@SWIN_AlbaranReceptor OUTPUT
  ,@SWIN_FacturaPlantilla OUTPUT
  ,@SWIN_FacturaEnviarEmail OUTPUT
  ,@SWIN_FacturaEmail OUTPUT
  ,@SWIN_FacturaReceptor OUTPUT
  ,@SWIN_Observaciones OUTPUT
  ,@SWIN_VerObsEnDocumentos OUTPUT
  ,@IdDoc OUTPUT
  ,@Usuario OUTPUT
  ,@FechaInsertUpdate OUTPUT;

Update Conf_Clientes 
  Set P_DataBaixa = @DataBaixaIN,
  P_EnviarEtiqMail = @EnviarEtiqMailIN,
  P_Gel = @GelIN,
  P_TipusCaixPrep = @TipusCaixPrepIN,
  P_Merma = @MermaIN,
  P_Potencial = @PotencialIN,
  P_Objectiu = @ObjectiuIN,
  P_TipusABC = @TipusABCIN,
  P_TipusFacturacio = @TipusFacturacioIN,
  P_DataIniciVacances = @IniciVacancesIN,
  P_DataFiVacances = @FiVacancesIN
where IdCliente = @IdCliente;
Return @RC
```

## Pers_PClientes_Datos_Economicos_U

### Cursor

Degut a que podria ser que hi hagues 2 clients amb el mateix codi de client de Prigest, utilitzo el seguent codi per a crear un cursor i aplicar els canvis a tots els clients amb `Micod = @IdClienteIN`.

```SQL
DECLARE clients CURSOR FOR Select IdCliente from Clientes_Datos where MiCod = @IdClienteIN

OPEN clients;

FETCH NEXT FROM clients into @Idcliente

WHILE @@FETCH_STATUS = 0
BEGIN

  -- logica del loop

	FETCH NEXT FROM clients into @Idcliente;
END;

CLOSE clients;

DEALLOCATE clients;
```

### DiaPago1 i DiaPago2

Ahora no permet que `DiaPago1 = 0` i `DiaPago2 <> 0`, per aixo en aquest cas apliquem un petit canvi.

```SQL
if @DiaPago1 = 0 and @DiaPago2 <> 0
begin
	Set @DiaPago1 = @DiaPago2 
	Set @DiaPago2 = 0
end
```

### Camps per fer l'Update

Al ser un Update, necesitem saber qui ha actualitzat el registre per ultim cop i quan ho ha fet.

```SQL
select @Usuario = Usuario, @FechaInsertUpdate = FechaInsertUpdate from Clientes_Datos_Economicos where IdCliente = @IdCliente;
```

### SQL

```SQL
ALTER procedure [dbo].[Pers_PClientes_Datos_Economicos_U] 
	@TipusFacturacioIN varchar(3),
	@IdClienteIN [dbo].[T_Id_Cliente],
	@DescuentoIN [dbo].[T_Decimal],
	@ProntoPagoIN [dbo].[T_Decimal],
	@IdIvaIN [dbo].[T_Id_Iva],
	@DiaPago1IN [dbo].[T_Dia_Pago],
	@DiaPago2IN [dbo].[T_Dia_Pago],
	@RiesgoIN [dbo].[T_Riesgo],
	@IdPortesIN [dbo].[T_Id_Portes],
	@PeriodicidadPagoIN [dbo].[T_Periodicidad_Pago],
	@FormaPagoIN [dbo].[T_Forma_Pago],
	@SerieFacturacionIN [dbo].[T_Serie]
as 
DECLARE @RC int
DECLARE @IdCliente [dbo].[T_Id_Cliente]
DECLARE @IdCliente_A [dbo].[T_Id_Cliente]
DECLARE @Usuario [dbo].[T_CEESI_Usuario]
DECLARE @FechaInsertUpdate [dbo].[T_CEESI_Fecha_Sistema]
DECLARE @IdLista [dbo].[T_Id_Lista]
DECLARE @FormaPago [dbo].[T_Forma_Pago]
DECLARE @CopiasFactura [dbo].[T_Cantidad_Bulto]
DECLARE @CopiasAlbaran [dbo].[T_Cantidad_Bulto]
DECLARE @Cuenta [dbo].[T_Cuenta_Contable]
DECLARE @IdDomiciliacion [dbo].[T_Id_Domiciliacion]
DECLARE @DiaPago1 [dbo].[T_Dia_Pago]
DECLARE @DiaPago2 [dbo].[T_Dia_Pago]
DECLARE @DiaPago3 [dbo].[T_Dia_Pago]
DECLARE @Riesgo [dbo].[T_Riesgo]
DECLARE @Riesgo_Euro [dbo].[T_Riesgo]
DECLARE @AlbValorado [dbo].[T_Booleano]
DECLARE @AlbPorPedido [dbo].[T_Booleano]
DECLARE @MesVacaciones [dbo].[T_Mes_Vacaciones]
DECLARE @IdTransportista [dbo].[T_Id_Proveedor]
DECLARE @IdMoneda [dbo].[T_Id_Moneda]
DECLARE @NSerie [dbo].[T_Serie]
DECLARE @Precodigo varchar(255)
DECLARE @FacturarPorContacto [dbo].[T_Booleano]
DECLARE @TipoFacturacion smallint
DECLARE @RecEquivalencia [dbo].[T_Booleano]
DECLARE @SerieContrato [dbo].[T_Serie]
DECLARE @CopiasPedido smallint
DECLARE @CopiasOferta smallint
DECLARE @FacturarPorReferencia [dbo].[T_Booleano]
DECLARE @NumPedidoObligado [dbo].[T_Booleano]
DECLARE @DiasPrecioMin smallint
DECLARE @IdRegimen smallint
DECLARE @MaxFacturacion float
DECLARE @IdListaTra [dbo].[T_Id_Lista]
DECLARE @NoBloqueaRiesgo [dbo].[T_Booleano]
DECLARE @IdTipoPedido int
DECLARE @DiasCarencia smallint
DECLARE @AgruparEfectos [dbo].[T_Booleano]
DECLARE @IdListaCont [dbo].[T_Id_Lista]
DECLARE @IdTarifa [dbo].[T_Id_Tarifa]
DECLARE @IdDelegacionDto [dbo].[T_Id_Delegacion]
DECLARE @FactXContrato [dbo].[T_Booleano]
DECLARE @IdImpuestoIndirecto smallint
DECLARE @F_Nacimiento [dbo].[T_Fecha_Corta]
DECLARE @Sexo smallint
DECLARE @Asociacion smallint
DECLARE @No_Paga_Cuota [dbo].[T_Booleano]
DECLARE @Categoria smallint
DECLARE @IdProveedor [dbo].[T_Id_Proveedor]
DECLARE @F_Alta_Socio [dbo].[T_Fecha_Corta]
DECLARE @F_Baja_Socio [dbo].[T_Fecha_Corta]
DECLARE @Aportacion_Socio float
DECLARE @AplicarLeyMorosidad [dbo].[T_Booleano]
DECLARE @CambioEnFactura [dbo].[T_Booleano]
DECLARE @IdMetodoPrecio smallint
DECLARE @IdGrupoRet int
DECLARE @IvaDefecto [dbo].[T_Booleano]
DECLARE @IdLineaDescuento int
DECLARE @IdModoFacturacion smallint
DECLARE @Descuento [dbo].[T_Decimal]
DECLARE @ProntoPago [dbo].[T_Decimal]
DECLARE @IdIva [dbo].[T_Id_Iva]
DECLARE @IdPortes [dbo].[T_Id_Portes]
DECLARE @PeriodicidadPago [dbo].[T_Periodicidad_Pago]

DECLARE clients CURSOR FOR Select IdCliente from Clientes_Datos where MiCod = @IdClienteIN

OPEN clients;

FETCH NEXT FROM clients into @Idcliente

WHILE @@FETCH_STATUS = 0
BEGIN

-- INPUTS
Set @TipoFacturacion = isnull((Select IdAhora from Traduccions..TipusFacturacio where IdPrigest = @TipusFacturacioIN),1);
Set @Descuento = @DescuentoIN
Set @ProntoPago = @ProntoPagoIN
Set @IdIva = @IdIvaIN
Set @DiaPago1 = @DiaPago1IN
Set @DiaPago2 = @DiaPago2IN
Set @NSerie = @SerieFacturacionIN

if @DiaPago1 = 0 and @DiaPago2 <> 0
begin
	Set @DiaPago1 = @DiaPago2 
	Set @DiaPago2 = 0
end

If @IdCliente is null
begin

  RAISERROR('CLIENT NO TROBAT',12,1)
  return 0
end

Set @Riesgo = @RiesgoIN
Set @IdPortes = @IdPortesIN
Set @PeriodicidadPago = @PeriodicidadPagoIN
Set @FormaPago = @FormaPagoIN

-- Auto
select @Usuario = Usuario, @FechaInsertUpdate = FechaInsertUpdate from Clientes_Datos_Economicos where IdCliente = @IdCliente;
Set @IdCliente_A = @IdCliente;
Set @DiaPago3 = 0;
Set @Riesgo_Euro = @Riesgo;

Set @CopiasFactura = 1
Set @CopiasAlbaran = 1
Set @Precodigo = NULL
Set @SerieContrato = NULL
Set @DiasPrecioMin = NULL
Set @IdListaTra = NULL
Set @IdTarifa = 0
SEt @F_Nacimiento = NULL
Set @IdProveedor = null
Set @F_Alta_Socio = null
Set @F_Baja_Socio = null
SEt @IdGrupoRet = null
Set @Aportacion_Socio = null
Set @IdLineaDescuento = null


-- Obligats
Set @MesVacaciones = 0;
Set @IdMoneda = 1; -- Taula: Moneda (Euro)
Set @IdDomiciliacion = 0
Set @AlbValorado = 0;
Set @AlbPorPedido = 0;
Set @FacturarPorContacto = 0;
Set @RecEquivalencia = 0
Set @FacturarPorReferencia = 0
Set @NumPedidoObligado = 0
Set @IdRegimen = 0
Set @MaxFacturacion = 0
Set @NoBloqueaRiesgo = 0
Set @IdTipoPedido = 0
Set @DiasCarencia = 0
Set @AgruparEfectos = 0
Set @FactXContrato = 0
Set @IdImpuestoIndirecto = 0 -- Impuestos_Indirectos
Set @Sexo = 0
Set @Asociacion = 0
Set @No_Paga_Cuota = 0
Set @Categoria = 0
Set @AplicarLeyMorosidad = 0
Set @CambioEnFactura = 0
Set @IdMetodoPrecio = 1 -- Metodo_Calculo_Precios
Set @IvaDefecto = 0
Set @IdModoFacturacion = 0
Set @IdLista = 0

EXECUTE @RC = [dbo].[PClientes_Datos_Economicos_U]
   @IdCliente_A
  ,@Usuario OUTPUT
  ,@FechaInsertUpdate OUTPUT
  ,@IdCliente OUTPUT
  ,@IdLista OUTPUT
  ,@FormaPago OUTPUT
  ,@Descuento OUTPUT
  ,@ProntoPago OUTPUT
  ,@IdIva OUTPUT
  ,@CopiasFactura OUTPUT
  ,@CopiasAlbaran OUTPUT
  ,@Cuenta OUTPUT
  ,@IdDomiciliacion OUTPUT
  ,@DiaPago1 OUTPUT
  ,@DiaPago2 OUTPUT
  ,@DiaPago3 OUTPUT
  ,@Riesgo OUTPUT
  ,@Riesgo_Euro OUTPUT
  ,@IdPortes OUTPUT
  ,@AlbValorado OUTPUT
  ,@AlbPorPedido OUTPUT
  ,@PeriodicidadPago OUTPUT
  ,@MesVacaciones OUTPUT
  ,@IdTransportista OUTPUT
  ,@IdMoneda OUTPUT
  ,@NSerie OUTPUT
  ,@Precodigo OUTPUT
  ,@FacturarPorContacto OUTPUT
  ,@TipoFacturacion OUTPUT
  ,@RecEquivalencia OUTPUT
  ,@SerieContrato OUTPUT
  ,@CopiasPedido OUTPUT
  ,@CopiasOferta OUTPUT
  ,@FacturarPorReferencia OUTPUT
  ,@NumPedidoObligado OUTPUT
  ,@DiasPrecioMin OUTPUT
  ,@IdRegimen OUTPUT
  ,@MaxFacturacion OUTPUT
  ,@IdListaTra OUTPUT
  ,@NoBloqueaRiesgo OUTPUT
  ,@IdTipoPedido OUTPUT
  ,@DiasCarencia OUTPUT
  ,@AgruparEfectos OUTPUT
  ,@IdListaCont OUTPUT
  ,@IdTarifa OUTPUT
  ,@IdDelegacionDto OUTPUT
  ,@FactXContrato OUTPUT
  ,@IdImpuestoIndirecto OUTPUT
  ,@F_Nacimiento OUTPUT
  ,@Sexo OUTPUT
  ,@Asociacion OUTPUT
  ,@No_Paga_Cuota OUTPUT
  ,@Categoria OUTPUT
  ,@IdProveedor OUTPUT
  ,@F_Alta_Socio OUTPUT
  ,@F_Baja_Socio OUTPUT
  ,@Aportacion_Socio OUTPUT
  ,@AplicarLeyMorosidad OUTPUT
  ,@CambioEnFactura OUTPUT
  ,@IdMetodoPrecio OUTPUT
  ,@IdGrupoRet OUTPUT
  ,@IvaDefecto OUTPUT
  ,@IdLineaDescuento OUTPUT
  ,@IdModoFacturacion OUTPUT

	FETCH NEXT FROM clients into @Idcliente;
END;

CLOSE clients;

DEALLOCATE clients;

select @RC Result
```

## Pers_PClientes_Datos_Comerciales_U

### SQL

```SQL
alter procedure Pers_PClientes_Datos_Comerciales_U
as
DECLARE @RC int
DECLARE @IdCliente_A [dbo].[T_Id_Cliente]
DECLARE @Usuario [dbo].[T_CEESI_Usuario]
DECLARE @FechaInsertUpdate [dbo].[T_CEESI_Fecha_Sistema]
DECLARE @IdCliente [dbo].[T_Id_Cliente]
DECLARE @Sector [dbo].[T_Sector]
DECLARE @CapitalSocial [dbo].[T_Precio_EURO]
DECLARE @Facturacion [dbo].[T_Precio_EURO]
DECLARE @Empleados [dbo].[T_Cantidad]
DECLARE @Origen [dbo].[T_Origen]
DECLARE @ObservacionesCom nvarchar(4000)
DECLARE @Clasificacion [dbo].[T_Clasificaci�n]
DECLARE @Mailing bit
DECLARE @DiasTransporte [dbo].[T_Cantidad]
DECLARE @FechaEntrega bit
DECLARE @IdEmpleado [dbo].[T_Id_Empleado]
DECLARE @Zona [dbo].[T_Zona]
DECLARE @Atencion [dbo].[T_Descripcion]
DECLARE @DirEntrega [dbo].[T_Descripcion_Larga]
DECLARE @NotaAlb [dbo].[T_Booleano]
DECLARE @NotaFact [dbo].[T_Booleano]
DECLARE @TextoFactura varchar(255)
DECLARE @TextoAlbaran varchar(255)
DECLARE @ImpFactura [dbo].[T_Id_Listado]
DECLARE @ImpAlbaran [dbo].[T_Id_Listado]
DECLARE @ImpPedido [dbo].[T_Id_Listado]
DECLARE @ImpOferta [dbo].[T_Id_Listado]
DECLARE @CodigoEAN [dbo].[T_Codigo_EAN13]
DECLARE @IdAgente int
DECLARE @IdRuta int
DECLARE @IdIdioma int
DECLARE @DescripDelOrigen nvarchar(500)
DECLARE @DescripDelEstado nvarchar(500)
DECLARE @IdEstadoComercial int
DECLARE @CreadoPorId [dbo].[T_Id_Empleado]
DECLARE @FechaModificacion smalldatetime
DECLARE @ModificadoPorId [dbo].[T_Id_Empleado]
DECLARE @Departamento nvarchar(500)
DECLARE @ImporteOportunidad decimal(18,2)
DECLARE @NombreContacto nvarchar(500)
DECLARE @CargoContacto nvarchar(100)
DECLARE @TelMovilContacto nvarchar(50)
DECLARE @Certificado varchar(250)
DECLARE @Tipo_EFactura smallint
DECLARE @Tipo_Envio_EFactura smallint
DECLARE @FechaCreacion smalldatetime
DECLARE @IdTipoRelacionContacto int
DECLARE @UltimaActuacion smalldatetime
DECLARE @IdInforme_FacturaE int
DECLARE @EmailContacto [dbo].[T_E_Mail]
DECLARE @Factura_Impresa [dbo].[T_Booleano]
DECLARE @B2BAdjuntarFactura bit
DECLARE @B2BAdjuntarGesDocu bit

-- TODO: Set parameter values here.

EXECUTE @RC = [dbo].[PClientes_Datos_Comerciales_U] 
   @IdCliente_A
  ,@Usuario OUTPUT
  ,@FechaInsertUpdate OUTPUT
  ,@IdCliente OUTPUT
  ,@Sector OUTPUT
  ,@CapitalSocial OUTPUT
  ,@Facturacion OUTPUT
  ,@Empleados OUTPUT
  ,@Origen OUTPUT
  ,@ObservacionesCom OUTPUT
  ,@Clasificacion OUTPUT
  ,@Mailing OUTPUT
  ,@DiasTransporte OUTPUT
  ,@FechaEntrega OUTPUT
  ,@IdEmpleado OUTPUT
  ,@Zona OUTPUT
  ,@Atencion OUTPUT
  ,@DirEntrega OUTPUT
  ,@NotaAlb OUTPUT
  ,@NotaFact OUTPUT
  ,@TextoFactura OUTPUT
  ,@TextoAlbaran OUTPUT
  ,@ImpFactura OUTPUT
  ,@ImpAlbaran OUTPUT
  ,@ImpPedido OUTPUT
  ,@ImpOferta OUTPUT
  ,@CodigoEAN OUTPUT
  ,@IdAgente OUTPUT
  ,@IdRuta OUTPUT
  ,@IdIdioma OUTPUT
  ,@DescripDelOrigen OUTPUT
  ,@DescripDelEstado OUTPUT
  ,@IdEstadoComercial OUTPUT
  ,@CreadoPorId OUTPUT
  ,@FechaModificacion OUTPUT
  ,@ModificadoPorId OUTPUT
  ,@Departamento OUTPUT
  ,@ImporteOportunidad OUTPUT
  ,@NombreContacto OUTPUT
  ,@CargoContacto OUTPUT
  ,@TelMovilContacto OUTPUT
  ,@Certificado OUTPUT
  ,@Tipo_EFactura OUTPUT
  ,@Tipo_Envio_EFactura OUTPUT
  ,@FechaCreacion OUTPUT
  ,@IdTipoRelacionContacto OUTPUT
  ,@UltimaActuacion OUTPUT
  ,@IdInforme_FacturaE OUTPUT
  ,@EmailContacto OUTPUT
  ,@Factura_Impresa OUTPUT
  ,@B2BAdjuntarFactura OUTPUT
  ,@B2BAdjuntarGesDocu OUTPUT
Return @RC
```

## Pers_PFormasPago_I

### SQL

```SQL
alter procedure Pers_PFormasPago_I
@IdFormaPagoIN int,
@DescripIN varchar(250)
as
DECLARE @RC int
DECLARE @IdFormaPago [dbo].[T_Forma_Pago]
DECLARE @CodFPago [dbo].[T_Forma_Pago]
DECLARE @Descrip varchar(250)
DECLARE @Fecha [dbo].[T_Fecha_Corta]
DECLARE @FechaBaja [dbo].[T_Fecha_Corta]
DECLARE @Activa [dbo].[T_Booleano]
DECLARE @DiasComerciales [dbo].[T_Booleano]
DECLARE @Variable [dbo].[T_Booleano]
DECLARE @IdFPagoPadre [dbo].[T_Forma_Pago]
DECLARE @DiasMaxPago [dbo].[T_Cantidad]
DECLARE @IdDoc [dbo].[T_Id_Doc]
DECLARE @Usuario [dbo].[T_CEESI_Usuario]
DECLARE @FechaInsertUpdate [dbo].[T_CEESI_Fecha_Sistema]

Set @IdFormaPago = isnull(@IdFormaPagoIN, (select isnull(Max(IdFormaPago),0)+1 from FormasPago))
Set @CodFPago = @IdFormaPago
Set @Descrip = @DescripIN
Set @Fecha = GETDATE()
Set @Activa = 1
Set @DiasComerciales = 0
Set @Variable = 0
Set @DiasMaxPago = 0

EXECUTE @RC = [dbo].[PFormasPago_I] 
   @IdFormaPago OUTPUT
  ,@CodFPago OUTPUT
  ,@Descrip OUTPUT
  ,@Fecha OUTPUT
  ,@FechaBaja OUTPUT
  ,@Activa OUTPUT
  ,@DiasComerciales OUTPUT
  ,@Variable OUTPUT
  ,@IdFPagoPadre OUTPUT
  ,@DiasMaxPago OUTPUT
  ,@IdDoc OUTPUT
  ,@Usuario OUTPUT
  ,@FechaInsertUpdate OUTPUT

return @RC
```

## Pers_PFormasPago_Lineas_I

### SQL

```SQL
alter procedure Pers_PFormasPago_Lineas_I
@IdFormaPagoIN [dbo].[T_Forma_Pago],
@IdTipoEfectoIN [dbo].[T_Id_TipoEfecto],
@PrimerVencimientoIN smallint,
@PorcentajeIN float = null,
@NumVencimientosIN smallint = null
as
DECLARE @RC int
DECLARE @IdFormaPago [dbo].[T_Forma_Pago]
DECLARE @IdLinea [dbo].[T_Id_Linea]
DECLARE @IdTipoEfecto [dbo].[T_Id_TipoEfecto]
DECLARE @Porcentaje float
DECLARE @Descrip varchar(50)
DECLARE @PrimerVencimiento smallint
DECLARE @NumVencimientos smallint
DECLARE @Periodo smallint
DECLARE @IdDoc [dbo].[T_Id_Doc]
DECLARE @Usuario [dbo].[T_CEESI_Usuario]
DECLARE @FechaInsertUpdate [dbo].[T_CEESI_Fecha_Sistema]

Set @IdFormaPago = @IdFormaPagoIN
Set @IdTipoEfecto = @IdTipoEfectoIN
Set @PrimerVencimiento = @PrimerVencimientoIN
Set @Descrip = (Select Descrip from Efectos_Tipos where Tipo = @IdTipoEfecto)
Set @IdLinea = (Select isnull(Max(@IdLinea),0)+1 from FormasPago_Lineas where IdFormaPago = @IdFormaPago)
Set @Porcentaje = isnull(@PorcentajeIN, 100)
Set @NumVencimientos = isnull(@NumVencimientosIN, 1)
Set @Periodo = 0

EXECUTE @RC = [dbo].[PFormasPago_Lineas_I] 
   @IdFormaPago OUTPUT
  ,@IdLinea OUTPUT
  ,@IdTipoEfecto OUTPUT
  ,@Porcentaje OUTPUT
  ,@Descrip OUTPUT
  ,@PrimerVencimiento OUTPUT
  ,@NumVencimientos OUTPUT
  ,@Periodo OUTPUT
  ,@IdDoc OUTPUT
  ,@Usuario OUTPUT
  ,@FechaInsertUpdate OUTPUT
return @RC
```
