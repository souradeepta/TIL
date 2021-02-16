# Understanding the URI Param and Query Param With RAML

## 1.0 Introduction

It is very important to know when to use Query Parameter or URI Parameter while designing an API. URI parameter (Path Param) is basically used to identify a specific resource or resources whereas Query Parameter is used to sort/filter those resources.

Let's consider an example where you want identify the employee on the basis of employeeID, and in that case, you will be using the URI param.

**GET** /employee/{employeeID}

Take another example where you want to filter the employee on the basis of designation, and in that case, you will be using Query Parameter.

**GET** /employee?designation=SSE

| Return list of car having color black | GET /cars?color=black                                   |
| ------------------------------------- | ------------------------------------------------------- |
| Return list of cars sorted by model   | GET /cars?sortBy=model                                  |
| Return list of drivers for car888     | GET /cars/{car-number}/drivers GET /cars/car888/drivers |
| Return employees having employeeID 10 | GET /employees/{employeeID} GET /employees/10           |

## 2.0 Defining URI Parameter With RAML

URI Parameter is a variable element enclosed in curly braces {} inside relative URI of resources.

```js
/employees:
  /{employeeID}:
    description: Return employee information on basis of EmployeeID
    get:
      responses: 
        200:
          body: 
            application/json:
              example:  |
                {"employeeID":"1", "employeeName":"Tom Berry","designation":"SSE"}
```

Looking at above RAML, the URL to get employee details on the basis of employeeID will be **GET /employees/{employeeID}** where employees is resource and {employeeID} is URI parameter**.**

## 3.0 Defining the Query Parameter With RAML

Query Parameter is basically used to filter or sort the resources. It is passed in the URL as query string in key-value form.

```js
/employees:
  get:
    queryParameters: 
      designation:
        description: Desgnation of employee
        type: string
        required: true
    responses: 
      200:
        body: 
          application/json:
            example: |
              {"employees":[
              {"employeeID":"1", "employeeName":"Tom Berry","designation":"SSE"},
              {"employeeID":"2", "employeeName":"JAck Jones","designation":"SSE"}
              ]}
```

Looking at the above RAML, the URL to get employee details having designation SSE will be GET /employees?designation=SSE.

In the above example, query parameter designation is mandatory and even you can make it optional by modifying the required to false.

## 4.0 Final RAML With URI Parameter and Query Parameter

```js
#%RAML 1.0
title: Employees

/employees:
  get:
    queryParameters: 
      designation:
        description: Desgnation of employee
        type: string
        required: true
    responses: 
      200:
        body: 
          application/json:
            example: |
              {"employees":[
              {"employeeID":"1", "employeeName":"Tom Berry","designation":"SSE"},
              {"employeeID":"2", "employeeName":"JAck Jones","designation":"SSE"}
              ]}
  /{employeeID}:
    description: Return employee information on basis of EmployeeID
    get:
      responses: 
        200:
          body: 
            application/json:
              example:  |
                {"employeeID":"1", "employeeName":"Tom Berry","designation":"SSE"}
```

Now you know when and how to use URI parameter and Query Parameter With RAML.