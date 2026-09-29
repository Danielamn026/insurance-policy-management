# Insurance Policy Management System

A comprehensive database project for managing insurance policies, policyholders, and coverage types across Colombian departments.

## 📋 Project Overview

This project implements a relational database system designed to manage insurance policies with the following key features:

- **Policy Management**: Track insurance policies with policy numbers, insurable values, and validity dates
- **Policyholder Registration**: Manage policyholder information including personal details and geographical data
- **Coverage Types**: Define different coverage options with customizable coverage percentages
- **Department Distribution**: Organize policyholders by Colombian departments with risk percentages

## 🗄️ Database Schema

### Tables

#### `departamento` (Department)
Stores department information with risk assessment percentages.

| Column | Type | Constraints |
|--------|------|-------------|
| id | NUMBER(10) | Primary Key, Auto-generated |
| nombre | VARCHAR(25) | Unique, Not Null |
| porcentaje | NUMBER | Not Null, Check (0-100) |

#### `tomador` (Policyholder)
Contains policyholder personal and geographical information.

| Column | Type | Constraints |
|--------|------|-------------|
| id | NUMBER(10) | Primary Key, Auto-generated |
| cedula | VARCHAR(10) | Unique, Not Null |
| nombre | VARCHAR(20) | Not Null |
| genero | VARCHAR(9) | Check (Masculino/Femenino), Not Null |
| edad | NUMBER(3) | Check (18-100), Not Null |
| iddepartamento | NUMBER(10) | Foreign Key → departamento |

#### `tipo_cubrimiento` (Coverage Type)
Defines different insurance coverage options.

| Column | Type | Constraints |
|--------|------|-------------|
| id | NUMBER(10) | Primary Key, Auto-generated |
| nombre | VARCHAR(20) | Unique, Not Null |
| porcentaje | NUMBER | Not Null, Check (0-100) |

#### `poliza` (Insurance Policy)
Stores policy details including coverage and policyholder information.

| Column | Type | Constraints |
|--------|------|-------------|
| id | NUMBER(10) | Primary Key, Auto-generated |
| numero | VARCHAR(10) | Unique, Not Null |
| valorasegurable | NUMBER(10) | Default 0, Not Null, ≥ 0 |
| fechainicio | DATE | Not Null |
| fechafin | DATE | Not Null |
| idtipocubrimiento | NUMBER(10) | Foreign Key → tipo_cubrimiento |
| idtomador | NUMBER(10) | Foreign Key → tomador |

## 📁 Files

- **CrearTablasProyecto.sql** - Database schema and table creation statements
- **InsertarInfo.sql** - Initial data insertion including:
  - 32 Colombian departments with risk percentages
  - 20 sample policyholders
  - 4 coverage types (Oferta, Equipos, Servicio, Contrato)
  - 20 sample insurance policies

- **Consultas_Punto1** - Query examples and use cases

## 🚀 Getting Started

### Prerequisites
- Oracle Database or compatible SQL engine
- SQL client or development environment

### Installation

1. Execute `CrearTablasProyecto.sql` to create the database schema
2. Execute `InsertarInfo.sql` to populate initial data

```sql
-- Create tables
@CrearTablasProyecto.sql

-- Insert sample data
@InsertarInfo.sql
```

## 💡 Key Features

✅ **Relational Integrity** - Foreign key constraints ensure data consistency  
✅ **Data Validation** - Check constraints for valid percentage ranges and age limits  
✅ **Unique Constraints** - Prevent duplicate records for critical fields  
✅ **Auto-generated IDs** - Automatic primary key generation for all tables  
✅ **Sample Data** - Pre-populated with realistic insurance data

## 📊 Example Queries

### Get all policies by coverage type
```sql
SELECT p.numero, t.nombre, p.valorasegurable
FROM poliza p
JOIN tipo_cubrimiento t ON p.idtipocubrimiento = t.id
ORDER BY t.nombre;
```

### Find policyholders by department
```sql
SELECT t.nombre, t.cedula, d.nombre as departamento
FROM tomador t
JOIN departamento d ON t.iddepartamento = d.id
WHERE d.nombre = 'Antioquia'
ORDER BY t.nombre;
```

### Calculate total policy value by department
```sql
SELECT d.nombre, COUNT(p.id) as total_policies, SUM(p.valorasegurable) as total_value
FROM poliza p
JOIN tomador t ON p.idtomador = t.id
JOIN departamento d ON t.iddepartamento = d.id
GROUP BY d.nombre
ORDER BY total_value DESC;
```

## 📝 Project Structure

```
Proyecto_1BasesDeDatos/
├── README.md                      # This file
├── CrearTablasProyecto.sql       # Schema definition
├── InsertarInfo.sql              # Sample data
└── Consultas_Punto1              # Query examples
```

## 👨‍💼 Author

**Danielamn026** - Database design and implementation

## 📄 License

This project is available as-is for educational and development purposes.

---

**Last Updated**: March 2024
