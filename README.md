# Back Example

Servidor básico con Gin que expone un endpoint `/health`.

## Instalación

```bash
go mod download
```

## Ejecución

```bash
go run main.go
```

El servidor estará disponible en `http://localhost:8080`

## Endpoints

### GET /health

Retorna el estado del servidor.

**Respuesta:**
```json
{
  "status": "ok"
}
```

