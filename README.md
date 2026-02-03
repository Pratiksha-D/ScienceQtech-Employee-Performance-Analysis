/*
SQL Project: ScienceQtech Employee Performance Analysis
Role: Junior Database Administrator / Data Analyst
*/

-- 1. Create the Database
CREATE DATABASE IF NOT EXISTS employee_db;
USE employee_db;

-- 2. Create Tables (Based on Dataset Description)
CREATE TABLE emp_record_table (
    EMP_ID VARCHAR(10) PRIMARY KEY,
    FIRST_NAME VARCHAR(50),
    LAST_NAME VARCHAR(50),
    GENDER CHAR(1),
    ROLE VARCHAR(50),
    DEPT VARCHAR(50),
    EXP INT,
    COUNTRY VARCHAR(50),
    CONTINENT VARCHAR(50),
    SALARY INT,
    EMP_RATING INT,
    MANAGER_ID VARCHAR(10),
    PROJ_ID VARCHAR(10)
);

CREATE TABLE proj_table (
    PROJECT_ID VARCHAR(10) PRIMARY KEY,
    PROJ_Name VARCHAR(100),
    DOMAIN VARCHAR(50),
    START_DATE DATE,
    CLOSURE_DATE DATE,
    DEV_QTR VARCHAR(5),
    STATUS VARCHAR(20)
);

CREATE TABLE data_science_team (
    EMP_ID VARCHAR(10),
    FIRST_NAME VARCHAR(50),
    LAST_NAME VARCHAR(50),
    GENDER CHAR(1),
    ROLE VARCHAR(50),
    DEPT VARCHAR(50),
    EXP INT,
    COUNTRY VARCHAR(50),
    CONTINENT VARCHAR(50)
);

-- Note: Use the 'LOAD DATA INFILE' command or the Import Wizard to populate these tables with your CSV data.

---
-- HR ANALYTICS QUERIES
---

-- Q1: Fetch maximum salary to understand the compensation ceiling.
SELECT MAX(SALARY) AS MAX_SALARY 
FROM emp_record_table;

-- Q2: Calculate bonuses for budget planning.
-- Formula: 5% of salary * employee rating
SELECT 
    EMP_ID, 
    FIRST_NAME, 
    LAST_NAME, 
    SALARY, 
    EMP_RATING,
    (SALARY * 0.05 * EMP_RATING) AS BONUS
FROM emp_record_table;

-- Q3: Performance Mapping and Role Standards.
-- Identifying employees with high performance (Rating > 4) for potential promotion.
SELECT 
    EMP_ID, 
    FIRST_NAME, 
    ROLE, 
    EXP, 
    EMP_RATING 
FROM emp_record_table 
WHERE EMP_RATING >= 4;

-- Q4: Identify employees requiring training.
-- Employees with a rating below 2 need development programs.
SELECT 
    EMP_ID, 
    FIRST_NAME, 
    LAST_NAME, 
    DEPT, 
    EMP_RATING 
FROM emp_record_table 
WHERE EMP_RATING < 2;

-- Q5: Project Assignment Report.
-- Joining Employee and Project tables to see who is working on what.
SELECT 
    e.EMP_ID, 
    e.FIRST_NAME, 
    p.PROJ_Name, 
    p.DOMAIN, 
    p.STATUS
FROM emp_record_table e
INNER JOIN proj_table p ON e.PROJ_ID = p.PROJECT_ID;
