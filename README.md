# CursoNet_BD2 — Plataforma de Cursos (Neo4j)

[![Neo4j](https://img.shields.io/badge/neo4j-Graph%20Database-blue)](https://neo4j.com/)
[![Cypher](https://img.shields.io/badge/query-Cypher-informational)](https://neo4j.com/developer/cypher/)
[![CSV](https://img.shields.io/badge/data-CSV-yellow)]()

---

## 📌 Descripción

**CursoNet_BD2** es una base de datos de **grafos en Neo4j** que modela una plataforma de cursos tipo Coursera/edX. Permite:
- Representar **Usuarios**, **Cursos**, **Módulos**, **Lecciones**, **Categorías**, **Idiomas** y **Países**.
- Registrar relaciones de **inscripción**, **visualización de lecciones**, **me gusta/valoración**, **composición de cursos** y **disponibilidad por país**.
- Ejecutar consultas analíticas y **recomendaciones** simples (popularidad local, afinidad por categorías y usuarios similares).

---

## 🧰 Requisitos

- Neo4j Desktop 5.x (o Neo4j Aura DB)  
- Neo4j Browser (incluido en Desktop/Aura)  
- Git (opcional, si vas a clonar el repo)  
- Editor de texto para revisar/editar CSV

---

## 🧩 Modelo (resumen)

**Nodos**
- `Usuario(usuarioId, nombre, edad, sexo, email, fechaNacimiento)`
- `Curso(cursoId, titulo, descripcion, nivel, anioPublicacion, calificacionPromedio)`
- `Modulo(moduloId, titulo, numero)`
- `Leccion(leccionId, titulo, descripcion, duracionMin, numero)`
- `Categoria(codigo, nombre, descripcion)`
- `Idioma(codigo, nombre)`
- `Pais(codigo, nombre)`

**Relaciones y propiedades**
- `(:Usuario)-[:VIO {fecha, minutosReproducidos, finalizado}]->(:Leccion)`
- `(:Usuario)-[:LE_GUSTA {valoracion, fecha}]->(:Curso)`
- `(:Usuario)-[:INSCRITO_EN {fechaInicio, fechaFin, progreso, estado}]->(:Curso)`
- `(:Curso)-[:DISPONIBLE_EN {fechaInicio, fechaFin}]->(:Pais)`
- `(:Curso)-[:ESTA_COMPUESTO {orden}]->(:Modulo)`
- `(:Modulo)-[:CONTIENE {orden}]->(:Leccion)`
- `(:Curso)-[:PERTENECE_A]->(:Categoria)`
- `(:Curso)-[:EN_IDIOMA]->(:Idioma)`

---
## 📥 CSV esperados

**Nodos**
- `usuarios.csv` → `usuarioId;nombre;edad;sexo;email;fechaNacimiento`
- `cursos.csv` → `cursoId;titulo;descripcion;nivel;anioPublicacion;calificacionPromedio`
- `modulos.csv` → `moduloId;titulo;numero`
- `lecciones.csv` → `leccionId;titulo;descripcion;duracionMin;numero`
- `categorias.csv` → `codigo;nombre;descripcion`
- `idiomas.csv` → `codigo;nombre`
- `paises.csv` → `codigo;nombre`

**Relaciones**
- `curso_categoria.csv` → `cursoId;categoriaCodigo`
- `curso_idioma.csv` → `cursoId;idiomaCodigo`
- `curso_pais.csv` → `cursoId;paisCodigo;fechaInicio;fechaFin`
- `curso_modulo.csv` → `cursoId;moduloId;orden`
- `modulo_leccion.csv` → `moduloId;leccionId;orden`
- `usuario_inscrito.csv` → `usuarioId;cursoId;fechaInicio;fechaFin;progreso;estado`
- `usuario_vio.csv` → `usuarioId;leccionId;fecha;minutosReproducidos;finalizado`
- `usuario_like.csv` → `usuarioId;cursoId;valoracion;fecha`

---

## 🧯 Problemas comunes

- **Orden de carga**: primero **nodos**, luego **relaciones**.  
- **Fechas**: usar `YYYY-MM-DD`; para vacíos, setear `NULL` vía `CASE`.  
- **Duplicados**: `MERGE` por ID + constraints evita registros repetidos.  
- **CSV/encoding**: guardar en **UTF‑8** con encabezados exactos.
