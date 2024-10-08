## Banco A

``` SQL
-- DROP TABLE Accounts;
CREATE TABLE Accounts (
    code NUMBER PRIMARY KEY,
    balance NUMBER
);

INSERT INTO Accounts (code, balance) VALUES (1, 100000);
INSERT INTO Accounts (code, balance) VALUES (2, 150000);

GRANT SELECT, UPDATE ON A.Accounts TO B;

```

## Banco B

``` SQL
-- DROP TABLE Accounts;
CREATE TABLE Accounts (
    code NUMBER PRIMARY KEY,
    balance NUMBER
);

INSERT INTO Accounts (code, balance) VALUES (1, 100000);
INSERT INTO Accounts (code, balance) VALUES (2, 150000);

GRANT SELECT, UPDATE ON B.Accounts TO A;

```

**Siguiente sección:** [[Caso de Calentamiento]]