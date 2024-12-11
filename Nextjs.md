# REACTJS 😎

- Installation and setup

```js
- npm create vite@latest my-react-app --template react
- cd my-react-app
- npm i
- npm run dev
localhost:5173
```

## REACT HOOKS

- To use the different functionality provided by react we need to use **react hooks**


### useState Hook

- useState is a React Hook that lets you add a state variable to your component.

```js
// INITIALIZATION
const [state, setState] = useState(initialState);

// MAIN CODE OF IMPLEMENTATION
import React from 'react'
import { useState } from 'react';

export const StateHook = () => {
    const [count, setcount] = useState(0);

  return (
    <div >
      <h1>You have clicked the button {count} times</h1>
      <button onClick={()=>{setcount(count+1)}}>Add</button>
      <button onClick={()=>{setcount(count-1)}}>Sub</button>
    </div>
  )
}
```




### React State lifting (Passing data from child to parent)

- Passing data from child to parent.
- If there are two child we can sync them together
- Here, parent will manage(change and update) the state.

```js
// CHILD COMPONENT
import React from 'react'

export const StateLifting = ({name,setName}) => {
  return (
    <div className='flex flex-col items-center justify-center p-10'>
      <h1>Hello my Name is {name}</h1>
      <input type="text" onChange={(e)=>{setName(e.target.value)}} className='bg-red-300 text-white'/>
    </div>
  )
}

// PARENT COMPONENET
function App() {
  const [name, setName] = React.useState("Anurag");
  const [theme, setTheme] = useState('light');

  return (
      <div>

      <StateLifting name={name} setName={setName}/>
      <h1>Hey there my name is {name}</h1>

      </div>
  );
}
```


### Conditional Rendering

1. Ternanry operators

```js
(state ? if_true_this : if_false_this);
```

- If state condition is true then perform **if_true_this** else **if_false_this**

2. Logical operators

```js
(state && perform_this);
```

- If state condition is true perform the operation

### Event Handling


```js
import React from "react";

export const eventHandling = ()=>{

  function handleSubmit(){
    alert("form submitted successfully!!!!");
  }

  return (
    <div>
        <form onSubmit={handleSubmit}>
          <input type="text" onChange={(e)=>{console.log(e.target.value)}}/>
          <input type="submit" value={"Submit"}/>
        </form>
    </div>
  );
}
```

- In form Handling, always use **(e.preventDefault())**
- Prevent **immediate invokation**
- **(e.stopPropagation())** -> Stops event bubbling



### useEffect Hook (Side-effect generator)

- Components  ->  Render/variable changes  ->  Perform some calls/connection/operations/tasks


```js
useEffect(() => {
  // Set-up function

  return () => {
    // Clean-up function
  };

}, [variable]);
```

-  After every re-render with changed dependencies, React will first run the cleanup function and then run your setup function with the new values.
- The list of all reactive values referenced inside of the setup code.


```js
import React, { useState } from 'react'
import { useEffect } from 'react';

export const UseEffect = () => {
    const [count, setCount] = useState(0);
    const [total, setTotal] = useState(1);

    function handleCount(){
        setCount(count+1);
    }
    function handleTotal(){
        setTotal(total+1);
    }


    // // VARIATION1 : RUNS ON EVERY RENDER
    // useEffect(()=>{
    //     console.log("I will run on every render");
    // });


    // // VARIATION2 : RUNS ON ONLY FIRST RENDER
    // useEffect(()=>{
    //     console.log("I will run on every render");
    // }, []);


    // // VARIATION3 : RUNS WHEN DEPENDENCIES IS CHANGED
    // useEffect(()=>{
    //     console.log("I will run on every render");
    // }, [count]);


    // // VARIATION4 : MULTIPLE DEPENDENCIES
    // useEffect(()=>{
    //     console.log("I will run on every render");
    // }, [count,total]);


    // // VARIATION5 : ADDING CLEAN-UP FUNCTION
    // useEffect(()=>{
    //     console.log("I will run on every render");
    //     return ()=>{
    //         console.log("I will run when variable is unmounted");
    //     }
    // }, [count,total]);

  return (
    <div className='flex flex-col p-10 gap-2 items-center'>
      <button onClick={handleCount} className='border border-black p-1'>Count</button>
      <p>Count:{count}</p>

      <button onClick={handleTotal} className='border border-black p-1'>Total</button>
      <p>total:{total}</p>
    </div>
  )
}

```


### useContext hook (Props-drilling)

- **Passing data deeply into component tree** is one of the main usage.
- Parent will provide data and children will provide,consume and update the data.


```js
// PARENT COMPONENT
import React, { createContext,useState } from "react";

// Create Context
// Wrap the children inside an provider
// Pass the value through provider
// Consume data inside children
const themeContext = createContext();

function App(){
  const [theme, setTheme] = useState('light');

  return (
    <div>
      <themeContext.Provider value={{theme,setTheme}}>
          <ChildComponent></ChildComponent>
      </themeContext.Provider>
    </div>
  );
}

export default App;
export {themeContext};


// CHILD COMPONENT
import React, { useContext } from 'react'
import {themeContext} from "../App";

export const UseContext = () => {
    const {theme, setTheme} = useContext(themeContext);

    function handleTheme(){
        if(theme == "light"){
            setTheme("dark");
        }else{
            setTheme("light"); 
        }
    }

  return (
      <div 
        className='border border-black p-4 h-28 w-48 m-10' 
        style={{backgroundColor:theme == "light" ? "red" : "blue"}}
        >
        <button className='border border-black p-1 bg-gray-700 text-white' onClick={handleTheme}>change Theme</button>
      </div>
  );
}

```



### useRef Hook ()

- useRef is a React Hook that lets you reference a value that's not needed for rendering.
- Whenever the ref variable is changed the page will not re-render.


```js
const ref = useRef(initialValue)
```

- When you change the **ref.current** property, React does not re-render your component.
- Do not write or read **ref.current** during rendering

- {ref.current} can be accesible only inside an event handlers.

**Usage**
1. COMPARE TO STATE HOOK

- State_var  ->  change  -> Re-render
- Ref_var    ->  change  -> No re-render


2. MANIPULATING DOM WITH REF


- We can change the DOM property of a component with useRef hook

```js
import React, { useRef } from 'react'

const UseRef = () => {
    const [time,setTime] = React.useState(0);
    const timeRef = useRef(null);
    const btnRef = useRef(null);

    function handleStart(){
        timeRef.current = setInterval(() => {
            setTime(time => time+1);
        }, 100);
    }
    
    
    function handleStop(){
        clearInterval(timeRef.current);
        timeRef.current = null;
    }
    function handleReset(){
        handleStop();
        setTime(0);
    }
    // MANIPULATING DOM WITH REF    
    function handleBtnRef(){
        btnRef.current.style.backgroundColor = "red";
    }

  return (
    <div className='border border-black m-10 p-3 flex flex-col'>
      <h1 className='text-3xl'>Use Reference Hook</h1>
      <div className='m-5'>
        <h1 className="text-2xl">Time: {time} Seconds</h1>
        <button 
            className='border border-black p-1' 
            onClick={handleStart}
            ref={btnRef}
            >
            Start
        </button>
        <br/>
        <button className='border border-black p-1' onClick={handleStop}>
            Stop
        </button>
        <br/>
        <button className='border border-black p-1' onClick={handleReset}>
            Reset
        </button>
        <br/>
      </div>
      <button onClick={handleBtnRef} className='border border-black p-1'>Change color of Start button</button>
    </div>
  )
}

export default UseRef;
```



### useMemo hook (prevent expensive operation)

- useMemo is a React Hook that lets you cache the result of a calculation between re-renders.

- In useMemo hook, already calculated value is **cached/store** and when it is call again we will return already store value to optimize performance.

```js
const cachedValue = useMemo(Value, dependencies);
```

- The function calculating the value that you want to cache(store).
- The list of all reactive values referenced inside of the calculateValue code


**Usage**
1. Skipping expensive recalculations 
2. Skipping re-rendering of components(child coomponent)
3. Memoizing a function 


```js
import React from 'react'
import { useMemo } from 'react';

const UseMemo = () => {
    const [count, setCount] = React.useState(0);
    let changingValue;

    function increment(){
        setCount(count+1);
    }


    function expensiveTask(num){
        console.log("Runs on every expensive operation");
        for (let i = 0; i <100000000; i++) {};
        return num*2;
    }
    
    console.time();
    const testOperation = useMemo(()=>{expensiveTask(4)},[changingValue_that_can_cause_re_render]);
    const testOperation = expensiveTask(4);
    console.timeEnd();

  return (
    <div className='border border-black m-10 p-3 flex flex-col'>
      <h1 className='text-3xl'>Use Memo hook</h1>
      
      <div className='m-10'>
        <h1 className='text-xl'>Count: {count}</h1>
        <button className='border border-black m-5' onClick={increment}>Add</button>
        <p>{testOperation}</p>
      </div>
    </div>
  )
}

export default UseMemo;
```


### useCallback hook ()

- useCallback is a React Hook that lets you cache a function definition between re-renders.

