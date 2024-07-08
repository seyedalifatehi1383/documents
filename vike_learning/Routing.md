
## [[Vike]] choice option! 
1. server Routing and client Routing
2. filesystem Routing or String Routing or Function Routing


## Server Routing!
1. This is old way of doing routing
2. In this way when you navigate to new page the old page discard and new HTML page will load 
3. The benefit of this way is its simple architecture 

## Client Routing!
1. implementing page navigation on client side 
2. when you navigate to new page instead of discard old page and load new page , new page will render by manipulate the DOM of old page 
3. The benefit of this way : faster and preserved state across navigation

## Filesystem Routing!
1. by default Vike is using this way and this way works with the path of the  +Page.tsx  file
2. notice : 'index/' and 'pages/' and 'src/' are ignored for  we can see some example of file system routing blow
3. Parameter Routing :  in parameterized routing you can make directory that start with **@** like /pages/todo/@id/+page.tsx has route like /todo/1
4. Group : we can group our route for organize your project by parentheses that ignored in route 
```
FILESYSTEM                      URL
========================        =================================
pages/about/+Page.tsx            /about
pages/faq/+Page.tsx              /faq
 
# index/ is mapped to the empty string
pages/index/+Page.tsx            /
 
# Parameterized route
pages/movie/@id/+Page.tsx        /movie/1, /movie/2, /movie/3, ...

# Group routing

pages/(group Name)/index/+page.tsx    /
pages/(group Name)/about/+page.tsx    /about
```

## String Routing

1. in This way you should create file in the directory of the page and  name it **+route.ts** and export your path that you want as string 
2. when you use /product/* mean every thing after product like /product/example/example/.... but /product is not excepted and 
3. use /product* for accept /product and other like /product/example/....
4. in segment 2 and 3 you can get value in pageContext.routeParams['*'] 
5. params that passed by URL is available at pageContext.routeParams.name for example pageContext.routeParams.id

```ts
//./page/directory/+route.ts
// Route String
export default '/product'
```

2. for passing data in String routing you can pass data by @ in route for example @id for pass id
```ts
// ./page/directory/+route.ts
// Route String by pass data

export default '/product/@Id'
```


### Globs Example :
```ts
// /pages/product/+route.js
export default '/product/*'      //route that make /product/12 /product/nested/12

// but /product  is not accepted

export default '/product*'  // In this case all route in top accepted and /product also accepted
```
#### notice that if use like this all route show one page

```ts
// /pages/catch-all/+route.js 
// Route all URLs to a single page
export default '*'
```

#### all URL show +Page file in catch-all directory


## Route Function

1. in this way you have more control and flexibility programing 
2. In file at /directory/+route.ts you can implement logic 

```ts

import { PageContextServer } from "vike/types";

import partRegex from "part-regex";

export {route}

const route = (pageContex : PageContextServer) => {
	//give patern that is /route-function/<characters>
    let regex = new RegExp(`/route-function/[a-z]`,"i")
	//check if /route-function/<character> return false and accept only number
    if (regex.test(pageContex.urlPathname)) {

        return false

    }
	//splite url and get id 
    const id = pageContex.urlPathname.split('/')[1]

  

    return {
		//the id is available at pageContex.routeparams.id
        routeParams : {id}

    }

}
```

### notice that route Function will complete