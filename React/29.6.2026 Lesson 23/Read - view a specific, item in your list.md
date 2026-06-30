### 📝 Polished Summary (Ready for GitHub)

**Title: 📖 [CRUD: Read (Single View) & Dynamic Routing]**

If you don't know what CRUD is, I recommend you go read my conceptual breakdown first:
[CRUD](<WHAT IS CRUD!!>.md)

You should also understand the basics of  list of cards etc.. : [Displaying a list of cards](<List of cards using rest api Northwind>.md).
Displaying a list is a form of **Read**. However, _Read_ can take multiple forms. As a programmer, you need to understand exactly what kind of information you want to show the user.

In this case, I will continue with the list of cards, using the Employees from the Northwind database. Since we already established a list of employees on the screen, I will show you how to display a **Single Employee Details** view when a user clicks on a specific card.

#### Step 1: Navigating to the Details Component

How does a user go to a specific employee? They click the card. You need to create an `onClick` event that triggers a function to route the user to this new component.

_Note: Every time you create a new component, you need to set up a new Route in your Routing component that contains the correct path and element._

**The Card Component Example:**

TypeScript

```
export function EmployeeCard(prop: EmployeeProps) {
    const navigate = useNavigate(); // <-- You need this React Hook to handle routing.
    
    function showDetails(): void {
        // Here is the function that navigates to a different component.
        // We are building a dynamic URL PATH using the employee ID!
        navigate("/employees/details/" + prop.employee.id); 
    }
    
    return (
        // The onClick event is attached to the wrapper
        <div className="EmployeeCard" onClick={showDetails}>
            <p>Full name: {prop.employee.firstName} {prop.employee.lastName}</p>
            <p>Birthday: {prop.employee.birthDate}</p>
            <p>Country: {prop.employee.country}</p>
            <p>City: {prop.employee.city}</p>
        </div>
    );
}
```

#### Step 2: Extract the ID & Fetch the Data

Now you want to request specific data from the server and display it. The method we use here is **extracting the ID from the URL path.**

Because we used the `Maps` function combined with the routing system, we created a dynamic URL path. We extract that ID using `useParams()`. Next, we need a service function to fetch this specific object. We combine that service call with `useState` and `useEffect` to render the component.

**The Conditional Rendering Trick:** Because we already created an `EmployeeCard` component, there is no need to violate the DRY (Don't Repeat Yourself) principle by writing out all those stats again. I used conditional rendering: it checks whether the employee exists yet. If yes, it renders the existing card. If no, it renders a loading text (or you could route to a 404 page if it fails).

**The Details Component Example:**

TypeScript

```
import { useEffect, useState } from "react";
import "./employee-details.css";
import type { EmployeeModel } from "../../../../models/employee-model";
import { NavLink, useNavigate, useParams } from "react-router-dom";
import { employeeService } from "../../../../services/employee-service";
import { EmployeeCard } from "../employee-card/employee-card";

export function EmployeeDetails() {
    const [employee, setEmployee] = useState<EmployeeModel>();
    const params = useParams();
    const id = Number(params.prodId); // Extracting the ID from the URL
    const navigate = useNavigate();

    useEffect(() => {
        employeeService.getOneEmployee(id)
            .then(dbEmp => setEmployee(dbEmp!))
            .catch(err => alert(err.message))
    }, [])

    return (
        <div className="EmployeeDetails">
            {/* Conditional Rendering Gate */}
            {employee ? (
                <div className="details-wrapper">
                    <EmployeeCard employee={employee} />
                    <NavLink to="/employees">Back</NavLink>
                </div>
            ) : (
                <p>Loading employee data.....</p>
            )}
        </div>
    );
}
```

#### Step 3: The Service Layer

Now that the component is set up, you need to add the `getOneEmployee(id)` function to your service layer. You simply fetch the base URL, but add exactly the `id` from the argument to the end of the path.

TypeScript

```
import type { EmployeeModel } from "../models/employee-model";
import { appConfig } from "../utils/app-config";

class EmployeeService {
    
    // FETCH ALL (List)
    public async getAllEmployees(): Promise<EmployeeModel[]> { 
        const response = await fetch(appConfig.employeesListUrl);
        const employees = await response.json();
        return employees;
    }
    
    // FETCH ONE (Single Details)
    public async getOneEmployee(id: number): Promise<EmployeeModel> { 
        // Look at the path: We append a "/" and the id parameter.
        const response = await fetch(appConfig.employeesListUrl + "/" + id);
        const employee = await response.json();
        return employee;
    }
}
export const employeeService = new EmployeeService();
```

#### 🔄 Architecture Flow Summary:

1. User clicks on the card.
    
2. The `onClick` event triggers `useNavigate`, dynamically passing the ID into the URL.
    
3. The Details page renders, extracts the ID, and sends a `Promise` request to the server.
    
4. The frontend waits via conditional rendering, receives the resolved Promise, and paints the employee data on the screen.
