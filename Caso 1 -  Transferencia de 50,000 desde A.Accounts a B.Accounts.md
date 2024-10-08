## 1. Ejecutar la estrategia A
### 1.1 Verificar disponibilidad de fondos en la cuenta de origen (`A.Accounts`):

```sql
SELECT balance FROM A.Accounts WHERE code = 1;
```

Si el saldo es suficiente (>= 50,000), continuar con la transferencia.
### 1.2. Transferir el dinero a la cuenta destino (`B.Accounts`):

```sql
UPDATE B.Accounts SET balance = balance + 50000 WHERE code = 2;
```

### 1.3. Restar el dinero de la cuenta de origen (`A.Accounts`):

```sql
UPDATE A.Accounts SET balance = balance - 50000 WHERE code = 1;
```

### 1.4. Comprometer la transacción `Ti`

```sql
COMMIT;
```

### 1.5. Consultar los valores finales de las cuentas

Ejecutar los siguientes `SELECT` para verificar el saldo de cada cuenta:
- **Banco A:**
```sql
SELECT * FROM A.Accounts;
```
- **Banco B:**
```sql
SELECT * FROM B.Accounts;
```

## 2. Restaurar los valores originales de la tabla de cuentas

Para restaurar los valores originales de la tabla `Cuentas`, se realiza una serie de `UPDATE` que reinicien el saldo a los valores iniciales Y por último, se hace `COMMIT`:

```sql
-- Restaurar valores en Banco A
UPDATE A.Accounts SET balance = 100000 WHERE code = 1;
UPDATE A.Accounts SET balance = 150000 WHERE code = 2;
COMMIT;

-- Restaurar valores en Banco B
UPDATE B.Accounts SET balance = 100000 WHERE code = 1;
UPDATE B.Accounts SET balance = 150000 WHERE code = 2;
COMMIT;
```

## 3. Ejecutar la estrategia B
### 3.1. Verificar disponibilidad de fondos en la cuenta de origen (`A.Accounts`):

```sql
SELECT balance FROM A.Accounts WHERE code = 1;
```

Si el saldo es suficiente (>= 50,000), continuar con la transferencia.
### 3.2. Restar el dinero de la cuenta de origen (`A.Accounts`):

```sql
UPDATE A.Accounts SET balance = balance - 50000 WHERE code = 1;
```

### 3.3. Sumar el dinero a la cuenta destino (`B.Accounts`):

```sql
UPDATE B.Accounts SET balance = balance + 50000 WHERE code = 2;
```

### 3.4. Comprometer la transacción `Ti`

```sql
COMMIT;
```

### 3.5. Consultar los valores finales de las cuentas

Ejecutar los siguientes `SELECT` para verificar el saldo de cada cuenta:
- **Banco A:**
```sql
SELECT * FROM A.Accounts;
```
- **Banco B:**
```sql
SELECT * FROM B.Accounts;
```

## 4. Comparación de los resultados

- **Estrategia A:**
	En esta estrategia, si falla la operación de transferencia en `B.Accounts` (por ejemplo, problemas de conectividad o permisos), se podría haber restado dinero de `A.Accounts` sin sumarlo a `B.Accounts`, causando una inconsistencia.
- **Estrategia B:**
    Es más segura, porque se retira el dinero del origen solo si se puede agregar a la cuenta destino. Si la suma en `B.Accounts` falla, se puede restaurar el estado original de `A.Accounts` antes de realizar el `COMMIT`.

**Conclusión:** La estrategia B es preferible, ya que minimiza el riesgo de inconsistencias. Si hay un error en la transferencia, se puede restaurar el saldo inicial de `A.Accounts` fácilmente.
## 5. Restaurar los valores originales de la tabla de cuentas

Para restaurar los valores originales de la tabla `Cuentas`, se realiza una serie de `UPDATE` que reinicien el saldo a los valores iniciales Y por último, se hace `COMMIT`:

```sql
-- Restaurar valores en Banco A
UPDATE A.Accounts SET balance = 100000 WHERE code = 1;
UPDATE A.Accounts SET balance = 150000 WHERE code = 2;
COMMIT;

-- Restaurar valores en Banco B
UPDATE B.Accounts SET balance = 100000 WHERE code = 1;
UPDATE B.Accounts SET balance = 150000 WHERE code = 2;
COMMIT;
```