## Data Fetching in [[Vike]] :
1. create +data.ts file in directory that you want 
2. In this file fetch data and export data function
3. see the simple example :
```ts

import fetch from "cross-fetch";

import { Product } from "./type";

export {data}

  

async function data() {

    const responsse = await fetch('https://fakestoreapi.com/products')

    let product : Product[]

    product = await responsse.json()

  

    return {

        product,

    }

}
```

4. and you can use it in anther component by useData() like this :
```tsx
import { useData } from "vike-react/useData";

import { Product } from "./type";

export default function Page() {

    let data : {product : Product[]}

    data = useData()

    const {product} = data    

    return <>

        <h1>this is product page</h1>

        {

            product.map((p) => (

                <div>{p.title}</div>

            ))

        }

    </>

}
```

## Data Fetching by id :
1. For fetch by id that come from URL we use pageContex.routeParams.id
2. other things is like previous example
3. The code comes in blow
```ts
import { PageContextServer } from "vike/types";

import fetch from "cross-fetch";

import { Product } from "../type";

export {data}

async function data(pageContex : PageContextServer) {

    let product : Product

    const responsse = await fetch(`https://fakestoreapi.com/products/${pageContex.routeParams.id}`)    

    product =await

    responsse.json()

    return {

        product : product

    }

}
```

4. and you can use it by this code
```tsx
import { useData } from "vike-react/useData";

import { Product } from "../type";

export default function Page() {

    let data : {product : Product}

    data = useData()

    const {product} =  data

    return<>

    <h1>Show one product</h1>

    <h3>{product.title}</h3>

    </>

}
```