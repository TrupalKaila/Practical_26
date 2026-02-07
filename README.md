# Practical-26

## Overview
This solution implements the practical requirements for an Employee Web API using ASP.NET Core with a CQRS-style separation via MediatR handlers and a dedicated DAL class library. The API supports create, update, soft delete (deactivate), and query operations for employees backed by a SQL Server table.  

## Projects
- **EmployeeAPI**: ASP.NET Core Web API project hosting the HTTP endpoints.
- **EmployeeDAL**: Class library containing models, repositories, MediatR handlers, and validation.

## Database
The DAL uses a SQL Server connection string configured in `EmployeeDAL/Data/DbService.cs`. Update it as needed for your environment.

```sql
CREATE TABLE Employee (
    EmployeeId INT IDENTITY(1,1) PRIMARY KEY,
    EmployeeName NVARCHAR(100) NOT NULL,
    EmployeeSalary DECIMAL(18,2) NOT NULL,
    DepartmentId INT NOT NULL, -- IT=1, Admin=2, HR=3, Sales=4, On-site=5 (example mapping)
    EmployeeEmail NVARCHAR(256) NOT NULL,
    EmployeeJoiningDate DATETIME NOT NULL,
    EmployeeStatus NVARCHAR(20) NOT NULL DEFAULT ('Active')
);
```

## CQRS Breakdown
The implementation uses MediatR to separate commands and queries:
- **Command Models**: `AddEmployeeDTO`, `UpdateEmployeeDTO`
- **Query Model**: `Employee`
- **Command Handlers**: Add, Update, Soft Delete
- **Query Handlers**: Get All, Get By Id

## API Endpoints
Base route: `api/employee`

### Create Employee
**POST** `/api/employee`  
Body:
```json
{
  "employeeName": "Jane Doe",
  "employeeSalary": 50000,
  "departmentId": 1,
  "employeeEmail": "jane@example.com",
  "employeeJoiningDate": "2024-01-01T00:00:00"
}
```

### Update Employee
**PUT** `/api/employee`  
Body:
```json
{
  "employeeId": 1,
  "employeeName": "Jane Doe",
  "employeeSalary": 55000,
  "departmentId": 2,
  "employeeEmail": "jane@example.com"
}
```

### Soft Delete (Deactivate Employee)
**DELETE** `/api/employee/{id}`

### Get Employees
- **GET** `/api/employee` → returns all employees  
- **GET** `/api/employee/{id}` → returns one employee  

## Running the API
From the repo root:
```bash
dotnet restore
dotnet build
dotnet run --project EmployeeAPI
```

Swagger is enabled in Development mode at:
```
https://localhost:<port>/swagger
```
