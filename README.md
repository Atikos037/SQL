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
Select queries
Totaal aantal medewerkers in dienst
Medewerkers die nog in dienst zijn hebben geen outservice_date.

sql
Code kopiëren
SELECT COUNT(*) AS totaal_indienst
FROM employees
WHERE outservice_date IS NULL;
Filtering & sortering
Medewerkers uit dienst in 2022
Resultaat gesorteerd van oud naar nieuw.

sql
Code kopiëren
SELECT
  firstname,
  lastname,
  CONCAT(firstname, lastname) AS username
FROM employees
WHERE outservice_date BETWEEN '2022-01-01' AND '2022-12-31'
ORDER BY outservice_date ASC;
Joins
Overzicht medewerkers in dienst met afdeling
Velden worden bewust opgebouwd in de query.

sql
Code kopiëren
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
Opmerking:

Het wachtwoordveld is een placeholder

Er wordt geen echt wachtwoord opgehaald

Tabellen aanmaken
Tabel adstatus
Deze tabel bevat mogelijke Active Directory statussen.

sql
Code kopiëren
CREATE TABLE adstatus (
  status_id INT NOT NULL PRIMARY KEY,
  status_omschrijving VARCHAR(100) NOT NULL
);
Controle:

Controleer of de tabel zichtbaar is in phpMyAdmin

Tabellen aanpassen
Kolom toevoegen aan employees
sql
Code kopiëren
ALTER TABLE employees
ADD adstatus_id INT;
Controle:

sql
Code kopiëren
DESCRIBE employees;
Relaties (foreign keys)
Relatie leggen volgens ERD
sql
Code kopiëren
ALTER TABLE employees
ADD CONSTRAINT fk_employees_adstatus
FOREIGN KEY (adstatus_id)
REFERENCES adstatus(status_id);
Doel:

Waarborgt dat alleen geldige statussen gebruikt worden

Data invoegen en wijzigen
AD-statussen toevoegen
sql
Code kopiëren
INSERT INTO adstatus (status_id, status_omschrijving) VALUES
(1, 'In de Active Directory en user is actief'),
(2, 'In de Active Directory en user niet actief'),
(3, 'Niet in de Active Directory'),
(0, 'Onbekend');
Controle:

sql
Code kopiëren
SELECT *
FROM adstatus
ORDER BY status_id;
Status toekennen aan medewerker
sql
Code kopiëren
UPDATE employees
SET adstatus_id = 3
WHERE employee_id = 145;
Controle:

sql
Code kopiëren
SELECT employee_id, adstatus_id
FROM employees
WHERE employee_id = 145;
Integriteit testen
Delete-test foreign key
sql
Code kopiëren
DELETE FROM adstatus
WHERE status_id = 3;
Verwachte uitkomst:

Verwijderen niet toegestaan

Foreign key constraint voorkomt dataverlies

Controle:

sql
Code kopiëren
SELECT *
FROM adstatus
ORDER BY status_id;
