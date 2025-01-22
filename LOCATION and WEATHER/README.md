# LOCATION and WEATHER Tables Query Examples

This repository contains SQL queries to work with two sample tables: `LOCATION` and `WEATHER`. These tables store information about geographical locations and weather conditions, respectively. Below is a detailed description of the table schemas and the queries provided in this project.

## Table Schemas

### LOCATION Table
```sql
CREATE TABLE LOCATION (
    City       VARCHAR2(25),
    Country    VARCHAR2(25),
    Continent  VARCHAR2(25),
    Latitude   NUMBER,
    NorthSouth CHAR(1),
    Longitude  NUMBER,
    EastWest   CHAR(1)
);
```

### WEATHER Table
```sql
CREATE TABLE WEATHER (
    City         VARCHAR2(11),
    Temperature  NUMBER,
    Humidity     NUMBER,
    Condition    VARCHAR2(9)
);
```

### Sample Data

#### LOCATION Table Data
```sql
INSERT INTO LOCATION VALUES ('ATHENS', 'GREECE', 'EUROPE', 37.58, 'N', 23.43, 'E');
INSERT INTO LOCATION VALUES ('CHICAGO', 'UNITED STATES', 'NORTH AMERICA', 41.53, 'N', 87.38, 'W');
INSERT INTO LOCATION VALUES ('CONAKRY', 'GUINEA', 'AFRICA', 9.31, 'N', 13.43, 'W');
INSERT INTO LOCATION VALUES ('LIMA', 'PERU', 'SOUTH AMERICA', 12.03, 'S', 77.03, 'W');
INSERT INTO LOCATION VALUES ('MADRAS', 'INDIA', 'INDIA', 13.05, 'N', 80.17, 'E');
INSERT INTO LOCATION VALUES ('MANCHESTER', 'ENGLAND', 'EUROPE', 53.30, 'N', 2.15, 'W');
INSERT INTO LOCATION VALUES ('MOSCOW', 'RUSSIA', 'EUROPE', 55.45, 'N', 37.35, 'E');
INSERT INTO LOCATION VALUES ('PARIS', 'FRANCE', 'EUROPE', 48.52, 'N', 2.20, 'E');
INSERT INTO LOCATION VALUES ('SHENYANG', 'CHINA', 'CHINA', 41.48, 'N', 123.27, 'E');
INSERT INTO LOCATION VALUES ('ROME', 'ITALY', 'EUROPE', 41.54, 'N', 12.29, 'E');
INSERT INTO LOCATION VALUES ('TOKYO', 'JAPAN', 'ASIA', 35.42, 'N', 139.46, 'E');
INSERT INTO LOCATION VALUES ('SYDNEY', 'AUSTRALIA', 'AUSTRALIA', 33.52, 'S', 151.13, 'E');
INSERT INTO LOCATION VALUES ('SPARTA', 'GREECE', 'EUROPE', 37.05, 'N', 22.27, 'E');
INSERT INTO LOCATION VALUES ('MADRID', 'SPAIN', 'EUROPE', 40.24, 'N', 3.41, 'W');
```

#### WEATHER Table Data
```sql
INSERT INTO WEATHER VALUES ('LIMA', 45, 79, 'RAIN');
INSERT INTO WEATHER VALUES ('PARIS', 81, 62, 'CLOUDY');
INSERT INTO WEATHER VALUES ('MANCHESTER', 66, 98, 'FOG');
INSERT INTO WEATHER VALUES ('ATHENS', 97, 89, 'SUNNY');
INSERT INTO WEATHER VALUES ('CHICAGO', 66, 88, 'RAIN');
INSERT INTO WEATHER VALUES ('SYDNEY', 69, 99, 'SUNNY');
INSERT INTO WEATHER VALUES ('SPARTA', 74, 63, 'CLOUDY');
```

## Queries

### Basic Queries

- Display all cities and countries from the `LOCATION` table:
  ```sql
  SELECT City, Country FROM LOCATION;
  ```

- Display all cities and weather conditions from the `WEATHER` table:
  ```sql
  SELECT City, Condition FROM WEATHER;
  ```

- Find all cities with cloudy weather:
  ```sql
  SELECT City FROM WEATHER WHERE Condition = 'CLOUDY';
  ```

- Find specific cities and their countries:
  ```sql
  SELECT City, Country FROM LOCATION WHERE City IN ('PARIS', 'SPARTA');
  ```

- Find cities and countries where the weather condition is cloudy:
  ```sql
  SELECT City, Country FROM LOCATION WHERE City IN (SELECT City FROM WEATHER WHERE Condition = 'CLOUDY');
  ```

### Advanced Queries

- Display city, condition, and temperature from the `WEATHER` table:
  ```sql
  SELECT City, Condition, Temperature FROM WEATHER;
  ```

- Display geographical details from the `LOCATION` table:
  ```sql
  SELECT City, Longitude, EastWest, Latitude, NorthSouth FROM LOCATION;
  ```

- Join `WEATHER` and `LOCATION` tables to display combined details:
  ```sql
  SELECT WEATHER.City, Condition, Temperature, Latitude, NorthSouth, Longitude, EastWest
  FROM WEATHER, LOCATION
  WHERE WEATHER.City = LOCATION.City;
  ```

### Views

- Create a view `INVASION` with combined data from both tables:
  ```sql
  CREATE VIEW INVASION AS
  SELECT WEATHER.City, Condition, Temperature, Latitude, NorthSouth, Longitude, EastWest
  FROM WEATHER, LOCATION
  WHERE WEATHER.City = LOCATION.City;
  ```

- Display all details from the `INVASION` view:
  ```sql
  SELECT City, Condition, Temperature, Latitude, NorthSouth, Longitude, EastWest FROM INVASION;
  ```

- Filter data from the `INVASION` view for a specific country:
  ```sql
  SELECT City, Condition, Temperature, Latitude, NorthSouth, Longitude, EastWest
  FROM INVASION
  WHERE Country = 'GREECE';
  ```

### Subqueries

- Find records for a specific country in the `LOCATION` table:
  ```sql
  SELECT * FROM LOCATION WHERE Country = 'PERU';
  ```

## How to Use
1. Clone this repository:
   ```bash
   git clone https://github.com/your-username/location-weather-sql.git
   ```
2. Import the SQL script into your database.
3. Run the queries as needed.

## License
This project is licensed under the MIT License. Feel free to use and modify it as needed.