```js
const cachedFn = useCallback(fn, dependencies);
```


**Usage**
1. Skipping re-rendering of components 

- Prevent unnecessary re-rendering of Child component inside an parent component.

```js
export const ChildComponent = React.memo(({count,setCount})=>{
    // code here
});
```
- here, we have passed a variable as a props so **React.memo(()=>{})** can help in re-rendering.
- But, if we passed function it will wom't stop from re-render.


2. Prevent re-rendering of expensive operation

- Wrap the expensive function/calculation inside an **useCallback hook**
- prevent re-creation of function to prevent unnecessary re-rendering of child component.
- Function re-creation decreases the performance of our web-app





### React Router

- Read docs for more understanding.


```js
import { BrowserRouter, Routes, Route } from "react-router";
import UseParams from "./components/RouterPages/UseParams";
import {ReactRouter} from "./components/ReactRouter";

function App() {
  return (
    <BrowserRouter>
      <div>
        <Routes>
        <Route path="/home" element={<Home />}>
          <Route path="profile" element={<Profile />}/>
        </Route>
        <Route path="/about" element={<About />} />
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/login/:name" element={<UseParams />} />
        <Route path="*" element={<ErrorPage />} />
      </Routes>
      <ReactRouter></ReactRouter>
      </div>
    </BrowserRouter>
  );
}


// REACT-ROUTER PAGE
import React from 'react'
import { NavLink,Link,useNavigate } from 'react-router';

// // INSATLLATION
// npm i react-router

export const ReactRouter = () => {
    const navigate = useNavigate();

  return (
    <div className='border border-black p-3 flex flex-row gap-10 m-10'>
        <NavLink to='/home' className={({isActive}) => isActive ? "text-red-900 font-bold" : "text-black"}>
            <h1>Home</h1>
        </NavLink>
        <NavLink to='/about' className={({isActive}) => isActive ? "text-red-900 font-bold" : "text-black"}>
            <h1>About</h1>
        </NavLink>

        <button 
            className='border border-black p-2 m-3'
            onClick={()=>{navigate('/dashboard')}}
            >
            Move to dash Page
        </button>  
    </div>
  ) 
}


// useParams hook
import { useParams } from 'react-router'

export default const UseParams = () => {
    const {name} = useParams();
  return (
    <div>
      <h1>Use Params Hook</h1>
      <h1 className='text-red-500 font-bold'>{name}</h1>
    </div>
  )
}
```



### React Hook Form

- Read docs for more understanding.

```js
import React from 'react'
import { useForm } from "react-hook-form";


const UseForm = () => {
    const { register, handleSubmit, formState: { errors, isSubmitting } } = useForm();

    async function handleFormSubmit(data) {
        await new Promise((resolve) => setTimeout(resolve, 5000));
        console.log(data);
    }

    return (
        <div className='border border-black p-3 m-10 flex flex-col gap-5'>
            <h1>Use Form Hook</h1>
            <form onSubmit={handleSubmit(handleFormSubmit)}>

                <div>
                    <label >Firstname:</label>
                    <input
                        className='border border-black m-2'
                        type="text"
                        {...register("firstName", { required: true })}
                        aria-invalid={errors.firstName ? "true" : "false"}
                    />
                    {errors.firstName?.type === 'required' && <p role="alert">First name is required</p>}
                </div>

                <div>
                    <label >MiddleName:</label>
                    <input
                        className='border border-black m-2'
                        type="text"
                        {...register("middleName", { required: "MiddleName is required!!!", minLength:{value:5,message:"5 character de!!!"} })}
                        aria-invalid={errors.middleName ? "true" : "false"}
                    />
                    {errors.middleName && <p role="alert">{errors.middleName?.message}</p>}
                </div>

                <div>
                    <label >LastName:</label>
                    <input
                        className='border border-black m-2'
                        type="text"
                        {...register("lastName", { required: "Lastname is required", minLength: { value: 4, message: "Minimum 4 characters are req!!!" }, maxLength: { value: 6, message: "maximum 6 characters is allowed!!!" } })}
                        aria-invalid={errors.lastName ? "true" : "false"}
                    />
                    {errors.lastName && <p role='alert'>{errors.lastName?.message}</p>}
                </div>

                <button
                    type="submit"
                    className='border border-black m-2'
                    disabled={isSubmitting}
                >
                    {isSubmitting ? "Loading..." : "Submit"}
                </button>
            </form>
        </div>
    )
}

export default UseForm

```



### STYLING AND CSS ✨

- Tailwind CSS
- Daisy UI
- Material UI
- Provider {react-hot-toast}
- Acertinity UI
- Shadcn UI


# NEXTJS 🔥

- It is an react framework use to build the fullstack application.


## INSTALLATION

- RUN THIS CODE IN ./nextjs DIRECTORY.

```js

npx create-next-app@latest appName

``` 

## PACKAGES TO BE INSTALLED

```js

npm install @clerk/nextjs@latest @prisma/client@latest  @tanstack/react-query@latest @tanstack/react-query-devtools@latest axios@latest react-hot-toast@latest react-icons@latest dayjs@latest next-themes@latest recharts@latest

```


## Tailwind and DaisyUI  🎆

- Install the npm package :-

```js

npm i -D daisyui@latest @tailwindcss/typography@latest prisma@latest

```

- Also add the plugin in 'tailwind.config.js' :-

```javascript
tailwind.config.js

module.exports = {
  ...
  plugins: [require('@tailwindcss/typography'), require('daisyui')]

};
```





## FILE AND FOLDER STRUCTURE

- The folders will contain all our .jsx pages and will be used for routing.



## ROOT LAYOUTS (Required) { layout.js }

- layouts preserve state, remain interactive, and do not re-render. Layouts can also be nested.
- The component should accept a {children} prop that will be populated with a child layout (if it exists) or a page during rendering.

- The root layout { layout.js } is defined at the top level of the app directory and applies to all routes.
- This layout is required and must contain html and body tags, allowing you to modify the initial HTML returned from the server.
- By default { NextJs } will automatically insert the { layout.js } file in our { app directory }.
- When a {layout.js} and {page.js} file are defined in the same folder, the layout will wrap the page.



### CODE SYNTAX FOR {layout.js}. THIS WILL REMAIN ALMOST SAME.

```javascript
// THIS IS THE NESTED LAYOUTS
// 'children' WILL FETCH THE REMAINNG '.jsx' FILE INSIDE OUR FOLDERS.

import { Inter } from "next/font/google";
import Navbar from "@/components/Navbar";
import Footer from "@/components/Footer";
import "./globals.css";
import { SpeedInsights } from "@vercel/speed-insights/next";
import Providers from "./providers";

const inter = Inter({ subsets: ["latin"] });

export const metadata = {
  title: "NextJs App",
  description: "Generated by create next app",
};

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body className={inter.className} style={{fontFamily:"monospace"}}>

        <Navbar></Navbar>
        <Providers>{children}</Providers>
        <Footer></Footer>
      </body>
    </html>
  ); 
} 

```


## PROVIDERS

```js

npm install react-hot-toast

```

#### app/providers.js

```js

'use client';
import { Toaster } from 'react-hot-toast'; 

const Providers = ({ children }) => {
  return (
    <>
      <Toaster />
      {children}
    </>
  );
};
export default Providers;

```

##### app/layout.js

```js


import Providers from './providers';

export default function RootLayout({ children }) {
  return (
    <html lang='en'>
      <body className={inter.className}>
        <Navbar />
        <main className='px-8 py-20 max-w-6xl mx-auto'>
          <Providers>{children}</Providers>
        </main>
      </body>
    </html>
  );
}

```



## ROUTES

- By 'Link' Component we can add routes to our page.
- You can use it by importing it from next/link, and passing a href prop to the component:
- HERE, { Clients, Drinks, Prisma, Task,  } IS EXAMPLE OF { ROUTE SEGMENTS FOLDER }.
- THIS FOLDER IS CALLED { ROUTE SEGEMNTS FOLDER }
- Create a folder in the 'app' directory and inside it create a page.js (.jsx) file. The folder name becomes the route segment in the project.

```js
// SIMPLE EXAMPLE TO USE 'LINK' COMPONENTS

"use client";

import Link from "next/link";
import "./globals.css";


async function HomePage(){

    return (
      <div className="homeContent flex items-center justify-center mt-8 flex-col gap-10">
        <h1 className="text-3xl">Get Started with NextJS</h1> 
        <Link href="/clients" className="btn btn-accent">
            <h1>Get Started</h1>
        </Link>        
      </div>
    );
  };
  
  export default HomePage;

```


## LOADING UI (loading.jsx) 

- THIS IS PRESENT INSIDE { ROUTE SEGEMNTS FOLDER  } WHICH WILL AUTOMATICALLY WRAP THE { page.jsx }
- The special file loading.js helps you create meaningful Loading UI with React.
- With this convention, you can show an instant loading state from the server while the content of a route segment loads. The new content is automatically swapped in once rendering is complete.

### SYNTAX FOR   (loading.jsx)
```javascript
function Loading(){

    return (
        <div className="flex items-center justify-center mt-20 mb-20">
            <span className="loading loading-dots loading-lg"></span>
        </div>
         
    );
}

export default Loading;
```



