## Banco A

``` SQL
alter session set "_ORACLE_SCRIPT"=true;

-- USER SQL
CREATE USER "A" IDENTIFIED BY "A"  
DEFAULT TABLESPACE "TS_SID2"
TEMPORARY TABLESPACE "TEMP";

-- QUOTAS
ALTER USER "A" QUOTA UNLIMITED ON "TS_SID2";

-- ROLES
GRANT "CONNECT" TO "A" ;
GRANT "RESOURCE" TO "A" ;

-- SYSTEM PRIVILEGES
GRANT CREATE ANY TABLE TO "A" ;
```

## Banco B

``` SQL
alter session set "_ORACLE_SCRIPT"=true;

-- USER SQL
CREATE USER "B" IDENTIFIED BY "B"  
DEFAULT TABLESPACE "TS_SID2"
TEMPORARY TABLESPACE "TEMP";

-- QUOTAS
ALTER USER "B" QUOTA UNLIMITED ON "TS_SID2";

-- ROLES
GRANT "CONNECT" TO "B" ;
GRANT "RESOURCE" TO "B" ;

-- SYSTEM PRIVILEGES
GRANT CREATE ANY TABLE TO "B" ;
```

**Siguiente sección:** [[Configuración del ambiente de trabajo]]
