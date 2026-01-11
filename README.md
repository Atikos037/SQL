# SQL

Praktische notities voor SQL (sectie 1–7). Basis queries en eenvoudige database-aanpassingen in phpMyAdmin.

## Inhoud
- Voorwaarden
- Query: controle data
- Query: totaal in dienst
- Query: overzicht in dienst
- Query: uit dienst in 2022
- Table: adstatus aanmaken
- Alter: employees uitbreiden
- Alter: foreign key relatie
- Insert: statuswaarden toevoegen
- Update: medewerker status zetten
- Delete-test: foreign key controle

## Voorwaarden
- MySQL / MariaDB
- phpMyAdmin
- Tabellen: employees, departments
- Kolommen gebruikt: firstname, lastname, adres, city, zipcode, email, outservice_date, department_id

## Query: controle data
Controle of de tabel bestaat en data bevat.

```sql
SELECT *
FROM employees
LIMIT 5;
```

In dienst = outservice_date is leeg (NULL).

```sql
SELECT COUNT(*) AS totaal_indienst
FROM employees
WHERE outservice_date IS NULL;
```

Vaste volgorde velden. Username wordt opgebouwd. Department komt uit departments.

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
```
Opmerking:

Password is placeholder (geen echte wachtwoorden)

Filter op datum en sorteer van oud naar nieuw.

```sql
SELECT
  firstname,
  lastname,
  CONCAT(firstname, lastname) AS username
FROM employees
WHERE outservice_date BETWEEN '2022-01-01' AND '2022-12-31'
ORDER BY outservice_date ASC;
```

Nieuwe tabel voor statuswaarden.

```sql
CREATE TABLE adstatus (
  status_id INT NOT NULL PRIMARY KEY,
  status_omschrijving VARCHAR(100) NOT NULL
);
```

Kolom toevoegen aan employees.

```sql
ALTER TABLE employees
ADD adstatus_id INT;
```

Relatie leggen tussen employees.adstatus_id en adstatus.status_id.

```sql
ALTER TABLE employees
ADD CONSTRAINT fk_employees_adstatus
FOREIGN KEY (adstatus_id)
REFERENCES adstatus(status_id);
```

Vier statuswaarden invoegen.

```sql
INSERT INTO adstatus (status_id, status_omschrijving) VALUES
(1, 'In de Active Directory en user is actief'),
(2, 'In de Active Directory en user niet actief'),
(3, 'Niet in de Active Directory'),
(0, 'Onbekend');
```

Voorbeeld: employee_id 145 krijgt status 3.

```sql
UPDATE employees
SET adstatus_id = 3
WHERE employee_id = 145;
```

Test of verwijderen geblokkeerd wordt door de relatie.

```sql
DELETE FROM adstatus
WHERE status_id = 3;
```
Verwachte uitkomst:

delete wordt geweigerd door foreign key constraint
