# [..]\Migracio Ahora\README

Abans de executar el programa s'ha de verificar que tinguem la vista [ppn_clients] a prigest i l'index [knif_clie] a la taula clients.

Un cop tinguem la vista preparada, el que farem sera crear els Stored Procedures personalitzats de Ahora, son els seguents:

- [Pers_PClientes_Datos_I]
- [Pers_PClientes_Datos_Economicos_U]
- [Pers_PClientes_Datos_Comerciales_U]
- [Pers_PFormasPago_I]
- [Pers_PFormasPago_Lineas_I]

L'script es de alter, s'ha de modificar per a poder fer el create.

Finalment crearem les Formes de pagament de ahora amb l'script seguent: [CrearFormesPagamentAhora].

Un cop tenim tots els stored i vistes creades, arrancarem el programa i li donarem a crear series, ja que son una dependencia.

Ara si, ja podem utilitzar el programa per a importar els clients i assigna'ls-hi les dades de economicos.

[ppn_clients]: MySQL#ppn_clients
[knif_clie]: MySQL#knif_clie
[Pers_PClientes_Datos_I]: StoredProcedures#sql
[Pers_PClientes_Datos_Economicos_U]: StoredProcedures#sql-1
[Pers_PClientes_Datos_Comerciales_U]: StoredProcedures#sql-2
[Pers_PFormasPago_I]: StoredProcedures#sql-3
[Pers_PFormasPago_Lineas_I]: StoredProcedures#sql-4
[CrearFormesPagamentAhora]: FormesPagamentAhora#sql-per-a-crear-totes-les-formes-de-pagament
[..]: ..
