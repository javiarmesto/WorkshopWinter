# 📊 Datos de Ejemplo - Eventbrite Export

## Archivo: eventbrite-export.xlsx

**Origen**: Export real de Eventbrite  
**Evento**: Business Central & Agents Winter Fest  
**Fecha del evento**: 17 de Enero 2026  
**Ubicación**: AZZ Valencia Congress Hotel & SPA  
**Registros**: 36 pedidos  

---

## Estructura del Excel (34 columnas)

### Información del Pedido
| Columna | Tipo | Ejemplo |
|---------|------|---------|
| Order ID | Text | 13793568833 |
| Order date | DateTime | 2025-11-23 13:36:54 |

### Información del Comprador
| Columna | Tipo | Ejemplo |
|---------|------|---------|
| Buyer first name | Text | Juan |
| Buyer last name | Text | García |
| Buyer email | Text | juan@empresa.com |
| Phone number | Text | (vacío en este export) |

### Ubicación del Comprador
| Columna | Tipo | Ejemplo |
|---------|------|---------|
| Purchaser city | Text | Valencia |
| Purchaser state | Text | V |
| Purchaser country | Text | ES |
| Billing zip code | Text | (vacío) |
| Billing country | Text | (vacío) |

### Información del Evento
| Columna | Tipo | Ejemplo |
|---------|------|---------|
| Event name | Text | Business Central & Agents Winter Fest |
| Event ID | Number | 1975105057395 |
| Event start date | Date | 2026-01-17 |
| Event start time | Time | 09:30:00 |
| Event timezone | Text | Europe/Madrid |
| Event location | Text | AZZ Valencia Congress Hotel & SPA |

### Tickets
| Columna | Tipo | Ejemplo |
|---------|------|---------|
| Ticket quantity | Integer | 1 |
| Add-ons quantity | Integer | 0 |

### Información Financiera
| Columna | Tipo | Ejemplo |
|---------|------|---------|
| Currency | Text | EUR |
| Payment status | Text | Free Order |
| Payment type | Text | Free |
| Payment details | Text | (vacío) |
| Gross sales | Decimal | 0 |
| Eventbrite service fee | Decimal | 0 |
| Eventbrite payment processing fee | Decimal | 0 |
| Eventbrite tax | Decimal | 0 |
| Organizer tax | Decimal | 0 |
| Royalty | Decimal | 0 |
| Ticket revenue | Decimal | 0 |
| Add-ons revenue | Decimal | 0 |
| Ticket + add-ons revenue | Decimal | 0 |
| Net sales | Decimal | 0 |

### Otros
| Columna | Tipo | Ejemplo |
|---------|------|---------|
| Guest | Text | No |

---

## Estadísticas del Dataset

- **Total registros**: 36
- **Países**: ES (España), AD (Andorra)
- **Ciudades principales**: Valencia, Madrid, Barcelona, Gandía, Castellón...
- **Tipo de evento**: Gratuito (Free Order)
- **Tickets por pedido**: 1-3 (mayoría 1)

---

## Uso en el Workshop

Este archivo se usa para:
1. Probar la funcionalidad de importación Excel
2. Validar el mapeo de columnas
3. Verificar la creación automática de eventos
4. Probar las APIs de consulta

---

## Notas

- Los datos personales han sido anonimizados para el workshop
- El formato es el estándar de exportación de Eventbrite
- Todas las columnas deben estar presentes aunque algunas estén vacías
