# 📊 Modelo de Datos — Torneo Deportivo Internacional (Versión Alternativa)

Este documento explica la estructura del modelo de datos diseñado para la versión alternativa del torneo deportivo.

## 🎯 Objetivo del modelo

Este modelo se centra en **la gestión de eventos, sedes, resultados individuales y personal técnico**, proporcionando una vista más organizativa y de rendimiento individual.

---

## 🗂️ Colecciones Definidas

### 1. `sedes`
Contiene información de las ciudades anfitrionas del torneo.

**Campos:**
- `_id`: Identificador único (`ObjectId`)
- `ciudad`: Nombre de la ciudad anfitriona (`string`)
- `pais`: País donde se encuentra la ciudad (`string`)
- `capacidad`: Capacidad del estadio o sede (`int`)
- `infraestructura`: Nivel de infraestructura disponible (`string` - Alta, Media, Baja)
- `clima`: Clima predominante en la ciudad (`string` - Frío, Templado, Cálido)

**Tipo de datos:** texto, numérico, categórico

---

### 2. `eventos`
Eventos deportivos individuales que componen el torneo.

**Campos:**
- `_id`: Identificador único (`ObjectId`)
- `nombre_evento`: Nombre del evento (`string`)
- `disciplina`: Disciplina deportiva (`string`)
- `fecha`: Fecha del evento (`date`)
- `sede_id`: Referencia a la sede donde se realiza (`ObjectId`)

**Relación:** cada evento pertenece a una sede

**Tipo de datos:** texto, fecha, referencia

---

### 3. `atletas`
Representa a los participantes del torneo.

**Campos:**
- `_id`: Identificador único (`ObjectId`)
- `nombre`: Nombre completo del atleta (`string`)
- `nacionalidad`: Nacionalidad del atleta (`string`)
- `edad`: Edad del atleta (`int`)
- `genero`: Género del atleta (`string` - M/F)
- `participaciones`: Lista de eventos en los que ha participado (`array de subdocumentos`)
  - `evento_id`: ID del evento (`ObjectId`)
  - `resultado_previo`: Estado previo a la competición (`string` - Clasificado, Descalificado, Pendiente)

**Relación:** un atleta puede participar en múltiples eventos

**Tipo de datos:** texto, numérico, array, referencia

---

### 4. `medallas`
Registra los resultados destacados de los atletas.

**Campos:**
- `_id`: Identificador único (`ObjectId`)
- `atleta_id`: Referencia al atleta que ganó la medalla (`ObjectId`)
- `evento_id`: Referencia al evento donde se otorgó (`ObjectId`)
- `tipo`: Tipo de medalla (`string` - Oro, Plata, Bronce)
- `marca`: Marca obtenida (tiempo o puntaje) (`float`)

**Relación:** cada medalla pertenece a un atleta y a un evento

**Tipo de datos:** referencia, texto, numérico

---

### 5. `staff`
Contiene información del personal organizador y técnico.

**Campos:**
- `_id`: Identificador único (`ObjectId`)
- `nombre`: Nombre del miembro del staff (`string`)
- `rol`: Función que desempeña en el torneo (`string` - Médico, Entrenador, Juez...)
- `experiencia_anios`: Años de experiencia en su rol (`int`)
- `asignado_a`: ID de la sede a la que está asignado (`ObjectId`)

**Relación:** cada miembro del staff está vinculado a una sede

**Tipo de datos:** texto, numérico, referencia

---

## 🔗 Relaciones entre colecciones

- `eventos` → `sedes`: un evento se realiza en una sede
- `atletas` → `eventos`: un atleta puede participar en muchos eventos
- `medallas` → `eventos` y `atletas`: registro de premiaciones
- `staff` → `sedes`: staff técnico y organizativo por sede

---

## ✅ Justificación del modelo

Este modelo permite analizar:
- Distribución de sedes por país y capacidad.
- Participación de atletas en múltiples disciplinas.
- Medallería por tipo, marca y disciplina.
- Logística del torneo a través del personal y sedes.

Se favorece la **flexibilidad y consultas analíticas**, especialmente para evaluación individual y planificación organizativa.