## ERROR HANDLING UI (error.jsx) 

- THE SIMPLE EXAMPLE OF HANDLING THE ERROR BY USING { error.jsx } FILE IS :==>


```javascript
"use client";

function error(error){

    return (
        <div className="flex items-center justify-center mt-28">
            {/* <h1>There was an {error}...</h1> */}
            <h1 className="text-3xl">{error.error.message}</h1>
        </div>
    );
}
export default error;

```



## NESTED LAYOUTS 

- THIS IS THE SIMPLE EXAMPLE OF NESTED LAYOUTS
- { /Drinks/layout.jsx  ||  /Drinks/loading.jsx  ||  /Drinks/error.jsx || /Drinks/page.jsx }   THIS IS THE EXAMPLE OF NESTED LAYOUTS
- NESTED Layouts preserve state, remain interactive, and do not re-render. Layouts can also be nested.


```javascript
function DrinksLayoutPage({children}){
    return (
            <h1 className="text-2xl mb-11">Drinks Layout Page</h1>
            {children}
    );
} 

export default DrinksLayoutPage;
```




## SERVER COMPONENTS 

- BY DEFAULT, NEXTJS USES SERVER COMPONENTS !!!!
- Fetching Data: They can fetch data directly from the server, avoiding the need for additional API calls from the client.
- Rendering on the Server: Server components are rendered on the server, which means they don't include client-side JavaScript in the final output sent to the browser.
- BASICALLY, ALL THE HTTPS REQUEST { GET, POST, PUT, PATCH, DELETE } ARE NOTHING BUT SERVER COMPONENTS.


## CLIENT COMPONENTS 

- Client Components allow you to write interactive UI that is prerendered on the server and can use client JavaScript to run in the browser.
- Interactivity: Client Components are nothing but use state, effects, and event listeners, meaning they can provide immediate feedback to the user and update the UI.
- Browser APIs: Client Components have access to browser APIs
- To use Client Components, you can add the React "use client" directive at the top of a file, above your imports.
- "use client" is used to declare a boundary between a Server and Client Component modules.




## Fetch Data in Server Components 🚀🚀🚀

- just add async and start using await 
- RATHER, WE CAN USE {AXIOS} FOR FETCHING AN API'S 
- the same for db



// THIS IS THE SIMPLE AXIOS METHOD TO FETCH THE API.

```javascript
import axios from "axios";

async function fetchData(){
  await new Promise((resolve) => {setTimeout(resolve,1000)});
  try{
    let response = await axios.get(url);
    let result = response.data; 
    console.log(result);
    return result;
  }
  catch(error){
    throw new Error("Failed to fetch the APIs")
  };

}

async function HandleFetchData(){
  let result = await fetchData();
  return (
    <h1>{result.content}</h1>
  );
}
```



## DYNAMIC ROUTES  

- A Dynamic Segment can be created by wrapping a folder's name in square brackets: [folderName]. For example, [id]
- Dynamic Segments are passed as the {params} prop to layout, page, route  functions.
- For example, a { Drinks } could include the following route {  app/Drinks/[id]/page.jsx  } where [id] is the Dynamic Segment for Drinks Lists.


// THIS IS AN SIMPLE EXAMPLE OF DYNAMIC ROUTING.

##### app/Drinks/[id]/page.jsx

```javascript


function DynamicDrinksPage({ params }){
  console.log(params);

  return (
    <h1>{params.id}</h1>
  );
}
```




## PRISMA  💰

- Prisma ORM is a database toolkit that simplifies database access in web applications.
- It allows developers to interact with databases using a type-safe and auto-generated API, making database operations easier and more secure.

1. Prisma server: A standalone infrastructure component sitting on top of your database.
2. Prisma client: An auto-generated library that connects to the Prisma server and lets you read, write and stream data in your database. It is used for data access in your applications.




### INSTALLATION 

```javascript
npm i prisma --save-dev
npm i prisma @prisma/client
npx prisma init
```

- This creates a new prisma directory with your Prisma schema file and configures SQLite as your database



### SETUP PRISMA {GENERATOR AND DATASOURCE} 

- Now, inside { schema.prisma } Setup and modify the { generator and datasource }


```javascript

generator client {
  provider = "prisma-client-js"
  previewFeatures = ["prismaSchemaFolder"]  
}

datasource db {
  provider = "sqlite"
  url      = env("DATABASE_URL")
}

```


#### .env in .gitignore 

```javascript
// .env
DATABASE_URL="file:./dev.db"      

```
- ADD  { .env } IN { .gitignore }
- This in turn initializes a new PrismaClient instance each time due to hot reloading that creates a connection to the database.
- CREATE NEW FOLDER { utils } and THEN ADD { db.ts } FILE INSIDE AN FOLDER.


#### {utils/db.ts} 

```javascript

import { PrismaClient } from '@prisma/client';

const prismaClientSingleton = () => {
  return new PrismaClient()
}

declare const globalThis: {
  prismaGlobal: ReturnType<typeof prismaClientSingleton>;
} & typeof global;

const prisma = globalThis.prismaGlobal ?? prismaClientSingleton()

export default prisma

if (process.env.NODE_ENV != 'production') globalThis.prismaGlobal = prisma

```



### PRISMA MODEL 

- Prisma models are defined in the Prisma schema
- the term "model" refers to an abstraction that maps to a table in your database.
- Each model in the Prisma schema corresponds to a table in the database,
- and each field within a model corresponds to a column in that table


```javascript

model Task {
  id Int @id @default(autoincrement())
  content String
  createdAt DateTime @default(now())
  completed Boolean @default(true)
}

```


## RUNNING LOCALLY 

```javascript

// WHENEVER WE MAKE CHANGES IN OUR SCHEMA RUN THE FOLLOWING CODE :=
npx prisma migrate dev

// WHENEVER WE DEPLOY AND MIGRATE OUR DATABASE ON SUPABASE. FOR MIGRATION CHANGES RUN,
npx prisma migrate dev --preview-feature


// Prisma Studio is a visual editor for the data in your database.
npx prisma studio

https://localhost:5555
```


### HOST OUR DATABASE ON CLOUD   (Render OR PlanetScale)

```javascript

WE WILL HOST OUR DATABASE ON CLOUD 
set DATABASE_URL in .env

'Render' will provide you an external URL update your 'DATABASE_URL' in .env
CHANGE PROVIDERS TO "postgresql" 

```

```javascript
// When done with URL. Run the following code 
npx prisma db push
```

## CONNECT OUR DATABASE WITH FRONTEND AND BACKEND 

- INSIDE THE { page.jsx }



```javascript
import prisma from "@/utils/db";

async function prismaHandlers(){
    try{
       await prisma.task.create({
        data:{
            content:'Hey Just trying CRUD operation'
        }
    })
 
    let allTasks = await prisma.task.findMany({
        orderBy:{
            createdAt:"asc"
        }
    });
    return allTasks; 
    }
    catch(error){
        throw new Error("Failed to connect with DataBAse")
    }
    
}

async function prismaPage(){

  let result = await prismaHandlers();
  return (
    console.log(result);
    <h1>{result.content}</h1>
  );

}
```

### CRUD OPERATION 

**REFER {prisma.io} FOR MORE INFORMATION OF CRUD OPERATION AND PRISMA.**


### CREATE OPERATION

```javascript
import prisma from "@/utils/db"; 
import {revalidatePath} from "next/cache"; 
import { redirect } from "next/navigation";  

async function handleAction(formData){
    "use server"
    let content = formData.get(' inputName ');
    console.log(content);

    try{
       await prisma.tableName.create({
        data:{
            content,
        }
    });
    revalidatePath('/'); 
    }
    catch(error){
        throw new Error("Failed to create task");
    }
}

async function createTask(){

    return (
        <div>
            <form action={handleAction}>
                <input name="inputName" placeholder="enter task" required>
                <button type="submit" value="Create Task..."/>
            </form>
        </div>
    );
}
```
- 'use server' directive is used to store the user input data.
- 'formData' can be used only inside the 'use server' directive
- 'form' tag should take an attribute of 'action' and passed the function 'handleAction'



### READ OPERATION

```javascript

import prisma from "@/utils/db.ts";

async function handleAction(){
    try{
        let result = await prisma.tableName.findMany({
        where:{
            id:3
        },
        select:{
              column1Name:true,
              column2Name:true
           }
    });
    return result;
    }
    catch(error){
        throw new Error("Email already exist")
    };
};

async function readTask(){

    let result = await handleAction();
    return (
        <div>
            <h1>{result.column1Name}</h1>
            <h1>{result.column2Name}</h1>
        </div>
    );
}
```

- By 'findMany' we can get all the tasks as return.
- We also have 'findUnique' method and just passed the 'where' condition.



### UPDATE OPERATION

