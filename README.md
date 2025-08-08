# Torneo Deportivo Internacional — Modelo Alternativo
 
---

## 🎯 Descripción del proyecto

Se propone las siguientes colecciones para el evento deportivo internacional:

- **Sedes** de los eventos (ciudades anfitrionas).
- **Eventos deportivos** específicos.
- **Atletas individuales** y su historial.
- **Medallas ganadas** en eventos.
- **Staff técnico y organizativo** asignado a sedes.

---

## 🗂️ Colecciones y estructura

### 1. `sedes`
- `ciudad`: Nombre de la ciudad anfitriona.
- `pais`: País correspondiente.
- `capacidad`: Capacidad del estadio.
- `infraestructura`: Nivel (Alta, Media, Baja).
- `clima`: Tipo de clima (Frío, Templado, Cálido).

### 2. `eventos`
- `nombre_evento`: Nombre del evento.
- `disciplina`: Disciplina del evento.
- `fecha`: Fecha del evento.
- `sede_id`: Referencia a la sede donde se realiza.

### 3. `atletas`
- `nombre`: Nombre del atleta.
- `nacionalidad`: Nacionalidad.
- `edad`: Edad.
- `genero`: Género.
- `participaciones`: Array de subdocumentos:
  - `evento_id`
  - `resultado_previo`

### 4. `medallas`
- `atleta_id`: Referencia al atleta.
- `evento_id`: Evento en el que participó.
- `tipo`: Oro, Plata o Bronce.
- `marca`: Tiempo o puntuación registrada.

### 5. `staff`
- `nombre`: Nombre del personal.
- `rol`: Función en el torneo.
- `experiencia_anios`: Años de experiencia.
- `asignado_a`: ID de sede asignada.

---

## 📥 Importación de los datos

Puedes importar los archivos usando `mongoimport`:

```bash
mongoimport --db torneo_alternativo --collection sedes --file sedes.json --jsonArray
mongoimport --db torneo_alternativo --collection eventos --file eventos.json --jsonArray
mongoimport --db torneo_alternativo --collection atletas --file atletas.json --jsonArray
mongoimport --db torneo_alternativo --collection medallas --file medallas.json --jsonArray
mongoimport --db torneo_alternativo --collection staff --file staff.json --jsonArray
```

O mediante MongoDB Compass → Add Data → Import JSON.

---

## 🔎 Consultas para mongosh

### 📘 Consultas sencillas

```js
// 1. Mostrar todos los eventos programados para después del 10 de agosto de 2025
printjson(
  db.eventos.find({
    fecha: { $gte: ISODate("2025-08-10T00:00:00Z") }
  }).toArray()
)

// 2. Listar atletas de nacionalidad "Francés"
printjson(
  db.atletas.find({
    nacionalidad: "Francés"
  }).toArray()
)

// 3. Buscar sedes que tengan infraestructura "Alta"
printjson(
  db.sedes.find({
    infraestructura: "Alta"
  }).toArray()
)

// 4. Mostrar personal técnico con más de 10 años de experiencia
printjson(
  db.staff.find({
    experiencia_anios: { $gt: 10 }
  }).toArray()
)

// 5. Ver medallas de tipo "Plata"
printjson(
  db.medallas.find({
    tipo: "Plata"
  }).toArray()
)
```

---

### 🧠 Consultas con operadores

```js
// 1. Atletas con edad menor a 20 años
printjson(
  db.atletas.find({
    edad: { $lt: 20 }
  }).toArray()
)

// 2. Medallas con marca mayor a 80
printjson(
  db.medallas.find({
    marca: { $gt: 80 }
  }).toArray()
)

// 3. Staff con rol "Juez" o "Médico"
printjson(
  db.staff.find({
    rol: { $in: ["Juez", "Médico"] }
  }).toArray()
)

// 4. Eventos realizados en ciudades con clima "Cálido" (requiere $lookup si se expande)
printjson(
  db.eventos.find().toArray() // (se evalúa luego por aplicación)
)

// 5. Atletas cuyo género no sea "M"
printjson(
  db.atletas.find({
    genero: { $ne: "M" }
  }).toArray()
)

// 6. Medallas cuyo tipo no sea "Bronce" y marca menor a 60
printjson(
  db.medallas.find({
    tipo: { $ne: "Bronce" },
    marca: { $lt: 60 }
  }).toArray()
)

// 7. Eventos de disciplina "Fútbol" o "Judo"
printjson(
  db.eventos.find({
    disciplina: { $in: ["Fútbol", "Judo"] }
  }).toArray()
)

// 8. Personal asignado a sedes con capacidad mayor a 30000 (requiere $lookup si se extiende)
printjson(
  db.staff.find().toArray() // se filtra manualmente
)

// 9. Atletas que participaron en más de un evento
printjson(
  db.atletas.find({
    "participaciones.1": { $exists: true }
  }).toArray()
)

// 10. Buscar sedes en países "Italia", "España", o "Alemania"
printjson(
  db.sedes.find({
    pais: { $in: ["Italia", "España", "Alemania"] }
  }).toArray()
)
```

---

## 📂 Archivos incluidos

- `sedes.json`  
- `eventos.json`  
- `atletas.json`  
- `medallas.json`  
- `staff.json`  
- `README.md`

---

## ✅ Conclusión

Este modelo propone una estructura diferente basada en sedes y eventos, permitiendo un análisis más logístico y de resultados individuales.  
Es ideal para estudiar rendimiento por sede, disciplina, clima o capacidad organizativa.

## Autor

- Anylorena Torres Ávila