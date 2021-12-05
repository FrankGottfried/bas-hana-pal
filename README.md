# bas-hana-pal

Create a user defined service to enable cross container access
PAL_SCHEMA_ACCESS
````
{
    "password": "Abcd123!",
    "schema": "PAL",
    "tags": [
        "hana"
    ],
    "user": "DATA_GRANTOR"
}

```

Create the DATA_GRANTOR User and grant the necessary rights:
```

-- as DBADMIN user 
CREATE USER DATA_GRANTOR PASSWORD "Abcd123!" NO FORCE_FIRST_PASSWORD_CHANGE SET USERGROUP DEFAULT;
CREATE ROLE "data::external_access_g";
CREATE ROLE "data::external_access"; 
GRANT "data::external_access_g", "data::external_access" TO DATA_GRANTOR WITH ADMIN OPTION; 
GRANT AFL__SYS_AFL_AFLPAL_EXECUTE_WITH_GRANT_OPTION, AFL__SYS_AFL_AFLPAL_EXECUTE_WITH_GRANT_OPTION to "data::external_access_g"; 
GRANT AFL__SYS_AFL_AFLPAL_EXECUTE, AFL__SYS_AFL_AFLPAL_EXECUTE_WITH_GRANT_OPTION to "data::external_access" ;

-- as PAL user connect PAL password "Abcd123!"; 
GRANT SELECT, SELECT METADATA ON SCHEMA PAL TO "data::external_access_g" WITH GRANT OPTION; 
GRANT SELECT, SELECT METADATA ON SCHEMA PAL TO "data::external_access";

```