```javascript

import prisma from "@/utils/db"; 
import {revalidatePath} from "next/cache"; 
import { redirect } from "next/navigation";  


async function handleAction(formData){
    "use server"
    let editTask = formData.get('editContent');
    let id = Number(formData.get('taskId'));

    try{
        await prisma.taskList.update({
        where:{
            id, 
        },
        data:{
            content:editTask,
        }
    });

    redirect('/Task');
    }
    catch(error){
        throw new Error("Failed to edit Task");
    };
    
};


async function editForm({id}){

    return (

        <div>
            <form action={handleAction}>

                <input type="hidden" name="taskId" value={id}/>
                <input
                    name="editContent"
                    type="text" 
                    placeholder="Edit the task" 
                />
                <button type="submit" >Edit Task</button>

            </form>
        </div>
    );
}
```


```javascript
<input type="hidden" name="taskId" value={id}/>
```

- THIS INPUT TAG WHICH IS SET HIDDEN, WILL GET US THE ' taskId ' OF SELECTED TASK.



### DELETE OPERATION


```javascript

import prisma from "@/utils/db"; 
import {revalidatePath} from "next/cache"; 
import { redirect } from "next/navigation";  


async function handleAction(formData){
    "use server" 
    let id = Number(formData.get('taskId'));

    try{
    await prisma.tableName.delete({
        where:{
            id,
        }
    });
    redirect('/Task')
    }
    catch(error){
        throw new Error("failed to delete the task");
    };
};


async function DeleteForm(props){

    return (
        <form action={handleAction}>
            <input type="hidden" name="taskId" value={props.id}/>
            <button type="submit">Delete</button>
        </form>
        
    );
}

```

```javascript
<input type="hidden" name="taskId" value={id}/>
```

- AGAIN, WE HAVE USED THE SAME METHOD OF 'INPUT TAG', WHICH WILL RETURN US THE ' TASKID ' WHICH HAS TO BE DELETED.


## Server Actions

- Server Actions are asynchronous functions that are executed on the server. 
- They can be used in Server and Client Components to handle form submissions and data mutations in Next.js applications.


### FORMS   (formData)

- 'form' element to allow Server Actions to be invoked with the {action} attribute.
- the action automatically receives the FormData object.
- You don't need to use React useState to manage fields, instead, you can extract the data using the native FormData methods.

```javascript

import React from "react";
import {revalidatePath} from "next/cache";
import {redirect} from "next/Navigatiom";

async function handleAction(formData){
    "use server"
    let content = formData.get(' content ');

    // mutate data
    // revalidate cache
    revalidatePath("/")
};

async function taskForm(){

    return (
        <form action={handleAction}>
            <input type="text" name="content" placeholder="Enter the content"/>
            <button type="submit" value="create task"/>
        </form>
    );
}

```


### PENDING STATES  (useFormStatus)

- You can use the React useFormStatus hook to show a pending state while the form is being submitted.
- "useFormStatus" returns the status for a specific <form>, so it must be defined as a child of the <form> element.
- It can be used inside an "client components".

```js

app/components/Button.jsx

'use client'
import { useFormStatus } from 'react-dom'
 
export function SubmitButton() {
  const { pending } = useFormStatus();
 
  return (
    <button type="submit" disabled={pending}>
      {pending ? "Please Wait..." : "Task Created"}
    </button>
  )
};

```

- SubmitButton can then be nested in any form:


### FORM STATE MANAGEMENT   (useFormState)

- Once the fields have been validated on the server, you can return a serializable object in your action
- and use the React useFormState hook to show a message to the user.


```js

utils/action.jsx

'use server'
import prisma from "@/utils/db.ts";
import {revalidatePath} from "next/cache";
 
export async function handleAction(prevState, formData) {
    "use server"
    let content = formData.get(' content ');
    try{
        await prisma.tableName.findMany();
        revalidatePath('/');
        return ({msg:"Success..."});
    }
    catch(error){
        return ({msg:"Error occured"});
    };
};

```
- Then, you can pass your action to the 'useFormState' hook and use the returned state to display an error message.

```js

'use client'
 
import { useFormState } from 'react-dom'
import { handleAction } from '@/app/actions'
import toast from "react-hot-toast";
 
const initialState = {
  message: null,
}
 
export function TaskForm() {
  
  let [state, formAction] = useFormState(handleAction,initialState);

    useEffect(() => {
        if (state.message === 'error') {
          toast.error('there was an error');
          return;
        }
        if (state.message) {
          toast.success('task created....');
        }
      }, [state]);

      return (
        <form action={formAction}>
            <input name="content">
            <button type="submit">
        </form>
      );
}

```


### LOCAL BUILD

##### package.json


```javascript
"build": "npx prisma generate && next build",
```

- START THE 'LOCAL BUILD' WHEN YOUR DB IS HOST ON CLOUD.

```javascript
npm run build
npm start
```

### NOTE 

- WHEN WE ARE BUILDING LOCALLY, CHECK WHICH ROUTE SEGMENT FOLDER IS STATIC OR DYNAMIC.

```javascript
○  (Static)   prerendered as static content
ƒ  (Dynamic)  server-rendered on demand
```


- IF WE NEED TO CHANGE ANY ROUTE SEGMENT FOLDER TO DYNAMIC.
- THEN INSERT THE FOLLOWING CODE IN  'page.jsx'

##### page.jsx

```js

export const dynamic = 'force-dynamic';
```

## DEPLOY  (VERCEL) 🌐

- sign up for account
- create github repo
- IMPORT THE GITHUB REPO TO HOST OUR WEBAPP

- COMMIT THE CHANGES, TO APPLY CHANGES IN OUR LIVE APP.


## CLERK AUTHENTICATION

- create account
- create new application
- complete Next.js setup


```js
npm install @clerk/nextjs
```


```js
// CREATE (.env.local) FILE AND UPLOAD NECESSARY VAR PROVIDED BY CLERK

NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY = your_publishable_key;
CLERK_SECRET_KEY = your_secret_key;

```

#### IMPORTANT COMPONENTS FOR AUTHENTICATION

```js
import { SignInButton, UserButton, SignedIn, SignedOut, RedirectToSignUp } from '@clerk/nextjs';
import { currentUser } from "@clerk/nextjs/server";
```

- With this UI components we can check for current user authentication status and can add the sign-in and sign-up buttons.


- Add (.env.local) IN (.gitignore)


### middleware.ts


```js
// app/

import { clerkMiddleware, createRouteMatcher } from "@clerk/nextjs/server";

// public routes in our case '/'
const isPublicRoute = createRouteMatcher(['/']);
const isProtectedRoute = createRouteMatcher([
  '/chat(.*)',
  '/addjob(.*)',
  '/jobs(.*)',
  '/stats(.*)',
]);


export default clerkMiddleware(async (auth, req) => {
  if (!isProtectedRoute(req)) auth().protect();
});

export const config = {
  matcher: ['/((?!.*\\..*|_next).*)', '/', '/(api|trpc)(.*)'],
};


```

### app/layout.js

- Wrap the HTML element inside <ClerkProvider>

```js
// UPDATE layout.js file

import { ClerkProvider } from "@clerk/nextjs";

export default function RootLayout({ children }) {
  return (
    <ClerkProvider>
      <html lang="en">
        <body className={inter.className}>
          <main className="text-white" style={{ fontFamily: "monospace" }}>
            <Providers>{children}</Providers>
          </main>
        </body>
      </html>
    </ClerkProvider>

  );
};

```



## CLERK COMPONENTS AND FUNCTIONS


### CURRENTUSER AND USERBUTTON FUNCTION

```js
import { UserButton } from "@clerk/nextjs";
import { currentUser } from "@clerk/nextjs/server";

async function Navprofile(){

    // THIS WILL RETURN US THE CURRENT USER WHICH IS AUTENTICATED.
    let user = await currentUser();
    console.log(user);
    return (
        <div className="px-4 flex items-center gap-2 relative top-48">
             {/* After SignOut we will redirect to ' redirectUrl '  */}
            <UserButton redirectUrl="/"></UserButton>
            <p className="text-xl text-white">{user.emailAddresses[0].emailAddress}</p>
        </div>
    );
};

export {Navprofile};

```

### UserProfile Page

app/Profile/page.jsx

```js

import { UserProfile } from '@clerk/nextjs';
const UserProfilePage = () => {
  return <UserProfile />;
};
export default UserProfilePage;

```




### THEME TOGGLE USING DAISY UI

- Whenever we will use Shadcn ui it will change the tailwind.config.js and global.css file
- Add daisy ui in tailwind.config file and global.css file.

```js
//  tailwind.config.js

{
  daisyui: {
    themes: ['winter', 'dracula'],
  },
}

// CHECK DAISY UI FOR MORE THEME
```

- Create ThemeToggle.jsx component

```js
//  /components/ThemeToggle.jsx

'use client';
import { BsMoonFill, BsSunFill } from 'react-icons/bs';
import { useState } from 'react';

const themes = {
  winter: 'winter',
  dracula: 'dracula',
};

const ThemeToggle = () => {
  const [theme, setTheme] = useState(themes.winter);

  const toggleTheme = () => {
    const newTheme = theme === themes.winter ? themes.dracula : themes.winter;
    document.documentElement.setAttribute('data-theme', newTheme);
    setTheme(newTheme);
  };

  return (
    <button onClick={toggleTheme} className='btn btn-sm btn-outline'>
      {theme === 'winter' ? (
        <BsMoonFill className='h-4 w-4 ' />
      ) : (
        <BsSunFill className='h-4 w-4' />
      )}
    </button>
  );
};
export default ThemeToggle;


```



