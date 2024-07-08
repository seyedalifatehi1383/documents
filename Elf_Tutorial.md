# Basic Leaning
##### In this section learn basic of elf and use it in react and in other section discuses about advance thing like `withEntities` and ....
## Installation 
### Elf support CLI and you can add it to your project by following command 

```sh
npx @ngneat/elf-cli install
```
## Define Interface 
### after installing you should add interface to your file like this

```ts
interface AuthProps {

  user: { id: string } | null;

}
```

## Create Store 
### You can initial your store by `createStore()` method . this method take two argument first is configuration object that specifics the name of your store like this `{name : 'something'}` . Second argument specific   feature that you want 

##### In this example we use Props but in other example use more feature
```ts
import { withProps , createStore } from "@ngneat/elf";
const authStore = createStore(  
// get the name of store in object
{ name: 'auth' }, 
// <AuthProps> specific  type and in ({user : {id : '1'}}) get initial value
withProps<AutProps>({user : {id : '1'}}) 
);
```

## accessible Value
### For using this store in react you should export and filter it by using `select()` and `pipe()` method like the example 

```ts
// this line export the user property in the state
export const user$ = authStore.pipe(select((state) => state.user));
```

## Use in React

### code that you see until this section is writes in a ts file and export an observable like you see in above code and you should use `useObservable()`  method to read values

```ts
import { useObservable } from '@ngneat/use-observable';
import {user$} from '<YOUR TS FILE PATH>'
  const [user] = useObservable(user$);
```

#### Notice : If you cant import `useObservable` you can install it by blow command

```sh
npm install @ngneat/use-observable
```

## Updating user value

#### For updating user first you should export a function that be accessible  in you `tsx` file and inside it use `storeName.update()`that get a callback function that return new immutable state that replace with previous state 

```ts
export function Update(user : AutProps['user']) {

    autStore.update((state) => {

        return {

            ...state,

            user : user

        }

    })

}
```
# Advance Learning

## Design Pattern 
### Design pattern that Elf get you make your code encapsulate and you can change your store without change your business logic . In Elf you have two choice for design patter that show blow

#### First design patter 
##### that we use in basic learning

```ts
import { createStore, withProps, select } from '@ngneat/elf';  
  
interface AuthProps {  
user: { id: string } | null;  
}  
  
const authStore = createStore(  
{ name: 'auth' },  
withProps<AuthProps>({ user: null })  
);  
  
export const user$ = authStore.pipe(select((state) => state.user));  
  
export function updateUser(user: AuthProps['user']) {  
authStore.update((state) => ({  
...state,  
user,  
}));  
}
```

#### Second design patter 
##### This design patter use **OOP** 

```ts
import { withProps , createStore, select } from "@ngneat/elf";
interface AutProps {

    user : {id : string} | null
}
const autStore = createStore(
    {name : 'auth'},

    withProps<AutProps>({user : {id : '1'}})
)
export class AutRepository {

  
  user$ = autStore.pipe(select((state) => state.user))
     
  Update(user : AutProps['user']) {

    autStore.update((state) => {

        return {
            ...state,

            user : user
        }

    })

}

}
```

##### Use second pattern in `tsx` file

```tsx
import { useObservable } from "@ngneat/use-observable";

import { AutRepository} from "./store/repository.use";

function App() {

  const Aut = new AutRepository()

  const [user] = useObservable(Aut.user$)

  return (

    <div>

      <h1>{user?.id}</h1>

      <button onClick={() => Aut.Update({id : '5'})}>change id</button>

    </div>

  );

}
```

## Define Entities in Store
#### Entities are like table in database and has some already-made operation like select , add , update and something else. You could use it same props but when you use `createStore()` add `withEntities<>()`

```ts
// import thing we need
import { createStore } from "@ngneat/elf";
import { selectAllEntities, withEntities } from "@ngneat/elf-entities";

// make interface for our object that want to store
interface Todo  {

    id : number,
    lable : string

}

// create store with Entities freatur (you can use more one feature)
const store = createStore(

    {name : 'todo'},

    withEntities<Todo>({initialValue : [{id :0 , lable : 'first todo'} , {id:1 , lable : 'second todo'}]})

)

```

## Queries 

#### With Queries we can read , update , set and delete data from our Entities In this section we say some most used queries in elf and you can see more queries in elf documentation 

### Select Queries 

##### With select queries you can read and access to data that saved in store and we talk about select `selectAllEntities` and  `selectAllEntitiesApply` in this section

#### `selectAllEntities()`
##### This method return all Entities in your store 
```ts
// exprot is optional and use when you want access out of file 
export const todos$ = store.pipe(selectAllEntities())
```

#### `selectAllEntitiesApply()`
##### In `selectAllEntitiesApply()`  you can select Todo with applying filter and map . notice that first filter apply after it map 
```ts
export const filterdTodo$ = store.pipe(selectAllEntitiesApply({

    filterEntity : (todo) => todo.lable === 'first todo',
    mapEntity : (todo) => todo.id

}))
```

### Set Queries
##### In set queries new data replace with old data 
```ts
store.update(setEntities(todos))
```

##### If you want access to this you should declare function and export it like this
```ts
export function SetTodos(todos : Todo[]) {
    store.update(setEntities(todos))
}
```

### Add Queries
#### In add queries you can add to your store

####  `addEntities()`

```ts
// add one todo
store.update(addEntities(todo))
// add two todo
store.update(addEntities([todo , todo]))
// add to begin of list
store.update(addEntities(todo  , {prepend : true}))
```

#### `addEntitiesFifo()`
##### In this way add by fifo strategy (first in first out)
```ts
todosStore.update(addEntitiesFifo([entity, entity]), { limit: 3 });
```

### Update Queries
#### Update queries edit a specific element 

#### `updateEntities()`

```ts
// lable is property that you want update in element by id that passed 
store.update(updateEntities(id , {lable : newLable}))
// update with arrow function 
store.update(updateEntities(id , (e) => ({...e , lable : newLable})))
// update one propety for several element by specific id
store.update(updateEntities([id , id] , {lable : 'hello'}))
```

#### `updateEntitiesByPredicate()`

###### In this case you can update element by specific condition

```ts
store.update(updateEntitiesByPredicate((state) => state.lable === oldLable,
									   {lable : newLable} ))
```

### Delete Queries
#### This queries delete one or more element , and sometimes by condition

####  `deleteEntities()`

```ts
  // delete one or more element by id
todosStore.update(deleteEntities(id));  
todosStore.update(deleteEntities([id, id]));
```

#### `deleteEntitiesByPredicate()`

```ts
// delete with candition 
todosStore.update(deleteEntitiesByPredicate((e) => e.lable === lable));
```

#### `deleteAllEntities()`

```ts
// delete all elements
todosStore.update(deleteAllEntities());
```