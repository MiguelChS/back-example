# Back Example

Servidor básico con Gin que expone un endpoint `/health` con HTTPS automático usando nginx-proxy + Let's Encrypt.

## 🚀 Ejecución con Docker (Recomendado)

### Prerequisitos

- Docker y Docker Compose instalados
- Un dominio configurado apuntando a tu servidor
- Puertos 80 y 443 disponibles

### Configuración

1. **Crea un archivo `.env`** en la raíz del proyecto con tu configuración:

```bash
VIRTUAL_HOST=orca-explorer-2021.duckdns.org
LETSENCRYPT_EMAIL=tu-email@example.com
```

2. **Inicia los servicios:**

```bash
docker-compose up -d
```

Esto iniciará:
- **nginx-proxy**: Reverse proxy que maneja HTTPS
- **letsencrypt**: Genera y renueva certificados SSL automáticamente
- **back-example**: Tu aplicación Go

3. **Verifica que todo esté funcionando:**

```bash
# Ver logs
docker-compose logs -f

# Verificar que los contenedores estén corriendo
docker-compose ps
```

4. **Accede a tu aplicación:**

Tu aplicación estará disponible en `https://orca-explorer-2021.duckdns.org/health`

**Nota:** La primera vez puede tardar unos minutos mientras Let's Encrypt genera los certificados SSL.

## 📦 Ejecución Local (sin Docker)

### Instalación

```bash
go mod download
```

### Ejecución

```bash
go run main.go
```

El servidor estará disponible en `http://localhost:8080`

## 🔧 Comandos Útiles

```bash
# Detener todos los servicios
docker-compose down

# Reconstruir la imagen de la app
docker-compose build back-example

# Ver logs de un servicio específico
docker-compose logs -f back-example

# Reiniciar un servicio
docker-compose restart back-example

# Ver logs de Let's Encrypt (para debug)
docker-compose logs -f letsencrypt
```

## 📡 Endpoints

### GET /health

Retorna el estado del servidor.

**Respuesta:**
```json
{
  "status": "ok"
}
```

**Ejemplo de uso:**
```bash
curl https://orca-explorer-2021.duckdns.org/health
```

## 🏗️ Estructura del Proyecto

```
.
├── main.go              # Aplicación Go con Gin
├── Dockerfile           # Imagen Docker de la aplicación
├── docker-compose.yml   # Orquestación de servicios
├── go.mod              # Dependencias Go
├── .env                # Variables de entorno (no versionado)
└── README.md           # Este archivo
```

## 🔒 Seguridad

- Los certificados SSL se generan y renuevan automáticamente
- La aplicación Go solo escucha en HTTP internamente (puerto 8080)
- Nginx maneja toda la terminación SSL
- Los certificados se almacenan en un volumen Docker persistente

## ⚠️ Notas Importantes

- Asegúrate de que tu dominio apunte correctamente a la IP de tu servidor (54.174.42.25)
- Los puertos 80 y 443 deben estar abiertos en tu firewall/security group
- La primera generación de certificados puede tardar 2-5 minutos
- Si tienes problemas, revisa los logs: `docker-compose logs letsencrypt`