### TO CREATE A COMMON (layout.js) FILE FOR ALL ROUTE SEGMENT FOLDER

- Create (app/(dashboard)/layout.jsx). This will be the common layout.js file for all route segment folder.

```js
function LayoutPage({children}){

  return(
    <div>
      {children}
    </div>
  );
}

```



## REACT QUERY AND NEXTJS


### INSTALLATION

```js
npm i @tanstack/react-query @tanstack/react-query-devtools
npm i -D @tanstack/eslint-plugin-query
```


### BASIC SETUP

- Update  app/providers.js file

```JS
// providers.js

'use client';
import { Toaster } from 'react-hot-toast';
import React from 'react'; 
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ReactQueryDevtools } from '@tanstack/react-query-devtools';

function Providers({ children }) {
  const [queryClient] = React.useState(
    () =>
      new QueryClient({
        defaultOptions: {
          queries: {
            // With SSR, we usually want to set some default staleTime
            // above 0 to avoid refetching immediately on the client
            staleTime: 60 * 1000,
          },
        },
      })
  );
  return (
    <QueryClientProvider client={queryClient}>
      <Toaster />
      {children}
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  );
};

export default Providers;

```


### USING useQuery and useMutation HOOKS

- Now to use the 'useQuery' hook, we need to wrap the 'page.jsx' file. Similar to the one shown below.
- The function which we are going to use inside an component function, we need to define the same in 'page.jsx'.
- We can define as many as 'preFetchQuery' we will use.

```js
// app/Chat/page.jsx
'use client'
import React from "react";
import {
    dehydrate,
    HydrationBoundary,
    QueryClient,
    QueryClientProvider,
} from '@tanstack/react-query';
 

async function ChatPage(){

  await new Promise((resolve) => { setTimeout(resolve, 1000) });

    // THIS WILL CREATE NEW QUERY CLIENT.
    const queryClient = new QueryClient();
    
    await queryClient.prefetchQuery({
      // Provide some default values.
      queryKey: ['todos', ''],
      queryFn: () => fetchListItem({}), 
    })
    
    return (
      <QueryClientProvider client={queryClient}>
          <HydrationBoundary state={dehydrate(queryClient)}>
            {/* CONTENT/COMPONENTS SHOULD BE WRAP BETWEEN THIS BOUNDARY. */}
            <Component />
        </HydrationBoundary>
      </QueryClientProvider>
        
    );
}


```


### useQuery Hooks (READ)

- A query can be used with any Promise based method (including GET and POST and FETCHING DATA FROM DATABASE) to fetch data from a server.

- Caching: Automatically caches the fetched data.
- Automatic Refetching: Automatically refetches data when the query key changes or when the component remounts.
- Data Synchronization: Keeps multiple components in sync with the same data.

```js
'use client'
import { useQuery, useQueryClient } from '@tanstack/react-query';


async function Todos() {

  let queryClient = useQueryClient();

  queryClient.invalidateQueries({ queryKey: ['todos'] });

  const { isPending, data,refetch } = useQuery({
    // HERE 'todos' IS NOTHING BUT AN GLOBAL KEY WHICH WE CAN USE IN OTHER QUERY.
    queryKey: ['todos', item],
    queryFn: () => fetchListItem(item), 
    select:(todos)=>{
      todos.map((todo)=>({id:todoId,title:todoTitle}))
    },
    enabled: !!data,
  });

  if (isPending) {
    return <span className="loading loading-dots loading-lg"></span>
  }
  

  // If the query is in an "isSuccess" state, the data is available via the "data" property.
  return (
    <div>
      <ul>
        {data.slice(0,5).map((todo) => (
          return (
            <li key={todo.id}>{todo.title}</li>
          );
        ))}
      </ul>
      <button 
        type="button"
        disabled={isPending}
        value={isPending ? "Please Wait..." : "Add Item"}
      />
    </div>
    
  )
}

export {Todos};
```

- Use **refetch()** inside the **onSuccess()** method to soft referesh the page.
- You can use **slice(0,n)** to boil down the total number of response.

#### queryKey : This helps React Query to uniquely identify and cache the result of the query. This means that if the same queryKey is used again, React Query can return the cached result instead of making a new network request.

#### queryFn : A query function can be literally any function that returns a promise. The promise that is returned should either resolve the data or throw an error. 'The fetch data is stored in data variable.'




### useMutation Hooks  (CREATE/UPDATE/DELETE)

- Mutations are typically used to create/update/delete data or perform server side-effects. 
- It is primarily used for write operations where you are sending data to a server.

```js
'use client';
import { useMutation, useQueryClient } from "@tanstack/react-query";
import toast from "react-hot-toast";
import {createItem} from "../utils/action.jsx";

function TodosPage(){

  let [item, setItem] = useState('');
  let queryClient = useQueryClient();

  let {mutate, isPending, data} = useMutation({
    mutationFn: async (item) => await createItem(item),
    onSuccess: (data)=>{
      if(!data){
        toast.error("An error occured");
      }
      queryClient.invalidateQueries({ queryKey: [''] });
    },
  });

  const handleSubmit = (e) => {
    e.preventDefault();
    setItem(e.target.value);
    mutate(item);
  };


  return (
    <div>
      {
        data.map((item)=>{
          return (
            <h1>{item.content}</h1>
          );
        })
      }
      <form onSubmit={handleSubmit}>
      <input type='input' />
      <button 
        type="button"
        disabled={isPending}
        value={isPending ? "Please Wait..." : "Add Item"}
      />
      </form>

    </div>

  );
}

```

- The most useful and popular use of useMutation is its async nature.
- We Know that we cannot use react hooks inside server components. Hence, we will declare 'use client' directive at top.
- With the help of 'useMutation' we can use  server-side logic or components.

**Note :** Use invalidateQueries to load the data quickly and to improve the UX. 


### SEARCH FUNCTIONALITY(READING) USING REACT QUERY

- Here we can use 'useQuery' Hooks to add search functionality in our web application.
- We can set an input form for which we are searching an term.

- We will use 'Combining operators' to search for an item.
- [OR, NOT]

```js
// utils/action.jsx

async function getAllListItem(searchTerm) {
    if (!searchTerm) {
      // LISTING ALL ITEMS
        let items = await prisma.tour.findMany({
            orderBy: {
                item: 'asc',
            }
        });
        return items;
    }
  
    let items = await prisma.tour.findMany({
        where: {
            OR: [
                {
                  modelItem1: {
                        contains: searchTerm
                  },
                  modelItem2: {
                        contains: searchTerm
                  },
                }, 
            ]
        },
        orderBy: {
            item: 'asc'
        }
    });
    return items;
}

This utility func will return all list item.

// THIS IS COMPONENT FOR OUR SEARCH FUNCTIONALITY. 

'use client';
import { useQuery } from '@tanstack/react-query';
import {getAllListItem} from "../utils/action.jsx";
import {displayItemList} from "../components/displayItemList";

function ItemPage(){

  let [search, setSearch] = React.useState('');
  let { data, isPending } = useQuery({
    queryKey: ['items', search],
    queryFn: () => getAllListItem(search), 
  }); 


  return (
    <div>
      <form> 
        <div className='join mt-4 mb-16 w-full'>
          <input 
            type="text" 
            placeholder="Enter Item..." 
            className="input input-bordered w-full max-w-xs join-item" 
            value={search}
            onChange={(e)=>{setSearch(e.target.value)}}
          />
          <button 
            type='button' 
            className='btn join-item btn-primary'
            disabled={isPending}
            onClick={()=>{setSearch('')}}
          >
            {isPending ? "Please wait..." : "Search"}
          </button>
        </div>
      </form>
      {
        isPending ? <span className="loading loading-dots loading-lg"></span> : <displayItemList data={data}></displayItemList>
      }
    </div>
  )

}

```




## INTRODUCTION TO GENAI.


- AI :- It is the theory and development of computer system able to perform tasks normally requiring human intelligence.
- ML :- It is the sub-field of AI, which trains a model from input data. And, the trained model can make useful prediction from new or never-before-seen data drawn from the same one used to train model.
- DL :- It uses Artificial neural network - allowing them to process more complex patterns than traditional machine learning.



#### DEEP LEARNING MODEL TYPES

- "Discriminative Model" :-  This will Describe, Detect and Classify the input Data.

- "Generative Model" :-  This will Create, Generate the new content for input Data.


### GENERATIVE AI 

- GenAI is a subset of Deep Learning.

<!-- - It uses Artificial neural network and can process both labelled and unlabelled data using supervise, un-supervise and semi-supervise methods. -->

- It is a type of AI that creates new content based on what it has learned from existing content.

<!-- - Generative language model ingest very large data from multiple source across internet and create new content. -->

- GenAI depends a lot on the training data that we provide. 

- It analysis the patterns and structure of input data and thus learns.

- GenAI uses "Foundational and Statistical Model".


