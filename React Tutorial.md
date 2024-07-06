## Introduction
React is a JS library (not framework) that is used to build and arrange user interfaces for web applications. React uses a syntax extension of JavaScript known as JSX meaning JavaScript XML.
React utilizes a virtual DOM that is a lightweight version of the real DOM of the web page that only update the changed part of the web page.

### What is DOM?
The **Document Object Model (DOM)** is a programming interface for web documents. It represents the structure of a web document (like an HTML or XML document) as a tree of objects. Each object represents a part of the document, such as an element, attribute, or piece of text.

## Single Page Applications (SPAs)

 In these applications, the server only needs to send a single html page to the browser for the website to run fully and then react takes over and manages the whole website in the browsing. (e.g. any kind of website data or user interactivity (such as click events and so on))
 For example users can navigate from page to page by click links in the website but those new pages are not sent to the browser from the server. Instead, react changes all the content in the browser depending on the route of the URL of the link that the user clicks. For example if a user clicks on a contact link then react will look at that forward `/contact` and then maybe inject a contact form onto the page so this process of react doing all of the work in the browser. (not like a link that when we click on contact button then a new link we be opened and it slows the speed of the website)
#### benefits:
1- the new pages are loaded in really quickly and it results in a very speedy user experience.
#### disadvantages:
1- This type of websites doesn't have any SEO because the htmls don't render from the server (unlike SSR).

## Start React
for create react project you can use this command (I recommend you to use `pnpm` package manager):
```shell
pnpm create vite@latest
```
Vite is a development server. It's a modern replacement to create react app which is now outdated.
A development server is a tool used in software development to provide a local environment for developers to build, test, and debug their applications.

`latest` means the latest version of Vite.

when we create the project, we should go to our made project and run these commands:
```shell
pnpm install #for installing node modules
pnpm run dev #for running the project
```

### What are the files and folders in the project?
let's introduce them:
* **node_modules** folder: This folder contains libraries and packages that our project relies on.
* **public** folder: The public folder contains any public assets (like fonts, images, etc.) and it can be accessible via a URL.
* **`src`** folder: We spend 99% of our time in the source folder. We have some files and folders in this folder and I want to explain them. 
	- **assets** folder:  We have **assets** folder that contain images and videos. It is very similar to the public folder but files within the assets folder are bundled during the final output. Public assets are not and they are generally available via a URL.
	- **`main.jsx`** or **`main.tsx`** file: It is the main JS or TS file.
	- **`App.jsx`** or **`App.tsx`** file: It is our root component in the start project.
	- **`index.css`**: This is the the main CSS for our application. We import this file from our `main.tsx` file.
	- **`App.css`**`: This is the CSS for App component.
- **`index.html`** file: This is the main entry point into our program.
- **`package.json`** file: `Json` files are structured in key value pairs. This file contains metadata about our project.

### Component syntax in react
### Components
First of all, let's introduce the components.
Component is a small section of code that can include TS or JS and HTML. Our function will return that code and it can be reusable.

we have several syntax for functions in components but I use this syntax for my components (these files are in `src` folder):
`Header.tsx`
```Tsx
// we use export default to use this function in our entire 
export default function Header({title}: {title: string}) {
	return(
		<header>
			<h1> {title} </h1>
		</header>
	);
}
```

`App.tsx`
```tsx
import Header from "./Header.tsx"

// this is the root component
export default function App() {
	return (
		<Header title="header"/>
	);
};
```
_tip 1_: We only have the capability of returning a single component or a single HTML tag. For example in the code above, I just return footer HTML tag and I used other tags into this tag.  
_tip 2_: The name of the components' files should be in `PascalCase` format.
_tip 3_: We can write components with an opening tag (`<Header />`) and a closing tag (`<Header></Header>`).

You can also see in `Header.tsx` file above that I defined a variable called title and I evaluated it in App component (passing a variable from father to son) so we should enclose them with a blank open-ended tag (`<> </>` (we call it a converter)) like this:
`App.tsx`
```tsx
import Header from "./Header.tsx"
import Footer from "./Footer.tsx"

