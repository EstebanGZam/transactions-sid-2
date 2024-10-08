### 1. Ejecución de `Ti` desde el banco A

```sql
UPDATE A.Accounts SET balance = balance + 20000 WHERE code = 1;
```

### 2. Ejecución de `Tj` desde el banco B

```sql
UPDATE A.Accounts SET balance = balance - 30000 WHERE code = 1;
```

**¿Qué ocurre con la transacción `Tj`?**
- La transacción `Tj` se quedó bloqueada debido a un **bloqueo de concurrencia**. En sistemas de bases de datos como Oracle, cuando una transacción (en este caso, `Ti` desde el banco A) está modificando una fila, esa fila queda bloqueada hasta que la transacción se confirma o se deshace (mediante `COMMIT` o `ROLLBACK`). Mientras tanto, cualquier otra transacción que intente modificar la misma fila (como `Tj` desde el banco B) quedará en espera hasta que el bloqueo se libere.

![[Pasted image 20241008070259.png|500]]

### 3. Consultar el saldo en la cuenta

```sql
SELECT balance FROM A.Accounts WHERE code = 1;
```

![[Pasted image 20241008070448.png]]
### 4. Comprometer la transacción `Ti`

```sql
COMMIT;
```

**¿Qué ocurre con `Tj` en el otro banco?**
- Cuando se hace `COMMIT` en el banco A para la transacción `Ti`, el bloqueo sobre la fila modificada (cuenta con `code = 1`) se libera. En ese momento, la transacción `Tj` en el banco B, que estaba esperando para modificar la misma fila, podrá continuar su ejecución.

### 5. Comprometer `Tj` y consultar el saldo

Ahora, desde el banco `B` se compromete la transacción `Tj`:

```sql
COMMIT; -- En Banco B
```

Se consulta el saldo en el Banco `A` nuevamente:

```sql
SELECT balance FROM A.Accounts WHERE code = 1;
```

![[Pasted image 20241008092825.png]]

**¿Qué ha pasado?**
Cuando el banco B compromete (`COMMIT`) la transacción `Tj`, se realizan los siguientes efectos:
1. **La transacción `Tj` se hace permanente**: El retiro de 30.000 de la cuenta con `code = 1` en el banco A se aplica, afectando el saldo de la cuenta en el banco A.
2. **Saldo Final en el Banco A**: Después del `COMMIT` de `Ti`, el saldo era de 120.000. Al hacer `COMMIT` en `Tj`, el nuevo saldo será de 90.000.
3. **Estado Consistente**: Ambas transacciones (`Ti` y `Tj`) se consideran completas y efectivas, manteniendo la consistencia en los saldos de ambas cuentas.
### 6. Invertir el orden de ejecución de Ti y Tj

Ahora, si se ejecuta primero `Tj` antes de `Ti`, esto sería:

```sql
UPDATE A.Accounts SET balance = balance - 30000 WHERE code = 1;
```

Y luego se ejecuta `Ti`:

```sql
UPDATE A.Accounts SET balance = balance + 20000 WHERE code = 1;
```

Al ejecutar esto en A, se vuelve a quedar bloqueado. Por lo tanto, para que termine esta ejecución, primero hay que hacer COMMIT en B y luego, para que todo quede guardado correctamente, se hace en A.

![[Pasted image 20241008093451.png]]

**¿Qué pasa con el saldo de la cuenta?**
- Si `Tj` se ejecuta primero, se retirarán 30.000 (partimos de un saldo de 90.000), lo que dejará un saldo de 60.000 (esto se verá reflejado tras hacer `COMMIT`). Después de que `Ti` se ejecute, se sumarán 20.000, resultando en un saldo final de 80.000 (tras hacer `COMMIT` de nuevo).
![[Pasted image 20241008094212.png]]
![[Pasted image 20241008094247.png]]
### 7. Restaurar los valores originales de la tabla de cuentas

Para restaurar los valores originales de la tabla `Cuentas`, se realiza una serie de `UPDATE` que reinicien el saldo a los valores iniciales Y por último, se hace `COMMIT`:

```sql
UPDATE A.Accounts SET balance = 100000 WHERE code = 1;
UPDATE A.Accounts SET balance = 150000 WHERE code = 2;
COMMIT;
```
