# SQL Cheatsheet – Databasebeheer & Queries (Sectie 1–7)

Praktische SQL-cheatsheet voor basis databasebeheer en query’s.
Geschikt als naslag voor SELECT-, JOIN-, CREATE-, ALTER- en UPDATE-opdrachten
zoals behandeld in **sectie 1 t/m 7**.

---

## Inhoud
- [Voorwaarden](#voorwaarden)
- [Snelle datacontrole](#snelle-datacontrole)
- [Select queries](#select-queries)
- [Filtering & sortering](#filtering--sortering)
- [Joins](#joins)
- [Tabellen aanmaken](#tabellen-aanmaken)
- [Tabellen aanpassen](#tabellen-aanpassen)
- [Relaties (foreign keys)](#relaties-foreign-keys)
- [Data invoegen en wijzigen](#data-invoegen-en-wijzigen)
- [Integriteit testen](#integriteit-testen)

---

## Voorwaarden
- MySQL / MariaDB database
- Toegang via phpMyAdmin of MySQL CLI
- Tabellen: `employees`, `departments`
- Basiskennis SQL (SELECT, WHERE, JOIN)

---

## Snelle datacontrole

Gebruik deze query om te controleren of de tabel data bevat.

```sql
SELECT *
FROM employees
LIMIT 5;
pgsql
Code kopiëren

```markdown
## Query 1 – Totaal aantal medewerkers in dienst

Medewerkers die nog in dienst zijn hebben geen `outservice_date`.

```sql
SELECT COUNT(*) AS totaal_indienst
FROM employees
WHERE outservice_date IS NULL;
pgsql
Code kopiëren

```markdown
## Query 2 – Overzicht medewerkers in dienst

Velden worden opgebouwd in de query zelf.

```sql
SELECT
  e.firstname,
  e.lastname,
  e.adres,
  e.city,
  e.zipcode,
  e.email,
  CONCAT(e.firstname, e.lastname) AS username,
  d.department_name AS department,
  '********' AS password
FROM employees e
JOIN departments d
  ON e.department_id = d.department_id
WHERE e.outservice_date IS NULL;
pgsql
Code kopiëren

```markdown
## Query 3 – Medewerkers uit dienst in 2022

Resultaat gesorteerd van oud naar nieuw.

```sql
SELECT
  firstname,
  lastname,
  CONCAT(firstname, lastname) AS username
FROM employees
WHERE outservice_date BETWEEN '2022-01-01' AND '2022-12-31'
ORDER BY outservice_date ASC;