##### Types of Model :-

Generative Language model (LLM) : Learn about patterns through training data.
- Takes Text as input and generate Text,Image,Audio and Decision.

Generative Image model : This use Diffusion technique to generate new Images.
- Takes image as input and generate Text,Image,Video.


- The power of GenAI comes from the use of 'Transformers'.
- Transformers consist of Encoder and Decoder.
- 'Encoder' encode the input sequence and passes it to 'Decoder' and which learns how to decode the representation for a task.


- "Prompt" is a short piece of text that is given to LLM as input and it can be used to control the output of models in variety of ways.


#### LANGCHAIN

- "LangChain" is a framework for developing GenAI applications powered by large language models (LLMs)/ Open source language model.

- LangSmith(Dashboard) :- A developer platform that lets you debug, test, evaluate, and monitor LLM applications.
- LangServe :- Deploy LangChain chains as REST APIs.
- LangGraph.js :-  Build robust and stateful multi-actor applications with LLMs by modeling steps as edges and nodes in a graph.

OpenSource Model
Paid Source Model


#### LARGE LANGUAGE MODEL (LLMS).

- LLM are the subset of Deep learning.

- An LLM is a type of artificial intelligence (AI) that can generate human-quality text. 
- LLMs are trained on massive datasets of text and code, and they can be used for many tasks, such as writing, translating, and coding.

- LLM refer to large, general-purpose language model that can be 'pre-trained' and then 'fine-tuned' for specific purpose.

- Pre-trained  :-  We will pre-train a LLM for a general pupose with a large dataset 
- Fine-tunning  :-  Then fine-tuned it  with specific aims  with a much smaller data set

- Fine-tuning in (GenAI) involves taking a pre-trained model (Gemini/OpenAI) and training it further on a specific dataset to adapt it to a particular task or domain.






 
### RAG PIPELINE.  (RETRIEVEL-AUGMENTED-GENERATION).

- **RAG** is a technique of adding or enhancing LLM knowledge with additional dataset and Generating response on the basis of dataset.
- Here we will build an **QnA Chatbot using RAG Pipeline, LangChain and LLM's**



#### TYPICAL RAG PIPELINE APPLICATION

1. **Indexing(pipeline)** - The Pipeline  in which we will Load source data , split it in small chunks , convert chunks in vector and storing it in vectorDB/vectorStore.

2. **Retrieval and generation (Chain)** - The Chain will takes User query , Extract relevant Data from source, Generating prompt on basis of question and Retrive data and Passes it to Model(LLMs).





#### RAG PIPELINE ARCHITECTURE.

1. **INDEXING METHOD**

- **Load** - We need to load our data. This is done with **Document Loaders**

- **Split** - **TextSplitters** break large document in small chunks. This is useful for indexing data and passing it in to a model. We will also consider the context window for LLM.

- **Embed** - Now we will convert the small chunks into vector using **Embedding models**
- **Store** - Lastly we will store the vector in **VectorStore/VectorDB**


2. **Retrieval and generation**

- **Retrievel** - Given user input, **Retriever** will extract the relevant data from database.

- **Generate** - **ChatModels/LLM** will take prompt which include question n retrieved data and will provide answer.





#### METHODS AND MODEL USED IN RAG PIPELINE WITH CODE.


0. **DECLARING MODEL**
```js
import { ChatGroq } from "@langchain/groq";
import "cheerio";
const llm = new ChatGroq({
            model: "mixtral-8x7b-32768",
            temperature: 0
});
```



1. **Document Loaders** are responsible for loading documents from a variety of sources.
- WebBasedLoader , PDFLoader
- Here, I can also use PDF folder present in our Root directory to enhance the model.

```js
import { CheerioWebBaseLoader } from "@langchain/community/document_loaders/web/cheerio";
const loader = new CheerioWebBaseLoader(
  "https://lilianweng.github.io/posts/2023-06-23-agent/",
);
const docs = await loader.load();
console.log(docs[0].pageContent);
```



2. **Text Splitters** encode large document and split into small chunks that can be used for retrieval. Small chunks will be suitable for context window of chatmodel.
- Recursively Split Text


```js
import { RecursiveCharacterTextSplitter } from "langchain/text_splitter";
const textSplitter = new RecursiveCharacterTextSplitter({
  chunkSize: 1000,
  chunkOverlap: 200,
});
const allSplits = await textSplitter.splitDocuments(docs);
console.log(allSplits.length);
```


3. **Embedding Models** takes small chunks of docs and convert them in a Vector representation of it.
- OpenAIEmbedding Model , GooglePalmEmbeddings , OllamaEmbeddings , etc.

4. **Vector stores** are databases that can efficiently store and retrieve embeddings.
- Memory / ChromaDB / Faiss / Prisma


```js
import { MemoryVectorStore } from "langchain/vectorstores/memory";
import { OllamaEmbeddings } from "@langchain/openai";
const vectorStore = await MemoryVectorStore.fromDocuments(
  allSplits,
  new OllamaEmbeddings(),
);
```


5. **Retriever**  will encode the userQuery, Extract relevant data from DataStore. Then convert the userquery and extracted data into prompt.

```js
const retriever = vectorStore.asRetriever({ k: 6, searchType: "similarity" });
```



6. **ChatModels/LLM** This is our language model which will encode prompt and generate the response on the basis of data.
- OpenSourceModel / PaidSourceModel

```js
import { PromptTemplate } from "@langchain/core/prompts";
import { createStuffDocumentsChain } from "langchain/chains/combine_documents";
import { StringOutputParser } from "@langchain/core/output_parsers";

const template = `Use the following pieces of context to answer the question at the end.
If you don't know the answer, just say that you don't know, don't try to make up an answer.
Use three sentences maximum and keep the answer as concise as possible.
Always say "thanks for asking!" at the end of the answer.

{context}

Question: {question}

Helpful Answer:`;

const customRagPrompt = PromptTemplate.fromTemplate(template);
const ragChain = await createStuffDocumentsChain({
  llm,
  prompt: customRagPrompt,
  outputParser: new StringOutputParser(),
});
const context = await retriever.invoke("what is task decomposition");
let result = await ragChain.invoke({
  question: "What is Task Decomposition?",
  context,
});
console.log(result);
```



## CHAT APPLICATION USING LANGCHAIN (LOGIC AND FRONTEND PART).

#### Frontend logic to handle the response from LLM.
- Here first we will understand the frontend part to handle the response from our LLM and logic behind it.
- We can use 'useMutation' or 'useQuery' hook to handle the server-comp inside an client-comp.


```js

'use client'
import React, { ChangeEvent, FormEvent, useState } from "react";
import { useMutation } from "@tanstack/react-query";
import toast from "react-hot-toast";
import { getchatResponse } from "../utils/action";


const Chat = () => {
    // state management to handle the response from llm
    const [text, setText] = useState("");
    const [messages, setMessages] = useState([]);

    // Here, we have used useMutation hook to handle SSC inside an CSC.
    const { mutate: mutateFunc, isPending, data } = useMutation({
        // mutate func will update our getchatResponse Function.
        mutationFn: async (text: string) => await getchatResponse(text),
        onSuccess: (data) => {
            // Our toast component for success and error message notification
            if (!data) {
                toast.error("An error occured");
            }
            else {
                toast.success("Success");
            }
            // Assuming result is the key holding the chat response
            let result = data.content.parts[0].text;
            // Storing response from our LLM in our state
            setMessages((prevMessages) => [...prevMessages, { role: 'bot', parts: [{ text: result }] }]);
        }
    });

    function handleSubmit(e: FormEvent<HTMLFormElement>) {
        e.preventDefault();
        // Here storing our User question
        setMessages((prevMessages) => [...prevMessages, { role: 'user', parts: [{ text: text }] }]);
        // updating our getchatResponse() function.
        mutateFunc(text);
        setText("");
        console.log(messages);
    };

    // 
    const handleInput = (e: ChangeEvent<HTMLInputElement>) => {
        // Storing our question in state.
        setText(e.target.value);
    };

    return (
        <div className="min-h-[calc(100vh-6rem)] grid grid-rows-[1fr,auto]">
            <div>
                <h1 className="text-3xl relative bottom-5">An Virtual AI Assistant</h1>
                // Here this is an array state of response and questions from our LLM, which we will display as a single box.
                {
                    messages.map((message, index) => {
                        // Avatar on the basis of response from LLM or user.
                        let avatar = message.role === 'user' ? '👨🏽' : '🤖';
                        let bcg = message.role === 'user' ? "bg-base-200 shadow-lg" : "bg-base-100 shadow-lg";

                        // Function to process the message text
                        const renderText = (text: string) => {
                            const parts = text.split('**');
                            return parts.map((part, idx) => {
                                // Here we have format the response on the basis of ** and *.
                                // Alternate between <h1> and <p> based on the split index
                                if (idx % 2 === 1) {
                                    return <h1 key={idx} className="font-extrabold">{part}</h1>;
                                } else {
                                    return <p key={idx} className="font-light font-mono">{part}</p>;
                                }
                            });
                        };

                        return (
                            <div key={index} className={`flex flex-row mt-6 leading-loose border-b border-base-300 ${bcg}`}>
                                <p className="m-3">{avatar}</p>
                                <div className="m-3">
                                    {renderText(message.parts[0].text)}
                                </div>
                            </div>
                        );
                    })
                }

                // Loading div component on submit to handle the response.
                <div className="m-10">
                    {isPending ? <span className="loading"></span> : null}
                </div>
            </div>
            // Form for submission of question and response.
            <form className="join relative mb-24" onSubmit={handleSubmit}>
                <div className="join w-full absolute bottom-auto mt-5">
                    <input
                        type="text"
                        placeholder="Message VoyageVision"
                        className="input input-bordered join-item w-full"
                        name="text"
                        value={text}
                        onChange={handleInput}
                    />
                    <button
                        type="submit"
                        className="btn btn-primary w-32 join-item rounded-2xl mb-5 font-bold"
                        disabled={isPending}

                    >
                        {isPending ? "Please Wait..." : "Ask"}
                    </button>
                </div>
            </form>
        </div>
    )
}

export { Chat };

```


