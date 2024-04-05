# StoredProcedures

## Index

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

### Pers_PClientes_Datos_I

#### @IdTipo

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

#### @IdPoblacion

Busca la relacio entre les poblacions de prigest i les de Ahora.

```SQL
Set @IdPoblacion = isnull((Select tp.IdPoblacioAhora From Traduccions..Poblacions tp where tp.PoblacioPrigest = @IdPoblacionIN),0)
```

#### @IdTipoOtro

Verifica que `@IdTipoOtro` tingui un valor correcte i en cas contrari, posa un valor per defecte, el `0`.

```SQL
Set @IdTipoOtro = (Select case when @IdTipoOtroIN < (select min(IdTipoOtro) from Clientes_TiposOtros) or (select max(IdTipoOtro) from Clientes_TiposOtros) < @IdTipoOtroIN then 0 else @IdTipoOtroIN end); -- Taula Clientes_TiposOtros (Normal (0), Destacat (1), VIP (2))
```

#### @Bloqueado

Defineix `@Bloqueado` i `@IdMotivoBloqueo` segons si el client esta de baixa o no.

```SQL
Set @Bloqueado = (Select case when @DataBaixaIN is null then 0 else 1 end) -- Alta (0) Baixa (1)
Set @IdMotivoBloqueo = (Select case when @DataBaixaIN is null then 0 else 1 end) -- Sense Motiu (0) Client de Baixa (1)
Set @IdDelegacionCli = (select count(*) from Clientes_Datos where Nif = @Nif)
```

#### Propietats no prigest

Aquestes propietats son obligatories, pero no les trobarem a prigest.

```SQL
-- Necessitem, no prigest
Set @TipoTransporte = 0;
Set @IdDelegacion = -1;
Set @Cliente_Contado = 0;
Set @EsUbicacion = '';
```

#### Autocalculades per Ahora

Aquestes propietats les calcula Ahora en algun trigger o stored a partir d'aquest punt

```SQL
-- dummy Ja es calculen automaticament
Set @Kmts = 0;
Set @IdContacto = 0;
Set @IdContactoA = @IdContacto;
Set @IdContactoF = @IdContacto;
Set @IdContactoG = @IdContacto;
```

#### No utilitzades per Puignau

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

#### Automatiques

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

#### Camps Configurables

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

#### SQL

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
