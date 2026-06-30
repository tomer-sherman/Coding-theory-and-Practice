# 🔄 CRUD: Update (Edit Flow & FormData)

**System Requirements:** Make sure you have this npm package installed: `npm install axios`

If you don't know what CRUD is, I recommend you go read my conceptual breakdown first:
[CRUD](<WHAT IS CRUD!!>.md)
You should also understand the basics of displaying lists:
[Displaying a list of cards](<List of cards using rest api Northwind>.md)
and viewing single items:
[Read](<Read - view a specific, item in your list>.md)

### 🏗️ The Overview

In this case, we use a separate form page to fill in the data in order to update a specific Employee.

1. The user clicks on a card and wants to edit it.
    
2. They click an "Edit" button, which leads them to a form page with all the current details already written in the inputs.
    
3. They edit the text and click Update to change this specific object in the database.
    

**Keep in mind:** Since it is an _Update_, there are **two requests** happening in this process. The first request gets the specific object data from the server to fill the form. The second request happens when the user presses update, sending the new data to the database.

### Step 1: Navigating to the Form Component

You need to make sure the user can navigate to the new Form component. You can use the `useNavigate` hook, or a `<NavLink>`. Here is how we navigate from the `EmployeeDetails` page to the `EmployeeEdit` page:

TypeScript

```
return (
    <div className="EmployeeDetails">
        {employee ? (
            <div className="details-wrapper">
                {/* Other employee details... */}
                <NavLink to="/employees">Back</NavLink>
                <NavLink to={"/employees/edit/" + employee.id}>Edit</NavLink> 
                <NavLink to="#">Delete 🚮</NavLink>
            </div>
        ) : (
            <p>Loading employee data.....</p>
        )}
    </div>
);   
```

_Note: If you use this option, you need to make sure your Router component has the route set up correctly:_ `<Route path='/employees/edit/:prodId' element={<EmployeeEdit />}/>`

### Step 2: Creating the EmployeeEdit Component

After routing correctly, you need to set up the form component. The first step is rendering the information about this specific object from the database into the inputs. We use `useForm`, `useParams`, and an API fetch.

#### A. Setting up the Form

In the return section, we set up a `<form>` and use the `useForm`'s `register` function to map the properties of our `EmployeeModel` to the correct inputs.

TypeScript

```
const { register, handleSubmit, reset } = useForm<EmployeeModel>(); 

return (
    <form>
        <label>First Name:</label>
        <input type="text" {...register("firstName")} />
        
        <label>Last Name:</label>
        <input type="text" {...register("lastName")} />
        
        <label>Birthday:</label>
        <input type="date" {...register("birthDate")} required />
        
        <label>Country:</label>
        <input type="text" {...register("country")} />
        
        <label>City:</label>
        <input type="text" {...register("city")} />
        
        <label>Image:</label>
        <input type="file" accept="image/*" {...register("image")} />
        
        <button>Update</button>
    </form>
);
```

#### B. Pre-filling the Form with Data (`reset`)

Using the `useEffect` hook and a service function, we fetch the data when the page loads. Here comes a very useful function within `useForm` called **`reset`**. It takes a data object and instantly puts it inside your form inputs.

**The Service Function:**

TypeScript

```
public async getOneEmployee(id: number): Promise<EmployeeModel> {
    const response = await fetch(appConfig.employeesListUrl + "/" + id);
    const employee = await response.json();
    return employee;
}
```

**The Component Logic:**

TypeScript

```
export function EmployeeEdit() {
    const { register, handleSubmit, reset } = useForm<EmployeeModel>();
    const navigate = useNavigate();
    const params = useParams();
    const id = Number(params.prodId);
    
    useEffect(() => {
        employeeService.getOneEmployee(id)
            .then(dbEmployee => {
                // We check if it exists, then reset the form to pre-fill the inputs
                if (dbEmployee) { 
                    reset(dbEmployee); 
                }
            })
            .catch(err => alert(err.message));
    }, []);
    
    // ... return statement below
```

_Note: Since it returns a promise, I used `.then` and `.catch`. Now all the existing data is rendered inside the inputs on the page!_

### Step 3: Sending the Updated Data (HTTP PUT)

Now we need to let the user update the text and send it to the server. This requires an `onSubmit` event that activates an update function, which utilizes a new service function.

#### A. The Service Function (`FormData` & Axios PUT)

This function takes the info you inputted in the form and makes it readable for the server. Because we are dealing with a physical image file, we must utilize the `FormData` class and `axios`.

You create a new `FormData` object and append all the employee values to it. What does `PUT` do? Instead of adding a new item, it overwrites the content of an existing item in the database.

TypeScript

```
public async updateEmployee(employee: EmployeeModel): Promise<void> {
    const employeeFormData = new FormData();
    
    employeeFormData.append("firstName", employee.firstName);
    employeeFormData.append("lastName", employee.lastName);
    employeeFormData.append("birthDate", employee.birthDate);
    employeeFormData.append("country", employee.country);
    employeeFormData.append("city", employee.city);
    
    // Only append the image if the user actually uploaded a new one!
    if (employee.image) {
        employeeFormData.append("image", employee.image as Blob);
    }
    
    // Send the FormData payload to the specific ID URL
    const response = await axios.put(appConfig.employeesListUrl + "/" + employee.id, employeeFormData); 
    console.log(response.data);
}
```

#### B. The Component Update Function

Wrap your update logic in a `try...catch` block. First, we assign the `id` from the URL to the employee object. Next, we extract the physical file from the input using `(employee.image as unknown as FileList)[0]`. Finally, we send it to the service and navigate back to the list.

**The Complete Component Execution:**

TypeScript

```
async function update(employee: EmployeeModel) {
    try {
        employee.id = id;
        
        // Extract the actual file object from the FileList array
        if (employee.image) {
            employee.image = (employee.image as unknown as FileList)[0];
        }
        
        await employeeService.updateEmployee(employee);
        navigate("/employees");
        
    } catch (err: any) {
        alert(err.message);
    }
}

return (
    <div className="EmployeeEdit">
        <form onSubmit={handleSubmit(update)}>
            {/* ... form inputs ... */}
        </form>
    </div>
);
```

### 🔄 Code Flow Review:

1. User clicks the NavLink to navigate to the Edit page.
    
2. The page loads and triggers a `GET` request from the server to pull the existing data.
    
3. `reset(dbEmployee)` injects that data into the input boxes.
    
4. The user updates the text/image and clicks Submit.
    
5. The `update` function packages the physical file and text into `FormData`.
    
6. Axios fires a `PUT` request to overwrite the database record.
    

**Update is just a combination of the Read concept and the Create concept.** If you understand how to Update, understanding Create (Insert) will be incredibly easy!