#### Server side rendering to fetch the response from LLM.

- Here we can either use Langchain and LLMs or Google Gemini as a our pre-trained model.


```js

"use server"
import React from "react";
// For google gemini llm
import { GoogleGenerativeAI } from "@google/generative-ai";
// for langchain and opensource llms
import { ChatGroq } from "@langchain/groq";
import { InMemoryChatMessageHistory } from "@langchain/core/chat_history";
import { ChatPromptTemplate } from "@langchain/core/prompts";
import { RunnableWithMessageHistory } from "@langchain/core/runnables";
import { HumanMessage } from "@langchain/core/messages";
import { AIMessage } from "@langchain/core/messages";
// for fine-tunning an pre-trained model on the large datasets.
import { ChatGroq } from "@langchain/groq";
import "cheerio";
import { CheerioWebBaseLoader } from "@langchain/community/document_loaders/web/cheerio";
import { RecursiveCharacterTextSplitter } from "langchain/text_splitter";
import { GooglePaLMEmbeddings } from "@langchain/community/embeddings/googlepalm";
import { MemoryVectorStore } from "langchain/vectorstores/memory";
import { PromptTemplate } from "@langchain/core/prompts";
import { createStuffDocumentsChain } from "langchain/chains/combine_documents";
import { StringOutputParser } from "@langchain/core/output_parsers";




// THIS IS AN OPEN SOURCE MODEL (GOOGLE GEMINI) API WHICH IS USED FOR SIMPLE VIRTUAL CHAT ASSISTANT.
export async function googleGeminiChat(prompt: string) {
  // Access your API key as an environment variable
  const genAI = new GoogleGenerativeAI(process.env.API_KEY); 
  try {
    // Choose a model that's appropriate for your use case.
    const model = genAI.getGenerativeModel({ model: "gemini-1.5-flash" }); 

    const result = await model.generateContent({
      contents: [
        {
          role: 'user',
          parts: [
            {
              text: prompt,
            }
          ],
        }
      ],
      generationConfig: {
        maxOutputTokens: 1000,
        temperature: 0.1,
      },
    });
    // console.log(result.response.text());
    console.log(result.response.candidates[0].content.parts[0].text);
    return (result.response.candidates[0]);
  }
  catch (error) {
    console.log(error);
  }
};


// THIS USES AN PRE-TRAINED MODEL AND STORE THE MESSAGE HISTORY OF THE USER AND REGARDING OUR APP.
export async function langchainChat(text: string) {
  try {
    // LANGUAGE MODEL TO BE USED.
    const model = new ChatGroq({
      model: "mixtral-8x7b-32768",
      temperature: 0
    });
    // THIS IS AN JS OBJECT WHICH WILL STORE OUR MESSAGE HISTORY.
    const messageHistories: Record<string, InMemoryChatMessageHistory> = {};

    // THIS IS OUR TEMPLATE TO GENERATE THE RESPONSE.
    const prompt = ChatPromptTemplate.fromMessages([
      [
        "system",
        `You are a helpful assistant who remembers all details the user shares with you.`,
      ],
      ["placeholder", "{chat_history}"],
      ["human", "{input}"],
    ]);

    // THIS IS USED TO CONNECT OUR LLM MODEL WITH OUTPUT PARSER
    const chain = prompt.pipe(model);

    // HERE WE CAN PROVIDE SPECIFIC DETAILS RELATED TO US OR FOR THIS APP.
    const messages = [
      new HumanMessage({ content: "hi! I'm anurag" }),
      new AIMessage({ content: "hi!" }),
    ];

    // THIS FUNCTION WILL ENCODE THE RESPONSE FROM AI AS WELL THE INPUT FROM USER. THIS FUNC REQUIRED THE SESSION ID.
    const withMessageHistory = new RunnableWithMessageHistory({
      runnable: chain,
      getMessageHistory: async (sessionId) => {
        if (messageHistories[sessionId] === undefined) {
          const messageHistory = new InMemoryChatMessageHistory();
          await messageHistory.addMessages(messages);
          messageHistories[sessionId] = messageHistory;
        }
        return messageHistories[sessionId];
      },
      inputMessagesKey: "input",
      historyMessagesKey: "chat_history",
    });


    // IF WE HAVE AN APP WHICH OPERATES MULTIPLE USER IN SAME APP. WE CAN SAVE THEIR HISTORY WITH THE UNIQUE SESSION ID.
    const config = {
      configurable: {
        sessionId: "abc2",
      },
    };


    // HERE, WE WILL GET THE RESPONSE FROM AI.
    const response = await withMessageHistory.invoke(
      {
        input: text,
      },
      config
    );
    console.log(response.content);
    return (response.content);
  }
  catch (error) {
    console.log(error);
  };


};


// THIS IS USED TO FINE-TUNED AN MODEL ON THE LARGE DATASET.
export async function PdfParser(){

    try {
        // LANGUAGE MODEL TO BE USED.
        const llm = new ChatGroq({
            model: "mixtral-8x7b-32768",
            temperature: 0
        });


        // TO LOAD THE DATA FROM DATA SOURCE WE WILL USE 'DOCUMENTLOADERS' 
        const loader = new CheerioWebBaseLoader(
            "https://lilianweng.github.io/posts/2023-06-23-agent/",

        );
        const docs = await loader.load();
        // console.log(docs);
        



        // NOW, THE LOADED DOCUMENT FROM DATASOURCE WILL BE SPLITTED INTO SMALL CHUNKS, SO THAT IT WILL APPLICABLE TO CONTEXT WINDOW OF LLM.
        // WE WILL USE 'RecursiveCharacterTextSplitter'
        const textSplitter = new RecursiveCharacterTextSplitter({
            chunkSize: 1000,
            chunkOverlap: 200,
        });
        const allSplits = await textSplitter.splitDocuments(docs);



        // NOW WE WILL CONVERT SMALL CHUNKS INTO VECTOR NUM USING 'EMBEDDINGS MODEL' AND STORE VECTOR IN 'VECTOR DB/VECTOR STORE'.
        const vectorStore = await MemoryVectorStore.fromDocuments(
            allSplits,
            new GooglePaLMEmbeddings(),
        );


        // NOW USING 'RETRIEVER' WE WILL EXTRACT THE MOST RELEVANT DATA FROM DATASOURCE.
        const retriever = vectorStore.asRetriever({ k: 6, searchType: "similarity" });


        //  A chain that takes a question, retrieves relevant documents, constructs a prompt, passes that to a ChatModel/LLMs, and parses the output. 
        const template = `Use the following pieces of context to answer the question at the end.
    If you don't know the answer, just say that you don't know, don't try to make up an answer.
    Use three sentences maximum and keep the answer as concise as possible.
    Always say "thanks for asking!" at the end of the answer.

    {context}

    Question: {question}

    Helpful Answer:`;

        const customRagPrompt = PromptTemplate.fromTemplate(template);
        const ragChain = await createStuffDocumentsChain({
            llm,
            prompt: customRagPrompt,
            outputParser: new StringOutputParser(),
        });
        const context = await retriever.invoke("What are types of memory?");
        let result = await ragChain.invoke({
            question: "What are types of memory?",
            context,
        });
        // console.log(result);



    } catch (error) {
        console.log(error);

    }
```


## FORM MANAGEMENT


### SIMPLE FORM STATE MANAGEMENT USING REACT-HOOK-FORM.

- Here we will use the react-hook-form to handle the form input for multiple input.
- Commonly we use useState Hook or formData method, but react support an library which will return the input values in an object.


- Install the library
```js
npm install react-hook-form

```

- Usage of react-hook-form

```js

import {useForm} from "react-hook-form";

export function inputData(){


  let {
    register,
    watch,
    handleSubmit
  } = useForm();

  function onSubmit(data){
    console.log(data);
    return data;
  };

  return (
    <div>
      <form onSubmit={handleSubmit(onSubmit)}>
          // Handle username
          <input type='text' placeholder='Enter username' {...register{'username'}}/>
          // Handle email
          <input type='text' placeholder='Enter email' {...register{'email'}}/>
          // Handle address
          <input type='text' placeholder='Enter address' {...register{'address'}}/>
          // This is button to submit the form.
          <button type='submit' className='btn btn-lg text-base-300 bg-base-900'>Add Data</button>
      </form>
    </div>
  );

};


// Example output in console.
{username:'rohit', email:'xyz@gmail.com', address:'mumbai'}
```

