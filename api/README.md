# BT_ARG_DEMO — API ficticia de identificación

**Todos los datos de esta carpeta son FICTICIOS.** Son para la demo de NICE CXone de Belltech Argentina. No hay datos de personas reales.
No subas DNI, CUIL, domicilios ni números de cuenta reales: el repositorio es público.

Base URL (GitHub Pages): `https://maverick17-belltech.github.io/BT_ARG_DEMO/api/`

## Endpoints (archivos)

| Endpoint | Para qué | Si no existe |
|---|---|---|
| `renaper/{dni}.json` | Identidad con formato de respuesta RENAPER (apellido, nombres, cuil, domicilio, ejemplar, vencimiento) | 404 = DNI inexistente |
| `clientes/{dni}.json` | En qué verticales es cliente y qué productos tiene | 404 = no es cliente de ninguna |
| `luz/medidores/{nro}.json` | N° de medidor (10 dígitos) → DNI del titular (VoltaSur) | 404 = medidor inexistente |
| `luz/cuentas/{nro}.json` | N° de cuenta (10 dígitos) → DNI del titular (VoltaSur) | 404 = cuenta inexistente |
| `gas/clientes/{nro}.json` | N° de cliente (12 dígitos) → DNI del titular (EnerGas Sur) | 404 = cliente inexistente |

Cómo se identifica cada vertical:
- Banca, Seguros y Financieras: solo DNI (7 u 8 dígitos).
- Luz: DNI, N° de medidor (10) o N° de cuenta (10).
- Gas: DNI o N° de cliente (12).

Siempre se llega al DNI. Desde ahí, `renaper/` valida la identidad y `clientes/` trae los productos.

## Personas de prueba

| DNI | Persona | Caso de prueba |
|---|---|---|
| 30111222 | Lucía Fernández | Banco Andino (Premium) + EnerGas Sur (cliente 400000000101) |
| 28333444 | Martín Gómez | Seguros del Plata (auto) + VoltaSur (medidor 3270000101, cuenta 6480000101, con deuda) |
| 35555666 | Sofía Ramírez | **DNI vencido** · Crédito Pampa en mora · VoltaSur (medidor 3270000202, cuenta 6480000202) |
| 7654321 | Roberto Díaz | DNI de **7 dígitos** · Seguros hogar con siniestro abierto · EnerGas Sur (cliente 400000000202) |
| 40111222 | Valentina López | DNI válido pero **no es cliente** de ninguna vertical |
| 33777888 | Sebastián Sosa | Banco Andino (Plus) + EnerGas Sur (cliente 400000000777) |
| 99999999 | — | **Inexistente** (no tiene archivo → 404) |

## Receta: agregar una persona

1. **Identidad:** copiá `renaper/30111222.json`, renombralo con el nuevo DNI y cambiá `apellido`, `nombres`, `cuil`, el domicilio, `ejemplar`, `emision` y `vencimiento` (si ponés una fecha pasada, el DNI está vencido).
2. **Productos:** copiá `clientes/30111222.json` con el mismo DNI. En cada vertical poné `"esCliente": true` con sus datos, o `{"esCliente": false}`.
3. **Si es cliente de Luz:** creá `luz/medidores/{medidor}.json` y `luz/cuentas/{cuenta}.json` (copiá uno existente y cambiá `numero` y `dni`).
4. **Si es cliente de Gas:** creá `gas/clientes/{nroCliente}.json` (copiá uno existente y cambiá `numero` y `dni`).
5. Commit. GitHub Pages lo publica en 1 o 2 minutos. Probalo abriendo la URL en el navegador.

Reglas: el nombre del archivo es exactamente el número, sin puntos ni guiones (`7654321.json`, no `7.654.321.json`). Los números de medidor, cuenta y cliente van entre comillas en el JSON (son texto, no números).
