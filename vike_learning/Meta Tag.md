
## General Meta tag in [[Vike]] :
1. In this way you can set an default meta tag to your pages 
2. first create file -> /pages/Head.tsx
3. write content like blow :
```tsx
export {Head}

function Head() {

    return <>

        <title>Mohammad Vike</title>

        <meta name="description" content="this is test project of viek" />

    </>

}
```

## Meta tag for special page :
1. You should create head file --> /directoryName/+Head.tsx
2. and like previous example add meta tags
```tsx
export {Head}

  

function Head() {

    return<>

        <meta name="description" content="this page show all product " />

        <title>Product | محصولات</title>

    </>

}
```


## special Meta tags by fetch
1. you can have have unique meta tag by fetch data for each page 
2. for this create file -> /directory-name/+Head.tsx
3. also you should create file for fetch data in this directory -> /directory-name/+data.ts
4. now you can code in each file like blow example

```ts
// this file is for fetch data in /directory-name/+data.ts

//import type of pageContex that use for get route params
import { PageContextServer } from "vike/types";

//import fetch library for fetch data 
import fetch from "cross-fetch";

//import type from file in my project that denote my types
import { Product } from "../type";

//exprot function for vike that can uses
export {data}

async function data(pageContex : PageContextServer) {
	//get a variable by type that I denote 
    let product : Product
	//fetch data from server by fetch library 
    const responsse = await fetch(`https://fakestoreapi.com/products/${pageContex.routeParams.id}`)    

    product =await responsse.json()
	//retrun my data in an object 
    return {product}
```


```ts
//this is file that I denote my types 
export type Product = {

    id : number,

    title : string,

    price : number,

    description : string,

    category : string,

    image : string,

    rating : {

        rate : number,

        count : number

    }

}
```

```tsx
//this is Head file that show meta tags

export {Head}

import { useData } from "vike-react/useData";

import { Product } from "../type";

//this meta should fill by data that come from server

function Head() {

    const data = useData() as {product : Product}

    const metas = data.product

    return<>

    <title>{metas.title}</title>

    <meta name="description" content={metas.description} />

    </>

}
```


## Title meta tags :
1. you can add title meta tag to your page by file in ->/directory-name/+title.ts
2. this file add title only to your page and if you want to add different title only to your pages you can use this instead of +Head.tsx file
3. in firs example fix title to each page
```ts
export const title = 'your title'
```