- TYPESCRIPT+ZOD , REACT-HOOK-FORM+TYPESCRIPT , TANSTACK-USEFORM



### FORM MANAGEMENT USING ZOD AND REACT-HOOK-FORM IN TYPESCRIPT.

- Add the form component from shadcn ui.

```js
npx shadcn-ui@latest add form
```
- Now we will look for an single input value and how to handle it using 'zod and react-hook-form'



1. Create a Form schema as an object using zod (basically declaring our input's name).


```js
import * as z from "zod";

// Defining an formSchema
export const formSchema = z.object({
  username: z.string().min(2, {message:"It should be maximum of two characters"}),
});
```


2. We will use 'useForm' Hook to declare the 'form' and default value for our input.

```js
import { zodResolver } from "@hookform/resolvers/zod";
import { useForm } from "react-hook-form";

// Define an form.
const form = useForm<z.infer<typeof formSchema>>({
    resolver: zodResolver(formSchema),
    defaultValues: {
      username: "",
    },
  });
```


3. We will define an 'onSubmit' Function similar to 'useForm' Hook.

```js
// Define a submit handler.
function onSubmit(values:z.infer<typeof formSchema>){
  console.log(values);
}
```
4. Finally, following 'Form' component of shadcn ui and 'form' tag to define our form.


```js
import {
  Form,
  FormControl,
  FormDescription,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from "@/components/ui/form";
import { Input } from "@/components/ui/input";

return (
  <Form {...form}>
    <form 
      className="bg-base-100 text-white"
      onSubmit={form.handleSubmit(onSubmit)}
    >
    <FormField
          control={form.control}
          name="username"
          render={({ field }) => (
            <FormItem>
              <FormLabel className="capitalize">Username</FormLabel>
              <FormControl>
                <Input  {...field} className="input bg-base-100 text-neutral-900"/>
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
    </form>
  </Form>
);
```


5. The full code for input management using zod and react-hook-form.


```js

import * as z from "zod";
import { zodResolver } from "@hookform/resolvers/zod";
import { useForm } from "react-hook-form";

import {
  Form,
  FormControl,
  FormDescription,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from "@/components/ui/form";
import { Input } from "@/components/ui/input";


// Defining an formSchema
export const formSchema = z.object({
  username: z.string().min(2, {message:"It should be maximum of two characters"}),
});


export function handleInput(){

// Define an form.
const form = useForm<z.infer<typeof formSchema>>({
      resolver: zodResolver(formSchema),
      defaultValues: {
        username: "",
    },
  });

// Define a submit handler.
function onSubmit(values:z.infer<typeof formSchema>){
  console.log(values);
};


return (
  <Form {...form}>
    <form 
      className="bg-base-100 text-white"
      onSubmit={form.handleSubmit(onSubmit)}
    >
    <FormField
          control={form.control}
          name="username"
          render={({ field }) => (
            <FormItem>
              <FormLabel className="capitalize">Username</FormLabel>
              <FormControl>
                <Input  {...field} className="input bg-base-100 text-neutral-900"/>
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <button type='submit' className='btn btn-lg text-neutral-900' >Add</button>
    </form>
  </Form>
  )
};

```



### If we have more than one input value to be declare.

- The procedure will be same from declaring an schema to default value.
- We need to declare a 'formField' components and used it in our main function.



```js
// components/FormComponent.tsx

import {
    FormField,
    FormItem,
    FormMessage,
    FormLabel,
    FormControl
} from '../@/components/ui/form'
import { Input } from '../@/components/ui/input';


export function customFormField({name,control}){

  return (
      <FormField
          control={control}
          name={name}
          render={({ field }) => (
            <FormItem>
              <FormLabel className="capitalize">Username</FormLabel>
              <FormControl>
                <Input  {...field} className="input bg-base-100 text-neutral-900"/>
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
  );
};

```

- Import the 'customFormField' component in our main function.

```js

import {customFormField} from "./components/customFormField.tsx";
import * as z from "zod";

const formSchema = z.object({
  username: z.string().min(2),
  email: z.string().min(2),
  location: z.string().min(2),
})

function handleInput(){
  // Define an form.
  const form = useForm<z.infer<typeof formSchema>>({
      resolver: zodResolver(formSchema),
      defaultValues: {
        username: "",
        email: "",
        location:"",
    },
  });

  function onSubmit(values:z.infer<typeof formSchema>){
    console.log(values);
  };

  return (
      <div>
        <Form {...form}>
          <form 
            onSubmit={form.handleSubmit(onSubmit)}
            className="bg-red-900"
          >
          <div>
            // For username
            <customFormField  name='username' control={form.control}/>
            // For email
            <customFormField  name='email' control={form.control}/>
            // For location
            <customFormField  name='location' control={form.control}/>
          </div>
          <button type="submit">Add</button>
          </form>
        </Form>
      </div>
  );
};
```


- We can now use multiple input for our form submission using zod and react-hook-form.



## USER-ID USING CLERK.

- We can use clerk to get the user-id so we can make an multi-user platform.

```js
import {auth} from "@clerk/nextjs/server";

function getClerkId(){
  let {userId} = auth();
  if(!userId){
    throw new Error("User is not authenticated");
  }
  return userId;
};

// This will give u the ID.
let userId = getClerkId();

```




####  useSearchParams

- 'useSearchParams' is a Client Component hook that lets you read the current URL's query string.


```js

import { useSearchParams } from 'next/navigation';

export function searchForm(){

  // URL :-  https://localhost:3000/Job?search=my-project+jobstatus=declined
  let searchParams = useSearchParams();
  let search = searchParams.get('search');
  let jobstatus = searchParams.get('jobstatus') || 'all';
  console.log(search,jobstatus);


  function handleSubmit(e){
    let formData = new FormData(e.currentTarget);
    let search = formData.get('search');
    let jobstatus = formData.get('jobstatus');
    console.log(search, jobstatus);
  };

  return (
      <form onSubmit={handleSubmit}>
          <div>
            <input name='search' type='text'/>
            <input name='jobstatus' type='text'/>
            <button type='submit' className='btn btn-primary'>Search Job</button>
          </div>
      </form>
  );
};

```


- We can access the value of query from URL.

```js

import { useSearchParams } from 'next/navigation';

export function getJobList(){
  
  let searchParams = useSearchParams();
  let search = searchParams.get('search');
  let jobstatus = searchParams.get('jobstatus') || 'all';
  
  console.log(search, jobstatus);
}
```


### COUNT NUMBER OF ROWS IN OUR MODEL FOR STATS.

- When we are analysing or representing stats and charts, we to count all the rows to analyze the numbers.
- Here we will use the 'count' method in prisma to count the number of rows on the basis of given condition.

```js

async function getStatsAction(){
  let userId = getClerkId();
  try {
    let stats = await prisma.jobs.groupBy({
      where:{
        clerkId:userId,
      },
      // 'by' method will decide for which row we are calculating the count and descriminate as we want.
      by:['status'],
      _count:{
        status:true
      }
    });

    // HERE WE HAVE CREATED AN OBJECT FOR JOBS COUNT AND JOB STATUS.
    let jobs = {};
    let data = stats.map((job)=>{
      jobs = {
        count:job._count.status,
        status:job.status,
      };
      return jobs;
    })
    console.log(data);
    return data;
  } 
  catch (error) {
    console.log(error);
    return null;
      
  }
};


// THIS WILL BE THE OUTPUT FOR THE ABOVE FUNCTION WHERE WE ARE CALCULATING THE TOTAL INPUT ENTRIES.
// [
//   { _count: { status: 33 }, status: 'declined' },
//   { _count: { status: 41 }, status: 'interview' },
//   { _count: { status: 29 }, status: 'pending' }
// ]


```


### GET THE COLUMNS AND ITS COUNT ON THE BASIS OF CONDITION.

- To create an charts we need counts for some specific columns.

```js

async function getChartsAction(){
  let userId = getClerkId();
  let date = dayjs().subtract(6,'month').toDate();
  try {
    let charts = await prisma.jobs.findMany({
      where:{
        clerkId:userId,
        createdAt:{
          gte:date,
        },
      },
      orderBy:{
        createdAt:'asc',
      }
    });
    
    let applicationsPerMonth = charts.reduce((acc, job) => {
      const date = dayjs(job.createdAt).format('MMM YY');

      const existingEntry = acc.find((entry) => entry.date === date);

      if (existingEntry) {
        existingEntry.JobsApplied += 1;
      } 
      else {
        acc.push({ date, JobsApplied: 1 });
      }

      return acc;
    }, [] as Array<{ date: string; JobsApplied: number }>);
    

    // console.log(applicationsPerMonth);
    return applicationsPerMonth;
  } 
  catch (error) {
    console.log(error);
    return null; 
  }
};

```