// this is the root component
export default function App() {
	return (
		<>
			<Header title="header"/>
			<Footer />
			<Footer />
		</>
	);
};
```
_tip_: We can use components more than once (I just use Footer twice in this case).

And also we can use JS or TS code in the return statement of our component using  curly braces (`{}`) but outside the return statement we don't need `{}`.
`Footer.tsx`
```tsx
export default function Footer() {
	const date = new Date().getFullYear();
	
	return(
		<footer>
			<p> &copy; {date} copyright </p>
		</footer>
	);
}
```

In react, we use `className` instead of `class` (they are same and just the name has changed).
```HTML
<div className="container"> div tag </div>
```

_tip_: You can get placeholder photos from this URL:
```html
<img src="https://via.placeholder.com/<size-in-pixel>" alt="placeholder" />
```

## Types of writing CSS in React
we have 3 types:
1. EXTERNAL
2. MODULES
3. INLINE

EXTERNAL:
`Button.tsx`
```tsx
export default function Button() {
	return(
		<button className="button"> click me </button>
	);
}
```

`index.css`
```css
.button {
	border: 1px;
}
```

MODULES:
Make a folder that is named `Button`.
```
src
|___
	Button
	|___
		Button.tsx
		Button.module.css
```

(The context of `Button.module.css` is like `index.css` file.)

`Button.tsx`
```tsx
import styles from './Button.module.css' 

export default function Button() {
	return(
		// We dont need to worry about the name of class because
		// the names are generated by hashing.
		<button className={styles.button}> click me </button>
	);
}
```
The disadvantage of this type is that if we want to use a style globally, we should import it and it can be hard to use and also it requires additional setup.

INLINE:
Some people create a TS variable and insert styles into it. The name of the properties should be `camelCase` and the their values should be in string format (should be between `""` or `''`) and they are seperated with `,`.
`Button.tsx`
```tsx
import React from 'react';

export default function Button() {
	const styles: React.CSSProperties = {
		backgroundColor: "white",
		color: 'rgb(120, 100, 130)'
	}
	
	return (
		<button style={styles}> Click me </button>
	);
}
```
or in jsx instead of tsx:
`Button.jsx`
```jsx
export default function Button() {
	const styles = {
		backgroundColor: "white",
		color: 'rgb(120, 100, 130)'
	}
	
	return (
		<button style={styles}> Click me </button>
	);
}
```
It is great for isolated components with minimal styling e.g. a subscribe button.

## Props
Props shorts for properties. They are read-only properties that are shared between components. A parent component can send data to a child component. 
In JSX:
`Student.jsx`
```jsx
export default function Student(props) {
return (
		<div>
			<p> name: {props.name} </p>
			<p> age: {props.age} </p>
			<p> isStudent: {props.isStudent} </p>
		</div>
	);
}
```
`App.jsx`
```jsx
import Student from "./Student";

// this is the root component
export default function App() {
	return (
		<Student name="name" age={30} isStudent={true} />
	);
};
```

In TSX (I personally define the type in a file that is called `types.ts` in `src` folder): 
`types.ts`
```tsx
export type Person = {
	name: string,
	age: number
	isStudent: boolean
}
```

`Student.tsx`
```tsx
import { Person } from "./types";

export default function Student({student}: {student: Person}) {
	return (
		<div>
			<p> name: {student.name && '5616'} </p>
			<p> age: {student.age} </p>
			<p> student: {student.isStudent ? "yes" : 'no'} </p>
		</div>
	);
}
```

`App.tsx`
```tsx
import Student from "./Student";

// this is the root component
export default function App() {
	return (
		<Student student={{name:'ali', age:10, isStudent: true}} />
	);
}
```

