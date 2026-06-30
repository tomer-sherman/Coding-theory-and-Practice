Before reading all of this you need too make sure you understand these contepts:
Routing: [Routing](Routing.md)
useEffect: [useEffect](useEffect.md)
useState: [useState](<useState (Local State).md>)

A complete step-by-step guide on how to fetch, type, and render data from an external API in a React TypeScript application.

## ⚙️ System Requirements

This specific exercise uses **Northwind** as a local server on your PC. You must install and run the Northwind package before starting your web app.

Bash

```
# 1. Install the Northwind Server globally
npm install --global northwind-rest-api

# 2. Run the server in a separate terminal window
northwind
```

## 🗺️ Information Flow Overview

1. **User Action:** The frontend user clicks on a navigation link to route to a specific page.
    
2. **Request:** The component mounts and calls the server to get information from the Database.
    
3. **Response:** The server grabs the data from the database and sends it back to the frontend to be displayed.
    

## 🚀 Implementation Steps

### Step 1: Understand the Data

Before writing any code to display a list, you must understand what data the server provides. Navigate to the API link in your browser: 👉 `http://localhost:3030/api/employees`

The JSON data looks like this:

JSON

```
[
  {
    "id": 1,
    "firstName": "Nancy",
    "lastName": "Davolio",
    "title": "Sales Representative",
    "country": "USA",
    "city": "Seattle",
    "birthDate": "1948-12-08",
    "imageUrl": "http://localhost:3030/api/employees/images/01a5f7d1-42e8-4f71-83c4-58e86751b272.jpg"
  }
]
```

### Step 2: Create a TypeScript Model

Models are used to create a custom **Type**. This makes TypeScript understand the exact structure of the data, making our code cleaner and preventing future bugs.

Use the CLI to generate the file quickly:

Bash

```
create model EmployeeModel
```

Inside the generated `employee-model.ts` file, change the class to a `type` and add all the properties you found in Step 1:

TypeScript

```
export type EmployeeModel = {
    id: number;
    firstName: string;
    lastName: string;
    title: string;
    country: string;
    city: string;
    birthDate: string;
    imageUrl: string;
};
```

### Step 3: Create the App Config

This file stores our API URLs so we don't have to copy-paste them everywhere.

Bash

```
create util AppConfig
```

Inside `app-config.ts`, add the link as a `public readonly` property:

TypeScript

```
class AppConfig {
    public readonly employeesUrl = "http://localhost:3030/api/employees";
}

// Export a single instance to use across the app
export const appConfig = new AppConfig();
```

### Step 4: Create a Service File

The service file holds the logic for fetching data from the API and returning it as an array of the specific Model we created.

Bash

```
create service EmployeeService
```

Inside `employee-service.ts`, write the fetch logic:

TypeScript

```
import { EmployeeModel } from "../models/employee-model";
import { appConfig } from "../utils/app-config";

class EmployeeService {
    // Fetch all employees and return a Promise containing an array of EmployeeModels
    public async getAllEmployees(): Promise<EmployeeModel[]> {
        const response = await fetch(appConfig.employeesUrl);
        const employees = await response.json();
        return employees;
    }
}

export const employeeService = new EmployeeService();
```

### Step 5: Create the Employee Card Component

This component represents a _single_ employee. We will take this component and map it inside our larger list component.

We must define a prop type (`EmployeeProp`) so the card knows it should only accept data structured like our `EmployeeModel`.

TypeScript

```
import "./employee-card.css";
import { EmployeeModel } from "../../../models/employee-model";

// 1. Define the props this component accepts
export type EmployeeProp = {
    employee: EmployeeModel;
}

// 2. Render the HTML using the passed-in employee data
export function EmployeeCard(props: EmployeeProp) {
    return (
        <div className="employee-card">
            <img src={props.employee.imageUrl} alt={props.employee.firstName} />
            <h3>{props.employee.firstName} {props.employee.lastName}</h3>
            <p>Birthday: {props.employee.birthDate}</p>
            <p>City: {props.employee.city}</p>
        </div>
    );
}
```

### Step 6: Create the Employee List Component (The Papa Component)

This is where everything connects. We use three main sections: **State Management**, the **Effect (Fetch) Hook**, and the **Return (Render)**.

TypeScript

```
import { useEffect, useState } from "react";
import "./employee-list.css";
import { EmployeeModel } from "../../../models/employee-model";
import { employeeService } from "../../../services/employee-service";
import { EmployeeCard } from "../employee-card/employee-card";

export function EmployeeList() {
    // 1. Local State Management: Starts as an empty array []
    const [employees, setEmployees] = useState<EmployeeModel[]>([]);

    // 2. useEffect: Triggers the service fetch ONCE when the component first loads
    useEffect(() => {
        employeeService.getAllEmployees()
            .then(employeesData => setEmployees(employeesData))
            .catch(err => alert(err.message));
    }, []); // <-- Empty dependency array ensures this only runs once

    // 3. Return Section: Maps over the state array to render a Card for every employee
    return (
        <div className="employee-list">
            {employees.map(emp => (
                <EmployeeCard key={emp.id} employee={emp} />
            ))}
        </div>
    );
}
```

## 🧠 Code Flow Summary

1. **Mount:** The user routes to the `EmployeeList` component.
    
2. **Fetch:** Immediately, the `useEffect` block triggers the `employeeService` to request data from the local server.
    
3. **Update State:** The server returns the data, which is successfully validated against our blueprint (`EmployeeModel`). The `setEmployees` function updates the local state.
    
4. **Render:** The state update causes the component to re-render, mapping through the array and printing beautiful `<EmployeeCard />` components to the screen!