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